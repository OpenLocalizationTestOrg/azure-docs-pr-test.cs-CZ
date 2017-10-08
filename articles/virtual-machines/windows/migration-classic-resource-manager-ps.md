---
title: "aaaMigrate tooResource Manager pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede hello migrace podporované platformy IaaS prostředků, jako jsou virtuální počítače (VM), virtuální sítě (virtuální sítě) a účty úložiště z classic tooAzure Resource Manager (ARM) pomocí příkazů prostředí Azure PowerShell"
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
ms.openlocfilehash: b5b2987da292f1c241be71a354b0c2e1a96a07c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-powershell"></a><span data-ttu-id="7ddae-103">Migraci prostředky infrastruktury z classic tooAzure správce prostředků pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ddae-103">Migrate IaaS resources from classic tooAzure Resource Manager by using Azure PowerShell</span></span>
<span data-ttu-id="7ddae-104">Tyto kroky vám ukážou, jak toouse prostředí Azure PowerShell příkazy toomigrate infrastruktury jako služby (IaaS) prostředky z modelu nasazení classic hello, toohello modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7ddae-104">These steps show you how toouse Azure PowerShell commands toomigrate infrastructure as a service (IaaS) resources from hello classic deployment model toohello Azure Resource Manager deployment model.</span></span>

