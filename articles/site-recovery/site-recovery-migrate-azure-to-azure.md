---
title: "virtuální počítače Azure IaaS aaaMigrate mezi oblastmi Azure | Microsoft Docs"
description: "Použijte virtuální počítače Azure IaaS Azure Site Recovery toomigrate z jedné oblasti Azure tooanother."
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
ms.openlocfilehash: c84dc77716b8d19969eab60707ed1332ca39b893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a><span data-ttu-id="2e0bb-103">Migrovat virtuální počítače Azure IaaS mezi oblastmi Azure s Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="2e0bb-103">Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery</span></span>
## <a name="overview"></a><span data-ttu-id="2e0bb-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="2e0bb-104">Overview</span></span>
<span data-ttu-id="2e0bb-105">Vítejte tooAzure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="2e0bb-105">Welcome tooAzure Site Recovery!</span></span> <span data-ttu-id="2e0bb-106">Tento článek použijte, pokud chcete, aby virtuální počítače Azure toomigrate mezi oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-106">Use this article if you want toomigrate Azure VMs between Azure regions.</span></span> <span data-ttu-id="2e0bb-107">Než začnete, Všimněte si, že:</span><span class="sxs-lookup"><span data-stu-id="2e0bb-107">Before you start, note that:</span></span>

* <span data-ttu-id="2e0bb-108">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: Azure Resource Manager a Klasický model.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-108">Azure has two different deployment models for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="2e0bb-109">Azure má také dva portály – portál Azure classic, která podporuje model nasazení classic hello hello a hello portál Azure s podporou pro oba modely nasazení.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-109">Azure also has two portals – hello Azure classic portal that supports hello classic deployment model, and hello Azure portal with support for both deployment models.</span></span> <span data-ttu-id="2e0bb-110">Hello základní kroky pro migraci jsou hello stejné zda konfigurujete Site Recovery ve Správci prostředků nebo classic.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-110">hello basic steps for migration are hello same whether you're configuring Site Recovery in Resource Manager or in classic.</span></span> <span data-ttu-id="2e0bb-111">Ale hello pokyny uživatelského rozhraní a snímky obrazovky v tomto článku jsou relevantní pro hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-111">However hello UI instructions and screenshots in this article are relevant for hello Azure portal.</span></span>
* <span data-ttu-id="2e0bb-112">**Aktuálně je můžete migrovat pouze z jedné oblasti tooanother. Můžete převzít virtuální počítače z jedné oblasti Azure tooanother, ale nemůžete je po obnovení zpět znovu.**</span><span class="sxs-lookup"><span data-stu-id="2e0bb-112">**Currently you can only migrate from one region tooanother. You can fail over VMs from one Azure region tooanother, but you can't fail them back again.**</span></span>

<span data-ttu-id="2e0bb-113">POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="2e0bb-113">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e0bb-114">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2e0bb-114">Prerequisites</span></span>
<span data-ttu-id="2e0bb-115">Zde je nutné pro toto nasazení:</span><span class="sxs-lookup"><span data-stu-id="2e0bb-115">Here's what you need for this deployment:</span></span>

* <span data-ttu-id="2e0bb-116">**Virtuální počítače IaaS**: hello chcete toomigrate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-116">**IaaS virtual machines**: hello VMs you want toomigrate.</span></span> <span data-ttu-id="2e0bb-117">Tyto virtuální počítače migrujete tak, že je považuje jako fyzické počítače.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-117">You migrate these VMs by treating them as physical machines.</span></span>

## <a name="deployment-steps"></a><span data-ttu-id="2e0bb-118">Kroky nasazení</span><span class="sxs-lookup"><span data-stu-id="2e0bb-118">Deployment steps</span></span>
<span data-ttu-id="2e0bb-119">Tato část popisuje postup nasazení hello hello novou verzi portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-119">This section describes hello deployment steps in hello new Azure portal.</span></span>

1. <span data-ttu-id="2e0bb-120">[Vytvoření trezoru](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="2e0bb-120">[Create a vault](site-recovery-vmware-to-azure.md).</span></span>
2. <span data-ttu-id="2e0bb-121">[Povolit replikaci](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="2e0bb-121">[Enable replication](site-recovery-vmware-to-azure.md).</span></span> <span data-ttu-id="2e0bb-122">Povolení replikace pro virtuální počítače chcete toomigrate a zvolte Azure jako zdroj hello.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-122">Enable replication for hello VMs you want toomigrate, and choose Azure as source.</span></span> 
3. <span data-ttu-id="2e0bb-123">[Spustit neplánované převzetí služeb při selhání](site-recovery-failover.md).</span><span class="sxs-lookup"><span data-stu-id="2e0bb-123">[ Run an unplanned failover](site-recovery-failover.md).</span></span> <span data-ttu-id="2e0bb-124">Po dokončení počáteční replikace můžete spustit neplánované převzetí služeb při selhání z jedné oblasti Azure tooanother.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-124">After initial replication is complete, you can run an unplanned failover from one Azure region tooanother.</span></span> <span data-ttu-id="2e0bb-125">Volitelně můžete vytvořit plán obnovení a spustit více virtuálních počítačů neplánované převzetí služeb při selhání, toomigrate mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-125">Optionally, you can create a recovery plan and run an unplanned failover, toomigrate multiple virtual machines between regions.</span></span> <span data-ttu-id="2e0bb-126">[Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.</span><span class="sxs-lookup"><span data-stu-id="2e0bb-126">[Learn more](site-recovery-create-recovery-plans.md) about recovery plans.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e0bb-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2e0bb-127">Next steps</span></span>
<span data-ttu-id="2e0bb-128">Další informace o jiných scénářích replikace v [co je Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="2e0bb-128">Learn more about other replication scenarios in [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>
