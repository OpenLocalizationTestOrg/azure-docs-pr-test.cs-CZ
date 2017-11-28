---
title: "aaaReplicate fyzický místní servery tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Poskytuje přehled hello kroky pro replikovat úlohy běžící na místní Windows nebo Linuxem fyzických serverů tooAzure s hello služba Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f801b9544072d4029ec06cc1abfd4ff370e852e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-physical-servers-tooazure-with-site-recovery"></a><span data-ttu-id="6538a-103">Replikace fyzických serverů tooAzure službou Site Recovery</span><span class="sxs-lookup"><span data-stu-id="6538a-103">Replicate physical servers tooAzure with Site Recovery</span></span>

<span data-ttu-id="6538a-104">Tento článek obsahuje přehled hello kroky požadované tooreplicate místní Windows nebo Linuxem fyzických serverů tooAzure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6538a-104">This article provides an overview of hello steps required tooreplicate on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="6538a-105">Krok 1: Posouzení architektura a požadavky</span><span class="sxs-lookup"><span data-stu-id="6538a-105">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="6538a-106">Před zahájením nasazení, zkontrolujte architekturu scénáře hello a ujistěte se, že rozumíte všechny součásti hello potřebujete tooset až hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="6538a-106">Before you start deployment, review hello scenario architecture, and make sure you understand all hello components you need tooset up hello deployment.</span></span>

