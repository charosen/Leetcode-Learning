<h1>Leetcode回溯系列（三）--- 组合之和(二)</h1>

题目：<https://leetcode-cn.com/problems/combination-sum-ii/>

<h2>1. 题目知识点</h2>

1. 回溯算法：
    1. **思想**：回溯算法可看作枚举法/暴力破解的升级版。是一种`选优搜索`法，按`选优条件`向前搜索，以达到最终`目标`。但当探索到某一步时，发现原先选择并`不优`或`达不到目标`，则`回溯`；
    2. **场景**：回溯算法从解决问题的每一步的所有可能选项里选择出可行解决方法，适合`由多个步骤组成的问题，并且每一个步骤都有多个选项`。当我们在某一步选择了其中一个选项，就进入下一步，然后又面临新的选项。
    3. **画图理解**：分析回溯问题，需要画图理清思路和寻找边界条件。用回溯法解决的问题可以使用树状结构来表示，某一个步骤有n个可能的选项，那么每一个步骤可以看作是树的一个节点，每一个选项可视作树的边，后续步骤是前序步骤的字节点。
    4. **实现**：回溯算法适合用**递归**实现。**当算法到达某一个节点时，尝试使用所有可能的选项，并在满足条件的前提下递归的前往下一个节点**。


<h2>2. 题目分析</h2>

1. 无序、有重复、正整数数组
2. 数组中的数字只能被选取一次
3. 解集不能包含重复的组合

<h2>3. 题目解法</h2>

<h3>3.1. 回溯法+剪枝</h3>

**核心思想**：

1. 虽然按顺序取能够遍历所有组合，但是对于重复数组而言，按顺序取可能取出重复组合；
2. 对重复数组去重的一个核心思想是先对数组进行升序排序，按顺序取，对于重复元素，只让第一个进入循环，后面的就不要再进入循环了(重复的元素一定不是排好序以后的第 1 个元素和相同元素的第 1 个元素)
3. 该算法去重核心思想：不允许同一层级/同一递归深度/同一个循环内存在重复元素。但是却允许了不同层级/不同递归深度/不同循环之间存在重复，基于这个思想来剪枝以去重；
    1. 不是一次循环/一层级的第一个分支，`curr > begin`；
    2. 当前分支元素与前一分支的元素相同，`candidates[curr-1] == candidate[curr]`


```
                例1
                  1
                 / \
                2   2  这种情况不允许发生 
               /     \
              5       5
                例2
                  1
                 /
                2      这种情况确是允许的
               /
              2  
```

![](https://pic.leetcode-cn.com/a470bcb582807c465ec03accfb29f204caab1438750e6fc5b029eb22700d7079-40-2.png)

![](https://pic.leetcode-cn.com/905c5e4df393a43890932903a5234e51048bc9a0a3aa7f3fc4fb0a65535e6a0b-40-3.png)

```
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        size = len(candidates)
        # handle empty array
        if size == 0: return []
        # sort array
        candidates.sort()
        res = []
        self.__dfs(0, target, size, candidates, [], res)
        return res

    
    def __dfs(self, start: int, target: int, size: int, candidates: List[int], pre: List[int], res: List[List[int]]) -> None:
        # stop condition
        if target == 0:
            res.append(pre[:])
            return
        
        for index in range(start, size):
            if target < candidates[index]:
                break
            
            if (index > start) and (candidates[index-1] == candidates[index]):
                continue
            
            pre.append(candidates[index])
            self.__dfs(index+1, target-candidates[index], size, candidates, pre, res)
            pre.pop()

```