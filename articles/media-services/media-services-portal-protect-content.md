---
title: "zásady ochrany obsahu aaaConfiguring pomocí hello portálu Azure | Microsoft Docs"
description: "Tento článek ukazuje, jak toouse hello Azure portálu tooconfigure zásady ochrany obsahu. Hello článku ukazuje také jak tooenable dynamického šifrování pro vaše prostředky."
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
ms.openlocfilehash: 3e7ce6ddaa0e738b5a1e26dafe9eef2df221f039
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-content-protection-policies-using-hello-azure-portal"></a><span data-ttu-id="42e7b-104">Konfigurace zásad ochrany obsahu pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="42e7b-104">Configuring content protection policies using hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="42e7b-105">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="42e7b-105">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="42e7b-106">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="42e7b-106">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
> 
> 

## <a name="overview"></a><span data-ttu-id="42e7b-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="42e7b-107">Overview</span></span>
<span data-ttu-id="42e7b-108">Microsoft Azure Media Services (AMS) umožňuje vám toosecure médiu z hello doby, kdy opouští váš počítač prostřednictvím úložiště, zpracování a doručení.</span><span class="sxs-lookup"><span data-stu-id="42e7b-108">Microsoft Azure Media Services (AMS) enables you toosecure your media from hello time it leaves your computer through storage, processing, and delivery.</span></span> <span data-ttu-id="42e7b-109">Služba Media Services umožňuje toodeliver obsah šifrované dynamicky s Standard AES (Advanced Encryption) (pomocí klíče 128bitové šifrování), pomocí PlayReady nebo Widevine DRM a Apple FairPlay běžné šifrování (CENC).</span><span class="sxs-lookup"><span data-stu-id="42e7b-109">Media Services allows you toodeliver your content encrypted dynamically with Advanced Encryption Standard (AES) (using 128-bit encryption keys), common encryption (CENC) using PlayReady and/or Widevine DRM, and Apple FairPlay.</span></span> 

<span data-ttu-id="42e7b-110">AMS poskytuje službu k doručování licencí DRM a AES zrušte klíče tooauthorized klientů.</span><span class="sxs-lookup"><span data-stu-id="42e7b-110">AMS provides a service for delivering DRM licenses and AES clear keys tooauthorized clients.</span></span> <span data-ttu-id="42e7b-111">Hello portál Azure vám umožní toocreate jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.</span><span class="sxs-lookup"><span data-stu-id="42e7b-111">hello Azure portal enables you toocreate one **key/license authorization policy** for all types of encryptions.</span></span>

<span data-ttu-id="42e7b-112">Tento článek ukazuje, jak tooconfigure obsahu zásady ochrany s hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="42e7b-112">This article demonstrates how tooconfigure content protection policies with hello Azure portal.</span></span> <span data-ttu-id="42e7b-113">Hello článku ukazuje také jak tooapply dynamického šifrování tooyour prostředky.</span><span class="sxs-lookup"><span data-stu-id="42e7b-113">hello article also shows how tooapply dynamic encryption tooyour assets.</span></span>


> [!NOTE]
> <span data-ttu-id="42e7b-114">Pokud jste použili zásady ochrany Azure classic portálu toocreate hello, zásady hello nebude v hello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="42e7b-114">If you used hello Azure classic portal toocreate protection policies, hello policies may not appear in hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="42e7b-115">Ale všechny hello staré zásady stále existují.</span><span class="sxs-lookup"><span data-stu-id="42e7b-115">However, all hello old polices still exist.</span></span> <span data-ttu-id="42e7b-116">Můžete je zkontrolovat pomocí hello Azure Media Services .NET SDK nebo hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) nástroj (toosee hello zásady, klikněte pravým tlačítkem na hello asset -> zobrazení informace (F4) -> klikněte na klíčů k obsahu -> karta Klikněte na klíč hello).</span><span class="sxs-lookup"><span data-stu-id="42e7b-116">You can examine them using hello Azure Media Services .NET SDK or hello [Azure-Media-Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) tool (toosee hello policies, right-click on hello asset -> Display information (F4)->click on Content keys tab-> click on hello key).</span></span> 
> 
> <span data-ttu-id="42e7b-117">Pokud chcete tooencrypt asset pomocí nové zásady, nakonfigurovat hello portálu Azure, klikněte na Uložit a znovu použít dynamické šifrování.</span><span class="sxs-lookup"><span data-stu-id="42e7b-117">If you want tooencrypt your asset using new policies, configure them with hello Azure portal, click save, and reapply dynamic encryption.</span></span> 
> 
> 

