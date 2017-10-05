---
title: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému | Microsoft Docs"
description: "Azure CLI skriptu ukázkové – vytvoření virtuálního počítače připojením se spravovaným diskem jako disk operačního systému"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 18eefee869b243b35d4426c226eff5f48cd490a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>Vytvoření virtuálního počítače existující spravované disk operačního systému pomocí rozhraní příkazového řádku

Tento skript vytvoří virtuální počítač připojením existující spravované disk jako disk s operačním systémem. Pomocí tohoto skriptu v předchozích scénáře:
* Vytvoření virtuálního počítače z existujícího spravovaného disku operačního systému, který jste zkopírovali ze spravovaných disků v jiném předplatném.
* Vytvoření virtuálního počítače z existujícího spravovaného disku, který byl vytvořen z specializované soubor virtuálního pevného disku 
* Vytvoření virtuálního počítače z existujícího spravované disku operačního systému, který byl vytvořen ze snímku 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "vytvoření virtuálního počítače ze spravovaných disků")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy získat vlastnosti spravovaných disků, připojte spravovaných disků na nový virtuální počítač a vytvoření virtuálního počítače. Každou položku v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Zobrazit az disku](https://docs.microsoft.com/cli/azure/disk#show) | Vlastnosti spravovaných disků na získá pomocí název disku a název skupiny prostředků. Vlastnost ID se používá k připojení se spravovaným diskem k vytvoření nového virtuálního počítače |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří virtuálního počítače pomocí spravovaného disk operačního systému |
## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
