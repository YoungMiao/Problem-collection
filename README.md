# Problem-collection
## 1、docker强制映射
> 参考网址 http://blog.csdn.net/a751101884/article/details/53374940  
### 给运行中的容器添加映射端口
### 方法1
1. 查看Container的映射的端口
   `docker port <container name or id>`
2. 获得容器IP
   `docker inspect <container name or id> | grep IPAddress`  
3. 用iptables查看容器映射情况
* `iptables -t nat -nvL`
   
* `iptables -t nat -nvL --line-number`       
4. iptable转发端口
将容器的8000端口映射到Docker主机的8001端口
   `iptables -t nat -A DOCKER -p tcp --dport 8001 -j DNAT --to-destination 172.17.0.19:8000`   
5. 保存iptables规则
   `iptables-save`
#### 注意：
* 172.17.0.19 是根据 `docker inspect <container name or id>| grep IPAddress` 的结果
* 端口映射完毕后，不能通过`docker port <container name or id>`查询到结果
* 可以通过`iptables -t nat -nvL | grep 172.17.0.19`查询 映射关系
### 方法2
1. 提交一个运行中的容器为镜像

    `docker commit containerid foo/live`  
2. 运行镜像并添加端口

    `docker run -d -p 8000:80 foo/live /bin/bash`
