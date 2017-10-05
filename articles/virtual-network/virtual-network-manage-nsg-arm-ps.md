---
title: "Správa skupin zabezpečení sítě - prostředí Azure PowerShell | Microsoft Docs"
description: "Naučte se spravovat skupiny zabezpečení sítě pomocí prostředí PowerShell."
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
ms.openlocfilehash: ca7f4926ca4edf9d20612aca74f6ae5f0ed847b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-groups-using-powershell"></a><span data-ttu-id="10ef5-103">Správa skupin zabezpečení sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="10ef5-103">Manage network security groups using PowerShell</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="10ef5-104">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="10ef5-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="10ef5-105">Tento článek se zabývá pomocí modelu nasazení Resource Manager, které společnost Microsoft doporučuje pro většinu nových nasazení místo modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="10ef5-105">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="10ef5-106">Načtení informací o</span><span class="sxs-lookup"><span data-stu-id="10ef5-106">Retrieve Information</span></span>
<span data-ttu-id="10ef5-107">Můžete zobrazit stávající skupiny Nsg, načíst pravidla pro existující skupiny NSG a zjistit, jaké prostředky skupinu NSG je přidružena k.</span><span class="sxs-lookup"><span data-stu-id="10ef5-107">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="10ef5-108">Zobrazit existující skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="10ef5-108">View existing NSGs</span></span>
<span data-ttu-id="10ef5-109">Chcete-li zobrazit všechny existující skupiny Nsg v odběru, spusťte `Get-AzureRmNetworkSecurityGroup` rutiny.</span><span class="sxs-lookup"><span data-stu-id="10ef5-109">To view all existing NSGs in a subscription, run the `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="10ef5-110">Očekávaný výsledek:</span><span class="sxs-lookup"><span data-stu-id="10ef5-110">Expected result:</span></span>

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


<span data-ttu-id="10ef5-111">Chcete-li zobrazit seznam skupin Nsg v určité skupiny zdrojů, spusťte `Get-AzureRmNetworkSecurityGroup` rutiny.</span><span class="sxs-lookup"><span data-stu-id="10ef5-111">To view the list of NSGs in a specific resource group, run the `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="10ef5-112">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="10ef5-112">Expected output:</span></span>

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

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="10ef5-113">Seznam všech pravidel pro skupiny NSG</span><span class="sxs-lookup"><span data-stu-id="10ef5-113">List all rules for an NSG</span></span>
<span data-ttu-id="10ef5-114">Chcete-li zobrazit pravidla s názvem skupiny NSG **NSG front-endu**, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-114">To view the rules of an NSG named **NSG-FrontEnd**, enter the following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

<span data-ttu-id="10ef5-115">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="10ef5-115">Expected output:</span></span>

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
> <span data-ttu-id="10ef5-116">Můžete také použít `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` seznam výchozích pravidel z **NSG front-endu** NSG.</span><span class="sxs-lookup"><span data-stu-id="10ef5-116">You can also use `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` to list the default rules from the **NSG-FrontEnd** NSG.</span></span>
> 

### <a name="view-nsgs-associations"></a><span data-ttu-id="10ef5-117">Zobrazte přidružení skupiny Nsg</span><span class="sxs-lookup"><span data-stu-id="10ef5-117">View NSGs associations</span></span>
<span data-ttu-id="10ef5-118">Chcete-li zobrazit prostředky **NSG front-endu** NSG je spojený s, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-118">To view what resources the **NSG-FrontEnd** NSG is associate with, run the following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

<span data-ttu-id="10ef5-119">Vyhledejte **NetworkInterfaces** a **podsítě** vlastnosti, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="10ef5-119">Look for the **NetworkInterfaces** and **Subnets** properties as shown below:</span></span>

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

