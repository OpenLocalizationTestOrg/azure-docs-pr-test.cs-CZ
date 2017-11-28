---
title: "aaaReplicate virtuálních počítačů Hyper-V v nástroji VMM cloudů tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled pro replikaci virtuálních počítačů Hyper-V v tooAzure cloudů VMM pomocí služby Azure Site Recovery hello"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: d6f729a49cc86ea07bebc4d7266fd7b58b3998f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a><span data-ttu-id="88172-103">Replikace virtuálních počítačů technologie Hyper-V v tooAzure cloudů VMM pomocí Site Recovery v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="88172-103">Replicate Hyper-V virtual machines in VMM clouds tooAzure using Site Recovery in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="88172-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="88172-104">Azure portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="88172-105">Azure Classic</span><span class="sxs-lookup"><span data-stu-id="88172-105">Azure classic</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="88172-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="88172-106">PowerShell Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="88172-107">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="88172-107">PowerShell classic</span></span>](site-recovery-deploy-with-powershell.md)


<span data-ttu-id="88172-108">Tento článek obsahuje přehled hello kroky požadované tooreplicate místní virtuální počítače Hyper-V (VM) spravované v cloudech tooAzure System Center Virtual Machine Manager (VMM), pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v Hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="88172-108">This article provides an overview of hello steps required tooreplicate on-premises Hyper-V virtual machines (VMs) managed in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="88172-109">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="88172-109">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="step-1-review-hello-scenario-architecture"></a><span data-ttu-id="88172-110">Krok 1: Posouzení architekturu scénáře hello</span><span class="sxs-lookup"><span data-stu-id="88172-110">Step 1: Review hello scenario architecture</span></span>

<span data-ttu-id="88172-111">Před spuštěním nasazení, zkontrolujte architekturu scénáře hello, ujistěte se, že rozumíte všechny součásti hello musíte toodeploy.</span><span class="sxs-lookup"><span data-stu-id="88172-111">Before you start deployment, review hello scenario architecture, and make sure that you understand all hello components you need toodeploy.</span></span>

