---
title: "aaaManage toku skupiny zabezpečení sítě protokoly s sledovací proces sítě Azure – prostředí PowerShell | Microsoft Docs"
description: "Tato stránka vysvětluje, jak toomanage toku skupiny zabezpečení sítě přihlásí sledovací proces sítě Azure pomocí prostředí PowerShell"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 987e8728fb6459fd6ff8eb5cd3d36ff855f2ccfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a><span data-ttu-id="d0a2e-103">Konfigurace protokolů toku skupiny zabezpečení sítě v prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0a2e-103">Configuring Network Security Group Flow logs with PowerShell</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="d0a2e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d0a2e-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="d0a2e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0a2e-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="d0a2e-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d0a2e-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="d0a2e-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d0a2e-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="d0a2e-108">REST API</span><span class="sxs-lookup"><span data-stu-id="d0a2e-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="d0a2e-109">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, který vám umožní tooview informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="d0a2e-109">Network Security Group flow logs are a feature of Network Watcher that allows you tooview information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="d0a2e-110">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit odchozí a příchozí tok na základě za pravidlo hello toku hello síťový adaptér se vztahuje na 5 řazené kolekce členů informace o toku hello (zdroj nebo cíl, zdrojový nebo cílový Port, protokol IP) a pokud hello bylo povolené přenosy nebo byl odepřen.</span><span class="sxs-lookup"><span data-stu-id="d0a2e-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="d0a2e-111">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="d0a2e-111">Register Insights provider</span></span>

<span data-ttu-id="d0a2e-112">V pořadí pro tok protokolování toowork úspěšně, hello **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="d0a2e-112">In order for flow logging toowork successfully, hello **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="d0a2e-113">Pokud si nejste jistí, jestli hello **Microsoft.Insights** zprostředkovatele je registrovaná, spusťte hello následující skript.</span><span class="sxs-lookup"><span data-stu-id="d0a2e-113">If you are not sure if hello **Microsoft.Insights** provider is registered, run hello following script.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="d0a2e-114">Protokoly toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="d0a2e-114">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="d0a2e-115">Hello příkaz tooenable toku protokoly se zobrazí v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d0a2e-115">hello command tooenable flow logs is shown in hello following example:</span></span>

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG-Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="d0a2e-116">Protokoly toku zakázat skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="d0a2e-116">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="d0a2e-117">Hello použijte následující příklad toodisable toku protokoly:</span><span class="sxs-lookup"><span data-stu-id="d0a2e-117">Use hello following example toodisable flow logs:</span></span>

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a><span data-ttu-id="d0a2e-118">Stáhnout protokolu toku</span><span class="sxs-lookup"><span data-stu-id="d0a2e-118">Download a Flow log</span></span>

<span data-ttu-id="d0a2e-119">umístění úložiště Hello toku protokolu se definuje při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="d0a2e-119">hello storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="d0a2e-120">Vhodné nástroje tooaccess, účet úložiště tooa protokoly uložené tyto tok je Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="d0a2e-120">A convenient tool tooaccess these flow logs saved tooa storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="d0a2e-121">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají tooa účet úložiště v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="d0a2e-121">If a storage account is specified, packet capture files are saved tooa storage account at hello following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="d0a2e-122">Informace o struktuře hello hello protokolu najdete v článku [toku skupiny zabezpečení sítě protokolu – přehled](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d0a2e-122">For information about hello structure of hello log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0a2e-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d0a2e-123">Next Steps</span></span>

<span data-ttu-id="d0a2e-124">Zjistěte, jak příliš[vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="d0a2e-124">Learn how too[Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="d0a2e-125">Zjistěte, jak příliš[vizualizovat toku protokolů NSG s otevřeným zdrojem nástroje](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="d0a2e-125">Learn how too[Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
