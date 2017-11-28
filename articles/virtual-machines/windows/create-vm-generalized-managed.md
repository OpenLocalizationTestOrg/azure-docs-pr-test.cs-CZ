---
title: "aaaCreate virtuální počítač z bitové kopie spravovaných virtuálních počítačů v Azure | Microsoft Docs"
description: "Vytvoření virtuálního počítače Windows z zobecněný spravované image virtuálního počítače pomocí prostředí Azure PowerShell v modelu nasazení Resource Manager hello."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: cynthn
ms.openlocfilehash: 5036ef1533c144a9a328e94599b359e0166f337d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-managed-image"></a><span data-ttu-id="1fc45-103">Vytvoření virtuálního počítače ze spravovaných bitové kopie</span><span class="sxs-lookup"><span data-stu-id="1fc45-103">Create a VM from a managed image</span></span>

<span data-ttu-id="1fc45-104">Můžete vytvořit víc virtuálních počítačů ze spravovaných image virtuálního počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="1fc45-104">You can create multiple VMs from a managed VM image in Azure.</span></span> <span data-ttu-id="1fc45-105">Spravované image virtuálního počítače obsahuje hello informace nezbytné toocreate virtuálního počítače, včetně hello operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="1fc45-105">A managed VM image contains hello information necessary toocreate a VM, including hello OS and data disks.</span></span> <span data-ttu-id="1fc45-106">Hello virtuální pevné disky, které tvoří hello bitovou kopii, včetně disků hello operačního systému a všech datových disků, jsou uloženy jako spravované disky.</span><span class="sxs-lookup"><span data-stu-id="1fc45-106">hello VHDs that make up hello image, including both hello OS disks and any data disks, are stored as managed disks.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="1fc45-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1fc45-107">Prerequisites</span></span>

<span data-ttu-id="1fc45-108">Je třeba toohave již [vytvořit spravované image virtuálního počítače](capture-image-resource.md) hello toouse pro vytvoření nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1fc45-108">You need toohave already [created a managed VM image](capture-image-resource.md) toouse for creating hello new VM.</span></span> 

<span data-ttu-id="1fc45-109">Ujistěte se, že máte nejnovější verzi modulů AzureRM.Compute a prostředí AzureRM.Network PowerShell hello hello.</span><span class="sxs-lookup"><span data-stu-id="1fc45-109">Make sure that you have hello latest versions of hello AzureRM.Compute and AzureRM.Network PowerShell modules.</span></span> <span data-ttu-id="1fc45-110">Otevřete příkazovém řádku prostředí PowerShell jako správce a spusťte následující příkaz tooinstall hello je.</span><span class="sxs-lookup"><span data-stu-id="1fc45-110">Open a PowerShell prompt as an Administrator and run hello following command tooinstall them.</span></span>

