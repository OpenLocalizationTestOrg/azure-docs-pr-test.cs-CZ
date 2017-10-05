---
title: "Příprava prostředků Azure, aby replikace místní fyzických serverů do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, co potřebujete zavedené v Azure před zahájením replikace místních serverů do Azure pomocí služby Azure Site Recovery"
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
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: b7411fa6aba04ffd34f3f4bd03e706ca75afc9c8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a><span data-ttu-id="57d43-103">Krok 5: Příprava prostředků Azure pro fyzický server replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="57d43-103">Step 5: Prepare Azure resources for physical server replication to Azure</span></span>


<span data-ttu-id="57d43-104">Postupujte podle pokynů v tomto článku Příprava prostředků Azure tak, aby místních serverů se můžou replikovat do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) služby.</span><span class="sxs-lookup"><span data-stu-id="57d43-104">Use the instructions in this article to prepare Azure resources so that you can replicate on-premises servers to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="57d43-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="57d43-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="57d43-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="57d43-106">Before you start</span></span>

<span data-ttu-id="57d43-107">Zajistěte, aby si přečetli [požadavky](physical-walkthrough-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="57d43-107">Make sure you've read the [prerequisites](physical-walkthrough-prerequisites.md).</span></span>

## <a name="set-up-an-azure-account"></a><span data-ttu-id="57d43-108">Nastavit účet Azure</span><span class="sxs-lookup"><span data-stu-id="57d43-108">Set up an Azure account</span></span>

- <span data-ttu-id="57d43-109">Získání [účet Microsoft Azure](http://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="57d43-109">Get a [Microsoft Azure account](http://azure.microsoft.com/).</span></span>
- <span data-ttu-id="57d43-110">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="57d43-110">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
- <span data-ttu-id="57d43-111">Zkontrolujte podporované oblasti pro obnovení lokality v části **geografickou dostupností** v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="57d43-111">Check the supported regions for Site Recovery, under **Geographic Availability** in [Azure Site Recovery Pricing Details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
- <span data-ttu-id="57d43-112">Další informace o [cenách za Site Recovery](site-recovery-faq.md#pricing)a získat [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="57d43-112">Learn about [Site Recovery pricing](site-recovery-faq.md#pricing), and get the [pricing details](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>



## <a name="set-up-an-azure-network"></a><span data-ttu-id="57d43-113">Nastavení sítě Azure</span><span class="sxs-lookup"><span data-stu-id="57d43-113">Set up an Azure network</span></span>

- <span data-ttu-id="57d43-114">Nastavte síť Azure.</span><span class="sxs-lookup"><span data-stu-id="57d43-114">Set up an Azure network.</span></span> <span data-ttu-id="57d43-115">Virtuální počítače Azure jsou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="57d43-115">Azure VMs are placed in this network when they're created after failover.</span></span>
- <span data-ttu-id="57d43-116">Site Recovery na portálu Azure můžete použít nastavení sítích [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="57d43-116">Site Recovery in the Azure portal can use networks set up in [Resource Manager](../resource-manager-deployment-model.md), or in classic mode.</span></span>
- <span data-ttu-id="57d43-117">Síť by měla být ve stejném umístění jako trezor služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="57d43-117">The network should be in the same region as the Recovery Services vault.</span></span>
- <span data-ttu-id="57d43-118">Další informace o [ceny virtuální sítě](https://azure.microsoft.com/pricing/details/virtual-network/).</span><span class="sxs-lookup"><span data-stu-id="57d43-118">Learn about [virtual network pricing](https://azure.microsoft.com/pricing/details/virtual-network/).</span></span>
- <span data-ttu-id="57d43-119">Další informace o [připojení virtuálního počítače Azure](physical-walkthrough-network.md) po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="57d43-119">Learn more about [Azure VM connectivity](physical-walkthrough-network.md) after failover.</span></span>


## <a name="set-up-an-azure-storage-account"></a><span data-ttu-id="57d43-120">Nastavení účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="57d43-120">Set up an Azure storage account</span></span>

- <span data-ttu-id="57d43-121">Obnovení lokality se replikují na místní servery do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="57d43-121">Site Recovery replicates on-premises servers to Azure storage.</span></span> <span data-ttu-id="57d43-122">Virtuální počítače Azure jsou vytvořené z úložiště po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="57d43-122">Azure VMs are created from the storage after failover occurs.</span></span>
- <span data-ttu-id="57d43-123">Nastavení [účtu úložiště Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.</span><span class="sxs-lookup"><span data-stu-id="57d43-123">Set up an [Azure storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) for replicated data.</span></span>
- <span data-ttu-id="57d43-124">Site Recovery na portálu Azure můžete použít účty úložiště nastavit ve službě Správce prostředků, nebo v klasickém režimu.</span><span class="sxs-lookup"><span data-stu-id="57d43-124">Site Recovery in the Azure portal can use storage accounts set up in Resource Manager, or in classic mode.</span></span>
- <span data-ttu-id="57d43-125">Účet úložiště může být standardní nebo [premium](../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="57d43-125">The storage account can be standard or [premium](../storage/common/storage-premium-storage.md).</span></span>
- <span data-ttu-id="57d43-126">Pokud jste nastavili prémiového účtu taky musíte další standardní účet pro data protokolu.</span><span class="sxs-lookup"><span data-stu-id="57d43-126">If you set up a premium account, you will also need an additional standard account for log data.</span></span>


## <a name="next-steps"></a><span data-ttu-id="57d43-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57d43-127">Next steps</span></span>

<span data-ttu-id="57d43-128">Přejděte na [krok 6: nastavení trezoru](physical-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="57d43-128">Go to [Step 6: Set up a vault](physical-walkthrough-create-vault.md)</span></span>
