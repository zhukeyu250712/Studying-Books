# 题型整理

[TOC]

## 一、二分

### 1、二分常规模板

#### [704. 二分查找](https://leetcode.cn/problems/binary-search/)

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2:**

```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

**提示：**

1. 你可以假设 `nums` 中的所有元素是不重复的。

2. `n` 将在 `[1, 10000]`之间。

3. `nums` 的每个元素都将在 `[-9999, 9999]`之间。

```c
class Solution {
public:
    int search(vector<int>& nums, int target) {
        if(nums.empty()) return -1;
        int n=nums.size();
        int l=0,r=n-1;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        if(nums[r]==target) return r;
        return -1;
    }
};
```

```java
class Solution {
    public int search(int[] nums, int target) {
        int n=nums.length;
        int l=0,r=n-1;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        if(nums[r]==target) return r;
        return -1;
    }
}
```



#### [35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

**提示:**

$1 <= nums.length <= 10^4$
$-10^4 <= nums[i] <= 10^4$,
$nums $为 无重复元素 的 升序 排列数组
$-10^4 <= target <= 10^4$

```c
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n=nums.size();
        int l=0,r=n;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        return r;
    }
};
```

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int l=0,r=nums.length;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        return r;
    }
}
```



#### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)

给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题。

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

**提示：**

$0 <= nums.length <= 10^5$,
$-10^9 <= nums[i] <= 10^9$,
$nums$ 是一个非递减数组
$-10^9 <= target <= 10^9$

```c
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty()) return {-1,-1};
        
        int l=0,r=nums.size()-1;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        if(nums[l]!=target) return {-1,-1};
        int L=r;
        l=0,r=nums.size()-1;
        while(l<r){
            int mid=(l+r+1)>>1;
            if(nums[mid]<=target) l=mid;
            else r=mid-1;
        }
        return {L,r};
    }
};
```

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n=nums.length;
        if(n==0) return new int[]{-1,-1};
        int l=0,r=n-1;
        while(l<r){
            int mid=(l+r)>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }
        if(nums[r]!=target) return new int[]{-1,-1};
        int L=r;
        l=0;
        r=n-1;
        while(l<r){
            int mid=(l+r+1)>>1;
            if(nums[mid]<=target) l=mid;
            else r=mid-1;
        }
        return new int[]{L,r};
    }
}
```

#### [374. 猜数字大小](https://leetcode.cn/problems/guess-number-higher-or-lower/)

猜数字游戏的规则如下：

每轮游戏，我都会从 1 到 n 随机选择一个数字。 请你猜选出的是哪个数字。
如果你猜错了，我会告诉你，你猜测的数字比我选出的数字是大了还是小了。
你可以通过调用一个预先定义好的接口 int guess(int num) 来获取猜测结果，返回值一共有 3 种可能的情况（-1，1 或 0）：

-1：我选出的数字比你猜的数字小 pick < num
1：我选出的数字比你猜的数字大 pick > num
0：我选出的数字和你猜的数字一样。恭喜！你猜对了！pick == num
返回我选出的数字。

**示例 1：**

```
输入：n = 10, pick = 6
输出：6
```

**示例 2：**

```
输入：n = 1, pick = 1
输出：1
```

**示例 3：**

```
输入：n = 2, pick = 1
输出：1
```

**示例 4：**

```
输入：n = 2, pick = 2
输出：2
```

**提示：**

- `1 <= n <= 231 - 1`
- `1 <= pick <= n`

```c
class Solution {
public:
    int guessNumber(int n) {
        int l=1,r=n;
        while(l<r){
            int mid=(long long) l+r >>1;
            //答案比当前的小
            if(guess(mid)<=0) r=mid;
            else l=mid+1;
        }
        return r;
    }
};
```

```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int l=1,r=n,mid;
        while(l<r){
            mid=(int) ((long)l+r>>1);
            //当前的mid比答案大了
            if(guess(mid)<=0) r=mid;
            else l=mid+1;
        }
        return r;
    }
}
```

#### [367. 有效的完全平方数](https://leetcode.cn/problems/valid-perfect-square/)

给定一个 **正整数** `num` ，编写一个函数，如果 `num` 是一个完全平方数，则返回 `true` ，否则返回 `false` 。

**进阶：不要** 使用任何内置的库函数，如 `sqrt` 。

**示例 1：**

```
输入：num = 16
输出：true
```

**示例 2：**

```
输入：num = 14
输出：false
```

**提示：**

- `1 <= num <= 2^31 - 1`

```c
class Solution {
public:
    bool isPerfectSquare(int num) {
        int l=1,r=num;
        while(l<r){
            int mid=(1ll+l+r)>>1;
            if(mid<=num/mid) l=mid;
            else r=mid-1;
        }
        return r*r==num;
    }
};
```

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int l=1,r=num;
        while(l<r){
            int mid=(int)((long)(1+l+r)>>1);
            if(mid<=num/mid) l=mid;
            else r=mid-1;
        }
        if(r*r==num) return true;
        return false;
    }
}
```



#### [1539. 第 k 个缺失的正整数](https://leetcode.cn/problems/kth-missing-positive-number/)、

给你一个 **严格升序排列** 的正整数数组 `arr` 和一个整数 `k` 。

请你找到这个数组里第 `k` 个缺失的正整数。

**示例 1：**

```
输入：arr = [2,3,4,7,11], k = 5
输出：9
解释：缺失的正整数包括 [1,5,6,8,9,10,12,13,...] 。第 5 个缺失的正整数为 9 。
```

**示例 2：**

```
输入：arr = [1,2,3,4], k = 2
输出：6
解释：缺失的正整数包括 [5,6,7,...] 。第 2 个缺失的正整数为 6 。
```

**提示：**

$1 <= arr.length <= 1000$,
$1 <= arr[i] <= 1000$,
$1 <= k <= 1000$
对于所有$ 1 <= i < j <= arr.length $的 $i$ 和$ j$ 满足$ arr[i] < arr[j]$ 

```c
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {
        //当前面都是缺失的时候，直接返回K
        if(arr[0]>k) return k;

        int l=1,r=arr.size();
        while(l<r){
            int mid=l+r>>1;
            /*
            由递推式得到：
            arr[i]的数对应缺失的个数:arr[i]-i-1
            */
            if(arr[mid]-mid-1 >= k) r=mid;
            else l=mid+1;
        }
        //arr[r-1]-(r-1)-1 表示r-1位置缺失多少个数字
        return k-(arr[r-1]-(r-1)-1)+arr[r-1];
    }
};
```



#### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序排列  ，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果设这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。

以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。

你可以假设每个输入 只对应唯一的答案 ，而且你 不可以 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

提示：

$- 2 <= numbers.length <= 3 * 10^4$,
$-1000 <= numbers[i] <= 1000$,
$numbers$ 按 非递减顺序 排列
$-1000 <= target <= 1000$
仅存在一个有效答案

```c
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for(int i=0;i<numbers.size();i++){
            int x=numbers[i];
            int l=i,r=numbers.size()-1;
            while(l<r){
                int mid=1ll+l+r>>1;
                if(numbers[mid]<=(target-x)) l=mid;
                else r=mid-1;
            }
            if(numbers[r]==(target-x)) return {i+1,r+1};
        }
        return {-1,-1};
    }
};
```



#### [1351. 统计有序矩阵中的负数](https://leetcode.cn/problems/count-negative-numbers-in-a-sorted-matrix/)

给你一个 `m * n` 的矩阵 `grid`，矩阵中的元素无论是按行还是按列，都以非递增顺序排列。 请你统计并返回 `grid` 中 **负数** 的数目。

**示例 1：**

```
输入：grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]
输出：8
解释：矩阵中共有 8 个负数。
```

**示例 2：**

```
输入：grid = [[3,2],[1,0]]
输出：0
```

**提示：**

$m == grid.length$,
$n == grid[i].length$,
$1 <= m, n <= 100$,
$-100 <= grid[i][j] <= 100$

```c
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int res=0;
        for(auto g:grid){
            int l=0,r=g.size();
            while(l<r){
                int mid=(l+r)>>1;
                if(g[mid]<0) r=mid;
                else l=mid+1;
            }
            // cout<<"r:"<<r<<endl;
            if(r!=g.size() && g[r]<0) res+=(g.size()-r);
        }
        return res;
    }
};
```

#### [74. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/)

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

**示例 2：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/mat2.jpg)

```
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```

**提示：**

$m == matrix.length$,
$n == matrix[i].length$,
$1 <= m, n <= 100$,
$-10^4 <= matrix[i][j], target <= 10^4$

```c
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n=matrix.size(),m=matrix[0].size();
        int l=0,r=n*m-1;
        while(l<r){
            int mid=1ll+l+r>>1;
            if(matrix[mid/m][mid%m]<=target) l=mid;
            else r=mid-1;
        }
        if(matrix[r/m][r%m]==target) return true;
        return false;
    }
};
```

#### [1346. 检查整数及其两倍数是否存在](https://leetcode.cn/problems/check-if-n-and-its-double-exist/)

给你一个整数数组 arr，请你检查是否存在两个整数 N 和 M，满足 N 是 M 的两倍（即，N = 2 * M）。

更正式地，检查是否存在两个下标 i 和 j 满足：

- `i != j`

- `0 <= i, j < arr.length`

- `arr[i] == 2 * arr[j]`

**示例 1：**

```
输入：arr = [10,2,5,3]
输出：true
解释：N = 10 是 M = 5 的两倍，即 10 = 2 * 5 。
```

**示例 2：**

```
输入：arr = [7,1,14,11]
输出：true
解释：N = 14 是 M = 7 的两倍，即 14 = 2 * 7 。
```

**示例 3：**

```
输入：arr = [3,1,7,11]
输出：false
解释：在该情况下不存在 N 和 M 满足 N = 2 * M 。
```

**提示：**

- `2 <= arr.length <= 500`
- `-10^3 <= arr[i] <= 10^3`

```c
class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        int n=arr.size();
        sort(arr.begin(),arr.end());

        if(arr[0]==0 && arr[1]==0) return true;
        for(int i=0;i<n;i++){
            int x=arr[i];
            int l=i,r=n;
            while(l<r){
                int mid=l+r>>1;
                if((x>0 && arr[mid]/2>=x) || (x<0 && arr[mid]>=x/2)) r=mid;
                else l=mid+1;
            }

            if(r!=n && x>0 && arr[r]==2*x ) return true;
            
            if(r!=n && x<0 && arr[r]*2==x ) return true;
        }
        return false;
    }
};
```

**边界巨多，还是我没get到二分的精华！！！**



#### [1855. 下标对中的最大距离](https://leetcode.cn/problems/maximum-distance-between-a-pair-of-values/)

给你两个 非递增 的整数数组 nums1 和 nums2 ，数组下标均 从 0 开始 计数。

下标对 (i, j) 中 0 <= i < nums1.length 且 0 <= j < nums2.length 。如果该下标对同时满足 i <= j 且 nums1[i] <= nums2[j] ，则称之为 有效 下标对，该下标对的 距离 为 j - i 。

返回所有 有效 下标对 (i, j) 中的 最大距离 。如果不存在有效下标对，返回 0 。

一个数组 arr ，如果每个 1 <= i < arr.length 均有 arr[i-1] >= arr[i] 成立，那么该数组是一个 非递增 数组。

**示例 1：**

```
输入：nums1 = [55,30,5,4,2], nums2 = [100,20,10,10,5]
输出：2
解释：有效下标对是 (0,0), (2,2), (2,3), (2,4), (3,3), (3,4) 和 (4,4) 。
最大距离是 2 ，对应下标对 (2,4) 。
```

**示例 2：**

```
输入：nums1 = [2,2,2], nums2 = [10,10,1]
输出：1
解释：有效下标对是 (0,0), (0,1) 和 (1,1) 。
最大距离是 1 ，对应下标对 (0,1) 。
```

**示例 3：**

```
输入：nums1 = [30,29,19,5], nums2 = [25,25,25,25,25]
输出：2
解释：有效下标对是 (2,2), (2,3), (2,4), (3,3) 和 (3,4) 。
最大距离是 2 ，对应下标对 (2,4) 。
```

**提示：**

$1 <= nums1.length <= 10^5$,
$1 <= nums2.length <= 10^5$,
$1 <= nums1[i], nums2[j] <= 10^5$,
$nums1$ 和 $nums2$ 都是 非递增 数组

```c
class Solution {
public:
    int maxDistance(vector<int>& nums1, vector<int>& nums2) {
        int res=0;
        int n=nums1.size(),m=nums2.size();
        for(int i=0;i<n;i++){
            int x=nums1[i];
            int l=0,r=m-1;
            while(l<r){
                int mid=1ll+l+r>>1;
                if(nums2[mid]>=x) l=mid;
                else r=mid-1;  
            }
            // cout<<i<<" "<<r<<endl;
            if(x<=nums2[r]){
                res=max(res,r-i);
            }
        }
        return res;
    }
};
```

#### [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。

**示例 1：**

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

**示例 3：**

```
输入：nums = [1], target = 0
输出：-1
```

**提示：**

$1 <= nums.length <= 5000$,
$-10^4 <= nums[i] <= 10^4$
$nums$ 中的每个值都 独一无二
题目数据保证 $nums$ 在预先未知的某个下标上进行了旋转
$-10^4 <= target <= 10^4$

```c
class Solution {
public:
    int search(vector<int>& nums, int target) {
        //找出两断区间，前面升序，后面也升序  x>=nums[0]找出
        //确定答案区间0--size()-1
        if(nums.empty()) return -1;
        int l=0,r=nums.size()-1;
        while(l<r){
            int mid=l+r+1>>1;
            if(nums[mid]>=nums[0]) l=mid;
            else r=mid-1;
        }

        if(target>=nums[0]) l=0;
        else l=r+1,r=nums.size()-1;

        while(l<r){
            int mid=l+r>>1;
            if(nums[mid]>=target) r=mid;
            else l=mid+1;
        }

        if(nums[r]==target) return r;
        return -1;
    }
};
```

#### [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：
若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]
若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]
注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。

**示例 1：**

```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

**示例 2：**

```
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

**示例 3：**

```
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```

**提示：**

$n == nums.length$,
$1 <= n <= 5000$
$-5000 <= nums[i] <= 5000$
$nums$ 中的所有整数 互不相同
$nums $原来是一个升序排序的数组，并进行了 1 至 n 次旋转

```c
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n=nums.size();
        if(nums.size()==1) return nums[0];
        int l=0,r=n-1;
        while(l<r){
            int mid=(1ll+l+r)>>1;
            if(nums[mid]>=nums[0]) l=mid;
            else r=mid-1;
        }
        // cout<<r<<endl;
        if(r==n-1) return nums[0];
        return nums[r+1];
    }
};
```




### 2.二分找峰值

#### [852. 山脉数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/)

符合下列属性的数组 arr 称为 山脉数组 ：
arr.length >= 3
存在 i（0 < i < arr.length - 1）使得：
arr[0] < arr[1] < ... arr[i-1] < arr[i]
arr[i] > arr[i+1] > ... > arr[arr.length - 1]
给你由整数组成的山脉数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i 。

**示例 1：**

```
输入：arr = [0,1,0]
输出：1
```

**示例 2：**

```
输入：arr = [0,2,1,0]
输出：1
```

**示例 3：**

```
输入：arr = [0,10,5,2]
输出：1
```

**示例 4：**

```
输入：arr = [3,4,5,1]
输出：2
```

**示例 5：**

```
输入：arr = [24,69,100,99,79,78,67,36,26,19]
输出：2
```

**提示：**

- `3 <= arr.length <= 104`
- `0 <= arr[i] <= 106`
- 题目数据保证 `arr` 是一个山脉数组

```c
class Solution {
public:
    /*
    根据某种性质划分：
    在分界点的左边满足arr[i-1]<arr[i]
    在分界点的右边满足arr[i-1]>arr[i]
    */
    int peakIndexInMountainArray(vector<int>& arr) {
        int n=arr.size();
        int l=1,r=n-2;
        while(l<r){
            int mid=(l+r+1)>>1;
            //分界点在mid的右边
            if(arr[mid-1]<arr[mid]) l=mid;
            else r=mid-1;
        }
        return r;
    }
};
```

```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int l=1,r=arr.length-2;
        while(l<r){
            int mid=(l+r+1)>>1;
            if(arr[mid-1]<arr[mid]) l=mid;
            else r=mid-1;
        }
        return r;
    }
}
```





### 3.综合二分

#### (1)容斥原理+二分

#### [878. 第 N 个神奇数字](https://leetcode.cn/problems/nth-magical-number/)

一个正整数如果能被 a 或 b 整除，那么它是神奇的。

给定三个整数$ n , a , b $，返回第 n 个神奇的数字。因为答案可能很大，所以返回答案 对 $10^9 + 7$ 取模 后的值。

**示例 1：**

```
输入：n = 1, a = 2, b = 3
输出：2
```

**示例 2：**

```
输入：n = 4, a = 2, b = 3
输出：6
```

**提示：**

- `1 <= n <= 109`
- `2 <= a, b <= 4 * 104`

**c++代码：**

**二分+容斥原理+最大公约数**

```c
typedef long long LL;
class Solution {
public:

    //求最大公约数
    int gcd(int a,int b){
        return b ? gcd(b,a % b) : a;
    }

    //容斥原理计算n对应得有多少个能被a或者b整除得数
    LL get(LL n,int a,int b){
        return n/a + n/b - n/(a*b/gcd(a,b));
    }

    int nthMagicalNumber(int n, int a, int b) {
        const int MOD=1e9+7;
        //n数据范围1e9，a和b得数据范围4*1e4
        LL l=1,r=4e13;
        while(l<r){
            LL mid=l+r>>1;
            if(get(mid,a,b)>=n) r=mid;
            else l=mid+1;
        }
        return r % MOD;
    }
};
```



#### [1201. 丑数 III](https://leetcode.cn/problems/ugly-number-iii/)

给你四个整数：`n` 、`a` 、`b` 、`c` ，请你设计一个算法来找出第 `n` 个丑数。

丑数是可以被 `a` **或** `b` **或** `c` 整除的 **正整数** 。

**示例 1：**

```
输入：n = 3, a = 2, b = 3, c = 5
输出：4
解释：丑数序列为 2, 3, 4, 5, 6, 8, 9, 10... 其中第 3 个是 4。
```

**示例 2：**

```
输入：n = 4, a = 2, b = 3, c = 4
输出：6
解释：丑数序列为 2, 3, 4, 6, 8, 9, 10, 12... 其中第 4 个是 6。
```

**示例 3：**

```
输入：n = 5, a = 2, b = 11, c = 13
输出：10
解释：丑数序列为 2, 4, 6, 8, 10, 11, 12, 13... 其中第 5 个是 10。
```

**示例 4：**

```
输入：n = 1000000000, a = 2, b = 217983653, c = 336916467
输出：1999999984
```

**提示：**

- `1 <= n, a, b, c <= 10^9`
- `1 <= a * b * c <= 10^18`
- 本题结果在 `[1, 2 * 10^9]` 的范围内

**c++代码：**

```c
typedef long long LL;
class Solution {
public:
    //最大公约数
    int gcd(int a,int b){
        return b ? gcd(b,a%b): a;
    }

    //最小公倍数
    LL lcm(LL a,LL b){
        return a*b/gcd(a,b);
    }

    //容斥原理,通过容斥原理求出mid左边的所有的丑数个数，和需要的n做比较
    LL getTotal(LL mid,int a,int b,int c){
        return mid/a+mid/b+mid/c-mid/lcm(a,b)-mid/lcm(a,c)-mid/lcm(b,c)+mid/lcm(lcm(a,b),c);
    }
    int nthUglyNumber(int n, int a, int b, int c) {
        LL l=1,r=2e9;
        while(l<r){
            LL mid=(l+r)>>1;
            LL ans=getTotal(mid,a,b,c);
            //如果mid左边的答案已经够n了，则r=mid
            if(ans>=n) r=mid;
            else l=mid+1;
        }
        return r;
    }
};
```



## 二、DFS

### 1.DFS+剪枝

#### [698. 划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/)

给定一个整数数组 `nums` 和一个正整数 `k`，找出是否有可能把这个数组分成 `k` 个非空子集，其总和都相等。

**示例 1：**

```
输入： nums = [4, 3, 2, 3, 5, 2, 1], k = 4
输出： True
说明： 有可能将其分成 4 个子集（5），（1,4），（2,3），（2,3）等于总和。
```

**示例 2:**

```
输入: nums = [1,2,3,4], k = 3
输出: false
```

**提示：**

- `1 <= k <= len(nums) <= 16`

- `0 < nums[i] < 10000`

- 每个元素的频率在 `[1,4]` 范围内

```c
class Solution {
public:
    vector<int> stick;  //nums
    vector<bool> st;    //标记是否使用过
    int len;            //每一段是多长

    //start表示当前是拼第几根,cur为目前木棍的长度，k为还需要拼接木棍数目
    bool dfs(int start,int cur,int k){
        if(!k) return true; //k==0表示已经全部拼接完成
        if(cur==len) return dfs(0,0,k-1);   // 如果当前cur==len,表示这根已经拼接好了
        
        //否则枚举所有，看加入哪一根合法
        for(int i=start;i<stick.size();i++){
            if(st[i]) continue;    //  如果已经使用过，则直接跳过
            if(cur+stick[i]<=len){  //将stick[i]加到木棍里面
                //先标记为已经使用
                st[i]=true;
                if(dfs(i+1,cur+stick[i],k)) return true;
                st[i]=false;    //恢复现场
            }
            //剪枝，当有重复的长度的时候，选择哪一个都是相同的效果
            while(i+1<stick.size() && stick[i+1]==stick[i]) i++;
            //剪枝，如果尝试拼入第一根就失败则后面都失败；
            //如果当前拼好这根失败了，那么后面的剩余木棒也失败
            if(!cur || cur+stick[i]==len) return false;
        }
        return false;
    }

    bool canPartitionKSubsets(vector<int>& nums, int k) {
        stick=nums;
        st.resize(nums.size());
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if(sum%k) return false;
        len=sum/k;
        sort(stick.begin(),stick.end(), greater<int>());

        return dfs(0,0,k);
    }
};
```

#### [473. 火柴拼正方形](https://leetcode.cn/problems/matchsticks-to-square/)

你将得到一个整数数组 matchsticks ，其中 matchsticks[i] 是第 i 个火柴棒的长度。你要用 所有的火柴棍 拼成一个正方形。你 不能折断 任何一根火柴棒，但你可以把它们连在一起，而且每根火柴棒必须 使用一次 。

如果你能使这个正方形，则返回 true ，否则返回 false 。

**示例 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/matchsticks1-grid.jpg)

```
输入: matchsticks = [1,1,2,2,2]
输出: true
解释: 能拼成一个边长为2的正方形，每边两根火柴。
```

**示例 2:**

```
输入: matchsticks = [3,3,3,3,4]
输出: false
解释: 不能用所有火柴拼成一个正方形。
```

**提示:**

- `1 <= matchsticks.length <= 15`
- `1 <= matchsticks[i] <= 108`

```c
class Solution {
public:

    vector<int> stick;  //  全局木棒
    vector<bool> st;    //标记木棒是否已经使用
    int len;            //均分之后的边长

    /*
    start:表示已经拼好的木棒
    cur:表示当前这根木棒的长度
    k:表示还有几根需要拼
    */
    bool dfs(int start,int cur,int k){
        if(!k) return true; // 如果拼好4则结束
        if(cur==len) return dfs(0,0,k-1);       //如果这根拼好了，则递归到下一根
        for(int i=start;i<stick.size();i++){
            if(st[i]) continue; //  如果已经使用则直接跳过
            //如果当前段可以加进来
            if(cur+stick[i]<=len){
                st[i]=true; //  标记已经使用了
                if(dfs(i+1,cur+stick[i],k)) return true;    //递归到下一层
                st[i]=false;   //恢复现场
            }
            //剪枝,如果遇到相同长度的，则相同的任意选一个就可以，效果都是一样的
            if(i+1<stick.size() && stick[i]==stick[i+1]) i++;
            /*
            剪枝：
            如果上面的都不满足了
            （1）当第一个位置的时候就失败了，则剪枝，后面不需要在继续了
            （2）当前根如果不合理了，则后面还没有拼的也不需要继续了
            */
            if(!cur || cur+stick[i]==len) return false;
        }
        return false;
    }

    bool makesquare(vector<int>& matchsticks) {
        /*
        1.从大到小枚举
        2.每条边内部，要求火柴编号递增
        3.若当前放某根火柴失败
         1）跳过长度相同的火柴
         2）是第一根火柴，则剪纸为当前分支
         3）是最后一根，则剪枝当前分支
        */
        stick=matchsticks;
        st.resize(stick.size());
        int sum=0;
        for(auto x:stick) sum+=x;
        //如果不能均分则false
        if(sum % 4) return false;

        len=sum / 4;
        //从大到小枚举，优化搜索顺序
        sort(stick.begin(),stick.end(),greater<int>());
        return dfs(0,0,4);
    }
};
```

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`
- 暴力搜索TLE了，优化写法，01背包问题

```c
class Solution {
public:
    vector<int> nums;
    vector<bool> st;
    int len;

    bool dfs(int start,int cur,int k){
        if(!k) return true;
        if(cur==len) return dfs(0,0,k-1);
        for(int i=start;i<nums.size();i++){
            if(st[i]) continue;
            if(cur+nums[i]<=len){
                st[i]=true;
                if(dfs(i+1,cur+nums[i],k)) return true;
                st[i]=false;
            }
            if(i+1 < nums.size() && nums[i]==nums[i+1]) i++;
            if(!cur || cur+nums[i]==len) return false;
        }
        return false;
    }

    bool canPartition(vector<int>& _nums) {
        nums=_nums;
        int sum=0;
        st.resize(nums.size());
        for(auto x:nums) sum+=x;

        if(sum % 2) return false;
        len=sum /2;
        sort(nums.begin(),nums.end(),greater<int>());
        return dfs(0,0,2);
    }
};
```

**01背包**


```c
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n=nums.size();
        int sum=0;
        for(auto x:nums) sum+=x;
        if(sum % 2) return false;
        sum/=2;
        vector<int> f(sum+1);   // 背包
        f[0]=1; //当值为0表示false没有，当1时候表示有
        for(auto x:nums){
            for(int j=sum;j>=x;j--){
                f[j]|=f[j-x];
            }
        }
        return f[sum];
    }
};
```

#### [301. 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)todo



#### [996. 正方形数组的数目](https://leetcode.cn/problems/number-of-squareful-arrays/)todo

**题解：**

```c
此题可以理解成一张图的路径搜索问题。我们提前处理所有的pair，对于相加是平方数的两个元素，我们就认为它们之间有一条边。我们的任务就是找出所有的路径，能够走遍所有的点。

考虑到题目需要穷举所有的路径，并且数据规模异常得小，大概率这是一个NP问题，可以用暴力的深度搜索，和473，698都很相似。这类题目都需要用visited来记录已经访问过的元素来避免重复。

特别注意的是，本题要求避免“长得一样”的重复路径，需要有剪支的操作。比如对于数列1,2,2,2,3,4,我们想取不重复的permutation。当我们考察完1,2,X,X,X,X之后，需要回溯考虑第二个元素的其他候选。我们发现，如果第二个位置再选其他的“2”，就会又完全重复之前的搜索。尽管是两个不同的“2”，但这样的两条路径被认为是重复。为了剪枝，我们在从cur开始寻找下一层深度的节点时，可以将所有的候选节点事先排个序，如果候选节点B和它之前考察过的候选节点A相同，那么我们就略过对候选节点B的考察。

另外一个细节就是如何判断一个数是否是平方数？正确的做法是if (sqrt(x)==(int)sqrt(x))
```



###  2.DFS+回溯

[推荐连接](https://leetcode.cn/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/)

#### (1)状态转移理解

#### [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)

数字 `n` 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合。

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

**示例 2：**

```
输入：n = 1
输出：["()"]
```

**提示：**

- `1 <= n <= 8`

**c++代码：**

```c
class Solution {
public:
    /*
        括号匹配问题：
        1.前缀集合中左括号数>右括号数
        2.左括号数==右括号数
    
        本题：
        当填充当前位置的括号时
        1.当lcount<n,当前位置'('
        2.当rcout<n && lcout>rcount,当前位置')'
    */
    vector<string> res;
    vector<string> generateParenthesis(int n) {
        if(!n) return res;
        dfs(n,0,0,"");
        return res;
    }

    //lc表示当前的左括号的数目，rc表示当前的右括号的数量
    void dfs(int n,int lc,int rc,string path){
        if(n==lc && rc==n){
            res.push_back(path);
        }else{
            if(lc<n) dfs(n,lc+1,rc,path+'(');
            if(rc<n && lc>rc) dfs(n,lc,rc+1,path+')');
        }
    }
};
```

**java代码：**

```java
class Solution {
    List<String> res=new ArrayList<>();

    public List<String> generateParenthesis(int n) {
        dfs(n,0,0,"");
        return res;
    }
    public void dfs(int n,int lc,int rc,String path){
        if(n==lc && n==rc) res.add(path);
        else{
            if(lc<n) dfs(n,lc+1,rc,path+"(");
            if(rc<n && lc>rc) dfs(n,lc,rc+1,path+")");
        }
    }
}
```

**py代码:**

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        res=[]
        self.dfs(res,n,0,0,"");
        return res
    def dfs(self,res,n,lc,rc,path):
        if lc > n or rc > n or rc > lc:
            return
        if n==lc and n==rc:
            res.append(path)
        self.dfs(res,n,lc+1,rc,path+"(")
        self.dfs(res,n,lc,rc+1,path+")")
```



#### [36. 有效的数独](https://leetcode.cn/problems/valid-sudoku/)

请你判断一个 9 x 9 的数独是否有效。只需要 根据以下规则 ，验证已经填入的数字是否有效即可。

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。（请参考示例图）

**注意：**

- 一个有效的数独（部分已被填充）不一定是可解的。
- 只需要根据以上规则，验证已经填入的数字是否有效即可。
- 空白格用 `'.'` 表示。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714svg.png)

```
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
```

**示例 2：**

```
输入：board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：false
解释：除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。 但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**提示：**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` 是一位数字（`1-9`）或者 `'.'`

```c
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        bool st[9];

        // 判断行
        for (int i = 0; i < 9; i ++ ) {
            memset(st, 0, sizeof st);
            for (int j = 0; j < 9; j ++ ) {
                if (board[i][j] != '.') {
                    int t = board[i][j] - '1';
                    if (st[t]) return false;
                    st[t] = true;
                }
            }
        }

        // 判断列
        for (int i = 0; i < 9; i ++ ) {
            memset(st, 0, sizeof st);
            for (int j = 0; j < 9; j ++ ) {
                if (board[j][i] != '.') {
                    int t = board[j][i] - '1';
                    if (st[t]) return false;
                    st[t] = true;
                }
            }
        }

        // 判断小方格
        for (int i = 0; i < 9; i += 3)
            for (int j = 0; j < 9; j += 3) {
                memset(st, 0, sizeof st);
                for (int x = 0; x < 3; x ++ )
                    for (int y = 0; y < 3; y ++ ) {
                        if (board[i + x][j + y] != '.') {
                            int t = board[i + x][j + y] - '1';
                            if (st[t]) return false;
                            st[t] = true;
                        }
                    }
            }

        return true;
    }
};
```

#### [37. 解数独](https://leetcode.cn/problems/sudoku-solver/)

编写一个程序，通过填充空格来解决数独问题。

数独的解法需 遵循如下规则：

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。（请参考示例图）
数独部分空格内已填入了数字，空白格用 '.' 表示。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714svg.png)

```
输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：
```

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714_solutionsvg.png)



**提示：**

- $board.length == 9$
- $board[i].length == 9$
- $board[i][j]$ 是一位数字或者$ '.'$
- 题目数据 保证 输入数独仅有一个解

**c++代码实现：**

```c
class Solution {
public:

    //每一个格子可以填0-9的数字
    bool row[9][9],col[9][9],cell[3][3][9];

    void solveSudoku(vector<vector<char>>& board) {
        memset(row,0,sizeof row);
        memset(col,0,sizeof col);
        memset(cell,0,sizeof cell);

        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j]!='.'){
                    //将当前位置的数标记为已经使用,需要减去1是将char映射为数组下标
                    int t=board[i][j]-'1';
                    row[i][t]=col[j][t]=cell[i/3][j/3][t]=true;
                }
            }
        }
        dfs(board,0,0);
    }

    bool dfs(vector<vector<char>>& board,int x,int y){
        if(y==9) x++,y=0;   //当y到达每一行的最后一个位置，更新到下一行第一个位置
        if(x==9) return true;   // 全部都填满了则返回

        // 满足则递归填充
        if(board[x][y]!='.') return dfs(board,x,y+1);

        //否则则填充当前位置
        for(int i=0;i<9;i++){
            //如果当前的数还没有使用过，则使用
            if(!row[x][i] && !col[y][i] && !cell[x/3][y/3][i]){
                row[x][i]=col[y][i]=cell[x/3][y/3][i]=true;
                board[x][y]='1'+i;		
                if(dfs(board,x,y+1)) return true;
                //恢复现场
                row[x][i]=col[y][i]=cell[x/3][y/3][i]=false;
                board[x][y]='.';
            }
        }
        return false;
    }
};
```

**java代码：**

```java
class Solution {
public:

    //每一个格子可以填0-9的数字
    bool row[9][9],col[9][9],cell[3][3][9];

    void solveSudoku(vector<vector<char>>& board) {
        memset(row,0,sizeof row);
        memset(col,0,sizeof col);
        memset(cell,0,sizeof cell);

        for(int i=0;i<9;i++){
            for(int j=0;j<9;j++){
                if(board[i][j]!='.'){
                    //将当前位置的数标记为已经使用
                    int t=board[i][j]-'1';
                    row[i][t]=col[j][t]=cell[i/3][j/3][t]=true;
                }
            }
        }
        dfs(board,0,0);
    }

    bool dfs(vector<vector<char>>& board,int x,int y){
        if(y==9) x++,y=0;   //当y到达每一行的最后一个位置，更新到下一行第一个位置
        if(x==9) return true;   // 全部都填满了则返回

        // 满足则递归填充
        if(board[x][y]!='.') return dfs(board,x,y+1);

        //否则则填充当前位置
        for(int i=0;i<9;i++){
            //如果当前的数还没有使用过，则使用
            if(!row[x][i] && !col[y][i] && !cell[x/3][y/3][i]){
                row[x][i]=col[y][i]=cell[x/3][y/3][i]=true;
                board[x][y]='1'+i;
                if(dfs(board,x,y+1)) return true;
                //恢复现场
                row[x][i]=col[y][i]=cell[x/3][y/3][i]=false;
                board[x][y]='.';
            }
        }
        return false;
    }
};
```

#### [79. 单词搜索](https://leetcode.cn/problems/word-search/)

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
输出：true
```

**示例 3：**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
输出：false
```

**提示：**

$m == board.length$,
$n = board[i].length$,
$1 <= m, n <= 6$,
$1 <= word.length <= 15$,
$board$ 和 $word $仅由大小写英文字母组成

**c++代码实现：**

```c
class Solution {
public:
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};
    int n,m;
    bool exist(vector<vector<char>>& board, string word) {
        n=board.size(),m=board[0].size();
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                if(dfs(board,word,0,i,j))
                    return true;
            }
        }
        return false;
    }

    //u表示当前word到哪个位置了，x,y表示board的位置
    bool dfs(vector<vector<char>>& board,string& word,int u,int x,int y){
        //数据范围都是大于1的，所以不需要特判
        //如果当前搜索到的路径位置的字符和word的字符不相同，则不满足
        if(word[u]!=board[x][y]) return false;
        //如果搜到了最后了，找到了答案
        if(u==word.size()-1) return true;

        //记录当前位置的值
        char t=board[x][y];
        board[x][y]='*';
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            //判断是否越界了，越界则直接跳过
            if(a<0 || a>=n || b<0 || b>=m || board[a][b]=='*') continue;
            //没越界则递归判断
            if(dfs(board,word,u+1,a,b)) return true;
        }
        //恢复现场
        board[x][y]=t;
        return false;
    }

};
```

#### [1079. 活字印刷](https://leetcode.cn/problems/letter-tile-possibilities/)

你有一套活字字模 tiles，其中每个字模上都刻有一个字母 tiles[i]。返回你可以印出的非空字母序列的数目。

注意：本题中，每个活字字模只能使用一次。

 **示例 1：**

```
输入："AAB"
输出：8
解释：可能的序列为 "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA"。
```

**示例 2：**

```
输入："AAABBC"
输出：188
```

**示例 3：**

```
输入："V"
输出：1
```

**提示：**

- `1 <= tiles.length <= 7`
- `tiles` 由大写英文字母组成

**c++代码实现：**

```c
class Solution {
public:
    
    int dfs(string& s,int u){
        int res=1; //每次都至少有一个
        if(u==s.size()) return res;
        for(int i=0;i<s.size();i++){
            //如果相同的数的第一个已经使用了，则直接跳过
            if(i && s[i]==s[i-1]) continue;
            //已经使用则直接跳过
            if(s[i]=='.') continue;
            char c=s[i];
            s[i]='.';
            res+=dfs(s,u+1);
            s[i]=c; //回溯
        }
        return res;
    }

    int numTilePossibilities(string tiles) {
        //排序去重复
        sort(tiles.begin(),tiles.end());
        //减去空字符串
        return dfs(tiles,0)-1;
    }
};
```



#### [282. 给表达式添加运算符](https://leetcode.cn/problems/expression-add-operators/)todo



#### [301. 删除无效的括号](https://leetcode.cn/problems/remove-invalid-parentheses/)todo



#### (2)组合

