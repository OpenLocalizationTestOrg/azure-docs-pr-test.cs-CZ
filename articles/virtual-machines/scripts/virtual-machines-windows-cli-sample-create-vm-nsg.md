---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořte dva virtuální počítače s vnitřní a vnější skupina NSG | Microsoft Docs"
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
ms.openlocfilehash: f04e62d09575bee34ac4aeee949736b34000c337
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Zabezpečení síťového provozu mezi virtuálními počítači

Tento skript vytvoří dva virtuální počítače a zabezpečuje tooboth příchozí přenosy. Jednou z virtuálního počítače je dostupný na Internetu hello a skupinu zabezpečení sítě (NSG) nakonfiguroval tooallow přenosy na portu 3389 a port 80. Hello druhý virtuální počítač není přístupný na hello Internetu, a má tooonly NSG nakonfigurované povolit provoz z hello prvním virtuálním počítači. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření az podsíti virtuální sítě](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | Vytvoří podsíť. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [aktualizace pravidla nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | Aktualizuje pravidlo NSG. V této ukázce je pravidlo back-end hello aktualizované toopass prostřednictvím jenom z podsítě front-endu hello. |
| [seznam pravidel nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | Vrací informace o pravidlo pro skupinu zabezpečení sítě. V této ukázce je název pravidla hello uložené v proměnné pro použití novější ve skriptu hello. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
