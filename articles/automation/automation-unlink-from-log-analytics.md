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
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Postup zrušení propojení účtu Automation z pracovního prostoru analýzy protokolů

Služby Azure Automation se integruje s analýzy protokolů pro podporu nejen Proaktivní monitorování vaše úlohy sady runbook ve všech svých účtů Automation, ale je také nutný, pokud importujete následující řešení, které jsou závislé na analýzy protokolů:

* [Správa aktualizací](../operations-management-suite/oms-solution-update-management.md)
* [Sledování změn](../log-analytics/log-analytics-change-tracking.md)
* [Spuštění a zastavení virtuálních počítačů, která](automation-solution-vm-management.md)
 
Pokud se rozhodnete, že již nechcete integrovat analýzy protokolů účtu Automation, můžete zrušit propojení účtu přímo z portálu Azure.  Než budete pokračovat, bude nejprve nutné odebrat řešení již bylo zmíněno dříve, v opačném případě tento proces nebude možné pokračovat.  Přečtěte si téma konkrétní řešení, které jste importovali pochopit na kroky potřebné k jeho odebrání.  

Po odebrání těchto řešení, abyste mohli provést následující kroky zrušení propojení účtu Automation.

## <a name="unlink-workspace"></a>Zrušit propojení pracovního prostoru

1. Z portálu Azure otevřete účet Automation a v okně účtu Automation, v okně účtu vyberte **zrušit propojení prostoru**.<br><br> ![Zrušit propojení možnost pracovní prostor](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. V okně prostoru zrušit propojení, klikněte na **zrušit propojení worksapce**.<br><br> ![Zrušit propojení prostoru okno](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Zobrazí se výzva s dotazem, jestli chcete pokračovat.<br><br>
3. Zatímco Azure Automation se pokusí o zrušení propojení účtu pracovní prostor analýzy protokolů, můžete sledovat průběh v části **oznámení** z nabídky.

Pokud jste použili řešení pro správu aktualizací, Volitelně můžete odebrat následující položky, které již nejsou potřebné po odebrání řešení.

* Aktualizujte plány.  Každá bude mít názvy, které odpovídají nasazení aktualizací, které jste vytvořili)

* Pro toto řešení vytvořeny skupinám hybrid worker.  Každý bude mít název podobně jako na machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Pokud jste použili spuštění a zastavení virtuálních počítačů během počítačem nepracujete řešení, Volitelně můžete odebrat následující položky, které již nejsou potřebné po odebrání řešení.

* Spuštění a zastavení sady runbook plány virtuálních počítačů 
* Spuštění a zastavení sad runbook virtuálních počítačů
* Proměnné   

## <a name="next-steps"></a>Další kroky

Změna konfigurace účtu Automation k integraci s OMS analýzy protokolů, najdete v části [předávání zpráv o stavu úlohy a datové proudy úlohy z Automatizace analýzy protokolů (OMS)](automation-manage-send-joblogs-log-analytics.md). 