#### [39. 组合总和](https://leetcode.cn/problems/combination-sum/)

给你一个 无重复元素 的整数数组 candidates 和一个目标整数 target ，找出 candidates 中可以使数字和为目标数 target 的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。

candidates 中的 **同一个 数字可以 无限制重复被选取** 。如果**至少一个数字**的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 target 的不同组合数少于 150 个。

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

**提示：**

$1 <= candidates.length <= 30$
$1 <= candidates[i] <= 200$
$candidate 中的每个元素都 互不相同$
$1 <= target <= 500$

**c++代码：**

```c
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        dfs(candidates,0,target);
        return res;
    }

    // u:表示当前搜到的位置    target表示当前还有多少没有凑出来
    void dfs(vector<int>& candidates,int u,int target){
        if(target==0){
            //凑满了则直接加入答案
            res.push_back(path);
            return;
        }
        //到最后一个数了还是没凑出来提前return
        if(u==candidates.size()) return;
        
        //尝试u位置的数
        for(int i=0;i*candidates[u]<=target;i++){
            dfs(candidates,u+1,target-i*candidates[u]);
            path.push_back(candidates[u]);
        }
        //恢复现场
        for(int i=0;i*candidates[u]<=target;i++){
            path.pop_back();
        }
    }
};
```

**java代码：**

```java
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    List<Integer> path=new ArrayList<>();;

    public void dfs(int[] candidates,int u,int target){
        if(target==0){
            res.add(new ArrayList<>(path));
            return;
        }

        if(u==candidates.length) return;

        for(int i=0;i*candidates[u]<=target;i++){
            dfs(candidates,u+1,target-i*candidates[u]);
            path.add(candidates[u]);
        }

        for(int i=0;i*candidates[u]<=target;i++){
            path.remove(path.size()-1);
        }
    }

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        dfs(candidates,0,target);
        return res;
    }
}
```

#### [40. 组合总和 II](https://leetcode.cn/problems/combination-sum-ii/)(怎么考虑去重)

给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的**每个数字在每个组合中只能使用 一次** 。

注意：**解集不能包含重复的组合。** 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```

**提示:**

- `1 <= candidates.length <= 100`
- `1 <= candidates[i] <= 50`
- `1 <= target <= 30`

**c++代码实现：**

```	c
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    void dfs(vector<int>& c,int u,int target){
        if(target==0){
            res.push_back(path);
            return;
        }
        if(u==c.size()) return;
		
        //强调一下，树层去重的话，需要对数组排序！
        // 统计相同的元素的个数
        int k=u+1;
        while(k<c.size() && c[k]==c[u]) k++;
        int cnt=k-u;    // 个数

        //当相同的时候选择，个数需要不超过
        for(int i=0;i*c[u]<=target && i<=cnt;i++){
            dfs(c,k,target-i*c[u]);
            path.push_back(c[u]);
        }

        for(int i=0;i*c[u]<=target && i<=cnt;i++){
            path.pop_back();
        }

    }

    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        //强调一下，树层去重的话，需要对数组排序！
        //先排序从小到大
        sort(candidates.begin(),candidates.end());
        dfs(candidates,0,target);
        return res;
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<Integer>> res=new ArrayList<>();
    List<Integer> path=new ArrayList<>();

    public void dfs(int[] c,int u,int target){
        if(target==0){
            res.add(new ArrayList<>(path));
            return;
        }
        if(u==c.length) return;

        int k=u+1;
        while(k<c.length && c[k]==c[u]) k++;
        int cnt=k-u;

        for(int i=0;i*c[u]<=target && i<=cnt;i++){
            dfs(c,k,target-i*c[u]);
            path.add(c[u]);
        }

        for(int i=0;i*c[u]<=target && i<=cnt;i++){
            path.remove(path.size()-1);
        }
    }

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        dfs(candidates,0,target);
        return res;
    }
}
```

#### [216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：

只使用数字1到9
**每个数字 最多使用一次** 
返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
解释:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
没有其他符合的组合了。
```

**示例 3:**

```
输入: k = 4, n = 1
输出: []
解释: 不存在有效的组合。
在[1,9]范围内使用4个不同的数字，我们可以得到的最小和是1+2+3+4 = 10，因为10 > 1，没有有效的组合。
```

**提示:**

- `2 <= k <= 9`
- `1 <= n <= 60`

**c++代码实现：**

```c
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    void dfs(int k,int n,int start){
        if(k==path.size() && n==0){
            res.push_back(path);
            return;
        }
        for(int i=start;i<=9;i++){
            path.push_back(i);
            if(n-i>=0)
                dfs(k,n-i,i+1);
            path.pop_back();
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {
        dfs(k,n,1);
        return res;
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<Integer>> res=new ArrayList<>();
    List<Integer> path=new ArrayList<>();

    public void dfs(int k,int n,int start){
        if(n==0 && path.size()==k){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i=start;i<=9;i++){
            path.add(i);
            if(n-i>=0){
                dfs(k,n-i,i+1);
            }
            path.remove(path.size()-1);
        }
    }

    public List<List<Integer>> combinationSum3(int k, int n) {
        dfs(k,n,1);
        return res;
    }
}
```



#### [77. 组合](https://leetcode.cn/problems/combinations/)

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2：**

```
输入：n = 1, k = 1
输出：[[1]]
```

**提示：**

- `1 <= n <= 20`
- `1 <= k <= n`

**c++代码实现：**

```c
class Solution {
public:
    //记录答案
    vector<vector<int>> res;
    //记录每一层
    vector<int> path;

    vector<vector<int>> combine(int n, int k) {
        dfs(n,k,1);
        return res;
    }

    //n表示1-n的数随意选择，k表示还有几个数需要组合，start表示从哪个位置的数开始组合
    void dfs(int n,int k,int start){
        //如果k个数都组合完了则将当前的path加入
        if(!k){
            res.push_back(path);
            return;
        }
        for(int i=start;i<=n;i++){
            path.push_back(i);
            dfs(n,k-1,i+1);
            //恢复现场
            path.pop_back();
        }
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<Integer>> res=new ArrayList<>();
    List<Integer> path=new ArrayList<>();

    public void dfs(int n,int k,int start){
        if(path.size()==k){
            res.add(new ArrayList(path));
            return;
        }
        for(int i=start;i<=n-(k-path.size())+1;i++){
            path.add(i);
            dfs(n,k,i+1);
            path.remove(path.size()-1);
        }
    }

    public List<List<Integer>> combine(int n, int k) {
        dfs(n,k,1);
        return res;
    }
}
```

#### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)（DP）

给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

**示例 1：**

```
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

**示例 2：**

```
输入：nums = [9], target = 3
输出：0
```

提示：

$1 <= nums.length <= 200$,
$1 <= nums[i] <= 1000$,
$nums $中的所有元素 互不相同
$1 <= target <= 1000$

**进阶：**如果给定的数组中含有负数会发生什么？问题会产生何种变化？如果允许负数出现，需要向题目中添加哪些限制条件？

**c++代码：**

```
1、每个数可以使用无限次（完全背包）
2、顺序不同方案不同（则不能用背包）

DP
1、状态表示:
	（1）集合：f[i][j]表示前i个位置，总和为j的所有方案
	（2）属性：数量
2、集合划分
将nums的一个元素nums[j]放到第i个位置，则f中前i-1个位置随便放，第i个位置放nums[j]=k
则前i-1个位置总和就是j-k,方案数就是f[i-1][j-k]

优化DP，把第一位去了
1、状态表示:
	（1）集合：f[j]表示总和为j的所有方案
	（2）属性：数量
2、集合划分
将nums的a1,a2,....ak,an一个元素ak放到第i个位置，则f中前i-1个位置随便放，此时需要j-ak的总和
则前i-1个位置总和就是j-k,方案数就是f[j-k]
需要按照从小到大的顺序枚举，前面的状态影响后面的状态
```

```c
class Solution {
public:
    int combinationSum4(vector<int>& nums, int m) {
        vector<unsigned> f(m+1);
        f[0]=1;
        for(int i=0;i<=m;i++){
            for(auto j:nums){
                if(i>=j)
                    f[i]+=f[i-j];
            }
        }
        return f[m];
    }
};
```

#### (3)多个集合求组合

#### [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

**提示：**

- `0 <= digits.length <= 4`
- `digits[i]` 是范围 `['2', '9']` 的一个数字。



**c代码：**

```c
class Solution {
public:
    string str[10]={
        "","","abc","def",
        "ghi","jkl","mno",
        "pqrs","tuv","wxyz"
    };
    vector<string> res;

    vector<string> letterCombinations(string digits) {
        if(digits.empty()) return res;
        dfs(0,digits,"");
        return res;
    }

    void dfs(int cur,string& digits,string path){
        if(cur==digits.size()){
            res.push_back(path);
            return;
        }else{
            for(auto s:str[digits[cur]-'0']){
                dfs(cur+1,digits,path+s);
            }
        }
    }
};
```

**java代码：**

```java
class Solution {

    List<String> res=new ArrayList<>();
    String[] str={
        "","","abc","def",
        "ghi","jkl","mno",
        "pqrs","tuv","wxyz"
    };

    public List<String> letterCombinations(String digits) {
        if(digits.length()==0) return res;
        dfs(0,digits,"");
        return res;
    }

    public void dfs(int u,String digits,String path){
        if(u==digits.length()){
            res.add(path);
            return;
        }

        String s=str[digits.charAt(u)-'0'];
        for(int i=0;i<s.length();i++){
            dfs(u+1,digits,path+s.charAt(i));
        }
    }
}
```



#### (4)排列

#### [46. 全排列](https://leetcode.cn/problems/permutations/)（不含重复元素）

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

**c++代码实现：**

```c
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<bool> st;

    void dfs(vector<int>& nums,int u){
        if(u==nums.size()){
            res.push_back(path);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(!st[i]){
                st[i]=true;
                path.push_back(nums[i]);
                dfs(nums,u+1);
                path.pop_back();
                st[i]=false;
            }
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        int n=nums.size();
        st.resize(n,false);

        dfs(nums,0);
        return res;
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<Integer>> res=new ArrayList<>();
    List<Integer> path=new ArrayList<>();
    int[] st;

    public void dfs(int[] nums,int u){
        if(u==nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(st[i]==0){
                st[i]=1;
                path.add(nums[i]);
                dfs(nums,u+1);
                st[i]=0;
                path.remove(path.size()-1);
            }
        }
    }

    public List<List<Integer>> permute(int[] nums) {
        int n=nums.length;
        st=new int[n];
        Arrays.fill(st,0);
        dfs(nums,0);
        return res;
    }
}
```

#### [47. 全排列 II](https://leetcode.cn/problems/permutations-ii/)（包含重复元素）

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

**c++代码:**

```c
class Solution {
public:

    vector<vector<int>> res;
    vector<int> path;
    vector<bool> st;
    
    void dfs(vector<int>& nums,int u){
        if(u==nums.size()){
            res.push_back(path);
            return;
        }
        for(int i=0;i<nums.size();i++){
            if(!st[i]){
                //按照顺序遍历选择当前位置填什么数
                //如果满足i>1的位置，和前面nums[i-1]是相同的，并且i-1也没有用过，则不能选i位置的数
                if(i && nums[i-1]==nums[i] && !st[i-1]) continue;
                st[i]=true;
                path.push_back(nums[i]);
                dfs(nums,u+1);
                //恢复现场之后可能的到的i位置前面的位置是false
                st[i]=false;
                path.pop_back();
            }
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        //排序来起到去重的作用
        sort(nums.begin(),nums.end());

        int n=nums.size();
        st.resize(n,false);
        dfs(nums,0);
        return res;
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<Integer>> res=new ArrayList<>();
    List<Integer> path=new ArrayList<>();
    int[] st;

    public void dfs(int[] nums,int u){
        if(u==nums.length){
            res.add(new ArrayList<>(path));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(st[i]==0){
                if(i!=0 && nums[i-1]==nums[i] && st[i-1]==0) continue;
                st[i]=1;
                path.add(nums[i]);
                dfs(nums,u+1);
                st[i]=0;
                path.remove(path.size()-1);
            }
        }
    }

    public List<List<Integer>> permuteUnique(int[] nums) {
        int n = nums.length;
        st=new int[n];
        Arrays.fill(st,0);
        Arrays.sort(nums);
        dfs(nums,0);
        return res;
    }
}
```

#### [剑指 Offer 38. 字符串的排列](https://leetcode.cn/problems/zi-fu-chuan-de-pai-lie-lcof/)(包含重复元素)

输入一个字符串，打印出该字符串中字符的所有排列。

你可以以任意顺序返回这个字符串数组，但里面**不能有重复元素**。

**示例:**

```
输入：s = "abc"
输出：["abc","acb","bac","bca","cab","cba"]
```

**限制：**

```
1 <= s 的长度 <= 8
```

**c++代码实现：**

```c
class Solution {
public:
    vector<string> res;
    vector<bool> st;
    vector<string> permutation(string s) {
        int n=s.size();
        st.resize(n);
        sort(s.begin(),s.end());//排序来起到去重的作用
        dfs(s,0,"");
        return res;
    }
    void dfs(string& s,int u,string path){
        if(path.size()==s.size()){
            res.push_back(path);
            return;
        }
        for(int i=0;i<s.size();i++){
            if(!st[i]){
                //按照顺序遍历选择当前位置填什么数
                //如果满足i>1的位置，和前面nums[i-1]是相同的
                //并且i-1也没有用过，则不能选i位置的数
                if(i && s[i-1]==s[i] && !st[i-1]) continue;
                st[i]=true;
                string tep=path;
                path+=s[i];
                dfs(s,u+1,path);
                path=tep;
                st[i]=false;
            }
        }
    }
};
```



#### (5)皇后问题

#### [51. N 皇后](https://leetcode.cn/problems/n-queens/)

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：[["Q"]]
```

**提示：**

- `1 <= n <= 9`

**c++代码实现：**

```c
class Solution {
public:
    /*
    填充皇后的时候按照一行一行的填充
    */
    int n;  //全局
    vector<bool> col,ug,udg;    //col表示列的个数，ug表示主对角线的数量，uug为副对角线
    vector<vector<string>> res; //答案
    vector<string> path;    //没一行的答案

    //第u行的皇后怎么安放
    void dfs(int u){
        if(u==n){
            //放到最后一行了，则加入答案数组
            res.push_back(path);
            return;
        }
        //否则则看看皇后放在u行的哪个位置
        for(int i=0;i<n;i++){
            //如果列没有冲突 && 主对角线没有冲突 && 副对角线没有冲突 则填充
            if(!col[i] && !ug[u-i+n] && !udg[u+i]){
                col[i]=ug[u-i+n]=udg[u+i]=true;
                path[u][i]='Q';
                dfs(u+1);
                //恢复现场
                col[i]=ug[u-i+n]=udg[u+i]=false;
                path[u][i]='.';
            }
        }

    }

    vector<vector<string>> solveNQueens(int N) {
        n=N;
        col.resize(n);
        ug.resize(2*n);
        udg.resize(2*n);
        path = vector<string>(n, string(n, '.'));
        dfs(0);
        return res;
    }
};
```

#### [52. N皇后 II](https://leetcode.cn/problems/n-queens-ii/)

n 皇后问题 研究的是如何将 n 个皇后放置在 n × n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回 n 皇后问题 不同的解决方案的数量。

 **示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例 2：**

```
输入：n = 1
输出：1
```

**提示：**

- `1 <= n <= 9`

**c++代码实现：**

```c
class Solution {
public:
    int n;  //全局
    vector<bool> col,dg,udg; //记录u行对应的列和主对角线以及副对角线是否冲突
    
    //第u行填充
    int dfs(int u){
        //如果填到了n行，越界了，则说明填好了
        if(u==n) return 1;

        int res = 0;
        //否则填第u行
        for(int i=0;i<n;i++){
            //看看第u行对应的列，以及主对角线和副对角线是否冲突
            if(!col[i] && !dg[u-i+n] && !udg[u+i]){
                col[i]=dg[u-i+n]=udg[u+i]=true;
                res+=dfs(u+1);
                col[i]=dg[u-i+n]=udg[u+i]=false;
            }
        }
        return res;
    }

    int totalNQueens(int N) {
        n=N;
        col.resize(n);
        dg.resize(n*2);
        udg.resize(n*2);
        return dfs(0);
    }
};
```

#### (6)子集问题

#### [78. 子集](https://leetcode.cn/problems/subsets/)

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

**C++代码实现：**

```c
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    void dfs(vector<int>& nums,int u){
        res.push_back(path);    // 收集子集，要放在终止添加的上面，否则会漏掉自己
        if(u>=nums.size()) return; //u已经大于数组的长度了，就终止了，因为没有元素可取了
        for(int i=u;i<nums.size();i++){
            path.push_back(nums[i]);
            dfs(nums,i+1);
            path.pop_back();
        }
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        int n=nums.size();
        dfs(nums,0);
        return res;
    }
};
```

**迭代写法：**

```c
class Solution {
public:
    /*
    迭代写法：
    (集合的二进制表示) O(2^n*n)
    每个数表示一个子集，假设这个数的二进制表示的第 i 位是1，则表示该子集包含第 i 个数，否则表示不包含。
    3   2^3-1个
    000
    001
    010
    011
    100
    101
    111
    */
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> res;
        int n=nums.size();
        for(int i=0;i<1<<n;i++){
            vector<int> path;
            for(int j=0;j<n;j++){
                if(i>>j&1)  //最低为为1则加入
                    path.push_back(nums[j]);
            }
            res.push_back(path);
        }
        return res;
    }
};
```

```c++
class Solution {
private:
    int n;
    vector<vector<int>> res;
    vector<int> path;

public:
    
    void dfs(vector<int>& nums,int u){
        if(u==n){
            res.push_back(path);
            return;
        }
        //选择u位置
        path.push_back(nums[u]);
        dfs(nums,u+1);
        path.pop_back();
        //不选择u位置
        dfs(nums,u+1);
    }

    vector<vector<int>> subsets(vector<int>& nums) {
        n = nums.size();
        dfs(nums,0);
        return res;        
    }
};
```

#### [784. 字母大小写全排列](https://leetcode.cn/problems/letter-case-permutation/)

给定一个字符串 `s` ，通过将字符串 `s` 中的每个字母转变大小写，我们可以获得一个新的字符串。

返回 *所有可能得到的字符串集合* 。以 **任意顺序** 返回输出。

**示例 1：**

```
输入：s = "a1b2"
输出：["a1b2", "a1B2", "A1b2", "A1B2"]
```

**示例 2:**

```
输入: s = "3z4"
输出: ["3z4","3Z4"]
```

**提示:**

- `1 <= s.length <= 12`
- `s` 由小写英文字母、大写英文字母和数字组成

**c++代码实现：**

```c
注意:
1、大小写字母转化^=32就可以
2、java代码实现的时候，需要注意转化为字符数组的时候，使用索引获取的值不能被修改
```

```c
class Solution {
public:
    vector<string> res;
    vector<string> letterCasePermutation(string s) {
        dfs(s,0);
        return res;
    }

    void dfs(string& s,int u){
        if(u==s.size()){
            res.push_back(s);
        }else{
            //不管是数字还是字母都先遍历下一个：包含数字的情况，以及不改变字母的情况
            dfs(s,u+1);
            //如果是字母
            if(!isdigit(s[u])){
                s[u]^=32;   //大写换小写，小写换大写
                dfs(s,u+1); // 下一位
                s[u]^=32;   //恢复现场
            }
        }
    }
};
```

**java代码实现：**

```java
class Solution {
    List<String> res=new ArrayList<>();
    public List<String> letterCasePermutation(String s) {
        dfs(s,s.toCharArray(),0);
        return res;
    }
    public void dfs(String s,char[] arr,int u){
        if(u==s.length()){
            res.add(new String(arr));
            return;
        }else{
            dfs(s,arr,u+1);
            // 先将c拿出来，在计算不能改变c
            // char c=arr[u];
            if(!Character.isDigit(arr[u])){
                // c^=32
                arr[u]^=32;
                dfs(s,arr,u+1);
                arr[u]^=32;
                // c^=32
            }
        }
    }
}
```



#### [90. 子集 II](https://leetcode.cn/problems/subsets-ii/)（去重问题）

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

**c++代码实现：**

```c
class Solution {
public:

    vector<vector<int>> res;
    vector<int> path;

    void dfs(vector<int>& nums,int u){
        if(nums.size() == u){
            //枚举到了最后一个了则加入答案
            res.push_back(path);
            return;
        }

        //找出重复元素个数
        int k=u+1;
        while(k<nums.size() && nums[k]==nums[u]) k++;

        //依次处理重复元素,重复元素只选择一个，,重复元素个数k-u+1
        for(int i=0;i<=k-u;i++){
            dfs(nums,k);
            path.push_back(nums[u]);
        }

        //回溯恢复现场
        for(int i=0;i<=k-u;i++){
            path.pop_back();
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        //排序目的去重
        sort(nums.begin(),nums.end());
        dfs(nums,0);
        return res;
    }
};
```

**hash表去重：**

```c
class Solution {
public:

    unordered_map<int,int> hash;
    vector<vector<int>> res;
    vector<int> path;

    void dfs(int u){
        if(u>10){
            //数据范围为-10到10
            res.push_back(path);
            return;
        }else{
            for(int i=0;i<hash[u]+1;i++){
                /*
                先写dfs，第一次都不选，选空，选完之后加到答案里面
                第2次则有一个元素
                */
                dfs(u+1);
                path.push_back(u);
            }
            //恢复现场
            for(int i=0;i<hash[u]+1;i++)
                path.pop_back();
        }
    }

    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        for(auto& x:nums) hash[x]++;
        dfs(-10);
        return res;
    }
};
```

#### (7)分割回文串

#### [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]
```

**提示：**

- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成

**c++代码：**

**切割问题类似组合问题**

```c
class Solution {
public:
    /*
    先预处理出来s的子串是回文串
    */
    vector<vector<bool>> f; //  记录子串是不是回文串
    vector<vector<string>> res;  
    vector<string> path;

    void dfs(string& s,int u){
        if(u==s.size()) res.push_back(path);
        else{
            for(int i=u;i<s.size();i++){
                //如果u到i是满足条件的，则递归
                if(f[u][i]){
                    path.push_back(s.substr(u,i-u+1));
                    dfs(s,i+1);
                    path.pop_back();    //恢复现场
                }
            }
        }
    }

    vector<vector<string>> partition(string s) {
        int n=s.size();
        // res=vector<vector<string>>(n,vector<string>(n));
        f=vector<vector<bool>>(n,vector<bool>(n));

        /*
        f[i][j]:  i--------j
            (1) i==j 只有一个字符串
            (2) f[i+1][j-1]=true,也就是s[i]==s[j]下面的情况
                s[i]==s[j]了，如果i+1>j-1则是长度为2的满足两个相同
        外层循环是从小到大枚举j，所以f[i+1][j-1]会在f[i][j]之前被计算出来
        先计算j-1的所有才计算j
        */
        for(int j=0;j<n;j++){
            for(int i=0;i<=j;i++){
                if(i==j){
                    //只有一个字符串
                    f[i][j]=true;
                }else if(s[i]==s[j]){
                    //i+1>j-1是指包含两个字符串的情况
                    if(i+1>j-1 || f[i+1][j-1])
                        f[i][j]=true;
                }
            }
        }

        dfs(s,0);
        return res;
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<String>> res=new ArrayList<>();
    List<String> path=new ArrayList<>();
    int[][] f;

    public void dfs(String s,int u){
        if(u==s.length()){
            res.add(new ArrayList<>(path));
            return;
        }else{
            for(int i=u;i<s.length();i++){
                if(f[u][i]==1){
                    path.add(s.substring(u,i+1));
                    dfs(s,i+1);
                    path.remove(path.size()-1);
                }
            }
        }
    }

    public List<List<String>> partition(String s) {
        int n=s.length();
        f=new int[n+1][n+1];
        for(int i=0;i<=n;i++){
            Arrays.fill(f[i],0);
        }

        for(int j=0;j<n;j++){
            for(int i=0;i<=j;i++){
                if(i==j) f[i][j]=1;
                else if(s.charAt(i)==s.charAt(j)){
                    if(i+1>j-1 || f[i+1][j-1]==1){
                        f[i][j]=1;
                    }
                }
            }
        }

        dfs(s,0);
        return res;
    }
}
```

#### [132. 分割回文串 II](https://leetcode.cn/problems/palindrome-partitioning-ii/)(DP)

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是回文。

返回符合要求的 **最少分割次数** 。

**示例 1：**

```
输入：s = "aab"
输出：1
解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```

**示例 2：**

```
输入：s = "a"
输出：0
```

**示例 3：**

```
输入：s = "ab"
输出：1
```

**提示：**

- `1 <= s.length <= 2000`
- `s` 仅由小写英文字母组成

**c++代码实现：**

```
(动态规划) O(n^2)
一共进行两次动态规划。

第一次动规：计算出每个子串是否是回文串。
状态表示：st[i][j]表示 s[i…j]是否是回文串;
转移方程：s[i…j] 是回文串当且仅当 s[i]等于s[j] 并且 s[i+1…j−1]是回文串；
边界情况：如果s[i…j]的长度小于等于2，则st[i][j]=(s[i]==s[j]);

在第一次动规的基础上，我们进行第二次动规。
状态表示：f[i]表示把前 i 个字符划分成回文串，最少划分成几部分；
状态转移：枚举最后一段回文串的起点 j，然后利用 st[j][i]可知 s[j…i] 是否是回文串，如果是回文串，则 f[i]可以从f[j−1]+1 转移；
边界情况：0个字符可以划分成0部分，所以f[0]=0。

题目让我们求最少切几刀，所以答案是 f[n]−1。

时间复杂度分析：两次动规都是两重循环，所以时间复杂度是O(n^2)。

作者：yxc
链接：https://www.acwing.com/solution/content/227/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

```c
class Solution {
public:
    int minCut(string s) {
        int n=s.size();
        s=' '+s;
        //存储s[i][j]从i到j是否构成回文串
        vector<vector<bool>> g(n+1,vector<bool>(n+1));
        vector<int> f(n+1,1e8);

        /*
        f[i][j]:  i--------j
            (1) i==j 只有一个字符串
            (2) s[i]==s[j]
               如果i+1>j-1则是长度为2的满足两个相同;
               如果f[i+1][j-1]=true,也就是s[i]==s[j]下面的情况
        外层循环是从小到大枚举j，所以f[i+1][j-1]会在f[i][j]之前被计算出来
        先计算j-1的所有才计算j
        */
        for(int j=1;j<=n;j++){
            for(int i=1;i<=n;i++){
                if(i==j) g[i][j]=true;
                else if(s[i]==s[j]){
                    if(i+1>j-1 || g[i+1][j-1]) g[i][j]=true;
                }
            }
        }

        //0个字符可以划分成0部分，所以f[0]=0
        f[0]=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i;j++){
                if(g[j][i]){
                    f[i]=min(f[i],f[j-1]+1);
                }
            }
        }
        return f[n]-1;
    }
};
```



#### (8)复原IP

#### [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

**有效 IP 地址** 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 **有效** IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 **无效** IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

**提示：**

- `1 <= s.length <= 20`
- `s` 仅由数字组成

**c++代码实现：**

```c
class Solution {
public:
    vector<string> res;
    vector<string> restoreIpAddresses(string s) {
        dfs(s,0,0,"");
        return res;
    }

    /*
        u表示当前搜索到s的哪个位置
        k表示当前搜索到哪个数字了0~4
        path为当前的状态路径
    */
    void dfs(string& s,int u,int k,string path){
        if(u==s.size()){
            if(k==4){   //如果搜素完s了，并且最后这个位置满了4个字符长度了
                //先将最后一步多加的点去了
                path.pop_back();
                res.push_back(path);
            }
            return;
        }
        //剪枝:字符串大于12的情况
        if(k==4) return;
        //t记录当前ip的一个位置的数值大小
        for(int i=u,t=0;i<s.size();i++){
            //处理前导0的情况，如果长度大于2并且第一位置是0
            if(i>u && s[u]=='0') break;
            t=t*10+s[i]-'0';    //计算当前的数值是否大于255
            if(t<=255){
                dfs(s,i+1,k+1,path + to_string(t) +'.');
            }else break;
        }
    }
};
```

#### (9)数组上的搜索

#### [491. 递增子序列](https://leetcode.cn/problems/increasing-subsequences/)

给你一个整数数组 nums ，找出并返回所有该数组中不同的递增子序列，递增子序列中 至少有两个元素 。你可以按 任意顺序 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

**示例 1：**

```
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**示例 2：**

```
输入：nums = [4,4,3,2,1]
输出：[[4,4]]
```

**提示：**

- `1 <= nums.length <= 15`
- `-100 <= nums[i] <= 100`

**c++代码实现：**

```c
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;

    //start表示当前已经搜索到的位置
    void dfs(vector<int>& nums,int start){
        if(path.size()>=2) res.push_back(path); //记录答案
        if(start==nums.size()) return;  //回溯结束
        

        unordered_set<int> S;
        for(int i=start;i<nums.size();i++){
            //如果当前方案为空 或者 当前的最后一个元素小于等于当前位置
            if(path.empty() || path.back()<=nums[i]){
                if(S.count(nums[i])) continue;  //出现过则去重跳过
                S.insert(nums[i]);
                path.push_back(nums[i]); 
                dfs(nums,i+1);
                path.pop_back(); //恢复现场
            }
        }
    }

    vector<vector<int>> findSubsequences(vector<int>& nums) {
        dfs(nums,0);
        return res;
    }
};
```



### 3、floodFill

#### [733. 图像渲染](https://leetcode.cn/problems/flood-fill/)

有一幅以 m x n 的二维整数数组表示的图画 image ，其中 image[i][j] 表示该图画的像素值大小。

你也被给予三个整数 sr ,  sc 和 newColor 。你应该从像素 image[sr][sc] 开始对图像进行 上色填充 。

为了完成 上色工作 ，从初始像素开始，记录初始坐标的 上下左右四个方向上 像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应 四个方向上 像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为 newColor 。

最后返回 经过上色渲染后的图像 。

**示例 1:**

![](https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg)

````
输入: image = [[1,1,1],[1,1,0],[1,0,1]]，sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]
解析: 在图像的正中间，(坐标(sr,sc)=(1,1)),在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，因为它不是在上下左右四个方向上与初始点相连的像素点。
````

**示例 2:**

```
输入: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, newColor = 2
输出: [[2,2,2],[2,2,2]]
```

**提示:**

m == image.length
n == image[i].length
1 <= m, n <= 50
0 <= image[i][j], newColor < 216
0 <= sr < m
0 <= sc < n

```c
class Solution {
public:
    vector<vector<int>> g;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    void dfs(int x,int y,int color,int newColor){
        g[x][y]=newColor;
        for(int k=0;k<4;k++){
            int a=x+dx[k],b=y+dy[k];
            if(a>=0 && a<g.size() && b>=0 && b<g[0].size() && g[a][b]==color){
                dfs(a,b,color,newColor);
            }
        }
    }
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor){
        g=image;
        int color=g[sr][sc];
        if(color==newColor) return g;
        dfs(sr,sc,color,newColor);
        return g;
    }
};
```

#### [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

**示例 1：**

```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

提示：

m == grid.length
n == grid[i].length
1 <= m, n <= 300
$grid[i][j] 的值为 '0' 或 '1'$

**c++代码实现：**

**DFS**

```c
class Solution {
public:
    vector<vector<char>> g;
    int n,m,res;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    void dfs(int x,int y){
        g[x][y]=0;
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]=='1'){
                dfs(a,b);
            }
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        g=grid;
        n=g.size(),m=g[0].size();
        res=0;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]=='1'){
                    dfs(i,j);
                    res++;
                }
            }
        }
        return res;    
    }
};
```

**java代码实现：**

```java
class Solution {
    char[][] g;
    int n,m,res;
    int[] dx={-1,0,1,0};
    int[] dy={0,1,0,-1};
    public void dfs(int x,int y){
        g[x][y]=0;
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]=='1'){
                dfs(a,b);
            }
        }
    }

    public int numIslands(char[][] grid) {
        g=grid;
        n=grid.length;
        m=grid[0].length;
        res=0;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]=='1'){
                    dfs(i,j);
                    res++;
                }
            }
        }
        return res;
    }
}
```

**BFS**

```c
typedef pair<int,int> PII;
#define x first
#define y second

class Solution {
private:
    int n;
    int m;
    queue<PII> q;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

public:
    int numIslands(vector<vector<char>>& grid) {
        n=grid.size(),m=grid[0].size();
        int res=0;

        for (int i=0; i<n; i++){
            for (int j=0; j<m; j++){
                if (grid[i][j]=='0') continue;
                grid[i][j]='0';
                q.push({i,j});
                res++;
                while (!q.empty()){
                    auto t=q.front();
                    q.pop();
                    for (int i=0; i<4; i++){
                        int a=t.x+dx[i],b=t.y+dy[i];
                        if(a>=0 && a<n && b>=0 && b<m && grid[a][b]=='1'){
                            q.push({a,b});
                            grid[a][b]='0';
                        }
                    }
                }
            }
        }
        return res;
    }
};
```



#### [130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)

给你一个 `m x n` 的矩阵 `board` ，由若干字符 `'X'` 和 `'O'` ，找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

```
输入：board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
输出：[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
解释：被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的
```

**示例 2：**

```
输入：board = [["X"]]
输出：[["X"]]
```

提示：

m == board.length
n == board[i].length
1 <= m, n <= 200
$board[i][j] 为 'X' 或 'O'$

**c++代码实现：**

**DFS**

```c
将边界的O和相邻的位置标记为不能修改，那么其他的O则可以修改
```

```c
class Solution {
public:
    int n,m;
    vector<vector<char>> g;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    void solve(vector<vector<char>>& board) {
        g=board;
        n=g.size(),m=g[0].size();

        for(int i=0;i<n;i++){
            if(g[i][0]=='O') dfs(i,0);
            if(g[i][m-1]=='O') dfs(i,m-1);
        }
        for(int i=0;i<m;i++){
            if(g[0][i]=='O') dfs(0,i);
            if(g[n-1][i]=='O') dfs(n-1,i);
        }

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]=='.') g[i][j]='O';
                else g[i][j]='X';
            }
        }

        board=g;
    }
    void dfs(int x,int y){
        g[x][y]='.';
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]=='O'){
                dfs(a,b);
            }
        }
    }
};
```

**BFS**

```c
typedef pair<int,int> PII;
#define x first
#define y second
class Solution {
public:
    int n,m;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};
    vector<vector<char>> g;
    queue<pair<int,int>> q;

    void solve(vector<vector<char>>& board) {
        g=board;
        n=g.size(),m=g[0].size();

        for(int i=0;i<n;i++){
            if(g[i][0]=='O') bfs(i,0);
            if(g[i][m-1]=='O') bfs(i,m-1);
        }

        for(int j=0;j<m;j++){
            if(g[0][j]=='O') bfs(0,j);
            if(g[n-1][j]=='O') bfs(n-1,j);
        }

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]=='#') g[i][j]='O';
                else if(g[i][j]=='O') g[i][j]='X';
            }
        }
        board=g;
    }

    void bfs(int x,int y){
        q.push({x,y});
        g[x][y]='#';
        while(!q.empty()){
            auto t=q.front();
            q.pop();

            for(int i=0;i<4;i++){
                int a=t.x+dx[i],b=t.y+dy[i];
                if(a<0 || a>=n || b<0 || b>=m || g[a][b]!='O') continue;
                g[a][b]='#';
                q.push({a,b});
            }
        }
    }
};
```



#### [417. 太平洋大西洋水流问题](https://leetcode.cn/problems/pacific-atlantic-water-flow/)(DFS+位运算)

有一个 m × n 的矩形岛屿，与 太平洋 和 大西洋 相邻。 “太平洋” 处于大陆的左边界和上边界，而 “大西洋” 处于大陆的右边界和下边界。

这个岛被分割成一个由若干方形单元格组成的网格。给定一个 m x n 的整数矩阵 heights ，$heights[r][c]$ 表示坐标 (r, c) 上单元格 高于海平面的高度 。

岛上雨水较多，如果相邻单元格的高度 小于或等于 当前单元格的高度，雨水可以直接向北、南、东、西流向相邻单元格。水可以从海洋附近的任何单元格流入海洋。

返回网格坐标 result 的 2D 列表 ，其中 result[i] = [ri, ci] 表示雨水从单元格 (ri, ci) 流动 既可流向太平洋也可流向大西洋 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)

```
输入: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
输出: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```

**示例 2：**

```
输入: heights = [[2,1],[1,2]]
输出: [[0,0],[0,1],[1,0],[1,1]]
```

**提示：**

m == heights.length
n == heights[r].length
1 <= m, n <= 200
0 <= $heights[r][c]$ <= 105

**c++代码实现：**

```c
class Solution {
public:
    int n,m;
    vector<vector<int>> g;
    vector<vector<int>> st; //用来记录能否到  太平洋：1  大西洋：2 两个都能到3  位运算两位来表示
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    void dfs(int x,int y,int t){
        /*
        如果(x,y)已经访问了
        位运算XX两位来表示三种状态，st[x][y] & t表示取出XX的对应t位置是否是1
        是1则表示已经访问，否则表示还未访问
        */
        if(st[x][y] & t) return;
        st[x][y] |=t;   //将(x,y)标记为能到t ，将 st[x][y]的t位置改为1
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            //从四周遍历，下一个点比xy高则能从ab到xy
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]>=g[x][y]){
                dfs(a,b,t);
            }
        }
    }

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        n=heights.size(),m=heights[0].size();
        if(!n || !m) return {};
        g=heights;
        st=vector<vector<int>>(n,vector<int>(m));

        for(int i=0;i<n;i++) dfs(i,0,1);    //左边
        for(int i=0;i<m;i++) dfs(0,i,1);    //上
        for(int i=0;i<n;i++) dfs(i,m-1,2);  //右
        for(int i=0;i<m;i++) dfs(n-1,i,2);  //下

        vector<vector<int>> res;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(st[i][j]==3)
                    res.push_back({i,j});
            }
        }
        return res;
    }
};
```



### 4、模拟+DFS

#### [529. 扫雷游戏](https://leetcode.cn/problems/minesweeper/)

让我们一起来玩扫雷游戏！

给你一个大小为 m x n 二维字符矩阵 board ，表示扫雷游戏的盘面，其中：

'M' 代表一个 未挖出的 地雷，
'E' 代表一个 未挖出的 空方块，
'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的 已挖出的 空白方块，
数字（'1' 到 '8'）表示有多少地雷与这块 已挖出的 方块相邻，
'X' 则表示一个 已挖出的 地雷。
给你一个整数数组 click ，其中 click = [clickr, clickc] 表示在所有 未挖出的 方块（'M' 或者 'E'）中的下一个点击位置（clickr 是行下标，clickc 是列下标）。

根据以下规则，返回相应位置被点击后对应的盘面：

如果一个地雷（'M'）被挖出，游戏就结束了- 把它改为 'X' 。
如果一个 没有相邻地雷 的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的 未挖出 方块都应该被递归地揭露。
如果一个 至少与一个地雷相邻 的空方块（'E'）被挖出，修改它为数字（'1' 到 '8' ），表示相邻地雷的数量。
如果在此次点击中，若无更多方块可被揭露，则返回盘面。

**示例 1：**

![](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_1.png)

```c
输入：board = [["E","E","E","E","E"],["E","E","M","E","E"],["E","E","E","E","E"],["E","E","E","E","E"]], click = [3,0]
输出：[["B","1","E","1","B"],["B","1","M","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]]
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2018/10/12/minesweeper_example_2.png)

```c
输入：board = [["B","1","E","1","B"],["B","1","M","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]], click = [1,2]
输出：[["B","1","E","1","B"],["B","1","X","1","B"],["B","1","1","1","B"],["B","B","B","B","B"]]
```

提示：

- m == board.length
- n == board[i].length
- 1 <= m, n <= 50
- $board[i][j] $为 'M'、'E'、'B' 或数字 '1' 到 '8' 中的一个
- click.length == 2
- 0 <= clickr < m
- 0 <= clickc < n
- $board[clickr][clickc] $为 'M' 或 'E'

**c++代码实现：**

**DFS**

```
处理八个方向的技巧
```

```c
class Solution {
public:
    int n,m;
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        n=board.size(),m=board[0].size();
        int x=click[0],y=click[1];
        //如果点击的位置是地雷的话，则修改直接返回
        if(board[x][y]=='M'){
            board[x][y]='X';
            return board;
        }
        //否则则直接dfs,处理当前位置是E的情况
        dfs(board,x,y);
        return board;
    }

    /*
    当前位置是E包含两种情况：
    (1) E旁边八个位置至少一个地雷，则将E修改为附近地雷的数量
    (2) E旁边八个位置没有地雷，修改E为B，将附近八个位置揭开
    */
    void dfs(vector<vector<char>>& board,int x,int y){
        if(board[x][y]!='E') return;    //回溯出口
        int ans=0;  //记录旁边八个位置地雷的数量
        //中间位置(x,y)，附近八个位置
        for(int i=max(x-1,0);i<=min(x+1,n-1);i++){
            for(int j=max(y-1,0);j<=min(y+1,m-1);j++){
                if(i!=x || j!=y){   //如果不是中间位置(x,y)
                    if(board[i][j]=='M' || board[i][j]=='X'){
                        //统计未揭开地雷后者揭开地雷数量
                        ans++;
                    }
                }
            }
        }
        //(1)E旁边八个位置至少一个地雷，则将E修改为附近地雷的数量
        if(ans){
            board[x][y]='0'+ans;
            return;
        }
        //(2) E旁边八个位置没有地雷，修改E为B，将附近八个位置揭开
        board[x][y]='B';
        //中间位置(x,y)，附近八个位置
        for(int i=max(x-1,0);i<=min(x+1,n-1);i++){
            for(int j=max(y-1,0);j<=min(y+1,m-1);j++){
                if(i!=x || j!=y){   //如果不是中间位置(x,y)
                    dfs(board,i,j);
                }
            }
        }
    } 
};
```

**BFS**

```c
typedef pair<int,int> PII;
#define x first
#define y second

class Solution {
public:
    int n,m;
    vector<vector<int>> st;
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        n=board.size(),m=board[0].size();
        int x=click[0],y=click[1];
        st=vector<vector<int>>(n,vector<int>(m,0));

        //如果点击的位置是地雷的话，则修改直接返回
        if(board[x][y]=='M'){
            board[x][y]='X';
            return board;
        }
        queue<PII> q;
        q.push({x,y});

        while(!q.empty()){
            auto t=q.front();
            q.pop();

            int a=t.x,b=t.y;
            //统计当前位置附近地雷的数量
            int ans=0; 
            //next记录(a,b)旁边八个位置没有地雷，修改E为B，将附近八个位置揭开
            vector<PII> ne;    
             //中间位置(a,b)，附近八个位置
            for(int i=max(a-1,0);i<=min(a+1,n-1);i++){
                for(int j=max(b-1,0);j<=min(b+1,m-1);j++){
                    if(i!=a || j!=b){ //如果不是中间位置(x,y)
                        if(board[i][j]=='M'|| board[i][j]=='X'){
                            //统计未揭开地雷后者揭开地雷数量
                            ans++;
                        }else{
                            if(st[i][j]==0){
                                ne.push_back({i,j});
                            }
                        }
                    }
                }
            }
            //(1)E旁边八个位置至少一个地雷，则将E修改为附近地雷的数量
            if(ans){
                board[a][b]='0'+ans;
            }else{
                //(2) E旁边八个位置没有地雷，修改E为B，将附近八个位置揭开
                board[a][b]='B';
                for(auto& item:ne){
                    q.push(item);
                    st[item.x][item.y]=1;
                }
            }

        }

        return board;
    }
};
```

**5、记忆化搜索（DP）**




## 三、BFS

### 1、暴力

#### [126. 单词接龙 II](https://leetcode.cn/problems/word-ladder-ii/)最短路问题

按字典 wordList 完成从单词 beginWord 到单词 endWord 转化，一个表示此过程的 转换序列 是形式上像 beginWord -> s1 -> s2 -> ... -> sk 这样的单词序列，并满足：

每对相邻的单词之间仅有单个字母不同。
转换过程中的每个单词 si（1 <= i <= k）必须是字典 wordList 中的单词。注意，beginWord 不必是字典 wordList 中的单词。
sk == endWord
给你两个单词 beginWord 和 endWord ，以及一个字典 wordList 。请你找出并返回所有从 beginWord 到 endWord 的 最短转换序列 ，如果不存在这样的转换序列，返回一个空列表。每个序列都应该以单词列表 [beginWord, s1, s2, ..., sk] 的形式返回。

**示例 1：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
解释：存在 2 种最短的转换序列：
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"
```

**示例 2：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：[]
解释：endWord "cog" 不在字典 wordList 中，所以不存在符合要求的转换序列。
```

提示：

1 <= beginWord.length <= 5
endWord.length == beginWord.length
1 <= wordList.length <= 500
wordList[i].length == beginWord.length
beginWord、endWord 和 wordList[i] 由小写英文字母组成
beginWord != endWord
wordList 中的所有单词 互不相同

**题解：**

```
算法
(BFS+DFS) O(2^n nL)
这道题目比较复杂，我们一步一步来解决。

首先考虑如何建图，有两种方式：

枚举所有单词对，然后判断是否可以通过改变一个字母相互转化，时间复杂度 O(n^2L))；
枚举每个单词，然后枚举该单词的每一位字母，再枚举这一位的所有备选字母，然后再判断改变后的字符串是否存在，时间复杂度  O(26nL^2)。
我们要根据数据范围选择使用哪种建图方式，如果 26L>n，则用第二种，否则用第一种。经测试，早先leetcode上两种方式都是可以AC的，但后来增加了 n 的大小，但并未增加 L 的大小，所以第二种建图方式会超时，于是我们选择第一种建图方式即可。

