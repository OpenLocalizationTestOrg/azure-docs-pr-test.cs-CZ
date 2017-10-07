---
title: "aaaAzure návod monitorování REST API | Microsoft Docs"
description: "Jak tooauthenticate požadavků tooand použití hello monitorování REST API služby Azure."
author: mcollier
manager: 
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: mcollier
ms.openlocfilehash: b8ae3a03fd21af872f1dc5fed40a101a24ca1652
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Monitorování návod rozhraní API REST Azure
Tento článek ukazuje, jak tooperform ověřování, aby váš kód můžete použít hello [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Hello rozhraní API služby Azure monitorování je možné tooprogrammatically načtení hello dostupná výchozí definice metrik (hello Typ Metrika například čas procesoru, požadavků, atd.), členitosti a metriky hodnoty. Jakmile načíst, mohou být hello data uložena v úložišti dat samostatné například Azure SQL Database, Azure Cosmos DB nebo Azure Data Lake. Odtud můžete provést další analýzu podle potřeby.

Kromě práce s různými metriky datové body, jako tento článek ukazuje, hello monitorování API je možné toolist pravidla výstrah, zobrazení protokoly aktivity a mnoho dalšího. Úplný seznam dostupné operace, najdete v části hello [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Ověřování Azure sledování požadavků
prvním krokem Hello je tooauthenticate hello požadavku.

Všechny úlohy hello prováděné vůči hello rozhraní API služby Azure monitorování použití hello Azure Resource Manager ověření modelu. Proto musí být ověřeny všechny požadavky s Azure Active Directory (Azure AD). Jeden způsob tooauthenticate hello klientská aplikace je toocreate objektu služby Azure AD a získat token hello ověřování (JWT). Hello následující ukázkový skript ukazuje vytvoření objektu služby pomocí prostředí PowerShell Azure AD. Pro podrobnější návod, naleznete v dokumentaci toohello na [pomocí prostředí Azure PowerShell toocreate prostředky služby hlavní tooaccess](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Je také možné příliš[vytvořit objekt služby prostřednictvím portálu Azure hello](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate tooa specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for hello service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with hello designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role toohello newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

hello tooquery rozhraní API Azure monitorování, klientská aplikace hello měli používat hello vytvořili tooauthenticate hlavní služby. Hello následující skript prostředí PowerShell příklad ukazuje jeden ze způsobů, pomocí hello [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) toohelp získání tokenu ověřování tokenů JWT hello. Hello JWT token se předal v rámci parametru HTTP autorizace v toohello požadavky REST API služby Azure monitorování.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Po dokončení kroku hello ověřování dotazy mohou být pak provedeny proti hello REST API služby Azure monitorování. Existují dva užitečné dotazy:

1. Seznam hello Definice metrik pro prostředek
2. Načíst hodnoty metriky hello

## <a name="retrieve-metric-definitions"></a>Načtení definice metrik
> [!NOTE]
> definice metrik tooretrieve pomocí hello REST API služby Azure monitorování, použijte "2016-03-01" hello verze rozhraní API.
>
>

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Pro aplikace logiky Azure objeví definice metrik hello podobné toohello následující snímek obrazovky:

![ALT "JSON zobrazení metriky definice odpovědi."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Další informace najdete v tématu hello [seznamu hello Definice metrik pro prostředek v rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentaci.

## <a name="retrieve-metric-values"></a>Načtení metriky hodnot
K dispozici definice metrik hello známé, je možné tooretrieve hello souvisejících hodnot metriky. Použijte název hello metrika 'Hodnota' (ne hello 'localizedValue') pro všechny filtrování požadavků (například načtení hello 'CpuTime' a 'Požadavky' metriky datové body). Pokud nejsou zadány žádné filtry, vrátí se hello výchozí metriku.

> [!NOTE]
> hodnoty metriky tooretrieve pomocí hello REST API služby Azure monitorování, použijte "2016-06-01" hello verze rozhraní API.
>
>

**Metoda**: získání

**Identifikátor URI požadavku je**: https://management.azure.com/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/*{-– obor názvů zprostředkovatele prostředků}*/*{typ prostředku}*/*{název prostředku}*/providers/microsoft.insights/metrics?$filter=*{filtru}*& verze api-version =*{apiVersion}*

Například tooretrieve hello RunsSucceeded metriky datových bodů pro hello zadaný časový rozsah a časovým intervalem 1 hodina, žádost hello vypadat takto:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

výsledek Hello vypadat podobně jako příklad toohello následující snímek obrazovky:

![ALT "Odpověď JSON zobrazuje průměrný čas odezvy metriky hodnota"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

tooretrieve více dat nebo agregace body, přidejte hello metriky definice názvy a agregace typy toohello filtr, jak je vidět v hello následující ukázka:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Použití ARMClient
Alternativní toousing prostředí PowerShell (jak je uvedeno výše), je toouse [ARMClient](https://github.com/projectkudu/ARMClient) na počítač se systémem Windows. Automaticky ARMClient zpracovává ověřování hello Azure AD (a výsledný token JWT). Hello následující kroky popisují použití ARMClient pro načítání metriky dat:

1. Nainstalujte [Chocolatey](https://chocolatey.org/) a [ARMClient](https://github.com/projectkudu/ARMClient).
2. Okno terminálu, zadejte *armclient.exe přihlášení*. To vás vyzve k toolog v tooAzure.
3. Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*

![ALT "Pomocí ARMClient toowork s hello rozhraní API REST Azure monitorování"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-hello-resource-id"></a>Načtení hello ID prostředku
Použití hello REST API může pomoci skutečně toounderstand hello k dispozici definice metrik, členitosti a souvisejících hodnot. Tyto informace jsou užitečné při použití hello [Knihovna správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Pro hello předcházející kódu toouse ID prostředku hello je úplná cesta toohello hello požadovaných prostředků Azure. Například tooquery proti webové aplikace Azure, ID prostředku hello by být:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Hello následující seznam obsahuje několik příkladů formáty ID prostředku pro různé prostředky Azure:

* **IoT Hub** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **Elastický fond SQL** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{fondu db}*/elasticpools/*{sql název fondu}*
* **Databáze SQL (v12)** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{název serveru}*/databases/*{název databáze}*
* **Service Bus** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.ServiceBus/*{obor názvů}*/*{servicebus-name}*
* **Škálovatelné sady virtuálních počítačů** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{název virtuálního počítače}*
* **Virtuální počítače** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachines/*{název virtuálního počítače}*
* **Služba Event Hubs** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

Nejsou k dispozici alternativní přístupy tooretrieving hello ID prostředku, včetně použití Průzkumníka prostředků Azure, zobrazení hello požadovaného prostředku v hello portál Azure a pomocí prostředí PowerShell nebo hello rozhraní příkazového řádku Azure.

### <a name="azure-resource-explorer"></a>Průzkumník prostředků Azure
ID prostředku hello toofind pro požadovaný prostředek, jeden ze způsobů užitečné je toouse hello [Průzkumníka prostředků Azure](https://resources.azure.com) nástroj. Přejděte toohello požadovaných prostředků a podívejte se na ID hello zobrazí jako hello následující snímek obrazovky:

![ALT "Průzkumníka prostředků Azure"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>portál Azure
ID prostředku Hello můžete získat také hello portálu Azure. toodo tedy přejděte toohello požadovaných prostředků a vyberte možnost Vlastnosti. Hello ID prostředku se zobrazí v okně Vlastnosti hello, jak je vidět v hello následující snímek obrazovky:

![ALT "ID prostředku zobrazí v okně Vlastnosti hello v hello portál Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
ID prostředku Hello můžete načíst pomocí rutin prostředí Azure PowerShell také. Například ID prostředku hello tooobtain pro webové aplikace Azure, spusťte rutinu Get-AzureRmWebApp hello jako hello následující snímek obrazovky:

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
tooretrieve hello ID prostředku pomocí hello příkazového řádku Azure CLI, spusťte příkaz "azure webapp zobrazit" hello, zadání hello ' – json, možnost, jak ukazuje následující snímek obrazovky hello:

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Načíst Data protokolu aktivit
V přidání tooworking s definice metrik a souvisejících hodnot je také možné tooretrieve další zajímavé přehledy související tooAzure prostředky. Jako příklad, je možné tooquery [protokol aktivit](https://msdn.microsoft.com/library/azure/dn931934.aspx) data. Hello následující příklad ukazuje použití data protokolu hello REST API služby Azure monitorování tooquery aktivit v určité datum rozsahu pro předplatné Azure:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Další kroky
* Zkontrolujte hello [Přehled monitorování](monitoring-overview.md).
* Zobrazení hello [podporované metriky s Azure monitorování](monitoring-supported-metrics.md).
* Zkontrolujte hello [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Zkontrolujte hello [Knihovna správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).
