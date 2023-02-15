Spring Cloud Gateway implements the matching rules of Route routes through Predicate assertions. To put it simply, Predicate is the judgment condition of route forwarding. The request will be forwarded to the specified service for processing only if it meets the condition of Predicate.

Let's use an example to demonstrate how Predicate is used.

1\. Create a Spring Boot module named micro-service-cloud-gateway-9527 under the parent project DataEngineSwarm, and introduce relevant dependencies in its pom.xml. The configuration is as follows.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.luxbp</groupId>
        <artifactId>DataEngineSwarm</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <groupId>com.luxbp</groupId>
    <artifactId>micro-service-cloud-gateway-9527</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>micro-service-cloud-gateway-9527</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!--Note：don't introduce spring-boot-starter-web into Gateway，othervise it will bring up a error-->
        <!-- Spring cloud gateway dependency-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        <!--Eureka -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
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

2\. In the classpath (/resources directory) of micro-service-cloud-gateway-9527, create a new configuration file application.yml, with the configuration content as follows.

```yaml
server:
  port: 9527
spring:
  application:
    name: microServiceCloudGateway
  cloud:
    gateway:
      routes:
        #hide micro-service-cloud-provider-dept-8001
        - id: provider_dept_list_routh
          uri: http://localhost:8001
          predicates:
            # Predicate
            - Path=/dept/list/**
            - Method=GET
eureka:
  instance:
    instance-id: micro-service-cloud-gateway-9527
    hostname: micro-service-cloud-gateway
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

3\. On the main startup class of micro-service-cloud-gateway-9527, use the @ EnableEurekaClient annotation to enable the Eureka client function



By default, Spring Cloud Gateway will create a dynamic route for forwarding based on the service list maintained in the service registry (such as Eureka Server) and the service name (spring. application. name) as the path, so as to realize the dynamic routing function.

We can modify the uri address of Route to the following form in the configuration file.

    lb://service-name

Modify the configuration of application.yml in micro-service-cloud-gateway-9527 and use the microservice name in the registry to create a dynamic route for forwarding. The configuration is as follows.

```yaml
server:
  port: 9527
spring:
  application:
    name: microServiceCloudGateway
   
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true
      routes:
        - id: provider_dept_list_routh
          uri: lb://MICROSERVICECLOUDPROVIDERDEPT # dynamic routing
          predicates:
            - Path=/dept/list/**
            - Method=GET
eureka:
  instance:
    instance-id: micro-service-cloud-gateway-9527
    hostname: micro-service-cloud-gateway
  client:
    fetch-registry: true
    register-with-eureka: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

Filter
------

wait to be updated



