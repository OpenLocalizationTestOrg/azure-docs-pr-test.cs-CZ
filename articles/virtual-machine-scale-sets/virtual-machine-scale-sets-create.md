---
title: "aaaCreate Azure škálovací sady virtuálních počítačů | Microsoft Docs"
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
ms.openlocfilehash: 73de25c1dd2424e64655b3accfea848926e72f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a><span data-ttu-id="183fe-103">Vytvoření a nasazení škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="183fe-103">Create and deploy a virtual machine scale set</span></span>
<span data-ttu-id="183fe-104">Sady škálování virtuálního počítače můžete snadno můžete toodeploy a spravovat identickými virtuálními počítači jako sada.</span><span class="sxs-lookup"><span data-stu-id="183fe-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="183fe-105">Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="183fe-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="183fe-106">Další informace o sadách škálování najdete v tématu [sady škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-106">For more information about scale sets, see [Virtual machine scale sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="183fe-107">Tento kurz ukazuje, jak toocreate škálovací sady virtuálních počítačů **bez** pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="183fe-107">This tutorial shows you how toocreate a virtual machine scale set **without** using hello Azure portal.</span></span> <span data-ttu-id="183fe-108">Informace o tom, jak toouse hello portálu Azure najdete v tématu [jak toocreate škálovací sady virtuálních počítačů s hello portál Azure](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-108">For information about how toouse hello Azure portal, see [How toocreate a virtual machine scale set with hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

>[!NOTE]
><span data-ttu-id="183fe-109">Další informace o prostředky Azure Resource Manager najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-109">For more information about Azure Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="sign-in-tooazure"></a><span data-ttu-id="183fe-110">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="183fe-110">Sign in tooAzure</span></span>

<span data-ttu-id="183fe-111">Pokud používáte Azure CLI 2.0 nebo Azure PowerShell toocreate škálování nastavit, musíte nejprve toosign v tooyour předplatného.</span><span class="sxs-lookup"><span data-stu-id="183fe-111">If you're using Azure CLI 2.0 or Azure PowerShell toocreate a scale set, you first need toosign in tooyour subscription.</span></span>

<span data-ttu-id="183fe-112">Další informace o tom, jak vidíte tooinstall, nastavení a přihlaste se pomocí rozhraní příkazového řádku Azure nebo prostředí PowerShell, tooAzure [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) nebo [Začínáme pomocí rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="183fe-112">For more information about how tooinstall, set up, and sign in tooAzure with Azure CLI or PowerShell, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) or [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="183fe-113">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="183fe-113">Create a resource group</span></span>

<span data-ttu-id="183fe-114">Je nutné nejprve toocreate, který je přidružen skupinu prostředků, hello škálovací sady virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="183fe-114">You first need toocreate a resource group that hello virtual machine scale set is associated with.</span></span>

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a><span data-ttu-id="183fe-115">Vytvoření z rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="183fe-115">Create from Azure CLI</span></span>

<span data-ttu-id="183fe-116">Pomocí rozhraní příkazového řádku Azure můžete vytvořit škálování virtuálního počítače s minimálním úsilím.</span><span class="sxs-lookup"><span data-stu-id="183fe-116">With Azure CLI, you can create a virtual machine scale set with minimal effort.</span></span> <span data-ttu-id="183fe-117">Pokud nezadáte výchozí hodnoty, poskytnou jsou pro vás.</span><span class="sxs-lookup"><span data-stu-id="183fe-117">If you omit default values, they are provided for you.</span></span> <span data-ttu-id="183fe-118">Například pokud nezadáte žádné informace o virtuální síti, se pro vás vytvoří virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="183fe-118">For example, if you don't specify any virtual network information, a virtual network is created for you.</span></span> <span data-ttu-id="183fe-119">Pokud vynecháte hello následující části jsou vytvořeny pro můžete:</span><span class="sxs-lookup"><span data-stu-id="183fe-119">If you omit hello following parts, they are created for you:</span></span> 
- <span data-ttu-id="183fe-120">Nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="183fe-120">A load balancer</span></span>
- <span data-ttu-id="183fe-121">Virtuální síť</span><span class="sxs-lookup"><span data-stu-id="183fe-121">A virtual network</span></span>
- <span data-ttu-id="183fe-122">Veřejnou IP adresu</span><span class="sxs-lookup"><span data-stu-id="183fe-122">A public IP address</span></span>

<span data-ttu-id="183fe-123">Při výběru hello bitovou kopii virtuálního počítače, které chcete toouse na škálovací sadu virtuálních počítačů hello, máte několik možností:</span><span class="sxs-lookup"><span data-stu-id="183fe-123">When choosing hello virtual machine image that you want toouse on hello virtual machine scale set, you have a few choices:</span></span>

- <span data-ttu-id="183fe-124">NÁZEV URN</span><span class="sxs-lookup"><span data-stu-id="183fe-124">URN</span></span>  
<span data-ttu-id="183fe-125">Hello identifikátor prostředku:</span><span class="sxs-lookup"><span data-stu-id="183fe-125">hello identifier of a resource:</span></span>  
<span data-ttu-id="183fe-126">**Win2012R2Datacenter**</span><span class="sxs-lookup"><span data-stu-id="183fe-126">**Win2012R2Datacenter**</span></span>

- <span data-ttu-id="183fe-127">Název URN alias</span><span class="sxs-lookup"><span data-stu-id="183fe-127">URN alias</span></span>  
<span data-ttu-id="183fe-128">Hello popisný název název URN:</span><span class="sxs-lookup"><span data-stu-id="183fe-128">hello friendly name of a URN:</span></span>  
<span data-ttu-id="183fe-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span><span class="sxs-lookup"><span data-stu-id="183fe-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span></span>

- <span data-ttu-id="183fe-130">Vlastní prostředek id</span><span class="sxs-lookup"><span data-stu-id="183fe-130">Custom resource id</span></span>  
<span data-ttu-id="183fe-131">Cesta tooan Hello prostředků Azure:</span><span class="sxs-lookup"><span data-stu-id="183fe-131">hello path tooan Azure resource:</span></span>  
<span data-ttu-id="183fe-132">**/subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/providers/Microsoft.COMPUTE/Images/MyImage**</span><span class="sxs-lookup"><span data-stu-id="183fe-132">**/subscriptions/subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**</span></span>

- <span data-ttu-id="183fe-133">Webové prostředky</span><span class="sxs-lookup"><span data-stu-id="183fe-133">Web resource</span></span>  
<span data-ttu-id="183fe-134">Cesta tooan Hello HTTP URI:</span><span class="sxs-lookup"><span data-stu-id="183fe-134">hello path tooan HTTP URI:</span></span>  
<span data-ttu-id="183fe-135">**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.VHD**</span><span class="sxs-lookup"><span data-stu-id="183fe-135">**http://contoso.blob.core.windows.net/vhds/osdiskimage.vhd**</span></span>

>[!TIP]
><span data-ttu-id="183fe-136">Můžete získat seznam dostupných imagí s `az vm image list`.</span><span class="sxs-lookup"><span data-stu-id="183fe-136">You can get a list of available images with `az vm image list`.</span></span>

<span data-ttu-id="183fe-137">toocreate škálovací sady virtuálních počítačů, je nutné zadat následující hello:</span><span class="sxs-lookup"><span data-stu-id="183fe-137">toocreate a virtual machine scale set, you must specify hello following:</span></span>

- <span data-ttu-id="183fe-138">Skupina prostředků</span><span class="sxs-lookup"><span data-stu-id="183fe-138">Resource group</span></span> 
- <span data-ttu-id="183fe-139">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="183fe-139">Name</span></span>
- <span data-ttu-id="183fe-140">Bitové kopie operačního systému</span><span class="sxs-lookup"><span data-stu-id="183fe-140">Operating system image</span></span>
- <span data-ttu-id="183fe-141">Informace o ověřování</span><span class="sxs-lookup"><span data-stu-id="183fe-141">Authentication information</span></span> 
 
<span data-ttu-id="183fe-142">Hello následující ukázka vytvoří sada škálování základní virtuální počítač (Tento krok může trvat několik minut).</span><span class="sxs-lookup"><span data-stu-id="183fe-142">hello following example creates a basic virtual machine scale set (this step might take a few minutes).</span></span>

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

<span data-ttu-id="183fe-143">Po dokončení příkazu hello bude mít teď vaše sad vytvořený škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="183fe-143">Once hello command finishes you will now have your virtual machine scale set created.</span></span> <span data-ttu-id="183fe-144">IP adresa hello tooget hello virtuálního počítače může být nutné, aby mohl připojit tooit.</span><span class="sxs-lookup"><span data-stu-id="183fe-144">You may need tooget hello IP address of hello virtual machine so that you can connect tooit.</span></span> <span data-ttu-id="183fe-145">Mnoho různé informace o virtuálním počítači hello (včetně hello IP adresu) můžete získat s hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="183fe-145">You can get a lot of different information about hello virtual machine (including hello IP address) with hello following command.</span></span> 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a><span data-ttu-id="183fe-146">Vytvoření z prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="183fe-146">Create from PowerShell</span></span>

<span data-ttu-id="183fe-147">PowerShell je složitější toouse než rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="183fe-147">PowerShell is more complicated toouse than Azure CLI.</span></span> <span data-ttu-id="183fe-148">Zatímco rozhraní příkazového řádku Azure poskytuje výchozí hodnoty pro souvisejícím se sítí prostředky (například nástroje pro vyrovnávání zatížení, IP adresy a virtuální sítě), prostředí PowerShell nemá.</span><span class="sxs-lookup"><span data-stu-id="183fe-148">While Azure CLI provides defaults for networking-related resources (such as load balancers, IP addresses, and virtual networks), PowerShell does not.</span></span> <span data-ttu-id="183fe-149">Odkazování na bitovou kopii pomocí prostředí PowerShell je něco víc složitá příliš.</span><span class="sxs-lookup"><span data-stu-id="183fe-149">Referencing an image with PowerShell is a slightly more complicated too.</span></span> <span data-ttu-id="183fe-150">Bitové kopie můžete získat pomocí hello následující rutiny:</span><span class="sxs-lookup"><span data-stu-id="183fe-150">You can get images with hello following cmdlets:</span></span>

1. <span data-ttu-id="183fe-151">Get-AzureRMVMImagePublisher</span><span class="sxs-lookup"><span data-stu-id="183fe-151">Get-AzureRMVMImagePublisher</span></span>
2. <span data-ttu-id="183fe-152">Get-AzureRMVMImageOffer</span><span class="sxs-lookup"><span data-stu-id="183fe-152">Get-AzureRMVMImageOffer</span></span>
3. <span data-ttu-id="183fe-153">Get-AzureRmVMImageSku</span><span class="sxs-lookup"><span data-stu-id="183fe-153">Get-AzureRmVMImageSku</span></span>

<span data-ttu-id="183fe-154">Hello rutiny pracovní lze předat v pořadí.</span><span class="sxs-lookup"><span data-stu-id="183fe-154">hello cmdlets work can be piped in sequence.</span></span> <span data-ttu-id="183fe-155">Tady je příklad, jak tooget všechny bitové kopie pro hello **západní USA 2** oblasti s vydavatele, který má název hello **microsoft** v ní.</span><span class="sxs-lookup"><span data-stu-id="183fe-155">Here is an example of how tooget all images for hello **West US 2** region with a publisher that has hello name **microsoft** in it.</span></span>

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

<span data-ttu-id="183fe-156">Hello pracovní postup pro vytváření škálovací sadu virtuálních počítačů je následující:</span><span class="sxs-lookup"><span data-stu-id="183fe-156">hello workflow for creating a virtual machine scale set is as follows:</span></span>

1. <span data-ttu-id="183fe-157">Vytvořte objekt konfigurace, který obsahuje informace o sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="183fe-157">Create a config object that holds information about hello scale set.</span></span>
2. <span data-ttu-id="183fe-158">Referenční dokumentace hello základní image OS.</span><span class="sxs-lookup"><span data-stu-id="183fe-158">Reference hello base OS image.</span></span>
3. <span data-ttu-id="183fe-159">Nakonfigurujte nastavení operačního systému hello: ověřování, předpona názvu virtuálního počítače a uživatele nebo heslo.</span><span class="sxs-lookup"><span data-stu-id="183fe-159">Configure hello operating system settings: authentication, VM name prefix, and user/pass.</span></span>
4. <span data-ttu-id="183fe-160">Konfigurace sítí.</span><span class="sxs-lookup"><span data-stu-id="183fe-160">Configure networking.</span></span>
5. <span data-ttu-id="183fe-161">Vytvořte sadu škálování hello.</span><span class="sxs-lookup"><span data-stu-id="183fe-161">Create hello scale set.</span></span>

<span data-ttu-id="183fe-162">Tento příklad vytvoří základní dvě instance škále nastavení v počítači, který má nainstalovaný Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="183fe-162">This example creates a basic two-instance scale set for a computer that has Windows Server 2016 installed.</span></span>

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create hello virtual network resources

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

# Attach hello virtual network toohello IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a><span data-ttu-id="183fe-163">Pomocí bitové kopie vlastního virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="183fe-163">Using a custom virtual machine image</span></span>
<span data-ttu-id="183fe-164">Pokud vytváříte škálování nastavit od svoji vlastní image, místo odkazující na bitovou kopii virtuálního počítače z Galerie hello hello _Set-AzureRmVmssStorageProfile_ příkaz bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="183fe-164">If you are creating a scale set from your own custom image, instead of referencing a virtual machine image from hello gallery, hello _Set-AzureRmVmssStorageProfile_ command would look like this:</span></span>
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a><span data-ttu-id="183fe-165">Vytvoření ze šablony</span><span class="sxs-lookup"><span data-stu-id="183fe-165">Create from a template</span></span>

<span data-ttu-id="183fe-166">Můžete nasadit škálování virtuálních počítačů, nastavit pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="183fe-166">You can deploy a virtual machine scale set by using an Azure Resource Manager template.</span></span> <span data-ttu-id="183fe-167">Můžete vytvořit vlastní šablonu nebo použijte jednu z hello [šablony úložiště](https://azure.microsoft.com/resources/templates/?term=vmss).</span><span class="sxs-lookup"><span data-stu-id="183fe-167">You can create your own template or use one from hello [template repository](https://azure.microsoft.com/resources/templates/?term=vmss).</span></span> <span data-ttu-id="183fe-168">Tyto šablony se dá nasadit přímo tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="183fe-168">These templates can be deployed directly tooyour Azure subscription.</span></span>

>[!NOTE]
><span data-ttu-id="183fe-169">toocreate vlastní šablonu, vytvořte textový soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="183fe-169">toocreate your own template, you create a JSON text file.</span></span> <span data-ttu-id="183fe-170">Obecné informace o toocreate a přizpůsobení šablony, najdete v části [šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-170">For general information about how toocreate and customize a template, see [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="183fe-171">Vzorové šablony je k dispozici [na Githubu](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span><span class="sxs-lookup"><span data-stu-id="183fe-171">A sample template is available [on GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span></span> <span data-ttu-id="183fe-172">Další informace o tom, jak toocreate a které ukázkové, najdete v části [minimální přijatelná škálovací sadu](.\virtual-machine-scale-sets-mvss-start.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-172">For more information about how toocreate and use that sample, see [Minimum viable scale set](.\virtual-machine-scale-sets-mvss-start.md).</span></span>

## <a name="create-from-visual-studio"></a><span data-ttu-id="183fe-173">Vytvořit ze sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="183fe-173">Create from Visual Studio</span></span>

<span data-ttu-id="183fe-174">Pomocí sady Visual Studio můžete vytvořit projekt skupiny prostředků Azure a přidat šablony tooit sady škálování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="183fe-174">With Visual Studio, you can create an Azure resource group project and add a virtual machine scale set template tooit.</span></span> <span data-ttu-id="183fe-175">Můžete zvolit, jestli chcete tooimport z Githubu nebo hello Galerie webových aplikací Azure.</span><span class="sxs-lookup"><span data-stu-id="183fe-175">You can choose whether you want tooimport it from GitHub or hello Azure Web Application Gallery.</span></span> <span data-ttu-id="183fe-176">Nasazení skript prostředí PowerShell je vytvořena také za vás.</span><span class="sxs-lookup"><span data-stu-id="183fe-176">A deployment PowerShell script is also generated for you.</span></span> <span data-ttu-id="183fe-177">Další informace najdete v tématu [jak toocreate škálovací sady virtuálních počítačů pomocí sady Visual Studio](virtual-machine-scale-sets-vs-create.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-177">For more information, see [How toocreate a virtual machine scale set with Visual Studio](virtual-machine-scale-sets-vs-create.md).</span></span>

## <a name="create-from-hello-azure-portal"></a><span data-ttu-id="183fe-178">Vytvoření z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="183fe-178">Create from hello Azure portal</span></span>

<span data-ttu-id="183fe-179">Hello portál Azure poskytuje pohodlný způsob tooquickly vytvoření sady škálování.</span><span class="sxs-lookup"><span data-stu-id="183fe-179">hello Azure portal provides a convenient way tooquickly create a scale set.</span></span> <span data-ttu-id="183fe-180">Další informace najdete v tématu [jak toocreate škálovací sady virtuálních počítačů s hello portál Azure](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-180">For more information, see [How toocreate a virtual machine scale set with hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="183fe-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="183fe-181">Next steps</span></span>

<span data-ttu-id="183fe-182">Další informace o [datových disků](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-182">Learn more about [data disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="183fe-183">Zjistěte, jak příliš[správě aplikací](virtual-machine-scale-sets-deploy-app.md).</span><span class="sxs-lookup"><span data-stu-id="183fe-183">Learn how too[manage your apps](virtual-machine-scale-sets-deploy-app.md).</span></span>
