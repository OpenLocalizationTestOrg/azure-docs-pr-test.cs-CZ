---
title: "využití dat aaaAnalyze v Log Analytics | Microsoft Docs"
description: "Pomocí řídicího panelu hello využití v tooview analýzy protokolů, kolik dat je odesílána toohello analýzy protokolů služby a řešení potíží s proto velké objemy dat jsou odesílány."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 74d0adcb-4dc2-425e-8b62-c65537cef270
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: magoedte
ms.openlocfilehash: c30373dd6edbe3ff900fbebc865575fee61ce14c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-usage-in-log-analytics"></a>Analýza využití dat v Log Analytics
Analýza protokolu obsahuje informace o hello množství dat shromažďovaných, které počítače odeslat hello data a různé typy dat odesílaných hello.  Použití hello **protokolu analýzy využití** řídicí panel toosee hello velikost dat odeslaných toohello analýzy protokolů služby. řídicí panel Hello zobrazuje množství dat shromažďovaných každé řešení a množství dat, které počítače jsou odesílání.

## <a name="understand-hello-usage-dashboard"></a>Pochopení využití hello řídicí panel
Hello **analýzy protokolů využití** řídicí panel zobrazuje hello následující informace:

- Objem dat
    - Objem dat v průběhu času (v závislosti na vašem aktuálním časovém rozsahu)
    - Objem dat podle řešení
    - Data, která nejsou spojena s počítačem
- Počítače
    - Počítače odesílající data
    - Počítače, které během posledních 24 hodin neodeslaly žádná data
- Nabídky
    - Uzly Insight and Analytics
    - Uzly Automation and Control
    - Uzly zabezpečení
- Výkon
    - Doba trvání data toocollect a index.
- Seznam dotazů

![řídicí panel využití](./media/log-analytics-usage/usage-dashboard01.png)

### <a name="toowork-with-usage-data"></a>toowork se data o využití
1. Pokud jste tak již neučinili, přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí svého předplatného Azure.
2. Na hello **rozbočovače** nabídky, klikněte na tlačítko **další služby** a v hello seznamu prostředků zadejte **analýzy protokolů**. Během zadávání hello seznamu filtrů podle vašeho zadání. Klikněte na **Log Analytics**.  
    ![Centrum Azure](./media/log-analytics-usage/hub.png)
3. Hello **analýzy protokolů** řídicí panel zobrazuje seznam pracovní prostory. Vyberte pracovní prostor.
4. V hello *prostoru* řídicí panel, klikněte na tlačítko **analýzy protokolů využití**.
5. Na hello **protokolu analýzy využití** řídicí panel, klikněte na tlačítko **čas: posledních 24 hodin** toochange hello časový interval.  
    ![časový interval](./media/log-analytics-usage/time.png)
6. Zobrazení hello využití kategorie okna, které se zobrazí oblasti vás zajímá. Zvolte okno a potom klikněte na položku v ní tooview další podrobnosti v [hledání protokolů](log-analytics-log-searches.md).  
    ![příklad okna využití dat](./media/log-analytics-usage/blade.png)
7. Na řídicím panelu vyhledávání protokolu hello zkontrolujte hello výsledky, které jsou z hledání hello.  
    ![příklad prohledávání protokolů využití](./media/log-analytics-usage/usage-log-search.png)

## <a name="create-an-alert-when-data-collection-is-higher-than-expected"></a>Vytvoření upozornění při větším než očekávaném shromažďování dat
Tato část popisuje, jak toocreate výstrahu, pokud:
- Objem dat překračuje zadanou velikost.
- Datový svazek je předpokládaných tooexceed po zadanou dobu.

