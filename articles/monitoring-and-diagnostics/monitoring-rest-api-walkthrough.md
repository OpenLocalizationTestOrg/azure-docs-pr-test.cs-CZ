---
title: "Monitorování návod rozhraní API REST Azure | Microsoft Docs"
description: "Postup žádosti o ověření a pomocí monitorování REST API služby Azure."
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
ms.openlocfilehash: 454a85c4752ec9c7522ef147d5ce594ef5992c32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-monitoring-rest-api-walkthrough"></a>Monitorování návod rozhraní API REST Azure
V tomto článku se dozvíte, jak provést ověření, abyste mohli používat kódu [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Rozhraní API služby Azure monitorování umožňuje načítání prostřednictvím kódu programu dostupná výchozí definice metrik (Typ Metrika například čas procesoru, požadavků, atd.), členitosti a metriky hodnoty. Po načíst, můžete data uložit v samostatných data store například Azure SQL Database, Azure Cosmos DB nebo Azure Data Lake. Odtud můžete provést další analýzu podle potřeby.

Kromě práce s různými metriky datové body, jako tento článek ukazuje, rozhraní API monitorování umožňuje k zobrazení seznamu pravidla výstrah, zobrazení protokoly aktivity a mnoho dalšího. Úplný seznam dostupné operace, najdete v článku [Microsoft referenční dokumentace rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Ověřování Azure sledování požadavků
Prvním krokem je ověřit žádost.

Všechny úlohy prováděné vůči monitorování rozhraní API služby Azure pomocí Azure Resource Manager ověření modelu. Proto musí být ověřeny všechny požadavky s Azure Active Directory (Azure AD). Jeden ze způsobů ověření klientská aplikace je vytvořit objekt služby Azure AD a načtení tokenu ověřování (JWT). Následující ukázkový skript ukazuje vytvoření objektu zabezpečení pomocí prostředí PowerShell služby Azure AD. Pro podrobnější návod, naleznete v dokumentaci na [pomocí Azure PowerShell k vytvoření objektu služby pro přístup k prostředkům](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Je také možné [vytvořit objekt služby prostřednictvím portálu Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Zpracovat dotaz rozhraní API Azure monitorování, měli klientské aplikace k ověření použít dříve vytvořenou instanční objekt. Skript prostředí PowerShell následující příklad ukazuje jeden přístupu, pomocí [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) ke získání tokenu ověřování tokenů JWT. JWT token je předán jako součást parametru HTTP autorizace v žádostech o monitorování REST API služby Azure.

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

Po dokončení instalace krok ověřování dotazy mohou být pak provedeny na REST API Azure monitorování. Existují dva užitečné dotazy:

1. Seznam definice metrik pro prostředek
2. Načíst metriky hodnoty

## <a name="retrieve-metric-definitions"></a>Načtení definice metrik
> [!NOTE]
> Pokud chcete načíst pomocí rozhraní REST API Azure monitorování definice metrik, použijte jako verze rozhraní API "2016-03-01".
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
Pro aplikace logiky Azure objeví definice metrik podobně jako na následujícím snímku obrazovky:

![ALT "JSON zobrazení metriky definice odpovědi."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Další informace najdete v tématu [seznamu definice metrik pro prostředek v rozhraní REST API Azure monitorování](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentaci.

## <a name="retrieve-metric-values"></a>Načtení metriky hodnot
Jakmile se ví, že k dispozici definice metrik, je pak možné načíst související hodnoty metriky. Použít název v metrice 'Hodnota' (ne ' localizedValue') pro všechny požadavky na filtrování (například získat metriky datových bodů 'CpuTime' a 'Požadavky'). Pokud nejsou zadány žádné filtry, vrátí se výchozí metriku.

> [!NOTE]
> Pro načtení metriky hodnoty pomocí rozhraní REST API Azure monitorování, použijte "2016-06-01" jako verze rozhraní API.
>
>

**Metoda**: získání

**Identifikátor URI požadavku je**: https://management.azure.com/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/*{-– obor názvů zprostředkovatele prostředků}*/*{typ prostředku}*/*{název prostředku}*/providers/microsoft.insights/metrics?$filter=*{filtru}*& verze api-version =*{apiVersion}*

Například pokud chcete načíst RunsSucceeded metriky datových bodů pro dané časové rozmezí a časovým intervalem 1 hodina, žádost vypadat takto:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Výsledek vypadat podobně jako v příkladu následující snímek obrazovky:

![ALT "Odpověď JSON zobrazuje průměrný čas odezvy metriky hodnota"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Načtení více bodů data nebo agregace, přidejte metriky definice názvy a typy agregace filtru, jak je vidět v následujícím příkladu:

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
Alternativu k použití prostředí PowerShell (jak je uvedeno výše), je použití [ARMClient](https://github.com/projectkudu/ARMClient) na počítač se systémem Windows. Automaticky ARMClient zpracovává ověřování Azure AD (a výsledný token JWT). Následující kroky popisují použití ARMClient pro načítání metriky dat:

1. Nainstalujte [Chocolatey](https://chocolatey.org/) a [ARMClient](https://github.com/projectkudu/ARMClient).
2. Okno terminálu, zadejte *armclient.exe přihlášení*. Výzva k přihlášení k Azure.
3. Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01*
4. Typ *armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01*

![ALT "Pomocí ARMClient pro práci s Azure monitorování REST API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

## <a name="retrieve-the-resource-id"></a>Načtení ID prostředku
Pomocí rozhraní REST API skutečně pomáhá pochopit dostupné definice metrik, členitosti a souvisejících hodnot. Informace jsou užitečné při použití [Knihovna správy Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).

ID prostředku, který má používat pro předchozí kód je úplná cesta k požadované prostředků Azure. Například k dotazování proti webové aplikace Azure, bude ID prostředku:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Následující seznam obsahuje několik příkladů formáty ID prostředku pro různé prostředky Azure:

* **IoT Hub** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **Elastický fond SQL** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{fondu db}*/elasticpools/*{sql název fondu}*
* **Databáze SQL (v12)** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Sql/servers/*{název serveru}*/databases/*{název databáze}*
* **Service Bus** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.ServiceBus/*{obor názvů}*/*{servicebus-name}*
* **Škálovatelné sady virtuálních počítačů** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{název virtuálního počítače}*
* **Virtuální počítače** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.Compute/virtualMachines/*{název virtuálního počítače}*
* **Služba Event Hubs** -/subscriptions/*{id předplatného}*/resourceGroups/*{resource-group name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

Načítání ID prostředku, včetně použití Průzkumníka prostředků Azure, zobrazení požadovaný prostředek na portálu Azure a pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure alternativní způsoby.

### <a name="azure-resource-explorer"></a>Průzkumník prostředků Azure
K vyhledání ID prostředku pro požadovaný prostředek, je užitečné jeden ze způsobů použití [Průzkumníka prostředků Azure](https://resources.azure.com) nástroj. Přejděte na požadovaný prostředek a podívejte se na zobrazené ID, stejně jako na následujícím snímku obrazovky:

![ALT "Průzkumníka prostředků Azure"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>portál Azure
ID prostředku můžete získat taky z portálu Azure. Uděláte to tak, přejděte na požadovaný prostředek a potom vyberte možnost Vlastnosti. ID prostředku se zobrazí v okně vlastností, jak je vidět na následujícím snímku obrazovky:

![ALT "ID prostředku zobrazí v okně vlastností na portálu Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure PowerShell
ID prostředku můžete načíst pomocí rutin prostředí Azure PowerShell také. Například pokud chcete získat ID prostředku pro webové aplikace Azure, spusťte rutinu Get-AzureRmWebApp, stejně jako na následujícím snímku obrazovky:

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

### <a name="azure-cli"></a>Azure CLI
Načíst ID prostředků pomocí Azure CLI, spusťte příkaz 'azure webapp zobrazit', určení ' – json, možnost, jak je znázorněno na následujícím snímku obrazovky:

![ALT "ID prostředku získat pomocí prostředí PowerShell"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Načíst Data protokolu aktivit
Kromě práci s definice metrik a souvisejících hodnot, je také možné načíst další zajímavé přehledy související s prostředky Azure. Jako příklad, je možné dotazu [protokol aktivit](https://msdn.microsoft.com/library/azure/dn931934.aspx) data. Následující příklad ukazuje, pomocí REST API pro monitorování Azure pro data protokolu aktivit dotazu v určité datum rozsahu pro předplatné Azure:

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
* Zkontrolujte [Přehled monitorování](monitoring-overview.md).
* Zobrazení [podporované metriky s Azure monitorování](monitoring-supported-metrics.md).
* Zkontrolujte [Microsoft Azure monitorovat referenční dokumentace rozhraní API REST](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Zkontrolujte [knihovna Azure správy](https://msdn.microsoft.com/library/azure/mt417623.aspx).
