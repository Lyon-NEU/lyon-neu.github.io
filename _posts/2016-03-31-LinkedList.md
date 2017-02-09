---
layout: post
title: "LinkedList"
description: ""
category: Java
tags: [Java]
---
{% include JB/setup %}


## 常用方法

* `LinkedList()` construct an empty list
* `LinkedList(Collection<? extends E>c)` Constructs a list containg the elements of the specified collection, in the order they are returned by the collection's iterator

* `getFirst()`,`getLast()`获取列表第一个、最后一个元素
* `addFirst()`与`add()`和`addLast()`相同，都将某个元素插入到列表的尾(端)部。

```java 

public class LinkedListTest {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		LinkedList<String> list=new LinkedList<String>();
		list.add("1");  
		list.add("2");  
		list.add("3");  
		list.add("4");  
		list.add("5");  
		list.add("6");
	  	list.addFirst("0");
	  	list.addLast("7");
	    System.out.println("链表的第一个元素是 : " + list.getFirst());  
	    System.out.println("链表最后一个元素是 : " + list.getLast()); 
	}
}

//output: 
链表的第一个元素是 : 0
链表最后一个元素是 : 7
```

* LinkedList也像ArrayList一样实现了List的接口，但是在执行某些操作(中间的插入、删除)时比ArrayList更高效，但在随机访问操作方面要逊色一些。

* LikedList添加了可以使其用作栈、队列或双端队列的方法

* removeFirst()与remove()功能一样，它们移除并返回列表的头，在列表为空时抛出NoSuchElementException,poll()在列表为空时返回null，removeLast()移除并返回列表的最后一个元素。

| Modifier and type | Method and description | 
| ----------------- | ---------------------- |
|boolean		    | `add(E e)` Appends the specified element to the end of this list|
|void 			    | `add(int index, E element)` Inserts the sepcified element at the specified position in this list|
|E                  | `get(int index)` Returns the element at the specified position int this list|
|boolean            | `offer(E e)` Adds the specified element as the tail of this list|
|E                  | `peek()` Retrieves, but does not remove the head (first element) of this list| 
|E                  | `pop()` Pops an element from the stack represented by this list.|
|void               | `push(E e)` Pushes an element onto the stack represented by this list.

* LinkedList作为栈

```java
import java.util.LinkedList;
public class Stack<T> {
	private LinkedList<T>stack=new LinkedList<T>();
	/**
	 * push element
	 * @param v
	 */
	public void push(T v){
		stack.addFirst(v);
	}
	/**
	 * pop up & remove the head element
	 * @return
	 */
	public T pop(){
		return  stack.removeFirst();
	}
	/**
	 * pop but does not remove the first element
	 * @return
	 */
	public T peek(){
		return stack.getFirst();
	}
	public String toString(){
		return stack.toString();
	}
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Stack<Integer> st=new Stack<Integer>();
		st.push(1);
		st.push(2);
		st.push(3);
		st.push(9);
		System.out.println(st);
		System.out.println("peek: "+st.peek());
		System.out.println("pop: "+st.pop());
	}

}

```

 [参考](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html)