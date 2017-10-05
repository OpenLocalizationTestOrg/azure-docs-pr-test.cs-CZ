---
title: "Migrace virtuálních počítačů Azure IaaS mezi oblastmi Azure | Microsoft Docs"
description: "Pomocí Azure Site Recovery k migraci virtuálních počítačů Azure IaaS z jedné oblasti Azure do jiné."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: ef2972c077a2b1dd2b2fd6ce53cc6560520ea870
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a><span data-ttu-id="81e5e-103">Migrovat virtuální počítače Azure IaaS mezi oblastmi Azure s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="81e5e-103">Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery</span></span>
## <a name="overview"></a><span data-ttu-id="81e5e-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="81e5e-104">Overview</span></span>
<span data-ttu-id="81e5e-105">Vítejte v Azure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="81e5e-105">Welcome to Azure Site Recovery!</span></span> <span data-ttu-id="81e5e-106">Tento článek použijte, pokud chcete migrovat virtuální počítače Azure mezi oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="81e5e-106">Use this article if you want to migrate Azure VMs between Azure regions.</span></span> <span data-ttu-id="81e5e-107">Než začnete, Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="81e5e-107">Before you start, note that:</span></span>

* <span data-ttu-id="81e5e-108">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="81e5e-108">Azure has two different deployment models for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="81e5e-109">Azure má také dva portály – portál Azure Classic, který podporuje klasický model nasazení, a portál Azure s podporou pro oba modely nasazení.</span><span class="sxs-lookup"><span data-stu-id="81e5e-109">Azure also has two portals – the Azure classic portal that supports the classic deployment model, and the Azure portal with support for both deployment models.</span></span> <span data-ttu-id="81e5e-110">Základní kroky pro migraci jsou stejné, zda konfigurujete Site Recovery ve Správci prostředků nebo classic.</span><span class="sxs-lookup"><span data-stu-id="81e5e-110">The basic steps for migration are the same whether you're configuring Site Recovery in Resource Manager or in classic.</span></span> <span data-ttu-id="81e5e-111">Ale pokyny uživatelského rozhraní a snímky obrazovky v tomto článku se vztahují k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="81e5e-111">However the UI instructions and screenshots in this article are relevant for the Azure portal.</span></span>
* <span data-ttu-id="81e5e-112">**Aktuálně jste můžete migrovat pouze z jedné oblasti do jiné. Můžete převzít virtuálních počítačů z jedné oblasti Azure do jiné, ale nemůžete je po obnovení zpět znovu.**</span><span class="sxs-lookup"><span data-stu-id="81e5e-112">**Currently you can only migrate from one region to another. You can fail over VMs from one Azure region to another, but you can't fail them back again.**</span></span>

<span data-ttu-id="81e5e-113">Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="81e5e-113">Post any comments or questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81e5e-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81e5e-114">Prerequisites</span></span>
<span data-ttu-id="81e5e-115">Zde je nutné pro toto nasazení:</span><span class="sxs-lookup"><span data-stu-id="81e5e-115">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="81e5e-116">**Virtuální počítače IaaS**: virtuálních počítačů, které chcete migrovat.</span><span class="sxs-lookup"><span data-stu-id="81e5e-116">**IaaS virtual machines**: The VMs you want to migrate.</span></span> <span data-ttu-id="81e5e-117">Tyto virtuální počítače migrujete tak, že je považuje jako fyzické počítače.</span><span class="sxs-lookup"><span data-stu-id="81e5e-117">You migrate these VMs by treating them as physical machines.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="81e5e-118">Kroky nasazení</span><span class="sxs-lookup"><span data-stu-id="81e5e-118">Deployment steps</span></span>
<span data-ttu-id="81e5e-119">Tato část popisuje kroky nasazení nového portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="81e5e-119">This section describes the deployment steps in the new Azure portal.</span></span>

1. <span data-ttu-id="81e5e-120">[Vytvoření trezoru](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="81e5e-120">[Create a vault](site-recovery-vmware-to-azure.md).</span></span>
2. <span data-ttu-id="81e5e-121">[Povolit replikaci](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="81e5e-121">[Enable replication](site-recovery-vmware-to-azure.md).</span></span> <span data-ttu-id="81e5e-122">Povolení replikace pro virtuální počítače, které chcete migrovat a zvolte Azure jako zdroj.</span><span class="sxs-lookup"><span data-stu-id="81e5e-122">Enable replication for the VMs you want to migrate, and choose Azure as source.</span></span> 
3. <span data-ttu-id="81e5e-123">[Spustit neplánované převzetí služeb při selhání](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="81e5e-123">[ Run an unplanned failover](site-recovery-failover.md).</span></span> <span data-ttu-id="81e5e-124">Po dokončení počáteční replikace můžete spustit neplánované převzetí služeb při selhání z jedné oblasti Azure do jiné.</span><span class="sxs-lookup"><span data-stu-id="81e5e-124">After initial replication is complete, you can run an unplanned failover from one Azure region to another.</span></span> <span data-ttu-id="81e5e-125">Volitelně můžete vytvořit plán obnovení a spusťte neplánované převzetí služeb při selhání, migrovat několik virtuálních počítačů mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="81e5e-125">Optionally, you can create a recovery plan and run an unplanned failover, to migrate multiple virtual machines between regions.</span></span> <span data-ttu-id="81e5e-126">[Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="81e5e-126">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81e5e-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81e5e-127">Next steps</span></span>
<span data-ttu-id="81e5e-128">Další informace o jiných scénářích replikace v [co je Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="81e5e-128">Learn more about other replication scenarios in [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>
