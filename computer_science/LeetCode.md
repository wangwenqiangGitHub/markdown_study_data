# 每日一题

## 1. 猜数字的大小(二分查找法)

猜数字游戏的规则如下：

每轮游戏，我都会从 1 到 n 随机选择一个数字。 请你猜选出的是哪个数字。
如果你猜错了，我会告诉你，你猜测的数字比我选出的数字是大了还是小了。
你可以通过调用一个预先定义好的接口 int guess(int num) 来获取猜测结果，返回值一共有 3 种可能的情况（-1，1 或 0）：

-1：我选出的数字比你猜的数字小 pick < num
1：我选出的数字比你猜的数字大 pick > num
0：我选出的数字和你猜的数字一样。恭喜！你猜对了！pick == num
返回我选出的数字。



<font color = blue>方法一：左边界放大加一，右边界缩小减一，相等时停止</font>

```java
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is lower than the guess number
 *			      1 if num is higher than the guess number
 *               otherwise return 0
 * int guess(int num);
 */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int left = 1; 
        int right = n;
        int mid = 0;
        while (left <= right)
        {
            mid = left + (right - left) / 2;
            if (guess(mid) == -1)
            {
                right = mid - 1;
            }
            if (guess(mid) == 1)
            {
                left = mid + 1;
            }
        }
        return mid;
    }
}
```

<font color = blue>方法二：左边界加一放大</font>

```c
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is lower than the guess number
 *			      1 if num is higher than the guess number
 *               otherwise return 0
 * int guess(int num);
 */

int guessNumber(int n){
    int left = 1;
    int right = n;
    int mid = 0;
    //left < right的最后一个循环就是left + 1 = right，此时mid = 2left + 1 / 2 = left
    while (left < right)
    {
        mid = left + (right - left) / 2;
        if (guess(mid) == 1)
        {
            left = mid + 1;
        }
        if (guess(mid) <= 0)
        {
            right = mid;
        }
        //判断到了，就不用遍历了，当然如果遍历只是消耗时间而已
        if (guess(mid) == 0)
            return mid;
    }
    return left;
}
```

注意左右边界相等。

## 2. 山峰数组(二分查找法)

符合下列属性的数组 arr 称为 山脉数组 ：

```
arr.length >= 3
存在 i（0 < i < arr.length - 1）使得：
arr[0] < arr[1] < ... arr[i-1] < arr[i]
arr[i] > arr[i+1] > ... > arr[arr.length - 1]
给你由整数组成的山脉数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i 。
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/peak-index-in-a-mountain-array
<font color = blue>方法一：遍历数组</font>

```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int length = arr.length;
        int i, j;
        for (i = 0, j = 1; j < length; j++, i++)
        {
            if (arr[j] < arr[i])
                break;
        }
        return i;
    }
}
```

<font color = blue>方法二：二分查找</font> 

```c
int peakIndexInMountainArray(int* arr, int arrSize){
    int left = 1;
    int right = arrSize - 2;
    int num = 0;
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (arr[mid] > arr[mid + 1])
        {
            num = mid;
            right = mid -1;
        }
        else
            left = mid +1;
    }
    return num;
}
```

while循环中的条件要保证遍历完这个数组。当遍历在山脉右边时就要改变山顶的值。

> 上面二分查找都是遍历完才退出，而不是找到就退出，因为找到的条件不够明确。

## 3. 石子游戏(数学方法)

亚历克斯和李用几堆石子在做游戏。偶数堆石子排成一行，每堆都有正整数颗石子 piles[i] 。

游戏以谁手中的石子最多来决出胜负。石子的总数是奇数，所以没有平局。

亚历克斯和李轮流进行，亚历克斯先开始。 每回合，玩家从行的开始或结束处取走整堆石头。 这种情况一直持续到没有更多的石子堆为止，此时手中石子最多的玩家获胜。

假设亚历克斯和李都发挥出最佳水平，当亚历克斯赢得比赛时返回 true ，当李赢得比赛时返回 false 。

**示例：**

> 输入：[5,3,4,5]
> 输出：true

解释：
亚历克斯先开始，只能拿前 5 颗或后 5 颗石子 。
假设他取了前 5 颗，这一行就变成了 [3,4,5] 。
如果李拿走前 3 颗，那么剩下的是 [4,5]，亚历克斯拿走后 5 颗赢得 10 分。
如果李拿走后 5 颗，那么剩下的是 [3,4]，亚历克斯拿走后 4 颗赢得 9 分。
这表明，取前 5 颗石子对亚历克斯来说是一个胜利的举动，所以我们返回 true 。

**提示：**

>2 <= piles.length <= 500
>piles.length 是偶数。
>1 <= piles[i] <= 500
>sum(piles) 是奇数。

![image-20210616211342616](https://i.loli.net/2021/06/16/NSyOWBe4k2oab6C.png)

![](https://assets.leetcode-cn.com/solution-static/877/877_fig1.png)

```java
class Solution {
    public boolean stoneGame(int[] piles) {
        return true;
    }
}
```

## 4. 罗马数字转换


罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做 `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

