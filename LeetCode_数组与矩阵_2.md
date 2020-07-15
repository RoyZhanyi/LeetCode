#### 1. [378. 有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)[medium]

> 给定一个` n x n `矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 k 小的元素。
> 请注意，它是排序后的第 k 小元素，而不是第 k 个不同的元素。
>
>  
>
> 示例：
>
> ```
> matrix = [
>    [ 1,  5,  9],
>    [10, 11, 13],
>    [12, 13, 15]
> ],
> k = 8,
> ```
>
> 返回 `13`。
>
> 提示：
> 你可以假设 k 的值永远是有效的，`1 ≤ k ≤ n2 `。

暴力解法：

将矩阵中的数放入一维数组中，对其进行排序，返回第k个即可。

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int row=matrix.size(),col=matrix[0].size();
        vector<int> nums;
        for(int i=0;i<row;++i){
            for(int j=0;j<col;++j){
                nums.push_back(matrix[i][j]);
            }
        }
        sort(nums.begin(),nums.end());
        return nums[k-1];
    }
};
```

归并排序：

由于该矩阵相当于n个一维有序数组，可转化为从n个数组中求出第k小的元素（归并排序思想）。利用小根堆，每次出堆，并且将堆顶元素所在的一维数组后的元素入堆。

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        struct point {
            int val, x, y;
            point(int val, int x, int y) : val(val), x(x), y(y) {}
            bool operator> (const point& a) const { return this->val > a.val; }
        };
        priority_queue<point, vector<point>, greater<point>> que;
        int n = matrix.size();
        for (int i = 0; i < n; i++) {
            que.emplace(matrix[i][0], i, 0);
        }
        for (int i = 0; i < k - 1; i++) {
            point now = que.top();
            que.pop();
            if (now.y != n - 1) {
                que.emplace(matrix[now.x][now.y + 1], now.x, now.y + 1);
            }
        }
        return qu	e.top().val;
    }
};
```

二分法：

```c++


class Solution {
public:
  	bool check(vector<vector<int>> &matrix,int mid,int k ,int n){
      int i=n-1;
      int j=0;
      int num=0;
      while(i>=0&&j<n){
        if(matrix[i][j]<=mid){
          num+=i+1;
          j++;
        }else{
          i--;
        }
      }
      return num>=k;
    }
    int kthSmallest(vector<vector<int>>& matrix, int k) {
      int n=matrix.size();
      int left=matrix[0][0];
      int right=matrix[n-1][n-1];
      while(left<right){
        int mid=left+(right-left)>>1;
        if(check(matrix,mid,k,n)){
					right=mid;
        }else{
					left=mid+1;
        }
      }
      return left;
    }
};
```



#### 2. [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)[easy]

> 集合 S 包含从1到 n 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。
>
> 给定一个数组 nums 代表了集合 S 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。
>
> 示例 1:
>
> ```
> 输入: nums = [1,2,2,4]
> 输出: [2,3]
> ```
>
> 注意:
>
> 给定数组的长度范围是 [2, 10000]。
> 给定的数组是无序的。

暴力法：

利用hash_table来记录

```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<bool> hash_table(nums.size());
        vector<int> ans;
        for(int i=0;i<nums.size();++i){
           if( hash_table[nums[i]-1]){
               ans.push_back(nums[i]);
           }
           hash_table[nums[i]-1]=true;
        }
        for(int i=0;i<nums.size();++i){
            if(!hash_table[i]){
                ans.push_back(i+1);
                break;
            }
        }
        return ans;
    }
};
```

使用异或运算：

对1到n以及nums中的数进行异或运算，得到结果`x^y`（x、y分别为重复数和缺失数）。以`x^y`位中最右边不为0的位`rightmostbit`为基准，将nums和1到n分别分成两个部分，可将x与y分开。nums与1到n 中与`rightmostbit`异或为1的两部分进行异或、得出一个数，nums与1到n 中与`rightmostbit`异或为0的两部分进行异或、得出一个数，这两个数就是x与y。

```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int Xor=0,Xor0=0,Xor1=0;
        for(int n:nums)
            Xor^=n;
        for(int i=1;i<=nums.size();++i)
            Xor^=i;
        int rightmostbit=Xor & ~(Xor-1);
        for(int n:nums){
            if(n&rightmostbit)  Xor1^=n;
            else
                Xor0^=n;
        }
        for(int i=1;i<=nums.size();++i){
            if(i&rightmostbit)  Xor1^=i;
            else
                Xor0^=i;
        }
        for(int i = 0;i <nums.size(); ++i){
            if(nums[i]==Xor0)
                return vector<int>{Xor0,Xor1};
        }
        return vector<int>{Xor1,Xor0};
    }
};
```



#### 3. [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)[medium]

> 给定一个包含 n + 1 个整数的数组 nums，其数字都在 1 到 n 之间（包括 1 和 n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。
>
> 示例 1:
>
> ```
> 输入: [1,3,4,2,2]
> 输出: 2
> ```
>
> 示例 2:
>
> ```
> 输入: [3,1,3,4,2]
> 输出: 3
> ```
>
> 说明：
>
> 不能更改原数组（假设数组是只读的）。
> 只能使用额外的 O(1) 的空间。
> 时间复杂度小于 O(n2) 。
> 数组中只有一个重复的数字，但它可能不止重复出现一次。

