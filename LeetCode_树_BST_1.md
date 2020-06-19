#### 1. [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)[easy]

> 给定一个二叉搜索树，同时给定最小边界L 和最大边界 R。通过修剪二叉搜索树，使得所有节点的值在[L, R]中 (R>=L) 。你可能需要改变树的根节点，所以结果应当返回修剪好的二叉搜索树的新的根节点。
>
> 示例 1:
>
> 输入: 
>
> ```input
>     1
>    / \
>   0   2
> ```
>
>   L = 1
>   R = 2
>
> 输出: 
>
> ```input
>    1
>     \
>      2
> ```
>
> 示例 2:
>
> 输入: 
>
> ```input
>     3
>    / \
>   0   4
>    \
>     2
>    /
>   1
> ```
>
> 
>
>   L = 1
>   R = 3
>
> 输出: 
>
> ```input
>      3
>     / 
>    2   
>   /
>  1
> ```

**未AC答案：**

```c++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        while(root!=nullptr&&root->val<L)   root=root->right;
        while(root!=nullptr&&root->val>R)   root=root->left;
        if(root==nullptr)   return nullptr;
        root->left=trimBST(root->left,L,R);
        root->right=trimBST(root->right,L,R);
        return root;
    }
};
```

**提示错误：**

> ## Wrong Answer
>
> - 74/77 cases passed (N/A)
>
> ### Testcase
>
> ```
> [45,30,46,10,36,null,49,8,24,34,42,48,null,4,9,14,25,31,35,41,43,47,null,0,6,null,null,11,20,null,28,null,33,null,null,37,null,null,44,null,null,null,1,5,7,null,12,19,21,26,29,32,null,null,38,null,null,null,3,null,null,null,null,null,13,18,null,null,22,null,27,null,null,null,null,null,39,2,null,null,null,15,null,null,23,null,null,null,40,null,null,null,16,null,null,null,null,null,17]
> ' +
>   '32
> ' +
>   '44
> ```
>
> ### Answer
>
> ```
> [30,null,36,34,42,33,35,41,43,32,null,null,null,37,null,null,44,null,null,null,38,null,null,null,39,null,40]
> ```
>
> ### Expected Answer
>
> ```
> [36,34,42,33,35,41,43,32,null,null,null,37,null,null,44,null,null,null,38,null,null,null,39,null,40]
> ```

**AC答案：**

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
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        int flag=0;
        while(root!=nullptr&&root->val<L){
            root=root->right;
            flag=1;
        }
        while(root!=nullptr&&root->val>R){
            root=root->left;
            flag=1;
        }
        if(root==nullptr)   return nullptr;
        if(flag)
            root=trimBST(root,L,R);
        root->left=trimBST(root->left,L,R);
        root->right=trimBST(root->right,L,R);
        return root;
    }
};
```

**分析第一次未能AC原因:**

> 按照第一次的算法来看，是因为先检查`root->val<L`再检查`root->val>R`,检查完`root->val>R`后可能当前`root`的值还不满足`L<=root->val<=R`,需要再检查是否满足`root->val<L`
>
> 在这里我使用一个`flag`来标记当前的root是否发生了更改（即不满足`L<=root->val<=R`）若`flag==0`	证明当前root未做更改，若`flag==1`,证明当前root发生了改变，需要对当前的root再一次检查。

**参考答案：**

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
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if(root==nullptr)   return root;
        if(root->val<L) return trimBST(root->right,L,R);//直接舍弃根节点以及左子树了
        if(root->val>R)   return trimBST(root->left,L,R);
        root->left=trimBST(root->left,L,R);
        root->right=trimBST(root->right,L,R);
        return root;
    }  
};
```



#### 2. [230. 二叉搜索树中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/)[medium]

