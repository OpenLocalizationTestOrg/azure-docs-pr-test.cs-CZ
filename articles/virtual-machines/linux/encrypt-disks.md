---
title: "Šifrování disky na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Postup zašifrování virtuální disky na virtuální počítač s Linuxem pro zvýšení zabezpečení pomocí Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 172b4c8f5c098d776cb689543f5d8f163b8895b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-linux-vm"></a><span data-ttu-id="1da5c-103">Postup zašifrování virtuální disky na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="1da5c-103">How to encrypt virtual disks on a Linux VM</span></span>
<span data-ttu-id="1da5c-104">Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure.</span><span class="sxs-lookup"><span data-stu-id="1da5c-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="1da5c-105">Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="1da5c-106">Řízení těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="1da5c-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="1da5c-107">Tento článek podrobně popisují zašifrovat virtuální disky na virtuální počítač s Linuxem pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="1da5c-107">This article details how to encrypt virtual disks on a Linux VM using the Azure CLI 2.0.</span></span> <span data-ttu-id="1da5c-108">K provedení těchto kroků můžete také využít [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1da5c-108">You can also perform these steps with the [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="1da5c-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="1da5c-109">Quick commands</span></span>
<span data-ttu-id="1da5c-110">Pokud potřebujete rychle provedení úlohy, následující část podrobně popisuje základní příkazy pro zašifrování virtuální disky na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1da5c-110">If you need to quickly accomplish the task, the following section details the base commands to encrypt virtual disks on your VM.</span></span> <span data-ttu-id="1da5c-111">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="1da5c-111">More detailed information and context for each step can be found the rest of the document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="1da5c-112">Budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="1da5c-112">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="1da5c-113">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1da5c-113">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="1da5c-114">Zahrnout názvy parametrů příklad *myResourceGroup*, *myKey*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="1da5c-114">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="1da5c-115">Nejprve povolit zprostředkovatele Azure Key Vault v rámci vašeho předplatného Azure s [registrace zprostředkovatele az](/cli/azure/provider#register) a vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1da5c-115">First, enable the Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="1da5c-116">Následující příklad vytvoří název skupiny prostředků *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="1da5c-116">The following example creates a resource group name *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="1da5c-117">Vytvoření Azure Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolte Key Vault pro použití s šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1da5c-117">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="1da5c-118">Zadejte jedinečný název pro Key Vault *keyvault_name* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1da5c-118">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="1da5c-119">Vytvoření kryptografické klíče v Key Vault s [vytvořit klíč keyvault az](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="1da5c-119">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="1da5c-120">Následující příklad vytvoří klíč s názvem *myKey*:</span><span class="sxs-lookup"><span data-stu-id="1da5c-120">The following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

<span data-ttu-id="1da5c-121">Vytvořit objekt služby pomocí služby Azure Active Directory s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="1da5c-121">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="1da5c-122">Objekt služby zpracovává ověřování a výměnu kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-122">The service principal handles the authentication and exchange of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="1da5c-123">Následující příklad načte v hodnoty pro objekt služby Id a heslo pro použití v novější příkazy:</span><span class="sxs-lookup"><span data-stu-id="1da5c-123">The following example reads in the values for the service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="1da5c-124">Heslo je výstup pouze při vytváření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="1da5c-124">The password is only output when you create the service principal.</span></span> <span data-ttu-id="1da5c-125">V případě potřeby, zobrazení a zaznamenejte heslo (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="1da5c-125">If desired, view and record the password (`echo $sp_password`).</span></span> <span data-ttu-id="1da5c-126">Můžete vytvořit seznam hlavních objektů s několika vaší služby [az ad sp seznamu](/cli/azure/ad/sp#list) a zobrazit další informace o konkrétní objekt s služby [az ad sp zobrazit](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="1da5c-126">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="1da5c-127">Nastavení oprávnění pro Key Vault s [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="1da5c-127">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="1da5c-128">V následujícím příkladu je zadané ID objektu zabezpečení služby z předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="1da5c-128">In the following example, the service principal ID is supplied from the preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

<span data-ttu-id="1da5c-129">Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojit datový disk 5 Gb.</span><span class="sxs-lookup"><span data-stu-id="1da5c-129">Create a VM with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="1da5c-130">Pouze některé bitové kopie marketplace podporují šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1da5c-130">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="1da5c-131">Následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí **CentOS 7.2n** bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="1da5c-131">The following example creates a VM named `myVM` using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="1da5c-132">SSH pro virtuální počítač pomocí `publicIpAddress` zobrazené ve výstupu předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="1da5c-132">SSH to your VM using the `publicIpAddress` shown in the output of the preceding command.</span></span> <span data-ttu-id="1da5c-133">Vytvořit oddíl a systému souborů a pak připojit datový disk.</span><span class="sxs-lookup"><span data-stu-id="1da5c-133">Create a partition and filesystem, then mount the data disk.</span></span> <span data-ttu-id="1da5c-134">Další informace najdete v tématu [připojit k virtuální počítač s Linuxem připojit nový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="1da5c-134">For more information, see [Connect to a Linux VM to mount the new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="1da5c-135">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="1da5c-135">Close your SSH session.</span></span>

<span data-ttu-id="1da5c-136">Šifrování virtuálního počítače s [povolit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="1da5c-136">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="1da5c-137">Následující příklad používá `$sp_id` a `$sp_password` proměnné z předchozí `ad sp create-for-rbac` příkaz:</span><span class="sxs-lookup"><span data-stu-id="1da5c-137">The following example uses the `$sp_id` and `$sp_password` variables from the preceding `ad sp create-for-rbac` command:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<span data-ttu-id="1da5c-138">Pro proces šifrování disku pro dokončení chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="1da5c-138">It takes some time for the disk encryption process to complete.</span></span> <span data-ttu-id="1da5c-139">Monitorování stavu procesu s [zobrazit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="1da5c-139">Monitor the status of the process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1da5c-140">Zobrazuje stav **EncryptionInProgress**.</span><span class="sxs-lookup"><span data-stu-id="1da5c-140">The status shows **EncryptionInProgress**.</span></span> <span data-ttu-id="1da5c-141">Počkejte, dokud stav operačního systému na disku sestavy **VMRestartPending**, potom restartujte virtuální počítač s [restartování virtuálního počítače az](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="1da5c-141">Wait until the status for the OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1da5c-142">Proces šifrování disku je dokončené během spouštění, proto Počkejte několik minut před zaškrtnutím stav šifrování znovu s **zobrazit šifrování virtuálních počítačů az**:</span><span class="sxs-lookup"><span data-stu-id="1da5c-142">The disk encryption process is finalized during the boot process, so wait a few minutes before checking the status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1da5c-143">Stav má teď disk operačního systému i datový disk jako **šifrovaný**.</span><span class="sxs-lookup"><span data-stu-id="1da5c-143">The status should now report both the OS disk and data disk as **Encrypted**.</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="1da5c-144">Přehled šifrování disku</span><span class="sxs-lookup"><span data-stu-id="1da5c-144">Overview of disk encryption</span></span>
<span data-ttu-id="1da5c-145">Virtuální disky na virtuální počítače s Linuxem se šifrují pomocí rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="1da5c-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="1da5c-146">Není nijak zpoplatněn pro šifrování virtuálních disků v Azure.</span><span class="sxs-lookup"><span data-stu-id="1da5c-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="1da5c-147">Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikovány pro FIPS 140-2 úroveň 2 standardů.</span><span class="sxs-lookup"><span data-stu-id="1da5c-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="1da5c-148">Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="1da5c-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="1da5c-149">Tyto klíče se používají k šifrování a dešifrování virtuálních disků připojených k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="1da5c-149">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="1da5c-150">Hlavní služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="1da5c-150">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="1da5c-151">Proces šifrování virtuálního počítače je následující:</span><span class="sxs-lookup"><span data-stu-id="1da5c-151">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="1da5c-152">Vytvoření kryptografické klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="1da5c-153">Nakonfigurujte kryptografický klíč možné používat pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="1da5c-153">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="1da5c-154">Čtení kryptografického klíče z Azure Key Vault, vytvořte služby Azure Active Directory objekt s příslušnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="1da5c-154">To read the cryptographic key from the Azure Key Vault, create an Azure Active Directory service principal with the appropriate permissions.</span></span>
4. <span data-ttu-id="1da5c-155">Příkaz pro zašifrování virtuální disky, zadávání Azure Active Directory service hlavní a vhodný kryptografické klíče k použití.</span><span class="sxs-lookup"><span data-stu-id="1da5c-155">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory service principal and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="1da5c-156">Objekt služby Azure Active Directory vyžaduje požadované kryptografický klíč z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-156">The Azure Active Directory service principal requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="1da5c-157">Virtuální disky jsou šifrované pomocí zadaný kryptografický klíč.</span><span class="sxs-lookup"><span data-stu-id="1da5c-157">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="1da5c-158">Proces šifrování</span><span class="sxs-lookup"><span data-stu-id="1da5c-158">Encryption process</span></span>
<span data-ttu-id="1da5c-159">Šifrování disku spoléhá na následující součásti:</span><span class="sxs-lookup"><span data-stu-id="1da5c-159">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="1da5c-160">**Azure Key Vault** – používané k ochraně kryptografické klíče a tajné klíče pro proces šifrování a dešifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1da5c-160">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span>
  * <span data-ttu-id="1da5c-161">Pokud ano, můžete použít existující Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="1da5c-162">Není nutné vyhradit Key Vault pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="1da5c-162">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="1da5c-163">K oddělení hranicemi správy a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-163">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="1da5c-164">**Azure Active Directory** -zpracovává zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.</span><span class="sxs-lookup"><span data-stu-id="1da5c-164">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="1da5c-165">Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1da5c-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="1da5c-166">Objekt služby poskytuje zabezpečené mechanismus k vyžádání a vydávají příslušné kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="1da5c-166">The service principal provides a secure mechanism to request and be issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="1da5c-167">Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1da5c-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="1da5c-168">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="1da5c-168">Requirements and limitations</span></span>
<span data-ttu-id="1da5c-169">Podporované scénáře a požadavky na šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="1da5c-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="1da5c-170">Následující server Linux SKU - Ubuntu, CentOS, SUSE a SUSE Linux Enterprise Server (SLES) a Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="1da5c-170">The following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="1da5c-171">Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být ve stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="1da5c-171">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="1da5c-172">Standardní A, D, DS, G a GS řada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1da5c-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="1da5c-173">Šifrování disku není aktuálně podporováno v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="1da5c-173">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="1da5c-174">Úroveň Basic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1da5c-174">Basic tier VMs.</span></span>
* <span data-ttu-id="1da5c-175">Virtuální počítače vytvořené pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="1da5c-175">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="1da5c-176">Zakázáním šifrování disku operačního systému na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1da5c-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="1da5c-177">Aktualizuje se kryptografické klíče na virtuální počítač již šifrované Linux.</span><span class="sxs-lookup"><span data-stu-id="1da5c-177">Updating the cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="1da5c-178">Vytvoření Azure Key Vault a klíče</span><span class="sxs-lookup"><span data-stu-id="1da5c-178">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="1da5c-179">Budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="1da5c-179">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="1da5c-180">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1da5c-180">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="1da5c-181">Zahrnout názvy parametrů příklad *myResourceGroup*, *myKey*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="1da5c-181">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="1da5c-182">Prvním krokem je vytvoření Azure Key Vault pro ukládání kryptografických klíčů.</span><span class="sxs-lookup"><span data-stu-id="1da5c-182">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="1da5c-183">Azure Key Vault můžete uložit klíčů, tajné klíče nebo hesla, které vám umožní bezpečně je implementovat v vašim aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="1da5c-183">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="1da5c-184">Šifrování virtuálního disku použijete k uložení kryptografického klíče, který se používá k šifrování a dešifrování virtuální disky Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-184">For virtual disk encryption, you use Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="1da5c-185">Povolit Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [registrace zprostředkovatele az](/cli/azure/provider#register) a vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1da5c-185">Enable the Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="1da5c-186">Následující příklad vytvoří název skupiny prostředků *myResourceGroup* v `eastus` umístění:</span><span class="sxs-lookup"><span data-stu-id="1da5c-186">The following example creates a resource group name *myResourceGroup* in the `eastus` location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="1da5c-187">Azure Key Vault, který obsahuje kryptografické klíče a přidružené výpočetní prostředky, jako je například úložiště a virtuální počítač se musí nacházet ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="1da5c-187">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="1da5c-188">Vytvoření Azure Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolte Key Vault pro použití s šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1da5c-188">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="1da5c-189">Zadejte jedinečný název pro Key Vault *keyvault_name* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1da5c-189">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="1da5c-190">Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="1da5c-190">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="1da5c-191">Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-191">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="1da5c-192">Není dalších nákladů na vytváření premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem.</span><span class="sxs-lookup"><span data-stu-id="1da5c-192">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="1da5c-193">Chcete-li vytvořit premium Key Vault, v předchozím kroku přidejte `--sku Premium` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="1da5c-193">To create a premium Key Vault, in the preceding step add `--sku Premium` to the command.</span></span> <span data-ttu-id="1da5c-194">Následující příklad používá klíče chráněné softwarem, protože jsme vytvořili standardní Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-194">The following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="1da5c-195">Pro oba modely ochrany musí mít udělen přístup k žádosti o kryptografických klíčů, když se virtuální počítač spustí do dešifrovat virtuální disky platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="1da5c-195">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="1da5c-196">Vytvoření kryptografické klíče v Key Vault s [vytvořit klíč keyvault az](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="1da5c-196">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="1da5c-197">Následující příklad vytvoří klíč s názvem *myKey*:</span><span class="sxs-lookup"><span data-stu-id="1da5c-197">The following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-the-azure-active-directory-service-principal"></a><span data-ttu-id="1da5c-198">Vytvořit objekt služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1da5c-198">Create the Azure Active Directory service principal</span></span>
<span data-ttu-id="1da5c-199">Když virtuální disky jsou zašifrovaná nebo dešifrovat, je třeba zadat účet, který chcete zpracovávat ověřování a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1da5c-199">When virtual disks are encrypted or decrypted, you specify an account to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="1da5c-200">Tento účet objektu zabezpečení služby Azure Active Directory umožňuje platformy Azure k vyžádání příslušné kryptografické klíče jménem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1da5c-200">This account, an Azure Active Directory service principal, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="1da5c-201">Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1da5c-201">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="1da5c-202">Vytvořit objekt služby pomocí služby Azure Active Directory s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="1da5c-202">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="1da5c-203">Následující příklad načte v hodnoty pro objekt služby Id a heslo pro použití v novější příkazy:</span><span class="sxs-lookup"><span data-stu-id="1da5c-203">The following example reads in the values for the service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="1da5c-204">Heslo se zobrazí jenom při vytvoření objektu služby.</span><span class="sxs-lookup"><span data-stu-id="1da5c-204">The password is only displayed when you create the service principal.</span></span> <span data-ttu-id="1da5c-205">V případě potřeby, zobrazení a zaznamenejte heslo (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="1da5c-205">If desired, view and record the password (`echo $sp_password`).</span></span> <span data-ttu-id="1da5c-206">Můžete vytvořit seznam hlavních objektů s několika vaší služby [az ad sp seznamu](/cli/azure/ad/sp#list) a zobrazit další informace o konkrétní objekt s služby [az ad sp zobrazit](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="1da5c-206">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="1da5c-207">Pro úspěšně šifrování nebo dešifrování virtuálních disků, musí se nastavit oprávnění na kryptografický klíč uložený v Key Vault tak, aby povolovala objektu služby Azure Active Directory ke čtení klíče.</span><span class="sxs-lookup"><span data-stu-id="1da5c-207">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory service principal to read the keys.</span></span> <span data-ttu-id="1da5c-208">Nastavení oprávnění pro Key Vault s [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="1da5c-208">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="1da5c-209">V následujícím příkladu je zadané ID objektu zabezpečení služby z předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="1da5c-209">In the following example, the service principal ID is supplied from the preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a><span data-ttu-id="1da5c-210">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1da5c-210">Create virtual machine</span></span>
<span data-ttu-id="1da5c-211">Ve skutečnosti šifrování některé virtuální disky, umožňuje vytvoření virtuálního počítače a přidat datový disk.</span><span class="sxs-lookup"><span data-stu-id="1da5c-211">To actually encrypt some virtual disks, lets create a VM and add a data disk.</span></span> <span data-ttu-id="1da5c-212">Vytvoření virtuálního počítače k šifrování s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojit datový disk 5 Gb.</span><span class="sxs-lookup"><span data-stu-id="1da5c-212">Create a VM to encrypt with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="1da5c-213">Pouze některé bitové kopie marketplace podporují šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1da5c-213">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="1da5c-214">Následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí **CentOS 7.2n** bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="1da5c-214">The following example creates a VM named *myVM* using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="1da5c-215">SSH pro virtuální počítač pomocí `publicIpAddress` zobrazené ve výstupu předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="1da5c-215">SSH to your VM using the `publicIpAddress` shown in the output of the preceding command.</span></span> <span data-ttu-id="1da5c-216">Vytvořit oddíl a systému souborů a pak připojit datový disk.</span><span class="sxs-lookup"><span data-stu-id="1da5c-216">Create a partition and filesystem, then mount the data disk.</span></span> <span data-ttu-id="1da5c-217">Další informace najdete v tématu [připojit k virtuální počítač s Linuxem připojit nový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="1da5c-217">For more information, see [Connect to a Linux VM to mount the new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="1da5c-218">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="1da5c-218">Close your SSH session.</span></span>


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="1da5c-219">Šifrování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1da5c-219">Encrypt virtual machine</span></span>
<span data-ttu-id="1da5c-220">Pokud chcete zašifrovat virtuální disky, přepnutím společně předchozí komponenty:</span><span class="sxs-lookup"><span data-stu-id="1da5c-220">To encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="1da5c-221">Zadejte objektu služby Azure Active Directory a heslo.</span><span class="sxs-lookup"><span data-stu-id="1da5c-221">Specify the Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="1da5c-222">Zadejte klíč trezoru k ukládání metadat pro šifrované disky.</span><span class="sxs-lookup"><span data-stu-id="1da5c-222">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="1da5c-223">Zadejte kryptografické klíče k použití pro skutečné šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="1da5c-223">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="1da5c-224">Zadejte, jestli chcete šifrovat disk operačního systému, datových disků nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="1da5c-224">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="1da5c-225">Šifrování virtuálního počítače s [povolit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="1da5c-225">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="1da5c-226">Následující příklad používá `$sp_id` a `$sp_password` proměnné z předchozí `ad sp create-for-rbac` příkaz:</span><span class="sxs-lookup"><span data-stu-id="1da5c-226">The following example uses the `$sp_id` and `$sp_password` variables from the preceding `ad sp create-for-rbac` command:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<span data-ttu-id="1da5c-227">Pro proces šifrování disku pro dokončení chvíli trvat.</span><span class="sxs-lookup"><span data-stu-id="1da5c-227">It takes some time for the disk encryption process to complete.</span></span> <span data-ttu-id="1da5c-228">Monitorování stavu procesu s [zobrazit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="1da5c-228">Monitor the status of the process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1da5c-229">Výstup bude vypadat v následujícím příkladu zkrácený:</span><span class="sxs-lookup"><span data-stu-id="1da5c-229">The output is similar to the following truncated example:</span></span>

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

<span data-ttu-id="1da5c-230">Počkejte, dokud stav operačního systému na disku sestavy **VMRestartPending**, potom restartujte virtuální počítač s [restartování virtuálního počítače az](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="1da5c-230">Wait until the status for the OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1da5c-231">Proces šifrování disku je dokončené během spouštění, proto Počkejte několik minut před zaškrtnutím stav šifrování znovu s **zobrazit šifrování virtuálních počítačů az**:</span><span class="sxs-lookup"><span data-stu-id="1da5c-231">The disk encryption process is finalized during the boot process, so wait a few minutes before checking the status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="1da5c-232">Stav má teď disk operačního systému i datový disk jako **šifrovaný**.</span><span class="sxs-lookup"><span data-stu-id="1da5c-232">The status should now report both the OS disk and data disk as **Encrypted**.</span></span>


## <a name="add-additional-data-disks"></a><span data-ttu-id="1da5c-233">Přidání dalších datových disků</span><span class="sxs-lookup"><span data-stu-id="1da5c-233">Add additional data disks</span></span>
<span data-ttu-id="1da5c-234">Jakmile jste zašifrovali datových disků, můžete později přidat další virtuální disky pro virtuální počítač a také zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="1da5c-234">Once you have encrypted your data disks, you can later add additional virtual disks to your VM and also encrypt them.</span></span> <span data-ttu-id="1da5c-235">Umožňuje například přidat druhý virtuální disk k virtuálnímu počítači následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1da5c-235">For example, lets add a second virtual disk to your VM as follows:</span></span>

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

<span data-ttu-id="1da5c-236">Spusťte znovu příkaz pro zašifrování virtuální disky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1da5c-236">Re-run the command to encrypt the virtual disks as follows:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a><span data-ttu-id="1da5c-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1da5c-237">Next steps</span></span>
* <span data-ttu-id="1da5c-238">Další informace o správě Azure Key Vault, včetně odstraňování kryptografické klíče a trezory, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="1da5c-238">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="1da5c-239">Další informace o šifrování disku, jako je například příprava šifrované vlastních virtuálních počítačů se nahrát do Azure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="1da5c-239">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
