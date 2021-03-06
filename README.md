# baota-paner 宝塔面板
## dockerHub: 
https://hub.docker.com/r/chenzhuoqi/baota/

## How to run
```bash
docker run --rm -d -p 20:20 -p 21:21 -p 80:80 -p 443:443 -p 888:888 -p 8888:8888 -v $PWD/www:/www chenzhuoqi/baota:centos7.2-98617c88 bash /bootstrap.sh
# or
docker-compose up -d

```
## How to visit
url:http://yourip:8888
username:admin
password:64b0dfc8

## How to persist data
www 目录是宝塔初始化需要的目录,将其挂载到容器里面,数据持久化

---------
## How to build
此镜像是是基于centos,run起来,安装宝塔面板,commit成的镜像

> run centos镜像
```bash
# 宿主机
docker run -i -t -d -p 20:20 -p 21:21 -p 80:80 -p 443:443 -p 888:888 -p 8888:8888 -v $PWD/www:/www centos

docker ps

docker exec -it XXXXX bash

```
> 安装宝塔面板
```bash
# 容器
yum install -y initscripts
# 安装宝塔
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh

```
安装完了就可以看到账户名密码

```bash
Bt-Panel: http://xxxxxx:8888
username: admin
password: XXXXXX
```
停止宝塔服务,添加docker的启动脚本
```
service bt stop
# 更新www权限,不然不能拷贝文件
chmod -R 766 /www
# 增加启动脚本,sleep 10000000000000000000000000000000 是为了让docker容器一直活着
echo 'service bt start && sleep 10000000000000000000000000000000' > /bootstrap.sh
# 给启动脚本添加执行权限
chmod +x /bootstrap.sh
```
> commit成镜像

```bash
# 将面板的初始数据共享出来,启动的时候再挂载进去
docker cp XXX(containerId):/www .
# commit镜像
docker commit XXX(containerId) baota:XXXXXX(password)
```


