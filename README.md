# MyBatis Generator Lombok plugin and Comment

## 实现的功能

- 主要整合了lombok插件实现getter/setter等通用方法的自动生成，同时自定义实现了一个注释生成器，
通过抓取数据库表里面的注释作为实体类的注释内容。

## 插件的用法

- 如果你想在你的maven中使用,就直接`git clone`这个项目到你的IDEA，然后使用Maven`clean|install`将这个项目添加到Maven本地仓库里去。
之后你只要在你的要使用这个插件的项目的pom.xml中加入如下内容便可：

```
    <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>${generator.version}</version>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <dependency>
                <groupId>net.conn</groupId>
                <artifactId>mybatis-generator-lombok-plugin</artifactId>
                <version>1.0-SNAPSHOT</version>
            </dependency>
        </dependencies>
        <configuration>
            <configurationFile>src/main/resources/generator/generatorConfig.xml</configurationFile>
            <!--是否覆盖同名文件（只是针对XML文件,java文件生成类似*.java.1、*.java.2形式的文件）-->
            <overwrite>true</overwrite>
            <!--是否将生成过程输出至控制台-->
            <verbose>true</verbose>
        </configuration>
    </plugin>
```

同时添加配置文件`generatorConfig.xml`,使用的时候请根据项目需要自行修改对应配置

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
    PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <properties resource="application.properties"/>

    <context id="Mysql" targetRuntime="MyBatis3Simple" defaultModelType="flat">
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        
        <!-- 整合lombok-->
        <plugin type="net.conn.mybatis.generator.plugins.LombokAndCommentPlugin" >
            <property name="hasLombok" value="true"/>
        </plugin>
        
        <!--是否去除mapper注释-->
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>

        <jdbcConnection driverClass="${spring.datasource.driverClassName}"
            connectionURL="${spring.datasource.url}"
            userId="${spring.datasource.username}"
            password="${spring.datasource.password}">
        </jdbcConnection>
        
        <!--Entity生成位置-->
        <javaModelGenerator targetPackage="net.conn.potal.entity" targetProject="src/main/java"/>
        
        <!--mapper文件生成位置-->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources"/>
        
        <!--Mapper接口生成位置-->
        <javaClientGenerator targetPackage="net.conn.potal.mapper" targetProject="src/main/java" type="XMLMAPPER"/>

        <table tableName="user" domainObjectName="User">
            <generatedKey column="id" sqlStatement="Mysql" identity="true"/>
        </table>
    </context>
</generatorConfiguration>
```

## Author
- Conn 做个俗人 贪财好色

