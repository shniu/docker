
## Jenkins practiced issues

#### 1. line 1: syntax error: bad substitution

在做 env 的变量替换时需要注意：

```
environment {
  VERSION = '1.0.0'
}

...

steps {
  // This is ok
  echo "${env.VERSION}"
  
  // This is wrong
  echo '${env.VERSION}'
}
```

#### 2. org.jenkinsci.plugins.workflow.steps.MissingContextVariableException: Required context class hudson.FilePath is missing

```
org.jenkinsci.plugins.workflow.steps.MissingContextVariableException: Required context class hudson.FilePath is missing
Perhaps you forgot to surround the code with a step that provides this, such as: node
	at org.jenkinsci.plugins.workflow.steps.StepDescriptor.checkContextAvailability(StepDescriptor.java:261)
```

在使用 `readMavenPom()` 时报的错

```
agent none
environment {
  V = readMavenPom().getVersion()
}  // This is wrong
// 上面这种配置方式就会 Required context class hudson.FilePath is missing, 原因是 没有配置可用的agent
// 修改成 agent any 或者 agent { label 'master' }
agent any
environment {
  V = readMavenPom().getVersion()
}  // This is OK
```

#### 3. java.lang.IllegalArgumentException: Expected named arguments but got pom.xml

```
// 在使用 readMavenPom() 并需要传参数时，需要
environment {
  V = readMavenPom(file: 'pom.xml').getVersion()
  SUB_MODULE_ARTIFACT_ID = readMavenPom(file: 'sub_module/pom.xml').getArtifactId()
}
```

#### 4. 在 readMavenPom 出错时，要在 scriptApproval 允许 Approvals

![](https://upload-images.jianshu.io/upload_images/13639652-1df0c5d66a7a8000.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

readMavenPom 可以获取 pom.xml 中的配置信息，只能是静态读取文件中的内容，意思就是:

```
// 如果 pom.xml 中是这么定义的
<version>${revision}</version>

<properties>
  <revision>1.0.0</revision>
</properties>

// 那么 
readMavenPom(file: 'pom.xml').getVersion()  // 输出 ${revision}, 这就是静态读取

// 如果读取到实际的值
environment {
  VERSION = readMavenPom(file: 'pom.xml').getProperties().getProperty("revision")  // 这里需要允许 Approvals
}
```
readMavenPom 可以读取到的信息：http://maven.apache.org/components/ref/3.3.9/maven-model/apidocs/org/apache/maven/model/Model.html
