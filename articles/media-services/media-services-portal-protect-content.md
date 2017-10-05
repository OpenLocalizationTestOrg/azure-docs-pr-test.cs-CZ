---
title: "Konfigurace zásad ochrany obsahu pomocí portálu Azure | Microsoft Docs"
description: "Tento článek ukazuje, jak pomocí portálu Azure ke konfiguraci zásad ochrany obsahu. Článek také ukazuje, jak povolit dynamické šifrování pro vaše prostředky."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 67b3fa9936daebeafb7e87fe3a7b0c7e0105b3b3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a><span data-ttu-id="f3462-104">Konfigurace zásad ochrany obsahu pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f3462-104">Configuring content protection policies using the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="f3462-105">K dokončení tohoto kurzu potřebujete mít účet Azure.</span><span class="sxs-lookup"><span data-stu-id="f3462-105">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="f3462-106">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f3462-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="f3462-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="f3462-107">Overview</span></span>
<span data-ttu-id="f3462-108">Microsoft Azure Media Services (AMS) umožňuje zabezpečit médiu od okamžiku, kdy by poté počítač prostřednictvím úložiště, zpracování a doručení.</span><span class="sxs-lookup"><span data-stu-id="f3462-108">Microsoft Azure Media Services (AMS) enables you to secure your media from the time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="f3462-109">Služba Media Services umožňuje doručovat obsah šifrované dynamicky s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování), pomocí PlayReady nebo Widevine DRM a Apple FairPlay běžné šifrování (CENC).</span><span class="sxs-lookup"><span data-stu-id="f3462-109">Media Services allows you to deliver your content encrypted dynamically with Advanced Encryption Standard (AES) (using 128-bit encryption keys), common encryption (CENC) using PlayReady and/or Widevine DRM, and Apple FairPlay.</span></span> 

<span data-ttu-id="f3462-110">AMS poskytuje službu k doručování licencí DRM a AES zrušte klíče autorizovaným klientům.</span><span class="sxs-lookup"><span data-stu-id="f3462-110">AMS provides a service for delivering DRM licenses and AES clear keys to authorized clients.</span></span> <span data-ttu-id="f3462-111">Na portálu Azure můžete vytvořit jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.</span><span class="sxs-lookup"><span data-stu-id="f3462-111">The Azure portal enables you to create one **key/license authorization policy** for all types of encryptions.</span></span>

<span data-ttu-id="f3462-112">Tento článek ukazuje, jak nakonfigurovat zásady ochrany obsahu pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f3462-112">This article demonstrates how to configure content protection policies with the Azure portal.</span></span> <span data-ttu-id="f3462-113">Článek také ukazuje, jak chcete použít dynamické šifrování pro vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="f3462-113">The article also shows how to apply dynamic encryption to your assets.</span></span>


