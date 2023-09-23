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

# Day 2

## 长度最小的子数组（209）

给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

示例：

输入：s = 7, nums = [2,3,1,2,4,3]

输出：2

解释：子数组 [4,3] 是该条件下的长度最小的子数组。

<font size = 4>核心思路：滑动窗口</font>

- 形式类似于双指针，`left, right` 标记窗口的起始位置和结束位置
- 窗口范围内是子数组
- 子数组和小于`target`时，`right` 右滑
- 子数组和大于`target`时，`left` 左滑

```python

def minSubArrayLen(self, target: int, nums: List[int]) -> int:
    left, right = 0, 0
    min_length = len(nums) + 1 # 设置大于数组长度的值，便于判别
    array_sum = 0

    while right < len(nums):
        array_sum += nums[right]

        while array_sum > target:
            array_sum -= nums[left]
            min_length = min(min_length, right - left +1)
            left -= 1
        
        right += 1
    
    if min_length > len(nums): # 没有找到满足条件的子数组
        return 0
    else:
        return min_length

```


## 904 水果成篮

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 fruits 表示，其中 fruits[i] 是第 i 棵树上的水果 种类 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

你只有 两个 篮子，并且每个篮子只能装 单一类型 的水果。每个篮子能够装的水果总量没有限制。
你可以选择任意一棵树开始采摘，你必须从每棵树（包括开始采摘的树）上恰好摘一个水果。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。

一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。

给你一个整数数组 fruits ，返回你可以收集的水果的最大数目。

示例 1：

输入：fruits = [1,2,1]
输出：3
解释：可以采摘全部 3 棵树。

示例 2：

输入：fruits = [0,1,2,2]
输出：3
解释：可以采摘 [1,2,2] 这三棵树。
如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。

示例 3：

输入：fruits = [1,2,3,2,2]
输出：4
解释：可以采摘 [2,3,2,2] 这四棵树。
如果从第一棵树开始采摘，则只能采摘 [1,2] 这两棵树。

示例 4：

输入：fruits = [3,3,3,1,2,1,1,2,3,3,4]
输出：5
解释：可以采摘 [1,2,1,1,2] 这五棵树。

<font size = 4>核心思路：滑动窗口</font>

- 窗口指向包含的元素
- 使用counter实例对水果的类型个数进行统计
- 当counter实例中水果类型个数超过2个时， `left` 开始滑动

```python

def totalFruit(self, fruits: List[int]) -> int:
    left, max_kind = 0, 0

    cnt = Counter()

    for right, x in enumerate(fruits):
        cnt[x] += 1

        while len(cnt) > 2:
            cnt[fruits[left]] -= 1
            if cnt[fruits[left]] == 0:
                cnt.pop(fruits[left])
            left += 1
        
        ans = max(ans, right - left + 1)

```

## 59 螺旋矩阵

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。

输入：n = 3

输出：[[1,2,3],[8,9,4],[7,6,5]]

<font size = 4>核心要点：边界条件与不变量</font>

- 填充边界：**左开右闭**的准则
- 使用`offset`划定边界
- 使用`loop`记录螺旋多少圈
- `startx, starty`记录每一次螺旋的开始位置

```python

def generateMatrix(self, n: int) -> List[List[int]]:
    res = [[0] * n for _ in range(n)] # [0] * n 创造n个元素的0
    startx, starty = 0, 0
    loop = n // 2
    mid = n // 2
    count = 1
    
    for offset in range(1, loop + 1):
        
        # 填充上边
        for j in range(starty, n - offset):
            res[startx][j] = count
            count += 1

        # 填充右边
        for i in range(startx, n - offset):
            res[i][n - offset] = count
            count += 1
        
        # 填充下边
        for j in range(n - offset, startx, -1):
            res[n - offset][j] = count
            count += 1
        
        # 填充上边
        for i in range(n - offset, starty, -1):
            res[i][starty] = count
            count += 1
        
        # 更改边界条件
        startx += 1
        starty += 1
    
    if n % 2: # 奇数，填充中间一个数
        res[mid][mid] = count
    
    return res
```
