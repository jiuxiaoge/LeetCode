---
title: 【LeetCode】Day3 Implement Queue using Stacks AND Implement Stack using Queues
date: 2018-10-12 11:29:00
categories:
- 算法
- LeetCode
tags:
- 算法
- LeetCode
- 栈
---
两个题，用双栈实现队列和用双队列实现栈！
<!--more-->
# 题目描述
Implement the following operations of a queue using stacks.

- push(x) -- Push element x to the back of queue.
- pop() -- Removes the element from in front of queue.
- peek() -- Get the front element.
- empty() -- Return whether the queue is empty.

**Example**

~~~python
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
~~~
## 理解题意
利用栈来实现队列的相关操作。
## 算法
考虑使用双栈来实现队列。
1. 创建两个栈，s1和s2
2. s1负责入队列操作，s2负责出队列操作
3. 入队列正常将其压入栈s1
4. 出队列，先判断s2是否为空，如果不为空，则弹出s2的栈顶元素。否则先将栈s1的元素全部压入栈s2，再将栈s2的栈顶元素弹出。
5. 返回首元素，类似出队列。
6. s1和s2都为空的话则队列为空。

比较简单，直接上代码
~~~C++
#include <iostream>
#include <stack>

using namespace std;
class MyQueue {
public:
    /** Initialize your data structure here. */
    //定义两个栈
    stack<int> s1;
    stack<int> s2;
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
            int a = s2.top();
            s2.pop();
            return a;
        }
        else{
            int b = s2.top();
            s2.pop();
            return b;
        }
    }

    /** Get the front element. */
    int peek() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
            return s2.top();
        }
        else{
            return s2.top();
        }
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        if(s2.empty() && s1.empty()){
            return true;
        }
        else{
            return false;
        }
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * bool param_4 = obj.empty();
 */
int main()
{
    MyQueue queue;

    queue.push(1);
    queue.push(2);
    cout << queue.peek() << endl;  // 返回 1
    cout << queue.pop() << endl;  // 返回 1
    cout << queue.empty() << endl; // 返回 false
}

~~~
用时截图

![在这里插入图片描述](https://img-blog.csdn.net/20181012114032131?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTI1OTIxMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

# 题目描述
Implement the following operations of a stack using queues.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- empty() -- Return whether the stack is empty.

**Example**

~~~python
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
~~~
## 理解题意
使用队列来模拟栈的操作。

和上一题类似。
## 算法
采用双队列来模拟栈。
1. 创建两个队列q1和q2。
2. q1负责放栈中的元素，q2负责出栈操作。
3. 入栈，直接在q1中进行入队列操作。
4. 出栈，先将除了q1的队尾元素的其他元素全部入队q2，再将q1的最后一个元素出队，再将q2的所有元素入队q1
5. 栈顶元素，直接返回q1的队尾元素。
6. q1为空则栈为空。

比较简单直接上代码：
~~~C++
#include <iostream>
#include <queue>
using namespace std;
class MyStack {
public:
    /** Initialize your data structure here. */
    queue<int> q1;
    queue<int> q2;
    MyStack() {

    }

    /** Push element x onto stack. */
    void push(int x) {
        q1.push(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        while(q1.size() != 1){
            q2.push(q1.front());
            q1.pop();
        }
        int a = q1.front();
        q1.pop();
        while(!q2.empty()){
            q1.push(q2.front());
            q2.pop();
        }
        return a;
    }

    /** Get the top element. */
    int top() {
        return q1.back();
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        if(q1.empty()){
            return true;
        }
        else{
            return false;
        }

    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * bool param_4 = obj.empty();
 */
int main()
{
    MyStack stack;

    stack.push(1);
    stack.push(2);
    cout << stack.top() << endl;   // returns 2
    cout << stack.pop() << endl;  // returns 2
    cout << stack.empty() << endl; // returns false
}
~~~
看一下运行时间：

![运行时间](https://img-blog.csdn.net/20181012121528983?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTI1OTIxMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

