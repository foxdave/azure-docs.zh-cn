---
title: 业务连续性计划 - QnA Maker
titleSuffix: Azure Cognitive Services
description: 业务连续性计划的主要目标是创建弹性知识库终结点，确保机器人或应用程序使用该终结点时不会发生停机。
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 41e7425a2e2e6dd8dc8416538cf77e5b8f273284
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "47041929"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>为 QnA Maker 服务创建业务连续性计划

业务连续性计划的主要目标是创建弹性知识库终结点，确保机器人或应用程序使用该终结点时不会发生停机。

![QnA Maker bcp 计划](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

上述高级想法如下：

1. 在 [Azure 配对区域](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)中设置两个并行 [QnA Maker 服务](../How-To/set-up-qnamaker-service-azure.md)。

2. 使主要和辅助 Azure 搜索索引保持同步。使用[此处](https://github.com/pchoudhari/QnAMakerBackupRestore)的 github 示例了解如何备份 - 还原 Azure 索引。

3. 使用[连续导出](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)备份 Application Insights。

4. 主要和辅助堆栈设置完成后，使用[流量管理器](https://docs.microsoft.com/azure/traffic-manager/)配置两个终结点并设置路由方法。

5. 需要创建流量管理器终结点的 SSL 证书。 在应用服务中[绑定 SSL 证书](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)。

6. 最后，在机器人或应用中使用流量管理器终结点。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [为 QnA Maker 开发选择容量](../Tutorials/choosing-capacity-qnamaker-deployment.md)
