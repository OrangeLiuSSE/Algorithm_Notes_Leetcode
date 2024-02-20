# 无限循环的妙用

## [1. [leetcode 202]快乐数](https://leetcode.cn/problems/happy-number/)

### 题目分析

其实快乐数并不需要多难的理解，只需要进行简单的思考，快乐数的题目描述中：
- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
这一点看出这是一个循环，题目中要求最终若这个过程能出现1，则为True，若无法出现1，则为False。因此我们需要判断在什么情况下，他不会出现1。
**因为快乐数本质上是是一个循环的过程，因此我们要将每次循环的结果放入数组中，若再次出现之前的结果，则证明已经出现无线循环。**

因此我们根据上述的思想，可以写出以下代码：
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        sum_list = []
        while True:
            n = self.get_sum(n)
            if n == 1:
                return True
            elif n == 0:
                return False
            else:
                if n in sum_list:
                    return False
                else:
                    sum_list.append(n)

    def get_sum(self, num: int) -> int:
        new_num = 0
        while num:
            num, r = divmod(num, 10)
            new_num += r ** 2
        return new_num
```

在上述代码中，还有一个python中的知识需要学习 divmod库函数：
python divmod() 函数把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a // b, a % b)。

因此在上述代码
```python
while num:
	num, r = divmod(num, 10)
	new_num += r ** 2
```
中，通过循环的方式，求出最后一位数的平方
即
1. num = 169, 
2. num = 16, r = 9
3. num = 1, r= 6
4. num = 0, break
