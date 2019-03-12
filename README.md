## [**21. Merge Two Sorted Lists**](https://leetcode.com/problems/merge-two-sorted-lists/)

> Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
>
> **Example:**
>
> ```
> Input: 1->2->4, 1->3->4
> Output: 1->1->2->3->4->4
> ```
>
>
>
> Accepted
>
> 517,422
>
> Submissions
>
> 1,126,736

**思路**：

- 1.重新开辟一个指针用来保存数据，每一次进行判断l1,l2链表中值的大小用来选择小的插入。

- 2.递归的思想，代码更简洁、

c++   

```c++
  ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* l = new ListNode(0);
        ListNode* h =l;//用来保存头结点
        while(l1!=NULL&&l2!=NULL){
        if(l1->val<l2->val){
            ListNode*p = new ListNode(0);
            p->val = l1->val;
            l->next = p;
            l1 = l1->next;
            l = l->next;
        }else{
           ListNode*p = new ListNode(0);
            p->val = l2->val;
             l->next = p;
            l2 = l2->next;
            l = l->next;
        }
        }
        while(l1!=NULL){//当l1还有其他结点
            ListNode*p = new ListNode(0);
            p->val = l1->val;
            l->next = p;
            l1 = l1->next;
            l = l->next;
        }
        while(l2!=NULL){//当l2还有其他结点
            ListNode*p = new ListNode(0);
            p->val = l2->val;
            l->next = p;
            l2 = l2->next;
            l = l->next;
        }
        return h->next;
        
    }
```

C++  递归也是开辟了新的内存来保存结点，代码更简洁。不过时间和空间稍差一些、

```c++
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1)return l2;
        if(!l2)return l1;
        ListNode *tem = NULL;
        if(l1->val<l2->val){
            tem = l1;
            tem->next = mergeTwoLists(l1->next,l2);
        }
        else{
            tem = l2;
            tem->next = mergeTwoLists(l1,l2->next);
        }
        return tem;
    }
```
Python 1

```Python
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        head = ListNode(None)
        cur = head
        while l1 and l2:
            if l1.val<l2.val:
                tem = ListNode(None)
                tem.val = l1.val
                cur.next = tem
                cur = cur.next
                l1 = l1.next
            else:
                tem = ListNode(None)
                tem.val = l2.val
                cur.next = tem
                cur = cur.next
                l2 = l2.next
        while l1:
                tem = ListNode(None)
                tem.val = l1.val
                cur.next = tem
                cur = cur.next
                l1 = l1.next
        while l2:
                tem = ListNode(None)
                tem.val = l2.val
                cur.next = tem
                cur = cur.next
                l2 = l2.next
        return head.next
```

Python 2

```python
         def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if l1 == None:
            return l2
        if l2 == None:
            return l1
        tem = ListNode(None)
        if l1.val < l2.val:
            tem = l1
            tem.next = self.mergeTwoLists(l1.next,l2)
        else:
            tem = l2
            tem.next = self.mergeTwoLists(l1,l2.next)
        return tem
```

## [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

> Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
>
> For example, given *n* = 3, a solution set is:
>
> ```
> [
>   "((()))",
>   "(()())",
>   "(())()",
>   "()(())",
>   "()()()"
> ]
> ```

找到满足条件的()搭配方法