## <a name="start-configuring-content-protection"></a><span data-ttu-id="42e7b-118">Zahájit konfiguraci ochrany obsahu</span><span class="sxs-lookup"><span data-stu-id="42e7b-118">Start configuring content protection</span></span>
<span data-ttu-id="42e7b-119">toouse hello portálu toostart Konfigurace ochrany obsahu, globální tooyour AMS účet hello následující:</span><span class="sxs-lookup"><span data-stu-id="42e7b-119">toouse hello portal toostart configuring content protection, global tooyour AMS account, do hello following:</span></span>

1. <span data-ttu-id="42e7b-120">V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="42e7b-120">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="42e7b-121">Vyberte **nastavení** > **obsahu ochrany**.</span><span class="sxs-lookup"><span data-stu-id="42e7b-121">Select **Settings** > **Content protection**.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a><span data-ttu-id="42e7b-123">Zásady autorizace pro klíč nebo licencí</span><span class="sxs-lookup"><span data-stu-id="42e7b-123">Key/license authorization policy</span></span>
<span data-ttu-id="42e7b-124">AMS podporuje více způsobů ověřování uživatelů, kteří žádají o klíč nebo licencí.</span><span class="sxs-lookup"><span data-stu-id="42e7b-124">AMS supports multiple ways of authenticating users who make key or license requests.</span></span> <span data-ttu-id="42e7b-125">zásady autorizace klíče obsahu Hello musíte nakonfigurovat a splní vašeho klienta v pořadí pro hello klíč nebo licenci toobe delived toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="42e7b-125">hello content key authorization policy must be configured by you and met by your client in order for hello key/license toobe delived toohello client.</span></span> <span data-ttu-id="42e7b-126">Hello zásady autorizace klíče obsahu může mít jeden nebo více omezení autorizace: **otevřete** nebo **tokenu** omezení.</span><span class="sxs-lookup"><span data-stu-id="42e7b-126">hello content key authorization policy could have one or more authorization restrictions: **open** or **token** restriction.</span></span>

<span data-ttu-id="42e7b-127">Hello portál Azure vám umožní toocreate jeden **zásady autorizace pro klíč nebo licenci** pro všechny typy šifrování.</span><span class="sxs-lookup"><span data-stu-id="42e7b-127">hello Azure portal enables you toocreate one **key/license authorization policy** for all types of encryptions.</span></span>

### <a name="open"></a><span data-ttu-id="42e7b-128">Otevřenost</span><span class="sxs-lookup"><span data-stu-id="42e7b-128">Open</span></span>
<span data-ttu-id="42e7b-129">Otevřete omezení znamená, že hello systému dodá hello tooanyone klíče, který vytváří klíče požadavek.</span><span class="sxs-lookup"><span data-stu-id="42e7b-129">Open restriction means that hello system will deliver hello key tooanyone who makes a key request.</span></span> <span data-ttu-id="42e7b-130">Toto omezení může být užitečná pro účely testování.</span><span class="sxs-lookup"><span data-stu-id="42e7b-130">This restriction might be useful for test purposes.</span></span> 

