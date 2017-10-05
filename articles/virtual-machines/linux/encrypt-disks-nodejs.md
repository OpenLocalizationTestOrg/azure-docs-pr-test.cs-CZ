---
title: "Šifrování disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 | Microsoft Docs"
description: "Postup zašifrování disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 a modelu nasazení Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: b436f2d43c41000f4385889edb3fa3983d4a8c66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli-10"></a><span data-ttu-id="1e0ca-103">Šifrování disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1e0ca-103">Encrypt disks on a Linux VM using the Azure CLI 1.0</span></span>
<span data-ttu-id="1e0ca-104">Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted at rest.</span></span> <span data-ttu-id="1e0ca-105">Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="1e0ca-106">Řízení těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="1e0ca-107">Tento článek podrobně popisují zašifrovat virtuální disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 a modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-107">This article details how to encrypt virtual disks on a Linux VM using the Azure CLI 1.0 and the Resource Manager deployment model.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="1e0ca-108">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="1e0ca-108">CLI versions to complete the task</span></span>
<span data-ttu-id="1e0ca-109">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-109">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="1e0ca-110">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="1e0ca-110">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="1e0ca-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="1e0ca-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="1e0ca-112">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="1e0ca-112">Quick commands</span></span>
<span data-ttu-id="1e0ca-113">Pokud potřebujete rychle provedení úlohy, následující část podrobně popisuje základní příkazy pro zašifrování virtuální disky na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-113">If you need to quickly accomplish the task, the following section details the base commands to encrypt virtual disks on your VM.</span></span> <span data-ttu-id="1e0ca-114">Podrobnější informace a kontext pro každý krok naleznete zbývající části dokumentu, [od zde](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="1e0ca-114">More detailed information and context for each step can be found the rest of the document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="1e0ca-115">Je nutné [nejnovější Azure CLI 1.0](../../xplat-cli-install.md) nainstalován a přihlášení pomocí režimu Resource Manager takto:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-115">You need the [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="1e0ca-116">V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-116">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="1e0ca-117">Zahrnout názvy parametrů příklad `myResourceGroup`, `myKeyVault`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-117">Example parameter names include `myResourceGroup`, `myKeyVault`, and `myVM`.</span></span>

<span data-ttu-id="1e0ca-118">Nejprve povolte Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure a vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-118">First, enable the Azure Key Vault provider within your Azure subscription and create a resource group.</span></span> <span data-ttu-id="1e0ca-119">Následující příklad vytvoří název skupiny prostředků `myResourceGroup` v `WestUS` umístění:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-119">The following example creates a resource group name `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="1e0ca-120">Vytvoření Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-120">Create an Azure Key Vault.</span></span> <span data-ttu-id="1e0ca-121">Následující příklad vytvoří Key Vault s názvem `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-121">The following example creates a Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="1e0ca-122">Vytvoření kryptografické klíče v Key Vault a povolit šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-122">Create a cryptographic key in your Key Vault and enable it for disk encryption.</span></span> <span data-ttu-id="1e0ca-123">Následující příklad vytvoří klíč s názvem `myKey`:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-123">The following example creates a key named `myKey`:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

<span data-ttu-id="1e0ca-124">Vytvořte koncový bod pomocí Azure Active Directory pro zpracování ověření a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-124">Create an endpoint using Azure Active Directory for handling the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="1e0ca-125">`--home-page` a `--identifier-uris` nemusí být skutečné směrovatelné adresa.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-125">The `--home-page` and `--identifier-uris` do not need to be actual routable address.</span></span> <span data-ttu-id="1e0ca-126">Pro nejvyšší úroveň zabezpečení je třeba použít klienta tajné klíče místo hesla.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-126">For the highest level of security, client secrets should be used instead of passwords.</span></span> <span data-ttu-id="1e0ca-127">Rozhraní příkazového řádku Azure nelze vygenerovat aktuálně tajné klíče klienta.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-127">The Azure CLI cannot currently generate client secrets.</span></span> <span data-ttu-id="1e0ca-128">Tajné klíče klienta lze vygenerovat pouze na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-128">Client secrets can only be generated in the Azure portal.</span></span> <span data-ttu-id="1e0ca-129">Následující příklad vytvoří koncový bod služby Azure Active Directory s názvem `myAADApp` a používá na heslo `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-129">The following example creates an Azure Active Directory endpoint named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="1e0ca-130">Zadejte své heslo následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-130">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="1e0ca-131">Poznámka: `applicationId` zobrazené ve výstupu z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-131">Note the `applicationId` shown in the output from the preceding command.</span></span> <span data-ttu-id="1e0ca-132">Toto ID aplikace se používá v následujících krocích:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-132">This application ID is used in the following steps:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

<span data-ttu-id="1e0ca-133">Přidáte datový disk do stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-133">Add a data disk to an existing VM.</span></span> <span data-ttu-id="1e0ca-134">Následující příklad přidá datový disk k virtuálnímu počítači s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-134">The following example adds a data disk to a VM named `myVM`:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="1e0ca-135">Podrobné informace pro Key Vault a klíče, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-135">Review the details for your Key Vault and the key you created.</span></span> <span data-ttu-id="1e0ca-136">Je třeba ID klíč trezoru, identifikátor URI a klíč adresy URL v posledním kroku.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-136">You need the Key Vault ID, URI, and key URL in the final step.</span></span> <span data-ttu-id="1e0ca-137">Následující příklad zkontroluje podrobnosti pro Key Vault s názvem `myKeyVault` a klíč s názvem `myKey`:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-137">The following example reviews the details for a Key Vault named `myKeyVault` and key named `myKey`:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="1e0ca-138">Šifrování vaše disky následujícím způsobem zadávání vlastní názvy parametrů v průběhu:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-138">Encrypt your disks as follows, entering your own parameter names throughout:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="1e0ca-139">Rozhraní příkazového řádku Azure neposkytuje podrobné chyby během procesu šifrování.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-139">The Azure CLI doesn't provide verbose errors during the encryption process.</span></span> <span data-ttu-id="1e0ca-140">Další informace o řešení potíží, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-140">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span></span> <span data-ttu-id="1e0ca-141">Předchozí příkaz má mnoho proměnné a nelze získat množství informací, proč proces selže, například dokončení příkazu vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-141">As the preceding command has many variables and you may not get much indication as to why the process fails, a complete command example would be as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

<span data-ttu-id="1e0ca-142">Nakonec zkontrolujte stav šifrování znovu pro potvrzení, že virtuální disky mají nyní šifrována.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-142">Finally, review the encryption status again to confirm that your virtual disks have now been encrypted.</span></span> <span data-ttu-id="1e0ca-143">Následující příklad ověří stav virtuálního počítače s názvem `myVM` v `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-143">The following example checks the status of a VM named `myVM` in the `myResourceGroup` resource group:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="1e0ca-144">Přehled šifrování disku</span><span class="sxs-lookup"><span data-stu-id="1e0ca-144">Overview of disk encryption</span></span>
<span data-ttu-id="1e0ca-145">Virtuální disky na virtuální počítače s Linuxem se šifrují pomocí rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="1e0ca-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="1e0ca-146">Není nijak zpoplatněn pro šifrování virtuálních disků v Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="1e0ca-147">Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikovány pro FIPS 140-2 úroveň 2 standardů.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="1e0ca-148">Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="1e0ca-149">Tyto klíče se používají k šifrování a dešifrování virtuálních disků připojených k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-149">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="1e0ca-150">Koncový bod služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-150">An Azure Active Directory endpoint provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="1e0ca-151">Proces šifrování virtuálního počítače je následující:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-151">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="1e0ca-152">Vytvoření kryptografické klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="1e0ca-153">Nakonfigurujte kryptografický klíč možné používat pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-153">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="1e0ca-154">Čtení kryptografického klíče z Azure Key Vault, vytvořte koncový bod pomocí služby Azure Active Directory s příslušnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-154">To read the cryptographic key from the Azure Key Vault, create an endpoint using Azure Active Directory with the appropriate permissions.</span></span>
4. <span data-ttu-id="1e0ca-155">Příkaz pro zašifrování virtuální disky, koncový bod služby Azure Active Directory a příslušné kryptografický klíč, který se má použít.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-155">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory endpoint and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="1e0ca-156">Koncový bod služby Azure Active Directory vyžaduje požadované kryptografický klíč z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-156">The Azure Active Directory endpoint requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="1e0ca-157">Virtuální disky jsou šifrované pomocí zadaný kryptografický klíč.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-157">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="supporting-services-and-encryption-process"></a><span data-ttu-id="1e0ca-158">Podpora služby a proces šifrování</span><span class="sxs-lookup"><span data-stu-id="1e0ca-158">Supporting services and encryption process</span></span>
<span data-ttu-id="1e0ca-159">Šifrování disku spoléhá na následující součásti:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-159">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="1e0ca-160">**Azure Key Vault** – používané k ochraně kryptografické klíče a tajné klíče pro proces šifrování a dešifrování disku.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-160">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span>
  * <span data-ttu-id="1e0ca-161">Pokud ano, můžete použít existující Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="1e0ca-162">Není nutné vyhradit Key Vault pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-162">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="1e0ca-163">K oddělení hranicemi správy a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-163">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="1e0ca-164">**Azure Active Directory** -zpracovává zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-164">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="1e0ca-165">Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="1e0ca-166">Aplikace je více koncový bod pro služby Key Vault a virtuální počítač k vyžádání a získání vydané příslušnou kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-166">The application is more of an endpoint for the Key Vault and Virtual Machine services to request and get issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="1e0ca-167">Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="1e0ca-168">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="1e0ca-168">Requirements and limitations</span></span>
<span data-ttu-id="1e0ca-169">Podporované scénáře a požadavky na šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="1e0ca-170">Následující server Linux SKU - Ubuntu, CentOS, SUSE a SUSE Linux Enterprise Server (SLES) a Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-170">The following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="1e0ca-171">Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být ve stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-171">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="1e0ca-172">Standardní A, D, DS, G a GS řada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="1e0ca-173">Šifrování disku není aktuálně podporováno v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-173">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="1e0ca-174">Úroveň Basic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-174">Basic tier VMs.</span></span>
* <span data-ttu-id="1e0ca-175">Virtuální počítače vytvořené pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-175">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="1e0ca-176">Zakázáním šifrování disku operačního systému na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="1e0ca-177">Aktualizuje se kryptografické klíče na virtuální počítač již šifrované Linux.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-177">Updating the cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-the-azure-key-vault-and-keys"></a><span data-ttu-id="1e0ca-178">Vytvoření Azure Key Vault a klíče</span><span class="sxs-lookup"><span data-stu-id="1e0ca-178">Create the Azure Key Vault and keys</span></span>
<span data-ttu-id="1e0ca-179">K dokončení zbytek tohoto průvodce, musíte [nejnovější Azure CLI 1.0](../../xplat-cli-install.md) nainstalován a přihlášení pomocí režimu Resource Manager takto:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-179">To complete the remainder of this guide, you need the [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="1e0ca-180">V příkladech nahraďte všechny parametry příklad vlastní názvy, umístění a hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-180">Throughout the command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="1e0ca-181">Následující příklady použití konvence `myResourceGroup`, `myKeyVault`, `myAADApp`atd.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-181">The following examples use a convention of `myResourceGroup`, `myKeyVault`, `myAADApp`, etc.</span></span>

<span data-ttu-id="1e0ca-182">Prvním krokem je vytvoření Azure Key Vault pro ukládání kryptografických klíčů.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-182">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="1e0ca-183">Azure Key Vault můžete uložit klíčů, tajné klíče nebo hesla, které vám umožní bezpečně je implementovat v vašim aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-183">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="1e0ca-184">Šifrování virtuálního disku použijete k uložení kryptografického klíče, který se používá k šifrování a dešifrování virtuální disky Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-184">For virtual disk encryption, you use Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="1e0ca-185">Povolit zprostředkovatele Azure Key Vault ve vašem předplatném Azure a pak vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-185">Enable the Azure Key Vault provider in your Azure subscription, then create a resource group.</span></span> <span data-ttu-id="1e0ca-186">Následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v `WestUS` umístění:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-186">The following example creates a resource group named `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="1e0ca-187">Azure Key Vault, který obsahuje kryptografické klíče a přidružené výpočetní prostředky, jako je například úložiště a virtuální počítač se musí nacházet ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-187">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="1e0ca-188">Následující příklad vytvoří Azure Key Vault s názvem `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-188">The following example creates an Azure Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="1e0ca-189">Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="1e0ca-189">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="1e0ca-190">Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-190">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="1e0ca-191">Není dalších nákladů na vytváření premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-191">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="1e0ca-192">Chcete-li vytvořit premium Key Vault, v předchozím kroku přidejte `--sku Premium` k příkazu.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-192">To create a premium Key Vault, in the preceding step add `--sku Premium` to the command.</span></span> <span data-ttu-id="1e0ca-193">Následující příklad používá klíče chráněné softwarem, protože jsme vytvořili standardní Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-193">The following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="1e0ca-194">Pro oba modely ochrany musí mít udělen přístup k žádosti o kryptografických klíčů, když se virtuální počítač spustí do dešifrovat virtuální disky platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-194">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="1e0ca-195">Vytvořit šifrovací klíč v Key Vault a pak ji povolit pro použití s šifrováním virtuálního disku.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-195">Create an encryption key within your Key Vault, then enable it for use with virtual disk encryption.</span></span> <span data-ttu-id="1e0ca-196">Následující příklad vytvoří klíč s názvem `myKey` a povolí ho šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-196">The following example creates a key named `myKey` and then enables it for disk encryption:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a><span data-ttu-id="1e0ca-197">Vytvoření aplikace Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1e0ca-197">Create the Azure Active Directory application</span></span>
<span data-ttu-id="1e0ca-198">Když virtuální disky jsou zašifrovaná nebo dešifrovat, použijete koncový bod pro zpracování ověření a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-198">When virtual disks are encrypted or decrypted, you use an endpoint to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="1e0ca-199">Tento koncový bod, aplikaci Azure Active Directory umožňuje platformy Azure k vyžádání příslušné kryptografické klíče jménem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-199">This endpoint, an Azure Active Directory application, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="1e0ca-200">Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-200">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="1e0ca-201">Jako nejsou vytvoření úplné aplikace Azure Active Directory, `--home-page` a `--identifier-uris` parametry v následujícím příkladu nemusíte být skutečné směrovatelné adresa.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-201">As you are not creating a full Azure Active Directory application, the `--home-page` and `--identifier-uris` parameters in the following example do not need to be actual routable address.</span></span> <span data-ttu-id="1e0ca-202">Následující příklad určuje také tajného klíče založené na heslech, nikoli generování klíčů z v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-202">The following example also specifies a password-based secret rather than generating keys from within the Azure portal.</span></span> <span data-ttu-id="1e0ca-203">Jako tento čas generování klíčů nelze provést z příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-203">As this time, generating keys cannot be done from the Azure CLI.</span></span>

<span data-ttu-id="1e0ca-204">Vytvoření aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-204">Create your Azure Active Directory application.</span></span> <span data-ttu-id="1e0ca-205">Následující příklad vytvoří aplikaci s názvem `myAADApp` a používá na heslo `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-205">The following example creates an application named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="1e0ca-206">Zadejte své heslo následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-206">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="1e0ca-207">Poznamenejte si `applicationId` , je vrácen ve výstupu z předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-207">Make a note of the `applicationId` that is returned in the output from the preceding command.</span></span> <span data-ttu-id="1e0ca-208">Toto ID aplikace se používá v některé z zbývající kroky.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-208">This application ID is used in some of the remaining steps.</span></span> <span data-ttu-id="1e0ca-209">Dále vytvořte hlavní název služby (SPN) tak, aby aplikace je přístupná ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-209">Next, create a service principal name (SPN) so that the application is accessible within your environment.</span></span> <span data-ttu-id="1e0ca-210">Pro úspěšně šifrování nebo dešifrování virtuálních disků, musí se nastavit oprávnění na kryptografický klíč uložený v Key Vault tak, aby povolovala aplikaci Azure Active Directory číst klíče.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-210">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory application to read the keys.</span></span>

<span data-ttu-id="1e0ca-211">Vytvořit název SPN a nastavte příslušná oprávnění následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-211">Create the SPN and set the appropriate permissions as follows:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a><span data-ttu-id="1e0ca-212">Přidat virtuální disk a zkontrolovat stav šifrování</span><span class="sxs-lookup"><span data-stu-id="1e0ca-212">Add a virtual disk and review encryption status</span></span>
<span data-ttu-id="1e0ca-213">Ve skutečnosti šifrování některé virtuální disky, umožňuje přidat disk do stávajícího virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-213">To actually encrypt some virtual disks, lets add a disk to an existing VM.</span></span> <span data-ttu-id="1e0ca-214">Přidáte datový disk 5Gb na existující virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-214">Add a 5Gb data disk to an existing VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="1e0ca-215">Aktuálně nejsou šifrovány virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-215">The virtual disks are not currently encrypted.</span></span> <span data-ttu-id="1e0ca-216">Zkontrolujte aktuální stav šifrování virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-216">Review the current encryption status of your VM as follows:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a><span data-ttu-id="1e0ca-217">Šifrování virtuálních disků</span><span class="sxs-lookup"><span data-stu-id="1e0ca-217">Encrypt virtual disks</span></span>
<span data-ttu-id="1e0ca-218">Pokud chcete nyní zašifrovat virtuální disky, přepnutím společně předchozí komponenty:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-218">To now encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="1e0ca-219">Zadejte aplikaci Azure Active Directory a heslo.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-219">Specify the Azure Active Directory application and password.</span></span>
2. <span data-ttu-id="1e0ca-220">Zadejte klíč trezoru k ukládání metadat pro šifrované disky.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-220">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="1e0ca-221">Zadejte kryptografické klíče k použití pro skutečné šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-221">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="1e0ca-222">Zadejte, jestli chcete šifrovat disk operačního systému, datových disků nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-222">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="1e0ca-223">Umožňuje, zkontrolujte podrobnosti pro Azure Key Vault a klíče, který jste vytvořili, potřebujete ID klíč trezoru, identifikátor URI, a potom klíče adresy URL v posledním kroku:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-223">Lets review the details for your Azure Key Vault and the key you created, as you need the Key Vault ID, URI, and then key URL in the final step:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="1e0ca-224">Zašifrovat virtuální disky pomocí výstup `azure keyvault show` a `azure keyvault key show` příkazy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-224">Encrypt your virtual disks using the output from the `azure keyvault show` and `azure keyvault key show` commands as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="1e0ca-225">Předchozí příkaz má mnoho proměnné, je v následujícím příkladu příkaz dokončení pro referenci:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-225">As the preceding command has many variables, the following example is the complete command for reference:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

<span data-ttu-id="1e0ca-226">Rozhraní příkazového řádku Azure neposkytuje podrobné chyby během procesu šifrování.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-226">The Azure CLI doesn't provide verbose errors during the encryption process.</span></span> <span data-ttu-id="1e0ca-227">Další informace o řešení potíží, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` ve virtuálním počítači se šifrují.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-227">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` on the VM you are encrypting.</span></span>

<span data-ttu-id="1e0ca-228">Nakonec umožňuje zkontrolovat stav šifrování znovu pro potvrzení, že mít teď šifrované virtuální disky:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-228">Finally, lets review the encryption status again to confirm that your virtual disks have now been encrypted:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a><span data-ttu-id="1e0ca-229">Přidání dalších datových disků</span><span class="sxs-lookup"><span data-stu-id="1e0ca-229">Add additional data disks</span></span>
<span data-ttu-id="1e0ca-230">Jakmile jste zašifrovali datových disků, můžete později přidat další virtuální disky pro virtuální počítač a také zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-230">Once you have encrypted your data disks, you can later add additional virtual disks to your VM and also encrypt them.</span></span> <span data-ttu-id="1e0ca-231">Při spuštění `azure vm enable-disk-encryption` příkaz, zvýšit pořadí verzi pomocí `--sequence-version` parametr.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-231">When you run the `azure vm enable-disk-encryption` command, increment the sequence version using the `--sequence-version` parameter.</span></span> <span data-ttu-id="1e0ca-232">Tento parametr pořadí verzi můžete k provedení operací s stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1e0ca-232">This sequence version parameter allows you to perform repeated operations on the same VM.</span></span>

<span data-ttu-id="1e0ca-233">Umožňuje například přidat druhý virtuální disk k virtuálnímu počítači následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-233">For example, lets add a second virtual disk to your VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="1e0ca-234">Spusťte znovu příkaz pro zašifrování virtuální disky, tento čas přidání `--sequence-version` parametr a přírůstkovou hodnotou z našich první spustit takto:</span><span class="sxs-lookup"><span data-stu-id="1e0ca-234">Rerun the command to encrypt the virtual disks, this time adding the `--sequence-version` parameter, and incrementing the value from our first run as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a><span data-ttu-id="1e0ca-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e0ca-235">Next steps</span></span>
* <span data-ttu-id="1e0ca-236">Další informace o správě Azure Key Vault, včetně odstraňování kryptografické klíče a trezory, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="1e0ca-236">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="1e0ca-237">Další informace o šifrování disku, jako je například příprava šifrované vlastních virtuálních počítačů se nahrát do Azure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="1e0ca-237">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
