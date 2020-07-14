
1. 概念
    - 1.区块链本质上是一个分布式数据库，数据的存储结构类似于链表
    - 2.Block 分为两部分，header和body，block的hash由且仅由header+body决定，body可以存储内容无法改变
    - 3.Header必须包含前一个区块的hash和本身的hash
    - 4.整个区块链中的block hash 是不能重复的
    - 5.为了增加block的获取难度，可以动态设置难度系数，与hash值相比较，(枚举)找出满足要求的nonce

2. 代码Demo
    


