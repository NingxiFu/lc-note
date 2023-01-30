# 数组
### 26. 删除有序数组中的重复项
[<img width="1006" alt="image" src="https://user-images.githubusercontent.com/70481780/215467608-2741c1ae-5a29-40fd-a06e-a7f39a7f77b6.png">](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

- 快慢指针
```go
func removeDuplicates(nums []int) int {
    if len(nums) == 0{
        return 0
    }
    slow, fast := 1, 1
    for ; fast < len(nums); fast++ {
        if nums[fast - 1] != nums[fast]{ //fast是新的不同元素，就写进slow
            nums[slow] = nums[fast]
            slow++
        }
    }
    return slow
}
```

```py
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        slow = 0
        fast = 1
        for i in range(fast, len(nums)):
            if nums[i] != nums[slow]:
                slow += 1
                nums[slow] = nums[i]
            fast += 1
        return slow + 1
```


### 83. 删除排序链表中的重复元素
[<img width="678" alt="image" src="https://user-images.githubusercontent.com/70481780/215518648-c93f5c58-af6a-436a-820f-65181ee39ac9.png">
](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)
- 26的链表版
- 快慢指针 删除链表的重复节点，注意head空的情况

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteDuplicates(head *ListNode) *ListNode {
    if head == nil { //head空则返回nil
        return nil
    }
    slow := head //快慢指针初始化，都指向head
    fast := head
    for fast != nil{ //fast遍历链表,slow只记录fast走过的不重复的节点
        if fast.Val == slow.Val { //若快慢相等说明fast指向的是重复值（因为该题的链表已排序）
            fast = fast.Next //fast前移，跳过，slow不记录这个相等的
        } else { //若fast指向的不是重复值，则让slow记录fast目前节点，然后两者前移
            slow.Next = fast
            slow = slow.Next
            fast = fast.Next
        }
    }
    slow.Next = nil //slow已经记录完所有不重复的值，结束链表
    return head
}
```

```py
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head:
            return head
        slow = head
        fast = head
        while fast:
            if fast.val == slow.val:
                fast = fast.next
            else:
                slow.next = fast
                slow = slow.next
                fast = fast.next
        slow.next = None
        return head
```

### 27. 移除元素
[<img width="726" alt="image" src="https://user-images.githubusercontent.com/70481780/215524236-a06a8310-ec8c-45da-80f0-c4a793818610.png">
](https://leetcode.cn/problems/remove-element/)


- 快慢指针 数组
```py
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for i in range(len(nums)):
            if nums[i] == val:
                continue
            else:
                nums[slow] = nums[i]
                slow += 1
        return slow
```
