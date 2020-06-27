#### 1. [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)[easy]

> 使用栈实现队列的下列操作：
>
> * push(x) -- 将一个元素放入队列的尾部。
> * pop() -- 从队列首部移除元素。
> * peek() -- 返回队列首部的元素。
> * empty() -- 返回队列是否为空。
>
>
> 示例:
>
> ```
> MyQueue queue = new MyQueue();
> 
> queue.push(1);
> queue.push(2);  
> queue.peek();  // 返回 1
> queue.pop();   // 返回 1
> queue.empty(); // 返回 false
> ```
>
> 
>
>
> 说明:
>
> * 你只能使用标准的栈操作 -- 也就是只有 `push to top`,` peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
> * 你所使用的语言也许不支持栈。你可以使用 `list` 或者 `deque（双端队列）`来模拟一个栈，只要是标准的栈操作即可。
> * 假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

思路：

* 用两个栈模拟一个队列

```c++
class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {}
    /** Push element x to the back of queue. */
    void push(int x) {
        s1.push(x);
    }
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        int temp=s2.top();
        s2.pop();
        return temp;
    }
    
    /** Get the front element. */
    int peek() {
        if(s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        return s2.top();
    }
    /** Returns whether the queue is empty. */
    bool empty() {
        if(s1.empty()&&s2.empty()){
            return true;
        }
        return false;
    }
private:
    stack<int> s1;
    stack<int> s2;
};
```

#### 2. [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)[easy]

> 使用队列实现栈的下列操作：
>
> * push(x) -- 元素 x 入栈
> * pop() -- 移除栈顶元素
> * top() -- 获取栈顶元素
> * empty() -- 返回栈是否为空
>
> 注意:
>
> * 你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和 `is empty `这些操作是合法的。
> * 你所使用的语言也许不支持队列。 你可以使用 `list` 或者 `deque（双端队列）`来模拟一个队列 , 只要是标准的队列操作即可。
> * 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 `pop` 或者` top `操作）。

思路：

* 在将一个元素 x 插入队列时，为了维护原来的后进先出顺序，需要让 x 插入队列首部。
* 而队列的默认插入顺序是队列尾部，因此在将 x 插入队列尾部之后，需要让除了 x 之外的所有元素出队列，再入队列。

```c++
class MyStack {
public:
    /** Initialize your data structure here. */
    MyStack() {

    }
    /** Push element x onto stack. */
    void push(int x) {
        int cnt=myQueue.size();
        myQueue.push(x);
        for(int i=0;i<cnt;++i){
            int temp=myQueue.front();
            myQueue.pop();
            myQueue.push(temp);
        }
    }
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int temp=myQueue.front();
        myQueue.pop();
        return temp;
    }
    /** Get the top element. */
    int top() {
        return myQueue.front();
    }
    /** Returns whether the stack is empty. */
    bool empty() {
        return myQueue.empty();
    }
private:
    queue<int> myQueue;
};
```



#### 3. [155. 最小栈](https://leetcode-cn.com/problems/min-stack/)[easy]

> 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
>
> * `push(x) `—— 将元素 x 推入栈中。
> * `pop()` —— 删除栈顶的元素。
> * `top()` —— 获取栈顶元素。
> * `getMin()` —— 检索栈中的最小元素。
>
>
> 示例:
>
> 输入：
>
> ```input
> ["MinStack","push","push","push","getMin","pop","top","getMin"]
> [[],[-2],[0],[-3],[],[],[],[]]
> ```
>
> 输出：
>
> ```output
> [null,null,null,null,-3,null,0,-2]
> ```
>
> 解释：
>
> ```
> MinStack minStack = new MinStack();
> minStack.push(-2);
> minStack.push(0);
> minStack.push(-3);
> minStack.getMin();   --> 返回 -3.
> minStack.pop();
> minStack.top();      --> 返回 0.
> minStack.getMin();   --> 返回 -2.
> ```
>
>
> 提示：
>
> `pop`、`top` 和 `getMin `操作总是在 非空栈 上调用。

思路：

* 由于multiset自带排序特性，且可以存放相同的值，所以multi可以 以常数的时间复杂度 获取当前栈中最小值

```c++
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {
    }
    
    void push(int x) {
        myStack.push(x);
        mySet.insert(x);
    }
    
    void pop() {
        auto it=mySet.find(myStack.top());
        mySet.erase(it);
        //这里不能直接erase(myStack.top()),假如有重复数据的话，会把整个重复数据给删除掉
      	//例如 mySet中有 0,0,1 直接erase(myStack.top())会删除两个0，而mySet.erase(it)只会删除某一位置的数据
        myStack.pop();
    }
    
    int top() {
        return myStack.top();
    }
    
    int getMin() {
        auto it=mySet.begin();
        return *it;
    }
private:
    stack<int> myStack;
    multiset<int> mySet;
};

