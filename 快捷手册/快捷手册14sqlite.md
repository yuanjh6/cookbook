# 快捷手册14sqlite
快捷手册sqlite

查询命令和mysql差不多，差异较大的是管理命令，sqlite大多以“.”点标记开头。


## 连接

|    操作or命令    |         含义         |
| ----------------- | ---------------- |
| sqlite3 gee.db | 连接数据库，不存在新建  |

## 数据库

|   操作or命令   |                     含义                      |
| ---------------- | --------------------------------- |
| .help                |  显示帮助文档                         |
| .databases     |  显示数据库名称及对应文件 |
| .output FILE  |  将输出定向到 FILE                |
| .show               |  显示已经设置的值                 |
| .dump              |  以 SQL 格式 dump 数据库   |
| .dump users  |  dump 某张表                         |
| .backup FILE | 备份数据库到文件                  |
| .quit                 |  退出                                         |

## 数据表


| 操作or命令                 | 含义                                         |
| ------------------------ | -------------------------------- |
| .table                           | 查看所有的表                         |
| .schema users           | 显示CREATE语句                   |
| .import FILE TABLE | 将文件的数据导入到表中。 |
| .head ON                    | 查询时显示列名称                 |

## 特性
**设置输出模式**

| 操作or命令                 | 含义                                         |
| ------------------------ | -------------------------------- |
| .mode csv                            | 设置输出模式为csv                         |
| .mode insert                           | 设置输出模式为insert                         |

.mode 支持 csv, column, html, insert, line, list, tabs, tcl 等 8 种模式。


csv:

```
> select * from users
name,age
Tom,18
Jack,20
```
 insert :

```
> select * from users
INSERT INTO "table"(name,age) VALUES('Tom',18);
INSERT INTO "table"(name,age) VALUES('Jack',20);
```

## 参考
SQLite 常用命令：https://geektutu.com/post/cheat-sheet-sqlite.html

SQLite 命令：https://www.runoob.com/sqlite/sqlite-commands.html

sqlite3常用命令&语法：https://blog.csdn.net/linchunhua/article/details/7184439

Sqlite常用命令及基本知识：https://www.cnblogs.com/happyWolf666/p/8232944.html

