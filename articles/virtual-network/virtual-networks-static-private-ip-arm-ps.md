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
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a>Nakonfigurovat privátní IP adresy pro virtuální počítač pomocí prostředí PowerShell

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

Azure nabízí dva modely nasazení: Azure Resource Manager a Classic. Společnost Microsoft doporučuje vytváření prostředků prostřednictvím modelu nasazení Resource Manager hello. Další informace o toolearn hello rozdíly mezi hello dva modely, přečtěte si hello [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md) článku. Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [spravovat statickou privátní IP adresou v modelu nasazení classic hello](virtual-networks-static-private-ip-classic-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Ukázka Hello jednoduché prostředí už vytvořený očekávat níže uvedené příkazy prostředí PowerShell založené na scénář hello výše. Pokud chcete příkazy hello toorun, jak jsou zobrazeny v tomto dokumentu, nejprve sestavení hello testovací prostředí popsané v [vytvoření virtuální sítě](virtual-networks-create-vnet-arm-ps.md).

## <a name="create-a-vm-with-a-static-private-ip-address"></a>Vytvoření virtuálního počítače se statickou privátní IP adresou
virtuální počítač s názvem toocreate *DNS01* v hello *front-endu* podsíť virtuální sítě s názvem *TestVNet* se statickou privátní IP z *192.168.1.101*, postupujte podle následujících kroků hello:

1. Nastavení proměnných pro účet úložiště hello, umístění, skupiny prostředků a toobe pověření použít. Budete potřebovat tooenter uživatelské jméno a heslo pro hello virtuálních počítačů. skupiny účtů a prostředků úložiště Hello již musí existovat.

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type hello name and password of hello local administrator account."
    ```

2. Načtení hello virtuální síť a podsíť má toocreate hello virtuálního počítače v.

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. V případě potřeby vytvořte veřejné hello tooaccess IP adresy virtuálních počítačů z hello Internetu.

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. Vytvořte pomocí hello statickou privátní IP adresou chcete tooassign toohello virtuálního počítače síťový adaptér. Přesvědčte se, zda text hello IP je z rozsahu hello podsítě, který chcete přidat hello virtuálního počítače. Toto je hlavní krok hello k tomuto článku, kde můžete nastavit privátní IP toobe hello statické.

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. Vytvoření virtuálního počítače pomocí hello seskupování vytvořili výše hello.

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

    Očekávaný výstup:
    
        EndTime             : [Date and time]
        Error               : 
        Output              : 
        StartTime           : [Date and time]
        Status              : Succeeded
        TrackingOperationId : [Id]
        RequestId           : [Id]
        StatusCode          : OK 

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a>Načíst statickou privátní IP adresu informace pro síťové rozhraní
tooview hello statickou privátní IP adresu pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *PrivateIpAddress* a  *PrivateIpAllocationMethod*:

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

Očekávaný výstup:

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

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a>Odeberte statickou privátní IP adresu z rozhraní sítě
tooremove hello statickou privátní IP adresou přidána toohello virtuálních počítačů ve skriptu hello nad spuštění hello následující příkazy prostředí PowerShell:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Očekávaný výstup:

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

## <a name="add-a-static-private-ip-address-tooa-network-interface"></a>Přidat statickou privátní IP adresu tooa rozhraní sítě
tooadd statickou privátní IP adresu toohello virtuální počítač vytvořený pomocí skriptu hello výše, spusťte následující příkazy hello:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```
## <a name="change-hello-allocation-method-for-a-private-ip-address-assigned-tooa-network-interface"></a>Změnit metodu hello přidělení privátní IP adresy přiřazené tooa síťové rozhraní

Tooa síťový adaptér s metodou statické nebo dynamické přidělování hello je přiřazená privátní IP adresa. Dynamické IP adresy můžete změnit po spuštění virtuálního počítače, která byla dříve v hello zastavení (deallocated) stavu. To může potenciálně způsobit problémy, pokud hello virtuální počítač je hostitelem služby, která vyžaduje hello stejnou IP adresu, a to i po restartování z zastaveném stavu (deallocated). Statické IP adresy se zachovají, dokud je neodstraní hello virtuálních počítačů. Metoda toochange hello přidělení IP adresy, spusťte následující skript, který změní metoda přidělení hello z dynamické toostatic hello. Pokud je metoda přidělení hello hello aktuální privátní IP adresy statické, změňte *statické* příliš*dynamické* před spuštěním skriptu hello.

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "hello allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for hello IP address" $IP"." -NoNewline
```

Pokud neznáte název hello hello síťový adaptér, můžete zobrazit seznam síťových adaptérů ve skupině prostředků tak, že zadáte následující příkaz hello:

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a>Další kroky
* Další informace o [vyhrazené veřejné IP adresy](virtual-networks-reserved-public-ip.md) adresy.
* Další informace o [veřejné IP (splnění) na úrovni instance](virtual-networks-instance-level-public-ip.md) adresy.
* Poraďte se hello [vyhrazené IP rozhraní REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx).

