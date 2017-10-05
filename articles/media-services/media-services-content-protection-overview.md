---
title: "Chránit obsah pomocí služby Azure Media Services | Microsoft Docs"
description: "Tento článek poskytuje přehled toho Ochrana obsahu pomocí služby Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 64be4ea104bd11b8e191e2c8d4170a2de88acb47
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protecting-content-overview"></a><span data-ttu-id="c0789-103">Ochrana obsahu – přehled</span><span class="sxs-lookup"><span data-stu-id="c0789-103">Protecting content overview</span></span>
<span data-ttu-id="c0789-104">Microsoft Azure Media Services umožňuje zabezpečení médií od okamžiku opuštění počítače přes uložení a zpracování až po doručení.</span><span class="sxs-lookup"><span data-stu-id="c0789-104">Microsoft Azure Media Services enables you to secure your media from the time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="c0789-105">Služba Media Services umožňuje doručovat obsah za provozu a na vyžádání dynamicky šifrován Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování) nebo některého z hlavní technologiemi DRM: Microsoft PlayReady, Google Widevine a Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="c0789-105">Media Services allows you to deliver your live and on-demand content encrypted dynamically with Advanced Encryption Standard (AES) (using 128-bit encryption keys) or any of the major DRMs: Microsoft PlayReady, Google Widevine, and Apple FairPlay.</span></span> <span data-ttu-id="c0789-106">Služba Media Services také poskytuje službu k doručování klíčů AES a DRM (PlayReady, Widevine a FairPlay) licence autorizovaným klientům.</span><span class="sxs-lookup"><span data-stu-id="c0789-106">Media Services also provides a service for delivering AES keys and DRM (PlayReady, Widevine, and FairPlay) licenses to authorized clients.</span></span> 

<span data-ttu-id="c0789-107">Následující obrázek ukazuje pracovní postupy ochrana obsahu, které podporuje AMS.</span><span class="sxs-lookup"><span data-stu-id="c0789-107">The following image demonstrates the content protection workflows that AMS supports.</span></span> 

