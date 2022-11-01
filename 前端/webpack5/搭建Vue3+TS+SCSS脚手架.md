# 搭建Vue3+TS+SCSS脚手架

## 初始化项目

```
yarn init -y
```

```
yarn add typescript webpack webpack-cli css-loader sass sass-loader vue-loader @vue/compiler-sfc @babel/core babel-loader @babel/preset-env @babel/preset-typescript core-js postcss-loader postcss-preset-env mini-css-extract-plugin html-webpack-plugin webpack-dev-server webpack-merge process -D
```

引入 vue框架 和 element-plus组件库
```
yarn add vue -S
```

```
npx tsc --init
```

新建`public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

新建`.browserslistrc`

```
> 1% #代表全球超过1%使用的浏览器
last 2 versions # 最新的 2 个主版本
not ie <= 8 #不包含 IE8 及更低的版本
```

新建`babel.config.js`

```javascript
module.exports = {
  presets: [
    ['@babel/preset-env', { // 配置转换语法
      useBuiltIns: 'usage', // 配置只转换在时代实际使用到的语法和填充api
      corejs: 3, // 使用版本为 3的corejs 来进行 polyfill
    }],
    ['@babel/preset-typescript', {
      allExtensions: true,        // 支持所有文件扩展名，必须配置，不然babel不会解析vue文件的ts，将导致报错
    }], // 用于解析 typescript
  ]
}
```

新建`postcss.config.js`

```javascript
module.exports = {
  plugins: [
    require('postcss-preset-env'),
  ]
}
```

新建`src/App.vue`

```vue
<script lang="ts" setup>
import { ref } from 'vue'

const msg = ref('Hello world!');

</script>

<template>
  <div class="example">{{ msg }}</div>
</template>


<style lang="scss">
.example {
  color: red;
}
</style>
```

新建`src/shims-vue.d.ts`声明文件

```typescript
declare module '*.vue' {
  import type { DefineComponent } from 'vue'
  const component: DefineComponent<{}, {}, any>
  export default component
}

declare module '*.svg'
declare module '*.png'
declare module '*.jpg'
declare module '*.jpeg'
declare module '*.gif'
declare module '*.bmp'
declare module '*.tiff'
```

修改`tsconfig.json`

将`compilerOptions`的`jsx`字段设置为`preserve`，用于在tempalte里面，typescript的智能提示。

将`compilerOptions`的`module`字段设置为`esnext`，使用ESM模块化规范，element-plus组件库的自动导入组件的插件才会生效。

将`compilerOptions`的`moduleResolution`字段设置为`node`，表示使用node模块解析规范，这样子在导入第三库的时候，typescript才会从node_modules找到模块，不然找不到模块就会报错。

将`compilerOptions`的`baseUrl`字段设置为`./`，声明当前项目的根路径，配置别名才知道项目的根路径，才能知道当前别名的完整路径。

将`compilerOptions`的`paths`字段设置为`{ "@/*": ["src/*"] }`，配置别名规则，这样子不需要每次都用绝对路径来导入模块。

将`compilerOptions`的`noEmit`字段设置为`true`，表示编译时只检查类型，不生成js文件，因为我们用的是babel来打包ts，babel没有类型检查。

新建`webpack.common.js`

```javascript
const path = require("path");
const { DefinePlugin } = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { VueLoaderPlugin } = require("vue-loader");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  entry: "./src/main.ts",
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
    new VueLoaderPlugin(),
    new DefinePlugin({
      __VUE_OPTIONS_API__: false,
      __VUE_PROD_DEVTOOLS__: true,
    }),
    new MiniCssExtractPlugin({
      filename: "[name].bundle.css",
    }),
  ],
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  resolve: {
    // 先尝试 ts 后缀的 TypeScript 源码文件
    extensions: [".ts", ".js"],
    alias: {
      "@": path.resolve(__dirname, "src"),
    },
  },
  optimization: {
    splitChunks: { // 将共同的依赖单独提取出来作为一个 bundle
      chunks: 'all',
    },
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: "css-loader",
            options: {
              importLoaders: 1,
            },
          },
          "postcss-loader",
        ],
      },
      {
        test: /\.scss$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: "css-loader",
            options: {
              importLoaders: 2,
            },
          },
          "postcss-loader",
          "sass-loader",
        ],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: "asset/resource", // webpack5 内置的模块
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/i,
        type: "asset/resource",
      },
      {
        test: /\.ts$/,
        exclude: /node_modules/,
        loader: "babel-loader",
      },
      {
        test: /\.vue$/,
        loader: "vue-loader",
      },
    ],
  },
};

