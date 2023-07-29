# 1、pip

| 命令                                   | 说明                               |
|:------------------------------------ |:-------------------------------- |
| pip install SomePackage whl          | 通过wh文件离线安装扩展库                    |
| pip install package package2         | 依次(在线)安装 package1、 package2等扩展模块 |
| pip install -r requirements. txt     | 安装 requirements. txt文件中指定的扩展库    |
| pip download SomePackage[==version]  | 下载扩展库的指定版本,不安装                   |
| pip freeze requirements txt          | 以 requirements的格式列出已安装模块         |
| pip list                             | 列出当前已安装的所有模块                     |
| pip install SomePackagel==version]   | 在线安装 Some Package模块的指定版本         |
| pip install--upgrade SomePackage     | 升级 Some Package模块                |
| pip uninstall SomePackage[==version] | 卸载 SomePackage模块的指定版本            |

安装通过国内镜像更快

清华：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple  包名

阿里云 ：pip install -i http://mirrors.aliyun.com/pypi/simple

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

华中理工大学：http://pypi.hustunique.com/

山东理工大学：http://pypi.sdutlinux.org/ 

豆瓣：http://pypi.douban.com/simple/

# 2、程序的格式框架

## 缩进

- 缩进表达程序的格式框架
- 严格明确：缩进是语法的一部分，缩进不正确程序运行错误
- 所属关系：表达代码间包含和层次关系的唯一手段
- 长度一致：程序内一致即可，一般用四个空格或一个tab

## 注释

- 用于提高代码可读性的辅助性文字
- 单行注释：以#开头，其后内容为注释
- 多行注释：以' ' ' 开头和结尾

## 命名和保留字

命名规则：大小写字母、数字、下划线和中文等字符组合

注意事项：大小写敏感、首字符不能是数字、不能和保留字相同

保留字：![image-20211014210125701](C:\Users\lilia\AppData\Roaming\Typora\typora-user-images\image-20211014210125701.png)

# 3、数据类型

- type(变量名) 可以查看变量类型

## 1 整数类型

- 可正可负，没有取值范围限制

- 四种进制变现形式：
  
  - 十进制：101，99
  - 二进制：以0b或0B开头，0b101,-0B101
  - 八进制：以0o或0O开头
  - 十六进制：以0x或0X开头

## 2 浮点型

- 取值范围为-10^307至10^308
- 浮点数可以采用科学计数法表示，使用字母e或E作为幂的符号，以10为基数
- 浮点数之间的计算存在不确定尾数，不是BUG

## 3  复数类型

**定义j=√-1,以此为基础,构建数学体系**
**a+bj被称为复数,其中,a是实部,b是虚部**
**z=1.23e - 4 + 5.6e1 + 89j**
**z.real 获得实部**
**z.imag 获得虚部**

## 4 数值运算操作符

- +
- -
- *
- /
- //      取整数，返回商的整数部分 9//2 输出结果 4 , 9.0//2.0 输出结果 4.0 
- %       取余
- **        指数 a**b 为10的20次方 
- x op =y    增强符，op可以是前面所有的符号

混合运算时，优先级顺序为： ** 高于 * / % // 高于 + - 

**算数运算符在字符串里的使用** 

- 如果是两个字符串做加法运算，会直接把这两个字符串拼接成一个字符串。 

- 如果是数字和字符串做加法运算，会直接报错。 

- 如果是数字和字符串做乘法运算，会将这个字符串重复多次

```python
# 多个变量赋值(使用逗号分隔) 

>>> num1, f1, str1 = 100, 3.14, "hello" 
```

## 5 函数

| 函数及使用             | 说明                               |
| ----------------- | -------------------------------- |
| abs(x)            | 取绝对值                             |
| divmod(x,y)       | 商余，同时输出商和余数，divmod(10,3)结果为（3，1） |
| pow(x,y[,z])      | 幂余，（x**y)%z,[..]表示z可以省略          |
| round(x[,d])      | 四舍五入，d是保留小数位数，默认为0               |
| max(x1,x2,...,xn) | 最大值                              |
| min(x1,x2,...,xn) | 最小值                              |
| int(x)            | 将X变成整数，舍弃小数部分                    |
| float(x)          | 将X变成浮点数，增加小数部分                   |
| complex(x)        | 将X变成复数，增加虚数部分，complex（4）结果为4+0j  |

## 6、字符串

- 字符串由一对单引号或一对双引号一对三引号（表示多行字符串）表示
- 字符串是字符的有序序列，可以对其中的字符进行索引

### 单引号（’）

把一段文本用单引号「'」包围起来，它就变成了字符串，和数一样是一个值。

### 双引号（'')

把一段文本用双引号「"」包围起来，它就变成了字符串，和数一样是一个值

