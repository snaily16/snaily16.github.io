---
layout: post
title: The Art of Concurrency
date: 2024-08-30
description: Unlock the power of concurrent programming in C, with practical examples using pthreads library
tags: c code os
categories: code-for-fun
toc:
---

Hey there, fellow coders!

Are you ready to unravel the mystery of threads? Not the kind you use to sew or weave, but the kind that makes your computer run multiple tasks at same time. Yeah, that kind! No yarns, threads, or needles required!
In this post, we'll dive into the world of pthreads in C, where we'll learn how to create, manage, and synchronize threads to achieve concurrency and improve system performance. We'll cover the basics of pthreads, exploring the best practices to follow and common pitfalls to avoid. So, buckle up and get ready to thread your way through the world of computer science!


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/threading/Cooking_multitasking.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
Imagine you're cooking dinner and have to prepare multiple dishes at once. Cooking one dish after the other would take a lot of time; instead, you prepare each dish simultaneously. You're chopping vegetables while the rice is cooking, and then preparing rotis while the vegetables are sautéing. This is an example of concurrency from day-to-day life.

<blockquote>
"Nothing is particularly hard if you divide it into small jobs." – Henry Ford
</blockquote>
In concurrency, multiple tasks are executed simultaneously, improving the overall efficiency and productivity of the system. In today's world of multi-core processors and increasingly complex software systems, concurrency has become a crucial aspect of programming. By harnessing the power of concurrency, developers can create faster, more efficient, and more scalable applications that take full advantage of modern hardware. However, concurrent programming can be a daunting task, especially for those new to the field.

Let's dive into the world of concurrent programming. One of the most popular ways to achieve concurrency in C programming is by using POSIX threads, also known as pthreads.

## What are pthread?
POSIX threads, also known as pthreads, developed by IEEE, as a part of POSIX (Portable Operating System Interface) standard, are a set of APIs that allow developers to create multiple threads of execution within a single process. 
POSIX threads were developed in 1990s to provide a standardized threading API, addressing lack of portability and efficiency in concurrent code. While there were existing threading APIs, they were not compatible, making it difficult to write portable code. Pthreads aimed to provide a standardized API for multiple platforms.

The `pthreads.h` header file is the primary interface for POSIX threads. It provides a set of functions, macros, and types that enable developers to create, manage, and synchronize threads.

## Thread Operations
In a POSIX threads environment, thread operations typically include -

1. Thread Creation: creating new threads within a process
2. Termination: terminating threads when they're no longer needed
3. Synchronization: coordinating access to shared resources between threads
4. Scheduling: managing the execution of threads by the OS
5.  Data management: managing shared data between threads
6.  Process interaction: interacting with the parent process and other threads


The Pthread API contains around 100 functions. These functions can be grouped in four major groups:

- Thread management functions `(e.g., pthread_create(), pthread_join()`)
- Synchronization functions `(e.g., pthread_mutex_lock(), pthread_cond_wait())`
- Thread attribute functions `(e.g., pthread_attr_init(), pthread_attr_setdetachstate())`
- Thread-specific data functions `(e.g., pthread_setspecific(), pthread_getspecific())`

## Thread Management
When a program starts, it consists of a single, default thread, often referred to as the main thread. This thread is responsible for executing the main() function, which is the entry point of the program.
To get thread ID for the newly created thread, we use the `pthread_self` function. 

### Thread Creation: pthread\_create

To create a new thread, we use the `pthread_create` function.
```c
 int pthread_create(pthread_t * thread, 
                       const pthread_attr_t * attr,
                       void * (*start_routine)(void *), 
                       void *arg);
```
Let's break down each argument:
- `thread`: a pointer to a `pthread_t` variable that will store thread ID.
- `attr`: a pointer to a `pthread_attr_t` structure that specifies thread attributes. (optional)
- `start_routine`: a pointer to function that the new thread will execute. 
- `arg`: an argument to be passed to the `start_routine` function. It must be passed by reference as (void \*) NULL

The `pthread_create` function returns zero on successful thread creation; otherwise, an error number shall be returned to indicate the error.

