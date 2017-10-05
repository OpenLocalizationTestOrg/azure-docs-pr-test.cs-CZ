---
title: "Příprava prostředků Azure k replikaci virtuálních počítačů technologie Hyper-V (bez System Center VMM) do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace virtuálních počítačů technologie Hyper-V (bez VMM) do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 1a30cadaab7e053184f0be133f1da5bfddc1fd91
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-to-azure"></a><span data-ttu-id="8a9a7-103">Krok 5: Příprava prostředků Azure pro replikaci technologie Hyper-V do Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-103">Step 5: Prepare Azure resources for Hyper-V replication to Azure</span></span>

<span data-ttu-id="8a9a7-104">Postupujte podle pokynů v tomto článku Příprava prostředků Azure, takže můžete replikovat místní virtuální počítače Hyper-V (bez System Center VMM) do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-104">Use the instructions in this article to prepare Azure resources so that you can replicate on-premises Hyper-V VMs (without System Center VMM) to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="8a9a7-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části, nebo požádat technické dotazy na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="8a9a7-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8a9a7-106">Before you start</span></span>

<span data-ttu-id="8a9a7-107">Zajistěte, aby si přečetli [požadavky](hyper-v-site-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="8a9a7-107">Make sure you've read the [prerequisites](hyper-v-site-walkthrough-prerequisites.md)</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="8a9a7-108">Nastavit účet Azure</span><span class="sxs-lookup"><span data-stu-id="8a9a7-108">Set up an Azure account</span></span>

- <span data-ttu-id="8a9a7-109">Získání [účet Microsoft Azure](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="8a9a7-110">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="8a9a7-111">Zkontrolujte podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-111">Check the supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="8a9a7-112">Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>


## <a name="set-up-an-azure-network"></a><span data-ttu-id="8a9a7-113">Nastavení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="8a9a7-113">Set up an Azure network</span></span>

- <span data-ttu-id="8a9a7-114">Nastavte síť Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-114">Set up an Azure network.</span></span> <span data-ttu-id="8a9a7-115">Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="8a9a7-116">Síť musí být ve stejné oblasti jako trezor služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="8a9a7-116">The network should be in the same region as the Recovery Services vault</span></span>
- <span data-ttu-id="8a9a7-117">Site Recovery na portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-117">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="8a9a7-118">Doporučujeme nastavit síť ještě před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-118">We recommend you set up a network before you begin.</span></span> <span data-ttu-id="8a9a7-119">Pokud to neuděláte, budete to muset udělat při nasazení služby Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-119">If you don't, you need to do it during Site Recovery deployment.</span></span>
- <span data-ttu-id="8a9a7-120">Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-120">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="8a9a7-121">Nastavení účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="8a9a7-121">Set up an Azure storage account</span></span>

- <span data-ttu-id="8a9a7-122">Site Recovery replikuje místní počítače do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-122">Site Recovery replicates on-premises machines to Azure storage.</span></span> <span data-ttu-id="8a9a7-123">Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-123">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="8a9a7-124">Nastavit standard nebo premium [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) k ukládání dat replikovaných do Azure.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-124">Set up a standard/premium [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) to hold data replicated to Azure.</span></span>
- <span data-ttu-id="8a9a7-125">[Storage úrovně Premium](../storage/common/storage-premium-storage.md) se obvykle používá u virtuálních počítačů, které je třeba neustále vysoké vstupně-výstupní výkon a nízká latence pro zatížení s intenzivním vstupně-výstupní operace hostitele.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-125">[Premium storage](../storage/common/storage-premium-storage.md) is typically used for virtual machines that need a consistently high IO performance, and low latency to host IO intensive workloads.</span></span>
- <span data-ttu-id="8a9a7-126">Pokud pro ukládání replikovaných dat chcete používat účet Storage úrovně Premium, musíte mít také účet Storage úrovně Standard pro ukládání protokolů replikace, do kterých se zaznamenávají průběžné změny místních dat.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-126">If you want to use a premium account to store replicated data, you also need a standard storage account to store replication logs that capture ongoing changes to on-premises data.</span></span>
- <span data-ttu-id="8a9a7-127">V závislosti na modelu prostředků, který chcete použít pro převzal virtuálních počítačích Azure, které nastavíte účet v [režimu Resource Manager](../storage/common/storage-create-storage-account.md), nebo [klasickém režimu](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="8a9a7-127">Depending on the resource model you want to use for failed over Azure VMs, you set up an account in [Resource Manager mode](../storage/common/storage-create-storage-account.md), or [classic mode](../storage/common/storage-create-storage-account.md).</span></span>
- <span data-ttu-id="8a9a7-128">Doporučujeme, abyste před zahájením nastavení účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-128">We recommend that you set up a storage account before you begin.</span></span> <span data-ttu-id="8a9a7-129">Pokud to neuděláte, budete muset udělat při nasazování Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-129">If you don't you need to do it during Site Recovery deployment.</span></span> <span data-ttu-id="8a9a7-130">Tyto účty musí být ve stejné oblasti jako trezor služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-130">The accounts must be in the same region as the Recovery Services vault.</span></span>
- <span data-ttu-id="8a9a7-131">Nelze přesunout úložiště účtů používaných při obnovení lokality, mezi skupinami prostředků v rámci stejného předplatného, nebo v různých předplatných.</span><span class="sxs-lookup"><span data-stu-id="8a9a7-131">You can't move storage accounts used by Site Recovery across resource groups within the same subscription, or across different subscriptions.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8a9a7-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a9a7-132">Next steps</span></span>

<span data-ttu-id="8a9a7-133">Přejděte na [krok 6: Příprava technologie Hyper-V prostředky](hyper-v-site-walkthrough-prepare-hyper-v.md)</span><span class="sxs-lookup"><span data-stu-id="8a9a7-133">Go to [Step 6: Prepare Hyper-V resources](hyper-v-site-walkthrough-prepare-hyper-v.md)</span></span>
