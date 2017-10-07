---
title: "rychlost spravovat aaa a souběžnost kódování službou Azure Media Services | Microsoft Docs"
description: "Tento článek poskytuje stručný přehled o tom, jak můžete spravovat rychlost a souběžnost kódování úlohy nebo úkoly službou Azure Media Services."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: da52a6278a3d3b084dbf5a594db37df8447bb944
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Spravovat rychlost a souběžnost vaše kódování

Tento článek poskytuje stručný přehled o tom, jak můžete spravovat rychlost a souběžnosti kódování úlohy a úkoly.

## <a name="overview"></a>Přehled

Ve službě Media Services **vyhrazený typ jednotky** určuje rychlost hello, ke které se zpracovávají médiu zpracování úlohy. Můžete si vybrat mezi hello následující vyhrazené typy jednotka: **S1**, **S2**, nebo **S3**. Například hello stejné úlohy kódování běží rychleji, pokud použijete hello **S2** jednotku rezervovanou typ porovnání toohello **S1** typu. Hello [škálování kódování jednotky](media-services-scale-media-processing-overview.md) téma ukazuje tabulku, která pomáhá zajistit rozhodnutí při výběru mezi různými rychlostmi kódování.

Kromě toho toospecifying hello vyhrazený typ jednotky, můžete zadat tooprovision vašeho účtu pomocí **jednotek rezervovaných**. Hello počet jednotek rezervovaných pro zřízené určuje hello počet úloh média, jež lze současně zpracovávat v daném účtu. Například pokud potom pět média úlohy bude běžet současně, pokud má váš účet jednotky rezervované pro pět, jako jsou toobe úlohy zpracování. zbývající úlohy Hello bude čekat ve frontě hello a bude získat zachyceny pro zpracování postupně po dokončení spuštěná úloha. Pokud účet nemá žádné jednotky rezervované pro zřízení, pak úlohy bude být zachyceny postupně. V takovém případě hello čekací doba mezi jeden dokončení úkolů a hello další jeden spuštění bude záviset na hello dostupnost prostředků systému hello.

Podrobné informace a příklady, zobrazující jak tooscale kódování jednotky, najdete v části [to](media-services-scale-media-processing-overview.md) tématu.

## <a name="next-step"></a>Další krok

[Kódování jednotky škálování](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

