---
title: "Další směrování s Azure sítě sledovacích procesů další směrování - REST aaaFind | Microsoft Docs"
description: "Tento článek popisuje, jak zjistíte, jaké hello dalšího směrování typ je a ip adresu pomocí další směrování pomocí hello REST API služby Azure"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 2216c059-45ba-4214-8304-e56769b779a6
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: a2b61b355aae8ae513ebd44837184fbc6cfd668c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-aure-network-watcher-using-azure-rest-api"></a>Zjistěte, jaký typ hello dalšího směrování je díky funkci další segment hello v sledovací proces sítě Azure pomocí rozhraní REST API Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI 1.0](network-watcher-check-next-hop-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-next-hop-cli.md)
> - [Rozhraní API Azure REST](network-watcher-check-next-hop-rest.md)

Další směrování je funkce sledovací proces sítě, která poskytuje možnost hello získat hello typ dalšího směrování a IP adresu podle zadaný virtuální počítač. Tato možnost je užitečná při určení toho, jestli je brána, internet nebo cílové tooits tooget virtuální sítě prochází přes odchozího provozu z virtuálního počítače.

## <a name="before-you-begin"></a>Než začnete

ARMclient je použité toocall hello REST API pomocí prostředí PowerShell. ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku používá další směrování, funkce sledovací proces sítě, který vyhledá hello typ dalšího směrování a IP adresu pro prostředek. toolearn Další informace o další směrování, navštivte [další směrování přehled](network-watcher-next-hop-overview.md).

V tomto scénáři provedete následující:

* Načtěte hello další směrování pro virtuální počítač.

## <a name="log-in-with-armclient"></a>Přihlaste se pomocí ARMClient

Přihlaste se pomocí svých přihlašovacích údajů Azure tooarmclient.

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Načíst virtuální počítač

Spusťte následující skript tooreturn hello virtuálního počítače. Tyto informace budete potřebovat pro spuštění dalším místě směrování.

Následující kód Hello potřebuje hodnoty pro hello následující proměnné:

- **ID předplatného** -hello toouse Id předplatného.
- **Název skupiny prostředků** – hello název skupiny prostředků, která obsahuje virtuální počítače.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Id hello hello virtuálního počítače hello následující výstup, se používá v hello následující ukázka:

```json
...
,
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="get-next-hop"></a>Získat další směrování

Po vytvoření hello autorizační hlavičky je možné načíst hello dalšího přechodu z virtuálního počítače. Hello následující hodnoty se musí nahradit pro toowork příklad kódu hello.

> [!Important]
> Volání rozhraní API REST sledovací proces sítě hello název skupiny prostředků v žádosti o hello, že je identifikátor URI hello skupinu prostředků, která obsahuje hello sledovací proces sítě, není hello prostředků, kterou provádíte hello diagnostiky akce na.

```powershell
$sourceIP = "10.0.0.4"
$destinationIP = "8.8.8.8"
$resourceGroupName = <resource group name>
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'sourceIpAddress': '${sourceIP}',
    'destinationIpAddress': '${destinationIP}',
    'targetNicResourceId' : '${targetNic}'
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/nextHop?api-version=2016-12-01" $requestBody
```

> [!NOTE]
> Další směrování vyžaduje, aby hello prostředků virtuálního počítače je přidělená toorun.

## <a name="results"></a>Výsledky

Hello následující fragment kódu je příklad výstupu hello přijata. Hello výsledky obsahují hello následující hodnoty:

* **nextHopType** – tato hodnota je jedním z následujících hodnot hello: Internet, VirtualAppliance, VirtualNetworkGateway, VnetLocal, HyperNetGateway nebo žádný.
* **nextHopIpAddress** -hello IP adresa dalšího směrování hello.
* **routeTableId** – hodnota hello je buď identifikátoru uri hello směrovací tabulka, která je spojená s trasou hello, nebo když není uživatelem definované trasy je hodnota definovaná hello *systémová trasa* je vrácen.

Hello následují výsledky hello ve formátu json.

```json
{
  "nextHopType": "Internet",
  "routeTableId": "System Route"
}
```

## <a name="next-steps"></a>Další kroky

Jakmile jste možnost toofind out hello další směrování pro virtuální počítač, můžete zobrazit hello zabezpečení síťových prostředků navštívíte [zobrazení zabezpečení – přehled](network-watcher-security-group-view-overview.md)














