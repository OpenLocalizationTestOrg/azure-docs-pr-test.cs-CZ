---
title: "Replikovat fyzické místní servery do Azure s Azure Site Recovery | Microsoft Docs"
description: "Obsahuje přehled kroků pro replikaci úlohy běžící na fyzických serverech místní Windows nebo Linuxem do Azure se službou Azure Site Recovery."
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
ms.openlocfilehash: 0a09b35e98dc0b2f5283c2a707a3a2b8ac9a39f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-physical-servers-to-azure-with-site-recovery"></a><span data-ttu-id="52add-103">Replikace fyzických serverů do Azure pomocí Site Recovery</span><span class="sxs-lookup"><span data-stu-id="52add-103">Replicate physical servers to Azure with Site Recovery</span></span>

<span data-ttu-id="52add-104">Tento článek obsahuje přehled kroků nezbytných k replikaci místní Windows nebo Linuxem fyzických serverů do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="52add-104">This article provides an overview of the steps required to replicate on-premises Windows/Linux physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


## <a name="step-1-review-architecture-and-prerequisites"></a><span data-ttu-id="52add-105">Krok 1: Posouzení architektura a požadavky</span><span class="sxs-lookup"><span data-stu-id="52add-105">Step 1: Review architecture and prerequisites</span></span>

<span data-ttu-id="52add-106">Před zahájením nasazení, zkontrolujte na architekturu scénáře a ujistěte se, že rozumíte všechny součásti, které je třeba nastavit nasazení.</span><span class="sxs-lookup"><span data-stu-id="52add-106">Before you start deployment, review the scenario architecture, and make sure you understand all the components you need to set up the deployment.</span></span>

