---
title: "aaaMonitor operace, události a čítače pro skupiny Nsg | Microsoft Docs"
description: "Zjistěte, jak tooenable čítače, události a provozní protokolování pro skupiny Nsg"
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
ms.openlocfilehash: f16f1a0ad693028ee7aba21574b5c8ddfcd27096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a><span data-ttu-id="0ad74-103">Analýza protokolu pro skupiny zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="0ad74-103">Log analytics for network security groups (NSGs)</span></span>

<span data-ttu-id="0ad74-104">Můžete povolit hello následující kategorie protokolů diagnostiky pro skupiny zabezpečení sítě:</span><span class="sxs-lookup"><span data-stu-id="0ad74-104">You can enable hello following diagnostic log categories for NSGs:</span></span>

* <span data-ttu-id="0ad74-105">**Události:** obsahuje položky, u které skupina NSG použitá tooVMs a instance rolí na základě adresy MAC jsou pravidla.</span><span class="sxs-lookup"><span data-stu-id="0ad74-105">**Event:** Contains entries for which NSG rules are applied tooVMs and instance roles based on MAC address.</span></span> <span data-ttu-id="0ad74-106">Hello stav pro tato pravidla se shromažďují každých 60 sekund.</span><span class="sxs-lookup"><span data-stu-id="0ad74-106">hello status for these rules is collected every 60 seconds.</span></span>
* <span data-ttu-id="0ad74-107">**Čítač pravidel:** obsahuje položky pro jednotlivé skupiny NSG počet použití pravidla je použité toodeny nebo povolení provozu.</span><span class="sxs-lookup"><span data-stu-id="0ad74-107">**Rule counter:** Contains entries for how many times each NSG rule is applied toodeny or allow traffic.</span></span>

