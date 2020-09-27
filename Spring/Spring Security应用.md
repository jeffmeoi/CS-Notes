# Spring Security应用

## Spring Boot集成Spring Security

- 添加依赖

  - ```xml
    <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-security</artifactId>
            </dependency>
    ```

  - 添加依赖完成后，SpringBoot自动配置Spring Security，打开网站会redirect到Spring Security默认的login页面。

    - ![image-20200923131942673](Spring%20Security%E5%BA%94%E7%94%A8.assets/image-20200923131942673.png)

  - 默认username为user，password会输出到控制台中。

    - ![image-20200923131917156](Spring%20Security%E5%BA%94%E7%94%A8.assets/image-20200923131917156.png)

## 使用自定义的登录页面

- 编写登录页面```login.html```

  - ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>Login</title>
    </head>
    <body>
    <h1>Login</h1>
    <form method="post" action="/login">
        <div>
            Username:<input type="text" name="username">
        </div>
        <div>
            Password:<input type="password" name="password">
        </div>
        <div>
            <button type="submit">Submit</button>
        </div>
    </form>
    </body>
    </html>
    ```

- 编写登陆成功页面```success.html```

  - ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>Login Success</title>
    </head>
    <body>
        <h1>登陆成功</h1>
        <button onclick="window.location.href='/logout'">退出登录</button>
    </body>
    </html>
    ```

  - 

