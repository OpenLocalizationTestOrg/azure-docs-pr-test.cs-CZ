---
title: "aaaAzure analýzy protokolů dotazu jazyka tahák | Microsoft Docs"
description: "Tento článek obsahuje pomoc na přechod toohello nové dotazovací jazyk pro analýzu protokolu, pokud jste již obeznámeni s jazykem hello starší verze."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 8b4ee3d0b5e1ec8a9f95a09e0ad9835615ad1342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transitioning-tooazure-log-analytics-new-query-language"></a>Přechod nové dotazovacího jazyka pro tooAzure analýzy protokolů

> [!NOTE]
> Můžete si přečíst další informace o hello nové analýzy protokolů dotazu jazyka a získat hello postup tooupgrade pracovního prostoru v upgradu vaší [hledání protokolů toonew pracovní prostor analýzy protokolů Azure](log-analytics-log-search-upgrade.md).

Tento článek obsahuje pomoc na přechod toohello nové dotazovací jazyk pro analýzu protokolu, pokud jste již obeznámeni s jazykem hello starší verze.

## <a name="language-converter"></a>Převaděč jazyk

Pokud jste obeznámeni s hello dotazovacího jazyka pro starší verze analýzy protokolů, hello nejjednodušší toocreate hello stejný dotaz v jazyce nové hello je toouse hello převaděč jazyk, bude nainstalován do portálu hledání protokolů hello při převodu pracovního prostoru.  Pomocí převaděče hello je jednoduché, zadáním v starší verze dotaz hello nejvyšší textového pole a pak levým na **převést**.  Můžete buď klikněte na tlačítko hello vyhledávací tlačítko toorun hello dotaz nebo kopírování a vložte jej toouse ho někde jinde.

![Převaděč jazyk](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a>Tahák

Hello následující tabulka uvádí srovnání mezi celou řadu běžných dotazů tooequivalent příkazy mezi hello nové a starší verze dotazovací jazyk v Azure Log Analytics.

| Popis | Starší verze | Nový |
|:--|:--|:--|
| Vyhledat všechny tabulky      | error | hledání "error" (ne velká a malá písmena) |
| Vyberte data z tabulky | Typ = událostí |  Událost |
|                        | Typ = událostí &#124; Vyberte zdroj protokolu událostí, ID události | Událost &#124; Zdroj protokolu událostí, EventID projektu |
|                        | Typ = událostí &#124; prvních 100 | Událost &#124; trvat 100 |
| Porovnání řetězců      | Typ = Computer=srv01.contoso.com událostí   | Událost &#124; kde počítač == "srv01.contoso.com" |
|                        | Typ = Computer=contains("contoso") událostí | Událost &#124; Pokud počítač obsahuje "contoso" (ne velká a malá písmena)<br>Událost &#124; kde contains_cs počítače "Contoso" (malá a velká písmena) |
|                        | Typ = počítač událostí = RegEx ("@contoso@")  | Událost &#124; kde počítače odpovídá regulárnímu výrazu ". *contoso*" |
| Porovnání kalendářních dat        | Typ události TimeGenerated = > nyní 1DAYS | Událost &#124; kde TimeGenerated > ago(1d) |
|                        | Typ události TimeGenerated = > 2017-05-01 TimeGenerated < 2017-05-31 | Událost &#124; kde TimeGenerated mezi (datetime(2017-05-01)... DATETIME(2017-05-31)) |
| Logická hodnota porovnání     | Typ = prezenčního signálu IsGatewayInstalled = false  | Prezenční signál | kde IsGatewayInstalled hodnotu false |
| Seřadit                   | Typ = událostí &#124; Seřaďte počítače asc, EventLevelName asc a desc protokolu událostí | Událost \| Řadit podle počítače asc, EventLevelName asc a desc protokolu událostí |
| Odlišné               | Typ = událostí &#124; odstranění duplicitních dat počítače \| Vyberte počítač | Událost &#124; shrnout počítačem protokolu událostí |
| Rozšíření sloupců         | Typ = výkonu název_čítače = "čas procesoru v %" &#124; ROZŠÍŘENÍ if(map(CounterValue,0,50,0,1),"HIGH","LOW") jako využití | Výkonu &#124; kde CounterName == "% času procesoru" \| rozšíření využití =, je-li ("Nedostatek" přepočtené > 50, "Vysoká") |
| Agregace            | Typ = událostí &#124; míra count() jako počet počítačem. | Událost &#124; shrnout Count = count() počítačem. |
|                                | Typ = výkonu ObjectName = název_čítače procesoru = "čas procesoru v %" &#124; míra avg(CounterValue) počítače interval 5 minut | Výkonu &#124; kde ObjectName == "Procesor" a název_čítače == "% času procesoru" &#124; Shrňte avg(CounterValue) počítačem bin (TimeGenerated, 5 minut) |
| Agregace s limitem | Typ = událostí &#124; míra count() podle počítače &#124; prvních 10 | Událost &#124; shrnout AggregatedValue = count() podle počítače &#124; limit 10 |
| sjednocení                  | Typ = události nebo typ = Syslog | sjednocení události procesu Syslog |
| Spojit                   | Typ = NetworkMonitoring &#124; připojení vnitřní AgentIP (typ = prezenční signál) ComputerIP | NetworkMonitoring &#124; připojení typu = vnitřní (vyhledávání typu == "Prezenčního signálu") na $left. AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Další kroky
- Podívejte se [kurz na zápis dotazů](https://go.microsoft.com/fwlink/?linkid=856078) pomocí dotazovacího jazyka pro nové hello.
- Odkazovat toohello [referenční příručka jazyka dotazů](https://go.microsoft.com/fwlink/?linkid=856079) podrobné informace o příkazu, operátory a funkce pro hello nové dotazovací jazyk.  
