---
title: "Chránit obsah pomocí Microsoft PlayReady nebo Apple FairPlay - Azure HLS | Microsoft Docs"
description: "Toto téma poskytuje přehled a ukazuje, jak pomocí Azure Media Services dynamicky šifrovat obsah HTTP Live Streaming (HLS) s Apple FairPlay. Také ukazuje, jak používat službu doručování licencí Media Services k doručování licence FairPlay klientům."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 895d6307b1cef74e195cc2ffd8dbef4196e97b1f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="protect-your-hls-content-with-apple-fairplay-or-microsoft-playready"></a><span data-ttu-id="653dc-104">Chránit váš obsah s Apple FairPlay nebo Microsoft PlayReady HLS</span><span class="sxs-lookup"><span data-stu-id="653dc-104">Protect your HLS content with Apple FairPlay or Microsoft PlayReady</span></span>
<span data-ttu-id="653dc-105">Azure Media Services umožňuje dynamicky šifrovat obsah HTTP Live Streaming (HLS) pomocí následujících formátů:</span><span class="sxs-lookup"><span data-stu-id="653dc-105">Azure Media Services enables you to dynamically encrypt your HTTP Live Streaming (HLS) content by using the following formats:</span></span>  

* <span data-ttu-id="653dc-106">**Nezašifrovaný klíč AES-128 obálky**</span><span class="sxs-lookup"><span data-stu-id="653dc-106">**AES-128 envelope clear key**</span></span>

    <span data-ttu-id="653dc-107">Celý blok se šifrují pomocí **AES-128 CBC** režimu.</span><span class="sxs-lookup"><span data-stu-id="653dc-107">The entire chunk is encrypted by using the **AES-128 CBC** mode.</span></span> <span data-ttu-id="653dc-108">IOS a OS X player je nativně podporované dešifrování datového proudu.</span><span class="sxs-lookup"><span data-stu-id="653dc-108">The decryption of the stream is supported by iOS and OS X player natively.</span></span> <span data-ttu-id="653dc-109">Další informace najdete v tématu [dynamického šifrování pomocí standardu AES-128 a doručení klíče služby](media-services-protect-with-aes128.md).</span><span class="sxs-lookup"><span data-stu-id="653dc-109">For more information, see [Using AES-128 dynamic encryption and key delivery service](media-services-protect-with-aes128.md).</span></span>
* <span data-ttu-id="653dc-110">**Apple FairPlay**</span><span class="sxs-lookup"><span data-stu-id="653dc-110">**Apple FairPlay**</span></span>

    <span data-ttu-id="653dc-111">Jednotlivé vzorky videa a zvuku jsou šifrována pomocí **AES-128 CBC** režimu.</span><span class="sxs-lookup"><span data-stu-id="653dc-111">The individual video and audio samples are encrypted by using the **AES-128 CBC** mode.</span></span> <span data-ttu-id="653dc-112">**Streamování FairPlay** (FPS) je integrována do operačních systémů zařízení, s nativní podporou na iOS a Apple TV.</span><span class="sxs-lookup"><span data-stu-id="653dc-112">**FairPlay Streaming** (FPS) is integrated into the device operating systems, with native support on iOS and Apple TV.</span></span> <span data-ttu-id="653dc-113">Prohlížeč Safari na OS X umožňuje FPS s využitím podpory rozhraní šifrované rozšíření média (EME).</span><span class="sxs-lookup"><span data-stu-id="653dc-113">Safari on OS X enables FPS by using the Encrypted Media Extensions (EME) interface support.</span></span>
* <span data-ttu-id="653dc-114">**Microsoft PlayReady**</span><span class="sxs-lookup"><span data-stu-id="653dc-114">**Microsoft PlayReady**</span></span>

<span data-ttu-id="653dc-115">Na následujícím obrázku **HLS + FairPlay nebo PlayReady dynamického šifrování** pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="653dc-115">The following image shows the **HLS + FairPlay or PlayReady dynamic encryption** workflow.</span></span>