**思路**： 采用dfs的方法，根据左右符号的数量进行dfs，优先加入( 。

c++

```c++

```

## [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

> Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appear only *once* and return the new length.
>
> Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.
>
> **Example 1:**
>
> ```
> Given nums = [1,1,2],
> 
> Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
> 
> It doesn't matter what you leave beyond the returned length.
> ```

判断依据排序好的数组中是否存在重复的值，返回不重复的个数，并且将数组转换成不重复的数组。

c++

```c++
    int removeDuplicates(vector<int>& nums) {
        if(nums.size()==0)return 0;
        int count=0;
        for(int i=1;i<nums.size();i++)
            if(nums[i]!=nums[count])
                nums[++count]=nums[i];
        return count+1;
    }
```

Python

```python
    def removeDuplicates(self, nums):
        c = 0
        if len(nums) == 0:
            return 0
        for i in range(1,len(nums)):
            if nums[c]!=nums[i]:
                c = c+1
                nums[c] = nums[i]
        return c+1
```

## [27. Remove Element](https://leetcode.com/problems/remove-element/)

> Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.
>
> Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.
>
> The order of elements can be changed. It doesn't matter what you leave beyond the new length.
>
> **Example 1:**
>
> ```
> Given nums = [3,2,2,3], val = 3,
> 
> Your function should return length = 2, with the first two elements of nums being 2.
> 
> It doesn't matter what you leave beyond the returned length.
> ```

​	思路：和26差不多，都是先定义一个index来保存不满足条件的序号，然后当不满足时，将原来数组里的序号值替换。

C++

```c++
    int removeElement(vector<int>& nums, int val) {
        if(nums.size()==0)return 0;
        int c = 0;
        for(int i=0;i<nums.size();i++)
            if(nums[i]!=val)
                nums[c++] = nums[i];
        return c;
    }
```

Python

```python
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        if len(nums)==0:
            return 0
        c = 0
        for i in range(0,len(nums)):
            if nums[i]!=val:
                nums[c] = nums[i]
                c = c+1
        return c
```



## [28. Implement strStr](https://leetcode.com/problems/implement-strstr/)

> Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).
>
> Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.
>
> **Example 1:**
>
> ```
> Input: haystack = "hello", needle = "ll"
> Output: 2
> ```
>
> **Example 2:**
>
> ```
> Input: haystack = "aaaaa", needle = "bba"
> Output: -1
> ```

- 字符串匹配算法

- 1.暴力,直接遍历两个串判断

- 2.经典的KMP算法 包括两部分

  - 1.求next数组
  - 2.kmp匹配

  C++ 1.

  ```c++
      int force(string t,string s){
          for(int i=0;i<=t.size()-s.size();i++){
              int j=0;
              for(;j<s.size();j++){
                  if(t[i+j]!=s[j]){
                      break;
                  }
              }
              if(j==s.size()){
                  return i;
              }
          }
          return -1;
      }
  ```

  C++ 2. KMP算法

  ```c++
  //求next数组
  vector<int> getNext(string s){
          vector<int>next(s.size(),0);
          next[0]=-1;
          int k=-1;
          int j=0;
          while(j<s.size()-1){
              if(k==-1||s[k]==s[j]){
                  j++;
                  k++;
                  next[j]=k;
              }else{
                  k = next[k];
              }
          }
          return next;
      }
  
  int kmp(string t,string s){
          int i=0;
          int j=0;
          vector<int>next;
          next = getNext(s);
          while(i<t.size()&&abs(j)<s.size()){
              if(j==-1||t[i]==s[j]){
                  i++;
                  j++;
              }else{
                  j = next[j];
              }
          }
          return j==s.size()?i-j:-1;
      }
  ```


## [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/)

> Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
>
> You may assume no duplicates in the array.
>
> **Example 1:**
>
> ```
> Input: [1,3,5,6], 5
> Output: 2
> ```
>
> **Example 2:**
>
> ```
> Input: [1,3,5,6], 2
> Output: 1
> ```

- 判断target是否出现在当前数组，如果有则返回index，如果没有则返回他应插入的index
- **思路**：很容易可以想到二分查找的方法。

c++ 

```c++
    int searchInsert(vector<int>& nums, int target) {
        if(nums.size()==0)return 0;
        return bs(0,nums.size(),target,nums);
    }
    int bs(int l,int r,int target,vector<int>&nums){
            if(l<r){
            int m = l+(r-l)/2;
            if(nums[m]==target)return m;
            else if(nums[m]<target)return bs(m+1,r,target,nums);
            else return bs(l,m,target,nums);
            }
        return r;
    }
```

PYTHON

```python
    def searchInsert(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        i = 0 
        j = len(nums)
        while i< j :
            m = (i+j)/2
            if nums[m] == target:
                return m
            elif nums[m] < target:
                i =m+1
            else:
                j = m
        return i
```

## [38. Count and Say](https://leetcode.com/problems/count-and-say/)