<span data-ttu-id="7ddae-105">Pokud chcete, můžete také migrovat prostředky pomocí hello [rozhraní příkazového řádku Azure (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7ddae-105">If you want, you can also migrate resources by using hello [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).</span></span>

* <span data-ttu-id="7ddae-106">Pro informace o podporovaných scénářů migrace, viz [platformy podporované migrace prostředky infrastruktury z classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7ddae-106">For background on supported migration scenarios, see [Platform-supported migration of IaaS resources from classic tooAzure Resource Manager](migration-classic-resource-manager-overview.md).</span></span>
* <span data-ttu-id="7ddae-107">Podrobné pokyny a migrace návod najdete v tématu [technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span><span class="sxs-lookup"><span data-stu-id="7ddae-107">For detailed guidance and a migration walkthrough, see [Technical deep dive on platform-supported migration from classic tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).</span></span>
* [<span data-ttu-id="7ddae-108">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="7ddae-108">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md)

<br>
<span data-ttu-id="7ddae-109">Tady je pořadí tooidentify hello vývojový diagram, ve kterém kroky nutné toobe provést během procesu migrace</span><span class="sxs-lookup"><span data-stu-id="7ddae-109">Here is a flowchart tooidentify hello order in which steps need toobe executed during a migration process</span></span>

![Snímek obrazovky, který zobrazuje kroky migrace hello](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a><span data-ttu-id="7ddae-111">Krok 1: Plánování migrace</span><span class="sxs-lookup"><span data-stu-id="7ddae-111">Step 1: Plan for migration</span></span>
<span data-ttu-id="7ddae-112">Tady je několik osvědčených postupů, které doporučujeme jak vyhodnotit migrace prostředky infrastruktury z classic tooResource Manager:</span><span class="sxs-lookup"><span data-stu-id="7ddae-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic tooResource Manager:</span></span>

* <span data-ttu-id="7ddae-113">Pročtěte hello [podporované a nepodporované funkce a konfigurace](migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7ddae-113">Read through hello [supported and unsupported features and configurations](migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="7ddae-114">Pokud máte virtuální počítače, které používají nepodporované konfigurace nebo funkce, doporučujeme počkejte hello konfigurace nebo funkce podpory toobe oznámeno.</span><span class="sxs-lookup"><span data-stu-id="7ddae-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for hello configuration/feature support toobe announced.</span></span> <span data-ttu-id="7ddae-115">Případně pokud ji vyhovuje vašim potřebám, odeberte tuto funkci nebo přesunout mimo tuto tooenable migraci konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7ddae-115">Alternatively, if it suits your needs, remove that feature or move out of that configuration tooenable migration.</span></span>
* <span data-ttu-id="7ddae-116">Pokud máte automatizované skripty, které jsou dnes nasazení infrastruktury a aplikace, zkuste toocreate podobné nastavení testu pomocí těchto skriptů pro migraci.</span><span class="sxs-lookup"><span data-stu-id="7ddae-116">If you have automated scripts that deploy your infrastructure and applications today, try toocreate a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="7ddae-117">Alternativně můžete nastavit ukázkové prostředí pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-117">Alternatively, you can set up sample environments by using hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ddae-118">Application Gateway nejsou aktuálně podporovány pro migraci z classic tooResource správce.</span><span class="sxs-lookup"><span data-stu-id="7ddae-118">Application Gateways are not currently supported for migration from classic tooResource Manager.</span></span> <span data-ttu-id="7ddae-119">Před spuštěním síť Příprava operace toomove hello odebrat toomigrate klasickou virtuální síť s aplikační brány, brány hello.</span><span class="sxs-lookup"><span data-stu-id="7ddae-119">toomigrate a classic virtual network with an Application gateway, remove hello gateway before running a Prepare operation toomove hello network.</span></span> <span data-ttu-id="7ddae-120">Po dokončení migrace hello znovu hello brány ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-120">After you complete hello migration, reconnect hello gateway in Azure Resource Manager.</span></span>
>
><span data-ttu-id="7ddae-121">Připojení tooExpressRoute okruhů v jiné předplatné brány ExpressRoute se nedají automaticky migrovat.</span><span class="sxs-lookup"><span data-stu-id="7ddae-121">ExpressRoute gateways connecting tooExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="7ddae-122">V takových případech odeberte hello brány ExpressRoute, migrovat hello virtuální síť a znovu hello brány.</span><span class="sxs-lookup"><span data-stu-id="7ddae-122">In such cases, remove hello ExpressRoute gateway, migrate hello virtual network and recreate hello gateway.</span></span> <span data-ttu-id="7ddae-123">Najdete v tématu [migrovat ExpressRoute okruhy a přidružené virtuální sítě z modelu nasazení Resource Manager classic toohello hello](../../expressroute/expressroute-migration-classic-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="7ddae-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from hello classic toohello Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
>
>

## <a name="step-2-install-hello-latest-version-of-azure-powershell"></a><span data-ttu-id="7ddae-124">Krok 2: Instalace hello nejnovější verzi prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ddae-124">Step 2: Install hello latest version of Azure PowerShell</span></span>
<span data-ttu-id="7ddae-125">Existují dvě hlavní možnosti tooinstall prostředí Azure PowerShell: [Galerie prostředí PowerShell](https://www.powershellgallery.com/profiles/azure-sdk/) nebo [instalačního programu webové platformy (WebPI)](http://aka.ms/webpi-azps).</span><span class="sxs-lookup"><span data-stu-id="7ddae-125">There are two main options tooinstall Azure PowerShell: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) or [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps).</span></span> <span data-ttu-id="7ddae-126">WebPI obdrží měsíčních aktualizací.</span><span class="sxs-lookup"><span data-stu-id="7ddae-126">WebPI receives monthly updates.</span></span> <span data-ttu-id="7ddae-127">Galerie prostředí PowerShell získává aktualizace trvale.</span><span class="sxs-lookup"><span data-stu-id="7ddae-127">PowerShell Gallery receives updates on a continuous basis.</span></span> <span data-ttu-id="7ddae-128">Tento článek je založená na prostředí Azure PowerShell verze 2.1.0.</span><span class="sxs-lookup"><span data-stu-id="7ddae-128">This article is based on Azure PowerShell version 2.1.0.</span></span>

<span data-ttu-id="7ddae-129">Pokyny k instalaci naleznete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7ddae-129">For installation instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-hello-subscription-in-azure-portal"></a><span data-ttu-id="7ddae-130">Krok 3: Ujistěte se, že jste správce pro předplatné hello na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7ddae-130">Step 3: Ensure that you are an administrator for hello subscription in Azure portal</span></span>
<span data-ttu-id="7ddae-131">tooperform této migrace, je třeba přidat jako spolusprávce pro předplatné hello v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7ddae-131">tooperform this migration, you must be added as a co-administrator for hello subscription in hello [Azure portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="7ddae-132">Přihlaste se k hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7ddae-132">Sign into hello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7ddae-133">V nabídce centra hello vyberte **předplatné**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-133">On hello Hub menu, select **Subscription**.</span></span> <span data-ttu-id="7ddae-134">Pokud ho nevidíte, vyberte **další služby**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-134">If you don't see it, select **More services**.</span></span>
3. <span data-ttu-id="7ddae-135">Najít položku odpovídající předplatné hello, potom si prohlédněte hello **Moje ROLE** pole.</span><span class="sxs-lookup"><span data-stu-id="7ddae-135">Find hello appropriate subscription entry, then look at hello **MY ROLE** field.</span></span> <span data-ttu-id="7ddae-136">Pro správce a společné hello hodnota by měla být _správce účtu_.</span><span class="sxs-lookup"><span data-stu-id="7ddae-136">For a co-administrator, hello value should be _Account admin_.</span></span>

<span data-ttu-id="7ddae-137">Pokud si nejste možné tooadd společné správce, pak obraťte se na služby správce nebo spolusprávce pro tooget předplatné hello sami přidat.</span><span class="sxs-lookup"><span data-stu-id="7ddae-137">If you are not able tooadd a co-administrator, then contact a service administrator or co-administrator for hello subscription tooget yourself added.</span></span>   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a><span data-ttu-id="7ddae-138">Krok 4: Nastavte předplatné a zaregistrujte se pro migraci</span><span class="sxs-lookup"><span data-stu-id="7ddae-138">Step 4: Set your subscription and sign up for migration</span></span>
<span data-ttu-id="7ddae-139">Nejprve spusťte příkazovém řádku prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7ddae-139">First, start a PowerShell prompt.</span></span> <span data-ttu-id="7ddae-140">Pro migraci, je nutné tooset prostředí pro obě classic a Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7ddae-140">For migration, you need tooset up your environment for both classic and Resource Manager.</span></span>

<span data-ttu-id="7ddae-141">Přihlaste se tooyour účet pro hello modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7ddae-141">Sign in tooyour account for hello Resource Manager model.</span></span>

```powershell
    Login-AzureRmAccount
```

<span data-ttu-id="7ddae-142">Získáte dostupných předplatných hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-142">Get hello available subscriptions by using hello following command:</span></span>

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

<span data-ttu-id="7ddae-143">Nastavte vašeho předplatného Azure pro hello aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="7ddae-143">Set your Azure subscription for hello current session.</span></span> <span data-ttu-id="7ddae-144">Tento příklad sady hello výchozí název odběru příliš**Moje předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-144">This example sets hello default subscription name too**My Azure Subscription**.</span></span> <span data-ttu-id="7ddae-145">Nahraďte název odběru příklad hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="7ddae-145">Replace hello example subscription name with your own.</span></span>

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> <span data-ttu-id="7ddae-146">Registrace je jednorázové krok, ale musíte to provést jednou před pokusem o migraci.</span><span class="sxs-lookup"><span data-stu-id="7ddae-146">Registration is a one-time step, but you must do it once before attempting migration.</span></span> <span data-ttu-id="7ddae-147">Bez registrace, najdete v části hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="7ddae-147">Without registering, you see hello following error message:</span></span>
>
> <span data-ttu-id="7ddae-148">*Struktura BadRequest: Předplatné není zaregistrované pro migraci.*</span><span class="sxs-lookup"><span data-stu-id="7ddae-148">*BadRequest : Subscription is not registered for migration.*</span></span>
>
>

<span data-ttu-id="7ddae-149">Zaregistrovat u zprostředkovatele prostředků migrace hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-149">Register with hello migration resource provider by using hello following command:</span></span>

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="7ddae-150">Počkejte pět minut, než toofinish registrace hello.</span><span class="sxs-lookup"><span data-stu-id="7ddae-150">Please wait five minutes for hello registration toofinish.</span></span> <span data-ttu-id="7ddae-151">Stav hello hello schválení můžete zkontrolovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-151">You can check hello status of hello approval by using hello following command:</span></span>

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

<span data-ttu-id="7ddae-152">Ujistěte se, že je RegistrationState `Registered` než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="7ddae-152">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

<span data-ttu-id="7ddae-153">Nyní přihlašovací účet tooyour pro hello klasického modelu.</span><span class="sxs-lookup"><span data-stu-id="7ddae-153">Now sign in tooyour account for hello classic model.</span></span>

```powershell
    Add-AzureAccount
```

<span data-ttu-id="7ddae-154">Získáte dostupných předplatných hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-154">Get hello available subscriptions by using hello following command:</span></span>

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

<span data-ttu-id="7ddae-155">Nastavte vašeho předplatného Azure pro hello aktuální relaci.</span><span class="sxs-lookup"><span data-stu-id="7ddae-155">Set your Azure subscription for hello current session.</span></span> <span data-ttu-id="7ddae-156">Tento příklad nastaví hello výchozí předplatné příliš**Moje předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-156">This example sets hello default subscription too**My Azure Subscription**.</span></span> <span data-ttu-id="7ddae-157">Nahraďte název odběru příklad hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="7ddae-157">Replace hello example subscription name with your own.</span></span>

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="7ddae-158">Krok 5: Zkontrolujte, zda že máte dostatečný počet jader virtuálního počítače Azure Resource Manager v hello oblast Azure vaše aktuální nasazení nebo virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="7ddae-158">Step 5: Make sure you have enough Azure Resource Manager Virtual Machine cores in hello Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="7ddae-159">Můžete použít hello následující prostředí PowerShell příkaz toocheck hello aktuální počet jader, které máte ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-159">You can use hello following PowerShell command toocheck hello current number of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="7ddae-160">toolearn Další informace o základní kvóty, najdete v části [omezení a hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span><span class="sxs-lookup"><span data-stu-id="7ddae-160">toolearn more about core quotas, see [Limits and hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).</span></span>

<span data-ttu-id="7ddae-161">Tento příklad zkontroluje dostupnost hello ve hello **západní USA** oblast.</span><span class="sxs-lookup"><span data-stu-id="7ddae-161">This example checks hello availability in hello **West US** region.</span></span> <span data-ttu-id="7ddae-162">Nahraďte název oblasti příklad hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="7ddae-162">Replace hello example region name with your own.</span></span>

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-toomigrate-your-iaas-resources"></a><span data-ttu-id="7ddae-163">Krok 6: Spuštění příkazů toomigrate vašich prostředků IaaS</span><span class="sxs-lookup"><span data-stu-id="7ddae-163">Step 6: Run commands toomigrate your IaaS resources</span></span>
> [!NOTE]
> <span data-ttu-id="7ddae-164">Všechny operace hello postupu popsaného tady jsou idempotent.</span><span class="sxs-lookup"><span data-stu-id="7ddae-164">All hello operations described here are idempotent.</span></span> <span data-ttu-id="7ddae-165">Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme, abyste se znovu pokusíte hello připravit, zrušení nebo potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="7ddae-165">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry hello prepare, abort, or commit operation.</span></span> <span data-ttu-id="7ddae-166">Platforma Hello potom pokusí hello akci znovu.</span><span class="sxs-lookup"><span data-stu-id="7ddae-166">hello platform then tries hello action again.</span></span>
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a><span data-ttu-id="7ddae-167">Krok 6.1: Možnost 1 - migraci virtuálních počítačů v rámci cloudové služby (ne ve virtuální síti)</span><span class="sxs-lookup"><span data-stu-id="7ddae-167">Step 6.1: Option 1 - Migrate virtual machines in a cloud service (not in a virtual network)</span></span>
<span data-ttu-id="7ddae-168">Získání seznamu hello cloudových služeb pomocí hello následující příkaz a potom vyberte hello cloudové služby, které chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="7ddae-168">Get hello list of cloud services by using hello following command, and then pick hello cloud service that you want toomigrate.</span></span> <span data-ttu-id="7ddae-169">Pokud jsou hello virtuálních počítačů v rámci hello cloudové služby ve virtuální síti nebo mají role web nebo worker, hello příkaz vrátí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="7ddae-169">If hello VMs in hello cloud service are in a virtual network or if they have web or worker roles, hello command returns an error message.</span></span>

```powershell
    Get-AzureService | ft Servicename
```

<span data-ttu-id="7ddae-170">Získáte název nasazení hello hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="7ddae-170">Get hello deployment name for hello cloud service.</span></span> <span data-ttu-id="7ddae-171">V tomto příkladu je název služby hello **Moje služba**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-171">In this example, hello service name is **My Service**.</span></span> <span data-ttu-id="7ddae-172">Nahraďte název službu příkladu hello svůj vlastní název služby.</span><span class="sxs-lookup"><span data-stu-id="7ddae-172">Replace hello example service name with your own service name.</span></span>

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

<span data-ttu-id="7ddae-173">Připravte hello virtuálních počítačů v rámci hello cloudové služby pro migraci.</span><span class="sxs-lookup"><span data-stu-id="7ddae-173">Prepare hello virtual machines in hello cloud service for migration.</span></span> <span data-ttu-id="7ddae-174">Máte dvě možnosti toochoose z.</span><span class="sxs-lookup"><span data-stu-id="7ddae-174">You have two options toochoose from.</span></span>

* <span data-ttu-id="7ddae-175">**Možnost 1. Migrovat hello virtuální počítače tooa platformy vytvořit virtuální síť**</span><span class="sxs-lookup"><span data-stu-id="7ddae-175">**Option 1. Migrate hello VMs tooa platform-created virtual network**</span></span>

    <span data-ttu-id="7ddae-176">Nejprve ověřte, jestli je možné migrovat hello cloudové služby pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="7ddae-176">First, validate if you can migrate hello cloud service using hello following commands:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    <span data-ttu-id="7ddae-177">Hello předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace.</span><span class="sxs-lookup"><span data-stu-id="7ddae-177">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="7ddae-178">Pokud je ověření úspěšné, pak můžete přesunout na toohello **Příprava** kroku:</span><span class="sxs-lookup"><span data-stu-id="7ddae-178">If validation is successful, then you can move on toohello **Prepare** step:</span></span>

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* <span data-ttu-id="7ddae-179">**Možnost 2. Migrovat existující virtuální síť tooan v modelu nasazení Resource Manager hello**</span><span class="sxs-lookup"><span data-stu-id="7ddae-179">**Option 2. Migrate tooan existing virtual network in hello Resource Manager deployment model**</span></span>

    <span data-ttu-id="7ddae-180">Tento příklad sady hello název skupiny prostředků příliš**myResourceGroup**, hello název virtuální sítě příliš**myVirtualNetwork** a hello název podsítě příliš**mySubNet**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-180">This example sets hello resource group name too**myResourceGroup**, hello virtual network name too**myVirtualNetwork** and hello subnet name too**mySubNet**.</span></span> <span data-ttu-id="7ddae-181">Nahraďte názvy hello v příkladu hello hello názvy vlastních prostředků.</span><span class="sxs-lookup"><span data-stu-id="7ddae-181">Replace hello names in hello example with hello names of your own resources.</span></span>

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    <span data-ttu-id="7ddae-182">Nejprve ověřte, jestli je možné migrovat hello virtuální sítě pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-182">First, validate if you can migrate hello virtual network using hello following command:</span></span>

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    <span data-ttu-id="7ddae-183">Hello předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace.</span><span class="sxs-lookup"><span data-stu-id="7ddae-183">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="7ddae-184">Pokud je ověření úspěšné, pak můžete pokračovat hello následující přípravný krok:</span><span class="sxs-lookup"><span data-stu-id="7ddae-184">If validation is successful, then you can proceed with hello following Prepare step:</span></span>

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

<span data-ttu-id="7ddae-185">Po úspěšné hello Příprava operace s některou z předchozích možností hello dotaz na stav migrace hello hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="7ddae-185">After hello Prepare operation succeeds with either of hello preceding options, query hello migration state of hello VMs.</span></span> <span data-ttu-id="7ddae-186">Ujistěte se, že jsou v hello `Prepared` stavu.</span><span class="sxs-lookup"><span data-stu-id="7ddae-186">Ensure that they are in hello `Prepared` state.</span></span>

<span data-ttu-id="7ddae-187">Tento příklad sady hello název virtuálního počítače příliš**Můjvp**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-187">This example sets hello VM name too**myVM**.</span></span> <span data-ttu-id="7ddae-188">Příklad názvu hello nahraďte název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7ddae-188">Replace hello example name with your own VM name.</span></span>

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

<span data-ttu-id="7ddae-189">Zkontrolujte konfiguraci hello hello připravenou prostředky pomocí prostředí PowerShell nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-189">Check hello configuration for hello prepared resources by using either PowerShell or hello Azure portal.</span></span> <span data-ttu-id="7ddae-190">Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-190">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

<span data-ttu-id="7ddae-191">Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-191">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="7ddae-192">Krok 6.1: Možnost 2 - migrovat virtuální počítače ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="7ddae-192">Step 6.1: Option 2 - Migrate virtual machines in a virtual network</span></span>

<span data-ttu-id="7ddae-193">toomigrate virtuální počítače ve virtuální síti, můžete migrovat hello virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="7ddae-193">toomigrate virtual machines in a virtual network, you migrate hello virtual network.</span></span> <span data-ttu-id="7ddae-194">Hello virtuální počítače migrovat automaticky pomocí hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="7ddae-194">hello virtual machines automatically migrate with hello virtual network.</span></span> <span data-ttu-id="7ddae-195">Vybrat hello virtuální sítě, které chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="7ddae-195">Pick hello virtual network that you want toomigrate.</span></span>
> [!NOTE]
> <span data-ttu-id="7ddae-196">[Migrovat jeden virtuální počítač classic](migrate-single-classic-to-resource-manager.md) po vytvoření nového virtuálního počítače správce prostředků se spravovaná diskům použít (hello virtuálního pevného disku operačního systému a datových) souborů hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="7ddae-196">[Migrate single classic virtual machine](migrate-single-classic-to-resource-manager.md) by creating a new Resource Manager virtual machine with Managed Disks using hello VHD (OS and data) files of hello virtual machine.</span></span>
<br>

> [!NOTE]
> <span data-ttu-id="7ddae-197">název virtuální sítě Hello se může lišit od informace zobrazené v hello nového portálu.</span><span class="sxs-lookup"><span data-stu-id="7ddae-197">hello virtual network name might be different from what is shown in hello new Portal.</span></span> <span data-ttu-id="7ddae-198">Hello nový portál Azure zobrazí název hello jako `[vnet-name]` ale hello skutečné virtuální síť s názvem je typu `Group [resource-group-name] [vnet-name]`.</span><span class="sxs-lookup"><span data-stu-id="7ddae-198">hello new Azure Portal displays hello name as `[vnet-name]` but hello actual virtual network name is of type `Group [resource-group-name] [vnet-name]`.</span></span> <span data-ttu-id="7ddae-199">Před migrací, vyhledávání hello skutečné virtuální síť s názvem pomocí příkazu hello `Get-AzureVnetSite | Select -Property Name` nebo ji zobrazit v hello starý portál Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-199">Before migrating, lookup hello actual virtual network name using hello command `Get-AzureVnetSite | Select -Property Name` or view it in hello old Azure Portal.</span></span> 

<span data-ttu-id="7ddae-200">Tento příklad sady hello název virtuální sítě příliš**myVnet**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-200">This example sets hello virtual network name too**myVnet**.</span></span> <span data-ttu-id="7ddae-201">Nahraďte název virtuální sítě příklad hello vlastní.</span><span class="sxs-lookup"><span data-stu-id="7ddae-201">Replace hello example virtual network name with your own.</span></span>

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> <span data-ttu-id="7ddae-202">Pokud hello virtuální síť obsahuje webové nebo rolí pracovního procesu nebo virtuálních počítačů s nepodporované konfigurace, zobrazí chybovou zprávu ověření.</span><span class="sxs-lookup"><span data-stu-id="7ddae-202">If hello virtual network contains web or worker roles, or VMs with unsupported configurations, you get a validation error message.</span></span>
>
>

<span data-ttu-id="7ddae-203">Nejprve ověřte, jestli virtuální sítě hello lze migrovat pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-203">First, validate if you can migrate hello virtual network by using hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

<span data-ttu-id="7ddae-204">Hello předchozí příkaz zobrazí všechny upozornění a chyb, které blokovat migrace.</span><span class="sxs-lookup"><span data-stu-id="7ddae-204">hello preceding command displays any warnings and errors that block migration.</span></span> <span data-ttu-id="7ddae-205">Pokud je ověření úspěšné, pak můžete pokračovat hello následující přípravný krok:</span><span class="sxs-lookup"><span data-stu-id="7ddae-205">If validation is successful, then you can proceed with hello following Prepare step:</span></span>

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

<span data-ttu-id="7ddae-206">Zkontrolujte konfiguraci hello hello připravenou virtuálních počítačů pomocí Azure PowerShell nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-206">Check hello configuration for hello prepared virtual machines by using either Azure PowerShell or hello Azure portal.</span></span> <span data-ttu-id="7ddae-207">Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-207">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

<span data-ttu-id="7ddae-208">Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-208">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a><span data-ttu-id="7ddae-209">Krok 6.2 migrací účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="7ddae-209">Step 6.2 Migrate a storage account</span></span>
<span data-ttu-id="7ddae-210">Po dokončení migrace hello virtuálních počítačů, doporučujeme že migraci účtů úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7ddae-210">Once you're done migrating hello virtual machines, we recommend you migrate hello storage accounts.</span></span>

<span data-ttu-id="7ddae-211">Před migrací hello účet úložiště, proveďte předcházející kontrol požadovaných součástí:</span><span class="sxs-lookup"><span data-stu-id="7ddae-211">Before you migrate hello storage account, please perform preceding prerequisite checks:</span></span>

* <span data-ttu-id="7ddae-212">**Migrace klasické virtuální počítače, jejichž disky jsou uložené v účtu úložiště hello**</span><span class="sxs-lookup"><span data-stu-id="7ddae-212">**Migrate classic virtual machines whose disks are stored in hello storage account**</span></span>

    <span data-ttu-id="7ddae-213">Předcházející příkaz vrátí RoleName a DiskName vlastnosti všech hello classic disků virtuálních počítačů v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7ddae-213">Preceding command returns RoleName and DiskName properties of all hello classic VM disks in hello storage account.</span></span> <span data-ttu-id="7ddae-214">RoleName je název hello toowhich hello virtuálního počítače, který je připojen na disk.</span><span class="sxs-lookup"><span data-stu-id="7ddae-214">RoleName is hello name of hello virtual machine toowhich a disk is attached.</span></span> <span data-ttu-id="7ddae-215">Pokud předcházející příkaz vrátí disky pak se ujistěte, že virtuální počítače toowhich tyto disky připojené jsou migrovány před migrací hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ddae-215">If preceding command returns disks then ensure that virtual machines toowhich these disks are attached are migrated before migrating hello storage account.</span></span>
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* <span data-ttu-id="7ddae-216">**Odstranit odpojit classic disky virtuálních počítačů uložených v účtu storage hello**</span><span class="sxs-lookup"><span data-stu-id="7ddae-216">**Delete unattached classic VM disks stored in hello storage account**</span></span>

    <span data-ttu-id="7ddae-217">Najít odpojit classic disky virtuálních počítačů v úložišti hello účet pomocí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-217">Find unattached classic VM disks in hello storage account using following command:</span></span>

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    <span data-ttu-id="7ddae-218">Pokud se výše příkaz vrátí disky pak odstraňte tyto disky pomocí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-218">If above command returns disks then delete these disks using following command:</span></span>

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* <span data-ttu-id="7ddae-219">**Odstranit Image virtuálních počítačů uložených v účtu storage hello**</span><span class="sxs-lookup"><span data-stu-id="7ddae-219">**Delete VM images stored in hello storage account**</span></span>

    <span data-ttu-id="7ddae-220">Předcházející příkaz vrátí všechny Image virtuálních počítačů hello disk operačního systému uložené v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7ddae-220">Preceding command returns all hello VM images with OS disk stored in hello storage account.</span></span>
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     <span data-ttu-id="7ddae-221">Předcházející příkaz vrátí všechny Image virtuálních počítačů hello s datovými disky uložené v účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="7ddae-221">Preceding command returns all hello VM images with data disks stored in hello storage account.</span></span>
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    <span data-ttu-id="7ddae-222">Odstraňte všechny Image virtuálních počítačů hello vrácený výše příkazy předchozím příkazem:</span><span class="sxs-lookup"><span data-stu-id="7ddae-222">Delete all hello VM images returned by above commands using preceding command:</span></span>
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

<span data-ttu-id="7ddae-223">Každý účet úložiště pro migraci ověřte pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="7ddae-223">Validate each storage account for migration by using hello following command.</span></span> <span data-ttu-id="7ddae-224">V tomto příkladu je název účtu úložiště hello **Můj_účet_úložiště**.</span><span class="sxs-lookup"><span data-stu-id="7ddae-224">In this example, hello storage account name is **myStorageAccount**.</span></span> <span data-ttu-id="7ddae-225">Příklad názvu hello nahraďte hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="7ddae-225">Replace hello example name with hello name of your own storage account.</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

<span data-ttu-id="7ddae-226">Dalším krokem je tooPrepare hello účet úložiště pro migraci</span><span class="sxs-lookup"><span data-stu-id="7ddae-226">Next step is tooPrepare hello storage account for migration</span></span>

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

<span data-ttu-id="7ddae-227">Zkontrolujte konfiguraci hello hello připravenou účet úložiště pomocí prostředí Azure PowerShell nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7ddae-227">Check hello configuration for hello prepared storage account by using either Azure PowerShell or hello Azure portal.</span></span> <span data-ttu-id="7ddae-228">Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-228">If you are not ready for migration and you want toogo back toohello old state, use hello following command:</span></span>

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

<span data-ttu-id="7ddae-229">Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7ddae-229">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command:</span></span>

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a><span data-ttu-id="7ddae-230">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ddae-230">Next steps</span></span>
* [<span data-ttu-id="7ddae-231">Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7ddae-231">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="7ddae-232">Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7ddae-232">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="7ddae-233">Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7ddae-233">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="7ddae-234">Pomocí rozhraní příkazového řádku toomigrate IaaS prostředky z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7ddae-234">Use CLI toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="7ddae-235">Komunita nástroje asistence s migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7ddae-235">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="7ddae-236">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="7ddae-236">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="7ddae-237">Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7ddae-237">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
