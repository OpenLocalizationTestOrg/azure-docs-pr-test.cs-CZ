---
title: "aaaReplicate virtuálních počítačů technologie Hyper-V pomocí prostředí PowerShell a Azure Resource Manager | Microsoft Docs"
description: "Automatizovat replikaci hello tooAzure virtuálních počítačů Hyper-V s Azure Site Recovery pomocí Powershellu a Azure Resource Manager."
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 4fb15ce2e9ad54f1dd6f54ff769eb912aa4b0272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="fd34c-103">Replikaci mezi místními technologie Hyper-V virtuální počítače a Azure pomocí prostředí PowerShell a Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fd34c-103">Replicate between on-premises Hyper-V virtual machines and Azure by using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd34c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fd34c-104">Azure Portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="fd34c-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fd34c-105">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
> * [<span data-ttu-id="fd34c-106">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="fd34c-106">Classic Portal</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a><span data-ttu-id="fd34c-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="fd34c-107">Overview</span></span>
<span data-ttu-id="fd34c-108">Azure Site Recovery přispívá tooyour obchodní kontinuitu a po havárii obnovení strategie tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení.</span><span class="sxs-lookup"><span data-stu-id="fd34c-108">Azure Site Recovery contributes tooyour business continuity and disaster recovery strategy by orchestrating replication, failover, and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="fd34c-109">Úplný seznam scénářů nasazení najdete v tématu hello [přehled Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fd34c-109">For a full list of deployment scenarios, see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="fd34c-110">Prostředí Azure PowerShell je modul, který poskytuje toomanage rutiny Azure pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fd34c-110">Azure PowerShell is a module that provides cmdlets toomanage Azure through Windows PowerShell.</span></span> <span data-ttu-id="fd34c-111">Můžete pracovat se dva typy modulů: hello profilu Azure modulu nebo modulu Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-111">It can work with two types of modules: hello Azure Profile module, or hello Azure Resource Manager module.</span></span>

<span data-ttu-id="fd34c-112">Rutiny prostředí PowerShell obnovení lokality pomocí prostředí Azure PowerShell pro Azure Resource Manager, můžete chránit a obnovit vaše servery v Azure.</span><span class="sxs-lookup"><span data-stu-id="fd34c-112">Site Recovery PowerShell cmdlets, available with Azure PowerShell for Azure Resource Manager, help you protect and recover your servers in Azure.</span></span>

<span data-ttu-id="fd34c-113">Tento článek popisuje, jak toouse prostředí Windows PowerShell, společně s Azure Resource Manager, tooconfigure toodeploy Site Recovery a orchestraci tooAzure ochrany serveru.</span><span class="sxs-lookup"><span data-stu-id="fd34c-113">This article describes how toouse Windows PowerShell, together with Azure Resource Manager, toodeploy Site Recovery tooconfigure and orchestrate server protection tooAzure.</span></span> <span data-ttu-id="fd34c-114">Příklad Hello používané v tomto článku se dozvíte, jak tooprotect, převzetí služeb při selhání a obnovení virtuálních počítačů na hostitele technologií Hyper-V tooAzure pomocí prostředí Azure PowerShell s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fd34c-114">hello example used in this article shows you how tooprotect, fail over, and recover virtual machines on a Hyper-V host tooAzure, by using Azure PowerShell with Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="fd34c-115">Hello rutiny prostředí PowerShell obnovení lokality aktuálně umožňují tooconfigure hello následující: jeden tooanother lokality nástroje Virtual Machine Manager, tooAzure lokality nástroje Virtual Machine Manager a tooAzure lokality technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd34c-115">hello Site Recovery PowerShell cmdlets currently allow you tooconfigure hello following: one Virtual Machine Manager site tooanother, a Virtual Machine Manager site tooAzure, and a Hyper-V site tooAzure.</span></span>
>
>

<span data-ttu-id="fd34c-116">Nepotřebujete toobe odborné toouse prostředí PowerShell v tomto článku, ale potřebujete toounderstand hello základní koncepty, jako jsou moduly, rutiny a relace.</span><span class="sxs-lookup"><span data-stu-id="fd34c-116">You don't need toobe a PowerShell expert toouse this article, but you do need toounderstand hello basic concepts, such as modules, cmdlets, and sessions.</span></span> <span data-ttu-id="fd34c-117">Další informace o prostředí Windows PowerShell najdete v tématu [Začínáme s prostředím Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span><span class="sxs-lookup"><span data-stu-id="fd34c-117">For more information about Windows PowerShell, see [Getting Started with Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span></span>

<span data-ttu-id="fd34c-118">Také další informace o [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="fd34c-118">You can also read more about [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fd34c-119">Partneři Microsoftu, které jsou součástí programu hello Cloud Solution Provider (CSP) můžete konfigurovat a spravovat ochranu systému serverů se svým zákazníkům tootheir zákazníků příslušného poskytovatele CSP odběry (odběry klienta).</span><span class="sxs-lookup"><span data-stu-id="fd34c-119">Microsoft partners that are part of hello Cloud Solution Provider (CSP) program can configure and manage protection of their customers' servers tootheir customers' respective CSP subscriptions (tenant subscriptions).</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="fd34c-120">Než začnete</span><span class="sxs-lookup"><span data-stu-id="fd34c-120">Before you start</span></span>
<span data-ttu-id="fd34c-121">Ujistěte se, že máte zavedenou tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="fd34c-121">Make sure you have these prerequisites in place:</span></span>

* <span data-ttu-id="fd34c-122">A [Microsoft Azure](https://azure.microsoft.com/) účtu.</span><span class="sxs-lookup"><span data-stu-id="fd34c-122">A [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="fd34c-123">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fd34c-123">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="fd34c-124">Kromě toho si můžete přečíst o [cenách Azure Site Recovery Manageru](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="fd34c-124">In addition, you can read about [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="fd34c-125">Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="fd34c-125">Azure PowerShell 1.0.</span></span> <span data-ttu-id="fd34c-126">Informace o tomto vydání a jak tooinstall, najdete v části [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="fd34c-126">For information about this release and how tooinstall it, see [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span></span>
* <span data-ttu-id="fd34c-127">Hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) a [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduly.</span><span class="sxs-lookup"><span data-stu-id="fd34c-127">hello [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) and [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules.</span></span> <span data-ttu-id="fd34c-128">Můžete získat nejnovější verze hello tyto moduly hello [Galerie prostředí PowerShell](https://www.powershellgallery.com/)</span><span class="sxs-lookup"><span data-stu-id="fd34c-128">You can get hello latest versions of these modules from hello [PowerShell gallery](https://www.powershellgallery.com/)</span></span>

<span data-ttu-id="fd34c-129">Tento článek ukazuje, jak toouse prostředí Azure Powershell s Azure Resource Manager tooconfigure a spravovat ochranu vašich serverů.</span><span class="sxs-lookup"><span data-stu-id="fd34c-129">This article illustrates how toouse Azure Powershell with Azure Resource Manager tooconfigure and manage protection of your servers.</span></span> <span data-ttu-id="fd34c-130">Příklad Hello používané v tomto článku se dozvíte, jak tooprotect virtuálního počítače na hostiteli technologie Hyper-V, tooAzure spuštěná.</span><span class="sxs-lookup"><span data-stu-id="fd34c-130">hello example used in this article shows you how tooprotect a virtual machine, running on a Hyper-V host, tooAzure.</span></span> <span data-ttu-id="fd34c-131">Hello následující požadované součásti jsou konkrétní toothis příklad.</span><span class="sxs-lookup"><span data-stu-id="fd34c-131">hello following prerequisites are specific toothis example.</span></span> <span data-ttu-id="fd34c-132">Další komplexní sadu požadavků pro hello různé scénáře obnovení lokality, naleznete v dokumentaci toohello toothat scénář, která se týkají.</span><span class="sxs-lookup"><span data-stu-id="fd34c-132">For a more comprehensive set of requirements for hello various Site Recovery scenarios, refer toohello documentation pertaining toothat scenario.</span></span>

* <span data-ttu-id="fd34c-133">Hostitele Hyper-V se systémem Windows Server 2012 R2 nebo Microsoft Hyper-V Server 2012 R2, který obsahuje jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fd34c-133">A Hyper-V host running Windows Server 2012 R2 or Microsoft Hyper-V Server 2012 R2 containing one or more virtual machines.</span></span>
* <span data-ttu-id="fd34c-134">Servery Hyper-V připojen toohello Internetu, buď přímo nebo prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="fd34c-134">Hyper-V servers connected toohello Internet, either directly or through a proxy.</span></span>
* <span data-ttu-id="fd34c-135">Hello virtuálních počítačů má tooprotect musí být v souladu s [požadavky virtuálního počítače](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="fd34c-135">hello virtual machines you want tooprotect should conform with [Virtual Machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

## <a name="step-1-sign-in-tooyour-azure-account"></a><span data-ttu-id="fd34c-136">Krok 1: Zaregistrujte v tooyour účet Azure</span><span class="sxs-lookup"><span data-stu-id="fd34c-136">Step 1: Sign in tooyour Azure account</span></span>
1. <span data-ttu-id="fd34c-137">Otevřete konzolu prostředí PowerShell a spusťte tento příkaz toosign v tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="fd34c-137">Open a PowerShell console and run this command toosign in tooyour Azure account.</span></span> <span data-ttu-id="fd34c-138">rutiny Hello otevře na webové stránce, který zobrazí výzvu k zadání přihlašovacích údajů účtu.</span><span class="sxs-lookup"><span data-stu-id="fd34c-138">hello cmdlet brings up a web page that will prompt you for your account credentials.</span></span>

        Login-AzureRmAccount

    <span data-ttu-id="fd34c-139">Alternativně může také obsahovat svoje přihlašovací údaje jako parametr toohello `Login-AzureRmAccount` rutiny pomocí hello `-Credential` parametr.</span><span class="sxs-lookup"><span data-stu-id="fd34c-139">Alternately, you could also include your account credentials as a parameter toohello `Login-AzureRmAccount` cmdlet, by using hello `-Credential` parameter.</span></span>

    <span data-ttu-id="fd34c-140">Pokud jste poskytovatel CSP partnera práce jménem klienta, zadejte hello zákazníka jako klient, pomocí jejich název primární domény tenantID nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="fd34c-140">If you are CSP partner working on behalf of a tenant, specify hello customer as a tenant, by using their tenantID or tenant primary domain name.</span></span>

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. <span data-ttu-id="fd34c-141">Účet může mít několik odběrů, takže přidružíte hello předplatné chcete toouse účtem hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-141">An account can have several subscriptions, so you should associate hello subscription you want toouse with hello account.</span></span>

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. <span data-ttu-id="fd34c-142">Ověřte, zda je vaše předplatné registrované toouse hello Azure zprostředkovatele služeb zotavení a obnovení lokality pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="fd34c-142">Verify that your subscription is registered toouse hello Azure providers for Recovery Services and Site Recovery, by using hello following commands:</span></span>

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   <span data-ttu-id="fd34c-143">V hello výstup z těchto příkazů, pokud hello **RegistrationState** je nastaven příliš**registrovaná**, abyste mohli pokračovat tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="fd34c-143">In hello output of these commands, if hello **RegistrationState** is set too**Registered**, you can proceed tooStep 2.</span></span> <span data-ttu-id="fd34c-144">V opačném případě byste měli zaregistrovat hello chybějící zprostředkovatele v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="fd34c-144">If not, you should register hello missing provider in your subscription.</span></span>

   <span data-ttu-id="fd34c-145">tooregister hello zprostředkovatele Azure Site Recovery a služeb zotavení, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="fd34c-145">tooregister hello Azure provider for Site Recovery and Recovery Services, run hello following commands:</span></span>

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   <span data-ttu-id="fd34c-146">Ověří, zda hello zprostředkovatelé úspěšně registrován pomocí hello následující příkazy: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` a `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span><span class="sxs-lookup"><span data-stu-id="fd34c-146">Verify that hello providers registered successfully by using hello following commands: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` and `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span></span>

## <a name="step-2-set-up-hello-recovery-services-vault"></a><span data-ttu-id="fd34c-147">Krok 2: Nastavení hello trezor služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="fd34c-147">Step 2: Set up hello Recovery Services vault</span></span>
1. <span data-ttu-id="fd34c-148">Vytvořte skupinu prostředků Azure Resource Manager, ve kterém budete vytvoření trezoru hello nebo použít existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fd34c-148">Create an Azure Resource Manager resource group in which you'll create hello vault, or use an existing resource group.</span></span> <span data-ttu-id="fd34c-149">Můžete vytvořit novou skupinu prostředků s použitím hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fd34c-149">You can create a new resource group by using hello following command:</span></span>

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    <span data-ttu-id="fd34c-150">kde hello $ResourceGroupName proměnná obsahuje hello název skupiny prostředků hello chcete toocreate a proměnná hello $Geo obsahuje hello Azure oblast, ve které toocreate skupině prostředků hello (například "Brazílie – jih").</span><span class="sxs-lookup"><span data-stu-id="fd34c-150">where hello $ResourceGroupName variable contains hello name of hello resource group you want toocreate, and hello $Geo variable contains hello Azure region in which toocreate hello resource group (for example, "Brazil South").</span></span>

    <span data-ttu-id="fd34c-151">Seznam skupin prostředků v rámci vašeho předplatného můžete získat pomocí hello `Get-AzureRmResourceGroup` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fd34c-151">You can obtain a list of resource groups in your subscription by using hello `Get-AzureRmResourceGroup` cmdlet.</span></span>
2. <span data-ttu-id="fd34c-152">Vytvořte nový trezor služeb zotavení Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-152">Create a new Azure Recovery Services vault as follows:</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    <span data-ttu-id="fd34c-153">Seznam trezorů existující můžete načíst pomocí hello `Get-AzureRmRecoveryServicesVault` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fd34c-153">You can retrieve a list of existing vaults by using hello `Get-AzureRmRecoveryServicesVault` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="fd34c-154">Pokud chcete tooperform operací na trezorů Site Recovery vytvořené pomocí portálu classic hello nebo modulu Azure Service Management PowerShell text hello, můžete načíst seznam takové trezorů pomocí hello `Get-AzureRmSiteRecoveryVault` rutiny.</span><span class="sxs-lookup"><span data-stu-id="fd34c-154">If you wish tooperform operations on Site Recovery vaults created using hello classic portal or hello Azure Service Management PowerShell module, you can retrieve a list of such vaults by using hello `Get-AzureRmSiteRecoveryVault` cmdlet.</span></span> <span data-ttu-id="fd34c-155">Měli byste vytvořit nový trezor služeb zotavení pro všechny nové operace.</span><span class="sxs-lookup"><span data-stu-id="fd34c-155">You should create a new Recovery Services vault for all new operations.</span></span> <span data-ttu-id="fd34c-156">Hello trezorů Site Recovery, který jste dříve vytvořili podporují, ale nemáte hello nejnovější funkce.</span><span class="sxs-lookup"><span data-stu-id="fd34c-156">hello Site Recovery vaults you've created earlier are supported, but don't have hello latest features.</span></span>
>
>

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="fd34c-157">Krok 3: Nastavte hello kontextu trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="fd34c-157">Step 3: Set hello Recovery Services vault context</span></span>
1. <span data-ttu-id="fd34c-158">Nastavit kontext hello trezoru spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fd34c-158">Set hello vault context by running hello following command:</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-hello-site"></a><span data-ttu-id="fd34c-159">Krok 4: Vytvoření serveru technologie Hyper-V a vygenerovat nový registrační klíč trezoru pro lokalitu hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-159">Step 4: Create a Hyper-V site and generate a new vault registration key for hello site.</span></span>
1. <span data-ttu-id="fd34c-160">Vytvoří nový web s technologií Hyper-V následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-160">Create a new Hyper-V site as follows:</span></span>

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    <span data-ttu-id="fd34c-161">Tato rutina se spustí lokality hello toocreate úlohy Site Recovery a vrátí objekt úlohy Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fd34c-161">This cmdlet starts a Site Recovery job toocreate hello site, and returns a Site Recovery job object.</span></span> <span data-ttu-id="fd34c-162">Počkejte toocomplete hello úlohy a ověřte, že hello úloha úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="fd34c-162">Wait for hello job toocomplete and verify that hello job completed successfully.</span></span>

    <span data-ttu-id="fd34c-163">Můžete načíst objekt úlohy hello a tím i kontrola hello aktuální stav úlohy hello, pomocí rutiny Get-AzureRmSiteRecoveryJob hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-163">You can retrieve hello job object, and thereby check hello current status of hello job, by using hello Get-AzureRmSiteRecoveryJob cmdlet.</span></span>
2. <span data-ttu-id="fd34c-164">Vygenerování a stažení registrační klíč pro lokalitu hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-164">Generate and download a registration key for hello site, as follows:</span></span>

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    <span data-ttu-id="fd34c-165">Kopírování hello stáhnout hostitele klíče toohello technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="fd34c-165">Copy hello downloaded key toohello Hyper-V host.</span></span> <span data-ttu-id="fd34c-166">Je nutné hello klíče tooregister hello technologie Hyper-V hostiteli toohello lokality.</span><span class="sxs-lookup"><span data-stu-id="fd34c-166">You need hello key tooregister hello Hyper-V host toohello site.</span></span>

## <a name="step-5-install-hello-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a><span data-ttu-id="fd34c-167">Krok 5: Instalace hello zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na vašem hostiteli Hyper-V</span><span class="sxs-lookup"><span data-stu-id="fd34c-167">Step 5: Install hello Azure Site Recovery provider and Azure Recovery Services Agent on your Hyper-V host</span></span>
1. <span data-ttu-id="fd34c-168">Stažení instalačního programu hello hello nejnovější verzi poskytovatele hello z [Microsoft](https://aka.ms/downloaddra).</span><span class="sxs-lookup"><span data-stu-id="fd34c-168">Download hello installer for hello latest version of hello provider from [Microsoft](https://aka.ms/downloaddra).</span></span>
2. <span data-ttu-id="fd34c-169">Instalační program spusťte hello na vašem hostiteli Hyper-V a na konci hello hello instalace pokračovat v kroku toohello registrace.</span><span class="sxs-lookup"><span data-stu-id="fd34c-169">Run hello installer on your Hyper-V host, and at hello end of hello installation continue toohello registration step.</span></span>
3. <span data-ttu-id="fd34c-170">Po zobrazení výzvy zadejte, že hello stáhli lokality registrační klíč a dokončení registrace web toohello hostitele Hyper-V hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-170">When prompted, provide hello downloaded site registration key, and complete registration of hello Hyper-V host toohello site.</span></span>
4. <span data-ttu-id="fd34c-171">Ověřte, že je tento hostitel hello technologie Hyper-V lokality registrované toohello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fd34c-171">Verify that hello Hyper-V host is registered toohello site by using hello following command:</span></span>

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-hello-protection-container"></a><span data-ttu-id="fd34c-172">Krok 6: Vytvoření zásady replikace a přidružte ji k kontejneru ochrany hello</span><span class="sxs-lookup"><span data-stu-id="fd34c-172">Step 6: Create a replication policy and associate it with hello protection container</span></span>
1. <span data-ttu-id="fd34c-173">Vytvořte zásadu replikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-173">Create a replication policy as follows:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    <span data-ttu-id="fd34c-174">Kontrola hello vrátil tooensure úlohu, která byla úspěšná, vytvoření zásad replikace hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-174">Check hello returned job tooensure that hello replication policy creation succeeds.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="fd34c-175">Hello zadaný účet úložiště by měl být ve stejné oblasti Azure jako trezor služeb zotavení hello a musí mít povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="fd34c-175">hello storage account specified should be in hello same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="fd34c-176">Pokud hello zadaný účet úložiště je typu Azure Storage (klasické) pro obnovení, převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (klasické).</span><span class="sxs-lookup"><span data-stu-id="fd34c-176">If hello specified Recovery storage account is of type Azure Storage (Classic), failover of hello protected machines recover hello machine tooAzure IaaS (Classic).</span></span>
   > * <span data-ttu-id="fd34c-177">Pokud hello zadané obnovení účtu úložiště je typu úložiště Azure (Azure Resource Manager), převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="fd34c-177">If hello specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of hello protected machines recover hello machine tooAzure IaaS (Azure Resource Manager).</span></span>
   >
   >
2. <span data-ttu-id="fd34c-178">Získáte hello ochrany kontejneru odpovídající toohello lokality, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-178">Get hello protection container corresponding toohello site, as follows:</span></span>

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. <span data-ttu-id="fd34c-179">Začněte hello přidružení kontejneru ochrany hello hello zásady replikace, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-179">Start hello association of hello protection container with hello replication policy, as follows:</span></span>

     <span data-ttu-id="fd34c-180">$Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-zásady $Policy - PrimaryProtectionContainer $protectionContainer</span><span class="sxs-lookup"><span data-stu-id="fd34c-180">$Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer</span></span>

   <span data-ttu-id="fd34c-181">Počkejte hello přidružení úlohy toocomplete a ujistěte se, že byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="fd34c-181">Wait for hello association job toocomplete, and ensure that it completed successfully.</span></span>

## <a name="step-7-enable-protection-for-virtual-machines"></a><span data-ttu-id="fd34c-182">Krok 7: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="fd34c-182">Step 7: Enable protection for virtual machines</span></span>
1. <span data-ttu-id="fd34c-183">Získáte hello ochrany entity odpovídající toohello virtuálních počítačů, které chcete tooprotect, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-183">Get hello protection entity corresponding toohello VM you want tooprotect, as follows:</span></span>

        $VMFriendlyName = "Fabrikam-app"                    #Name of hello VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. <span data-ttu-id="fd34c-184">Začněte s ochranou hello virtuálního počítače, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-184">Start protecting hello virtual machine, as follows:</span></span>

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > <span data-ttu-id="fd34c-185">Hello zadaný účet úložiště by měl být ve stejné oblasti Azure jako trezor služeb zotavení hello a musí mít povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="fd34c-185">hello storage account specified should be in hello same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="fd34c-186">Pokud hello zadaný účet úložiště je typu Azure Storage (klasické) pro obnovení, převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (klasické).</span><span class="sxs-lookup"><span data-stu-id="fd34c-186">If hello specified Recovery storage account is of type Azure Storage (Classic), failover of hello protected machines recover hello machine tooAzure IaaS (Classic).</span></span>
   > * <span data-ttu-id="fd34c-187">Pokud hello zadané obnovení účtu úložiště je typu úložiště Azure (Azure Resource Manager), převzetí služeb při selhání hello chráněné počítače obnovit hello počítač tooAzure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="fd34c-187">If hello specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of hello protected machines recover hello machine tooAzure IaaS (Azure Resource Manager).</span></span>
   >
   > <span data-ttu-id="fd34c-188">Pokud hello chráníte virtuální počítač má více než jeden disk připojené tooit, zadejte hello disk operačního systému pomocí hello *OSDiskName* parametr.</span><span class="sxs-lookup"><span data-stu-id="fd34c-188">If hello VM you are protecting has more than one disk attached tooit, specify hello operating system disk by using hello *OSDiskName* parameter.</span></span>
   >
   >
3. <span data-ttu-id="fd34c-189">Počkejte tooreach hello virtuální počítače chráněné stavu po počáteční replikaci hello.</span><span class="sxs-lookup"><span data-stu-id="fd34c-189">Wait for hello virtual machines tooreach a protected state after hello initial replication.</span></span> <span data-ttu-id="fd34c-190">To může nějakou dobu trvat, v závislosti na faktorech jako hello množství dat toobe replikovat a hello tooAzure dostupnou šířku pásma nadřazený.</span><span class="sxs-lookup"><span data-stu-id="fd34c-190">This can take a while, depending on factors such as hello amount of data toobe replicated and hello available upstream bandwidth tooAzure.</span></span> <span data-ttu-id="fd34c-191">Stav úlohy Hello a StateDescription jsou aktualizované následujícím způsobem, při hello dosažení chráněném stavu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fd34c-191">hello job State and StateDescription are updated as follows, upon hello VM reaching a protected state.</span></span>

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. <span data-ttu-id="fd34c-192">Aktualizujte vlastnosti, obnovení, například hello velikost role virtuálního počítače a hello síť Azure tooattach hello virtuálního počítače síťových rozhraní karty tooupon převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="fd34c-192">Update recovery properties, such as hello VM role size, and hello Azure network tooattach hello virtual machine's network interface cards tooupon failover.</span></span>

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update hello virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a><span data-ttu-id="fd34c-193">Krok 8: Spustit testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="fd34c-193">Step 8: Run a test failover</span></span>
1. <span data-ttu-id="fd34c-194">Testovací převzetí služeb při selhání úlohy, spusťte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-194">Run a test failover job, as follows:</span></span>

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. <span data-ttu-id="fd34c-195">Ověřte testu hello virtuálních počítačů je vytvořena v Azure.</span><span class="sxs-lookup"><span data-stu-id="fd34c-195">Verify that hello test VM is created in Azure.</span></span> <span data-ttu-id="fd34c-196">(hello testovací převzetí služeb při selhání úlohy je pozastaven, po vytvoření hello testovací virtuální počítač v Azure.</span><span class="sxs-lookup"><span data-stu-id="fd34c-196">(hello test failover job is suspended, after creating hello test VM in Azure.</span></span> <span data-ttu-id="fd34c-197">Hello dokončení úlohy tak, že vyčistí rušivé vlivy hello vytvořen při obnovení úlohy hello, jak je ukázáno v dalším kroku hello.)</span><span class="sxs-lookup"><span data-stu-id="fd34c-197">hello job completes by cleaning up hello created artefacts upon resuming hello job, as illustrated in hello next step.)</span></span>
3. <span data-ttu-id="fd34c-198">Dokončení hello testovací převzetí služeb při selhání, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="fd34c-198">Complete hello test failover, as follows:</span></span>

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a><span data-ttu-id="fd34c-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fd34c-199">Next Steps</span></span>
<span data-ttu-id="fd34c-200">[Další informace](https://msdn.microsoft.com/library/azure/mt637930.aspx) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fd34c-200">[Read more](https://msdn.microsoft.com/library/azure/mt637930.aspx) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