![Ochrana technologií PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[!NOTE]
><span data-ttu-id="c0789-109">Po vytvoření účtu AMS se do vašeho účtu přidá **výchozí** koncový bod streamování ve stavu **Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="c0789-109">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="c0789-110">Pokud chcete spustit streamování vašeho obsahu a využít výhod dynamického balení a dynamického šifrování, musí koncový bod streamování, ze kterého chcete streamovat obsah, být ve stavu **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="c0789-110">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 

<span data-ttu-id="c0789-111">Toto téma vysvětluje [principy a terminologií](media-services-content-protection-overview.md) relevantní k pochopení ochrana obsahu pomocí služby AMS.</span><span class="sxs-lookup"><span data-stu-id="c0789-111">This topic explains [concepts and terminology](media-services-content-protection-overview.md) relevant to understanding content protection with AMS.</span></span> <span data-ttu-id="c0789-112">Téma obsahuje také [odkazy](media-services-content-protection-overview.md#common-scenarios) na témata, která ukazují, jak dosáhnout úlohy ochrany obsahu.</span><span class="sxs-lookup"><span data-stu-id="c0789-112">The topic also contains [links](media-services-content-protection-overview.md#common-scenarios) to topics that show how to achieve content protection tasks.</span></span> 

## <a name="dynamic-encryption"></a><span data-ttu-id="c0789-113">Dynamické šifrování</span><span class="sxs-lookup"><span data-stu-id="c0789-113">Dynamic encryption</span></span>
<span data-ttu-id="c0789-114">Microsoft Azure Media Services umožňuje doručovat obsah dynamicky šifrovaných pomocí nezašifrovaného klíče AES nebo DRM šifrování: Microsoft PlayReady, Google Widevine a Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="c0789-114">Microsoft Azure Media Services enables you to deliver your content encrypted  dynamically with AES clear key or DRM encryption: Microsoft PlayReady, Google Widevine, and Apple FairPlay.</span></span>

<span data-ttu-id="c0789-115">V současné době můžete šifrovat formátech streamování: HLS, MPEG DASH a technologie Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="c0789-115">Currently, you can encrypt the following streaming formats: HLS, MPEG DASH, and Smooth Streaming.</span></span> <span data-ttu-id="c0789-116">Nelze zašifrovat progresivní stahování.</span><span class="sxs-lookup"><span data-stu-id="c0789-116">You cannot encrypt progressive downloads.</span></span>

<span data-ttu-id="c0789-117">Pokud chcete použít pro Media Services k šifrování prostředek, budete muset přidružit asset šifrovací klíč (CommonEncryption nebo EnvelopeEncryption) a taky nakonfigurovat zásady autorizace pro klíč.</span><span class="sxs-lookup"><span data-stu-id="c0789-117">If you want for Media Services to encrypt an asset, you need to associate an encryption key (CommonEncryption or EnvelopeEncryption) with your asset and also configure authorization policies for the key.</span></span>

<span data-ttu-id="c0789-118">Také musíte nakonfigurovat zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="c0789-118">You also need to configure the asset's delivery policy.</span></span> <span data-ttu-id="c0789-119">Pokud chcete asset šifrované úložiště datového proudu, nezapomeňte určit, jak chcete poskytovaným konfigurace zásad doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="c0789-119">If you want to stream a storage encrypted asset, make sure to specify how you want to deliver it by configuring asset delivery policy.</span></span>

<span data-ttu-id="c0789-120">Datový proud je žádost přehrávač, Media Services používá k zadanému klíči dynamicky šifrovat obsah pomocí nezašifrovaného klíče AES nebo DRM šifrování.</span><span class="sxs-lookup"><span data-stu-id="c0789-120">When a stream is requested by a player, Media Services uses the specified key to dynamically encrypt your content using AES clear key or DRM encryption.</span></span> <span data-ttu-id="c0789-121">K dešifrování datového proudu, bude přehrávač požadovat klíč ze služby doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="c0789-121">To decrypt the stream, the player will request the key from the key delivery service.</span></span> <span data-ttu-id="c0789-122">Při rozhodování, zda je uživatel oprávnění k získání klíče, služba vyhodnocuje zásady autorizace, které jste zadali pro klíč.</span><span class="sxs-lookup"><span data-stu-id="c0789-122">To decide whether or not the user is authorized to get the key, the service evaluates the authorization policies that you specified for the key.</span></span>


## <a name="storage-encryption"></a><span data-ttu-id="c0789-123">Šifrování úložiště</span><span class="sxs-lookup"><span data-stu-id="c0789-123">Storage encryption</span></span>
<span data-ttu-id="c0789-124">Šifrování úložiště použijte k šifrování vašeho nešifrovaného obsahu pomocí 256bitového šifrování AES 256 a nahrajte ho do Azure Storage kde bude uložený v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="c0789-124">Use storage encryption to encrypt your clear content locally using AES 256 bit encryption and then upload it to Azure Storage where it is stored encrypted at rest.</span></span> <span data-ttu-id="c0789-125">Prostředky chráněné pomocí šifrování úložiště jsou automaticky bez šifrování umístěny do systému souborů EFS před kódování a volitelně se znovu zašifrují před jejich odesláním zpět v podobě nového výstupního prostředku.</span><span class="sxs-lookup"><span data-stu-id="c0789-125">Assets protected with storage encryption are automatically unencrypted and placed in an encrypted file system prior to encoding, and optionally re-encrypted prior to uploading back as a new output asset.</span></span> <span data-ttu-id="c0789-126">Případem primárního použití šifrování úložiště je, když chcete zabezpečit vysoké kvality souborů vstupními médii pomocí silného šifrování v klidovém stavu na disku.</span><span class="sxs-lookup"><span data-stu-id="c0789-126">The primary use case for storage encryption is when you want to secure your high quality input media files with strong encryption at rest on disk.</span></span>

<span data-ttu-id="c0789-127">Aby bylo možné poskytovat asset šifrované úložiště, musíte nakonfigurovat zásady doručení assetu, aby věděl Media Services může způsob doručení obsahu.</span><span class="sxs-lookup"><span data-stu-id="c0789-127">In order to deliver a storage encrypted asset, you must configure the asset’s delivery policy so Media Services knows how you want to deliver your content.</span></span> <span data-ttu-id="c0789-128">Před asset Streamovat, server datových proudů odebere šifrování úložiště a datové proudy svůj obsah pomocí zadaného doručování zásad (například AES, běžným šifrováním nebo žádné šifrování).</span><span class="sxs-lookup"><span data-stu-id="c0789-128">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy (for example, AES, common encryption, or no encryption).</span></span>

## <a name="common-encryption-cenc"></a><span data-ttu-id="c0789-129">Běžné šifrování (CENC)</span><span class="sxs-lookup"><span data-stu-id="c0789-129">Common encryption (CENC)</span></span>
<span data-ttu-id="c0789-130">Běžné šifrování se používá při šifrování svůj obsah pomocí PlayReady nebo / a Widewine.</span><span class="sxs-lookup"><span data-stu-id="c0789-130">Common encryption is used when encrypting your content with PlayReady or/and Widewine.</span></span>

## <a name="using-cbcs-aapl-encryption"></a><span data-ttu-id="c0789-131">Pomocí šifrování na cbcs-aapl</span><span class="sxs-lookup"><span data-stu-id="c0789-131">Using cbcs-aapl encryption</span></span>
<span data-ttu-id="c0789-132">Cbcs-aapl se používá při šifrování svůj obsah pomocí FairPlay.</span><span class="sxs-lookup"><span data-stu-id="c0789-132">Cbcs-aapl is used when encrypting your content with FairPlay.</span></span>

## <a name="envelope-encryption"></a><span data-ttu-id="c0789-133">Šifrování obálky</span><span class="sxs-lookup"><span data-stu-id="c0789-133">Envelope encryption</span></span>
<span data-ttu-id="c0789-134">Tuto možnost použijte, pokud chcete chránit obsah pomocí nezašifrovaný klíč AES-128.</span><span class="sxs-lookup"><span data-stu-id="c0789-134">Use this option if you want to protect your content with AES-128 clear key.</span></span> <span data-ttu-id="c0789-135">Pokud chcete bezpečnější možnost, vyberte jednu z technologiemi DRM uvedené v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="c0789-135">If you want a more secure option, choose one of the DRMs listed in this topic.</span></span> 

## <a name="licenses-and-keys-delivery-service"></a><span data-ttu-id="c0789-136">Službu doručování licencí a klíče</span><span class="sxs-lookup"><span data-stu-id="c0789-136">Licenses and keys delivery service</span></span>
<span data-ttu-id="c0789-137">Služba Media Services poskytuje službu k doručování licencí DRM (technologie PlayReady, Widevine, FairPlay) a AES zrušte klíče autorizovaným klientům.</span><span class="sxs-lookup"><span data-stu-id="c0789-137">Media Services provides a service for delivering DRM (PlayReady, Widevine, FairPlay) licenses and AES clear keys to authorized clients.</span></span> <span data-ttu-id="c0789-138">Můžete použít [portálu Azure](media-services-portal-protect-content.md), REST API nebo sady Media Services SDK pro .NET, můžete nakonfigurovat zásady ověřování a ověřování licencí a klíčů.</span><span class="sxs-lookup"><span data-stu-id="c0789-138">You can use [the Azure portal](media-services-portal-protect-content.md), REST API, or Media Services SDK for .NET to configure authorization and authentication policies for your licenses and keys.</span></span>

## <a name="token-restriction"></a><span data-ttu-id="c0789-139">Omezení s tokenem</span><span class="sxs-lookup"><span data-stu-id="c0789-139">Token restriction</span></span>
<span data-ttu-id="c0789-140">Zásady autorizace pro klíč k obsahu mohou obsahovat jedno nebo více omezení autorizace: otevřené omezení nebo omezení s tokenem.</span><span class="sxs-lookup"><span data-stu-id="c0789-140">The content key authorization policy could have one or more authorization restrictions: open or token restriction.</span></span> <span data-ttu-id="c0789-141">Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS).</span><span class="sxs-lookup"><span data-stu-id="c0789-141">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="c0789-142">Služba Media Services podporuje tokeny ve formátu jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT).</span><span class="sxs-lookup"><span data-stu-id="c0789-142">Media Services supports tokens in the Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span> <span data-ttu-id="c0789-143">Služba Media Services neposkytuje zabezpečení tokenu služby.</span><span class="sxs-lookup"><span data-stu-id="c0789-143">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="c0789-144">Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS problém tokeny.</span><span class="sxs-lookup"><span data-stu-id="c0789-144">You can create a custom STS or leverage Microsoft Azure ACS to issue tokens.</span></span> <span data-ttu-id="c0789-145">Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem.</span><span class="sxs-lookup"><span data-stu-id="c0789-145">The STS must be configured to create a token signed with the specified key and issue claims that you specified in the token restriction configuration.</span></span> <span data-ttu-id="c0789-146">Služba Media Services doručení klíče vrátí požadovaný klíč (nebo licencí) do klienta, pokud je token platný a deklarace identity v tokenu shody ty nakonfigurované pro klíč (nebo licencí).</span><span class="sxs-lookup"><span data-stu-id="c0789-146">The Media Services key delivery service will return the requested key (or license) to the client if the token is valid and the claims in the token match those configured for the key (or license).</span></span>

