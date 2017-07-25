---
layout: post
title: Functors in C++ - Part II
tags: [ c++ ]
comments : true
---

This post is in continuation to [Functors in C++ - Part I](https://mayankj08.github.io/2017/07/02/Functors-In-C++/).

In last blog we talked about basics of functors. In this post let's dive deep into functors and talk about some application of functors.

Functors are commonly used in STL algorithms. The power of functors can clearly be realised when we use them with STL. 

## STL Functors ##

There is one more good thing to this whole story. We don't have to write all functors by ourselves. STL provides some built-in functors for basic operations like addition, divison, some bitwise operators etc. 
Some of the STL Functors are^:

* Binary Arithmetic Functors : plus (for addition, equivalent to arg1 + arg2), minus (for subtraction, same as arg1 - arg2 ), divides (division) etc. ^^
* Binary Logical Functors : logical_and, logical_or
* Unary Functors : negate, logical_not.

Binary functors are functors those take two parameters while unary take only one parameter. 

^ Complete list and their descriptions can be found [here](http://en.cppreference.com/w/cpp/utility/functional).   
^^ arg1 and arg2 are two input parameters to functors. Example: `std::plus<int>()(arg1 + arg2)`

Let's take a example here. Assume we need to multiply each element of vector by -1. This can be done very easily as:

```c
// Compile this code with -std=c++11 flag.
// like: g++ -std=c++11 main.cpp

#include<vector>
#include<algorithm>
#include<functional>
#include<iostream>

int main(){

        // Initializing vector with 1,2 and 3 as it's elements
        std::vector<int> vec = {1,2,3};

        // Transforming each element of vector by multiplying 
	// it with -1. First argument to std::Transform is 
	// start iterator. Second argument is end iterator
        // Third argument says where we need to store the 
	// result. Here we are storing result in vec itself.
        // And the last argument says which functor do we 
	// need to call for each element of vector from 
	// start iterator to end iterator
        std::transform(vec.begin(),vec.end(),
		vec.begin(),std::negate<int>());

        // print the transformed vector
        for(int i=0;i<vec.size();i++){
                std::cout << vec[i] << ' ';
        }
        // output would be
        // -1 -2 -3
}
```

It's so easy and much more readble than doing same thing without using STL. Isn't it!?

But we do have one problem here if we use binary functor with std::transform instead of unary functor. Binary functor takes two input arguments while std::transform expects functor with just one argument. In this scenario `std::bind` can be useful.

## Bind ##

Let's take a example where we need to add 5 to each element of std::vector, vec1 and store the result in another vector named vec2. How can this be done using std::transform and std::plus?

Here's the solution:

```c
// Compile this code with -std=c++11.
// g++ -std=c++11 main.cpp

#include<vector>
#include<algorithm>
#include<functional>
#include<iostream>

#define ELEM_TO_ADD 5

int main(){

        // Initializing vector with 1,2 and 3 as three elements
        std::vector<int> vec1 = {1,2,3};
        std::vector<int> vec2;

        std::transform(vec1.begin(),vec1.end(),
			std::back_inserter(vec2),
                        std::bind(std::plus<int>(),
				std::placeholders::_1,
				ELEM_TO_ADD)
		      );

        // print the transformed vector
        for(int i=0;i<vec2.size();i++){
                std::cout << vec2[i] << ' ';
        }
        // output would be
        // 6 7 8
}
```

Let's try to understand logic of above piece of code. `std::transform` as discussed previously invoke the functor (specified by last argument) for each element iterating from vec1.begin() to vec1.end(). Before moving forward let's first understand what `std::bind` does.


`std::bind` is [partial function application](https://en.wikipedia.org/wiki/Partial_application). What this means is suppose you have a function `func1` which takes two args as `func1(arg1,arg2)`.

Now, you want to define a new function `func2` as:

```c
func2(arg1){
	func1(arg1,5)
}
``` 
`func2` has been defined by fixing argument arg2, so `func2` is said as partial application of `func1`. `std::bind` does same. In terms of C++ STL `func2` can be defined as:

`auto func2 = std::bind(func1,std::placeholders::_1,5)`

What we are saying by above line is: Create `func2` by binding func1, first argument of func1 (specified as, std::placeholders::_1) and 5.^

 ^`std::placeholders::_1` means first argument, `std::placeholders::_2` means second parameter and so on.

You can do more crazy things with `std::bind` like changing order of arguments of a function.

## Back to example

Let's move back to our example.  `std::bind(std::plus<int>(),std::placeholders::_1, ELEM_TO_ADD)` in our example binds std::plus functor, first argument passed to bind (which in our case would be elements of vector, passed one by one) and constant ELEM\_TO\_ADD. In short this line transforms binary functor `std::plus` into unary functor. Another argument of functor is fixed value ELEM_TO_ADD.

Finally, we are storing the result of functor in vector, vec2, by using stl function [std::back_inserter](http://en.cppreference.com/w/cpp/iterator/back_inserter). This function just uses push_back function of vector to insert result at back of specfied vector.

So, now you might be feeling that functors are indeed powerful. In next blog we will talk about lambdas, which are quite concise way of writing anonymous functions. 

Till then [Sayonara](https://www.google.com/#q=sayonara).

# Exercise 
Can you think of solution to re-order arguments of a functions and create a new function? Basically, we want to define func3 as 

```
func3(arg1,arg2,arg3){
	func4(arg2,arg3,arg1);
}
```

If yes, let me know in comments how would you achieve this using `std::bind`

Thanks
