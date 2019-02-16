
## Install Docker

- [Ubuntu install docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

```

sudo groupadd docker     # 添加docker用户组
sudo gpasswd -a $USER docker     # 将登陆用户加入到docker用户组中
newgrp docker     # 更新用户组
docker ps    # 测试docker命令是否可以使用sudo正常使用

```



