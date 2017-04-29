---
title: LeetCode/LintCode刷题之线性表
date: 2017-3-17 16:50:03
comments: true
mathjax: true
categories:
	- 刷题
	- 面试
tags: 
	- C++
	- Python
---


#线性表

## Longest Consecutive Sequence
- leetcode: [Longest Consecutive Sequence | LeetCode OJ](https://leetcode.com/problems/longest-consecutive-sequence/)
- lintcode: [(124) Longest Consecutive Sequence](http://www.lintcode.com/en/problem/longest-consecutive-sequence/)


### Problem Statement

Given an unsorted array of integers, find the length of the longest
consecutive elements sequence.

#### Example

Given `[100, 4, 200, 1, 3, 2]`,
The longest consecutive elements sequence is `[1, 2, 3, 4]`. Return its
length: `4`.

#### Clarification

Your algorithm should run in O(_n_) complexity.


### 题解

首先看题要求，时间复杂度为 $O(n)$, 如果排序，基于比较的实现为 $O(n \log n)$, 基数排序需要数据有特征。故排序无法达到复杂度要求。接下来可以联想空间换时间的做法，其中以哈希表为代表。这个题要求返回最长连续序列，不要求有序，非常符合哈希表的用法。**由于给定一个数其连续的数要么比它小1，要么大1，那么我们只需往左往右搜索知道在数组中找不到数为止。**结合哈希表查找为 $O(1)$ 的特性即可满足要求。用一个哈希表 ```unordered_map<int, bool> used``` 记录每个元素是否使用，对每个元素，以该元素为中心，往左右扩张，直到不连续为止，记录下贵长的长度。

详细说明：你看到[100, 4, 200, 1, 3, 2]这个数组，首先你会看99或者101在不在这个数组里，发现数组没这两个数，那么100组成的连续序列长度仅为1。接着会看5或者3在不在数组里，会发现3存在，5不存在；紧接着会看2在不在....直到发现0不在。从而得到4组成的最长序列为4。
总结一下会发现，我们在判断某个数的连续序列时，会分别往减小和增大的方向找下一个连续数在不在数组中。然后把两个方向的长度加起来即为包含该数的一个连续序列。需要注意的是，当前数的长度计数只需要出现在一个方向的查找中计算即可，否则就重复了。要找一个数是不是在数组中，不可能用遍历的方法实现，这样时间复杂度就超过$O(n)$了。而要降低时间复杂度，一个经典的方案就是空间换时间。用增加空间复杂度的方法来换取时间复杂度的降低。所以我们可以先对数组进行一次预处理，生成一份包含数组元素的哈希表。这样在求解某个数字在不在数组时就可以得到O(1)的时间复杂度。


### C++
```C++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_map<int, bool> used;
        for (auto i : nums) used[i] = false; //遍历容器bector里的每一个元素,设为false
        //for (auto i=1;i<5;i++) cout<<used[i];
        int longest = 0;
        for (auto i : nums) {
            if (used[i]) continue; //防止array中有重复integar，continue跳出本次循环
            int length = 1;
            used[i] = true;
            for (int j = i + 1; used.find(j) != used.end(); ++j) {
            //用find函数来定位数据出现位置，它返回的一个迭代器
            //如果map中没有要查找的数据，它返回的迭代器等于end函数返回的迭代器
                used[j] = true;
                ++length;
                }
            for (int j = i - 1; used.find(j) != used.end(); --j) {
                used[j] = true;
                ++length;
            }
            longest = max(longest, length);
        }
        return longest;
    }
};
```
### 复杂度分析
时间复杂度和空间复杂度均为 $O(n)$.

参考：map用法：http://blog.sina.com.cn/s/blog_59b6af690100xy0z.html

## Two Sum

Tags: Array, Hash Table, Easy

## Question

- leetcode: [Two Sum](https://leetcode.com/problems/two-sum/)
- lintcode: [Two Sum](http://www.lintcode.com/en/problem/two-sum/)

### Problem Statement

Given an array of integers, return **indices** of the two numbers such that
they add up to a specific target.

You may assume that each input would have **_exactly_** one solution, and you
may not use the _same_ element twice.

**Example:**  
    
    Given nums = [2, 7, 11, 15], target = 9,
    
    Because nums[**0**] + nums[**1**] = 2 + 7 = 9,
    return [**0**, **1**].



### 题解 - 哈希表

找两数之和是否为`target`, 如果是找数组中一个值为`target`该多好啊！遍历一次就知道了, 难道要将数组中所有元素的两两组合都求出来与`target`比较吗？时间复杂度显然为 $O(n^2)$, 显然不符题目要求。找一个数时直接遍历即可，那么可不可以将两个数之和转换为找一个数呢？我们先来看看两数之和为`target`所对应的判断条件—— $x_i + x_j = target$, 可进一步转化为 $x_i = target - x_j$, 其中 $i$ 和 $j$ 为数组中的下标。一段神奇的数学推理就**将找两数之和转化为了找一个数是否在数组中了**！
基本思路有了，现在就来看看怎么实现，显然我们需要额外的空间(也就是哈希表)来保存已经处理过的 $x_j$(**注意这里并不能先初始化哈希表，否则无法排除两个相同的元素相加为 target 的情况**), 如果不满足等式条件，那么我们就往后遍历，并把之前的元素加入到哈希表中，如果`target`减去当前索引后的值在哈希表中找到了，那么就将哈希表中相应的索引返回，大功告成！

**万能的哈希！！！！**

### C++
```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result;
        const int length = nums.size();
        if (!length) {
            return result;
        }
    // first value, second index
    unordered_map<int, int> hash(length);
    for (int i = 0; i != length; ++i) {
        if (hash.find(target - nums[i]) != hash.end()) {
            result.push_back(hash[target - nums[i]]);//index比i要小
            result.push_back(i);//题目要求index小的在前面
            return result;
        } else {
            hash[nums[i]] = i;//放入哈希表中 i=0先放入哈希表
        }
    }
    return result;
    }
};
```
### 复杂度分析
哈希表⽤了和数组等⻓的空间，空间复杂度为$O(n)$, 遍历⼀次数组，时间复杂度为$O(n)$.

参考：http://www.jiuzhang.com/solutions/2-sum/

## 3 Sum

## Question

- leetcode: [3Sum | LeetCode OJ](https://leetcode.com/problems/3sum/)
- lintcode: [(57) 3 Sum](http://www.lintcode.com/en/problem/3sum/)

### Problem Statement

Given an array _S_ of _n_ integers, are there elements _a_, _b_, _c_ in _S_
such that _a_ + _b_ + _c_ = 0? Find all unique triplets in the array which
gives the sum of zero.

#### Note:

The solution set must not contain duplicate triplets.
    
    For example, given array S = [-1, 0, 1, 2, -1, -4],
    
    A solution set is:
    [
      [-1, 0, 1],
      [-1, -1, 2]
    ]



### 题解1 - 排序 + 哈希表 + 2 Sum

相比之前的 2 Sum, 3 Sum 又多加了一个数，按照之前 2 Sum 的分解为『1 Sum + 1 Sum』的思路，我们同样可以将 3 Sum 分解为『1 Sum + 2 Sum』的问题，具体就是首先对原数组排序，排序后选出第一个元素，随后在剩下的元素中使用 2 Sum 的解法。
### C++
```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int> > result;
        if (nums.size() < 3) return result;
        int ans = 0;
        sort(nums.begin(), nums.end());
        const int length = nums.size();
        //for(i = 0; i < nums.size() - 2; ++i){ //最后2个没有可能性
        for(int i = 0; i < nums.size() - 2; ++i){
            if (i > 0 && nums[i] == nums[i - 1])
                continue; //重复数字跳出循环
            int target = 0 - nums[i];
            unordered_map<int, int> hash(length);// first value, second index
            for(int j = i + 1; j < nums.size(); ++j){
                if (hash.find(target - nums[j]) != hash.end()) {
                    result.push_back({nums[i], target - nums[j], nums[j]});
                    //result.push_front(nums[j]);//题目要求index小的在前面
                    //result.push_front(target - nums[j]);//index比j的index小
                    //return result;
                } else {
                    hash[nums[j]] = j;
                }
            }
  
        }
        result.erase(unique(result.begin(), result.end()), result.end());
        return result;
    }
};
//error:[0,0,0,0] Output:[[0,0,0],[0,0,0]] 解决：加入erase删除重复
```
或：先排序，然后左右夹逼，复杂度 $O(n^2)$。
这个方法可以推广到 k-sum，先排序，然后敬 k − 2 次悟环，在偎内层悟环左右夹逼，肘间复杂 度是 $O(max\{n log n, n^{k−1}\})$。


### C++
```c++

```


