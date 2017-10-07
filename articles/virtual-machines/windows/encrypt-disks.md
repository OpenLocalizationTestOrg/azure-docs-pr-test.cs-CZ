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
# <a name="how-tooencrypt-virtual-disks-on-a-windows-vm"></a>Jak tooencrypt virtuální disky na virtuální počítač s Windows
Pro lepší virtuální počítač (VM) zabezpečení a dodržování předpisů je možné zašifrovat virtuální disky v Azure. Disky jsou šifrované pomocí kryptografických klíčů, které jsou zabezpečené v Azure Key Vault. Řízení těchto kryptografické klíče a můžete auditovat jejich použití. Tento článek podrobnosti o tom, jak tooencrypt virtuální disky na virtuální počítač s Windows pomocí Azure PowerShell. Můžete také [šifrování virtuálního počítače s Linuxem pomocí hello Azure CLI 2.0](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Přehled šifrování disku
Virtuální disky na virtuální počítače Windows jsou zašifrovaná přinejmenším pomocí nástroje Bitlocker. Není nijak zpoplatněn pro šifrování virtuálních disků v Azure. Kryptografické klíče ukládají v Azure Key Vault pomocí ochrany proti softwaru, nebo můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM) certifikované tooFIPS standardy úroveň 2 140-2. Tyto kryptografické klíče jsou použité tooencrypt a dešifrování tooyour připojené virtuální disky virtuálních počítačů. Uchování kontroly nad těchto kryptografické klíče a můžete auditovat jejich použití. Hlavní služby Azure Active Directory poskytuje zabezpečené mechanismus pro vydávání tyto kryptografické klíče jako virtuální počítače jsou zapnuté zapnout a vypnout.

Hello proces šifrování virtuálního počítače je následujícím způsobem:

1. Vytvoření kryptografické klíče v Azure Key Vault.
2. Nakonfigurujte hello kryptografické klíče toobe použitelné pro šifrování disků.
3. tooread hello kryptografický klíč z hello Azure Key Vault, vytvoření služby Azure Active Directory objekt zabezpečení s příslušnými oprávněními hello.
4. Vydejte příkaz tooencrypt hello virtuální disky, zadáte hello objektu služby Azure Active Directory a příslušné kryptografické klíče toobe použít.
5. hlavní požadavky služby Azure Active Directory Hello hello požadované kryptografický klíč z Azure Key Vault.
6. Hello virtuální disky jsou šifrované pomocí hello zadaný kryptografický klíč.

## <a name="encryption-process"></a>Proces šifrování
Šifrování disku spoléhá na hello následující další součásti:

* **Azure Key Vault** -použít toosafeguard kryptografické klíče a tajné klíče používané pro proces šifrování/dešifrování hello disku. 
  * Pokud ano, můžete použít existující Azure Key Vault. Nemáte toodedicate disky tooencrypting Key Vault.
  * hranicemi správy tooseparate a klíče viditelnost, můžete vytvořit vyhrazený Key Vault.
* **Azure Active Directory** – obslužné rutiny hello zabezpečené výměna požadované kryptografické klíče a ověřování pro požadované akce. 
  * Pro uložení aplikace můžete obvykle použít existující instanci služby Azure Active Directory.
  * Hello instanční objekt poskytuje zabezpečené mechanismus toorequest a vydávají hello odpovídající kryptografické klíče. Nevyvíjíte skutečné aplikace, která se integruje se službou Azure Active Directory.

## <a name="requirements-and-limitations"></a>Požadavky a omezení
Podporované scénáře a požadavky na šifrování disku:

* Povoluje šifrování na nové virtuální počítače Windows z Azure Marketplace obrázky nebo vlastní image virtuálního pevného disku.
* Povoluje šifrování na stávajících virtuálních počítačích Windows v Azure.
* Povoluje šifrování na virtuálních počítačích Windows, které jsou nakonfigurované pomocí prostorů úložiště.
* Zakázáním šifrování na operačního systému a datové disky pro virtuální počítače Windows.
* Všechny prostředky (například Key Vault, účet úložiště a virtuálních počítačů) musí být v hello stejné oblasti Azure a předplatné.
* Úroveň Standard virtuálních počítačů, jako je například, D, DS, G a GS řadu virtuálních počítačů.

Šifrování disku aktuálně nepodporuje hello následující scénáře:

* Úroveň Basic virtuálních počítačů.
* Virtuální počítače vytvořené pomocí modelu nasazení Classic hello.
* Aktualizace hello kryptografické klíče již šifrované virtuálního počítače.
* Integrace s místní službu správy klíčů.

## <a name="create-azure-key-vault-and-keys"></a>Vytvoření Azure Key Vault a klíče
Než začnete, zkontrolujte, že hello nejnovější verzi hello byl nainstalován modul Azure PowerShell. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview). V rámci hello příkladech nahraďte všechny parametry příklad vlastní názvy, umístění a hodnoty klíče. Hello následující příklady použití konvence *myResourceGroup*, *myKeyVault*, *Můjvp*atd.

