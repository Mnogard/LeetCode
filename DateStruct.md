# 数据结构

> 来源：力扣（LeetCode）

## 1.链表

### 1.1 单链表

+ **定义**

<img src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/12/screen-shot-2018-04-12-at-152754.png" alt="img" style="zoom:33%;" />

`Java实现`

```java
// Definition for singly-linked list.
public class SinglyListNode {
    int val;
    SinglyListNode next;
    SinglyListNode(int x) { val = x; }
}
```

`C++实现`

```c++
// Definition for singly-linked list.
struct SinglyListNode {
    int val;
    SinglyListNode *next;
    SinglyListNode(int x) : val(x), next(NULL) {}
};
```

在大多数情况下，我们将使用头结点(第一个结点)来表示整个列表。

+ 添加操作

如果我们想在给定的结点 `prev` 之后添加新值，我们应该：

1. 使用给定值初始化新结点 `cur`

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/08/05/screen-shot-2018-04-25-at-163224.png" alt="img" style="zoom:33%;" />

2. 将 `cur` 的 `next` 字段链接到 `prev` 的下一个结点 `next` 

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/26/screen-shot-2018-04-25-at-163234.png" alt="img" style="zoom:33%;" />

3. 将 `prev` 中的 `next` 字段链接到 `cur` 

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/26/screen-shot-2018-04-25-at-163243.png" alt="img" style="zoom: 33%;" />

与数组不同，我们不需要将所有元素移动到插入元素之后。因此，您可以在 `O(1)` 时间复杂度中将新结点插入到链表中，这非常高效

+ 删除操作

如果我们想从单链表中删除现有结点 `cur`，可以分两步完成：

1. 找到 cur 的上一个结点 `prev` 及其下一个结点 `next` 

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/27/screen-shot-2018-04-26-at-203558.png" alt="img" style="zoom:33%;" />

2. 接下来链接 `prev` 到 cur 的下一个节点 `next` 

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/26/screen-shot-2018-04-26-at-203640.png" alt="img" style="zoom:33%;" />

在我们的第一步中，我们需要找出 `prev` 和 `next`。使用 `cur` 的参考字段很容易找出 `next`，但是，我们必须从头结点遍历链表，以找出 `prev`，它的平均时间是 `O(N)`，其中 `N` 是链表的长度。因此，删除结点的时间复杂度将是 `O(N)`

空间复杂度为 `O(1)`，因为我们只需要常量空间来存储指针。

+ 设计单链表 `java`

<img src="https://pic.leetcode-cn.com/b3221337c286323c36ce9b991b6248c3ad8e93bce90fb261af9a687f16d02933-file_1578973150799" alt="在这里插入图片描述" style="zoom:33%;" />

```java
class MyLinkedList {
  int size;
  ListNode head;  // sentinel node as pseudo-head
  public MyLinkedList() {
    size = 0;
    head = new ListNode(0);
  }
}
```

哨兵节点被用作伪头始终存在，这样结构中永远不为空，它将至少包含一个伪头。`MyLinkedList` 中所有节点均包含：值 + 链接到下一个元素的指针。

```java
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) { val = x; }
}
```

`addAtIndex`，`addAtHead` 和 `addAtTail`：
我们首先讨论 `addAtIndex`，因为伪头的关系 `addAtHead` 和 addAtTail 可以使用 `addAtIndex` 来完成。

这个想法很简单：

> 找到要插入位置节点的前驱节点。如果要在头部插入，则它的前驱节点就是伪头。如果要在尾部插入节点，则前驱节点就是尾节点。
> 通过改变 `next` 来插入节点。

```java
toAdd.next = pred.next;
pred.next = toAdd;
```

<img src="https://pic.leetcode-cn.com/b7d03b863918800496ee9e58758a654e2e05569a27c798251ae95e65edf9b402-file_1578973150851" alt="在这里插入图片描述" style="zoom:33%;" />

<img src="https://pic.leetcode-cn.com/34ea9b5645a5ec2f31eff8766d50417c2d5c8b72d638ac36d4ca123d6fb68729-file_1578973150845" alt="在这里插入图片描述" style="zoom:33%;" />

`deleteAtIndex`：
和插入同样的道理。

1. 找到要删除节点的前驱节点。

2. 通过改变 `next` 来删除节点。

```java
// delete pred.next 
pred.next = pred.next.next;
```

<img src="https://pic.leetcode-cn.com/72fe13e2fc3ed8358180b5101af1cbe7fb2126dc9b0801c6fd30f90dc60aca41-file_1578973150923" alt="在这里插入图片描述" style="zoom:33%;" />

