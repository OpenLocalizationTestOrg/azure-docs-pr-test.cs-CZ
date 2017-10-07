---
title: "aaaReplicate tooAzure virtuálních počítačů Hyper-V s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak tooorchestrate replikace, převzetí služeb při selhání a obnovení místní technologie Hyper-V virtuální počítače tooAzure"
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
ms.openlocfilehash: ab9cd14149ef32a416428d0f4327aa18b042e9c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure"></a><span data-ttu-id="90bc2-103">Replikovat virtuální počítače (bez VMM) tooAzure technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="90bc2-103">Replicate Hyper-V virtual machines (without VMM) tooAzure</span></span> 

> [!div class="op_single_selector"]
> * [<span data-ttu-id="90bc2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="90bc2-104">Azure portal</span></span>](site-recovery-hyper-v-site-to-azure.md)
> * [<span data-ttu-id="90bc2-105">Azure Classic</span><span class="sxs-lookup"><span data-stu-id="90bc2-105">Azure classic</span></span>](site-recovery-hyper-v-site-to-azure-classic.md)
> * [<span data-ttu-id="90bc2-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="90bc2-106">PowerShell - Resource Manager</span></span>](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

<span data-ttu-id="90bc2-107">Tento článek obsahuje přehled hello kroky tooreplicate místní technologie Hyper-V virtuální počítače tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="90bc2-107">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) in hello Azure portal.</span></span> <span data-ttu-id="90bc2-108">Ve virtuálních počítačích této nasazení technologie Hyper-V nejsou spravovány nástrojem System Center Virtual Machine Manager (VMM).</span><span class="sxs-lookup"><span data-stu-id="90bc2-108">In this deployment Hyper-V VMs aren't managed by System Center Virtual Machine Manager (VMM).</span></span>


<span data-ttu-id="90bc2-109">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="90bc2-109">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="90bc2-110">Krok 1: Posouzení architektura a požadavky</span><span class="sxs-lookup"><span data-stu-id="90bc2-110">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="90bc2-111">Před zahájením nasazení, zkontrolujte architekturu scénáře hello a ujistěte se, že rozumíte všechny součásti hello potřebujete toodeploy</span><span class="sxs-lookup"><span data-stu-id="90bc2-111">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need toodeploy</span></span>

<span data-ttu-id="90bc2-112">Přejděte příliš[krok 1: Zkontrolujte architektura hello](hyper-v-site-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-112">Go too[Step 1: Review hello architecture](hyper-v-site-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="90bc2-113">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="90bc2-113">Step 2: Review prerequisites</span></span>

<span data-ttu-id="90bc2-114">Ujistěte se, že máte nastavené pro jednotlivé součásti nasazení hello požadavky:</span><span class="sxs-lookup"><span data-stu-id="90bc2-114">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="90bc2-115">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="90bc2-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="90bc2-116">**Požadavky technologie Hyper-V na místě**: Zajistěte, aby hostitelů Hyper-V jsou připraveny pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="90bc2-116">**On-premises Hyper-V prerequisites**: Make sure Hyper-V hosts are prepared for Site Recovery deployment.</span></span>
- <span data-ttu-id="90bc2-117">**Replikovat virtuální počítače**: virtuální počítače chcete tooreplicate potřebovat toocomply s požadavky na Azure.</span><span class="sxs-lookup"><span data-stu-id="90bc2-117">**Replicated VMs**: VMs you want tooreplicate need toocomply with Azure requirements.</span></span>

<span data-ttu-id="90bc2-118">Přejděte příliš[krok 2: ověření předpoklady a omezení](hyper-v-site-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-118">Go too[Step 2: Verify prerequisites and limitations](hyper-v-site-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="90bc2-119">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="90bc2-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="90bc2-120">Pokud provádíte úplné nasazení musíte toofigure na jaké replikace prostředky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="90bc2-120">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="90bc2-121">Existuje několik z nástrojů k dispozici toohelp to uděláte.</span><span class="sxs-lookup"><span data-stu-id="90bc2-121">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="90bc2-122">Přejděte tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="90bc2-122">Go tooStep 2.</span></span> <span data-ttu-id="90bc2-123">Pokud provádíte nějakou rychlou nastavení tootest hello prostředí můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="90bc2-123">If you're doing a quick set up tootest hello environment you can skip this step.</span></span>

<span data-ttu-id="90bc2-124">Přejděte příliš[krok 3: plánování kapacity](hyper-v-site-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-124">Go too[Step 3: Plan capacity](hyper-v-site-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="90bc2-125">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="90bc2-125">Step 4: Plan networking</span></span>

<span data-ttu-id="90bc2-126">Je nutné toodo některé síťové plánování tooensure, že virtuální počítače Azure jsou připojené toonetworks po převzetí služeb při selhání, a že, ke kterým mají hello správné IP adres.</span><span class="sxs-lookup"><span data-stu-id="90bc2-126">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="90bc2-127">Přejděte příliš[krok 4: plánování sítě](hyper-v-site-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-127">Go too[Step 4: Plan networking](hyper-v-site-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="90bc2-128">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="90bc2-128">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="90bc2-129">Nastavte před zahájením Azure sítím a úložišti.</span><span class="sxs-lookup"><span data-stu-id="90bc2-129">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="90bc2-130">Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.</span><span class="sxs-lookup"><span data-stu-id="90bc2-130">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="90bc2-131">Přejděte příliš[krok 5: Příprava Azure](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-131">Go too[Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-hyper-v"></a><span data-ttu-id="90bc2-132">Krok 6: Příprava technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="90bc2-132">Step 6: Prepare Hyper-V</span></span>

<span data-ttu-id="90bc2-133">Ujistěte se, že technologie Hyper-V servery splňují požadavky na nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="90bc2-133">Make sure that Hyper-V servers meet Site Recovery deployment requirements.</span></span>

<span data-ttu-id="90bc2-134">Přejděte příliš[krok 6: Příprava technologie Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-134">Go too[Step 6: Prepare Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="90bc2-135">Krok 7: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="90bc2-135">Step 7: Set up a vault</span></span>

<span data-ttu-id="90bc2-136">Potřebujete tooset nahoru tooorchestrate trezoru služeb zotavení a správě replikace.</span><span class="sxs-lookup"><span data-stu-id="90bc2-136">You need tooset up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="90bc2-137">Při nastavování hello trezoru, určíte, co chcete tooreplicate, a místo, kam chcete tooreplicate jeho.</span><span class="sxs-lookup"><span data-stu-id="90bc2-137">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="90bc2-138">Přejděte příliš[krok 7: vytvoření trezoru](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-138">Go too[Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="90bc2-139">Krok 8: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="90bc2-139">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="90bc2-140">Nastavte hello zdroj a cíl, který se používá pro replikaci.</span><span class="sxs-lookup"><span data-stu-id="90bc2-140">Set up hello source and target that's used for replication.</span></span> <span data-ttu-id="90bc2-141">Nastavení nastavení zdroje, zahrnuje přidání hostitelů technologie Hyper-V, tooa web Hyper-V, instalaci hello zprostředkovatele služby Site Recovery a agenta služeb zotavení na každém hostiteli technologie Hyper-V a registraci hello lokality v trezoru služeb zotavení hello.</span><span class="sxs-lookup"><span data-stu-id="90bc2-141">Setting up source settings includes adding Hyper-V hosts tooa Hyper-V site, installing hello Site Recovery Provider and Recovery Services agent on each Hyper-V host, and registering hello site in hello Recovery Services vault.</span></span>

<span data-ttu-id="90bc2-142">Přejděte příliš[krok 8: nastavení hello zdroje a cíle](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-142">Go too[Step 8: Set up hello source and target](hyper-v-site-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="90bc2-143">Krok 9: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="90bc2-143">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="90bc2-144">Můžete nastavit nastavení replikace toospecify zásady pro virtuální počítače Hyper-v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="90bc2-144">You set up a policy toospecify replication settings for Hyper-V VMs in hello vault.</span></span>

<span data-ttu-id="90bc2-145">Přejděte příliš[krok 9: nastavení zásad replikace](hyper-v-site-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-145">Go too[Step 9: Set up a replication policy](hyper-v-site-walkthrough-replication.md)</span></span>


## <a name="step-10-enable-replication"></a><span data-ttu-id="90bc2-146">Krok 10: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="90bc2-146">Step 10: Enable replication</span></span>

<span data-ttu-id="90bc2-147">Až budete mít zásadu replikace na místě, po povolení, dojde k počáteční replikaci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="90bc2-147">After you have a replication policy in place,  After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="90bc2-148">Přejděte příliš[krok 10: povolení replikace](hyper-v-site-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-148">Go too[Step 10: Enable replication](hyper-v-site-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="90bc2-149">Krok 11: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="90bc2-149">Step 11: Run a test failover</span></span>

<span data-ttu-id="90bc2-150">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="90bc2-150">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="90bc2-151">Přejděte příliš[krok 11: spustit testovací převzetí služeb](hyper-v-site-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="90bc2-151">Go too[Step 11: Run a test failover](hyper-v-site-walkthrough-test-failover.md)</span></span>
