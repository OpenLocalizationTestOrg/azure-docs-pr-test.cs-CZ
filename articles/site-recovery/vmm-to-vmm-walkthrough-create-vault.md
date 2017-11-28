---
title: "aaaCreate trezoru pro tooa replikace technologie Hyper-V s Azure Site Recovery sekundární lokality. | Microsoft Docs"
description: "Popisuje, jak toocreate trezoru při replikaci virtuálních počítačů Hyper-V tooa sekundární System Center VMM lokalitu s Azure Site Recovery."
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
ms.openlocfilehash: 96ee09cbf2376a5089b9efa09dc7ab3fb7d472cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-create-a-vault-for-hyper-v-replication-tooa-secondary-site"></a><span data-ttu-id="bcfa3-103">Krok 5: Vytvoření trezoru pro sekundární lokalitě tooa replikace Hyper-V</span><span class="sxs-lookup"><span data-stu-id="bcfa3-103">Step 5: Create a vault for Hyper-V replication tooa secondary site</span></span>

<span data-ttu-id="bcfa3-104">Jakmile připravíte místní [servery System Center Virtual Machine Manager (VMM) a hostitele nebo Clustery Hyper-V](vmm-to-vmm-walkthrough-vmm-hyper-v.md) pro sekundární lokalitě tooa replikace technologie Hyper-V pomocí [Azure Site Recovery](site-recovery-overview.md), můžete vytvořit Trezor služeb zotavení a vyberte hello replikace scénář.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-104">After preparing on-premises [System Center Virtual Machine Manager (VMM) servers and Hyper-V hosts/clusters](vmm-to-vmm-walkthrough-vmm-hyper-v.md) for Hyper-V replication tooa secondary site using [Azure Site Recovery](site-recovery-overview.md), you can create a Recovery Services vault, and select hello replication scenario.</span></span>

<span data-ttu-id="bcfa3-105">Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="bcfa3-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="bcfa3-106">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="bcfa3-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a><span data-ttu-id="bcfa3-107">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="bcfa3-107">Choose a protection goal</span></span>

<span data-ttu-id="bcfa3-108">Vyberte, co chcete tooreplicate a místo, kam chcete tooreplicate k.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-108">Select what you want tooreplicate and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="bcfa3-109">Klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-109">Click **Site Recovery** > **Step 1: Prepare Infrastructure** > **Protection goal**.</span></span>
2. <span data-ttu-id="bcfa3-110">Vyberte **toorecovery lokality**a vyberte **Ano, s technologií Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-110">Select **toorecovery site**, and select **Yes, with Hyper-V**.</span></span>
3. <span data-ttu-id="bcfa3-111">Vyberte **Ano** tooindicate používáte hostitelů nástroje VMM toomanage hello technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-111">Select **Yes** tooindicate you're using VMM toomanage hello Hyper-V hosts.</span></span>
4. <span data-ttu-id="bcfa3-112">Vyberte **Ano** Pokud máte sekundární server VMM.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-112">Select **Yes** if you have a secondary VMM server.</span></span> <span data-ttu-id="bcfa3-113">Pokud nasazujete replikace mezi cloudy na jednom serveru VMM, klikněte na tlačítko **ne**.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-113">If you're deploying replication between clouds on a single VMM server, click **No**.</span></span> <span data-ttu-id="bcfa3-114">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcfa3-114">Then click **OK**.</span></span>

    ![Zvolte cíle.](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a><span data-ttu-id="bcfa3-116">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcfa3-116">Next steps</span></span>

<span data-ttu-id="bcfa3-117">Přejděte příliš[krok 6: nastavení hello replikace zdrojové a cílové](vmm-to-vmm-walkthrough-source-target.md).</span><span class="sxs-lookup"><span data-stu-id="bcfa3-117">Go too[Step 6: Set up hello replication source and target](vmm-to-vmm-walkthrough-source-target.md).</span></span>