> The count-and-say sequence is the sequence of integers with the first five terms as following:
>
> ```
> 1.     1
> 2.     11
> 3.     21
> 4.     1211
> 5.     111221
> ```
>
> `1` is read off as `"one 1"` or `11`.
> `11` is read off as `"two 1s"` or `21`.
> `21` is read off as `"one 2`, then `one 1"` or `1211`.
>
> Given an integer *n* where 1 ≤ *n* ≤ 30, generate the *n*th term of the count-and-say sequence.
>
> Note: Each term of the sequence of integers will be represented as a string.

c++

```c++
    string countAndSay(int n) {
        if(n==1)return "1";
        if(n==2)return "11";
        string sh="11";
        string tem="";
        int c=0;
        int num=0;
        for(int i=2;i<n;i++){
            num = sh[0]-'0';
            c = 1;
            tem="";
            for(int j=1;j<sh.size();j++){
                if(sh[j]-'0'==num){
                    c++;
                }else{
                   tem.insert(tem.end(),1,c+'0');
                    tem.insert(tem.end(),1,num+'0');
                    c=1;
                    num=sh[j]-'0';
                }
                if(j==sh.size()-1){
                    tem.insert(tem.end(),1,c+'0');
                    tem.insert(tem.end(),1,num+'0');
                    }

            }
            sh = tem;
        }
        return sh;
        
    }
```

## [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

> iven a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.
>
> The **same** repeated number may be chosen from `candidates` unlimited number of times.
>
> **Note:**
>
> - All numbers (including `target`) will be positive integers.
> - The solution set must not contain duplicate combinations.
>
> **Example 1:**
>
> ```
> Input: candidates = [2,3,6,7], target = 7,
> A solution set is:
> [
>   [7],
>   [2,2,3]
> ]
> ```

​	寻找数组中元素的组合，使其sum等于target。一道典型的搜索类型的题目。回溯法

C++

```C++
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>>res;
        vector<int>path;
        dfs(0,target,path,candidates,res);
        return res;
    }
void dfs(int cur,int target,vector<int>&path,vector<int>&candidates,vector<vector<int>>&res){
        if(target==0){
            res.push_back(path);
            return ;
        }
        for(int i=cur;i<candidates.size();i++){
            if(target - candidates[i]>=0){
                path.push_back(candidates[i]);
                dfs(i,target-candidates[i],path,candidates,res);
                path.pop_back();
            }
        }
    }
```

python

```python
    def combinationSum(self, candidates, target):
        res = []
        candidates.sort()
        self.dfs(target,0,[],res,candidates)
        return res
    
    def dfs(self,target,index,path,res,cand):
        if target == 0:
            res.append(path)
            return
        for i in range(index,len(cand)):
            if target - cand[i]>=0:
                self.dfs(target-cand[i],i,path+[cand[i]],res,cand)
```

------------------------------------------------------------------------##

[413. Arithmetic Slices](https://leetcode.com/problems/arithmetic-slices/)

> 
>
>
找出数组中符合等差数列的子数组的个数（长度大于3）

C++

```C++
       int numberOfArithmeticSlices(vector<int>& A) {
        int s = A.size();
        if(s<=2)return 0;
        int start = 0,end = start+2;
        int count = 0;
        int tem1,tem2;
        while(start<s-2){
            tem1 = A[end] - A[end-1];
            tem2 = A[start+1] - A[start];
            if(tem1==tem2){
                count++;
                if(end+1>=s){
                    start++;
                    end = start+2;
                }else{
                    end++;
                }
            }else{
                start++;
                end = start+2;
            }
        }
        return count;
        
    }
   
```

python

```python
       def numberOfArithmeticSlices(self, A):
        """
        :type A: List[int]
        :rtype: int
        """
        start = 0
        end = start+2
        count = 0
        l = len(A)
        while start < l-2:
            tem1 = A[start+1] - A[start]
            tem2 = A[end] - A[end-1]
            if tem1 == tem2:
                count = count+1
                if end >l-2:
                    start =start+1
                    end = start +2
                else:
                    end =end+1
            else:
                start = start+1
                end = start+2
        return count
   
```