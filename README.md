# Redis-Knowledge-Points
Redis知识点记录

1. 特性
    - 速度快
    - 持久化
    - 多种数据结构
    - 支持多编程语言
    - 功能丰富
    - 简单
    - 主从复制
2. 典型应用场景
    - 缓存系统
    - 计数器
    - 消息队列系统
    - 排行榜
    - 社交网络
    - 实时系统

3. 注意
    - keys命令一般不再生产环境使用(On)
    - 拒绝长(慢)命令：
        - keys
        - flushall
        - flushdb
        - slow
        - lua script
        - mutil/exec
        - operate big value(collection)
    - 其实不是单线程
        - fysnc file descriptor
        - close file descriptor

4. list
    1. 特点
        - 有序
        - 可以重复
        - 左右两边插入弹出

5. RDB
    1. 什么是RDB
        使用快照方式进行持久化方式
    2. 触发机制-不容忽略方式
    3. 触发机制
    4. 总结
        - RDB是Redis内存到硬盘的快照，用于持久化
        - save通常会阻塞Redis
        - bgsave不会阻塞Redis，但是会fork新进程
        - save自动配置满足任一就会被执行
        - 有些触发机制不容好

6. AOF
    1. RDB现存问题
        - 耗时、耗性能
            - O(n)数据: 耗时
            - fork(): 消耗内存,copy-on-write策略
            - Disk I/O: IO性能
        - 不可控、丢失数据
    2. 什么是AOF
    3. AOF三种策略
        |  命令   | always  |  everysec   | no  |
        |  ----  | ----  |  ----  | ----  |
        | 优点  | 不丢失数据 | 每秒一次fsync丢一秒数据  | 不用管 |
        | 缺点  | IO开销较大，一般的sata盘只有几百TPS | 丢一秒数据  | 不可控 |
    4. AOF重写
        - 减少硬盘占用量
        - 加快恢复速度
        - AOF重写实现两种方式
            - bgrewriteaof
            - AOF重写配置