其实单引号的字符串和双引号的字符串是一样的，不过为什么Python要支持单引号又支持双引号呢？

因为有时候我们想表达说话的话就可以这样使用了

```python
'"如果能写小说就好了"，他说。'
```

### 三引号

这三引号是来干嘛的呢？如果你要表示一个很长很长的字符串，那么这个三引号就可以派上用场了，因为它支持跨多行，而且在这个三引号的字符串里面你要用单引号和双引号都无所谓。

### 转义字符

不想又使用双引号单引号的时候，可以使用转义字符

```python
'\' 如果能写小说就好了\',他说。 '
```

### 原始字符串

有一些符号是代表特殊意义的，比如说 「\n」就代表换行

可是，有时候 Python 自作聪明了，比如说我们有这么一个在 c 盘下的一个叫做niubi的文件夹「C:\niubi」，那么我们这样打印的话：

print("C:\niubi")

结果你也知道了，路径被拆掉了。

所以我们可以在字符串前面加一个r,表示我们不需要python帮我们转义

```python
print(r"C:\niubi")
```

当然python中还有一些其他的前缀

例如b：代表这是Bytes数据

还有u：表示后面的字符串使用Unicode编码，因为中文也有对应的Unicode编码，所以常用于中文字符串的                前面，防止出现乱码。

### 索引

返回字符串中的单个字符

```py
"这是一个字符串"[0]
TempStr[-1]
#-1即倒数第一个
```

### 切片

> 切片注意是左闭右开

```pytho
str = "这是一个字符串"
print(str[1:])#从第二个到最后一个
print(str[1:2])#从第二个到第三个
print(str[M:N])#M缺失表示到开头，N缺失表示到末尾

str='123456789'
print(str[::-1])#结果为987654321
print(str[::2])#结果为13579，第三位为步长
print(str[-1])#输出倒数第一个，9
```

### 字符串操作符

| x+y        | 连接两个字符串x和y                |
| ---------- | ------------------------- |
| n\*x或者x\*n | 复制n次字符串x                  |
| x in s     | 如果x是s的字串，返回True，否则返回False |

### 字符串处理函数

| 函数            | 说明                   |
| ------------- | -------------------- |
| len(X)        | 返回字符串的长度             |
| str(x)        | 任意类型x所对应的字符串形式       |
| hex(x)或oct(x) | 整数x的十六进制或八进制小写形式字符串  |
| chr(u)        | x为Unicode编码，返回其对应的字符 |
| ord(x)        | x为字符，返回其对应的Unicode编码 |

### 字符串处理方法

| 方法                           | 说明                                                                  |
|:---------------------------- | ------------------------------------------------------------------- |
| str.lower()或str.upper()      | 返回字符串的副本，全部大写/小写                                                    |
| str.split(sep=None)          | 返回一个列表，由str根据sep被分隔的部分组成<br />"A,b,C".split(",")，结果为['A', 'b', 'C'] |
| str.count(sub)               | 返回子串sub在str中出现的次数                                                   |
| str.replace(old,new)         | 返回字符串str副本，所有old子串被替换为new                                           |
| str.center(width[,fillchar]) | 字符串根据str根据宽度width居中，fillchar可选，空格会用fillchar填补                       |
| str.strip(chars)             | 从str中去掉在其左侧和右侧chars中列出的字符<br />"=python=".strip("=np")  结果为：ytho    |
| str.join(iter)               | 在iter变量除最后元素外每个元素后增加一个str<br />",".join("12345")  ,结果为1,2,3,4,5     |

# 4、数组

## 1 序列

- 序列是有先后关系的一组元素
- 序列是一维元素向量，元素类型可以不同

| 方法                        | 说明                 |
| ------------------------- | ------------------ |
| s.index(x)或s.index(i,j,x) | 返回序列S从i到j第一次出现x的位置 |
| x in s                    |                    |
| x not in s                |                    |

## 2 元组

元组是一种序列的扩展类型

- 原则是一种序列类型，一旦创建就不能更改

- 使用小括号或tuple()创建，元素间用逗号,分隔

- 可以使用或不使用小括号

- ```pyhton
  creat = "12","123"
  creat=("12","123")
  ```

- 元组继承了序列的所有通用操作

- 元组因为创建后不能修改，所以没有特殊操作

## 3 列表

**列表是序列类型的一种扩展，十分常用**

- 列表可以被随意改变

- 使用方括号[]或list()创建

- 元素可以不同类型，长度不限

- ```python
  ls = ["11q","qaq",("12","OVvo")]
  ```

