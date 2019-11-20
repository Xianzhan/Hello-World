<!-- TOC -->

- [身份认证（验证）](#身份认证验证)
    - [从配置文件获取用户密码](#从配置文件获取用户密码)
    - [从数据库获取用户密码](#从数据库获取用户密码)
- [权限认证（授权）](#权限认证授权)
    - [编程式授权](#编程式授权)
    - [注解式授权](#注解式授权)
    - [JSP 标签授权](#jsp-标签授权)
- [集成 Web](#集成-web)
    - [Default Filters](#default-filters)
        - [身份验证](#身份验证)
        - [授权相关](#授权相关)
        - [其他](#其他)
- [自定义 Realm](#自定义-realm)
- [整合 Spring](#整合-spring)
    - [JavaSE应用](#javase应用)
    - [Web应用](#web应用)
    - [Shiro权限注解](#shiro权限注解)
- [资源](#资源)

<!-- /TOC -->

[Apache Shiro 官网](http://shiro.apache.org/)

> Apache Shiro是一个强大且易用的Java安全框架,执行身份验证、授权、密码学和会话管理。使用Shiro的易于理解的API,您可以快速、轻松地获得任何应用程序,从最小的移动应用程序到最大的网络和企业应用程序。

- 主要特性

![主要特性](http://shiro.apache.org/assets/images/ShiroFeatures.png)

 1. Authentication（验证）
 2. Authorization（授权）
 3. Session Management（会话管理）
 4. Cryptography（加密）

# 身份认证（验证）

## 从配置文件获取用户密码

- 依赖

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.3.2</version>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.23</version>
</dependency>
```

- 配置文件

shiro.ini

```ini
# 此处只是演示，实际项目中用户/密码会在数据库取得
[users]
lee=123456
```

log4j.properties

```properties
#
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#
log4j.rootLogger=INFO, stdout

log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m %n

# General Apache libraries
log4j.logger.org.apache=WARN

# Spring
log4j.logger.org.springframework=WARN

# Default Shiro logging
log4j.logger.org.apache.shiro=TRACE

# Disable verbose logging
log4j.logger.org.apache.shiro.util.ThreadContext=WARN
log4j.logger.org.apache.shiro.cache.ehcache.EhCache=WARN
```

- HelloShiro.java

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

public class HelloShiro {
    public static void main(String[] args) {
        // 读取配置文件，初始化 SecurityManager 工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");
        // 获取 SecurityManager 实例
        SecurityManager securityManager = factory.getInstance();
        // 把 SecurityManager 实例绑定到 SecurityUtils
        SecurityUtils.setSecurityManager(securityManager);
        // 得到当前执行的用户
        Subject currentUser = SecurityUtils.getSubject();

        // 创建 token 令牌，用户名/密码
        UsernamePasswordToken token = new UsernamePasswordToken("lee",
                "123456");

        try {
            // 登录
            currentUser.login(token);
            System.out.println("身份认证成功");
        } catch (AuthenticationException e) {
            e.printStackTrace();
            System.out.println("身份认证失败");
        }
        // 退出
        currentUser.logout();
    }
}
```

以上就是一简单的 Shiro 实例。

## 从数据库获取用户密码

此过程根据上述代码修改

- 依赖

```xml
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.5.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.39</version>
</dependency>
<!-- org.apache.shiro.util.AbstractFactory.getInstance需要 -->
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```

- 配置文件

jdbcRealm.ini

```ini
[main]
# 使用数据库保存的用户密码
jdbcRealm=org.apache.shiro.realm.jdbc.JdbcRealm

# 数据源
dataSource=com.mchange.v2.c3p0.ComboPooledDataSource
dataSource.driverClass=com.mysql.jdbc.Driver
dataSource.jdbcUrl=jdbc:mysql://localhost:3306/java
dataSource.user=root
dataSource.password=root

# 设置 jdbcRealm 数据源
jdbcRealm.dataSource=$dataSource

# 设置 securityManager 的 realm，多个逗号隔开
securityManager.realms=$jdbcRealm
```

- SQL 文件

在编写 SQL 时先说明下，Shiro 默认是根据提供的数据库，去寻找`users`的**表**，用户名和密码字段为`username`和`password`。格式如下：

```sql
create table `users`(
  `id` int not null,
  `username` varchar not null,
  `password` varchar not null 
)
```

- JdbcShiro.java

```java
// 此处只需改变配置文件即可，其它代码与上述 HelloShrio 代码一致
Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:jdbcRealm.ini");
```

# 权限认证（授权）

最核心的三个要素：权限，角色和用户。

- ShiroUtils.java

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.config.IniSecurityManagerFactory;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.Factory;

public class ShiroUtils {

    public static Subject login(String iniResourcePath, String username, String password) {
        // 读取配置文件，初始化 SecurityManager 工厂
        Factory<SecurityManager> factory = new IniSecurityManagerFactory(iniResourcePath);
        // 获取 SecurityManager 实例
        SecurityManager securityManager = factory.getInstance();
        // 把 SecurityManager 实例绑定到 SecurityUtils
        SecurityUtils.setSecurityManager(securityManager);
        // 得到当前执行的用户
        Subject currentUser = SecurityUtils.getSubject();
        // 创建 token 令牌，用户名/密码
        UsernamePasswordToken token = new UsernamePasswordToken(username, password);
        try {
            // 登录
            currentUser.login(token);
            System.out.println("身份认证成功");
        } catch (AuthenticationException e) {
            e.printStackTrace();
            System.out.println("身份认证失败");
        }
        return currentUser;
    }
}
```

## 编程式授权

**基于角色的访问控制（RBAC）**

- baseRole.ini

```ini
[users]
# 等号右边第一个为密码，后面为角色
lee1=123, role1
lee2=456, role1, role2
```

[角色控制](http://shiro.apache.org/authorization.html#programmatic-authorization)

- RoleTest.java

```java
import com.lee.shiro.util.ShiroUtils;
import org.apache.shiro.subject.Subject;
import org.junit.Test;

public class RoleTest {
    @Test
    public void HasRoleTest() {
        Subject currentUser = ShiroUtils.login("classpath:shiroRole.ini",
                "lee1", "123");
        System.out.println(currentUser.hasRole("role1"));
    }
}
```

Subject 判断当前用户是否具有某角色的方法

- 返回值为布尔类型的

1. `boolean hasRole(String roleIdentifier)`
2. `boolean[] hasRoles(List<String> roleIdentifiers)`
3. `boolean hasAllRoles(Collection<String> roleIdentifiers)`

- 没有返回值但抛异常的

1. `void checkRole(String roleIdentifier) throws AuthorizationException`
2. `void checkRoles(Collection<String> roleIdentifiers) throws AuthorizationException`
3. `void checkRoles(String... roleIdentifiers) throws AuthorizationException`

**基于权限的访问控制**

- basePermission.ini

```ini
[users]
# 等号右边第一个为密码，后面为角色
lee1=123, role1
lee2=456, role1, role2

[roles]
role1=user:select
role2=user:add, user:update, user:delete
```

[权限控制](http://shiro.apache.org/authorization.html#permission-checks)

- PermissionTest.java

```java
import com.lee.shiro.util.ShiroUtils;
import org.apache.shiro.subject.Subject;
import org.junit.Test;

public class PermissionTest {
    @Test
    public void isPermitted() {
        Subject currentUser = ShiroUtils.login("classpath:basePermission.ini",
                "lee1", "123");
        System.out.println(currentUser.isPermitted("user:select"));
    }
}
```

Subject 判断当前用户是否具有某权限的方法

- 返回值为布尔类型的

1. `boolean isPermitted(Permission permission)`
2. `boolean[] isPermitted(String... permissions)`
3. `boolean[] isPermitted(List<Permission> permissions)`
4. `boolean isPermittedAll(String... permissions)`
5. `boolean isPermittedAll(Collection<Permission> permissions)`

- 没有返回值但抛异常的

1. `void checkPermission(String permission) throws AuthorizationException`
2. `void checkPermission(Permission permission) throws AuthorizationException`
3. `void checkPermissions(String... permissions) throws AuthorizationException`
4. `void checkPermissions(Collection<Permission> permissions) throws AuthorizationException`

## 注解式授权

[基于注解授权](http://shiro.apache.org/authorization.html#Authorization-AnnotationbasedAuthorization)

`@RequiresAuthentication`

要求当前 Subject 已经在当前的 session 中被验证通过才能被访问或调用。

```java
@RequiresAuthentication
public void updateAccount(Account userAccount) {
    //this method will only be invoked by a
    //Subject that is guaranteed authenticated
    ...
}
```

等同于

```java
public void updateAccount(Account userAccount) {
    if (!SecurityUtils.getSubject().isAuthenticated()) {
        throw new AuthorizationException(...);
    }

    //Subject is guaranteed authenticated here
    ...
}
```

`@RequiresGuest`

要求当前的 Subject 是一个“guest”，也就是说，他们必须是在之前的 session 中没有被验证或被记住才能被访问或调用。

```java
@RequiresGuest
public void signUp(User newUser) {
    //this method will only be invoked by a
    //Subject that is unknown/anonymous
    ...
}
```

等同于

```java
public void signUp(User newUser) {
    Subject currentUser = SecurityUtils.getSubject();
    PrincipalCollection principals = currentUser.getPrincipals();
    if (principals != null && !principals.isEmpty()) {
        //known identity - not a guest:
        throw new AuthorizationException(...);
    }

    //Subject is guaranteed to be a 'guest' here
    ...
}
```

`@RequiresPermissions("account:create")`

要求当前的 Subject 被允许一个或多个权限，以便执行注解方法。

```java
@RequiresPermissions("account:create")
public void createAccount(Account account) {
    //this method will only be invoked by a Subject
    //that is permitted to create an account
    ...
}
```

等同于

```java
public void createAccount(Account account) {
    Subject currentUser = SecurityUtils.getSubject();
    if (!subject.isPermitted("account:create")) {
        throw new AuthorizationException(...);
    }

    //Subject is guaranteed to be permitted here
    ...
}
```

`@RequiresRoles("administrator")`

要求当前的 Subject 拥有所有指定的角色，如果他们没有，则该方法将不会被执行，而且`AuthorizationException`异常将会被抛出。

```java
@RequiresRoles("administrator")
public void deleteUser(User user) {
    //this method will only be invoked by an administrator
    ...
}
```

等同于

```java
public void deleteUser(User user) {
    Subject currentUser = SecurityUtils.getSubject();
    if (!subject.hasRole("administrator")) {
        throw new AuthorizationException(...);
    }

    //Subject is guaranteed to be an 'administrator' here
    ...
}
```

`@RequiresUser`

注解需要当前的 Subject 是一个应用程序用户才能被注解的类/实例方法访问或调用。一个“应用程序用户”被定义为一个拥有已知身份，或在当前 session 中通过验证确认，或者在之前 session 中的“RememberMe”服务被记住。

```java
@RequiresUser
public void updateAccount(Account account) {
    //this method will only be invoked by a 'user'
    //i.e. a Subject with a known identity
    ...
}
```

等同于

```java
public void updateAccount(Account account) {
    Subject currentUser = SecurityUtils.getSubject();
    PrincipalCollection principals = currentUser.getPrincipals();
    if (principals == null || principals.isEmpty()) {
        //no identity - they're anonymous, not allowed:
        throw new AuthorizationException(...);
    }

    //Subject is guaranteed to have a known identity here
    ...
}
```

## JSP 标签授权

JSP 标签授权需要导入`shiro-web.jar`，并添加标签：

```
<%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>
```

**The guest tag**

用户没有身份验证时显示相应信息，即游客访问信息。

```
<shiro:guest>
    Hi there!  Please <a href="login.jsp">Login</a> or <a href="signup.jsp">Signup</a> today!
</shiro:guest>
```

**The user tag**

用户已经身份验证/记住我登录后显示相应的信息。

```
<shiro:user>
    Welcome back John!  Not John? Click <a href="login.jsp">here<a> to login.
</shiro:user>
```

**The authenticated tag**

用户已经身份验证通过，即 Subject.login 登录成功，不是记住我登录的。

```
<shiro:authenticated>
    <a href="updateAccount.jsp">Update your contact information</a>.
</shiro:authenticated>
```

**The notAuthenticated tag**

用户没有身份验证通过，即没有调用 Subject.login 进行登录，包括记住我自动登录的也属于未进行身份验证。

```
<shiro:notAuthenticated>
    Please <a href="login.jsp">login</a> in order to update your credit card information.
</shiro:notAuthenticated>
```

**The principal tag**

显示用户身份信息，默认调用 Subject.getPrincipal() 获取，即 Primary Principal。

```
Hello, <shiro:principal/>, how are you today?
```

等同于

```
Hello, <%= SecurityUtils.getSubject().getPrincipal().toString() %>, how are you today?
```

- Principal property

```
Hello, <shiro:principal property="firstName"/>, how are you today?
```

```
Hello, <%= SecurityUtils.getSubject().getPrincipal().getFirstName().toString() %>, how are you today?
```

```
Hello, <shiro:principal type="com.foo.User" property="firstName"/>, how are you today?
```

```
Hello, <%= SecurityUtils.getSubject().getPrincipals().oneByType(com.foo.User.class).getFirstName().toString() %>, how are you today?
```

**The hasRole tag**

如果当前 Subject 有此角色将显示 body 内容。

```
<shiro:hasRole name="administrator">
    <a href="admin.jsp">Administer the system</a>
</shiro:hasRole>
```

**The lacksRole tag**

如果当前 Subject 没有角色将显示 body 内容。

```
<shiro:lacksRole name="administrator">
    Sorry, you are not allowed to administer the system.
</shiro:lacksRole>
```

**The hasAnyRole tag**

如果当前 Subject 有任意一个角色（或关系），将显示 body 内容。

```
<shiro:hasAnyRoles name="developer, project manager, administrator">
    You are either a developer, project manager, or administrator.
</shiro:lacksRole>
```

**The hasPermission tag**

如果当前 Subject 有权限将显示 body 内容。

```
<shiro:hasPermission name="user:create">
    <a href="createUser.jsp">Create a new User</a>
</shiro:hasPermission>
```

**The lacksPermission tag**

如果当前 Subject 没有权限将显示 body 内容。

```
<shiro:lacksPermission name="user:delete">
    Sorry, you are not allowed to delete user accounts.
</shiro:hasPermission>
```

# 集成 Web

此处只列举关键代码，若要查看详细代码移步到[GitHub](https://github.com/Xianzhan/lee_project/tree/master/lee_shiro)

- 依赖

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-web</artifactId>
    <version>1.3.2</version>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
</dependency>
```

- web.xml

```xml
<!-- 若使用 classpath 则需在此定义，否则将此去掉 -->
<context-param>
    <param-name>shiroConfigLocations</param-name>
    <param-value>classpath:shiroWeb.ini</param-value>
</context-param>

<listener>
    <listener-class>org.apache.shiro.web.env.EnvironmentLoaderListener</listener-class>
</listener>

<filter>
    <filter-name>ShiroFilter</filter-name>
    <filter-class>org.apache.shiro.web.servlet.ShiroFilter</filter-class>
    <!-- 若配置文件在 /WEB-INF/ 下则在此配置
    <init-param>
        <param-name>configPath</param-name>
        <param-value>/WEB-INF/shiroWeb.ini</param-value>
    </init-param>
    -->
</filter>

<filter-mapping>
    <filter-name>ShiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

- shiroWeb.ini

```ini
# authc、roles 等都代表着一个 Filter，具体意义看 Default Filters
[main]
# 身份认证，若没有登录则跳转到 /login
authc.loginUrl=/login

# 角色认证，若无此角色的用户将跳转到 /unauthorized.jsp
roles.unauthorizedUrl=/unauthorized.jsp

# 权限认证，若无权限则跳转到 /unauthorized.jsp
perms.unauthorizedUrl=/unauthorized.jsp

[users]
lee1=123, admin
lee2=456, teacher
lee3=789

[roles]
admin=user:*, student:*
teacher=student:*

# 该 urls 里的所有 url 都将被右边的过滤器拦截
# ? 匹配单个字符，如：/admin? -> /admin1、/admin2
# * 匹配零个或多个字符，如：/admin* -> /admin、/admin1、/admin123
# ** 匹配多个路径，如：/admin/** -> /admin/、/admin/1、/admin/1/2
[urls]
# 需要 anon 权限才能访问，anon 表示不需要权限，游客
/login=anon

# 首先需要登录，判断有 admin 权限的用户才能访问
/admin=roles[admin]

# 访问 /student 需要角色需要有 teacher
/student=roles[teacher]

# 访问 /teacher 需要有 user:create
/teacher=perms[user:create]
```

- LoginServlet.java

```java
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebServlet(urlPatterns = "/login") // 需 web 3.0
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("login doGet");
        req.getRequestDispatcher("login.jsp").forward(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("login doPost");
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        Subject subject = SecurityUtils.getSubject();
        UsernamePasswordToken token = new UsernamePasswordToken(username, password);
        try {
            subject.login(token);
            resp.sendRedirect("success.jsp");
        } catch (AuthenticationException e) {
            e.printStackTrace();
            req.setAttribute("errorInfo", "用户名或密码错误");
            req.getRequestDispatcher("login.jsp").forward(req, resp);
        }
    }
}
```

## Default Filters

[Default Filters](http://shiro.apache.org/web.html#default-filters) 是 Shiro 提供给我们的 Web 过滤器，他们将拦截各种请求，并判断是否有权限访问。

### 身份验证

- authc

```
org.apache.shiro.web.filter.authc.FormAuthenticationFilter
```

基于表单的拦截器；
如 “/**=authc”，如果没有登录会跳到相应的登录页面登录；
主要属性：usernameParam：表单提交的用户名参数名（ username）；  
passwordParam：表单提交的密码参数名（password）； 
rememberMeParam：表单提交的密码参数名（rememberMe）；  
loginUrl：登录页面地址（/login.jsp）；
successUrl：登录成功后的默认重定向地址； 
failureKeyAttribute：登录失败后错误信息存储 key（shiroLoginFailure）；

- authcBasic

```
org.apache.shiro.web.filter.authc.BasicHttpAuthenticationFilter
```

Basic HTTP 身份验证拦截器，主要属性： 
applicationName：弹出登录框显示的信息（application）；

- logout

```
org.apache.shiro.web.filter.authc.LogoutFilter
```

退出拦截器，主要属性：
redirectUrl：退出成功后重定向的地址（/）;
示例 “`/logout=logout`”

- user

```
org.apache.shiro.web.filter.authc.UserFilter
```

用户拦截器，用户已经身份验证/记住我登录的都可；
示例 “`/**=user`”

- anon

```
org.apache.shiro.web.filter.authc.AnonymousFilter
```

匿名拦截器，即不需要登录即可访问；
一般用于静态资源过滤；
示例 “`/static/**=anon`”

### 授权相关

- roles

```
org.apache.shiro.web.filter.authz.RolesAuthorizationFilter
```

角色授权拦截器，验证用户是否拥有所有角色；
主要属性： loginUrl：登录页面地址（/login.jsp）；
unauthorizedUrl：未授权后重定向的地址；
示例 “`/admin/**=roles[admin]`”

- perms

```
org.apache.shiro.web.filter.authz.PermissionsAuthorizationFilter
```

权限授权拦截器，验证用户是否拥有所有权限；
属性和 roles 一样；
示例 “`/user/**=perms["user:create"]`”

- port

```
org.apache.shiro.web.filter.authz.PortFilter
```

端口拦截器，主要属性：port（80）：可以通过的端口；
示例 “`/test= port[80]`”，如果用户访问该页面是非 80，将自动将请求端口改为 80 并重定向到该 80 端口，其他路径 / 参数等都一样

- rest

```
org.apache.shiro.web.filter.authz.HttpMethodPermissionFilter
```

rest风格拦截器，自动根据请求方法构建权限字符串
（GET=read, POST=create,PUT=update,DELETE=delete,
HEAD=read,TRACE=read,OPTIONS=read, MKCOL=create）
构建权限字符串；
示例 “`/users=rest[user]`”，会自动拼出 “user:read,user:create,user:update,user:delete” 权限字符串进行权限匹配（所有都得匹配，isPermittedAll）；

- ssl

```
org.apache.shiro.web.filter.authz.SslFilter
```

SSL 拦截器，只有请求协议是 https 才能通过；
否则自动跳转会 https 端口（443）；
其他和 port 拦截器一样；

### 其他

- noSessionCreation

```	
org.apache.shiro.web.filter.session.NoSessionCreationFilter
```

不创建会话拦截器，调用 `subject.getSession(false)` 不会有什么问题，
但是如果 `subject.getSession(true)` 将抛出 `DisabledSessionException` 异常；

# 自定义 Realm

在认证、授权内部实现机制中都有提到，最终处理都将交给Real进行处理。因为在 Shiro 中，最终是通过 Realm 来获取应用程序中的用户、角色及权限信息的。通常情况下，在 Realm 中会直接从我们的数据源中获取 Shiro 需要的验证信息。可以说，Realm 是专用于安全框架的 DAO。

[如何编写自定义 Realm](http://shiro.apache.org/realm.html)

- CustomizeRealm.java

```java
import com.lee.shiro.dao.UserDao;
import com.lee.shiro.dao.impl.UserDaoImpl;
import com.lee.shiro.entity.User;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

import java.util.Set;

public class CustomizeRealm extends AuthorizingRealm {
    private UserDao userDao = new UserDaoImpl();

    /**
     * 为当前用户授权
     * 先执行 doGetAuthenticationInfo(token)
     * 然后执行此方法
     *
     * @param principalCollection
     * @return
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        String username = (String) principalCollection.getPrimaryPrincipal();
        SimpleAuthorizationInfo authInfo = new SimpleAuthorizationInfo();
        Set<String> rolesSet = userDao.getRolesByUsername(username);
        Set<String> permissionSet = userDao.getPermissionsByUsername(username);
        authInfo.setRoles(rolesSet);
        authInfo.setStringPermissions(permissionSet);
        return authInfo;
    }

    /**
     * 验证当前登录的用户
     *
     * @param token
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        String username = (String) token.getPrincipal();
        User user = userDao.getByUsername(username);
        AuthenticationInfo authInfo = null;
        if (user != null) {
            authInfo = new SimpleAuthenticationInfo(
                    user.getUsername(), user.getPassword(), "userRealm");
        }
        return authInfo;
    }
}
```

就像我们之前使用 Shiro 自带的 `org.apache.shiro.realm.jdbc.JdbcRealm`，只不过我们需要按照 Shiro 规定的数据库表和字段，自定义则可以自己编写 SQL，接上面 Web 继续编写：

- customizeRealm.ini

```ini
# 有顺序的，依次向下
[main]
# 身份认证，若没有登录则跳转到 /login
authc.loginUrl=/login

# 角色认证，若无此角色的用户将跳转到 /unauthorized.jsp
roles.unauthorizedUrl=/unauthorized.jsp

# 权限认证，若无权限则跳转到 /unauthorized.jsp
perms.unauthorizedUrl=/unauthorized.jsp

# 设置 Shiro 使用自定义 Realm
customizeRealm=com.lee.shiro.realm.CustomizeRealm
securityManager.realms=$customizeRealm

#########
# 分割线 #
#########

# 该 urls 里的所有 url 都将被右边的过滤器拦截
# ? 匹配单个字符，如：/admin? -> /admin1、/admin2
# * 匹配零个或多个字符，如：/admin* -> /admin、/admin1、/admin123
# ** 匹配多个路径，如：/admin/** -> /admin/、/admin/1、/admin/1/2
[urls]
# 需要 anon 权限才能访问，anon 表示不需要权限，游客
/login=anon

# 首先需要登录，判断有 admin 权限的用户才能访问
/admin=roles[admin]

# 访问 /student 需要角色需要有 teacher
/student=roles[teacher]

# 访问 /teacher 需要有 user:create
/teacher=perms[user:create]
```

# 整合 Spring

[整合 Spring](http://shiro.apache.org/spring.html)

## JavaSE应用

spring-shiro.xml 提供了普通 JavaSE 独立应用的 Spring 配置：

- spring-shiro.xml

```xml
<!-- 缓存管理器 使用Ehcache实现 -->  
<bean id="cacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">  
    <property name="cacheManagerConfigFile" value="classpath:ehcache.xml"/>  
</bean>  
  
<!-- 凭证匹配器 -->  
<bean id="credentialsMatcher" class="  
com.github.zhangkaitao.shiro.chapter12.credentials.RetryLimitHashedCredentialsMatcher">  
    <constructor-arg ref="cacheManager"/>  
    <property name="hashAlgorithmName" value="md5"/>  
    <property name="hashIterations" value="2"/>  
    <property name="storedCredentialsHexEncoded" value="true"/>  
</bean>  
  
<!-- Realm实现 -->  
<bean id="userRealm" class="com.github.zhangkaitao.shiro.chapter12.realm.UserRealm">  
    <property name="userService" ref="userService"/>  
    <property name="credentialsMatcher" ref="credentialsMatcher"/>  
    <property name="cachingEnabled" value="true"/>  
    <property name="authenticationCachingEnabled" value="true"/>  
    <property name="authenticationCacheName" value="authenticationCache"/>  
    <property name="authorizationCachingEnabled" value="true"/>  
    <property name="authorizationCacheName" value="authorizationCache"/>  
</bean>  
<!-- 会话ID生成器 -->  
<bean id="sessionIdGenerator"   
class="org.apache.shiro.session.mgt.eis.JavaUuidSessionIdGenerator"/>  
<!-- 会话DAO -->  
<bean id="sessionDAO"   
class="org.apache.shiro.session.mgt.eis.EnterpriseCacheSessionDAO">  
    <property name="activeSessionsCacheName" value="shiro-activeSessionCache"/>  
    <property name="sessionIdGenerator" ref="sessionIdGenerator"/>  
</bean>  
<!-- 会话验证调度器 -->  
<bean id="sessionValidationScheduler"   
class="org.apache.shiro.session.mgt.quartz.QuartzSessionValidationScheduler">  
    <property name="sessionValidationInterval" value="1800000"/>  
    <property name="sessionManager" ref="sessionManager"/>  
</bean>  
<!-- 会话管理器 -->  
<bean id="sessionManager" class="org.apache.shiro.session.mgt.DefaultSessionManager">  
    <property name="globalSessionTimeout" value="1800000"/>  
    <property name="deleteInvalidSessions" value="true"/>  
    <property name="sessionValidationSchedulerEnabled" value="true"/>  
   <property name="sessionValidationScheduler" ref="sessionValidationScheduler"/>  
    <property name="sessionDAO" ref="sessionDAO"/>  
</bean>  
<!-- 安全管理器 -->  
<bean id="securityManager" class="org.apache.shiro.mgt.DefaultSecurityManager">  
    <property name="realms">  
        <list><ref bean="userRealm"/></list>  
    </property>  
    <property name="sessionManager" ref="sessionManager"/>  
    <property name="cacheManager" ref="cacheManager"/>  
</bean>  
<!-- 相当于调用SecurityUtils.setSecurityManager(securityManager) -->  
<bean class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">  
<property name="staticMethod"   
value="org.apache.shiro.SecurityUtils.setSecurityManager"/>  
    <property name="arguments" ref="securityManager"/>  
</bean>  
<!-- Shiro生命周期处理器-->  
<bean id="lifecycleBeanPostProcessor"   
class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>  
```

可以看出，只要把之前的 ini 配置翻译为此处的 spring xml 配置方式即可。

LifecycleBeanPostProcessor 用于在实现了 Initializable 接口的 Shiro bean 初始化时调用 Initializable 接口回调，在实现了 Destroyable 接口的 Shiro bean 销毁时调用 Destroyable 接口回调。如 UserRealm 就实现了 Initializable，而 DefaultSecurityManager 实现了 Destroyable。具体可以查看它们的继承关系。

## Web应用

Web应用和普通 JavaSE 应用的某些配置是类似的，此处只提供一些不一样的配置，详细配置可以参考 spring-shiro-web.xml。 

- spring-shiro-web.xml

```xml
<!-- 会话Cookie模板 -->  
<bean id="sessionIdCookie" class="org.apache.shiro.web.servlet.SimpleCookie">  
    <constructor-arg value="sid"/>  
    <property name="httpOnly" value="true"/>  
    <property name="maxAge" value="180000"/>  
</bean>  
<!-- 会话管理器 -->  
<bean id="sessionManager"   
class="org.apache.shiro.web.session.mgt.DefaultWebSessionManager">  
    <property name="globalSessionTimeout" value="1800000"/>  
    <property name="deleteInvalidSessions" value="true"/>  
    <property name="sessionValidationSchedulerEnabled" value="true"/>  
    <property name="sessionValidationScheduler" ref="sessionValidationScheduler"/>  
    <property name="sessionDAO" ref="sessionDAO"/>  
    <property name="sessionIdCookieEnabled" value="true"/>  
    <property name="sessionIdCookie" ref="sessionIdCookie"/>  
</bean>  
<!-- 安全管理器 -->  
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">  
<property name="realm" ref="userRealm"/>  
    <property name="sessionManager" ref="sessionManager"/>  
    <property name="cacheManager" ref="cacheManager"/>  
</bean>   
```

1. sessionIdCookie 是用于生产 Session ID Cookie 的模板；
2. 会话管理器使用用于 web 环境的 DefaultWebSessionManager；
3. 安全管理器使用用于 web 环境的 DefaultWebSecurityManager。

-- spring-shiro-web.xml

```xml
<!-- 基于Form表单的身份验证过滤器 -->  
<bean id="formAuthenticationFilter"   
class="org.apache.shiro.web.filter.authc.FormAuthenticationFilter">  
    <property name="usernameParam" value="username"/>  
    <property name="passwordParam" value="password"/>  
    <property name="loginUrl" value="/login.jsp"/>  
</bean>  
<!-- Shiro的Web过滤器 -->  
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">  
    <property name="securityManager" ref="securityManager"/>  
    <property name="loginUrl" value="/login.jsp"/>  
    <property name="unauthorizedUrl" value="/unauthorized.jsp"/>  
    <property name="filters">  
        <util:map>  
            <entry key="authc" value-ref="formAuthenticationFilter"/>  
        </util:map>  
    </property>  
    <property name="filterChainDefinitions">  
        <value>  
            /index.jsp = anon  
            /unauthorized.jsp = anon  
            /login.jsp = authc  
            /logout = logout  
            /** = user  
        </value>  
    </property>  
</bean>   
```

1. formAuthenticationFilter 为基于 Form 表单的身份验证过滤器；此处可以再添加自己的 Filter bean 定义；
2. shiroFilter：此处使用 ShiroFilterFactoryBean 来创建 ShiroFilter 过滤器；filters属性用于定义自己的过滤器，即 ini 配置中的`[filters]`部分；filterChainDefinitions 用于声明 url 和 filter 的关系，即 ini 配置中的 `[urls]` 部分。

- web.xml

```xml
<context-param>  
    <param-name>contextConfigLocation</param-name>  
    <param-value>  
        classpath:spring-beans.xml,  
        classpath:spring-shiro-web.xml  
    </param-value>  
</context-param>  
<listener>  
    <listener-class>  
		org.springframework.web.context.ContextLoaderListener  
	</listener-class>  
</listener>   

<filter>  
    <filter-name>shiroFilter</filter-name>  
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>  
    <init-param>  
        <param-name>targetFilterLifecycle</param-name>  
        <param-value>true</param-value>  
    </init-param>  
</filter>  
<filter-mapping>  
    <filter-name>shiroFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>  
```

## Shiro权限注解

```
<aop:config proxy-target-class="true"></aop:config>  
<bean class="  
org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">  
    <property name="securityManager" ref="securityManager"/>  
</bean>   
```

如上配置用于开启Shiro Spring AOP权限注解的支持；`<aop:config proxy-target-class="true">`表示代理类。

接着就可以在相应的控制器（`AnnotationController`）中使用如下方式进行注解： 

```
@RequiresRoles("admin")  
@RequestMapping("/hello2")  
public String hello2() {  
    return "success";  
}   
```

访问 hello2 方法的前提是当前用户有 admin 角色。

当验证失败，其会抛出 `UnauthorizedException` 异常，此时可以使用 Spring 的 `ExceptionHandler`（`DefaultExceptionHandler`）来进行拦截处理：

```
@ExceptionHandler({UnauthorizedException.class})  
@ResponseStatus(HttpStatus.UNAUTHORIZED)  
public ModelAndView processUnauthenticatedException(NativeWebRequest request, UnauthorizedException e) {  
    ModelAndView mv = new ModelAndView();  
    mv.addObject("exception", e);  
    mv.setViewName("unauthorized");  
    return mv;  
}  
```

# 资源

[跟我学 Shiro 目录贴](http://jinnianshilongnian.iteye.com/blog/2018398)