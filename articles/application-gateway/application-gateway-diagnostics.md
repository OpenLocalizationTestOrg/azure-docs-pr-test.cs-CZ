---
title: "aaaMonitor přístup protokoly, protokoly výkonu, back-end stavu a metrik pro službu Application Gateway | Microsoft Docs"
description: "Zjistěte, jak tooenable a spravovat přístup k protokolům a protokolování výkonu pro službu Application Gateway"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 36ebf15c28f776158350ef8e73d617ef68e09266
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Stav back-end, diagnostické protokoly a metriky pro službu Application Gateway

Pomocí Azure Application Gateway můžete sledovat prostředky v hello následující způsoby:

* [Back-end stavu](#back-end-health): Application Gateway poskytuje hello schopností toomonitor hello hello stav serverů stavu v hello back endové fondy prostřednictvím hello portál Azure a pomocí prostředí PowerShell. Můžete také získat stav hello hello back endové fondy prostřednictvím hello výkonu diagnostické protokoly.

* [Protokoly](#diagnostic-logs): protokoly umožňují pro výkon, přístup, a další data toobe uložit, nebo využívat z prostředku pro účely monitorování.

* [Metriky](#metrics): Aplikační brána má aktuálně jeden metriku. Tato metrika měří hello propustnost hello Aplikační brána v bajtech za sekundu.

## <a name="back-end-health"></a>Back-end stavu

Application Gateway poskytuje hello schopností toomonitor hello stavu jednotlivých členů fondu back-end hello prostřednictvím portálu hello, prostředí PowerShell a hello rozhraní příkazového řádku (CLI). Můžete také vyhledat agregovaný stav souhrn back endové fondy prostřednictvím hello výkonu diagnostické protokoly. 

Sestava stavu back-end Hello odráží výstup hello instancí hello Application Gateway stavu testu toohello back-end. Při zjišťování proběhlo úspěšně a hello zpět end může přijímat provoz, bude považován za v pořádku. Jinak považuje není v pořádku.

> [!IMPORTANT]
> Pokud je skupina zabezpečení sítě (NSG) na podsíť aplikační bránu, otevřete rozsahy portů 65503 65534 v podsíti hello aplikační brány pro příchozí provoz. Tyto porty jsou povinné pro toowork rozhraní API back-end stavu hello.


### <a name="view-back-end-health-through-hello-portal"></a>Zobrazit stav back-end prostřednictvím portálu hello

Back-end stavu hello portál, je zadán automaticky. V existující aplikační brány, vyberte **monitorování** > **back-end stavu**. 

Každý člen ve fondu back-end hello je uvedený na této stránce (jestli je síťový adaptér, IP nebo plně kvalifikovaný název domény). Název fondu back-end, port, název nastavení HTTP back-end a stav se zobrazí. Platné hodnoty pro stav jsou **stavu v pořádku**, **není v pořádku**, a **neznámé**.

> [!NOTE]
> Pokud se zobrazí back-end stav **neznámé**, ujistěte se, že přístup toohello back-end není blokován nastavením pravidlo NSG, trasy definované uživatelem (UDR) nebo vlastní DNS ve virtuální síti hello.

![Back-end stavu][10]

### <a name="view-back-end-health-through-powershell"></a>Zobrazit stav back-end pomocí prostředí PowerShell

Hello následující kódu PowerShell ukazuje, jak tooview stavu back-end pomocí hello `Get-AzureRmApplicationGatewayBackendHealth` rutiny:

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a>Zobrazit stav back-end pomocí Azure CLI 2.0

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Výsledky

Hello následující fragment kódu ukazuje příklad hello odpovědi:

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Diagnostické protokoly

Můžete použít různé typy protokolů v Azure toomanage a řešení potíží s application Gateway. Některé z těchto protokolů můžete přistupovat prostřednictvím portálu hello. Všechny protokoly lze extrahovat z Azure Blob storage a zobrazit v různých nástrojů, jako například [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md), Excel a Power BI. Další informace o různých typech hello protokolů z hello následující seznamu:

* **Protokol aktivit**: můžete použít [protokoly Azure aktivity](../monitoring-and-diagnostics/insights-debugging-with-events.md) (dříve označovaný jako operační protokoly a protokoly auditu) tooview všechny operace, které jsou odeslána tooyour předplatného Azure a jejich stav. Ve výchozím nastavení se shromažďují položky protokolu aktivity a lze je zobrazit v hello portálu Azure.
* **Přístup k protokolu**: můžete použít tento protokol tooview Application Gateway přístupové vzorce a analýza důležité informace, včetně hello volající IP, požadovanou adresu URL, latence odpovědi, návratový kód a bajtů a odhlášení. Protokol přístupu se shromažďují každých 300 sekund. Tento protokol obsahuje jeden záznam za instance aplikační brány. instance brány aplikace Hello lze identifikovat podle vlastnosti instanceId hello.
* **Výkon protokolu**: můžete použít tento tooview protokolu výkonu instance aplikační brány. Tento protokol zaznamená informace o výkonu pro každou instanci, včetně celkový počet požadavků zpracovaných, propustnost v bajtech, celkový počet požadavků zpracovaných počet chybných požadavků a počet instancí back-end v pořádku a není v pořádku. Protokolu výkonu shromažďovaných každých 60 sekund.
* **Protokol brány firewall**: můžete použít tento protokolu tooview hello žádosti, které se protokolují prostřednictvím zjišťování nebo zabránění režim služby application gateway, která je konfigurovaná pomocí brány firewall webových aplikací hello.

> [!NOTE]
> Protokoly jsou k dispozici pouze pro prostředky nasazené v modelu nasazení Azure Resource Manager hello. Protokoly nelze použít pro prostředky v modelu nasazení classic hello. Lépe porozumět hello dva modely, najdete v části hello [nasazení Resource Manager principy a nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md) článku.

Máte tři možnosti pro ukládání protokolů:

* **Účet úložiště**: účty úložiště jsou nejvhodnější pro protokoly při protokoly jsou uloženy delší dobu a zkontrolovat v případě potřeby.
* **Služba Event hubs**: Event hubs je skvělou možnost pro integraci s další informace o zabezpečení a tooget výstrahy na vaše prostředky nástroje pro správu událostí (SEIM).
* **Analýza protokolu**: analýzy protokolů je nejvhodnější pro obecné sledování v reálném čase vaší aplikace nebo při prohlížení trendy.

### <a name="enable-logging-through-powershell"></a>Povolit protokolování pomocí prostředí PowerShell

Pro každý prostředek Resource Manager je automaticky povolené protokolování aktivit. Je nutné povolit přístup a toostart protokolování výkonu shromažďování dat hello k dispozici prostřednictvím tyto protokoly. tooenable protokolování, použijte hello následující kroky:

1. Poznamenejte si ID prostředku účtu úložiště, které jsou uložená data protokolu hello. Tato hodnota je ve formátu hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Storage/storageAccounts/\<název účtu úložiště\>. Můžete použít libovolný účet úložiště v rámci vašeho předplatného. Tyto informace můžete použít hello Azure toofind portálu.

    ![Portál: ID prostředku pro účet úložiště](./media/application-gateway-diagnostics/diagnostics1.png)

2. Poznamenejte si ID prostředku Aplikační brána, pro které je povoleno protokolování. Tato hodnota je ve formátu hello: /subscriptions/\<subscriptionId\>/resourceGroups/\<název skupiny prostředků\>/providers/Microsoft.Network/applicationGateways/\<aplikační brány název\>. Tyto informace můžete použít portál toofind hello.

    ![Portál: ID prostředku application Gateway.](./media/application-gateway-diagnostics/diagnostics2.png)

3. Povolte protokolování diagnostiky pomocí hello následující rutiny prostředí PowerShell:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Protokoly aktivity nevyžadují, aby účet samostatného úložiště. použití Hello úložiště pro přístup a protokolování výkonu způsobuje poplatky za služby.

### <a name="enable-logging-through-hello-azure-portal"></a>Povolení protokolování prostřednictvím hello portálu Azure

1. V hello portálu Azure, najít prostředek a klikněte na **diagnostické protokoly**.

   Pro službu Application Gateway jsou k dispozici tři protokoly:

   * Přístup k protokolu
   * Protokolu výkonu
   * Protokol brány firewall

2. toostart shromažďování dat, klikněte na tlačítko **zapněte diagnostiku**.

   ![Zapnutí diagnostiky][1]

3. Hello **nastavení diagnostiky** okno poskytuje hello nastavení pro hello diagnostické protokoly. V tomto příkladu analýzy protokolů ukládá protokoly hello. Klikněte na tlačítko **konfigurace** pod **analýzy protokolů** tooconfigure pracovního prostoru. Můžete také použít centrům událostí a úložiště účet toosave hello diagnostické protokoly.

   ![Počáteční proces konfigurace hello][2]

4. Zvolte existujícímu pracovnímu prostoru služby Operations Management Suite (OMS) nebo vytvořte novou. Tento příklad používá nějaký existující.

   ![Možnosti pro OMS pracovní prostory][3]

5. Potvrzení hello nastavení a klikněte na tlačítko **Uložit**.

   ![Okno nastavení diagnostiky se výběry][4]

### <a name="activity-log"></a>Protokol aktivit

Ve výchozím nastavení vygeneruje Azure protokol aktivit hello. protokoly Hello se zachovají 90 dní v úložišti Azure protokoly událostí hello. Další informace o tyto protokoly načtením hello [zobrazování událostí a protokolu aktivity](../monitoring-and-diagnostics/insights-debugging-with-events.md) článku.

### <a name="access-log"></a>Přístup k protokolu

Hello přístup protokol je generovaný pouze v případě, že jste ho povolili na každou instanci aplikace brány, podle popisu v předchozích krocích hello. Hello data se ukládají v účtu úložiště hello, kterou jste zadali, pokud jste povolili protokolování hello. Každý přístup Application Gateway je zaznamenána ve formátu JSON, jak ukazuje následující příklad hello:


|Hodnota  |Popis  |
|---------|---------|
|identifikátor instanceId     | Aplikační brána instanci tohoto požadavku obsloužit hello.        |
|Když     | Původní IP pro požadavek hello.        |
|clientPort     | Výchozí port pro požadavek hello.       |
|HttpMethod     | Metoda HTTP používaný hello požadavku.       |
|requestUri     | Identifikátor URI hello přijatý požadavek.        |
|RequestQuery     | **Server směrovat**: instance fond Back-end, který vám byl zaslán požadavek hello. </br> **X-AzureApplicationGateway-LOG-ID**: ID korelace použitý pro požadavek hello. Může být použité tootroubleshoot provoz problémy na hello back-end serverů. </br>**Stav serveru**: kód odpovědi HTTP, který Application Gateway získali od hello back-end.       |
|UserAgent     | Agent u uživatele z hlavičky požadavku hello HTTP.        |
|httpStatus     | Stavový kód HTTP vrácená toohello klienta z aplikační brány.       |
|httpVersion     | Verze protokolu HTTP žádosti hello.        |
|ReceivedBytes     | Velikost paketu přijaté v bajtech.        |
|SentBytes| Velikost paket odeslaný v bajtech.|
|timeTaken| Délka dobu (v milisekundách), která je potřebná pro žádost o toobe, zpracování a jeho toobe odpovědi odeslat. Počítá se jako interval hello od času hello při Application Gateway přijímá první bajt hello doba toohello požadavku HTTP při odeslání odpovědi hello dokončení operace. Je důležité, že toonote, který hello Time-Taken pole obvykle zahrnuje hello čas požadavku a odpovědi pakety hello cestách přes síť hello. |
|Protokol| Jestli komunikace toohello back endové fondy používat protokol SSL. Platné hodnoty jsou zapnout a vypnout.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a>Protokolu výkonu

Hello výkonu protokol je generovaný pouze v případě, že jste povolili na každou instanci aplikace brány, podle popisu v předchozích krocích hello. Hello data se ukládají v účtu úložiště hello, kterou jste zadali, pokud jste povolili protokolování hello. data protokolu výkonu Hello je vygenerována v intervalech 1 minutu. zaznamená se Hello následující data:


|Hodnota  |Popis  |
|---------|---------|
|identifikátor instanceId     |  Instance brány aplikace, které výkonu je generován data. Pro bránu více instancí aplikace je jeden řádek pro každou instanci.        |
|healthyHostCount     | Počet pořádku hostitelích ve fondu back-end hello.        |
|unHealthyHostCount     | Počet není v pořádku hostitelích ve fondu back-end hello.        |
|RequestCount     | Počet požadavků zpracovaných.        |
|Čekací doba | Čekací doba (v milisekundách) požadavků od hello instance toohello back-end, který obsluhuje žádosti hello. |
|failedRequestCount| Počet neúspěšných požadavků.|
|Propustnost| Průměrná propustnost od poslední protokolu hello měřená v bajtech za sekundu.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> Latencí se počítá z hello čas, kdy hello první bajt požadavku hello HTTP přijatých toohello čas odeslání poslední bajt hello hello odpověď HTTP. Jeho hello součet hello Application Gateway doba zpracování plus hello sítě náklady toohello zpět ukončení, plus hello dobu, po kterou hello back-end trvá tooprocess hello požadavku.

### <a name="firewall-log"></a>Protokol brány firewall

Hello brány firewall protokol je generovaný pouze v případě, že jste je povolili pro každý aplikační brány, podle popisu v předchozích krocích hello. Tento protokol taky vyžaduje, že aby brána firewall hello webové aplikace je nakonfigurovaná na aplikační brány. Hello data se ukládají v účtu úložiště hello, kterou jste zadali, pokud jste povolili protokolování hello. zaznamená se Hello následující data:


|Hodnota  |Popis  |
|---------|---------|
|identifikátor instanceId     | Instance brány aplikace, které brány firewall data jsou generována. Pro bránu více instancí aplikace je jeden řádek pro každou instanci.         |
|Když     |   Původní IP pro požadavek hello.      |
|clientPort     |  Výchozí port pro požadavek hello.       |
|requestUri     | Adresa URL hello přijatý požadavek.       |
|ruleSetType     | Typ sady pravidel. k dispozici hodnota Hello je OWASP.        |
|ruleSetVersion     | Verze použitá sady pravidel. Dostupné jsou hodnoty 2.2.9 a 3.0.     |
|RuleId     | ID pravidla hello spuštěním události.        |
|Zpráva     | Uživatelsky přívětivý zpráva pro hello spuštěním události. Další podrobnosti najdete v části Podrobnosti o hello.        |
|Akce     |  Akce na vyžádání hello. Dostupné hodnoty jsou blokované a povolené.      |
|Lokality     | Web, pro které hello byla vygenerována protokolu. V současné době pouze globální se má zobrazit, protože pravidla jsou globální.|
|Podrobnosti     | Podrobnosti o hello spuštěním události.        |
|details.Message     | Popis pravidla hello.        |
|details.data     | Konkrétní data nalezena v žádosti o tomto pravidle odpovídající hello.         |
|details.File     | Konfigurační soubor, který obsahoval hello pravidlo.        |
|details.Line     | Číslo řádku v hello konfigurační soubor, který aktivoval hello událostí.       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-hello-activity-log"></a>Zobrazit a analyzovat protokol aktivit hello

Můžete zobrazit a analyzovat data protokolu aktivit pomocí některé z hello následující metody:

* **Nástroje Azure**: načtení informací z protokolu aktivit hello pomocí Azure Powershellu hello rozhraní příkazového řádku Azure, hello REST API služby Azure, nebo hello portálu Azure. Podrobné pokyny pro jednotlivé metody jsou podrobně popsané na hello [operací aktivit s Resource Managerem](../azure-resource-manager/resource-group-audit.md) článku.
* **Power BI**: Pokud ještě nemáte [Power BI](https://powerbi.microsoft.com/pricing) účet, můžete zkusit je zdarma. Pomocí hello [obsahu protokoly aktivity Azure pack pro Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), můžete analyzovat svá data pomocí předkonfigurované řídicí panely, které můžete použít nebo přizpůsobit.

### <a name="view-and-analyze-hello-access-performance-and-firewall-logs"></a>Zobrazit a analyzovat hello přístup, výkonu a protokoly brány firewall

Azure [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md) můžete shromažďovat soubory protokolů událostí a čítače hello z vašeho účtu úložiště objektů Blob. Obsahuje vizualizace a výkonné vyhledávání možnosti tooanalyze protokolů.

Můžete se také připojit tooyour účet úložiště a načíst položky protokolu hello JSON pro přístup a protokoly výkonu. Po stažení hello soubory JSON, můžete je převést tooCSV a zobrazit je v aplikaci Excel, Power BI nebo jakýkoli jiný nástroj, vizualizace dat.

> [!TIP]
> Pokud jste obeznámeni s Visual Studio a základní koncepty změna hodnoty konstanty a proměnné v jazyce C#, můžete použít hello [protokolu nástroje Převaděč](https://github.com/Azure-Samples/networking-dotnet-log-converter) dostupné z Githubu.
> 
> 

## <a name="metrics"></a>Metriky

Metriky jsou funkce u některých prostředků Azure, kde můžete zobrazit čítače výkonu portálu hello. Pro službu Application Gateway jedna metrika je k dispozici nyní. Tato metrika je propustnost a zobrazí se na portálu hello. Procházet tooan aplikační brány a klikněte na tlačítko **metriky**. tooview hello hodnoty, vyberte propustnost v hello **dostupné metriky** části. V hello následující bitové kopie vidíte příklad s filtry hello používané toodisplay hello data v různých časových rozsahů.

![Zobrazení metriky s filtry][5]

toosee aktuální seznam metriky, najdete v části [podporované metriky s Azure monitorování](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

### <a name="alert-rules"></a>Pravidla výstrah

Můžete spustit na základě metriky pro prostředek pravidla výstrah. Výstrahu můžete například volat webhook, jehož nebo e-mailu správce, pokud hello propustnost hello aplikační brány je výše, níže nebo na prahovou hodnotu v zadaném období.

Následující ukázka Hello vás provede vytvořením pravidlo, které pošle e-mailu správce tooan za narušení propustnost a prahové hodnoty:

1. Klikněte na tlačítko **přidat metriky upozornění** tooopen hello **přidat pravidlo** okno. Mohou být využity také toto okno, v okně metriky hello.

   ![Tlačítko "Přidat metriky upozornění"][6]

2. Na hello **přidat pravidlo** okně vyplnit hello název, stav a upozornit části a klikněte na tlačítko **OK**.

   * V hello **podmínku** selektor, vyberte jednu z hodnot hello čtyři: **větší než**, **větší než nebo rovna**, **menší než**, nebo  **Menší než nebo rovna hodnotě**.

   * V hello **období** selektor, vyberte dobu od 5 minut too6 hodin.

   * Pokud vyberete **e-mailu vlastníci, přispěvatelé a čtenáři**, e-mailu hello může být dynamické na základě hello uživatelů, kteří mají přístup toothat prostředků. Jinak, můžete zadat čárkami oddělený seznam uživatelů v hello **email(s) další správce** pole.

   ![Přidat pravidlo okno][7]

Pokud je prahová hodnota hello nedodržení, dorazí e-mail, který je podobný toohello, jeden v hello následující bitové kopie:

![E-mailu pro porušení prahové hodnoty][8]

Po vytvoření metriky upozornění se zobrazí seznam výstrah. Poskytuje přehled o všech hello pravidla výstrah.

![Seznam výstrah a pravidla][9]

toolearn Další informace o oznámení o výstrahách, najdete v části [dostávat oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

toounderstand informace o webhooky a jak je možné používat s výstrahy, navštivte [konfigurace webhook, jehož na výstrahu Azure metriky](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="next-steps"></a>Další kroky

* Vizualizovat čítač a protokoly událostí pomocí [analýzy protokolů](../log-analytics/log-analytics-azure-networking-analytics.md).
* [Aktivita Azure protokolu s Power BI vizualizovat](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) příspěvku na blogu.
* [Zobrazení a analýza protokolů Azure aktivity v Power BI a další](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) příspěvku na blogu.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
