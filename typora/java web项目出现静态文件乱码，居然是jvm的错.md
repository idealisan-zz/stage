## java web项目出现静态文件乱码，居然是jvm的错

项目里的js文件明明是用utf8写的，spring的过滤器和html的标签页都写清楚了utf8编码，但是浏览器收到的js文件确实乱码，导致js加载出来的页面也是乱码。

网上有几种办法：

-   在html页头标注utf8编码
-   在spring的web.xml里使用编码过滤器，使用utf8编码
-   检查js文件自身编写的编码是否为utf8编码
-   JVM的启动参数里添加文件编码参数以确保js文件被作为utf8编码的文档读取

实际上，前几点都很容易做到，而最后一条jvm的启动参数确实难以想到。

实际操作，在eclipse里的servers选项卡里，双击server，点击Overview下方的general下方的open launch configuration连接，在common选项卡的右侧，编码处选择utf8而不是默认的沿袭系统编码。也可以在arguments选项卡里添加-Dfile.encoding=UTF-8参数。

在实际的tomcat的运行环境里，比如我的服务器使用的就不是中文utf8的系统，那么需要修改catalina.sh或者对应的bat文件，在文件的开头或者第一次出现JAVA_OPTS的下方一行加上**-Dfile.encoding=UTF-8**参数。

真是太坑了。