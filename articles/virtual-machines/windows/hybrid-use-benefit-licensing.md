---
title: "aaaAzure výhody použití hybridní pro Windows Server a klienta Windows | Microsoft Docs"
description: "Zjistěte, jak toomaximize vaše toobring výhody Windows Software Assurance místní licence tooAzure"
services: virtual-machines-windows
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 5/26/2017
ms.author: xujing
ms.openlocfilehash: f24487320a60132aaf766a31f3e6f3726d4a3bd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a>Výhody použití Azure hybridní pro Windows Server a klienta Windows
Pro zákazníky s programu Software Assurance, Azure hybridní použití Benefit vám umožní toouse licence systému Windows Server a klienta systému Windows na místě a spuštění systému Windows virtuálního počítače v Azure s malými náklady. Azure hybridní použití zvýhodnění pro systém Windows Server obsahuje Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016. Azure hybridní použití zvýhodnění pro klienta Windows zahrnuje Windows 10. Další informace najdete v tématu hello [licencování stránky Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

>[!IMPORTANT]
>Azure hybridní použití výhody pro klienta systému Windows je aktuálně ve verzi Preview pomocí bitové kopie hello Windows 10 v hello Azure Marketplace. Pouze podnikových zákazníků s Windows 10 Enterprise E3/E5 na uživatele nebo systému Windows VDA na uživatele (licence předplatného uživatele nebo licence předplatného uživatele rozšíření) ("opravňujících licencí") jsou způsobilé.
>
>

## <a name="ways-toouse-azure-hybrid-use-benefit"></a>Způsoby toouse výhody použití hybridní Azure
Existují několika různými způsoby toodeploy virtuálních počítačů Windows s hello Azure hybridní použití výhody:

1. Můžete nasadit virtuální počítače z [bitové kopie konkrétní Marketplace](#deploy-a-vm-using-the-azure-marketplace) , které jsou předem nakonfigurovaným rozhraním Azure hybridní použití zvýhodnění - Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1.
2. Můžete [nahrát vlastní virtuální](#upload-a-windows-vhd) a [nasazení pomocí šablony Resource Manageru](#deploy-a-vm-via-resource-manager) nebo [prostředí Azure PowerShell](#detailed-powershell-deployment-walkthrough).

## <a name="deploy-a-vm-using-hello-azure-marketplace"></a>Nasazení virtuálního počítače pomocí hello Azure Marketplace
Následující bitové kopie jsou k dispozici v hello předem nakonfigurované s výhody použití hybridní Azure Marketplace: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1. Tyto Image můžete nasadit přímo z hello portálu Azure, šablony Resource Manageru nebo Azure PowerShell.

Nasazením těchto bitových kopií přímo z hello portálu Azure. Pro použití v šablonách Resource Manageru a v prostředí Azure PowerShell zobrazení seznamu hello bitových kopií následujícím způsobem:

Windows Server:
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- Verze 2016 Datacenter 2016.127.20170406 nebo novější

- Verze 2012 R2 – Datacenter 4.127.20170406 nebo novější

- Verze 2012 Datacenter 3.127.20170406 nebo novější

- Verze aktualizace SP1 2008 R2 2.127.20170406 nebo novější

Pro klienta systému Windows:
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a>Nahrání virtuálního pevného disku serveru systému Windows
toodeploy virtuálního počítače Windows serveru v Azure, musíte nejdřív toocreate virtuální pevný disk, který obsahuje vaše základní build Windows. Tento virtuální pevný disk musí před nahráním tooAzure odpovídajícím způsobem připravit pomocí nástroje Sysprep. Můžete [Další informace o požadavcích hello virtuálního pevného disku a procesu Sysprep](upload-generalized-managed.md) a [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Zálohujte hello virtuálních počítačů před spuštěním nástroje Sysprep. 

Zajistěte, aby byla [instalace a konfigurace hello nejnovější Azure PowerShell](/powershell/azure/overview). Jakmile připravíte svůj disk VHD, nahrajte hello virtuálního pevného disku tooyour účtu úložiště Azure pomocí hello `Add-AzureRmVhd` rutiny následujícím způsobem:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server a Dynamics můžete také používat váš Software Assurance licencování. Stále potřebujete bitové kopie systému Windows Server hello tooprepare instalace součásti vaší aplikace a odpovídajícím způsobem poskytování aktivační kódy VLK a poté odesílání tooAzure bitové kopie disku hello. Si přečtěte hello příslušné dokumentaci ke spuštění nástroje Sysprep s vaší aplikací, jako například [důležité informace týkající se instalace systému SQL Server pomocí nástroje Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) nebo [sestavení SharePoint serveru 2016 referenční bitové kopie (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Také další informace o [odesílání proces tooAzure hello virtuálního pevného disku](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)


## <a name="deploy-a-vm-via-resource-manager-template"></a>Nasazení virtuálního počítače prostřednictvím šablony Resource Manageru
V rámci šablony Resource Manageru, dalším parametr pro `licenseType` lze zadat. Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md). Až budete mít vaše tooAzure nahrát virtuální pevný disk, upravte je typ licence hello tooinclude šablony správce prostředků jako součást hello výpočetní zprostředkovatele a nasazení vaší šablony jako za normálních okolností:

Windows Server:
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

Pro klienta Windows toouse Azure Marketplace Image pouze:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Nasazení virtuálního počítače pomocí rychlého startu prostředí PowerShell
Při nasazování virtuálního počítače Windows serveru pomocí prostředí PowerShell, máte další parametr pro `-LicenseType`. Až budete mít vaše tooAzure nahrát virtuální pevný disk, můžete vytvořit virtuální počítač pomocí `New-AzureRmVM` a zadejte typ licencování hello následujícím způsobem:

Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Pro klienta Windows toouse Azure Marketplace Image pouze:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

Můžete [přečíst podrobnější návod k nasazení virtuálních počítačů v Azure pomocí prostředí PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) níže nebo čtení popisnější příručce na hello různé kroky příliš[vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="verify-your-vm-is-utilizing-hello-licensing-benefit"></a>Ověřte, zda že virtuální počítač je využitím licencování benefit hello
Po nasazení virtuálního počítače prostřednictvím hello prostředí PowerShell nebo metodu nasazení Resource Manager ověřit hello typ licence s `Get-AzureRmVM` následujícím způsobem:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Hello výstup je podobné toohello následující ukázka pro systém Windows Server:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Tento výstup se liší od hello následující virtuální počítač nasazen bez licencování Azure hybridní použití zvýhodnění, jako je například virtuální počítač nasadit přímo z Galerie Azure hello:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a>Podrobný postup nasazení prostředí PowerShell
Hello následující podrobné kroky zobrazit prostředí PowerShell úplné nasazení virtuálního počítače. Můžete si přečíst další kontext jako skutečný rutiny toohello a různé součásti vytváří v [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Krok procesem vytvoření vaší skupiny prostředků, účet úložiště a virtuální sítě, pak zadejte virtuální počítač a nakonec vytvořte virtuální počítač.

Nejprve bezpečně získat přihlašovací údaje, nastavte umístění a název skupiny prostředků:

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

Vytvoření veřejné IP adresy:

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

Definujte vaší virtuální sítě, podsítě a síťový adaptér:

```powershell
$subnetName = "mySubnet"
$nicName = "myNIC"
$vnetName = "myVnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location `
    -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Název virtuálního počítače a vytvoření konfigurace virtuálního počítače:

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Definujte operačním systému:

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Přidejte toohello váš síťový adaptér virtuálního počítače:

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Definujte toouse účet úložiště hello:

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

Nahrát svůj disk VHD, vhodně připravit a připojte tooyour virtuálních počítačů pro použití:

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Nakonec vytvořte virtuální počítač a definovat hello licencování typ tooutilize výhody použití hybridní Azure:

Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a>Nasazení škálování virtuálních počítačů, nastavit pomocí šablony Resource Manageru
V rámci šablony správce prostředků VMSS dalším parametr pro `licenseType` musí být zadán. Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md). Upravit vlastnost licenseType tooinclude hello váš správce prostředků šablony jako součást virtualMachineProfile hello škálovací sadu a nasazení vaší šablony jako normální – viz níže pomocí bitové kopie systému Windows Server 2016 příkladu:


```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```

> [!NOTE]
> Podpora pro nasazení škálování virtuálních počítačů s AHUB výhody pomocí prostředí PowerShell a další nástroje SDK se připravuje.
>

## <a name="next-steps"></a>Další kroky
Další informace o [licencování Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Další informace o [pomocí šablony Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

Další informace o [výhody použití hybridní Azure a Azure Site Recovery zkontrolujte migrace tooAzure aplikace i více nákladově efektivní](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).
