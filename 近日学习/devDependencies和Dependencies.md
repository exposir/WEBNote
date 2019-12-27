### devDependencies和Dependencies

- npm install module-name -save 自动把模块和版本号添加到dependencies部分,即**生产环境**
- npm install module-name -save-dev 自动把模块和版本号添加到devdependencies部分，即**开发环境**

当项目A发布到npm时，其他人npm install A时会一起下载A的dependencies，而A的devDependencies不会一起下载。对于项目A自己而言，npm install时都会下载，没什么区别。