然后，计算所有点 i 到起点 S 的最短距离 dist[i]。由于边权都是1，所以用BFS即可。

最后，我们利用 dist[]数组爆搜出所有最短路径：
从终点 T 开始搜索，首先枚举终点的所有邻接点 v，如果 dist[v]+1==dist[T]，则说明存在一条最短路径，最后一步是从 v 走到 T；然后递归搜索节点 v，依此类推，直到搜索到起点 S 为止，说明找到了一条从 S 到 T 的最短路径。

时间复杂度分析：

建图，通过上述分析可知，时间复杂度是 O(26nL^2)；
求最短路用的是BFS，每个节点仅会遍历一次，每个点遍历时需要O(L)的计算量，所以时间复杂度是 O(nL)；
求方案用的是DFS，总方案数是指数级的，再加上记录方案需要 O(nL) 的时间，所以时间复杂度是 O(2^n nL)；
所以总时间复杂度是 O(26nL^2+nL+2^n nL) = O(2^n nL)。

作者：yxc
链接：https://www.acwing.com/solution/content/217/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

**c++代码：**

```c
class Solution {
public:
    unordered_set<string> S;
    unordered_map<string, int> dist;
    queue<string> q;
    vector<vector<string>> ans;
    vector<string> path;
    string beginWord;

    vector<vector<string>> findLadders(string _beginWord, string endWord, vector<string>& wordList) {
        for (auto word: wordList) S.insert(word);
        beginWord = _beginWord;
        dist[beginWord] = 0;
        q.push(beginWord);
        while (q.size()) {
            auto t = q.front();
            q.pop();

            string r = t;
            for (int i = 0; i < t.size(); i ++ ) {
                t = r;
                for (char j = 'a'; j <= 'z'; j ++ ) {
                    t[i] = j;
                    if (S.count(t) && dist.count(t) == 0) {
                        dist[t] = dist[r] + 1;
                        if (t == endWord) break;
                        q.push(t);
                    }
                }
            }
        }

        if (dist.count(endWord) == 0) return ans;
        path.push_back(endWord);
        dfs(endWord);
        return ans;
    }

    void dfs(string t) {
        if (t == beginWord) {
            reverse(path.begin(), path.end());
            ans.push_back(path);
            reverse(path.begin(), path.end());
        } else {
            string r = t;
            for (int i = 0; i < t.size(); i ++ ) {
                t = r;
                for (char j = 'a'; j <= 'z'; j ++ ) {
                    t[i] = j;
                    if (dist.count(t) && dist[t] + 1 == dist[r]) {
                        path.push_back(t);
                        dfs(t);
                        path.pop_back();
                    }
                }
            }
        }
    }
};
```

#### [127. 单词接龙](https://leetcode.cn/problems/word-ladder/)

字典 wordList 中从单词 beginWord 和 endWord 的 转换序列 是一个按下述规格形成的序列 beginWord -> s1 -> s2 -> ... -> sk：

每一对相邻的单词只差一个字母。
 对于 1 <= i <= k 时，每个 si 都在 wordList 中。注意， beginWord 不需要在 wordList 中。
sk == endWord
给你两个单词 beginWord 和 endWord 和一个字典 wordList ，返回 从 beginWord 到 endWord 的 最短转换序列 中的 单词数目 。如果不存在这样的转换序列，返回 0 。

**示例 1：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

**示例 2：**

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：0
解释：endWord "cog" 不在字典中，所以无法进行转换。
```

提示：

1 <= beginWord.length <= 10
endWord.length == beginWord.length
1 <= wordList.length <= 5000
wordList[i].length == beginWord.length
beginWord、endWord 和 wordList[i] 由小写英文字母组成
beginWord != endWord
wordList 中的所有字符串 互不相同

**c++代码实现：**

```c
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> S;
        for(auto& word:wordList) S.insert(word);
        unordered_map<string,int> dist;
        dist[beginWord]=0;
        queue<string> q;
        q.push(beginWord);

        while(q.size()){
            auto t=q.front();
            q.pop();
            //将t的每一位变换，变换之后看是否在wordList里面，并且还没有使用过(距离)
            string r=t;
            for(int i=0;i<t.size();i++){
                t=r;
                for(char j='a';j<='z';j++){
                    t[i]=j;
                    if(S.count(t) && !dist.count(t)){
                        dist[t]=dist[r]+1;
                        if(t==endWord) return dist[t]+1;
                        q.push(t);
                    }
                }
            }
        }
        return 0;
    }
};
```

java代码实现：

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        Set<String> S=new HashSet<>();
        for(String word:wordList) S.add(word);
        Queue<String> q = new LinkedList<>();
        q.add(beginWord);
        Map<String,Integer> dist=new HashMap();
        dist.put(beginWord,0);

        while(!q.isEmpty()){
            String t=q.poll();
            if(t.equals(endWord)) return dist.get(endWord)+1;
            for(String item:wordList){
                if(dist.containsKey(item)) continue;    //已经遍历了
                if(!isChange(t,item)) continue; //如果t和item不能通过改变一个字母边得
                q.add(item);
                dist.put(item,dist.get(t)+1);
            }
        }
        return 0;
    }

    public boolean isChange(String str1,String str2){
        int len=str1.length();
        if(len!=str2.length()) return false;
        char[] c1=str1.toCharArray();
        char[] c2=str2.toCharArray();
        int ans=0;
        for(int i=0;i<len;i++){
            if(c1[i]!=c2[i]) ans++;
        }
        return ans==1;
    }
}
```

#### [130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)todo

#### [433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/)

基因序列可以表示为一条由 8 个字符组成的字符串，其中每个字符都是 'A'、'C'、'G' 和 'T' 之一。

假设我们需要调查从基因序列 start 变为 end 所发生的基因变化。一次基因变化就意味着这个基因序列中的一个字符发生了变化。

例如，"AACCGGTT" --> "AACCGGTA" 就是一次基因变化。
另有一个基因库 bank 记录了所有有效的基因变化，只有基因库中的基因才是有效的基因序列。（变化后的基因必须位于基因库 bank 中）

给你两个基因序列 start 和 end ，以及一个基因库 bank ，请你找出并返回能够使 start 变化为 end 所需的最少变化次数。如果无法完成此基因变化，返回 -1 。

注意：起始基因序列 start 默认是有效的，但是它并不一定会出现在基因库中。

示例 1：

输入：start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]
输出：1
示例 2：

输入：start = "AACCGGTT", end = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
输出：2
示例 3：

输入：start = "AAAAACCC", end = "AACCCCCC", bank = ["AAAACCCC","AAACCCCC","AACCCCCC"]
输出：3


提示：

start.length == 8
end.length == 8
0 <= bank.length <= 10
bank[i].length == 8
start、end 和 bank[i] 仅由字符 ['A', 'C', 'G', 'T'] 组成

```c
class Solution {
public:
    int minMutation(string start, string end, vector<string>& bank) {
        //将bank放在S中
        unordered_set<string> S;
        unordered_map<string,int> hash;
        char str[4]={'A','C','G','T'};

        for(auto& s:bank) S.insert(s);
        hash[start]=0;

        queue<string> q;
        q.push(start);

        while(q.size()){
            auto t=q.front();
            int len=t.size();
            q.pop();

            for(int i=0;i<len;i++){
                auto s=t;
                for(auto c:str){
                    s[i]=c;
                    //如果改变之后的字符串在S中也就是在bank中，并且在hash中的距离还为0
                    //更新距离
                    if(S.count(s) && hash.count(s)==0){
                        //更新距离,利用t更新s
                        hash[s]=hash[t]+1;
                        //如果相等了则返回答案
                        if(s==end) return hash[s];
                        q.push(s);
                    }
                }
            }
        }
        //如果都不满足
        return -1;
    }
};
```

#### [752. 打开转盘锁](https://leetcode.cn/problems/open-the-lock/)

你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为 '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 -1 。

示例 1:

输入：deadends = ["0201","0101","0102","1212","2002"], target = "0202"
输出：6
解释：
可能的移动序列为 "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202"。
注意 "0000" -> "0001" -> "0002" -> "0102" -> "0202" 这样的序列是不能解锁的，
因为当拨动到 "0102" 时这个锁就会被锁定。
示例 2:

输入: deadends = ["8888"], target = "0009"
输出：1
解释：把最后一位反向旋转一次即可 "0000" -> "0009"。
示例 3:

输入: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
输出：-1
解释：无法旋转到目标数字且不被锁定。


提示：

1 <= deadends.length <= 500
deadends[i].length == 4
target.length == 4
target 不在 deadends 之中
target 和 deadends[i] 仅由若干位数字组成

```c
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        string start="0000";
         //特判一下
        if(start==target) return 0;

        unordered_map<string,int> hash;
        unordered_set<string> S;
        // char str[10]={'0','1','2','3','4','5','6','7','8','9'};
        for(auto& s:deadends) S.insert(s);

        if(S.count(start)) return -1; 

        queue<string> q;
        q.push(start);
        hash[start]=0;

        while(q.size()){
            auto t=q.front();
            q.pop();

            for(int i=0;i<4;i++){
                for(int j=-1;j<=1;j+=2){
                    auto s=t;
                    s[i]=(s[i]-'0'+j+10)%10+'0';
                    if(S.count(s)==0 && hash.count(s)==0){
                        hash[s]=hash[t]+1;
                        if(s==target) return hash[s];
                        q.push(s);
                    } 
                }
            }
        }
        return -1;
    }
};
```

#### [777. 在LR字符串中交换相邻字符](https://leetcode.cn/problems/swap-adjacent-in-lr-string/)

在一个由 'L' , 'R' 和 'X' 三个字符组成的字符串（例如"RXXLRXRXL"）中进行移动操作。一次移动操作指用一个"LX"替换一个"XL"，或者用一个"XR"替换一个"RX"。现给定起始字符串start和结束字符串end，请编写代码，当且仅当存在一系列移动操作使得start可以转换成end时， 返回True。

**示例 :**

```
输入: start = "RXXLRXRXL", end = "XRLXXRRLX"
输出: True
解释:
我们可以通过以下几步将start转换成end:
RXXLRXRXL ->
XRXLRXRXL ->
XRLXRXRXL ->
XRLXXRRXL ->
XRLXXRRLX
```

**提示：**

- `1 <= len(start) = len(end) <= 10000`。
- `start`和`end`中的字符串仅限于`'L'`, `'R'`和`'X'`。

**c++代码:**

```c
class Solution {
public:
    /*
    R和L的相对顺序不变
    必要性：
    （1）删除x之后L和R的相对顺序相等
    （2）L只能向左走，R只能向右走

    */
    bool canTransform(string start, string end) {
        string  a,b;
        for(auto& x:start)
            if(x!='X') a+=x;
        for(auto& x:end)
            if(x!='X') b+=x;
        if(a!=b) return false;

        //L只能左移
        for(int i=0,j=0;i<start.size();i++,j++){
            while(i<start.size() && start[i]!='L') i++;
            while(j<end.size() && end[j]!='L') j++;
            if(i<j) return false;
        }

        //R只能右移
        for(int i=0,j=0;i<start.size();i++,j++){
            while(i<start.size() && start[i]!='R') i++;
            while(j<end.size() && end[j]!='R') j++;
            if(i>j) return false;
        }
        return true;
    }
};
```

**java代码：**

```java
class Solution {
    public boolean canTransform(String start, String end) {
        StringBuilder a=new StringBuilder();
        StringBuilder b=new StringBuilder();

        for(int i=0;i<start.length();i++){
            if(start.charAt(i)!='X') a.append(start.charAt(i));
        }
        // System.out.println(a);
        for(int i=0;i<end.length();i++){
            if(end.charAt(i)!='X') b.append(end.charAt(i));
        }
        // System.out.println(b);

        if(a.length()!=b.length()) return false;
        for(int i=0;i<a.length();i++){
            if(a.charAt(i)!=b.charAt(i)) return false;
        }

        //L只能左移
        for(int i=0,j=0;i<start.length();i++,j++){
            while(i<start.length() && start.charAt(i)!='L') i++;
            while(j<end.length() && end.charAt(j)!='L') j++;
            if(i<j) return false;
        }

        //R只能右移
        for(int i=0,j=0;i<start.length();i++,j++){
            while(i<start.length() && start.charAt(i)!='R') i++;
            while(j<end.length() && end.charAt(j)!='R') j++;
            if(i>j) return false;
        }
        return true;
    }
}
```

**python代码：**

```python
class Solution:
    def canTransform(self, start: str, end: str) -> bool:
        if len(start) != len(end):
            return False

        lr_start = [x for x in start if x != 'X']

        lr_end = [x for x in end if x != 'X']

        if lr_start != lr_end:
            return False

        i, j = 0, 0
        while i < len(start) and j < len(end):
            while i < len(start) and start[i] != 'L':
                i += 1
            while j < len(end) and end[j] != 'L':
                j += 1
            if i < j:
                return False
            i += 1
            j += 1

        i, j = 0, 0
        while i < len(start) and j < len(end):
            while i < len(start) and start[i] != 'R':
                i += 1
            while j < len(end) and end[j] != 'R':
                j += 1
            if i > j:
                return False
            i += 1
            j += 1
        return True
```

#### [1036. 逃离大迷宫](https://leetcode.cn/problems/escape-a-large-maze/)todo

#### [1263. 推箱子](https://leetcode.cn/problems/minimum-moves-to-move-a-box-to-their-target-location/)todo

#### [1298. 你能从盒子里获得的最大糖果数](https://leetcode.cn/problems/maximum-candies-you-can-get-from-boxes/)todo

#### [1311. 获取你好友已观看的视频](https://leetcode.cn/problems/get-watched-videos-by-your-friends/)todo

#### [1345. 跳跃游戏 IV](https://leetcode.cn/problems/jump-game-iv/)todo



### 2、多源最短路BFS

#### [542. 01 矩阵](https://leetcode.cn/problems/01-matrix/)

给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

**示例 1：**

![](https://pic.leetcode-cn.com/1626667201-NCWmuP-image.png)

```
输入：mat = [[0,0,0],[0,1,0],[0,0,0]]
输出：[[0,0,0],[0,1,0],[0,0,0]]
```

**示例 2：**

![](https://pic.leetcode-cn.com/1626667205-xFxIeK-image.png)

```
输入：mat = [[0,0,0],[0,1,0],[1,1,1]]
输出：[[0,0,0],[0,1,0],[1,2,1]]
```

提示：

m == mat.length
n == mat[i].length
1 <= m, n <= 104
1 <= m * n <= 104
$mat[i][j] is either 0 or 1.$
mat 中至少有一个 0 

**c++代码实现：**

```c
/*
多源BFS：
抽象为图,无向图，边权都是1直接使用bfs（提高课证明）
添加一个超级源点，源点和起点都连一条边
求:每个起点到v的最短距离dis1
超级源点到v的最短距离dis2
dis1和dis2是一一对应的，dis2=dis1+1
*/
#define x first 
#define y second 
typedef pair<int,int> PII;
class Solution {
public:
    vector<vector<int>> dis;
    int n,m;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    //多源bfs
    //无向图，很多个起点，每个起点到V的最短距离取min；添加超级原点，到V的最短距离

    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        n=mat.size(),m=mat[0].size();
        dis=vector<vector<int>>(n,vector<int>(m,-1));
        queue<PII> q;
        if(!n || !m) return mat;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(mat[i][j]==0){
                    dis[i][j]=0;
                    q.push({i,j});
                }
            }
        }
        while(q.size()){
            auto t=q.front();
            q.pop();
            for(int i=0;i<4;i++){
                int a=t.x+dx[i],b=t.y+dy[i];
                if(a>=0 && a<n && b>=0 && b<m && dis[a][b]==-1){
                    dis[a][b]=dis[t.x][t.y]+1;
                    q.push({a,b});
                }
            }
        }
        return dis;
    }
};
```



#### [934. 最短的桥](https://leetcode.cn/problems/shortest-bridge/)

在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。）

现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。

返回必须翻转的 0 的最小数目。（可以保证答案至少是 1 。）

示例 1：

```
输入：A = [[0,1],[1,0]]
输出：1
```

示例 2：

```
输入：A = [[0,1,0],[0,0,0],[0,0,1]]
输出：2
```

示例 3：

```
输入：A = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
输出：1
```

**提示：**

- `2 <= A.length == A[0].length <= 100`
- `A[i][j] == 0` 或 `A[i][j] == 1`

```c
#define x first 
#define y second 
typedef pair<int,int> PII;

class Solution {
public:

    int n,m;    //矩阵的长和宽
    vector<vector<int>> g,dist;  //全局的g和距离
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};
    queue<PII> q;   //记录源点的所有的点

    void dfs(int x,int y){
        //先标记为已经访问了
        g[x][y]=0;
        //起始远点距离为0
        dist[x][y]=0;
        q.push({x,y});

        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]){
                dfs(a,b);
            }
        }
    }

    int bfs(){
        while(q.size()){
            auto t=q.front();
            q.pop();

            for(int i=0;i<4;i++){
                int a=t.x+dx[i],b=t.y+dy[i];
                if(a>=0 && a<n && b>=0 && b<m && dist[a][b]>dist[t.x][t.y]+1){
                    //根据题目可以得到需要的0的个数减1
                    dist[a][b]=dist[t.x][t.y]+1;
                    if(g[a][b]) return dist[a][b]-1;
                    q.push({a,b});
                }
            }
        }
        return -1;
    }

    int shortestBridge(vector<vector<int>>& grid) {
        g=grid;
        n=g.size(),m=g[0].size();
        //若初始化为INT_MAX可能会溢出
        dist=vector<vector<int>>(n,vector<int>(m,1e8));

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                //如果是起点则dfs，返回bfs的结果
                if(g[i][j]){
                    dfs(i,j);
                    return bfs();
                }
            }
        }
        return -1;
    }
};
```

#### [994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)

在给定的 m x n 网格 grid 中，每个单元格可以有以下三个值之一：

值 0 代表空单元格；
值 1 代表新鲜橘子；
值 2 代表腐烂的橘子。
每分钟，腐烂的橘子 周围 4 个方向上相邻 的新鲜橘子都会腐烂。

返回 直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1 。

 

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/oranges.png)

```
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

**示例 2：**

输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个正向上。

示例 3：

输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。


提示：

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 10`
- `grid[i][j]` 仅为 `0`、`1` 或 `2`

```c
#define x first
#define y second
typedef pair<int,int> PII;

class Solution {
public:

    int orangesRotting(vector<vector<int>>& grid) {
        int n=grid.size(),m=grid[0].size();

        int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

        queue<PII> q;

        for(int i=0;i<n;i++)
            for(int j=0;j<m;j++)
                if(grid[i][j]==2)
                    q.push({i,j});
        
        int res=0;

        while(q.size()){
            //res表示层数
            res++;
            //一层一层的宽搜
            int len=q.size();
            while(len--){
                auto t=q.front();
                q.pop();

                for(int i=0;i<4;i++){
                    int a=t.x+dx[i],b=t.y+dy[i];
                    if(a<0 || a>=n || b<0 || b>=m ||grid[a][b]!=1) continue;
                    grid[a][b]=2;
                    q.push({a,b});
                }
            }
        }

        for(int i=0;i<n;i++)
            for(int j=0;j<m;j++)
                //有新鲜橘子没有腐烂
                if(grid[i][j]==1)
                    return -1;
        //答案是多少步数，应该是层数-1
        if(res) res--;
        return res;
    }
};
```



#### [1162. 地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/)

你现在手里有一份大小为 n x n 的 网格 grid，上面的每个 单元格 都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地。

请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的，并返回该距离。如果网格上只有陆地或者海洋，请返回 -1。

我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。

 

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/1336_ex1.jpeg)

```
输入：grid = [[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释： 
海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大，最大距离为 2。
```

**示例 2：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/1336_ex2.jpeg)

```
输入：grid = [[1,0,0],[0,0,0],[0,0,0]]
输出：4
解释： 
海洋单元格 (2, 2) 和所有陆地单元格之间的距离都达到最大，最大距离为 4。
```

**提示：**



- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 100`
- `grid[i][j]` 不是 `0` 就是 `1`

```c
#define x first
#define y second
typedef pair<int,int> PII;

class Solution {
public:
    int maxDistance(vector<vector<int>>& grid) {
        int n=grid.size(),m=grid[0].size(),INF=1e8;
        vector<vector<int>> dist(n,vector<int>(m,INF));
        int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

        //多源BFS，将所有的1先加到队列里面
        queue<PII> q;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(grid[i][j]){
                    dist[i][j]=0;
                    q.push({i,j});
                }
            }
        }

        while(q.size()){
            auto t=q.front();
            q.pop();

            for(int i=0;i<4;i++){
                int a=t.x+dx[i],b=t.y+dy[i];
                if(a<0 || a>=n || b<0 || b>=m ) continue;
                if(dist[a][b]>dist[t.x][t.y]+1){
                    dist[a][b]=dist[t.x][t.y]+1;
                    q.push({a,b});
                }
            }
        }

        int res=-1;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(!grid[i][j]){
                    res=max(res,dist[i][j]);
                }
            }
        }

        if(res==INF) return -1;
        return res;
    }
};
```

#### [815. 公交路线](https://leetcode.cn/problems/bus-routes/)

给你一个数组 routes ，表示一系列公交线路，其中每个 routes[i] 表示一条公交线路，第 i 辆公交车将会在上面循环行驶。

例如，路线 routes[0] = [1, 5, 7] 表示第 0 辆公交车会一直按序列 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ... 这样的车站路线行驶。
现在从 source 车站出发（初始时不在公交车上），要前往 target 车站。 期间仅可乘坐公交车。

求出 最少乘坐的公交车数量 。如果不可能到达终点车站，返回 -1 。

**示例 1：**

```
输入：routes = [[1,2,7],[3,6,7]], source = 1, target = 6
输出：2
解释：最优策略是先乘坐第一辆公交车到达车站 7 , 然后换乘第二辆公交车到车站 6 。
```

**示例 2：**

```
输入：routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
输出：-1
```

提示：

1 <= routes.length <= 500.
1 <= routes[i].length <= 10^5
routes[i] 中的所有值 互不相同
sum(routes[i].length) <= 10^5
$0 <= routes[i][j] $< 10^6
0 <= source, target < 10^6

**c++代码：**

```c
class Solution {
public:
    /*
    思路：一个公交路线看作一个点，每两个车的路线有相交的则连接一条边
    建图：每个点上有哪些公交路线
    优化：每个公交车，每次遍历上面的站，已经使用则删掉
    g存储每个点上会挂在的环线
    */
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        if(source==target) return 0;
        int n = routes.size();
        unordered_map<int,vector<int>> g; //g存储每个点上会挂在的环线上
        vector<int> dist(n,1e8);    //起点到target的最短路径
        queue<int> q;
        for(int i=0;i<n;i++){
            for(int x:routes[i]){
                //i公交车的线路上面有source，则改公交车可以当坐起点
                if(x==source){
                    dist[i]=1;
                    q.push(i);
                }
                g[x].push_back(i);  //x这个点能挂在i公交线路
            }
        }

        while(!q.empty()){
            auto t=q.front();
            q.pop();

            //遍历公交车t的环线
            for(auto x:routes[t]){
                //环线站有target则直接返回
                if(x==target) return dist[t];
                //否则遍历环线站挂在的公交线路
                for(auto y:g[x]){
                    if(dist[y]>dist[t]+1){
                        dist[y]=dist[t]+1;
                        q.push(y);
                    }
                }
                // g.erase(x); // 优化：每个公交车，每次遍历上面的站，已经使用则删掉
            }
        }
        return -1;
    }

};
```



### 3、单源最短路

#### [ACWing 845. 八数码](https://www.acwing.com/problem/content/847/)

在一个 3×33×3 的网格中，1∼81∼8 这 88 个数字和一个 `x` 恰好不重不漏地分布在这 3×33×3 的网格中。

例如：

```
1 2 3
x 4 6
7 5 8
```

在游戏过程中，可以把 `x` 与其上、下、左、右四个方向之一的数字交换（如果存在）。

我们的目的是通过交换，使得网格变为如下排列（称为正确排列）：

```
1 2 3
4 5 6
7 8 x
```

例如，示例中图形就可以通过让 `x` 先后与右、下、右三个方向的数字交换成功得到正确排列。

交换过程如下：

```
1 2 3   1 2 3   1 2 3   1 2 3
x 4 6   4 x 6   4 5 6   4 5 6
7 5 8   7 5 8   7 x 8   7 8 x
```

现在，给你一个初始网格，请你求出得到正确排列至少需要进行多少次交换。

**输入格式**

输入占一行，将 3×33×3 的初始网格描绘出来。

例如，如果初始网格如下所示：

```
1 2 3 
x 4 6 
7 5 8 
```

则输入为：`1 2 3 x 4 6 7 5 8`

**输出格式**

输出占一行，包含一个整数，表示最少交换次数。

如果不存在解决方案，则输出 −1−1。

**输入样例：**

```
2 3 4 1 5 x 7 6 8
```

**输出样例**

```
19
```

**c++代码实现：**

```c
#include <iostream>
#include <algorithm>
#include <string>
#include <unordered_map>
#include <queue>

using namespace std;

int bfs(string start){
    string ed="12345678x";
    unordered_map<string,int> d;
    queue<string> q;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};
    
    d[start]=0;
    q.push(start);
    
    while(q.size()){
        auto t=q.front();
        q.pop();
        
        if(t==ed) return d[t];
        
        int distance=d[t];
        int k=t.find("x");
        int x=k/3,y=k%3;
        
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a>=0 && a<3 && b>=0 && b<3){
                swap(t[a*3+b],t[k]);
                if(!d.count(t)){
                    d[t]=distance+1;
                    q.push(t);
                }
                swap(t[a*3+b],t[k]);
            }
        }
    }
    return -1;
}

int main(){
    string str, start;
    while(cin >> str) start += str;
    cout<<bfs(start)<<endl;
    return 0;
}
```

#### [864. 获取所有钥匙的最短路径](https://leetcode.cn/problems/shortest-path-to-get-all-keys/)

给定一个二维网格 grid ，其中：

'.' 代表一个空房间
'#' 代表一堵
'@' 是起点
小写字母代表钥匙
大写字母代表锁
我们从起点开始出发，一次移动是指向四个基本方向之一行走一个单位空间。我们不能在网格外面行走，也无法穿过一堵墙。如果途经一个钥匙，我们就把它捡起来。除非我们手里有对应的钥匙，否则无法通过锁。

假设 k 为 钥匙/锁 的个数，且满足 1 <= k <= 6，字母表中的前 k 个字母在网格中都有自己对应的一个小写和一个大写字母。换言之，每个锁有唯一对应的钥匙，每个钥匙也有唯一对应的锁。另外，代表钥匙和锁的字母互为大小写并按字母顺序排列。

返回获取所有钥匙所需要的移动的最少次数。如果无法获取所有钥匙，返回 -1 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-keys2.jpg)

```
输入：grid = ["@.a.#","###.#","b.A.B"]
输出：8
解释：目标是获得所有钥匙，而不是打开所有锁。
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-key2.jpg)

````
输入：grid = ["@..aA","..B#.","....b"]
输出：6
````

**示例 3:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-keys3.jpg)

```
输入: grid = ["@Aa"]
输出: -1
```

提示：

m == grid.length
n == grid[i].length
1 <= m, n <= 30
grid[i][j] 只含有 '.', '#', '@', 'a'-'f' 以及 'A'-'F'
钥匙的数目范围是 [1, 6] 
每个钥匙都对应一个 不同 的字母
每个钥匙正好打开一个对应的锁

**c++代码实现：**

```c
//6个钥匙：2^6个状态
int dist[31][31][64];

class Solution {
public:
    
    struct Node{
        int x,y,s;
    };

    int shortestPathAllKeys(vector<string>& g) {
        int n=g.size(),m=g[0].size(),keysum=0; //需要多少钥匙
        memset(dist, 0x3f, sizeof dist);
        queue<Node> q;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]=='@'){
                    //初始化dist数组
                    dist[i][j][0]=0;    // 设置起点距离为0
                    q.push({i,j,0});    //起点加入队列
                }else if(g[i][j]>='A' && g[i][j]<='Z'){
                    keysum++;   //  统计需要多少锁
                }
            }
        }

        int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};
        while(q.size()){
            auto t=q.front();
            q.pop();
            int d=dist[t.x][t.y][t.s];  // 距离

            for(int i=0;i<4;i++){
                int x=dx[i]+t.x,y=dy[i]+t.y,s=t.s;
                if(x<0 || x>=n || y<0 || y>=m || g[x][y]=='#') continue;
                char c=g[x][y];
                //是钥匙的话把状态设置为1
                if(c>='a' && c<='z'){
                    s|=1<<c-'a'; //  把c对于的字母的状态设置为1
                    if(dist[x][y][s]>d+1){  //  更新距离
                        dist[x][y][s]=d+1;
                        //更新完之后查看是否所有的状态都为1
                        if(s==(1<<keysum)-1) return d+1; 
                        q.push({x,y,s});
                    }
                }else if(c>='A' && c<='Z'){ //  处理锁
                    if(s & (1<<c-'A')){ //  将锁对应的位置设置为1
                        if(dist[x][y][s]>d+1){
                            dist[x][y][s]=d+1;
                            q.push({x,y,s});
                        }
                    }
                }else{
                    if(dist[x][y][s]>d+1){
                        dist[x][y][s]=d+1;
                        q.push({x,y,s});
                    }
                }
            }
        }
        return -1;
    }
};
```



### 3、bellman-ford

#### [787. K 站中转内最便宜的航班](https://leetcode.cn/problems/cheapest-flights-within-k-stops/)

