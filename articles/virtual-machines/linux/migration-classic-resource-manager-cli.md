---
title: "virtuální počítače tooResource aaaMigrate Manager pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek vás provede hello platformy podporované migrace prostředků z classic tooAzure Resource Manager pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 0b11f4bb1b4658b3e88bf7629147ed953b678556
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-cli"></a><span data-ttu-id="0f987-103">Migrovat prostředky infrastruktury z classic tooAzure Resource Manager pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="0f987-103">Migrate IaaS resources from classic tooAzure Resource Manager by using Azure CLI</span></span>
<span data-ttu-id="0f987-104">Tyto kroky vám ukážou, jak toouse rozhraní příkazového řádku Azure (CLI) příkazy toomigrate infrastruktury jako služby (IaaS) prostředky z modelu nasazení classic hello, toohello modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0f987-104">These steps show you how toouse Azure command-line interface (CLI) commands toomigrate infrastructure as a service (IaaS) resources from hello classic deployment model toohello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="0f987-105">článek Hello vyžaduje hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="0f987-105">hello article requires hello [Azure CLI](../../cli-install-nodejs.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0f987-106">Všechny operace hello postupu popsaného tady jsou idempotent.</span><span class="sxs-lookup"><span data-stu-id="0f987-106">All hello operations described here are idempotent.</span></span> <span data-ttu-id="0f987-107">Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, doporučujeme, abyste se znovu pokusíte hello připravit, zrušení nebo potvrzení operace.</span><span class="sxs-lookup"><span data-stu-id="0f987-107">If you have a problem other than an unsupported feature or a configuration error, we recommend that you retry hello prepare, abort, or commit operation.</span></span> <span data-ttu-id="0f987-108">Hello platformy a zkuste to znovu hello akce.</span><span class="sxs-lookup"><span data-stu-id="0f987-108">hello platform will then try hello action again.</span></span>
> 
> 

<br>
<span data-ttu-id="0f987-109">Tady je pořadí tooidentify hello vývojový diagram, ve kterém kroky nutné toobe provést během procesu migrace</span><span class="sxs-lookup"><span data-stu-id="0f987-109">Here is a flowchart tooidentify hello order in which steps need toobe executed during a migration process</span></span>

