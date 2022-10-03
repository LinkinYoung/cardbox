---
title      : "What is Cocoa?"
description: 介绍 Cocoa 的功能、框架和开发环境。
date       : 2022-05-11T14:54:52+08:00
lastmod    : 2022-05-12T13:31:51+08:00

Sections:
  - Title: 链接区域
    Groups:
    - Title: 下一页
      Links:
      - Type: Page
        URL : REF21C2
    - Title: 所属书籍
      Links:
      - Type: Page # 这是一个指向首页的卡片链接。
        URL : REF21
    - Title: 索引
      Links:
      - Type: Page # 这是一个指向首页的卡片链接。
        URL : /
---
章节链接: https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/WhatIsCocoa/WhatIsCocoa.html#//apple_ref/doc/uid/TP40002974-CH3-SW16

{{< aside>}}
**文献卡片备注**
　　此卡片为文献卡片。文献卡片使用精炼的语言总结原始文献的内容，并可能带有指向其他卡片的链接。文献卡片不追求完整还原原始文献，建议读者结合此类卡片完整阅读原始文献。
{{< /aside >}}

## 1 The Cocoa Environment
　　Cocoa 是一个面向对象的框架，为 OS X 和 iOS 应用提供运行环境。OS X 上的 Carbon 是面向过程的框架。

