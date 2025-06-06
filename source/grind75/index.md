---
title: LeetCode Grind 75
date: 2025-06-01 16:43:49
layout: page
---

# Grind 75

> Grind 75 是 Leetcode 推出的刷題路線，適合給準備面試的同學。

## Week 1 (13/13)

### Two Sum

> [題目連結](https://leetcode.com/problems/two-sum/)  
> **標籤**: Array, Hash Table  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.  
>  
> You may assume that each input would have exactly one solution, and you may not use the same element twice.  
>  
> You can return the answer in any order.  

**範例**

> Example 1:  
> Input: nums = [2,7,11,15], target = 9  
> Output: [0,1]  
> Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].  

> Example 2:  
> Input: nums = [3,2,4], target = 6  
> Output: [1,2]  

> Example 3:  
> Input: nums = [3,3], target = 6  
> Output: [0,1]  

**限制**

> - 2 <= nums.length <= 10^4  
> - -10^9 <= nums[i] <= 10^9  
> - -10^9 <= target <= 10^9  
> - 只有一個有效答案  

**思路**

> - 用 Hash Map 儲存已經看過的數字及其 index  
> - 遍歷陣列，對於每個數字 `nums[i]`，檢查 `target - nums[i]` 是否已經在 map 裡  
> - 如果有，回傳 `{ map[補數], i }`  
> - 否則把 `nums[i]` 加進 map  

**程式碼**

```cpp
class Solution {
public:
    vector<int> twoSum(const vector<int>& nums, const int target) {
        unordered_map<int,int> num_to_complement_idx; // 儲存「補數 → 索引」
        for (int i = 0; i < (int)nums.size(); i++) {  // 遍歷 nums 陣列
            auto it = num_to_complement_idx.find(nums[i]); // 查找當前值是否為之前存的補數
            if (it != num_to_complement_idx.end()) {       // 若找到匹配
                return {it->second, i};                    // 回傳「先前元素的索引, 當前索引」
            }
            num_to_complement_idx[target - nums[i]] = i;    // 存入「target - nums[i]（補數） → i（索引）」
        }
        return {}; // 理論上不會執行到，因為題目保證至少有一組解
    }
};

```

**複雜度分析**

- 時間複雜度：O(n)
    - 我們只需一次迴圈遍歷整個陣列，對每個元素執行一次雜湊表（Hash Map）的查找與插入操作，而這些操作均攤銷為 O(1)。

- 空間複雜度：O(n)
    - 最壞情況下，雜湊表需要儲存所有 n 個元素的（補數→索引）對。

### Valid Parentheses

> [題目連結](https://leetcode.com/problems/valid-parentheses/)  
> **標籤**: Stack, String  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.  
>
> An input string is valid if:  
> 1. Open brackets must be closed by the same type of brackets.  
> 2. Open brackets must be closed in the correct order.  
> 3. Every close bracket has a corresponding open bracket of the same type.

**範例（Input & Output）**

> Input: s = "()"
> Output: true

> Input: s = "()[]{}"
> Output: true

> Input: s = "(]"
> Output: false

> Input: s = "{[]}"
> Output: true

**限制**

> - `1 <= s.length <= 10^4`  
> - `s` consists only of the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, `']'`.  

**思路**

> - 使用一個 `stack<char>` 來追蹤尚未配對的開括號。  
> - 遍歷字串中每個字元 `c`：  
>   1. 若 `c` 是開括號（`'('`、`'{'`、`'['`），則推入堆疊。  
>   2. 否則若 `c` 是閉括號，必須檢查堆疊頂端是否有對應的開括號：  
>      - 如果堆疊為空，或頂端字元不是對應類型的開括號，即可立即回傳 `false`。  
>      - 否則彈出堆疊頂端，繼續遍歷。  
> - 最後遍歷完字串，如果堆疊為空表示所有括號都正確配對，回傳 `true`；否則回傳 `false`。  

**程式碼**
```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;                    // 用來存放開括號
        for (char c : s) {                 // 遍歷字串中的每個字元
            if (c == '(' || c == '{' || c == '[') {
                st.push(c);                // 若是開括號，推入堆疊
            } else {
                if (st.empty()) return false; // 若遇到閉括號但堆疊空，無法配對 -> false
                char top = st.top();      // 取出堆疊頂端開括號
                if ((c == ')' && top != '(') || // 檢查是否為對應的左括號
                    (c == '}' && top != '{') ||
                    (c == ']' && top != '[')) {
                    return false;         // 若不匹配，回傳 false
                }
                st.pop();                 // 匹配成功，彈出堆疊頂端
            }
        }
        return st.empty();                // 遍歷結束後若堆疊為空，代表所有括號都配對成功
    }
};

```

**複雜度分析**

- 時間複雜度：O(n)  
  - 一次遍歷長度為 n 的字串，對每個字元做常數次的 `stack` 操作（`push`、`pop`、`empty`、`top`），皆為 O(1)。  

- 空間複雜度：O(n)  
  - 最壞情況下所有字元都是開括號，`stack` 大小會達到 n，故額外空間為 O(n)。  

### Merge Two Sorted Lists

> [題目連結](https://leetcode.com/problems/merge-two-sorted-lists/)  
> **標籤**: Linked List, Recursion  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> You are given the heads of two sorted linked lists `l1` and `l2`.  
> Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.  
> Return the head of the merged linked list.  

**範例（Input & Output）**

> Input: l1 = [1, 2, 4], l2 = [1, 3, 4]
> Output: [1, 1, 2, 3, 4, 4]

> Input: l1 = [], l2 = []
> Output: []

> Input: l1 = [], l2 = [0]
> Output: [0]

**限制**

> - The number of nodes in both lists is in the range `[0, 50]`.  
> - `-100 <= Node.val <= 100`  
> - Both `l1` and `l2` are sorted in non-decreasing order.  

**思路**

> - 使用一個虛擬頭節點（dummy head）來簡化合併過程的邊界處理。  
> - 令 `curr` 指向這個虛擬頭節點，當作合併後鏈結串列的尾巴指標。  
> - 同時維護指標 `p1` 指向 `l1` 的當前節點，`p2` 指向 `l2` 的當前節點。  
> - 迴圈比較 `p1->val` 和 `p2->val`：  
>   1. 如果 `p1->val <= p2->val`，將 `p1` 節點接到 `curr->next`，並讓 `p1` 移到下一個節點；  
>   2. 否則，將 `p2` 節點接到 `curr->next`，並讓 `p2` 移到下一個節點；  
>   3. 將 `curr` 一併移到剛接上的節點，保證尾巴始終指向最新節點。  
> - 當 `l1` 或 `l2` 其中一方先到達末端（`nullptr`）時，直接把另一個尚未遍歷完的鏈結串列「接」到 `curr->next`。  
> - 最後回傳 `dummy->next` 作為合併後的頭節點。  

**程式碼**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
 
#include <iostream>
using namespace std;

class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        // 建立虛擬頭節點，方便處理合併時的邊界
        ListNode* dummy = new ListNode(-1);
        ListNode* curr = dummy;

        // 指標 p1 指向 l1，p2 指向 l2
        ListNode* p1 = l1;
        ListNode* p2 = l2;

        // 只要兩個串列都還沒走到尾端，就繼續比較並合併
        while (p1 != nullptr && p2 != nullptr) {
            if (p1->val <= p2->val) {
                // 若 l1 節點較小或相等，就把 p1 節點接到 curr
                curr->next = p1;
                p1 = p1->next;  // p1 移到下一個
            } else {
                // 否則把 p2 節點接到 curr
                curr->next = p2;
                p2 = p2->next;  // p2 移到下一個
            }
            curr = curr->next;  // curr 移到最新接上的節點
        }

        // 當其中一個串列提前結束，直接把另一串列剩餘部分接上
        if (p1 != nullptr) {
            curr->next = p1;
        } else {
            curr->next = p2;
        }

        // 合併後的頭節點是 dummy->next
        ListNode* head = dummy->next;
        delete dummy;  // 釋放虛擬頭節點
        return head;
    }
};

```

**複雜度分析**

 - 時間複雜度：O(n + m)
     - 其中 n 為 l1 節點數，m 為 l2 節點數；合併過程中每個節點只會被訪問一次。

 - 空間複雜度：O(1)
     - 除了輸入的兩個鏈結串列節點外，只使用了常數個輔助指標（dummy、curr、p1、p2），不額外配置與輸入規模相關的空間。  

### Best Time to Buy and Sell Stock

> [題目連結](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)  
> **標籤**: Array, Dynamic Programming  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> You are given an array `prices` where `prices[i]` is the price of a given stock on the i-th day.  
> You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.  
> Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.  

**範例**

> Example 1:  
> Input: prices = [7,1,5,3,6,4]  
> Output: 5  
> Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.  
> Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.  

> Example 2:  
> Input: prices = [7,6,4,3,1]  
> Output: 0  
> Explanation: In this case, no transaction is done and the max profit = 0.  

**限制**

> - `1 <= prices.length <= 10^5`  
> - `0 <= prices[i] <= 10^4`  

**思路**

> - 只需一次遍歷陣列，記錄到目前為止見過的最低價格 `minPrice`，以及最大收益 `maxProfit`。  
> - 對於第 `i` 天的價格 `prices[i]`：  
>   1. 如果 `prices[i] < minPrice`，則更新 `minPrice = prices[i]`。  
>   2. 否則，計算當前收益 `prices[i] - minPrice`，若大於 `maxProfit`，則更新 `maxProfit`。  
> - 最終 `maxProfit` 即為答案，如果始終沒有正收益，則回傳 `0`。  

**程式碼**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int maxProfit(const vector<int>& prices) {
        int minPrice = INT_MAX;    // 初始化為極大值
        int maxProfit = 0;         // 初始化最大收益為 0

        for (int price : prices) {
            // 更新見過的最低價格
            if (price < minPrice) {
                minPrice = price;
            } else {
                // 計算當前收益，並更新最大收益
                int profit = price - minPrice;
                if (profit > maxProfit) {
                    maxProfit = profit;
                }
            }
        }

        return maxProfit;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需要遍歷一次長度為 n 的陣列，對每個元素進行常數次操作。

 - 空間複雜度：O(1)
     - 只使用固定數量的輔助變數，與輸入規模無關。

### Valid Palindrome

> [題目連結](https://leetcode.com/problems/valid-palindrome/)  
> **標籤**: Two Pointers, String  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.  
>  
> Return `true` if it is a palindrome, otherwise return `false`.  

**範例**

> Example 1:  
> Input: s = `"A man, a plan, a canal: Panama"`  
> Output: `true`  
> Explanation: `"amanaplanacanalpanama"` is a palindrome.  

> Example 2:  
> Input: s = `"race a car"`  
> Output: `false`  
> Explanation: `"raceacar"` is not a palindrome.  

> Example 3:  
> Input: s = `" "`  
> Output: `true`  
> Explanation: 空字串視為回文。  

**限制**

> - `1 <= s.length <= 2 * 10^5`  
> - `s` 只包含英文字母、數字、空格，以及標點符號等可列印字符。  

**思路**

> - 使用左右兩個指標 `left`、`right` 分別指向字串開頭與結尾。  
> - 迴圈中：  
>   1. 若 `s[left]` 不是字母或數字，就將 `left` 向右移動直到遇到合法字符或 `left >= right`。  
>   2. 若 `s[right]` 不是字母或數字，就將 `right` 向左移動直到遇到合法字符或 `left >= right`。  
>   3. 當左右指標都指向合法字符時，比較將 `s[left]`、`s[right]` 都轉為小寫之後是否相等；  
>      - 若不相等，直接回傳 `false`。  
>      - 若相等，則 `left++`、`right--`，繼續下一輪比較。  
> - 若整個過程中都未發現不相等的情況，最終 `left >= right` 時，回傳 `true`。  

**程式碼**

```cpp
#include <string>
using namespace std;

