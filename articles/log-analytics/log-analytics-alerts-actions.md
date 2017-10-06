---
title: aaaResponses tooalerts v OMS Log Analytics | Microsoft Docs
description: "Výstrahy v analýzy protokolů zjišťovat důležité informace ve svém úložišti OMS a zda lze proaktivně upozorňují na problémy nebo vyvolání akce tooattempt toocorrect je.  Tento článek popisuje, jak toocreate pravidlo výstrahy a podrobnosti hello různé akce jejich trvat."
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
ms.openlocfilehash: d24bb726a96e7143985f111c0599dc4e7898b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-actions-tooalert-rules-in-log-analytics"></a>Přidání pravidla tooalert akce v analýzy protokolů
Když [výstraha je vytvořen v analýzy protokolů](log-analytics-alerts.md), máte možnost hello [konfiguraci pravidlo výstrahy hello](log-analytics-alerts.md) tooperform jednu nebo více akcí.  Tento článek popisuje hello různé akce, které jsou k dispozici a podrobnosti o konfiguraci jednotlivých typů.

| Akce | Popis |
|:--|:--|
| [E-mailu](#email-actions) | Odeslat e-mail s hello podrobností výstrahy tooone hello nebo další příjemce. |
| [Webhooku](#webhook-actions) | Vyvolání externího procesu prostřednictvím jedné žádosti HTTP POST. |
| [Sady Runbook](#runbook-actions) | Spuštění sady runbook ve službě Azure Automation. |


## <a name="email-actions"></a>Akce e-mailu
E-mailu akce Odeslat e-mail s hello podrobností výstrahy tooone hello nebo další příjemce.  Můžete zadat hello předmět e-mailu hello, ale jeho obsah je standardní formát vytvořená pomocí analýzy protokolů.  V přidání toodetails z až tooten záznamů vrácených hledání protokolů hello obsahuje souhrnné informace, jako je název hello hello výstrahy.  Zahrnuje také odkaz tooa protokolu vyhledávání v analýzy protokolů, který vrátí hello celou sadu záznamů z tohoto dotazu.   Odesílatel Hello hello pošty je *týmu Microsoft Operations Management Suite &lt; noreply@oms.microsoft.com &gt;* . 

E-mailu akce vyžadují hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Předmět |Subjektu v e-mailu hello.  Nelze upravit hello textu hello pošty. |
| Příjemce |Adresy všech příjemců e-mailu.  Pokud zadáte víc než jednou adresou pak samostatné hello adresy oddělte středníkem (;). |


## <a name="webhook-actions"></a>Akce Webhooku

Akce Webhooku povolit tooinvoke externího procesu prostřednictvím jedné žádosti HTTP POST.  Služba Hello volané by měla podporovat webhooky a určit, jak se bude používat žádné datové části obdrží.  Může také zavolat REST API, která nepodporuje konkrétně webhooků, dokud hello požadavku je ve formátu, že hello rozumí rozhraní API.  Příklady použití webhook, jehož v odpovědi tooan výstrahy jsou odesílání zprávy v [Slack](http://slack.com) nebo vytváření incidentu v [PagerDuty](http://pagerduty.com/).  Kompletní a podrobný postup vytvoření pravidla výstrahy s webhooku toocall Slack je k dispozici na [Webhooky ve výstrahách analýzy protokolů](log-analytics-alerts-webhooks.md).

Akce Webhooku vyžadují hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Adresa URL Webhooku |Hello adresa URL webhooku hello. |
| Vlastní datovou část JSON |Vlastní datovou část toosend s webhooku hello.  Níže naleznete podrobnosti. |


Webhooky přidat adresu URL a datovou část ve formátu JSON, který je hello data odeslána toohello externí služby.  Ve výchozím nastavení datová část hello obsahovat hello hodnoty v hello následující tabulka.  Můžete tooreplace tato datová část s vlastní jeden vlastní.  V takovém případě můžete použít hello proměnné v tabulce hello pro jednotlivé parametry tooinclude hello jejich hodnotu ve vaší vlastní datovou část.

| Parametr | Proměnná | Popis |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Název pravidla výstrahy hello. |
| AlertThresholdOperator |#thresholdoperator |Operátor prahová hodnota pro pravidlo výstrahy hello.  *Větší než* nebo *menší než*. |
| AlertThresholdValue |#thresholdvalue |Prahová hodnota pro pravidlo výstrahy hello. |
| LinkToSearchResults |#linktosearchresults |Odkaz tooLog Analytics protokolu vyhledávání, který vrátí hello záznamů z hello dotazu, který vytvořili výstrahu hello. |
| Element resultcount element |#searchresultcount |Počet záznamů ve výsledcích hledání hello. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Koncový čas pro dotaz hello ve formátu UTC. |
| SearchIntervalInSeconds |#searchinterval |Časový interval pro pravidlo výstrahy hello. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Počáteční čas pro hello dotazu ve formátu UTC. |
| SearchQuery |#searchquery |Vyhledávací dotaz protokolu používá pravidlo výstrahy hello. |
| SearchResults |Níže najdete |Záznamů vrácených dotazem hello ve formátu JSON.  Omezené toohello první 5 000 záznamů. |
| ID pracovního prostoru |#workspaceid |ID pracovního prostoru OMS. |

Například mohla uvádět hello následující vlastní datovou část, která obsahuje jeden parametr s názvem *text*.  Hello služby, který volá tento webhook by byla očekávána tento parametr.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Tato datová část příkladu by vyřešit toosomething jako hello následující při odesílání toohello webhooku.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

tooinclude výsledků vyhledávání ve vlastní datovou část, přidejte následující řádek jako vlastnost nejvyšší úrovně v datové části json hello hello.  

    "IncludeSearchResults":true

Například toocreate vlastní datové části, která obsahuje jenom hello název výstrahy a výsledky hledání hello, můžete použít následující hello. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Si můžete projít kompletní příklad vytvoření pravidla výstrahy s webhook toostart služby externí v [vytvoří akci, výstrah webhooku v OMS Log Analytics toosend zpráva tooSlack](log-analytics-alerts-webhooks.md).

## <a name="runbook-actions"></a>Akce sady Runbook
Runbook akce spuštění sady runbook ve službě Azure Automation.  V pořadí toouse tento typ akce, musí mít hello [řešení služby Automation](log-analytics-add-solutions.md) nainstalován a nakonfigurován v vaším pracovním prostorem OMS.  Můžete vybrat ze sady runbook hello v hello účet automation, který jste nakonfigurovali v hello řešení služby Automation.

Akce Runbook vyžadují hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:---|
| Runbook | Sada Runbook má toostart, když se vytvoří výstraha. |
| Spusťte na | Zadejte **Azure** toorun hello runbooku v cloudu hello.  Zadejte **hybridní pracovní proces** toorun hello runbook na agenta s [hybridní pracovní proces Runbooku](../automation/automation-hybrid-runbook-worker.md ) nainstalována.  |

Akce Runbook spustit hello runbooku pomocí [webhooku](../automation/automation-webhooks.md).  Když vytvoříte pravidlo výstrahy hello, automaticky vytvoří nové webhooku hello sady runbook s názvem hello **OMS výstraha nápravy** následuje identifikátor GUID.  

Nemůžete přímo naplnit žádné parametry hello sady runbook, ale hello [parametru $WebhookData](../automation/automation-webhooks.md) zahrne podrobnosti hello hello výstrahy, včetně hello výsledky hledání hello protokolů, která ji vytvořila.  Hello runbook bude nutné toodefine **$WebhookData** jako parametr pro něj tooaccess hello vlastnosti hello výstrahy.  Hello dat výstrah je k dispozici ve formátu json v jednom vlastnost s názvem **SearchResults** v hello **RequestBody** vlastnost **$WebhookData**.  To bude mít s vlastnostmi hello v hello následující tabulka.

| Node | Popis |
|:--- |:--- |
| id |Cesta a identifikátor GUID hello hledání. |
| __metadata |Informace o hello výstrahy včetně hello počet záznamů a stav hello výsledky hledání. |
| hodnota |Samostatný záznam pro každý záznam ve výsledcích hledání hello.  Podrobnosti o Hello hello položky bude odpovídat hello vlastnosti a hodnoty hello záznamu. |

Například hello následující runbook by extrahovat hello záznamů vrácených hledání hello protokolů a přiřaďte jiné vlastnosti na základě typu hello každý záznam.  Všimněte si dané sady runbook hello začne převádění **RequestBody** z formátu json, které se možné pracovat s jako objekt v prostředí PowerShell.

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
- Zjistěte, jak toowrite [sady runbook ve službě Azure Automation](https://azure.microsoft.com/documentation/services/automation) tooremediate problémy identifikovat pomocí výstrah.
