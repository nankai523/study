# Reactjs

## Git 
* win10已经集成了openssh，不用安装openssh
* win7下，需要先安装OpenSSH
<https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH>
* 打开终端允许ssh-keygen产生证书
* 在用户目录下.ssh文件夹下有证书
* 安装Git for window <https://gitforwindows.org/>
* 安装TortoiseGit客户端，集成到资源管理器，<https://tortoisegit.org/>,安装选择中间，请选择OpenSSH。
## nodejs安装
使用nvm（node version  mananger）来安装nodejs，可以进行nodejs版本管理。具体参见：<https://github.com/coreybutler/nvm-windows>
安装完nvm之后，以管理员身份启动命令行程序；执行：nvm install，安装当前最新版本nodejs。安装完后

## 常用命令 
* 罗列所有安装的nodejs：nvm ls 
* 查看当前nodejs版本：node -v
* 启用nodejs：nvm on 否则可能报nodejs没有找到错误。
* 安装最近版本的nodejs: nvm install latest
* 切换nodejs版本号： nvm use 【version】

## create-react-app
```
npx create-react-app my-app
npx create-react-app my-app --scripts-version=react-scripts-ts 版本更新可能较慢
npx create-react-app my-app --scripts-version=react-scripts-ts-antd
cd my-app
npm start
```
创建react-app 具体参见: 
<https://github.com/facebook/create-react-app> 

<https://facebook.github.io/create-react-app/docs/documentation-intro>
## 常见配置
* 在程序的根目录下增加 .eslintrc 文件，以便进行Lint检查
    ```
    {
        "extends": "react-app"
    }
    ```
* 在VSCODE中添加chrome插件，直接在vs中调试。运行F5，出现 launch.json 文件，配置如下：
    ```
    webpack中的。
    devtool: 'cheap-module-source-map',请选用这个，否则可能vscode会提示。未验证的断点
    lauanch.json中内容如下：

    {
        "version": "0.2.0",
        "configurations": [
            {
            "name": "Chrome",
            "type": "chrome",
            "request": "launch",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceRoot}/src",
            "sourceMapPathOverrides": {
                "webpack:///src/*": "${webRoot}/*"
            }
            }
        ]
    }
    ```
* 分析各个模块大小，
    ```
    npm install --save source-map-explorer
    in package.json add +
    "scripts": 
    {
        +"analyze": "source-map-explorer build/static/js/main.*",
        "start": "react-scripts start",
        "build": "react-scripts build",
        "test": "react-scripts test"
    }
    npm run build
    npm run analyze
    ```
* 添加typescript支持
    ```
    npm install --save typescript @types/node @types/react @types/react-dom @types/jest
    ```
* 添加react-router
    ```
    npm install --save react-router-dom
    ```
    参考文档：<https://reacttraining.com/react-router/web/example/basic>
* 添加 proxy 配置，新增src\setupProxy.js，开发阶段使用basic认证:
    ```
    const proxy = require('http-proxy-middleware');
	module.exports = function(app) {
	  app.use(proxy('/api', { 
	    target: 'http://localhost:8080/myapp', 
	    secure: false,
	    changeOrigin: true,
	    auth: 'name:password' 
	  }));
	};
    ```
* 如果使用typscript，可能出现setupProxy.js编译报错，需要在tsconfig.json中增加配置
	```
	"exclude": [
	    "src/setupProxy.js"
	],
	```
* npm命令
    ```
    1、npm init：这个命令用于创建一个package.json。
    2、npm update -g <package>：更新全局软件包。
    3、npm update -g：更新所有的全局软件包。
    4、npm outdated -g --depth=0：找出需要更新的包。
    ```
* 增加webpack最新版 <https://webpack.js.org/guides/getting-started/>
    ```
    npm install --save-dev webpack-cli
    "start": "webpack --mode development",
    "build": "webpack --mode production"
    ```
* cookie 和localStorage，sessionStorage存储
	```
	在 Html5 中新加入的 localStorage 特性，主要是用来作为本地存储使用的，
	解决了 cookie 存储空间不足的问题。
	cookie 中每条 cookie 的存储空间为 4K， localStorage 中一般浏览器支持的是 5M大小，
	在不同浏览器中 localStorage 会有所不同。
	优点：拓展了 cookie 的 4k 限制， 可以将第一次请求的数据直接存储到本地，
	但只有在高版本的浏览器中才支持。
	localStorage 与 sessionStorage 的唯一区别就是  localStorage 属于永久性存储，
	而sessionStorage 属于当会话结束的时候，sessionStorage 中的键值对就会被清空。
	```
* 保存滚动条位置 <https://reacttraining.com/react-router/web/guides/scroll-restoration>
	```
	First, ScrollRestoration would scroll the window up on navigation. 
	Second, it would use location.key to save the window 
	scroll position and the scroll positions of RestoredScroll components
	to sessionStorage. 
	Then, when ScrollRestoration or RestoredScroll components mount, 
	they could look up their position from sessionsStorage.
	```
* 利用classNames来动态控制css <https://github.com/JedWatson/classnames>
    ```
        classNames('foo', 'bar'); // => 'foo bar'
        classNames('foo', { bar: true }); // => 'foo bar'
        classNames({ 'foo-bar': true }); // => 'foo-bar'
        classNames({ 'foo-bar': false }); // => ''
        classNames({ foo: true }, { bar: true }); // => 'foo bar'
        classNames({ foo: true, bar: true }); // => 'foo bar'

        // lots of arguments of various types
        classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

        // other falsy values are just ignored
        classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'

        let buttonType = 'primary';
        classNames({ [`btn-${buttonType}`]: true });
        var btnClass = classNames('btn', this.props.className, {
        'btn-pressed': this.state.isPressed,
        'btn-over': !this.state.isPressed && this.state.isHovered
        });
    ```
* redux <https://redux-docs.netlify.com/introduction/examples>
* 代码检查 
    > Configuring TSLint <https://palantir.github.io/tslint/usage/configuration/>

    > Configuring ESLint <https://eslint.org/docs/user-guide/configuring/>

    > https://github.com/webpack-contrib/webpack-bundle-analyzer
## react doc
<https://reactjs.org/docs/getting-started.html>

## 安装yarn
* yarn
	```
	npm install -g yarn
	yarn -v
	
	
	# 初始化一个项目
	yarn init
	# 装包
	yarn add packagename
	yarn add packagename --dev
	# 更新包
	yarn upgrade packagename
	# 删除包
	yarn remove packagename
	# 安装所有包
	yarn
	yarn install
	# 发布包
	yarn publish
	# 查看包的缓存列表
	yarn cache list
	# 全局安装包 == npm -g
	yarn global
	```
