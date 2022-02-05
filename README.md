- json简单配置
- muduo安装
- mysql安装，建表
- https://blog.csdn.net/qq_35078688/article/details/121405907
- nginx安装
- Redis安装





# 服务端

- chatserver：网络层
- chatservice：业务层
- 数据库MySQL增删改查操作封装
- 
- 注册功能 reg
- 登录功能 login
- 注销 
- 客户端异常退出 clientCloseException
- 一对一聊天 onechat
- 离线消息处理
- 服务端异常退出（更新所有用户状态为offline）
- 
- 添加好友，**返回用户的所有好友**
- 
- 创建群
- 加入群
- 查询所在群的信息

- 群聊天

# 客户端

- 登录
- 注册
- 退出
- 主线程负责和用户交互，子线程负责接收数据



- 添加好友、一对一聊天

- 
- 创建群组、加入群组、群组聊天

# Ngnix

- 

# Redis

- 发布-订阅

# 问题

- chatservice是个单例模式

- ```cpp
  //登陆业务中
  //点对点聊天需要是个长连接，因为用户不知道什么时候收到，只能服务器推，所以要长连接
  //记录下来一个用户一个connection
  
  // 存储在线用户的通信连接
  unordered_map<int, TcpConnectionPtr> _userConnMap;
  
  //server中onmessage会在多个线程中进行读写处理，多线程环境中被回调
  //多个用户上线下线存在线程安全问题
  //解决：定义互斥锁
      
  // 登录成功，记录用户连接信息
  {
      lock_guard<mutex> lock(_connMutex);
      _userConnMap.insert({id, conn});
  }
  //有作用域就不需要加速解锁，出作用域直接解锁
  ```

- Friend表中是联合主键，两个键不能同时重复

- ```mysql
  select a.id,a.name,a.state from user a inner join friend b on b.friendid = a.id where b.userid=%d", 
  
  查满足userid的所有朋友 的信息（user表中）
  ```


# 修改bug

- 离线消息表，userid设置成主键，导致离线消息不能存储多条，不设置主键，innodb自动设置主键



- 上线之后咋接收消息，什么时候发送