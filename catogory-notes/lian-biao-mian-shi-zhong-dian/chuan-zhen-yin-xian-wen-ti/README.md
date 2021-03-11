# 穿针引线问题

### **经典穿针引线问题**

**解决思路：**

* **明确迭代指针需要哪些：例如prev,next,pv,nt等**
* **对于穿针引线问题，也就是需要找到穿针的头尾节点**

**（示例：92.反转链表-ii\)**

\*\*\*\*

![](../../../.gitbook/assets/1%20%281%29.jpg)

![](../../../.gitbook/assets/2%20%281%29.jpg)

![](../../../.gitbook/assets/3%20%281%29.jpg)

如图有两个断点，共涉及到四个节点。于是我给它们依次编号为 a，b，c，d**（这些都要用指针存下来）**

找到后就简单了，直接**「穿针引线」（表示为pv, nt, prev, node\)**

```cpp
...(先找到节点）
nt = pv->next;
node = prev->next;
...(对内部进行操作,例如完成反转链表)
pv->next = prev;
nt->next = node;
```

（**其实对于反转链表-ii的问题，可以一边反转链表，一边找节点，只用一次循环完成**）

### **穿针引线问题代表示例**

**61.旋转链表**

```text
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
其中k可能大于len，也可能等于0
```

坑：注意是向右移动

**这题题解：**

题意是向右移动，所以前面用长度减去k的mod就是从head移动move次到达中断节点

```cpp
int move = (len - k%len)%len; //表示中断的节点位置，从head向后移动了move次
```

* 所以这个问题示例中，也就是找到4， 5， 1， 3，
* 然后4通过3来找到，作为head， 把5接到1上，完成穿针引线

### **反转链表问题**

**3.11 新思路，prev设置为NULL，不用判断head或者head-&gt;next存不存在**

```cpp
ListNode* reverseList(ListNode* head){
    ListNode* prev = NULL;
    ListNode* cur = head;
    while(cur){
        ListNode* nxt = cur->next;
        cur->next = prev;
        prev = cur;
        cur = nxt;
    }
    return prev;
}
```

**=========================下面是以前思路（不推荐了）==================**

**三步：（4行）**

* 1.声明指针
  * 自变指针：prev, node,
  * 他变指针：nxt
* 2.确定操作operate
* 3.确定指针每次循环的更新iterate

![](../../../.gitbook/assets/wu-biao-ti-%20%284%29.png)

**反转链表模板（四行模板）**

* **自变指针：prev, node**
* **他变指针：nxt**

```cpp
/*operate*/
ListNode* nxt = node->next; 
node->next = prev;
/*iterate*/
prev = node;
node = nxt;
```

**记忆：（四要素）**

```cpp
ListNode* reverseList(ListNode* head) {
    if(!head || !head->next)return head;//特判
    /*初始化*/
    ListNode* prev = head;
    ListNode* node = head->next;
    prev->next = NULL;
    while(node){//终止条件
        /*四行模板*/
         ListNode* nxt = node->next;
         node->next = prev;
         prev = node;
         node = nxt;
    }
    return prev;
}
```