![Snímek obrazovky, který zobrazuje kroky migrace hello](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a><span data-ttu-id="0f987-111">Krok 1: Příprava na migraci</span><span class="sxs-lookup"><span data-stu-id="0f987-111">Step 1: Prepare for migration</span></span>
<span data-ttu-id="0f987-112">Tady je několik osvědčených postupů, které doporučujeme jak vyhodnotit migrace prostředky infrastruktury z classic tooResource Manager:</span><span class="sxs-lookup"><span data-stu-id="0f987-112">Here are a few best practices that we recommend as you evaluate migrating IaaS resources from classic tooResource Manager:</span></span>

* <span data-ttu-id="0f987-113">Pročtěte hello [seznam nepodporovaných konfigurací nebo funkce](../windows/migration-classic-resource-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0f987-113">Read through hello [list of unsupported configurations or features](../windows/migration-classic-resource-manager-overview.md).</span></span> <span data-ttu-id="0f987-114">Pokud máte virtuální počítače, které používají nepodporované konfigurace nebo funkce, doporučujeme počkejte hello konfigurace funkcí nebo podporu toobe oznámeno.</span><span class="sxs-lookup"><span data-stu-id="0f987-114">If you have virtual machines that use unsupported configurations or features, we recommend that you wait for hello feature/configuration support toobe announced.</span></span> <span data-ttu-id="0f987-115">Alternativně můžete odebrat tuto funkci nebo přesunout mimo tuto tooenable migraci konfigurace, pokud ji vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="0f987-115">Alternatively, you can remove that feature or move out of that configuration tooenable migration if it suits your needs.</span></span>
* <span data-ttu-id="0f987-116">Pokud máte automatizované skripty, které jsou dnes nasazení infrastruktury a aplikace, zkuste toocreate podobné nastavení testu pomocí těchto skriptů pro migraci.</span><span class="sxs-lookup"><span data-stu-id="0f987-116">If you have automated scripts that deploy your infrastructure and applications today, try toocreate a similar test setup by using those scripts for migration.</span></span> <span data-ttu-id="0f987-117">Alternativně můžete nastavit ukázkové prostředí pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0f987-117">Alternatively, you can set up sample environments by using hello Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0f987-118">Application Gateway nejsou aktuálně podporovány pro migraci z classic tooResource správce.</span><span class="sxs-lookup"><span data-stu-id="0f987-118">Application Gateways are not currently supported for migration from classic tooResource Manager.</span></span> <span data-ttu-id="0f987-119">Před spuštěním síť Příprava operace toomove hello odebrat toomigrate klasickou virtuální síť s aplikační brány, brány hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-119">toomigrate a classic virtual network with an Application gateway, remove hello gateway before running a Prepare operation toomove hello network.</span></span> <span data-ttu-id="0f987-120">Po dokončení migrace hello znovu hello brány ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="0f987-120">After you complete hello migration, reconnect hello gateway in Azure Resource Manager.</span></span> 
>
><span data-ttu-id="0f987-121">Připojení tooExpressRoute okruhů v jiné předplatné brány ExpressRoute se nedají automaticky migrovat.</span><span class="sxs-lookup"><span data-stu-id="0f987-121">ExpressRoute gateways connecting tooExpressRoute circuits in another subscription cannot be migrated automatically.</span></span> <span data-ttu-id="0f987-122">V takových případech odeberte hello brány ExpressRoute, migrovat hello virtuální síť a znovu hello brány.</span><span class="sxs-lookup"><span data-stu-id="0f987-122">In such cases, remove hello ExpressRoute gateway, migrate hello virtual network and recreate hello gateway.</span></span> <span data-ttu-id="0f987-123">Najdete v tématu [migrovat ExpressRoute okruhy a přidružené virtuální sítě z modelu nasazení Resource Manager classic toohello hello](../../expressroute/expressroute-migration-classic-resource-manager.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0f987-123">Please see [Migrate ExpressRoute circuits and associated virtual networks from hello classic toohello Resource Manager deployment model](../../expressroute/expressroute-migration-classic-resource-manager.md) for more information.</span></span>
> 
> 

## <a name="step-2-set-your-subscription-and-register-hello-provider"></a><span data-ttu-id="0f987-124">Krok 2: Nastavte předplatné a zaregistrujte zprostředkovatele hello</span><span class="sxs-lookup"><span data-stu-id="0f987-124">Step 2: Set your subscription and register hello provider</span></span>
<span data-ttu-id="0f987-125">Pro scénáře migrace, je třeba tooset prostředí pro obě classic a Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0f987-125">For migration scenarios, you need tooset up your environment for both classic and Resource Manager.</span></span> <span data-ttu-id="0f987-126">[Instalace rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) a [vyberte své předplatné](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="0f987-126">[Install Azure CLI](../../cli-install-nodejs.md) and [select your subscription](../../xplat-cli-connect.md).</span></span>

<span data-ttu-id="0f987-127">Tooyour přihlašovací účet.</span><span class="sxs-lookup"><span data-stu-id="0f987-127">Sign-in tooyour account.</span></span>

    azure login

<span data-ttu-id="0f987-128">Vyberte hello předplatného Azure pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-128">Select hello Azure subscription by using hello following command.</span></span>

    azure account set "<azure-subscription-name>"

> [!NOTE]
> <span data-ttu-id="0f987-129">Registrace je v jednu chvíli krok ale vyžaduje toobe provádí jednou před pokusem o migraci.</span><span class="sxs-lookup"><span data-stu-id="0f987-129">Registration is a one time step but it needs toobe done once before attempting migration.</span></span> <span data-ttu-id="0f987-130">Bez registrace se zobrazí následující chybová zpráva hello</span><span class="sxs-lookup"><span data-stu-id="0f987-130">Without registering you'll see hello following error message</span></span> 
> 
> <span data-ttu-id="0f987-131">*Struktura BadRequest: Předplatné není zaregistrované pro migraci.*</span><span class="sxs-lookup"><span data-stu-id="0f987-131">*BadRequest : Subscription is not registered for migration.*</span></span> 
> 
> 

<span data-ttu-id="0f987-132">Zaregistrovat u zprostředkovatele prostředků migrace hello pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-132">Register with hello migration resource provider by using hello following command.</span></span> <span data-ttu-id="0f987-133">Všimněte si, že v některých případech tento příkaz časového limitu. Registrace hello však bude úspěšné.</span><span class="sxs-lookup"><span data-stu-id="0f987-133">Note that in some cases, this command times out. However, hello registration will be successful.</span></span>

    azure provider register Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="0f987-134">Počkejte pět minut, než toofinish registrace hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-134">Please wait five minutes for hello registration toofinish.</span></span> <span data-ttu-id="0f987-135">Stav hello hello schválení můžete zkontrolovat pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-135">You can check hello status of hello approval by using hello following command.</span></span> <span data-ttu-id="0f987-136">Ujistěte se, že je RegistrationState `Registered` než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="0f987-136">Make sure that RegistrationState is `Registered` before you proceed.</span></span>

    azure provider show Microsoft.ClassicInfrastructureMigrate

<span data-ttu-id="0f987-137">Nyní přepínač příkazového řádku toohello `asm` režimu.</span><span class="sxs-lookup"><span data-stu-id="0f987-137">Now switch CLI toohello `asm` mode.</span></span>

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a><span data-ttu-id="0f987-138">Krok 3: Zkontrolujte, zda že máte dostatečný počet jader virtuálního počítače Azure Resource Manager v hello oblast Azure vaše aktuální nasazení nebo virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="0f987-138">Step 3: Make sure you have enough Azure Resource Manager Virtual Machine cores in hello Azure region of your current deployment or VNET</span></span>
<span data-ttu-id="0f987-139">V tomto kroku budete potřebovat tooswitch příliš`arm` režimu.</span><span class="sxs-lookup"><span data-stu-id="0f987-139">For this step you'll need tooswitch too`arm` mode.</span></span> <span data-ttu-id="0f987-140">To lze proveďte pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-140">Do this with hello following command.</span></span>

```
azure config mode arm
```

<span data-ttu-id="0f987-141">Můžete použít hello následující rozhraní příkazového řádku příkaz toocheck hello aktuální velikost jádra, které máte ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="0f987-141">You can use hello following CLI command toocheck hello current amount of cores you have in Azure Resource Manager.</span></span> <span data-ttu-id="0f987-142">toolearn Další informace o základní kvóty, najdete v části [omezení a hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span><span class="sxs-lookup"><span data-stu-id="0f987-142">toolearn more about core quotas, see [Limits and hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)</span></span>

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

<span data-ttu-id="0f987-143">Po dokončení ověření tento krok, můžete přepnout zpět příliš`asm` režimu.</span><span class="sxs-lookup"><span data-stu-id="0f987-143">Once you're done verifying this step, you can switch back too`asm` mode.</span></span>

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a><span data-ttu-id="0f987-144">Krok 4: Možnost 1 - migraci virtuálních počítačů v rámci cloudové služby</span><span class="sxs-lookup"><span data-stu-id="0f987-144">Step 4: Option 1 - Migrate virtual machines in a cloud service</span></span>
<span data-ttu-id="0f987-145">Získání seznamu hello cloudových služeb pomocí hello následující příkaz a potom vyberte hello cloudové služby, které chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="0f987-145">Get hello list of cloud services by using hello following command, and then pick hello cloud service that you want toomigrate.</span></span> <span data-ttu-id="0f987-146">Všimněte si, že pokud hello virtuálních počítačů v rámci hello cloudové služby jsou ve virtuální síti nebo mají webové/role pracovního procesu, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="0f987-146">Note that if hello VMs in hello cloud service are in a virtual network or if they have web/worker roles, you will get an error message.</span></span>

    azure service list

<span data-ttu-id="0f987-147">Spusťte následující příkaz tooget hello název nasazení pro cloudovou službu hello z podrobný výstup hello hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-147">Run hello following command tooget hello deployment name for hello cloud service from hello verbose output.</span></span> <span data-ttu-id="0f987-148">Ve většině případů je hello název nasazení hello stejný jako název hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="0f987-148">In most cases, hello deployment name is hello same as hello cloud service name.</span></span>

    azure service show <serviceName> -vv

<span data-ttu-id="0f987-149">Nejprve ověřte, jestli je možné migrovat hello cloudové služby pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0f987-149">First, validate if you can migrate hello cloud service using hello following commands:</span></span>

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

<span data-ttu-id="0f987-150">Připravte hello virtuálních počítačů v rámci hello cloudové služby pro migraci.</span><span class="sxs-lookup"><span data-stu-id="0f987-150">Prepare hello virtual machines in hello cloud service for migration.</span></span> <span data-ttu-id="0f987-151">Máte dvě možnosti toochoose z.</span><span class="sxs-lookup"><span data-stu-id="0f987-151">You have two options toochoose from.</span></span>

<span data-ttu-id="0f987-152">Pokud chcete toomigrate hello virtuální počítače tooa platformy vytvořit virtuální síť, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-152">If you want toomigrate hello VMs tooa platform-created virtual network, use hello following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

<span data-ttu-id="0f987-153">Pokud chcete stávající virtuální sítě v modelu nasazení Resource Manager hello tooan toomigrate, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-153">If you want toomigrate tooan existing virtual network in hello Resource Manager deployment model, use hello following command.</span></span>

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

<span data-ttu-id="0f987-154">Po přípravě hello operace proběhne úspěšně, můžete projděte tooget podrobný výstup hello hello migrace stavu hello virtuální počítače a ujistěte se, že jsou v hello `Prepared` stavu.</span><span class="sxs-lookup"><span data-stu-id="0f987-154">After hello prepare operation is successful, you can look through hello verbose output tooget hello migration state of hello VMs and ensure that they are in hello `Prepared` state.</span></span>

    azure vm show <vmName> -vv

<span data-ttu-id="0f987-155">Zkontrolujte konfiguraci hello hello připravenou prostředky pomocí rozhraní CLI nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0f987-155">Check hello configuration for hello prepared resources by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="0f987-156">Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-156">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure service deployment abort-migration <serviceName> <deploymentName>

<span data-ttu-id="0f987-157">Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-157">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a><span data-ttu-id="0f987-158">Krok 4: Možnost 2 - migrovat virtuální počítače ve virtuální síti</span><span class="sxs-lookup"><span data-stu-id="0f987-158">Step 4: Option 2 -  Migrate virtual machines in a virtual network</span></span>
<span data-ttu-id="0f987-159">Vybrat hello virtuální sítě, které chcete toomigrate.</span><span class="sxs-lookup"><span data-stu-id="0f987-159">Pick hello virtual network that you want toomigrate.</span></span> <span data-ttu-id="0f987-160">Všimněte si, že pokud hello virtuální síť obsahuje role web nebo worker nebo virtuální počítače s nepodporované konfigurace, obdržíte chybovou zprávu ověření.</span><span class="sxs-lookup"><span data-stu-id="0f987-160">Note that if hello virtual network contains web/worker roles or VMs with unsupported configurations, you will get a validation error message.</span></span>

<span data-ttu-id="0f987-161">Získáte všechny hello virtuální sítě v rámci předplatného hello pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-161">Get all hello virtual networks in hello subscription by using hello following command.</span></span>

    azure network vnet list

<span data-ttu-id="0f987-162">Hello výstup bude vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="0f987-162">hello output will look something like this:</span></span>

![Snímek obrazovky hello příkazového řádku s zvýrazněná název hello celý virtuální sítě.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

<span data-ttu-id="0f987-164">V hello výše příklad hello **virtualNetworkName** je celý název hello **"Classicubuntu16 classicubuntu16 skupiny"**.</span><span class="sxs-lookup"><span data-stu-id="0f987-164">In hello above example, hello **virtualNetworkName** is hello entire name **"Group classicubuntu16 classicubuntu16"**.</span></span>

<span data-ttu-id="0f987-165">Nejprve ověřte, jestli je možné migrovat hello virtuální sítě pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0f987-165">First, validate if you can migrate hello virtual network using hello following command:</span></span>

```shell
azure network vnet validate-migration <virtualNetworkName>
```

<span data-ttu-id="0f987-166">Připravte hello virtuální síť podle svého výběru pro migraci pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-166">Prepare hello virtual network of your choice for migration by using hello following command.</span></span>

    azure network vnet prepare-migration <virtualNetworkName>

<span data-ttu-id="0f987-167">Zkontrolujte konfiguraci hello hello připravit virtuální počítače pomocí rozhraní příkazového řádku nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0f987-167">Check hello configuration for hello prepared virtual machines by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="0f987-168">Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-168">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure network vnet abort-migration <virtualNetworkName>

<span data-ttu-id="0f987-169">Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-169">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a><span data-ttu-id="0f987-170">Krok 5: Migrace účet úložiště</span><span class="sxs-lookup"><span data-stu-id="0f987-170">Step 5: Migrate a storage account</span></span>
<span data-ttu-id="0f987-171">Po dokončení migrace hello virtuálních počítačů, doporučujeme, abyste že migrujete hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="0f987-171">Once you're done migrating hello virtual machines, we recommend you migrate hello storage account.</span></span>

<span data-ttu-id="0f987-172">Příprava na migraci účtu úložiště hello pomocí hello následující příkaz</span><span class="sxs-lookup"><span data-stu-id="0f987-172">Prepare hello storage account for migration by using hello following command</span></span>

    azure storage account prepare-migration <storageAccountName>

<span data-ttu-id="0f987-173">Zkontrolujte konfiguraci hello hello připravenou účet úložiště pomocí rozhraní CLI nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0f987-173">Check hello configuration for hello prepared storage account by using either CLI or hello Azure portal.</span></span> <span data-ttu-id="0f987-174">Pokud si nejste připravený pro migraci a chcete toogo back toohello starý stav, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="0f987-174">If you are not ready for migration and you want toogo back toohello old state, use hello following command.</span></span>

    azure storage account abort-migration <storageAccountName>

<span data-ttu-id="0f987-175">Pokud hello připravené konfigurací spokojeni, můžete přejít a potvrdit hello prostředky pomocí hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="0f987-175">If hello prepared configuration looks good, you can move forward and commit hello resources by using hello following command.</span></span>

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a><span data-ttu-id="0f987-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f987-176">Next steps</span></span>

* [<span data-ttu-id="0f987-177">Přehled migrace podporované platformy IaaS prostředků z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f987-177">Overview of platform-supported migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="0f987-178">Technické podrobné informace o platformy podporované migrace z klasického tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f987-178">Technical deep dive on platform-supported migration from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="0f987-179">Plánování migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f987-179">Planning for migration of IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="0f987-180">Pomocí prostředí PowerShell toomigrate IaaS prostředky z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f987-180">Use PowerShell toomigrate IaaS resources from classic tooAzure Resource Manager</span></span>](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="0f987-181">Komunita nástroje asistence s migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f987-181">Community tools for assisting with migration of IaaS resources from classic tooAzure Resource Manager</span></span>](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [<span data-ttu-id="0f987-182">Běžné chyby při migraci</span><span class="sxs-lookup"><span data-stu-id="0f987-182">Review most common migration errors</span></span>](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="0f987-183">Zkontrolujte hello nejčastěji kladené dotazy týkající se migrace prostředky infrastruktury z classic tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f987-183">Review hello most frequently asked questions about migrating IaaS resources from classic tooAzure Resource Manager</span></span>](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