<span data-ttu-id="88172-112">Přejděte příliš[krok 1: Zkontrolujte architektura hello](vmm-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="88172-112">Go too[Step 1: Review hello architecture](vmm-to-azure-walkthrough-architecture.md)</span></span>

## <a name="step-2-review-prerequisites-and-limitations"></a><span data-ttu-id="88172-113">Krok 2: Kontrola předpoklady a omezení</span><span class="sxs-lookup"><span data-stu-id="88172-113">Step 2: Review prerequisites and limitations</span></span>

<span data-ttu-id="88172-114">Ujistěte se, že rozumíte hello nasazení předpoklady a omezení.</span><span class="sxs-lookup"><span data-stu-id="88172-114">Make sure that you understand hello deployment prerequisites and limitations.</span></span>

<span data-ttu-id="88172-115">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="88172-115">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
<span data-ttu-id="88172-116">**Na místní servery VMM a hostitelé Hyper-V**: Ujistěte se, že jsou servery VMM a hostitelé Hyper-V kompatibilní a připravený pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="88172-116">**On-premises VMM servers and Hyper-V hosts**: Make sure that VMM servers and Hyper-V hosts are compliant and prepared for Site Recovery deployment.</span></span>
<span data-ttu-id="88172-117">**Replikovat virtuální počítače**: Zkontrolujte, zda chcete tooreplicate virtuálních počítačů v souladu s požadavky na Azure.</span><span class="sxs-lookup"><span data-stu-id="88172-117">**Replicated VMs**: Check that VMs you want tooreplicate comply with Azure requirements.</span></span>

<span data-ttu-id="88172-118">Přejděte příliš[krok 2: ověření předpoklady a omezení](vmm-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="88172-118">Go too[Step 2: Verify prerequisites and limitations](vmm-to-azure-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="88172-119">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="88172-119">Step 3: Plan capacity</span></span>

<span data-ttu-id="88172-120">Pokud provádíte úplné nasazení, je třeba toofigure na jaké replikace prostředky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="88172-120">If you're doing a full deployment, you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="88172-121">Existuje několik z nástrojů k dispozici toohelp to uděláte.</span><span class="sxs-lookup"><span data-stu-id="88172-121">There are a couple of tools available toohelp you do this.</span></span> <span data-ttu-id="88172-122">Pokud jste to nějakou rychlou nastavení tootest hello prostředí, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="88172-122">If you're doing a quick set up tootest hello environment, you can skip this step.</span></span>

<span data-ttu-id="88172-123">Přejděte příliš[krok 3: plánování kapacity](vmm-to-azure-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="88172-123">Go too[Step 3: Plan capacity](vmm-to-azure-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="88172-124">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="88172-124">Step 4: Plan networking</span></span>

<span data-ttu-id="88172-125">Je nutné toodo některé síťové plánování tooensure, které lze konfigurovat mapování sítě, když nasazujete hello scénář, virtuální počítače Azure budou virtuální sítě připojený tooAzure po převzetí služeb při selhání, a který je přiřazený příslušné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="88172-125">You need toodo some network planning tooensure that you can configure network mapping when you deploy hello scenario, that Azure VMs will be connected tooAzure virtual networks after failover occurs, and that that they're assigned appropriate IP addresses.</span></span>

<span data-ttu-id="88172-126">Přejděte příliš[krok 4: plánování sítě](vmm-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="88172-126">Go too[Step 4: Plan networking](vmm-to-azure-walkthrough-network.md)</span></span>


## <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="88172-127">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="88172-127">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="88172-128">Nastavte účet Azure, sítě a úložiště.</span><span class="sxs-lookup"><span data-stu-id="88172-128">Set up an Azure account, networks, and storage.</span></span> <span data-ttu-id="88172-129">Můžete to provést během nasazení, ale doporučujeme, abyste že to uděláte před zahájením.</span><span class="sxs-lookup"><span data-stu-id="88172-129">You can do this during deployment, but we recommend you do this before you start.</span></span>

<span data-ttu-id="88172-130">Přejděte příliš[krok 5: Příprava Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="88172-130">Go too[Step 5: Prepare Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span></span>

## <a name="step-6-prepare-vmm-and-hyper-v"></a><span data-ttu-id="88172-131">Krok 6: Příprava VMM a technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="88172-131">Step 6: Prepare VMM and Hyper-V</span></span>

<span data-ttu-id="88172-132">Příprava serverů VMM místní hello a hostitelů Hyper-V pro nasazení Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="88172-132">Prepare hello on-premises VMM servers and Hyper-V hosts for Site Recovery deployment.</span></span>

<span data-ttu-id="88172-133">Přejděte příliš[krok 6: Příprava na místní servery](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="88172-133">Go too[Step 6: Prepare on-premises servers](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>

## <a name="step-7-set-up-a-vault"></a><span data-ttu-id="88172-134">Krok 7: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="88172-134">Step 7: Set up a vault</span></span>

<span data-ttu-id="88172-135">Nastavení trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="88172-135">Set up a Recovery Services vault.</span></span> <span data-ttu-id="88172-136">Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.</span><span class="sxs-lookup"><span data-stu-id="88172-136">hello vault contains configuration settings, and orchestrates replication.</span></span>

[<span data-ttu-id="88172-137">Krok 7: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="88172-137">Step 7: Set up a vault</span></span>](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a><span data-ttu-id="88172-138">Krok 8: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="88172-138">Step 8: Configure source and target settings</span></span>

<span data-ttu-id="88172-139">Nastavte hello zdrojové a cílové umístění replikace.</span><span class="sxs-lookup"><span data-stu-id="88172-139">Set up hello source and target replication locations.</span></span> <span data-ttu-id="88172-140">Přidejte hello VMM server toohello trezoru a stáhnout hello instalační soubory pro součásti Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="88172-140">Add hello VMM server toohello vault, and download hello installation files for Site Recovery components.</span></span> <span data-ttu-id="88172-141">Spusťte instalační program zprostředkovatele Azure Site Recovery na serveru VMM hello.</span><span class="sxs-lookup"><span data-stu-id="88172-141">Run Azure Site Recovery Provider setup on hello VMM server.</span></span> <span data-ttu-id="88172-142">Instalační program nainstaluje hello zprostředkovatele na serveru VMM hello a zaregistruje hello server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="88172-142">Setup installs hello Provider on hello VMM server, and registers hello server in hello vault.</span></span> <span data-ttu-id="88172-143">Nainstalujte agenta služeb zotavení Microsoft hello na každém hostiteli technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="88172-143">You install hello Microsoft Recovery Services agent on each Hyper-V host.</span></span>

<span data-ttu-id="88172-144">Přejděte příliš[krok 8: Konfigurace nastavení zdroje a cíle](vmm-to-azure-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="88172-144">Go too[Step 8: Configure source and target settings](vmm-to-azure-walkthrough-source-target.md)</span></span>

## <a name="step-9-configure-network-mapping"></a><span data-ttu-id="88172-145">Krok 9: Konfigurace mapování sítě</span><span class="sxs-lookup"><span data-stu-id="88172-145">Step 9: Configure network mapping</span></span>

<span data-ttu-id="88172-146">Mapa místní virtuální sítě pro tooAzure sítě virtuálních počítačů nástroje VMM.</span><span class="sxs-lookup"><span data-stu-id="88172-146">Map on-premises VMM VM networks tooAzure virtual networks.</span></span> <span data-ttu-id="88172-147">Po převzetí služeb při selhání jsou virtuální počítače Azure vytvořené v hello síť Azure, která mapuje sítě virtuálních počítačů místní toohello, ve které hello se nachází zdroj technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="88172-147">After failover, Azure VMs are created in hello Azure network that maps toohello on-premises VM network in which hello source Hyper-V is located.</span></span>

<span data-ttu-id="88172-148">Přejděte příliš[krok 9: nakonfigurování mapování sítě](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="88172-148">Go too[Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>


## <a name="step-10-set-up-a-replication-policy"></a><span data-ttu-id="88172-149">Krok 10: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="88172-149">Step 10: Set up a replication policy</span></span>

<span data-ttu-id="88172-150">Zadejte, jak místní virtuální počítače budou replikované tooAzure.</span><span class="sxs-lookup"><span data-stu-id="88172-150">Specify how on-premises VMs will be replicated tooAzure.</span></span>

<span data-ttu-id="88172-151">Přejděte příliš[krok 10: nastavení zásad replikace](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="88172-151">Go too[Step 10: Set up a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>


## <a name="step-11-enable-replication-for-vms"></a><span data-ttu-id="88172-152">Krok 11: Povolení replikace pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="88172-152">Step 11: Enable replication for VMs</span></span>

<span data-ttu-id="88172-153">Vyberte virtuální počítače hello chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="88172-153">Select hello VMs you want tooreplicate.</span></span> <span data-ttu-id="88172-154">Povolení virtuálního počítače pro replikaci aktivační události hello počáteční replikace tooAzure, následovaný probíhající rozdílová replikace.</span><span class="sxs-lookup"><span data-stu-id="88172-154">ENabling a VM for replication triggers hello initial replication tooAzure, followed by ongoing delta replication.</span></span>

<span data-ttu-id="88172-155">Přejděte příliš[krok 11: povolení replikace](vmm-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="88172-155">Go too[Step 11: Enable replication](vmm-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-12-run-a-test-failover"></a><span data-ttu-id="88172-156">Krok 12: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="88172-156">Step 12: Run a test failover</span></span>

<span data-ttu-id="88172-157">Spusťte toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="88172-157">Run a test failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="88172-158">Přejděte příliš[krok 12: spustit testovací převzetí služeb](vmm-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="88172-158">Go too[Step 12: Run a test failover](vmm-to-azure-walkthrough-test-failover.md)</span></span>


