# 链表（面试重点）

**链表题一定要捋清思路，注意特判**

* 需要几个指针：node, left? right?
* 指针之间怎么更新和移动
* 怎么判定边界和终止条件**（一定要切记开头判断一下head是否为空）**

### **移动链表节点：tmp指针4步法**

**移动链表节点到指定位置的后面：取出+缝合+接头部+接尾部**

e.g.把`right->next节点`移动到`left位置`的后面，再把right缺口处缝合

```cpp
ListNode * tmp = right->next;  //取出
right->next = right->next->next; //缝合
tmp->next = left->next; //接尾部
left->next = tmp; //接头部
```

**注意：处理后并没有移动right,left的位置**

### **题目**

| 序号/难度 | 名字 | 备注 |  |
| :--- | :--- | :--- | :--- |
| 328 | 奇偶链表 | 捋清思路 | 中等 |
| 142 | 环形链表 | 双指针 | 中等 |
| 剑指 Offer 24 | 翻转链表 | 缕清思路 | 简单 |
| 面试题 02.04 | 分割链表 | 经典：节点移动来移动去（需要总结这类规律） | 很容易乱 |
| 138 | 复杂链表的复制 | 链表里面包含random指针，怎么深拷贝，用hashmap存 | 中等/做出 |
| 剑指 Offer 52 | 两个链表的第一个公共交点 | O\(1\)的空间，所以不能用哈希表，用双指针 | 中等 |
| 445. | 两数相加II | 反转链表完成 / 栈完成 / 正向处理 | 中等 |
| 61 | 旋转链表 | 缕清思路 | 中等 |
| 24 | 两两交换链表中获得节点 | 注意长度可能是奇偶，也可能是0或1要讨论一下 | 中等 |

**328. 奇偶链表**

给定一个单链表，把所有的奇数位置节点和偶数位置节点分别排在一起**，**要求空间复杂度O\(1\)，时间O\(N\)

**捋清整个过程**

假如我有1-&gt;2-&gt;3-&gt;null

**需要几个指针**：

* odd指针：作用指向odd部分尾端，初始是节点1
* even指针：作用指向even部分尾端，初始是节点2
* node指针：指向even后面的部分，表示这一步需要处理的节点

**更新和移动：**

node = even-&gt;next node是3

* 处理even: 
  * 移动2的next节点从3到null: 将even-&gt;next更新为node-&gt;next
  * 更新even：even = even-&gt;next
* 处理odd: 
  * 移动3到1与2中间，将node-&gt;next更新为even，将odd-&gt;next更新为node
  * 更新odd：odd = odd-&gt;next

**边界条件+终止条件：**

1. 如果head为null或者head-&gt;next为null，那么只用返回head就可以了
2. 如果开始进入while正片的时候，终止条件就是even后面没有节点了+even-&gt;next没有节点了

   这个判定其实可以在前面编程中意识到

代码：

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head == NULL || head->next == NULL)return head;
        ListNode* odd = head;
        ListNode* even = head->next;
        while(even!=NULL && even->next!=NULL){
            ListNode* node = even->next;
            even->next = node->next;
            even = even->next;
            node->next = odd->next;
            odd->next = node;
            odd = odd->next;
        }
        return head;
    }
};
```

**剑指 Offer 24 反转链表**

有条件尽量在纸上画一下，node, nxt怎么移动，以及next指针怎么更新

**反转链表需要tmp node来协助**

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL)return NULL;
        ListNode * nxt = head->next;
        ListNode * node = head;
        node->next = NULL;
        while(nxt!=NULL){
            ListNode * tmp = nxt;
            nxt = nxt->next;
            tmp->next = node;
            node = tmp;
        }
        return node;
    }
};
```

**面试题 02.04. 分割链表**

**题意：**要把小于x的数移动到链表的左边，大于等于x的数移动到链表的右边

**题解：**维护两个指针

指针①：表示小于x的末尾指针

指针②：表示当前往前扫的指针

处理开头如果有特殊情况+指针初始化

* 如果开头是大于x的，要找到一个小于x的第一个节点，把它作为新的开头
  * 如果找不到直接返回head
  * 如果找到就移动这个节点到开头
* 如果开头就是小于x的，把往前扫的指针移动到第一个不是小于x的位置

然后处理正式部分

* 从当前右边的指针开始如果扫到小于x的节点，把它插入到前面部分的末尾

