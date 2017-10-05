---
title: "Vytvoření sady škálování virtuálního počítače Azure | Microsoft Docs"
description: "Vytvoření a nasazení škálování virtuálních počítačů Linux nebo Windows Azure nastavit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, šablonu nebo Visual Studio."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 32af01aa545c541688128a7ae6bbb82a0e046f2d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a><span data-ttu-id="532da-103">Vytvoření a nasazení škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="532da-103">Create and deploy a virtual machine scale set</span></span>
<span data-ttu-id="532da-104">Sady škálování virtuálního počítače můžete snadno můžete nasadit a spravovat jako sada identickými virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="532da-104">Virtual machine scale sets make it easy for you to deploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="532da-105">Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="532da-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="532da-106">Další informace o sadách škálování najdete v tématu [sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="532da-106">For more information about scale sets, see [Virtual machine scale sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="532da-107">V tomto kurzu se dozvíte, jak vytvořit škálovací sadu virtuálních počítačů **bez** pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="532da-107">This tutorial shows you how to create a virtual machine scale set **without** using the Azure portal.</span></span> <span data-ttu-id="532da-108">Informace o tom, jak pomocí portálu Azure najdete v tématu [nastavit postup vytvoření škálování virtuálních počítačů pomocí portálu Azure](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="532da-108">For information about how to use the Azure portal, see [How to create a virtual machine scale set with the Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

>[!NOTE]
><span data-ttu-id="532da-109">Další informace o prostředky Azure Resource Manager najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="532da-109">For more information about Azure Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="sign-in-to-azure"></a><span data-ttu-id="532da-110">Přihlášení k Azure</span><span class="sxs-lookup"><span data-stu-id="532da-110">Sign in to Azure</span></span>

<span data-ttu-id="532da-111">Pokud používáte Azure CLI 2.0 nebo Azure PowerShell k vytvoření sady škálování, musíte nejdřív přihlásit k vašemu předplatnému.</span><span class="sxs-lookup"><span data-stu-id="532da-111">If you're using Azure CLI 2.0 or Azure PowerShell to create a scale set, you first need to sign in to your subscription.</span></span>

