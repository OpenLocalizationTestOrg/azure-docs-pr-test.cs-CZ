---
title: "Ukázka skriptu aaaAzure rozhraní příkazového řádku - směrovat provoz prostřednictvím zařízení virtuální sítě | Microsoft Docs"
description: "Azure CLI ukázka skriptu - směrovat provoz prostřednictvím brány firewall sítě virtuálního zařízení."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: 981d6073be04a7ebaf96b657fbab8a378e7a995e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Směrovat provoz prostřednictvím sítě virtuálního zařízení

Tento ukázkový skript vytvoří virtuální síť s podsítí front-end a back-end. Vytvoří také virtuální počítač s IP Adresou předávání povoleno tooroute provozu mezi dvěma podsítěmi hello. Po spuštění skriptu hello můžete nasadit software pro sítě, jako například aplikací brány firewall, toohello virtuálních počítačů.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Ukázkový skript


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate hello skupinu prostředků, virtuální sítě a skupiny zabezpečení sítě. Každý příkaz v dokumentaci k toocommand specifické hello tabulky odkazů.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a front-end podsítě. |
| [Vytvoření podsítě az sítě](/cli/azure/network/vnet/subnet#create) | Vytvoří back-end a DMZ podsítě. |
| [Vytvoření veřejné sítě az-ip](/cli/azure/network/public-ip#create) | Vytvoří veřejnou hello tooaccess IP adresy virtuálních počítačů z hello Internetu. |
| [Vytvoření az síťových adaptérů sítě](/cli/azure/network/nic#create) | Vytvoří rozhraní virtuální sítě a předávání IP povolit pro ni. |
| [Vytvoření az sítě nsg](/cli/azure/network/nsg#create) | Vytvoří skupinu zabezpečení sítě (NSG). |
| [Vytvoření pravidla nsg az sítě](/cli/azure/network/nsg/rule#create) | Vytvoří pravidla NSG, které umožňují, že porty HTTP a HTTPS příchozí toohello virtuálních počítačů. |
| [aktualizace az sítě vnet podsíť](/cli/azure/network/vnet/subnet#update)| Partnerů hello skupiny Nsg a toosubnets tabulky trasy. |
| [Vytvoření sítě az trasy – tabulka](/cli/azure/network/route-table#create)| Vytvoří směrovací tabulku pro všechny trasy. |
| [Vytvoření az síťovou směrovací tabulku trasu](/cli/azure/network/route-table/route#create)| Vytvoří trasy tooroute provoz mezi podsítěmi a hello Internet prostřednictvím hello virtuálních počítačů. |
| [Vytvoření virtuálního počítače az](/cli/azure/vm#create) | Vytvoří virtuální počítač a připojí tooit hello síťový adaptér. Tento příkaz také určuje toouse bitové kopie virtuálního počítače hello a pověření pro správu. |
| [Odstranění skupiny az](/cli/azure/group#delete) | Odstraní skupinu prostředků a všechny prostředky, které obsahuje. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](/cli/azure/overview).

Další síťové rozhraní příkazového řádku skriptu ukázky lze nalézt v hello [dokumentace přehled sítě Azure](../cli-samples.md)
