---
title: "porovnání Azure na a aaaOverview potřebují média kodéry | Microsoft Docs"
description: "Toto téma nabízí přehled a porovnání Azure na vyžádání kodéry média."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 24a3e0a16162b1bebfcde290b6baf2dd8dbfff17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Přehled a porovnání Azure na vyžádání média kodéry
## <a name="encoding-overview"></a>Kódování – přehled
Azure Media Services poskytuje několik možností hello kódování médií v cloudu hello.

Při spouštění pomocí služby Media Services, je důležité toounderstand hello rozdíl mezi formáty kodeků a souboru.
Kodeky jsou hello software, který implementuje hello kompresi nebo dekompresi algoritmy, zatímco formáty souborů jsou kontejnery, které obsahují hello komprimované video.

Služba Media Services poskytuje dynamické balení, což vám umožní toodeliver vaše s adaptivní přenosovou rychlostí MP4 nebo technologie Smooth Streaming kódování obsahu ve formátech streamování podporovaných službou Media Services (MPEG DASH, HLS, technologie Smooth Streaming) bez nutnosti toore balíček do těchto formátů streamování.

>[!NOTE]
>Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu. toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu. tootake výhod [dynamické balení](media-services-dynamic-packaging-overview.md), je nutné toodo hello následující:
>
>Navíc zakódujte váš zdrojový soubor do sady souborů MP4 nebo technologie Smooth Streaming soubory s adaptivní přenosovou rychlostí (postup hello kódování je ukázán později v tomto kurzu).

Služba Media Services podporuje následující hello na vyžádání kodéry, které jsou popsané v tomto článku:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Pracovní postup kodéru Media Encoder Premium](media-services-encode-asset.md#media-encoder-premium-workflow)

Tento článek poskytuje stručný přehled na vyžádání kodéry média a poskytuje tooarticles odkazy, které poskytují podrobnější informace. Hello téma obsahuje také porovnání kodéry hello.

>[!NOTE]
>Ve výchozím nastavení může každý účet Media Services mít jeden aktivní kódování úkol najednou. Je možné rezervovat kódování jednotky, které vám umožňují toohave více kódování úloh spuštěných současně, jednu pro jednotlivé kódování vyhrazené jednotky, které jste si koupili. Informace najdete v tématu [škálování kódování jednotky](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Standard
### <a name="how-toouse"></a>Jak toouse
[Jak tooencode pomocí procesoru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formáty
[Formáty a kodeky](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Přednastavení
Media Encoder Standard je konfigurován pomocí jedno z přednastavení kodér hello popsané [zde](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Vstup a výstup metadat
Hello kodéry vstupní metadat je popsán [zde](media-services-input-metadata-schema.md).

Hello kodéry výstup metadat je popsán [zde](media-services-output-metadata-schema.md).

### <a name="generate-thumbnails"></a>Vytváření miniatur
Informace najdete v tématu [jak toogenerate miniatur pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Trim videa (výstřižek)
Informace najdete v tématu [jak tootrim videa pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Vytvoření překryvy
Informace najdete v tématu [jak toocreate překryvy pomocí kodéru Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Viz také
[blog Hello Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Pracovní postup kodéru Media Encoder Premium
### <a name="overview"></a>Přehled
[Představení Premium kódování v Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-toouse"></a>Jak toouse
Media Encoder Premium pracovní postup je nakonfigurovat pomocí komplexní pracovních postupů. Soubory pracovního postupu by bylo možné vytvořit a aktualizovat pomocí hello [Návrhář postupu provádění](media-services-workflow-designer.md) nástroj.

[Jak tooUse Premium kódování ve službě Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Známé problémy
Pokud vaše vstupní video neobsahuje titulků, hello výstupní že Asset bude stále obsahovat prázdný soubor TTML.


## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Související články
* [Přizpůsobením Media Encoder Standard přednastavení provádět pokročilé úlohy kódování](media-services-custom-mes-presets-with-dotnet.md)
* [Kvóty a omezení](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
