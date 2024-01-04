## io

### 阻塞io

![image](https://github.com/2Youself/learn/assets/135308244/2f20958d-17e9-428d-a14f-54b7539feb03)

### 非阻塞io

![image](https://github.com/2Youself/learn/assets/135308244/f0780f58-6837-4018-9d47-a3c7e38885ad)

### 信号驱动io

![image](https://github.com/2Youself/learn/assets/135308244/b4117884-6242-4548-aefc-cf493bf9c989)

### 多路io复用

![image](https://github.com/2Youself/learn/assets/135308244/6838aa44-6b40-4299-8444-3f06a4d23f5d)

### 异步io

![image](https://github.com/2Youself/learn/assets/135308244/0f8c355c-ef44-4bd6-8583-830896a0f969)

### io阻塞点

![image](https://github.com/2Youself/learn/assets/135308244/c1a8c463-918b-4924-aa47-17a8c40b3f93)

### tcp连接过程

![image](https://github.com/2Youself/learn/assets/135308244/62c0cdc5-d1e5-44b9-adb0-3b9498a083d7)

### select和epoll区别

#### select的缺点

1、被监控的fds需要从用户空间拷贝到内核空间为了减少数据拷贝带来的性能损坏，内核对被监控的fds集合大小做了限制，并且这个是通过宏控制的，大小不可改变(限制为1024)。
2、被监控的fds集合中，只要有一个有数据可读，整个socket集合就会被遍历一次调用sk的poll函数收集可读事件由于当初的需求是朴素，仅仅关心是否有数据可读这样一个事件，当事件通知来的时候，由于数据的到来是异步的，我们不知道事件来的时候，有多少个被监控的socket有数据可读了，于是，只能挨个遍历每个socket来收集可读事件。

#### epoll的解决方案

1、epoll引入了epoll_ctl系统调用，将高频调用的epoll_wait和低频的epoll_ctl隔离开。
2、epoll引入了一个中间层，一个双向链表(ready_list)，一个单独的睡眠队列(single_epoll_wait_list)，并且，与select或poll不同的是，epoll的process不需要同时插入到多路复用的socket集合的所有睡眠队列中，相反process只是插入到中间层的epoll的单独睡眠队列中，process睡眠在epoll的单独队列上，等待事件的发生。同时，引入一个中间的wait_entry_sk，它与某个socket sk密切相关，wait_entry_sk睡眠在sk的睡眠队列上，其callback函数逻辑是将当前sk排入到epoll的ready_list中，并唤醒epoll的single_epoll_wait_list。而single_epoll_wait_list上睡眠的process的回调函数就明朗了：遍历ready_list上的所有sk，挨个调用sk的poll函数收集事件，然后唤醒process从epoll_wait返回。
