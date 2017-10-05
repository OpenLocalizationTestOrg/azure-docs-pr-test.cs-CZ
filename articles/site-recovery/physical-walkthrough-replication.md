---
title: "Nastavení zásad replikace pro fyzický server replikaci do Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky, které je třeba nastavit zásady replikace při replikaci místní fyzických serverů do úložiště Azure pomocí služby Azure Site Recovery"
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
ms.openlocfilehash: 9ce23382001b54e7b9b7a58b8dd9aa61b400826d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-to-azure"></a><span data-ttu-id="c2fe4-103">Krok 8: Nastavení zásad replikace pro fyzický server replikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-103">Step 8: Set up a replication policy for physical server replication to Azure</span></span>


<span data-ttu-id="c2fe4-104">Tento článek popisuje, jak nastavit zásadu replikace, když replikujete fyzických serverů Windows nebo Linuxem do Azure pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-104">This article describes how to set up a replication policy when you're replicating Windows/Linux physical servers to Azure using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>


<span data-ttu-id="c2fe4-105">POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c2fe4-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="configure-a-policy"></a><span data-ttu-id="c2fe4-106">Konfigurace zásad</span><span class="sxs-lookup"><span data-stu-id="c2fe4-106">Configure a policy</span></span>

1. <span data-ttu-id="c2fe4-107">Klikněte na tlačítko **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-107">Click **Site Recovery infrastructure** > **Replication Policies** > **+Replication Policy**.</span></span>
2. <span data-ttu-id="c2fe4-108">V **vytvořit zásadu replikace**, zadejte název zásady.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-108">In **Create replication policy**, specify a policy name.</span></span>
3. <span data-ttu-id="c2fe4-109">V části **Prahová hodnota cíle bodu obnovení (RPO)** zadejte omezení RPO.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-109">In **RPO threshold**, specify the RPO limit.</span></span> <span data-ttu-id="c2fe4-110">Tato hodnota určuje, jak často jsou vytvořeny body obnovení data.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-110">This value indicates how often data recovery points are created.</span></span> <span data-ttu-id="c2fe4-111">Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-111">An alert is generated if continuous replication exceeds this limit.</span></span>
4. <span data-ttu-id="c2fe4-112">V **uchování bodu obnovení**, zadejte (v hodinách), jak dlouho je okno uchování pro každý bod obnovení.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-112">In **Recovery point retention**, specify (in hours) how long the retention window is for each recovery point.</span></span> <span data-ttu-id="c2fe4-113">Replikované virtuální počítače můžete obnovit do libovolného bodu v okně.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-113">Replicated VMs can be recovered to any point in a window.</span></span> <span data-ttu-id="c2fe4-114">Až 24 hodin uchování je podporována pro počítače, na které se replikují do úložiště úrovně premium a 72 hodin, než standardní úložiště.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-114">Up to 24 hours retention is supported for machines replicated to premium storage, and 72 hours for standard storage.</span></span>
5. <span data-ttu-id="c2fe4-115">V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-115">In **App-consistent snapshot frequency**, specify how often (in minutes) recovery points containing application-consistent snapshots will be created.</span></span> <span data-ttu-id="c2fe4-116">Klikněte na tlačítko **OK** k vytvoření zásad.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-116">Click **OK** to create the policy.</span></span>

    ![Zásady replikace](./media/physical-walkthrough-replication/gs-replication2.png)
8. <span data-ttu-id="c2fe4-118">Když vytvoříte novou zásadu je automaticky přiřazen s konfiguračním serverem.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-118">When you create a new policy it's automatically associated with the configuration server.</span></span> <span data-ttu-id="c2fe4-119">Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-119">By default, a matching policy is automatically created for failback.</span></span> <span data-ttu-id="c2fe4-120">Například, pokud je zásada replikace **rep zásad** bude navrácení služeb po obnovení zásad **rep. zásady navrácení**.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-120">For example, if the replication policy is **rep-policy** then the failback policy will be **rep-policy-failback**.</span></span> <span data-ttu-id="c2fe4-121">Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.</span><span class="sxs-lookup"><span data-stu-id="c2fe4-121">This policy isn't used until you initiate a failback from Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2fe4-122">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2fe4-122">Next steps</span></span>

<span data-ttu-id="c2fe4-123">Přejděte na [krok 9: instalace služby Mobility](physical-walkthrough-install-mobility.md)</span><span class="sxs-lookup"><span data-stu-id="c2fe4-123">Go to [Step 9: Install the Mobility service](physical-walkthrough-install-mobility.md)</span></span>
