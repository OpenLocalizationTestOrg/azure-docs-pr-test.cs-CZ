---
title: "Nové hledání protokolu log Analytics nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje nejčastější dotazy týkající se upgradu analýzy protokolů pro nové dotazovací jazyk."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2017
ms.author: bwren
ms.openlocfilehash: d7bd0d780c265cc15ad09a73ede8c5a886005e37
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Nejčastější dotazy k vyhledávání a známé problémy protokolu nové analýzy protokolů

Tento článek obsahuje nejčastější dotazy a známé problémy týkající se upgradu [analýzy protokolů pro nové dotazovací jazyk](log-analytics-log-search-upgrade.md).  Měli byste si přečíst prostřednictvím celý článek před provedením rozhodnutí o upgrade vašeho pracovního prostoru.


## <a name="alerts"></a>Výstrahy

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Otázka: je nutné spoustu pravidla výstrah. Je třeba je znovu vytvořit v požadovaném jazyce po upgradu?  
Ne, vaše pravidla výstrah jsou automaticky převedena na nový jazyk vyhledávání během upgradu.  


## <a name="computer-groups"></a>Skupiny počítačů

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Otázka: zobrazuje chyby při pokusu o použití skupiny počítačů.  Změnila se jejich syntaxi?
Ano, syntaxe pro používání počítače skupin změny upgradován pracovního prostoru.  V tématu [skupiny počítačů v analýzy protokolů protokolu hledání](log-analytics-computer-groups.md) podrobnosti.

### <a name="known-issue-groups-imported-from-active-directory"></a>Známý problém: skupiny importovat ze služby Active Directory
Nelze momentálně vytvořit dotaz, který používá skupinu počítačů importovat ze služby Active Directory.  Jako alternativní řešení teprve po opravě tohoto problému, vytvořte novou skupinu počítačů pomocí importovaných skupiny služby Active Directory a potom pomocí této nové skupiny v dotazu.

Příklad dotazu pro vytvoření nové skupiny počítačů obsahující importované skupinu služby Active Directory je následující:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Řídicí panely

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Otázka: Použití řídicích panelů v upgradované pracovního prostoru?
Můžete nadále používat všechny dlaždice, které jste přidali do **vlastní řídicí panel** předtím, než byl upgradován pracovního prostoru, ale nelze upravit tyto dlaždice nebo přidat nové.  Můžete vytvořit a upravit zobrazení s [Návrhář zobrazení](log-analytics-view-designer.md) a také vytvořit řídicí panely na portálu Azure.


## <a name="log-searches"></a>Protokol hledání

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Otázka: mimo pracovní prostor upgradovaný I mají uložená hledání. Můžete převést je na nový jazyk vyhledávání automaticky?
Můžete na stránce vyhledávání protokolu převáděcí nástroj jazyk převést každé z nich.  Neexistuje žádná metoda automaticky převést více hledání bez nutnosti upgradu pracovním prostoru.

