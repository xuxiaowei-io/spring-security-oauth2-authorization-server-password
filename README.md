# Spring Authorization Server 支持 password 模式

## 前提条件

- 确保项目可以正常使用 `OAuth` `2.1` 的 `authorization_code`、`refresh_token`、`client_credentials` 模式

## 版本选择

- 快照（只能保留 `90天`）:
  https://central.sonatype.com/repository/maven-snapshots/io/xuxiaowei/security/spring-security-oauth2-authorization-server-password/maven-metadata.xml
- 发布版:
  https://repo1.maven.org/maven2/io/xuxiaowei/security/spring-security-oauth2-authorization-server-password/maven-metadata.xml

| org.springframework.security:spring-security-oauth2-authorization-server | io.xuxiaowei.security:spring-security-oauth2-authorization-server-password | CI/CD Spring Boot 版本 |
|--------------------------------------------------------------------------|----------------------------------------------------------------------------|----------------------|
| 7.0.x                                                                    | 7.0.0-4                                                                    | 4.0.0-M3             |
| 2.0.x                                                                    | 2.0.0-3                                                                    | 4.0.0-M2             |
| 1.1.x、1.2.x、1.3.x、1.4.x、1.5.x                                            | 1.1.1-4                                                                    | 3.5.5                |
| 1.0.x                                                                    | 1.0.1-4                                                                    | 3.5.5                |
| 0.4.x                                                                    | 0.4.1-4                                                                    | 2.7.18               |
| 0.3.1                                                                    | 0.3.1-4                                                                    | 2.7.18               |
| 0.3.0                                                                    | 0.3.0-4                                                                    | 2.7.18               |
| 0.2.3                                                                    | 0.2.3-4                                                                    | 2.7.18               |

## [更新说明](CHANGELOG.md)

## 使用方法

1. 引入依赖

    ```xml
    
    <dependency>
        <groupId>io.xuxiaowei.security</groupId>
        <artifactId>spring-security-oauth2-authorization-server-password</artifactId>
        <version>版本</version>
    </dependency>
    ```

2. 引入配置

    ```java
    package io.xuxiaowei.security.password.config;
    
    /**
     * @author xuxiaowei
     * @see OAuth2PasswordAuthenticationProvider#init(HttpSecurity, OAuth2AuthorizationServerConfigurer, OAuth2AuthorizationService, UserDetailsService)
     */
    @Configuration(proxyBeanMethods = false)
    public class AuthorizationServerConfig {
    
        @Bean
        @Order(Ordered.HIGHEST_PRECEDENCE)
        public SecurityFilterChain authorizationServerSecurityFilterChain(HttpSecurity http, OAuth2AuthorizationService authorizationService, UserDetailsService userDetailsService) throws Exception {
    
            OAuth2AuthorizationServerConfigurer authorizationServerConfigurer = new OAuth2AuthorizationServerConfigurer<>();
    
            // 你的其他配置 ......
    
            // 一行代码启用 password 模式
            OAuth2PasswordAuthenticationProvider.init(http, authorizationServerConfigurer, authorizationService, userDetailsService);
    
            return http.build();
        }
    
    }
    ```

## 源码

1. 只通过 `Maven` `中央仓库` 发布 `正式版源码`，`Maven` 可使用 `mvn clean -U package dependency:sources -DskipTests=true`
   批量下载项目中使用的源码
2. 自建 GitLab 平台仓库：
   https://gitlab.xuxiaowei.com.cn/xuxiaowei-io/spring-security-oauth2-authorization-server-password.git
    1. 包含所有源码及历史提交记录
    2. 包含完整的 `OAuth` `2.1` `password` 测试用例
    3. 包含完整的 `CI/CD` 配置：自动构建、自动测试、自动发布
