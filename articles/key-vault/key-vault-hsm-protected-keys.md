---
title: "aaaHow toogenerate a přenos klíčů chráněných pomocí HSM pro Azure Key Vault | Microsoft Docs"
description: "Použijte tento článek toohelp plánování, generovat a potom přeneste vlastní toouse klíčů chráněných pomocí HSM s Azure Key Vault. Taky označovaný jako BYOK nebo přineste si vlastní klíč."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: ambapat
ms.openlocfilehash: 3bb234bd1c4b81770542ccf7110e256385ca3309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a><span data-ttu-id="21e89-104">Jak toogenerate a přenos klíčů chráněných pomocí HSM pro Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="21e89-104">How toogenerate and transfer HSM-protected keys for Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="21e89-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="21e89-105">Introduction</span></span>
<span data-ttu-id="21e89-106">Pro lepší kontrolu Pokud používáte Azure Key Vault, můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-106">For added assurance, when you use Azure Key Vault, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="21e89-107">Tento scénář je často označují tooas *přineste si vlastní klíč*, nebo BYOK.</span><span class="sxs-lookup"><span data-stu-id="21e89-107">This scenario is often referred tooas *bring your own key*, or BYOK.</span></span> <span data-ttu-id="21e89-108">Hello moduly hardwarového zabezpečení jsou FIPS 140-2 Level 2 ověřit.</span><span class="sxs-lookup"><span data-stu-id="21e89-108">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="21e89-109">Azure Key Vault používá klíče Thales nShield řadu tooprotect moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="21e89-109">Azure Key Vault uses Thales nShield family of HSMs tooprotect your keys.</span></span>

<span data-ttu-id="21e89-110">Použijte hello informace v této toohelp téma plánování, generovat a potom přeneste vlastní toouse klíčů chráněných pomocí HSM s Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="21e89-110">Use hello information in this topic toohelp you plan for, generate, and then transfer your own HSM-protected keys toouse with Azure Key Vault.</span></span>

<span data-ttu-id="21e89-111">Tato funkce není dostupná pro Azure China.</span><span class="sxs-lookup"><span data-stu-id="21e89-111">This functionality is not available for Azure China.</span></span>