```

新建`webpack.dev.js`

```javascript
const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'development',
   devtool: 'inline-source-map',
   devServer: {
     static: './dist',
   },
 });
```

新建`webpack.prod.js`

```javascript
const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'production',
 });
```

修改`pacakge.json`，添加`scripts`

```javascript
"scripts": {
  "dev": "webpack serve --config webpack.dev.js",
  "build": "tsc && webpack --config webpack.prod.js",
  "checkTypes": "tsc --watch"
},
```

## 自动按需引入 element-plus 组件

```
yarn add element-plus
```

```
yarn add unplugin-vue-components unplugin-auto-import 
```

修改`webpack.commmon.js`

```javascript
const path = require("path");
const { DefinePlugin } = require("webpack");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { VueLoaderPlugin } = require("vue-loader");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

const AutoImport = require('unplugin-auto-import/webpack')
const Components = require('unplugin-vue-components/webpack')
const { ElementPlusResolver } = require('unplugin-vue-components/resolvers')

module.exports = {
  entry: "./src/main.ts",
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
    }),
    new VueLoaderPlugin(),
    new DefinePlugin({
      __VUE_OPTIONS_API__: false,
      __VUE_PROD_DEVTOOLS__: true,
    }),
    new MiniCssExtractPlugin({
      filename: "[name].bundle.css",
    }),
    AutoImport({
      resolvers: [ElementPlusResolver()],
    }),
    Components({
      resolvers: [ElementPlusResolver()],
    }),
  ],
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  resolve: {
    // 先尝试 ts 后缀的 TypeScript 源码文件
    extensions: [".ts", ".js"],
    alias: {
      "@": path.resolve(__dirname, "src"),
    },
  },
  optimization: {
    splitChunks: { // 将共同的依赖单独提取出来作为一个 bundle
      chunks: 'all',
    },
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: "css-loader",
            options: {
              importLoaders: 1,
            },
          },
          "postcss-loader",
        ],
      },
      {
        test: /\.scss$/,
        use: [
          MiniCssExtractPlugin.loader,
          {
            loader: "css-loader",
            options: {
              importLoaders: 2,
            },
          },
          "postcss-loader",
          "sass-loader",
        ],
      },
      {
        test: /\.(png|svg|jpg|jpeg|gif)$/i,
        type: "asset/resource", // webpack5 内置的模块
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf)$/i,
        type: "asset/resource",
      },
      {
        test: /\.ts$/,
        // exclude: /node_modules/,
        loader: "babel-loader",
      },
      {
        test: /\.vue$/,
        loader: "vue-loader",
      },
    ],
  },
};

```

修改 `src/main.ts`

```javascript
import { createApp } from "vue";
import App from "./App.vue";

// 这是全局样式文件，必须要引入，否则会有样式丢失
import "element-plus/dist/index.css";

const app = createApp(App);

app.mount("#app");
```

修改`src/App.vue`

```vue
<script lang="ts" setup>
import { ref } from 'vue'
const msg = ref('Hello world!');

</script>

<template>
  <div>
    <div class="example">{{ msg }}</div>

    <el-row class="mb-4">
      <el-button>Default</el-button>
      <el-button type="primary">Primary</el-button>
      <el-button type="success">Success</el-button>
      <el-button type="info">Info</el-button>
      <el-button type="warning">Warning</el-button>
      <el-button type="danger">Danger</el-button>
      <el-button>中文</el-button> 
    </el-row>
  </div>

</template>


<style lang="scss">
.example {
  color: red;
}
</style>
```

## 遇到问题

在配置element-plus组件的时候，vscode提示没有智能提示，使用组件也报错。重装node_modules也无效。

最后发现是element-plus 2.2.3版本，竟然没有声明文件。
