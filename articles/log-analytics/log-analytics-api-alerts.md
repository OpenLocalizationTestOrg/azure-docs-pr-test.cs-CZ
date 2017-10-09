---
title: "aaaUsing OMS Log Analytics výstrahy REST API"
description: "Hello Log Analytics výstrahy REST API můžete toocreate a Správa výstrah v analýzy protokolů, která je součástí služby Operations Management Suite (OMS).  Tento článek obsahuje podrobnosti o hello rozhraní API a několik příkladů pro provádění různých akcí."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 418dc7eb71d6151c6380b8925f1f147a0e13b178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Vytvářet a spravovat pravidla výstrah v analýzy protokolů pomocí rozhraní REST API
Hello Log Analytics výstrahy REST API můžete toocreate a Správa výstrah v Operations Management Suite (OMS).  Tento článek obsahuje podrobnosti o hello rozhraní API a několik příkladů pro provádění různých akcí.

Hello Log Analytics vyhledávání REST API je dosáhl standardu RESTful a je přístupný prostřednictvím hello REST API služby Azure Resource Manager. V tomto dokumentu najdete příklady kterých je přístup k hello rozhraní API z příkazového řádku pomocí prostředí PowerShell [ARMClient](https://github.com/projectkudu/ARMClient), nástroj pro příkazový řádek s otevřeným zdrojem, který zjednodušuje vyvolání hello rozhraní API Správce prostředků Azure. použití Hello ARMClient a prostředí PowerShell je jedním z mnoha možností tooaccess hello Log Analytics vyhledávání API. Pomocí těchto nástrojů můžete využívat pracovní prostory hello RESTful API Azure Resource Manager toomake volání tooOMS a provádět příkazy vyhledávání v nich. Hello rozhraní API bude výstup tooyou výsledků vyhledávání ve formátu JSON, což vám výsledky hledání hello toouse mnoha různými způsoby prostřednictvím kódu programu.

## <a name="prerequisites"></a>Požadavky
V současné době mohou výstrahy vytvořeny pouze s uloženého hledání v analýzy protokolů.  Můžete se podívat toohello [rozhraní API REST vyhledávání protokolu](log-analytics-log-search-api.md) Další informace.

## <a name="schedules"></a>Plány
Uložené hledání může mít jeden nebo více plánů. Hello plán definuje jak často hello vyhledávání běží a je identifikován hello časový interval, přes které hello kritéria.
Plány mít hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Interval |Jak často hello vyhledávání běží. Měří v minutách. |
| QueryTimeSpan |Hello časový interval, přes které hello je vyhodnotit kritéria. Musí být rovna tooor větší než Interval. Měří v minutách. |
| Verze |Hello používá verzi rozhraní API.  V současné době to musí být vždy nastavená too1. |

Představte si třeba dotazu událostí s Interval 15 minut a časový interval 30 minut. V takovém případě by být hello dotaz spustit každých 15 minut a výstraha by spustí, pokud hello kritéria dál tooresolve tootrue během určitého 30 minut.

### <a name="retrieving-schedules"></a>Načítání plány
Použití hello získat metoda tooretrieve všechny plány uložených hledání.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Použití hello získat metoda s ID tooretrieve plán konkrétní plán uložených hledání.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Následuje ukázková odpověď pro plán.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Vytvoření plánu
Použijte metodu Put hello s plán jedinečné ID toocreate nový plán.  Všimněte si, že dvě plány nemůže mít hello stejným ID i v případě, že jsou přidruženy různé uložená hledání.  Při vytváření plánu v konzole OMS hello, se vytvoří identifikátor GUID pro ID hello plán.

> [!NOTE]
> Název Hello všechna uložená hledání, plány a akce, které jsou vytvořené pomocí hello Log Analytics API musí být malými písmeny.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Úprava plánu
Použijte metodu Put hello s ID pro hello stejné uložit hledání toomodify, který naplánovat existující plán.  Hello textu hello požadavku musí obsahovat hello etag hello plánu.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Odstraňování plány
Použijte metodu Delete hello s toodelete plán ID plánu.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Akce
Plán může mít více akcí. Akce může definovat jeden nebo více tooperform procesy, jako je například odesílání e-mailu nebo spuštění sady runbook, nebo se může definovat prahové hodnoty, která určuje, kdy hello výsledky hledání kritériím některé.  Některé akce bude definovat i tak, aby procesy hello provede, když je dosaženo prahové hodnoty hello.

Všechny akce mít hello vlastnosti v hello následující tabulka.  Různé typy výstrah mají různé další vlastnosti, které jsou popsány níže.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |Typ akce hello.  Aktuálně hello možné hodnoty jsou výstrahy a Webhooku. |
| Name (Název) |Zobrazovaný název pro hello výstrahu. |
| Verze |Hello používá verzi rozhraní API.  V současné době to musí být vždy nastavená too1. |

### <a name="retrieving-actions"></a>Načítání akce
Použití hello získat metoda tooretrieve všechny akce pro plán.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Použití hello získat metoda s hello akce ID tooretrieve určité akce pro plán.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Vytvořením nebo úpravou akce
Použijte metodu Put hello s ID akce, které je jedinečné toohello plán toocreate novou akci.  Když vytvoříte akce v konzole OMS hello, je identifikátor GUID pro ID hello akce.

> [!NOTE]
> Název Hello všechna uložená hledání, plány a akce, které jsou vytvořené pomocí hello Log Analytics API musí být malými písmeny.

Použijte metodu Put hello s existující akci ID pro hello stejné uložit hledání toomodify, který naplánovat.  Hello textu hello požadavku musí obsahovat hello etag hello plánu.

Hello formát požadavku pro vytvoření nové akce se liší podle typ akce, takže tyto příklady jsou uvedeny v následujících částech hello.

### <a name="deleting-actions"></a>Odstranění akcí
Použijte metodu Delete hello s hello akce ID toodelete akce.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Akce výstrah
Plán by měl mít pouze jeden výstrahy akce.  Akce výstrah minimálně jedna z částí hello v hello následující tabulka.  Každý je podrobně popsaná v další níže.

| Část | Popis |
|:--- |:--- |
| Prahová hodnota |Kritéria pro spuštění akce hello. |
| EmailNotification |Odesílat e-maily toomultiple příjemce. |
| Nápravy |Spuštění sady runbook v Azure Automation tooattempt toocorrect identifikovat problém. |

#### <a name="thresholds"></a>Prahové hodnoty
Výstrahy akce by měl mít pouze jednu prahovou hodnotu.  Pokud výsledky hello uloženého hledání neodpovídají hello prahovou hodnotu v akci spojené s toto hledání, jsou spuštěny žádné další procesy, které jsou v této akce.  Akce může také obsahovat pouze prahovou hodnotu, aby se může použít s akcemi jiných typů, které neobsahují žádný prahové hodnoty.

Prahové hodnoty mají vlastnosti hello v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Operátor |Operátor pro porovnání hello prahovou hodnotu. <br> gt = větší než <br> lt = menší než |
| Hodnota |Hodnota pro mezní hodnotu hello. |

Představte si třeba dotazu událostí se v intervalu 15 minut, časový interval 30 minut a prahové hodnoty větší než 10. V takovém případě by být hello dotaz spustit každých 15 minut a výstraha by spustí, pokud je vrácen 10 události, které byly vytvořeny během určitého 30 minut.

Následuje ukázková odpověď pro akce s pouze prahovou hodnotu.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Použijte metodu Put hello s akce jedinečné ID toocreate novou akci prahové hodnoty pro plán.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Použijte metodu Put hello s existující toomodify ID akce akce prahové hodnoty pro plán.  Hello textu hello požadavku musí obsahovat hello etag hello akce.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>E-mailových oznámení
E-mailová oznámení odesílat e-mailu tooone nebo další příjemce.  Patří mezi ně hello vlastnosti v hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| Příjemce |Seznam adres e-mailu. |
| Předmět |Hello předmět e-mailu hello. |
| Přílohy |Přílohy nejsou aktuálně podporovány, takže to bude mít vždy hodnotu "Žádný". |

Následuje ukázková odpověď pro akci oznámení e-mailu s prahovou hodnotou.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is hello subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Použijte metodu Put hello s akce jedinečné ID toocreate novou akci e-mailu pro plán.  Hello následující příklad vytvoří e-mailové oznámení s prahovou hodnotou, odešle hello e-mailu v případě, že výsledky hello hello uložené hledání překročit prahovou hodnotu hello.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Použijte metodu Put hello s existující toomodify ID akce akce e-mailu pro plán.  Hello textu hello požadavku musí obsahovat hello etag hello akce.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Akce nápravy
Nápravy spuštění sady runbook ve službě Azure Automation, která se pokusí problém hello toocorrect identifikovaný hello výstrahy.  Musíte vytvořit webhooku pro sadu runbook hello používá v rámci nápravy akce a potom zadejte hello URI do hello WebhookUri vlastnost.  Když vytvoříte tuto akci pomocí konzoly OMS hello, se automaticky vytvoří nové webhooku pro sadu runbook hello.

Nápravy zahrnout hello vlastnosti hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| RunbookName |Název sady runbook hello. To se musí shodovat publikované sady runbook v účtu automation hello nakonfigurované v hello řešení služby Automation v pracovním prostoru OMS. |
| WebhookUri |Identifikátor URI služby webhooku hello. |
| Vypršení platnosti |Datum vypršení platnosti Hello a čas webhooku hello.  Pokud hello webhooku nemá vypršení platnosti, může to být žádné platné budoucí datum. |

Následuje ukázková odpověď pro akci automatické nápravy s prahovou hodnotou.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Použijte metodu Put hello s akce jedinečné ID toocreate novou akci automatické nápravy pro plán.  Hello následující příklad vytvoří nápravy s prahovou hodnotu, hello runbook je spuštěn, když hello výsledky hello uložené hledání překročit prahovou hodnotu hello.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Použijte metodu Put hello s existující toomodify ID akce akci automatické nápravy pro plán.  Hello textu hello požadavku musí obsahovat hello etag hello akce.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Příklad
Toto je kompletní příklad toocreate novou e-mailové výstrahy.  Tím se vytvoří nový plán spolu s akce obsahující prahovou hodnotu a e-mailu.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is hello subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Akce Webhooku
Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části toobe, odeslána.  Jsou podobné tooRemediation akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook.  Obsahují taky hello další možnost poskytnout vzdáleného procesu toohello toobe doručovat datové části.

Akce Webhooku nemáte prahovou hodnotu, ale místo toho musí být přidaní tooa plán, který má výstrahy akce s prahovou hodnotou.  Můžete přidat více Webhooku akcí, které se všechny spustí při splnění hello prahovou hodnotu.

Akce Webhooku zahrnout hello vlastnosti hello následující tabulka.

| Vlastnost | Popis |
|:--- |:--- |
| WebhookUri |Hello předmět e-mailu hello. |
| CustomPayload |Webhooku toohello toobe odeslat vlastní datovou část.  Formát Hello bude záviset na jaké hello webhooku očekává. |

Následuje ukázková odpověď pro akce webhooku a přidružené akce výstrah s prahovou hodnotou.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Vytvořte nebo upravte akce webhooku
Použijte metodu Put hello s akce jedinečné ID toocreate Nová akce webhooku pro plán.  Hello následující příklad vytvoří akce Webhooku a výstrah akce s prahovou hodnotu, aby hello webhooku se aktivuje, když hello výsledky hello uložené hledání překročit prahovou hodnotu hello.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Použijte metodu Put hello s existující toomodify ID akce akce webhooku pro plán.  Hello textu hello požadavku musí obsahovat hello etag hello akce.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Další kroky
* Použití hello [REST API tooperform protokolu hledání](log-analytics-log-search-api.md) v analýzy protokolů.

