---
title: "Rozhraní příkazového řádku Azure ukázkový skript – zašifrovat virtuální počítač s Windows | Microsoft Docs"
description: "Rozhraní příkazového řádku Azure ukázkový skript – zašifrovat Windows virtuálního počítače"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 9574d779982c939aa970cd4bdf7df6e66793e3b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Šifrování virtuálního počítače s Windows v Azure

Tento skript vytvoří zabezpečené Azure Key Vault, šifrovacích klíčů, objektu služby Azure Active Directory a systému Windows virtuálního počítače (VM). Virtuální počítač se pak šifrují pomocí šifrovacího klíče z Key Vault a hlavní pověření služby.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "disky šifrování virtuálních počítačů")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení 

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy k vytvoření skupiny prostředků Azure Key Vault, instančního objektu, virtuální počítač a všechny související prostředky. Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.

| Příkaz | Poznámky |
|---|---|
| [Vytvoření skupiny az](https://docs.microsoft.com/cli/azure/group#create) | Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky. |
| [Vytvoření az keyvault](https://docs.microsoft.com/cli/azure/keyvault#create) | Vytvoří Azure Key Vault k uložení zabezpečení dat, jako jsou šifrovací klíče. |
| [Vytvořte klíč keyvault az](https://docs.microsoft.com/cli/azure/keyvault/key#create) | Vytvoří šifrovací klíč v Key Vault. |
| [AZ ad sp vytvořit pro rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | Vytvoří Azure Active Directory instanční objekt bezpečně ověřování a řízení přístupu k šifrovací klíče. |
| [AZ keyvault set zásad](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Nastaví oprávnění pro Key Vault udělit přístup k hlavní službě pro šifrovací klíče. |
| [Vytvoření virtuálního počítače az](https://docs.microsoft.com/cli/azure/vm#create) | Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG. Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.  |
| [Povolit šifrování az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | Povoluje šifrování na virtuálním počítači pomocí pověření hlavní služby a šifrovací klíč. |
| [Zobrazit šifrování az virtuálních počítačů](https://docs.microsoft.com/cli/azure/vm/encryption#show) | Zobrazuje stav proces šifrování virtuálních počítačů. |
| [Odstranění skupiny az](https://docs.microsoft.com/cli/azure/vm/extension#set) | Odstraní skupinu prostředků, včetně všech vnořených prostředků. |

## <a name="next-steps"></a>Další kroky

Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).

Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
