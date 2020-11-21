---
title: LeetCode刷题记录
date: 2020-11-19 10:00:22
index_img: /resource/img/sfbj.jpg
category: 刷题
tags: 刷题
---


# 链表



# 树

# 动态规划

## 礼物的最大价值
     
### 题目描述

>在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？
      
       
### 示例
     
输入:
>\[
   &emsp;\[1,3,1],  
   &emsp;\[1,5,1],  
   &emsp;\[4,2,1]  
]

输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
     
     
     
     
     
### 解题思路

动态规划+dp数组，自底向上，状态转移方程 `f(i, j) = max{f(i - 1, j), f(i, j - 1)} + grid[i][j]`

```java
class Solution {
    public int maxValue(int[][] grid) {
        int m = grid.length, n = grid[0].length;//获取数组长度
        int[] dp = new int[n + 1];//dp数组 最长长度为n+1 用于存放
        for (int i = 1; i <= m; i++) {//两层循环
            for (int j = 1; j <= n; j++) {
                dp[j] = Math.max(dp[j], dp[j - 1]) + grid[i - 1][j - 1];//结合状态转移方程
            } 
        }
        return dp[n];
    }
}

```

## 丑数

### 题目描述

>我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

### 示例

>输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。

### 说明  

1 是丑数。
n 不超过1690。

### 解题思路

状态定义： 设动态规划列表 dp ，dp[i] 代表第 i + 1 个丑数。

转移方程：
当索引 a, b, c 满足以下条件时， dp[i] 为三种情况的最小值；
每轮计算 dp[i] 后，需要更新索引 a, b, c 的值，使其始终满足方程条件。

实现方法：分别独立判断 dp[i] 和 dp[a]×2 , dp[b]×3 , dp[c]×5 的大小关系，若相等则将对应索引 a , b , c 加 1 。

⎧
⎪ dp[a]×2>dp[i−1]≥dp[a−1]×2  
⎪   
⎨ dp[b]×3>dp[i−1]≥dp[b−1]×3  
⎪
⎪ dp[c]×5>dp[i−1]≥dp[c−1]×5  
​⎩	

`dp[i] = min(dp[a]×2,dp[b]×3,dp[c]×5)`

初始状态： dp[0] = 1 ，即第一个丑数为 1.  
返回值： dp[n-1] ，即返回第 n 个丑数.



### 代码实现

```java
class Solution {
    public int nthUglyNumber(int n) {
        int a = 0, b = 0, c = 0;
        int[] dp = new int[n];//dp数组
        dp[0] = 1;
        for(int i = 1; i < n; i++) {
            int n2 = dp[a] * 2, n3 = dp[b] * 3, n5 = dp[c] * 5;//状态转移方程
            dp[i] = Math.min(n2, n3, n5);
            if(dp[i] == n2) a++;
            if(dp[i] == n3) b++;
            if(dp[i] == n5) c++;
        }
        return dp[n - 1];
    }
}
```






## 数组中的逆序对

### 题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

 

### 示例

输入: [7,5,6,4]
输出: 5

### 解题思路

>











