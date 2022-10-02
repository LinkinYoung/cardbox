---
title      : "Cocoa Objects"
description: 介绍 OC 的优势和基本用法以及 Cocoa 对象的共通行为、接口和生命周期。
date       : 2022-05-12T16:10:13+08:00
lastmod    : 2022-05-12T18:42:04+08:00

Sections:
  - Title: 相关内容
    Groups:
    - Title: 下一页
      Links:
      - Type: Page
        URL : REF21C3
    - Title: 上一页
      Links:
      - Type: Page
        URL : REF21C1
    - Title: 所属书籍
      Links:
      - Type: Page
        URL : REF21
    - Title: 卡片和链接
      Links:
      - Type: Page
        URL : C43
      - Type: Page
        URL : /
---
文章链接: https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW31

{{< aside >}}
**文献卡片备注**
　　此卡片为文献卡片。文献卡片使用精炼的语言总结原始文献的内容，并可能带有指向其他卡片的链接。文献卡片不追求完整还原原始文献，建议读者结合此类卡片完整阅读原始文献。
{{< /aside >}}

　　本章主要介绍 Cocoa 与 Objective-C 的关系，以及 Cocoa 提供的重要基类 NSObject。本章也介绍了如何使用 Objective-C 从一个对象向另一个对象发送消息和接受返回值。

1 A Simple Cocoa Command-Line Tool
----------------------------------
　　以一个简单的 Cocoa 命令行工具为例。此工具将参数去重，然后按字典顺序打印出来：

```bash
localhost> SimpleCocoaTool a z c a l q m z
a
c
l
m
q
z
```

源代码如下：

```objective-c
#import <Foundation/Foundation.h>
 
int main (int argc, const char * argv[]) {
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
    NSArray *param = [[NSProcessInfo processInfo] arguments];
    NSCountedSet *cset = [[NSCountedSet alloc] initWithArray:param];
    NSArray *sorted_args = [[cset allObjects]
        sortedArrayUsingSelector:@selector(compare:)];
    NSEnumerator *enm = [sorted_args objectEnumerator];
    id word;
    while (word = [enm nextObject]) {
        printf("%s\n", [word UTF8String]);
    }
 
    [cset release];
    [pool release];
    return 0;
}
```

此代码使用一个自动释放池（NSAutoreleasePool）管理内存，使用数组（NSArray）和集合（NSCountedSet） 去重和排序，使用迭代器（NSEnumerator） 遍历排序后的数组并打印。

　　本章接下来的部分介绍此代码中的 OC 部分。

2 Object-Oriented Programming with Objective-C
----------------------------------------------

2.1 The Objective-C Advantage
-----------------------------
　　每一个 OC 对象有一个隐藏结构体，其第一个成员是一个 `isa` 指针，指向这个对象的类。OC 中的类也是一个对象，维护一个调度表，该表主要由对象方法实现组成。类对象还有一个指向超类的指针。根据这个引用链，对象可以访问他的所有方法和从超类继承的公开/保护方法（访问权限为 `public` 或 `protect`）。

2.2 The Dynamism of Objective-C
-------------------------------
　　Objective-C 是一个动态性很强的语言，很多符号解析工作从编译和链接移到了运行时。OC 有以下动态性：
- Dynamic typing——运行时才确定对象的类
- Dynamic binding——运行时才确定要调用的方法
- Dynamic loading——运行时向程序添加新的模块

　　OC 引入一个新的类型 `id` 表示任何 Cocoa 对象。下面的代码中，`word` 是 `id` 类型的变量：

```objective-c
id word;
while (word = [enm nextObject]) {
    // do something with 'word' variable....
}
```

　　`id` 类型的对象在运行时确定实际的类。
　　动态类型为动态绑定提供基础。动态绑定将决定调用哪一个方法推迟到运行时，即只有自消息到达对象时才确定调用哪个方法。当你向一个动态类型对象发送消息时，运行时根据对象的 `isa` 指针定位对象的类，然后从中查找要调用的方法。
　　有了动态加载，程序无需在启动时一口气载入所有的代码和资源。代码和本地化的资源以 bundle 的形式打包，通过 `NSBundle` 类的方法载入程序中。懒加载减少了程序的内存占用，使程序易于扩展（设计插件）。

2.3 Extensions to the Objective-C Language
------------------------------------------
　　OC 提供四种扩展机制：categories，protocols，declared properties 和 fast enumeration。

2.3.1 Categories
----------------
　　Categories 类似于 swift 中的 extension，可以直接给类增加方法。在运行时增加的方法与类自己的方法没有区别。类和子类的对象都可以调用 category 中的方法。定义 Category 与定义子类类似，category 名位于 `@interface` 或 `@implementation` 指令后，用小括号包裹。例如，为 `NSArray` 增加一个 `prettyPrintDescription` 方法：

```objective-c
// PrettyPrintCategory.h
#import <Foundation/NSArray.h> // if Foundation not already imported
 
@interface NSArray (PrettyPrintElements)
- (NSString *)prettyPrintDescription;
@end
```

```objective-c
#import “PrettyPrintCategory.h”
 
@implementation NSArray (PrettyPrintElements)
- (NSString *)prettyPrintDescription {
    // implementation code here...
}
@end
```

　　Category 不能用于添加新的实例属性。Category 可以覆盖现有方法，但是由于不是继承，无法通过 `super` 调用原始实现。你可以用 category 为 `NSObject` 类添加方法，但是这会导致程序运行时所有对象都能调用你添加的方法。

2.3.2 Protocols{#2.3.2}
-----------------------
　　OC 中的 protocol 与 Java 中的 interface 类似。Protocol 中定义一系列公开的方法，可用于隐藏类型。实现了相同协议的对象可以在不知道对方类的情况下根据协议发送消息。

{{< aside >}}
**批注**
　　Swift 中的 protocol 还可以定义计算属性。
{{< /aside >}}

　　Protocol 分为 formal 和 informal 两类。Informal protocol 是 `NSObject` 的 category，因此所有对象都隐含遵守 informal protocol。由于所有的对象都继承了 category 提供的默认实现，对象可以选择性的实现协议规定的方法。实现方法时应保证对 `respondsToSelector:` 消息返回 `true`。调用 informal protocol 定义的方法前应当首先发送 `respondsToSelector:` 消息检查对方是否实现了方法。


2.10 Creating a Singleton Instance{#2.10}
-----------------------------------------
见[原文](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW32)。


## 修订记录
### 2022-05-12T16:10:13+08:00
* 创建。
### 2022-05-12T18:42:04+08:00
* 阅读到 [Informal Protocols](#2.3.2)。