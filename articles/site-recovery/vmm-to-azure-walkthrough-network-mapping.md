---
title: "Konfigurace mapování sítě pro replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure s Azure Site Recovery | Microsoft Docs"
description: "Popisuje postup konfigurace mapování sítě při replikaci virtuálních počítačů Hyper-V v cloudech VMM do Azure s Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a><span data-ttu-id="570aa-103">Krok 9: Konfigurace mapování sítě pro replikaci technologie Hyper-V (s nástrojem VMM) do Azure.</span><span class="sxs-lookup"><span data-stu-id="570aa-103">Step 9: Configure network mapping for Hyper-V replication (with VMM) to Azure</span></span>

<span data-ttu-id="570aa-104">Po nastavení [zdrojové a cílové nastavení replikace](vmm-to-azure-walkthrough-source-target.md), použijte tento článek ke konfiguraci mapování sítě pro mapování mezi místními sítěmi virtuálních počítačů nástroje VMM a sítě Azure.</span><span class="sxs-lookup"><span data-stu-id="570aa-104">After you set up the [source and target replication settings](vmm-to-azure-walkthrough-source-target.md), use this article to configure network mapping to map between on-premises VMM VM networks, and Azure networks.</span></span>

<span data-ttu-id="570aa-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="570aa-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="before-you-start"></a><span data-ttu-id="570aa-106">Než začnete</span><span class="sxs-lookup"><span data-stu-id="570aa-106">Before you start</span></span>

- <span data-ttu-id="570aa-107">Další informace o [mapování sítě](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="570aa-107">Learn about [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="570aa-108">[Příprava mapování sítě VMM](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span><span class="sxs-lookup"><span data-stu-id="570aa-108">[Prepare VMM for network mapping](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping).</span></span> 
- <span data-ttu-id="570aa-109">Ověřte, že jsou virtuální počítače na serveru VMM připojené k síti virtuálních počítačů a že jste vytvořili aspoň jednu virtuální síť Azure.</span><span class="sxs-lookup"><span data-stu-id="570aa-109">Verify that virtual machines on the VMM server are connected to a VM network, and that you've created at least one Azure virtual network.</span></span> <span data-ttu-id="570aa-110">Na jednu síť Azure je možné namapovat několik sítí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="570aa-110">Multiple VM networks can be mapped to a single Azure network.</span></span>

## <a name="configure-mapping"></a><span data-ttu-id="570aa-111">Konfigurace mapování</span><span class="sxs-lookup"><span data-stu-id="570aa-111">Configure mapping</span></span>

<span data-ttu-id="570aa-112">Nakonfigurujte mapování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="570aa-112">Configure mapping as follows:</span></span>

1. <span data-ttu-id="570aa-113">V části **Infrastruktura Site Recovery** > **Mapování sítí** > **Mapování sítě** klikněte na ikonu **+Mapování sítě**.</span><span class="sxs-lookup"><span data-stu-id="570aa-113">In **Site Recovery Infrastructure** > **Network mappings** > **Network Mapping**, click the **+Network Mapping** icon.</span></span>

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. <span data-ttu-id="570aa-115">V části **Přidání mapování sítě** vyberte zdrojový server VMM a jako cíl vyberte **Azure**.</span><span class="sxs-lookup"><span data-stu-id="570aa-115">In **Add network mapping**, select the source VMM server, and **Azure** as the target.</span></span>
3. <span data-ttu-id="570aa-116">Ověřte předplatné a model nasazení po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="570aa-116">Verify the subscription and the deployment model after failover.</span></span>
4. <span data-ttu-id="570aa-117">V poli **Zdrojová síť** vyberte ze seznamu přidruženého k tomuto serveru VMM zdrojovou místní síť virtuálních počítačů, kterou chcete namapovat.</span><span class="sxs-lookup"><span data-stu-id="570aa-117">In **Source network**, select the source on-premises VM network you want to map from the list associated with the VMM server.</span></span>
5. <span data-ttu-id="570aa-118">V seznamu **Cílová síť** vyberte síť Azure, ve které budou po vytvoření umístěné virtuální počítače Azure repliky.</span><span class="sxs-lookup"><span data-stu-id="570aa-118">In **Target network**, select the Azure network in which replica Azure VMs will be located when they're created.</span></span> <span data-ttu-id="570aa-119">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="570aa-119">Then click **OK**.</span></span>

    ![Mapování sítě](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

<span data-ttu-id="570aa-121">Když se začne mapovat síť, dojde k tomuto:</span><span class="sxs-lookup"><span data-stu-id="570aa-121">Here's what happens when network mapping begins:</span></span>

* <span data-ttu-id="570aa-122">Když začne mapování, existující virtuální počítače ve zdrojové síti virtuálních počítačů jsou připojené k cílové síti.</span><span class="sxs-lookup"><span data-stu-id="570aa-122">Existing VMs on the source VM network are connected to the target network when mapping begins.</span></span> <span data-ttu-id="570aa-123">Nové virtuální počítače připojené ke zdrojové síti virtuálních počítačů jsou po replikaci připojené k namapované síti Azure.</span><span class="sxs-lookup"><span data-stu-id="570aa-123">New VMs connected to the source VM network are connected to the mapped Azure network when replication occurs.</span></span>
* <span data-ttu-id="570aa-124">Pokud upravíte existující mapování sítě, virtuální počítače repliky se připojí pomocí nového nastavení.</span><span class="sxs-lookup"><span data-stu-id="570aa-124">If you modify an existing network mapping, replica virtual machines connect using the new settings.</span></span>
* <span data-ttu-id="570aa-125">Pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které je umístěný zdrojový virtuální počítač, pak se virtuální počítač repliky po převzetí služeb při selhání připojí k této cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="570aa-125">If the target network has multiple subnets, and one of those subnets has the same name as subnet on which the source virtual machine is located, then the replica virtual machine connects to that target subnet after failover.</span></span>
* <span data-ttu-id="570aa-126">Pokud neexistuje žádná cílová podsíť s odpovídajícím názvem, připojí se virtuální počítač k první podsíti v síti.</span><span class="sxs-lookup"><span data-stu-id="570aa-126">If there’s no target subnet with a matching name, the virtual machine connects to the first subnet in the network.</span></span>



## <a name="next-steps"></a><span data-ttu-id="570aa-127">Další kroky</span><span class="sxs-lookup"><span data-stu-id="570aa-127">Next steps</span></span>

<span data-ttu-id="570aa-128">Přejděte na [krok 10: vytvoření zásady replikace](vmm-to-azure-walkthrough-replication.md)</span><span class="sxs-lookup"><span data-stu-id="570aa-128">Go to [Step 10: Create a replication policy](vmm-to-azure-walkthrough-replication.md)</span></span>
