- 分布式存储的难点
	- 悖论：即高性能表现的一致性的矛盾 ![image.jpg](../assets/0364cc9d-784a-40e7-8a9b-c2bd41d4e612-1115003.jpg)
- 一致性
	- 强一致性
	- 弱一致性
- GFS
	- 内容
		- 需求：针对爬虫，MapReduce等应用需要
		- 多个备份部署，一个数据中心
		- 原子操作的分享和恢复.
		- 针对序列化的 对文件的操作
	- 结构
		- client：访问文件的应用
		- 每个文件分为64MB的chunk
		- 每个文件的chunk分布在多个chunk servers上
		- 单个master
		- 分工：master 解决 namespace 域上的wr，worker 解决文件上的
	- Master state ![image.jpg](../assets/5de3b7c6-0e16-4c19-b2a7-f4287595c571-1115003.jpg)
	- **log 和 checkpoints**
	- 读文件的流程
		- C sends (filename offset)给master
		- master找到 对应的chunk，并返回具有最新更新的chunkserver列表 + handle
		- C caches handle + chunkserver list，C发生请求给最近的chunkserver（网络优化）
		- chunk server 读
	- master 定时更新 chunkserver 列表
	- record append 流程 ![image.jpg](../assets/1c367df5-e3c8-4e40-8f17-b748bd0757da-1115003.jpg)
		- C 询问 文件最后的 chunk
			- 如果没有找到primary （租期到了） ![image.jpg](../assets/499bb7ee-6ba7-40f4-9171-1abe54075471-1115003.jpg)
				- 根据log中的内容，主要是写入的记录
				- 如果没有写入记录报错
				- pick 最新的写入记录的 secondary，并更新log，设置最新的secondary作为P
		- M 返回 Primary 和 Secondary Replica的位置
		- C send 数据给所有服务器（内存，还没有落盘，从最近的地址传播）
		- C tells  Primarily to append
		- P 检查租期和剩余空间，并落盘，并tells secondary 落盘
		- P等待所有secondary 同步回复
		- P返回C OK
	- GFS的一致性 ![image.jpg](../assets/850df896-9aa4-4ff9-978f-217b32c38b81-1115003.jpg)
		- 名词解释：
			- define：写入什么东西，就可以读到什么东西
			- consistent：多个replica的数据一致
		- 并行的解释：
			- 并行写：并行写可能会导致覆盖的情况，所有undefined
			- 并行append：由于client 会cache 老的地址，所有可能会导致不一致
		- 错误处理的部分
			-