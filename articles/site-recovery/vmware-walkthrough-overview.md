---
title: "virtuální počítače VMware tooAzure aaaReplicate s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled hello kroky pro replikaci úloh běžících na virtuálních počítačích VMware tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7104f67a3635b916048dcb61bca770c89f0c77fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-vms-tooazure-with-site-recovery"></a><span data-ttu-id="22f3a-103">Replikovat virtuální počítače VMware tooAzure službou Site Recovery</span><span class="sxs-lookup"><span data-stu-id="22f3a-103">Replicate VMware VMs tooAzure with Site Recovery</span></span>

<span data-ttu-id="22f3a-104">Tento článek obsahuje přehled hello kroky požadované tooreplicate místní VMware virtuální počítače tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="22f3a-104">This article provides an overview of hello steps required tooreplicate on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


![Proces nasazení](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

<span data-ttu-id="22f3a-106">**Obrázek 1: Souhrn procesu nasazení**</span><span class="sxs-lookup"><span data-stu-id="22f3a-106">**Figure 1: Deployment process summary**</span></span>

## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="22f3a-107">Krok 1: Posouzení architektura a požadavky</span><span class="sxs-lookup"><span data-stu-id="22f3a-107">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="22f3a-108">Před zahájením nasazení, zkontrolujte architekturu scénáře hello a ujistěte se, že rozumíte všechny součásti hello potřebujete toodeploy</span><span class="sxs-lookup"><span data-stu-id="22f3a-108">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need toodeploy</span></span>

<span data-ttu-id="22f3a-109">Přejděte příliš[krok 1: Zkontrolujte architektura hello](vmware-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-109">Go too[Step 1: Review hello architecture](vmware-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="22f3a-110">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="22f3a-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="22f3a-111">Ujistěte se, že máte nastavené pro jednotlivé součásti nasazení hello požadavky:</span><span class="sxs-lookup"><span data-stu-id="22f3a-111">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="22f3a-112">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="22f3a-112">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="22f3a-113">**Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.</span><span class="sxs-lookup"><span data-stu-id="22f3a-113">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="22f3a-114">**Místní požadavky VMware**: budete potřebovat tooset účtů, aby Site Recovery přístup serverů VMware a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="22f3a-114">**On-premises VMware prerequisites**: You need tooset up accounts so that Site Recovery can access VMware servers and VMs.</span></span>
- <span data-ttu-id="22f3a-115">**Replikovat virtuální počítače**: virtuální počítače chcete tooreplicate potřeba toocomply s požadavky na Azure a mají hello nainstalovat službu mobility.</span><span class="sxs-lookup"><span data-stu-id="22f3a-115">**Replicated VMs**: VMs you want tooreplicate need toocomply with Azure requirements, and have hello Mobility service component installed.</span></span>

<span data-ttu-id="22f3a-116">Přejděte příliš[krok 2: Přečtěte si požadavky a omezení](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-116">Go too[Step 2: Review prerequisites and limitations](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="22f3a-117">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="22f3a-117">Step 3: Plan capacity</span></span>

<span data-ttu-id="22f3a-118">Pokud provádíte úplné nasazení musíte toofigure na jaké replikace prostředky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="22f3a-118">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="22f3a-119">Existuje několik z nástrojů k dispozici toohelp to uděláte.</span><span class="sxs-lookup"><span data-stu-id="22f3a-119">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="22f3a-120">Přejděte tooStep 2.</span><span class="sxs-lookup"><span data-stu-id="22f3a-120">Go tooStep 2.</span></span> <span data-ttu-id="22f3a-121">Pokud provádíte nějakou rychlou nastavení tootest hello prostředí můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="22f3a-121">If you're doing a quick set up tootest hello environment you can skip this step.</span></span>

<span data-ttu-id="22f3a-122">Přejděte příliš[krok 3: plánování kapacity](vmware-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-122">Go too[Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="22f3a-123">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="22f3a-123">Step 4: Plan networking</span></span>

<span data-ttu-id="22f3a-124">Je nutné toodo některé síťové plánování tooensure, že virtuální počítače Azure jsou připojené toonetworks po převzetí služeb při selhání, a že, ke kterým mají hello správné IP adres.</span><span class="sxs-lookup"><span data-stu-id="22f3a-124">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="22f3a-125">Přejděte příliš[krok 4: plánování sítě](vmware-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-125">Go too[Step 4: Plan networking](vmware-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="22f3a-126">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="22f3a-126">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="22f3a-127">Nastavte před zahájením Azure sítím a úložišti.</span><span class="sxs-lookup"><span data-stu-id="22f3a-127">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="22f3a-128">Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.</span><span class="sxs-lookup"><span data-stu-id="22f3a-128">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="22f3a-129">Přejděte příliš[krok 5: Příprava Azure](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-129">Go too[Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-vmware"></a><span data-ttu-id="22f3a-130">Krok 6: Příprava VMware</span><span class="sxs-lookup"><span data-stu-id="22f3a-130">Step 6: Prepare VMware</span></span>

<span data-ttu-id="22f3a-131">Je třeba tooset účtů, které budou používat Site Recovery na:</span><span class="sxs-lookup"><span data-stu-id="22f3a-131">You need tooset up accounts that Site Recovery will use to:</span></span>

- <span data-ttu-id="22f3a-132">Přístup k VMware virtualizace serverů tooautomatically zjistit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="22f3a-132">Access VMware virtualization servers tooautomatically detect VMs.</span></span>
- <span data-ttu-id="22f3a-133">Přístup k služba Mobility hello tooinstall virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="22f3a-133">Access VMs tooinstall hello Mobility service.</span></span> <span data-ttu-id="22f3a-134">Každý virtuální počítač, budete ho chtít tooreplicate musí mít agenta služby Mobility hello nainstalovat předtím, než můžete povolit pro něho replikaci.</span><span class="sxs-lookup"><span data-stu-id="22f3a-134">Each VM you want tooreplicate must have hello Mobility service agent installed before you can enable replication for it.</span></span>

<span data-ttu-id="22f3a-135">Přejděte příliš[krok 6: Příprava VMware](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-135">Go too[Step 6: Prepare VMware](vmware-walkthrough-prepare-vmware.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="22f3a-136">Krok 7: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="22f3a-136">Step 7: Set up a vault</span></span>

<span data-ttu-id="22f3a-137">Potřebujete tooset nahoru tooorchestrate trezoru služeb zotavení a správě replikace.</span><span class="sxs-lookup"><span data-stu-id="22f3a-137">You need tooset up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="22f3a-138">Při nastavování hello trezoru, určíte, co chcete tooreplicate, a místo, kam chcete tooreplicate jeho.</span><span class="sxs-lookup"><span data-stu-id="22f3a-138">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="22f3a-139">Přejděte příliš[krok 7: nastavení trezoru](vmware-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-139">Go too[Step 7: Set up a vault](vmware-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="22f3a-140">Krok 8: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="22f3a-140">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="22f3a-141">Nastavte hello zdroj a cíl, který se používá pro replikaci.</span><span class="sxs-lookup"><span data-stu-id="22f3a-141">Set up hello source and target that's used for replication.</span></span> <span data-ttu-id="22f3a-142">Nastavení nastavení zdroje zahrnuje spuštění tooinstall Unified instalace součásti Site Recovery místní hello.</span><span class="sxs-lookup"><span data-stu-id="22f3a-142">Setting up source settings includes running Unified Setup tooinstall hello on-premises Site Recovery components.</span></span>

<span data-ttu-id="22f3a-143">Přejděte příliš[krok 8: nastavení hello zdroje a cíle](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-143">Go too[Step 8: Set up hello source and target](vmware-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="22f3a-144">Krok 9: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="22f3a-144">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="22f3a-145">Můžete nastavit nastavení replikace toospecify zásady pro virtuální počítače VMware v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="22f3a-145">You set up a policy toospecify replication settings for VMware VMs in hello vault.</span></span>

<span data-ttu-id="22f3a-146">Přejděte příliš[krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-146">Go too[Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>

## <a name="step-10-install-hello-mobility-service"></a><span data-ttu-id="22f3a-147">Krok 10: Instalace služby Mobility hello</span><span class="sxs-lookup"><span data-stu-id="22f3a-147">Step 10: Install hello Mobility service</span></span>

<span data-ttu-id="22f3a-148">musí být nainstalován Hello služba Mobility na jednotlivé virtuální počítače chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="22f3a-148">hello Mobility service must be installed on each VM you want tooreplicate.</span></span> <span data-ttu-id="22f3a-149">Existuje několik způsobů tooset až služba hello se instalace push nebo pull.</span><span class="sxs-lookup"><span data-stu-id="22f3a-149">There are a few ways tooset up hello service with push or pull installation.</span></span>

<span data-ttu-id="22f3a-150">Přejděte příliš[krok 10: instalace služby Mobility hello](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-150">Go too[Step 10: Install hello Mobility service](vmware-walkthrough-install-mobility.md)</span></span>

## <a name="step-11-enable-replication"></a><span data-ttu-id="22f3a-151">Krok 11: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="22f3a-151">Step 11: Enable replication</span></span>

<span data-ttu-id="22f3a-152">Po hello služba Mobility je spuštěna na virtuálním počítači můžete povolit pro něho replikaci.</span><span class="sxs-lookup"><span data-stu-id="22f3a-152">After hello Mobility service is running on a VM you can enable replication for it.</span></span> <span data-ttu-id="22f3a-153">Když povolíte, dojde k počáteční replikaci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="22f3a-153">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="22f3a-154">Přejděte příliš[krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-154">Go too[Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>

## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="22f3a-155">Krok 12: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="22f3a-155">Step 12: Run a test failover</span></span>

<span data-ttu-id="22f3a-156">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="22f3a-156">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="22f3a-157">Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="22f3a-157">Go too[Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
