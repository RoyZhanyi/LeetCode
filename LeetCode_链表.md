#### 1. [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)[easy]

> 编写一个程序，找到两个单链表相交的起始节点。
>
> 如下面的两个链表**：**
>
> 例如以下示例中 A 和 B 两个链表相交于 c1：
>
> ```
> A:          a1 → a2
>                     ↘
>                       c1 → c2 → c3
>                     ↗
> B:    b1 → b2 → b3
> ```
>
> 在节点 c1 开始相交。
>
> 注意：
>
> 如果两个链表没有交点，返回 null.
> 在返回结果后，两个链表仍须保持原有的结构。
> 可假定整个链表结构中没有循环。
> 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。

第一次解法：

```c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
       ListNode *l1=headA,*l2=headB;
       int len1=0,len2=0;
       while(l1){
            ++len1;
            l1=l1->next;
       }
       while(l2){
           ++len2;
           l2=l2->next;
       }
       l1=headA;
       l2=headB;
       if(len1<len2){
           int num=len2-len1;
           for(int i=0;i<num;++i){
               l2=l2->next;
           }
       }else{
           int num=len1-len2;
           for(int i=0;i<num;++i){
               l1=l1->next;
           }
       }
       while(l1&&l2&&l1!=l2){
           l1=l1->next;
           l2=l2->next;
       }
     return l1;

    }
};
```

改进：把相似的功能封装成一个函数。

```C++
class Solution {
public:
  	int getLen(ListNode *l){
      int count=0;
      while(l) {
        ++count;
        l=l->next;
      }
      return count;
    }
  	ListNode *getBegin(ListNode *l,int movelen){
      while(movelen--){
        l=l->next;
      }
      return l;
    }
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
      if(headA==NULL||headB==NULL){
        	return  NULL;
      }
      ListNode *p=headA,*q=headB;
      int len1=getLen(headA),len2=getLen(headB);
      if(len1<len2){
        q=getBegin(headB,len2-len1);
      }else if(len1>len2){
        p=getBegin(headA,len1-len2);
      }
      while(p&&q&&p!=q){
        p=p->next;
        q=q->next;
      }
    	 return p;
    }
};
```

#### 2. [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)<easy>

> 反转一个单链表。
>
> 示例:
>
> 输入: 1->2->3->4->5->NULL
> 输出: 5->4->3->2->1->NULL
> 进阶:
> 你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

解法一：头插法

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
      if(head==nullptr||head->next==nullptr){
        return head;
      } 
      ListNode *pre=nullptr,*p=head,*q=p->next;
      while(p!=nullptr){
        p->next=pre;
        pre=p;
        p=q;
        if(q!=nullptr)
            q=q->next;
      }
        return pre;
    }
};
```

解法二：递归

```c++
class Solution {
public:
   ListNode* reverseList(ListNode* head) {
    if (head == nullptr || head->next == nullptr) {
        return head;
    }
    ListNode* next = head->next;
    ListNode* newHead = reverseList(next);
    next->next = head;
    head->next = nullptr;
    return newHead;
}
};
```

#### 3. [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)[easy]

> 将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
>
>  
>
> **示例：**
>
> ```c++
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```

```C++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
       if(l1==nullptr) return l2;
       if(l2==nullptr) return l1;
       ListNode *head,*tail,*p=l1,*q=l2;//head为新节点的头部
       if(p->val<=q->val) {//处理头节点
           head=p;
           p=p->next;
       }
       else {
           head=q;
           q=q->next;
       }
       tail=head;
       while(p!=nullptr&&q!=nullptr){
          if(p->val<=q->val) {
             tail->next=p;
             tail=p;
             p=p->next;
            }
          else{
           tail->next=q;
           tail=q;
           q=q->next;
           } 
       }
       if(p!=nullptr) tail->next=p;
       else if(q!=nullptr) tail->next=q;
       else tail->next=nullptr;
       return head;
    }
};
```

#### 4. [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)[easy]

> 给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
>
> 示例 1:
>
> 输入: 1->1->2
> 输出: 1->2
> 示例 2:
>
> 输入: 1->1->2->3->3
> 输出: 1->2->3

```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if(head==nullptr||head->next==nullptr) return head;
        ListNode *p=head,*q=head->next;
        while(q!=nullptr){
            if(p->val==q->val){
                // ListNode *temp=q;
                // q=q->next;
                // delete temp;
                p->next=q->next;
                delete q;
                q=p->next;

            }else{
                p->next=q;
                p=q;
                q=q->next;
            }
        }
        p->next=q;
        return head;
    }
};
```

#### 5. [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)[medium]

> 给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
>
> 示例：
>
> 给定一个链表: 1->2->3->4->5, 和 n = 2.
>
> 当删除了倒数第二个节点后，链表变为 1->2->3->5.
> 说明：
>
> 给定的 n 保证是有效的。
>
> 进阶：
>
> 你能尝试使用一趟扫描实现吗？

**此题利用快慢指针即可解决，快指针先移动n个节点，之后两个指针同时往前移动，当快指针指向nullptr时，删除此时所指向节点的next节点即可**

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *slow,*fast;
        slow=fast=head;
        int i=0;
        while(fast!=nullptr&&i<n){
            fast=fast->next;
            ++i;
        }
        if(fast==nullptr){//证明倒数第n个节点是头节点
            fast=head;
            head=head->next;
        }else{
            fast=fast->next;
            while(fast!=nullptr){
                slow=slow->next;
                fast=fast->next;
            }
            fast=slow->next;
            slow->next=fast->next;
        }
        delete fast;//释放内存，防止内存泄露...虽然题目没有要求
        return head;
    }
};
```



