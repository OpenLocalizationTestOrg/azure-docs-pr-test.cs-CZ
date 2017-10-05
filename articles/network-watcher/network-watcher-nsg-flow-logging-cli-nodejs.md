---
title: "Správa protokolů toku skupiny zabezpečení sítě s sledovací proces sítě Azure - 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat protokoly toku skupiny zabezpečení sítě v sledovací proces sítě Azure s Azure CLI 1.0"
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
ms.openlocfilehash: 2ea8543857c062e76f96da99fb295ce831c3c5f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli-10"></a><span data-ttu-id="dd992-103">Konfigurace toku skupiny zabezpečení sítě protokolů pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="dd992-103">Configuring Network Security Group Flow logs with Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="dd992-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="dd992-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="dd992-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="dd992-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="dd992-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="dd992-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="dd992-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="dd992-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="dd992-108">REST API</span><span class="sxs-lookup"><span data-stu-id="dd992-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="dd992-109">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, která vám umožní zobrazit informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="dd992-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="dd992-110">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit příchozí a odchozí toky na základě pravidla na síťový adaptér tok se vztahuje na 5 řazené kolekce členů informace o toku (protokol IP zdroj nebo cíl, zdrojový nebo cílový Port), a pokud se povoluje nebo odepírá provoz.</span><span class="sxs-lookup"><span data-stu-id="dd992-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="dd992-111">Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="dd992-111">This article uses cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="dd992-112">Sledovací proces sítě aktuálně používá pro podporu rozhraní příkazového řádku Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="dd992-112">Network Watcher currently uses Azure CLI 1.0 for CLI support.</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="dd992-113">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="dd992-113">Register Insights provider</span></span>

<span data-ttu-id="dd992-114">V pořadí pro tok protokolování nemusí fungovat správně **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="dd992-114">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="dd992-115">Pokud si nejste jisti Pokud **Microsoft.Insights** zaregistrovat poskytovatele, spusťte následující skript.</span><span class="sxs-lookup"><span data-stu-id="dd992-115">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```azurecli
azure provider register --namespace Microsoft.Insights --subscription <subscriptionid>
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="dd992-116">Protokoly toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="dd992-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="dd992-117">Příkaz pro povolení protokolů toku je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="dd992-117">The command to enable flow logs is shown in the following example:</span></span>

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e true
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="dd992-118">Protokoly toku zakázat skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="dd992-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="dd992-119">Pomocí následujícího příkladu zakázat toku protokoly:</span><span class="sxs-lookup"><span data-stu-id="dd992-119">Use the following example to disable flow logs:</span></span>

```azurecli
azure network watcher configure-flow-log -g resourceGroupName -n networkWatcherName -t nsgId -i storageAccountId -e false
```

## <a name="download-a-flow-log"></a><span data-ttu-id="dd992-120">Stáhnout protokolu toku</span><span class="sxs-lookup"><span data-stu-id="dd992-120">Download a Flow log</span></span>

<span data-ttu-id="dd992-121">Umístění úložiště toku protokolu se definuje při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="dd992-121">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="dd992-122">Je vhodné nástroj pro přístup k tyto protokoly toku uložit do účtu úložiště Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="dd992-122">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="dd992-123">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="dd992-123">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="dd992-124">Informace o struktuře protokolu najdete v článku [toku skupiny zabezpečení sítě protokolu – přehled](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="dd992-124">For information about the structure of the log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd992-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dd992-125">Next Steps</span></span>

<span data-ttu-id="dd992-126">Zjistěte, jak [vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="dd992-126">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="dd992-127">Zjistěte, jak [vizualizovat toku protokolů NSG s otevřeným zdrojem nástroje](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="dd992-127">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