**示例 1:**

```
输入: "III"
输出: 3
```

**示例 2:**

```
输入: "IV"
输出: 4
```

**示例 3:**

```
输入: "IX"
输出: 9
```

**示例 4:**

```
输入: "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

**示例 5:**

```
输入: "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

 

**提示：**

- `1 <= s.length <= 15`
- `s` 仅含字符 `('I', 'V', 'X', 'L', 'C', 'D', 'M')`
- 题目数据保证 `s` 是一个有效的罗马数字，且表示整数在范围 `[1, 3999]` 内
- 题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
- IL 和 IM 这样的例子并不符合题目要求，49 应该写作 XLIX，999 应该写作 CMXCIX 。
- 关于罗马数字的详尽书写规则，可以参考 [罗马数字 - Mathematics ](https://b2b.partcommunity.com/community/knowledge/zh_CN/detail/10753/罗马数字#knowledge_article)

```c

int romanToInt(char * s){
    int array[100];
    
    array['I'] = 1;
    array['V'] = 5;
    array['X'] = 10;
    array['L'] = 50;
    array['C'] = 100;
    array['D'] = 500;
    array['M'] = 1000;

    int num = 0;
    while ( *(s + 1) != '\0')
    {
        int current = array[*s];
        if (current >= array[*(s + 1)])//当前的大于后面的就执行两者加法
        {
            num += current;
        }
        else//否则就执行减法
            num -= current;
        s += 1;
    }
    return num + array[*s];
}
```

## 5. 整数反转

![image-20210618102016164](https://i.loli.net/2021/06/18/PUbZpw8t9XNf746.png)

![image-20210618102051359](https://i.loli.net/2021/06/18/gu2TJQBGq1FzS3M.png)

![image-20210618102124853](https://i.loli.net/2021/06/18/aiorXZHjkAQgF4G.png)

$$digit\in [0, 9]且为整数$$

当$$ rev = \frac{INT\_MAX}{10}$$，因为$$digit$$是通过x取余得到的，而x是int类型，所以$$digit$$只有1或2两种可能。

* 证明：当x < 0时

  判断条件必须满足：

  
  $$
  rev \cdot10 + digit \geq INT\_MIN
  $$

  $$
  INT\_MIN = -2^{31} = \frac{INT\_MIN}{10}\cdot10 -7
  $$

  $$
  rev \cdot10 + digit \geq\frac{INT\_MIN}{10}\cdot10 -7
  $$

  $$
  (rev - \frac{INT\_MIN}{10})\cdot10 \geq digit - 7
  $$

  $$ digit \in [-9, 0]且为整数$$====>$digit - 7 \in[-7,2]$

  * 当$rev > \frac{INT\_MIN}{10}$时，左边结果大于10，必成立
  * 当$rev = \frac{INT\_MIN}{10}$时，左边结果为0，digit 只能为-1,或-2，也必成立
  * 当$rev < \frac{INT\_MIN}{10}$时，左边结果小于-10，不成立

```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0)
        {
            if (x < 10 && (rev < -214748364 || rev > 214748364))//根据题目要求，只有可能是最后一位出问题
                return 0;
            rev = rev * 10 + x % 10;
            x /= 10;
        }
        return rev;
    }   
}
```

## 6. 回文数

给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。

**示例 1：**

> 输入：x = 121
> 输出：true

**示例 2：**

> 输入：x = -121
> 输出：false

解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
**示例 3：**

> 输入：x = 10
> 输出：false

解释：从右向左读, 为 01 。因此它不是一个回文数。
**示例 4：**

> 输入：x = -101
> 输出：false

**提示：**

> -231 <= x <= 231 - 1

<font color = blue >方法一：自已的解法</font>

```c
bool isPalindrome(int x){
    if (x < 0)
    {
        return false;
    }
    int array[10] = {0};
    int i;
    for (i = 0; x ; i++)
    {
        array[i] = x % 10;
        x /= 10;
    }
    int left = 0;
    int right = i - 1;
    while (left < right)
    {
        if (array[left++] != array[right--])
        {
            return false;
        }
    }
    return true;
}
```

<font color = blue >方法二：官方解法</font>

映入脑海的第一个想法是将数字转换为字符串，并检查字符串是否为回文。但是，这需要额外的非常量空间来创建问题描述中所不允许的字符串。

第二个想法是将数字本身反转，然后将反转后的数字与原始数字进行比较，如果它们是相同的，那么这个数字就是回文。
但是，如果反转后的数字大于 $int.MAX$，我们将遇到整数溢出问题。

按照第二个想法，为了避免数字反转可能导致的溢出问题，为什么不考虑只反转 $int$ 数字的一半？毕竟，如果该数字是回文，其后半部分反转后应该与原始数字的前半部分相同。

例如，输入 1221，我们可以将数字 “1221” 的后半部分从 “21” 反转为 “12”，并将其与前半部分 “12” 进行比较，因为二者相同，我们得知数字 1221 是回文。

```java
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0 || (x % 10 == 0 && x != 0))
        {
            return false;
        }
        int reverNumer = 0;
        while (x > reverNumer) //直接从后向前求得后面是数字，直到两者相等(偶数时)或x更小(奇数时)
        {
            reverNumer = reverNumer * 10 + x % 10;
            x /= 10;
        }
        return x == reverNumer || x == reverNumer / 10; //简单的去除并不会影响结果
    }
}
```

## 7. 最长公共前缀(纵向比较，横向比较)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

**示例 1：**

> 输入：strs = ["flower","flow","flight"]
> 输出："fl"

**示例 2：**

> 输入：strs = ["dog","racecar","car"]
> 输出：""
> 解释：输入不存在公共前缀。

**提示**：

* 0 <= strs.length <= 200
* 0 <= strs[i].length <= 200
* strs[i] 仅由小写英文字母组成

<font color = blue>方法一：纵向比较</font>

两个for循环来对每个字符串的每个位置进行比较。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int length = strs.length;
        if (strs == null || length == 0)
        {
            return "";
        }
        int firstLength = strs[0].length(); //第一个字符串的长度
        int i, j;

        for (i = 0; i < firstLength; i++) //i用来记录一个字符串中字符的位置
        {
            char c = strs[0].charAt(i);
            for (j = 0; j < length; j++) //j是字符数组中的字符串位置
            {
                if (i == strs[j].length() || c != strs[j].charAt(i)) //到最短串，或者不相等就返回
                {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0]; //只有一个字符串时
    }
}
```

<font color = blue >方法二：横向比较</font>

每次比较两个字符串，找到两个字符串的公共前缀，再拿这个字符串与后面的进行比较。

```c
#include <string.h>
//返回两个字符串的公共前缀
char * commPrefix(char *str1, char *str2)
{
    int length1 = strlen(str1);
    int length2 = strlen(str2);
    int length = length1 < length2 ? length1 : length2; //短的字符串长度
    int i;
    for (i = 0; i < length; i++)
    { 
        if (str1[i] != str2[i])
        {
            break; 
        }
    }
    char str[200] = "";
    return strncpy(str, str1, i); 
}
char * longestCommonPrefix(char ** strs, int strsSize){
    if (strsSize == 0 || !strcmp(strs[0], "\0")) //没有串或者是个空串
    {
        return "\0";
    }
    char *str0 = strs[0]; //str0记录公共前缀
    for (int i = 1; i < strsSize; i++)
    {
        str0 = commPrefix(strs[i], str0);
    }
    return str0;
}
```

## 8.  有效的括号(栈)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。

**示例 1：**

> 输入：s = "()"
> 输出：true

**示例 2：**

> 输入：s = "()[]{}"
> 输出：true

**示例 3：**

> 输入：s = "(]"
> 输出：false

**示例 4：**

> 输入：s = "([)]"
> 输出：false

**示例 5：**

> 输入：s = "{[]}"
> 输出：true


**提示：**

> 1 <= s.length <= 104
> s 仅由括号 '()[]{}' 组成

<font color = blue> 用栈编写</font>

```c
char getChar(char ch)
{
    if (ch == '}') return '{';
    if (ch == ']') return '[';
    if (ch == ')') return '(';
    return 0;
}
bool isValid(char * s){
    int length = strlen(s);
    if (length % 2 == 1)
        return false;
    int array[length + 1];
    int top = 0;
    for (int i = 0; i < length; i++)
    {
        char ch = getChar(s[i]);
        if (ch) //是右括号
        {
            if (top == 0 || ch != array[top - 1]) //是第一个或者不是最后一个左括号对应的
                return false;
            top--; //有对应的右括号
        }
        else //是左括号
            array[top++] = s[i]; //array是用来记录左括号的数组
    }
    return top == 0; //top是左括号的数量
}
```

## 9. 合并两个有序链表(双指针法，递归法)

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

![img](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

**示例 1：**

> 输入：l1 = [1,2,4], l2 = [1,3,4]
> 输出：[1,1,2,3,4,4]

**示例 2：**

> 输入：l1 = [], l2 = []
> 输出：[]

**示例 3：**

> 输入：l1 = [], l2 = [0]
> 输出：[0]

**提示：**

* 两个链表的节点数目范围是 [0, 50]
* -100 <= Node.val <= 100
* l1 和 l2 均按 非递减顺序 排列

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode *mergeTwoLists(struct ListNode *l1, struct ListNode *l2)
{
    struct ListNode *head = (struct ListNode *)malloc(sizeof(struct ListNode)); //头结点
    head->next = NULL;
    struct ListNode *record = head; //记录指针
    while (l1 != NULL && l2 != NULL)
    {
        if (l1->val < l2->val) //l1中的更小
        {
            record->next = l1;
            l1 = l1->next;
        }
        else //l2中的更小
        {
            record->next = l2;
            l2 = l2->next;
        }
        record = record->next;
    }
    record->next = l1 ? l1 : l2;
    return head->next; //返回
}
```

<font color = blue>方法二：递归</font>

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode *mergeTwoLists(struct ListNode *l1, struct ListNode *l2)
{
    if (l1 == NULL)
    {
        return l2;
    }
    if (l2 == NULL)
    {
        return l1;
    }
    if (l1->val < l2->val) //l1更小
    {
        l1->next = mergeTwoLists(l1->next, l2);
        return l1; //l1接上了，返回当前的l1
    }
    else
    {
        l2->next = mergeTwoLists(l1, l2->next);
        return l2;
    }
}
```

## 10. 移除元素(双指针法原地填入)

给你一个数组` nums` 和一个值` val`，你需要 原地 移除所有数值等于` val `的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 原地 **修改输入数组。**

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```c
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

**示例 1**：

>输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。

**示例 2：**

> 输入：nums = [0,1,2,2,3,0,4,2], val = 2
> 输出：5, nums = [0,1,4,0,3]
> 解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。

**提示：**

> 0 <= nums.length <= 100
> 0 <= nums[i] <= 50
> 0 <= val <= 100

<font color = blue>方法一：暴力解法</font>

```c
int removeElement(int* nums, int numsSize, int val){
    for (int i = 0; i < numsSize; )
    {
        if (nums[i] == val)
        {
            for (int j = i; j < numsSize - 1; j++)
            {
                nums[j] = nums[j + 1];
            }
            numsSize--;
        }
        else
            i++;
    }
    return numsSize;
}
```

<font color = blue > 方法二：双指针法</font>

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int length = nums.length;
        int i, j;
        for (i = 0, j = 0; i < length; i++){ //j记录新数组，i用来遍历
            if (nums[i] != val){
                nums[j++] = nums[i];
            }
        }
        return j;
    }
}
```

优化：可以把等于`val`的元素用最后的元素替换，如果不相等就自增，否则再替换

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length;
        while (left < right) { //注意条件要满足把所有内容都遍历一遍
            if (nums[left] == val) {//相等就把最后的换到前面去
                nums[left] = nums[right - 1];
                right--;
            } else {//不相等就不管
                left++;
            }
        }
        return left;
    }
}

```

## 11. 实现strStr()(KMP算法)

实现` strStr() `函数。

给你两个字符串`haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  `-1` 。

**说明：**

当` needle `是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与 C 语言的` strstr() `以及 Java 的 `indexOf()` 定义相符。

**示例 1：**

> 输入：haystack = "hello", needle = "ll"
> 输出：2

**示例 2：**

>输入：haystack = "aaaaa", needle = "bba"
输出：-1

**示例 3：**

> 输入：haystack = "", needle = ""
> 输出：0

**提示：**

* 0 <= haystack.length, needle.length <= 5 * 104
* haystack 和 needle 仅由小写英文字符组成

<font color = blue >方法一：暴力解法</font>

```c
int strStr(char * haystack, char * needle){
    int m = strlen(needle);
    int n = strlen(haystack);

    for (int i = 0; i + m <= n; i++) //i位置加m的长小于等于n的长时循环
    {
        int flag = 1;
        for (int j = 0; j < m; j++) //对m进行遍历
        {
            if (haystack[i + j] != needle[j]) //不匹配，跳出进入下一轮循环
            {
                flag = 0;
                break;
            }
        }
        if (flag) //如果匹配，返回正确位置
        {
            return i;
        }
    }
    return -1;
}
```

<font color = blue>方法二：KMP算法：</font>

$\text{Knuth-Morris-Pratt} $算法，简称 $\text{KMP}KMP $算法，由 $\text{Donald Knuth}、\text{James H. Morris}$和 $\text{Vaughan Pratt} $三人于 19771977 年联合发表。

$\text{Knuth-Morris-Pratt}$算法的核心为前缀函数，记作 $\pi(i)π(i)$，其定义如下：

对于长度为 `m` 的字符串` s`，其前缀函数$ \pi(i)(0 \leq i < m) $表示 `s` 的子串` s[0:i]`的最长的**相等的真前缀与真后缀**的长度。特别地，如果不存在符合条件的前后缀，那么$ \pi(i) = 0$。<font color  = blue>其中真前缀与真后缀的定义为不等于自身的的前缀与后缀。</font>

我们举个例子说明：字符串 `aabaaab`的前缀函数值依次为 `0,1,0,1,2$,2,3`。

* $\pi(0) = 0$，因为 `a` 没有真前缀和真后缀，根据规定为 `0`（可以发现对于任意字符串 $\pi(0)=0$ 必定成立）；
* $\pi(1) = 1$，因为 `aa` 最长的一对相等的真前后缀为 `a`，长度为 1；
* $\pi(2) = 0$，因为 `aab` 没有对应真前缀和真后缀，根据规定为 0；
* $\pi(3) = 1$，因为 `aaba` 最长的一对相等的真前后缀为 `a`，长度为 1；
* $\pi(4) = 2$，因为 `aabaa` 最长的一对相等的真前后缀为 `aa`，长度为 2；
* $\pi(5) = 2$，因为 `aabaaa` 最长的一对相等的真前后缀为 `aa`，长度为 2；
* $\pi(6) = 3$，因为 `aabaaab` 最长的一对相等的真前后缀为 `aab`，长度为 3。

> $\pi(i)(0 \leq i < m)$，代表子串中最长的相等前后缀，当然前后缀可以用到相同位置的字母。

## 反转字符串：

```c
/*交换方法*/
void reverseString(char *s, int sSize)
{
    for (int left = 0, right = sSize - 1; left < right; left++, right--)
    {
        swap(s, left, right);
    }
}
void swap(char *s, int left, int right)
{
    int t = s[left];
    s[left] = s[right];
    s[right] = t;
}
```

```c
/*尾递归方法*/
void reverseString(char *s, int sSize)
{
    help(s, 0, sSize - 1);
}
void help(char* s, int left, int right)
{
    if (s == NULL || left > right)
        return ;
    swap(s, left, right);
    help(s, left + 1, right - 1);
}
void swap(char* s, int left, int right)
{
    char temp = s[left];
    s[left] = s[right];
    s[right] = temp;
}
```

![image-20201213193745812](https://raw.githubusercontent.com/little-zhengjj/PicGo/master/img/image-20201213193745812.png)

## [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

难度中等759

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

**示例 1：**

![img](https://raw.githubusercontent.com/little-zhengjj/PicGo/master/img/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* swapPairs(struct ListNode* head){
    if (head == NULL || head->next == NULL)
        return head;
    struct ListNode* newList = head->next;
    head->next = swapPairs(head->next->next);
    newList->next = head;
    return newList;
}
```

1. 找到终止条件
2. 相信递归
3. 找到最后的问题解决

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

struct ListNode* swapPairs(struct ListNode* head){
    struct ListNode Head;
    Head.next = head;
    struct ListNode* temp = &Head;
    while(temp->next != NULL && temp->next->next != NULL)
    {
        struct ListNode* before = temp->next;
        temp->next = before->next;
        before->next = before->next->next;
        temp->next->next = before;
        temp = before;
    }
    return Head.next;
}
/*
struct ListNode* swapPairs(struct ListNode* head) {
    struct ListNode dummyHead;
    dummyHead.next = head;
    struct ListNode* temp = &dummyHead;
    while (temp->next != NULL && temp->next->next != NULL) {
        struct ListNode* node1 = temp->next;
        struct ListNode* node2 = temp->next->next;
        temp->next = node2;
        node1->next = node2->next;
        node2->next = node1;
        temp = node1;
    }
    return dummyHead.next;
}
*/
```

## 给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

在杨辉三角中，每个数是它左上方和右上方的数的和。

示例:

```
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

```c
int** generate(int numRows, int* returnSize, int** returnColumnSizes) 
{
    int** p = (int**)malloc(sizeof(int*) * numRows);
    *returnSize = numRows;
    *returnColumnSizes = (int*)malloc(sizeof(int) * numRows);
    for (int i = 0; i < numRows; i++)
    {
        p[i] = (int*)malloc(sizeof(int) * (i + 1));
        returnColumnSizes[0][i] = i + 1;
        p[i][0] = p[i][i] = 1;
        for (int j = 1; j < i; j++)
        {
            p[i][j] = p[i - 1][j - 1] + p[i - 1][j];
        }
    }
    return p;
}
```



# 链表

## 奇偶链表

> 给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。
>
> 请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。示例 1:输入: 1->2->3->4->5->NULL

> 输出: 1->3->5->2->4->NULL

> 示例 2:

> 输入: 2->1->3->5->6->4->7->NULL

> 输出: 2->3->6->7->1->5->4->NULL

> 说明:应当保持奇数节点和偶数节点的相对顺序。

> 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。

```c
struct ListNode* oddEvenList(struct ListNode* head)
{
    if (!head || !head->next)
        return head;
    struct ListNode *record = head;//奇表遍历
    struct ListNode *head2 = head->next;//偶表头
    struct ListNode *record2 = head2;//偶表遍历

    while (record2 && record2->next)//左值不能为空
    {
        record->next = record2->next; //奇是偶的下一个
        record = record->next;//前移
        record2->next = record->next;//偶是奇的下一个
        record2 =record2->next;//前移
    }
    record->next = head2;
    return head;
}
```

## 解法

将奇节点放在一个链表里，偶链表放在另一个链表里。然后把偶链表接在奇链表的尾部。

算法

这个解法非常符合直觉思路也很简单。但是要写一个精确且没有 bug 的代码还是需要进行一番思索的。



一个 LinkedList 需要一个头指针和一个尾指针来支持双端操作。我们用变量 head 和 odd 保存奇链表的头和尾指针。 evenHead 和 even 保存偶链表的头和尾指针。算法会遍历原链表一次并把奇节点放到奇链表里去、偶节点放到偶链表里去。遍历整个链表我们至少需要一个指针作为迭代器。这里 odd 指针和 even 指针不仅仅是尾指针，也可以扮演原链表迭代器的角色。



解决链表问题最好的办法是在脑中或者纸上把链表画出来。比方说：

![img](https://cdn.nlark.com/yuque/0/2020/png/1518861/1592733295054-59062f99-dd21-4464-b17a-8bcbcabbc924.png)



图片 1. 奇偶链表的例子



```c
public class Solution {
   public ListNode oddEvenList(ListNode head) {
       if (head == null) return null;
       ListNode odd = head, even = head.next, evenHead = even;
       while (even != null && even.next != null) {
           odd.next = even.next;
           odd = odd.next;
           even.next = odd.next;
           even = even.next;
       }
       odd.next = evenHead;
       return head;
   }
}
```



复杂度分析

时间复杂度： O(n)O(n) 。总共有 nn 个节点，我们每个遍历一次。

空间复杂度： O(1)O(1) 。我们只需要 4 个指针。

# 初级算法

## 数组

### 删除数组中的重复项

**给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。**

说明:

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

```java
你可以想象内部操作如下:

// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

示例 1：

> 输入：nums = [1,1,2]
> 输出：2, nums = [1,2]
> 解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。

示例 2：

> 输入：nums = [0,0,1,1,1,2,2,3,3,4]
> 输出：5, nums = [0,1,2,3,4]
> 解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。


提示：

> 0 <= nums.length <= 3 * 104
> -104 <= nums[i] <= 104
> nums 已按升序排列

```c
int removeDuplicates(int* nums, int numsSize){
    if (numsSize <= 1)
        return numsSize;
    int fast = 1;
    int slow = 1;
    while (fast < numsSize)
    {
        if (nums[fast] != nums[fast - 1])
        {
            nums[slow++] = nums[fast];
        }
        fast++;
    }
    return slow;
}
```

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int length = nums.length;
        if (length == 0)
            return length;
        int fast = 1;
        int slow = 1; //这里慢指针就是数组数据的个数
        while (fast < length)
        {
            if (nums[fast] != nums[fast - 1])
            {
                nums[slow++] = nums[fast];
            }
            fast++;
        }
        return slow;
    }
}
```

### 旋转数组

**给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。**

进阶：

> 尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
> 你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？


示例 1:

> 输入: nums = [1,2,3,4,5,6,7], k = 3
> 输出: [5,6,7,1,2,3,4]
> 解释:
> 向右旋转 1 步: [7,1,2,3,4,5,6]
> 向右旋转 2 步: [6,7,1,2,3,4,5]
> 向右旋转 3 步: [5,6,7,1,2,3,4]

示例 2:

> 输入：nums = [-1,-100,3,99], k = 2
> 输出：[3,99,-1,-100]
> 解释: 
> 向右旋转 1 步: [99,-1,-100,3]
> 向右旋转 2 步: [3,99,-1,-100]


提示：

> 1 <= nums.length <= 2 * 104
> -231 <= nums[i] <= 231 - 1
> 0 <= k <= 105

多次反转法：

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int length = nums.length;
        k %= length;
        if (k == 0)
        {
            return ;
        }
        reverse(nums, 0, length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, length - 1);
    }
    public void reverse(int[] nums, int left, int right)
    {
        while (left < right)
        {
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
            left++;
            right--;
        }
    }
}
```

数组拷贝法：

```c
void rotate(int* nums, int numsSize, int k){
    k %= numsSize;
    if (k == 0)
        return;
    
    int *newNums = (int *)malloc(numsSize * sizeof(int));
    //拷贝数组
    for (int i = 0; i < numsSize; i++)
    {
        *(newNums + i) = nums[i];
    }
    int i;
    for (i = 0; i < k; i++)
    {
        nums[i] = *(newNums + numsSize - k + i);
    }
    for (int j = 0; j < numsSize - k; j++)
    {
        nums[i++] = *(newNums + j);
    }
    free(newNums);
}
```

循环数组法：

有点麻烦， 不想写了，记得考虑碰到一起的情况。

