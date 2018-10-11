---
title: 【LeetCode】Day1 Valid Parentheses
date: 2018-10-10 17:24:46
categories:
- 算法
- LeetCode
tags:
- 算法
- LeetCode
- 栈
---
在表示问题的递归结构时，栈数据结构可以派上用场。我们无法真正地从内到外处理这个问题，因为我们对整体结构一无所知。但是，栈可以帮助我们递归地处理这种情况，即从外部到内部。
<!--more-->
# LeetCode刷题之旅
## WHY
数据结构和算法太差劲，自己撸代码的能力几乎为0。经知乎大神推荐，了解到了LeetCode这个平台，准备开始撸代码啦！
## HOW
按照tags的顺序刷题，先将简单的题做完，再提高难度。
## 题目
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.
~~~python
Input: "()"
Output: true
Input: "()[]{}"
Output: true
Input: "(]"
Output: false
Input: "([)]"
Output: false
Input: "{[]}"
Output: true
~~~
### 理解题意
意思就是有一个字符串，由“() {} []”三种符号组成，按照示例那样，所有的符号得正常匹配才可以。所以这个题就是要判断括号是否匹配。
### 解题思路
我们可以观察到，任何一个满足题意的序列，他的子序列中的括号也都是匹配的，我们可以引入**栈**这一数据结构来描述
这个问题。
![理解的图片](https://leetcode-cn.com/problems/valid-parentheses/Figures/20/20-Valid-Parentheses-Recursive-Property.png)
#### 为什么要用栈？
栈的特点是什么？**先入后出**，**后入先出**。
根据我们观察的特点:
> 任何一个满足题意的序列，他的子序列中的括号也都是匹配的

假如现在有一个满足题意要求的序列:
> {()[]}

我们先判断容易判断的（序列数少的），先判断出()和[]两者是满足题意的序列，再判断{}是满足的，这样整个序列就是匹配的了。
> 在表示问题的递归结构时，栈数据结构可以派上用场。我们无法真正地从内到外处理这个问题，因为我们对整体结构一无所知。但是，栈可以帮助我们递归地处理这种情况，即从外部到内部。

我们的分析过程，不就是从内部到外部，但我们无法真正的通过编程来实现，或者说很麻烦来实现，而栈就是一种从外部到内部来解决问题的**数据结构**！
下面这个图更清楚的展示了这一过程。
~~~python
{ { ( { } ) } }
      |_|

{ { (      ) } }
    |______|

{ {          } }
  |__________|

{                }
|________________|

VALID EXPRESSION!
~~~
### 算法
1. 设计一个栈。
2. 设计一个for循环，依次向栈中压入元素。
3. 如果压入元素和栈顶元素匹配，则将栈顶元素弹出，结束此次循环。
4. 如果压入元素和栈顶元素不匹配，将新元素压入。
5. 最后判断栈是否为空栈，是空栈则序列匹配，否则不匹配。

#### C++实现
~~~C++
class Solution {
public:
    bool isMatch(char c1, char c2){
        char a = '(', A = ')', b = '[', B = ']', c = '{' , C = '}';
        // c1是栈顶的元素，c2是要放入的元素
        if(c1 == a && c2 == A){
            return true;
        }
        else if(c1 == b && c2 == B){
            return true;
        }
        else if(c1 == c && c2 == C){
            return true;
        }
        else {
            return false;
        }
    }
    bool isValid(string s) {
        stack<char> st;
        for(int i = 0;i < s.size() ;i++)
        {
            //如果栈为空直接压入
            if(st.empty()){
                st.push(s[i]);
            }
            //如果不为空需要比较栈顶元素和要压入的元素是否匹配来判断是否压入
            else{
                if(isMatch(st.top(), s[i])){
                    //如果匹配，就弹出栈顶的元素
                    st.pop();
                }
                else{
                    //如果不匹配，就将其压入栈中
                    st.push(s[i]);
                }
            }
        }
        //经历上述操作以后，如果栈为空，则说明s匹配，否则s不匹配
        if(st.empty()){
            //为空
            return true;
        }
        else{
            //不为空，说明有不匹配的
            return false;
        }
    }
};
~~~
### 其他写法
在看了其他人的代码以后，发现还有一种更简洁的实现方法，直接用一个switch来判断，虽然速度一样，但简洁明了，值得学习。
~~~C++
class Solution {
public:
    bool isValid(string s) {
        if (s.size() %2 !=0 ) return false;
        stack<char> ss;
        for (int i = 0; i != s.size(); i++){
            switch (s[i]){
            case '(':
            case '[':
            case '{':
                ss.push(s[i]);
                break;
            
            case ')':
                if (ss.empty() || ss.top() != '(') return false;
                ss.pop();
                break;
            
            case ']':
                if (ss.empty() || ss.top() != '[') return false;
                ss.pop();
                break;
            
            case '}':
                if (ss.empty() || ss.top() != '{' ) return false;
                ss.pop();
                break;
            } 
        }
        if (ss.empty()) return true;
        return false;
    }
};
~~~