class Solution {
public:
    bool isPalindrome(const string& s) {
        int left = 0;
        int right = (int)s.size() - 1;

        while (left < right) {
            // 左指標跳過非字母或數字字符
            while (left < right && !isalnum(s[left])) {
                left++;
            }
            // 右指標跳過非字母或數字字符
            while (left < right && !isalnum(s[right])) {
                right--;
            }
            // 比較忽略大小寫後的字符
            if (left < right) {
                char c1 = tolower(s[left]);
                char c2 = tolower(s[right]);
                if (c1 != c2) {
                    return false;  // 發現不匹配，直接回傳 false
                }
                left++;
                right--;
            }
        }
        return true;  // 走到這裡表示所有字符都匹配，回傳 true
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需一次從頭到尾的掃描，左右指標共移動至多 n 次，對每個字符做常數次 isalnum 和 tolower 操作。

 - 空間複雜度：O(1)
     - 僅使用常數個指標與臨時變數，不依賴額外與輸入規模相關的空間。

### Invert Binary Tree

> [題目連結](https://leetcode.com/problems/invert-binary-tree/)  
> **標籤**: Tree, Depth-First Search, Breadth-First Search  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> Given the root of a binary tree, invert the tree, and return its root.  


**範例**

> Example 1:  
> Input: `root = [4,2,7,1,3,6,9]`  
> Output: `[4,7,2,9,6,3,1]`  
> Explanation:  
```text
Input:  
    4
   / \
  2   7
 / \ / \
1  3 6  9

Output:  
    4
   / \
  7   2
 / \ / \
9  6 3  1
```

> Example 2:  
> Input: `root = [2,1,3]`  
> Output: `[2,3,1]`  

> Example 3:  
> Input: `root = []`  
> Output: `[]`  

**限制**

> - The number of nodes in the tree is in the range `[0, 100]`.  
> - `-100 <= Node.val <= 100`  

**思路**

> - 本題可利用遞迴（DFS）或層序遍歷（BFS）來交換每個節點的左右子樹。  
> - 遞迴（DFS）做法：  
>   1. 若當前節點為 `nullptr`，直接回傳 `nullptr`。  
>   2. 交換當前節點的左右子節點指標。  
>   3. 對左右子節點分別遞迴執行相同操作。  
>   4. 最後回傳當前節點。  
> - 或者使用迴圈搭配佇列（BFS）：  
>   1. 將根節點放入佇列。  
>   2. 每次從佇列取出一個節點，交換其左右子節點，並將非空的左右子節點推入佇列。  
>   3. 直到佇列為空，最後回傳根節點。  
> - 遞迴版本較為簡潔，空間複雜度取決於遞迴深度；BFS 版本需要額外佇列空間。  

**程式碼**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

#include <queue>
using namespace std;

class Solution {
public:
    // 遞迴（DFS）解法
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) {
            return nullptr;
        }
        // 先交換左右子節點
        TreeNode* temp = root->left;
        root->left = root->right;
        root->right = temp;
        // 分別對交換後的左右子樹遞迴調用
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }

    // 若要使用 BFS（迴圈）解法，可替換為以下函式：
    /*
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) {
            return nullptr;
        }
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            // 交換當前節點的左右子節點
            TreeNode* temp = node->left;
            node->left = node->right;
            node->right = temp;
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        return root;
    }
    */
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 每個節點訪問一次並交換左右子節點，總計 n 個節點，因此時間複雜度為 O(n)。
 - 空間複雜度（遞迴版本）：O(h)
     - h 為二元樹高度，最壞情況（樹為一條鍊狀）時 h = n，空間複雜度為 O(n)。平均情況下若為平衡樹，h = O(log n)。
 - 空間複雜度（BFS 版本）：O(n)
     - 需使用佇列儲存節點，最壞情況時可能將 O(n/2) 個節點同時放入佇列，空間複雜度為 O(n)。

### Valid Anagram

> [題目連結](https://leetcode.com/problems/valid-anagram/)  
> **標籤**: Hash Table, String, Sorting  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> Given two strings `s` and `t`, return `true` _if_ `t` _is an anagram of_ `s`, and `false` _otherwise_.  
>  
> An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.  

**範例**

> Example 1:  
> Input: s = `"anagram"`, t = `"nagaram"`  
> Output: `true`  

> Example 2:  
> Input: s = `"rat"`, t = `"car"`  
> Output: `false`  

**限制**

> - `1 <= s.length, t.length <= 5 * 10^4`  
> - `s` 和 `t` 只包含小寫英文字母。  

**思路**

> - 方法一（排序 Sort）：  
>   1. 如果 `s.length() != t.length()`，直接回傳 `false`。  
>   2. 將 `s`、`t` 轉為字母陣列並排序。  
>   3. 如果排序後的兩個字串相等，回傳 `true`；否則回傳 `false`。  
>   - 時間複雜度主要在排序：O(n log n)，其中 n 為字串長度。  
>   - 空間複雜度取決於排序所需緩衝（取決於標準庫實現），額外可視為 O(1)。  
>  
> - 方法二（雜湊表 Hash Table / 計數）：  
>   1. 如果 `s.length() != t.length()`，直接回傳 `false`。  
>   2. 建立長度為 26 的整數陣列 `count[26]`，初始化為 0。  
>   3. 迴圈遍歷 `s`，對每個字符 `c = s[i]` 做 `count[c - 'a']++`。  
>   4. 迴圈遍歷 `t`，對每個字符 `c = t[i]` 做 `count[c - 'a']--`，若減到小於 0，代表字符出現次數不匹配，可直接回傳 `false`。  
>   5. 最後如果所有 `count[i] == 0`，回傳 `true`；否則 `false`。  
>   - 時間複雜度為 O(n)，只需兩次線性掃描（n 為字串長度）。  
>   - 空間複雜度為 O(1)，由於計數陣列大小固定為 26。  

**程式碼**

```cpp
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    bool isAnagram(const string& s, const string& t) {
        // 長度不同則不可能為 anagram
        if (s.size() != t.size()) {
            return false;
        }
        // 使用長度為 26 的計數陣列
        vector<int> count(26, 0);
        // 對 s 中字符計數 +1，對 t 中字符計數 -1
        for (int i = 0; i < s.size(); i++) {
            count[s[i] - 'a']++;
            count[t[i] - 'a']--;
        }
        // 檢查是否全部歸零
        for (int c : count) {
            if (c != 0) {
                return false;
            }
        }
        return true;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需對長度為 n 的字串各做一次線性掃描，計數與檢查操作均為 O(1)。

 - 空間複雜度：O(1)
     - 使用固定大小為 26 的整數陣列作為計數，與輸入長度無關。

### Binary Search

> [題目連結](https://leetcode.com/problems/binary-search/)  
> **標籤**: Array, Binary Search  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.  
>  
> You must write an algorithm with O(log n) runtime complexity.  

**範例**

> Example 1:  
> Input: nums = [-1,0,3,5,9,12], target = 9  
> Output: 4  
> Explanation: 9 exists in nums and its index is 4  

> Example 2:  
> Input: nums = [-1,0,3,5,9,12], target = 2  
> Output: -1  
> Explanation: 2 does not exist in nums so return -1  

**限制**

> - `1 <= nums.length <= 10^4`  
> - `-10^4 < nums[i], target < 10^4`  
> - All the integers in `nums` are **unique**.  
> - `nums` is sorted in ascending order.  
> - `target` is an integer.  

**思路**

> - 使用二分搜尋法（Binary Search）。  
> - 設定兩個指標 `left = 0` 與 `right = nums.size() - 1`，維護當前搜尋範圍。  
> - 迴圈條件為 `left <= right`：  
>   1. 計算 `mid = left + (right - left) / 2`（避免整數溢位）。  
>   2. 若 `nums[mid] == target`，則找到目標，回傳 `mid`。  
>   3. 若 `nums[mid] < target`，代表目標在右半段，將 `left = mid + 1`。  
>   4. 否則 `nums[mid] > target`，代表目標在左半段，將 `right = mid - 1`。  
> - 若迴圈結束都沒找到，回傳 `-1`。  

**程式碼**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int search(const vector<int>& nums, int target) {
        int left = 0;
        int right = (int)nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;  // 避免 (left+right) 溢位
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(log n)
     - 每次都將搜尋範圍對半，最多需要 log n 次比較。

 - 空間複雜度：O(1)
     - 只使用常數個輔助變數，不額外配置與輸入規模相關的空間。

### Flood Fill

> [題目連結](https://leetcode.com/problems/flood-fill/)  
> **標籤**: Depth-First Search, Breadth-First Search, Matrix  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> An image is represented by a 2D integer grid `image` where `image[i][j]` represents the pixel value of the image.  
> You are also given three integers `sr`, `sc`, and `newColor`.  
> You should perform a "flood fill" on the image starting from the pixel `image[sr][sc]`.  
> To perform a "flood fill", consider the starting pixel, plus any pixels connected 4-directionally to the starting pixel of the same color as the starting pixel, plus any pixels connected 4-directionally to those pixels (also with the same color), and so on.  
> Replace the color of all of the aforementioned pixels with `newColor`.  
> Return the modified image after performing the flood fill.  

**範例**

> Example 1:  
> Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, newColor = 2  
image = [[1,1,1],  
         [1,1,0],  
         [1,0,1]],  
  sr = 1, sc = 1, newColor = 2  
> Output: [[2,2,2],[2,2,0],[2,0,1]]  
 [[2,2,2],  
  [2,2,0],  
  [2,0,1]] 
> Explanation: 
From the center of the image (position (1,1)), all pixels connected by a path of the same color as the starting pixel are colored.  
初始的 image（3×3 矩陣）：
1  1  1
1  1  0
1  0  1
> 起始位置 (sr, sc) = (1, 1)，newColor = 2

>─── 填色過程 ───
> 從 image[1][1]（數值為 1）開始，將和它「上下左右相連且數字相同」的所有 1 全部改成 2。

> (1) 先把中心點 (1,1) 換色：  
    1  1  1        1  1  1
    1 [1] 0   →    1 [2] 0
    1  0  1        1  0  1

> (2) 看 (1,1) 的上下左右：  
    上方 (0,1) = 1 → 也要改成 2  
    下方 (2,1) = 0 → 不同，跳過  
    左方 (1,0) = 1 → 也要改成 2  
    右方 (1,2) = 0 → 跳過  

    **此時的狀態**：  
       1  [2]  1        1  [2]  1
      [2]  2   0   →   [2]  2   0
       1   0   1        1   0   1

> (3) 再繼續往外擴散：  
    - 處理 (0,1)（目前值 2）的相鄰：  
      (0,0)=1、(0,2)=1、(1,1)=2（已改）、(-1,1)不存在  
      其中 (0,0)=1、(0,2)=1 都屬於要改的範圍 → 改成 2  
    - 處理 (1,0)（目前值 2）的相鄰：  
      (0,0)=1（剛才改過了）、(2,0)=1 → 改成 2、(1,-1)不存在、(1,1)=2（已改）  
    - 不用再看 (1,2)、(2,1) 因為它們一開始就不是「1」。

    **此時的狀態**：  
      [2]  [2]  [2]        [2]  [2]  [2]
      [2]  [2]  0   →     [2]  [2]  0
      [2]   0   1         [2]   0   1

> (4) 接著 (2,0) 原本是 1，因為跟 (1,0) 相連，也要改成 2，(2,2)=1 則和任何「已改成 2」都不相鄰（中間隔了 0），所以不要改。  

    **最終結果**：  
      2   2   2
      2   2   0
      2   0   1

> Example 2:  
> Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, newColor = 0  
> Output: [[0,0,0],[0,0,0]]  
> Explanation: The new color is the same as the color of the starting pixel, so no changes are made.  

**限制**

> - `m == image.length`  
> - `n == image[i].length`  
> - `1 <= m, n <= 50`  
> - `0 <= image[i][j], newColor < 2^16`  
> - `0 <= sr < m`  
> - `0 <= sc < n`  

**思路**

> - 使用深度優先搜尋（DFS）或廣度優先搜尋（BFS）來遍歷與起始像素顏色相同且 4 方向相連的所有像素，並將它們的顏色換為 `newColor`。  
> - 首先記錄起始點的顏色 `originalColor`，如果 `originalColor == newColor`，則直接回傳 `image`（因為無需更改）。  
> - 定義方向陣列 `dirs = {{1,0},{-1,0},{0,1},{0,-1}}` 方便遍歷 4 個方向。  
> - 使用遞迴函式 `dfs(r, c)`：  
>   1. 如果當前坐標 `(r, c)` 越界或 `image[r][c] != originalColor`，則返回。  
>   2. 否則將 `image[r][c]` 設為 `newColor`。  
>   3. 依序對 4 個方向呼叫 `dfs(r + dr, c + dc)`。  
> - 主函式調用 `dfs(sr, sc)` 後，回傳更新後的 `image`。  

**程式碼**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        int m = image.size();
        int n = image[0].size();
        int originalColor = image[sr][sc];
        // 若原色與新色相同，直接回傳
        if (originalColor == newColor) {
            return image;
        }
        // 方向陣列：下、上、右、左
        vector<pair<int,int>> dirs = {{1,0}, {-1,0}, {0,1}, {0,-1}};
        // 開始 DFS
        dfs(image, sr, sc, originalColor, newColor, m, n, dirs);
        return image;
    }
    
private:
    void dfs(vector<vector<int>>& image, int r, int c, int originalColor, int newColor, int m, int n, const vector<pair<int,int>>& dirs) {
        // 邊界條件與顏色檢查
        if (r < 0 || r >= m || c < 0 || c >= n || image[r][c] != originalColor) {
            return;
        }
        // 替換顏色
        image[r][c] = newColor;
        // 遞迴探索四個方向
        for (auto& d : dirs) {
            int nr = r + d.first;
            int nc = c + d.second;
            dfs(image, nr, nc, originalColor, newColor, m, n, dirs);
        }
    }
};
```

**複雜度分析**

 - 時間複雜度：O(m * n)
     - 最壞情況下，所有像素都需要被訪問一次，因此需要遍歷 m*n 個格子。

 - 空間複雜度：O(m * n)（遞迴棧）
    - 最壞情況下，遞迴深度可能達到 m*n，若圖像所有像素同色且連通。

### Lowest Common Ancestor of a Binary Search Tree

> [題目連結](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)  
> **標籤**: Tree, Binary Search Tree  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定一個二元搜尋樹（BST）的根節點 `root`，以及樹中兩個節點指標 `p` 和 `q`，請找出這兩個節點的最近公共祖先（Lowest Common Ancestor, LCA）。  
>  
> 在 BST 中，對於任意節點 `node`，若 `p->val` 和 `q->val` 分別小於 `node->val`，代表兩者都在 `node` 的左子樹；若都大於 `node->val`，則在右子樹。若一個在左、一個在右，或者其中有一個等於 `node`，則 `node` 為最近公共祖先。  

**範例**

> Example 1:  
> Input:  
> ```
   6
   / \
  2   8
 / \ / \
0  4 7  9
  / \
 3   5
```

> `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8`
> Output: `6` 
> Explanation: 節點 2 和 8 的 LCA 是 6。  

> Example 2:  
> Input:  
> ```
   6
   / \
  2   8
 / \ / \
0  4 7  9
  / \
 3   5
``` 

> `root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4`  
> Output: `2`  
> Explanation: 節點 2 和 4 的 LCA 是 2（因為 2 是 4 的祖先）。  

> Example 3:  
> Input: `root = [2, 1], p = 2, q = 4`  
> Output: `2`  

**限制**

> - 樹中節點數量在範圍 `[2, 10^5]`。  
> - `-10^9 <= Node.val <= 10^9`，且樹中的所有 `Node.val` 皆互不相同。  
> - `p` 和 `q` 都是樹中存在的不同節點。  
> - BST 保證左子樹所有節點值小於根節點值，右子樹所有節點值大於根節點值。  

**思路**

> - 利用 BST 性質：對於當前節點 `cur`，比較 `p->val`、`q->val` 與 `cur->val`：  
>   1. 若 `p->val < cur->val` 且 `q->val < cur->val`，代表兩個節點都在左子樹，則將 `cur` 移至 `cur->left` 繼續搜尋。  
>   2. 若 `p->val > cur->val` 且 `q->val > cur->val`，代表兩個節點都在右子樹，則將 `cur` 移至 `cur->right` 繼續搜尋。  
>   3. 否則（即一個小於或等於 `cur->val`，另一個大於或等於 `cur->val`），當前 `cur` 即為最近公共祖先，可直接回傳。  
> - 由於每次都往該方向跨越一個子樹，最壞情況需遍歷至樹高。  

**程式碼**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* cur = root;
        while (cur) {
            // 若兩者都小於當前節點，往左子樹找
            if (p->val < cur->val && q->val < cur->val) {
                cur = cur->left;
            }
            // 若兩者都大於當前節點，往右子樹找
            else if (p->val > cur->val && q->val > cur->val) {
                cur = cur->right;
            }
            // 否則當前節點即為 LCA
            else {
                return cur;
            }
        }
        return nullptr; // 理論上不會到這裡，因為 p 和 q 一定存在於 BST 中
    }
};
```

**複雜度分析**

 - 時間複雜度：O(h)
     - h 為樹的高度。最壞情況（退化成鏈）時 h = n，時間為 O(n)；若為平衡 BST，h = O(log n)。

 - 空間複雜度：O(1)
     - 只使用常數個指標，不需額外空間儲存與遞迴棧。

### Balanced Binary Tree

> [題目連結](https://leetcode.com/problems/balanced-binary-tree/)  
> **標籤**: Tree, Depth-First Search  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> 給定一個二元樹的根節點 `root`，判斷該二元樹是否為「平衡二元樹」（Balanced Binary Tree）。  
>  
> 定義：「平衡二元樹」是指每個節點的左右子樹高度差的絕對值至多為 1，且它的左右子樹皆為平衡二元樹。  

**範例**

> Example 1:  
> Input: `root = [3,9,20,null,null,15,7]`  
> Output: `true`  
> Explanation:  
> ```
>    3
>    / \
>   9  20
>      / \
>     15  7
```

> Example 2:  
> Input: `root = [1,2,2,3,3,null,null,4,4]`  
> Output: `false`  
> Explanation:  
> ```
>        1
>       / \
>      2   2
>     / \
>    3   3
>   / \
>  4   4
```

> Example 3:  
> Input: `root = []`  
> Output: `true`  
> Explanation: 空樹被視為平衡二元樹。  

**限制**

> - 二元樹節點數量在範圍 `[0, 10^4]`。  
> - `-10^4 <= Node.val <= 10^4`。  

**思路**

> - 我們可以透過遞迴（DFS）在一次遍歷中，同時計算子樹高度並檢查平衡性。  
> - 定義一個輔助函式 `height(TreeNode* node)`，回傳該節點為根時的子樹高度；若該子樹不平衡，則回傳 `-1` 作為標記。  
>   1. 如果 `node` 為 `nullptr`，回傳高度 `0`。  
>   2. 遞迴計算左子樹高度 `leftH = height(node->left)`，若 `leftH == -1`，直接回傳 `-1`（代表左子樹已經不平衡）。  
>   3. 遞迴計算右子樹高度 `rightH = height(node->right)`，若 `rightH == -1`，直接回傳 `-1`（代表右子樹已經不平衡）。  
>   4. 如果 `abs(leftH - rightH) > 1`，代表當前節點不平衡，回傳 `-1`。  
>   5. 否則回傳 `max(leftH, rightH) + 1` 作為當前子樹高度。  
> - 最後在主函式 `isBalanced(TreeNode* root)` 中，只需檢查 `height(root) != -1` 即可判定是否為平衡二元樹。  

**程式碼**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    bool isBalanced(TreeNode* root) {
        return height(root) != -1;
    }

private:
    // 若子樹不平衡，回傳 -1；否則回傳子樹高度
    int height(TreeNode* node) {
        if (node == nullptr) {
            return 0;
        }
        // 計算左子樹高度
        int leftH = height(node->left);
        if (leftH == -1) {
            return -1;  // 左子樹不平衡
        }
        // 計算右子樹高度
        int rightH = height(node->right);
        if (rightH == -1) {
            return -1;  // 右子樹不平衡
        }
        // 如果左右高度差超過 1，當前節點不平衡
        if (abs(leftH - rightH) > 1) {
            return -1;
        }
        // 否則回傳當前節點子樹的高度
        return max(leftH, rightH) + 1;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 每個節點只會被訪問一次；透過遞迴自底向上計算高度並檢查平衡，總共 O(n) 次呼叫。

 - 空間複雜度：O(h)（遞迴棧）
     - h 為二元樹高度；最壞情況（樹為一條鏈）時 h = n，空間複雜度為 O(n)；若為平衡樹，h = O(log n)。

### Linked List Cycle

> [題目連結](https://leetcode.com/problems/linked-list-cycle/)  
> **標籤**: 鏈表 (Linked List)、雙指針 (Two Pointers)  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定一個單向鏈表的頭節點 `head`，判斷鏈表中是否存在環 (cycle)。如果存在，則返回 `true`；否則返回 `false`。

**範例**

> Example 1:  
> ```
(3) → (2) → (0) → (-4)
       ↑           │
       └───────────┘
```
> Input: head = [3,2,0,-4], pos = 1
> Output: true
> Explanation: 尾節點指向第二個節點，形成環。

> Example 2:  
> ```
(1) → (2)
 ↑     │
 └─────┘
```
> Input: head = [1,2], pos = 0
> Output: true
> Explanation: 尾節點指向第一個節點，形成環。


> Example 3:  
> Input: head = [1], pos = -1
> Output: false
> Explanation: 無環，pos = -1 表示沒有環。

**限制**

> - 鏈表節點數量範圍為 `[0, 10^4]`。  
> - `-10^5 <= Node.val <= 10^5`。  
> - `pos` 表示尾節點指向的位置索引，`pos = -1` 表示沒有環。  

**思路**

> - 使用「快慢指針」(tortoise and hare) 方法：  
>   1. 初始化慢指針 `slow = head`，快指針 `fast = head`。  
>   2. 每一步中，慢指針前進一個節點 (`slow = slow->next`)，快指針前進兩個節點 (`fast = fast->next->next`)。  
>   3. 如果在某一步驟中 `fast == nullptr` 或 `fast->next == nullptr`，說明鏈表到達末端，沒有環，返回 `false`。  
>   4. 如果 `slow == fast`，說明快指針已經追上慢指針，存在環，返回 `true`。  
>   5. 重複步驟直到返回結果。  

**程式碼**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(nullptr) {}
 * };
 */

#include <iostream>
using namespace std;

class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (head == nullptr || head->next == nullptr) {
            return false;  // 空鏈表或僅一個節點，不可能有環
        }
        ListNode *slow = head;          // 慢指針，每次走一步
        ListNode *fast = head->next;    // 快指針，每次走兩步

        while (slow != fast) {
            if (fast == nullptr || fast->next == nullptr) {
                return false;  // 快指針到達末端，無環
            }
            slow = slow->next;           // 慢指針向前一步
            fast = fast->next->next;     // 快指針向前兩步
        }
        return true;  // 慢指針與快指針相遇，有環
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 快慢指針最多各自遍歷 O(n) 次，在鏈表長度為 n 時，通過有限次迭代即可判斷是否有環。

 - 空間複雜度：O(1)
     - 只使用常數個指針，不需要額外記憶體，空間複雜度為常數。

### Implement Queue using Stacks

> [題目連結](https://leetcode.com/problems/implement-queue-using-stacks/)  
> **標籤**: Stack, Design  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `pop`, `peek`, and `empty`).  
>  
> Implement the `MyQueue` class:  
>  
> - `void push(int x)` Pushes element `x` to the back of the queue.  
> - `int pop()` Removes the element from the front of the queue and returns it.  
> - `int peek()` Returns the element at the front of the queue.  
> - `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.  
>  
> **Notes:**  
> - You must use only standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.  
> - Depending on your language, stack may not be supported natively. You may simulate a stack using a list or deque (as long as you use only standard stack operations).  

**範例**

> Example 1:  
> ```
> Input
> ["MyQueue", "push", "push", "peek", "pop", "empty"]
> [[], [1], [2], [], [], []]
> Output
> [null, null, null, 1, 1, false]
> Explanation
> MyQueue myQueue = new MyQueue();
> myQueue.push(1); // queue is: [1]
> myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
> myQueue.peek();  // return 1
> myQueue.pop();   // return 1, queue is [2]
> myQueue.empty(); // return false
> ```

**限制**

> - `1 <= x <= 9`  
> - 最多呼叫 `push`, `pop`, `peek`, `empty` 共 `100` 次  
> - 當呼叫 `pop` 或 `peek` 時，保證隊列不為空  

**思路**

> - 使用兩個堆疊：一個作為「輸入棧（inStack）」用來接收 `push` 的元素；另一個作為「輸出棧（outStack）」用來做 `pop` 或 `peek` 操作。  
> - 當執行 `push(x)` 時，直接將 `x` 推入 `inStack`。  
> - 當執行 `pop()` 或 `peek()` 時，若 `outStack` 為空，則將 `inStack` 內所有元素依次彈出並推入 `outStack`（這樣可以將最早推入的元素放到 `outStack` 頂部）。之後，`pop()` 就從 `outStack.top()` 取出並 `pop`；`peek()` 則回傳 `outStack.top()`。  
> - `empty()` 只需檢查 `inStack` 和 `outStack` 是否都為空。  
> - 這樣可以保證隊列操作的正確性：每當需要出隊時，如果 `outStack` 有元素就直接對應隊首，否則就把所有「輸入棧」的元素移到「輸出棧」，完成先進先出行為。  
> - 每個元素最多從 `inStack` 移到 `outStack` 一次，因此攤銷時間複雜度仍為 O(1)。  

**程式碼**

```cpp
#include <stack>
using namespace std;

class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        inStack.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        moveInToOut();
        int frontVal = outStack.top();
        outStack.pop();
        return frontVal;
    }
    
    /** Get the front element. */
    int peek() {
        moveInToOut();
        return outStack.top();
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return inStack.empty() && outStack.empty();
    }

private:
    stack<int> inStack;   // 用來接收 push 的元素
    stack<int> outStack;  // 用來執行 pop 和 peek

    // 將 inStack 中的所有元素移動到 outStack，僅在 outStack 為空時呼叫
    void moveInToOut() {
        if (outStack.empty()) {
            while (!inStack.empty()) {
                outStack.push(inStack.top());
                inStack.pop();
            }
        }
    }
};
```

**複雜度分析**

