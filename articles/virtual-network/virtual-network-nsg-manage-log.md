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
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Analýza protokolu pro skupiny zabezpečení sítě (NSG)

Můžete povolit hello následující kategorie protokolů diagnostiky pro skupiny zabezpečení sítě:

* **Události:** obsahuje položky, u které skupina NSG použitá tooVMs a instance rolí na základě adresy MAC jsou pravidla. Hello stav pro tato pravidla se shromažďují každých 60 sekund.
* **Čítač pravidel:** obsahuje položky pro jednotlivé skupiny NSG počet použití pravidla je použité toodeny nebo povolení provozu.

> [!NOTE]
> Diagnostické protokoly jsou dostupné pouze pro skupiny Nsg nasazené prostřednictvím modelu nasazení Azure Resource Manager hello. Nelze povolit protokolování diagnostiky pro nasazení pomocí modelu nasazení classic hello skupiny Nsg. Lépe porozumět hello dva modely, odkazovat hello [modelech nasazení Azure Principy](../resource-manager-deployment-model.md) článku.

Ve výchozím nastavení je u skupiny Nsg vytvořena prostřednictvím buď modelu nasazení Azure povoleno protokolování aktivity (dříve označované jako auditování nebo provozní protokoly). toodetermine operací, které byly dokončeny podle skupin Nsg v hello protokol aktivit, podívejte se na položky, které obsahují hello následující typy prostředků: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Čtení hello [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) článku toolearn více informací o protokoly aktivity. 

## <a name="enable-diagnostic-logging"></a>Povolení protokolování diagnostiky

Musí být povoleno protokolování diagnostiky pro *každý* NSG chcete toocollect data pro. Hello [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článek vysvětluje, kde lze odesílat diagnostické protokoly. Pokud nemáte existující skupina NSG, dokončení hello kroky v hello [vytvořit skupinu zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md) článku toocreate jeden. Můžete povolit NSG diagnostické protokolování pomocí kteréhokoli hello následující metody:

### <a name="azure-portal"></a>portál Azure

toouse hello portálu tooenable protokolování, přihlášení toohello [portál](https://portal.azure.com). Klikněte na tlačítko **další služby**, pak zadejte *skupin zabezpečení sítě*. Vyberte hello chcete tooenable protokolování pro NSG. Postupujte podle pokynů hello za jiný výpočetní prostředky ve hello [povolení diagnostických protokolů portálu hello](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku. Vyberte **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, nebo obě kategorie protokolů.

### <a name="powershell"></a>PowerShell

tooenable prostředí PowerShell toouse protokolování, postupujte podle pokynů hello v hello [povolení diagnostických protokolů pomocí prostředí PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku. Vyhodnoťte následující informace před zadáním příkazu z článku hello hello:

- Můžete určit hello hodnota toouse pro hello `-ResourceId` parametr nahrazením hello následující [text], podle potřeby, pak zadáte příkaz hello `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. Hello ID výstupu z příkazu hello bude vypadat podobně jako příliš*/subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.
- Pokud chcete pouze toocollect dat z protokolu kategorie přidat `-Categories [category]` toohello konec příkazu hello v hello článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*. Pokud nepoužijete hello `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.

### <a name="azure-command-line-interface-cli"></a>Rozhraní příkazového řádku Azure (CLI)

toouse hello protokolování tooenable rozhraní příkazového řádku, postupujte podle pokynů hello v hello [povolení diagnostických protokolů prostřednictvím rozhraní příkazového řádku](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) článku. Vyhodnoťte následující informace před zadáním příkazu z článku hello hello:

- Můžete určit hello hodnota toouse pro hello `-ResourceId` parametr nahrazením hello následující [text], podle potřeby, pak zadáte příkaz hello `azure network nsg show [resource-group-name] [nsg-name]`. Hello ID výstupu z příkazu hello bude vypadat podobně jako příliš*/subscriptions/ [název odběru Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.
- Pokud chcete pouze toocollect dat z protokolu kategorie přidat `-Categories [category]` toohello konec příkazu hello v hello článku, kde kategorie je buď *NetworkSecurityGroupEvent* nebo *NetworkSecurityGroupRuleCounter*. Pokud nepoužijete hello `-Categories` parametr, shromažďování dat je povolený pro protokolu i kategorií.

## <a name="logged-data"></a>Data protokolu

Data ve formátu JSON je napsán pro oba protokoly. Hello konkrétní data, zapsaná pro každý typ protokolu je uvedené v následující části hello:

### <a name="event-log"></a>V protokolu událostí
Tento protokol obsahuje informace o které skupina NSG pravidla jsou použité tooVMs a instance rolí služby, na základě adresy MAC v cloudu. pro každou jednotlivou událost je protokolována Hello následující příklad dat:

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

### <a name="rule-counter-log"></a>Pravidlo čítače protokolu

Tento protokol obsahuje informace o každé pravidlo tooresources. Hello příklad se protokolují tato data pokaždé, když se použije pravidlo:

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

## <a name="view-and-analyze-logs"></a>Zobrazení a analýza protokolů

toolearn jak aktivity tooview protokolovat data, přečtěte si hello [přehled hello protokol činnosti Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku. toolearn jak Diagnostika tooview protokolovat data, přečtěte si hello [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) článku. Při odeslání diagnostiky dat tooLog analýzy, můžete použít hello [skupinu zabezpečení sítě Azure analytics](../log-analytics/log-analytics-azure-networking-analytics.md) řešení pro správu (preview) pro rozšířené statistiky. 