<span data-ttu-id="c0789-147">Při konfiguraci token omezený zásad, musíte zadat klíč primární ověřování, vystavitele a cílová skupina parametry.</span><span class="sxs-lookup"><span data-stu-id="c0789-147">When configuring the token restricted policy, you must specify the primary verification key, issuer and audience parameters.</span></span> <span data-ttu-id="c0789-148">Ověření primární klíč obsahuje klíč, který byl podepsaný token, Vystavitel je zabezpečený tokenu služba, která vydá token.</span><span class="sxs-lookup"><span data-stu-id="c0789-148">The primary verification key contains the key that the token was signed with, issuer is the secure token service that issues the token.</span></span> <span data-ttu-id="c0789-149">Cílová skupina (někdy nazývané oboru) popisuje záměr tokenu nebo prostředek token povolí přístup k.</span><span class="sxs-lookup"><span data-stu-id="c0789-149">The audience (sometimes called scope) describes the intent of the token or the resource the token authorizes access to.</span></span> <span data-ttu-id="c0789-150">Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.</span><span class="sxs-lookup"><span data-stu-id="c0789-150">The Media Services key delivery service validates that these values in the token match the values in the template.</span></span>

## <a name="streaming-urls"></a><span data-ttu-id="c0789-151">Adresy URL streamování</span><span class="sxs-lookup"><span data-stu-id="c0789-151">Streaming URLs</span></span>
<span data-ttu-id="c0789-152">Pokud váš asset byla zašifrována pomocí více než jeden DRM, ve adresu URL streamování byste měli používat značky šifrování: (formát = 'm3u8-aapl' šifrování = 'xxx').</span><span class="sxs-lookup"><span data-stu-id="c0789-152">If your asset was encrypted with more than one DRM, you should use an encryption tag in the streaming URL: (format='m3u8-aapl', encryption='xxx').</span></span>

