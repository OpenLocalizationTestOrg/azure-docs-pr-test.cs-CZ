---
title: "zabezpečení sítě aaaAnalyze s Azure sítě sledovacích procesů zabezpečení skupiny zobrazení – 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Tento článek popisuje, jak tooanalyze toouse 1.0 rozhraní příkazového řádku Azure a virtuálních počítačů zabezpečení s zobrazení skupiny zabezpečení."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 96383a734b94d215d5b0f3d47339e46940d700b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a>Analýza zabezpečení vašeho virtuálního počítače s zobrazení skupiny zabezpečení pomocí Azure CLI 1.0

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Zobrazení skupiny zabezpečení vrátí pravidla zabezpečení sítě nakonfigurované a efektivní, které jsou použité tooa virtuálního počítače. Tato funkce je užitečná tooaudit a diagnostikovat skupin zabezpečení sítě, a pravidla, které jsou nakonfigurované na přenosem tooensure virtuálních počítačů se správně povolený nebo zakázaný. V tomto článku jsme ukážeme, jak tooretrieve hello nakonfigurované a efektivní zabezpečení pravidla tooa virtuálního počítače pomocí rozhraní příkazového řádku Azure

Tento článek používá 1.0 rozhraní příkazového řádku Azure a platformy, která je dostupná pro Windows, Mac a Linux. Sledovací proces sítě aktuálně používá pro podporu rozhraní příkazového řádku Azure CLI 1.0.

## <a name="before-you-begin"></a>Než začnete

Tento scénář předpokládá, že jste již provedli kroky hello v [vytvořit sledovací proces sítě](network-watcher-create.md) toocreate sledovací proces sítě.

## <a name="scenario"></a>Scénář

scénář Hello popsaná v tomto článku načte hello nakonfigurované a pravidla efektivní zabezpečení pro daný virtuální počítač.

## <a name="get-a-vm"></a>Získat virtuální počítač

Virtuální počítač je požadovaná toorun hello `vm list` rutiny. Hello následující příkaz vypíše hello virtuální machinese ve skupině prostředků:

```azurecli
azure vm list -g resourceGroupName
```

Jakmile znáte hello virtuálního počítače, můžete použít hello `vm show` rutiny tooget jeho Id prostředku:

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Načtení zobrazení skupiny zabezpečení

dalším krokem Hello je tooretrieve hello zabezpečení skupiny zobrazení výsledků. Přidání hello "--json" příznak naformátuje výsledky hello ve formátu json.

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-hello-results"></a>Zobrazení výsledků hello

Hello následující příklad je zkrácení odpověď vráceny výsledky hello. Hello výsledky zobrazit všechna pravidla zabezpečení efektivní a použitých hello na virtuálním počítači hello rozdělení v skupiny **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, a  **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Další kroky

Navštivte [auditování zabezpečení sítě skupiny (NSG) s sledovací proces sítě](network-watcher-nsg-auditing-powershell.md) toolearn jak tooautomate ověření skupin zabezpečení sítě.

Další informace o pravidlech hello zabezpečení, které jsou použité tooyour síťovým prostředkům, navštivte stránky [přehled zobrazení skupiny zabezpečení](network-watcher-security-group-view-overview.md)
