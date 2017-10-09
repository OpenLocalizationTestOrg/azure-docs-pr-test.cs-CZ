---
title: "aaaUser Azure Automation Runbook akce zahájená v analýzy protokolů | Microsoft Docs"
description: "Tento článek popisuje, jak toorun runbook služby automatizace z analýzy protokolů hledání výsledek na vyžádání."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: 53c25431572babd5fd54bf964e4683077e2a4c2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Akce s Runbook služby automatizace z výsledků na vyhledávacím protokolu analýzy protokolů

Ve výsledku hledání protokolu v Azure Log Analytics můžete nyní vybrat **akce** toorun runbook služby automatizace.  Hello runbook lze tooremediate hello problém trvat některých jiných akcí, jako je například shromažďovat informace o odstraňování potíží, e-mailovou zprávu nebo vytvořit žádost o služby. 

## <a name="components-and-features-used"></a>Použité komponenty a funkce
* [Účet Azure Automation.](../automation/automation-offering-get-started.md)
* [Pracovní prostor analýzy protokolů](../log-analytics/log-analytics-overview.md)

## <a name="tooinitiate-runbook-from-log-search"></a>runbook tooinitiate z protokolu hledání

Akce tootake na události a spuštění sady runbook z výsledků hledání protokolu, začněte vytvořením hledání protokolů a z výsledků hello můžete vyvolat runbook na vyžádání.  Toho lze dosáhnout z hello protokolu vyhledávací funkce hello Azure nebo [portálu OMS](../log-analytics/log-analytics-log-searches.md).  V tomto příkladu jsme vyhledávání protokolu z hello portál Azure s základní ukázka této funkce.

1. Hello portálu Azure, v nabídce centra hello klikněte na možnost **další služby** a vyberte **analýzy protokolů**.  
2. V okně Log Analytics hello, vyberte pracovní prostor analýzy protokolů a v okně hello prostoru vyberte **hledání protokolů**.  
3. V okně hello protokolu vyhledávání hledání protokolu.  
4. Z výsledků hledání protokolu hello, klikněte na levé straně toohello elipsy hello jednoho hello polí a z místní hello, vyberte **provést akci pro**.<br><br> ![Vyberte akci trvat od výsledek hledání](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
5. V okně Akce hello provést, vyberte **spuštění sady runbook**a na hello **spuštění sady runbook** okně vyberete toorun sady runbook.  Všechny sady runbook můžete vybrat v hello účtu Automation, který je pracovní prostor analýzy protokolů propojené toohello.  Vezměte na vědomí následující hello:

    * Sady Runbook jsou uspořádané podle značky
    * Hodnoty vstupní parametr Runbooku je možné vybrat přímo z pole hello výsledek hledání hello.  Rozevíracím seznamu se zobrazí všechna dostupná pole hello z hello výsledek tooselect ze zobrazení.  
    * Můžete zvolit toorun hello runbook na [hybridní pracovní proces runbooku](../automation/automation-hybrid-runbook-worker.md) nainstalovaného na hello počítači, který má problém hello, pokud máte odpovídající skupinu hybridní pracovní proces Runbooku, který obsahuje pouze tento počítač jako člena.  Pokud název hello skupinu hybridních pracovních procesů hello odpovídá názvu hello hello počítač, který je ve výsledku hledání hello protokolu, bude automaticky vybrána skupina hello.    

6. Po kliknutí na tlačítko **spustit**, otevře se okno úlohy runbooku hello tooallow jste tooreview hello stav hello úlohy.   

Pokud vyberete sady runbook, který byl nakonfigurovaný toobe [volat z upozornění na analýzy protokolů](../automation/automation-invoke-runbook-from-omsla-alert.md), má vstupní parametr s názvem **WebhookData** tedy **objekt** typu.  Pokud hello vstupní parametr je povinný, je třeba toopass hello vyhledávání výsledky toohello runbook proto řetězec ve formátu JSON hello můžete převést na typ objektu, který umožňuje toofilter na konkrétní položky, které budete odkazovat v aktivity sady runbook.  To uděláte tak, že vyberete **hledání výsledek (objekt)** z rozevíracího seznamu hello.<br><br> ![Vyberte datový objekt Webhooku pro parametr sady runbook](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Další kroky

* Zkontrolujte hello [analýzy protokolů protokolu vyhledávání odkaz](log-analytics-search-reference.md) tooview všechny hello vyhledávání polí a vlastností, které jsou k dispozici v analýzy protokolů.
* jak automaticky, zkontrolujte tooinvoke runbook služby automatizace toolearn [volání runbook služby automatizace Azure z výstrahu analýzy protokolů OMS](../automation/automation-invoke-runbook-from-omsla-alert.md).  
