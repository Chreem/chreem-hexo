# 环境配置

|name|描述
|---|---|
|环境变量|JAVA_HOME, CLASSPATH等|
|工具|VSCode, Intellij及激活|
|BS|hello world|
|问题|一些阻碍hello world运行的问题|

## 环境变量

JAVA_HOME: `JDK目录`, 目的是将JDK下的bin目录告诉path, 以及方便配置CLASSPATH
CLASSPATH: `.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`, `.;`表示执行java命令所在目录下的class

## 工具

### VSCode

安装`Language support for Java ™ for Visual Studio Code`插件即可  
**非工程目录会提示classpath的警告**

### Intellij

非旗舰版不用, 注册:

1. download `JetbrainsCrack-x.x.x-release-enc.jar`

2. move 上面的文件至intellij安装目录的`bin`下

3. idea.exe.vmoptions和idea64.exe.vmoptions后面都加上`-javaagent:Intellij目录\bin\JetbrainsCrack-x.x.x-release-enc.jar`

4. active code:  
    ```md
    ThisCrackLicenseId-{  
    "licenseId":"ThisCrackLicenseId",  
    "licenseeName":"idea",  
    "assigneeName":"",  
    "assigneeEmail":"idea@163.com",  
    "licenseRestriction":"For This Crack, Only Test! Please support genuine!!!",  
    "checkConcurrentUse":false,  
    "products":[  
    {"code":"II","paidUpTo":"2099-12-31"},  
    {"code":"DM","paidUpTo":"2099-12-31"},  
    {"code":"AC","paidUpTo":"2099-12-31"},  
    {"code":"RS0","paidUpTo":"2099-12-31"},  
    {"code":"WS","paidUpTo":"2099-12-31"},  
    {"code":"DPN","paidUpTo":"2099-12-31"},  
    {"code":"RC","paidUpTo":"2099-12-31"},  
    {"code":"PS","paidUpTo":"2099-12-31"},  
    {"code":"DC","paidUpTo":"2099-12-31"},  
    {"code":"RM","paidUpTo":"2099-12-31"},  
    {"code":"CL","paidUpTo":"2099-12-31"},  
    {"code":"PC","paidUpTo":"2099-12-31"}  
    ],  
    "hash":"2911276/0",  
    "gracePeriodDays":7,  
    "autoProlongated":false}
    ```

## Basic Usage

1. 新建文件Hello.java

2. 类Hello:
    ```java
    public class Hello {
        public static void main(String[] args) {
            System.out.println("hello world");
        }
    }
    ```

## 问题

1. java中存在中文则javac提示 `编码GBK的不可映射字符`  
    指明编码类型: `javac -encoding UTF-8 有中文的*.java`
