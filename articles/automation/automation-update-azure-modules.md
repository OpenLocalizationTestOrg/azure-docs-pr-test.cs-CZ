---
title: "aaaUpdate Azure modulů ve službě Azure Automation | Microsoft Docs"
description: "Tento článek popisuje, jak můžete nyní aktualizovat společnými moduly prostředí Azure PowerShell, které jsou dostupné ve výchozím nastavení ve službě Azure Automation."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: afa404a643349f044e55be2280e435b575049dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-azure-powershell-modules-in-azure-automation"></a>Jak tooupdate modulů prostředí Azure PowerShell ve službě Azure Automation

Nejběžnější modulů prostředí Azure PowerShell Hello jsou dostupné ve výchozím nastavení v jednotlivých účtů Automation.  aktualizace tým Azure Hello hello Azure moduly pravidelně, tak v hello účet Automation jsme poskytnout způsob, jak vám tooupdate hello moduly v účtu hello Jakmile je nová verze dostupná z portálu, hello.  

Protože moduly jsou pravidelně aktualizovány skupinou hello produktu, změny můžou nastat s rutinami hello zahrnuty, což může mít negativní vliv na vaše sady runbook v závislosti na typu hello změny, jako je například přejmenování parametr nebo zcela místo začne rutiny. tooavoid sady runbook a hello, které mají vliv procesy jejich automatizovat, důrazně doporučujeme testování a ověření než budete pokračovat.  Pokud nemáte určené pro tento účel vyhrazený účet Automation, zvažte vytvoření jeden, mohli otestovat mnoha různých scénářů a permutací během vývoje hello sadu runbook, přidání tooiterative změnám jako aktualizace hello Moduly prostředí PowerShell.  Po ověření hello výsledky a jste použili potřebné změny, pokračujte koordinace hello migrace jakékoli sady runbook, který vyžaduje úpravu a provést aktualizaci hello, jak je popsáno níže v produkčním prostředí.     

## <a name="updating-azure-modules"></a>Aktualizace Azure moduly

1. V modulech hello je okno účtu Automation existuje možnost **moduly Azure aktualizace**.  Je vždy povolena.<br><br> ![Aktualizovat možnost moduly Azure v okně moduly](media/automation-update-azure-modules/automation-update-azure-modules-option.png)

2. Klikněte na tlačítko **moduly Azure aktualizace** a zobrazí se oznámení o potvrzení, která požaduje, pokud chcete, aby toocontinue.<br><br> ![Moduly Azure oznámení o aktualizaci](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)

3. Klikněte na tlačítko **Ano** a zahájí proces aktualizace modulu hello.  proces aktualizace Hello trvá o 15-20 minut tooupdate hello následující moduly:

  * Azure
  * Azure.Storage
  * AzureRm.Automation
  * AzureRm.Compute
  * AzureRm.Profile
  * AzureRm.Resources
  * AzureRm.Sql
  * AzureRm.Storage

    Pokud hello moduly jsou již až toodate, proces hello dokončí za několik sekund.  Po dokončení procesu aktualizace hello budete informováni.<br><br> ![Aktualizovat stav aktualizace moduly Azure](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> Služby Azure Automation bude používat nejnovější moduly hello ve vašem účtu Automation, při spuštění novou naplánovanou úlohu.    

Pokud používáte rutiny z těchto modulů prostředí Azure PowerShell ve vaší sady runbook toomanage Azure prostředky a potom se mají tooperform tento proces aktualizace každý měsíc nebo tak hello tooassure, zda máte nejnovější moduly.

## <a name="next-steps"></a>Další kroky

* toolearn informace o integrační moduly a jak toocreate vlastní moduly toofurther integrovat automatizace s jinými systémy, služby nebo řešení, najdete v části [moduly integrace](automation-integration-modules.md).

* Zvažte použití integrace zdroj ovládacího prvku [Githubu Enterprise](automation-scenario-source-control-integration-with-github-ent.md) nebo [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally spravovat a řídit verzích portfolio automatizace sady runbook a konfigurace.  
