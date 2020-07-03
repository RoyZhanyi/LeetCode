#### 1. [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)[easy]

> 给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
>
> 示例 1:
>
> ```example
> 输入: s = "anagram", t = "nagaram"
> 输出: true
> ```
>
> 示例 2:
>
> ```example2
> 输入: s = "rat", t = "car"
> 输出: false
> ```
>
> 说明:
> 你可以假设字符串只包含小写字母。
>
> 进阶:
> 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        vector<int> s1_hash(26),s2_hash(26);
        int len=s.size();
        for(int i=0;i<len;++i){
            s1_hash[s[i]-'a']++;
        }
        len=t.size();
        for(int i=0;i<len;++i){
            s2_hash[t[i]-'a']++;
        }
        for(int i=0;i<26;++i){
            if(s1_hash[i]!=s2_hash[i])
                return false;
        }
        return true;
    }
};
```



#### 2. [409. 最长回文串](https://leetcode-cn.com/problems/longest-palindrome/)[easy]

> 给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。
>
> 在构造过程中，请注意区分大小写。比如 "Aa" 不能当做一个回文字符串。
>
> 注意:
> 假设字符串的长度不会超过 1010。
>
> 示例 1:
>
> 输入:
> "abccccdd"
>
> 输出:
> 7
>
> 解释:
> 我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

分析：

> 每个字符有偶数个可以用来构成回文字符串
>
> 回文字符串最中间的字符可以单独出现，如果有单独的字符就把它放在最中间

```c++
class Solution {
public:
    int longestPalindrome(string s) {
        vector<int> cnts(256);
        for(auto c:s){
            cnts[c]++;
        }
        int ans=0;
        for(int i=0;i<256;++i){
            ans+=cnts[i]/2*2;
        }
        if(ans<s.size()){
            ans++;
        }
        return ans;
    }
};
```



#### 3. [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)[easy]

> 给定两个字符串 s 和 t，判断它们是否是同构的。
>
> 如果 s 中的字符可以被替换得到 t ，那么这两个字符串是同构的。
>
> 所有出现的字符都必须用另一个字符替换，同时保留字符的顺序。两个字符不能映射到同一个字符上，但字符可以映射自己本身。
>
> 示例 1:
>
> ```example
> 输入: s = "egg", t = "add"
> 输出: true
> ```
>
> 示例 2:
>
> ```example
> 输入: s = "foo", t = "bar"
> 输出: false
> ```
>
> 示例 3:
>
> ```example
> 输入: s = "paper", t = "title"
> 输出: true
> ```
>
> 
>
> 说明:
> 你可以假设 s 和 t 具有相同的长度。

方法一：

> 对比字符串中每一个字符出现的第一个位置是否相同（用`find`）

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
       for(int index=0;index<s.size();++index){
           if(s.find(s[index])!=t.find(t[index])){
               return false;
           }
       }
       return true;
    }
};
```

方法二：

> 考虑到`find`效率低，对其进行改进，用hash

```c++
class Solution{
public:
  bool isIsomorphic(string s,string t){
    vector<int> indexOfs(256),indexOft(256);
    for(int index=0;index<s.size();++index){
      if(indexOfs[s[index]]!=indexOft[t[index]]){
        return false;
      }
      indexOfs[s[index]]=index+1;
      indexOft[t[index]]=index+1;//+1是因为vector数组默认值为0
    }
    return true;
  }
};
```



#### 4. [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)[medium]

> 给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。
>
> 具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被计为是不同的子串。
>
> 示例 1:
>
> ```example
> 输入: "abc"
> 输出: 3
> 解释: 三个回文子串: "a", "b", "c".
> ```
>
> 示例 2:
>
> ```example
> 输入: "aaa"
> 输出: 6
> 说明: 6个回文子串: "a", "a", "a", "aa", "aa", "aaa".
> ```
>
> 
>
> 注意:
>
> 输入的字符串长度不会超过1000。

分析：

> 中心扩展法
> 这是一个比较巧妙的方法，实质的思路和动态规划的思路类似。
>
> 比如对一个字符串`ababa`，选择最中间的`a`作为中心点，往两边扩散，第一次扩散发现`left`指向的是`b`，`right`指向的也是`b`，所以是回文串，继续扩散，同理`ababa`也是回文串。
>
> 这个是确定了一个中心点后的寻找的路径，然后我们只要寻找到所有的中心点，问题就解决了。
>
> 中心点一共有多少个呢？看起来像是和字符串长度相等，但你会发现，如果是这样，上面的例子永远也搜不到abab，想象一下单个字符的哪个中心点扩展可以得到这个子串？似乎不可能。所以中心点不能只有单个字符构成，还要包括两个字符，比如上面这个子串`abab`，就可以有中心点ba扩展一次得到，所以最终的中心点由`2 * len - 1`个，分别是`len`个单字符和`len - 1`个双字符。
>
> 如果上面看不太懂的话，还可以看看下面几个问题：
>
> 为什么有 `2 * len - 1 `个中心点？
> `abc` 有5个中心点，分别是` a、b、c、ab、ba`
> `abba` 有7个中心点，分别是 `a、b、b、a、ab、bb、ba`
> 什么是中心点？
> 中心点即left指针和right指针初始化指向的地方，可能是一个也可能是两个
> 为什么不可能是三个或者更多？
> 因为3个可以由1个扩展一次得到，4个可以由两个扩展一次得到

