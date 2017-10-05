---
title: "Odpovědi na výstrahy v OMS Log Analytics | Microsoft Docs"
description: "Výstrahy v Log Analytics identifikovat důležité informace ve svém úložišti OMS a můžete proaktivně upozorňují na problémy nebo vyvolání akce se pokusit o opravte je.  Tento článek popisuje, jak vytvořit pravidlo výstrahy a podrobnosti o různé akce, které jejich zajištění může trvat."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Přidání akce do pravidla výstrah v analýzy protokolů
Když [v analýzy protokolů se vytvoří výstraha](log-analytics-alerts.md), máte možnost [konfigurace pravidlo výstrahy](log-analytics-alerts.md) provést několik akcí.  Tento článek popisuje různé akce, které jsou k dispozici a podrobnosti o konfiguraci jednotlivých typů.

| Akce | Popis |
|:--|:--|
| [E-mailu](#email-actions) | Jeden nebo více příjemcům odeslat e-mail s detaily výstrahy. |
| [Webhooku](#webhook-actions) | Vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST. |
| [Sady Runbook](#runbook-actions) | Spuštění sady runbook ve službě Azure Automation. |


## <a name="email-actions"></a>Akce e-mailu
E-mailu akce odeslání e-mailu s detaily výstrahy jednoho nebo více příjemců.  Můžete zadat předmět e-mailu, ale jeho obsah je standardní formát vytvořená pomocí analýzy protokolů.  Obsahuje souhrnné informace, jako je název výstrahy kromě podrobností až deset záznamů protokolu nalezené.  Zahrnuje také odkaz k hledání protokolů v analýzy protokolů, který vrátí celou sadu záznamů z tohoto dotazu.   Odesílatelem e-mailu je *týmu Microsoft Operations Management Suite &lt; noreply@oms.microsoft.com &gt;* . 

E-mailu akce vyžadují vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Předmět |Subjektu v e-mailu.  Tělo e-mailu se nedá změnit. |
| Příjemce |Adresy všech příjemců e-mailu.  Pokud zadáte víc než jednou adresou, jednotlivé adresy oddělujte středníkem (;). |


## <a name="webhook-actions"></a>Akce Webhooku

Akce Webhooku umožňují vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST.  Služba volané by měla podporovat webhooky a určit, jak se bude používat žádné datové části obdrží.  Může také zavolat REST API, která nepodporuje konkrétně webhooků, pokud je požadavek ve formátu, který funguje s technologií rozhraní API.  Příklady použití webhook, jehož v reakci na výstrahy jsou odesílání zprávy v [Slack](http://slack.com) nebo vytváření incidentu v [PagerDuty](http://pagerduty.com/).  Kompletní a podrobný postup vytvoření pravidla výstrahy s webhooku pro volání Slack je k dispozici na [Webhooky ve výstrahách analýzy protokolů](log-analytics-alerts-webhooks.md).

Akce Webhooku vyžadují vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:--- |
| Adresa URL Webhooku |Adresa URL webhooku. |
| Vlastní datovou část JSON |Vlastní datovou část odeslat spolu s webhooku.  Níže naleznete podrobnosti. |


Webhooky zahrnují adresu URL a datovou část ve formátu JSON, který se data odesílají externí služby.  Ve výchozím nastavení budou datové části zahrnují hodnoty v následující tabulce.  Můžete nahradit vlastní jeden vlastní tato datová část.  V takovém případě můžete proměnné v tabulce pro jednotlivé parametry mají být zahrnuty jejich hodnota vaše vlastní datovou část.

| Parametr | Proměnná | Popis |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Název pravidla výstrahy. |
| AlertThresholdOperator |#thresholdoperator |Operátor prahová hodnota pro pravidlo výstrahy.  *Větší než* nebo *menší než*. |
| AlertThresholdValue |#thresholdvalue |Prahová hodnota pro pravidlo výstrahy. |
| LinkToSearchResults |#linktosearchresults |Propojit vyhledávání protokolu analýzy protokolů, který vrátí záznamy v dotazu, který vytvořili výstrahu. |
| Element resultcount element |#searchresultcount |Počet záznamů ve výsledcích hledání. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Koncový čas pro dotaz ve formátu UTC. |
| SearchIntervalInSeconds |#searchinterval |Časový interval pro pravidlo výstrahy. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Počáteční čas pro dotaz ve formátu UTC. |
| SearchQuery |#searchquery |Vyhledávací dotaz protokolu používá pravidlo výstrahy. |
| SearchResults |Níže najdete |Záznamů vrácených dotazem ve formátu JSON.  Omezeno na první 5 000 záznamů. |
| ID pracovního prostoru |#workspaceid |ID pracovního prostoru OMS. |

Například může určit následující vlastní datovou část, která obsahuje jeden parametr s názvem *text*.  Služby, který volá tento webhook by byla očekávána tento parametr.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Tato datová část příkladu by odkazující na něco jako následující odeslání do webhooku.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Zahrnout vlastní datovou část výsledky hledání, přidejte následující řádek jako vlastnost nejvyšší úrovně v datové části json.  

    "IncludeSearchResults":true

Například pokud chcete vytvořit vlastní datovou část, která obsahuje jenom název výstrahy a výsledky hledání, můžete použít následující. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Si můžete projít kompletní příklad vytvoření pravidla výstrahy s webhooku zahájíte externí služba v [vytvoří akci, výstrah webhooku v OMS analýzy protokolů k odeslání zprávy na Slack](log-analytics-alerts-webhooks.md).

## <a name="runbook-actions"></a>Akce sady Runbook
Runbook akce spuštění sady runbook ve službě Azure Automation.  Chcete-li použít tento typ akce, musíte mít [řešení služby Automation](log-analytics-add-solutions.md) nainstalován a nakonfigurován v vaším pracovním prostorem OMS.  Můžete vybrat ze sady runbook do účtu automation, který jste nakonfigurovali v řešení služby Automation.

Akce Runbook vyžadují vlastnosti v následující tabulce.

| Vlastnost | Popis |
|:--- |:---|
| Runbook | Sady Runbook, který chcete spustit v případě, že se vytvoří výstraha. |
| Spusťte na | Zadejte **Azure** ke spuštění runbooku v cloudu.  Zadejte **hybridní pracovní proces** ke spuštění sady runbook na agenta s [hybridní pracovní proces Runbooku](../automation/automation-hybrid-runbook-worker.md ) nainstalována.  |

Akce Runbook spustit pomocí sady runbook [webhooku](../automation/automation-webhooks.md).  Když vytvoříte pravidlo výstrahy, automaticky vytvoří nové webhooku pro sadu runbook s názvem **OMS výstraha nápravy** následuje identifikátor GUID.  

Nelze přímo naplnit žádné parametry sady runbook, ale [parametru $WebhookData](../automation/automation-webhooks.md) bude obsahovat podrobnosti o výstraze, včetně výsledky hledání protokolů, která ji vytvořila.  Sada runbook bude muset definovat **$WebhookData** jako parametr pro ho pro přístup k vlastnostem výstrahy.  Data výstrah je k dispozici ve formátu json v jednom vlastnost s názvem **SearchResults** v **RequestBody** vlastnost **$WebhookData**.  To bude mít s vlastnostmi v následující tabulce.

| Node | Popis |
|:--- |:--- |
| id |Cesta a identifikátor GUID hledání. |
| __metadata |Informace o výstraze, včetně počet záznamů a stav výsledky hledání. |
| hodnota |Samostatný záznam pro každý záznam ve výsledcích hledání.  Podrobnosti o položka bude odpovídat vlastnosti a hodnoty záznamu. |

Například následující runbook by extrahovat záznamy nalezené protokolu a přiřaďte jiné vlastnosti na základě typu každý záznam.  Všimněte si, že sada runbook spustí převedením **RequestBody** z formátu json, které se možné pracovat s jako objekt v prostředí PowerShell.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a>Další kroky
- Dokončete průvodce pro [konfigurace webook](log-analytics-alerts-webhooks.md) s pravidlo výstrahy.  
- Další informace o zápisu [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) k nápravě problémů identifikovaný výstrahy.