![Diagram pracovního postupu dynamického šifrování](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

<span data-ttu-id="653dc-117">Toto téma ukazuje, jak pomocí služby Media Services dynamicky šifrovat obsah HLS s Apple FairPlay.</span><span class="sxs-lookup"><span data-stu-id="653dc-117">This topic demonstrates how to use Media Services to dynamically encrypt your HLS content with Apple FairPlay.</span></span> <span data-ttu-id="653dc-118">Také ukazuje, jak používat službu doručování licencí Media Services k doručování licence FairPlay klientům.</span><span class="sxs-lookup"><span data-stu-id="653dc-118">It also shows how to use the Media Services license delivery service to deliver FairPlay licenses to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="653dc-119">Pokud chcete zašifrovat obsah HLS pomocí technologie PlayReady, budete muset vytvořit běžné klíč obsahu a přidružte ji k asset.</span><span class="sxs-lookup"><span data-stu-id="653dc-119">If you also want to encrypt your HLS content with PlayReady, you need to create a common content key and associate it with your asset.</span></span> <span data-ttu-id="653dc-120">Budete také muset nakonfigurovat zásady autorizace pro klíč k obsahu, jak je popsáno v [běžného dynamického šifrování PlayReady pomocí](media-services-protect-with-drm.md).</span><span class="sxs-lookup"><span data-stu-id="653dc-120">You also need to configure the content key’s authorization policy, as described in [Using PlayReady dynamic common encryption](media-services-protect-with-drm.md).</span></span>
>
>

## <a name="requirements-and-considerations"></a><span data-ttu-id="653dc-121">Požadavky a důležité informace</span><span class="sxs-lookup"><span data-stu-id="653dc-121">Requirements and considerations</span></span>

<span data-ttu-id="653dc-122">Toto jsou požadovány při použití služby Media Services k poskytování HLS se šifrováním pomocí FairPlay a k poskytování licence FairPlay:</span><span class="sxs-lookup"><span data-stu-id="653dc-122">The following are required when using Media Services to deliver HLS encrypted with FairPlay, and to deliver FairPlay licenses:</span></span>

  * <span data-ttu-id="653dc-123">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="653dc-123">An Azure account.</span></span> <span data-ttu-id="653dc-124">Podrobnosti najdete na stránce [bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="653dc-124">For details, see [Azure free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
  * <span data-ttu-id="653dc-125">Účet Media Services.</span><span class="sxs-lookup"><span data-stu-id="653dc-125">A Media Services account.</span></span> <span data-ttu-id="653dc-126">Chcete-li vytvořit, přečtěte si téma [vytvoření účtu Azure Media Services pomocí webu Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="653dc-126">To create one, see [Create an Azure Media Services account using the Azure portal](media-services-portal-create-account.md).</span></span>
  * <span data-ttu-id="653dc-127">Zaregistrovat [vývoj programem](https://developer.apple.com/).</span><span class="sxs-lookup"><span data-stu-id="653dc-127">Sign up with [Apple Development Program](https://developer.apple.com/).</span></span>
  * <span data-ttu-id="653dc-128">Apple vyžaduje vlastníka obsahu k získání [balíček pro nasazení](https://developer.apple.com/contact/fps/).</span><span class="sxs-lookup"><span data-stu-id="653dc-128">Apple requires the content owner to obtain the [deployment package](https://developer.apple.com/contact/fps/).</span></span> <span data-ttu-id="653dc-129">Stav už implementované klíč zabezpečení modulu (KSM) pomocí služby Media Services a že žádáte poslední balíček FPS.</span><span class="sxs-lookup"><span data-stu-id="653dc-129">State that you already implemented Key Security Module (KSM) with Media Services, and that you are requesting the final FPS package.</span></span> <span data-ttu-id="653dc-130">Pokyny v posledním FPS balíčku pro generování certifikační a získat klíč tajný klíč aplikace (požádejte) jsou.</span><span class="sxs-lookup"><span data-stu-id="653dc-130">There are instructions in the final FPS package to generate certification and obtain the Application Secret Key (ASK).</span></span> <span data-ttu-id="653dc-131">Požádejte použijete ke konfiguraci FairPlay.</span><span class="sxs-lookup"><span data-stu-id="653dc-131">You use ASK to configure FairPlay.</span></span>
  * <span data-ttu-id="653dc-132">Azure Media Services .NET SDK verze **3.6.0** nebo novější.</span><span class="sxs-lookup"><span data-stu-id="653dc-132">Azure Media Services .NET SDK version **3.6.0** or later.</span></span>

<span data-ttu-id="653dc-133">Na straně doručení klíče služby Media Services musí být nastavena následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="653dc-133">The following things must be set on Media Services key delivery side:</span></span>

  * <span data-ttu-id="653dc-134">**Aplikace Cert (AC)**: Toto je soubor .pfx, který obsahuje soukromý klíč.</span><span class="sxs-lookup"><span data-stu-id="653dc-134">**App Cert (AC)**: This is a .pfx file that contains the private key.</span></span> <span data-ttu-id="653dc-135">Tento soubor vytvořte a šifrování pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="653dc-135">You create this file and encrypt it with a password.</span></span>

       <span data-ttu-id="653dc-136">Když konfigurujete zásady doručení klíče, je nutné zadat toto heslo a soubor .pfx, ve formátu Base64.</span><span class="sxs-lookup"><span data-stu-id="653dc-136">When you configure a key delivery policy, you must provide that password and the .pfx file in Base64 format.</span></span>

      <span data-ttu-id="653dc-137">Následující kroky popisují, jak generovat soubor .pfx certifikátu pro FairPlay:</span><span class="sxs-lookup"><span data-stu-id="653dc-137">The following steps describe how to generate a .pfx certificate file for FairPlay:</span></span>

    1. <span data-ttu-id="653dc-138">Nainstalujte OpenSSL z https://slproweb.com/products/Win32OpenSSL.html.</span><span class="sxs-lookup"><span data-stu-id="653dc-138">Install OpenSSL from https://slproweb.com/products/Win32OpenSSL.html.</span></span>

        <span data-ttu-id="653dc-139">Přejděte do složky, kde jsou FairPlay certifikátu a další soubory dodané společností Apple.</span><span class="sxs-lookup"><span data-stu-id="653dc-139">Go to the folder where the FairPlay certificate and other files delivered by Apple are.</span></span>
    2. <span data-ttu-id="653dc-140">V příkazovém řádku spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="653dc-140">Run the following command from the command line.</span></span> <span data-ttu-id="653dc-141">Tento soubor .cer převede na soubor .pem.</span><span class="sxs-lookup"><span data-stu-id="653dc-141">This converts the .cer file to a .pem file.</span></span>

        <span data-ttu-id="653dc-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509-informovat der – v fairplay.cer-out fairplay out.pem</span><span class="sxs-lookup"><span data-stu-id="653dc-142">"C:\OpenSSL-Win32\bin\openssl.exe" x509 -inform der -in fairplay.cer -out fairplay-out.pem</span></span>
    3. <span data-ttu-id="653dc-143">V příkazovém řádku spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="653dc-143">Run the following command from the command line.</span></span> <span data-ttu-id="653dc-144">Tento soubor .pem převede do souboru .pfx s privátním klíčem.</span><span class="sxs-lookup"><span data-stu-id="653dc-144">This converts the .pem file to a .pfx file with the private key.</span></span> <span data-ttu-id="653dc-145">Heslo pro soubor .pfx pak požaduje od OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="653dc-145">The password for the .pfx file is then asked by OpenSSL.</span></span>

        <span data-ttu-id="653dc-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-export - out fairplay out.pfx-inkey privatekey.pem-v - passin file:privatekey-pem-pass.txt fairplay out.pem</span><span class="sxs-lookup"><span data-stu-id="653dc-146">"C:\OpenSSL-Win32\bin\openssl.exe" pkcs12 -export -out fairplay-out.pfx -inkey privatekey.pem -in fairplay-out.pem -passin file:privatekey-pem-pass.txt</span></span>
  * <span data-ttu-id="653dc-147">**Heslo aplikace Cert**: heslo pro vytvoření souboru .pfx.</span><span class="sxs-lookup"><span data-stu-id="653dc-147">**App Cert password**: The password for creating the .pfx file.</span></span>
  * <span data-ttu-id="653dc-148">**ID aplikace Cert heslo**: je potřeba načíst heslo, podobně jako jak odesílají jiných klíčů Media Services.</span><span class="sxs-lookup"><span data-stu-id="653dc-148">**App Cert password ID**: You must upload the password, similar to how they upload other Media Services keys.</span></span> <span data-ttu-id="653dc-149">Použití **ContentKeyType.FairPlayPfxPassword** hodnota výčtu získat ID služby média.</span><span class="sxs-lookup"><span data-stu-id="653dc-149">Use the **ContentKeyType.FairPlayPfxPassword** enum value to get the Media Services ID.</span></span> <span data-ttu-id="653dc-150">Toto je požadované použít uvnitř možnost zásady doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="653dc-150">This is what they need to use inside the key delivery policy option.</span></span>
  * <span data-ttu-id="653dc-151">**IV**: je to hodnota náhodných 16 bajtů.</span><span class="sxs-lookup"><span data-stu-id="653dc-151">**iv**: This is a random value of 16 bytes.</span></span> <span data-ttu-id="653dc-152">Musí se shodovat iv do zásady doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="653dc-152">It must match the iv in the asset delivery policy.</span></span> <span data-ttu-id="653dc-153">Generovat iv a umístí jej na obou místech: zásady doručení assetu a možnost zásady doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="653dc-153">You generate the iv, and put it in both places: the asset delivery policy and the key delivery policy option.</span></span>
  * <span data-ttu-id="653dc-154">**Požádejte**: Tento klíč je oznámených při generování certifikační pomocí portálu pro vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="653dc-154">**ASK**: This key is received when you generate the certification by using the Apple Developer portal.</span></span> <span data-ttu-id="653dc-155">Každý vývojový tým obdrží jedinečný požádejte.</span><span class="sxs-lookup"><span data-stu-id="653dc-155">Each development team will receive a unique ASK.</span></span> <span data-ttu-id="653dc-156">Uložte kopii požádejte a uložit na bezpečném místě.</span><span class="sxs-lookup"><span data-stu-id="653dc-156">Save a copy of the ASK, and store it in a safe place.</span></span> <span data-ttu-id="653dc-157">Musíte nakonfigurovat požádejte jako FairPlayAsk ke službě Media Services později.</span><span class="sxs-lookup"><span data-stu-id="653dc-157">You will need to configure ASK as FairPlayAsk to Media Services later.</span></span>
  * <span data-ttu-id="653dc-158">**Požádejte ID**: Toto ID je získali při nahrávání požádejte ve službě Media Services.</span><span class="sxs-lookup"><span data-stu-id="653dc-158">**ASK ID**: This ID is obtained when you upload ASK into Media Services.</span></span> <span data-ttu-id="653dc-159">Požádejte musíte nahrát pomocí **ContentKeyType.FairPlayAsk** hodnota výčtu.</span><span class="sxs-lookup"><span data-stu-id="653dc-159">You must upload ASK by using the **ContentKeyType.FairPlayAsk** enum value.</span></span> <span data-ttu-id="653dc-160">V důsledku toho se vrátí Media Services ID a je to, co by měl být použit při nastavení možnosti zásad doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="653dc-160">As the result, the Media Services ID is returned, and this is what should be used when setting the key delivery policy option.</span></span>

<span data-ttu-id="653dc-161">Na straně klienta FPS musí nastavit následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="653dc-161">The following things must be set by the FPS client side:</span></span>

  * <span data-ttu-id="653dc-162">**Aplikace Cert (AC)**: Toto je.cer/.der soubor, který obsahuje veřejný klíč, který operační systém používá k šifrování některé datové části.</span><span class="sxs-lookup"><span data-stu-id="653dc-162">**App Cert (AC)**: This is a .cer/.der file that contains the public key, which the operating system uses to encrypt some payload.</span></span> <span data-ttu-id="653dc-163">Služba Media Services je potřeba upozornit, protože je požadována přehrávače.</span><span class="sxs-lookup"><span data-stu-id="653dc-163">Media Services needs to know about it because it is required by the player.</span></span> <span data-ttu-id="653dc-164">Službu doručení klíče dešifruje pomocí příslušného privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="653dc-164">The key delivery service decrypts it using the corresponding private key.</span></span>

<span data-ttu-id="653dc-165">Přehrát FairPlay šifrovaného datového proudu, převeďte skutečné požádejte první a pak vygenerovat certifikát skutečné.</span><span class="sxs-lookup"><span data-stu-id="653dc-165">To play back a FairPlay encrypted stream, get a real ASK first, and then generate a real certificate.</span></span> <span data-ttu-id="653dc-166">Tento proces vytvoří všech tří částí:</span><span class="sxs-lookup"><span data-stu-id="653dc-166">That process creates all three parts:</span></span>

  * <span data-ttu-id="653dc-167">souboru .der</span><span class="sxs-lookup"><span data-stu-id="653dc-167">.der file</span></span>
  * <span data-ttu-id="653dc-168">soubor .pfx</span><span class="sxs-lookup"><span data-stu-id="653dc-168">.pfx file</span></span>
  * <span data-ttu-id="653dc-169">heslo .pfx</span><span class="sxs-lookup"><span data-stu-id="653dc-169">password for the .pfx</span></span>

<span data-ttu-id="653dc-170">Následující klienti podporují HLS s **AES-128 CBC** šifrování: Safari v systému OS X, Apple TV, iOS.</span><span class="sxs-lookup"><span data-stu-id="653dc-170">The following clients support HLS with **AES-128 CBC** encryption: Safari on OS X, Apple TV, iOS.</span></span>

## <a name="configure-fairplay-dynamic-encryption-and-license-delivery-services"></a><span data-ttu-id="653dc-171">Konfigurace FairPlay dynamické šifrování a licencí služeb doručování</span><span class="sxs-lookup"><span data-stu-id="653dc-171">Configure FairPlay dynamic encryption and license delivery services</span></span>
<span data-ttu-id="653dc-172">Následují obecné kroky pro své assety chráníte pomocí FairPlay pomocí službu doručování licencí Media Services a také používáte dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="653dc-172">The following are general steps for protecting your assets with FairPlay by using the Media Services license delivery service, and also by using dynamic encryption.</span></span>

1. <span data-ttu-id="653dc-173">Vytvoření assetu a nahrání souborů do assetu.</span><span class="sxs-lookup"><span data-stu-id="653dc-173">Create an asset, and upload files into the asset.</span></span>
2. <span data-ttu-id="653dc-174">Zakódujte asset, který obsahuje soubor s adaptivní přenosovou rychlostí sady souborů MP4.</span><span class="sxs-lookup"><span data-stu-id="653dc-174">Encode the asset that contains the file to the adaptive bitrate MP4 set.</span></span>
3. <span data-ttu-id="653dc-175">Vytvořte klíč obsahu a přidružte ji k zakódovanému assetu.</span><span class="sxs-lookup"><span data-stu-id="653dc-175">Create a content key, and associate it with the encoded asset.</span></span>  
4. <span data-ttu-id="653dc-176">Nakonfigurujte zásady autorizace pro klíč k obsahu.</span><span class="sxs-lookup"><span data-stu-id="653dc-176">Configure the content key’s authorization policy.</span></span> <span data-ttu-id="653dc-177">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="653dc-177">Specify the following:</span></span>

   * <span data-ttu-id="653dc-178">Metodu doručení (v tomto případě FairPlay).</span><span class="sxs-lookup"><span data-stu-id="653dc-178">The delivery method (in this case, FairPlay).</span></span>
   * <span data-ttu-id="653dc-179">Konfigurace možností FairPlay zásad.</span><span class="sxs-lookup"><span data-stu-id="653dc-179">FairPlay policy options configuration.</span></span> <span data-ttu-id="653dc-180">Podrobnosti o tom, jak nakonfigurovat FairPlay najdete v tématu **ConfigureFairPlayPolicyOptions()** metoda v následující ukázce.</span><span class="sxs-lookup"><span data-stu-id="653dc-180">For details on how to configure FairPlay, see the **ConfigureFairPlayPolicyOptions()** method in the sample below.</span></span>

     > [!NOTE]
     > <span data-ttu-id="653dc-181">Obvykle byste chcete nakonfigurovat možnosti zásad FairPlay jenom jednou, protože budou mít pouze jednu sadu certifikace a ASK.</span><span class="sxs-lookup"><span data-stu-id="653dc-181">Usually, you would want to configure FairPlay policy options only once, because you will only have one set of a certification and an ASK.</span></span>
     >
     >
   * <span data-ttu-id="653dc-182">Omezení (otevřené nebo s tokenem).</span><span class="sxs-lookup"><span data-stu-id="653dc-182">Restrictions (open or token).</span></span>
   * <span data-ttu-id="653dc-183">Informace specifické pro daný typ doručení klíče, který definuje, jak je klíč doručen do klienta.</span><span class="sxs-lookup"><span data-stu-id="653dc-183">Information specific to the key delivery type that defines how the key is delivered to the client.</span></span>
5. <span data-ttu-id="653dc-184">Konfigurace zásad doručení assetu.</span><span class="sxs-lookup"><span data-stu-id="653dc-184">Configure the asset delivery policy.</span></span> <span data-ttu-id="653dc-185">Konfigurace zásad doručení zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="653dc-185">The delivery policy configuration includes:</span></span>

   * <span data-ttu-id="653dc-186">Doručovací protokol (HLS).</span><span class="sxs-lookup"><span data-stu-id="653dc-186">The delivery protocol (HLS).</span></span>
   * <span data-ttu-id="653dc-187">Typ dynamického šifrování (běžné CBC šifrování).</span><span class="sxs-lookup"><span data-stu-id="653dc-187">The type of dynamic encryption (common CBC encryption).</span></span>
   * <span data-ttu-id="653dc-188">Získávání adresu URL licence.</span><span class="sxs-lookup"><span data-stu-id="653dc-188">The license acquisition URL.</span></span>

     > [!NOTE]
     > <span data-ttu-id="653dc-189">Pokud chcete poskytovat datový proud, který je šifrován FairPlay a jinému systému pro správu digitálních práv (DRM), budete muset nakonfigurovat zásady samostatné doručení:</span><span class="sxs-lookup"><span data-stu-id="653dc-189">If you want to deliver a stream that is encrypted with FairPlay and another Digital Rights Management (DRM) system, you have to configure separate delivery policies:</span></span>
     >
     > * <span data-ttu-id="653dc-190">Jeden IAssetDeliveryPolicy konfigurace dynamické adaptivní datové proudy přes protokol HTTP (pomlčka) s běžné šifrování (CENC) (PlayReady a Widevine) a protokol Smooth s technologií PlayReady</span><span class="sxs-lookup"><span data-stu-id="653dc-190">One IAssetDeliveryPolicy to configure Dynamic Adaptive Streaming over HTTP (DASH) with Common Encryption (CENC) (PlayReady + Widevine), and Smooth with PlayReady</span></span>
     > * <span data-ttu-id="653dc-191">Jiné IAssetDeliveryPolicy konfigurace FairPlay pro HLS</span><span class="sxs-lookup"><span data-stu-id="653dc-191">Another IAssetDeliveryPolicy to configure FairPlay for HLS</span></span>
     >
     >
6. <span data-ttu-id="653dc-192">Vytvořte Lokátor OnDemand získat adresu URL pro streamování.</span><span class="sxs-lookup"><span data-stu-id="653dc-192">Create an OnDemand locator to get a streaming URL.</span></span>

## <a name="use-fairplay-key-delivery-by-player-apps"></a><span data-ttu-id="653dc-193">Použít doručení klíče FairPlay player aplikace</span><span class="sxs-lookup"><span data-stu-id="653dc-193">Use FairPlay key delivery by player apps</span></span>
<span data-ttu-id="653dc-194">Přehrávač aplikace můžete vyvíjet pomocí iOS SDK.</span><span class="sxs-lookup"><span data-stu-id="653dc-194">You can develop player apps by using the iOS SDK.</span></span> <span data-ttu-id="653dc-195">Abyste mohli k přehrávání obsahu FairPlay, budete muset implementovat protokol exchange licence.</span><span class="sxs-lookup"><span data-stu-id="653dc-195">To be able to play FairPlay content, you have to implement the license exchange protocol.</span></span> <span data-ttu-id="653dc-196">Tato položka protocol není zadán společností Apple.</span><span class="sxs-lookup"><span data-stu-id="653dc-196">This protocol is not specified by Apple.</span></span> <span data-ttu-id="653dc-197">Je na každou aplikaci, jak odesílat žádosti o doručení klíče.</span><span class="sxs-lookup"><span data-stu-id="653dc-197">It is up to each app how to send key delivery requests.</span></span> <span data-ttu-id="653dc-198">Služba Media Services FairPlay doručení klíče očekává SPC pocházet jako zprávu kódovaného post www-form-url v následující podobě:</span><span class="sxs-lookup"><span data-stu-id="653dc-198">The Media Services FairPlay key delivery service expects the SPC to come as a www-form-url encoded post message, in the following form:</span></span>

    spc=<Base64 encoded SPC>

> [!NOTE]
> <span data-ttu-id="653dc-199">Azure Media Player nepodporuje přehrávání FairPlay mimo pole.</span><span class="sxs-lookup"><span data-stu-id="653dc-199">Azure Media Player doesn’t support FairPlay playback out of the box.</span></span> <span data-ttu-id="653dc-200">Pokud chcete získat FairPlay přehrávání na MAC OS X, získejte player ukázkové z účtu vývojáře Apple.</span><span class="sxs-lookup"><span data-stu-id="653dc-200">To get FairPlay playback on MAC OS X, obtain the sample player from the Apple developer account.</span></span>
>
>

## <a name="streaming-urls"></a><span data-ttu-id="653dc-201">Adresy URL streamování</span><span class="sxs-lookup"><span data-stu-id="653dc-201">Streaming URLs</span></span>
<span data-ttu-id="653dc-202">Pokud váš asset byla zašifrována pomocí více než jeden DRM, ve adresu URL streamování byste měli používat značky šifrování: (formát = 'm3u8-aapl' šifrování = 'xxx').</span><span class="sxs-lookup"><span data-stu-id="653dc-202">If your asset was encrypted with more than one DRM, you should use an encryption tag in the streaming URL: (format='m3u8-aapl', encryption='xxx').</span></span>

<span data-ttu-id="653dc-203">Platí následující aspekty:</span><span class="sxs-lookup"><span data-stu-id="653dc-203">The following considerations apply:</span></span>

* <span data-ttu-id="653dc-204">Lze zadat pouze nula nebo jeden typ šifrování.</span><span class="sxs-lookup"><span data-stu-id="653dc-204">Only zero or one encryption type can be specified.</span></span>
* <span data-ttu-id="653dc-205">Typ šifrování nemusí být zadaného v adrese URL, pokud jenom jeden šifrování byla použita pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="653dc-205">The encryption type doesn't have to be specified in the URL if only one encryption was applied to the asset.</span></span>
* <span data-ttu-id="653dc-206">Typ šifrování je malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="653dc-206">The encryption type is case insensitive.</span></span>
* <span data-ttu-id="653dc-207">Určit lze následující typy šifrování:</span><span class="sxs-lookup"><span data-stu-id="653dc-207">The following encryption types can be specified:</span></span>  
  * <span data-ttu-id="653dc-208">**šifrování cenc**: Common encryption (PlayReady nebo Widevine)</span><span class="sxs-lookup"><span data-stu-id="653dc-208">**cenc**:  Common encryption (PlayReady or Widevine)</span></span>
  * <span data-ttu-id="653dc-209">**cbcs-aapl**: FairPlay</span><span class="sxs-lookup"><span data-stu-id="653dc-209">**cbcs-aapl**: FairPlay</span></span>
  * <span data-ttu-id="653dc-210">**CBC**: šifrování AES obálky</span><span class="sxs-lookup"><span data-stu-id="653dc-210">**cbc**: AES envelope encryption</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="653dc-211">Vytvoření a konfigurace projektu Visual Studia</span><span class="sxs-lookup"><span data-stu-id="653dc-211">Create and configure a Visual Studio project</span></span>

1. <span data-ttu-id="653dc-212">Nastavte své vývojové prostředí a v souboru app.config vyplňte informace o připojení, jak je popsáno v tématu [Vývoj pro Media Services v .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="653dc-212">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 
2. <span data-ttu-id="653dc-213">Do části **appSettings** definované ve vašem souboru app.config přidejte následující elementy:</span><span class="sxs-lookup"><span data-stu-id="653dc-213">Add the following elements to **appSettings** defined in your app.config file:</span></span>

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

## <a name="example"></a><span data-ttu-id="653dc-214">Příklad</span><span class="sxs-lookup"><span data-stu-id="653dc-214">Example</span></span>

<span data-ttu-id="653dc-215">Následující příklad ukazuje možnost používat pro doručování obsahu šifrován FairPlay Media Services.</span><span class="sxs-lookup"><span data-stu-id="653dc-215">The following sample demonstrates the ability to use Media Services to deliver your content encrypted with FairPlay.</span></span> <span data-ttu-id="653dc-216">Tato funkce byla zavedená v Azure Media Services SDK pro .NET verze 3.6.0.</span><span class="sxs-lookup"><span data-stu-id="653dc-216">This functionality was introduced in the Azure Media Services SDK for .NET version 3.6.0.</span></span> 

<span data-ttu-id="653dc-217">Přepište kód v souboru Program.cs kódem zobrazeným v této části.</span><span class="sxs-lookup"><span data-stu-id="653dc-217">Overwrite the code in your Program.cs file with the code shown in this section.</span></span>

>[!NOTE]
><span data-ttu-id="653dc-218">Je stanovený limit 1 000 000 různých zásad AMS (třeba zásady lokátoru nebo ContentKeyAuthorizationPolicy).</span><span class="sxs-lookup"><span data-stu-id="653dc-218">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="653dc-219">Pokud vždy používáte stejné dny / přístupová oprávnění, například zásady pro lokátory, které mají zůstat na místě po dlouhou dobu (zásady bez odeslání), měli byste použít stejné ID zásad.</span><span class="sxs-lookup"><span data-stu-id="653dc-219">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="653dc-220">Další informace najdete v [tomto](media-services-dotnet-manage-entities.md#limit-access-policies) tématu.</span><span class="sxs-lookup"><span data-stu-id="653dc-220">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

<span data-ttu-id="653dc-221">Nezapomeňte aktualizovat proměnné tak, aby odkazovaly do složek, ve kterých jsou umístěné vaše vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="653dc-221">Make sure to update variables to point to folders where your input files are located.</span></span>

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Threading;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
    using Newtonsoft.Json;
    using System.Security.Cryptography.X509Certificates;

    namespace DynamicEncryptionWithFairPlay
    {
        class Program
        {
        // Read values from the App.config file.
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

            IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
            Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
            Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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

            // Generate a test token based on the the data in the given TokenRestrictionTemplate.
            // Note, you need to pass the key id Guid because we specified
            // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
            Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
            string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                        DateTime.UtcNow.AddDays(365));
            Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
            Console.WriteLine();
            }

            string url = GetStreamingOriginLocator(encodedAsset);
            Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);

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

            IJob job = _context.Jobs.Create(String.Format("Encoding {0}", inputAsset.Name));

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

        static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
        {
            // Create HLS SAMPLE AES encryption content key
            Guid keyId = Guid.NewGuid();
            byte[] contentKey = GetRandomBuffer(16);

            IContentKey key = _context.ContentKeys.Create(
                        keyId,
                        contentKey,
                        "ContentKey",
                        ContentKeyType.CommonEncryptionCbcs);

            // Associate the key with the asset.
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


            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();

            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
            ContentKeyDeliveryType.FairPlay,
            restrictions,
            FairPlayConfiguration);


            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key.
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

            // Configure FairPlay policy option.
            string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();


            IContentKeyAuthorizationPolicyOption FairPlayPolicy =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                   ContentKeyDeliveryType.FairPlay,
                   restrictions,
                   FairPlayConfiguration);

            IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                ContentKeyAuthorizationPolicies.
                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                Result;

            contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);

            // Associate the content key authorization policy with the content key
            contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
            contentKey = contentKey.UpdateAsync().Result;

            return tokenTemplateString;
        }

        private static string ConfigureFairPlayPolicyOptions()
        {
            // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK.
            // However, for production you must use a real ASK from Apple bound to a real prod certificate.
            byte[] askBytes = Guid.NewGuid().ToByteArray();
            var askId = Guid.NewGuid();
            // Key delivery retrieves askKey by askId and uses this key to generate the response.
            IContentKey askKey = _context.ContentKeys.Create(
                        askId,
                        askBytes,
                        "askKey",
                        ContentKeyType.FairPlayASk);

            //Customer password for creating the .pfx file.
            string pfxPassword = "<customer password for creating the .pfx file>";
            // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
            var pfxPasswordId = Guid.NewGuid();
            byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
            IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                        pfxPasswordId,
                        pfxPasswordBytes,
                        "pfxPasswordKey",
                        ContentKeyType.FairPlayPfxPassword);

            // iv - 16 bytes random value, must match the iv in the asset delivery policy.
            byte[] iv = Guid.NewGuid().ToByteArray();

            //Specify the .pfx file created by the customer.
            var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);

            string FairPlayConfiguration =
            Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                appCert,
                pfxPassword,
                pfxPasswordId,
                askId,
                iv);

            return FairPlayConfiguration;
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

        static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
        {
            var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();

            var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);

            FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);

            // Get the FairPlay license service URL.
            Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);

            // The reason the below code replaces "https://" with "skd://" is because
            // in the IOS player sample code which you obtained in Apple developer account,
            // the player only recognizes a Key URL that starts with skd://.
            // However, if you are using a customized player,
            // you can choose whatever protocol you want.
            // For example, "https".

            Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                    {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                    {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
            };

            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            assetDeliveryPolicyConfiguration);

            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        }


        /// <summary>
        /// Gets the streaming origin locator.
        /// </summary>
        /// <param name="assets"></param>
        /// <returns></returns>
        static public string GetStreamingOriginLocator(IAsset asset)
        {

            // Get a reference to the streaming manifest file from the  
            // collection of files in the asset.

            var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                         EndsWith(".ism")).
                         FirstOrDefault();

            // Create a 30-day readonly access policy.
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


## <a name="next-steps-media-services-learning-paths"></a><span data-ttu-id="653dc-222">Další kroky: Mapy kurzů ke službě Media Services</span><span class="sxs-lookup"><span data-stu-id="653dc-222">Next steps: Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="653dc-223">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="653dc-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
