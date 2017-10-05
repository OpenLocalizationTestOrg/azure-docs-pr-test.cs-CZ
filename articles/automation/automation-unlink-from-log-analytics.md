---
title: "Zrušení propojení účtu Azure Automation z Log Analytics | Microsoft Docs"
description: "Tento článek obsahuje přehled postupu zrušení propojení účtu Azure Automation z pracovního prostoru OMS."
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
ms.openlocfilehash: 56b09c2cfc14813b5efcb364c580787fec1bf639
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a><span data-ttu-id="af138-103">Postup zrušení propojení účtu Automation z pracovního prostoru analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="af138-103">How to unlink your Automation account from a Log Analytics workspace</span></span>

<span data-ttu-id="af138-104">Služby Azure Automation se integruje s analýzy protokolů pro podporu nejen Proaktivní monitorování vaše úlohy sady runbook ve všech svých účtů Automation, ale je také nutný, pokud importujete následující řešení, které jsou závislé na analýzy protokolů:</span><span class="sxs-lookup"><span data-stu-id="af138-104">Azure Automation integrates with Log Analytics to not only support proactive monitoring of your runbook jobs across all of your Automation accounts, but is also required when you import the following solutions that are dependent on Log Analytics:</span></span>

* [<span data-ttu-id="af138-105">Správa aktualizací</span><span class="sxs-lookup"><span data-stu-id="af138-105">Update Management</span></span>](../operations-management-suite/oms-solution-update-management.md)
* [<span data-ttu-id="af138-106">Sledování změn</span><span class="sxs-lookup"><span data-stu-id="af138-106">Change Tracking</span></span>](../log-analytics/log-analytics-change-tracking.md)
* [<span data-ttu-id="af138-107">Spuštění a zastavení virtuálních počítačů, která</span><span class="sxs-lookup"><span data-stu-id="af138-107">Start/Stop VMs during off-hours</span></span>](automation-solution-vm-management.md)
 
<span data-ttu-id="af138-108">Pokud se rozhodnete, že již nechcete integrovat analýzy protokolů účtu Automation, můžete zrušit propojení účtu přímo z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="af138-108">If you decide you no longer wish to integrate your Automation account with Log Analytics, you can unlink your account directly from the Azure portal.</span></span>  <span data-ttu-id="af138-109">Než budete pokračovat, bude nejprve nutné odebrat řešení již bylo zmíněno dříve, v opačném případě tento proces nebude možné pokračovat.</span><span class="sxs-lookup"><span data-stu-id="af138-109">Before you proceed, you will first need to remove the solutions mentioned earlier, otherwise this process will be prevented from proceeding.</span></span>  <span data-ttu-id="af138-110">Přečtěte si téma konkrétní řešení, které jste importovali pochopit na kroky potřebné k jeho odebrání.</span><span class="sxs-lookup"><span data-stu-id="af138-110">Review the topic for the particular solution you have imported to understand the steps required to remove it.</span></span>  

<span data-ttu-id="af138-111">Po odebrání těchto řešení, abyste mohli provést následující kroky zrušení propojení účtu Automation.</span><span class="sxs-lookup"><span data-stu-id="af138-111">After you remove these solutions you can perform the following steps to unlink your Automation account.</span></span>

## <a name="unlink-workspace"></a><span data-ttu-id="af138-112">Zrušit propojení pracovního prostoru</span><span class="sxs-lookup"><span data-stu-id="af138-112">Unlink workspace</span></span>

1. <span data-ttu-id="af138-113">Z portálu Azure otevřete účet Automation a v okně účtu Automation, v okně účtu vyberte **zrušit propojení prostoru**.</span><span class="sxs-lookup"><span data-stu-id="af138-113">From the Azure portal, open your Automation account, and in the Automation account blade, in the account blade, select **Unlink workspace**.</span></span><br><br> <span data-ttu-id="af138-114">![Zrušit propojení možnost pracovní prostor](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span><span class="sxs-lookup"><span data-stu-id="af138-114">![Unlink workspace option](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)</span></span><br><br>  
2. <span data-ttu-id="af138-115">V okně prostoru zrušit propojení, klikněte na **zrušit propojení worksapce**.</span><span class="sxs-lookup"><span data-stu-id="af138-115">On the Unlink workspace blade, click **Unlink worksapce**.</span></span><br><br> <span data-ttu-id="af138-116">![Zrušit propojení prostoru okno](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span><span class="sxs-lookup"><span data-stu-id="af138-116">![Unlink workspace blade](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).</span></span><br><br>  <span data-ttu-id="af138-117">Zobrazí se výzva s dotazem, jestli chcete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="af138-117">You will receive a prompt verifying you wish to proceed.</span></span><br><br>
3. <span data-ttu-id="af138-118">Zatímco Azure Automation se pokusí o zrušení propojení účtu pracovní prostor analýzy protokolů, můžete sledovat průběh v části **oznámení** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="af138-118">While Azure Automation attempts to unlink the account your Log Analytics workspace, you can track the progress under **Notifications** from the menu.</span></span>

<span data-ttu-id="af138-119">Pokud jste použili řešení pro správu aktualizací, Volitelně můžete odebrat následující položky, které již nejsou potřebné po odebrání řešení.</span><span class="sxs-lookup"><span data-stu-id="af138-119">If you used the Update Management solution, optionally you may want to remove the following items that are no longer needed after you remove the solution.</span></span>

* <span data-ttu-id="af138-120">Aktualizujte plány.</span><span class="sxs-lookup"><span data-stu-id="af138-120">Update schedules.</span></span>  <span data-ttu-id="af138-121">Každá bude mít názvy, které odpovídají nasazení aktualizací, které jste vytvořili)</span><span class="sxs-lookup"><span data-stu-id="af138-121">Each will have names that match the update deployments you created)</span></span>

* <span data-ttu-id="af138-122">Pro toto řešení vytvořeny skupinám hybrid worker.</span><span class="sxs-lookup"><span data-stu-id="af138-122">Hybrid worker groups created for the solution.</span></span>  <span data-ttu-id="af138-123">Každý bude mít název podobně jako na machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).</span><span class="sxs-lookup"><span data-stu-id="af138-123">Each will be named similarly to  machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8).</span></span>

<span data-ttu-id="af138-124">Pokud jste použili spuštění a zastavení virtuálních počítačů během počítačem nepracujete řešení, Volitelně můžete odebrat následující položky, které již nejsou potřebné po odebrání řešení.</span><span class="sxs-lookup"><span data-stu-id="af138-124">If you used the Start/Stop VMs during off-hours solution, optionally you may want to remove the following items that are no longer needed after you remove the solution.</span></span>

* <span data-ttu-id="af138-125">Spuštění a zastavení sady runbook plány virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="af138-125">Start and stop VM runbook schedules</span></span> 
* <span data-ttu-id="af138-126">Spuštění a zastavení sad runbook virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="af138-126">Start and stop VM runbooks</span></span>
* <span data-ttu-id="af138-127">Proměnné</span><span class="sxs-lookup"><span data-stu-id="af138-127">Variables</span></span>   

## <a name="next-steps"></a><span data-ttu-id="af138-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af138-128">Next steps</span></span>

<span data-ttu-id="af138-129">Změna konfigurace účtu Automation k integraci s OMS analýzy protokolů, najdete v části [předávání zpráv o stavu úlohy a datové proudy úlohy z Automatizace analýzy protokolů (OMS)](automation-manage-send-joblogs-log-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="af138-129">To reconfigure your Automation account to integrate with OMS Log Analytics, see [Forward job status and job streams from Automation to Log Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md).</span></span> 