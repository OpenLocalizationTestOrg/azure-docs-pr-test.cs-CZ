---
title: "aaaRedeploy virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Jak problémy tooredeploy Linuxové virtuální počítače v Azure toomitigate připojení SSH."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 9adfd1b11f262d362133366b2bba5e69c70c9b82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-linux-virtual-machine-toonew-azure-node"></a>Znovu nasaďte toonew Linux virtuálního počítače Azure uzlu
Pokud čelí potíží při řešení potíží s SSH nebo aplikace přistupovat ke tooa Linuxový virtuální počítač (VM) v Azure, mohou pomoci opětovného nasazení hello virtuálních počítačů. Při opětovném nasazování virtuálního počítače, přesune nový uzel tooa hello virtuálních počítačů v rámci hello infrastrukturu Azure a pak ho znovu zapne. Možnosti konfigurace a přidružené prostředky zůstanou zachovány. Tento článek ukazuje, jak tooredeploy a virtuálního počítače pomocí rozhraní příkazového řádku Azure nebo hello portálu Azure.

> [!NOTE]
> Po opětovném virtuálního počítače, dojde ke ztrátě dočasným diskovým hello a dynamické IP adresy přidružené k rozhraní virtuální sítě se aktualizují. 

Můžete znovu nasadit virtuální počítač pomocí jedné z hello následující možnosti. Je třeba pouze jednu možnost tooredeploy toochoose virtuálního počítače:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure Portal](#using-azure-portal)

## <a name="use-hello-azure-cli-20"></a>Hello použití Azure CLI 2.0
Instalace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Opětovné nasazení virtuálního počítače s [az virtuálního počítače znovu ho zaveďte](/cli/azure/vm#redeploy). Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-hello-azure-cli-10"></a>Hello použití Azure CLI 1.0
Nainstalujte hello [nejnovější Azure CLI 1.0](../../cli-install-nodejs.md), přihlaste se tooan účet Azure a ujistěte se, že jste v režimu Resource Manager (`azure config mode arm`).

Následující příklad opětovně nasadí Hello hello virtuálního počítače s názvem *Můjvp* v hello skupinu prostředků s názvem *myResourceGroup*:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením tooyour virtuálních počítačů, najdete konkrétní pomoc na [řešení potíží s připojení SSH](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [podrobné SSH při řešení potíží](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Pokud nelze získat přístup k aplikaci běžící na vašem virtuálním počítači, můžete si také přečíst [řešení potíží s aplikací](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