<span data-ttu-id="6538a-107">Přejděte příliš[krok 1: Zkontrolujte architektura hello](physical-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-107">Go too[Step 1: Review hello architecture](physical-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="6538a-108">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="6538a-108">Step 2: Review prerequisites</span></span>

<span data-ttu-id="6538a-109">Ujistěte se, že máte nastavené pro jednotlivé součásti nasazení hello požadavky:</span><span class="sxs-lookup"><span data-stu-id="6538a-109">Make sure you have hello prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="6538a-110">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="6538a-110">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="6538a-111">**Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.</span><span class="sxs-lookup"><span data-stu-id="6538a-111">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="6538a-112">**Replikovat počítače**: servery, které chcete tooreplicate potřebovat toocomply s místními a požadavky pro Azure.</span><span class="sxs-lookup"><span data-stu-id="6538a-112">**Replicated machines**: Servers you want tooreplicate need toocomply with on-premises and Azure requirements.</span></span>

<span data-ttu-id="6538a-113">Přejděte příliš[krok 2: Přečtěte si požadavky a omezení](physical-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-113">Go too[Step 2: Review prerequisites and limitations](physical-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="6538a-114">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="6538a-114">Step 3: Plan capacity</span></span>

<span data-ttu-id="6538a-115">Pokud provádíte úplné nasazení musíte toofigure na jaké replikace prostředky, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6538a-115">If you're doing a full deployment you need toofigure out what replication resources you need.</span></span> <span data-ttu-id="6538a-116">Pokud jste to nějakou rychlou nastavení tootest hello prostředí, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="6538a-116">If you're doing a quick set up tootest hello environment, you can skip this step.</span></span>

<span data-ttu-id="6538a-117">Přejděte příliš[krok 3: plánování kapacity](physical-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-117">Go too[Step 3: Plan capacity](physical-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="6538a-118">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="6538a-118">Step 4: Plan networking</span></span>

<span data-ttu-id="6538a-119">Je nutné toodo některé síťové plánování tooensure, že virtuální počítače Azure jsou připojené toonetworks po převzetí služeb při selhání, a že, ke kterým mají hello správné IP adres.</span><span class="sxs-lookup"><span data-stu-id="6538a-119">You need toodo some network planning tooensure that Azure VMs are connected toonetworks after failover occurs, and  that that they have hello right IP addresses.</span></span>

<span data-ttu-id="6538a-120">Přejděte příliš[krok 4: plánování sítě](physical-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-120">Go too[Step 4: Plan networking](physical-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="6538a-121">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="6538a-121">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="6538a-122">Nastavte před zahájením Azure sítím a úložišti.</span><span class="sxs-lookup"><span data-stu-id="6538a-122">Set up Azure networks and storage before you start.</span></span> 

<span data-ttu-id="6538a-123">Přejděte příliš[krok 5: Příprava Azure](physical-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-123">Go too[Step 5: Prepare Azure](physical-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-set-up-a-vault"></a><span data-ttu-id="6538a-124">Krok 6: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="6538a-124">Step 6: Set up a vault</span></span>

<span data-ttu-id="6538a-125">Můžete nastavit tooorchestrate trezoru služeb zotavení a spravovat replikace.</span><span class="sxs-lookup"><span data-stu-id="6538a-125">You set up a Recovery Services vault tooorchestrate and manage replication.</span></span> <span data-ttu-id="6538a-126">Při nastavování hello trezoru, určíte, co chcete tooreplicate, a místo, kam chcete tooreplicate jeho.</span><span class="sxs-lookup"><span data-stu-id="6538a-126">When you set up hello vault, you specify what you want tooreplicate, and where you want tooreplicate it to.</span></span>

<span data-ttu-id="6538a-127">Přejděte příliš[krok 6: nastavení trezoru](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-127">Go too[Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>

## <a name="step-7-configure-source-and-target-settings"></a><span data-ttu-id="6538a-128">Krok 7: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="6538a-128">Step 7: Configure source and target settings</span></span>

<span data-ttu-id="6538a-129">Nakonfigurujte nastavení pro hello zdrojové a cílové lokality (Azure).</span><span class="sxs-lookup"><span data-stu-id="6538a-129">Configure settings for hello source and target (Azure) site.</span></span> <span data-ttu-id="6538a-130">Nastavení zdroje zahrnuje spuštění tooinstall Unified instalace součásti Site Recovery místní hello.</span><span class="sxs-lookup"><span data-stu-id="6538a-130">Source settings includes running Unified Setup tooinstall hello on-premises Site Recovery components.</span></span>

<span data-ttu-id="6538a-131">Přejděte příliš[krok 7: nastavení hello zdroje a cíle](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-131">Go too[Step 7: Set up hello source and target](physical-walkthrough-source-target.md)</span></span>

## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="6538a-132">Krok 8: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="6538a-132">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="6538a-133">Nastavení zásad toospecify jak fyzické servery musí replikovat.</span><span class="sxs-lookup"><span data-stu-id="6538a-133">You set up a policy toospecify how physical servers should replicate.</span></span>

<span data-ttu-id="6538a-134">Přejděte příliš[krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-134">Go too[Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>

## <a name="step-9-install-hello-mobility-service"></a><span data-ttu-id="6538a-135">Krok 9: Instalace služby Mobility hello</span><span class="sxs-lookup"><span data-stu-id="6538a-135">Step 9: Install hello Mobility service</span></span>

<span data-ttu-id="6538a-136">Služba Mobility Hello musí být nainstalován na každém serveru chcete tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="6538a-136">hello Mobility service must be installed on each server you want tooreplicate.</span></span> <span data-ttu-id="6538a-137">Existuje několik způsobů tooset až služba hello se instalace push nebo pull.</span><span class="sxs-lookup"><span data-stu-id="6538a-137">There are a few ways tooset up hello service, with push or pull installation.</span></span>

<span data-ttu-id="6538a-138">Přejděte příliš[krok 9: instalace služby Mobility hello](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-138">Go too[Step 9: Install hello Mobility service](physical-walkthrough-install-mobility.md)</span></span>

## <a name="step-10-enable-replication"></a><span data-ttu-id="6538a-139">Krok 10: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="6538a-139">Step 10: Enable replication</span></span>

<span data-ttu-id="6538a-140">Po hello služba Mobility běží na serveru, můžete povolit pro něho replikaci.</span><span class="sxs-lookup"><span data-stu-id="6538a-140">After hello Mobility service is running on a server, you can enable replication for it.</span></span> <span data-ttu-id="6538a-141">Když povolíte, dojde k počáteční replikaci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6538a-141">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="6538a-142">Přejděte příliš[krok 10: povolení replikace](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-142">Go too[Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="6538a-143">Krok 11: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="6538a-143">Step 11: Run a test failover</span></span>

<span data-ttu-id="6538a-144">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="6538a-144">After initial replication finishes and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="6538a-145">Přejděte příliš[krok 11: spustit testovací převzetí služeb](physical-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="6538a-145">Go too[Step 11: Run a test failover](physical-walkthrough-test-failover.md)</span></span>