```cpp
ListNode* partition(ListNode* head, int x) {
    if(head==NULL)return NULL;
    ListNode* newhead = NULL;
    ListNode* right = head;
    //处理开头特殊情况以及指针初始化
    if(head->val >= x){
       while(right->next!=NULL){
           if(right->next->val < x){
               newhead = right->next;
               right->next = right->next->next;
               newhead->next = head;
               break;
           }
           right = right->next;
       }
    }else{
        while(right->next!=NULL){
            if(right->next->val < x){
                right = right->next;
            }else{
                right = right->next;
                break;
            }
        }
    }
    if(newhead == NULL)newhead = head;
    //不加这个，会漏了一种关键情况，就是本身list中就没有比x小的
    ListNode* left = newhead;
    //处理正式部分
    while(right->next!=NULL){
        if(right->next->val < x){
            //tmp指针法：4行移动节点到指定位置后面
            ListNode * tmp = right->next;
            right->next = right->next->next;
            tmp->next = left->next;
            left->next = tmp;
        }else{
            right = right->next;    
        }
    }
    return newhead;
}
```

**剑指 Offer 35. 复杂链表的复制**

**题意：深拷贝一个带有随机指针的链表**

**题解：**第一遍先循环一遍普通指针，然后建立unordered\_map&lt;Node\*, Node\*&gt;存新旧指针

第二遍更新new linked list的random指针

代码见leetcode 剑指offer35

**445.两数相加II**

写一个正向处理不用 翻转链表/栈 的方法

**题解：**先计算两个长度

然后为了避免讨论如果l1是短的那个，就交换，保证l1是长的链表

正向处理，先创建一个首位节点为0，因为sum的长度最多也就是比l1多一位

数位对齐后，计算下一位的值注意更新上一位的carry位

最后判断首位节点还是不是0，如果是0返回head-&gt;next就好了

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int count = 0, temp;
        ListNode *head, *last;
        for(head = l1; head; head = head->next)
            count++;
        for(head = l2; head; head = head->next)
            count--;
        if(count < 0)                       //计算两链表长度，将l1指向长链，l2指向短链，将l2的值加到l1中
            swap(l1,l2);            
        last = head = new ListNode(0);      //在链首加一个值为0的节点作为初始的last节点，如果最终该节点值仍为0则删除该节点
        head->next = l1;
        for(int i = abs(count); i != 0; i--){  //将两链数位对齐
            if(l1->val != 9)
                last = l1;
            l1 = l1->next;
        }
        while(l1){
            temp = l1->val + l2->val;
            if(temp > 9){                   //如果发生进位，则更新last到l1之间所有数位的值
                temp -= 10;                 //进位后当前数位最大值为8，故将last指针指向当前数位
                last->val += 1;
                last = last->next;
                while(last != l1){
                    last->val = 0;
                    last = last->next;
                }
            }
            else if(temp != 9)             
                last = l1;
            l1->val = temp;
            l1 = l1->next;
            l2 = l2->next;
        }
        return head->val == 1 ? head : head->next;
    }
};

作者：aninvalidname
链接：https://leetcode-cn.com/problems/add-two-numbers-ii/solution/c-bu-yong-di-gui-bu-yong-zhan-yuan-di-ji-suan-by-a/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

**61.旋转链表**

```text
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
其中k可能大于len，也可能等于0
```

坑：注意是向右移动

**题解：**

①计算长度，并且获得一个末尾的node

②因为题意是向右移动，所以前面用长度减去k的mod就是从head移动move次到达中断节点

③找到中断两边的节点left与right，也有末尾节点end，以及head

```cpp
ListNode* rotateRight(ListNode* head, int k) {
    if(head==NULL)return NULL;
    ListNode* newhead = NULL;
    ListNode* right = head;
    ListNode* tail = head;
    int len = 1;
    while(tail->next!=NULL){
        tail = tail->next;
        len++;
    }
    int move = (len - k%len)%len; //表示中断的节点位置，从head向后移动了move次
    ListNode* left = NULL;
    while(move--){
        left = right;
        right = right->next; //找到中断节点的左右节点，move范围就是0~len-1
    }
    newhead = right;
    if(left!=NULL){ //left==NULL的时候就是move=0的时候，要判定一下
        tail->next = head;
        left->next = NULL;
    }
    return newhead;
}
```

