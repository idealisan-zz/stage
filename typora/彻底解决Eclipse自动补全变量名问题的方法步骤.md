# 彻底解决Eclipse自动补全变量名问题的方法步骤

 原文： https://www.cnblogs.com/w-wfy/p/5861274.html 

## 设置增强的补全功能 

一、打开 Eclipse

-> Window -> Preferences

找到Java 下的　Editor 下的　Content Assist ,　右边出现的选项中，有一个Auto activation triggers for Java:会看到只有一个"."存在。表示：只有输入"."之后才会有代码提示,把"."的地方修改成

```
.abcdefghijklmnopqrstuvwsyzABCDEFGHIJKLMNOPQRSTUVWSYZ_
```

点最下面的"OK"来保存设置。 Ps：如果你的版本比较低，不能直接修改的话，就导出配置文件，然后修改配置文件。最后再导入配置文件就可以了。 

## 去掉不想要的自动上屏键

*需要使用Eclipse SDK，一般的Eclipse发行版不行。SDK下载 https://archive.eclipse.org/eclipse/downloads/* 

1.先找到相关的插件： window -> show view -> plug-ins 找到插件org.eclipse.jface.text,右键点击,选择import as Source Project,导入完成后,在你的workspace就可以看到这个project了

2.修改代码

在src/org/eclipse/jface/text/contentassist/CompletionProposalPopup.java文件中,找到这样一行代码 

```java
char[] triggers = t.getTriggerCharacter(); 
if(contains(triggers,key))
```

在那行if判断里面,eclipse会判断key(就是你按下的键)是否在triggers中,如果是,那就触发下面的第一行提示上屏的代码.所以我们要做的就是把空格和=号排除就可以了: 

```java
if(key != '=' && key != 0x20 &&contains(triggers,key)){ 
.........
}
```

代码修改成这样后，提示的时候按下空格或者等号，提示就会没掉，也不会自动补全了咯！！！

3.把修改好的org.eclipse.jface.text导出

右键点击你的workspace里的org.eclipse.jface.text,选择export-->Deployable plugins and

fragments, next,destination 选择archive file，然后finish.你就可以在zip文件里看到生成好的jar ,用它替换掉eclipse/plugins里面的同名jar包,就可以了。

 

三、注意：MyEclipse无法导入插件的源码工程，可以下载对应版本的Eclipse，重新编译得到插件后再覆盖MyEclipse里的插件即可。

我这里有一个MyEclipse10修改好了的jar包。。如果你的版本跟我一样的话，直接把这个jar包拷到plugins下就可以了。下载后解压，有一个是修改好的jar，一个是没修改的jar，如果哪天你想换回来，把那个没修改过的jar复制回去就行了。

注意了，版本不一样的记得自己去修改！！！我这个只有myeclipse10可以用的！！

 

之所以使用Eclipse是因为Mycelipse中没有反编译插件JODE，而且有的这个插件反而跟Myeclipse兼容性不好，所以使用Eclipse进行反编译，并且可以直接在Eclipse中进行修改。

 

**如何查找MyecIipse所对应Eclipse的版本：**

  打开Myeclipse-->Window-->about Myeclipse enterprise workbench-->detail information --->plug-ins ,在feature中找到org.eclipse.jface.text，然后看对应的Provider，比如此时的是Eclipse org,再然后就在Features中找到对应的Eclipse org所对应的版本即可，从而下载eclipse修改上述的例子。