| 方法             | 说明                     |
| -------------- | ---------------------- |
| ls[i:j:k] = lt | 用列表lt替换ls切片后所对应元素子列表   |
| del ls[i]      | 删除第 i 元素               |
| del ls[i:j:k]  | 删除列表ls中第i到第j以k为步长的元素   |
| ls += lt       | 更新列表ls,将列表lt元素增加到列表ls中 |
| ls*=n          | 更新列表ls，其元素重复n次         |
| ls.append(x)   | 在列表ls最后增加一个元素x         |
| ls.clear()     | 删除列表中所以的元素             |
| ls.copy()      | 生成一个新列表，赋值ls中所有元素      |
| ls.insert(i，x) | 在列表ls第i位置增加元素x         |
| ls.pop(i)      | 将第i元素取出，并删除该元素         |
| ls.remove(x)   | 将出现的第一个元素x删除           |
| ls.reverse()   | 将列表元素反转                |

```python
我们可直接使用sort给列表排序
avlist = ['aimer','liquid','particular','practice']
avlist.sort()
for s in avlist:
    print(s)
```

## 4 集合

- 使用{}和set()函数创建，建立空集合时必须使用set()
- 主要用于关系比较、去重
- 集合元素不可更改，不能是可变数据类型
- 集合元素之间无序，每个元素唯一，不存在相同元素

| 操作符      | 说明                     |
| -------- | ---------------------- |
| S \| T   | 并                      |
| S  - T   | 差                      |
| S & T    | 交                      |
| S ^ T    | 补                      |
| S<=T或S<T | 返回True/False，判断两者的子集关系 |
| S>=T或S>T | 返回True/False，判断两者的包含关系 |

| 函数           | 说明                               |
| ------------ | -------------------------------- |
| S.discard(x) | 移除元素X,不存在不报错                     |
| S.remove(x)  | 移除元素x，不存在产生Key Error异常           |
| S.clear()    | 移除所有元素                           |
| S.pop()      | 随机返回S的一个元素，更新S，若S为空产生Key Error异常 |
| set(x)       | 将其他类型转换为集合                       |
|              |                                  |

## 5 映射

**映射是一种键（索引）和值（数据）的对应**

- 内部颜色：蓝色     外部颜色：红色

## 6 字典

字典类型是“映射”的体现

- 键值对：键是数据索引的扩展
- 字典是键值对的集合，键值对之间无序
- 采用大括号和dict()创建，键值对用冒号：表示

| 函数或方法              | 说明                         |
| ------------------ | -------------------------- |
| del d[K]           | 删除字典中建k对应的数据值              |
| k  in  d           | 判断建k是否在字典中                 |
| d.values()         | 返回所有的值信息                   |
| d.keys()           | 返回所有的键信息                   |
| d.items            | 返回所有的键值对信息                 |
| d.get(k,<default>) | 键k存在，则返回相应值，不存在返回<default> |
| d.pop(k,<default>) | 键k存在，则取出相应值，不存在返回<default> |
| d.popitem()        | 随机取出一个键值对，以元组的方式返回         |
| d.clear()          | 删除所有的键值对                   |
| len(d)             | 返回字典d中元素的个数                |

dict['Age'] =8  //若没有Age就会自动添加，若存在则更新数据

# 5、几个特别的语法

## 2 列表推导式

列表推导式（又称列表解析式）提供了一种简明扼要的方法来创建列表。

它的结构是在一个中括号里包含一个表达式，然后是一个for语句，然后是 0 个或多个 for 或者 if 语句。那个表达式可以是任意的，意思是你可以在列表中放入任意类型的对象。返回结果将是一个新的列表，在这个以 if 和 for 语句为上下文的表达式运行完成之后产生。

列表推导式的执行顺序：各语句之间是嵌套关系，左边第二个语句是最外层，依次往右进一层，左边第一条语句是最后一层。

```python
[x*y for x in range(1,5) if x > 2 for y in range(1,4) if y < 3]
#执行顺序
for x in range(1,5)
    if x > 2
        for y in range(1,4)
            if y < 3
                x*y
```

## 3 生成器（generator）

一、概念：

生成器仅仅保存了一套生成值的算法 ，并且没有让这个算法现在就开始执行，而是我什么时候调它，它什么时候开始计算一个新的值，并给你返回这个值。生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

生成器对象具有惰性求值的特点。

不管用哪种方法访问生成器对象，都无法再次访问已访问过的元素 。

二、创建生成器

方法一很简单，只要把一个列表生成式的[ ] 改成( ) ，也称生成器推导式，这样就创建了一个generator

```python
# 列表生成式
lis = [ x*x for x in range(10) ]
print(lis)

# 生成器
generator_ex = ( x*x for x in range(10) )
print(generator_ex)

print(next(generator_ex))            # 第一次通过内置函数next( )执行会打印 0
print(generator_ex.__next__())     # 打印1，使用生成器对象的__next__()方法获取下一个值

结果：
[ 0, 1, 4, 9, 16, 25, 36, 49, 64, 81 ]
<generator object <genexpr> at 0x000002A4CBF9EBA0>
0
1
```