#### 6. [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)[medium]

> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
>
> 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
>
>  示例:
>
> 给定 1->2->3->4, 你应该返回 2->1->4->3.。
>

解法一：递归

**该解法可采用206题反转链表中的思想，利用递归求出两两交换的链表**

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if(head==nullptr||head->next==nullptr)
            return head;
        ListNode *p=head,*newHead=head->next;
        p->next=swapPairs(newHead->next);
        newHead->next=p;
        return newHead;
    }
};
```

解法二：头插

```c++
class Solution{
public:
	 ListNode* swapPairs(ListNode* head){
     if(head==nullptr||head->next==nullptr){
       	return head;
     }
     ListNode *p,*q,*h,*t;
     p=head;
     q=head->next;
     h=q;
     while(p!=nullptr&&q!=nullptr){
			 p->next=q->next;
       q->next=p;
       t=p;
       if(p->next==nullptr){
         break;
       }else{
         p=p->next;
       }
       if(p->next==nullptr){
         break;
       }else{
         q=p->next;
         t->next=q;
       }
     }
     return h;
   }
};
```

#### 7. [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)[medium]

> 给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
>
> 你可以假设除了数字 0 之外，这两个数字都不会以零开头。
>
>  进阶：
>
> 如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。
>
> 示例：
>
>  输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
>输出：7 -> 8 -> 0 -> 7

解法一：将链表逆转再求和

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* l){
        if(l==nullptr||l->next==nullptr) return l;
        ListNode* next=l->next;
        ListNode* newHead=reverseList(next);
        next->next=l;
        l->next=nullptr;
        return newHead;
    }
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        l1=reverseList(l1);
        l2=reverseList(l2);
        int a=0;
        ListNode *p=l1,*q=l2;
        while(p!=nullptr&&q!=nullptr){
            int temp=p->val+q->val+a;
            p->val=q->val=temp%10;
            a=temp/10;
            p=p->next;
            q=q->next;
        }
        if(p!=nullptr){
            while(a!=0&&p!=nullptr){
               int temp=p->val+a;
               p->val=temp%10;
               a=temp/10;
               p=p->next;
            }
            l1=reverseList(l1);
            if(a!=0){
               p=new ListNode(a);
               p->next=l1;
               l1=p;
            }
            return l1;
        }else if(q!=nullptr){
            while(a!=0&&q!=nullptr){
               int temp=q->val+a;
               q->val=temp%10;
               a=temp/10;
               q=q->next;
            }
            l2=reverseList(l2);
            if(a!=0){
               q=new ListNode(a);
               q->next=l2;
               l2=q;
            }
            return l2;
        }else{
           l2=reverseList(l2);
           if(a!=0){
               q=new ListNode(a);
               q->next=l2;
               l2=q;
            }
            return l2;
        } 
    
    }
};
```

