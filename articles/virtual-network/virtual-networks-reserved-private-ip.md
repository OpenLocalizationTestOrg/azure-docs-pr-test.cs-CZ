---
title: "aaaStatic interní privátní IP - virtuální počítač Azure – Classic"
description: "Principy statické interní IP adresy (vyhrazené) a jak toomanage je"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2016
ms.author: jdial
ms.openlocfilehash: 5abe1c59f2f3ed19bcf56c269dfe57ac32d4f601
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-a-static-internal-private-ip-address-using-powershell-classic"></a>Jak tooset interní statickou privátní IP adresy pomocí prostředí PowerShell (klasické)
Ve většině případů nebude nutné toospecify statické interní IP adresu pro virtuální počítač. Virtuální počítače ve virtuální síti se automaticky zobrazí interní IP adresu z rozsahu, který určíte. Ale v některých případech, zadat statickou IP adresu pro konkrétní virtuální počítač má smysl. Například pokud virtuální počítač má toorun DNS nebo bude řadič domény. Statické interní IP adresu zůstává hello virtuálních počítačů i prostřednictvím stavu zastavení nebo deprovision. 

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala hello [modelu nasazení Resource Manager](virtual-networks-static-private-ip-arm-ps.md).
> 
> 

## <a name="how-tooverify-if-a-specific-ip-address-is-available"></a>Jak tooverify, pokud je k dispozici konkrétní IP adresu
tooverify Pokud hello IP adresu *10.0.0.7* je k dispozici ve virtuální síti s názvem *TestVnet*, spusťte následující příkaz prostředí PowerShell hello a ověření hello hodnoty pro *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> Pokud chcete, aby příkaz hello tootest výše v bezpečné prostředí, postupujte podle pokynů hello v [vytvoření virtuální sítě (klasické)](virtual-networks-create-vnet-classic-pportal.md) toocreate virtuální síť s názvem *TestVnet* a ujistěte se, používá hello  *10.0.0.0/8* adresní prostor.
> 
> 

## <a name="how-toospecify-a-static-internal-ip-when-creating-a-vm"></a>Jak toospecify statické interní IP při vytváření virtuálního počítače
Hello následující skript prostředí PowerShell vytvoří novou cloudovou službu s názvem *TestService*, načte bitovou kopii z Azure a pak vytvoří virtuální počítač s názvem *TestVM* v hello novou cloudovou službu pomocí hello načíst image Nastaví hello toobe virtuálního počítače v podsíti s názvem *Subnet-1*a nastaví *10.0.0.7* jako statické interní IP adresu pro hello virtuálních počítačů:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-tooretrieve-static-internal-ip-information-for-a-vm"></a>Jak tooretrieve interní informace o statických IP pro virtuální počítač
tooview hello interní informace o statických IP pro hello virtuální počítač vytvořen pomocí skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello a sledovat hello hodnoty pro *IpAddress*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-tooremove-a-static-internal-ip-from-a-vm"></a>Jak tooremove statické interní IP z virtuálního počítače
tooremove hello statické interní IP přidat toohello virtuálních počítačů ve skriptu hello výše, spusťte následující příkaz prostředí PowerShell hello:

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-tooadd-a-static-internal-ip-tooan-existing-vm"></a>Jak tooadd statické vnitřní tooan IP existující virtuální počítač
tooadd statické vnitřní toohello IP virtuálních počítačů, které jsou vytvořené pomocí skriptu hello výše, o následující příkaz:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a>Další kroky
[Vyhrazená IP adresa](virtual-networks-reserved-public-ip.md)

[Úrovni instance veřejnou IP adresu (splnění)](virtual-networks-instance-level-public-ip.md)

[Vyhrazená IP adresa REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx)

