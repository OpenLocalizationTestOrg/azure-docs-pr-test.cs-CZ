---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvořit hostitele Docker | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvořit hostitele Docker"
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
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7df2561c428ff5d3ab0a0d53c8e046781996be77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-docker"></a>Vytvoření virtuálního počítače s Docker

Tento skript vytvoří virtuální počítač s Docker povolené a spustí kontejner Docker systémem NGINX. Po spuštění skriptu hello, můžete přístup hello NGINX webovému serveru přes hello plně kvalifikovaný název domény hello virtuální počítač Azure. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate hello nasazení. Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální sítě, podsítě a skupinu zabezpečení sítě. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [virtuální počítač az open-port](https://docs.microsoft.com/cli/azure/vm#open-port) | Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy. V této ukázce je otevřen port 80 pro přenosy protokolu HTTP. |
| [sada rozšíření virtuálního počítače Azure](https://docs.microsoft.com/cli/azure/vm/extension#set) | Přidá a běží virtuálním počítači rozšíření tooa virtuálních počítačů. V této ukázce je hello rozšíření virtuálního počítače Docker použité tooconfigure Docker hostitele.|
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
