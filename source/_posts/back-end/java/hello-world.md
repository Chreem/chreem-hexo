---
title: Java basic usage
date: 2017-08-21 15:15:40
---
# 环境配置

|name|描述
|---|---|
|环境变量|JAVA_HOME, CLASSPATH等|
|工具|VSCode, Intellij|
|BS|hello world|
|问题|一些阻碍hello world运行的问题|

<!-- more -->

## 环境变量

JAVA_HOME: `JDK目录`, 目的是将JDK下的bin目录告诉path, 以及方便配置CLASSPATH
CLASSPATH: `.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;`, `.;`表示执行java命令所在目录下的class

## 工具

### VSCode

安装`Language support for Java ™ for Visual Studio Code`插件即可  
**非工程目录会提示classpath的警告**

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
