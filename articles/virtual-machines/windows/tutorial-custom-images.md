---
title: "aaaCreate vlastní Image virtuálních počítačů s hello prostředí Azure PowerShell | Microsoft Docs"
description: "Kurz – vytvoření vlastní image virtuálního počítače pomocí hello prostředí Azure PowerShell."
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
ms.openlocfilehash: 3a759fe1b7e7b72f531399b0f4a99e341713c6a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a><span data-ttu-id="79e7b-103">Vytvořit vlastní image virtuálního počítače Azure pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="79e7b-103">Create a custom image of an Azure VM using PowerShell</span></span>

<span data-ttu-id="79e7b-104">Vlastní Image jsou jako obrázky marketplace, ale můžete vytvořit sami.</span><span class="sxs-lookup"><span data-stu-id="79e7b-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="79e7b-105">Vlastní Image může být použité toobootstrap konfigurace, jako je například předběžného načítání aplikace, konfigurace aplikace a další konfigurace operačního systému.</span><span class="sxs-lookup"><span data-stu-id="79e7b-105">Custom images can be used toobootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="79e7b-106">V tomto kurzu vytvoříte svoji vlastní image virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="79e7b-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="79e7b-107">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="79e7b-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79e7b-108">Nástroj Sysprep a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="79e7b-108">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="79e7b-109">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="79e7b-109">Create a custom image</span></span>
> * <span data-ttu-id="79e7b-110">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="79e7b-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="79e7b-111">Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="79e7b-111">List all hello images in your subscription</span></span>
> * <span data-ttu-id="79e7b-112">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="79e7b-112">Delete an image</span></span>

<span data-ttu-id="79e7b-113">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="79e7b-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="79e7b-114">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="79e7b-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="79e7b-115">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="79e7b-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="79e7b-116">Než začnete</span><span class="sxs-lookup"><span data-stu-id="79e7b-116">Before you begin</span></span>

<span data-ttu-id="79e7b-117">Následující postup Hello podrobnosti, jak tootake existující virtuální počítač a zapněte ji do opakovaně použitelné vlastní image, které můžete použít toocreate nové instance virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79e7b-117">hello steps below detail how tootake an existing VM and turn it into a re-usable custom image that you can use toocreate new VM instances.</span></span>

<span data-ttu-id="79e7b-118">Příklad hello toocomplete v tomto kurzu, musí mít existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="79e7b-118">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="79e7b-119">V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás.</span><span class="sxs-lookup"><span data-stu-id="79e7b-119">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="79e7b-120">Při absolvování hello kurzu, nahraďte hello skupinu prostředků a virtuálních počítačů názvy, kde je potřeba.</span><span class="sxs-lookup"><span data-stu-id="79e7b-120">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

## <a name="prepare-vm"></a><span data-ttu-id="79e7b-121">Příprava virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="79e7b-121">Prepare VM</span></span>

<span data-ttu-id="79e7b-122">toocreate bitové kopie virtuálního počítače, musíte tooprepare hello virtuálních počítačů pomocí generalizací hello virtuálních počítačů, rušení přidělení a pak označí hello zdrojového virtuálního počítače jako zobecněn v Azure.</span><span class="sxs-lookup"><span data-stu-id="79e7b-122">toocreate an image of a virtual machine, you need tooprepare hello VM by generalizing hello VM, deallocating, and then marking hello source VM as generalized in Azure.</span></span>

### <a name="generalize-hello-windows-vm-using-sysprep"></a><span data-ttu-id="79e7b-123">Generalize hello virtuální počítač s Windows pomocí nástroje Sysprep</span><span class="sxs-lookup"><span data-stu-id="79e7b-123">Generalize hello Windows VM using Sysprep</span></span>

