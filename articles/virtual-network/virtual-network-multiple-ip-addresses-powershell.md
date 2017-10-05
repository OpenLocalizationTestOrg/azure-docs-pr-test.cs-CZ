---
title: "Více IP adres pro virtuální počítače Azure – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak přiřadit více IP adres k virtuálnímu počítači pomocí prostředí PowerShell | Správce prostředků."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c44ea62f-7e54-4e3b-81ef-0b132111f1f8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/24/2017
ms.author: jdial;annahar
ms.openlocfilehash: 29f64aeefc2a7deb1f84d759c2323347536b9c27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-powershell"></a><span data-ttu-id="9ee90-103">Přiřadit více IP adres virtuálních počítačů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ee90-103">Assign multiple IP addresses to virtual machines using PowerShell</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="9ee90-104">Tento článek vysvětluje, jak vytvořit virtuální počítač (VM) pomocí modelu nasazení Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ee90-104">This article explains how to create a virtual machine (VM) through the Azure Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="9ee90-105">Nelze přiřadit více IP adres k prostředkům, které jsou vytvořené pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="9ee90-105">Multiple IP addresses cannot be assigned to resources created through the classic deployment model.</span></span> <span data-ttu-id="9ee90-106">Další informace o modelech nasazení Azure, najdete [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-106">To learn more about Azure deployment models, read the [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="9ee90-107"><a name = "create"></a>Vytvoření virtuálního počítače s více IP adres</span><span class="sxs-lookup"><span data-stu-id="9ee90-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="9ee90-108">Kroky, které následují vysvětlují, jak vytvořit příklad virtuálních počítačů s více IP adres, jak je popsáno v tomto scénáři.</span><span class="sxs-lookup"><span data-stu-id="9ee90-108">The steps that follow explain how to create an example VM with multiple IP addresses, as described in the scenario.</span></span> <span data-ttu-id="9ee90-109">Změňte hodnoty proměnných podle potřeby týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="9ee90-109">Change variable values as required for your implementation.</span></span>

1. <span data-ttu-id="9ee90-110">Otevřete příkazový řádek prostředí PowerShell a dokončit zbývající kroky v této části v rámci jedné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ee90-110">Open a PowerShell command prompt and complete the remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="9ee90-111">Pokud ještě nemáte prostředí PowerShell nainstalovaný a nakonfigurovaný, proveďte kroky v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-111">If you don't already have PowerShell installed and configured, complete the steps in the [How to install and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="9ee90-112">Přihlášení k účtu s `login-azurermaccount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9ee90-112">Login to your account with the `login-azurermaccount` command.</span></span>
3. <span data-ttu-id="9ee90-113">Nahraďte *myResourceGroup* a *westus* s názvem a umístění dle vlastního výběru.</span><span class="sxs-lookup"><span data-stu-id="9ee90-113">Replace *myResourceGroup* and *westus* with a name and location of your choosing.</span></span> <span data-ttu-id="9ee90-114">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9ee90-114">Create a resource group.</span></span> <span data-ttu-id="9ee90-115">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="9ee90-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span>

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. <span data-ttu-id="9ee90-116">Vytvořte virtuální síť (VNet) a podsíť ve stejném umístění jako pro skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="9ee90-116">Create a virtual network (VNet) and subnet in the same location as the resource group:</span></span>

    ```powershell
    
    # Create a subnet configuration
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name MySubnet `
    -AddressPrefix 10.0.0.0/24

    # Create a virtual network
    $VNet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyVNet `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $subnetConfig

    # Get the subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. <span data-ttu-id="9ee90-117">Vytvořte skupinu zabezpečení sítě (NSG) a pravidla.</span><span class="sxs-lookup"><span data-stu-id="9ee90-117">Create a network security group (NSG) and a rule.</span></span> <span data-ttu-id="9ee90-118">NSG zabezpečuje virtuálního počítače pomocí příchozí a odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="9ee90-118">The NSG secures the VM using inbound and outbound rules.</span></span> <span data-ttu-id="9ee90-119">V tomto případě se vytvoří příchozí pravidlo pro port 3389, které umožní příchozí připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="9ee90-119">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span>

    ```powershell
    
    # Create an inbound network security group rule for port 3389

    $NSGRule = New-AzureRmNetworkSecurityRuleConfig `
    -Name MyNsgRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow
    
    # Create a network security group
    $NSG = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $RgName `
    -Location $Location `
    -Name MyNetworkSecurityGroup `
    -SecurityRules $NSGRule
    ```

6. <span data-ttu-id="9ee90-120">Zadejte primární konfiguraci IP adresy pro síťovou kartu.</span><span class="sxs-lookup"><span data-stu-id="9ee90-120">Define the primary IP configuration for the NIC.</span></span> <span data-ttu-id="9ee90-121">Pokud jste nepoužili hodnota definovaná dříve, změňte na platnou adresou v podsíti, kterou jste vytvořili, 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="9ee90-121">Change 10.0.0.4 to a valid address in the subnet you created, if you didn't use the value defined previously.</span></span> <span data-ttu-id="9ee90-122">Před přiřazením statické IP adresy, doporučujeme nejdřív ověřit, zda že se již používá.</span><span class="sxs-lookup"><span data-stu-id="9ee90-122">Before assigning a static IP address, it's recommended that you first confirm it's not already in use.</span></span> <span data-ttu-id="9ee90-123">Zadejte příkaz `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span><span class="sxs-lookup"><span data-stu-id="9ee90-123">Enter the command `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span></span> <span data-ttu-id="9ee90-124">Pokud je k dispozici na adresu, vrátí výstup *True*.</span><span class="sxs-lookup"><span data-stu-id="9ee90-124">If the address is available, the output returns *True*.</span></span> <span data-ttu-id="9ee90-125">Pokud není k dispozici, vrátí výstup *False* a seznam adres, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9ee90-125">If it's not available, the output returns *False* and a list of addresses that are available.</span></span> 

    <span data-ttu-id="9ee90-126">V následujících příkazech **< nahradit s vaše jedinečné name > nahraďte jedinečný název DNS k použití.**</span><span class="sxs-lookup"><span data-stu-id="9ee90-126">In the following commands, **Replace <replace-with-your-unique-name> with the unique DNS name to use.**</span></span> <span data-ttu-id="9ee90-127">Název musí být jedinečný mezi všechny veřejné IP adresy v rámci oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="9ee90-127">The name must be unique across all public IP addresses within an Azure region.</span></span> <span data-ttu-id="9ee90-128">Toto je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="9ee90-128">This is an optional parameter.</span></span> <span data-ttu-id="9ee90-129">Může být odebrán, pokud se chcete připojit k virtuálnímu počítači pomocí veřejné adresy IP.</span><span class="sxs-lookup"><span data-stu-id="9ee90-129">It can be removed if you only want to connect to the VM using the public IP address.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP ddress to it
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    <span data-ttu-id="9ee90-130">Když přiřadíte víc konfigurací IP adres síťovou kartu, musí být přiřazena jednu konfiguraci jako *-primární*.</span><span class="sxs-lookup"><span data-stu-id="9ee90-130">When you assign multiple IP configurations to a NIC, one configuration must be assigned as the *-Primary*.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9ee90-131">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="9ee90-131">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="9ee90-132">Další informace o cenách IP adresu, najdete [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="9ee90-132">To learn more about IP address pricing, read the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="9ee90-133">Maximální počet veřejné IP adresy, které lze použít v předplatném je.</span><span class="sxs-lookup"><span data-stu-id="9ee90-133">There is a limit to the number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="9ee90-134">Další informace o omezeních najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="9ee90-134">To learn more about the limits, read the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

7. <span data-ttu-id="9ee90-135">Definují sekundární konfigurace IP síťovém adaptéru.</span><span class="sxs-lookup"><span data-stu-id="9ee90-135">Define the secondary IP configurations for the NIC.</span></span> <span data-ttu-id="9ee90-136">Můžete přidávat nebo odebírat konfigurace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ee90-136">You can add or remove configurations as necessary.</span></span> <span data-ttu-id="9ee90-137">Každá konfigurace IP musí mít přiřazené privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9ee90-137">Each IP configuration must have a private IP address assigned.</span></span> <span data-ttu-id="9ee90-138">Každá konfigurace můžete volitelně může mít jednu veřejnou IP adresu přiřadit.</span><span class="sxs-lookup"><span data-stu-id="9ee90-138">Each configuration can optionally have one public IP address assigned.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign the public IP ddress to it
    $IpConfigName2 = "IPConfig-2"
    $IpConfig2     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName2 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.5 `
    -PublicIpAddress $PublicIP2
        
    $IpConfigName3 = "IpConfig-3"
    $IpConfig3 = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IPConfigName3 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.6
    ```

8. <span data-ttu-id="9ee90-139">Vytvořte na síťový adaptér a přidružte tři konfigurace protokolu IP k němu:</span><span class="sxs-lookup"><span data-stu-id="9ee90-139">Create the NIC and associate the three IP configurations to it:</span></span>

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    ><span data-ttu-id="9ee90-140">Když na jeden síťový adaptér v tomto článku jsou přiřazeny všechny konfigurace, můžete přiřadit víc konfigurací IP adres pro každou síťovou kartu připojenou k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="9ee90-140">Though all configurations are assigned to one NIC in this article, you can assign multiple IP configurations to every NIC attached to the VM.</span></span> <span data-ttu-id="9ee90-141">Naučte se vytvořit virtuální počítač s více síťovými kartami, přečtěte si téma [vytvoření virtuálního počítače s více síťovými kartami](virtual-network-deploy-multinic-arm-ps.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-141">To learn how to create a VM with multiple NICs, read the [Create a VM with multiple NICs](virtual-network-deploy-multinic-arm-ps.md) article.</span></span>

9. <span data-ttu-id="9ee90-142">Vytvoření virtuálního počítače tak, že zadáte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="9ee90-142">Create the VM by entering the following commands:</span></span>

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted to enter a sername and password for the VM you're reating.
    $cred = Get-Credential
    
    # Create a virtual machine configuration
    $VmConfig = New-AzureRmVMConfig `
    -VMName MyVM `
    -VMSize Standard_DS1_v2 | `
    Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName MyVM `
    -Credential $cred | `
    Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
    Add-AzureRmVMNetworkInterface `
    -Id $NIC.Id
    
    # Create the VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. <span data-ttu-id="9ee90-143">Přidání privátních IP adres do operačního systému virtuálního počítače pomocí kroků pro operační systém v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-143">Add the private IP addresses to the VM operating system by completing the steps for your operating system in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="9ee90-144">Nepřidávejte veřejné IP adresy v operačním systému.</span><span class="sxs-lookup"><span data-stu-id="9ee90-144">Do not add the public IP addresses to the operating system.</span></span>

## <span data-ttu-id="9ee90-145"><a name="add"></a>Přidání IP adres pro virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="9ee90-145"><a name="add"></a>Add IP addresses to a VM</span></span>

<span data-ttu-id="9ee90-146">Provedením následujících kroků můžete přidat privátní a veřejné IP adresy pro síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="9ee90-146">You can add private and public IP addresses to a NIC by completing the steps that follow.</span></span> <span data-ttu-id="9ee90-147">Příklady v následujících částech předpokládají, že už máte virtuální počítač s tři konfigurace protokolu IP, které jsou popsané v [scénář](#Scenario) v tomto článku, ale není to nutné, abyste provedli.</span><span class="sxs-lookup"><span data-stu-id="9ee90-147">The examples in the following sections assume that you already have a VM with the three IP configurations described in the [scenario](#Scenario) in this article, but it's not required that you do.</span></span>

1. <span data-ttu-id="9ee90-148">Otevřete příkazový řádek prostředí PowerShell a dokončit zbývající kroky v této části v rámci jedné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ee90-148">Open a PowerShell command prompt and complete the remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="9ee90-149">Pokud ještě nemáte prostředí PowerShell nainstalovaný a nakonfigurovaný, proveďte kroky v [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-149">If you don't already have PowerShell installed and configured, complete the steps in the [How to install and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="9ee90-150">Změňte název síťového adaptéru, který chcete přidat IP adresu a skupinu prostředků a umístění, které síťový adaptér existuje v "hodnoty" $Variables následující:</span><span class="sxs-lookup"><span data-stu-id="9ee90-150">Change the "values" of the following $Variables to the name of the NIC you want to add IP address to and the resource group and location the NIC exists in:</span></span>

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    <span data-ttu-id="9ee90-151">Pokud neznáte název síťového adaptéru, který chcete změnit, zadejte následující příkazy, pak změňte hodnoty proměnných předchozí:</span><span class="sxs-lookup"><span data-stu-id="9ee90-151">If you don't know the name of the NIC you want to change, enter the following commands, then change the values of the previous variables:</span></span>

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. <span data-ttu-id="9ee90-152">Vytvoření proměnné a nastavte ji na stávající síťové karty tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-152">Create a variable and set it to the existing NIC by typing the following command:</span></span>

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. <span data-ttu-id="9ee90-153">V následujících příkazech změnit *MyVNet* a *MySubnet* na názvy virtuálních sítí a podsítí, které jsou na síťový adaptér je připojen k.</span><span class="sxs-lookup"><span data-stu-id="9ee90-153">In the following commands, change *MyVNet* and *MySubnet* to the names of the VNet and subnet the NIC is connected to.</span></span> <span data-ttu-id="9ee90-154">Zadejte příkazy pro načtení objektů virtuálních sítí a podsítí, které síťový adaptér je připojen k:</span><span class="sxs-lookup"><span data-stu-id="9ee90-154">Enter the commands to retrieve the VNet and subnet objects the NIC is connected to:</span></span>

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    <span data-ttu-id="9ee90-155">Pokud neznáte název virtuální síť nebo podsíť, které síťový adaptér je připojen k, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-155">If you don't know the VNet or subnet name the NIC is connected to, enter the following command:</span></span>
    ```powershell
    $MyNIC.IpConfigurations
    ```
    <span data-ttu-id="9ee90-156">Ve výstupu vyhledejte text podobné výstupu v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9ee90-156">In the output, look for text similar to the following example output:</span></span>
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    <span data-ttu-id="9ee90-157">V tento výstup *MyVnet* je síť VNet a *MySubnet* je síťový adaptér je připojený k podsíti.</span><span class="sxs-lookup"><span data-stu-id="9ee90-157">In this output, *MyVnet* is the VNet and *MySubnet* is the subnet the NIC is connected to.</span></span>

5. <span data-ttu-id="9ee90-158">Proveďte kroky v jednom z těchto částí, podle potřeb:</span><span class="sxs-lookup"><span data-stu-id="9ee90-158">Complete the steps in one of the following sections, based on your requirements:</span></span>

    <span data-ttu-id="9ee90-159">**Přidejte privátní IP adresy**</span><span class="sxs-lookup"><span data-stu-id="9ee90-159">**Add a private IP address**</span></span>

    <span data-ttu-id="9ee90-160">Chcete-li přidat privátní IP adresy pro síťový adaptér, je nutné vytvořit konfiguraci IP adres.</span><span class="sxs-lookup"><span data-stu-id="9ee90-160">To add a private IP address to a NIC, you must create an IP configuration.</span></span> <span data-ttu-id="9ee90-161">Následující příkaz vytvoří konfiguraci se statickou IP adresou 10.0.0.7.</span><span class="sxs-lookup"><span data-stu-id="9ee90-161">The following command creates a configuration with a static IP address of 10.0.0.7.</span></span> <span data-ttu-id="9ee90-162">Při zadávání statickou IP adresu, musí být nepoužívané adresu podsítě.</span><span class="sxs-lookup"><span data-stu-id="9ee90-162">When specifying a static IP address, it must be an unused address for the subnet.</span></span> <span data-ttu-id="9ee90-163">Doporučuje se nejdřív otestovat adresu, zda je k dispozici zadáním `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9ee90-163">It's recommended that you first test the address to ensure it's available by entering the `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` command.</span></span> <span data-ttu-id="9ee90-164">Pokud IP adresa je k dispozici, vrátí výstup *True*.</span><span class="sxs-lookup"><span data-stu-id="9ee90-164">If the IP address is available, the output returns *True*.</span></span> <span data-ttu-id="9ee90-165">Pokud není k dispozici, vrátí výstup *False*a seznam adres, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9ee90-165">If it's not available, the output returns *False*, and a list of addresses that are available.</span></span>

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    <span data-ttu-id="9ee90-166">Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).</span><span class="sxs-lookup"><span data-stu-id="9ee90-166">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="9ee90-167">Přidání privátní IP adresu do operačního systému virtuálního počítače pomocí kroků pro operační systém v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-167">Add the private IP address to the VM operating system by completing the steps for your operating system in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span>

    <span data-ttu-id="9ee90-168">**Přidejte veřejnou IP adresu**</span><span class="sxs-lookup"><span data-stu-id="9ee90-168">**Add a public IP address**</span></span>

    <span data-ttu-id="9ee90-169">Veřejná IP adresa se přidá tím, že přidružíte prostředek veřejné IP adresy na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres.</span><span class="sxs-lookup"><span data-stu-id="9ee90-169">A public IP address is added by associating a public IP address resource to either a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="9ee90-170">Proveďte kroky v jednom z následujících, potřebujete.</span><span class="sxs-lookup"><span data-stu-id="9ee90-170">Complete the steps in one of the sections that follow, as you require.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9ee90-171">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="9ee90-171">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="9ee90-172">Další informace o cenách IP adresu, najdete [IP adresu ceny](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="9ee90-172">To learn more about IP address pricing, read the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="9ee90-173">Maximální počet veřejné IP adresy, které lze použít v předplatném je.</span><span class="sxs-lookup"><span data-stu-id="9ee90-173">There is a limit to the number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="9ee90-174">Další informace o omezeních najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md#networking-limits).</span><span class="sxs-lookup"><span data-stu-id="9ee90-174">To learn more about the limits, read the [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
    >

    - <span data-ttu-id="9ee90-175">**Přidružte prostředek veřejné IP adresy na novou konfiguraci protokolu IP**</span><span class="sxs-lookup"><span data-stu-id="9ee90-175">**Associate the public IP address resource to a new IP configuration**</span></span>
    
        <span data-ttu-id="9ee90-176">Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9ee90-176">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="9ee90-177">Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="9ee90-177">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="9ee90-178">Pokud chcete vytvořit nový, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-178">To create a new one, enter the following command:</span></span>
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        <span data-ttu-id="9ee90-179">Chcete-li vytvořit novou konfiguraci protokolu IP se statickou privátní IP adresou a přidruženého *myPublicIp3* veřejných IP adres prostředků, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-179">To create a new IP configuration with a static private IP address and the associated *myPublicIp3* public IP address resource, enter the following command:</span></span>

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - <span data-ttu-id="9ee90-180">**Přidružte prostředek veřejné IP adresy na existující konfiguraci IP adres**</span><span class="sxs-lookup"><span data-stu-id="9ee90-180">**Associate the public IP address resource to an existing IP configuration**</span></span>

        <span data-ttu-id="9ee90-181">Prostředek veřejné IP adresy lze přidružit pouze pro konfiguraci IP adres, který ho přidružené neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="9ee90-181">A public IP address resource can only be associated to an IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="9ee90-182">Můžete určit, zda má konfiguraci IP adres přidružené veřejnou IP adresu tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-182">You can determine whether an IP configuration has an associated public IP address by entering the following command:</span></span>

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        <span data-ttu-id="9ee90-183">Zobrazí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="9ee90-183">You see output similar to the following:</span></span>

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        <span data-ttu-id="9ee90-184">Vzhledem k tomu **PublicIpAddress** sloupec pro *IpConfig 3* je prázdné, žádný prostředek veřejné IP adresy je aktuálně k ní přidružena.</span><span class="sxs-lookup"><span data-stu-id="9ee90-184">Since the **PublicIpAddress** column for *IpConfig-3* is blank, no public IP address resource is currently associated to it.</span></span> <span data-ttu-id="9ee90-185">Můžete přidat existující prostředek veřejné IP adresy na IpConfig 3, nebo zadejte následující příkaz k jeho vytvoření:</span><span class="sxs-lookup"><span data-stu-id="9ee90-185">You can add an existing public IP address resource to IpConfig-3, or enter the following command to create one:</span></span>

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        <span data-ttu-id="9ee90-186">Zadejte následující příkaz pro přidružení prostředek veřejné IP adresy ke stávající konfiguraci IP adresy s názvem *IpConfig 3*:</span><span class="sxs-lookup"><span data-stu-id="9ee90-186">Enter the following command to associate the public IP address resource to the existing IP configuration named *IpConfig-3*:</span></span>
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. <span data-ttu-id="9ee90-187">Síťový adaptér s novou konfigurací IP adresy nastavte tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-187">Set the NIC with the new IP configuration by entering the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. <span data-ttu-id="9ee90-188">Zobrazení privátních IP adres a prostředky veřejné adresy IP adresy přiřazené na síťový adaptér tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9ee90-188">View the private IP addresses and the public IP address resources assigned to the NIC by entering the following command:</span></span>

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. <span data-ttu-id="9ee90-189">Přidání privátní IP adresu do operačního systému virtuálního počítače pomocí kroků pro operační systém v [přidat IP adresy na operační systém virtuálního počítače](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9ee90-189">Add the private IP address to the VM operating system by completing the steps for your operating system in the [Add IP addresses to a VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="9ee90-190">Nepřidávejte veřejnou IP adresu do operačního systému.</span><span class="sxs-lookup"><span data-stu-id="9ee90-190">Do not add the public IP address to the operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
