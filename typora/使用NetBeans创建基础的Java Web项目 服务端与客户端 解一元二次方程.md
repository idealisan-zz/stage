# 使用NetBeans创建Java Web项目 CS模式

## 环境配置

1.  JDK7
2.  NetBeans8.2
3.  GlassFish服务器

## 起步

创建项目

在AD的菜单里选择新建项目。

![image-20200311204641013](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311204641013.png)

在新建项目的对话框中选择Java Web类别的Web应用程序项目，然后单击下一步。

![image-20200311204705352](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311204705352.png)

接下为项目去一个合适的名字，选择项目的位置，会自动创建一个和项目名相同的文件夹，单击下一步进入下一页。

![image-20200311204850641](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311204850641.png)

使用默认的服务器和Java EE配置，然后单击下一步。

![image-20200311205043162](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311205043162.png)

这个项目比较基础，不需要使用任何框架，直接点击完成即可。

![image-20200311205127258](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311205127258.png)

接下来会看到默认的一个HTML页面源代码，这是自动生成的页面，本次尝试并不在意HTML页面。

![image-20200311205307668](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311205307668.png)

右键单击项目名称，选择新建Web服务。

![image-20200311205431631](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311205431631.png)

接下来为这个服务要生成的类取个名字，比如对于一元二次方程ax^2+bx+c=0不妨叫做A2ab；还要提供一个包名，例如math。然后单击完成。

![image-20200311205727217](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311205727217.png)

然后会看到刚刚创建的类的代码。

![image-20200311205824400](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311205824400.png)

其中自动生成的代码如下：

```java
/**
 *
 * @author U
 */
@WebService(serviceName = "A2ab")
public class A2ab {

    /**
     * This is a sample web service operation
     */
    @WebMethod(operationName = "hello")
    public String hello(@WebParam(name = "name") String txt) {
        return "Hello " + txt + " !";
    }
}
```

可以仿照生成的方法编写计算一元二次方程的方法。

```java
    /**
     * 计算一元二次方程
     */
    @WebMethod(operationName = "getA2ab")
    public String getA2ab(@WebParam(name = "a") int a, @WebParam(name = "b") int b, @WebParam(name = "c") int c) {
        //求delta
        int delta=b*b-(4*a*c);
        if(delta<0){
            return "无解";
        }else if(delta==0){
            double ret=(-b+Math.sqrt(delta))/(2*a);
            return "唯一解："+ret;
        }else{
            double ret1=(-b+Math.sqrt(delta))/(2*a);
            double ret2=(-b-Math.sqrt(delta))/(2*a);
            return "两个解分别是：x1="+ret1+"\tx2="+ret2;
        }
    }
```

编码完成后右键单击项目，选择清理并构建。

![image-20200311213739619](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311213739619.png)

再次右键单击项目，选择部署，将项目部署到GlassFish服务器中。

![image-20200311213819438](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311213819438.png)

## 创建客户端程序

### 创建项目

从NetBeans菜单选择新建项目，在弹出的对话框里选择Java类别的Java 应用程序，并单击下一步。

![image-20200311214215252](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311214215252.png)

接下来为项目起个名字，比如Calculator，然后单击完成。

![image-20200311214256529](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311214256529.png)

在客户端项目上右键单击，选择新建Web服务客户端。

![image-20200311214331290](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311214331290.png)

在对话框里选择单击浏览按钮，选择MathTool下的A2ab，并单击确定按钮。

![image-20200311214500318](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311214500318.png)

编写一个包名，例如calc，然后单击完成。

![image-20200311214553663](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311214553663.png)

在左侧的Web服务引用中拖动A2ab到右侧的类代码中，会自动生成代码。

![image-20200311214956611](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311214956611.png)

在main方法中添加测试代码

```java
    public static void main(String[] args) {
        	System.out.println("11x^2+2x+1=0");
		System.out.println(getA2Ab(11, 2, 1));
		
		System.out.println("x^2+2x+1=0");
		System.out.println(getA2Ab(1, 2, 1));
		
		System.out.println("x^2+x-2=0");
		System.out.println(getA2Ab(1, 1, -2));
		
		System.out.println("x-2=0");
		System.out.println(getA2Ab(0, 1, 10));
    }
```

构建客户端并运行，可以看到运行结果。

![image-20200311220342396](%E4%BD%BF%E7%94%A8NetBeans%E5%88%9B%E5%BB%BA%E5%9F%BA%E7%A1%80%E7%9A%84Java%20Web%E9%A1%B9%E7%9B%AE%20%E6%9C%8D%E5%8A%A1%E7%AB%AF%E4%B8%8E%E5%AE%A2%E6%88%B7%E7%AB%AF%20%E8%A7%A3%E4%B8%80%E5%85%83%E4%BA%8C%E6%AC%A1%E6%96%B9%E7%A8%8B.assets/image-20200311220342396.png)