---
title: spring-aop
cover: >-
  https://github.com/jjnian/images/blob/main/blog/%E7%81%AB%E8%BD%A6%E7%AB%99.png?raw=true
date: 2023-07-16 17:35:25
---

##  

### 引入依赖

本次使用的是spring源码，所以直接引入的是spring-aspects模块

```gradle
implementation(project(":spring-aspects"))
```

### 创建切面类

```java
package cn.clean.aspect.annotation;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.*;
import org.aspectj.lang.reflect.SourceLocation;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class ExecutionAspect {
    @Pointcut("execution(* cn.clean.service..*(..))")
    public void pointCut(){}

    @Before("pointCut()")
    public void before(){
        System.out.println("Before");
    }

    @AfterThrowing(pointcut = "pointCut()",throwing = "ex")
    public void afterThrowing(Exception ex){
        ex.printStackTrace();
        System.out.println("AfterThrowing");
    }

    @AfterReturning(pointcut = "pointCut()", returning = "result")
    public void afterReturning(Object result){
        System.out.println("AfterReturning:"+result);
    }

     @Around("pointCut()")
     public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
         System.out.println("Around after:" + proceed);
         return proceed;
     }

    @After("pointCut()")
    public void after(){
        System.out.println("After");
    }
}

```

### 创建测试服务类

```java
package cn.clean.service.annotation;

import org.springframework.stereotype.Service;

@Service
public class AspectService {

    public String aspect(String name, String address){
        System.out.println("hello aspect");
        System.out.println("args:" + name + " args:" + address);
        return "hello aspect";
    }
}
```

### 创建测试主类

```java
package cn.clean.main.annotation;

import cn.clean.config.AppConfig;
import cn.clean.service.annotation.ArgsService;
import cn.clean.service.annotation.AnnoAspectService;
import cn.clean.service.annotation.AspectService;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class AspectMain {
    public static void main(String[] args) {
         AnnotationConfigApplicationContext acp = new AnnotationConfigApplicationContext(AppConfig.class);
         AspectService bean = acp.getBean(AspectService.class);
         bean.aspect("name","address");
    }
}
```

### Advice的调用的先后顺序

Around前置处理   >  Before  >  目标方法  >  AfterReturning  >  After  >  Around后置处理

### 切点的定义

```java
// 权限修饰符 目标方法
// 第一个*代表任意修饰符
// cn.clean.service..*(..)) 代表cn.clean.service包下的任意方法
@Pointcut("execution(* cn.clean.service..*(..))")
public void pointCut(){}

// 匹配方法中的参数，如果匹配上就触发该切点
// 匹配多个参数时可以@Pointcut("args(java.lang.String, java.lang.String)")
@Pointcut("args(java.lang.String)")
public void argsPointCut(){}

// 匹配指定包下的类的所有方法
@Pointcut("within(cn.clean.service..*)")
public void withPointCut(){}

// 匹配方法上有这个注解的方法
@Pointcut("@annotation(cn.clean.aspect.annotation.anno.MyAnnotation)")
public void pointCut(){}

// 匹配方法的第一个参数类带这个注解的方法
// 一定是参数类上面带注解，例如一个参数是Address这个类，这个类上必须带这个注解才能命中
@Pointcut("@args(cn.clean.aspect.annotation.anno.ArgsAnnotation)")
public void argsPointCut(){}

// 匹配目标类上带这个注解的类
// 这个注解必须带@Retention(RetentionPolicy.RUNTIME)
@Pointcut("@target(cn.clean.aspect.annotation.anno.TargetAnnotation)")
public void argsPointCut(){}

// 匹配目标类上带这个注解的类
@Pointcut("@within(cn.clean.aspect.annotation.anno.WithAnnotation)")
public void withPointCut1(){}
```
