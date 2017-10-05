---
title: "Výhody použití Azure hybridní pro Windows Server a klienta Windows | Microsoft Docs"
description: "Zjistěte, jak chcete maximalizovat své výhody Windows Software Assurance a dovést místní licencí do Azure"
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
ms.openlocfilehash: 210635624946ddb293427167e9d476c377bcc9b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a>Výhody použití Azure hybridní pro Windows Server a klienta Windows
Pro zákazníky s programu Software Assurance Azure hybridní použít Benefit vám umožní použít místní systém Windows Server a klienta Windows licencí a spustit virtuální počítače s Windows v Azure s malými náklady. Azure hybridní použití zvýhodnění pro systém Windows Server obsahuje Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016. Azure hybridní použití zvýhodnění pro klienta Windows zahrnuje Windows 10. Další informace najdete v tématu [licencování stránky Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

>[!IMPORTANT]
>Azure hybridní použití výhody pro klienta systému Windows je aktuálně ve verzi Preview pomocí bitové kopie systému Windows 10 v Azure Marketplace. Pouze podnikových zákazníků s Windows 10 Enterprise E3/E5 na uživatele nebo systému Windows VDA na uživatele (licence předplatného uživatele nebo licence předplatného uživatele rozšíření) ("opravňujících licencí") jsou způsobilé.
>
>

## <a name="ways-to-use-azure-hybrid-use-benefit"></a>Způsoby použití Azure hybridní použít výhody
Existuje několik různých způsobů pro nasazení virtuálních počítačů Windows s výhodou použití hybridní Azure:

1. Můžete nasadit virtuální počítače z [bitové kopie konkrétní Marketplace](#deploy-a-vm-using-the-azure-marketplace) , které jsou předem nakonfigurovaným rozhraním Azure hybridní použití zvýhodnění - Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1.
2. Můžete [nahrát vlastní virtuální](#upload-a-windows-vhd) a [nasazení pomocí šablony Resource Manageru](#deploy-a-vm-via-resource-manager) nebo [prostředí Azure PowerShell](#detailed-powershell-deployment-walkthrough).

## <a name="deploy-a-vm-using-the-azure-marketplace"></a>Nasazení virtuálního počítače pomocí Azure Marketplace
Následující bitové kopie jsou k dispozici na webu Marketplace předem nakonfigurované s Azure hybridní použití zvýhodnění: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1. Tyto image je možné nasadit přímo z webu Azure Portal, šablon Resource Manageru nebo Azure PowerShellu.

Nasazením těchto bitových kopií přímo z portálu Azure. Pro použití v šablonách Resource Manageru a v prostředí Azure PowerShell zobrazí se seznam bitové kopie následujícím způsobem:

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
Pokud chcete nasadit virtuální počítač Windows serveru v Azure, musíte nejprve vytvořit virtuální pevný disk, který obsahuje vaše základní build Windows. Tento virtuální pevný disk musí být správně připravena pomocí nástroje Sysprep, před nahráním do Azure. Můžete [Další informace o požadavcích virtuálního pevného disku a procesu Sysprep](upload-generalized-managed.md) a [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Zálohování virtuálního počítače před spuštěním nástroje Sysprep. 

Zajistěte, aby byla [nainstalovaný a nakonfigurovaný nejnovější prostředí Azure PowerShell](/powershell/azure/overview). Jakmile připravíte svůj disk VHD, odeslání disku VHD do vašeho účtu úložiště Azure pomocí `Add-AzureRmVhd` rutiny následujícím způsobem:

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> Microsoft SQL Server, SharePoint Server a Dynamics můžete také používat váš Software Assurance licencování. Stále je nutné připravit bitovou kopii systému Windows Server tak, že instalace součásti vaší aplikace a poskytování aktivační kódy VLK odpovídajícím způsobem pak odesílání bitové kopie disku do Azure. Zkontrolujte v příslušné dokumentaci ke spuštění nástroje Sysprep s vaší aplikací, jako například [důležité informace týkající se instalace systému SQL Server pomocí nástroje Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) nebo [sestavení SharePoint serveru 2016 referenční bitové kopie (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).
>
>

Také další informace o [nahrávání virtuální pevný disk do Azure procesu](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)


## <a name="deploy-a-vm-via-resource-manager-template"></a>Nasazení virtuálního počítače prostřednictvím šablony Resource Manageru
V rámci šablony Resource Manageru, dalším parametr pro `licenseType` lze zadat. Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md). Až budete mít svůj disk VHD nahrán do Azure, upravte šablony Resource Manageru zahrnout typ licence jako součást poskytovatele výpočetních a nasazení vaší šablony jako za normálních okolností:

Windows Server:
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

Pro klienta systému Windows pro použití s Azure Marketplace Image pouze:
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a>Nasazení virtuálního počítače pomocí rychlého startu prostředí PowerShell
Při nasazování virtuálního počítače Windows serveru pomocí prostředí PowerShell, máte další parametr pro `-LicenseType`. Až budete mít svůj disk VHD nahrán do Azure, vytvoříte virtuální počítač pomocí `New-AzureRmVM` a zadejte typ licencování následujícím způsobem:

Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

Pro klienta systému Windows pro použití s Azure Marketplace Image pouze:
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

Můžete [přečíst podrobnější návod k nasazení virtuálních počítačů v Azure pomocí prostředí PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) níže, nebo si můžete přečíst více popisný příručce na různé kroky, které [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Ověřte, zda že je virtuální počítač využívá licencování benefit
Po nasazení virtuálního počítače prostřednictvím buď prostředí PowerShell nebo správce prostředků metodu nasazení ověřte typ licence s `Get-AzureRmVM` následujícím způsobem:

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Výstup bude vypadat v následujícím příkladu pro systém Windows Server:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Tento výstup uvádí vedle sebe s následující virtuální počítač nasadit bez licencování Azure hybridní použití výhody, jako je například virtuální počítač nasadit přímo z Galerie Azure:

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a>Podrobný postup nasazení prostředí PowerShell
Zobrazit následující podrobné kroky prostředí PowerShell úplné nasazení virtuálního počítače. Můžete si přečíst další kontext, skutečný rutiny a vytváří v různých komponent [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Krok procesem vytvoření vaší skupiny prostředků, účet úložiště a virtuální sítě, pak zadejte virtuální počítač a nakonec vytvořte virtuální počítač.

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

Do virtuálního počítače přidejte svoji síťovou kartu:

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Zadejte účet úložiště, který chcete použít:

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

Nahrát svůj disk VHD, vhodně připravit a připojte k virtuálnímu počítači pro použití:

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Nakonec vytvořte virtuální počítač a definování licencování typu využívat výhody použití hybridní Azure:

Windows Server:
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a>Nasazení škálování virtuálních počítačů, nastavit pomocí šablony Resource Manageru
V rámci šablony správce prostředků VMSS dalším parametr pro `licenseType` musí být zadán. Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md). Upravte svou šablonu Resource Manager zahrnout vlastnost licenseType jako součást virtualMachineProfile byly sadou škálování a nasazení vaší šablony jako normální-najdete v příkladu níže pomocí bitové kopie systému Windows Server 2016:


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

Další informace o [výhody použití hybridní Azure a Azure Site Recovery zkontrolujte migrace aplikací do Azure i více nákladově efektivní](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).
