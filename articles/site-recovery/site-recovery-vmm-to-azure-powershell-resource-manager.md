---
title: "virtuální počítače aaaReplicate technologie Hyper-V v cloudech VMM pomocí Azure Site Recovery a prostředí PowerShell (Resource Manager) | Microsoft Docs"
description: "Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM pomocí Azure Site Recovery a prostředí PowerShell"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: b0f641de4e10a600ead415ceb9bd488fb4d1659d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="6e511-103">Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudů VMM pomocí prostředí PowerShell a Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e511-103">Replicate Hyper-V virtual machines in VMM clouds tooAzure using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e511-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e511-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="6e511-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6e511-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="6e511-106">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="6e511-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="6e511-107">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="6e511-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="6e511-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="6e511-108">Overview</span></span>
<span data-ttu-id="6e511-109">Azure Site Recovery přispívá tooyour obchodní kontinuitu a po havárii (BCDR) strategii zotavení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení.</span><span class="sxs-lookup"><span data-stu-id="6e511-109">Azure Site Recovery contributes tooyour business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="6e511-110">Úplný seznam nasazení scénářů najdete v části hello [přehled Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6e511-110">For a full list of deployment scenarios see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="6e511-111">Tento článek ukazuje, jak toouse prostředí PowerShell tooautomate běžné úlohy je nutné tooperform při nastavení Azure Site Recovery tooreplicate technologie Hyper-V virtuálních počítačů v System Center VMM cloudy tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="6e511-111">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooAzure storage.</span></span>

<span data-ttu-id="6e511-112">Hello článek obsahuje požadavky pro hello scénář a ukazuje</span><span class="sxs-lookup"><span data-stu-id="6e511-112">hello article includes prerequisites for hello scenario, and shows you</span></span>

* <span data-ttu-id="6e511-113">Jak tooset do trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="6e511-113">How tooset up a Recovery Services Vault</span></span>
* <span data-ttu-id="6e511-114">Nainstalujte na zdrojovém serveru VMM hello hello zprostředkovatele Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6e511-114">Install hello Azure Site Recovery Provider on hello source VMM server</span></span>
* <span data-ttu-id="6e511-115">Zaregistrujte hello server v hello trezoru, přidat účet úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6e511-115">Register hello server in hello vault, add an Azure storage account</span></span>
* <span data-ttu-id="6e511-116">Nainstalujte agenta služeb zotavení Azure hello na hostitelských serverech technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="6e511-116">Install hello Azure Recovery Services agent on Hyper-V host servers</span></span>
* <span data-ttu-id="6e511-117">Konfigurovat nastavení ochrany pro cloudy VMM, které budou použité tooall chráněné virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="6e511-117">Configure protection settings for VMM clouds, that will be applied tooall protected virtual machines</span></span>
* <span data-ttu-id="6e511-118">Povolení ochrany pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6e511-118">Enable protection for those virtual machines.</span></span>
* <span data-ttu-id="6e511-119">Testovací převzetí služeb při selhání toomake hello, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="6e511-119">Test hello fail-over toomake sure everything's working as expected.</span></span>

<span data-ttu-id="6e511-120">Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6e511-120">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="6e511-121">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6e511-121">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="6e511-122">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-122">This article covers using hello Resource Manager deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="6e511-123">Než začnete</span><span class="sxs-lookup"><span data-stu-id="6e511-123">Before you start</span></span>
<span data-ttu-id="6e511-124">Ujistěte se, že máte zavedenou tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="6e511-124">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="6e511-125">Požadavky Azure</span><span class="sxs-lookup"><span data-stu-id="6e511-125">Azure prerequisites</span></span>
* <span data-ttu-id="6e511-126">Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="6e511-126">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="6e511-127">Pokud nemáte, začínat [bezplatný účet](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="6e511-127">If you don't have one, start with a [free account](https://azure.microsoft.com/free).</span></span> <span data-ttu-id="6e511-128">Kromě toho si můžete přečíst o hello [cenách Azure Site Recovery Manageru](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="6e511-128">In addition, you can read about hello [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="6e511-129">Pokud se pokoušíte se hello replikace tooa CSP předplatné scénáři budete potřebovat předplatného poskytovatele CSP.</span><span class="sxs-lookup"><span data-stu-id="6e511-129">You'll need a CSP subscription if you are trying out hello replication tooa CSP subscription scenario.</span></span> <span data-ttu-id="6e511-130">Další informace o programu hello CSP v [jak tooenroll CSP programu hello](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e511-130">Learn more about hello CSP program in [how tooenroll in hello CSP program](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).</span></span>
* <span data-ttu-id="6e511-131">Budete potřebovat tooAzure dat replikovaných účet toostore Azure v2 úložiště (Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="6e511-131">You'll need an Azure v2 storage (Resource Manager) account toostore data replicated tooAzure.</span></span> <span data-ttu-id="6e511-132">Hello účet potřebuje povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="6e511-132">hello account needs geo-replication enabled.</span></span> <span data-ttu-id="6e511-133">Je nutné v hello stejné oblasti jako hello služba Azure Site Recovery a možné přidružit k hello stejného předplatného nebo hello předplatného poskytovatele CSP.</span><span class="sxs-lookup"><span data-stu-id="6e511-133">It should be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription or hello CSP subscription.</span></span> <span data-ttu-id="6e511-134">toolearn Další informace o nastavení služby Azure storage, najdete v části hello [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md) pro referenci.</span><span class="sxs-lookup"><span data-stu-id="6e511-134">toolearn more about setting up Azure storage, see hello [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) for reference.</span></span>
* <span data-ttu-id="6e511-135">Musíte mít jistotu, že virtuální počítače, které chcete tooprotect vyhovují hello toomake [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="6e511-135">You'll need toomake sure that virtual machines you want tooprotect comply with hello [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

> [!NOTE]
> <span data-ttu-id="6e511-136">V současné době pouze úrovně operace virtuálního počítače je možné pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="6e511-136">Currently, only VM level operations are possible through Powershell.</span></span> <span data-ttu-id="6e511-137">Podpora pro úrovně operace plánu obnovení, bude brzy se.</span><span class="sxs-lookup"><span data-stu-id="6e511-137">Support for recovery plan level operations will be made soon.</span></span>  <span data-ttu-id="6e511-138">Zatím jste omezené tooperforming selhání Center pouze na rozlišením "chráněné virtuální počítač" a ne na úrovni plán obnovení.</span><span class="sxs-lookup"><span data-stu-id="6e511-138">For now, you are limited tooperforming fail-overs only at a ‘protected VM’ granularity and not at a Recovery Plan level.</span></span>
>
>

### <a name="vmm-prerequisites"></a><span data-ttu-id="6e511-139">Požadavky VMM</span><span class="sxs-lookup"><span data-stu-id="6e511-139">VMM prerequisites</span></span>
* <span data-ttu-id="6e511-140">Budete potřebovat VMM server běžící na System Center 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="6e511-140">You'll need VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="6e511-141">Jakýkoli server VMM obsahuje virtuální počítače budete chtít tooprotect musí být spuštěna hello zprostředkovatele Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6e511-141">Any VMM server containing virtual machines you want tooprotect must be running hello Azure Site Recovery Provider.</span></span> <span data-ttu-id="6e511-142">To je nainstalován během hello nasazení Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6e511-142">This is installed during hello Azure Site Recovery deployment.</span></span>
* <span data-ttu-id="6e511-143">Budete potřebovat alespoň jeden cloud na serveru VMM má tooprotect hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-143">You'll need at least one cloud on hello VMM server you want tooprotect.</span></span> <span data-ttu-id="6e511-144">Hello cloud by měl obsahovat:</span><span class="sxs-lookup"><span data-stu-id="6e511-144">hello cloud should contain:</span></span>
  * <span data-ttu-id="6e511-145">Minimálně jednu skupinu hostitelů VMM.</span><span class="sxs-lookup"><span data-stu-id="6e511-145">One or more VMM host groups.</span></span>
  * <span data-ttu-id="6e511-146">Minimálně jeden hostitelský server nebo cluster Hyper-V v každé skupině hostitelů.</span><span class="sxs-lookup"><span data-stu-id="6e511-146">One or more Hyper-V host servers or clusters in each host group.</span></span>
  * <span data-ttu-id="6e511-147">Jeden nebo více virtuálních počítačů na serveru hello zdroj technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="6e511-147">One or more virtual machines on hello source Hyper-V server.</span></span>
* <span data-ttu-id="6e511-148">Další informace o nastavení cloudů VMM:</span><span class="sxs-lookup"><span data-stu-id="6e511-148">Learn more about setting up VMM clouds:</span></span>
  * <span data-ttu-id="6e511-149">Další informace o privátních cloudů VMM ve [co je nového v privátním cloudu s nástrojem System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) a v [cloudy VMM 2012 a hello](http://go.microsoft.com/fwlink/?LinkId=324956).</span><span class="sxs-lookup"><span data-stu-id="6e511-149">Read more about private VMM clouds in [What’s New in Private Cloud with System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) and in [VMM 2012 and hello clouds](http://go.microsoft.com/fwlink/?LinkId=324956).</span></span>
  * <span data-ttu-id="6e511-150">Další informace o [konfigurace hello VMM cloudových prostředků infrastruktury](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)</span><span class="sxs-lookup"><span data-stu-id="6e511-150">Learn about [Configuring hello VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)</span></span>
  * <span data-ttu-id="6e511-151">Po vaší cloudové infrastruktury prvky jsou zavedené Další informace o vytváření privátních cloudů ve [vytvoření privátního cloudu v nástroji VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) a v [návod: vytvoření privátních cloudů pomocí System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).</span><span class="sxs-lookup"><span data-stu-id="6e511-151">After your cloud fabric elements are in place learn about creating private clouds in [Creating a private cloud in VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) and in [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="6e511-152">Požadavky technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="6e511-152">Hyper-V prerequisites</span></span>
* <span data-ttu-id="6e511-153">Hello servery hostitele technologie Hyper-V musí běžet minimálně **systému Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a mít hello nainstalované nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="6e511-153">hello host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have hello latest updates installed.</span></span>
* <span data-ttu-id="6e511-154">Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách.</span><span class="sxs-lookup"><span data-stu-id="6e511-154">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="6e511-155">Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně.</span><span class="sxs-lookup"><span data-stu-id="6e511-155">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="6e511-156">Pro</span><span class="sxs-lookup"><span data-stu-id="6e511-156">For</span></span>
* <span data-ttu-id="6e511-157">Pokyny naleznete v části [jak tooConfigure zprostředkovatele replik technologie Hyper-V](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e511-157">For instructions see [How tooConfigure Hyper-V Replica Broker](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).</span></span>
* <span data-ttu-id="6e511-158">Libovolný server hostitele technologie Hyper-V nebo clusteru, pro které chcete toomanage ochrany musí být součástí cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="6e511-158">Any Hyper-V host server or cluster for which you want toomanage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="6e511-159">Požadavky na mapování sítě</span><span class="sxs-lookup"><span data-stu-id="6e511-159">Network mapping prerequisites</span></span>
<span data-ttu-id="6e511-160">Když chráníte virtuální počítače v Azure, hello mapování sítě zajišťuje mapování sítě virtuálních počítačů hello na serveru VMM hello zdrojové a cílové sítě Azure tooenable hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e511-160">When you protect virtual machines in Azure, hello network mapping  maps hello virtual machine networks on hello source VMM server and target Azure networks tooenable hello following:</span></span>

* <span data-ttu-id="6e511-161">Všechny počítače, které převzetí služeb při selhání na hello stejné tooeach jiných, bez ohledu na plánu obnovení se mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="6e511-161">All machines which fail over on hello same network can connect tooeach other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="6e511-162">Pokud bránu sítě je nastavena na hello cílovou síť Azure, můžete virtuální počítače připojit tooother místní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6e511-162">If a network gateway is setup on hello target Azure network, virtual machines can connect tooother on-premises virtual machines.</span></span>
* <span data-ttu-id="6e511-163">Pokud nenakonfigurujete mapování sítě, hello pouze virtuální počítače, které převzetí služeb při selhání v hello stejný plán obnovení bude možné tooconnect tooeach jiných po tooAzure převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e511-163">If you don’t configure network mapping, only hello virtual machines that fail over in hello same recovery plan will be able tooconnect tooeach other after fail-over tooAzure.</span></span>

<span data-ttu-id="6e511-164">Pokud chcete, aby mapování sítě toodeploy budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-164">If you want toodeploy network mapping you'll need hello following:</span></span>

* <span data-ttu-id="6e511-165">Hello virtuálních počítačů má tooprotect na zdrojovém serveru VMM hello by měl být připojený tooa síť virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e511-165">hello virtual machines you want tooprotect on hello source VMM server should be connected tooa VM network.</span></span> <span data-ttu-id="6e511-166">Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="6e511-166">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
* <span data-ttu-id="6e511-167">Po převzetí služeb při selhání můžete připojit síti Azure toowhich replikovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6e511-167">An Azure network toowhich replicated virtual machines can connect after fail-over.</span></span> <span data-ttu-id="6e511-168">Tuto síť vyberete v době hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e511-168">You'll select this network at hello time of fail-over.</span></span> <span data-ttu-id="6e511-169">Hello síť musí být ve hello stejné oblasti jako vaše předplatné Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6e511-169">hello network should be in hello same region as your Azure Site Recovery subscription.</span></span>

<span data-ttu-id="6e511-170">Další informace o mapování sítě v</span><span class="sxs-lookup"><span data-stu-id="6e511-170">Learn more about network mapping in</span></span>

* [<span data-ttu-id="6e511-171">Jak tooconfigure logické sítě v nástroji VMM</span><span class="sxs-lookup"><span data-stu-id="6e511-171">How tooconfigure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="6e511-172">Jak tooconfigure virtuálních počítačů sítí a bran v nástroji VMM</span><span class="sxs-lookup"><span data-stu-id="6e511-172">How tooconfigure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [<span data-ttu-id="6e511-173">Jak tooconfigure a monitorování virtuálních sítí v Azure</span><span class="sxs-lookup"><span data-stu-id="6e511-173">How tooconfigure and monitor virtual networks in Azure</span></span>](https://azure.microsoft.com/documentation/services/virtual-network/)

### <a name="powershell-prerequisites"></a><span data-ttu-id="6e511-174">Požadavky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e511-174">PowerShell prerequisites</span></span>
<span data-ttu-id="6e511-175">Ujistěte se, že máte připravené toogo prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e511-175">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="6e511-176">Pokud už používáte prostředí PowerShell, budete potřebovat tooupgrade tooversion 0.8.10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6e511-176">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="6e511-177">Informace o nastavení prostředí PowerShell najdete v tématu hello [Průvodce tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="6e511-177">For information about setting up PowerShell, see hello [Guide tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="6e511-178">Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny hello dostupných rutin služby hello [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6e511-178">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="6e511-179">toolearn o tipy, které vám pomohou při používání hello rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části hello [Průvodce Začínáme s rutinami Azure tooget](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="6e511-179">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see hello [Guide tooget Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="6e511-180">Krok 1: Nastavení odběru hello</span><span class="sxs-lookup"><span data-stu-id="6e511-180">Step 1: Set hello subscription</span></span>
1. <span data-ttu-id="6e511-181">Z prostředí Azure powershell, tooyour přihlašovací účet Azure: pomocí následující rutiny hello</span><span class="sxs-lookup"><span data-stu-id="6e511-181">From Azure powershell, login tooyour Azure account: using hello following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="6e511-182">Získejte seznam předplatných.</span><span class="sxs-lookup"><span data-stu-id="6e511-182">Get a list of your subscriptions.</span></span> <span data-ttu-id="6e511-183">Pro každé z předplatných hello to taky zobrazí seznam hello subscriptionIDs.</span><span class="sxs-lookup"><span data-stu-id="6e511-183">This will also list hello subscriptionIDs for each of hello subscriptions.</span></span> <span data-ttu-id="6e511-184">Poznamenejte si ID předplatného hello hello předplatného, ve kterém chcete trezor služeb zotavení toocreate hello</span><span class="sxs-lookup"><span data-stu-id="6e511-184">Note down hello subscriptionID of hello subscription in which you wish toocreate hello recovery services vault</span></span>

        Get-AzureRmSubscription
3. <span data-ttu-id="6e511-185">Nastavit hello předplatné, ve které hello trezoru služeb zotavení je toobe vytvořené zmínit ID předplatného hello</span><span class="sxs-lookup"><span data-stu-id="6e511-185">Set hello subscription in which hello recovery services vault is toobe created by mentioning hello subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="6e511-186">Krok 2 – Vytvoření trezoru Služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="6e511-186">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="6e511-187">Vytvořte skupinu prostředků ve službě Správce prostředků Azure, pokud již nemáte</span><span class="sxs-lookup"><span data-stu-id="6e511-187">Create a resource group  in Azure Resource Manager if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="6e511-188">Vytvořit nový trezor služeb zotavení a uložte hello vytvořený objekt trezoru automatické obnovení systému v proměnné (použijí později).</span><span class="sxs-lookup"><span data-stu-id="6e511-188">Create a new Recovery Services vault and save hello created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="6e511-189">Můžete také načíst hello automatické obnovení systému trezoru post vytvoření objektu pomocí rutiny Get-AzureRMRecoveryServicesVault hello:-</span><span class="sxs-lookup"><span data-stu-id="6e511-189">You can also retrieve hello ASR vault object post creation using hello Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="6e511-190">Krok 3: Nastavte kontext hello trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="6e511-190">Step 3: Set hello Recovery Services Vault context</span></span>

<span data-ttu-id="6e511-191">Nastavit kontext hello trezoru spuštěním hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="6e511-191">Set hello vault context by running hello below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="6e511-192">Krok 4: Instalace hello zprostředkovatele Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6e511-192">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="6e511-193">Na počítači VMM hello vytvořte adresář spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-193">On hello VMM machine, create a directory by running hello following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="6e511-194">Extrahujte soubory hello pomocí zprostředkovatele hello stáhnout tak, že spustíte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="6e511-194">Extract hello files using hello downloaded provider by running hello following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="6e511-195">Nainstalujte zprostředkovatele hello pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6e511-195">Install hello provider using hello following commands:</span></span>

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   <span data-ttu-id="6e511-196">Počkejte toofinish instalace hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-196">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="6e511-197">Hello server zaregistrujte v trezoru hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-197">Register hello server in hello vault using hello following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="6e511-198">Krok 5: Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="6e511-198">Step 5: Create an Azure storage account</span></span>

<span data-ttu-id="6e511-199">Pokud nemáte účet úložiště Azure, vytvořte účet povolenou geografickou replikací v hello stejném geograficky redundantním jako hello trezoru spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-199">If you don't have an Azure storage account, create a geo-replication enabled account in hello same geo as hello vault by running hello following command:</span></span>

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

<span data-ttu-id="6e511-200">Všimněte si, že účet úložiště hello musí být ve stejné oblasti jako služba Azure Site Recovery hello hello a přidružen hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="6e511-200">Note that hello storage account must be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription.</span></span>

## <a name="step-6-install-hello-azure-recovery-services-agent"></a><span data-ttu-id="6e511-201">Krok 6: Instalace hello agenta služeb zotavení Azure</span><span class="sxs-lookup"><span data-stu-id="6e511-201">Step 6: Install hello Azure Recovery Services Agent</span></span>
1. <span data-ttu-id="6e511-202">Stáhnout agenta služeb zotavení Azure hello v [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) a nainstalujte ji na každém serveru hostitele technologie Hyper-V umístěný v hello VMM cloudů je chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="6e511-202">Download hello Azure Recovery Services agent at [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) and install it on each Hyper-V host server located in hello VMM clouds you want tooprotect.</span></span>
2. <span data-ttu-id="6e511-203">Spusťte následující příkaz na všech hostitelích VMM hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-203">Run hello following command on all VMM hosts:</span></span>

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="6e511-204">Krok 7: Konfigurace cloudu nastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="6e511-204">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="6e511-205">Vytvořte tooAzure zásady replikace spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-205">Create a replication policy tooAzure by running hello following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. <span data-ttu-id="6e511-206">Kontejner ochrany získáte spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6e511-206">Get a protection container by running hello following commands:</span></span>

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. <span data-ttu-id="6e511-207">Získáte hello zásad podrobnosti tooa proměnnou pomocí hello úlohy, která byla vytvořena a zmínit hello zásad popisný název:</span><span class="sxs-lookup"><span data-stu-id="6e511-207">Get hello policy details tooa variable using hello job that was created and mentioning hello friendly policy name:</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="6e511-208">Přidružení hello kontejneru ochrany hello začněte zásady replikace hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-208">Start hello association of hello protection container with hello replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. <span data-ttu-id="6e511-209">Po dokončení úlohy hello spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-209">After hello job has finished, run hello following command:</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. <span data-ttu-id="6e511-210">Po dokončení zpracování úlohy hello spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-210">After hello job has finished processing, run hello following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="6e511-211">dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="6e511-211">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="6e511-212">Krok 8: Nakonfigurování mapování sítě</span><span class="sxs-lookup"><span data-stu-id="6e511-212">Step 8: Configure network mapping</span></span>
<span data-ttu-id="6e511-213">Před zahájením mapování sítě ověřte, zda virtuální počítače na zdrojovém serveru VMM hello jsou připojené tooa síť virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6e511-213">Before you begin network mapping verify that virtual machines on hello source VMM server are connected tooa VM network.</span></span> <span data-ttu-id="6e511-214">Kromě toho můžete vytvořte jeden nebo více virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="6e511-214">In addition, create one or more Azure virtual networks.</span></span>

<span data-ttu-id="6e511-215">Další informace o tom, jak toocreate a virtuální sítě pomocí Azure Resource Manageru a prostředí PowerShell v [vytvořit virtuální síť s připojením VPN typu site-to-site pomocí Azure Resource Manageru a prostředí PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="6e511-215">Learn more about how toocreate a virtual network using Azure Resource Manager and PowerShell, in [Create a virtual network with a site-to-site VPN connection using Azure Resource Manager and PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)</span></span>

<span data-ttu-id="6e511-216">Všimněte si, že více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.</span><span class="sxs-lookup"><span data-stu-id="6e511-216">Note that multiple Virtual Machine networks can be mapped tooa single Azure network.</span></span> <span data-ttu-id="6e511-217">Pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello nachází stejný název jako podsíť, na které hello zdrojového virtuálního počítače a potom virtuální počítač repliky hello budou po převzetí služeb při selhání připojené toothat cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="6e511-217">If hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after fail-over.</span></span> <span data-ttu-id="6e511-218">Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-218">If there is no target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>

1. <span data-ttu-id="6e511-219">První příkaz Hello získá servery pro aktuální trezoru Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-219">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="6e511-220">příkaz Hello ukládá v proměnné pole hello $Servers hello servery Microsoft Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="6e511-220">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="6e511-221">Hello druhém příkazu je získán hello síti pro obnovení lokality pro první server hello hello $Servers pole.</span><span class="sxs-lookup"><span data-stu-id="6e511-221">hello second command gets hello site recovery network for hello first server in hello $Servers array.</span></span> <span data-ttu-id="6e511-222">příkaz Hello ukládá hello sítě hello $Networks proměnné.</span><span class="sxs-lookup"><span data-stu-id="6e511-222">hello command stores hello networks in hello $Networks variable.</span></span>

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. <span data-ttu-id="6e511-223">třetí příkaz Hello získá virtuálních sítí Azure a pak tuto hodnotu v hello $AzureVmNetworks proměnné.</span><span class="sxs-lookup"><span data-stu-id="6e511-223">hello third command gets Azure virtual networks, and then that value in hello $AzureVmNetworks variable.</span></span>

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. <span data-ttu-id="6e511-224">Hello konečné rutina vytvoří mapování mezi primární a sítí virtuálních počítačů Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-224">hello final cmdlet creates a mapping between hello primary network and hello Azure virtual machine network.</span></span> <span data-ttu-id="6e511-225">rutiny Hello určuje hello primární síť jako první prvek $Networks hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-225">hello cmdlet specifies hello primary network as hello first element of $Networks.</span></span> <span data-ttu-id="6e511-226">rutiny Hello určuje síť virtuálních počítačů jako první prvek $AzureVmNetworks hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-226">hello cmdlet specifies a virtual machine network as hello first element of $AzureVmNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="6e511-227">Krok 9: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="6e511-227">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="6e511-228">Po hello serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-228">After hello servers, clouds and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span>

 <span data-ttu-id="6e511-229">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-229">Note hello following:</span></span>

* <span data-ttu-id="6e511-230">Virtuální počítače musí splňovat požadavky pro Azure.</span><span class="sxs-lookup"><span data-stu-id="6e511-230">Virtual machines must meet Azure requirements.</span></span> <span data-ttu-id="6e511-231">Zkontrolujte tyto v [požadavky a podpora](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) v příručce plánování na hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-231">Check these in [Prerequisites and Support](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) in hello planning guide.</span></span>
* <span data-ttu-id="6e511-232">tooenable ochrany, hello operačního systému a vlastnosti disku operačního systému musí být nastavena pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-232">tooenable protection, hello operating system and operating system disk properties must be set for hello virtual machine.</span></span> <span data-ttu-id="6e511-233">Při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-233">When you create a virtual machine in VMM using a virtual machine template you can set hello property.</span></span> <span data-ttu-id="6e511-234">Můžete také nastavit tyto vlastnosti pro existující virtuální počítače v hello **Obecné** a **konfigurace hardwaru** karty hello vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6e511-234">You can also set these properties for existing virtual machines on hello **General** and **Hardware Configuration** tabs of hello virtual machine properties.</span></span> <span data-ttu-id="6e511-235">Pokud tyto vlastnosti nenastavíte v nástroji VMM budete moct tooconfigure je na portálu Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-235">If you don't set these properties in VMM you'll be able tooconfigure them in hello Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="6e511-236">Ochrana tooenable, spusťte následující příkaz tooget hello ochrany kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-236">tooenable protection, run hello following command tooget hello protection container:</span></span>

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. <span data-ttu-id="6e511-237">Entita ochrany hello (VM) získáte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-237">Get hello protection entity (VM) by running hello following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. <span data-ttu-id="6e511-238">Povolte hello zotavení po Havárii pro hello virtuálních počítačů tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-238">Enable hello DR for hello VM by running hello following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a><span data-ttu-id="6e511-239">Otestujte nasazení</span><span class="sxs-lookup"><span data-stu-id="6e511-239">Test your deployment</span></span>
<span data-ttu-id="6e511-240">tootest nasazení můžete spustit test převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spusťte test selhání pro plán hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-240">tootest your deployment you can run a test fail-over for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test fail-over for hello plan.</span></span> <span data-ttu-id="6e511-241">Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a obnovení v izolované síti.</span><span class="sxs-lookup"><span data-stu-id="6e511-241">Test fail-over simulates your fail-over and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="6e511-242">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="6e511-242">Note that:</span></span>

* <span data-ttu-id="6e511-243">Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e511-243">If you want tooconnect toohello virtual machine in Azure using Remote Desktop after hello fail-over, enable Remote Desktop Connection on hello virtual machine before you run hello test failover.</span></span>
* <span data-ttu-id="6e511-244">Po převzetí služeb při selhání použijete veřejnou IP adresu tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="6e511-244">After fail-over, you'll use a public IP address tooconnect toohello virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="6e511-245">Pokud chcete, toodo to, ujistěte se, že nemáte žádné zásady domény, které vám zabrání připojování tooa virtuálnímu počítači pomocí veřejné adresy.</span><span class="sxs-lookup"><span data-stu-id="6e511-245">If you want toodo this, ensure you don't have any domain policies that prevent you from connecting tooa virtual machine using a public address.</span></span>

<span data-ttu-id="6e511-246">dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="6e511-246">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="6e511-247">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e511-247">Run a test failover</span></span>
- <span data-ttu-id="6e511-248">Spusťte hello testovací převzetí služeb při selhání spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e511-248">Start hello test failover by running hello following command:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a><span data-ttu-id="6e511-249">Spusťte plánované převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e511-249">Run a planned failover</span></span>
- <span data-ttu-id="6e511-250">Spuštění hello plánované převzetí služeb při selhání tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-250">Start hello planned failover by running hello following command:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="6e511-251">Spustit neplánované převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="6e511-251">Run an unplanned failover</span></span>
- <span data-ttu-id="6e511-252">Spustit hello neplánované převzetí služeb při selhání tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6e511-252">Start hello unplanned failover by running hello following command:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

## <span data-ttu-id="6e511-253"><a name=monitor></a>Sledování aktivity</span><span class="sxs-lookup"><span data-stu-id="6e511-253"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="6e511-254">Použijte následující příkazy toomonitor hello aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-254">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="6e511-255">Všimněte si, že máte toowait mezi úlohy pro zpracování toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="6e511-255">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="6e511-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e511-256">Next steps</span></span>
<span data-ttu-id="6e511-257">[Další informace](/powershell/module/azurerm.recoveryservices.backup/#recovery) o Azure Site Recovery pomocí rutin prostředí PowerShell Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6e511-257">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
