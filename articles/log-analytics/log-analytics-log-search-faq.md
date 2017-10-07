---
title: "nové hledání protokolu aaaLog Analytics nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje nejčastější dotazy týkající se upgradu hello analýzy protokolů toohello nové dotazovací jazyk."
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
ms.openlocfilehash: b8664c8329fab0547f270793fa13e8cdd06ba637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Nejčastější dotazy k vyhledávání a známé problémy protokolu nové analýzy protokolů

Tento článek obsahuje nejčastější dotazy a známé problémy týkající se upgradu hello [toohello analýzy protokolů nový dotaz jazyka](log-analytics-log-search-upgrade.md).  Měli byste si přečíst prostřednictvím celý článek před provedením hello rozhodnutí tooupgrade pracovního prostoru.


## <a name="alerts"></a>Výstrahy

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-toocreate-them-again-in-hello-new-language-after-i-upgrade"></a>Otázka: je nutné spoustu pravidla výstrah. Potřebuji toocreate je znovu za nový jazyk hello po upgradu?  
Ne, vaše pravidla výstrah jsou automaticky převedený toohello nový vyhledávání jazyk během upgradu.  


## <a name="computer-groups"></a>Skupiny počítačů

### <a name="question-im-getting-errors-when-trying-toouse-computer-groups--has-their-syntax-changed"></a>Otázka: zobrazuje chyby při pokusu o toouse skupiny počítačů.  Změnila se jejich syntaxi?
Ano, hello syntaxe pro používání počítače skupin změny upgradován pracovního prostoru.  V tématu [skupiny počítačů v analýzy protokolů protokolu hledání](log-analytics-computer-groups.md) podrobnosti.

### <a name="known-issue-groups-imported-from-active-directory"></a>Známý problém: skupiny importovat ze služby Active Directory
Nelze momentálně vytvořit dotaz, který používá skupinu počítačů importovat ze služby Active Directory.  Jako alternativní řešení teprve po opravě tohoto problému, vytvořte novou skupinu počítačů pomocí hello importovat skupiny služby Active Directory a potom pomocí této nové skupiny v dotazu.

Příklad dotazu toocreate nová skupina počítačů, která zahrnuje importované skupinu služby Active Directory je následující:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Řídicí panely

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Otázka: Použití řídicích panelů v upgradované pracovního prostoru?
Můžete pokračovat toouse všechny dlaždice, které jste přidali příliš**vlastní řídicí panel** předtím, než byl upgradován pracovního prostoru, ale nelze upravit tyto dlaždice nebo přidat nové.  Můžete pokračovat toocreate a upravit zobrazení s [Návrhář zobrazení](log-analytics-view-designer.md) a také vytvořit řídicí panely v hello portálu Azure.


## <a name="log-searches"></a>Protokol hledání

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-toohello-new-search-language-automatically"></a>Otázka: mimo pracovní prostor upgradovaný I mají uložená hledání. Můžete převést je nový jazyk vyhledávání toohello automaticky?
Můžete vytvořit hello jazyk převáděcí nástroj v hello protokolu hledání stránky tooconvert každé z nich.  Neexistuje žádná metoda tooautomatically převést více vyhledávání bez upgrade hello prostoru.

