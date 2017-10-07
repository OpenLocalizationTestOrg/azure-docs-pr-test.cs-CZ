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
# <a name="encoding-error-codes"></a><span data-ttu-id="6f524-103">Kódování kódy chyb</span><span class="sxs-lookup"><span data-stu-id="6f524-103">Encoding error codes</span></span>

<span data-ttu-id="6f524-104">Hello následující tabulce jsou uvedeny kódy chyb, které mohou být vráceny v případě, že během hello kódování provádění úlohy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="6f524-104">hello following table lists error codes that could be returned in case an error was encountered during hello encoding task execution.</span></span>  <span data-ttu-id="6f524-105">Podrobnosti o chybě tooget ve vašem kódu .NET použijte hello [detaily chyby](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="6f524-105">tooget error details in your .NET code, use hello [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) class.</span></span> <span data-ttu-id="6f524-106">Podrobnosti o chybě tooget ve vašem kódu REST použít hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span><span class="sxs-lookup"><span data-stu-id="6f524-106">tooget error details in your REST code, use hello [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span></span>

| <span data-ttu-id="6f524-107">ErrorDetail.Code</span><span class="sxs-lookup"><span data-stu-id="6f524-107">ErrorDetail.Code</span></span> | <span data-ttu-id="6f524-108">Možné příčiny chyb</span><span class="sxs-lookup"><span data-stu-id="6f524-108">Possible causes for error</span></span> |
| --- | --- |
| <span data-ttu-id="6f524-109">Neznámý</span><span class="sxs-lookup"><span data-stu-id="6f524-109">Unknown</span></span> |<span data-ttu-id="6f524-110">Neznámá chyba při provádění úlohy hello</span><span class="sxs-lookup"><span data-stu-id="6f524-110">Unknown error while executing hello task</span></span> |
| <span data-ttu-id="6f524-111">ErrorDownloadingInputAssetMalformedContent</span><span class="sxs-lookup"><span data-stu-id="6f524-111">ErrorDownloadingInputAssetMalformedContent</span></span> |<span data-ttu-id="6f524-112">Kategorie chyby, která obsahuje chyby ve stahování vstupní asset například názvy souborů, soubory nulové délky, nesprávný naformátuje a tak dále.</span><span class="sxs-lookup"><span data-stu-id="6f524-112">Category of errors that covers errors in downloading input asset such as bad file names, zero length files, incorrect formats and so on.</span></span> |
| <span data-ttu-id="6f524-113">ErrorDownloadingInputAssetServiceFailure</span><span class="sxs-lookup"><span data-stu-id="6f524-113">ErrorDownloadingInputAssetServiceFailure</span></span> |<span data-ttu-id="6f524-114">Kategorie chyby, které pokrývá problémy na straně služby hello – Příklad chyby sítě nebo úložiště při stahování.</span><span class="sxs-lookup"><span data-stu-id="6f524-114">Category of errors that covers problems on hello service side - for example network or storage errors while downloading.</span></span> |
| <span data-ttu-id="6f524-115">ErrorParsingConfiguration</span><span class="sxs-lookup"><span data-stu-id="6f524-115">ErrorParsingConfiguration</span></span> |<span data-ttu-id="6f524-116">Kategorie chyby kde úloha <see cref="MediaTask.PrivateData"/> (konfigurace) není platná, například hello konfigurace není platný systém přednastavení nebo obsahuje neplatný kód XML.</span><span class="sxs-lookup"><span data-stu-id="6f524-116">Category of errors where task <see cref="MediaTask.PrivateData"/> (configuration) is not valid, for example hello configuration is not a valid system preset or it contains invalid XML.</span></span> |
| <span data-ttu-id="6f524-117">ErrorExecutingTaskMalformedContent</span><span class="sxs-lookup"><span data-stu-id="6f524-117">ErrorExecutingTaskMalformedContent</span></span> |<span data-ttu-id="6f524-118">Kategorie chyby během provádění hello hello úlohy, kde problémy uvnitř hello zadané mediálních souborů způsobit selhání.</span><span class="sxs-lookup"><span data-stu-id="6f524-118">Category of errors during hello execution of hello task where issues inside hello input media files cause failure.</span></span> |
| <span data-ttu-id="6f524-119">ErrorExecutingTaskUnsupportedFormat</span><span class="sxs-lookup"><span data-stu-id="6f524-119">ErrorExecutingTaskUnsupportedFormat</span></span> |<span data-ttu-id="6f524-120">Kategorie chyby kde procesor médií hello nemůže zpracovat soubory, hello – formátu média není podporován nebo neodpovídá hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6f524-120">Category of errors where hello media processor cannot process hello files provided - media format not supported, or does not match hello Configuration.</span></span> <span data-ttu-id="6f524-121">Například při tooproduce pouze výstup z prostředek, který má jenom video</span><span class="sxs-lookup"><span data-stu-id="6f524-121">For example, trying tooproduce an audio-only output from an asset that has only video</span></span> |
| <span data-ttu-id="6f524-122">ErrorProcessingTask</span><span class="sxs-lookup"><span data-stu-id="6f524-122">ErrorProcessingTask</span></span> |<span data-ttu-id="6f524-123">Během zpracování hello hello úlohy, které jsou nesouvisejícími toocontent zjistí kategorii jiné chyby, které hello procesor médií.</span><span class="sxs-lookup"><span data-stu-id="6f524-123">Category of other errors that hello media processor encounters during hello processing of hello task that are unrelated toocontent.</span></span> |
| <span data-ttu-id="6f524-124">ErrorUploadingOutputAsset</span><span class="sxs-lookup"><span data-stu-id="6f524-124">ErrorUploadingOutputAsset</span></span> |<span data-ttu-id="6f524-125">Kategorie chyby při odesílání výstupní asset hello</span><span class="sxs-lookup"><span data-stu-id="6f524-125">Category of errors when uploading hello output asset</span></span> |
| <span data-ttu-id="6f524-126">ErrorCancelingTask</span><span class="sxs-lookup"><span data-stu-id="6f524-126">ErrorCancelingTask</span></span> |<span data-ttu-id="6f524-127">Kategorie chyby toocover selhání při pokusu o toocancel hello úloh</span><span class="sxs-lookup"><span data-stu-id="6f524-127">Category of errors toocover failures when attempting toocancel hello Task</span></span> |
| <span data-ttu-id="6f524-128">TransientError</span><span class="sxs-lookup"><span data-stu-id="6f524-128">TransientError</span></span> |<span data-ttu-id="6f524-129">Kategorie chyby toocover přechodné problémy (např.</span><span class="sxs-lookup"><span data-stu-id="6f524-129">Category of errors toocover transient issues (eg.</span></span> <span data-ttu-id="6f524-130">dočasné síťové potíže s Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="6f524-130">temporary networking issues with Azure Storage)</span></span> |

<span data-ttu-id="6f524-131">tooget pomoci hello **Media Services** týmu, otevřete [lístek podpory](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="6f524-131">tooget help from hello **Media Services** team, open a [support ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="6f524-132">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="6f524-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6f524-133">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="6f524-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="6f524-134">Související články</span><span class="sxs-lookup"><span data-stu-id="6f524-134">Related articles</span></span>
* [<span data-ttu-id="6f524-135">Přizpůsobením Media Encoder Standard přednastavení provádět pokročilé úlohy kódování</span><span class="sxs-lookup"><span data-stu-id="6f524-135">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="6f524-136">Kvóty a omezení</span><span class="sxs-lookup"><span data-stu-id="6f524-136">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
