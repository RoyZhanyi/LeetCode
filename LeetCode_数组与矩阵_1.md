#### 1. [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)[easy]

> 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。
>
> 示例:
>
> ```
> 输入: [0,1,0,3,12]
> 输出: [1,3,12,0,0]
> ```
>
> 说明:
>
> 必须在原数组上操作，不能拷贝额外的数组。
> 尽量减少操作次数。

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
       int len=nums.size(),j=0;
       for(int i = 0;i < len;++i){
           if(nums[i]!=0){
               nums[j]=nums[i];
               ++j;
           }
       }
       while(j<len){
           nums[j++]=0;
       }
    }
};
```

#### 2. [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)[easy]

> 在MATLAB中，有一个非常有用的函数 reshape，它可以将一个矩阵重塑为另一个大小不同的新矩阵，但保留其原始数据。
>
> 给出一个由二维数组表示的矩阵，以及两个正整数r和c，分别表示想要的重构的矩阵的行数和列数。
>
> 重构后的矩阵需要将原始矩阵的所有元素以相同的行遍历顺序填充。
>
> 如果具有给定参数的reshape操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。
>
> 示例 1:
>
> ```
> 输入: 
> nums = 
> [[1,2],
>  [3,4]]
> r = 1, c = 4
> 输出: 
> [[1,2,3,4]]
> 
> 
> 解释:
> 行遍历nums的结果是 [1,2,3,4]。新的矩阵是 1 * 4 矩阵, 用之前的元素值一行一行填充新矩阵。
> ```
>
> 
>
> 示例 2:
>
> ```
> 输入: 
> nums = 
> [[1,2],
>  [3,4]]
> r = 2, c = 4
> 输出: 
> [[1,2],
>  [3,4]]
> 解释:
> 没有办法将 2 * 2 矩阵转化为 2 * 4 矩阵。 所以输出原矩阵。
> ```
>
> 
>
> 注意：
>
> 给定矩阵的宽和高范围在 [1, 100]。
> 给定的 r 和 c 都是正数。

分析：当矩阵中的数量不等于`r*c`时，不能转化；转化时，第`index`个数组位于数组的`nums[index/n][index%n]`位置(假设n为列数)。

```c++
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        int m=nums.size(),n=nums[0].size();
        if(m*n!=r*c)    return nums;
        int index=0;
        vector<vector<int>> ans(r,vector<int>(c));
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
               ans[i][j]=nums[index/n][index%n];
               index++;
            }
        }
        return ans;
    }
};
```



#### 3. [485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)[easy]

> 给定一个二进制数组， 计算其中最大连续1的个数。
>
> 示例 1:
>
> ```
> 输入: [1,1,0,1,1,1]
> 输出: 3
> 解释: 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.
> ```
>
> 注意：
>
> 输入的数组只包含 0 和1。
> 输入数组的长度是正整数，且不超过 10,000。

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int maxNum=0,cnt=0;
        for(int i=0;i<nums.size();++i){
            if(nums[i]==1){
                cnt++;
            }else{
                cnt=0;
            }
         	 maxNum=max(maxNum,cnt);
        }
       return maxNum;
    }
};
```

#### 4. [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)[medium]

> 编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：
>
> 每行的元素从左到右升序排列。
> 每列的元素从上到下升序排列。
> 示例:
>
> 现有矩阵 matrix 如下：
>
> ```
> [
>   [1,   4,  7, 11, 15],
>   [2,   5,  8, 12, 19],
>   [3,   6,  9, 16, 22],
>   [10, 13, 14, 17, 24],
>   [18, 21, 23, 26, 30]
> ]
> ```
>
> 
>
> 给定 `target = 5`，返回` true`。
>
> 给定 `target = 20`，返回 `false`。

分析：初始化指向右上角的指针`matrix[row][col]`;当`target`小于当前值时`--col`；当`target`大于当前值时`++row`

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0||matrix[0].size()==0)
                return false;
       int m=matrix.size(),n=matrix[0].size();
       int row=0,col=n-1;
       while(row<m&&col>=0){
           if(target==matrix[row][col]) return true;
           else if(target>matrix[row][col]) ++row;
           else --col;
       }
       return false;
       }
};
```