```

#### 4. [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)[easy]

> 给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']' `的字符串，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 1. 左括号必须用相同类型的右括号闭合。
> 2. 左括号必须以正确的顺序闭合。
> 3. 注意空字符串可被认为是有效字符串。
>
> 示例 1:
>
> ```
> 输入: "()"
> 输出: true
> ```
>
> 示例 2:
>
> ```
> 输入: "()[]{}"
> 输出: true
> ```
>
> 示例 3:
>
> ```
> 输入: "(]"
> 输出: false
> ```
>
> 示例 4:
>
> ```
> 输入: "([)]"
> 输出: false
> ```
>
> 示例 5:
>
> ```
> 输入: "{[]}"
> 输出: true
> 
> ```

思路：

* 利用`stack`规则即可，`'('`、`'{'`、`'['`进栈；`')'`、`'}'`、`']'`检查栈顶元素是否与当前匹配，若匹配则出栈

注意：

* `')'`、`'}'`、`']'`检查栈顶元素时，可能当前栈为空
* 在遍历完字符串s之后，还要检查一下当前栈是否为空

```c++
class Solution {
public:
    bool isValid(string s) {
        if(s.empty())   return true;
        int len=s.size();
        for(int i=0;i<len;++i){
            if(s[i]=='('||s[i]=='{'||s[i]=='['){
                myStack.push(s[i]);
            }else{
                if(myStack.empty()) return false;
                if(s[i]==')'){
                    if(myStack.top()!='('){
                         return false;
                    }
                }else if(s[i]=='}'){
                    if(myStack.top()!='{'){
                        return false;
                    }
                }else if(s[i]==']'){
                    if(myStack.top()!='['){
                        return false;
                    }
                }else{
                    return false;
                }
                myStack.pop();
            }
        }
        if(!myStack.empty()){
            return false;
        }
        return true;
    }
private:
    stack<char> myStack;
};
```

#### 5.	[739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)[medium]

> 请根据每日 气温 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 0 来代替。
>
> 例如，给定一个列表` temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。
>
> 提示：气温 列表长度的范围是` [1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。
>

第一次答案：时间复杂度为`n^2`

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int len=T.size();
        vector<int> ans(len);
        for(int i=0;i<len;++i){
            for(int j=i+1;j<len;++j){
                if(T[j]>T[i]){
                    ans[i]=j-i;
                    break;
                }
            }
        }
        return ans;
    }
};
```

超时：

> ## Time Limit Exceeded
>
> - 34/37 cases passed (N/A)
>
> ### Testcase
>
> //测试用例非常多

正确思路：

* 在遍历数组时用栈把数组中的数存起来，如果当前遍历的数比栈顶元素来的大，说明栈顶元素的下一个比它大的数就是当前元素。
* 说明：当一个元素入栈时，非栈顶元素肯定比栈顶元素要大（可以手动模拟一下过程）

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int len=T.size();
        vector<int> ans(len);
        stack<int> myStack;
        for(int curIndex=0;curIndex<len;++curIndex){
            while(!myStack.empty() && T[curIndex]>T[myStack.top()]){
                int preIndex=myStack.top();
                myStack.pop();
                ans[preIndex]=curIndex-preIndex;
            }
            myStack.push(curIndex);
        }
        return ans;
    }
};
```

#### 6. [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)[medium]

> 给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。
>
> 示例 1:
>
> ```
> 输入: [1,2,1]
> 输出: [2,-1,2]
> 解释: 第一个 1 的下一个更大的数是 2；
> 数字 2 找不到下一个更大的数； 
> 第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
> ```
>
> **注意:** 输入数组的长度不会超过 10000。

来自柳神的分析：

> 分析:二叉搜索树的中序遍历的结果恰好是所有数的递增序列列，根据中序遍历结果，对于当前遍历结 点，标记maxCount为最⼤大出现次数，tempCount为当前数字出现的次数，currentVal为当前保存的 值。 ⾸首先，tempCount++表示当前的数字出现次数+1，如果当前结点的值不不等于保存的值，就更更新 currentVal的值，并且将tempCount标记为1~ 接下来，如果tempCount即当前数字出现的次数⼤大于 maxCount，就更更新maxCount，并且将result数组清零，并将当前数字放⼊入result数组中;如果 tempCount只是等于maxCount，说明是出现次数⼀一样的，则将当前数字直接放⼊入result数组中~

```c++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n=nums.size();
        vector<int> next(n);
        for(int i=0;i<n;++i){
             next[i]=-1;
        }
        stack<int> pre;
        for(int i=0; i< n*2;++i){
            int num=nums[i%n];
            while(!pre.empty() && nums[pre.top()]<num){
                next[pre.top()]=num;
                pre.pop();
            }
            if(i<n){
                pre.push(i);
            }
        }
        return next;
    }
};
```

