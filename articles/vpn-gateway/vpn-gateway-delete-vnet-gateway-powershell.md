---
title: "Odstranit bránu virtuální sítě: prostředí PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Odstraňte bránu virtuální sítě pomocí prostředí PowerShell v modelu nasazení Resource Manager."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 4d0f085423d5bd60b24d88649ee1d77bcd1d009f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a><span data-ttu-id="a0eef-103">Odstranit bránu virtuální sítě pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0eef-103">Delete a virtual network gateway using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a0eef-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a0eef-104">Azure portal</span></span>](vpn-gateway-delete-vnet-gateway-portal.md)
> * [<span data-ttu-id="a0eef-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a0eef-105">PowerShell</span></span>](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [<span data-ttu-id="a0eef-106">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="a0eef-106">PowerShell (classic)</span></span>](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

<span data-ttu-id="a0eef-107">Existuje několik způsobů, které můžete provést, pokud chcete odstranit bránu virtuální sítě pro konfiguraci brány VPN.</span><span class="sxs-lookup"><span data-stu-id="a0eef-107">There are a couple of different approaches you can take when you want to delete a virtual network gateway for a VPN gateway configuration.</span></span>

- <span data-ttu-id="a0eef-108">Pokud chcete odstranit vše a začít od začátku, jako v případě testovacím prostředí, můžete odstranit skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-108">If you want to delete everything and start over, as in the case of a test environment, you can delete the resource group.</span></span> <span data-ttu-id="a0eef-109">Pokud odstraníte skupinu prostředků, odstraní všechny prostředky ve skupině.</span><span class="sxs-lookup"><span data-stu-id="a0eef-109">When you delete a resource group, it deletes all the resources within the group.</span></span> <span data-ttu-id="a0eef-110">Toto je metoda se doporučuje jenom Pokud nechcete, aby zachovat prostředky ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-110">This is method is only recommended if you don't want to keep any of the resources in the resource group.</span></span> <span data-ttu-id="a0eef-111">Nelze odstranit selektivně pouze několik prostředků pomocí tohoto přístupu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-111">You can't selectively delete only a few resources using this approach.</span></span>

- <span data-ttu-id="a0eef-112">Pokud chcete zachovat některé prostředky ve vaší skupině prostředků, odstraňuje se Brána virtuální sítě je něco víc složité.</span><span class="sxs-lookup"><span data-stu-id="a0eef-112">If you want to keep some of the resources in your resource group, deleting a virtual network gateway becomes slightly more complicated.</span></span> <span data-ttu-id="a0eef-113">Než budete moct odstranit bránu virtuální sítě, musíte nejprve odstranit všechny prostředky, které jsou závislé na bráně.</span><span class="sxs-lookup"><span data-stu-id="a0eef-113">Before you can delete the virtual network gateway, you must first delete any resources that are dependent on the gateway.</span></span> <span data-ttu-id="a0eef-114">Kroky, které budete postupovat podle závisí na typu připojení, které jste vytvořili a závislé prostředky pro každé připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-114">The steps you follow depend on the type of connections that you created and the dependent resources for each connection.</span></span>

## <a name="before-beginning"></a><span data-ttu-id="a0eef-115">Před zahájením</span><span class="sxs-lookup"><span data-stu-id="a0eef-115">Before beginning</span></span>

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a><span data-ttu-id="a0eef-116">1. Stáhněte si nejnovější rutiny Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0eef-116">1. Download the latest Azure Resource Manager PowerShell cmdlets.</span></span>

<span data-ttu-id="a0eef-117">Stáhněte a nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0eef-117">Download and install the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="a0eef-118">Další informace o stažení a instalaci rutin prostředí PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a0eef-118">For more information about downloading and installing PowerShell cmdlets, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="2-connect-to-your-azure-account"></a><span data-ttu-id="a0eef-119">2. Připojte k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="a0eef-119">2. Connect to your Azure account.</span></span>

<span data-ttu-id="a0eef-120">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-120">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="a0eef-121">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="a0eef-121">Use the following example to help you connect:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="a0eef-122">Zkontrolujte předplatná pro příslušný účet.</span><span class="sxs-lookup"><span data-stu-id="a0eef-122">Check the subscriptions for the account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="a0eef-123">Pokud máte více než jedno předplatné, určete předplatné, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="a0eef-123">If you have more than one subscription, specify the subscription that you want to use.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <span data-ttu-id="a0eef-124"><a name="S2S"></a>Odstranit bránu Site-to-Site VPN</span><span class="sxs-lookup"><span data-stu-id="a0eef-124"><a name="S2S"></a>Delete a Site-to-Site VPN gateway</span></span>

<span data-ttu-id="a0eef-125">Pokud chcete odstranit bránu virtuální sítě pro konfiguraci S2S, musíte nejprve odstranit všechny prostředky, které se vztahují na bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-125">To delete a virtual network gateway for a S2S configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="a0eef-126">V určitém pořadí závislé položky musí dojít k odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-126">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="a0eef-127">Při práci s příklady níže, některé musí být hodnoty zadané při další hodnoty jsou výsledek výstup.</span><span class="sxs-lookup"><span data-stu-id="a0eef-127">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="a0eef-128">Můžeme použít následující konkrétní hodnoty v příkladech pro demonstrační účely:</span><span class="sxs-lookup"><span data-stu-id="a0eef-128">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="a0eef-129">Název virtuální sítě: VNet1</span><span class="sxs-lookup"><span data-stu-id="a0eef-129">VNet name: VNet1</span></span><br>
<span data-ttu-id="a0eef-130">Název skupiny prostředků: RG1</span><span class="sxs-lookup"><span data-stu-id="a0eef-130">Resource Group name: RG1</span></span><br>
<span data-ttu-id="a0eef-131">Název brány virtuální sítě: GW1</span><span class="sxs-lookup"><span data-stu-id="a0eef-131">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="a0eef-132">Následující postup se vztahuje k modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0eef-132">The following steps apply to the Resource Manager deployment model.</span></span>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="a0eef-133">1. Získáte bránu virtuální sítě, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="a0eef-133">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a><span data-ttu-id="a0eef-134">2. Zkontrolujte, jestli Brána virtuální sítě má všechna připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-134">2. Check to see if the virtual network gateway has any connections.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a><span data-ttu-id="a0eef-135">3. Odstraňte všechna připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-135">3. Delete all connections.</span></span>

<span data-ttu-id="a0eef-136">Zobrazí se výzva k potvrzení odstranění jednotlivých připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-136">You may be prompted to confirm the deletion of each of the connections.</span></span>

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a><span data-ttu-id="a0eef-137">4. Odstraňte bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-137">4. Delete the virtual network gateway.</span></span>

<span data-ttu-id="a0eef-138">Zobrazí se výzva k potvrzení odstranění brány.</span><span class="sxs-lookup"><span data-stu-id="a0eef-138">You may be prompted to confirm the deletion of the gateway.</span></span> <span data-ttu-id="a0eef-139">Pokud máte kromě konfiguraci S2S P2S konfigurace do této virtuální sítě, odstraňuje se Brána virtuální sítě automaticky odpojte všechny klienty P2S bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="a0eef-139">If you have a P2S configuration to this VNet in addition to your S2S configuration, deleting the virtual network gateway will automatically disconnect all P2S clients without warning.</span></span>


```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="a0eef-140">V tomto okamžiku je Odstraněná brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-140">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="a0eef-141">Další kroky můžete odstranit všechny prostředky, které se již nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="a0eef-141">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="5-delete-the-local-network-gateways"></a><span data-ttu-id="a0eef-142">5 odstranit brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-142">5 Delete the local network gateways.</span></span>

<span data-ttu-id="a0eef-143">Získejte seznam odpovídající brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-143">Get the list of the corresponding local network gateways.</span></span>

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

<span data-ttu-id="a0eef-144">Odstraňte brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-144">Delete the local network gateways.</span></span> <span data-ttu-id="a0eef-145">Zobrazí se výzva k potvrzení odstranění jednotlivých bránu místní sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-145">You may be prompted to confirm the deletion of each of the local network gateway.</span></span>

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a><span data-ttu-id="a0eef-146">6. Odstraňte veřejnou IP adresu prostředky.</span><span class="sxs-lookup"><span data-stu-id="a0eef-146">6. Delete the Public IP address resources.</span></span>

<span data-ttu-id="a0eef-147">Získáte konfiguraci IP brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-147">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="a0eef-148">Získejte seznam veřejnou IP adresu prostředky používané pro tuto bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-148">Get the list of Public IP address resources used for this virtual network gateway.</span></span> <span data-ttu-id="a0eef-149">Pokud brána virtuální sítě byla aktivní aktivní, zobrazí se dvě veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a0eef-149">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="a0eef-150">Odstraňte veřejnou IP adresu prostředky.</span><span class="sxs-lookup"><span data-stu-id="a0eef-150">Delete the Public IP resources.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="a0eef-151">7. Odstraňte podsíť brány a nastavit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a0eef-151">7. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="a0eef-152"><a name="v2v"></a>Odstranit bránu VPN VNet-to-VNet</span><span class="sxs-lookup"><span data-stu-id="a0eef-152"><a name="v2v"></a>Delete a VNet-to-VNet VPN gateway</span></span>

<span data-ttu-id="a0eef-153">Pokud chcete odstranit bránu virtuální sítě pro konfiguraci V2V, musíte nejprve odstranit všechny prostředky, které se vztahují na bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-153">To delete a virtual network gateway for a V2V configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="a0eef-154">V určitém pořadí závislé položky musí dojít k odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-154">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="a0eef-155">Při práci s příklady níže, některé musí být hodnoty zadané při další hodnoty jsou výsledek výstup.</span><span class="sxs-lookup"><span data-stu-id="a0eef-155">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="a0eef-156">Můžeme použít následující konkrétní hodnoty v příkladech pro demonstrační účely:</span><span class="sxs-lookup"><span data-stu-id="a0eef-156">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="a0eef-157">Název virtuální sítě: VNet1</span><span class="sxs-lookup"><span data-stu-id="a0eef-157">VNet name: VNet1</span></span><br>
<span data-ttu-id="a0eef-158">Název skupiny prostředků: RG1</span><span class="sxs-lookup"><span data-stu-id="a0eef-158">Resource Group name: RG1</span></span><br>
<span data-ttu-id="a0eef-159">Název brány virtuální sítě: GW1</span><span class="sxs-lookup"><span data-stu-id="a0eef-159">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="a0eef-160">Následující postup se vztahuje k modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0eef-160">The following steps apply to the Resource Manager deployment model.</span></span>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="a0eef-161">1. Získáte bránu virtuální sítě, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="a0eef-161">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a><span data-ttu-id="a0eef-162">2. Zkontrolujte, jestli Brána virtuální sítě má všechna připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-162">2. Check to see if the virtual network gateway has any connections.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
<span data-ttu-id="a0eef-163">Mohou existovat další připojení k bráně virtuální sítě, které jsou součástí jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-163">There may be other connections to the virtual network gateway that are part of a different resource group.</span></span> <span data-ttu-id="a0eef-164">Zkontrolujte pro další připojení v každé skupině dalších prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-164">Check for additional connections in each additional resource group.</span></span> <span data-ttu-id="a0eef-165">V tomto příkladu jsme se kontroluje připojení z RG2.</span><span class="sxs-lookup"><span data-stu-id="a0eef-165">In this example, we are checking for connections from RG2.</span></span> <span data-ttu-id="a0eef-166">Spustit pro každou skupinu prostředků, abyste měli, která může mít připojení k bráně virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-166">Run this for each resource group that you have which may have a connection to the virtual network gateway.</span></span>

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a><span data-ttu-id="a0eef-167">3. Získejte seznam připojení v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="a0eef-167">3. Get the list of connections in both directions.</span></span>

<span data-ttu-id="a0eef-168">Protože to je konfigurace propojení VNet-to-VNet, je třeba seznam připojení v obou směrech.</span><span class="sxs-lookup"><span data-stu-id="a0eef-168">Because this is a VNet-to-VNet configuration, you need the list of connections in both directions.</span></span>

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
<span data-ttu-id="a0eef-169">V tomto příkladu jsme se kontroluje připojení z RG2.</span><span class="sxs-lookup"><span data-stu-id="a0eef-169">In this example, we are checking for connections from RG2.</span></span> <span data-ttu-id="a0eef-170">Spustit pro každou skupinu prostředků, abyste měli, která může mít připojení k bráně virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-170">Run this for each resource group that you have which may have a connection to the virtual network gateway.</span></span>

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a><span data-ttu-id="a0eef-171">4. Odstraňte všechna připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-171">4. Delete all connections.</span></span>

<span data-ttu-id="a0eef-172">Zobrazí se výzva k potvrzení odstranění jednotlivých připojení.</span><span class="sxs-lookup"><span data-stu-id="a0eef-172">You may be prompted to confirm the deletion of each of the connections.</span></span>

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a><span data-ttu-id="a0eef-173">5. Odstraňte bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-173">5. Delete the virtual network gateway.</span></span>

<span data-ttu-id="a0eef-174">Zobrazí se výzva k potvrzení odstranění brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-174">You may be prompted to confirm the deletion of the virtual network gateway.</span></span> <span data-ttu-id="a0eef-175">Pokud máte P2S konfigurace do vaší virtuální sítě kromě konfiguraci V2V, odstranění brány virtuální sítě automaticky odpojte všechny klienty P2S bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="a0eef-175">If you have P2S configurations to your VNets in addition to your V2V configuration, deleting the virtual network gateways will automatically disconnect all P2S clients without warning.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="a0eef-176">V tomto okamžiku je Odstraněná brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-176">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="a0eef-177">Další kroky můžete odstranit všechny prostředky, které se již nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="a0eef-177">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="6-delete-the-public-ip-address-resources"></a><span data-ttu-id="a0eef-178">6. Odstranit veřejnou IP adresu prostředky</span><span class="sxs-lookup"><span data-stu-id="a0eef-178">6. Delete the Public IP address resources</span></span>

<span data-ttu-id="a0eef-179">Získáte konfiguraci IP brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-179">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="a0eef-180">Získejte seznam veřejnou IP adresu prostředky používané pro tuto bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-180">Get the list of Public IP address resources used for this virtual network gateway.</span></span> <span data-ttu-id="a0eef-181">Pokud brána virtuální sítě byla aktivní aktivní, zobrazí se dvě veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a0eef-181">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="a0eef-182">Odstraňte veřejnou IP adresu prostředky.</span><span class="sxs-lookup"><span data-stu-id="a0eef-182">Delete the Public IP resources.</span></span> <span data-ttu-id="a0eef-183">Zobrazí se výzva k potvrzení odstranění veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a0eef-183">You may be prompted to confirm the deletion of the Public IP.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="a0eef-184">7. Odstraňte podsíť brány a nastavit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a0eef-184">7. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="a0eef-185"><a name="deletep2s"></a>Odstranit bránu VPN typu Point-to-Site</span><span class="sxs-lookup"><span data-stu-id="a0eef-185"><a name="deletep2s"></a>Delete a Point-to-Site VPN gateway</span></span>

<span data-ttu-id="a0eef-186">Pokud chcete odstranit bránu virtuální sítě pro konfiguraci P2S, musíte nejprve odstranit všechny prostředky, které se vztahují na bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-186">To delete a virtual network gateway for a P2S configuration, you must first delete each resource that pertains to the virtual network gateway.</span></span> <span data-ttu-id="a0eef-187">V určitém pořadí závislé položky musí dojít k odstranění prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-187">Resources must be deleted in a certain order due to dependencies.</span></span> <span data-ttu-id="a0eef-188">Při práci s příklady níže, některé musí být hodnoty zadané při další hodnoty jsou výsledek výstup.</span><span class="sxs-lookup"><span data-stu-id="a0eef-188">When working with the examples below, some of the values must be specified, while other values are an output result.</span></span> <span data-ttu-id="a0eef-189">Můžeme použít následující konkrétní hodnoty v příkladech pro demonstrační účely:</span><span class="sxs-lookup"><span data-stu-id="a0eef-189">We use the following specific values in the examples for demonstration purposes:</span></span>

<span data-ttu-id="a0eef-190">Název virtuální sítě: VNet1</span><span class="sxs-lookup"><span data-stu-id="a0eef-190">VNet name: VNet1</span></span><br>
<span data-ttu-id="a0eef-191">Název skupiny prostředků: RG1</span><span class="sxs-lookup"><span data-stu-id="a0eef-191">Resource Group name: RG1</span></span><br>
<span data-ttu-id="a0eef-192">Název brány virtuální sítě: GW1</span><span class="sxs-lookup"><span data-stu-id="a0eef-192">Virtual network gateway name: GW1</span></span><br>

<span data-ttu-id="a0eef-193">Následující postup se vztahuje k modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0eef-193">The following steps apply to the Resource Manager deployment model.</span></span>


>[!NOTE]
> <span data-ttu-id="a0eef-194">Pokud bránu VPN odstraníte, všechny připojené klienty bude odpojen od sítě VNet bez upozornění.</span><span class="sxs-lookup"><span data-stu-id="a0eef-194">When you delete the VPN gateway, all connected clients will be disconnected from the VNet without warning.</span></span>
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a><span data-ttu-id="a0eef-195">1. Získáte bránu virtuální sítě, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="a0eef-195">1. Get the virtual network gateway that you want to delete.</span></span>

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a><span data-ttu-id="a0eef-196">2. Odstraňte bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-196">2. Delete the virtual network gateway.</span></span>

<span data-ttu-id="a0eef-197">Zobrazí se výzva k potvrzení odstranění brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-197">You may be prompted to confirm the deletion of the virtual network gateway.</span></span>

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

<span data-ttu-id="a0eef-198">V tomto okamžiku je Odstraněná brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-198">At this point, your virtual network gateway has been deleted.</span></span> <span data-ttu-id="a0eef-199">Další kroky můžete odstranit všechny prostředky, které se již nepoužívají.</span><span class="sxs-lookup"><span data-stu-id="a0eef-199">You can use the next steps to delete any resources that are no longer being used.</span></span>

### <a name="3-delete-the-public-ip-address-resources"></a><span data-ttu-id="a0eef-200">3. Odstranit veřejnou IP adresu prostředky</span><span class="sxs-lookup"><span data-stu-id="a0eef-200">3. Delete the Public IP address resources</span></span>

<span data-ttu-id="a0eef-201">Získáte konfiguraci IP brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-201">Get the IP configurations of the virtual network gateway.</span></span>

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

<span data-ttu-id="a0eef-202">Získejte seznam veřejné IP adresy použít pro tuto bránu virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="a0eef-202">Get the list of Public IP addresses used for this virtual network gateway.</span></span> <span data-ttu-id="a0eef-203">Pokud brána virtuální sítě byla aktivní aktivní, zobrazí se dvě veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a0eef-203">If the virtual network gateway was active-active, you will see two Public IP addresses.</span></span>

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

<span data-ttu-id="a0eef-204">Odstraňte veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a0eef-204">Delete the Public IPs.</span></span> <span data-ttu-id="a0eef-205">Zobrazí se výzva k potvrzení odstranění veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="a0eef-205">You may be prompted to confirm the deletion of the Public IP.</span></span>

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a><span data-ttu-id="a0eef-206">4. Odstraňte podsíť brány a nastavit konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="a0eef-206">4. Delete the gateway subnet and set the configuration.</span></span>

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <span data-ttu-id="a0eef-207"><a name="delete"></a>Odstraňte bránu VPN odstraníte skupinu prostředků</span><span class="sxs-lookup"><span data-stu-id="a0eef-207"><a name="delete"></a>Delete a VPN gateway by deleting the resource group</span></span>

<span data-ttu-id="a0eef-208">Pokud si nejste zajímá zachování některé z vašich prostředků ve skupině prostředků a chcete začít znovu, můžete odstranit celé skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-208">If you are not concerned about keeping any of your resources in the resource group and you just want to start over, you can delete an entire resource group.</span></span> <span data-ttu-id="a0eef-209">To je rychlý způsob, jak odebrat vše.</span><span class="sxs-lookup"><span data-stu-id="a0eef-209">This is a quick way to remove everything.</span></span> <span data-ttu-id="a0eef-210">Následující postup se vztahuje pouze na modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a0eef-210">The following steps apply only to the Resource Manager deployment model.</span></span>

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a><span data-ttu-id="a0eef-211">1. Získáte seznam všech skupin prostředků v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="a0eef-211">1. Get a list of all the resource groups in your subscription.</span></span>

```powershell
Get-AzureRmResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a><span data-ttu-id="a0eef-212">2. Najděte skupinu prostředků, který chcete odstranit.</span><span class="sxs-lookup"><span data-stu-id="a0eef-212">2. Locate the resource group that you want to delete.</span></span>

<span data-ttu-id="a0eef-213">Najděte skupinu prostředků, která chcete k odstranění a zobrazení seznamu prostředků v příslušné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-213">Locate the resource group that you want to delete and view the list of resources in that resource group.</span></span> <span data-ttu-id="a0eef-214">V příkladu je název skupiny prostředků RG1.</span><span class="sxs-lookup"><span data-stu-id="a0eef-214">In the example, the name of the resource group is RG1.</span></span> <span data-ttu-id="a0eef-215">Upravte v příkladu se načíst seznam všech prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-215">Modify the example to retrieve a list of all the resources.</span></span>

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a><span data-ttu-id="a0eef-216">3. Zkontrolujte prostředky v seznamu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-216">3. Verify the resources in the list.</span></span>

<span data-ttu-id="a0eef-217">Pokud se vrátí seznam, zkontrolujte a ověřit, že chcete odstranit všechny prostředky ve skupině prostředků a také samotnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-217">When the list is returned, review it to verify that you want to delete all the resources in the resource group, as well as the resource group itself.</span></span> <span data-ttu-id="a0eef-218">Pokud chcete zachovat některé prostředky ve skupině prostředků, použijte kroky v předchozích částech tohoto článku se odstranit bránu.</span><span class="sxs-lookup"><span data-stu-id="a0eef-218">If you want to keep some of the resources in the resource group, use the steps in the earlier sections of this article to delete your gateway.</span></span>

### <a name="4-delete-the-resource-group-and-resources"></a><span data-ttu-id="a0eef-219">4. Odstraňte skupinu prostředků a prostředky.</span><span class="sxs-lookup"><span data-stu-id="a0eef-219">4. Delete the resource group and resources.</span></span>

<span data-ttu-id="a0eef-220">Pokud chcete odstranit skupinu prostředků a všechny prostředky, které jsou obsažené ve skupině prostředků, upravte v příkladu a spustit.</span><span class="sxs-lookup"><span data-stu-id="a0eef-220">To delete the resource group and all the resource contained in the resource group, modify the example and run.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a><span data-ttu-id="a0eef-221">5. Zkontrolujte stav.</span><span class="sxs-lookup"><span data-stu-id="a0eef-221">5. Check the status.</span></span>

<span data-ttu-id="a0eef-222">Jak dlouho trvá delší dobu pro Azure. Chcete-li odstranit všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="a0eef-222">It takes some time for Azure to delete all the resources.</span></span> <span data-ttu-id="a0eef-223">Pomocí této rutiny můžete zkontrolovat stav vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0eef-223">You can check the status of your resource group by using this cmdlet.</span></span>

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

<span data-ttu-id="a0eef-224">Výsledek, který je vrácen ukazuje 'Úspěch'.</span><span class="sxs-lookup"><span data-stu-id="a0eef-224">The result that is returned shows 'Succeeded'.</span></span>

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```