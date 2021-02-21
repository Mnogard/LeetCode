# 剑指Offer

## 1.数据结构

### 1.1 数组

#### <span style="color:green;">剑指Offer 03. 数组中重复的数字</span>

**找出数组中重复的数字**


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

**示例 1：**

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

**限制：**

`2 <= n <= 100000`

---

**方法一：遍历数组**

由于只需要找出数组中任意一个重复的数字，因此遍历数组，遇到重复的数字即返回。为了判断一个数字是否重复遇到，使用集合存储已经遇到的数字，如果遇到的一个数字已经在集合中，则当前的数字是重复数字。

+ 初始化集合为空集合，重复的数字 `repeat = -1`
+ 遍历数组中的每个元素：
  + 将该元素加入集合中，判断是否添加成功
    + 如果添加失败，说明该元素已经在集合中，因此该元素是重复元素，将该元素的值赋给 `repeat`，并结束遍历
+ 返回 repeat

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Set<Integer> set = new HashSet<Integer>();
        int repeat = -1;
        for (int num : nums) {
            if (!set.add(num)) {
                repeat = num;
                break;
            }
        }
        return repeat;
    }
}
```

**复杂性分析**

+ 时间复杂度：O(n)。
  + 遍历数组一遍。使用哈希集合（HashSet），添加元素的时间复杂度为 O(1)，故总的时间复杂度是 O(n)。
+ 空间复杂度：O(n)。不重复的每个元素都可能存入集合，因此占用 O(n) 额外空间。

---

### 1.2 字符串

#### <span style="color:green;">剑指 Offer 05. 替换空格</span>

**请实现一个函数，把字符串 s 中的每个空格替换成"%20"**

**示例 1：**

```
输入：s = "We are happy."
输出："We%20are%20happy."
```


限制：

`0 <= s 的长度 <= 10000`

---

**方法一：字符数组**

由于每次替换从 1 个字符变成 3 个字符，使用字符数组可方便地进行替换。建立字符数组地长度为 s 的长度的 3 倍，这样可保证字符数组可以容纳所有替换后的字符。

```java
class Solution {
    public String replaceSpace(String s) {
        int length = s.length();
        char[] array = new char[length * 3];
        int size = 0;
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            if (c == ' ') {
                array[size++] = '%';
                array[size++] = '2';
                array[size++] = '0';
            } else {
                array[size++] = c;
            }
        }
        String newStr = new String(array, 0, size);
        return newStr;
    }
}
```

**复杂性分析**

- 时间复杂度：O(n)。遍历字符串 `s` 一遍。
- 空间复杂度：O(n)。额外创建字符数组，长度为 `s` 的长度的 3 倍。

---

### 1.3 链表

#### <span style="color:green;">剑指 Offer 06. 从尾到头打印链表</span>

**输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）**

**示例 1：**

```
输入：head = [1,3,2]
输出：[2,3,1]
```

**限制：**

`0 <= 链表长度 <= 10000`

---

**方法一：栈**

栈的特点是后进先出，即最后压入栈的元素最先弹出。考虑到栈的这一特点，使用栈将链表元素顺序倒置。从链表的头节点开始，依次将每个节点压入栈内，然后依次弹出栈内的元素并存储到数组中。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public int[] reversePrint(ListNode head) {
        Stack<ListNode> stack = new Stack<ListNode>();
        ListNode temp = head;
        while (temp != null) {
            stack.push(temp);
            temp = temp.next;
        }
        int size = stack.size();
        int[] print = new int[size];
        for (int i = 0; i < size; i++) {
            print[i] = stack.pop().val;
        }
        return print;
    }
}
```

**复杂性分析**

+ 时间复杂度：O(n)。正向遍历一遍链表，然后从栈弹出全部节点，等于又反向遍历一遍链表。
+ 空间复杂度：O(n)。额外使用一个栈存储链表中的每个节点。

---

