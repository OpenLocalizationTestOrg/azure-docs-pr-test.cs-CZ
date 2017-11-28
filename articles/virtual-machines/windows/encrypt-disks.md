---
title: "aaaEncrypt disky na virtuální počítač s Windows v Azure | Microsoft Docs"
description: "Jak tooencrypt virtuální disky na virtuální počítač s Windows pro rozšířené zabezpečení pomocí Azure PowerShell"
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
ms.openlocfilehash: 77c42a67cb57a9dc5fe3159fce0be75e3a965be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-windows-vm"></a><span data-ttu-id="3dacc-103">Jak tooencrypt virtuální disky na virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="3dacc-103">How tooencrypt virtual disks on a Windows VM</span></span>
<span data-ttu-id="3dacc-104">Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure.</span><span class="sxs-lookup"><span data-stu-id="3dacc-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="3dacc-105">Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="3dacc-106">Řízení těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="3dacc-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="3dacc-107">Tento článek podrobnosti o tom, jak tooencrypt virtuální disky na virtuální počítač s Windows pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3dacc-107">This article details how tooencrypt virtual disks on a Windows VM using Azure PowerShell.</span></span> <span data-ttu-id="3dacc-108">Můžete také [šifrování virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../linux/encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="3dacc-108">You can also [encrypt a Linux VM using hello Azure CLI 2.0](../linux/encrypt-disks.md).</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="3dacc-109">Přehled šifrování disku</span><span class="sxs-lookup"><span data-stu-id="3dacc-109">Overview of disk encryption</span></span>
<span data-ttu-id="3dacc-110">Virtuální disky na virtuální počítače Windows jsou zašifrovaná přinejmenším pomocí nástroje Bitlocker.</span><span class="sxs-lookup"><span data-stu-id="3dacc-110">Virtual disks on Windows VMs are encrypted at rest using Bitlocker.</span></span> <span data-ttu-id="3dacc-111">Není nijak zpoplatněn pro šifrování virtuálních disků v Azure.</span><span class="sxs-lookup"><span data-stu-id="3dacc-111">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="3dacc-112">Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikované tooFIPS standardy úroveň 2 140-2.</span><span class="sxs-lookup"><span data-stu-id="3dacc-112">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="3dacc-113">Tyto kryptografické klíče jsou použité tooencrypt a dešifrování tooyour připojené virtuální disky virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-113">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="3dacc-114">Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití.</span><span class="sxs-lookup"><span data-stu-id="3dacc-114">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="3dacc-115">Hlavní služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.</span><span class="sxs-lookup"><span data-stu-id="3dacc-115">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="3dacc-116">Hello proces šifrování virtuálního počítače je následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dacc-116">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="3dacc-117">Vytvoření kryptografické klíče v Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-117">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="3dacc-118">Nakonfigurujte hello kryptografické klíče toobe použitelné pro šifrování disků.</span><span class="sxs-lookup"><span data-stu-id="3dacc-118">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="3dacc-119">tooread hello kryptografický klíč z hello Azure Key Vault, vytvoření služby Azure Active Directory objekt zabezpečení s příslušnými oprávněními hello.</span><span class="sxs-lookup"><span data-stu-id="3dacc-119">tooread hello cryptographic key from hello Azure Key Vault, create an Azure Active Directory service principal with hello appropriate permissions.</span></span>
4. <span data-ttu-id="3dacc-120">Vydejte příkaz tooencrypt hello virtuální disky, zadáte hello objektu služby Azure Active Directory a příslušné kryptografické klíče toobe použít.</span><span class="sxs-lookup"><span data-stu-id="3dacc-120">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory service principal and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="3dacc-121">hlavní požadavky služby Azure Active Directory Hello hello požadované kryptografický klíč z Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-121">hello Azure Active Directory service principal requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="3dacc-122">Hello virtuální disky jsou šifrované pomocí hello zadaný kryptografický klíč.</span><span class="sxs-lookup"><span data-stu-id="3dacc-122">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="3dacc-123">Proces šifrování</span><span class="sxs-lookup"><span data-stu-id="3dacc-123">Encryption process</span></span>
<span data-ttu-id="3dacc-124">Šifrování disku spoléhá na hello následující další součásti:</span><span class="sxs-lookup"><span data-stu-id="3dacc-124">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="3dacc-125">**Azure Key Vault** -použít toosafeguard kryptografické klíče a tajné klíče používané pro proces šifrování/dešifrování hello disku.</span><span class="sxs-lookup"><span data-stu-id="3dacc-125">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span> 
  * <span data-ttu-id="3dacc-126">Pokud ano, můžete použít existující Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-126">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="3dacc-127">Nemáte toodedicate disky tooencrypting Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-127">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="3dacc-128">hranicemi správy tooseparate a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-128">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="3dacc-129">**Azure Active Directory** – obslužné rutiny hello zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce.</span><span class="sxs-lookup"><span data-stu-id="3dacc-129">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span> 
  * <span data-ttu-id="3dacc-130">Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3dacc-130">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="3dacc-131">Hello instanční objekt poskytuje zabezpečené mechanismus toorequest a vydávají hello odpovídající kryptografické klíče.</span><span class="sxs-lookup"><span data-stu-id="3dacc-131">hello service principal provides a secure mechanism toorequest and be issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="3dacc-132">Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3dacc-132">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="3dacc-133">Požadavky a omezení</span><span class="sxs-lookup"><span data-stu-id="3dacc-133">Requirements and limitations</span></span>
