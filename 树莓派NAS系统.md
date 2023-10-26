
# 研究背景
自己闲置 1T 斐讯的移动硬盘
树莓派4B 4G版本
网线
公网ipv4

由于最近 B站新番越来越少，翻墙也给了各种各样的限制，可以看番的途径越来越少。 因此萌生了 smb 进行存储番的想法。

实现提前准备，提前看的操作，而且全程无广告，实现自由。

我自己鼓捣的路线非常原始，现在简单介绍一下 一些踩坑的悲惨遭遇

一开始，只是简单进行smb分享，这个是我在华为的网上邻居发现的，发现可以把硬盘变成网上邻居， 然后发现 这个网上邻居叫 smb

这种操作，可以实现 多端访问视频，很方便。 

然后研究如何把番存到 smb中， 发现qb和Transmission都可以进行下载bt。

然后我发现，我直接安装的qb 版本比较老，没有rss功能，就参照docker 直接在docker中拉取最新 支持rss订阅的qb


 备注：挂载硬盘的时候，开机的时候需要手动挂载一下硬盘;不能参照网上的教程 
 **当在/etc/fstab中，修改开机自启后 树莓派重启之后，直接死机**




## 公网申请
一开始申请，是直接找的营业厅的微信人员，直接说帮我后台申请，我一直以为申请成功了，结果发现，我 查询的 ip地址和 路由器上写的wlan 地址不是同一个地址
这才知道，我原来使用的不是公网 ，~~**解决办法： 找帮我装宽带的师傅，他直接去操作，最后，我重启一下光猫，就有公网地址了**~~
光猫桥接模式，我是直接搬家的时候，让师傅直接帮我把光猫改成桥接模式，然后我直接在路由器上进行拨号上网

~~改桥接， 公网ip的方式，在南京联通这边有个小漏洞就是：如果改好之后，只要路由器不重启，公网ip就不会变。就不用申请域名。~~

后续搬家的话，也要采用这种办法进行公网申请和操作

路由器也是一个值得折腾的点，后面买路由器的话，会优先考虑 可以刷固件的路由器，这样折腾起来很方便；  一回生，二回熟；其次是搞个带U盘接口的路由器。
这样 如果是在路由器上配置一些插件，这样比较方便，红米 RM2100 便宜，耐操。 后续的话，可以考虑升级wifi 升级到wifi6--可能wifi 7都快了。

1000M宽带，下载速度是真的快

## docker

安装docker容器，是我在安装qb的时候，无奈之举，发现直接安装的qb版本太老，因此考虑使用docker进行安装。
但是又发现，docker 并不适合进行容器配置等操作，启动之后 如果容器停止后，需要重新配置，这里引入compose 通过配置文件的方式，配置docker

> 总体而言，Docker 简化了应用程序的构建、部署和管理过程，提供了一种可靠和可移植的环境，
> 使得应用程序能够更快速、高效地运行。它已经广泛应用于各种领域，包括软件开发、微服务架构、持续集成/持续部署（CI/CD）等。
> 
> 
 有点类似 anconda，可以把Python文件进行配置和隔离起来，方便进行整理和开发。 他也是一个容器。方便进行配置。 但目前来看 docker 进行容器管理，比ancoda 效果更好

我采用docker compose 的方式进行配置镜像。

这样的好处，不会污染环境，而且方便固化配置，不用的时候，可以直接停止。

### docker compose

Docker Compose 是一个用于定义和运行多个 Docker 容器的工具。它允许您使用简单的 YAML 文件来定义多个服务、网络和卷，并通过一个命令一键启动、停止和管理这些容器。

以下是一般步骤使用 Docker Compose：

安装 Docker Compose：首先，确保已在 Linux 系统上安装了 Docker Compose。您可以通过官方文档提供的指南来安装它。

创建 Compose 文件：在您的项目根目录中创建一个名为 docker-compose.yml 的文件，该文件将包含您定义的服务、网络和卷的配置。

