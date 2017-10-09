---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Linuxem | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – vytvoření virtuálního počítače s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c9855204a85cc0f6cd758a1d20121fbeea070943
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a>Vytvoření kompletně nakonfigurovaný virtuálního počítače

Tento skript vytvoří virtuální počítač Azure s Ubuntu operačního systému. Po spuštění skriptu hello, můžete přistupovat přes protokol SSH hello virtuálního počítače.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření sítě vnet az](https://docs.microsoft.com/cli/azure/network/vnet#create) | Vytvoří virtuální síť Azure a podsíť. |
| [Vytvoření veřejné sítě az-ip](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Vytvoří veřejnou IP adresu se statickou IP adresu a přidružené název DNS. |
| [Vytvoření az sítě nsg](https://docs.microsoft.com/cli/azure/network/nsg#create) | Vytvoří skupinu zabezpečení sítě (NSG), což je hranice zabezpečení mezi hello Internetu a hello virtuálního počítače. |
| [Vytvoření pravidla nsg az sítě](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Vytvoří tooallow pravidla NSG příchozí přenosy. V této ukázce je otevřen port 22 pro SSH provoz. |
| [Vytvoření az síťových adaptérů sítě](https://docs.microsoft.com/cli/azure/network/nic#create) | Vytvoří virtuální síťové karty a připojí jej toohello virtuální sítě, podsítě a NSG. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
