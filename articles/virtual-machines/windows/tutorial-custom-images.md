---
title: "Vytvoření vlastní Image virtuálních počítačů pomocí prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí prostředí Azure PowerShell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 96be2872a902a7d7063bf1dff7b4ca209a5b67c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a><span data-ttu-id="698f9-103">Vytvořit vlastní image virtuálního počítače Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="698f9-103">Create a custom image of an Azure VM using PowerShell</span></span>

<span data-ttu-id="698f9-104">Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami.</span><span class="sxs-lookup"><span data-stu-id="698f9-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="698f9-105">Vlastní Image slouží k zavedení konfigurace, třeba předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="698f9-105">Custom images can be used to bootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="698f9-106">V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="698f9-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="698f9-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="698f9-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="698f9-108">Nástroj Sysprep a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="698f9-108">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="698f9-109">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="698f9-109">Create a custom image</span></span>
> * <span data-ttu-id="698f9-110">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="698f9-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="698f9-111">Seznam všechny bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="698f9-111">List all the images in your subscription</span></span>
> * <span data-ttu-id="698f9-112">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="698f9-112">Delete an image</span></span>

<span data-ttu-id="698f9-113">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="698f9-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="698f9-114">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="698f9-114">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="698f9-115">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="698f9-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="698f9-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="698f9-116">Before you begin</span></span>

<span data-ttu-id="698f9-117">Následující postup podrobnosti jak využít existující virtuální počítač a zapnout ho do opakovaně použitelné vlastní obrázek, který můžete použít k vytvoření nové instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="698f9-117">The steps below detail how to take an existing VM and turn it into a re-usable custom image that you can use to create new VM instances.</span></span>

<span data-ttu-id="698f9-118">Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="698f9-118">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="698f9-119">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="698f9-119">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="698f9-120">Při absolvování kurzu, nahraďte skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="698f9-120">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

## <a name="prepare-vm"></a><span data-ttu-id="698f9-121">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="698f9-121">Prepare VM</span></span>

<span data-ttu-id="698f9-122">Chcete-li vytvořit bitovou kopii virtuálního počítače, je nutné připravit virtuální počítač generalizací virtuální počítač, rušení přidělení a pak označením zdrojového virtuálního počítače jako zobecněn v Azure.</span><span class="sxs-lookup"><span data-stu-id="698f9-122">To create an image of a virtual machine, you need to prepare the VM by generalizing the VM, deallocating, and then marking the source VM as generalized in Azure.</span></span>

### <a name="generalize-the-windows-vm-using-sysprep"></a><span data-ttu-id="698f9-123">Generalize virtuální počítač s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="698f9-123">Generalize the Windows VM using Sysprep</span></span>

