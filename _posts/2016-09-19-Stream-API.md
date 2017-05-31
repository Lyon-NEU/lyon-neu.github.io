---
layout: post
title: "Java 8 Stream API"
description: ""
category: Java
tags: [Java]
---
{% include JB/setup %}

## Java 7 的排序，取值实现

```java
List<Transaction>groceryTransctions=new ArrayList<>();
for(Transction t:transctions){
	if(t.type()== Transctions.GROCERY){
		groceryTranctions.add(t);
	}
}
Collectinos.sort(groceryTranctions,new Comparator(){
	public int compare(Transaction t1,Transction t2){
	return t2.getValue().compareTo(t1.getValue());
}
});
List<Integer>transctionIds=new ArrayList<>();
for(Transction t:groceryTransctions){
	transctionsIds.add(t.getId());
}
```
- 在Java 8中使用Stream，代码更加简洁易读，而且使用并发模式，程序执行速度更快

```java
List<Integer>transctionIds=transction.parallelStream().filter(t->t.getType()==Transaction.GROCERY).sorted(comparing(Transaction::getValue).reversed()).map(Transaction::getId).collect(toList());

Collection.Sort(a,(a,b)->b.compare(a))
```
---------------------------------------------
## Stream介绍

- 什么是流
Stream不是集合元素，不是数据结构，它并不保存数据，它是有关算法和计算的，更像一个高级的Iterator.原始的Iterator，用户只能显式地一个一个遍历元素并对其执行某些操作，高级的Stream，用户只需要给出需要对其包含元素执行什么操作，比如“过滤掉长度大于10的字符串”/“获取每个字符串的首字线”等，Stream会隐式地在内部遍历，做出相应的数据转换。  

- 而和迭代器又不同的是，Stream可以并行化操作，迭代器只能命令式地，串行化地操作，Sttream的并行化操作依赖于Java7中引入的For/Join框加来拆分任务和加速处理过程.Java的并行API演变历程基本如下:

	+ 1.0-1.4中的Java.lang.Thread
	+ 5.0中的java.util.concurrent
	+ 6.0中的Phasers等
	+ 7.0中的Fork/Join框架
	+ 8.0中的Lambda

****************************************************

## 流的构成

当我们使用一个流的时候，通常包括三个基本步骤：   
获取一个数据源(sorce)->数据转换->执行操作获取想要的结果，每次转换原有Stream不变，返回一个新的Stream对象，这就允许对其操作可以像链条一样的排列，变成一个管道，如下图所示:

![流管道]({{site.url}}/assets/pictures/StreamPipeline.png)

有多种方式生成Stream Source:

- 从Collection和数组
	- Collection.stream()
	- Collection.parallelStream()
	- Arrays.stream(T array) or Stream.of()
	- java.io.BufferedReader.lines()  
- 静态工厂
	- java.util.stream.IntStream.range()
	- java.noi.file.Files.walk()  
- 自己构建
    - java.util.Spliterator
    - Random.ints()
    - BitSet.stream()
    - Pattern.splitAsStream(java.lang.CharSequence)
    - JarFile.stream(0)

流的操作类型分为两种:

- Intermediate: 一个流可以后面跟随零个或多个intermediate操作，其主要目的是打开流，做出某种程度的映射/过滤，然后返回一个新的流，交给下一个操作使用。这类操作都是惰性化的，就是说，仅仅调用到这类方法，并没有真正开始流的遍历

- Terminal: 一个流只能有一个terminal,当这个操作执行后，流就被使用"光"了，无法再被使用，所以这必定是流的最后一个操作，Terminal操作的执行，才会真正开始流的遍历，并且会生成一个结果，或者一个side effect

## 流的使用

**流的构造**

```java
// 1. Individual values
Stream stream = Stream.of("a", "b", "c");
// 2. Arrays
String [] strArray = new String[] {"a", "b", "c"};
stream = Stream.of(strArray);
stream = Arrays.stream(strArray);
// 3. Collections
List<String> list = Arrays.asList(strArray);
stream = list.stream();

```

**流的操作**

当把一个数据结构包装成Stream后，就要开始对里面的元素进行各类操作:

	- Intermediate: map(mapToInt,flatMap等)，filter,distinct,sorted,peek,limit
	- Terminal: forEach,forEachOrdered,toArray,reduce,collect,min,max,count...
	- Short-circuiting: anyMatch,allMatch,noneMatch,findFirst,findAny,limit

**map/flatMap**

转换大写

```java
List<String>output=wordList.stream().
map(String::toUpperCase).
collect(Collectors.toList());

```
**平方数**
```java
List<Integer>nums=Arrays.asList(1,2,3,4);
List<Integer>squareNums=nums.stream().
map(n->n*n).
collect(Collectors.toList());
```
上面例子中，map生成的是1:1映射，每个输入元素，都按照规则转换成为另一个元素