<span data-ttu-id="3dacc-134">Podporované scénáře a požadavky na šifrování disku:</span><span class="sxs-lookup"><span data-stu-id="3dacc-134">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="3dacc-135">Povoluje šifrování na nové virtuální počítače Windows z Azure Marketplace obrázky nebo vlastní image virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="3dacc-135">Enabling encryption on new Windows VMs from Azure Marketplace images or custom VHD image.</span></span>
* <span data-ttu-id="3dacc-136">Povoluje šifrování na stávajících virtuálních počítačích Windows v Azure.</span><span class="sxs-lookup"><span data-stu-id="3dacc-136">Enabling encryption on existing Windows VMs in Azure.</span></span>
* <span data-ttu-id="3dacc-137">Povoluje šifrování na virtuálních počítačích Windows, které jsou nakonfigurované pomocí prostorů úložiště.</span><span class="sxs-lookup"><span data-stu-id="3dacc-137">Enabling encryption on Windows VMs that are configured using Storage Spaces.</span></span>
* <span data-ttu-id="3dacc-138">Zakázáním šifrování na operačního systému a datové disky pro virtuální počítače Windows.</span><span class="sxs-lookup"><span data-stu-id="3dacc-138">Disabling encryption on OS and data drives for Windows VMs.</span></span>
* <span data-ttu-id="3dacc-139">Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být v hello stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="3dacc-139">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="3dacc-140">Úroveň Standard virtuálních počítačů, jako je například, D, DS, G a GS řadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-140">Standard tier VMs, such as A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="3dacc-141">Šifrování disku aktuálně nepodporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="3dacc-141">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="3dacc-142">Úroveň Basic virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-142">Basic tier VMs.</span></span>
* <span data-ttu-id="3dacc-143">Virtuální počítače vytvořené pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="3dacc-143">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="3dacc-144">Aktualizace hello kryptografické klíče již šifrované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3dacc-144">Updating hello cryptographic keys on an already encrypted VM.</span></span>
* <span data-ttu-id="3dacc-145">Integrace s místní službu správy klíčů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-145">Integration with on-prem Key Management Service.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="3dacc-146">Vytvoření Azure Key Vault a klíče</span><span class="sxs-lookup"><span data-stu-id="3dacc-146">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="3dacc-147">Než začnete, zkontrolujte, že hello nejnovější verzi hello byl nainstalován modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3dacc-147">Before you start, make sure that hello latest version of hello Azure PowerShell module has been installed.</span></span> <span data-ttu-id="3dacc-148">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3dacc-148">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="3dacc-149">V rámci hello příkladech nahraďte všechny parametry příklad vlastní názvy, umístění a hodnoty klíče.</span><span class="sxs-lookup"><span data-stu-id="3dacc-149">Throughout hello command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="3dacc-150">Hello následující příklady použití konvence *myResourceGroup*, *myKeyVault*, *Můjvp*atd.</span><span class="sxs-lookup"><span data-stu-id="3dacc-150">hello following examples use a convention of *myResourceGroup*, *myKeyVault*, *myVM*, etc.</span></span>

