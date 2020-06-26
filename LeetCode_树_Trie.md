#### 0.Trie的介绍

* LeetCode:https://leetcode-cn.com/problems/implement-trie-prefix-tree/solution/shi-xian-trie-qian-zhui-shu-by-leetcode/

* 维基百科：https://zh.wikipedia.org/wiki/Trie

* 很清爽的模板

  * c++

    ```c++
    class Trie
    {
    private:
    	bool is_string = false;
    	Trie* next[26] = { nullptr };
    public:
    	Trie() {}
    
    	void insert(const string& word)//插入单词
    	{
    		Trie* root = this;
    		for (const auto& w : word) {
    			if (root->next[w - 'a'] == nullptr)root->next[w - 'a'] = new Trie();
    			root = root->next[w - 'a'];
    		}
    		root->is_string = true;
    	}
    
    	bool search(const string& word)//查找单词
    	{
    		Trie* root = this;
    		for (const auto& w : word) {
    			if (root->next[w - 'a'] == nullptr)return false;
    			root = root->next[w - 'a'];
    		}
    		return root->is_string;
    	}
    
    	bool startsWith(const string& prefix)//查找前缀
    	{
    		Trie* root = this;
    		for (const auto& p : prefix) {
    			if (root->next[p - 'a'] == nullptr)return false;
    			root = root->next[p - 'a'];
    		}
    		return true;
    	}
    };
    ```

    

  * Java

    ```c++
    public class Trie {
        private boolean is_string=false;
        private Trie next[]=new Trie[26];
    
        public Trie(){}
    
        public void insert(String word){//插入单词
            Trie root=this;
            char w[]=word.toCharArray();
            for(int i=0;i<w.length;++i){
                if(root.next[w[i]-'a']==null)root.next[w[i]-'a']=new Trie();
                root=root.next[w[i]-'a'];
            }
            root.is_string=true;
        }
    
        public boolean search(String word){//查找单词
            Trie root=this;
            char w[]=word.toCharArray();
            for(int i=0;i<w.length;++i){
                if(root.next[w[i]-'a']==null)return false;
                root=root.next[w[i]-'a'];
            }
            return root.is_string;
        }
        
        public boolean startsWith(String prefix){//查找前缀
            Trie root=this;
            char p[]=prefix.toCharArray();
            for(int i=0;i<p.length;++i){
                if(root.next[p[i]-'a']==null)return false;
                root=root.next[p[i]-'a'];
            }
            return true;
        }
    }
    ```

#### 1.[208. 实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)[medium]

> 实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。
>
> 示例:
>
> ```shili
> Trie trie = new Trie();
> 
> trie.insert("apple");
> trie.search("apple");   // 返回 true
> trie.search("app");     // 返回 false
> trie.startsWith("app"); // 返回 true
> trie.insert("app");   
> trie.search("app");     // 返回 true
> ```
>
> 说明:
>
> 你可以假设所有的输入都是由小写字母 a-z 构成的。
> 保证所有输入均为非空字符串。

```c++
class Trie {
private:
    bool is_string=false;
    Trie* next[26]={nullptr};
public:
    /** Initialize your data structure here. */
    Trie() {
    }
    /** Inserts a word into the trie. */
    void insert(string word) {
        Trie* root=this;
        for(auto &w:word){
            int index=w-'a';
            if(root->next[index]==nullptr) 
                root->next[index]=new Trie();
            root=root->next[index];
        }
        root->is_string=true;
    }
    
    /** Returns if the word is in the trie. */
    bool search(string word) {
        Trie* root=this;
        for(auto& w:word){
            int index=w-'a';
            if(root->next[index]==nullptr)  return false;
            root=root->next[index];
        }
        return root->is_string;
    }
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        Trie* root=this;
        for(auto& w:prefix){
            int index=w-'a';
            if(root->next[index]==nullptr)  return false;
            root=root->next[index]; 
        }
        return true;
    }
};
```



#### 2. [677. 键值映射](https://leetcode-cn.com/problems/map-sum-pairs/)[medium]

> 实现一个 MapSum 类里的两个方法，`insert` 和 `sum`。
>
> 对于方法` insert`，你将得到一对（字符串，整数）的键值对。字符串表示键，整数表示值。如果键已经存在，那么原来的键值对将被替代成新的键值对。
>
> 对于方法` sum`，你将得到一个表示前缀的字符串，你需要返回所有以该前缀开头的键的值的总和。
>
> 示例 1:
>
> ```shili
> 输入: insert("apple", 3), 输出: Null
> 输入: sum("ap"), 输出: 3
> 输入: insert("app", 2), 输出: Null
> 输入: sum("ap"), 输出: 5
> ```

```c++
class MapSum {
private:
    int val=0;
    int valSum=0;
    bool isLeaf=false;
    MapSum* next[26]={nullptr};
public:
    /** Initialize your data structure here. */
    MapSum() {
    }
    void insert(string key, int val) {
        MapSum* root=this;
        for(auto &w:key){
            int index=w-'a';
            if(root->next[index]==nullptr)  
                root->next[index]=new MapSum();
            root=root->next[index];
        }
        if(root->isLeaf){
            int difference=val-root->val;
            root->val=val;
            root=this;
            for(auto &w:key){
                int index=w-'a';
                if(root->next[index]==nullptr)  
                   root->next[index]=new MapSum();
                root->next[index]->valSum+=difference;
                root=root->next[index];
            }
        }else{
            root->isLeaf=true;
            root->val=val;
            root=this;
            for(auto &w:key){
                int index=w-'a';
                if(root->next[index]==nullptr)  
                    root->next[index]=new MapSum();
                root->next[index]->valSum+=val;
                root=root->next[index];
            }
        }
    }
    int sum(string prefix) {
        MapSum* root=this;
        for(auto &w:prefix){
            int index=w-'a';
            if(root->next[index]==nullptr)  return 0;
            root=root->next[index];
        } 
        return root->valSum;
    }
};
```

