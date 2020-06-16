#### 1.  [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)[easy]

> 给定一个二叉树，找出其最大深度。
>
> 二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
>
> 说明: 叶子节点是指没有子节点的节点。
>
> 示例：
> 给定二叉树 [3,9,20,null,null,15,7]，
>
>     		3
>        / \
>       9  20
>         /  \
>        15   7
> 返回它的最大深度 3 。

没有AC的答案：

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr)
            return 0;
        return (maxDepth(root->left)>maxDepth(root->right)?maxDepth(root->left):maxDepth(root->right))+1;
    }
};
```

会提示以下信息：

> ## Time Limit Exceeded
>
> - 37/39 cases passed (N/A)
>
> ### Testcase
>
> ```
> 很长一串 测试用例，不再列举
> ```

改进后答案：

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr)
            return 0;
        int leftDepth=maxDepth(root->left);
        int rightDepth=maxDepth(root->right);
        return leftDepth>rightDepth?leftDepth:rightDepth+1;
    }
};
```

**分析原因**

` return (maxDepth(root->left)>maxDepth(root->right)?maxDepth(root->left):maxDepth(root->right))+1;`这个语句中，maxDepth执行了三次，大大浪费了时间(也没必要重复执行一次，浪费感情)。改进后maxDepth()执行两次即可。



#### 2. [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)[easy]

> 给定一个二叉树，判断它是否是高度平衡的二叉树。
>
> 本题中，一棵高度平衡二叉树定义为：
>
> 一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。
>
> 示例 1:
>
> 给定二叉树 [3,9,20,null,null,15,7]
>
>     		3
>        / \
>       9  20
>         /  \
>        15   7
>
> 返回 true 。
>
> 示例 2:
>
> 给定二叉树 [1,2,2,3,3,null,null,4,4]
>
>     	 		1
>      	 	 / \
>      	  2   2
>     	 / \
>      	3   3
>      / \
>     4   4
>
> 返回 false 。



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
    int flag=1;
    int dfs(TreeNode* root){
        if(root==nullptr){
             return 0;//保留
        }
        int l,r;
        if(root->left==nullptr){
            l=0;
        }else{
            l=dfs(root->left);
        }
        if(root->right==nullptr){
            r=0;
        }else{
            r=dfs(root->right);
        }
        if(abs(l-r)>=2){
            flag=0;
        }
        return (l>r?l:r)+1;
    }
    bool isBalanced(TreeNode* root) {
        dfs(root);
        return flag;
    }
};
```

#### 3. [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)[easy]

> 给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。
>
>  
>
> 示例 :
> 给定二叉树
>
>           1
>          / \
>         2   3
>        / \     
>       4   5    
> 返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。
>
>  
>
> 注意：两结点之间的路径长度是以它们之间边的数目表示。

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
    int maxDiameter=0;
    int dfs(TreeNode* root){
        if(root==nullptr)
            return 0;
        int l=dfs(root->left);
        int r=dfs(root->right);
        maxDiameter=max(maxDiameter,l+r);
        return (l>r?l:r)+1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
       dfs(root);
       return maxDiameter;
    }
};
```

**注意**

`(l>r?l:r)+1`与`l>r?l:r+1`结果是不一样的，因为`?:`优先级要比`+`低。

#### 4. [226. 翻转二叉树](https://leetcode-cn.com/problems/invert-binary-tree/)[easy]

超级简单的一个递归



> 翻转一棵二叉树。
>
> 示例：
>
> 输入：
>
>     			4
>         /   \
>       2      7
>      / \   	/ \
>     1   3  6   9
>
> 输出：
>
>      			4
>         /   \
>       7      2
>      / \    / \
>     9   6  3   1
>
> 备注:
> 这个问题是受到 Max Howell 的 原问题 启发的 ：
>
> 谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。
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
    TreeNode* invertTree(TreeNode* root) {
        if(root==nullptr||root->left==nullptr&&root->right==nullptr)
            return root;
        invertTree(root->left);
        invertTree(root->right);
        TreeNode *t=root->left;
        root->left=root->right;
        root->right=t;
        return root;
    }
};
```

#### 5. [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)[easy]

> 给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
>
> 你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。
>
> 示例 1:
>
> 输入: 
>
> ```
> 				Tree 1                     Tree 2                  
>           1                         2                             
>          / \                       / \                            
>      		3   2                     1   3                        
>    		 /                         	  \   \                      
>   	 	5                           	  4   7               
> ```
>
> 
>
> 输出: 
> 合并后的树:
>
> ```
>        3
> 	    / \
> 	   4   5
> 	  / \   \ 
> 	 5   4   7
> ```
>
> 
>
> 注意: 合并必须从两个树的根节点开始。

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
    TreeNode* mergeTrees(TreeNode* t1, TreeNode* t2) {
        if(t1!=nullptr&&t2!=nullptr){
            t1->val+=t2->val;
            t1->left=mergeTrees(t1->left,t2->left);
            t1->right=mergeTrees(t1->right,t2->right);
            return t1;
        }else if(t1==nullptr){
            return t2;
        }else{
            return t1;
        }
    }
};
```

#### 6. [112. 路径总和](https://leetcode-cn.com/problems/path-sum/)[easy]

