## § Problem 86 Partition List
> [分隔链表](
https://leetcode-cn.com/problems/partition-list/)

给你一个链表和一个特定值 `x` ，请你对链表进行分隔，
使得所有小于 `x` 的节点都出现在大于或等于 `x` 的节点之前。
你应当保留两个分区中每个节点的初始相对位置。


### Notes
* 创建两个链表，分别保存小于x的节点和大于等于x的节点，
遍历完链表节点后将这两个链表连接并返回即可。

* **链表的创建**。 分别定义一个链表节点 `listNode` 和指向链表头的指针 `listHead`。
在链表末尾添加新的节点只需要：
将当前节点的 `next`  指针指向下一节点地址；
将当前节点移向下一节点 (`next` 指针指向的节点)。


**My Solution:** 
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode* left = new ListNode(0);
        ListNode* leftHead = left;
        ListNode* right = new ListNode(0);
        ListNode* rightHead = right;
        while (head != nullptr) {
            if (head->val < x) {
                left->next = head;
                left = left->next;
            } else {
                right->next = head;
                right = right->next;
            }
            head = head->next;  // 查询下一节点
        }
        left->next = rightHead->next;  // 合并链表
        right->next = nullptr;
        return leftHead->next;
    }
};```


