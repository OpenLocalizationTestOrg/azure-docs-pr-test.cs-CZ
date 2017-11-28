---
title: "aaaPrepare prostředky Azure tooreplicate virtuálních počítačů Hyper-V (s nástrojem System Center VMM) tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace virtuálních počítačů Hyper-V (s nástrojem VMM) tooAzure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1568bdc3-e767-477b-b040-f13699ab5644
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 86bfbab7722fe5bd5b93b92e398d1d441505d3b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-tooazure"></a><span data-ttu-id="15eb8-103">Krok 5: Příprava prostředků Azure pro tooAzure replikace (s VMM) technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="15eb8-103">Step 5: Prepare Azure resources for Hyper-V replication (with VMM) tooAzure</span></span>

<span data-ttu-id="15eb8-104">Po ověření [požadavky na síťovou](vmm-to-azure-walkthrough-network.md), použijte hello pokyny v této článku tooprepare Azure prostředky tak, aby můžete replikovat místní virtuální počítače Hyper-v tooAzure cloudech System Center Virtual Machine Manager (VMM), pomocí Hello [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="15eb8-104">After verifying [network requirements](vmm-to-azure-walkthrough-network.md), use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="15eb8-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello, nebo požádat technické dotazy na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="15eb8-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-an-azure-account"></a><span data-ttu-id="15eb8-106">Nastavit účet Azure</span><span class="sxs-lookup"><span data-stu-id="15eb8-106">Set up an Azure account</span></span>

- <span data-ttu-id="15eb8-107">Získání [účet Microsoft Azure](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="15eb8-107">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="15eb8-108">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15eb8-108">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="15eb8-109">Zkontrolujte hello podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="15eb8-109">Check hello supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="15eb8-110">Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat hello [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="15eb8-110">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="15eb8-111">Ověřte, zda má váš účet Azure hello správné [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="15eb8-111">Make sure your Azure account has hello correct [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines)toocreate Azure VMs.</span></span> <span data-ttu-id="15eb8-112">[Další informace](../active-directory/role-based-access-built-in-roles.md) o řízení přístupu Azure na základě rolí.</span><span class="sxs-lookup"><span data-stu-id="15eb8-112">[Learn more](../active-directory/role-based-access-built-in-roles.md) about Azure role-based access control.</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="15eb8-113">Nastavení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="15eb8-113">Set up an Azure network</span></span>

- <span data-ttu-id="15eb8-114">Nastavení [síť Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span><span class="sxs-lookup"><span data-stu-id="15eb8-114">Set up an [Azure network](../virtual-network/virtual-network-get-started-vnet-subnet.md).</span></span> <span data-ttu-id="15eb8-115">Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="15eb8-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="15eb8-116">Hello síť musí být ve hello stejné oblasti jako hello trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="15eb8-116">hello network should be in hello same region as hello Recovery Services vault</span></span>
- <span data-ttu-id="15eb8-117">Site Recovery na hello portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="15eb8-117">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="15eb8-118">Doporučujeme nastavit síť ještě před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="15eb8-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="15eb8-119">Pokud to neuděláte, musíte toodo ji během nasazování Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="15eb8-119">If you don't, you need toodo it during Site Recovery deployment.</span></span>
- <span data-ttu-id="15eb8-120">Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="15eb8-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="15eb8-121">Nastavení účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="15eb8-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="15eb8-122">Site Recovery replikuje úložiště tooAzure místního počítače.</span><span class="sxs-lookup"><span data-stu-id="15eb8-122">Site Recovery replicates on-premises machines tooAzure storage.</span></span> <span data-ttu-id="15eb8-123">Virtuální počítače Azure jsou vytvořené z úložiště hello po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="15eb8-123">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="15eb8-124">Nastavit standard nebo premium [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replikují tooAzure.</span><span class="sxs-lookup"><span data-stu-id="15eb8-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) toohold data replicated tooAzure.</span></span>
- <span data-ttu-id="15eb8-125">[Storage úrovně Premium](../storage/common/storage-premium-storage.md) se obvykle používá u virtuálních počítačů, které je třeba konzistentně vysoký výkon vstupně-výstupní operace a s nízkou latencí toohost vstupně-výstupní operace náročné úlohy.</span><span class="sxs-lookup"><span data-stu-id="15eb8-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency toohost IO intensive workloads.</span></span>
- <span data-ttu-id="15eb8-126">Pokud chcete toouse toostore účtu premium replikovaná data, musíte taky standardní úložiště účet toostore protokoly replikace, zachycení probíhající změny tooon místní data.</span><span class="sxs-lookup"><span data-stu-id="15eb8-126">If you want toouse a premium account toostore replicated data, you also need a standard storage account toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
- <span data-ttu-id="15eb8-127">V závislosti na modelu prostředků hello chcete toouse pro převzetí služeb virtuálních počítačích Azure při selhání, nastavíte účet v [režimu Resource Manager](../storage/common/storage-create-storage-account.md), nebo [klasickém režimu](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="15eb8-127">Depending on hello resource model you want toouse for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="15eb8-128">Doporučujeme, abyste před zahájením nastavení účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="15eb8-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="15eb8-129">Pokud to neuděláte, budete potřebovat toodo ji během nasazování Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="15eb8-129">If you don't you need toodo it during Site Recovery deployment.</span></span> <span data-ttu-id="15eb8-130">Hello účty musí být v hello stejné oblasti jako hello trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="15eb8-130">hello accounts must be in hello same region as hello Recovery Services vault.</span></span>
- <span data-ttu-id="15eb8-131">Nelze přesunout, účty úložiště používané při Site Recovery přes skupiny prostředků v rámci hello stejné předplatné, nebo v různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="15eb8-131">You can't move storage accounts used by Site Recovery across resource groups within hello same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="15eb8-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="15eb8-132">Next steps</span></span>

<span data-ttu-id="15eb8-133">Přejděte příliš[krok 6: Příprava VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="15eb8-133">Go too[Step 6: Prepare VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md)</span></span>
