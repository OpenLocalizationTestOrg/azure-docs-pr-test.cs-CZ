---
title: "Šifrování disky na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Postup zašifrování virtuální disky na virtuální počítač s Windows pro zvýšení zabezpečení pomocí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/10/2017
ms.author: iainfou
ms.openlocfilehash: 98b42b252a601af090579e3939f3c7ab91c3803b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a><span data-ttu-id="25b25-103">Postup zašifrování virtuální disky na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="25b25-103">How to encrypt virtual disks on a Windows VM</span></span>
<span data-ttu-id="25b25-104">Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure.</span><span class="sxs-lookup"><span data-stu-id="25b25-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="25b25-105">Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="25b25-106">Řízení těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="25b25-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="25b25-107">Tento článek podrobně popisují zašifrovat virtuální disky na virtuální počítač s Windows pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="25b25-107">This article details how to encrypt virtual disks on a Windows VM using Azure PowerShell.</span></span> <span data-ttu-id="25b25-108">Můžete také [šifrování virtuálního počítače s Linuxem pomocí Azure CLI 2.0](../linux/encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="25b25-108">You can also [encrypt a Linux VM using the Azure CLI 2.0](../linux/encrypt-disks.md).</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="25b25-109">Přehled šifrování disku</span><span class="sxs-lookup"><span data-stu-id="25b25-109">Overview of disk encryption</span></span>
<span data-ttu-id="25b25-110">Virtuální disky na virtuální počítače Windows jsou zašifrovaná přinejmenším pomocí nástroje Bitlocker.</span><span class="sxs-lookup"><span data-stu-id="25b25-110">Virtual disks on Windows VMs are encrypted at rest using Bitlocker.</span></span> <span data-ttu-id="25b25-111">Není nijak zpoplatněn pro šifrování virtuálních disků v Azure.</span><span class="sxs-lookup"><span data-stu-id="25b25-111">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="25b25-112">Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikovány pro FIPS 140-2 úroveň 2 standardů.</span><span class="sxs-lookup"><span data-stu-id="25b25-112">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="25b25-113">Tyto klíče se používají k šifrování a dešifrování virtuálních disků připojených k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="25b25-113">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="25b25-114">Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="25b25-114">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="25b25-115">Hlavní služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="25b25-115">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="25b25-116">Proces šifrování virtuálního počítače je následující:</span><span class="sxs-lookup"><span data-stu-id="25b25-116">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="25b25-117">Vytvoření kryptografické klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-117">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="25b25-118">Nakonfigurujte kryptografický klíč možné používat pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="25b25-118">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="25b25-119">Čtení kryptografického klíče z Azure Key Vault, vytvořte služby Azure Active Directory objekt s příslušnými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="25b25-119">To read the cryptographic key from the Azure Key Vault, create an Azure Active Directory service principal with the appropriate permissions.</span></span>
4. <span data-ttu-id="25b25-120">Příkaz pro zašifrování virtuální disky, zadávání Azure Active Directory service hlavní a vhodný kryptografické klíče k použití.</span><span class="sxs-lookup"><span data-stu-id="25b25-120">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory service principal and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="25b25-121">Objekt služby Azure Active Directory vyžaduje požadované kryptografický klíč z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-121">The Azure Active Directory service principal requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="25b25-122">Virtuální disky jsou šifrované pomocí zadaný kryptografický klíč.</span><span class="sxs-lookup"><span data-stu-id="25b25-122">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="25b25-123">Proces šifrování</span><span class="sxs-lookup"><span data-stu-id="25b25-123">Encryption process</span></span>
<span data-ttu-id="25b25-124">Šifrování disku spoléhá na následující součásti:</span><span class="sxs-lookup"><span data-stu-id="25b25-124">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="25b25-125">**Azure Key Vault** – používané k ochraně kryptografické klíče a tajné klíče pro proces šifrování a dešifrování disku.</span><span class="sxs-lookup"><span data-stu-id="25b25-125">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span> 
  * <span data-ttu-id="25b25-126">Pokud ano, můžete použít existující Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-126">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="25b25-127">Není nutné vyhradit Key Vault pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="25b25-127">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="25b25-128">K oddělení hranicemi správy a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-128">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="25b25-129">**Azure Active Directory** -zpracovává zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.</span><span class="sxs-lookup"><span data-stu-id="25b25-129">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span> 
  * <span data-ttu-id="25b25-130">Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25b25-130">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="25b25-131">Objekt služby poskytuje zabezpečené mechanismus k vyžádání a vydávají příslušné kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="25b25-131">The service principal provides a secure mechanism to request and be issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="25b25-132">Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25b25-132">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="25b25-133">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="25b25-133">Requirements and limitations</span></span>
<span data-ttu-id="25b25-134">Podporované scénáře a požadavky na šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="25b25-134">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="25b25-135">Povoluje šifrování na nové virtuální počítače Windows z Azure Marketplace obrázky nebo vlastní image virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="25b25-135">Enabling encryption on new Windows VMs from Azure Marketplace images or custom VHD image.</span></span>
* <span data-ttu-id="25b25-136">Povoluje šifrování na stávajících virtuálních počítačích Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="25b25-136">Enabling encryption on existing Windows VMs in Azure.</span></span>
* <span data-ttu-id="25b25-137">Povoluje šifrování na virtuálních počítačích Windows, které jsou nakonfigurované pomocí prostorů úložiště.</span><span class="sxs-lookup"><span data-stu-id="25b25-137">Enabling encryption on Windows VMs that are configured using Storage Spaces.</span></span>
* <span data-ttu-id="25b25-138">Zakázáním šifrování na operačního systému a datové disky pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="25b25-138">Disabling encryption on OS and data drives for Windows VMs.</span></span>
* <span data-ttu-id="25b25-139">Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být ve stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="25b25-139">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="25b25-140">Úroveň Standard virtuálních počítačů, jako je například, D, DS, G a GS řadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="25b25-140">Standard tier VMs, such as A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="25b25-141">Šifrování disku není aktuálně podporováno v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="25b25-141">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="25b25-142">Úroveň Basic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="25b25-142">Basic tier VMs.</span></span>
* <span data-ttu-id="25b25-143">Virtuální počítače vytvořené pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="25b25-143">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="25b25-144">Aktualizuje se kryptografické klíče již šifrované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="25b25-144">Updating the cryptographic keys on an already encrypted VM.</span></span>
* <span data-ttu-id="25b25-145">Integrace s místní službu správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="25b25-145">Integration with on-prem Key Management Service.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="25b25-146">Vytvoření Azure Key Vault a klíče</span><span class="sxs-lookup"><span data-stu-id="25b25-146">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="25b25-147">Než začnete, ujistěte se, že je nainstalovaná nejnovější verze modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="25b25-147">Before you start, make sure that the latest version of the Azure PowerShell module has been installed.</span></span> <span data-ttu-id="25b25-148">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="25b25-148">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="25b25-149">V příkladech nahraďte všechny parametry příklad vlastní názvy, umístění a hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="25b25-149">Throughout the command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="25b25-150">Následující příklady použití konvence *myResourceGroup*, *myKeyVault*, *Můjvp*atd.</span><span class="sxs-lookup"><span data-stu-id="25b25-150">The following examples use a convention of *myResourceGroup*, *myKeyVault*, *myVM*, etc.</span></span>

<span data-ttu-id="25b25-151">Prvním krokem je vytvoření Azure Key Vault pro ukládání kryptografických klíčů.</span><span class="sxs-lookup"><span data-stu-id="25b25-151">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="25b25-152">Azure Key Vault můžete uložit klíčů, tajné klíče nebo hesla, které vám umožní bezpečně je implementovat v vašim aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="25b25-152">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="25b25-153">Šifrování virtuálního disku vytvoříte Key Vault pro uložení kryptografického klíče, který se používá k šifrování a dešifrování virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="25b25-153">For virtual disk encryption, you create a Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span> 

<span data-ttu-id="25b25-154">Povolit Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), pak vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="25b25-154">Enable the Azure Key Vault provider within your Azure subscription with [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), then create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="25b25-155">Následující příklad vytvoří název skupiny prostředků *myResourceGroup* v *východní USA* umístění:</span><span class="sxs-lookup"><span data-stu-id="25b25-155">The following example creates a resource group name *myResourceGroup* in the *East US* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

