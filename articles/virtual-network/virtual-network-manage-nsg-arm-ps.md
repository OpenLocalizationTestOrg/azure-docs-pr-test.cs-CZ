---
title: "aaaManage skupin zabezpečení - sítě, prostředí Azure PowerShell | Microsoft Docs"
description: "Zjistěte, jak toomanage sítě pomocí prostředí PowerShell skupiny zabezpečení."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3706ce6c-d9ae-46cb-a048-f0a4e84dc5cc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 930fe5e0827896ad67b24d84e41a5d3f898ba838
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-powershell"></a>Správa skupin zabezpečení sítě pomocí prostředí PowerShell

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Načtení informací o
Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.

### <a name="view-existing-nsgs"></a>Zobrazit existující skupiny Nsg
tooview všechny existující skupiny Nsg v předplatném, spusťte hello `Get-AzureRmNetworkSecurityGroup` rutiny.

Očekávaný výsledek:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


tooview hello seznam skupin Nsg v určité skupiny zdrojů, spusťte hello `Get-AzureRmNetworkSecurityGroup` rutiny.

Očekávaný výstup:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

### <a name="list-all-rules-for-an-nsg"></a>Seznam všech pravidel pro skupiny NSG
pravidla hello tooview skupinu NSG s názvem **NSG front-endu**, zadejte následující příkaz hello:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

Očekávaný výstup:

    Name                     : rdp-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound

    Name                     : web-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

> [!NOTE]
> Můžete také použít `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello výchozí pravidla z hello **NSG front-endu** NSG.
> 

### <a name="view-nsgs-associations"></a>Zobrazte přidružení skupiny Nsg
tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, spusťte hello následující příkaz:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

Vyhledejte hello **NetworkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

V předchozím příkladu hello hello NSG není přidružené tooany síťových rozhraní (NIC); je přidružená tooa podsíť s názvem **front-endu**.

## <a name="manage-rules"></a>Spravovat pravidla
Můžete přidat pravidla tooan existující skupina NSG, upravit stávající pravidla a odstranit pravidla.

### <a name="add-a-rule"></a>Přidání pravidla
pravidlo, které povoluje tooadd **příchozí** provoz tooport **443** z toohello všechny počítače **NSG front-endu** NSG, dokončení hello následující kroky:

1. Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Spusťte následující příkaz tooadd hello toohello pravidla NSG:

    ```powershell
    Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. toosave hello změn toohello NSG, spusťte následující příkaz hello:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    Očekávaný výstup zobrazuje pouze hello pravidla zabezpečení:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Změna pravidla
pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze, postupujte podle následujících kroků hello.

1. Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Spusťte následující příkaz s hello nové pravidel nastavení hello:

    ```powershell
    Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix Internet `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. toosave hello změn toohello NSG, spusťte následující příkaz hello:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Očekávaný výstup zobrazuje pouze hello pravidla zabezpečení:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Odstranění pravidla
1. Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Spusťte následující příkaz tooremove hello pravidlo z hello NSG hello:

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. Uložte změny provedené toohello hello NSG, spuštěním hello následující příkaz:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Očekávaný výstup zobrazuje pouze hello pravidla zabezpečení, Všimněte si hello **https pravidlo** již není uveden:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Správa přidružení
Můžete přidružit toosubnets NSG a síťových karet. Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.

### <a name="associate-an-nsg-tooa-nic"></a>Přidružit NSG tooa síťový adaptér
tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:

1. Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Spusťte následující příkaz tooretrieve hello stávající síťovou kartu hello a uložte ho do proměnné:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. Sada hello **skupinu zabezpečení sítě** vlastnost hello **seskupování** hodnotu proměnné toohello hello **NSG** proměnné tak, že zadáte následující příkaz hello:

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. toosave hello změn toohello síťového adaptéru, spusťte následující příkaz hello:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Očekávaný výstup zobrazuje pouze hello **skupinu zabezpečení sítě** vlastnost:
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Zrušit přidružení skupiny NSG z síťový adaptér
toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:

1. Spusťte následující příkaz tooretrieve hello stávající síťovou kartu hello a uložte ho do proměnné:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. Sada hello **skupinu zabezpečení sítě** vlastnost hello **seskupování** proměnné příliš**$null** spuštěním hello následující příkaz:

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. toosave hello změn toohello síťového adaptéru, spusťte následující příkaz hello:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Očekávaný výstup zobrazuje pouze hello **skupinu zabezpečení sítě** vlastnost:
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Zrušit přidružení skupiny NSG z podsítě
toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, dokončení hello následující kroky:

1. Spusťte následující příkaz tooretrieve hello existující virtuální síť hello a uložit jako proměnnou:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Spuštění hello následující příkaz tooretrieve hello **front-endu** podsítě a uložte ho do proměnné:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Sada hello **skupinu zabezpečení sítě** vlastnost hello **podsíť** proměnné příliš**$null** zadáním hello následující příkaz:

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. toosave hello změn toohello podsíť, spusťte následující příkaz hello:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Očekávaný výstup zobrazuje pouze hello vlastnosti hello **front-endu** podsítě. Všimněte si, není k dispozici vlastnost pro **skupinu zabezpečení sítě**:
   
            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-tooa-subnet"></a>Přidružení podsíť tooa NSG
tooassociate hello **NSG front-endu** NSG toohello **FronEnd** znovu podsíť, dokončení hello následující kroky:

1. Spusťte následující příkaz tooretrieve hello existující virtuální síť hello a uložit jako proměnnou:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Spuštění hello následující příkaz tooretrieve hello **front-endu** podsítě a uložte ho do proměnné:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. Sada hello **skupinu zabezpečení sítě** vlastnost hello **podsíť** proměnné příliš**$null** spuštěním hello následující příkaz:

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. toosave hello změn toohello podsíť, spusťte následující příkaz hello:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Očekávaný výstup zobrazuje pouze hello **skupinu zabezpečení sítě** vlastnost hello **front-endu** podsítě:
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Odstranit skupinu NSG
Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen. toodelete skupina NSG, postupujte podle následujících kroků hello.

1. toocheck hello prostředky přidružené tooan NSG, spusťte hello `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).
2. Pokud hello NSG přidružená tooany síťové adaptéry, spusťte hello `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér. 
3. Pokud hello NSG přidružená tooany podsíť, spusťte hello `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.
4. hello toodelete NSG, spusťte následující příkaz hello:

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > Hello `-Force` parametr zajišťuje nepotřebujete tooconfirm hello odstranění.
   > 

## <a name="next-steps"></a>Další kroky
* [Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.

