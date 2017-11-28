---
title: "aaaEncrypt disky na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Jak hello tooencrypt virtuální disky na virtuální počítač s Linuxem pro lepší zabezpečení pomocí Azure CLI 2.0"
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
ms.openlocfilehash: d6197742bc8562630e8395588c072093fc01d614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-linux-vm"></a><span data-ttu-id="cba9e-103">Jak tooencrypt virtuální disky na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="cba9e-103">How tooencrypt virtual disks on a Linux VM</span></span>
<span data-ttu-id="cba9e-104">Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure.</span><span class="sxs-lookup"><span data-stu-id="cba9e-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="cba9e-105">Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="cba9e-106">Řízení těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="cba9e-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="cba9e-107">Tento článek podrobně popisuje, jak hello tooencrypt virtuální disky na virtuální počítač s Linuxem pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="cba9e-107">This article details how tooencrypt virtual disks on a Linux VM using hello Azure CLI 2.0.</span></span> <span data-ttu-id="cba9e-108">Můžete také provést tyto kroky hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cba9e-108">You can also perform these steps with hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="cba9e-109">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="cba9e-109">Quick commands</span></span>
<span data-ttu-id="cba9e-110">Pokud je třeba tooquickly dosáhnout hello, hello následující části Podrobnosti hello základní příkazy tooencrypt virtuální disky na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="cba9e-110">If you need tooquickly accomplish hello task, hello following section details hello base commands tooencrypt virtual disks on your VM.</span></span> <span data-ttu-id="cba9e-111">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="cba9e-111">More detailed information and context for each step can be found hello rest of hello document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="cba9e-112">Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cba9e-112">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="cba9e-113">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="cba9e-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cba9e-114">Zahrnout názvy parametrů příklad *myResourceGroup*, *myKey*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="cba9e-114">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="cba9e-115">Nejprve povolit hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [registrace zprostředkovatele az](/cli/azure/provider#register) a vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="cba9e-115">First, enable hello Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="cba9e-116">Hello následující příklad vytvoří název skupiny prostředků *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="cba9e-116">hello following example creates a resource group name *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="cba9e-117">Vytvoření Azure Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolte hello Key Vault pro použití s šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="cba9e-117">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="cba9e-118">Zadejte jedinečný název pro Key Vault *keyvault_name* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cba9e-118">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="cba9e-119">Vytvoření kryptografické klíče v Key Vault s [vytvořit klíč keyvault az](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="cba9e-119">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="cba9e-120">Hello následující příklad vytvoří klíč s názvem *myKey*:</span><span class="sxs-lookup"><span data-stu-id="cba9e-120">hello following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

<span data-ttu-id="cba9e-121">Vytvořit objekt služby pomocí služby Azure Active Directory s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="cba9e-121">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="cba9e-122">hlavní obslužných rutin služby Hello hello ověřování a výměnu kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-122">hello service principal handles hello authentication and exchange of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="cba9e-123">Následující ukázka Hello čtení hodnoty hello hello instanční objekt Id a heslo pro použití v novější příkazy:</span><span class="sxs-lookup"><span data-stu-id="cba9e-123">hello following example reads in hello values for hello service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="cba9e-124">Při vytváření hello instanční objekt se nachází pouze výstup Hello heslo.</span><span class="sxs-lookup"><span data-stu-id="cba9e-124">hello password is only output when you create hello service principal.</span></span> <span data-ttu-id="cba9e-125">V případě potřeby, zobrazení a záznam hello heslo (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="cba9e-125">If desired, view and record hello password (`echo $sp_password`).</span></span> <span data-ttu-id="cba9e-126">Můžete vytvořit seznam hlavních objektů s několika vaší služby [az ad sp seznamu](/cli/azure/ad/sp#list) a zobrazit další informace o konkrétní objekt s služby [az ad sp zobrazit](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="cba9e-126">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="cba9e-127">Nastavení oprávnění pro Key Vault s [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="cba9e-127">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="cba9e-128">V následujícím příkladu hello hello ID objektu zabezpečení služby je dodávána hello předcházející příkaz:</span><span class="sxs-lookup"><span data-stu-id="cba9e-128">In hello following example, hello service principal ID is supplied from hello preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

<span data-ttu-id="cba9e-129">Vytvoření virtuálního počítače s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojit datový disk 5 Gb.</span><span class="sxs-lookup"><span data-stu-id="cba9e-129">Create a VM with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="cba9e-130">Pouze některé bitové kopie marketplace podporují šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="cba9e-130">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="cba9e-131">Hello následující příklad vytvoří virtuální počítač s názvem `myVM` pomocí **CentOS 7.2n** bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="cba9e-131">hello following example creates a VM named `myVM` using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="cba9e-132">SSH tooyour virtuální počítač pomocí hello `publicIpAddress` ukazuje výstup hello hello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="cba9e-132">SSH tooyour VM using hello `publicIpAddress` shown in hello output of hello preceding command.</span></span> <span data-ttu-id="cba9e-133">Vytvořit oddíl a systému souborů a pak připojit datový disk hello.</span><span class="sxs-lookup"><span data-stu-id="cba9e-133">Create a partition and filesystem, then mount hello data disk.</span></span> <span data-ttu-id="cba9e-134">Další informace najdete v tématu [připojit tooa virtuálního počítače s Linuxem toomount hello nový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="cba9e-134">For more information, see [Connect tooa Linux VM toomount hello new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="cba9e-135">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="cba9e-135">Close your SSH session.</span></span>

<span data-ttu-id="cba9e-136">Šifrování virtuálního počítače s [povolit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="cba9e-136">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="cba9e-137">Hello následující příklad používá hello `$sp_id` a `$sp_password` proměnné z předchozí hello `ad sp create-for-rbac` příkaz:</span><span class="sxs-lookup"><span data-stu-id="cba9e-137">hello following example uses hello `$sp_id` and `$sp_password` variables from hello preceding `ad sp create-for-rbac` command:</span></span>

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

<span data-ttu-id="cba9e-138">Toocomplete proces šifrování disku hello nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="cba9e-138">It takes some time for hello disk encryption process toocomplete.</span></span> <span data-ttu-id="cba9e-139">Monitorování stavu hello hello proces s [zobrazit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="cba9e-139">Monitor hello status of hello process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cba9e-140">Dobrý den zobrazí stav **EncryptionInProgress**.</span><span class="sxs-lookup"><span data-stu-id="cba9e-140">hello status shows **EncryptionInProgress**.</span></span> <span data-ttu-id="cba9e-141">Počkat, až hello stav pro sestavy disku hello OS **VMRestartPending**, potom restartujte virtuální počítač s [restartování virtuálního počítače az](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="cba9e-141">Wait until hello status for hello OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cba9e-142">Hello proces šifrování disku je dokončen. během procesu spuštění hello, proto Počkejte několik minut před zaškrtnutím hello stav šifrování znovu s **zobrazit šifrování virtuálních počítačů az**:</span><span class="sxs-lookup"><span data-stu-id="cba9e-142">hello disk encryption process is finalized during hello boot process, so wait a few minutes before checking hello status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cba9e-143">Stav Hello by teď disku hello operačního systému i datový disk jako **šifrovaný**.</span><span class="sxs-lookup"><span data-stu-id="cba9e-143">hello status should now report both hello OS disk and data disk as **Encrypted**.</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="cba9e-144">Přehled šifrování disku</span><span class="sxs-lookup"><span data-stu-id="cba9e-144">Overview of disk encryption</span></span>
<span data-ttu-id="cba9e-145">Virtuální disky na virtuální počítače s Linuxem se šifrují pomocí rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="cba9e-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="cba9e-146">Není nijak zpoplatněn pro šifrování virtuálních disků v Azure.</span><span class="sxs-lookup"><span data-stu-id="cba9e-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="cba9e-147">Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikované tooFIPS standardy úroveň 2 140-2.</span><span class="sxs-lookup"><span data-stu-id="cba9e-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="cba9e-148">Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="cba9e-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="cba9e-149">Tyto kryptografické klíče jsou použité tooencrypt a dešifrování tooyour připojené virtuální disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cba9e-149">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="cba9e-150">Hlavní služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="cba9e-150">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="cba9e-151">Hello proces šifrování virtuálního počítače je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cba9e-151">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="cba9e-152">Vytvoření kryptografické klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="cba9e-153">Nakonfigurujte hello kryptografické klíče toobe použitelné pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="cba9e-153">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="cba9e-154">tooread hello kryptografický klíč z hello Azure Key Vault, vytvoření služby Azure Active Directory objekt zabezpečení s příslušnými oprávněními hello.</span><span class="sxs-lookup"><span data-stu-id="cba9e-154">tooread hello cryptographic key from hello Azure Key Vault, create an Azure Active Directory service principal with hello appropriate permissions.</span></span>
4. <span data-ttu-id="cba9e-155">Vydejte příkaz tooencrypt hello virtuální disky, zadáte hello objektu služby Azure Active Directory a příslušné kryptografické klíče toobe použít.</span><span class="sxs-lookup"><span data-stu-id="cba9e-155">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory service principal and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="cba9e-156">hlavní požadavky služby Azure Active Directory Hello hello požadované kryptografický klíč z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-156">hello Azure Active Directory service principal requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="cba9e-157">Hello virtuální disky jsou šifrované pomocí hello zadaný kryptografický klíč.</span><span class="sxs-lookup"><span data-stu-id="cba9e-157">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="cba9e-158">Proces šifrování</span><span class="sxs-lookup"><span data-stu-id="cba9e-158">Encryption process</span></span>
<span data-ttu-id="cba9e-159">Šifrování disku spoléhá na hello následující další součásti:</span><span class="sxs-lookup"><span data-stu-id="cba9e-159">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="cba9e-160">**Azure Key Vault** -použít toosafeguard kryptografické klíče a tajné klíče používané pro proces šifrování/dešifrování hello disku.</span><span class="sxs-lookup"><span data-stu-id="cba9e-160">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span>
  * <span data-ttu-id="cba9e-161">Pokud ano, můžete použít existující Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="cba9e-162">Nemáte toodedicate disky tooencrypting Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-162">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="cba9e-163">hranicemi správy tooseparate a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-163">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="cba9e-164">**Azure Active Directory** – obslužné rutiny hello zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.</span><span class="sxs-lookup"><span data-stu-id="cba9e-164">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="cba9e-165">Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cba9e-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="cba9e-166">Hello instanční objekt poskytuje zabezpečené mechanismus toorequest a vydávají hello odpovídající kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="cba9e-166">hello service principal provides a secure mechanism toorequest and be issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="cba9e-167">Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cba9e-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="cba9e-168">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="cba9e-168">Requirements and limitations</span></span>
<span data-ttu-id="cba9e-169">Podporované scénáře a požadavky na šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="cba9e-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="cba9e-170">Hello následující server Linux SKU - Ubuntu, CentOS, SUSE a SUSE Linux Enterprise Server (SLES) a Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="cba9e-170">hello following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="cba9e-171">Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být v hello stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="cba9e-171">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="cba9e-172">Standardní A, D, DS, G a GS řada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cba9e-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="cba9e-173">Šifrování disku aktuálně nepodporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="cba9e-173">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="cba9e-174">Úroveň Basic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cba9e-174">Basic tier VMs.</span></span>
* <span data-ttu-id="cba9e-175">Virtuální počítače vytvořené pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="cba9e-175">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="cba9e-176">Zakázáním šifrování disku operačního systému na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="cba9e-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="cba9e-177">Aktualizace hello kryptografické klíče na virtuální počítač již šifrované Linux.</span><span class="sxs-lookup"><span data-stu-id="cba9e-177">Updating hello cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="cba9e-178">Vytvoření Azure Key Vault a klíče</span><span class="sxs-lookup"><span data-stu-id="cba9e-178">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="cba9e-179">Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cba9e-179">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="cba9e-180">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="cba9e-180">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cba9e-181">Zahrnout názvy parametrů příklad *myResourceGroup*, *myKey*, a *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="cba9e-181">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="cba9e-182">prvním krokem Hello je toocreate Azure Key Vault toostore kryptografických klíčů.</span><span class="sxs-lookup"><span data-stu-id="cba9e-182">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="cba9e-183">Azure Key Vault můžete ukládat klíče, tajné klíče, nebo jejich hesla, které vám umožňují toosecurely implementaci v vašim aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="cba9e-183">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="cba9e-184">Šifrování virtuálního disku použijte Key Vault toostore kryptografický klíč, který je použité tooencrypt nebo dešifrovat virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="cba9e-184">For virtual disk encryption, you use Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="cba9e-185">Povolit hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [registrace zprostředkovatele az](/cli/azure/provider#register) a vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="cba9e-185">Enable hello Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="cba9e-186">Hello následující příklad vytvoří název skupiny prostředků *myResourceGroup* v hello `eastus` umístění:</span><span class="sxs-lookup"><span data-stu-id="cba9e-186">hello following example creates a resource group name *myResourceGroup* in hello `eastus` location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="cba9e-187">Hello Azure Key Vault obsahující hello kryptografické klíče a přidružených výpočetních, které prostředky, jako je například úložiště a hello virtuální počítač se musí nacházet v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="cba9e-187">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="cba9e-188">Vytvoření Azure Key Vault s [vytvořit az keyvault](/cli/azure/keyvault#create) a povolte hello Key Vault pro použití s šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="cba9e-188">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="cba9e-189">Zadejte jedinečný název pro Key Vault *keyvault_name* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cba9e-189">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="cba9e-190">Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="cba9e-190">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="cba9e-191">Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-191">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="cba9e-192">Není další náklady toocreating premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem.</span><span class="sxs-lookup"><span data-stu-id="cba9e-192">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="cba9e-193">Přidat premium Key Vault, v předchozím kroku hello toocreate `--sku Premium` toohello příkaz.</span><span class="sxs-lookup"><span data-stu-id="cba9e-193">toocreate a premium Key Vault, in hello preceding step add `--sku Premium` toohello command.</span></span> <span data-ttu-id="cba9e-194">Hello následující příklad používá klíče chráněné softwarem vzhledem k tomu, že jsme vytvořili standardní Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-194">hello following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="cba9e-195">U obou modelů ochranu se musí hello platformy Azure toobe udělit přístup toorequest hello kryptografické klíče, když hello virtuální počítač spustí toodecrypt hello virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="cba9e-195">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="cba9e-196">Vytvoření kryptografické klíče v Key Vault s [vytvořit klíč keyvault az](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="cba9e-196">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="cba9e-197">Hello následující příklad vytvoří klíč s názvem *myKey*:</span><span class="sxs-lookup"><span data-stu-id="cba9e-197">hello following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-hello-azure-active-directory-service-principal"></a><span data-ttu-id="cba9e-198">Vytvořit objekt služby Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="cba9e-198">Create hello Azure Active Directory service principal</span></span>
<span data-ttu-id="cba9e-199">Když virtuální disky jsou zašifrovaná nebo dešifrovat, je třeba zadat ověření účtu toohandle hello a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cba9e-199">When virtual disks are encrypted or decrypted, you specify an account toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="cba9e-200">Tento účet objektu zabezpečení služby Azure Active Directory umožňuje hello platformy Azure toorequest odpovídající kryptografické klíče hello jménem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="cba9e-200">This account, an Azure Active Directory service principal, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="cba9e-201">Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cba9e-201">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="cba9e-202">Vytvořit objekt služby pomocí služby Azure Active Directory s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="cba9e-202">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="cba9e-203">Následující ukázka Hello čtení hodnoty hello hello instanční objekt Id a heslo pro použití v novější příkazy:</span><span class="sxs-lookup"><span data-stu-id="cba9e-203">hello following example reads in hello values for hello service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="cba9e-204">Hello hesla se zobrazí pouze při vytváření objektu služby hello.</span><span class="sxs-lookup"><span data-stu-id="cba9e-204">hello password is only displayed when you create hello service principal.</span></span> <span data-ttu-id="cba9e-205">V případě potřeby, zobrazení a záznam hello heslo (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="cba9e-205">If desired, view and record hello password (`echo $sp_password`).</span></span> <span data-ttu-id="cba9e-206">Můžete vytvořit seznam hlavních objektů s několika vaší služby [az ad sp seznamu](/cli/azure/ad/sp#list) a zobrazit další informace o konkrétní objekt s služby [az ad sp zobrazit](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="cba9e-206">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="cba9e-207">toosuccessfully zašifrovat nebo dešifrovat virtuální disky, oprávnění hello kryptografického klíče uložené v Key Vault musí být sada toopermit hello Azure Active Directory service hlavní tooread hello klíče.</span><span class="sxs-lookup"><span data-stu-id="cba9e-207">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory service principal tooread hello keys.</span></span> <span data-ttu-id="cba9e-208">Nastavení oprávnění pro Key Vault s [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="cba9e-208">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="cba9e-209">V následujícím příkladu hello hello ID objektu zabezpečení služby je dodávána hello předcházející příkaz:</span><span class="sxs-lookup"><span data-stu-id="cba9e-209">In hello following example, hello service principal ID is supplied from hello preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a><span data-ttu-id="cba9e-210">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cba9e-210">Create virtual machine</span></span>
<span data-ttu-id="cba9e-211">tooactually šifrování některé virtuální disky, umožňuje vytvoření virtuálního počítače a přidat datový disk.</span><span class="sxs-lookup"><span data-stu-id="cba9e-211">tooactually encrypt some virtual disks, lets create a VM and add a data disk.</span></span> <span data-ttu-id="cba9e-212">Vytvoření virtuálního počítače tooencrypt s [vytvořit virtuální počítač az](/cli/azure/vm#create) a připojit datový disk 5 Gb.</span><span class="sxs-lookup"><span data-stu-id="cba9e-212">Create a VM tooencrypt with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="cba9e-213">Pouze některé bitové kopie marketplace podporují šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="cba9e-213">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="cba9e-214">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí **CentOS 7.2n** bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="cba9e-214">hello following example creates a VM named *myVM* using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="cba9e-215">SSH tooyour virtuální počítač pomocí hello `publicIpAddress` ukazuje výstup hello hello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="cba9e-215">SSH tooyour VM using hello `publicIpAddress` shown in hello output of hello preceding command.</span></span> <span data-ttu-id="cba9e-216">Vytvořit oddíl a systému souborů a pak připojit datový disk hello.</span><span class="sxs-lookup"><span data-stu-id="cba9e-216">Create a partition and filesystem, then mount hello data disk.</span></span> <span data-ttu-id="cba9e-217">Další informace najdete v tématu [připojit tooa virtuálního počítače s Linuxem toomount hello nový disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="cba9e-217">For more information, see [Connect tooa Linux VM toomount hello new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="cba9e-218">Ukončení relace SSH.</span><span class="sxs-lookup"><span data-stu-id="cba9e-218">Close your SSH session.</span></span>


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="cba9e-219">Šifrování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="cba9e-219">Encrypt virtual machine</span></span>
<span data-ttu-id="cba9e-220">tooencrypt hello virtuální disky, můžete seskupit všechny předchozí komponenty hello:</span><span class="sxs-lookup"><span data-stu-id="cba9e-220">tooencrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="cba9e-221">Zadejte hello objektu služby Azure Active Directory a heslo.</span><span class="sxs-lookup"><span data-stu-id="cba9e-221">Specify hello Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="cba9e-222">Zadejte hello Key Vault toostore hello metadata pro šifrované disky.</span><span class="sxs-lookup"><span data-stu-id="cba9e-222">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="cba9e-223">Zadejte hello toouse kryptografické klíče pro hello skutečné šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="cba9e-223">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="cba9e-224">Zadejte, zda chcete disk tooencrypt hello operačního systému, hello datových disků nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="cba9e-224">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="cba9e-225">Šifrování virtuálního počítače s [povolit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="cba9e-225">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="cba9e-226">Hello následující příklad používá hello `$sp_id` a `$sp_password` proměnné z předchozí hello `ad sp create-for-rbac` příkaz:</span><span class="sxs-lookup"><span data-stu-id="cba9e-226">hello following example uses hello `$sp_id` and `$sp_password` variables from hello preceding `ad sp create-for-rbac` command:</span></span>

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

<span data-ttu-id="cba9e-227">Toocomplete proces šifrování disku hello nějakou dobu trvá.</span><span class="sxs-lookup"><span data-stu-id="cba9e-227">It takes some time for hello disk encryption process toocomplete.</span></span> <span data-ttu-id="cba9e-228">Monitorování stavu hello hello proces s [zobrazit šifrování virtuálních počítačů az](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="cba9e-228">Monitor hello status of hello process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cba9e-229">Hello výstup je podobné toohello následující zkrácený ukázka:</span><span class="sxs-lookup"><span data-stu-id="cba9e-229">hello output is similar toohello following truncated example:</span></span>

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

<span data-ttu-id="cba9e-230">Počkat, až hello stav pro sestavy disku hello OS **VMRestartPending**, potom restartujte virtuální počítač s [restartování virtuálního počítače az](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="cba9e-230">Wait until hello status for hello OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cba9e-231">Hello proces šifrování disku je dokončen. během procesu spuštění hello, proto Počkejte několik minut před zaškrtnutím hello stav šifrování znovu s **zobrazit šifrování virtuálních počítačů az**:</span><span class="sxs-lookup"><span data-stu-id="cba9e-231">hello disk encryption process is finalized during hello boot process, so wait a few minutes before checking hello status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cba9e-232">Stav Hello by teď disku hello operačního systému i datový disk jako **šifrovaný**.</span><span class="sxs-lookup"><span data-stu-id="cba9e-232">hello status should now report both hello OS disk and data disk as **Encrypted**.</span></span>


## <a name="add-additional-data-disks"></a><span data-ttu-id="cba9e-233">Přidání dalších datových disků</span><span class="sxs-lookup"><span data-stu-id="cba9e-233">Add additional data disks</span></span>
<span data-ttu-id="cba9e-234">Jakmile jste zašifrovali datových disků, můžete později přidat tooyour další virtuální disky virtuálních počítačů a také zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="cba9e-234">Once you have encrypted your data disks, you can later add additional virtual disks tooyour VM and also encrypt them.</span></span> <span data-ttu-id="cba9e-235">Umožňuje například přidat druhý virtuální disk tooyour virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cba9e-235">For example, lets add a second virtual disk tooyour VM as follows:</span></span>

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

<span data-ttu-id="cba9e-236">Znovu spusťte hello příkaz tooencrypt hello virtuální disky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cba9e-236">Re-run hello command tooencrypt hello virtual disks as follows:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="cba9e-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cba9e-237">Next steps</span></span>
* <span data-ttu-id="cba9e-238">Další informace o správě Azure Key Vault, včetně odstraňování kryptografické klíče a trezory, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="cba9e-238">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="cba9e-239">Další informace o šifrování disku, například při přípravě šifrované vlastní virtuální počítač tooupload tooAzure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="cba9e-239">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
