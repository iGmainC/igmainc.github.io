---
title: Docker创建docker用户组
tags: Docker Linux
---

安装 docker 后运行 docker 相关命令经常需要 root 权限，这个时候需要把用户添加到 docker 用户组

**1\. 创建docker用户组**
```shell
 sudo groupadd docker
```

**2\. 将当前用户加入docker用户组**
```shell
 sudo usermod -aG docker ${USER}
```

**3\. 更新用户组**
```shell
newgrp docker
```

**4\. 切换或者退出当前账户再从新登入**
```shell
su root             切换到root用户
su ${USER}          再切换到原来的应用用户以上配置才生效
```