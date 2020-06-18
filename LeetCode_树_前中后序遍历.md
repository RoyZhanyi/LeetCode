#### 1. [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)[medium]

> 给定一个二叉树，返回它的 前序 遍历。
>
>  示例:
>
> 输入: [1,null,2,3]  
>
> ```input
>    1
>     \
>      2
>     /
>    3 
> ```
>
> 输出: [1,2,3]
> 进阶: 递归算法很简单，你可以通过迭代算法完成吗？

非递归实现：

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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ans;
        if(root==nullptr)   return ans;
        stack<TreeNode*> st;
        st.push(root);
        while(!st.empty()){
            TreeNode* p=st.top();
            ans.push_back(p->val);
            st.pop();
            if(p->right!=nullptr)    st.push(p->right);
            if(p->left!=nullptr)    st.push(p->left);
        }
        return ans;
    }
};
```

#### 2. [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)[hard]

> 给定一个二叉树，返回它的 后序 遍历。
>
> 示例:
>
> 输入: [1,null,2,3]  
>
>    ```input
> 	1
>     \
>      2
>     /
>    3 
>    ```
>
> 输出: [3,2,1]
> 进阶: 递归算法很简单，你可以通过迭代算法完成吗？

分析：前序遍历的顺序为root->left->right，后序遍历的顺序为left->right->root，将前序遍历变成root->right->left再反转即可得出结果。

非递归实现：

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
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        if(root==nullptr)   return ans;
        stack<TreeNode*> s;
        TreeNode *p=root;
        s.push(p);
        while(!s.empty()){
            p=s.top();
            s.pop();
            ans.push_back(p->val);
            if(p->left!=nullptr)    s.push(p->left);
            if(p->right!=nullptr)   s.push(p->right);
        } 
        reverse(ans.begin(),ans.end());
        return ans;
    }
};
```

#### 3. [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)[medium]

> 给定一个二叉树，返回它的中序 遍历。
>
> 示例:
>
> 输入: [1,null,2,3]
>
> ```input
>    1
>     \
>      2
>     /
>    3
> ```
>
> 输出: [1,3,2]
> 进阶: 递归算法很简单，你可以通过迭代算法完成吗？

非递归实现：

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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ans;
        if(root==nullptr)   return ans;
        stack<TreeNode *> s;
        TreeNode *cur=root;
        while(cur!=nullptr||!s.empty()){
            while(cur!=nullptr){
                s.push(cur);
                cur=cur->left;
            }
            TreeNode* temp=s.top();
            s.pop();  
            ans.push_back(temp->val);//遍历没有左节点的节点
            cur=temp->right;//左节点与当前节点都遍历以后 遍历当前节点的右节点
        }
        return ans;
    }
};
```