定义服务：在 Compose 文件中，您可以定义各个服务的名称、容器映像、端口、环境变量等。每个服务都可以在文件中以 YAML 格式进行定义。

构建和启动容器：使用 docker-compose up 命令在后台构建和启动定义的容器。如果指定了 --build 参数，它将针对 Dockerfile 构建新的镜像。

管理容器：通过 Docker Compose，您可以使用命令如 docker-compose stop、docker-compose start、docker-compose restart 等来管理已定义的容器。

清理容器：使用 docker-compose down 命令停止并删除所有相关的容器、网络和卷。

除了基本的操作外，Docker Compose 还提供了更多功能，如控制容器的依赖关系、扩展性、环境变量的管理等。

需要注意的是，使用 Docker Compose 需要一定的 Docker 知识和经验，并且需要正确配置和管理容器和服务。建议参考 Docker 和 Docker Compose 的官方文档和教程以获得更详细的指导和了解。


#### yaml 配置

Docker Compose 使用 YAML 文件来定义 Docker 容器的配置和设置。YAML（YAML Ain't Markup Language）
是一种人类可读性强的数据序列化语言，它使用缩进和特定符号结构来表示数据和层次关系。以下是 Docker Compose YAML 文件的常见配置部分：

version：指定 Docker Compose 文件的版本。例如：

copy code
version: '3'
这表示使用 Docker Compose v3 的语法和功能。；

备注 需要看你的docker compose 文件的版本对应上的，我的是1.25 对应的就是2.1 版本

