```
@title : R语言入门
Created on 2017-08-20
@author : Jixue
```
## R的介绍和安装

> 参考书籍：
《R语言实战》
《R语言：核心技术手册》
《ggplot2 : 数据分析与图形艺术》

## Why R
* 免费的
* 专业统计分析软件
* 优秀的作图能力
* 导入数据源的多样性
* 跨平台的
* 大量的包
* 交互式的

##  R的获取与安装
> CRAN：https://www.r-project.org/
> 
RStudio：https://www.rstudio.com/   下载Desktop版本

## 包管理
* 安装包 ： install.packages(“ggplot2”)
* 查看当前已经安装的包：installed.packages()
* 包的加载：library()

## 基础语法
详见jupyter notebook

## 数据结构
### vector

向量：存储数值型、字符型或逻辑型数据的一种数组

向量生成:
```
a <- c(3,2,7,1,2)
b <- c(‘one’,’two’,’three’)
```
单个向量中的数据必须拥有相同的类型(若不一致，转换成字符串)
访问向量
```
a[2]
a[c(3,4)]
```
### matrix
矩阵是二维数组，每个元素拥有相同的类型数据

矩阵生成：
```
matrix(vector,nrow,ncol)
m1<- matrix(11:30, 4, 5)
```
可以使用下标和方括号来访问元素
```
m1[1,2]

```
### array
数组与矩阵类似，但是维度可以大于2

数组创建:
```
arr1 <- array(vector,dimensions,dimnames)
arr2 <- array(1:30,c(3,5,2))

```
元素访问:
与矩阵类似,
```
arr2[1,2,3]
```
### data frame (columns can be different modes)
数据框由不同的列可以包含不同类型的数据组成

数据框生成
```
age <-c(22,23,26,27,30)
gender <-c('m','f','f','m','m')
id <-c(1,2,3,4,5)
df1 <-data.frame(id,age,gender)

```
元素选取
* 下标和方括号
* 通过列名字
* 使用美元符号

```
df1[2] #选取列
df1[1,]  #选取行
df1[1,c('id','age')]
df1$id  #选取列
```
### list
列表是一些对象的有序集合，比较复杂

列表创建
```
mylist <- list(obj1,obj2,……)

```
元素选取
通过在双重方括号中指明代表某个成分的数字或名称来访问列表中的元素
```
mylist[[2]]

```
## 处理数据对象的函数
详见jupyter notebook

## 数据的读取与存储
* getwd()
* setwd()
* read.table(file,header = TRUE,sep = "")
* read.csv2()
* write.table()
* write.csv2()

##缺失值
* 数据缺少，未作答，记录错误等产生
* 缺失值以符号NA表示，不可能出现的值以NaN表示。
* 字符型与数值型数据使用相同的缺失值符号
```
sum(is.na(df$age))
na.omit(df1)  #将带有missing value的记录都删除
df[is.na(df)] = value  #替换NA
```

## 数据排序
使用order()函数排序，默认的是升序，添加减号是降序

## 增加列
```
df$newCol <- value
df[newCol] <- value
variable[cond] <- expression
within(df,{ variable[con] <- expression})
```
## 重新命名

```
library(reshape)
df1 <-rename(df,c(label = "LABEL"))  #不在原数据上修改，生成新的数据集
names(df1)[1:3] <-c("AGE","WORKCLASS","FNLWGT")  #在原数据上修改
```
## 数据验证与转换
```
is.data.frame(df1)
is.vector(df1)
n1 <-as.numeric(s1)
```
## 函数应用
* apply(x,MARGIN,FUN,…..)
* MARGIN=1代表行，MARGIN=2代表列
* FUN可以是自己写的函数，也可以R自带
* lapply允许在向量或者列表的每一个元素运行函数，返回一个列表
* sapply返回的不是列表
```
apply(df[c("age","education.num")],MARGIN=2,sum)
```
## 子集
```
subset(df,age >= 30 & label == 0,select = c(age,workclass,label))
```

## 汇总函数
```
tapply(df$age,df$sex,mean)  #年龄根据性别分类
tapply(df$age,list(df$sex,df$class),mean) #根据性别和class分类
by(df,list(df$sex,df$class),summary) #根据性别和class分类
aggregate(age ~ sex + class,data = df,mean)   #也可以写为下面的形式
aggregate(df$age,list(df$sex,df$class),mean)
```
综合统计量 summary(df)

## 数据透视表
```
table(df$sex,df$class)
xtabs(df$age ~ df$sex)
```

## 高级数据管理
dplyr
### filter() 对行进行过滤
```
filter(data,cond1,cond2,….) and 
filter(data,con1 | cond2,|…..) or 
slice(data,1:10)  #通过为止选取
```

### arrange() 对数据框进行排序
```
arrange(data,v1,v2,v3)
arrange(data,desc(v1))
```

### select 选择列
```
select(data,v1,v2,…)         #选择列
select (data,v1:v3)         #选择v1 到v3之间的列（包含）
select(data,-(v1:v3)         #剔除v1到v3的列（包含）
```

### distinct 行的唯一值
```
distinct(data,v1,v2)
```

### mutate 增加新列
```
mutate(data,vnew = v1 –v2, vnew2 = vnew  + v3)
```
原理与Transform接近，唯一的区别是Mutate你可以使用你刚刚创建的column

### summarise() 计算统计数
```
summarise(data,mean())
```

### 随机抽取
```
sample_n(data,n)
sample_frac(data,frac)   #百分比
```

### group_by函数
```
grouped <- group_by(data,v1)  #一个分组
newdf <- summarise(grouped,count =n(),dist = mean()….)
summarise             #返回一个数值，我们可以调用R基础包里面的常用函数
grouped <- group_by(data,v1,v2,…) 多个分组
```

### join
```
inner_join(df1,df2,by = "id")
inner_join(df2,df3,by=c("id"="id1"))  #key名称不同
```

## data.table()
[data.table](https://github.com/Rdatatable/data.table/wiki
)
```
DT <- data.table()
DT [ i , j , by ]
#使用数字选择行 
DT [3 : 5]

#基于条件选择行 
DT [ v2 == ‘A’]

#使用%in%
DT [V2 %in% c(“A”,”B”)]

#选择列 
DT [, V1]
DT [,.(V1，V3) ]

#在j上调用函数
DT [, sum(v1)]
DT [,.(sum(v1),sd(v3)]

#计算的列明赋予名称
DT [, .(agg = sum(v1), Sd = SD(V3))]

#Doing J by group 
DT[,.(v4.sum = sum(v4)),by=v1]

#Doing J by several groups using .()
DT[,.(v4.sum = sum(v4)),by=.(v1,v2)]

#Call functions in by 
DT[,.(v4.sum = sum(v4)),by=.(log.v1log(v1))]
```