# 6、流程控制语句

## 1 elif

```python
#类似switch
score =91;
if(score>90):
    print("优秀")
elif(score>80):
    print("良好")
else:
    print("一般")
```

## 2.and

同时满足

```python
if 有妹妹 and 有房 :
    德国骨科
```

## 3.or

满足一个即可

```py
if 有妹妹 or 有房 :
    也好
```

## 4.if

```python
 if 你追到我：   （如果的条件语句）

        我就跟你嘿嘿嘿  （如果为真，就执行这里）

    else ：           （否则）

        我就不跟你嘿嘿嘿  （如果为假，就执行这里）
```

## 5.range

range可以生成数字供for循环遍历，它可以传递三个参数，分别表示起始、结束和步长。

range(1,5,2)  左闭右开区间[1,5) 步长为2

```python
for i in range(1,5):
    print(i)

for i in range(1,10,3):
    print(i)
```

# 7、函数

def<名称> （参数）：

## 参数

可以无参数，但必须有

返回多个值默认是以元组返回

## 1、random()

```java
print( random.randint(1,10) )        # 产生 1 到 10 的一个整数型随机数  
print( random.random() )             # 产生 0 到 1 之间的随机浮点数
print( random.uniform(1.1,5.4) )     # 产生  1.1 到 5.4 之间的随机浮点数，区间可以不是整数
print( random.choice('tomorrow') )   # 从序列中随机选取一个元素
print( random.randrange(1,100,2) )   # 生成从1到100的间隔为2的随机整数
a=[1,3,5,6,7]                # 将序列a中的元素顺序打乱
random.shuffle(a)
```

## 2、内置函数

### int函数

int() 函数用于将一个字符串或数字转换为整型。

```python
int(x,base=10)
X-- 字符串或数字
bas--进制，默认十进制
>>>int()               # 不传入参数时，得到结果0
0
>>> int(3)
3
>>> int(3.6)
3
>>> int('12',16)        # 如果是带参数base的话，12要以字符串的形式进行输入，12 为 16进制
18
>>> int('0xa',16)  
10  
>>> int('10',8)  
8
```

# 8、pandas

```python
import numpy as np
import pandas as pd
_author_ = '李樑萍'

df = pd.read_csv('euro.csv')
#欧洲杯总进球数
goals = np.sum(df["Goals"])
print("欧洲杯总进球数：{}".format(goals))

#计算参赛的队伍
count_team = np.count_nonzero(df["Team"])
print("计算参赛的队伍：{}".format(count_team))

#将数据集中的Team, Yellow Cards和Red Cards单独存为个名叫discipline的数据框
# discipline = df[['Team','Yellow Cards',  "Red Cards"]]
discipline = df.loc[:, ['Team', 'Yellow Cards',  "Red Cards"]]
# print(discipline)

#对数据框discipline按照先Red Cards再Yellow Cards进排序
# discipline_list = pd.DataFrame(discipline)
discipline_list = df[['Team', 'Yellow Cards',  "Red Cards"]].sort_values(by=['Red Cards','Yellow Cards'])
print(discipline_list)

#计算每个球队拿到的黄牌数的平均值
aver = np.average(df['Yellow Cards'])
print("每个球队拿到的黄牌数的平均值：{}".format(aver))
```

## padas练习

```python
#数据探索
import numpy as np
import pandas as pd
import os
air = pd.read_csv('air_data.csv', encoding='utf-8')
#输出前5行
print(air.head(5))
#行
print("索引：", air.index)
#列
print("列数：", air.columns)
#输出行和列
print(air.shape)
#os.sep系统的分隔符
#描述属性，第一个是百分位，第二个是包含所有数据，默认是只有数值，第三个是排除
explore = air.describe(percentiles=[], include='all', exclude=None).T

# print(air.max())
# print(air.min())
explore['null'] = len(air)-explore['count']

#选择列， 选择多列需要双括号，单列只需要一个括号
explore = explore[['null', 'max', 'min']]
explore.columns = ['空值数', '最大值', '最小值']
print(explore)

#写入文件
csvPath = os.getcwd()+os.sep+'out'
if not os.path.exists(csvPath):
    print("\n指定文件不存在，创建文件。。。。")
    os.mkdir(csvPath)
    print("\n创建文件成功。。。。")

with open('out/air_data.csv', 'wb') as f:
    explore.to_csv(f, encoding='utf-8')
    f.close()
```

## datatime

