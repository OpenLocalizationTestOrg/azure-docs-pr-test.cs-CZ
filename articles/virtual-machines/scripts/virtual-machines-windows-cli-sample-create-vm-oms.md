---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 s monitorováním OMS | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače Windows serveru 2016 s monitorováním OMS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: ddb191163061dc47712e024c8c1d7a6f4bdf325b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a>Monitorování virtuálních počítačů pomocí služby Operations Management Suite

Tento skript vytvoří virtuální počítač Azure, nainstaluje agenta nástroje Operations Management Suite (OMS) a zaregistruje systém s pracovní prostor služby OMS. Po spuštění skriptu se nebude zobrazovat v konzole OMS virtuálního počítače.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "rychlé vytvoření virtuálního počítače")]

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
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.  |
| [sada rozšíření virtuálního počítače Azure](https://docs.microsoft.com/cli/azure/vm/extension#set) | Virtuální počítač spouští rozšíření virtuálního počítače. V takovém případě rozšíření agenta Operations Management Suite se používá pro instalaci agenta OMS a registraci virtuálních počítačů v pracovním prostoru OMS. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
