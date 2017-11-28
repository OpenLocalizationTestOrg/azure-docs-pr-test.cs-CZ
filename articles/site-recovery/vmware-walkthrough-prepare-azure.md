---
title: "prostředky Azure tooreplicate aaaPrepare místní virtuální počítače VMware tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure než začnete replikovat místní virtuální počítače VMware tooAzure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ac72fff0593783add789408ecfeb1812d70108b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-tooazure"></a><span data-ttu-id="c069f-103">Krok 5: Příprava prostředků Azure pro tooAzure replikace VMWare</span><span class="sxs-lookup"><span data-stu-id="c069f-103">Step 5: Prepare Azure resources for VMWare replication tooAzure</span></span>


<span data-ttu-id="c069f-104">Použijte hello pokyny v této článku tooprepare Azure prostředky tak, aby můžete replikovat místní počítače tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="c069f-104">Use hello instructions in this article tooprepare Azure resources so that you can replicate on-premises machines tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="c069f-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c069f-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="c069f-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c069f-106">Before you start</span></span>

<span data-ttu-id="c069f-107">Ujistěte se, že jste si přečetli hello [požadavky](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="c069f-107">Make sure you've read hello [prerequisites](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="c069f-108">Nastavit účet Azure</span><span class="sxs-lookup"><span data-stu-id="c069f-108">Set up an Azure account</span></span>

- <span data-ttu-id="c069f-109">Získání [účet Microsoft Azure](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="c069f-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="c069f-110">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c069f-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="c069f-111">Zkontrolujte hello podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="c069f-111">Check hello supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="c069f-112">Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat hello [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="c069f-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get hello [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="c069f-113">Nastavení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="c069f-113">Set up an Azure network</span></span>

- <span data-ttu-id="c069f-114">Nastavte síť Azure.</span><span class="sxs-lookup"><span data-stu-id="c069f-114">Set up an Azure network.</span></span> <span data-ttu-id="c069f-115">Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c069f-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="c069f-116">Site Recovery na hello portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="c069f-116">Site Recovery in hello Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="c069f-117">Hello síť musí být ve hello stejné oblasti jako hello trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="c069f-117">hello network should be in hello same region as hello Recovery Services vault</span></span>
- <span data-ttu-id="c069f-118">Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="c069f-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="c069f-119">Další informace o [připojení virtuálního počítače Azure](site-recovery-network-design.md) po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c069f-119">Learn more about [Azure VM connectivity](site-recovery-network-design.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="c069f-120">Nastavení účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="c069f-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="c069f-121">Site Recovery replikuje úložiště tooAzure místního počítače.</span><span class="sxs-lookup"><span data-stu-id="c069f-121">Site Recovery replicates on-premises machines tooAzure storage.</span></span> <span data-ttu-id="c069f-122">Virtuální počítače Azure jsou vytvořené z úložiště hello po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c069f-122">Azure VMs are created from hello storage after failover occurs.</span></span>
- <span data-ttu-id="c069f-123">Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.</span><span class="sxs-lookup"><span data-stu-id="c069f-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="c069f-124">Site Recovery na hello portálu Azure můžete použít účty úložiště nastavit ve službě Správce prostředků, nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="c069f-124">Site Recovery in hello Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="c069f-125">účet úložiště Hello může být standardní nebo [premium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c069f-125">hello storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="c069f-126">Pokud jste nastavili prémiového účtu taky musíte další standardní účet pro data protokolu.</span><span class="sxs-lookup"><span data-stu-id="c069f-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="c069f-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c069f-127">Next steps</span></span>

<span data-ttu-id="c069f-128">Přejděte příliš[krok 6: Příprava VMware prostředky](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="c069f-128">Go too[Step 6: Prepare VMware resources](vmware-walkthrough-prepare-vmware.md)</span></span>
