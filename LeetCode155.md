
---
title: 【LeetCode】Day2 Min Stack
date: 2018-10-11 15:15:40
categories:
- 算法
- LeetCode
tags:
- 算法
- LeetCode
- 栈
---
两次优化改进了最原始的比较方法，通过牺牲了部分空间来换取了更快的运行速度。
<!--more-->
# 题目描述
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

**Example:**
~~~python
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
~~~
## 理解题意
大致就是设计一个栈，这个栈具有找出栈中最小元素的功能。设计栈不难，难点在于如何提高算法的效率。
## 算法
### 方法一
1. 根据栈的特点设计出来一个简单的栈。
2. 依次查找比较找出最小的元素并输出。

这是最先想到的，也是效率比较低的。
~~~C++
class MinStack {
public:
    /** initialize your data structure here. */
    int a[100010];
    int top_number;
    MinStack() {
        top_number = -1;
    }

    void push(int x) {
        a[++top_number] = x;
    }

    void pop() {
        top_number -= 1;
    }

    int top() {
        return a[top_number];
    }

    int getMin() {
        int min_number_index = 0;
        for(int i = 1 ; i <= top_number ; i++){
            if(a[i] < a[min_number_index] || a[i] == a[min_number_index]){
                min_number_index = i;
            }
        }
        return a[min_number_index];
    }
};
~~~
经过提交，虽然正确，但用时相当长！

![在这里插入图片描述](https://img-blog.csdn.net/20181011181551933?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTI1OTIxMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
### 优化一
1. 创建两个栈，栈**s1**用来放所有数据，栈**s2**用来动态存放最小值。
2. 在s2中放入一个最大的int数最为保底。2147483647
3. 每次压入元素时，将其压入s1，在判断是否小于或等于栈顶的元素，如果判断结果为真，则将其压入s2，否则不压入。
4. 取最小值时，就从栈s2中取栈顶元素即可。

~~~C++
class MinStack {
public:
    /** initialize your data structure here. */
    //建立两个栈，一个栈存放所有数据，一个栈放小数据
    stack<int> s1;
    stack<int> s2;
    MinStack() {
        s2.push(2147483647);
    }

    void push(int x) {
        s1.push(x);
        if(x < s2.top() || x == s2.top()){
            s2.push(x);
        }
    }

    void pop() {
        if(s1.top() == s2.top()){
            s2.pop();
            s1.pop();
        }
        else{
            s1.pop();
        }
    }

    int top() {
        return s1.top();
    }

    int getMin() {
        return s2.top();
    }
};
~~~
我们再看一下运行的时间，会发现提高了不少，但还是和最好的方法有一定差距。

![在这里插入图片描述](https://img-blog.csdn.net/20181011182424946?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTI1OTIxMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
### 优化二
> 想换取时间，可以通过牺牲空间。

根据这个思想，我们可以考虑将栈对象换成一个栈结构体，也就是栈中的存储单元变成了结构体。
1. 创建一个以结构体为存储单元的栈。
2. 结构体含有一个数值，一个最小值。

这个最小值是代表在当前元素及其之下的元素中的最小值。

**核心算法**
~~~C++
    struct min_s{
        int val;
        int min_val;
    };
    stack<min_s> s1;
    void push(int x) {
            if(!s1.empty()){
                s1.push(min_s{x , x < s1.top().min_val ? x : s1.top().min_val});
            }
            else{
                s1.push(min_s{x,x});
            }
        }
~~~

> s1.push(min_s{x , x < s1.top().min_val ? x : s1.top().min_val});

这句就是我们的核心代码，通过压入一个结构体，再用一个条件判断符来确定**min_val**的值。

完整的代码：

~~~C++
class MinStack {
public:
    /** initialize your data structure here. */
    struct min_s{
        int val;
        int min_val;
    };
    stack<min_s> s1;
    MinStack() {

    }

    void push(int x) {
        if(!s1.empty()){
            s1.push(min_s{x , x < s1.top().min_val ? x : s1.top().min_val});
        }
        else{
            s1.push(min_s{x,x});
        }
    }

    void pop() {
        s1.pop();
    }

    int top() {
        return s1.top().val;
    }

    int getMin() {
        return s1.top().min_val;
    }
};
/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
~~~

来看一下我们的用时，又进步啦！！

![在这里插入图片描述](https://img-blog.csdn.net/2018101119021177?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTI1OTIxMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

