---
title: "aaaSet se zásada replikace pro fyzický server tooAzure replikace s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello potřebujete tooset si zásadu replikace, pokud replikace místní fyzických serverů tooAzure úložiště pomocí služby Azure Site Recovery hello"
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
ms.openlocfilehash: d6bdeeffc24ffc8eaba24311f7c76452edb65648
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-tooazure"></a><span data-ttu-id="7d596-103">Krok 8: Nastavení zásada replikace pro replikační tooAzure fyzického serveru</span><span class="sxs-lookup"><span data-stu-id="7d596-103">Step 8: Set up a replication policy for physical server replication tooAzure</span></span>


<span data-ttu-id="7d596-104">Tento článek popisuje, jak tooset si zásadu replikace při replikaci tooAzure fyzických serverů Windows nebo Linuxem pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7d596-104">This article describes how tooset up a replication policy when you're replicating Windows/Linux physical servers tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>


<span data-ttu-id="7d596-105">POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7d596-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="7d596-106">Konfigurace zásad</span><span class="sxs-lookup"><span data-stu-id="7d596-106">Configure a policy</span></span>

1. <span data-ttu-id="7d596-107">Klikněte na tlačítko **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="7d596-107">Click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="7d596-108">V **vytvořit zásadu replikace**, zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="7d596-108">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="7d596-109">V **prahovou hodnotu RPO**, zadejte limit hello plánovaný bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="7d596-109">In **RPO threshold**, specify hello RPO limit.</span></span> <span data-ttu-id="7d596-110">Tato hodnota určuje, jak často jsou vytvořeny body obnovení data.</span><span class="sxs-lookup"><span data-stu-id="7d596-110">This value indicates how often data recovery points are created.</span></span> <span data-ttu-id="7d596-111">Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.</span><span class="sxs-lookup"><span data-stu-id="7d596-111">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="7d596-112">V **uchování bodu obnovení**, určit, jak dlouho (v hodinách) pro každý bod obnovení je okno uchování hello.</span><span class="sxs-lookup"><span data-stu-id="7d596-112">In **Recovery point retention**, specify (in hours) how long hello retention window is for each recovery point.</span></span> <span data-ttu-id="7d596-113">Replikované virtuální počítače mohou být obnovena tooany bodu v okně.</span><span class="sxs-lookup"><span data-stu-id="7d596-113">Replicated VMs can be recovered tooany point in a window.</span></span> <span data-ttu-id="7d596-114">Až too24 čas uchování je podporována pro počítače replikují toopremium úložiště a 72 hodin, než standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="7d596-114">Up too24 hours retention is supported for machines replicated toopremium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="7d596-115">V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="7d596-115">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="7d596-116">Klikněte na tlačítko **OK** toocreate hello zásad.</span><span class="sxs-lookup"><span data-stu-id="7d596-116">Click **OK** toocreate hello policy.</span></span>

    ![Zásady replikace](./media/physical-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="7d596-118">Když vytvoříte novou zásadu je přidružená automaticky hello konfigurační server.</span><span class="sxs-lookup"><span data-stu-id="7d596-118">When you create a new policy it's automatically associated with hello configuration server.</span></span> <span data-ttu-id="7d596-119">Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady.</span><span class="sxs-lookup"><span data-stu-id="7d596-119">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="7d596-120">Například pokud hello zásady replikace je **rep zásad** bude hello navrácení služeb po obnovení zásad **rep. zásady navrácení**.</span><span class="sxs-lookup"><span data-stu-id="7d596-120">For example, if hello replication policy is **rep-policy** then hello failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="7d596-121">Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.</span><span class="sxs-lookup"><span data-stu-id="7d596-121">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d596-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7d596-122">Next steps</span></span>

<span data-ttu-id="7d596-123">Přejděte příliš[krok 9: instalace služby Mobility hello](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="7d596-123">Go too[Step 9: Install hello Mobility service](physical-walkthrough-install-mobility.md)</span></span>