<span data-ttu-id="52add-107">Přejděte na [krok 1: Přečtěte si architektura](physical-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="52add-107">Go to [Step 1: Review the architecture](physical-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="52add-108">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="52add-108">Step 2: Review prerequisites</span></span>

<span data-ttu-id="52add-109">Ujistěte se, že máte požadavky na místě pro každou součást nasazení:</span><span class="sxs-lookup"><span data-stu-id="52add-109">Make sure you have the prerequisites in place for each deployment component:</span></span>

- <span data-ttu-id="52add-110">**Požadavky Azure**: budete potřebovat účet Microsoft Azure, sítě Azure a účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="52add-110">**Azure prerequisites**: You need a Microsoft Azure account, Azure networks, and storage accounts.</span></span>
- <span data-ttu-id="52add-111">**Místní součásti Site Recovery**: budete potřebovat počítač se systémem součásti Site Recovery na místě.</span><span class="sxs-lookup"><span data-stu-id="52add-111">**On-premises Site Recovery components**: You need a machine running on-premises Site Recovery components.</span></span>
- <span data-ttu-id="52add-112">**Replikovat počítače**: servery, které chcete replikovat potřebovat pro dosažení souladu s místními a požadavky pro Azure.</span><span class="sxs-lookup"><span data-stu-id="52add-112">**Replicated machines**: Servers you want to replicate need to comply with on-premises and Azure requirements.</span></span>

<span data-ttu-id="52add-113">Přejděte na [krok 2: Přečtěte si požadavky a omezení](physical-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="52add-113">Go to [Step 2: Review prerequisites and limitations](physical-walkthrough-prerequisites.md)</span></span>

## <a name="step-3-plan-capacity"></a><span data-ttu-id="52add-114">Krok 3: Plánování kapacity</span><span class="sxs-lookup"><span data-stu-id="52add-114">Step 3: Plan capacity</span></span>

<span data-ttu-id="52add-115">Pokud provádíte úplné nasazení potřebujete zjistit, jaké prostředky replikace, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="52add-115">If you're doing a full deployment you need to figure out what replication resources you need.</span></span> <span data-ttu-id="52add-116">Pokud jste to nějakou rychlou nastavit tak, aby testovací prostředí, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="52add-116">If you're doing a quick set up to test the environment, you can skip this step.</span></span>

<span data-ttu-id="52add-117">Přejděte na [Krok 3: Plánování kapacity](physical-walkthrough-capacity.md)</span><span class="sxs-lookup"><span data-stu-id="52add-117">Go to [Step 3: Plan capacity](physical-walkthrough-capacity.md)</span></span>

## <a name="step-4-plan-networking"></a><span data-ttu-id="52add-118">Krok 4: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="52add-118">Step 4: Plan networking</span></span>

<span data-ttu-id="52add-119">Budete muset provést některé síťové plánování zajistit, že virtuální počítače Azure, jsou připojeny k sítím po převzetí služeb při selhání a která, ke kterým mají správné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="52add-119">You need to do some network planning to ensure that Azure VMs are connected to networks after failover occurs, and  that that they have the right IP addresses.</span></span>

<span data-ttu-id="52add-120">Přejděte na [krok 4: plánování sítě](physical-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="52add-120">Go to [Step 4: Plan networking](physical-walkthrough-network.md)</span></span>

##  <a name="step-5-prepare-azure-resources"></a><span data-ttu-id="52add-121">Krok 5: Příprava prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="52add-121">Step 5: Prepare Azure resources</span></span>

<span data-ttu-id="52add-122">Nastavte před zahájením Azure sítím a úložišti.</span><span class="sxs-lookup"><span data-stu-id="52add-122">Set up Azure networks and storage before you start.</span></span> 

<span data-ttu-id="52add-123">Přejděte na [krok 5: Příprava Azure](physical-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="52add-123">Go to [Step 5: Prepare Azure](physical-walkthrough-prepare-azure.md)</span></span>


## <a name="step-6-set-up-a-vault"></a><span data-ttu-id="52add-124">Krok 6: Nastavení trezoru</span><span class="sxs-lookup"><span data-stu-id="52add-124">Step 6: Set up a vault</span></span>

<span data-ttu-id="52add-125">Nastavíte trezor služeb zotavení pro orchestraci a správu replikace.</span><span class="sxs-lookup"><span data-stu-id="52add-125">You set up a Recovery Services vault to orchestrate and manage replication.</span></span> <span data-ttu-id="52add-126">Když nastavíte trezor, zadejte co chcete replikovat a kam chcete replikovat, aby.</span><span class="sxs-lookup"><span data-stu-id="52add-126">When you set up the vault, you specify what you want to replicate, and where you want to replicate it to.</span></span>

<span data-ttu-id="52add-127">Přejděte na [krok 6: nastavení trezoru](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="52add-127">Go to [Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>

## <a name="step-7-configure-source-and-target-settings"></a><span data-ttu-id="52add-128">Krok 7: Konfigurace nastavení zdroje a cíle</span><span class="sxs-lookup"><span data-stu-id="52add-128">Step 7: Configure source and target settings</span></span>

<span data-ttu-id="52add-129">Nakonfigurujte nastavení pro zdrojové a cílové lokality (Azure).</span><span class="sxs-lookup"><span data-stu-id="52add-129">Configure settings for the source and target (Azure) site.</span></span> <span data-ttu-id="52add-130">Nastavení zdroje zahrnuje systémem Unified instalačního programu pro instalaci součásti Site Recovery na místě.</span><span class="sxs-lookup"><span data-stu-id="52add-130">Source settings includes running Unified Setup to install the on-premises Site Recovery components.</span></span>

<span data-ttu-id="52add-131">Přejděte na [krok 7: nastavit zdroje a cíle](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="52add-131">Go to [Step 7: Set up the source and target](physical-walkthrough-source-target.md)</span></span>

## <a name="step-8-set-up-a-replication-policy"></a><span data-ttu-id="52add-132">Krok 8: Nastavení zásad replikace</span><span class="sxs-lookup"><span data-stu-id="52add-132">Step 8: Set up a replication policy</span></span>

<span data-ttu-id="52add-133">Můžete nastavit zásadu k určení jak fyzických serverů musí replikovat.</span><span class="sxs-lookup"><span data-stu-id="52add-133">You set up a policy to specify how physical servers should replicate.</span></span>

<span data-ttu-id="52add-134">Přejděte na [krok 8: nastavení zásad replikace](physical-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="52add-134">Go to [Step 8: Set up a replication policy](physical-walkthrough-replication.md)</span></span>

## <a name="step-9-install-the-mobility-service"></a><span data-ttu-id="52add-135">Krok 9: Instalace služby Mobility</span><span class="sxs-lookup"><span data-stu-id="52add-135">Step 9: Install the Mobility service</span></span>

<span data-ttu-id="52add-136">Služba Mobility musí být nainstalovaná na každém serveru, který chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="52add-136">The Mobility service must be installed on each server you want to replicate.</span></span> <span data-ttu-id="52add-137">Existuje několik způsobů, jak nastavit službu s instalace push nebo pull.</span><span class="sxs-lookup"><span data-stu-id="52add-137">There are a few ways to set up the service, with push or pull installation.</span></span>

<span data-ttu-id="52add-138">Přejděte na [krok 9: instalace služby Mobility](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="52add-138">Go to [Step 9: Install the Mobility service](physical-walkthrough-install-mobility.md)</span></span>

## <a name="step-10-enable-replication"></a><span data-ttu-id="52add-139">Krok 10: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="52add-139">Step 10: Enable replication</span></span>

<span data-ttu-id="52add-140">Jakmile je na serveru běží služba Mobility, můžete povolit pro něho replikaci.</span><span class="sxs-lookup"><span data-stu-id="52add-140">After the Mobility service is running on a server, you can enable replication for it.</span></span> <span data-ttu-id="52add-141">Když povolíte, dojde k počáteční replikaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="52add-141">After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="52add-142">Přejděte na [krok 10: povolení replikace](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="52add-142">Go to [Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>

## <a name="step-11-run-a-test-failover"></a><span data-ttu-id="52add-143">Krok 11: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="52add-143">Step 11: Run a test failover</span></span>

<span data-ttu-id="52add-144">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="52add-144">After initial replication finishes and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="52add-145">Přejděte na [krok 11: spustit testovací převzetí služeb](physical-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="52add-145">Go to [Step 11: Run a test failover](physical-walkthrough-test-failover.md)</span></span>

