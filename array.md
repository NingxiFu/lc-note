# 数组
### 26. 删除有序数组中的重复项
[<img width="1006" alt="image" src="https://user-images.githubusercontent.com/70481780/215467608-2741c1ae-5a29-40fd-a06e-a7f39a7f77b6.png">](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)

- 快慢指针
- 删除有序数组里重复的项
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
- 26题的链表版
- 快慢指针 
- 删除链表的重复节点
- 注意head空的边界
- fast遍历链表,slow只记录fast走过的不重复的节点

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteDuplicates(head *ListNode) *ListNode {
    if head == nil {                //head空则返回nil
        return nil
    }
    slow := head                    //快慢指针初始化，都指向head
    fast := head
    for fast != nil{                //fast遍历链表,slow只记录fast走过的不重复的节点
        if fast.Val == slow.Val {   //若快慢相等说明fast指向的是重复值（因为该题的链表已排序）
            fast = fast.Next        //fast前移，跳过，slow不记录这个相等的
        } else {                    //若fast指向的不是重复值，则让slow记录fast目前节点，然后两者前移
            slow.Next = fast
            slow = slow.Next
            fast = fast.Next
        }
    }
    slow.Next = nil                 //slow已经记录完所有不重复的值，结束链表
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


- 快慢指针 
- 移除数组里的指定值 
- 快指针遍历 慢指针逐个改动原数组（fast遍历数组,slow只写fast走过的不重复的值）
```go
func removeElement(nums []int, val int) int {
    slow := 0
    fast := 0
    for fast < len(nums) {
        if nums[fast] == val {  //等于val要删掉的直接不管
            fast++
        } else {                //不删掉的直接往slow上加，slow和fast再后移
            nums[slow] = nums[fast]
            slow++
            fast++
        }
    }
    return slow
}
```
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
### 167. 两数之和 II - 输入有序数组
[<img width="873" alt="image" src="https://user-images.githubusercontent.com/70481780/215535240-4416d30a-040b-406c-ae51-8f229d6ca28f.png">
](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

- 两数之和
- 双指针
```go
func twoSum(numbers []int, target int) []int {
    l, r := 0, len(numbers) - 1
    for l < r {
        sum := numbers[l] + numbers[r]
        if sum == target {
            return []int{l + 1, r + 1}
        } else if sum < target {
            l++
        } else if sum > target {
            r--
        }
    }
    return []int{-1, -1}
}
```
```py
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        while left < right:
            cur = numbers[left] + numbers[right]
            if cur == target:
                return [left + 1, right + 1]
            elif cur > target:
                right -= 1
            elif cur < target:
                left += 1
```

### 344. 反转字符串
[<img width="640" alt="image" src="https://user-images.githubusercontent.com/70481780/215535954-e9b6cf70-5e40-4ad3-9bb0-52b23dc2e5b8.png">
](https://leetcode.cn/problems/reverse-string/)

- 双指针 
- 原地改变数组值 反转字符串

```go
func reverseString(s []byte)  {
    l, r := 0, len(s) - 1
    for l < r {
        s[l], s[r] = s[r], s[l]
        l++
        r--
    }
}
```
```py
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```
### 5. 最长回文子串
[<img width="602" alt="image" src="https://user-images.githubusercontent.com/70481780/215538809-a3b63cac-12a5-43d3-a777-070efba79b3c.png">
](https://leetcode.cn/problems/longest-palindromic-substring/)
