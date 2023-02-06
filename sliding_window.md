# 滑动窗口
### 567. 字符串的排列
[<img width="726" alt="image" src="https://user-images.githubusercontent.com/70481780/216898826-58bb40dd-7fa6-48fd-a911-3c03684b8a20.png">
](https://leetcode.cn/problems/permutation-in-string/)
- map
```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {return false}
    need := map[byte]int{}
    window := map[byte]int{}
    for i, _ := range s1 {
        need[s1[i]]++
        window[s2[i]]++
    }
    judge := func() bool {
        for k := range need {
            if need[k] != window[k] {
                return false
            }
        } 
        return true
    }
    if judge() {
        return true
    }
    for r := len(s1); r < len(s2); r++ {
        window[s2[r]]++
        window[s2[r - len(s1)]]--
        if judge() {
            return true
        }
    }
    return false
}
```