### <a name="question-why-are-my-query-results-not-sorted"></a>Otázka: Proč nejsou Moje výsledky dotazu seřazeny?
Výsledky nejsou ve výchozím nastavení v hello nové dotazovací jazyk seřazeny.  Použití hello [řazení operátor](https://go.microsoft.com/fwlink/?linkid=856079) toosort výsledky podle jednoho nebo více vlastností.

### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Známý problém: výsledky hledání v seznamu může obsahovat vlastnosti bez dat
Výsledky hledání protokolu v seznamu zobrazit vlastnosti se žádná data.  Předchozí tooupgrade tyto vlastnosti nebudou zahrnuty.  Tento problém bude vyřešen, tak, aby prázdný vlastnosti se nezobrazí.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Známý problém: Výběr hodnoty v grafu nezobrazí podrobné výsledky
Předchozí tooupgrade, když jste vybrali hodnota v grafu, měla by vrátit podrobný seznam záznamů, odpovídající hodnota hello vybrané.  Po upgradu je vrácena pouze hello jednu souhrnnou řádek.  Tento problém je aktuálně probíhá prozkoumat.

## <a name="log-search-api"></a>Rozhraní API pro prohledávání protokolů

### <a name="question-does-hello-log-search-api-get-updated-after-i-upgrade"></a>Otázka: Hello rozhraní API pro vyhledávání protokolu aktualizovat po upgradu?
Hello [rozhraní API pro vyhledávání protokolu](log-analytics-log-search-api.md) dosud nebyla upgradovaná toohello nový jazyk vyhledávání.  Starší verze dotazovacího jazyka pro toouse hello s tímto rozhraním API pokračujte i po upgradu pracovního prostoru.  Když je aktualizována, budou k dispozici pro hello rozhraní API pro protokol hledání aktualizované dokumentace.


## <a name="portals"></a>Portály

### <a name="question-should-i-use-hello-new-advanced-analytics-portal-or-keep-using-hello-log-search-portal"></a>Otázka: Mám použít nový portál pokročilé analýzy hello nebo zachovat pomocí portálu hledání protokolů hello?
Uvidíte porovnání hello dva portály v [portály pro vytváření a úpravy dotazů protokolu v Azure Log Analytics](log-analytics-log-search-portals.md).  Každý má různé výhody, abyste si mohli vybrat hello nejvhodnější pro vaše požadavky.  Je běžné dotazy toowrite hello pokročilé analýzy portálu a vložit do jiných místech, například Návrhář zobrazení.  Měli byste si přečíst o [problémy tooconsider](log-analytics-log-search-portals.md#advanced-analytics-portal) při učinit.


## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Otázka: Nic mění s integrací PowerBI?
Ano.  Po upgradu pracovního prostoru pak hello proces pro export dat tooPower analýzy protokolů BI přestane fungovat.  Všechny existující plány, které jste vytvořili před upgradem bude zakázán.  Po upgradu, hello Azure Log Analytics používá stejnou platformu jako Application Insights a použijete stejný proces tooexport analýzy protokolů dotazy tooPower BI jako hello [hello proces tooexport Application Insights dotazuje tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Známý problém: omezení velikosti Power BI
Není aktuálně omezení velikosti 8 MB pro analýzy protokolů dotaz, který může být exportovaný tooPower BI.  Tento limit bude brzy zvýšena.


##<a name="powershell-cmdlets"></a>Rutiny prostředí PowerShell

### <a name="question-does-hello-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Otázka: Rutiny prostředí PowerShell vyhledávání protokolu hello aktualizovat po upgradu?
Hello [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) dosud nebyla upgradovaná toohello nový jazyk vyhledávání.  Starší verze dotazovacího jazyka pro toouse hello pomocí této rutiny pokračujte i po upgradu pracovního prostoru.  Aktualizované dokumentace bude k dispozici pro rutinu hello, když je aktualizována.


## <a name="resource-manager-templates"></a>Šablony Resource Manageru

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Otázka: Je možné vytvořit pracovní prostor služby upgradovaný pomocí šablony Resource Manageru?
Ano.  Musíte použít verzi rozhraní API 2017-03-15-preview a zahrnout **funkce** oddílu v šabloně jako hello následující ukázka.

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

### <a name="question-will-my-solutions-continue-toowork"></a>Otázka: Bude Moje řešení toowork?
Všechna řešení bude pokračovat toowork v upgradované prostoru, i když jejich výkonu bude zvýšit, pokud jsou převedené toohello nové dotazovací jazyk.  Existují známé problémy s některé existující řešení, které jsou popsané v této části.

### <a name="known-issue-capacity-and-performance-solution"></a>Známý problém: kapacitu a výkon řešení
Některá z částí hello v hello [kapacitu a výkon](log-analytics-capacity.md) zobrazení může být prázdný.  Chybu opravte toothis bude brzy k dispozici.

### <a name="known-issue-device-health-solution"></a>Známý problém: řešení stavu zařízení
Hello [řešení stavu zařízení](https://docs.microsoft.com/windows/deployment/update/device-health-monitor) nebude shromažďování dat v pracovním prostoru upgradovaný.  Chybu opravte toothis bude brzy k dispozici.

### <a name="known-issue-application-insights-connector"></a>Známý problém: konektoru Application Insights
Perspektivy v [řešení Application Insights konektor](log-analytics-app-insights-connector.md) aktuálně nejsou podporované v upgradované prostoru.  Chybu opravte toothis je aktuálně v rámci analýzy.

## <a name="upgrade-process"></a>Proces upgradu

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-hello-same-time"></a>Otázka: je nutné několik pracovních prostorů. Můžete upgradovat všechny pracovní prostory v hello současně?  
Ne.  Upgrade platí pokaždé, když tooa jednoho pracovního prostoru. Aktuálně neexistuje žádný způsob upgradu mnoho pracovních prostorů najednou. Upozorňujeme, že také se týká jiných uživatelů hello upgradovat pracovního prostoru.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Otázka: Existující data protokolu, které jsou shromážděny v pracovního prostoru se změní-li I upgrade?  
Ne. Hello protokolu data k dispozici tooyour prostoru hledání nemá vliv hello upgradu. Uložená hledání, výstrahy a zobrazení bude převedený toohello nový vyhledávání jazyk automaticky.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Otázka: Co se stane, když nemáte upgradu pracovního prostoru?  
hledání starší verze protokolů Hello přestanou v hello přicházející měsíců. Pracovní prostory, které nejsou upgradovány do té doby budou automaticky aktualizovány.

### <a name="question-i-didnt-choose-tooupgrade-but-my-workspace-has-been-upgraded-anyway-what-happened"></a>Otázka: nebyla zvolit tooupgrade, ale pracovní prostor byl upgradován přesto! Co se přihodilo?  
Jiný správce, tento pracovní prostor může upgradovat hello prostoru. Upozorňujeme, že všechny pracovní prostory budou automaticky upgradovány nový jazyk hello dosáhne obecné dostupnosti.  

### <a name="question-i-have-upgraded-by-mistake-and-now-need-toocancel-it-and-restore-everything-back-what-should-i-do"></a>Otázka: I omylem upgradu a nyní nutné toocancel ho a obnovení všechno zpět. Co mám udělat?  
Žádný problém.  Vytvoříme snímek pracovního prostoru před provedením upgradu, aby bylo možné jej obnovit. Mějte na paměti, která hledá, výstrahy a zobrazení, který jste uložili po upgradu hello budou ztraceny, když.  toorestore prostředí pracovního prostoru, použijte proceduru hello v [můžete přejít zpět po upgradu?](log-analytics-log-search-upgrade.md#can-i-go-back-after-i-upgrade)


## <a name="views"></a>Zobrazení

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Otázka: Jak lze vytvořit nové zobrazení pomocí návrháře zobrazení?
Předchozí tooupgrade, můžete vytvořit nové zobrazení pomocí návrháře zobrazit z dlaždice na řídicím panelu hlavní hello.  Pracovní prostor upgradován, odeberou se tato dlaždice.  Můžete vytvořit nové zobrazení pomocí návrháře zobrazení na portálu OMS hello kliknutím na tlačítko v levé nabídce hello + hello zelená.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Známý problém: najdete v části všeho u spojnicových grafů v zobrazeních nevede k spojnicový graf
Po kliknutí na hello *zobrazit všechny* možnost v hello dolní části grafu řádku v zobrazení, se zobrazí tabulku.  Předchozí tooupgrade by zobrazí se spojnicový graf.  Tento problém je analyzován potenciální upravovat.


## <a name="next-steps"></a>Další kroky

- Další informace o [upgrade vašeho pracovního prostoru toohello nové analýzy protokolů dotazu jazyka](log-analytics-log-search-upgrade.md).
