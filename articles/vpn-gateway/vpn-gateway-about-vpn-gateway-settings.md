---
title: "Nastavení brány sítě VPN pro Azure připojení mezi různými místy | Microsoft Docs"
description: "Další informace o nastavení brány sítě VPN pro brány virtuální sítě Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: ae665bc5-0089-45d0-a0d5-bc0ab4e79899
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/26/2017
ms.author: cherylmc
ms.openlocfilehash: 07aa6946b9c3994c5afc5c88837f23567b95d8a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a><span data-ttu-id="0b93d-103">O nastavení konfigurace brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="0b93d-103">About VPN Gateway configuration settings</span></span>

<span data-ttu-id="0b93d-104">Brána sítě VPN je typ brány virtuální sítě, které odesílá šifrovaný provoz mezi virtuální sítí a vaše místní umístění přes připojení veřejné.</span><span class="sxs-lookup"><span data-stu-id="0b93d-104">A VPN gateway is a type of virtual network gateway that sends encrypted traffic between your virtual network and your on-premises location across a public connection.</span></span> <span data-ttu-id="0b93d-105">Brána sítě VPN můžete použít také k posílání provozu mezi virtuálními sítěmi přes páteřní strukturu Azure.</span><span class="sxs-lookup"><span data-stu-id="0b93d-105">You can also use a VPN gateway to send traffic between virtual networks across the Azure backbone.</span></span>

<span data-ttu-id="0b93d-106">Připojení k bráně VPN závisí na konfiguraci více prostředků, z nichž každý obsahuje konfigurovat nastavení.</span><span class="sxs-lookup"><span data-stu-id="0b93d-106">A VPN gateway connection relies on the configuration of multiple resources, each of which contains configurable settings.</span></span> <span data-ttu-id="0b93d-107">Části v tomto článku popisují prostředky a nastavení, které se týkají brány VPN pro virtuální síti vytvořené v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0b93d-107">The sections in this article discuss the resources and settings that relate to a VPN gateway for a virtual network created in Resource Manager deployment model.</span></span> <span data-ttu-id="0b93d-108">Můžete najít popisy a diagramy topologie pro každé připojení řešení v [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku.</span><span class="sxs-lookup"><span data-stu-id="0b93d-108">You can find descriptions and topology diagrams for each connection solution in the [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span>

## <span data-ttu-id="0b93d-109"><a name="gwtype"></a>Typy brány</span><span class="sxs-lookup"><span data-stu-id="0b93d-109"><a name="gwtype"></a>Gateway types</span></span>

<span data-ttu-id="0b93d-110">Každá virtuální síť může mít pouze jednu bránu virtuální sítě každého typu.</span><span class="sxs-lookup"><span data-stu-id="0b93d-110">Each virtual network can only have one virtual network gateway of each type.</span></span> <span data-ttu-id="0b93d-111">Při vytváření brány virtuální sítě, musí se ujistěte, že je typ brány odpovídá vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0b93d-111">When you are creating a virtual network gateway, you must make sure that the gateway type is correct for your configuration.</span></span>

<span data-ttu-id="0b93d-112">Dostupné hodnoty pro - GatewayType jsou:</span><span class="sxs-lookup"><span data-stu-id="0b93d-112">The available values for -GatewayType are:</span></span>

* <span data-ttu-id="0b93d-113">Vpn</span><span class="sxs-lookup"><span data-stu-id="0b93d-113">Vpn</span></span>
* <span data-ttu-id="0b93d-114">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="0b93d-114">ExpressRoute</span></span>

<span data-ttu-id="0b93d-115">Brána sítě VPN vyžaduje `-GatewayType` *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="0b93d-115">A VPN gateway requires the `-GatewayType` *Vpn*.</span></span>

<span data-ttu-id="0b93d-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0b93d-116">Example:</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <span data-ttu-id="0b93d-117"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="0b93d-117"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-the-gateway-sku"></a><span data-ttu-id="0b93d-118">Konfigurace skladová položka brány</span><span class="sxs-lookup"><span data-stu-id="0b93d-118">Configure the gateway SKU</span></span>

#### <a name="azure-portal"></a><span data-ttu-id="0b93d-119">portál Azure</span><span class="sxs-lookup"><span data-stu-id="0b93d-119">Azure portal</span></span>