```powershell
Install-Module AzureRM.Compute,AzureRM.Network
```
<span data-ttu-id="1fc45-111">Další informace najdete v tématu [Azure PowerShell verze](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1fc45-111">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>



## <a name="collect-information-about-hello-image"></a><span data-ttu-id="1fc45-112">Shromažďovat informace o bitové kopii hello</span><span class="sxs-lookup"><span data-stu-id="1fc45-112">Collect information about hello image</span></span>

<span data-ttu-id="1fc45-113">Nejprve nutné toogather základní informace o bitových hello a vytvořte proměnnou pro bitovou kopii hello.</span><span class="sxs-lookup"><span data-stu-id="1fc45-113">First we need toogather basic information about hello image and create a variable for hello image.</span></span> <span data-ttu-id="1fc45-114">Tento příklad používá spravované image virtuálního počítače s názvem **myImage** který je v hello **myResourceGroup** skupiny prostředků v hello **– Západ střední USA** umístění.</span><span class="sxs-lookup"><span data-stu-id="1fc45-114">This example uses a managed VM image named **myImage** that is in hello **myResourceGroup** resource group in hello **West Central US** location.</span></span> 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a><span data-ttu-id="1fc45-115">Vytvoření virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1fc45-115">Create a virtual network</span></span>
<span data-ttu-id="1fc45-116">Vytvoření hello virtuální síť a podsíť hello [virtuální sítě](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1fc45-116">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="1fc45-117">Vytvořte podsíť hello.</span><span class="sxs-lookup"><span data-stu-id="1fc45-117">Create hello subnet.</span></span> <span data-ttu-id="1fc45-118">Tento příklad vytvoří podsíť s názvem **mySubnet** s předponou adresy hello z **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="1fc45-118">This example creates a subnet named **mySubnet** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="1fc45-119">Vytvoření virtuální sítě hello.</span><span class="sxs-lookup"><span data-stu-id="1fc45-119">Create hello virtual network.</span></span> <span data-ttu-id="1fc45-120">Tento příklad vytvoří virtuální síť s názvem **myVnet** s předponou adresy hello z **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="1fc45-120">This example creates a virtual network named **myVnet** with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="1fc45-121">Vytvoření veřejné IP adresy a síťového rozhraní</span><span class="sxs-lookup"><span data-stu-id="1fc45-121">Create a public IP address and network interface</span></span>

<span data-ttu-id="1fc45-122">tooenable komunikaci s hello virtuálním počítačem ve virtuální síti hello, musíte [veřejnou IP adresu](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="1fc45-122">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="1fc45-123">Vytvoření veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="1fc45-123">Create a public IP address.</span></span> <span data-ttu-id="1fc45-124">Tento příklad vytvoří veřejnou IP adresu s názvem **myPip**.</span><span class="sxs-lookup"><span data-stu-id="1fc45-124">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="1fc45-125">Vytvoření hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="1fc45-125">Create hello NIC.</span></span> <span data-ttu-id="1fc45-126">Tento příklad vytvoří síťový adaptér s názvem **myNic**.</span><span class="sxs-lookup"><span data-stu-id="1fc45-126">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="1fc45-127">Vytvořte skupinu zabezpečení sítě hello a pravidlo protokolu RDP</span><span class="sxs-lookup"><span data-stu-id="1fc45-127">Create hello network security group and an RDP rule</span></span>

<span data-ttu-id="1fc45-128">možnost toolog toobe v tooyour virtuálního počítače pomocí protokolu RDP, je nutné toohave pravidla zabezpečení sítě (NSG), který umožňuje přístup RDP na portu 3389.</span><span class="sxs-lookup"><span data-stu-id="1fc45-128">toobe able toolog in tooyour VM using RDP, you need toohave a network security rule (NSG) that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="1fc45-129">Tento příklad vytvoří skupinu NSG s názvem **myNsg** obsahující pravidlo názvem **myRdpRule** přes port 3389 umožňuje provoz protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="1fc45-129">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="1fc45-130">Další informace o skupinách Nsg najdete v tématu [otevřít porty tooa virtuálních počítačů v Azure pomocí prostředí PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1fc45-130">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="1fc45-131">Vytvořte proměnnou pro virtuální síť hello</span><span class="sxs-lookup"><span data-stu-id="1fc45-131">Create a variable for hello virtual network</span></span>

<span data-ttu-id="1fc45-132">Vytvořte proměnnou pro virtuální síť hello byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="1fc45-132">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a><span data-ttu-id="1fc45-133">Získání přihlašovacích údajů hello pro hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1fc45-133">Get hello credentials for hello VM</span></span>

<span data-ttu-id="1fc45-134">Hello následující rutiny se otevře okno, kde bude zadat nové toouse jméno a heslo uživatele jako účet místního správce hello pro vzdálený přístup k hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1fc45-134">hello following cmdlet will open a window where you will enter a new user name and password toouse as hello local administrator account for remotely accessing hello VM.</span></span> 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-hello-vm-name-computer-name-and-hello-size-of-hello-vm"></a><span data-ttu-id="1fc45-135">Sada proměnných pro hello virtuálních počítačů název, název počítače a hello velikost hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1fc45-135">Set variables for hello VM name, computer name and hello size of hello VM</span></span>

1. <span data-ttu-id="1fc45-136">Vytváření proměnných pro hello název virtuálního počítače a název počítače.</span><span class="sxs-lookup"><span data-stu-id="1fc45-136">Create variables for hello VM name and computer name.</span></span> <span data-ttu-id="1fc45-137">Tento příklad nastaví hello název virtuálního počítače jako **Můjvp** a název počítače hello jako **tento počítač**.</span><span class="sxs-lookup"><span data-stu-id="1fc45-137">This example sets hello VM name as **myVM** and hello computer name as **myComputer**.</span></span>

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. <span data-ttu-id="1fc45-138">Nastavení velikosti hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1fc45-138">Set hello size of hello virtual machine.</span></span> <span data-ttu-id="1fc45-139">Tento příklad vytvoří **Standard_DS1_v2** velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1fc45-139">This example creates **Standard_DS1_v2** sized VM.</span></span> <span data-ttu-id="1fc45-140">V tématu hello [velikosti virtuálních počítačů](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) Další informace naleznete v dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1fc45-140">See hello [VM sizes](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) documentation for more information.</span></span>

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. <span data-ttu-id="1fc45-141">Přidejte hello název virtuálního počítače a konfigurace virtuálního počítače toohello velikost.</span><span class="sxs-lookup"><span data-stu-id="1fc45-141">Add hello VM name and size toohello VM configuration.</span></span>

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a><span data-ttu-id="1fc45-142">Sada hello image virtuálního počítače jako zdroj bitové kopie pro hello nového virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1fc45-142">Set hello VM image as source image for hello new VM</span></span>

<span data-ttu-id="1fc45-143">Nastavit hello zdrojové bitové kopie pomocí ID hello hello spravované image virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1fc45-143">Set hello source image using hello ID of hello managed VM image.</span></span>

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a><span data-ttu-id="1fc45-144">Nastavte hello konfigurace operačního systému a přidejte hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="1fc45-144">Set hello OS configuration and add hello NIC.</span></span>

<span data-ttu-id="1fc45-145">Zadejte typ úložiště hello (PremiumLRS nebo StandardLRS) a velikost hello hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="1fc45-145">Enter hello storage type (PremiumLRS or StandardLRS) and hello size of hello OS disk.</span></span> <span data-ttu-id="1fc45-146">Tento příklad nastaví typ účtu hello příliš**PremiumLRS**, hello velikost disku příliš**128 GB** a disku ukládání do mezipaměti příliš**ReadWrite**.</span><span class="sxs-lookup"><span data-stu-id="1fc45-146">This example sets hello account type too**PremiumLRS**, hello disk size too**128 GB** and disk caching too**ReadWrite**.</span></span>

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -StorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a><span data-ttu-id="1fc45-147">Vytvoření hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1fc45-147">Create hello VM</span></span>

<span data-ttu-id="1fc45-148">Vytvoření nového virtuálního počítače pomocí hello konfigurace, který jsme vytvořen a uložen v hello hello **$vm** proměnné.</span><span class="sxs-lookup"><span data-stu-id="1fc45-148">Create hello new Vm using hello configuration that we have built and stored in hello **$vm** variable.</span></span>

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="1fc45-149">Ověřte, že hello, ke které byl virtuální počítač vytvořen</span><span class="sxs-lookup"><span data-stu-id="1fc45-149">Verify that hello VM was created</span></span>
<span data-ttu-id="1fc45-150">Po dokončení byste měli vidět hello nově vytvořený virtuální počítač v hello [portál Azure](https://portal.azure.com) pod **Procházet** > **virtuální počítače**, nebo pomocí následujících hello Příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="1fc45-150">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="1fc45-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1fc45-151">Next steps</span></span>
<span data-ttu-id="1fc45-152">toomanage nový virtuální počítač s prostředím Azure PowerShell najdete v části [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1fc45-152">toomanage your new virtual machine with Azure PowerShell, see [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

