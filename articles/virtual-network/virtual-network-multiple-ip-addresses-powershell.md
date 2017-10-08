---
title: "aaaMultiple IP adresy pro virtuální počítače Azure – prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooassign více IP adres tooa virtuálního počítače pomocí prostředí PowerShell | Správce prostředků."
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
ms.openlocfilehash: df54c4386ce13521e660a3e7208c8c1ab1459bc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-multiple-ip-addresses-toovirtual-machines-using-powershell"></a><span data-ttu-id="9662d-103">Přiřadit více IP adres toovirtual počítače pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="9662d-103">Assign multiple IP addresses toovirtual machines using PowerShell</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

<span data-ttu-id="9662d-104">Tento článek vysvětluje, jak toocreate virtuální počítač (VM) prostřednictvím nasazení Azure Resource Manager hello modelu pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9662d-104">This article explains how toocreate a virtual machine (VM) through hello Azure Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="9662d-105">Tooresources vytvořené pomocí modelu nasazení classic hello nelze přiřadit více IP adres.</span><span class="sxs-lookup"><span data-stu-id="9662d-105">Multiple IP addresses cannot be assigned tooresources created through hello classic deployment model.</span></span> <span data-ttu-id="9662d-106">Další informace o modelech nasazení Azure, přečtěte si hello toolearn [pochopit modely nasazení](../resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-106">toolearn more about Azure deployment models, read hello [Understand deployment models](../resource-manager-deployment-model.md) article.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <span data-ttu-id="9662d-107"><a name = "create"></a>Vytvoření virtuálního počítače s více IP adres</span><span class="sxs-lookup"><span data-stu-id="9662d-107"><a name = "create"></a>Create a VM with multiple IP addresses</span></span>

<span data-ttu-id="9662d-108">Hello kroky, které následují vysvětlují, jak toocreate příklad virtuálního počítače s více IP adres, jak je popsáno v hello scénáři.</span><span class="sxs-lookup"><span data-stu-id="9662d-108">hello steps that follow explain how toocreate an example VM with multiple IP addresses, as described in hello scenario.</span></span> <span data-ttu-id="9662d-109">Změňte hodnoty proměnných podle potřeby týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="9662d-109">Change variable values as required for your implementation.</span></span>

1. <span data-ttu-id="9662d-110">Otevřete příkazový řádek prostředí PowerShell a dokončení hello zbývající kroky v této části v rámci jedné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9662d-110">Open a PowerShell command prompt and complete hello remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="9662d-111">Pokud ještě nemáte prostředí PowerShell nainstalovaný a nakonfigurovaný, dokončení hello kroky v hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-111">If you don't already have PowerShell installed and configured, complete hello steps in hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="9662d-112">Účet pro přihlášení tooyour s hello `login-azurermaccount` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9662d-112">Login tooyour account with hello `login-azurermaccount` command.</span></span>
3. <span data-ttu-id="9662d-113">Nahraďte *myResourceGroup* a *westus* s názvem a umístění dle vlastního výběru.</span><span class="sxs-lookup"><span data-stu-id="9662d-113">Replace *myResourceGroup* and *westus* with a name and location of your choosing.</span></span> <span data-ttu-id="9662d-114">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="9662d-114">Create a resource group.</span></span> <span data-ttu-id="9662d-115">Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="9662d-115">A resource group is a logical container into which Azure resources are deployed and managed.</span></span>

    ```powershell
    $RgName   = "MyResourceGroup"
    $Location = "westus"

    New-AzureRmResourceGroup `
    -Name $RgName `
    -Location $Location
    ```