<span data-ttu-id="0b93d-120">Pokud používáte portál Azure k vytvoření brány virtuální sítě Resource Manager, můžete vybrat SKU brány pomocí rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="0b93d-120">If you use the Azure portal to create a Resource Manager virtual network gateway, you can select the gateway SKU by using the dropdown.</span></span> <span data-ttu-id="0b93d-121">Možnosti, které se zobrazí s odpovídají typ brány a typ sítě VPN, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="0b93d-121">The options you are presented with correspond to the Gateway type and VPN type that you select.</span></span>

#### <a name="powershell"></a><span data-ttu-id="0b93d-122">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0b93d-122">PowerShell</span></span>

<span data-ttu-id="0b93d-123">Následující příklad PowerShell Určuje `-GatewaySku` jako VpnGw1.</span><span class="sxs-lookup"><span data-stu-id="0b93d-123">The following PowerShell example specifies the `-GatewaySku` as VpnGw1.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <span data-ttu-id="0b93d-124"><a name="resize"></a>Změna (změny velikosti) skladová položka brány</span><span class="sxs-lookup"><span data-stu-id="0b93d-124"><a name="resize"></a>Change (resize) a gateway SKU</span></span>

<span data-ttu-id="0b93d-125">Pokud chcete upgradovat bránu SKU výkonnější SKU, můžete použít `Resize-AzureRmVirtualNetworkGateway` rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0b93d-125">If you want to upgrade your gateway SKU to a more powerful SKU, you can use the `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet.</span></span> <span data-ttu-id="0b93d-126">Můžete také starší verzi brány velikost SKU použití této rutiny.</span><span class="sxs-lookup"><span data-stu-id="0b93d-126">You can also downgrade the gateway SKU size using this cmdlet.</span></span>

<span data-ttu-id="0b93d-127">Následující příklad PowerShell ukazuje změnu velikosti na VpnGw2 SKU brány.</span><span class="sxs-lookup"><span data-stu-id="0b93d-127">The following PowerShell example shows a gateway SKU being resized to VpnGw2.</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <span data-ttu-id="0b93d-128"><a name="connectiontype"></a>Typy připojení</span><span class="sxs-lookup"><span data-stu-id="0b93d-128"><a name="connectiontype"></a>Connection types</span></span>

<span data-ttu-id="0b93d-129">V modelu nasazení Resource Manager Každá konfigurace vyžaduje typ připojení brány konkrétní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="0b93d-129">In the Resource Manager deployment model, each configuration requires a specific virtual network gateway connection type.</span></span> <span data-ttu-id="0b93d-130">Dostupné hodnoty prostředí PowerShell v Resource Manageru pro `-ConnectionType` jsou:</span><span class="sxs-lookup"><span data-stu-id="0b93d-130">The available Resource Manager PowerShell values for `-ConnectionType` are:</span></span>

* <span data-ttu-id="0b93d-131">Protokol IPsec</span><span class="sxs-lookup"><span data-stu-id="0b93d-131">IPsec</span></span>
* <span data-ttu-id="0b93d-132">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="0b93d-132">Vnet2Vnet</span></span>
* <span data-ttu-id="0b93d-133">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="0b93d-133">ExpressRoute</span></span>
* <span data-ttu-id="0b93d-134">VPNClient</span><span class="sxs-lookup"><span data-stu-id="0b93d-134">VPNClient</span></span>

<span data-ttu-id="0b93d-135">V následujícím příkladu prostředí PowerShell, vytvoříme připojení S2S, která vyžaduje typ připojení *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="0b93d-135">In the following PowerShell example, we create a S2S connection that requires the connection type *IPsec*.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <span data-ttu-id="0b93d-136"><a name="vpntype"></a>Typy sítě VPN</span><span class="sxs-lookup"><span data-stu-id="0b93d-136"><a name="vpntype"></a>VPN types</span></span>

