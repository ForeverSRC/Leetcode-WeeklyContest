## [1.逐步求和得到正数的最小值](https://leetcode-cn.com/problems/minimum-value-to-get-positive-step-by-step-sum/)

### 题目描述
给你一个整数数组`nums`。你可以选定任意的**正数**`startValue`作为初始值。

你需要从左到右遍历`nums`数组，并将`startValue`依次累加上`nums`数组中的值。

请你在确保累加和始终大于等于`1`的前提下，选出一个最小的**正数**作为`startValue `。

**示例 1：**
>输入：nums = [-3,2,-3,4,2]
>输出：5
>解释：如果你选择 startValue = 4，在第三次累加时，和小于 1 。
>                累加求和
>                startValue = 4 | startValue = 5 | nums
>                  (4 -3 ) = 1  | (5 -3 ) = 2    |  -3
>                  (1 +2 ) = 3  | (2 +2 ) = 4    |   2
>                  (3 -3 ) = 0  | (4 -3 ) = 1    |  -3
>                  (0 +4 ) = 4  | (1 +4 ) = 5    |   4
>                  (4 +2 ) = 6  | (5 +2 ) = 7    |   2

**示例 2：**
>输入：nums = [1,2]
>输出：1
>解释：最小的 startValue 需要是正数。

**示例 3：**
>输入：nums = [1,-2,-3]
>输出：5
 
**提示：**

* `1 <= nums.length <= 100`
* `-100 <= nums[i] <= 100`

### 解题思路
#### 关键词：累加和
若保证每次累加的结果始终大于1，则需要保证在遇到数组元素逐次累加时，最小的累加和的情况下，结果仍然大于等于1。
因此，找到最小的累加和：

* 该值为正数，返回1
* 该值为负数，返回该值的绝对值加1

### 参考代码
```cpp
class Solution {
public:
    int minStartValue(vector<int>& nums) 
    {
        int n=nums.size();
        vector<int> sums(n+1,0);
        for(int i=1;i<=n;++i)
            sums[i]=sums[i-1]+nums[i-1];
        int mini=*(min_element(sums.begin()+1,sums.end()));
        int ans=0;
        if(mini<0)
            return 1-mini;
        else
            return 1;
    }
};
```

## [2.和为K的最少斐波那契数字数目](https://leetcode-cn.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/)

### 题目描述
给你数字`k` ，请你返回和为`k`的斐波那契数字的最少数目，其中，每个斐波那契数字都可以被使用多次。

斐波那契数字定义为：

* F1 = 1
* F2 = 1
* Fn = Fn-1 + Fn-2 ， 其中 n > 2 。

数据保证对于给定的`k` ，一定能找到可行解。

**示例 1：**

>输入：k = 7
>输出：2 
>解释：斐波那契数字为：1，1，2，3，5，8，13，……
>对于 k = 7 ，我们可以得到 2 + 5 = 7 。

**示例 2：**
>输入：k = 10
>输出：2 
>解释：对于 k = 10 ，我们可以得到 2 + 8 = 10 。

**示例 3：**
>输入：k = 19
>输出：3 
>解释：对于 k = 19 ，我们可以得到 1 + 5 + 13 = 19 。
 
**提示：**

* `1 <= k <= 10^9`

### 解题思路
#### 关键词：贪心
首先，所需的斐波那契数字的最大值一定不超过k，因此只需生成前k个斐波那契数字。
其次，要保证最少，则需要尽可能的选择最大的数字。因此每次迭代都去找到一个不小于k个数字并进行判断：

* 若k等于这个数字，则返回结果
* 若k不等于这个数字，则k减去这个数字的前一个数字，继续进行迭代

### 参考代码
```cpp
class Solution {
public:
    int findMinFibonacciNumbers(int k) {
        if(k==1)
            return 1;
        vector<int> fib(2,1);
        int temp=2;
        while(temp<k)
        {
            fib.push_back(temp);
            temp=fib.back()+(*(fib.end()-2));
        }
        if(temp==k)
            return 1;
        else
        {
            int ans=1;
            k-=fib.back();
            while(1)
            {
                auto index=lower_bound(fib.begin(),fib.end(),k);
              
                if(k==*index)
                    return ++ans;
                else
                {
                    k-=*(index-1);
                    ++ans;
                }
            }
        }
    }
};
```
## [3.长度为 n 的开心字符串中字典序第 k 小的字符串](https://leetcode-cn.com/problems/the-k-th-lexicographical-string-of-all-happy-strings-of-length-n/)

### 题目描述
一个 「开心字符串」定义为：

* 仅包含小写字母`['a', 'b', 'c']`.
* 对所有在`1`到`s.length - 1`之间的`i`，满足`s[i] != s[i + 1]` （字符串的下标从 1 开始）。

比方说，字符串 **"abc"** ，**"ac"**，**"b"** 和 **"abcbabcbcb"** 都是开心字符串，但是 **"aa"**，**"baa"** 和 **"ababbc"** 都不是开心字符串。

给你两个整数`n`和`k`，你需要将长度为`n`的所有开心字符串按字典序排序。

请你返回排序后的第`k`个开心字符串，如果长度为`n`的开心字符串少于`k`个，那么请你返回**空字符串**。

