---
title: "aaaSet až do trezoru pro tooAzure replikace VMware pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset až do trezoru pro tooAzure replikace VMware pomocí Azure Site Recovery"
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
ms.openlocfilehash: 8a7755a6c9a3f55f241c615e425285bc4b782493
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-a-vault-for-vmware-replication-tooazure"></a><span data-ttu-id="cf5a5-103">Krok 7: Nastavení trezoru pro tooAzure replikace VMware</span><span class="sxs-lookup"><span data-stu-id="cf5a5-103">Step 7: Set up a vault for VMware replication tooAzure</span></span>


<span data-ttu-id="cf5a5-104">Tento článek popisuje, jak tooset až do trezoru a určit, co chcete tooreplicate z místního umístění, tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cf5a5-104">This article describes how tooset up a vault, and specify what you want tooreplicate from your on-premises location, tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="cf5a5-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="cf5a5-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="cf5a5-106">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="cf5a5-106">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="cf5a5-107">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="cf5a5-107">Select a protection goal</span></span>

<span data-ttu-id="cf5a5-108">Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.</span><span class="sxs-lookup"><span data-stu-id="cf5a5-108">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="cf5a5-109">Klikněte na tlačítko **trezory služeb zotavení** > trezoru.</span><span class="sxs-lookup"><span data-stu-id="cf5a5-109">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="cf5a5-110">V hello prostředků nabídky, klikněte na **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="cf5a5-110">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="cf5a5-111">V **cíl ochrany**, vyberte **tooAzure** > **Ano, s hypervisoru VMware vSphere**.</span><span class="sxs-lookup"><span data-stu-id="cf5a5-111">In **Protection goal**, select **tooAzure** > **Yes, with VMware vSphere Hypervisor**.</span></span>



## <a name="next-steps"></a><span data-ttu-id="cf5a5-112">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cf5a5-112">Next steps</span></span>

<span data-ttu-id="cf5a5-113">Přejděte příliš[krok 8: nastavit zdroje a cíle](vmware-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="cf5a5-113">Go too[Step 8: Set up source and target](vmware-walkthrough-source-target.md)</span></span>