<span data-ttu-id="0b93d-137">Když vytvoříte bránu virtuální sítě pro konfiguraci brány VPN, musíte zadat typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="0b93d-137">When you create the virtual network gateway for a VPN gateway configuration, you must specify a VPN type.</span></span> <span data-ttu-id="0b93d-138">Typ sítě VPN, který zvolíte, závisí na topologii připojení, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0b93d-138">The VPN type that you choose depends on the connection topology that you want to create.</span></span> <span data-ttu-id="0b93d-139">Například připojení P2S vyžaduje typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="0b93d-139">For example, a P2S connection requires a RouteBased VPN type.</span></span> <span data-ttu-id="0b93d-140">Typ sítě VPN můžete také závisí na hardwaru, který používáte.</span><span class="sxs-lookup"><span data-stu-id="0b93d-140">A VPN type can also depend on the hardware that you are using.</span></span> <span data-ttu-id="0b93d-141">Konfigurace S2S vyžadují zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="0b93d-141">S2S configurations require a VPN device.</span></span> <span data-ttu-id="0b93d-142">Některá zařízení VPN podporují pouze určitý typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="0b93d-142">Some VPN devices only support a certain VPN type.</span></span>

<span data-ttu-id="0b93d-143">Typ sítě VPN, kterou vyberete, musí splňovat všechny připojení požadavky pro řešení, že které chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="0b93d-143">The VPN type you select must satisfy all the connection requirements for the solution you want to create.</span></span> <span data-ttu-id="0b93d-144">Pokud chcete vytvořit připojení k bráně S2S VPN a připojení k bráně P2S VPN pro stejnou virtuální síť, je třeba, použijte typ sítě VPN *RouteBased* protože P2S vyžaduje typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="0b93d-144">For example, if you want to create a S2S VPN gateway connection and a P2S VPN gateway connection for the same virtual network, you would use VPN type *RouteBased* because P2S requires a RouteBased VPN type.</span></span> <span data-ttu-id="0b93d-145">Musíte také ověřte, že vaše zařízení VPN podporuje připojení k síti VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="0b93d-145">You would also need to verify that your VPN device supported a RouteBased VPN connection.</span></span> 

<span data-ttu-id="0b93d-146">Po vytvoření brány virtuální sítě, nelze změnit typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="0b93d-146">Once a virtual network gateway has been created, you can't change the VPN type.</span></span> <span data-ttu-id="0b93d-147">Budete muset odstranit bránu virtuální sítě a vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="0b93d-147">You have to delete the virtual network gateway and create a new one.</span></span> <span data-ttu-id="0b93d-148">Existují dva typy sítě VPN:</span><span class="sxs-lookup"><span data-stu-id="0b93d-148">There are two VPN types:</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="0b93d-149">Následující příklad PowerShell Určuje `-VpnType` jako *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="0b93d-149">The following PowerShell example specifies the `-VpnType` as *RouteBased*.</span></span> <span data-ttu-id="0b93d-150">Při vytváření brány se musíte ujistit, že parametr -VpnType odpovídá vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0b93d-150">When you are creating a gateway, you must make sure that the -VpnType is correct for your configuration.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <span data-ttu-id="0b93d-151"><a name="requirements"></a>Požadavky na brány</span><span class="sxs-lookup"><span data-stu-id="0b93d-151"><a name="requirements"></a>Gateway requirements</span></span>

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <span data-ttu-id="0b93d-152"><a name="gwsub"></a>Podsíť brány</span><span class="sxs-lookup"><span data-stu-id="0b93d-152"><a name="gwsub"></a>Gateway subnet</span></span>

<span data-ttu-id="0b93d-153">Před vytvořením brány VPN, musíte vytvořit podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="0b93d-153">Before you create a VPN gateway, you must create a gateway subnet.</span></span> <span data-ttu-id="0b93d-154">Podsíť brány obsahuje IP adresy, které používají bránu virtuální sítě virtuálních počítačů a služeb.</span><span class="sxs-lookup"><span data-stu-id="0b93d-154">The gateway subnet contains the IP addresses that the virtual network gateway VMs and services use.</span></span> <span data-ttu-id="0b93d-155">Když vytvoříte bránu virtuální sítě, virtuální počítače brány jsou nasazené na podsíť brány a nakonfigurovaný pomocí požadovaného nastavení brány sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="0b93d-155">When you create your virtual network gateway, gateway VMs are deployed to the gateway subnet and configured with the required VPN gateway settings.</span></span> <span data-ttu-id="0b93d-156">Nikdy jakékoli jiné (například další virtuální počítače) musíte nasadit na podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="0b93d-156">You must never deploy anything else (for example, additional VMs) to the gateway subnet.</span></span> <span data-ttu-id="0b93d-157">Podsíť brány musí mít název "GatewaySubnet" správně fungovat.</span><span class="sxs-lookup"><span data-stu-id="0b93d-157">The gateway subnet must be named 'GatewaySubnet' to work properly.</span></span> <span data-ttu-id="0b93d-158">Pojmenování podsíť brány "GatewaySubnet" umožňuje vědět, že se jedná o podsítě k nasazení brány virtuální sítě virtuálních počítačů a služeb do Azure.</span><span class="sxs-lookup"><span data-stu-id="0b93d-158">Naming the gateway subnet 'GatewaySubnet' lets Azure know that this is the subnet to deploy the virtual network gateway VMs and services to.</span></span>

