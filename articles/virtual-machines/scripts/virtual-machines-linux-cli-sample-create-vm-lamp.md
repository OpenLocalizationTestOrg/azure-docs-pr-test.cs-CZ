---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - nasazení hello svítilna zásobníku v architektuře s vyrovnáváním zatížení virtuální Machin Škálovací sadě | Microsoft Docs"
description: "Použít vlastní skript rozšíření toodeploy hello svítilna zásobníku v zatížení = sad v Azure škálování vyrovnáváním virtuálního počítače."
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
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: d5278db809faaa0997a08b00a53387d754fce3d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Nasazení hello svítilna zásobníku v sadě škálování Vyrovnávání zatížení sítě virtuálních počítačů

Tento příklad vytvoří škálovací sadu virtuálních počítačů a platí rozšíření, které běží vlastní skript toodeploy hello svítilna zásobníku na každém virtuálním počítači v sadě škálování hello.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Připojení

Použijte tento kód toosee, jak nastavit tooconnect tooyour virtuální počítače a vaše škálování.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access hello virtual machine scale set")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny, sadě škálování hello a virtuální počítače a všechny související prostředky hello.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá hello následující příkazy toocreate skupinu prostředků, virtuální počítač, skupinu dostupnosti, nástroj pro vyrovnávání zatížení a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az vmss](https://docs.microsoft.com/cli/azure/vmss#create) | Vytvoří škálovací sadu virtuálních počítačů |
| [Vytvořit pravidlo lb az sítě](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Přidat koncový bod Vyrovnávání zatížení sítě |
| [sada rozšíření vmss az](https://docs.microsoft.com/cli/azure/vmss/extension#set) | Vytváření rozšíření hello, který spouští hello vlastních skriptů při nasazení virtuálního počítače |
| [AZ vmss aktualizace instance](https://docs.microsoft.com/cli/azure/vmss#update-instances) | Spusťte skript vlastní hello hello instancí virtuálních počítačů, které byly nasazené, než bylo použito rozšíření hello toohello škálovací sadu. |
| [AZ vmss škálování](https://docs.microsoft.com/cli/azure/vmss#scale) | Škálování hello škálování nastavený přidáním více instancí virtuálního počítače. vlastní skript Hello běží v nich při jejich nasazení. |
| [seznam veřejné ip az sítě](https://docs.microsoft.com/cli/azure/network/public-ip#list) | Získáte IP adresy hello hello virtuální počítače vytvořené ukázka hello. |
| [Zobrazit lb az sítě](https://docs.microsoft.com/cli/azure/network/lb#show) | Porty používané pro vyrovnávání zatížení hello získáte hello front-end a back-end. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