有 n 个城市通过一些航班连接。给你一个数组 flights ，其中 flights[i] = [fromi, toi, pricei] ，表示该航班都从城市 fromi 开始，以价格 pricei 抵达 toi。

现在给定所有的城市和航班，以及出发城市 src 和目的地 dst，你的任务是找到出一条最多经过 k 站中转的路线，使得从 src 到 dst 的 价格最便宜 ，并返回该价格。 如果不存在这样的路线，则输出 -1。

 

示例 1：

输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
输出: 200
解释: 
城市航班图如下

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

从城市 0 到城市 2 在 1 站中转以内的最便宜价格是 200，如图中红色所示。
示例 2：

输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
输出: 500
解释: 
城市航班图如下

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)


从城市 0 到城市 2 在 0 站中转以内的最便宜价格是 500，如图中蓝色所示。


提示：

1 <= n <= 100
0 <= flights.length <= (n * (n - 1) / 2)
flights[i].length == 3
0 <= fromi, toi < n
fromi != toi
1 <= pricei <= 104
航班没有重复，且不存在自环
0 <= src, dst, k < n
src != dst

```c
class Solution {
public:

    const int INF=1e8;

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> dist(n,INF);
        dist[src]=0;

        //中间需要k个中转站，就是需要迭代k+1次
        k++;
        while(k--){
            auto cur=dist;
            for(auto& e:flights){
                int a=e[0],b=e[1],w=e[2];
                cur[b]=min(cur[b],dist[a]+w);
            }
            //滚动数组
            dist=cur;
        }
        if(dist[dst]==INF) return -1;
        return dist[dst];
    }
};
```



### 4、染色法

#### [785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/)

存在一个 无向图 ，图中有 n 个节点。其中每个节点都有一个介于 0 到 n - 1 之间的唯一编号。给你一个二维数组 graph ，其中 graph[u] 是一个节点数组，由节点 u 的邻接节点组成。形式上，对于 graph[u] 中的每个 v ，都存在一条位于节点 u 和节点 v 之间的无向边。该无向图同时具有以下属性：
不存在自环（graph[u] 不包含 u）。
不存在平行边（graph[u] 不包含重复值）。
如果 v 在 graph[u] 内，那么 u 也应该在 graph[v] 内（该图是无向图）
这个图可能不是连通图，也就是说两个节点 u 和 v 之间可能不存在一条连通彼此的路径。
二分图 定义：如果能将一个图的节点集合分割成两个独立的子集 A 和 B ，并使图中的每一条边的两个节点一个来自 A 集合，一个来自 B 集合，就将这个图称为 二分图 。

如果图是二分图，返回 true ；否则，返回 false 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/10/21/bi2.jpg)

```
输入：graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
输出：false
解释：不能将节点分割成两个独立的子集，以使每条边都连通一个子集中的一个节点与另一个子集中的一个节点。
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/10/21/bi1.jpg)

```
输入：graph = [[1,3],[0,2],[1,3],[0,2]]
输出：true
解释：可以将节点分成两组: {0, 2} 和 {1, 3} 。
```

提示：

graph.length == n
1 <= n <= 100
0 <= graph[u].length < n
0 <= graph[u][i] <= n - 1
graph[u] 不会包含 u
graph[u] 的所有值 互不相同
如果 graph[u] 包含 v，那么 graph[v] 也会包含 u

**c++代码实现：**

**DFS染色**

```c
class Solution {
public:
    vector<int> color;
    vector<vector<int>> g;
   
    //染色为1和2
    // 参数：u表示当前节点，c表示当前点的颜色
    bool dfs(int u,int c){
        //记录染色
        color[u]=c;
        //遍历当前点得邻点
        for(auto x:g[u]){
            //如果当前点没有被染色
            if(color[x]!=-1){
                //如果两个点的颜色相同则染色失败
                if(color[x]==c) return false;
            }else if(!dfs(x,!c)) return false;
        }
        return true;
    }

    bool isBipartite(vector<vector<int>>& graph) {
        g=graph;
        // 表示每个点的颜色，-1表示未染色，0表示白色，1表示黑色
        color=vector<int>(g.size(),-1);

        for(int i=0;i<g.size();i++){
            if(color[i]==-1){
                //如果矛盾了返回false
                if(!dfs(i,0))
                    return false;
            }
        }
        return true;
    }
};
```

**BFS染色**

```c
typedef pair<int,int> PII;
#define x first
#define y second
class Solution {
public:
    vector<int> color;
    int n;

    bool isBipartite(vector<vector<int>>& g) {
        n=g.size();
        color.resize(n,-1);

        for(int i=0;i<n;i++){
            if(color[i]!=-1) continue;

            queue<PII> q;
            q.push({i,0});  //i位置染色为0
            while(!q.empty()){
                auto t=q.front();
                q.pop();
                int u=t.x,c=t.y;
                color[u]=c; //将u染色为c
                for(auto& x:g[u]){
                    //如果u的相邻节点x已经染色
                    if(color[x]!=-1){
                        //判断颜色
                        if(color[x]==c) return false;
                    }else{
                        //否则x还没染色，将其染色为u相反的颜色
                        color[x]=(!c);
                        q.push({x,!c});
                    }
                }
            }
        }
        return true;
    }
};
```

#### [886. 可能的二分法](https://leetcode.cn/problems/possible-bipartition/)

给定一组 n 人（编号为 1, 2, ..., n）， 我们想把每个人分进任意大小的两组。每个人都可能不喜欢其他人，那么他们不应该属于同一组。

给定整数 n 和数组 dislikes ，其中 dislikes[i] = [ai, bi] ，表示不允许将编号为 ai 和  bi的人归入同一组。当可以用这种方法将所有人分进两组时，返回 true；否则返回 false。

 

示例 1：

输入：n = 4, dislikes = [[1,2],[1,3],[2,4]]
输出：true
解释：group1 [1,4], group2 [2,3]
示例 2：

输入：n = 3, dislikes = [[1,2],[1,3],[2,3]]
输出：false
示例 3：

输入：n = 5, dislikes = [[1,2],[2,3],[3,4],[4,5],[1,5]]
输出：false


提示：

1 <= n <= 2000
0 <= dislikes.length <= 104
dislikes[i].length == 2
1 <= dislikes[i][j] <= n
ai < bi
dislikes 中每一组都 不同

**c++代码实现：**

```c
class Solution {
public:
    /*
        染色法：二分图
    */

    //存储图：邻接表
    vector<vector<int>> g;
    //记录染色：染为1和2
    vector<int> color;

    bool dfs(int u,int c){
        //将当前点u染色为c
        color[u]=c;
        for(int x:g[u]){
            //如果x位置已经染色了
            if(color[x]){
                //如果x染色和u相同则冲突
                if(color[x]==c){
                   return false;
                }
            }else if(!dfs(x,3-c)){
                //dfs(x,3-c)
                //如果u的染色为1，则x染色为3-c=2
                //如果u的染色为2，则x染色为3-c=1
                return false;
            }
        }
        return true;
    }

    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        g.resize(n);
        color.resize(n);

        for(auto& e:dislikes){
            //这里编号下标为1开始，所以需要处理一下
            int a=e[0]-1,b=e[1]-1;
            g[a].push_back(b);
            g[b].push_back(a);
        }

        for(int i=0;i<n;i++){
            //如果还没有染色，并且染色会有冲突则染色失败
            if(!color[i] && !dfs(i,1)){
               return false;
            }
        }
        return true;
    }

};
```

**c++代码实现：**

```c
class Solution {
public:
    vector<vector<int>> g;
    vector<int> color;

    //将位置u染色为c
    bool dfs(int u,int c){
        color[u]=c;

        for(int x:g[u]){
            if(!color[x]){
                if(!dfs(x,3-c))
                    return false;
            }else if(color[x]==c){
                return false;
            }
        }
        return true;
    }

    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        g.resize(n);
        color.resize(n);

        for(auto& x:dislikes){
            //下标从0开始
            int a=x[0]-1,b=x[1]-1;
            g[a].push_back(b),g[b].push_back(a);
        }

        for(int i=0;i<n;i++){
            if(!color[i] && !dfs(i,1))
                return false;
        }
        return true;
    }
};
```

**java代码实现：**

```java
class Solution {
    List<List<Integer>> g;
    int[] color;

    public boolean dfs(int u,int c){
        color[u]=c;
        for(int x:g.get(u)){
            if(color[x]==-1){
                if(!dfs(x,3-c)){
                    return false;
                }
            }else if(color[x]==c){
                return false;
            }
        }
        return true;
    }

    public boolean possibleBipartition(int n, int[][] dislikes) {
        g=new ArrayList<>();
        color=new int[n+1];
        Arrays.fill(color,-1);

        for(int i=0;i<=n;i++) g.add(new ArrayList<>());

        for(int[] x:dislikes){
            int a=x[0]-1,b=x[1]-1;
            g.get(a).add(b);
            g.get(b).add(a);
        }

        for(int i=0;i<n;i++){
            if(color[i]==-1 && !dfs(i,1)){
                return false;
            }
        }
        return true;
    }
}
```



#### [1034. 边界着色](https://leetcode.cn/problems/coloring-a-border/)

给你一个大小为 m x n 的整数矩阵 grid ，表示一个网格。另给你三个整数 row、col 和 color 。网格中的每个值表示该位置处的网格块的颜色。

两个网格块属于同一 连通分量 需满足下述全部条件：

两个网格块颜色相同
在上、下、左、右任意一个方向上相邻
连通分量的边界 是指连通分量中满足下述条件之一的所有网格块：

在上、下、左、右任意一个方向上与不属于同一连通分量的网格块相邻
在网格的边界上（第一行/列或最后一行/列）
请你使用指定颜色 color 为所有包含网格块grid[row][col]的 连通分量的边界 进行着色，并返回最终的网格 grid 。

 

示例 1：

输入：grid = [[1,1],[1,2]], row = 0, col = 0, color = 3
输出：[[3,3],[3,2]]
示例 2：

输入：grid = [[1,2,2],[2,3,2]], row = 0, col = 1, color = 3
输出：[[1,3,3],[2,3,3]]
示例 3：

输入：grid = [[1,1,1],[1,1,1],[1,1,1]], row = 1, col = 1, color = 2
输出：[[2,2,2],[2,1,2],[2,2,2]]


提示：

m == grid.length
n == grid[i].length
1 <= m, n <= 50
1 <= grid[i][j], color <= 1000
0 <= row < m
0 <= col < n

```c
class Solution {
public:

    vector<vector<int>> g;
    //st数组用来进行标记的，0：没有搜过，1：表示搜过，2：搜过且是边界
    vector<vector<int>> st; 
    int n,m;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};


    void dfs(int x,int y){
        bool is_border=false;
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            //查看是否越界了，并且和g[x][y]是否是连通块
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]==g[x][y]){
                //如果还没访问，则访问一下，并且递归下去
                if(!st[a][b]){
                    st[a][b]=1;
                    dfs(a,b);
                }
            }else{
                //否则说明越界了，是边界
                is_border=true;
            }
        }
        //如果是边界就标记好，最后染色
        if(is_border) st[x][y]=2;
    }

    vector<vector<int>> colorBorder(vector<vector<int>>& grid, int row, int col, int color) {
        g=grid;
        n=g.size(),m=g[0].size();
        st = vector<vector<int>>(n, vector<int>(m));

        //标记着色点为已经搜索
        st[row][col]=1;
        dfs(row,col);

        for(int i=0;i<n;i++)
            for(int j=0;j<m;j++)
                if(st[i][j]==2)
                    grid[i][j]=color;
        return grid;
    }
};
```



### 5、走迷宫问题

#### [1091. 二进制矩阵中的最短路径](https://leetcode.cn/problems/shortest-path-in-binary-matrix/)

给你一个 n x n 的二进制矩阵 grid 中，返回矩阵中最短 畅通路径 的长度。如果不存在这样的路径，返回 -1 。

二进制矩阵中的 畅通路径 是一条从 左上角 单元格（即，(0, 0)）到 右下角 单元格（即，(n - 1, n - 1)）的路径，该路径同时满足下述要求：

路径途经的所有单元格都的值都是 0 。
路径中所有相邻的单元格应当在 8 个方向之一 上连通（即，相邻两单元之间彼此不同且共享一条边或者一个角）。
畅通路径的长度 是该路径途经的单元格总数。

 

示例 1：

![](https://assets.leetcode.com/uploads/2021/02/18/example1_1.png)

```
输入：grid = [[0,1],[1,0]]
输出：2
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2021/02/18/example2_1.png)

```
输入：grid = [[0,0,0],[1,1,0],[1,1,0]]
输出：4
```

**示例 3：**

```
输入：grid = [[1,0,0],[1,1,0],[1,1,0]]
输出：-1
```

**提示：**

- `n == grid.length`
- `n == grid[i].length`
- `1 <= n <= 100`
- `grid[i][j]` 为 `0` 或 `1`

```c
#define x first
#define y second
typedef pair<int,int> PII;

class Solution {
public:

    vector<vector<int>> dist;
    int n;

    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        //特判一下起点是1的情况
        if(grid[0][0]) return -1;

        n=grid.size();
        //初始化为-1的距离
        dist=vector<vector<int>>(n,vector<int>(n,-1));
        //由于是算有多少块，则需要算上起点位置初始化为1
        dist[0][0]=1;
        
        //八个方向
        int dx[]={-1,-1,-1,0,1,1,1,0};
        int dy[]={-1,0,1,1,1,0,-1,-1};

        queue<PII> q;
        q.push({0,0});

        while(q.size()){
            auto t=q.front();
            q.pop();

            for(int i=0;i<8;i++){
                int a=t.x+dx[i],b=t.y+dy[i];
                if(a>=0 && a<n && b>=0 && b<n && grid[a][b]==0 && dist[a][b]==-1){
                    dist[a][b]=dist[t.x][t.y]+1;
                    q.push({a,b});
                }
            }
        }
        return dist[n-1][n-1];

    }
};
```

#### [675. 为高尔夫比赛砍树](https://leetcode.cn/problems/cut-off-trees-for-golf-event/)

你被请来给一个要举办高尔夫比赛的树林砍树。树林由一个 m x n 的矩阵表示， 在这个矩阵中：

0 表示障碍，无法触碰
1 表示地面，可以行走
比 1 大的数 表示有树的单元格，可以行走，数值表示树的高度
每一步，你都可以向上、下、左、右四个方向之一移动一个单位，如果你站的地方有一棵树，那么你可以决定是否要砍倒它。

你需要按照树的高度从低向高砍掉所有的树，每砍过一颗树，该单元格的值变为 1（即变为地面）。

你将从 (0, 0) 点开始工作，返回你砍完所有树需要走的最小步数。 如果你无法砍完所有的树，返回 -1 。

可以保证的是，没有两棵树的高度是相同的，并且你至少需要砍倒一棵树。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/26/trees1.jpg)

```
输入：forest = [[1,2,3],[0,0,4],[7,6,5]]
输出：6
解释：沿着上面的路径，你可以用 6 步，按从最矮到最高的顺序砍掉这些树。
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/11/26/trees2.jpg)

```
输入：forest = [[1,2,3],[0,0,0],[7,6,5]]
输出：-1
解释：由于中间一行被障碍阻塞，无法访问最下面一行中的树。
```

**示例 3：**

```
输入：forest = [[2,3,4],[0,0,5],[8,7,6]]
输出：6
解释：可以按与示例 1 相同的路径来砍掉所有的树。
(0,0) 位置的树，可以直接砍去，不用算步数。
```

提示：

m == forest.length
n == forest[i].length
1 <= m, n <= 50
$0 <= forest[i][j] $<= 109

**c++代码实现：**

```c
class Solution {
public:
    struct Node{
        int x,y,h;
        bool operator<(const Node& t) const{
            return h<t.h;
        }
    };
    int n,m;
    vector<vector<int>> g;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    // bfs返回从start到end的距离,这里题目意思是砍过的树也能走
    int bfs(Node start,Node end){
        if(start.x==end.x && start.y==end.y) return 0;  //第一次进来bfs里面的时候，起点和终点重合
        queue<Node> q;
        q.push({start});
        const int INF = 1e8;
        vector<vector<int>> dist(n,vector<int>(m,INF)); //定义距离数组
        dist[start.x][start.y]=0;
        while(!q.empty()){
            auto t=q.front();
            q.pop();
            for(int i=0;i<4;i++){
                //处理(a,b)附近位置的点
                int a=t.x+dx[i],b=t.y+dy[i];
                if(a>=0 && a<n && b>=0 && b<m && g[a][b]){
                    //到(a,b)的距离需要更新
                    if(dist[a][b]>dist[t.x][t.y]+1){
                        dist[a][b]=dist[t.x][t.y]+1;
                        //更新之后如果是end，则返回dist
                        if(a==end.x && b==end.y) return dist[a][b];
                        q.push({a,b});
                    }

                }
            }
        }
        //如果没有则返回-1
        return -1;
    }

    int cutOffTree(vector<vector<int>>& forest) {
        g=forest;
        n = g.size(), m = g[0].size();

        vector<Node> nodes;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                //将所有的树加进去
                if(g[i][j]>1)
                    nodes.push_back({i,j,g[i][j]});
            }
        }
        sort(nodes.begin(),nodes.end());
        //定义起始坐标
        Node last={0,0};
        int res=0;
        for(auto& now:nodes){
            int k=bfs(last,now);
            if(k==-1) return -1;    //如果从last不能到now，则直接返回
            res+=k;
            last=now;
        }
        return res;
    }
};
```



### 6、并查集+DFS/BFS

#### [827. 最大人工岛](https://leetcode.cn/problems/making-a-large-island/)

给你一个大小为 n x n 二进制矩阵 grid 。最多 只能将一格 0 变成 1 。

返回执行此操作后，grid 中最大的岛屿面积是多少？

岛屿 由一组上、下、左、右四个方向相连的 1 形成。

**示例 1:**

```
输入: grid = [[1, 0], [0, 1]]
输出: 3
解释: 将一格0变成1，最终连通两个小岛得到面积为 3 的岛屿。
```

**示例 2:**

```
输入: grid = [[1, 1], [1, 0]]
输出: 4
解释: 将一格0变成1，岛屿的面积扩大为 4。
```

**示例 3:**

```
输入: grid = [[1, 1], [1, 1]]
输出: 4
解释: 没有0可以让我们变成1，面积依然为 4。
```

**提示：**

- `n == grid.length`

- `n == grid[i].length`

- `1 <= n <= 500`

- `grid[i][j]` 为 `0` 或 `1`

```c
class Solution {
public:
    int n,m;    // 矩阵的长和宽
    vector<int> p,size; // 并查集和并查集的大小

    // 并查集
    int find(int x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }

    //将二维坐标转化为一维坐标
    int get(int x,int y){
        return x*m+y;
    }

    //遍历四个方向
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

    int largestIsland(vector<vector<int>>& grid) {
        n=grid.size(),m=grid[0].size();
        
        //初始化并查集
        for(int i=0;i<n*m;i++){
            p.push_back(i);
            size.push_back(1);
        }

        // 如果全0的话则至少答案是1
        int res=1;
        //找到每个连通块的祖宗和每个连通块的大小
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                //如果是1则枚举四个方向合并连通块
                if(grid[i][j]){
                    //映射为一维坐标
                    int a=get(i,j);
                    for(int k=0;k<4;k++){
                        int x=i+dx[k],y=j+dy[k];
                        if(x>=0 && x<n && y>=0 && y<m && grid[x][y]){
                            //映射为一维坐标
                            int b=get(x,y);
                            if(find(a)!=find(b)){
                                size[find(b)]+=size[find(a)];
                                // cout<<"size[find(b)]:"<<size[find(b)]<<endl;
                                p[find(a)]=find(b);
                                // cout<<"p[find(a)]:"<<p[find(a)]<<endl;
                            }
                        }
                    }
                    res=max(res,size[find(a)]);
                }
            }
        }

        // cout<<res<<endl;
        // for(int i=0;i<n*m;i++){
        //     cout<<p[i]<<" "<<size[i]<<endl;
        // }

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(!grid[i][j]){
                    //存储(i,j)四个方向的不同的连通块
                    map<int,int> hash;
                    for(int k=0;k<4;k++){
                        int x=i+dx[k],y=j+dy[k];
                        if(x>=0 && x<n && y>=0 && y<m && grid[x][y]){
                            int a=get(x,y);
                            // cout<<a<<endl;
                            hash[find(a)]=size[find(a)];
                            // cout<<hash[find(a)]<<endl;
                        }
                    }
                    //将0变为1
                    int cnt=1;
                    for(auto [k,v]:hash) cnt+=v;
                    res=max(res,cnt);
                }
            }
        }
        return res;
    }
};
```

#### [928. 尽量减少恶意软件的传播 II](https://leetcode.cn/problems/minimize-malware-spread-ii/)



### 7、树的层序遍历

#### [637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)

给定一个非空二叉树的根节点 `root` , 以数组的形式返回每一层节点的平均值。与实际答案相差 10^(-5)​以内的答案可以被接受。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/03/09/avg1-tree.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[3.00000,14.50000,11.00000]
解释：第 0 层的平均值为 3,第 1 层的平均值为 14.5,第 2 层的平均值为 11 。
因此返回 [3, 14.5, 11] 。
```

**示例 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/avg2-tree.jpg)

```
输入：root = [3,9,20,15,7]
输出：[3.00000,14.50000,11.00000]
```

**提示：**

- 树中节点数量在 `[1, 104]` 范围内
- `-2^31 <= Node.val <= 2^31 - 1`

**c++代码实现：**

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> res;
        queue<TreeNode*> q;
        q.push(root);

        while(!q.empty()){
            int len=q.size();
            double ans=0;

            for(int i=0;i<len;i++){
                auto t=q.front();
                q.pop();
                ans+=t->val;
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right);
            }
            res.push_back(ans/len);
        }
        return res;
    }
};
```

#### [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

**c++代码实现：**

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        //需要判空
        if(root) q.push(root);

        while(!q.empty()){
            int len=q.size();
            vector<int> path;

            for(int i=0;i<len;i++){
                auto t=q.front();
                q.pop();
                path.push_back(t->val);
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right);
            }
            res.push_back(path);
        }
        return res;
    }
};
```

#### [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-100 <= Node.val <= 100`

**c++代码：**

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if(root) q.push(root);

        int level=0;
        while(!q.empty()){
            int len=q.size();
            vector<int> path;
            while(len--){
                auto t=q.front();
                q.pop();
                path.push_back(t->val);
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right); 
            }
            if((level&1)==0) res.push_back(path);
            else{
                reverse(path.begin(),path.end());
                res.push_back(path);
            }
            level++;
        }
        return res;
    }
};
```



#### [107. 二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

给你二叉树的根节点 `root` ，返回其节点值 **自底向上的层序遍历** 。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[15,7],[9,20],[3]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围 `[0, 2000]` 内
- `-1000 <= Node.val <= 1000`

**c++代码实现：**

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> q;
        if(root) q.push(root);

        while(!q.empty()){
            int len=q.size();
            vector<int> path;
            while(len--){
                auto t=q.front();
                q.pop();
                path.push_back(t->val);
                if(t->left) q.push(t->left);
                if(t->right) q.push(t->right);
            }
            res.push_back(path);
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```

#### [429. N 叉树的层序遍历](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)

给定一个 N 叉树，返回其节点值的*层序遍历*。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

**示例 1：**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2019/11/08/sample_4_964.png)

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

**提示：**

- 树的高度不会超过 `1000`
- 树的节点总数在 `[0, 10^4]` 之间

**c++代码实现：**

```c
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
        queue<Node*> q;
        if(root) q.push(root);

        while(!q.empty()){
            int len=q.size();
            vector<int> path;
            while(len--){
                auto t=q.front();
                q.pop();
                path.push_back(t->val);
                for(auto& son:t->children){
                    if(son) q.push(son);
                }
            }
            res.push_back(path);
        }
        return res;
    }
};
```



## 四、单调栈

#### [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)

nums1 中数字 x 的 下一个更大元素 是指 x 在 nums2 中对应位置 右侧 的 第一个 比 x 大的元素。

给你两个 没有重复元素 的数组 nums1 和 nums2 ，下标从 0 开始计数，其中nums1 是 nums2 的子集。

对于每个 0 <= i < nums1.length ，找出满足 nums1[i] == nums2[j] 的下标 j ，并且在 nums2 确定 nums2[j] 的 下一个更大元素 。如果不存在下一个更大元素，那么本次查询的答案是 -1 。

返回一个长度为 nums1.length 的数组 ans 作为答案，满足 ans[i] 是如上所述的 下一个更大元素 。

**示例 1：**

```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```

**示例 2：**

```
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

提示：

$1 <= nums1.length <= nums2.length <= 1000$,
$0 <= nums1[i], nums2[i] <= 104$,
$nums1$和$nums2$中所有整数 互不相同
$nums1$ 中的所有整数同样出现在 $nums2$ 中

**c++代码实现：**

```c
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        //定义单调栈
        stack<int> stk;
        vector<int> res(nums2.size());
        //题目是求右边第一个大于该数的数，后面往前遍历
        //倒序遍历,单调栈中维护当前位置右边的更大的元素列表，从栈底到栈顶的元素是单调递减的。
        for(int i=nums2.size()-1;i>=0;i--){
            int x=nums2[i];
            //当栈不为空并且栈顶元素stk.top()比x小，则栈顶元素不合法，弹出
            while(stk.size() && x>=stk.top()) stk.pop();
            //如果没有满足要求的则-1，否则满足就是栈顶元素
            if(stk.empty()) res[i]=-1;
            else res[i]=stk.top();
            //将当前元素加入栈
            stk.push(x);
        }
		
        //nums[i]和res[i]以及nums2[i]一一对应
        unordered_map<int,int> hash;
        for(int i=0;i<nums2.size();i++) hash[nums2[i]]=i;

        vector<int> ans;
        for(auto x:nums1){
            ans.push_back(res[hash[x]]);
        }
        return ans;
    }
};
```



#### [503.下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/)

给定一个循环数组 nums （ nums[nums.length - 1] 的下一个元素是 nums[0] ），返回 nums 中每个元素的 下一个更大元素 。

数字 x 的 下一个更大的元素 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1 。

**示例 1:**

输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。

**示例 2:**

输入: nums = [1,2,3,4,3]
输出: [2,3,4,-1,4]

**提示:**

- `1 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`

```c
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        //静态数组设置为环形数组形式，将当前数组复制一份，在加到一个新的数组后面
        //两份nums
        int n=nums.size();
        nums.insert(nums.end(),nums.begin(),nums.end());
        vector<int> res(n);
        stack<int> stk;
        //从后面往前面遍历找值
        for(int i=n*2-1;i>=0;i--){
            //如果当stk不为空并且x大于等于栈顶元素，则把栈顶元素删除
            int x=nums[i];
            while(stk.size() && x >= stk.top()) stk.pop();
            //最后n个进行统计，只需要求N个数
            if(i<n){
                if(stk.empty()) res[i]=-1;
                else res[i]=stk.top();
            }
            //否则将该元素加入到栈
            stk.push(x);
        }
        return res;
    }
};
```

#### [1475. 商品折扣后的最终价格](https://leetcode.cn/problems/final-prices-with-a-special-discount-in-a-shop/)

给你一个数组 prices ，其中 prices[i] 是商店里第 i 件商品的价格。

商店里正在进行促销活动，如果你要买第 i 件商品，那么你可以得到与 prices[j] 相等的折扣，其中 j 是满足 j > i 且 prices[j] <= prices[i] 的 最小下标 ，如果没有满足条件的 j ，你将没有任何折扣。

请你返回一个数组，数组中第 i 个元素是折扣后你购买商品 i 最终需要支付的价格。

**示例 1：**

输入：prices = [8,4,6,2,3]
输出：[4,2,4,2,3]
解释：
商品 0 的价格为 price[0]=8 ，你将得到 prices[1]=4 的折扣，所以最终价格为 8 - 4 = 4 。
商品 1 的价格为 price[1]=4 ，你将得到 prices[3]=2 的折扣，所以最终价格为 4 - 2 = 2 。
商品 2 的价格为 price[2]=6 ，你将得到 prices[3]=2 的折扣，所以最终价格为 6 - 2 = 4 。
商品 3 和 4 都没有折扣。

**示例 2：**

输入：prices = [1,2,3,4,5]
输出：[1,2,3,4,5]
解释：在这个例子中，所有商品都没有折扣。

**示例 3：**

输入：prices = [10,1,1,6]
输出：[9,0,1,6]

**提示：**

- `1 <= prices.length <= 500`
- `1 <= prices[i] <= 10^3`

```c
class Solution {
public:
    vector<int> finalPrices(vector<int>& prices) {
        vector<int> res(prices.size());
        stack<int> stk;
        for(int i=prices.size()-1;i>=0;i--){
            int x=prices[i];
            while(stk.size() && x<stk.top()) stk.pop();
            if(stk.empty()) res[i]=x; 
            else res[i]=x-stk.top();
            stk.push(x);
        }
        return res;
    }
};
```

```java
class Solution {
    public int[] finalPrices(int[] prices) {
        int n=prices.length;
        int[] res=new int[n];
        Stack<Integer> stk=new Stack();
        for(int i=n-1;i>=0;i--){
            int x=prices[i];
            while(!stk.empty() && x<stk.peek()) stk.pop();
            if(stk.empty()) res[i]=x;
            else res[i]=x-stk.peek();
            stk.push(x);
        }
        return res;
    }
}
```

#### [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/)

编写一个 StockSpanner 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 [100, 80, 60, 70, 60, 75, 85]，那么股票跨度将是 [1, 1, 1, 2, 1, 4, 6]。

**示例：**

```
输入：["StockSpanner","next","next","next","next","next","next","next"], [[],[100],[80],[60],[70],[60],[75],[85]]
输出：[null,1,1,1,2,1,4,6]
解释：
首先，初始化 S = StockSpanner()，然后：
S.next(100) 被调用并返回 1，
S.next(80) 被调用并返回 1，
S.next(60) 被调用并返回 1，
S.next(70) 被调用并返回 2，
S.next(60) 被调用并返回 1，
S.next(75) 被调用并返回 4，
S.next(85) 被调用并返回 6。

注意 (例如) S.next(75) 返回 4，因为截至今天的最后 4 个价格
(包括今天的价格 75) 小于或等于今天的价格。
```

提示：

调用 StockSpanner.next(int price) 时，将有 1 <= price <= 10^5。
每个测试用例最多可以调用  10000 次 StockSpanner.next。
在所有测试用例中，最多调用 150000 次 StockSpanner.next。
此问题的总时间限制减少了 50%。

**c++代码实现：**

```c
 class StockSpanner {
public:
    int k=0;    // 表示当前的位置
    stack<int> day,price;

    StockSpanner() {
        //添加哨兵防止边界问题
        day.push(-1);
        price.push(1e6);
    }
    
    int next(int x) {
        //找左边第一个比它大的数,price为栈底到栈顶的单调递减栈
        while(price.top()<=x){
            day.pop();
            price.pop();
        }
        int res=k-day.top();
        day.push(k++);
        price.push(x);
        return res;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```

#### [907. 子数组的最小值之和](https://leetcode.cn/problems/sum-of-subarray-minimums/)

给定一个整数数组 arr，找到 min(b) 的总和，其中 b 的范围为 arr 的每个（连续）子数组。

由于答案可能很大，因此 返回答案模 $10^9 + 7$ 。

**示例 1：**

```
输入：arr = [3,1,2,4]
输出：17
解释：
子数组为 [3]，[1]，[2]，[4]，[3,1]，[1,2]，[2,4]，[3,1,2]，[1,2,4]，[3,1,2,4]。 
最小值为 3，1，2，4，1，1，2，1，1，1，和为 17。
```

**示例 2：**

```
输入：arr = [11,81,94,43,3]
输出：444
```

**提示：**

- `1 <= arr.length <= 3 * 104`
- `1 <= arr[i] <= 3 * 104`

**c++代码：**

**本题目计算当前位置对整个答案的贡献度，当前的贡献*贡献多少次就是答案**

```c
class Solution {
public:
    /*
    每次计算当前的元素对整个答案的贡献
    l----------i----------r
    当前位置i,找到arr[i]左边第一个比它小的元素将坐标放到l[i]中
             找到arr[i]右边第一个比它大的元素将坐标放到r[i]中
    可能有多个元素满足条件，优先选择最先满足的元素
    */
    int sumSubarrayMins(vector<int>& arr) {
        int n=arr.size();
        typedef long long LL;
        const int MOD=1e9+7;
        int res=0;
        vector<int> l(n),r(n);

        stack<int> stk;

        for(int i=0;i<n;i++){
            /*
            如果栈顶元素值对应的arr的数值，比当前的arr[i]大的话，则出栈
            这里严格大于：
            因为如果stk.top()=j,arr[j]==arr[j-1]都满足的话，优先选择arr[j-1]
            如果相等的话，栈顶元素也比当前元素arr[i]小
            */
            while(stk.size() && arr[stk.top()] > arr[i]) stk.pop();
            //没有答案的时候填边界-1
            if(stk.empty()) l[i]=-1;
            else l[i]=stk.top();
            stk.push(i);
        }
        stk=stack<int>();
        for(int i=n-1;i>=0;i--){
            /*
            严格大于等于，右边的数比当前的大的话也是算大
            */
            while(stk.size() && arr[stk.top()]>=arr[i]) stk.pop();
            //没有找到的话边界为n
            if(stk.empty()) r[i]=n;
            else r[i]=stk.top();
            stk.push(i);
        }

        for(int i=0;i<n;i++){
            res=(res+(LL)arr[i]*(i-l[i])*(r[i]-i)) % MOD;
        }
        return res;
    }
};
```

#### [907相似题目891. 子序列宽度之和](https://leetcode.cn/problems/sum-of-subsequence-widths/)

一个序列的 宽度 定义为该序列中最大元素和最小元素的差值。

给你一个整数数组 nums ，返回 nums 的所有非空 子序列 的 宽度之和 。由于答案可能非常大，请返回对 109 + 7 取余 后的结果。

子序列 定义为从一个数组里删除一些（或者不删除）元素，但不改变剩下元素的顺序得到的数组。例如，[3,6,2,7] 就是数组 [0,3,1,6,2,2,7] 的一个子序列。

**示例 1：**

```
输入：nums = [2,1,3]
输出：6
解释：子序列为 [1], [2], [3], [2,1], [2,3], [1,3], [2,1,3] 。
相应的宽度是 0, 0, 0, 1, 1, 2, 2 。
宽度之和是 6 。
```

**示例 2：**

```
输入：nums = [2]
输出：0
```

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

**c++实现：**

```c
const int N = 1e5+5,MOD=1e9+7;
typedef long long LL;
int p[N];
class Solution {
public:
    /*
        0------i------n
        将nums排序之后，选择i，计算对答案的贡献度是多少
        当i对应的是最大值的时候，则i的贡献值是加，左边位置i的选择有2^i
        当i对应的是最小值的时候，则i的贡献度是减，右边位置i的选择有-2^(n-i-1)
    */
    int sumSubseqWidths(vector<int>& nums) {
        int n=nums.size();
        p[0]=1;
        for(int i=1;i<=n;i++) p[i]=p[i-1]*2 % MOD; 

        int res = 0;
        sort(nums.begin(),nums.end());

        for(int i=0;i<n;i++){
            res=(res+(LL)nums[i]*p[i]-(LL)nums[i]*p[n-i-1]) % MOD;
        }
        return res;
    }
};
```



## 五、单调队列

#### [862. 和至少为 K 的最短子数组](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/)todo

相似题目[560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/)

### 1、滑动窗口

#### [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

**示例 1：**

```c
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```c
输入：nums = [1], k = 1
输出：[1]
```

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

**c++代码实现：**

```c
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        //滑动窗口的队列存的是数字下标
        deque<int> q;

        for(int i=0;i<nums.size();i++){
            //当前的位置是i,那么i的前面k的位置的坐标是i-k+1
            //deque的front对应是nums[0],在back的位置进行加入nums[i]
            if(q.size() && i-k+1>q.front()) q.pop_front();

            //deque维护的是一个从front到back单调下降的队列
            //当nums[i]的值是大于队列的队列尾部的数，则将队列的尾部的数据出队列
            while(q.size() && nums[i] >= nums[q.back()]) q.pop_back();
            q.push_back(i);

            if(i>=k-1) res.push_back(nums[q.front()]);
        }
        return res;
    }
};
```

#### [1425. 带限制的子序列和](https://leetcode.cn/problems/constrained-subsequence-sum/)（todo）

## 六、贪心

### 1、区间问题

#### [646. 最长数对链](https://leetcode.cn/problems/maximum-length-of-pair-chain/)

给出 n 个数对。 在每一个数对中，第一个数字总是比第二个数字小。

现在，我们定义一种跟随关系，当且仅当 b < c 时，数对(c, d) 才可以跟在 (a, b) 后面。我们用这种形式来构造一个数对链。

给定一个数对集合，找出能够形成的最长数对链的长度。你不需要用到所有的数对，你可以以任何顺序选择其中的一些数对来构造。

 **示例：**

```
输入：[[1,2], [2,3], [3,4]]
输出：2
解释：最长的数对链是 [1,2] -> [3,4]
```

**提示：**

- 给出数对的个数在 `[1, 1000]` 范围内。

```c
class Solution {
public:
    /*
    1.将所有区间按照右端点排序
    2.从前往后依次枚举每一个区间（单峰的时候可以用贪心）
    当上一个区间的r和当前区间l没有交集的时候才能选择

    贪心解==最优解
    1.贪心解<=最优解
    2.贪心解>=最优解
    证明：
    任意给一组最优解，找到第一个不一样的最优解，如果没有则能证明贪心解>=最优解
    （前面都是一样的，到了某一步不满足了）

    相似推荐：AcWing 908. 最大不相交区间数量

    */
    int findLongestChain(vector<vector<int>>& pairs) {
        int n=pairs.size();
        sort(pairs.begin(),pairs.end(),[](vector<int>&a ,vector<int>& b){
            return a[1]<b[1];
        });

        //至少都有一个区间满足，就是选择第一个区间
        int res=1;
        int ed=pairs[0][1]; // 用来记录第一个区间的结束位置

        for(auto& x:pairs){
            int l=x[0],r=x[1];
            if(l>ed){
                res++;
                ed=r;
            }
        }
        return res;
    }
};
```

```java
class Solution {
     /*
    1.将所有区间按照右端点排序
    2.从前往后依次枚举每一个区间（单峰的时候可以用贪心）
    当上一个区间的r和当前区间l没有交集的时候才能选择

    贪心解==最优解
    1.贪心解<=最优解
    2.贪心解>=最优解
    证明：
    任意给一组最优解，找到第一个不一样的最优解，如果没有则能证明贪心解>=最优解
    （前面都是一样的，到了某一步不满足了）

    相似推荐：AcWing 908. 最大不相交区间数量

    */
    public int findLongestChain(int[][] pairs) {
        Arrays.sort(pairs,(a,b)->a[1]-b[1]);

        //至少都有一个区间满足，就是选择第一个区间
        int res=1;

        // ed用来记录第一个区间的结束位置
        int ed=pairs[0][1];

        for(int[] x:pairs){
            int l=x[0],r=x[1];
            if(l>ed){
                res++;
                ed=r;
            }
        }
        return res;
    }
}
```



## 七、动态规划

### 1、DP理论基础

#### （1）最长上升子序列

#### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

```c
class Solution {
public:
    /*
    DP
    1.状态表示 f[i]
    	1.1 集合:所有以第i个数结尾的上升子序列的集合
    	1.2 属性：集合里面每一个上升子序列的长度的最大值Max
    2.状态计算
        集合划分f[i]:0 1 2 3 ....i-1
        (1)集合里面没有数
        (2)集合里面以第1个数结尾，以第2个数结尾，以第3个数结尾，...
        状态转移方程:a[i]=max(f[i],f[j]+1)  a[j]<a[i]  j=0,1,2,..i-1
        时间复杂度=状态数量X转移的计算量=n*n
    */
    int lengthOfLIS(vector<int>& nums) {
        int n=nums.size();
        vector<int> f(n+1);
        vector<int> g(n+1);

        for (int i = 1; i <= n; i++)
        {
            f[i] = 1; //以i结尾的只有一个数a[i]
            g[i] = 0; //表示只有一个数
            for (int j = 1; j < i; j++)
            {
                if (nums[j] < nums[i])
                {
                    if (f[i] < f[j] + 1)
                    {
                        f[i] = f[j] + 1;
                        //记录每个状态是从哪里转移来的
                        g[i] = j;
                    }
                }
            }
        }

        //最优解下标
        int k = 1;
        for (int i = 1; i <= n; i++)
            if (f[k] < f[i])
                k = i;

        return  f[k];  
    }
};
```

```c
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> q;
        for (auto x: nums) {
            if (q.empty() || x > q.back()) q.push_back(x);
            else {
                if (x <= q[0]) q[0] = x;
                else {
                    int l = 0, r = q.size() - 1;
                    while (l < r) {
                        int mid = l + r + 1 >> 1;
                        if (q[mid] < x) l = mid;
                        else r = mid - 1;
                    }
                    q[r + 1] = x;
                }
            }
        }
        return q.size();
    }
};
```



#### [1235. 规划兼职工作](https://leetcode.cn/problems/maximum-profit-in-job-scheduling/)

你打算利用空闲时间来做兼职工作赚些零花钱。

这里有 n 份兼职工作，每份工作预计从 startTime[i] 开始到 endTime[i] 结束，报酬为 profit[i]。

给你一份兼职工作表，包含开始时间 startTime，结束时间 endTime 和预计报酬 profit 三个数组，请你计算并返回可以获得的最大报酬。

注意，时间上出现重叠的 2 份工作不能同时进行。

如果你选择的工作在时间 X 结束，那么你可以立刻进行在时间 X 开始的下一份工作。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample1_1584.png)

```
输入：startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
输出：120
解释：
我们选出第 1 份和第 4 份工作， 
时间范围是 [1-3]+[3-6]，共获得报酬 120 = 50 + 70。
```

**示例 2：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample22_1584.png)