<span data-ttu-id="0b93d-159">Při vytváření podsítě brány zadáte počet IP adres, které podsíť obsahuje.</span><span class="sxs-lookup"><span data-stu-id="0b93d-159">When you create the gateway subnet, you specify the number of IP addresses that the subnet contains.</span></span> <span data-ttu-id="0b93d-160">IP adresy v podsíti brány mají při přidělování brány virtuální počítače a služby brány.</span><span class="sxs-lookup"><span data-stu-id="0b93d-160">The IP addresses in the gateway subnet are allocated to the gateway VMs and gateway services.</span></span> <span data-ttu-id="0b93d-161">Některé konfigurace vyžadují více IP adres než jiné.</span><span class="sxs-lookup"><span data-stu-id="0b93d-161">Some configurations require more IP addresses than others.</span></span> <span data-ttu-id="0b93d-162">Podívejte se na pokyny pro konfiguraci, kterou chcete vytvořit a ověřit, jestli podsíť brány, kterou chcete vytvořit splňuje tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="0b93d-162">Look at the instructions for the configuration that you want to create and verify that the gateway subnet you want to create meets those requirements.</span></span> <span data-ttu-id="0b93d-163">Kromě toho můžete ujistěte, že podsítě brány obsahuje dost IP adres, aby dokázala pojmout možné budoucí další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0b93d-163">Additionally, you may want to make sure your gateway subnet contains enough IP addresses to accommodate possible future additional configurations.</span></span> <span data-ttu-id="0b93d-164">Můžete si sice vytvořit podsíť brány jako malé/29, doporučujeme vytvořit podsíť brány/28 nebo větší (/ 28, / 27, /26 atd.).</span><span class="sxs-lookup"><span data-stu-id="0b93d-164">While you can create a gateway subnet as small as /29, we recommend that you create a gateway subnet of /28 or larger (/28, /27, /26 etc.).</span></span> <span data-ttu-id="0b93d-165">Tímto způsobem, pokud přidáte funkci v budoucnu nebudete mít k oddělení bránu, pak odstraňte a vytvořte znovu podsíť brány, aby bylo možné víc IP adres.</span><span class="sxs-lookup"><span data-stu-id="0b93d-165">That way, if you add functionality in the future, you won't have to tear your gateway, then delete and recreate the gateway subnet to allow for more IP addresses.</span></span>

<span data-ttu-id="0b93d-166">Následující příklad správce prostředků PowerShell ukazuje podsíť brány s názvem GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="0b93d-166">The following Resource Manager PowerShell example shows a gateway subnet named GatewaySubnet.</span></span> <span data-ttu-id="0b93d-167">Uvidíte, že zápis CIDR Určuje velikost/27, která umožňuje dost IP adres pro většinu konfigurace, které aktuálně neexistuje.</span><span class="sxs-lookup"><span data-stu-id="0b93d-167">You can see the CIDR notation specifies a /27, which allows for enough IP addresses for most configurations that currently exist.</span></span>

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <span data-ttu-id="0b93d-168"><a name="lng"></a>Brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="0b93d-168"><a name="lng"></a>Local network gateways</span></span>

<span data-ttu-id="0b93d-169">Při vytváření konfiguraci brány VPN, bránu místní sítě často představuje vaše místní umístění.</span><span class="sxs-lookup"><span data-stu-id="0b93d-169">When creating a VPN gateway configuration, the local network gateway often represents your on-premises location.</span></span> <span data-ttu-id="0b93d-170">V modelu nasazení Classic se brána místní sítě označovala jako „místní lokalita“.</span><span class="sxs-lookup"><span data-stu-id="0b93d-170">In the classic deployment model, the local network gateway was referred to as a Local Site.</span></span> 

