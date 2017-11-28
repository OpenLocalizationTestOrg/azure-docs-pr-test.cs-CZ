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
# <a name="how-tooset-up-key-vault-for-virtual-machines-with-hello-azure-cli-20"></a><span data-ttu-id="7846b-103">Jak tooset až Key Vault pro virtuální počítače s hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7846b-103">How tooset up Key Vault for virtual machines with hello Azure CLI 2.0</span></span>

<span data-ttu-id="7846b-104">V zásobníku hello Azure Resource Manager tajných klíčů nebo certifikáty jsou modelovat jako prostředky, které jsou poskytovány Key Vault.</span><span class="sxs-lookup"><span data-stu-id="7846b-104">In hello Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by Key Vault.</span></span> <span data-ttu-id="7846b-105">toolearn Další informace o Azure Key Vault najdete v části [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="7846b-105">toolearn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="7846b-106">Aby mohl používat s virtuálními počítači Azure Resource Manager toobe Key Vault, hello *EnabledForDeployment* musí být nastavena vlastnost v Key Vault tootrue.</span><span class="sxs-lookup"><span data-stu-id="7846b-106">In order for Key Vault toobe used with Azure Resource Manager VMs, hello *EnabledForDeployment* property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="7846b-107">Tento článek ukazuje, jak hello tooset až Key Vault pro použití s Azure virtuální počítače (VM) pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="7846b-107">This article shows you how tooset up Key Vault for use with Azure virtual machines (VMs) using hello Azure CLI 2.0.</span></span> <span data-ttu-id="7846b-108">Můžete také provést tyto kroky hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7846b-108">You can also perform these steps with hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="7846b-109">Tyto kroky tooperform, budete potřebovat hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="7846b-109">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="7846b-110">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="7846b-110">Create a Key Vault</span></span>
<span data-ttu-id="7846b-111">Vytvoření trezoru klíčů a přiřaďte hello nasazení zásad s [vytvořit az keyvault](/cli/azure/keyvault#create).</span><span class="sxs-lookup"><span data-stu-id="7846b-111">Create a key vault and assign hello deployment policy with [az keyvault create](/cli/azure/keyvault#create).</span></span> <span data-ttu-id="7846b-112">Hello následující příklad vytvoří trezoru klíčů s názvem `myKeyVault` v hello `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="7846b-112">hello following example creates a key vault named `myKeyVault` in hello `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a><span data-ttu-id="7846b-113">Aktualizace Key Vault pro použití s virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="7846b-113">Update a Key Vault for use with VMs</span></span>
<span data-ttu-id="7846b-114">Nastavit zásady nasazení hello na existující trezor klíčů s [az keyvault aktualizace](/cli/azure/keyvault#update).</span><span class="sxs-lookup"><span data-stu-id="7846b-114">Set hello deployment policy on an existing key vault with [az keyvault update](/cli/azure/keyvault#update).</span></span> <span data-ttu-id="7846b-115">Hello následující aktualizace hello trezor klíčů s názvem `myKeyVault` v hello `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="7846b-115">hello following updates hello key vault named `myKeyVault` in hello `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="7846b-116">Použití šablony tooset až Key Vault</span><span class="sxs-lookup"><span data-stu-id="7846b-116">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="7846b-117">Pokud použijete šablonu, budete potřebovat tooset hello `enabledForDeployment` vlastnost příliš`true` prostředku Key Vault hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="7846b-117">When you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource as follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="7846b-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7846b-118">Next steps</span></span>
<span data-ttu-id="7846b-119">Další možnosti, které můžete konfigurovat při vytvoření trezoru klíč pomocí šablon, najdete v části [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="7846b-119">For other options that you can configure when you create a Key Vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