### 1.4 树

---

### 1.5 栈和队列

#### <span style="color:green;">剑指 Offer 09. 用两个栈实现队列</span>

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 `appendTail` 和 `deleteHead` ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，`deleteHead` 操作返回 `-1` )

**示例 1：**


```
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
```

**示例 2：**


```
输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```

提示：

`1 <= values <= 10000`
`最多会对 appendTail、deleteHead 进行 10000 次调用`

---

**方法一：双栈**

**思路和算法**

维护两个栈，第一个栈支持插入操作，第二个栈支持删除操作。

根据栈先进后出的特性，我们每次往第一个栈里插入元素后，第一个栈的底部元素是最后插入的元素，第一个栈的顶部元素是下一个待删除的元素。为了维护队列先进先出的特性，我们引入第二个栈，用第二个栈维护待删除的元素，在执行删除操作的时候我们首先看下第二个栈是否为空。如果为空，我们将第一个栈里的元素一个个弹出插入到第二个栈里，这样第二个栈里元素的顺序就是待删除的元素的顺序，要执行删除操作的时候我们直接弹出第二个栈的元素返回即可。

<img src="https://assets.leetcode-cn.com/solution-static/jianzhi_09/jianzhi_9.gif" alt="jianzhi_9" style="zoom: 50%;" />

```java
class CQueue {
    Deque<Integer> stack1;
    Deque<Integer> stack2;
    
    public CQueue() {
        stack1 = new LinkedList<Integer>();
        stack2 = new LinkedList<Integer>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
        // 如果第二个栈为空
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        } 
        if (stack2.isEmpty()) {
            return -1;
        } else {
            int deleteItem = stack2.pop();
            return deleteItem;
        }
    }
}
```

**复杂度分析**

+ 时间复杂度：对于插入和删除操作，时间复杂度均为 O(1)。插入不多说，对于删除操作，虽然看起来是 O(n) 的时间复杂度，但是仔细考虑下每个元素只会「至多被插入和弹出 stack2 一次」，因此均摊下来每个元素被删除的时间复杂度仍为 O(1)。

+ 空间复杂度：O(n)。需要使用两个栈存储已有的元素。

---

## 2 算法和数据操作

### 2.1 递归和循环