> 给定一个二叉搜索树，编写一个函数 kthSmallest 来查找其中第 k 个最小的元素。
>
> 说明：
> 你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。
>
> 示例 1:
>
> 输入: root = [3,1,4,null,2], k = 1
>
> ```input
>    3
>   / \
>  1   4
>   \
>    2
> ```
>
> 输出: 1
> 示例 2:
>
> 输入: root = [5,3,6,2,4,null,null,1], k = 3
>
> ```input
>        5
>       / \
>      3   6
>     / \
>    2   4
>   /
>  1
> ```
>
> 输出: 3
> 进阶：
> 如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第 k 小的值，你将如何优化 kthSmallest 函数？

**题解：**

```c++
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
    int kthSmallest(TreeNode* root, int k) {
        dfs(root);
  			return ans[k-1];
    }
private:
    set<int> ans;
    void dfs(TreeNode* root){
        if(root==nullptr)   return ;
        dfs(root->left);
        ans.insert(root->val);
        dfs(root->right);
    }
};
```

**简洁做法：**

```c++
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
    vector<int> nums;
    int kthSmallest(TreeNode* root, int k) {
        if (root == NULL || nums.size() >= k) return nums[k-1]; 
        if (root->left != NULL) kthSmallest(root->left, k);
        nums.push_back(root->val);
        if (root->right != NULL) kthSmallest(root->right, k); 
        if(nums.size()>=k)
            return nums[k - 1];
        return 0;
    }
};
```

#### 3. [538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)[easy]

> 给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。
>
>  
>
> 例如：
>
> 输入: 原始二叉搜索树:
>
>  ```input
>      5
>    /   \
>   2     13
>  ```
>
> 输出: 转换为累加树:
>
> ```output
>      18
>     /   \
>   20     13
> ```
>
> 
>
>
> 注意：本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同
>

**题解（参考）：**

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
    TreeNode* convertBST(TreeNode* root) {
       midOrder(root);
       return root;
    }
private:
    int sum=0;
    void midOrder(TreeNode* root){
        if(root==nullptr)   return ;
        midOrder(root->right);
        sum+=root->val;
        root->val=sum;
        midOrder(root->left);
    }
};
```

**分析：**

由于一个节点要加上比他所有要大的节点的值，用一个sum来记录比当前节点大的值的总和。

在**“找比当前节点大的节点”**时，可以借鉴排序树的中序遍历（遍历的结果是从小到大），在中序遍历的思想上，先遍历右子树再遍历左子树。

#### 4. [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)[easy]

> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
>
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
> 例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]
>
> <img src="https://s1.ax1x.com/2020/06/19/NuvLZR.png" alt="NuvLZR.png" style="zoom:67%;" />
>
> 示例 1:
>
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
> 输出: 6 
> 解释: 节点 2 和节点 8 的最近公共祖先是 6。
> 示例 2:
>
> 输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
> 输出: 2
> 解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
>
>
> 说明:
>
> 所有节点的值都是唯一的。
> p、q 为不同节点且均存在于给定的二叉搜索树中。

**题解(参考)**

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root->val>p->val&&root->val>q->val)	
            return lowestCommonAncestor(root->left,p,q);
        if(root->val<p->val&&root->val<q->val)	
            return lowestCommonAncestor(root->right,p,q);
        return root;
    }
};
```

***与669题思路有点像？？？***



#### 5. [236. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)[medium]

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
>
> 百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
>
> 例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
>
> ![NKSCVA.png](https://s1.ax1x.com/2020/06/19/NKSCVA.png)
>
> 示例 1:
>
> ```1
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
> 输出: 3
> 解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
> ```
>
> 示例 2:
>
> ```2
> 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
> 输出: 5
> 解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
> ```
>
> 说明:
>
> 所有节点的值都是唯一的。
> p、q 为不同节点且均存在于给定的二叉树中。

**题解**（参考）：

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
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root==nullptr||root==p||root==q) return root;
        TreeNode* left=lowestCommonAncestor(root->left,p,q);
        TreeNode* right=lowestCommonAncestor(root->right,p,q);
        return left == nullptr?right:right==nullptr?left:root;
    }
};
```

#### 小结：

`二叉树找公共祖先`之前就是自己的薄弱项，这个地方要多练几道题！！！

