---
layout: post
title: Threadpool in C++
tags: [ c++ ]
comments : true
---

**What is a ThreadPool**

A thread pool is a pool of initialized threads which are ready to take workload. Thread pool is preferred over conventional thread system when we have large number of small task to be done.

**Working**

When ThreadPool class is initialized then few number of threads are created (we may even create zero threads initially, as done in my implementation) and these newly created threads are made to wait on [condition variable](https://en.wikipedia.org/wiki/Monitor_(synchronization)#Condition_variables_2).

Now when we want a thread from our threadpool to run a new job we follow below steps:

1. Check if there is any free thread in our ThreadPool. Free thread means thread which is waiting on condition variable.
2. If there is atleast one free available then choose one free thread from pool and jump to step 5. Else continue.
3. Create few more threads like 5, 10 or so and make these new threads wait on condition variable too.
4. Now select one free thread from newly created threadpool.
5. Wake the selected thread by signalling on condition variable and give the workload to this selected thread.
6. When the workload is finished then make the thread wait on condition variable again. 

In threadpool we never destroy any thread until destructor of ThreadPool class is called. 

**API**

Following methods are available in the API:

| Signature | Parameters | Return | Description |
|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ThreadPool::ThreadPool() | None | None | Constructor. Creates zero threads and initializes all the required members of ThreadPool class. |
| ThreadPool::~ThreadPool() | None | None | Destructor. Destructs ThreadPool class. |
| int ThreadPool_run(struct ThreadPool *tp, struct ThreadID* threadId, void* (*run)(void *), void * args) | struct ThreadPool* : ThreadPool Id returned by ThreadPool constructor.  void* (*run)(void *) : Workload. Callback function which selected thread should run.  void* : Arguments to be passed to callback function. | 0 on Success  and 1 on failure | Workload is send to ThreadPool using this function.  |
| int ThreadPool_join(struct ThreadID thdid, void **ret) | struct ThreadID : ThreadId as reutrned by ThreadPool_run.  void** : Return value of callback function for a given ThreadID is returned back. | 0 on Success,and 1 on failure | This function blocks the calling thread till ThreadId passed in argument is busy with some workload and function returns when workload is completed.   In case given ThreadId is not running any workload then this function returns immediately.  |

**Implementation** 

Can be found [here](https://github.com/mayankj08/APIs/tree/master/ThreadPool).

Hope you found this post useful. Please comment in case of any query.

Thanks
