---
title: "Azure Media Services kódování kódy chyb | Microsoft Docs"
description: "Toto téma uvádí chybové kódy, které mohou být vráceny v případě, že došlo k chybě při kódování provádění úlohy..."
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
ms.openlocfilehash: f4fd2212d19f89148dde08c75c5a48cdd322d029
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="encoding-error-codes"></a><span data-ttu-id="7a419-103">Kódování kódy chyb</span><span class="sxs-lookup"><span data-stu-id="7a419-103">Encoding error codes</span></span>

<span data-ttu-id="7a419-104">V následující tabulce jsou uvedeny kódy chyb, které mohou být vráceny v případě, že při kódování provádění úlohy došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="7a419-104">The following table lists error codes that could be returned in case an error was encountered during the encoding task execution.</span></span>  <span data-ttu-id="7a419-105">Chcete-li získat podrobnosti o chybě v rozhraní .NET kódu, použijte [detaily chyby](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) třídy.</span><span class="sxs-lookup"><span data-stu-id="7a419-105">To get error details in your .NET code, use the [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) class.</span></span> <span data-ttu-id="7a419-106">Chcete-li získat podrobnosti o chybě v kódu REST, použijte [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span><span class="sxs-lookup"><span data-stu-id="7a419-106">To get error details in your REST code, use the [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.</span></span>

| <span data-ttu-id="7a419-107">ErrorDetail.Code</span><span class="sxs-lookup"><span data-stu-id="7a419-107">ErrorDetail.Code</span></span> | <span data-ttu-id="7a419-108">Možné příčiny chyb</span><span class="sxs-lookup"><span data-stu-id="7a419-108">Possible causes for error</span></span> |
| --- | --- |
| <span data-ttu-id="7a419-109">Neznámý</span><span class="sxs-lookup"><span data-stu-id="7a419-109">Unknown</span></span> |<span data-ttu-id="7a419-110">Při provádění úlohy došlo k neznámé chybě.</span><span class="sxs-lookup"><span data-stu-id="7a419-110">Unknown error while executing the task</span></span> |
| <span data-ttu-id="7a419-111">ErrorDownloadingInputAssetMalformedContent</span><span class="sxs-lookup"><span data-stu-id="7a419-111">ErrorDownloadingInputAssetMalformedContent</span></span> |<span data-ttu-id="7a419-112">Kategorie chyby, která obsahuje chyby ve stahování vstupní asset například názvy souborů, soubory nulové délky, nesprávný naformátuje a tak dále.</span><span class="sxs-lookup"><span data-stu-id="7a419-112">Category of errors that covers errors in downloading input asset such as bad file names, zero length files, incorrect formats and so on.</span></span> |
| <span data-ttu-id="7a419-113">ErrorDownloadingInputAssetServiceFailure</span><span class="sxs-lookup"><span data-stu-id="7a419-113">ErrorDownloadingInputAssetServiceFailure</span></span> |<span data-ttu-id="7a419-114">Kategorie chyby, které pokrývá problémy na straně služby – Příklad chyby sítě nebo úložiště při stahování.</span><span class="sxs-lookup"><span data-stu-id="7a419-114">Category of errors that covers problems on the service side - for example network or storage errors while downloading.</span></span> |
| <span data-ttu-id="7a419-115">ErrorParsingConfiguration</span><span class="sxs-lookup"><span data-stu-id="7a419-115">ErrorParsingConfiguration</span></span> |<span data-ttu-id="7a419-116">Kategorie chyby kde úloha <see cref="MediaTask.PrivateData"/> (konfigurace) není platná, například konfigurace není platný systém přednastavení nebo obsahuje neplatný kód XML.</span><span class="sxs-lookup"><span data-stu-id="7a419-116">Category of errors where task <see cref="MediaTask.PrivateData"/> (configuration) is not valid, for example the configuration is not a valid system preset or it contains invalid XML.</span></span> |
| <span data-ttu-id="7a419-117">ErrorExecutingTaskMalformedContent</span><span class="sxs-lookup"><span data-stu-id="7a419-117">ErrorExecutingTaskMalformedContent</span></span> |<span data-ttu-id="7a419-118">Kategorie chyby během provádění úlohy, kde problémy uvnitř vstupní mediálních souborů způsobit selhání.</span><span class="sxs-lookup"><span data-stu-id="7a419-118">Category of errors during the execution of the task where issues inside the input media files cause failure.</span></span> |
| <span data-ttu-id="7a419-119">ErrorExecutingTaskUnsupportedFormat</span><span class="sxs-lookup"><span data-stu-id="7a419-119">ErrorExecutingTaskUnsupportedFormat</span></span> |<span data-ttu-id="7a419-120">Kategorie chyby kde procesor médií nemůže zpracovat soubory zadané – formátu média není podporován nebo neodpovídá konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7a419-120">Category of errors where the media processor cannot process the files provided - media format not supported, or does not match the Configuration.</span></span> <span data-ttu-id="7a419-121">Například pokusu vytvořit pouze výstup z prostředek, který má jenom video</span><span class="sxs-lookup"><span data-stu-id="7a419-121">For example, trying to produce an audio-only output from an asset that has only video</span></span> |
| <span data-ttu-id="7a419-122">ErrorProcessingTask</span><span class="sxs-lookup"><span data-stu-id="7a419-122">ErrorProcessingTask</span></span> |<span data-ttu-id="7a419-123">Kategorie jiné chyby, které procesor médií, zaznamená při zpracování úloh, které se nevztahují na obsah.</span><span class="sxs-lookup"><span data-stu-id="7a419-123">Category of other errors that the media processor encounters during the processing of the task that are unrelated to content.</span></span> |
| <span data-ttu-id="7a419-124">ErrorUploadingOutputAsset</span><span class="sxs-lookup"><span data-stu-id="7a419-124">ErrorUploadingOutputAsset</span></span> |<span data-ttu-id="7a419-125">Kategorie chyby při odesílání výstupní asset</span><span class="sxs-lookup"><span data-stu-id="7a419-125">Category of errors when uploading the output asset</span></span> |
| <span data-ttu-id="7a419-126">ErrorCancelingTask</span><span class="sxs-lookup"><span data-stu-id="7a419-126">ErrorCancelingTask</span></span> |<span data-ttu-id="7a419-127">Kategorie chyby tak, aby pokrývalo selhání při pokusu o zrušení úlohy</span><span class="sxs-lookup"><span data-stu-id="7a419-127">Category of errors to cover failures when attempting to cancel the Task</span></span> |
| <span data-ttu-id="7a419-128">TransientError</span><span class="sxs-lookup"><span data-stu-id="7a419-128">TransientError</span></span> |<span data-ttu-id="7a419-129">Kategorie chyby tak, aby pokrývalo přechodné problémy (např.</span><span class="sxs-lookup"><span data-stu-id="7a419-129">Category of errors to cover transient issues (eg.</span></span> <span data-ttu-id="7a419-130">dočasné síťové potíže s Azure Storage)</span><span class="sxs-lookup"><span data-stu-id="7a419-130">temporary networking issues with Azure Storage)</span></span> |

<span data-ttu-id="7a419-131">Jak získat nápovědu z **Media Services** týmu, otevřete [lístek podpory](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="7a419-131">To get help from the **Media Services** team, open a [support ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7a419-132">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="7a419-132">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7a419-133">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="7a419-133">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="7a419-134">Související články</span><span class="sxs-lookup"><span data-stu-id="7a419-134">Related articles</span></span>
* [<span data-ttu-id="7a419-135">Přizpůsobením Media Encoder Standard přednastavení provádět pokročilé úlohy kódování</span><span class="sxs-lookup"><span data-stu-id="7a419-135">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="7a419-136">Kvóty a omezení</span><span class="sxs-lookup"><span data-stu-id="7a419-136">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
