---
title: "aaaUsing AES-128 dynamické šifrování a klíč služby doručení | Microsoft Docs"
description: "Microsoft Azure Media Services umožňuje toodeliver můžete obsah šifrován šifrovacích klíčů AES 128 bitů. Služba Media Services také poskytuje služba hello doručení klíče, která zajišťuje šifrování klíče tooauthorized uživatelé. Toto téma ukazuje, jak toodynamically šifrování s AES-128 a pomocí služby doručení klíče hello."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: cb1b413ec2ba79f7437464099cf72236ab93f312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a><span data-ttu-id="bf20e-105">Použití dynamické šifrování AES-128 a doručení klíče služby</span><span class="sxs-lookup"><span data-stu-id="bf20e-105">Using AES-128 dynamic encryption and key delivery service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bf20e-106">.NET</span><span class="sxs-lookup"><span data-stu-id="bf20e-106">.NET</span></span>](media-services-protect-with-aes128.md)
> * [<span data-ttu-id="bf20e-107">Java</span><span class="sxs-lookup"><span data-stu-id="bf20e-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="bf20e-108">PHP</span><span class="sxs-lookup"><span data-stu-id="bf20e-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="bf20e-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="bf20e-109">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="bf20e-110">V tématu [to](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video přehled o tom, jak tooprotect médiu obsahu se šifrování AES.</span><span class="sxs-lookup"><span data-stu-id="bf20e-110">See [this](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video for an overview of how tooprotect your Media Content with AES encryption.</span></span>
> 
> 

<span data-ttu-id="bf20e-111">Microsoft Azure Media Services umožňuje toodeliver Http-Live-Streaming (HLS) a funkce Smooth Streams šifrované s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování).</span><span class="sxs-lookup"><span data-stu-id="bf20e-111">Microsoft Azure Media Services enables you toodeliver Http-Live-Streaming (HLS) and Smooth Streams encrypted with Advanced Encryption Standard (AES) (using 128-bit encryption keys).</span></span> <span data-ttu-id="bf20e-112">Služba Media Services také poskytuje služba hello doručení klíče, která zajišťuje šifrování klíče tooauthorized uživatelé.</span><span class="sxs-lookup"><span data-stu-id="bf20e-112">Media Services also provides hello Key Delivery service that delivers encryption keys tooauthorized users.</span></span> <span data-ttu-id="bf20e-113">Pokud chcete pro Media Services tooencrypt prostředek, třeba tooassociate šifrovací klíč assetu hello a taky nakonfigurovat zásady autorizace pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-113">If you want for Media Services tooencrypt an asset, you need tooassociate an encryption key with hello asset and also configure authorization policies for hello key.</span></span> <span data-ttu-id="bf20e-114">Když přehrávač vyžádá datový proud, služba Media Services použije hello zadané klíče toodynamically šifrování svůj obsah pomocí šifrování AES.</span><span class="sxs-lookup"><span data-stu-id="bf20e-114">When a stream is requested by a player, Media Services uses hello specified key toodynamically encrypt your content using AES encryption.</span></span> <span data-ttu-id="bf20e-115">datový proud hello toodecrypt, hello player bude požadovat hello klíč z hello doručení klíče služby.</span><span class="sxs-lookup"><span data-stu-id="bf20e-115">toodecrypt hello stream, hello player will request hello key from hello key delivery service.</span></span> <span data-ttu-id="bf20e-116">toodecide, jestli je uživatel hello autorizovaný tooget hello klíč, hello služba vyhodnocuje hello zásady autorizace, které jste zadali pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-116">toodecide whether or not hello user is authorized tooget hello key, hello service evaluates hello authorization policies that you specified for hello key.</span></span>

