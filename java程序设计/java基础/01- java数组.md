# java数组

***

### 三种定义方式

1.数组类型[]  数组名字 = new 数组类型[数组长度]

```java
String[] str = new String[n];
//这里n代表数组的长度可变

//另外上面这种写法和下面这种写法作用一样没有区别
String str[] = new String[n];
```

2.数组类型[]  数组名 = {数组0， 数组1， 数组2.....};

```java
int[] nums = {1, 2, 3, 4,.....};
```

3.数组类型[] 数组名 = new 数组类型[]{数组0， 数组1， 数组2，数组3，数组4，......};

```java
int[] nums = new int[]{1, 2, 3, 4,.....};
```

#### 数组初始化的时候有默认值

```java
int 0,  short 0, byte 0, long 0, float 0.0, double 0.0, char /u0000, boolean flase, String null,
```



#### 对象数组初始化

由于对象数组声明时，元素的默认初始值是null，因此数组初始化时需要创建对象。

```java
Student[] student;//声明
student = new Student[3];//整体创建一次
student[0] = new student("Li");//在初始化时还需要用到new再次创建
```

