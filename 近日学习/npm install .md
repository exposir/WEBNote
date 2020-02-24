# npm isntall

## devDependencies 和 Dependencies

- npm install module-name -save 自动把模块和版本号添加到 dependencies 部分,即**生产环境**
- npm install module-name -save-dev 自动把模块和版本号添加到 devdependencies 部分，即**开发环境**

当项目 A 发布到 npm 时，其他人 npm install A 时会一起下载 A 的 dependencies，而 A 的 devDependencies 不会一起下载。对于项目 A 自己而言，npm install 时都会下载，没什么区别。

## npm install

- 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
- 可以通过 require() 来引入本地安装的包。

## npm install -g 全局安装

- 将安装包放在 /usr/local 下或者你 node 的安装目录。
- 可以直接在命令行里使用。

## npm install --save

- 会把 lodash 包安装到 node_modules 目录中
- 会在 package.json 的 dependencies 属性下添加 lodash
- 之后运行 npm install 命令时，会自动安装 lodash 到 node_modules 目录中
- 之后运行 npm install --production 或者注明 NODE_ENV 变量值为 production 时，会自动安装 lodash 到 node_modules 目录中

## npm install --save-dev

- 会把 msbuild 包安装到 node_modules 目录中
- 会在 package.json 的 devDependencies 属性下添加 lodash
- 之后运行 npm install 命令时，会自动安装 lodash 到 node_modules 目录中
- 之后运行 npm install --production 或者注明 NODE_ENV 变量值为 production 时，不会自动安装 lodash 到 node_modules 目录中
