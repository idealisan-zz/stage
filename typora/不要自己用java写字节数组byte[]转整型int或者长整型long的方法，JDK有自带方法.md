# 不要自己用java写字节数组byte[]转整型int或者长整型long的方法，JDK有自带方法

无论是从Integer还是Long还是Byte的类里都找不到把字节数组转换为数字的方法，无奈自己写，吭哧吭哧写完了之后发现原来BigInteger里边有从字节数组转换的构造函数，网上致我们吭哧吭哧找也没找到，别人也都是自己写的转换函数，真是费神不讨好啊。

看了一下BigInteger的源代码，叹为观止。不过我只用得到转换成字节数组和从字节数组构造这两个方法。

```java
package com.idealisan.exchange.bytes;

import java.math.BigInteger;

public class Bytes {

	/**
	 * Big endian converter. bytes[0] will be placed at the highest byte of the long
	 * type value. If you want a little endian result, use
	 * <code>Long.reverseBytes(long i)</code> to do that easily.
	 * 
	 * @param bytes
	 * @return a long type number converted from 8 bytes of that array using big
	 *         endian strategy.
	 */
	public static long toLong(byte[] bytes, int offset) {
		long ret = 0;

		for (int i = 0; i < 8; i++) {
			ret = ret << 8;
			ret = ret | bytes[offset + i];
		}

		return ret;
	}
	
	
	/**
	 * Big endian converter. bytes[0] will be placed at the highest byte of the integer
	 * type value. If you want a little endian result, use
	 * <code>Integer.reverseBytes(int i)</code> to do that easily.
	 * 
	 * @param bytes
	 * @return a integer type number converted from 8 bytes of that array using big
	 *         endian strategy.
	 */
	public static int toInteger(byte[] bytes, int offset) {
		long ret = 0;
		byte[] temp=new byte[8];
		System.arraycopy(bytes, offset,temp, 4, 4);
		return (int) toLong(temp, 0);
	}
	
	
	public static void main(String[] args) {
		byte[] bytes = new byte[] { 0x01, 0x01,  0x01,  0x01 };
		
		System.out.println(toInteger(bytes, 0));
		
		BigInteger bigInteger = new BigInteger(bytes);
		System.out.println(bigInteger);
	}
}

```

