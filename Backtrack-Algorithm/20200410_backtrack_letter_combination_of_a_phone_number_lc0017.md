<h1>Leetcode回溯系列（九）--- 电话号码的字母组合</h1>

题目：<https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/>

<h2>1. 题目知识点</h2>

1. 回溯算法：
    1. **思想**：回溯算法可看作枚举法/暴力破解的升级版。是一种`选优搜索`法，按`选优条件`向前搜索，以达到最终`目标`。但当探索到某一步时，发现原先选择并`不优`或`达不到目标`，则`回溯`；
    2. **场景**：回溯算法从解决问题的每一步的所有可能选项里选择出可行解决方法，适合`由多个步骤组成的问题，并且每一个步骤都有多个选项`。当我们在某一步选择了其中一个选项，就进入下一步，然后又面临新的选项。
    3. **画图理解**：分析回溯问题，需要画图理清思路和寻找边界条件。用回溯法解决的问题可以使用树状结构来表示，某一个步骤有n个可能的选项，那么每一个步骤可以看作是树的一个节点，每一个选项可视作树的边，后续步骤是前序步骤的字节点。
    4. **实现**：回溯算法适合用**递归**实现。**当算法到达某一个节点时，尝试使用所有可能的选项，并在满足条件的前提下递归的前往下一个节点**。

<h2>2. 题目分析</h2>

1. 简单的回溯问题
2. 边界条件就是字符串长度k
3. 字符串操作会让代码写起来更简洁


<h2>3. 题目解法</h2>

<h3>3.1 回溯法</h3>

**核心思想**是分析回溯问题，必须画图！！！理清思路和边界条件。


**画图分析**

![](../media/lc0017-电话号码的字母组合.pdf)


```
from typing import Dict, List


class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if digits == "": return []
        mapping = {"2": ["a", "b", "c"],
                   "3": ["d", "e", "f"],
                   "4": ["g", "h", "i"],
                   "5": ["j", "k", "l"],
                   "6": ["m", "n", "o"],
                   "7": ["p", "q", "r", "s"],
                   "8": ["t", "u", "v"],
                   "9": ["w", "x", "y", "z"]}
        res = []
        self.backtrack(digits, "", res, mapping)
        return res
        

    
    def backtrack(self, digits: str, pre: str, res: List[str], mapping: Dict[str, List[str]]) -> None:
        if len(digits) == 0:
            res.append(pre)
            return

        for char in mapping[digits[0]]:
            self.backtrack(digits[1:], pre + char, res, mapping)
```