`get`：
从伪头节点开始，向前走 `index+1` 步。

```java
// index steps needed 
// to move from sentinel node to wanted index
for(int i = 0; i < index + 1; ++i) curr = curr.next;
return curr.val;
```

<img src="https://pic.leetcode-cn.com/f4df3682e14bbd9fd24edd58337ed727503f559badc39b7b80aec88ec6909233-file_1578973150859" alt="在这里插入图片描述" style="zoom:33%;" />

**全部代码：**

```java
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) { val = x; }
}

class MyLinkedList {
  int size;
  ListNode head;  // sentinel node as pseudo-head
  public MyLinkedList() {
    size = 0;
    head = new ListNode(0);
  }

  /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
  public int get(int index) {
    // if index is invalid
    if (index < 0 || index >= size) return -1;

    ListNode curr = head;
    // index steps needed 
    // to move from sentinel node to wanted index
    for(int i = 0; i < index + 1; ++i) curr = curr.next;
    return curr.val;
  }

  /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
  public void addAtHead(int val) {
    addAtIndex(0, val);
  }

  /** Append a node of value val to the last element of the linked list. */
  public void addAtTail(int val) {
    addAtIndex(size, val);
  }

  /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
  public void addAtIndex(int index, int val) {
    // If index is greater than the length, 
    // the node will not be inserted.
    if (index > size) return;

    // [so weird] If index is negative, 
    // the node will be inserted at the head of the list.
    if (index < 0) index = 0;

    ++size;
    // find predecessor of the node to be added
    ListNode pred = head;
    for(int i = 0; i < index; ++i) pred = pred.next;

    // node to be added
    ListNode toAdd = new ListNode(val);
    // insertion itself
    toAdd.next = pred.next;
    pred.next = toAdd;
  }

  /** Delete the index-th node in the linked list, if the index is valid. */
  public void deleteAtIndex(int index) {
    // if the index is invalid, do nothing
    if (index < 0 || index >= size) return;

    size--;
    // find predecessor of the node to be deleted
    ListNode pred = head;
    for(int i = 0; i < index; ++i) pred = pred.next;

    // delete pred.next 
    pred.next = pred.next.next;
  }
}
```

**复杂度分析**

1. 时间复杂度：
   + `addAtHead`： *O*(1)
   + `addAtInder`，`get`，`deleteAtIndex`: *O*(k)，其中 k指的是元素的索引。
   + `addAtTail`：*O*(N)，其中 N指的是链表的元素个数。

2. 空间复杂度：所有的操作都是 *O*(1)。

---

### 1.2 环形列表

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 `0` 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。注意：`pos` 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 `true` 。 否则，返回 `false` 。

**方法一：哈希表**

**思路及算法**

最容易想到的方法是遍历所有节点，每次遍历到一个节点时，判断该节点此前是否被访问过。