```python
pd.to_datetime()
可以将str转换为datetime类型
import pandas as pd

dictDate = {'date': ['2019-11-01 19:30', '2019-11-30 19:00']}
df = pd.DataFrame(dictDate)

df['datetime'] = pd.to_datetime(df['date'])
df['today'] = df['datetime'].apply(lambda x: x.strftime('%Y%m%d'))
df['tomorrow'] = (df['datetime'] + datetime.timedelta(days=1)).dt.strftime('%Y%m%d')
1.列date，用pd.to_datetime，将原本date列从object格式转为datetime64[ns]格式。
2.列today，用.strftime('%Y%m%d')取出年月日，把这个函数用apply lambda应用到df的这一列中。
3.列tomorrow，用+ datetime.timedelta(days=1)加上一天。然后用.dt.strftime('%Y%m%d')取出年月日。
```

# 9、numpy

## 1.数组切片

```python
# coding=utf-8
import numpy as np

file_path = "./demo.csv"

# loadtxt()加载文本文件或CSV格式文件中的数据
# t1 = np.loadtxt(file_path, delimiter=",", dtype="int", unpack=True)  # unpack表示矩阵的转置,默认False
t2 = np.loadtxt(file_path, delimiter=",", dtype="int")

# print(t1)
print(t2)  # 二维

######################################
# 数组的切片

# 取行
print(t2[2])  # 下标从0开始
# t2[2] = 0  # 可以直接通过=赋值 修改对应位置的数据。

# 取连续的多行
print(t2[2:])
print(t2[2::2])  # 第二个冒号:表示步长

# 取不连续的多行
print(t2[[2,8,10]])

# 通用方式取行(取行取列都可以通过该方式取，逗号前表示行  逗号后表示列)
print(t2[1,:])
print(t2[2:,:])
print(t2[[2,10,3],:])  # 取不连续的多行


# 取列
print(t2[:,0])  # 一维数组

# 取连续的多列
print(t2[:,2:])

# 取不连续的多列
print(t2[:,[0,2]])

# 取行和列，取第3行，第4列的值
a = t2[2,3]
print(a)
print(type(a))  # <class 'numpy.int64'>

# 取多行和多列，取第3行到第5行，第2列到第4列的结果
# 取的是行和列交叉点的位置
b = t2[2:5,1:4]   # 2:5 包含2,不包含5
print(b)

# 取多个不相邻的点
c = t2[[0,2,4],[0,1,3]]  # 选出来的位置是（0，0） （2，1） （4，3）
print(c)
```

```python
#将两列变为二维数组
sxh_x = np.matrix([5.1, 4.9, 4.7, 4.6, 5.0, 5.4, 4.6, 5.0, 4.4, 4.4, 7.0, 6.4, 6.9, 5.5, 6.5, 5.7, 6.3, 4.9, 6.6, 5.2])
sxh_y = np.matrix([3.5, 3.0, 3.2, 3.1, 3.6, 3.9, 3.4, 3.4, 2.9, 3.1, 3.2, 3.2, 3.1, 2.3, 2.8, 2.8, 3.3, 2.4, 2.9, 2.7])
sxh_character = np.matrix([1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])
sxh_x = sxh_x.T
sxh_y = sxh_y.T
sxh_character = sxh_character.T
sxh = np.hstack((sxh_x, sxh_y, sxh_character))
sxh = np.asarray(sxh)
```

## reshape()

用于改变数组的形状，仅改变原始数据的形状，不改变原始数据的值

```python
import numpy as np
arr = np.arange(12)
arr1 = arr1.reshape(3,4)#设置ndarray的维度3行4列
arr_r = arr1.ravel()#将多维数组转换为一维数组
```

![image-20211127232134087](C:\Users\lilia\AppData\Roaming\Typora\typora-user-images\image-20211127232134087.png)

# 10、模块

你可以把模块理解为一个 .py文件，这个文件里面包含了所需要的函数和变量，那么下次我们任何一个程序要使用这里面的东西，我们只需要把这个模块导入到我们的程序里面来，就可以直接用了，简直不要太爽。造轮子多麻烦啊，拿来就用是了。

其实 Python 有内置了一些模块，我们可以直接引用，还有一些第三方模块，也就是我们可以自己创建模块，安装好模块就可以直接使用了

## 1.使用模块

如果我们要使用一个模块，可以将这个模块导入，使用 import ，比如我们要导入 Python 的内置的 sys 模块（sys模块包含了与Python解释器和它的环境有关的函数），那么我们就可以使用 import sys:

```python
import sys
```

## 2.创建自己的模块

创建自己的模块其实就是自己写了个程序，然后给别人import，我们来写一个模块

```python
version= 'v0.1'
def fun():
    print("oh~")
```

记住，这模块要保存到和你即将要用的 Python 程序的同一目录下，然后这文件必须是 .py 结尾不用我说了吧。

