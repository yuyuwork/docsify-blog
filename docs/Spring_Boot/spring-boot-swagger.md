
<!-- # Spring Boot集成Swagger -->

?> Spring Boot整合 [Swagger](https://swagger.io/) 生成在线接口文档，[Swagger Github](http://springfox.github.io/springfox/) 


### 快速开始 <!-- {docsify-ignore} -->

**1、添加swagger依赖**

```xml
<!-- swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.7.0</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.7.0</version>
</dependency>
```

**2、添加swagger配置类**

```java
/**
 * Swagger2 配置类
 *
 * @author
 * @date 2021/12/26 11:58
 */
@Configuration
//开启swagger2注解
@EnableSwagger2
public class Swagger2Config{

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    @Bean
    public ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("API文档")//接口文档主题
                .description("API网关接口：http://www.baidu.com")//地址
                .termsOfServiceUrl("www.taobao.com")//路径
                .version("1.0.0")//版本号
                .build();
    }
}
```

**3、Controller接口添加注解**

添加注解用于说明参数和接口相关信息

```java
@ApiOperation("测试接口")
@ApiImplicitParams({
    @ApiImplicitParam(name = "name", value = "姓名", required = false, dataType = "String", paramType = "query"),
    @ApiImplicitParam(name = "password", value = "密码", required = false, dataType = "String", paramType = "query")
})
@RequestMapping(value = "/demo", method = RequestMethod.POST)
public String demo(String name, String password) {
    return "hhh";
}
```

**4、常用的Swagger2注解**

```txt
@Api：修饰整个类，描述Controller的作用
@ApiOperation：描述一个类的一个方法，或者说一个接口
@ApiIgnore：使用该注解忽略这个API

@ApiParam：单个单数描述
@ApiModel：用对象来接收参数
@ApiProperty：用对象接收参数时，描述对象的一个字段
@ApiImplicitParam：一个请求参数
@ApiImplicitParams：多个请求参数

@ApiResponse：HTTP响应其中一个描述
@ApiResponses：HTTP响应整体描述
@ApiError：发生错误返回的信息
```

**5、运行效果示例**

![tu1](https://gitee.com/zhantu/picture-bed/raw/master/blog-images/20220116151708.jpg)

![tu2](https://gitee.com/zhantu/picture-bed/raw/master/blog-images/20220116153011.jpg)

在Swagger2中可以在线调用接口，填写好需要的参数，点击Try it out即可

![tu3](https://gitee.com/zhantu/picture-bed/raw/master/blog-images/20220116153116.jpg)


### Swagger详细配置 <!-- {docsify-ignore} -->

完整的 `Swagger2Config` 配置文件

```java
/**
 * Swagger2 配置类
 *
 * @author
 * @date 2021/12/26 11:58
 */
@Configuration
@EnableSwagger2
public class Swagger2Config {

    /** 配置文件没有配置的话，启动服务会报错 */
    @Value("${server.isProd}")
    private boolean isProd;

    @Bean
    public Docket createRestApi() {
        if(isProd){
            return (new Docket(DocumentationType.SWAGGER_2))
                    //配置说明
                    .apiInfo(apiInfo())
                    //定义组
                    .groupName("Admin API")

                    //全局的操作参数
                    .globalOperationParameters(this.setHeaderToken())

                    .forCodeGeneration(true).pathMapping("/")

                    //选择那些路径和api会生成document
                    .select()

                    //拦截的包路径 .apis(RequestHandlerSelectors.basePackage("org.turing.spring.boot.microsvc.user.controller"))
                    .apis(RequestHandlerSelectors.basePackage("org.turing.spring.boot.microsvc.user.controller"))
                    //配置扫描哪些类或者方法
                    //.apis(RequestHandlerSelectors.withClassAnnotation(RestController.class))
                    //.apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))

                    //拦截的接口路径或者：.paths(paths())  .paths(regex("/api/.*"))
                    .paths(PathSelectors.any())
                    .build()
                    .useDefaultResponseMessages(false)
                    .ignoredParameterTypes(Map.class);
        }else{
            return new Docket(DocumentationType.SWAGGER_2)
                    .apiInfo(apiInfoOnline())
                    .select().paths(PathSelectors.none()).build();
        }
    }

    private ApiInfo apiInfoOnline() {
        return new ApiInfoBuilder()
                .title("")
                .description("")
                .license("")
                .licenseUrl("")
                .termsOfServiceUrl("")
                .version("")
                .contact(new Contact("","", ""))
                .build();
    }

    @Bean
    public ApiInfo apiInfo() {
        //@Bean 可有可无
        Contact contact = new Contact("IT技术中心", "http://www.baidu.com", "22045@qq.com");

        return (new ApiInfoBuilder())
                //接口文档主题
                .title("API文档")
                //地址
                .description("更多Api参考文档参考Confluence http://192.168.16.243/dashboard.action")
                //开源协议
                .license("Apache License Version 2.0").licenseUrl("https://github.com/springfox/springfox/blob/master/LICENSE")

                //路径
                .termsOfServiceUrl("http://www.baidu.com/")
                //联系
                .contact(contact)
                //版本号
                .version("1.0.0")
                .build();
    }

    private List<Parameter> setHeaderToken() {
        //@ApiImplicitParam(name = "Authorization", value = "认证信息", required = true, paramType = "header", defaultValue = "Bearer 467405f6-331c-4914-beb7-42027bf09a01", dataType = "string"),
        ParameterBuilder tokenPar = new ParameterBuilder();
        tokenPar.name("Authorization").defaultValue("32221635-C638-4A8E-8F26-8E7C6B58EAEB--item--487--item--9999--item--111111").description("token").modelRef(new ModelRef("string")).parameterType("header").required(false).build();

        List<Parameter> pars = new ArrayList();
        pars.add(tokenPar.build());
        return pars;
    }

    private Predicate<String> paths(){
        return Predicates.and(PathSelectors.regex("/.*"), Predicates.not(PathSelectors.regex("/error")));
        //return PathSelectors.regex("^/(?!error).*$");
    }

}

```


!> 注意：TODO图床模式项目 待完善！

