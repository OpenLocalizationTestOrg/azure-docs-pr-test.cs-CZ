---
title: "aaaUnlink účet Azure Automation z Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje přehled o tom, jak toounlink Azure Automation účet z pracovního prostoru OMS."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 02/07/2017
ms.author: magoedte
ms.openlocfilehash: 3ec0745673f6a872538d33023a7a1d2bb1d18ec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toounlink-your-automation-account-from-a-log-analytics-workspace"></a><span data-ttu-id="74122-103">Jak toounlink vaše automatizace účet z pracovního prostoru analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="74122-103">How toounlink your Automation account from a Log Analytics workspace</span></span>

<span data-ttu-id="74122-104">Služby Azure Automation se integruje s analýzy protokolů toonot pouze podporu Proaktivní monitorování vaše úlohy sady runbook ve všech svých účtů Automation, ale je také nutný, pokud importujete hello následující řešení, které jsou závislé na analýzy protokolů:</span><span class="sxs-lookup"><span data-stu-id="74122-104">Azure Automation integrates with Log Analytics toonot only support proactive monitoring of your runbook jobs across all of your Automation accounts, but is also required when you import hello following solutions that are dependent on Log Analytics:</span></span>

* [<span data-ttu-id="74122-105">Správa aktualizací</span><span class="sxs-lookup"><span data-stu-id="74122-105">Update Management</span></span>](../operations-management-suite/oms-solution-update-management.md)
* [<span data-ttu-id="74122-106">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="74122-106">Change Tracking</span></span>](../log-analytics/log-analytics-change-tracking.md)
* [<span data-ttu-id="74122-107">Spuštění a zastavení virtuálních počítačů, která</span><span class="sxs-lookup"><span data-stu-id="74122-107">Start/Stop VMs during off-hours</span></span>](automation-solution-vm-management.md)
 
<span data-ttu-id="74122-108">Pokud se rozhodnete, že už nechcete toointegrate účtu Automation s analýzy protokolů, můžete zrušit propojení účtu přímo z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="74122-108">If you decide you no longer wish toointegrate your Automation account with Log Analytics, you can unlink your account directly from hello Azure portal.</span></span>  <span data-ttu-id="74122-109">Než budete pokračovat, musíte nejdřív řešení hello tooremove již bylo zmíněno dříve, v opačném případě tento proces nebude možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="74122-109">Before you proceed, you will first need tooremove hello solutions mentioned earlier, otherwise this process will be prevented from proceeding.</span></span>  <span data-ttu-id="74122-110">Zkontrolujte hello téma pro konkrétní řešení hello jste importovali toounderstand hello kroky tooremove ho.</span><span class="sxs-lookup"><span data-stu-id="74122-110">Review hello topic for hello particular solution you have imported toounderstand hello steps required tooremove it.</span></span>  

<span data-ttu-id="74122-111">Po odebrání těchto řešení můžete provést následující kroky toounlink hello účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="74122-111">After you remove these solutions you can perform hello following steps toounlink your Automation account.</span></span>

## <a name="unlink-workspace"></a><span data-ttu-id="74122-112">Zrušit propojení pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="74122-112">Unlink workspace</span></span>

1. <span data-ttu-id="74122-113">Z hello portálu Azure otevřete účet Automation a v okně účtu Automation hello, v okně účtu hello vyberte **zrušit propojení prostoru**.</span><span class="sxs-lookup"><span data-stu-id="74122-113">From hello Azure portal, open your Automation account, and in hello Automation account blade, in hello account blade, select **Unlink workspace**.</span></span><br><br> <span data-ttu-id="74122-114">![Zrušit propojení možnost pracovní prostor](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span><span class="sxs-lookup"><span data-stu-id="74122-114">![Unlink workspace option](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span></span><br><br>  
2. <span data-ttu-id="74122-115">V okně prostoru hello zrušit propojení, klikněte na tlačítko **zrušit propojení worksapce**.</span><span class="sxs-lookup"><span data-stu-id="74122-115">On hello Unlink workspace blade, click **Unlink worksapce**.</span></span><br><br> <span data-ttu-id="74122-116">![Zrušit propojení prostoru okno](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span><span class="sxs-lookup"><span data-stu-id="74122-116">![Unlink workspace blade](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span></span><br><br>  <span data-ttu-id="74122-117">Zobrazí se výzva ověření, že chcete tooproceed.</span><span class="sxs-lookup"><span data-stu-id="74122-117">You will receive a prompt verifying you wish tooproceed.</span></span><br><br>
3. <span data-ttu-id="74122-118">Zatímco Azure Automation pokusí toounlink hello účet pracovní prostor analýzy protokolů, můžete sledovat průběh hello pod **oznámení** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="74122-118">While Azure Automation attempts toounlink hello account your Log Analytics workspace, you can track hello progress under **Notifications** from hello menu.</span></span>

<span data-ttu-id="74122-119">Pokud jste použili řešení pro správu aktualizací hello, Volitelně můžete tooremove hello následující položky, které již nejsou potřebné po odebrání hello řešení.</span><span class="sxs-lookup"><span data-stu-id="74122-119">If you used hello Update Management solution, optionally you may want tooremove hello following items that are no longer needed after you remove hello solution.</span></span>

* <span data-ttu-id="74122-120">Aktualizujte plány.</span><span class="sxs-lookup"><span data-stu-id="74122-120">Update schedules.</span></span>  <span data-ttu-id="74122-121">Každá bude mít názvy, které odpovídají hello nasazení aktualizací, které jste vytvořili)</span><span class="sxs-lookup"><span data-stu-id="74122-121">Each will have names that match hello update deployments you created)</span></span>

* <span data-ttu-id="74122-122">Vytvořit pro řešení hello skupinám hybrid worker.</span><span class="sxs-lookup"><span data-stu-id="74122-122">Hybrid worker groups created for hello solution.</span></span>  <span data-ttu-id="74122-123">Každý budou pojmenované podobně příliš machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).</span><span class="sxs-lookup"><span data-stu-id="74122-123">Each will be named similarly too machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).</span></span>

<span data-ttu-id="74122-124">Pokud jste použili hello spuštění a zastavení virtuálních počítačů během počítačem nepracujete řešení, Volitelně můžete tooremove hello následující položky, které již nejsou potřebné po odebrání hello řešení.</span><span class="sxs-lookup"><span data-stu-id="74122-124">If you used hello Start/Stop VMs during off-hours solution, optionally you may want tooremove hello following items that are no longer needed after you remove hello solution.</span></span>

* <span data-ttu-id="74122-125">Spuštění a zastavení sady runbook plány virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="74122-125">Start and stop VM runbook schedules</span></span> 
* <span data-ttu-id="74122-126">Spuštění a zastavení sad runbook virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="74122-126">Start and stop VM runbooks</span></span>
* <span data-ttu-id="74122-127">Proměnné</span><span class="sxs-lookup"><span data-stu-id="74122-127">Variables</span></span>   

## <a name="next-steps"></a><span data-ttu-id="74122-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="74122-128">Next steps</span></span>

<span data-ttu-id="74122-129">tooreconfigure vaše toointegrate účtu automatizace s OMS analýzy protokolů, najdete v části [předávání zpráv o stavu úlohy a datové proudy úlohy z automatizace tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="74122-129">tooreconfigure your Automation account toointegrate with OMS Log Analytics, see [Forward job status and job streams from Automation tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span></span> 