<span data-ttu-id="10ef5-120">V předchozím příkladu NSG není přidružen k žádné síťových rozhraní (NIC); je přidružen k podsíti s názvem **front-endu**.</span><span class="sxs-lookup"><span data-stu-id="10ef5-120">In the previous example, the NSG is not associated to any network interfaces (NICs); it is associated to a subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="10ef5-121">Spravovat pravidla</span><span class="sxs-lookup"><span data-stu-id="10ef5-121">Manage rules</span></span>
<span data-ttu-id="10ef5-122">Můžete přidat pravidla do existující skupiny NSG, upravit stávající pravidla a odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="10ef5-122">You can add rules to an existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="10ef5-123">Přidání pravidla</span><span class="sxs-lookup"><span data-stu-id="10ef5-123">Add a rule</span></span>
<span data-ttu-id="10ef5-124">Chcete-li přidat pravidlo, které povoluje **příchozí** přenosy na portu **443** z libovolného počítače k **NSG front-endu** NSG, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10ef5-124">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, complete the following steps:</span></span>

1. <span data-ttu-id="10ef5-125">Spusťte následující příkaz pro načtení existující skupina NSG a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-125">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="10ef5-126">Spusťte následující příkaz pro přidání pravidla k této skupině:</span><span class="sxs-lookup"><span data-stu-id="10ef5-126">Run the following command to add a rule to the NSG:</span></span>

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

3. <span data-ttu-id="10ef5-127">Pokud chcete uložit změny provedené v této skupině, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-127">To save the changes made to the NSG, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    <span data-ttu-id="10ef5-128">Očekávaný výstup zobrazuje pouze pravidla zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="10ef5-128">Expected output showing only the security rules:</span></span>
   
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

### <a name="change-a-rule"></a><span data-ttu-id="10ef5-129">Změna pravidla</span><span class="sxs-lookup"><span data-stu-id="10ef5-129">Change a rule</span></span>
<span data-ttu-id="10ef5-130">Chcete-li změnit pravidlo vytvořili výše, které pokud chcete povolit příchozí přenosy z **Internet** pouze, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="10ef5-130">To change the rule created above to allow inbound traffic from the **Internet** only, follow the steps below.</span></span>

1. <span data-ttu-id="10ef5-131">Spusťte následující příkaz pro načtení existující skupina NSG a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-131">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="10ef5-132">Spusťte následující příkaz s novým nastavením pravidlo:</span><span class="sxs-lookup"><span data-stu-id="10ef5-132">Run the following command with the new rule settings:</span></span>

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

3. <span data-ttu-id="10ef5-133">Pokud chcete uložit změny provedené v této skupině, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-133">To save the changes made to the NSG, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="10ef5-134">Očekávaný výstup zobrazuje pouze pravidla zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="10ef5-134">Expected output showing only the security rules:</span></span>
   
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

### <a name="delete-a-rule"></a><span data-ttu-id="10ef5-135">Odstranění pravidla</span><span class="sxs-lookup"><span data-stu-id="10ef5-135">Delete a rule</span></span>
1. <span data-ttu-id="10ef5-136">Spusťte následující příkaz pro načtení existující skupina NSG a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-136">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="10ef5-137">Spusťte následující příkaz pro odebrání pravidla z této skupině:</span><span class="sxs-lookup"><span data-stu-id="10ef5-137">Run the following command to remove the rule from the NSG:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. <span data-ttu-id="10ef5-138">Uložte změny provedené NSG, tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-138">Save the changes made to the NSG, by running the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="10ef5-139">Očekávaný výstup zobrazuje pouze pravidla zabezpečení, upozornění **https pravidlo** již není uveden:</span><span class="sxs-lookup"><span data-stu-id="10ef5-139">Expected output showing only the security rules, notice the **https-rule** is no longer listed:</span></span>
   
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

