---
title: "Kurz sady dostupnosti pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o skupiny dostupnosti pro virtuální počítače Windows v Azure."
documentationcenter: 
services: virtual-machines-windows
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
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d918362106ef93cf47620e0018d363cd510884b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-availability-sets"></a><span data-ttu-id="07008-103">Jak používat skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-103">How to use availability sets</span></span>

<span data-ttu-id="07008-104">V tomto kurzu se dozvíte, jak zvýšit dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="07008-104">In this tutorial, you will learn how to increase the availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="07008-105">Skupiny dostupnosti Ujistěte se, že virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru.</span><span class="sxs-lookup"><span data-stu-id="07008-105">Availability sets ensure that the VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="07008-106">To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z perspektivy zákazníky ho pomocí.</span><span class="sxs-lookup"><span data-stu-id="07008-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from the perspective of your customers using it.</span></span> 

<span data-ttu-id="07008-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="07008-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07008-108">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-108">Create an availability set</span></span>
> * <span data-ttu-id="07008-109">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="07008-110">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="07008-110">Check available VM sizes</span></span>

<span data-ttu-id="07008-111">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="07008-111">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="07008-112">Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="07008-112">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="07008-113">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="07008-113">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="availability-set-overview"></a><span data-ttu-id="07008-114">Přehled sady dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-114">Availability set overview</span></span>

<span data-ttu-id="07008-115">Skupiny dostupnosti je logické seskupení funkci, která můžete použít v Azure k zajištění, že prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="07008-115">An Availability Set is a logical grouping capability that you can use in Azure to ensure that the VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="07008-116">Azure zajistí, že virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="07008-116">Azure ensures that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="07008-117">To zajistí, že v případě hardwaru nebo softwaru Azure selhání, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkové zůstane nahoru a nadále k dispozici pro vaše zákazníky.</span><span class="sxs-lookup"><span data-stu-id="07008-117">This ensures that in the event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue to be available to your customers.</span></span> <span data-ttu-id="07008-118">Pomocí nastavení dostupnosti je nezbytné schopnost využít, pokud chcete vytvářet spolehlivé cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="07008-118">Using Availability Sets is an essential capability to leverage when you want to build reliable cloud solutions.</span></span>

<span data-ttu-id="07008-119">Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze.</span><span class="sxs-lookup"><span data-stu-id="07008-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="07008-120">S Azure, by chcete definovat dvě sady dostupnosti před nasazením virtuálních počítačů: jednu sadu dostupnosti pro vrstvu "web" a jednu sadu dostupnosti pro vrstvu "databáze".</span><span class="sxs-lookup"><span data-stu-id="07008-120">With Azure, you’d want to define two availability sets before you deploy your VMs: one availability set for the “web” tier and one availability set for the “database” tier.</span></span> <span data-ttu-id="07008-121">Při vytváření nového virtuálního počítače můžete určit skupiny dostupnosti jako parametr pro virtuální počítač az vytvoření příkazu a Azure automaticky zajistí, že virtuální počítače, vytvořte v rámci sady k dispozici jsou izolované napříč několika prostředcích fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="07008-121">When you create a new VM you can then specify the availability set as a parameter to the az vm create command, and Azure will automatically ensure that the VMs you create within the available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="07008-122">To znamená, že pokud fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.</span><span class="sxs-lookup"><span data-stu-id="07008-122">This means that if the physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that the other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="07008-123">Pokud chcete nasadit spolehlivé řešení virtuálních počítačů na základě v rámci Azure byste měli vždycky používat skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="07008-123">You should always use Availability Sets when you want to deploy reliable VM based solutions within Azure.</span></span>

## <a name="create-an-availability-set"></a><span data-ttu-id="07008-124">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-124">Create an availability set</span></span>

