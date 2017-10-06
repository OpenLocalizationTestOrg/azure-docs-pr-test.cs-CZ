---
title: "aaaSet až do trezoru pro fyzický server replikace tooAzure pomocí Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset až tooAzure trezoru tooreplicate fyzických serverů pomocí Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 988928e3ece31116823f132cc39223fe44443468
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-tooazure"></a><span data-ttu-id="7fd1e-103">Krok 6: Nastavení trezoru pro tooAzure replikace fyzického serveru</span><span class="sxs-lookup"><span data-stu-id="7fd1e-103">Step 6: Set up a vault for physical server replication tooAzure</span></span>


<span data-ttu-id="7fd1e-104">Tento článek popisuje, jak tooset až do trezoru.</span><span class="sxs-lookup"><span data-stu-id="7fd1e-104">This article describes how tooset up a vault.</span></span> <span data-ttu-id="7fd1e-105">Vytvoření trezoru hello a určit, co chcete tooreplicate z vaší místní tooAzure umístění pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7fd1e-105">You create hello vault, and specify what you want tooreplicate from your on-premises location tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="7fd1e-106">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7fd1e-106">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="7fd1e-107">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="7fd1e-107">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a><span data-ttu-id="7fd1e-108">Vyberte cíl ochrany</span><span class="sxs-lookup"><span data-stu-id="7fd1e-108">Select a protection goal</span></span>

<span data-ttu-id="7fd1e-109">Vyberte, co chcete tooreplicate, a místo, kam chcete tooreplicate k.</span><span class="sxs-lookup"><span data-stu-id="7fd1e-109">Select what you want tooreplicate, and where you want tooreplicate to.</span></span>

1. <span data-ttu-id="7fd1e-110">Klikněte na tlačítko **trezory služeb zotavení** > trezoru.</span><span class="sxs-lookup"><span data-stu-id="7fd1e-110">Click **Recovery Services vaults** > vault.</span></span>
2. <span data-ttu-id="7fd1e-111">V hello prostředků nabídky, klikněte na **Site Recovery** > **Příprava infrastruktury** > **cíl ochrany**.</span><span class="sxs-lookup"><span data-stu-id="7fd1e-111">In hello Resource Menu, click **Site Recovery** > **Prepare Infrastructure** > **Protection goal**.</span></span>
3. <span data-ttu-id="7fd1e-112">V **cíl ochrany**, vyberte **tooAzure** > **není virtualizované/jiné**.</span><span class="sxs-lookup"><span data-stu-id="7fd1e-112">In **Protection goal**, select **tooAzure** > **Not virtualized/Other**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7fd1e-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7fd1e-113">Next steps</span></span>

<span data-ttu-id="7fd1e-114">Přejděte příliš[krok 7: nastavit zdroje a cíle](physical-walkthrough-source-target.md)</span><span class="sxs-lookup"><span data-stu-id="7fd1e-114">Go too[Step 7: Set up source and target](physical-walkthrough-source-target.md)</span></span>
