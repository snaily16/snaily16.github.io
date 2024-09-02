---
layout: post
title: The Art of Concurrency
date: 2024-08-30
description: Unlock the power of concurrent programming in C, with practical examples using pthreads library
tags: c code os
categories: code-for-fun
toc:
---

Are you ready to unravel the mystery of threads? Not the kind you use to sew or weave, but the kind that makes your computer run multiple tasks at same time. Yeah, that kind! No yarns, threads, or needles required!
In this post, we'll dive into the world of pthreads in C, where we'll learn how to create, manage, and synchronize threads to achieve concurrency and improve system performance. So, buckle up and get ready to thread your way through the world of computer science!


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


The pthreads.h header file defines a wide range of functions, including:

- Thread creation and management functions `(e.g., pthread_create(), pthread_join()`)
- Synchronization functions `(e.g., pthread_mutex_lock(), pthread_cond_wait())`
- Thread attribute functions `(e.g., pthread_attr_init(), pthread_attr_setdetachstate())`
- Thread-specific data functions `(e.g., pthread_setspecific(), pthread_getspecific())`

