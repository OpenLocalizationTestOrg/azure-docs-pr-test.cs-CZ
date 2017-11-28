---
title: "aaaReplicate virtuálních počítačů Hyper-V tooa sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled pro replikaci virtuálních počítačů Hyper-V tooa sekundární lokalita VMM pomocí hello portálu Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 90e44bfc2237dfa7646fb2b7b1e533a7f6d83c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a><span data-ttu-id="c65c5-103">Replikace virtuálních počítačů technologie Hyper-V v nástroji VMM cloudy tooa sekundární lokalita VMM</span><span class="sxs-lookup"><span data-stu-id="c65c5-103">Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c65c5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c65c5-104">Azure portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="c65c5-105">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="c65c5-105">Classic portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="c65c5-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c65c5-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="c65c5-107">Tento článek obsahuje přehled hello kroky požadované tooreplicate místně spravované v cloudech System Center Virtual Machine Manager (VMM), tooa sekundárního umístění VMM, Hyper-V virtuální počítače (VM) pomocí [Azure Site Recovery](site-recovery-overview.md)v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c65c5-107">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds, tooa secondary VMM location, using [Azure Site Recovery](site-recovery-overview.md) in hello Azure portal.</span></span>

<span data-ttu-id="c65c5-108">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c65c5-108">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-hello-scenario-architecture"></a><span data-ttu-id="c65c5-109">Krok 1: Posouzení architekturu scénáře hello</span><span class="sxs-lookup"><span data-stu-id="c65c5-109">Step 1: Review hello scenario architecture</span></span>

<span data-ttu-id="c65c5-110">Před spuštěním nasazení, zkontrolujte architekturu scénáře hello, ujistěte se, že rozumíte všechny součásti hello musíte toodeploy.</span><span class="sxs-lookup"><span data-stu-id="c65c5-110">Before you start deployment, review hello scenario architecture, and make sure that you understand all hello components you need toodeploy.</span></span>

