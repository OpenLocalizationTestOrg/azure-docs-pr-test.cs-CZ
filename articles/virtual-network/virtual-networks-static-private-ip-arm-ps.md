---
title: "aaaConfigure privátních IP adres pro virtuální počítače - prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooconfigure privátní IP adresy pro virtuální počítače pomocí prostředí PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4a3eb67de583e08208fcab40de1c2a8a9b65618c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a><span data-ttu-id="93a0c-103">Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="93a0c-103">Configure private IP addresses for a virtual machine using PowerShell</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

<span data-ttu-id="93a0c-104">Azure nabízí dva modely nasazení: Azure Resource Manager a Classic.</span><span class="sxs-lookup"><span data-stu-id="93a0c-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="93a0c-105">Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="93a0c-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="93a0c-106">Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku.</span><span class="sxs-lookup"><span data-stu-id="93a0c-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="93a0c-107">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="93a0c-107">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="93a0c-108">Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="93a0c-108">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="93a0c-109">Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše.</span><span class="sxs-lookup"><span data-stu-id="93a0c-109">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="93a0c-110">Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="93a0c-110">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-ps.md).</span></span>

## <a name="create-a-vm-with-a-static-private-ip-address"></a><span data-ttu-id="93a0c-111">Vytvoření virtuálního počítače se statickou privátní IP adresou</span><span class="sxs-lookup"><span data-stu-id="93a0c-111">Create a VM with a static private IP address</span></span>
<span data-ttu-id="93a0c-112">virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="93a0c-112">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="93a0c-113">Nastavení proměnných pro účet úložiště hello, umístění, skupiny prostředků a toobe pověření použít.</span><span class="sxs-lookup"><span data-stu-id="93a0c-113">Set variables for hello storage account, location, resource group, and credentials toobe used.</span></span> <span data-ttu-id="93a0c-114">Budete potřebovat tooenter uživatelské jméno a heslo pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="93a0c-114">You will need tooenter a user name and password for hello VM.</span></span> <span data-ttu-id="93a0c-115">skupiny účtů a prostředků úložiště Hello již musí existovat.</span><span class="sxs-lookup"><span data-stu-id="93a0c-115">hello storage account and resource group must already exist.</span></span>

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type hello name and password of hello local administrator account."
    ```

2. <span data-ttu-id="93a0c-116">Načtení hello virtuální síť a podsíť má toocreate hello virtuálního počítače v.</span><span class="sxs-lookup"><span data-stu-id="93a0c-116">Retrieve hello virtual network and subnet you want toocreate hello VM in.</span></span>

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. <span data-ttu-id="93a0c-117">V případě potřeby vytvořte veřejné hello tooaccess IP adresy virtuálních počítačů z hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="93a0c-117">If necessary, create a public IP address tooaccess hello VM from hello Internet.</span></span>

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. <span data-ttu-id="93a0c-118">Vytvořte pomocí hello statickou privátní IP adresou chcete tooassign toohello virtuálního počítače síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="93a0c-118">Create a NIC using hello static private IP address you want tooassign toohello VM.</span></span> <span data-ttu-id="93a0c-119">Přesvědčte se, zda text hello IP je z rozsahu hello podsítě, který chcete přidat hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="93a0c-119">Make sure hello IP is from hello subnet range you are adding hello VM to.</span></span> <span data-ttu-id="93a0c-120">Toto je hlavní krok hello k tomuto článku, kde můžete nastavit privátní IP toobe hello statické.</span><span class="sxs-lookup"><span data-stu-id="93a0c-120">This is hello main step for this article, where you set hello private IP toobe static.</span></span>

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. <span data-ttu-id="93a0c-121">Vytvoření virtuálního počítače pomocí hello seskupování vytvořili výše hello.</span><span class="sxs-lookup"><span data-stu-id="93a0c-121">Create hello VM using hello NIC created above.</span></span>

    ```powershell
    $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01 `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri `
    -CreateOption fromImage
    New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 
    ```

    <span data-ttu-id="93a0c-122">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="93a0c-122">Expected output:</span></span>
    
        EndTime             : [Date and time]
        Error               : 
        Output              : 
        StartTime           : [Date and time]
        Status              : Succeeded
        TrackingOperationId : [Id]
        RequestId           : [Id]
        StatusCode          : OK 

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a><span data-ttu-id="93a0c-123">Načíst statickou privátní IP adresu informace pro síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="93a0c-123">Retrieve static private IP address information for a network interface</span></span>
<span data-ttu-id="93a0c-124">tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *PrivateIpAddress* a  *PrivateIpAllocationMethod*:</span><span class="sxs-lookup"><span data-stu-id="93a0c-124">tooview hello static private IP address information for hello VM created with hello script above, run hello following PowerShell command and observe hello values for *PrivateIpAddress* and *PrivateIpAllocationMethod*:</span></span>

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

<span data-ttu-id="93a0c-125">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="93a0c-125">Expected output:</span></span>

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a><span data-ttu-id="93a0c-126">Odeberte statickou privátní IP adresu z rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="93a0c-126">Remove a static private IP address from a network interface</span></span>
<span data-ttu-id="93a0c-127">tooremove hello statickou privátní IP adresou přidána toohello virtuálních počítačů ve skriptu hello nad spuštění hello následující příkazy prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="93a0c-127">tooremove hello static private IP address added toohello VM in hello script above, run hello following PowerShell commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

<span data-ttu-id="93a0c-128">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="93a0c-128">Expected output:</span></span>

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="add-a-static-private-ip-address-tooa-network-interface"></a><span data-ttu-id="93a0c-129">Přidat statickou privátní IP adresu tooa rozhraní sítě</span><span class="sxs-lookup"><span data-stu-id="93a0c-129">Add a static private IP address tooa network interface</span></span>
<span data-ttu-id="93a0c-130">tooadd statickou privátní IP adresu toohello virtuální počítač vytvořený pomocí skriptu hello výše, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="93a0c-130">tooadd a static private IP address toohello VM created using hello script above, run hello following commands:</span></span>

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```
## <a name="change-hello-allocation-method-for-a-private-ip-address-assigned-tooa-network-interface"></a><span data-ttu-id="93a0c-131">Změnit metodu hello přidělení privátní IP adresy přiřazené tooa síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="93a0c-131">Change hello allocation method for a private IP address assigned tooa network interface</span></span>