```
输入：startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
输出：150
解释：
我们选择第 1，4，5 份工作。 
共获得报酬 150 = 20 + 70 + 60。
```

**示例 3：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/10/19/sample3_1584.png)

```
输入：startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
输出：6
```

**提示：**

$1 <= startTime.length == endTime.length == profit.length <= 5 * 10^4$,
$1 <= startTime[i] < endTime[i] <= 10^9$,
$1 <= profit[i] <= 10^4$

**c++代码实现：**

```c
class Solution {
public:
    /*
    DP:
    （1）按照右端点排序
     (2) f[i]表示第i个区间的最大值
        1)当i不满足的时候，f[i]=f[i-1]
        2)当i区间满足的时候，则需要找到i区间左端l更小的区间j，使得j.r<=i.l
            由于数据范围很大，找j的时候不能枚举，使用二分来找j左边第一个满足条件的区间
    */
    struct Jobs{
        int l,r,w;
        bool operator<(const Jobs& t)const{
            return r<t.r;
        }
    };

    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        int n=startTime.size();
        vector<Jobs> jobs;
        //获取数据
        for(int i=0;i<n;i++) jobs.push_back({startTime[i],endTime[i],profit[i]});
        //按照右端点排序
        sort(jobs.begin(),jobs.end());
        //状态数组
        vector<int> f(n);
        f[0]=jobs[0].w; //初始满足第一个区间
        for(int i=1;i<n;i++){
            //(1)当区间i不满足条件的时候
            f[i]=max(f[i-1],jobs[i].w);
            //(2)当区间i满足的时候，二分找到i区间左边第一个满足条件的区间j
            if(jobs[0].r<=jobs[i].l){
                int l=0,r=i-1;
                while(l<r){
                    int mid = l+r+1>>1;
                    if(jobs[mid].r<=jobs[i].l) l=mid;
                    else r=mid-1;
                }
                f[i]=max(f[i],f[r]+jobs[i].w);
            }
        }
        return f[n-1];
    }
};
```

#### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)todo



#### （2）字符串匹配问题

#### [10. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)todo

#### [44. 通配符匹配](https://leetcode.cn/problems/wildcard-matching/)todo

#### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)todo





#### [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)

给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。

字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）

题目数据保证答案符合 32 位带符号整数范围。

**示例 1：**

```
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

**示例 2：**

```
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下图所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag
```

**提示：**

- `0 <= s.length, t.length <= 1000`
- `s` 和 `t` 由英文字母组成

**c++代码实现**

```c
class Solution {
public:
    /*
    DP
    1、状态表示
    （1）集合：f(i，j)表示s[0]~s[i]中和t[0]~t[j]中相互匹配的满足条件的部分
    （2）属性：count满足s中有多少个t
    2、状态计算
    集合的划分：
    将s序列考虑是否加s[i]
    （1）当不选则s[i]
    	相当于 s 中新加入的第 i 个字符 s[i−1] 没有起到作用
        则f[i][j]=f[i-1][j]
    （2）当选s[i]
        此时s[i]==t[j],则f[i][j]+=f[i-1][j-1]
    */
    int numDistinct(string s, string t) {
        int n=s.size(),m=t.size();
        //下标从1开始
        s=' '+s,t=' '+t;
        vector<vector<unsigned long long>> f(n+1,vector<unsigned long long>(m+1));

        //处理边界情况
        //将 s[:j] 变成空字符 ' ' 只有一种操作：删除s[:j] 中的全部元素 ；
        for(int i=0;i<=n;i++) f[i][0]=1;

        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++){
                f[i][j]=f[i-1][j];
                if(s[i]==t[j]){
                    f[i][j]+=f[i-1][j-1];
                }
            }
        return f[n][m];
    }
};
```

#### [剑指 Offer II 097. 子序列的数目](https://leetcode.cn/problems/21dk04/)todo

**和115题目相同**

#### [583. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/)todo



#### （3）数字三角形

#### [62. 不同路径](https://leetcode.cn/problems/unique-paths/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

**示例 1：**

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```c
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

**提示：**

- `1 <= m, n <= 100`
- 题目数据保证答案小于等于 `2 * 109`

**c++代码：**

```c
class Solution {
public:
    /*
    边界f[0][0]=1
    当i=0的时候，只能向右边走
    当j=0的时候，只能向下边走
    其余部分：f[i][j]=f[i-1][j]+f[i][j-1]    
    */
    int uniquePaths(int m, int n) {
        vector<vector<int>> f(m,vector<int>(n));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(!i && !j) f[i][j]=1;
                else{
                    if(i) f[i][j]+=f[i-1][j];
                    if(j) f[i][j]+=f[i][j-1];
                }
            }
        }
        return f[m-1][n-1];
    }
};
```

#### [63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

**提示：**

$m == obstacleGrid.length$,
$n == obstacleGrid[i].length$,
$1 <= m, n <= 100$,
$obstacleGrid[i][j] $为 0 或 1

**c++代码实现：**

```c
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& g) {
        int n=g.size(),m=g[0].size();
        vector<vector<int>> f(n,vector<int>(m));
        if(g[n-1][m-1]) return 0;
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(!g[i][j]){
                    if(!i && !j) f[i][j]=1;
                    else{
                        if(i) f[i][j]+=f[i-1][j];
                        if(j) f[i][j]+=f[i][j-1]; 
                    }
                }
            }
        }
        return f[n-1][m-1];
    }
};
```



#### [980. 不同路径 III](https://leetcode.cn/problems/unique-paths-iii/)（连通性DP-插头DP）

在二维网格 grid 上，有 4 种类型的方格：

1 表示起始方格。且只有一个起始方格。
2 表示结束方格，且只有一个结束方格。
0 表示我们可以走过的空方格。
-1 表示我们无法跨越的障碍。
返回在四个方向（上、下、左、右）上行走时，从起始方格到结束方格的不同路径的数目。

每一个无障碍方格都要通过一次，但是一条路径中不能重复通过同一个方格。

**示例 1：**

```c
输入：[[1,0,0,0],[0,0,0,0],[0,0,2,-1]]
输出：2
解释：我们有以下两条路径：
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)
```

**示例 2：**

```
输入：[[1,0,0,0],[0,0,0,0],[0,0,0,2]]
输出：4
解释：我们有以下四条路径： 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)
```

**示例 3：**

```
输入：[[0,1],[2,0]]
输出：0
解释：
没有一条路能完全穿过每一个空的方格一次。
请注意，起始和结束方格可以位于网格中的任意位置。
```

**提示：**

- `1 <= grid.length * grid[0].length <= 20`

**c++代码实现：**

```c
class Solution {
public:
    vector<vector<int>> g;
    int n,m,k;  // k表示当前还有多少格子需要遍历
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    int uniquePathsIII(vector<vector<int>>& grid) {
        g=grid;
        n=g.size(),m=g[0].size();
        int stx,sty;    //起始坐标
        k=0;

        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(g[i][j]==1) stx=i,sty=j,k++;
                else if(!g[i][j]) k++;  // 0表示需要走过得空方格
            }
        }
        return dfs(stx,sty);
    }

    int dfs(int x,int y){
        //到达目的地了，并且都走完了
        if(g[x][y]==2){
            if(!k) return 1;
            return 0;
        }
        int res=0;
        g[x][y]=-1,k--;
        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a>=0 && a<n && b>=0 && b<m && g[a][b]!=-1){
                res+=dfs(a,b);
            }
        }
        //恢复现场
        g[x][y]=0,k++;
        return res;
    }
};
```

#### [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
输入：n = 3
输出：5
```

**示例 2：**

```
输入：n = 1
输出：1
```

**提示：**

- `1 <= n <= 19`

**c++代码实现：**

```c
class Solution {
public:
    /*
    方法一：直接用卡特兰公式
    方法二：暴力搜索
        假设根节点root是数字K，则root是在L-R之间的一个数[L----k-1 -- k -- k+1----R]，
        左子树[L,k-1],右子树[k+1,R]
        从左子树left和右子树right中分别选择一个节点作为root的左右子树
        方案数:f(left)*f(right)
        left=k-1-L+1=k-L
        right=R-(k+1)+1=R-k
        f(i)=求和f(j-L)*f(R-j);
    */
    int numTrees(int n) {
        vector<int> f(n+1);
        f[0]=1;     //左子树为空
        for(int i=1;i<=n;i++){
            for(int j=1;j<=i;j++){
                f[i]+=f[j-1]*f[i-j];
            }
        }
        return f[n];
    }
};
```



#### [931. 下降路径最小和](https://leetcode.cn/problems/minimum-falling-path-sum/)

给你一个 n x n 的 方形 整数数组 matrix ，请你找出并返回通过 matrix 的下降路径 的 最小和 。

下降路径 可以从第一行中的任何元素开始，并从每一行中选择一个元素。在下一行选择的元素和当前行所选元素最多相隔一列（即位于正下方或者沿对角线向左或者向右的第一个元素）。具体来说，位置 (row, col) 的下一个元素应当是 (row + 1, col - 1)、(row + 1, col) 或者 (row + 1, col + 1) 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/11/03/failing1-grid.jpg)



```
输入：matrix = [[2,1,3],[6,5,4],[7,8,9]]
输出：13
解释：如图所示，为和最小的两条下降路径
```

**示例 2：**

```
输入：matrix = [[-19,57],[-40,-5]]
输出：-59
解释：如图所示，为和最小的下降路径
```

![](https://assets.leetcode.com/uploads/2021/11/03/failing2-grid.jpg)

**提示：**

$n == matrix.length == matrix[i].length$,
$1 <= n <= 100$,
$-100 <= matrix[i][j] <= 100$

**c++代码实现：**

```c
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& g) {
        int n = g.size();
        vector<vector<int>> dp(n,vector<int>(n,1e8));
        for(int i = 0; i < n; i++) dp[0][i] = g[0][i];
        for(int i = 1; i < n; i++) {
            for(int j = 0; j < n; j++) {
                for(int  k = max(0,j-1); k <= min(n-1,j+1); k++) {
                    dp[i][j] = min(dp[i][j], dp[i-1][k] + g[i][j]);
                }
            }
        }
        int res = 1e8;
        for(int i = 0; i < n; i++) res = min(res,dp[n-1][i]);
        return res;
    }
};
```



#### [343. 整数拆分](https://leetcode.cn/problems/integer-break/)

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

 **示例 1:**

```c
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

**示例 2:**

```
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

**提示:**

- `2 <= n <= 58`

**c++代码实现：**

```c
class Solution {
public:
    /*
    设将n可以分为两部分 j 和 n-j,定义f[n]表示为数字n能划分得到得最大得乘积
    如果n-j不能在划分，则答案为j*(n-j)
    如果n-j能继续划分，则答案为j*f[n-j]
    综上f[n]=max( f[n], max(j*(n-j) , j*f[n-j]))
    */
    int integerBreak(int n) {
        vector<int> f(n+1);
        
        for(int i=2;i<=n;i++){
            for(int j=1;j<=i;j++){
                f[i]=max(f[i],max(j*(i-j),j*f[i-j]));
            }
        }
        return f[n];
    }
};
```

#### [509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)

**斐波那契数** （通常用 `F(n)` 表示）形成的序列称为 **斐波那契数列** 。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
```

给定 `n` ，请计算 `F(n)` 。

**示例 1：**

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

**示例 2：**

```
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

**示例 3：**

```
输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

**提示：**

- `0 <= n <= 30`

**c++代码实现：**

```c
class Solution {
public:
    int fib(int n) {
        int a=0,b=1;
        while(n--){
            int c=a+b;
            a=b,b=c;
        }
        return a;
    }
};
```

#### [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

**提示：**

- `1 <= n <= 45`

**c++代码实现：**

```c
class Solution {
public:
    int climbStairs(int n) {
        int a=1,b=1;
        //执行n-1次
        /*
        a b c 
          a b c
        1 1 2 3 5 8 13 21 34...
        */
        while(--n){
            int c=a+b;
            a=b,b=c;
        }
        return b;
    }
};
```

#### 

#### [799. 香槟塔](https://leetcode.cn/problems/champagne-tower/)

我们把玻璃杯摆成金字塔的形状，其中 第一层 有 1 个玻璃杯， 第二层 有 2 个，依次类推到第 100 层，每个玻璃杯 (250ml) 将盛有香槟。

从顶层的第一个玻璃杯开始倾倒一些香槟，当顶层的杯子满了，任何溢出的香槟都会立刻等流量的流向左右两侧的玻璃杯。当左右两边的杯子也满了，就会等流量的流向它们左右两边的杯子，依次类推。（当最底层的玻璃杯满了，香槟会流到地板上）

例如，在倾倒一杯香槟后，最顶层的玻璃杯满了。倾倒了两杯香槟后，第二层的两个玻璃杯各自盛放一半的香槟。在倒三杯香槟后，第二层的香槟满了 - 此时总共有三个满的玻璃杯。在倒第四杯后，第三层中间的玻璃杯盛放了一半的香槟，他两边的玻璃杯各自盛放了四分之一的香槟，如下图所示。

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/09/tower.png)

现在当倾倒了非负整数杯香槟后，返回第 `i` 行 `j` 个玻璃杯所盛放的香槟占玻璃杯容积的比例（ `i` 和 `j` 都从0开始）。

**示例 1:**

```
输入: poured(倾倒香槟总杯数) = 1, query_glass(杯子的位置数) = 1, query_row(行数) = 1
输出: 0.00000
解释: 我们在顶层（下标是（0，0））倒了一杯香槟后，没有溢出，因此所有在顶层以下的玻璃杯都是空的。
```

**示例 2:**

```
输入: poured(倾倒香槟总杯数) = 2, query_glass(杯子的位置数) = 1, query_row(行数) = 1
输出: 0.50000
解释: 我们在顶层（下标是（0，0）倒了两杯香槟后，有一杯量的香槟将从顶层溢出，位于（1，0）的玻璃杯和（1，1）的玻璃杯平分了这一杯香槟，所以每个玻璃杯有一半的香槟。
```

**示例 3:**

```
输入: poured = 100000009, query_row = 33, query_glass = 17
输出: 1.00000
```

**提示:**

- `0 <= poured <= 109`
- `0 <= query_glass <= query_row < 100`

**c++代码：**

```c
class Solution {
public:
    /*
    线性DP:
    (i,j)
    (i+1,j)  (i+1,j+1)

    初始f(0,0)=poured
    if f(i,j)>1:
        x=(f(i,j)-1)/2
    f(i+1,j)+=x;
    f(i+1,j+1)+=x;
    */ 
    double champagneTower(int poured, int query_row, int query_glass) {
        vector<vector<double>> f(query_row+1,vector<double>(query_row+1));
        f[0][0]=poured;
        for(int i=0;i<query_row;i++){
            for(int j=0;j<=i;j++){
                if(f[i][j]>1){
                    double x=(f[i][j]-1)/2;
                    f[i+1][j]+=x;
                    f[i+1][j+1]+=x;
                }
            }
        }
        return min(1.0,f[query_row][query_glass]);
    }
};
```

#### [264. 丑数 II](https://leetcode.cn/problems/ugly-number-ii/)

给你一个整数 `n` ，请你找出并返回第 `n` 个 **丑数** 。

**丑数** 就是只包含质因数 `2`、`3` 和/或 `5` 的正整数。

**示例 1：**

```
输入：n = 10
输出：12
解释：[1, 2, 3, 4, 5, 6, 8, 9, 10, 12] 是由前 10 个丑数组成的序列。
```

**示例 2：**

```
输入：n = 1
输出：1
解释：1 通常被视为丑数。
```

**提示：**

- `1 <= n <= 1690`

**c++代码：**

```c
class Solution {
public:
    /*
    多路归并---->最后相当于都是在S操作（三个指针*2/3/5）
    S2(包含2的丑数):1*2,2*2,3*2。。。。。===S*2
    S3(包含3的丑数)：1*3，2*3，3*3。。。。===S*3
    S5：1*5，2*5，3*5 。。。===S*5
    S=1+s2+s3+s5
    */
    int nthUglyNumber(int n) {
        vector<int> a(1,1);
        for(int i=0,j=0,k=0;a.size()<n;){
            int res=min(a[i]*2,min(a[j]*3,a[k]*5));
            a.push_back(res);
            if(a[i]*2==res) i++;
            if(a[j]*3==res) j++;
            if(a[k]*5==res) k++;
        }
        return a.back();
    }
};
```

#### [790. 多米诺和托米诺平铺](https://leetcode.cn/problems/domino-and-tromino-tiling/)todo

#### [313. 超级丑数](https://leetcode.cn/problems/super-ugly-number/)todo  mid

#### [91. 解码方法](https://leetcode.cn/problems/decode-ways/)todo  mid

#### [639. 解码方法 II](https://leetcode.cn/problems/decode-ways-ii/)todo  hard

#### [823. 带因子的二叉树](https://leetcode.cn/problems/binary-trees-with-factors/)todo  mid

#### [221. 最大正方形](https://leetcode.cn/problems/maximal-square/)todo mid

#### [1277. 统计全为 1 的正方形子矩阵](https://leetcode.cn/problems/count-square-submatrices-with-all-ones/)todo mid

#### [600. 不含连续1的非负整数](https://leetcode.cn/problems/non-negative-integers-without-consecutive-ones/)todo  hard

#### [818. 赛车](https://leetcode.cn/problems/race-car/)todo hard

#### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/)（DfS+回溯）

#### [837. 新 21 点](https://leetcode.cn/problems/new-21-game/)todo mid

#### [887. 鸡蛋掉落](https://leetcode.cn/problems/super-egg-drop/)todo hard

#### （4）DP在字符串的应用

#### [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)（样本对应模型）

给你两个单词 word1 和 word2， 请返回将 word1 转换成 word2 所使用的最少操作数  。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

**提示：**

- `0 <= word1.length, word2.length <= 500`
- `word1` 和 `word2` 由小写英文字母组成

**java代码：**

```java
class Solution {
    /* 
    大厂刷题班5（样本对应模型）
    dp[i][j] 集合表示 s1 前缀 i 个; s2 前缀 j 个
    （1）dp[0][0] = 0;
    （2）dp[0][i]第一行是删除s1
    （3）dp[j][0]第一列是增加s1
    （4）dp[i][j]表示对应s1[0.....i-1],对应s2[1.....j-1]
        1）dp[i-1][j] + del 表示将 s1 的前面0至i-1变成 s2 的前面 0至j，最后将s1[i]删除
        2）dp[i][j-1] + add 表示将 s1 的前面0至i 变成 s2 的前面 0至j-1，最后将s1 加上s2 最后一个字符
        3）s1[i-1] == s2[j-1]，则只需要dp[i-1][j-1]
        4）s1[i-1] != s2[j-1],dp[i-1][j-1] + replace
    */
    public int minDistance(String str1, String str2) {
        int N = str1.length(), M = str2.length();
        char[] s1 = str1.toCharArray();
        char[] s2 = str2.toCharArray();
        int dp[][] = new int[N+1][M+1];
        // dp[0][0] = 0;
        for(int i = 1; i <= N; i++) dp[i][0] = i * 1; //增加的代价为1
        for(int j = 1; j <= M; j++) dp[0][j] = j * 1; //删除的代价为1
        for(int i = 1; i <= N; i++) {
            for(int j = 1; j <= M; j++) {
                if(s1[i-1] == s2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                }else {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                dp[i][j] = Math.min(dp[i][j], dp[i-1][j] + 1);
                dp[i][j] = Math.min(dp[i][j], dp[i][j-1] + 1);
            }
        }
        return dp[N][M];
    }
}
```

**c++代码：**

```c
class Solution {
public:
    int minDistance(string s1, string s2) {
        int n = s1.size(), m = s2.size();
        vector<vector<int>> dp(n + 1,vector<int>(m + 1));
        for(int i = 0; i <= n; i++) dp[i][0] = i;
        for(int j = 0; j <= m; j++) dp[0][j] = j;
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                dp[i][j] = min(dp[i-1][j],min(dp[i][j-1],dp[i-1][j-1])) + 1;
                if(s1[i-1] == s2[j-1]) dp[i][j] = min(dp[i][j], dp[i-1][j-1]);
            }
        }
        return dp[n][m];
    }
};
```

**python代码：**

```python
class Solution:
    def minDistance(self, a: str, b: str) -> int:
        n , m = len(a) , len(b)
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        for i in range (n + 1) :
            dp[i][0] = i;
        for j in range (m + 1) :
            dp[0][j] = j;
        
        for i in range (1, n + 1) :
            for j in range (1, m + 1) :
                dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1;
                if a[i-1] == b[j-1]:
                    dp[i][j] = min(dp[i][j], dp[i-1][j-1])
        return dp[n][m]
```

#### [712. 两个字符串的最小ASCII删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/)(样本对应模型)

给定两个字符串`s1` 和 `s2`，返回 *使两个字符串相等所需删除字符的 **ASCII** 值的最小和* 。

**示例 1:**

```
输入: s1 = "sea", s2 = "eat"
输出: 231
解释: 在 "sea" 中删除 "s" 并将 "s" 的值(115)加入总和。
在 "eat" 中删除 "t" 并将 116 加入总和。
结束时，两个字符串相等，115 + 116 = 231 就是符合条件的最小和。
```

**示例 2:**

```
输入: s1 = "delete", s2 = "leet"
输出: 403
解释: 在 "delete" 中删除 "dee" 字符串变成 "let"，
将 100[d]+101[e]+101[e] 加入总和。在 "leet" 中删除 "e" 将 101[e] 加入总和。
结束时，两个字符串都等于 "let"，结果即为 100+101+101+101 = 403 。
如果改为将两个字符串转换为 "lee" 或 "eet"，我们会得到 433 或 417 的结果，比答案更大。
```

**提示:**

- `0 <= s1.length, s2.length <= 1000`
- `s1` 和 `s2` 由小写英文字母组成

**java代码实现：**

```java
class Solution {
    /* 
    大厂刷题班5（样本对应模型）
    dp[i][j] 集合表示 s1 前缀 i 个; s2 前缀 j 个
    （1）dp[0][0] = 0;
    （2）dp[0][i]第一行是删除s1
    （3）dp[j][0]第一列是增加s1
    （4）dp[i][j]表示对应s1[0.....i-1],对应s2[1.....j-1]
        1）dp[i-1][j] + del 表示将 s1 的前面0至i-1变成 s2 的前面 0至j，最后将s1[i]删除
        2）dp[i][j-1] + add 表示将 s1 的前面0至i 变成 s2 的前面 0至j-1，最后将s1 加上s2 最后一个字符
        3）s1[i-1] == s2[j-1]，则只需要dp[i-1][j-1]
        4）s1[i-1] != s2[j-1],dp[i-1][j-1] + replace
    */
    public int minimumDeleteSum(String s1, String s2) {
        int N = s1.length(), M = s2.length();
        int dp[][] = new int[N+1][M+1];
        // dp[0][0] = 0;
        for(int i = 1; i <= N; i++) dp[i][0] = dp[i-1][0] + s1.codePointAt(i-1); //增加的代价为
        for(int j = 1; j <= M; j++) dp[0][j] = dp[0][j-1] + s2.codePointAt(j-1); //删除的代价为
        for(int i = 1; i <= N; i++) {
            for(int j = 1; j <= M; j++) {
                if(s1.codePointAt(i-1) == s2.codePointAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1];
                }else {
                    dp[i][j] = Math.min(dp[i][j-1] + s2.codePointAt(j-1), dp[i-1][j] + s1.codePointAt(i-1));
                }
            }
        }
        return dp[N][M];
    }
}
```

**c++代码：**

```c
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int N = s1.size(), M = s2.size();
        vector<vector<int>> dp(N+1,vector<int>(M+1,0));
        // dp[0][0] = 0;
        for(int i = 1; i <= N; i++) dp[i][0] = dp[i-1][0] + s1[i-1]; //增加的代价为
        for(int j = 1; j <= M; j++) dp[0][j] = dp[0][j-1] + s2[j-1]; //删除的代价为
        for(int i = 1; i <= N; i++) {
            for(int j = 1; j <= M; j++) {
                if(s1[i-1] == s2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                }else {
                    dp[i][j] = min(dp[i][j-1] + s2[j-1], dp[i-1][j] + s1[i-1]);
                }
            }
        }
        return dp[N][M];
    }
};
```



#### [516. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/)

给你一个字符串 s ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

**示例 1：**

```
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
```

**示例 2：**

```
输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb" 。
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由小写英文字母组成

**c++代码：**

**TLE**：

```c
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        if(s == ""  || s.size() == 0) {
            return 0;
        }
        return dfs(s, 0, s.size() - 1);
    }

    int dfs(string s, int l, int r) {
        if(l == r) return 1; //只有一个字符
        if(l == r-1) return s[l] == s[r] ? 2 : 1; //两个字符的时候
        //不包含l，不包含r
        int p1 = dfs(s,l + 1,r - 1);
        //不包含l，包含r
        int p2 = dfs(s, l + 1, r);
        //包含l，不包含r
        int p3 = dfs(s, l, r - 1);
        //包含l，包含r
        int p4 = s[l] == s[r] ? (2 + dfs(s, l + 1, r - 1)) : 0;
        return max(max(p1, p2), max(p3, p4));
    }
};
```

**y总：**

按照长度枚举

```c
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        vector<vector<int>> f(n,vector<int>(n));
        //按照步长来遍历
        for(int len = 1; len <= n; len++) {
            //i不超过len的长度
            for(int i = 0;i + len -1 < n; i++) {
                int j = i+ len -1;
                if(len == 1) f[i][j] = 1; //只有一个字母的时候
                else {
                    if(s[i] == s[j]) f[i][j] = f[i+1][j-1] + 2;
                    f[i][j] = max(f[i][j], max(f[i+1][j], f[i][j-1]));
                }
            }
        }
        return f[0][n-1];
    }
};
```

**左神：**

```c
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        if(s == ""  || s.size() == 0) {
            return 0;
        }
        int n = s.size();
        vector<vector<int>> f(n,vector<int>(n));
        //长度为1
        for(int i = 0; i < n; i++) {
            f[i][i] = 1; // 表示长度为1的
        }
        //长度为2，也就是l == r-1
        for(int i = 0; i < n-1; i++) f[i][i+1] = s[i]==s[i+1] ? 2 : 1;

        //其余长度的，按照递归写法，应该是从下到上，从左到右递推
        //最后一行的对角线已经求出
        for(int l = n -1; l >= 0; l--) {
            //长度为1和2的已经求出
            for(int r = l+2; r < n; r++) {
                //不包含l，不包含r
                int p1 = f[l + 1][r - 1];
                //不包含l，包含r
                int p2 = f[l + 1][r];
                //包含l，不包含r
                int p3 = f[l][r - 1];
                //包含l，包含r
                int p4 = s[l] == s[r] ? (2 + f[l + 1][r - 1]) : 0;
                f[l][r] = max(max(p1, p2), max(p3, p4));
            }
        }
        return f[0][n-1];
    }
};
```

**java代码实现：**

```java
package class21;

/*
leetcode 516. 最长回文子序列
给定一个字符串str,返回这个字符串的最长回文子序列长度
比如：str=“a12b3c43def2ghi1kpm"
最长回文子序列是“1234321”或者“123c321”，
返回长度 7
* */
public class Code01_PalindromeSubsequence {
    //1、一个字符串及其逆序串的最长公共子序列，就是原始字符串的最长回文子序列

    public static int lpsl1(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        char[] str = s.toCharArray();
        return f(str, 0, str.length - 1);
    }

    //str[L,R]最长回文子序列长度
    public static int f(char[] str,int l, int r) {
        //base case
        if (l == r) return 1;
        if ( l == r - 1){ //两个字符的情况
            return str[l] == str[r] ? 2 : 1;
        }
        //1、l,r不包含
        int p1 = f(str, l+1, r-1);
        //2、l包含，不包含r
        int p2 = f(str, l, r-1);
        //3、l不包含，r包含
        int p3 = f(str, l+1, r);
        //4、l，r包含
        int p4 =str[l] != str[r] ? 0 : ( 2 + f(str, l+1, r-1));
        return Math.max(Math.max(p1,p2),Math.max(p3,p4));
    }

    public static int lpsl2(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        char[] str = s.toCharArray();
        int N = str.length;
        //L 0....N-1  行
        //R 0....N-1  列
        int[][] dp = new int[N][N];
        //先填对角线的最后一个格子，方便下面填对角线及相邻对角线
        dp[N-1][N-1] = 1;
        for(int i = 0; i < N-1; i ++) {
            dp[i][i] = 1;
            //两个字符的情况str[l] == str[r-1]
            dp[i][i+1] = str[i] == str[i+1] ? 2 : 1;
        }
        //上三角按照从下到上，从左向右顺序填，看f的几个依赖关系可以得到
        //从N-3行填
        for(int L = N-1; L >=0; L--) {
            for(int R = L + 2; R < N; R++) {
                //1、l,r不包含
                int p1 = dp[L+1][R-1];
                //2、l包含，不包含r
                int p2 = dp[L][R-1];
                //3、l不包含，r包含
                int p3 = dp[L+1][R];
                //4、l，r包含
                int p4 =str[L] != str[R] ? 0 : ( 2 + dp[L+1][R-1]);
                dp[L][R] = Math.max(Math.max(p1,p2),Math.max(p3,p4));
            }
        }
        return dp[0][N-1];
    }

    public static int lpsl3(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        char[] str = s.toCharArray();
        int N = str.length;
        //L 0....N-1  行
        //R 0....N-1  列
        int[][] dp = new int[N][N];
        //先填对角线的最后一个格子，方便下面填对角线及相邻对角线
        dp[N-1][N-1] = 1;
        for(int i = 0; i < N-1; i ++) {
            dp[i][i] = 1;
            //两个字符的情况str[l] == str[r-1]
            dp[i][i+1] = str[i] == str[i+1] ? 2 : 1;
        }
        //上三角按照从下到上，从左向右顺序填，看f的几个依赖关系可以得到
        //从N-3行填
        for(int L = N-1; L >=0; L--) {
            for(int R = L + 2; R < N; R++) {
                //dp[L][R]的结果是取决于左边，左下，下面三个位置
                // 可以发现，左边位置又依赖于左边位置的左边，左下，下面三个位置，则对于dp[L][R]左下可以不考虑，左边包含了
                dp[L][R] = Math.max(dp[L][R-1],dp[L+1][R]); //求左边和下面
                if (str[L] == str[R]) {  //可能性4存在
                    dp[L][R] = Math.max(dp[L][R], 2 + dp[L+1][R-1]);
                }
            }
        }
        return dp[0][N-1];
    }
}
```





### 2、基本型 I (时间序列型)

给出一个序列（数组/字符串），其中每一个元素可以认为“一天”，并且“**今天**”的状态只取决于“**昨天**”的状态。

#### [198. 打家劫舍](https://leetcode.cn/problems/house-robber/)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

**c++代码实现：**

```c
这是“双状态”DP最典型的一道题。它的特点是：每一轮的状态只取决于上一轮的状态；并且每一轮的状态只有两种，通常就是“取”或“不取”。
```

```c
class Solution {
public:
    int rob(vector<int>& nums) {
        /*
        DP
        1.状态表示 f[i],g[i]
        (1)集合：
            f[i]表示在1~i的所有nums里面，一定选择nums[i]
            g[i]表示在1~i的所有nums里面，一定不选择nums[i]

        (2)属性：最大值max(g[n],f[n])

        2.状态计算
    `   (1)计算f[i]:
            当选择nums[i]，则一定不能选择nums[i-1],此时:
            f[i]=g[i-1]+nums[i];
            考虑到数组下标从0开始
            f[i]=g[i-1]+nums[i-1];

        (2)计算g[i]:
            当不选择nums[i]，则可以选择nums[i-1]或者不选择nums[i-1]
            g[i]=max(f[i-1],g[i-1]);

        */
        int n=nums.size();
        vector<int> f(n+1),g(n+1);
        for(int i=1;i<=n;i++){
            f[i]=g[i-1]+nums[i-1];
            g[i]=max(f[i-1],g[i-1]);
        }
        return max(f[n],g[n]);
    }
};
```

```c
class Solution {
public:
    /*
    第i-1轮     抢（下一轮一定不抢）   不抢（下一轮可以抢可以不抢）
    第i轮       抢+val[j]           不抢+0

    0：这轮抢得到的最大收益
    1：这轮不抢得到的最大收益
    */

    int rob(vector<int>& nums) {
        int n=nums.size();
        vector<vector<int>> dp(n+1,vector<int>(n+1,0));
        for(int i=1;i<=n;i++){
            //数组下标0开始
            dp[i][0] = dp[i-1][1]+nums[i-1];
            dp[i][1] = max(dp[i-1][0],dp[i-1][1]);
        }
        return max(dp[n][0],dp[n][1]);
    }
};
```



#### [740. 删除并获得点数](https://leetcode.cn/problems/delete-and-earn/)

给你一个整数数组 nums ，你可以对它进行一些操作。

每次操作中，选择任意一个 nums[i] ，删除它并获得 nums[i] 的点数。之后，你必须删除 所有 等于 nums[i] - 1 和 nums[i] + 1 的元素。

开始你拥有 0 个点数。返回你能通过这些操作获得的最大点数。

**示例 1：**

```
输入：nums = [3,4,2]
输出：6
解释：
删除 4 获得 4 个点数，因此 3 也被删除。
之后，删除 2 获得 2 个点数。总共获得 6 个点数。
```

**示例 2：**

```
输入：nums = [2,2,3,3,3,4]
输出：9
解释：
删除 3 获得 3 个点数，接着要删除两个 2 和 4 。
之后，再次删除 3 获得 3 个点数，再次删除 3 获得 3 个点数。
总共获得 9 个点数。
```

**提示：**

- `1 <= nums.length <= 2 * 10^4`
- `1 <= nums[i] <= 10^4`

**c++代码实现：**

```c
const int N = 10010;
int f[N],g[N];
int cnt[N];

