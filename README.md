# Problem-collection
## 1、docker强制映射
> 参考网址 http://blog.csdn.net/a751101884/article/details/53374940  
### 给运行中的容器添加映射端口
### 方法1
1. 获得容器IP
将container_name 换成实际环境中的容器名

   `docker inspect `container_name` | grep IPAddress`  
2. iptable转发端口

将容器的8000端口映射到Docker主机的8001端口

   `iptables -t nat -A DOCKER -p tcp --dport 8001 -j DNAT --to-destination 172.17.0.19:8000`
### 方法2
1. 提交一个运行中的容器为镜像

    `docker commit containerid foo/live`  
2. 运行镜像并添加端口

    `docker run -d -p 8000:80 foo/live /bin/bash`
