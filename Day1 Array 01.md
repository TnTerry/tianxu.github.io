# Day 1

## 二分查找（704）

二分查找的不变量：

1. 区间形式：左闭右闭
2. 更新条件：`left = middle - 1`
   - `left = middle - 1` 可以将 `middle` 从搜索范围中排除

```python
def binary_search(nums, target):

    '''
    nums: 待查找的数组
    target: 目标元素
    '''

    left, right = 0, len(nums) - 1 # 左闭右闭区间

    while left < right: # left == right时，区间也有意义
        middle = left + (right - left) // 2

        if nums[middle] > target:
            right = middle - 1
        elif nums[middle] < target:
            left = middle + 1
        else:
            return middle
    
    return -1

```
## 删除元素（27）

双指针法：快慢指针

- 快指针：`fast` 遍历数组中的所有元素
- 慢指针：`slow` 标记待更新的元素的位置

```python
def delete_element(nums, val):

    '''
    nums: 待修改的数组
    val: 删除的元素值
    '''

    slow = 0
    
    for fast in range(0, len(nums)):
        if nums[fast] != val: # 遇到非待删除的元素
            nums[slow] = nums[fast] # 存储到slow指针的位置，即新数组的最后位置
            slow += 1
    
    return slow

```