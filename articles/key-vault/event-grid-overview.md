---
title: 教程：通过 Azure 事件网格监视 Key Vault
description: 教程：使用 Azure 事件网格订阅 Key Vault 事件
services: media-services
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 5771af365b763d2152eea4ef4f662e08769b378c
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2019
ms.locfileid: "74133347"
---
# <a name="tutorial-monitoring-key-vault-with-azure-event-grid-preview"></a>教程：通过 Azure 事件网格监视 Key Vault（预览版）

与事件网格的 Key Vault 集成目前处于预览阶段。 用户可以通过它在 Key Vault 中存储的机密状态发生更改时收到通知。 状态更改定义为即将过期的机密（在 30 天内到期）、已过期的机密或具有可用新版本的机密。 支持所有三种机密类型（密钥、证书和机密）的通知。

应用程序可以响应这些使用新式无服务器体系结构的事件，无需采用复杂代码或昂贵低效的轮询服务。 可以通过 [Azure 事件网格](https://azure.microsoft.com/services/event-grid/)向事件处理程序（如 [Azure Functions](https://azure.microsoft.com/services/functions/)、[Azure 逻辑应用](https://azure.microsoft.com/services/logic-apps/)），甚至是向自己的 Webhook 推送事件，且仅需为使用的内容付费。 有关定价的详细信息，请参阅[事件网格定价](https://azure.microsoft.com/pricing/details/event-grid/)。

## <a name="key-vault-events-and-schemas"></a>Key Vault 事件和架构

事件网格使用[事件订阅](../event-grid/concepts.md#event-subscriptions)将事件消息路由到订阅方。 Key Vault 事件包含响应数据更改所需的所有信息。 可以识别 Key Vault 事件，因为 eventType 属性以“Microsoft.KeyVault”开头。

有关详细信息，请参阅 [Key Vault 事件架构](../event-grid/event-schema-key-vault.md)。

> [!NOTE]
> 仅在设置订阅后创建的机密版本（所有三个类型）才会触发事件。
>
> 对于现有的机密，必须生成新的版本。

## <a name="practices-for-consuming-events"></a>使用事件的做法

处理 Key Vault 事件的应用程序应遵循以下建议的做法：

* 可以配置多个订阅，将事件路由至同一事件处理程序。 重要的是不要假设事件来自特定源，但要检查消息的主题，以确保它来自于你所期望的 Key Vault。
* 同样，检查 eventType 是否为准备处理的项，并且不假定所接收的全部事件都是期望的类型。
* 忽略不了解的字段。  此做法有助于适应将来可能添加的新功能。
* 使用“subject”前缀和后缀匹配项，将事件限制为特定事件。

## <a name="next-steps"></a>后续步骤

- [Azure Key Vault 概述](key-vault-overview.md)
- [Azure 事件网格概述](../event-grid/overview.md)
- 如何：[将 Key Vault 事件路由到自动化 Runbook（预览版）](event-grid-tutorial.md)。
- 如何：[Key Vault 机密发生更改时接收电子邮件](event-grid-logicapps.md)
- [Azure Key Vault 的 Azure 事件网格事件架构（预览版）](../event-grid/event-schema-key-vault.md)
- [Azure 自动化概述](../automation/index.yml)