> [!NOTE]
> <span data-ttu-id="0ad74-108">Diagnostické protokoly jsou dostupné pouze pro skupiny Nsg nasazené prostřednictvím modelu nasazení Azure Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="0ad74-108">Diagnostic logs are only available for NSGs deployed through hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="0ad74-109">Nelze povolit protokolování diagnostiky pro nasazení pomocí modelu nasazení classic hello skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="0ad74-109">You cannot enable diagnostic logging for NSGs deployed through hello classic deployment model.</span></span> <span data-ttu-id="0ad74-110">Lépe porozumět hello dva modely, odkazovat hello [modelech nasazení Azure Principy](../resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0ad74-110">For a better understanding of hello two models, reference hello [Understanding Azure deployment models](../resource-manager-deployment-model.md) article.</span></span>

<span data-ttu-id="0ad74-111">Ve výchozím nastavení je u skupiny Nsg vytvořena prostřednictvím buď modelu nasazení Azure povoleno protokolování aktivity (dříve označované jako auditování nebo provozní protokoly).</span><span class="sxs-lookup"><span data-stu-id="0ad74-111">Activity logging (previously known as audit or operational logs) is enabled by default for NSGs created through either Azure deployment model.</span></span> <span data-ttu-id="0ad74-112">toodetermine operací, které byly dokončeny podle skupin Nsg v hello protokol aktivit, podívejte se na položky, které obsahují hello následující typy prostředků:</span><span class="sxs-lookup"><span data-stu-id="0ad74-112">toodetermine which operations were completed on NSGs in hello activity log, look for entries that contain hello following resource types:</span></span> 

- <span data-ttu-id="0ad74-113">Microsoft.ClassicNetwork/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="0ad74-113">Microsoft.ClassicNetwork/networkSecurityGroups</span></span> 
- <span data-ttu-id="0ad74-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="0ad74-114">Microsoft.ClassicNetwork/networkSecurityGroups/securityRules</span></span>
- <span data-ttu-id="0ad74-115">Microsoft.Network/networkSecurityGroups</span><span class="sxs-lookup"><span data-stu-id="0ad74-115">Microsoft.Network/networkSecurityGroups</span></span>
- <span data-ttu-id="0ad74-116">Microsoft.Network/networkSecurityGroups/securityRules</span><span class="sxs-lookup"><span data-stu-id="0ad74-116">Microsoft.Network/networkSecurityGroups/securityRules</span></span> 

<span data-ttu-id="0ad74-117">Čtení hello [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) článku toolearn více informací o protokoly aktivity.</span><span class="sxs-lookup"><span data-stu-id="0ad74-117">Read hello [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) article toolearn more about activity logs.</span></span> 

## <a name="enable-diagnostic-logging"></a><span data-ttu-id="0ad74-118">Povolení protokolování diagnostiky</span><span class="sxs-lookup"><span data-stu-id="0ad74-118">Enable diagnostic logging</span></span>

<span data-ttu-id="0ad74-119">Musí být povoleno protokolování diagnostiky pro *každý* NSG chcete toocollect data pro.</span><span class="sxs-lookup"><span data-stu-id="0ad74-119">Diagnostic logging must be enabled for *each* NSG you want toocollect data for.</span></span> <span data-ttu-id="0ad74-120">Hello [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článek vysvětluje, kde lze odesílat diagnostické protokoly.</span><span class="sxs-lookup"><span data-stu-id="0ad74-120">hello [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article explains where diagnostic logs can be sent.</span></span> <span data-ttu-id="0ad74-121">Pokud nemáte existující skupina NSG, dokončení hello kroky v hello [vytvořit skupinu zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md) článku toocreate jeden.</span><span class="sxs-lookup"><span data-stu-id="0ad74-121">If you don't have an existing NSG, complete hello steps in hello [Create a network security group](virtual-networks-create-nsg-arm-pportal.md) article toocreate one.</span></span> <span data-ttu-id="0ad74-122">Můžete povolit NSG diagnostické protokolování pomocí kteréhokoli hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="0ad74-122">You can enable NSG diagnostic logging using any of hello following methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="0ad74-123">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0ad74-123">Azure portal</span></span>

<span data-ttu-id="0ad74-124">toouse hello portálu tooenable protokolování, přihlášení toohello [portál](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ad74-124">toouse hello portal tooenable logging, login toohello [portal](https://portal.azure.com).</span></span> <span data-ttu-id="0ad74-125">Klikněte na tlačítko **další služby**, pak zadejte *skupin zabezpečení sítě*.</span><span class="sxs-lookup"><span data-stu-id="0ad74-125">Click **More services**, then type *network security groups*.</span></span> <span data-ttu-id="0ad74-126">Vyberte hello chcete tooenable protokolování pro NSG.</span><span class="sxs-lookup"><span data-stu-id="0ad74-126">Select hello NSG you want tooenable logging for.</span></span> <span data-ttu-id="0ad74-127">Postupujte podle pokynů hello za jiný výpočetní prostředky ve hello [povolení diagnostických protokolů portálu hello](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku.</span><span class="sxs-lookup"><span data-stu-id="0ad74-127">Follow hello instructions for non-compute resources in hello [Enable diagnostic logs in hello portal](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="0ad74-128">Vyberte **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, nebo obě kategorie protokolů.</span><span class="sxs-lookup"><span data-stu-id="0ad74-128">Select **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, or both categories of logs.</span></span>

### <a name="powershell"></a><span data-ttu-id="0ad74-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0ad74-129">PowerShell</span></span>

<span data-ttu-id="0ad74-130">tooenable prostředí PowerShell toouse protokolování, postupujte podle pokynů hello v hello [povolení diagnostických protokolů pomocí prostředí PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku.</span><span class="sxs-lookup"><span data-stu-id="0ad74-130">toouse PowerShell tooenable logging, follow hello instructions in hello [Enable diagnostic logs via PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="0ad74-131">Vyhodnoťte následující informace před zadáním příkazu z článku hello hello:</span><span class="sxs-lookup"><span data-stu-id="0ad74-131">Evaluate hello following information before entering a command from hello article:</span></span>

- <span data-ttu-id="0ad74-132">Můžete určit hello hodnota toouse pro hello `-ResourceId` parametr nahrazením hello následující [text], podle potřeby, pak zadáte příkaz hello `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span><span class="sxs-lookup"><span data-stu-id="0ad74-132">You can determine hello value toouse for hello `-ResourceId` parameter by replacing hello following [text], as appropriate, then entering hello command `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`.</span></span> <span data-ttu-id="0ad74-133">Hello ID výstupu z příkazu hello bude vypadat podobně jako příliš*/subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.</span><span class="sxs-lookup"><span data-stu-id="0ad74-133">hello ID output from hello command looks similar too*/subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="0ad74-134">Pokud chcete pouze toocollect dat z protokolu kategorie přidat `-Categories [category]` toohello konec příkazu hello v hello článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="0ad74-134">If you only want toocollect data from log category add `-Categories [category]` toohello end of hello command in hello article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="0ad74-135">Pokud nepoužijete hello `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.</span><span class="sxs-lookup"><span data-stu-id="0ad74-135">If you don't use hello `-Categories` parameter, data collection is enabled for both log categories.</span></span>

### <a name="azure-command-line-interface-cli"></a><span data-ttu-id="0ad74-136">Rozhraní příkazového řádku Azure (CLI)</span><span class="sxs-lookup"><span data-stu-id="0ad74-136">Azure command-line interface (CLI)</span></span>

<span data-ttu-id="0ad74-137">toouse hello protokolování tooenable rozhraní příkazového řádku, postupujte podle pokynů hello v hello [povolení diagnostických protokolů prostřednictvím rozhraní příkazového řádku](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku.</span><span class="sxs-lookup"><span data-stu-id="0ad74-137">toouse hello CLI tooenable logging, follow hello instructions in hello [Enable diagnostic logs via CLI](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) article.</span></span> <span data-ttu-id="0ad74-138">Vyhodnoťte následující informace před zadáním příkazu z článku hello hello:</span><span class="sxs-lookup"><span data-stu-id="0ad74-138">Evaluate hello following information before entering a command from hello article:</span></span>

- <span data-ttu-id="0ad74-139">Můžete určit hello hodnota toouse pro hello `-ResourceId` parametr nahrazením hello následující [text], podle potřeby, pak zadáte příkaz hello `azure network nsg show [resource-group-name] [nsg-name]`.</span><span class="sxs-lookup"><span data-stu-id="0ad74-139">You can determine hello value toouse for hello `-ResourceId` parameter by replacing hello following [text], as appropriate, then entering hello command `azure network nsg show [resource-group-name] [nsg-name]`.</span></span> <span data-ttu-id="0ad74-140">Hello ID výstupu z příkazu hello bude vypadat podobně jako příliš*/subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.</span><span class="sxs-lookup"><span data-stu-id="0ad74-140">hello ID output from hello command looks similar too*/subscriptions/[Subscription Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG name]*.</span></span>
- <span data-ttu-id="0ad74-141">Pokud chcete pouze toocollect dat z protokolu kategorie přidat `-Categories [category]` toohello konec příkazu hello v hello článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*.</span><span class="sxs-lookup"><span data-stu-id="0ad74-141">If you only want toocollect data from log category add `-Categories [category]` toohello end of hello command in hello article, where category is either *NetworkSecurityGroupEvent* or *NetworkSecurityGroupRuleCounter*.</span></span> <span data-ttu-id="0ad74-142">Pokud nepoužijete hello `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.</span><span class="sxs-lookup"><span data-stu-id="0ad74-142">If you don't use hello `-Categories` parameter, data collection is enabled for both log categories.</span></span>

## <a name="logged-data"></a><span data-ttu-id="0ad74-143">Data protokolu</span><span class="sxs-lookup"><span data-stu-id="0ad74-143">Logged data</span></span>

<span data-ttu-id="0ad74-144">Data ve formátu JSON je napsán pro oba protokoly.</span><span class="sxs-lookup"><span data-stu-id="0ad74-144">JSON-formatted data is written for both logs.</span></span> <span data-ttu-id="0ad74-145">Hello konkrétní data, zapsaná pro každý typ protokolu je uvedené v následující části hello:</span><span class="sxs-lookup"><span data-stu-id="0ad74-145">hello specific data written for each log type is listed in hello following sections:</span></span>

### <a name="event-log"></a><span data-ttu-id="0ad74-146">V protokolu událostí</span><span class="sxs-lookup"><span data-stu-id="0ad74-146">Event log</span></span>
<span data-ttu-id="0ad74-147">Tento protokol obsahuje informace o které skupina NSG pravidla jsou použité tooVMs a instance rolí služby, na základě adresy MAC v cloudu.</span><span class="sxs-lookup"><span data-stu-id="0ad74-147">This log contains information about which NSG rules are applied tooVMs and cloud service role instances, based on MAC address.</span></span> <span data-ttu-id="0ad74-148">pro každou jednotlivou událost je protokolována Hello následující příklad dat:</span><span class="sxs-lookup"><span data-stu-id="0ad74-148">hello following example data is logged for each event:</span></span>

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

### <a name="rule-counter-log"></a><span data-ttu-id="0ad74-149">Pravidlo čítače protokolu</span><span class="sxs-lookup"><span data-stu-id="0ad74-149">Rule counter log</span></span>

<span data-ttu-id="0ad74-150">Tento protokol obsahuje informace o každé pravidlo tooresources.</span><span class="sxs-lookup"><span data-stu-id="0ad74-150">This log contains information about each rule applied tooresources.</span></span> <span data-ttu-id="0ad74-151">Hello příklad se protokolují tato data pokaždé, když se použije pravidlo:</span><span class="sxs-lookup"><span data-stu-id="0ad74-151">hello following example data is logged each time a rule is applied:</span></span>

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

## <a name="view-and-analyze-logs"></a><span data-ttu-id="0ad74-152">Zobrazení a analýza protokolů</span><span class="sxs-lookup"><span data-stu-id="0ad74-152">View and analyze logs</span></span>

<span data-ttu-id="0ad74-153">toolearn jak aktivity tooview protokolovat data, přečtěte si hello [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0ad74-153">toolearn how tooview activity log data, read hello [Overview of hello Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="0ad74-154">toolearn jak Diagnostika tooview protokolovat data, přečtěte si hello [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0ad74-154">toolearn how tooview diagnostic log data, read hello [Overview of Azure Diagnostic Logs](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) article.</span></span> <span data-ttu-id="0ad74-155">Při odeslání diagnostiky dat tooLog analýzy, můžete použít hello [skupinu zabezpečení sítě Azure analytics](../log-analytics/log-analytics-azure-networking-analytics.md) řešení pro správu (preview) pro rozšířené statistiky.</span><span class="sxs-lookup"><span data-stu-id="0ad74-155">If you send diagnostics data tooLog Analytics, you can use hello [Azure Network Security Group analytics](../log-analytics/log-analytics-azure-networking-analytics.md) (preview) management solution for enhanced insights.</span></span> 