4. <span data-ttu-id="9662d-116">Vytvořte virtuální síť (VNet) a podsítě v hello stejné umístění jako skupina prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="9662d-116">Create a virtual network (VNet) and subnet in hello same location as hello resource group:</span></span>

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

    # Get hello subnet object
    $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
    ```

5. <span data-ttu-id="9662d-117">Vytvořte skupinu zabezpečení sítě (NSG) a pravidla.</span><span class="sxs-lookup"><span data-stu-id="9662d-117">Create a network security group (NSG) and a rule.</span></span> <span data-ttu-id="9662d-118">Hello NSG zabezpečuje hello virtuálních počítačů pomocí příchozí a odchozí pravidla.</span><span class="sxs-lookup"><span data-stu-id="9662d-118">hello NSG secures hello VM using inbound and outbound rules.</span></span> <span data-ttu-id="9662d-119">V tomto případě se vytvoří příchozí pravidlo pro port 3389, které umožní příchozí připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="9662d-119">In this case, an inbound rule is created for port 3389, which allows incoming remote desktop connections.</span></span>

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

6. <span data-ttu-id="9662d-120">Definování hello primární konfiguraci IP adresy pro hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="9662d-120">Define hello primary IP configuration for hello NIC.</span></span> <span data-ttu-id="9662d-121">Změna 10.0.0.4 tooa platná adresa v podsíti hello jste vytvořili, pokud jste nepoužili dříve definovanou hodnotu hello.</span><span class="sxs-lookup"><span data-stu-id="9662d-121">Change 10.0.0.4 tooa valid address in hello subnet you created, if you didn't use hello value defined previously.</span></span> <span data-ttu-id="9662d-122">Před přiřazením statické IP adresy, doporučujeme nejdřív ověřit, zda že se již používá.</span><span class="sxs-lookup"><span data-stu-id="9662d-122">Before assigning a static IP address, it's recommended that you first confirm it's not already in use.</span></span> <span data-ttu-id="9662d-123">Zadejte příkaz hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span><span class="sxs-lookup"><span data-stu-id="9662d-123">Enter hello command `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.4 -VirtualNetwork $VNet`.</span></span> <span data-ttu-id="9662d-124">Pokud není k dispozici hello adresa, výstup hello vrátí *True*.</span><span class="sxs-lookup"><span data-stu-id="9662d-124">If hello address is available, hello output returns *True*.</span></span> <span data-ttu-id="9662d-125">Pokud není k dispozici, výstup hello vrátí *False* a seznam adres, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9662d-125">If it's not available, hello output returns *False* and a list of addresses that are available.</span></span> 

    <span data-ttu-id="9662d-126">V následující příkazy, hello **< nahradit s vaše jedinečné name > nahraďte hello toouse název jedinečný DNS.**</span><span class="sxs-lookup"><span data-stu-id="9662d-126">In hello following commands, **Replace <replace-with-your-unique-name> with hello unique DNS name toouse.**</span></span> <span data-ttu-id="9662d-127">Název Hello musí být jedinečný mezi všechny veřejné IP adresy v rámci oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="9662d-127">hello name must be unique across all public IP addresses within an Azure region.</span></span> <span data-ttu-id="9662d-128">Toto je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="9662d-128">This is an optional parameter.</span></span> <span data-ttu-id="9662d-129">Může být odebrán, pokud chcete pouze tooconnect toohello virtuálních počítačů pomocí hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="9662d-129">It can be removed if you only want tooconnect toohello VM using hello public IP address.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP1 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP1" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -DomainNameLabel <replace-with-your-unique-name> `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
    $IpConfigName1 = "IPConfig-1"
    $IpConfig1     = New-AzureRmNetworkInterfaceIpConfig `
    -Name $IpConfigName1 `
    -Subnet $Subnet `
    -PrivateIpAddress 10.0.0.4 `
    -PublicIpAddress $PublicIP1 `
    -Primary
    ```

    <span data-ttu-id="9662d-130">Přiřadíte-li více tooa konfigurace IP síťovou kartu, musí být přiřazena jednu konfiguraci jako hello *-primární*.</span><span class="sxs-lookup"><span data-stu-id="9662d-130">When you assign multiple IP configurations tooa NIC, one configuration must be assigned as hello *-Primary*.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9662d-131">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="9662d-131">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="9662d-132">více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="9662d-132">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="9662d-133">Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném.</span><span class="sxs-lookup"><span data-stu-id="9662d-133">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="9662d-134">Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-134">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>

