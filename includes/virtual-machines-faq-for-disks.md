---
title: include 文件
description: include 文件
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7e83aa69cb4099885fc45e719c812a6c92299b7a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75359898"
---
本文将对有关 Azure 托管磁盘和 Azure 高级 SSD 盘的一些常见问题进行解答。

## <a name="managed-disks"></a>托管磁盘

什么是 Azure 托管磁盘？

托管磁盘是一种通过处理存储帐户管理来简化 Azure IaaS VM 的磁盘管理的功能。 有关详细信息，请参阅[托管磁盘概述](../articles/virtual-machines/windows/managed-disks-overview.md)。

如果从现有的 VHD（80 GB）创建标准托管磁盘，需要多少费用？

从 80 GB VHD 创建的标准托管磁盘被视为下一个可用的标准磁盘大小（S10 磁盘）。 我们按 S10 磁盘定价收费。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

标准托管磁盘是否存在任何事务成本？

可以。 我们针对每个事务进行收费。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

对于标准托管磁盘，是对磁盘上的数据实际大小收费还是对磁盘的预配容量收费？

我们根据磁盘的预配容量进行收费。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

高级托管磁盘与非托管磁盘的定价有何不同？

高级托管磁盘的定价与高级非托管磁盘的定价相同。

是否可以更改托管磁盘的存储帐户类型（标准或高级）？

可以。 可以使用 Azure 门户、PowerShell 或 Azure CLI 更改托管磁盘的存储帐户类型。

是否可以使用 Azure 存储帐户中的 VHD 文件以不同的订阅创建托管磁盘？

可以。

是否可以使用 Azure 存储帐户中的 VHD 文件在不同的区域中创建托管磁盘？

不。

客户使用托管磁盘是否存在任何规模限制？

托管磁盘取消了与存储帐户相关的限制。 但是，订阅的最大限制为每个区域、每个磁盘类型 50,000 个托管磁盘。

是否可以生成托管磁盘的增量快照？

不。 当前的快照功能可提供托管磁盘的完整副本。

可用性集中的 VM 是否可以同时包含托管和非托管磁盘？

不。 可用性集中的 VM 必须全部使用托管磁盘或全部使用非托管磁盘。 创建可用性集时，可以选择要使用的磁盘类型。

托管磁盘是否是 Azure 门户中的默认选项？

可以。

是否可以创建一个空托管磁盘？

可以。 可创建空磁盘。 可独立于 VM 创建托管磁盘，例如，不需要将磁盘附加到 VM。

什么是使用托管磁盘的可用性集的支持容错域计数？

使用托管磁盘的可用性集的支持容错域计数为 2 或 3，具体取决于它所在的区域。

如何设置用于诊断的标准存储帐户？

设置 VM 诊断的专用存储帐户。

托管磁盘支持哪类基于角色的访问控制？

托管磁盘支持三个密钥默认角色：

* 所有者：可管理所有内容，包括访问权限
* 参与者：可管理除访问权限以外的所有内容
* 读者：可查看所有内容，但不能进行更改

是否可将托管磁盘复制或导出到专用存储帐户？

可以为托管磁盘生成只读共享访问签名 (SAS) URI，使用它将内容复制到专用存储帐户或本地存储。 可以通过 Azure 门户、Azure PowerShell、Azure CLI 或 [AzCopy](../articles/storage/common/storage-use-azcopy.md) 使用 SAS URI

是否可以创建托管磁盘副本？

客户可以生成托管磁盘的快照，并使用快照创建另一个托管磁盘。

是否仍支持非托管磁盘？

是的，非托管磁盘和托管磁盘均受支持。 我们建议你对新的工作负荷使用托管磁盘，并将当前的工作负荷迁移到托管磁盘。

是否可以在同一 VM 上归置非托管和托管磁盘？

不。

**如果创建 128 GB 磁盘，然后将大小增加到 130 gb （GiB），则将对下一个磁盘大小（256 GiB）收费？**

可以。

是否可以创建本地冗余存储、异地冗余存储和区域冗余存储托管磁盘？

Azure 托管磁盘当前仅支持本地冗余存储托管磁盘。

是否可以收缩或缩小托管磁盘？

不。 目前，不支持此功能。

是否可以在磁盘上中断租用？

