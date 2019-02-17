#### 基准压测：

| 名称 | QPS | 压测性能图 | 备注 | 链接 |
| :------ | :------ | :------ | :------ |
| swoole单进程模式 | 约6.4万 | [压测图](https://github.com/hanhyu/wlsh-doc/blob/master/swoole-base.png) | c1k | https://www.swoole.com/ |
| swoole多进程模式 | 约8.5万 | [压测图](https://github.com/hanhyu/wlsh-doc/blob/master/swoole-process.png) | c10k | https://www.swoole.com/ |
| golang的gf框架 | 约6.2万 | [压测图](https://github.com/hanhyu/wlsh-doc/blob/master/gf.png) | c10k  | https://goframe.org |
| c++的mongols网络库 | 约10万 | [压测图](https://github.com/hanhyu/wlsh-doc/blob/master/mongols.png) | c10k | https://mongols.hi-nginx.com |
| wlsh框架 | 约5.8万 | [压测图](https://github.com/hanhyu/wlsh-doc/blob/master/wlsh.png) | c10k | https://wlsh.site |

> 结论：使用wlsh即方便又在性能上不掉队。