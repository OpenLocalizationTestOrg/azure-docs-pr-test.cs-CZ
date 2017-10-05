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
# <a name="azure-hybrid-use-benefit-for-windows-server-and-windows-client"></a><span data-ttu-id="1a2db-103">Výhody použití Azure hybridní pro Windows Server a klienta Windows</span><span class="sxs-lookup"><span data-stu-id="1a2db-103">Azure Hybrid Use Benefit for Windows Server and Windows Client</span></span>
<span data-ttu-id="1a2db-104">Pro zákazníky s programu Software Assurance Azure hybridní použít Benefit vám umožní použít místní systém Windows Server a klienta Windows licencí a spustit virtuální počítače s Windows v Azure s malými náklady.</span><span class="sxs-lookup"><span data-stu-id="1a2db-104">For customers with Software Assurance, Azure Hybrid Use Benefit allows you to use your on-premises Windows Server and Windows Client licenses and run Windows virtual machines in Azure at a reduced cost.</span></span> <span data-ttu-id="1a2db-105">Azure hybridní použití zvýhodnění pro systém Windows Server obsahuje Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="1a2db-105">Azure Hybrid Use Benefit for Windows Server includes Windows Server 2008R2, Windows Server 2012, Windows Server 2012R2, and Windows Server 2016.</span></span> <span data-ttu-id="1a2db-106">Azure hybridní použití zvýhodnění pro klienta Windows zahrnuje Windows 10.</span><span class="sxs-lookup"><span data-stu-id="1a2db-106">Azure Hybrid Use Benefit for Windows Client includes Windows 10.</span></span> <span data-ttu-id="1a2db-107">Další informace najdete v tématu [licencování stránky Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="1a2db-107">For more information, please see the [Azure Hybrid Use Benefit licensing page](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

>[!IMPORTANT]
><span data-ttu-id="1a2db-108">Azure hybridní použití výhody pro klienta systému Windows je aktuálně ve verzi Preview pomocí bitové kopie systému Windows 10 v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="1a2db-108">Azure Hybrid Use Benefits for Windows Client is currently in Preview using the Windows 10 image in the Azure Marketplace.</span></span> <span data-ttu-id="1a2db-109">Pouze podnikových zákazníků s Windows 10 Enterprise E3/E5 na uživatele nebo systému Windows VDA na uživatele (licence předplatného uživatele nebo licence předplatného uživatele rozšíření) ("opravňujících licencí") jsou způsobilé.</span><span class="sxs-lookup"><span data-stu-id="1a2db-109">Only Enterprise customers with Windows 10 Enterprise E3/E5 per user or Windows VDA per user (User Subscription Licenses or Add-on User Subscription Licenses) (“Qualifying Licenses”) are eligible.</span></span>
>
>

## <a name="ways-to-use-azure-hybrid-use-benefit"></a><span data-ttu-id="1a2db-110">Způsoby použití Azure hybridní použít výhody</span><span class="sxs-lookup"><span data-stu-id="1a2db-110">Ways to use Azure Hybrid Use Benefit</span></span>
<span data-ttu-id="1a2db-111">Existuje několik různých způsobů pro nasazení virtuálních počítačů Windows s výhodou použití hybridní Azure:</span><span class="sxs-lookup"><span data-stu-id="1a2db-111">There are a couple of different ways to deploy Windows VMs with the Azure Hybrid Use Benefit:</span></span>

1. <span data-ttu-id="1a2db-112">Můžete nasadit virtuální počítače z [bitové kopie konkrétní Marketplace](#deploy-a-vm-using-the-azure-marketplace) , které jsou předem nakonfigurovaným rozhraním Azure hybridní použití zvýhodnění - Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="1a2db-112">You can deploy VMs from [specific Marketplace images](#deploy-a-vm-using-the-azure-marketplace) that are pre-configured with Azure Hybrid Use Benefit - Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span>
2. <span data-ttu-id="1a2db-113">Můžete [nahrát vlastní virtuální](#upload-a-windows-vhd) a [nasazení pomocí šablony Resource Manageru](#deploy-a-vm-via-resource-manager) nebo [prostředí Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span><span class="sxs-lookup"><span data-stu-id="1a2db-113">You can [upload a custom VM](#upload-a-windows-vhd) and [deploy using a Resource Manager template](#deploy-a-vm-via-resource-manager) or [Azure PowerShell](#detailed-powershell-deployment-walkthrough).</span></span>

## <a name="deploy-a-vm-using-the-azure-marketplace"></a><span data-ttu-id="1a2db-114">Nasazení virtuálního počítače pomocí Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="1a2db-114">Deploy a VM using the Azure Marketplace</span></span>
<span data-ttu-id="1a2db-115">Následující bitové kopie jsou k dispozici na webu Marketplace předem nakonfigurované s Azure hybridní použití zvýhodnění: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 a Windows Server 2008SP1.</span><span class="sxs-lookup"><span data-stu-id="1a2db-115">Following images are available in the Marketplace pre-configured with Azure Hybrid Use Benefit: Windows Server 2016, Windows Server 2012R2, Windows Server 2012 and Windows Server 2008SP1.</span></span> <span data-ttu-id="1a2db-116">Tyto image je možné nasadit přímo z webu Azure Portal, šablon Resource Manageru nebo Azure PowerShellu.</span><span class="sxs-lookup"><span data-stu-id="1a2db-116">These images can be deployed directly from the Azure portal, Resource Manager templates, or Azure PowerShell.</span></span>

<span data-ttu-id="1a2db-117">Nasazením těchto bitových kopií přímo z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1a2db-117">You can deploy these images directly from the Azure portal.</span></span> <span data-ttu-id="1a2db-118">Pro použití v šablonách Resource Manageru a v prostředí Azure PowerShell zobrazí se seznam bitové kopie následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1a2db-118">For use in Resource Manager templates and with Azure PowerShell, view the list of images as follows:</span></span>

<span data-ttu-id="1a2db-119">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="1a2db-119">For Windows Server:</span></span>
```powershell
Get-AzureRmVMImagesku -Location westus -PublisherName MicrosoftWindowsServer -Offer WindowsServer
```
- <span data-ttu-id="1a2db-120">Verze 2016 Datacenter 2016.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="1a2db-120">2016-Datacenter version 2016.127.20170406 or above</span></span>

- <span data-ttu-id="1a2db-121">Verze 2012 R2 – Datacenter 4.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="1a2db-121">2012-R2-Datacenter version 4.127.20170406 or above</span></span>

- <span data-ttu-id="1a2db-122">Verze 2012 Datacenter 3.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="1a2db-122">2012-Datacenter version 3.127.20170406 or above</span></span>

- <span data-ttu-id="1a2db-123">Verze aktualizace SP1 2008 R2 2.127.20170406 nebo novější</span><span class="sxs-lookup"><span data-stu-id="1a2db-123">2008-R2-SP1 version 2.127.20170406 or above</span></span>

<span data-ttu-id="1a2db-124">Pro klienta systému Windows:</span><span class="sxs-lookup"><span data-stu-id="1a2db-124">For Windows Client:</span></span>
```powershell
Get-AzureRMVMImageSku -Location "West US" -Publisher "MicrosoftWindowsServer" `
    -Offer "Windows-HUB"
```

## <a name="upload-a-windows-server-vhd"></a><span data-ttu-id="1a2db-125">Nahrání virtuálního pevného disku serveru systému Windows</span><span class="sxs-lookup"><span data-stu-id="1a2db-125">Upload a Windows Server VHD</span></span>
<span data-ttu-id="1a2db-126">Pokud chcete nasadit virtuální počítač Windows serveru v Azure, musíte nejprve vytvořit virtuální pevný disk, který obsahuje vaše základní build Windows.</span><span class="sxs-lookup"><span data-stu-id="1a2db-126">To deploy a Windows Server VM in Azure, you first need to create a VHD that contains your base Windows build.</span></span> <span data-ttu-id="1a2db-127">Tento virtuální pevný disk musí být správně připravena pomocí nástroje Sysprep, před nahráním do Azure.</span><span class="sxs-lookup"><span data-stu-id="1a2db-127">This VHD must be appropriately prepared via Sysprep before you upload it to Azure.</span></span> <span data-ttu-id="1a2db-128">Můžete [Další informace o požadavcích virtuálního pevného disku a procesu Sysprep](upload-generalized-managed.md) a [podpora nástroje Sysprep pro role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span><span class="sxs-lookup"><span data-stu-id="1a2db-128">You can [read more about the VHD requirements and Sysprep process](upload-generalized-managed.md) and [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).</span></span> <span data-ttu-id="1a2db-129">Zálohování virtuálního počítače před spuštěním nástroje Sysprep.</span><span class="sxs-lookup"><span data-stu-id="1a2db-129">Back up the VM before running Sysprep.</span></span> 

<span data-ttu-id="1a2db-130">Zajistěte, aby byla [nainstalovaný a nakonfigurovaný nejnovější prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1a2db-130">Make sure you have [installed and configured the latest Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="1a2db-131">Jakmile připravíte svůj disk VHD, odeslání disku VHD do vašeho účtu úložiště Azure pomocí `Add-AzureRmVhd` rutiny následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1a2db-131">Once you have prepared your VHD, upload the VHD to your Azure Storage account using the `Add-AzureRmVhd` cmdlet as follows:</span></span>

```powershell
Add-AzureRmVhd -ResourceGroupName "myResourceGroup" -LocalFilePath "C:\Path\To\myvhd.vhd" `
    -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd"
```

> [!NOTE]
> <span data-ttu-id="1a2db-132">Microsoft SQL Server, SharePoint Server a Dynamics můžete také používat váš Software Assurance licencování.</span><span class="sxs-lookup"><span data-stu-id="1a2db-132">Microsoft SQL Server, SharePoint Server, and Dynamics can also utilize your Software Assurance licensing.</span></span> <span data-ttu-id="1a2db-133">Stále je nutné připravit bitovou kopii systému Windows Server tak, že instalace součásti vaší aplikace a poskytování aktivační kódy VLK odpovídajícím způsobem pak odesílání bitové kopie disku do Azure.</span><span class="sxs-lookup"><span data-stu-id="1a2db-133">You still need to prepare the Windows Server image by installing your application components and providing license keys accordingly, then uploading the disk image to Azure.</span></span> <span data-ttu-id="1a2db-134">Zkontrolujte v příslušné dokumentaci ke spuštění nástroje Sysprep s vaší aplikací, jako například [důležité informace týkající se instalace systému SQL Server pomocí nástroje Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) nebo [sestavení SharePoint serveru 2016 referenční bitové kopie (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a2db-134">Review the appropriate documentation for running Sysprep with your application, such as [Considerations for Installing SQL Server using Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) or [Build a SharePoint Server 2016 Reference Image (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).</span></span>
>
>

<span data-ttu-id="1a2db-135">Také další informace o [nahrávání virtuální pevný disk do Azure procesu](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span><span class="sxs-lookup"><span data-stu-id="1a2db-135">You can also read more about [uploading the VHD to Azure process](upload-generalized-managed.md#upload-the-vhd-to-your-storage-account)</span></span>


## <a name="deploy-a-vm-via-resource-manager-template"></a><span data-ttu-id="1a2db-136">Nasazení virtuálního počítače prostřednictvím šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1a2db-136">Deploy a VM via Resource Manager Template</span></span>
<span data-ttu-id="1a2db-137">V rámci šablony Resource Manageru, dalším parametr pro `licenseType` lze zadat.</span><span class="sxs-lookup"><span data-stu-id="1a2db-137">Within your Resource Manager templates, an additional parameter for `licenseType` can be specified.</span></span> <span data-ttu-id="1a2db-138">Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1a2db-138">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="1a2db-139">Až budete mít svůj disk VHD nahrán do Azure, upravte šablony Resource Manageru zahrnout typ licence jako součást poskytovatele výpočetních a nasazení vaší šablony jako za normálních okolností:</span><span class="sxs-lookup"><span data-stu-id="1a2db-139">Once you have your VHD uploaded to Azure, edit you Resource Manager template to include the license type as part of the compute provider and deploy your template as normal:</span></span>

<span data-ttu-id="1a2db-140">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="1a2db-140">For Windows Server:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

<span data-ttu-id="1a2db-141">Pro klienta systému Windows pro použití s Azure Marketplace Image pouze:</span><span class="sxs-lookup"><span data-stu-id="1a2db-141">For Windows Client to use with Azure Marketplace Image only:</span></span>
```json
"properties": {  
   "licenseType": "Windows_Client",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="deploy-a-vm-via-powershell-quickstart"></a><span data-ttu-id="1a2db-142">Nasazení virtuálního počítače pomocí rychlého startu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a2db-142">Deploy a VM via PowerShell quickstart</span></span>
<span data-ttu-id="1a2db-143">Při nasazování virtuálního počítače Windows serveru pomocí prostředí PowerShell, máte další parametr pro `-LicenseType`.</span><span class="sxs-lookup"><span data-stu-id="1a2db-143">When deploying your Windows Server VM via PowerShell, you have an additional parameter for `-LicenseType`.</span></span> <span data-ttu-id="1a2db-144">Až budete mít svůj disk VHD nahrán do Azure, vytvoříte virtuální počítač pomocí `New-AzureRmVM` a zadejte typ licencování následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1a2db-144">Once you have your VHD uploaded to Azure, you create a VM using `New-AzureRmVM` and specify the licensing type as follows:</span></span>

<span data-ttu-id="1a2db-145">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="1a2db-145">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Server"
```

<span data-ttu-id="1a2db-146">Pro klienta systému Windows pro použití s Azure Marketplace Image pouze:</span><span class="sxs-lookup"><span data-stu-id="1a2db-146">For Windows Client to use with Azure Marketplace Image only:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName "myResourceGroup" -Location "West US" -VM $vm -LicenseType "Windows_Client"
```

<span data-ttu-id="1a2db-147">Můžete [přečíst podrobnější návod k nasazení virtuálních počítačů v Azure pomocí prostředí PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) níže, nebo si můžete přečíst více popisný příručce na různé kroky, které [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1a2db-147">You can [read a more detailed walkthrough on deploying a VM in Azure via PowerShell](hybrid-use-benefit-licensing.md#detailed-powershell-deployment-walkthrough) below, or read a more descriptive guide on the different steps to [create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a><span data-ttu-id="1a2db-148">Ověřte, zda že je virtuální počítač využívá licencování benefit</span><span class="sxs-lookup"><span data-stu-id="1a2db-148">Verify your VM is utilizing the licensing benefit</span></span>
<span data-ttu-id="1a2db-149">Po nasazení virtuálního počítače prostřednictvím buď prostředí PowerShell nebo správce prostředků metodu nasazení ověřte typ licence s `Get-AzureRmVM` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1a2db-149">Once you have deployed your VM through either the PowerShell or Resource Manager deployment method, verify the license type with `Get-AzureRmVM` as follows:</span></span>

```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="1a2db-150">Výstup bude vypadat v následujícím příkladu pro systém Windows Server:</span><span class="sxs-lookup"><span data-stu-id="1a2db-150">The output is similar to the following example for Windows Server:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

<span data-ttu-id="1a2db-151">Tento výstup uvádí vedle sebe s následující virtuální počítač nasadit bez licencování Azure hybridní použití výhody, jako je například virtuální počítač nasadit přímo z Galerie Azure:</span><span class="sxs-lookup"><span data-stu-id="1a2db-151">This output contrasts with the following VM deployed without Azure Hybrid Use Benefit licensing, such as a VM deployed straight from the Azure Gallery:</span></span>

```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

## <a name="detailed-powershell-deployment-walkthrough"></a><span data-ttu-id="1a2db-152">Podrobný postup nasazení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1a2db-152">Detailed PowerShell deployment walkthrough</span></span>
<span data-ttu-id="1a2db-153">Zobrazit následující podrobné kroky prostředí PowerShell úplné nasazení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1a2db-153">The following detailed PowerShell steps show a full deployment of a VM.</span></span> <span data-ttu-id="1a2db-154">Můžete si přečíst další kontext, skutečný rutiny a vytváří v různých komponent [vytvořit virtuální počítač s Windows pomocí Resource Manageru a prostředí PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1a2db-154">You can read more context as to the actual cmdlets and different components being created in [Create a Windows VM using Resource Manager and PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="1a2db-155">Krok procesem vytvoření vaší skupiny prostředků, účet úložiště a virtuální sítě, pak zadejte virtuální počítač a nakonec vytvořte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="1a2db-155">You step through creating your resource group, storage account, and virtual networking, then define your VM and finally create your VM.</span></span>

<span data-ttu-id="1a2db-156">Nejprve bezpečně získat přihlašovací údaje, nastavte umístění a název skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="1a2db-156">First, securely obtain credentials, set a location, and resource group name:</span></span>

```powershell
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "myResourceGroup"
```

<span data-ttu-id="1a2db-157">Vytvoření veřejné IP adresy:</span><span class="sxs-lookup"><span data-stu-id="1a2db-157">Create a public IP:</span></span>

```powershell
$publicIPName = "myPublicIP"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName `
    -Location $location -AllocationMethod "Dynamic"
```

<span data-ttu-id="1a2db-158">Definujte vaší virtuální sítě, podsítě a síťový adaptér:</span><span class="sxs-lookup"><span data-stu-id="1a2db-158">Define your subnet, NIC, and VNET:</span></span>

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

<span data-ttu-id="1a2db-159">Název virtuálního počítače a vytvoření konfigurace virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="1a2db-159">Name your VM and create a VM config:</span></span>

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

<span data-ttu-id="1a2db-160">Definujte operačním systému:</span><span class="sxs-lookup"><span data-stu-id="1a2db-160">Define your OS:</span></span>

```powershell
$computerName = "myVM"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

<span data-ttu-id="1a2db-161">Do virtuálního počítače přidejte svoji síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="1a2db-161">Add your NIC to the VM:</span></span>

```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

<span data-ttu-id="1a2db-162">Zadejte účet úložiště, který chcete použít:</span><span class="sxs-lookup"><span data-stu-id="1a2db-162">Define the storage account to use:</span></span>

```powershell
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName mystorageaccount
```

<span data-ttu-id="1a2db-163">Nahrát svůj disk VHD, vhodně připravit a připojte k virtuálnímu počítači pro použití:</span><span class="sxs-lookup"><span data-stu-id="1a2db-163">Upload your VHD, suitably prepared, and attach to your VM for use:</span></span>

```powershell
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/vhd/myvhd.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage `
    -SourceImageUri $urlOfUploadedImageVhd -Windows
```

<span data-ttu-id="1a2db-164">Nakonec vytvořte virtuální počítač a definování licencování typu využívat výhody použití hybridní Azure:</span><span class="sxs-lookup"><span data-stu-id="1a2db-164">Finally, create your VM and define the licensing type to utilize Azure Hybrid Use Benefit:</span></span>

<span data-ttu-id="1a2db-165">Windows Server:</span><span class="sxs-lookup"><span data-stu-id="1a2db-165">For Windows Server:</span></span>
```powershell
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType "Windows_Server"
```

## <a name="deploy-a-virtual-machine-scale-set-via-resource-manager-template"></a><span data-ttu-id="1a2db-166">Nasazení škálování virtuálních počítačů, nastavit pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="1a2db-166">Deploy a virtual machine scale set via Resource Manager template</span></span>
<span data-ttu-id="1a2db-167">V rámci šablony správce prostředků VMSS dalším parametr pro `licenseType` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="1a2db-167">Within your VMSS Resource Manager templates, an additional parameter for `licenseType` must be specified.</span></span> <span data-ttu-id="1a2db-168">Další informace o [vytváření šablon Azure Resource Manager](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1a2db-168">You can read more about [authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span> <span data-ttu-id="1a2db-169">Upravte svou šablonu Resource Manager zahrnout vlastnost licenseType jako součást virtualMachineProfile byly sadou škálování a nasazení vaší šablony jako normální-najdete v příkladu níže pomocí bitové kopie systému Windows Server 2016:</span><span class="sxs-lookup"><span data-stu-id="1a2db-169">Edit your Resource Manager template to include the licenseType property as part of the scale set’s virtualMachineProfile and deploy your template as normal - see example below using 2016 Windows Server image:</span></span>


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
> <span data-ttu-id="1a2db-170">Podpora pro nasazení škálování virtuálních počítačů s AHUB výhody pomocí prostředí PowerShell a další nástroje SDK se připravuje.</span><span class="sxs-lookup"><span data-stu-id="1a2db-170">Support for deploying a virtual machine scale set with AHUB benefits through PowerShell and other SDK tools is coming soon.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="1a2db-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a2db-171">Next steps</span></span>
<span data-ttu-id="1a2db-172">Další informace o [licencování Azure hybridní použití zvýhodnění](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span><span class="sxs-lookup"><span data-stu-id="1a2db-172">Read more about [Azure Hybrid Use Benefit licensing](https://azure.microsoft.com/pricing/hybrid-use-benefit/).</span></span>

<span data-ttu-id="1a2db-173">Další informace o [pomocí šablony Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a2db-173">Learn more about [using Resource Manager templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="1a2db-174">Další informace o [výhody použití hybridní Azure a Azure Site Recovery zkontrolujte migrace aplikací do Azure i více nákladově efektivní](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span><span class="sxs-lookup"><span data-stu-id="1a2db-174">Learn more about [Azure Hybrid Use Benefit and Azure Site Recovery make migrating applications to Azure even more cost-effective](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/).</span></span>
