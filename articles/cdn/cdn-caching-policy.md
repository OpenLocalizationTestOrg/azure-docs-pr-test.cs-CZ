---
title: "Správa Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services | Microsoft Docs"
description: "Naučte se spravovat Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services."
services: media-services,cdn
documentationcenter: .NET
author: juliako
manager: erikre
editor: 
ms.assetid: be33aecc-6dbe-43d7-a056-10ba911e0e94
ms.service: media-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/04/2017
ms.author: juliako
ms.openlocfilehash: 0c479a58f4158bb1a72dc43432507160f65d2791
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-cdn-caching-policy-in-azure-media-services"></a><span data-ttu-id="27030-103">Správa Azure CDN ukládání do mezipaměti zásady ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="27030-103">Manage Azure CDN caching policy in Azure Media Services</span></span>
<span data-ttu-id="27030-104">Azure Media Services poskytuje adaptivní streamování a progresivní stahování založený na HTTP.</span><span class="sxs-lookup"><span data-stu-id="27030-104">Azure Media Services provides HTTP based Adaptive Streaming and progressive download.</span></span> <span data-ttu-id="27030-105">Streamování na základě protokolu HTTP je vysoce škálovatelná výhod ukládání do mezipaměti v proxy a CDN vrstvách a také použití mezipaměti klienta.</span><span class="sxs-lookup"><span data-stu-id="27030-105">HTTP based streaming is highly scalable with benefits of caching in proxy and CDN layers as well as client side caching.</span></span> <span data-ttu-id="27030-106">Koncové body streamování poskytuje obecné možnosti streamování a také konfigurace pro mezipaměť hlavičky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="27030-106">Streaming endpoints provides general streaming capabilities and also configuration for HTTP cache headers.</span></span> <span data-ttu-id="27030-107">Koncové body streamování nastaví HTTP Cache-Control: maximální doba a hlavičky Expires.</span><span class="sxs-lookup"><span data-stu-id="27030-107">Streaming endpoints sets HTTP Cache-Control: max-age and Expires headers.</span></span> <span data-ttu-id="27030-108">Můžete získat další informace o mezipaměti hlavičky protokolu HTTP z [adrese W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).</span><span class="sxs-lookup"><span data-stu-id="27030-108">You can get more information for HTTP cache headers from [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).</span></span>

## <a name="default-caching-headers"></a><span data-ttu-id="27030-109">Výchozí záhlaví ukládání do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="27030-109">Default Caching headers</span></span>
<span data-ttu-id="27030-110">Ve výchozím nastavení použít – koncové body streamování hlavičky mezipaměť 3 dne pro streamování na vyžádání dat (fragmenty samotný mediální bloků dat) a manifest(playlist).</span><span class="sxs-lookup"><span data-stu-id="27030-110">By default streaming-endpoints apply 3 day cache headers for on-demand streaming data (actual media fragments/chunks) and manifest(playlist).</span></span> <span data-ttu-id="27030-111">Pro živé streamování koncových bodů streamování použít záhlaví mezipaměť 3 dne pro data (fragmenty samotný mediální bloků dat) a 2 sekund mezipaměti hlavičku pro žádosti o manifest(playlist).</span><span class="sxs-lookup"><span data-stu-id="27030-111">For live streaming, streaming endpoints apply 3 day cache headers for data (actual media fragments/chunks) and 2 seconds cache header for manifest(playlist) requests.</span></span> <span data-ttu-id="27030-112">Je-li program za provozu se změní na vyžádání (archiv za provozu) mezipaměti hlavičky streamování na vyžádání se použije.</span><span class="sxs-lookup"><span data-stu-id="27030-112">When live program turns to on-demand (live archive) then on-demand streaming cache headers apply.</span></span>

## <a name="azure-cdn-integration"></a><span data-ttu-id="27030-113">Integrace se službou Azure CDN</span><span class="sxs-lookup"><span data-stu-id="27030-113">Azure CDN integration</span></span>
<span data-ttu-id="27030-114">Služba Azure Media Services poskytuje [integrované CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) pro streamování koncové body.</span><span class="sxs-lookup"><span data-stu-id="27030-114">Azure Media Services provides [integrated CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) for streaming-endpoints.</span></span> <span data-ttu-id="27030-115">Hlavičky cache-control platí stejným způsobem jako koncových bodů streamování na CDN povolené koncových bodů streamování.</span><span class="sxs-lookup"><span data-stu-id="27030-115">Cache-control headers applies in the same way as streaming endpoints to CDN enabled streaming endpoints.</span></span> <span data-ttu-id="27030-116">Azure CDN používá koncový bod streamování nakonfigurované hodnot mezipaměti můžete definovat dobu životnosti interně v mezipaměti objektů a také používá tato hodnota k nastavení doručování hlaviček mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27030-116">Azure CDN uses streaming endpoint configured cache values to define the life time of the internally cached objects and also uses this value to set the delivery cache headers.</span></span> <span data-ttu-id="27030-117">Při používání CDN povoleno koncových bodů streamování se nedoporučuje pro nastavení hodnot malé mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27030-117">When using CDN enabled streaming endpoints it is not recommended to set small cache values.</span></span> <span data-ttu-id="27030-118">Nastavení hodnot malé snížit výkon a snížit výhodou CDN.</span><span class="sxs-lookup"><span data-stu-id="27030-118">Setting small values will decrease the performance and reduce the benefit of CDN.</span></span> <span data-ttu-id="27030-119">Nastavení mezipaměti hlavičky menší než 600 sekund pro CDN povoleno koncových bodů streamování není povoleno.</span><span class="sxs-lookup"><span data-stu-id="27030-119">It is not allowed to set cache headers smaller than 600 seconds for CDN enabled streaming endpoints.</span></span>