具体地，我们可以使用哈希表来存储所有已经访问过的节点。每次我们到达一个节点，如果该节点已经存在于哈希表中，则说明该链表是环形链表，否则就将该节点加入哈希表中。重复这一过程，直到我们遍历完整个链表即可。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<ListNode>();
        while (head != null) {
            if (!seen.add(head)) {
                return true;
            }
            head = head.next;
        }
        return false;
    }
}
```

**复杂度分析**

	+  时间复杂度：O(N)，其中 N是链表中的节点数。最坏情况下我们需要遍历每个节点一次。
	+  空间复杂度：O(N)，其中 N是链表中的节点数。主要为哈希表的开销，最坏情况下我们需要将每个节点插入到哈希表中一次。

**方法二：快慢指针**

思路及算法

本方法需要读者对「Floyd 判圈算法」（又称龟兔赛跑算法）有所了解。

假想「乌龟」和「兔子」在链表上移动，「兔子」跑得快，「乌龟」跑得慢。当「乌龟」和「兔子」从链表上的同一个节点开始移动时，如果该链表中没有环，那么「兔子」将一直处于「乌龟」的前方；如果该链表中有环，那么「兔子」会先于「乌龟」进入环，并且一直在环内移动。等到「乌龟」进入环时，由于「兔子」的速度快，它一定会在某个时刻与乌龟相遇，即套了「乌龟」若干圈。

我们可以根据上述思路来解决本题。具体地，我们定义两个指针，一快一满。慢指针每次只移动一步，而快指针每次移动两步。初始时，慢指针在位置 `head`，而快指针在位置 `head.next`。这样一来，如果在移动的过程中，快指针反过来追上慢指针，就说明该链表为环形链表。否则快指针将到达链表尾部，该链表不为环形链表。

<img src="https://assets.leetcode-cn.com/solution-static/141/1.png" alt="img" style="zoom: 25%;" />

**细节**

为什么我们要规定初始时慢指针在位置 `head`，快指针在位置 `head.next`，而不是两个指针都在位置 `head`（即与「乌龟」和「兔子」中的叙述相同）？

+ 观察下面的代码，我们使用的是 `while` 循环，循环条件先于循环体。由于循环条件一定是判断快慢指针是否重合，如果我们将两个指针初始都置于 `head`，那么 `while` 循环就不会执行。因此，我们可以假想一个在 `head` 之前的虚拟节点，慢指针从虚拟节点移动一步到达 `head`，快指针从虚拟节点移动两步到达 `head.next`，这样我们就可以使用 `while` 循环了。
+ 当然，我们也可以使用 `do-while` 循环。此时，我们就可以把快慢指针的初始值都置为 `head`。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

**复杂度分析**

+ 时间复杂度：O(N)，其中 N是链表中的节点数。
  + 当链表中不存在环时，快指针将先于慢指针到达链表尾部，链表中每个节点至多被访问两次。
  + 当链表中存在环时，每一轮移动后，快慢指针的距离将减小一。而初始距离为环的长度，因此至多移动 N轮。
+ 空间复杂度：O(1)。我们只使用了两个指针的额外空间。

### 1.3 双链表

+ **定义**

双链表以类似的方式工作，但`还有一个引用字段`，称为`“prev”`字段。有了这个额外的字段，您就能够知道当前结点的前一个结点。

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/17/screen-shot-2018-04-17-at-161130.png" alt="img" style="zoom:33%;" />

```java
// Definition for doubly-linked list.
class DoublyListNode {
    int val;
    DoublyListNode next, prev;
    DoublyListNode(int x) {val = x;}
}
```

+ **添加操作**

如果我们想在现有的结点 `prev` 之后插入一个新的结点 `cur`，我们可以将此过程分为两个步骤：

1. 链接 `cur` 与 `prev` 和 `next`，其中 `next` 是 `prev` 原始的下一个节点；

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/28/screen-shot-2018-04-28-at-173045.png" alt="img" style="zoom:33%;" />

2. 用 `cur` 重新链接 `prev` 和 `next`。

<img src="https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/04/29/screen-shot-2018-04-28-at-173055.png" alt="img" style="zoom:33%;" />

与单链表类似，添加操作的时间和空间复杂度都是 `O(1)`。

+ **删除操作**

如果我们想从双链表中删除一个现有的结点 `cur`，我们可以简单地将它的前一个结点 `prev` 与下一个结点 `next` 链接起来。

> 与单链表不同，使用“prev”字段可以很容易地在常量时间内获得前一个结点。

因为我们不再需要遍历链表来获取前一个结点，所以时间和空间复杂度都是`O(1)`。

+ **设计双链表**

双链表比单链表快得多，测试用例花费的时间比单链表快了两倍。但是它更加复杂，它包含了 `size`，记录链表元素个数，和伪头伪尾。

<img src="https://pic.leetcode-cn.com/ffb51bf8583d324c68afb3c5b5f9fe0f61f3fa65167d462d6f8549566c3c8f33-file_1578973150884" alt="在这里插入图片描述" style="zoom:33%;" />

```java
class MyLinkedList {
  int size;
  // sentinel nodes as pseudo-head and pseudo-tail
  ListNode head, tail;
  public MyLinkedList() {
    size = 0;
    head = new ListNode(0);
    tail = new ListNode(0);
    head.next = tail;
    tail.prev = head;
  }
}
```

伪头和伪尾总是存在，MyLinkedList 中所有节点都包含：值 + 指向前一个节点的指针 + 指向后一个节点的指针。

```java
public class ListNode {
  int val;
  ListNode next;
  ListNode prev;
  ListNode(int x) { val = x; }
}
```

`addAtIndex`，`addAtHead` 和 `addAtTail`：

+ 找到要插入节点的前驱节点和后继节点。如果要在头部插入节点，则它的前驱结点是伪头。如果要在尾部插入节点，则它的后继节点是伪尾。
+ 通过改变前驱结点和后继节点的链接关系添加元素。

```java
toAdd.prev = pred
toAdd.next = succ
pred.next = toAdd
succ.prev = toAdd
```

<img src="https://pic.leetcode-cn.com/b4e5057ef258b98ea66252cd168cae535419161b28a6d6e5859c405e5585eb1b-file_1578973150914" alt="在这里插入图片描述" style="zoom:33%;" />

`deleteAtIndex`：
和插入同样的道理。

+ 找到要删除节点的前驱结点和后继节点。
+ 通过改变前驱结点和后继节点的链接关系删除元素。

```java
pred.next = succ
succ.prev = pred
```

<img src="https://pic.leetcode-cn.com/323a5bf16db256a4267fb8e379606ab54f73e9f6c95db4980f4fdd5bf4f57a08-file_1578973150887" alt="在这里插入图片描述" style="zoom:33%;" />

`get`：

- 通过比较 `index` 和 `size - index` 的大小判断从头开始较快还是从尾巴开始较快。
- 从较快的方向开始。

```java
// choose the fastest way: to move from the head
// or to move from the tail
ListNode curr = head;
if (index + 1 < size - index)
  for(int i = 0; i < index + 1; ++i) curr = curr.next;
