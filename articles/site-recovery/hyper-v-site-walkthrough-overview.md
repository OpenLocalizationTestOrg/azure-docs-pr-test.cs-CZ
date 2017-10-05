---
title: "Replikace virtuálních počítačů technologie Hyper-V do Azure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak k orchestraci replikace, převzetí služeb při selhání a obnovení místní virtuální počítače Hyper-V do Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: da10b213bc2543942b5ac77cf5c5d8547c00220c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure"></a><span data-ttu-id="1ff9f-103">Replikace virtuálních počítačů technologie Hyper-V (bez VMM) do Azure</span><span class="sxs-lookup"><span data-stu-id="1ff9f-103">Replicate Hyper-V virtual machines (without VMM) to Azure</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1ff9f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1ff9f-104">Azure portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="1ff9f-105">Azure Classic</span><span class="sxs-lookup"><span data-stu-id="1ff9f-105">Azure classic</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
> * [<span data-ttu-id="1ff9f-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="1ff9f-106">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

<span data-ttu-id="1ff9f-107">Tento článek obsahuje přehled kroků nezbytných k replikaci virtuálních počítačů Hyper-V místní do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-107">This article provides an overview of the steps required to replicate on-premises Hyper-V virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) in the Azure portal.</span></span> <span data-ttu-id="1ff9f-108">Ve virtuálních počítačích této nasazení technologie Hyper-V nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM).</span><span class="sxs-lookup"><span data-stu-id="1ff9f-108">In this deployment Hyper-V VMs aren't managed by System Center Virtual Machine Manager (VMM).</span></span>


<span data-ttu-id="1ff9f-109">Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1ff9f-109">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="1ff9f-110">Krok 1: Posouzení architektura a požadavky</span><span class="sxs-lookup"><span data-stu-id="1ff9f-110">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="1ff9f-111">Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které potřebujete k nasazení</span><span class="sxs-lookup"><span data-stu-id="1ff9f-111">Before you start deployment, review the scenario architecture, and make sure you understand all the components you need to deploy</span></span>

<span data-ttu-id="1ff9f-112">Přejděte na [krok 1: Přečtěte si architektura](hyper-v-site-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-112">Go to [Step 1: Review the architecture](hyper-v-site-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="1ff9f-113">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="1ff9f-113">Step 2: Review prerequisites</span></span>

<span data-ttu-id="1ff9f-114">Ujistěte se, že máte požadavky na místě pro každou součást nasazení:</span><span class="sxs-lookup"><span data-stu-id="1ff9f-114">Make sure you have the prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="1ff9f-115">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="1ff9f-116">**Požadavky technologie Hyper-V na místě**: Zajistěte, aby hostitelů Hyper-V jsou připraveny pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-116">**On-premises Hyper-V prerequisites**: Make sure Hyper-V hosts are prepared for Site Recovery deployment.</span></span>
- <span data-ttu-id="1ff9f-117">**Replikovat virtuální počítače**: virtuální počítače, které chcete replikovat musí splňovat požadavky pro Azure.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-117">**Replicated VMs**: VMs you want to replicate need to comply with Azure requirements.</span></span>

<span data-ttu-id="1ff9f-118">Přejděte na [krok 2: ověření předpoklady a omezení](hyper-v-site-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-118">Go to [Step 2: Verify prerequisites and limitations](hyper-v-site-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="1ff9f-119">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="1ff9f-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="1ff9f-120">Pokud provádíte úplné nasazení potřebujete zjistit, jaké prostředky replikace, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-120">If you're doing a full deployment you need to figure out what replication resources you need.</span></span> <span data-ttu-id="1ff9f-121">Existuje několik nástrojů, které vám pomůžou vám to.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-121">There are a couple of tools available to help you do this.</span></span> <span data-ttu-id="1ff9f-122">Přejděte ke kroku 2.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-122">Go to Step 2.</span></span> <span data-ttu-id="1ff9f-123">Pokud provádíte stručně si pro testovací prostředí můžete přeskočit tento krok.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-123">If you're doing a quick set up to test the environment you can skip this step.</span></span>

<span data-ttu-id="1ff9f-124">Přejděte na [Krok 3: Plánování kapacity](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-124">Go to [Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="1ff9f-125">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="1ff9f-125">Step 4: Plan networking</span></span>

<span data-ttu-id="1ff9f-126">Budete muset provést některé síťové plánování zajistit, že virtuální počítače Azure, jsou připojeny k sítím po převzetí služeb při selhání a která, ke kterým mají správné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-126">You need to do some network planning to ensure that Azure VMs are connected to networks after failover occurs, and  that that they have the right IP addresses.</span></span>

<span data-ttu-id="1ff9f-127">Přejděte na [krok 4: plánování sítě](hyper-v-site-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-127">Go to [Step 4: Plan networking](hyper-v-site-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="1ff9f-128">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="1ff9f-128">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="1ff9f-129">Nastavte před zahájením Azure sítím a úložišti.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-129">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="1ff9f-130">Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-130">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="1ff9f-131">Přejděte na [krok 5: Příprava Azure](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-131">Go to [Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-hyper-v"></a><span data-ttu-id="1ff9f-132">Krok 6: Příprava technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="1ff9f-132">Step 6: Prepare Hyper-V</span></span>

<span data-ttu-id="1ff9f-133">Ujistěte se, že technologie Hyper-V servery splňují požadavky na nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-133">Make sure that Hyper-V servers meet Site Recovery deployment requirements.</span></span>

<span data-ttu-id="1ff9f-134">Přejděte na [krok 6: Příprava technologie Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-134">Go to [Step 6: Prepare Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="1ff9f-135">Krok 7: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="1ff9f-135">Step 7: Set up a vault</span></span>

<span data-ttu-id="1ff9f-136">Budete muset nastavit orchestraci a Správa replikace v trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-136">You need to set up a Recovery Services vault to orchestrate and manage replication.</span></span> <span data-ttu-id="1ff9f-137">Když nastavíte trezor, zadejte co chcete replikovat a kam chcete replikovat, aby.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-137">When you set up the vault, you specify what you want to replicate, and where you want to replicate it to.</span></span>

<span data-ttu-id="1ff9f-138">Přejděte na [krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-138">Go to [Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="1ff9f-139">Krok 8: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="1ff9f-139">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="1ff9f-140">Nastavte zdroje a cíle, který se používá pro replikaci.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-140">Set up the source and target that's used for replication.</span></span> <span data-ttu-id="1ff9f-141">Nastavení nastavení zdroje, zahrnuje přidání hostitelů Hyper-V na Hyper-V lokalitě, instalace agenta zprostředkovatele služby Site Recovery a služeb zotavení na každém hostiteli technologie Hyper-V a registrace lokality v trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-141">Setting up source settings includes adding Hyper-V hosts to a Hyper-V site, installing the Site Recovery Provider and Recovery Services agent on each Hyper-V host, and registering the site in the Recovery Services vault.</span></span>

<span data-ttu-id="1ff9f-142">Přejděte na [krok 8: nastavit zdroje a cíle](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-142">Go to [Step 8: Set up the source and target](hyper-v-site-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="1ff9f-143">Krok 9: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="1ff9f-143">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="1ff9f-144">Nastavení zásad a zadejte nastavení replikace pro virtuální počítače Hyper-v trezoru.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-144">You set up a policy to specify replication settings for Hyper-V VMs in the vault.</span></span>

<span data-ttu-id="1ff9f-145">Přejděte na [krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-145">Go to [Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>


## <a name="step-10-enable-replication"></a><span data-ttu-id="1ff9f-146">Krok 10: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="1ff9f-146">Step 10: Enable replication</span></span>

<span data-ttu-id="1ff9f-147">Až budete mít zásadu replikace na místě, po povolení, dojde k počáteční replikaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-147">After you have a replication policy in place,  After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="1ff9f-148">Přejděte na [krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-148">Go to [Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="1ff9f-149">Krok 11: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="1ff9f-149">Step 11: Run a test failover</span></span>

<span data-ttu-id="1ff9f-150">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="1ff9f-150">After initial replication finishes, and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="1ff9f-151">Přejděte na [krok 11: spustit testovací převzetí služeb](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="1ff9f-151">Go to [Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
