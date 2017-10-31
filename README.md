[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.exampledriven/grpc-eureka-java/badge.svg)](https://maven-badges.herokuapp.com/com.github.exampledriven/grpc-eureka-java/rsql-parser)

[![Build Status](https://travis-ci.org/ExampleDriven/grpc-eureka-java.svg?branch=master)](https://travis-ci.org/ExampleDriven/grpc-eureka-java)

# GRPC, Eureka integration in Java

This project integrates GRPC with Eureka, the best way to understand how it works is to look at the [example code](https://github.com/ExampleDriven/spring-boot-grpc-example)

## Server setup
The server does not need any special setup, it should just start GRPC the usual way and register itself to Eureka the usual way

## Client setup

### Using the Spring Boot Starter

1. Add the starter dependency to your project
```xml
    <parent>
        <groupId>com.github.exampledriven</groupId>
        <artifactId>grpc-eureka-java-starter</artifactId>
        <version>version</version>
    </parent>
```


2. Set the `grpc.eureka.service-id` property to the service id of your server

3. A default instance of ManagedChannel is auto created for you. You can use it like this

```Java

    @Autowired
    ManagedChannel managedChannel;
    
    @PostConstruct
    private void initializeClient() {

        bookServiceBlockingStub = BookServiceGrpc.newBlockingStub(managedChannel);

    }     
    
```

## Without using Spring Boot Starter

1. Add the following dependency 

```xml
    <parent>
        <groupId>com.github.exampledriven</groupId>
        <artifactId>grpc-eureka-java</artifactId>
        <version>version</version>
    </parent>
```    

2. You have to manually create an instance of ManagedChannel like this

```Java
        return ManagedChannelBuilder
                .forTarget("eureka://" + eurekaServiceId)
                .nameResolverFactory(new EurekaNameResolverProvider(eurekaClientConfig, "grpc.port"))
                .loadBalancerFactory(RoundRobinLoadBalancerFactory.getInstance())
                .usePlaintext(true)
                .build();
```