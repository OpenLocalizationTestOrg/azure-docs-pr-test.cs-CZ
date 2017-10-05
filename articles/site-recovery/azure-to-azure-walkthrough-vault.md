---
title: "Nastavení úložiště pro virtuální počítač Azure repliction mezi oblastmi s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které budete muset nastavit trezor Azure replikace mezi oblastmi Azure pomocí Azure Site Recovery"
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
ms.openlocfilehash: e03d17992ee0b12049636e40188950bcc4a6f31e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-set-up-a-vault-for-azure-to-azure-replication"></a><span data-ttu-id="72c19-103">Krok 4: Nastavení trezoru pro replikaci Azure do Azure</span><span class="sxs-lookup"><span data-stu-id="72c19-103">Step 4: Set up a vault for Azure to Azure replication</span></span>

<span data-ttu-id="72c19-104">Po [plánování sítě](azure-to-azure-walkthrough-network.md), použijte tento článek k nastavení trezoru, pro virtuální počítače Azure (VM) replikující se do jiné oblasti Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="72c19-104">After [planning networks](azure-to-azure-walkthrough-network.md), use this article to set up a vault, for Azure virtual machines (VMs) replicating to another Azure region, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

- <span data-ttu-id="72c19-105">Po dokončení článek, měli byste mít nastavení trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="72c19-105">When you finish the article, you should have a Recovery Services vault set up.</span></span>
- <span data-ttu-id="72c19-106">Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="72c19-106">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>



>[!NOTE]
>
> <span data-ttu-id="72c19-107">Azure replikace virtuálního počítače je aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="72c19-107">Azure VM replication is currently in preview.</span></span>




## <a name="create-a-vault"></a><span data-ttu-id="72c19-108">Vytvoření trezoru</span><span class="sxs-lookup"><span data-stu-id="72c19-108">Create a vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="72c19-109">Doporučujeme vytvořit trezor služeb zotavení v umístění, kde chcete replikovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="72c19-109">We recommend that you create the Recovery Services vault in the location where you want your VMs to replicate.</span></span> <span data-ttu-id="72c19-110">Například, pokud cílové umístění je střed USA, vytvořte trezor v **střed USA**.</span><span class="sxs-lookup"><span data-stu-id="72c19-110">For example, if your target location is the central US, create the vault in **Central US**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="72c19-111">Další kroky</span><span class="sxs-lookup"><span data-stu-id="72c19-111">Next steps</span></span>

<span data-ttu-id="72c19-112">Přejděte na [krok 5: povolení replikace](azure-to-azure-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="72c19-112">Go to [Step 5: Enable replication](azure-to-azure-walkthrough-enable-replication.md)</span></span>
