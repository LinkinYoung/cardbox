---
title      : 【中等】水果成篮
description: 
date       : 2022-10-22T10:51:57+08:00
lastmod    : 2022-10-22T10:51:57+08:00

Sections:
  - Title: 链接区域
    Groups:
    - Title: 索引
      Links:
      - Type: Page
        URL : LeetCode0
---
题目链接： https://leetcode.cn/problems/fruit-into-baskets/

　　最初思路：从左到右遍历，按先后顺序记下两个篮子的水果种类和起始坐标。如果新的水果不属于这两个水果，那就扔掉第一个水果，然后将当前水果作为第二个水果。
　　问题：当扔掉第一个水果时，第二个水果与当前水果之间可能仍然存在被扔掉的水果。
　　第一次修改：记录最后一种水果的起始坐标。当扔掉第一个水果时，将起始坐标作为新的第一个水果的坐标。
　　问题：遇到第三种水果时，前一个水果不一定是第二种水果。
　　第二次修改：记录最后一个水果种类和起始坐标，以及两个篮子的水果种类。当遇到第三种水果时扔掉所有水果，将篮子里的水果设置为当前水果和最后一个水果。当没有遇到新水果但是与前一个水果不同时，更新最后一个水果的种类和起始坐标。

```swift
class Solution {
    func totalFruit(_ fruits: [Int]) -> Int {
        var max = 0
        var fruit1: Int?
        var fruit2: Int?
        var lastFruit: Int?
        var start = 0
        var lastStart = 0
        
        for (i, fruit) in fruits.enumerated() {
            if fruit != fruit1 && fruit != fruit2 {
                let length = i - start
                if length > max {
                    max = length
                }
                fruit1 = lastFruit
                fruit2 = fruit
                start = lastStart
                lastStart = i
                lastFruit = fruit
            } else {
                if fruit != lastFruit {
                    lastStart = i
                    lastFruit = fruit
                }
            }
        }
        let length = fruits.count - start
        if length > max {
            max = length
        }
        return max
    }
}
```

## 修订记录
2022-10-22T10:51:57+08:00
* 创建。