<span data-ttu-id="07008-125">Můžete vytvořit pomocí sadu dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="07008-125">You can create an availability set using [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="07008-126">V tomto příkladu jsme nastavit počet aktualizací a chyby domén v *2* pro skupinu dostupnosti s názvem *myAvailabilitySet* v *myResourceGroupAvailability* Skupina prostředků.</span><span class="sxs-lookup"><span data-stu-id="07008-126">In this example, we set both the number of update and fault domains at *2* for the availability set named *myAvailabilitySet* in the *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="07008-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="07008-127">Create a resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="07008-128">Vytvořit virtuální počítače uvnitř skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-128">Create VMs inside an availability set</span></span>

<span data-ttu-id="07008-129">Virtuální počítače musí být vytvořen v rámci skupinu dostupnosti a ujistěte se, že jsou správně rozmístěny v hardwaru.</span><span class="sxs-lookup"><span data-stu-id="07008-129">VMs need to be created within the availability set to make sure they are correctly distributed across the hardware.</span></span> <span data-ttu-id="07008-130">Nelze přidat existující virtuální počítač po vytvoření sadu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="07008-130">You can't add an existing VM to an availability set after it is created.</span></span> 

<span data-ttu-id="07008-131">Hardware v umístění je rozděleno v několika aktualizace domén a domén selhání.</span><span class="sxs-lookup"><span data-stu-id="07008-131">The hardware in a location is divided in to multiple update domains and fault domains.</span></span> <span data-ttu-id="07008-132">**Aktualizace domény** je skupina virtuální počítače a základní fyzický hardware, který může být restartován ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="07008-132">An **update domain** is a group of VMs and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="07008-133">Virtuální počítače ve stejné **doména selhání** sdílejí společné úložiště, jakož i běžné zdroje a sítě vypínač.</span><span class="sxs-lookup"><span data-stu-id="07008-133">VMs in the same **fault domain** share common storage as well as a common power source and network switch.</span></span> 

<span data-ttu-id="07008-134">Při vytváření virtuálního počítače pomocí konfigurace pomocí [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) zadáte skupinu dostupnosti pomocí `-AvailabilitySetId` parametru určete ID skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="07008-134">When you create a VM using configuration using [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) you specify the availability set using the `-AvailabilitySetId` parameter to specify the ID of the availability set.</span></span>

<span data-ttu-id="07008-135">Vytvoření 2 virtuální počítače s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ve dostupnosti nastavit.</span><span class="sxs-lookup"><span data-stu-id="07008-135">Create 2 VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in the availability set.</span></span>

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

<span data-ttu-id="07008-136">Trvá několik minut vytvořit a nakonfigurovat oba virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="07008-136">It takes a few minutes to create and configure both VMs.</span></span> <span data-ttu-id="07008-137">Po dokončení bude mít virtuální počítače 2 rozložené mezi základní hardware.</span><span class="sxs-lookup"><span data-stu-id="07008-137">When finished, you will have 2 virtual machines distributed across the underlying hardware.</span></span> 

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="07008-138">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="07008-138">Check for available VM sizes</span></span> 

<span data-ttu-id="07008-139">Můžete přidat další virtuální počítače pro skupinu dostupnosti později, ale musíte vědět, jaké velikosti virtuálních počítačů jsou k dispozici na hardware.</span><span class="sxs-lookup"><span data-stu-id="07008-139">You can add more VMs to the availability set later, but you need to know what VM sizes are available on the hardware.</span></span> <span data-ttu-id="07008-140">Použití [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) seznam všech dostupných velikostí na hardwaru clusteru pro skupinu dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="07008-140">Use [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) to list all the available sizes on the hardware cluster for the availability set.</span></span>

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a><span data-ttu-id="07008-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07008-141">Next steps</span></span>

<span data-ttu-id="07008-142">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="07008-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="07008-143">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-143">Create an availability set</span></span>
> * <span data-ttu-id="07008-144">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="07008-144">Create a VM in an availability set</span></span>
> * <span data-ttu-id="07008-145">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="07008-145">Check available VM sizes</span></span>

<span data-ttu-id="07008-146">Přechodu na v dalším kurzu se dozvíte o sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="07008-146">Advance to the next tutorial to learn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="07008-147">Vytvoření škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="07008-147">Create a VM scale set</span></span>](tutorial-create-vmss.md)


