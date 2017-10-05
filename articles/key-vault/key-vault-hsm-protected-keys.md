---
title: "Postup generování a přenos klíčů chráněných pomocí HSM pro Azure Key Vault | Microsoft Docs"
description: "Použijte tento článek vám pomohou plánovat, generovat a potom přeneste vlastní klíčů chráněných pomocí HSM pro použití s Azure Key Vault. Taky označovaný jako BYOK nebo přineste si vlastní klíč."
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
ms.openlocfilehash: 5dbee1221f64045c64fecb344de1e03b2183dfb1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a><span data-ttu-id="0944f-104">Klíče postup generování a přenos chráněných pomocí HSM pro Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0944f-104">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="0944f-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="0944f-105">Introduction</span></span>
<span data-ttu-id="0944f-106">Pro lepší kontrolu Pokud používáte Azure Key Vault, můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM.</span><span class="sxs-lookup"><span data-stu-id="0944f-106">For added assurance, when you use Azure Key Vault, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary.</span></span> <span data-ttu-id="0944f-107">Tento scénář se často označuje jako *přineste si vlastní klíč*, nebo BYOK.</span><span class="sxs-lookup"><span data-stu-id="0944f-107">This scenario is often referred to as *bring your own key*, or BYOK.</span></span> <span data-ttu-id="0944f-108">Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2.</span><span class="sxs-lookup"><span data-stu-id="0944f-108">The HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="0944f-109">Azure Key Vault používá k ochraně klíče Thales nShield řadu moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0944f-109">Azure Key Vault uses Thales nShield family of HSMs to protect your keys.</span></span>

<span data-ttu-id="0944f-110">Použijte informace v tomto tématu vám pomohou plánovat, generovat a potom přeneste vlastní klíčů chráněných pomocí HSM pro použití s Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-110">Use the information in this topic to help you plan for, generate, and then transfer your own HSM-protected keys to use with Azure Key Vault.</span></span>

<span data-ttu-id="0944f-111">Tato funkce není dostupná pro Azure China.</span><span class="sxs-lookup"><span data-stu-id="0944f-111">This functionality is not available for Azure China.</span></span>

