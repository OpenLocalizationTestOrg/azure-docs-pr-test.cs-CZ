---
title: "Azure CLI skriptu ukázkové – vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: cc80e3fc5aaa12200e9a441db9d4a9c613c0548a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Zabezpečení síťového provozu mezi virtuálními počítači

Tento skript vytvoří dva virtuální počítače a zabezpečuje příchozí provoz do obou. Jeden virtuální počítač je přístupný v síti internet a pokud chcete povolit přenosy na portu 3389 a port 80 nakonfiguroval skupinu zabezpečení sítě (NSG). Druhý virtuální počítač není přístupné z Internetu a možnost Povolit jenom přenos z první virtuální počítač má nakonfigurované skupinu NSG. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "vytvoření virtuálního počítače s NSG")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků, virtuální počítač a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření az podsíti virtuální sítě](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | Vytvoří podsíť. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.  |
| [aktualizace pravidla nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | Aktualizuje pravidlo NSG. V této ukázce se aktualizuje pravidlo back-end předávat provoz jenom z podsítě front-endu. |
| [seznam pravidel nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | Vrací informace o pravidlo pro skupinu zabezpečení sítě. V této ukázce je název pravidla uložené v proměnné pro použití novější ve skriptu. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