<span data-ttu-id="93a0c-132">Tooa síťový adaptér s metodou statické nebo dynamické přidělování hello je přiřazená privátní IP adresa.</span><span class="sxs-lookup"><span data-stu-id="93a0c-132">A private IP address is assigned tooa NIC with hello static or dynamic allocation method.</span></span> <span data-ttu-id="93a0c-133">Dynamické IP adresy můžete změnit po spuštění virtuálního počítače, která byla dříve v hello zastavení (deallocated) stavu.</span><span class="sxs-lookup"><span data-stu-id="93a0c-133">Dynamic IP addresses can change after starting a VM that was previously in hello stopped (deallocated) state.</span></span> <span data-ttu-id="93a0c-134">To může potenciálně způsobit problémy, pokud hello virtuální počítač je hostitelem služby, která vyžaduje hello stejnou IP adresu, a to i po restartování z zastaveném stavu (deallocated).</span><span class="sxs-lookup"><span data-stu-id="93a0c-134">This can potentially cause issues if hello VM is hosting a service that requires hello same IP address, even after restarts from a stopped (deallocated) state.</span></span> <span data-ttu-id="93a0c-135">Statické IP adresy se zachovají, dokud je neodstraní hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="93a0c-135">Static IP addresses are retained until hello VM is deleted.</span></span> <span data-ttu-id="93a0c-136">Metoda toochange hello přidělení IP adresy, spusťte následující skript, který změní metoda přidělení hello z dynamické toostatic hello.</span><span class="sxs-lookup"><span data-stu-id="93a0c-136">toochange hello allocation method of an IP address, run hello following script, which changes hello allocation method from dynamic toostatic.</span></span> <span data-ttu-id="93a0c-137">Pokud je metoda přidělení hello hello aktuální privátní IP adresy statické, změňte *statické* příliš*dynamické* před spuštěním skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="93a0c-137">If hello allocation method for hello current private IP address is static, change *Static* too*Dynamic* before executing hello script.</span></span>

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "hello allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for hello IP address" $IP"." -NoNewline
```

<span data-ttu-id="93a0c-138">Pokud neznáte název hello hello síťový adaptér, můžete zobrazit seznam síťových adaptérů ve skupině prostředků tak, že zadáte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="93a0c-138">If you don't know hello name of hello NIC, you can view a list of NICs within a resource group by entering hello following command:</span></span>

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a><span data-ttu-id="93a0c-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93a0c-139">Next steps</span></span>
* <span data-ttu-id="93a0c-140">Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="93a0c-140">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="93a0c-141">Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.</span><span class="sxs-lookup"><span data-stu-id="93a0c-141">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="93a0c-142">Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="93a0c-142">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>

