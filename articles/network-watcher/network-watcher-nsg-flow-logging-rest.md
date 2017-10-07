---
title: "protokoly aaaManage toku skupinu zabezpečení sítě s sledovací proces sítě Azure - REST API | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage skupinu zabezpečení sítě toku přihlásí sledovací proces sítě Azure s REST API"
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
ms.openlocfilehash: be81e35f4d01c67efef99773e9b4e2ae4b8e209e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a><span data-ttu-id="1971f-103">Protokoly toku konfigurace skupinu zabezpečení sítě pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="1971f-103">Configuring Network Security Group flow logs using REST API</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="1971f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1971f-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="1971f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1971f-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="1971f-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1971f-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="1971f-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="1971f-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="1971f-108">REST API</span><span class="sxs-lookup"><span data-stu-id="1971f-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="1971f-109">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, který vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1971f-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="1971f-110">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit odchozí a příchozí tok na základě za pravidlo hello toku hello síťový adaptér se vztahuje na 5 řazené kolekce členů informace o toku hello (zdroj nebo cíl, zdrojový nebo cílový Port, protokol IP) a pokud hello bylo povolené přenosy nebo byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="1971f-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="1971f-111">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1971f-111">Before you begin</span></span>

<span data-ttu-id="1971f-112">ARMclient je použité toocall hello REST API pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1971f-112">ARMclient is used toocall hello REST API using PowerShell.</span></span> <span data-ttu-id="1971f-113">ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)</span><span class="sxs-lookup"><span data-stu-id="1971f-113">ARMClient is found on chocolatey at [ARMClient on Chocolatey](https://chocolatey.org/packages/ARMClient)</span></span>

<span data-ttu-id="1971f-114">Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.</span><span class="sxs-lookup"><span data-stu-id="1971f-114">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span>

> [!Important]
> <span data-ttu-id="1971f-115">Volání rozhraní API REST sledovací proces sítě hello název skupiny prostředků v žádosti o hello, že je identifikátor URI hello skupinu prostředků, která obsahuje hello sledovací proces sítě, není hello prostředků, kterou provádíte hello diagnostiky akce na.</span><span class="sxs-lookup"><span data-stu-id="1971f-115">For Network Watcher REST API calls hello resource group name in hello request URI is hello resource group that contains hello Network Watcher, not hello resources you are performing hello diagnostic actions on.</span></span>

## <a name="scenario"></a><span data-ttu-id="1971f-116">Scénář</span><span class="sxs-lookup"><span data-stu-id="1971f-116">Scenario</span></span>

<span data-ttu-id="1971f-117">scénář Hello popsaná v tomto článku ukazuje, jak tooenable, zakázat a dotazování toku protokolů pomocí hello REST API.</span><span class="sxs-lookup"><span data-stu-id="1971f-117">hello scenario covered in this article shows you how tooenable, disable, and query flow logs using hello REST API.</span></span> <span data-ttu-id="1971f-118">toolearn Další informace o toku loggings skupinu zabezpečení sítě, navštivte [protokolování toku skupinu zabezpečení sítě - přehled](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1971f-118">toolearn more about Network Security Group flow loggings, visit [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="1971f-119">V tomto scénáři provedete následující:</span><span class="sxs-lookup"><span data-stu-id="1971f-119">In this scenario, you will:</span></span>

* <span data-ttu-id="1971f-120">Povolení toku protokolů</span><span class="sxs-lookup"><span data-stu-id="1971f-120">Enable flow logs</span></span>
* <span data-ttu-id="1971f-121">Zakázat protokoly toku</span><span class="sxs-lookup"><span data-stu-id="1971f-121">Disable flow logs</span></span>
* <span data-ttu-id="1971f-122">Stav protokoly toku dotazu</span><span class="sxs-lookup"><span data-stu-id="1971f-122">Query flow logs status</span></span>

## <a name="log-in-with-armclient"></a><span data-ttu-id="1971f-123">Přihlaste se pomocí ARMClient</span><span class="sxs-lookup"><span data-stu-id="1971f-123">Log in with ARMClient</span></span>

<span data-ttu-id="1971f-124">Přihlaste se pomocí svých přihlašovacích údajů Azure tooarmclient.</span><span class="sxs-lookup"><span data-stu-id="1971f-124">Log in tooarmclient with your Azure credentials.</span></span>

```PowerShell
armclient login
```

## <a name="register-insights-provider"></a><span data-ttu-id="1971f-125">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="1971f-125">Register Insights provider</span></span>

<span data-ttu-id="1971f-126">V pořadí pro tok protokolování toowork úspěšně, hello **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="1971f-126">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="1971f-127">Pokud si nejste jistí, jestli hello **Microsoft.Insights** zprostředkovatele je registrovaná, spusťte hello následující skript.</span><span class="sxs-lookup"><span data-stu-id="1971f-127">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="1971f-128">Protokoly toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="1971f-128">Enable Network Security Group flow logs</span></span>

<span data-ttu-id="1971f-129">Hello příkaz tooenable toku protokoly se zobrazí v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="1971f-129">hello command tooenable flow logs is shown in hello following example:</span></span>

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

<span data-ttu-id="1971f-130">Hello odpověď vrácená z hello předchozí příklad je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1971f-130">hello response returned from hello preceding example is as follows:</span></span>

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

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="1971f-131">Protokoly toku zakázat skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="1971f-131">Disable Network Security Group flow logs</span></span>

<span data-ttu-id="1971f-132">Následující příklad toodisable toku protokoly použití hello.</span><span class="sxs-lookup"><span data-stu-id="1971f-132">Use hello following example toodisable flow logs.</span></span> <span data-ttu-id="1971f-133">Hello volání je stejné jako povolení toku protokoly, s výjimkou hello **false** nastavená pro vlastnost hello povolena.</span><span class="sxs-lookup"><span data-stu-id="1971f-133">hello call is hello same as enabling flow logs, except **false** is set for hello enabled property.</span></span>

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

<span data-ttu-id="1971f-134">Hello odpověď vrácená z hello předchozí příklad je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1971f-134">hello response returned from hello preceding example is as follows:</span></span>

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

## <a name="query-flow-logs"></a><span data-ttu-id="1971f-135">Protokoly toku dotazu</span><span class="sxs-lookup"><span data-stu-id="1971f-135">Query flow logs</span></span>

<span data-ttu-id="1971f-136">Následující volání REST, že dotazy hello stav toku Hello protokolů na skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1971f-136">hello following REST call queries hello status of flow logs on a Network Security Group.</span></span>

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

<span data-ttu-id="1971f-137">Hello následuje příklad hello odpovědi vrácené:</span><span class="sxs-lookup"><span data-stu-id="1971f-137">hello following is an example of hello response returned:</span></span>

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

## <a name="download-a-flow-log"></a><span data-ttu-id="1971f-138">Stáhnout protokolu toku</span><span class="sxs-lookup"><span data-stu-id="1971f-138">Download a flow log</span></span>

<span data-ttu-id="1971f-139">umístění úložiště Hello toku protokolu se definuje při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="1971f-139">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="1971f-140">Vhodné nástroje tooaccess, účet úložiště tooa protokoly uložené tyto tok je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="1971f-140">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="1971f-141">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="1971f-141">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

## <a name="next-steps"></a><span data-ttu-id="1971f-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1971f-142">Next steps</span></span>

<span data-ttu-id="1971f-143">Zjistěte, jak příliš[vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="1971f-143">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="1971f-144">Zjistěte, jak příliš[vizualizovat toku protokolů NSG s otevřeným zdrojem nástroje](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="1971f-144">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