二分查找法：

用`left`与`right`来记录当前查找区间的最大值与最小值，并且令`mid=（left+right）/2`。遍历整个数组,并记录数组中的值比`mid`小或等于的个数`cnt`。若`cnt>mid`，则证明小于等于`mid`的数中有重复数值，令`right=mid-1`,反之，则相反。

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums){
      int left=1,right=nums.size()-1;
      while(left<=right){
        int mid=(left+right)/2;
        int cnt=0;
        for(int i=0;i<nums.size();++i){
          if(nums[i]<=mid)	cnt++;
        }
        if(cnt>mid)	right=mid-1;
        else	left=mid+1;
      }
      return left;
    }
};
```



#### 4. [667. 优美的排列 II](https://leetcode-cn.com/problems/beautiful-arrangement-ii/)[medium]

> 给定两个整数` n` 和 `k`,你需要实现一个数组，这个数组包含从 1 到 n 的 n 个不同整数，同时满足以下条件：
>
> ① 如果这个数组是 [a1, a2, a3, ... , an] ，那么数组 [|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|] 中应该有且仅有 k 个不同整数；.
>
> ② 如果存在多种答案，你只需实现并返回其中任意一种.
>
> 示例 1:
>
> ```
> 输入: n = 3, k = 1
> 输出: [1, 2, 3]
> 解释: [1, 2, 3] 包含 3 个范围在 1-3 的不同整数， 并且 [1, 1] 中有且仅有 1 个不同整数 : 1
> ```
>
>
> 示例 2:
>
> ```
> 输入: n = 3, k = 2
> 输出: [1, 3, 2]
> 解释: [1, 3, 2] 包含 3 个范围在 1-3 的不同整数， 并且 [2, 1] 中有且仅有 2 个不同整数: 1 和 2
> ```
>
>
> 提示:
>
> ` n `和 `k` 满足条件 `1 <= k < n <= 104.`

分析：

假设n=8

当k=1时，[1,2,3,4,5,6,7,8] 满足题意，不需要翻转；

当k=2时，[1,8,7,6,5,4,3,2]满足题意，对后边`n-1`个数反转一次即可；

当k=3时，[1,8,2,3,4,5,6,7]满足题意，对后边`n-1`个数反转一次，再对后边`n-2`个数反转一次;

........

当k>1时,需要翻转的次数为`k-1`次，每次翻转保留前`m(m = 1,2,3...k-1)`个数，每次翻转都在原数组进行

```c++
class Solution {
public:
    vector<int> constructArray(int n, int k){
        vector<int> nums(n);
        for(int i=0;i<n;++i)
            nums[i]=i+1;
        for(int i=1;i<k;++i){
            reverse(nums,i,n-1);
        }
        return nums;
    }
private:
    void reverse(vector<int>& nums,int left,int right){
        while(left<right){
            int temp=nums[left];
            nums[left]=nums[right];
            nums[right]=temp;
            ++left;
            --right;
        }
    }
};
```

#### 5. [697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)[easy]

> 给定一个非空且只包含非负数的整数数组 nums, 数组的度的定义是指数组里任一元素出现频数的最大值。
>
> 你的任务是找到与 nums 拥有相同大小的度的最短连续子数组，返回其长度。
>
> 示例 1:
>
> ```
> 输入: [1, 2, 2, 3, 1]
> 输出: 2
> 解释: 
> 输入数组的度是2，因为元素1和2的出现频数最大，均为2.
> 连续子数组里面拥有相同度的有如下所示:
> [1, 2, 2, 3, 1], [1, 2, 2, 3], [2, 2, 3, 1], [1, 2, 2], [2, 2, 3], [2, 2]
> 最短连续子数组[2, 2]的长度为2，所以返回2.
> ```
>
> 示例 2:
>
> ```
> 输入: [1,2,2,3,1,4,2]
> 输出: 6
> ```
>
> 注意:
>
> nums.length 在1到50,000区间范围内。
> nums[i] 是一个在0到49,999范围内的整数。

分析：

相同大小度的最短连续子数组，通过查找出现次数最多的数的最左边的坐标`left`与最右边的坐标`right`，此时`right-left+1`就是所求值。如[1,2,2,3,4]。2出现次数最多，且left=1，right=2，则结果为2。

```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        map<int,int>   left,right,count;
        for(int i=0;i<nums.size();++i){
            int num=nums[i];
            if(left.find(num)==left.end())  left[num]=i;
            right[num]=i;
            count[num]+=1;
        }
        int degree=-1;
        int ans=nums.size();
        for(auto it:count){
            if(it.second>degree)
                degree=it.second;
        }
        for(int i=0;i<nums.size();++i){
            if(count[nums[i]]==degree){
                int temp=right[nums[i]]-left[nums[i]]+1;
                if(temp<ans)    ans=temp;
            }
        }
        return ans;
    }
};
```