#### Best Practices for Thread Creation
1. Use meaningful thread names to identify threads, this makes debugging easier.
2. Use thread attributes to specify the thread's scheduling policy, priority and other properties.
3. Pass arguments carefully to avoid data corruption and ensure thread safety.

### Thread Termination: pthread\_exit

To terminate the calling thread, we use the `pthread_exit` function. It takes a single argument, `value_ptr`, which is a pointer to the exit status of the thread. This argument is optional, and if not provided, the thread exits with a default status.
```c
void pthread_exit(void *value_ptr);
```
This function allows thread to exit cleanly, realising any resources it may have allocated. While using the `pthread_exit`.

#### Best Practices for Thread Termination
1. Use `pthread_exit` instead of `return` to terminate the thread. This ensures that the thread exists cleanly and releases resources it may have allocated.
2. Pass on exit status to `pthread_exit` to provide information about the thread termination.
3. Avoid calling `pthread_exit` from signal handler to prevent unexpected behaviour.
4. Use `pthread_join` to wait for the thread to terminate and ensure that resources are released.

### Waiting for Thread Termination: pthread\_join
In addition to creating threads, it's essential to manage their termination properly. One of the most critical aspects of thread termination is waiting for a thread to finish its execution. This is where `pthread_join` comes into play. When `pthread_join` is called the main program will block until the specified thread terminates.
```c
int pthread_join(pthread_t thread, void **value_ptr);
```

Let's break down each argument:
- `thread`: a pointer to a `pthread_t` variable that will store thread ID.
- `value_ptr`: a pointer to a location where the thread's return value will be stored.

Once the thread finishes the execution, `pthread_join` will return the thread's return value through the `value_ptr` argument. It allows you to synchornize the main program with the thread's execution. This is particularly useful when the main program needs to wait for a thread to complete a task before proceeding.
If a thread encounters an error, `pthread_join` can help you catch and handle the error properly.

#### Best Practices for Using pthread\_join
1. Make it a habit to join threads to ensure proper resource cleanup and synchronization
2. Always pair `pthread_join` with `pthread_create` to ensure that threads are properly created and terminated.
3. Use `pthread_join` to catch and handle errors that may occur during thread execution.

Here is an example code that demonstrates the best practices:

```c
#include<pthread.h>
#include<stdio.h>
#include<stdlib.h>

void *thread_func(void *arg);

void main(){
    pthread_t thread;
    const char *msg = "Hello, Thread!!!";
    int iret;

    // Create a thread
    iret = pthread_create( &thread, NULL, thread_func, (void*) msg);
    if (iret) {
        fprintf(stderr, "Error - pthread_create() return code: %d\n", iret);
        exit(EXIT_FAILURE);
    }
    printf("pthread_create for thread returns: %d\n", iret);
    
    // Wait for the thread to finish
    void *retval;
    pthread_join(thread, &retval);
    printf("Thread finished with return value %d\n", (int)retval);
    
}

void *thread_func(void *arg){
    // Get the argument passed to the thread
    char *message = (char *)arg;

    // Get the thread ID
    pthread_t tid = pthread_self();
    printf("Thread %ld started with message: %s\n", (long)tid, message);
    
    // Return a value to indicate success
    pthread_exit((void*)1);
}
```

To compile this program using GCC, we need to use the `-pthread` flag. This flag tells the compiler to link against the Pthread library. If you're using a different C compiler, you'll need to use a different (but equivalent) flag to link against the Pthread library. For example, with Clang compiler, we use the `-lpthread` flag.

