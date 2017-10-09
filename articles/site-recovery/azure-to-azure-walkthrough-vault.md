---
title: "aaaSet až úložiště pro virtuální počítač Azure repliction mezi oblastmi s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello nutné tooset až do trezoru pro Azure replikaci mezi oblastmi Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 9959c59c7ea57114763f13bf060404ddd267ba80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-a-vault-for-azure-tooazure-replication"></a><span data-ttu-id="6edca-103">Krok 4: Nastavení trezoru Azure tooAzure replikace</span><span class="sxs-lookup"><span data-stu-id="6edca-103">Step 4: Set up a vault for Azure tooAzure replication</span></span>

<span data-ttu-id="6edca-104">Po [plánování sítě](azure-to-azure-walkthrough-network.md), použijte tento článek tooset nahoru k trezoru pro virtuální počítače Azure (VM) replikace tooanother oblast Azure, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6edca-104">After [planning networks](azure-to-azure-walkthrough-network.md), use this article tooset up a vault, for Azure virtual machines (VMs) replicating tooanother Azure region, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

- <span data-ttu-id="6edca-105">Po dokončení hello článku, měli byste mít nastavení trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="6edca-105">When you finish hello article, you should have a Recovery Services vault set up.</span></span>
- <span data-ttu-id="6edca-106">Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6edca-106">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



>[!NOTE]
>
> <span data-ttu-id="6edca-107">Azure replikace virtuálního počítače je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="6edca-107">Azure VM replication is currently in preview.</span></span>




## <a name="create-a-vault"></a><span data-ttu-id="6edca-108">Vytvoření trezoru</span><span class="sxs-lookup"><span data-stu-id="6edca-108">Create a vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="6edca-109">Doporučujeme vytvořit trezor služeb zotavení hello v hello umístění, kam má tooreplicate vaše virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6edca-109">We recommend that you create hello Recovery Services vault in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="6edca-110">Například pokud cílové umístění je hello střed USA, vytvořte trezor hello v **střed USA**.</span><span class="sxs-lookup"><span data-stu-id="6edca-110">For example, if your target location is hello central US, create hello vault in **Central US**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6edca-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6edca-111">Next steps</span></span>

<span data-ttu-id="6edca-112">Přejděte příliš[krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="6edca-112">Go too[Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>
