# CI/CD
本文记录一次持续集成、持续部署实施过程。
### 依赖

+ [docker](https://www.docker.com/)

+ [gitlab](https://about.gitlab.com/)

+ [jenkins](https://jenkins.io/)

![structure](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/structure.jpg)

### docker
[安装](https://github.com/wanmbv/docker_practice/blob/master/install/centos.md)

### gitlab and jenkins
安装docker后，gitlab和jenkins可以选择docker部署，本文采用[docker compose部署](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/docker-compose.yml)

jenkins首次登陆密码可以通过下面的命令查看

    docker logs jenkins
 
登陆后根据提示安装所需插件（maven,git,ssh等）

在全局配置中配置jdk和maven等

![全局配置](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E5%85%A8%E5%B1%80%E9%85%8D%E7%BD%AE%E5%B7%A5%E5%85%B7.jpg)
本文是在docker宿主机的jdk和maven通过卷映射到了jenkins容器中。

系统设置中配置远程主机，也就是docker宿主机的ssh相关配置，以便在jenkins中可以通过ssh远程操作docker宿主机build和run容器。

![系统设置](https://github.com/wanmbv/docker-gitlab-jenkins/blob/master/%E7%B3%BB%E7%BB%9F%E8%AE%BE%E7%BD%AE.jpg)
