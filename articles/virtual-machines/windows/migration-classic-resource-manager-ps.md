---
title: "Migrace do Správce prostředků v prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede migraci podporované platformy IaaS prostředků, jako jsou virtuální počítače (VM), virtuální sítě (virtuální sítě) a účty úložiště z klasického do Azure Resource Manager (ARM) pomocí příkazů prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 489e6cc6bd3c5b36635f5f7e398d08fed681d2e7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a><span data-ttu-id="d4a68-103">Migrovat prostředky infrastruktury z klasického do Azure Resource Manageru pomocí prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4a68-103">Migrate IaaS resources from classic to Azure Resource Manager by using Azure PowerShell</span></span>
<span data-ttu-id="d4a68-104">Tyto kroky ukazují, jak používat příkazy Azure PowerShell k migraci infrastruktury jako služby (IaaS) prostředky z modelu nasazení classic do modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4a68-104">These steps show you how to use Azure PowerShell commands to migrate infrastructure as a service (IaaS) resources from the classic deployment model to the Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="d4a68-105">Pokud chcete, můžete také migrovat prostředky pomocí [rozhraní příkazového řádku Azure (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d4a68-105">If you want, you can also migrate resources by using the [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span></span>

* <span data-ttu-id="d4a68-106">Pro informace o podporovaných scénářů migrace, viz [platformy podporované migrace z klasického do Azure Resource Manageru prostředků IaaS](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4a68-106">For background on supported migration scenarios, see [Platform-supported migration of IaaS resources from classic to Azure Resource Manager](migration-classic-resource-manager-overview.md).</span></span>
* <span data-ttu-id="d4a68-107">Podrobné pokyny a migrace návod najdete v tématu [technické podrobné informace o platformy podporované migrace z klasického do Azure Resource Manageru](migration-classic-resource-manager-deep-dive.md).</span><span class="sxs-lookup"><span data-stu-id="d4a68-107">For detailed guidance and a migration walkthrough, see [Technical deep dive on platform-supported migration from classic to Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span></span>
* [<span data-ttu-id="d4a68-108">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="d4a68-108">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md)

<br>
<span data-ttu-id="d4a68-109">Tady je Vývojový diagram k identifikaci pořadí, ve kterém kroky se musí provést během procesu migrace</span><span class="sxs-lookup"><span data-stu-id="d4a68-109">Here is a flowchart to identify the order in which steps need to be executed during a migration process</span></span>

![Snímek obrazovky, který ukazuje kroky migrace](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a><span data-ttu-id="d4a68-111">Krok 1: Plánování migrace</span><span class="sxs-lookup"><span data-stu-id="d4a68-111">Step 1: Plan for migration</span></span>
<span data-ttu-id="d4a68-112">Tady je několik osvědčených postupů, které doporučujeme jak vyhodnotit migrace IaaS prostředky z classic do Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="d4a68-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic to Resource Manager:</span></span>

* <span data-ttu-id="d4a68-113">Pročtěte [podporované a nepodporované funkce a konfigurace](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4a68-113">Read through the [supported and unsupported features and configurations](migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="d4a68-114">Pokud máte virtuální počítače, které používají nepodporované konfigurace nebo funkce, doporučujeme, abyste počkali podpory konfigurace nebo funkce, která má být oznámeno.</span><span class="sxs-lookup"><span data-stu-id="d4a68-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for the configuration/feature support to be announced.</span></span> <span data-ttu-id="d4a68-115">Případně pokud ji vyhovuje vašim potřebám, odeberte tuto funkci nebo přesunout mimo tuto konfiguraci, chcete-li migrovat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-115">Alternatively, if it suits your needs, remove that feature or move out of that configuration to enable migration.</span></span>
* <span data-ttu-id="d4a68-116">Pokud máte automatizované skripty, které jsou dnes nasazení infrastruktury a aplikace, pokuste se vytvořit podobné nastavení testu pomocí těchto skriptů pro migraci.</span><span class="sxs-lookup"><span data-stu-id="d4a68-116">If you have automated scripts that deploy your infrastructure and applications today, try to create a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="d4a68-117">Alternativně můžete nastavit ukázkové prostředí pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-117">Alternatively, you can set up sample environments by using the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d4a68-118">Application Gateway nejsou aktuálně podporovány pro migraci z classic do Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4a68-118">Application Gateways are not currently supported for migration from classic to Resource Manager.</span></span> <span data-ttu-id="d4a68-119">Pokud chcete migrovat klasickou virtuální síť s aplikační brány, odstranění brány před spuštěním operace Příprava přesunout sítě.</span><span class="sxs-lookup"><span data-stu-id="d4a68-119">To migrate a classic virtual network with an Application gateway, remove the gateway before running a Prepare operation to move the network.</span></span> <span data-ttu-id="d4a68-120">Po dokončení migrace znovu připojte bránu ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-120">After you complete the migration, reconnect the gateway in Azure Resource Manager.</span></span>
>
><span data-ttu-id="d4a68-121">Připojování k okruhy ExpressRoute v jiné předplatné brány ExpressRoute se nedají automaticky migrovat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-121">ExpressRoute gateways connecting to ExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="d4a68-122">V takových případech odebrat bránu ExpressRoute, migrujte virtuální sítě a znovu vytvořit bránu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-122">In such cases, remove the ExpressRoute gateway, migrate the virtual network and recreate the gateway.</span></span> <span data-ttu-id="d4a68-123">Najdete v tématu [okruhy ExpressRoute migrovat a přidružené virtuální sítě z klasického modelu nasazení Resource Manager](../../expressroute/expressroute-migration-classic-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d4a68-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from the classic to the Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
>
>

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a><span data-ttu-id="d4a68-124">Krok 2: Nainstalujte nejnovější verzi prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4a68-124">Step 2: Install the latest version of Azure PowerShell</span></span>
<span data-ttu-id="d4a68-125">Existují dvě hlavní možnosti nainstalujte prostředí Azure PowerShell: [Galerie prostředí PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) nebo [instalačního programu webové platformy (WebPI)](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="d4a68-125">There are two main options to install Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) or [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="d4a68-126">WebPI obdrží měsíčních aktualizací.</span><span class="sxs-lookup"><span data-stu-id="d4a68-126">WebPI receives monthly updates.</span></span> <span data-ttu-id="d4a68-127">Galerie prostředí PowerShell získává aktualizace trvale.</span><span class="sxs-lookup"><span data-stu-id="d4a68-127">PowerShell Gallery receives updates on a continuous basis.</span></span> <span data-ttu-id="d4a68-128">Tento článek je založená na prostředí Azure PowerShell verze 2.1.0.</span><span class="sxs-lookup"><span data-stu-id="d4a68-128">This article is based on Azure PowerShell version 2.1.0.</span></span>

<span data-ttu-id="d4a68-129">Pokyny k instalaci naleznete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d4a68-129">For installation instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a><span data-ttu-id="d4a68-130">Krok 3: Ujistěte se, že jste správce pro předplatné na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d4a68-130">Step 3: Ensure that you are an administrator for the subscription in Azure portal</span></span>
<span data-ttu-id="d4a68-131">Jak tuto migraci provést, je třeba přidat jako spolusprávce pro předplatné ve [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d4a68-131">To perform this migration, you must be added as a co-administrator for the subscription in the [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="d4a68-132">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d4a68-132">Sign into the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d4a68-133">V nabídce centra vyberte **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-133">On the Hub menu, select **Subscription**.</span></span> <span data-ttu-id="d4a68-134">Pokud ho nevidíte, vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-134">If you don't see it, select **More services**.</span></span>
3. <span data-ttu-id="d4a68-135">Najít položku odpovídající předplatné a podívejte se na **Moje ROLE** pole.</span><span class="sxs-lookup"><span data-stu-id="d4a68-135">Find the appropriate subscription entry, then look at the **MY ROLE** field.</span></span> <span data-ttu-id="d4a68-136">Pro správce a společné hodnota musí být _správce účtu_.</span><span class="sxs-lookup"><span data-stu-id="d4a68-136">For a co-administrator, the value should be _Account admin_.</span></span>

<span data-ttu-id="d4a68-137">Pokud si nejste moct přidávat společné správce, kontaktujte správce služeb nebo spolusprávce pro předplatné získat sami přidat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-137">If you are not able to add a co-administrator, then contact a service administrator or co-administrator for the subscription to get yourself added.</span></span>   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a><span data-ttu-id="d4a68-138">Krok 4: Nastavte předplatné a zaregistrujte se pro migraci</span><span class="sxs-lookup"><span data-stu-id="d4a68-138">Step 4: Set your subscription and sign up for migration</span></span>
<span data-ttu-id="d4a68-139">Nejprve spusťte příkazovém řádku prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d4a68-139">First, start a PowerShell prompt.</span></span> <span data-ttu-id="d4a68-140">Pro migraci, budete muset nastavit svoje prostředí pro obě classic a Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4a68-140">For migration, you need to set up your environment for both classic and Resource Manager.</span></span>

<span data-ttu-id="d4a68-141">Přihlaste se ke svému účtu pro model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d4a68-141">Sign in to your account for the Resource Manager model.</span></span>

```powershell
    Login-AzureRmAccount
```

<span data-ttu-id="d4a68-142">Získáte dostupných předplatných pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-142">Get the available subscriptions by using the following command:</span></span>

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

<span data-ttu-id="d4a68-143">Nastavte předplatné Azure pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="d4a68-143">Set your Azure subscription for the current session.</span></span> <span data-ttu-id="d4a68-144">Tento příklad nastaví výchozí název odběru **Moje předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-144">This example sets the default subscription name to **My Azure Subscription**.</span></span> <span data-ttu-id="d4a68-145">Nahraďte název odběru příklad vlastní.</span><span class="sxs-lookup"><span data-stu-id="d4a68-145">Replace the example subscription name with your own.</span></span>

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> <span data-ttu-id="d4a68-146">Registrace je jednorázové krok, ale musíte to provést jednou před pokusem o migraci.</span><span class="sxs-lookup"><span data-stu-id="d4a68-146">Registration is a one-time step, but you must do it once before attempting migration.</span></span> <span data-ttu-id="d4a68-147">Bez registrace, zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d4a68-147">Without registering, you see the following error message:</span></span>
>
> <span data-ttu-id="d4a68-148">*Struktura BadRequest: Předplatné není zaregistrované pro migraci.*</span><span class="sxs-lookup"><span data-stu-id="d4a68-148">*BadRequest : Subscription is not registered for migration.*</span></span>
>
>

<span data-ttu-id="d4a68-149">Zaregistrovat u zprostředkovatele prostředků migrace pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-149">Register with the migration resource provider by using the following command:</span></span>

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="d4a68-150">Počkejte 5 minut pro registraci dokončit.</span><span class="sxs-lookup"><span data-stu-id="d4a68-150">Please wait five minutes for the registration to finish.</span></span> <span data-ttu-id="d4a68-151">Pomocí následujícího příkazu můžete zkontrolovat stav schválení:</span><span class="sxs-lookup"><span data-stu-id="d4a68-151">You can check the status of the approval by using the following command:</span></span>

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="d4a68-152">Ujistěte se, že je RegistrationState `Registered` než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-152">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

<span data-ttu-id="d4a68-153">Přihlaste se teď ke svému účtu pro klasického modelu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-153">Now sign in to your account for the classic model.</span></span>

```powershell
    Add-AzureAccount
```

<span data-ttu-id="d4a68-154">Získáte dostupných předplatných pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-154">Get the available subscriptions by using the following command:</span></span>

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

<span data-ttu-id="d4a68-155">Nastavte předplatné Azure pro aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="d4a68-155">Set your Azure subscription for the current session.</span></span> <span data-ttu-id="d4a68-156">Tento příklad nastaví výchozí odběr do **Moje předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-156">This example sets the default subscription to **My Azure Subscription**.</span></span> <span data-ttu-id="d4a68-157">Nahraďte název odběru příklad vlastní.</span><span class="sxs-lookup"><span data-stu-id="d4a68-157">Replace the example subscription name with your own.</span></span>

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="d4a68-158">Krok 5: Zkontrolujte, zda že máte dostatečný počet jader virtuálního počítače Azure Resource Manager v oblasti Azure vaše aktuální nasazení nebo virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d4a68-158">Step 5: Make sure you have enough Azure Resource Manager Virtual Machine cores in the Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="d4a68-159">Následující příkaz prostředí PowerShell slouží ke kontrole aktuální počet jader, které máte ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-159">You can use the following PowerShell command to check the current number of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="d4a68-160">Další informace o základní kvóty, najdete v části [omezení a Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span><span class="sxs-lookup"><span data-stu-id="d4a68-160">To learn more about core quotas, see [Limits and the Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span></span>

<span data-ttu-id="d4a68-161">Tento příklad zkontroluje dostupnost **západní USA** oblast.</span><span class="sxs-lookup"><span data-stu-id="d4a68-161">This example checks the availability in the **West US** region.</span></span> <span data-ttu-id="d4a68-162">Příklad názvu oblasti nahraďte vlastními.</span><span class="sxs-lookup"><span data-stu-id="d4a68-162">Replace the example region name with your own.</span></span>

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a><span data-ttu-id="d4a68-163">Krok 6: Spuštění příkazů pro migraci vašich prostředků IaaS</span><span class="sxs-lookup"><span data-stu-id="d4a68-163">Step 6: Run commands to migrate your IaaS resources</span></span>
> [!NOTE]
> <span data-ttu-id="d4a68-164">Všechny operace, které jsou zde popsané jsou idempotent.</span><span class="sxs-lookup"><span data-stu-id="d4a68-164">All the operations described here are idempotent.</span></span> <span data-ttu-id="d4a68-165">Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme zopakovat Příprava, zrušení nebo potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="d4a68-165">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry the prepare, abort, or commit operation.</span></span> <span data-ttu-id="d4a68-166">Platformu a potom se pokusí akci znovu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-166">The platform then tries the action again.</span></span>
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a><span data-ttu-id="d4a68-167">Krok 6.1: Možnost 1 - migraci virtuálních počítačů v rámci cloudové služby (ne ve virtuální síti)</span><span class="sxs-lookup"><span data-stu-id="d4a68-167">Step 6.1: Option 1 - Migrate virtual machines in a cloud service (not in a virtual network)</span></span>
<span data-ttu-id="d4a68-168">Získání seznamu cloudových služeb pomocí následujícího příkazu a pak vyberte cloudovou službu, která chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-168">Get the list of cloud services by using the following command, and then pick the cloud service that you want to migrate.</span></span> <span data-ttu-id="d4a68-169">Pokud jsou virtuální počítače v rámci cloudové služby ve virtuální síti nebo mají role web nebo worker, příkaz vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-169">If the VMs in the cloud service are in a virtual network or if they have web or worker roles, the command returns an error message.</span></span>

```powershell
    Get-AzureService | ft Servicename
```

<span data-ttu-id="d4a68-170">Získáte název nasazení pro cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-170">Get the deployment name for the cloud service.</span></span> <span data-ttu-id="d4a68-171">V tomto příkladu je název služby **Moje služba**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-171">In this example, the service name is **My Service**.</span></span> <span data-ttu-id="d4a68-172">Nahraďte název službu příkladu vlastní název služby.</span><span class="sxs-lookup"><span data-stu-id="d4a68-172">Replace the example service name with your own service name.</span></span>

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

<span data-ttu-id="d4a68-173">Vypněte virtuální počítače v rámci cloudové služby pro migraci.</span><span class="sxs-lookup"><span data-stu-id="d4a68-173">Prepare the virtual machines in the cloud service for migration.</span></span> <span data-ttu-id="d4a68-174">Máte dvě možnosti, které lze vybírat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-174">You have two options to choose from.</span></span>

* <span data-ttu-id="d4a68-175">**Možnost 1. Migrovat virtuální počítače k virtuální síti vytvořené platformy**</span><span class="sxs-lookup"><span data-stu-id="d4a68-175">**Option 1. Migrate the VMs to a platform-created virtual network**</span></span>

    <span data-ttu-id="d4a68-176">Nejprve ověřte, jestli je možné migrovat cloudové služby, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d4a68-176">First, validate if you can migrate the cloud service using the following commands:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    <span data-ttu-id="d4a68-177">Předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace.</span><span class="sxs-lookup"><span data-stu-id="d4a68-177">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="d4a68-178">Pokud je ověření úspěšné, pak můžete přesunout **Příprava** kroku:</span><span class="sxs-lookup"><span data-stu-id="d4a68-178">If validation is successful, then you can move on to the **Prepare** step:</span></span>

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* <span data-ttu-id="d4a68-179">**Možnost 2. Migrovat na existující virtuální síť v modelu nasazení Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="d4a68-179">**Option 2. Migrate to an existing virtual network in the Resource Manager deployment model**</span></span>

    <span data-ttu-id="d4a68-180">Tento příklad nastaví na název skupiny prostředků **myResourceGroup**, název virtuální sítě k **myVirtualNetwork** a název podsítě k **mySubNet**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-180">This example sets the resource group name to **myResourceGroup**, the virtual network name to **myVirtualNetwork** and the subnet name to **mySubNet**.</span></span> <span data-ttu-id="d4a68-181">Názvy v příkladu nahraďte názvy vlastních prostředků.</span><span class="sxs-lookup"><span data-stu-id="d4a68-181">Replace the names in the example with the names of your own resources.</span></span>

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    <span data-ttu-id="d4a68-182">Nejprve ověřte, jestli je možné migrovat virtuální sítě pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-182">First, validate if you can migrate the virtual network using the following command:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    <span data-ttu-id="d4a68-183">Předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace.</span><span class="sxs-lookup"><span data-stu-id="d4a68-183">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="d4a68-184">Pokud je ověření úspěšné, pak můžete pokračovat následujícím přípravný krok:</span><span class="sxs-lookup"><span data-stu-id="d4a68-184">If validation is successful, then you can proceed with the following Prepare step:</span></span>

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

<span data-ttu-id="d4a68-185">Po úspěšné operace přípravy s některou z předchozích možností dotazování na stav migrace virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d4a68-185">After the Prepare operation succeeds with either of the preceding options, query the migration state of the VMs.</span></span> <span data-ttu-id="d4a68-186">Ujistěte se, že jsou v `Prepared` stavu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-186">Ensure that they are in the `Prepared` state.</span></span>

<span data-ttu-id="d4a68-187">Tento příklad nastaví název virtuálního počítače na **Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-187">This example sets the VM name to **myVM**.</span></span> <span data-ttu-id="d4a68-188">Příklad názvu nahraďte váš vlastní název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d4a68-188">Replace the example name with your own VM name.</span></span>

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

<span data-ttu-id="d4a68-189">Zkontrolujte konfiguraci pro připravené prostředky pomocí prostředí PowerShell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-189">Check the configuration for the prepared resources by using either PowerShell or the Azure portal.</span></span> <span data-ttu-id="d4a68-190">Pokud si nejste připravený pro migraci a chcete přejít zpět do původního stavu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4a68-190">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

<span data-ttu-id="d4a68-191">Pokud připravené konfigurací spokojeni, můžete přejít a potvrdit prostředky pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-191">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="d4a68-192">Krok 6.1: Možnost 2 - migrovat virtuální počítače ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="d4a68-192">Step 6.1: Option 2 - Migrate virtual machines in a virtual network</span></span>

<span data-ttu-id="d4a68-193">Pokud chcete migrovat virtuální počítače ve virtuální síti, migrujte virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d4a68-193">To migrate virtual machines in a virtual network, you migrate the virtual network.</span></span> <span data-ttu-id="d4a68-194">Virtuální počítače migrovat automaticky pomocí virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d4a68-194">The virtual machines automatically migrate with the virtual network.</span></span> <span data-ttu-id="d4a68-195">Vyberte virtuální síť, která chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="d4a68-195">Pick the virtual network that you want to migrate.</span></span>
> [!NOTE]
> <span data-ttu-id="d4a68-196">[Migrovat jeden virtuální počítač classic](migrate-single-classic-to-resource-manager.md) po vytvoření nového virtuálního počítače správce prostředků se spravovaná diskům použít soubory virtuálního pevného disku (operačního systému a data) virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d4a68-196">[Migrate single classic virtual machine](migrate-single-classic-to-resource-manager.md) by creating a new Resource Manager virtual machine with Managed Disks using the VHD (OS and data) files of the virtual machine.</span></span>
<br>

> [!NOTE]
> <span data-ttu-id="d4a68-197">Název virtuální sítě se může lišit od informace zobrazené v nového portálu.</span><span class="sxs-lookup"><span data-stu-id="d4a68-197">The virtual network name might be different from what is shown in the new Portal.</span></span> <span data-ttu-id="d4a68-198">Zobrazí název nového portálu Azure `[vnet-name]` , ale skutečný virtuální síť s názvem typu `Group [resource-group-name] [vnet-name]`.</span><span class="sxs-lookup"><span data-stu-id="d4a68-198">The new Azure Portal displays the name as `[vnet-name]` but the actual virtual network name is of type `Group [resource-group-name] [vnet-name]`.</span></span> <span data-ttu-id="d4a68-199">Před migrací, vyhledávací název skutečné virtuální sítě pomocí příkazu `Get-AzureVnetSite | Select -Property Name` nebo ji zobrazit v starý portál Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-199">Before migrating, lookup the actual virtual network name using the command `Get-AzureVnetSite | Select -Property Name` or view it in the old Azure Portal.</span></span> 

<span data-ttu-id="d4a68-200">Tento příklad nastaví název virtuální sítě na **myVnet**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-200">This example sets the virtual network name to **myVnet**.</span></span> <span data-ttu-id="d4a68-201">Nahraďte název virtuální sítě příklad vlastní.</span><span class="sxs-lookup"><span data-stu-id="d4a68-201">Replace the example virtual network name with your own.</span></span>

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> <span data-ttu-id="d4a68-202">Pokud virtuální síť obsahuje webové nebo rolí pracovního procesu nebo virtuálních počítačů s nepodporované konfigurace, zobrazí chybovou zprávu ověření.</span><span class="sxs-lookup"><span data-stu-id="d4a68-202">If the virtual network contains web or worker roles, or VMs with unsupported configurations, you get a validation error message.</span></span>
>
>

<span data-ttu-id="d4a68-203">Nejprve ověřte, jestli virtuální sítě můžete migrovat pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-203">First, validate if you can migrate the virtual network by using the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

<span data-ttu-id="d4a68-204">Předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace.</span><span class="sxs-lookup"><span data-stu-id="d4a68-204">The preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="d4a68-205">Pokud je ověření úspěšné, pak můžete pokračovat následujícím přípravný krok:</span><span class="sxs-lookup"><span data-stu-id="d4a68-205">If validation is successful, then you can proceed with the following Prepare step:</span></span>

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

<span data-ttu-id="d4a68-206">Zkontrolujte konfiguraci připravené virtuálních počítačů pomocí Azure PowerShell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-206">Check the configuration for the prepared virtual machines by using either Azure PowerShell or the Azure portal.</span></span> <span data-ttu-id="d4a68-207">Pokud si nejste připravený pro migraci a chcete přejít zpět do původního stavu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4a68-207">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

<span data-ttu-id="d4a68-208">Pokud připravené konfigurací spokojeni, můžete přejít a potvrdit prostředky pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-208">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a><span data-ttu-id="d4a68-209">Krok 6.2 migrací účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="d4a68-209">Step 6.2 Migrate a storage account</span></span>
<span data-ttu-id="d4a68-210">Po dokončení migrace virtuálních počítačů, doporučujeme, migraci účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4a68-210">Once you're done migrating the virtual machines, we recommend you migrate the storage accounts.</span></span>

<span data-ttu-id="d4a68-211">Před migrací účtu úložiště, proveďte předcházející kontrol požadovaných součástí:</span><span class="sxs-lookup"><span data-stu-id="d4a68-211">Before you migrate the storage account, please perform preceding prerequisite checks:</span></span>

* <span data-ttu-id="d4a68-212">**Migrace klasické virtuální počítače, jejichž disky jsou uložené v účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="d4a68-212">**Migrate classic virtual machines whose disks are stored in the storage account**</span></span>

    <span data-ttu-id="d4a68-213">Předcházející příkaz vrátí RoleName a DiskName vlastnosti všechny klasické disky virtuálních počítačů v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4a68-213">Preceding command returns RoleName and DiskName properties of all the classic VM disks in the storage account.</span></span> <span data-ttu-id="d4a68-214">RoleName je název virtuálního počítače, ke kterému je disk připojen.</span><span class="sxs-lookup"><span data-stu-id="d4a68-214">RoleName is the name of the virtual machine to which a disk is attached.</span></span> <span data-ttu-id="d4a68-215">Pokud předcházející příkaz vrátí disky pak se ujistěte, že jsou virtuální počítače, na které tyto disky připojené migrovány před migrací účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4a68-215">If preceding command returns disks then ensure that virtual machines to which these disks are attached are migrated before migrating the storage account.</span></span>
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* <span data-ttu-id="d4a68-216">**Odstranit odpojit classic disky virtuálních počítačů uložených v účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="d4a68-216">**Delete unattached classic VM disks stored in the storage account**</span></span>

    <span data-ttu-id="d4a68-217">Najít odpojit classic disky virtuálních počítačů v úložišti účtu pomocí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4a68-217">Find unattached classic VM disks in the storage account using following command:</span></span>

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    <span data-ttu-id="d4a68-218">Pokud se výše příkaz vrátí disky pak odstraňte tyto disky pomocí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4a68-218">If above command returns disks then delete these disks using following command:</span></span>

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* <span data-ttu-id="d4a68-219">**Odstranit Image virtuálních počítačů uložených v účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="d4a68-219">**Delete VM images stored in the storage account**</span></span>

    <span data-ttu-id="d4a68-220">Předcházející příkaz vrátí všechny bitové kopie virtuálních počítačů s diskem operačního systému uložené v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4a68-220">Preceding command returns all the VM images with OS disk stored in the storage account.</span></span>
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     <span data-ttu-id="d4a68-221">Předcházející příkaz vrátí všechny bitové kopie virtuálního počítače s datovými disky uložené v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4a68-221">Preceding command returns all the VM images with data disks stored in the storage account.</span></span>
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    <span data-ttu-id="d4a68-222">Odstraňte všechny Image virtuálních počítačů vrácený výše příkazy předchozím příkazem:</span><span class="sxs-lookup"><span data-stu-id="d4a68-222">Delete all the VM images returned by above commands using preceding command:</span></span>
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

<span data-ttu-id="d4a68-223">Pomocí následujícího příkazu ověřte každý účet úložiště pro migraci.</span><span class="sxs-lookup"><span data-stu-id="d4a68-223">Validate each storage account for migration by using the following command.</span></span> <span data-ttu-id="d4a68-224">V tomto příkladu je název účtu úložiště **Můj_účet_úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d4a68-224">In this example, the storage account name is **myStorageAccount**.</span></span> <span data-ttu-id="d4a68-225">Příklad názvu nahraďte názvem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d4a68-225">Replace the example name with the name of your own storage account.</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

<span data-ttu-id="d4a68-226">Dalším krokem je účet úložiště přípravy na migraci</span><span class="sxs-lookup"><span data-stu-id="d4a68-226">Next step is to Prepare the storage account for migration</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

<span data-ttu-id="d4a68-227">Zkontrolujte konfiguraci pro účet připravený úložiště pomocí prostředí Azure PowerShell nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4a68-227">Check the configuration for the prepared storage account by using either Azure PowerShell or the Azure portal.</span></span> <span data-ttu-id="d4a68-228">Pokud si nejste připravený pro migraci a chcete přejít zpět do původního stavu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d4a68-228">If you are not ready for migration and you want to go back to the old state, use the following command:</span></span>

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

<span data-ttu-id="d4a68-229">Pokud připravené konfigurací spokojeni, můžete přejít a potvrdit prostředky pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d4a68-229">If the prepared configuration looks good, you can move forward and commit the resources by using the following command:</span></span>

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a><span data-ttu-id="d4a68-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d4a68-230">Next steps</span></span>
* [<span data-ttu-id="d4a68-231">Přehled platformy podporované migrace z klasického do Azure Resource Manageru prostředků IaaS</span><span class="sxs-lookup"><span data-stu-id="d4a68-231">Overview of platform-supported migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d4a68-232">Technické podrobné informace o platformy podporované migrace z klasického do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d4a68-232">Technical deep dive on platform-supported migration from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d4a68-233">Plánování migrace prostředků IaaS z nasazení Classic do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d4a68-233">Planning for migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d4a68-234">Pomocí rozhraní příkazového řádku můžete migrovat prostředky infrastruktury z classic do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d4a68-234">Use CLI to migrate IaaS resources from classic to Azure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d4a68-235">Komunita nástroje asistence s migrace z klasického do Azure Resource Manageru prostředků IaaS</span><span class="sxs-lookup"><span data-stu-id="d4a68-235">Community tools for assisting with migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d4a68-236">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="d4a68-236">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="d4a68-237">Přečtěte si nejčastější dotazy o migraci prostředky infrastruktury jako služby z klasického do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d4a68-237">Review the most frequently asked questions about migrating IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
