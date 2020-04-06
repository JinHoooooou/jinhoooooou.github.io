---
title: "Spring-Boot,JPA로 질문/답변 게시판 구현(38)"
excerpt: "Swagger 라이브러리를 통한 API 문서화 및 테스트"
classes: wide
categories:
 - Blog
tags:
 - Java
 - Spring
 - Project
 - Git
last_modified_at: 2020-04-06
---



## Spring-Boot, JPA로 질문/잡변 게시판 구현 및 배포

자바지기님 유튜브 강의 정리

### 6-6 Swagger 라이브러리를 통한 API 문서화 및 테스트

* 기존에 html페이지를 만들고 테스트를 할때는 ui가 있기때문에 ui기반으로 테스트를 진행했다.

* HTTP method중 get방식을 사용할때는 API 테스트를 하는데 문제가 없지만 답변하기같은 post방식으로 데이터를 전달하고 그에 대한 응답(response)이 잘 오는지, 또 json기반으로 서비스하고있는 API들을 클라이언트 개발자가 볼 수 있어야하는데 API서버를 만들고 ui가 없는 json API에 대한 테스트를하고 문서화를 하는 작업이 필요한데, 별도의 페이지를 만들어서 진행하는것은 상당히 귀찮은 작업이다. 이를 자동화해주는 swagger라는 라이브러리가있는데 이를 적용해서 우리가 개발한 json API들을 문서화하고 테스트할 수 있다.

* 가장 기본적인 swagger 라이브러리를 적용하기 위해 pom.xml에 라이브러리를 추가하자.

  ```xml
  <!-- pom.xml -->
  ...
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger-ui</artifactId>
      <version>2.2.2</version>
      <scope>compile</scope>
  </dependency>
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-swagger2</artifactId>
      <version>2.2.2</version>
      <scope>compile</scope>
  </dependency>
  ...
  ```

* 스웨거 설정을 위해 Application class에서 설정추가를 해준다.

  ```java
  import static springfox.documentation.builders.PathSelectors.regex;
  
  @EnableSwagger2
  public class MySlippApplication {
    ...
    @Bean
    public Docket newsApi() {
      return new Docket(DocumentationType.SWAGGER_2)
          .groupName("my-slipp")
          .apiInfo(apiInfo())
          .select()
          .paths(PathSelectors.ant("/api/**"))
          .build();
    }
      
    private ApiInfo apiInfo() {
      return new ApiInfoBuilder()
          .title("Spring REST Sample with Swagger")
          .description("Spring REST Sample with Swaager")
          .termsOfServiceUrl("http://www-03.ibm.com/software/sla/sladb.nsf/sla/bm?Open")
          .contact("Niklas Heidloff")
          .license("Apache License Version 2.0")
          .licenseUrl("https://github.com/IBM-Bluemix/news-aggrefator/blob/master/LICENSE")
          .version("2.0")
          .build();
    }
  }
  ```

* 테스트