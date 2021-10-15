## Webpack、Rollup、Parcel 对比

### Webpack 的特点

- **代码分割**：Webpack 有两种组件模块依赖的方式，同步和异步。异步依赖作为分割点，打包时会形成一个新的块。在优化了依赖树后，每一个异步区块都会作为一个文件被打包。
- **Loader**：Webpack 本身只能处理原生 `javascript` 模块，但是 loader 转换器可以将各种类型资源转化成 `javascript` 模块被 Webpack 处理。
- **插件系统**：Webpack 有一个功能丰富的插件系统。大多数内容功能都是基于这个插件系统运行的，还可以开发和使用开源的 Webpack 插件，来满足各式各样的需求

### Rollup 的特点

- **Tree-Shaking 优势**：也是 Rollup 推出的一大特点。通过对代码的静态分析，分析出冗余代码，打包时候删除。
- **ES6 模块打包支持**：不需要像 Webpack 一样需要 `bebal` 将 `import` 转换成 `Commonjs` 的 `require` 方式，极大地利用了 `ES6` 模块的优势。

### Parcel 的特点

- **零配置**：不需要像 Webpack 和 Rollup 一样通过配置文件去启动打包流程。
- **快速打包**：工作进程启用多核编译，并具有系统缓存，即使再重新启动后也可以快速构建。
- **自动转换**：内置了很多常见的转换和编译器例如 Babel，PostCSS，PostHTML。
- **代码拆分**：使用动态 `import()` 函数拆分输出包来实现引入时按需加载。
- **模块热替换**：开发过程中进行模块修改时，Parcel 会自动更新浏览器对应模块，无需配置。

### 对比优缺点

#### 对于配置方面

**Parcel 胜出**，因为它根本无需配置文件。只需要安装 Parcel 后然后编译，它开箱即用地为你完成一切工作。

Webpack 和 Rollup 都需要一个配置文件，用于指定入口、加载器、插件、转化工具、输出等。不过还是有点区别：

- Rollup 有 `import/export` 的 `node` 兼容库， Webpack 没有。
- Rollup 支持在配置中使用相对路径， Webpack 需要通过借助 `path.resolve` 或者 `path.join` 来实现。

Webpack 的配置可能会变得很复杂，但是支持第三方导入、图片导入、css 预处理等众多功能。

#### Tree Shaking

**Parcel 胜出**，它为 `ES6` 和 `CommonJS` 模块两种都支持 `Tree Shaking`，这点很重要，因为 npm 包大多仍在使用 `CommonJS` 进行模块化管理。

**Rollup 第二**，开箱即用，它会静态分析要打包的代码，并将所有未使用的内容删除。

Webpack 则需要做额外的工作来完成：

- 使用 `ES6` 中的模块化管理（`import` 和 `export`）
- 在 `package.json` 中设置 `SideEffects`
- 引入一个支持 `Tree Shaking` 功能的 `plugin` （例如 UglifyJSPlugin）

Rollup 和 Webpack 更加专注为 `ES6` 做 `Tree Shaking`，因为 `ES6` 的静态分析要容易的多。针对 `CommonJS` 的依赖，则需要借助插件来做 `Tree Shaking`。

#### 代码分割

**Webpack** 以最少的工作量和更快的加载时间在这方面成为赢家。 它提供了三种方法来启用代码拆分：

- 定义入口文件 -- 使用 `entry` 配置项手动拆分代码
- 引入了 `optimization.splitChunks`，Webpack4 之前是用的 `CommonsChunkPlugin`
- 动态导入 -- 在模块内部使用行内函数调用

在 `Rollup` 的代码拆分中，你的代码块自身就是标准的 `ES` 模块，使用浏览器内置的模块加载器，没有任何额外的开销，但是仍然能够充分利用 `Tree Shaking` 的功能。对于尚不支持 `ES` 模块的浏览器，你也可以用 `SystemJS` 或其它的 `AMD` 加载器。它是完全自动化的，并且代码重复为零。

Parcel 支持零配置代码拆分。它的代码拆分是通过使用动态 `import()` 函数语法建议规范来控制的，该建议类似于常规的 `import` 语句或者 `require` 函数，但是返回 `Promise`。这意味着该模块加载时异步的。

使用 `Rollup` 和 `Parcel` 而不是 `Webpack` 进行代码拆分看起来很有诱惑力，但都还有些不太成熟。

#### 实时重新加载

在开发阶段，修改代码后应用自动实时重新加载这点很必要。

Parcel 考虑的很周到，通过内置开发服务器，当你修改文件时，该服务器会自动重新打包你的应用程序。

使用 Rollup 时我们需要安装并配置 `rollup-plugin-server`，它提供了实时重新加载的功能。不过它需要另一个插件配合工作 `rollup-plugin-livereload` 。

**Webpack**，则只需要使用 `webpack-dev-server` 的插件，它提供了一个默认开启了实时重新加载功能的简单版开发服务器。

#### 模块热替换

实时重新加载 并不相同，模块热替换（`HMR`）通过在运行时自动更新浏览器中的模块而无需刷新整个页面，从而改善了开发体验。 在代码中进行少量更改时，可以保留应用程序状态，而不用重新加载页面。

**Webpack** 拥有自己的 `Web` 服务器叫做 `webpack-dev-server` ，通过它可以支持热模块替换。

Parcel 已经内置了对热模块替换的支持。

Rollup 也发布了一个插件 `rollup-plugin-hotreload` 来支持热加载。

### 总结

Parcel 适合构建一个简单的应用并让它快速运行起来。

Rollup 适合构建一个需要导入很少第三方库的类库。

Webpack 适合开发一个复杂的应用，需要代码拆分，需要使用静态资源文件。
