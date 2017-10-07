---
title: "topologie sledovací proces sítě Azure aaaView - REST API | Microsoft Docs"
description: "Tento článek popisuje, jak toouse REST API tooquery topologii vaší sítě."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 39292025bcd561f007c9e31271b1389be48ea79f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a>Zobrazení topologie sledovací proces sítě pomocí rozhraní REST API

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

V tomto scénáři můžete načíst informace o topologii hello. ARMclient je použité toocall hello REST API pomocí prostředí PowerShell. ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku načte odpověď hello topologie pro danou skupinu prostředků.

## <a name="log-in-with-armclient"></a>Přihlaste se pomocí ARMClient

Přihlaste se pomocí svých přihlašovacích údajů Azure tooarmclient.

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a>Načtení topologie

Následující ukázka Hello požadavků hello topologie hello REST API.  je třeba Hello parametrizované tooallow flexibilitu při vytváření příklad.  Nahraďte všechny hodnoty s \< \> které obaluje je.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name toorun topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

Hello následující odpověď je příkladem zkrácení odpovědi, která je vrácena při načtení topologie pro skupina prostředků:

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toovisualize vaše skupina NSG toku protokoly s Power BI navštivte stránky [vizualizovat NSG toků protokoly s Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