### <a name="token"></a><span data-ttu-id="42e7b-131">Token</span><span class="sxs-lookup"><span data-stu-id="42e7b-131">Token</span></span>
<span data-ttu-id="42e7b-132">zásady omezení tokenem Hello musí být doplněny tokenem vydaným podle zabezpečení tokenu služby (STS).</span><span class="sxs-lookup"><span data-stu-id="42e7b-132">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS).</span></span> <span data-ttu-id="42e7b-133">Služba Media Services podporuje tokeny ve formátu hello jednoduché webové tokeny (SWT) a formátu JSON Web Token (JWT).</span><span class="sxs-lookup"><span data-stu-id="42e7b-133">Media Services supports tokens in hello Simple Web Tokens (SWT) format and JSON Web Token (JWT) format.</span></span> <span data-ttu-id="42e7b-134">Služba Media Services neposkytuje zabezpečení tokenu služby.</span><span class="sxs-lookup"><span data-stu-id="42e7b-134">Media Services does not provide Secure Token Services.</span></span> <span data-ttu-id="42e7b-135">Můžete vytvořit vlastní službu tokenů zabezpečení nebo využívat Microsoft Azure ACS tooissue tokeny.</span><span class="sxs-lookup"><span data-stu-id="42e7b-135">You can create a custom STS or leverage Microsoft Azure ACS tooissue tokens.</span></span> <span data-ttu-id="42e7b-136">Hello služby tokenů zabezpečení musí být nakonfigurované toocreate token podepsané hello zadaný klíč a vystavování deklarací identity, které jste zadali v konfiguraci omezení s tokenem hello.</span><span class="sxs-lookup"><span data-stu-id="42e7b-136">hello STS must be configured toocreate a token signed with hello specified key and issue claims that you specified in hello token restriction configuration.</span></span> <span data-ttu-id="42e7b-137">Hello Media Services, který vrátí doručení klíče služby hello požadovanou klíč (nebo licencí) toohello klienta, když je platný hello token a hello deklarace identity ve hello tokenu shoda ty nakonfigurované pro klíč hello (nebo licencí).</span><span class="sxs-lookup"><span data-stu-id="42e7b-137">hello Media Services key delivery service will return hello requested key (or license) toohello client if hello token is valid and hello claims in hello token match those configured for hello key (or license).</span></span>

