[参考SpringBoot官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/getting-started.html#getting-started-first-application)

# 1. 创建POM文件

打开idea，创建一个maven工程

在pom.xml文件中加入以下代码：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.5.RELEASE</version>
    </parent>

    <description/>
    <developers>
        <developer/>
    </developers>
    <licenses>
        <license/>
    </licenses>
    <scm>
        <url/>
    </scm>
    <url/>

    <!-- Additional lines to be added here... -->

</project>
```

打开Terminal命令窗口，执行`mvn package`命令检测是否配置成功

# 2. 添加类路径依赖

由于我们要创建一个web应用，我们应该在`parent`之下添加`spring-boot-stater-web`依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

# 3. 写代码

在`src/main/java`包下创建`Example.java`:

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController//RestController 是一个定型注解，它将Example这个类变成了一个 web @Contrller ，这样的话Spring在处理web请求时会考虑这个类
@EnableAutoConfiguration//这个注解告诉Spring Boot 根据所添加的依赖来“猜测”应该如何配置Spring
public class Example {

    @RequestMapping("/")//带有路径信息，它告诉Spring任何带有 “/” 的HTTP请求应该映射到home方法
    String home() {
        return "Hello World!";
    }

    /**
     * 应用入口点，main方法通过调用run来委托Spring Boot的SpringApplication类
     * SpringApplication 引导应用启动Spring，Spring反过来又启动自动配置的TomcatWeb服务器
     * 我们需要将Example.class作为run方法的参数来告诉SpringApplication那个是主要的Spring组件
     * args数组也传递过去来暴露任何命令行参数
     * @param args
     */
    public static void main(String[] args) {
        SpringApplication.run(Example.class, args);
    }
}

```

# 4. 执行程序

由于使用了`spring-boot-stater-parent`POM,我们可以在根项目路径下执行`mvn spring-boot:run`来启动应用程序，效果如下：

```
$ mvn spring-boot:run

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v2.3.5.RELEASE)
....... . . .
....... . . . (log output here)
....... . . .
........ Started Example in 2.222 seconds (JVM running for 6.514)
```



# 5. 创建可执行jar文件

为了创建可执行jar文件，我们需要在`pom.xml`文件中添加`spring-boot-maven-plugin`。将如下代码添加到`dependencies`下面：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

在命令行执行`mvn package`命令：

```
D:\Users\acui\IdeaProjects\MyFirstSpringbootApplication>mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] --------------< org.example:MyFirstSpringbootApplication >--------------
[INFO] Building MyFirstSpringbootApplication 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:resources (default-resources) @ MyFirstSpringbootApplication ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 0 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ MyFirstSpringbootApplication ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:3.1.0:testResources (default-testResources) @ MyFirstSpringbootApplication ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory D:\Users\acui\IdeaProjects\MyFirstSpringbootApplication\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ MyFirstSpringbootApplication ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-surefire-plugin:2.22.2:test (default-test) @ MyFirstSpringbootApplication ---
[INFO]
[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ MyFirstSpringbootApplication ---
[INFO]
[INFO] --- spring-boot-maven-plugin:2.3.5.RELEASE:repackage (repackage) @ MyFirstSpringbootApplication ---
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.546 s
[INFO] Finished at: 2020-11-04T16:29:06+08:00
[INFO] ------------------------------------------------------------------------

```

现在可以看到`target`目录下有了`MyFirstSpringbootApplication-0.0.1-SNAPSHOT.jar`,用以下命令来查看其内部结构:

```
> java -jar target/MyFirstSpringbootApplication-0.0.1-SNAPSHOT.jar
```

可以通过`java -jar`命令来执行该文件：

```
$ java -jar target/MyFirstSpringbootApplication-0.0.1-SNAPSHOT.jar

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v2.3.5.RELEASE)
....... . . .
....... . . . (log output here)
....... . . .
........ Started Example in 2.536 seconds (JVM running for 2.864)
```

