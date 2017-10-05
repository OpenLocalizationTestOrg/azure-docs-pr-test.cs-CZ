---
title: "Použití dynamické šifrování AES-128 a doručení klíče služby | Microsoft Docs"
description: "Microsoft Azure Media Services umožňuje doručovat obsah šifrován šifrovacích klíčů AES 128 bitů. Služba Media Services také poskytuje službu doručování klíč, který doručí šifrovací klíče na oprávněné uživatele. Toto téma ukazuje, jak dynamicky šifrovat pomocí standardu AES-128 a používat službu doručení klíče."
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
ms.openlocfilehash: ae1b36c26e688e74eb8fcc1a4cdbd3be0c014c08
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a><span data-ttu-id="1fee4-105">Použití dynamické šifrování AES-128 a doručení klíče služby</span><span class="sxs-lookup"><span data-stu-id="1fee4-105">Using AES-128 dynamic encryption and key delivery service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1fee4-106">.NET</span><span class="sxs-lookup"><span data-stu-id="1fee4-106">.NET</span></span>](media-services-protect-with-aes128.md)
> * [<span data-ttu-id="1fee4-107">Java</span><span class="sxs-lookup"><span data-stu-id="1fee4-107">Java</span></span>](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [<span data-ttu-id="1fee4-108">PHP</span><span class="sxs-lookup"><span data-stu-id="1fee4-108">PHP</span></span>](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a><span data-ttu-id="1fee4-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="1fee4-109">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="1fee4-110">V tématu [to](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video přehled o tom, jak chránit obsah vašeho média s šifrování AES.</span><span class="sxs-lookup"><span data-stu-id="1fee4-110">See [this](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) video for an overview of how to protect your Media Content with AES encryption.</span></span>
> 
> 

<span data-ttu-id="1fee4-111">Microsoft Azure Media Services umožňuje doručovat Http-Live-Streaming (HLS) a funkce Smooth Streams šifrované s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování).</span><span class="sxs-lookup"><span data-stu-id="1fee4-111">Microsoft Azure Media Services enables you to deliver Http-Live-Streaming (HLS) and Smooth Streams encrypted with Advanced Encryption Standard (AES) (using 128-bit encryption keys).</span></span> <span data-ttu-id="1fee4-112">Služba Media Services také poskytuje službu doručování klíč, který doručí šifrovací klíče na oprávněné uživatele.</span><span class="sxs-lookup"><span data-stu-id="1fee4-112">Media Services also provides the Key Delivery service that delivers encryption keys to authorized users.</span></span> <span data-ttu-id="1fee4-113">Pokud chcete použít pro Media Services k šifrování prostředek, budete muset přidružit šifrovací klíč assetu a taky nakonfigurovat zásady autorizace pro klíč.</span><span class="sxs-lookup"><span data-stu-id="1fee4-113">If you want for Media Services to encrypt an asset, you need to associate an encryption key with the asset and also configure authorization policies for the key.</span></span> <span data-ttu-id="1fee4-114">Datový proud je žádost přehrávač, Media Services používá k zadanému klíči dynamicky šifrovat obsah pomocí šifrování AES.</span><span class="sxs-lookup"><span data-stu-id="1fee4-114">When a stream is requested by a player, Media Services uses the specified key to dynamically encrypt your content using AES encryption.</span></span> <span data-ttu-id="1fee4-115">K dešifrování datového proudu, bude přehrávač požadovat klíč ze služby doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="1fee4-115">To decrypt the stream, the player will request the key from the key delivery service.</span></span> <span data-ttu-id="1fee4-116">Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.</span><span class="sxs-lookup"><span data-stu-id="1fee4-116">To decide whether or not the user is authorized to get the key, the service evaluates the authorization policies that you specified for the key.</span></span>

<span data-ttu-id="1fee4-117">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="1fee4-117">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="1fee4-118">Zásady autorizace pro klíč k obsahu mohou obsahovat jedno nebo více omezení autorizace: otevřené omezení nebo omezení s tokenem.</span><span class="sxs-lookup"><span data-stu-id="1fee4-118">The content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="1fee4-119">Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS).</span><span class="sxs-lookup"><span data-stu-id="1fee4-119">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="1fee4-120">Služba Media Services podporuje tokeny ve  formátu [jednoduchých webových tokenů](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) a [webových tokenů JSON](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT).</span><span class="sxs-lookup"><span data-stu-id="1fee4-120">Media Services supports tokens in the [Simple Web Tokens](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) format and [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) format.</span></span> <span data-ttu-id="1fee4-121">Další informace najdete v tématu [nakonfigurujte zásady autorizace klíče obsahu](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="1fee4-121">For more information, see [Configure the content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span>

<span data-ttu-id="1fee4-122">Abyste mohli využívat dynamické šifrování, je třeba mít asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming s více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="1fee4-122">To take advantage of dynamic encryption, you need to have an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="1fee4-123">Také musíte nakonfigurovat zásady doručení pro asset (popsáno dále v tomto tématu).</span><span class="sxs-lookup"><span data-stu-id="1fee4-123">You also need to configure the delivery policy for the asset (described later in this topic).</span></span> <span data-ttu-id="1fee4-124">Potom, na základě formátu určeného v adrese URL streamování, server streamingu na vyžádání zajistí, aby byl datový proud doručen v protokolu podle vašeho výběru.</span><span class="sxs-lookup"><span data-stu-id="1fee4-124">Then, based on the format specified in the streaming URL, the On-Demand Streaming server will ensure that the stream is delivered in the protocol you have chosen.</span></span> <span data-ttu-id="1fee4-125">Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="1fee4-125">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="1fee4-126">Toto téma bude užitečné pro vývojáře pracující na aplikacích, které doručují média chráněná.</span><span class="sxs-lookup"><span data-stu-id="1fee4-126">This topic would be useful to developers that work on applications that deliver protected media.</span></span> <span data-ttu-id="1fee4-127">Téma ukazuje, jak nakonfigurovat službu doručení klíče pomocí zásad autorizace tak, aby pouze autorizovaní klienti mohli dostávat šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="1fee4-127">The topic shows you how to configure the key delivery service with authorization policies so that only authorized clients could receive the encryption keys.</span></span> <span data-ttu-id="1fee4-128">Také ukazuje, jak používat dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="1fee4-128">It also shows how to use dynamic encryption.</span></span>


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a><span data-ttu-id="1fee4-129">Dynamické šifrování AES-128 a doručení klíče služby pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="1fee4-129">AES-128 Dynamic Encryption and Key Delivery Service Workflow</span></span>

<span data-ttu-id="1fee4-130">Níže jsou uvedeny základní kroky, které je třeba provést při šifrování vaše prostředky pomocí standardu AES, pomocí služby Media Services doručení klíče a také používáte dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="1fee4-130">The following are general steps that you would need to perform when encrypting your assets with AES, using the Media Services key delivery service, and also using dynamic encryption.</span></span>

1. <span data-ttu-id="1fee4-131">[Vytvoření assetu a nahrání souborů do assetu](media-services-protect-with-aes128.md#create_asset).</span><span class="sxs-lookup"><span data-stu-id="1fee4-131">[Create an asset and upload files into the asset](media-services-protect-with-aes128.md#create_asset).</span></span>
2. <span data-ttu-id="1fee4-132">[Zakódujte asset obsahující soubor do sady souborů MP4 s adaptivní přenosovou](media-services-protect-with-aes128.md#encode_asset).</span><span class="sxs-lookup"><span data-stu-id="1fee4-132">[Encode the asset containing the file to the adaptive bitrate MP4 set](media-services-protect-with-aes128.md#encode_asset).</span></span>
3. <span data-ttu-id="1fee4-133">[Vytvořte klíč obsahu a přidružte ji k zakódovanému assetu](media-services-protect-with-aes128.md#create_contentkey).</span><span class="sxs-lookup"><span data-stu-id="1fee4-133">[Create a content key and associate it with the encoded asset](media-services-protect-with-aes128.md#create_contentkey).</span></span> <span data-ttu-id="1fee4-134">Klíč k obsahu ve službě Media Services obsahuje šifrovací klíč assetu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-134">In Media Services, the content key contains the asset’s encryption key.</span></span>
4. <span data-ttu-id="1fee4-135">[Nakonfigurujte zásady autorizace klíče obsahu](media-services-protect-with-aes128.md#configure_key_auth_policy).</span><span class="sxs-lookup"><span data-stu-id="1fee4-135">[Configure the content key’s authorization policy](media-services-protect-with-aes128.md#configure_key_auth_policy).</span></span> <span data-ttu-id="1fee4-136">Zásady autorizace pro klíč k obsahu musíte vy nakonfigurovat a klient splnit, aby bylo možné mu klíč k obsahu doručit.</span><span class="sxs-lookup"><span data-stu-id="1fee4-136">The content key authorization policy must be configured by you and met by the client in order for the content key to be delivered to the client.</span></span>
5. <span data-ttu-id="1fee4-137">[Konfigurace zásad doručení pro asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span><span class="sxs-lookup"><span data-stu-id="1fee4-137">[Configure the delivery policy for an asset](media-services-protect-with-aes128.md#configure_asset_delivery_policy).</span></span> <span data-ttu-id="1fee4-138">Konfigurace zásad doručení zahrnuje: klíčů adresu URL pro získání a inicializační vektor (IV) (AES 128 vyžaduje stejné IV je nutné zadat při šifrování a dešifrování), doručovací protokol (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny), typ dynamického šifrování (například obálky nebo žádné dynamické šifrování).</span><span class="sxs-lookup"><span data-stu-id="1fee4-138">The delivery policy configuration includes: key acquisition URL and Initialization Vector (IV) (AES 128 requires the same IV to be supplied when encrypting and decrypting), delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all), the type of dynamic encryption (for example, envelope or no dynamic encryption).</span></span>

    <span data-ttu-id="1fee4-139">Na každý protokol stejného assetu můžete použít jiné zásady.</span><span class="sxs-lookup"><span data-stu-id="1fee4-139">You could apply different policy to each protocol on the same asset.</span></span> <span data-ttu-id="1fee4-140">Můžete například použít šifrování PlayReady na protokol Smooth/DASH a šifrování pomocí standardu AES Envelope na protokol HLS.</span><span class="sxs-lookup"><span data-stu-id="1fee4-140">For example, you could apply PlayReady encryption to Smooth/DASH and AES Envelope to HLS.</span></span> <span data-ttu-id="1fee4-141">Veškeré protokoly, které nejsou v zásadách doručení definovány (například když přidáte jedinou zásadu, která jako protokol určuje pouze HLS), budou při streamování blokovány.</span><span class="sxs-lookup"><span data-stu-id="1fee4-141">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="1fee4-142">Výjimkou je, pokud nemáte definovány vůbec žádné zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-142">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="1fee4-143">Pak budou všechny protokoly povolené v nešifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="1fee4-143">Then, all protocols will be allowed in the clear.</span></span>

6. <span data-ttu-id="1fee4-144">[Vytvořit lokátor OnDemand](media-services-protect-with-aes128.md#create_locator) Chcete-li získat adresu URL streamování.</span><span class="sxs-lookup"><span data-stu-id="1fee4-144">[Create an OnDemand locator](media-services-protect-with-aes128.md#create_locator) in order to get a streaming URL.</span></span>

<span data-ttu-id="1fee4-145">Téma také ukazuje [jak klientská aplikace požádejte o klíč ze služby doručení klíče](media-services-protect-with-aes128.md#client_request).</span><span class="sxs-lookup"><span data-stu-id="1fee4-145">The topic also shows [how a client application can request a key from the key delivery service](media-services-protect-with-aes128.md#client_request).</span></span>

<span data-ttu-id="1fee4-146">Najdete kompletní .NET [příklad](media-services-protect-with-aes128.md#example) na konci tohoto tématu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-146">You will find a complete .NET [example](media-services-protect-with-aes128.md#example) at the end of the topic.</span></span>

<span data-ttu-id="1fee4-147">Následující obrázek znázorňuje pracovní postup popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="1fee4-147">The following image demonstrates the workflow described above.</span></span> <span data-ttu-id="1fee4-148">Zde se k ověření používá token.</span><span class="sxs-lookup"><span data-stu-id="1fee4-148">Here the token is used for authentication.</span></span>

![Ochrana pomocí AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

<span data-ttu-id="1fee4-150">Zbývající část tohoto tématu poskytuje podrobné vysvětlení, ukázky kódů a odkazy na témata, která ukazují, jak dokončit výše popsané úlohy.</span><span class="sxs-lookup"><span data-stu-id="1fee4-150">The rest of this topic provides detailed explanations, code examples, and links to topics that show you how to achieve the tasks described above.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="1fee4-151">Aktuální omezení</span><span class="sxs-lookup"><span data-stu-id="1fee4-151">Current limitations</span></span>
<span data-ttu-id="1fee4-152">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="1fee4-152">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>

## <span data-ttu-id="1fee4-153"><a id="create_asset"></a>Vytvoření assetu a nahrání souborů do assetu</span><span class="sxs-lookup"><span data-stu-id="1fee4-153"><a id="create_asset"></a>Create an asset and upload files into the asset</span></span>
<span data-ttu-id="1fee4-154">Aby bylo možné spravovat, kódovat a streamovat videa, musíte nejprve nahrát obsah do služby Microsoft Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="1fee4-154">In order to manage, encode, and stream your videos, you must first upload your content into Microsoft Azure Media Services.</span></span> <span data-ttu-id="1fee4-155">Po nahrání bude váš obsah bezpečně uložen v cloudu pro další zpracování a streamování.</span><span class="sxs-lookup"><span data-stu-id="1fee4-155">Once uploaded, your content is stored securely in the cloud for further processing and streaming.</span></span> 

<span data-ttu-id="1fee4-156">Podrobné informace najdete v článku o [nahrání souborů do účtu služby Media Services](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-156">For detailed information, see [Upload Files into a Media Services account](media-services-dotnet-upload-files.md).</span></span>

## <span data-ttu-id="1fee4-157"><a id="encode_asset"></a>Zakódujte asset obsahující soubor s adaptivní přenosovou rychlostí sady souborů MP4</span><span class="sxs-lookup"><span data-stu-id="1fee4-157"><a id="encode_asset"></a>Encode the asset containing the file to the adaptive bitrate MP4 set</span></span>
<span data-ttu-id="1fee4-158">V případě dynamického šifrování je třeba pouze vytvořit asset, který obsahuje sadu souborů MP4 s více přenosovými rychlostmi nebo zdrojové soubory technologie Smooth Streaming s více přenosovými rychlostmi.</span><span class="sxs-lookup"><span data-stu-id="1fee4-158">With dynamic encryption all you need is to create an asset that contains a set of multi-bitrate MP4 files or multi-bitrate Smooth Streaming source files.</span></span> <span data-ttu-id="1fee4-159">Pak na základě formátu určeného v manifestu nebo fragmentu požadavek streamingu na vyžádání serveru zajistí datový proud obdrželi v protokolu, kterou jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="1fee4-159">Then, based on the specified format in the manifest or fragment request, the On-Demand Streaming server will ensure that you receive the stream in the protocol you have chosen.</span></span> <span data-ttu-id="1fee4-160">Díky tomu pak stačí uložit (a platit) soubory pouze v jednom úložném formátu a služba Media Services bude sestavovat a dodávat vhodný formát streamování v reakci na požadavky klientů.</span><span class="sxs-lookup"><span data-stu-id="1fee4-160">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span> <span data-ttu-id="1fee4-161">Další informace najdete v tématu [Přehled dynamického balení](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-161">For more information, see the [Dynamic Packaging Overview](media-services-dynamic-packaging-overview.md) topic.</span></span>

>[!NOTE]
><span data-ttu-id="1fee4-162">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="1fee4-162">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="1fee4-163">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="1fee4-163">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="1fee4-164">Abyste mohli používat dynamické balením a dynamickým šifrováním také váš asset musí obsahovat sadu s adaptivní přenosovou rychlostí soubory MP4 s rychlostmi nebo soubory technologie Smooth Streaming s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="1fee4-164">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="1fee4-165">Pokyny ke kódování najdete v článku o [kódování assetu pomocí kodéru Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-165">For instructions on how to encode, see [How to encode an asset using Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>

## <span data-ttu-id="1fee4-166"><a id="create_contentkey"></a>Vytvoření klíče k obsahu a jeho přiřazení k zakódovanému assetu</span><span class="sxs-lookup"><span data-stu-id="1fee4-166"><a id="create_contentkey"></a>Create a content key and associate it with the encoded asset</span></span>
<span data-ttu-id="1fee4-167">Klíč k obsahu ve službě Media Services obsahuje klíč, kterým chcete asset šifrovat.</span><span class="sxs-lookup"><span data-stu-id="1fee4-167">In Media Services, the content key contains the key that you want to encrypt an asset with.</span></span>

<span data-ttu-id="1fee4-168">Podrobné informace najdete v tématu [Vytvoření klíče k obsahu](media-services-dotnet-create-contentkey.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-168">For detailed information, see [Create content key](media-services-dotnet-create-contentkey.md).</span></span>

## <span data-ttu-id="1fee4-169"><a id="configure_key_auth_policy"></a>Konfigurace zásad autorizace klíče obsahu</span><span class="sxs-lookup"><span data-stu-id="1fee4-169"><a id="configure_key_auth_policy"></a>Configure the content key’s authorization policy</span></span>
<span data-ttu-id="1fee4-170">Služba Media Services podporuje více způsobů ověřování uživatelů, kteří žádají o klíč.</span><span class="sxs-lookup"><span data-stu-id="1fee4-170">Media Services supports multiple ways of authenticating users who make key requests.</span></span> <span data-ttu-id="1fee4-171">Zásady autorizace pro klíč k obsahu musíte vy nakonfigurovat a klient (přehrávač) splnit, aby bylo možné mu klíč doručit.</span><span class="sxs-lookup"><span data-stu-id="1fee4-171">The content key authorization policy must be configured by you and met by the client (player) in order for the key to be delivered to the client.</span></span> <span data-ttu-id="1fee4-172">Zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: otevřete, token omezení nebo omezení IP adres.</span><span class="sxs-lookup"><span data-stu-id="1fee4-172">The content key authorization policy could have one or more authorization restrictions: open, token restriction, or IP restriction.</span></span>

<span data-ttu-id="1fee4-173">Podrobnější informace najdete v tématu [Konfigurace zásad autorizace pro klíč k obsahu](media-services-dotnet-configure-content-key-auth-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-173">For detailed information, see [Configure Content Key Authorization Policy](media-services-dotnet-configure-content-key-auth-policy.md).</span></span>

## <span data-ttu-id="1fee4-174"><a id="configure_asset_delivery_policy"></a>Konfigurace zásad doručení assetu</span><span class="sxs-lookup"><span data-stu-id="1fee4-174"><a id="configure_asset_delivery_policy"></a>Configure asset delivery policy</span></span>
<span data-ttu-id="1fee4-175">Nakonfigurujte zásady doručení pro asset.</span><span class="sxs-lookup"><span data-stu-id="1fee4-175">Configure the delivery policy for your asset.</span></span> <span data-ttu-id="1fee4-176">Konfigurace zásad doručení assetu zahrnuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="1fee4-176">Some things that the asset delivery policy configuration includes:</span></span>

* <span data-ttu-id="1fee4-177">Adresa URL získávání klíčů.</span><span class="sxs-lookup"><span data-stu-id="1fee4-177">The Key acquisition URL.</span></span> 
* <span data-ttu-id="1fee4-178">Inicializace vektoru (IV) má použít pro šifrování obálku.</span><span class="sxs-lookup"><span data-stu-id="1fee4-178">The Initialization Vector (IV) to use for the envelope encryption.</span></span> <span data-ttu-id="1fee4-179">AES 128 vyžaduje stejné IV je nutné zadat při šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="1fee4-179">AES 128 requires the same IV to be supplied when encrypting and decrypting.</span></span> 
* <span data-ttu-id="1fee4-180">Doručovací protokol assetu (například MPEG DASH, HLS, technologie Smooth Streaming nebo všechny).</span><span class="sxs-lookup"><span data-stu-id="1fee4-180">The asset delivery protocol (for example, MPEG DASH, HLS, Smooth Streaming or all).</span></span>
* <span data-ttu-id="1fee4-181">Typ dynamického šifrování (například pomocí standardu AES envelope) nebo žádné dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="1fee4-181">The type of dynamic encryption (for example, AES envelope) or no dynamic encryption.</span></span> 

<span data-ttu-id="1fee4-182">Podrobné informace najdete v tématu [Konfigurace zásad doručení assetu ](media-services-rest-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-182">For detailed information, see [Configure asset delivery policy ](media-services-rest-configure-asset-delivery-policy.md).</span></span>

## <span data-ttu-id="1fee4-183"><a id="create_locator"></a>Pokud chcete získat adresu URL streamování, vytvořte lokátor streamování OnDemand.</span><span class="sxs-lookup"><span data-stu-id="1fee4-183"><a id="create_locator"></a>Create an OnDemand streaming locator in order to get a streaming URL</span></span>
<span data-ttu-id="1fee4-184">Uživateli je třeba poskytnout adresu URL streamování pro protokol Smooth, DASH nebo HLS.</span><span class="sxs-lookup"><span data-stu-id="1fee4-184">You will need to provide your user with the streaming URL for Smooth, DASH or HLS.</span></span>

> [!NOTE]
> <span data-ttu-id="1fee4-185">Pokud přidáte nebo aktualizujete zásady pro doručení assetu, musíte odstranit stávající lokátor (pokud existuje) a vytvořit nový.</span><span class="sxs-lookup"><span data-stu-id="1fee4-185">If you add or update your asset’s delivery policy, you must delete an existing locator (if any) and create a new locator.</span></span>
> 
> 

<span data-ttu-id="1fee4-186">Pokyny k publikování assetu a vytvoření adresy URL streamování najdete v článku o [vytvoření adresy URL streamování](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-186">For instructions on how to publish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="get-a-test-token"></a><span data-ttu-id="1fee4-187">Získání testovacího tokenu</span><span class="sxs-lookup"><span data-stu-id="1fee4-187">Get a test token</span></span>
<span data-ttu-id="1fee4-188">Získejte testovací token na základě omezení s tokenem, které se používá v zásadách autorizace klíče.</span><span class="sxs-lookup"><span data-stu-id="1fee4-188">Get a test token based on the token restriction that was used for the key authorization policy.</span></span>

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

<span data-ttu-id="1fee4-189">K testování datového proudu můžete použít [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="1fee4-189">You can use the [AMS Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) to test your stream.</span></span>

## <span data-ttu-id="1fee4-190"><a id="client_request"></a>Jak můžete vašeho klienta požadavku klíč ze služby doručení klíče?</span><span class="sxs-lookup"><span data-stu-id="1fee4-190"><a id="client_request"></a>How can your client request a key from the key delivery service?</span></span>
<span data-ttu-id="1fee4-191">V předchozím kroku sestavit adresu URL, která odkazuje na soubor manifestu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-191">In the previous step, you constructed the URL that points to a manifest file.</span></span> <span data-ttu-id="1fee4-192">Váš klient je potřeba extrahovat nezbytné informace z datových proudů souborů manifestu aby bylo možné žádost službě doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="1fee4-192">Your client needs to extract the necessary information from the streaming manifest files in order to make a request to the key delivery service.</span></span>

### <a name="manifest-files"></a><span data-ttu-id="1fee4-193">Soubory manifestu</span><span class="sxs-lookup"><span data-stu-id="1fee4-193">Manifest files</span></span>
<span data-ttu-id="1fee4-194">Klient musí k extrakci adresu URL (který také obsahuje klíč obsahu Id (dětský)) hodnotu ze souboru manifestu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-194">The client needs to extract the URL (that also contains content key Id (kid)) value from the manifest file.</span></span> <span data-ttu-id="1fee4-195">Klient se potom pokusí získat šifrovací klíč ze služby doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="1fee4-195">The client will then try to get the encryption key from the key delivery service.</span></span> <span data-ttu-id="1fee4-196">Klient musí také k extrakci IV hodnota a použít ho dešifrovat datového proudu. Následující fragment kódu ukazuje <Protection> element manifestu technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="1fee4-196">The client also needs to extract the IV value and use it do decrypt the stream.The following snippet shows the <Protection> element of the Smooth Streaming manifest.</span></span>

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

<span data-ttu-id="1fee4-197">V případě HLS kořenové manifest je rozděleno do segmentu souborů.</span><span class="sxs-lookup"><span data-stu-id="1fee4-197">In the case of HLS, the root manifest is broken into segment files.</span></span> 

<span data-ttu-id="1fee4-198">Například kořenový manifest je: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) a obsahuje seznam názvů souborů segmentu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-198">For example, the root manifest is: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) and it contains a list of segment file names.</span></span>

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

<span data-ttu-id="1fee4-199">Pokud je jeden ze souborů segment otevřete v textovém editoru (například http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), měl by obsahovat #EXT-X-KEY, který označuje, že soubor je šifrován.</span><span class="sxs-lookup"><span data-stu-id="1fee4-199">If you open one of the segment files in text editor (for example, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it should contain #EXT-X-KEY which indicates that the file is encrypted.</span></span>

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
><span data-ttu-id="1fee4-200">Pokud máte v úmyslu přehrání AES šifrovat HLS v prohlížeči Safari najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="1fee4-200">If you are planning to play an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

### <a name="request-the-key-from-the-key-delivery-service"></a><span data-ttu-id="1fee4-201">Klíč žádat službu doručení klíče</span><span class="sxs-lookup"><span data-stu-id="1fee4-201">Request the key from the key delivery service</span></span>

<span data-ttu-id="1fee4-202">Následující kód ukazuje, jak odeslat žádost o doručení klíče službě Media Services pomocí doručení klíče identifikátor Uri (extrahovaný z manifestu) a token (v tomto tématu není mluvit o tom, jak získat jednoduchých webových tokenů od služby tokenů zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="1fee4-202">The following code shows how to send a request to the Media Services key delivery service using a key delivery Uri (that was extracted from the manifest) and a token (this topic does not talk about how to get Simple Web Tokens from a Secure Token Service).</span></span>

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

## <a name="protect-your-content-with-aes-128-using-net"></a><span data-ttu-id="1fee4-203">Chránit obsah s AES-128 pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="1fee4-203">Protect your content with AES-128 using .NET</span></span>

### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="1fee4-204">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="1fee4-204">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="1fee4-205">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="1fee4-205">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="1fee4-206">Do části **appSettings** definované ve vašem souboru app.config přidejte následující elementy:</span><span class="sxs-lookup"><span data-stu-id="1fee4-206">Add the following elements to **appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <span data-ttu-id="1fee4-207"><a id="example"></a>Příklad</span><span class="sxs-lookup"><span data-stu-id="1fee4-207"><a id="example"></a>Example</span></span>

<span data-ttu-id="1fee4-208">Přepište kód v souboru Program.cs kódem zobrazeným v této části.</span><span class="sxs-lookup"><span data-stu-id="1fee4-208">Overwrite the code in your Program.cs file with the code shown in this section.</span></span>
 
>[!NOTE]
><span data-ttu-id="1fee4-209">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="1fee4-209">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="1fee4-210">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="1fee4-210">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="1fee4-211">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="1fee4-211">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="1fee4-212">Nezapomeňte aktualizovat proměnné tak, aby odkazovaly do složek, ve kterých jsou umístěné vaše vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="1fee4-212">Make sure to update variables to point to folders where your input files are located.</span></span>

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
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // A Uri describing the issuer of the token.  
        // Must match the value in the token for the token to be considered valid.
        private static readonly Uri _sampleIssuer =
            new Uri(ConfigurationManager.AppSettings["Issuer"]);
        // The Audience or Scope of the token.  
        // Must match the value in the token for the token to be considered valid.
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
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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

            // Generate a test token based on the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified 
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);

            //The GenerateTestToken method returns the token without the word “Bearer” in front
            //so you have to add it in front of the token string. 
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            // You can use the bit.ly/aesplayer Flash player to test the URL 
            // (with open authorization policy). 
            // Paste the URL and click the Update button to play the video. 
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
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task with the encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
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

            // Associate the key with the asset.
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

            // Add ContentKeyAutorizationPolicy to ContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
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

            // Add ContentKeyAutorizationPolicy to ContentKey
            contentKey.AuthorizationPolicyId = policy.Id;
            IContentKey updatedKey = contentKey.UpdateAsync().Result;
            Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);

            return tokenTemplateString;
        }

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);

            string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));

            // When configuring delivery policy, you can choose to associate it
            // with a key acquisition URL that has a KID appended or
            // or a key acquisition URL that does not have a KID appended  
            // in which case a content key can be reused. 

            // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
            // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

            // The following policy configuration specifies: 
            // key url that will have KID=<Guid> appended to the envelope and
            // the Initialization Vector (IV) to use for the envelope encryption.

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

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
        }

        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset. 

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                EndsWith(".ism")).
                FirstOrDefault();

            // Create a 30-day readonly access policy. 
            // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
            IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

            // Create a locator to the streaming content on an origin. 
            ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

            // Create a URL to the manifest file. 
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


## <a name="media-services-learning-paths"></a><span data-ttu-id="1fee4-213">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="1fee4-213">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1fee4-214">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="1fee4-214">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

