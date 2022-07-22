# 2.1 NGINX

根据 w3techs（https://w3techs.com/technologies/overview/web\_server）的调查，截至2022年5月，NGINX 以 33.5% 的市场占有率排在Web服务器软件的第一位。

NGINX 非常流行且易于拓展，可以轻松实现反向代理和负载均衡，这使得NGINX天然就是构建API网关的理想平台。由NGINX官方推出的NGinx Plus就是一款商用的网关软件，基于NGINX的Kong和Apache APISIX 更是常见的网关软件。

> Kong（https://github.com/Kong/kong）和Apache APISIX（https://github.com/apache/apisix）非常相似，两者均基于 OpenResty（NGINX + Lua），且都是云原生的。它们目前是非常流行的开源网关软件。本书中所实现的网关软件，或多或少会参考 Kong 或 APISIX 的实现方式，通过了解网关的搭建过程，读者们也能对Kong或APISIX有更好的理解。

所以，本书选择在 NGINX 基础上搭建我们的网关软件。不熟悉 NGINX 的读者可以通过本章节快速掌握 NGINX 的基础知识。本书中所述的软件与工具，如无特殊说明，均安装部署于Linux系统。

<mark style="background-color:orange;">参考文章：http://nginx.org/en/docs/beginners\_guide.html</mark>
