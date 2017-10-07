---
title: "aaaStarting runbook služby automatizace Azure s webhook, jehož | Microsoft Docs"
description: "Webhook, která umožňuje toostart klienta sady runbook ve službě Azure Automation z volání protokolu HTTP.  Tento článek popisuje, jak toocreate webhook, jehož a jak toocall jeden toostart sady runbook."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9b20237c-a593-4299-bbdc-35c47ee9e55d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: magoedte;bwren;sngun
ms.openlocfilehash: ca6cde66b3784ceb5d0bc5921cee87aea74cb150
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Počínaje webhook, jehož runbook služby automatizace Azure
A *webhooku* vám umožní toostart konkrétní runbook ve službě Azure Automation prostřednictvím jedné žádosti HTTP. To umožňuje externích služeb, jako je například Visual Studio Team Services, GitHub, analýzy protokolů Microsoft Operations Management Suite nebo vlastních aplikací toostart runbooků bez implementace úplné řešení pomocí hello rozhraní API služby Azure Automation.  
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Můžete porovnat webhooky tooother metody spuštění sady runbook [spuštění sady runbook ve službě Azure Automation](automation-starting-a-runbook.md)

## <a name="details-of-a-webhook"></a>Podrobnosti o webhook, jehož
Hello následující tabulka popisuje hello vlastnosti, které je nutné nakonfigurovat webhooku.

| Vlastnost | Popis |
|:--- |:--- |
| Name (Název) |Můžete zadat, že libovolný název, který chcete použít pro webhook, jehož vzhledem k tomu, že toto nejsou viditelné toohello klienta.  Toto pravidlo se používá pouze pro jste tooidentify hello sady runbook ve službě Azure Automation. <br>  Jako osvědčený postup musíte získat hello webhooku klienta související toohello název, který bude používat. |
| ADRESA URL |Adresa URL webhooku hello Hello je toohello webhooku propojené hello jedinečnou adresu, klient volá s runbook hello toostart služby HTTP POST.  Generuje se automaticky při vytvoření webhooku hello.  Nelze zadat vlastní adresu URL. <br> <br>  Adresa URL Hello obsahuje token zabezpečení, který umožňuje toobe hello runbook vyvolané systémem třetích stran s další ověřování. Z tohoto důvodu by zpracovávat jako heslo.  Z bezpečnostních důvodů můžete pouze hello zobrazení, které se vytvoří adresu URL v portálu Azure v hello čas hello webhooku hello. Upozorňujeme ale, adresa URL hello na bezpečné místo pro budoucí použití. |
| Datum vypršení platnosti |Stejně jako certifikát má každý webhooku datum vypršení platnosti, po kterém již slouží.  Toto datum vypršení platnosti můžete změnit po vytvoření webhooku hello. |
| Povoleno |Webhook, jehož je ve výchozím nastavení povolena, když je vytvořeno.  Pokud ji nastavíte tooDisabled, pak žádný klient bude mít toouse ho.  Můžete nastavit hello **povoleno** vlastnost při vytváření webhooku hello nebo kdykoli po jeho vytvoření. |

### <a name="parameters"></a>Parametry
Webhook, jehož můžete definovat hodnoty pro parametry runbooku, které se použijí při spuštění runbooku hello podle tohoto webhooku. Hello webhooku musí obsahovat hodnoty všech povinných parametrů hello runbooku a můžou obsahovat hodnoty pro volitelné parametry. Webhook, jehož parametr nakonfigurovaná hodnota tooa můžete změnit i po vytvoření hello webhoook. Více webhooky propojené tooa jedné sady runbook můžete použít jiné hodnoty parametru.

Při spuštění sady runbook pomocí webhook, jehož klienta, je nejde přepsat hello hodnot parametrů definovaných v webhooku hello.  tooreceive data z klienta hello, hello runbook může přijmout jeden parametr s názvem **$WebhookData** typu [object], která bude obsahovat data tohoto klienta hello zahrnuje v požadavku POST hello.

![Vlastnosti Webhookdata](media/automation-webhooks/webhook-data-properties.png)

Hello **$WebhookData** objektu bude mít hello následující vlastnosti:

