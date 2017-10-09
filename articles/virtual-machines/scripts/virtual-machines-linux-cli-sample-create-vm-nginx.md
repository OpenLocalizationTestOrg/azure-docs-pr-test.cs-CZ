---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - vytvoření virtuálního počítače s Linuxem pomocí NGINX | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure Script ukázka – vytvoření virtuálního počítače s Linuxem pomocí NGINX"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9166ccfd4f2e6eea731a8dc6956575d9f8f85488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-nginx"></a>Vytvoření virtuálního počítače s NGINX

Tento skript vytvoří virtuální počítač Azure a používá hello rozšíření vlastních skriptů Azure virtuálního počítače tooinstall NGINX. Po spuštění skriptu hello, můžete přejít na web ukázku hello veřejnou IP adresu hello virtuálního počítače.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a>Rozšíření vlastních skriptů

rozšíření vlastních skriptů Hello zkopíruje tento skript na hello virtuálního počítače. skript Hello je tooinstall spusťte a nakonfigurujte webového serveru se službou NGINX. 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

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
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří hello virtuálního počítače. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [virtuální počítač az open-port](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Vytvoří tooallow pravidla skupiny zabezpečení sítě příchozí přenosy. V této ukázce je otevřen port 80 pro přenosy protokolu HTTP. |
| [sada rozšíření virtuálního počítače Azure](https://docs.microsoft.com/cli/azure/vm/extension#set) | Přidá a běží virtuálním počítači rozšíření tooa virtuálních počítačů. V této ukázce je rozšíření vlastních skriptů hello použité tooinstall NGINX.|
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
