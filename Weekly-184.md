# 第184场周赛
##  [1.数组中的字符串匹配](https://leetcode-cn.com/problems/string-matching-in-an-array)
### 题目描述
给你一个字符串数组`words`，数组中的每个字符串都可以看作是一个单词。请你按**任意**顺序返回`words`中是其他单词的子字符串的所有单词。
如果你可以删除`words[j]`最左侧和/或最右侧的若干字符得到 `word[i]`，那么字符串`words[i]`就是`words[j]`的一个子字符串。
**示例 1：**
>输入：words = ["mass","as","hero","superhero"]
输出：["as","hero"]
解释："as" 是 "mass" 的子字符串，"hero" 是 "superhero" 的子字符串。["hero","as"] 也是有效的答案。

**示例2：**
>输入：words = ["leetcode","et","code"]
输出：["et","code"]
解释："et" 和 "code" 都是 "leetcode" 的子字符串。

**示例3：**
>输入：words = ["blue","green","bu"]
输出：[]

**提示：**

* `1 <= words.length <= 100`
* `1 <= words[i].length <= 30`
* `words[i]`仅包含小写英文字母。
* 题目数据**保证**每个`words[i]`都是独一无二的。

### 解题思路
##### 关键词：循环
循环判断每个字符串是否是其他字符串的子串。
可先将字符串按长度由小到大排序，再进行比较。
### 参考代码
```python
class Solution:
    def stringMatching(self, words: List[str]) -> List[str]:
        words.sort(key=lambda x:len(x))
        ans=[]
        n=len(words)
        for i in range(0,n):
            for j in range(i+1,n):
                if words[i] in words[j]:
                    ans.append(words[i])
                    break
        return ans
```

## [2.查询带键的排列](https://leetcode-cn.com/problems/queries-on-a-permutation-with-key/)

### 题目描述
给你一个待查数组`queries`，数组中的元素为`1`到`m`之间的正整数。 请你根据以下规则处理所有待查项`queries[i]`（从`i=0`到`i=queries.length-1`）：

* 一开始，排列 P=[1,2,3,...,m]。
* 对于当前的`i`，请你找出待查项`queries[i]`在排列`P`中的位置（**下标从 0 开始**），然后将其从原位置移动到排列`P`的起始位置（即下标为 0 处）。注意，`queries[i]` 在`P`中的位置就是`queries[i]`的查询结果。
请你以数组形式返回待查数组`queries`的查询结果。

**示例1：**
>输入：queries = [3,1,2,1], m = 5
输出：[2,1,2,1] 
解释：待查数组 queries 处理如下：
对于 i=0: queries[i]=3, P=[1,2,3,4,5], 3 在 P 中的位置是 2，接着我们把 3 移动到 P 的起始位置，得到 P=[3,1,2,4,5] 。
对于 i=1: queries[i]=1, P=[3,1,2,4,5], 1 在 P 中的位置是 1，接着我们把 1 移动到 P 的起始位置，得到 P=[1,3,2,4,5] 。 
对于 i=2: queries[i]=2, P=[1,3,2,4,5], 2 在 P 中的位置是 2，接着我们把 2 移动到 P 的起始位置，得到 P=[2,1,3,4,5] 。
对于 i=3: queries[i]=1, P=[2,1,3,4,5], 1 在 P 中的位置是 1，接着我们把 1 移动到 P 的起始位置，得到 P=[1,2,3,4,5] 。 
因此，返回的结果数组为 [2,1,2,1] 。  

**示例2：**
>输入：queries = [4,1,2,2], m = 4
输出：[3,1,2,0]

**示例3：**
>输入：queries = [7,5,5,8,3], m = 8
>输出：[6,5,0,7,5]

**提示：**

* `1 <= m <= 10^3`
* `1 <= queries.length <= m`
* `1 <= queries[i] <= m`

### 参考思路
##### 关键词：模拟
由于数据量较少，可支持一个$O(n^{2})$的算法，每次查询后，逐次遍历重新调整顺序。
P通过哈希表进行模拟，键为1至m，值为对应数字的位置。
更新的具体过程为：每次查询后，判断其余数字与当前数字的位置关系，若当前数字的位置在某一数字后，则某一数字的位置加1，否则位置不变。
### 参考代码
```cpp
class Solution {
public:
    vector<int> processQueries(vector<int>& queries, int m) {
        int n=queries.size();
        unordered_map<int,int> hash;
        for(int i=1;i<=m;++i)
            hash[i]=i-1;
        vector<int> ans(n,0);
        for(int i=0;i<n;++i)
        {
            ans[i]=hash[queries[i]];
            for(int j=1;j<=m;++j)
            {
                if(hash[queries[i]]>hash[j])
                    hash[j]++;
            }
            hash[queries[i]]=0;
        }
        return ans;
    }
};
```

## [3.HTML实体解析器](https://leetcode-cn.com/problems/html-entity-parser)

### 题目描述
「HTML 实体解析器」 是一种特殊的解析器，它将 HTML 代码作为输入，并用字符本身替换掉所有这些特殊的字符实体。

HTML 里这些特殊字符和它们对应的字符实体包括：