> [!NOTE]
> <span data-ttu-id="21e89-112">Další informace o Azure Key Vault najdete v tématu [co je Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="21e89-112">For more information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>  
>
> <span data-ttu-id="21e89-113">Úvodní kurz, který zahrnuje vytvoření trezoru klíčů pro klíčů chráněných pomocí HSM, najdete v části [Začínáme s Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="21e89-113">For a getting started tutorial, which includes creating a key vault for HSM-protected keys, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>
>
>

<span data-ttu-id="21e89-114">Další informace o generování a přenos klíč chráněný HSM pomocí přes hello Internetu:</span><span class="sxs-lookup"><span data-stu-id="21e89-114">More information about generating and transferring an HSM-protected key over hello Internet:</span></span>

* <span data-ttu-id="21e89-115">Vygenerování klíče hello z offline pracovní stanici, což snižuje prostor pro útoky hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-115">You generate hello key from an offline workstation, which reduces hello attack surface.</span></span>
* <span data-ttu-id="21e89-116">Hello je klíč zašifrovaný pomocí klíč výměny klíčů (KEK), který zůstává zašifrovaný, dokud nebude přenášená toohello Azure Key Vault moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="21e89-116">hello key is encrypted with a Key Exchange Key (KEK), which stays encrypted until it is transferred toohello Azure Key Vault HSMs.</span></span> <span data-ttu-id="21e89-117">Pouze hello zašifrovaná verze vašeho klíče ponechá původní pracovní stanici hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-117">Only hello encrypted version of your key leaves hello original workstation.</span></span>
* <span data-ttu-id="21e89-118">Sada nástrojů Hello nastaví vlastnosti pro klíč klienta, která se sváže vaše klíče toohello Azure Key Vault architektury security world.</span><span class="sxs-lookup"><span data-stu-id="21e89-118">hello toolset sets properties on your tenant key that binds your key toohello Azure Key Vault security world.</span></span> <span data-ttu-id="21e89-119">Proto po hello Azure Key Vault moduly hardwarového zabezpečení přijmou a dešifrují váš klíč, pouze tyto moduly hardwarového zabezpečení můžete použít.</span><span class="sxs-lookup"><span data-stu-id="21e89-119">So after hello Azure Key Vault HSMs receive and decrypt your key, only these HSMs can use it.</span></span> <span data-ttu-id="21e89-120">Klíč se nedá exportovat.</span><span class="sxs-lookup"><span data-stu-id="21e89-120">Your key cannot be exported.</span></span> <span data-ttu-id="21e89-121">Tuto vazbu vynucují moduly HSM Thales hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-121">This binding is enforced by hello Thales HSMs.</span></span>
* <span data-ttu-id="21e89-122">Hello klíč výměny klíčů (KEK), je použít tooencrypt váš klíč se generuje uvnitř hello Azure Key Vault moduly hardwarového zabezpečení a není exportovatelný.</span><span class="sxs-lookup"><span data-stu-id="21e89-122">hello Key Exchange Key (KEK) that is used tooencrypt your key is generated inside hello Azure Key Vault HSMs and is not exportable.</span></span> <span data-ttu-id="21e89-123">Moduly hardwarového zabezpečení Hello vynutit, aby může existovat žádná čitelná verze hello KEK mimo hello moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="21e89-123">hello HSMs enforce that there can be no clear version of hello KEK outside hello HSMs.</span></span> <span data-ttu-id="21e89-124">Kromě toho hello nástrojů obsahuje záruku od společnosti Thales, že hello KEK nedá exportovat a byl vygenerovaný v originálním modulu HSM vyrobeným společností Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-124">In addition, hello toolset includes attestation from Thales that hello KEK is not exportable and was generated inside a genuine HSM that was manufactured by Thales.</span></span>
* <span data-ttu-id="21e89-125">Hello sada nástrojů obsahuje záruku od společnosti Thales, že hello Azure Key Vault architektury security world také vygenerovaná na originálním modulu HSM vyrobeném společností Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-125">hello toolset includes attestation from Thales that hello Azure Key Vault security world was also generated on a genuine HSM manufactured by Thales.</span></span> <span data-ttu-id="21e89-126">Toto ověření prokáže, že Microsoft používá originální hardware tooyou.</span><span class="sxs-lookup"><span data-stu-id="21e89-126">This attestation proves tooyou that Microsoft is using genuine hardware.</span></span>
* <span data-ttu-id="21e89-127">Společnost Microsoft používá jiné klíče Kek a jiné architektury Security World v každé geografické oblasti.</span><span class="sxs-lookup"><span data-stu-id="21e89-127">Microsoft uses separate KEKs and separate Security Worlds in each geographical region.</span></span> <span data-ttu-id="21e89-128">Toto oddělení zajišťuje, že váš klíč lze použít pouze v datových centrech v hello oblast, ve kterém jste ho zašifrovali.</span><span class="sxs-lookup"><span data-stu-id="21e89-128">This separation ensures that your key can be used only in data centers in hello region in which you encrypted it.</span></span> <span data-ttu-id="21e89-129">Například klíč od Evropského zákazníka nelze použít v datových centrech v Severní Americe nebo Asii.</span><span class="sxs-lookup"><span data-stu-id="21e89-129">For example, a key from a European customer cannot be used in data centers in North American or Asia.</span></span>

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a><span data-ttu-id="21e89-130">Další informace o modulech HSM Thales a služby Microsoft</span><span class="sxs-lookup"><span data-stu-id="21e89-130">More information about Thales HSMs and Microsoft services</span></span>
<span data-ttu-id="21e89-131">Thales e-Security je přední globální poskytovatel šifrování dat a kybernetického zabezpečení řešení toohello finančních služeb, špičkové technologie, výroby, government a technologie.</span><span class="sxs-lookup"><span data-stu-id="21e89-131">Thales e-Security is a leading global provider of data encryption and cyber security solutions toohello financial services, high technology, manufacturing, government, and technology sectors.</span></span> <span data-ttu-id="21e89-132">S 40-let chrání firemní i vládní informace řešení společnosti Thales využívají je čtyři hello pěti největších energetických a letecký společností.</span><span class="sxs-lookup"><span data-stu-id="21e89-132">With a 40-year track record of protecting corporate and government information, Thales solutions are used by four of hello five largest energy and aerospace companies.</span></span> <span data-ttu-id="21e89-133">Svá řešení jsou také používány 22 členských zemí NATO a zabezpečení více než 80 % po celém světě platebních transakcí.</span><span class="sxs-lookup"><span data-stu-id="21e89-133">Their solutions are also used by 22 NATO countries, and secure more than 80 per cent of worldwide payment transactions.</span></span>

<span data-ttu-id="21e89-134">Microsoft spolupracoval se stavem hello tooenhance Thales obrázky pro moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="21e89-134">Microsoft has collaborated with Thales tooenhance hello state of art for HSMs.</span></span> <span data-ttu-id="21e89-135">Tato vylepšení umožňují tooget hello typické výhod hostované služby bez vzdát kontrolu nad klíče.</span><span class="sxs-lookup"><span data-stu-id="21e89-135">These enhancements enable you tooget hello typical benefits of hosted services without relinquishing control over your keys.</span></span> <span data-ttu-id="21e89-136">Konkrétně tato vylepšení umožňují Microsoftu spravovat hello moduly hardwarového zabezpečení, takže není nutné.</span><span class="sxs-lookup"><span data-stu-id="21e89-136">Specifically, these enhancements let Microsoft manage hello HSMs so that you do not have to.</span></span> <span data-ttu-id="21e89-137">Jako cloudové služby, Azure Key Vault škálování na krátké oznámení toomeet špičky využití vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="21e89-137">As a cloud service, Azure Key Vault scales up at short notice toomeet your organization’s usage spikes.</span></span> <span data-ttu-id="21e89-138">Na hello stejný čas, váš klíč je uvnitř modulů HSM Microsoftu chráněný: ponecháte si kontrolu nad hello životního cyklu u klíče, protože generování klíče hello a přenést na tooMicrosoft moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="21e89-138">At hello same time, your key is protected inside Microsoft’s HSMs: You retain control over hello key lifecycle because you generate hello key and transfer it tooMicrosoft’s HSMs.</span></span>

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a><span data-ttu-id="21e89-139">Implementace funkce přineste si vlastní klíč (BYOK) pro Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="21e89-139">Implementing bring your own key (BYOK) for Azure Key Vault</span></span>
<span data-ttu-id="21e89-140">Hello použijte následující informace a postupy, pokud chcete vygenerovat si vlastní klíč chráněný HSM a pak ho přenést tooAzure Key Vault – hello přineste si vlastní klíč (BYOK) scénář.</span><span class="sxs-lookup"><span data-stu-id="21e89-140">Use hello following information and procedures if you will generate your own HSM-protected key and then transfer it tooAzure Key Vault—hello bring your own key (BYOK) scenario.</span></span>

## <a name="prerequisites-for-byok"></a><span data-ttu-id="21e89-141">Předpoklady pro funkci BYOK</span><span class="sxs-lookup"><span data-stu-id="21e89-141">Prerequisites for BYOK</span></span>
<span data-ttu-id="21e89-142">Viz následující tabulka obsahuje seznam požadavků pro hello přineste si vlastní klíč (BYOK) pro Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="21e89-142">See hello following table for a list of prerequisites for bring your own key (BYOK) for Azure Key Vault.</span></span>

| <span data-ttu-id="21e89-143">Požadavek</span><span class="sxs-lookup"><span data-stu-id="21e89-143">Requirement</span></span> | <span data-ttu-id="21e89-144">Další informace</span><span class="sxs-lookup"><span data-stu-id="21e89-144">More information</span></span> |
| --- | --- |
| <span data-ttu-id="21e89-145">TooAzure předplatného</span><span class="sxs-lookup"><span data-stu-id="21e89-145">A subscription tooAzure</span></span> |<span data-ttu-id="21e89-146">toocreate Azure Key Vault, budete potřebovat předplatné Azure: [zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="21e89-146">toocreate an Azure Key Vault, you need an Azure subscription: [Sign up for free trial](https://azure.microsoft.com/pricing/free-trial/)</span></span> |
| <span data-ttu-id="21e89-147">Hello Azure Key Vault Premium služby vrstvě toosupport klíčů chráněných pomocí HSM</span><span class="sxs-lookup"><span data-stu-id="21e89-147">hello Azure Key Vault Premium service tier toosupport HSM-protected keys</span></span> |<span data-ttu-id="21e89-148">Další informace o úrovních služeb hello a možnosti pro Azure Key Vault najdete v tématu hello [Azure Key Vault ceny](https://azure.microsoft.com/pricing/details/key-vault/) webu.</span><span class="sxs-lookup"><span data-stu-id="21e89-148">For more information about hello service tiers and capabilities for Azure Key Vault, see hello [Azure Key Vault Pricing](https://azure.microsoft.com/pricing/details/key-vault/) website.</span></span> |
| <span data-ttu-id="21e89-149">Modulu HSM společnosti Thales, čipové karty a podpůrný software</span><span class="sxs-lookup"><span data-stu-id="21e89-149">Thales HSM, smartcards, and support software</span></span> |<span data-ttu-id="21e89-150">Musíte mít přístup k tooa modulu hardwarového zabezpečení Thales a základní provozní znalosti o modulech HSM Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-150">You must have access tooa Thales Hardware Security Module and basic operational knowledge of Thales HSMs.</span></span> <span data-ttu-id="21e89-151">V tématu [modulu hardwarového zabezpečení Thales](https://www.thales-esecurity.com/msrms/buy) hello seznam kompatibilních modelů nebo toopurchase modul hardwarového zabezpečení, pokud nemáte jeden.</span><span class="sxs-lookup"><span data-stu-id="21e89-151">See [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) for hello list of compatible models, or toopurchase an HSM if you do not have one.</span></span> |
| <span data-ttu-id="21e89-152">Hello následující hardware a software:</span><span class="sxs-lookup"><span data-stu-id="21e89-152">hello following hardware and software:</span></span><ol><li><span data-ttu-id="21e89-153">Offline x64 pracovní stanice s minimální operační systém Windows Windows 7 a Thales software nshield od, který je minimálně verze 11.50.</span><span class="sxs-lookup"><span data-stu-id="21e89-153">An offline x64 workstation with a minimum Windows operation system of Windows 7 and Thales nShield software that is at least version 11.50.</span></span><br/><br/><span data-ttu-id="21e89-154">Pokud tato pracovní stanice používá Windows 7, musíte [instalace rozhraní Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span><span class="sxs-lookup"><span data-stu-id="21e89-154">If this workstation runs Windows 7, you must [install Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span></span></li><li><span data-ttu-id="21e89-155">Pracovní stanice, která je připojená toohello Internet a má minimální operační systém Windows Windows 7 a [prostředí Azure PowerShell](/powershell/azure/overview) **minimální verzi 1.1.0** nainstalována.</span><span class="sxs-lookup"><span data-stu-id="21e89-155">A workstation that is connected toohello Internet and has a minimum Windows operating system of Windows 7 and [Azure PowerShell](/powershell/azure/overview) **minimum version 1.1.0** installed.</span></span></li><li><span data-ttu-id="21e89-156">USB Flash disk nebo jiné přenosné úložné zařízení, která obsahuje aspoň 16 MB volného místa.</span><span class="sxs-lookup"><span data-stu-id="21e89-156">A USB drive or other portable storage device that has at least 16 MB free space.</span></span></li></ol> |<span data-ttu-id="21e89-157">Z bezpečnostních důvodů doporučujeme, abyste že tento hello první pracovní stanice není připojený tooa sítí.</span><span class="sxs-lookup"><span data-stu-id="21e89-157">For security reasons, we recommend that hello first workstation is not connected tooa network.</span></span> <span data-ttu-id="21e89-158">Toto doporučení se však nevynucuje prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="21e89-158">However, this recommendation is not programmatically enforced.</span></span><br/><br/><span data-ttu-id="21e89-159">Všimněte si, že v hello pokynech, této pracovní stanici odkazované tooas hello odpojená pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="21e89-159">Note that in hello instructions that follow, this workstation is referred tooas hello disconnected workstation.</span></span></p></blockquote><br/><span data-ttu-id="21e89-160">Kromě toho pokud váš klíč tenanta je pro produkční síť, doporučujeme používat druhou, samostatnou pracovní stanici toodownload hello nástrojů a nahrání hello klíč tenanta.</span><span class="sxs-lookup"><span data-stu-id="21e89-160">In addition, if your tenant key is for a production network, we recommend that you use a second, separate workstation toodownload hello toolset and upload hello tenant key.</span></span> <span data-ttu-id="21e89-161">Ale pro účely testování můžete použít jako hello první hello pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="21e89-161">But for testing purposes, you can use hello same workstation as hello first one.</span></span><br/><br/><span data-ttu-id="21e89-162">Upozorňujeme, že v hello pokynech se druhá pracovní stanice se stanice připojené k Internetu odkazované tooas hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-162">Note that in hello instructions that follow, this second workstation is referred tooas hello Internet-connected workstation.</span></span></p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-tooazure-key-vault-hsm"></a><span data-ttu-id="21e89-163">Generování a přenos vašeho klíče tooAzure klíč trezoru modulu hardwarového zabezpečení</span><span class="sxs-lookup"><span data-stu-id="21e89-163">Generate and transfer your key tooAzure Key Vault HSM</span></span>
<span data-ttu-id="21e89-164">Budete používat hello následujících pět toogenerate kroky a přenos vašeho klíče tooan Azure Key Vault modulu hardwarového zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="21e89-164">You will use hello following five steps toogenerate and transfer your key tooan Azure Key Vault HSM:</span></span>

* [<span data-ttu-id="21e89-165">Krok 1: Příprava pracovní stanice připojené k Internetu</span><span class="sxs-lookup"><span data-stu-id="21e89-165">Step 1: Prepare your Internet-connected workstation</span></span>](#step-1-prepare-your-internet-connected-workstation)
* [<span data-ttu-id="21e89-166">Krok 2: Příprava odpojené pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="21e89-166">Step 2: Prepare your disconnected workstation</span></span>](#step-2-prepare-your-disconnected-workstation)
* [<span data-ttu-id="21e89-167">Krok 3: Vygenerování klíče</span><span class="sxs-lookup"><span data-stu-id="21e89-167">Step 3: Generate your key</span></span>](#step-3-generate-your-key)
* [<span data-ttu-id="21e89-168">Krok 4: Příprava klíče pro přenos</span><span class="sxs-lookup"><span data-stu-id="21e89-168">Step 4: Prepare your key for transfer</span></span>](#step-4-prepare-your-key-for-transfer)
* [<span data-ttu-id="21e89-169">Krok 5: Přenos vašeho klíče tooAzure Key Vault</span><span class="sxs-lookup"><span data-stu-id="21e89-169">Step 5: Transfer your key tooAzure Key Vault</span></span>](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a><span data-ttu-id="21e89-170">Krok 1: Příprava pracovní stanice připojené k Internetu</span><span class="sxs-lookup"><span data-stu-id="21e89-170">Step 1: Prepare your Internet-connected workstation</span></span>
<span data-ttu-id="21e89-171">Pro tento první krok text hello následující postupy na pracovní stanici, která je připojená toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="21e89-171">For this first step, do hello following procedures on your workstation that is connected toohello Internet.</span></span>

### <a name="step-11-install-azure-powershell"></a><span data-ttu-id="21e89-172">Krok 1.1: Nainstalování prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="21e89-172">Step 1.1: Install Azure PowerShell</span></span>
<span data-ttu-id="21e89-173">Z hello stanice připojené k Internetu stáhněte a nainstalujte modul Azure PowerShell text hello, který obsahuje toomanage hello rutiny Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="21e89-173">From hello Internet-connected workstation, download and install hello Azure PowerShell module that includes hello cmdlets toomanage Azure Key Vault.</span></span> <span data-ttu-id="21e89-174">To vyžaduje minimální verzi 0.8.13.</span><span class="sxs-lookup"><span data-stu-id="21e89-174">This requires a minimum version of 0.8.13.</span></span>

<span data-ttu-id="21e89-175">Pokyny k instalaci naleznete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="21e89-175">For installation instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="step-12-get-your-azure-subscription-id"></a><span data-ttu-id="21e89-176">Krok 1.2: Získání ID předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="21e89-176">Step 1.2: Get your Azure subscription ID</span></span>
<span data-ttu-id="21e89-177">Spusťte relaci prostředí Azure PowerShell a přihlaste se pomocí hello následující příkaz tooyour účet Azure:</span><span class="sxs-lookup"><span data-stu-id="21e89-177">Start an Azure PowerShell session and sign in tooyour Azure account by using hello following command:</span></span>

        Add-AzureAccount
<span data-ttu-id="21e89-178">V okně hello automaticky otevírané okno prohlížeče zadejte účet Azure uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="21e89-178">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="21e89-179">Poté použijte hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) příkaz:</span><span class="sxs-lookup"><span data-stu-id="21e89-179">Then, use hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) command:</span></span>

        Get-AzureSubscription
<span data-ttu-id="21e89-180">Z výstupu hello vyhledejte ID hello hello předplatného, které chcete použít pro Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="21e89-180">From hello output, locate hello ID for hello subscription you will use for Azure Key Vault.</span></span> <span data-ttu-id="21e89-181">Toto ID předplatného budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="21e89-181">You will need this subscription ID later.</span></span>

<span data-ttu-id="21e89-182">Nezavírejte okno Azure PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-182">Do not close hello Azure PowerShell window.</span></span>

### <a name="step-13-download-hello-byok-toolset-for-azure-key-vault"></a><span data-ttu-id="21e89-183">Krok 1.3: Stažení sady nástrojů funkce BYOK hello pro Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="21e89-183">Step 1.3: Download hello BYOK toolset for Azure Key Vault</span></span>
<span data-ttu-id="21e89-184">Přejděte toohello Microsoft Download Center a [stažení nástrojů Azure Key Vault BYOK hello](http://www.microsoft.com/download/details.aspx?id=45345) pro zeměpisné oblasti nebo instanci Azure.</span><span class="sxs-lookup"><span data-stu-id="21e89-184">Go toohello Microsoft Download Center and [download hello Azure Key Vault BYOK toolset](http://www.microsoft.com/download/details.aspx?id=45345) for your geographic region or instance of Azure.</span></span> <span data-ttu-id="21e89-185">Použijte hello informace tooidentify hello balíčku název toodownload a jeho odpovídající SHA-256 balíčku hash:</span><span class="sxs-lookup"><span data-stu-id="21e89-185">Use hello following information tooidentify hello package name toodownload and its corresponding SHA-256 package hash:</span></span>

- - -
<span data-ttu-id="21e89-186">**Spojené státy americké:**</span><span class="sxs-lookup"><span data-stu-id="21e89-186">**United States:**</span></span>

<span data-ttu-id="21e89-187">KeyVault-BYOK-nástroje spojené States.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-187">KeyVault-BYOK-Tools-UnitedStates.zip</span></span>

<span data-ttu-id="21e89-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span><span class="sxs-lookup"><span data-stu-id="21e89-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span></span>

- - -
<span data-ttu-id="21e89-189">**Evropa:**</span><span class="sxs-lookup"><span data-stu-id="21e89-189">**Europe:**</span></span>

<span data-ttu-id="21e89-190">KeyVault BYOK nástroje Europe.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-190">KeyVault-BYOK-Tools-Europe.zip</span></span>

<span data-ttu-id="21e89-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span><span class="sxs-lookup"><span data-stu-id="21e89-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span></span>

- - -
<span data-ttu-id="21e89-192">**Asii:**</span><span class="sxs-lookup"><span data-stu-id="21e89-192">**Asia:**</span></span>

<span data-ttu-id="21e89-193">KeyVault BYOK nástroje AsiaPacific.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-193">KeyVault-BYOK-Tools-AsiaPacific.zip</span></span>

<span data-ttu-id="21e89-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span><span class="sxs-lookup"><span data-stu-id="21e89-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span></span>

- - -
<span data-ttu-id="21e89-195">**Latinská Amerika:**</span><span class="sxs-lookup"><span data-stu-id="21e89-195">**Latin America:**</span></span>

<span data-ttu-id="21e89-196">KeyVault BYOK nástroje LatinAmerica.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-196">KeyVault-BYOK-Tools-LatinAmerica.zip</span></span>

<span data-ttu-id="21e89-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span><span class="sxs-lookup"><span data-stu-id="21e89-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span></span>

- - -
<span data-ttu-id="21e89-198">**Japonsko:**</span><span class="sxs-lookup"><span data-stu-id="21e89-198">**Japan:**</span></span>

<span data-ttu-id="21e89-199">KeyVault BYOK nástroje Japan.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-199">KeyVault-BYOK-Tools-Japan.zip</span></span>

<span data-ttu-id="21e89-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span><span class="sxs-lookup"><span data-stu-id="21e89-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span></span>

- - -
<span data-ttu-id="21e89-201">**Korea:**</span><span class="sxs-lookup"><span data-stu-id="21e89-201">**Korea:**</span></span>

<span data-ttu-id="21e89-202">KeyVault BYOK nástroje Korea.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-202">KeyVault-BYOK-Tools-Korea.zip</span></span>

<span data-ttu-id="21e89-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span><span class="sxs-lookup"><span data-stu-id="21e89-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span></span>

- - -
<span data-ttu-id="21e89-204">**Austrálie:**</span><span class="sxs-lookup"><span data-stu-id="21e89-204">**Australia:**</span></span>

<span data-ttu-id="21e89-205">KeyVault BYOK nástroje Australia.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-205">KeyVault-BYOK-Tools-Australia.zip</span></span>

<span data-ttu-id="21e89-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span><span class="sxs-lookup"><span data-stu-id="21e89-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span></span>

- - -
[<span data-ttu-id="21e89-207">**Azure Government:**</span><span class="sxs-lookup"><span data-stu-id="21e89-207">**Azure Government:**</span></span>](https://azure.microsoft.com/features/gov/)

<span data-ttu-id="21e89-208">KeyVault BYOK nástroje USGovCloud.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-208">KeyVault-BYOK-Tools-USGovCloud.zip</span></span>

<span data-ttu-id="21e89-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span><span class="sxs-lookup"><span data-stu-id="21e89-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span></span>

- - -
<span data-ttu-id="21e89-210">**Vláda USA DOD:**</span><span class="sxs-lookup"><span data-stu-id="21e89-210">**US Government DOD:**</span></span>

<span data-ttu-id="21e89-211">KeyVault BYOK nástroje USGovernmentDoD.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-211">KeyVault-BYOK-Tools-USGovernmentDoD.zip</span></span>

<span data-ttu-id="21e89-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span><span class="sxs-lookup"><span data-stu-id="21e89-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span></span>

- - -
<span data-ttu-id="21e89-213">**Kanada:**</span><span class="sxs-lookup"><span data-stu-id="21e89-213">**Canada:**</span></span>

<span data-ttu-id="21e89-214">KeyVault BYOK nástroje Canada.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-214">KeyVault-BYOK-Tools-Canada.zip</span></span>

<span data-ttu-id="21e89-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span><span class="sxs-lookup"><span data-stu-id="21e89-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span></span>

- - -
<span data-ttu-id="21e89-216">**Německo:**</span><span class="sxs-lookup"><span data-stu-id="21e89-216">**Germany:**</span></span>

<span data-ttu-id="21e89-217">KeyVault BYOK nástroje Germany.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-217">KeyVault-BYOK-Tools-Germany.zip</span></span>

<span data-ttu-id="21e89-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span><span class="sxs-lookup"><span data-stu-id="21e89-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span></span>

- - -
<span data-ttu-id="21e89-219">**Indie:**</span><span class="sxs-lookup"><span data-stu-id="21e89-219">**India:**</span></span>

<span data-ttu-id="21e89-220">KeyVault BYOK nástroje India.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-220">KeyVault-BYOK-Tools-India.zip</span></span>

<span data-ttu-id="21e89-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span><span class="sxs-lookup"><span data-stu-id="21e89-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span></span>

- - -
<span data-ttu-id="21e89-222">**Spojené království:**</span><span class="sxs-lookup"><span data-stu-id="21e89-222">**United Kingdom:**</span></span>

<span data-ttu-id="21e89-223">KeyVault BYOK nástroje UnitedKingdom.zip</span><span class="sxs-lookup"><span data-stu-id="21e89-223">KeyVault-BYOK-Tools-UnitedKingdom.zip</span></span>

<span data-ttu-id="21e89-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span><span class="sxs-lookup"><span data-stu-id="21e89-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span></span>

- - -

<span data-ttu-id="21e89-225">toovalidate hello integritu vašeho stažené sady nástrojů funkce BYOK, z relace prostředí Azure PowerShell, použijte hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="21e89-225">toovalidate hello integrity of your downloaded BYOK toolset, from your Azure PowerShell session, use hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span></span>

    Get-FileHash KeyVault-BYOK-Tools-*.zip

<span data-ttu-id="21e89-226">Sada nástrojů Hello zahrnuje hello následující:</span><span class="sxs-lookup"><span data-stu-id="21e89-226">hello toolset includes hello following:</span></span>

* <span data-ttu-id="21e89-227">Balíček klíče výměny klíčů (KEK), který má název začíná **BYOK-KEK - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="21e89-227">A Key Exchange Key (KEK) package that has a name beginning with **BYOK-KEK-pkg-.**</span></span>
* <span data-ttu-id="21e89-228">Balíček architektury Security World, jehož název začíná **BYOK-SecurityWorld - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="21e89-228">A Security World package that has a name beginning with **BYOK-SecurityWorld-pkg-.**</span></span>
* <span data-ttu-id="21e89-229">Skript v jazyce python s názvem **verifykeypackage.py.**</span><span class="sxs-lookup"><span data-stu-id="21e89-229">A python script named **verifykeypackage.py.**</span></span>
* <span data-ttu-id="21e89-230">Spustitelný soubor příkazového řádku, s názvem **KeyTransferRemote.exe** a přidružené knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="21e89-230">A command-line executable file named **KeyTransferRemote.exe** and associated DLLs.</span></span>
* <span data-ttu-id="21e89-231">Visual C++ Redistributable balíčku, s názvem **vcredist_x64.exe.**</span><span class="sxs-lookup"><span data-stu-id="21e89-231">A Visual C++ Redistributable Package, named **vcredist_x64.exe.**</span></span>

<span data-ttu-id="21e89-232">Zkopírujte hello balíček tooa USB Flash disku nebo jiného přenosného úložiště.</span><span class="sxs-lookup"><span data-stu-id="21e89-232">Copy hello package tooa USB drive or other portable storage.</span></span>

## <a name="step-2-prepare-your-disconnected-workstation"></a><span data-ttu-id="21e89-233">Krok 2: Příprava odpojené pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="21e89-233">Step 2: Prepare your disconnected workstation</span></span>
<span data-ttu-id="21e89-234">Pro tento druhý krok text hello následující postupy na hello pracovní stanici, která není připojená tooa sítí (hello Internet nebo interní síti).</span><span class="sxs-lookup"><span data-stu-id="21e89-234">For this second step, do hello following procedures on hello workstation that is not connected tooa network (either hello Internet or your internal network).</span></span>

### <a name="step-21-prepare-hello-disconnected-workstation-with-thales-hsm"></a><span data-ttu-id="21e89-235">Krok 2.1: Příprava hello odpojená pracovní stanice s modulu HSM společnosti Thales</span><span class="sxs-lookup"><span data-stu-id="21e89-235">Step 2.1: Prepare hello disconnected workstation with Thales HSM</span></span>
<span data-ttu-id="21e89-236">Nainstalujte na počítač s Windows podpůrný software nCipher (Thales) hello a pak připojte počítač toothat modulu HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-236">Install hello nCipher (Thales) support software on a Windows computer, and then attach a Thales HSM toothat computer.</span></span>

<span data-ttu-id="21e89-237">Ujistěte se, že hello nástroje Thales jsou ve své cestě (**%nfast_home%\bin**).</span><span class="sxs-lookup"><span data-stu-id="21e89-237">Ensure that hello Thales tools are in your path (**%nfast_home%\bin**).</span></span> <span data-ttu-id="21e89-238">Můžete například zadáte následující hello:</span><span class="sxs-lookup"><span data-stu-id="21e89-238">For example, type hello following:</span></span>

        set PATH=%PATH%;"%nfast_home%\bin"

<span data-ttu-id="21e89-239">Další informace najdete v tématu hello uživatelské příručce dodané s hello modulu HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-239">For more information, see hello user guide included with hello Thales HSM.</span></span>

### <a name="step-22-install-hello-byok-toolset-on-hello-disconnected-workstation"></a><span data-ttu-id="21e89-240">Krok 2.2: Instalace hello sady nástrojů funkce BYOK na pracovní stanici hello odpojení</span><span class="sxs-lookup"><span data-stu-id="21e89-240">Step 2.2: Install hello BYOK toolset on hello disconnected workstation</span></span>
<span data-ttu-id="21e89-241">Zkopírujte balíček sady nástrojů funkce BYOK hello z hello USB Flash disku nebo jiného přenosného úložiště a pak hello následující:</span><span class="sxs-lookup"><span data-stu-id="21e89-241">Copy hello BYOK toolset package from hello USB drive or other portable storage, and then do hello following:</span></span>

1. <span data-ttu-id="21e89-242">Extrahujte soubory hello z hello stáhli balíčku do libovolné složky.</span><span class="sxs-lookup"><span data-stu-id="21e89-242">Extract hello files from hello downloaded package into any folder.</span></span>
2. <span data-ttu-id="21e89-243">Z této složky spusťte program vcredist_x64.exe.</span><span class="sxs-lookup"><span data-stu-id="21e89-243">From that folder, run vcredist_x64.exe.</span></span>
3. <span data-ttu-id="21e89-244">Postupujte podle pokynů hello toohello nainstalovat hello komponenty modulu runtime Visual C++ pro Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="21e89-244">Follow hello instructions toohello install hello Visual C++ runtime components for Visual Studio 2013.</span></span>

## <a name="step-3-generate-your-key"></a><span data-ttu-id="21e89-245">Krok 3: Vygenerování klíče</span><span class="sxs-lookup"><span data-stu-id="21e89-245">Step 3: Generate your key</span></span>
<span data-ttu-id="21e89-246">Pro tento třetí krok text hello následující postupy na pracovní stanici hello odpojen.</span><span class="sxs-lookup"><span data-stu-id="21e89-246">For this third step, do hello following procedures on hello disconnected workstation.</span></span> <span data-ttu-id="21e89-247">toocomplete tento krok vašeho HSM musí být v režimu inicializace.</span><span class="sxs-lookup"><span data-stu-id="21e89-247">toocomplete this step your HSM must be in initialization mode.</span></span> 


### <a name="step-31-change-hello-hsm-mode-tooi"></a><span data-ttu-id="21e89-248">Krok 3.1: Změnit too'I režimu hello HSM.</span><span class="sxs-lookup"><span data-stu-id="21e89-248">Step 3.1: Change hello HSM mode too'I'</span></span>
<span data-ttu-id="21e89-249">Pokud používáte Thales nShield okraji a toochange hello režim: 1.</span><span class="sxs-lookup"><span data-stu-id="21e89-249">If you are using Thales nShield Edge, toochange hello mode: 1.</span></span> <span data-ttu-id="21e89-250">Použijte hello režimu tlačítko toohighlight hello požadovaný režim.</span><span class="sxs-lookup"><span data-stu-id="21e89-250">Use hello Mode button toohighlight hello required mode.</span></span> <span data-ttu-id="21e89-251">2.</span><span class="sxs-lookup"><span data-stu-id="21e89-251">2.</span></span> <span data-ttu-id="21e89-252">Během pár sekund stiskněte a podržte tlačítko Vymazat hello z několika sekund.</span><span class="sxs-lookup"><span data-stu-id="21e89-252">Within a few seconds, press and hold hello Clear button for a couple of seconds.</span></span> <span data-ttu-id="21e89-253">Pokud se změní hello režimu, hello nový režim DIODU přestane blikat a zůstane po.</span><span class="sxs-lookup"><span data-stu-id="21e89-253">If hello mode changes, hello new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="21e89-254">Hello Indikátor stavu může nepravidelně flash na několik sekund a pak bliká pravidelně při hello zařízení je připraveno.</span><span class="sxs-lookup"><span data-stu-id="21e89-254">hello Status LED might flash irregularly for a few seconds and then flashes regularly when hello device is ready.</span></span> <span data-ttu-id="21e89-255">V opačném lit hello zařízení zůstanou v aktuálním režimu hello s Indikátor příslušné režimu hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-255">Otherwise, hello device remains in hello current mode, with hello appropriate mode LED lit.</span></span>

### <a name="step-32-create-a-security-world"></a><span data-ttu-id="21e89-256">Krok 3.2: Vytvoření architektury security world</span><span class="sxs-lookup"><span data-stu-id="21e89-256">Step 3.2: Create a security world</span></span>
<span data-ttu-id="21e89-257">Otevřete příkazový řádek a spusťte program společnosti Thales nové architektury Security world hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-257">Start a command prompt and run hello Thales new-world program.</span></span>

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

<span data-ttu-id="21e89-258">Tento program vytvoří **architektury Security World** soubor v adresáři % NFAST_KMDATA%\local\world, který odpovídá toohello C:\ProgramData\nCipher\Key správu aplikací\Místní složky.</span><span class="sxs-lookup"><span data-stu-id="21e89-258">This program creates a **Security World** file at %NFAST_KMDATA%\local\world, which corresponds toohello C:\ProgramData\nCipher\Key Management Data\local folder.</span></span> <span data-ttu-id="21e89-259">Můžete použít jiné hodnoty pro hello kvorum, ale v našem příkladu jste výzvami tooenter tři prázdné karty a kódy PIN pro každé z nich.</span><span class="sxs-lookup"><span data-stu-id="21e89-259">You can use different values for hello quorum but in our example, you’re prompted tooenter three blank cards and pins for each one.</span></span> <span data-ttu-id="21e89-260">Jakékoli dvě karty potom poskytnout úplný přístup toohello security world.</span><span class="sxs-lookup"><span data-stu-id="21e89-260">Then, any two cards give full access toohello security world.</span></span> <span data-ttu-id="21e89-261">Tyto karty tvoří hello **Administrator Card Set** pro hello, world nové zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="21e89-261">These cards become hello **Administrator Card Set** for hello new security world.</span></span>

<span data-ttu-id="21e89-262">Potom hello následující:</span><span class="sxs-lookup"><span data-stu-id="21e89-262">Then do hello following:</span></span>

* <span data-ttu-id="21e89-263">Zálohujte soubor hello world.</span><span class="sxs-lookup"><span data-stu-id="21e89-263">Back up hello world file.</span></span> <span data-ttu-id="21e89-264">Zabezpečení a ochraně soubor hello world, karty Správce hello a kódu PIN a ujistěte se, že jeden člověk měl přístup toomore než jedné karty.</span><span class="sxs-lookup"><span data-stu-id="21e89-264">Secure and protect hello world file, hello Administrator Cards, and their pins, and make sure that no single person has access toomore than one card.</span></span>

### <a name="step-33-change-hello-hsm-mode-tooo"></a><span data-ttu-id="21e89-265">Krok 3.3: Změnit too'O režimu hello HSM.</span><span class="sxs-lookup"><span data-stu-id="21e89-265">Step 3.3: Change hello HSM mode too'O'</span></span>
<span data-ttu-id="21e89-266">Pokud používáte Thales nShield okraji a toochange hello režim: 1.</span><span class="sxs-lookup"><span data-stu-id="21e89-266">If you are using Thales nShield Edge, toochange hello mode: 1.</span></span> <span data-ttu-id="21e89-267">Použijte hello režimu tlačítko toohighlight hello požadovaný režim.</span><span class="sxs-lookup"><span data-stu-id="21e89-267">Use hello Mode button toohighlight hello required mode.</span></span> <span data-ttu-id="21e89-268">2.</span><span class="sxs-lookup"><span data-stu-id="21e89-268">2.</span></span> <span data-ttu-id="21e89-269">Během pár sekund stiskněte a podržte tlačítko Vymazat hello z několika sekund.</span><span class="sxs-lookup"><span data-stu-id="21e89-269">Within a few seconds, press and hold hello Clear button for a couple of seconds.</span></span> <span data-ttu-id="21e89-270">Pokud se změní hello režimu, hello nový režim DIODU přestane blikat a zůstane po.</span><span class="sxs-lookup"><span data-stu-id="21e89-270">If hello mode changes, hello new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="21e89-271">Hello Indikátor stavu může nepravidelně flash na několik sekund a pak bliká pravidelně při hello zařízení je připraveno.</span><span class="sxs-lookup"><span data-stu-id="21e89-271">hello Status LED might flash irregularly for a few seconds and then flashes regularly when hello device is ready.</span></span> <span data-ttu-id="21e89-272">V opačném lit hello zařízení zůstanou v aktuálním režimu hello s Indikátor příslušné režimu hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-272">Otherwise, hello device remains in hello current mode, with hello appropriate mode LED lit.</span></span>


### <a name="step-34-validate-hello-downloaded-package"></a><span data-ttu-id="21e89-273">Krok 3.4: Ověření hello Stáhnout balíček</span><span class="sxs-lookup"><span data-stu-id="21e89-273">Step 3.4: Validate hello downloaded package</span></span>
<span data-ttu-id="21e89-274">Tento krok je volitelný, ale doporučujeme, aby mohli ověřit hello následující:</span><span class="sxs-lookup"><span data-stu-id="21e89-274">This step is optional but recommended so that you can validate hello following:</span></span>

* <span data-ttu-id="21e89-275">Hello klíč výměny klíče, který je součástí sady nástrojů hello byl vygenerovaný na originálním modulu HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-275">hello Key Exchange Key that is included in hello toolset has been generated from a genuine Thales HSM.</span></span>
* <span data-ttu-id="21e89-276">Hodnota hash Hello hello World zabezpečení, která je součástí sady nástrojů hello byla vygenerovaná na originálním modulu HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-276">hello hash of hello Security World that is included in hello toolset has been generated in a genuine Thales HSM.</span></span>
* <span data-ttu-id="21e89-277">Hello klíč pro výměnu klíčů je neexportovatelného.</span><span class="sxs-lookup"><span data-stu-id="21e89-277">hello Key Exchange Key is non-exportable.</span></span>

> [!NOTE]
> <span data-ttu-id="21e89-278">toovalidate hello stáhli balíček, hello modulu hardwarového zabezpečení musí být připojený, zapnutý a musí mít architektury security world v něm (například hello jeden, který jste právě vytvořili).</span><span class="sxs-lookup"><span data-stu-id="21e89-278">toovalidate hello downloaded package, hello HSM must be connected, powered on, and must have a security world on it (such as hello one you’ve just created).</span></span>
>
>

<span data-ttu-id="21e89-279">toovalidate hello Stáhnout balíček:</span><span class="sxs-lookup"><span data-stu-id="21e89-279">toovalidate hello downloaded package:</span></span>

1. <span data-ttu-id="21e89-280">Spusťte skript verifykeypackage.py hello zadáním jedné z následujících hello, v závislosti na zeměpisné oblasti nebo instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="21e89-280">Run hello verifykeypackage.py script by typing one of hello following, depending on your geographic region or instance of Azure:</span></span>

   * <span data-ttu-id="21e89-281">Pro Severní Ameriku:</span><span class="sxs-lookup"><span data-stu-id="21e89-281">For North America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * <span data-ttu-id="21e89-282">Pro Evropu:</span><span class="sxs-lookup"><span data-stu-id="21e89-282">For Europe:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * <span data-ttu-id="21e89-283">Pro Asii:</span><span class="sxs-lookup"><span data-stu-id="21e89-283">For Asia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * <span data-ttu-id="21e89-284">Pro Latinská Amerika:</span><span class="sxs-lookup"><span data-stu-id="21e89-284">For Latin America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * <span data-ttu-id="21e89-285">Pro Japonsko:</span><span class="sxs-lookup"><span data-stu-id="21e89-285">For Japan:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * <span data-ttu-id="21e89-286">Pro Koreu:</span><span class="sxs-lookup"><span data-stu-id="21e89-286">For Korea:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * <span data-ttu-id="21e89-287">Pro Austrálii:</span><span class="sxs-lookup"><span data-stu-id="21e89-287">For Australia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * <span data-ttu-id="21e89-288">Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá hello US government instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="21e89-288">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * <span data-ttu-id="21e89-289">Pro US Government DOD:</span><span class="sxs-lookup"><span data-stu-id="21e89-289">For US Government DOD:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * <span data-ttu-id="21e89-290">Pro Kanadu:</span><span class="sxs-lookup"><span data-stu-id="21e89-290">For Canada:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * <span data-ttu-id="21e89-291">Pro Německo:</span><span class="sxs-lookup"><span data-stu-id="21e89-291">For Germany:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * <span data-ttu-id="21e89-292">Pro Indii:</span><span class="sxs-lookup"><span data-stu-id="21e89-292">For India:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > <span data-ttu-id="21e89-293">Hello software společnosti Thales obsahuje python v %NFAST_HOME%\python\bin</span><span class="sxs-lookup"><span data-stu-id="21e89-293">hello Thales software includes python at %NFAST_HOME%\python\bin</span></span>
     >
     >
2. <span data-ttu-id="21e89-294">Zkontrolujte, jestli hello následující příkaz, který znamená úspěšné ověření: **výsledek: Úspěch**</span><span class="sxs-lookup"><span data-stu-id="21e89-294">Confirm that you see hello following, which indicates successful validation: **Result: SUCCESS**</span></span>

<span data-ttu-id="21e89-295">Tento skript ověřuje řetězec podepisujících hello až toohello kořenovému klíči Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-295">This script validates hello signer chain up toohello Thales root key.</span></span> <span data-ttu-id="21e89-296">ve skriptu hello vložené Hello hash tohoto kořenového klíče a jeho hodnota by měla být **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span><span class="sxs-lookup"><span data-stu-id="21e89-296">hello hash of this root key is embedded in hello script and its value should be **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span></span> <span data-ttu-id="21e89-297">Tuto hodnotu můžete potvrdit samostatně návštěvou hello [webu společnosti Thales](http://www.thalesesec.com/).</span><span class="sxs-lookup"><span data-stu-id="21e89-297">You can also confirm this value separately by visiting hello [Thales website](http://www.thalesesec.com/).</span></span>

<span data-ttu-id="21e89-298">Nyní jste připravené toocreate nový klíč.</span><span class="sxs-lookup"><span data-stu-id="21e89-298">You’re now ready toocreate a new key.</span></span>

### <a name="step-35-create-a-new-key"></a><span data-ttu-id="21e89-299">3.5 krok: Vytvořte nový klíč</span><span class="sxs-lookup"><span data-stu-id="21e89-299">Step 3.5: Create a new key</span></span>
<span data-ttu-id="21e89-300">Generování klíče pomocí hello Thales **generatekey** program.</span><span class="sxs-lookup"><span data-stu-id="21e89-300">Generate a key by using hello Thales **generatekey** program.</span></span>

<span data-ttu-id="21e89-301">Spusťte následující příkaz toogenerate hello klíč hello:</span><span class="sxs-lookup"><span data-stu-id="21e89-301">Run hello following command toogenerate hello key:</span></span>

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

<span data-ttu-id="21e89-302">Když spustíte tento příkaz, použijte tyto pokyny:</span><span class="sxs-lookup"><span data-stu-id="21e89-302">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="21e89-303">Hello parametr *chránit* musí být nastavena hodnota toohello **modulu**, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="21e89-303">hello parameter *protect* must be set toohello value **module**, as shown.</span></span> <span data-ttu-id="21e89-304">Tím se vytvoří klíč chráněný modulem.</span><span class="sxs-lookup"><span data-stu-id="21e89-304">This creates a module-protected key.</span></span> <span data-ttu-id="21e89-305">Hello sady nástrojů funkce BYOK nepodporuje klíče chráněné OCS.</span><span class="sxs-lookup"><span data-stu-id="21e89-305">hello BYOK toolset does not support OCS-protected keys.</span></span>
* <span data-ttu-id="21e89-306">Nahraďte hodnotu hello *contosokey* pro hello **ident** a **plainname** s libovolnou hodnotou řetězce.</span><span class="sxs-lookup"><span data-stu-id="21e89-306">Replace hello value of *contosokey* for hello **ident** and **plainname** with any string value.</span></span> <span data-ttu-id="21e89-307">správní režie toominimize a snížit riziko hello chyb, doporučujeme použít stejnou hodnotu pro oba hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-307">toominimize administrative overheads and reduce hello risk of errors, we recommend that you use hello same value for both.</span></span> <span data-ttu-id="21e89-308">Hello **ident** hodnota musí obsahovat jenom čísla, pomlčky a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="21e89-308">hello **ident** value must contain only numbers, dashes, and lower case letters.</span></span>
* <span data-ttu-id="21e89-309">Hello parametr pubexp je prázdný (výchozí nastavení) v tomto příkladu, ale můžete zadat konkrétní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="21e89-309">hello pubexp is left blank (default) in this example, but you can specify specific values.</span></span> <span data-ttu-id="21e89-310">Další informace najdete v tématu hello dokumentace společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-310">For more information, see hello Thales documentation.</span></span>

<span data-ttu-id="21e89-311">Tento příkaz vytvoří soubor Tokenizovaného klíče ve složce %NFAST_KMDATA%\local s názvem začínajícím textem **key_simple_**, za nímž následují hello **ident** zadaný v příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-311">This command creates a Tokenized Key file in your %NFAST_KMDATA%\local folder with a name starting with **key_simple_**, followed by hello **ident** that was specified in hello command.</span></span> <span data-ttu-id="21e89-312">Příklad: **key_simple_contosokey**.</span><span class="sxs-lookup"><span data-stu-id="21e89-312">For example: **key_simple_contosokey**.</span></span> <span data-ttu-id="21e89-313">Tento soubor obsahuje šifrovaný klíč.</span><span class="sxs-lookup"><span data-stu-id="21e89-313">This file contains an encrypted key.</span></span>

<span data-ttu-id="21e89-314">Zálohujte tento soubor Tokenizovaného klíče do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="21e89-314">Back up this Tokenized Key File in a safe location.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21e89-315">Pokud později přenést vaše klíče tooAzure Key Vault, Microsoft nelze exportovat tento klíč back tooyou tak bude velmi důležité, abyste zálohovali váš klíč a architekturu security world bezpečně.</span><span class="sxs-lookup"><span data-stu-id="21e89-315">When you later transfer your key tooAzure Key Vault, Microsoft cannot export this key back tooyou so it becomes extremely important that you back up your key and security world safely.</span></span> <span data-ttu-id="21e89-316">Pokyny a osvědčené postupy pro zálohování klíčů získáte od společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="21e89-316">Contact Thales for guidance and best practices for backing up your key.</span></span>
>
>

<span data-ttu-id="21e89-317">Můžete je nyní připraven tootransfer vaše klíče tooAzure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="21e89-317">You are now ready tootransfer your key tooAzure Key Vault.</span></span>

## <a name="step-4-prepare-your-key-for-transfer"></a><span data-ttu-id="21e89-318">Krok 4: Příprava klíče pro přenos</span><span class="sxs-lookup"><span data-stu-id="21e89-318">Step 4: Prepare your key for transfer</span></span>
<span data-ttu-id="21e89-319">Pro tento čtvrtý krok text hello následující postupy na pracovní stanici hello odpojen.</span><span class="sxs-lookup"><span data-stu-id="21e89-319">For this fourth step, do hello following procedures on hello disconnected workstation.</span></span>

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a><span data-ttu-id="21e89-320">Krok 4.1: Vytvoření kopie klíče se sníženými oprávněními</span><span class="sxs-lookup"><span data-stu-id="21e89-320">Step 4.1: Create a copy of your key with reduced permissions</span></span>

<span data-ttu-id="21e89-321">Otevřete nový příkazový řádek a změňte hello aktuální toohello umístění adresáře kde jste rozbalené soubor zip BYOK hello.</span><span class="sxs-lookup"><span data-stu-id="21e89-321">Open a new command prompt and change hello current directory toohello location where you unzipped hello BYOK zip file.</span></span> <span data-ttu-id="21e89-322">tooreduce hello oprávnění na váš klíč z příkazového řádku, spusťte jeden z následujících hello, v závislosti na zeměpisné oblasti nebo instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="21e89-322">tooreduce hello permissions on your key, from a command prompt, run one of hello following, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="21e89-323">Pro Severní Ameriku:</span><span class="sxs-lookup"><span data-stu-id="21e89-323">For North America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* <span data-ttu-id="21e89-324">Pro Evropu:</span><span class="sxs-lookup"><span data-stu-id="21e89-324">For Europe:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* <span data-ttu-id="21e89-325">Pro Asii:</span><span class="sxs-lookup"><span data-stu-id="21e89-325">For Asia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* <span data-ttu-id="21e89-326">Pro Latinská Amerika:</span><span class="sxs-lookup"><span data-stu-id="21e89-326">For Latin America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* <span data-ttu-id="21e89-327">Pro Japonsko:</span><span class="sxs-lookup"><span data-stu-id="21e89-327">For Japan:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* <span data-ttu-id="21e89-328">Pro Koreu:</span><span class="sxs-lookup"><span data-stu-id="21e89-328">For Korea:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* <span data-ttu-id="21e89-329">Pro Austrálii:</span><span class="sxs-lookup"><span data-stu-id="21e89-329">For Australia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* <span data-ttu-id="21e89-330">Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá hello US government instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="21e89-330">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* <span data-ttu-id="21e89-331">Pro US Government DOD:</span><span class="sxs-lookup"><span data-stu-id="21e89-331">For US Government DOD:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* <span data-ttu-id="21e89-332">Pro Kanadu:</span><span class="sxs-lookup"><span data-stu-id="21e89-332">For Canada:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* <span data-ttu-id="21e89-333">Pro Německo:</span><span class="sxs-lookup"><span data-stu-id="21e89-333">For Germany:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* <span data-ttu-id="21e89-334">Pro Indii:</span><span class="sxs-lookup"><span data-stu-id="21e89-334">For India:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

<span data-ttu-id="21e89-335">Když spustíte tento příkaz, nahraďte *contosokey* s hello stejnou hodnotu, kterou jste zadali v **3.5 krok: Vytvořte nový klíč** z hello [vygenerování klíče](#step-3-generate-your-key) krok.</span><span class="sxs-lookup"><span data-stu-id="21e89-335">When you run this command, replace *contosokey* with hello same value you specified in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>

<span data-ttu-id="21e89-336">Zobrazí se výzva tooplug v karty Správce security world.</span><span class="sxs-lookup"><span data-stu-id="21e89-336">You are asked tooplug in your security world admin cards.</span></span>

<span data-ttu-id="21e89-337">Pokud příkaz hello dokončení zobrazí **výsledek: Úspěch** a hello kopie vašeho klíče se sníženými oprávněními jsou v hello soubor s názvem key_xferacId_<contosokey>.</span><span class="sxs-lookup"><span data-stu-id="21e89-337">When hello command completes, you see **Result: SUCCESS** and hello copy of your key with reduced permissions are in hello file named key_xferacId_<contosokey>.</span></span>

<span data-ttu-id="21e89-338">Může zkontroluje hello seznamy ACL pomocí následujících příkazů pomocí nástrojů Thales hello:</span><span class="sxs-lookup"><span data-stu-id="21e89-338">You may inspects hello ACLS using following commands using hello Thales utilities:</span></span>

* <span data-ttu-id="21e89-339">aclprint.PY:</span><span class="sxs-lookup"><span data-stu-id="21e89-339">aclprint.py:</span></span>

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* <span data-ttu-id="21e89-340">kmfile-dump.exe:</span><span class="sxs-lookup"><span data-stu-id="21e89-340">kmfile-dump.exe:</span></span>

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  <span data-ttu-id="21e89-341">Při spuštění těchto příkazů, nahraďte contosokey hello stejnou hodnotu, kterou jste zadali v **3.5 krok: Vytvořte nový klíč** z hello [vygenerování klíče](#step-3-generate-your-key) krok.</span><span class="sxs-lookup"><span data-stu-id="21e89-341">When you run these commands, replace contosokey with hello same value you specified in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a><span data-ttu-id="21e89-342">Krok 4.2: Zašifrování klíče pomocí klíč pro výměnu klíčů společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="21e89-342">Step 4.2: Encrypt your key by using Microsoft’s Key Exchange Key</span></span>
<span data-ttu-id="21e89-343">Spusťte jeden z následujících příkazů, v závislosti na zeměpisné oblasti nebo instanci Azure hello:</span><span class="sxs-lookup"><span data-stu-id="21e89-343">Run one of hello following commands, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="21e89-344">Pro Severní Ameriku:</span><span class="sxs-lookup"><span data-stu-id="21e89-344">For North America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-345">Pro Evropu:</span><span class="sxs-lookup"><span data-stu-id="21e89-345">For Europe:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-346">Pro Asii:</span><span class="sxs-lookup"><span data-stu-id="21e89-346">For Asia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-347">Pro Latinská Amerika:</span><span class="sxs-lookup"><span data-stu-id="21e89-347">For Latin America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-348">Pro Japonsko:</span><span class="sxs-lookup"><span data-stu-id="21e89-348">For Japan:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-349">Pro Koreu:</span><span class="sxs-lookup"><span data-stu-id="21e89-349">For Korea:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-350">Pro Austrálii:</span><span class="sxs-lookup"><span data-stu-id="21e89-350">For Australia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-351">Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá hello US government instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="21e89-351">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-352">Pro US Government DOD:</span><span class="sxs-lookup"><span data-stu-id="21e89-352">For US Government DOD:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-353">Pro Kanadu:</span><span class="sxs-lookup"><span data-stu-id="21e89-353">For Canada:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-354">Pro Německo:</span><span class="sxs-lookup"><span data-stu-id="21e89-354">For Germany:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="21e89-355">Pro Indii:</span><span class="sxs-lookup"><span data-stu-id="21e89-355">For India:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

<span data-ttu-id="21e89-356">Když spustíte tento příkaz, použijte tyto pokyny:</span><span class="sxs-lookup"><span data-stu-id="21e89-356">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="21e89-357">Nahraďte *contosokey* s hello identifikátor, který jste použili klíč hello toogenerate v **3.5 krok: Vytvořte nový klíč** z hello [vygenerování klíče](#step-3-generate-your-key) krok.</span><span class="sxs-lookup"><span data-stu-id="21e89-357">Replace *contosokey* with hello identifier that you used toogenerate hello key in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>
* <span data-ttu-id="21e89-358">Nahraďte *SubscriptionID* s ID hello hello předplatné Azure, která obsahuje váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="21e89-358">Replace *SubscriptionID* with hello ID of hello Azure subscription that contains your key vault.</span></span> <span data-ttu-id="21e89-359">Načíst tuto hodnotu dříve, v **krok 1.2: získání ID předplatného Azure** z hello [Příprava pracovní stanice připojené k Internetu](#step-1-prepare-your-internet-connected-workstation) krok.</span><span class="sxs-lookup"><span data-stu-id="21e89-359">You retrieved this value previously, in **Step 1.2: Get your Azure subscription ID** from hello [Prepare your Internet-connected workstation](#step-1-prepare-your-internet-connected-workstation) step.</span></span>
* <span data-ttu-id="21e89-360">Nahraďte *ContosoFirstHSMKey* s popiskem, který se používá pro názvu výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="21e89-360">Replace *ContosoFirstHSMKey* with a label that is used for your output file name.</span></span>

<span data-ttu-id="21e89-361">Po dokončení této akce úspěšně, zobrazí **výsledek: Úspěch** a nový soubor existuje v aktuální složce hello, který má následující název hello: KeyTransferPackage -*ContosoFirstHSMkey*.byok</span><span class="sxs-lookup"><span data-stu-id="21e89-361">When this completes successfully, it displays **Result: SUCCESS** and there is a new file in hello current folder that has hello following name: KeyTransferPackage-*ContosoFirstHSMkey*.byok</span></span>

### <a name="step-43-copy-your-key-transfer-package-toohello-internet-connected-workstation"></a><span data-ttu-id="21e89-362">Krok 4.3: Kopírování vaše klíče přenosu balíčku toohello stanice připojené k Internetu</span><span class="sxs-lookup"><span data-stu-id="21e89-362">Step 4.3: Copy your key transfer package toohello Internet-connected workstation</span></span>
<span data-ttu-id="21e89-363">Použijte USB Flash disk nebo jiné přenosné úložné toocopy hello výstupní soubor z hello předchozího kroku (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour stanice připojené k Internetu.</span><span class="sxs-lookup"><span data-stu-id="21e89-363">Use a USB drive or other portable storage toocopy hello output file from hello previous step (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour Internet-connected workstation.</span></span>

## <a name="step-5-transfer-your-key-tooazure-key-vault"></a><span data-ttu-id="21e89-364">Krok 5: Přenos vašeho klíče tooAzure Key Vault</span><span class="sxs-lookup"><span data-stu-id="21e89-364">Step 5: Transfer your key tooAzure Key Vault</span></span>
<span data-ttu-id="21e89-365">Tento poslední krok, hello stanice připojené k Internetu, použít hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) rutiny tooupload hello přenos klíče balíček, který jste zkopírovali ze hello odpojená pracovní stanice toohello Azure Key Vault modulu hardwarového zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="21e89-365">For this final step, on hello Internet-connected workstation, use hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet tooupload hello key transfer package that you copied from hello disconnected workstation toohello Azure Key Vault HSM:</span></span>

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

<span data-ttu-id="21e89-366">Pokud je hello nahrání úspěšné, zobrazí vlastnosti zobrazené hello hello klíče, který jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="21e89-366">If hello upload is successful, you see displayed hello properties of hello key that you just added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21e89-367">Další kroky</span><span class="sxs-lookup"><span data-stu-id="21e89-367">Next steps</span></span>
<span data-ttu-id="21e89-368">Teď můžete použít tento klíč chráněný HSM v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="21e89-368">You can now use this HSM-protected key in your key vault.</span></span> <span data-ttu-id="21e89-369">Další informace najdete v tématu hello **Pokud budete chtít toouse modul hardwarového zabezpečení (HSM)** část v hello [Začínáme s Azure Key Vault](key-vault-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="21e89-369">For more information, see hello **If you want toouse a hardware security module (HSM)** section in hello [Getting started with Azure Key Vault](key-vault-get-started.md) tutorial.</span></span>
