## tomcat源码在idea中的构建
### tomcat8.5示例
1. 下载源码包，如apache-tomcat-8.5.58-windows-x64.zip，并解压
2. 在解压后的apache-tomcat-8.5.58-src目录中，创建文件夹source，将conf以及webapps文件夹移动到source中
3. 在解压后的apache-tomcat-8.5.58-src目录中新增pom.xml，见源码pom.xml
4. 修改源码解决idea中运行控制台乱码问题，参见截图或者本项目源码进行查看
> org.apache.tomcat.util.res.StringManager.java

[StringManager类修改源码位置截图详情](docs/images/StringManager源码-修改编码位置图.png)

```
// change encoding start
try {
    str = new String(str.getBytes(StandardCharsets.ISO_8859_1), StandardCharsets.UTF_8);
} catch (Exception e) {
    e.printStackTrace();
}
// change encoding end
```
> java.org.apache.jasper.compiler.Localizer.java

[Localizer类修改源码位置截图详情](docs/images/Localizer源码-修改编码位置图.png)

```
// change encoding start
try {
    errMsg = new String(errMsg.getBytes(StandardCharsets.ISO_8859_1), StandardCharsets.UTF_8);
} catch (Exception e) {
    e.printStackTrace();
}
// change encoding end
```
5. 启动类org.apache.catalina.startup.Bootstrap.java，idea启动设置VM options，具体路径修改为自己的source文件夹路径即可

```
-Dcatalina.home=${YOUR_PATH}/apache-tomcat-8.5.58-src/source
-Dcatalina.base=${YOUR_PATH}/apache-tomcat-8.5.58-src/source
-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
-Djava.util.logging.config.file=${YOUR_PATH}/apache-tomcat-8.5.58-src/source/conf/logging.properties
```
6. jsp引擎初始化
> org.apache.catalina.startup.ContextConfig

[初始化jsp解析引擎-源码位置截图详情](docs/images/初始化jsp解析引擎-jasper.png)
```
// 初始化jsp解析引擎-jasper
context.addServletContainerInitializer(new JasperInitializer(), null);
```
7. 访问
> http://localhost:8080

