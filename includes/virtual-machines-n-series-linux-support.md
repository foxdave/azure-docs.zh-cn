---
title: include 文件
description: include 文件
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/08/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: c829b8d6fedaabfb9b43c6352c8188128cf36701
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "51333737"
---
## <a name="supported-distributions-and-drivers"></a>支持的分发和驱动程序

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA 驱动程序

仅下表列出的 Linux 分发中支持适用于 NC、NCv2、NCv3 和 ND 系列 VM 的 NVIDIA CUDA 驱动程序（对 NV 系列为可选项）。 本文发布时，CUDA 驱动程序信息为最新版本。 有关最新 CUDA 驱动程序，请访问 [NVIDIA](https://developer.nvidia.com/cuda-zone) 网站。 确保安装或升级到最新 CUDA 驱动程序分发软件包。 

> [!TIP]
> 作为一种在 Linux VM 上手动安装 CUDA 驱动程序的替代方法，可以部署 Azure [数据科学虚拟机](../articles/machine-learning/data-science-virtual-machine/overview.md)映像。 用于 Ubuntu 16.04 LTS 或 CentOS 7.4 的 DSVM 版本预安装 NVIDIA CUDA 驱动程序、CUDA 深度神经网络库和其他工具。

| 分发 | 驱动程序 |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 或 7.4<br/><br/> 基于 CentOS 的 7.3 或 7.4 HPC、基于 CentOS 的 7.4 HPC | NVIDIA CUDA 10.0，驱动程序分支为 R410 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID 驱动程序

Microsoft 为用作虚拟工作站或用于虚拟应用程序的 NV 和 NVv2 系列 VM，重新分发 NVIDIA GRID 驱动程序安装程序。 请仅在下表所列操作系统上的 Azure NV VM 上安装这些 GRID 驱动程序。 这些驱动程序包括 Azure 中 GRID Virtual GPU Software 的许可。 不需要设置 NVIDIA vGPU 软件许可证服务器。

| 分发 | 驱动程序 |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 或 7.4<br/><br/>基于 CentOS 的 7.3 或 7.4 | NVIDIA GRID 6.2，驱动程序分支为 R390|

> [!WARNING] 
> 在 Red Hat 产品上安装第三方软件可能会影响 Red Hat 支持条款。 请参阅 [Red Hat 知识库文章](https://access.redhat.com/articles/1067)。
>