[docker版本对应compose版本参考链接](https://blog.csdn.net/whatday/article/details/108865782)

services：定义要运行的服务，每个服务都是一个独立的容器。例如：

```copy code

services:
  web:
    image: nginx:latest
    
```
这个示例中定义了一个名为 "web" 的服务，它将使用最新版本的 Nginx 镜像。

volumes：定义要使用的卷（volumes），用于在容器之间共享数据。例如：

```copy code
volumes:
  - ./data:/app/data
```
这个示例中定义了一个将本地 "./data" 目录挂载到容器的 "/app/data" 目录的卷。
这个./data 就是代表 同 docker-compose.yml文件位置下的data文件夹，映射到 容器的/app/date 路径下
（容器中，实际上会生成一个完整的路径结构，使用/app 就是容器的**根目录中的/app**。 
而 ./data 是以 docker-compose.yml 同路径作为参考的

这样就方便容器和实际的数据空间的通信


**这个容器的位置，就是运行docker-compose.yaml 的文件位置**，如果你想要把a文件 挂载到 此容器中，就需要写好

如果你不写好挂载，数据是无法写入到此文件夹中。

networks：定义要使用的网络，用于连接不同的容器。例如：

copy code
networks:
  mynetwork:
这个示例中定义了一个名为 "mynetwork" 的网络。

environment：指定服务所需的环境变量。例如：


下方 是 我部署的镜像配置文件，clash
```bash
version: '2.1'
services:
  clash:
    # ghcr.io/dreamacro/clash
    # ghcr.io/dreamacro/clash-premium
    # dreamacro/clash
    # dreamacro/clash-premium
    image: dreamacro/clash
    container_name: clash
    volumes:
      - ./config.yaml:/root/.config/clash/config.yaml
      - ./Country.mmdb:/root/.config/clash/Country.mmdb
      - ./ui:/ui # dashboard volume
    ports:
      - "7890:7890"
      - "7891:7891"
      - "9090:9090" # external controller (Restful API)
    # # TUN
    # cap_add:
    #   - NET_ADMIN
    # devices:
    #   - /dev/net/tun
    restart: unless-stopped
    network_mode: "bridge" # or "host" on Linux

```


下方是挂载BT的脚本

```bash
version: "2.1"
services:
  qbittorrent:
    image: linuxserver/qbittorrent
    container_name: qbittorrent
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Shanghai # 你的时区
      - UMASK_SET=022
      - WEBUI_PORT=8081 # 将此处修改成你欲使用的 WEB 管理平台端口
    volumes:
      - /home/pi/qBittorrent/config:/config # 绝对路径请修改为自己的config文件夹
      - /downloads:/config/downloads # 我是把在同路径下的downloads 挂载到容器里，容器内部对应的是config/download
      # 我只要在容器中，把文件存入到config/downloads中，这样，会直接把数据写入到 downloads中
    ports:
      # 要使用的映射下载端口与内部下载端口，可保持默认，安装完成后在管理页面仍然可以改成其他端口。
      - 6881:6881
      - 6881:6881/udp
      # 此处WEB UI 目标端口与内部端口务必保证相同，见问题1
      - 8081:8081
    restart: unless-stopped


```

#### 进入容器内部
    sudo docker exec -it jupyter /bin/bash 
jupyter 是容器的名字
it 是代表进入容器内部
/bin/bash 代表的是 用什么终端进去


一般还是需要去看源码，才知道他怎么解读才是最合适的

执行这个命令的效果是:

检查当前是否有名为jupyter的容器正在运行

如果有,使用-it参数以交互模式进入该容器内部

分配一个伪终端并打开标准输入,给我们一个类似登录服务器的Shell交互环境

在容器内直接使用bash命令行进行操作,和本地一样

这种方式可以很方便地进入容器内部操作,比如查看日志、安装软件等



### 容器打包和重新引用
如果你在容器内部修改了部分设置，而且你无法通过直接引入config的方式 直接挂载容器设置进行直接配置。
那么就需要把这个镜像修改后，进行二次打包，打包后，docker-compose文件直接引用这个镜像。

下面以我使用 jupter docker 具体操作步骤演示。
我一开始 是直接使用这个配置文件

```shell
version: '2.1'
services:
  jellyfin:
    image: jupyter/scipy-notebook:latest
    restart: "unless-stopped"
    container_name: jupyter
    ports:
      - 8888:8888
    environment:
      - "JUPYTER_ENABLE_LAB=yes"
      - "RESTARTABLE=yes"
    volumes:
      - ./files:/home/jovyan/work


```

我发现这个容器 界面是英文，且没有我想要的requests包。
如果我想一直使用这个容器，不关机，直接进入容器内部，直接安装新的包也行，但是保不齐，容器被迫停止，容器重启，原来的这些配置就都没了。

所以考虑直接通过镜像打包的形式，后面直接docker-compose 直接拉取这个打包后修改的新镜像，这样就避免后面复杂的操作。

```shell
sudo docker exec -it jupyter /bin/bash 
# 安装各种包，这个就像终端一样配置和运行即可，这个终端的进入密码 已经被我自己初始化为admin
# 安装完成后 退出终端 exit
sudo docker commit b2b8db4f43b1 jupter_chinese
# b2b8db4f43b1 为原来容器的名称<docker ps 查询>  新容器为jupter_chinese
# 这个操作会直接会生成一个新的镜像，把你原来的设置都给打包好了
# 在docker-compose.yml 文件中 修改这个image 为 jupter_chinese 即可
```
重新拉取这个docker 即可完成运行

如果已经修改了docker-compose.yml文件，并且希望更新其中的某个容器，可以直接使用docker-compose up -d命令。该命令会更新docker-compose.yml中修改过的服务，并根据修改的设置进行重新配置

如果在docker-compose up -d命令中不指定service-name，则会默认寻找当前目录下的docker-compose.yml文件，并启动该文件中定义的所有服务。

如果在命令中指定了service-name，例如docker-compose up -d myservice，则只会启动指定的服务，并不会重新拉取其他镜像。这将忽略docker-compose.yml文件中定义的其他服务。

因此，如果你只想重新拉取某个特定的镜像或运行特定的服务，请务必确保指定正确的service-name。如果要重新拉取所有服务，应该使用docker-compose pull命令。