> [!NOTE]
> <span data-ttu-id="0944f-112">Další informace o Azure Key Vault najdete v tématu [co je Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="0944f-112">For more information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>  
>
> <span data-ttu-id="0944f-113">Úvodní kurz, který zahrnuje vytvoření trezoru klíčů pro klíčů chráněných pomocí HSM, najdete v části [Začínáme s Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0944f-113">For a getting started tutorial, which includes creating a key vault for HSM-protected keys, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>
>
>

<span data-ttu-id="0944f-114">Další informace o generování a přenos klíč chráněný HSM pomocí přes Internet:</span><span class="sxs-lookup"><span data-stu-id="0944f-114">More information about generating and transferring an HSM-protected key over the Internet:</span></span>

* <span data-ttu-id="0944f-115">Vygenerování klíče z offline pracovní stanice, která omezuje prostor pro útok.</span><span class="sxs-lookup"><span data-stu-id="0944f-115">You generate the key from an offline workstation, which reduces the attack surface.</span></span>
* <span data-ttu-id="0944f-116">Že je klíč zašifrovaný pomocí klíč výměny klíčů (KEK), který zůstává zašifrovaný, dokud se přenese do HSM Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-116">The key is encrypted with a Key Exchange Key (KEK), which stays encrypted until it is transferred to the Azure Key Vault HSMs.</span></span> <span data-ttu-id="0944f-117">Jenom zašifrovaná verze vašeho klíče ponechá původní pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="0944f-117">Only the encrypted version of your key leaves the original workstation.</span></span>
* <span data-ttu-id="0944f-118">Sada nástrojů nastaví vlastnosti pro klíč klienta, která se sváže klíč do architektury security world Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-118">The toolset sets properties on your tenant key that binds your key to the Azure Key Vault security world.</span></span> <span data-ttu-id="0944f-119">Proto po HSM Azure Key Vault přijmou a dešifrují váš klíč, pouze tyto moduly hardwarového zabezpečení můžete použít.</span><span class="sxs-lookup"><span data-stu-id="0944f-119">So after the Azure Key Vault HSMs receive and decrypt your key, only these HSMs can use it.</span></span> <span data-ttu-id="0944f-120">Klíč se nedá exportovat.</span><span class="sxs-lookup"><span data-stu-id="0944f-120">Your key cannot be exported.</span></span> <span data-ttu-id="0944f-121">Tuto vazbu vynucují moduly HSM Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-121">This binding is enforced by the Thales HSMs.</span></span>
* <span data-ttu-id="0944f-122">Klíč výměny klíčů (KEK) používaný k šifrování vašeho klíče se generuje uvnitř HSM Azure Key Vault a není exportovatelný.</span><span class="sxs-lookup"><span data-stu-id="0944f-122">The Key Exchange Key (KEK) that is used to encrypt your key is generated inside the Azure Key Vault HSMs and is not exportable.</span></span> <span data-ttu-id="0944f-123">Moduly hardwarového zabezpečení vynutit, aby může existovat žádná čitelná verze klíče kek mimo tyto moduly Hsm.</span><span class="sxs-lookup"><span data-stu-id="0944f-123">The HSMs enforce that there can be no clear version of the KEK outside the HSMs.</span></span> <span data-ttu-id="0944f-124">Kromě toho sada nástrojů obsahuje záruku od společnosti Thales, že se klíč KEK nedá exportovat a byl vygenerovaný v originálním modulu HSM vyrobeným společností Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-124">In addition, the toolset includes attestation from Thales that the KEK is not exportable and was generated inside a genuine HSM that was manufactured by Thales.</span></span>
* <span data-ttu-id="0944f-125">Sada nástrojů obsahuje záruku od společnosti Thales, že architektury security world Azure Key Vault vygenerovalo také na originálním modulu HSM vyrobeném společností Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-125">The toolset includes attestation from Thales that the Azure Key Vault security world was also generated on a genuine HSM manufactured by Thales.</span></span> <span data-ttu-id="0944f-126">Toto ověření vám poskytuje důkaz, že Microsoft používá originální hardware.</span><span class="sxs-lookup"><span data-stu-id="0944f-126">This attestation proves to you that Microsoft is using genuine hardware.</span></span>
* <span data-ttu-id="0944f-127">Společnost Microsoft používá jiné klíče Kek a jiné architektury Security World v každé geografické oblasti.</span><span class="sxs-lookup"><span data-stu-id="0944f-127">Microsoft uses separate KEKs and separate Security Worlds in each geographical region.</span></span> <span data-ttu-id="0944f-128">Toto oddělení zajišťuje, že váš klíč lze použít pouze v datových centrech v oblasti, ve kterém jste ho zašifrovali.</span><span class="sxs-lookup"><span data-stu-id="0944f-128">This separation ensures that your key can be used only in data centers in the region in which you encrypted it.</span></span> <span data-ttu-id="0944f-129">Například klíč od Evropského zákazníka nelze použít v datových centrech v Severní Americe nebo Asii.</span><span class="sxs-lookup"><span data-stu-id="0944f-129">For example, a key from a European customer cannot be used in data centers in North American or Asia.</span></span>

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a><span data-ttu-id="0944f-130">Další informace o modulech HSM Thales a služby Microsoft</span><span class="sxs-lookup"><span data-stu-id="0944f-130">More information about Thales HSMs and Microsoft services</span></span>
<span data-ttu-id="0944f-131">Thales e-Security je přední globální poskytovatel šifrování dat a řešení kybernetického zabezpečení finančních služeb, špičkové technologie, výroby, government a technologie.</span><span class="sxs-lookup"><span data-stu-id="0944f-131">Thales e-Security is a leading global provider of data encryption and cyber security solutions to the financial services, high technology, manufacturing, government, and technology sectors.</span></span> <span data-ttu-id="0944f-132">S 40-let chrání firemní i vládní informace řešení společnosti Thales využívají je čtyři z pěti největších energie a letecký společností.</span><span class="sxs-lookup"><span data-stu-id="0944f-132">With a 40-year track record of protecting corporate and government information, Thales solutions are used by four of the five largest energy and aerospace companies.</span></span> <span data-ttu-id="0944f-133">Svá řešení jsou také používány 22 členských zemí NATO a zabezpečení více než 80 % po celém světě platebních transakcí.</span><span class="sxs-lookup"><span data-stu-id="0944f-133">Their solutions are also used by 22 NATO countries, and secure more than 80 per cent of worldwide payment transactions.</span></span>

<span data-ttu-id="0944f-134">Microsoft spolupracoval se společností Thales k vylepšení stav obrázky pro moduly hardwarového zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0944f-134">Microsoft has collaborated with Thales to enhance the state of art for HSMs.</span></span> <span data-ttu-id="0944f-135">Tato vylepšení umožňují získat typické výhod hostované služby bez vzdát kontrolu nad klíče.</span><span class="sxs-lookup"><span data-stu-id="0944f-135">These enhancements enable you to get the typical benefits of hosted services without relinquishing control over your keys.</span></span> <span data-ttu-id="0944f-136">Konkrétně tato vylepšení umožňují Microsoftu spravovat moduly HSM, takže není nutné.</span><span class="sxs-lookup"><span data-stu-id="0944f-136">Specifically, these enhancements let Microsoft manage the HSMs so that you do not have to.</span></span> <span data-ttu-id="0944f-137">Jako cloudová služba Azure Key Vault škálování krátkodobě ke splnění nárazovým zvýšením požadavků vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="0944f-137">As a cloud service, Azure Key Vault scales up at short notice to meet your organization’s usage spikes.</span></span> <span data-ttu-id="0944f-138">Ve stejnou dobu, vaše klíč uvnitř modulů HSM Microsoftu chráněný: ponecháte si kontrolu nad životním cyklem klíče, protože klíč vygenerujete a jeho přenesení do HSM společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0944f-138">At the same time, your key is protected inside Microsoft’s HSMs: You retain control over the key lifecycle because you generate the key and transfer it to Microsoft’s HSMs.</span></span>

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a><span data-ttu-id="0944f-139">Implementace funkce přineste si vlastní klíč (BYOK) pro Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0944f-139">Implementing bring your own key (BYOK) for Azure Key Vault</span></span>
<span data-ttu-id="0944f-140">Použijte následující informace a postupy, pokud chcete vygenerovat si vlastní klíč chráněný HSM a pak ho přenést do Azure Key Vault – přineste si vlastní klíč (BYOK) scénář.</span><span class="sxs-lookup"><span data-stu-id="0944f-140">Use the following information and procedures if you will generate your own HSM-protected key and then transfer it to Azure Key Vault—the bring your own key (BYOK) scenario.</span></span>

## <a name="prerequisites-for-byok"></a><span data-ttu-id="0944f-141">Předpoklady pro funkci BYOK</span><span class="sxs-lookup"><span data-stu-id="0944f-141">Prerequisites for BYOK</span></span>
<span data-ttu-id="0944f-142">Najdete v následující tabulce najdete seznam požadavků pro přineste si vlastní klíč (BYOK) pro Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-142">See the following table for a list of prerequisites for bring your own key (BYOK) for Azure Key Vault.</span></span>

| <span data-ttu-id="0944f-143">Požadavek</span><span class="sxs-lookup"><span data-stu-id="0944f-143">Requirement</span></span> | <span data-ttu-id="0944f-144">Další informace</span><span class="sxs-lookup"><span data-stu-id="0944f-144">More information</span></span> |
| --- | --- |
| <span data-ttu-id="0944f-145">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="0944f-145">A subscription to Azure</span></span> |<span data-ttu-id="0944f-146">K vytvoření Azure Key Vault, budete potřebovat předplatné Azure: [zaregistrovat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="0944f-146">To create an Azure Key Vault, you need an Azure subscription: [Sign up for free trial](https://azure.microsoft.com/pricing/free-trial/)</span></span> |
| <span data-ttu-id="0944f-147">Úroveň služby Azure Key Vault Premium pro podporu klíčů chráněných pomocí HSM</span><span class="sxs-lookup"><span data-stu-id="0944f-147">The Azure Key Vault Premium service tier to support HSM-protected keys</span></span> |<span data-ttu-id="0944f-148">Další informace o úrovních služeb a možnosti pro Azure Key Vault najdete v tématu [Azure Key Vault ceny](https://azure.microsoft.com/pricing/details/key-vault/) webu.</span><span class="sxs-lookup"><span data-stu-id="0944f-148">For more information about the service tiers and capabilities for Azure Key Vault, see the [Azure Key Vault Pricing](https://azure.microsoft.com/pricing/details/key-vault/) website.</span></span> |
| <span data-ttu-id="0944f-149">Modulu HSM společnosti Thales, čipové karty a podpůrný software</span><span class="sxs-lookup"><span data-stu-id="0944f-149">Thales HSM, smartcards, and support software</span></span> |<span data-ttu-id="0944f-150">Musíte mít přístup k modulu hardwarového zabezpečení Thales a základní provozní znalosti o modulech HSM Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-150">You must have access to a Thales Hardware Security Module and basic operational knowledge of Thales HSMs.</span></span> <span data-ttu-id="0944f-151">V tématu [modulu hardwarového zabezpečení Thales](https://www.thales-esecurity.com/msrms/buy) seznam kompatibilních modelů nebo modul HSM zakoupit, pokud nemáte jeden.</span><span class="sxs-lookup"><span data-stu-id="0944f-151">See [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) for the list of compatible models, or to purchase an HSM if you do not have one.</span></span> |
| <span data-ttu-id="0944f-152">Následující hardware a software:</span><span class="sxs-lookup"><span data-stu-id="0944f-152">The following hardware and software:</span></span><ol><li><span data-ttu-id="0944f-153">Offline x64 pracovní stanice s minimální operační systém Windows Windows 7 a Thales software nshield od, který je minimálně verze 11.50.</span><span class="sxs-lookup"><span data-stu-id="0944f-153">An offline x64 workstation with a minimum Windows operation system of Windows 7 and Thales nShield software that is at least version 11.50.</span></span><br/><br/><span data-ttu-id="0944f-154">Pokud tato pracovní stanice používá Windows 7, musíte [instalace rozhraní Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span><span class="sxs-lookup"><span data-stu-id="0944f-154">If this workstation runs Windows 7, you must [install Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span></span></li><li><span data-ttu-id="0944f-155">Pracovní stanice, která je připojený k Internetu a má minimální operační systém Windows Windows 7 a [prostředí Azure PowerShell](/powershell/azure/overview) **minimální verzi 1.1.0** nainstalována.</span><span class="sxs-lookup"><span data-stu-id="0944f-155">A workstation that is connected to the Internet and has a minimum Windows operating system of Windows 7 and [Azure PowerShell](/powershell/azure/overview) **minimum version 1.1.0** installed.</span></span></li><li><span data-ttu-id="0944f-156">USB Flash disk nebo jiné přenosné úložné zařízení, která obsahuje aspoň 16 MB volného místa.</span><span class="sxs-lookup"><span data-stu-id="0944f-156">A USB drive or other portable storage device that has at least 16 MB free space.</span></span></li></ol> |<span data-ttu-id="0944f-157">Z bezpečnostních důvodů doporučujeme, aby první pracovní stanice nebyla připojená k síti.</span><span class="sxs-lookup"><span data-stu-id="0944f-157">For security reasons, we recommend that the first workstation is not connected to a network.</span></span> <span data-ttu-id="0944f-158">Toto doporučení se však nevynucuje prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="0944f-158">However, this recommendation is not programmatically enforced.</span></span><br/><br/><span data-ttu-id="0944f-159">Všimněte si, že v následujících pokynech, tento pracovní stanice se označuje jako odpojené pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="0944f-159">Note that in the instructions that follow, this workstation is referred to as the disconnected workstation.</span></span></p></blockquote><br/><span data-ttu-id="0944f-160">Kromě toho pokud váš klíč tenanta je pro produkční síť, doporučujeme použít druhou, samostatnou pracovní stanici stáhnete sadu nástrojů a odešlete klíč tenanta.</span><span class="sxs-lookup"><span data-stu-id="0944f-160">In addition, if your tenant key is for a production network, we recommend that you use a second, separate workstation to download the toolset and upload the tenant key.</span></span> <span data-ttu-id="0944f-161">Ale pro účely testování můžete použít jako první pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="0944f-161">But for testing purposes, you can use the same workstation as the first one.</span></span><br/><br/><span data-ttu-id="0944f-162">Všimněte si, že v následujících pokynech, druhá pracovní stanice se označuje jako stanice připojené k Internetu.</span><span class="sxs-lookup"><span data-stu-id="0944f-162">Note that in the instructions that follow, this second workstation is referred to as the Internet-connected workstation.</span></span></p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a><span data-ttu-id="0944f-163">Generování a přenos vašeho klíče do Azure Key Vault HSM</span><span class="sxs-lookup"><span data-stu-id="0944f-163">Generate and transfer your key to Azure Key Vault HSM</span></span>
<span data-ttu-id="0944f-164">Následující kroky pět použije k vygenerování a přenos vašeho klíče do Azure Key Vault HSM:</span><span class="sxs-lookup"><span data-stu-id="0944f-164">You will use the following five steps to generate and transfer your key to an Azure Key Vault HSM:</span></span>

* [<span data-ttu-id="0944f-165">Krok 1: Příprava pracovní stanice připojené k Internetu</span><span class="sxs-lookup"><span data-stu-id="0944f-165">Step 1: Prepare your Internet-connected workstation</span></span>](#step-1-prepare-your-internet-connected-workstation)
* [<span data-ttu-id="0944f-166">Krok 2: Příprava odpojené pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="0944f-166">Step 2: Prepare your disconnected workstation</span></span>](#step-2-prepare-your-disconnected-workstation)
* [<span data-ttu-id="0944f-167">Krok 3: Vygenerování klíče</span><span class="sxs-lookup"><span data-stu-id="0944f-167">Step 3: Generate your key</span></span>](#step-3-generate-your-key)
* [<span data-ttu-id="0944f-168">Krok 4: Příprava klíče pro přenos</span><span class="sxs-lookup"><span data-stu-id="0944f-168">Step 4: Prepare your key for transfer</span></span>](#step-4-prepare-your-key-for-transfer)
* [<span data-ttu-id="0944f-169">Krok 5: Přenos vašeho klíče do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0944f-169">Step 5: Transfer your key to Azure Key Vault</span></span>](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a><span data-ttu-id="0944f-170">Krok 1: Příprava pracovní stanice připojené k Internetu</span><span class="sxs-lookup"><span data-stu-id="0944f-170">Step 1: Prepare your Internet-connected workstation</span></span>
<span data-ttu-id="0944f-171">Pro tento první krok proveďte následující postupy na pracovní stanici, která je připojena k Internetu.</span><span class="sxs-lookup"><span data-stu-id="0944f-171">For this first step, do the following procedures on your workstation that is connected to the Internet.</span></span>

### <a name="step-11-install-azure-powershell"></a><span data-ttu-id="0944f-172">Krok 1.1: Nainstalování prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0944f-172">Step 1.1: Install Azure PowerShell</span></span>
<span data-ttu-id="0944f-173">Z pracovní stanice připojené k Internetu stáhněte a nainstalujte modul Azure PowerShell, který obsahuje rutiny pro správu Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-173">From the Internet-connected workstation, download and install the Azure PowerShell module that includes the cmdlets to manage Azure Key Vault.</span></span> <span data-ttu-id="0944f-174">To vyžaduje minimální verzi 0.8.13.</span><span class="sxs-lookup"><span data-stu-id="0944f-174">This requires a minimum version of 0.8.13.</span></span>

<span data-ttu-id="0944f-175">Pokyny k instalaci naleznete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0944f-175">For installation instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="step-12-get-your-azure-subscription-id"></a><span data-ttu-id="0944f-176">Krok 1.2: Získání ID předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="0944f-176">Step 1.2: Get your Azure subscription ID</span></span>
<span data-ttu-id="0944f-177">Spusťte relaci prostředí Azure PowerShell a přihlaste se k účtu Azure pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="0944f-177">Start an Azure PowerShell session and sign in to your Azure account by using the following command:</span></span>

        Add-AzureAccount
<span data-ttu-id="0944f-178">V automaticky otevřeném okně prohlížeče zadejte svoje uživatelské jméno a heslo k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="0944f-178">In the pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="0944f-179">Potom použít [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) příkaz:</span><span class="sxs-lookup"><span data-stu-id="0944f-179">Then, use the [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) command:</span></span>

        Get-AzureSubscription
<span data-ttu-id="0944f-180">Z výstupu vyhledejte ID pro odběr, který budete používat pro Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-180">From the output, locate the ID for the subscription you will use for Azure Key Vault.</span></span> <span data-ttu-id="0944f-181">Toto ID předplatného budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="0944f-181">You will need this subscription ID later.</span></span>

<span data-ttu-id="0944f-182">Nezavírejte okno Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0944f-182">Do not close the Azure PowerShell window.</span></span>

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a><span data-ttu-id="0944f-183">Krok 1.3: Stažení sady nástrojů funkce BYOK pro Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0944f-183">Step 1.3: Download the BYOK toolset for Azure Key Vault</span></span>
<span data-ttu-id="0944f-184">Přejděte na web Microsoft Download Center a [stáhnete sadu nástrojů Azure Key Vault BYOK](http://www.microsoft.com/download/details.aspx?id=45345) pro zeměpisné oblasti nebo instanci Azure.</span><span class="sxs-lookup"><span data-stu-id="0944f-184">Go to the Microsoft Download Center and [download the Azure Key Vault BYOK toolset](http://www.microsoft.com/download/details.aspx?id=45345) for your geographic region or instance of Azure.</span></span> <span data-ttu-id="0944f-185">Název balíčku pro stahování a jeho odpovídající hodnotu hash SHA-256 balíčku pomocí následující informace:</span><span class="sxs-lookup"><span data-stu-id="0944f-185">Use the following information to identify the package name to download and its corresponding SHA-256 package hash:</span></span>

- - -
<span data-ttu-id="0944f-186">**Spojené státy americké:**</span><span class="sxs-lookup"><span data-stu-id="0944f-186">**United States:**</span></span>

<span data-ttu-id="0944f-187">KeyVault-BYOK-nástroje spojené States.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-187">KeyVault-BYOK-Tools-UnitedStates.zip</span></span>

<span data-ttu-id="0944f-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span><span class="sxs-lookup"><span data-stu-id="0944f-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span></span>

- - -
<span data-ttu-id="0944f-189">**Evropa:**</span><span class="sxs-lookup"><span data-stu-id="0944f-189">**Europe:**</span></span>

<span data-ttu-id="0944f-190">KeyVault BYOK nástroje Europe.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-190">KeyVault-BYOK-Tools-Europe.zip</span></span>

<span data-ttu-id="0944f-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span><span class="sxs-lookup"><span data-stu-id="0944f-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span></span>

- - -
<span data-ttu-id="0944f-192">**Asii:**</span><span class="sxs-lookup"><span data-stu-id="0944f-192">**Asia:**</span></span>

<span data-ttu-id="0944f-193">KeyVault BYOK nástroje AsiaPacific.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-193">KeyVault-BYOK-Tools-AsiaPacific.zip</span></span>

<span data-ttu-id="0944f-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span><span class="sxs-lookup"><span data-stu-id="0944f-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span></span>

- - -
<span data-ttu-id="0944f-195">**Latinská Amerika:**</span><span class="sxs-lookup"><span data-stu-id="0944f-195">**Latin America:**</span></span>

<span data-ttu-id="0944f-196">KeyVault BYOK nástroje LatinAmerica.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-196">KeyVault-BYOK-Tools-LatinAmerica.zip</span></span>

<span data-ttu-id="0944f-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span><span class="sxs-lookup"><span data-stu-id="0944f-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span></span>

- - -
<span data-ttu-id="0944f-198">**Japonsko:**</span><span class="sxs-lookup"><span data-stu-id="0944f-198">**Japan:**</span></span>

<span data-ttu-id="0944f-199">KeyVault BYOK nástroje Japan.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-199">KeyVault-BYOK-Tools-Japan.zip</span></span>

<span data-ttu-id="0944f-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span><span class="sxs-lookup"><span data-stu-id="0944f-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span></span>

- - -
<span data-ttu-id="0944f-201">**Korea:**</span><span class="sxs-lookup"><span data-stu-id="0944f-201">**Korea:**</span></span>

<span data-ttu-id="0944f-202">KeyVault BYOK nástroje Korea.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-202">KeyVault-BYOK-Tools-Korea.zip</span></span>

<span data-ttu-id="0944f-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span><span class="sxs-lookup"><span data-stu-id="0944f-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span></span>

- - -
<span data-ttu-id="0944f-204">**Austrálie:**</span><span class="sxs-lookup"><span data-stu-id="0944f-204">**Australia:**</span></span>

<span data-ttu-id="0944f-205">KeyVault BYOK nástroje Australia.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-205">KeyVault-BYOK-Tools-Australia.zip</span></span>

<span data-ttu-id="0944f-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span><span class="sxs-lookup"><span data-stu-id="0944f-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span></span>

- - -
[<span data-ttu-id="0944f-207">**Azure Government:**</span><span class="sxs-lookup"><span data-stu-id="0944f-207">**Azure Government:**</span></span>](https://azure.microsoft.com/features/gov/)

<span data-ttu-id="0944f-208">KeyVault BYOK nástroje USGovCloud.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-208">KeyVault-BYOK-Tools-USGovCloud.zip</span></span>

<span data-ttu-id="0944f-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span><span class="sxs-lookup"><span data-stu-id="0944f-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span></span>

- - -
<span data-ttu-id="0944f-210">**Vláda USA DOD:**</span><span class="sxs-lookup"><span data-stu-id="0944f-210">**US Government DOD:**</span></span>

<span data-ttu-id="0944f-211">KeyVault BYOK nástroje USGovernmentDoD.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-211">KeyVault-BYOK-Tools-USGovernmentDoD.zip</span></span>

<span data-ttu-id="0944f-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span><span class="sxs-lookup"><span data-stu-id="0944f-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span></span>

- - -
<span data-ttu-id="0944f-213">**Kanada:**</span><span class="sxs-lookup"><span data-stu-id="0944f-213">**Canada:**</span></span>

<span data-ttu-id="0944f-214">KeyVault BYOK nástroje Canada.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-214">KeyVault-BYOK-Tools-Canada.zip</span></span>

<span data-ttu-id="0944f-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span><span class="sxs-lookup"><span data-stu-id="0944f-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span></span>

- - -
<span data-ttu-id="0944f-216">**Německo:**</span><span class="sxs-lookup"><span data-stu-id="0944f-216">**Germany:**</span></span>

<span data-ttu-id="0944f-217">KeyVault BYOK nástroje Germany.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-217">KeyVault-BYOK-Tools-Germany.zip</span></span>

<span data-ttu-id="0944f-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span><span class="sxs-lookup"><span data-stu-id="0944f-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span></span>

- - -
<span data-ttu-id="0944f-219">**Indie:**</span><span class="sxs-lookup"><span data-stu-id="0944f-219">**India:**</span></span>

<span data-ttu-id="0944f-220">KeyVault BYOK nástroje India.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-220">KeyVault-BYOK-Tools-India.zip</span></span>

<span data-ttu-id="0944f-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span><span class="sxs-lookup"><span data-stu-id="0944f-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span></span>

- - -
<span data-ttu-id="0944f-222">**Spojené království:**</span><span class="sxs-lookup"><span data-stu-id="0944f-222">**United Kingdom:**</span></span>

<span data-ttu-id="0944f-223">KeyVault BYOK nástroje UnitedKingdom.zip</span><span class="sxs-lookup"><span data-stu-id="0944f-223">KeyVault-BYOK-Tools-UnitedKingdom.zip</span></span>

<span data-ttu-id="0944f-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span><span class="sxs-lookup"><span data-stu-id="0944f-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span></span>

- - -

<span data-ttu-id="0944f-225">Chcete-li ověřit integritu vašeho stažené sady nástrojů funkce BYOK, z relace prostředí Azure PowerShell, použijte [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0944f-225">To validate the integrity of your downloaded BYOK toolset, from your Azure PowerShell session, use the [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span></span>

    Get-FileHash KeyVault-BYOK-Tools-*.zip

<span data-ttu-id="0944f-226">Sada nástrojů obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="0944f-226">The toolset includes the following:</span></span>

* <span data-ttu-id="0944f-227">Balíček klíče výměny klíčů (KEK), který má název začíná **BYOK-KEK - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="0944f-227">A Key Exchange Key (KEK) package that has a name beginning with **BYOK-KEK-pkg-.**</span></span>
* <span data-ttu-id="0944f-228">Balíček architektury Security World, jehož název začíná **BYOK-SecurityWorld - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="0944f-228">A Security World package that has a name beginning with **BYOK-SecurityWorld-pkg-.**</span></span>
* <span data-ttu-id="0944f-229">Skript v jazyce python s názvem **verifykeypackage.py.**</span><span class="sxs-lookup"><span data-stu-id="0944f-229">A python script named **verifykeypackage.py.**</span></span>
* <span data-ttu-id="0944f-230">Spustitelný soubor příkazového řádku, s názvem **KeyTransferRemote.exe** a přidružené knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="0944f-230">A command-line executable file named **KeyTransferRemote.exe** and associated DLLs.</span></span>
* <span data-ttu-id="0944f-231">Visual C++ Redistributable balíčku, s názvem **vcredist_x64.exe.**</span><span class="sxs-lookup"><span data-stu-id="0944f-231">A Visual C++ Redistributable Package, named **vcredist_x64.exe.**</span></span>

<span data-ttu-id="0944f-232">Zkopírujte balíček na USB Flash disk nebo jiného přenosného úložiště.</span><span class="sxs-lookup"><span data-stu-id="0944f-232">Copy the package to a USB drive or other portable storage.</span></span>

## <a name="step-2-prepare-your-disconnected-workstation"></a><span data-ttu-id="0944f-233">Krok 2: Příprava odpojené pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="0944f-233">Step 2: Prepare your disconnected workstation</span></span>
<span data-ttu-id="0944f-234">Pro tento druhý krok proveďte následující postupy na pracovní stanici, který není připojen k síti (Internet nebo interní síti).</span><span class="sxs-lookup"><span data-stu-id="0944f-234">For this second step, do the following procedures on the workstation that is not connected to a network (either the Internet or your internal network).</span></span>

### <a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a><span data-ttu-id="0944f-235">Krok 2.1: Příprava odpojené pracovní stanice s modulu HSM společnosti Thales</span><span class="sxs-lookup"><span data-stu-id="0944f-235">Step 2.1: Prepare the disconnected workstation with Thales HSM</span></span>
<span data-ttu-id="0944f-236">Nainstalujte na počítači s Windows podpůrný software nCipher (Thales) a pak k tomuto počítači připojte modul HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-236">Install the nCipher (Thales) support software on a Windows computer, and then attach a Thales HSM to that computer.</span></span>

<span data-ttu-id="0944f-237">Zajistěte, aby byly nástroje Thales ve své cestě (**%nfast_home%\bin**).</span><span class="sxs-lookup"><span data-stu-id="0944f-237">Ensure that the Thales tools are in your path (**%nfast_home%\bin**).</span></span> <span data-ttu-id="0944f-238">Můžete například zadejte následující:</span><span class="sxs-lookup"><span data-stu-id="0944f-238">For example, type the following:</span></span>

        set PATH=%PATH%;"%nfast_home%\bin"

<span data-ttu-id="0944f-239">Další informace najdete v tématu v uživatelské příručce dodané s modulem HSM Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-239">For more information, see the user guide included with the Thales HSM.</span></span>

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a><span data-ttu-id="0944f-240">Krok 2.2: Instalace sady nástrojů funkce BYOK na odpojené pracovní stanice</span><span class="sxs-lookup"><span data-stu-id="0944f-240">Step 2.2: Install the BYOK toolset on the disconnected workstation</span></span>
<span data-ttu-id="0944f-241">Zkopírujte balíček sady nástrojů funkce BYOK z USB Flash disku nebo jiného přenosného úložiště a potom postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0944f-241">Copy the BYOK toolset package from the USB drive or other portable storage, and then do the following:</span></span>

1. <span data-ttu-id="0944f-242">Extrahujte soubory ze staženého balíčku do libovolné složky.</span><span class="sxs-lookup"><span data-stu-id="0944f-242">Extract the files from the downloaded package into any folder.</span></span>
2. <span data-ttu-id="0944f-243">Z této složky spusťte program vcredist_x64.exe.</span><span class="sxs-lookup"><span data-stu-id="0944f-243">From that folder, run vcredist_x64.exe.</span></span>
3. <span data-ttu-id="0944f-244">Postupujte podle pokynů pro instalaci běhových součástí Visual C++ pro Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0944f-244">Follow the instructions to the install the Visual C++ runtime components for Visual Studio 2013.</span></span>

## <a name="step-3-generate-your-key"></a><span data-ttu-id="0944f-245">Krok 3: Vygenerování klíče</span><span class="sxs-lookup"><span data-stu-id="0944f-245">Step 3: Generate your key</span></span>
<span data-ttu-id="0944f-246">Pro tento třetí krok proveďte následující postupy na odpojené pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="0944f-246">For this third step, do the following procedures on the disconnected workstation.</span></span> <span data-ttu-id="0944f-247">K dokončení tohoto kroku vašeho HSM musí být v režimu inicializace.</span><span class="sxs-lookup"><span data-stu-id="0944f-247">To complete this step your HSM must be in initialization mode.</span></span> 


### <a name="step-31-change-the-hsm-mode-to-i"></a><span data-ttu-id="0944f-248">Krok 3.1: Změňte režim modulu hardwarového zabezpečení na "I"</span><span class="sxs-lookup"><span data-stu-id="0944f-248">Step 3.1: Change the HSM mode to 'I'</span></span>
<span data-ttu-id="0944f-249">Pokud používáte Thales nShield Edge, chcete-li změnit režim: 1.</span><span class="sxs-lookup"><span data-stu-id="0944f-249">If you are using Thales nShield Edge, to change the mode: 1.</span></span> <span data-ttu-id="0944f-250">Pomocí tlačítka režimu zvýrazněte požadovaný režim.</span><span class="sxs-lookup"><span data-stu-id="0944f-250">Use the Mode button to highlight the required mode.</span></span> <span data-ttu-id="0944f-251">2.</span><span class="sxs-lookup"><span data-stu-id="0944f-251">2.</span></span> <span data-ttu-id="0944f-252">Během pár sekund stiskněte a podržte tlačítko Vymazat z několika sekund.</span><span class="sxs-lookup"><span data-stu-id="0944f-252">Within a few seconds, press and hold the Clear button for a couple of seconds.</span></span> <span data-ttu-id="0944f-253">Pokud se změní režim, nový režim DIODU přestane blikat a zůstane po.</span><span class="sxs-lookup"><span data-stu-id="0944f-253">If the mode changes, the new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="0944f-254">Indikátor stavu může nepravidelně flash na několik sekund a pak bliká pravidelně v případě, že zařízení není připraveno.</span><span class="sxs-lookup"><span data-stu-id="0944f-254">The Status LED might flash irregularly for a few seconds and then flashes regularly when the device is ready.</span></span> <span data-ttu-id="0944f-255">Jinak zařízení zůstane v aktuálním režimu, s odpovídající režim DIODU lit.</span><span class="sxs-lookup"><span data-stu-id="0944f-255">Otherwise, the device remains in the current mode, with the appropriate mode LED lit.</span></span>

### <a name="step-32-create-a-security-world"></a><span data-ttu-id="0944f-256">Krok 3.2: Vytvoření architektury security world</span><span class="sxs-lookup"><span data-stu-id="0944f-256">Step 3.2: Create a security world</span></span>
<span data-ttu-id="0944f-257">Spusťte příkazový řádek a spusťte program společnosti Thales nové architektury Security world.</span><span class="sxs-lookup"><span data-stu-id="0944f-257">Start a command prompt and run the Thales new-world program.</span></span>

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

<span data-ttu-id="0944f-258">Tento program vytvoří **architektury Security World** soubor v adresáři % NFAST_KMDATA%\local\world, který odpovídá složce aplikací\Místní C:\ProgramData\nCipher\Key správy.</span><span class="sxs-lookup"><span data-stu-id="0944f-258">This program creates a **Security World** file at %NFAST_KMDATA%\local\world, which corresponds to the C:\ProgramData\nCipher\Key Management Data\local folder.</span></span> <span data-ttu-id="0944f-259">Pro kvorum můžete použít jiné hodnoty, ale v našem příkladu se zobrazí výzva k zadání pro každé z nich tři prázdné karty a kódy PIN.</span><span class="sxs-lookup"><span data-stu-id="0944f-259">You can use different values for the quorum but in our example, you’re prompted to enter three blank cards and pins for each one.</span></span> <span data-ttu-id="0944f-260">Jakékoli dvě karty potom poskytnout úplný přístup k architektury security world.</span><span class="sxs-lookup"><span data-stu-id="0944f-260">Then, any two cards give full access to the security world.</span></span> <span data-ttu-id="0944f-261">Tyto karty tvoří **Administrator Card Set** pro nové architektury security world.</span><span class="sxs-lookup"><span data-stu-id="0944f-261">These cards become the **Administrator Card Set** for the new security world.</span></span>

<span data-ttu-id="0944f-262">Potom udělejte následující:</span><span class="sxs-lookup"><span data-stu-id="0944f-262">Then do the following:</span></span>

* <span data-ttu-id="0944f-263">Zálohujte soubor world.</span><span class="sxs-lookup"><span data-stu-id="0944f-263">Back up the world file.</span></span> <span data-ttu-id="0944f-264">Zabezpečení a ochraně soubor world, karty správce a jejich kódy PIN a ujistěte se, že jeden člověk měl přístup k více než jednu kartu.</span><span class="sxs-lookup"><span data-stu-id="0944f-264">Secure and protect the world file, the Administrator Cards, and their pins, and make sure that no single person has access to more than one card.</span></span>

### <a name="step-33-change-the-hsm-mode-to-o"></a><span data-ttu-id="0944f-265">Krok 3.3: Změnit režim HSM, o.</span><span class="sxs-lookup"><span data-stu-id="0944f-265">Step 3.3: Change the HSM mode to 'O'</span></span>
<span data-ttu-id="0944f-266">Pokud používáte Thales nShield Edge, chcete-li změnit režim: 1.</span><span class="sxs-lookup"><span data-stu-id="0944f-266">If you are using Thales nShield Edge, to change the mode: 1.</span></span> <span data-ttu-id="0944f-267">Pomocí tlačítka režimu zvýrazněte požadovaný režim.</span><span class="sxs-lookup"><span data-stu-id="0944f-267">Use the Mode button to highlight the required mode.</span></span> <span data-ttu-id="0944f-268">2.</span><span class="sxs-lookup"><span data-stu-id="0944f-268">2.</span></span> <span data-ttu-id="0944f-269">Během pár sekund stiskněte a podržte tlačítko Vymazat z několika sekund.</span><span class="sxs-lookup"><span data-stu-id="0944f-269">Within a few seconds, press and hold the Clear button for a couple of seconds.</span></span> <span data-ttu-id="0944f-270">Pokud se změní režim, nový režim DIODU přestane blikat a zůstane po.</span><span class="sxs-lookup"><span data-stu-id="0944f-270">If the mode changes, the new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="0944f-271">Indikátor stavu může nepravidelně flash na několik sekund a pak bliká pravidelně v případě, že zařízení není připraveno.</span><span class="sxs-lookup"><span data-stu-id="0944f-271">The Status LED might flash irregularly for a few seconds and then flashes regularly when the device is ready.</span></span> <span data-ttu-id="0944f-272">Jinak zařízení zůstane v aktuálním režimu, s odpovídající režim DIODU lit.</span><span class="sxs-lookup"><span data-stu-id="0944f-272">Otherwise, the device remains in the current mode, with the appropriate mode LED lit.</span></span>


### <a name="step-34-validate-the-downloaded-package"></a><span data-ttu-id="0944f-273">Krok 3.4: Ověření staženého balíčku</span><span class="sxs-lookup"><span data-stu-id="0944f-273">Step 3.4: Validate the downloaded package</span></span>
<span data-ttu-id="0944f-274">Tento krok je volitelný, ale doporučujeme, aby mohli ověřit tyto:</span><span class="sxs-lookup"><span data-stu-id="0944f-274">This step is optional but recommended so that you can validate the following:</span></span>

* <span data-ttu-id="0944f-275">Klíč pro výměnu klíčů, který je součástí sady nástrojů, byl vygenerovaný na originálním modulu HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-275">The Key Exchange Key that is included in the toolset has been generated from a genuine Thales HSM.</span></span>
* <span data-ttu-id="0944f-276">Hodnota hash architektury Security World, který je součástí sady nástrojů, byla vygenerovaná na originálním modulu HSM společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-276">The hash of the Security World that is included in the toolset has been generated in a genuine Thales HSM.</span></span>
* <span data-ttu-id="0944f-277">Klíč pro výměnu klíčů je neexportovatelného.</span><span class="sxs-lookup"><span data-stu-id="0944f-277">The Key Exchange Key is non-exportable.</span></span>

> [!NOTE]
> <span data-ttu-id="0944f-278">Chcete-li ověření staženého balíčku, modul hardwarového zabezpečení musí být připojený, zapnutý a musí mít architektury security world v něm (třeba tu, kterou jste právě vytvořili).</span><span class="sxs-lookup"><span data-stu-id="0944f-278">To validate the downloaded package, the HSM must be connected, powered on, and must have a security world on it (such as the one you’ve just created).</span></span>
>
>

<span data-ttu-id="0944f-279">Ověření staženého balíčku:</span><span class="sxs-lookup"><span data-stu-id="0944f-279">To validate the downloaded package:</span></span>

1. <span data-ttu-id="0944f-280">Spusťte skript verifykeypackage.py tak, že zadáte jednu z těchto, v závislosti na zeměpisné oblasti nebo instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="0944f-280">Run the verifykeypackage.py script by typing one of the following, depending on your geographic region or instance of Azure:</span></span>

   * <span data-ttu-id="0944f-281">Pro Severní Ameriku:</span><span class="sxs-lookup"><span data-stu-id="0944f-281">For North America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * <span data-ttu-id="0944f-282">Pro Evropu:</span><span class="sxs-lookup"><span data-stu-id="0944f-282">For Europe:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * <span data-ttu-id="0944f-283">Pro Asii:</span><span class="sxs-lookup"><span data-stu-id="0944f-283">For Asia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * <span data-ttu-id="0944f-284">Pro Latinská Amerika:</span><span class="sxs-lookup"><span data-stu-id="0944f-284">For Latin America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * <span data-ttu-id="0944f-285">Pro Japonsko:</span><span class="sxs-lookup"><span data-stu-id="0944f-285">For Japan:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * <span data-ttu-id="0944f-286">Pro Koreu:</span><span class="sxs-lookup"><span data-stu-id="0944f-286">For Korea:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * <span data-ttu-id="0944f-287">Pro Austrálii:</span><span class="sxs-lookup"><span data-stu-id="0944f-287">For Australia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * <span data-ttu-id="0944f-288">Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá instanci Azure US government:</span><span class="sxs-lookup"><span data-stu-id="0944f-288">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses the US government instance of Azure:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * <span data-ttu-id="0944f-289">Pro US Government DOD:</span><span class="sxs-lookup"><span data-stu-id="0944f-289">For US Government DOD:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * <span data-ttu-id="0944f-290">Pro Kanadu:</span><span class="sxs-lookup"><span data-stu-id="0944f-290">For Canada:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * <span data-ttu-id="0944f-291">Pro Německo:</span><span class="sxs-lookup"><span data-stu-id="0944f-291">For Germany:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * <span data-ttu-id="0944f-292">Pro Indii:</span><span class="sxs-lookup"><span data-stu-id="0944f-292">For India:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > <span data-ttu-id="0944f-293">Software společnosti Thales obsahuje python v %NFAST_HOME%\python\bin</span><span class="sxs-lookup"><span data-stu-id="0944f-293">The Thales software includes python at %NFAST_HOME%\python\bin</span></span>
     >
     >
2. <span data-ttu-id="0944f-294">Zkontrolujte, jestli následující příkaz, který znamená úspěšné ověření: **výsledek: Úspěch**</span><span class="sxs-lookup"><span data-stu-id="0944f-294">Confirm that you see the following, which indicates successful validation: **Result: SUCCESS**</span></span>

<span data-ttu-id="0944f-295">Tento skript ověřuje řetězec podepisujících až ke kořenovému klíči Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-295">This script validates the signer chain up to the Thales root key.</span></span> <span data-ttu-id="0944f-296">Hodnota hash tohoto kořenového klíče je vložená ve skriptu a jeho hodnota by měla být **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span><span class="sxs-lookup"><span data-stu-id="0944f-296">The hash of this root key is embedded in the script and its value should be **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span></span> <span data-ttu-id="0944f-297">Tuto hodnotu můžete potvrdit samostatně navštivte stránky [webu společnosti Thales](http://www.thalesesec.com/).</span><span class="sxs-lookup"><span data-stu-id="0944f-297">You can also confirm this value separately by visiting the [Thales website](http://www.thalesesec.com/).</span></span>

<span data-ttu-id="0944f-298">Nyní jste připraveni vytvořit nový klíč.</span><span class="sxs-lookup"><span data-stu-id="0944f-298">You’re now ready to create a new key.</span></span>

### <a name="step-35-create-a-new-key"></a><span data-ttu-id="0944f-299">3.5 krok: Vytvořte nový klíč</span><span class="sxs-lookup"><span data-stu-id="0944f-299">Step 3.5: Create a new key</span></span>
<span data-ttu-id="0944f-300">Generování klíče pomocí společnosti Thales **generatekey** program.</span><span class="sxs-lookup"><span data-stu-id="0944f-300">Generate a key by using the Thales **generatekey** program.</span></span>

<span data-ttu-id="0944f-301">Spusťte následující příkaz k vygenerování klíče:</span><span class="sxs-lookup"><span data-stu-id="0944f-301">Run the following command to generate the key:</span></span>

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

<span data-ttu-id="0944f-302">Když spustíte tento příkaz, použijte tyto pokyny:</span><span class="sxs-lookup"><span data-stu-id="0944f-302">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="0944f-303">Parametr *chránit* musí být nastavena na hodnotu **modulu**, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="0944f-303">The parameter *protect* must be set to the value **module**, as shown.</span></span> <span data-ttu-id="0944f-304">Tím se vytvoří klíč chráněný modulem.</span><span class="sxs-lookup"><span data-stu-id="0944f-304">This creates a module-protected key.</span></span> <span data-ttu-id="0944f-305">Sada nástrojů BYOK nepodporuje klíče chráněné OCS.</span><span class="sxs-lookup"><span data-stu-id="0944f-305">The BYOK toolset does not support OCS-protected keys.</span></span>
* <span data-ttu-id="0944f-306">Nahraďte hodnotu *contosokey* pro **ident** a **plainname** s libovolnou hodnotou řetězce.</span><span class="sxs-lookup"><span data-stu-id="0944f-306">Replace the value of *contosokey* for the **ident** and **plainname** with any string value.</span></span> <span data-ttu-id="0944f-307">Chcete-li minimalizovat správní režie a snížilo riziko chyby, doporučujeme použít stejnou hodnotu pro oba.</span><span class="sxs-lookup"><span data-stu-id="0944f-307">To minimize administrative overheads and reduce the risk of errors, we recommend that you use the same value for both.</span></span> <span data-ttu-id="0944f-308">**Ident** hodnota musí obsahovat jenom čísla, pomlčky a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="0944f-308">The **ident** value must contain only numbers, dashes, and lower case letters.</span></span>
* <span data-ttu-id="0944f-309">Pubexp je v prázdné (výchozí) v tomto příkladu, ale můžete zadat konkrétní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="0944f-309">The pubexp is left blank (default) in this example, but you can specify specific values.</span></span> <span data-ttu-id="0944f-310">Další informace naleznete v dokumentaci společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-310">For more information, see the Thales documentation.</span></span>

<span data-ttu-id="0944f-311">Tento příkaz vytvoří soubor Tokenizovaného klíče ve složce %NFAST_KMDATA%\local s názvem začínajícím textem **key_simple_**, za nímž následují **ident** zadaný v příkazu.</span><span class="sxs-lookup"><span data-stu-id="0944f-311">This command creates a Tokenized Key file in your %NFAST_KMDATA%\local folder with a name starting with **key_simple_**, followed by the **ident** that was specified in the command.</span></span> <span data-ttu-id="0944f-312">Příklad: **key_simple_contosokey**.</span><span class="sxs-lookup"><span data-stu-id="0944f-312">For example: **key_simple_contosokey**.</span></span> <span data-ttu-id="0944f-313">Tento soubor obsahuje šifrovaný klíč.</span><span class="sxs-lookup"><span data-stu-id="0944f-313">This file contains an encrypted key.</span></span>

<span data-ttu-id="0944f-314">Zálohujte tento soubor Tokenizovaného klíče do bezpečného umístění.</span><span class="sxs-lookup"><span data-stu-id="0944f-314">Back up this Tokenized Key File in a safe location.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0944f-315">Když budete chtít klíč později přenést do Azure Key Vault, Microsoft nelze exportovat tento klíč vám tak bude velmi důležité, abyste zálohovali váš klíč a architekturu security world bezpečně.</span><span class="sxs-lookup"><span data-stu-id="0944f-315">When you later transfer your key to Azure Key Vault, Microsoft cannot export this key back to you so it becomes extremely important that you back up your key and security world safely.</span></span> <span data-ttu-id="0944f-316">Pokyny a osvědčené postupy pro zálohování klíčů získáte od společnosti Thales.</span><span class="sxs-lookup"><span data-stu-id="0944f-316">Contact Thales for guidance and best practices for backing up your key.</span></span>
>
>

<span data-ttu-id="0944f-317">Nyní jste připraveni přenos vašeho klíče do Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="0944f-317">You are now ready to transfer your key to Azure Key Vault.</span></span>

## <a name="step-4-prepare-your-key-for-transfer"></a><span data-ttu-id="0944f-318">Krok 4: Příprava klíče pro přenos</span><span class="sxs-lookup"><span data-stu-id="0944f-318">Step 4: Prepare your key for transfer</span></span>
<span data-ttu-id="0944f-319">Pro tento čtvrtý krok proveďte následující postupy na odpojené pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="0944f-319">For this fourth step, do the following procedures on the disconnected workstation.</span></span>

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a><span data-ttu-id="0944f-320">Krok 4.1: Vytvoření kopie klíče se sníženými oprávněními</span><span class="sxs-lookup"><span data-stu-id="0944f-320">Step 4.1: Create a copy of your key with reduced permissions</span></span>

<span data-ttu-id="0944f-321">Otevřete nový příkazový řádek a změňte aktuální adresář na umístění, kde unzipped BYOK souboru zip.</span><span class="sxs-lookup"><span data-stu-id="0944f-321">Open a new command prompt and change the current directory to the location where you unzipped the BYOK zip file.</span></span> <span data-ttu-id="0944f-322">Chcete-li omezit oprávnění pro váš klíč z příkazového řádku, spusťte jeden z následujících, v závislosti na zeměpisné oblasti nebo instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="0944f-322">To reduce the permissions on your key, from a command prompt, run one of the following, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="0944f-323">Pro Severní Ameriku:</span><span class="sxs-lookup"><span data-stu-id="0944f-323">For North America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* <span data-ttu-id="0944f-324">Pro Evropu:</span><span class="sxs-lookup"><span data-stu-id="0944f-324">For Europe:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* <span data-ttu-id="0944f-325">Pro Asii:</span><span class="sxs-lookup"><span data-stu-id="0944f-325">For Asia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* <span data-ttu-id="0944f-326">Pro Latinská Amerika:</span><span class="sxs-lookup"><span data-stu-id="0944f-326">For Latin America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* <span data-ttu-id="0944f-327">Pro Japonsko:</span><span class="sxs-lookup"><span data-stu-id="0944f-327">For Japan:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* <span data-ttu-id="0944f-328">Pro Koreu:</span><span class="sxs-lookup"><span data-stu-id="0944f-328">For Korea:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* <span data-ttu-id="0944f-329">Pro Austrálii:</span><span class="sxs-lookup"><span data-stu-id="0944f-329">For Australia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* <span data-ttu-id="0944f-330">Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá instanci Azure US government:</span><span class="sxs-lookup"><span data-stu-id="0944f-330">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses the US government instance of Azure:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* <span data-ttu-id="0944f-331">Pro US Government DOD:</span><span class="sxs-lookup"><span data-stu-id="0944f-331">For US Government DOD:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* <span data-ttu-id="0944f-332">Pro Kanadu:</span><span class="sxs-lookup"><span data-stu-id="0944f-332">For Canada:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* <span data-ttu-id="0944f-333">Pro Německo:</span><span class="sxs-lookup"><span data-stu-id="0944f-333">For Germany:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* <span data-ttu-id="0944f-334">Pro Indii:</span><span class="sxs-lookup"><span data-stu-id="0944f-334">For India:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

<span data-ttu-id="0944f-335">Když spustíte tento příkaz, nahraďte *contosokey* nahraďte stejnou hodnotou, kterou jste zadali v **3.5 krok: Vytvořte nový klíč** z [vygenerování klíče](#step-3-generate-your-key) krok.</span><span class="sxs-lookup"><span data-stu-id="0944f-335">When you run this command, replace *contosokey* with the same value you specified in **Step 3.5: Create a new key** from the [Generate your key](#step-3-generate-your-key) step.</span></span>

<span data-ttu-id="0944f-336">Zobrazí se výzva k připojení karet správce zabezpečení world.</span><span class="sxs-lookup"><span data-stu-id="0944f-336">You are asked to plug in your security world admin cards.</span></span>

<span data-ttu-id="0944f-337">Až se příkaz dokončí, zobrazí **výsledek: Úspěch** a kopii klíče se sníženými oprávněními jsou v souboru s názvem key_xferacId_<contosokey>.</span><span class="sxs-lookup"><span data-stu-id="0944f-337">When the command completes, you see **Result: SUCCESS** and the copy of your key with reduced permissions are in the file named key_xferacId_<contosokey>.</span></span>

<span data-ttu-id="0944f-338">Může zkontroluje seznamy řízení přístupu pomocí následujících příkazů pomocí nástrojů Thales:</span><span class="sxs-lookup"><span data-stu-id="0944f-338">You may inspects the ACLS using following commands using the Thales utilities:</span></span>

* <span data-ttu-id="0944f-339">aclprint.PY:</span><span class="sxs-lookup"><span data-stu-id="0944f-339">aclprint.py:</span></span>

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* <span data-ttu-id="0944f-340">kmfile-dump.exe:</span><span class="sxs-lookup"><span data-stu-id="0944f-340">kmfile-dump.exe:</span></span>

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  <span data-ttu-id="0944f-341">Při spuštění těchto příkazů, nahraďte contosokey stejnou hodnotu, která jste zadali v **3.5 krok: Vytvořte nový klíč** z [vygenerování klíče](#step-3-generate-your-key) krok.</span><span class="sxs-lookup"><span data-stu-id="0944f-341">When you run these commands, replace contosokey with the same value you specified in **Step 3.5: Create a new key** from the [Generate your key](#step-3-generate-your-key) step.</span></span>

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a><span data-ttu-id="0944f-342">Krok 4.2: Zašifrování klíče pomocí klíč pro výměnu klíčů společnosti Microsoft</span><span class="sxs-lookup"><span data-stu-id="0944f-342">Step 4.2: Encrypt your key by using Microsoft’s Key Exchange Key</span></span>
<span data-ttu-id="0944f-343">Spusťte jeden z následujících příkazů, v závislosti na zeměpisné oblasti nebo instanci Azure:</span><span class="sxs-lookup"><span data-stu-id="0944f-343">Run one of the following commands, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="0944f-344">Pro Severní Ameriku:</span><span class="sxs-lookup"><span data-stu-id="0944f-344">For North America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-345">Pro Evropu:</span><span class="sxs-lookup"><span data-stu-id="0944f-345">For Europe:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-346">Pro Asii:</span><span class="sxs-lookup"><span data-stu-id="0944f-346">For Asia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-347">Pro Latinská Amerika:</span><span class="sxs-lookup"><span data-stu-id="0944f-347">For Latin America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-348">Pro Japonsko:</span><span class="sxs-lookup"><span data-stu-id="0944f-348">For Japan:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-349">Pro Koreu:</span><span class="sxs-lookup"><span data-stu-id="0944f-349">For Korea:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-350">Pro Austrálii:</span><span class="sxs-lookup"><span data-stu-id="0944f-350">For Australia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-351">Pro [Azure Government](https://azure.microsoft.com/features/gov/), který používá instanci Azure US government:</span><span class="sxs-lookup"><span data-stu-id="0944f-351">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses the US government instance of Azure:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-352">Pro US Government DOD:</span><span class="sxs-lookup"><span data-stu-id="0944f-352">For US Government DOD:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-353">Pro Kanadu:</span><span class="sxs-lookup"><span data-stu-id="0944f-353">For Canada:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-354">Pro Německo:</span><span class="sxs-lookup"><span data-stu-id="0944f-354">For Germany:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="0944f-355">Pro Indii:</span><span class="sxs-lookup"><span data-stu-id="0944f-355">For India:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

<span data-ttu-id="0944f-356">Když spustíte tento příkaz, použijte tyto pokyny:</span><span class="sxs-lookup"><span data-stu-id="0944f-356">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="0944f-357">Nahraďte *contosokey* nahraďte identifikátorem, který jste použili k vygenerování klíče v **3.5 krok: Vytvořte nový klíč** z [vygenerování klíče](#step-3-generate-your-key) krok.</span><span class="sxs-lookup"><span data-stu-id="0944f-357">Replace *contosokey* with the identifier that you used to generate the key in **Step 3.5: Create a new key** from the [Generate your key](#step-3-generate-your-key) step.</span></span>
* <span data-ttu-id="0944f-358">Nahraďte *SubscriptionID* s ID předplatného Azure, která obsahuje váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="0944f-358">Replace *SubscriptionID* with the ID of the Azure subscription that contains your key vault.</span></span> <span data-ttu-id="0944f-359">Načíst tuto hodnotu dříve, v **krok 1.2: získání ID předplatného Azure** z [Příprava pracovní stanice připojené k Internetu](#step-1-prepare-your-internet-connected-workstation) krok.</span><span class="sxs-lookup"><span data-stu-id="0944f-359">You retrieved this value previously, in **Step 1.2: Get your Azure subscription ID** from the [Prepare your Internet-connected workstation](#step-1-prepare-your-internet-connected-workstation) step.</span></span>
* <span data-ttu-id="0944f-360">Nahraďte *ContosoFirstHSMKey* s popiskem, který se používá pro názvu výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="0944f-360">Replace *ContosoFirstHSMKey* with a label that is used for your output file name.</span></span>

<span data-ttu-id="0944f-361">Po dokončení této akce úspěšně, zobrazí **výsledek: Úspěch** a nový soubor existuje v aktuální složce, která má následující název: KeyTransferPackage -*ContosoFirstHSMkey*.byok</span><span class="sxs-lookup"><span data-stu-id="0944f-361">When this completes successfully, it displays **Result: SUCCESS** and there is a new file in the current folder that has the following name: KeyTransferPackage-*ContosoFirstHSMkey*.byok</span></span>

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a><span data-ttu-id="0944f-362">Krok 4.3: Zkopírování balíčku pro přenos klíče na pracovní stanici připojené k Internetu</span><span class="sxs-lookup"><span data-stu-id="0944f-362">Step 4.3: Copy your key transfer package to the Internet-connected workstation</span></span>
<span data-ttu-id="0944f-363">Pomocí USB Flash disk nebo jiného přenosného úložiště zkopírujte výstupní soubor z předchozího kroku (KeyTransferPackage-ContosoFirstHSMkey.byok) do pracovní stanice připojené k Internetu.</span><span class="sxs-lookup"><span data-stu-id="0944f-363">Use a USB drive or other portable storage to copy the output file from the previous step (KeyTransferPackage-ContosoFirstHSMkey.byok) to your Internet-connected workstation.</span></span>

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a><span data-ttu-id="0944f-364">Krok 5: Přenos vašeho klíče do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="0944f-364">Step 5: Transfer your key to Azure Key Vault</span></span>
<span data-ttu-id="0944f-365">Tento poslední krok, na pracovní stanici připojené k Internetu, použijte [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) rutinu k odeslání balíčku pro přenos klíče, který jste zkopírovali z odpojené pracovní stanici do Azure Key Vault HSM:</span><span class="sxs-lookup"><span data-stu-id="0944f-365">For this final step, on the Internet-connected workstation, use the [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet to upload the key transfer package that you copied from the disconnected workstation to the Azure Key Vault HSM:</span></span>

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

<span data-ttu-id="0944f-366">Pokud bude odesílání úspěšné, zobrazí zobrazí vlastnosti klíče, který jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="0944f-366">If the upload is successful, you see displayed the properties of the key that you just added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0944f-367">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0944f-367">Next steps</span></span>
<span data-ttu-id="0944f-368">Teď můžete použít tento klíč chráněný HSM v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="0944f-368">You can now use this HSM-protected key in your key vault.</span></span> <span data-ttu-id="0944f-369">Další informace najdete v tématu **Pokud chcete použít modul hardwarového zabezpečení (HSM)** tématu [Začínáme s Azure Key Vault](key-vault-get-started.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="0944f-369">For more information, see the **If you want to use a hardware security module (HSM)** section in the [Getting started with Azure Key Vault](key-vault-get-started.md) tutorial.</span></span>
