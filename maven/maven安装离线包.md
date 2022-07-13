Maven 安装离线包

1. 遇到一个问题，当 maven 安装一个自定义SDK 时，使用 
```shell
mvn install:install-file -Dfile=chain-sdk-1.0.0.jar -DgroupId=com.zdww.sroc -DartifactId=chain-sdk -Dversion=1.0.0 -Dpackaging=jar
```
时，会导致本地 maven 仓库 对应包的pom 文件不全（没有包含当前sdk 所引用的其他包，会导致其他项目引用该项目时报错找不到对应的包）

2. 解决方式，通过maven install plugins 方式安装离线包，注意插件版本

```
mvn org.apache.maven.plugins:maven-install-plugin:3.0.0-M1:install-file -Dfile=chain-sdk-1.0.0.jar 
```

参考： https://stackoverflow.com/questions/26258877/maven-does-not-read-pom-when-installing-jar-to-repository