---
title: include 文件
description: include 文件
services: functions
author: nzthiago
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2018
ms.author: nzthiago
ms.custom: include file
ms.openlocfilehash: 3501ff3f92ae045019df2766bbcf7fc2c3fec5b0
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2020
ms.locfileid: "75768906"
---
## <a name="timeout"></a>函数应用超时持续时间 

函数应用的超时持续时间由[主机 json](../articles/azure-functions/functions-host-json.md#functiontimeout)项目文件中的 `functionTimeout` 属性定义。 下表显示了计划和不同运行时版本的默认值和最大值（以分钟为单位）：

| 套餐 | 运行时版本 | 默认 | 最大值 |
|------|---------|---------|---------|
| 消耗 | 1.x | 5 | 10 |
| 消耗 | 2.x | 5 | 10 |
| 消耗 | 3.x | 5 | 10 |
| 应用服务 | 1.x | 无限制 | 无限制 |
| 应用服务 | 2.x | 30 | 无限制 |
| 应用服务 | 3.x | 30 | 无限制 |

> [!NOTE] 
> 无论函数应用超时设置如何，230秒是 HTTP 触发函数响应请求所需的最长时间。 这是由于[Azure 负载均衡器的默认空闲超时](../articles/app-service/faq-availability-performance-application-issues.md#why-does-my-request-time-out-after-230-seconds)。 对于较长的处理时间，请考虑使用[Durable Functions 异步模式](../articles/azure-functions/durable/durable-functions-overview.md#async-http)或[延迟实际工作，并返回立即响应](../articles/azure-functions/functions-best-practices.md#avoid-long-running-functions)。