接着我们就来使用我们自己的模块吧：

```python
import funmodule
funmodule.fun()
print("version:",funmodule.version)
```

# 11、面向对象

python也是面向对象的语言，可以定义class

```python
class DuoLAM:
    name = "aimer"

     def getZhuQingTing(self):
        print("给一个竹蜻蜓")

#使用
duola = DuoLAM()
duola.getZhuQingTing
print("名字：", duola.name)
```

我们可以看到在定义 getZhuQingTing 这个方法的时候，定义了一个 self 这个参数，其实这个参数指的是DuoLaAMeng对象本身，这就和我们普通定义的函数有些许区别

## 1.__ init __函数

我们在调用对象的时候，有些东西是可以初始化的，这个时候 Python 就给我们提供了一个初始化函数，也就是当我们去调用这个对象的时候，它会先去执行 **init** 这个函数。

```python
class DuoLaAMeng:

    def __init__(self, name):
        self.name = name

    def getZhuQingTing(self):
        print("给" + self.name + "一个竹蜻蜓")


duola = DuoLaAMeng("大雄")
duola.getZhuQingTing()
```

我们定义了一个 DuoLaAMeng 类， 并且给了一个初始化函数，当别人调用这个类的时候呢，传一个 name 进来，我们就可以对这个名字进行初始化了。

## 2.继承

python既然是面向对象的语言，那么它也有三个特征（多态、继承、封装）

python中的继承表现形式是这样的

```python
class 哆啦A梦（机器人）：
#哆啦A梦继承了父类机器人，它拥有机器人的功能
```

```python
class DuoLaAMeng:

    def __init__(self, name):
        self.name = name

    def getZhuQingTing(self):
        print("给" + self.name + "一个竹蜻蜓")

class 哆啦A梦(DuoLaAMeng):
    pass

duola = 哆啦A梦("大雄")
duola.getZhuQingTing()
```

## 3.多态

DuoLaBMeng 和 DuoLaCMeng 是 DuoLaAMeng 的儿子，我们也可以把它的儿子当做 DuoLaAMeng 对象来使用，比如说有一天 DuoLaAMeng在忙，这时候大雄完全可以把它的儿子们当做是 DuoLaAMeng 来使用，完全木有问题，这就是面向对象中多态的意思。

```python
print(isinstance(duola, DuoLaAMeng))
#true
```

# 12、异常

知道代码有错还狂往下写？是的没错，就是明明知道可能代码会有错误，但是我们还是往下写。就是这么任性！

## 1.异常捕获

有时候我们对我们的代码的报错是可预知的，比如我们想让 Python 帮我们打开一个小黄文的文件，比如 yellow.txt，可是我们的电脑不一定有，如果这个时候没有的话我们的代码会报错的对吧？

```
document = open('yellow.txt')
print('filename:' + document.name)
```

运行之后可以看到这里报错:

```
FileNotFoundError: [Errno 2] No such file or directory: 'yellow.txt'
```

告诉我们没有这个文件。

但是如果这时候我们还想往下运行怎么办呢？

那就可以把这异常给捕获掉，使用 `try...except...finally...`

> try：用来包裹我们可能存在错误的代码； except：当发现错了就会执行这里 finally：无论怎么样最后都会执行到的。

## 2.抛出异常

有时候我们没有去处理异常， Python 也会给我们报出错误，这是因为 Python 有个 BaseException 的异常基类，当Python发现我们的代码错误的时候，又没人去处理，它就会层层的往上抛出错误，直到最上级。

我们可以自己定义一个异常类：

```python
class MyError(Exception):
    pass

def foo(value):
    if(value==0):
        raise MyError('ERROR %s' % value)

foo(0)
```

可以看到我们自定义了一个叫做MyError的异常类，继承与Exception，当我们传入 0 的时候就会抛出异常。在这里我们使用到的**关键字是raise，就是用来抛出异常的意思**。

# 13、IO

## 1.读取文件

python有个内置的函数叫做 open() , 我们可以通过它直接打开文件，打开完文件就可以读取了，但是有可能会报错，就是文件不存在，这个时候我们可以用到上次说的 try...finally 来处理异常：

```python
try:
    f = open("G:/xiaohuangwen.txt","r",encoding="UTF-8")
    print(f.read())
finally:
    if f:
        f.close()
```

我们通过 open 打开了 xiaohuangwen.txt 这个文件。 r 就是读的意思， encoding就是定义好文件编码。

最后一定要记得将文件 close 掉，这样才不会造成系统浪费资源。

有时候你在读取文件的时候，是不是觉得每次都要 try...finally 很麻烦？ 贴心的 Python 帮我们简化了流程，我们只要直接这样写就可以了：

