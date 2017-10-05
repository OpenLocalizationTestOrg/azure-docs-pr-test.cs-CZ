---
title: "Nastavení Azure Key Vault pro virtuální počítače s Linuxem | Microsoft Docs"
description: "Jak nastavit Key Vault pro použití s se virtuální počítač Azure Resource Manager pomocí rozhraní příkazového řádku 2.0."
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
ms.openlocfilehash: 2cc9b4c978e9a4deb0c8443c4b0f9e301a7cf492
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-set-up-key-vault-for-virtual-machines-with-the-azure-cli-20"></a><span data-ttu-id="5c7e1-103">Jak nastavit Key Vault pro virtuální počítače s 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5c7e1-103">How to set up Key Vault for virtual machines with the Azure CLI 2.0</span></span>

<span data-ttu-id="5c7e1-104">V zásobníku Azure Resource Manager tajných klíčů nebo certifikáty jsou modelovat jako prostředky, které jsou poskytovány Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5c7e1-104">In the Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by Key Vault.</span></span> <span data-ttu-id="5c7e1-105">Další informace o Azure Key Vault najdete v tématu [co je Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="5c7e1-105">To learn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="5c7e1-106">V pořadí pro Key Vault, který se má použít s virtuálními počítači Azure Resource Manager *EnabledForDeployment* vlastnost v Key Vault musí být nastavena na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="5c7e1-106">In order for Key Vault to be used with Azure Resource Manager VMs, the *EnabledForDeployment* property on Key Vault must be set to true.</span></span> <span data-ttu-id="5c7e1-107">Tento článek ukazuje, jak nastavit Key Vault pro použití s Azure virtuální počítače (VM) pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="5c7e1-107">This article shows you how to set up Key Vault for use with Azure virtual machines (VMs) using the Azure CLI 2.0.</span></span> <span data-ttu-id="5c7e1-108">K provedení těchto kroků můžete také využít [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c7e1-108">You can also perform these steps with the [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="5c7e1-109">K provedení těchto kroků, budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5c7e1-109">To perform these steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="5c7e1-110">Vytvoření trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="5c7e1-110">Create a Key Vault</span></span>
<span data-ttu-id="5c7e1-111">Vytvoření trezoru klíčů a přiřaďte nasazení zásad s [vytvořit az keyvault](/cli/azure/keyvault#create).</span><span class="sxs-lookup"><span data-stu-id="5c7e1-111">Create a key vault and assign the deployment policy with [az keyvault create](/cli/azure/keyvault#create).</span></span> <span data-ttu-id="5c7e1-112">Následující příklad vytvoří trezoru klíčů s názvem `myKeyVault` v `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="5c7e1-112">The following example creates a key vault named `myKeyVault` in the `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a><span data-ttu-id="5c7e1-113">Aktualizace Key Vault pro použití s virtuálními počítači</span><span class="sxs-lookup"><span data-stu-id="5c7e1-113">Update a Key Vault for use with VMs</span></span>
<span data-ttu-id="5c7e1-114">Sada zásady nasazení na existující klíč trezoru s [az keyvault aktualizace](/cli/azure/keyvault#update).</span><span class="sxs-lookup"><span data-stu-id="5c7e1-114">Set the deployment policy on an existing key vault with [az keyvault update](/cli/azure/keyvault#update).</span></span> <span data-ttu-id="5c7e1-115">Následující aktualizace trezoru klíčů s názvem `myKeyVault` v `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="5c7e1-115">The following updates the key vault named `myKeyVault` in the `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="5c7e1-116">Použití šablon nastavit Key Vault</span><span class="sxs-lookup"><span data-stu-id="5c7e1-116">Use templates to set up Key Vault</span></span>
<span data-ttu-id="5c7e1-117">Pokud použijete šablonu, je nutné nastavit `enabledForDeployment` vlastnost `true` pro klíč trezoru prostředků následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="5c7e1-117">When you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource as follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="5c7e1-118">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5c7e1-118">Next steps</span></span>
<span data-ttu-id="5c7e1-119">Další možnosti, které můžete konfigurovat při vytvoření trezoru klíč pomocí šablon, najdete v části [vytvoření trezoru klíčů](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="5c7e1-119">For other options that you can configure when you create a Key Vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