Below is the example of how we can compile the above code (let's name it thread\_basic.c) in GCC:
```
gcc -o thread_basic thread_basic.c -pthread
```

## Thread Synchronization

When working with pthreads, synchronization is key to ensuring that your threads play nicely together. It is curcial to ensure that threads access shared resources safely and efficiently. Pthreads provide a range of synchronization functions that allow threads to coordinate their actions and avoid race conditions.

Types of Synchronization Functions:
1. Mutexes (Mutual Exclusion): Allow only one thread to access a shared resource at a time.
2. Condition Variables: Allow threads to wait for a specific condition to occur before proceeding.
3. Semaphores: Allow a limited number of threads to access a shared resource.

To illustrate the concept of synchronization functions, let's consider the earlier example of cooking dinner, but lets add multiple chefs.

### Problem Statement: The Dinner Party Story

Imagine a busy kitchen where multiple chefs (threads) are working together to prepare a multi-course dinner for a large party. The kitchen has limited resources, such as ovens, stoves and utensils, which need to be shared safely among the chefs. 

### Example 1: Critical Section Problem with Mutexes

Two chefs Alex and Luke, need to access the oven to bake different dishes. Without synchronization, both chefs might try to access the oven simultaneously, leading to conflicts and potentially ruining the dishes. To avoid this problem, the chefs use a mutex lock to synchronize the access to the oven.

A mutex (short for "mutual exclusion") is a synchronization primitive that allows only one thread to access a shared resource at a time. In this example we'll use the `pthread_mutex_lock` function to lock the mutex, ensuring that only one chef can access the oven at a time.

#### Declaration: 
```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

`pthread_mutex_lock` locks the mutex specified by varibale "mutex". If the mutex is already locked, the calling thread will block until the mutex is unlocked by `pthread_mutex_unlock`. 
The `pthread_mutex_trylock` is similar to `pthread_mutex_lock`, except that if the mutex object referenced by mutex is currently locked, instead of blocking the thread, it return immediately.

The `pthread_mutex_lock` and `pthread_mutex_unlock` will returns 0 on success and error code on failure. The `pthread_mutex_trylock` will return 0 if a lock on the mutex object referenced by mutex is acquired, otherwise, and error code.

#### The Code:
Here is the code snippet that demonstrates how the chefs use a mutex lock to synchronize their actions:

```c
pthread_mutex_t oven_mutex = PTHREAD_MUTEX_INITIALIZER;

void* Chef1(void* arg){
    pthread_mutex_lock(&oven_mutex);
    // Bake dish 1
    printf("Chef 1 is baking a cake... \n");
    pthread_mutex_unlock(&oven_mutex);
    return NULL;
}

void* Chef2(void* arg){
    pthread_mutex_lock(&oven_mutex);
    // Bake dish 2
    printf("Chef 2 is baking a pizza... \n");
    pthread_mutex_unlock(&oven_mutex);
    return NULL;
}
```
#### How it Works: 
- The mutex `oven_mutex` is shared by Chef1 and Chef2 and is initialized using `PTHREAD_MUTEX_INITIALIZER`.
- They create their threads and start executing their respective thread functions. When Chef1 tries to access the oven, it locks the mutex using `pthread_mutex_lock`.
- Once the mutexis locked, Chef1 enters the critical section and bakes the cake.
- Meanwhile, if Chef2 also access the oven and see the mutex in locked state, it will block until the mutex is unlocked by Chef1. 
- After baking the cake, Chef1 unlocks the mutex using `pthread_mutex_unlock`.
- And similarly, chef2 locks the mutex, bakes the pizza and unlocks the mutex. 

By using a mutex lock, the chefs ensure that only one of them can access the oven at a time, preventing conflictst and ensuring that each dish is prepared perfectly.

### Example 2: Producer-Consumer Problem with Condition Variables

Now, one of your chef, let's say Claire, is responsible for preparing ingredients, while another chef, Phil, needs to use those ingredients to prepare a dish. 
Without synchronization, Phil (consumer) might try to use the ingredients before Claire (producer) has finished preparing them, reuslting in a dish that is incomplete or incorrect. For instance, Claire might be preparing a sauce, while Phil tries to use it before it's ready, leading to a culinary disaster.

To avoid this problem, the chefs use a condition variable to synchronize their actions. A condition variable is a synchronization primitive that allows threads to wait until a specific condition is met. In this example we'll use the `pthread_cond_wait` and `pthread_cond_signal` functions to implement the producer-consumer synchronization.

#### Declaration:

```c
// wait on a condition
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
int pthread_cond_timedwait(pthread_cond_t *cond, 
                           pthread_mutex_t *mutex, const struct timespec *abstime);

// signal or broadcast a condition
int pthread_cond_signal(pthread_cond_t *cond);
int pthread_cond_broadcast(pthread_cond_t *cond);
```

1. Wait on a condition:
- Releases the mutex specified by `mutex` and waits until the condition varibale `cond` is signaled.
- They are called with mutex locked by the calling thread.
- Returns 0 on success, or an error code on failure
- The `pthread_cond_timedwait()` function is the same as `pthread_cond_wait()` except that an error is returned if the absolute time specified by abstime passes before the condition cond is signaled or broadcasted, or if the absolute time specified by abstime has already been passed at the time of the call.

2. Signal or broadcast a condition:
- Signals the condition variable `cond`, waking up one or more threads that are waiting on it.
- Returns 0 on success, or an error code on failure
- The `pthread_cond_signal` wakes up at least one of the thread that are waiting on `cond`, whereas the `pthread_cond_broadcast` wakes up all the threads that are waiting.
- There is no effect for these functions if there are no threads currently blocked on `cond`.

#### The Code:
Here is the code snippet that demonstrates how the chefs use a condition variable to synchronize their actions:

```c
pthread_mutex_t sauce_mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t sauce_cond = PTHREAD_COND_INITIALIZER;

int sauce_ready = 0;

// Producer: Claire
void Chef_Producer(void *arg){
    pthread_mutex_lock(&sauce_mutex);
    // Prepare the sauce
    printf("Chef Claire is preparing the sauce...\n");
    sauce_ready = 1;
    pthread_cond_signal(&sauce_cond);
    pthread_mutex_unlock(&sauce_mutex);
    return NULL;
}

// Consumer: Phil
void Chef_Consumer(void *arg){
    pthread_mutex_lock(&sauce_mutex);
    while (sauce_ready == 0){
        pthread_cond_wait(&sauce_cond, &sauce_mutex);
    }
    // Use the sauce to prepare the dish
    printf("Chef Phil is preparing the dish...\n");
    sauce_ready = 0;
    pthread_mutex_unlock(&sauce_mutex);
    return NULL;
}
```

#### How it Works:
- The mutex `sauce_mutex` and condition varibale `sauce_cond` are initialized using `PTHREAD_MUTEX_INITIALIZER` and `PTHREAD_COND_INITIALIZER`, respectively.
- Claire (the producer) creates its thread and starts executing its thread function, she prepares the sauce and sets the `sauce_ready` flag to 1. 
- Claire signals the condition variable using `pthread_cond_signal`, waking up Phil (the consumer).- Phil waits on the condition variable using `pthread_cond_wait` until the sauce is ready. Once its ready, Phil uses it to prepare the dish.
- Phil resets the `sauce_ready` flag to 0 to indicate that the sauce is no longer available.

By using a condition variable, both Claire and Phil ensures that Phil wait until Claire has finished preparing the sauce, ensuring the dish is prepared correctly.

However, this code can cause a deadlock if multiple consumers are present.

Here's why:

1. If multiple consumer threads are waiting on the sauce_cond condition variable, only one of them will be woken up and will continue executing.
2. The other consumer threads will still be waiting on the sauce_cond condition variable, holding the sauce_mutex lock.
3.  When the producer thread tries to produce the sauce again, it will block on the sauce_mutex lock, waiting for one of the consumer threads to release it.
4. But the consumer threads are still waiting on the sauce_cond condition variable, and none of them can release the sauce_mutex lock because they are blocked.
5. This creates a deadlock situation, where the producer thread is waiting for a consumer thread to release the sauce_mutex lock, and the consumer threads are waiting for the producer thread to signal the sauce_cond condition variable again.

By using `pthread_mutex_lock` and `pthread_mutex_unlock` with condition variables, we can ensure that only one thread can access the critical section of code at a time, preventing race conditions and ensuring thread safety.

### Example 3: Reader-Writer Problem with Semaphores

Our head chef, Gloria, has meticulously recorded all her grandmother's recipes in her treasured recipe book, which only she is allowed to modify. Her team of chefs, including Phil, Claire, Alex and Luke, need to access to prepare different dishes. 

Without proper synchronization, multiple chefs might attempt to access the recipe book simultaneously, leading to inconsistencies and errors. For instance, if one chef is reading a recipe while another chef is updating it, the first chef might obtain an outdated or incorrect recipe. To avoid this problem, the chefs utilize a semaphore to synchronize their access to the recipe book. A semaphore is a synchronization primitive that allows a limited number of threads to access a shared resource.

#### Declaration

```c
// lock a semaphore
int sem_wait(sem_t *sem);
int sem_trywait(sem_t *sem);

// unlock a semaphore
int sem_post(sem_t *sem);
```

1. Lock a semaphore:
- Decrements the semaphore value.
- If the semaphore value is currently zero, `sem_wait` blocks the calling thread until the semaphore value becomes positive. When semaphore value is positive, it decrements the value and allows the thread to proceed.
- Unlike `sem_wait`, `sem_trywait` does not block the calling thread if the semaphore value is zero. Instead, it returns an error code. 
- Returns 0 on success, or an error code on failure.

2. Unlock a semaphore:
- The `sem_post` call, increments the semaphore value and wakes up one or more threads that are waiting on it.
- Returns 0 on success, or an error code on failure.

#### The Code:

Here's an example code snipped that demonstrates how the chefs use a semaphore to synchronize their access to the recipe book:

```c

sem_t recipe_book_semaphore;
#define READERS 5

void* ChefReader(void *arg) {
    sem_wait(&recipe_book_semaphore);
    // Read the recipe book
    printf("Chef %ld is reading recipe book...\n", (long)arg);
    sem_post(&recipe_book_semaphore);
    return NULL;
}

void *HeadChef(void *arg){
    // Wait for all readers to finish
    for(int i=0; i<READERS; i++)
        sem_wait(&recipe_book_semaphore);
    // Update the recipe book
    printf("Head Chef is updating recipe book...\n");
    sem_post(&recipe_book_semaphore);
    return NULL;
}

int main(){
  sem_init(&recipe_book_semaphore, 0, READERS); // Allow up to n readers to access simultaneously
  // Create threads here...
  sem_destroy(&recipe_book_semaphore);
  return 0;
}
```
#### How it Works:
- The semaphore `recipe_book_semaphore` is initialized uisng the `sem_t` type. This semaphore is used to synchronize access to the recipe book.
- When a ChefReader thread arrives, it waits for the semaphore to be available using sem_wait. If the semaphore is available, the thread decrements its value and continues execution. Since multiple ChefReader threads can decrement the semaphore value simultaneously, multiple readers can access the recipe book at the same time.
- When a HeadChef thread arrives, it also waits for the semaphore to be available using sem_wait. However, since the semaphore value is decremented by the ChefReader threads, the HeadChef thread will block until all ChefReader threads have finished reading the recipe book and released the semaphore. This ensures that the HeadChef thread has exclusive access to the recipe book when updating it.
- When a ChefReader thread finishes reading the recipe book, it releases the semaphore using sem_post, incrementing its value. This allows other ChefReader threads to access the recipe book.
- When the HeadChef thread finishes updating the recipe book, it releases the semaphore using sem_post, incrementing its value. This allows other threads to access the recipe book.

By using a semaphore to synchronize access to the recipe book, the code ensures that multiple ChefReader threads can read the recipe book simultaneously, while the HeadChef thread has exclusive access to the recipe book when updating it.

### Conclusion
And that's a wrap, folks! We've covered the basics of pthreads and synchronization functions in C. From creating threads to synchronizing access to shared resources, we've learned how to cook up some tasty multi-threaded programs.

Think of it like cooking a meal - you need to make sure all the ingredients are ready at the right time, and that each step is executed in the right order. With pthreads and synchronization functions, you can ensure that your threads are working together in harmony, just like a well-coordinated kitchen team.

I hope this blog has been a recipe for success in understanding pthreads and synchronization. If you have any questions or need further clarification, please don't hesitate to ask! Happy coding, and bon appétit!
