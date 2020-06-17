#### 1.[101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)[easy]

> 给定一个二叉树，检查它是否是镜像对称的。
>
>  
>
> 例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
>
>     		1
>        / \
>       2   2
>      / \ / \
>     3  4 4  3
> 
>
>
> 但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
>
>     		1
>        / \
>       2   2
>        \   \
>        3    3
> 
>
>
> 进阶：
>
> 你可以运用递归和迭代两种方法解决这个问题吗？
>

递归解法：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSame(TreeNode* t1,TreeNode* t2){
        if(t1==nullptr&&t2==nullptr)    return true;
        if(t1==nullptr||t2==nullptr)    return false;
        if(t1->val==t2->val){
            return isSame(t1->left,t2->right)&&isSame(t1->right,t2->left);
        }else{
            return false;
        }
    }
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr||(root->left==nullptr&&root->right==nullptr))    
            return true;
        return isSame(root->left,root->right);
    }
};
```





#### 2. [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)[easy]

> 给定一个二叉树，找出其最小深度。
>
> 最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
>
> 说明: 叶子节点是指没有子节点的节点。
>
> 示例:
>
> 给定二叉树 [3,9,20,null,null,15,7],
>
>     	3
>      / \
>     9  20
>       /  \
>      15   7
> 返回它的最小深度  2.

题解：

```c++
lass Solution {
public:
    int minDepth(TreeNode* root) {
       if(root==nullptr)
            return 0;
       dfs(root,0);
       return mDepth; 
    }
private:
    int mDepth=10000;
    void dfs(TreeNode* root,int depth){
        if(root==nullptr)   return ;
        if(root->left==nullptr&&root->right==nullptr){
            if(depth+1<mDepth)   
                 mDepth=depth+1;
            return ;
        }
        dfs(root->left,depth+1);
        dfs(root->right,depth+1);
    }

};
```



#### 3. [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)[easy]

> 计算给定二叉树的所有左叶子之和。
>
> 示例：
>
>     		3
>        / \
>       9  20
>         /  \
>        15   7
> 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
>

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        dfs(root,'r');
        return sum;
    }
private:
    int sum=0;
    void dfs(TreeNode* root,char flag){
        if(root==nullptr)   return ;
        if(root->left==nullptr&&root->right==nullptr&&flag=='l'){
            sum+=root->val;
            return ;
        }
        dfs(root->left,'l');
        dfs(root->right,'r');
    }
};
```

#### 4. [687. 最长同值路径](https://leetcode-cn.com/problems/longest-univalue-path/)[easy]

> 给定一个二叉树，找到最长的路径，这个路径中的每个节点具有相同值。 这条路径可以经过也可以不经过根节点。
>
> 注意：两个节点之间的路径长度由它们之间的边数表示。
>
> 示例 1:
>
> 输入:
>
>               5
>              / \
>             4   5
>            / \   \
>           1   1   5
> 输出:
>
> 2
> 示例 2:
>
> 输入:
>
>               1
>              / \
>             4   5
>            / \   \
>           4   4   5
> 输出:
>
> 2
> 注意: 给定的二叉树不超过10000个结点。 树的高度不超过1000

第一次提交未AC：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    int longestUnivaluePath(TreeNode* root) {
        dfs(root,nullptr,0);
        return maxCount;
    }

