---
title: "Migrovat virtuální počítače do Resource Manager pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek vás provede migraci podporované platformy prostředků z klasického do Azure Resource Manageru pomocí rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: a63d758570b09b37b8e51c639267f729521d9ae0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a><span data-ttu-id="4cf40-103">Migrovat prostředky infrastruktury z klasického do Azure Resource Manageru pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4cf40-103">Migrate IaaS resources from classic to Azure Resource Manager by using Azure CLI</span></span>
<span data-ttu-id="4cf40-104">Tyto kroky ukazují, jak používat příkazy rozhraní příkazového řádku Azure (CLI) k migraci infrastruktury jako služby (IaaS) prostředky z modelu nasazení classic do modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4cf40-104">These steps show you how to use Azure command-line interface (CLI) commands to migrate infrastructure as a service (IaaS) resources from the classic deployment model to the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="4cf40-105">Článek vyžaduje [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4cf40-105">The article requires the [Azure CLI](../../cli-install-nodejs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4cf40-106">Všechny operace, které jsou zde popsané jsou idempotent.</span><span class="sxs-lookup"><span data-stu-id="4cf40-106">All the operations described here are idempotent.</span></span> <span data-ttu-id="4cf40-107">Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme zopakovat Příprava, zrušení nebo potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="4cf40-107">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry the prepare, abort, or commit operation.</span></span> <span data-ttu-id="4cf40-108">Platforma se pak zkuste akci znovu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-108">The platform will then try the action again.</span></span>
> 
> 

<br>
<span data-ttu-id="4cf40-109">Tady je Vývojový diagram k identifikaci pořadí, ve kterém kroky se musí provést během procesu migrace</span><span class="sxs-lookup"><span data-stu-id="4cf40-109">Here is a flowchart to identify the order in which steps need to be executed during a migration process</span></span>

![Snímek obrazovky, který ukazuje kroky migrace](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a><span data-ttu-id="4cf40-111">Krok 1: Příprava na migraci</span><span class="sxs-lookup"><span data-stu-id="4cf40-111">Step 1: Prepare for migration</span></span>
<span data-ttu-id="4cf40-112">Tady je několik osvědčených postupů, které doporučujeme jak vyhodnotit migrace IaaS prostředky z classic do Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="4cf40-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic to Resource Manager:</span></span>

* <span data-ttu-id="4cf40-113">Pročtěte [seznam nepodporovaných konfigurací nebo funkce](../windows/migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4cf40-113">Read through the [list of unsupported configurations or features](../windows/migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="4cf40-114">Pokud máte virtuální počítače, které používají nepodporované konfigurace nebo funkce, doporučujeme počkejte funkce nebo konfigurační podpory, která má být oznámeno.</span><span class="sxs-lookup"><span data-stu-id="4cf40-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for the feature/configuration support to be announced.</span></span> <span data-ttu-id="4cf40-115">Alternativně můžete odebrat tuto funkci nebo přesunout mimo tuto konfiguraci, chcete-li migrovat, pokud ji vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="4cf40-115">Alternatively, you can remove that feature or move out of that configuration to enable migration if it suits your needs.</span></span>
* <span data-ttu-id="4cf40-116">Pokud máte automatizované skripty, které jsou dnes nasazení infrastruktury a aplikace, pokuste se vytvořit podobné nastavení testu pomocí těchto skriptů pro migraci.</span><span class="sxs-lookup"><span data-stu-id="4cf40-116">If you have automated scripts that deploy your infrastructure and applications today, try to create a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="4cf40-117">Alternativně můžete nastavit ukázkové prostředí pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-117">Alternatively, you can set up sample environments by using the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4cf40-118">Application Gateway nejsou aktuálně podporovány pro migraci z classic do Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4cf40-118">Application Gateways are not currently supported for migration from classic to Resource Manager.</span></span> <span data-ttu-id="4cf40-119">Pokud chcete migrovat klasickou virtuální síť s aplikační brány, odstranění brány před spuštěním operace Příprava přesunout sítě.</span><span class="sxs-lookup"><span data-stu-id="4cf40-119">To migrate a classic virtual network with an Application gateway, remove the gateway before running a Prepare operation to move the network.</span></span> <span data-ttu-id="4cf40-120">Po dokončení migrace znovu připojte bránu ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-120">After you complete the migration, reconnect the gateway in Azure Resource Manager.</span></span> 
>
><span data-ttu-id="4cf40-121">Připojování k okruhy ExpressRoute v jiné předplatné brány ExpressRoute se nedají automaticky migrovat.</span><span class="sxs-lookup"><span data-stu-id="4cf40-121">ExpressRoute gateways connecting to ExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="4cf40-122">V takových případech odebrat bránu ExpressRoute, migrujte virtuální sítě a znovu vytvořit bránu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-122">In such cases, remove the ExpressRoute gateway, migrate the virtual network and recreate the gateway.</span></span> <span data-ttu-id="4cf40-123">Najdete v tématu [okruhy ExpressRoute migrovat a přidružené virtuální sítě z klasického modelu nasazení Resource Manager](../../expressroute/expressroute-migration-classic-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4cf40-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from the classic to the Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a><span data-ttu-id="4cf40-124">Krok 2: Nastavte předplatné a zaregistrujte zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="4cf40-124">Step 2: Set your subscription and register the provider</span></span>
<span data-ttu-id="4cf40-125">Pro scénáře migrace, budete muset nastavit svoje prostředí pro obě classic a Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4cf40-125">For migration scenarios, you need to set up your environment for both classic and Resource Manager.</span></span> <span data-ttu-id="4cf40-126">[Instalace rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) a [vyberte své předplatné](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="4cf40-126">[Install Azure CLI](../../cli-install-nodejs.md) and [select your subscription](../../xplat-cli-connect.md).</span></span>

<span data-ttu-id="4cf40-127">Přihlášení ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-127">Sign-in to your account.</span></span>

    azure login

<span data-ttu-id="4cf40-128">Pomocí následujícího příkazu vyberte předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-128">Select the Azure subscription by using the following command.</span></span>

    azure account set "<azure-subscription-name>"

> [!NOTE]
> <span data-ttu-id="4cf40-129">Registrace je čas krok, ale je nutné provést jednou před pokusem o migraci.</span><span class="sxs-lookup"><span data-stu-id="4cf40-129">Registration is a one time step but it needs to be done once before attempting migration.</span></span> <span data-ttu-id="4cf40-130">Bez registrace se zobrazí následující chybová zpráva</span><span class="sxs-lookup"><span data-stu-id="4cf40-130">Without registering you'll see the following error message</span></span> 
> 
> <span data-ttu-id="4cf40-131">*Struktura BadRequest: Předplatné není zaregistrované pro migraci.*</span><span class="sxs-lookup"><span data-stu-id="4cf40-131">*BadRequest : Subscription is not registered for migration.*</span></span> 
> 
> 

<span data-ttu-id="4cf40-132">Zaregistrovat u zprostředkovatele prostředků migrace pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-132">Register with the migration resource provider by using the following command.</span></span> <span data-ttu-id="4cf40-133">Všimněte si, že v některých případech tento příkaz časového limitu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-133">Note that in some cases, this command times out.</span></span> <span data-ttu-id="4cf40-134">Registrace však bude úspěšné.</span><span class="sxs-lookup"><span data-stu-id="4cf40-134">However, the registration will be successful.</span></span>

    azure provider register Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="4cf40-135">Počkejte 5 minut pro registraci dokončit.</span><span class="sxs-lookup"><span data-stu-id="4cf40-135">Please wait five minutes for the registration to finish.</span></span> <span data-ttu-id="4cf40-136">Pomocí následujícího příkazu můžete zkontrolovat stav schválení.</span><span class="sxs-lookup"><span data-stu-id="4cf40-136">You can check the status of the approval by using the following command.</span></span> <span data-ttu-id="4cf40-137">Ujistěte se, že je RegistrationState `Registered` než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="4cf40-137">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

    azure provider show Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="4cf40-138">Nyní přepínat rozhraní příkazového řádku pro `asm` režimu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-138">Now switch CLI to the `asm` mode.</span></span>

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="4cf40-139">Krok 3: Zkontrolujte, zda že máte dostatečný počet jader virtuálního počítače Azure Resource Manager v oblasti Azure vaše aktuální nasazení nebo virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="4cf40-139">Step 3: Make sure you have enough Azure Resource Manager Virtual Machine cores in the Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="4cf40-140">V tomto kroku budete potřebovat přepnout do `arm` režimu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-140">For this step you'll need to switch to `arm` mode.</span></span> <span data-ttu-id="4cf40-141">To lze proveďte pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-141">Do this with the following command.</span></span>

```
azure config mode arm
```

<span data-ttu-id="4cf40-142">Následující příkaz rozhraní příkazového řádku můžete zkontrolovat aktuální množství jader, které máte ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-142">You can use the following CLI command to check the current amount of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="4cf40-143">Další informace o základní kvóty, najdete v části [omezení a Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span><span class="sxs-lookup"><span data-stu-id="4cf40-143">To learn more about core quotas, see [Limits and the Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span></span>

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

<span data-ttu-id="4cf40-144">Po dokončení ověření tento krok, můžete přepnout zpět na `asm` režimu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-144">Once you're done verifying this step, you can switch back to `asm` mode.</span></span>

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a><span data-ttu-id="4cf40-145">Krok 4: Možnost 1 - migraci virtuálních počítačů v rámci cloudové služby</span><span class="sxs-lookup"><span data-stu-id="4cf40-145">Step 4: Option 1 - Migrate virtual machines in a cloud service</span></span>
<span data-ttu-id="4cf40-146">Získání seznamu cloudových služeb pomocí následujícího příkazu a pak vyberte cloudovou službu, která chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="4cf40-146">Get the list of cloud services by using the following command, and then pick the cloud service that you want to migrate.</span></span> <span data-ttu-id="4cf40-147">Pamatujte, že pokud jsou virtuální počítače v rámci cloudové služby ve virtuální síti nebo pokud budou mít web/role pracovního procesu, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="4cf40-147">Note that if the VMs in the cloud service are in a virtual network or if they have web/worker roles, you will get an error message.</span></span>

    azure service list

<span data-ttu-id="4cf40-148">Spusťte následující příkaz pro získání názvu nasazení pro cloudovou službu z podrobný výstup.</span><span class="sxs-lookup"><span data-stu-id="4cf40-148">Run the following command to get the deployment name for the cloud service from the verbose output.</span></span> <span data-ttu-id="4cf40-149">Ve většině případů název nasazení je stejný jako název cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="4cf40-149">In most cases, the deployment name is the same as the cloud service name.</span></span>

    azure service show <serviceName> -vv

<span data-ttu-id="4cf40-150">Nejprve ověřte, jestli je možné migrovat cloudové služby, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4cf40-150">First, validate if you can migrate the cloud service using the following commands:</span></span>

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

<span data-ttu-id="4cf40-151">Vypněte virtuální počítače v rámci cloudové služby pro migraci.</span><span class="sxs-lookup"><span data-stu-id="4cf40-151">Prepare the virtual machines in the cloud service for migration.</span></span> <span data-ttu-id="4cf40-152">Máte dvě možnosti, které lze vybírat.</span><span class="sxs-lookup"><span data-stu-id="4cf40-152">You have two options to choose from.</span></span>

<span data-ttu-id="4cf40-153">Pokud chcete migrovat virtuální počítače k virtuální síti vytvořené platformy, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4cf40-153">If you want to migrate the VMs to a platform-created virtual network, use the following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

<span data-ttu-id="4cf40-154">Pokud chcete migrovat na existující virtuální síť v modelu nasazení Resource Manager, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4cf40-154">If you want to migrate to an existing virtual network in the Resource Manager deployment model, use the following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

<span data-ttu-id="4cf40-155">Po úspěšné operace Příprava můžete zobrazit prostřednictvím podrobný výstup, pokud chcete získat stav migrace virtuálních počítačů a ujistěte se, že jsou v `Prepared` stavu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-155">After the prepare operation is successful, you can look through the verbose output to get the migration state of the VMs and ensure that they are in the `Prepared` state.</span></span>

    azure vm show <vmName> -vv

<span data-ttu-id="4cf40-156">Zkontrolujte konfiguraci pro připravené prostředky pomocí rozhraní příkazového řádku nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-156">Check the configuration for the prepared resources by using either CLI or the Azure portal.</span></span> <span data-ttu-id="4cf40-157">Pokud si nejste připravený pro migraci a chcete přejít zpět do původního stavu, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4cf40-157">If you are not ready for migration and you want to go back to the old state, use the following command.</span></span>

    azure service deployment abort-migration <serviceName> <deploymentName>

<span data-ttu-id="4cf40-158">Pokud připravené konfigurací spokojeni, můžete přejít a potvrdit prostředky pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-158">If the prepared configuration looks good, you can move forward and commit the resources by using the following command.</span></span>

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="4cf40-159">Krok 4: Možnost 2 - migrovat virtuální počítače ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="4cf40-159">Step 4: Option 2 -  Migrate virtual machines in a virtual network</span></span>
<span data-ttu-id="4cf40-160">Vyberte virtuální síť, která chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="4cf40-160">Pick the virtual network that you want to migrate.</span></span> <span data-ttu-id="4cf40-161">Všimněte si, že pokud virtuální síť obsahuje webové/role pracovního procesu nebo virtuální počítače s nepodporované konfigurace, obdržíte chybovou zprávu ověření.</span><span class="sxs-lookup"><span data-stu-id="4cf40-161">Note that if the virtual network contains web/worker roles or VMs with unsupported configurations, you will get a validation error message.</span></span>

<span data-ttu-id="4cf40-162">Pomocí následujícího příkazu získáte všechny virtuální sítě v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="4cf40-162">Get all the virtual networks in the subscription by using the following command.</span></span>

    azure network vnet list

<span data-ttu-id="4cf40-163">Výstup bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="4cf40-163">The output will look something like this:</span></span>

![Snímek obrazovky s celý virtuální síť s názvem zvýrazněná příkazového řádku.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

<span data-ttu-id="4cf40-165">V předchozím příkladu **virtualNetworkName** je celý název **"Classicubuntu16 classicubuntu16 skupiny"**.</span><span class="sxs-lookup"><span data-stu-id="4cf40-165">In the above example, the **virtualNetworkName** is the entire name **"Group classicubuntu16 classicubuntu16"**.</span></span>

<span data-ttu-id="4cf40-166">Nejprve ověřte, jestli je možné migrovat virtuální sítě pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="4cf40-166">First, validate if you can migrate the virtual network using the following command:</span></span>

```shell
azure network vnet validate-migration <virtualNetworkName>
```

<span data-ttu-id="4cf40-167">Pomocí následujícího příkazu připravte virtuální síť podle svého výběru pro migraci.</span><span class="sxs-lookup"><span data-stu-id="4cf40-167">Prepare the virtual network of your choice for migration by using the following command.</span></span>

    azure network vnet prepare-migration <virtualNetworkName>

<span data-ttu-id="4cf40-168">Zkontrolujte konfiguraci pro virtuální počítače, který připravené pomocí rozhraní příkazového řádku nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-168">Check the configuration for the prepared virtual machines by using either CLI or the Azure portal.</span></span> <span data-ttu-id="4cf40-169">Pokud si nejste připravený pro migraci a chcete přejít zpět do původního stavu, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4cf40-169">If you are not ready for migration and you want to go back to the old state, use the following command.</span></span>

    azure network vnet abort-migration <virtualNetworkName>

<span data-ttu-id="4cf40-170">Pokud připravené konfigurací spokojeni, můžete přejít a potvrdit prostředky pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-170">If the prepared configuration looks good, you can move forward and commit the resources by using the following command.</span></span>

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a><span data-ttu-id="4cf40-171">Krok 5: Migrace účet úložiště</span><span class="sxs-lookup"><span data-stu-id="4cf40-171">Step 5: Migrate a storage account</span></span>
<span data-ttu-id="4cf40-172">Po dokončení migrace virtuálních počítačů, doporučujeme, migraci účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4cf40-172">Once you're done migrating the virtual machines, we recommend you migrate the storage account.</span></span>

<span data-ttu-id="4cf40-173">Pomocí následujícího příkazu připravte účet úložiště pro migraci</span><span class="sxs-lookup"><span data-stu-id="4cf40-173">Prepare the storage account for migration by using the following command</span></span>

    azure storage account prepare-migration <storageAccountName>

<span data-ttu-id="4cf40-174">Zkontrolujte konfiguraci pro účet úložiště připravené pomocí rozhraní příkazového řádku nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4cf40-174">Check the configuration for the prepared storage account by using either CLI or the Azure portal.</span></span> <span data-ttu-id="4cf40-175">Pokud si nejste připravený pro migraci a chcete přejít zpět do původního stavu, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="4cf40-175">If you are not ready for migration and you want to go back to the old state, use the following command.</span></span>

    azure storage account abort-migration <storageAccountName>

<span data-ttu-id="4cf40-176">Pokud připravené konfigurací spokojeni, můžete přejít a potvrdit prostředky pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="4cf40-176">If the prepared configuration looks good, you can move forward and commit the resources by using the following command.</span></span>

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a><span data-ttu-id="4cf40-177">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4cf40-177">Next steps</span></span>

* [<span data-ttu-id="4cf40-178">Přehled platformy podporované migrace z klasického do Azure Resource Manageru prostředků IaaS</span><span class="sxs-lookup"><span data-stu-id="4cf40-178">Overview of platform-supported migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="4cf40-179">Technické podrobné informace o platformy podporované migrace z klasického do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4cf40-179">Technical deep dive on platform-supported migration from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="4cf40-180">Plánování migrace prostředků IaaS z nasazení Classic do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4cf40-180">Planning for migration of IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="4cf40-181">Migrace prostředků IaaS z klasického do Azure Resource Manageru pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4cf40-181">Use PowerShell to migrate IaaS resources from classic to Azure Resource Manager</span></span>](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="4cf40-182">Komunita nástroje asistence s migrace z klasického do Azure Resource Manageru prostředků IaaS</span><span class="sxs-lookup"><span data-stu-id="4cf40-182">Community tools for assisting with migration of IaaS resources from classic to Azure Resource Manager</span></span>](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="4cf40-183">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="4cf40-183">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="4cf40-184">Přečtěte si nejčastější dotazy o migraci prostředky infrastruktury jako služby z klasického do Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4cf40-184">Review the most frequently asked questions about migrating IaaS resources from classic to Azure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
