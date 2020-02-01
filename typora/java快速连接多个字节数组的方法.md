

#    java连接多个字节数组的最简单方法

网上有java连接数组的方法，然而那些都是针对泛型的，偏偏字节数组不属于引用类型能操控的，于是所有使用泛型的连接数组的方法都失效，只剩下System.arraycopy()这一个可以用的了，然而这个函数参数很多，用起来并没有那么方便，而且每次只能连接两个数组，太麻烦了。

我想到可以用JDK提供的ByteArrayInputStream和ByteArrayOutputStream搞定这个麻烦。秩序要提前计算好最终数组的大小，分配好数组空间之后，刷唰唰往里边按顺序写入想要连接的数组就好了。

```java
byte[] getBytes() throws IOException {
		byte[] groupUID=new BigInteger(""+this.groupUID).toByteArray();
		byte[] sn=new BigInteger(""+this.serialNumber).toByteArray();
		byte[] bodySize=new BigInteger(""+this.bodySize).toByteArray();
		
		ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream(HEADER_SIZE+data.length);
		byteArrayOutputStream.write(groupUID);
		byteArrayOutputStream.write(sn);
		byteArrayOutputStream.write(bodySize);
		byteArrayOutputStream.write(data);
		byteArrayOutputStream.flush();
		
		return byteArrayOutputStream.toByteArray();
	}
```

