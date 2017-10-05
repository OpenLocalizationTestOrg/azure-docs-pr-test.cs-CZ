---
title: "Správa protokolů toku skupiny zabezpečení sítě pomocí sledovací proces sítě Azure - rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tato stránka vysvětluje, jak spravovat protokoly toku skupiny zabezpečení sítě v Azure sledovací proces sítě pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: d5a8aa0cd274132798a0d8484a950926761dae7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-network-security-group-flow-logs-with-azure-cli"></a><span data-ttu-id="e28c4-103">Konfigurace protokolů toku skupiny zabezpečení sítě pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="e28c4-103">Configuring Network Security Group Flow logs with Azure CLI</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="e28c4-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e28c4-104">Azure portal</span></span>](network-watcher-nsg-flow-logging-portal.md)
> - [<span data-ttu-id="e28c4-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e28c4-105">PowerShell</span></span>](network-watcher-nsg-flow-logging-powershell.md)
> - [<span data-ttu-id="e28c4-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e28c4-106">CLI 1.0</span></span>](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [<span data-ttu-id="e28c4-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e28c4-107">CLI 2.0</span></span>](network-watcher-nsg-flow-logging-cli.md)
> - [<span data-ttu-id="e28c4-108">REST API</span><span class="sxs-lookup"><span data-stu-id="e28c4-108">REST API</span></span>](network-watcher-nsg-flow-logging-rest.md)

<span data-ttu-id="e28c4-109">Skupina zabezpečení sítě toku protokoly jsou funkce sledovací proces sítě, která vám umožní zobrazit informace o příchozí a odchozí provoz IP prostřednictvím skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="e28c4-109">Network Security Group flow logs are a feature of Network Watcher that allows you to view information about ingress and egress IP traffic through a Network Security Group.</span></span> <span data-ttu-id="e28c4-110">Tyto protokoly toku jsou zapsané ve formátu json a zobrazit příchozí a odchozí toky na základě pravidla na síťový adaptér tok se vztahuje na 5 řazené kolekce členů informace o toku (protokol IP zdroj nebo cíl, zdrojový nebo cílový Port), a pokud se povoluje nebo odepírá provoz.</span><span class="sxs-lookup"><span data-stu-id="e28c4-110">These flow logs are written in json format and show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="e28c4-111">Tento článek používá naší nové generace rozhraní příkazového řádku pro model nasazení prostředků management, Azure CLI 2.0, která je dostupná pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="e28c4-111">This article uses our next generation CLI for the resource management deployment model, Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span>

<span data-ttu-id="e28c4-112">Chcete-li provést kroky v tomto článku, je potřeba [nainstalovat rozhraní příkazového řádku Azure pro Mac, Linux a Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="e28c4-112">To perform the steps in this article, you need to [install the Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span>

## <a name="register-insights-provider"></a><span data-ttu-id="e28c4-113">Registrace zprostředkovatele statistiky</span><span class="sxs-lookup"><span data-stu-id="e28c4-113">Register Insights provider</span></span>

<span data-ttu-id="e28c4-114">V pořadí pro tok protokolování nemusí fungovat správně **Microsoft.Insights** zprostředkovatele musí být zaregistrován.</span><span class="sxs-lookup"><span data-stu-id="e28c4-114">In order for flow logging to work successfully, the **Microsoft.Insights** provider must be registered.</span></span> <span data-ttu-id="e28c4-115">Pokud si nejste jisti Pokud **Microsoft.Insights** zaregistrovat poskytovatele, spusťte následující skript.</span><span class="sxs-lookup"><span data-stu-id="e28c4-115">If you are not sure if the **Microsoft.Insights** provider is registered, run the following script.</span></span>

```azurecli
az provider register --namespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a><span data-ttu-id="e28c4-116">Protokoly toku povolit skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="e28c4-116">Enable Network Security Group Flow logs</span></span>

<span data-ttu-id="e28c4-117">Příkaz pro povolení protokolů toku je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="e28c4-117">The command to enable flow logs is shown in the following example:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled true --nsg nsgName --storage-account storageAccountName
```

## <a name="disable-network-security-group-flow-logs"></a><span data-ttu-id="e28c4-118">Protokoly toku zakázat skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="e28c4-118">Disable Network Security Group Flow logs</span></span>

<span data-ttu-id="e28c4-119">Pomocí následujícího příkladu zakázat toku protokoly:</span><span class="sxs-lookup"><span data-stu-id="e28c4-119">Use the following example to disable flow logs:</span></span>

```azurecli
az network watcher flow-log configure --resource-group resourceGroupName --enabled false --nsg nsgName
```

## <a name="download-a-flow-log"></a><span data-ttu-id="e28c4-120">Stáhnout protokolu toku</span><span class="sxs-lookup"><span data-stu-id="e28c4-120">Download a Flow log</span></span>

<span data-ttu-id="e28c4-121">Umístění úložiště toku protokolu se definuje při vytvoření.</span><span class="sxs-lookup"><span data-stu-id="e28c4-121">The storage location of a flow log is defined at creation.</span></span> <span data-ttu-id="e28c4-122">Je vhodné nástroj pro přístup k tyto protokoly toku uložit do účtu úložiště Microsoft Azure Storage Explorer, kterou můžete stáhnout tady: http://storageexplorer.com/</span><span class="sxs-lookup"><span data-stu-id="e28c4-122">A convenient tool to access these flow logs saved to a storage account is Microsoft Azure Storage Explorer, which can be downloaded here:  http://storageexplorer.com/</span></span>

<span data-ttu-id="e28c4-123">Pokud je zadaný účet úložiště, soubory zachytávání paketů ukládají na účet úložiště v následujícím umístění:</span><span class="sxs-lookup"><span data-stu-id="e28c4-123">If a storage account is specified, packet capture files are saved to a storage account at the following location:</span></span>

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

<span data-ttu-id="e28c4-124">Informace o struktuře protokolu najdete v článku [toku skupiny zabezpečení sítě protokolu – přehled](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="e28c4-124">For information about the structure of the log visit [Network Security Group Flow log Overview](network-watcher-nsg-flow-logging-overview.md)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e28c4-125">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e28c4-125">Next Steps</span></span>

<span data-ttu-id="e28c4-126">Zjistěte, jak [vizualizovat protokolů NSG toku pomocí PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="e28c4-126">Learn how to [Visualize your NSG flow logs with PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>

<span data-ttu-id="e28c4-127">Zjistěte, jak [vizualizovat toku protokolů NSG s otevřeným zdrojem nástroje](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="e28c4-127">Learn how to [Visualize your NSG flow logs with open source tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)</span></span>
