---
title: Azure Service Fabric sfctl 网格卷
description: 了解 sfctl，Azure Service Fabric 命令行界面。 包含用于获取和删除卷资源的命令的列表。
author: jeffj6123
ms.topic: reference
ms.date: 9/17/2019
ms.author: jejarry
ms.openlocfilehash: e77c98bf384278b0bf27bb0f30f425375093ffab
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645304"
---
# <a name="sfctl-mesh-volume"></a>sfctl mesh volume
获取和删除卷资源。

## <a name="commands"></a>命令

|命令|Description|
| --- | --- |
| delete | 删除卷资源。 |
| list | 列出所有卷资源。 |
| show | 获取具有给定名称的卷资源。 |

## <a name="sfctl-mesh-volume-delete"></a>sfctl mesh volume delete
删除卷资源。

删除由名称标识的卷资源。

### <a name="arguments"></a>参数

|参数|Description|
| --- | --- |
| --name -n [必需] | 卷的名称。 |

### <a name="global-arguments"></a>全局参数

|参数|Description|
| --- | --- |
| --debug | 提高日志记录详细程度以显示所有调试日志。 |
| --help -h | 显示此帮助消息并退出。 |
| --output -o | 输出格式。  允许的值\: json、jsonc、table、tsv。  默认值\: json。 |
| --query | JMESPath 查询字符串。 有关详细信息和示例，请参阅 http\://jmespath.org/。 |
| --verbose | 提高日志记录详细程度。 使用 --debug 获取完整的调试日志。 |

## <a name="sfctl-mesh-volume-list"></a>sfctl mesh volume list
列出所有卷资源。

获取给定资源组中所有卷资源的相关信息。 此信息包括卷的说明和其他属性。

### <a name="global-arguments"></a>全局参数

|参数|Description|
| --- | --- |
| --debug | 提高日志记录详细程度以显示所有调试日志。 |
| --help -h | 显示此帮助消息并退出。 |
| --output -o | 输出格式。  允许的值\: json、jsonc、table、tsv。  默认值\: json。 |
| --query | JMESPath 查询字符串。 有关详细信息和示例，请参阅 http\://jmespath.org/。 |
| --verbose | 提高日志记录详细程度。 使用 --debug 获取完整的调试日志。 |

## <a name="sfctl-mesh-volume-show"></a>sfctl mesh volume show
获取具有给定名称的卷资源。

获取具有给定名称的卷资源的相关信息。 此信息包括卷的说明和其他属性。

### <a name="arguments"></a>参数

|参数|Description|
| --- | --- |
| --name -n [必需] | 卷的名称。 |

### <a name="global-arguments"></a>全局参数

|参数|Description|
| --- | --- |
| --debug | 提高日志记录详细程度以显示所有调试日志。 |
| --help -h | 显示此帮助消息并退出。 |
| --output -o | 输出格式。  允许的值\: json、jsonc、table、tsv。  默认值\: json。 |
| --query | JMESPath 查询字符串。 有关详细信息和示例，请参阅 http\://jmespath.org/。 |
| --verbose | 提高日志记录详细程度。 使用 --debug 获取完整的调试日志。 |


## <a name="next-steps"></a>后续步骤
- [安装](service-fabric-cli.md) Service Fabric CLI。
- 了解如何通过[示例脚本](/azure/service-fabric/scripts/sfctl-upgrade-application)使用 Service Fabric CLI。