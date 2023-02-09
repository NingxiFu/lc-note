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
- 优化, 用26定长数组, (字符-'a')是下标
```go
func checkInclusion(s1 string, s2 string) bool {
    if len(s1) > len(s2) {return false}
    need := [26]int{}
    window := [26]int{}
    for i, _ := range s1 {
        need[s1[i] - 'a']++
        window[s2[i] - 'a']++
    }
    if need == window {
        return true
    }
    for r := len(s1); r < len(s2); r++ {
        window[s2[r] - 'a']++
        window[s2[r - len(s1)] - 'a']--
        if need == window {
            return true
        }
    }
    return false
}
```

```py
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        need = {}
        window = {}
        left = 0
        right = 0
        valid = 0
        for a in s1:
            need[a] = need.get(a, 0) + 1
        while right < len(s2):
            a = s2[right]
            right += 1
            if a in need:
                window[a] = window.get(a, 0) + 1
                if window[a] == need[a]:
                    valid += 1
            if right - left >= len(s1): #right这时候先加了一，表示的是目前子串再往右一格的位置
                if valid == len(need): #valid是和need的长度比较，不是和s1比较！因为可能有重复字符
                    return True
                a = s2[left]
                left += 1
                if a in need:
                    if window[a] == need[a]:
                        valid -= 1
                    window[a] = window.get(a, 0) - 1
        return False
```
