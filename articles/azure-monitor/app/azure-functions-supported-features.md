---
title: Azure 应用程序 Insights-Azure Functions 支持的功能
description: 适用于 Azure Functions 的 Application Insights 支持功能
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 5688bd92e63016e195726488bd9ff9ed039c5795
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2019
ms.locfileid: "74928409"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>适用于 Azure Functions 的 Application Insights 支持功能

Azure Functions 提供与 Application Insights 的[内置集成](../../azure-functions/functions-monitoring.md)功能（通过 ILogger Interface 可用）。 以下是当前受支持的功能列表。 有关[入门](../../azure-functions/functions-monitoring.md#enable-application-insights-integration)信息，请参阅 Azure Functions 指南。

## <a name="supported-features"></a>受支持的功能

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application Insights .NET SDK**   | 2\.5.0       | **2.9.1**         |
| | | | 
| **自动集合**        |                 |                   |               
| &bull; 请求                     | 是             | 是               | 
| &bull; 异常                   | 是             | 是               | 
| &bull; 性能计数器         | 是             | 是               |
| &bull; 依赖项                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | 是               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | 是               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | 是               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | 是               | 
| | | | 
| **受支持的功能**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | 是             | 是               | 
| &nbsp;&nbsp;&nbsp;&mdash; 安全控制通道|                 | 是               | 
| &bull; 采样                     | 是             | 是               | 
| &bull; 检测信号                   |                 | 是               | 
| | | | 
| **关联性**                       |                   |                   |               
| &bull; ServiceBus                     |                   | 是               | 
| &bull; EventHub                       |                   | 是               | 
| | | | 
| **可配置性**                      |                   |                   |           
| &bull; 完全可配置。<br/>请参阅 [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) 了解相应说明。<br/>请参阅 [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) 了解所有选项。               |                   | 是                   | 


## <a name="performance-counters"></a>性能计数器

性能计数器的自动收集仅适用于 Windows 计算器。


## <a name="live-metrics--secure-control-channel"></a>实时指标和安全控制通道

指定的自定义筛选器条件将发回到 Application Insights SDK 中的“实时指标”组件。 筛选器可能包含 customerID 等敏感信息。 可以使用机密 API 密钥确保通道安全。 有关说明，请参阅[确保控制通道安全](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel)。

## <a name="sampling"></a>采样

Azure Functions 默认在其配置中启用采样功能。 有关详细信息，请参阅[配置采样](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling)。

如果你的项目依赖于 Application Insights SDK 执行手动遥测跟踪，则如果你的采样配置不同于函数的采样配置，则可能会遇到奇怪的行为。 

建议使用与函数相同的配置。 使用**函数 v2**，你可以在构造函数中使用依赖关系注入来获取相同的配置：

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
