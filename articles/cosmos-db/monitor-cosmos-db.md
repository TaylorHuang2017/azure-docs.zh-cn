---
title: 监视 Azure Cosmos DB |Microsoft Docs
description: 了解如何监视 Azure Cosmos DB 的性能和可用性。
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: c166811bbfd27691f9a01a944d304d06560b0232
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75445178"
---
# <a name="monitoring-azure-cosmos-db"></a>监视 Azure Cosmos DB
如果你有依赖于 Azure 资源的关键应用程序和业务流程，则需要监视这些资源的可用性、性能和操作。 本文介绍 Azure Cosmos 数据库生成的监视数据，以及如何使用 Azure Monitor 的功能对此数据进行分析和发出警报。

## <a name="what-is-azure-monitor"></a>说明是 Azure Monitor？
Azure Cosmos DB 使用[Azure Monitor](../azure-monitor/overview.md)创建监视数据，该服务是 azure 中的一个完整 stack 监视服务，它提供了一组完整的功能来监视 azure 资源以及其他云和本地的资源。 

如果你尚不熟悉如何监视 Azure 服务，请从[通过 Azure Monitor 来监视 azure 资源](../azure-monitor/insights/monitor-azure-resource.md)一文中了解以下内容：

- 说明是 Azure Monitor？
- 与监视相关的成本
- 监视 Azure 中收集的数据
- 配置数据收集
- Azure 中用于分析和警报监视数据的标准工具

以下各节将介绍从 Azure Cosmos DB 收集的特定数据，并提供有关使用 Azure tools 配置数据收集和分析此数据的示例。

## <a name="azure-monitor-for-cosmos-db-preview"></a>Cosmos DB 的 Azure Monitor （预览）
[Azure Cosmos DB Azure Monitor](../azure-monitor/insights/cosmosdb-insights-overview.md)基于[Azure Monitor 的工作簿功能](../azure-monitor/app/usage-workbooks.md)，并使用为以下部分中所述的 Cosmos DB 收集的相同监视数据。 使用此工具可查看采用统一交互式体验的所有 Azure Cosmos DB 资源的总体性能、故障、容量和操作运行状况，并利用 Azure Monitor 的其他功能进行详细分析和发出警报。 

![Cosmos DB 的 Azure Monitor](media/monitor-cosmos-db/azure-monitor-cosmos-db.png)

## <a name="monitoring-data-collected-from-azure-cosmos-db"></a>监视从 Azure Cosmos DB 收集的数据

Azure Cosmos DB 收集相同种类的监视数据，如[监视 Azure 资源](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data)中的数据中所述的其他 azure 资源。 有关 Azure Cosmos DB 创建的日志和指标的详细参考信息，请参阅[Azure Cosmos DB 监视数据参考](monitor-cosmos-db-reference.md)。

每个 Azure Cosmos 数据库的 Azure 门户中的 "**概述**" 页包含数据库使用情况的简要视图，包括请求和每小时计费。 这是有用的信息，但只提供少量的监视数据。 在您创建数据库时，会自动收集这些数据，并可进行分析，同时可以使用某些配置启用其他数据收集。

![概述页](media/monitor-cosmos-db/overview-page.png)

## <a name="analyzing-metric-data"></a>分析指标数据

Azure Cosmos DB 提供了使用指标的自定义体验。 有关使用此体验和分析不同的 Azure Cosmos DB 方案的详细信息，请参阅[监视和调试 Azure Monitor 中的 Azure Cosmos DB 度量](cosmos-db-azure-monitor-metrics.md)。

可以通过在 " **Azure Monitor** " 菜单中打开 "**指标**"，使用指标资源管理器从其他 Azure 服务中的指标 Azure Cosmos DB 分析指标。 有关使用此工具的详细信息，请参阅[Azure 指标资源管理器](../azure-monitor/platform/metrics-getting-started.md)入门。 Azure Cosmos DB 的所有指标都在命名空间**Cosmos DB 标准指标**中。 向图表添加筛选器时，可以使用以下维度和这些指标：

- CollectionName
- DatabaseName
- OperationType
- 地区
- StatusCode


## <a name="analyzing-log-data"></a>分析日志数据
Azure Monitor 日志中的数据存储在表中，每个表都具有自己的唯一属性集。 Azure Cosmos DB 将数据存储在下表中。

