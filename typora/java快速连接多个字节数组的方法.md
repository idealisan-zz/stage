

#    java连接多个字节数组的最简单方法

网上有java连接数组的方法，然而那些都是针对泛型的，偏偏字节数组以及其他几个原始数据类型的数组不属于泛型方法能操控的，于是所有使用泛型的连接数组的方法都失效，只剩下System.arraycopy()这一个可以用的了，然而这个函数参数很多，用起来并没有那么方便，而且每次只能连接两个数组，太麻烦了。

可以用JDK提供的ByteArrayOutputStream搞定这个麻烦。只需要提前计算好最终数组的大小，分配好数组空间之后，刷唰唰往里边按顺序写入想要连接的数组就好了。

```java
//实际上不会因为数组读写而抛出异常
byte[] getStrunctureBytes(byte[] data) throws IOException {
		byte[] groupUID=new BigInteger("0123").toByteArray();
		byte[] sn=new BigInteger("456").toByteArray();
		byte[] bodySize=new BigInteger("789").toByteArray();

		//上边三个字节数组用16字节空间+data的大小，以此大小分配缓冲区避免反复分配和复制内存
		ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream(16+data.length);
		byteArrayOutputStream.write(groupUID);
		byteArrayOutputStream.write(sn);
		byteArrayOutputStream.write(bodySize);
		byteArrayOutputStream.write(data);
		byteArrayOutputStream.flush();//字节数组流的冲刷其实没有用，但是使用流的时候及时冲刷是好习惯
		
		return byteArrayOutputStream.toByteArray();
	}
```

