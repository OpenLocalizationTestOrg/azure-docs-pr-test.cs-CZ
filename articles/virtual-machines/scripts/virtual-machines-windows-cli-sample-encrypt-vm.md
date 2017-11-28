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
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="75302-103">Šifrování virtuálního počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="75302-103">Encrypt a Windows virtual machine in Azure</span></span>

<span data-ttu-id="75302-104">Tento skript vytvoří zabezpečené Azure Key Vault, šifrovacích klíčů, objektu služby Azure Active Directory a systému Windows virtuálního počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="75302-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="75302-105">Virtuální počítač se pak šifrují pomocí šifrovacího klíče z Key Vault a hlavní pověření služby.</span><span class="sxs-lookup"><span data-stu-id="75302-105">The VM is then encrypted using the encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="75302-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="75302-106">Sample script</span></span>

<span data-ttu-id="75302-107">[!code-azurecli-interactive[hlavní](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "disky šifrování virtuálních počítačů")]</span><span class="sxs-lookup"><span data-stu-id="75302-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="75302-108">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="75302-108">Clean up deployment</span></span> 

<span data-ttu-id="75302-109">Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="75302-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="75302-110">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="75302-110">Script explanation</span></span>

<span data-ttu-id="75302-111">Tento skript používá následující příkazy k vytvoření skupiny prostředků Azure Key Vault, instančního objektu, virtuální počítač a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="75302-111">This script uses the following commands to create a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="75302-112">Každý příkaz v tabulce odkazy na dokumentaci konkrétní příkaz.</span><span class="sxs-lookup"><span data-stu-id="75302-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="75302-113">Příkaz</span><span class="sxs-lookup"><span data-stu-id="75302-113">Command</span></span> | <span data-ttu-id="75302-114">Poznámky</span><span class="sxs-lookup"><span data-stu-id="75302-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="75302-115">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="75302-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="75302-116">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="75302-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="75302-117">Vytvoření az keyvault</span><span class="sxs-lookup"><span data-stu-id="75302-117">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="75302-118">Vytvoří Azure Key Vault k uložení zabezpečení dat, jako jsou šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="75302-118">Creates an Azure Key Vault to store secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="75302-119">Vytvořte klíč keyvault az</span><span class="sxs-lookup"><span data-stu-id="75302-119">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="75302-120">Vytvoří šifrovací klíč v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="75302-120">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="75302-121">AZ ad sp vytvořit pro rbac</span><span class="sxs-lookup"><span data-stu-id="75302-121">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="75302-122">Vytvoří Azure Active Directory instanční objekt bezpečně ověřování a řízení přístupu k šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="75302-122">Creates an Azure Active Directory service principal to securely authenticate and control access to encryption keys.</span></span> |
| [<span data-ttu-id="75302-123">AZ keyvault set zásad</span><span class="sxs-lookup"><span data-stu-id="75302-123">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="75302-124">Nastaví oprávnění pro Key Vault udělit přístup k hlavní službě pro šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="75302-124">Sets permissions on the Key Vault to grant the service principal access to encryption keys.</span></span> |
| [<span data-ttu-id="75302-125">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="75302-125">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="75302-126">Vytvoří virtuální počítač a připojí jej k síťové karty, virtuální sítě, podsítě a NSG.</span><span class="sxs-lookup"><span data-stu-id="75302-126">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="75302-127">Tento příkaz také Určuje bitovou kopii virtuálního počítače, který se má použít a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="75302-127">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="75302-128">Povolit šifrování az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="75302-128">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="75302-129">Povoluje šifrování na virtuálním počítači pomocí pověření hlavní služby a šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="75302-129">Enables encryption on a VM using the service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="75302-130">Zobrazit šifrování az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="75302-130">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="75302-131">Zobrazuje stav proces šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="75302-131">Shows the status of the VM encryption process.</span></span> |
| [<span data-ttu-id="75302-132">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="75302-132">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="75302-133">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="75302-133">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="75302-134">Další kroky</span><span class="sxs-lookup"><span data-stu-id="75302-134">Next steps</span></span>

<span data-ttu-id="75302-135">Další informace o rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="75302-135">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="75302-136">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="75302-136">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span></span>