<span data-ttu-id="c0789-153">Platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="c0789-153">The following considerations apply:</span></span>

* <span data-ttu-id="c0789-154">Lze zadat pouze nula nebo jeden typ šifrování.</span><span class="sxs-lookup"><span data-stu-id="c0789-154">Only zero or one encryption type can be specified.</span></span>
* <span data-ttu-id="c0789-155">Typ šifrování nemusí být zadaného v adrese url, pokud jenom jeden šifrování byla použita pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="c0789-155">Encryption type doesn't have to be specified in the url if only one encryption was applied to the asset.</span></span>
* <span data-ttu-id="c0789-156">Typ šifrování je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="c0789-156">Encryption type is case insensitive.</span></span>
* <span data-ttu-id="c0789-157">Určit lze následující typy šifrování:</span><span class="sxs-lookup"><span data-stu-id="c0789-157">The following encryption types can be specified:</span></span>  
  * <span data-ttu-id="c0789-158">**šifrování cenc**: Common encryption (Playready nebo Widevine)</span><span class="sxs-lookup"><span data-stu-id="c0789-158">**cenc**:  Common encryption (Playready or Widevine)</span></span>
  * <span data-ttu-id="c0789-159">**cbcs-aapl**: Fairplay</span><span class="sxs-lookup"><span data-stu-id="c0789-159">**cbcs-aapl**: Fairplay</span></span>
  * <span data-ttu-id="c0789-160">**CBC**: obálky šifrování AES.</span><span class="sxs-lookup"><span data-stu-id="c0789-160">**cbc**: AES envelope encryption.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="c0789-161">Obvyklé scénáře</span><span class="sxs-lookup"><span data-stu-id="c0789-161">Common scenarios</span></span>