private:
    int maxCount=-1;
    void dfs(TreeNode* root,TreeNode* pre,int count){
        if(root==nullptr){
            if(count>maxCount)  maxCount=count;
            return ;
        }
        if(pre!=nullptr&&root->val==pre->val){
            count++;
        }else{
            count=1;
        }
        dfs(root->left,root,count);
        dfs(root->right,root,count);
    }
};
```

输出错误

> ## Finished
>
> ### Your Input
>
> ```
> [5,4,5,1,1,5]
> ```
>
> ### Output (0 ms)
>
> ```
> 3
> ```
>
> ### Expected Answer
>
> ```
> 2
> ```



第二次未AC：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    int longestUnivaluePath(TreeNode* root) {
        dfs(root);
        return maxCount;
    }

private:
    int maxCount=-1;
    int dfs(TreeNode* root){
        int count=0;
        if(root==nullptr){
            return 0;
        }
        if(root->left==nullptr&&root->right==nullptr)
            return 0;
        
        if(root->left!=nullptr){
            if(root->val==root->left->val)   
                count=count+dfs(root->left);
        }
        if(root->right!=nullptr){
            if(root->val==root->right->val)   
                count=count+dfs(root->right)+1;
        }
        if(maxCount<count){
            maxCount=count;
        }
      return count;
    }
};
```

提示错误：

> ## Wrong Answer
>
> - 24/71 cases passed (N/A)
>
> ### Testcase
>
> ```
> [1,4,5,4,4,5]
> ```
>
> ### Answer
>
> ```
> 0
> ```
>
> ### Expected Answer
>
> ```
> 2
> ```

AC答案

```c++
// @lc code=start
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    int longestUnivaluePath(TreeNode* root) {
        dfs(root);
        return maxCount;
    }

private:
    int maxCount=0;
    int dfs(TreeNode* root){
        int count=0;
        if(root==nullptr){
            return 0;
        }
        int left=dfs(root->left);
        int right=dfs(root->right);
        int leftPath=0,rightPath=0;
        if(root->left!=nullptr){
            if(root->val==root->left->val)   
                leftPath=left+1;
        }
        if(root->right!=nullptr){
            if(root->val==root->right->val)   
                rightPath=right+1;
        }
        if(maxCount<(leftPath+rightPath)){
            maxCount=leftPath+rightPath;
        }
      return max(leftPath,rightPath);
    }
};
```





#### 5. [337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)[medium]

> 在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。
>
> 计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。
>
> 示例 1:
>
> 输入: [3,2,3,null,3,null,1]
>
>      		3
>     	 / \
>       2   3
>        \   \ 
>         3   1
>  
>
> 输出: 7 
> 解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
> 示例 2:
>
> 输入: [3,4,5,1,3,null,1]
>
>          3
>         / \
>        4   5
>       / \   \ 
>      1   3   1
>   
>
> 输出: 9
> 解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        if(root==nullptr)   return 0;
        int val1 = root->val;
        if(root->left!=nullptr)
            val1+=rob(root->left->left)+rob(root->left->right);
        if(root->right!=nullptr)
            val1+=rob(root->right->left)+rob(root->right->right);
        int val2=rob(root->left)+rob(root->right);
        if(val1>val2)   
            return val1;
        return val2;
    }
};
```



#### 6. [671. 二叉树中第二小的节点](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)[easy]

> 给定一个非空特殊的二叉树，每个节点都是正数，并且每个节点的子节点数量只能为 2 或 0。如果一个节点有两个子节点的话，那么这个节点的值不大于它的子节点的值。 
>
> 给出这样的一个二叉树，你需要输出所有节点中的第二小的值。如果第二小的值不存在的话，输出 -1 。
>
> 示例 1:
>
> 输入: 
>
> ```input
>  2
> / \
> 2   5
>   / \
>  5   7
> ```
>
> 输出: 5
> 说明: 最小的值是 2 ，第二小的值是 5 。
> 示例 2:
>
> 输入: 
>
> ```output
>  2
> / \
> 2   2
> ```
>
> 输出: -1
> 说明: 最小的值是 2, 但是不存在第二小的值。

#### 

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int findSecondMinimumValue(TreeNode* root) {
        dfs(root);
        if(ans.size()<2)    return -1;
        auto it=ans.begin();
        ++it;
        return *it;
    }
private:
    set<int> ans;
    void dfs(TreeNode* root){
        if(root==nullptr)   return ;
        ans.insert(root->val);
        dfs(root->left);
        dfs(root->right);
    }
};
```

