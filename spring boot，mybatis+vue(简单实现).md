# spring boot，mybatis+vue(简单实现)

我们使用spring boot+vue实现一个简单的前后端分离工程。

说明：此工程的目的旨在理解前后端分离的具体实现，前端的项目创建也没有使用npm,当然，如果理解了前后端的真正实现，那么我们就可以在这个项目的基础上进行更复杂项目的创建。前端项目便可在npm上进行。

功能：打开一个网站然后我们获取数据库中的信息。

## 一、前端实现

前端我们使用vue的cdn实现一个展示数据的简单界面，然后使用axios与后端进行交互。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>axios+vue</title>
</head>
<style>

</style>

<body>
    <div id="app">
        <table>
            <tr>
                <td>编号</td>
                <td>姓名</td>
                <td>作者</td>
                <td>价格</td>
                <td>statis</td>
            </tr>
            <tr v-for="item in books">
                <td>{{item.id}}</td>
                <td>{{item.name}}</td>
                <td>{{item.author}}</td>
                <td>{{item.price}}</td>
                <td>{{item.statis}}</td>
            </tr>
        </table>
    </div>

    <!-- 官网提供的 axios 在线地址 -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <!-- 请求地址:http://localhost:8081/getAllBooks
    请求方法:get
    请求参数:无(查询关键字)
    响应内容:显示数据库内容 -->
    <script>
        var app = new Vue({
            el: "#app",
            data() {
                return {
                    books: [
                        {
                            id: 1,
                            name: 'java',
                            author: 'zhangsan',
                            price: '2000',
                            statis: 0
                        },
                        {
                            id: 2,
                            name: 'c++',
                            author: 'lisi',
                            price: '1000',
                            statis: 0
                        },
                        {
                            id: 3,
                            name: 'js',
                            author: 'wangwu',
                            price: '500',
                            statis: 0
                        }
                    ]
                }
            },
            methods: {
                getData() {
                    axios.get("http://localhost:8081/getAllBooks").then((response) => {
                        console.log(response.data);
                        this.books = response.data
                    })
                }
            },
            created() {
                console.log("created")
                this.getData();
            }
        })


    </script>
</body>
</html>
```

## 二、后端实现

后端我们使用spring boot来实现数据库数据的查询，并将接口给前端进行实现。

1、我们创建一个简单的spring boot工程。

pojo下的实体类Book.java

```Java
public class Book {
    private int id;
    private String name;
    private String author;
    private  String price;
    private int status;
    }
    //set/get/toString方法
```

dao层下的mapper接口

```java
package com.zhang.springbootapitest.dao;

import com.zhang.springbootapitest.pojo.Book;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

import java.util.List;

@Mapper
@Repository
public interface BookMapper {
    List<Book> getAll();
}

```

因为这个功能十分简单，我们此处就不编写service层。

实现controller层，为了和前端实现交互我们在此处使用@CrossOrigin注解解决跨域问题。

```Java

//跨域问题解决方法
@CrossOrigin(origins = {"http://localhost:8081", "null"})
@RestController
public class BookController {
    @Autowired
    private BookMapper bookMapper;

    @GetMapping("/getAllBooks")
    public List<Book> selectAllBooks(){
        List<Book> list=bookMapper.getAll();
        return list;
    }
}
```

整合mybatis,在resources目录下创建mybatis包，并创建bookMapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.zhang.springbootapitest.dao.BookMapper">

    <select id="getAll" resultType="com.zhang.springbootapitest.pojo.Book">
       select * from book;
    </select>

</mapper>
```

我们人后配置application.yaml文件（此处我们没有使用默认的.properties）

此处我们还改了端口号防止与前端的接口发生冲突。

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/book_manager?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
server:
  port: 8081

#    整合mybatis
mybatis:
  type-aliases-package: com.zhang.springbootapitest.pojo
  mapper-locations: classpath:mybatis/*.xml
```

## 三、测试

1、首先启动后端程序，然后测试数据是否能够正常读取，我们发现数据库的信息读取正常。

![image-20201121161739075](C:\Users\11499\AppData\Roaming\Typora\typora-user-images\image-20201121161739075.png)

2、启动前端

![image-20201121161837417](C:\Users\11499\AppData\Roaming\Typora\typora-user-images\image-20201121161837417.png)

测试成功。

## 四、总结

我们此处使用了spring boot+mybatis+vue的流行框架进行前后端工程，实现一个

查询数据库信息的项目。