> [!IMPORTANT]
><span data-ttu-id="27030-120">Azure Media Services je kompletní integrovaná s Azure CDN.</span><span class="sxs-lookup"><span data-stu-id="27030-120">Azure Media Services has complete integration with Azure CDN.</span></span> <span data-ttu-id="27030-121">Jediným kliknutím můžete integrovat všechny dostupné Azure CDN zprostředkovatele (Akamai a Verizon) na váš koncový bod streamování, včetně produktů CDN Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="27030-121">With a single click you can integrate all the available Azure CDN providers (Akamai and Verizon) to your streaming endpoint including CDN Standard and Premium products.</span></span> <span data-ttu-id="27030-122">Další informace najdete v tématu to [oznámení](https://azure.microsoft.com/blog/standardstreamingendpoint/).</span><span class="sxs-lookup"><span data-stu-id="27030-122">For more information, see this [announcement](https://azure.microsoft.com/blog/standardstreamingendpoint/).</span></span>
> 
> <span data-ttu-id="27030-123">Poplatků za data z datových proudů koncového bodu CDN získá pouze zakázáno, pokud je povoleno CDN prostřednictvím rozhraní API pro koncový bod streamování nebo pomocí portálu Azure management portal streamování oddíl koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="27030-123">Data charges from streaming endpoint to CDN only gets disabled if the CDN is enabled over streaming endpoint APIs or using Azure management portal's streaming endpoint section.</span></span> <span data-ttu-id="27030-124">Ruční integrace nebo přímo vytváření koncový bod CDN pomocí rozhraní API CDN nebo portálu části zakázat, nebude poplatků za data.</span><span class="sxs-lookup"><span data-stu-id="27030-124">Manual integration or directly creating a CDN endpoint using CDN APIs or portal section will not disable the data charges.</span></span>

## <a name="configuring-cache-headers-with-azure-media-services"></a><span data-ttu-id="27030-125">Konfigurace mezipaměti hlavičky službou Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="27030-125">Configuring cache headers with Azure Media Services</span></span>
<span data-ttu-id="27030-126">Portál pro správu Azure nebo rozhraní API služby Azure Media Services můžete použít ke konfiguraci hodnoty hlavičky mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="27030-126">You can use Azure Management portal or Azure Media Services APIs to configure cache header values.</span></span>

1. <span data-ttu-id="27030-127">Můžete nakonfigurovat hlavičky mezipaměti pomocí portálu pro správu naleznete v [jak spravovat koncové body streamování](../media-services/media-services-portal-manage-streaming-endpoints.md) část konfigurace koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="27030-127">To configure cache headers using management portal please refer to [How to Manage Streaming Endpoints](../media-services/media-services-portal-manage-streaming-endpoints.md) section Configuring the Streaming Endpoint.</span></span>
2. <span data-ttu-id="27030-128">Rozhraní REST API služby Azure Media Services [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).</span><span class="sxs-lookup"><span data-stu-id="27030-128">Azure Media Services REST API, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).</span></span>
3. <span data-ttu-id="27030-129">Azure Media Services .NET SDK [StreamingEndpointCacheControl vlastnosti](http://go.microsoft.com/fwlink/?LinkId=615302).</span><span class="sxs-lookup"><span data-stu-id="27030-129">Azure Media Services .NET SDK, [StreamingEndpointCacheControl Properties](http://go.microsoft.com/fwlink/?LinkId=615302).</span></span>

## <a name="cache-configuration-precedence-order"></a><span data-ttu-id="27030-130">Pořadí priorit konfigurace mezipaměti</span><span class="sxs-lookup"><span data-stu-id="27030-130">Cache configuration precedence order</span></span>
1. <span data-ttu-id="27030-131">Azure Media Services nakonfigurovat mezipaměť hodnota přepíše výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="27030-131">Azure Media Services configured cache value overrides default value.</span></span>
2. <span data-ttu-id="27030-132">Pokud není žádná ruční konfigurace, použije se výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="27030-132">If there is no manual configuration, default values applies.</span></span>
3. <span data-ttu-id="27030-133">2 sekundy mezipamětí výchozí hlavičky platí pro živé streamování manifest(playlist) bez ohledu na média Azure nebo Azure Storage konfigurací a přepsání této hodnoty není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="27030-133">By default 2 seconds cache headers applies to live streaming manifest(playlist) regardless of Azure Media or Azure Storage configuration and overriding of this value is not available.</span></span>
