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
## 2、Ubuntu python升级
1. 查看当前python

   `ls |grep python`
2. 升级python到3.5

    `update-alternatives --install /usr/bin/python python /usr/bin/python2 100`
    
    `update-alternatives --install /usr/bin/python python /usr/bin/python3 150`
 3. 随意切换python版本
 
   `sudo update-alternatives --config python`
 ## 3、安装nvidia-docker
 #### 1. ubuntu安装nvidia-docker
      # Install nvidia-docker and nvidia-docker-plugin
      wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
      sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb
      #Test nvidia-smi
      nvidia-docker run --rm nvidia/cuda nvidia-smi      
#### 2. 通用安装nvidia-docker
      # Install nvidia-docker and nvidia-docker-plugin
      wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1_amd64.tar.xz
      sudo tar --strip-components=1 -C /usr/bin -xvf /tmp/nvidia-docker*.tar.xz && rm /tmp/nvidia-docker*.tar.xz

      # Run nvidia-docker-plugin
      sudo -b nohup nvidia-docker-plugin > /tmp/nvidia-docker.log

      # Test nvidia-smi
      nvidia-docker run --rm nvidia/cuda nvidia-smi
 ##### 3.nvidia-docker相关错误
      错误：Error response from daemon: linux runtime spec devices: error gathering device information while adding custom device "/dev/nvidia-uvm-tools": lstat /dev/nvidia-uvm-tools: no such file or directory
      解决方法：`nohup nvidia-docker-plugin &`