class Solution {
public:
    int deleteAndEarn(vector<int>& nums) {
        int n = nums.size();
        memset(f, 0, sizeof(f));
        memset(g, 0, sizeof(g));
        memset(cnt, 0, sizeof(cnt));
        int res = 0;
        for(auto x: nums) cnt[x]++;
        for(int i = 1; i < N; i++) {
            //f[i]表示不选择i,那么i-1可以选择也可以不选择
            f[i] = max(f[i-1], g[i-1]);
            //g[i]表示选择i,那么i-1一定不能选择
            g[i] = f[i-1] + i * cnt[i];
            res = max(f[i],g[i]);
        }
        return res;
    }
};
```



#### [213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)(状态机DP)

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

**示例 1：**

```c
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例 2：**

```c
输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 3：**

```c
输入：nums = [1,2,3]
输出：3
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

**c++代码实现：**

**状态机DP做法**

```c
class Solution {
public:
    int rob(vector<int>& nums) {
        /*
        状态机DP：
        每个房间分为两种情况，就是选和不选
        位置i：
            不选的话(收益为0),则位置i-1可以选择也可以不选择
                f[i][0]=max(f[i-1][1],f[i-1][0])
            选的话(收益为nums[i]),则收益为位置i-1不选的加上nums[i]值
                f[i][1]=f[i-1][0]+nums[i]

        这里首先对第一间房间进行选择1和不选0操作
        (1)选
            初始化：
                f[0][0]=-1e8(题意选，这里不选和题意不符合，初始化无穷)
                f[0][1]=nums[0]
            因为第一个选择了，则最后一个不能选择，由于下标从0开始
            最终结果就是f[n-1][0]
        (2)不选
            初始化：
                f[0][0]=0
                f[0][1]=-1e8(和假设不符合初始化为无穷)
            因为第一个不选，则最后一个位置可以选也可以不选择
            最终结果为max(f[n-1][0],f[n-1][1])
        选择和不选择取最大值为最终答案
        */
        int n=nums.size(),INF=1e8;
        if(n==1) return nums[0];    //特判
        vector<vector<int>> f(n,vector<int>(2));

        //1.起始位置选(初始化->递推中间步骤得到最后解f[n-1][1])
        f[0][0]=-INF,f[0][1]=nums[0];   //选的初始化
        for(int i=1;i<n;i++){
            f[i][0]=max(f[i-1][0],f[i-1][1]);
            f[i][1]=f[i-1][0]+nums[i];
        }
        //得到起始位置选，则最后一个位置不选的答案
        int res=f[n-1][0];  

        //2.不选(初始化->递推中间步骤得到最后解f[n-1][0])
        f[0][0]=0,f[0][1]=-INF; //不选的初始化
        for(int i=1;i<n;i++){
            f[i][0]=max(f[i-1][0],f[i-1][1]);
            f[i][1]=f[i-1][0]+nums[i];
        }
        //得到起始位置不选，最后位置选或者不选的答案
        int ans=max(f[n-1][0],f[n-1][1]);  
        return max(res,ans); 
    }
};
```

**常规DP做法**

```c
class Solution {
public:
    int rob(vector<int>& nums) {
    /*
        打家劫舍 I
        DP
        1.状态表示 f[i],g[i]
        (1)集合：
            f[i]表示在1~i的所有nums里面，一定选择nums[i]
            g[i]表示在1~i的所有nums里面，一定不选择nums[i]

        (2)属性：最大值max(g[n],f[n])

        2.状态计算
    `   (1)计算f[i]:
            当选择nums[i]，则一定不能选择nums[i-1],此时:
            f[i]=g[i-1]+nums[i];
            考虑到数组下标从0开始
            f[i]=g[i-1]+nums[i-1];

        (2)计算g[i]:
            当不选择nums[i]，则可以选择nums[i-1]或者不选择nums[i-1]
            g[i]=max(f[i-1],g[i-1]);

        打家劫舍 II
        相对于I的话，我们将f[i]和g[i]定义为
        f[i]表示在1~i的所有nums里面，一定选择nums[i]并且nums[0]一定选择的情况
        g[i]表示在1~i的所有nums里面，一定不选择nums[i]并且nums[0]一定不选择的情况
    */

        int n=nums.size();
        if(!n) return 0;
        if(n==1) return nums[0];

        vector<int> f(n+1),g(n+1);
        //先处理第一个位置不选的情况，第一个不选则最后位置n可以选择也可以不选择
        for(int i=2;i<=n;i++){
            f[i]=g[i-1]+nums[i-1];  //nums下标0开始
            g[i]=max(g[i-1],f[i-1]);
        }
        int res=max(f[n],g[n]); //最后位置n可以选择也可以不选择

        //枚举第一个位置选择，则最后位置n不选择
        f[1]=nums[0],g[1]=-1;   //假设是第一个位置选，g[1]表示不选择矛盾
        for(int i=2;i<=n;i++){
            f[i]=g[i-1]+nums[i-1]; //nums下标0开始
            g[i]=max(g[i-1],f[i-1]);
        }
        int ans=g[n];   //最后位置n不选择
        return max(res,ans);
    }
};
```

**残酷刷题**

```
Tick:首位和末位不能同时抢，这说明至少有一个不能抢。
1.考虑首位的房子我不抢，那么对于nouse[1]~house[last]就是一个基本的House Robberl问题。
2.考虑未位的房子我不抢，那么对于nouse[O]~house[last1]就是一个基本的House Robberl问题。
```



#### [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)(树形DP)

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 root 。

除了 root 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 root 。返回 **在不触动警报的情况下** ，小偷能够盗取的最高金额 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```c
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

```c
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

**提示：**

- 树的节点数在 `[1, 10^4]` 范围内
- `0 <= Node.val <= 10^4`

**c++代码实现：**

```c 
    u
 /    \
x      y
1、根节点u,子节点x和y是相互独立的
	（1）当不选u的时候
        f[u][0]=max(f[x][0],f[x][1])+max(f[y][0],f[y][1]);
    （2）当选择u的时候
        f[u][1]=f[x][0]+f[y][0]
2、最后取max(f[root][0],f[root][1])
```

```c
/*
        树形DP(上司的舞会阉割版):
           u
         /   \
        x     y
        1.不选择u,对于x和y两点，可以进行选择也可以不选择
            f(u,0)=max(f(x,0),f(x,1)) + max(f(y,0),f(y,1));
        2.选择u,不选x和y 
            f(u,1)=f(x,0)+f(y,0)+u->val
    */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        auto f=dfs(root);
        return max(f[0],f[1]);
    }

    vector<int> dfs(TreeNode* u){
        if (!u) return {0, 0};
        auto x = dfs(u->left), y = dfs(u->right);
        return {max(x[0], x[1]) + max(y[0], y[1]), x[0] + y[0] + u->val};
    }
};
```

#### [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)(树形DP：换根DP)



#### 状态机DP模型股票买卖：

#### [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**提示：**

- `1 <= prices.length <= 10^5`
- `0 <= prices[i] <= 10^4`

**c++代码实现：**

```c
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res=0;
        for(int i=0,minp=INT_MAX;i<prices.size();i++){
            minp=min(minp,prices[i]);
            res=max(res,prices[i]-minp);
        }
        return res;
    }
};
```



#### [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。

返回 你能获得的 最大 利润 。

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     总利润为 4 。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0 。
```

**提示：**

- `1 <= prices.length <= 3 * 10^4`
- `0 <= prices[i] <= 10^4`

**c++代码实现：**

**i天**买入，j天卖出
    `Pj-pi=(p_i+1-pi)+(p_i+2-pi+1)+..+(pj-p_j-1)`

将交易分解为单天交易，如果后一天和前一天的交易的值为负值，则交易满足条件

```c
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res=0;
        for(int i=0;i+1<prices.size();i++){
            res+=max(0,prices[i+1]-prices[i]);
        }
        return res;
    }
};
```

#### [123. 买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)(前后缀分解)

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。   
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1] 
输出：0 
解释：在这个情况下, 没有交易完成, 所以最大利润为 0。
```

**示例 4：**

```
输入：prices = [1]
输出：0
```

**提示：**

- `1 <= prices.length <= 10^5`
- `0 <= prices[i] <= 10^5`

**c++代码实现：**

```c
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        /*
        前后缀分解问题：
        1----i-1---i---i+1----n
        以i为分界点，分为左边部分1---i-1和右边部分i+1-----n是两个互相没关系的区间
        1.左边部分计算：
        (1)定义最小的值minp=INT_MAX，是买入对应的价值，和当前遍历到的prices[i]得到f[i]
            f[i]=prices[i]-minp
        (2)当i的时候不卖得到第二种情况：
            f[i]=f[i-1];
        综上：f[i]=max(f[i-1],prices[i]-minp)
        (3)更新minp
            minp=min(minp,prices[i-1]);
        为了方便计算，可以使f[]数组得下标从1开始，则需要考虑偏移量prices[i-1]
             f[i]=max(f[i-1],prices[i-1]-minp)

        2.右边部分计算:
        从后往前枚举区间i+1-----n,定义最大值maxp=0
        (1)后面部分最大收益sum
        sum=max(maxp-prices[i-1],sum)
        (2)总得收益
        res=max(res,sum+f[i-1]);
        更新maxp=max(maxp,prices[i-1])
        */
        int n=prices.size();

        int res=0;
        //定义区间前面部分数组
        vector<int> f(n+2);

        for(int i=1,minp=INT_MAX;i<=n;i++){
            f[i]=max(f[i-1],prices[i-1]-minp);
            minp=min(minp,prices[i-1]);
        }

        for(int i=n,maxp=0;i;i--){
            res=max(res,maxp-prices[i-1]+f[i-1]);
            maxp=max(maxp,prices[i-1]);
        }
        return res;
    }
};
```

**残酷刷题**

```c
0:这轮我已持有第1股的最大收益
    两种情况：
    （1）该轮买入第1股需要花费0-val[0];
	（2）上一轮买入第1股，当前不变dp[i-1][0]
            
1:这轮我已售出第1股的最大收益
    两种情况：
	（1）上一轮已经售出第1股，当前这轮不变dp[i-1][1]
    （2）上一轮买入第1股dp[i-1][0],这轮售出+val[i-1],也就是dp[i-1][0]+val[i-1]
            
2:这轮我已持有第2股的最大收益
    两种情况：
	（1）上一轮已经买入第2股，当前不变dp[i-1][2]
    （2）上一轮售出第1股dp[i-1][1],这轮买入第2股-val[i-1],也就是dp[i-1][1]-val[i-1]
    
3:这轮我已售出第2股的最大收益
    两种情况：
	（1）上一轮已经售出第2股，当前不变dp[i-1][3]
    （2）上一轮买入第2股dp[i-1][2],这轮售出第2股+val[i-1],也就是dp[i-1][2]+val[i-1]
    
for (int i=1;i<=N;i++)
	dp[i][0] = max(dp[i-1][0],0-val[i-1]);
	dp[i][1] = max(dp[i-1][1],dp[i-1][0]+val[i-1])
	dp[i][2] = max(dp[i-1][2],dp[i-1][1]-val[i-1])
	dp[i][3] = max(dp[i-1][3],dp[i-1][2]+val[i-1])
}
Ans=max{dp[N][i]}(i=0,1,2,3)    
```



#### [188. 买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)（状态机模型DP）todo

给定一个整数数组 prices ，它的第 i 个元素 prices[i] 是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1：**

```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

**示例 2：**

```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```

**提示：**

- `0 <= k <= 100`
- `0 <= prices.length <= 1000`
- `0 <= prices[i] <= 1000`

**c++代码实现：**

```c
状态机模型DP
    
```

#### [714. 买卖股票的最佳时机含手续费](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)(todo)

给定一个整数数组 prices，其中 prices[i]表示第 i 天的股票价格 ；整数 fee 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

注意：这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

**示例 1：**

```c
输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
解释：能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8
```

**示例 2：**

```c
输入：prices = [1,3,7,5,10,3], fee = 3
输出：6
```

**提示：**

- `1 <= prices.length <= 5 * 10^4`
- `1 <= prices[i] < 5 * 10^4`
- `0 <= fee < 5 * 10^4`

**c++代码实现：**

```c

```



#### [309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)(状态机DPtodo)

给定一个整数数组prices，其中第  prices[i] 表示第 i 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```c
输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**示例 2:**

```c
输入: prices = [1]
输出: 0
```

**提示：**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

**c++代码实现：**

```C

```

**残酷刷题：**

```c

```



#### [376. 摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 摆动序列 。第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

    例如， [1, 7, 4, 9, 2, 5] 是一个 摆动序列 ，因为差值 (6, -3, 5, -7, 3) 是正负交替出现的。
    相反，[1, 4, 7, 2, 5] 和 [1, 7, 4, 5, 5] 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

子序列 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。

给你一个整数数组 nums ，返回 nums 中作为 摆动序列 的 最长子序列的长度 。

**示例 1：**

```
输入：nums = [1,7,4,9,2,5]
输出：6
解释：整个序列均为摆动序列，各元素之间的差值为 (6, -3, 5, -7, 3) 。
```

**示例 2：**

```
输入：nums = [1,17,5,10,13,15,10,5,16,8]
输出：7
解释：这个序列包含几个长度为 7 摆动序列。
其中一个是 [1, 17, 10, 13, 10, 16, 8] ，各元素之间的差值为 (16, -7, 3, -3, 6, -8) 。
```

**示例 3：**

```
输入：nums = [1,2,3,4,5,6,7,8,9]
输出：2
```

**提示：**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 1000`

**进阶：**你能否用 `O(n)` 时间复杂度完成此题?

**c++代码：**

```c
0:以当前元素结尾且上升
1:以当前元素结尾且下降
    
第i-1轮：
    若以当前元素结尾且上升0的最长摆动序列
    则第i轮：
    	以当前元素结尾且下降的最长摆动序列+1 if nums[i-1]>nums[i]
            
第i-1轮：
    若以当前元素结尾且下降1的最长摆动序列
    则第i轮：
    	以当前元素结尾且上升的最长摆动序列+1 if nums[i-1] < nums[i]
```

```c
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        //去重
        nums.erase(unique(nums.begin(), nums.end()), nums.end());
        //如果长度小于2
        if(nums.size()<=2) return nums.size();
        //其他情况选择首尾
        int res=2;
        //局部最大值或者局部最小值情况
        for(int i=1;i+1<nums.size();i++){
            int a=nums[i-1],b=nums[i],c=nums[i+1];
            if(a>b && c>b || a<b && c<b) res++;
        }
        return res;
    }
};
```

#### [1289. 下降路径最小和  II](https://leetcode.cn/problems/minimum-falling-path-sum-ii/)(数字三角形类型)

给你一个 n x n 整数矩阵 arr ，请你返回 非零偏移下降路径 数字和的最小值。

非零偏移下降路径 定义为：从 arr 数组中的每一行选择一个数字，且按顺序选出来的数字中，相邻数字不在原数组的同一列。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/08/10/falling-grid.jpg)

```c
输入：arr = [[1,2,3],[4,5,6],[7,8,9]]
输出：13
解释：
所有非零偏移下降路径包括：
[1,5,9], [1,5,7], [1,6,7], [1,6,8],
[2,4,8], [2,4,9], [2,6,7], [2,6,8],
[3,4,8], [3,4,9], [3,5,7], [3,5,9]
下降路径中数字和最小的是 [1,5,7] ，所以答案是 13 。
```

**示例 2：**

```c
输入：grid = [[7]]
输出：7
```

**提示：**

$n == grid.length == grid[i].length$,
$1 <= n <= 200$,
$-99 <= grid[i][j] <= 99$

**c++代码实现**

```c

```



### 3、基本型 II

### 4、走迷宫型

### 5、背包型

#### （1）01背包

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)(01背包)

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

**c++代码实现:**

```c
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n=nums.size(),m=0;
        for(auto x:nums) m+=x;
        if(m % 2) return false;
        m/=2;
        vector<int> f(m+1);
        f[0]=1;	//1表示存在，0表示不存在
        for(auto x:nums){
            for(int j=m;j>=x;j--){
                f[j]|=f[j-x];
            }
        }
        return f[m];
    }
};
```

#### [494. 目标和](https://leetcode.cn/problems/target-sum/)（todo）

给你一个整数数组 nums 和一个整数 target 。

向数组中的每个整数前添加 '+' 或 '-' ，然后串联起所有整数，可以构造一个 表达式 ：

例如，nums = [2, 1] ，可以在 2 之前添加 '+' ，在 1 之前添加 '-' ，然后串联起来得到表达式 "+2-1" 。
返回可以通过上述方法构造的、运算结果等于 target 的不同 表达式 的数目。

**示例 1：**

```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**示例 2：**

```
输入：nums = [1], target = 1
输出：1
```

**提示：**

$1 <= nums.length <= 20$,
$0 <= nums[i] <= 1000$,
$0 <= sum(nums[i]) <= 1000$,
$-1000 <= target <= 1000$

**c++代码：**

```c
DP
1、状态表示
	(1)集合表示：f[i][j]表示前i个，总和为j
    (2)属性：数量
2、状态计算
    添加号取正号，添减号取负号
    集合划分
    (1)nums[i]取整正
    f[i-1][j-nums[i]]
    (2)nums[i]取负数
    f[i-1][j+nums[i]]
    f[i][j]=f[i-1][j-nums[i]]+f[i-1][j-nums[i]]
    当没有任何元素可以j选取时，元素和只能是0，对应的方案数是1，也就是f[0][0]=1
    
    注意：
    j可能是负数，而j作为数组的index不能为负。解决方法就是给j加上一个offset的偏移，
    将[-offset,offset]的区间平移至[0,offset*2]作为DP数组的第二个维度的index
```

```c
class Solution {
public:
    int findTargetSumWays(vector<int>& a, int S) {
        if (S < -1000 || S > 1000) return 0;
        int n = a.size(), Offset = 1000;
        vector<vector<int>> f(n + 1, vector<int>(2001));
        f[0][Offset] = 1;
        for (int i = 1; i <= n; i ++ )
            for (int j = -1000; j <= 1000; j ++ ) {
                if (j - a[i - 1] >= -1000)
                    f[i][j + Offset] += f[i - 1][j - a[i - 1] + Offset];
                if (j + a[i - 1] <= 1000)
                    f[i][j + Offset] += f[i - 1][j + a[i - 1] + Offset];
            }
        return f[n][S + Offset];
    }
};
```

#### [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/)todo

#### （2）完全背包

#### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

给你一个整数 n ，返回 和为 n 的完全平方数的最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

**提示：**

- `1 <= n <= 10^4`​

**c++代码实现：**

**dp:**

```c
class Solution {
public:
    /*
    设f(i)  表示通过平方数组成 i 所需要完全平方数的最少数量。
    初始时，f(0)=0，其余待定。
    转移时，对于一个 i，枚举 j，f(i)=min(f(i−j∗j)+1)，其中 1≤j≤i^(1/2)。
    最终答案为 f(n)。
    */
    int numSquares(int n) {
        vector<int> f(n+1,n);   //至少答案都是全1组成
        f[0]=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j*j<=i;j++){
                f[i]=min(f[i],f[i-j*j]+1);
            }
        }
        return f[n];
    }
};
```

**BFS:**

```c
class Solution {
public:
    /*
    BFS:
    通过宽搜来优化动态规划的效率，可以将整个过程看做一张图，每个数字都是一个点，两个数字之间差距为平方数时有一条单向边。用宽搜来求从 0 到 n 的最短路。
    */
    int numSquares(int n) {
        vector<int> f(n+1,n);
        queue<int> q;
        f[0]=0;
        q.push(0);
        while(!q.empty()){
            auto t=q.front();
            q.pop();
            if(t==n) break;
            for(int i=1;t+i*i<=n;i++){
                if(f[t+i*i]>f[t]+1){
                    f[t+i*i]=f[t]+1;
                    q.push(t+i*i);
                }
            }
        }
        return f[n];
    }
};
```



#### [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)（完全背包）

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

**提示：**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

**题解：**

```c
完全背包问题。
相当于有 n 种物品，每种物品的体积是硬币面值，价值w是1。问装满背包最少需要多少价值的物品？

状态表示： f[j] 表示凑出 j 价值的钱，最少需要多少个硬币。
第一层循环枚举不同硬币，第二层循环从小到大枚举所有价值（由于每种硬币有无限多个，所以要从小到大枚举），然后用第 i 种硬币更新 f[j]：f[j] = min(f[j], f[j - coins[i]] + 1)。

时间复杂度分析：令 n 表示硬币种数，m 表示总价钱，则总共两层循环，所以时间复杂度是O(nm)。
```

**c++代码：**

```c
class Solution {
public:
    int coinChange(vector<int>& coins, int m) {
        vector<int> f(m + 1, 1e8);
        f[0] = 0;
        for (auto v: coins)
            for (int j = v; j <= m; j ++ )
                f[j] = min(f[j], f[j - v] + 1);
        if (f[m] == 1e8) return -1;
        return f[m];
    }
};
```



#### [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)（完全背包）

给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

**示例 1：**

```
输入：amount = 5, coins = [1, 2, 5]
输出：4
解释：有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**示例 2：**

```
输入：amount = 3, coins = [2]
输出：0
解释：只用面额 2 的硬币不能凑成总金额 3 。
```

**示例 3：**

```
输入：amount = 10, coins = [10] 
输出：1
```

**提示：**

1 <= coins.length <= 300
1 <= coins[i] <= 5000
coins 中的所有值 互不相同
0 <= amount <= 5000

**c++代码：**

```c
完全背包：
   总金额：总背包容量
   硬币：物品
   面值：体积
```

```c
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> f(amount+1);
        f[0]=1;
        for(auto x:coins){
            for(int i=x;i<=amount;i++){
                f[i]+=f[i-x];
            }
        }
        return f[amount];
    }
};
```

#### （3）二维费用的背包问题

#### [474. 一和零](https://leetcode.cn/problems/ones-and-zeroes/)(二维费用的背包问题)

给你一个二进制字符串数组 strs 和两个整数 m 和 n 。

请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。

如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

**示例 1：**

```
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```

**示例 2：**

```
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
```

**提示：**

$1 <= strs.length <= 600$,
$1 <= strs[i].length <= 100$,
$strs[i] $仅由 '0' 和 '1' 组成,
$1 <= m, n <= 100$

**c++代码实现:**

```c
每个01字符串看作一个物品
    0的个数：重量
    1的个数：体积
    价值：1
    m 个 0 和 n 个 1也就是重量为m,体积为n
```

```c
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> f(m+1,vector<int>(n+1));
        for(auto& s:strs){
            int a=0,b=0;    //a表示0的个数，b表示1的个数
            for(auto& c:s){
                if(c=='0') a++;
                else b++;
            }
            // 每个物品只能用一次01背包，01背包倒叙遍历
            for(int i=m;i>=a;i--){
                for(int j=n;j>=b;j--){
                    f[i][j]=max(f[i][j],f[i-a][j-b]+1);
                }
            }
        }
        return f[m][n];
    }
};
```

**题目模板:**

#### [Acwing8.二维费用的背包问题](https://www.acwing.com/problem/content/8/)

**题目描述：**

有 N 件物品和一个容量是 V 的背包，背包能承受的最大重量是 M。

每件物品只能用一次。体积是 vi，重量是 mi，价值是 wi。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，总重量不超过背包可承受的最大重量，且价值总和最大。输出最大价值。

**输入格式**

第一行三个整数，N,V,M，用空格隔开，分别表示物品件数、背包容积和背包可承受的最大重量。

接下来有 N 行，每行三个整数 vi,mi,wi，用空格隔开，分别表示第 i 件物品的体积、重量和价值。

**输出格式**

输出一个整数，表示最大价值。

**数据范围**

$0<N≤1000$,
$0<V,M≤100$,
$0<vi,mi≤100$,
$0<wi≤1000$

**输入样例**

```
4 5 6
1 2 3
2 4 4
3 4 5
4 5 6
```

**输出样例**

```
8
```

**c++代码实现：**

```
每个物品只能用一次01背包
f[i][j]表示重量不超过i，体积不超过j

DP:
1、状态表示
	f[i][j][k]
	(1)集合：所有只从前i个物品中选，并且总体积不超过j, 总重量不超过k的选法
	(2)属性：max
2、状态计算
	集合划分：
	(1)所有不包含物品i的选法
		不包含物品i：
		f[i][j][k]=f[i-1][j][k]
	(2)所有包含物品i的选法
		包含物品i：
		先将物品i拿走，看前面i-1个物品，则体积j-vi，重量k-mi
		f[i-1][j-vi][k-mi]+wi
```

```c
#include <iostream>
#include <cstring>
#include <cstdio>
#include <algorithm>

using namespace std;
const int N = 110;
int f[N][N];
int n,v,m;

int main(){
    cin>>n>>v>>m;
    for(int i=0;i<n;i++){
        int a,b,c;
        cin>>a>>b>>c;
        
        for(int j=v;j>=a;j--){
            for(int k=m;k>=b;k--){
                f[j][k]=max(f[j][k],f[j-a][k-b]+c);
            }
        }
    }
    cout<<f[v][m]<<endl;
    return 0;
}
```



dplist

#### [805. 数组的均值分割](https://leetcode.cn/problems/split-array-with-same-average/)todo

#### [879. 盈利计划](https://leetcode.cn/problems/profitable-schemes/)todo

#### [956. 最高的广告牌](https://leetcode.cn/problems/tallest-billboard/)todo





### 6、键盘型

### 7、To Do or Not To Do

很多不是那么套路的DP题，DP状态可能比较难设计。不过还是有套路可循。

某些题目给你一次“行使某种策略的权力”。联想到买卖股票系列的题，我们常会设计

的两个状态就是“行使了权力”和“没有行使权力”分别对应的价值。

#### [1186. 删除一次得到子数组最大和](https://leetcode.cn/problems/maximum-subarray-sum-with-one-deletion/)（前后缀分解）

给你一个整数数组，返回它的某个 非空 子数组（连续元素）在执行一次可选的删除操作后，所能得到的最大元素总和。换句话说，你可以从原数组中选出一个子数组，并可以决定要不要从中删除一个元素（只能删一次哦），（删除后）子数组中至少应当有一个元素，然后该子数组（剩下）的元素总和是所有子数组之中最大的。

注意，删除一个元素后，子数组 不能为空。

**示例 1：**

```c
输入：arr = [1,-2,0,3]
输出：4
解释：我们可以选出 [1, -2, 0, 3]，然后删掉 -2，这样得到 [1, 0, 3]，和最大。
```

**示例 2：**

```c
输入：arr = [1,-2,-2,3]
输出：3
解释：我们直接选出 [3]，这就是最大和。
```

**示例 3：**

```c
输入：arr = [-1,-1,-1,-1]
输出：-1
解释：最后得到的子数组不能为空，所以我们不能选择 [-1] 并从中删去 -1 来得到 0。
     我们应该直接选择 [-1]，或者选择 [-1, -1] 再从中删去一个 -1。
```

**提示：**

- `1 <= arr.length <= 10^5`
- `-10^4 <= arr[i] <= 10^4`

**c++代码：**

**残酷刷题**

```c
0:以当前元素结尾且没有行使删除权力的MSS
1:以当前元素结尾且已经行使别除权力的MSS
    
第i-1轮：
    (1)以当前元素结尾且没有行使删除权力的MSS
    	第i轮：
    	a.以当前元素结尾且没有行使删除权力的MSS
    		+nums[i]或者是第一个元素直接取nums[i]
    	b.以当前元素结尾且已经行使删除权力的MSS
    		+0
    (2)以当前元素结尾且已经行使删除权力的MSS
    	第i轮：
    	a.以当前元素结尾且已经行使删除权力的MSS
    		+nums[i]或者+0
    
第i轮：
    以当前元素结尾且没有行使删除权力的MSS
    以当前元素结尾且已经行使删除权力的MSS
    
    
for(int i=1;i<=n;i++){
	dp[i][0] = max(dp[i-1][0]+nums[i],nums[i]);
    dp[i][1] = max(dp[i-1][0],dp[i-1][1]+nums[i]);
}
ans = max{dp[i][j]}  i,j=0,1
```

```c
class Solution {
public:
    /*
    分为两种情况：
    1.直接不删除，那就是直接求子序列和的最大值
        f[i] = max(f[i-1],0)+arr[i]
        也就是选择arr[i]，前面部分也选f[i-1]；或者只选arr[i]，前面部分不选前面部分0
    2.删除i,前面部分用f[i-1]来表示以i-1结尾的子序列的和的最大值，用g[i+1]表示以i+1为开始的子序列的和的最大值
        g[i+1]倒着遍历就和f[i-1]是一样的
    */
    int maximumSum(vector<int>& arr) {
        int n=arr.size();
        int res=arr[0];
        vector<int> f(n),g(n);

        f[0]=arr[0]; //初始化f[0]，至少都是能选第一个和最后一个
        for(int i=1;i<n;i++){
            f[i]=max(f[i-1],0)+arr[i];  //递推得到子序列前缀最大值
            //1、求解第一张情况的答案
            res=max(res,f[i]);
        }

        g[n-1]=arr[n-1];
        for(int i=n-2;i>=0;i--){
            g[i]=max(g[i+1],0)+arr[i];
        }

        //2、求解第二种情况
        for(int i=1;i+1<n;i++){
            res=max(res,f[i-1]+g[i+1]);
        }
        return res;
    }
};
```



### 8、区间型 I

### 9、区间型 II

### 10、双序列型

### 11、状态压缩DP

### 12、数位DP

#### [902. 最大为 N 的数字组合](https://leetcode.cn/problems/numbers-at-most-n-given-digit-set/)

给定一个按 非递减顺序 排列的数字数组 digits 。你可以用任意次数 digits[i] 来写的数字。例如，如果 digits = ['1','3','5']，我们可以写数字，如 '13', '551', 和 '1351315'。

返回 可以生成的小于或等于给定整数 n 的正整数的个数 。

**示例 1：**

```
输入：digits = ["1","3","5","7"], n = 100
输出：20
解释：
可写出的 20 个数字是：
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.
```

**示例 2：**

```
输入：digits = ["1","4","9"], n = 1000000000
输出：29523
解释：
我们可以写 3 个一位数字，9 个两位数字，27 个三位数字，
81 个四位数字，243 个五位数字，729 个六位数字，
2187 个七位数字，6561 个八位数字和 19683 个九位数字。
总共，可以使用D中的数字写出 29523 个整数。
```

**示例 3:**

```
输入：digits = ["7"], n = 8
输出：1
```

提示：

$1 <= digits.length <= 9$,
$digits[i].length == 1$,
$digits[i] $是从 '1' 到 '9' 的数,
$digits $中的所有值都 不同 ,
$digits $按 非递减顺序 排列,
$1 <= n <= 10^9$

**c++代码实现：**

```c
class Solution {
public:
    /*
    数位DP
    */
    int power(int a, int b) {
        int res = 1;
        while (b -- ) res *= a;
        return res;
    }

    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        string num = to_string(n);
        reverse(num.begin(), num.end());

        int res = 0;
        //所有比n小的数都满足，一个位置可以填digits.size()的i次方
        for (int i = 1; i < num.size(); i ++ ) res += power(digits.size(), i);

        //计算当位数为n的时候满足条件的数
        bool flag = true;   //  判断右边分支是否存在
        //从高位开始枚举
        for (int i = num.size() - 1; i >= 0; i -- ) {
            //x为这一位数
            //x所在位置后面还有i个位置需要填，如果能随便填t的个数可以算出来
            int x = num[i] - '0', t = power(digits.size(), i);
            
            //计算x左边分支
            int j;
            for (j = 0; j < digits.size(); j ++ )
                if (digits[j][0] - '0' < x)//若这个数是比x小的，则后面可以随便填，答案加t
                    res += t;
                else break;

            //没有越界并且可以填和x一样的数，那么存在右边分支；否则标记flag
            if (j < digits.size() && digits[j][0] - '0' == x) continue;
            flag = false;
            break;
        }

        //如果最后分支存在，更新答案
        if (flag) res ++ ;
        return res;
    }
};
```



低买《算法竞赛进阶指南》



### 13、树形DP

#### [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)



## 八、思维题目

### 1.下一个全排列

#### [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)

整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 

整数数组的 下一个排列 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 下一个排列 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，arr = [1,2,3] 的下一个排列是 [1,3,2] 。
- 类似地，arr = [2,3,1] 的下一个排列是 [3,1,2] 。
- 而 arr = [3,2,1] 的下一个排列是 [1,2,3] ，因为 [3,2,1] 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

```c
class Solution {
public:
    /*
    找到一对逆序数，在分界的左边找一个小的数，在分界右边找一个大的数
    交换两个数
    之后将后面部分按照升序排列
    */
    void nextPermutation(vector<int>& nums) {
        int k=nums.size()-1;
        //从后面往前面找到第一个降序的逆序对
        while(k>0 && nums[k-1] >= nums[k]) k--;
        //如果该序列已经是最大了，则反转变为最小
        if(k<=0){
            reverse(nums.begin(),nums.end());
        }else{
            //逆序对（x,y）的y
            int t=k;
            //找到y右边比x大的数x1
            while(t<nums.size() && nums[t] > nums[k-1]) t++;
            //交换x1和y
            swap(nums[k-1],nums[t-1]);
            //将x1后面的数据升序排列
            reverse(nums.begin()+k,nums.end());
        }
    }
};
```



#### [670. 最大交换](https://leetcode.cn/problems/maximum-swap/)

给定一个非负整数，你**至多**可以交换一次数字中的任意两位。返回你能得到的最大值。

**示例 1 :**

```
输入: 2736
输出: 7236
解释: 交换数字2和数字7。
```

**示例 2 :**

```
输入: 9973
输出: 9973
解释: 不需要交换。
```

**注意:**

- 给定数字的范围是$ [0, 10^8]$

```c
class Solution {
public:
    /*
    和下一个全排列题目相似
    题目的意思，最好的情况是满足一个逆序，降序得到的数字最大
    先找到第一个逆序2736
    找到27
    以7为分界点，在7的右边找到最后一个比2大的数x，出现相同的数，约在后面越好（高位在更高位）
    以7为分界点，在7的左边找到一个比x小的数
    */
    int maximumSwap(int num) {
        string str = to_string(num);
        for(int i=0;i+1<str.size();i++){
            if(str[i]<str[i+1]){
                //找到第一个逆序对
                int k=i+1;
                //在分界点后面找一个最后并且最大的值
                for(int j=k;j<str.size();j++){
                    if(str[j]>=str[k]){
                        k=j;
                    }
                }
                //在左边找一个数
                for(int j=0;j<=i;j++){
                    if(str[j]<str[k]){
                        swap(str[j],str[k]);
                        return stoi(str);
                    }
                }
            }
        }
        //如果不需要交换则直接返回原来数字
        return num;
    }
};
```

```java
class Solution {
    /*
    和下一个全排列题目相似
    题目的意思，最好的情况是满足一个逆序，降序得到的数字最大
    先找到第一个逆序2736
    找到27
    以7为分界点，在7的右边找到最后一个比2大的数x，出现相同的数，约在后面越好（高位在更高位）
    以7为分界点，在7的左边找到一个比x小的数
    */
    public int maximumSwap(int num) {
        char[] str=String.valueOf(num).toCharArray();
        int n=str.length;
        for(int i=0;i+1<n;i++){
            //找到第一个升序的对
            if(str[i]<str[i+1]){
                int k=i+1;
                //找到后面部分最大的一个数x
                for(int j=k;j<n;j++){
                    if(str[j]>=str[k]){
                        k=j;
                    }
                }
                //在前面部分找最小的
                for(int j=0;j<=i;j++){
                    if(str[j]<str[k]){
                        char tem=str[j];
                        str[j]=str[k];
                        str[k]=tem;
                        String res=new String(str);
                        return Integer.parseInt(res);
                    }
                }
            }
        }
        return num;
    }
}
```



## 九、二叉树

### 1.二叉树遍历

#### [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,2,3]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

**示例 4：**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg)

```
输入：root = [1,2]
输出：[1,2]
```

**示例 5：**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg)

```
输入：root = [1,null,2]
输出：[1,2]
```

**提示：**

- 树中节点数目在范围 `[0, 100]` 内
- `-100 <= Node.val <= 100`

**非递归版本：**

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        if(!root) return res;
        stk.push(root);
        while(!stk.empty()){
            auto t=stk.top();
            stk.pop();
            res.push_back(t->val);
            if(t->right) stk.push(t->right);
            if(t->left) stk.push(t->left); 
        }
        return res;
    }
};
```

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> stk;
        while(root || stk.size()){
            while(root){
                res.push_back(root->val);
                stk.push(root);
                root=root->left;
            }
            root=stk.top()->right;
            stk.pop();
        }
        return res;
    }
};
```

**递归版本：**

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> res;
    void dfs(TreeNode* root){
        if(!root) return;
        res.push_back(root->val);
        dfs(root->left);
        dfs(root->right);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        dfs(root);
        return res;
    }
};
```

## 十、链表

#### [817. 链表组件](https://leetcode.cn/problems/linked-list-components/)

给定链表头结点 head，该链表上的每个结点都有一个 唯一的整型值 。同时给定列表 nums，该列表是上述链表中整型值的一个子集。

返回列表 nums 中组件的个数，这里对组件的定义为：链表中一段最长连续结点的值（该值必须在列表 nums 中）构成的集合。

 **示例 1：**

![](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom1.jpg)

```
输入: head = [0,1,2,3], nums = [0,1,3]
输出: 2
解释: 链表中,0 和 1 是相连接的，且 nums 中不包含 2，所以 [0, 1] 是 nums 的一个组件，同理 [3] 也是一个组件，故返回 2。
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom2.jpg)

