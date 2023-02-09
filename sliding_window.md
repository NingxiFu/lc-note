# 滑动窗口
### 76. 最小覆盖子串
[<img width="622" alt="image" src="https://user-images.githubusercontent.com/70481780/217740599-5b7873ee-03a8-4dbc-8fab-ac9ff65275f1.png">](https://leetcode.cn/problems/minimum-window-substring/)

#### go
- 思路同下面py的注释
- 注意go语法：是否在字典里，以及range，赋值
```go
func minWindow(s string, t string) string {
    l, r := 0, 0
    need := map[byte]int{}
    window := map[byte]int{}
    valid := 0
    start, minl := 0, math.MaxInt
    for i, _ := range t {
        need[t[i]]++
    }
    for r, _ = range s {
        window[s[r]]++
        if _, ok := need[s[r]]; ok { //看s[r]是否在字典need里
            if window[s[r]] == need[s[r]] {
                valid++
            }
        }
        for valid == len(need) {
            if r - l + 1 < minl {
                start = l
                minl = r - l + 1
            }
            if _, ok := need[s[l]]; ok {
                if window[s[l]] == need[s[l]] {
                    valid--
                }
            }
            window[s[l]]--
            l++
        }
    }
    if minl == math.MaxInt {
        return ""
    } else {
        return s[start: start + minl]
    }
}
```
#### py
```py
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        need = {} #目标字串包括目标字母对应个数的字典
        window = {} #目前字串包含目标字母对应个数的字典
        valid = 0 #目前字串含有的符合条件的字母的个数
        left, right = 0, 0
        start = 0 #最短符合条件字串的开始位置
        sub_len = inf #最短符合条件字串的长度
        for a in t: #把目标子串的字典建好
            need[a] = need.get(a, 0) + 1
        while right < len(s): #right走完整个s大字符串
            if s[right] in need: #滑动窗口右侧加入元素，看是否是目标元素之一
                window[s[right]] = window.get(s[right], 0) + 1
                if window[s[right]] == need[s[right]]: #某字母符合要求，valid加一
                    valid += 1
            right += 1
            while len(need) == valid: #目前字串已经符合要求，就一直pop左边，直到缺一个不符合条件
                if right - left < sub_len: #算符合条件时候的子串长度是否是最短的，若是，更新
                    start = left
                    sub_len = right - left
                a = s[left]
                if a in need: #若左侧出去的元素是目标字母之一
                    if window[a] == need[a]: #比较两个计数以后再减window对应数量和可能valid减一
                        valid -= 1
                    window[a] = window.get(a, 0) - 1
                left += 1
        if sub_len == inf: #若没找到符合子串，返回空
            return ''   
        else:
            return s[start : start + sub_len]
```
### 567. 字符串的排列
[<img width="726" alt="image" src="https://user-images.githubusercontent.com/70481780/216898826-58bb40dd-7fa6-48fd-a911-3c03684b8a20.png">
](https://leetcode.cn/problems/permutation-in-string/)
#### 用go和python写了两种思路，第一种是先把窗口初始化为目标长度然后一步步挪动，第二种是直接从零开始增大窗口，控制需要缩减窗口长度的触发条件（等于目标长度时）。总体来说，第一种适用于固定长度的窗口，第二种更为灵活。
#### go1.1
- 用map
- 特点是滑动窗口是长度固定的，所以相当于把所有的滑动窗口可能的所在位置遍历，一一校验就行。
- 先从s2的头开始，截取和s1长度相等的那一段，作为初始化的比较段。将来的比较段的长度也一直维持在len(s1)长度。
- 初始化的比较段和s1进行比对，若相等则直接返回，若不相等再进行下面的步骤，一步步挪动长度为len(s1)的比较字段直到s2的末尾。
- 先把比较段挪动一步，把window内参数改了，一加一减。
- 此时的新比较段和s1进行比对，若相等则直接返回，若不相等就继续循环挪动。
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
        window[s2[r]]++ //这里变成了目标字符串长度+1的长度
        window[s2[r - len(s1)]]-- //缩短1，变成目标字符串长，这里l和r之间只差len(s1)而不是len(s1)-1，是因为该解法的中间时刻比较段长度是len(s1)+1
        if judge() {
            return true
        }
    }
    return false
}
```
#### go1.2
- 1.1基础上优化数据结构，因为该题窗口内元素可能的值只能是26个字母，可以用定长的数组来装。
- 不用map，用26定长数组, (字符-'a')作下标。
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
        window[s2[r]]++ //这里变成了目标字符串长度+1的长度
        window[s2[r - len(s1)]]-- //缩短1，变成目标字符串长，这里l和r之间只差len(s1)而不是len(s1)-1，是因为该解法的中间时刻比较段长度是len(s1)+1
        if need == window {
            return true
        }
    }
    return false
}
```
#### py2.1
- map
- 用valid计map中符合条件的key个数
- valid==need中key个数时就说明窗口完全符合了
```py
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        need = {} #目标数据
        l, r = 0, 0
        window = {} #窗口数据主要就是window和valid
        valid = 0
        for a in s1:
            need[a] = need.get(a, 0) + 1
        for r in range(len(s2)): #窗口不初始化为目标长度，直接从0开始增大，只控制它增到目标大小时就进行缩减
            window[s2[r]] = window.get(s2[r], 0) + 1 #增大窗口，更新窗口数据
            if window.get(s2[r], 0) == need.get(s2[r], 0):
                valid += 1
            if r - l == len(s1) - 1:    #目前窗口长度=目标长度，开始比对，紧接着下一步要收缩窗口才能继续重新增大窗口
                if valid == len(need):  #判断窗口是否符合条件
                    return True         #符合则立刻返回，不符合则紧接着开始缩小窗口，为了for循环下一轮的增大窗口（由于该题窗口为定长）
                if window.get(s2[l], 0) == need.get(s2[l], 0): #缩小窗口，更新窗口数据
                    valid -= 1
                window[s2[l]] = window.get(s2[l], 0) - 1
                l += 1 #r在for循环里一直在加，别忘了缩小窗口时更新l，一直指向窗口最左侧index
        return False
```