<span data-ttu-id="bf20e-117">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="bf20e-117">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="bf20e-118">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevření nebo token omezení.</span><span class="sxs-lookup"><span data-stu-id="bf20e-118">hello content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="bf20e-119">zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS).</span><span class="sxs-lookup"><span data-stu-id="bf20e-119">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="bf20e-120">Služba Media Services podporuje tokeny ve hello [jednoduchých webových tokenů](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) formátu (SWT) a [webových tokenů JSON](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) formátu (JWT).</span><span class="sxs-lookup"><span data-stu-id="bf20e-120">Media Services supports tokens in hello [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="bf20e-121">Další informace najdete v tématu [nakonfigurujte zásady autorizace hello klíč obsahu](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="bf20e-121">For more information, see [Configure hello content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span>

<span data-ttu-id="bf20e-122">tootake využívat dynamické šifrování, potřebujete toohave asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="bf20e-122">tootake advantage of dynamic encryption, you need toohave an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="bf20e-123">Budete také potřebovat zásady doručení hello tooconfigure pro prostředek hello (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="bf20e-123">You also need tooconfigure hello delivery policy for hello asset (described later in this topic).</span></span> <span data-ttu-id="bf20e-124">Pak na základě hello formátem zadaným v hello adresu URL streamování, server streamingu na vyžádání hello zajistí, že tento hello datový proud doručen v protokolu hello, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="bf20e-124">Then, based on hello format specified in hello streaming URL, hello On-Demand Streaming server will ensure that hello stream is delivered in hello protocol you have chosen.</span></span> <span data-ttu-id="bf20e-125">V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="bf20e-125">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="bf20e-126">Toto téma bude užitečné toodevelopers, které pracují v aplikacích, které doručují média chráněná.</span><span class="sxs-lookup"><span data-stu-id="bf20e-126">This topic would be useful toodevelopers that work on applications that deliver protected media.</span></span> <span data-ttu-id="bf20e-127">Hello téma ukazuje, jak tooconfigure hello doručení klíče služby pomocí zásad autorizace tak, aby pouze autorizovaní klienti mohli dostávat hello šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="bf20e-127">hello topic shows you how tooconfigure hello key delivery service with authorization policies so that only authorized clients could receive hello encryption keys.</span></span> <span data-ttu-id="bf20e-128">Také ukazuje, jak toouse dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-128">It also shows how toouse dynamic encryption.</span></span>


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a><span data-ttu-id="bf20e-129">Dynamické šifrování AES-128 a doručení klíče služby pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="bf20e-129">AES-128 Dynamic Encryption and Key Delivery Service Workflow</span></span>

<span data-ttu-id="bf20e-130">Hello následují obecné kroky, potřebovali byste tooperform při šifrování vaše prostředky pomocí standardu AES, pomocí služby doručení klíče hello Media Services a také používáte dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-130">hello following are general steps that you would need tooperform when encrypting your assets with AES, using hello Media Services key delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="bf20e-131">[Vytvoření assetu a nahrání souborů do hello asset](media-services-protect-with-aes128.md#create_asset).</span><span class="sxs-lookup"><span data-stu-id="bf20e-131">[Create an asset and upload files into hello asset](media-services-protect-with-aes128.md#create_asset).</span></span>
2. <span data-ttu-id="bf20e-132">[Zakódujte asset hello obsahující hello souboru toohello s adaptivní přenosovou rychlostí sady souborů MP4](media-services-protect-with-aes128.md#encode_asset).</span><span class="sxs-lookup"><span data-stu-id="bf20e-132">[Encode hello asset containing hello file toohello adaptive bitrate MP4 set](media-services-protect-with-aes128.md#encode_asset).</span></span>
3. <span data-ttu-id="bf20e-133">[Vytvoření klíče k obsahu a přidružte ji k hello kódovaný asset](media-services-protect-with-aes128.md#create_contentkey).</span><span class="sxs-lookup"><span data-stu-id="bf20e-133">[Create a content key and associate it with hello encoded asset](media-services-protect-with-aes128.md#create_contentkey).</span></span> <span data-ttu-id="bf20e-134">Ve službě Media Services obsahuje klíč obsahu hello hello asset šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="bf20e-134">In Media Services, hello content key contains hello asset’s encryption key.</span></span>
4. <span data-ttu-id="bf20e-135">[Nakonfigurujte zásady autorizace hello klíč obsahu](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="bf20e-135">[Configure hello content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span> <span data-ttu-id="bf20e-136">zásady autorizace klíče obsahu Hello musíte nakonfigurovat a splnit klient hello hello obsahu klíče toobe doručené toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="bf20e-136">hello content key authorization policy must be configured by you and met by hello client in order for hello content key toobe delivered toohello client.</span></span>
5. <span data-ttu-id="bf20e-137">[Konfigurace zásad doručení hello pro určitý prostředek](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span><span class="sxs-lookup"><span data-stu-id="bf20e-137">[Configure hello delivery policy for an asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span></span> <span data-ttu-id="bf20e-138">Konfigurace zásad doručení Hello zahrnuje: klíčů adresu URL pro získání a inicializační vektor (IV) (AES 128 vyžaduje hello stejné toobe IV zadaný při šifrování a dešifrování), typ hello doručovací protokol (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), dynamického šifrování (například obálky nebo žádné dynamické šifrování).</span><span class="sxs-lookup"><span data-stu-id="bf20e-138">hello delivery policy configuration includes: key acquisition URL and Initialization Vector (IV) (AES 128 requires hello same IV toobe supplied when encrypting and decrypting), delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), hello type of dynamic encryption (for example, envelope or no dynamic encryption).</span></span>

    <span data-ttu-id="bf20e-139">Může použít protokol tooeach jinou zásadu na hello stejné asset.</span><span class="sxs-lookup"><span data-stu-id="bf20e-139">You could apply different policy tooeach protocol on hello same asset.</span></span> <span data-ttu-id="bf20e-140">Můžete například použít šifrování PlayReady tooSmooth/DASH a pomocí standardu AES Envelope tooHLS.</span><span class="sxs-lookup"><span data-stu-id="bf20e-140">For example, you could apply PlayReady encryption tooSmooth/DASH and AES Envelope tooHLS.</span></span> <span data-ttu-id="bf20e-141">Veškeré protokoly, které nejsou v zásadách doručení definovány (například přidáte jedinou zásadu, která jako hello protokol určuje pouze HLS) budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="bf20e-141">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="bf20e-142">Výjimka toothis Hello je, pokud máte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-142">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="bf20e-143">Potom bude možné v hello zrušte všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="bf20e-143">Then, all protocols will be allowed in hello clear.</span></span>

6. <span data-ttu-id="bf20e-144">[Vytvořit lokátor OnDemand](media-services-protect-with-aes128.md#create_locator) v pořadí tooget adresu URL pro streamování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-144">[Create an OnDemand locator](media-services-protect-with-aes128.md#create_locator) in order tooget a streaming URL.</span></span>

<span data-ttu-id="bf20e-145">Hello téma také ukazuje [jak klientská aplikace požádejte o klíč ze služby doručení klíče hello](media-services-protect-with-aes128.md#client_request).</span><span class="sxs-lookup"><span data-stu-id="bf20e-145">hello topic also shows [how a client application can request a key from hello key delivery service](media-services-protect-with-aes128.md#client_request).</span></span>

<span data-ttu-id="bf20e-146">Najdete kompletní .NET [příklad](media-services-protect-with-aes128.md#example) na konci hello hello tématu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-146">You will find a complete .NET [example](media-services-protect-with-aes128.md#example) at hello end of hello topic.</span></span>

<span data-ttu-id="bf20e-147">Hello následující obrázek ukazuje pracovní postup hello popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="bf20e-147">hello following image demonstrates hello workflow described above.</span></span> <span data-ttu-id="bf20e-148">Zde hello token slouží k ověřování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-148">Here hello token is used for authentication.</span></span>

![Ochrana pomocí AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

<span data-ttu-id="bf20e-150">Hello zbývající část tohoto tématu poskytuje podrobné vysvětlení, ukázky kódu a tootopics odkazy, které ukazují, jak tooachieve hello výše popsané úlohy.</span><span class="sxs-lookup"><span data-stu-id="bf20e-150">hello rest of this topic provides detailed explanations, code examples, and links tootopics that show you how tooachieve hello tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="bf20e-151">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="bf20e-151">Current limitations</span></span>
<span data-ttu-id="bf20e-152">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="bf20e-152">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>

## <span data-ttu-id="bf20e-153"><a id="create_asset"></a>Vytvoření assetu a nahrání souborů do hello asset</span><span class="sxs-lookup"><span data-stu-id="bf20e-153"><a id="create_asset"></a>Create an asset and upload files into hello asset</span></span>
<span data-ttu-id="bf20e-154">V pořadí toomanage, kódovat a Streamovat videa, musíte nejprve nahrát obsah do Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="bf20e-154">In order toomanage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="bf20e-155">Po nahrání váš obsah bezpečně uložen v hello cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-155">Once uploaded, your content is stored securely in hello cloud for further processing and streaming.</span></span> 

<span data-ttu-id="bf20e-156">Podrobné informace najdete v článku o [nahrání souborů do účtu služby Media Services](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-156">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <span data-ttu-id="bf20e-157"><a id="encode_asset"></a>Kódování hello asset obsahující hello souboru toohello s adaptivní přenosovou rychlostí sady souborů MP4</span><span class="sxs-lookup"><span data-stu-id="bf20e-157"><a id="encode_asset"></a>Encode hello asset containing hello file toohello adaptive bitrate MP4 set</span></span>
<span data-ttu-id="bf20e-158">V případě dynamického šifrování je třeba je toocreate asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="bf20e-158">With dynamic encryption all you need is toocreate an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="bf20e-159">Pak na základě zadaného formátu hello v manifestu hello nebo fragmentovat požadavku, hello streamování na vyžádání server zajistí zobrazí datový proud hello hello protokolu, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="bf20e-159">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="bf20e-160">V důsledku toho stačí pouze toostore a platím hello souborů v jednom úložném formátu a služba Media Services bude sestavovat a dodávat hello odpovídající reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="bf20e-160">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span> <span data-ttu-id="bf20e-161">Další informace najdete v tématu hello [přehled dynamického balení](media-services-dynamic-packaging-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-161">For more information, see hello [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

>[!NOTE]
><span data-ttu-id="bf20e-162">Při vytvoření účtu AMS **výchozí** koncový bod streamování se přidá účet tooyour hello **Zastaveno** stavu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-162">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="bf20e-163">toostart streamování vašeho obsahu a proveďte výhod dynamického balení dynamické šifrování, hello streamování koncový bod, ze kterého mají být má obsah toostream toobe v hello **systémem** stavu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-163">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="bf20e-164">Navíc toobe možné toouse dynamické balením a dynamickým šifrováním váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="bf20e-164">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="bf20e-165">Návod, jak tooencode, najdete v části [jak tooencode assetu pomocí kodéru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-165">For instructions on how tooencode, see [How tooencode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="bf20e-166"><a id="create_contentkey"></a>Vytvoření klíče k obsahu a přidružte ji k asset hello kódování</span><span class="sxs-lookup"><span data-stu-id="bf20e-166"><a id="create_contentkey"></a>Create a content key and associate it with hello encoded asset</span></span>
<span data-ttu-id="bf20e-167">Ve službě Media Services obsahuje klíč obsahu hello hello klíče, které chcete tooencrypt prostředek s.</span><span class="sxs-lookup"><span data-stu-id="bf20e-167">In Media Services, hello content key contains hello key that you want tooencrypt an asset with.</span></span>

<span data-ttu-id="bf20e-168">Podrobné informace najdete v tématu [Vytvoření klíče k obsahu](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-168">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="bf20e-169"><a id="configure_key_auth_policy"></a>Nakonfigurujte zásady autorizace hello klíč obsahu</span><span class="sxs-lookup"><span data-stu-id="bf20e-169"><a id="configure_key_auth_policy"></a>Configure hello content key’s authorization policy</span></span>
<span data-ttu-id="bf20e-170">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="bf20e-170">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="bf20e-171">zásady autorizace klíče obsahu Hello musíte nakonfigurovat a splní hello klient (přehrávač) v pořadí pro klíče toobe hello doručit toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="bf20e-171">hello content key authorization policy must be configured by you and met by hello client (player) in order for hello key toobe delivered toohello client.</span></span> <span data-ttu-id="bf20e-172">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevřete, token omezení nebo omezení IP adres.</span><span class="sxs-lookup"><span data-stu-id="bf20e-172">hello content key authorization policy could have one or more authorization restrictions: open, token restriction, or IP restriction.</span></span>

<span data-ttu-id="bf20e-173">Podrobnější informace najdete v tématu [Konfigurace zásad autorizace pro klíč k obsahu](media-services-dotnet-configure-content-key-auth-policy.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-173">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md).</span></span>

## <span data-ttu-id="bf20e-174"><a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="bf20e-174"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="bf20e-175">Konfigurace zásad hello doručení pro asset.</span><span class="sxs-lookup"><span data-stu-id="bf20e-175">Configure hello delivery policy for your asset.</span></span> <span data-ttu-id="bf20e-176">Některé kroky, které hello asset konfigurace zásad doručení zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="bf20e-176">Some things that hello asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="bf20e-177">Adresa URL získávání klíčů Hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-177">hello Key acquisition URL.</span></span> 
* <span data-ttu-id="bf20e-178">Hello toouse inicializační vektor (IV) pro šifrování obálky hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-178">hello Initialization Vector (IV) toouse for hello envelope encryption.</span></span> <span data-ttu-id="bf20e-179">AES 128 vyžaduje hello stejné toobe IV zadaný při šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-179">AES 128 requires hello same IV toobe supplied when encrypting and decrypting.</span></span> 
* <span data-ttu-id="bf20e-180">Hello doručovací protokol assetu (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny).</span><span class="sxs-lookup"><span data-stu-id="bf20e-180">hello asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="bf20e-181">Hello typ dynamického šifrování (například pomocí standardu AES envelope) nebo žádné dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="bf20e-181">hello type of dynamic encryption (for example, AES envelope) or no dynamic encryption.</span></span> 

<span data-ttu-id="bf20e-182">Podrobné informace najdete v tématu [Konfigurace zásad doručení assetu ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-182">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="bf20e-183"><a id="create_locator"></a>Vytvořte v pořadí tooget adresu URL pro streamování Lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="bf20e-183"><a id="create_locator"></a>Create an OnDemand streaming locator in order tooget a streaming URL</span></span>
<span data-ttu-id="bf20e-184">Budete potřebovat tooprovide uživatelů s hello streamování adresu URL pro protokol Smooth, DASH nebo HLS.</span><span class="sxs-lookup"><span data-stu-id="bf20e-184">You will need tooprovide your user with hello streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="bf20e-185">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="bf20e-185">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
> 
> 

<span data-ttu-id="bf20e-186">Pokyny, jak toopublish prostředek a sestavení adresu URL pro streamování, najdete v části [sestavit adresu URL pro streamování](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-186">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="bf20e-187">Získání testovacího tokenu</span><span class="sxs-lookup"><span data-stu-id="bf20e-187">Get a test token</span></span>
<span data-ttu-id="bf20e-188">Získání testovacího tokenu podle hello tokenu omezení, která byla použita pro zásad autorizace pro klíč hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-188">Get a test token based on hello token restriction that was used for hello key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
    //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
    //so you have tooadd it in front of hello token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);

<span data-ttu-id="bf20e-189">Můžete použít hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest datového proudu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-189">You can use hello [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) tootest your stream.</span></span>

## <span data-ttu-id="bf20e-190"><a id="client_request"></a>Jak můžete vašeho klienta požadavku klíč z hello doručení klíče služby?</span><span class="sxs-lookup"><span data-stu-id="bf20e-190"><a id="client_request"></a>How can your client request a key from hello key delivery service?</span></span>
<span data-ttu-id="bf20e-191">V předchozím kroku hello zkonstruovat hello adresa URL, která odkazuje tooa souboru manifestu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-191">In hello previous step, you constructed hello URL that points tooa manifest file.</span></span> <span data-ttu-id="bf20e-192">Váš klient musí tooextract hello nezbytné informace z hello streamování souborů manifestu v pořadí toomake doručení klíče služby toohello požadavku.</span><span class="sxs-lookup"><span data-stu-id="bf20e-192">Your client needs tooextract hello necessary information from hello streaming manifest files in order toomake a request toohello key delivery service.</span></span>

### <a name="manifest-files"></a><span data-ttu-id="bf20e-193">Soubory manifestu</span><span class="sxs-lookup"><span data-stu-id="bf20e-193">Manifest files</span></span>
<span data-ttu-id="bf20e-194">Hello klient potřebuje hello adresy URL tooextract (který také obsahuje klíč obsahu Id (dětský)) hodnotu ze souboru manifestu hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-194">hello client needs tooextract hello URL (that also contains content key Id (kid)) value from hello manifest file.</span></span> <span data-ttu-id="bf20e-195">Hello klienta a zkuste to tooget hello šifrovací klíč ze služby doručení klíče hello.</span><span class="sxs-lookup"><span data-stu-id="bf20e-195">hello client will then try tooget hello encryption key from hello key delivery service.</span></span> <span data-ttu-id="bf20e-196">Hello klienta taky musí být hodnota hello IV tooextract a použít ho dešifrovat hello stream.hello následující fragment kódu ukazuje hello <Protection> element hello technologie Smooth Streaming manifestu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-196">hello client also needs tooextract hello IV value and use it do decrypt hello stream.hello following snippet shows hello <Protection> element of hello Smooth Streaming manifest.</span></span>

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

<span data-ttu-id="bf20e-197">V případě hello HLS manifest kořenové hello je rozděleno do segmentu souborů.</span><span class="sxs-lookup"><span data-stu-id="bf20e-197">In hello case of HLS, hello root manifest is broken into segment files.</span></span> 

<span data-ttu-id="bf20e-198">Například hello kořenové manifest je: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) a obsahuje seznam názvů souborů segmentu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-198">For example, hello root manifest is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) and it contains a list of segment file names.</span></span>

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

<span data-ttu-id="bf20e-199">Pokud jeden z hello segment souborů otevřete v textovém editoru (například http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should obsahovat #EXT-X-KEY, který označuje, že tento soubor hello je zašifrován.</span><span class="sxs-lookup"><span data-stu-id="bf20e-199">If you open one of hello segment files in text editor (for example, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain #EXT-X-KEY which indicates that hello file is encrypted.</span></span>

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
><span data-ttu-id="bf20e-200">Pokud plánujete tooplay AES šifrovat HLS v prohlížeči Safari najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="bf20e-200">If you are planning tooplay an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

### <a name="request-hello-key-from-hello-key-delivery-service"></a><span data-ttu-id="bf20e-201">Žádost o hello klíč z hello doručení klíče služby</span><span class="sxs-lookup"><span data-stu-id="bf20e-201">Request hello key from hello key delivery service</span></span>

<span data-ttu-id="bf20e-202">Hello následující kód ukazuje, jak toosend požadavku toohello Media Services klíče služby doručení pomocí doručení klíče identifikátor Uri (extrahovaný z manifestu hello) a token (v tomto tématu není mluvit o tom, jak tooget jednoduché webové tokeny od služby tokenů zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="bf20e-202">hello following code shows how toosend a request toohello Media Services key delivery service using a key delivery Uri (that was extracted from hello manifest) and a token (this topic does not talk about how tooget Simple Web Tokens from a Secure Token Service).</span></span>

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-using-net"></a><span data-ttu-id="bf20e-203">Chránit obsah s AES-128 pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="bf20e-203">Protect your content with AES-128 using .NET</span></span>

### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="bf20e-204">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="bf20e-204">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="bf20e-205">Nastavení vývojového prostředí a naplnění souboru app.config hello s informace o připojení, jak je popsáno v [vývoj pro Media Services s .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="bf20e-205">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="bf20e-206">Přidejte následující prvky příliš hello**appSettings** definované v souboru app.config:</span><span class="sxs-lookup"><span data-stu-id="bf20e-206">Add hello following elements too**appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <span data-ttu-id="bf20e-207"><a id="example"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="bf20e-207"><a id="example"></a>Example</span></span>

<span data-ttu-id="bf20e-208">Přepište hello kód v souboru Program.cs kódem hello uvedené v této sekci.</span><span class="sxs-lookup"><span data-stu-id="bf20e-208">Overwrite hello code in your Program.cs file with hello code shown in this section.</span></span>
 
>[!NOTE]
><span data-ttu-id="bf20e-209">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="bf20e-209">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="bf20e-210">Měli byste použít hello stejné ID zásad, pokud vždy používáte hello stejné dny / přístupová oprávnění, například zásady pro lokátory, které jsou určený tooremain zavedené po dlouhou dobu (bez odeslání zásady).</span><span class="sxs-lookup"><span data-stu-id="bf20e-210">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="bf20e-211">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="bf20e-211">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="bf20e-212">Ujistěte se že proměnné tooupdate toopoint toofolders kde jsou umístěné vaše vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="bf20e-212">Make sure tooupdate variables toopoint toofolders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Security.Cryptography;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;

    namespace AESDynamicEncryptionAndKeyDeliverySvc
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing hello issuer of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // hello Audience or Scope of hello token.  
        // Must match hello value in hello token for hello token toobe considered valid.
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

            IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for hello asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on hello data in hello given TokenRestrictionTemplate.
            // Note, you need toopass hello key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during hello creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //hello GenerateTestToken method returns hello token without hello word “Bearer” in front
            //so you have tooadd it in front of hello token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("hello authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use hello bit.ly/aesplayer Flash player tootest hello URL 
            // (with open authorization policy). 
            // Paste hello URL and click hello Update button tooplay hello video. 
            //
            string URL = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
            Console.WriteLine();
            Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
            Console.WriteLine();

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
            IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);


            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with hello encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.StorageEncrypted);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }

        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }

        static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
        {
            // Create envelope encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                keyId,
                contentKey,
                "ContentKey",
                ContentKeyType.EnvelopeEncryption);

            // Associate hello key with hello asset.
            asset.ContentKeys.Add(key);

            return key;
        }

        static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
        {
            // Create ContentKeyAuthorizationPolicy with Open restrictions 
            // and create authorization policy             
            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Open Authorization Policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
            Name = "HLS Open Authorization Policy",
            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
            Requirements = null // no requirements needed for HLS
        };

            restrictions.Add(restriction);

            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "policy",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            "");

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);
        }

        public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
        {
            string tokenTemplateString = GenerateTokenRequirements();

            IContentKeyAuthorizationPolicy policy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("HLS token restricted authorization policy").Result;

            List<ContentKeyAuthorizationPolicyRestriction> restrictions =
            new List<ContentKeyAuthorizationPolicyRestriction>();

            ContentKeyAuthorizationPolicyRestriction restriction =
            new ContentKeyAuthorizationPolicyRestriction
            {
                Name = "Token Authorization Policy",
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString
            };

            restrictions.Add(restriction);

            //You could have multiple options 
            IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create(
            "Token option for HLS",
            ContentKeyDeliveryType.BaselineHttp,
            restrictions,
            null  // no key delivery data is needed for HLS
            );

            policy.Options.Add(policyOption);

            // Add ContentKeyAutorizationPolicy tooContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key tooAsset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose tooassociate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in hello key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  hello URL does not contains a key ID

            // hello following policy configuration specifies: 
            // key url that will have KID=<Guid> appended toohello envelope and
            // hello Initialization Vector (IV) toouse for hello envelope encryption.

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
            };

            IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference toohello streaming manifest file from hello  
            // collection of files in hello asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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

        static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
            ((IJob)sender).Name,
            e.CurrentState,
            DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
        }

        static private byte[] GetRandomBuffer(int size)
        {
            byte[] randomBytes = new byte[size];
            using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
            {
            rng.GetBytes(randomBytes);
            }

            return randomBytes;
        }
        }
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="bf20e-213">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="bf20e-213">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bf20e-214">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="bf20e-214">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

