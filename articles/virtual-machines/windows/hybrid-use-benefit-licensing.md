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
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a><span data-ttu-id="be05a-103">Výhody použití Azure hybridní pro Windows Server a klienta Windows</span><span class="sxs-lookup"><span data-stu-id="be05a-103">Azure Hybrid Use Benefit for Windows Server and Windows Client</span></span>
<span data-ttu-id="be05a-104">Pro zákazníky s programu Software Assurance, Azure hybridní použití Benefit vám umožní toouse licence systému Windows Server a klienta systému Windows na místě a spuštění systému Windows virtuálního počítače v Azure s malými náklady.</span><span class="sxs-lookup"><span data-stu-id="be05a-104">For customers with Software Assurance, Azure Hybrid Use Benefit allows you toouse your on-premises Windows Server and Windows Client licenses and run Windows virtual machines in Azure at a reduced cost.</span></span> <span data-ttu-id="be05a-105">Azure hybridní použití zvýhodnění pro systém Windows Server obsahuje Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="be05a-105">Azure Hybrid Use Benefit for Windows Server includes Windows Server 2008R2, Windows Server 2012, Windows Server 2012R2, and Windows Server 2016.</span></span> <span data-ttu-id="be05a-106">Azure hybridní použití zvýhodnění pro klienta Windows zahrnuje Windows 10.</span><span class="sxs-lookup"><span data-stu-id="be05a-106">Azure Hybrid Use Benefit for Windows Client includes Windows 10.</span></span> <span data-ttu-id="be05a-107">Další informace najdete v tématu hello [licencování stránky Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="be05a-107">For more information, please see hello [Azure Hybrid Use Benefit licensing page](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

>[!IMPORTANT]
><span data-ttu-id="be05a-108">Azure hybridní použití výhody pro klienta systému Windows je aktuálně ve verzi Preview pomocí bitové kopie hello Windows 10 v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="be05a-108">Azure Hybrid Use Benefits for Windows Client is currently in Preview using hello Windows 10 image in hello Azure Marketplace.</span></span> <span data-ttu-id="be05a-109">Pouze podnikových zákazníků s Windows 10 Enterprise E3/E5 na uživatele nebo systému Windows VDA na uživatele (licence předplatného uživatele nebo licence předplatného uživatele rozšíření) ("opravňujících licencí") jsou způsobilé.</span><span class="sxs-lookup"><span data-stu-id="be05a-109">Only Enterprise customers with Windows 10 Enterprise E3/E5 per user or Windows VDA per user (User Subscription Licenses or Add-on User Subscription Licenses) (“Qualifying Licenses”) are eligible.</span></span>
>
>

## <a name="ways-toouse-azure-hybrid-use-benefit"></a><span data-ttu-id="be05a-110">Způsoby toouse výhody použití hybridní Azure</span><span class="sxs-lookup"><span data-stu-id="be05a-110">Ways toouse Azure Hybrid Use Benefit</span></span>
<span data-ttu-id="be05a-111">Existují několika různými způsoby toodeploy virtuálních počítačů Windows s hello Azure hybridní použití výhody:</span><span class="sxs-lookup"><span data-stu-id="be05a-111">There are a couple of different ways toodeploy Windows VMs with hello Azure Hybrid Use Benefit:</span></span>

1. <span data-ttu-id="be05a-112">Můžete nasadit virtuální počítače z [bitové kopie konkrétní Marketplace](#deploy-a-vm-using-the-azure-marketplace) , které jsou předem nakonfigurovaným rozhraním Azure hybridní použití zvýhodnění - Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="be05a-112">You can deploy VMs from [specific Marketplace images](#deploy-a-vm-using-the-azure-marketplace) that are pre-configured with Azure Hybrid Use Benefit - Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span>
2. <span data-ttu-id="be05a-113">Můžete [nahrát vlastní virtuální](#upload-a-windows-vhd) a [nasazení pomocí šablony Resource Manageru](#deploy-a-vm-via-resource-manager) nebo [prostředí Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="be05a-113">You can [upload a custom VM](#upload-a-windows-vhd) and [deploy using a Resource Manager template](#deploy-a-vm-via-resource-manager) or [Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span></span>

## <a name="deploy-a-vm-using-hello-azure-marketplace"></a><span data-ttu-id="be05a-114">Nasazení virtuálního počítače pomocí hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="be05a-114">Deploy a VM using hello Azure Marketplace</span></span>
<span data-ttu-id="be05a-115">Následující bitové kopie jsou k dispozici v hello předem nakonfigurované s výhody použití hybridní Azure Marketplace: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="be05a-115">Following images are available in hello Marketplace pre-configured with Azure Hybrid Use Benefit: Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span> <span data-ttu-id="be05a-116">Tyto Image můžete nasadit přímo z hello portálu Azure, šablony Resource Manageru nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="be05a-116">These images can be deployed directly from hello Azure portal, Resource Manager templates, or Azure PowerShell.</span></span>

<span data-ttu-id="be05a-117">Nasazením těchto bitových kopií přímo z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="be05a-117">You can deploy these images directly from hello Azure portal.</span></span> <span data-ttu-id="be05a-118">Pro použití v šablonách Resource Manageru a v prostředí Azure PowerShell zobrazení seznamu hello bitových kopií následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be05a-118">For use in Resource Manager templates and with Azure PowerShell, view hello list of images as follows:</span></span>

<span data-ttu-id="be05a-119">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="be05a-119">For Windows Server:</span></span>
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- <span data-ttu-id="be05a-120">Verze 2016 Datacenter 2016.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="be05a-120">2016-Datacenter version 2016.127.20170406 or above</span></span>

- <span data-ttu-id="be05a-121">Verze 2012 R2 – Datacenter 4.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="be05a-121">2012-R2-Datacenter version 4.127.20170406 or above</span></span>

- <span data-ttu-id="be05a-122">Verze 2012 Datacenter 3.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="be05a-122">2012-Datacenter version 3.127.20170406 or above</span></span>

- <span data-ttu-id="be05a-123">Verze aktualizace SP1 2008 R2 2.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="be05a-123">2008-R2-SP1 version 2.127.20170406 or above</span></span>

<span data-ttu-id="be05a-124">Pro klienta systému Windows:</span><span class="sxs-lookup"><span data-stu-id="be05a-124">For Windows Client:</span></span>
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a><span data-ttu-id="be05a-125">Nahrání virtuálního pevného disku serveru systému Windows</span><span class="sxs-lookup"><span data-stu-id="be05a-125">Upload a Windows Server VHD</span></span>
<span data-ttu-id="be05a-126">toodeploy virtuálního počítače Windows serveru v Azure, musíte nejdřív toocreate virtuální pevný disk, který obsahuje vaše základní build Windows.</span><span class="sxs-lookup"><span data-stu-id="be05a-126">toodeploy a Windows Server VM in Azure, you first need toocreate a VHD that contains your base Windows build.</span></span> <span data-ttu-id="be05a-127">Tento virtuální pevný disk musí před nahráním tooAzure odpovídajícím způsobem připravit pomocí nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="be05a-127">This VHD must be appropriately prepared via Sysprep before you upload it tooAzure.</span></span> <span data-ttu-id="be05a-128">Můžete [Další informace o požadavcích hello virtuálního pevného disku a procesu Sysprep](upload-generalized-managed.md) a [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="be05a-128">You can [read more about hello VHD requirements and Sysprep process](upload-generalized-managed.md) and [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span> <span data-ttu-id="be05a-129">Zálohujte hello virtuálních počítačů před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="be05a-129">Back up hello VM before running Sysprep.</span></span> 

<span data-ttu-id="be05a-130">Zajistěte, aby byla [instalace a konfigurace hello nejnovější Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="be05a-130">Make sure you have [installed and configured hello latest Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="be05a-131">Jakmile připravíte svůj disk VHD, nahrajte hello virtuálního pevného disku tooyour účtu úložiště Azure pomocí hello `Add-AzureRmVhd` rutiny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be05a-131">Once you have prepared your VHD, upload hello VHD tooyour Azure Storage account using hello `Add-AzureRmVhd` cmdlet as follows:</span></span>

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> <span data-ttu-id="be05a-132">Microsoft SQL Server, SharePoint Server a Dynamics můžete také používat váš Software Assurance licencování.</span><span class="sxs-lookup"><span data-stu-id="be05a-132">Microsoft SQL Server, SharePoint Server, and Dynamics can also utilize your Software Assurance licensing.</span></span> <span data-ttu-id="be05a-133">Stále potřebujete bitové kopie systému Windows Server hello tooprepare instalace součásti vaší aplikace a odpovídajícím způsobem poskytování aktivační kódy VLK a poté odesílání tooAzure bitové kopie disku hello.</span><span class="sxs-lookup"><span data-stu-id="be05a-133">You still need tooprepare hello Windows Server image by installing your application components and providing license keys accordingly, then uploading hello disk image tooAzure.</span></span> <span data-ttu-id="be05a-134">Si přečtěte hello příslušné dokumentaci ke spuštění nástroje Sysprep s vaší aplikací, jako například [důležité informace týkající se instalace systému SQL Server pomocí nástroje Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) nebo [sestavení SharePoint serveru 2016 referenční bitové kopie (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span><span class="sxs-lookup"><span data-stu-id="be05a-134">Review hello appropriate documentation for running Sysprep with your application, such as [Considerations for Installing SQL Server using Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) or [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span></span>
>
>

<span data-ttu-id="be05a-135">Také další informace o [odesílání proces tooAzure hello virtuálního pevného disku](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span><span class="sxs-lookup"><span data-stu-id="be05a-135">You can also read more about [uploading hello VHD tooAzure process](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span></span>


## <a name="deploy-a-vm-via-resource-manager-template"></a><span data-ttu-id="be05a-136">Nasazení virtuálního počítače prostřednictvím šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="be05a-136">Deploy a VM via Resource Manager Template</span></span>
<span data-ttu-id="be05a-137">V rámci šablony Resource Manageru, dalším parametr pro `licenseType` lze zadat.</span><span class="sxs-lookup"><span data-stu-id="be05a-137">Within your Resource Manager templates, an additional parameter for `licenseType` can be specified.</span></span> <span data-ttu-id="be05a-138">Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="be05a-138">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="be05a-139">Až budete mít vaše tooAzure nahrát virtuální pevný disk, upravte je typ licence hello tooinclude šablony správce prostředků jako součást hello výpočetní zprostředkovatele a nasazení vaší šablony jako za normálních okolností:</span><span class="sxs-lookup"><span data-stu-id="be05a-139">Once you have your VHD uploaded tooAzure, edit you Resource Manager template tooinclude hello license type as part of hello compute provider and deploy your template as normal:</span></span>

<span data-ttu-id="be05a-140">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="be05a-140">For Windows Server:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

<span data-ttu-id="be05a-141">Pro klienta Windows toouse Azure Marketplace Image pouze:</span><span class="sxs-lookup"><span data-stu-id="be05a-141">For Windows Client toouse with Azure Marketplace Image only:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a><span data-ttu-id="be05a-142">Nasazení virtuálního počítače pomocí rychlého startu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="be05a-142">Deploy a VM via PowerShell quickstart</span></span>
<span data-ttu-id="be05a-143">Při nasazování virtuálního počítače Windows serveru pomocí prostředí PowerShell, máte další parametr pro `-LicenseType`.</span><span class="sxs-lookup"><span data-stu-id="be05a-143">When deploying your Windows Server VM via PowerShell, you have an additional parameter for `-LicenseType`.</span></span> <span data-ttu-id="be05a-144">Až budete mít vaše tooAzure nahrát virtuální pevný disk, můžete vytvořit virtuální počítač pomocí `New-AzureRmVM` a zadejte typ licencování hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be05a-144">Once you have your VHD uploaded tooAzure, you create a VM using `New-AzureRmVM` and specify hello licensing type as follows:</span></span>

<span data-ttu-id="be05a-145">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="be05a-145">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

<span data-ttu-id="be05a-146">Pro klienta Windows toouse Azure Marketplace Image pouze:</span><span class="sxs-lookup"><span data-stu-id="be05a-146">For Windows Client toouse with Azure Marketplace Image only:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

<span data-ttu-id="be05a-147">Můžete [přečíst podrobnější návod k nasazení virtuálních počítačů v Azure pomocí prostředí PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) níže nebo čtení popisnější příručce na hello různé kroky příliš[vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be05a-147">You can [read a more detailed walkthrough on deploying a VM in Azure via PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) below, or read a more descriptive guide on hello different steps too[create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="verify-your-vm-is-utilizing-hello-licensing-benefit"></a><span data-ttu-id="be05a-148">Ověřte, zda že virtuální počítač je využitím licencování benefit hello</span><span class="sxs-lookup"><span data-stu-id="be05a-148">Verify your VM is utilizing hello licensing benefit</span></span>
<span data-ttu-id="be05a-149">Po nasazení virtuálního počítače prostřednictvím hello prostředí PowerShell nebo metodu nasazení Resource Manager ověřit hello typ licence s `Get-AzureRmVM` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="be05a-149">Once you have deployed your VM through either hello PowerShell or Resource Manager deployment method, verify hello license type with `Get-AzureRmVM` as follows:</span></span>

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="be05a-150">Hello výstup je podobné toohello následující ukázka pro systém Windows Server:</span><span class="sxs-lookup"><span data-stu-id="be05a-150">hello output is similar toohello following example for Windows Server:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

<span data-ttu-id="be05a-151">Tento výstup se liší od hello následující virtuální počítač nasazen bez licencování Azure hybridní použití zvýhodnění, jako je například virtuální počítač nasadit přímo z Galerie Azure hello:</span><span class="sxs-lookup"><span data-stu-id="be05a-151">This output contrasts with hello following VM deployed without Azure Hybrid Use Benefit licensing, such as a VM deployed straight from hello Azure Gallery:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a><span data-ttu-id="be05a-152">Podrobný postup nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="be05a-152">Detailed PowerShell deployment walkthrough</span></span>
<span data-ttu-id="be05a-153">Hello následující podrobné kroky zobrazit prostředí PowerShell úplné nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="be05a-153">hello following detailed PowerShell steps show a full deployment of a VM.</span></span> <span data-ttu-id="be05a-154">Můžete si přečíst další kontext jako skutečný rutiny toohello a různé součásti vytváří v [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be05a-154">You can read more context as toohello actual cmdlets and different components being created in [Create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="be05a-155">Krok procesem vytvoření vaší skupiny prostředků, účet úložiště a virtuální sítě, pak zadejte virtuální počítač a nakonec vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="be05a-155">You step through creating your resource group, storage account, and virtual networking, then define your VM and finally create your VM.</span></span>

<span data-ttu-id="be05a-156">Nejprve bezpečně získat přihlašovací údaje, nastavte umístění a název skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="be05a-156">First, securely obtain credentials, set a location, and resource group name:</span></span>

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

<span data-ttu-id="be05a-157">Vytvoření veřejné IP adresy:</span><span class="sxs-lookup"><span data-stu-id="be05a-157">Create a public IP:</span></span>

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

<span data-ttu-id="be05a-158">Definujte vaší virtuální sítě, podsítě a síťový adaptér:</span><span class="sxs-lookup"><span data-stu-id="be05a-158">Define your subnet, NIC, and VNET:</span></span>

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

<span data-ttu-id="be05a-159">Název virtuálního počítače a vytvoření konfigurace virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="be05a-159">Name your VM and create a VM config:</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

<span data-ttu-id="be05a-160">Definujte operačním systému:</span><span class="sxs-lookup"><span data-stu-id="be05a-160">Define your OS:</span></span>

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="be05a-161">Přidejte toohello váš síťový adaptér virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="be05a-161">Add your NIC toohello VM:</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="be05a-162">Definujte toouse účet úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="be05a-162">Define hello storage account toouse:</span></span>

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

<span data-ttu-id="be05a-163">Nahrát svůj disk VHD, vhodně připravit a připojte tooyour virtuálních počítačů pro použití:</span><span class="sxs-lookup"><span data-stu-id="be05a-163">Upload your VHD, suitably prepared, and attach tooyour VM for use:</span></span>

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

<span data-ttu-id="be05a-164">Nakonec vytvořte virtuální počítač a definovat hello licencování typ tooutilize výhody použití hybridní Azure:</span><span class="sxs-lookup"><span data-stu-id="be05a-164">Finally, create your VM and define hello licensing type tooutilize Azure Hybrid Use Benefit:</span></span>

<span data-ttu-id="be05a-165">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="be05a-165">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a><span data-ttu-id="be05a-166">Nasazení škálování virtuálních počítačů, nastavit pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="be05a-166">Deploy a virtual machine scale set via Resource Manager template</span></span>
<span data-ttu-id="be05a-167">V rámci šablony správce prostředků VMSS dalším parametr pro `licenseType` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="be05a-167">Within your VMSS Resource Manager templates, an additional parameter for `licenseType` must be specified.</span></span> <span data-ttu-id="be05a-168">Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="be05a-168">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="be05a-169">Upravit vlastnost licenseType tooinclude hello váš správce prostředků šablony jako součást virtualMachineProfile hello škálovací sadu a nasazení vaší šablony jako normální – viz níže pomocí bitové kopie systému Windows Server 2016 příkladu:</span><span class="sxs-lookup"><span data-stu-id="be05a-169">Edit your Resource Manager template tooinclude hello licenseType property as part of hello scale set’s virtualMachineProfile and deploy your template as normal - see example below using 2016 Windows Server image:</span></span>


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
> <span data-ttu-id="be05a-170">Podpora pro nasazení škálování virtuálních počítačů s AHUB výhody pomocí prostředí PowerShell a další nástroje SDK se připravuje.</span><span class="sxs-lookup"><span data-stu-id="be05a-170">Support for deploying a virtual machine scale set with AHUB benefits through PowerShell and other SDK tools is coming soon.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="be05a-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be05a-171">Next steps</span></span>
<span data-ttu-id="be05a-172">Další informace o [licencování Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="be05a-172">Read more about [Azure Hybrid Use Benefit licensing](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

<span data-ttu-id="be05a-173">Další informace o [pomocí šablony Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="be05a-173">Learn more about [using Resource Manager templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="be05a-174">Další informace o [výhody použití hybridní Azure a Azure Site Recovery zkontrolujte migrace tooAzure aplikace i více nákladově efektivní](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span><span class="sxs-lookup"><span data-stu-id="be05a-174">Learn more about [Azure Hybrid Use Benefit and Azure Site Recovery make migrating applications tooAzure even more cost-effective](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span></span>