**示例 1：**
>输入：n = 1, k = 3
>输出："c"
>解释：列表 ["a", "b", "c"] 包含了所有长度为 1 的开心字符串。按照字典序排序后第三个字符串为 "c" 。

**示例 2：**
>输入：n = 1, k = 4
>输出：""
>解释：长度为 1 的开心字符串只有 3 个。

**示例 3：**
>输入：n = 3, k = 9
>输出："cab"
>解释：长度为 3 的开心字符串总共有 12 个 ["aba", "abc", "aca", "acb", "bab", "bac", "bca", "bcb", "cab", "cac", "cba", "cbc"] 。第 9 个字符串为 "cab"

**示例 4：**
>输入：n = 2, k = 7
>输出：""

**示例 5：**
>输入：n = 10, k = 100
>输出："abacbabacb"


 **提示：**

* `1 <= n <= 10`
* `1 <= k <= 100`

### 解题思路
#### 关键词：全排列
参考[wnjxyk](https://www.bilibili.com/video/BV1w54y1R74L?p=4/)的思路，首先生成所有情况下的全排列，然后统计符合开心字符串的全排列的数目，并返回第k个全排列。
为了降低算法复杂度，采用状态压缩的方法，利用3进制代表字符串，令a=0,b=1,c=2，例如n=2时：
```cpp
aa      0     00
ab      1     01
ac      2     02
ba      3     10
bb      4     11
bc      5     12
ca      6     20
cb      7     21
cc      8     22
```
于是只需判断每个三进制数的当前位是否等于前一位，若不等于，则该数代表的是一个合法的开心字符串。
由此找到第k个开心字符串即可。
### 参考代码
```cpp

class Solution {
public:
    string getHappyString(int n, int k) 
    {
        int total=1;
        for(int i=0;i<n;++i)
            total*=3;
        int count=0;
        for(int s=0;s<total;++s)
        {
            bool flag=true;
            for(int i=1,v=s,last=3;i<=n&&flag;++i)
            {
                int cur=v%3;
                v/=3;
                if(last==cur)
                    flag=false;
                last=cur;
            }
            if(flag)
                ++count;
            if(count==k)
            {
                string ans="";
                for(int i=1,v=s;i<=n;++i)
                {
                    int cur=v%3;
                    v/=3;
                    ans=(char)('a'+cur)+ans;
                }
                return ans;
            }
        }
        return "";
    }
};


```

## [4.恢复数组](https://leetcode-cn.com/problems/restore-the-array)
### 题目描述
某个程序本来应该输出一个整数数组。但是这个程序忘记输出空格了以致输出了一个数字字符串，我们所知道的信息只有：数组中所有整数都在`[1, k]`之间，且数组中的数字都没有前导0 。

给你字符串`s`和整数`k`。可能会有多种不同的数组恢复结果。

按照上述程序，请你返回所有可能输出字符串`s`的数组方案数。

由于数组方案数可能会很大，请你返回它对`10^9 + 7` 取余后的结果。

**示例 1：**
>输入：s = "1000", k = 10000
>输出：1
>解释：唯一一种可能的数组方案是 [1000]

**示例 2：**
>输入：s = "1000", k = 10
>输出：0
>解释：不存在任何数组方案满足所有整数都 >= 1 且 <= 10 同时输出结果为 s 。

**示例 3：**
>输入：s = "1317", k = 2000
>输出：8
>解释：可行的数组方案为 [1317]，[131,7]，[13,17]，[1,317]，[13,1,7]，[1,31,7]，[1,3,17]，[1,3,1,7]

**示例 4：**
>输入：s = "2020", k = 30
>输出：1
>解释：唯一可能的数组方案是 [20,20] 。 [2020] 不是可行的数组方案，原因是 2020 > 30 。 [2,020] 也不是可行的数组方案，因为 020 含有前导 0 。

**示例 5：**
>输入：s = "1234567890", k = 90
>输出：34


**提示：**

* `1 <= s.length <= 10^5`
* `s`只包含数字且不包含前导`0`。
* `1 <= k <= 10^9`

### 解题思路
#### 关键词：动态规划
令dp[i]表示前i个字符组成的字符串的可能方案数。
则状态转移方程为：
```
dp[i+1]=dp[i]+dp[i-1]+...+dp[0]
```
但上述的每个dp[i]并非全部需要加，当满足：

* 当前子串长度小于等于`10`
* 子串代表的数字在`[1,k]`内
* 子串没有前导零

这三个条件时，可进行dp[i]的累加。
因为k的最大值为1e9，因此子串只要判定10位即可。这一点也是**保证算法不会超时**的要点。

### 参考代码
```python
class Solution:
    def numberOfArrays(self, s: str, k: int) -> int:
        n=len(s)
        dp=[0]*(n+1)
        dp[0]=1
        for i in range(n):
            temp=0
            if 1<=int(s[i])<=k:
                temp+=dp[i]
            for j in range(i-1,-1,-1):
                if  i+1-j>10:
                    break
                if 1<=int(s[j:i+1])<=k and s[j]!='0':
                    temp+=dp[j]
            dp[i+1]=temp
        return dp[-1]%1000000007
```