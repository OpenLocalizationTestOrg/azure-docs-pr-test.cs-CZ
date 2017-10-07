---
title: "Ověřte aaaVerify provoz s tokem IP sledovací proces sítě Azure - REST | Microsoft Docs"
description: "Tento článek popisuje, jak toocheck Pokud tooor provoz z virtuálního počítače povolený nebo zakázaný"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 956db0d326db597c6c402a9e8d4a5522c47c02d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Zkontrolujte, jestli je povolené nebo zakázané s tokem IP přenosy ověřte součást sledovací proces sítě Azure

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Rozhraní API Azure REST](network-watcher-check-ip-flow-verify-rest.md)


Tok IP ověřte, že je funkce sledovací proces sítě, která umožňuje tooverify Pokud provoz je povolený tooor z virtuálního počítače. ověření Hello lze spustit pro příchozí nebo odchozí provoz. Tento scénář je užitečné tooget aktuálním stavu o tom, jestli virtuální počítač může kontaktovat tooan externí prostředek nebo back-end. Tok IP ověření může být použité tooverify, pokud vaše skupina zabezpečení sítě (NSG) pravidla jsou správně nakonfigurovány a řešení potíží s toky, kteří jsou Blokovaní pravidla NSG. Dalším důvodem pro použití IP tok ověření je tooensure přenosy, které chcete blokovat je správně blokován nastavením hello NSG.

## <a name="before-you-begin"></a>Než začnete

ARMclient je použité toocall hello REST API pomocí prostředí PowerShell. ARMClient se nachází na chocolatey v [ARMClient na Chocolatey](https://chocolatey.org/packages/ARMClient)

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

## <a name="scenario"></a>Scénář

Tento scénář používá tooverify ověřte toku IP, pokud virtuální počítač může kontaktovat počítač tooanother přes port 443. Pokud je odepřená hello provoz, vrátí hello pravidlo zabezpečení, které je odepřen, aby provoz. toolearn Další informace o toku IP ověřit, navštivte [IP tok ověření – přehled](network-watcher-ip-flow-verify-overview.md)

V tomto scénáři můžete:

* Načíst virtuální počítač
* Ověření volání IP toku
* Zkontrolujte výsledky

## <a name="log-in-with-armclient"></a>Přihlaste se pomocí ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Načíst virtuální počítač

Spusťte následující skript tooreturn hello virtuálního počítače. Hello následující kód potřebuje hodnoty pro proměnné hello:

* **ID předplatného** -hello toouse Id předplatného.
* **Název skupiny prostředků** – hello název skupiny prostředků, která obsahuje virtuální počítače.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

Hello informace, které je potřeba je hello id v rámci hello typu `Microsoft.Compute/virtualMachines`. výsledky Hello by měl vypadat podobně jako toohello následující ukázka kódu:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a>Volání IP toku ověřte

Hello následující příklad vytvoří přenosem hello tooverify požadavku pro zadaný virtuální počítač. Hello odpověď se vrátí, pokud hello provoz je povolený, nebo pokud hello provoz byl odepřen. Pokud je provoz odepřela také vrátí hodnotu jaké bloky pravidel hello provoz.

> [!NOTE]
> Tok IP ověření vyžaduje, aby hello prostředků virtuálního počítače je přidělená.

skript Hello vyžaduje Id virtuálního počítače a síťový adaptér na virtuálním počítači hello hello prostředku. Tyto hodnoty jsou poskytovány hello předcházející výstup.

> [!Important]
> Pro všechny REST sledovací proces sítě hello volání název skupiny prostředků v žádosti o hello, že je identifikátor URI hello ten, který obsahuje instanci hello sledovací proces sítě, není hello prostředků, kterou provádíte hello diagnostiky akce na.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-hello-results"></a>Seznámení s výsledky hello

Hello odpověď, kterou jste se vrátit poznáte, jestli je povolené nebo zakázané hello přenosy. odpověď Hello vypadá jako jeden z hello následující příklady:

**Povoleno**

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

**Odepřen**

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a>Další kroky

Pokud je blokován provoz a neměl by být, najdete v části [spravovat skupiny zabezpečení sítě](../virtual-network/virtual-network-manage-nsg-arm-portal.md) toolearn Další informace o skupinách zabezpečení sítě.












