<a name="mIloo"></a>
#### [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/25962088/1675932639550-a01a6a36-e85b-489d-be91-ff6b3b333b9c.png#averageHue=%23efeff0&clientId=ue3351d53-2db2-4&from=paste&height=556&id=ua220e5d0&name=image.png&originHeight=1112&originWidth=1246&originalType=binary&ratio=1&rotation=0&showTitle=false&size=183913&status=done&style=none&taskId=u1222cf92-6536-4dab-8633-08389b024f7&title=&width=623)
```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        need = {}
        window = {}
        left = 0
        right = 0
        valid = 0
        start = []
        for a in p:
            need[a] = need.get(a, 0) + 1
        while right < len(s):
            a = s[right]
            right += 1
            if a in need:
                window[a] = window.get(a, 0) + 1
                if window[a] == need[a]:
                    valid += 1
            if right - left >= len(p):
                if valid == len(need):
                    start.append(left)
                a = s[left]
                left += 1
                if a in need:
                    if window[a] == need[a]:
                        valid -= 1
                    window[a] -= 1
        return start
```
