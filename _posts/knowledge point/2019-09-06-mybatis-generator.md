---
layout: post
title: MyBatis Generator
subtitle: MyBatis逆向工程
author: Balbo Cheng
categories: programming-language
tags: [java, mybatis]
---

## 什么是MyBatis的逆向工程？

MyBatis逆向工程是指用数据库的表直接生成Java代码，利用MyBatis官方提供的逆向工程，可以针对单表自动生MyBatis执行所需要的代码（如po类，mapper.java和mapper.xml）

## 生成逆向工程

生成逆向工程的方式有多种，推荐使用Java程序和XML配置文件的方式进行实现。

## MyBatis逆向工程

### 数据库配置文件db.properties

这里为了解耦，将数据库信息和要生成的表的信息放到一个db.properties文件里

jdbc连接数据库配置信息

```java
jdbc.driver = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8
jdbc.username = root
jdbc.password = 123456

#数据库中要生成的表
jdbc.table.items = items
jdbc.table.orders = orders
jdbc.table.orderdetail = orderdetail
jdbc.table.user = user
```

### 生成代码配置文件generateConfig.xml

在该文件中配置要生成哪些表的信息，以及生成的PO类，mapper.xml和mapper.java所在的包，这里为了今后能用MyBatis的代理进行开发，让mapper.xml和mapper.java在同一个包中。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>

<!-- mybatis的配置文件 -->

<properties resource="db.properties"/>

<!--指定特定数据库的jdbc驱动jar包的位置 -->
<classPathEntry location="${jdbc.driverLocation}"/>

<!--classPathEntry 
location="D:\zngkpt\m2\repository\mysql\mysql-connector-java\5.1.40\mysql-connector-java-5.1.40.jar" 
-->
<context id="context1" targetRuntime="MyBatis3">

<commentGenerator>
<!-- 去除自动生成的注释 -->
<property name="suppressAllComments" value="true" />
</commentGenerator>

<!-- 数据库连接配置 -->
<jdbcConnection driverClass="${jdbc.driverClass}"
connectionURL="${jdbc.connectionURL}"
userId="${jdbc.userId}"
password="${jdbc.password}" />
<!--jdbcConnection driverClass="com.mysql.jdbc.Driver"
connectionURL="jdbc:mysql://localhost:3306/test"
userId="root"
password="mysql" /-->

<!-- 非必需，类型处理器，在数据库类型和java类型之间的转换控制-->
<javaTypeResolver>
<property name="forceBigDecimals" value="false"/>
</javaTypeResolver>

<!--配置生成的实体包
targetPackage：生成的实体包位置，默认存放在src目录下
targetProject：目标工程名
-->
<!-- 指定javabean生成的位置 -->
<javaModelGenerator targetPackage="com.maven.bean" targetProject="..\ssm_crud\src\main\java">
<property name="enableSubPackages" value="false"/>
<property name="trimStrings" value="true"/>
</javaModelGenerator>

<sqlMapGenerator targetPackage="mapper" targetProject="..\ssm_crud\src\main\resources">
<property name="enableSubPackages" value="false"/>
</sqlMapGenerator>

<javaClientGenerator targetPackage="com.maven.dao" type="XMLMAPPER"
 targetProject="..\ssm_crud\src\main\java">
<property name="enableSubPackages" value="false"/>
</javaClientGenerator>

<!-- 配置表
schema：不用填写
tableName: 表名
enableCountByExample、enableSelectByExample、enableDeleteByExample、
enableUpdateByExample、selectByExampleQueryId：
去除自动生成的例子
-->

<table tableName="tbl_emp" domainObjectName="Employee" 
enableCountByExample="true" enableDeleteByExample="true"
enableSelectByExample="true" enableUpdateByExample="true"/>
<table tableName="tbl_dept" domainObjectName="Department" 
enableCountByExample="true" enableDeleteByExample="true"
enableSelectByExample="true" enableUpdateByExample="true"/>

</context>
</generatorConfiguration>
```

### 执行生成程序GenerateSqlMap.java

```java
import java.io.File;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