解法二：利用vector数组来求和

```c++
class Solution {
public:
   
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
      vector<int>  num1,num2;
      ListNode *p=l1,*q=l2;
      while(p){
          num1.push_back(p->val);
          p=p->next;
      }
      while(q){
          num2.push_back(q->val);
          q=q->next;
      }
      int i=num1.size()-1,j=num2.size()-1;
      int a=0;
      while(i>=0&&j>=0){
          int temp=num1[i]+num2[j]+a;
          num1[i]=num2[j]=temp%10;
          a=temp/10;
          --i;
          --j;
      }
      if(i>=0){
          while(a!=0&&i>=0){
              int temp=num1[i]+a;
              num1[i]=temp%10;
              a=temp/10;
              --i;
          }
          p=l1;
          i=0;
          while(p){
            p->val=num1[i++];
            p=p->next;
          }
          if(a!=0){
              ListNode* t=new ListNode(a);
              t->next=l1;
              l1=t;
          }
          return l1;
      }else{
         while(a!=0&&j>=0){
              int temp=num2[j]+a;
              num2[j]=temp%10;
              a=temp/10;
              --j;
          }
          q=l2;
          j=0;
          while(q){
            q->val=num2[j++];
            q=q->next;
          }
          if(a!=0){
              ListNode* t=new ListNode(a);
              t->next=l2;
              l2=t;
          }
          return l2;
      }
    
    }
};
```

柳神解法：

```c++
class Solution {
public:
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) { 
  stack<int> s1, s2, s;
	while(l1 != NULL) {
		s1.push(l1->val);
		l1 = l1->next; 
  }
	while(l2 != NULL) { 
   	s2.push(l2->val); l2 = l2->next;
	}
	int carry = 0;
	while(!s1.empty() || !s2.empty()) {
	int tempsum = carry; 
  if(!s1.empty()) {
		tempsum += s1.top();
		s1.pop();
  }
	if(!s2.empty()) { 
    tempsum += s2.top(); s2.pop();
	}
	carry = 0; 
  if(tempsum >= 10) {
	carry = 1;
	tempsum = tempsum - 10;
	}
	s.push(tempsum); }
	if(carry == 1) 
    	s.push(1);
	ListNode* result = new ListNode(0); ListNode* cur = result; while(!s.empty()) {
	int top = s.top();
	s.pop();
	ListNode* node = new ListNode(top); cur->next = node;
	cur = cur->next;
	}
	return result->next; 
}
};
```

#### 8. [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)<easy>

> 请判断一个链表是否为回文链表。
>
> 示例 1:
>
> 输入: 1->2
> 输出: false
> 示例 2:
>
> 输入: 1->2->2->1
> 输出: true
> 进阶：
> 你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

解法一：利用vector，空间/时间复杂度都为o(n)

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        vector<int> v;
        ListNode* p=head;
        while(p){
            v.push_back(p->val);
            p=p->next;
        }
        int i=v.size()/2-1,j;
        if(v.size()%2==0){
            j=v.size()/2;
        }else{
            j=v.size()/2+1;
        }
        while(i>=0){
            if(v[i]!=v[j])
                return false;
            --i;
            ++j;
        }

        return true;
    }
};
```

解法二：利用快慢指针，slow、quick一次分别移动一个和两个next，可将链表分为前后两个部分，前半部分链表反转后与后半部分链表进行比对，即可使用O(1)空间复杂度O(n)时间复杂度解决此题；

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head){
        if(head==nullptr||head->next==nullptr){
            return head;
        }
        ListNode* next=head->next;
        ListNode* newHead=reverseList(next);
        next->next=head;
        head->next=nullptr;
        return newHead;
    }
    bool isPalindrome(ListNode* head) {
        if(head==nullptr||head->next==nullptr){
            return true;
        }
        ListNode *slow,*quick,*pre;
        pre=slow=head;
        quick=head->next;
        while(quick!=nullptr&&quick->next!=nullptr){
            pre=slow;
            slow=slow->next;
            quick=quick->next->next;
        }
        if(quick==nullptr){//证明链表节点为奇数个
           quick=slow->next;
            pre->next=nullptr;
        }else{//节点个数为偶数个
            quick=slow->next;
            slow->next=nullptr;
        }
        slow=reverseList(head);
        while(slow){
            if(slow->val!=quick->val)  return false;
            slow=slow->next;
            quick=quick->next;
        }
        return true;
    }
};
```

