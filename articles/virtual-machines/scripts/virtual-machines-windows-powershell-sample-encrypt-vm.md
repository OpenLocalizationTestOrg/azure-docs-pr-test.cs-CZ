---
title: "aaaAzure ukázkový skript prostředí PowerShell - zašifrovat virtuální počítač s Windows | Microsoft Docs"
description: "Azure skript prostředí PowerShell ukázkový – zašifrovat Windows virtuálního počítače"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 80c52a0b734a52a051ed30026b294840fd521143
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-with-azure-powershell"></a><span data-ttu-id="1e448-103">Šifrování virtuálního počítače s Windows v prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1e448-103">Encrypt a Windows virtual machine with Azure PowerShell</span></span>

<span data-ttu-id="1e448-104">Tento skript vytvoří zabezpečené Azure Key Vault, šifrovacích klíčů, objektu služby Azure Active Directory a systému Windows virtuálního počítače (VM).</span><span class="sxs-lookup"><span data-stu-id="1e448-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="1e448-105">Hello virtuálních počítačů se potom šifruje pomocí hello šifrovacího klíče z Key Vault a hlavní pověření služby.</span><span class="sxs-lookup"><span data-stu-id="1e448-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1e448-106">Ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="1e448-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/encrypt-vm/encrypt-windows-vm.ps1 "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1e448-107">Vyčištění nasazení</span><span class="sxs-lookup"><span data-stu-id="1e448-107">Clean up deployment</span></span> 

<span data-ttu-id="1e448-108">Spusťte následující příkaz tooremove hello prostředků skupiny virtuálních počítačů a všechny související prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="1e448-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="1e448-109">Vysvětlení skriptu</span><span class="sxs-lookup"><span data-stu-id="1e448-109">Script explanation</span></span>

<span data-ttu-id="1e448-110">Tento skript používá hello následující příkazy toocreate hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="1e448-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="1e448-111">Každou položku v tabulce hello propojí toocommand konkrétní dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1e448-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1e448-112">Příkaz</span><span class="sxs-lookup"><span data-stu-id="1e448-112">Command</span></span> | <span data-ttu-id="1e448-113">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1e448-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1e448-114">Nový AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1e448-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="1e448-115">Vytvoří skupinu prostředků, ve kterém jsou uložené všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="1e448-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1e448-116">Nový AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="1e448-116">New-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/new-azurermkeyvault) | <span data-ttu-id="1e448-117">Vytvoří Azure Key Vault toostore zabezpečené data, jako jsou šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="1e448-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="1e448-118">Přidat AzureKeyVaultKey</span><span class="sxs-lookup"><span data-stu-id="1e448-118">Add-AzureKeyVaultKey</span></span>](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) | <span data-ttu-id="1e448-119">Vytvoří šifrovací klíč v Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e448-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="1e448-120">Nové AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="1e448-120">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="1e448-121">Vytvoří služby Azure Active Directory hlavní toosecurely ověřování a řízení přístupu tooencryption klíče.</span><span class="sxs-lookup"><span data-stu-id="1e448-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="1e448-122">Set-AzureRmKeyVaultAccessPolicy</span><span class="sxs-lookup"><span data-stu-id="1e448-122">Set-AzureRmKeyVaultAccessPolicy</span></span>](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) | <span data-ttu-id="1e448-123">Nastaví oprávnění hello Key Vault toogrant hello služby hlavní přístup tooencryption klíče.</span><span class="sxs-lookup"><span data-stu-id="1e448-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="1e448-124">Nové AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="1e448-124">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="1e448-125">Vytvoří konfiguraci podsítě.</span><span class="sxs-lookup"><span data-stu-id="1e448-125">Creates a subnet configuration.</span></span> <span data-ttu-id="1e448-126">Tato konfigurace se používá s procesem vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="1e448-126">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="1e448-127">Nový AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="1e448-127">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="1e448-128">Vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="1e448-128">Creates a virtual network.</span></span> |
| [<span data-ttu-id="1e448-129">Nové AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="1e448-129">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="1e448-130">Vytvoří veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="1e448-130">Creates a public IP address.</span></span> |
| [<span data-ttu-id="1e448-131">Nové AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="1e448-131">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="1e448-132">Vytvoří pravidlo konfiguraci skupiny zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1e448-132">Creates a network security group rule configuration.</span></span> <span data-ttu-id="1e448-133">Tato konfigurace je použité toocreate pravidlo NSG při vytvoření hello NSG.</span><span class="sxs-lookup"><span data-stu-id="1e448-133">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="1e448-134">Nové AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="1e448-134">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="1e448-135">Vytvoří skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1e448-135">Creates a network security group.</span></span> |
| [<span data-ttu-id="1e448-136">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="1e448-136">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="1e448-137">Získá informace o podsíti.</span><span class="sxs-lookup"><span data-stu-id="1e448-137">Gets subnet information.</span></span> <span data-ttu-id="1e448-138">Tato informace se používají při vytváření síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1e448-138">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="1e448-139">Nové AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="1e448-139">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="1e448-140">Vytvoří rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="1e448-140">Creates a network interface.</span></span> |
| [<span data-ttu-id="1e448-141">Nové AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="1e448-141">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="1e448-142">Vytvoří konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e448-142">Creates a VM configuration.</span></span> <span data-ttu-id="1e448-143">Tato konfigurace zahrnuje informace, jako je název virtuálního počítače, operační systém a pověření pro správu.</span><span class="sxs-lookup"><span data-stu-id="1e448-143">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="1e448-144">Konfigurace Hello se používá při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e448-144">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="1e448-145">Nový AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="1e448-145">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="1e448-146">Vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e448-146">Create a virtual machine.</span></span> |
| [<span data-ttu-id="1e448-147">Get-AzureRmKeyVault</span><span class="sxs-lookup"><span data-stu-id="1e448-147">Get-AzureRmKeyVault</span></span>](/powershell/module/azurerm.keyvault/get-azurermkeyvault) | <span data-ttu-id="1e448-148">Získá požadované informace na hello Key Vault</span><span class="sxs-lookup"><span data-stu-id="1e448-148">Gets required information on hello Key Vault</span></span> |
| [<span data-ttu-id="1e448-149">Set-AzureRmVMDiskEncryptionExtension</span><span class="sxs-lookup"><span data-stu-id="1e448-149">Set-AzureRmVMDiskEncryptionExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) | <span data-ttu-id="1e448-150">Povoluje šifrování na virtuálním počítači pomocí pověření hlavní služby hello a šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="1e448-150">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="1e448-151">Get-AzureRmVmDiskEncryptionStatus</span><span class="sxs-lookup"><span data-stu-id="1e448-151">Get-AzureRmVmDiskEncryptionStatus</span></span>](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) | <span data-ttu-id="1e448-152">Zobrazuje stav hello hello proces šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e448-152">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="1e448-153">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1e448-153">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="1e448-154">Odebere skupinu prostředků a všechny prostředky obsažené v rámci.</span><span class="sxs-lookup"><span data-stu-id="1e448-154">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1e448-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e448-155">Next steps</span></span>

<span data-ttu-id="1e448-156">Další informace o modulu Azure PowerShell hello najdete v tématu [dokumentace Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1e448-156">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="1e448-157">Ukázky skriptu PowerShell další virtuální počítač nachází v hello [virtuálního počítače Windows Azure dokumentaci](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e448-157">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
