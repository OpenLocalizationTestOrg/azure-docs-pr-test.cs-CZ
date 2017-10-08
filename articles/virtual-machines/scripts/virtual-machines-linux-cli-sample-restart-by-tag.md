---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - restartovat virtuální počítače | Microsoft Docs"
description: "Ukázka skriptu Azure CLI - restartování virtuálních počítačů, značky a ID"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: a1ae07bd1d2be906553bef817385a4a333a10eca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restart-vms"></a>Restartujte virtuální počítače

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Tato ukázka zobrazuje několika způsoby tooget některé virtuální počítače a je restartovat.

Hello nejprve restartuje všechny hello virtuální počítače ve skupině prostředků hello.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

Hello druhý hello získá označené virtuálních počítačů pomocí `az resouce list` a filtry toohello prostředky, které jsou virtuální počítače a znovu tyto virtuální počítače.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Tato ukázka funguje v prostředí Bash. Možnosti na spouštění skriptů rozhraní příkazového řádku Azure v klientovi Windows najdete v tématu [běžící ve Windows hello rozhraní příkazového řádku Azure](../windows/cli-options.md).


## <a name="sample-script"></a>Ukázkový skript

Ukázka Hello má tři skripty.
Hello první z nich zřizuje hello virtuálních počítačů.
Možnost Ne čekání hello používá tak hello příkaz vrátí bez čekání na každý počítač toobe zřízený.
Hello druhý čeká toobe virtuální počítače hello plně zřízený.
třetí skriptu Hello restartuje všechny hello virtuální počítače, které byly zřízeny, a pak jenom hello označené virtuálních počítačů.

### <a name="provision-hello-vms"></a>Hello zřizování virtuálních počítačů

Tento skript vytvoří skupinu prostředků a vytvoří tři toorestart virtuálních počítačů.
Dvě z nich jsou označené.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision hello VMs")]

### <a name="wait"></a>Wait

Tento skript kontroluje na stav zřizování každých 20 sekund, dokud se zřizují všechny tři virtuální počítače, nebo jeden z nich selže tooprovision hello.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for hello VMs toobe provisioned")]

### <a name="restart-hello-vms"></a>Restartujte virtuální počítače hello

Tento skript restartuje všechny virtuální počítače hello ve skupině prostředků hello, a poté znovu spustí jenom virtuální počítače s příznakem hello.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Po spuštění ukázka skriptu hello hello následující příkaz může být skupiny prostředků použít tooremove hello, virtuální počítače a všechny související prostředky.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Vytvoří hello virtuální počítače.  |
| [Seznam virtuálních počítačů az](https://docs.microsoft.com/cli/azure/vm#list) | Použít s `--query` tooensure hello virtuálních počítačů, které jsou zřízené před restartováním je a pak tooget hello ID toorestart hello virtuálních počítačů je. |
| [Seznam zdrojů az](https://docs.microsoft.com/cli/azure/vm#list) | Použít s `--query` tooget hello ID hello virtuálních počítačů pomocí hello značky. |
| [AZ restartování virtuálního počítače](https://docs.microsoft.com/cli/azure/vm#list) | Restartuje hello virtuálních počítačů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
