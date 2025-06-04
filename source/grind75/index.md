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

```markdown
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
```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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

```markdown
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