## Docker LNMP 1.0

Docker LNMP 构建出基于 Docker 的 PHP 开发环境，其优势有在短时间内随意构建不同版本的相关服务、环境统一分布在不同服务器等，使开发者能够更专注于开发业务本身。

##### 版本及组件

* 当前版本：2.0
* 自带组件：PHP/FPM 7.1、Nginx 1.12.1、mariadb 10.11、Redis 4.0.8

##### 目录结构

    docker-lnmp
    |----/build                  镜像构建目录
    |----/work                   持久化目录
    |--------/common/            公共组件库
    |------------/*              组件，包括了数据、配置文件、日志等持久化数据
    |-----------------/config    组件的配置目录
    |-----------------/log       组件的日志目录
    |--------/wwwroot            WEB 文件目录
    |----/.env-example           配置文件
    |----/docker-compose.yml     docker compose 配置文件

#### 开始安装


    cd ~/
    git clone https://github.com/imofei/docker-lnmp.git

    cd docker-lnmp
    cp .env-example .env

    # 配置应用目录、数据库密码、时区、端口等
    vim .env
    
    # 给组件文件夹可写权限（日志需要）
    sudo chmod -R 777 work/common

    # 构建镜像并启动容器
    sudo docker-compose up --build -d

启动成功访问 http://localhost 出现 安装成功页面

#### 可能遇到的问题

##### 常用操作命令

    # 查看当前启动的容器
    sudo docker ps
    
    # 启动部分服务在后边加服务名，不加表示启动所有，-d 表示在后台运行
    sudo docker-compose up [nginx|php|mysql|redis] -d
    
    # 停止和启动类似
    sudo docker-compose stop [nginx|php|mysql|redis]

    # 删除所有未运行的容器
    sudo docker rm $(docker ps -a -q)

    # 删除所有镜像，-f 可以强制删除
    sudo docker rmi $(docker images -q)

## License
MIT
