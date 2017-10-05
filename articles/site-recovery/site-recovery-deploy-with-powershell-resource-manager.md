---
title: "Replikace virtuálních počítačů technologie Hyper-V přes PowerShell a Azure Resource Manager | Microsoft Docs"
description: "Automatizovat replikaci virtuálních počítačů Hyper-V do Azure s Azure Site Recovery pomocí Powershellu a Azure Resource Manager."
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
ms.openlocfilehash: dbd562b73b0caecd0feb993bd6fb796dddb0438c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="983c5-103">Replikaci mezi místními technologie Hyper-V virtuální počítače a Azure pomocí prostředí PowerShell a Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="983c5-103">Replicate between on-premises Hyper-V virtual machines and Azure by using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="983c5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="983c5-104">Azure Portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="983c5-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="983c5-105">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
> * [<span data-ttu-id="983c5-106">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="983c5-106">Classic Portal</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a><span data-ttu-id="983c5-107">Přehled</span><span class="sxs-lookup"><span data-stu-id="983c5-107">Overview</span></span>
<span data-ttu-id="983c5-108">Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii obnovení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení.</span><span class="sxs-lookup"><span data-stu-id="983c5-108">Azure Site Recovery contributes to your business continuity and disaster recovery strategy by orchestrating replication, failover, and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="983c5-109">Úplný seznam scénářů nasazení, najdete v článku [přehled Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="983c5-109">For a full list of deployment scenarios, see the [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="983c5-110">Prostředí Azure PowerShell je modul, který poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="983c5-110">Azure PowerShell is a module that provides cmdlets to manage Azure through Windows PowerShell.</span></span> <span data-ttu-id="983c5-111">Můžete pracovat se dva typy modulů: modul profilu Azure nebo modulu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="983c5-111">It can work with two types of modules: the Azure Profile module, or the Azure Resource Manager module.</span></span>

<span data-ttu-id="983c5-112">Rutiny prostředí PowerShell obnovení lokality pomocí prostředí Azure PowerShell pro Azure Resource Manager, můžete chránit a obnovit vaše servery v Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-112">Site Recovery PowerShell cmdlets, available with Azure PowerShell for Azure Resource Manager, help you protect and recover your servers in Azure.</span></span>

<span data-ttu-id="983c5-113">Tento článek popisuje, jak pomocí prostředí Windows PowerShell, společně s Azure Resource Manager, nasazení Site Recovery pro konfiguraci a orchestraci ochrany serveru do Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-113">This article describes how to use Windows PowerShell, together with Azure Resource Manager, to deploy Site Recovery to configure and orchestrate server protection to Azure.</span></span> <span data-ttu-id="983c5-114">Příklad používané v tomto článku se dozvíte, jak chránit, převzetí služeb při selhání a obnovení virtuálních počítačů na hostiteli technologie Hyper-V do Azure pomocí Azure PowerShell s Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="983c5-114">The example used in this article shows you how to protect, fail over, and recover virtual machines on a Hyper-V host to Azure, by using Azure PowerShell with Azure Resource Manager.</span></span>

> [!NOTE]
> <span data-ttu-id="983c5-115">Rutiny prostředí PowerShell obnovení lokality aktuálně umožňují konfigurovat následující: jednu lokalitu nástroje Virtual Machine Manager do jiného, lokalitu nástroje Virtual Machine Manager do Azure a Web Hyper-V do Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-115">The Site Recovery PowerShell cmdlets currently allow you to configure the following: one Virtual Machine Manager site to another, a Virtual Machine Manager site to Azure, and a Hyper-V site to Azure.</span></span>
>
>

<span data-ttu-id="983c5-116">Nemusíte být odborné prostředí PowerShell použít v tomto článku, ale potřebujete seznámit se základními koncepcemi, jako jsou moduly, rutiny a relace.</span><span class="sxs-lookup"><span data-stu-id="983c5-116">You don't need to be a PowerShell expert to use this article, but you do need to understand the basic concepts, such as modules, cmdlets, and sessions.</span></span> <span data-ttu-id="983c5-117">Další informace o prostředí Windows PowerShell najdete v tématu [Začínáme s prostředím Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span><span class="sxs-lookup"><span data-stu-id="983c5-117">For more information about Windows PowerShell, see [Getting Started with Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).</span></span>

<span data-ttu-id="983c5-118">Také další informace o [použití Azure Powershellu s Azure Resource Manager](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="983c5-118">You can also read more about [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>

> [!NOTE]
> <span data-ttu-id="983c5-119">Partneři Microsoftu, které jsou součástí programu Cloud Solution Provider (CSP) můžete konfigurovat a spravovat ochranu systému serverů se svým zákazníkům svým zákazníkům příslušného poskytovatele CSP předplatným (odběry klienta).</span><span class="sxs-lookup"><span data-stu-id="983c5-119">Microsoft partners that are part of the Cloud Solution Provider (CSP) program can configure and manage protection of their customers' servers to their customers' respective CSP subscriptions (tenant subscriptions).</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="983c5-120">Než začnete</span><span class="sxs-lookup"><span data-stu-id="983c5-120">Before you start</span></span>
<span data-ttu-id="983c5-121">Ujistěte se, že máte zavedenou tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="983c5-121">Make sure you have these prerequisites in place:</span></span>

* <span data-ttu-id="983c5-122">A [Microsoft Azure](https://azure.microsoft.com/) účtu.</span><span class="sxs-lookup"><span data-stu-id="983c5-122">A [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="983c5-123">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="983c5-123">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="983c5-124">Kromě toho si můžete přečíst o [cenách Azure Site Recovery Manageru](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="983c5-124">In addition, you can read about [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="983c5-125">Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="983c5-125">Azure PowerShell 1.0.</span></span> <span data-ttu-id="983c5-126">Informace o této verzi a k jeho instalaci najdete v tématu [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span><span class="sxs-lookup"><span data-stu-id="983c5-126">For information about this release and how to install it, see [Azure PowerShell 1.0.](https://azure.microsoft.com/)</span></span>
* <span data-ttu-id="983c5-127">[AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) a [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduly.</span><span class="sxs-lookup"><span data-stu-id="983c5-127">The [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) and [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modules.</span></span> <span data-ttu-id="983c5-128">Můžete získat nejnovější verze tyto moduly z [Galerie prostředí PowerShell](https://www.powershellgallery.com/)</span><span class="sxs-lookup"><span data-stu-id="983c5-128">You can get the latest versions of these modules from the [PowerShell gallery](https://www.powershellgallery.com/)</span></span>

<span data-ttu-id="983c5-129">Tento článek ukazuje, jak pomocí prostředí Azure Powershell s Azure Resource Manager ke konfiguraci a správě ochrany vašich serverů.</span><span class="sxs-lookup"><span data-stu-id="983c5-129">This article illustrates how to use Azure Powershell with Azure Resource Manager to configure and manage protection of your servers.</span></span> <span data-ttu-id="983c5-130">Příklad používané v tomto článku se dozvíte, jak k ochraně virtuálního počítače, spuštěná v hostiteli technologie Hyper-V do Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-130">The example used in this article shows you how to protect a virtual machine, running on a Hyper-V host, to Azure.</span></span> <span data-ttu-id="983c5-131">Následující požadované součásti jsou specifické pro tento příklad.</span><span class="sxs-lookup"><span data-stu-id="983c5-131">The following prerequisites are specific to this example.</span></span> <span data-ttu-id="983c5-132">Komplexnější sadu požadavků pro různé scénáře obnovení lokality naleznete v dokumentaci týkající se tento scénář.</span><span class="sxs-lookup"><span data-stu-id="983c5-132">For a more comprehensive set of requirements for the various Site Recovery scenarios, refer to the documentation pertaining to that scenario.</span></span>

* <span data-ttu-id="983c5-133">Hostitele Hyper-V se systémem Windows Server 2012 R2 nebo Microsoft Hyper-V Server 2012 R2, který obsahuje jeden nebo více virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="983c5-133">A Hyper-V host running Windows Server 2012 R2 or Microsoft Hyper-V Server 2012 R2 containing one or more virtual machines.</span></span>
* <span data-ttu-id="983c5-134">Servery Hyper-V připojený k Internetu, buď přímo nebo prostřednictvím proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="983c5-134">Hyper-V servers connected to the Internet, either directly or through a proxy.</span></span>
* <span data-ttu-id="983c5-135">Virtuální počítače, které chcete chránit musí být v souladu s [požadavky virtuálního počítače](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="983c5-135">The virtual machines you want to protect should conform with [Virtual Machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

## <a name="step-1-sign-in-to-your-azure-account"></a><span data-ttu-id="983c5-136">Krok 1: Přihlaste se k účtu Azure</span><span class="sxs-lookup"><span data-stu-id="983c5-136">Step 1: Sign in to your Azure account</span></span>
1. <span data-ttu-id="983c5-137">Otevřete konzolu prostředí PowerShell a spusťte tento příkaz k přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-137">Open a PowerShell console and run this command to sign in to your Azure account.</span></span> <span data-ttu-id="983c5-138">Rutina zobrazí webové stránky, která vás vyzve k zadání pověření účtu.</span><span class="sxs-lookup"><span data-stu-id="983c5-138">The cmdlet brings up a web page that will prompt you for your account credentials.</span></span>

        Login-AzureRmAccount

    <span data-ttu-id="983c5-139">Alternativně může také obsahovat přihlašovací údaje účtu jako parametr, který se `Login-AzureRmAccount` rutiny, pomocí `-Credential` parametr.</span><span class="sxs-lookup"><span data-stu-id="983c5-139">Alternately, you could also include your account credentials as a parameter to the `Login-AzureRmAccount` cmdlet, by using the `-Credential` parameter.</span></span>

    <span data-ttu-id="983c5-140">Pokud jste poskytovatel CSP partnera práce jménem klienta, zadejte zákazníka jako klient, pomocí jejich název primární domény tenantID nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="983c5-140">If you are CSP partner working on behalf of a tenant, specify the customer as a tenant, by using their tenantID or tenant primary domain name.</span></span>

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. <span data-ttu-id="983c5-141">Účet může mít několik odběrů, takže přidružíte odběr, který chcete používat s účtem.</span><span class="sxs-lookup"><span data-stu-id="983c5-141">An account can have several subscriptions, so you should associate the subscription you want to use with the account.</span></span>

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. <span data-ttu-id="983c5-142">Ověřte, že vaše předplatné je registrovaný k použití Azure zprostředkovatele služeb zotavení a obnovení lokality pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="983c5-142">Verify that your subscription is registered to use the Azure providers for Recovery Services and Site Recovery, by using the following commands:</span></span>

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   <span data-ttu-id="983c5-143">Ve výstupu z těchto příkazů Pokud **RegistrationState** je nastaven na **registrovaná**, můžete přejít ke kroku 2.</span><span class="sxs-lookup"><span data-stu-id="983c5-143">In the output of these commands, if the **RegistrationState** is set to **Registered**, you can proceed to Step 2.</span></span> <span data-ttu-id="983c5-144">V opačném případě byste měli zaregistrovat chybějící zprostředkovatele v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="983c5-144">If not, you should register the missing provider in your subscription.</span></span>

   <span data-ttu-id="983c5-145">Chcete-li zaregistrovat zprostředkovatele Azure Site Recovery a služeb zotavení, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="983c5-145">To register the Azure provider for Site Recovery and Recovery Services, run the following commands:</span></span>

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   <span data-ttu-id="983c5-146">Ověřte, že zprostředkovatele úspěšně zaregistrován pomocí následujících příkazů: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` a `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span><span class="sxs-lookup"><span data-stu-id="983c5-146">Verify that the providers registered successfully by using the following commands: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` and `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.</span></span>

## <a name="step-2-set-up-the-recovery-services-vault"></a><span data-ttu-id="983c5-147">Krok 2: Nastavení trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="983c5-147">Step 2: Set up the Recovery Services vault</span></span>
1. <span data-ttu-id="983c5-148">Vytvořte skupinu prostředků Azure Resource Manager, ve kterém budete vytvořit trezor nebo použít existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="983c5-148">Create an Azure Resource Manager resource group in which you'll create the vault, or use an existing resource group.</span></span> <span data-ttu-id="983c5-149">Pomocí následujícího příkazu můžete vytvořit novou skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="983c5-149">You can create a new resource group by using the following command:</span></span>

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    <span data-ttu-id="983c5-150">kde proměnnou $ResourceGroupName obsahuje název skupiny prostředků, kterou chcete vytvořit, a proměnná $Geo obsahuje oblast Azure, ve kterém chcete vytvořit skupinu prostředků (například "Brazílie – jih").</span><span class="sxs-lookup"><span data-stu-id="983c5-150">where the $ResourceGroupName variable contains the name of the resource group you want to create, and the $Geo variable contains the Azure region in which to create the resource group (for example, "Brazil South").</span></span>

    <span data-ttu-id="983c5-151">Seznam skupin prostředků v rámci vašeho předplatného můžete získat pomocí `Get-AzureRmResourceGroup` rutiny.</span><span class="sxs-lookup"><span data-stu-id="983c5-151">You can obtain a list of resource groups in your subscription by using the `Get-AzureRmResourceGroup` cmdlet.</span></span>
2. <span data-ttu-id="983c5-152">Vytvořte nový trezor služeb zotavení Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-152">Create a new Azure Recovery Services vault as follows:</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    <span data-ttu-id="983c5-153">Seznam trezorů existující můžete načíst pomocí `Get-AzureRmRecoveryServicesVault` rutiny.</span><span class="sxs-lookup"><span data-stu-id="983c5-153">You can retrieve a list of existing vaults by using the `Get-AzureRmRecoveryServicesVault` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="983c5-154">Pokud chcete k provádění operací na trezorů Site Recovery vytvořené pomocí portálu classic nebo modulu Azure Service Management PowerShell, můžete načíst seznam takové trezorů pomocí `Get-AzureRmSiteRecoveryVault` rutiny.</span><span class="sxs-lookup"><span data-stu-id="983c5-154">If you wish to perform operations on Site Recovery vaults created using the classic portal or the Azure Service Management PowerShell module, you can retrieve a list of such vaults by using the `Get-AzureRmSiteRecoveryVault` cmdlet.</span></span> <span data-ttu-id="983c5-155">Měli byste vytvořit nový trezor služeb zotavení pro všechny nové operace.</span><span class="sxs-lookup"><span data-stu-id="983c5-155">You should create a new Recovery Services vault for all new operations.</span></span> <span data-ttu-id="983c5-156">Trezorů Site Recovery, který jste dříve vytvořili podporují, ale nemají nejnovější funkce.</span><span class="sxs-lookup"><span data-stu-id="983c5-156">The Site Recovery vaults you've created earlier are supported, but don't have the latest features.</span></span>
>
>

## <a name="step-3-set-the-recovery-services-vault-context"></a><span data-ttu-id="983c5-157">Krok 3: Nastavte kontext trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="983c5-157">Step 3: Set the Recovery Services vault context</span></span>
1. <span data-ttu-id="983c5-158">Nastavte kontext trezoru spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="983c5-158">Set the vault context by running the following command:</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a><span data-ttu-id="983c5-159">Krok 4: Vytvoření serveru technologie Hyper-V a vygenerovat nový registrační klíč trezoru pro lokalitu.</span><span class="sxs-lookup"><span data-stu-id="983c5-159">Step 4: Create a Hyper-V site and generate a new vault registration key for the site.</span></span>
1. <span data-ttu-id="983c5-160">Vytvoří nový web s technologií Hyper-V následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-160">Create a new Hyper-V site as follows:</span></span>

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    <span data-ttu-id="983c5-161">Tato rutina se spustí úloha Site Recovery pro vytvoření webu a vrátí objekt úlohy Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="983c5-161">This cmdlet starts a Site Recovery job to create the site, and returns a Site Recovery job object.</span></span> <span data-ttu-id="983c5-162">Počkejte na dokončení a ověřte, zda úlohy úloha byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="983c5-162">Wait for the job to complete and verify that the job completed successfully.</span></span>

    <span data-ttu-id="983c5-163">Můžete načíst objekt úlohy a tím zkontrolujte aktuální stav úlohy, pomocí rutiny Get-AzureRmSiteRecoveryJob.</span><span class="sxs-lookup"><span data-stu-id="983c5-163">You can retrieve the job object, and thereby check the current status of the job, by using the Get-AzureRmSiteRecoveryJob cmdlet.</span></span>
2. <span data-ttu-id="983c5-164">Vygenerování a stažení registrační klíč pro lokalitu, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-164">Generate and download a registration key for the site, as follows:</span></span>

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    <span data-ttu-id="983c5-165">Zkopírujte stažený klíč na hostitele Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="983c5-165">Copy the downloaded key to the Hyper-V host.</span></span> <span data-ttu-id="983c5-166">Je nutné klíč k registraci hostitele technologie Hyper-V do lokality.</span><span class="sxs-lookup"><span data-stu-id="983c5-166">You need the key to register the Hyper-V host to the site.</span></span>

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a><span data-ttu-id="983c5-167">Krok 5: Instalace zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure na vašem hostiteli Hyper-V</span><span class="sxs-lookup"><span data-stu-id="983c5-167">Step 5: Install the Azure Site Recovery provider and Azure Recovery Services Agent on your Hyper-V host</span></span>
1. <span data-ttu-id="983c5-168">Stažení instalačního programu pro nejnovější verzi zprostředkovatele ze [Microsoft](https://aka.ms/downloaddra).</span><span class="sxs-lookup"><span data-stu-id="983c5-168">Download the installer for the latest version of the provider from [Microsoft](https://aka.ms/downloaddra).</span></span>
2. <span data-ttu-id="983c5-169">Spusťte instalační program na vašem hostiteli Hyper-V a na konci instalace pokračujte krokem registrace.</span><span class="sxs-lookup"><span data-stu-id="983c5-169">Run the installer on your Hyper-V host, and at the end of the installation continue to the registration step.</span></span>
3. <span data-ttu-id="983c5-170">Po zobrazení výzvy zadejte registrační klíč stažený lokality a dokončení registrace hostitele technologie Hyper-V do lokality.</span><span class="sxs-lookup"><span data-stu-id="983c5-170">When prompted, provide the downloaded site registration key, and complete registration of the Hyper-V host to the site.</span></span>
4. <span data-ttu-id="983c5-171">Ověřte, že hostitel Hyper-V je zaregistrován k lokalitě pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="983c5-171">Verify that the Hyper-V host is registered to the site by using the following command:</span></span>

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a><span data-ttu-id="983c5-172">Krok 6: Vytvoření zásady replikace a přidružte ji k kontejner ochrany</span><span class="sxs-lookup"><span data-stu-id="983c5-172">Step 6: Create a replication policy and associate it with the protection container</span></span>
1. <span data-ttu-id="983c5-173">Vytvořte zásadu replikace následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-173">Create a replication policy as follows:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    <span data-ttu-id="983c5-174">Zkontrolujte vrácený úlohu, která zajistí, že úspěšné vytvoření zásad replikace.</span><span class="sxs-lookup"><span data-stu-id="983c5-174">Check the returned job to ensure that the replication policy creation succeeds.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="983c5-175">Zadaný účet úložiště by měl být ve stejné oblasti jako trezor služeb zotavení Azure a musí mít povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="983c5-175">The storage account specified should be in the same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="983c5-176">Pokud je zadaný účet úložiště obnovení typ úložiště Azure (klasický), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (klasické).</span><span class="sxs-lookup"><span data-stu-id="983c5-176">If the specified Recovery storage account is of type Azure Storage (Classic), failover of the protected machines recover the machine to Azure IaaS (Classic).</span></span>
   > * <span data-ttu-id="983c5-177">Pokud je zadaný účet úložiště obnovení typ úložiště Azure (Azure Resource Manager), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="983c5-177">If the specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of the protected machines recover the machine to Azure IaaS (Azure Resource Manager).</span></span>
   >
   >
2. <span data-ttu-id="983c5-178">Získání kontejneru ochrany odpovídající k lokalitě, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-178">Get the protection container corresponding to the site, as follows:</span></span>

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. <span data-ttu-id="983c5-179">Spusťte přidružení kontejner ochrany zásadám replikace, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-179">Start the association of the protection container with the replication policy, as follows:</span></span>

     <span data-ttu-id="983c5-180">$Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-zásady $Policy - PrimaryProtectionContainer $protectionContainer</span><span class="sxs-lookup"><span data-stu-id="983c5-180">$Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName   $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer</span></span>

   <span data-ttu-id="983c5-181">Počkejte na dokončení úlohy přidružení a ujistěte se, že byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="983c5-181">Wait for the association job to complete, and ensure that it completed successfully.</span></span>

## <a name="step-7-enable-protection-for-virtual-machines"></a><span data-ttu-id="983c5-182">Krok 7: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="983c5-182">Step 7: Enable protection for virtual machines</span></span>
1. <span data-ttu-id="983c5-183">Získáte entita ochrany odpovídající virtuálních počítačů, které chcete chránit, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-183">Get the protection entity corresponding to the VM you want to protect, as follows:</span></span>

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. <span data-ttu-id="983c5-184">Začněte s ochranou virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-184">Start protecting the virtual machine, as follows:</span></span>

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > <span data-ttu-id="983c5-185">Zadaný účet úložiště by měl být ve stejné oblasti jako trezor služeb zotavení Azure a musí mít povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="983c5-185">The storage account specified should be in the same Azure region as your Recovery Services vault, and should have geo-replication enabled.</span></span>
   >
   > * <span data-ttu-id="983c5-186">Pokud je zadaný účet úložiště obnovení typ úložiště Azure (klasický), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (klasické).</span><span class="sxs-lookup"><span data-stu-id="983c5-186">If the specified Recovery storage account is of type Azure Storage (Classic), failover of the protected machines recover the machine to Azure IaaS (Classic).</span></span>
   > * <span data-ttu-id="983c5-187">Pokud je zadaný účet úložiště obnovení typ úložiště Azure (Azure Resource Manager), převzetí služeb při selhání chráněných počítačů obnovit počítače Azure IaaS (Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="983c5-187">If the specified Recovery storage account is of type Azure Storage (Azure Resource Manager), failover of the protected machines recover the machine to Azure IaaS (Azure Resource Manager).</span></span>
   >
   > <span data-ttu-id="983c5-188">Pokud chráníte virtuální počítač má více než jeden disk připojený k němu, zadejte disku operačního systému pomocí *OSDiskName* parametr.</span><span class="sxs-lookup"><span data-stu-id="983c5-188">If the VM you are protecting has more than one disk attached to it, specify the operating system disk by using the *OSDiskName* parameter.</span></span>
   >
   >
3. <span data-ttu-id="983c5-189">Počkejte, než pro virtuální počítače dosáhne chráněném stavu po počáteční replikaci.</span><span class="sxs-lookup"><span data-stu-id="983c5-189">Wait for the virtual machines to reach a protected state after the initial replication.</span></span> <span data-ttu-id="983c5-190">To může nějakou dobu trvat, v závislosti na faktorech, jako je množství dat, které se budou replikovat a dostupnou šířku pásma odesílání dat do Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-190">This can take a while, depending on factors such as the amount of data to be replicated and the available upstream bandwidth to Azure.</span></span> <span data-ttu-id="983c5-191">Stav úlohy a StateDescription jsou aktualizované následujícím způsobem, při dosažení chráněném stavu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="983c5-191">The job State and StateDescription are updated as follows, upon the VM reaching a protected state.</span></span>

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. <span data-ttu-id="983c5-192">Aktualizujte vlastnosti, obnovení, například velikost role virtuálního počítače a síť Azure připojit virtuální počítač karty síťového rozhraní na po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="983c5-192">Update recovery properties, such as the VM role size, and the Azure network to attach the virtual machine's network interface cards to upon failover.</span></span>

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
        DisplayName      : Update the virtual machine
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



## <a name="step-8-run-a-test-failover"></a><span data-ttu-id="983c5-193">Krok 8: Spustit testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="983c5-193">Step 8: Run a test failover</span></span>
1. <span data-ttu-id="983c5-194">Testovací převzetí služeb při selhání úlohy, spusťte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-194">Run a test failover job, as follows:</span></span>

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. <span data-ttu-id="983c5-195">Ověřte, že testovací virtuální počítač je vytvořen v Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-195">Verify that the test VM is created in Azure.</span></span> <span data-ttu-id="983c5-196">(Testovací převzetí služeb při selhání úlohy je pozastaven, po vytvoření testovacího virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="983c5-196">(The test failover job is suspended, after creating the test VM in Azure.</span></span> <span data-ttu-id="983c5-197">Dokončení úlohy tak, že vyčistí vytvořený rušivé vlivy při obnovení úlohy, jak je ukázáno v dalším kroku.)</span><span class="sxs-lookup"><span data-stu-id="983c5-197">The job completes by cleaning up the created artefacts upon resuming the job, as illustrated in the next step.)</span></span>
3. <span data-ttu-id="983c5-198">Dokončete testovací převzetí služeb při selhání, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="983c5-198">Complete the test failover, as follows:</span></span>

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a><span data-ttu-id="983c5-199">Další kroky</span><span class="sxs-lookup"><span data-stu-id="983c5-199">Next Steps</span></span>
<span data-ttu-id="983c5-200">[Další informace](https://msdn.microsoft.com/library/azure/mt637930.aspx) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="983c5-200">[Read more](https://msdn.microsoft.com/library/azure/mt637930.aspx) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
