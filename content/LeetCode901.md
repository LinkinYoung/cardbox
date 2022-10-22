---
title      : 【中等】股票价格跨度
description: 实现一个类，每次操作输入当天价格，输出股票价格低于当天价格的包含当天的连续日数。
date       : 2022-10-22T20:49:29+08:00
lastmod    : 2022-10-22T20:49:29+08:00

Sections:
  - Title: 链接区域
    Groups:
    - Title: 索引
      Links:
      - Type: Page
        URL : /
---
题目链接： https://leetcode.cn/problems/online-stock-span/

　　使用单调栈解决此问题。

单调栈
: 从栈底到栈顶元素单调递增或单调减少的栈。

　　对于本题，使用栈保存某天的价格和与上一个元素之间的距离（包括当前元素）。入栈前将所有价格小于当天价格的栈顶元素弹出。使用单调栈后时间复杂度为 O(n)，每一个元素都恰好入栈和出栈一次。使用模拟法时间复杂度为 O(n^2)。

```swift
class StockSpanner {

    var stock: [(Int, Int)]

    init() {
        self.stock = []
    }
    
    func next(_ price: Int) -> Int {
        var count = 1
        while let (p, pCount) = self.stock.popLast() {
            if p > price {
                self.stock.append((p, pCount))
                break
            } else {
                count += pCount
            }
        }
        self.stock.append((price, count))
        return count
    }
}
```

## 修订记录
2022-10-22T20:49:29+08:00
* 创建。