public class GeneratorSqlmap {

    public void generator() throws Exception{

        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        //指定 逆向工程配置文件
        File configFile = new File("config/generatorConfig.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
        callback, warnings);
        myBatisGenerator.generate(null);

    }
    public static void main(String[] args) throws Exception {
        try {
        GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
        generatorSqlmap.generator();
        } catch (Exception e) {
        e.printStackTrace();
        }

    }

}
```

**在实际开发中，建议另写一个project来实现MyBatis的逆向工程**

然后再从中挑选出实际需要的POJO类和mapper.xml，千万不要在原有项目中直接生成，那样会比较乱。

MyBatis逆向工程比较简单，可以直接做为模板工程进行应用，只需要改一下数据库名，表名和包的路径就行了，这里提供项目下载地址，可以直接使用。

## 在maven工程中的resource中创建generatorConfig.xml

### 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
   <!--mysql 连接数据库jar 这里选择自己本地位置-->
   <classPathEntry location="D:/mysql-connector-java-5.1.20-bin.jar" />
   <context id="testTables" targetRuntime="MyBatis3">
      <commentGenerator>
         <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
         <property name="suppressAllComments" value="true" />
      </commentGenerator>
      <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
      <jdbcConnection driverClass="com.mysql.jdbc.Driver"
         connectionURL="jdbc:mysql://localhost:3306/ecps" userId="root"
         password="root">
      </jdbcConnection>
      <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
         NUMERIC 类型解析为java.math.BigDecimal -->
      <javaTypeResolver>
         <property name="forceBigDecimals" value="false" />
      </javaTypeResolver>

      <!-- targetProject:生成PO类的位置 -->
      <javaModelGenerator targetPackage="com.ecps.seckill.pojo"
         targetProject="src/main/java">
         <!-- enableSubPackages:是否让schema作为包的后缀 -->
         <property name="enableSubPackages" value="false" />
         <!-- 从数据库返回的值被清理前后的空格 -->
         <property name="trimStrings" value="true" />
      </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置
           如果maven工程只是单独的一个工程，targetProject="src/main/java"
           若果maven工程是分模块的工程，targetProject="所属模块的名称"，例如：
           targetProject="ecps-manager-mapper"，下同-->
      <sqlMapGenerator targetPackage="com.ecps.seckill.mapper"
         targetProject="src/main/java">
         <!-- enableSubPackages:是否让schema作为包的后缀 -->
         <property name="enableSubPackages" value="false" />
      </sqlMapGenerator>
      <!-- targetPackage：mapper接口生成的位置 -->
      <javaClientGenerator type="XMLMAPPER"
         targetPackage="com.ecps.seckill.mapper"
         targetProject="src/main/java">
         <!-- enableSubPackages:是否让schema作为包的后缀 -->
         <property name="enableSubPackages" value="false" />
      </javaClientGenerator>
      <!-- 指定数据库表 -->
      <table schema="" tableName="seckill"></table>
      <table schema="" tableName="success_killed"></table>   
   </context>
</generatorConfiguration>
```

### 配置pom.xml

在pom.xml中位置mybatis-generator的插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.2</version>
            <configuration>
            <!--配置文件的位置-->      
            <configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
                <verbose>true</verbose>
                <overwrite>true</overwrite>
            </configuration>
            <executions>
                <execution>
                    <id>Generate MyBatis Artifacts</id>
                    <goals>
                        <goal>generate</goal>
                    </goals>
                </execution>
            </executions>
            <dependencies>
                <dependency>
                    <groupId>org.mybatis.generator</groupId>
                    <artifactId>mybatis-generator-core</artifactId>
                    <version>1.3.2</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```

### 生成对象的两种方式

**# 使用idea的maven插件直接快速生成**

1. 在完成以上两步之后。就会在idea中看到：直接点击mybatis-generator:generate就可生成

**# 在Intellij IDEA添加一个“Run运行”选项，使用maven运行mybatis-generator-maven-plugin插件**

1. 选择配置edit configuration

2. 创建maven运行项

3. 配置命令 mybatis-generator:generate -e

4. 运行