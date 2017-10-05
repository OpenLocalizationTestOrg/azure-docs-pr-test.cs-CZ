---
title: "Kódování assetu pomocí kodéru Media Encoder Standard na portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede jednotlivými kroky kódování assetu pomocí kodéru Media Encoder Standard na portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: a299245e285c4caa68988b184799cd6f4d13e080
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Kódování assetu pomocí kodéru Media Encoder Standard na portálu Azure
> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Při práci se službou Azure Media Services je jedním nejběžnější scénářů doručování streamování s adaptivní přenosovou rychlostí vašim klientům. Služba Media Services podporuje následující technologie streamování s adaptivní přenosovou rychlostí: HTTP Live Streaming (HLS), technologie Smooth Streaming, MPEG DASH. Příprava vašich videí pro streamování s adaptivní přenosovou rychlostí spočívá v zakódování zdrojového videa zdroje do souborů s více přenosovými rychlostmi. Ke kódování vašich videí byste měli použít kodér **Media Encoder Standard**.  

Služba Media Services také poskytuje dynamické balení, což vám umožní dodávat vaše soubory MP4 více přenosovými rychlostmi ve formátech streamování: MPEG DASH, HLS, technologie Smooth Streaming, aniž byste je museli znovu zabalit do těchto formátů streamování. Při dynamickém balení stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.

Pokud chcete využít výhod dynamického balení, musíte zdrojový soubor zakódovat do sady souborů MP4 s více přenosovými rychlostmi (postup kódování je uvedený dále v této části).

Škálování zpracování média, najdete v části [to](media-services-portal-scale-media-processing.md) tématu.

## <a name="encode-with-the-azure-portal"></a>Zakódovat pomocí portálu Azure
Tato část popisuje kroky, jak můžete zakódovat svůj obsah pomocí procesoru Media Encoder Standard.

1. Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.
2. V okně **Nastavení** vyberte **Assety**.  
3. V okně **Assety** vyberte asset, který chcete zakódovat.
4. Stiskněte tlačítko **Kódovat**.
5. V okně **Kódovat asset** vyberte procesor „Media Encoder Standard“ a jedno z přednastavení. Informace o předvolbách najdete v tématu popisujícím [automatické generování žebříčku přenosových rychlostí](media-services-autogen-bitrate-ladder-with-mes.md) a v tématu [Předvolby úloh pro MES](media-services-mes-presets-overview.md). Pokud se chystáte řídit, která předvolba kódování se použije, pamatujte, že je důležité vybrat předvolbu, která je nejvhodnější pro vaše vstupní video. Pokud například více, že vaše vstupní video má rozlišení 1920 × 1080 pixelů, můžete použít přednastavení „H264 Multiple Bitrate 1080p“. Pokud máte video s nízkým rozlišením (640 × 360), neměli byste používat předvolbu H264 Multiple Bitrate 1080p.
   
   Pro snadnější správu máte možnost upravit název výstupního assetu a název úlohy.
   
   ![Kódování assetů](./media/media-services-portal-vod-get-started/media-services-encode1.png)
6. Stiskněte **Vytvořit**.

## <a name="next-step"></a>Další krok
Kódování průběh úlohy pomocí portálu Azure, můžete sledovat, jak je popsáno v [to](media-services-portal-check-job-progress.md) článku.  

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