else {
  curr = tail;
  for(int i = 0; i < size - index; ++i) curr = curr.prev;
}
```

<img src="https://pic.leetcode-cn.com/eaefffaaf1f3155f210d6b9a2f4f302b0bb4a1653deea8e63e4313005e4faa60-file_1578973150893" alt="在这里插入图片描述" style="zoom:33%;" />

+ **全部代码**

```java
public class ListNode {
  int val;
  ListNode next;
  ListNode prev;
  ListNode(int x) { val = x; }
}

class MyLinkedList {
  int size;
  // sentinel nodes as pseudo-head and pseudo-tail
  ListNode head, tail;
  public MyLinkedList() {
    size = 0;
    head = new ListNode(0);
    tail = new ListNode(0);
    head.next = tail;
    tail.prev = head;
  }

  /** Get the value of the index-th node in the linked list. If the index is invalid, return -1. */
  public int get(int index) {
    // if index is invalid
    if (index < 0 || index >= size) return -1;

    // choose the fastest way: to move from the head
    // or to move from the tail
    ListNode curr = head;
    if (index + 1 < size - index)
      for(int i = 0; i < index + 1; ++i) curr = curr.next;
    else {
      curr = tail;
      for(int i = 0; i < size - index; ++i) curr = curr.prev;
    }

    return curr.val;
  }

  /** Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. */
  public void addAtHead(int val) {
    ListNode pred = head, succ = head.next;

    ++size;
    ListNode toAdd = new ListNode(val);
    toAdd.prev = pred;
    toAdd.next = succ;
    pred.next = toAdd;
    succ.prev = toAdd;
  }

  /** Append a node of value val to the last element of the linked list. */
  public void addAtTail(int val) {
    ListNode succ = tail, pred = tail.prev;

    ++size;
    ListNode toAdd = new ListNode(val);
    toAdd.prev = pred;
    toAdd.next = succ;
    pred.next = toAdd;
    succ.prev = toAdd;
  }

  /** Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. */
  public void addAtIndex(int index, int val) {
    // If index is greater than the length, 
    // the node will not be inserted.
    if (index > size) return;

    // [so weird] If index is negative, 
    // the node will be inserted at the head of the list.
    if (index < 0) index = 0;

    // find predecessor and successor of the node to be added
    ListNode pred, succ;
    if (index < size - index) {
      pred = head;
      for(int i = 0; i < index; ++i) pred = pred.next;
      succ = pred.next;
    }
    else {
      succ = tail;
      for (int i = 0; i < size - index; ++i) succ = succ.prev;
      pred = succ.prev;
    }

    // insertion itself
    ++size;
    ListNode toAdd = new ListNode(val);
    toAdd.prev = pred;
    toAdd.next = succ;
    pred.next = toAdd;
    succ.prev = toAdd;
  }

  /** Delete the index-th node in the linked list, if the index is valid. */
  public void deleteAtIndex(int index) {
    // if the index is invalid, do nothing
    if (index < 0 || index >= size) return;

    // find predecessor and successor of the node to be deleted
    ListNode pred, succ;
    if (index < size - index) {
      pred = head;
      for(int i = 0; i < index; ++i) pred = pred.next;
      succ = pred.next.next;
    }
    else {
      succ = tail;
      for (int i = 0; i < size - index - 1; ++i) succ = succ.prev;
      pred = succ.prev.prev;
    }

    // delete pred.next 
    --size;
    pred.next = succ;
    succ.prev = pred;
  }
}
```

**复杂度分析**

+ 时间复杂度：
  + `addAtHead`，`addAtTail`： O(1)
  + `get`，`addAtIndex`，`delete`：O(min⁡(k,N−k))，其中 k指的是元素的索引
+ 空间复杂度：所有的操作都是 O(1)。