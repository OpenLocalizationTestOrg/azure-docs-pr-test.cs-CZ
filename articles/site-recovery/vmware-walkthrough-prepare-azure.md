---
title: "Příprava prostředků Azure replikovat místní virtuální počítače VMware do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace místní virtuální počítače VMware do Azure pomocí Azure Site Recovery"
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
ms.openlocfilehash: 40abff72278c9f8d9f701023fd473fe52c17b421
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-vmware-replication-to-azure"></a><span data-ttu-id="68bab-103">Krok 5: Příprava prostředků Azure pro replikaci VMWare do Azure.</span><span class="sxs-lookup"><span data-stu-id="68bab-103">Step 5: Prepare Azure resources for VMWare replication to Azure</span></span>


<span data-ttu-id="68bab-104">Postupujte podle pokynů v tomto článku Příprava prostředků Azure tak, aby místní počítače můžete replikovat do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="68bab-104">Use the instructions in this article to prepare Azure resources so that you can replicate on-premises machines to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="68bab-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="68bab-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="68bab-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="68bab-106">Before you start</span></span>

<span data-ttu-id="68bab-107">Zajistěte, aby si přečetli [požadavky](vmware-walkthrough-prerequisites.md)</span><span class="sxs-lookup"><span data-stu-id="68bab-107">Make sure you've read the [prerequisites](vmware-walkthrough-prerequisites.md)</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="68bab-108">Nastavit účet Azure</span><span class="sxs-lookup"><span data-stu-id="68bab-108">Set up an Azure account</span></span>

- <span data-ttu-id="68bab-109">Získání [účet Microsoft Azure](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="68bab-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="68bab-110">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="68bab-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="68bab-111">Zkontrolujte podporované oblasti pro obnovení lokality v rámci geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="68bab-111">Check the supported regions for Site Recovery, Under Geographic Availability in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="68bab-112">Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="68bab-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="68bab-113">Nastavení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="68bab-113">Set up an Azure network</span></span>

- <span data-ttu-id="68bab-114">Nastavte síť Azure.</span><span class="sxs-lookup"><span data-stu-id="68bab-114">Set up an Azure network.</span></span> <span data-ttu-id="68bab-115">Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68bab-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="68bab-116">Site Recovery na portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="68bab-116">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="68bab-117">Síť musí být ve stejné oblasti jako trezor služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="68bab-117">The network should be in the same region as the Recovery Services vault</span></span>
- <span data-ttu-id="68bab-118">Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="68bab-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="68bab-119">Další informace o [připojení virtuálního počítače Azure](site-recovery-network-design.md) po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68bab-119">Learn more about [Azure VM connectivity](site-recovery-network-design.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="68bab-120">Nastavení účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="68bab-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="68bab-121">Site Recovery replikuje místní počítače do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="68bab-121">Site Recovery replicates on-premises machines to Azure storage.</span></span> <span data-ttu-id="68bab-122">Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="68bab-122">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="68bab-123">Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.</span><span class="sxs-lookup"><span data-stu-id="68bab-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="68bab-124">Site Recovery na portálu Azure můžete použít účty úložiště nastavit ve službě Správce prostředků, nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="68bab-124">Site Recovery in the Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="68bab-125">Účet úložiště může být standardní nebo [premium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="68bab-125">The storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="68bab-126">Pokud jste nastavili prémiového účtu taky musíte další standardní účet pro data protokolu.</span><span class="sxs-lookup"><span data-stu-id="68bab-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="68bab-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68bab-127">Next steps</span></span>

<span data-ttu-id="68bab-128">Přejděte na [krok 6: Příprava VMware prostředky](vmware-walkthrough-prepare-vmware.md)</span><span class="sxs-lookup"><span data-stu-id="68bab-128">Go to [Step 6: Prepare VMware resources](vmware-walkthrough-prepare-vmware.md)</span></span>