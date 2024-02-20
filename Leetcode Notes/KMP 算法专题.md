# 实现strStr() or IndexOf()

[28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

## 题目分析

在找出字符串中第一个匹配项的下标中，我们不难得出暴力解法，即两层循环即可实现，时间复杂度为O(mn), 但在暴力解法过程中，我们发现每次发现不匹配后，直接从头重新开始匹配，这种情况下会出现一些浪费现象。举个例子：
```python
haystack = "aabaabaaf"
needle = "aabaaf"
```
上述的匹配串和模式串我们可以看到，在第一个循环匹配到 haystack\[5] 时，循环发现了不匹配的情况，则haystack重新退回，从haystack\[1]开始进行匹配，然而在haystack\[1-2] 这两次循环是无用的。

我们如何减少这种无意义的循环，转而让程序跳转至之前确认相同的地方重新匹配呢？

按照上述两个字符串的例子我们发现，在第一个循环中如果haystack\[5]无法匹配，则可以将needle\[5]转为needle\[2]，这样可以继续匹配，能够大大的缩短时间，如何实现这种算法，我们将介绍KMP算法
## KMP 算法介绍

KMP算法是通过记录先前匹配的最大相等前后缀的值来记忆之前匹配过的值的，在记录最大相等前后缀的过程中，需要进行以下分析：

```python 
needle = "aabaaf"
```
**前缀：** 指除了最后一个字符以外的所有字符串，在上述字符串中：a, aa, aab, aaba, aabaa 均为前缀
**后缀：** 指除了第一个字符以外的所有字符串，在上述字符串中：a, ab, aba, abaa, abaaf 均为后缀

什么叫最大相等前后缀，即前缀和后缀完全相等时，前后缀的长度为最大相等前后缀长度。

那么我们如何在字符串匹配的过程中使用到这个最大前后缀呢？
```python
haystack = "aabaabaaf"
needle = "aabaaf"
```
仍然是这个例子，我们可以知道在第一次循环中 走到 haystack\[5]和needle\[5]时两个字符不匹配，但是我们知道，既然needle能够走到needle\[5]，那么证明needle\[0-4]和haystack中所比较的字符串是相等的。也就是说 "<font color=red>aabaa</font>baaf"和 "<font color=red>aabaa</font>f"是肯定相等的，但是我们发现aabaa中， 最大相等前后缀为aa，也就是说，虽然第六位的b和f不相等了，但是前面相等的部分的最后aa和本身这个字符串的最开始两个字符aa是一样的，因此我们不需要立即回到aabaaf中第二个a来进行循环，而是尝试先将needle的下标返回到与后缀相等的前缀后的位置尝试比较，在这个例子中needle为aabaaf中的aa，因此可以将needle的下标转到b并重新与haystack\[5]进行比较，发现相等后可以继续向下比较知道needle下标指向最后一个字符。
## next数组求解

next数组本质上就是自己与自己比较的过程，其具体的求解方法为：最长的后缀先进行比较，并一一向后推进，出现不匹配情况时，与比较其他字符串过程相同:
```python
def getNext(s: str):
	head_index = 0
	next = []
	for tail_index in range(1, len(s)):
		while head_index > 0 and s[tail_index] != s[head_index]:
			head_index = next[head_index - 1]
		if s[tail_index] != s[head_index]:
			head_index += 1
		next[tail_index] = head_index

```


## 题目解答：

```python
class Solution:
    def getNext(self, s: str) -> List[int]:
        s_list = list(s)
  
        head_index = 0
        next = [0] * len(s_list)
        for tail_index in range(1, len(s_list)):
            while head_index > 0 and s_list[tail_index] != s_list[head_index]:
                head_index = next[head_index - 1]
            if s_list[tail_index] == s_list[head_index]:
                head_index += 1
            next[tail_index] = head_index
        return next

    def strStr(self, haystack: str, needle: str) -> int:
        next = self.getNext(needle)
        hay = list(haystack)
        need = list(needle)
        j = 0
        for i in range(len(hay)):
            while j > 0 and hay[i] != need[j]:
                j = next[j - 1]
            if hay[i] == need[j]:
                j += 1
            if j == len(need):
                return i - len(need) + 1
        return -1
```
