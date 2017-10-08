---
title: "aaaSet si Azure Key Vault pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Jak tooset až Key Vault pro použití s se virtuální počítač Azure Resource Manager s hello 2.0 rozhraní příkazového řádku."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: a5dc1fbe59a71b4456ba5b9bbacdb90440064757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-key-vault-for-virtual-machines-with-hello-azure-cli-20"></a>Jak tooset až Key Vault pro virtuální počítače s hello 2.0 rozhraní příkazového řádku Azure

V zásobníku hello Azure Resource Manager tajných klíčů nebo certifikáty jsou modelovat jako prostředky, které jsou poskytovány Key Vault. toolearn Další informace o Azure Key Vault najdete v části [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md) Aby mohl používat s virtuálními počítači Azure Resource Manager toobe Key Vault, hello *EnabledForDeployment* musí být nastavena vlastnost v Key Vault tootrue. Tento článek ukazuje, jak hello tooset až Key Vault pro použití s Azure virtuální počítače (VM) pomocí Azure CLI 2.0. Můžete také provést tyto kroky hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tyto kroky tooperform, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

## <a name="create-a-key-vault"></a>Vytvoření trezoru klíčů
Vytvoření trezoru klíčů a přiřaďte hello nasazení zásad s [vytvořit az keyvault](/cli/azure/keyvault#create). Hello následující příklad vytvoří trezoru klíčů s názvem `myKeyVault` v hello `myResourceGroup` skupiny prostředků:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Aktualizace Key Vault pro použití s virtuálními počítači
Nastavit zásady nasazení hello na existující trezor klíčů s [az keyvault aktualizace](/cli/azure/keyvault#update). Hello následující aktualizace hello trezor klíčů s názvem `myKeyVault` v hello `myResourceGroup` skupiny prostředků:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-tooset-up-key-vault"></a>Použití šablony tooset až Key Vault
Pokud použijete šablonu, budete potřebovat tooset hello `enabledForDeployment` vlastnost příliš`true` prostředku Key Vault hello následujícím způsobem:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Další kroky
Další možnosti, které můžete konfigurovat při vytvoření trezoru klíč pomocí šablon, najdete v části [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