 - 時間複雜度：O(1)（攤銷）
     - 每個元素最多被移動一次：從 inStack 推到 outStack；其餘操作（push、pop、peek、empty）均為常數時間。

 - 空間複雜度：O(n)
     - 需要額外使用兩個堆疊來儲存最多 n 個元素。

## Week 2 (12/12)

### First Bad Version

> [題目連結](https://leetcode.com/problems/first-bad-version/)  
> **標籤**: Binary Search  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.  
>  
> Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.  
>  
> You are given an API `bool isBadVersion(version)` which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**範例**

> Example 1:  
> Input: `n = 5`, `bad = 4`  
> Output: `4`  
> Explanation:  
> call `isBadVersion(3)` → false  
> call `isBadVersion(5)` → true  
> call `isBadVersion(4)` → true  
> So, 4 is the first bad version.  

> Example 2:  
> Input: `n = 1`, `bad = 1`  
> Output: `1`  
> Explanation: Only one version and it is bad.

**限制**

> - `1 <= bad <= n <= 2^31 - 1`  
> - 呼叫 API `isBadVersion` 次數應盡量少。

**思路**

> - 使用二分搜尋法（Binary Search）定位第一個壞版本：  
>   1. 設定 `left = 1`, `right = n`。  
>   2. 在迴圈中計算 `mid = left + (right - left) / 2`（防止溢位）。  
>   3. 呼叫 `isBadVersion(mid)`：  
>      - 如果返回 `true`，代表第 `mid` 版是壞的，壞版本範圍縮至 `[left, mid]`，因此將 `right = mid`。  
>      - 如果返回 `false`，代表第 `mid` 版是好的，壞版本必在 `mid+1` 到 `right`，因此將 `left = mid + 1`。  
>   4. 直到 `left == right`，此時 `left` 或 `right` 即為第一個壞版本，返回 `left`。  
> - 二分搜尋可保證最少化函式調用次數，時間複雜度為 `O(log n)`。

**程式碼**

```cpp
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        long left = 1;
        long right = n;
        while (left < right) {
            long mid = left + (right - left) / 2;
            if (isBadVersion(mid)) {
                // mid 及之後皆為壞版本，向左收縮
                right = mid;
            } else {
                // mid 為好版本，壞版本在 mid+1 之後
                left = mid + 1;
            }
        }
        return (int)left;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(log n)
     - 每次二分搜尋都將範圍縮減一半，最壞情況下需 log n 次呼叫 isBadVersion。

 - 空間複雜度：O(1)
     - 只使用常數個輔助變數（left, right, mid），不依賴額外空間。

### Ransom Note

> [題目連結](https://leetcode.com/problems/ransom-note/)  
> **標籤**: Hash Table, String  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.  
>  
> Each letter in `magazine` can only be used once in `ransomNote`.  

**範例**

> Example 1:  
> Input: ransomNote = `"a"`, magazine = `"b"`  
> Output: `false`  

> Example 2:  
> Input: ransomNote = `"aa"`, magazine = `"ab"`  
> Output: `false`  

> Example 3:  
> Input: ransomNote = `"aa"`, magazine = `"aab"`  
> Output: `true`  

**限制**

> - `1 <= ransomNote.length, magazine.length <= 10^5`  
> - `ransomNote` 和 `magazine` 只包含小寫英文字母。  

**思路**

> - 方法一（計數陣列）：  
>   1. 如果 `ransomNote.length() > magazine.length()`，直接回傳 `false`，因為信件請求長度大於雜誌字母總數，不可能構造。  
>   2. 建立長度為 26 的整數陣列 `count[26]`，初始化為 0。  
>   3. 遍歷 `magazine`，對 `magazine[i]` 字符做 `count[magazine[i] - 'a']++`。  
>   4. 遍歷 `ransomNote`，對 `ransomNote[i]` 字符做 `count[ransomNote[i] - 'a']--`，若減到小於 0，說明雜誌中該字符不足，返回 `false`。  
>   5. 整個過程若未出現不足，即可回傳 `true`。  
>   - 時間複雜度 O(n + m)，n 為 `magazine` 長度，m 為 `ransomNote` 長度。  
>   - 空間複雜度 O(1)，計數陣列大小固定為 26。  

**程式碼**

```cpp
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    bool canConstruct(const string& ransomNote, const string& magazine) {
        if (ransomNote.size() > magazine.size()) {
            return false;
        }
        vector<int> count(26, 0);
        // 對 magazine 中字符計數 +1
        for (char c : magazine) {
            count[c - 'a']++;
        }
        // 對 ransomNote 中字符計數 -1，若不足則回傳 false
        for (char c : ransomNote) {
            if (--count[c - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n + m)
     - 需遍歷 magazine 長度 n 做一次計數，以及遍歷 ransomNote 長度 m 做一次扣減，總共為 O(n + m)。

 - 空間複雜度：O(1)
     - 使用固定大小為 26 的整數陣列，不隨輸入長度變化。

### Climbing Stairs

> [題目連結](https://leetcode.com/problems/climbing-stairs/)  
> **標籤**: Dynamic Programming, Math  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> You are climbing a staircase. It takes `n` steps to reach the top.  
>  
> Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?  

**範例**

> Example 1:  
> Input: n = 2  
> Output: 2  
> Explanation: There are two ways to climb to the top:  
> 1. 1 step + 1 step  
> 2. 2 steps  

> Example 2:  
> Input: n = 3  
> Output: 3  
> Explanation: There are three ways to climb to the top:  
> 1. 1 step + 1 step + 1 step  
> 2. 1 step + 2 steps  
> 3. 2 steps + 1 step  

**限制**

> - `1 <= n <= 45`  

**思路**

> - 本題屬於經典的動態規劃 (Dynamic Programming) 問題，也可用數學的方法（Fibonacci）解決。  
> - 設 `dp[i]` 為爬到第 `i` 級台階的所有可能方式數。  
> - 由於每次只能爬 1 或 2 級，因此到達第 `i` 級的方式可由到達第 `i-1` 級再跨 1 步，或到達第 `i-2` 級再跨 2 步：  
>   > `dp[i] = dp[i-1] + dp[i-2]`  
> - 初始條件：  
>   > `dp[1] = 1` （只有一種方式：跨 1 步）  
>   > `dp[2] = 2` （兩種方式：1+1 或 2）  
> - 依此遞推至 `dp[n]` 即為答案。  
> - 為節省空間，可只用兩個變數滾動更新，不需整個 `dp` 陣列。  

**程式碼**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2) {
            return n;
        }
        // prev2 代表 dp[i-2]，初始為 dp[1] = 1
        int prev2 = 1;
        // prev1 代表 dp[i-1]，初始為 dp[2] = 2
        int prev1 = 2;
        int curr = 0;
        for (int i = 3; i <= n; i++) {
            curr = prev1 + prev2; // dp[i] = dp[i-1] + dp[i-2]
            prev2 = prev1;        // 滾動更新：下一輪的 dp[i-2]
            prev1 = curr;         // 更新 dp[i-1]
        }
        return curr;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需要一次從 3 到 n 的迴圈，每次操作為常數時間。

 - 空間複雜度：O(1)
     - 只使用常數個變數，不需額外與 n 線性相關的空間。

### Longest Palindrome

> [題目連結](https://leetcode.com/problems/longest-palindrome/)  
> **標籤**: Hash Table, String, Greedy  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定一個由大小寫英文字母組成的字串 `s`，請你找出可以由這些字母「組成」的最長回文串的長度。你可以假設字母的大小寫有區分，例如 `'A'` 和 `'a'` 被視為不同字元。

**範例**

> Example 1:  
> Input: s = `"abccccdd"`  
> Output: `7`  
> Explanation: 可以構造的最長回文串為 `"dccaccd"`，長度為 7。  

> Example 2:  
> Input: s = `"a"`  
> Output: `1`  
> Explanation: 回文串就是 `"a"`，長度為 1。   

**限制**

> - `1 <= s.length <= 2000`  
> - `s` 只包含英文字母（大寫、小寫）。

**思路**

> - 要構造最長回文串，回文的特性是中間可以有一個單獨的字元，其餘字元必須成對出現（左右對稱）。  
> - 可以先統計每個字元出現次數，對於每個字元：  
>   1. 如果該字元出現次數為偶數，則可全部用於回文，長度直接加上該偶數。  
>   2. 如果該字元出現次數為奇數，則可用 `count - 1`（偶數部分）來組成對稱部分，並且保留一個候選作為中心字元（如果尚未使用中心字元）。  
> - 最終，只要有至少一個字元出現次數為奇數，就可以在中心放置一個字元讓回文長度加 1。  
> - 因此流程為：  
>   1. 建立 `unordered_map<char,int>` 或長度 128 的陣列來統計字元出現次數。  
>   2. 遍歷所有字元計算總可用偶數長度，並記錄是否存在奇數出現。  
>   3. 如果存在任一奇數，總長度再加 1；否則直接回傳偶數總和。  

**程式碼**

```cpp
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    int longestPalindrome(const string& s) {
        // 用大小為 128 的計數陣列統計所有 ASCII 字元
        vector<int> count(128, 0);
        for (char c : s) {
            count[c]++;
        }
        
        int length = 0;
        bool hasOdd = false; // 標記是否存在出現次數為奇數的字元
        
        for (int freq : count) {
            if (freq % 2 == 0) {
                // 偶數次數全部可以用
                length += freq;
            } else {
                // 奇數次數只能用 freq - 1，並標記可放置中心字元
                length += freq - 1;
                hasOdd = true;
            }
        }
        
        // 如果有任何一個字元出現次數為奇數，可以把其中一個放到回文中心
        if (hasOdd) {
            length += 1;
        }
        return length;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - n 為字串長度。需要一次掃描字串進行統計，然後再掃描長度固定（128）的計數陣列。

 - 空間複雜度：O(1)
     - 使用固定大小（128）的計數陣列，與輸入長度無關。

### Reverse Linked List

> [題目連結](https://leetcode.com/problems/reverse-linked-list/)  
> **標籤**: Linked List, Iteration, Recursion  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定一個單向鏈表的頭節點 `head`，將該鏈表反轉，並返回反轉後的頭節點。  

**範例**

> Example 1:  
> Input: `head = [1,2,3,4,5]`  
> Output: `[5,4,3,2,1]`  
```text
原始鏈結串列：
(1) → (2) → (3) → (4) → (5)

              ↓
              ↓

反轉後鏈結串列：
(5) → (4) → (3) → (2) → (1)
```

> Example 2:  
> Input: `head = [1,2]`  
> Output: `[2,1]`  
```text
原始：
(1) → (2)

   ↓

反轉後：
(2) → (1)
```

> Example 3:  
> Input: `head = []`  
> Output: `[]`  

**限制**

> - 鏈表節點數量範圍為 `[0, 5000]`。  
> - `-5000 <= Node.val <= 5000`。  
> - 必須在 O(n) 時間內完成，且使用 O(1) 額外空間（不含遞迴棧空間）。  

**思路**

> - **迭代法 (Iteration)**：  
>   1. 使用三個指標：`prev = nullptr`（指向已反轉部分的尾節點）、`curr = head`（當前要處理的節點）、`nextTemp`（暫存 `curr->next`）。  
>   2. 迴圈條件為 `curr != nullptr`：  
>      - 先將 `curr->next` 暫存到 `nextTemp`。  
>      - 將 `curr->next` 指向 `prev`，完成節點反向指向。  
>      - 更新 `prev = curr`（新的已反轉部分尾巴），並將 `curr = nextTemp`（繼續處理下一個節點）。  
>   3. 迴圈結束時，`prev` 即為反轉後的新頭節點，返回 `prev`。  
>   - 此方法只需一趟遍歷，並且使用常數級輔助變數。  
>  
> - **遞迴法 (Recursion)**：  
>   1. 如果 `head == nullptr` 或 `head->next == nullptr`，直接返回 `head`（空表或單節點就是反轉後本身）。  
>   2. 呼叫 `ListNode* p = reverseList(head->next)` 取得反轉後的尾部節點 `p`。  
>   3. 將 `head->next->next = head`，使下一節點指回 `head`，再將 `head->next = nullptr` 斷開。  
>   4. 返回 `p`（整個反轉後的頭節點）。  
>   - 遞迴法的呼叫深度為鏈表長度 h，因此額外空間為遞迴棧 O(h)。  

**程式碼**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(nullptr) {}
 * };
 */

#include <iostream>
using namespace std;

class Solution {
public:
    // 迭代法
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* nextTemp = curr->next; // 暫存下一節點
            curr->next = prev;               // 反轉指向
            prev = curr;                     // 移動 prev
            curr = nextTemp;                 // 處理下一節點
        }
        return prev; // prev 即為新的頭節點
    }

    // 遞迴法
    /*
    ListNode* reverseList(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        ListNode* p = reverseList(head->next); 
        head->next->next = head; // 下個節點指回 current
        head->next = nullptr;    // 斷開 current 節點的 next
        return p;                // p 是反轉後的新頭節點
    }
    */
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 單趟遍歷整個鏈表，共有 n 個節點，每個節點只處理一次。

 - 空間複雜度 (迭代法)：O(1)
     - 只使用常數個指標變數，不額外配置與 n 相關的空間。

 - 空間複雜度 (遞迴法)：O(n)（遞迴棧）
     - 遞迴深度等於鏈表長度 n，遞迴棧使用空間為 O(n)。

### Majority Element

> [題目連結](https://leetcode.com/problems/majority-element/)  
> **標籤**: Array, Divide and Conquer, Hash Table, Sorting  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定一個大小為 `n` 的陣列 `nums`，請找出其中的「主元素」（Majority Element），即在陣列中出現次數 **嚴格大於** ⌊n/2⌋ 的元素。  
> 你可以假設主元素一定存在於陣列中。  

**範例**

> Example 1:  
> Input: `nums = [3,2,3]`  
> Output: `3`  

> Example 2:  
> Input: `nums = [2,2,1,1,1,2,2]`  
> Output: `2`  

**限制**

> - `1 <= nums.length <= 5 * 10^4`  
> - `-10^9 <= nums[i] <= 10^9`  
> - 主元素一定存在。  

**思路**

> 本題可以使用 **Boyer–Moore 投票演算法（Boyer–Moore Voting Algorithm）**，時間複雜度 O(n)、空間複雜度 O(1)。  
> 
> 演算法流程：  
> 1. 初始化 `candidate`（候選元素）為任意值、`count = 0`。  
> 2. 遍歷陣列 `nums` 中的每個元素 `num`：  
>    - 當 `count == 0` 時，將 `candidate = num`。  
>    - 若 `num == candidate`，`count++`；否則 `count--`。  
> 3. 最終 `candidate` 即為主元素。  
> 
> 原理：因為主元素出現次數大於 ⌊n/2⌋，當我們遍歷時，凡是當前值與候選相同，就增加計數；若不同，就抵消一個候選的影響。由於主元素出現次數占大多數，最終剩下的候選必為主元素。  

**程式碼**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    int majorityElement(const vector<int>& nums) {
        int candidate = 0;
        int count = 0;
        for (int num : nums) {
            if (count == 0) {
                candidate = num;
                count = 1;
            } else if (num == candidate) {
                count++;
            } else {
                count--;
            }
        }
        return candidate;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需一次線性掃描陣列。

 - 空間複雜度：O(1)
     - 僅使用常數個輔助變數。

### Add Binary

> [題目連結](https://leetcode.com/problems/add-binary/)  
> **標籤**: String, Math  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定兩個二進位字串 `a` 和 `b`，請回傳它們相加後的二進位字串。  

**範例**

> Example 1:  
> Input: a = `"11"`, b = `"1"`  
> Output: `"100"`  

> Example 2:  
> Input: a = `"1010"`, b = `"1011"`  
> Output: `"10101"`  

**限制**

> - `1 <= a.length, b.length <= 10^4`  
> - `a` 和 `b` 只包含字符 `'0'` 或 `'1'`。  
> - 每個字串除非整串為 `"0"`，否則不含前導零。  

**思路**

> - 使用雙指標從字串末端開始遍歷，同時維護進位 `carry`。  
> - 設定 `i = a.length() - 1`, `j = b.length() - 1`。  
> - 當 `i >= 0 || j >= 0 || carry != 0`：  
>   1. 取 `digitA = (i >= 0) ? a[i] - '0' : 0`，`digitB = (j >= 0) ? b[j] - '0' : 0`。  
>   2. 計算總和 `sum = digitA + digitB + carry`。  
>   3. 當前位結果為 `sum % 2`，更新 `carry = sum / 2`。  
>   4. 將 `(sum % 2)` 推入答案字串（先收集後反轉）。  
>   5. `i--`, `j--`，重複上述步驟。  
> - 最後反轉收集到的結果並回傳。  

**程式碼**

```cpp
#include <string>
#include <algorithm>
using namespace std;

class Solution {
public:
    string addBinary(const string& a, const string& b) {
        int i = (int)a.size() - 1;
        int j = (int)b.size() - 1;
        int carry = 0;
        string result;
        while (i >= 0 || j >= 0 || carry != 0) {
            int digitA = (i >= 0) ? a[i] - '0' : 0;
            int digitB = (j >= 0) ? b[j] - '0' : 0;
            int sum = digitA + digitB + carry;
            result.push_back(char('0' + (sum % 2)));
            carry = sum / 2;
            i--;
            j--;
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(max(m, n))
     - 其中 m = a.length(), n = b.length()；需要遍歷最長字串長度次數。

 - 空間複雜度：O(max(m, n))
     - 用於存儲結果的字串長度最多為 max(m, n) + 1。

### Diameter of Binary Tree

> [題目連結](https://leetcode.com/problems/diameter-of-binary-tree/)  
> **標籤**: Tree, Depth-First Search, Recursion  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> 給定一個二元樹的根節點 `root`，請你計算並回傳該二元樹的「直徑」（diameter）。  
>  
> 二元樹的直徑定義為任意兩個節點之間的最長路徑邊數。這條路徑可以不經過根節點。  

**範例**

> Example 1:  
> Input: `root = [1,2,3,4,5]`  
> Output: `3`  
> Explanation: 最長路徑為節點 4 → 2 → 1 → 3（或 5 → 2 → 1 → 3），邊的數量為 3。  

> Example 2:  
> Input: `root = [1,2]`  
> Output: `1`  
> Explanation: 最長路徑為節點 2 → 1，邊的數量為 1。  

**限制**

> - 二元樹節點數量在範圍 `[1, 10^4]`。  
> - `-100 <= Node.val <= 100`。  
> - 結果保證在 32 位整數範圍內。  

**思路**

> - 我們需要計算任意兩個節點之間的最長路徑邊數。  
> - 對於某個節點 `node`，以該節點為「橋梁」的最長路徑長度可以表示為「左子樹最大深度」加上「右子樹最大深度」。  
> - 因此，只要在一次遞迴遍歷中，同時計算每個節點的子樹高度，並在計算過程中更新全域的 `diameter` 為 `max(diameter, leftHeight + rightHeight)`，即可得到最終答案。  
> - 定義輔助函式 `height(TreeNode* node)`：  
>   1. 若 `node == nullptr`，回傳高度 0。  
>   2. 透過遞迴，先取得 `leftH = height(node->left)`，再取得 `rightH = height(node->right)`。  
>   3. 更新全域變數 `diameter = max(diameter, leftH + rightH)`。  
>   4. 回傳 `max(leftH, rightH) + 1` 作為該節點的高度。  
> - 最後在主函式中呼叫 `height(root)`，並回傳紀錄到的 `diameter`。  

**程式碼**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

#include <algorithm>
using namespace std;

class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        diameter = 0;
        height(root);
        return diameter;
    }

private:
    int diameter;  // 紀錄當前最長直徑（邊數）

    // 返回以 node 為根的子樹高度，同時更新 diameter
    int height(TreeNode* node) {
        if (node == nullptr) {
            return 0;
        }
        int leftH = height(node->left);
        int rightH = height(node->right);
        // 以當前節點為橋梁，左右子樹高度之和就是通過此節點的最大邊數
        diameter = max(diameter, leftH + rightH);
        // 返回該子樹高度：較高的子樹高度 + 1
        return max(leftH, rightH) + 1;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 每個節點只會被訪問一次，計算左右子樹高度與更新 diameter 均為常數時間。

 - 空間複雜度：O(h)（遞迴棧空間）
     - h 為二元樹高度，最壞情況（樹退化為鏈）時 h = n，空間複雜度為 O(n)；若為平衡樹，h = O(log n)。

### Middle of the Linked List

> [題目連結](https://leetcode.com/problems/middle-of-the-linked-list/)  
> **標籤**: Linked List, Two Pointers  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定單向鏈表的頭節點 `head`，請返回鏈表的中間節點。如果鏈表有兩個中間節點，則返回第二個中間節點。  

**範例**

> Example 1:  
> Input: head = [1,2,3,4,5]
> Output: [3,4,5]
> Explanation: 中間節點為 3。返回節點 3 及其後所有節點。
> (1) → (2) → [3] → (4) → (5)

> Example 2:  
> Input: head = [1,2,3,4,5,6]
> Output: [4,5,6]
> Explanation: 鏈表有兩個中間節點（3 和 4），因此返回第二個中間節點 4 及其後所有節點。
> (1) → (2) → (3) → [4] → (5) → (6)

**限制**

> - 鏈表節點數量範圍為 `[1, 100]`。  
> - `1 <= Node.val <= 100`。  

**思路**

> - 使用「快慢指針」（Two Pointers）：  
>   1. 初始化兩個指針 `slow = head`（每次走一步）和 `fast = head`（每次走兩步）。  
>   2. 當 `fast` 不為 `nullptr` 且 `fast->next` 不為 `nullptr` 時，執行：  
>      - `slow = slow->next;`  
>      - `fast = fast->next->next;`  
>   3. 當迴圈結束時，`slow` 指向的即為中間節點（如果長度為偶數，則指向第二個中間節點）。  
>   4. 直接返回 `slow`。  
> - 此方法只需一次遍歷，時間複雜度 O(n)，且空間複雜度 O(1)。  

**程式碼**

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(nullptr) {}
 * };
 */

#include <iostream>
using namespace std;

class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        // 快慢指針都從 head 開始
        ListNode* slow = head;
        ListNode* fast = head;
        // fast 每次走兩步，slow 每次走一步
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }
        // 最後 slow 指向中間節點（若為偶數長度，為第二中間節點）
        return slow;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需一次遍歷鏈表，快指針走兩步、慢指針走一步，最多迴圈 ⌈n/2⌉ 次。

 - 空間複雜度：O(1)
     - 只使用常數個輔助指針，不使用額外空間。

### Maximum Depth of Binary Tree

> [題目連結](https://leetcode.com/problems/maximum-depth-of-binary-tree/)  
> **標籤**: Tree, Depth-First Search, Breadth-First Search  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> Given the root of a binary tree, return its maximum depth.  
>  
> A binary tree's `maximum depth` is the number of nodes along the longest path from the root node down to the farthest leaf node.  

**範例**

> Example 1:  
> Input: `root = [3,9,20,null,null,15,7]`  
> Output: `3`  
> Explanation: The maximum depth is 3 because the longest path is 3 → 20 → 7 (or 3 → 20 → 15).  
```text
   (3)
   /   \
 (9)   (20)
       /  \
    (15)  (7)
```

> Example 2:  
> Input: `root = [1,null,2]`  
> Output: `2`  

> Example 3:  
> Input: `root = []`  
> Output: `0`  

**限制**

> - The number of nodes in the tree is in the range `[0, 10^4]`.  
> - `-100 <= Node.val <= 100`。  

**思路**

> - 可以使用遞迴（Depth-First Search）或迭代（Breadth-First Search）來計算最大深度。  
> - **遞迴（DFS）做法**：  
>   1. 如果當前節點為 `nullptr`，回傳深度 `0`。  
>   2. 否則遞迴計算左子樹深度 `leftDepth = maxDepth(root->left)`，再計算右子樹深度 `rightDepth = maxDepth(root->right)`。  
>   3. 當前節點的最大深度即為 `max(leftDepth, rightDepth) + 1`，其中 `+1` 代表包含當前節點。  
> - **迭代（BFS）做法**：  
>   1. 如果根為 `nullptr`，回傳 `0`。  
>   2. 使用一個佇列 `queue<TreeNode*> q`，先將 `root` 推入佇列。  
>   3. 初始化 `depth = 0`。  
>   4. 當佇列不為空時：  
>      - 令 `levelSize = q.size()`（當前層節點數），並遍歷這一層所有節點：  
>        - 取出節點 `node = q.front()` 並 `q.pop()`，若 `node->left` 非空則 `q.push(node->left)`、若 `node->right` 非空則 `q.push(node->right)`。  
>      - 這一層處理完畢後，`depth++` 表示深度加 1。  
>   5. 最終 `depth` 即為最大深度。  
> - 在簡單易實現的情況下，遞迴寫法最為直觀。  

**程式碼**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

#include <algorithm>
using namespace std;

class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int leftDepth = maxDepth(root->left);
        int rightDepth = maxDepth(root->right);
        return max(leftDepth, rightDepth) + 1;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 需要訪問每個節點一次，n 為節點總數。

 - 空間複雜度：O(h)（遞迴棧空間）
     - h 為樹的高度；最壞情況（樹退化成鏈）時 h = n，空間複雜度為 O(n)；若樹較平衡，h = O(log n)。

### Contains Duplicate

> [題目連結](https://leetcode.com/problems/contains-duplicate/)  
> **標籤**: Array, Hash Table  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 10 分鐘  

**題目描述**

> 給定一個整數陣列 `nums`，判斷是否存在至少一個值在陣列中出現兩次。如果任意值出現至少兩次，回傳 `true`；如果所有元素皆只出現一次，回傳 `false`。  

**範例**

> Example 1:  
> Input: nums = [1,2,3,1]  
> Output: true  

> Example 2:  
> Input: nums = [1,2,3,4]  
> Output: false  

> Example 3:  
> Input: nums = [1,1,1,3,3,4,3,2,4,2]  
> Output: true  

**限制**

> - `1 <= nums.length <= 10^5`  
> - `-10^9 <= nums[i] <= 10^9`  

**思路**

> - 方法一（使用哈希集合）：  
>   1. 建立一個空的 `unordered_set<int> seen`。  
>   2. 遍歷陣列 `nums` 的每個元素 `x`：  
>      - 如果 `x` 已存在於 `seen` 中，表示該值重複，立即回傳 `true`。  
>      - 否則將 `x` 插入 `seen` 中。  
>   3. 如果遍歷結束都沒有發現重複元素，則回傳 `false`。  
> - 方法二（排序）：  
>   1. 將 `nums` 進行排序，時間複雜度為 O(n log n)。  
>   2. 遍歷排序後的陣列，檢查任意相鄰元素是否相等，如果相等，則回傳 `true`。  
>   3. 若遍歷完整沒有相等的相鄰元素，回傳 `false`。  
> - 由於限制 n 可達 10^5，方法一時間複雜度 O(n)、空間複雜度 O(n)；方法二時間為 O(n log n)、空間為 O(1)（排序視為額外空間由語言實現決定）。  

**程式碼**

```cpp
#include <vector>
#include <unordered_set>
using namespace std;

class Solution {
public:
    bool containsDuplicate(const vector<int>& nums) {
        unordered_set<int> seen;
        for (int x : nums) {
            if (seen.count(x)) {
                return true;
            }
            seen.insert(x);
        }
        return false;
    }
};
```
**複雜度分析**

 - 時間複雜度：O(n)
     - 遍歷陣列一次，對每個元素進行哈希集合查找和插入，平均均攤時間為 O(1)。
 - 空間複雜度：O(n)
     - 最壞情況下，所有元素都被插入集合，共需額外 O(n) 空間。

### Maximum Subarray

> [題目連結](https://leetcode.com/problems/maximum-subarray/)  
> **標籤**: Array, Dynamic Programming, Divide and Conquer  
> **語言**: C++  
> **難度**: Easy  
> **解題時間**: 15 分鐘  

**題目描述**

> 給定一個整數陣列 `nums`，請計算具有最大和的連續子陣列，並返回其最大和。  

**範例**

> Example 1:  
> Input: `nums = [-2,1,-3,4,-1,2,1,-5,4]`  
> Output: `6`  
> Explanation: 最大子陣列為 `[4,-1,2,1]`，其和為 6。  

> Example 2:  
> Input: `nums = [1]`  
> Output: `1`  

> Example 3:  
> Input: `nums = [5,4,-1,7,8]`  
> Output: `23`  

**限制**

> - `1 <= nums.length <= 10^5`  
> - `-10^4 <= nums[i] <= 10^4`  

**思路**

> - 常見解法是 **Kadane’s Algorithm**，時間複雜度 O(n)，只需一次線性掃描。  
> - 使用兩個變數：`currentSum` 表示以當前位置結尾且和最大的子陣列和，`maxSum` 表示迄今為止見過的最大子陣列和。  
> - 初始化：  
>   - `currentSum = nums[0]`  
>   - `maxSum = nums[0]`  
> - 從索引 1 開始，對每個元素 `num = nums[i]`：  
>   1. 如果在 `currentSum + num` 與 `num` 之間，`num` 本身比較大，表示重新從當前元素開始累積，因此 `currentSum = max(num, currentSum + num)`。  
>   2. 更新 `maxSum = max(maxSum, currentSum)`。  
> - 掃描結束後，`maxSum` 即為答案。  
> - 此方法的核心在於：若前面累積的和是負數，加上後續的元素只會讓總和更小，因此在遇到負累積時就重置為當前元素。  

**程式碼**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxSubArray(const vector<int>& nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            currentSum = max(nums[i], currentSum + nums[i]);
            maxSum = max(maxSum, currentSum);
        }
        return maxSum;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 只需一次遍歷長度為 n 的陣列，每次計算和更新均為 O(1)。

 - 空間複雜度：O(1)
     - 僅使用固定數量的輔助變數，不依賴輸入規模。

## Week 3 (8/8)

### Insert Interval

> [題目連結](https://leetcode.com/problems/insert-interval/)  
> **標籤**: Array, Sorting  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個由不重疊且依照起點排序的區間列表 `intervals`，以及一個新的區間 `newInterval`，請在列表中插入 `newInterval`，並確保最終結果中的區間仍然是不重疊且已排序。如果插入後有任何區間重疊，請將它們合併為一個區間。  
>  
> 每個區間由 `[start, end]` 表示，且 `start <= end`。

**範例**

> Example 1:  
> Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
> Output: [[1,5],[6,9]]
> Explanation: 插入 [2,5] 後，與 [1,3] 重疊，合併為 [1,5]。

> Example 2:  
> Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,9]
> Output: [[1,2],[3,10],[12,16]]
> Explanation:
> 插入 [4,9] 後，會與 [3,5],[6,7],[8,10] 三個區間重疊，合併為 [3,10]。  

**限制**

> - `0 <= intervals.length <= 10^4`  
> - `intervals[i].length == 2`  
> - `0 <= intervals[i][0] <= intervals[i][1] <= 10^5`  
> - `intervals` 依照 `intervals[i][0]`（起點）嚴格遞增排序，且彼此不重疊。  
> - `newInterval.length == 2`  
> - `0 <= newInterval[0] <= newInterval[1] <= 10^5`  

**思路**

> - 因為原本的 `intervals` 已經依起點排序，且彼此不重疊，我們只需將 `newInterval` 插入適當位置，並與相鄰的重疊區間做合併。整體流程可分為三個階段：  
>  
>   1. **將所有在 `newInterval` 之前且不會重疊的區間先加入結果**  
>      - 換言之，遍歷原列表，對於每個區間 `intervals[i]`，如果其結束時間 `intervals[i][1] < newInterval[0]`（比 `newInterval` 的起點還要早結束），則代表這些區間完全在 `newInterval` 之前且不重疊，直接推入結果 `result`。  
>  
>   2. **處理與 `newInterval` 可能重疊的區間，進行合併**  
>      - 當遇到某個區間 `intervals[i]` 的起點 `intervals[i][0] <= newInterval[1]` 時，代表它與 `newInterval` 有重疊，需更新：  
>        - `newInterval[0] = min(newInterval[0], intervals[i][0])`  
>        - `newInterval[1] = max(newInterval[1], intervals[i][1])`  
>      - 此時不立即將 `intervals[i]` 推入 `result`，而是持續向後合併所有重疊區間。只要 `intervals[i][0] <= newInterval[1]`，就持續更新 `newInterval`。  
>  
>   3. **將合併完成的 `newInterval` 推入結果**  
>      - 當遇到第一個不與 `newInterval` 重疊的區間時（或原列表已遍歷完），將合併後的 `newInterval` 推入 `result`，並切換到下一個階段。  
>  
>   4. **將所有在 `newInterval` 之後且不會重疊的區間加入結果**  
>      - 從目前位置繼續遍歷，每個區間的起點都比 `newInterval[1]` 大，直接推入 `result` 即可。  
>  
> 以上步驟保證最終的 `result` 依然是按照起點排序，且所有重疊區間已經被合併。

**程式碼**

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
        int n = intervals.size();
        int i = 0;

        // 1. 處理所有在 newInterval 之前且不重疊的區間
        while (i < n && intervals[i][1] < newInterval[0]) {
            result.push_back(intervals[i]);
            i++;
        }

        // 2. 處理與 newInterval 重疊的區間，合併到 newInterval 中
        while (i < n && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);
            i++;
        }
        // 合併完成後，把 newInterval 加入結果
        result.push_back(newInterval);

        // 3. 處理所有在 newInterval 之後且不重疊的區間
        while (i < n) {
            result.push_back(intervals[i]);
            i++;
        }

        return result;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 我們僅對 intervals 列表做一次線性掃描，n 為原列表長度。

 - 空間複雜度：O(n)
     - 除了用於輸出結果的 result，僅使用常數級別的額外變數（i, n 及更新 newInterval 的空間），因此額外空間為 O(1)。最終輸出空間則為 O(n + 1)，可視為 O(n)。

### 01 Matrix

> [題目連結](https://leetcode.com/problems/01-matrix/)  
> **標籤**: Breadth-First Search, Dynamic Programming, Queue, Matrix  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個由 0 和 1 組成的 m x n 二維矩陣 `mat`，請返回一個同樣大小的矩陣 `res`，其中 `res[i][j]` 是原矩陣中位置 `(i, j)` 到最近的 0 的距離。距離由上下左右四個方向的一步算作 1。  

**範例**

> Example 1:  
> Input:  
> mat = [
>   [0,0,0],
>   [0,1,0],
>   [0,0,0]
> ]

> Output:  
> [
>   [0,0,0],
>   [0,1,0],
>   [0,0,0]
> ]

> Example 2:  
> Input:  
> mat = [
>   [0,0,0],
>   [0,1,0],
>   [1,1,1]
> ]

> Output:  
> [
>   [0,0,0],
>   [0,1,0],
>   [1,2,1]
> ]

**限制**

> - `m == mat.length`  
> - `n == mat[i].length`  
> - `1 <= m, n <= 10^4`  
> - `1 <= m * n <= 10^5`  
> - `mat[i][j]` 為 `0` 或 `1`  
> - 保證至少有一個 `0`  

**思路**

> 由於每個格子的值要計算到最近的 0 的最短步數，若對每個 1 單獨做 BFS，複雜度將高達 O((mn)·(mn))，在大矩陣中無法接受。  
> 
> 我們可以反向思考，將所有值為 0 的位置作為 BFS 的多源起點，同時從每個 0 開始向外擴散，逐步標記各 1 到「其最近 0」的距離。  
> 
> 演算法步驟如下：  
> 1. **初始化距離矩陣 `dist`**  
>    - 建立與 `mat` 同維度的距離矩陣 `dist`，初始化為一個很大的整數（如 `INT_MAX` 或 `m+n`）。  
> 2. **多源 BFS**  
>    - 建立一個佇列 `queue<pair<int,int>> q`。  
>    - 遍歷 `mat` 中所有格子，若 `mat[i][j] == 0`，則將 `(i, j)` 推入佇列 `q`，並將 `dist[i][j] = 0`。  
>    - 四個方向的偏移陣列 `dirs = {{1,0}, {-1,0}, {0,1}, {0,-1}}` 用於遍歷相鄰格子。  
> 3. **執行 BFS 擴散**  
>    - 當 `q` 不為空時，不斷取出當前格 `(r, c)`：  
>      1. 對於四個方向 `d`，計算鄰格 `(nr, nc) = (r + d.first, c + d.second)`。  
>      2. 檢查 `(nr, nc)` 是否在矩陣內，且若 `dist[nr][nc] > dist[r][c] + 1`，代表從當前 0 或已知較近 0 過來能取得更短距離：  
>         - 更新 `dist[nr][nc] = dist[r][c] + 1`，  
>         - 並將 `(nr, nc)` 推入佇列 `q`。  
>    - 由於所有 0 先入隊，第一輪拓展時距離將被標記為 1，再下一輪擴散將標記距離 2，以此類推。最終 `dist[i][j]` 即為該格到最近 0 的距離。  
> 
> 這種方法相當於從所有 0「同時」開始做 BFS，保證第一次到達某個 1 的距離即為最短距離。整體時間複雜度為 O(mn)。  

**程式碼**

```cpp
#include <vector>
#include <queue>
#include <utility>
#include <algorithm>
#include <limits>
using namespace std;

class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int m = mat.size();
        int n = mat[0].size();
        // 初始化 dist 矩陣，預設為一個足夠大的值
        const int INF = m + n; 
        vector<vector<int>> dist(m, vector<int>(n, INF));
        queue<pair<int,int>> q;

        // 1. 將所有 0 的位置入隊，並將 dist 設為 0
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0;
                    q.emplace(i, j);
                }
            }
        }

        // 四個方向偏移：下、上、右、左
        vector<pair<int,int>> dirs = {{1,0}, {-1,0}, {0,1}, {0,-1}};

        // 2. 多源 BFS，從所有 0 同時擴散
        while (!q.empty()) {
            auto [r, c] = q.front();
            q.pop();
            for (auto& d : dirs) {
                int nr = r + d.first;
                int nc = c + d.second;
                // 若鄰格在範圍內，且可用更短距離更新
                if (nr >= 0 && nr < m && nc >= 0 && nc < n) {
                    if (dist[nr][nc] > dist[r][c] + 1) {
                        dist[nr][nc] = dist[r][c] + 1;
                        q.emplace(nr, nc);
                    }
                }
            }
        }

        return dist;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(mn)
     - 每個格子最多被加入佇列一次，且從佇列中彈出後最多檢查 4 個鄰格，總共為 O(mn)。

 - 空間複雜度：O(mn)
     - 使用一個與輸入矩陣等大的距離矩陣 dist，以及最壞情況下可達 O(mn) 大小的佇列。

### K Closest Points to Origin

> [題目連結](https://leetcode.com/problems/k-closest-points-to-origin/)  
> **標籤**: Array, Math, Divide and Conquer, Sorting, Heap (Priority Queue)  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個「點」的列表 `points`，其中 `points[i] = [xi, yi]` 表示第 i 個點在 2D 平面上的座標。請返回距離原點 (0,0) 最近的 k 個點。  
>  
> 所有點到原點的距離由歐幾里得距離計算：`sqrt(xi^2 + yi^2)`。你可以按任何順序返回答案。答案傳回的 k 個點必須精確為「距離最小的 k 個」，不需要排序。  

**範例**

> Example 1:  
> Input: points = [[1,3],[-2,2]], k = 1  
> Output: [[-2,2]]  
> Explanation:  
> 點 (-2,2) 到原點距離為 sqrt(4+4) = sqrt(8) ≈ 2.828；  
> 而點 (1,3) 到原點距離為 sqrt(1+9) = sqrt(10) ≈ 3.162。  
> 因此最近的 1 個點為 [-2,2]。  

> Example 2:  
> Input: points = [[3,3],[5,-1],[-2,4]], k = 2  
> Output: [[3,3],[-2,4]]  
> Explanation:  
> 距離分別為：  
> (3,3) → sqrt(9+9)=sqrt(18) ≈ 4.243  
> (5,-1) → sqrt(25+1)=sqrt(26) ≈ 5.099  
> (-2,4) → sqrt(4+16)=sqrt(20) ≈ 4.472  
> 最近的兩個點為 (3,3) 與 (-2,4)，順序可以互換。  

**限制**

> - `1 <= k <= points.length <= 10^4`  
> - `-10^4 <= xi, yi <= 10^4`  
> - 答案保證是唯一的（除了順序）。  

**思路**

> 本題要求返回距離原點最近的 k 個點，可使用以下常見方法：  
>  
> 1. **Max-Heap (Priority Queue) 維護大小為 k 的集合**  
>    - 我們可以維護一個「最大堆」，堆中保存當前距離最小的 k 個點，並以距離作為比較依據。  
>    - 遍歷所有 `points`：  
>      1. 對第 i 個點 `(x, y)`，計算其「距離平方」`d = x*x + y*y`（不需開根號，比較平方即可）。  
>      2. 如果堆的大小小於 k，就直接將 `(d, i)` 推入最大堆；  
>      3. 否則，比較當前點距離 `d` 與堆頂元素的距離（即目前 k 個最遠點的距離），若 `d` 比堆頂小，代表這個點更接近原點，應該將堆頂彈出、並把 `(d, i)` 推入，否則跳過。  
>    - 最終，堆中就保留了 k 個「距離最小」的點；取出它們的索引即可。  
>    - **時間複雜度**：O(n log k)，其中 n 為點的數量；每次對堆操作為 log k，總共 n 次插入／比較。  
>    - **空間複雜度**：O(k)，最大堆中最多保存 k 個點。  
>  
> 2. **快速選擇法 (Quickselect)**  
>    - 類似快速排序的「分區」操作，利用 `nth_element` 或手動實現 partition，一次可以將第 k 小(距離)的點放到正確位置，左側都是更小距離的點。  
>    - `nth_element(points.begin(), points.begin()+k, points.end(), cmp)` 之後，前 k 個元素即為距離最小的 k 個點，但不保證排序。  
>    - **時間複雜度**：平均 O(n)，最壞 O(n^2)（但使用標準庫的 `nth_element` 大多能保證線性平均）。  
>    - **空間複雜度**：O(1)（就地在原陣列操作）。  
>  
> 此處我們以 **最大堆** 方法示範，因為邏輯清晰易理解。  

**程式碼**

```cpp
#include <vector>
#include <queue>
#include <cmath>
using namespace std;

class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        // 定義一個「最大堆」，元素對以 pair<distanceSquared, index>
        // distanceSquared 用作比較（距離平方越大排越前面）
        using PII = pair<int,int>;
        auto cmp = [](const PII& a, const PII& b) {
            // 距離平方大的排在堆頂（最大堆）
            return a.first < b.first;
        };
        priority_queue<PII, vector<PII>, decltype(cmp)> maxHeap(cmp);

        int n = points.size();
        // 遍歷所有點，維護一個大小最多為 k 的最大堆
        for (int i = 0; i < n; i++) {
            int x = points[i][0];
            int y = points[i][1];
            int distSq = x * x + y * y;  // 距離平方
            if ((int)maxHeap.size() < k) {
                // 先把前 k 個點放入堆
                maxHeap.emplace(distSq, i);
            } else if (distSq < maxHeap.top().first) {
                // 若當前點更接近原點，彈出堆頂後推入
                maxHeap.pop();
                maxHeap.emplace(distSq, i);
            }
            // 否則，當前點距離比堆中最遠的 k 個點還大，跳過
        }

        // 現在堆中有 k 個「距離最小」的點，把它們取出
        vector<vector<int>> result;
        result.reserve(k);
        while (!maxHeap.empty()) {
            int idx = maxHeap.top().second;
            maxHeap.pop();
            result.push_back(points[idx]);
        }
        return result;  // 返回順序可不固定
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n log k)
     - n = points.size()，每次插入／彈出堆的操作為 O(log k)，共進行 n 次。

 - 空間複雜度：O(k)
     - 最大堆中最多保存 k 個元素，此外只使用常數級輔助變數。


### Longest Substring Without Repeating Characters

> [題目連結](https://leetcode.com/problems/longest-substring-without-repeating-characters/)  
> **標籤**: Hash Table, Two Pointers, String, Sliding Window  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個字串 `s`，請找出不含重複字符的最長子字串的長度。  

**範例**

> Example 1:  
> Input: `s = "abcabcbb"`  
> Output: `3`  
> Explanation: 最長不含重複字符的子字串是 `"abc"`，長度為 3。  

> Example 2:  
> Input: `s = "bbbbb"`  
> Output: `1`  
> Explanation: 最長不含重複字符的子字串是 `"b"`，長度為 1。  

> Example 3:  
> Input: `s = "pwwkew"`  
> Output: `3`  
> Explanation: 最長不含重複字符的子字串是 `"wke"`（注意是子字串，必須是連續的），長度為 3。  
>    請注意，你的答案必須是子字串的長度，`"pwke"` 不是連續的子字串。  

**限制**

> - `0 <= s.length <= 5 * 10^4`  
> - `s` 由英文字母、數字、符號和空格組成。  

**思路**

> 本題可以利用「滑動視窗」（Sliding Window）＋「哈希表（字元映射）」技巧，在 O(n) 時間內找出最長不含重複字符的子字串長度。  
> 
> 1. **定義左右指標**  
>    - 使用兩個指標 `left`、`right` 表示視窗範圍，初始都指向 0。視窗代表目前考慮的子字串 `s[left..right-1]`。  
>    - 我們會讓 `right` 向右逐步擴展，同時保證視窗內沒有重複字符；只要發現重複，就移動 `left` 直到視窗內不含該重複字元為止。  
> 
> 2. **哈希表記錄最近出現位置**  
>    - 建立一個大小至少為 128（覆蓋 ASCII 範圍）的整數陣列 `lastIndex[128]`，初始值皆設為 -1。當我們遍歷到 `s[right]`，若之前 `lastIndex[s[right]]` ≠ -1，代表在 `s[right]` 字符上次出現的位置。  
>    - 每次右指標 `right` 移動到新字元時，檢查該字元上次出現的位置 `prev = lastIndex[s[right]]`：  
>      1. 如果 `prev >= left`，代表該字符之前在當前視窗範圍內出現過，因此必須將 `left` 更新到 `prev + 1`，才能移除重複。  
>      2. 將 `lastIndex[s[right]] = right` 作為該字符的新紀錄。  
> 
> 3. **更新答案**  
>    - 在每次擴展 `right` 之前或之後（都可，只要維持視窗內無重複即可），計算當前視窗長度 `right - left + 1`，並與 `maxLen` 比較取最大值。  
> 
> 4. **遍歷終止條件**  
>    - 讓 `right` 從 0 一路移動到 `n-1`，整個過程 O(n)；更新 `left` 最多也 O(n) 次，因此總複雜度 O(n)。  
> 
> 下面以範例 `s = "abba"` 來示範：  
> 
> - 初始化：`left = 0, maxLen = 0, lastIndex 全部為 -1`  
> 
> - `right = 0`，`s[0] = 'a'`：  
>   - `prev = lastIndex['a'] = -1`，不需移動 `left`。  
>   - 更新 `lastIndex['a'] = 0`。  
>   - 當前視窗長度 `0 - 0 + 1 = 1`，`maxLen = 1`。  
> 
> - `right = 1`，`s[1] = 'b'`：  
>   - `prev = lastIndex['b'] = -1`，不需移動 `left`。  
>   - 更新 `lastIndex['b'] = 1`。  
>   - 當前視窗長度 `1 - 0 + 1 = 2`，`maxLen = 2`。  
> 
> - `right = 2`，`s[2] = 'b'`：  
>   - `prev = lastIndex['b'] = 1`，此時 `1 >= left(0)`，表示重複出現在視窗內。  
>   - 更新 `left = prev + 1 = 2`，移除先前的 `'b'`。  
>   - 更新 `lastIndex['b'] = 2`。  
>   - 當前視窗長度 `2 - 2 + 1 = 1`，`maxLen = max(2,1) = 2`。  
> 
> - `right = 3`，`s[3] = 'a'`：  
>   - `prev = lastIndex['a'] = 0`，因為 `0 < left(2)`，表示上次的 `'a'` 已不在當前視窗內，故不移動 `left`。  
>   - 更新 `lastIndex['a'] = 3`。  
>   - 當前視窗長度 `3 - 2 + 1 = 2`，`maxLen = max(2,2) = 2`。  
> 
> 最終 `maxLen = 2`，對應的最長子字串例如 `"ab"` 或 `"ba"`。  

**程式碼**

```cpp
#include <string>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int lengthOfLongestSubstring(const string& s) {
        int n = s.size();
        // lastIndex 用於記錄每個 ASCII 字元上次出現的位置，初始化為 -1
        vector<int> lastIndex(128, -1);

        int maxLen = 0;
        int left = 0;  // 滑動視窗左邊界

        for (int right = 0; right < n; right++) {
            char c = s[right];
            int prev = lastIndex[c];
            // 如果該字元上次出現在視窗內，則移動左邊界
            if (prev >= left) {
                left = prev + 1;
            }
            // 更新該字元最近出現的位置
            lastIndex[c] = right;
            // 計算當前視窗長度並更新最大值
            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - right 指標從 0 到 n-1 各走一次；left 最多向右移動 n 次，整體加起來為線性時間。

 - 空間複雜度：O(1)
     - 我們只使用固定大小為 128 的陣列儲存 ASCII 字元的最近位置，不隨字串長度 n 增長。

### 3Sum

> [題目連結](https://leetcode.com/problems/3sum/)  
> **標籤**: Array, Two Pointers, Sorting  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個整數陣列 `nums`，請找出所有不重複且總和為 0 的三元組 `[nums[i], nums[j], nums[k]]`，並將這些三元組以二維陣列形式回傳。  
>  
> 注意：答案中不能包含重複的三元組。  

**範例**

> Example 1:  
> Input: nums = [-1,0,1,2,-1,-4]
> Output: [[-1,-1,2],[-1,0,1]]
> Explanation:
> 三元組 [-1,-1,2] 和 [-1,0,1] 的元素和均為 0，且不重複。

> Example 2:  
> Input: nums = [0, 1, 1]
> Output: []

> Example 3:  
> Input: nums = [0, 0, 0]
> Output: [[0, 0, 0]]

**限制**

> - `0 <= nums.length <= 3000`  
> - `-10^5 <= nums[i] <= 10^5`  

**思路**

> 1. **排序 (Sorting)**  
>    - 先將陣列 `nums` 依升冪排序，方便後續使用雙指針掃描並去重。  
>  
> 2. **固定第一個數字，對剩餘部分做雙指針搜尋**  
>    - 令三元組的第一個元素索引為 `i`，遍歷 `i` 從 `0` 到 `n-3`（因為至少要三個元素）。  
>    - 為了避免重複，在遍歷 `i` 時，如果 `i > 0` 且 `nums[i] == nums[i-1]`，直接跳過。  
>    - 接著在排序好的陣列中，對 `i` 之後的子陣列使用左右指標 `left = i+1`、`right = n-1`，在 `left < right` 的情況下：  
>      1. 計算三數總和 `sum = nums[i] + nums[left] + nums[right]`。  
>      2. 如果 `sum == 0`，則找到一組符合條件的三元組，將其加入結果，並同時移動 `left++`、`right--`，但要跳過重複元素（`while (left < right && nums[left] == nums[left-1]) left++;`、`while (left < right && nums[right] == nums[right+1]) right--;`），以避免重複三元組。  
>      3. 如果 `sum < 0`，代表總和太小，需要增大 `left` 對應的值，故 `left++`。  
>      4. 如果 `sum > 0`，代表總和太大，需要減小 `right` 對應的值，故 `right--`。  
>    - 重複上述雙指針搜尋流程直到 `left >= right`。  
>  
> 3. **去重技巧**  
>    - 在固定 `i` 時：若 `i > 0` 且 `nums[i] == nums[i-1]`，直接跳過，避免第一個數字重複。  
>    - 找到 `sum == 0` 後，移動 `left`、`right` 時，跳過相鄰且相同的值。  

**程式碼**

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        int n = nums.size();
        if (n < 3) return result;
        
        // 1. 排序
        sort(nums.begin(), nums.end());
        
        // 2. 遍歷 i 作為三元組的第一個元素
        for (int i = 0; i < n - 2; i++) {
            // 避免第一個元素重複
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int left = i + 1;
            int right = n - 1;
            
            // 3. 雙指針搜尋其餘兩個元素
            while (left < right) {
                long sum = (long)nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    // 找到一組符合條件的三元組
                    result.push_back({nums[i], nums[left], nums[right]});
                    
                    // 跳過重複的 left 元素
                    int leftVal = nums[left];
                    while (left < right && nums[left] == leftVal) {
                        left++;
                    }
                    // 跳過重複的 right 元素
                    int rightVal = nums[right];
                    while (left < right && nums[right] == rightVal) {
                        right--;
                    }
                }
                else if (sum < 0) {
                    // 總和太小，left 往右移動
                    left++;
                }
                else { // sum > 0
                    // 總和太大，right 往左移動
                    right--;
                }
            }
        }
        
        return result;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n²)
     - 排序 O(n log n)；主迴圈遍歷 i 為 O(n)，每次在子陣列執行雙指針搜尋為 O(n)，合計為 O(n²)。

 - 空間複雜度：O(log n)（排序所需）
     - 若忽略返回結果所佔空間，只使用排程排序遞迴空間為 O(log n)。若計算輸出占用，則最壞情況 O(n²)（取決於可形成幾組三元組）。

### Binary Tree Level Order Traversal

> [題目連結](https://leetcode.com/problems/binary-tree-level-order-traversal/)  
> **標籤**: Tree, Breadth-First Search, Queue  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個二元樹的根節點 `root`，請返回其「層序遍歷」（level order traversal）的節點值——即從左到右，逐層訪問所有節點。輸出結果應為一個二維陣列，其中第 `i` 個子陣列包含樹的第 `i` 層所有節點的值。  

**範例**

> Example 1:  
> Input: root = [3,9,20,null,null,15,7]
> Output: [[3],[9,20],[15,7]]
> Explanation:
> 層序遍歷結果為 [[3], [9,20], [15,7]]。
```text
  3
  / \
 9  20
    / \
   15  7
```

> Example 2:  
> Input: root = [1]
> Output: [[1]]

> Example 3:  
> Input: root = []
> Output: []

**限制**

> - 樹中節點總數在範圍 `[0, 2000]`。  
> - `-1000 <= Node.val <= 1000`。  

**思路**

> - 本題最常用的做法是「廣度優先搜尋」（Breadth-First Search, BFS），利用佇列（queue）按層次逐層訪問。  
> - 具體流程：  
>   1. 如果 `root` 為 `nullptr`，直接回傳空陣列 `[]`。  
>   2. 建立一個佇列 `queue<TreeNode*> q`，先將 `root` 推入。  
>   3. 當佇列不為空時，先記錄當前層的節點數 `levelSize = q.size()`，這個值代表本次要從佇列中取出的節點數量（相當於本層的節點數）。  
>   4. 建立一個臨時容器 `vector<int> levelVals`，用來儲存本層所有節點的值。  
>   5. 用一個迴圈跑 `levelSize` 次，每次從佇列中取出一個節點 `node = q.front()`、`q.pop()`，並把 `node->val` 加入 `levelVals`。若 `node->left` 非空，則 `q.push(node->left)`；若 `node->right` 非空，則 `q.push(node->right)`。  
>   6. 當 `levelSize` 次迴圈結束，代表本層所有節點都處理完，將 `levelVals` 推入結果二維陣列 `result`。  
>   7. 重複步驟 3～6，直到佇列為空。  
> - 這樣可以保證每次只將同一層的節點值收集一次，並依序推入結果。  

**程式碼**

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

#include <vector>
#include <queue>
using namespace std;

class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        if (root == nullptr) {
            return result;  // 空樹返回空陣列
        }
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            int levelSize = q.size();  // 當前層的節點數
            vector<int> levelVals;
            levelVals.reserve(levelSize);
            
            for (int i = 0; i < levelSize; i++) {
                TreeNode* node = q.front();
                q.pop();
                levelVals.push_back(node->val);
                if (node->left) {
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
            }
            
            result.push_back(levelVals);
        }
        
        return result;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - n 為樹中節點總數。每個節點會被加入佇列並彈出一次，並進行常數次操作（讀值、推入子節點），整體為 O(n)。

 - 空間複雜度：O(n)
     - 佇列最壞情況下可能儲存一整層所有節點，例如接近 n/2 個（若樹為完全二元樹）。此外，結果的二維陣列也會儲存所有節點值，需 O(n)。

### Clone Graph

> [題目連結](https://leetcode.com/problems/clone-graph/)  
> **標籤**: Hash Table, Depth-First Search, Breadth-First Search, Graph  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 20 分鐘  

**題目描述**

> 給定一個無向連通圖的節點 `node`，請返回此圖的深拷貝（Clone）。  
>  
> 圖中每個節點包含一個整數 `val`（節點編號），以及一個鄰居列表 `vector<Node*> neighbors`，代表與該節點相連的所有節點。節點編號範圍為 1 到 N（N ≤ 100）。  
>  
> 需構造一個與原圖結構、節點值完全相同的新圖，且新舊圖之間的節點指標互不相同。

**範例**

> Example 1:
> adjList = [[2,4],[1,3],[2,4],[1,3]]
> 返回：[[2,4],[1,3],[2,4],[1,3]]
> 圖結構：
```text
1 -- 2
|    |
4 -- 3
```

> Example 2:
> adjList = [[]]
> 圖只有一個節點，且沒有鄰居，返回：[[]]

> Example 3:
> adjList = [] 
> 空圖，返回：nullptr

**限制**

> 節點數量不超過 100。
> 節點值 val 範圍為 1 <= val <= 100，且圖為無向連通圖。
> 同一節點的鄰居列表中不含重複節點。
> 圖中無自環或重邊。

**思路**

> 複製一個圖的常見方法是透過 深度優先搜尋（DFS） 或 廣度優先搜尋（BFS），並使用一個 哈希映射 來保存「原節點指標 → 新節點指標」的映射，避免重複複製同一個節點。
>　下面以 DFS 方式示範：
> 1. **建立映射表**
>  - 使用 unordered_map<Node*, Node*> visited;
>    - Key：原圖的節點指標
>    - Value：對應已創建的複製節點指標
> 2. **遞迴 DFS 函式 `clone(Node* node)`**
>  - 如果 `node == nullptr`，直接返回 `nullptr`。
>  - 如果 `visited.count(node)` 已包含此 `node`，代表該節點已被複製過，直接返回 `visited[node]`。
>  - 否則：
>    - 創建一個新節點 Node* copy = new Node(node->val);，先只複製節點值，不處理鄰居列表。
>    - 在 visited[node] = copy; 中記錄這個映射關係，避免後續重複複製。
>    - 遍歷 node->neighbors 中的每個鄰居 nei：呼叫 clone(nei)，並將返回的新節點指標加入 copy->neighbors。
>    - 返回 copy。
> 3. 主函式 `cloneGraph(Node* node)`
>  - 如果輸入 node == nullptr，直接返回 nullptr。
>  - 否則呼叫並返回 clone(node)，即可獲得整個圖的克隆。

**程式碼**

```cpp
#include <vector>
#include <unordered_map>
using namespace std;

// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() : val(0), neighbors(vector<Node*>()) {}
    Node(int _val) : val(_val), neighbors(vector<Node*>()) {}
    Node(int _val, vector<Node*> _neighbors) : val(_val), neighbors(_neighbors) {}
};

class Solution {
public:
    unordered_map<Node*, Node*> visited;

    Node* cloneGraph(Node* node) {
        if (node == nullptr) return nullptr;
        return clone(node);
    }

private:
    Node* clone(Node* node) {
        if (visited.count(node)) {
            return visited[node];
        }
        Node* copy = new Node(node->val);
        visited[node] = copy;
        for (Node* nei : node->neighbors) {
            copy->neighbors.push_back(clone(nei));
        }
        return copy;
    }
};
```

**複雜度分析**

 - 時間複雜度：O(N + M)
     - N 為節點數量，M 為邊的數量。每個節點只會被 clone 訪問一次，並處理它所有的鄰邊。

 - 空間複雜度：O(N)
     - 使用 visited 哈希映射存儲 N 個節點，遞迴棧最壞情況也為 O(N)。

### Evaluate Reverse Polish Notation

> [題目連結](https://leetcode.com/problems/evaluate-reverse-polish-notation/)  
> **標籤**: Stack, Array  
> **語言**: C++  
> **難度**: Medium  
> **解題時間**: 15 分鐘  

**題目描述**

> Evaluate the value of an arithmetic expression in Reverse Polish Notation.  
>  
> 有效的運算符為 `+`, `-`, `*`, `/`。每個運算符都應該作用於最近的兩個操作數之上。除法運算符 `/` 表示整數除法，結果會向零取整。  
>  
> 你可以假設給定的 RPN 表達式總是有效的，也就是說運算過程中不會出現除以零的情況，並且最終得到的結果只會有一個值。  
>  
> 舉例來說，給定 RPN 表達式索引為 `["2", "1", "+", "3", "*"]`，其對應的計算步驟為：  
> 1. 遇到 `"2"` 與 `"1"`，先把兩個數字當作操作數推入堆疊。  
> 2. 遇到 `"+"`，從堆疊彈出 `1` 和 `2`，計算 `2 + 1 = 3`，再把結果 `3` 推回堆疊。  
> 3. 接著讀到 `"3"`，把 `3` 推入堆疊。  
> 4. 最後讀到 `"*"`，從堆疊彈出剛剛的 `3`（結果）和新推入的 `3`，計算 `3 * 3 = 9`，再把結果 `9` 推回。  
> 5. 到達結尾，堆疊中只剩下一個值 `9`，即為最終答案。  

**範例**

> Example 1:  
> Input: `tokens = ["2", "1", "+", "3", "*"]`  
> Output: `9`  
> Explanation: `((2 + 1) * 3) = 9`  

> Example 2:  
> Input: `tokens = ["4", "13", "5", "/", "+"]`  
> Output: `6`  
> Explanation: `(4 + (13 / 5)) = 6`  

> Example 3:  
> Input: `tokens = ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]`  
> Output: `22`  
> Explanation:  
>   1. 當前堆疊狀態：`[10, 6, 9, 3]`  
>   2. 遇到 `"+"`：彈出 `3, 9` → `9 + 3 = 12` → 推入 `12` → `[10, 6, 12]`  
>   3. 遇到 `"-11"`：推入 `-11` → `[10, 6, 12, -11]`  
>   4. 遇到 `"*"`：彈出 `-11, 12` → `12 * -11 = -132` → 推入 `-132` → `[10, 6, -132]`  
>   5. 遇到 `"/"`：彈出 `-132, 6` → `6 / -132 = 0`（整數除法向零取整）→ 推入 `0` → `[10, 0]`  
>   6. 遇到 `"*"`：彈出 `0, 10` → `10 * 0 = 0` → 推入 `0` → `[0]`  
>   7. 遇到 `"17"`：推入 `17` → `[0, 17]`  
>   8. 遇到 `"+"`：彈出 `17, 0` → `0 + 17 = 17` → 推入 `17` → `[17]`  
>   9. 遇到 `"5"`：推入 `5` → `[17, 5]`  
>  10. 遇到 `"+"`：彈出 `5, 17` → `17 + 5 = 22` → 推入 `22` → `[22]`  
>  11. 最終答案 `22`。  

**限制**

> - `1 <= tokens.length <= 10^4`  
> - 每個 `tokens[i]` 要麼是一個整數（可能帶負號），要麼是運算符 `"+"`, `"-"`, `"*"`, `"/"`  
> - 在 RPN 運算過程中，不會出現除以零的情況  
> - 最終結果一定是一個整數，且範圍在 32 位元整數之內  

**思路**

> - 使用棧（Stack）來模擬 RPN 計算：  
>   1. 遍歷 `tokens` 陣列，若當前元素是數字字串，就將其轉換成 `int` 後推入棧中。  
>   2. 若當前元素是運算符（`"+", "-", "*", "/"`），則從棧中先後彈出兩個操作數（右操作數先彈出，左操作數後彈出）。  
>   3. 根據運算符對這兩個操作數執行相應運算，並把計算結果再推回棧中。  
>   4. 反覆上述步驟，直到遍歷結束，棧中只剩一個元素，即為最終答案。  
> - 注意：除法運算時要做整數除法，向零取整。C++ 中使用 `/` 即可滿足該條件。  

**程式碼**

```cpp
#include <vector>
#include <string>
#include <stack>
#include <cstdlib>   // for std::stoi

using namespace std;

class Solution {
public:
    int evalRPN(const vector<string>& tokens) {
        stack<int> st;
        
        for (const auto& tok : tokens) {
            if (tok == "+" || tok == "-" || tok == "*" || tok == "/") {
                // 先彈出右操作數，再彈出左操作數
                int b = st.top(); st.pop();
                int a = st.top(); st.pop();
                int res = 0;
                
                if (tok == "+") {
                    res = a + b;
                } else if (tok == "-") {
                    res = a - b;
                } else if (tok == "*") {
                    res = a * b;
                } else { // tok == "/"
                    // C++ / 即為向零取整
                    res = a / b;
                }
                
                st.push(res);
            } else {
                // 整數字串，轉成 int 並推入棧中
                st.push(stoi(tok));
            }
        }
        
        // 最終棧中只剩一個結果
        return st.top();
    }
};
```

**複雜度分析**

 - 時間複雜度：O(n)
     - 需要對 tokens 進行一次線性遍歷。對於每個元素，要麼做一次 push（O(1)），要麼做一次 pop、運算、再 push（都為 O(1) 操作）。因此整體為 O(n)。

 - 空間複雜度：O(n)
     - 最壞情況下（所有元素都是數字），棧中最多會存 n 個數字，所以需要 O(n) 的額外空間。

## Week 4 (8/8)