```c++
class Solution {
public:
    int countSubstrings(string s) {
        int N = s.length(), ans = 0;
        for (int center = 0; center <= 2*N-1; ++center) {
            int left = center / 2;
            int right = left + center % 2;
            while (left >= 0 && right < N && s[left] == s[right]) {
                ans++;
                left--;
                right++;
            }
        }
        return ans;
    }
};
```



#### 5. [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)[easy]

> 判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
>
> 示例 1:
>
> ```example
> 输入: 121
> 输出: true
> ```
>
> 示例 2:
>
> ```example
> 输入: -121
> 输出: false
> 解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
> ```
>
> 
>
> 示例 3:
>
> ```example
> 输入: 10
> 输出: false
> 解释: 从右向左读, 为 01 。因此它不是一个回文数。
> ```
>
> 
>
> 进阶:
>
> 你能不将整数转为字符串来解决这个问题吗？
>

分析：

> 首先，我们应该处理一些临界情况。所有负数都不可能是回文，例如：`-123 `不是回文，因为` - `不等于 `3`。所以我们可以对所有负数返回` false`。除了` 0` 以外，所有个位是` 0 `的数字不可能是回文，因为最高位不等于 `0`。所以我们可以对所有大于 `0` 且个位是 `0` 的数字返回` false`。
>
> 现在，让我们来考虑如何反转后半部分的数字。
>
> 对于数字` 1221`，如果执行` 1221 % 10`，我们将得到最后一位数字 `1`，要得到倒数第二位数字，我们可以先通过除以 `10 `把最后一位数字从` 1221` 中移除，`1221 / 10 = 122`，再求出上一步结果除以 `10` 的余数，`122 % 10 = 2`，就可以得到倒数第二位数字。如果我们把最后一位数字乘以 `10`，再加上倒数第二位数字，`1 * 10 + 2 = 12`，就得到了我们想要的反转后的数字。如果继续这个过程，我们将得到更多位数的反转数字。
>
> 现在的问题是，我们如何知道反转数字的位数已经达到原始数字位数的一半？
>
> 由于整个过程我们不断将原始数字除以 `10`，然后给反转后的数字乘上` 10`，所以，当原始数字小于或等于反转后的数字时，就意味着我们已经处理了一半位数的数字了。
>

```c++
class Solution {
public:
    bool isPalindrome(int x) {
       if(x<0||(x%10==0&&x!=0))  return false;
       int reverseNum=0;
       while(reverseNum<x){
           reverseNum=reverseNum*10+x%10;
           x/=10;
       }
       if(reverseNum==x||reverseNum/10==x)    return true;
       return false;
    }
};
```



#### 6. [696. 计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)[easy]

> 给定一个字符串 s，计算具有相同数量0和1的非空(连续)子字符串的数量，并且这些子字符串中的所有0和所有1都是组合在一起的。
>
> 重复出现的子串要计算它们出现的次数。
>
> 示例 1 :
>
> ```example
> 输入: "00110011"
> 输出: 6
> 解释: 有6个子串具有相同数量的连续1和0：“0011”，“01”，“1100”，“10”，“0011” 和 “01”。
> 
> 请注意，一些重复出现的子串要计算它们出现的次数。
> 
> 另外，“00110011”不是有效的子串，因为所有的0（和1）没有组合在一起。
> ```
>
> 
>
> 示例 2 :
>
> ```example
> 输入: "10101"
> 输出: 4
> 解释: 有4个子串：“10”，“01”，“10”，“01”，它们具有相同数量的连续1和0。
> 
> ```

方法一：

> 我们可以将字符串 s 转换为 `groups` 数组表示字符串中相同字符连续块的长度。例如，如果 `s=“11000111000000”`，则 `groups=[2，3，3，6]`。
>
> 对于 `'0' * k + '1' * k `或` '1' * k + '0' * k `形式的每个二进制字符串，此字符串的中间部分必须出现在两个组之间。
>
> 让我们尝试计算 `groups[i] `和` groups[i+1]` 之间的有效二进制字符串数。如果我们有` groups[i] = 2`, `groups[i+1] = 3，`那么它表示 `“00111”` 或 `“11000”`。显然，我们可以在此字符串中生成 `min(groups[i], groups[i+1]) `有效的二进制字符串。
>

```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        int len=s.size(),cnt=1;
        vector<int> groups;
        for(int i=1;i<len;++i){
            if(s[i]==s[i-1]){
                ++cnt;
            }else{
                groups.push_back(cnt);
                cnt=1;
            }
        }
        groups.push_back(cnt);
        int ans=0;
        for(int i=1;i<groups.size();++i){
            ans+=min(groups[i],groups[i-1]);
        }
        return ans;
    }
};
```

方法二：

> 我们可以修改我们的方法 1 来实时计算答案。我们将只记住 `prev = groups[-2]` 和` cur=groups[-1] `来代替 `groups`。然后，答案是我们看到的每个不同的 `(prev, cur) `的` min(prev, cur) `之和。

```c++
class Solution {
public:
    int countBinarySubstrings(string s) {
        int preLen=0,curLen=1,ans=0;
        for(int i=1;i<s.size();++i){
            if(s[i]!=s[i-1]){
                ans+=min(preLen,curLen);
                preLen=curLen;
                curLen=1;
            }else{
                curLen++;
            }
        }
        return ans+min(preLen,curLen);
    }
};
```

