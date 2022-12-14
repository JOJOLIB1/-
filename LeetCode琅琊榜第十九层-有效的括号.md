# LeetCode琅琊榜第十九层-有效的括号

[TOC]

------

## 题目解读

### 题目内容



### 关键信息

1. 左括号必须和必须用相同类型的右括号闭合
2. 左括号必须以正确的顺序闭合

### 实例分析

- ```java
  输入:s = "()"
  输出:true 
      示例解读:')'有唯一匹配他的'(',故为true
  ```

- ```java
  输入:s = "(){}[]"
  输出:true
      示例解读:')',']','}'都对应着有唯一匹配他们的'(','[','{'
  ```

- ```java
  输入:s = "([)]"
  输出:false
      示例解读:针对于括号之间的相交情况,题目所给为false,所以,当我们编写代码的时候,不能让他们相交
  ```

- ```java
  输入:s = "{[]}"
  输出:true
      实力解读:针对于括号之间的包含情况,题目所给为true,所以,是允许包含情况的
  ```

### 结论

```java
正确的格式如下:
1. "()" 闭合情况,无外界的插入
2. "([{}])"闭合情况,整个括号包含在里面
3. "([]{})"闭合情况,一个括号内可能有多个闭合括号
```

------

## 算法解读

### 使用该算法的原因

> 1. 我们发现,每一个右括号的左边一定会有一个对应的左括号,这个左括号有以下两个位置
>    1. 直接在这个右括号的左边
>    2. 在这个右括号左边的第2N个位置,因为里面有可能有N个成对的括号
>
> 2. 我们都可以每加一个右括号就匹配一次左括号,因为括号最后的成对出现都是里面先构造好然后构造外面
>
>    1. ```java
>       "((()"都是先构建里面,再构建外面,所以,依次匹配就可以将前面的左括号匹配完
>       ```
>
> 3. 栈的特性是先入后出,栈顶元素永远是最新加入的元素,而最新加入的元素正好是右括号需要匹配的左括号

### 算法

- 利用栈来解决该问题
- 时间复杂度为`O(N)` 
- 空间复杂度为`O(N)`

------

## 算法思路

1. 利用哈希表,将所有右括号对应的左括号作为键值对的形式存储,`灵活的替代了switch case,避免没有必要的判断`
2. 创建一个`stack`空间,用于压栈和弹栈
3. 将最新压入的元素进行判定,如果是`左括号类型`,直接压栈
4. 如果是`右括号类型`,进行比较
5. 最后检查栈空间的元素情况,返回结果

------

## 代码实现与解析

```java
class Solution {
    public boolean isValid(String s) {
        var map = new HashMap<Character,Character>();
        // 将所有的情况都记录在哈希表中
        map.put(')','(');
        map.put(']','[');
        map.put('}','{');
        // 习惯了,记录字符串的长度
        var n = s.length();
        // 如果该条件成立,说明括号总数是奇数,一定是false
        if (n % 2 == 1) {
            return false;
        }
        var stack = new Stack<Character>();
        // 循环遍历每一个元素
        for (int i = 0; i < n; i++) {
            // 获取当前下标的字符
            var c = s.charAt(i);
            // 查看是否是右括号
            if (map.containsKey(c)) {
                // 如果当前栈为空,一定没有对应的左括号,直接返回false
                // 如果栈顶元素不是其对应的左括号,返回false
                if (stack.isEmpty() || stack.peek() != map.get(c)) {
                    return false;
                }else {
                    // 说明是与其匹配的左括号,直接弹栈
                    stack.pop();
                }
            }else {
                // 说明是左括号,直接压栈
                stack.push(c);
            }
        }
        // 如果最后不为空,说明左括号多于右括号,一定没有与之对应的右括号,直接返回false
        return stack.isEmpty();
    }
}
```

------

## 算法总结

> 如果我们发现类似的括号问题,一般可以用栈这种数据结构去解决该列问题
>
> 由于博主刷题还是比较少,所以,的不出很好的总结,见谅
>
> 如果对算法有兴趣,一起研究哦!
