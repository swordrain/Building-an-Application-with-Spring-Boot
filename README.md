#[翻译]使用Spring Boot创建应用

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

##能用Spring Boot做什么
Spring Boot可以快速搭建应用。它会浏览你的classpath和bean，做出合理的推断，对于缺失的会自动添加进来。有了Spring Boot你可以更专注于业务而不是构架。

##创建一个简单的web应用
`src/main/java/hello/HelloController.java`
```
package hello;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
public class HelloController {

    @RequestMapping("/")
    public String index() {
        return "Greetings from Spring Boot!";
    }

}
```
`@RestController`注解表示可以被Spring MVC用来处理web请求。`index()`方法用`@RequestMapping`匹配`/`路径。如果有请求到来，就返回一个纯文本。由于`@RestControler`结合了`@Controller`和`ResponseBody`这两个注解，响应返回的是数据而不是视图。

##创建应用class
`src/main/java/hello/Application.java`
```
package hello;

import java.util.Arrays;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        ApplicationContext ctx = SpringApplication.run(Application.class, args);

        System.out.println("Let's inspect the beans provided by Spring Boot:");

        String[] beanNames = ctx.getBeanDefinitionNames();
        Arrays.sort(beanNames);
        for (String beanName : beanNames) {
            System.out.println(beanName);
        }
    }

}
```
`@SpringBootApplication`注解合并了
* `@Configuration`
* `@EnableAutoConfiguration`
* `@EnableWebMvc`
* `@ComponentScan`

`main()`方法使用Spring Boot的`SpringApplication.run()`来启动应用。主要到这里没有一行的XML了吗，也没有web.xml文件。这个web应用是100%的纯Java，不需要去处理任何配置。

`run()`方法返回了`ApplicationContext`然后应用取出了所有的bean，不论是你自己添加的还是自动被引入的。Spring Boot把它们排序并打印出来。

##运行应用
如果用的是Gradle，执行  
`./gradlew build && java -jar build/libs/gs-spring-boot-0.1.0.jar`  
如果用的是Maven，执行  
`mvn package && java -jar target/gs-spring-boot-0.1.0.jar`  
你应该看到如下输出
```
Let's inspect the beans provided by Spring Boot:
application
beanNameHandlerMapping
defaultServletHandlerMapping
dispatcherServlet
embeddedServletContainerCustomizerBeanPostProcessor
handlerExceptionResolver
helloController
httpRequestHandlerAdapter
messageSource
mvcContentNegotiationManager
mvcConversionService
mvcValidator
org.springframework.boot.autoconfigure.MessageSourceAutoConfiguration
org.springframework.boot.autoconfigure.PropertyPlaceholderAutoConfiguration
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration$DispatcherServletConfiguration
org.springframework.boot.autoconfigure.web.EmbeddedServletContainerAutoConfiguration$EmbeddedTomcat
org.springframework.boot.autoconfigure.web.ServerPropertiesAutoConfiguration
org.springframework.boot.context.embedded.properties.ServerProperties
org.springframework.context.annotation.ConfigurationClassPostProcessor.enhancedConfigurationProcessor
org.springframework.context.annotation.ConfigurationClassPostProcessor.importAwareProcessor
org.springframework.context.annotation.internalAutowiredAnnotationProcessor
org.springframework.context.annotation.internalCommonAnnotationProcessor
org.springframework.context.annotation.internalConfigurationAnnotationProcessor
org.springframework.context.annotation.internalRequiredAnnotationProcessor
org.springframework.web.servlet.config.annotation.DelegatingWebMvcConfiguration
propertySourcesBinder
propertySourcesPlaceholderConfigurer
requestMappingHandlerAdapter
requestMappingHandlerMapping
resourceHandlerMapping
simpleControllerHandlerAdapter
tomcatEmbeddedServletContainerFactory
viewControllerHandlerMapping
```
你可以清楚看到有org.springframework.boot.autoconfigure，还有`tomcatEmbeddedServletContainerFactory`。  
检查一下服务  
```
$ curl localhost:8080
Greetings from Spring Boot!
```

##添加UT
如果想要添加一些测试，Spring Test已经具备了这些功能，很容易把它导入到你的项目中去

如果使用Gradle，添加如下的依赖
```
testCompile("org.springframework.boot:spring-boot-starter-test")
```
如果使用Maven，添加
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```
现在写一个简单的单元测试来模拟servlet的request和response
```
src/test/java/hello/HelloControllerTest.java
```

```
package hello;

import static org.hamcrest.Matchers.equalTo;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest
public class HelloControllerTest {

	private MockMvc mvc;

	@Before
	public void setUp() throws Exception {
		mvc = MockMvcBuilders.standaloneSetup(new HelloController()).build();
	}

	@Test
	public void getHello() throws Exception {
		mvc.perform(MockMvcRequestBuilders.get("/").accept(MediaType.APPLICATION_JSON))
				.andExpect(status().isOk())
				.andExpect(content().string(equalTo("Greetings from Spring Boot!")));
	}
}
```
使用了`MockServletContext`来建立了一个空的`WebApplicationContext`，`HelloController`能在`@Before`里创建并传递给`MockMvcBuilders.standaloneSetup()`。另一个替代方案是使用`Application`类创建完整的application context，并`@autowired` `HelloController`到测试里。`MovkMvc`来自于Spring Test可以让你通过一系列的builder类来发送HTTP请求到`DispatcherServlet`然后对结果进行断言。
除了模拟HTTP请求，也可以使用Spring Boot写一个简单的全栈集成测试。
```
src/test/java/hello/HelloControllerIT.java
```
```
package hello;