<span data-ttu-id="3dacc-151">prvním krokem Hello je toocreate Azure Key Vault toostore kryptografických klíčů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-151">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="3dacc-152">Azure Key Vault můžete ukládat klíče, tajné klíče, nebo jejich hesla, které vám umožňují toosecurely implementaci v vašim aplikacím a službám.</span><span class="sxs-lookup"><span data-stu-id="3dacc-152">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="3dacc-153">Šifrování virtuálního disku můžete vytvořit toostore Key Vault kryptografický klíč, který je použité tooencrypt nebo dešifrovat virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="3dacc-153">For virtual disk encryption, you create a Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span> 

<span data-ttu-id="3dacc-154">Povolit hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), pak vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="3dacc-154">Enable hello Azure Key Vault provider within your Azure subscription with [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), then create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="3dacc-155">Hello následující příklad vytvoří název skupiny prostředků *myResourceGroup* v hello *východní USA* umístění:</span><span class="sxs-lookup"><span data-stu-id="3dacc-155">hello following example creates a resource group name *myResourceGroup* in hello *East US* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

<span data-ttu-id="3dacc-156">Hello Azure Key Vault obsahující hello kryptografické klíče a přidružených výpočetních, které prostředky, jako je například úložiště a hello virtuální počítač se musí nacházet v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="3dacc-156">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="3dacc-157">Vytvoření Azure Key Vault s [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) a povolte hello Key Vault pro použití s šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="3dacc-157">Create an Azure Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="3dacc-158">Zadejte jedinečný název pro Key Vault *keyVaultName* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dacc-158">Specify a unique Key Vault name for *keyVaultName* as follows:</span></span>

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

<span data-ttu-id="3dacc-159">Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM).</span><span class="sxs-lookup"><span data-stu-id="3dacc-159">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="3dacc-160">Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-160">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="3dacc-161">Není další náklady toocreating premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem.</span><span class="sxs-lookup"><span data-stu-id="3dacc-161">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="3dacc-162">toocreate premium Key Vault, v předchozím kroku hello přidat hello *- Sku "Premium"* parametry.</span><span class="sxs-lookup"><span data-stu-id="3dacc-162">toocreate a premium Key Vault, in hello preceding step add hello *-Sku "Premium"* parameters.</span></span> <span data-ttu-id="3dacc-163">Hello následující příklad používá klíče chráněné softwarem vzhledem k tomu, že jsme vytvořili standardní Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-163">hello following example uses software-protected keys since we created a standard Key Vault.</span></span> 

