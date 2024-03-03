# 项目介绍
## 使用该仓库可以很方便的调试 react 18.2 项目，在调试 react-read-source-demo 时，可以调试到 react 项目中具体的对应文件处
## 原理介绍
### vscode 中如何调试前端web代码
#### 配置 .vscode/lanch.json, 具体内容见项目。可以通过 Launch Chrome 在 vscode 中来调试前端代码。
### 修改 react/scripts/rollup/build.js 脚本，使其可以打包出 *.map 文件，有了 map 文件，在调试时，就可以找到对应的源代码
#### 具体修改内容
1. getRollupOutputOptions 打包配置项：sourceMap: true
2. 在打包时会出错，提示某些插件没有打包 sourceMap 的功能，我们把其注释掉就行
3. 将打包后的react相关的包路径改为react相关包的绝对地址，使其在 react-read-source-demo 运行时可以找到原来的源文件
```javascript
sourcemapPathTransform(r) {
      return r.replace('../../../../packages', '/Users/{xxx}/react-code-learning/react/packages')
    }
```
### 在 react-read-source-demo  中，我们不再打包 react 相关包
1. 将 react 打包出的以下文件拷到 react-read-source-demo 中的 public 文件夹下(react-dom.development.js、react.development.js 以及它们的 map 文件)
2. 在其 webpack 配置文件中声明 react 相关包不再打包
```json
    externals: {
      "react": "React",
      "react-dom": "ReactDOM"
    }
```
3. 修改 public/index.html，直接加在 public 下的 react 相关包
```html
    <script src="./react.development.js"></script>
    <script src="./react-dom.development.js"></script>
```
### 在 react-read-source-demo 中启动 npm run start，通过 Launch Chrome 就可以在 vscode 调试和阅读 react 代码了
## 参考：https://juejin.cn/post/7126501202866470949?searchId=20240222233632059687704B8E9E5DBF5C 每一步讲的更详细点
