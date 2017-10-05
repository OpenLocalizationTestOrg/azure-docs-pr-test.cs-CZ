---
title: "Vytvoření trezoru pro replikaci technologie Hyper-V do sekundární lokality s Azure Site Recovery | Microsoft Docs"
description: "Popisuje postup vytvoření trezoru při replikaci virtuálních počítačů Hyper-V do sekundární lokality System Center VMM s Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a><span data-ttu-id="4171f-103">Krok 5: Vytvoření trezoru pro replikaci technologie Hyper-V do sekundární lokality</span><span class="sxs-lookup"><span data-stu-id="4171f-103">Step 5: Create a vault for Hyper-V replication to a secondary site</span></span>

<span data-ttu-id="4171f-104">Jakmile připravíte místní [servery System Center Virtual Machine Manager (VMM) a hostitele nebo Clustery Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md) pro replikaci technologie Hyper-V do sekundární lokality pomocí [Azure Site Recovery](site-recovery-overview.md), můžete vytvořit Trezor služeb zotavení a vyberte scénář replikace.</span><span class="sxs-lookup"><span data-stu-id="4171f-104">After preparing on-premises [System Center Virtual Machine Manager (VMM) servers and Hyper-V hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) for Hyper-V replication to a secondary site using [Azure Site Recovery](site-recovery-overview.md), you can create a Recovery Services vault, and select the replication scenario.</span></span>

<span data-ttu-id="4171f-105">Po přečtení tohoto článku můžete publikovat jakékoli dotazy nebo připomínky na jeho konci nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4171f-105">After reading this article, post any comments at the bottom, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="4171f-106">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="4171f-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a><span data-ttu-id="4171f-107">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="4171f-107">Choose a protection goal</span></span>

<span data-ttu-id="4171f-108">Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="4171f-108">Select what you want to replicate and where you want to replicate to.</span></span>

1. <span data-ttu-id="4171f-109">Klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="4171f-109">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>
2. <span data-ttu-id="4171f-110">Vyberte **k obnovení lokality**a vyberte **Ano, s technologií Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="4171f-110">Select **To recovery site**, and select **Yes, with Hyper-V**.</span></span>
3. <span data-ttu-id="4171f-111">Vyberte **Ano** indikující, že používáte VMM ke správě hostitelů technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="4171f-111">Select **Yes** to indicate you're using VMM to manage the Hyper-V hosts.</span></span>
4. <span data-ttu-id="4171f-112">Vyberte **Ano** Pokud máte sekundární server VMM.</span><span class="sxs-lookup"><span data-stu-id="4171f-112">Select **Yes** if you have a secondary VMM server.</span></span> <span data-ttu-id="4171f-113">Pokud nasazujete replikace mezi cloudy na jednom serveru VMM, klikněte na tlačítko **ne**.</span><span class="sxs-lookup"><span data-stu-id="4171f-113">If you're deploying replication between clouds on a single VMM server, click **No**.</span></span> <span data-ttu-id="4171f-114">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="4171f-114">Then click **OK**.</span></span>

    ![Zvolte cíle.](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="4171f-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4171f-116">Next steps</span></span>

<span data-ttu-id="4171f-117">Přejděte na [krok 6: nastavení replikace zdrojové a cílové](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="4171f-117">Go to [Step 6: Set up the replication source and target](vmm-to-vmm-walkthrough-source-target.md).</span></span>