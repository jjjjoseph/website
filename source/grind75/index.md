---
title: LeetCode Grind 75
date: 2025-06-01 16:43:49
layout: post
---

## Grind 75

> Grind 75 是 Leetcode 推出的刷題路線...

## Week 1 (13/13)

### Two Sum

[題目連結](https://leetcode.com/problems/two-sum/)  
**標籤**: Array, Hash Table  
**語言**: C++

### 題目描述

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

### 範例

Example 1:
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

Example 2:
Input: nums = [3,2,4], target = 6
Output: [1,2]

Example 3:
Input: nums = [3,3], target = 6
Output: [0,1]

### 限制

- 2 <= nums.length <= 10^4
- -10^9 <= nums[i] <= 10^9
- -10^9 <= target <= 10^9
- 只有一個有效答案

#### 思路

- 用 Hash Map 儲存已經看過的數字及其 index  
- 遍歷陣列，對於每個數字 `nums[i]`，檢查 `target - nums[i]` 是否已經在 map 裡  
- 如果有，回傳 `{ map[補數], i }`  
- 否則把 `nums[i]` 加進 map  

#### 程式碼

```markdown
class Solution {
public:
    vector<int> twoSum(const vector<int>& nums, const int target) {
        unordered_map<int,int> num_to_complement_idx;
        for (int i = 0; i < (int)nums.size(); i++) {
            auto it = num_to_complement_idx.find(nums[i]);
            if (it != num_to_complement_idx.end()) {
                return {it->second, i};
            }
            num_to_complement_idx[target - nums[i]] = i;
        }
        return {};
    }
};
```