[Upozornění](log-analytics-alerts-creating.md) v Log Analytics používají vyhledávací dotazy. Hello následující dotaz výsledek obsahuje po více než 100 GB dat shromažďovaných hello posledních 24 hodin:

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`

Hello následující dotaz používá jednoduchý vzorec toopredict při více než 100 GB dat zašle za den: 

`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`

tooalert na jiný datový svazek, změnu hello 100 v hello dotazy toohello číslo chcete tooalert na GB.

Použít hello kroků popsaných v [vytvořit pravidlo výstrahy](log-analytics-alerts-creating.md#create-an-alert-rule) toobe upozornění při shromažďování dat je vyšší, než se očekávalo.

Při vytváření hello upozornění pro první dotaz hello – když je více než 100 GB dat v 24 hodin, nastavte:
- **Název** příliš*datový svazek je větší než 100 GB za 24 hodin*
- **Závažnost** příliš*upozornění*
- **Vyhledávací dotaz** příliš`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(Quantity,1024)) as DataGB by Type | where DataGB > 100`
- **Časový interval** příliš*24 hodin*.
- **Výstrahy frekvence** toobe jedné hodiny vzhledem k tomu, že data o využití hello aktualizuje jenom jednou za hodinu.
- **Generují výstrahu založenou na** toobe *počet výsledků*
- **Počet výsledků** toobe *větší než 0.*

Použít hello kroků popsaných v [přidat pravidla tooalert akce](log-analytics-alerts-actions.md) nakonfigurovat e-mailu, webhooku nebo sadu runbook akce pro pravidlo výstrahy hello.

Při vytváření hello výstrahy pro druhý dotaz hello – když je předpovědět, že bude více než 100 GB dat za 24 hodin, nastavte:
- **Název** příliš*datový svazek toogreater než 100 GB očekávaný za 24 hodin*
- **Závažnost** příliš*upozornění*
- **Vyhledávací dotaz** příliš`Type=Usage QuantityUnit=MBytes IsBillable=true | measure sum(div(mul(Quantity,8),1024)) as EstimatedGB by Type | where EstimatedGB > 100`
- **Časový interval** příliš*3 hodiny*.
- **Výstrahy frekvence** toobe jedné hodiny vzhledem k tomu, že data o využití hello aktualizuje jenom jednou za hodinu.
- **Generují výstrahu založenou na** toobe *počet výsledků*
- **Počet výsledků** toobe *větší než 0.*

Pokud obdržíte výstrahu, použijte hello kroky v následující části tootroubleshoot, proč je vyšší, než se očekávalo využití hello.

## <a name="troubleshooting-why-usage-is-higher-than-expected"></a>Řešení potíží způsobujících větší využití, než se čekalo
řídicí panel využití Hello vám pomůže tooidentify proč použití (a proto náklady) je vyšší, než se očekává.

Větší využití je způsobeno jedním nebo obojím z těchto aspektů:
- Více dat než bylo očekáváno odesílány tooLog Analytics
- Další uzly, než se očekávalo, odesílání dat tooLog Analytics

### <a name="check-if-there-is-more-data-than-expected"></a>Kontrola, jestli se shromažďuje více dat, než se čekalo 
Existují dva klíče oddíly hello využití stránky, které je pomohou identifikovat příčinu hello toobe většinu dat shromážděných.

Hello *datový svazek v čase* graf znázorňuje hello celkový objem dat odesílaných a hello počítače odesílání hello většinu dat. Graf Hello v horní části hello umožňuje toosee Pokud vaše využití celkového dat roste, zbývající konstantní nebo klesá. Hello seznam počítačů, zobrazí počítače hello 10 odesílání hello většinu dat.

Hello *datový svazek podle řešení* graf znázorňuje hello objem dat, které odesílají každé řešení a řešení hello odesílání hello většina dat. Graf Hello v horní části hello znázorňuje hello celkový objem dat, které odesílají každé řešení v čase. Tyto informace můžete tooidentify zda řešení odesílá další data, o hello stejné částka dat nebo méně dat v čase. Hello seznam řešení ukazuje odesílání hello většinu dat řešení hello 10. 

Tyto dva grafy zobrazují veškerá data. Některá data jsou fakturovatelná, ostatní jsou zdarma. toofocus pouze pro data, která fakturovatelný upravit dotaz hello na tooinclude stránku hello vyhledávání `IsBillable=true`.  

![Grafy objemu dat](./media/log-analytics-usage/log-analytics-usage-data-volume.png)

Podívejte se na hello *datový svazek v čase* grafu. toosee hello řešení a typy dat, které odesílají hello většinu dat pro určitý počítač, klikněte na název hello hello počítače. Klikněte na název hello hello počítače první v seznamu hello.

V hello následující snímek obrazovky, hello *Správa protokolu nebo výkonu* datový typ odesílá hello většinu dat pro počítač hello. 

![objem dat pro počítač](./media/log-analytics-usage/log-analytics-usage-data-volume-computer.png)

Dále přejděte zpět toohello *využití* řídicí panel a podívejte se na hello *datový svazek podle řešení* grafu. počítače hello toosee odesílání hello většinu dat pro řešení, klikněte na název hello hello řešení v seznamu hello. Klikněte na název hello hello první řešení v seznamu hello. 

V hello následující snímek obrazovky, potvrzuje se tím, že hello *acmetomcat* počítač odesílá hello většinu dat hello protokolu řešení pro správu.

![objem dat pro řešení](./media/log-analytics-usage/log-analytics-usage-data-volume-solution.png)

V případě potřeby proveďte další analýzu tooidentify velké svazky v rámci řešení nebo datového typu. Ukázky dotazů:

+ Řešení **zabezpečení**
  - `Type=SecurityEvent | measure count() by EventID`
+ Řešení **pro správu protokolů**
  - `Type=Usage Solution=LogManagement IsBillable=true | measure count() by DataType`
+ Datový typ **Perf**
  - `Type=Perf | measure count() by CounterPath`
  - `Type=Perf | measure count() by CounterName`
+ Datový typ **Event**
  - `Type=Event | measure count() by EventID`
  - `Type=Event | measure count() by EventLog, EventLevelName`
+ Datový typ **Syslog**
  - `Type=Syslog | measure count() by Facility, SeverityLevel`
  - `Type=Syslog | measure count() by ProcessName`
+ Datový typ **AzureDiagnostics**
  - `Type=AzureDiagnostics | measure count() by ResourceProvider, ResourceId`

Použijte následující kroky tooreduce hello objemu shromážděné protokoly hello:

| Zdroj velkého objemu dat | Jak tooreduce datový svazek |
| -------------------------- | ------------------------- |
| Události zabezpečení            | Vyberte [běžné nebo minimální události zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/). <br> Změnit události toocollect potřeba jenom zásad auditu zabezpečení hello. Konkrétně zkontrolujte hello nutné toocollect události pro <br> - [audit platformy Filtering Platform](https://technet.microsoft.com/library/dd772749(WS.10).aspx) <br> - [audit registru](https://docs.microsoft.com/windows/device-security/auditing/audit-registry)<br> - [audit systému souborů](https://docs.microsoft.com/windows/device-security/auditing/audit-file-system)<br> - [audit objektu jádra](https://docs.microsoft.com/windows/device-security/auditing/audit-kernel-object)<br> - [audit manipulace s popisovačem](https://docs.microsoft.com/windows/device-security/auditing/audit-handle-manipulation)<br> - [audit vyměnitelného úložiště](https://docs.microsoft.com/windows/device-security/auditing/audit-removable-storage) |
| Čítače výkonu       | Změňte [konfiguraci čítačů výkonu](log-analytics-data-sources-performance-counters.md) tak, aby se: <br> -Snížit frekvenci hello shromažďování <br> – Snížil počet čítačů výkonu |
| Protokoly událostí                 | Změňte [konfiguraci protokolů událostí](log-analytics-data-sources-windows-events.md) tak, aby se: <br> -Snížit počet hello shromážděné protokoly událostí <br> – Shromažďovaly pouze požadované úrovně událostí Například zrušte shromažďování událostí úrovně *Informace*. |
| Syslog                     | Změňte [konfiguraci syslogu](log-analytics-data-sources-syslog.md) tak, aby se: <br> -Snížit hello počet zařízení, shromážděných <br> – Shromažďovaly pouze požadované úrovně událostí Například zrušte shromažďování událostí úrovně *Informace* a *Ladění*. |
| AzureDiagnostics           | Změňte shromažďování protokolů prostředků tak, aby se: <br> -Snížení hello počtu prostředků odeslat protokoly tooLog Analytics <br> – Shromažďovaly pouze požadované protokoly |
| Řešení data z počítačů, které nepotřebujete hello řešení | Použití [cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md) toocollect data z pouze požadovaných skupin počítačů. |

### <a name="check-if-there-are-more-nodes-than-expected"></a>Kontrola, jestli existuje více uzlů, než se čekalo
Pokud jste na hello *na uzel (OMS)* cenovou úroveň, pak budou účtovat na základě hello počtu uzlů a řešení, které používáte. Zobrazí se, kolik uzly každý nabídka jsou používány v hello *nabídky* část řídicího panelu využití hello.

![řídicí panel využití](./media/log-analytics-usage/log-analytics-usage-offerings.png)

Klikněte na **zobrazit všechny...**  tooview hello úplný seznam počítačů odeslání dat pro vybranou nabídku hello.

Použití [cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md) toocollect data z pouze požadovaných skupin počítačů.


## <a name="next-steps"></a>Další kroky
* V tématu [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) toolearn jak toouse hello vyhledávání jazyk. Můžete vytvořit další analýzu tooperform vyhledávací dotazy na data o využití hello.
* Použít hello kroků popsaných v [vytvořit pravidlo výstrahy](log-analytics-alerts-creating.md#create-an-alert-rule) splníte toobe upozorněni, když kritéria vyhledávání
* Použití [cílení na řešení](../operations-management-suite/operations-management-suite-solution-targeting.md) toocollect data z jenom požadované skupiny počítačů
* Vyberte [běžné nebo minimální události zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/11/08/filter-the-security-events-the-oms-security-collects/).
* Změňte [konfiguraci čítačů výkonu](log-analytics-data-sources-performance-counters.md).
* Změňte [konfiguraci protokolů událostí](log-analytics-data-sources-windows-events.md).
* Změňte [konfiguraci syslogu](log-analytics-data-sources-syslog.md).
