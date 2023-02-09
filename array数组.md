<a name="q7Co3"></a>
### [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
![](https://user-images.githubusercontent.com/70481780/215467608-2741c1ae-5a29-40fd-a06e-a7f39a7f77b6.png#height=831&id=Ay1AK&originHeight=1662&originWidth=2012&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=1006)

- 快慢指针
- 删除有序数组里重复的项
<a name="KiOGq"></a>
#### go
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
<a name="wrIrx"></a>
#### py
```python
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

<a name="dWFI0"></a>
### [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)[<br />](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/)![](https://user-images.githubusercontent.com/70481780/215518648-c93f5c58-af6a-436a-820f-65181ee39ac9.png#height=629&id=FDvfK&originHeight=1258&originWidth=1356&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=678)
<a name="PBDwx"></a>
#### go

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
<a name="BUFnC"></a>
#### py
```python
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

<a name="B17yU"></a>
### [27. 移除元素](https://leetcode.cn/problems/remove-element/)[<br />](https://leetcode.cn/problems/remove-element/)![](https://user-images.githubusercontent.com/70481780/215524236-a06a8310-ec8c-45da-80f0-c4a793818610.png#height=848&id=phIZT&originHeight=1696&originWidth=1452&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=726)
<a name="JBJmW"></a>
#### go

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
<a name="R5yuD"></a>
#### py
```python
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
<a name="KYnww"></a>
### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)[<br />](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)![](https://user-images.githubusercontent.com/70481780/215535240-4416d30a-040b-406c-ae51-8f229d6ca28f.png#height=638&id=YAMsQ&originHeight=1276&originWidth=1746&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=873)
<a name="OH3Gc"></a>
#### go

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
<a name="CNyKO"></a>
#### py
```python
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
<a name="OQoVt"></a>
### [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)[<br />](https://leetcode.cn/problems/reverse-string/)![](https://user-images.githubusercontent.com/70481780/215535954-e9b6cf70-5e40-4ad3-9bb0-52b23dc2e5b8.png#height=385&id=YjwoH&originHeight=770&originWidth=1280&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=640)
<a name="cFkjf"></a>
#### go

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
<a name="wh9LA"></a>
#### py
```python
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
<a name="OdsN6"></a>
### [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/)[<br />](https://leetcode.cn/problems/longest-palindromic-substring/)![](https://user-images.githubusercontent.com/70481780/215538809-a3b63cac-12a5-43d3-a777-070efba79b3c.png#height=401&id=Dl8Ry&originHeight=802&originWidth=1204&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=&width=602)
<a name="NEHfe"></a>
#### go

- 双指针从中间往外侧走
- 回文子串长度分奇偶，遍历中心点时，每个位置作为奇数的中点和偶数的中点都试一次
- 不断更新最长子串
```go
func longestPalindrome(s string) string {
    res := ""
    l, r := 0, 0 //双指针，指目前子串的左右边界
    for i := 0; i < len(s); i++ {
        l, r = i, i //回文字符串长度奇数，从同一个数开始扩展
        for l >= 0 && r < len(s) && s[l] == s[r] {
            l--
            r++
        }
        if r - l + 1 > len(res) + 2 { //出循环时，两个指针指向子串左右多走一步的位置
            res = s[l + 1: r]
        }
        l, r = i, i + 1 //回文字符串长度偶数，从两个相邻数开始扩展
        for l >= 0 && r < len(s) && s[l] == s[r] {
            l--
            r++
        }
        if r - l + 1 > len(res) + 2 {
            res = s[l + 1: r]
        }
    }
    return res
}
```
<a name="sQ9Oh"></a>
#### py

- python贴个之前写的老版本
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) <= 1:
            return s
        if len(s) == 2:
            if s[0] == s[1]:
                return s
            else:
                return s[0]
        max_sub = s[0]
        for mid in range(0,len(s) - 1):
            left = mid
            right = mid
            while left >= 0 and right <= len(s) - 1 and s[left] == s[right]:
                left -= 1
                right += 1
            if right - left - 1 > len(max_sub): #这里right和left都比实际符合条件的子串向外多走了一步
                max_sub = s[left + 1 : right]
            left_1 = mid
            right_1 = mid + 1
            while left_1 >= 0 and right_1 <= len(s) - 1 and s[left_1] == s[right_1]:
                left_1 -= 1
                right_1 += 1
            if right_1 - left_1 - 1 > len(max_sub):
                max_sub = s[left_1 + 1 : right_1]
        return max_sub
```
