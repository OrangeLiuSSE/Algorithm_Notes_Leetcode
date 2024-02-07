# 二分法

## 方法简介

***此方法用于有序且不重复数组的某个值的查询***

即模拟二分过程，创建两个下标变量（头与尾），然后确定中间点，并判断是否中间点对应的值大于还是小于要查询的值，并根据中间值和查找值的大小关系进一步确定下一个区间在中间值左侧还是右侧，最终可以确定是否存在此值。
同时，在编写代码的过程中，要注意区间的含义，在本二分法中，我们使用左闭右闭区间[left, right]，因此在更新左右值的过程中，是通过middle +/- 1去完成的；同时当left=right的时候，该区间是有含义的。
## 示例代码

下面的示例代码题目为：[二分查找](https://leetcode.cn/problems/binary-search/description/)

```python
def findValue(nums, target):
	left = 0
	right = len(nums) - 1
	# 左闭右闭区间，因此等于有含义
	while left <= right:
		middle = left + (right - left)//2    #取下限 这样能有效防止list index out of range
		if nums[middle] < target:
			left = middle + 1
		elif nums[middle] > target:
			right = middle - 1
		else:
			return middle
	return -1
```

# 快慢指针（双指针）
## 方法简介

快慢指针，一般用于不允许使用额外空间来对数组进行修改的题目，简单而言就是使用两个指针，实现两个for循环的功能，可以用于替换元素，获取元素等操作，同时也可查询最小子串。
## 示例代码

下面的示例代码题目为：[比较含退格的字符串](https://leetcode.cn/problems/backspace-string-compare/)
```python
class Solution(object):
    def backspaceCompare(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        fast_index = 0
        slow_index = 0
        s_dict = list(s)
        while fast_index < len(s_dict):
        # 快指针寻找'#'，慢指针定位在第一个需要删除的元素上，并等待被替换，如果#多于要删除的元素的话（慢指针index小于0），则慢指针一直置0
            if s_dict[fast_index] != "#":
                if slow_index != fast_index:
                    s_dict[slow_index] = s_dict[fast_index]
                fast_index += 1
                slow_index += 1
            else:
                slow_index = slow_index - 1 if slow_index -1 >= 0 else 0
                fast_index += 1
        # 数组[] 为左闭右开区间，因此如果为0，则返回空值
        s_final = "".join(s_dict[0:slow_index])
        
        fast_index = 0
        slow_index = 0
        t_dict = list(t)
        while fast_index < len(t_dict):
            if t_dict[fast_index] != "#":
                if slow_index != fast_index:
                    t_dict[slow_index] = t_dict[fast_index]
                fast_index += 1
                slow_index += 1
            else:
                slow_index = slow_index - 1 if slow_index -1 >= 0 else 0
                fast_index += 1
  
        t_final = "".join(t_dict[0:slow_index])

        return s_final == t_final
```

# 滑动窗口
## 方法简介

滑动窗口很有意思，其实和快慢指针类似，但是滑动窗口中的双指针所涵括的数组长度是受限制的，因此滑动窗口多用于与长度限制有关的数组题目。
其中right为右边界 left为左边界，扩张时，右边界向右扩张；收缩时，左边界向右收缩，并随时判断是否满足题目条件并更新相关结果，达到左右边界均只遍历一遍数组的时间复杂度是O(n)。

## 示例代码

下面的代码为： [长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)
```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        """
        :type target: int
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = 0
        num_cnt = 0
        min_length = float('inf')

        while right < len(nums):
            num_cnt += nums[right]
            while num_cnt >= target:
                if min_length > right - left + 1:
                    min_length = right - left + 1
                num_cnt -= nums[left]
                left += 1
            right += 1
        if min_length == float('inf'):
            return 0
        else:
            return min_length
```

# 其他做题思路

在做数组相关的题目过程中，一定要注意临界值的定义，包括区间到底是左闭右开还是左闭右闭区间，在做没有上述做题思路的数组题目是，一定要注意***循环过程中临界值的更新***。

## 题目示例

### 螺旋矩阵

[螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if len(matrix) == 0:
            return []
        output_list = []
        top, bottom, left, right = 0, len(matrix) - 1, 0, len(matrix[0]) - 1
        while True:
            # left to right:
            for y_index in range(left, right + 1):
                output_list.append(matrix[top][y_index])
            top += 1
            if top > bottom:
                break
            # top to bottom:
            for x_index in range(top, bottom + 1):
                output_list.append(matrix[x_index][right])
            right -= 1
            if left > right:
                break
            # right to left:
            for y_index in range(right, left - 1, -1):
                output_list.append(matrix[bottom][y_index])
            bottom -= 1
            if top > bottom:
                break
            # bottom to top
            for x_index in range(bottom, top - 1, -1):
                output_list.append(matrix[x_index][left])
            left += 1
            if left > right:
                break
                
        return output_list
```


在做数组相关题目中，一定要看清是否题目中有限定条件，例如： ***有序， 不重复***等等，有利于做题思路解出。