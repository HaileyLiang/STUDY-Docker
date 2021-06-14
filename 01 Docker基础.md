
`Tianchi Documents: `  
https://dockerpractice.readthedocs.io/zh/latest/dockerai/?spm=5176.20850343.J_3678908510.1.417f28541khq0o

https://tianchi.aliyun.com/course/351/4119?spm=5176.20850343.J_3678908510.3.417f28541khq0o


`Github: `  
https://github.com/HaileyLiang/STUDY---SQL/branches


`Tianchi Lab Guide: `  
https://tianchi.aliyun.com/notebook-ai/detail?postId=127457


`Docker on Azure tutoral: `  
https://docs.microsoft.com/en-us/azure/docker/
https://docs.microsoft.com/en-us/azure/app-service/tutorial-python-postgresql-app?tabs=bash%2Cclone
https://docs.docker.com/engine/install/linux-postinstall/

## 1. Docker底层技术支持
- NameSpaces 用作进程之间的隔离
- Control Groups 用于资源控制，根据需求划分资源的core，memory，disk等
- Union File Systems（UFS） Container和Imdage的分层 ？

## 2. Docker基本概念
- Image：
  镜像是文件与metadata的集合；
  分层的；
  不同的镜像可以共享相同的层（layout）
  只读的；

- Container：
  容器是镜像的一个运行实例；
  通过image创建；
  在image的最后一层上面再添加一层；可以通过build命令把容器打包成我们自己需要的镜像；
  container可读写；
  image负责存储和分发，container负责运行；
  镜像启动后会行程一个容器，容器在计算机中是一个进程，这个进程对其他进程不可见。

- Repository
  镜像存储在repo中，各云厂商提供的镜像存取服务

## 3. Docker命令：
 
- **拉取镜像**  
  ``` docker pull hello-world:latest[docker 镜像地址：标签]```

- 运行镜像 ``` docker run hello-world```

- **运行镜像并进入容器**
  ``` docker run -it --rm ubuntu:18.04 bash```
  - docker run运行容器，后面如果只跟镜像，那么久执行镜像的默认命令然后退出
  - -itd 后台(-d)交互式(-i)终端(-t) -i交互式操作  -t终端
  - --rm容器退出后随之将其删除，默认情况下，为了排障需要，推出容器并不会立即删除 除非手动docker rm
  - ubuntu:18.04 指用ubuntu:18.04镜像为基础来启动容器
  - bash 放在镜像后面，希望有个交互式shell，exit推出

- 查看本地镜像，image ID是镜像的唯一标识
  ```linux
    docker images
  ```
- 查看运行中的容器
  ```linux
    docker ps //当前活跃container
    docker ps -a //所有运行中的container
  ```
- **进入运行中/后台运行的容器**，可以对镜像做修改，如安装python包
  ```
  docker exec -it [container ID] /bin/bash
  ```

- **保存修改**,但不建议使用这种方式，因为commit操作会把所有信息都保存下来，包括如ls操作，所以该方式可以作为保留现场的手段，通过修改dockerfile构建镜像
  ```
  docker commit [container ID] [ImagesName]:[MyVersion Tag]
  ```
  
- 打TAG
  ```
  docker tag [image]registry.cn-shanghai.aliyuncs.com/test/pytorch:myversion [tag]my_tmp_version:0.1
  ```
- 推送镜像到repo  //registry 注册表  // repositroy 仓库
  ```
  docker push registry.cn-shanghai.aliyuncs.com/test/pytorch:myversion
  ```

- 使用dockerfile构建镜像
  1. 在空文件下vim Dockerfile
  2. 文件中填入信息
      ``` 
     From [基础镜像]
     RUN <命令>
     ## 注意每run一次会在docker上新建一层，避免层过多 造成镜像膨胀过大 可以用&&连接命令，这样执行后，只会创建一层
     https://www.runoob.com/docker/docker-dockerfile.html
      ```
  3. 构建镜像，在Dokcerfile文件存放目录下执行构建动作
      ``` 
      docker build -t niginx:v3 .
      ## .表示本次执行的上下文路径
      ## 上下文路径是指docker在构建镜像，会将指定目录下所有内容打包发给docker引擎，如果未说明最后一个参数默认上下文路径是Dockerfile所在位置
      ## build命令参数: https://www.runoob.com/docker/docker-build-command.html?_t_t_t=0.1479063844308257
      ## tianchi base docker images: https://tianchi.aliyun.com/forum/postDetail?postId=67720
      ```
- 删除容器
  ``` docker rm [container id]```
  如果容器还在运行会删除失败，应先结束掉容器 ``` docker kill [container id] ```

- 删除镜像：
  ``` docker rmi registry.cn-shanghai.aliyuns.com/target:test ```








  


