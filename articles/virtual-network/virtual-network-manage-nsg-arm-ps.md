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
# <a name="manage-network-security-groups-using-powershell"></a><span data-ttu-id="c7e66-103">Správa skupin zabezpečení sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c7e66-103">Manage network security groups using PowerShell</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="c7e66-104">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c7e66-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c7e66-105">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="c7e66-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="c7e66-106">Načtení informací o</span><span class="sxs-lookup"><span data-stu-id="c7e66-106">Retrieve Information</span></span>
<span data-ttu-id="c7e66-107">Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.</span><span class="sxs-lookup"><span data-stu-id="c7e66-107">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="c7e66-108">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="c7e66-108">View existing NSGs</span></span>
<span data-ttu-id="c7e66-109">tooview všechny existující skupiny Nsg v předplatném, spusťte hello `Get-AzureRmNetworkSecurityGroup` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c7e66-109">tooview all existing NSGs in a subscription, run hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="c7e66-110">Očekávaný výsledek:</span><span class="sxs-lookup"><span data-stu-id="c7e66-110">Expected result:</span></span>

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


<span data-ttu-id="c7e66-111">tooview hello seznam skupin Nsg v určité skupiny zdrojů, spusťte hello `Get-AzureRmNetworkSecurityGroup` rutiny.</span><span class="sxs-lookup"><span data-stu-id="c7e66-111">tooview hello list of NSGs in a specific resource group, run hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="c7e66-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c7e66-112">Expected output:</span></span>

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

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="c7e66-113">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="c7e66-113">List all rules for an NSG</span></span>
<span data-ttu-id="c7e66-114">pravidla hello tooview skupinu NSG s názvem **NSG front-endu**, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-114">tooview hello rules of an NSG named **NSG-FrontEnd**, enter hello following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

<span data-ttu-id="c7e66-115">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="c7e66-115">Expected output:</span></span>

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
> <span data-ttu-id="c7e66-116">Můžete také použít `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello výchozí pravidla z hello **NSG front-endu** NSG.</span><span class="sxs-lookup"><span data-stu-id="c7e66-116">You can also use `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello default rules from hello **NSG-FrontEnd** NSG.</span></span>
> 

### <a name="view-nsgs-associations"></a><span data-ttu-id="c7e66-117">Zobrazte přidružení skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="c7e66-117">View NSGs associations</span></span>
<span data-ttu-id="c7e66-118">tooview jaké prostředky hello **NSG front-endu** NSG je spojený s, spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7e66-118">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

<span data-ttu-id="c7e66-119">Vyhledejte hello **NetworkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="c7e66-119">Look for hello **NetworkInterfaces** and **Subnets** properties as shown below:</span></span>

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

<span data-ttu-id="c7e66-120">V předchozím příkladu hello hello NSG není přidružené tooany síťových rozhraní (NIC); je přidružená tooa podsíť s názvem **front-endu**.</span><span class="sxs-lookup"><span data-stu-id="c7e66-120">In hello previous example, hello NSG is not associated tooany network interfaces (NICs); it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="c7e66-121">Spravovat pravidla</span><span class="sxs-lookup"><span data-stu-id="c7e66-121">Manage rules</span></span>
<span data-ttu-id="c7e66-122">Můžete přidat pravidla tooan existující skupina NSG, upravit stávající pravidla a odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="c7e66-122">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="c7e66-123">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="c7e66-123">Add a rule</span></span>
<span data-ttu-id="c7e66-124">pravidlo, které povoluje tooadd **příchozí** provoz tooport **443** z toohello všechny počítače **NSG front-endu** NSG, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7e66-124">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, complete hello following steps:</span></span>

1. <span data-ttu-id="c7e66-125">Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-125">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="c7e66-126">Spusťte následující příkaz tooadd hello toohello pravidla NSG:</span><span class="sxs-lookup"><span data-stu-id="c7e66-126">Run hello following command tooadd a rule toohello NSG:</span></span>

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

3. <span data-ttu-id="c7e66-127">toosave hello změn toohello NSG, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-127">toosave hello changes made toohello NSG, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    <span data-ttu-id="c7e66-128">Očekávaný výstup zobrazuje pouze hello pravidla zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="c7e66-128">Expected output showing only hello security rules:</span></span>
   
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

