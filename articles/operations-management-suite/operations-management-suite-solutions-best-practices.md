---
title: "osvědčené postupy řešení aaaOMSManagement | Microsoft Docs"
description: 
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 08cf1c101e301d24fb5c2bf4bc02a978e508a198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-management-solutions-in-operations-management-suite-oms-preview"></a>Osvědčené postupy pro vytváření řešení pro správu v Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview. Žádné schéma níže popsané je toochange subjektu.  

Tento článek obsahuje osvědčené postupy pro [vytváření soubor řešení správy](operations-management-suite-solutions-solution-file.md) v Operations Management Suite (OMS).  Tyto informace budou aktualizovány, jako jsou identifikovány další osvědčené postupy.

## <a name="data-sources"></a>Zdroje dat
- Zdroje dat mohou být [nakonfigurovat pomocí šablony Resource Manageru](../log-analytics/log-analytics-template-workspace-configuration.md), ale by neměly být obsažené v souboru řešení.  Hello důvodem je, že konfigurace zdroje dat není aktuálně idempotent, což znamená, že vaše řešení může přepsat existující konfiguraci v prostoru hello uživatele.<br><br>Řešení může například vyžadovat upozornění a chyb události z protokolu událostí aplikace hello.  Pokud toto určíte jako zdroj dat ve vašem řešení, riskujete odebrání – informační události, pokud uživatel hello měli nakonfigurovaný v jejich pracovním prostoru.  Pokud jste zahrnuli všechny události, pak vám může být shromažďování nadměrné informační události v prostoru hello uživatele.

- Pokud vaše řešení vyžaduje data z jedné hello standardní datových zdrojů, pak byste měli definovat to předpokladem je.  Stav v dokumentaci tohoto zákazníka hello musíte nakonfigurovat hello zdroj dat na své vlastní.  
- Přidat [ověření toku dat](../log-analytics/log-analytics-view-designer-tiles.md) shromažďovat zprávy tooany zobrazení v řešení tooinstruct hello uživatelů pro zdroje dat této toobe nutné nakonfigurovat pro toobe požadovaná data.  Tato zpráva se zobrazí na dlaždici hello hello zobrazení po nebyla nalezena požadovaná data.


## <a name="runbooks"></a>Runbooky
- Přidat [plánování Automation](../automation/automation-schedules.md) pro každou sadu runbook ve vašem řešení, která potřebuje toorun podle plánu.
- Zahrnout hello [IngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) v toobe vaše řešení používá v runboocích zápis úložiště analýzy protokolů toohello data.  Konfigurace řešení hello příliš[odkaz](operations-management-suite-solutions-solution-file.md#solution-resource) tento prostředek, že to zůstává, pokud je odebrán hello řešení.  To umožňuje více řešení tooshare hello modulu.
- Použití [proměnné služeb automatizace](../automation/automation-schedules.md) tooprovide hodnoty toohello řešení, které uživatelé pravděpodobně třeba toochange později.  I v případě, že je řešení hello nakonfigurované toocontain hello proměnné, jeho hodnota může být stále změněna.

## <a name="views"></a>Zobrazení
- Všechna řešení by měly obsahovat jedno zobrazení, které se zobrazí v hello user portal.  Hello zobrazení může obsahovat více [vizualizace částí](../log-analytics/log-analytics-view-designer-parts.md) tooillustrate různých nastaví dat.
- Přidat [ověření toku dat](../log-analytics/log-analytics-view-designer-tiles.md) shromažďovat zprávy tooany zobrazení v řešení tooinstruct hello uživatelů pro zdroje dat této toobe nutné nakonfigurovat pro toobe požadovaná data.
- Konfigurace řešení hello příliš[obsahovat](operations-management-suite-solutions-solution-file.md#solution-resource) hello zobrazení tak, aby se odebere, pokud je odebrán hello řešení.

## <a name="alerts"></a>Výstrahy
- Definujte seznam příjemců hello jako parametr v souboru řešení hello, takže hello uživatel může definovat je při instalaci hello řešení.
- Konfigurace řešení hello příliš[odkaz](operations-management-suite-solutions-solution-file.md#solution-resource) pravidla upozornění, že uživatel může změnit jejich konfigurace.  Chtějí může toomake změny, jako je například úprava hello seznam příjemců, změna hello prahová hodnota výstrahy hello nebo zakázat pravidlo výstrahy hello. 


## <a name="next-steps"></a>Další kroky
* Provede hello základní proces [navrhování a vytváření řešení pro správu](operations-management-suite-solutions-creating.md).
* Zjistěte, jak příliš[vytvořte soubor řešení](operations-management-suite-solutions-solution-file.md).
* [Přidat uložená hledání a výstrahy](operations-management-suite-solutions-resources-searches-alerts.md) tooyour řešení pro správu.
* [Přidání zobrazení](operations-management-suite-solutions-resources-views.md) tooyour řešení pro správu.
* [Přidat runbooků služeb automatizace a dalším prostředkům](operations-management-suite-solutions-resources-automation.md) tooyour řešení pro správu.