**一对多**
```java
Stream<List<Integer>>inputStream=Stream.of(
Arrays.asList(1),
Arrays.asList(2,3),
Arrays.asList(4,5,6));
Stream<Integer>outputStream=inputStream.flatMap((childList)->childList.stream());
```

flatMpa把inputStream中的层级结构扁平化，就是将底层元素抽出来放到一起，最终output的新Stream里面已经没有List了，都是直接的数字。

**filter**

filter对原始Stream进行某项测试，通过测试的元素被留下来生成一个新Steam

**留下偶数**
```java
Integer[]sixNums={1,2,3,4,5,6};
Integer[]evens=Stream.of(sixNums).filter(n->n%2==0).toArray(Integer[]::new);
```
**把单词挑出来**
```java
List<String>output=reader.lines().
flatMap(line->Stream.of(line.split(REGEXP))).
filter(word->word.length()>0).
collect(Collectors.toList());
```
**reduce**
这个方法的主要作用是把Stream元素组合起来，它提供一个起始值(种子)，然后依照规则和前面的第一个，第二个，第三个，，，第n个元素组合。从这个意义上说，字符串拼接、数值的sum、min、max、average都是特殊的reduce，例如Stream的sum就相当于: Integer sum=integers.reduce(0,(a,b)->a+b);

```java
//字符串拼接
String concat=Stream.of("A","B","C","D").readuce("",String::concat)
//求最小值
double minValue = Stream.of(-1.5, 1.0, -3.0, -2.0).reduce(Double.MAX_VALUE, Double::min); 
// 求和，sumValue = 10, 有起始值
int sumValue = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
// 求和，sumValue = 10, 无起始值
sumValue = Stream.of(1, 2, 3, 4).reduce(Integer::sum).get();
// 过滤，字符串连接，concat = "ace"
concat = Stream.of("a", "B", "c", "D", "e", "F").
 filter(x -> x.compareTo("Z") > 0).
 reduce("", String::concat);
```
**limit/skip**

limit返回Stream的前面n个元素，skip则是扔掉前n个元素
```java
public void testLimitAndSkop(){
	List<Person>persons=new ArrayList();
	for(int i=0;i<10000;i++){
		Person person=new Person(i,"name"+i);
	}
	List<String>personList2=person.stream().
	map(Person::getName).limit(10).skip(3).collect(Collectors.toList());
	System.out.println(personList2);
}
private class Person{
	public int no;
	private String name;
	public Person(int no,String name){
		this.no=no;
		this.name=name;
	}
	public String getName(){
		System.out.println(name);
		return name;
	}
}

//output
name1
name2
name3
name4
name5
name6
name7
name8
name9
name10
[name4, name5, name6, name7, name8, name9, name10]
```

**min/max/distinct**

找出最长一行的长度
```java
BufferedReader br=new BufferedReader(new FileReader("c:\\test.txt"));
int longest=br.lines().
mapToInt(String::length).
max().
getAsInt();
br.close();
System.out.println(longest);
```

## Conclusion

- 它不是数据结构
- 它没有内部存储，只是用操作管道从source抓取数据
- 不修改自己所封装的底层数据结构的数据，例如Stream的filter操作会产生一个不包启被过滤元素的新Stream,而不是从source删除那些元素
- 所有Stream的操作必须以lambda表达式为参数
- 不支持索引访问
- 你可以请求第一个元素，但无法请求第二个、第三个，或者最后一个
- 很容易生成数组或者List
- 惰性化
- 很多Stream操作是向后延迟的，一直到它弄清楚了最后需要多少数据才会开始
- Intermediate操作是永远惰性化的
- 并行能力
- 当一个Stream是并行化的，就不需要再多写线程代码，所有对它的操作会自动并行进行
- 可以是无限的
- 集合有固定大小，Stream则不必

**toArray**
<A> A[] toArray(IntFunction<A[]>geneartor)

返回包含流元素的数组，使用提供的*generator*函数来分配返回的数组
这是一个终结操作

```java
	Person[] men=people.stream().filter(p->p.getGender()==MALE).
	toArray(Person[]::new);
```

- 
The Optional<T> class (java.util .Optional) is a container class to represent the existence or absence of a value. In Listing 8, it is possible that findAny doesn’t find any transaction of type grocery. The Optional class contains several methods to test the existence of an element. For example, if a transaction is present, we can choose to apply an operation on the optional object by using the ifPresent method, as shown in Listing 9 (where we just print the transaction).

```
 transactions.stream()
              .filter(t -> t.getType() == Transaction.GROCERY)
              .findAny()
              .ifPresent(System.out::println);
```

-
You can also convert a file in a stream of lines using the Files.lines static method. For example, in Listing 17 we count the number of lines in a file.

```
long numberOfLines = 
    Files.lines(Paths.get(“yourFile.txt”), Charset.defaultCharset())
         .count()
```











[具体请参考oracle 官方文档](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html)