> [!NOTE]
> <span data-ttu-id="f3462-114">Pokud jste použili portál Azure classic k vytvoření zásady ochrany, zásady se nemůže nacházet v [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f3462-114">If you used the Azure classic portal to create protection policies, the policies may not appear in the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="f3462-115">Ale všechny zásady starý stále existují.</span><span class="sxs-lookup"><span data-stu-id="f3462-115">However, all the old polices still exist.</span></span> <span data-ttu-id="f3462-116">Můžete zkontrolovat pomocí Azure Media Services .NET SDK nebo [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) nástroj (viz zásady, klikněte pravým tlačítkem na asset -> informace (F4) -> zobrazení, klikněte na kartu klíčů k obsahu -> klikněte na klíč).</span><span class="sxs-lookup"><span data-stu-id="f3462-116">You can examine them using the Azure Media Services .NET SDK or the [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) tool (to see the policies, right-click on the asset -> Display information (F4)->click on Content keys tab-> click on the key).</span></span> 
> 
> <span data-ttu-id="f3462-117">Pokud chcete zašifrovat asset pomocí nové zásady, je nakonfigurovat pomocí portálu Azure, klikněte na Uložit a znovu použít dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="f3462-117">If you want to encrypt your asset using new policies, configure them with the Azure portal, click save, and reapply dynamic encryption.</span></span> 
> 
> 

## <a name="start-configuring-content-protection"></a><span data-ttu-id="f3462-118">Zahájit konfiguraci ochrany obsahu</span><span class="sxs-lookup"><span data-stu-id="f3462-118">Start configuring content protection</span></span>
<span data-ttu-id="f3462-119">Pomocí portálu pro spuštění konfigurace ochrany obsahu, globální vašeho účtu AMS, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f3462-119">To use the portal to start configuring content protection, global to your AMS account, do the following:</span></span>

1. <span data-ttu-id="f3462-120">Na webu [Azure Portal](https://portal.azure.com/) zvolte účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3462-120">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="f3462-121">Vyberte **nastavení** > **obsahu ochrany**.</span><span class="sxs-lookup"><span data-stu-id="f3462-121">Select **Settings** > **Content protection**.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a><span data-ttu-id="f3462-123">Zásady autorizace pro klíč nebo licencí</span><span class="sxs-lookup"><span data-stu-id="f3462-123">Key/license authorization policy</span></span>
<span data-ttu-id="f3462-124">AMS podporuje více způsobů ověřování uživatelů, kteří žádají o klíč nebo licencí.</span><span class="sxs-lookup"><span data-stu-id="f3462-124">AMS supports multiple ways of authenticating users who make key or license requests.</span></span> <span data-ttu-id="f3462-125">Zásady autorizace klíče obsahu musíte nakonfigurovat a splní vašeho klienta v pořadí pro klíč nebo licenci k být delived do klienta.</span><span class="sxs-lookup"><span data-stu-id="f3462-125">The content key authorization policy must be configured by you and met by your client in order for the key/license to be delived to the client.</span></span> <span data-ttu-id="f3462-126">Zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.</span><span class="sxs-lookup"><span data-stu-id="f3462-126">The content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span>

<span data-ttu-id="f3462-127">Na portálu Azure můžete vytvořit jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.</span><span class="sxs-lookup"><span data-stu-id="f3462-127">The Azure portal enables you to create one **key/license authorization policy** for all types of encryptions.</span></span>

### <a name="open"></a><span data-ttu-id="f3462-128">Otevřenost</span><span class="sxs-lookup"><span data-stu-id="f3462-128">Open</span></span>
<span data-ttu-id="f3462-129">Otevřete omezení znamená, že systém získávat klíč všem uživatelům, kteří vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="f3462-129">Open restriction means that the system will deliver the key to anyone who makes a key request.</span></span> <span data-ttu-id="f3462-130">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="f3462-130">This restriction might be useful for test purposes.</span></span> 

### <a name="token"></a><span data-ttu-id="f3462-131">Token</span><span class="sxs-lookup"><span data-stu-id="f3462-131">Token</span></span>
<span data-ttu-id="f3462-132">Zásady omezení tokenem musí být doplněny tokenem vydaným službou tokenů zabezpečení (STS).</span><span class="sxs-lookup"><span data-stu-id="f3462-132">The token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="f3462-133">Služba Media Services podporuje tokeny ve formátu jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT).</span><span class="sxs-lookup"><span data-stu-id="f3462-133">Media Services supports tokens in the Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span> <span data-ttu-id="f3462-134">Služba Media Services neposkytuje zabezpečení tokenu služby.</span><span class="sxs-lookup"><span data-stu-id="f3462-134">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="f3462-135">Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS problém tokeny.</span><span class="sxs-lookup"><span data-stu-id="f3462-135">You can create a custom STS or leverage Microsoft Azure ACS to issue tokens.</span></span> <span data-ttu-id="f3462-136">Služba tokenů zabezpečení musí být nakonfigurované vytvořit token podepsané zadaný klíč a vystavování deklarací identity, které jste zadali v nastavení omezení s tokenem.</span><span class="sxs-lookup"><span data-stu-id="f3462-136">The STS must be configured to create a token signed with the specified key and issue claims that you specified in the token restriction configuration.</span></span> <span data-ttu-id="f3462-137">Služba Media Services doručení klíče vrátí požadovaný klíč (nebo licencí) do klienta, pokud je token platný a deklarace identity v tokenu shody ty nakonfigurované pro klíč (nebo licencí).</span><span class="sxs-lookup"><span data-stu-id="f3462-137">The Media Services key delivery service will return the requested key (or license) to the client if the token is valid and the claims in the token match those configured for the key (or license).</span></span>

<span data-ttu-id="f3462-138">Při konfiguraci token omezený zásad, musíte zadat ověření primární klíč, vystavitele a cílová skupina parametry.</span><span class="sxs-lookup"><span data-stu-id="f3462-138">When configuring the token restricted policy, you must specify the primary verification key, issuer, and audience parameters.</span></span> <span data-ttu-id="f3462-139">Ověření primární klíč obsahuje klíč, který byl podepsaný token, Vystavitel je zabezpečený tokenu služba, která vydá token.</span><span class="sxs-lookup"><span data-stu-id="f3462-139">The primary verification key contains the key that the token was signed with, issuer is the secure token service that issues the token.</span></span> <span data-ttu-id="f3462-140">Cílová skupina (někdy nazývané oboru) popisuje záměr tokenu nebo prostředek token povolí přístup k.</span><span class="sxs-lookup"><span data-stu-id="f3462-140">The audience (sometimes called scope) describes the intent of the token or the resource the token authorizes access to.</span></span> <span data-ttu-id="f3462-141">Služba Media Services doručení klíče ověří, jestli tyto hodnoty v tokenu shodují s hodnotami v šabloně.</span><span class="sxs-lookup"><span data-stu-id="f3462-141">The Media Services key delivery service validates that these values in the token match the values in the template.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a><span data-ttu-id="f3462-143">Šablony práv PlayReady</span><span class="sxs-lookup"><span data-stu-id="f3462-143">PlayReady rights template</span></span>
<span data-ttu-id="f3462-144">Podrobné informace o šabloně rights PlayReady, najdete v části [Media Services PlayReady licence šablony přehled](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3462-144">For detailed information about the PlayReady rights template, see [Media Services PlayReady License Template Overview](media-services-playready-license-template-overview.md).</span></span>

### <a name="non-persistent"></a><span data-ttu-id="f3462-145">Bez trvalé</span><span class="sxs-lookup"><span data-stu-id="f3462-145">Non persistent</span></span>
<span data-ttu-id="f3462-146">Pokud nakonfigurujete jako dočasnou licenci, pouze udržované ve paměti při přehrávač používá licence.</span><span class="sxs-lookup"><span data-stu-id="f3462-146">If you configure license as non-persistent, it is only held in memory while the player is using the license.</span></span>  

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a><span data-ttu-id="f3462-148">Trvalé</span><span class="sxs-lookup"><span data-stu-id="f3462-148">Persistent</span></span>
<span data-ttu-id="f3462-149">Pokud konfigurujete licence jako trvalé, se uloží do trvalého úložiště na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f3462-149">If you configure the license  as persistent, it is saved in persistent storage on the client.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a><span data-ttu-id="f3462-151">Šablony práv Widevine</span><span class="sxs-lookup"><span data-stu-id="f3462-151">Widevine rights template</span></span>
<span data-ttu-id="f3462-152">Podrobné informace o šabloně rights Widevine, najdete v části [přehled šablonu licence Widevine](media-services-widevine-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f3462-152">For detailed information about the Widevine rights template, see [Widevine License Template Overview](media-services-widevine-license-template-overview.md).</span></span>

### <a name="basic"></a><span data-ttu-id="f3462-153">Basic</span><span class="sxs-lookup"><span data-stu-id="f3462-153">Basic</span></span>
<span data-ttu-id="f3462-154">Když vyberete **základní**, vytvoří se šablony s všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f3462-154">When you select **Basic**, the template will be created with all defaults values.</span></span>

### <a name="advanced"></a><span data-ttu-id="f3462-155">Advanced</span><span class="sxs-lookup"><span data-stu-id="f3462-155">Advanced</span></span>
<span data-ttu-id="f3462-156">Podrobné vysvětlení o – možnost zálohy Widevine konfigurací najdete v tématu [to](media-services-widevine-license-template-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="f3462-156">For detailed explanation about advance option of Widevine configurations, see [this](media-services-widevine-license-template-overview.md) topic.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a><span data-ttu-id="f3462-158">Konfigurace FairPlay</span><span class="sxs-lookup"><span data-stu-id="f3462-158">FairPlay configuration</span></span>
<span data-ttu-id="f3462-159">Pokud chcete povolit šifrování FairPlay, potřebujete poskytovat certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím možnost FairPlay konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f3462-159">To enable FairPlay encryption, you need to provide the App Certificate and Application Secret Key (ASK) through the FairPlay Configuration option.</span></span> <span data-ttu-id="f3462-160">Podrobné informace o konfiguraci FairPlay a požadavky najdete v tématu [to](media-services-protect-hls-with-fairplay.md) článku.</span><span class="sxs-lookup"><span data-stu-id="f3462-160">For detailed information about FairPlay configuration and requirements, see [this](media-services-protect-hls-with-fairplay.md) article.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a><span data-ttu-id="f3462-162">Použít na asset dynamické šifrování</span><span class="sxs-lookup"><span data-stu-id="f3462-162">Apply dynamic encryption to your asset</span></span>
<span data-ttu-id="f3462-163">Abyste mohli využívat dynamické šifrování, budete muset zakódovat váš zdrojový soubor do sady souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="f3462-163">To take advantage of dynamic encryption, you need to encode your source file into a set of adaptive-bitrate MP4 files.</span></span>

### <a name="select-an-asset-that-you-want-to-encrypt"></a><span data-ttu-id="f3462-164">Vyberte asset, který chcete zašifrovat</span><span class="sxs-lookup"><span data-stu-id="f3462-164">Select an asset that you want to encrypt</span></span>
<span data-ttu-id="f3462-165">Pokud chcete zobrazit všechny prostředky, vyberte **nastavení** > **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="f3462-165">To see all your assets, select **Settings** > **Assets**.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a><span data-ttu-id="f3462-167">Šifrování AES nebo DRM</span><span class="sxs-lookup"><span data-stu-id="f3462-167">Encrypt with AES or DRM</span></span>
<span data-ttu-id="f3462-168">Po stisknutí klávesy **šifrovat** na prostředek, se zobrazí dvě možnosti: **AES** nebo **DRM**.</span><span class="sxs-lookup"><span data-stu-id="f3462-168">Once you press **Encrypt** on an asset, you are presented wtih two choices: **AES** or **DRM**.</span></span> 

#### <a name="aes"></a><span data-ttu-id="f3462-169">AES</span><span class="sxs-lookup"><span data-stu-id="f3462-169">AES</span></span>
<span data-ttu-id="f3462-170">AES zrušte Povolí šifrování pomocí klíče na všechny protokoly streamování: technologie Smooth Streaming, HLS a MPEG-DASH.</span><span class="sxs-lookup"><span data-stu-id="f3462-170">AES clear key encryption will be enabled on all streaming protocols: Smooth Streaming, HLS, and MPEG-DASH.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a><span data-ttu-id="f3462-172">DRM</span><span class="sxs-lookup"><span data-stu-id="f3462-172">DRM</span></span>
<span data-ttu-id="f3462-173">Když vyberete kartu DRM, budete mít různé možnosti zásad ochrany obsahu (který musíte nakonfigurovat nyní) + sadu protokolů streamování.</span><span class="sxs-lookup"><span data-stu-id="f3462-173">When you select the DRM tab, you are presented with different choices of content protection policies (which you must have configured by now) + a set of streaming protocols.</span></span>

* <span data-ttu-id="f3462-174">**PlayReady a Widevine s MPEG-DASH** -bude dynamicky šifrovat MPEG-DASH datového proudu s technologií PlayReady a Widevine technologiemi DRM.</span><span class="sxs-lookup"><span data-stu-id="f3462-174">**PlayReady and Widevine with MPEG-DASH** - will dynamically encrypt your MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span>
* <span data-ttu-id="f3462-175">**PlayReady a Widevine s MPEG-DASH + FairPlay s HLS** -bude dynamicky šifrovat stream MPEG-DASH s technologií PlayReady a Widevine technologiemi DRM.</span><span class="sxs-lookup"><span data-stu-id="f3462-175">**PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** - will dynamically encrypt you MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="f3462-176">Bude se šifrovat taky vaše datové proudy HLS s FairPlay.</span><span class="sxs-lookup"><span data-stu-id="f3462-176">Will also encrypt your HLS streams with FairPlay.</span></span>
* <span data-ttu-id="f3462-177">**PlayReady pouze s technologie Smooth Streaming, HLS a MPEG-DASH** -bude dynamicky šifrovat technologie Smooth Streaming, HLS, datové proudy MPEG-DASH s technologií PlayReady DRM.</span><span class="sxs-lookup"><span data-stu-id="f3462-177">**PlayReady only with Smooth Streaming, HLS and MPEG-DASH** - will dynamically encrypt Smooth Streaming, HLS, MPEG-DASH streams with PlayReady DRM.</span></span>
* <span data-ttu-id="f3462-178">**Widevine pouze s MPEG-DASH** -bude dynamicky šifrovat můžete MPEG-DASH pomocí Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="f3462-178">**Widevine only with MPEG-DASH** - will dynamically encrypt you MPEG-DASH with Widevine DRM.</span></span>
* <span data-ttu-id="f3462-179">**FairPlay pouze s HLS** -bude dynamicky šifrovat HLS datového proudu s FairPlay.</span><span class="sxs-lookup"><span data-stu-id="f3462-179">**FairPlay only with HLS** - will dynamically encrypt your HLS stream with FairPlay.</span></span>

<span data-ttu-id="f3462-180">Chcete-li povolit šifrování FairPlay, poskytují certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím možnosti konfigurace FairPlay obrazovky okna nastavení ochrany obsahu.</span><span class="sxs-lookup"><span data-stu-id="f3462-180">To enable FairPlay encryption, you need to provide the App Certificate and Application Secret Key (ASK) through the FairPlay Configuration option of the Content Protection settings blade.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection009.png)

<span data-ttu-id="f3462-182">Až provedete výběr šifrování, stiskněte klávesu **použít**.</span><span class="sxs-lookup"><span data-stu-id="f3462-182">Once you make the encryption selection, press **Apply**.</span></span>

>[!NOTE] 
><span data-ttu-id="f3462-183">Pokud máte v úmyslu přehrání AES šifrovat HLS v prohlížeči Safari najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="f3462-183">If you are planning to play an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3462-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f3462-184">Next steps</span></span>
<span data-ttu-id="f3462-185">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="f3462-185">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f3462-186">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="f3462-186">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