<span data-ttu-id="c0789-162">Následující témata ukazují, jak chránit obsah v úložišti, doručování dynamicky šifrovaných streamovaných médií, použijte doručení klíč nebo licenční služby AMS</span><span class="sxs-lookup"><span data-stu-id="c0789-162">The following topics demonstrate how to protect content in storage, deliver dynamically encrypted streaming media, use AMS key/license delivery service</span></span>

* [<span data-ttu-id="c0789-163">Ochrana pomocí standardu AES</span><span class="sxs-lookup"><span data-stu-id="c0789-163">Protect with AES</span></span>](media-services-protect-with-aes128.md) 
* [<span data-ttu-id="c0789-164">Chránit pomocí PlayReady nebo Widevine</span><span class="sxs-lookup"><span data-stu-id="c0789-164">Protect with PlayReady and/or Widevine </span></span>](media-services-protect-with-drm.md)
* [<span data-ttu-id="c0789-165">Stream obsahu chráněného vaše HLS Apple FairPlay a/nebo PlayReady</span><span class="sxs-lookup"><span data-stu-id="c0789-165">Stream your HLS content Protected with Apple FairPlay and/or PlayReady</span></span>](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a><span data-ttu-id="c0789-166">Další scénáře</span><span class="sxs-lookup"><span data-stu-id="c0789-166">Additional scenarios</span></span>
* <span data-ttu-id="c0789-167">[Postup pro integraci služby Azure licence PlayReady modulu pro šifrování nebo streamování serveru](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).</span><span class="sxs-lookup"><span data-stu-id="c0789-167">[How to integrate Azure PlayReady License service with your own encryptor/streaming server](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).</span></span>
* [<span data-ttu-id="c0789-168">Použití castLabs k doručování licencí DRM do služby Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="c0789-168">Using castLabs to deliver DRM licenses to Azure Media Services</span></span>](media-services-castlabs-integration.md)

>[!NOTE]
><span data-ttu-id="c0789-169">Scénář, ve kterém můžete používat externí DRM server(technology) a datový proud z AMS není aktuálně podporováno.</span><span class="sxs-lookup"><span data-stu-id="c0789-169">A scenario in which you use an external DRM server(technology) and stream from AMS is currently not supported.</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="c0789-170">Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="c0789-170">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c0789-171">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="c0789-171">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="c0789-172">Související odkazy</span><span class="sxs-lookup"><span data-stu-id="c0789-172">Related Links</span></span>
[<span data-ttu-id="c0789-173">Uvedení PlayReady jako dynamické šifrování AES pomocí Azure Media Services a služby</span><span class="sxs-lookup"><span data-stu-id="c0789-173">Announcing PlayReady as a service and AES dynamic encryption with Azure Media Services</span></span>](http://mingfeiy.com/playready)

[<span data-ttu-id="c0789-174">Vysvětlení Azure ceny doručování licencí Media Services PlayReady</span><span class="sxs-lookup"><span data-stu-id="c0789-174">Azure Media Services PlayReady license delivery pricing explained</span></span>](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[<span data-ttu-id="c0789-175">Postup ladění pro šifrované datový proud AES ve službě Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="c0789-175">How to debug for AES encrypted stream in Azure Media Services</span></span>](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[<span data-ttu-id="c0789-176">Authenitcation tokenu JWT</span><span class="sxs-lookup"><span data-stu-id="c0789-176">JWT token authenitcation</span></span>](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

<span data-ttu-id="c0789-177">[Integrace aplikace Azure Media Services OWIN MVC se službou Azure Active Directory a omezit klíče doručování obsahu na základě deklarací JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span><span class="sxs-lookup"><span data-stu-id="c0789-177">[Integrate Azure Media Services OWIN MVC based app with Azure Active Directory and restrict content key delivery based on JWT claims](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).</span></span>

<span data-ttu-id="c0789-178">[Pomocí Azure ACS problém tokeny](http://mingfeiy.com/acs-with-key-services).</span><span class="sxs-lookup"><span data-stu-id="c0789-178">[Use Azure ACS to issue tokens](http://mingfeiy.com/acs-with-key-services).</span></span>

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png