---
title: "Replikace virtuálních počítačů technologie Hyper-V do sekundární lokalita VMM s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled pro replikaci virtuálních počítačů Hyper-V do sekundární lokalita VMM pomocí portálu Azure."
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
ms.openlocfilehash: b422dd2cf23426de2f154a553b38509082536309
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a><span data-ttu-id="9096a-103">Replikace virtuálních počítačů technologie Hyper-V v cloudech VMM do sekundární lokalita VMM</span><span class="sxs-lookup"><span data-stu-id="9096a-103">Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9096a-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9096a-104">Azure portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="9096a-105">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="9096a-105">Classic portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="9096a-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="9096a-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="9096a-107">Tento článek obsahuje přehled kroků nezbytných replikovat místní technologie Hyper-V virtuální počítače (VM) spravovat v nástroji System Center Virtual Machine Manager (VMM) cloudy do sekundárního umístění VMM pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9096a-107">This article provides an overview of the steps required to replicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds, to a secondary VMM location, using [Azure Site Recovery](site-recovery-overview.md) in the Azure portal.</span></span>

<span data-ttu-id="9096a-108">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="9096a-108">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-the-scenario-architecture"></a><span data-ttu-id="9096a-109">Krok 1: Posouzení na architekturu scénáře</span><span class="sxs-lookup"><span data-stu-id="9096a-109">Step 1: Review the scenario architecture</span></span>

<span data-ttu-id="9096a-110">Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které potřebujete k nasazení.</span><span class="sxs-lookup"><span data-stu-id="9096a-110">Before you start deployment, review the scenario architecture, and make sure that you understand all the components you need to deploy.</span></span>

