# 滑动窗口
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