```python
with open("G:/spider/xiaohuangwen.txt","r",encoding="UTF-8") as f:
    print(f.read())
```

## 2.写入文件

写入文件内容也是一个道理，我们首先要打开文件，然后往里写内容，如果我们传入的参数是 ‘w’ 的话，它会覆盖原来的文件，而我们传入 ‘a’ 则可以在文件末尾追加内容：

```python
with open("G:/spider/IO.txt","a",encoding="UTF-8") as f:
    print(f.write("\n明月清风与我！"))
```

## 3.文件存储器

Python 有一个叫做 pickle 的模块，有了它，我们就可以在一个文件中持久的存储我们的对象。

还有一个叫做 cPickle 的模块，它是用 C 写的，所以它更加牛逼一点，比 pickle 速度快，要快上 1000 倍，所以我么用 cPickle 这个模块会好点。

不过在 Python3 已经将 cPickle 改名为 pickle 了，所以我们就可以直接 import pickle 就可以啦。

```python
import pickle as p


# 我们要存储内容的文件名
girlfriendlistfile = 'girlfriend.data'

girlfriends = ['麻衣', '奏', '葵']

# 把我们的女朋友写到文件里，然后存储器存储
with open(girlfriendlistfile,'wb+') as f:
    p.dump(girlfriends, f)
    f.close()

del girlfriends # 删掉我们的女朋友

# 把我们的女朋友读回来！！
with open(girlfriendlistfile,'rb+') as f:
    list = p.load(f)
    print (list)
```

# 14、写一个操作界面

Python 有一个自带的库叫做 tkinter ，用它我们可以写出系统的操作界面，不管你是 Mac OS 系统，还是 Windows 系统，它都可以生成相对应的操作界面。这就是所谓的跨平台。

