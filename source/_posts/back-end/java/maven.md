---
title: Maven
date: 2017-03-21 15:15:40
categories: java
tags: maven
---
# Maven

## 配置

将`%M2_HOME%\conf`下的`settings.xml` CV到 `%HOME%\.m2`下

### 修改maven仓库

改为英国节点, 在`mirrors`节点下添加:  

```xml
<mirror>
    <id>UK</id>
    <name>UK Central</name>
    <url>http://uk.maven.org/maven2</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```
