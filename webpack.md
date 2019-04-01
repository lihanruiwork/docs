把你的资源本身当做模块
注册加载器（loaders），加载器就是当你遇到某种文件的时候，对它做相应处理的一种插件
```json
{
  // 当你导入（import）一个a.ts文件时，系统将会用Typescript加载器去解析它
  test: /\.ts/,
  loader: 'typescript',
}
```
所有的loader返回的都是string，这样webpack最终可以把资源都包装成javascript模块
scss文件被loaders转换后
```javascript
export default 'body{font-size:12px}';
```
把所有文件都放在一个文件里，这样就保证不浪费我们的HTTP请求。
把这个文件引入到每一个页面。这就意味着渲染每一个页面的时候大部分时间都浪费在加载一大堆根本就没用到的资源上。
如果你不这么做，那么你很有可能得手动把这些资源引入到指定的页面
把 node_modules/.bin 加到你的PATH环境变量里，以避免每次都手动打 node_modules/.bin/webpack 
全局安装 webpack 则忽略此步骤 npm install webpack -g
entry会告诉webpack哪个文件是你的app的入口点, 依赖树的顶端
webpack --watch 来自动监视文件的变化，如果文件有发生变化，便会自动重新编译。
--save-dev : 项目开发过程中需要依赖的包，发布之后无需依赖的包，例如我们的babel，开发过程需要babel为我们把书写的es6转为es5，发布之后由于我们所有书写的es6代码都被转为es5，因此无需继续依赖；
--save : 项目发布后依然需要依赖的包，例如我们jquery；
loaders既可以有 include 也可以是 exclude
可以是字符串(string)，正则表达式(regex)，也可以是一个回调(callback)
SCSS通过第一个loader净化之后变成CSS，CSS通过管道（pipe）流向第二个loader，CSS再通过第二个loader又变成可以被 import 的STYLE模块等等等过程
```
{
  test: /\.scss/,
  loader: 'style-loader!css-loader!sass-loader',
}
```
代码拆分是webpack对我们去手动把Button导入需要的界面相关繁琐操作的解决方案
你的代码可以轻松拆分开到部分单独文件中，然后按需加载
```javascript
if (document.querySelectorAll('a').length) {
    require.ensure([], () => {
        const Button = require('./Components/Button').default;
        const button = new Button('google.com');
        button.render('a');
    });
}
```
plugin 跟 loader 不同，它不是对指定的文件执行一些操作，而是对所有文件进行处理
loader是转换，plugin可以做各种优化
```
if (production) {
    plugins = plugins.concat([
       // 生产环境的插件放在这里
    ]);
}
```
寻找类似的块和文件然后进行合并
根据模块在你的程序中使用的次数来优化模块
最小化所有最终资源的javascript代码
webpack提供很多插件(plugin)用来优化处理你的modules
给我们的打包后的资源加版本
output.filename 设置为 bundle.js,有几个变量可以在命名的时候用
清理我们之前的生产版本 clean-webpack-plugin
如果你拆分的代码块突然有了更多的依赖，那么这些拆分块将被移动到一个异步块，而不是被合并。
如果这些拆分块过于相似以至于不值得单独加载，它们将被合并
你仅仅需要配置一下，webpack就会用最好的方式自动优化你的应用程序。
一切都是全自动