![img](http://www.ubuntu520.com/images/py18-01.webp)

原理就是我们使用 Python 的代码去调用 Tkinter， Tkinter 已经封装了访问TK的接口，这个接口是一个图形库，支持多个操作系统，通过它我们就可以调用我们系统本身的GUI接口了。

```python
from tkinter import *
import tkinter.messagebox as messagebox

class MyApp(Frame):

    def __init__(self,master=None):
        Frame.__init__(self,master)
        self.pack()
        self.createWidgets()

    def createWidgets(self):
        self.helloLabel = Label(self,text="世界上最帅的人是谁？")
        self.helloLabel.pack()
        self.quitButton = Button(self,text="谁呢？",command=self.who)
        self.quitButton.pack()

    def who(self):
        messagebox.showinfo("答案","当然是你啊")


myapp = MyApp()
myapp.master.title('hello')
myapp.mainloop()
```

在这里：

1. 我们导入了 tkinter 的相关模块

2. 定义了初始化函数，通过 pack（） 方法将我们的组件传给父容器

3. 自定义一个创建组件的方法，我们创建了一个标签和一个按钮，这个按钮被点击后就会触发 who 这个方法

4. 我们通过 messagebox 来显示一个提示框

5. 实例化我们的 APP，然后通过主线程来监听我们的界面操作

# 15、Tcp连接

当我们通过socket连接上某个网站，并请求了图片资源之后，socket接收到的只是字节流，响应的实体部分就是我们的图片了，我们要做的是把http响应实体从http报文中分离出来保存到文件中。(注意: 不要把接收到的所有信息全部保存到文件中，那样做系统识别不了其中包含的图片)

```python
import socket

HOST = 'localhost'
PORT = 80
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((HOST, PORT)) # 通过socket与服务器连接

# http请求头，下载服务器上uri为'/test.jpg'的图片
http_request = """
GET /test.jpg HTTP/1.1
Host: localhost

"""
# 发送http请求，把响应先保存在test.jpg中，这个test.jpg并不是正确的图片格式，
# 里面还包含有http响应头，如果不做转换是没法打开的
s.sendall(http_request)
f = open('test.jpg', 'wb')
try:
    while True:
        http_response = s.recv(9192)
        if not http_response:
            f.close()
            break
        f.write(http_response)
except socket.error:
    print socket.error.message
finally:
    f.close()

# 把实际下载的图片保存在new.jpg中
jpg = open('test.jpg', 'rb')
new = open('new.jpg', 'wb')

flag = 0
for line in jpg.readlines():
    if flag == 0:
        print line,
    # 空行是区分http响应头和响应实体的标志，因为http头是不能有空行的
    if line == '\r\n' or line == '\n' or line == '\r':
        flag = 1
        continue
    if flag == 1:
        new.write(line)

# 下载完成后关闭文件资源
jpg.close()
new.close()
```

上面的例子做了很繁琐的处理，实际上这类工作在Python已经有非常成熟的第三方库了，如urllib2:

```python
import urllib2

http_response = urllib2.urlopen('http://localhost/test.jpg')
f = open('download.jpg', 'wb')
f.write(http_response.read())
f.close()
```

# 16、输入输出

## 1.输出(格式化输出重点)

1. 1. 普通输出

```python
# 普通输出
print('我希望五十年以后，我还能伴你左右')
```

1. 1. 格式化输出

2. 1. 1. 使用prinf()函数
- - - - %在字符串中表示格式化操作符，它后面必须附加一个格式化符号（与c语言格式化输出一样，只不过中间用%进行分隔，而不是逗号）
      - ![img](https://cdn.nlark.com/yuque/0/2021/png/22824747/1636635419959-da74cbc6-067c-45e7-b7a7-2501057a1ab2.png)

​                    %s代表字符串，%d代表数值（整型、浮点型、复数,当输出值为浮点型时会强制转换为整型输出）

```python
#格式化输出
age=19
name='smile'
# %s代表字符串，%d代表数值（整型、浮点型、复数,当输出值为浮点型时会强制转换为整型输出）
print('我的年龄是%d,我的名字是%s' %(age,name))
# 10
print('我的年龄是%d'%10.7)
```

- - - - 格式化输出浮点数（默认是右对齐，用法与c语言一样）

```python
# 总宽度为10，小数位精度为3
print('pi1=%10.3f' %PI)
# 用0填充空白
print('pi3=%010.3f' %PI)
# 左对齐，总宽度10个字符，小数位精度为3
print('pi4=%-10.3f' %PI)
# 在浮点数前面显示正号
print('pi5=%+f' %PI)
```

![img](https://cdn.nlark.com/yuque/0/2021/png/22824747/1636636270562-d15498bf-e227-4b44-88e7-2ee3a52122fd.png)

1. 1. 1. 使用str.format()方法，它通过{}操作符和:辅助指令类替代%操作符
- - - - 通过位置索引值

```python
# python 3.7
print('{0} {1}'.format('python',3.7))
# python 3.7
print('{} {}'.format('python',3.7))
# 3.7 python 3.7
print('{1} {0} {1}'.format('python',3.7))
```

​                         注意：在字符串中可以使用 {} 作为格式化操作符。与 % 操作符不同的是，{} 操作符可以通过包含的位置值自定义引用值的位置，也可以重复引用。

- - - - 通过关键字进行索引

```python
# Amo年龄是18岁。
print('{name}年龄是{age}岁。'.format(age=18,name="Amo"))
```

- - - - 通过下标进行索引

```python
L=['Jason',30]
# Jason年龄是30。
print('{0[0]}年龄是{0[1]}。'.format(L))
```

注意：通过使用 format() 函数这种便捷的 映射 方式，列表和元组可以 打散 成普通参数传递给 format() 方法，字典可以打散成关键字参数给方法。format() 方法包含丰富的格式限定符，附带在 {} 操作符中 : 符号的后面。

- - - - 填充与对齐

: 符号后面可以附带填充的字符，默认为空格，^、<、>分别表示居中、左对齐、右对齐，后面附带宽度限定值。

- - - - - 【示例1】下面示例设计输出8位字符，并分别设置不同的填充字符和值对齐方式

```python
# 总宽度为8，右对齐，默认空格填充
print('{:>8}'.format('1'))
# 总宽度为8，右对齐，使用0填充
print('{:0>8}'.format('1'))
# 总宽度为8,左对齐，使用a填充
print('{:a<8}'.format('1'))
```

![img](https://cdn.nlark.com/yuque/0/2021/png/22824747/1636684333146-8ffc734f-9995-4ba5-83c5-8db304588f16.png)

- - - - 精度与类型f

- - - - - 【示例2】f与float类型数据配合使用

```python
print('{:.2f}'.format(3.1415926))
```

![img](https://cdn.nlark.com/yuque/0/2021/png/22824747/1636684570169-8b331dfb-9b9f-4bb1-a371-4ef2ecb427ed.png)

- - - - 进制数字输出

- - - - - 【示例3】使用b、d、o、x分别输出二进制、十进制、八进制、十六进制数字。

```python
num=100
print('{:b}'.format(num))
print('{:d}'.format(num))
print('{:o}'.format(num))
print('{:x}'.format(num))
```

![img](https://cdn.nlark.com/yuque/0/2021/png/22824747/1636684748956-37c9da81-e6e0-4dae-9501-ddcaf4c05f85.png)

## 2.输入

input返回的是字符串类型

```python
name=input('请输入你的名字:')
print('我的名字是{}'.format(name))
```

# 17、爬虫

## urllib

```python
response = urllib.request.urlopen('http://www.baidu.com')
print(response.read().decode('utf-8'))
```
