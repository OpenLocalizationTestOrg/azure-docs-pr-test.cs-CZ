---
title: "Spravovat skupiny zabezpečení sítě toku protokoly s sledovací proces sítě Azure - REST API | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat protokoly toku skupinu zabezpečení sítě v Azure sledovací proces sítě pomocí rozhraní REST API"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2ab25379-0fd3-4bfe-9d82-425dfc7ad6bb
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: c89a2ab4c39978771c940a819493b4e2283d5f9f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a><span data-ttu-id="31e30-103">Protokoly toku konfigurace skupinu zabezpečení sítě pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="31e30-103">Configuring Network Security Group flow logs using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="31e30-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31e30-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="31e30-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="31e30-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="31e30-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="31e30-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="31e30-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="31e30-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="31e30-108">REST API</span><span class="sxs-lookup"><span data-stu-id="31e30-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="31e30-109">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, která vám umožní zobrazit informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="31e30-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="31e30-110">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit příchozí a odchozí toky na základě pravidla na síťový adaptér tok se vztahuje na 5 řazené kolekce členů informace o toku (protokol IP zdroj nebo cíl, zdrojový nebo cílový Port), a pokud se povoluje nebo odepírá provoz.</span><span class="sxs-lookup"><span data-stu-id="31e30-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="31e30-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="31e30-111">Before you begin</span></span>

<span data-ttu-id="31e30-112">ARMclient se používá k volání rozhraní REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="31e30-112">ARMclient is used to call the REST API using PowerShell.</span></span> <span data-ttu-id="31e30-113">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="31e30-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="31e30-114">Tento scénář předpokládá, že už jste udělali kroky v [vytvořit sledovací proces sítě](network-watcher-create.md) vytvořit sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="31e30-114">This scenario assumes you have already followed the steps in [Create a Network Watcher](network-watcher-create.md) to create a Network Watcher.</span></span>

> [!Important]
> <span data-ttu-id="31e30-115">Pro volání rozhraní API REST sledovací proces sítě, že název skupiny prostředků v identifikátoru URI požadavku je skupina prostředků, který obsahuje sledovací proces sítě ne prostředky provádíte diagnostiky akce na.</span><span class="sxs-lookup"><span data-stu-id="31e30-115">For Network Watcher REST API calls the resource group name in the request URI is the resource group that contains the Network Watcher, not the resources you are performing the diagnostic actions on.</span></span>

## <a name="scenario"></a><span data-ttu-id="31e30-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="31e30-116">Scenario</span></span>

<span data-ttu-id="31e30-117">Scénář popsaná v tomto článku se dozvíte, jak povolit, zakázat a dotaz toku protokolů pomocí rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="31e30-117">The scenario covered in this article shows you how to enable, disable, and query flow logs using the REST API.</span></span> <span data-ttu-id="31e30-118">Další informace o toku loggings skupinu zabezpečení sítě, navštivte [protokolování toku skupinu zabezpečení sítě - přehled](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="31e30-118">To learn more about Network Security Group flow loggings, visit [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="31e30-119">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="31e30-119">In this scenario, you will:</span></span>

* <span data-ttu-id="31e30-120">Povolení toku protokolů</span><span class="sxs-lookup"><span data-stu-id="31e30-120">Enable flow logs</span></span>
* <span data-ttu-id="31e30-121">Zakázat protokoly toku</span><span class="sxs-lookup"><span data-stu-id="31e30-121">Disable flow logs</span></span>
* <span data-ttu-id="31e30-122">Stav protokoly toku dotazu</span><span class="sxs-lookup"><span data-stu-id="31e30-122">Query flow logs status</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="31e30-123">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="31e30-123">Log in with ARMClient</span></span>

<span data-ttu-id="31e30-124">Přihlaste se k armclient pomocí svých přihlašovacích údajů Azure.</span><span class="sxs-lookup"><span data-stu-id="31e30-124">Log in to armclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a><span data-ttu-id="31e30-125">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="31e30-125">Register Insights provider</span></span>

<span data-ttu-id="31e30-126">V pořadí pro tok protokolování nemusí fungovat správně **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="31e30-126">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="31e30-127">Pokud si nejste jisti Pokud **Microsoft.Insights** zaregistrovat poskytovatele, spusťte následující skript.</span><span class="sxs-lookup"><span data-stu-id="31e30-127">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="31e30-128">Protokoly toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="31e30-128">Enable Network Security Group flow logs</span></span>

<span data-ttu-id="31e30-129">Příkaz pro povolení protokolů toku je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="31e30-129">The command to enable flow logs is shown in the following example:</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="31e30-130">Odpovědi vrácené z předchozího příkladu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="31e30-130">The response returned from the preceding example is as follows:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="31e30-131">Protokoly toku zakázat skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="31e30-131">Disable Network Security Group flow logs</span></span>

<span data-ttu-id="31e30-132">Pomocí následujícího příkladu zakázat toku protokoly.</span><span class="sxs-lookup"><span data-stu-id="31e30-132">Use the following example to disable flow logs.</span></span> <span data-ttu-id="31e30-133">Volání je totéž jako povolení toku protokoly, s výjimkou **false** nastavená pro vlastnost povoleno.</span><span class="sxs-lookup"><span data-stu-id="31e30-133">The call is the same as enabling flow logs, except **false** is set for the enabled property.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="31e30-134">Odpovědi vrácené z předchozího příkladu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="31e30-134">The response returned from the preceding example is as follows:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="query-flow-logs"></a><span data-ttu-id="31e30-135">Protokoly toku dotazu</span><span class="sxs-lookup"><span data-stu-id="31e30-135">Query flow logs</span></span>

<span data-ttu-id="31e30-136">Následující dotazy volání REST stav toku protokolů na skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="31e30-136">The following REST call queries the status of flow logs on a Network Security Group.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

<span data-ttu-id="31e30-137">Následuje příklad odpovědi vrácené:</span><span class="sxs-lookup"><span data-stu-id="31e30-137">The following is an example of the response returned:</span></span>

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
   "retentionPolicy": {
      "days": 5,
      "enabled": true
    }
  }
}
```

## <a name="download-a-flow-log"></a><span data-ttu-id="31e30-138">Stáhnout protokolu toku</span><span class="sxs-lookup"><span data-stu-id="31e30-138">Download a flow log</span></span>

<span data-ttu-id="31e30-139">Umístění úložiště toku protokolu se definuje při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="31e30-139">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="31e30-140">Je vhodné nástroj pro přístup k tyto protokoly toku uložit do účtu úložiště Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="31e30-140">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="31e30-141">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="31e30-141">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a><span data-ttu-id="31e30-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="31e30-142">Next steps</span></span>

<span data-ttu-id="31e30-143">Zjistěte, jak [vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="31e30-143">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="31e30-144">Zjistěte, jak [vizualizovat toku protokolů NSG s otevřeným zdrojem nástroje](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="31e30-144">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