<span data-ttu-id="9096a-111">Přejděte na [krok 1: Přečtěte si architekturu](vmm-to-vmm-walkthrough-architecture.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-111">Go to [Step 1: Review the architecture](vmm-to-vmm-walkthrough-architecture.md).</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="9096a-112">Krok 2: Kontrola předpoklady a omezení</span><span class="sxs-lookup"><span data-stu-id="9096a-112">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="9096a-113">Ujistěte se, že rozumíte nasazení předpoklady a omezení.</span><span class="sxs-lookup"><span data-stu-id="9096a-113">Make sure that you understand the deployment prerequisites and limitations.</span></span>

<span data-ttu-id="9096a-114">**Požadavky Azure**: budete potřebovat předplatné Microsoft Azure a trezoru služeb zotavení Azure, orchestraci a správě replikace.</span><span class="sxs-lookup"><span data-stu-id="9096a-114">**Azure prerequisites**: You need a Microsoft Azure subscription, and Azure Recovery Services vault, to orchestrate and manage replication.</span></span>
<span data-ttu-id="9096a-115">**Na místní servery VMM a hostitelé Hyper-V**: Ujistěte se, že jsou servery VMM a hostitelé Hyper-V kompatibilní a připravený pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9096a-115">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>

<span data-ttu-id="9096a-116">Přejděte na [krok 2: ověření předpoklady a omezení](vmm-to-vmm-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-116">Go to [Step 2: Verify prerequisites and limitations](vmm-to-vmm-walkthrough-prerequisites.md).</span></span>

## <a name="step-3-plan-networking"></a><span data-ttu-id="9096a-117">Krok 3: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="9096a-117">Step 3: Plan networking</span></span>

<span data-ttu-id="9096a-118">Budete muset provést některé síťové plánování nezapomeňte nakonfigurovat mapování sítě mezi sítěmi virtuálních počítačů nástroje VMM při nasazení tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="9096a-118">You need to do some network planning to ensure that you can configure network mapping between VMM VM networks when you deploy the scenario.</span></span>

<span data-ttu-id="9096a-119">Přejděte na [krok 3: plánování sítě](vmm-to-vmm-walkthrough-network.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-119">Go to [Step 3: Plan networking](vmm-to-vmm-walkthrough-network.md).</span></span>


## <a name="step-4-prepare-vmm-and-hyper-v"></a><span data-ttu-id="9096a-120">Krok 4: Příprava VMM a technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="9096a-120">Step 4: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="9096a-121">Připravte servery VMM a hostitelé Hyper-V pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9096a-121">Prepare the VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="9096a-122">Přejděte na [krok 4: Příprava na místní servery](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-122">Go to [Step 4: Prepare on-premises servers](vmm-to-vmm-walkthrough-vmm-hyper-v.md).</span></span>

## <a name="step-5-set-up-a-vault"></a><span data-ttu-id="9096a-123">Krok 5: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="9096a-123">Step 5: Set up a vault</span></span>

<span data-ttu-id="9096a-124">Nastavení trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="9096a-124">Set up a Recovery Services vault.</span></span> <span data-ttu-id="9096a-125">Trezor obsahuje konfigurační nastavení a orchestruje replikaci.</span><span class="sxs-lookup"><span data-stu-id="9096a-125">The vault contains configuration settings, and orchestrates replication.</span></span>

<span data-ttu-id="9096a-126">[Krok 5: Nastavení trezoru](vmm-to-vmm-walkthrough-create-vault.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-126">[Step 5: Set up a vault](vmm-to-vmm-walkthrough-create-vault.md).</span></span>

## <a name="step-6-set-up-source-and-target-settings"></a><span data-ttu-id="9096a-127">Krok 6: Nastavení zdrojové a cílové nastavení</span><span class="sxs-lookup"><span data-stu-id="9096a-127">Step 6: Set up source and target settings</span></span>

<span data-ttu-id="9096a-128">Nastavte zdrojové a cílové umístění VMM replikace.</span><span class="sxs-lookup"><span data-stu-id="9096a-128">Set up the source and target replication VMM locations.</span></span> <span data-ttu-id="9096a-129">Přidejte servery VMM do trezoru a stáhnout instalační soubory pro součásti Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="9096a-129">Add the VMM servers to the vault, and download the installation files for Site Recovery components.</span></span> <span data-ttu-id="9096a-130">Spusťte instalační program zprostředkovatele Azure Site Recovery na serveru VMM.</span><span class="sxs-lookup"><span data-stu-id="9096a-130">Run Azure Site Recovery Provider setup on the VMM server.</span></span> <span data-ttu-id="9096a-131">Instalační program nainstaluje zprostředkovatele na serveru VMM a zaregistruje server v trezoru.</span><span class="sxs-lookup"><span data-stu-id="9096a-131">Setup installs the Provider on the VMM server, and registers the server in the vault.</span></span> <span data-ttu-id="9096a-132">Nainstalujte agenta služeb zotavení Microsoft na každém hostiteli technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="9096a-132">You install the Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="9096a-133">Přejděte na [krok 6: nastavení nastavení zdrojové a cílové](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-133">Go to [Step 6: Set up the source and target settings](vmm-to-vmm-walkthrough-source-target.md).</span></span>

## <a name="step-7-configure-network-mapping"></a><span data-ttu-id="9096a-134">Krok 7: Nakonfigurování mapování sítě</span><span class="sxs-lookup"><span data-stu-id="9096a-134">Step 7: Configure network mapping</span></span>

<span data-ttu-id="9096a-135">Mapování sítě virtuálních počítačů nástroje VMM ve zdrojové a cílové umístění.</span><span class="sxs-lookup"><span data-stu-id="9096a-135">Map VMM VM networks in the source and target locations.</span></span> <span data-ttu-id="9096a-136">Po převzetí služeb při selhání jsou virtuální počítače vytvořené v cílové síti, která se mapuje na zdrojové síti virtuálních počítačů, ve kterém se nachází zdrojový virtuální počítač technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="9096a-136">After failover, VMs are created in the target network that maps to the source VM network in which the source Hyper-V VM is located.</span></span>

<span data-ttu-id="9096a-137">Přejděte na [krok 7: nakonfigurování mapování sítě](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-137">Go to [Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>


## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="9096a-138">Krok 8: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="9096a-138">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="9096a-139">Zadejte, jak bude replikován virtuální počítače mezi umístěními VMM.</span><span class="sxs-lookup"><span data-stu-id="9096a-139">Specify how  VMs will be replicated between VMM locations.</span></span>

<span data-ttu-id="9096a-140">Přejděte na [krok 8: nastavte zásady replikace](vmm-to-vmm-walkthrough-replication.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-140">Go to [Step 8: Set up a replication policy](vmm-to-vmm-walkthrough-replication.md).</span></span>


## <a name="step-9-enable-replication-for-vms"></a><span data-ttu-id="9096a-141">Krok 9: Povolení replikace pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="9096a-141">Step 9: Enable replication for VMs</span></span>

<span data-ttu-id="9096a-142">Vyberte virtuální počítače, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="9096a-142">Select the VMs you want to replicate.</span></span> <span data-ttu-id="9096a-143">Povolení virtuálního počítače pro replikace spustí počáteční replikace do sekundární lokality, za nímž následuje probíhající rozdílová replikace.</span><span class="sxs-lookup"><span data-stu-id="9096a-143">Enabling a VM for replication triggers the initial replication to the secondary site, followed by ongoing delta replication.</span></span>

<span data-ttu-id="9096a-144">Přejděte na [krok 9: povolení replikace](vmm-to-vmm-walkthrough-enable-replication.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-144">Go to [Step 9: Enable replication](vmm-to-vmm-walkthrough-enable-replication.md).</span></span>


## <a name="step-10-run-a-test-failover"></a><span data-ttu-id="9096a-145">Krok 10: Spustit testovací převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="9096a-145">Step 10: Run a test failover</span></span>

<span data-ttu-id="9096a-146">Otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje, jak má.</span><span class="sxs-lookup"><span data-stu-id="9096a-146">Run a test failover to make sure everything's working as expected.</span></span>

<span data-ttu-id="9096a-147">Přejděte na [krok 10: spustit testovací převzetí služeb](vmm-to-vmm-walkthrough-test-failover.md).</span><span class="sxs-lookup"><span data-stu-id="9096a-147">Go to [Step 10: Run a test failover](vmm-to-vmm-walkthrough-test-failover.md).</span></span>
