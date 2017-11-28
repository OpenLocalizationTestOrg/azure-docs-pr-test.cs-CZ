---
title: "aaaUsing PlayReady nebo Widevine běžného dynamického šifrování | Microsoft Docs"
description: "Microsoft Azure Media Services umožňuje vám toodeliver MPEG-DASH, technologie Smooth Streaming a Http-Live-Streaming (HLS) datové proudy chráněné pomocí Microsoft PlayReady DRM. Také vám umožní toodelivery DASH šifrované pomocí Widevine DRM. Toto téma ukazuje, jak toodynamically šifrovat pomocí PlayReady a Widevine DRM."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 548d1a12-e2cb-45fe-9307-4ec0320567a2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 0475e6ec80dcf39eb4e5c4ad4d17f821502951bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-playready-andor-widevine-dynamic-common-encryption"></a><span data-ttu-id="eacce-105">Použití běžného dynamického šifrování PlayReady nebo Widevine</span><span class="sxs-lookup"><span data-stu-id="eacce-105">Using PlayReady and/or Widevine dynamic common encryption</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="eacce-106">.NET</span><span class="sxs-lookup"><span data-stu-id="eacce-106">.NET</span></span>](media-services-protect-with-drm.md)
> * [<span data-ttu-id="eacce-107">Java</span><span class="sxs-lookup"><span data-stu-id="eacce-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="eacce-108">PHP</span><span class="sxs-lookup"><span data-stu-id="eacce-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
>
>

<span data-ttu-id="eacce-109">Microsoft Azure Media Services umožňuje toodeliver MPEG-DASH, technologie Smooth Streaming a datové proudy HTTP-Live-Streaming (HLS) chráněné pomocí [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span><span class="sxs-lookup"><span data-stu-id="eacce-109">Microsoft Azure Media Services enables you toodeliver MPEG-DASH, Smooth Streaming, and HTTP-Live-Streaming (HLS) streams protected with [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/).</span></span> <span data-ttu-id="eacce-110">Také vám umožní toodeliver šifrované datové proudy DASH s licencemi Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="eacce-110">It also enables you toodeliver encrypted DASH streams with Widevine DRM licenses.</span></span> <span data-ttu-id="eacce-111">Technologie PlayReady a Widevine jsou šifrované podle specifikace Common Encryption (ISO/IEC CENC 23001-7) hello.</span><span class="sxs-lookup"><span data-stu-id="eacce-111">Both PlayReady and Widevine are encrypted per hello Common Encryption (ISO/IEC 23001-7 CENC) specification.</span></span> <span data-ttu-id="eacce-112">Můžete použít [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (počínaje hello verzí 3.5.1) nebo REST API tooconfigure vaše AssetDeliveryConfiguration toouse Widevine.</span><span class="sxs-lookup"><span data-stu-id="eacce-112">You can use [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (starting with hello version 3.5.1) or REST API tooconfigure your AssetDeliveryConfiguration toouse Widevine.</span></span>

<span data-ttu-id="eacce-113">Media Services poskytuje službu k doručování licencí PlayReady DRM a Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="eacce-113">Media Services provides a service for delivering PlayReady and Widevine DRM licenses.</span></span> <span data-ttu-id="eacce-114">Služba Media Services také poskytuje rozhraní API umožňující nakonfigurovat práva hello a omezení, které chcete použít pro hello PlayReady nebo Widevine DRM runtime tooenforce když uživatel přehrává chráněný obsah.</span><span class="sxs-lookup"><span data-stu-id="eacce-114">Media Services also provides APIs that let you configure hello rights and restrictions that you want for hello PlayReady or Widevine DRM runtime tooenforce when a user plays back protected content.</span></span> <span data-ttu-id="eacce-115">Když si uživatel vyžádá obsah chráněný pomocí DRM, aplikace hello přehrávače si vyžádá licenci z licenční služby hello AMS.</span><span class="sxs-lookup"><span data-stu-id="eacce-115">When a user requests a DRM protected content, hello player application will request a license from hello AMS license service.</span></span> <span data-ttu-id="eacce-116">Licenční služba AMS Hello vydá přehrávač toohello licenci, pokud je povoleno.</span><span class="sxs-lookup"><span data-stu-id="eacce-116">hello AMS license service will issue a license toohello player if it is authorized.</span></span> <span data-ttu-id="eacce-117">Licence PlayReady nebo Widevine obsahuje dešifrovací klíč hello, které je možné hello klientům player toodecrypt a datový proud hello obsahu.</span><span class="sxs-lookup"><span data-stu-id="eacce-117">A PlayReady or Widevine license contains hello decryption key that can be used by hello client player toodecrypt and stream hello content.</span></span>

<span data-ttu-id="eacce-118">Můžete také použít následující toohelp partneři AMS doručování licence na Widevine hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="eacce-118">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span> <span data-ttu-id="eacce-119">Další informace najdete v popisu integrace s [Axinom](media-services-axinom-integration.md) a [castLabs](media-services-castlabs-integration.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-119">For more information, see: integration with [Axinom](media-services-axinom-integration.md) and [castLabs](media-services-castlabs-integration.md).</span></span>

<span data-ttu-id="eacce-120">Služba Media Services podporuje více způsobů autorizace uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="eacce-120">Media Services supports multiple ways of authorizing users who make key requests.</span></span> <span data-ttu-id="eacce-121">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevření nebo token omezení.</span><span class="sxs-lookup"><span data-stu-id="eacce-121">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="eacce-122">zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS).</span><span class="sxs-lookup"><span data-stu-id="eacce-122">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="eacce-123">Služba Media Services podporuje tokeny ve hello [jednoduchých webových tokenů](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) formátu (SWT) a [webových tokenů JSON](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) formátu (JWT).</span><span class="sxs-lookup"><span data-stu-id="eacce-123">Media Services supports tokens in hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="eacce-124">Další informace najdete v tématu Konfigurace hello obsahu zásad autorizace klíče.</span><span class="sxs-lookup"><span data-stu-id="eacce-124">For more information, see Configure hello content key’s authorization policy.</span></span>

<span data-ttu-id="eacce-125">tootake využívat dynamické šifrování, potřebujete toohave asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="eacce-125">tootake advantage of dynamic encryption, you need toohave an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="eacce-126">Budete také potřebovat zásady doručení hello tooconfigure pro prostředek hello (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="eacce-126">You also need tooconfigure hello delivery policies for hello asset (described later in this topic).</span></span> <span data-ttu-id="eacce-127">Pak na základě hello formátem zadaným v hello adresu URL streamování, server streamingu na vyžádání hello zajistí, že tento hello datový proud doručen v protokolu hello, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="eacce-127">Then, based on hello format specified in hello streaming URL, hello On-Demand Streaming server will ensure that hello stream is delivered in hello protocol you have chosen.</span></span> <span data-ttu-id="eacce-128">V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající HTTP v reakci na každý požadavek klienta.</span><span class="sxs-lookup"><span data-stu-id="eacce-128">As a result, you only need toostore and pay for hello files in a single storage format and Media Services will build and serve hello appropriate HTTP response based on each request from a client.</span></span>

<span data-ttu-id="eacce-129">Toto téma bude užitečné toodevelopers, které pracují v aplikacích, které doručují média chráněná několika technologiemi DRM, např. PlayReady a Widevine.</span><span class="sxs-lookup"><span data-stu-id="eacce-129">This topic would be useful toodevelopers that work on applications that deliver media protected with multiple DRMs, such as PlayReady and Widevine.</span></span> <span data-ttu-id="eacce-130">Hello téma ukazuje, jak tooconfigure hello služby doručování licencí PlayReady pomocí zásad autorizace tak, aby pouze autorizovaní klienti mohli dostávat licence PlayReady nebo Widevine.</span><span class="sxs-lookup"><span data-stu-id="eacce-130">hello topic shows you how tooconfigure hello PlayReady license delivery service with authorization policies so that only authorized clients could receive PlayReady or Widevine licenses.</span></span> <span data-ttu-id="eacce-131">Také ukazuje, jak toouse dynamické šifrování s DRM PlayReady nebo Widevine přes streamování DASH.</span><span class="sxs-lookup"><span data-stu-id="eacce-131">It also shows how toouse dynamic encryption encryption with PlayReady or Widevine DRM over DASH.</span></span>

>[!NOTE]
><span data-ttu-id="eacce-132">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="eacce-132">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="eacce-133">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="eacce-133">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 

## <a name="download-sample"></a><span data-ttu-id="eacce-134">Stažení ukázky</span><span class="sxs-lookup"><span data-stu-id="eacce-134">Download sample</span></span>
<span data-ttu-id="eacce-135">Hello ukázku popsanou v tomto článku si můžete stáhnout [zde](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span><span class="sxs-lookup"><span data-stu-id="eacce-135">You can download hello sample described in this article from [here](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm).</span></span>

## <a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a><span data-ttu-id="eacce-136">Konfigurace běžného dynamického šifrování a služeb doručování licencí DRM</span><span class="sxs-lookup"><span data-stu-id="eacce-136">Configuring Dynamic Common Encryption and DRM License Delivery Services</span></span>

<span data-ttu-id="eacce-137">Hello následují obecné kroky, potřebovali byste tooperform Pokud své assety chráníte pomocí technologie PlayReady, pomocí služby doručování licencí Media Services hello a také používáte dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="eacce-137">hello following are general steps that you would need tooperform when protecting your assets with PlayReady, using hello Media Services license delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="eacce-138">Vytvoření assetu a nahrání souborů do hello asset.</span><span class="sxs-lookup"><span data-stu-id="eacce-138">Create an asset and upload files into hello asset.</span></span>
2. <span data-ttu-id="eacce-139">Zakódujte hello asset obsahující hello souboru toohello s adaptivní přenosovou rychlostí sady souborů MP4.</span><span class="sxs-lookup"><span data-stu-id="eacce-139">Encode hello asset containing hello file toohello adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="eacce-140">Vytvoření klíče k obsahu a přidružte ji k hello kódovaný asset.</span><span class="sxs-lookup"><span data-stu-id="eacce-140">Create a content key and associate it with hello encoded asset.</span></span> <span data-ttu-id="eacce-141">Ve službě Media Services obsahuje klíč obsahu hello hello asset šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="eacce-141">In Media Services, hello content key contains hello asset’s encryption key.</span></span>
4. <span data-ttu-id="eacce-142">Nakonfigurujte zásady autorizace hello klíč obsahu.</span><span class="sxs-lookup"><span data-stu-id="eacce-142">Configure hello content key’s authorization policy.</span></span> <span data-ttu-id="eacce-143">zásady autorizace klíče obsahu Hello musíte nakonfigurovat a splnit klient hello hello obsahu klíče toobe doručené toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="eacce-143">hello content key authorization policy must be configured by you and met by hello client in order for hello content key toobe delivered toohello client.</span></span>

    <span data-ttu-id="eacce-144">Při vytváření zásady autorizace klíče obsahu hello, je třeba toospecify hello následující: metodu doručení (PlayReady nebo Widevine), omezení (otevřené nebo s tokenem) a typ doručení klíče toohello konkrétní informace, která definuje, jak je hello klíč doručen toohello klienta ([PlayReady](media-services-playready-license-template-overview.md) nebo [Widevine](media-services-widevine-license-template-overview.md) šablona licence).</span><span class="sxs-lookup"><span data-stu-id="eacce-144">When creating hello content key authorization policy, you need toospecify hello following: delivery method (PlayReady or Widevine), restrictions (open or token), and information specific toohello key delivery type that defines how hello key is delivered toohello client ([PlayReady](media-services-playready-license-template-overview.md) or [Widevine](media-services-widevine-license-template-overview.md) license template).</span></span>

5. <span data-ttu-id="eacce-145">Konfigurace zásad doručení hello pro určitý prostředek.</span><span class="sxs-lookup"><span data-stu-id="eacce-145">Configure hello delivery policy for an asset.</span></span> <span data-ttu-id="eacce-146">Konfigurace zásad doručení Hello zahrnuje: doručovací protokol (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), hello typ dynamického šifrování (například Common Encryption), PlayReady nebo adresa URL pro získání licence Widevine.</span><span class="sxs-lookup"><span data-stu-id="eacce-146">hello delivery policy configuration includes: delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), hello type of dynamic encryption (for example, Common Encryption), PlayReady or Widevine license acquisition URL.</span></span>

    <span data-ttu-id="eacce-147">Může použít protokol tooeach jinou zásadu na hello stejné asset.</span><span class="sxs-lookup"><span data-stu-id="eacce-147">You could apply different policy tooeach protocol on hello same asset.</span></span> <span data-ttu-id="eacce-148">Můžete například použít šifrování PlayReady tooSmooth/DASH a pomocí standardu AES Envelope tooHLS.</span><span class="sxs-lookup"><span data-stu-id="eacce-148">For example, you could apply PlayReady encryption tooSmooth/DASH and AES Envelope tooHLS.</span></span> <span data-ttu-id="eacce-149">Veškeré protokoly, které nejsou v zásadách doručení definovány (například přidáte jedinou zásadu, která jako hello protokol určuje pouze HLS) budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="eacce-149">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="eacce-150">Výjimka toothis Hello je, pokud máte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="eacce-150">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="eacce-151">Potom bude možné v hello zrušte všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="eacce-151">Then, all protocols will be allowed in hello clear.</span></span>

6. <span data-ttu-id="eacce-152">V pořadí tooget adresu URL streamování vytvořte Lokátor OnDemand.</span><span class="sxs-lookup"><span data-stu-id="eacce-152">Create an OnDemand locator in order tooget a streaming URL.</span></span>

<span data-ttu-id="eacce-153">Najdete kompletní příklad rozhraní .NET na konci hello hello tématu.</span><span class="sxs-lookup"><span data-stu-id="eacce-153">You will find a complete .NET example at hello end of hello topic.</span></span>

<span data-ttu-id="eacce-154">Hello následující obrázek ukazuje pracovní postup hello popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="eacce-154">hello following image demonstrates hello workflow described above.</span></span> <span data-ttu-id="eacce-155">Zde hello token slouží k ověřování.</span><span class="sxs-lookup"><span data-stu-id="eacce-155">Here hello token is used for authentication.</span></span>

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

<span data-ttu-id="eacce-157">Hello zbývající část tohoto tématu poskytuje podrobné vysvětlení, ukázky kódu a tootopics odkazy, které ukazují, jak tooachieve hello výše popsané úlohy.</span><span class="sxs-lookup"><span data-stu-id="eacce-157">hello rest of this topic provides detailed explanations, code examples, and links tootopics that show you how tooachieve hello tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="eacce-158">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="eacce-158">Current limitations</span></span>
<span data-ttu-id="eacce-159">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit hello přidružené Lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="eacce-159">If you add or update an asset delivery policy, you must delete hello associated locator (if any) and create a new locator.</span></span>

<span data-ttu-id="eacce-160">Omezení při šifrování s technologií Widevine ve službě Azure Media Services: v současné době není podporováno více klíčů k obsahu.</span><span class="sxs-lookup"><span data-stu-id="eacce-160">Limitation when encrypting with Widevine with Azure Media Services: currently, multiple content keys are not supported.</span></span>

## <a name="create-an-asset-and-upload-files-into-hello-asset"></a><span data-ttu-id="eacce-161">Vytvoření assetu a nahrání souborů do hello asset</span><span class="sxs-lookup"><span data-stu-id="eacce-161">Create an asset and upload files into hello asset</span></span>
<span data-ttu-id="eacce-162">V pořadí toomanage, kódovat a Streamovat videa, musíte nejprve nahrát obsah do Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="eacce-162">In order toomanage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="eacce-163">Po nahrání váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="eacce-163">Once uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span>

<span data-ttu-id="eacce-164">Podrobné informace najdete v článku o [nahrání souborů do účtu služby Media Services](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-164">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <a name="encode-hello-asset-containing-hello-file-toohello-adaptive-bitrate-mp4-set"></a><span data-ttu-id="eacce-165">Kódování hello asset obsahující hello souboru toohello s adaptivní přenosovou rychlostí sady souborů MP4</span><span class="sxs-lookup"><span data-stu-id="eacce-165">Encode hello asset containing hello file toohello adaptive bitrate MP4 set</span></span>
<span data-ttu-id="eacce-166">V případě dynamického šifrování je třeba je toocreate asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="eacce-166">With dynamic encryption all you need is toocreate an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="eacce-167">Pak na základě zadaného formátu hello v manifestu hello a fragmentovat požadavku, hello streamování na vyžádání server zajistí zobrazí datový proud hello hello protokolu, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="eacce-167">Then, based on hello specified format in hello manifest and fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="eacce-168">V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="eacce-168">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span> <span data-ttu-id="eacce-169">Další informace najdete v tématu hello [přehled dynamického balení](media-services-dynamic-packaging-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="eacce-169">For more information, see hello [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

<span data-ttu-id="eacce-170">Návod, jak tooencode, najdete v části [jak tooencode assetu pomocí kodéru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-170">For instructions on how tooencode, see [How tooencode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="eacce-171"><a id="create_contentkey"></a>Vytvoření klíče k obsahu a přidružte ji k asset hello kódování</span><span class="sxs-lookup"><span data-stu-id="eacce-171"><a id="create_contentkey"></a>Create a content key and associate it with hello encoded asset</span></span>
<span data-ttu-id="eacce-172">Ve službě Media Services obsahuje klíč obsahu hello hello klíče, které chcete tooencrypt prostředek s.</span><span class="sxs-lookup"><span data-stu-id="eacce-172">In Media Services, hello content key contains hello key that you want tooencrypt an asset with.</span></span>

<span data-ttu-id="eacce-173">Podrobné informace najdete v tématu [Vytvoření klíče k obsahu](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-173">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="eacce-174"><a id="configure_key_auth_policy"></a>Nakonfigurujte zásady autorizace hello klíč obsahu</span><span class="sxs-lookup"><span data-stu-id="eacce-174"><a id="configure_key_auth_policy"></a>Configure hello content key’s authorization policy</span></span>
<span data-ttu-id="eacce-175">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="eacce-175">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="eacce-176">zásady autorizace klíče obsahu Hello musíte nakonfigurovat a splní hello klient (přehrávač) v pořadí pro klíče toobe hello doručit toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="eacce-176">hello content key authorization policy must be configured by you and met by hello client (player) in order for hello key toobe delivered toohello client.</span></span> <span data-ttu-id="eacce-177">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevření nebo token omezení.</span><span class="sxs-lookup"><span data-stu-id="eacce-177">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span>

<span data-ttu-id="eacce-178">Podrobnější informace najdete v tématu [Konfigurace zásad autorizace pro klíč k obsahu](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span><span class="sxs-lookup"><span data-stu-id="eacce-178">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).</span></span>

## <span data-ttu-id="eacce-179"><a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="eacce-179"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="eacce-180">Konfigurace zásad hello doručení pro asset.</span><span class="sxs-lookup"><span data-stu-id="eacce-180">Configure hello delivery policy for your asset.</span></span> <span data-ttu-id="eacce-181">Některé kroky, které hello asset konfigurace zásad doručení zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="eacce-181">Some things that hello asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="eacce-182">Hello DRM licence adresu URL pro získání.</span><span class="sxs-lookup"><span data-stu-id="eacce-182">hello DRM license acquisition URL.</span></span>
* <span data-ttu-id="eacce-183">Hello doručovací protokol assetu (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny).</span><span class="sxs-lookup"><span data-stu-id="eacce-183">hello asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="eacce-184">Hello typ dynamického šifrování (v tomto případě Common Encryption).</span><span class="sxs-lookup"><span data-stu-id="eacce-184">hello type of dynamic encryption (in this case, Common Encryption).</span></span>

<span data-ttu-id="eacce-185">Podrobné informace najdete v tématu [Konfigurace zásad doručení assetu ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-185">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="eacce-186"><a id="create_locator"></a>Vytvořte v pořadí tooget adresu URL pro streamování Lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="eacce-186"><a id="create_locator"></a>Create an OnDemand streaming locator in order tooget a streaming URL</span></span>
<span data-ttu-id="eacce-187">Budete potřebovat tooprovide uživatelů s hello streamování adresu URL pro protokol Smooth, DASH nebo HLS.</span><span class="sxs-lookup"><span data-stu-id="eacce-187">You will need tooprovide your user with hello streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="eacce-188">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="eacce-188">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
>
>

<span data-ttu-id="eacce-189">Pokyny, jak toopublish prostředek a sestavení adresu URL pro streamování, najdete v části [sestavit adresu URL pro streamování](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-189">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="eacce-190">Získání testovacího tokenu</span><span class="sxs-lookup"><span data-stu-id="eacce-190">Get a test token</span></span>
<span data-ttu-id="eacce-191">Získání testovacího tokenu podle hello tokenu omezení, která byla použita pro zásad autorizace pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="eacce-191">Get a test token based on hello token restriction that was used for hello key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string.
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);


<span data-ttu-id="eacce-192">Můžete použít hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest datového proudu.</span><span class="sxs-lookup"><span data-stu-id="eacce-192">You can use hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest your stream.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="eacce-193">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="eacce-193">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="eacce-194">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="eacce-194">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="eacce-195">Přidejte následující prvky příliš hello**appSettings** definované v souboru app.config:</span><span class="sxs-lookup"><span data-stu-id="eacce-195">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="eacce-196">Příklad</span><span class="sxs-lookup"><span data-stu-id="eacce-196">Example</span></span>

<span data-ttu-id="eacce-197">Hello následující příklad ukazuje funkce, která byla zavedena v Azure Media Services SDK pro .net – verze 3.5.2 (konkrétně hello možnost toodefine Widevine licence šablony a žádat o licenci Widevine ze služby Azure Media Services).</span><span class="sxs-lookup"><span data-stu-id="eacce-197">hello following sample demonstrates functionality that was introduced in Azure Media Services SDK for .Net -Version 3.5.2 (specifically, hello ability toodefine a Widevine license template and request a Widevine license from Azure Media Services).</span></span>

<span data-ttu-id="eacce-198">Přepište hello kód v souboru Program.cs kódem hello uvedené v této sekci.</span><span class="sxs-lookup"><span data-stu-id="eacce-198">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="eacce-199">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="eacce-199">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="eacce-200">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="eacce-200">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="eacce-201">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="eacce-201">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="eacce-202">Ujistěte se že proměnné tooupdate toopoint toofolders kde jsou umístěné vaše vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="eacce-202">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
    using Newtonsoft.Json;

    namespace DynamicEncryptionWithDRM
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        private static readonly Uri _sampleAudience =
            new Uri(ConfigurationManager.AppSettings["Audience"]);

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            bool tokenRestriction = false;
            string tokenTemplateString = null;

            IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
            Console.WriteLine("Uploaded asset: {0}", asset.Id);

            IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
            Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);

            IContentKey key = CreateCommonTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
            Console.WriteLine();

            if (tokenRestriction)
            tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
            else
            AddOpenAuthorizationPolicy(key);

            Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
            Console.WriteLine();

            CreateAssetDeliveryPolicy(encodedAsset, key);
            Console.WriteLine("Created asset delivery policy. \n");
            Console.WriteLine();

            if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
            {
            // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
            // back into a TokenRestrictionTemplate class instance.
            TokenRestrictionTemplate tokenTemplate =
                TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

            // Generate a test token based on hello hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello http://amsplayer.azurewebsites.net/azuremediaplayer.html player tootest streams.
            // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);

            Console.ReadLine();
        }

        static public IAsset UploadFileAndCreateAsset(string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
            Console.WriteLine("File does not exist.");
            return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
        {
            var encodingPreset = "Adaptive Streaming";

            IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} too{1}",
                        inputAsset.Name,
                        encodingPreset));

            var mediaProcessors =
            _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();

            var latestMediaProcessor =
            mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();

            ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
            encodeTask.InputAssets.Add(inputAsset);
            encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset), AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }


        static public IContentKey CreateCommonTypeContentKey(IAsset asset)
        {

            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {

            // Create ContentKeyAuthorizationPolicy with Open restrictions
            // and create authorization policy          

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Open",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                    Requirements = null
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with no restrictions").
                Result;


            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
            // Associate hello content key authorization policy with hello content key.
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                new ContentKeyAuthorizationPolicyRestriction
                {
                    Name = "Token Authorization Policy",
                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                    Requirements = tokenTemplateString,
                }
                };

            // Configure PlayReady and Widevine license templates.
            string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();

            string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();

            IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                restrictions, PlayReadyLicenseTemplate);

            IContentKeyAuthorizationPolicyOption WidevinePolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.Widevine,
                restrictions, WidevineLicenseTemplate);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
            contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);

            // Associate hello content key authorization policy with hello content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        static private string GenerateTokenRequirements()
        {
            TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);

            template.PrimaryVerificationKey = new SymmetricVerificationKey();
            template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
            template.Audience = _sampleAudience.ToString();
            template.Issuer = _sampleIssuer.ToString();
            template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);

            return TokenRestrictionTemplateSerializer.Serialize(template);
        }

        static private string ConfigurePlayReadyLicenseTemplate()
        {
            // hello following code configures PlayReady License Template using .NET classes
            // and returns hello XML string.

            //hello PlayReadyLicenseResponseTemplate class represents hello template for hello response sent back toohello end user.
            //It contains a field for a custom data string between hello license server and hello application
            //(may be useful for custom app logic) as well as a list of one or more license templates.
            PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();

            // hello PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
            // toobe returned toohello end users.
            //It contains hello data on hello content key in hello license and any rights or restrictions toobe
            //enforced by hello PlayReady DRM runtime when using hello content key.
            PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
            //Configure whether hello license is persistent (saved in persistent storage on hello client)
            //or non-persistent (only held in memory while hello player is using hello license).  
            licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;

            // AllowTestDevices controls whether test devices can use hello license or not.  
            // If true, hello MinimumSecurityLevel property of hello license
            // is set too150.  If false (hello default), hello MinimumSecurityLevel property of hello license is set too2000.
            licenseTemplate.AllowTestDevices = true;

            // You can also configure hello Play Right in hello PlayReady license by using hello PlayReadyPlayRight class.
            // It grants hello user hello ability tooplayback hello content subject toohello zero or more restrictions
            // configured in hello license and on hello PlayRight itself (for playback specific policy).
            // Much of hello policy on hello PlayRight has toodo with output restrictions
            // which control hello types of outputs that hello content can be played over and
            // any restrictions that must be put in place when using a given output.
            // For example, if hello DigitalVideoOnlyContentRestriction is enabled,
            //then hello DRM runtime will only allow hello video toobe displayed over digital outputs
            //(analog video outputs won’t be allowed toopass hello content).

            //IMPORTANT: These types of restrictions can be very powerful but can also affect hello consumer experience.
            // If hello output protections are configured too restrictive,
            // hello content might be unplayable on some clients. For more information, see hello PlayReady Compliance Rules document.

            // For example:
            //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);

            responseTemplate.LicenseTemplates.Add(licenseTemplate);

            return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
        }

        private static string ConfigureWidevineLicenseTemplate()
        {
            var template = new WidevineMessage
            {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                    new ContentKeySpecs
                    {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                    }
                },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
            };

            string configuration = JsonConvert.SerializeObject(template);
            return configuration;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            // Get hello PlayReady license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

            // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
            // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
            // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption
            // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
            // As a result Widevine license acquisition URL will have KID appended twice,
            // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

            Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
            UriBuilder uriBuilder = new UriBuilder(widevineUrl);
            uriBuilder.Query = String.Empty;
            widevineUrl = uriBuilder.Uri;

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                    {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}

            };

            // In this case we only specify Dash streaming protocol in hello delivery policy,
            // All other protocols will be blocked from streaming.
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }

        /// <summary>
        /// Gets hello streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator toohello streaming content on an origin.
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL toohello manifest file.
            return originLocator.Path + assetFile.Name;
        }

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int length)
        {
            var returnValue = new byte[length];

            using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
            {
            rng.GetBytes(returnValue);
            }

            return returnValue;
        }
        }
    }


## <a name="next-step"></a><span data-ttu-id="eacce-203">Další krok</span><span class="sxs-lookup"><span data-stu-id="eacce-203">Next step</span></span>
<span data-ttu-id="eacce-204">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="eacce-204">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="eacce-205">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="eacce-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="eacce-206">Viz také</span><span class="sxs-lookup"><span data-stu-id="eacce-206">See also</span></span>
[<span data-ttu-id="eacce-207">Šifrování CENC s více technologiemi DRM a řízením přístupu</span><span class="sxs-lookup"><span data-stu-id="eacce-207">CENC with Multi-DRM and Access Control</span></span>](media-services-cenc-with-multidrm-access-control.md)

[<span data-ttu-id="eacce-208">Konfigurace balení Widevine pomocí služby AMS</span><span class="sxs-lookup"><span data-stu-id="eacce-208">Configure Widevine packaging with AMS</span></span>](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[<span data-ttu-id="eacce-209">Uvedení služeb doručování licence Google Widevine ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="eacce-209">Announcing Google Widevine license delivery services in Azure Media Services</span></span>](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
