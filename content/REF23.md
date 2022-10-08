---
title      : How to Inspect a Docker Image’s Content Without Starting a Container
description: 
date       : 2022-10-08T16:06:39+08:00
lastmod    : 2022-10-08T16:06:39+08:00

Sections:
  - Title: 链接区域
    Groups:
    - Title: 索引
      Links:
      - Type: Page
        URL : C38
      - Type: Page
        URL : /
---
文章链接: https://www.howtogeek.com/devops/how-to-inspect-a-docker-images-content-without-starting-a-container/

{{< aside>}}
**文献卡片备注**
　　此卡片为文献卡片。文献卡片使用精炼的语言总结原始文献的内容，并可能带有指向其他卡片的链接。文献卡片不追求完整还原原始文献，建议读者结合此类卡片完整阅读原始文献。
{{< /aside >}}

## 方法一：使用 create 命令创建容器但不运行

    docker create --name suspect-container suspect-image:latest
    docker export suspect-container > suspect-container.tar

## 方法二：使用 image save 命令保存镜像每一层的元数据和文件

    docker image save suspect-image:latest > suspect-image.tar

## 修订记录
2022-10-08T16:06:39+08:00
* 创建。