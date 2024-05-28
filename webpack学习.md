# webpack学习

## 概念

> 本质上，**webpack** 是一个用于现代 JavaScript 应用程序的静态模块打包工具。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个依赖图，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。

## loader

webpack 只能理解 JavaScript 和 JSON 文件，这是 webpack 开箱可用的自带能力。**loader** 让 webpack 能够去处理其他类型的文件，并将它们转换为有效模块，以供应用程序使用，以及被添加到依赖图中。

在更高层面，在 webpack 的配置中，**loader** 有两个属性：

1. `test` 属性，识别出哪些文件会被转换。

2. `use` 属性，定义出在进行转换时，应该使用哪个 loader。

**webpack.config.js**

```js
module.exports = {
  output: {
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.txt$/,
        use: 'raw-loader'
      }
    ]
  }
};
```

以上配置中，对一个单独的 module 对象定义了 `rules` 属性，里面包含两个必须属性：`test` 和 `use`。这告诉 webpack 编译器(compiler) 如下信息：

> “嘿，webpack 编译器，当你碰到「在 `require()`/`import` 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先 **use(使用)** `raw-loader` 转换一下。”

## 插件(plugin)

loader 用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。包括：打包优化，资源管理，注入环境变量。

想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。多数插件可以通过选项自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 `new` 操作符来创建一个插件实例。

**webpack.config.js**

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
};
```

在上面的示例中，`html-webpack-plugin` 为应用程序生成一个 HTML 文件，并自动将生成的所有 bundle 注入到此文件中。

## 流程

一般来说，我们通过 `npm run webpack` 命令来启动 webpack。当我们在命令行运行以上命令后，npm 会让命令行工具进入 `node_modules\.bin` 目录查找是否存在 `webpack` 或 `webpack.cmd` 文件，如果存在就执行，不存在就抛出错误。

我们来看下 `webpack.cmd` 的内容：

```
@ECHO off
GOTO start
:find_dp0
SET dp0=%~dp0
EXIT /b
:start
SETLOCAL
CALL :find_dp0

IF EXIST "%dp0%\node.exe" (
  SET "_prog=%dp0%\node.exe"
) ELSE (
  SET "_prog=node"
  SET PATHEXT=%PATHEXT:;.JS;=;%
)

endLocal & goto #_undefined_# 2>NUL || title %COMSPEC% & "%_prog%"  "%dp0%\..\webpack\bin\webpack.js" %*
```

总体逻辑就是先判断当前目录下是否存在 node 执行文件，无论存不存在，最后都是使用 node 去执行 `node_modules\webpack\bin\webpack.js`。

linux 下的启动脚本逻辑也基本一致。

首先 `webpack.js` 引用 `node_modules\webpack-cli\bin\cli.js`。

然后 `cli.js` 引用 `node_modules\webpack-cli\lib\bootstrap.js` 并执行 `runCLI` 方法。

接着 `runCLI` 引用 `node_modules\webpack-cli\lib\webpack-cli.js` 并执行 `run` 方法。

```js
async run(args, parseOptions) {
  ...
  const loadCommandByName = async (commandName, allowToInstall = false) => {
    const isBuildCommandUsed = isCommand(commandName, buildCommandOptions);
    const isWatchCommandUsed = isCommand(commandName, watchCommandOptions);
    if (isBuildCommandUsed || isWatchCommandUsed) {
      await this.makeCommand(isBuildCommandUsed ? buildCommandOptions : watchCommandOptions, async 
      () => {
        this.webpack = await this.loadWebpack();
        return this.getBuiltInOptions();
      }, async (entries, options) => {
        if (entries.length > 0) {
          options.entry = [...entries, ...(options.entry || [])];
        }
        await this.runWebpack(options, isWatchCommandUsed);
      });
    }
  };
  ...
  this.program.action(async (options, program) => {
    ...
    if (isKnownCommand(commandToRun)) {
      await loadCommandByName(commandToRun, true);
    }
    ...
    await this.program.parseAsync([commandToRun, ...commandOperands, ...unknown], {
      from: "user",
    });
  });
  await this.program.parseAsync(args, parseOptions);
}
```

```js
const WEBPACK_PACKAGE_IS_CUSTOM = !!process.env.WEBPACK_PACKAGE;
const WEBPACK_PACKAGE = WEBPACK_PACKAGE_IS_CUSTOM
  ? process.env.WEBPACK_PACKAGE
  : "webpack";
