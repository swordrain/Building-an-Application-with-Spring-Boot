# Building-an-Application-with-Spring-Boot
[翻译]使用Spring Boot创建应用

本文展示如何使用[Spring Boot](https://github.com/spring-projects/spring-boot)来高速便捷进行开发。当你读下去时，你会发现更多的Spring Boot的use case，意味着你快速品尝到了Spring Boot的滋味。如果你想创建一个你自己的基于Spring Boot的项目，可以访问[Spring Initializr](http://start.spring.io/)，填入你的项目信息，做出选择，然后下载Maven的构建文件或者打包后的zip文件。

##你将会创建的
* 你将会使用Spring Boot创建一个简单应用，并添加一些有用的服务。

##你所需要的
* 大约15分钟
* 你趁手的文本编辑器或IDE
* [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)或更新版本
* [Gradle 2.3+](http://www.gradle.org/downloads)或[Maven 3.0+](http://maven.apache.org/download.cgi)
* 你既可以直接看浏览网页也可以导入本文的代码到[STS](http://spring.io/guides/gs/sts)并继续

##如何完成本文示例
类似于大多数的Spring [Getting Started guides](http://spring.io/guides)，你可以从头开始完成每一步，也可以跳过熟悉的基础步骤。

从头开始，请看 **Gradle构建**  
想到跳过基础步骤，
* [下载](https://github.com/spring-guides/gs-spring-boot/archive/master.zip)并解压，或者用Git来Clone `git clone https://github.com/spring-guides/gs-spring-boot.git`
* 进入目录`gs-spring-boot/initial`
* 继续

当你完成时，可以和`gs-spring-boot/complete`的代码比较一下

##Gradle构建

└── src  
&nbsp;&nbsp;&nbsp;&nbsp;└── main  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── java  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── hello  

`build.gradle`
```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.4.0.RELEASE")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'spring-boot'

jar {
    baseName = 'gs-spring-boot'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    // tag::jetty[]
    compile("org.springframework.boot:spring-boot-starter-web") {
        exclude module: "spring-boot-starter-tomcat"
    }
    compile("org.springframework.boot:spring-boot-starter-jetty")
    // end::jetty[]
    // tag::actuator[]
    compile("org.springframework.boot:spring-boot-starter-actuator")
    // end::actuator[]
    testCompile("junit:junit")
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.3'
}
```

##Maven构建

创建目录结构  
└── src  
&nbsp;&nbsp;&nbsp;&nbsp;└── main  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── java  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── hello  

`pom.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-spring-boot</artifactId>
    <version>0.1.0</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.0.RELEASE</version>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <properties>
        <java.version>1.8</java.version>
    </properties>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```
[Spring Boot Maven 插件](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-tools/spring-boot-maven-plugin)提供了
* 集合所有的classpath的jar包，编译出一个可执行的`über-jar`，使得执行和传输服务更方便
* 查找 `public static void main()`方法来标记为可执行class
* 提供了内建的依赖处理设置版本号来匹配[Spring Boot 依赖](https://github.com/spring-projects/spring-boot/blob/master/spring-boot-dependencies/pom.xml)。你也可以覆盖版本号，但默认是Spring Boot选择的那一系列版本号。

##使用IDE构建
[STS](http://spring.io/guides/gs/sts/)  
[IntelliJ IDEA](http://spring.io/guides/gs/intellij-idea)

##能用Spring Boot做she那么