<span data-ttu-id="25b25-156">Azure Key Vault, který obsahuje kryptografické klíče a přidružené výpočetní prostředky, jako je například úložiště a virtuální počítač se musí nacházet ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="25b25-156">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="25b25-157">Vytvoření Azure Key Vault s [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) a povolte Key Vault pro použití s šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="25b25-157">Create an Azure Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="25b25-158">Zadejte jedinečný název pro Key Vault *keyVaultName* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="25b25-158">Specify a unique Key Vault name for *keyVaultName* as follows:</span></span>

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

<span data-ttu-id="25b25-159">Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="25b25-159">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="25b25-160">Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-160">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="25b25-161">Není dalších nákladů na vytváření premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem.</span><span class="sxs-lookup"><span data-stu-id="25b25-161">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="25b25-162">Chcete-li vytvořit premium Key Vault, přidejte v předchozím kroku *- Sku "Premium"* parametry.</span><span class="sxs-lookup"><span data-stu-id="25b25-162">To create a premium Key Vault, in the preceding step add the *-Sku "Premium"* parameters.</span></span> <span data-ttu-id="25b25-163">Následující příklad používá klíče chráněné softwarem, protože jsme vytvořili standardní Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-163">The following example uses software-protected keys since we created a standard Key Vault.</span></span> 

