---
title: "Replikace virtuálních počítačů Azure mezi oblastmi Azure | Microsoft Docs"
description: "Shrnuje kroky potřebné k replikaci virtuálních počítačů Azure mezi oblastí Azure se službou Azure Site Recovery na portálu Azure"
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
ms.openlocfilehash: 9258613161a61e36b1d0c5796d5763c916d66859
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="8290e-103">Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="8290e-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

><span data-ttu-id="8290e-104">Tento článek obsahuje přehled kroků nezbytných k replikaci adresáře Azure virtuální počítače (VM) v jedné oblasti Azure k virtuálním počítačům Azure v jiné oblasti.</span><span class="sxs-lookup"><span data-stu-id="8290e-104">This article provides an overview of the steps required to replicate Azure virtual machines (VMs) in one Azure region to Azure VMs in a different region.</span></span> 

>[!NOTE]
>
> <span data-ttu-id="8290e-105">Azure replikace virtuálního počítače je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="8290e-105">Azure VM replication is currently in preview.</span></span>

<span data-ttu-id="8290e-106">POST dotazy a v dolní části tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="8290e-106">Post comments and questions at the bottom of this article or on the [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="step-1-review-architecture"></a><span data-ttu-id="8290e-107">Krok 1: Kontrola architektura</span><span class="sxs-lookup"><span data-stu-id="8290e-107">Step 1: Review architecture</span></span>

<span data-ttu-id="8290e-108">Před zahájením nasazení, zkontrolujte na architekturu scénáře a součásti, které je třeba nasadit.</span><span class="sxs-lookup"><span data-stu-id="8290e-108">Before you start deployment, review the scenario architecture, and the components you need to deploy.</span></span>

<span data-ttu-id="8290e-109">Přejděte na [krok 1: Přečtěte si architektura](azure-to-azure-walkthrough-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="8290e-109">Go to [Step 1: Review the architecture](azure-to-azure-walkthrough-architecture.md)</span></span>


## <a name="step-2-review-prerequisites"></a><span data-ttu-id="8290e-110">Krok 2: Kontrola předpokladů</span><span class="sxs-lookup"><span data-stu-id="8290e-110">Step 2: Review prerequisites</span></span>

<span data-ttu-id="8290e-111">Zkontrolujte, zda máte požadavky Azure v místech, včetně předplatného, virtuálních sítí, účty úložiště a požadavky virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8290e-111">Check that you have the Azure prerequisites in places, including a subscription, virtual networks, storage accounts, and VM requirements.</span></span>

<span data-ttu-id="8290e-112">Přejděte na [krok 2: ověření předpoklady a omezení](azure-to-azure-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="8290e-112">Go to [Step 2: Verify prerequisites and limitations](azure-to-azure-walkthrough-prerequisites.md)</span></span>


## <a name="step-3-plan-networking"></a><span data-ttu-id="8290e-113">Krok 3: Plánování sítě</span><span class="sxs-lookup"><span data-stu-id="8290e-113">Step 3: Plan networking</span></span>

<span data-ttu-id="8290e-114">Zkontrolujte, zda odchozí připojení nastavená na virtuálních počítačích Azure, které chcete replikovat, a nastavení připojení z místní.</span><span class="sxs-lookup"><span data-stu-id="8290e-114">Check that outbound connectivity is set up on Azure VMs you want to replicate, and that connections from on-premises are set up.</span></span>

<span data-ttu-id="8290e-115">Přejděte na [krok 4: plánování sítě](azure-to-azure-walkthrough-network.md)</span><span class="sxs-lookup"><span data-stu-id="8290e-115">Go to [Step 4: Plan networking](azure-to-azure-walkthrough-network.md)</span></span>



## <a name="step-4-create-a-vault"></a><span data-ttu-id="8290e-116">Krok 4: Vytvoření trezoru</span><span class="sxs-lookup"><span data-stu-id="8290e-116">Step 4: Create a vault</span></span> 

<span data-ttu-id="8290e-117">Budete muset nastavit trezor služeb zotavení pro orchestraci a správu replikace a zadejte oblasti zdroje.</span><span class="sxs-lookup"><span data-stu-id="8290e-117">You need to set up a Recovery Services vault to orchestrate and manage replication, and specify the source region.</span></span>

<span data-ttu-id="8290e-118">Přejděte na [krok 4: vytvoření trezoru](azure-to-azure-walkthrough-vault.md)</span><span class="sxs-lookup"><span data-stu-id="8290e-118">Go to [Step 4: Create a vault](azure-to-azure-walkthrough-vault.md)</span></span>


## <a name="step-5-enable-replication"></a><span data-ttu-id="8290e-119">Krok 5: Povolení replikace</span><span class="sxs-lookup"><span data-stu-id="8290e-119">Step 5: Enable replication</span></span>


<span data-ttu-id="8290e-120">K povolení replikace, nakonfigurujte nastavení cílového umístění, nastavení zásad replikace a vyberte virtuální počítače Azure, který chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="8290e-120">To enable replication, you configure target location settings, set up a replication policy, and select the Azure VMs that you want to replicate.</span></span> <span data-ttu-id="8290e-121">Když povolíte, dojde k počáteční replikaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8290e-121">After enabling, initial replication of the VM occurs.</span></span>

<span data-ttu-id="8290e-122">Přejděte na [krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="8290e-122">Go to [Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>


## <a name="step-6-run-a-test-failover"></a><span data-ttu-id="8290e-123">Krok 6: Spuštění testu převzetí služeb</span><span class="sxs-lookup"><span data-stu-id="8290e-123">Step 6: Run a test failover</span></span>

<span data-ttu-id="8290e-124">Po dokončení počáteční replikace a je spuštěna rozdílová replikace, můžete spustit převzetí služeb při selhání a ujistěte se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="8290e-124">After initial replication finishes, and delta replication is running, you can run a test failover to make sure everything works as expected.</span></span>

<span data-ttu-id="8290e-125">Přejděte na [krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="8290e-125">Go to [Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>


