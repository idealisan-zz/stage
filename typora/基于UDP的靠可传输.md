# 基于UDP的靠可传输

## 简单的版本，应答式可靠传输

数据包格式

![image-20200220231615161](C:\Users\U\AppData\Roaming\Typora\typora-user-images\image-20200220231615161.png)

状态位

1.  0x00 - 发起请求
2.  0x01 - 关闭连接
3.  0x02 - 纯确认
4.  0x03 - 确认并有数据
5.  0x04 - 纯数据