<span data-ttu-id="c65c5-111">Přejděte příliš[krok 1: Zkontrolujte hello architektura](vmm-to-vmm-walkthrough-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-111">Go too[Step 1: Review hello architecture](vmm-to-vmm-walkthrough-architecture.md).</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="c65c5-112">Krok 2: Kontrola předpoklady a omezení</span><span class="sxs-lookup"><span data-stu-id="c65c5-112">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="c65c5-113">Ujistěte se, že rozumíte hello nasazení předpoklady a omezení.</span><span class="sxs-lookup"><span data-stu-id="c65c5-113">Make sure that you understand hello deployment prerequisites and limitations.</span></span>

<span data-ttu-id="c65c5-114">**Požadavky Azure**: musíte mít předplatné Microsoft Azure a služeb zotavení Azure trezoru, tooorchestrate a spravovat replikace.</span><span class="sxs-lookup"><span data-stu-id="c65c5-114">**Azure prerequisites**: You need a Microsoft Azure subscription, and Azure Recovery Services vault, tooorchestrate and manage replication.</span></span>
<span data-ttu-id="c65c5-115">**Na místní servery VMM a hostitelé Hyper-V**: Ujistěte se, že jsou servery VMM a hostitelé Hyper-V kompatibilní a připravený pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c65c5-115">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>

<span data-ttu-id="c65c5-116">Přejděte příliš[krok 2: ověření předpoklady a omezení](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-116">Go too[Step 2: Verify prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>

## <a name="step-3-plan-networking"></a><span data-ttu-id="c65c5-117">Krok 3: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="c65c5-117">Step 3: Plan networking</span></span>

<span data-ttu-id="c65c5-118">Je nutné toodo některé síťové plánování tooensure lze nastavit mapování sítě mezi sítěmi virtuálních počítačů nástroje VMM při nasazování hello scénář.</span><span class="sxs-lookup"><span data-stu-id="c65c5-118">You need toodo some network planning tooensure that you can configure network mapping between VMM VM networks when you deploy hello scenario.</span></span>

<span data-ttu-id="c65c5-119">Přejděte příliš[krok 3: plánování sítě](vmm-to-vmm-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-119">Go too[Step 3: Plan networking](vmm-to-vmm-walkthrough-network.md).</span></span>


## <a name="step-4-prepare-vmm-and-hyper-v"></a><span data-ttu-id="c65c5-120">Krok 4: Příprava VMM a technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="c65c5-120">Step 4: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="c65c5-121">Příprava pro nasazení Site Recovery hello servery VMM a hostitelů Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c65c5-121">Prepare hello VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="c65c5-122">Přejděte příliš[krok 4: Příprava na místní servery](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-122">Go too[Step 4: Prepare on-premises servers](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span></span>

## <a name="step-5-set-up-a-vault"></a><span data-ttu-id="c65c5-123">Krok 5: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="c65c5-123">Step 5: Set up a vault</span></span>

<span data-ttu-id="c65c5-124">Nastavení trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="c65c5-124">Set up a Recovery Services vault.</span></span> <span data-ttu-id="c65c5-125">Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.</span><span class="sxs-lookup"><span data-stu-id="c65c5-125">hello vault contains configuration settings, and orchestrates replication.</span></span>

<span data-ttu-id="c65c5-126">[Krok 5: Nastavení trezoru](vmm-to-vmm-walkthrough-create-vault.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-126">[Step 5: Set up a vault](vmm-to-vmm-walkthrough-create-vault.md).</span></span>

## <a name="step-6-set-up-source-and-target-settings"></a><span data-ttu-id="c65c5-127">Krok 6: Nastavení zdrojové a cílové nastavení</span><span class="sxs-lookup"><span data-stu-id="c65c5-127">Step 6: Set up source and target settings</span></span>

<span data-ttu-id="c65c5-128">Nastavte hello zdrojové a cílové replikace VMM umístění.</span><span class="sxs-lookup"><span data-stu-id="c65c5-128">Set up hello source and target replication VMM locations.</span></span> <span data-ttu-id="c65c5-129">Přidejte hello VMM servery toohello trezoru a stáhnout hello instalační soubory pro součásti Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="c65c5-129">Add hello VMM servers toohello vault, and download hello installation files for Site Recovery components.</span></span> <span data-ttu-id="c65c5-130">Spusťte instalační program zprostředkovatele Azure Site Recovery na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="c65c5-130">Run Azure Site Recovery Provider setup on hello VMM server.</span></span> <span data-ttu-id="c65c5-131">Instalační program nainstaluje hello zprostředkovatele na serveru VMM hello a zaregistruje hello server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="c65c5-131">Setup installs hello Provider on hello VMM server, and registers hello server in hello vault.</span></span> <span data-ttu-id="c65c5-132">Nainstalujte agenta služeb zotavení Microsoft hello na každém hostiteli technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c65c5-132">You install hello Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="c65c5-133">Přejděte příliš[krok 6: nastavení hello zdrojové a cílové nastavení](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-133">Go too[Step 6: Set up hello source and target settings](vmm-to-vmm-walkthrough-source-target.md).</span></span>

## <a name="step-7-configure-network-mapping"></a><span data-ttu-id="c65c5-134">Krok 7: Nakonfigurování mapování sítě</span><span class="sxs-lookup"><span data-stu-id="c65c5-134">Step 7: Configure network mapping</span></span>

<span data-ttu-id="c65c5-135">Mapování sítě virtuálních počítačů nástroje VMM v hello zdrojové a cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="c65c5-135">Map VMM VM networks in hello source and target locations.</span></span> <span data-ttu-id="c65c5-136">Po převzetí služeb při selhání jsou virtuální počítače vytvořené v cílové síti hello této mapy toohello zdrojové síti virtuálních počítačů ve které hello se nachází zdrojový virtuální počítač Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="c65c5-136">After failover, VMs are created in hello target network that maps toohello source VM network in which hello source Hyper-V VM is located.</span></span>

<span data-ttu-id="c65c5-137">Přejděte příliš[krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-137">Go too[Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>


## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="c65c5-138">Krok 8: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="c65c5-138">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="c65c5-139">Zadejte, jak bude replikován virtuální počítače mezi umístěními VMM.</span><span class="sxs-lookup"><span data-stu-id="c65c5-139">Specify how  VMs will be replicated between VMM locations.</span></span>

<span data-ttu-id="c65c5-140">Přejděte příliš[krok 8: nastavte zásady replikace](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-140">Go too[Step 8: Set up a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>


## <a name="step-9-enable-replication-for-vms"></a><span data-ttu-id="c65c5-141">Krok 9: Povolení replikace pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c65c5-141">Step 9: Enable replication for VMs</span></span>

<span data-ttu-id="c65c5-142">Vyberte virtuální počítače hello chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="c65c5-142">Select hello VMs you want tooreplicate.</span></span> <span data-ttu-id="c65c5-143">Povolení virtuálního počítače pro replikaci aktivační události hello počáteční replikace toohello sekundární lokality, pak probíhající rozdílová replikace.</span><span class="sxs-lookup"><span data-stu-id="c65c5-143">Enabling a VM for replication triggers hello initial replication toohello secondary site, followed by ongoing delta replication.</span></span>

<span data-ttu-id="c65c5-144">Přejděte příliš[krok 9: povolení replikace](vmm-to-vmm-walkthrough-enable-replication.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-144">Go too[Step 9: Enable replication](vmm-to-vmm-walkthrough-enable-replication.md).</span></span>


## <a name="step-10-run-a-test-failover"></a><span data-ttu-id="c65c5-145">Krok 10: Spustit testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="c65c5-145">Step 10: Run a test failover</span></span>

<span data-ttu-id="c65c5-146">Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="c65c5-146">Run a test failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="c65c5-147">Přejděte příliš[krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="c65c5-147">Go too[Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