| Vlastnost | Popis |
|:--- |:--- |
| WebhookName |Název webhooku hello Hello. |
| RequestHeader |Hodnota hash tabulku obsahující hello hlavičky hello příchozí požadavek POST. |
| RequestBody |Hello textu hello příchozích požadavků POST.  To zachová veškeré formátování, jako je například řetězec, formát JSON, XML, nebo data formuláře kódovaná v řetězci. Hello runbook musí být napsané toowork s hello data formátu, který se očekává. |

Neexistuje žádná konfigurace hello webhook vyžaduje toosupport hello **$WebhookData** parametr, a hello runbook není požadovaná tooaccept ho.  Pokud hello runbook nedefinuje hello parametr, žádné podrobnosti požadavku hello odeslaný klientem hello ignorován.

Pokud zadáte hodnotu pro $WebhookData při vytváření webhooku hello, tato hodnota bude elementem při hello webhooku spuštění sady runbook hello hello daty z požadavku POST hello klienta, i když hello klienta neobsahuje žádná data v textu žádosti hello.  Pokud spustíte runbook, který má $WebhookData pomocí jiné metody než webhook, jehož, je zadat hodnotu pro $Webhookdata, rozpozná pomocí hello runbook.  Tato hodnota by měla být objekt s hello stejné [vlastnosti](#details-of-a-webhook) jako $Webhookdata, jako kdyby byla práce s skutečné WebhookData předaná webhook, jehož mohli dané sady runbook hello správně pracovat s ním.

Například pokud začínáte hello následujících runbook z hello portálu Azure a chcete toopass některé ukázkové WebhookData pro testování, protože WebhookData je objekt, se mají být předány jako JSON v hello uživatelského rozhraní.

![Parametr WebhookData z uživatelského rozhraní](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Pro hello výše sady runbook, pokud máte hello následující vlastnosti pro parametr WebhookData hello:

1. WebhookName: *MyWebhook*
2. RequestHeader: *z = testovacího uživatele*
3. RequestBody: *["VM1", "Virtuálního počítače 2"]*

Potom by úspěšně prošel zpracováním hello následující hodnota JSON v hello uživatelského rozhraní pro parametr WebhookData hello:  

* {"WebhookName": "MyWebhook", "RequestHeader": {"Od": "Testovací uživatele"}, "RequestBody": "[\"VM1\",\"virtuálního počítače 2\"]"}

![Parametr WebhookData zahájení z uživatelského rozhraní](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Hello hodnoty všech vstupních parametrů se protokolují pod hello úlohy runbooku.  To znamená, že žádný vstup hello klientem v požadavku webhooku hello bude protokolu a k dispozici tooanyone s úlohu automatizace toohello přístup.  Z tohoto důvodu byste měli být opatrní při voláních webhooku včetně citlivé informace.
>

## <a name="security"></a>Zabezpečení
Hello zabezpečení webhook, jehož spoléhá na hello soukromí jeho adresa URL, který obsahuje token zabezpečení, který umožní toobe vyvolat. Služby Azure Automation neprovede žádné ověřování na požádání hello tak dlouho, dokud je k toohello správnou adresu URL. Z tohoto důvodu webhooky nepoužívejte pro sady runbook, které provádějí vysoce citlivých funkce bez použití alternativní způsob ověřování hello požadavku.

Můžete zahrnout logiky v rámci hello runbook toodetermine webhook, jehož ho byl volány kontrolou hello **WebhookName** vlastnosti parametru hello $WebhookData. Hello runbook může provést další ověření tak, že vyhledá konkrétní informace v hello **RequestHeader** nebo **RequestBody** vlastnosti.

Další strategie je toohave hello runbook provést některé ověření externí podmínka v případě, že přijala žádost o webhooku.  Představte si třeba sadu runbook, která je volána metodou Githubu, kdykoli dojde k nové úložiště GitHub tooa potvrzení.  Hello runbook se může připojit toovalidate tooGitHub, které nové potvrzení měl ve skutečnosti jenom došlo než budete pokračovat.

## <a name="creating-a-webhook"></a>Vytváření webhooku
Použijte následující postup toocreate nová sada runbook tooa webhooku propojené v hello portál Azure hello.

1. Z hello **sady Runbook okno** v hello portálu Azure, klikněte na hello runbook, který hello webhooku se spustí tooview její okno podrobností.
2. Klikněte na tlačítko **Webhooku** hello horní části hello okno tooopen hello **přidat Webhooku** okno. <br>
   ![Tlačítko Webhooky](media/automation-webhooks/webhooks-button.png)
3. Klikněte na tlačítko **vytvořit nové webhooku** tooopen hello **okno Vytvoření webhooku**.
4. Zadejte **název**, **datum vypršení platnosti** hello webhooku a jestli se má povolit. V tématu [podrobnosti o webhook, jehož](#details-of-a-webhook) pro další informace o těchto vlastností.
5. Klikněte na ikonu kopírování hello a stiskněte kombinaci kláves Ctrl + C toocopy hello URL webhooku hello.  Potom zaznamenejte jej na bezpečném místě.  **Po vytvoření webhooku hello nelze znovu načíst hello URL.** <br>
   ![Adresa URL Webhooku](media/automation-webhooks/copy-webhook-url.png)
6. Klikněte na tlačítko **parametry** tooprovide hodnoty pro parametry runbooku hello.  Pokud má hello runbook povinné parametry, pak nebude možné toocreate hello webhooku Pokud hodnoty jsou k dispozici.
7. Klikněte na tlačítko **vytvořit** webhooku toocreate hello.

## <a name="using-a-webhook"></a>Pomocí webhook, jehož
toouse webhooku po jeho vytvoření klientské aplikace musíte vydat HTTP POST s adresou URL hello webhooku hello.  Syntaxe Hello hello webhooku bude v hello formátu.

    http://<Webhook Server>/token?=<Token Value>

Hello klienta se zobrazí jednu z následujících návratové kódy z požadavku POST hello hello.  

| Kód | Text | Popis |
|:--- |:--- |:--- |
| 202 |Přijmout |Hello žádost byla přijata, a hello runbook byla úspěšně zařazených do fronty. |
| 400 |Chybný požadavek |Hello požadavku nebyl přijat pro jednu z následujících důvodů hello. <ul> <li>Hello webhooku vypršela platnost.</li> <li>Hello webhooku je zakázána.</li> <li>token Hello v adrese URL hello je neplatný.</li>  </ul> |
| 404 |Nebyl nalezen |Hello požadavku nebyl přijat pro jednu z následujících důvodů hello. <ul> <li>Hello webhooku nebyl nalezen.</li> <li>Hello runbook nebyl nalezen.</li> <li>nebyl nalezen účet Hello.</li>  </ul> |
| 500 |Vnitřní chyba serveru |Adresa URL Hello byla platná, ale došlo k chybě.  Odešlete žádost hello. |

Za předpokladu, že je požadavek hello úspěšné, hello webhooku odpovědi obsahuje id úlohy hello ve formátu JSON následujícím způsobem. Bude obsahovat jednu úlohu s id, ale umožňuje potenciální budoucí vylepšení hello formátu JSON.

    {"JobIds":["<JobId>"]}  

Hello klienta nemůže zjistit po dokončení úlohy runbooku hello nebo její stav dokončení od webhooku hello.  Může zjistit, tyto informace id úlohy hello pomocí jiné metody, jako [prostředí Windows PowerShell](http://msdn.microsoft.com/library/azure/dn690263.aspx) nebo hello [rozhraní API služby Azure Automation](https://msdn.microsoft.com/library/azure/mt163826.aspx).

### <a name="example"></a>Příklad
Následující ukázka Hello používá prostředí Windows PowerShell toostart sady runbook s webhook, jehož.  Upozorňujeme, že jakýkoli jazyk, který může odeslat požadavek HTTP, můžete použít webhooku; Prostředí Windows PowerShell se právě používá jako příklad sem.

Hello runbook očekává seznam virtuálních počítačů, které jsou ve formátu JSON v textu hello hello požadavku. Můžeme také jsou včetně informací o který zahajuje hello runbook a hello datum a čas, že je právě spuštěna v záhlaví hello hello požadavku.      

    $uri = "https://s1events.azure-automation.net/webhooks?token=8ud0dSrSo%2fvHWpYbklW%3c8s0GrOKJZ9Nr7zqcS%2bIQr4c%3d"
    $headers = @{"From"="user@contoso.com";"Date"="05/28/2015 15:47:00"}

    $vms  = @(
                @{ Name="vm01";ServiceName="vm01"},
                @{ Name="vm02";ServiceName="vm02"}
            )
    $body = ConvertTo-Json -InputObject $vms

    $response = Invoke-RestMethod -Method Post -Uri $uri -Headers $headers -Body $body
    $jobid = ConvertFrom-Json $response


Hello následující obrázek znázorňuje informace hlavičky hello (pomocí [Fiddler](http://www.telerik.com/fiddler) trasování) z této žádosti. To zahrnuje standardní hlavičky požadavku HTTP v přidání toohello vlastní hodnota data a z hlavičky, které jsme přidali.  Každý z těchto hodnot je k dispozici toohello runbook v hello **RequestHeaders** vlastnost **WebhookData**.

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-headers.png)

Hello následující obrázek ukazuje hello textu hello požadavku (pomocí [Fiddler](http://www.telerik.com/fiddler) trasování) který je k dispozici toohello runbook v hello **RequestBody** vlastnost **WebhookData**. To je naformátován jako JSON, protože hello formátu, který je zahrnutý v textu hello hello požadavku, který byl.     

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-body.png)

Hello následující obrázek znázorňuje hello požadavek odesílány z prostředí Windows PowerShell a výsledné odpovědi hello.  id úlohy Hello je extrahovat z řetězec převedený tooa a hello odpovědi.

![Tlačítko Webhooky](media/automation-webhooks/webhook-request-response.png)

Hello následující vzorový runbook přijímá hello předchozí příklad požadavek a spustí hello virtuální počítače zadaný v textu žádosti hello.

    workflow Test-StartVirtualMachinesFromWebhook
    {
        param (
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {

            # Collect properties of WebhookData
            $WebhookName     =     $WebhookData.WebhookName
            $WebhookHeaders =     $WebhookData.RequestHeader
            $WebhookBody     =     $WebhookData.RequestBody

            # Collect individual headers. VMList converted from JSON.
            $From = $WebhookHeaders.From
            $VMList = ConvertFrom-Json -InputObject $WebhookBody
            Write-Output "Runbook started from webhook $WebhookName by $From."

            # Authenticate tooAzure resources
            $Cred = Get-AutomationPSCredential -Name 'MyAzureCredential'
            Add-AzureAccount -Credential $Cred

            # Start each virtual machine
            foreach ($VM in $VMList)
            {
                $VMName = $VM.Name
                Write-Output "Starting $VMName"
                Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName
            }
        }
        else {
            Write-Error "Runbook mean toobe started only from webhook."
        }
    }


## <a name="starting-runbooks-in-response-tooazure-alerts"></a>Spouštění sad runbook v odpovědi tooAzure výstrahy
Webhooku povoleno sady runbook může být příliš použité tooreact[Azure výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Shromažďování statistik hello jako výkon, dostupnost a využití hello pomoci Azure výstrah můžete sledovat prostředky v Azure. Obdržíte výstrahu založenou na monitorování metriky nebo události pro vaše prostředky Azure, účty služby Automation aktuálně podporují pouze metriky. Pokud hello hodnota zadané metriky překročí prahovou hodnotu hello přiřazené nebo pokud hello nakonfigurované událost se aktivuje, pak jsou oznámení odesílána toohello služby správce nebo spolusprávci tooresolve hello výstraha, další informace o metrikách a události naleznete příliš[ Azure výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Kromě použití Azure výstrahy jako systém oznámení, můžete také ji sady runbook v tooalerts odpovědi. Azure Automation nabízí hello schopností toorun webhooku povoleno sady runbook pomocí Azure výstrahy. Pokud překročí metriky hello nakonfigurovat prahová hodnota pravidlo výstrahy hello stane aktivní a aktivační události hello webhook automatizace, který pak provede hello runbook.

![Webhooky](media/automation-webhooks/webhook-alert.jpg)

### <a name="alert-context"></a>Kontext výstrahy
Vezměte v úvahu prostředek služby Azure, jako je například virtuální počítač, využití procesoru tohoto počítače je jedním z klíčových metrika hello. Pokud hello využití procesoru je 100 % nebo více než určité dlouhou dobu, můžete chtít toorestart hello virtuálního počítače toofix hello problém. To lze vyřešit tak, že nakonfigurujete se pravidlo výstrahy toohello virtuální počítač a toto pravidlo trvá procento využití procesoru jako jeho metriku. Procento využití procesoru v tomto poli je právě prováděné jako příklad, ale existuje mnoho metriky tooyour Azure můžete nakonfigurovat prostředky a restartování virtuálního počítače hello je akce, která je provedena toofix tento problém hello runbook tootake můžete nakonfigurovat další akce.

Když pravidlo výstrahy hello stane aktivní a hello aktivační události webhooku povoleno sady runbook, odešle runbook toohello hello kontext výstrahy. [Kontext výstrahy](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) obsahuje podrobnosti, včetně **SubscriptionID**, **ResourceGroupName**, **ResourceName**, **ResourceType**, **ResourceId** a **časové razítko** které jsou požadovány pro hello runbook tooidentify hello prostředků, na kterém je provedením akce. Výstrahy kontextu vložené v části textu hello hello **WebhookData** objektu odeslané toohello runbook a je přístupný pomocí **Webhook.RequestBody** vlastnost

### <a name="example"></a>Příklad
Vytvoření virtuálního počítače Azure ve vašem předplatném a přidružení [výstrahy toomonitor procesoru procento metrika](../monitoring-and-diagnostics/insights-receive-alert-notifications.md). Při vytváření hello výstrahy zkontrolujte, zda že naplnit hello webhooku pole s adresou URL hello hello webhooku, který byl vygenerován při vytváření webhooku hello.

Hello následujícím vzorovém runbooku se aktivuje, když pravidlo výstrahy hello stane aktivní a shromáždí hello kontext výstrahy parametry, které jsou vyžadovány pro hello runbook tooidentify hello prostředků, na kterém je provedením akce.

    workflow Invoke-RunbookUsingAlerts
    {
        param (      
            [object]$WebhookData
        )

        # If runbook was called from Webhook, WebhookData will not be null.
        if ($WebhookData -ne $null) {   
            # Collect properties of WebhookData.
            $WebhookName    =   $WebhookData.WebhookName
            $WebhookBody    =   $WebhookData.RequestBody
            $WebhookHeaders =   $WebhookData.RequestHeader

            # Outputs information on hello webhook name that called This
            Write-Output "This runbook was started from webhook $WebhookName."


            # Obtain hello WebhookBody containing hello AlertContext
            $WebhookBody = (ConvertFrom-Json -InputObject $WebhookBody)
            Write-Output "`nWEBHOOK BODY"
            Write-Output "============="
            Write-Output $WebhookBody

            # Obtain hello AlertContext     
            $AlertContext = [object]$WebhookBody.context

            # Some selected AlertContext information
            Write-Output "`nALERT CONTEXT DATA"
            Write-Output "==================="
            Write-Output $AlertContext.name
            Write-Output $AlertContext.subscriptionId
            Write-Output $AlertContext.resourceGroupName
            Write-Output $AlertContext.resourceName
            Write-Output $AlertContext.resourceType
            Write-Output $AlertContext.resourceId
            Write-Output $AlertContext.timestamp

            # Act on hello AlertContext data, in our case restarting hello VM.
            # Authenticate tooyour Azure subscription using Organization ID toobe able toorestart that Virtual Machine.
            $cred = Get-AutomationPSCredential -Name "MyAzureCredential"
            Add-AzureAccount -Credential $cred
            Select-AzureSubscription -subscriptionName "Visual Studio Ultimate with MSDN"

            #Check hello status property of hello VM
            Write-Output "Status of VM before taking action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
            Write-Output "Restarting VM"

            # Restart hello VM by passing VM name and Service name which are same in this case
            Restart-AzureVM -ServiceName $AlertContext.resourceName -Name $AlertContext.resourceName
            Write-Output "Status of VM after alert is active and takes action"
            Get-AzureVM -Name $AlertContext.resourceName -ServiceName $AlertContext.resourceName
        }
        else  
        {
            Write-Error "This runbook is meant tooonly be started from a webhook."  
        }  
    }



## <a name="next-steps"></a>Další kroky
* Podrobnosti na různé způsoby toostart sady runbook najdete v tématu [spuštění sady Runbook](automation-starting-a-runbook.md).
* Informace o hello zobrazení stavu úlohy Runbooku, najdete v části příliš[spuštění sady Runbook ve službě Azure Automation](automation-runbook-execution.md).
* toolearn jak toouse akce tootake Azure Automation v Azure výstrahy, najdete v části [napravit výstrahy virtuálních počítačů Azure pomocí Runbooků Automation](automation-azure-vm-alert-integration.md).
