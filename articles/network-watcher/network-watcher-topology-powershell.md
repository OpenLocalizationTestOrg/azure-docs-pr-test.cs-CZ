---
title: "sledovací proces sítě Azure topologii aaaView – prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje jak toouse prostředí PowerShell tooquery topologii vaší sítě."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: bd0e882d-8011-45e8-a7ce-de231a69fb85
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 2bc0ecf5baa81a68be53f55c74f362a7bc97116f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-powershell"></a>Zobrazit sledovací proces sítě topologie pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

Funkce topologie Hello sledovací proces sítě poskytuje vizuální reprezentace hello síťových prostředků v předplatném. V portálu hello tuto vizualizaci zobrazí tooyou automaticky. informace o Hello za zobrazení topologie hello portálu hello se dají získat pomocí prostředí PowerShell.
Díky této vlastnosti je informace o topologii hello rozmanitější jako hello data mohou být spotřebovávána jiné nástroje toobuild out hello vizualizace.

propojení Hello je modelován v dva vztahy.

- **Členství ve skupině** – příklad: obsahuje podsíť virtuální sítě obsahuje síťový adaptér
- **Související** – příklad: síťový adaptér je přidružený virtuální počítač

Hello následujícím seznamu jsou uvedeny vlastnosti, které jsou vráceny při dotazování hello topologie REST API.

* **název** – hello název prostředku hello
* **ID** -hello identifikátor uri prostředku hello.
* **umístění** -hello umístění, kde existuje prostředek hello.
* **přidružení** – seznam přidružení toohello odkazuje objekt.
    * **název** -hello název hello odkazuje prostředků.
    * **resourceId** -hello resourceId je identifikátor uri hello hello prostředku, kterou se odkazuje v hello přidružení.
    * **Třída associationType** – tato hodnota se odkazuje hello vztah mezi hello podřízený objekt a nadřazené hello. Platné hodnoty jsou **obsahuje** nebo **přidružené**.

## <a name="before-you-begin"></a>Než začnete

V tomto scénáři použijete hello `Get-AzureRmNetworkWatcherTopology` informace o topologii rutiny tooretrieve hello. K dispozici je také článek o příliš[načíst topologie sítě pomocí rozhraní REST API](network-watcher-topology-rest.md).

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku načte odpověď hello topologie pro danou skupinu prostředků.

## <a name="retrieve-network-watcher"></a>Načtení sledovací proces sítě

prvním krokem Hello je tooretrieve hello sledovací proces sítě instance. Hello `$networkWatcher` proměnné je předán toohello `Get-AzureRmNetworkWatcherTopology` rutiny.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" }
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName
```

## <a name="retrieve-topology"></a>Načtení topologie

Hello `Get-AzureRmNetworkWatcherTopology` rutina načte hello topologie pro danou skupinu prostředků.

```powershell
Get-AzureRmNetworkWatcherTopology -NetworkWatcher $networkWatcher -TargetResourceGroupName testrg
```

## <a name="results"></a>Výsledky

Hello výsledky vrácené mít vlastnost název "prostředky", které obsahuje hello těla odpovědi json pro hello `Get-AzureRmNetworkWatcherTopology` rutiny.  Hello odpovědi obsahuje hello prostředky v hello skupinu zabezpečení sítě a jejich přidružení (který je obsahuje, přidružené).

```json
Id              : 00000000-0000-0000-0000-000000000000
CreatedDateTime : 2/1/2017 7:52:21 PM
LastModified    : 2/1/2017 7:46:18 PM
Resources       : [
                    {
                      "Name": "testrg-vnet",
                      "Id":
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet/subnets/default"
                        }
                      ]
                    },
                    {
                      "Name": "default",
                      "Id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testr
                  g-vnet/subnets/default",
                      "Location": "westcentralus",
                      "Associations": []
                    },
                    {
                      "Name": "testrg-vnet2",
                      "Id": 
                  "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet2",
                      "Location": "westcentralus",
                      "Associations": [
                        {
                          "AssociationType": "Contains",
                          "Name": "default",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/default"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "GatewaySubnet",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworks/testrg-vnet2/subnets/GatewaySubnet"
                        },
                        {
                          "AssociationType": "Contains",
                          "Name": "gateway2",
                          "ResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNe
                  tworkGateways/gateway2"
                        }
                      ]
                    },
                    ...
                  ]
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)


