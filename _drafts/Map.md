---
layout: post
title: "Map"
description: ""
category: Java
tags: [Map]
---
{% include JB/setup %}

## Map的几种遍历方式
*方法一* 在 _foreach_ 循环中使用 *entries* 来遍历

```java
    Map<String,Integer>map=new HashMap<String,Integer>();

    for(Map.Entry<String,Integer>entry:map.entrySet()){
        System.out.println("Key= "+entry.getKey()+", Value="+entry.getValue());
    }
```

*注意:*，此方法是在java 5中被引入的，如果遍历的是一个空的对象，将抛出NullPointException异常

*方法二* 在 _foreach_ 循环中遍历keys或values

```java
    Map<String,Integer>map=new HashMap<String,Integer>();

    //遍历map中的键
    for(String key:map.keySet()){
        System.out.println("Key= "+key);
    }

    //遍历map中的值
    for(Integer value:map.values()){
        System.out.println("Value= "+value);
    }
```

*方法三* 使用 _Iterator_ 遍历

```java
    Map<String,Integer>map=new HashMap<String,Integer>();
    Iterator<Map.Entry<String,Integer>>iter=map.entrySet().iterator();

    while(iter.hasNext()){
        Map.Entry<String,Integer>entry=iter.next();
        System.out.println("Key= "+entry.getKey()+",Value= "+entry.getValue());
    }
```

*方法四* 通过键值遍历

```java
    Map<String,Integer> map=new HashMap<String,Integer>();

    for(String key:map.keySet()){
        Integer value=map.get(key);
        System.out.println("Key= "+key+", Value= "+value);
    }
```
此种方法效率比`foreach`低很多