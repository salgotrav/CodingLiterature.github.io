---
layout: post
title: Functors in C++ - Part I
tags: [ c++ ]
comments : true
---

## What is a functor? ##

Functor or function object is a C++ class which defines the operator **( )**. Functor let's you create objects which "looks like" functions.

Consider below code to add two numbers:

```c
#include<cassert>

struct MyAddFunctor {
	
	// Constructor
	MyAddFunctor(int inp){
		x = inp;
	}
	
	// Defining operator()
	int operator()(int y){
		return x+y;
	}
	
	int x;
};

int main(){
	MyAddFunctor func(5);
	int ret = func(10);
	//ret would be 15.
	assert(ret == 15);
	
	int ret2 = func(25);
	// ret would br 30
	assert(ret2 == 30);
}
```

Now **MyAddFunctor** is a functor. It is a functor because **MyAddFunctor** is C++ class which implemented operator().

## How are functors called?

We call functors just like regular C++ class. Unlike functions we first need to create object of functor like ```MyAddFunctor func(5)```. We are passing 5 as argument only becase we have a constructor defined in **MyAddFunctor** class which takes int as an argument.

Now when object of functor class is created we invoke the function using () operator just like `func(10)`.

We could also combine instantiation and invocation in single statement as `MyAddFunctor(5)(10)`.

## Why are they called as Function Objects?

The reason why they are called as function objects is because we can call class **MyAddFunctor** as if it is a function. Example : ```func(10)```.

## Why are functors used? ##

The main advantage of using functor is that they store state. Consider our previous example of **MyAddFunctor**. In that example we stored value of x in the class and so we didn't need to pass x in each call.

One can argue that the work done by **MyAddFunctor** can simply be done by writing C++ function as below:

```c
int addFunction(int x){
	return 5+x;
}
```
But in **addFunction** we are hardcoding the value 5 and in functor we are not doing so. So **MyAddFunctor** is more customizable. 

Functors are often used in STL. We will talk about functors more in next blog post. Till then stay tuned and let me know your comments on this post.

Thanks

