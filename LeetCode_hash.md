#### 1. [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)[easy]

> 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
>
> 你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
>
>  
>
> 示例:
>
> ```example
> 给定 nums = [2, 7, 11, 15], target = 9
> 
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]
> ```

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int>   myHash;
        vector<int> ans;
        int len=nums.size();
        for(int i=0;i<len;++i){
            if(myHash.find(target-nums[i])!=myHash.end()){
                ans.push_back(i);
                ans.push_back(myHash[target-nums[i]]);
            }else{
                myHash[nums[i]]=i;
            }
        }
        return ans;
    }
};
```



#### 2. [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)[easy]

> 给定一个整数数组，判断是否存在重复元素。
>
> 如果任意一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。
>
>  
>
> 示例 1:
>
> ```example
> 输入: [1,2,3,1]
> 输出: true
> ```
>
> 示例 2:
>
> ```example
> 输入: [1,2,3,4]
> 输出: false
> ```
>
> 示例 3:
>
> ```example
> 输入: [1,1,1,3,3,4,3,2,4,2]
> 输出: true
> ```

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        set<int> mySet;
        int len=nums.size();
        for(int i=0;i<len;++i){
            mySet.insert(nums[i]);
        }
        return mySet.size()<nums.size();
    }
};
```



#### 3. [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)[easy]

> 和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。
>
> 现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。
>
> 示例 1:
>
> ```example
> 输入: [1,3,2,2,5,2,3,7]
> 输出: 5
> 原因: 最长的和谐数组是：[3,2,2,2,3].
> ```
>
> 说明: 输入的数组长度最大不超过20,000.

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        int maxCnt=0,len=nums.size();
        map<int,int> myMapp;
        for(int i=0;i<len;++i){
            myMapp[nums[i]]++;
            if(myMapp.find(nums[i]-1)!=myMapp.end()){
                maxCnt=max(maxCnt,myMapp[nums[i]]+myMapp[nums[i]-1]);
            }
           if(myMapp.find(nums[i]+1)!=myMapp.end()){
                maxCnt=max(maxCnt,myMapp[nums[i]]+myMapp[nums[i]+1]);
            }
        }
        return maxCnt;
    }
};
```



#### 4. [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)[hard]

> 给定一个未排序的整数数组，找出最长连续序列的长度。
>
> 要求算法的时间复杂度为 O(n)。
>
> 示例:
>
> ```example
> 输入: [100, 4, 200, 1, 3, 2]
> 输出: 4
> 解释: 最长连续序列是 [1, 2, 3, 4]。它的长度为 4。
> ```

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int len=nums.size();
        if(len==0||len==1)  return len;
        set<int> mySet;
        for(int i=0;i<len;++i){
            mySet.insert(nums[i]);
        }
        auto it=mySet.begin();
        int preNum=*it,curNum,curCnt=1,maxCnt=0;
        ++it;
        for(;it!=mySet.end();++it){
            curNum=*it;
            if(curNum==preNum+1){
                curCnt++;
            }else{
                maxCnt=max(maxCnt,curCnt);
                curCnt=1;
            }
            preNum=curNum;
        }
        maxCnt=max(maxCnt,curCnt);
        return maxCnt;
    }
};
```

