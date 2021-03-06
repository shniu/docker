

### Jenkin on Ubuntu 16.04

由于使用 Docker 启动 Jenkins 时，在 Jenkins 容器内部运行 docker 命令，报 `command not found`，一直无法解决，所以直接在操作系统上安装 Jenkins。

安装脚本：install_jenkins_on_ubuntu16.04.sh

- 安装完并启动 Jenkins 后，安装 blueocean 插件

blueocean plugin: https://jenkins.io/doc/book/blueocean/getting-started/

### Jenkins on Docker

- [使用Maven构建Java应用程序](https://jenkins.io/zh/doc/tutorials/build-a-java-app-with-maven/)


### Jenkins blueocean on Docker

- [docker-jenkins](https://github.com/shazChaudhry/docker-jenkins)

- [jenkins dockerfile custome](https://github.com/tomsun28/DockerFile/tree/master/jenkins-dockerUse)


### Jenkins CLI

```
# http://192.168.1.34:18080/cli/
wget http://192.168.1.34:18080/jnlpJars/jenkins-cli.jar
java -jar jenkins-cli.jar -s http://192.168.1.34:18080/ -auth admin:123456 help

# https://jenkins.io/redirect/cli
```

### Jenkins script

```
# http://192.168.1.34:18080/script
Jenkins.instance.pluginManager.plugins.each{
  plugin -> 
    println ("${plugin.getDisplayName()} (${plugin.getShortName()}): ${plugin.getVersion()}")
}
```

插件导入导出：https://github.com/mlabouardy/butler

### 总结及问题

- [Jenkins 的一些总结](Jenkins.md)
- [在实践 Jenkins 时遇到的一些问题集合](Jenkins-practiced-issues.md)
