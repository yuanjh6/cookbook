# 快捷手册13mongodb
快捷手册mongodb

## 用户


|                操作or命令                |               含义                |
| --------------------------------- | -------------------------- |
| db.auth('name', 'pwd')         | 用户认证                        |
| show users                              | 显示当前库所有用户 |
| db.getUser('admin')              | 查看用户详情                |
| db.addUser('name', 'pwd')  |  增加或修改用户密码 |
| db.dropUser('admin')           | 删除用户                      |
| db.logout()                              | 退出当前登录              |

## 数据库相关操作


|                                操作or命令                                 |                              含义                              |
| ------------------------------------------------------- | -------------------------------------------- |
| use < dbName >;                                                      | 进入到 <dbName> 库，不存在则创建 |
| db;                                                                                | 查看当前所在库名                                   |
| db.help();                                                                   | 数据库命令帮助                                       |
| db.dropDatabase();                                                | 删除当前所在的库                                   |
| db.stats();                                                                   | 当前库状态                                               |
| db.repairDatabase();                                              | 修复当前数据库                                       |
| db.getMongo();                                                        | 查看当前db的链接机器地址                  |
| db.printCollectionStats();                                       | 显示当前db所有聚集索引的状态          |
| db.cloneDatabase("10.0.0.1");                              | 克隆 10.0.0.1 的库到本机                       |
| db.copyDatabase("db1", "db2", "127.0.0.1");   | 将本机 db1 库复制到 bd2 库中             |

## 集合操作
集合操作需先进入库中


|                                    操作or命令                                     |                                              含义                                              |
| ------------------------------------------------------------- | ----------------------------------------------------------------- |
| db.test.find({id:10})                                                          | 返回test数据集ID=10的数据集                                             |
| db.test.find({id:10}).count()                                           | 返回test数据集ID=10的数据总数                                         |
| db.test.find({id:10}).limit(2)                                           | 返回test数据集ID=10的数据集从第二条开始的数据集     |
| db.test.find({id:10}).skip(8)                                            | 返回test数据集ID=10的数据集从0到第八条的数据集       |
| db.test.find({id:10}).limit(2).skip(8)                            | 返回test数据集ID=1=的数据集从第二条到第八条的数据 |
| db.test.find({id:10}).sort()                                              | 返回test数据集ID=10的排序数据集                                     |
| db.test.find().sort({'ID':-1})                                           | select * from test order by ID desc                                     |
| db.test.distinct('name',{'ID':{$lt:20}})                         | select distinct(name) from test where ID<20                  |
| db.test.update({'name':'foobar'},{$set:{'age':36}}) | update test set age=36 where name='foobar'                |
| db.test.update({'name':'foobar'},{$inc:{'age':3}})   | update test set age=age+3 where name='foobar'          |
| db.test.findOne(\[query\])                                             | 返回符合条件的一条数据                                                       |
| db.test.getDB()                                                                   | 返回此数据集所属的数据库名称                                           |
| db.test.getIndexes()                                                         | 返回些数据集的索引信息                                                       |
| db.test.remove(query)                                                    | 在数据集中删除一条数据                                                       |
| db.test.renameCollection(newName)                        | 重命名些数据集名称                                                               |
| db.test.save(obj)                                                               | 往数据集中插入一条数据                                                       |
| db.test.stats()                                                                     | 返回此数据集的状态                                                               |
| db.test.storageSize()                                                        | 返回此数据集的存储大小                                                       |
| db.test.totalIndexSize()                                                   | 返回此数据集的索引文件大小                                               |
| db.test.totalSize()                                                             | 返回些数据集的总大小                                                           |
| db.test.update(query,object\[,upsert_bool\])         | 在此数据集中更新一条数据                                                   |
| db.test.validate()                                                               | 验证此数据集                                                                           |
| db.test.getShardVersion()                                               | 返回数据集共享版本号                                                           |

## 索引

|                                                 操作or命令                                                 |                                 含义                                  |
| ----------------------------------------------------------------------------- | ------------------------------------------------- |
| db.foo.ensureIndex({firstname: 1, lastname: 1}, {unique: true}); | 增加索引：1(ascending),-1(descending) |
| db.foo.getIndexes()    , db.foo.getIndexKeys()                                     | 查看索引信息                                                  |
| db.user_addr.dropIndex('Al.Em_1')                                                       | 根据索引名删除索引                                      |


## 参考
mongodb常用命令:https://segmentfault.com/a/1190000007550421

MongoDB 数据库常用操作命令:1987.name/1755.html

MongoDB常用命令：https://www.jianshu.com/p/fffb581bb1a9

MongoDB常用命令总结：https://blog.csdn.net/manduner/article/details/100014489

MongoDB常用命令：https://blog.csdn.net/lvyongyi/article/details/52788138

