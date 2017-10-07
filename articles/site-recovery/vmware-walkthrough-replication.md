---
title: "aaaSet se zásada replikace pro virtuální počítač VMware tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset si zásadu replikace při replikaci virtuálních počítačů VMware tooAzure úložiště"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 12870f3f98a4dd412221b817581403cd4bf7a76d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-tooazure"></a><span data-ttu-id="ed7aa-103">Krok 9: Nastavení zásad replikace pro virtuální počítač VMware tooAzure replikace</span><span class="sxs-lookup"><span data-stu-id="ed7aa-103">Step 9: Set up a replication policy for VMware VM replication tooAzure</span></span>


<span data-ttu-id="ed7aa-104">Tento článek popisuje, jak tooset si zásadu replikace při replikaci virtuálních počítačů VMware tooAzure pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-104">This article describes how tooset up a replication policy when you're replicating VMware VMs tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="ed7aa-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="ed7aa-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="ed7aa-106">Konfigurace zásad</span><span class="sxs-lookup"><span data-stu-id="ed7aa-106">Configure a policy</span></span>

<span data-ttu-id="ed7aa-107">Vám zajistí rychlý přehled videa, než začnete:</span><span class="sxs-lookup"><span data-stu-id="ed7aa-107">Get a quick video overview before you start:</span></span>
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. <span data-ttu-id="ed7aa-108">Klikněte na tlačítko toocreate novou zásadu replikace, **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-108">toocreate a new replication policy, click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="ed7aa-109">V **vytvořit zásadu replikace**, zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-109">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="ed7aa-110">V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-110">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="ed7aa-111">Tato hodnota určuje, jak často jsou vytvořeny body obnovení data.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-111">This value specifies how often data recovery points are created.</span></span> <span data-ttu-id="ed7aa-112">Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-112">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="ed7aa-113">V **uchování bodu obnovení**, určit, jak dlouho (v hodinách) pro každý bod obnovení je okno uchování hello.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-113">In **Recovery point retention**, specify (in hours) how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="ed7aa-114">Replikované virtuální počítače mohou být obnovena tooany bodu v okně.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-114">Replicated VMs can be recovered tooany point in a window.</span></span> <span data-ttu-id="ed7aa-115">Až too24 čas uchování je podporována pro počítače replikují toopremium úložiště a 72 hodin, než standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-115">Up too24 hours retention is supported for machines replicated toopremium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="ed7aa-116">V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-116">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="ed7aa-117">Klikněte na tlačítko **OK** toocreate hello zásad.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-117">Click **OK** toocreate hello policy.</span></span>

    ![Zásady replikace](./media/vmware-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="ed7aa-119">Když vytvoříte novou zásadu je přidružená automaticky hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-119">When you create a new policy it's automatically associated with hello configuration server.</span></span> <span data-ttu-id="ed7aa-120">Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-120">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="ed7aa-121">Například pokud hello zásady replikace je **rep zásad** bude hello navrácení služeb po obnovení zásad **rep. zásady navrácení**.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-121">For example, if hello replication policy is **rep-policy** then hello failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="ed7aa-122">Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.</span><span class="sxs-lookup"><span data-stu-id="ed7aa-122">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed7aa-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed7aa-123">Next steps</span></span>

<span data-ttu-id="ed7aa-124">Přejděte příliš[krok 10: instalace služby Mobility hello](vmware-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="ed7aa-124">Go too[Step 10: Install hello Mobility service](vmware-walkthrough-install-mobility.md)</span></span>
