## shopify cli econnreset error <!-- {docsify-ignore} -->

最近在学习shopify的项目，在使用终端搭建环境时运行shopify theme dev 后报错，出现 ``` econnreset; shopify client network socket disconnected before secure tls connection was established ``` 等网络错误。

遇到以上网络错误毫无疑问时Shopify的一些域名被墙了，需要科学上网。但发现开启代理后还是没能解决；我在Mac上用的小飞机开全局模式以及在终端执行export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087; 等命令还是会出现上述错误。

后来换成surge代理软件，设置为系统代理开启增强模式；此问题解决。

Windows的系统需要使用支持tun模式的代理软件

关于tun模式貌似就是开启一块儿虚拟网卡，所有流量都要走这个网卡代理；这样就避免了一些不听话的软件不走代理的情况。

解决该问题还有另外一种方式就是在路由器上做代理。