<span data-ttu-id="698f9-124">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví počítač, který se má použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="698f9-124">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="698f9-125">Podrobnosti o nástroji Sysprep najdete v tématu [postup použití nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="698f9-125">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>


1. <span data-ttu-id="698f9-126">Připojte k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="698f9-126">Connect to the virtual machine.</span></span>
2. <span data-ttu-id="698f9-127">Otevřete okno příkazového řádku jako správce.</span><span class="sxs-lookup"><span data-stu-id="698f9-127">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="698f9-128">Změňte adresář na *%windir%\system32\sysprep*a poté spusťte *sysprep.exe*.</span><span class="sxs-lookup"><span data-stu-id="698f9-128">Change the directory to *%windir%\system32\sysprep*, and then run *sysprep.exe*.</span></span>
3. <span data-ttu-id="698f9-129">V **nástroj pro přípravu systému** dialogové okno, vyberte *prostředí Out-of-Box zadejte systému (při prvním zapnutí)*a ujistěte se, že *generalizace* je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="698f9-129">In the **System Preparation Tool** dialog box, select *Enter System Out-of-Box Experience (OOBE)*, and make sure that the *Generalize* check box is selected.</span></span>
4. <span data-ttu-id="698f9-130">V **možnosti vypnutí**, vyberte *vypnutí* a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="698f9-130">In **Shutdown Options**, select *Shutdown* and then click **OK**.</span></span>
5. <span data-ttu-id="698f9-131">Po dokončení nástroj Sysprep vypne virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="698f9-131">When Sysprep completes, it shuts down the virtual machine.</span></span> <span data-ttu-id="698f9-132">**Virtuální počítač nerestartuje**.</span><span class="sxs-lookup"><span data-stu-id="698f9-132">**Do not restart the VM**.</span></span>

### <a name="deallocate-and-mark-the-vm-as-generalized"></a><span data-ttu-id="698f9-133">Deallocate a označit virtuální počítač jako zobecněn</span><span class="sxs-lookup"><span data-stu-id="698f9-133">Deallocate and mark the VM as generalized</span></span>

<span data-ttu-id="698f9-134">Virtuální počítač na vytvoření bitové kopie, je třeba navrácena a označené jako zobecněn v Azure.</span><span class="sxs-lookup"><span data-stu-id="698f9-134">To create an image, the VM needs to be deallocated and marked as generalized in Azure.</span></span>

<span data-ttu-id="698f9-135">Virtuální počítač pomocí navrátil [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="698f9-135">Deallocated the VM using [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

<span data-ttu-id="698f9-136">Nastavte stav virtuálního počítače na `-Generalized` pomocí [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="698f9-136">Set the status of the virtual machine to `-Generalized` using [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span></span> 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-the-image"></a><span data-ttu-id="698f9-137">Vytvoření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="698f9-137">Create the image</span></span>

<span data-ttu-id="698f9-138">Nyní můžete vytvořit image virtuálního počítače pomocí [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) a [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span><span class="sxs-lookup"><span data-stu-id="698f9-138">Now you can create an image of the VM by using [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) and [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span></span> <span data-ttu-id="698f9-139">Následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="698f9-139">The following example creates an image named *myImage* from a VM named *myVM*.</span></span>

<span data-ttu-id="698f9-140">Získáte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="698f9-140">Get the virtual machine.</span></span> 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

<span data-ttu-id="698f9-141">Vytvořte konfiguraci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="698f9-141">Create the image configuration.</span></span>

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

<span data-ttu-id="698f9-142">Vytvoření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="698f9-142">Create the image.</span></span>

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-the-image"></a><span data-ttu-id="698f9-143">Vytvořit virtuální počítače z image</span><span class="sxs-lookup"><span data-stu-id="698f9-143">Create VMs from the image</span></span>

<span data-ttu-id="698f9-144">Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače z image.</span><span class="sxs-lookup"><span data-stu-id="698f9-144">Now that you have an image, you can create one or more new VMs from the image.</span></span> <span data-ttu-id="698f9-145">Vytvoření virtuálního počítače z vlastní image je velmi podobný k vytvoření virtuálního počítače pomocí image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="698f9-145">Creating a VM from a custom image is very similar to creating a VM using a Marketplace image.</span></span> <span data-ttu-id="698f9-146">Pokud používáte image pořízenou prostřednictvím Marketplace, budete muset informace o bitové kopie, zprostředkovatele bitové kopie, nabídky, SKU a verzi.</span><span class="sxs-lookup"><span data-stu-id="698f9-146">When you use a Marketplace image, you have to information about the image, image provider, offer, SKU and version.</span></span> <span data-ttu-id="698f9-147">Vlastní Image stačí zadat Identifikátor prostředku vlastní image.</span><span class="sxs-lookup"><span data-stu-id="698f9-147">With a custom image, you just need to provide the ID of the custom image resource.</span></span> 

<span data-ttu-id="698f9-148">V následující skript, vytvoříme proměnnou *$image* k ukládání informací o používání vlastní image [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) a pak nám pomocí [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) a zadejte ID pomocí *$image* proměnné jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="698f9-148">In the following script, we create a variable *$image* to store information about the custom image using [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) and then we use [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) and specify the ID using the *$image* variable we just created.</span></span> 

<span data-ttu-id="698f9-149">Tento skript vytvoří virtuální počítač s názvem *myVMfromImage* z našich vlastní image ve novou skupinu prostředků s názvem *myResourceGroupFromImage* v *západní USA* umístění.</span><span class="sxs-lookup"><span data-stu-id="698f9-149">The script creates a VM named *myVMfromImage* from our custom image in a new resource group named *myResourceGroupFromImage* in the *West US* location.</span></span>


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a><span data-ttu-id="698f9-150">Správy bitových kopií</span><span class="sxs-lookup"><span data-stu-id="698f9-150">Image management</span></span> 

<span data-ttu-id="698f9-151">Zde jsou některé příklady běžné úlohy správy bitové kopie a postup jejich dokončení pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="698f9-151">Here are some examples of common management image tasks and how to complete them using PowerShell.</span></span>

<span data-ttu-id="698f9-152">Zobrazí seznam všech Image podle názvu.</span><span class="sxs-lookup"><span data-stu-id="698f9-152">List all images by name.</span></span>

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

<span data-ttu-id="698f9-153">Odstraňte bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="698f9-153">Delete an image.</span></span> <span data-ttu-id="698f9-154">Tento příklad odstraní image s názvem *myOldImage* z *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="698f9-154">This example deletes the image named *myOldImage* from the *myResourceGroup*.</span></span>

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="698f9-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="698f9-155">Next steps</span></span>

<span data-ttu-id="698f9-156">V tomto kurzu jste vytvořili vlastní image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="698f9-156">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="698f9-157">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="698f9-157">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="698f9-158">Nástroj Sysprep a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="698f9-158">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="698f9-159">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="698f9-159">Create a custom image</span></span>
> * <span data-ttu-id="698f9-160">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="698f9-160">Create a VM from a custom image</span></span>
> * <span data-ttu-id="698f9-161">Seznam všechny bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="698f9-161">List all the images in your subscription</span></span>
> * <span data-ttu-id="698f9-162">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="698f9-162">Delete an image</span></span>

<span data-ttu-id="698f9-163">Přechodu na další informace o tom, jak vysoce dostupné virtuální počítače v dalším kurzu.</span><span class="sxs-lookup"><span data-stu-id="698f9-163">Advance to the next tutorial to learn about how highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="698f9-164">Vytvoření vysoce dostupných virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="698f9-164">Create highly available VMs</span></span>](tutorial-availability-sets.md)



