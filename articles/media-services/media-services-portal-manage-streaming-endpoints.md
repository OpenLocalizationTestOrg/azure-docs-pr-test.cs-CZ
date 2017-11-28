---
title: "aaaManage streamování koncové body pomocí hello portálu Azure | Microsoft Docs"
description: "Toto téma ukazuje, jak hello koncových bodů streamování toomanage pomocí portálu Azure."
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
ms.openlocfilehash: dfa9352894d37edb317a6334d7f109419deb362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-streaming-endpoints-with-hello-azure-portal"></a><span data-ttu-id="651bd-103">Správa koncových bodů streamování s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="651bd-103">Manage streaming endpoints with hello Azure portal</span></span>

<span data-ttu-id="651bd-104">Toto téma ukazuje, jak toouse hello Azure portálu toomanage koncových bodů streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-104">This topic shows  how toouse hello Azure portal toomanage streaming endpoints.</span></span> 

>[!NOTE]
><span data-ttu-id="651bd-105">Ujistěte se, zda text hello tooreview [přehled](media-services-streaming-endpoints-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="651bd-105">Make sure tooreview hello [overview](media-services-streaming-endpoints-overview.md) topic.</span></span> 

<span data-ttu-id="651bd-106">Informace o tom, jak tooscale hello koncový bod streamování najdete v tématu [to](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="651bd-106">For information about how tooscale hello streaming endpoint, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <a name="start-managing-streaming-endpoints"></a><span data-ttu-id="651bd-107">Zahájení správy koncových bodů streamování</span><span class="sxs-lookup"><span data-stu-id="651bd-107">Start managing streaming endpoints</span></span> 

<span data-ttu-id="651bd-108">Správa koncových bodů streamování pro váš účet toostart hello následující.</span><span class="sxs-lookup"><span data-stu-id="651bd-108">toostart managing streaming endpoints for your account, do hello following.</span></span>

1. <span data-ttu-id="651bd-109">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="651bd-109">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="651bd-110">V hello **nastavení** vyberte **koncové body streamování**.</span><span class="sxs-lookup"><span data-stu-id="651bd-110">In hello **Settings** blade, select **Streaming endpoints**.</span></span>
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> <span data-ttu-id="651bd-112">Fakturuje se jenom, když je v běžícím stavu je koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-112">You are only billed when your Streaming Endpoint is in running state.</span></span>

## <a name="adddelete-a-streaming-endpoint"></a><span data-ttu-id="651bd-113">Přidání nebo odstranění koncového bodu streamování</span><span class="sxs-lookup"><span data-stu-id="651bd-113">Add/delete a streaming endpoint</span></span>

>[!NOTE]
><span data-ttu-id="651bd-114">nelze odstranit, Hello výchozího koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-114">hello default streaming endpoint cannot be deleted.</span></span>

<span data-ttu-id="651bd-115">tooadd nebo odstranění pomocí koncového bodu streamování hello portálu Azure, hello následující:</span><span class="sxs-lookup"><span data-stu-id="651bd-115">tooadd/delete streaming endpoint using hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="651bd-116">tooadd koncový bod streamování, klikněte na tlačítko hello **+ koncový bod** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="651bd-116">tooadd a streaming endpoint, click hello **+ Endpoint** at hello top of hello page.</span></span> 

    <span data-ttu-id="651bd-117">Pokud máte v plánu toohave můžete několik koncových bodů streamování různých sítím CDN nebo CDN a přímý přístup.</span><span class="sxs-lookup"><span data-stu-id="651bd-117">You might want multiple Streaming Endpoints if you plan toohave different CDNs or a CDN and direct access.</span></span>

2. <span data-ttu-id="651bd-118">stiskněte klávesu toodelete koncový bod streamování, **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="651bd-118">toodelete a streaming endpoint, press **Delete** button.</span></span>      
3. <span data-ttu-id="651bd-119">Klikněte na tlačítko hello **spustit** hello toostart tlačítko koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-119">Click hello **Start** button toostart hello streaming endpoint.</span></span>
   
    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <span data-ttu-id="651bd-121"><a id="configure_streaming_endpoints"></a>Konfigurace hello Streaming koncový bod</span><span class="sxs-lookup"><span data-stu-id="651bd-121"><a id="configure_streaming_endpoints"></a>Configuring hello Streaming Endpoint</span></span>
<span data-ttu-id="651bd-122">Koncový bod streamování umožňuje tooconfigure hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="651bd-122">Streaming Endpoint enables you tooconfigure hello following properties:</span></span>

* <span data-ttu-id="651bd-123">Řízení přístupu</span><span class="sxs-lookup"><span data-stu-id="651bd-123">Access control</span></span>
* <span data-ttu-id="651bd-124">Ovládací prvek mezipaměti</span><span class="sxs-lookup"><span data-stu-id="651bd-124">Cache control</span></span>
* <span data-ttu-id="651bd-125">Různé zásady přístupu lokality</span><span class="sxs-lookup"><span data-stu-id="651bd-125">Cross site access policies</span></span>

<span data-ttu-id="651bd-126">Podrobné informace o těchto vlastnostech najdete v tématu [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span><span class="sxs-lookup"><span data-stu-id="651bd-126">For detailed information about these properties, see [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint).</span></span>

<span data-ttu-id="651bd-127">Koncový bod streamování můžete nakonfigurovat pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="651bd-127">You can configure streaming endpoint by doing hello following:</span></span>

1. <span data-ttu-id="651bd-128">Vyberte hello chcete tooconfigure koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-128">Select hello streaming endpoint you want tooconfigure.</span></span>
2. <span data-ttu-id="651bd-129">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="651bd-129">Click **Settings**.</span></span>

<span data-ttu-id="651bd-130">Následuje stručný popis hello pole.</span><span class="sxs-lookup"><span data-stu-id="651bd-130">A brief description of hello fields follows.</span></span>

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. <span data-ttu-id="651bd-132">Maximální mezipaměť zásad: používané tooconfigure Životnost mezipaměti pro prostředky obsluhovat prostřednictvím tento koncový bod streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-132">Maximum cache policy: used tooconfigure cache lifetime for assets served through this streaming endpoint.</span></span> <span data-ttu-id="651bd-133">Pokud není nastavena žádná hodnota, použije se výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="651bd-133">If no value is set, hello default is used.</span></span> <span data-ttu-id="651bd-134">Hello výchozí hodnoty můžete definovat také přímo v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="651bd-134">hello default values can also be defined directly in Azure storage.</span></span> <span data-ttu-id="651bd-135">Pokud Azure CDN je povolená pro hello koncový bod streamování, neměla by nastavená hello mezipaměť zásad hodnota tooless než 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="651bd-135">If Azure CDN is enabled for hello streaming endpoint, you should not set hello cache policy value tooless than 600 seconds.</span></span>  
2. <span data-ttu-id="651bd-136">Povolené IP adresy: používá toospecify IP adresy, které bude mít možnost tooconnect toohello publikovaná koncového bodu streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-136">Allowed IP addresses: used toospecify IP addresses that would be allowed tooconnect toohello published streaming endpoint.</span></span> <span data-ttu-id="651bd-137">Pokud žádné IP adresy zadané jakékoli IP adresy by mít tooconnect.</span><span class="sxs-lookup"><span data-stu-id="651bd-137">If no IP addresses specified, any IP address would be able tooconnect.</span></span> <span data-ttu-id="651bd-138">IP adresy lze zadat jako jednu IP adresu (například (10.0.0.1), rozsah IP pomocí IP adresy a masky podsítě CIDR (například (10.0.0.1/22) nebo rozsah IP adres pomocí IP adresy a masky podsítě s tečkou (například 10.0.0.1 ' () 255.255.255.0) ").</span><span class="sxs-lookup"><span data-stu-id="651bd-138">IP addresses can be specified as either a single IP address (for example, '10.0.0.1'), an IP range using an IP address and a CIDR subnet mask (for example, '10.0.0.1/22'), or an IP range using IP address and a dotted decimal subnet mask (for example, '10.0.0.1(255.255.255.0)').</span></span>
3. <span data-ttu-id="651bd-139">Konfigurace pro ověřování záhlaví Akamai podpisu: používá toospecify konfiguraci žádosti o ověření podpisu hlavičky ze serverů Akamai.</span><span class="sxs-lookup"><span data-stu-id="651bd-139">Configuration for Akamai signature header authentication: used toospecify how signature header authentication request from Akamai servers is configured.</span></span> <span data-ttu-id="651bd-140">Vypršení platnosti je ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="651bd-140">Expiration is in UTC.</span></span>

## <a name="scale-your-premium-streaming-endpoint"></a><span data-ttu-id="651bd-141">Škálovat vaše Premium koncový bod streamování</span><span class="sxs-lookup"><span data-stu-id="651bd-141">Scale your Premium streaming endpoint</span></span>

<span data-ttu-id="651bd-142">Další informace najdete v [tomto](media-services-portal-scale-streaming-endpoints.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="651bd-142">For more information, see [this](media-services-portal-scale-streaming-endpoints.md) topic.</span></span>

## <span data-ttu-id="651bd-143"><a id="enable_cdn"></a>Povolit integraci Azure CDN</span><span class="sxs-lookup"><span data-stu-id="651bd-143"><a id="enable_cdn"></a>Enable Azure CDN integration</span></span>

<span data-ttu-id="651bd-144">Když vytvoříte nový účet, je výchozí streamování koncového bodu Azure CDN integrace povolena ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="651bd-144">When you create a new account, default Streaming Endpoint Azure CDN integration is enabled by default.</span></span>

<span data-ttu-id="651bd-145">Pokud chcete později toodisable/povolit hello CDN, koncový bod streamování, musí být v hello **zastavena** stavu.</span><span class="sxs-lookup"><span data-stu-id="651bd-145">If you later want toodisable/enable hello CDN, your streaming endpoint must be in hello **stopped** state.</span></span> <span data-ttu-id="651bd-146">Může trvat až too2 hodin pro tooget integrace Azure CDN hello povolená a toobe změny hello active mezi všechny hello CDN POP.</span><span class="sxs-lookup"><span data-stu-id="651bd-146">It could take up too2 hours for hello Azure CDN integration tooget enabled and for hello changes toobe active across all hello CDN POPs.</span></span> <span data-ttu-id="651bd-147">Však můžete spustit váš koncový bod streamování a datový proud bez přerušení z hello koncový bod streamování a po dokončení integrace hello hello datový proud doručen z hello CDN.</span><span class="sxs-lookup"><span data-stu-id="651bd-147">However, your can start your streaming endpoint and stream without interruptions from hello streaming endpoint and once hello integration is complete, hello stream will be delivered from hello CDN.</span></span> <span data-ttu-id="651bd-148">Při zřizování období hello koncový bod streamování bude v **od** stavu a můžete všimnout degredad výkonu.</span><span class="sxs-lookup"><span data-stu-id="651bd-148">During hello provisioning period your streaming endpoint will be in **starting** state and you might observe degredad performance.</span></span>

<span data-ttu-id="651bd-149">Integrace CDN je povolený v všechny hello dat Azure centrech execpt Číně a Federal Goverment oblastí.</span><span class="sxs-lookup"><span data-stu-id="651bd-149">CDN integration is enabled in all hello Azure data centers execpt China and Federal Goverment regions.</span></span>

<span data-ttu-id="651bd-150">Když je tato funkce povolená, hello **řízení přístupu**, **vlastní název hostitele** a **ověřování podpisu Akamai** konfigurace získá zakázána.</span><span class="sxs-lookup"><span data-stu-id="651bd-150">Once it is enabled, hello **Access Control**, **Custom hostname** and **Akamai Signature authentication** configuration gets disabled.</span></span>
 
> [!IMPORTANT]
> <span data-ttu-id="651bd-151">Azure Media Services integraci s Azure CDN se implementuje na **Azure CDN společnosti Verizon** pro standardní koncové body streamování.</span><span class="sxs-lookup"><span data-stu-id="651bd-151">Azure Media Services integration with Azure CDN is implemented on **Azure CDN from Verizon** for standard streaming endpoints.</span></span> <span data-ttu-id="651bd-152">Premium koncové body streamování pomocí se dají konfigurovat všechny **Azure CDN cenové úrovně a poskytovatelé**.</span><span class="sxs-lookup"><span data-stu-id="651bd-152">Premium streaming endpoints can be configured using all **Azure CDN pricing tiers and providers**.</span></span> <span data-ttu-id="651bd-153">Další informace o funkcích Azure CDN najdete v tématu hello [přehled CDN](../cdn/cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="651bd-153">For more information about Azure CDN features, see hello [CDN overview](../cdn/cdn-overview.md).</span></span>
 
### <a name="additional-considerations"></a><span data-ttu-id="651bd-154">Další aspekty</span><span class="sxs-lookup"><span data-stu-id="651bd-154">Additional considerations</span></span>

* <span data-ttu-id="651bd-155">Pokud CDN je povoleno pro koncový bod streamování, nemůže klient vyžádá obsah přímo z počátku hello.</span><span class="sxs-lookup"><span data-stu-id="651bd-155">When CDN is enabled for a streaming endpoint, clients cannot request content directly from hello origin.</span></span> <span data-ttu-id="651bd-156">Pokud potřebujete hello možnost tootest obsah s nebo bez CDN, můžete vytvořit další streamování koncový bod, který není povolen CDN.</span><span class="sxs-lookup"><span data-stu-id="651bd-156">If you need hello ability tootest your content with or without CDN, you can create another streaming endpoint that isn't CDN enabled.</span></span>
* <span data-ttu-id="651bd-157">Vaše streamování zůstane názvu hostitele koncového bodu hello stejné po povolení CDN.</span><span class="sxs-lookup"><span data-stu-id="651bd-157">Your streaming endpoint hostname remains hello same after enabling CDN.</span></span> <span data-ttu-id="651bd-158">Nepotřebujete toomake pracovního postupu všechny změny tooyour media services po povolení CDN.</span><span class="sxs-lookup"><span data-stu-id="651bd-158">You don’t need toomake any changes tooyour media services workflow after CDN is enabled.</span></span> <span data-ttu-id="651bd-159">Například pokud vaše streamování hostitele koncového bodu strasbourg.streaming.mediaservices.windows.net, po povolení CDN, se používá hello přesně stejný název hostitele.</span><span class="sxs-lookup"><span data-stu-id="651bd-159">For example, if your streaming endpoint hostname is strasbourg.streaming.mediaservices.windows.net, after enabling CDN, hello exact same hostname is used.</span></span>
* <span data-ttu-id="651bd-160">Pro nové koncových bodů streamování můžete povolit CDN jednoduše tak, že vytvoříte nový koncový bod; pro existující koncových bodů streamování budete potřebovat toofirst zastavení hello koncový bod a poté povolí nebo zakáže hello CDN.</span><span class="sxs-lookup"><span data-stu-id="651bd-160">For new streaming endpoints, you can enable CDN simply by creating a new endpoint; for existing streaming endpoints, you need toofirst stop hello endpoint and then enable/disable hello CDN.</span></span>
* <span data-ttu-id="651bd-161">Koncový bod streamování standard se dá nakonfigurovat jenom pomocí **poskytovatele CDN úrovně Standard Verizon** pomocí portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="651bd-161">Standard streaming endpoint can only be configured using **Verizon Standard CDN provider** using Azure management portal.</span></span> <span data-ttu-id="651bd-162">Můžete ale povolit jiných poskytovatelů Azure CDN pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="651bd-162">However, you can enable other Azure CDN providers using REST APIs.</span></span>

## <a name="configure-cdn-profile"></a><span data-ttu-id="651bd-163">Konfigurace profilu CDN</span><span class="sxs-lookup"><span data-stu-id="651bd-163">Configure CDN profile</span></span>

<span data-ttu-id="651bd-164">Profil CDN hello můžete nakonfigurovat tak, že vyberete hello **spravovat CDN** tlačítko shora hello.</span><span class="sxs-lookup"><span data-stu-id="651bd-164">You can configure hello CDN profile by selecting hello **Manage CDN** button from hello top.</span></span>

![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a><span data-ttu-id="651bd-166">Další kroky</span><span class="sxs-lookup"><span data-stu-id="651bd-166">Next steps</span></span>
<span data-ttu-id="651bd-167">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="651bd-167">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="651bd-168">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="651bd-168">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

