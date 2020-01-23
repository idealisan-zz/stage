

## VB的特点

1.  不区分大小写
2.  只允许字母开头的标识符

## 变量类型

1.  integer
2.  long
3.  single
4.  double
5.  currency
6.  byte
7.  String
8.  boolean
9.  Object
10.  Variant

## 访问控制

1.  public
2.  private

## 生命周期

form1.load->form2,show

static和其他所有的编程语言一样，是与程序一样的生命周期。



## 常量定义

```vb
public const myNumber as integer = 123
```

## 声明变量

dim是dimension的缩写，意思是定义大小。

```vb
dim private myVar as integer
```

## 隐式声明和可变类型

```vb
myVar = "my name"
```

这种简单的写法定义了一个隐式声明的可变类型（variant），可变类型可以存放任何类型的数据。

## 运算符

VB的一个特殊运算符表示方法是“不等于”运算符，采用<>或者><而不是！=

布尔运算符更加不同：

1.  and
2.  or
3.  not
4.  xor 异或
5.  eqv 相等（等价）
6.  imp 隐含

一般用到就前三个

## 条件语句

```vb
if a>b then
	print a
else
	print b
end if
```

```vb
select case myWord
case is myword= hello   'case hello
		print "i said hello"
	case world
		print "i said world"
	case else
		print "i said something unknow"
	end select
```

注意， select语句不需要、也没有break，每个条件一旦成立直接做完语句就跳转到末尾，不会判断下一个、做下一个里边的语句。

## 循环结构

for .... next

```vb
for myAge = 0 to 100 step 1
	print "I'm living."
	if myAge = 100 then 
		print "i'm too old to live."
		exit for
	end if
	print "I'm going to live next year."
next myAge
```

while ... wend

```vb
while myage <=100
	print "I'm living"
	myage = myage + 1
	if myage <100 then 
		print "I'm going to live next year."
	end if
wend
```

do ... loop

```vb
Dim age As Integer
age = 10
Do 
msgbox ("屋里哇啦")
age = age + 1
Loop While age < 3
msgbox ("三岁了，该说点什么了")
```

vba字符串函数列表：

Trim(string)             去掉string左右两端空白

Ltrim(string)            去掉string左端空白

Rtrim(string)            去掉string右端空白

Len(string)              计算string长度

Left(string, x)          取string左段x个字符组成的字符串

Right(string, x)         取string右段x个字符组成的字符串

Mid(string, start,x)     取string从start位开始的x个字符组成的字符串

Ucase(string)            转换为大写

Lcase(string)            转换为小写

Space(x)                 返回x个空白的字符串

Asc(string)              返回一个 integer，代表字符串中首字母的字符代码

Chr(charcode)           返回 string,其中包含有与指定的字符代码相关的字符

function有返回数值，sub没有
function直接写不用call
sub 可以用call方式调用，也可以不用call,但这时要去掉括号，function 不要返回值时也可以这样调用