#### <span style="color:green;">剑指 Offer 10- I. 斐波那契数列</span>

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
```


斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。 

**示例 1：**

```
输入：n = 2
输出：1
```

**示例 2：**

```
输入：n = 5
输出：5
```

**提示：**

`0 <= n <= 100`

**代码：**

```java
class Solution {
    public int fib(int n) {
        int a = 0, b = 1;
        int sum = 0;
        int[] result = {0, 1};
        if(n < 2) {
            return result[n];
        }else{
            for(int i = 0; i < n-1; i++) {
            sum = (a + b)%1000000007;
            a = b;
            b = sum;
            }
            return sum;
        }    
    }
}
```

**复杂度分析：**

+ 时间复杂度 O(N)： 计算 f(n)需循环 n 次，每轮循环内计算操作使用 O(1) 。
+ 空间复杂度 O(1) ： 几个标志变量使用常数大小的额外空间。

---

#### <span style="color:green;">剑指 Offer 10- II. 青蛙跳台阶问题</span>

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

**示例 1：**

```
输入：n = 2
输出：2
```

**示例 2：**

```
输入：n = 7
输出：21
```

**示例 3：**

```
输入：n = 0
输出：1
```

**提示：**

`0 <= n <= 100`

**代码：**

```java
class Solution {
    public int numWays(int n) {
        int a = 1, b = 1;
        int sum = 0;
        int[] result = {1, 1};
        if(n < 2) {
            return result[n];
        }else{
            for(int i = 0; i < n-1; i++) {
            sum = (a + b)%1000000007;
            a = b;
            b = sum;
            }
            return sum;
        }    
    }
}
```

---

### 2.2 查找和排序

#### <span style="color:green;">剑指 Offer 11. 旋转数组的最小数字</span>

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

**示例 1：**

```
输入：[3,4,5,1,2]
输出：1
```


**示例 2：**

```
输入：[2,2,2,0,1]
输出：0
```

**代码：**

```java
//二分查找
class Solution {
    public int minArray(int[] numbers) {
        int i = 0, j = numbers.length - 1;
        while (i < j) {
            int m = (i + j) / 2;
            if (numbers[m] > numbers[j]) i = m + 1;
            else if (numbers[m] < numbers[j]) j = m;
            else j--;
        }
        return numbers[i];
    }
}
```

**复杂度分析：**

+ 时间复杂度 O(\log_2 N) :  在特例情况下(例如[1,1,1,1),会退化到O(N)。
+ 空间复杂度 O(1) :   i, j, m变量使用常数大小的额外空间。

### 2.3 回溯法

### 2.4 动态规划 & 贪婪算法

### 2.5 位运算

#### <span style="color:green;">剑指 Offer 15. 二进制中1的个数</span>

请实现一个函数，输入一个整数（以二进制串形式），输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

**示例 1：**

```
输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```


**示例 2：**

```
输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
```


**示例 3：**

```
输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
```

**提示：**

输入必须是长度为 32 的 二进制串 。

---

**方法一：逐位判断**

```java
public class Solution {
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res += n & 1;
            n >>>= 1;
        }
        return res;
    }
}
```

##### 复杂度分析：

+ 时间复杂度  O(log2 n):  此算法循环内部仅有移位、与、加等基本运算,占用O(1);逐位判断需循 环log2n次,其中log2n代表数字n最高位1的所在位数(例如log24=2,log216=4)。

+ 空间复杂度  O(1):  变量res使用常数大小额外空间。

---

**方法二：巧用 n \& (n - 1)*n*&(*n*−1)**

```java
public class Solution {
    public int hammingWeight(int n) {
        int res = 0;
        while(n != 0) {
            res++;
            n &= n - 1;
        }
        return res;
    }
}
```

**复杂度分析：**

+ 时间复杂度 O(M) ： n \& (n - 1)n&(n−1) 操作仅有减法和与运算，占用 O(1) ；设 MM 为二进制数字 n 中 11 的个数，则需循环 M 次（每轮消去一个 11 ），占用 O(M) 。
+ 空间复杂度 O(1) ： 变量 resres 使用常数大小额外空间。

---

## 3 高质量代码

### 3.1 代码的完整性

#### <span style="color:green;">剑指 Offer 17. 打印从1到最大的n位数</span>

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

**示例 1:**

```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

**说明：**

`用返回一个整数列表来代替打印`
`n 为正整数`

**代码：**

```java
class Solution {
    public int[] printNumbers(int n) {
        int end = (int)Math.pow(10, n) - 1;
        int[] res = new int[end];
        for(int i = 0; i < end; i++) {
            res[i] = i + 1;
        }
        return res;
    }
}
```

**复杂度分析：**

+ 时间复杂度 O(10^n)： 生成长度为 10^n  的列表需使用 O(10^n) 时间。
+ 空间复杂度 O(1) ： 建立列表需使用 O(1) 大小的额外空间（ 列表作为返回结果，不计入额外空间 ）。

---

#### <span style="color:green;">剑指 Offer 18. 删除链表的节点</span>

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

**注意：**此题对比原题有改动

**示例 1:**

```
输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```


**示例 2:**

```
输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
```

**说明：**

`题目保证链表中节点的值互不相同`
`若使用 C 或 C++ 语言，你不需要 free 或 delete 被删除的节点`