### <a name="change-a-rule"></a><span data-ttu-id="c7e66-129">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="c7e66-129">Change a rule</span></span>
<span data-ttu-id="c7e66-130">pravidlo hello toochange vytvořili výše tooallow příchozí provoz z hello **Internet** pouze, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="c7e66-130">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, follow hello steps below.</span></span>

1. <span data-ttu-id="c7e66-131">Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-131">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="c7e66-132">Spusťte následující příkaz s hello nové pravidel nastavení hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-132">Run hello following command with hello new rule settings:</span></span>

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

3. <span data-ttu-id="c7e66-133">toosave hello změn toohello NSG, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-133">toosave hello changes made toohello NSG, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="c7e66-134">Očekávaný výstup zobrazuje pouze hello pravidla zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="c7e66-134">Expected output showing only hello security rules:</span></span>
   
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

### <a name="delete-a-rule"></a><span data-ttu-id="c7e66-135">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="c7e66-135">Delete a rule</span></span>
1. <span data-ttu-id="c7e66-136">Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-136">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="c7e66-137">Spusťte následující příkaz tooremove hello pravidlo z hello NSG hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-137">Run hello following command tooremove hello rule from hello NSG:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. <span data-ttu-id="c7e66-138">Uložte změny provedené toohello hello NSG, spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7e66-138">Save hello changes made toohello NSG, by running hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="c7e66-139">Očekávaný výstup zobrazuje pouze hello pravidla zabezpečení, Všimněte si hello **https pravidlo** již není uveden:</span><span class="sxs-lookup"><span data-stu-id="c7e66-139">Expected output showing only hello security rules, notice hello **https-rule** is no longer listed:</span></span>
   
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

## <a name="manage-associations"></a><span data-ttu-id="c7e66-140">Správa přidružení</span><span class="sxs-lookup"><span data-stu-id="c7e66-140">Manage associations</span></span>
<span data-ttu-id="c7e66-141">Můžete přidružit toosubnets NSG a síťových karet.</span><span class="sxs-lookup"><span data-stu-id="c7e66-141">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="c7e66-142">Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.</span><span class="sxs-lookup"><span data-stu-id="c7e66-142">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="c7e66-143">Přidružit NSG tooa síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="c7e66-143">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="c7e66-144">tooassociate hello **NSG front-endu** NSG toohello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7e66-144">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="c7e66-145">Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-145">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="c7e66-146">Spusťte následující příkaz tooretrieve hello stávající síťovou kartu hello a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="c7e66-146">Run hello following command tooretrieve hello existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. <span data-ttu-id="c7e66-147">Sada hello **skupinu zabezpečení sítě** vlastnost hello **seskupování** hodnotu proměnné toohello hello **NSG** proměnné tak, že zadáte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-147">Set hello **NetworkSecurityGroup** property of hello **NIC** variable toohello value of hello **NSG** variable, by entering hello following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. <span data-ttu-id="c7e66-148">toosave hello změn toohello síťového adaptéru, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-148">toosave hello changes made toohello NIC, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="c7e66-149">Očekávaný výstup zobrazuje pouze hello **skupinu zabezpečení sítě** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c7e66-149">Expected output showing only hello **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="c7e66-150">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="c7e66-150">Dissociate an NSG from a NIC</span></span>
<span data-ttu-id="c7e66-151">toodissociate hello **NSG front-endu** NSG z hello **TestNICWeb1** síťového adaptéru, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7e66-151">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="c7e66-152">Spusťte následující příkaz tooretrieve hello stávající síťovou kartu hello a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="c7e66-152">Run hello following command tooretrieve hello existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. <span data-ttu-id="c7e66-153">Sada hello **skupinu zabezpečení sítě** vlastnost hello **seskupování** proměnné příliš**$null** spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7e66-153">Set hello **NetworkSecurityGroup** property of hello **NIC** variable too**$null** by running hello following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. <span data-ttu-id="c7e66-154">toosave hello změn toohello síťového adaptéru, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-154">toosave hello changes made toohello NIC, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="c7e66-155">Očekávaný výstup zobrazuje pouze hello **skupinu zabezpečení sítě** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c7e66-155">Expected output showing only hello **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="c7e66-156">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="c7e66-156">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="c7e66-157">toodissociate hello **NSG front-endu** NSG z hello **front-endu** podsíť, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7e66-157">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, complete hello following steps:</span></span>

