# ssm框架学习笔记

## 一、java mybatis

### 1.1 第一个mybatis程序

1、文档：https://mybatis.org/mybatis-3/zh/getting-started.html

2、准备

2.1 建立数据库，此处可以在DOS环境下编写SQL语句创建数据库表单（推荐，加深学习）（当然亦可在Navicat创建）

```sql
CREATE DATABASE /*!32312 IF NOT EXISTS*/`mybatis` /*!40100 DEFAULT CHARACTER SET utf8 */;

USE `mybatis`;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` INT(20) NOT NULL,
  `name` VARCHAR(30) DEFAULT NULL,
  `pwd` VARCHAR(30) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT  INTO `user`(`id`,`name`,`pwd`) VALUES (1,'陈情','123456'),(2,'避尘','abcdef'),(3,'随便','111111');
```

