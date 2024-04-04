# **Spring Cloud Eureka**

### **Eureka Server 구축**

#### **1. 프로젝트 생성**

<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/9d288fc9-c683-4d77-93f5-685bcee48d22" width="800" height="450">



#### **2. Eureka Server 어노테이션 추가**
<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/475b3e66-8ca9-4587-8fcf-5471951bf91a" width="600" height="350">


#### **3. application.properties 추가**
```
spring.application.name=eureka-server
server.port=8761

# eureka 서버에 자기자신을 등록하지 않음
eureka.client.register-with-eureka=false
```

#### **4. 실행 및 동작 확인**
<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/24d60b65-8204-4615-8ea6-f7bd6fdb47c9" width="800" height="400">



 
---
### **Eureka Client 구축**

#### **1. 프로젝트 생성**

<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/52ebed05-45b8-432f-9622-e9bfe90b765a" width="800" height="400">

#### **2. application.properties 추가**
```
server.port=8762
spring.application.name=eureka-client

# eureka 서버에 등록
eureka.client.register-with-eureka=true

# 이 서비스에 대한 정보를 유레카 서버에서 주기적으로 갱신
eureka.client.fetch-registry=true

eureka.client.service-url.defaultZone=http://localhost:8761/eureka
```

#### **3. 실행 및 동작 확인**
<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/e9f03c73-eff2-4367-8305-e48565339383" width="800" height="400">

---
### **Application Service 생성**

#### **1. 프로젝트 생성**
- 프로젝트 이름은 사진과 다르게 eureka-service로 생성

<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/ae0d07b8-6bea-4bc3-bee5-613105630a27" width="800" height="400">


#### **2. application.properties 추가**
```
spring.application.name=eureka-service
server.port=8763

# eureka 서버에 등록
eureka.client.register-with-eureka=true

# 이 서비스에 대한 정보를 유레카 서버에서 주기적으로 갱신
eureka.client.fetch-registry=true

eureka.client.service-url.defaultZone:http://localhost:8761/eureka
```

#### **3. 컨트롤러 생성**
```
package com.example.userservice.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {

    @GetMapping("/user")
    public String home() {
        return "Hello User Service";
    }

}

```


#### **4. 실행 및 동작 확인**
- eureka-service로 만들어서 EUREKA-SERVICE라는 application 이름으로 생성되는 것을 확인 가능

<img src="https://github.com/hoseokjang/spring-cloud-eureka/assets/72066274/b1449f1c-a546-4688-a0a0-9146d340a1a8" width="800" height="400">


---

### **게이트웨이 서비스 개선**

#### **1. client 프로젝트 application.properties 코드 추가**
```
spring.cloud.gateway.mvc.routes[0].id=mgt-service1
spring.cloud.gateway.mvc.routes[0].uri=lb://EUREKA-SERVICE
spring.cloud.gateway.mvc.routes[0].predicates[0]=Path=/user/**
```

#### **2. 실행 및 동작 확인**

- 기존 eureka-service 접속 주소인 localhost:8763/user으로 접근하는 대신
- gateway 설정이 있는 eureka-client의 port로 접근해도 동일한 동작 (localhost:8762/user)


---

## **참고 주소**
https://velog.io/@dawonseo/MSA-%EC%9C%A0%EB%A0%88%EC%B9%B4-Spring-Cloud-Eureka-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0
