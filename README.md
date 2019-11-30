# MongoDB大数据处理权威指南
 * 书籍阅读
### 基础指令
 * 查看数据库
 ```
 show dbs
 ```
 * 使用指定数据库
 ```
 use [options]
 例如: use test
 ```
 * 查看当前数据库下的全部集合
 ```
 show collections
 ```
#### tips查看当前正在使用的数据库
```
在MongoDB shell中输入db即可
```
 * 在集合中插入数据
 ```
 db.[collection].insert({[obj]})
 db.users.insert({name:'张三',age: 18, height: "170cm"})
 db.users.insert({name:'李四',age: 21, height: "164cm"})
 db.users.insert({name:'王五',age: 23, height: "175cm"})
 ```
 * 查看集合中全部的数据
 ```
 db.users.find()
 ```
 * 按照条件查询
 ```
 db.users.find({name:"王五"})
 db.users.find({height:"175cm"})
 ```
 * 函数sort, limit, skip
 ```
 sort: 对查询返回的结果进行排序, 可以是 1 或者 -1 , 1表示升序排序, -1表示降序排序
 比如按照年龄从大到小排
 db.users.find().sort({age: -1})
 limit: 通过limit()函数可以限制返回结果的最大数目, 表示返回指定数量的数据
 比如查询两条数据:
 db.users.find().limit(2)
 skip: 使用skip()函数忽略掉集合中的前n个数据
 比如跳过1条数据
 db.users.find().skip(1)

 分页的原理
 页面展示几条, 第几页
 比如: 每页十条数据,展示第2页的十条数据
 db.users.find().skip(10).limit(10)
 ```
 * 固定集合长度,也就是数据库大小确定,比如一个集合最多放两百个数据
   * 优点: 一旦固定集合达到设置的大小, 最老的数据将被删除,最新的数据将被添加到末端,保证自然顺序与文档插入的顺序一致.这个类型可以用于日志或自动归档数据
 ```
 db.createCollection("class", {capped: true, size: 50})
 上面的代码表示创建一个班级集合, 最多放大小50的数据
 ```
 * 鉴于固定集合保证乐自然顺序与插入顺序一致,那么在查询时,就不需要再使用任何特殊的参数,任何其他特殊的命令或函数, 除非希望逆转默认结果的顺序.
 ```
 例如: 假设希望找到固定集合中最近10条数据,该集合用于保存登陆失败的记录,可以使用$natrual参数来实现:
 db.class.find().sort({$natural: -1}).limit(10)
 ```
 #### 注意: 已经添加到固定集合中的数据可以被更新, 但大小无法被改变, 也无法删除数据,相反,如果希望数据就必须删除整个集合
 ```
 可以使用max参数来限制插入到固定集合中的文档数目, 比如:
 db.createCollection("citys", {capped:true, size: 20480,max: 100})
 接下来,使用validate()函数检查集合的大小
 db.city.validate()
 ```
 * 获取当个文档
 ```
 db.[collection].findOne([options])
 比如: 查询一个姓名为张三的文档
 db.users.findOne({name:'张三'})
 ```
### 使用聚集命令
 * 使用count()函数返回文档的数目
 ```
 db.[collection].count()
 比如: 查询users有条文档
 db.users.count()
 再比如: 通过条件查询处理符合条件的人有集合
 db.users.find({age: 18}).count()
 ```
 * 使用distinct()函数获取唯一值
 ```
 db.[collection].distinct({[options]})
 比如: 查询出当前users集合中有那些人
 db.users.distinct("name")
 查询处理的数据会去除重复值, 就是如果有两个人同名, 只会显示一个
 ```
 * 使用条件操作符
 66页