<span data-ttu-id="3dacc-164">U obou modelů ochranu se musí hello platformy Azure toobe udělit přístup toorequest hello kryptografické klíče, když hello virtuální počítač spustí toodecrypt hello virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="3dacc-164">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="3dacc-165">Vytvoření kryptografické klíče v Key Vault s [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span><span class="sxs-lookup"><span data-stu-id="3dacc-165">Create a cryptographic key in your Key Vault with [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span></span> <span data-ttu-id="3dacc-166">Hello následující příklad vytvoří klíč s názvem *myKey*:</span><span class="sxs-lookup"><span data-stu-id="3dacc-166">hello following example creates a key named *myKey*:</span></span>

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-hello-azure-active-directory-service-principal"></a><span data-ttu-id="3dacc-167">Vytvořit objekt služby Azure Active Directory hello</span><span class="sxs-lookup"><span data-stu-id="3dacc-167">Create hello Azure Active Directory service principal</span></span>
<span data-ttu-id="3dacc-168">Když virtuální disky jsou zašifrovaná nebo dešifrovat, je třeba zadat ověření účtu toohandle hello a výměna kryptografických klíčů z Key Vault.</span><span class="sxs-lookup"><span data-stu-id="3dacc-168">When virtual disks are encrypted or decrypted, you specify an account toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="3dacc-169">Tento účet objektu zabezpečení služby Azure Active Directory umožňuje hello platformy Azure toorequest odpovídající kryptografické klíče hello jménem hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-169">This account, an Azure Active Directory service principal, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="3dacc-170">Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3dacc-170">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="3dacc-171">Vytvoření instančního objektu v Azure Active Directory s [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span><span class="sxs-lookup"><span data-stu-id="3dacc-171">Create a service principal in Azure Active Directory with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span></span> <span data-ttu-id="3dacc-172">toospecify zabezpečeného hesla, postupujte podle hello [zásady hesel a omezení v Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span><span class="sxs-lookup"><span data-stu-id="3dacc-172">toospecify a secure password, follow hello [Password policies and restrictions in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span></span>

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

<span data-ttu-id="3dacc-173">toosuccessfully zašifrovat nebo dešifrovat virtuální disky, oprávnění hello kryptografického klíče uložené v Key Vault musí být sada toopermit hello Azure Active Directory service hlavní tooread hello klíče.</span><span class="sxs-lookup"><span data-stu-id="3dacc-173">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory service principal tooread hello keys.</span></span> <span data-ttu-id="3dacc-174">Nastavení oprávnění pro Key Vault s [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span><span class="sxs-lookup"><span data-stu-id="3dacc-174">Set permissions on your Key Vault with [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a><span data-ttu-id="3dacc-175">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3dacc-175">Create virtual machine</span></span>
<span data-ttu-id="3dacc-176">tootest hello proces šifrování, můžeme vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3dacc-176">tootest hello encryption process, let's create a VM.</span></span> <span data-ttu-id="3dacc-177">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí *Windows Server 2016 Datacenter* bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="3dacc-177">hello following example creates a VM named *myVM* using a *Windows Server 2016 Datacenter* image:</span></span>

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


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="3dacc-178">Šifrování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="3dacc-178">Encrypt virtual machine</span></span>
<span data-ttu-id="3dacc-179">tooencrypt hello virtuální disky, můžete seskupit všechny předchozí komponenty hello:</span><span class="sxs-lookup"><span data-stu-id="3dacc-179">tooencrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="3dacc-180">Zadejte hello objektu služby Azure Active Directory a heslo.</span><span class="sxs-lookup"><span data-stu-id="3dacc-180">Specify hello Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="3dacc-181">Zadejte hello Key Vault toostore hello metadata pro šifrované disky.</span><span class="sxs-lookup"><span data-stu-id="3dacc-181">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="3dacc-182">Zadejte hello toouse kryptografické klíče pro hello skutečné šifrování a dešifrování.</span><span class="sxs-lookup"><span data-stu-id="3dacc-182">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="3dacc-183">Zadejte, zda chcete disk tooencrypt hello operačního systému, hello datových disků nebo všechny.</span><span class="sxs-lookup"><span data-stu-id="3dacc-183">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="3dacc-184">Šifrování virtuálního počítače s [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) pomocí klíče hello Azure Key Vault a hlavní přihlašovací údaje služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3dacc-184">Encrypt your VM with [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) using hello Azure Key Vault key and Azure Active Directory service principal credentials.</span></span> <span data-ttu-id="3dacc-185">Hello následující příklad načte všechny klíčové informace hello potom šifruje hello virtuálního počítače s názvem *Můjvp*:</span><span class="sxs-lookup"><span data-stu-id="3dacc-185">hello following example retrieves all hello key information then encrypts hello VM named *myVM*:</span></span>

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

<span data-ttu-id="3dacc-186">Přijetí výzvy toocontinue hello šifrováním hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-186">Accept hello prompt toocontinue with hello VM encryption.</span></span> <span data-ttu-id="3dacc-187">během procesu hello restartuje Hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dacc-187">hello VM restarts during hello process.</span></span> <span data-ttu-id="3dacc-188">Po dokončení procesu hello šifrování a hello virtuální počítač byl restartován, zkontrolovat stav šifrování hello s [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span><span class="sxs-lookup"><span data-stu-id="3dacc-188">Once hello encryption process completes and hello VM has rebooted, review hello encryption status with [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

<span data-ttu-id="3dacc-189">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3dacc-189">hello output is similar toohello following example:</span></span>

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a><span data-ttu-id="3dacc-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3dacc-190">Next steps</span></span>
* <span data-ttu-id="3dacc-191">Další informace o správě Azure Key Vault najdete v tématu [nastavit Key Vault pro virtuální počítače](key-vault-setup.md).</span><span class="sxs-lookup"><span data-stu-id="3dacc-191">For more information about managing Azure Key Vault, see [Set up Key Vault for virtual machines](key-vault-setup.md).</span></span>
* <span data-ttu-id="3dacc-192">Další informace o šifrování disku, například při přípravě šifrované vlastní virtuální počítač tooupload tooAzure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="3dacc-192">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
