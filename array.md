# 数组
[26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
<img width="1006" alt="image" src="https://user-images.githubusercontent.com/70481780/215467608-2741c1ae-5a29-40fd-a06e-a7f39a7f77b6.png">
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