---
title: "aaaAutomate NSG auditování s zobrazení skupiny zabezpečení sledovací proces sítě Azure | Microsoft Docs"
description: "Tato stránka obsahuje pokyny, jak tooconfigure auditování skupinu zabezpečení sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 78a01bcf-74fe-402a-9812-285f3501f877
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 24fc418c433fceaf55a74b7c3b0e354dc46c8729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-nsg-auditing-with-azure-network-watcher-security-group-view"></a>Automatizovat NSG auditování s zobrazení skupiny zabezpečení sledovací proces sítě Azure

Zákazníci se často potýkají s hello výzvy ověřování postavení zabezpečení hello svoji infrastrukturu. Tento problém se neliší pro jejich virtuální počítače v Azure. Je důležité toohave podobné profil zabezpečení na základě hello skupina zabezpečení sítě (NSG) pravidel použít. Pomocí hello zobrazení skupiny zabezpečení, můžete nyní získat hello seznam pravidla použít tooa virtuálních počítačů v rámci skupiny NSG. Můžete definovat zlaté profil zabezpečení NSG a zahájit zobrazení skupiny zabezpečení na týdenní cadence a porovnání hello výstup toohello zlaté profil a vytvořit sestavu. Tímto způsobem můžete identifikovat snadno všechny hello virtuálních počítačů, které nejsou v souladu s toohello předepsané profil zabezpečení.

Pokud jste obeznámeni s skupin zabezpečení sítě, navštivte [Přehled zabezpečení sítě](../virtual-network/virtual-networks-nsg.md)

## <a name="before-you-begin"></a>Než začnete

V tomto scénáři můžete porovnat skupiny zabezpečení toohello známé dobré směrného plánu zobrazit výsledky vrácené pro virtuální počítač.

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě. scénář Hello také předpokládá, že skupina prostředků se platný virtuální počítač existuje toobe použít.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku získá hello zobrazení skupiny zabezpečení pro virtuální počítač.

V tomto scénáři provedete následující:

- Načíst sadu známé dobré pravidel
- Načíst virtuální počítač s Rest API
- Získat zobrazení skupiny zabezpečení pro virtuální počítač
- Vyhodnocení odpovědi

## <a name="retrieve-rule-set"></a>Načtení sady pravidel

prvním krokem Hello v tomto příkladu je toowork s stávajících standardních hodnot. Hello následující příklad je některé json extrahoval z existující skupinu zabezpečení sítě pomocí hello `Get-AzureRmNetworkSecurityGroup` rutinu, která je použit jako účaří hello v tomto příkladu.

```json
[
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "3389",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1000,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "default-allow-rdp",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/default-allow-rdp"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "111",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1010,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "MyRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/MyRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "*",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "112",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Allow",
        "Priority":  1020,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "My2ndRuleDoNotDelete",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/My2ndRuleDoNotDelete"
    },
    {
        "Description":  null,
        "Protocol":  "TCP",
        "SourcePortRange":  "*",
        "DestinationPortRange":  "5672",
        "SourceAddressPrefix":  "*",
        "DestinationAddressPrefix":  "*",
        "Access":  "Deny",
        "Priority":  1030,
        "Direction":  "Inbound",
        "ProvisioningState":  "Succeeded",
        "Name":  "ThisRuleNeedsToStay",
        "Etag":  "W/\"d8859256-1c4c-4b93-ba7d-73d9bf67c4f1\"",
        "Id":  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/testvm1-nsg/securityRules/ThisRuleNeedsToStay"
    }
]
```

## <a name="convert-rule-set-toopowershell-objects"></a>Převést objekty tooPowerShell sad pravidel

V tomto kroku jsme jsou čtení souboru json, který byl vytvořen již dříve hello pravidla, která jsou očekávané toobe na hello skupinu zabezpečení sítě v tomto příkladu.

```powershell
$nsgbaserules = Get-Content -Path C:\temp\testvm1-nsg.json | ConvertFrom-Json
```

## <a name="retrieve-network-watcher"></a>Načtení sledovací proces sítě

dalším krokem Hello je tooretrieve hello sledovací proces sítě instance. Hello `$networkWatcher` proměnné je předán toohello `AzureRmNetworkWatcherSecurityGroupView` rutiny.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>Získat virtuální počítač

Virtuální počítač je požadovaná toorun hello `Get-AzureRmNetworkWatcherSecurityGroupView` rutiny proti. Následující ukázka Hello získá objektu virtuálního počítače.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="retrieve-security-group-view"></a>Načtení zobrazení skupiny zabezpečení

dalším krokem Hello je tooretrieve hello zabezpečení skupiny zobrazení výsledků. Tento výsledek je json "základní" porovnání toohello, zobrazená dříve.

```powershell
$secgroup = Get-AzureRmNetworkWatcherSecurityGroupView -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id
```

## <a name="analyzing-hello-results"></a>Analýza výsledků hello

odpověď Hello se seskupují po síťových rozhraní. Hello různých typů pravidel vrátil jsou platné a výchozí pravidla zabezpečení. výsledek Hello je další členěné podle jak se používají, buď na podsíť, nebo virtuální síťový adaptér.

Hello následující skript prostředí PowerShell porovná hello výsledky hello zobrazení skupiny zabezpečení tooan existující výstup skupinu NSG. Hello následující příklad je jednoduchý příklad, jak je možné porovnávat hello výsledky s `Compare-Object` rutiny.

```powershell
Compare-Object -ReferenceObject $nsgbaserules `
-DifferenceObject $secgroup.NetworkInterfaces[0].NetworkInterfaceSecurityRules `
-Property Name,Description,Protocol,SourcePortRange,DestinationPortRange,SourceAddressPrefix,DestinationAddressPrefix,Access,Priority,Direction
```

Následující ukázka Hello je výsledek hello. Uvidíte dvě hello pravidel, které byly první sada pravidel hello nebyly nalezeny v porovnání hello.

```
Name                     : My2ndRuleDoNotDelete
Description              : 
Protocol                 : *
SourcePortRange          : *
DestinationPortRange     : 112
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Allow
Priority                 : 1020
Direction                : Inbound
SideIndicator            : <=

Name                     : ThisRuleNeedsToStay
Description              : 
Protocol                 : TCP
SourcePortRange          : *
DestinationPortRange     : 5672
SourceAddressPrefix      : *
DestinationAddressPrefix : *
Access                   : Deny
Priority                 : 1030
Direction                : Inbound
SideIndicator            : <=
```

## <a name="next-steps"></a>Další kroky

Pokud se změnila nastavení, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tootrack dolů hello pravidla zabezpečení sítě skupiny a zabezpečení, které jsou v.













