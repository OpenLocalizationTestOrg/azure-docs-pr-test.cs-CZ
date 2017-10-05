---
title: "Nastavení trezoru pro replikace VMware do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které budete muset nastavit trezor pro replikace VMware do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: dca95ad46b8de587140c3573ba6ed5702a122032
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-to-azure"></a><span data-ttu-id="02258-103">Krok 7: Nastavení trezoru pro replikace VMware do Azure</span><span class="sxs-lookup"><span data-stu-id="02258-103">Step 7: Set up a vault for VMware replication to Azure</span></span>


<span data-ttu-id="02258-104">Tento článek popisuje, jak nastavit trezor a určit, co chcete replikaci z vaší místní umístění, do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="02258-104">This article describes how to set up a vault, and specify what you want to replicate from your on-premises location, to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="02258-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="02258-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="02258-106">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="02258-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="02258-107">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="02258-107">Select a protection goal</span></span>

<span data-ttu-id="02258-108">Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="02258-108">Select what you want to replicate, and where you want to replicate to.</span></span>

1. <span data-ttu-id="02258-109">Klikněte na tlačítko **trezory služeb zotavení** > trezoru.</span><span class="sxs-lookup"><span data-stu-id="02258-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="02258-110">V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="02258-110">In the Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="02258-111">V **cíl ochrany**, vyberte **do Azure** > **Ano, s hypervisoru VMware vSphere**.</span><span class="sxs-lookup"><span data-stu-id="02258-111">In **Protection goal**, select **To Azure** > **Yes, with VMware vSphere Hypervisor**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="02258-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="02258-112">Next steps</span></span>

<span data-ttu-id="02258-113">Přejděte na [krok 8: nastavit zdroje a cíle](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="02258-113">Go to [Step 8: Set up source and target](vmware-walkthrough-source-target.md)</span></span>
