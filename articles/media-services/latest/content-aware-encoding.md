---
title: 内容感知编码的实验性预设-Azure |Microsoft Docs
description: 本文介绍 Microsoft Azure 媒体服务 v3 中的内容感知编码。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: c2846759a8daa04fc5c1d3b7f69e2c061bacb272
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75933482"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>内容感知编码的实验预设

若要为[自适应比特率流式处理](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)准备传递内容，需要以多个比特率（从高到低）对视频进行编码。 为了确保质量的质量下降，因为比特率降低，因此视频的分辨率。 这会生成一个所谓的编码阶梯，即一个解决方案表和一个比特率;请参阅媒体服务[内置的编码预设](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset)。

## <a name="overview"></a>概述

在 Netflix 2015 年12月发布其[博客](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2)后，超出一种全视频式的视频方法的兴趣。 自那时起，在 marketplace 中发布了多个用于内容识别编码的解决方案;有关概述，请参阅[此文](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx)。 其思路是要了解内容-自定义编码方法，并将其调整为单独视频的复杂性。 在每个解决方法中，有比特率更高的比特率敏锐–编码器以此最佳比特率进行操作。 下一级别的优化是选择基于内容的解决方案–例如，PowerPoint 演示文稿的视频不会受益于下面的720p。 接下来，可以对编码器进行优化，以优化视频中每个拍摄的设置。 Netflix 在2018中介绍[了这种方法](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830)。

在早期版本2017中，Microsoft 发布了[自适应流式处理](autogen-bitrate-ladder.md)预设，以解决源视频的质量和分辨率发生变化的问题。 我们的客户有不同的内容组合，某些位置为1080p，其他的是 而且，并非所有源内容都是从胶片或电视工作室获得高质量的 mezzanines。 自适应流式处理预设通过确保比特率阶梯从不超过输入夹层的分辨率或平均比特率来解决这些问题。

新的内容识别编码预设通过合并自定义逻辑来扩展该机制，使编码器能够查找给定解析的最佳比特率值，但不需要进行大量的计算分析。 此预设生成一组 GOP 对齐的 Mp4。 根据给定的任何输入内容，该服务会对输入内容执行初始的轻型分析，并使用这些结果来确定最理想的层数，并为自适应流式处理提供适当的比特率和分辨率设置。 此预设对于低和中复杂度视频特别有效，其中输出文件的比特率低于自适应流式处理预设，但仍会为查看者提供良好的体验。 输出将包含带有视频和音频交错的有文件文件

请参阅以下示例图形，其中显示了使用[PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio)和[VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion)之类的质量指标进行比较。 源是通过连接电影和电视节目中的高复杂性照片的简短剪辑来创建的，目的是为了强调编码器。 按照定义，此预设会生成与内容不同的结果-这也意味着对于某些内容，可能不会显著降低比特率或质量提高。

![使用 PSNR 的速率失真（RD）曲线](media/cae-experimental/msrv1.png)

**图1：使用 PSNR 度量值为高复杂性源的速率失真（RD）曲线**

![使用 VMAF 的速率失真（RD）曲线](media/cae-experimental/msrv2.png)

**图2：使用 VMAF 度量值为高复杂性源的速率失真（RD）曲线**

下面是另一个类别的源内容的结果，编码器能够确定输入的质量质量较差（由于低比特率，很多压缩项目）。 请注意，使用内容感知预设时，编码器决定仅生成一个输出层–低比特率，以便大多数客户端能够在不停止的情况下播放流。

![使用 PSNR 的 RD 曲线](media/cae-experimental/msrv3.png)

**图3：使用 PSNR 进行低质量输入的 RD 曲线（以1080p 为限）**

![使用 VMAF 的 RD 曲线](media/cae-experimental/msrv4.png)

**图4：使用 VMAF 进行低质量输入的 RD 曲线（以1080p 为限）**

## <a name="use-the-experimental-preset"></a>使用实验预设

可以创建使用此预设的转换，如下所示。 如果使用[此类](stream-files-tutorial-with-api.md)教程，则可以更新代码，如下所示：

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncoding
      }
   }
};
```

> [!NOTE]
> 底层算法需要进一步改进。 随着时间的推移，可能会随时间变化，用于生成比特率 ladders，目的是提供一种可靠的算法，并适应各种输入条件。 使用此预设的编码作业仍将根据输出分钟数计费，并且可从我们的流式处理终结点（如短划线和 HLS）传递输出资产。

## <a name="next-steps"></a>后续步骤

现在，你已了解优化视频的这一新选项，我们会邀请你试用。你可以使用本文末尾的链接向我们发送反馈。