| 表 | Description |
|:---|:---|
| AzureDiagnostics | 多个服务用来存储资源日志的公用表。 Azure Cosmos DB 中的资源日志可通过 `MICROSOFT.DOCUMENTDB`标识。   |
| AzureActivity    | 用于存储活动日志中所有记录的公用表。 


> [!IMPORTANT]
> 从 "Azure Cosmos DB" 菜单中选择 "**日志**" 时，会打开 Log Analytics 并将查询范围设置为当前的 Azure Cosmos 数据库。 这意味着日志查询只包含来自该资源的数据。 如果要运行的查询包含来自其他数据库或其他 Azure 服务的数据，请从 " **Azure Monitor** " 菜单中选择 "**日志**"。 有关详细信息，请参阅[Azure Monitor Log Analytics 中的日志查询范围和时间范围](../azure-monitor/log-query/scope.md)。

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Cosmos DB Azure Monitor 中的 Log Analytics 查询

下面是一些可在**日志搜索**搜索栏中输入的查询，可帮助你监视 Azure Cosmos 容器。 这些查询使用[新语言](../log-analytics/log-analytics-log-search-upgrade.md)。

以下是可用于帮助监视 Azure Cosmos 数据库的查询。

* 若要查询指定时间段内来自 Azure Cosmos DB 的所有诊断日志，请执行以下操作：

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"

    ```

* 查询最近记录的 10 个事件：

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | limit 10
    ```

* 查询已按操作类型分组的所有操作：

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName
    ```

* 查询按资源分组的所有操作：

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* 查询已按资源分组的所有用户活动：

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```
* 如果为，则从**DataPlaneRequests**和**QueryRunTimeStatistics**中的数据联接大于 100 ru 的所有查询。

    ```Kusto
    AzureDiagnostics
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" and todouble(requestCharge_s) > 100.0
    | project activityId_g, requestCharge_s
    | join kind= inner (
           AzureDiagnostics
           | where ResourceProvider =="MICROSOFT.DOCUMENTDB" and Category == "QueryRuntimeStatistics"
           | project activityId_g, querytext_s
    ) on $left.activityId_g == $right.activityId_g
    | order by requestCharge_s desc
    | limit 100
    ```

* 查询哪些操作花费的时间超过 3 毫秒：

    ```Kusto
    AzureDiagnostics 
    | where toint(duration_s) > 3 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* 查询哪些代理正在运行操作：

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | summarize count() by OperationName, userAgent_s
    ```

* 查询何时执行了长时间运行的操作：

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" 
    | project TimeGenerated , duration_s 
    | summarize count() by bin(TimeGenerated, 5s)
    | render timechart
    ```
    
* 获取分区键统计信息，以便评估数据库帐户的前3个分区之间的偏差：

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="PartitionKeyStatistics" 
    | project SubscriptionId, regionName_s, databaseName_s, collectionname_s, partitionkey_s, sizeKb_s, ResourceId 
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a>以编程方式监视 Azure Cosmos DB
门户中提供的帐户级指标（如帐户存储使用率和总请求数）不可通过 SQL API 获得。 但是，可以使用 SQL API 在集合级别检索使用情况数据。 若要检索集合级别的数据，请执行以下操作：

* 若要使用 REST API，请[对集合执行 GET](https://msdn.microsoft.com/library/mt489073.aspx)。 集合的配额和使用情况信息将返回到响应中的 x-ms-resource-quota 和 x-ms-resource-usage 标头中。
* 要使用 .NET SDK，请使用 [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx) 方法，它将返回 [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx)，其中包含大量使用情况属性，例如 **CollectionSizeUsage**、**DatabaseUsage**、**DocumentUsage** 等。

若要访问其他指标，请使用 [Azure Monitor SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights)。 可以通过调用以下命令来检索可用的指标定义：

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metricDefinitions?api-version=2015-04-08

用于检索各个指标的查询使用以下格式：

    https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/metrics?api-version=2015-04-08&$filter=%28name.value%20eq%20%27Total%20Requests%27%29%20and%20timeGrain%20eq%20duration%27PT5M%27%20and%20startTime%20eq%202016-06-03T03%3A26%3A00.0000000Z%20and%20endTime%20eq%202016-06-10T03%3A26%3A00.0000000Z



## <a name="next-steps"></a>后续步骤

- 有关 Azure Cosmos DB 创建的日志和指标的参考，请参阅[Azure Cosmos DB 监视数据参考](monitor-cosmos-db-reference.md)。
- 有关监视 Azure 资源的详细信息，请参阅[监视 azure 资源的 Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md) 。
