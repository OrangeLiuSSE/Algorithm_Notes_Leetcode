Edited by: Orange Liu,  Feb. 20 2024

哈希表，本质上是一个 <font color=red>key-value</font> 值的存储的数据结构，在python中，HashMap所表示的数据结构为list，即字典。

在构建哈希表过程中，一般都会设置一个哈希函数，使得该key值在底层数据结构中映射为某个其哈希表长度范围内的值。

通常情况下，哈希函数的输入空间远大于输出空间，会出现哈希冲突的现象，为了解决哈希冲突，解决方案： [6.2   哈希冲突](https://www.hello-algo.com/chapter_hashing/hash_collision/)


# 用哈希表解决的问题

其实哈希表的数据结构基本决定了其主要能够解决问题的范围，由于哈希表中的key值多种多样（在python中），因此我们可以利用其存储key-value两个值的特点，完成以下类型的题目：
1.  [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)
2. [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)
3. [1. 两数之和](https://leetcode.cn/problems/two-sum/)


## 题目分析

[1. 两数之和](https://leetcode.cn/problems/two-sum/)

***本题中的提示中提到，每个例子只有一个有效答案，换句话数，不会出现多于两个的重复元素，因此使用哈希表存储的过程中不会出现哈希冲突的现象***
两数之和虽然可以通过暴力解法顺利通过，但在实际算法环境中，使用Hash法能将复杂度从O(n2) 降低至O(n)，由于Hash能够存储key-value值，且本题要求的是返回下标，因此我们可以将下标作为value值，将实际值作为key值存储，在遍历数组过程中，查找是否存在target-当前值的key，如果存在则返回两个value即可。

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        nums_dict = dict()
        for index, value in enumerate(nums):
            if target - value in nums_dict.keys():
                return [nums_dict[target-value], index]
            else:
                nums_dict[value] = index
        return []
```

有关本道题的升级版本[四数之和](https://leetcode.cn/problems/4sum-ii/)，本质上的思路和两数之和类似，在四数之和中，题目要求只返回能够满足条件的四数之和的次数，因此我们将四个数组两两分开，分别求出其和，并在每次求出和后更新hash表，每次更新只需+1，这样可以将时间复杂度从On4降低为On2