```
输入: head = [0,1,2,3,4], nums = [0,3,1,4]
输出: 2
解释: 链表中，0 和 1 是相连接的，3 和 4 是相连接的，所以 [0, 1] 和 [3, 4] 是两个组件，故返回 2。
```

**c++代码实现**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    int numComponents(ListNode* head, vector<int>& nums) {
        int s = 0,res = 0;  //s表示维护的链表的一段，res为答案
        unordered_set<int> S(nums.begin(),nums.end());
        for(auto p = head;p; p = p->next){
            if(S.count(p->val)) s++;    //如果满足则一直记录维护s的段
            else{
                //不在里面了，统计前面部分，如果s不为0，得到答案+1，清空s
                if(s){
                    s=0;
                    res++;
                }
            }
        }
        //处理最后一段
        if(s) res++;
        return res;
    }
};
```

**java代码实现：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int numComponents(ListNode head, int[] nums) {
        int s = 0, res = 0;
        Set<Integer> S=new HashSet<>();
        for(int i = 0;i< nums.length;i++){
            S.add(nums[i]);
        }
        for(ListNode p = head; p!=null ; p =p.next){
            // System.out.println(p.val);
            if(S.contains(p.val)) s++;
            else{
                if(s>0){
                    s=0;
                    res++;
                }
            }
        }
        if(s > 0) res++;
        return res;
    }
}
```

## 十一、图论

### 1.SPFA+带权图

#### [743. 网络延迟时间](https://leetcode.cn/problems/network-delay-time/)

有 n 个网络节点，标记为 1 到 n。

给你一个列表 times，表示信号经过 有向 边的传递时间。 times[i] = (ui, vi, wi)，其中 ui 是源节点，vi 是目标节点， wi 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1 。

**示例 1：**

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

```
输入：times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
输出：2
```

**示例 2：**

```
输入：times = [[1,2,1]], n = 2, k = 1
输出：1
```

**示例 3：**

```
输入：times = [[1,2,1]], n = 2, k = 2
输出：-1
```

**提示：**

- 1 <= k <= n <= 100
- 1 <= times.length <= 6000
- times[i].length == 3
- 1 <= ui, vi <= n
- ui != vi
- 0 <= wi <= 100
- 所有 (ui, vi) 对都 互不相同（即，不含重复边）

**c++代码实现：**

```c
const int N = 110,M = 6010,INF = 0x3f3f3f3f;
int h[N],e[M],ne[M],w[M],idx; //邻接表需要的
int dist[N]; // 存储每个点到1号点的最短距离
bool st[N];  // 存储每个点是否在队列中

class Solution {
public:
    void add(int a,int b,int c){
        e[idx]=b,w[idx]=c,ne[idx]=h[a],h[a]=idx++;
    }

    void spfa(int start){
        memset(dist,0x3f,sizeof dist);  // 初始化距离
        queue<int> q;
        dist[start]=0;  //起点距离为0
        q.push(start);
        st[start] = true;	//是否在队列中

        while(!q.empty()){
            int t=q.front();
            q.pop();
            st[t]=false;    //标记不在队列
            //遍历更新所有t的出边
            for(int i=h[t];~i;i=ne[i]){
                //取出当前出边j
                int j=e[i];
                if(dist[j]>dist[t]+w[i]){
                    dist[j]=dist[t]+w[i];
                    if(!st[j]){
                        q.push(j);
                        st[j]=true;
                    }
                }
            }
        }
    }

    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        //初始化邻接表的表头和当前位置idx
        memset(h,-1,sizeof h);
        idx=0;
        //建图
        for(auto& e:times){
            int a=e[0],b=e[1],w=e[2];
            add(a,b,w);
        }
        //从k点开始单源最短路
        spfa(k);
        //求单元最短路的值
        int res=1;
        for(int i=1;i<=n;i++){
            res=max(res,dist[i]);
        }
        if(res==INF) return -1;
        return res;
    }
};
```



#### [882. 细分图中的可到达节点](https://leetcode.cn/problems/reachable-nodes-in-subdivided-graph/)

给你一个无向图（原始图），图中有 n 个节点，编号从 0 到 n - 1 。你决定将图中的每条边 细分 为一条节点链，每条边之间的新节点数各不相同。

图用由边组成的二维数组 edges 表示，其中 edges[i] = [ui, vi, cnti] 表示原始图中节点 ui 和 vi 之间存在一条边，cnti 是将边 细分 后的新节点总数。注意，cnti == 0 表示边不可细分。

要 细分 边 [ui, vi] ，需要将其替换为 (cnti + 1) 条新边，和 cnti 个新节点。新节点为 x1, x2, ..., xcnti ，新边为 [ui, x1], [x1, x2], [x2, x3], ..., [xcnti+1, xcnti], [xcnti, vi] 。

现在得到一个 新的细分图 ，请你计算从节点 0 出发，可以到达多少个节点？如果节点间距离是 maxMoves 或更少，则视为 可以到达 。

给你原始图和 maxMoves ，返回 新的细分图中从节点 0 出发 可到达的节点数 。

**示例 1：**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/08/01/origfinal.png)

```
输入：edges = [[0,1,10],[0,2,1],[1,2,2]], maxMoves = 6, n = 3
输出：13
解释：边的细分情况如上图所示。
可以到达的节点已经用黄色标注出来。
```

**示例 2：**

```
输入：edges = [[0,1,4],[1,2,6],[0,2,8],[1,3,1]], maxMoves = 10, n = 4
输出：23
```

**示例 3：**

```
输入：edges = [[1,2,4],[1,4,5],[1,3,1],[2,3,4],[3,4,5]], maxMoves = 17, n = 5
输出：1
解释：节点 0 与图的其余部分没有连通，所以只有节点 0 可以到达。
```

提示：

0 <= edges.length <= min(n * (n - 1) / 2, 104)
edges[i].length == 3
0 <= ui < vi < n
图中 不存在平行边
0 <= cnti <= 104
0 <= maxMoves <= 109
1 <= n <= 3000

**c++代码实现：**

```c
const int N = 3010, M = 20010, INF = 0x3f3f3f3f;	//	M开两倍是因为无向图

int h[N], e[M], w[M], ne[M], idx;
int dist[N];
bool st[N];

class Solution {
public:
    /*
    a----k----b   a到b的长度c
    如果是a和b中间的位置，那么经过a或者b可以走
    a->k:x    x+dist[a]<=max得到x<=max-dist[a]
    k->b:y    y+dist[a]<=max得到y<=max-dist[a]
    a---b可以到的点树min{x+y,c-1}
    */
    void add(int a, int b, int c) {
        e[idx] = b, w[idx] = c, ne[idx] = h[a], h[a] = idx ++ ;
    }

    void spfa() {
        memset(dist, 0x3f, sizeof dist); //  初始化dist
        dist[0] = 0; // 0的位置设置未0
        queue<int> q;
        q.push(0);
        while (!q.empty()) {
            int t = q.front();
            q.pop();
            st[t] = false;  // 将t标记未false

            //t的邻接点
            for (int i = h[t]; ~i; i = ne[i]) {
                int j = e[i];
                if (dist[j] > dist[t] + w[i]) {
                    dist[j] = dist[t] + w[i];
                    if (!st[j]) {// 如果队列中已存在j，则不需要将j重复插入
                        q.push(j);
                        st[j] = true;
                    }
                }
            }
        }
    }

    int reachableNodes(vector<vector<int>>& edges, int maxMoves, int n) {
        memset(h, -1, sizeof h);//初始化头节点
        idx = 0;
        for (auto& e: edges) {
            int a = e[0], b = e[1], c = e[2];
            //a和b之间共有c个点也就是c+1条边
            add(a, b, c + 1), add(b, a, c + 1);
        }
        spfa();

        int res = 0;
        //处理顶点
        for (int i = 0; i < n; i ++ )
            if (dist[i] <= maxMoves)
                res ++ ;

        //处理节点之间的中间点
        for (auto& e: edges) {
            int a = e[0], b = e[1], c = e[2];
            int x = max(0, maxMoves - dist[a]), y = max(0, maxMoves - dist[b]);
            res += min(x + y, c);
        }
        return res;
    }
};
```



### 2、拓扑排序

```c
有向无环图-拓扑图:时间复杂度 O(n+m), n 表示点数，m 表示边数
所有入度为0的点，放入queue里面
    while(queue不空){
        取队头t
        枚举t的所有的出边t->j
            删除t->j的边，d[j]--;
        	if(d[j]==0)
                j入队
    }
一个有向无环图，一定至少存在一入度为0的点
```

#### [207. 课程表](https://leetcode.cn/problems/course-schedule/)

你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程  bi 。

例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。
请你判断是否可能完成所有课程的学习？如果可以，返回 true ；否则，返回 false 。

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
```

**示例 2：**

```
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```

**提示：**

1 <= numCourses <= 105
0 <= prerequisites.length <= 5000
prerequisites[i].length == 2
0 <= ai, bi < numCourses
prerequisites[i] 中的所有课程对 互不相同

**c++代码：**

```c
class Solution {
public:
    /*有向图的拓扑排序
    1、统计所有点的入度
    入度为0可以走，入度不为0则前面还有先修课没有完成则不能继续
    2、队列维护所有当前入度为0的点，将所有入度为0的点入度即d[u]=0的点入队
    3、bfs每次从对头取一个点，更新
    4、判断所有点是否都遍历完，是否得到拓扑排序
    存在拓扑排序====没有环
    */
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> g(numCourses);   //定义邻接表
        vector<int> d(numCourses);       //记录入度

        //1、统计所有点的入度,构造邻接表
        for(auto& e:prerequisites){
            //[ai, bi]表示如果要学习课程ai则必须先学习课程bi。ai--->bi,bi的入度加1
            int b=e[0],a=e[1];  //学习a之前需要学习b
            //a->b的一条边
            g[a].push_back(b);
            d[b]++;
        }
        //2、队列维护所有当前入度为0的点，将所有入度为0的点入度即d[u]=0的点入队
        queue<int> q;
        for(int i=0;i<numCourses;i++){
            if(d[i]==0)
                q.push(i);
        }
        // 3、bfs每次从对头取一个点
        int res=0;
        while(q.size()){
            auto t=q.front();
            q.pop();
            res++;
            //枚举t的所有的出边t->j
            for(auto i:g[t]){
                //删除t->j的边，d[j]--;
                if(--d[i]==0){
                    q.push(i);
                }
            }
        }
        return res==numCourses;
    }
};
```



#### [210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

现在你总共有 numCourses 门课需要选，记为 0 到 numCourses - 1。给你一个数组 prerequisites ，其中 prerequisites[i] = [ai, bi] ，表示在选修课程 ai 前 必须 先选修 bi 。

例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示：[0,1] 。
返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 任意一种 就可以了。如果不可能完成所有课程，返回 一个空数组 。

**示例 1：**

```
输入：numCourses = 2, prerequisites = [[1,0]]
输出：[0,1]
解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
```

**示例 2：**

```
输入：numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
输出：[0,2,1,3]
解释：总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
```

**示例 3：**

```
输入：numCourses = 1, prerequisites = []
输出：[0]
```

**提示：**

- 1 <= numCourses <= 2000
- 0 <= prerequisites.length <= numCourses * (numCourses - 1)
- prerequisites[i].length == 2
- 0 <= ai, bi < numCourses
- ai != bi
- 所有[ai, bi] 互不相同



**c++代码：**

```c
class Solution {
public:
    vector<int> findOrder(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);  //构造邻接矩阵
        vector<int> d(n);  //统计度数
        for(auto& e:edges){
            // 想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]
            //从第二个数指向第一个数a-->b，a为前驱，b为后继
            int b=e[0],a=e[1];
            g[a].push_back(b);  //将b添加至a的入度，g[a][]的行
            d[b]++;  //b的入度++
        }
        //1.将入度为0的节点入队列
        queue<int> q;
        for(int i=0;i<n;i++){
            if(d[i]==0){
                q.push(i);
            }
        }
        vector<int> res;
        while(q.size()){
            auto t=q.front();
            q.pop();
            res.push_back(t); //将当前节点入队列
            for(auto i:g[t]){   //扫描改节点的下一个节点
                //将所有t指向的顶点的入度减1，并且将减为0的顶点压入队列中
                if(--d[i]==0)
                    q.push(i);
            }
        }
        //排序失败，有向图中有回路
        if(res.size()<n) res={};
        return res;
    }
};
```

#### [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)



#### [802. 找到最终的安全状态](https://leetcode.cn/problems/find-eventual-safe-states/)

有一个有 n 个节点的有向图，节点按 0 到 n - 1 编号。图由一个 索引从 0 开始 的 2D 整数数组 graph表示， graph[i]是与节点 i 相邻的节点的整数数组，这意味着从节点 i 到 graph[i]中的每个节点都有一条边。

如果一个节点没有连出的有向边，则它是 终端节点 。如果没有出边，则节点为终端节点。如果从该节点开始的所有可能路径都通向 终端节点 ，则该节点为 安全节点 。

返回一个由图中所有 安全节点 组成的数组作为答案。答案数组中的元素应当按 升序 排列。

**示例 1：**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/03/17/picture1.png)

```
输入：graph = [[1,2],[2,3],[5],[0],[5],[],[]]
输出：[2,4,5,6]
解释：示意图如上。
节点 5 和节点 6 是终端节点，因为它们都没有出边。
从节点 2、4、5 和 6 开始的所有路径都指向节点 5 或 6 。
```

**示例 2：**

```
输入：graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
输出：[4]
解释:
只有节点 4 是终端节点，从节点 4 开始的所有路径都通向节点 4 。
```

**提示**：

n == graph.length
1 <= n <= 104
0 <= graph[i].length <= n
0 <= $graph[i][j] $<= n - 1
graph[i] 按严格递增顺序排列。
图中可能包含自环。
图中边的数目在范围 [1, 4 * 10^4] 内。

**c++代码：**

```c
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n=graph.size();
        vector<vector<int>> g(n);
        vector<int> d(n);

        //先存储图，存储的时候按照边反向，反向之后再来拓扑排序
        for(int i=0;i<n;i++){
            for(auto b:graph[i]){
                int a=i;
                //原来是a-->b，现在将b-->a，a的入度++
                g[b].push_back(a);
                d[a]++;
            }
        }

        queue<int> q;
        //将入度为0的加入队列
        for(int i=0;i<n;i++){
            if(!d[i])
                q.push(i);
        }

        while(q.size()){
            int t=q.front();
            q.pop();
            for(auto x:g[t]){
                if(--d[x]==0)
                    q.push(x);
            }
        }
        vector<int> res;
        for(int i=0;i<n;i++){
            if(!d[i])
                res.push_back(i);
        }

        return res;
    }
};
```



#### [1203. 项目管理](https://leetcode.cn/problems/sort-items-by-groups-respecting-dependencies/)



#### [1462. 课程表 IV](https://leetcode.cn/problems/course-schedule-iv/)

你总共需要上 numCourses 门课，课程编号依次为 0 到 numCourses-1 。你会得到一个数组 prerequisite ，其中 prerequisites[i] = [ai, bi] 表示如果你想选 bi 课程，你 必须 先选 ai 课程。

有的课会有直接的先修课程，比如如果想上课程 1 ，你必须先上课程 0 ，那么会以 [0,1] 数对的形式给出先修课程数对。
先决条件也可以是 间接 的。如果课程 a 是课程 b 的先决条件，课程 b 是课程 c 的先决条件，那么课程 a 就是课程 c 的先决条件。

你也得到一个数组 queries ，其中 queries[j] = [uj, vj]。对于第 j 个查询，您应该回答课程 uj 是否是课程 vj 的先决条件。

返回一个布尔数组 answer ，其中 answer[j] 是第 j 个查询的答案。

**示例 1：**

![](https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg)

```
输入：numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
输出：[false,true]
解释：课程 0 不是课程 1 的先修课程，但课程 1 是课程 0 的先修课程。
```

**示例 2：**

```
输入：numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
输出：[false,false]
解释：没有先修课程对，所以每门课程之间是独立的。
```

**示例 3：**

![](https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg)

```
输入：numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
输出：[true,true]
```

**提示：**

- 2 <= numCourses <= 100
- 0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)
- prerequisites[i].length == 2
- 0 <= ai, bi <= n - 1
- ai != bi
- 每一对 [ai, bi] 都 不同
- 先修课程图中没有环。
- 0 <= ui, vi <= n - 1
- ui != vi

**c++代码：**

```
考虑到n<=100，即使将每个节点的所有先修课程都记录下来，时间复杂度o(n^2)也是可以接受的。于是本题就是常规的拓扑排序算法，需要特别处理的是：每次从cur拓展到下一个next节点时，要把cur的所有先修课程都复制一遍给next。至于数据结构，显然用集合来实现去重和查询query都很方便。
```

```c
class Solution {
public:
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& edges, vector<vector<int>>& queries) {
        vector<bool> res;
        vector<unordered_set<int>> g(n);
        vector<unordered_set<int>> pre(n);  //记录拓扑序列
        vector<int> d(n);

        for(auto edge:edges){
            int a=edge[0],b=edge[1];
            //a-->b
            g[a].insert(b);
            d[b]++;
        }

        queue<int> q;
        for(int i=0;i<n;i++){
            pre[i].insert(i); //i点的起点为i
            if(!d[i]){
                q.push(i);
            }
        }

        while(!q.empty()){
            auto t=q.front();
            q.pop();
            //遍历t的邻接边
            for(auto& i :g[t]){
                //遍历t点开始的拓扑路径
                for(auto x:pre[t]){
                    pre[i].insert(x);
                }
                if(--d[i]==0){
                    q.push(i);
                }
            }
        }

        for(auto x:queries){
            int a=x[0],b=x[1];
            //a-->b是否有路径
            res.push_back(pre[b].find(a)!=pre[b].end());
        }
        return res;
    }
};
```

### 3、Dijkstra (BFS+PQ)



### 4、bellman-ford

#### [787. K 站中转内最便宜的航班](https://leetcode.cn/problems/cheapest-flights-within-k-stops/)

有 n 个城市通过一些航班连接。给你一个数组 flights ，其中 flights[i] = [fromi, toi, pricei] ，表示该航班都从城市 fromi 开始，以价格 pricei 抵达 toi。

现在给定所有的城市和航班，以及出发城市 src 和目的地 dst，你的任务是找到出一条最多经过 k 站中转的路线，使得从 src 到 dst 的 价格最便宜 ，并返回该价格。 如果不存在这样的路线，则输出 -1。

**示例 1：**

```
输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
输出: 200
解释: 
从城市 0 到城市 2 在 1 站中转以内的最便宜价格是 200，如图中红色所示。
```

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

**示例 2：**

```
输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
输出: 500
解释: 
从城市 0 到城市 2 在 0 站中转以内的最便宜价格是 500，如图中蓝色所示。
```

城市航班图如下

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

**提示：**

- 1 <= n <= 100
- 0 <= flights.length <= (n * (n - 1) / 2)
- flights[i].length == 3
- 0 <= fromi, toi < n
- fromi != toi
- 1 <= pricei <= 104
- 航班没有重复，且不存在自环
- 0 <= src, dst, k < n
- src != dst

**c++代码：**

**未通过**

```c
const int N=101;
int dist[N],backup[N];
int m;
//结构体存边
struct Edge
{
    //起点终点和权重
    int a, b, w;
} edges[N];

class Solution {
public:

    int bellman_ford(int src,int dst,int k){
        //初始化
        memset(dist,0x3f,sizeof dist);
        dist[src]=0;
        k++;
        //不超过K次,则迭代K次
        for(int i=0;i<k;i++){
            //更新得时候只用上一次迭代的结果，先将上一次的结果backup备份下
            memcpy(backup,dist,sizeof backup);
            for(int j=0;j<m;j++){
                int a=edges[j].a,b=edges[j].b,w=edges[j].w;
                dist[b]=min(dist[b],backup[a]+w);
            }
        }
        if(dist[dst]>0x3f3f3f3f / 2)
            return -1;
        return dist[dst];
    }

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        m=flights.size();
        //建图
        for(int i=0;i<m;i++){
            int a=flights[i][0],b=flights[i][1],c=flights[i][2];
            edges[i] = {a, b, c};
        }
        int t = bellman_ford(src,dst,k);
        return t;
    }
};
```

AC：

```c
class Solution {
public:
    const int INF=1e8;

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> dist(n,INF);
        k++;    //中间有K个点，则加上起点和终点有k+2个点，k+1条边
        dist[src]=0;
        while(k--){
            auto cur=dist;  //滚动数组
            for(auto& e:flights){
                int a=e[0],b=e[1],w=e[2];
                cur[b]=min(cur[b],dist[a]+w);
            }
            dist=cur;
        }
        if(dist[dst]==INF) return -1;
        return dist[dst];
    }
};
```

### 5、floyd算法

#### [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)

给你一个变量对数组 equations 和一个实数值数组 values 作为已知条件，其中 equations[i] = [Ai, Bi] 和 values[i] 共同表示等式 Ai / Bi = values[i] 。每个 Ai 或 Bi 是一个表示单个变量的字符串。

另有一些以数组 queries 表示的问题，其中 queries[j] = [Cj, Dj] 表示第 j 个问题，请你根据已知条件找出 Cj / Dj = ? 的结果作为答案。

返回 所有问题的答案 。如果存在某个无法确定的答案，则用 -1.0 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 -1.0 替代这个答案。

注意：输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

**示例 1：**

```
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
```

**示例 2：**

```
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]
```

**示例 3：**

```
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```

**提示：**

1 <= equations.length <= 20
equations[i].length == 2
1 <= Ai.length, Bi.length <= 5
values.length == equations.length
0.0 < values[i] <= 20.0
1 <= queries.length <= 20
queries[i].length == 2
1 <= Cj.length, Dj.length <= 5
Ai, Bi, Cj, Dj 由小写英文字母与数字组成

**c++代码：**

```c
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_set<string> vers;//存点
        unordered_map<string,unordered_map<string,double>> d;   //建图

        for(int i=0;i<equations.size();i++){
            auto a=equations[i][0],b=equations[i][1];
            double c=values[i];
            d[a][b]=c,d[b][a]=1/c;
            vers.insert(a),vers.insert(b);
        }

        for(auto k:vers){
            for(auto i:vers){
                for(auto j:vers){
                    //floyd，如果存在则更新
                    if(d[i][k] && d[j][k]){
                        d[i][j]=d[i][k]*d[k][j];
                    }
                }
            }
        }
        vector<double> res;
        for(auto e:queries){
            auto a=e[0],b=e[1];
            if(d[a][b]) res.push_back(d[a][b]);
            else res.push_back(-1);
        }
        return res;
    }
};
```



### 6、图遍历

#### [1306. 跳跃游戏 III](https://leetcode.cn/problems/jump-game-iii/)

这里有一个非负整数数组 arr，你最开始位于该数组的起始下标 start 处。当你位于下标 i 处时，你可以跳到 i + arr[i] 或者 i - arr[i]。

请你判断自己是否能够跳到对应元素值为 0 的 任一 下标处。

注意，不管是什么情况下，你都无法跳到数组之外。

**示例 1：**

```
输入：arr = [4,2,3,0,3,1,2], start = 5
输出：true
解释：
到达值为 0 的下标 3 有以下可能方案： 
下标 5 -> 下标 4 -> 下标 1 -> 下标 3 
下标 5 -> 下标 6 -> 下标 4 -> 下标 1 -> 下标 3
```

**示例 2：**

```
输入：arr = [4,2,3,0,3,1,2], start = 0
输出：true 
解释：
到达值为 0 的下标 3 有以下可能方案： 
下标 0 -> 下标 4 -> 下标 1 -> 下标 3
```

**示例 3：**

```
输入：arr = [3,0,2,1,2], start = 2
输出：false
解释：无法到达值为 0 的下标 1 处。 
```

**提示：**

- 1 <= arr.length <= 5 * 10^4
- 0 <= arr[i] < arr.length
- 0 <= start < arr.length

**c++代码：**

```c
class Solution {
public:
    bool dfs(vector<int>& arr,int u){
        //当u对应的已经是0，则直接返回
        if(!arr[u]) return true;
        int pos[]={u-arr[u],u+arr[u]};  //两个方向
        //将u标记为已经访问
        arr[u]=-1;
        for(auto x:pos){
            if(x>=0 && x<arr.size() && arr[x]!=-1){
                //没有越界并且没有访问，如果能到达0，则返回
                if(dfs(arr,x)) return true;
            }
        }
        return false;
    }

    bool canReach(vector<int>& arr, int start) {
        return dfs(arr,start);
    }
};
```

## 十二、并查集

#### [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)

有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。

**示例 1：**

![](https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg)

```c
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg)

```
输入：isConnected = [[1,0,0],[0,1,0],[0,0,1]]
输出：3
```

**提示：**

`1 <= n <= 200`
`n == isConnected.length`
`n == isConnected[i].length`
`isConnected[i][j] 为 1 或 0`
`isConnected[i][i] == 1`
`isConnected[i][j] == isConnected[j][i]`

**c++代码实现：**

```c
class Solution {
public:
    vector<int> p;
    
    int find(int x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int n=isConnected.size();
        int res=n;
        //初始化并查集数组
        for(int i=0;i<n;i++) p.push_back(i);
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                //如果i和j是朋友，并且两个不在一个集合里面的话则合并
                if(isConnected[i][j] && find(i)!=find(j)){
                    //将i和j合并
                    //合并为i的祖宗是j
                    p[find(i)]=find(j);
                    res--;
                }
            }
        }
        return res;
    }
};
```

#### [684. 冗余连接](https://leetcode.cn/problems/redundant-connection/)

树可以看成是一个连通且 无环 的 无向 图。

给定往一棵 n 个节点 (节点值 1～n) 的树中添加一条边后的图。添加的边的两个顶点包含在 1 到 n 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 n 的二维数组 edges ，edges[i] = [ai, bi] 表示图中在 ai 和 bi 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 n 个节点的树。如果有多个答案，则返回数组 edges 中最后出现的边。

**示例 1：**