<span data-ttu-id="0b93d-171">Pojmenujte bránu místní sítě, veřejnou IP adresu místního zařízení VPN a zadáte předpony adres, které se nacházejí na místní umístění.</span><span class="sxs-lookup"><span data-stu-id="0b93d-171">You give the local network gateway a name, the public IP address of the on-premises VPN device, and specify the address prefixes that are located on the on-premises location.</span></span> <span data-ttu-id="0b93d-172">Azure zjistí předpony cílových adres pro síťový provoz, zajímají konfiguraci, která jste zadali pro bránu místní sítě a směruje pakety odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0b93d-172">Azure looks at the destination address prefixes for network traffic, consults the configuration that you have specified for your local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="0b93d-173">Je také zadat brány místní sítě pro sítě VNet-to-VNet konfigurace, které používají připojení k bráně VPN.</span><span class="sxs-lookup"><span data-stu-id="0b93d-173">You also specify local network gateways for VNet-to-VNet configurations that use a VPN gateway connection.</span></span>

<span data-ttu-id="0b93d-174">Následující příklad PowerShell vytvoří novou bránu místní sítě:</span><span class="sxs-lookup"><span data-stu-id="0b93d-174">The following PowerShell example creates a new local network gateway:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

<span data-ttu-id="0b93d-175">V některých případech budete muset změnit nastavení brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="0b93d-175">Sometimes you need to modify the local network gateway settings.</span></span> <span data-ttu-id="0b93d-176">Například když přidáváte nebo odebíráte rozsah adres nebo pokud se IP adresa zařízení VPN změní.</span><span class="sxs-lookup"><span data-stu-id="0b93d-176">For example, when you add or modify the address range, or if the IP address of the VPN device changes.</span></span> <span data-ttu-id="0b93d-177">Pro klasické virtuální síti můžete změnit tato nastavení na portálu classic na stránce místní sítě.</span><span class="sxs-lookup"><span data-stu-id="0b93d-177">For a classic VNet, you can change these settings in the classic portal on the Local Networks page.</span></span> <span data-ttu-id="0b93d-178">Pro správce prostředků najdete v části [upravit nastavení brány místní sítě pomocí prostředí PowerShell](vpn-gateway-modify-local-network-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="0b93d-178">For Resource Manager, see [Modify local network gateway settings using PowerShell](vpn-gateway-modify-local-network-gateway.md).</span></span>

## <span data-ttu-id="0b93d-179"><a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="0b93d-179"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>

<span data-ttu-id="0b93d-180">Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API, rutiny prostředí PowerShell nebo rozhraní příkazového řádku Azure pro konfigurace brány sítě VPN najdete na následujících stránkách:</span><span class="sxs-lookup"><span data-stu-id="0b93d-180">For additional technical resources and specific syntax requirements when using REST APIs, PowerShell cmdlets, or Azure CLI for VPN Gateway configurations, see the following pages:</span></span>

| <span data-ttu-id="0b93d-181">**Classic**</span><span class="sxs-lookup"><span data-stu-id="0b93d-181">**Classic**</span></span> | <span data-ttu-id="0b93d-182">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="0b93d-182">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="0b93d-183">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0b93d-183">PowerShell</span></span>](/powershell/module/azure#networking) |[<span data-ttu-id="0b93d-184">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0b93d-184">PowerShell</span></span>](/powershell/module/azurerm.network#vpn) |
| [<span data-ttu-id="0b93d-185">REST API</span><span class="sxs-lookup"><span data-stu-id="0b93d-185">REST API</span></span>](https://msdn.microsoft.com/library/jj154113) |[<span data-ttu-id="0b93d-186">REST API</span><span class="sxs-lookup"><span data-stu-id="0b93d-186">REST API</span></span>](/rest/api/network/virtualnetworkgateways) |
| <span data-ttu-id="0b93d-187">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="0b93d-187">Not supported</span></span> | [<span data-ttu-id="0b93d-188">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0b93d-188">Azure CLI</span></span>](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a><span data-ttu-id="0b93d-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b93d-189">Next steps</span></span>

<span data-ttu-id="0b93d-190">Další informace o konfiguracích dostupné připojení najdete v tématu [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="0b93d-190">For more information about available connection configurations, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>