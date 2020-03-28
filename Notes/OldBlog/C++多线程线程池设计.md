---
title: C++多线程设计
date: 2019-01-25 22:14:51
categories: C++学习笔记
tags: [计算机科学]
---
##C++线程池代码
头文件
```
#ifndef THREADPOOLS_H
#define THREADPOOLS_H

#include <thread>
#include <functional>
#include <condition_variable>
#include <mutex>
#include <deque>
#include <memory.h>
#include <vector>
using namespace std;
class ThreadPools
{
public:
    using Task = std::function<void()>;
    ThreadPools(int num_thread = 5);
    ~ThreadPools();
    void thread_run();
    void start_threads();
    void add_task_to_queue(Task task);
    void stop();
    std::condition_variable condition;
    std::mutex queue_mutex;
    vector<unique_ptr<thread>> threads;
    deque<Task> tasks_queue;
    int num_thread;
    bool is_running = false;
};
#endif // THREADPOOLS_H

```
代码具体实现
```
#include "threadpools.h"
#include <iostream>


ThreadPools::ThreadPools(int num_thread):num_thread(num_thread){}

ThreadPools::~ThreadPools(){
    stop();
}

void ThreadPools::thread_run()
{
    while(is_running){
        Task task;
        {
            unique_lock<mutex> lock(queue_mutex);
            while(tasks_queue.empty()&&is_running){
                condition.wait(lock);
            }
            if(!tasks_queue.empty()){
                task = tasks_queue.front();
                tasks_queue.pop_front();
            }
        }
        if(task){
            task();
        }
    }
}

void ThreadPools::start_threads()
{
    is_running = true;
    for(int i = 0;i<num_thread;i++){
        threads.emplace_back(new thread(std::bind(&ThreadPools::thread_run,this)));
    }
}

void ThreadPools::add_task_to_queue(Task task)
{
    {
        unique_lock<mutex> lock(queue_mutex);
        tasks_queue.push_back(std::move(task));
    }
    condition.notify_one();
}

void ThreadPools::stop()
{
    {
        unique_lock<mutex> lock(queue_mutex);
        is_running = false;
        condition.notify_all();
    }
    for(auto &t:threads){
        t->join();
    }
    cout<<tasks_queue.size()<<endl;
}

```