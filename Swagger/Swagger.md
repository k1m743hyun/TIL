# Swagger


## Swagger란?
- 서버로 요청되는 URL 리스트를 HTML 화면으로 문서화 및 테스트할 수 있는 라이브러리
- *API Spec 문서*를 자동화하여 간편하게 API 문서를 관리하면서 테스트할 수 있음


### Swagger 설정하기


#### Swagger를 dependency에 추가
- pom.xml
```
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```
- build.gradle
```
dependencies {
    implementation 'io.springfox:springfox-swagger2:2.9.2'
    implementation 'io.springfox:springfox-swagger-ui:2.9.2'
}
```


#### SwaggerConfig class 생성
```
package com.k1m743hyun.config;

import java.util.HashSet;
import java.util.Set;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@EnableSwagger2
@Configuration
public class Swagger2Config {

    private static final String API_NAME = "Member";

    private static final String API_DESCRIPTION = "API 명세서(스펙)";

    private static final String API_VERSION = "1.0.0";

    /**
     * localhost:8080/swagger-ui.html
     * @return
     */

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
            .useDefaultResponseMessages(false)
            .consumes(this.getConsumeContentType())
            .produces(this.getProduceContentType())
            .apiInfo(this.apiInfo())
            .select()
            .apis(RequestHandlerSelectors.basePackage("com.k1m743hyun"))
            .paths(PathSelectors.any())
            .build();
    }

    private Set<String> getConsumeContentType() {

        Set<String> consumes = new HashSet<>();
        consumes.add("application/json;charset=UTF-8");
        consumes.add("application/xml;charset=UTF-8");
        consumes.add("application/x-www-form-urlencoded");

        return consumes;
    }

    private Set<String> getProduceContentType() {

        Set<String> produces = new HashSet<>();
        produces.add("application/json");
        produces.add("application/xml");

        return produces;
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
            .title(API_NAME)
            .description(API_DESCRIPTION)
            .version(API_VERSION)
            .build();
    }
}

```
- `.consume()`과 `.produces()`는 각각 `Request Content-Type`, `Response Content-Type`에 대한 설정.(선택)
- `.apiInfo()`는 Swagger API 문서에 대한 설명을 표기하는 메소드.(선택)
- `.apis()`는 Swagger API 문서로 만들기 원하는 BasePackage 경로.(필수)
- `.path()`는 URL 경로를 지정하여 해당 URL에 해당하는 요청만 Swagger API 문서로 만든다.(필수)


### Swagger 사용하기


#### Swagger Annotations


##### `@ApiOperation`
- value, notes를 통해서 해당 API에 대한 설명이나 설정 진행


##### `@ApiParam`
- DTO 변수에 대한 설명 및 다양한 설정을 사용할 수 있음


##### `@ApiOperation`
- 해당 Controller 안의 Method에 대한 설명을 추가


##### `@ApiImplicitParam`
- 해당 API Method 호출에 필요한 Parameter들의 설명을 추가할 수 있음
- `dataType`, `paramType`, `required`의 경우 해당 name의 정보가 자동으로 채워지므로 생략 가능
- `paramType`의 경우 `@RequestParam`은 `query`를, `@PathVariable`은 `path`를 명시해주면 됨
- 해당 Method의 Parameter가 복수일 경우, `@ApiImplictParams`로 `@ApiImplictParam`을 복수 개 사용할 수 있음


##### `@ApiResponse`
- 해당 Method의 Response에 대한 설명을 작성할 수 있음
- 복수 개의 Response에 대한 설명을 추가하고싶다면, `@ApiResponses`를 사용하면 됨


##### `@ApiParam`
- DTO의 field에 대한 설명을 추가할 수 있음


##### `@ApiModelProperty`
- DTO Class Field에 추가하면, 해당 DTO Field의 예제 Data를 추가할 수 있음
- `name`은 생략 가능


##### `@ApiIgnore`
- `@ApiImplictParam`으로 선언하지 않은 Parameter 정보들은 무시할 수 있음
- Method의 `return type` 앞에 명시하면 해당 Method를 Swagger UI에서 아예 노출되지 않게 바꿀 수 있음


#### Default Response Message 삭제
- Default Response Message들을 삭제하고 싶다면, Swagger Config에 있는 `Docket` 객체에 `useDefaultResponseMessage(false)`를 설정해주면 됨
- `useDefaultResponseMessage(false)`를 설정해주면, `@ApiResponse`로 설명하지 않은 `401`, `403`, `404` 응답들이 사라짐


### Swagger UI API 화면 커스텀
- Swagger Config Class에서 `.apiInfo(ApiInfo Type)`을 작성하면, Swagger UI 기본 화면의 API 설명 문구 등을 커스텀할 수 있음


# 출처
- [[SpringBoot] Swagger 설정하기](https://velog.io/@gillog/SpringBoot-Swagger-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
- [[Swagger UI] Annotation 설명](https://velog.io/@gillog/Swagger-UI-Annotation-%EC%84%A4%EB%AA%85)