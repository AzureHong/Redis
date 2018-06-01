1. Redis 遵守BDS协议，高性能Key-Value数据库
    1) 支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用
    2) Key-Value数据库，还提供list、set、zset、hash等数据结构的存储
    3) 支持master-slave模式的数据备份：主从备份

2. 优势
    1) 读写性能极高
    2) 丰富的数据类型
    3) 原子性操作，同事还支持对几个操作全并后的原子性执行
    4) 特性，支持publish/subscribe，通知，key过期等特性

3. Docker 安装redis容器
    拉取镜像    docker pull redis
    运行容器    docker run --name some-redis -d redis
    启动容器    docker exec -it e24b3e0a7df0 redis-cli

4. 命令
    ping 用于检查redis服务是否启动
    redis-cli -h host -p port -a password 远程启动
    示例：redis-cli -h 127.0.0.1 -p 6379 -a "mypass"


    1) 查看命令配置选项，格式： CONFIG GET CONFIG_SETTING_NAME
        示例： CONFIG GET loglevel
               CONFIG GET *
    2) 修改配置 CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
        示例：CONFIG SET loglevel "notice"

    3) Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程

    4) 绑定的主机地址 bind 127.0.0.1

    5) 当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能 timeout 300

    6) 日志级别：debug、verbose（冗长）、notice、warning，默认为verbose

    7) 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid> 命令在连接上指定数据库id      database 16

    8) 指定在多长时间内，有多少次更新操作，就将数据库同步到数据文件，可以多条件配合
        save (seconds) <changes> 默认配置中提供三种分别为：15-1,300-10,60-10000

    9) 默认开启压缩，LZF压缩，若为了节省CPU时间，可以关闭，但数据库文件会变得巨大

    10) 指定本地数据库文件名称，默认为dump.rdb

5. 数据类型
    1) String 一个键最大能存储512MB SET GET 命令
    2) Redis hash 键值对集合    HMSET HEGTALL 命令
    3) List lpush lrange(注意参数指明范围) 命令
    4) Set 是String的无序集合，集合是通过哈希表实现的 sadd smembers 命令
        先添加key，然后再加入各类元素
    5) zset 有序集合 ，不允许重复成员，其内每个元素都会关联一个double类型的分数，redis正是通过分数来为集合中的成员进行从小到大的排序。
    zset的成员是唯一的，但分数是可以重复的

6. 操作详解
    键命令基本语法： command key_name
    示例：SET RUNOOBKEY REDIS
          DEL RUNOOBKEY

7. HyperLogLog 结构
   基数计算

8. 发布pub-订阅sub Redis客户端可以订阅任意数量的频道
    创建订阅频道redisChat SUBSCRIBE redisChat

    psubscribe pattern [pattern] 订阅一个或多个符合给定模式的频道
    pubsub subcommand [args] 查看订阅与发布系统状态
    publish channel message     将消息发送到指定的频道

9. Redis 事务
    事务状态：开始事务→命令入队→执行事务
    1) MULTI 开始一个事务
    2) 多个命令入队列到事务中
    3) EXEC 命令触发事务，一并执行事务中的所有命令

备注：Redis命令执行是原子性的，但是Redis事务的执行并不是原子性的
    事务可以简单理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做的指令回滚，也不会造成后续的指令不做

    示例：
        MULTI
        SET book-name "Mastering C++ 21 days"
        GET book-name
        SADD tag "C++ " "Programming" "Mastering Series"
        SMEMBERS tag
        EXEC

    Discard 取消事务，放弃执行事务块内的所有命令
    EXEC 执行所有事务
    MULTI 标记一个事务块开始
    UNWATCH 取消watch命令对所有key的建设
    watch keys 监视一个或多个key，如果在事务执行之前key被其它命令所改动，那么事务将被打断

10. Lua解释器来执行脚本 EVAL 前缀命令

11. 数据备份与恢复
    SAVE 备份 在redis目录中创建dump.rdb文件
    或者
    BGSAVE 备份，后台运行

    恢复，将dump.rdb备份文件移动到redis安装目录并启动服务
    config 命令可以用于查询redis目录


12. Redis 安全 
    查询是否设置密码 config get requirepass
    设置密码    config set requirepass "123456"

    验证服务密码 AUTH password

13. 客户端连接










    
    

    
    
    
    