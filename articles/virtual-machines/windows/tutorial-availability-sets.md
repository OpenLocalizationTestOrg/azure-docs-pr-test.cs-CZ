---
title: "aaaAvailability nastaví kurz pro virtuální počítače Windows v Azure | Microsoft Docs"
description: "Další informace o hello dostupnosti nastaví pro virtuální počítače Windows v Azure."
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
ms.openlocfilehash: 853775c5f126dd815c1933f9d71d2274a75ea661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a><span data-ttu-id="f8479-103">Jak nastaví toouse dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-103">How toouse availability sets</span></span>

<span data-ttu-id="f8479-104">V tomto kurzu se dozvíte, jak tooincrease hello dostupnost a spolehlivost řešení virtuálního počítače v Azure pomocí funkce názvem skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="f8479-104">In this tutorial, you will learn how tooincrease hello availability and reliability of your Virtual Machine solutions on Azure using a capability called Availability Sets.</span></span> <span data-ttu-id="f8479-105">Skupiny dostupnosti Ujistěte se, že hello virtuálních počítačů nasadit v Azure jsou distribuovány na více clusterů izolované hardwaru.</span><span class="sxs-lookup"><span data-stu-id="f8479-105">Availability sets ensure that hello VMs you deploy on Azure are distributed across multiple isolated hardware clusters.</span></span> <span data-ttu-id="f8479-106">To zajišťuje, že pokud dojde selhání hardwaru nebo softwaru v rámci Azure, bude mít vliv pouze dílčí sadu virtuálních počítačů a že celkového řešení zůstanou dostupné a funkční z hlediska hello zákazníků použití.</span><span class="sxs-lookup"><span data-stu-id="f8479-106">Doing this ensures that if a hardware or software failure within Azure happens, only a sub-set of your VMs will be impacted and that your overall solution will remain available and operational from hello perspective of your customers using it.</span></span> 

<span data-ttu-id="f8479-107">V tomto kurzu se naučíte:</span><span class="sxs-lookup"><span data-stu-id="f8479-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8479-108">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-108">Create an availability set</span></span>
> * <span data-ttu-id="f8479-109">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-109">Create a VM in an availability set</span></span>
> * <span data-ttu-id="f8479-110">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f8479-110">Check available VM sizes</span></span>

