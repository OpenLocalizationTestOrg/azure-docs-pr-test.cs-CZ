---
title: "aaaSet až úložiště pro Hyper-V tooAzure replikace (bez System Center VMM) pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset až do trezoru pro tooAzure replikace technologie Hyper-V pomocí Azure Site Recovery"
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
ms.openlocfilehash: e3ef8758faab36d19d0968d98a23105bed7830f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a><span data-ttu-id="fefa0-103">Krok 7: Nastavení trezoru pro replikaci technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="fefa0-103">Step 7: Set up a vault for Hyper-V replication</span></span>

<span data-ttu-id="fefa0-104">Tento článek popisuje, jak tooset až do trezoru a určit, co chcete tooreplicate z místního umístění, tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fefa0-104">This article describes how tooset up a vault, and specify what you want tooreplicate from your on-premises location, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="fefa0-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="fefa0-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="fefa0-106">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="fefa0-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a><span data-ttu-id="fefa0-107">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="fefa0-107">Select a protection goal</span></span>

<span data-ttu-id="fefa0-108">Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.</span><span class="sxs-lookup"><span data-stu-id="fefa0-108">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="fefa0-109">Klikněte na tlačítko **trezory služeb zotavení** > trezoru.</span><span class="sxs-lookup"><span data-stu-id="fefa0-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="fefa0-110">V hello prostředků nabídky, klikněte na **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="fefa0-110">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="fefa0-111">V **cíl ochrany**, vyberte **tooAzure** > **Ano, s technologií Hyper-V**.</span><span class="sxs-lookup"><span data-stu-id="fefa0-111">In **Protection goal**, select **tooAzure** > **Yes, with Hyper-V**.</span></span> <span data-ttu-id="fefa0-112">Vyberte **ne** tooconfirm nepoužíváte VMM.</span><span class="sxs-lookup"><span data-stu-id="fefa0-112">Select **No** tooconfirm you're not using VMM.</span></span> 

    ![Zvolte cíle.](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a><span data-ttu-id="fefa0-114">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fefa0-114">Next steps</span></span>

<span data-ttu-id="fefa0-115">Přejděte příliš[krok 8: nastavit zdroje a cíle](hyper-v-site-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="fefa0-115">Go too[Step 8: Set up source and target](hyper-v-site-walkthrough-source-target.md)</span></span>
