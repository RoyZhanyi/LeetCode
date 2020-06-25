#### 1. [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)[easy]

> 将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。
>
> 本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。
>
> 示例:
>
> 给定有序数组: [-10,-3,0,5,9],
>
> 一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：
>
>       		 0
>      			/ \
>         -3   9
>         /   /
>       -10  5

**思路**：

* 题目要求高度平衡二叉树。
* 对于有序数组来讲，以当前数组中元素的正中间的值`（start+end）/2`建立根，正中间左侧建立左子树，右侧建立右子树

题解：

```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return toBST(nums,0,nums.size()-1);
    }
private:
    TreeNode* toBST(vector<int>& nums,int startIndex,int endIndex){
        if(startIndex>endIndex) return nullptr;
        int mIndex=(startIndex+endIndex)/2;
        TreeNode* root=new TreeNode(nums[mIndex]);
        root->left=toBST(nums,startIndex,mIndex-1);
        root->right=toBST(nums,mIndex+1,endIndex);
        return root;
    }
};
```



#### 2. [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)[medium]

> 给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。
>
> 本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。
>
> 示例:
>
> 给定的有序链表： [-10, -3, 0, 5, 9],
>
> 一个可能的答案是：[0, -3, 9, -10, null, 5], 它可以表示下面这个高度平衡二叉搜索树：
>
>       	 0
>      		/ \
>       -3   9
>       /   /
>     -10  5

**思路**：

* 方法一沿用上题思路，先将链表转换成vector数组，剩余思路与109题雷同
* 方法二利用快慢指针找到中间位上的节点，也类似于上题的构造思想

方法一：

```c++
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
       exchange(head);
       return toBST(nums,0,nums.size()-1); 
    }
private:
    vector<int> nums;
    void exchange(ListNode* head){
        while(head){
            nums.push_back(head->val);
            head=head->next;
        }
    }
    TreeNode* toBST(vector<int>& nums,int low,int high){
        if(low>high)    return nullptr;
        int mid=(low+high)/2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=toBST(nums,low,mid-1);
        root->right=toBST(nums,mid+1,high);
        return root;
    }
};
```

方法二：

```c++
class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        return func(head, NULL);
    }
    TreeNode* func(ListNode* head, ListNode* tail) {
        ListNode *fast = head, *slow = head;
        if (head == tail) return NULL;
        while (fast != tail && fast->next != tail) {
            fast = fast->next->next;
            slow = slow->next; 
        }
        TreeNode* root = new TreeNode(slow->val);
        root->left = func(head, slow); 
        root->right = func(slow->next, tail); 
        return root;
    } 
 };
```



#### 3. [653. 两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)[easy]

> 给定一个二叉搜索树和一个目标结果，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 true。
>
> 案例 1:
>
> 输入: 
>
> ```input
>     5
>    / \
>   3   6
>  / \   \
> 2   4   7
> 
> Target = 9
> ```
>
> 输出: True
>
>
> 案例 2:
>
> 输入: 
>
> ```input
>     5
>    / \
>   3   6
>  / \   \
> 2   4   7
> 
> Target = 28
> ```
>
> 
>
> 输出: False

**思路:**

* 二叉搜索树的中序遍历为有序，将二叉搜索树遍历成有序数组，利用两个指针`left`·`right`对有序数组进行查找

```c++
class Solution {
public:
    bool findTarget(TreeNode* root, int k) {
        midOrder(root);
        int left=0,right=nums.size()-1;
        while(left<right){
            if(nums[left]+nums[right]==k) 
                return true;
            if(nums[left]+nums[right]<k){
                ++left;
            }else{
                --right;
            }
        }
        return false;
    }
private:
    vector<int> nums;
    void midOrder(TreeNode* root){
       if(root==nullptr)   return ;
       midOrder(root->left);
       nums.push_back(root->val);
       midOrder(root->right);
    }

};
```

#### 4. [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)[easy]

> 给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。
>
> 示例：
>
> 输入：
>
> ```input
>    1
>     \
>      3
>     /
>    2
> ```
>
> 输出：
> 1
>
> 解释：
> 最小绝对差为 1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
>
>
> 提示：
>
> 树中至少有 2 个节点。
> 本题与 783 https://leetcode-cn.com/problems/minimum-distance-between-bst-nodes/ 相同

**思路**：

* 由于树与链表 之间不好做一些操作，一般先转化为数组再对其进行操作

```C++
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        toNums(root);
        return getMini();
    }
private:
    vector<int> nums;
    void toNums(TreeNode* root){//中序遍历
        if(root==nullptr)   return ;
        toNums(root->left);
        nums.push_back(root->val);
        toNums(root->right);
    }
    int getMini(){
        unsigned int minNum=0xffffffff;
        int len=nums.size();
        for(int i = 1; i < len; ++i){
            int a=abs(nums[i]-nums[i-1]);
            if(a<minNum)    minNum=a;
        }
        return minNum;
    }
};
```



#### 5. [501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)[easy]

> 	给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。
>
> 假定 BST 有如下定义：
>
> 结点左子树中所含结点的值小于等于当前结点的值
> 结点右子树中所含结点的值大于等于当前结点的值
> 左子树和右子树都是二叉搜索树
> 例如：
> 给定 BST [1,null,2,2],
>
> ```input
>    1
>     \
>      2
>     /
>    2
> ```
>
> 
>
> 返回[2].
>
> 提示：如果众数超过1个，不需考虑输出顺序
>
> 进阶：你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）
>
> 
>

**思路：**

* 由于map底层是红黑树，以及map中元素插入的特殊性`可用map[关键字]访问`，考虑使用map进行存储遍历结果

```c++
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
       dfs(root);
       vector<int> ans;
       for(auto it=myMapp.begin();it!=myMapp.end();++it){
           if(it->second==maxRepeat){
               ans.push_back(it->first);
           }    
       } 
       return ans;
    }
private:
    map<int,int> myMapp;
    int maxRepeat=-1;
    void dfs(TreeNode* root){
        if(root==nullptr)   return ;
        myMapp[root->val]++;
        if(myMapp[root->val]>maxRepeat){
            maxRepeat=myMapp[root->val];
        }
        dfs(root->left);
        dfs(root->right);
    }
};
```

