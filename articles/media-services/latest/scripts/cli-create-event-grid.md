---
title: Azure CLI 脚本示例 - 创建 Azure 事件网格订阅 | Microsoft Docs
description: 本主题中的 Azure CLI 脚本演示如何为作业状态更改创建一个帐户级别的事件网格订阅。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: eae9947b7dcbd6f2c52fb0d4abc65375aed7766e
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378782"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>CLI 示例：创建 Azure 事件网格订阅 

本文中的 Azure CLI 脚本演示如何为作业状态更改创建一个帐户级别的事件网格订阅。

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

如果选择在本地安装并使用 CLI，本文要求运行 Azure CLI 2.0.20 或更高版本。 运行 `az --version` 即可查找版本。 如需进行安装或升级，请参阅[安装 Azure CLI](/cli/azure/install-azure-cli)。 

## <a name="example-script"></a>示例脚本

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>后续步骤

有关详细示例，请参阅 [Azure CLI 示例](../cli-samples.md)。