1. <span data-ttu-id="c7e66-158">Spusťte následující příkaz tooretrieve hello existující virtuální síť hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-158">Run hello following command tooretrieve hello existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="c7e66-159">Spuštění hello následující příkaz tooretrieve hello **front-endu** podsítě a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="c7e66-159">Run hello following command tooretrieve hello **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="c7e66-160">Sada hello **skupinu zabezpečení sítě** vlastnost hello **podsíť** proměnné příliš**$null** zadáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7e66-160">Set hello **NetworkSecurityGroup** property of hello **subnet** variable too**$null** by entering hello following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. <span data-ttu-id="c7e66-161">toosave hello změn toohello podsíť, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-161">toosave hello changes made toohello subnet, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="c7e66-162">Očekávaný výstup zobrazuje pouze hello vlastnosti hello **front-endu** podsítě.</span><span class="sxs-lookup"><span data-stu-id="c7e66-162">Expected output showing only hello properties of hello **FrontEnd** subnet.</span></span> <span data-ttu-id="c7e66-163">Všimněte si, není k dispozici vlastnost pro **skupinu zabezpečení sítě**:</span><span class="sxs-lookup"><span data-stu-id="c7e66-163">Notice there isn't a property for **NetworkSecurityGroup**:</span></span>
   
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

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="c7e66-164">Přidružení podsíť tooa NSG</span><span class="sxs-lookup"><span data-stu-id="c7e66-164">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="c7e66-165">tooassociate hello **NSG front-endu** NSG toohello **FronEnd** znovu podsíť, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7e66-165">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, complete hello following steps:</span></span>

1. <span data-ttu-id="c7e66-166">Spusťte následující příkaz tooretrieve hello existující virtuální síť hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-166">Run hello following command tooretrieve hello existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="c7e66-167">Spuštění hello následující příkaz tooretrieve hello **front-endu** podsítě a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="c7e66-167">Run hello following command tooretrieve hello **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="c7e66-168">Spusťte následující příkaz tooretrieve hello existující skupina NSG hello a uložit jako proměnnou:</span><span class="sxs-lookup"><span data-stu-id="c7e66-168">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. <span data-ttu-id="c7e66-169">Sada hello **skupinu zabezpečení sítě** vlastnost hello **podsíť** proměnné příliš**$null** spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7e66-169">Set hello **NetworkSecurityGroup** property of hello **subnet** variable too**$null** by running hello following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. <span data-ttu-id="c7e66-170">toosave hello změn toohello podsíť, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-170">toosave hello changes made toohello subnet, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="c7e66-171">Očekávaný výstup zobrazuje pouze hello **skupinu zabezpečení sítě** vlastnost hello **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="c7e66-171">Expected output showing only hello **NetworkSecurityGroup** property of hello **FrontEnd** subnet:</span></span>
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a><span data-ttu-id="c7e66-172">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="c7e66-172">Delete an NSG</span></span>
<span data-ttu-id="c7e66-173">Skupinu NSG můžete odstranit, pouze pokud je tooany prostředku není přiřazen.</span><span class="sxs-lookup"><span data-stu-id="c7e66-173">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="c7e66-174">toodelete skupina NSG, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="c7e66-174">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="c7e66-175">toocheck hello prostředky přidružené tooan NSG, spusťte hello `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="c7e66-175">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="c7e66-176">Pokud hello NSG přidružená tooany síťové adaptéry, spusťte hello `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="c7e66-176">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="c7e66-177">Pokud hello NSG přidružená tooany podsíť, spusťte hello `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="c7e66-177">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="c7e66-178">hello toodelete NSG, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c7e66-178">toodelete hello NSG, run hello following command:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > <span data-ttu-id="c7e66-179">Hello `-Force` parametr zajišťuje nepotřebujete tooconfirm hello odstranění.</span><span class="sxs-lookup"><span data-stu-id="c7e66-179">hello `-Force` parameter ensures you don't need tooconfirm hello deletion.</span></span>
   > 

## <a name="next-steps"></a><span data-ttu-id="c7e66-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7e66-180">Next steps</span></span>
* <span data-ttu-id="c7e66-181">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="c7e66-181">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

