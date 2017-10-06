---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - šifrování virtuálního počítače s Linuxem | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – zašifrovat virtuální počítač s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 1e455da4a8ea6d75b6d0d74b338d2e4d84973413
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Zašifrovat virtuální počítač s Linuxem v Azure

Tento skript vytvoří zabezpečené Azure Key Vault, šifrovacích klíčů, objektu služby Azure Active Directory a Linux virtuálního počítače (VM). Hello virtuálních počítačů se potom šifruje pomocí hello šifrovacího klíče z Key Vault a hlavní pověření služby.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy toocreate hello prostředků skupiny, Azure Key Vault, služba hlavní, virtuální počítač, a všechny související prostředky. Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az keyvault](https://docs.microsoft.com/cli/azure/keyvault#create) | Vytvoří Azure Key Vault toostore zabezpečené data, jako jsou šifrovací klíče. |
| [Vytvořte klíč keyvault az](https://docs.microsoft.com/cli/azure/keyvault/key#create) | Vytvoří šifrovací klíč v Key Vault. |
| [AZ ad sp vytvořit pro rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | Vytvoří služby Azure Active Directory hlavní toosecurely ověřování a řízení přístupu tooencryption klíče. |
| [AZ keyvault set zásad](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Nastaví oprávnění hello Key Vault toogrant hello služby hlavní přístup tooencryption klíče. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG. Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.  |
| [Povolit šifrování az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | Povoluje šifrování na virtuálním počítači pomocí pověření hlavní služby hello a šifrovací klíč. |
| [Zobrazit šifrování az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/encryption#show) | Zobrazuje stav hello hello proces šifrování virtuálních počítačů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuální počítač Azure s Linuxem dokumentaci](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
