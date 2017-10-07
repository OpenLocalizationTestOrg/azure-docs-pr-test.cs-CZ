---
title: "aaaAlert správy v aplikaci Microsoft monitoring produkty | Microsoft Docs"
description: "Výstraha naznačuje některé problém, který vyžaduje pozornost správce.  Tento článek popisuje hello rozdíly v tom, jak jsou výstrahy vytvořit a spravovat v System Center Operations Manager (SCOM) a analýzy protokolů a obsahuje osvědčené postupy v využití hello dva produkty pro správu výstrah strategie hybridní."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6572c3f8-78ca-4fa9-8fe1-d0b488590788
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 526a3d92f3b6a5d6dec2f3979a934cc94c13f2e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-alerts-with-microsoft-monitoring"></a>Správa výstrah s monitorování Microsoft
Výstraha naznačuje některé problém, který vyžaduje pozornost správce.  Existují významné rozdíly mezi System Center Operations Manager (SCOM) a analýzy protokolů v Operations Management Suite (OMS) z hlediska vytváření výstrah, jak jsou spravovaná a analyzovat a jak jsou oznámit, že kritický problém byl zjištěna.

## <a name="alerts-in-operations-manager"></a>Výstrahy v nástroji Operations Manager
Výstrahy v aplikaci SCOM jsou generovány podle jednotlivých pravidel nebo monitorování tooindicate určitého problému.  Monitorování mohou generovat výstrahu při vstupuje do chybového stavu, zatímco pravidla mohou generovat výstrahy tooindicate některé důležité problém, který není přímo související toohello stav spravovaného objektu.  Sady Management Pack zahrnují řadu pracovních postupů, které vytvářet výstrahy pro hello aplikace nebo služba, která spravují.  Součástí procesu hello konfigurace nové sady management pack je je ladění tooensure nepřijímáte nadměrného výstrahy pro problémy, které nejsou považujete za důležité.

![Zobrazení výstrah SCOM](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM poskytuje kompletní správu výstrah s výstrahami, které mají status, která lze změnit správci, jak fungují tooresolve hello problém.  Když hello problém byl vyřešen, nastaví správce hello hello výstrahy tooclosed, po kterém se nebude zobrazovat v zobrazeních zobrazení aktivní výstrahy.  Výstrahy, které jsou generované z monitorování lze automaticky vyřešit při návratu monitorování hello tooa stavu v pořádku.

![Stav výstrahy](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Výstrahy v analýzy protokolů
Výstrahy v analýzy protokolů je vytvořen z protokolu dotazu, který se automaticky spouští v pravidelných intervalech.  Pravidlo výstrahy můžete vytvořit z jakékoli protokolu dotazu.  Pokud hello dotaz vrátí výsledky, které odpovídají kritériím hello, které zadáte, se vytvoří výstraha.  To může být konkrétní dotaz, který vytvoří výstrahu, pokud je zjištěna určitá událost, nebo můžete použít obecnější dotaz, hledá všechny událost chyby související s tooa konkrétní aplikace.

Výstrahy Analytics protokolu se zapisují toohello OMS úložiště jako událost a můžete načíst pomocí dotazu protokolu.  Nemají stav jako SCOM události tak, aby můžete určit, když hello problém byl vyřešen.

![Výstraha OMS](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Při SCOM slouží jako zdroj dat pro analýzy protokolů, SCOM výstrahy jsou zapsány toohello OMS úložiště, jako jsou vytvořit a upravit.  

![Výstraha SCOM](media/operations-management-suite-monitoring-alerts/scom-alert.png)

Hello [řešení pro správu výstrah](http://technet.microsoft.com/library/mt484092.aspx) poskytuje souhrn aktivní výstrahy a několik běžných dotazů tooretrieve různé sady výstrahy.  To poskytuje efektivnější analýzu upozornění než sestavu v aplikaci SCOM.  Můžete přejít na nižší úroveň z hello souhrny toodetailed dat a vytvořit ad hoc dotazy tooretrieve různé sady výstrahy.

![Řešení pro správu výstrah](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Oznámení
Oznámení v aplikaci SCOM odesílat e-mailu nebo textu v odpovědi tooalerts, které odpovídají konkrétním kritériím.  Můžete vytvářet odběry různých oznámení, které mají jiné osoby oznámení podle kritérií, jako například objekt hello monitorovány, hello závažnost výstrahy hello, hello druh problému, který zjistit nebo hello času dne.

Několik předplatných může být použité tooimplement strategie dokončení oznámení pro velký počet sad management Pack.

![Výstrahy SCOM](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Analýzy protokolů vás může upozornit, prostřednictvím e-mailu že výstraha byla vytvořena nastavením akce oznámení e-mailu na každém [pravidlo výstrahy](http://technet.microsoft.com/library/mt614775.aspx).  Nemá schopnost hello SCOM hello přihlášení k odběru výstrah toomultiple s jedním pravidlem.  Budete také potřebovat toocreate vlastní pravidla výstrah od OMS neposkytuje žádné předem nakonfigurované.

![Výstrahy Log Analytics](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Nelze spravovat zcela SCOM výstrahy v analýzy protokolů ale vzhledem k tomu, že je lze upravit pouze v hello konzole Operations Console.  Analýzy protokolů je užitečné jako součást výstrahy správu zpracovat, když pro poskytování nástrojů pro analýzu této SCOM samostatně nemá.

## <a name="alert-remediation"></a>Výstrahy nápravy
[Náprava](http://technet.microsoft.com/library/mt614775.aspx) odkazuje tooan pokus o tooautomatically správné hello problém identifikovaný výstrahu.

SCOM umožňuje toorun diagnostiky a obnovení v odpovědi tooa monitorování zadávání stav není v pořádku.  K tomu dochází souběžných toohello monitorování, vytváření hello výstrahy.  Diagnostiky a obnovení jsou obvykle implementovány jako skriptu, který běží na agentovi hello.  Toogather diagnostiky pokusy o další informace o hello zjištěn problém při obnovení pokusí toocorrect hello problém.

Analýzy protokolů vám umožní toostart [runbook automatizace Azure](https://azure.microsoft.com/documentation/services/automation/) nebo volat webhook, jehož v odpovědi tooa analýzy protokolů výstrahy.  Sady Runbook může obsahovat komplexní logiku implementována v prostředí PowerShell.  skript Hello přístup všechny prostředky Azure nebo externím prostředkům, které jsou k dispozici z hello cloudu a spuštění v Azure.  Služby Azure Automation nemá hello možnost tooexecute sady runbook na serveru ve vašem místním datacentru, ale tato funkce není aktuálně k dispozici, při spuštění sady runbook hello v odpovědi tooLog Analytics výstrahy.

Obnovení v aplikaci SCOM i sady runbook v OMS může obsahovat skripty prostředí PowerShell, ale obnovení jsou obtížnější toocreate a spravovat, protože musí být obsažena v sadě management pack.  Sady Runbook jsou uložené ve službě Azure Automation, který poskytuje funkce pro vytváření, testování a správu sad runbook.

Pokud používáte SCOM jako zdroj dat pro analýzu protokolu, můžete vytvořit analýzy protokolů výstrah pomocí protokolu dotazu tooretrieve SCOM výstrahy uložené v hello OMS úložiště.  To umožní toorun runbook služby Azure Automation v odpovědi tooa SCOM výstrahy.  Samozřejmě protože hello runbook poběží v Azure, to nebude strategie přijatelná pro obnovení místního problémy.

## <a name="next-steps"></a>Další kroky
* Další podrobnosti o hello [výstrahy v System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).