<span data-ttu-id="532da-112">Další informace o tom, jak nainstalovat, nastavit a přihlaste se k Azure pomocí Azure CLI nebo prostředí PowerShell najdete v tématu [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) nebo [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="532da-112">For more information about how to install, set up, and sign in to Azure with Azure CLI or PowerShell, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) or [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="532da-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="532da-113">Create a resource group</span></span>

<span data-ttu-id="532da-114">Nejprve musíte vytvořit skupinu prostředků, který je přidružený škálovací sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="532da-114">You first need to create a resource group that the virtual machine scale set is associated with.</span></span>

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a><span data-ttu-id="532da-115">Vytvoření z rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="532da-115">Create from Azure CLI</span></span>

<span data-ttu-id="532da-116">Pomocí rozhraní příkazového řádku Azure můžete vytvořit škálování virtuálního počítače s minimálním úsilím.</span><span class="sxs-lookup"><span data-stu-id="532da-116">With Azure CLI, you can create a virtual machine scale set with minimal effort.</span></span> <span data-ttu-id="532da-117">Pokud nezadáte výchozí hodnoty, poskytnou jsou pro vás.</span><span class="sxs-lookup"><span data-stu-id="532da-117">If you omit default values, they are provided for you.</span></span> <span data-ttu-id="532da-118">Například pokud nezadáte žádné informace o virtuální síti, se pro vás vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="532da-118">For example, if you don't specify any virtual network information, a virtual network is created for you.</span></span> <span data-ttu-id="532da-119">Pokud vynecháte následujících částí, jsou vytvořeny pro můžete:</span><span class="sxs-lookup"><span data-stu-id="532da-119">If you omit the following parts, they are created for you:</span></span> 
- <span data-ttu-id="532da-120">Nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="532da-120">A load balancer</span></span>
- <span data-ttu-id="532da-121">Virtuální síť</span><span class="sxs-lookup"><span data-stu-id="532da-121">A virtual network</span></span>
- <span data-ttu-id="532da-122">Veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="532da-122">A public IP address</span></span>

<span data-ttu-id="532da-123">Při výběru image virtuálního počítače, který chcete použít na škálovací sadu virtuálních počítačů, máte několik možností:</span><span class="sxs-lookup"><span data-stu-id="532da-123">When choosing the virtual machine image that you want to use on the virtual machine scale set, you have a few choices:</span></span>

- <span data-ttu-id="532da-124">NÁZEV URN</span><span class="sxs-lookup"><span data-stu-id="532da-124">URN</span></span>  
<span data-ttu-id="532da-125">Identifikátor prostředku:</span><span class="sxs-lookup"><span data-stu-id="532da-125">The identifier of a resource:</span></span>  
<span data-ttu-id="532da-126">**Win2012R2Datacenter**</span><span class="sxs-lookup"><span data-stu-id="532da-126">**Win2012R2Datacenter**</span></span>

- <span data-ttu-id="532da-127">Název URN alias</span><span class="sxs-lookup"><span data-stu-id="532da-127">URN alias</span></span>  
<span data-ttu-id="532da-128">Popisný název název URN:</span><span class="sxs-lookup"><span data-stu-id="532da-128">The friendly name of a URN:</span></span>  
<span data-ttu-id="532da-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span><span class="sxs-lookup"><span data-stu-id="532da-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span></span>

- <span data-ttu-id="532da-130">Vlastní prostředek id</span><span class="sxs-lookup"><span data-stu-id="532da-130">Custom resource id</span></span>  
<span data-ttu-id="532da-131">Cesta k prostředek Azure:</span><span class="sxs-lookup"><span data-stu-id="532da-131">The path to an Azure resource:</span></span>  
<span data-ttu-id="532da-132">**/subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.COMPUTE/Images/MyImage**</span><span class="sxs-lookup"><span data-stu-id="532da-132">**/subscriptions/subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**</span></span>

- <span data-ttu-id="532da-133">Webové prostředky</span><span class="sxs-lookup"><span data-stu-id="532da-133">Web resource</span></span>  
<span data-ttu-id="532da-134">Cesta k identifikátor URI HTTP:</span><span class="sxs-lookup"><span data-stu-id="532da-134">The path to an HTTP URI:</span></span>  
<span data-ttu-id="532da-135">**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**</span><span class="sxs-lookup"><span data-stu-id="532da-135">**http://contoso.blob.core.windows.net/vhds/osdiskimage.vhd**</span></span>

>[!TIP]
><span data-ttu-id="532da-136">Můžete získat seznam dostupných imagí s `az vm image list`.</span><span class="sxs-lookup"><span data-stu-id="532da-136">You can get a list of available images with `az vm image list`.</span></span>

<span data-ttu-id="532da-137">Pokud chcete vytvořit škálovací sadu virtuálních počítačů, je nutné zadat následující:</span><span class="sxs-lookup"><span data-stu-id="532da-137">To create a virtual machine scale set, you must specify the following:</span></span>

- <span data-ttu-id="532da-138">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="532da-138">Resource group</span></span> 
- <span data-ttu-id="532da-139">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="532da-139">Name</span></span>
- <span data-ttu-id="532da-140">Bitové kopie operačního systému</span><span class="sxs-lookup"><span data-stu-id="532da-140">Operating system image</span></span>
- <span data-ttu-id="532da-141">Informace o ověřování</span><span class="sxs-lookup"><span data-stu-id="532da-141">Authentication information</span></span> 
 
<span data-ttu-id="532da-142">Následující příklad vytvoří sada škálování základní virtuální počítač (Tento krok může trvat několik minut).</span><span class="sxs-lookup"><span data-stu-id="532da-142">The following example creates a basic virtual machine scale set (this step might take a few minutes).</span></span>

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

<span data-ttu-id="532da-143">Po dokončení příkazu budete mít teď vaše sad vytvořený škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="532da-143">Once the command finishes you will now have your virtual machine scale set created.</span></span> <span data-ttu-id="532da-144">Musíte získat IP adresu virtuálního počítače tak, aby k nim mohla připojit.</span><span class="sxs-lookup"><span data-stu-id="532da-144">You may need to get the IP address of the virtual machine so that you can connect to it.</span></span> <span data-ttu-id="532da-145">Pomocí následujícího příkazu můžete získat spoustu různé informace o virtuálním počítači (včetně IP adresa).</span><span class="sxs-lookup"><span data-stu-id="532da-145">You can get a lot of different information about the virtual machine (including the IP address) with the following command.</span></span> 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a><span data-ttu-id="532da-146">Vytvoření z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="532da-146">Create from PowerShell</span></span>

<span data-ttu-id="532da-147">PowerShell je složitější než rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="532da-147">PowerShell is more complicated to use than Azure CLI.</span></span> <span data-ttu-id="532da-148">Zatímco rozhraní příkazového řádku Azure poskytuje výchozí hodnoty pro souvisejícím se sítí prostředky (například nástroje pro vyrovnávání zatížení, IP adresy a virtuální sítě), prostředí PowerShell nemá.</span><span class="sxs-lookup"><span data-stu-id="532da-148">While Azure CLI provides defaults for networking-related resources (such as load balancers, IP addresses, and virtual networks), PowerShell does not.</span></span> <span data-ttu-id="532da-149">Odkazování na bitovou kopii pomocí prostředí PowerShell je něco víc složitá příliš.</span><span class="sxs-lookup"><span data-stu-id="532da-149">Referencing an image with PowerShell is a slightly more complicated too.</span></span> <span data-ttu-id="532da-150">Můžete získat obrázků pomocí následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="532da-150">You can get images with the following cmdlets:</span></span>

1. <span data-ttu-id="532da-151">Get-AzureRMVMImagePublisher</span><span class="sxs-lookup"><span data-stu-id="532da-151">Get-AzureRMVMImagePublisher</span></span>
2. <span data-ttu-id="532da-152">Get-AzureRMVMImageOffer</span><span class="sxs-lookup"><span data-stu-id="532da-152">Get-AzureRMVMImageOffer</span></span>
3. <span data-ttu-id="532da-153">Get-AzureRmVMImageSku</span><span class="sxs-lookup"><span data-stu-id="532da-153">Get-AzureRmVMImageSku</span></span>

<span data-ttu-id="532da-154">Rutiny pracovní lze předat v pořadí.</span><span class="sxs-lookup"><span data-stu-id="532da-154">The cmdlets work can be piped in sequence.</span></span> <span data-ttu-id="532da-155">Tady je příklad toho, jak získat všechny bitové kopie pro **západní USA 2** oblasti s vydavatele, který má název **microsoft** v ní.</span><span class="sxs-lookup"><span data-stu-id="532da-155">Here is an example of how to get all images for the **West US 2** region with a publisher that has the name **microsoft** in it.</span></span>

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

<span data-ttu-id="532da-156">Pracovní postup pro vytváření škálovací sadu virtuálních počítačů je následující:</span><span class="sxs-lookup"><span data-stu-id="532da-156">The workflow for creating a virtual machine scale set is as follows:</span></span>

1. <span data-ttu-id="532da-157">Vytvořte objekt konfigurace, který obsahuje informace o sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="532da-157">Create a config object that holds information about the scale set.</span></span>
2. <span data-ttu-id="532da-158">Odkazovat na základní bitovou kopii operačního systému.</span><span class="sxs-lookup"><span data-stu-id="532da-158">Reference the base OS image.</span></span>
3. <span data-ttu-id="532da-159">Nakonfigurujte nastavení operační systém: ověřování, předpona názvu virtuálního počítače a uživatele nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="532da-159">Configure the operating system settings: authentication, VM name prefix, and user/pass.</span></span>
4. <span data-ttu-id="532da-160">Konfigurace sítí.</span><span class="sxs-lookup"><span data-stu-id="532da-160">Configure networking.</span></span>
5. <span data-ttu-id="532da-161">Vytvoření sady škálování.</span><span class="sxs-lookup"><span data-stu-id="532da-161">Create the scale set.</span></span>

<span data-ttu-id="532da-162">Tento příklad vytvoří základní dvě instance škále nastavení v počítači, který má nainstalovaný Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="532da-162">This example creates a basic two-instance scale set for a computer that has Windows Server 2016 installed.</span></span>

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach the virtual network to the IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a><span data-ttu-id="532da-163">Pomocí bitové kopie vlastního virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="532da-163">Using a custom virtual machine image</span></span>
<span data-ttu-id="532da-164">Pokud vytváříte nastavit od svoji vlastní image, místo odkazující na bitovou kopii virtuálního počítače z galerie, škálování _Set-AzureRmVmssStorageProfile_ příkaz bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="532da-164">If you are creating a scale set from your own custom image, instead of referencing a virtual machine image from the gallery, the _Set-AzureRmVmssStorageProfile_ command would look like this:</span></span>
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a><span data-ttu-id="532da-165">Vytvoření ze šablony</span><span class="sxs-lookup"><span data-stu-id="532da-165">Create from a template</span></span>

<span data-ttu-id="532da-166">Můžete nasadit škálování virtuálních počítačů, nastavit pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="532da-166">You can deploy a virtual machine scale set by using an Azure Resource Manager template.</span></span> <span data-ttu-id="532da-167">Můžete vytvořit vlastní šablonu nebo použijte jednu z [šablony úložiště](https://azure.microsoft.com/resources/templates/?term=vmss).</span><span class="sxs-lookup"><span data-stu-id="532da-167">You can create your own template or use one from the [template repository](https://azure.microsoft.com/resources/templates/?term=vmss).</span></span> <span data-ttu-id="532da-168">Tyto šablony se dá nasadit přímo do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="532da-168">These templates can be deployed directly to your Azure subscription.</span></span>

>[!NOTE]
><span data-ttu-id="532da-169">Pokud chcete vytvořit vlastní šablonu, vytvořte textový soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="532da-169">To create your own template, you create a JSON text file.</span></span> <span data-ttu-id="532da-170">Obecné informace o tom, jak vytvořit a přizpůsobení šablony najdete v tématu [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="532da-170">For general information about how to create and customize a template, see [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="532da-171">Vzorové šablony je k dispozici [na Githubu](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span><span class="sxs-lookup"><span data-stu-id="532da-171">A sample template is available [on GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span></span> <span data-ttu-id="532da-172">Další informace o tom, jak vytvořit a použít tuto ukázku najdete v tématu [minimální přijatelná škálovací sadu](.\virtual-machine-scale-sets-mvss-start.md).</span><span class="sxs-lookup"><span data-stu-id="532da-172">For more information about how to create and use that sample, see [Minimum viable scale set](.\virtual-machine-scale-sets-mvss-start.md).</span></span>

## <a name="create-from-visual-studio"></a><span data-ttu-id="532da-173">Vytvořit ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="532da-173">Create from Visual Studio</span></span>

<span data-ttu-id="532da-174">Pomocí sady Visual Studio můžete vytvořit projekt skupiny prostředků Azure a přidat že škálovací sady virtuálních počítačů šablony do ní.</span><span class="sxs-lookup"><span data-stu-id="532da-174">With Visual Studio, you can create an Azure resource group project and add a virtual machine scale set template to it.</span></span> <span data-ttu-id="532da-175">Můžete zvolit, jestli chcete importovat z webu GitHub nebo Galerie webových aplikací Azure.</span><span class="sxs-lookup"><span data-stu-id="532da-175">You can choose whether you want to import it from GitHub or the Azure Web Application Gallery.</span></span> <span data-ttu-id="532da-176">Nasazení skript prostředí PowerShell je vytvořena také za vás.</span><span class="sxs-lookup"><span data-stu-id="532da-176">A deployment PowerShell script is also generated for you.</span></span> <span data-ttu-id="532da-177">Další informace najdete v tématu [jak vytvořit škálování virtuálních počítačů sady pomocí sady Visual Studio](virtual-machine-scale-sets-vs-create.md).</span><span class="sxs-lookup"><span data-stu-id="532da-177">For more information, see [How to create a virtual machine scale set with Visual Studio](virtual-machine-scale-sets-vs-create.md).</span></span>

## <a name="create-from-the-azure-portal"></a><span data-ttu-id="532da-178">Vytvoření z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="532da-178">Create from the Azure portal</span></span>

<span data-ttu-id="532da-179">Portál Azure poskytuje pohodlný způsob, jak rychle vytvořit škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="532da-179">The Azure portal provides a convenient way to quickly create a scale set.</span></span> <span data-ttu-id="532da-180">Další informace najdete v tématu [nastavit postup vytvoření škálování virtuálních počítačů pomocí portálu Azure](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="532da-180">For more information, see [How to create a virtual machine scale set with the Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="532da-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="532da-181">Next steps</span></span>

<span data-ttu-id="532da-182">Další informace o [datových disků](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="532da-182">Learn more about [data disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="532da-183">Zjistěte, jak [správě aplikací](virtual-machine-scale-sets-deploy-app.md).</span><span class="sxs-lookup"><span data-stu-id="532da-183">Learn how to [manage your apps](virtual-machine-scale-sets-deploy-app.md).</span></span>
