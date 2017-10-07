---
title: "hello aaaScale média zpracování pomocí portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello škálování média zpracování pomocí hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 89240c6f7579b8795e7b47f2b1c398b1d5477e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-reserved-unit-type"></a>Změna hello vyhrazený typ jednotky.
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Azure Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Přehled

Účet Media Services je přidružen vyhrazené jednotky typu, který určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy. Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**. Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu.

Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision vašeho účtu pomocí **jednotek rezervovaných** (ruština). Hello počet zřízené RUs určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu.

>[!NOTE]
>RU fungují pro paralelní provádění veškerého zpracování médií, včetně úloh indexování pomocí Azure Media Indexeru. Ale na rozdíl od kódování se úlohy indexování s rychlejšími rezervovanými jednotkami nezpracovávají rychleji.

> [!IMPORTANT]
> Ujistěte se, zda text hello tooreview [přehled](media-services-scale-media-processing-overview.md) tématu tooget Další informace o škálování média zpracování tématu.
> 
> 

## <a name="scale-media-processing"></a>Škálování zpracování médií
toochange hello vyhrazené jednotky typu a hello počet jednotek rezervovaných hello následující:

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. V hello **nastavení** vyberte **jednotky rezervované pro média**.
   
    toochange hello počet jednotek rezervovaných pro hello vybrali jednotku rezervovanou typ, použijte hello **obsluhovat jednotky média** posuvníku.
   
    toochange hello **vyhrazený typ jednotky**, stiskněte klávesu S1, S2 nebo S3.
   
    ![Stránka procesorů](./media/media-services-portal-scale-media-processing/media-services-scale-media-processing.png)
3. Změny ULOŽTE hello stiskněte tlačítko toosave.
   
    Hello nové vyhrazené jednotky jsou přiděleny po stisknutí klávesy uložit.

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

