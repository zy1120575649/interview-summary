---
title: 腾讯精选练习题50题-字符串转换整数 (atoi)
date: 2021-01-11 15:03:03
tags:
	- 腾讯算法
categories: 算法刷题

---



请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

- 如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
- 假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
- 该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。

假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

**注意：**

- 本题中的空白字符只包括空格字符 `' '` 。
- 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 `[−231, 231 − 1]`。如果数值超过这个范围，请返回 `231 − 1` 或 `−231` 。

 

**示例 1:**

```
输入: "42"
输出: 42
```

**示例 2:**

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

**示例 3:**

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

**示例 4:**

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

**示例 5:**

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

 

**提示：**

- `0 <= s.length <= 200`
- `s` 由英文字母（大写和小写）、数字、`' '`、`'+'`、`'-'` 和 `'.'` 组成

```java


/*
 * @lc app=leetcode.cn id=8 lang=java
 *
 * [8] 字符串转换整数 (atoi)
 */

// @lc code=start
class Solution {
  
    public int myAtoi(String s) {
       int n = s.length();
       int k=0;
       while(k<n&&s.charAt(k)==' '){
        k++;
       }
       if(k==n){
           return 0;
       }

       int op = 1;//假设是正号
       if(s.charAt(k)=='-'){
           op=-1;
           k++;
       }else if(s.charAt(k)=='+'){k++;}
/*
算法1
用long记录

1、找到第一个非空的字符位置k，若k == n表示全都是空字符，直接返回0
2、op记录截取后的整数的正负性，1表示正号，-1表示负号
3、使用long类型的res来存储截取后的整数（一定是正整数，因为括号还没包括），在某一刻若res > INT_MAX，则break，表示已经超出
4、截取到res后，附上op的符号，若res > INT_MAX或者res < INT_MIN则分别返回INF_MAX或者INT_MIN
时间复杂度 O(n)
*/
    //    long res = 0;
    //    while(k<n&&s.charAt(k)>='0' && s.charAt(k)<='9'){
    //         res = res * 10 + s.charAt(k)-'0';
    //         k++;

    //         if(res>Integer.MAX_VALUE) break;
    //    }
    //    res= res*op;
    //    if(res>Integer.MAX_VALUE) res=Integer.MAX_VALUE;
    //    if(res<Integer.MIN_VALUE) res= Integer.MIN_VALUE;
    //    return (int)res;
/*
算法2
用int记录

1、找到第一个非空的字符位置k，若k == n表示全都是空字符，直接返回0
2、op记录截取后的整数的正负性，1表示正号，-1表示负号
3、使用int类型的res来存储截取后的整数（一定是正整数，因为括号还没包括），
在某一刻若op是正数，(res * 10 + x) * op > INT_MAX，即res * op > (INT_MAX - x ) / 10，则返回INF_MAX，表示已经超出（其中x是正数）
在某一刻若op是负数，(res * 10 + x) * op < INT_MIN，即res * op > (INT_MIN - x * (-1)) / 10，则返回INF_MIN，表示已经超出（其中x是负数）
4、截取到res后，附上op的符号，若res > INT_MAX或者res < INT_MIN则分别返回INF_MAX或者INT_MIN
5、最后返回res
时间复杂度 O(n)
*/
        int res=0;
        while(k<n&&s.charAt(k)>='0'&&s.charAt(k)<='9'){
            int x = s.charAt(k)-'0';
            // if(op>0&&res*op>(Integer.MAX_VALUE-x)/10){
            //     res =  Integer.MAX_VALUE;
            //     break;
            // }
            // if(op<0&&res*op<(Integer.MIN_VALUE-x*(-1))/10){
            //     res = Integer.MIN_VALUE;
            //     break;
            // }
            ////为了防止overflow，需要检测一下数字是否大于计算机可以表达的最大值和最小值
            //num = num * 10 + digt > Integer.MAX_VALUE
            //如果是负数就要检测是否num * 10 + digit < Integer.MIN_VALUE
            if(res>(Integer.MAX_VALUE-x)/10){
                return op==1?Integer.MAX_VALUE:Integer.MIN_VALUE;
            }

            res = res*10+s.charAt(k) - '0';
            k++;
            // int x = arr[k] - '0';
            // if (minus > 0 && res > (Integer.MAX_VALUE - x) / 10) return Integer.MAX_VALUE;
            // if (minus < 0 && -res < (Integer.MIN_VALUE + x) / 10) return Integer.MIN_VALUE;
            // if (-res * 10 - x == Integer.MIN_VALUE) return Integer.MIN_VALUE;
            // res = res * 10 + x;
            // k++;

        }
        return res*op;
    }
}
// @lc code=end


```