### <a name="question-why-are-my-query-results-not-sorted"></a>Otázka: Proč nejsou Moje výsledky dotazu seřazeny?
Výsledky nejsou ve výchozím nastavení v nové dotazovací jazyk seřazeny.  Použití [řazení operátor](https://go.microsoft.com/fwlink/?linkid=856079) seřadit výsledky podle jednoho nebo více vlastností.

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Známý problém: výsledky hledání v seznamu může obsahovat vlastnosti bez dat
Výsledky hledání protokolu v seznamu zobrazit vlastnosti se žádná data.  Před upgradem nebude součástí těchto vlastností.  Tento problém bude vyřešen, tak, aby prázdný vlastnosti se nezobrazí.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Známý problém: Výběr hodnoty v grafu nezobrazí podrobné výsledky
Pokud vyberete hodnotu v grafu, ho před upgradem, vrátí podrobný seznam záznamů odpovídající vybrané hodnoty.  Po upgradu je vrácena pouze jeden řádek souhrnnou.  Tento problém je aktuálně probíhá prozkoumat.

## <a name="log-search-api"></a>Rozhraní API pro prohledávání protokolů

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Otázka: Rozhraní API pro vyhledávání protokolu aktualizovat po upgradu?
[Rozhraní API pro vyhledávání protokolu](log-analytics-log-search-api.md) dosud nebyly upgradovány na nový jazyk vyhledávání.  Dál používat starší verze dotazovací jazyk s tímto rozhraním API i po upgradu pracovního prostoru.  Aktualizované dokumentace bude k dispozici pro rozhraní API pro vyhledávání protokolu při její aktualizaci.


## <a name="portals"></a>Portály

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Otázka: Mám použít nový portál pro pokročilé analýzy nebo zachovat pomocí portálu hledání protokolů?
Uvidíte porovnání dva portály v [portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics](log-analytics-log-search-portals.md).  Každá má odlišné výhody, abyste si mohli vybrat nejvhodnější pro vaše požadavky.  Je běžné psát dotazy na portálu pokročilé analýzy a vložit je na jiných místech, například Návrhář zobrazení.  Měli byste si přečíst o [problémy vzít v úvahu](log-analytics-log-search-portals.md#advanced-analytics-portal) při učinit.


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Otázka: Nic mění s integrací PowerBI?
Ano.  Jakmile pracovního prostoru, byl upgradován pak proces pro export dat analýzy protokolů do Power BI přestane fungovat.  Všechny existující plány, které jste vytvořili před upgradem bude zakázán.  Po upgradu, analýzy protokolů Azure používá stejnou platformu jako Application Insights a použít stejný postup exportování dotazů analýzy protokolů pro Power BI jako [proces exportu dotazy Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Známý problém: omezení velikosti Power BI
Není aktuálně omezení velikosti 8 MB pro analýzy protokolů dotaz, který je možné exportovat do Power BI.  Tento limit bude brzy zvýšena.


##<a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Otázka: Rutiny prostředí PowerShell vyhledávání protokolu aktualizovat po upgradu?
[Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) dosud nebyly upgradovány na nový jazyk vyhledávání.  Dál používat starší verze dotazovací jazyk pomocí této rutiny i po upgradu pracovního prostoru.  Aktualizované dokumentace bude k dispozici pro rutinu, když je aktualizována.


## <a name="resource-manager-templates"></a>Šablony Resource Manageru

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Otázka: Je možné vytvořit pracovní prostor služby upgradovaný pomocí šablony Resource Manageru?
Ano.  Musíte použít verzi rozhraní API 2017-03-15-preview a zahrnout **funkce** oddílu v šabloně jako v následujícím příkladu.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Řešení

### <a name="question-will-my-solutions-continue-to-work"></a>Otázka: Moje řešení budou fungovat?
Všechna řešení budou dále fungovat v upgradované prostoru, i když jejich výkonu bude zvýšit, pokud se převedou na nový dotazovací jazyk.  Existují známé problémy s některé existující řešení, které jsou popsané v této části.

### <a name="known-issue-capacity-and-performance-solution"></a>Známý problém: kapacitu a výkon řešení
Některá z částí v [kapacitu a výkon](log-analytics-capacity.md) zobrazení může být prázdný.  Oprava pro tento problém bude brzy k dispozici.

### <a name="known-issue-device-health-solution"></a>Známý problém: řešení stavu zařízení
[Řešení stavu zařízení](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) nebude shromažďování dat v pracovním prostoru upgradovaný.  Oprava pro tento problém bude brzy k dispozici.

### <a name="known-issue-application-insights-connector"></a>Známý problém: konektoru Application Insights
Perspektivy v [řešení Application Insights konektor](log-analytics-app-insights-connector.md) aktuálně nejsou podporované v upgradované prostoru.  Oprava pro tento problém aktuálně probíhá analýza.

## <a name="upgrade-process"></a>Proces upgradu

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Otázka: je nutné několik pracovních prostorů. Můžete upgradovat všechny pracovní prostory ve stejnou dobu?  
Ne.  Upgrade platí pokaždé, když do jednoho pracovního prostoru. Aktuálně neexistuje žádný způsob upgradu mnoho pracovních prostorů najednou. Upozorňujeme, že také se týká jiných uživatelů upgradovaný pracovního prostoru.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Otázka: Existující data protokolu, které jsou shromážděny v pracovního prostoru se změní-li I upgrade?  
Ne. Data protokolu, který je k dispozici pro hledání prostoru nemá vliv upgrade. Uložená hledání, výstrahy a zobrazení budou převedeny na nový jazyk vyhledávání automaticky.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Otázka: Co se stane, když nemáte upgradu pracovního prostoru?  
V nadcházejících měsících bude zastaralá hledání starších protokolů. Pracovní prostory, které nejsou upgradovány do té doby budou automaticky aktualizovány.

### <a name="question-i-didnt-choose-to-upgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Otázka: nebyla zvolena možnost upgrade, ale pracovní prostor byl upgradován přesto! Co se přihodilo?  
Jiný správce, tento pracovní prostor může upgradovat pracovním prostoru. Upozorňujeme, že všechny pracovní prostory budou automaticky upgradovány nový jazyk dosáhne obecné dostupnosti.  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-to-cancel-it-and-restore-everything-back-what-should-i-do"></a>Otázka: I omylem upgradu a teď potřebujete ho zrušit a obnovit vše zpět. Co mám udělat?  
Žádný problém.  Vytvoříme snímek pracovního prostoru před provedením upgradu, aby bylo možné jej obnovit. Mějte na paměti, která hledá, výstrahy a zobrazení, který jste uložili po upgradu budou ztraceny, když.  Chcete-li obnovit prostředí pracovního prostoru, postupujte podle pokynů v [můžete přejít zpět po upgradu?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade)


## <a name="views"></a>Zobrazení

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Otázka: Jak lze vytvořit nové zobrazení pomocí návrháře zobrazení?
Před upgradem můžete vytvořit nové zobrazení pomocí návrháře zobrazit z dlaždice na řídicím panelu hlavní.  Pracovní prostor upgradován, odeberou se tato dlaždice.  Můžete vytvořit nové zobrazení pomocí návrháře zobrazení na portálu OMS kliknutím na tlačítko v levé nabídce + zelená.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Známý problém: najdete v části všeho u spojnicových grafů v zobrazeních nevede k spojnicový graf
Po kliknutí na *zobrazit všechny* možnost v dolní části grafu řádku v zobrazení, se zobrazí tabulku.  Před upgradem se zobrazí se spojnicový graf.  Tento problém je analyzován potenciální upravovat.


## <a name="next-steps"></a>Další kroky

- Další informace o [upgrade na novou pracovní prostor analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md).