> 给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。
>
> 说明: 叶子节点是指没有子节点的节点。
>
> 示例: 
> 给定如下二叉树，以及目标和 sum = 22，
>
>               5
>              / \
>             4   8
>            /   / \
>           11  13  4
>          /  \      \
>         7    2      1
> 返回 true, 因为存在目标和为 22 的根节点到叶子节点的路径 5->4->11->2。

题解：

```C++
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
    int target=0,flag=0;
    void dfs(TreeNode* root,int sum){
        if(root==nullptr)   return ;
        sum+=root->val;
        if(root->left==nullptr&&root->right==nullptr){
            if(sum==target)
                flag=1;
        }
        dfs(root->left,sum);
        dfs(root->right,sum);

    }
    bool hasPathSum(TreeNode* root, int sum) {
        target=sum;
        dfs(root,0);
        return flag;
    }
};
```

#### 7. [437. 路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)[easy]

***标记：如何解释此题重复计算。***

> 给定一个二叉树，它的每个结点都存放着一个整数值。
>
> 找出路径和等于给定数值的路径总数。
>
> 路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
>
> 二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。
>
> 示例：
>
> root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
>
>      		  10
>     	   /  \
>       	5   -3
>        / \    \
>       3   2   11
>      / \   \
>     3  -2   1
> 返回 3。和等于 8 的路径有:
>
> 1.  5 -> 3
> 2.  5 -> 2 -> 1
> 3.  -3 -> 11
>

未AC的答案：

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
    int count=0,target;
    void dfs(TreeNode* root,int curSum){
        if(root==nullptr)
            return ;
        curSum+=root->val;
        if(curSum==target)  count++;
        dfs(root->left,0);
        dfs(root->left,curSum);
        dfs(root->right,0);
        dfs(root->right,curSum);
    }
    int pathSum(TreeNode* root, int sum) {
        target=sum;
        dfs(root,0);
        return count;
    }
};
```

出现错误：

> ## Wrong Answer
>
> - 100/126 cases passed (N/A)
>
> ### Testcase
>
> ```
> [1,null,2,null,3,null,4,null,5]
> 3
> ```
>
> ### Answer
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

**分析错误：**

```c++
  **会出现重复计算**
	上述的节点3重复计算了两次
  从上往下就会出现这种情况
  为什么会重复计算呢？
```





参考柳神答案：

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
    int result = 0;
    int pathSum(TreeNode* root, int sum) {
        if(root == NULL) return 0;
        pathSum(root->left, sum); 
        pathSum(root->right, sum); 
        dfs(root, sum);
        return result;
    }
    void dfs(TreeNode* root, int sum) { 
        if(root == NULL) return ; 
        if(root->val == sum) 
            result++; 
        dfs(root->left, sum - root->val); 
        dfs(root->right, sum - root->val);
		} 
};
```

**思路：**从下至上，以当前节点为根的树是否有符合题意的

#### 8. [572. 另一个树的子树](https://leetcode-cn.com/problems/subtree-of-another-tree/)[easy]

> 给定两个非空二叉树 s 和 t，检验 s 中是否包含和 t 具有相同结构和节点值的子树。s 的一个子树包括 s 的一个节点和这个节点的所有子孙。s 也可以看做它自身的一棵子树。
>
> 示例 1:
> 给定的树 s:
>
>     		 3
>     		/ \
>        4   5
>       / \
>      1   2
>
> 给定的树 t：
>
> ```a
>    4 
>   / \
>  1   2
> ```
>
> 返回 true，因为 t 与 s 的一个子树拥有相同的结构和节点值。
>
> 示例 2:
> 给定的树 s：
>
>     	    3
>     	   / \
>         4   5
>     	 / \
>       1   2
>      /
>     0
>
> 给定的树 t：
>
> ```a
>    4 
>   / \
>  1   2
> ```
>
> 返回 false。

题解：

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
    bool dfsCompare(TreeNode* t1,TreeNode* t2){
        if(t1==nullptr&&t2==nullptr){
            return true;
        }else if(t1==nullptr){
            return false;
        }else if(t2==nullptr){
            return false;
        }else{
            if(t1->val==t2->val){
                return dfsCompare(t1->left,t2->left)&&dfsCompare(t1->right,t2->right);
            }else{
                return false;
            }
        }
    }
    int flag=0;
    void dfs(TreeNode* s,TreeNode* t){
        if(s==nullptr)  return ;
        if(s->val==t->val){
           if(dfsCompare(s,t)) 
            flag=1;
        }
        dfs(s->left,t);
        dfs(s->right,t);
    }
    bool isSubtree(TreeNode* s, TreeNode* t) {
        dfs(s,t);
        return flag;
    }
};
```

柳神简洁答案

```c++
class Solution {
public:
    bool isSubtree(TreeNode* s, TreeNode* t) {
        if (s == NULL && t == NULL) return true;
        if (s == NULL || t == NULL) return false;
        if (s->val == t->val && isSame(s, t)) return true; 
        return isSubtree(s->left, t) || isSubtree(s->right, t);
        } 
private:
    bool isSame(TreeNode* r, TreeNode* t) {
        if (r == NULL && t == NULL) return true;
        if (r == NULL || t == NULL || r->val != t->val) return false; 
        return (isSame(r->left, t->left) && isSame(r->right, t->right)); 
    }
};  
```

