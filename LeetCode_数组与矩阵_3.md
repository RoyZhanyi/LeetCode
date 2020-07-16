#### 1. [766. 托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)[easy]

> 如果矩阵上每一条由左上到右下的对角线上的元素都相同，那么这个矩阵是 托普利茨矩阵 。
>
> 给定一个 M x N 的矩阵，当且仅当它是托普利茨矩阵时返回 True。
>
> 示例 1:
>
> ```
> 输入: 
> matrix = [
>   [1,2,3,4],
>   [5,1,2,3],
>   [9,5,1,2]
> ]
> 输出: True
> 解释:
> 在上述矩阵中, 其对角线为:
> "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]"。
> 各条对角线上的所有元素均相同, 因此答案是True。
> ```
>
> 
>
> 示例 2:
>
> ```
> 输入:
> matrix = [
>   [1,2],
>   [2,2]
> ]
> 输出: False
> 解释: 
> 对角线"[1, 2]"上的元素不同。
> ```
>
> 
>
> 说明:
>
>  matrix 是一个包含整数的二维数组。
>
> matrix 的行数和列数均在 [1, 20]范围内。
>
> matrix[i][j] 包含的整数在 [0, 99]范围内。
>
> 进阶:
>
> 如果矩阵存储在磁盘上，并且磁盘内存是有限的，因此一次最多只能将一行矩阵加载到内存中，该怎么办？
> 如果矩阵太大以至于只能一次将部分行加载到内存中，该怎么办？
>

暴力法：

```c++
class Solution {
public:
    bool isToeplitzMatrix(vector<vector<int>>& matrix) {
        int m=matrix.size(),n=matrix[0].size();
        int c=0,r=0;
        for(int i=0;i<n-1;++i){
            c=0;
            r=i;
            int temp=matrix[c][r];
            while(c<m&&r<n){
                if(matrix[c++][r++]!=temp){
                    return false;
                }
            }
        }
        for(int i=0;i<m-1;++i){
            c=i;
            r=0;
            int temp=matrix[c][r];
            while(c<m&&r<n){
                if(matrix[c++][r++]!=temp){
                    return false;
                }
            } 
        }
        return true;
    }
};
```



#### 2. [565. 数组嵌套](https://leetcode-cn.com/problems/array-nesting/)[medium]

> 索引从`0`开始长度为`N`的数组`A`，包含`0`到`N - 1`的所有整数。找到最大的集合S并返回其大小，其中` S[i] = {A[i], A[A[i]], A[A[A[i]]], ... }`且遵守以下的规则。
>
> 假设选择索引为i的元素`A[i]`为`S`的第一个元素，`S`的下一个元素应该是`A[A[i]]`，之后是`A[A[A[i]]]... `以此类推，不断添加直到`S`出现重复的元素。
>
>  
>
> 示例 1:
>
> ```
> 输入: A = [5,4,0,3,1,6,2]
> 输出: 4
> 解释: 
> A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.
> 
> 其中一种最长的 S[K]:
> S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
> ```
>
>
> 提示：
>
> N是[1, 20,000]之间的整数。
> A中不含有重复的元素。
> A中的元素大小在[0, N-1]之间。

分析一:

利用`hash_table`来记录当前遍历过的数

问题：会导致`Time Limit Exceeded`

```c++
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        if(nums.size()==0)  return 0;
        int ans=0,len=nums.size();
        vector<int> temp(len);

        for(int i=0;i<len;++i){
            vector<bool>    hash_table(20001);
            int temp=nums[i],cnt=0;
            while(!hash_table[temp]){
                hash_table[temp]=true;
                temp=nums[temp];
                cnt++;
            }
            ans=max(ans,cnt);
        }
        return ans;
    }
};
```

分析二:

将访问过的数标记为`-1`，访问到`-1`直接跳出。

同一循环序列之内无论起点在何处，长度都是一样的。而不同的序列之间不会有交集。

```c++
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        int maxCnt = 0;
        for (int i = 0; i < nums.size(); i++) {
            int cnt = 0;
            for (int j = i; nums[j] != -1; ) {
                cnt++;
                int t = nums[j];
                nums[j] = -1; // 标记该位置已经被访问
                j = t;

        }
        maxCnt = max(maxCnt, cnt);
       }
        return maxCnt;
    }
};
```



#### 3. [769. 最多能完成排序的块](https://leetcode-cn.com/problems/max-chunks-to-make-sorted/)[medium]

> 数组`arr`是`[0, 1, ..., arr.length - 1]`的一种排列，我们将这个数组分割成几个“块”，并将这些块分别进行排序。之后再连接起来，使得连接的结果和按升序排序后的原数组相同。
>
> 我们最多能将数组分成多少块？
>
> 示例 1:
>
> ```
> 输入: arr = [4,3,2,1,0]
> 输出: 1
> 解释:
> 将数组分成2块或者更多块，都无法得到所需的结果。
> 例如，分成 [4, 3], [2, 1, 0] 的结果是 [3, 4, 0, 1, 2]，这不是有序的数组。
> ```
>
> 示例 2:
>
> ```
> 输入: arr = [1,0,2,3,4]
> 输出: 4
> 解释:
> 我们可以把它分成两块，例如 [1, 0], [2, 3, 4]。
> 然而，分成 [1, 0], [2], [3], [4] 可以得到最多的块数。
> ```
>
> 
>
> 注意:
>
> arr 的长度在 [1, 10] 之间。
> arr[i]是 [0, 1, ..., arr.length - 1]的一种排列。

分析：从左至右遍历数组，并且记录当前区间的最大值，当最大值与数组下标相同时，当前区间可分为一个“块”。
由于题目给出数组长度为[1,n]，故不需要判断数组是否为空。

```c++
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        int cnt=0;
        int maxNum=arr[0];
        for(int i=0;i<arr.size();++i){
            maxNum=max(maxNum,arr[i]);
            if(maxNum==i)    cnt++;
        }
        return cnt;
    }
};

```

