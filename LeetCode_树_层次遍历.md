

**当开始遍历一层时，当前队列中的节点数就是该层的节点数，可以用一个cnt来控制遍历当前层的节点**

#### 1. [637. 二叉树的层平均值](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)[easy]

> 给定一个非空二叉树, 返回一个由每层节点平均值组成的数组.
>
> 示例 1:
>
> 输入:
>
> ```input
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> 输出: [3, 14.5, 11]
> 解释:
> 第0层的平均值是 3,  第1层是 14.5, 第2层是 11. 因此返回 [3, 14.5, 11].
> 注意：
>
> 节点值的范围在32位有符号整数范围内。
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
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        if(root==nullptr)   return ans;
        TreeNode* cur=root;
        queue<TreeNode*> que;
        que.push(cur);
        while(!que.empty()){
            int cnt=que.size();
            double sum=0;
            for(int i=0;i<cnt;++i){
                cur=que.front();
                que.pop();
                sum+=cur->val;
                if(cur->left!=nullptr) que.push(cur->left);
                if(cur->right!=nullptr)    que.push(cur->right);
            }
            ans.push_back(sum/cnt);
        }
        return ans;
    }
};
```



#### 2. [513. 找树左下角的值](https://leetcode-cn.com/problems/find-bottom-left-tree-value/)[medium]

> 给定一个二叉树，在树的最后一行找到最左边的值。
>
> 示例 1:
>
> 输入:
>
>     	2
>      / \
>     1   3  
> 输出:
> 1
>
>
> 示例 2:
>
> 输入:
>
>         1
>        / \
>       2   3
>      /   / \
>     4   5   6
>        /
>       7
>
> 输出:
> 7
>
> 注意: 您可以假设树（即给定的根节点）不为 NULL。

题解一：与上述思想相同，

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
    int findBottomLeftValue(TreeNode* root) {
        int ans;
        TreeNode* cur=root;
        queue<TreeNode*> que;
        que.push(cur);
        while(!que.empty()){
            int cnt=que.size();
            double sum=0;
            for(int i=0;i<cnt;++i){
               cur=que.front();
               que.pop();
               if(i==0) ans=cur->val;
               if(cur->left!=nullptr)  que.push(cur->left);
               if(cur->right!=nullptr)    que.push(cur->right);
            }
        }
        return ans;
    }
};
```

题解二：

***思路***：利用队列进行从右到左层次遍历，遍历到最后一个节点即为最左下角的节点

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
    int findBottomLeftValue(TreeNode* root) {
        TreeNode* cur=root;
        queue<TreeNode*> que;
        que.push(cur);
        while(!que.empty()){
            cur=que.front();
            que.pop();
            if(cur->right!=nullptr)    que.push(cur->right);
            if(cur->left!=nullptr)  que.push(cur->left);
        }
        return cur->val;
    }
};
```

