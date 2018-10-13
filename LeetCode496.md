---
title: 【LeetCode】Day4 Next Greater Element I
date: 2018-10-13 17:10:06
categories:
- 算法
- LeetCode
tags:
- 算法
- LeetCode
- 栈
---
牢记栈的解题原则就是从内到外，所以很多依靠就近原则的问题，如下一个最大数，下一个最小数，括号匹配等问题，都可以考虑使用栈来解决问题。
同样这个问题也涉及了vector动态数组容器，map键值对容器！！
<!--more-->
# 题目描述
You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

**Example 1:**
~~~python
Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
    For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
    For number 1 in the first array, the next greater number for it in the second array is 3.
    For number 2 in the first array, there is no next greater number for it in the second array, so output -1.
~~~
**Example 2:**
~~~python
Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
    For number 2 in the first array, the next greater number for it in the second array is 3.
    For number 4 in the first array, there is no next greater number for it in the second array, so output -1.
~~~
**Note:**
1. All elements in nums1 and nums2 are unique.
2. The length of both nums1 and nums2 would not exceed 1000.

## 理解题意
vector1是vector2的子集，这俩vector大小都不超过1000，且没有重复元素。现在让你找到和vector1中元素大小相等的vector2中的元素，并在vector2中找到下一个比该元素大的元素。
如果找不到，就返回一个-1。依此类推，将vector1遍历完毕。

## 算法1

刚开始也不会什么算法，就称之为暴力法吧，循环遍历nums1，内置循环遍历nums2找到和nums1中相等的元素，跳出内循环，在来到另一个内循环，找到比
这个元素大的一个元素，把它放到结果vector中。

代码如下:
~~~C++
#include <iostream>
#include <vector>
using namespace std;
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        vector<int> res;
        int i , j , k, m;
        //为下面判断是否在res中添加了元素做判断
        m = 0;
        for(i = 0 ; i < findNums.size() ; i++){
            //找到相等的元素
            for(j = 0 ; j < nums.size() ; j++){
                if(findNums[i] == nums[j]){
                    k = j;
                    break;
                }
            }


            //找到下一个较大元素
            for(j = k + 1 ; j < nums.size() ; j++){
                if(nums[j] > findNums[i]){
                    res.push_back(nums[j]);
                    m++;
                    break;
                }
            }
            // 如果没有找到较大元素,赋值一个-1
            if(m  == i){
                res.push_back(-1);
                m++;
            }
        }
        return res;
    }
};
int main()
{
    int n1[3] = {4,1,2};
    int n2[4] = {1,3,4,2};
    vector<int> nums1(n1 , n1+3);
    vector<int> nums2(n2 , n2+4);
    Solution a;
    vector<int> result = a.nextGreaterElement(nums1 , nums2);
    for(int i = 0 ; i< result.size();i++){
        cout << result[i] << endl;
    }
    return 0;
}
~~~

用时基本在20ms，在同样使用c++的算法中不算快。

## 算法2
想想这道题是在栈的标签内的，那我们考虑下能否用栈来解决问题。想起之前的LeetCode20，匹配括号的那个问题，栈的解题思想就是从内到外解决问题。
同样这道题也可以使用这样的思想。

我们的目的是找到每一个元素对应的下一个最大的元素。

为此特意学习了一下标准库**map**，这个很好理解，就是以key - value形式存储数据的一个方法。可以理解为一一映射。

在这里 key 就是我们的nums中每一个元素，对应的value就是其下一个最大的元素。最后组装为我们要的vector就可以了。

利用栈，可以帮助我们在每次入栈时，找到其对应的下一个最大的元素。

将nums反向入栈，如果栈非空并且栈顶元素小于被压入元素，就将栈顶元素弹出。直到栈空，或者栈顶元素大于被压入元素，就将其存储到**map**中。key为当前要压入的元素，value为栈顶元素值或-1。

用个例子来理解一下，假如nums为1 3 4 2，栈为s

**反向入栈开始：**

1. 操作元素2：

- 栈：[          ]

- 为空，则key = 2， value = -1

- 将2 入栈

2. 操作元素4：

- 栈：[2        ]

- 不为空，栈顶2小于4，弹出2，此时栈又为空

- 则key = 4，value = -1

- 将4入栈

3. 操作元素3

- 栈 [4            ]

- 栈顶元素4大于3，则key = 3，value = 4

- 将3入栈

4. 操作元素1

- 栈 [4 ,3 ]

- 栈顶元素3大于1，则key = 1，value = 3

- 将1入栈

我们就得到了一个**map：**

    1 : 3,
    3 : 4,
    4 : -1,
    2 : -1

再将其组装成vector即可

~~~C++
#include <iostream>
#include <vector>
#include <map>
#include <stack>
/*
1.push_back 在数组的最后添加一个数据

2.pop_back 去掉数组的最后一个数据

3.at 得到编号位置的数据

4.begin 得到数组头的指针

5.end 得到数组的最后一个单元+1的指针

6．front 得到数组头的引用

7.back 得到数组的最后一个单元的引用

8.max_size 得到vector最大可以是多大

9.capacity 当前vector分配的大小

10.size 当前使用数据的大小

11.resize 改变当前使用数据的大小，如果它比当前使用的大，者填充默认值

12.reserve 改变当前vecotr所分配空间的大小

13.erase 删除指针指向的数据项

14.clear 清空当前的vector

15.rbegin 将vector反转后的开始指针返回(其实就是原来的end-1)

16.rend 将vector反转构的结束指针返回(其实就是原来的begin-1)

17.empty 判断vector是否为空

18.swap 与另一个vector交换数据
*/
using namespace std;
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& findNums, vector<int>& nums) {
        //返回结果的vector
        vector<int> res;
        stack<int> s;
        //key是nums中的每一个元素，value是该元素对应的下一个最大值，如果没有则对应的是1
        map<int,int> m;
        //开始反向入栈
        for(int i = nums.size() - 1;i >= 0 ; i--){
            //当栈非空，且栈顶元素小于当前要入栈的元素时，就把栈顶元素弹出
            //其实就是要保证当前栈顶元素为你要找的那个下一个最大元素
            while(!s.empty() && s.top() < nums[i]){
                s.pop();
            }
            //找到以后，我们就把他存入map中
            m[nums[i]] = s.empty() ? -1 : s.top();
            //将新元素入栈
            s.push(nums[i]);
        }
        //我们得到的map是nums中每一个元素以及它对应的下一个最大元素构成的映射
        //将映射组装成vector
        for(int i = 0; i<findNums.size();i++){
            res.push_back(m[findNums[i]]);
        }
        return res;
    }
};
int main()
{
    int n1[3] = {4,1,2};
    int n2[4] = {1,3,4,2};
    vector<int> nums1(n1 , n1+3);
    vector<int> nums2(n2 , n2+4);
    Solution a;
    vector<int> result = a.nextGreaterElement(nums1 , nums2);
    for(int i = 0 ; i< result.size();i++){
        cout << result[i] << endl;
    }
    return 0;
}
~~~

执行用时: 8 ms, 在Next Greater Element I的C++提交中击败了99.07% 的用户！！