不。 目前不支持此功能，因为租用的作用是防止磁盘在使用时被意外删除。

当使用专用（未使用系统准备工具创建或未通用化）操作系统磁盘预配 VM 时，是否可以更改计算机名称属性？

不。 无法更新计算机名称属性。 新 VM 从创建操作系统磁盘时所用的父 VM 继承该属性。 

在哪里可找到用于使用托管磁盘创建 VM 的示例 Azure 资源管理器模板？
* [使用托管磁盘的模板列表](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**基于 blob 创建磁盘时，与该源 blob 之间是否会一直保持任何现有关系？**

否，创建新磁盘时，它是该 blob 在该时间的完整独立副本，两者之间没有联系。 如果你愿意，可以在创建磁盘后删除源 blob，这对新创建的磁盘没有任何影响。

**创建托管或非托管磁盘后，是否可以对其进行重命名？**

对于托管磁盘，无法对其进行重命名。 但是，可以对非托管磁盘进行重命名，只要它当前未附加到 VHD 或 VM。

**可否在 Azure 磁盘上使用 GPT 分区？**

第1代映像只能在数据磁盘而非 OS 磁盘上使用 GPT 分区。 操作系统磁盘必须使用 MBR 分区样式。

[第2代映像](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2)可以在 OS 磁盘和数据磁盘上使用 GPT 分区。

**哪些磁盘类型支持快照？**

高级 SSD、标准 SSD 和标准 HDD 支持快照。 对于这三种磁盘类型，所有磁盘大小（包括最大为 32 TiB 的磁盘）都支持快照。 超磁盘不支持快照。

### <a name="disk-reservation"></a>磁盘保留

**什么是 Azure 磁盘保留？**
磁盘保留是提前购买一年的磁盘存储的选项，降低了总成本。

**Azure 磁盘保留提供哪些选项？**
Azure 磁盘保留提供的选项可用于从 P30 （1 TiB）到一年的时间从（1）到 P80 （32 TiB）购买高级 Ssd。 购买磁盘保留所需的最小磁盘数量没有限制。 此外，用户还可以选择以单笔预付款或按月付款的方式付款。 高级 SSD 托管磁盘不应用附加的事务开销。

保留磁盘的形式，而非容量。 换句话说，当你保留 P80 （32 TiB）磁盘时，你将收到单个 P80 磁盘，因此，你不能将该特定预订就到两个较小的 P70 （16 TiB）磁盘。 当然，您可以根据自己的需要保留任意多个磁盘，包括两个单独的 P70 （16 TiB）磁盘。

**如何对 Azure 磁盘保留计费？**
- 对于企业协议（EA）客户，Azure 货币承诺将首先用于购买 Azure 磁盘预留。 在 EA 客户已使用其所有货币承诺的情况下，仍可能会购买磁盘预留，并将对其下一次超额支付的单个支付开票。

- 对于通过 Azure.com 购买的客户，在购买时，将按 Azure 磁盘预留的完全提前支付（或月度固定付款）对文件中的信用卡收费。

**如何应用 Azure 磁盘保留？**
磁盘保留遵循类似于保留虚拟机（VM）实例的模型。 不同之处在于，无法将磁盘保留应用于不同的 Sku，而 VM 实例可以。 有关 VM 实例的详细信息，请参阅[通过 Azure 保留 VM 实例节省成本](../articles/virtual-machines/linux/prepay-reserved-vm-instances.md)。 

**能否使用通过 Azure 磁盘预留购买的数据存储跨多个区域？**
Azure 磁盘保留适用于特定区域和 SKU （如美国东部2中的 P30），因此不能在这些构造之外使用。 对于其他区域或 Sku 中的磁盘存储需求，你始终可以购买额外的 Azure 磁盘保留。

**Azure 磁盘保留过期时会发生什么情况？**
你将在过期前30天收到电子邮件通知，并在过期日期再次收到通知。 保留到期后，部署的磁盘将继续运行，并按最新的现用现[付费率](https://azure.microsoft.com/pricing/details/managed-disks/)进行计费。

## <a name="ultra-disks"></a>超级磁盘

**我应该如何将我的 ultra 磁盘吞吐量设置为？**
如果不确定如何将磁盘吞吐量设置为，则建议首先假定 IO 大小为 16 KiB，并在监视应用程序时调整性能。 公式为：以 MBps 为单位的吞吐量 = IOPS * 16/1000。

**我将我的磁盘配置为 40000 IOPS，但只看到 12800 IOPS，为何我看不到磁盘的性能？**
除了磁盘限制外，还会在 VM 级别施加 IO 限制。 请确保所使用的 VM 大小能够支持磁盘上配置的级别。 有关 VM 施加的 IO 限制的详细信息，请参阅[Azure 中 Windows 虚拟机的大小](../articles/virtual-machines/windows/sizes.md)。

**是否可以对超磁盘使用缓存级别？**
不能，超磁盘不支持其他磁盘类型支持的不同缓存方法。 将磁盘缓存设置为 "无"。

**是否可以将超磁盘附加到现有 VM？**
也许，你的 VM 必须位于支持 Ultra 磁盘的区域和可用性区域对中。 有关详细信息，请参阅[超磁盘](../articles/virtual-machines/windows/disks-enable-ultra-ssd.md)入门。

**能否使用超磁盘作为 VM 的 OS 磁盘？**
不能，仅支持将专用磁盘作为数据磁盘，并且仅支持为4K 本地磁盘。

**是否可以将现有磁盘转换为超磁盘？**
不可以，但你可以将数据从现有磁盘迁移到超磁盘。 若要将现有磁盘迁移到超磁盘，请将这两个磁盘附加到同一个 VM，并将磁盘的数据从一个磁盘复制到另一个磁盘或利用第三方解决方案进行数据迁移。

**能否为超磁盘创建快照？**
不，快照尚不可用。

**Azure 备份是否可用于超磁盘？**
不，Azure 备份支持尚不可用。

**是否可以将超磁盘附加到在可用性集中运行的 VM？**
不能，目前尚不支持。

**是否可以使用超磁盘为 Vm 启用 Azure Site Recovery？**
不，对于超磁盘，尚不支持 Azure Site Recovery。

## <a name="uploading-to-a-managed-disk"></a>上传到托管磁盘

**是否可以将数据上传到现有托管磁盘？**

不能，只能在创建**ReadyToUpload**状态的新空磁盘期间使用上传。

**如何实现上传到托管磁盘？**

创建一个将[createOption](https://docs.microsoft.com/rest/api/compute/disks/createorupdate#diskcreateoption)属性设置为 "上传[" 的托管](https://docs.microsoft.com/rest/api/compute/disks/createorupdate#creationdata)磁盘，然后将数据上传到该磁盘。

**能否在虚拟机处于上传状态时将磁盘附加到该 VM？**

不。

**是否可以在上载状态拍摄托管磁盘的快照？**

不。

## <a name="standard-ssd-disks"></a>标准 SSD 盘

Azure 标准 SSD 盘是什么？
标准 SSD 盘是受固态介质支持的标准磁盘，经过优化而作为在较低 IOPS 级别需要一致性能的工作负载的高性价比存储。

<a id="standard-ssds-azure-regions"></a>**当前支持标准 SSD 盘的区域有哪些？**
所有 Azure 区域现在都支持标准 SSD 盘。

**使用标准 SSD 时是否可以使用 Azure 备份？**
是的，Azure 备份现已可用。

如何创建标准 SSD 盘？
可以使用 Azure 资源管理器模板、SDK、PowerShell 或 CLI 创建标准 SSD 磁盘。 以下为创建标准 SSD 盘时资源管理器模板中所需的参数：

* Microsoft.Compute 的 apiVersion 必须设置为 `2018-04-01`（或更高）
* 将 managedDisk.storageAccountType 指定为 `StandardSSD_LRS`

以下示例显示了使用标准 SSD 盘的 VM 的 properties.storageProfile.osDisk 部分：

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

有关如何使用模板创建标准 SSD 盘的完整模板示例，请参阅[使用标准 SSD 数据磁盘从 Windows 映像创建 VM](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)。

是否可以将现有磁盘转换为标准 SSD？
是的，你可以。 请参阅[将 Azure 托管磁盘存储从标准转换为高级，反之亦然](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage)，以了解有关转换托管磁盘的常规指南。 此外，使用以下值将磁盘类型更新为标准 SSD。
-AccountType StandardSSD_LRS

**使用标准 SSD 盘而不使用 HDD 的好处是什么？**
与 HDD 磁盘相比，标准 SSD 磁盘提供更好的延迟、一致性、可用性和可靠性。 因此，应用程序工作负荷可以更平稳地在标准 SSD 上运行。 注意，高级 SSD 盘是适用于大多数 IO 密集型生产工作负荷的建议解决方案。

是否可将标准 SSD 用作非托管磁盘？
不可以，标准 SSD 盘仅可用作托管磁盘。

标准 SSD 磁盘是否支持“单实例 VM SLA”？
不是，标准 SSD 没有单实例 VM SLA。 将高级 SSD 磁盘用于单实例 VM SLA。

## <a name="migrate-to-managed-disks"></a>迁移到托管磁盘

**迁移对托管磁盘性能是否有影响？**

迁移涉及将磁盘从一个存储位置移动到另一个存储位置。 这会通过数据的后台副本进行协调，这可能需要几个小时才能完成，通常情况下，这可能需要几个小时，具体取决于磁盘中的数据量。 在此期间，由于一些读取可能被重定向到原始位置，所以应用程序可能会经历比平常更高的读取延迟，并且可能需要花费更长时间才能完成。 在此期间，对写入延迟没有影响。  

迁移到托管磁盘之前/之后，需要在现有的 Azure 备份服务配置中进行哪些更改？

不需要进行任何更改。

在迁移之前通过 Azure 备份服务创建的 VM 备份是否可继续工作？

是的，备份可以顺利工作。

迁移到托管磁盘之前/之后，需要在现有的 Azure 磁盘加密配置中进行哪些更改？

不需要进行任何更改。

**是否自动将现有虚拟机规模集从非托管磁盘迁移到受支持的托管磁盘？**

不。 可以使用包含非托管磁盘的旧规模集中的映像创建包含托管磁盘的新规模集。

是否可以通过迁移到托管磁盘之前创建的页 Blob 快照创建托管磁盘？

不。 可将页 Blob 快照导出为页 Blob，然后从导出的页 Blob 创建托管磁盘。

是否可将 Azure Site Recovery 保护的本地计算机故障转移到包含托管磁盘的 VM？

是的，可以选择故障转移到包含托管磁盘的 VM。

迁移是否影响 Azure Site Recovery 通过 Azure 到 Azure 复制保护的 Azure VM？

不。 使用托管磁盘的 Vm Azure Site Recovery Azure 到 Azure 保护。

是否可以迁移位于存储帐户中现在或以前已加密的 VM 的非托管磁盘迁移到托管磁盘？

是

## <a name="managed-disks-and-storage-service-encryption"></a>托管磁盘和存储服务加密

创建托管磁盘时，是否会默认启用 Azure 存储服务加密？

可以。

**默认情况下，托管磁盘上的启动卷是否已加密？**

可以。 默认情况下，所有托管磁盘都将加密，包括操作系统磁盘。

加密密钥由谁管理？

Microsoft 管理加密密钥。

是否可以为托管磁盘禁用存储服务加密？

不。

存储服务加密是否仅适用于特定区域？

不。 它适用于托管磁盘可用的所有区域。 托管磁盘适用于所有公共区域和德国。 这也适用于中国，但是，仅适用于 Microsoft 托管密钥，不适用于客户托管密钥。

如何确定我的托管磁盘是否已加密？

你可以从 Azure 门户、Azure CLI 和 PowerShell 确定托管磁盘的创建时间。 如果时间是在 2017 年 6 月 9 日之后，则磁盘已加密。

如何对 2017 年 6 月 10 日之前创建的现有磁盘加密？

自 2017 年 6 月 10 日起，写入到现有托管磁盘的新数据会自动加密。 我们还打算对现有数据进行加密，且在后台以异步方式加密。 如果必须立即对现有数据进行加密，请创建磁盘的副本。 将对新磁盘进行加密。

* [使用 Azure CLI 复制托管磁盘](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [使用 PowerShell 复制托管磁盘](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

是否已加密托管快照和映像？

可以。 2017 年 6 月 9 日之后创建的所有托管快照和映像均会自动加密。 

是否可以将 VM 的位于存储帐户且现在或以前已加密的非托管磁盘转换为托管磁盘？

是

是否同时会加密从托管磁盘或快照导出的 VHD？

不。 但如果将 VHD 从加密托管磁盘或快照导出到加密存储帐户，则会对其进行加密。 

## <a name="premium-disks-managed-and-unmanaged"></a>高级磁盘：托管和非托管

如果 VM 使用支持高级 SSD 盘的大小系列（比如 DSv2），是否可以同时附加高级和标准数据磁盘？ 

可以。

是否可以同时将高级和标准数据磁盘附加到不支持高级 SSD 盘的大小系列，例如 D、Dv2、G 或 F 系列？

不。 只可以将标准数据磁盘附加到不使用支持高级 SSD 盘的大小系列的 VM。

如果从现有的 VHD (80 GB) 创建高级数据磁盘，需要多少费用？

从 80 GB VHD 创建的高级数据磁盘被视为下一个可用的高级磁盘大小（P10 磁盘）。 我们按 P10 磁盘定价收费。

使用高级 SSD 盘时是否存在事务成本？

每个磁盘大小都有固定成本，其根据 IOPS 和吞吐量的特定限制进行预配。 其他成本包括出站带宽和快照容量（如果适用）。 有关详细信息，请参阅[定价页](https://azure.microsoft.com/pricing/details/storage)。

可从磁盘缓存获取的 IOPS 和吞吐量限制是多少？

DS 系列的缓存和本地 SSD 合并限制是每个核心 4,000 IOPS，以及每个核心每秒 33 MiB。 GS 系列提供每个核心 5,000 IOPS，每个核心每秒 50 MiB。

托管磁盘 VM 是否支持本地 SSD？

本地 SSD 是托管磁盘 VM 随附的临时存储。 临时存储不需要额外的成本。 建议不要使用此本地 SSD 来存储应用程序数据，因为这些数据不会永久保存在 Azure Blob 存储中。

在高级磁盘上使用 TRIM 是否有任何影响？

在高级或标准磁盘的 Azure 磁盘上使用 TRIM 没有负面影响。

## <a name="new-disk-sizes-managed-and-unmanaged"></a>新磁盘大小：托管和非托管

**哪些区域支持适用的高级 SSD 磁盘大小的突发功能？**

目前，Azure 美国西部支持突发功能。

**哪些区域是在中支持的 4/8/16 GiB 托管磁盘大小（P1/P2/P3、E1/E2/E3）？**

Azure 美国西部当前支持这些新的磁盘大小。

**非托管磁盘或页 blob 是否支持 P1/P2/P3 磁盘大小？**

不能，仅在高级 SSD 托管磁盘上受支持。 

**非托管磁盘或页 blob 是否支持 E1/E2/E3 磁盘大小？**

不能，任何大小的标准 SSD 托管磁盘不能用于非托管磁盘或页 blob。

**操作系统和数据磁盘支持的最大托管磁盘大小是多少？**

Azure 支持的操作系统磁盘的分区类型是主启动记录 (MBR)。 MBR 格式支持的磁盘最大大小为 2 TiB。 Azure 支持的操作系统磁盘的最大大小为 2 TiB。 Azure 支持的托管数据磁盘最大大小为 32 TiB。

**操作系统和数据磁盘支持的最大非托管磁盘大小是多少？**

Azure 支持的操作系统磁盘的分区类型是主启动记录 (MBR)。 MBR 格式支持的磁盘最大大小为 2 TiB。 Azure 支持的操作系统非托管磁盘的最大大小为 2 TiB。 Azure 支持的非托管数据磁盘最大大小为 4 TiB。

支持的最大页 blob 大小是多少？

Azure 支持的最大页 blob 大小是 8 TiB (8,191 GiB)。 附加到 VM 作为数据或操作系统磁盘时，最大页 blob 大小为 4 TiB (4,095 GiB)。

**是否需要使用新版本的 Azure 工具来创建、附加、上传大于 1 TiB 的磁盘并重设其大小？**

无需升级现有 Azure 工具即可创建、附加大于 1 TiB 的磁盘或重设其大小。 若要直接从本地将 VHD 文件作为页 blob 或非托管磁盘上传到到 Azure，需要使用下方列出的最新工具集。 我们仅支持最大大小为 8 TiB 的 VHD 上传。

|Azure 工具      | 支持的版本                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | 版本号 4.1.0：2017 年 6 月版本或更高版本|
|Azure CLI v1     | 版本号 0.10.13：2017 年 5 月版本或更高版本|
|Azure CLI v2     | 版本号 2.0.12：2017 年 7 月版本或更高版本|
|AzCopy           | 版本号 6.1.0：2017 年 6 月版本或更高版本|

非托管磁盘或页 blob 是否支持 P4 和 P6 磁盘大小？

非托管磁盘和页 blob 不支持 P4 (32 GiB) 和 P6 (64 GiB) 磁盘大小作为默认磁盘层。 需要显式[设置 Blob 层](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier)，将其设为 P4 和 P6，以便存储映射到这些层的磁盘。 如果使用少于 32 GiB 或介于 32 GiB 到 64 GiB 之间的磁盘大小或内容长度部署非托管磁盘或页 blob 而不设置 Blob 层，则将继续停留在 P10（500 IOPS 和 100 MiB/秒）和映射的定价层。

**如果在支持较小磁盘（约 2017 年 6 月 15 日）之前创建了小于 64 GiB 的高级托管磁盘，将如何计费？**

根据 P10 定价层继续对小于 64 GiB 的现有高级小磁盘计费。

**如何将小于 64 GiB 的高级小磁盘的磁盘层级从 P10 切换到 P4 或 P6？**

你可以拍摄小磁盘的快照，然后创建磁盘以自动根据预配大小将定价层切换到 P4 或 P6。

**是否可以将现有托管磁盘的大小调整为小于 4 tib （TiB）的新引入磁盘大小（最大为 32 TiB）？**

可以。

**Azure 备份和 Azure Site Recovery 服务支持的最大磁盘大小是多少？**

Azure 备份和 Azure Site Recovery 服务支持的最大磁盘大小为 4 TiB。 目前尚不支持最大为 32 TiB 的磁盘。

**对于标准 SSD 和标准 HDD 磁盘的较大磁盘大小（> 4 TiB），建议的 VM 大小是多少，以实现优化的磁盘 IOPS 和带宽？**

若要实现标准 SSD 的磁盘吞吐量，并标准 HDD 大磁盘大小（> 4 个 TiB），超过 500 IOPS 和 60 MiB/秒，我们建议从以下 VM 大小之一部署新的 VM，以优化性能： B 系列、DSv2 系列、Dsv3、ESv3 系列、Fs 系列、Fsv2 系列、M 系列、GS 系列、NCv2 系列、NCv3 系列或 Ls 系列虚拟机。 将较大的磁盘附加到不使用上述建议大小的现有 Vm 或 Vm 可能会降低性能。

**如何升级在较大的磁盘大小预览过程中部署的磁盘（> 4 TiB）以获得更高的 IOPS & 在 GA 时获得更高的带宽？**

可以停止和启动磁盘附加到的 VM，也可以分离并重新附加磁盘。 对于 GA 的高级 Ssd 和标准 Ssd，较大磁盘大小的性能目标已增加。

**哪些区域是在中支持的 8 TiB、16 TiB 和 32 TiB 的托管磁盘大小？**

在全球 Azure、Microsoft Azure 政府和 Azure 中国世纪互联的所有地区都支持 8 TiB、16 TiB 和 32 TiB disk Sku。

**是否支持在所有磁盘上启用主机缓存？**

我们支持只读和读/写的主机缓存，磁盘大小小于4个 TiB。 对于超过 4 TiB 的磁盘大小，除提供“无”选项外，我们并不支持设置缓存选项。 建议利用较小磁盘大小的缓存，以便通过缓存到 VM 的数据观察到明显的性能提升。

## <a name="what-if-my-question-isnt-answered-here"></a>如果未在此处找到相关问题怎么办？

如果未在此处找到相关问题，请联系我们获取帮助。 你可以在本文末尾的评论中发布问题。 若要与 Azure 存储团队和其他社区成员就本文进行沟通，请使用 MSDN [Azure 存储论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)。

若要提出功能请求，请将请求和想法提交到 [Azure 存储反馈论坛](https://feedback.azure.com/forums/217298-storage)。