## <a name="manage-associations"></a><span data-ttu-id="10ef5-140">Správa přidružení</span><span class="sxs-lookup"><span data-stu-id="10ef5-140">Manage associations</span></span>
<span data-ttu-id="10ef5-141">Můžete přidružit skupiny NSG k podsítí a síťových karet.</span><span class="sxs-lookup"><span data-stu-id="10ef5-141">You can associate an NSG to subnets and NICs.</span></span> <span data-ttu-id="10ef5-142">Můžete také zrušit přidružení skupiny NSG ze všech prostředků, které je přidružené k.</span><span class="sxs-lookup"><span data-stu-id="10ef5-142">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="10ef5-143">Přidružení skupiny NSG k síťové karty</span><span class="sxs-lookup"><span data-stu-id="10ef5-143">Associate an NSG to a NIC</span></span>
<span data-ttu-id="10ef5-144">Pro přidružení **NSG front-endu** NSG k **TestNICWeb1** síťovou kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10ef5-144">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="10ef5-145">Spusťte následující příkaz pro načtení existující skupina NSG a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-145">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="10ef5-146">Spusťte následující příkaz pro načtení stávající síťové karty a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-146">Run the following command to retrieve the existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. <span data-ttu-id="10ef5-147">Nastavte **skupinu zabezpečení sítě** vlastnost **seskupování** proměnné na hodnotu **NSG** proměnné tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-147">Set the **NetworkSecurityGroup** property of the **NIC** variable to the value of the **NSG** variable, by entering the following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. <span data-ttu-id="10ef5-148">Pokud chcete uložit změny provedené na síťový adaptér, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-148">To save the changes made to the NIC, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="10ef5-149">Očekávaný výstup zobrazuje jenom **skupinu zabezpečení sítě** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="10ef5-149">Expected output showing only the **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="10ef5-150">Zrušit přidružení skupiny NSG z síťový adaptér</span><span class="sxs-lookup"><span data-stu-id="10ef5-150">Dissociate an NSG from a NIC</span></span>
<span data-ttu-id="10ef5-151">Zrušení přidružení **NSG front-endu** NSG z **TestNICWeb1** síťovou kartu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10ef5-151">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, complete the following steps:</span></span>

1. <span data-ttu-id="10ef5-152">Spusťte následující příkaz pro načtení stávající síťové karty a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-152">Run the following command to retrieve the existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. <span data-ttu-id="10ef5-153">Nastavte **skupinu zabezpečení sítě** vlastnost **seskupování** proměnnou **$null** spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="10ef5-153">Set the **NetworkSecurityGroup** property of the **NIC** variable to **$null** by running the following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. <span data-ttu-id="10ef5-154">Pokud chcete uložit změny provedené na síťový adaptér, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-154">To save the changes made to the NIC, run the following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="10ef5-155">Očekávaný výstup zobrazuje jenom **skupinu zabezpečení sítě** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="10ef5-155">Expected output showing only the **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="10ef5-156">Zrušit přidružení skupiny NSG z podsítě</span><span class="sxs-lookup"><span data-stu-id="10ef5-156">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="10ef5-157">Zrušení přidružení **NSG front-endu** NSG z **front-endu** podsíť, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10ef5-157">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, complete the following steps:</span></span>

1. <span data-ttu-id="10ef5-158">Spusťte následující příkaz pro načtení existující virtuální síť a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-158">Run the following command to retrieve the existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="10ef5-159">Spusťte následující příkaz pro načtení **front-endu** podsítě a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-159">Run the following command to retrieve the **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="10ef5-160">Nastavte **skupinu zabezpečení sítě** vlastnost **podsíť** proměnnou **$null** tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-160">Set the **NetworkSecurityGroup** property of the **subnet** variable to **$null** by entering the following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. <span data-ttu-id="10ef5-161">Pokud chcete uložit změny provedené v podsíti, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-161">To save the changes made to the subnet, run the following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="10ef5-162">Očekávaný výstup zobrazuje pouze vlastnosti typu **front-endu** podsítě.</span><span class="sxs-lookup"><span data-stu-id="10ef5-162">Expected output showing only the properties of the **FrontEnd** subnet.</span></span> <span data-ttu-id="10ef5-163">Všimněte si, není k dispozici vlastnost pro **skupinu zabezpečení sítě**:</span><span class="sxs-lookup"><span data-stu-id="10ef5-163">Notice there isn't a property for **NetworkSecurityGroup**:</span></span>
   
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

