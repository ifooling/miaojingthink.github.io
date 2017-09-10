## 安装mongoDB
 - 下载：[https://www.mongodb.com/download-center#enterprise](https://www.mongodb.com/download-center#enterprise)
 - 安装
 - 创建文件夹：进入安装路径 `C:\Program Files\MongoDB\Server\3.4\bin`，创建 **data** 文件夹，进入 **data** 文件夹，创建 **db** 文件夹和 **log** 文件夹，在 **log** 文件夹下创建 **MongoDB.log**文件
 - 以Windows Service的方式启动MongoDB：管理员方式启动 **cmd**，，cd到 **bin目录** `cd C:\Program Files\MongoDB\Server\3.4\bin`,执行命令`mongod --dbpath "C:\Program Files\MongoDB\Server\3.4\bin\data\db" --logpath "C:\Program Files\MongoDB\Server\3.4\bin\data\log\MongoDB.log" --install --serviceName "MongoDB"`.此命令会创建一个名称为 **MongoDB** 的windows系统服务
 - 启动 **MongoDB** 服务：`net start mongodb`

## 创建数据库（带数据库认证）
**mongoDB** 默认不开启认证，即不使用用户名和密码即可访问数据库。首先介绍一下如何开启数据库认证。

### 数据库认证相关
- **开启数据库认证**：定位到注册表`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MongoDB`,编辑字符串 **ImagePath** 在'数值数据'后追加`--auth`，重启 **MongoDB** 服务即可.
- **关闭数据库认证**：定位到注册表`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MongoDB`,编辑字符串 **ImagePath** 删除'数值数据'后的`--auth`，重启 **MongoDB** 服务即可.

### 创建超级管理员用户
 - 关闭数据库认证
 - 管理员方式启动 **cmd**，cd到 **bin目录** `cd C:\Program Files\MongoDB\Server\3.4\bin`，执行命令 `mongo`
 - 切换到 **admin** 数据库 `use admin`,创建超级管理员`db.createUser({"user" : "root","pwd": "root",roles: [{ role: "root", db: "admin"}]})`
 - 开启数据库认证
 - 切换到 **admin** 数据库 `use admin`，使用上一步创建的 **root** 用户登录 `db.auth("root", "root")`,返回'1'则代表登录成功
 - 创建数据库，如创建名称为 **demo** 的数据库 `use demo`
 - 创建 **demo** 数据库用户 `db.createUser({"user" : "demoUser","pwd": "demoUser",roles: [{ role: "dbOwner", db: "demo"}]})`
 - 验证数据库及用户是否创建成功 `mongo --host localhost -u demoUser-p demoUser --authenticationDatabase demo`,控制台出现 **connecting to: mongodb://localhost:27017/**，则表示创建成功
