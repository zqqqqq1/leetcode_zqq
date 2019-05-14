[**1.Two Sum**](https://leetcode.com/problems/two-sum/)

> Given an array of integers, return indices of the two numbers such
> that they add up to a specific target.
> You may assume that each input would have exactly one solution, and
> you may not use the same element twice.
> Example:
> Given nums = [2, 7, 11, 15], target = 9,
> Because nums[0] + nums[1] = 2 + 7 = 9, return [0, 1].

给出一个数组和一个target 找出数组中是否存在两个数相加为target，相同的数也可以满足。

**思路**：
采用hashtable的思想，因为数组中必然存在满足条件的两个数。那么遍历一次数组，每次将数组中的数放入map中时判断target - nums[i]在map中是否存在。
如果存在：就得到了满足条件的值。
如果不存在：那么将map[nums[i]] = i;用于保存数字原来的index位置。

c++

```c++
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int>m;
        for(int i=0;i<nums.size();i++){
            auto it = m.find(target - nums[i]);
            if(it!=m.end()){
                return {it->second,i};
            }
            m[nums[i]] = i ;
        }
        return {};
    }
```

Python

```python
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        m = {}
        for i,num in enumerate(nums):
            if target - num in m:
                return [m[target - num] , i]
            else:
                m[num] = i
        return []
```

[**7. Reverse Integer**](https://leetcode.com/problems/reverse-integer/)

> Given a 32-bit signed integer, reverse digits of an integer.
>
> **Example 1:**
>
> ```
> Input: 123
> Output: 321
> ```
>
> **Example 2:**
>
> ```
> Input: -123
> Output: -321
> ```
>
> **Example 3:**
>
> ```
> Input: 120
> Output: 21
> ```

给出一个int类型数字，你需要将其反转，不能有0开头。

**思路**：非常简单的思想，但是需要小心overflows

颠倒数字    

```c++
res = res*10 + x%10 
x /=10
```



c++

```c++
    int reverse(int x) {
        if(x==0)return 0;
        long long res = 0;
        while(x!=0){
            res = res*10+x%10;
            if(res>INT32_MAX||res<INT32_MIN)
                return 0;
            x/=10;
        }
        return int(res);
    }
```

Python

```python
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        sign = 1
        if x<0:
            sign = -1
        x = abs(x)
        num = 0
        while x!=0:
            num = num*10 + x%10
            x = x//10
            if num <(-2**31) or num>(2**31-1):
                return 0
        return num * sign
```

[**9. Palindrome Number**](https://leetcode.com/problems/palindrome-number/)

> Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.
>
> **Example 1:**
>
> ```
> Input: 121
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input: -121
> Output: false
> Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
> ```
>
> **Example 3:**
>
> ```
> Input: 10
> Output: false
> Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
> ```

给出一个int类型数字，你需要判断是否为回文数，即左读，右读都一样

**思路**：不转换成string类型 ，直接进行判断。

- 1.负数，可以被0整除的数肯定不可能满足要求
- 2.直接根据加和思想，倒序加和，判断是否相等。

颠倒数字    

```c++
res = res*10 + x%10 
x /=10
```



c++   **1.缺点是采用了额外的空间  long long **如果直接采用int，则会有overflows的风险

```c++
    bool isPalindrome(int x) {
        if(x<0)return false;
        long long sum = 0,tem = x;
        while(tem>0){
            sum = sum*10+tem%10;
            tem/=10;
        }
        //cout<<sum<<" "<<x<<endl;
        return sum==x;
    }
```

C++ 2. **优点是只需要遍历一半的数就可以得到结果了**

```c++
    bool isPalindrome(int x) {
        if(x<0||(x!=0&&x%10==0))return false;
        int sum = 0;
        while(x>sum){
            sum = sum*10+x%10;
            x/=10;
        }
        //cout<<sum<<" "<<x<<endl;
        //为什么要判断x==sum/10呢？
        //因为有可能数的位数是单数，这时中间的一位就不需要判断了
        return sum==x ||(x==sum/10);
    }
```

Python

```python
       def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        if x<0 or (x!=0 and x%10==0):
            return False
        sum = 0
        while x>sum:
            sum = sum*10+x%10
            x = x//10
        return sum==x or x==sum//10
```



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

## [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)

>
>
>  题目	寻找第三大的元素，如果没有则返回最大的元素

C++

```C++
    int thirdMax(vector<int>& nums) {
        int size = nums.size();
        if(size==1)return nums[0];
        if(size==2)return max(nums[0],nums[1]);
        long long  m1 = LLONG_MIN;
        long long  m2 = m1, m3 = m2;
        for(int i=0;i<nums.size();i++){
            if(nums[i]<=m3||nums[i]==m1||nums[i]==m2){
                continue;
            }
            m3 = nums[i];
            if(m3>m2)swap(m3,m2);
            if(m2>m1)swap(m1,m2);
        }
        return m3==LLONG_MIN?m1:m3;
        
    }
   
```





## [415. Add Strings](https://leetcode.com/problems/add-strings/)

>
>
>  题目	两个只包括0-9的字符串，求两个字符串数字的和，并且返回一个字符串。
>
> **思路**：按照从后到前，即从小到大的顺序，进行字符加和，保存到一个sum下，每次在结果字符串首插入sum%10，并且sum/10

C++

```C++
       string addStrings(string num1, string num2) {
        int i =num1.size()-1;
        int j =num2.size()-1;
        int sum = 0;
        string res;
        while(i>=0||j>=0||sum>0){
            if(i>=0)sum+=num1[i--]-'0';
            if(j>=0)sum+=num2[j--]-'0';
            res.insert(0,1,(sum%10)+'0');
            sum/=10;
        }
        return res;
    }
   
```



## [429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)

>
>
>  题目	N叉树的层序遍历。
>
> **思路**：和二叉树的层序遍历不同的是，N叉树不只有Left和Right节点，而是一个children集合的子树。那么我们在层序遍历的时候，除了通过队列的size判断当前层之外，还需要进行的是children的遍历添加。

C++

```C++
       vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>>res;
        vector<int>v;
        vector<Node*>c;
        Node *tem;
        if(!root)return res;
        queue<Node*>q;
        q.push(root);
        int s = 0;
        while(!q.empty()){
            v.clear();
            s = q.size();
            while(s){
                tem = q.front();
                v.push_back(tem->val);
                q.pop();
                for(int j=0;j<tem->children.size();j++){
                    q.push(tem->children[j]);
                }
            s--;
            }
            res.push_back(v);
        }
        return res;
    }
   
```

python

```python
   
   
```





## [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

>
>
>  题目	在二叉树中找到路径和为sum 的路径（不要求从根节点到叶子节点）

C++

```C++
       int pathSum(TreeNode* root, int sum) {
        return root?pathSum(root->left,sum)+pathSum(root->right,sum)+get(root,sum):0;
    }
    int get(TreeNode* root,int sum){
        return root?(sum==root->val)+get(root->left,sum-root->val)+
                     get(root->right,sum-root->val):0;
    }
   
```

python

```python
   
   
```





## [448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

>
>
>  题目	寻找数组中[1,n]未出现的元素
>
> 在数组[1,n]中寻找重复或者未出现元素时。根据数组的特性：数据元素从[1,n]
>
> 那么根据数据元素nums[abs(nums[i])-1] 来标记元素出现与否。
>
> 例如 [4,3,2,7,8,2,3,1] 第一个元素4出现，那么我们就置nums[3]= -1*nums[3] 这样就标记了4出现。如果之后4再次出现，就会再次乘以-1。再计算重复值时，其他出现1次的数字对应的index都为负数,4对应的index 3 则会为正。如果计算缺省值时，我们只需要在nums[index]>0时置一次-1，那么缺省值就会为正数。遍历时就可以很容易找到了
>
>

C++

```C++
       vector<int> findDisappearedNumbers(vector<int>& nums) {
        vector<int>res;
        int n =nums.size();
        for(int i=0;i<n;i++){
            if(nums[abs(nums[i]) - 1]>0){
                nums[abs(nums[i]) - 1] *=-1;
            }
        }
        for(int i=0;i<n;i++){
            if(nums[i]>0)
                res.push_back(i+1);
        }
        return res;
    }
   
```

python

```python
   
   
```





## [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)

>
>
>  题目	将字符串中字符出现的频率排序
>
> **思路**：先使用map统计频率，将频率大小插入到vector数组中，再根据频率进行插入。

C++

```C++
       string frequencySort(string s) {
        unordered_map<char,int>m;
        vector<string>res(s.size()+1,"");
        for(char c:s)m[c]++;
        for(auto& it:m){
            int n = it.second;
            int c = it.first;
            res[n].append(n,c);
        }
        string rs;
        for(int i=res.size()-1;i>=0;i--){
            if(!res[i].empty())
            rs.append(res[i]);
        }
        return rs;
    }
   
```

python

```python
   
   
```





## [538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

>
>
>  题目	将BST中的每个元素都加上比它大的元素的值。
>
>**思路**：由于是BST，那么右边子树肯定大于父节点和左子树。既然使用要加上所有大的元素的值，那么我们DFS到右子树，然后维护一个sum值，一直加上当前节点。然后在每一个节点都加上sum。

C++ 1.DFS 

```C++
   TreeNode* convertBST(TreeNode* root) {
        TreeNode *q=root;
        int sum=0;
        p(root,sum);
        return q;
    }
    void p(TreeNode* root,int& sum){
        if(!root)return ;
        p(root->right,sum);
        root->val+=sum;
        sum=root->val;
        p(root->left,sum);
        
        
    }
   
```

python

```python
   
   
```



## [581. Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)

>
>
>  题目	寻找数组元素中，排序一段子数组之后能让数组完全有序的数组。

C++   sort当前数组，然后维护两个index，遍历数组之后找到数组元素不相同的最小index

```C++
       int findUnsortedSubarray(vector<int>& nums) {
        vector<int>tem = nums;
        sort(tem.begin(),tem.end());
        if(tem==nums)return 0;
        int b=-1;
        int e=0;
        for(int i=0;i<nums.size();i++){
            if(nums[i]!=tem[i]){
                if(b==-1){
                    b= i;
                }
                e = i;
            }
        }
        return e-b+1;
        
    }
   
```

python

```python
   
   
```

## [594. Longest Harmonious Subsequence](https://leetcode.com/problems/longest-harmonious-subsequence/)

>
>
>
> 题目	寻找数组中最长的和谐子数组。
> 和谐子数组的定义：这一个子数组中最大值和最小值的差值小于等于1
>
**思路** 由于本题不需要考虑原来数组的顺序问题，只需要考虑各个元素之间的差值即可。
那么使用一个map来保存数字出现的频率。维护一个max来保存差值数组在1的个数。

C++

```C++
       int findLHS(vector<int>& nums) {
        int res = INT_MIN;
        unordered_map<int,int>m;
        unordered_map<int,int>::iterator it;
        for(auto x:nums)m[x]+=1;
        for(it=m.begin();it!=m.end();it++){
            if(m.find(it->first + 1)!=m.end()){
                int sum = m[it->first]+m[it->first+1];
                res = max(sum,res);
            }
        }
        return max(res,0);        
    }
   
```

python

```python
   
   
```

## [598. Range Addition II](https://leetcode.com/problems/range-addition-ii//)

>
>
>
> 题目	
>

C++

```C++
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        if(ops.size()==0)return m*n;
        int min_i = INT_MAX;
        int min_j = INT_MAX;
        for(int i=0;i<ops.size();i++){
            if(ops[i][0]<min_i)
                min_i = ops[i][0];
            if(ops[i][1]<min_j)
                min_j = ops[i][1];
        }
        return min_i*min_j;
    }
   
```

python

```python
   
   
```

## [599. Minimum Index Sum of Two Lists](https://leetcode.com/problems/minimum-index-sum-of-two-lists/)

>
>
>
> 题目	
>

C++

```C++
       vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_map<string,int>m;
        vector<string>res;
        for(int i=0;i<list1.size();i++)
            m[list1[i]]=i;
        int _m = INT_MAX;
        for(int i=0;i<list2.size();i++){
            if(m.find(list2[i])!=m.end()){
                int sum = m[list2[i]] + i;
                if(sum==_m){
                    res.push_back(list2[i]);
                }else if(sum<_m){
                    _m = sum;
                    res = {list2[i]};
                }
            }
        }
        return res;
    }
   
```

python

```python
   
   
```


## [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)

>
>
>
> 题目	
>

C++

```C++
           bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int cur = 1;
        int count = 0;
        if(n==0)return true;
        flowerbed.insert(flowerbed.begin(),0);
        flowerbed.push_back(0);
        int s = flowerbed.size();
        for(;cur<s-1;cur++){
           if(flowerbed[cur-1]==0&&flowerbed[cur]==0&&flowerbed[cur+1]==0){
                    flowerbed[cur]=1;
                    count++;
                    if(count==n)return true;
                    cur++;
                }
        }
        return false;
    }
```

python

```python
   
   
```


## [653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

>
>
>
> 题目	在BST中找到两个节点和为k
>

思路：递归的方法:维护两个节点l,r
若当前节点和值大于k，则l->left,r || l,r->left
若小于K,则l->right, r->right
相等 return true

非递归，建立一个vector保存 ，然后使用两个point 查找

C++

```C++
             bool flag = false;
    bool findTarget(TreeNode* root, int k) {
        find(root,root,k);
        return flag;
    }
    bool find(TreeNode* l,TreeNode* r,int k){
        if(flag)return flag;
        if(!l)return false;
        if(!r)return false;
        int sum = l->val+r->val;
        if(sum ==k&&l!=r){
            flag = true;
            return true;
        }
            if(sum<k)return find(l->right,r,k)||find(l,r->right,k);
        else{
            return find(l->left,r,k)||find(l,r->left,k);
        }
    }
```

C++

```c++
    bool flag = false;
    vector<int>res;
    bool findTarget(TreeNode* root, int k) {
        if(!root)return false;
        dfs(root);
        int i=0,j=res.size()-1;
        while(i<j){
            int sum = res[i]+res[j];
            if(sum==k)return true;
            else if(sum<k){
                i++;
            }else
                j--;
        }
        return false;
    }
    void dfs(TreeNode* root){
        if(!root)return;
        dfs(root->left);
        res.push_back(root->val);
        dfs(root->right);
    }
   
```

## [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)

>
>
>
> 题目	修剪BST，使得所有的节点都在[L,R]内
>

思路：根据BST的特性，

C++

```C++
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if(!root)return NULL;
        if(L>root->val)return trimBST(root->right,L,R);
        if(R<root->val)return trimBST(root->left,L,R);
        root->left = trimBST(root->left,L,R);
        root->right =trimBST(root->right,L,R);
        return root;
    }

```


## [671. Second Minimum Node In a Binary Tree](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree//)

>
>
>
> 题目	寻找二叉树中第二小的数
>

思路：维护两个int 来保存最小的和第二小

C++

```C++
    int min2,min1;
    int findSecondMinimumValue(TreeNode* root) {
        min1 = root->val;
        min2 = INT_MAX;
        dfs(root);
        return min2==INT_MAX?-1:min2;
    }
    void dfs(TreeNode* root){
        if(!root)return ;
        if(min2>root->val&&root->val!=min1){
            min2 = root->val;
            //cout<<min2<<" "<<min1<<endl;
        }
            
        if(min2<min1&&min2!=INT_MAX)
            swap(min2,min1);
        dfs(root->left);
        dfs(root->right);
    }
```


## [674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

>
>
>
> 题目	寻找最长上升子序列
>

思路：dp

C++

```C++
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size()==0)return 0;
        vector<int>res(nums.size(),1);
        for(int i=1;i<nums.size();i++){
            if(nums[i]>nums[i-1])
                res[i] = res[i-1]+1;
            else
                res[i] = 1;
        }
        int m = INT_MIN;
        for(auto x:res){
           // cout<<x<<endl;
            m = max(x,m);
        }
        return m;
    }
```


## [680. Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii//)

>
>
>
> 题目	在最多删除一个字符的情况下，判断一个字符串是否是回文字符串
>

思路：dp

C++

```C++
    bool validPalindrome(string s) {
        if(s.size()<2)return false;
        int l=0,r=s.size()-1;
        while(l<r){
            if(s[l]==s[r]){
                l++;
                r--;
            }else if(check(s,l+1,r)||check(s,l,r-1)){
                return true;
            }
            else return false;
        }
        return true;
    }
    bool check(string s, int l,int r){
        while(l<r){
            if(s[l]!=s[r])return false;
            l++;
            r--;
        }
        return true;
    }
```





## [687. Longest Univalue Path](https://leetcode.com/problems/longest-univalue-path/)

>
>
>
> 题目	找出二叉树中最长的相同数字的路径长度
>

思路：

C++

```C++
        int longestUnivaluePath(TreeNode* root) {
        if(!root)return 0;
        int cur = 0;
        dfs(root,cur);
        return cur;
        
    }
    int dfs(TreeNode* root,int& cur){
        int l = root->left?dfs(root->left,cur):0;
        int r = root->right?dfs(root->right,cur):0;
        l=(root->left && root->left->val==root->val) ? l+1 : 0;
        r=(root->right && root->right->val==root->val) ? r+1 : 0;
        cur=max(cur,l+r);
        return max(l,r);
    }
```



## [695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)

>
>
>
> 题目	找出2d数组中，包含最多相邻1的数目
>
>

思路：对于每一个点[i,j]，进行dfs判断上下左右走，走过的置visited为1，计算

C++

```C++
 int max=0;
    int count=0;
    int v[100][100];
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        for(int i=0;i<100;i++){
            for(int j=0;j<100;j++){
                v[i][j]=0;
            }
        }
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==1&&v[i][j]==0){
                   // cout<<i<<" "<<j<<endl;
                    count=0;
                    dfs(i,j,grid);
                }
            }
        }
        return max;
        
    }
    void dfs(int i,int j,vector<vector<int>>& g){
        if(i<0||i>=g.size()||j<0||j>=g[0].size())return ;
        if(v[i][j]==1)return;
        if(g[i][j]==0)return ;
        //cout<<i<<" "<<j<<endl;
        v[i][j]=1;
        count++;
        if(max<count)max=count;
      
        dfs(i+1,j,g);
        
        dfs(i,j+1,g);
       
        dfs(i-1,j,g);
      
        dfs(i,j-1,g);
    }
```


## [696. Count Binary Substrings](https://leetcode.com/problems/count-binary-substrings/)

>
>
>
> 题目
>
>



C++

```C++
     int countBinarySubstrings(string s) {
        int cur = 1;
        int pre = 0;
        int res = 0;
        for(int i=1;i<s.size();i++){
            if(s[i] == s[i-1])cur++;
            else{
                pre = cur;
                cur = 1;
            }
            if(pre >= cur)res++;
        }
        return res;
    }
```


## [697. Degree of an Array](https://leetcode.com/problems/degree-of-an-array/)

>
>
>
> 题目 找到数组中出现次数最多的频率数，然后计算出一个最短的子数组，使得这个子数组中的数字频率能够达到这个频率。
>
>



C++

```C++
         int findShortestSubArray(vector<int>& nums) {
        unordered_map<int,int>m;
        int m1 = INT_MIN;
     
        for(auto x:nums){
            m[x]++;
            if(m1<m[x]){
                m1 = m[x];
            }
        }
        
        int min1 = INT_MAX;
        for(auto x:m){
            if(x.second ==m1){
                min1 = min(min1,get(nums,x.first));
            }
            
        }
        return min1;
        
    }
    int get(vector<int>& nums,int n){
        int i = 0, j = nums.size()-1;
        while(i<nums.size()&&nums[i]!=n)
            i++;
        while(j>=0&&nums[j]!=n)
            j--;
        return j-i+1;
    }
```



## [754. Reach a Number](https://leetcode.com/problems/reach-a-number/)

>
>
>
> 思路：题目的意思是在i时刻，我们可以向左或者右走最多i步。要在最小步数能够达到target目标步数，那么利用贪心的思想，我们每一步应该是走最多，这样就可以越来越接近目标。当某一步超过目标时，在其中一步取负就可以了。
最简单的情况是：一直往右走,1+2+3 + … …+i = target。那么就是达到了目标。这时的情况就是i*(i+1)/2 = target。这显然是我们想要的最好的结果。
其他情况1：显然，目标步数不可能一直是往右走 就可以得到了的。
那么就是当 sum =i*(i+1) /2 >target 。这时走到最后一步时超过了目标，那么根据sum - target的值来判断哪一步或者哪些步应该往反向走。
if sum - target 是偶数步，就可以直接在sum - target 这一步做反向操作。其他步数都向正。
if sum - target 是奇数步 ，就要分类讨论了，若i为奇数i+1就是偶数 无论向左还是向右 都不会产生一个奇数的差来因此需要再走一步故要i+2步
若n为偶数，i+1则为奇数，可以产生一个奇数的差，故要i+1步.
---------------------
> 
>



C++

```C++
    int reachNumber(int target) {
        if(target<=0)return reachNumber(-target);
        int i=0;
        while(i*(i+1)/2<target){
            i++;
        }
        if(i*(i+1)/2==target)return i;
        if((i*(i+1)/2 - target) %2 ==0)return i;
        else
            if(i%2==0)return i+1;
            else return i+2;
    }
```



##[315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
**寻找当前数字后小于它的数字的个数，返回所有数字的数组**


**二分法**
```c++
vector<int> countSmaller(vector<int>& nums) {
        /*
        寻找当前数之后小于这个数的个数
        
        利用二分法：
            维护一个有序数组sorted
            由于是要找小于当前数的个数，那么我们从后到前遍历
            每一次将当前数添加到sorted数组中（找到第一个大于这个数的index，插入到结果数组中）
            
        */
        if(!nums.size())return nums;
        vector<int>res(nums.size(),0);
        vector<int>sorted;
        int l =0,r=0;
        for(int i=nums.size()-1;i>=0;i--){
            l = 0, r = sorted.size();
            while(l<r){
                int m = l+(r-l)/2;
                if(sorted[m]<nums[i]){
                    l = m+1;
                }else{
                    r = m;
                }
            }
            res[i]=r;
            sorted.insert(sorted.begin()+r,nums[i]);
        }
        return res;
    }
```

```c++
    vector<int> countSmaller(vector<int>& nums) {
        /*
        和二分法差不多思想
        只不过寻找index时采用STL的lower_bound函数
        
        */
        if(!nums.size())return nums;
        vector<int>res(nums.size(),0);
        vector<int>sorted;
        for(int i=nums.size()-1;i>=0;i--){
            int d = distance(sorted.begin(),lower_bound(sorted.begin(),sorted.end(),nums[i]));
            res[i]=d;
            sorted.insert(sorted.begin()+d,nums[i]);
        }
        return res;
    }

```


[239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
- 寻找每一个k长度的滑动窗口中的最大值,
- 使用deque的思想来做，保持一个单调队列。
- 前面出队，后面入队
- 每一次从后面入队之前，从后往前判断当前队列最后的值是否小于当前值
- 如果是：那么在当前值存在的情况下，这个值不可能为最大值，故而直接出队。
- 如果不是：则当前值<小于这个值，那么当前值在这个值出队之后，仍有机会为最大，故而直接进队。
- 使用deque保存的是当前index，当q.front()==i-k时，当前头结点出队。
-
```c++
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int>res;
        /**
        使用双向队列来储存index
        **/
        if(nums.size()==0)return res;
        deque<int>q;
        for(int i=0;i<nums.size();i++){
            while(!q.empty()&&q.front()==i-k)
                q.pop_front();
            while(!q.empty()&&nums[q.back()]<nums[i])
                q.pop_back();
            q.push_back(i);
            if(i>=k-1)
                res.push_back(nums[q.front()]);
        }
        return res;
    }

```

[395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)
- 题目：找个一个最长的子字符串，要求其中所有的字符都满足出现个数>=k
```c++
/**
递归：使用数据或者map记录当前i-j内所有字符的出现次数，寻找不满足条件的断点，然后递归下一轮，找到最长的子串。
**/
int longestSubstring(string s, int k) {
        int cnt[26]={0};
        for(auto x:s)
            cnt[x-'a']++;
        return cur(s,cnt,k,0,s.size());
    }
    int cur(string s,int *cnt,int k,int start,int last){
        int max_len  = 0;
        for(int i=start;i<last;){
            while(i<last && cnt[s[i]-'a']<k)i++;
            if(i==last)break;
            int j = i;
            int tem[26]={0};
            while(j<last&&cnt[s[j]-'a']>=k)tem[s[j++]-'a']++;
            if(i==start && j==last)return last - start;
            max_len = max(max_len, cur(s,tem,k,i,j));
            i = j;
        }
        return max_len;
    }

```
```python
    def longestSubstring(self, s, k):
        for i in set(s):
            if s.count(i)<k:
                return max(self.longestSubstring(t,k)for t in s.split(i))
        return len(s)
```


[207. Course Schedule](https://leetcode.com/problems/course-schedule/)
- 课程调度，判断有向图中是否存在环。
拓扑排序：在有向图中首先选择入度为0的节点，放入队列。
```c++
    bool canFinish(int numCourses, vector<pair<int, int>>& pre) {
        if(pre.size()==0||numCourses==0)return true;
        vector<vector<int>>g(numCourses,vector<int>(numCourses,0));
        for(auto x:pre){
            g[x.first][x.second] = 1;
        }
        vector<int>in(numCourses,0);
        for(int i=0;i<g.size();i++){
            for(int j=0;j<g[i].size();j++){
                if(g[i][j]==1){
                    in[j]++;
                }
            }
        }
        queue<int>q;
        for(int i=0;i<in.size();i++){
            if(in[i]==0){
                q.push(i);
             }
        }
        vector<int>top;
        while(!q.empty()){
            int cur = q.front();
            top.push_back(cur);
            q.pop();
            for(int j=0;j<g[cur].size();j++){
                if(g[cur][j]){
                in[j]--;
                if(in[j]==0)
                    q.push(j);
                }
            }
        }
        return top.size()==numCourses?1:0;
    }
```

[295. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/)
- 对于一个数据流，找到其中的中间值。
- 如果直接用vector来维护这个数组，每一次都得重新排序，时间复杂度接受不了。
- 使用优先队列priority_queue 来保持一个small 维护中间值左边的一半数，large维护中间值右边的一半数。因为优先队列时从大到小排序的，所有在large中使用-1*num保存从小到大的顺序。
- 每次到了一个数，首先进入small队列里，然后从small队列里找到最大的，加入large队列里，判断大小之后，在放最小的到small队列里。
- Amazzzzzzzzzzzzzzzing
- talk is cheap,show me the code.
```c++
    priority_queue<int>large,small;
    /** initialize your data structure here. */
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        small.push(num);
        large.push(-small.top());
        small.pop();
        if(small.size()<large.size()){
            small.push(-large.top());
            large.pop();
        }
    }
    
    double findMedian() {
        return (small.size()+large.size())%2?small.top():(small.top()-large.top())/2.0;
    }

```

[56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

```c++
/*
    1.贪心的思想：
        将所有的intervals根据start从小到大排序
        维护一个s和e用来继续当前最大的重叠区域
    2.初始化s=[0].start e = [0].end
        if e < [i].start:
            加入当前[s,e]
            更改s = [i].start
                e = [i].end
        if e> [i].start: 有重叠出现
            if e > [i].end 此前区域已经被当前最大区域覆盖
                continue
            if e<= [i].end 找到一个新的最大区域
                e = [i].end
    */

    vector<Interval> merge(vector<Interval>& in) {
        vector<Interval>res;
        if(in.size()==0)return res;
        sort(in.begin(),in.end(),cmp);
        Interval tem = Interval();
        int s = in[0].start;
        int e = in[0].end;
        if(in.size()==1){
            tem.start = s;
            tem.end = e;
            res.push_back(tem);
            return res;
        }
        for(int i=0;i<in.size();i++){
            if(in[i].start==s&&in[i].end==e){
                ;
            }
           // cout<<i<<endl;
           // cout<<s<< " "<<e<<endl;
            if(e<in[i].start){
                tem.start = s;
                tem.end=e;
                res.push_back(tem);
                if(i==in.size()-1){
                   // cout<<"end"<<endl;
                   // cout<<s<<" "<<e<<endl;
                  //  cout<<in[i].start<<" "<<in[i].end<<endl;
                    tem.start = in[i].start;
                    tem.end = in[i].end;
                    res.push_back(tem);
                    return res;
                }
                s = in[i].start;
                e = in[i].end;
              //  cout<<s<<" "<<e<<endl;
            }else if(e>in[i].start){
                if(e<=in[i].end)
                    e = in[i].end;
            }else{
                e = in[i].end;
            }
            if(i==in.size()-1){
                tem.start = s;
                tem.end = e;
                res.push_back(tem);
            }
        }
        return res;
    }
```

```python 
def merge(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[Interval]
        """
        res = []
        if not intervals:
            return res
        intervals.sort(key = lambda x:x.start)
        base = intervals[0]
        for i in range(1,len(intervals)):
            if base and base.end<intervals[i].start:
                res.append(base)
                base = intervals[i]
            else:
                base.start = min(base.start,intervals[i].start)
                base.end = max(base.end,intervals[i].end)
        res.append(base)
        return res
```

### [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

```c
    int search(vector<int>& nums, int target) {
        int l=0;
        int r=nums.size();
        while(l<r){
            int m =l+(r-l)/2;
            //如果m 和target在同一个递增序列中
            if((nums[m]<nums[0])==(target<nums[0])){
                if(nums[m]<target){
                    l=m+1;
                }else if(nums[m]>target){
                    r=m;
                }else{
                    return m;
                }//不在同一个递增中
            }else{
                if(nums[m]<nums[0]){
                    r = m;
                }else{
                    l = m+1;
                }
            }
        }
        return -1;
    }
```

```python
    def search(self, nums, target):
        l ,r = 0,len(nums)
        while l<r:
            m = l+(r-l)/2
            if (nums[m]<nums[0])==(target<nums[0]):
                if nums[m]<target:
                    l = m+1
                elif nums[m]>target:
                    r = m
                else:
                    return m
            else:
                if nums[m]<nums[0]:
                    r = m
                else:
                    l = m+1
        return -1
```