## 1.1 Introducing Cocoa
　　小知识：Cocoa 发布于 1989 年，最初是 NextSTEP。
　　Cocoa 框架主要配合 Objective-C 语言使用。OC 是 ANSI C 的超集，扩展了面向对象编程的语法。小知识：这些语法从 Smalltalk 语言发展而来。见本章第五节 [A Bit of History](#5)。
　　Cocoa 最重要的两个类库分别是 Foundation 和 AppKit/UIKit。

1.2 How Cocoa Fits into OS X
----------------------------
 　　OS X 层级结构（自顶向下）：
- User Experience
  Aqua, Dashboard, Spotlight, Accessibility
- Application Frameworks
  Carbon, Java, AppKit
- Graphics and Media (Application Services)
  Cora Animation, Core Image, Core Video, QuickTime, OpenGL, Quartz, Core Audio
- Core Services
  Carbon Core, Launch Service, Core Foundation, Foundation
- Darwin

 　　Foundation 框架定义对象的基本行为，提供对象管理机制，提供基本类、集合类和系统服务对象。Foundation 框架相当于 Core Foundation 框架的面向对象版。

2 Features of a Cocoa Application
---------------------------------
 　　这一节介绍了 Cocoa 为应用程序开发提供的便利。具体内容请见[原文](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/WhatIsCocoa/WhatIsCocoa.html#//apple_ref/doc/uid/TP40002974-CH3-SW6)。

3 The Development Environment
-----------------------------

3.3 Xcode
---------
 　　竟然还有 Objective-C++ 这种语言！当时的 Xcode 界面看起来与现在也大不相同。当时似乎还没有 Git？
　　编译工具的细节见 [Xcode Build System Guide](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/XcodeBuildSystem/000-Introduction/Introduction.html#//apple_ref/doc/uid/TP40006904)。

4 The Cocoa Frameworks
----------------------
　　这一节大致介绍了 Cocoa 框架中的主要框架和类。Cocoa 应用使用一个共同的根类——NSObject。尽管 Foundation 框架提供了另一个根类——NSProxy，但是很少用在 Cocoa 程序中。在 OC 中也可以定义自己的根类，不过需要编写大量与 OC 运行时交互的代码。

4.1 Foundation
--------------
4.1.1 Foundation Paradigms and Policies
　　Foundation 框架包含一些范式和策略。
* **类的可变版本**：许多值类和容器类都有一个可变的版本，可变类永远是不可变类的子类。
* **类簇**：类簇是一个抽象类和多个具体类，抽象类用作接口。根据上下文，特定的具体类会被使用。
  {{<aside>}}
  **批注**
  看起来与 Java 的抽象类类似。
  {{</aside>}}
* **通知**：这是 Cocoa 中的一个主要设计模式。依赖广播机制，让对象（观察者）知道另一个对象在做什么或者发生了什么系统事件。发出消息的对象可以不知道观察者的信息。有三种通知：同步通知、异步通知、分布通知。Foundation 的通知机制通过这几个类实现：`NSNotification`、`NSNotificationCenter`、`NSnotificationQueue`、`NSDistributedNotificationCenter`。

4.1.2 Foundation Classes
------------------------
4.1.2.1 Value Objects
---------------------
　　值对象包装基本数据类型（包括字符串、整型、浮点类型、日期、结构体和指针）的值。当比较两个值对象时比较的是其存储的值而非指针值。值对象常常用作其他对象的属性。除了值对象外，你也可以直接使用标量。

4.1.2.2 集合
----------
　　NSArray、NSDictionary、NSSet。

4.2 AppKit (OS X){#4.2}
-----------------
 　　这一节主要介绍以下内容：
4.2.1 Overview of the AppKit Framework
4.2.2 General User-Interface Classes
4.2.3 Text and Fonts
4.2.4 Graphics and Colors
4.2.5 Printing and Faxing
4.2.6 Document and File-System Support
4.2.7 Internationalization and Character Input Support
4.2.8 Operating-System Services
4.2.9 Interface Builder Support

4.3 UIKit (iOS)
---------------
 　　这一节主要介绍以下内容：
4.3.1 Overview of UIKit Classes
4.3.2 Application Coordination
4.3.3 Differences in Event and Drawing Models
4.3.4 General User-Interface Classes
4.3.5 Text

4.3.1 Overview of UIKit Classes
-------------------------------
 　　与 AppKit 类似，大部分处理事件的 UIKit 的对象继承 UIRensonder。与 AppKit 不同的是 UIKit 不使用 cell，而是在 UIControl 中指定事件类型和目标。UIKit 也使用代理模式。

4.3.2 Application Coordination
------------------------------
 　　iOS 下的应用被一个单例 UIApplication 对象管理。此对象管理 main event loop，跟踪窗口和视图，将事件分派到合适的 Responser 对象。
 　　UIApplication 对象接受系统通知和应用事件，大部分使用代理处理，例如应用启动和结束、内存不足、时间更改等。

4.3.3 Differences in Event and Drawing Models
---------------------------------------------
　　UIKit 的事件包括了触控事件。UIKit 有一个手势识别器，将一系列触控事件识别为对应的手势，然后发送给对应的目标。

4.4 Comparing AppKit and UIKit Classes
--------------------------------------
　　大部分 AppKit 与 UIKit 类名只有前缀不同。一般来说，UIKit 类比对应的 AppKit 类的方法更少。

4.5 Core Data
-------------
　　Core Data 框架管理对象图和持久化。对象图管理提供了以下功能：撤销/重做、数据验证、保证对象关系的完整性。
　　Core Data 的核心是托管对象（managed object）。一个托管对象是被 Core Data 管理的模型对象，是 `NSManagedObject` 的子类。描述托管对象的 schema 是托管数据模型，包含托管对象（实体）的描述。每个实体包含属性、关系和元数据，元数据包含实体名称和表示类。
　　托管对象上下文是一个管理对象图的对象，所有在对象图中的托管对象必须注册在一个上下文对象下。上下文对象允许应用添加和删除对象、提供撤销/重做机制。当存储托管对象时，上下文对象会验证数据。应用向上下文对象发送FetchRequest对象获取数据，FetchRequest对象包含一组查询条件（criteria）。
　　Core Data 使用持久栈（persistence stack）管理数据持久化。持久栈作为托管上下文和外部存储之间的中间层，包含两层对象，顶部是 Persistent Store Coordinator，底部是 Persistent Object Store。Persistent Object Store 管理外部存储（XML，sqlite文件等）。
　　最后，Core Data 包括一个 `NSPersistentDocument` 类作为 `NSDocument` 的子类，创建它自己的持久栈和托管上下文，将一个文档映射到一个外部存储中。

4.6 Other Frameworks with a Cocoa API
-------------------------------------
　　这部分简要介绍其他框架和对应文档的链接，包括 Sync Service、Address Book、Preference Panes、Screen Saver、WebKit、iAD、Map Kit、Event Kit、Core Motion、Core Location、Media Player。

5 A Bit of History{#5}
----------------------
　　最初的 NeXTSTEP 发布于 1989 年，是一个基于 Mach 内核的操作系统。当时还没有 Foundation，动态库被称作 “Kit”。NeXTSTEP 包括 Application Kit、Sound Kit、Music Kit 和 Display PostScript 等技术。如今的 Foundation 框架在当时是 Application Kit 中定义的函数、结构体、常量和其他类型。
　　1993 年，OpenStep 出现。OpenStep 主要将 NextStep 中的 Application Kit 和 Display PostScript 移植到 Solaris。1994 年 OpenStep API 发布，首次将 Application Kit 和 Foundation 分开，并且使用 NS 前缀。
　　1997 年 Apple 收购 NeXT Software，OpenStep 变成 “Yello Box”，随 OS X Server 发布。随着 OS X 发展，最终被命名为 Cocoa。
　　

修订记录
-------
### 2022-05-11T14:31:16+08:00
* 创建。
### 2022-05-11T17:31:02+08:00
* 读完 [4.2.1 Overview of the AppKit Framework](#4.2) 和 [5 A Bit of History](#5)。
### 2022-05-12T13:31:51+08:00
* 完成此章所有内容。
