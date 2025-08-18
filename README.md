# Spring Authorization Server 支持 password 模式

## 前提条件

- 确保项目可以正常使用 `OAuth` `2.1` 的 `authorization_code`、`refresh_token`、`client_credentials` 模式

## 版本选择

- 快照（只能保留 `90天`）:
  https://central.sonatype.com/repository/maven-snapshots/io/xuxiaowei/security/spring-security-oauth2-authorization-server-password/maven-metadata.xml
- 发布版:
  https://repo1.maven.org/maven2/io/xuxiaowei/security/spring-security-oauth2-authorization-server-password/maven-metadata.xml

| org.springframework.security:spring-security-oauth2-authorization-server | io.xuxiaowei.security:spring-security-oauth2-authorization-server-password |
|--------------------------------------------------------------------------|----------------------------------------------------------------------------|
| 1.0.x、1.1.x、1.2.x、1.3.x、1.4.x、1.5.x                                      | 1.0.x                                                                      |
| 0.4.x                                                                    | 0.4.x                                                                      |
| 0.3.1                                                                    | 0.3.1                                                                      |
| 0.3.0                                                                    | 0.3.0                                                                      |
| 0.2.3                                                                    | 0.2.3                                                                      |

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
