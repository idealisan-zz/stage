# Kotlin项目在IDEA中打包成JAR的方法

不用Gradle，直接用项目结构就可以生成JAR。

## ![image-20200209221611799](C:\Users\U\AppData\Roaming\Typora\typora-user-images\image-20200209221611799.png)

例如Run.kt时一个kotlin语言写的函数式源文件，其中包含了main函数。

此时在项目结构里添加带依赖的JAR即可。

![image-20200209221756075](C:\Users\U\AppData\Roaming\Typora\typora-user-images\image-20200209221756075.png)

不过，其中的主类要自己手动填写，不能靠浏览的方式，因为不识别kotlin源文件。

并且MANIFEST.MF的文件夹要填写为项目的目录，而不是源文件的目录，把后边多余的部分删掉即可。

![image-20200209221827322](C:\Users\U\AppData\Roaming\Typora\typora-user-images\image-20200209221827322.png)

如果出现了在VFS里边已存在MANIFEST的错误，

![image-20200209222004634](C:\Users\U\AppData\Roaming\Typora\typora-user-images\image-20200209222004634.png)

把项目文件夹里的META-INF删掉，再次尝试即可。

![image-20200209222122641](C:\Users\U\AppData\Roaming\Typora\typora-user-images\image-20200209222122641.png)