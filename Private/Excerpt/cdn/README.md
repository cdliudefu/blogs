# CDN 加速原理

CDN 的全称是（Content Delivery Network），即内容分发网络。其目的是通过在现有的 Internet 中添加一层新的 CACHE（缓存）层，将网站的内容发布到最接近用户的网络”边缘“的节点，使用户可以就近取得所需的内容，提高用户访问网站的响应速度。

简单的说，CDN 的工作原理就是将您源站的资源缓存到位于全球各地的 CDN 节点上，用户请求资源时，就近返回节点上的缓存的资源，而不需要每个用户的请求都回您的源站获取，避免网络拥塞、缓解源站压力，保证用户访问资源的速度和体验。

使用了 CDN 缓存后的网站的访问过程如下：
1. 用户输入访问的域名，操作系统向 LocalDns 查询域名的 IP 地址；
2. LocalDns 向 ROOT DNS 查询域名的授权服务器（这里假设 LocalDns 缓存过期）；
3. ROOT DNS 将域名授权 dns 记录回应给 localDns；
4. LocalDns 得到域名的授权 dns 记录后，继续向域名授权 dns 查询域名的 IP 地址；
5. 域名授权 dns 查询域名记录后（一般是 CNAME），回应给 localDns；
6. LocalDns 拿到域名记录后，向智能调度 DNS 查询域名的 ip 地址；
7. 智能调度 DNS 根据一定的算法和策略（比如静态拓扑、容量等），将最适合的 CDN 节点 ip 地址回应给 LocalDns；
8. LocalDns 将得到的域名 IP 地址，回应给用户端；
9. 用户得到域名 ip 地址后，访问站点服务器；
10. CDN 节点服务器应答请求，将内容返回给客户端。（缓存服务器一方面在本地进行保存，以便以后使用，二方面将获取的数据返回给客户端，完成数据服务过程）

通过以上的分析我们可以得到，为了实现对普通用户透明(使用缓存后用户客户端无需进行任何设置)访问，需要使用DNS(域名解析)来引导用户来访问Cache服务器，以实现透明的加速服务. 由于用户访问网站的第一步就是域名解析,所以通过修改dns来引导用户访问是最简单有效的方式.