# CI/CD
本文记录一次持续集成、持续部署实施过程。
## 依赖

+ [docker](https://www.docker.com/)

+ [gitlab](https://about.gitlab.com/)

+ [jenkins](https://jenkins.io/)

![structure](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/structure.jpg)

## docker
[安装](https://github.com/wanmbv/docker_practice/blob/master/install/centos.md)

## gitlab and jenkins
安装docker后，gitlab和jenkins可以选择docker部署，本文采用[docker compose部署](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/docker-compose.yml)。

jenkins首次登陆密码可以通过下面的命令查看：

    docker logs jenkins
 
#### 登陆后根据提示安装所需插件（maven,git,ssh等）

#### 在全局配置中配置jdk和maven等

![全局配置](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE%E5%B7%A5%E5%85%B7.jpg)
本文是在docker宿主机的jdk和maven通过卷映射到了jenkins容器中。

#### 系统设置中配置远程主机

也就是docker宿主机的ssh相关配置，以便在jenkins中可以通过ssh远程操作docker宿主机build和run容器。

![系统设置](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E7%B3%BB%E7%BB%9F%E8%AE%BE%E7%BD%AE.jpg)

#### 创建项目

选择创建maven项目。

![create-project](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/create-project.jpg)

#### 项目配置

+ 源码管理

![源码配置](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E6%BA%90%E7%A0%81%E9%85%8D%E7%BD%AE.jpg)

+ 构建触发

![自动构建触发](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E8%87%AA%E5%8A%A8%E6%9E%84%E5%BB%BA%E8%A7%A6%E5%8F%91.jpg)

+ 远程构建

![远程构建](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E8%BF%9C%E7%A8%8B%E6%9E%84%E5%BB%BA.jpg)

## 问题
Q: 项目源码配置时，在gitlab中正确配置ssh public key后，发现在jenkins中还是无法远程clone gitlab上的project。
A: jenkins和gitlab都是在docker容器中部署的，且给gitlab服务还映射了hostname，虽然在外界可以通过宿主机ip+对应port访问服务，
   但其实这些都是通过宿主机netfilter完成的，jenkins和gitlab容器启动时，会被分配自己的ip地址，所以jenkins想远程clone gitlab上
   源码时，要么在jenkins hosts中配置gitlab的ip映射、要么通过gitlab真实ip clone。

Q: docker build自动构建后的project时，无法COPY或是ADD项目jar文件。
A: 原因是没有完全理解COPY或是ADD操作所对应的context，docker构建镜像时，会将docker上下文中的所有文件上传到镜像临时目录下，
   directory中的文件，会被提取放到和directory同级目录下。
