---
title: "aaaAzure ukázka skriptu rozhraní příkazového řádku - zašifrovat virtuální počítač s Windows | Microsoft Docs"
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
ms.openlocfilehash: 7a9928467f818cd5358d3d19bff5129d8fa21313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="47b8c-103">Šifrování virtuálního počítače s Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="47b8c-103">Encrypt a Windows virtual machine in Azure</span></span>

<span data-ttu-id="47b8c-104">Tento skript vytvoří zabezpečené Azure Key Vault, šifrovacích klíčů, objektu služby Azure Active Directory a systému Windows virtuálního počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="47b8c-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="47b8c-105">Hello virtuálních počítačů se potom šifruje pomocí hello šifrovacího klíče z Key Vault a hlavní pověření služby.</span><span class="sxs-lookup"><span data-stu-id="47b8c-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="47b8c-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="47b8c-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="47b8c-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="47b8c-107">Clean up deployment</span></span> 

<span data-ttu-id="47b8c-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="47b8c-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="47b8c-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="47b8c-109">Script explanation</span></span>

<span data-ttu-id="47b8c-110">Tento skript používá následující příkazy toocreate hello prostředků skupiny, Azure Key Vault, služba hlavní, virtuální počítač, a všechny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="47b8c-110">This script uses hello following commands toocreate a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="47b8c-111">Každý příkaz v hello tabulky odkazů toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="47b8c-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="47b8c-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="47b8c-112">Command</span></span> | <span data-ttu-id="47b8c-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="47b8c-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="47b8c-114">Vytvoření skupiny az</span><span class="sxs-lookup"><span data-stu-id="47b8c-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="47b8c-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="47b8c-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="47b8c-116">Vytvoření az keyvault</span><span class="sxs-lookup"><span data-stu-id="47b8c-116">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="47b8c-117">Vytvoří Azure Key Vault toostore zabezpečené data, jako jsou šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="47b8c-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="47b8c-118">Vytvořte klíč keyvault az</span><span class="sxs-lookup"><span data-stu-id="47b8c-118">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="47b8c-119">Vytvoří šifrovací klíč v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="47b8c-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="47b8c-120">AZ ad sp vytvořit pro rbac</span><span class="sxs-lookup"><span data-stu-id="47b8c-120">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="47b8c-121">Vytvoří služby Azure Active Directory hlavní toosecurely ověřování a řízení přístupu tooencryption klíče.</span><span class="sxs-lookup"><span data-stu-id="47b8c-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="47b8c-122">AZ keyvault set zásad</span><span class="sxs-lookup"><span data-stu-id="47b8c-122">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="47b8c-123">Nastaví oprávnění hello Key Vault toogrant hello služby hlavní přístup tooencryption klíče.</span><span class="sxs-lookup"><span data-stu-id="47b8c-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="47b8c-124">Vytvoření virtuálního počítače az</span><span class="sxs-lookup"><span data-stu-id="47b8c-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="47b8c-125">Vytvoří hello virtuální počítač a připojí ho toohello síťové karty, virtuální síť, podsíť a NSG.</span><span class="sxs-lookup"><span data-stu-id="47b8c-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="47b8c-126">Tento příkaz také určuje toobe bitové kopie virtuálního počítače hello používá a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="47b8c-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="47b8c-127">Povolit šifrování az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="47b8c-127">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="47b8c-128">Povoluje šifrování na virtuálním počítači pomocí pověření hlavní služby hello a šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="47b8c-128">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="47b8c-129">Zobrazit šifrování az virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="47b8c-129">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="47b8c-130">Zobrazuje stav hello hello proces šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="47b8c-130">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="47b8c-131">Odstranění skupiny az</span><span class="sxs-lookup"><span data-stu-id="47b8c-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="47b8c-132">Odstraní skupinu prostředků, včetně všech vnořených prostředků.</span><span class="sxs-lookup"><span data-stu-id="47b8c-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="47b8c-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47b8c-133">Next steps</span></span>

<span data-ttu-id="47b8c-134">Další informace o hello rozhraní příkazového řádku Azure najdete v tématu [dokumentaci k rozhraní příkazového řádku Azure](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="47b8c-134">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="47b8c-135">Ukázky skriptu rozhraní příkazového řádku další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="47b8c-135">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span></span>
