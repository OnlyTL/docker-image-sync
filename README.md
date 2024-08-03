# docker-image-sync
使用 Github Action 同步Docker 镜像至腾讯云 Coding 制品库，解决国内拉取镜像失败问题

随着国内各 Docker 镜像仓库被下，现在拉个镜像真是一言难尽，通过fork 本仓库将镜像同步至腾讯云 Coding 制品库，来解决拉取 Docker 镜像失败问题。

# 1. 前提条件

## Coding 准备
- 注册腾讯云 Coding 账号 [CODING | 一站式软件研发管理平台](https://e.coding.net/login)
- 创建项目
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031257809.png)

- 创建完成后，进入项目，选择制品管理->制品仓库，创建一个制品仓库
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031304809.png)

- Docker 制品仓库创建完成后，点击操作指引，可以看到针对此制品库的推送和拉取命令，其中有两个需要关注的地方，一个是仓库地址，一个是命名空间，后续会用到
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031308487.png)

# 2. Github 相关配置

- 首先需要在 github fork 下面项目，觉得有用可以⭐,打开下面地址fork 到自己仓库
[GitHub - OnlyTL/docker-image-sync: 使用 Github Action 同步Docker 镜像至腾讯云 Coding 制品库，解决国内拉取镜像失败问题](https://github.com/OnlyTL/docker-image-sync)
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031311829.png)

- fork 完成后需要在自己仓库的setting中设置 4 个 变量
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031313581.png)


| Secret Name      | 说明                         | 来源                     |
| ---------------- | -------------------------- | ---------------------- |
| CODING_REGISTRY  | 腾讯云Coding 制品库仓库地址          | 制品库操作指引中查找，前文提到过       |
| CODING_NAMESPACE | 腾讯云Coding 制品库名称（项目名/制品库名称） | 制品库操作指引中查找，前文提到过       |
| CODING_USERNAME  | 腾讯云Coding 登录用户名            |                        |
| CODING_PASSWORD  | 腾讯云Coding 登录密码             | 也可以在操作指引中配置访问令牌，使用令牌也行 |
- 配置完成后，就可以同步镜像了
# 3. 同步及拉取
## 3.1 同步

- 回到 Github 项目代码中，点击images.txt ,编辑，输入需要同步的镜像

❗images.txt 文件中配置需要拉取的镜像，如下，表示需要同步mysql5.7和redis6.0两个镜像

```txt
mysql:5.7
redis:6.0
```

点击编辑
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031323234.png)

提交
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031324956.png)

![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031324225.png)

提交完成后，就可以在 Action 中看到正在同步了...
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031325264.png)

等待同步完成后，就可以通过 Coding 拉取使用了

## 3.2 拉取

等待 Github Action 同步完成后，就可以在 Coding 制品库中看到了
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031327319.png)

然后根据操作指引中的命令先进行登录
```shell
docker login -u <USERNAME> -p <PASSWORD> g-docker.pkg.coding.net
```

登录完成后，根据根据拉取命令拉取对应镜像对应版本就行了，也可以进入镜像中，右上角有操作指引，直接复制拉取命令也可以
![image.png](https://onlytl.oss-cn-chengdu.aliyuncs.com/images/202408031331208.png)

至此，可以拉取你想要的镜像了，当然，还是稍微有些许麻烦，但是好在能下下来了不是😎🚀
