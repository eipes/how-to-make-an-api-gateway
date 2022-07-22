# 2.2 OpenResty

上节我们利用NGINX的 `upstream` 模块和 `proxy_pass` 指令，初步实现了一个网关的基础功能：反向代理和负载均衡。它作为一个最简单的API网关，完全可以部署在生产环境使用。但是，这样一个基础的网关，除了缺少流量控制，身份验证等高级功能外，最大的问题是NGINX更新麻烦。

当 upstream 中的源服务器，location 路由等配置项有更新时，我们必须要NGINX主进程发送 `reload` 信号，使得其使用最新的配置文件。这种频繁更新NGINX配置文件的行为增加了运维的成本。特别是，当我们将网关容器化后，每次更新文件都意味着要重新打包镜像文件，造成很大浪费。

一般情况下，网关软件会提供一个控制台，运维人员通过可视化的操作界面，编辑网关的配置信息。而这些配置信息将保存在数据库中。网关在启动时会将配置信息从数据库中读出，在后续工作过程中，通过定时查询或者数据库主动推送，网关可以实时获取最新的配置。通过引入数据库和控制台，运维人员可以在不更新网关服务的情况下，实现配置更新。这样做还有另外一个好处，就是运维人员只需要清楚怎样进行可视化配置即可，不必了解网关的技术细节，降低了网关软件的使用门槛。

<mark style="background-color:orange;">todo 主流网关的控制台。</mark>

以上的功能，特别是从数据库中读取和更新配置，仅靠原生的 NGINX 是较难实现的，像 Kong 和 APACHE APISIX 一样，我们求助于 OpenResty（https://github.com/openresty/openresty）。

OpenResty使用Lua语言对 NGINX 进行了拓展，它的核心组件是 `lua-nginx-module`（https://github.com/openresty/lua-nginx-module）。Lua是一种解释性的脚本语言，OpenResty将Lua语言的解释器嵌入到NGINX中，由此赋予了NGINX丰富的拓展能力。

除了嵌入到NGINX中，Lua也被用来拓展 Apache（mod\_lua模块） 和 Lighttpd（mod\_magnet模块） 等服务器软件。另外，C，Perl，JavaScript 等编程语言也被用来拓展NGINX，不过，当前最流行的还是 OpenResty 的方案。

关于 OpenResty 的使用，大家可以参阅电子书《OpenResty 最佳实践》（https://github.com/moonbingbing/openresty-best-practices）。本书就重点内容简要说明。