<span data-ttu-id="25b25-164">Pro oba modely ochrany musí mít udělen přístup k žádosti o kryptografických klíčů, když se virtuální počítač spustí do dešifrovat virtuální disky platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="25b25-164">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="25b25-165">Vytvoření kryptografické klíče v Key Vault s [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span><span class="sxs-lookup"><span data-stu-id="25b25-165">Create a cryptographic key in your Key Vault with [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span></span> <span data-ttu-id="25b25-166">Následující příklad vytvoří klíč s názvem *myKey*:</span><span class="sxs-lookup"><span data-stu-id="25b25-166">The following example creates a key named *myKey*:</span></span>

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a><span data-ttu-id="25b25-167">Vytvořit objekt služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="25b25-167">Create the Azure Active Directory service principal</span></span>
<span data-ttu-id="25b25-168">Když virtuální disky jsou zašifrovaná nebo dešifrovat, je třeba zadat účet, který chcete zpracovávat ověřování a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="25b25-168">When virtual disks are encrypted or decrypted, you specify an account to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="25b25-169">Tento účet objektu zabezpečení služby Azure Active Directory umožňuje platformy Azure k vyžádání příslušné kryptografické klíče jménem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="25b25-169">This account, an Azure Active Directory service principal, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="25b25-170">Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25b25-170">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="25b25-171">Vytvoření instančního objektu v Azure Active Directory s [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span><span class="sxs-lookup"><span data-stu-id="25b25-171">Create a service principal in Azure Active Directory with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span></span> <span data-ttu-id="25b25-172">Chcete-li zadat zabezpečeného hesla, postupujte podle [zásady hesel a omezení v Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span><span class="sxs-lookup"><span data-stu-id="25b25-172">To specify a secure password, follow the [Password policies and restrictions in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span></span>

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

<span data-ttu-id="25b25-173">Pro úspěšně šifrování nebo dešifrování virtuálních disků, musí se nastavit oprávnění na kryptografický klíč uložený v Key Vault tak, aby povolovala objektu služby Azure Active Directory ke čtení klíče.</span><span class="sxs-lookup"><span data-stu-id="25b25-173">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory service principal to read the keys.</span></span> <span data-ttu-id="25b25-174">Nastavení oprávnění pro Key Vault s [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span><span class="sxs-lookup"><span data-stu-id="25b25-174">Set permissions on your Key Vault with [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a><span data-ttu-id="25b25-175">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="25b25-175">Create virtual machine</span></span>
<span data-ttu-id="25b25-176">Chcete-li otestovat proces šifrování, vytvoříme virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="25b25-176">To test the encryption process, let's create a VM.</span></span> <span data-ttu-id="25b25-177">Následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí *Windows Server 2016 Datacenter* bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="25b25-177">The following example creates a VM named *myVM* using a *Windows Server 2016 Datacenter* image:</span></span>

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "mypublicdns$(Get-Random)"

$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$cred = Get-Credential

$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="25b25-178">Šifrování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="25b25-178">Encrypt virtual machine</span></span>
<span data-ttu-id="25b25-179">Pokud chcete zašifrovat virtuální disky, přepnutím společně předchozí komponenty:</span><span class="sxs-lookup"><span data-stu-id="25b25-179">To encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="25b25-180">Zadejte objektu služby Azure Active Directory a heslo.</span><span class="sxs-lookup"><span data-stu-id="25b25-180">Specify the Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="25b25-181">Zadejte klíč trezoru k ukládání metadat pro šifrované disky.</span><span class="sxs-lookup"><span data-stu-id="25b25-181">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="25b25-182">Zadejte kryptografické klíče k použití pro skutečné šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="25b25-182">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="25b25-183">Zadejte, jestli chcete šifrovat disk operačního systému, datových disků nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="25b25-183">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="25b25-184">Šifrování virtuálního počítače s [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) pomocí Azure Key Vault klíče a hlavní přihlašovací údaje služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="25b25-184">Encrypt your VM with [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) using the Azure Key Vault key and Azure Active Directory service principal credentials.</span></span> <span data-ttu-id="25b25-185">Následující příklad načte všechny klíčové informace potom šifruje virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="25b25-185">The following example retrieves all the key information then encrypts the VM named *myVM*:</span></span>

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

<span data-ttu-id="25b25-186">Přijetí výzvy a pokračujte šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="25b25-186">Accept the prompt to continue with the VM encryption.</span></span> <span data-ttu-id="25b25-187">Virtuální počítač se restartuje během procesu.</span><span class="sxs-lookup"><span data-stu-id="25b25-187">The VM restarts during the process.</span></span> <span data-ttu-id="25b25-188">Po dokončení procesu šifrování a virtuální počítač byl restartován, zkontrolujte stav šifrování s [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span><span class="sxs-lookup"><span data-stu-id="25b25-188">Once the encryption process completes and the VM has rebooted, review the encryption status with [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

<span data-ttu-id="25b25-189">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="25b25-189">The output is similar to the following example:</span></span>

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a><span data-ttu-id="25b25-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25b25-190">Next steps</span></span>
* <span data-ttu-id="25b25-191">Další informace o správě Azure Key Vault najdete v tématu [nastavit Key Vault pro virtuální počítače](key-vault-setup.md).</span><span class="sxs-lookup"><span data-stu-id="25b25-191">For more information about managing Azure Key Vault, see [Set up Key Vault for virtual machines](key-vault-setup.md).</span></span>
* <span data-ttu-id="25b25-192">Další informace o šifrování disku, jako je například příprava šifrované vlastních virtuálních počítačů se nahrát do Azure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="25b25-192">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