* 双引号：字符实体为 `&quot; `，对应的字符是 `"` 。
* 单引号：字符实体为 `&apos;` ，对应的字符是 `'` 。
* 与符号：字符实体为 `&amp;` ，对应对的字符是 `&` 。
* 大于号：字符实体为 `&gt;` ，对应的字符是 `>` 。
* 小于号：字符实体为 `&lt;` ，对应的字符是 `<` 。
* 斜线号：字符实体为 `&frasl;` ，对应的字符是 `/` 。

给你输入字符串`text` ，请你实现一个`HTML`实体解析器，返回解析器解析后的结果。

**示例 1：**
>输入：text = "&amp; is an HTML entity but &ambassador; is not."
>输出："& is an HTML entity but &ambassador; is not."
>解释：解析器把字符实体 &amp; 用 & 替换

**示例 2：**
>输入：text = "and I quote: &quot;...&quot;"
>输出："and I quote: \"...\""

**示例 3：**
>输入：text = "Stay home! Practice on Leetcode :)"
>输出："Stay home! Practice on Leetcode :)"

**示例 4：**
>输入：text = "x &gt; y &amp;&amp; x &lt; y is always false"
>输出："x > y && x < y is always false"

**示例 5：**
>输入：text = "leetcode.com&frasl;problemset&frasl;all"
>输出："leetcode.com/problemset/all"
 
**提示：**
* `1 <= text.length <= 10^5`
* 字符串可能包含 256 个ASCII 字符中的任意字符。

### 参考思路
##### 关键词：遍历 替换
特殊字符均以`&`开头，以`;`结尾，为其开始于结束提供了标识。
题目中需要替换的6种特殊字符，长度种类为3、4、5、6。因此，当遍历原字符串，遇到`&`字符时，根据对应的长度判断某个子串是否是`&...;`形式的：

* 如果是，则再次判断是否是需要替换的字符
    * 如果是，则替换
    * 如果不是，则将原字符串原封不动的加入答案
* 如果不是，将子串原封不动的加入答案，继续遍历

### 参考代码
```python
d={"&quot;":"\"","&apos;":"'","&amp;":"&","&gt;":">","&lt;":"<","&frasl;":"/"}
delta=[3,4,5,6]
class Solution:
    def entityParser(self, text: str) -> str:
        ans=""
        n=len(text)
        i=0
        while i<n:
            if text[i]!="&":
                ans+=text[i]
                i+=1
            else:
                temp=""
                for j in range(4):
                    if text[i+delta[j]]==";":
                        try:
                            temp=d[text[i:i+delta[j]+1]]
                            ans+=temp
                        except:
                            ans+=text[i:i+delta[j]+1]
                        finally:
                            i=i+delta[j]+1
                        break
                else:
                    ans+=text[i:i+delta[j]+1]
                    i=i+delta[j]+1
        return ans
```
### 他山之石
[wnjxyk](https://www.bilibili.com/video/BV1DK411L7Yo?p=4)大神通过Python字符串的`replace`方法实现对应子串的暴力替换。

## [4.给N×3网格涂色的方案数](https://leetcode-cn.com/problems/number-of-ways-to-paint-n-x-3-grid)

### 题目描述
你有一个 `n x 3` 的网格图 `grid` ，你需要用**红，黄，绿**三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜色不同）。

给你网格图的行数`n` 。

请你返回给`grid`涂色的方案数。由于答案可能会非常大，请你返回答案对 `10^9 + 7` 取余的结果。

**示例 1：**
>输入：n = 1
>输出：12
>解释：总共有 12 种可行的方法：
>![a05d8e0929929c2257c3a57d91f35b83.png](en-resource://database/20837:1)

**示例 2：**
>输入：n = 2
>输出：54

**示例 3：**
>输入：n = 3
>输出：246

**示例 4：**
>输入：n = 7
>输出：106494

**示例 5：**
>输入：n = 5000
>输出：30228214
 

提示：
* `n == grid.length`
* `grid[i].length == 3`
* `1 <= n <= 5000`

### 参考思路
##### 关键词：递推
示例1中已经给出了n=1时的基本情况。现以ABC来代表三种颜色，则这12种结果可分为两类：ABA型与ABC型。
其中，ABA型包括：

* ABA
* ACA
* BAB
* BCB
* CAC
* CBC

ABC型包括：

* ABC
* ACB
* BAC
* BCA
* CAB
* CBA

由于每行的结果只受上一行的影响，因此仅考虑上一行的情况。
通过观察分析可得：

* 上一行为ABA型
    * 当前行可用的ABC型为2种
    * 当前行可用的ABA型为3种
* 上一行为ABC型
    * 当前行可用的ABC型为2种
    * 当前行可用的ABA型为2种

因此当前行的结果为：`a*3+b*2+a*2+b*2`
其中a表示上一行ABA型的结果，b表示上一行ABC型的结果

### 参考代码
```python
class Solution:
    def numOfWays(self, n: int) -> int:
        a=6
        b=6
        tempa=6 #ABA
        tempb=6 #ABC
        for i in range(n-1):
            tempa=a*3+b*2
            tempb=a*2+b*2
            a=tempa
            b=tempb
        return (tempa+tempb)%1000000007
```