<span data-ttu-id="42e7b-138">Při konfiguraci zásady omezení tokenem hello, musíte zadat klíč hello primární ověření, vystavitele a cílová skupina parametry.</span><span class="sxs-lookup"><span data-stu-id="42e7b-138">When configuring hello token restricted policy, you must specify hello primary verification key, issuer, and audience parameters.</span></span> <span data-ttu-id="42e7b-139">Hello ověření primární klíč obsahuje hello klíče, že byl podepsaný hello token, vystavitele hello zabezpečené tokenu služba, která vydá hello token.</span><span class="sxs-lookup"><span data-stu-id="42e7b-139">hello primary verification key contains hello key that hello token was signed with, issuer is hello secure token service that issues hello token.</span></span> <span data-ttu-id="42e7b-140">Cílová skupina Hello (někdy nazývané oboru) popisuje hello záměr hello tokenu nebo token hello prostředku hello povolí přístup k.</span><span class="sxs-lookup"><span data-stu-id="42e7b-140">hello audience (sometimes called scope) describes hello intent of hello token or hello resource hello token authorizes access to.</span></span> <span data-ttu-id="42e7b-141">Hello doručení klíče služby Media Services ověří, že tyto hodnoty v tokenu hello shodují s hodnotami hello v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="42e7b-141">hello Media Services key delivery service validates that these values in hello token match hello values in hello template.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a><span data-ttu-id="42e7b-143">Šablony práv PlayReady</span><span class="sxs-lookup"><span data-stu-id="42e7b-143">PlayReady rights template</span></span>
<span data-ttu-id="42e7b-144">Podrobné informace o šablony práv PlayReady hello najdete v tématu [Media Services PlayReady licence šablony přehled](media-services-playready-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="42e7b-144">For detailed information about hello PlayReady rights template, see [Media Services PlayReady License Template Overview](media-services-playready-license-template-overview.md).</span></span>

### <a name="non-persistent"></a><span data-ttu-id="42e7b-145">Bez trvalé</span><span class="sxs-lookup"><span data-stu-id="42e7b-145">Non persistent</span></span>
<span data-ttu-id="42e7b-146">Pokud nakonfigurujete jako dočasnou licenci, pouze udržované ve paměti při hello player je pomocí hello licence.</span><span class="sxs-lookup"><span data-stu-id="42e7b-146">If you configure license as non-persistent, it is only held in memory while hello player is using hello license.</span></span>  

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a><span data-ttu-id="42e7b-148">Trvalé</span><span class="sxs-lookup"><span data-stu-id="42e7b-148">Persistent</span></span>
<span data-ttu-id="42e7b-149">Pokud konfigurujete hello licence jako trvalé, se uloží do trvalého úložiště na klientovi hello.</span><span class="sxs-lookup"><span data-stu-id="42e7b-149">If you configure hello license  as persistent, it is saved in persistent storage on hello client.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a><span data-ttu-id="42e7b-151">Šablony práv Widevine</span><span class="sxs-lookup"><span data-stu-id="42e7b-151">Widevine rights template</span></span>
<span data-ttu-id="42e7b-152">Podrobné informace o šablony práv Widevine hello najdete v tématu [přehled šablonu licence Widevine](media-services-widevine-license-template-overview.md).</span><span class="sxs-lookup"><span data-stu-id="42e7b-152">For detailed information about hello Widevine rights template, see [Widevine License Template Overview](media-services-widevine-license-template-overview.md).</span></span>

### <a name="basic"></a><span data-ttu-id="42e7b-153">Basic</span><span class="sxs-lookup"><span data-stu-id="42e7b-153">Basic</span></span>
<span data-ttu-id="42e7b-154">Když vyberete **základní**, vytvoří se hello šablony s všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="42e7b-154">When you select **Basic**, hello template will be created with all defaults values.</span></span>

### <a name="advanced"></a><span data-ttu-id="42e7b-155">Advanced</span><span class="sxs-lookup"><span data-stu-id="42e7b-155">Advanced</span></span>
<span data-ttu-id="42e7b-156">Podrobné vysvětlení o – možnost zálohy Widevine konfigurací najdete v tématu [to](media-services-widevine-license-template-overview.md) tématu.</span><span class="sxs-lookup"><span data-stu-id="42e7b-156">For detailed explanation about advance option of Widevine configurations, see [this](media-services-widevine-license-template-overview.md) topic.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a><span data-ttu-id="42e7b-158">Konfigurace FairPlay</span><span class="sxs-lookup"><span data-stu-id="42e7b-158">FairPlay configuration</span></span>
<span data-ttu-id="42e7b-159">tooenable FairPlay šifrování, potřebujete tooprovide hello certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím hello možnost FairPlay konfigurace.</span><span class="sxs-lookup"><span data-stu-id="42e7b-159">tooenable FairPlay encryption, you need tooprovide hello App Certificate and Application Secret Key (ASK) through hello FairPlay Configuration option.</span></span> <span data-ttu-id="42e7b-160">Podrobné informace o konfiguraci FairPlay a požadavky najdete v tématu [to](media-services-protect-hls-with-fairplay.md) článku.</span><span class="sxs-lookup"><span data-stu-id="42e7b-160">For detailed information about FairPlay configuration and requirements, see [this](media-services-protect-hls-with-fairplay.md) article.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-tooyour-asset"></a><span data-ttu-id="42e7b-162">Použití dynamické šifrování tooyour asset</span><span class="sxs-lookup"><span data-stu-id="42e7b-162">Apply dynamic encryption tooyour asset</span></span>
<span data-ttu-id="42e7b-163">tootake využívat dynamické šifrování, je nutné tooencode zdrojového souboru do sady souborů MP4 s adaptivní přenosovou rychlostí.</span><span class="sxs-lookup"><span data-stu-id="42e7b-163">tootake advantage of dynamic encryption, you need tooencode your source file into a set of adaptive-bitrate MP4 files.</span></span>

### <a name="select-an-asset-that-you-want-tooencrypt"></a><span data-ttu-id="42e7b-164">Vyberte prostředek, který má tooencrypt</span><span class="sxs-lookup"><span data-stu-id="42e7b-164">Select an asset that you want tooencrypt</span></span>
<span data-ttu-id="42e7b-165">Vyberte všechny vaše prostředky toosee **nastavení** > **prostředky**.</span><span class="sxs-lookup"><span data-stu-id="42e7b-165">toosee all your assets, select **Settings** > **Assets**.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a><span data-ttu-id="42e7b-167">Šifrování AES nebo DRM</span><span class="sxs-lookup"><span data-stu-id="42e7b-167">Encrypt with AES or DRM</span></span>
<span data-ttu-id="42e7b-168">Po stisknutí klávesy **šifrovat** na prostředek, se zobrazí dvě možnosti: **AES** nebo **DRM**.</span><span class="sxs-lookup"><span data-stu-id="42e7b-168">Once you press **Encrypt** on an asset, you are presented wtih two choices: **AES** or **DRM**.</span></span> 

#### <a name="aes"></a><span data-ttu-id="42e7b-169">AES</span><span class="sxs-lookup"><span data-stu-id="42e7b-169">AES</span></span>
<span data-ttu-id="42e7b-170">AES zrušte Povolí šifrování pomocí klíče na všechny protokoly streamování: technologie Smooth Streaming, HLS a MPEG-DASH.</span><span class="sxs-lookup"><span data-stu-id="42e7b-170">AES clear key encryption will be enabled on all streaming protocols: Smooth Streaming, HLS, and MPEG-DASH.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a><span data-ttu-id="42e7b-172">DRM</span><span class="sxs-lookup"><span data-stu-id="42e7b-172">DRM</span></span>
<span data-ttu-id="42e7b-173">Když vyberete kartu hello DRM, budete mít různé možnosti zásad ochrany obsahu (který musíte nakonfigurovat nyní) + sadu protokolů streamování.</span><span class="sxs-lookup"><span data-stu-id="42e7b-173">When you select hello DRM tab, you are presented with different choices of content protection policies (which you must have configured by now) + a set of streaming protocols.</span></span>

* <span data-ttu-id="42e7b-174">**PlayReady a Widevine s MPEG-DASH** -bude dynamicky šifrovat MPEG-DASH datového proudu s technologií PlayReady a Widevine technologiemi DRM.</span><span class="sxs-lookup"><span data-stu-id="42e7b-174">**PlayReady and Widevine with MPEG-DASH** - will dynamically encrypt your MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span>
* <span data-ttu-id="42e7b-175">**PlayReady a Widevine s MPEG-DASH + FairPlay s HLS** -bude dynamicky šifrovat stream MPEG-DASH s technologií PlayReady a Widevine technologiemi DRM.</span><span class="sxs-lookup"><span data-stu-id="42e7b-175">**PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** - will dynamically encrypt you MPEG-DASH stream with PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="42e7b-176">Bude se šifrovat taky vaše datové proudy HLS s FairPlay.</span><span class="sxs-lookup"><span data-stu-id="42e7b-176">Will also encrypt your HLS streams with FairPlay.</span></span>
* <span data-ttu-id="42e7b-177">**PlayReady pouze s technologie Smooth Streaming, HLS a MPEG-DASH** -bude dynamicky šifrovat technologie Smooth Streaming, HLS, datové proudy MPEG-DASH s technologií PlayReady DRM.</span><span class="sxs-lookup"><span data-stu-id="42e7b-177">**PlayReady only with Smooth Streaming, HLS and MPEG-DASH** - will dynamically encrypt Smooth Streaming, HLS, MPEG-DASH streams with PlayReady DRM.</span></span>
* <span data-ttu-id="42e7b-178">**Widevine pouze s MPEG-DASH** -bude dynamicky šifrovat můžete MPEG-DASH pomocí Widevine DRM.</span><span class="sxs-lookup"><span data-stu-id="42e7b-178">**Widevine only with MPEG-DASH** - will dynamically encrypt you MPEG-DASH with Widevine DRM.</span></span>
* <span data-ttu-id="42e7b-179">**FairPlay pouze s HLS** -bude dynamicky šifrovat HLS datového proudu s FairPlay.</span><span class="sxs-lookup"><span data-stu-id="42e7b-179">**FairPlay only with HLS** - will dynamically encrypt your HLS stream with FairPlay.</span></span>

<span data-ttu-id="42e7b-180">tooenable FairPlay šifrování, potřebujete tooprovide hello certifikát aplikace a aplikace tajný klíč (požádejte) prostřednictvím hello možnost konfigurace FairPlay okna nastavení ochrany obsahu hello.</span><span class="sxs-lookup"><span data-stu-id="42e7b-180">tooenable FairPlay encryption, you need tooprovide hello App Certificate and Application Secret Key (ASK) through hello FairPlay Configuration option of hello Content Protection settings blade.</span></span>

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection009.png)

<span data-ttu-id="42e7b-182">Až provedete výběr hello šifrování, stiskněte klávesu **použít**.</span><span class="sxs-lookup"><span data-stu-id="42e7b-182">Once you make hello encryption selection, press **Apply**.</span></span>

>[!NOTE] 
><span data-ttu-id="42e7b-183">Pokud plánujete tooplay AES šifrovat HLS v prohlížeči Safari najdete v tématu [tomto blogu](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span><span class="sxs-lookup"><span data-stu-id="42e7b-183">If you are planning tooplay an AES encrypted HLS in Safari, see [this blog](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="42e7b-184">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42e7b-184">Next steps</span></span>
<span data-ttu-id="42e7b-185">Prohlédněte si mapy kurzů k Media Services.</span><span class="sxs-lookup"><span data-stu-id="42e7b-185">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="42e7b-186">Poskytnutí zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="42e7b-186">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