async loadWebpack(handleError = true) {
  return this.tryRequireThenImport(WEBPACK_PACKAGE, handleError);
}
```

```js
async runWebpack(options, isWatchCommand) {
  // eslint-disable-next-line prefer-const
  let compiler;
  ...
  compiler = await this.createCompiler(options, callback);
  if (!compiler) {
    return;
  }
  const isWatch = (compiler) => Boolean(this.isMultipleCompiler(compiler)
    ? compiler.compilers.some((compiler) => compiler.options.watch)
    : compiler.options.watch);
  if (isWatch(compiler) && this.needWatchStdin(compiler)) {
    process.stdin.on("end", () => {
      process.exit(0);
    });
    process.stdin.resume();
  }
}
```

```js
async createCompiler(options, callback) {
  let config = await this.loadConfig(options);
  config = await this.buildConfig(config, options);
  let compiler;
  try {
    compiler = this.webpack(config.options, callback
      ? (error, stats) => {
        if (error && this.isValidationError(error)) {
          this.logger.error(error.message);
          process.exit(2);
        }
        callback(error, stats);
      }
      : callback);
    // @ts-expect-error error type assertion
  }
  catch (error) {
    if (this.isValidationError(error)) {
      this.logger.error(error.message);
    }
    else {
      this.logger.error(error);
    }
    process.exit(2);
  }
  return compiler;
}
```

`webpack` 入口文件 `node_modules\webpack\lib\index.js`。

```js
const fn = lazyFunction(() => require("./webpack"));
module.exports = mergeExports(fn, {
});
```

引用 `node_modules\webpack\lib\webpack.js`。

```js
const webpack = /** @type {WebpackFunctionSingle & WebpackFunctionMulti} */ (
  /**
   * @param {WebpackOptions | (ReadonlyArray<WebpackOptions> & MultiCompilerOptions)} options options
   * @param {Callback<Stats> & Callback<MultiStats>=} callback callback
   * @returns {Compiler | MultiCompiler | null} Compiler or MultiCompiler
   */
  (options, callback) => {
    const create = () => {
      if (!asArray(options).every(webpackOptionsSchemaCheck)) {
        getValidateSchema()(webpackOptionsSchema, options);
        util.deprecate(
          () => {},
          "webpack bug: Pre-compiled schema reports error while real schema is happy. This has performance drawbacks.",
          "DEP_WEBPACK_PRE_COMPILED_SCHEMA_INVALID"
        )();
      }
      /** @type {MultiCompiler|Compiler} */
      let compiler;
      /** @type {boolean | undefined} */
      let watch = false;
      /** @type {WatchOptions|WatchOptions[]} */
      let watchOptions;
      if (Array.isArray(options)) {
        /** @type {MultiCompiler} */
        compiler = createMultiCompiler(
          options,
          /** @type {MultiCompilerOptions} */ (options)
        );
        watch = options.some(options => options.watch);
        watchOptions = options.map(options => options.watchOptions || {});
      } else {
        const webpackOptions = /** @type {WebpackOptions} */ (options);
        /** @type {Compiler} */
        compiler = createCompiler(webpackOptions);
        watch = webpackOptions.watch;
        watchOptions = webpackOptions.watchOptions || {};
      }
      return { compiler, watch, watchOptions };
    };
    if (callback) {
      try {
        const { compiler, watch, watchOptions } = create();
        if (watch) {
          compiler.watch(watchOptions, callback);
        } else {
          compiler.run((err, stats) => {
            compiler.close(err2 => {
              callback(
                err || err2,
                /** @type {options extends WebpackOptions ? Stats : MultiStats} */
                (stats)
              );
            });
          });
        }
        return compiler;
      } catch (err) {
        process.nextTick(() => callback(/** @type {Error} */ (err)));
        return null;
      }
    } else {
      const { compiler, watch } = create();
      if (watch) {
        util.deprecate(
          () => {},
          "A 'callback' argument needs to be provided to the 'webpack(options, callback)' function when the 'watch' option is set. There is no way to handle the 'watch' option without a callback.",
          "DEP_WEBPACK_WATCH_WITHOUT_CALLBACK"
        )();
      }
      return compiler;
    }
  }
);
```
