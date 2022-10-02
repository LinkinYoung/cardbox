---
title      : DocumentRevisions-100 的数据库结构
description: 
date       : 2022-10-02T21:56:56+08:00
lastmod    : 2022-10-02T21:56:56+08:00

Sections:
  - Title: 链接区域
    Groups:
    - Title: 父页面
      Links:
      - Type: Page
        URL : REF2
    - Title: 索引
      Links:
      - Type: Page
        URL : /
---
文章链接: https://arstechnica.com/gadgets/2011/07/mac-os-x-10-7/14/

{{< aside>}}
**文献卡片备注**
　　此卡片为文献卡片。文献卡片使用精炼的语言总结原始文献的内容，并可能带有指向其他卡片的链接。文献卡片不追求完整还原原始文献，建议读者结合此类卡片完整阅读原始文献。
{{< /aside >}}

文档历史记录功能自  macOS 10.7 Lion 开始提供，自动保存 NSDocument 实例的各种版本。所有文件的历史记录存储在所在卷的 .DocumentRevisions-100 隐藏目录下，内含一个 sqlite 数据库。以下是 [REF2 - Mac OS X 10.7 Lion: the Ars Technica review](ref2.html) 给出的数据库结构：

```sql
CREATE TABLE files (
    file_row_id     INTEGER PRIMARY KEY ASC,
    file_name       TEXT,
    file_parent_id  INTEGER,
    file_path       TEXT,
    file_inode      INTEGER,
    file_last_seen  INTEGER NOT NULL DEFAULT 0,
    file_status     INTEGER NOT NULL DEFAULT 1,
    file_storage_id INTEGER NOT NULL
);

CREATE TABLE generations (
    generation_id         INTEGER PRIMARY KEY ASC,
    generation_storage_id INTEGER NOT NULL,
    generation_name       TEXT NOT NULL,
    generation_client_id  TEXT NOT NULL,
    generation_path       TEXT UNIQUE,
    generation_options    INTEGER NOT NULL DEFAULT 1,
    generation_status     INTEGER NOT NULL DEFAULT 1,
    generation_add_time   INTEGER NOT NULL DEFAULT 0,
    generation_size       INTEGER NOT NULL DEFAULT 0
);

CREATE TABLE storage (
    storage_id      INTEGER PRIMARY KEY ASC AUTOINCREMENT,
    storage_options INTEGER NOT NULL DEFAULT 1,
    storage_status  INTEGER NOT NULL DEFAULT 1
);
```

## 相关内容

- 根据此结构，我整理了 [[C8｜一种数据库命名方案]]

## 修订记录
2022-03-23 19:08
* 创建

2022-10-02T21:57:49+0800
* 整理到 Hugo 卡片盒仓库中。