<span data-ttu-id="f8479-111">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f8479-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="f8479-112">Spustit ` Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="f8479-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="f8479-113">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="f8479-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="availability-set-overview"></a><span data-ttu-id="f8479-114">Přehled sady dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-114">Availability set overview</span></span>

<span data-ttu-id="f8479-115">Skupiny dostupnosti je logické seskupení schopností, který můžete použít v Azure tooensure že hello prostředky virtuálních počítačů, které umístíte v něm jsou od sebe navzájem oddělené, když jsou nasazeny v rámci datového centra Azure.</span><span class="sxs-lookup"><span data-stu-id="f8479-115">An Availability Set is a logical grouping capability that you can use in Azure tooensure that hello VM resources you place within it are isolated from each other when they are deployed within an Azure datacenter.</span></span> <span data-ttu-id="f8479-116">Azure zajistí, že hello virtuální počítače umístíte do skupiny dostupnosti spustit více fyzických serverů, výpočetní stojany, jednotky úložiště a síťové přepínače.</span><span class="sxs-lookup"><span data-stu-id="f8479-116">Azure ensures that hello VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches.</span></span> <span data-ttu-id="f8479-117">To zajistí, že hello události hardwaru nebo softwaru Azure chyby, bude mít vliv jenom podmnožinu virtuální počítače a aplikace celkový zůstane nahoru a bude pokračovat zákazníky k dispozici tooyour toobe.</span><span class="sxs-lookup"><span data-stu-id="f8479-117">This ensures that in hello event of a hardware or Azure software failure, only a subset of your VMs will be impacted, and your overall application will stay up and continue toobe available tooyour customers.</span></span> <span data-ttu-id="f8479-118">Pomocí nastavení dostupnosti je tooleverage nezbytné funkce, pokud chcete toobuild spolehlivé cloudové řešení.</span><span class="sxs-lookup"><span data-stu-id="f8479-118">Using Availability Sets is an essential capability tooleverage when you want toobuild reliable cloud solutions.</span></span>

<span data-ttu-id="f8479-119">Pojďme typické řešení virtuálních počítačů na základě kterých může mít 4 front-end webové servery a používat 2 back-end virtuální počítače, které jsou hostiteli databáze.</span><span class="sxs-lookup"><span data-stu-id="f8479-119">Let’s consider a typical VM-based solution where you might have 4 front-end web servers and use 2 back-end VMs that host a database.</span></span> <span data-ttu-id="f8479-120">S Azure, je vhodnější toodefine dvě sady dostupnosti před nasazením virtuálních počítačů: sadu jeden dostupnosti pro vrstvu "web" hello a jednu sadu dostupnosti pro vrstvu "databáze" hello.</span><span class="sxs-lookup"><span data-stu-id="f8479-120">With Azure, you’d want toodefine two availability sets before you deploy your VMs: one availability set for hello “web” tier and one availability set for hello “database” tier.</span></span> <span data-ttu-id="f8479-121">Při vytváření nového virtuálního počítače můžete určit hello dostupnosti jako virtuální počítač parametr toohello az vytvoření příkazu a Azure automaticky zajistí, že hello virtuálních počítačů, které vytvoříte v rámci hello k dispozici sady jsou izolovány napříč několika prostředcích fyzický hardware.</span><span class="sxs-lookup"><span data-stu-id="f8479-121">When you create a new VM you can then specify hello availability set as a parameter toohello az vm create command, and Azure will automatically ensure that hello VMs you create within hello available set are isolated across multiple physical hardware resources.</span></span> <span data-ttu-id="f8479-122">To znamená, že pokud hello fyzický hardware, který webový Server nebo virtuální počítače databáze serveru běží na problém, víte, že hello ostatní instance webového serveru a databáze virtuální počítače zůstanou spuštěné bez problémů vzhledem k tomu, že jsou na jiný hardware.</span><span class="sxs-lookup"><span data-stu-id="f8479-122">This means that if hello physical hardware that one of your Web Server or Database Server VMs is running on has a problem, you know that hello other instances of your Web Server and Database VMs will remain running fine because they are on different hardware.</span></span>

<span data-ttu-id="f8479-123">Pokud chcete toodeploy spolehlivé řešení virtuálních počítačů na základě v rámci Azure byste měli vždycky používat skupiny dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="f8479-123">You should always use Availability Sets when you want toodeploy reliable VM based solutions within Azure.</span></span>

## <a name="create-an-availability-set"></a><span data-ttu-id="f8479-124">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-124">Create an availability set</span></span>

<span data-ttu-id="f8479-125">Můžete vytvořit pomocí sadu dostupnosti [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span><span class="sxs-lookup"><span data-stu-id="f8479-125">You can create an availability set using [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset).</span></span> <span data-ttu-id="f8479-126">V tomto příkladu jsme nastavit počet aktualizací a chyby domén v obou hello *2* pro sadu s názvem dostupnosti hello *myAvailabilitySet* v hello *myResourceGroupAvailability*skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f8479-126">In this example, we set both hello number of update and fault domains at *2* for hello availability set named *myAvailabilitySet* in hello *myResourceGroupAvailability* resource group.</span></span>

<span data-ttu-id="f8479-127">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="f8479-127">Create a resource group.</span></span>

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

## <a name="create-vms-inside-an-availability-set"></a><span data-ttu-id="f8479-128">Vytvořit virtuální počítače uvnitř skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-128">Create VMs inside an availability set</span></span>

<span data-ttu-id="f8479-129">Virtuální počítače nutné toobe vytvořen v rámci toomake sadu dostupnosti hello se, že jsou správně distribuovány mezi hello hardwaru.</span><span class="sxs-lookup"><span data-stu-id="f8479-129">VMs need toobe created within hello availability set toomake sure they are correctly distributed across hello hardware.</span></span> <span data-ttu-id="f8479-130">Nelze přidat sadu po vytvoření existující tooan dostupnosti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f8479-130">You can't add an existing VM tooan availability set after it is created.</span></span> 

<span data-ttu-id="f8479-131">Hello hardwaru na místo je rozděleno v toomultiple aktualizace domén a domén selhání.</span><span class="sxs-lookup"><span data-stu-id="f8479-131">hello hardware in a location is divided in toomultiple update domains and fault domains.</span></span> <span data-ttu-id="f8479-132">**Aktualizace domény** je skupina virtuální počítače a základní fyzický hardware, který může být restartován v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="f8479-132">An **update domain** is a group of VMs and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="f8479-133">Virtuální počítače ve stejné hello **doména selhání** sdílejí společné úložiště, jakož i běžné zdroje a sítě vypínač.</span><span class="sxs-lookup"><span data-stu-id="f8479-133">VMs in hello same **fault domain** share common storage as well as a common power source and network switch.</span></span> 

<span data-ttu-id="f8479-134">Při vytváření virtuálního počítače pomocí konfigurace pomocí [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) zadejte hello dostupnosti pomocí hello `-AvailabilitySetId` parametr toospecify hello ID sady dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="f8479-134">When you create a VM using configuration using [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) you specify hello availability set using hello `-AvailabilitySetId` parameter toospecify hello ID of hello availability set.</span></span>

<span data-ttu-id="f8479-135">Vytvoření 2 virtuální počítače s [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) v sadě dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="f8479-135">Create 2 VMs with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) in hello availability set.</span></span>

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

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

   # Here is where we specify hello availability set
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

<span data-ttu-id="f8479-136">Trvá několik minut toocreate a nakonfigurujte oba virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f8479-136">It takes a few minutes toocreate and configure both VMs.</span></span> <span data-ttu-id="f8479-137">Po dokončení, budete mít 2 virtuální počítače distribuované ve hello základní hardware.</span><span class="sxs-lookup"><span data-stu-id="f8479-137">When finished, you will have 2 virtual machines distributed across hello underlying hardware.</span></span> 

## <a name="check-for-available-vm-sizes"></a><span data-ttu-id="f8479-138">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f8479-138">Check for available VM sizes</span></span> 

<span data-ttu-id="f8479-139">Můžete přidat další virtuální počítače toohello dostupnosti později, ale potřebujete tooknow jaké velikosti virtuálních počítačů jsou k dispozici na hello hardwaru.</span><span class="sxs-lookup"><span data-stu-id="f8479-139">You can add more VMs toohello availability set later, but you need tooknow what VM sizes are available on hello hardware.</span></span> <span data-ttu-id="f8479-140">Použití [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist všech dostupných velikostí hello na hello hardwaru clusteru pro sadu dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="f8479-140">Use [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist all hello available sizes on hello hardware cluster for hello availability set.</span></span>

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a><span data-ttu-id="f8479-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8479-141">Next steps</span></span>

<span data-ttu-id="f8479-142">V tomto kurzu jste se naučili:</span><span class="sxs-lookup"><span data-stu-id="f8479-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8479-143">Vytvoření skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-143">Create an availability set</span></span>
> * <span data-ttu-id="f8479-144">Vytvoření virtuálního počítače v nastavení dostupnosti</span><span class="sxs-lookup"><span data-stu-id="f8479-144">Create a VM in an availability set</span></span>
> * <span data-ttu-id="f8479-145">Zkontrolujte dostupné velikosti virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f8479-145">Check available VM sizes</span></span>

<span data-ttu-id="f8479-146">Posunutí další kurz toolearn toohello o sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f8479-146">Advance toohello next tutorial toolearn about virtual machine scale sets.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f8479-147">Vytvoření škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="f8479-147">Create a VM scale set</span></span>](tutorial-create-vmss.md)


