本问题来自做一道leetcode的题目的时候
[删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/description/)

原本采用的写法是直接遍历list：
```
class Solution:
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) < 2:
            return len(nums)
        ret = [nums[0]]
        for i in range(1, len(nums)):
            if nums[i] == ret[-1]:
                nums.pop(i)
            if  nums[i] != ret[-1]:
                ret.append(nums[i])
                
        return len(nums)
```
但是测试发现一直会报`index out of range`错误，
仔细分析之后觉得应该是删除元素之后索引移动的缘故，经过查询之后确实如此。
[参见这篇文章](https://www.cnblogs.com/bananaplan/p/remove-listitem-while-iterating.html)

>原因是删除元素之后list大小变小了，但是`for...in`循环依然是按照原来的大小进行循环，自然就会出现超过的索引。
python删除list的方法有很多，其中`pop`和`del`两种会连索引一起删除，后面的元素会往前移动占据原来的元素的位置。

解决办法很多，常见的标准做法有两种：
* 倒序遍历
* 拷贝原数组进行遍历，即使用原数组的切片进行遍历
`for i in range(nums[:])` 
缺点是耗费内存

至于倒序遍历也很简单，
`for i in range(len(nums)-1, -1, -1) //遍历到最后一个元素但不包括，即遍历到第一个元素`

正确的`AC`的代码见如下：
```
class Solution:
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) < 2:
            return len(nums)
        ret = [nums[-1]]
        for i in range(len(nums)-2, -1, -1):
            if nums[i] == ret[-1]:
                nums.pop(i)
            if  nums[i] != ret[-1]:
                ret.append(nums[i])
                
        return len(nums)
```
