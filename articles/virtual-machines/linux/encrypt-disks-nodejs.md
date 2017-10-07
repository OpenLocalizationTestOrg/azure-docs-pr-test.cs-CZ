---
title: "aaaEncrypt disky na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Jak hello tooencrypt disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 a modelu nasazení Resource Manager hello"
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
ms.openlocfilehash: 68a0394d366c3c6941e2c6db0d4263123f951946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-hello-azure-cli-10"></a><span data-ttu-id="36d5a-103">Šifrování disky na virtuální počítač s Linuxem pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="36d5a-103">Encrypt disks on a Linux VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="36d5a-104">Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="36d5a-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted at rest.</span></span> <span data-ttu-id="36d5a-105">Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="36d5a-106">Řízení těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="36d5a-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="36d5a-107">Tento článek podrobně popisuje, jak hello tooencrypt virtuální disky na virtuální počítač s Linuxem pomocí Azure CLI 1.0 a modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="36d5a-107">This article details how tooencrypt virtual disks on a Linux VM using hello Azure CLI 1.0 and hello Resource Manager deployment model.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="36d5a-108">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="36d5a-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="36d5a-109">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="36d5a-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="36d5a-110">[Azure CLI 1.0](#quick-commands) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="36d5a-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="36d5a-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="36d5a-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="36d5a-112">Rychlé příkazy</span><span class="sxs-lookup"><span data-stu-id="36d5a-112">Quick commands</span></span>
<span data-ttu-id="36d5a-113">Pokud je třeba tooquickly dosáhnout hello, hello následující části Podrobnosti hello základní příkazy tooencrypt virtuální disky na virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="36d5a-113">If you need tooquickly accomplish hello task, hello following section details hello base commands tooencrypt virtual disks on your VM.</span></span> <span data-ttu-id="36d5a-114">Podrobnější informace a kontext pro každý krok naleznete hello zbytek dokumentu hello [od zde](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="36d5a-114">More detailed information and context for each step can be found hello rest of hello document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="36d5a-115">Je třeba hello [nejnovější Azure CLI 1.0](../../xplat-cli-install.md) nainstalován a přihlášení pomocí režimu Resource Manager hello takto:</span><span class="sxs-lookup"><span data-stu-id="36d5a-115">You need hello [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="36d5a-116">Následující příklady, v hello nahraďte názvy parametrů příklad vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="36d5a-116">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="36d5a-117">Zahrnout názvy parametrů příklad `myResourceGroup`, `myKeyVault`, a `myVM`.</span><span class="sxs-lookup"><span data-stu-id="36d5a-117">Example parameter names include `myResourceGroup`, `myKeyVault`, and `myVM`.</span></span>

<span data-ttu-id="36d5a-118">Nejprve povolte hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure a vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="36d5a-118">First, enable hello Azure Key Vault provider within your Azure subscription and create a resource group.</span></span> <span data-ttu-id="36d5a-119">Hello následující příklad vytvoří název skupiny prostředků `myResourceGroup` v hello `WestUS` umístění:</span><span class="sxs-lookup"><span data-stu-id="36d5a-119">hello following example creates a resource group name `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="36d5a-120">Vytvoření Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-120">Create an Azure Key Vault.</span></span> <span data-ttu-id="36d5a-121">Hello následující příklad vytvoří Key Vault s názvem `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="36d5a-121">hello following example creates a Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="36d5a-122">Vytvoření kryptografické klíče v Key Vault a povolit šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="36d5a-122">Create a cryptographic key in your Key Vault and enable it for disk encryption.</span></span> <span data-ttu-id="36d5a-123">Hello následující příklad vytvoří klíč s názvem `myKey`:</span><span class="sxs-lookup"><span data-stu-id="36d5a-123">hello following example creates a key named `myKey`:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

<span data-ttu-id="36d5a-124">Vytvořte koncový bod pomocí Azure Active Directory pro zpracování hello ověřování a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-124">Create an endpoint using Azure Active Directory for handling hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="36d5a-125">Hello `--home-page` a `--identifier-uris` nemusí toobe skutečné směrovatelné adresu.</span><span class="sxs-lookup"><span data-stu-id="36d5a-125">hello `--home-page` and `--identifier-uris` do not need toobe actual routable address.</span></span> <span data-ttu-id="36d5a-126">Hello nejvyšší úroveň zabezpečení je třeba použít klienta tajné klíče místo hesla.</span><span class="sxs-lookup"><span data-stu-id="36d5a-126">For hello highest level of security, client secrets should be used instead of passwords.</span></span> <span data-ttu-id="36d5a-127">Hello rozhraní příkazového řádku Azure nelze vygenerovat aktuálně tajné klíče klienta.</span><span class="sxs-lookup"><span data-stu-id="36d5a-127">hello Azure CLI cannot currently generate client secrets.</span></span> <span data-ttu-id="36d5a-128">Tajné klíče klienta může být generována pouze v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36d5a-128">Client secrets can only be generated in hello Azure portal.</span></span> <span data-ttu-id="36d5a-129">Hello následující příklad vytvoří koncový bod služby Azure Active Directory s názvem `myAADApp` a používá na heslo `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="36d5a-129">hello following example creates an Azure Active Directory endpoint named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="36d5a-130">Zadejte své heslo následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36d5a-130">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="36d5a-131">Poznámka: hello `applicationId` ukazuje výstup hello hello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="36d5a-131">Note hello `applicationId` shown in hello output from hello preceding command.</span></span> <span data-ttu-id="36d5a-132">Toto ID aplikace se používá v hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="36d5a-132">This application ID is used in hello following steps:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

<span data-ttu-id="36d5a-133">Přidáte datového disku tooan existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="36d5a-133">Add a data disk tooan existing VM.</span></span> <span data-ttu-id="36d5a-134">Hello následující příklad přidá disk tooa data virtuálního počítače s názvem `myVM`:</span><span class="sxs-lookup"><span data-stu-id="36d5a-134">hello following example adds a data disk tooa VM named `myVM`:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="36d5a-135">Zkontrolujte podrobnosti hello Key Vault a hello klíče, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="36d5a-135">Review hello details for your Key Vault and hello key you created.</span></span> <span data-ttu-id="36d5a-136">Třeba hello ID klíč trezoru, identifikátor URI a klíč adresu URL v posledním kroku hello.</span><span class="sxs-lookup"><span data-stu-id="36d5a-136">You need hello Key Vault ID, URI, and key URL in hello final step.</span></span> <span data-ttu-id="36d5a-137">Hello následující příklad zkontroluje hello podrobnosti pro Key Vault s názvem `myKeyVault` a klíč s názvem `myKey`:</span><span class="sxs-lookup"><span data-stu-id="36d5a-137">hello following example reviews hello details for a Key Vault named `myKeyVault` and key named `myKey`:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="36d5a-138">Šifrování vaše disky následujícím způsobem zadávání vlastní názvy parametrů v průběhu:</span><span class="sxs-lookup"><span data-stu-id="36d5a-138">Encrypt your disks as follows, entering your own parameter names throughout:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="36d5a-139">Hello rozhraní příkazového řádku Azure neposkytuje podrobné chyby během procesu šifrování hello.</span><span class="sxs-lookup"><span data-stu-id="36d5a-139">hello Azure CLI doesn't provide verbose errors during hello encryption process.</span></span> <span data-ttu-id="36d5a-140">Další informace o řešení potíží, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span><span class="sxs-lookup"><span data-stu-id="36d5a-140">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span></span> <span data-ttu-id="36d5a-141">Jako hello předcházející příkaz obsahuje mnoho proměnné a nelze získat mnohem indikace jako toowhy hello proces selže, například dokončení příkazu vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="36d5a-141">As hello preceding command has many variables and you may not get much indication as toowhy hello process fails, a complete command example would be as follows:</span></span>

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

<span data-ttu-id="36d5a-142">Nakonec zkontrolujte stav šifrování hello znovu tooconfirm, že virtuální disky mají nyní šifrována.</span><span class="sxs-lookup"><span data-stu-id="36d5a-142">Finally, review hello encryption status again tooconfirm that your virtual disks have now been encrypted.</span></span> <span data-ttu-id="36d5a-143">Hello následující příklad ověří hello stav virtuálního počítače s názvem `myVM` v hello `myResourceGroup` skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="36d5a-143">hello following example checks hello status of a VM named `myVM` in hello `myResourceGroup` resource group:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="36d5a-144">Přehled šifrování disku</span><span class="sxs-lookup"><span data-stu-id="36d5a-144">Overview of disk encryption</span></span>
<span data-ttu-id="36d5a-145">Virtuální disky na virtuální počítače s Linuxem se šifrují pomocí rest [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="36d5a-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="36d5a-146">Není nijak zpoplatněn pro šifrování virtuálních disků v Azure.</span><span class="sxs-lookup"><span data-stu-id="36d5a-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="36d5a-147">Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikované tooFIPS standardy úroveň 2 140-2.</span><span class="sxs-lookup"><span data-stu-id="36d5a-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="36d5a-148">Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="36d5a-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="36d5a-149">Tyto kryptografické klíče jsou použité tooencrypt a dešifrování tooyour připojené virtuální disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36d5a-149">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="36d5a-150">Koncový bod služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="36d5a-150">An Azure Active Directory endpoint provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="36d5a-151">Hello proces šifrování virtuálního počítače je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36d5a-151">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="36d5a-152">Vytvoření kryptografické klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="36d5a-153">Nakonfigurujte hello kryptografické klíče toobe použitelné pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="36d5a-153">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="36d5a-154">tooread hello kryptografický klíč z hello Azure Key Vault, vytvořit koncový bod pomocí služby Azure Active Directory s příslušnými oprávněními hello.</span><span class="sxs-lookup"><span data-stu-id="36d5a-154">tooread hello cryptographic key from hello Azure Key Vault, create an endpoint using Azure Active Directory with hello appropriate permissions.</span></span>
4. <span data-ttu-id="36d5a-155">Vydejte příkaz tooencrypt hello virtuální disky, zadání koncový bod služby Azure Active Directory hello a příslušné kryptografické klíče toobe použít.</span><span class="sxs-lookup"><span data-stu-id="36d5a-155">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory endpoint and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="36d5a-156">koncový bod služby Azure Active Directory Hello požadavků hello požadované kryptografický klíč z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-156">hello Azure Active Directory endpoint requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="36d5a-157">Hello virtuální disky jsou šifrované pomocí hello zadaný kryptografický klíč.</span><span class="sxs-lookup"><span data-stu-id="36d5a-157">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="supporting-services-and-encryption-process"></a><span data-ttu-id="36d5a-158">Podpora služby a proces šifrování</span><span class="sxs-lookup"><span data-stu-id="36d5a-158">Supporting services and encryption process</span></span>
<span data-ttu-id="36d5a-159">Šifrování disku spoléhá na hello následující další součásti:</span><span class="sxs-lookup"><span data-stu-id="36d5a-159">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="36d5a-160">**Azure Key Vault** -použít toosafeguard kryptografické klíče a tajné klíče používané pro proces šifrování/dešifrování hello disku.</span><span class="sxs-lookup"><span data-stu-id="36d5a-160">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span>
  * <span data-ttu-id="36d5a-161">Pokud ano, můžete použít existující Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="36d5a-162">Nemáte toodedicate disky tooencrypting Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-162">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="36d5a-163">hranicemi správy tooseparate a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-163">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="36d5a-164">**Azure Active Directory** – obslužné rutiny hello zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.</span><span class="sxs-lookup"><span data-stu-id="36d5a-164">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="36d5a-165">Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="36d5a-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="36d5a-166">aplikace Hello koncový bod pro hello Key Vault a virtuální počítač služby toorequest a získat vydané hello odpovídající kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="36d5a-166">hello application is more of an endpoint for hello Key Vault and Virtual Machine services toorequest and get issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="36d5a-167">Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="36d5a-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="36d5a-168">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="36d5a-168">Requirements and limitations</span></span>
<span data-ttu-id="36d5a-169">Podporované scénáře a požadavky na šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="36d5a-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="36d5a-170">Hello následující server Linux SKU - Ubuntu, CentOS, SUSE a SUSE Linux Enterprise Server (SLES) a Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="36d5a-170">hello following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="36d5a-171">Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být v hello stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="36d5a-171">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="36d5a-172">Standardní A, D, DS, G a GS řada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36d5a-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="36d5a-173">Šifrování disku aktuálně nepodporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="36d5a-173">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="36d5a-174">Úroveň Basic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36d5a-174">Basic tier VMs.</span></span>
* <span data-ttu-id="36d5a-175">Virtuální počítače vytvořené pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="36d5a-175">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="36d5a-176">Zakázáním šifrování disku operačního systému na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="36d5a-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="36d5a-177">Aktualizace hello kryptografické klíče na virtuální počítač již šifrované Linux.</span><span class="sxs-lookup"><span data-stu-id="36d5a-177">Updating hello cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-hello-azure-key-vault-and-keys"></a><span data-ttu-id="36d5a-178">Vytvoření hello Azure Key Vault a klíče</span><span class="sxs-lookup"><span data-stu-id="36d5a-178">Create hello Azure Key Vault and keys</span></span>
<span data-ttu-id="36d5a-179">toocomplete hello zbytek tohoto průvodce, je nutné hello [nejnovější Azure CLI 1.0](../../xplat-cli-install.md) nainstalován a přihlášení pomocí režimu Resource Manager hello takto:</span><span class="sxs-lookup"><span data-stu-id="36d5a-179">toocomplete hello remainder of this guide, you need hello [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="36d5a-180">V rámci hello příkladech nahraďte všechny parametry příklad vlastní názvy, umístění a hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="36d5a-180">Throughout hello command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="36d5a-181">Hello následující příklady použití konvence `myResourceGroup`, `myKeyVault`, `myAADApp`atd.</span><span class="sxs-lookup"><span data-stu-id="36d5a-181">hello following examples use a convention of `myResourceGroup`, `myKeyVault`, `myAADApp`, etc.</span></span>

<span data-ttu-id="36d5a-182">prvním krokem Hello je toocreate Azure Key Vault toostore kryptografických klíčů.</span><span class="sxs-lookup"><span data-stu-id="36d5a-182">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="36d5a-183">Azure Key Vault můžete ukládat klíče, tajné klíče, nebo jejich hesla, které vám umožňují toosecurely implementaci v vašim aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="36d5a-183">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="36d5a-184">Šifrování virtuálního disku použijte Key Vault toostore kryptografický klíč, který je použité tooencrypt nebo dešifrovat virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="36d5a-184">For virtual disk encryption, you use Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="36d5a-185">Povolit zprostředkovatele Azure Key Vault hello ve vašem předplatném Azure a pak vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="36d5a-185">Enable hello Azure Key Vault provider in your Azure subscription, then create a resource group.</span></span> <span data-ttu-id="36d5a-186">Hello následující příklad vytvoří skupinu prostředků s názvem `myResourceGroup` v hello `WestUS` umístění:</span><span class="sxs-lookup"><span data-stu-id="36d5a-186">hello following example creates a resource group named `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="36d5a-187">Hello Azure Key Vault obsahující hello kryptografické klíče a přidružených výpočetních, které prostředky, jako je například úložiště a hello virtuální počítač se musí nacházet v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="36d5a-187">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="36d5a-188">Hello následující příklad vytvoří Azure Key Vault s názvem `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="36d5a-188">hello following example creates an Azure Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="36d5a-189">Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="36d5a-189">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="36d5a-190">Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-190">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="36d5a-191">Není další náklady toocreating premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem.</span><span class="sxs-lookup"><span data-stu-id="36d5a-191">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="36d5a-192">Přidat premium Key Vault, v předchozím kroku hello toocreate `--sku Premium` toohello příkaz.</span><span class="sxs-lookup"><span data-stu-id="36d5a-192">toocreate a premium Key Vault, in hello preceding step add `--sku Premium` toohello command.</span></span> <span data-ttu-id="36d5a-193">Hello následující příklad používá klíče chráněné softwarem vzhledem k tomu, že jsme vytvořili standardní Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-193">hello following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="36d5a-194">U obou modelů ochranu se musí hello platformy Azure toobe udělit přístup toorequest hello kryptografické klíče, když hello virtuální počítač spustí toodecrypt hello virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="36d5a-194">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="36d5a-195">Vytvořit šifrovací klíč v Key Vault a pak ji povolit pro použití s šifrováním virtuálního disku.</span><span class="sxs-lookup"><span data-stu-id="36d5a-195">Create an encryption key within your Key Vault, then enable it for use with virtual disk encryption.</span></span> <span data-ttu-id="36d5a-196">Hello následující příklad vytvoří klíč s názvem `myKey` a povolí ho šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="36d5a-196">hello following example creates a key named `myKey` and then enables it for disk encryption:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-hello-azure-active-directory-application"></a><span data-ttu-id="36d5a-197">Vytvoření aplikace Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="36d5a-197">Create hello Azure Active Directory application</span></span>
<span data-ttu-id="36d5a-198">Když virtuální disky jsou zašifrovaná nebo dešifrovat, použijte ověřování hello toohandle koncový bod a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="36d5a-198">When virtual disks are encrypted or decrypted, you use an endpoint toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="36d5a-199">Tento koncový bod, aplikaci Azure Active Directory umožňuje hello platformy Azure toorequest odpovídající kryptografické klíče hello jménem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36d5a-199">This endpoint, an Azure Active Directory application, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="36d5a-200">Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="36d5a-200">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="36d5a-201">Jako úplné aplikace Azure Active Directory nejsou vytváření, hello `--home-page` a `--identifier-uris` parametry v hello následující ukázka nemusí toobe skutečné směrovatelné adresu.</span><span class="sxs-lookup"><span data-stu-id="36d5a-201">As you are not creating a full Azure Active Directory application, hello `--home-page` and `--identifier-uris` parameters in hello following example do not need toobe actual routable address.</span></span> <span data-ttu-id="36d5a-202">Hello následující příklad také určuje tajného klíče založené na heslech, nikoli generování klíčů z v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36d5a-202">hello following example also specifies a password-based secret rather than generating keys from within hello Azure portal.</span></span> <span data-ttu-id="36d5a-203">Jako tento čas generování klíčů nelze provést z hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="36d5a-203">As this time, generating keys cannot be done from hello Azure CLI.</span></span>

<span data-ttu-id="36d5a-204">Vytvoření aplikace Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="36d5a-204">Create your Azure Active Directory application.</span></span> <span data-ttu-id="36d5a-205">Hello následující příklad vytvoří aplikaci s názvem `myAADApp` a používá na heslo `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="36d5a-205">hello following example creates an application named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="36d5a-206">Zadejte své heslo následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36d5a-206">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="36d5a-207">Poznamenejte si hello `applicationId` , je vrácen ve výstupu hello z hello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="36d5a-207">Make a note of hello `applicationId` that is returned in hello output from hello preceding command.</span></span> <span data-ttu-id="36d5a-208">Toto ID aplikace se používá v některé z hello zbývající kroky.</span><span class="sxs-lookup"><span data-stu-id="36d5a-208">This application ID is used in some of hello remaining steps.</span></span> <span data-ttu-id="36d5a-209">Dále vytvořte hlavní název služby (SPN) tak, aby aplikace hello je dostupný ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="36d5a-209">Next, create a service principal name (SPN) so that hello application is accessible within your environment.</span></span> <span data-ttu-id="36d5a-210">toosuccessfully zašifrovat nebo dešifrovat virtuální disky, oprávnění hello kryptografického klíče uložené v Key Vault musí být sada toopermit hello Azure Active Directory aplikace tooread hello klíče.</span><span class="sxs-lookup"><span data-stu-id="36d5a-210">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory application tooread hello keys.</span></span>

<span data-ttu-id="36d5a-211">Vytvořte hello hlavního názvu služby a nastavte příslušná oprávnění hello takto:</span><span class="sxs-lookup"><span data-stu-id="36d5a-211">Create hello SPN and set hello appropriate permissions as follows:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a><span data-ttu-id="36d5a-212">Přidat virtuální disk a zkontrolovat stav šifrování</span><span class="sxs-lookup"><span data-stu-id="36d5a-212">Add a virtual disk and review encryption status</span></span>
<span data-ttu-id="36d5a-213">tooactually šifrování některé virtuální disky, umožňuje přidat disk tooan, existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="36d5a-213">tooactually encrypt some virtual disks, lets add a disk tooan existing VM.</span></span> <span data-ttu-id="36d5a-214">Přidejte tooan 5Gb dat disku existující virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36d5a-214">Add a 5Gb data disk tooan existing VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="36d5a-215">aktuálně nejsou šifrovány Hello virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="36d5a-215">hello virtual disks are not currently encrypted.</span></span> <span data-ttu-id="36d5a-216">Zkontrolujte aktuální stav šifrování hello vašeho virtuálního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="36d5a-216">Review hello current encryption status of your VM as follows:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a><span data-ttu-id="36d5a-217">Šifrování virtuálních disků</span><span class="sxs-lookup"><span data-stu-id="36d5a-217">Encrypt virtual disks</span></span>
<span data-ttu-id="36d5a-218">toonow šifrování hello virtuální disky, můžete seskupit všechny předchozí komponenty hello:</span><span class="sxs-lookup"><span data-stu-id="36d5a-218">toonow encrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="36d5a-219">Zadejte hello aplikaci Azure Active Directory a heslo.</span><span class="sxs-lookup"><span data-stu-id="36d5a-219">Specify hello Azure Active Directory application and password.</span></span>
2. <span data-ttu-id="36d5a-220">Zadejte hello Key Vault toostore hello metadata pro šifrované disky.</span><span class="sxs-lookup"><span data-stu-id="36d5a-220">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="36d5a-221">Zadejte hello toouse kryptografické klíče pro hello skutečné šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="36d5a-221">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="36d5a-222">Zadejte, zda chcete disk tooencrypt hello operačního systému, hello datových disků nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="36d5a-222">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="36d5a-223">Umožňuje, zkontrolujte podrobnosti hello Azure Key Vault a hello klíče, které jste vytvořili, potřebujete hello ID klíč trezoru, identifikátor URI, a potom klíče adresy URL v posledním kroku hello:</span><span class="sxs-lookup"><span data-stu-id="36d5a-223">Lets review hello details for your Azure Key Vault and hello key you created, as you need hello Key Vault ID, URI, and then key URL in hello final step:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="36d5a-224">Zašifrovat virtuální disky pomocí hello výstup hello `azure keyvault show` a `azure keyvault key show` příkazy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36d5a-224">Encrypt your virtual disks using hello output from hello `azure keyvault show` and `azure keyvault key show` commands as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="36d5a-225">Jak hello předchozí příkaz má mnoho proměnné, hello následující příklad je hello dokončení příkazu pro referenci:</span><span class="sxs-lookup"><span data-stu-id="36d5a-225">As hello preceding command has many variables, hello following example is hello complete command for reference:</span></span>

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

<span data-ttu-id="36d5a-226">Hello rozhraní příkazového řádku Azure neposkytuje podrobné chyby během procesu šifrování hello.</span><span class="sxs-lookup"><span data-stu-id="36d5a-226">hello Azure CLI doesn't provide verbose errors during hello encryption process.</span></span> <span data-ttu-id="36d5a-227">Další informace o řešení potíží, `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` na hello šifrujete virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="36d5a-227">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` on hello VM you are encrypting.</span></span>

<span data-ttu-id="36d5a-228">Nakonec umožňuje zkontrolovat stav šifrování hello znovu tooconfirm, mají nyní šifrované virtuální disky:</span><span class="sxs-lookup"><span data-stu-id="36d5a-228">Finally, lets review hello encryption status again tooconfirm that your virtual disks have now been encrypted:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a><span data-ttu-id="36d5a-229">Přidání dalších datových disků</span><span class="sxs-lookup"><span data-stu-id="36d5a-229">Add additional data disks</span></span>
<span data-ttu-id="36d5a-230">Jakmile jste zašifrovali datových disků, můžete později přidat tooyour další virtuální disky virtuálních počítačů a také zašifrovat.</span><span class="sxs-lookup"><span data-stu-id="36d5a-230">Once you have encrypted your data disks, you can later add additional virtual disks tooyour VM and also encrypt them.</span></span> <span data-ttu-id="36d5a-231">Když spustíte hello `azure vm enable-disk-encryption` příkaz, přírůstek hello pořadí verzi pomocí hello `--sequence-version` parametr.</span><span class="sxs-lookup"><span data-stu-id="36d5a-231">When you run hello `azure vm enable-disk-encryption` command, increment hello sequence version using hello `--sequence-version` parameter.</span></span> <span data-ttu-id="36d5a-232">Tento parametr pořadí verzi můžete tooperform operací s hello stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="36d5a-232">This sequence version parameter allows you tooperform repeated operations on hello same VM.</span></span>

<span data-ttu-id="36d5a-233">Umožňuje například přidat druhý virtuální disk tooyour virtuální počítač následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="36d5a-233">For example, lets add a second virtual disk tooyour VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="36d5a-234">Znovu spustit hello příkaz tooencrypt hello virtuální disky, tentokrát přidání hello `--sequence-version` parametr a narůstajícími hello hodnotu z našich prvního spustit takto:</span><span class="sxs-lookup"><span data-stu-id="36d5a-234">Rerun hello command tooencrypt hello virtual disks, this time adding hello `--sequence-version` parameter, and incrementing hello value from our first run as follows:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="36d5a-235">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36d5a-235">Next steps</span></span>
* <span data-ttu-id="36d5a-236">Další informace o správě Azure Key Vault, včetně odstraňování kryptografické klíče a trezory, najdete v části [Key Vault spravovat pomocí rozhraní příkazového řádku](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="36d5a-236">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="36d5a-237">Další informace o šifrování disku, například při přípravě šifrované vlastní virtuální počítač tooupload tooAzure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="36d5a-237">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
