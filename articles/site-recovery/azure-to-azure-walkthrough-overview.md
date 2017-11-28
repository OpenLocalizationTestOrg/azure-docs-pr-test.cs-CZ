---
title: "virtuální počítače Azure aaaReplicate mezi oblastmi Azure | Microsoft Docs"
description: "Shrnuje hello kroky požadované tooreplicate virtuálních počítačích Azure mezi oblastí Azure se službou Azure Site Recovery hello v hello portálu Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 6dd36239-4363-4538-bf80-a18e71b8ec67
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: f4ac386f040945131f8a10f02143437f4441e298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="cd6c9-103">Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="cd6c9-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

><span data-ttu-id="cd6c9-104">Tento článek obsahuje přehled hello kroky požadované tooreplicate virtuální počítače Azure (VM) v jedné oblasti Azure tooAzure virtuálních počítačů v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-104">This article provides an overview of hello steps required tooreplicate Azure virtual machines (VMs) in one Azure region tooAzure VMs in a different region.</span></span> 

>[!NOTE]
>
> <span data-ttu-id="cd6c9-105">Azure replikace virtuálního počítače je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-105">Azure VM replication is currently in preview.</span></span>

<span data-ttu-id="cd6c9-106">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="cd6c9-106">Post comments and questions at hello bottom of this article or on hello [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="step-1-review-architecture"></a><span data-ttu-id="cd6c9-107">Krok 1: Kontrola architektura</span><span class="sxs-lookup"><span data-stu-id="cd6c9-107">Step 1: Review architecture</span></span>

<span data-ttu-id="cd6c9-108">Před zahájením nasazení, zkontrolujte architekturu scénáře hello a potřebujete toodeploy součásti hello.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-108">Before you start deployment, review hello scenario architecture, and hello components you need toodeploy.</span></span>

<span data-ttu-id="cd6c9-109">Přejděte příliš[krok 1: Zkontrolujte architektura hello](azure-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="cd6c9-109">Go too[Step 1: Review hello architecture](azure-to-azure-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="cd6c9-110">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="cd6c9-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="cd6c9-111">Zkontrolujte, že mají hello požadavky Azure v místech, včetně předplatného, virtuálních sítí, účty úložiště a požadavky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-111">Check that you have hello Azure prerequisites in places, including a subscription, virtual networks, storage accounts, and VM requirements.</span></span>

<span data-ttu-id="cd6c9-112">Přejděte příliš[krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="cd6c9-112">Go too[Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>


## <a name="step-3-plan-networking"></a><span data-ttu-id="cd6c9-113">Krok 3: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="cd6c9-113">Step 3: Plan networking</span></span>

<span data-ttu-id="cd6c9-114">Zkontrolujte, zda odchozí připojení nastavená na virtuálních počítačích Azure, které chcete tooreplicate a že jsou nastavení připojení z místní.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-114">Check that outbound connectivity is set up on Azure VMs you want tooreplicate, and that connections from on-premises are set up.</span></span>

<span data-ttu-id="cd6c9-115">Přejděte příliš[krok 4: plánování sítě](azure-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="cd6c9-115">Go too[Step 4: Plan networking](azure-to-azure-walkthrough-network.md)</span></span>



## <a name="step-4-create-a-vault"></a><span data-ttu-id="cd6c9-116">Krok 4: Vytvoření trezoru</span><span class="sxs-lookup"><span data-stu-id="cd6c9-116">Step 4: Create a vault</span></span> 

<span data-ttu-id="cd6c9-117">Třeba tooset nahoru tooorchestrate trezoru služeb zotavení a správu replikace a zadejte hello zdroj oblast.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-117">You need tooset up a Recovery Services vault tooorchestrate and manage replication, and specify hello source region.</span></span>

<span data-ttu-id="cd6c9-118">Přejděte příliš[krok 4: vytvoření trezoru](azure-to-azure-walkthrough-vault.md)</span><span class="sxs-lookup"><span data-stu-id="cd6c9-118">Go too[Step 4: Create a vault](azure-to-azure-walkthrough-vault.md)</span></span>


## <a name="step-5-enable-replication"></a><span data-ttu-id="cd6c9-119">Krok 5: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="cd6c9-119">Step 5: Enable replication</span></span>


<span data-ttu-id="cd6c9-120">tooenable replikace, nakonfigurovat nastavení cílového umístění, nastavení zásad replikace a vyberte, které chcete tooreplicate hello virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-120">tooenable replication, you configure target location settings, set up a replication policy, and select hello Azure VMs that you want tooreplicate.</span></span> <span data-ttu-id="cd6c9-121">Když povolíte, dojde k počáteční replikaci hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-121">After enabling, initial replication of hello VM occurs.</span></span>

<span data-ttu-id="cd6c9-122">Přejděte příliš[krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="cd6c9-122">Go too[Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-6-run-a-test-failover"></a><span data-ttu-id="cd6c9-123">Krok 6: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="cd6c9-123">Step 6: Run a test failover</span></span>

<span data-ttu-id="cd6c9-124">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit toomake testovací převzetí služeb při selhání, se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="cd6c9-124">After initial replication finishes, and delta replication is running, you can run a test failover toomake sure everything works as expected.</span></span>

<span data-ttu-id="cd6c9-125">Přejděte příliš[krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="cd6c9-125">Go too[Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>


