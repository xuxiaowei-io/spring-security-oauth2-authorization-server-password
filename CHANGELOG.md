# 更新说明

1. 按照时间倒序排列
2. 自定义反序列化见：类 `OAuth2PasswordAuthenticationToken` 的说明、以及以 `OAuth2PasswordAuthenticationToken` 为前缀的类
3. 新增用户扩展：
    1. 新增 `io.xuxiaowei.security.password.userdetails.UserExpand` 类，继承
       `org.springframework.security.core.userdetails.User`。
    2. `UserExpand` 类中存在 `additionalParameters` 属性（`Map<String, Object>` 类型），用于存储用户附加参数，
       并在 `/oauth2/token` 中返回。
       `additionalParameters` 请勿添加敏感信息，如密码等。
    3. `UserExpand` 类中存在 `userInfo` 属性（`Map<String, Object>` 类型），用于存储用户信息，在系统内部使用。
       请勿使用数字类型（请将数字专为字符串，否则将出现异常: https://github.com/spring-projects/spring-security/issues/4370 ），
       请确保数据可以正常序列化与反序列化。
       可在 `org.springframework.security.oauth2.server.authorization.authentication.OAuth2PasswordAuthenticationToken`
       获取 `userInfo`。
    4. 如果使用 `JWT`，
       可实现 `org.springframework.security.oauth2.server.authorization.OAuth2TokenCustomizer<JwtEncodingContext>` 接口，
       在 `org.springframework.security.core.Authentication` 中获取获取 `userInfo` 添加到 `JWT` `payload`（`claims`）中。
    5. 使用方法：实现 `org.springframework.security.core.userdetails.UserDetailsService#loadUserByUsername(String)`，
       返回值使用 `UserExpand` 对象。
4. 补充用户验证异常：
    1. 可在 `OAuth2AuthorizationServerConfigurer#tokenEndpoint(Customizer)`
       `OAuth2TokenEndpointConfigurer#errorResponseHandler(AuthenticationFailureHandler)` 拦截异常处理
        - 用户不存在异常：`invalid_username`
        - 用户已被禁用：`user_disabled`
        - 用户已过期：`account_expired`
        - 用户凭证已过期：`credentials_expired`
        - 用户已锁定：`account_locked`
        - 用户密码不匹配：`invalid_password`
5. 支持 OAuth2AuthorizationService 使用 jackson 3 反序列化：使用方式参见：
   [JdbcImplConfig.java](https://gitlab.xuxiaowei.com.cn/xuxiaowei-io/spring-security-oauth2-authorization-server-password/-/blob/spring-boot/4.x/src/test/java/io/xuxiaowei/security/password/config/JdbcImplConfig.java)

| 日期         | 版本      | 更新内容                                                                                                                             |
|------------|---------|----------------------------------------------------------------------------------------------------------------------------------|
| 2025-10-30 | 7.0.0-6 | 支持 JDK 25 构建，spring-authorization-server 升级到 7.0.0-RC1，spring-boot 升级到 4.0.0-RC1，支持 OAuth2AuthorizationService 使用 jackson 3 反序列化 |
| 2025-10-30 | 1.1.1-6 | 支持 JDK 25 构建，spring-authorization-server 升级到 1.5.3，spring-boot 升级到 3.5.7                                                         |
| 2025-09-22 | 7.0.0-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 2.0.0-4 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 1.1.1-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 1.0.1-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 0.4.1-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 0.3.1-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 0.3.0-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-22 | 0.2.3-5 | 补充用户验证异常                                                                                                                         |
| 2025-09-19 | 7.0.0-4 | 首次发布 7.x 版本，需要注意 RegisteredClientRepository 的反序列化，详情见源码中的测试类                                                                     |
| 2025-09-12 | 2.0.0-3 | 新增用户扩展                                                                                                                           |
| 2025-09-12 | 1.1.1-4 | 新增用户扩展                                                                                                                           |
| 2025-09-12 | 1.0.1-4 | 新增用户扩展                                                                                                                           |
| 2025-09-12 | 0.4.1-4 | 新增用户扩展                                                                                                                           |
| 2025-09-12 | 0.3.1-4 | 新增用户扩展                                                                                                                           |
| 2025-09-12 | 0.3.0-4 | 新增用户扩展                                                                                                                           |
| 2025-09-12 | 0.2.3-4 | 新增用户扩展                                                                                                                           |
| 2025-09-06 | 2.0.0-2 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-06 | 1.1.1-3 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-06 | 1.0.1-3 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-06 | 0.4.1-3 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-06 | 0.3.1-3 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-06 | 0.3.0-3 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-06 | 0.2.3-3 | 验证 `password` 模式 `scope`                                                                                                         |
| 2025-09-04 | 1.1.1-2 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-09-04 | 1.0.1-2 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-09-04 | 0.4.1-2 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-09-04 | 0.3.1-2 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-09-04 | 0.3.0-2 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-09-04 | 0.2.3-2 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-09-03 | 2.0.0-1 | 删除在 `CLIENT_SECRET_POST` 模式下的 `additionalParameters` 中的客户端的密码，传递用户 `authorities`                                                 |
| 2025-08-25 | 2.0.0   | 首次发布，支持 password 模式，支持默认反序列化，支持自定义反序列化                                                                                           |
| 2025-08-25 | 1.1.1-1 | 新增默认反序列化，支持自定义反序列化                                                                                                               |
| 2025-08-25 | 1.0.1-1 | 新增默认反序列化，支持自定义反序列化                                                                                                               |
| 2025-08-25 | 0.4.1-1 | 新增默认反序列化，支持自定义反序列化                                                                                                               |
| 2025-08-25 | 0.3.1-1 | 新增默认反序列化，支持自定义反序列化                                                                                                               |
| 2025-08-25 | 0.3.0-1 | 新增默认反序列化，支持自定义反序列化                                                                                                               |
| 2025-08-25 | 0.2.3-1 | 新增默认反序列化，支持自定义反序列化                                                                                                               |
| 2025-08-16 | 1.0.1   | 首次发布，支持 `password` 模式，无默认反序列化，需要使用者自己实现                                                                                          |
| 2025-08-16 | 0.4.1   | 首次发布，支持 `password` 模式，无默认反序列化，需要使用者自己实现                                                                                          |
| 2025-08-16 | 0.3.1   | 首次发布，支持 `password` 模式，无默认反序列化，需要使用者自己实现                                                                                          |
| 2025-08-16 | 0.3.0   | 首次发布，支持 `password` 模式，无默认反序列化，需要使用者自己实现                                                                                          |
| 2025-08-16 | 0.2.3   | 首次发布，支持 `password` 模式，无默认反序列化，需要使用者自己实现                                                                                          |
