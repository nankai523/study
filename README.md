# 日常学习记录备忘
## nodejs安装
使用nvm（node version  mananger）来安装nodejs，可以进行nodejs版本管理。具体参见：<https://github.com/coreybutler/nvm-windows>
安装完nvm之后，以管理员身份启动命令行程序；执行：nvm install，安装当前最新版本nodejs。安装完后

### 常用命令 
* 罗列所有安装的nodejs：nvm ls 
* 查看当前nodejs版本：node -v
* 安装最近版本的nodejs: nvm install latest
* 切换nodejs版本号： nvm use 【version】
-------
## create-react-app
```
npx create-react-app my-app
cd my-app
npm start
```
创建react-app 具体参见: 
<https://github.com/facebook/create-react-app> 

<https://facebook.github.io/create-react-app/docs/documentation-intro>
### 常见配置
* 在程序的根目录下增加 .eslintrc 文件，以便进行Lint检查
    ```
    {
        "extends": "react-app"
    }
    ```
* 在VSCODE中添加chrome插件，直接在vs中调试。运行F5，出现 launch.json 文件，配置如下：
    ```
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
## react doc
<https://reactjs.org/docs/getting-started.html>