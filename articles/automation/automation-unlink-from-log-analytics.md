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
# <a name="how-toounlink-your-automation-account-from-a-log-analytics-workspace"></a>Jak toounlink vaše automatizace účet z pracovního prostoru analýzy protokolů

Služby Azure Automation se integruje s analýzy protokolů toonot pouze podporu Proaktivní monitorování vaše úlohy sady runbook ve všech svých účtů Automation, ale je také nutný, pokud importujete hello následující řešení, které jsou závislé na analýzy protokolů:

* [Správa aktualizací](../operations-management-suite/oms-solution-update-management.md)
* [Sledování změn](../log-analytics/log-analytics-change-tracking.md)
* [Spuštění a zastavení virtuálních počítačů, která](automation-solution-vm-management.md)
 
Pokud se rozhodnete, že už nechcete toointegrate účtu Automation s analýzy protokolů, můžete zrušit propojení účtu přímo z hello portálu Azure.  Než budete pokračovat, musíte nejdřív řešení hello tooremove již bylo zmíněno dříve, v opačném případě tento proces nebude možné pokračovat.  Zkontrolujte hello téma pro konkrétní řešení hello jste importovali toounderstand hello kroky tooremove ho.  

Po odebrání těchto řešení můžete provést následující kroky toounlink hello účtu Automation.

## <a name="unlink-workspace"></a>Zrušit propojení pracovního prostoru

1. Z hello portálu Azure otevřete účet Automation a v okně účtu Automation hello, v okně účtu hello vyberte **zrušit propojení prostoru**.<br><br> ![Zrušit propojení možnost pracovní prostor](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. V okně prostoru hello zrušit propojení, klikněte na tlačítko **zrušit propojení worksapce**.<br><br> ![Zrušit propojení prostoru okno](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Zobrazí se výzva ověření, že chcete tooproceed.<br><br>
3. Zatímco Azure Automation pokusí toounlink hello účet pracovní prostor analýzy protokolů, můžete sledovat průběh hello pod **oznámení** nabídce hello.

Pokud jste použili řešení pro správu aktualizací hello, Volitelně můžete tooremove hello následující položky, které již nejsou potřebné po odebrání hello řešení.

* Aktualizujte plány.  Každá bude mít názvy, které odpovídají hello nasazení aktualizací, které jste vytvořili)

* Vytvořit pro řešení hello skupinám hybrid worker.  Každý budou pojmenované podobně příliš machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8).

Pokud jste použili hello spuštění a zastavení virtuálních počítačů během počítačem nepracujete řešení, Volitelně můžete tooremove hello následující položky, které již nejsou potřebné po odebrání hello řešení.

* Spuštění a zastavení sady runbook plány virtuálních počítačů 
* Spuštění a zastavení sad runbook virtuálních počítačů
* Proměnné   

## <a name="next-steps"></a>Další kroky

tooreconfigure vaše toointegrate účtu automatizace s OMS analýzy protokolů, najdete v části [předávání zpráv o stavu úlohy a datové proudy úlohy z automatizace tooLog Analytics (OMS)](automation-manage-send-joblogs-log-analytics.md). 