#### 9. [725. 分隔链表](https://leetcode-cn.com/problems/split-linked-list-in-parts/)<medium>

> 给定一个头结点为 root 的链表, 编写一个函数以将链表分隔为 k 个连续的部分。
>
> 每部分的长度应该尽可能的相等: 任意两部分的长度差距不能超过 1，也就是说可能有些部分为 null。
>
> 这k个部分应该按照在链表中出现的顺序进行输出，并且排在前面的部分的长度应该大于或等于后面的长度。
>
> 返回一个符合上述规则的链表的列表。
>
> 举例： 1->2->3->4, k = 5 // 5 结果 [ [1], [2], [3], [4], null ]
>
> 示例 1：
>
> 输入: 
> root = [1, 2, 3], k = 5
> 输出: [[1],[2],[3],[],[]]
> 解释:
> 输入输出各部分都应该是链表，而不是数组。
> 例如, 输入的结点 root 的 val= 1, root.next.val = 2, \root.next.next.val = 3, 且 root.next.next.next = null。
> 第一个输出 output[0] 是 output[0].val = 1, output[0].next = null。
> 最后一个元素 output[4] 为 null, 它代表了最后一个部分为空链表。
> 示例 2：
>
> 输入: 
> root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
> 输出: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
> 解释:
> 输入被分成了几个连续的部分，并且每部分的长度相差不超过1.前面部分的长度大于等于后面部分的长度。
>
>
> 提示:
>
> root 的长度范围： [0, 1000].
> 输入的每个节点的大小范围：[0, 999].
> k 的取值范围： [1, 50].

```c++
class Solution {
public:
    vector<ListNode*> splitListToParts(ListNode* root, int k) {
        vector<ListNode*> ans;
        if(root==nullptr){
            for(int i=0;i<k;++i){
                ans.push_back(nullptr);
            }
            return ans;
        }
        ListNode *p=root,*pre=root;
        int count=0;
        while(p!=nullptr){
            ++count;
            p=p->next;
        }
        if(k>=count){
            while(pre!=nullptr){
                p=pre->next;
                pre->next=nullptr;
                ans.push_back(pre);
                pre=p;
                if(p!=nullptr)
                  p=p->next;
            }
            for(int i=0;i<k-count;++i){
                ans.push_back(nullptr); 
            }
        }else{
            int n=count%k;
            int num=count/k;//每份分成n个
            while(root!=nullptr&&k>0){
                pre=root;
                for(int i=1;i<num&&pre!=nullptr;++i){
                    pre=pre->next;
                }
                if(n>0&&pre!=nullptr){
                    pre=pre->next;
                    --n;
                }
                p=pre->next;
                pre->next=nullptr;
                ans.push_back(root);
                root=p;
                --k;
            }
        }
        return ans;
    }
};
```

#### 10. [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)<medium>

> 给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。
>
> 请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。
>
> 示例 1:
>
> 输入: 1->2->3->4->5->NULL
> 输出: 1->3->5->2->4->NULL
> 示例 2:
>
> 输入: 2->1->3->5->6->4->7->NULL 
> 输出: 2->3->6->7->1->5->4->NULL
> 说明:
>
> 应当保持奇数节点和偶数节点的相对顺序。
> 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

```c++
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head==nullptr||head->next==nullptr){
            return head;
        }
        ListNode *odd=head,*even=head->next;
        ListNode *oddTail=odd,*evenTail=even;
        ListNode *p=even->next,*q;
        int flag=1;
        while(p){
            if(flag==1){
                oddTail->next=p;
                oddTail=oddTail->next;
            }else{
                evenTail->next=p;
                evenTail=evenTail->next;
            }
            flag*=-1;
            p=p->next;
        }
        evenTail->next=nullptr;
        oddTail->next=even;
        return odd;
    }
};
```

