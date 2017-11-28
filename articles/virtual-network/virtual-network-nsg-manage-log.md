---
title: "Sledování operací, události a čítače pro skupiny Nsg | Microsoft Docs"
description: "Informace o povolení čítače, události a provozní protokolování pro skupiny Nsg"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: 552f37dd704de25159bc0f0ad34fdae9ed8b73f5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a><span data-ttu-id="0a14f-103">Analýza protokolu pro skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="0a14f-103">Log analytics for network security groups (NSGs)</span></span>

<span data-ttu-id="0a14f-104">Pro skupiny Nsg můžete povolit v těchto kategoriích protokolů diagnostiky:</span><span class="sxs-lookup"><span data-stu-id="0a14f-104">You can enable the following diagnostic log categories for NSGs:</span></span>

* <span data-ttu-id="0a14f-105">**Události:** obsahuje položky, pro které skupina NSG pravidla, použije se pro virtuální počítače a instance rolí na základě adresy MAC.</span><span class="sxs-lookup"><span data-stu-id="0a14f-105">**Event:** Contains entries for which NSG rules are applied to VMs and instance roles based on MAC address.</span></span> <span data-ttu-id="0a14f-106">Stav pro tato pravidla se shromažďují každých 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="0a14f-106">The status for these rules is collected every 60 seconds.</span></span>
* <span data-ttu-id="0a14f-107">**Čítač pravidel:** obsahuje položky pro jednotlivé skupiny NSG počet použití pravidla se aplikuje zakázat nebo povolit provoz.</span><span class="sxs-lookup"><span data-stu-id="0a14f-107">**Rule counter:** Contains entries for how many times each NSG rule is applied to deny or allow traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="0a14f-108">Diagnostické protokoly jsou dostupné pouze pro skupiny Nsg nasazené prostřednictvím modelu nasazení Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0a14f-108">Diagnostic logs are only available for NSGs deployed through the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="0a14f-109">Nelze povolit protokolování diagnostiky pro skupiny Nsg nasazené prostřednictvím modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="0a14f-109">You cannot enable diagnostic logging for NSGs deployed through the classic deployment model.</span></span> <span data-ttu-id="0a14f-110">Lépe porozumět obou modelů, odkazovat [modelech nasazení Azure Principy](../resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0a14f-110">For a better understanding of the two models, reference the [Understanding Azure deployment models](../resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="0a14f-111">Ve výchozím nastavení je u skupiny Nsg vytvořena prostřednictvím buď modelu nasazení Azure povoleno protokolování aktivity (dříve označované jako auditování nebo provozní protokoly).</span><span class="sxs-lookup"><span data-stu-id="0a14f-111">Activity logging (previously known as audit or operational logs) is enabled by default for NSGs created through either Azure deployment model.</span></span> <span data-ttu-id="0a14f-112">Pokud chcete zjistit, které operace byly dokončeny podle skupin Nsg v protokolu aktivit, vyhledejte položky, které obsahují následující typy prostředků:</span><span class="sxs-lookup"><span data-stu-id="0a14f-112">To determine which operations were completed on NSGs in the activity log, look for entries that contain the following resource types:</span></span> 

- <span data-ttu-id="0a14f-113">Microsoft.ClassicNetwork/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="0a14f-113">Microsoft.ClassicNetwork/networkSecurityGroups</span></span> 
- <span data-ttu-id="0a14f-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="0a14f-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span></span>
- <span data-ttu-id="0a14f-115">Microsoft.Network/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="0a14f-115">Microsoft.Network/networkSecurityGroups</span></span>
- <span data-ttu-id="0a14f-116">Microsoft.Network/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="0a14f-116">Microsoft.Network/networkSecurityGroups/securityRules</span></span> 

<span data-ttu-id="0a14f-117">Pro čtení [přehled protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) článku Další informace o protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="0a14f-117">Read the [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) article to learn more about activity logs.</span></span> 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="0a14f-118">Povolení protokolování diagnostiky</span><span class="sxs-lookup"><span data-stu-id="0a14f-118">Enable diagnostic logging</span></span>

<span data-ttu-id="0a14f-119">Musí být povoleno protokolování diagnostiky pro *každý* NSG, které chcete shromažďovat data.</span><span class="sxs-lookup"><span data-stu-id="0a14f-119">Diagnostic logging must be enabled for *each* NSG you want to collect data for.</span></span> <span data-ttu-id="0a14f-120">[Přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článek vysvětluje, kde lze odesílat diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="0a14f-120">The [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article explains where diagnostic logs can be sent.</span></span> <span data-ttu-id="0a14f-121">Pokud nemáte existující skupina NSG, proveďte kroky v [vytvořit skupinu zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md) článek k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="0a14f-121">If you don't have an existing NSG, complete the steps in the [Create a network security group](virtual-networks-create-nsg-arm-pportal.md) article to create one.</span></span> <span data-ttu-id="0a14f-122">Můžete povolit NSG diagnostické protokolování pomocí kteréhokoli z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="0a14f-122">You can enable NSG diagnostic logging using any of the following methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="0a14f-123">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0a14f-123">Azure portal</span></span>

<span data-ttu-id="0a14f-124">Povolení protokolování, přihlášení, které budou používat portál [portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0a14f-124">To use the portal to enable logging, login to the [portal](https://portal.azure.com).</span></span> <span data-ttu-id="0a14f-125">Klikněte na tlačítko **další služby**, pak zadejte *skupin zabezpečení sítě*.</span><span class="sxs-lookup"><span data-stu-id="0a14f-125">Click **More services**, then type *network security groups*.</span></span> <span data-ttu-id="0a14f-126">Vyberte NSG, které chcete povolit protokolování.</span><span class="sxs-lookup"><span data-stu-id="0a14f-126">Select the NSG you want to enable logging for.</span></span> <span data-ttu-id="0a14f-127">Postupujte podle pokynů pro jiný výpočetní prostředky v [povolení diagnostických protokolů na portálu](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku.</span><span class="sxs-lookup"><span data-stu-id="0a14f-127">Follow the instructions for non-compute resources in the [Enable diagnostic logs in the portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="0a14f-128">Vyberte **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, nebo obě kategorie protokolů.</span><span class="sxs-lookup"><span data-stu-id="0a14f-128">Select **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, or both categories of logs.</span></span>

### <a name="powershell"></a><span data-ttu-id="0a14f-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0a14f-129">PowerShell</span></span>

<span data-ttu-id="0a14f-130">Chcete-li povolit protokolování pomocí prostředí PowerShell, postupujte podle pokynů v [povolení diagnostických protokolů pomocí prostředí PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku.</span><span class="sxs-lookup"><span data-stu-id="0a14f-130">To use PowerShell to enable logging, follow the instructions in the [Enable diagnostic logs via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="0a14f-131">Vyhodnoťte následující informace před zadáním příkazu z článku:</span><span class="sxs-lookup"><span data-stu-id="0a14f-131">Evaluate the following information before entering a command from the article:</span></span>

- <span data-ttu-id="0a14f-132">Můžete určit hodnotu pro `-ResourceId` parametr nahrazením následující [text], podle potřeby, pak zadáte příkaz `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span><span class="sxs-lookup"><span data-stu-id="0a14f-132">You can determine the value to use for the `-ResourceId` parameter by replacing the following [text], as appropriate, then entering the command `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span></span> <span data-ttu-id="0a14f-133">ID výstup z tohoto příkazu bude vypadat podobně jako */subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.</span><span class="sxs-lookup"><span data-stu-id="0a14f-133">The ID output from the command looks similar to */subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="0a14f-134">Pokud chcete shromažďovat data z protokolu kategorie přidat `-Categories [category]` na konec příkazu v následujícím článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="0a14f-134">If you only want to collect data from log category add `-Categories [category]` to the end of the command in the article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="0a14f-135">Pokud nepoužijete `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.</span><span class="sxs-lookup"><span data-stu-id="0a14f-135">If you don't use the `-Categories` parameter, data collection is enabled for both log categories.</span></span>

### <a name="azure-command-line-interface-cli"></a><span data-ttu-id="0a14f-136">Rozhraní příkazového řádku Azure (CLI)</span><span class="sxs-lookup"><span data-stu-id="0a14f-136">Azure command-line interface (CLI)</span></span>

<span data-ttu-id="0a14f-137">Chcete-li povolit protokolování pomocí rozhraní příkazového řádku, postupujte podle pokynů [povolení diagnostických protokolů prostřednictvím rozhraní příkazového řádku](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku.</span><span class="sxs-lookup"><span data-stu-id="0a14f-137">To use the CLI to enable logging, follow the instructions in the [Enable diagnostic logs via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="0a14f-138">Vyhodnoťte následující informace před zadáním příkazu z článku:</span><span class="sxs-lookup"><span data-stu-id="0a14f-138">Evaluate the following information before entering a command from the article:</span></span>

- <span data-ttu-id="0a14f-139">Můžete určit hodnotu pro `-ResourceId` parametr nahrazením následující [text], podle potřeby, pak zadáte příkaz `azure network nsg show [resource-group-name] [nsg-name]`.</span><span class="sxs-lookup"><span data-stu-id="0a14f-139">You can determine the value to use for the `-ResourceId` parameter by replacing the following [text], as appropriate, then entering the command `azure network nsg show [resource-group-name] [nsg-name]`.</span></span> <span data-ttu-id="0a14f-140">ID výstup z tohoto příkazu bude vypadat podobně jako */subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.</span><span class="sxs-lookup"><span data-stu-id="0a14f-140">The ID output from the command looks similar to */subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="0a14f-141">Pokud chcete shromažďovat data z protokolu kategorie přidat `-Categories [category]` na konec příkazu v následujícím článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="0a14f-141">If you only want to collect data from log category add `-Categories [category]` to the end of the command in the article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="0a14f-142">Pokud nepoužijete `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.</span><span class="sxs-lookup"><span data-stu-id="0a14f-142">If you don't use the `-Categories` parameter, data collection is enabled for both log categories.</span></span>

## <a name="logged-data"></a><span data-ttu-id="0a14f-143">Data protokolu</span><span class="sxs-lookup"><span data-stu-id="0a14f-143">Logged data</span></span>

<span data-ttu-id="0a14f-144">Data ve formátu JSON je napsán pro oba protokoly.</span><span class="sxs-lookup"><span data-stu-id="0a14f-144">JSON-formatted data is written for both logs.</span></span> <span data-ttu-id="0a14f-145">Konkrétní data, zapsaná pro každý typ protokolu je uvedené v následujících částech:</span><span class="sxs-lookup"><span data-stu-id="0a14f-145">The specific data written for each log type is listed in the following sections:</span></span>

### <a name="event-log"></a><span data-ttu-id="0a14f-146">V protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="0a14f-146">Event log</span></span>
<span data-ttu-id="0a14f-147">Tento protokol obsahuje informace o které skupina NSG pravidla, použije se pro virtuální počítače a instance role služby, na základě adresy MAC v cloudu.</span><span class="sxs-lookup"><span data-stu-id="0a14f-147">This log contains information about which NSG rules are applied to VMs and cloud service role instances, based on MAC address.</span></span> <span data-ttu-id="0a14f-148">Pro všechny události se protokolují tato data příklad:</span><span class="sxs-lookup"><span data-stu-id="0a14f-148">The following example data is logged for each event:</span></span>

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a><span data-ttu-id="0a14f-149">Pravidlo čítače protokolu</span><span class="sxs-lookup"><span data-stu-id="0a14f-149">Rule counter log</span></span>

<span data-ttu-id="0a14f-150">Tento protokol obsahuje informace o každé pravidlo použito pro prostředky.</span><span class="sxs-lookup"><span data-stu-id="0a14f-150">This log contains information about each rule applied to resources.</span></span> <span data-ttu-id="0a14f-151">Pokaždé, když se použije pravidlo se protokolují tato data příklad:</span><span class="sxs-lookup"><span data-stu-id="0a14f-151">The following example data is logged each time a rule is applied:</span></span>

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a><span data-ttu-id="0a14f-152">Zobrazení a analýza protokolů</span><span class="sxs-lookup"><span data-stu-id="0a14f-152">View and analyze logs</span></span>

<span data-ttu-id="0a14f-153">Zjistěte, jak zobrazit data protokolu aktivit, přečtěte si téma [přehled protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0a14f-153">To learn how to view activity log data, read the [Overview of the Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="0a14f-154">Zjistěte, jak zobrazit data protokolů diagnostiky, přečtěte si téma [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0a14f-154">To learn how to view diagnostic log data, read the [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="0a14f-155">Pokud odešlete diagnostická data k analýze protokolů, můžete použít [skupinu zabezpečení sítě Azure analytics](../log-analytics/log-analytics-azure-networking-analytics.md) řešení pro správu (preview) pro rozšířené statistiky.</span><span class="sxs-lookup"><span data-stu-id="0a14f-155">If you send diagnostics data to Log Analytics, you can use the [Azure Network Security Group analytics](../log-analytics/log-analytics-azure-networking-analytics.md) (preview) management solution for enhanced insights.</span></span> 
