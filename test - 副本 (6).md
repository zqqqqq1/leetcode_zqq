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

