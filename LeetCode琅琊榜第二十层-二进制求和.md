# LeetCode琅琊榜第二十层-二进制求和

[TOC]

## 题目内容





------

## 二进制计算规则的复习

- 在引出二进制的计算规则之前,不妨再复习一下十进制的计算规则
  - 逢十进一,借一当十
- 二进制的计算规则
  - **逢二进一,借一当二**

------

## 示例分析

```java
输入 a = "11",b = "1"
输出 "100"
解析:
		我们不妨把他们两个的长度补成一致,即"011"与"001"
        1) 1 与 1 与 对应进位相加 = 10 => 这个位上面的结果是0,进位是1[注意,一开始的进位为0]
        2) 1 与 0 与 对应进位相加 = 10 => 这个位置上的结果是0,进位是1
        3) 0 与 0 与 对应进位相加 = 1 => 这个位置上的结果为1,进位为0
```

```java
输入 a = "11",b = "1"
输出 "100"
解析:
		我们不妨把他们两个的长度补成一致,即"011"与"001"
        1) 1 与 1 与 对应进位相加 = 10 => 这个位上面的结果是0,进位是1[注意,一开始的进位为0]
        2) 1 与 0 与 对应进位相加 = 10 => 这个位置上的结果是0,进位是1
        3) 0 与 0 与 对应进位相加 = 1 => 这个位置上的结果为1,进位为0
```

```java
输入 a = "1010",b = "1011"
输出 "10101"
解析:
		我们不妨把他们两个的长度补成一致,即"01010"与"01011"
        1) 1 与 0 与 对应进位相加 = 1 => 这个位上面的结果是1,进位是0[注意,一开始的进位为0]
        2) 1 与 1 与 对应进位相加 = 10 => 这个位上面的结果是0,进位是1
        3) 0 与 0 与 对应进位相加 = 1 => 这个位上面的结果是1,进位是0
        4) 1 与 1 与 对应进位相加 = 10 => 这个位上面的结果是0,进位是1
        5) 0 与 0 与 对应进位相加 = 1 => 这个位上面的结果是1,进位是0    
```

------

## 算法思想的引出

1. 我们发现每一位相加都是**对应两个数字与对应的进位相加而得到对应的结果**
2. 将上面所有的结果拼接起来,然后再将他们**翻转**过来就是最终索要的答案

------

## 代码实现与分析

```Java
class Solution {
    public String addBinary(String a, String b) {
        // carry 该变量用于记录进位的大小
        var carry = 0;
        // len 该变量用于记录较长的字符串的长度,作用请看解析F
        var len = Math.max(a.length(),b.length());
        // res 该变量用于拼接结果
        var res = new StringBuilder();
        // i的初始化,作用请看解析S
        for (int i = 0; i < len; i++) {
            // 代码思想请看解析T
            carry += i < a.length() ? (a.charAt(a.length() - 1 - i) - '0') : 0;
            carry += i < b.length() ? (b.charAt(b.length() - 1 - i) - '0') : 0;
            res.append((char) (carry % 2 + '0'));
            carry /= 2;
        }
        // 如果超过了最长长度,还有进位,即示例2,我们需要再拿一个位补1
        if (carry > 0) {
            res.append("1");
        }
        // 将结果翻转即可
        return res.reverse().toString();
    }
}
```

### 解析:

##### 解析F

- 在示例分析中提出了一个重要的操作,就是要将他们两个的字符串长度补成一样长,用`len`记录较长的字符串长度,就可以实现将他们两个字符串的长度补成一致

##### 解析X

- 首先,我们先明确,我们的算法目的是先从低位算起,然后再逐步推向高位的,即**低位 -> 高位**
- 其次,我们为什么不让`i = len - 1`呢?
  - 如果我们让`i = len - 1`,如果有一个较短的字符串会直接越界
  - 而且对于字符串边界的情况的判断会变得更加复杂
- 我们为什么让`i = 0`
  - 从 `i < a.length()`或`i < b.length()`可以直接看出是否越界的情况

##### 解析T

- 如果`i < a.length`,说明没有越界,由于`carry`是`int`类型,所以我们对其取到的字符`-'0'`取到其对应的整数数字,将整数数字叠加到`carry`
- 如果`i > a.length`,说明越界了,根据示例,我们需要在不满长度的地方补够对应的长度,补的地方数字为0,所以直接将0叠加到`carry`即可
- 同理b
- 将`carry%2 + '0'`的结果强制类型转换成`char`添加即可
  - 为什么强制类型转换?
    - 为了得到一个整数数字对应的字符,而不是添加一个整数数字
  - 为什么`%2`
    - 因为二进制的取值范围是`{0,1}`,所以要`%2`
- 将`carry/=2`
  - 为了拿到进位的结果
    - 比如`1 + 1 = 10 进位 = 1,换算成10进制是 1 + 1 = 2 , 2 / 2 = 1,刚好拿到其进位`

## 算法总结

> 该算法被称为模拟法,是官方解法的法一,法二涉及到位运算,待我把位运算的前置算法题目都刷完,再给大家带来这道题的第二种解法