---
title: Azure Cosmos DB 中的高可用性
description: 本文介绍 Azure Cosmos DB 如何提供高可用性
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 5dc43aa5b98f097bd8bdf927f40b2d3efc878d48
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283489"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Azure Cosmos DB 中的高可用性

Azure Cosmos DB 以透明方式在与 Cosmos 帐户关联的所有 Azure 区域之间复制数据。 Cosmos DB 对数据采用多层冗余，如下图所示：

![资源分区](./media/high-availability/figure1.png) 

- Cosmos 容器中的数据已水平分区。

- 在每个区域中，每个分区受副本集的保护，该副本集中的大多数副本将复制并以持久方式提交所有写入内容。 副本分布在最多 10 到 20 个容错域中。

- 将复制所有区域中的每个分区。 每个区域包含某个 Cosmos 容器的所有数据分区，可接受写入并为读取提供服务。  

如果 Cosmos 帐户分布在 N 个 Azure 区域之间，则所有数据至少有 N x 4 个副本。 除了在与 Cosmos 帐户关联的区域之间提供低延迟数据访问和缩放写入/读取吞吐量以外，部署更多的区域（N 值较高）还能提高可用性。  

## <a name="slas-for-availability"></a>可用性 SLA   

作为全局分布式数据库，Cosmos DB 提供综合性的 SLA，涵盖了吞吐量、99% 时间内的延迟、一致性和高可用性。 下表描述了 Cosmos DB 为单区域和多区域帐户提供的高可用性保证。 为实现高可用性，请将 Cosmos 帐户配置为使用多个写入区域。

|操作类型  | 单区域 |多区域（单区域写入）|多区域（多区域写入） |
|---------|---------|---------|-------|
|写入    | 99.99    |99.99   |99.999|
|读取     | 99.99    |99.999  |99.999|

> [!NOTE]
> 在实践中，有限过期、会话、一致前缀和最终一致性模型的实际写入可用性明显高于发布的 SLA。 所有一致性级别的实际读取可用性明显高于发布的 SLA。

## <a name="regional-outages"></a>区域性服务中断

区域性服务中断并不少见，而 Azure Cosmos DB 可确保你的数据库始终可用。 下面根据 Cosmos 帐户配置详细汇总了 Cosmos DB 在服务中断期间的行为： 

- 配置为使用多个写入区域的多区域帐户将保持很高的读写可用性。 区域性故障转移可在瞬间完成，不需要在应用程序中进行任何更改。

- 配置为使用单个写入区域的多区域帐户：在发生写入区域服务中断期间，这些帐户将保持很高的读取可用性。 但是，对于写入，必须在 Cosmos 帐户中“启用自动故障转移”，才能将受影响区域故障转移到关联的另一区域。 故障转移将按指定的区域优先级进行。 最终，当受影响的区域重新联机时，将通过冲突源提供在服务中断期间保留在受影响写入区域中的未复制数据。 应用程序可以读取冲突源，根据应用程序特定的逻辑解决冲突，并相应地将更新后的数据写回 Cosmos 容器。 以前受影响的写入区域恢复后，它将自动用作读取区域。 可以调用手动故障转移，并将受影响的区域恢复为写入区域。 可以使用 [Azure CLI 或 Azure 门户](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account)执行手动故障转移。  

- 配置为使用单个写入区域的多区域帐户：在发生读取区域服务中断期间，这些帐户将保持很高的读写可用性。 受影响的区域将自动从写入区域断开连接，并标记为脱机。 Cosmos DB SDK 会将读取调用重定向到首选区域列表中的下一个可用区域。 如果首选区域列表中没有区域可用，则会自动让调用返回到当前的写入区域。 处理读取区域服务中断不需要对应用程序代码进行更改。 最终，当受影响区域重新联机时，以前受影响的读取区域将自动与当前写入区域同步，并再次可用于为读取请求提供服务。 后续的读取会重定向到恢复的区域，不需更改应用程序代码。 在故障转移和重新加入以前发生故障的区域期间，Cosmos DB 会持续遵循读取一致性保证。

- 发生区域性服务中断时，单区域帐户可能会失去可用性。 建议对 Cosmos 帐户至少设置两个区域（最好是至少设置两个写入区域），以确保始终保持高可用性。

## <a name="building-highly-available-applications"></a>生成高可用性应用程序

- 为确保较高的写入和读取可用性，请将 Cosmos 帐户配置为跨越多个写入区域中的至少两个区域。 这种配置可确保读取和写入操作获得有 SLA 作为保障的可用性、最低延迟和可伸缩性。 详细了解如何[将 Cosmos 帐户配置为使用多个写入区域](tutorial-global-distribution-sql-api.md)。

- 对于配置为使用单个写入区域的多区域 Cosmos 帐户，请[使用 Azure CLI 或 Azure 门户中启用自动故障转移](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account)。 启用自动故障转移后，每当发生区域性灾难时，Cosmos DB 都会自动故障转移你的帐户。  

- 即使 Cosmos 帐户具有高可用性，应用程序也不一定能够正常保持高可用性。 为确保应用程序的端到端高可用性，请在应用程序测试或灾难恢复 (DR) 演练过程中，定期[使用 Azure CLI 或 Azure 门户调用手动故障转移](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account)。 

## <a name="next-steps"></a>后续步骤

接下来，可通过以下文章了解如何缩放吞吐量：

* [缩放吞吐量](scaling-throughput.md)

* [各种一致性级别的可用性和性能权衡](consistency-levels-tradeoffs.md)

* [全局缩放预配的吞吐量](scaling-throughput.md)

* [全球分布 - 揭秘](global-dist-under-the-hood.md)

* [Azure Cosmos DB 中的一致性级别](consistency-levels.md)