import static org.hamcrest.Matchers.equalTo;
import static org.junit.Assert.assertThat;

import java.net.URL;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.context.embedded.LocalServerPort;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = Application.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HelloControllerIT {

    @LocalServerPort
    private int port;

    private URL base;
    private TestRestTemplate template;

    @Before
    public void setUp() throws Exception {
        this.base = new URL("http://localhost:" + port + "/");
        template = new TestRestTemplate();
    }

    @Test
    public void getHello() throws Exception {
        ResponseEntity<String> response = template.getForEntity(base.toString(), String.class);
        assertThat(response.getBody(), equalTo("Greetings from Spring Boot!"));
    }
}
```
通过`@IntegrationTest("${server.port=0}")`使内置服务器在启动时打开一个随机的端口号，在运行时能用`@Value("${local.server.port}")`检测到

##添加生产级别服务
如果为你的业务而搭建网站，你可能需要添加一些可管理的服务。Spring Boot在[actuator module](http://docs.spring.io/spring-boot/docs/1.4.0.RELEASE/reference/htmlsingle/#production-ready)外提供了一些模块，比如健康度，审计等。

在Gradle中，添加依赖
```
compile("org.springframework.boot:spring-boot-starter-actuator")
```
在Maven中，添加依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```
如果是Gradle，重启
```
./gradlew build && java -jar build/libs/gs-spring-boot-0.1.0.jar
```
如果是Maven
```
mvn package && java -jar target/gs-spring-boot-0.1.0.jar
```
你会看到一系列新的RESTful节点被添加进来，它们是由Spring Boot提供的管理服务。
```
2014-06-03 13:23:28.119  ... : Mapped "{[/error],methods=[],params=[],headers=[],consumes...
2014-06-03 13:23:28.119  ... : Mapped "{[/error],methods=[],params=[],headers=[],consumes...
2014-06-03 13:23:28.136  ... : Mapped URL path [/**] onto handler of type [class org.spri...
2014-06-03 13:23:28.136  ... : Mapped URL path [/webjars/**] onto handler of type [class ...
2014-06-03 13:23:28.440  ... : Mapped "{[/info],methods=[GET],params=[],headers=[],consum...
2014-06-03 13:23:28.441  ... : Mapped "{[/autoconfig],methods=[GET],params=[],headers=[],...
2014-06-03 13:23:28.441  ... : Mapped "{[/mappings],methods=[GET],params=[],headers=[],co...
2014-06-03 13:23:28.442  ... : Mapped "{[/trace],methods=[GET],params=[],headers=[],consu...
2014-06-03 13:23:28.442  ... : Mapped "{[/env/{name:.*}],methods=[GET],params=[],headers=...
2014-06-03 13:23:28.442  ... : Mapped "{[/env],methods=[GET],params=[],headers=[],consume...
2014-06-03 13:23:28.443  ... : Mapped "{[/configprops],methods=[GET],params=[],headers=[]...
2014-06-03 13:23:28.443  ... : Mapped "{[/metrics/{name:.*}],methods=[GET],params=[],head...
2014-06-03 13:23:28.443  ... : Mapped "{[/metrics],methods=[GET],params=[],headers=[],con...
2014-06-03 13:23:28.444  ... : Mapped "{[/health],methods=[GET],params=[],headers=[],cons...
2014-06-03 13:23:28.444  ... : Mapped "{[/dump],methods=[GET],params=[],headers=[],consum...
2014-06-03 13:23:28.445  ... : Mapped "{[/beans],methods=[GET],params=[],headers=[],consu...
```
检查应用的健康情况就容易了
```
$ curl localhost:8080/health
{"status":"UP"}
```
如果尝试通过curl来关闭
```
$ curl -X POST localhost:8080/shutdown
{"timestamp":1401820343710,"error":"Method Not Allowed","status":405,"message":"Request method 'POST' not supported"}
```
由于我们没启用，所以该请求被阻止了。
如果需要更多相关的功能，你可以配置`application.properties`文件，参考[docs about the endpoints](http://docs.spring.io/spring-boot/docs/1.4.0.RELEASE/reference/htmlsingle/#production-ready-endpoints)

##查看Spring Boot的初学者文档
[Spring Boot's starters](http://docs.spring.io/spring-boot/docs/1.4.0.RELEASE/reference/htmlsingle/#using-boot-starter-poms)  
以及[这里](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-starters)

##JAR支持和Groovy支持
Spring Boot不止支持传统的war部署，还通过loader module来打包成可执行的JAR。  
另外Spring Boot还支持Groovy，使你可以仅通过一个文件来创建Spring MVC web应用。  
创建一个文件取名`app.groovy`
```
@RestController
class ThisWillActuallyRun {

    @RequestMapping("/")
    String home() {
        return "Hello World!"
    }

}
```
文件的路径无所谓。
下一步，[安装Spring Boot's CLI](http://docs.spring.io/spring-boot/docs/1.4.0.RELEASE/reference/htmlsingle/#getting-started-installing-the-cli)
运行
```
$ spring run app.groovy
```
在另一个终端窗口运行
```
$ curl localhost:8080
Hello World!
```
Spring Boot能动态添加关键注解到你的代码中并拉取必须的库

##总结
略
