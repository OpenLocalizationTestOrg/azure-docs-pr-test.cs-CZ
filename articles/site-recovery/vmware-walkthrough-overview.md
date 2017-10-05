---
title: "Replikace virtuálních počítačů VMware do Azure s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled kroků pro replikaci úlohy běžící na virtuálních počítačích VMware do Azure"
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
ms.openlocfilehash: db6f5f95929503e82a529dba26b56af1edb0767f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-vms-to-azure-with-site-recovery"></a><span data-ttu-id="a2787-103">Replikace virtuálních počítačů VMware do Azure pomocí Site Recovery</span><span class="sxs-lookup"><span data-stu-id="a2787-103">Replicate VMware VMs to Azure with Site Recovery</span></span>

<span data-ttu-id="a2787-104">Tento článek obsahuje přehled kroků nezbytných k replikaci na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a2787-104">This article provides an overview of the steps required to replicate on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


![Proces nasazení](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

<span data-ttu-id="a2787-106">**Obrázek 1: Souhrn procesu nasazení**</span><span class="sxs-lookup"><span data-stu-id="a2787-106">**Figure 1: Deployment process summary**</span></span>

## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="a2787-107">Krok 1: Posouzení architektura a požadavky</span><span class="sxs-lookup"><span data-stu-id="a2787-107">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="a2787-108">Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které potřebujete k nasazení</span><span class="sxs-lookup"><span data-stu-id="a2787-108">Before you start deployment, review the scenario architecture, and make sure you understand all the components you need to deploy</span></span>

<span data-ttu-id="a2787-109">Přejděte na [krok 1: Přečtěte si architektura](vmware-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-109">Go to [Step 1: Review the architecture](vmware-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="a2787-110">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="a2787-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="a2787-111">Ujistěte se, že máte požadavky na místě pro každou součást nasazení:</span><span class="sxs-lookup"><span data-stu-id="a2787-111">Make sure you have the prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="a2787-112">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="a2787-112">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="a2787-113">**Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.</span><span class="sxs-lookup"><span data-stu-id="a2787-113">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="a2787-114">**Místní požadavky VMware**: je nutné nastavit účtů, aby Site Recovery přístup serverů VMware a virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a2787-114">**On-premises VMware prerequisites**: You need to set up accounts so that Site Recovery can access VMware servers and VMs.</span></span>
- <span data-ttu-id="a2787-115">**Replikovat virtuální počítače**: virtuální počítače, které chcete replikovat potřebují k dosažení souladu s požadavky na Azure a nainstalována součást služby Mobility.</span><span class="sxs-lookup"><span data-stu-id="a2787-115">**Replicated VMs**: VMs you want to replicate need to comply with Azure requirements, and have the Mobility service component installed.</span></span>

<span data-ttu-id="a2787-116">Přejděte na [krok 2: Přečtěte si požadavky a omezení](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-116">Go to [Step 2: Review prerequisites and limitations](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="a2787-117">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="a2787-117">Step 3: Plan capacity</span></span>

<span data-ttu-id="a2787-118">Pokud provádíte úplné nasazení potřebujete zjistit, jaké prostředky replikace, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="a2787-118">If you're doing a full deployment you need to figure out what replication resources you need.</span></span> <span data-ttu-id="a2787-119">Existuje několik nástrojů, které vám pomůžou vám to.</span><span class="sxs-lookup"><span data-stu-id="a2787-119">There are a couple of tools available to help you do this.</span></span> <span data-ttu-id="a2787-120">Přejděte ke kroku 2.</span><span class="sxs-lookup"><span data-stu-id="a2787-120">Go to Step 2.</span></span> <span data-ttu-id="a2787-121">Pokud provádíte stručně si pro testovací prostředí můžete přeskočit tento krok.</span><span class="sxs-lookup"><span data-stu-id="a2787-121">If you're doing a quick set up to test the environment you can skip this step.</span></span>

<span data-ttu-id="a2787-122">Přejděte na [Krok 3: Plánování kapacity](vmware-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-122">Go to [Step 3: Plan capacity](vmware-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="a2787-123">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="a2787-123">Step 4: Plan networking</span></span>

<span data-ttu-id="a2787-124">Budete muset provést některé síťové plánování zajistit, že virtuální počítače Azure, jsou připojeny k sítím po převzetí služeb při selhání a která, ke kterým mají správné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a2787-124">You need to do some network planning to ensure that Azure VMs are connected to networks after failover occurs, and  that that they have the right IP addresses.</span></span>

<span data-ttu-id="a2787-125">Přejděte na [krok 4: plánování sítě](vmware-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-125">Go to [Step 4: Plan networking](vmware-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="a2787-126">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="a2787-126">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="a2787-127">Nastavte před zahájením Azure sítím a úložišti.</span><span class="sxs-lookup"><span data-stu-id="a2787-127">Set up Azure networks and storage before you start.</span></span> <span data-ttu-id="a2787-128">Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.</span><span class="sxs-lookup"><span data-stu-id="a2787-128">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="a2787-129">Přejděte na [krok 5: Příprava Azure](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-129">Go to [Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-prepare-vmware"></a><span data-ttu-id="a2787-130">Krok 6: Příprava VMware</span><span class="sxs-lookup"><span data-stu-id="a2787-130">Step 6: Prepare VMware</span></span>

<span data-ttu-id="a2787-131">Je třeba nastavit účty, které budou používat Site Recovery na:</span><span class="sxs-lookup"><span data-stu-id="a2787-131">You need to set up accounts that Site Recovery will use to:</span></span>

- <span data-ttu-id="a2787-132">Přístup k VMware virtualizace serverů automaticky zjistit virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="a2787-132">Access VMware virtualization servers to automatically detect VMs.</span></span>
- <span data-ttu-id="a2787-133">Virtuální počítače přístup k instalaci služby Mobility.</span><span class="sxs-lookup"><span data-stu-id="a2787-133">Access VMs to install the Mobility service.</span></span> <span data-ttu-id="a2787-134">Každý virtuální počítač, který chcete replikovat, musí mít nainstalován předtím, než můžete povolit pro něho replikaci agenta služby Mobility.</span><span class="sxs-lookup"><span data-stu-id="a2787-134">Each VM you want to replicate must have the Mobility service agent installed before you can enable replication for it.</span></span>

<span data-ttu-id="a2787-135">Přejděte na [krok 6: Příprava VMware](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-135">Go to [Step 6: Prepare VMware](vmware-walkthrough-prepare-vmware.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="a2787-136">Krok 7: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="a2787-136">Step 7: Set up a vault</span></span>

<span data-ttu-id="a2787-137">Budete muset nastavit orchestraci a Správa replikace v trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="a2787-137">You need to set up a Recovery Services vault to orchestrate and manage replication.</span></span> <span data-ttu-id="a2787-138">Když nastavíte trezor, zadejte co chcete replikovat a kam chcete replikovat, aby.</span><span class="sxs-lookup"><span data-stu-id="a2787-138">When you set up the vault, you specify what you want to replicate, and where you want to replicate it to.</span></span>

<span data-ttu-id="a2787-139">Přejděte na [krok 7: nastavení trezoru](vmware-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-139">Go to [Step 7: Set up a vault](vmware-walkthrough-create-vault.md)</span></span>

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="a2787-140">Krok 8: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="a2787-140">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="a2787-141">Nastavte zdroje a cíle, který se používá pro replikaci.</span><span class="sxs-lookup"><span data-stu-id="a2787-141">Set up the source and target that's used for replication.</span></span> <span data-ttu-id="a2787-142">Nastavení nastavení zdroje zahrnuje systémem Unified instalačního programu pro instalaci součásti Site Recovery na místě.</span><span class="sxs-lookup"><span data-stu-id="a2787-142">Setting up source settings includes running Unified Setup to install the on-premises Site Recovery components.</span></span>

<span data-ttu-id="a2787-143">Přejděte na [krok 8: nastavit zdroje a cíle](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-143">Go to [Step 8: Set up the source and target](vmware-walkthrough-source-target.md)</span></span>

## <a name="step-9-set-up-a-replication-policy"></a><span data-ttu-id="a2787-144">Krok 9: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="a2787-144">Step 9: Set up a replication policy</span></span>

<span data-ttu-id="a2787-145">Nastavení zásady zadat nastavení replikace pro virtuální počítače VMware v trezoru.</span><span class="sxs-lookup"><span data-stu-id="a2787-145">You set up a policy to specify replication settings for VMware VMs in the vault.</span></span>

<span data-ttu-id="a2787-146">Přejděte na [krok 9: nastavení zásad replikace](vmware-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-146">Go to [Step 9: Set up a replication policy](vmware-walkthrough-replication.md)</span></span>

## <a name="step-10-install-the-mobility-service"></a><span data-ttu-id="a2787-147">Krok 10: Nainstalujte službu Mobility</span><span class="sxs-lookup"><span data-stu-id="a2787-147">Step 10: Install the Mobility service</span></span>

<span data-ttu-id="a2787-148">Služba Mobility musí být nainstalovaná na jednotlivé virtuální počítače, které chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="a2787-148">The Mobility service must be installed on each VM you want to replicate.</span></span> <span data-ttu-id="a2787-149">Existuje několik způsobů, jak nastavit službu s instalace push nebo pull.</span><span class="sxs-lookup"><span data-stu-id="a2787-149">There are a few ways to set up the service with push or pull installation.</span></span>

<span data-ttu-id="a2787-150">Přejděte na [krok 10: Nainstalujte službu Mobility](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-150">Go to [Step 10: Install the Mobility service](vmware-walkthrough-install-mobility.md)</span></span>

## <a name="step-11-enable-replication"></a><span data-ttu-id="a2787-151">Krok 11: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="a2787-151">Step 11: Enable replication</span></span>

<span data-ttu-id="a2787-152">Jakmile je na virtuálním počítači běží služba Mobility můžete povolit pro něho replikaci.</span><span class="sxs-lookup"><span data-stu-id="a2787-152">After the Mobility service is running on a VM you can enable replication for it.</span></span> <span data-ttu-id="a2787-153">Když povolíte, dojde k počáteční replikaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a2787-153">After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="a2787-154">Přejděte na [krok 11: povolení replikace](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-154">Go to [Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>

## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="a2787-155">Krok 12: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="a2787-155">Step 12: Run a test failover</span></span>

<span data-ttu-id="a2787-156">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="a2787-156">After initial replication finishes, and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="a2787-157">Přejděte na [krok 12: spustit testovací převzetí služeb](vmware-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="a2787-157">Go to [Step 12: Run a test failover](vmware-walkthrough-test-failover.md)</span></span>
