---
title: webpack-react-ts环境
---
最近再看typescript的时候结合了官网的配置加上自己的一些配置实现了一个可以自动打包,可以去解析一些常规文件

接下来开始吧

```shell
mkdir proj
cd proj
```

```shell
mkdir src
```

```shell
npm init -y
```

```shell
npm install -g webpack
```

现在我们添加React和React-DOM以及它们的声明文件到`package.json`文件里做为依赖：

```shell
npm install --save react react-dom @types/react @types/react-dom
```

接下来，我们要添加开发时依赖[awesome-typescript-loader](https://www.npmjs.com/package/awesome-typescript-loader)和[source-map-loader](https://www.npmjs.com/package/source-map-loader)。

```shell
npm install --save-dev typescript awesome-typescript-loader source-map-loader
```

我们需要创建一个`tsconfig.json`文件，它包含了输入文件列表以及编译选项。 在工程根目录下新建文件 `tsconfig.json`文件，添加以下内容：

```shell
{
    "compilerOptions": {
        "outDir": "./dist/",
        "sourceMap": true,
        "noImplicitAny": true,
        "module": "commonjs",
        "target": "es6",
        "jsx": "react"
    },
    "include": [
        "./src/**/*"
    ]
}
```

在src目录建一个App.tsx,内容如下:

```tsx
import * as React from "react";

type Props = {
    compiler:String,
    framework:String,
}

export const App: React.FC<Props> = (props) => {
    return (
        <div className="home-wrap">
            <h1>Hello from {props.compiler} and {props.framework}!</h1>;
        </div>
    )
}
```

在src目录下建一个index.tsx,内容如下:

```tsx
import * as React from "react";
import * as ReactDOM from "react-dom";

import {App} from "./App";

ReactDOM.render(
    <App compiler="TypeScript" framework="React" />,
    document.getElementById("app")
);
```



在proj的根目录下面建一个index.html用来展示,内容如下:

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <title>Hello React!</title>
    </head>
    <body>
        <div id="app"></div>
        <script src="./node_modules/react/umd/react.development.js"></script>
        <script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
        <script src="./dist/bundle.js"></script>
    </body>
</html>
```



然后在proj文件里新建一个 webpack.config.js,用来配置

```js
module.exports = {
    // 入口
    entry: "./src/index.tsx",
    // 出口
    output: {
        filename: "bundle.js",
        path: __dirname + "/dist"
    },

    // 启用 sourcemaps 以调试 webpack 的输出
    devtool: "source-map",
    mode: 'development',
    resolve: {
        // Add '.ts' and '.tsx' as resolvable extensions.
        extensions: [".ts", ".tsx", ".js", ".json"]
    },

    module: {
        rules: [
            // 所有带有“.ts”或“.tsx”扩展名的文件都将由“awesome-typescript-loader”处理
            { test: /\.tsx?$/, loader: "awesome-typescript-loader" },

            // 所有输出 '.js' 文件都将包含由 'source-map-loader' 重新处理的所有源映射。
            { enforce: "pre", test: /\.js$/, loader: "source-map-loader" }
        ]
    },

    // 当导入一个路径匹配以下之一的模块时，只需
    // 假设存在相应的全局变量并改用它。
    // 这很重要，因为它允许我们避免捆绑我们所有的
    // 依赖项，允许浏览器在构建之间缓存这些库。
    externals: {
        "react": "React",
        "react-dom": "ReactDOM"
    }
};
```

执行：

```shell
webpack
```

这时候就运行成功了 ! ! !

不过远远不够

如果你想在你的文件里面写jsx后缀的或者是js后缀的,则需要去配置解析jsx和js,内容如下:

```shell
npm i babel-loader @babel/core @babel/preset-env @babel/preset-react -D
```

安装完了之后,在你的 webpack.config.js 中添加:

```js
rules: [
    // 解析jsx
    {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        use: {
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env', '@babel/preset-react']
            }
        }
    },
]
```

这时候该项目已经可以去解析,jsx,js,ts后缀的文件了,

如果你想要去样式 less,则需要添加:

```shell
npm i style-loader css-loader less less-loader -D
```

安装完之后,再去webpack.config.js里面配置:

```js
rules: [
	// 解析less css 文件
    {
        test: /\.(css|less)$/,
        // 这玩意儿是有顺序的
        use: ['style-loader', 'css-loader', 'less-loader']
    }
]
```

这时候就已经可以去解析css,less文件了

这时候你可能会发现,我每次写完之后都会去执行webpack打包之后再去运行index.html,特别的麻烦,这时候你可以在webpack.config.js里面去配置一个watch 如下:

```js
module.exports = {  
	// 监听 执行 webpack --watch
    watch: true,
    watchOptions: {
        // 不监听的文件或文件夹
        ignored: /node_modules/,
        // 监听到变化发生后会等300ms再去执行动作，防止文件更新太快导致重新编译频率太高  
        aggregateTimeout: 300,
        // 判断文件是否发生变化是通过不停的去询问系统指定文件有没有变化实现的
        poll: 1000
    },
}
```

好了,你去执行webpack 或者 webpack --watch 就不要再去管cmd了 ,保存之后,会自动去打包

动手去试试吧 ! ! !
