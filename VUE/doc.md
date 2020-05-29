#### vue-cli4 vue项目架构说明文档
[VUE-CLI参考文档](https://cli.vuejs.org/zh/config/)
1. `public` （相当于vue-cli2中的static文件夹）
    - `index.html` 主页面 挂载vue组件
    - `favicon` 图标
    - `config.js` 全局配置文件---根据不同环境，配置不同信息
2. src
    - `assets` 盛放静态文件，比如imags、svgs、fonts等等
    - `common` 盛放公共方法文件
    - `components` 盛放公共组件
    - `request` 盛放接口文件
        - `http.js` api全局配置 用于处理请求拦截、相应拦截、错误信息处理等
        - `API` 盛放每个页面的接口定义
            - `index.js` api接口统一出口
    - `router` 路由
        - `index.js` 路由配置统一出口
    - `store` 状态管理
        - `index.js` 状态管理统一出口
        - `Modules` 模块 盛放各个页面的状态管理配置 包括state、mutations、actions
    - `views` 各页面
        - `Login->router.js` 每个页面单独对应的路由，最后被router文件夹下的index.js统一配置
3. `tests` 单元测试文件（暂时用不到）
4. `.browserslistrc` 配置浏览器兼容
5. `.editorconfig` 配置书写格式规则
6. `.eslintrc.js` 用来管理和检测js代码风格
7. `.gitignore` 用来指定不被push到git仓库的文件，换句话说，用来指定push到git仓库时被忽视的文件
8. `babel.config.js` babel配置
9. `CHANGELOG.md` 记录开发过程，比如某某某开发了哪些模块，修改了哪些bug,新添加了哪些功能，有助于团队协作
10. `default.conf` nginx服务配置文件
11. `Dockerfile` Docker镜像打包文件
12. `jest.config.js` 单元测试配置文件（暂时用不到）
13. `package.json` 定义了项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。npm install 命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境。
14. `package-lock.json` 锁定安装时的包的版本号，并且需要上传到git，以保证其他人在npm install时大家的依赖能保证一致
15. `README.md` 说明文档，一般简单介绍项目信息
16. `vue.config.js` 配置文件。取代了原来的build文件夹和config文件夹


 
