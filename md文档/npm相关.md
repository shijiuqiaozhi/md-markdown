## npx和npm区别

主要特点：

1、临时安装可执行依赖包，不用全局安装，不用担心长期的污染。

2、可以执行依赖包中的命令，安装完成自动运行。

3、自动加载node_modules中依赖包，不用指定$PATH。

4、可以指定node版本、命令的版本，解决了不同项目使用不同版本的命令的问题。

以上就是npx与npm的区别。



有些包需要用命令行调用，但不需要全局安装，这时npx就起作用了

比如npm i -S eslint

npx eslint 就可以直接调用了，没必要再使用npm i -g

## 运行npm run xxx时发生了什么

详细链接: [前端项目中运行 npm run xxx 的时候发生了什么？_mb5ff58fc86bda8的技术博客_51CTO博客](https://blog.51cto.com/u_15077533/4531157)

[当输入 npm run 后发生了什么 - 掘金 (juejin.cn)](https://juejin.cn/post/6971723285138505765)

简单表述: 会先去package.json的scripts里找对应的xxx对应的命令。例如npm run serve在vue中一般会去找对应的vue servexxx执行。

为什么不直接执行？ 因为没有 vue serve xxx这个命令。这个执行vue serve xxx也不是直接执行。实际上是

npm 会先在当前目录的 node_modules/.bin 查找要执行的程序，如果找到则运行；

没有找到则从全局的 node_modules/.bin 中查找，npm i -g xxx就是安装到到全局目录；

如果全局目录还是没找到，那么就从 path 环境变量中查找有没有其他同名的可执行程序。