7. <span data-ttu-id="9662d-135">Definovat hello sekundární konfigurace IP pro hello síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="9662d-135">Define hello secondary IP configurations for hello NIC.</span></span> <span data-ttu-id="9662d-136">Můžete přidávat nebo odebírat konfigurace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9662d-136">You can add or remove configurations as necessary.</span></span> <span data-ttu-id="9662d-137">Každá konfigurace IP musí mít přiřazené privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9662d-137">Each IP configuration must have a private IP address assigned.</span></span> <span data-ttu-id="9662d-138">Každá konfigurace můžete volitelně může mít jednu veřejnou IP adresu přiřadit.</span><span class="sxs-lookup"><span data-stu-id="9662d-138">Each configuration can optionally have one public IP address assigned.</span></span>

    ```powershell
    
    # Create a public IP address
    $PublicIP2 = New-AzureRmPublicIpAddress `
    -Name "MyPublicIP2" `
    -ResourceGroupName $RgName `
    -Location $Location `
    -AllocationMethod Static
        
    #Create an IP configuration with a static private IP address and assign hello public IP ddress tooit
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

8. <span data-ttu-id="9662d-139">Vytvořte hello síťový adaptér a přidružte hello tři IP konfigurace tooit:</span><span class="sxs-lookup"><span data-stu-id="9662d-139">Create hello NIC and associate hello three IP configurations tooit:</span></span>

    ```powershell
    
    $NIC = New-AzureRmNetworkInterface `
    -Name MyNIC `
    -ResourceGroupName $RgName `
    -Location $Location `
    -NetworkSecurityGroupId $NSG.Id `
    -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3
    ```

    >[!NOTE]
    ><span data-ttu-id="9662d-140">I když všechny konfigurace přiřazené tooone síťový adaptér v tomto článku, můžete přiřadit více IP konfigurace tooevery síťovou kartu připojenou toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="9662d-140">Though all configurations are assigned tooone NIC in this article, you can assign multiple IP configurations tooevery NIC attached toohello VM.</span></span> <span data-ttu-id="9662d-141">jak toocreate virtuálního počítače s více síťovými kartami, číst toolearn hello [vytvoření virtuálního počítače s více síťovými kartami](virtual-network-deploy-multinic-arm-ps.md) článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-141">toolearn how toocreate a VM with multiple NICs, read hello [Create a VM with multiple NICs](virtual-network-deploy-multinic-arm-ps.md) article.</span></span>