prvním krokem Hello je toocreate Azure Key Vault toostore kryptografických klíčů. Azure Key Vault můžete ukládat klíče, tajné klíče, nebo jejich hesla, které vám umožňují toosecurely implementaci v vašim aplikacím a službám. Šifrování virtuálního disku můžete vytvořit toostore Key Vault kryptografický klíč, který je použité tooencrypt nebo dešifrovat virtuální disky. 

Povolit hello Azure Key Vault zprostředkovatele v rámci vašeho předplatného Azure s [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), pak vytvořte skupinu prostředků s [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Hello následující příklad vytvoří název skupiny prostředků *myResourceGroup* v hello *východní USA* umístění:

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

Hello Azure Key Vault obsahující hello kryptografické klíče a přidružených výpočetních, které prostředky, jako je například úložiště a hello virtuální počítač se musí nacházet v hello stejné oblasti. Vytvoření Azure Key Vault s [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) a povolte hello Key Vault pro použití s šifrování disku. Zadejte jedinečný název pro Key Vault *keyVaultName* následujícím způsobem:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

Můžete uložit kryptografické klíče pomocí softwaru nebo ochrany modelu hardwarového zabezpečení (HSM). Použití modulu hardwarového zabezpečení vyžaduje premium Key Vault. Není další náklady toocreating premium Key Vault, nikoli standardní Key Vault, který ukládá klíče chráněné softwarem. toocreate premium Key Vault, v předchozím kroku hello přidat hello *- Sku "Premium"* parametry. Hello následující příklad používá klíče chráněné softwarem vzhledem k tomu, že jsme vytvořili standardní Key Vault. 

U obou modelů ochranu se musí hello platformy Azure toobe udělit přístup toorequest hello kryptografické klíče, když hello virtuální počítač spustí toodecrypt hello virtuální disky. Vytvoření kryptografické klíče v Key Vault s [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). Hello následující příklad vytvoří klíč s názvem *myKey*:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-hello-azure-active-directory-service-principal"></a>Vytvořit objekt služby Azure Active Directory hello
Když virtuální disky jsou zašifrovaná nebo dešifrovat, je třeba zadat ověření účtu toohandle hello a výměna kryptografických klíčů z Key Vault. Tento účet objektu zabezpečení služby Azure Active Directory umožňuje hello platformy Azure toorequest odpovídající kryptografické klíče hello jménem hello virtuálních počítačů. Výchozí instance služby Azure Active Directory je ve vašem předplatném dostupná, když máte mnoho organizací vyhrazené adresáře služby Azure Active Directory.

Vytvoření instančního objektu v Azure Active Directory s [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal). toospecify zabezpečeného hesla, postupujte podle hello [zásady hesel a omezení v Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

toosuccessfully zašifrovat nebo dešifrovat virtuální disky, oprávnění hello kryptografického klíče uložené v Key Vault musí být sada toopermit hello Azure Active Directory service hlavní tooread hello klíče. Nastavení oprávnění pro Key Vault s [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a>Vytvoření virtuálního počítače
tootest hello proces šifrování, můžeme vytvořit virtuální počítač. Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* pomocí *Windows Server 2016 Datacenter* bitové kopie:

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


## <a name="encrypt-virtual-machine"></a>Šifrování virtuálního počítače
tooencrypt hello virtuální disky, můžete seskupit všechny předchozí komponenty hello:

1. Zadejte hello objektu služby Azure Active Directory a heslo.
2. Zadejte hello Key Vault toostore hello metadata pro šifrované disky.
3. Zadejte hello toouse kryptografické klíče pro hello skutečné šifrování a dešifrování.
4. Zadejte, zda chcete disk tooencrypt hello operačního systému, hello datových disků nebo všechny.

Šifrování virtuálního počítače s [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) pomocí klíče hello Azure Key Vault a hlavní přihlašovací údaje služby Azure Active Directory. Hello následující příklad načte všechny klíčové informace hello potom šifruje hello virtuálního počítače s názvem *Můjvp*:

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

Přijetí výzvy toocontinue hello šifrováním hello virtuálních počítačů. během procesu hello restartuje Hello virtuálních počítačů. Po dokončení procesu hello šifrování a hello virtuální počítač byl restartován, zkontrolovat stav šifrování hello s [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

Hello výstup je podobné toohello následující ukázka:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Další kroky
* Další informace o správě Azure Key Vault najdete v tématu [nastavit Key Vault pro virtuální počítače](key-vault-setup.md).
* Další informace o šifrování disku, například při přípravě šifrované vlastní virtuální počítač tooupload tooAzure, najdete v části [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
