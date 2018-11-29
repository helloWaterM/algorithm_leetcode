### 数组中两个数的最大异或值

#### 题目描述

给定一个非空数组，数组中元素为 $a^0, a^1, a^2, … , a^{n-1}$，其中 $0 ≤ ai < 2^{31}$ 。

找到 ai 和aj 最大的异或 (XOR) 运算结果，其中0 ≤ *i*,  *j* < *n* 。

你能在O(*n*)的时间解决这个问题吗？

**示例:**

```
输入: [3, 10, 5, 25, 2, 8]

输出: 28

解释: 最大的结果是 5 ^ 25 = 28.
```



### 算法题解

核心的思想是搜索答案
从答案的高位开始搜索

判断答案是否存在需要用到掩码，把不想关的01抹去。如果不抹去，会影响后续的异或操作

MAX记录了最大的值，temp记录了可能可以作为新的答案的值，也就是C。

用 C^B = A, 通过判断A是否存在，来判断C是否存在。

(异或操作有这样的使用方式：A^B = C; 那么: A^C = B, B^C = A;)

最后：算法的复杂度为 $O(62n) = O(n)$



```
class Solution {
public:
​    int findMaximumXOR(vector<int>& nums) {
​        int mask = 0;
​        int MAX = 0;
​        for(int i = 31; i >=0; i--){   				 //由高位开始进行搜索
​            set<int> s;
​            mask = 1 << i | mask;
​            for(int num: nums){ 				   //用掩码把所有数字除了当前需要比较的位的其他位抹去
​                s.insert(num & mask);
​            }
​            int temp = MAX | 1 << i; 				//新的可能的答案
​            for(int ele: s){
​                if(s.find(ele^temp) != s.end()){ 		//可以找到匹配的 A=C^B
​                    MAX = temp;
​                    break;
​                }
​            }
​        }   
​        return MAX;        
​    }
};


```