9. <span data-ttu-id="9662d-142">Vytvořte hello virtuálních počítačů zadáním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="9662d-142">Create hello VM by entering hello following commands:</span></span>

    ```powershell
    
    # Define a credential object. When you run these commands, you're prompted tooenter a sername and password for hello VM you're reating.
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
    
    # Create hello VM
    New-AzureRmVM `
    -ResourceGroupName $RgName `
    -Location $Location `
    -VM $VmConfig
    ```

10. <span data-ttu-id="9662d-143">Přidat hello privátní IP adresy toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-143">Add hello private IP addresses toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="9662d-144">Nepřidávejte hello veřejné IP adresy toohello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="9662d-144">Do not add hello public IP addresses toohello operating system.</span></span>

## <span data-ttu-id="9662d-145"><a name="add"></a>Přidat tooa IP adresy virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="9662d-145"><a name="add"></a>Add IP addresses tooa VM</span></span>

<span data-ttu-id="9662d-146">Privátní a veřejné IP adresy tooa síťovou kartu můžete přidat pomocí hello kroků, které následují.</span><span class="sxs-lookup"><span data-stu-id="9662d-146">You can add private and public IP addresses tooa NIC by completing hello steps that follow.</span></span> <span data-ttu-id="9662d-147">Hello příklady v následující části hello předpokládají, že už máte virtuální počítač s konfigurací protokolu IP hello tři popsané v hello [scénář](#Scenario) v tomto článku, ale není to nutné, abyste provedli.</span><span class="sxs-lookup"><span data-stu-id="9662d-147">hello examples in hello following sections assume that you already have a VM with hello three IP configurations described in hello [scenario](#Scenario) in this article, but it's not required that you do.</span></span>

1. <span data-ttu-id="9662d-148">Otevřete příkazový řádek prostředí PowerShell a dokončení hello zbývající kroky v této části v rámci jedné relace prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9662d-148">Open a PowerShell command prompt and complete hello remaining steps in this section within a single PowerShell session.</span></span> <span data-ttu-id="9662d-149">Pokud ještě nemáte prostředí PowerShell nainstalovaný a nakonfigurovaný, dokončení hello kroky v hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-149">If you don't already have PowerShell installed and configured, complete hello steps in hello [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="9662d-150">Změna hello "hodnoty" hello následující název toohello $Variables hello chcete tooadd IP adresu tooand hello prostředků skupiny a umístění hello síťový adaptér existuje v síťovou kartu:</span><span class="sxs-lookup"><span data-stu-id="9662d-150">Change hello "values" of hello following $Variables toohello name of hello NIC you want tooadd IP address tooand hello resource group and location hello NIC exists in:</span></span>

    ```powershell
    $NicName  = "MyNIC"
    $RgName   = "MyResourceGroup"
    $Location = "westus"
    ```

    <span data-ttu-id="9662d-151">Pokud neznáte název hello hello chcete toochange síťovou kartu, zadejte následující příkazy hello, pak změňte hodnoty hello předchozích proměnných hello:</span><span class="sxs-lookup"><span data-stu-id="9662d-151">If you don't know hello name of hello NIC you want toochange, enter hello following commands, then change hello values of hello previous variables:</span></span>

    ```powershell
    Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location
    ```
3. <span data-ttu-id="9662d-152">Vytvoření proměnné a nastavte ji toohello stávající síťovou kartu zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9662d-152">Create a variable and set it toohello existing NIC by typing hello following command:</span></span>

    ```powershell
    $MyNIC = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName
    ```
4. <span data-ttu-id="9662d-153">Hello následující příkazy, změňte *MyVNet* a *MySubnet* toohello názvy virtuálních sítí a podsítí hello hello síťový adaptér je připojen k.</span><span class="sxs-lookup"><span data-stu-id="9662d-153">In hello following commands, change *MyVNet* and *MySubnet* toohello names of hello VNet and subnet hello NIC is connected to.</span></span> <span data-ttu-id="9662d-154">Zadejte hello příkazy tooretrieve hello virtuálních sítí a podsítí objekty hello síťový adaptér je připojen k:</span><span class="sxs-lookup"><span data-stu-id="9662d-154">Enter hello commands tooretrieve hello VNet and subnet objects hello NIC is connected to:</span></span>

    ```powershell
    $MyVNet = Get-AzureRMVirtualnetwork -Name MyVNet -ResourceGroupName $RgName
    $Subnet = $MyVnet.Subnets | Where-Object { $_.Name -eq "MySubnet" }
    ```
    <span data-ttu-id="9662d-155">Pokud si nejste jisti hello virtuální síť nebo podsíť název hello síťový adaptér je připojen k, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9662d-155">If you don't know hello VNet or subnet name hello NIC is connected to, enter hello following command:</span></span>
    ```powershell
    $MyNIC.IpConfigurations
    ```
    <span data-ttu-id="9662d-156">Ve výstupu hello hledejte text podobné toohello následující příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="9662d-156">In hello output, look for text similar toohello following example output:</span></span>
    
    ```
    "Id": "/subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/MyVNet/subnets/MySubnet"
    ```
    <span data-ttu-id="9662d-157">V tento výstup *MyVnet* je hello virtuální sítě a *MySubnet* je hello podsíť hello síťový adaptér je připojen k.</span><span class="sxs-lookup"><span data-stu-id="9662d-157">In this output, *MyVnet* is hello VNet and *MySubnet* is hello subnet hello NIC is connected to.</span></span>

5. <span data-ttu-id="9662d-158">Proveďte kroky hello v jednom z hello následující části, podle požadavků:</span><span class="sxs-lookup"><span data-stu-id="9662d-158">Complete hello steps in one of hello following sections, based on your requirements:</span></span>

    <span data-ttu-id="9662d-159">**Přidejte privátní IP adresy**</span><span class="sxs-lookup"><span data-stu-id="9662d-159">**Add a private IP address**</span></span>

    <span data-ttu-id="9662d-160">tooadd tooa privátní adresy IP síťový adaptér, je nutné vytvořit konfiguraci IP adres.</span><span class="sxs-lookup"><span data-stu-id="9662d-160">tooadd a private IP address tooa NIC, you must create an IP configuration.</span></span> <span data-ttu-id="9662d-161">Hello následující příkaz vytvoří konfiguraci se statickou IP adresu 10.0.0.7.</span><span class="sxs-lookup"><span data-stu-id="9662d-161">hello following command creates a configuration with a static IP address of 10.0.0.7.</span></span> <span data-ttu-id="9662d-162">Při zadávání statickou IP adresu, musí být adresu nepoužívané hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="9662d-162">When specifying a static IP address, it must be an unused address for hello subnet.</span></span> <span data-ttu-id="9662d-163">Doporučuje se nejdřív otestovat tooensure adresu hello je k dispozici zadáním hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9662d-163">It's recommended that you first test hello address tooensure it's available by entering hello `Test-AzureRmPrivateIPAddressAvailability -IPAddress 10.0.0.7 -VirtualNetwork $myVnet` command.</span></span> <span data-ttu-id="9662d-164">Pokud není k dispozici hello IP adresa, výstup hello vrátí *True*.</span><span class="sxs-lookup"><span data-stu-id="9662d-164">If hello IP address is available, hello output returns *True*.</span></span> <span data-ttu-id="9662d-165">Pokud není k dispozici, výstup hello vrátí *False*a seznam adres, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="9662d-165">If it's not available, hello output returns *False*, and a list of addresses that are available.</span></span>

    ```powershell
    Add-AzureRmNetworkInterfaceIpConfig -Name IPConfig-4 -NetworkInterface `
    $MyNIC -Subnet $Subnet -PrivateIpAddress 10.0.0.7
    ```
    <span data-ttu-id="9662d-166">Vytvořte tolik konfigurace, jak požadujete, pomocí konfigurace jedinečné názvy a privátní IP adresy (pro konfigurace s statické IP adresy).</span><span class="sxs-lookup"><span data-stu-id="9662d-166">Create as many configurations as you require, using unique configuration names and private IP addresses (for configurations with static IP addresses).</span></span>

    <span data-ttu-id="9662d-167">Přidat hello privátní IP adresu toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-167">Add hello private IP address toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span>

    <span data-ttu-id="9662d-168">**Přidejte veřejnou IP adresu**</span><span class="sxs-lookup"><span data-stu-id="9662d-168">**Add a public IP address**</span></span>

    <span data-ttu-id="9662d-169">Veřejná IP adresa se přidá spojením veřejnou IP adresu prostředku tooeither na novou konfiguraci protokolu IP nebo existující konfiguraci IP adres.</span><span class="sxs-lookup"><span data-stu-id="9662d-169">A public IP address is added by associating a public IP address resource tooeither a new IP configuration or an existing IP configuration.</span></span> <span data-ttu-id="9662d-170">Kroky hello v jedné z hello částí, které následují, potřebujete.</span><span class="sxs-lookup"><span data-stu-id="9662d-170">Complete hello steps in one of hello sections that follow, as you require.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9662d-171">Veřejné IP adresy mají nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="9662d-171">Public IP addresses have a nominal fee.</span></span> <span data-ttu-id="9662d-172">více informací o IP adresy, ceny, toolearn číst hello [ceny IP adresu](https://azure.microsoft.com/pricing/details/ip-addresses) stránky.</span><span class="sxs-lookup"><span data-stu-id="9662d-172">toolearn more about IP address pricing, read hello [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span> <span data-ttu-id="9662d-173">Existuje limit toohello, počet veřejné IP adresy, které lze použít v předplatném.</span><span class="sxs-lookup"><span data-stu-id="9662d-173">There is a limit toohello number of public IP addresses that can be used in a subscription.</span></span> <span data-ttu-id="9662d-174">Další informace o omezení hello, přečtěte si hello toolearn [Azure omezuje](../azure-subscription-service-limits.md#networking-limits) článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-174">toolearn more about hello limits, read hello [Azure limits](../azure-subscription-service-limits.md#networking-limits) article.</span></span>
    >

    - <span data-ttu-id="9662d-175">**Přidružení hello veřejné IP adresy prostředků tooa nová konfigurace IP**</span><span class="sxs-lookup"><span data-stu-id="9662d-175">**Associate hello public IP address resource tooa new IP configuration**</span></span>
    
        <span data-ttu-id="9662d-176">Vždy, když přidáte veřejnou IP adresu v novou konfiguraci protokolu IP, musíte taky přidat privátní IP adresy, protože všechny konfigurace protokolu IP, musí mít privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="9662d-176">Whenever you add a public IP address in a new IP configuration, you must also add a private IP address, because all IP configurations must have a private IP address.</span></span> <span data-ttu-id="9662d-177">Můžete přidat existující prostředek veřejné IP adresy, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="9662d-177">You can either add an existing public IP address resource, or create a new one.</span></span> <span data-ttu-id="9662d-178">toocreate novou, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9662d-178">toocreate a new one, enter hello following command:</span></span>
    
        ```powershell
        $myPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "myPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location `
        -AllocationMethod Static
        ```

        <span data-ttu-id="9662d-179">toocreate na novou konfiguraci protokolu IP se statickou privátní IP adresou a hello přidružené *myPublicIp3* veřejných IP adres prostředků, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="9662d-179">toocreate a new IP configuration with a static private IP address and hello associated *myPublicIp3* public IP address resource, enter hello following command:</span></span>

        ```powershell
        Add-AzureRmNetworkInterfaceIpConfig `
        -Name IPConfig-4 `
        -NetworkInterface $myNIC `
        -Subnet $Subnet `
        -PrivateIpAddress 10.0.0.7 `
        -PublicIpAddress $myPublicIp3
        ```

    - <span data-ttu-id="9662d-180">**Přidružení hello veřejné konfiguraci IP adresy prostředků tooan stávající IP**</span><span class="sxs-lookup"><span data-stu-id="9662d-180">**Associate hello public IP address resource tooan existing IP configuration**</span></span>

        <span data-ttu-id="9662d-181">Prostředek veřejné IP adresy lze pouze přidružené tooan konfigurace protokolu IP, který ho přidružené neobsahuje.</span><span class="sxs-lookup"><span data-stu-id="9662d-181">A public IP address resource can only be associated tooan IP configuration that doesn't already have one associated.</span></span> <span data-ttu-id="9662d-182">Můžete určit, zda konfiguraci IP adres má přidružené veřejnou IP adresu zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9662d-182">You can determine whether an IP configuration has an associated public IP address by entering hello following command:</span></span>

        ```powershell
        $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
        ```

        <span data-ttu-id="9662d-183">Zobrazí výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="9662d-183">You see output similar toohello following:</span></span>

        ```     
        Name       PrivateIpAddress PublicIpAddress                                           Primary
        
        IPConfig-1 10.0.0.4         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress    True
        IPConfig-2 10.0.0.5         Microsoft.Azure.Commands.Network.Models.PSPublicIpAddress   False
        IpConfig-3 10.0.0.6                                                                     False
        ```

        <span data-ttu-id="9662d-184">Od hello **PublicIpAddress** sloupec pro *IpConfig 3* je prázdné, žádný prostředek veřejné IP adresy je aktuálně přidružené tooit.</span><span class="sxs-lookup"><span data-stu-id="9662d-184">Since hello **PublicIpAddress** column for *IpConfig-3* is blank, no public IP address resource is currently associated tooit.</span></span> <span data-ttu-id="9662d-185">Můžete přidat existující veřejnou IP adresu prostředku tooIpConfig-3, nebo zadejte následující příkaz toocreate jeden hello:</span><span class="sxs-lookup"><span data-stu-id="9662d-185">You can add an existing public IP address resource tooIpConfig-3, or enter hello following command toocreate one:</span></span>

        ```powershell
        $MyPublicIp3 = New-AzureRmPublicIpAddress `
        -Name "MyPublicIp3" `
        -ResourceGroupName $RgName `
        -Location $Location -AllocationMethod Static
        ```

        <span data-ttu-id="9662d-186">Zadejte následující příkaz tooassociate hello veřejných IP adres toohello stávající IP konfigurace prostředků s názvem hello *IpConfig 3*:</span><span class="sxs-lookup"><span data-stu-id="9662d-186">Enter hello following command tooassociate hello public IP address resource toohello existing IP configuration named *IpConfig-3*:</span></span>
    
        ```powershell
        Set-AzureRmNetworkInterfaceIpConfig `
        -Name IpConfig-3 `
        -NetworkInterface $mynic `
        -Subnet $Subnet `
        -PublicIpAddress $myPublicIp3
        ```

6. <span data-ttu-id="9662d-187">Nastavte hello síťový adaptér s novou konfigurací IP adresy hello zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9662d-187">Set hello NIC with hello new IP configuration by entering hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $MyNIC
    ```

7. <span data-ttu-id="9662d-188">Zobrazení hello privátních IP adres a hello veřejnou IP adresu prostředky přiřazené toohello hello síťový adaptér tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="9662d-188">View hello private IP addresses and hello public IP address resources assigned toohello NIC by entering hello following command:</span></span>

    ```powershell   
    $MyNIC.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary
    ```
8. <span data-ttu-id="9662d-189">Přidat hello privátní IP adresu toohello virtuálních počítačů operačního systému pomocí kroků hello operačního systému v hello [přidání IP adres pro operační systém virtuálního počítače tooa](#os-config) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9662d-189">Add hello private IP address toohello VM operating system by completing hello steps for your operating system in hello [Add IP addresses tooa VM operating system](#os-config) section of this article.</span></span> <span data-ttu-id="9662d-190">Nepřidávejte hello veřejnou IP adresu toohello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="9662d-190">Do not add hello public IP address toohello operating system.</span></span>

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