### <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="10ef5-164">Přidružení skupiny NSG k podsíti</span><span class="sxs-lookup"><span data-stu-id="10ef5-164">Associate an NSG to a subnet</span></span>
<span data-ttu-id="10ef5-165">Pro přidružení **NSG front-endu** NSG k **FronEnd** podsíť znovu, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10ef5-165">To associate the **NSG-FrontEnd** NSG to the **FronEnd** subnet again, complete the following steps:</span></span>

1. <span data-ttu-id="10ef5-166">Spusťte následující příkaz pro načtení existující virtuální síť a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-166">Run the following command to retrieve the existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="10ef5-167">Spusťte následující příkaz pro načtení **front-endu** podsítě a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-167">Run the following command to retrieve the **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="10ef5-168">Spusťte následující příkaz pro načtení existující skupina NSG a uložte ho do proměnné:</span><span class="sxs-lookup"><span data-stu-id="10ef5-168">Run the following command to retrieve the existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. <span data-ttu-id="10ef5-169">Nastavte **skupinu zabezpečení sítě** vlastnost **podsíť** proměnnou **$null** spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="10ef5-169">Set the **NetworkSecurityGroup** property of the **subnet** variable to **$null** by running the following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. <span data-ttu-id="10ef5-170">Pokud chcete uložit změny provedené v podsíti, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-170">To save the changes made to the subnet, run the following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="10ef5-171">Očekávaný výstup zobrazuje jenom **skupinu zabezpečení sítě** vlastnost **front-endu** podsítě:</span><span class="sxs-lookup"><span data-stu-id="10ef5-171">Expected output showing only the **NetworkSecurityGroup** property of the **FrontEnd** subnet:</span></span>
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a><span data-ttu-id="10ef5-172">Odstranit skupinu NSG</span><span class="sxs-lookup"><span data-stu-id="10ef5-172">Delete an NSG</span></span>
<span data-ttu-id="10ef5-173">Skupinu NSG můžete odstranit, pouze pokud má není přidružen k žádnému prostředku.</span><span class="sxs-lookup"><span data-stu-id="10ef5-173">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="10ef5-174">Pokud chcete odstranit skupinu NSG, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="10ef5-174">To delete an NSG, follow the steps below.</span></span>

1. <span data-ttu-id="10ef5-175">Chcete-li zkontrolovat prostředky přidružené k skupinu NSG, spusťte `azure network nsg show` jak je znázorněno v [přidružení skupiny Nsg zobrazení](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="10ef5-175">To check the resources associated to an NSG, run the `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="10ef5-176">Pokud skupina NSG je přidružen k žádné síťové adaptéry, spusťte `azure network nic set` jak je znázorněno v [zrušit přidružení skupiny NSG z síťový adaptér](#Dissociate-an-NSG-from-a-NIC) pro každý síťový adaptér.</span><span class="sxs-lookup"><span data-stu-id="10ef5-176">If the NSG is associated to any NICs, run the `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="10ef5-177">Pokud je přidružen k žádné podsíti NSG, spusťte `azure network vnet subnet set` jak je znázorněno v [zrušit přidružení skupiny NSG z podsítě](#Dissociate-an-NSG-from-a-subnet) pro každou podsíť.</span><span class="sxs-lookup"><span data-stu-id="10ef5-177">If the NSG is associated to any subnet, run the `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="10ef5-178">Pokud chcete odstranit NSG, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="10ef5-178">To delete the NSG, run the following command:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > <span data-ttu-id="10ef5-179">`-Force` Parametr zajišťuje nemusíte potvrďte odstranění.</span><span class="sxs-lookup"><span data-stu-id="10ef5-179">The `-Force` parameter ensures you don't need to confirm the deletion.</span></span>
   > 

## <a name="next-steps"></a><span data-ttu-id="10ef5-180">Další kroky</span><span class="sxs-lookup"><span data-stu-id="10ef5-180">Next steps</span></span>
* <span data-ttu-id="10ef5-181">[Povolit protokolování](virtual-network-nsg-manage-log.md) pro skupiny Nsg.</span><span class="sxs-lookup"><span data-stu-id="10ef5-181">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

