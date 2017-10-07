---
title: "kódování kódy chyb aaaAzure Media Services | Microsoft Docs"
description: "Toto téma uvádí chybové kódy, které mohou být vráceny v případě, že došlo k chybě při provádění úlohy kódování hello..."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: b69b6abee797c40c9b8b8f23bf2398273c170e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encoding-error-codes"></a>Kódování kódy chyb

Hello následující tabulce jsou uvedeny kódy chyb, které mohou být vráceny v případě, že během hello kódování provádění úlohy došlo k chybě.  Podrobnosti o chybě tooget ve vašem kódu .NET použijte hello [detaily chyby](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) třídy. Podrobnosti o chybě tooget ve vašem kódu REST použít hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

| ErrorDetail.Code | Možné příčiny chyb |
| --- | --- |
| Neznámý |Neznámá chyba při provádění úlohy hello |
| ErrorDownloadingInputAssetMalformedContent |Kategorie chyby, která obsahuje chyby ve stahování vstupní asset například názvy souborů, soubory nulové délky, nesprávný naformátuje a tak dále. |
| ErrorDownloadingInputAssetServiceFailure |Kategorie chyby, které pokrývá problémy na straně služby hello – Příklad chyby sítě nebo úložiště při stahování. |
| ErrorParsingConfiguration |Kategorie chyby kde úloha <see cref="MediaTask.PrivateData"/> (konfigurace) není platná, například hello konfigurace není platný systém přednastavení nebo obsahuje neplatný kód XML. |
| ErrorExecutingTaskMalformedContent |Kategorie chyby během provádění hello hello úlohy, kde problémy uvnitř hello zadané mediálních souborů způsobit selhání. |
| ErrorExecutingTaskUnsupportedFormat |Kategorie chyby kde procesor médií hello nemůže zpracovat soubory, hello – formátu média není podporován nebo neodpovídá hello konfigurace. Například při tooproduce pouze výstup z prostředek, který má jenom video |
| ErrorProcessingTask |Během zpracování hello hello úlohy, které jsou nesouvisejícími toocontent zjistí kategorii jiné chyby, které hello procesor médií. |
| ErrorUploadingOutputAsset |Kategorie chyby při odesílání výstupní asset hello |
| ErrorCancelingTask |Kategorie chyby toocover selhání při pokusu o toocancel hello úloh |
| TransientError |Kategorie chyby toocover přechodné problémy (např. dočasné síťové potíže s Azure Storage) |

tooget pomoci hello **Media Services** týmu, otevřete [lístek podpory](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Související články
* [Přizpůsobením Media Encoder Standard přednastavení provádět pokročilé úlohy kódování](media-services-custom-mes-presets-with-dotnet.md)
* [Kvóty a omezení](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