**代码：**

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head.val == val) return head.next;//链表只有一个元素
        ListNode p = head;//使用单指针

        while(p.next.val != val && p.next != null){//遍历单链表，查找下一个节点等于目标节点的节点，同时防止空指针
            p = p.next;
        }

        if(p.next != null){//如果是空指针，直接返回head，若非空，执行删除操作（覆盖目标节点）
            p.next = p.next.next;
        }
        return head;

    }
}
```

---

#### <span style="color:green;">剑指 Offer 21. 调整数组顺序使奇数位于偶数前面</span>

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

**示例：**

```
输入：nums = [1,2,3,4]
输出：[1,3,2,4] 
注：[3,1,2,4] 也是正确的答案之一。
```

**提示：**

`0 <= nums.length <= 50000`
`1 <= nums[i] <= 10000`

---

**方法一 :（My）**

**代码：**

```java
class Solution {
    public int[] exchange(int[] nums) {
        int length = nums.length;
        int[] temp = new int[length];
        int head = 0, tail = length - 1;
        for (int i = 0; i < length; i++) {
            if(nums[i] % 2 == 1) {
                temp[head] = nums[i];
                head++;
            }else{
                temp[tail] = nums[i];
                tail--;
            }       
        }
        return temp;
    }
}
```

---

**方法二：**

```java
class Solution {
    public int[] exchange(int[] nums) {
        int i = 0, j = nums.length - 1, tmp;
        while(i < j) {
            while(i < j && (nums[i] & 1) == 1) i++;
            while(i < j && (nums[j] & 1) == 0) j--;
            tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;
        }
        return nums;
    }
}
```

**复杂度分析：**

+ 时间复杂度 O(N) ： N 为数组 numsnums 长度，双指针 i, j 共同遍历整个数组。
+ 空间复杂度 O(1)： 双指针 i, j 使用常数大小的额外空间。

---

### 3.2 代码的鲁棒性

#### <span style="color:green;">剑指 Offer 22. 链表中倒数第k个节点</span>

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。 

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

**代码：**

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode former = head, latter = head;
        for(int i = 0; i < k; i++) {
            former = former.next;
        }
        while(former != null) {
            former = former.next;
            latter = latter.next;
        }
        return latter;
    }
}
```

---

#### <span style="color:green;">剑指 Offer 24. 反转链表</span>

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

**限制：**

`0 <= 节点个数 <= 5000`

**代码：**

**方法一（迭代）：**

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

**方法二（递归）⚠️：**

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode newHead = reverseList(head.next);  //newHead = return head 也就是最后一个点的地址，将保持不变
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

---

#### <span style="color:green;">剑指 Offer 25. 合并两个排序的链表</span>

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

**示例1：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

**限制：**

`0 <= 链表长度 <= 1000`

**代码：**

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dum = new ListNode(0), cur = dum;
        while(l1 != null && l2 != null) {
            if(l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            }
            else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }
        cur.next = l1 != null ? l1 : l2;
        return dum.next;
    }
}
```

**复杂度分析：**

+ 时间复杂度O(M+N):  M,N分别为链表1,12的长度,合并操作需遍历两链表。

+ 空间复杂度O(1):  节点引用dam,cur使用常数大小的额外空间。

---

## 4 解决面试题的思路

### 4.1 画图让问题形象化

#### <span style="color:green;">剑指 Offer 27. 二叉树的镜像</span>

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

**例如输入：**

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

**镜像输出：**

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**示例 1：**

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**代码（递归法）：**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if(root == null) return null;
        TreeNode tmp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(tmp);
        return root;
    }
}
```

---

#### <span style="color:green;">剑指 Offer 28. 对称的二叉树</span>

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

**例如，二叉树 [1,2,2,3,4,4,3] 是对称的。**

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

**但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:**

```
    1
   / \
  2   2
   \   \
   3    3
```

**示例 1：**

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**限制：**

`0 <= 节点个数 <= 1000`

