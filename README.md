LVS 四种工作模式总结：
NAT 多目标的DNAT，四层，支持端口修改，请求报文和响应报文都要经过LVS
DR 默认模式，二层，只修改MAC，不支持端口修改，性能好，LVS负载比小，LVS和RS并在同一网段，请求
报文经过LVS，响应报文不经过LVS  （默认）
TUNNEL 三层，添加一个新的IP头，支持LVS和RS并在不在同一网段，不支持端口修改，请求报文经过
LVS，响应报文不经过LVS
FULLNAT 多目标的SNAT+DNAT，四层，支持端口修改，请求报文和响应报文都要经过LVS

 LVS 调度算法
ipvs scheduler：根据其调度时是否考虑各RS当前的负载状态
分为两种：静态方法和动态方法
1 静态方法
仅根据算法本身进行调度
1、轮询算法RR：roundrobin，轮询,较常用
2、加权轮询算法WRR：Weighted RR，较常用
3、源地址哈希算法SH：Source Hashing，实现session sticky，源IP地址hash；将来自于同一个IP地址
的请求始终发往第一次挑中的RS，从而实现会话绑定
NAT 多目标的DNAT，四层，支持端口修改，请求报文和响应报文都要经过LVS
DR 默认模式，二层，只修改MAC，不支持端口修改，性能好，LVS负载比小，LVS和RS并在同一网段，请求
报文经过LVS，响应报文不经过LVS
TUNNEL 三层，添加一个新的IP头，支持LVS和RS并在不在同一网段，不支持端口修改，请求报文经过
LVS，响应报文不经过LVS
FULLNAT 多目标的SNAT+DNAT，四层，支持端口修改，请求报文和响应报文都要经过LVS
4、目标地址哈希算法DH：Destination Hashing；目标地址哈希，第一次轮询调度至RS，后续将发往
同一个目标地址的请求始终转发至第一次挑中的RS，典型使用场景是正向代理缓存场景中的负载均衡,
如: Web缓存
2 动态方法
主要根据每RS当前的负载状态及调度算法进行调度Overhead=value 较小的RS将被调度
1、最少连接算法LC：least connections 适用于长连接应用
2、加权最少连接算法WLC：Weighted LC，默认调度方法,较常用
3、最短期望延迟算法SED：Shortest Expection Delay，初始连接高权重优先,只检查活动连接,而不考虑
非活动连接
4、最少队列算法NQ：Never Queue，第一轮均匀分配，后续SED
5、基于局部的最少连接算法LBLC：Locality-Based LC，动态的DH算法，使用场景：根据负载状态实现
正向代理,实现Web Cache等
6、带复制的基于局部的最少连接算法LBLCR：LBLC with Replication，带复制功能的LBLC，解决LBLC
负载不均衡问题，从负载重的复制到负载轻的RS,,实现Web Cache等