<span data-ttu-id="79e7b-124">Nástroj Sysprep odstraní všechny vaše osobní informace o účtu, mimo jiné a připraví toobe počítač hello použít jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="79e7b-124">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="79e7b-125">Podrobnosti o nástroji Sysprep najdete v tématu [jak tooUse nástroje Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="79e7b-125">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>


1. <span data-ttu-id="79e7b-126">Připojte toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79e7b-126">Connect toohello virtual machine.</span></span>
2. <span data-ttu-id="79e7b-127">Otevřete okno příkazového řádku hello jako správce.</span><span class="sxs-lookup"><span data-stu-id="79e7b-127">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="79e7b-128">Změnit adresář, hello příliš*%windir%\system32\sysprep*a poté spusťte *sysprep.exe*.</span><span class="sxs-lookup"><span data-stu-id="79e7b-128">Change hello directory too*%windir%\system32\sysprep*, and then run *sysprep.exe*.</span></span>
3. <span data-ttu-id="79e7b-129">V hello **nástroj pro přípravu systému** dialogové okno, vyberte *prostředí Out-of-Box zadejte systému (při prvním zapnutí)*a ujistěte se, že hello *generalizace* je zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="79e7b-129">In hello **System Preparation Tool** dialog box, select *Enter System Out-of-Box Experience (OOBE)*, and make sure that hello *Generalize* check box is selected.</span></span>
4. <span data-ttu-id="79e7b-130">V **možnosti vypnutí**, vyberte *vypnutí* a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="79e7b-130">In **Shutdown Options**, select *Shutdown* and then click **OK**.</span></span>
5. <span data-ttu-id="79e7b-131">Po dokončení nástroj Sysprep vypne hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79e7b-131">When Sysprep completes, it shuts down hello virtual machine.</span></span> <span data-ttu-id="79e7b-132">**Nerestartovat hello virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="79e7b-132">**Do not restart hello VM**.</span></span>

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a><span data-ttu-id="79e7b-133">Deallocate a označit hello jako zobecněný virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="79e7b-133">Deallocate and mark hello VM as generalized</span></span>

<span data-ttu-id="79e7b-134">toocreate bitovou kopii, hello virtuálních počítačů musí toobe navrácena a označené jako zobecněn v Azure.</span><span class="sxs-lookup"><span data-stu-id="79e7b-134">toocreate an image, hello VM needs toobe deallocated and marked as generalized in Azure.</span></span>

<span data-ttu-id="79e7b-135">Virtuální počítač pomocí deallocated hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="79e7b-135">Deallocated hello VM using [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).</span></span>

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

<span data-ttu-id="79e7b-136">Nastavte stav hello hello virtuálního počítače příliš`-Generalized` pomocí [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="79e7b-136">Set hello status of hello virtual machine too`-Generalized` using [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm).</span></span> 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-hello-image"></a><span data-ttu-id="79e7b-137">Vytvoření bitové kopie hello</span><span class="sxs-lookup"><span data-stu-id="79e7b-137">Create hello image</span></span>

<span data-ttu-id="79e7b-138">Nyní můžete vytvořit image hello virtuálních počítačů pomocí [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) a [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span><span class="sxs-lookup"><span data-stu-id="79e7b-138">Now you can create an image of hello VM by using [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) and [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage).</span></span> <span data-ttu-id="79e7b-139">Hello následující příklad vytvoří bitovou kopii s názvem *myImage* z virtuálního počítače s názvem *Můjvp*.</span><span class="sxs-lookup"><span data-stu-id="79e7b-139">hello following example creates an image named *myImage* from a VM named *myVM*.</span></span>

<span data-ttu-id="79e7b-140">Získáte hello virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="79e7b-140">Get hello virtual machine.</span></span> 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

<span data-ttu-id="79e7b-141">Vytvořte konfiguraci hello bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="79e7b-141">Create hello image configuration.</span></span>

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

<span data-ttu-id="79e7b-142">Vytvoření bitové kopie hello.</span><span class="sxs-lookup"><span data-stu-id="79e7b-142">Create hello image.</span></span>

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-hello-image"></a><span data-ttu-id="79e7b-143">Vytvořit virtuální počítače z hello image</span><span class="sxs-lookup"><span data-stu-id="79e7b-143">Create VMs from hello image</span></span>

<span data-ttu-id="79e7b-144">Teď, když máte image, můžete vytvořit jeden nebo více nové virtuální počítače z hello image.</span><span class="sxs-lookup"><span data-stu-id="79e7b-144">Now that you have an image, you can create one or more new VMs from hello image.</span></span> <span data-ttu-id="79e7b-145">Vytvoření virtuálního počítače z vlastní image je velmi podobné toocreating virtuálního počítače pomocí image pořízenou prostřednictvím Marketplace.</span><span class="sxs-lookup"><span data-stu-id="79e7b-145">Creating a VM from a custom image is very similar toocreating a VM using a Marketplace image.</span></span> <span data-ttu-id="79e7b-146">Pokud používáte image pořízenou prostřednictvím Marketplace, máte tooinformation o hello image, image poskytovatele, nabídky, SKU a verzi.</span><span class="sxs-lookup"><span data-stu-id="79e7b-146">When you use a Marketplace image, you have tooinformation about hello image, image provider, offer, SKU and version.</span></span> <span data-ttu-id="79e7b-147">Vlastní Image bude třeba jen tooprovide hello ID hello vlastní prostředek obrázku.</span><span class="sxs-lookup"><span data-stu-id="79e7b-147">With a custom image, you just need tooprovide hello ID of hello custom image resource.</span></span> 

<span data-ttu-id="79e7b-148">V následující skript hello, vytvoříme proměnnou *$image* toostore informace o používání vlastní image hello [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) a pak nám pomocí [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage)a zadejte ID hello pomocí hello *$image* proměnné jsme právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="79e7b-148">In hello following script, we create a variable *$image* toostore information about hello custom image using [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) and then we use [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) and specify hello ID using hello *$image* variable we just created.</span></span> 

<span data-ttu-id="79e7b-149">skript Hello vytvoří virtuální počítač s názvem *myVMfromImage* z našich vlastní image ve novou skupinu prostředků s názvem *myResourceGroupFromImage* v hello *západní USA* umístění.</span><span class="sxs-lookup"><span data-stu-id="79e7b-149">hello script creates a VM named *myVMfromImage* from our custom image in a new resource group named *myResourceGroupFromImage* in hello *West US* location.</span></span>


```powershell
$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

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

# Here is where we create a variable toostore information about hello image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want toocreate hello VM from and image and provide hello image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a><span data-ttu-id="79e7b-150">Správy bitových kopií</span><span class="sxs-lookup"><span data-stu-id="79e7b-150">Image management</span></span> 

<span data-ttu-id="79e7b-151">Zde jsou některé příklady běžné úlohy správy bitové kopie a jak toocomplete je pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="79e7b-151">Here are some examples of common management image tasks and how toocomplete them using PowerShell.</span></span>

<span data-ttu-id="79e7b-152">Zobrazí seznam všech Image podle názvu.</span><span class="sxs-lookup"><span data-stu-id="79e7b-152">List all images by name.</span></span>

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

<span data-ttu-id="79e7b-153">Odstraňte bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="79e7b-153">Delete an image.</span></span> <span data-ttu-id="79e7b-154">Tento příklad odstranění hello image s názvem *myOldImage* z hello *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="79e7b-154">This example deletes hello image named *myOldImage* from hello *myResourceGroup*.</span></span>

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="79e7b-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="79e7b-155">Next steps</span></span>

<span data-ttu-id="79e7b-156">V tomto kurzu jste vytvořili vlastní image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="79e7b-156">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="79e7b-157">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="79e7b-157">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="79e7b-158">Nástroj Sysprep a generalize virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="79e7b-158">Sysprep and generalize VMs</span></span>
> * <span data-ttu-id="79e7b-159">Vytvoření vlastní image</span><span class="sxs-lookup"><span data-stu-id="79e7b-159">Create a custom image</span></span>
> * <span data-ttu-id="79e7b-160">Vytvoření virtuálního počítače z vlastní image</span><span class="sxs-lookup"><span data-stu-id="79e7b-160">Create a VM from a custom image</span></span>
> * <span data-ttu-id="79e7b-161">Zobrazí seznam všech hello bitové kopie v rámci vašeho předplatného</span><span class="sxs-lookup"><span data-stu-id="79e7b-161">List all hello images in your subscription</span></span>
> * <span data-ttu-id="79e7b-162">Odstranit bitovou kopii</span><span class="sxs-lookup"><span data-stu-id="79e7b-162">Delete an image</span></span>

<span data-ttu-id="79e7b-163">Posunutí další kurz toolearn toohello o tom, jak vysoce dostupných virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="79e7b-163">Advance toohello next tutorial toolearn about how highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="79e7b-164">Vytvoření vysoce dostupných virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="79e7b-164">Create highly available VMs</span></span>](tutorial-availability-sets.md)