**代码（递归）：**

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null ? true : recur(root.left, root.right);
    }
    boolean recur(TreeNode L, TreeNode R) {
        if(L == null && R == null) return true;
        if(L == null || R == null || L.val != R.val) return false;
        return recur(L.left, R.right) && recur(L.right, R.left);
    }
}
```

---

#### <span style="color:green;">剑指 Offer 29. 顺时针打印矩阵</span>

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

**示例 1：**

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

**限制：**

`0 <= matrix.length <= 100`
`0 <= matrix[i].length <= 100`

**代码：**

```java
class Solution {
    public int[] spiralOrder(int[][] matrix) {
        if(matrix.length == 0) return new int[0];
        int l = 0, r = matrix[0].length -1, t = 0, b = matrix.length - 1, x = 0;
        int[] res = new int[(r + 1) * (b + 1)];
        while(true) {
            for(int i = l; i <= r; i++) res[x++] = matrix[t][i];
            if(++t > b) break;
            for(int i = t; i <= b; i++) res[x++] = matrix[i][r];
            if(--r < l) break;
            for(int i = r; i >= l; i--) res[x++] = matrix[b][i];
            if(--b < t) break;
            for(int i = b; i >= t; i--) res[x++] = matrix[i][l];
            if(++l > r) break;
        }
        return res;
    }
}
```

---

### 4.2 举例让抽象问题具体化

#### <span style="color:green;">剑指 Offer 30. 包含min函数的栈</span>

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

**示例:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

**提示：**

`各函数的调用总次数不超过 20000 次`

**代码：**

```java
class MinStack {
    Stack<Integer> A, B;
    public MinStack() {
        A = new Stack<>();
        B = new Stack<>();
    }
    public void push(int x) {
        A.add(x);
        if(B.empty() || B.peek() >= x)
            B.add(x);
    }
    public void pop() {
        if(A.pop().equals(B.peek()))
            B.pop();
    }
    public int top() {
        return A.peek();
    }
    public int min() {
        return B.peek();
    }
}
```

---

#### <span style="color:green;">剑指 Offer 32 - II. 从上到下打印二叉树 II</span>

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

**例如:**
给定二叉树: [3,9,20,null,null,15,7],

```
3

   / \
  9  20
    /  \
   15   7
```

**返回其层次遍历结果：**

```
[
  [3],
  [9,20],
  [15,7]
]
```

**提示：**

`节点总数 <= 1000`

**代码：**

```java

```

---

### 4.3 分解让复杂问题简单化



## 5 优化时间和空间效率

### 5.1 时间效率

#### <span style="color:green;">剑指 Offer 39. 数组中出现次数超过一半的数字</span>

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。 

你可以假设数组是非空的，并且给定的数组总是存在多数元素。 

**示例 1:**

```
输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
```

**限制：**

`1 <= 数组长度 <= 50000`

**代码：**

```java
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0, length = nums.length;
        for(int i = 0; i < length; i++) {
            if(votes == 0) x = nums[i];
            votes += x == nums[i] ? 1 : -1;
        }
        return x;
    }
}
```

```java
//增强for(验证是否为存在超过一半的数)
class Solution {
    public int majorityElement(int[] nums) {
        int x = 0, votes = 0;
        for(int num : nums) {
            if(votes == 0) x = num;
            votes += x == num ? 1 : -1;
        }
        // 验证 x 是否为众数
        for(int num : nums)
            if(num == x) count++;
        return count > nums.length / 2 ? x : 0; // 当无众数时返回 0
    }
}

```

---

#### <span style="color:green;">剑指 Offer 40. 最小的k个数</span>

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。 

**示例 1：**

```
输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
```


**示例 2：**

```
输入：arr = [0,1,2,1], k = 1
输出：[0]
```

**限制：**

`0 <= k <= arr.length <= 10000`
`0 <= arr[i] <= 10000`

代码：

```java
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        int[] vec = new int[k];
        Arrays.sort(arr);
        for(int i = 0; i < k; i++) {
            vec[i] = arr[i];
        }
        return vec;
    }
}
```

