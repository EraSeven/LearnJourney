# 创建Git hub

**1.改名Era_7th**

**2.创建 [EraSeven/LearnJourney](https://github.com/EraSeven/LearnJourney) 仓库**

# LeetCode 第五题题解分析

## 📝 错误题解 1

```java
class Solution {
    public String longestPalindrome(String s) {
        int max = 0;
        int left = s.length() / 2 - 1;
        int right = left + 1;
        Map<Integer, String> strs = new HashMap<>();

        while (left >= 0){
            for(; right < s.length(); right++){
                if (s.charAt(left) == s.charAt(right)){
                    int currentLength = right - left + 1;
                    if (max < currentLength){
                        max = currentLength;
                    }
                    strs.put(max, s.substring(left, right + 1));
                }
            }
            left--;
            right = s.length() / 2 + 1;
        }
        return strs.get(max);
    }
}
```

**未处理 s="a" 边界情况**



## 📝 错误题解 2

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s.length() == 1){
            return s;
        }
        int max = 0;
        int left = s.length() / 2 - 1;
        int right = left + 1;
        Map<Integer, String> strs = new HashMap<>();

        while (left >= 0){
            for(; right < s.length(); right++){
                if (s.charAt(left) == s.charAt(right)){
                    int currentLength = right - left + 1;
                    if (max < currentLength){
                        max = currentLength;
                    }
                    strs.put(max, s.substring(left, right + 1));
                }
            }
            left--;
            right = s.length() / 2 + 1;
        }
        if (max == 0){
            return String.valueOf(s.charAt(0));
        } else {
            return strs.get(max);
        }
    }
}
```

**原代码思想为从中间裂开向两边拓展，没有考虑到回文串在两边的情况，遇到案例 s = "abb" 输出 "a", 答案是"bb".**



## 📝 错误题解 3

```java
class Solution {
    public String longestPalindrome(String s) {
                 if (s.length() == 1){
            return s;
        }
        int max = 0;
        Map<Integer, String> strs = new HashMap<>();

        for (int left = 0; left < s.length(); left++) {
            for(int right = left + 1; right < s.length(); right++){
                if (s.charAt(left) == s.charAt(right)){
                    int currentLength = right - left + 1;
                    if (max < currentLength){
                        max = currentLength;
                    }
                    strs.put(max, s.substring(left, right + 1));
                }
            }
        }
        if (max == 0){
            return String.valueOf(s.charAt(0));
        } else {
            return strs.get(max);
        }
    }
}
```

**没有考虑到 s = "ccc" 的情况，代码只会输出前后两个字母相等，即输出 "cc" ，考虑欠缺**



## 📝 正确题解 1

```java
class Solution {
    public String longestPalindrome(String s) {
                 if (s.length() == 1){
            return s;
        }
        int max = 0;
        Map<Integer, String> strs = new HashMap<>();

        for (int left = 0; left < s.length(); left++) {
            for(int right = left + 1; right < s.length(); right++){
                if (isPalindrome(s, left, right)) {
                    int currentLength = right - left + 1;
                    if (currentLength > max) {
                        max = currentLength;
                        strs.put(max, s.substring(left, right + 1));
                    }
                }
            }
        }
        if (max == 0){
            return String.valueOf(s.charAt(0));
        } else {
            return strs.get(max);
        }
    }

        private static boolean isPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

**暴力枚举**



## 📝 正确题解 2

```java
public class Solution {
private int maxLen = 0;
private int start = 0;

public String longestPalindrome(String s) {
    if (s.length() < 1) return "";
    maxLen = 0;
    start = 0;

    for (int i = 0; i < s.length(); i++) {
        expand(s, i, i);    // 奇数扩展
        expand(s, i, i + 1);// 偶数扩展
    }

    return s.substring(start, start + maxLen);
}

private void expand(String s, int left, int right) {
    while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    int currentLen = right - left - 1;
    if (currentLen > maxLen) {
        maxLen = currentLen;
        start = left + 1;
    }
}
}
```

**定义变量 start 来记录回文串起始位置，定义变量 maxLen 来记录最大回文串长度，时间复杂度从O(n^3)减少到O(n^2)，不使用Map来记录，大大降低了时间复杂度和空间复杂度**



## 📝 正确题解 3

```java
public class Solution {
private int maxLen = 0;
private int start = 0;

public String longestPalindrome(String s) {
        if (s.length() < 1) return "";

        char[] arr = s.toCharArray();

        for (int i = 0; i < arr.length; i++) {
            if ((arr.length - i) * 2 <= maxLen) break;
            expand(arr, i, i);    // 奇数长度回文
            expand(arr, i, i + 1);// 偶数长度回文
        }

        if (maxLen != 0) {
            return s.substring(start, start + maxLen);
        } else {
            return String.valueOf(s.charAt(0));
        }
}

    public void expand(char[] arr, int left, int right) {
        while (left >= 0 && right < arr.length
                && arr[left] == arr[right]) {
            left--;
            right++;
        }

        // 计算实际回文长度（退出循环时 left/right 已指向不匹配位置）
        int currentLen = right - left - 1;
        if (currentLen > maxLen) {
            maxLen = currentLen;
            start = left + 1; // 回文起始位置为 left+1
        }
    }
}
```

**在正确题解2上进一步优化**

- 使用数组来处理字符串，减少charAt()方法的使用次数；
- 当剩余点中心的最大可能长度无法超越当前最大长度时，提前终止循环
