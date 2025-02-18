# [519. Random Flip Matrix](https://leetcode-cn.com/problems/random-flip-matrix/)

## 题目

There is an m x n binary grid matrix with all the values set 0 initially. Design an algorithm to randomly pick an index (i, j) where matrix[i][j] == 0 and flips it to 1. All the indices (i, j) where matrix[i][j] == 0 should be equally likely to be returned.

Optimize your algorithm to minimize the number of calls made to the built-in random function of your language and optimize the time and space complexity.

Implement the Solution class:

- Solution(int m, int n) Initializes the object with the size of the binary matrix m and n.
- int[] flip() Returns a random index [i, j] of the matrix where matrix[i][j] == 0 and flips it to 1.
- void reset() Resets all the values of the matrix to be 0.

**Example 1**:

    Input
    ["Solution", "flip", "flip", "flip", "reset", "flip"]
    [[3, 1], [], [], [], [], []]
    Output
    [null, [1, 0], [2, 0], [0, 0], null, [2, 0]]

    Explanation
    Solution solution = new Solution(3, 1);
    solution.flip();  // return [1, 0], [0,0], [1,0], and [2,0] should be equally likely to be returned.
    solution.flip();  // return [2, 0], Since [1,0] was returned, [2,0] and [0,0]
    solution.flip();  // return [0, 0], Based on the previously returned indices, only [0,0] can be returned.
    solution.reset(); // All the values are reset to 0 and can be returned.
    solution.flip();  // return [2, 0], [0,0], [1,0], and [2,0] should be equally likely to be returned.

**Constraints:**

- 1 <= m, n <= 10000
- There will be at least one free cell for each call to flip.
- At most 1000 calls will be made to flip and reset.

## 题目大意

给你一个 m x n 的二元矩阵 matrix ，且所有值被初始化为 0 。请你设计一个算法，随机选取一个满足 matrix[i][j] == 0 的下标 (i, j) ，并将它的值变为 1 。所有满足 matrix[i][j] == 0 的下标 (i, j) 被选取的概率应当均等。

尽量最少调用内置的随机函数，并且优化时间和空间复杂度。

实现 Solution 类：

- Solution(int m, int n) 使用二元矩阵的大小 m 和 n 初始化该对象
- int[] flip() 返回一个满足 matrix[i][j] == 0 的随机下标 [i, j] ，并将其对应格子中的值变为 1
- void reset() 将矩阵中所有的值重置为 0

## 解题思路

- 二维矩阵利用哈希表转换为一维,每次随机选择一维中的任意一个元素，然后与最后一个元素交换，一维元素的总个数减一
- 哈希表中默认的映射为x->x, 然后将不满足这个映射的特殊键值对存入哈希表

## 代码

```go
package leetcode

import "math/rand"

type Solution struct {
	r     int
	c     int
	total int
	mp    map[int]int
}

func Constructor(m int, n int) Solution {
	return Solution{
		r:     m,
		c:     n,
		total: m * n,
		mp:    map[int]int{},
	}
}

func (this *Solution) Flip() []int {
	k := rand.Intn(this.total)
	val := k
	if v, ok := this.mp[k]; ok {
		val = v
	}
	if _, ok := this.mp[this.total-1]; ok {
		this.mp[k] = this.mp[this.total-1]
	} else {
		this.mp[k] = this.total - 1
	}
	delete(this.mp, this.total - 1)
	this.total--
	newR, newC := val/this.c, val%this.c
	return []int{newR, newC}
}

func (this *Solution) Reset() {
	this.total = this.r * this.c
	this.mp = map[int]int{}
}
```