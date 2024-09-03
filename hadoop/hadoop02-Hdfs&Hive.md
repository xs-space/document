### 一、HDFS简介

```shell
hdfs简介
  全称Hadoop Distributed File System，是Hadoop框架的一个组件，用于实现分布式存储。
  即：HDFS是大数据分布式存储文件系统，可以存储海量数据。
HDFS存储思路
  由Client（客户端）对文件进行切块（默认128MB/块，可修改），然后由namenode分配到不同的datanode上存储。
HDFS组件的角色划分
  HDFS Client
    负责文件切分
    负责和namenode、datanode交互
    负责副本策略，集群的启动和关闭
  namenode
    1.管理整个HDFS集群
    2.维护和管理元数据
  SecondaryNameNode
    辅助namenode管理元数据
  datanode
    负责具体数据（块）的存储
    负责数据的读写操作
    定时向namenode汇报自己的块信息
HDFS的适用场景
  1.存储海量的大文件
  2.对数据的时效性要求相对较低
  3.一次写入，多次读取
  4.随机读写要求相对较低
```

### 二、namenode如何管理datanode

![1684030996667](assets/1684030996667.png)

### 三、HDFS默认的3副本数是如何存储

![1684031906326](assets/1684031906326.png)

### 四、HDFS-Shell命令

```shell
格式：如下两种方式，除了通用性以外，其它没区别
  hadoop fs -命令名 [选项] [参数]    # 更通用, 可以操作多种文件系统
  hdfs dfs -命令名 [选项] [参数]     # 只能操作HDFS文件系统

# ls命令, 查看指定目录的(子级)信息的
hadoop fs -ls /		# 只能查看单级
hdfs dsf -ls /		# 只能查看单级

hadoop fs -lsr /	# 查看目录的信息(包括所有子集), 已过时, 推荐使用 -ls -R

# mkdir命令, 创建目录的
hadoop fs -mkdir /aa
hadoop fs -mkdir -p /aa/bb/cc

# put命令, 把Linux文件上传到HDFS中.
# 格式: hadoop fs -put Linux文件路径 HDFS目录路径
hadoop fs -put /root/1.txt /aa/bb/cc

# cat命令, 查看HDFS的文件内容的.
hadoop fs -cat /aa/bb/cc/1.txt

# get命令, 从HDFS中下载文件到本地(Linux系统)
# 格式: hadoop fs -get HDFS文件路径 Linux目录路径
hadoop fs -get /aa/bb/cc/1.txt ./

# mv命令, 剪切, 必须是: HDFS <=> HDFS
hadoop fs -mv /aa/bb/cc/1.txt /aa/bb

# cp命令, 复制, 必须是: HDFS <=> HDFS
hadoop fs -cp /aa/bb/cc/1.txt /aa/bb

# rm命令, 删除.  如果是文件夹, 用-rmr, 但是这个命令已过时, 推荐使用: -rm -r
hadoop fs -rmr /aa

# appendToFile命令, 它是唯一一个可以修改HDFS文件内容的命令, 即: 把1个文件数据追加到某个文件中.
# 细节: Linux => HDFS
# hadoop fs -appendToFile Linux文件路径 HDFS文件路径
hadoop fs -appendToFile /root/2.txt  /aa/1.txt
```