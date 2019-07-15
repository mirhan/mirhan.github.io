---
layout:     post
title:      敏感词检测算法
date:       2019-07-15
author:     李不清
catalog: true
tags:
    - 算法
---

# 敏感词过滤
已知一个有敏感词列表，如果输入的长文本里包含此列表的任意一个值，则给出报警。

## 直觉解法
这里把长文本称为 longText；  
把敏感词列表都读入一个 wordList；  
遍历此 List，对每一个敏感词 word；  
如果长文本 longText 包含此敏感词 sensitiveWord，则记录或者退出。  
伪代码：  

```java
// Check if contains any of wordList
for(int i; i < wordList.size(); i++ ) {
    String word = wordList.get(i);
    if (longText.contains(word)) {
        return true; // contains sensitive word
    }
}

return false; // NOT contains sensitive word
```

### 直觉解法复杂度

longText.contains(word) 的实现，实际上是暴力解法，  
也就是说，会从位置 0 到位置 longText.length - word.length 开始，比较每一个字符是否相等  
伪代码：  
```java
int max = longText.length - word.length;
for (int i = 0; i <= max; i++) {
    // Look for first character.
    if (longText[i] != word[0]) {
        while (++i <= max && longText[i] != word[0]);
    }

    // Found first character, now look at the rest of v2
    if (i <= max) {
        int j = i + 1;
        int end = j + word.length - 1;
        for (int k = 1; j < end && longText[j]
                == word[k]; j++, k++);

        if (j == end) {
            // Found whole string.
            return i;
        }
    }
}
return -1;
```

可以看到 contains 复杂度为 O(longText.length) * O(word.length)  
* (思考：为什么 contains 不使用 KMP 算法？) *  

那么这个直觉解法的复杂度就是   
O(wordList.size()) * O(longText.length) * O(word.length)  

如果将 contains 更换为 KMP 算法的话，实际上这里的效率是会有所提高的。  

### 直觉解法评估
wordList 占用内存空间大；  
算法复杂度高；  


## 前缀树算法
同样的，这里把长文本称为 longText；  
把敏感词列表读入到一个前缀树；  
以图片展示，下图是存了 8个键的前缀树："A", "to", "tea", "ted", "ten", "i", "in", 和 "inn"  
![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Trie_example.svg/250px-Trie_example.svg.png "前缀树")

###  前缀树算法复杂度
前缀树算法需要从位置 0 到位置 longText.length - word.length 开始，比较每一个字符是否被包含于前缀树  
如果被包含的时候，比较每一个字符是否被包含于子树；  
复杂度为 O(longText.length) * O(word.length)

### 前缀树算法评估
占用内存空间小；  
算法复杂度高；  

前缀树本质上就是一个 DFA，所以很多地方也把他称作 DFA 算法，但我更偏向于叫做前缀树算法。  

## 更多思考
这个算法还比较原始，只作为初始思考，还需要更多完善。  
比如，无法处理用户输入字符串被混淆的情况，比如 "大&傻*叉"。  
当然，你可以简单地将用户输入字符串里面的无意义字符过滤以后再做比较。  
如果有更多想法，欢迎交流。  