![](https://pic.leetcode-cn.com/1626676174-hOEVUL-image.png)

```c
输入: edges = [[1,2], [1,3], [2,3]]
输出: [2,3]
```

**示例 2：**

![](https://pic.leetcode-cn.com/1626676179-kGxcmu-image.png)

```c
输入: edges = [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出: [1,4]
```

**提示:**

`n == edges.length`
`3 <= n <= 1000`
`edges[i].length == 2`
`1 <= ai < bi <= edges.length`
`ai != bi`
`edges 中无重复元素`
`给定的图是连通的 `

**c++代码实现：**

**无向图连通性 考虑 并查集 有向图依赖性 考虑 深度广度优先 拓扑排序**

```c
class Solution {
public:
    vector<int> p;
    //路径压缩
    int find(int x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n=edges.size();
        //初始化并查集
        p.resize(n+1);
        for(int i=1;i<=n;i++) p[i]=i;

        for(auto& e:edges){
            //找到两个节点得祖宗是在哪
            int a=find(e[0]),b=find(e[1]);
            //如果不是一个则合并，否则则直接返回
            //（也就是最后一个不满足条件得）
            if(a!=b) p[a]=b;
            else return e;
        }
        return {};
    }
};
```



#### [959. 由斜杠划分区域](https://leetcode.cn/problems/regions-cut-by-slashes/)

在由 1 x 1 方格组成的 n x n 网格 grid 中，每个 1 x 1 方块由 '/'、'\' 或空格构成。这些字符会将方块划分为一些共边的区域。

给定网格 grid 表示为一个字符串数组，返回 区域的数量 。

请注意，反斜杠字符是转义的，因此 '\' 用 '\\' 表示。

**示例 1：**

![](https://assets.leetcode.com/uploads/2018/12/15/1.png)

```
输入：grid = [" /","/ "]
输出：2
```

**示例 2：**

![](https://assets.leetcode.com/uploads/2018/12/15/2.png)

```
输入：grid = [" /","  "]
输出：1
```

**示例 3：**

![](https://assets.leetcode.com/uploads/2018/12/15/4.png)

```
输入：grid = ["/\\","\\/"]
输出：5
解释：回想一下，因为 \ 字符是转义的，所以 "/\\" 表示 /\，而 "\\/" 表示 \/。
```

**提示：**

- `n == grid.length == grid[i].length`
- `1 <= n <= 30`
- `grid[i][j]` 是 `'/'`、`'\'`、或 `' '`

**c++代码实现：**

```c
class Solution {
public:
    /*
    将1*1的格子沿着对角线和副对角线分割为4个小三角形
    |--------|---------|
    | \ 0 /  |    0    |
    | 3 \  1 | 3     1 |
    | / 2 \  |    2    |
    |--------|---------|
    |   0    |    0    |  
    | 3    1 | 3     1 |  
    |   2    |    3    |  
    |--------|---------|
    则0和2是相邻的，1和3是相邻的
    */
    int n;
    int dx[4]={-1,0,1,0},dy[4]={0,1,0,-1};

    vector<int> p;
    int find(int x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }

    //将三维转化为一维
    int get(int i,int j,int k){
        return (i*n+j)*4+k;
    }

    int regionsBySlashes(vector<string>& grid) {
        n=grid.size();
        //初始化并查集
        for(int i=0;i<n*n*4;i++) p.push_back(i);

        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                for(int k=0;k<4;k++){
                    int x=i+dx[k],y=j+dy[k];
                    if(x>=0 && x<n && y>=0 && y<n){
                        //不越界的话合并块(正方形还没有分割为4个小的)与块之间
                        /*
                        0和2  1和3
                        0^2=2  2^2=0 1^2=3 3^2=1
                        */
                        p[find(get(i,j,k))]=find(get(x,y,k^2));
                    }
                }
                if(grid[i][j]!='/'){
                    p[find(get(i, j, 0))] = find(get(i, j, 1));
                    p[find(get(i, j, 2))] = find(get(i, j, 3));
                }
                if(grid[i][j]!='\\'){
                    p[find(get(i, j, 0))] = find(get(i, j, 3));
                    p[find(get(i, j, 1))] = find(get(i, j, 2));
                }
            }
        }
        unordered_set<int> S;
        for(int i=0;i<n*n*4;i++){
            S.insert(find(i));
        }
        return S.size();
    }
};
```



#### [765. 情侣牵手](https://leetcode.cn/problems/couples-holding-hands/)

`n` 对情侣坐在连续排列的`2n` 个座位上，想要牵到对方的手。

人和座位由一个整数数组 `row` 表示，其中 `row[i]` 是坐在第 `i` 个座位上的人的 `ID`。情侣们按顺序编号，第一对是` (0, 1)`，第二对是 `(2, 3)`，以此类推，最后一对是 `(2n-2, 2n-1)`。

返回 最少交换座位的次数，以便每对情侣可以并肩坐在一起。 每次交换可选择任意两人，让他们站起来交换座位。

**示例 1:**

```c
输入: row = [0,2,1,3]
输出: 1
解释: 只需要交换row[1]和row[2]的位置即可。
```

**示例 2:**

```c
输入: row = [3,2,0,1]
输出: 0
解释: 无需交换座位，所有的情侣都已经可以手牵手了。
```

**提示:**

- `2n == row.length`
- `2 <= n <= 30`
- `n` 是偶数
- `0 <= row[i] < 2n`
- `row` 中所有元素均**无重复**

**c++代码：**

```c
class Solution {
public:
    /*
    将一对情侣当作一个点，沙发当作1条边
    当交换座位的时候，在同一个环里面，则会增加环；在不同环里面，会合并环
    需要操作的次数就是总的数减去目前需要合并的环数目
    */
    vector<int> p;
    int find(int x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }

    int minSwapsCouples(vector<int>& row) {
        int n=row.size()/2;
        int cnt=n;
        for(int i=0;i<n;i++) p.push_back(i);

        for(int i=0;i<n*2;i+=2){
            int a=row[i]/2,b=row[i+1]/2;
            if(find(a)!=find(b)) {
                p[find(a)]=find(b);
                cnt--;
            }
        }
        return n-cnt;
    }
};
```



## 十三、杂题

### 1.扫描线算法

#### [850. 矩形面积 II](https://leetcode.cn/problems/rectangle-area-ii/)

我们给出了一个（轴对齐的）二维矩形列表 rectangles 。 对于 rectangle[i] = [x1, y1, x2, y2]，其中（x1，y1）是矩形 i 左下角的坐标， (xi1, yi1) 是该矩形 左下角 的坐标， (xi2, yi2) 是该矩形 右上角 的坐标。

计算平面中所有 rectangles 所覆盖的 总面积 。任何被两个或多个矩形覆盖的区域应只计算 一次 。

返回 总面积 。因为答案可能太大，返回 109 + 7 的 模 。

**示例 1：**

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/06/rectangle_area_ii_pic.png)

```
输入：rectangles = [[0,0,2,2],[1,0,2,3],[1,0,3,1]]
输出：6
解释：如图所示，三个矩形覆盖了总面积为6的区域。
从(1,1)到(2,2)，绿色矩形和红色矩形重叠。
从(1,0)到(2,3)，三个矩形都重叠。
```

**示例 2：**

```
输入：rectangles = [[0,0,1000000000,1000000000]]
输出：49
解释：答案是 1018 对 (109 + 7) 取模的结果， 即 49 。
```

**提示：**

$1 <= rectangles.length <= 200$,
$rectanges[i].length = 4$,
$0 <= xi1, yi1, xi2, yi2 <= 109$
矩形叠加覆盖后的总面积不会超越 2^63 - 1 ，这意味着可以用一个 64 位有符号整数来保存面积结果。

```c
typedef long long LL;
typedef pair<int,int> PII;
#define x first
#define y second

class Solution {
public:
    LL cal(vector<vector<int>>& rec,int a,int b){
        vector<PII> q;
        for(auto& r:rec){
            //****这里如果是r[0]>=a && r[2]<=b的话则相当于是重复倒回去计算了
            //如果rec的x满足在a到b的区间范围里面，则将该区间里面的纵坐标进行记录
            if(r[0]<=a && r[2]>=b){
                //要是a到b的区间包含在r区间里面的话，则可以直接计算该区间
                q.push_back({r[1], r[3]});
            }
        }
        //将其按照第一个坐标排序
        sort(q.begin(),q.end());
        //区间合并
        LL res=0,st=-1,ed=-1;
        for(auto& r:q){
            // 如果当前的头节点大于前一个尾节点 新开一个节点
            if(r.x > ed){
                // 新开区间时 求区间大小
                res+=ed-st;
                //更新区间
                st=r.x,ed=r.y;
            }else if(r.y >ed){
                // 如果当前的尾节点大于前一个尾节点 合并尾节点
                ed=r.y;
            }
        }
         // 求最后区间大小
        res+=ed-st;
        return res*(b-a);
    }

    int rectangleArea(vector<vector<int>>& rectangles) {
        /*
        按照横坐标将矩形进行分割，每次通过区间合并将每个区间的纵坐标进行合并得到每个区间的答案
        */
        vector<int> q;
        for(auto& r:rectangles){
            //将每次的矩形的横坐标加入到里面
            int x1=r[0],x2=r[2];
            q.push_back(x1);
            q.push_back(x2);
        }
        //对其进行排序在计算
        sort(q.begin(),q.end());

        //每次计算q[i-1]-q[i]这个区间
        LL res=0;
        for(int i=1;i<q.size();i++){
            res+=cal(rectangles,q[i-1],q[i]);
        }
        return res%1000000007;
    }
};
```

```java
class Solution {
    List<Integer> list;
    int[][] rec;

    public long cal(int a,int b){
        List<int[]> q=new ArrayList<>();
        for(int[] r:rec){
            //如果ab区间在r这组数据的范围里面，则将这组数据加到q里面
            if(r[0]<=a && r[2]>=b){
                q.add(new int[]{r[1],r[3]});
            }
        }
        //将其进行排序
        Collections.sort(q,(x, y) -> {
            if (x[0] == y[0]) {
                return x[1] - y[1];
            }
            return x[0] - y[0];
        });
        long res=0;
        int st=-1,ed=-1;
        for(int[] r:q){
            //如果r这组数据和st到ed没有交集，则加入答案，并且更新st和ed
            if(r[0] > ed){
                res+=ed-st;
                st=r[0];
                ed=r[1];
            }else{
                //如果有交集则更新区间
                ed=Math.max(ed,r[1]);
            }
        }
        res+=ed-st;
        return res*(b-a);
    }

    public int rectangleArea(int[][] rectangles) {
        list=new ArrayList<>();
        rec=rectangles;

        //先将所有点的横坐标加入到list里面进行扫描线
        for(int[] r:rectangles){
            list.add(r[0]);
            list.add(r[2]);
        }
        //对其进行排序
        Collections.sort(list);
        int MOD=(int)1e9+7;

        //按照扫描线方法，也就是按照横坐标枚举，每次计算一个区间的面积再累加起来
        long res=0;

        // for(int i=1;i<list.size();i++){
        //     res=(res+cal(list.get(i-1),list.get(i))) % MOD;
        // }
        // return (int)res;

        for(int i=1;i<list.size();i++){
            res+=cal(list.get(i-1),list.get(i));
        }
        return (int)(res % MOD);
    }
}
```



### 2.多路归并

#### [23. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

**提示：**

$k == lists.length$
$0 <= k <= 10^4$
$0 <= lists[i].length <= 500$
$-10^4 <= lists[i][j] <= 10^4$
$lists[i]$ 按 升序 排列
$lists[i].length $的总和不超过$ 10^4$

**c++代码实现：**

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    //重载运算符()
    struct cmp{
        bool operator() (ListNode* a,ListNode* b){
            return a->val > b->val;
        }
    };

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        //定义头节点和尾节点
        auto head=new ListNode(-1),tail=head;
        //定义小根堆
        priority_queue<ListNode*,vector<ListNode*>,cmp> heap;

        //先将每个非空链表的表头插入到小根堆里面
        for(auto list:lists){
            if(list) heap.push(list);
        }

        while(heap.size()){
            //拿出来最小的一个node，将其弹出，并且找到所在链表，将后面的点加入head
            auto t=heap.top();
            heap.pop();

            //将堆顶元素加到tail，同时更新tail
            tail=tail->next=t;
            if(t->next) heap.push(t->next);
        }
        return head->next;
    }
};
```

**java代码实现：**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        Queue<ListNode> heap=new PriorityQueue<>(
            new Comparator<ListNode>(){
                @Override
                public int compare(ListNode a,ListNode b){
                    return a.val - b.val;
                }
            }
        );

        //定义虚拟头节点
        ListNode dummy=new ListNode(-1);
        ListNode tail=dummy;

        for(ListNode l:lists){
            if(l!=null)
                heap.add(l);
        }

        while(!heap.isEmpty()){
            ListNode t=heap.poll();

            tail.next=t;
            tail=tail.next;

            if(t.next!=null) heap.add(t.next);
        }
        return dummy.next;
    }
}
```



#### [313. 超级丑数](https://leetcode.cn/problems/super-ugly-number/)

超级丑数 是一个正整数，并满足其所有质因数都出现在质数数组 primes 中。

给你一个整数 n 和一个整数数组 primes ，返回第 n 个 超级丑数 。

题目数据保证第 n 个 超级丑数 在 32-bit 带符号整数范围内。

**示例 1：**

```
输入：n = 12, primes = [2,7,13,19]
输出：32 
解释：给定长度为 4 的质数数组 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。
```

**示例 2：**

```
输入：n = 1, primes = [2,3,5]
输出：1
解释：1 不含质因数，因此它的所有质因数都在质数数组 primes = [2,3,5] 中。
```

**提示：**

$1 <= n <= 10^5$
$1 <= primes.length <= 100$

$2 <= primes[i] <= 1000$
题目数据 保证$ primes[i]$ 是一个质数
$primes$ 中的所有值都 互不相同 ，且按 递增顺序 排列

**c++代码实现：(优先队列+质数)**

```c
typedef pair<long,long> PII;
#define x first
#define y second

class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        priority_queue<PII , vector<PII> ,greater<PII>> heap;
        //first为当前这个质数，second为指针
        for(auto x:primes) heap.push({x,0});
        vector<int> f(n);
        f[0]=1;
        for(int i=1;i<n;){
            auto t=heap.top();
            heap.pop();
            //可能存在重复的数,当不重复的时候最小值对应的指针++,
            if(f[i-1]!=t.x) f[i++]=t.x;
            //index表示当前的指针，p表示质数
            int index=t.y,p=t.x/f[index];
            heap.push({(long long)f[index+1]*p,index+1});
        }
        return f[n-1];
    }
};
```

### 3.字符串hash

#### [648. 单词替换](https://leetcode.cn/problems/replace-words/)

#### [139. 单词拆分](https://leetcode.cn/problems/word-break/)（字符串hash+dp）

给你一个字符串 s 和一个字符串列表 wordDict 作为字典。请你判断是否可以利用字典中出现的单词拼接出 s 。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

**提示：**

$1 <= s.length <= 300$,
$1 <= wordDict.length <= 1000$,
$1 <= wordDict[i].length <= 20$,
$s$ 和$ wordDict[i] $仅有小写英文字母组成
$wordDict$ 中的所有字符串 互不相同

**c++代码实现：**

```c
DP
    1、状态表示
    (1)集合：s[1-i]的所有合法的划分方案
    (2)属性：集合是否非空
    2、状态计算
    集合划分：
    按照最后一个单词是1~i,2~i,3~i,....,k~i,.....i~i划分
    以k~i为例，分成两部分，前面部分1~k-1可以随便划分,将后面部分k~i固定,并且k~i在字典里面出现过
    那么整个1~i能否划分就等价于前面1~k-1是否能够划分，前面也就是f[k-1]
```

**从前往后遍历：**

```c
typedef unsigned long long ULL;
const int P=131;
class Solution {
public:
    /*
        字符串hash+dp
        （一）字符串hash:
        将字符串abcd转化为数字，p进制(a*p^3+b*p^2+c*p+d*p^0)
        再对a*p^3+b*p^2+c*p+d*p^0取模Q操作
        经验值P=131或者13331，取模Q=2^64
        用unsigned long long 处理溢出就相当于对2^64取模
        （二）DP:
        1.状态表示 f(i)
         （1）集合:f(i)表示从1到i的所有合法的拼接方案
         （2）属性：字符串是否为空
        2.状态计算
          以s的最后一个出现的单词是出现在哪段单词里面划分
          (1)出现在1~i
          (2)出现在2~i
          .......
          (k)出现在k~i
          .......
          (i)出现在i~i
        */
    bool wordBreak(string s, vector<string>& wordDict) {
        //字符串hash
        int n=s.size();
        unordered_set<ULL> hash;
        for(auto& word:wordDict){
            ULL h=0;
            for(auto c:word){
                h=h*P+c;
            }
            hash.insert(h);
        }
        vector<bool> f(n+1);
        //边界:当字符串为空的时候为ture
        f[0]=true;
        s=' '+s;    //划分的时候从1开始的
        //从前往后，因为计算后面部分是否在hash里面，从前往后好计算，用当前状态推到后面可以到什么状态
        for(int i=0;i<n;i++){
            //如果前面部分已经满足，则看后面i+1到j的部分是否在hash
            if(f[i]){
                ULL h=0;
                for(int j=i+1;j<=n;j++){
                    h=h*P+s[j];
                    //如果往后面加字符后在hash在里面，则更新状态
                    if(hash.count(h)) f[j]=true;
                }
            }
        }
        return f[n];
    } 
};
```

**从后往前遍历：**

```c
typedef unsigned long long ULL;
const int P=131;
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        //字符串hash
        int n=s.size();
        unordered_set<ULL> hash;
        for(auto& word:wordDict){
            ULL h=0;
            for(auto c:word){
                h=h*P+c;
            }
            hash.insert(h);
        }
        vector<bool> f(n+1);
        //边界:当字符串为空的时候为ture
        f[n]=true;
        //从后面往前面递推
        for(int i=n-1;i>=0;i--){
            ULL h=0;
            for(int j=i;j<n;j++){
                h=h*P+s[j];
                if(hash.count(h) && f[j+1]){
                    f[i]=true;
                    break;
                }
            }
        }
        return f[0];
    } 
};
```



### 4.双指针

#### [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

**示例 1：**

```c
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2：**

```c
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3：**

```c
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

**提示：**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

**c++代码实现：**

```c
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n=nums.size();
        int res=INT_MAX;
        //双指针严格满足j在i后面，当i向前移动到i‘,j也只能向前移动不会后退
        for(int i=0,j=0,sum=0;i<n;i++){
            //一定sum维护当前的窗口内的总和
            sum+=nums[i];
            /*
            假设将nums[j]位置的数从sum里面减去
            结果还满足条件，则可以把其真去除，将指针j向前移动
            */
            while(sum-nums[j]>=target) sum-=nums[j++];
            //更新答案
            if(sum>=target) 
                res=min(res,i-j+1);
        }
        if(res==INT_MAX) return 0;
        return res;
    }
};
```



#### [434. 字符串中的单词数](https://leetcode.cn/problems/number-of-segments-in-a-string/)

统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。

请注意，你可以假定字符串里不包括任何不可打印的字符。

**示例:**·

```
输入: "Hello, my name is John"
输出: 5
解释: 这里的单词是指连续的不是空格的字符，所以 "Hello," 算作 1 个单词。
```

**c++代码：**

```c
class Solution {
public:
    int countSegments(string s) {
        //双指针
        int res=0;
        for(int i=0;i<s.size();i++){
            if(s[i]==' ') continue;
            int j=i+1;
            while(s[j]!=' ' && j<s.size()) j++;
            res++;
            //上面遍历的时候i++,这里-1刚好可以抵消
            i=j-1;
        }
        return res;
    }
};
```

#### [1784. 检查二进制字符串字段](https://leetcode.cn/problems/check-if-binary-string-has-at-most-one-segment-of-ones/)

给你一个二进制字符串 s ，该字符串 不含前导零 。

如果 s 包含 零个或一个由连续的 '1' 组成的字段 ，返回 true 。否则，返回 false 。

如果 s 中 由连续若干个 '1' 组成的字段 数量不超过 1，返回 true 。否则，返回 false 。

**示例 1：**

```
输入：s = "1001"
输出：false
解释：由连续若干个 '1' 组成的字段数量为 2，返回 false
```

**示例 2：**

```
输入：s = "110"
输出：true
```

**提示：**

- `1 <= s.length <= 100`
- `s[i]` 为 `'0'` 或 `'1'`
- `s[0]` 为 `'1'`

**c++代码：**

```c
class Solution {
public:
    bool checkOnesSegment(string s) {
        int res=0;
        for(int i=0;i<s.size();i++){
            if(s[i]=='0') continue;
            int j=i+1;
            while(j<s.size() && s[j]!='0') j++;
            res++;
            i=j-1;
        }
        return res<=1;
    }
};
```

#### [904. 水果成篮](https://leetcode.cn/problems/fruit-into-baskets/)

你正在探访一家农场，农场从左到右种植了一排果树。这些树用一个整数数组 fruits 表示，其中 fruits[i] 是第 i 棵树上的水果 种类 。

你想要尽可能多地收集水果。然而，农场的主人设定了一些严格的规矩，你必须按照要求采摘水果：

你只有 两个 篮子，并且每个篮子只能装 单一类型 的水果。每个篮子能够装的水果总量没有限制。
你可以选择任意一棵树开始采摘，你必须从 每棵 树（包括开始采摘的树）上 恰好摘一个水果 。采摘的水果应当符合篮子中的水果类型。每采摘一次，你将会向右移动到下一棵树，并继续采摘。
一旦你走到某棵树前，但水果不符合篮子的水果类型，那么就必须停止采摘。
给你一个整数数组 fruits ，返回你可以收集的水果的 最大 数目。

**示例 1：**

```
输入：fruits = [1,2,1]
输出：3
解释：可以采摘全部 3 棵树。
```

**示例 2：**

```
输入：fruits = [0,1,2,2]
输出：3
解释：可以采摘 [1,2,2] 这三棵树。
如果从第一棵树开始采摘，则只能采摘 [0,1] 这两棵树。
```

**示例 3：**

```
输入：fruits = [1,2,3,2,2]
输出：4
解释：可以采摘 [2,3,2,2] 这四棵树。
如果从第一棵树开始采摘，则只能采摘 [1,2] 这两棵树。
```

**示例 4：**

```
输入：fruits = [3,3,3,1,2,1,1,2,3,3,4]
输出：5
解释：可以采摘 [1,2,1,1,2] 这五棵树。
```

**c++代码实现：**

```c
class Solution {
public:
    /*
    维护双指针j---i,保证在双指针区间范围内只包含两种数，求区间长度最大
    */
    int totalFruit(vector<int>& fruits) {
        unordered_map<int,int> hash;
        int res=0;
        //s表示当前得区间里面有几个数，几个hash值
        for(int i=0,j=0,s=0;i<fruits.size();i++) {
            if(++hash[fruits[i]]==1) s++;   //当前的数为新加入的
            while(s>2){
                //把j指针向后移动，同时hash表里面的数减1
                if(--hash[fruits[j]]==0) s--;
                j++;
            }
            //更新区间长度
            res=max(res,i-j+1);
        }
        return res;
    }
};
```

**java代码实现：**

```java
class Solution {
    public int totalFruit(int[] fruits) {
        int res=0;
        Map<Integer,Integer> hash=new HashMap<>();
        for(int i=0,j=0,s=0;i<fruits.length;i++){
            int cnt= hash.getOrDefault(fruits[i],0)+1;
            hash.put(fruits[i],cnt);
            while(hash.size()>2){
                int k=hash.get(fruits[j])-1;
                if(k==0){
                    hash.remove(fruits[j]);
                }else{
                    hash.put(fruits[j],k);
                }
                j++;
            }
            res=Math.max(res,i-j+1);
        }
        return res;
    }
}
```

#### [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

 **示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**提示：**

- `0 <= s.length <= 5 * 104`
- `s` 由英文字母、数字、符号和空格组成

**c++代码实现：**

```c
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<char,int> heap;
        int len=s.size();
        int ans=0;
        for(int i=0,j=0;i<len;i++){     //i是后面指针，j前面
            heap[s[i]]++;
            while(heap[s[i]]>1){
                heap[s[j++]]--;   //删除j位置   //j后移
            }
            ans=max(ans,i-j+1);
        }
        return ans;
    }
};
```

#### [795. 区间子数组个数](https://leetcode.cn/problems/number-of-subarrays-with-bounded-maximum/)

给你一个整数数组 nums 和两个整数：left 及 right 。找出 nums 中连续、非空且其中最大元素在范围 [left, right] 内的子数组，并返回满足条件的子数组的个数。

生成的测试用例保证结果符合 32-bit 整数范围。

**示例 1：**

```
输入：nums = [2,1,4,3], left = 2, right = 3
输出：3
解释：满足条件的三个子数组：[2], [2, 1], [3]
```

**示例 2：**

```
输入：nums = [2,9,2,5,6], left = 2, right = 8
输出：7
```

**提示：**

- `1 <= nums.length <= 105`
- `0 <= nums[i] <= 109`
- `0 <= left <= right <= 109`

**c++代码：**

```c
j<nums.size()在&&后面报错：
while(nums[j]<=r && j<nums.size()) j++;
```

```c
class Solution {
public:
    /*
    提高课数位DP技巧： 上下界转化为一个边界来求
    calc(k):最大值<=k的非空子数组数量
    L到R的非空子数组个数=calc(R)-calc(L-1)   
    先将>k的数找出来，求非空子数组的时候不包含这些数就行
    |----------|---->k的数-----|-----------|
    */
     
    int numSubarrayBoundedMax(vector<int>& nums, int left, int right) {
        return solve(nums,right)-solve(nums,left-1);
    }

    /*
    求nums<=r的非空子数组的数量
    长度为k的区间的非空子区间的个数res=k+(k-1)+(k-2)...+1=k(k-1)/2
    */
    int solve(vector<int>& nums,int r){
        int res=0;
        for(int i=0;i<nums.size();i++){
            if(nums[i]>r) continue;
            int j=i+1;
            while(j<nums.size() && nums[j]<=r) j++;
            int k=j-i;  // 满足条件的区间长度
            res+=(long long)k*(k+1)/2;
            i=j;    //  跳过
        }
        return res;
    }
};
```



#### [809. 情感丰富的文字](https://leetcode.cn/problems/expressive-words/)（todo）



### 5、位运算枚举

#### [401. 二进制手表](https://leetcode.cn/problems/binary-watch/)

二进制手表顶部有 4 个 LED 代表 小时（0-11），底部的 6 个 LED 代表 分钟（0-59）。每个 LED 代表一个 0 或 1，最低位在右侧。

例如，下面的二进制手表读取 "3:25" 。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/03/29/binary_clock_samui_moon.jpg)

给你一个整数 turnedOn ，表示当前亮着的 LED 的数量，返回二进制手表可以表示的所有可能时间。你可以 按任意顺序 返回答案。

小时不会以零开头：

例如，"01:00" 是无效的时间，正确的写法应该是 "1:00" 。
分钟必须由两位数组成，可能会以零开头：

例如，"10:2" 是无效的时间，正确的写法应该是 "10:02" 。

**示例 1：**

```
输入：turnedOn = 1
输出：["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]
```

**示例 2：**

```
输入：turnedOn = 9
输出：[]
```

**提示：**

- `0 <= turnedOn <= 10`

**c++代码实现：**

```c
class Solution {
public:
    /*
    a<<n等于a乘2的n次方，a>>n等于a整除2的n次方
    */
    vector<string> readBinaryWatch(int turnedOn) {
        vector<string> res;
        char str[10];
        //一共有2^10种情况
        for(int i=0;i<1<<10;i++){
            int ans=0;
            //枚举10种状态
            for(int j=0;j<10;j++){
                //如果当前是1则数量++
                if(i>>j & 1){
                    ans++;
                }
            }
            if(ans==turnedOn){
                //h表示小时也就是高四位，m表示分钟，低六位
                int h=i>>6,m=i & 63;
                //如果合法
                if(h<12 && m<60){
                    sprintf(str,"%d:%02d",h,m);
                    res.push_back(str);
                }
            }
        }
        return res;
    }
};
```

### 6、贡献度问题

#### [891. 子序列宽度之和](https://leetcode.cn/problems/sum-of-subsequence-widths/)

一个序列的 宽度 定义为该序列中最大元素和最小元素的差值。

给你一个整数数组 nums ，返回 nums 的所有非空 子序列 的 宽度之和 。由于答案可能非常大，请返回对 109 + 7 取余 后的结果。

子序列 定义为从一个数组里删除一些（或者不删除）元素，但不改变剩下元素的顺序得到的数组。例如，[3,6,2,7] 就是数组 [0,3,1,6,2,2,7] 的一个子序列。

**示例 1：**

```
输入：nums = [2,1,3]
输出：6
解释：子序列为 [1], [2], [3], [2,1], [2,3], [1,3], [2,1,3] 。
相应的宽度是 0, 0, 0, 1, 1, 2, 2 。
宽度之和是 6 。
```

**示例 2：**

```
输入：nums = [2]
输出：0
```

**提示：**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`

**c++代码实现：**

```c
const int N = 1e5+5,MOD=1e9+7;
typedef long long LL;
int p[N];
int n;

class Solution {
public:
    /*
        0------i------n
        将nums排序之后，选择i，计算对答案的贡献度是多少
        当i对应的是最大值的时候，则i的贡献值是加，左边位置i的选择有2^i
        当i对应的是最小值的时候，则i的贡献度是减，右边位置i的选择有-2^(n-i-1)
    */
    int sumSubseqWidths(vector<int>& nums) {
        int res=0;
        n = nums.size();
        p[0]=1;
        for(int i=1;i<=n;i++) p[i]=p[i-1]*2 % MOD;
        sort(nums.begin(),nums.end());
        for(int i=0;i<n;i++){
            res=(res+(LL)nums[i]*p[i]-(LL)nums[i]*p[n-i-1]) % MOD;
        }
        return res;
    }
};
```

### 7、Trie

#### [208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)

Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：

Trie() 初始化前缀树对象。
void insert(String word) 向前缀树中插入字符串 word 。
boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
boolean startsWith(String prefix) 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。

**示例：**

```c
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```

**提示：**

1 <= word.length, prefix.length <= 2000
word 和 prefix 仅由小写英文字母组成
insert、search 和 startsWith 调用次数 总计 不超过 3 * 10^4 次

**c++代码实现：**

```c
class Trie {
public:
    struct Node{
        bool is_end;
        Node* son[26];
        Node(){
            is_end=false;
            for(int i=0;i<26;i++) son[i]=NULL;

        }
    }*root;

    Trie() {
        root=new Node();
    }
    
    void insert(string word) {
        auto p=root;
        for(auto c:word){
            auto u=c-'a';
            if(!p->son[u]) p->son[u]=new Node();
            p=p->son[u];
        }
        p->is_end=true;
    }
    
    bool search(string word) {
        auto p=root;
        for(auto x:word){
            auto u=x-'a';
            if(!p->son[u]) return false;
            p=p->son[u];
        }
        return p->is_end;
    }
    
    bool startsWith(string prefix) {
        auto p=root;
        for(auto c:prefix){
            auto u=c-'a';
            if(!p->son[u]) return false;
            p=p->son[u];
        }
        return true;
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

#### [211. 添加与搜索单词 - 数据结构设计](https://leetcode.cn/problems/design-add-and-search-words-data-structure/)(todo)

请你设计一个数据结构，支持 添加新单词 和 查找字符串是否与任何先前添加的字符串匹配 。

实现词典类 WordDictionary ：

WordDictionary() 初始化词典对象
void addWord(word) 将 word 添加到数据结构中，之后可以对它进行匹配
bool search(word) 如果数据结构中存在字符串与 word 匹配，则返回 true ；否则，返回  false 。word 中可能包含一些 '.' ，每个 . 都可以表示任何一个字母。

**示例：**

```c
输入：
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
输出：
[null,null,null,null,false,true,true,true]

解释：
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // 返回 False
wordDictionary.search("bad"); // 返回 True
wordDictionary.search(".ad"); // 返回 True
wordDictionary.search("b.."); // 返回 True
```

**提示：**

1 <= word.length <= 25
addWord 中的 word 由小写英文字母组成
search 中的 word 由 '.' 或小写英文字母组成
最多调用 10^4 次 addWord 和 search

**c++代码实现：**

```c
class WordDictionary {
public:
    //先确定数据结构
    struct Node{
        bool is_end;
        Node *son[26];
        Node(){
            is_end=false;
            for(int i=0;i<26;i++) son[i]=NULL;
        }
    }*root;

    WordDictionary() {
        root=new Node();
    }
    
    void addWord(string word) {
        auto p=root;
        for(auto x:word){
            int u=x-'a';
            if (!p->son[u]) p->son[u] = new Node();
            p = p->son[u];
        }
        p->is_end=true;
    }
    
    bool search(string word) {
        return dfs(root,word,0);
    }

    bool dfs(Node* p,string& word,int i){
        if(i==word.size()) return p->is_end;
        //如果不是.的话则按照正常的trie树的查找
        if(word[i]!='.'){
            int u=word[i]-'a';
            if(!p->son[u]) return false;
            return dfs(p->son[u],word,i+1);
        }else{  //如果是.的话则选择爆索遍历下所有的儿子
            for(int j=0;j<26;j++){
                if(p->son[j] && dfs(p->son[j],word,i+1))
                    return true;
            }
            return false;
        }
    }
};

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```



## 十四、树状数组和线段树

### 1、树状数组

#### (1)单点更新题目

#### [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)

给定一个整数数组  nums，处理以下类型的多个查询:

1、计算索引 left 和 right （包含 left 和 right）之间的 nums 元素的 和 ，其中 left <= right
实现 NumArray 类：

* NumArray(int[] nums) 使用数组 nums 初始化对象
* int sumRange(int i, int j) 返回数组 nums 中索引 left 和 right 之间的元素的 总和 ，包含 left 和 right 两点（也就是 nums[left] + nums[left + 1] + ... + nums[right] )

**示例 1：**

```c
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

**提示：**

$1 <= nums.length <= 10^4$,
$-10^5 <= nums[i] <= 10^5$,
$0 <= i <= j < nums.length$
最多调用 $10^4 $次 sumRange 方法

**c++代码实现：**

```c
class NumArray {
public:
    int n;
    vector<int> nums,tr;

    int lowbit(int x){
        return x&-x;
    }
	
    //x位置加v
    void add(int x,int v){
        for(int i=x;i<=n;i+=lowbit(i)) tr[i]+=v;
    }

    int query(int x){
        int res=0;
        for(int i=x;i;i-=lowbit(i)) res+=tr[i];
        return res;
    }

    NumArray(vector<int>& _nums) {
        nums=_nums;
        n=nums.size();
        tr.resize(n+1);
        //树状数组下标1开始
        for(int i=0;i<n;i++) add(i+1,nums[i]);
    }
    
    int sumRange(int left, int right) {
        //树状数组下标1开始
        return query(right+1)-query(left);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 */
```



#### [307. 区域和检索 - 数组可修改](https://leetcode.cn/problems/range-sum-query-mutable/)

给你一个数组 nums ，请你完成两类查询。

* 其中一类查询要求 更新 数组 nums 下标对应的值

* 另一类查询要求返回数组 nums 中索引 left 和索引 right 之间（ 包含 ）的nums元素的 和 ，其中 left <= right

实现 NumArray 类：

* NumArray(int[] nums) 用整数数组 nums 初始化对象

* void update(int index, int val) 将 nums[index] 的值 更新 为 val
* int sumRange(int left, int right) 返回数组 nums 中索引 left 和索引 right 之间（ 包含 ）的nums元素的 和 （即，nums[left] + nums[left + 1], ..., nums[right]）

**示例 1：**

```c
输入：
["NumArray", "sumRange", "update", "sumRange"]
[[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]
输出：
[null, 9, null, 8]

解释：
NumArray numArray = new NumArray([1, 3, 5]);
numArray.sumRange(0, 2); // 返回 1 + 3 + 5 = 9
numArray.update(1, 2);   // nums = [1,2,5]
numArray.sumRange(0, 2); // 返回 1 + 2 + 5 = 8
```

**提示：**

$1 <= nums.length <= 3 * 10^4$,
$-100 <= nums[i] <= 100$,
$0 <= index < nums.length$,
$-100 <= val <= 100$,
$0 <= left <= right < nums.length$,
调用 update 和 sumRange 方法次数不大于$ 3 * 10^4 $

**c++代码实现：**

```c
class NumArray {
public:
    int n;
    vector<int> nums,tr;

    int lowbit(int x){
        return x&-x;
    }
		
    //x位置加v
    void add(int x,int v){
        for(int i=x;i<=n;i+=lowbit(i)) tr[i]+=v;
    }

    int query(int x){
        int res=0;
        for(int i=x;i;i-=lowbit(i)) res+=tr[i];
        return res;
    }
    
    NumArray(vector<int>& _nums) {
        nums=_nums;
        n=nums.size();
        tr.resize(n+1);
        //树状数组下标1开始
        for(int i=0;i<n;i++) add(i+1,nums[i]);
    }
    
    void update(int index, int val) {
        //树状数组下标1开始
        //将index位置的值nums[index]修改为val就是将nums[index]先减nums[index],后加val
        add(index+1,val-nums[index]);
        nums[index]=val;
    }
    
    int sumRange(int left, int right) {
        //树状数组下标1开始
        return query(right+1)-query(left);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(index,val);
 * int param_2 = obj->sumRange(left,right);
 */
```

#### (2)逆序对类题目

#### [315. 计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/)

给你一个整数数组 nums ，按要求返回一个新数组 counts 。数组 counts 有该性质： counts[i] 的值是  nums[i] 右侧小于 nums[i] 的元素的数量。

**示例 1：**

```c
输入：nums = [5,2,6,1]
输出：[2,1,1,0] 
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素
```

**示例 2：**

```c
输入：nums = [-1]
输出：[0]
```

**示例 3：**

```c
输入：nums = [-1,-1]
输出：[0,0]
```

**提示：**

- `1 <= nums.length <= 10^5`​
- `-104 <= nums[i] <= 10^4`

**c++代码实现：**

```c
class Solution {
public:
    //数据范围是-1e4到0到1e4，则一共有20001个数字
    int n=20001;
    vector<int> tr; //树状数组存的是有多少个数

    int lowbit(int x){
        return x&-x;
    }

    void add(int x,int v){
        for(int i=x;i<=n;i+=lowbit(i)) tr[i]+=v;
    }

    int query(int x){
        int res=0;
        for(int i=x;i;i-=lowbit(i)) res+=tr[i];
        return res;
    }

    vector<int> countSmaller(vector<int>& nums) {
        //树状数组的下标是从1开始的
        tr.resize(n+1);
        vector<int> res(nums.size());
        
        for(int i=nums.size()-1;i>=0;i--){
            //将数据全部右移到1开始
            int x=nums[i]+10001;
            //x位置后面有多少比x小的数，就是query(x-1)
            res[i]=query(x-1);
            //将x对于的tr加1
            add(x,1);
        }
        return res;
    }
};
```

#### [1450. 在既定时间做作业的学生人数](https://leetcode.cn/problems/number-of-students-doing-homework-at-a-given-time/)

给你两个整数数组 startTime（开始时间）和 endTime（结束时间），并指定一个整数 queryTime 作为查询时间。

已知，第 i 名学生在 startTime[i] 时开始写作业并于 endTime[i] 时完成作业。

请返回在查询时间 queryTime 时正在做作业的学生人数。形式上，返回能够使 queryTime 处于区间 [startTime[i], endTime[i]]（含）的学生人数。

**示例 1：**

```c
输入：startTime = [1,2,3], endTime = [3,2,7], queryTime = 4
输出：1
解释：一共有 3 名学生。
第一名学生在时间 1 开始写作业，并于时间 3 完成作业，在时间 4 没有处于做作业的状态。
第二名学生在时间 2 开始写作业，并于时间 2 完成作业，在时间 4 没有处于做作业的状态。
第三名学生在时间 3 开始写作业，预计于时间 7 完成作业，这是是唯一一名在时间 4 时正在做作业的学生。
```

**示例 2：**

```c
输入：startTime = [4], endTime = [4], queryTime = 4
输出：1
解释：在查询时间只有一名学生在做作业。
```

**示例 3：**

```c
输入：startTime = [4], endTime = [4], queryTime = 5
输出：0
```

**示例 4：**

```c
输入：startTime = [1,1,1,1], endTime = [1,3,2,4], queryTime = 7
输出：0
```

**示例 5：**

```c
输入：startTime = [9,8,7,6,5,4,3,2,1], endTime = [10,10,10,10,10,10,10,10,10], queryTime = 5
输出：5
```

**提示：**

startTime.length == endTime.length
1 <= startTime.length <= 100
1 <= startTime[i] <= endTime[i] <= 1000
1 <= queryTime <= 1000

**c++代码实现：**

```c
class Solution {
public:
    int n=1000;
    vector<int> tr;

    int lowbit(int x){
        return x&-x;
    }

    void add(int x,int v){
        for(int i=x;i<=n;i+=lowbit(i)) tr[i]+=v;
    }

    int query(int x){
        int res=0;
        for(int i=x;i;i-=lowbit(i)) res+=tr[i];
        return res;
    }

    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        tr.resize(1001);
        for(int i=0;i<startTime.size();i++){
            int a=startTime[i],b=endTime[i];
            for(int j=a;j<=b;j++){
                add(j,1);
            }
        }
        return query(queryTime)-query(queryTime-1);
    }
};
```

#### [1893. 检查是否区域内所有整数都被覆盖](https://leetcode.cn/problems/check-if-all-the-integers-in-a-range-are-covered/)

[参考解答](https://leetcode.cn/problems/check-if-all-the-integers-in-a-range-are-covered/solution/gong-shui-san-xie-yi-ti-shuang-jie-mo-ni-j83x/)

给你一个二维整数数组 ranges 和两个整数 left 和 right 。每个 ranges[i] = [starti, endi] 表示一个从 starti 到 endi 的 闭区间 。

如果闭区间 [left, right] 内每个整数都被 ranges 中 至少一个 区间覆盖，那么请你返回 true ，否则返回 false 。

已知区间 ranges[i] = [starti, endi] ，如果整数 x 满足 starti <= x <= endi ，那么我们称整数x 被覆盖了。

**示例 1：**

```c
输入：ranges = [[1,2],[3,4],[5,6]], left = 2, right = 5
输出：true
解释：2 到 5 的每个整数都被覆盖了：
- 2 被第一个区间覆盖。
- 3 和 4 被第二个区间覆盖。
- 5 被第三个区间覆盖。
```

**示例 2：**

```c
输入：ranges = [[1,10],[10,20]], left = 21, right = 21
输出：false
解释：21 没有被任何一个区间覆盖。
```

**提示：**

1 <= ranges.length <= 50
1 <= starti <= endi <= 50
1 <= left <= right <= 50

**c++树状数组写法：**

```c
class Solution {
public:
    //数据范围50
    int n=55;
    vector<int> tr;

    int lowbit(int x){
        return x&-x;
    }

    void add(int x,int v){
        for(int i=x;i<=n;i+=lowbit(i)) tr[i]+=v;
    }

    int query(int x){
        int res=0;
        for(int i=x;i;i-=lowbit(i)) res+=tr[i];
        return res;
    }

    bool isCovered(vector<vector<int>>& ranges, int left, int right) {
        tr.resize(n);
        for(auto e:ranges){
            int a=e[0],b=e[1];
            for(int j=a;j<=b;j++){
                add(j,1);
            }
        }

        for(int i=left;i<=right;i++){
            //查询每个单位1的区间里面有没有数，没有则返回false
            int res=query(i)-query(i-1);
            if(res==0) return false;
        }
        return true;
    }
};
```

#### (3)树状数组+离散化

#### [327. 区间和的个数](https://leetcode.cn/problems/count-of-range-sum/)

给你一个整数数组 nums 以及两个整数 lower 和 upper 。求数组中，值位于范围 [lower, upper] （包含 lower 和 upper）之内的 区间和的个数 。

区间和 S(i, j) 表示在 nums 中，位置从 i 到 j 的元素之和，包含 i 和 j (i ≤ j)。

**示例 1：**

```c
输入：nums = [-2,5,-1], lower = -2, upper = 2
输出：3
解释：存在三个区间：[0,0]、[2,2] 和 [0,2] ，对应的区间和分别是：-2 、-1 、2 。
```

**示例 2：**

```c
输入：nums = [0], lower = 0, upper = 0
输出：1
```

**提示：**

- 1 <= nums.length <= 10^5^
- 2^31^ <= nums[i] <= 2^31^- 1
- -10^5^ <= lower <= upper <= 10^5^
- 题目数据保证答案是一个 32 位 的整数

**c++代码实现：**

```c
typedef long long ll;
class Solution {
public:
    /*
    题目求：
    (1)求出前缀和s数组，对于区间[j, i]，要算的就是i前面有多少个数，满足：
        lower<=S[i]-S[j]<=upper
    (2)那么如果给定S[i],变形得到： 
        S[i]-upper<=S[j]<=S[i]-lower
    (3)也就是对于某个位置i，要求一下前面有多少数在某一个范围内，求这个有很多种做法，最直观的就是用平衡树来做，但是手写平衡树太难了，可以用离散化+树状数组来做，相当于一个山寨版的平衡树。
    (4)可以把中间可能用到的所有数全都离散化掉，反正能保持相对的大小顺序就可以了。然后全都存到树状数组里，要求这个区间里有多少个s[j]时候只要算一下两个前缀，做个减法就可以了，也就是来算f(1到si-lower)以及f(1到si-upper-1)，所以只要离散化一下s[i]、s[i]-lower和s[i]-upper-1就可以了。
    */
    int n,m;
    vector<int> tr;
    vector<ll> numbers; //离散化的数组

    int lowbit(int x){
        return x&-x;
    }

    //这里tr里面维护的是numbers,所以需要遍历到m
    void add(int x,int v){
        for(int i=x;i<=m;i+=lowbit(i)) tr[i]+=v;
    }

    int query(int x){
        int res=0;
        for(int i=x;i;i-=lowbit(i)) res+=tr[i];
        return res;
    }

    // 离散化取数，即看一下x在numbers中的下标，返回下标+1
    //这里的x可能是ll
    int get(ll x){
        return lower_bound(numbers.begin(),numbers.end(),x)-numbers.begin()+1;
    }

    int countRangeSum(vector<int>& nums, int lower, int upper) {
        n=nums.size();
        vector<ll> s(n+1); //前缀和

        /*
        所求为si - upper ~ s1 - lower这个范围内满足的区间个数
        这个区间个数利用树状数组快速求除，即f(s1 - lower) - f(s1 - upper -1)
        所以树状树组中维护的数 前缀和的个数
        前缀和也有0的情况， 所以需要加入到树状数组 即 add(get(0), 1);
        因为对所有前缀和进行了离散化， 0属于前缀和，因此也需要离散化。*/
        numbers.push_back(0);

        //前缀和需要从小标1开始
        for(int i=1;i<=n;i++){
            s[i]=s[i-1]+nums[i-1];
            numbers.push_back(s[i]);
            numbers.push_back(s[i]-lower);
            numbers.push_back(s[i]-upper-1);
        }

        //离散化去重
        sort(numbers.begin(),numbers.end());
        numbers.erase(unique(numbers.begin(),numbers.end()),numbers.end());
        
        m=numbers.size(); //树状数组里面维护的数：前缀和的个数
        tr.resize(m+1);
        int res = 0;
        // 在树状数组中加入0离散化之后的结果，因为求区间是需要这个点的
        add(get(0),1);

        for(int i=1;i<=n;i++){
            res+=query(get(s[i]-lower))-query(get(s[i]-upper-1));
            // 每次求完之后，要把当前的s[i]的离散化结果加入到树状数组中
            // 这样才能保证每次遍历到i的时候树状数组里总是前面的所有数（的离散化结果）
            add(get(s[i]),1);
        }
        return res;
    }
};
```



### 2、线段树



## 十五、数学

### 1.质数

#### [204. 计数质数](https://leetcode.cn/problems/count-primes/)（线性筛法求质数）

给定整数 `n` ，返回 *所有小于非负整数 `n` 的质数的数量* 。

**示例 1：**

```c
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

**示例 2：**

```c
输入：n = 0
输出：0
```

**示例 3：**

```c
输入：n = 1
输出：0
```

**提示：**

- `0 <= n <= 5 * 106`

**c++代码：**

```c
// primes[]表示存储所有的素数
// st[x]存储x是否被筛掉
const int N=5e6+10;
bool st[N];
int primes[N];

class Solution {
public:
    
    int countPrimes(int n) {
        int res=0;
        for(int i=2;i<n;i++){
            //如果没有筛过则是质数
            if(!st[i]) primes[res++]=i;
            // n只有最小质因子筛掉
            //对于一个合数x,假设primes[j]是x的最小质因子，当i枚举到x/primes[j]的时候
            for(int j=0;primes[j]*i<n;j++){
                st[primes[j]*i]=true;
                if(i%primes[j]==0) break;
            }
        }
        return res;
    }
};
```

