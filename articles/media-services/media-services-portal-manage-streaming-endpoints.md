---
title: "Správa koncových bodů streamování pomocí portálu Azure | Microsoft Docs"
description: "Toto téma ukazuje, jak spravovat koncových bodů streamování pomocí portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 797dced6c3e2525730afa29987259cb9b435ba66
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a><span data-ttu-id="ccb15-103">Správa koncových bodů streamování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ccb15-103">Manage streaming endpoints with the Azure portal</span></span>

<span data-ttu-id="ccb15-104">Toto téma ukazuje, jak pomocí portálu Azure ke správě koncových bodů streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-104">This topic shows  how to use the Azure portal to manage streaming endpoints.</span></span> 

>[!NOTE]
><span data-ttu-id="ccb15-105">Projděte si [přehled](media-services-streaming-endpoints-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="ccb15-105">Make sure to review the [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> 

<span data-ttu-id="ccb15-106">Informace o tom, jak škálování koncový bod streamování najdete v tématu [to](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="ccb15-106">For information about how to scale the streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="start-managing-streaming-endpoints"></a><span data-ttu-id="ccb15-107">Zahájení správy koncových bodů streamování</span><span class="sxs-lookup"><span data-stu-id="ccb15-107">Start managing streaming endpoints</span></span> 

<span data-ttu-id="ccb15-108">Pokud chcete spustit, Správa koncových bodů streamování pro váš účet, postupujte takto.</span><span class="sxs-lookup"><span data-stu-id="ccb15-108">To start managing streaming endpoints for your account, do the following.</span></span>

1. <span data-ttu-id="ccb15-109">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="ccb15-109">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="ccb15-110">V **nastavení** vyberte **koncové body streamování**.</span><span class="sxs-lookup"><span data-stu-id="ccb15-110">In the **Settings** blade, select **Streaming endpoints**.</span></span>
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> <span data-ttu-id="ccb15-112">Fakturuje se jenom, když je v běžícím stavu je koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-112">You are only billed when your Streaming Endpoint is in running state.</span></span>

## <a name="adddelete-a-streaming-endpoint"></a><span data-ttu-id="ccb15-113">Přidání nebo odstranění koncového bodu streamování</span><span class="sxs-lookup"><span data-stu-id="ccb15-113">Add/delete a streaming endpoint</span></span>

>[!NOTE]
><span data-ttu-id="ccb15-114">Nelze odstranit výchozí koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-114">The default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="ccb15-115">K přidání nebo odstranění koncového bodu streamování pomocí portálu Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ccb15-115">To add/delete streaming endpoint using the Azure portal, do the following:</span></span>

1. <span data-ttu-id="ccb15-116">Chcete-li přidat koncový bod streamování, klikněte na tlačítko **+ koncový bod** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="ccb15-116">To add a streaming endpoint, click the **+ Endpoint** at the top of the page.</span></span> 

    <span data-ttu-id="ccb15-117">Pokud chcete mít jiný sítím CDN nebo CDN a přímý přístup můžete několik koncových bodů streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-117">You might want multiple Streaming Endpoints if you plan to have different CDNs or a CDN and direct access.</span></span>

2. <span data-ttu-id="ccb15-118">Pokud chcete odstranit koncový bod streamování, stiskněte **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ccb15-118">To delete a streaming endpoint, press **Delete** button.</span></span>      
3. <span data-ttu-id="ccb15-119">Klikněte **spustit** tlačítko Spustit koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-119">Click the **Start** button to start the streaming endpoint.</span></span>
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <span data-ttu-id="ccb15-121"><a id="configure_streaming_endpoints"></a>Konfigurace koncového bodu streamování</span><span class="sxs-lookup"><span data-stu-id="ccb15-121"><a id="configure_streaming_endpoints"></a>Configuring the Streaming Endpoint</span></span>
<span data-ttu-id="ccb15-122">Koncový bod streamování, můžete nakonfigurovat následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="ccb15-122">Streaming Endpoint enables you to configure the following properties:</span></span>

* <span data-ttu-id="ccb15-123">Řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="ccb15-123">Access control</span></span>
* <span data-ttu-id="ccb15-124">Ovládací prvek mezipaměti</span><span class="sxs-lookup"><span data-stu-id="ccb15-124">Cache control</span></span>
* <span data-ttu-id="ccb15-125">Různé zásady přístupu lokality</span><span class="sxs-lookup"><span data-stu-id="ccb15-125">Cross site access policies</span></span>

<span data-ttu-id="ccb15-126">Podrobné informace o těchto vlastnostech najdete v tématu [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="ccb15-126">For detailed information about these properties, see [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="ccb15-127">Koncový bod streamování můžete nakonfigurovat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ccb15-127">You can configure streaming endpoint by doing the following:</span></span>

1. <span data-ttu-id="ccb15-128">Vyberte koncový bod streamování, kterou chcete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="ccb15-128">Select the streaming endpoint you want to configure.</span></span>
2. <span data-ttu-id="ccb15-129">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="ccb15-129">Click **Settings**.</span></span>

<span data-ttu-id="ccb15-130">Následuje stručný popis polí.</span><span class="sxs-lookup"><span data-stu-id="ccb15-130">A brief description of the fields follows.</span></span>

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. <span data-ttu-id="ccb15-132">Maximální mezipaměť zásad: slouží ke konfiguraci Životnost mezipaměti pro prostředky obsluhovat prostřednictvím tento koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-132">Maximum cache policy: used to configure cache lifetime for assets served through this streaming endpoint.</span></span> <span data-ttu-id="ccb15-133">Pokud není nastavena žádná hodnota, použije se výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="ccb15-133">If no value is set, the default is used.</span></span> <span data-ttu-id="ccb15-134">Výchozí hodnoty můžete definovat také přímo v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="ccb15-134">The default values can also be defined directly in Azure storage.</span></span> <span data-ttu-id="ccb15-135">Pokud Azure CDN je povolená pro koncový bod streamování, nastavíte nesmí hodnota zásady mezipaměti na menší než 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="ccb15-135">If Azure CDN is enabled for the streaming endpoint, you should not set the cache policy value to less than 600 seconds.</span></span>  
2. <span data-ttu-id="ccb15-136">Povolené IP adresy: slouží k zadání IP adresy, které bude mít možnost připojení k publikované koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-136">Allowed IP addresses: used to specify IP addresses that would be allowed to connect to the published streaming endpoint.</span></span> <span data-ttu-id="ccb15-137">Pokud žádné IP adresy zadané, bude moct připojit jakékoli IP adresy.</span><span class="sxs-lookup"><span data-stu-id="ccb15-137">If no IP addresses specified, any IP address would be able to connect.</span></span> <span data-ttu-id="ccb15-138">IP adresy lze zadat jako jednu IP adresu (například (10.0.0.1), rozsah IP pomocí IP adresy a masky podsítě CIDR (například (10.0.0.1/22) nebo rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (například 10.0.0.1 ' () 255.255.255.0) ").</span><span class="sxs-lookup"><span data-stu-id="ccb15-138">IP addresses can be specified as either a single IP address (for example, '10.0.0.1'), an IP range using an IP address and a CIDR subnet mask (for example, '10.0.0.1/22'), or an IP range using IP address and a dotted decimal subnet mask (for example, '10.0.0.1(255.255.255.0)').</span></span>
3. <span data-ttu-id="ccb15-139">Konfigurace pro ověřování záhlaví Akamai podpisu: umožňuje určit, jak konfigurovat žádosti o ověření podpisu hlavičky ze serverů Akamai.</span><span class="sxs-lookup"><span data-stu-id="ccb15-139">Configuration for Akamai signature header authentication: used to specify how signature header authentication request from Akamai servers is configured.</span></span> <span data-ttu-id="ccb15-140">Vypršení platnosti je ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="ccb15-140">Expiration is in UTC.</span></span>

## <a name="scale-your-premium-streaming-endpoint"></a><span data-ttu-id="ccb15-141">Škálovat vaše Premium koncový bod streamování</span><span class="sxs-lookup"><span data-stu-id="ccb15-141">Scale your Premium streaming endpoint</span></span>

<span data-ttu-id="ccb15-142">Další informace najdete v [tomto](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="ccb15-142">For more information, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <span data-ttu-id="ccb15-143"><a id="enable_cdn"></a>Povolit integraci Azure CDN</span><span class="sxs-lookup"><span data-stu-id="ccb15-143"><a id="enable_cdn"></a>Enable Azure CDN integration</span></span>

<span data-ttu-id="ccb15-144">Když vytvoříte nový účet, je výchozí streamování koncového bodu Azure CDN integrace povolena ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="ccb15-144">When you create a new account, default Streaming Endpoint Azure CDN integration is enabled by default.</span></span>

<span data-ttu-id="ccb15-145">Pokud budete později chtít zapnout/vypnout CDN, koncový bod streamování, musí být ve **zastavena** stavu.</span><span class="sxs-lookup"><span data-stu-id="ccb15-145">If you later want to disable/enable the CDN, your streaming endpoint must be in the **stopped** state.</span></span> <span data-ttu-id="ccb15-146">Může to trvat až 2 hodiny pro integraci produktů Azure CDN tak, aby získat povolená a změny se být aktivní v rámci všech CDN POP.</span><span class="sxs-lookup"><span data-stu-id="ccb15-146">It could take up to 2 hours for the Azure CDN integration to get enabled and for the changes to be active across all the CDN POPs.</span></span> <span data-ttu-id="ccb15-147">Však můžete spustit z koncového bodu streamování váš koncový bod streamování a datový proud bez přerušení a po dokončení integrace datový proud doručen od CDN.</span><span class="sxs-lookup"><span data-stu-id="ccb15-147">However, your can start your streaming endpoint and stream without interruptions from the streaming endpoint and once the integration is complete, the stream will be delivered from the CDN.</span></span> <span data-ttu-id="ccb15-148">Zřizování období koncový bod streamování bude v **od** stavu a můžete všimnout degredad výkonu.</span><span class="sxs-lookup"><span data-stu-id="ccb15-148">During the provisioning period your streaming endpoint will be in **starting** state and you might observe degredad performance.</span></span>

<span data-ttu-id="ccb15-149">Integrace CDN je povolena ve všech centrech execpt dat Azure Číně a Federal Goverment oblasti.</span><span class="sxs-lookup"><span data-stu-id="ccb15-149">CDN integration is enabled in all the Azure data centers execpt China and Federal Goverment regions.</span></span>

<span data-ttu-id="ccb15-150">Když je tato funkce povolená, **řízení přístupu**, **vlastní název hostitele** a **ověřování podpisu Akamai** konfigurace získá zakázána.</span><span class="sxs-lookup"><span data-stu-id="ccb15-150">Once it is enabled, the **Access Control**, **Custom hostname** and **Akamai Signature authentication** configuration gets disabled.</span></span>
 
> [!IMPORTANT]
> <span data-ttu-id="ccb15-151">Azure Media Services integraci s Azure CDN se implementuje na **Azure CDN společnosti Verizon** pro standardní koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="ccb15-151">Azure Media Services integration with Azure CDN is implemented on **Azure CDN from Verizon** for standard streaming endpoints.</span></span> <span data-ttu-id="ccb15-152">Premium koncové body streamování pomocí se dají konfigurovat všechny **Azure CDN cenové úrovně a poskytovatelé**.</span><span class="sxs-lookup"><span data-stu-id="ccb15-152">Premium streaming endpoints can be configured using all **Azure CDN pricing tiers and providers**.</span></span> <span data-ttu-id="ccb15-153">Další informace o funkcích Azure CDN najdete v tématu [přehled CDN](../cdn/cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ccb15-153">For more information about Azure CDN features, see the [CDN overview](../cdn/cdn-overview.md).</span></span>
 
### <a name="additional-considerations"></a><span data-ttu-id="ccb15-154">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="ccb15-154">Additional considerations</span></span>

* <span data-ttu-id="ccb15-155">Pokud CDN je povoleno pro koncový bod streamování, nemůže klient vyžádá obsah přímo z tohoto počátku.</span><span class="sxs-lookup"><span data-stu-id="ccb15-155">When CDN is enabled for a streaming endpoint, clients cannot request content directly from the origin.</span></span> <span data-ttu-id="ccb15-156">Pokud chcete mít možnost otestovat váš obsah s nebo bez CDN, můžete vytvořit další streamování koncový bod, který není povolen CDN.</span><span class="sxs-lookup"><span data-stu-id="ccb15-156">If you need the ability to test your content with or without CDN, you can create another streaming endpoint that isn't CDN enabled.</span></span>
* <span data-ttu-id="ccb15-157">Vaše streamování hostitele koncového bodu zůstává stejná i poté, co povolíte CDN.</span><span class="sxs-lookup"><span data-stu-id="ccb15-157">Your streaming endpoint hostname remains the same after enabling CDN.</span></span> <span data-ttu-id="ccb15-158">Nepotřebujete žádné změny do pracovního postupu media services po povolení CDN.</span><span class="sxs-lookup"><span data-stu-id="ccb15-158">You don’t need to make any changes to your media services workflow after CDN is enabled.</span></span> <span data-ttu-id="ccb15-159">Například pokud vaše streamování hostitele koncového bodu strasbourg.streaming.mediaservices.windows.net, po povolení CDN, je použít přesný stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="ccb15-159">For example, if your streaming endpoint hostname is strasbourg.streaming.mediaservices.windows.net, after enabling CDN, the exact same hostname is used.</span></span>
* <span data-ttu-id="ccb15-160">Pro nové koncových bodů streamování můžete povolit CDN jednoduše tak, že vytvoříte nový koncový bod; pro existující koncových bodů streamování musíte nejprve zastavit koncový bod a potom povolit nebo zakázat CDN.</span><span class="sxs-lookup"><span data-stu-id="ccb15-160">For new streaming endpoints, you can enable CDN simply by creating a new endpoint; for existing streaming endpoints, you need to first stop the endpoint and then enable/disable the CDN.</span></span>
* <span data-ttu-id="ccb15-161">Koncový bod streamování standard se dá nakonfigurovat jenom pomocí **poskytovatele CDN úrovně Standard Verizon** pomocí portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="ccb15-161">Standard streaming endpoint can only be configured using **Verizon Standard CDN provider** using Azure management portal.</span></span> <span data-ttu-id="ccb15-162">Můžete ale povolit jiných poskytovatelů Azure CDN pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="ccb15-162">However, you can enable other Azure CDN providers using REST APIs.</span></span>

## <a name="configure-cdn-profile"></a><span data-ttu-id="ccb15-163">Konfigurace profilu CDN</span><span class="sxs-lookup"><span data-stu-id="ccb15-163">Configure CDN profile</span></span>

<span data-ttu-id="ccb15-164">Profil CDN můžete nakonfigurovat tak, že vyberete **spravovat CDN** tlačítko shora.</span><span class="sxs-lookup"><span data-stu-id="ccb15-164">You can configure the CDN profile by selecting the **Manage CDN** button from the top.</span></span>

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a><span data-ttu-id="ccb15-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccb15-166">Next steps</span></span>
<span data-ttu-id="ccb15-167">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="ccb15-167">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ccb15-168">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="ccb15-168">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

