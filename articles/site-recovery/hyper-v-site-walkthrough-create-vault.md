---
title: "Nastavení úložiště pro replikaci technologie Hyper-V (bez System Center VMM) do Azure pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které budete muset nastavit trezor pro replikaci technologie Hyper-V do Azure pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 8212ff011633c3a89d3310e828b6d5f1cda6ce3f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="3dc27-103">Krok 7: Nastavení trezoru pro replikaci technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="3dc27-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="3dc27-104">Tento článek popisuje, jak nastavit trezor a určit, co chcete replikaci z vaší místní umístění, do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="3dc27-104">This article describes how to set up a vault, and specify what you want to replicate from your on-premises location, to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="3dc27-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3dc27-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="3dc27-106">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="3dc27-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="3dc27-107">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="3dc27-107">Select a protection goal</span></span>

<span data-ttu-id="3dc27-108">Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.</span><span class="sxs-lookup"><span data-stu-id="3dc27-108">Select what you want to replicate, and where you want to replicate to.</span></span>

1. <span data-ttu-id="3dc27-109">Klikněte na tlačítko **trezory služeb zotavení** > trezoru.</span><span class="sxs-lookup"><span data-stu-id="3dc27-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="3dc27-110">V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="3dc27-110">In the Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="3dc27-111">V **cíl ochrany**, vyberte **do Azure** > **Ano, s technologií Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="3dc27-111">In **Protection goal**, select **To Azure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="3dc27-112">Vyberte **ne** potvrďte, že nepoužíváte VMM.</span><span class="sxs-lookup"><span data-stu-id="3dc27-112">Select **No** to confirm you're not using VMM.</span></span> 

    ![Zvolte cíle.](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a><span data-ttu-id="3dc27-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3dc27-114">Next steps</span></span>

<span data-ttu-id="3dc27-115">Přejděte na [krok 8: nastavit zdroje a cíle](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="3dc27-115">Go to [Step 8: Set up source and target](hyper-v-site-walkthrough-source-target.md)</span></span>
