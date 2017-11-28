---
title: "nastavení brány aaaVPN pro Azure připojení mezi různými místy | Microsoft Docs"
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
ms.openlocfilehash: 01229d99fa37e30e00aa00f939f488d631b5593c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-vpn-gateway-configuration-settings"></a><span data-ttu-id="c4dbb-103">O nastavení konfigurace brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="c4dbb-103">About VPN Gateway configuration settings</span></span>

<span data-ttu-id="c4dbb-104">Brána sítě VPN je typ brány virtuální sítě, které odesílá šifrovaný provoz mezi virtuální sítí a vaše místní umístění přes připojení veřejné.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-104">A VPN gateway is a type of virtual network gateway that sends encrypted traffic between your virtual network and your on-premises location across a public connection.</span></span> <span data-ttu-id="c4dbb-105">Můžete také použít přenosem toosend brány VPN mezi virtuálními sítěmi přes hello páteřní síti Azure.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-105">You can also use a VPN gateway toosend traffic between virtual networks across hello Azure backbone.</span></span>

<span data-ttu-id="c4dbb-106">Připojení k bráně VPN je závislá na konfiguraci hello více prostředků, z nichž každý obsahuje konfigurovat nastavení.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-106">A VPN gateway connection relies on hello configuration of multiple resources, each of which contains configurable settings.</span></span> <span data-ttu-id="c4dbb-107">Hello části v tomto článku popisují hello prostředky a nastavení, které se týkají brány VPN tooa pro virtuální síti vytvořené v modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-107">hello sections in this article discuss hello resources and settings that relate tooa VPN gateway for a virtual network created in Resource Manager deployment model.</span></span> <span data-ttu-id="c4dbb-108">Můžete najít popisy a diagramy topologie pro každé připojení řešení v hello [o službě VPN Gateway](vpn-gateway-about-vpngateways.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-108">You can find descriptions and topology diagrams for each connection solution in hello [About VPN Gateway](vpn-gateway-about-vpngateways.md) article.</span></span>

## <span data-ttu-id="c4dbb-109"><a name="gwtype"></a>Typy brány</span><span class="sxs-lookup"><span data-stu-id="c4dbb-109"><a name="gwtype"></a>Gateway types</span></span>

<span data-ttu-id="c4dbb-110">Každá virtuální síť může mít pouze jednu bránu virtuální sítě každého typu.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-110">Each virtual network can only have one virtual network gateway of each type.</span></span> <span data-ttu-id="c4dbb-111">Při vytváření brány virtuální sítě, musí se ujistěte, že typ brány hello je správný pro vaši konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-111">When you are creating a virtual network gateway, you must make sure that hello gateway type is correct for your configuration.</span></span>

<span data-ttu-id="c4dbb-112">Hello - GatewayType dostupné hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="c4dbb-112">hello available values for -GatewayType are:</span></span>

* <span data-ttu-id="c4dbb-113">Vpn</span><span class="sxs-lookup"><span data-stu-id="c4dbb-113">Vpn</span></span>
* <span data-ttu-id="c4dbb-114">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c4dbb-114">ExpressRoute</span></span>

<span data-ttu-id="c4dbb-115">Brána sítě VPN vyžaduje hello `-GatewayType` *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-115">A VPN gateway requires hello `-GatewayType` *Vpn*.</span></span>

<span data-ttu-id="c4dbb-116">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c4dbb-116">Example:</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
-VpnType RouteBased
```

## <span data-ttu-id="c4dbb-117"><a name="gwsku"></a>SKU brány</span><span class="sxs-lookup"><span data-stu-id="c4dbb-117"><a name="gwsku"></a>Gateway SKUs</span></span>

[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configure-hello-gateway-sku"></a><span data-ttu-id="c4dbb-118">Konfigurace brány hello SKU</span><span class="sxs-lookup"><span data-stu-id="c4dbb-118">Configure hello gateway SKU</span></span>

#### <a name="azure-portal"></a><span data-ttu-id="c4dbb-119">portál Azure</span><span class="sxs-lookup"><span data-stu-id="c4dbb-119">Azure portal</span></span>

<span data-ttu-id="c4dbb-120">Pokud používáte hello Azure portálu toocreate bránu virtuální sítě Resource Manager, můžete vybrat hello skladová položka brány pomocí rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-120">If you use hello Azure portal toocreate a Resource Manager virtual network gateway, you can select hello gateway SKU by using hello dropdown.</span></span> <span data-ttu-id="c4dbb-121">Hello možností, které se zobrazí s odpovídají toohello typ brány a typ sítě VPN, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-121">hello options you are presented with correspond toohello Gateway type and VPN type that you select.</span></span>

#### <a name="powershell"></a><span data-ttu-id="c4dbb-122">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4dbb-122">PowerShell</span></span>

<span data-ttu-id="c4dbb-123">Hello následující příklad PowerShell určuje hello `-GatewaySku` jako VpnGw1.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-123">hello following PowerShell example specifies hello `-GatewaySku` as VpnGw1.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1 `
-GatewayType Vpn -VpnType RouteBased
```

#### <span data-ttu-id="c4dbb-124"><a name="resize"></a>Změna (změny velikosti) skladová položka brány</span><span class="sxs-lookup"><span data-stu-id="c4dbb-124"><a name="resize"></a>Change (resize) a gateway SKU</span></span>

<span data-ttu-id="c4dbb-125">Pokud chcete tooupgrade tooa skladová položka vaší brány výkonnější SKU, můžete použít hello `Resize-AzureRmVirtualNetworkGateway` rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-125">If you want tooupgrade your gateway SKU tooa more powerful SKU, you can use hello `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet.</span></span> <span data-ttu-id="c4dbb-126">Také může ponížit velikost SKU brány hello použití této rutiny.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-126">You can also downgrade hello gateway SKU size using this cmdlet.</span></span>

<span data-ttu-id="c4dbb-127">Hello následující příklad PowerShell ukazuje, že SKU brány se po změně velikosti tooVpnGw2.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-127">hello following PowerShell example shows a gateway SKU being resized tooVpnGw2.</span></span>

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

## <span data-ttu-id="c4dbb-128"><a name="connectiontype"></a>Typy připojení</span><span class="sxs-lookup"><span data-stu-id="c4dbb-128"><a name="connectiontype"></a>Connection types</span></span>

<span data-ttu-id="c4dbb-129">V modelu nasazení Resource Manager hello Každá konfigurace vyžaduje typ připojení brány konkrétní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-129">In hello Resource Manager deployment model, each configuration requires a specific virtual network gateway connection type.</span></span> <span data-ttu-id="c4dbb-130">Hello k dispozici Resource Manager hodnoty prostředí PowerShell pro `-ConnectionType` jsou:</span><span class="sxs-lookup"><span data-stu-id="c4dbb-130">hello available Resource Manager PowerShell values for `-ConnectionType` are:</span></span>

* <span data-ttu-id="c4dbb-131">Protokol IPsec</span><span class="sxs-lookup"><span data-stu-id="c4dbb-131">IPsec</span></span>
* <span data-ttu-id="c4dbb-132">Vnet2Vnet</span><span class="sxs-lookup"><span data-stu-id="c4dbb-132">Vnet2Vnet</span></span>
* <span data-ttu-id="c4dbb-133">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c4dbb-133">ExpressRoute</span></span>
* <span data-ttu-id="c4dbb-134">VPNClient</span><span class="sxs-lookup"><span data-stu-id="c4dbb-134">VPNClient</span></span>

<span data-ttu-id="c4dbb-135">V následující příklad PowerShell text hello, vytvoříme připojení S2S, která vyžaduje typ připojení hello *IPsec*.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-135">In hello following PowerShell example, we create a S2S connection that requires hello connection type *IPsec*.</span></span>

```powershell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

## <span data-ttu-id="c4dbb-136"><a name="vpntype"></a>Typy sítě VPN</span><span class="sxs-lookup"><span data-stu-id="c4dbb-136"><a name="vpntype"></a>VPN types</span></span>

<span data-ttu-id="c4dbb-137">Když vytvoříte hello brány virtuální sítě pro konfiguraci brány VPN, musíte zadat typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-137">When you create hello virtual network gateway for a VPN gateway configuration, you must specify a VPN type.</span></span> <span data-ttu-id="c4dbb-138">Hello typ sítě VPN, který zvolíte, závisí na topologii hello připojení, které chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-138">hello VPN type that you choose depends on hello connection topology that you want toocreate.</span></span> <span data-ttu-id="c4dbb-139">Například připojení P2S vyžaduje typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-139">For example, a P2S connection requires a RouteBased VPN type.</span></span> <span data-ttu-id="c4dbb-140">Typ sítě VPN můžete také závisí na hello hardwaru, který používáte.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-140">A VPN type can also depend on hello hardware that you are using.</span></span> <span data-ttu-id="c4dbb-141">Konfigurace S2S vyžadují zařízení VPN.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-141">S2S configurations require a VPN device.</span></span> <span data-ttu-id="c4dbb-142">Některá zařízení VPN podporují pouze určitý typ sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-142">Some VPN devices only support a certain VPN type.</span></span>

<span data-ttu-id="c4dbb-143">Hello typ sítě VPN, který jste vybrali musí splňovat všechny požadavky připojení hello hello řešení, že chcete toocreate.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-143">hello VPN type you select must satisfy all hello connection requirements for hello solution you want toocreate.</span></span> <span data-ttu-id="c4dbb-144">Například pokud chcete, aby toocreate připojení S2S brány VPN a připojení k bráně P2S VPN pro hello stejné virtuální síti, používáte typ sítě VPN *RouteBased* protože P2S vyžaduje typ sítě VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-144">For example, if you want toocreate a S2S VPN gateway connection and a P2S VPN gateway connection for hello same virtual network, you would use VPN type *RouteBased* because P2S requires a RouteBased VPN type.</span></span> <span data-ttu-id="c4dbb-145">Také potřebujete tooverify, že vaše zařízení VPN podporuje připojení k síti VPN RouteBased.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-145">You would also need tooverify that your VPN device supported a RouteBased VPN connection.</span></span> 

<span data-ttu-id="c4dbb-146">Po vytvoření brány virtuální sítě, nelze změnit typ sítě VPN hello.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-146">Once a virtual network gateway has been created, you can't change hello VPN type.</span></span> <span data-ttu-id="c4dbb-147">Máte toodelete hello brány virtuální sítě a vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-147">You have toodelete hello virtual network gateway and create a new one.</span></span> <span data-ttu-id="c4dbb-148">Existují dva typy sítě VPN:</span><span class="sxs-lookup"><span data-stu-id="c4dbb-148">There are two VPN types:</span></span>

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

<span data-ttu-id="c4dbb-149">Hello následující příklad PowerShell určuje hello `-VpnType` jako *RouteBased*.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-149">hello following PowerShell example specifies hello `-VpnType` as *RouteBased*.</span></span> <span data-ttu-id="c4dbb-150">Při vytváření brány, ujistěte se, že hello - VpnType odpovídá vaší konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-150">When you are creating a gateway, you must make sure that hello -VpnType is correct for your configuration.</span></span>

```powershell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
-Location 'West US' -IpConfigurations $gwipconfig `
-GatewayType Vpn -VpnType RouteBased
```

## <span data-ttu-id="c4dbb-151"><a name="requirements"></a>Požadavky na brány</span><span class="sxs-lookup"><span data-stu-id="c4dbb-151"><a name="requirements"></a>Gateway requirements</span></span>

[!INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]

## <span data-ttu-id="c4dbb-152"><a name="gwsub"></a>Podsíť brány</span><span class="sxs-lookup"><span data-stu-id="c4dbb-152"><a name="gwsub"></a>Gateway subnet</span></span>

<span data-ttu-id="c4dbb-153">Před vytvořením brány VPN, musíte vytvořit podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-153">Before you create a VPN gateway, you must create a gateway subnet.</span></span> <span data-ttu-id="c4dbb-154">podsíť brány Hello obsahuje hello IP adres tohoto hello brány virtuální sítě virtuálních počítačů a služeb použití.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-154">hello gateway subnet contains hello IP addresses that hello virtual network gateway VMs and services use.</span></span> <span data-ttu-id="c4dbb-155">Když vytvoříte bránu virtuální sítě, brána virtuální počítače jsou nasazené toohello podsíť brány a nakonfigurované hello požadované nastavení brány sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-155">When you create your virtual network gateway, gateway VMs are deployed toohello gateway subnet and configured with hello required VPN gateway settings.</span></span> <span data-ttu-id="c4dbb-156">Je nutné nasadit nikdy cokoli jiného (např. dalších virtuálních počítačů) toohello podsíť brány.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-156">You must never deploy anything else (for example, additional VMs) toohello gateway subnet.</span></span> <span data-ttu-id="c4dbb-157">Hello podsíť brány musí mít název "GatewaySubnet" toowork správně.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-157">hello gateway subnet must be named 'GatewaySubnet' toowork properly.</span></span> <span data-ttu-id="c4dbb-158">Pojmenování podsíť brány hello "GatewaySubnet" umožňuje Azure vědět, že se jedná hello podsíť toodeploy hello brány virtuální sítě virtuálních počítačů a služeb na.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-158">Naming hello gateway subnet 'GatewaySubnet' lets Azure know that this is hello subnet toodeploy hello virtual network gateway VMs and services to.</span></span>

<span data-ttu-id="c4dbb-159">Při vytváření podsítě brány hello určíte, že obsahuje hello počet IP adres, které hello podsítě.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-159">When you create hello gateway subnet, you specify hello number of IP addresses that hello subnet contains.</span></span> <span data-ttu-id="c4dbb-160">Hello IP adresy v podsíti brány hello jsou přidělené toohello brány virtuální počítače a služby brány.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-160">hello IP addresses in hello gateway subnet are allocated toohello gateway VMs and gateway services.</span></span> <span data-ttu-id="c4dbb-161">Některé konfigurace vyžadují více IP adres než jiné.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-161">Some configurations require more IP addresses than others.</span></span> <span data-ttu-id="c4dbb-162">Podívejte se na hello pokyny pro konfiguraci hello má toocreate a ověřte, že tuto podsíť brány hello chcete toocreate splňuje tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-162">Look at hello instructions for hello configuration that you want toocreate and verify that hello gateway subnet you want toocreate meets those requirements.</span></span> <span data-ttu-id="c4dbb-163">Kromě toho můžete toomake se, že podsítě brány obsahuje dostatek IP adresy tooaccommodate možné budoucí další konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-163">Additionally, you may want toomake sure your gateway subnet contains enough IP addresses tooaccommodate possible future additional configurations.</span></span> <span data-ttu-id="c4dbb-164">Můžete si sice vytvořit podsíť brány jako malé/29, doporučujeme vytvořit podsíť brány/28 nebo větší (/ 28, / 27, /26 atd.).</span><span class="sxs-lookup"><span data-stu-id="c4dbb-164">While you can create a gateway subnet as small as /29, we recommend that you create a gateway subnet of /28 or larger (/28, /27, /26 etc.).</span></span> <span data-ttu-id="c4dbb-165">Tímto způsobem, pokud přidáte funkci v hello budoucí, nebude máte tootear bránu, pak odstranit a znovu vytvořte tooallow podsíť brány hello více IP adres.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-165">That way, if you add functionality in hello future, you won't have tootear your gateway, then delete and recreate hello gateway subnet tooallow for more IP addresses.</span></span>

<span data-ttu-id="c4dbb-166">Hello následující příklad správce prostředků PowerShell ukazuje podsíť brány s názvem GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-166">hello following Resource Manager PowerShell example shows a gateway subnet named GatewaySubnet.</span></span> <span data-ttu-id="c4dbb-167">Uvidíte, že hello notaci CIDR Určuje velikost/27, která umožňuje dost IP adres pro většinu konfigurace, které aktuálně existují.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-167">You can see hello CIDR notation specifies a /27, which allows for enough IP addresses for most configurations that currently exist.</span></span>

```powershell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <span data-ttu-id="c4dbb-168"><a name="lng"></a>Brány místní sítě</span><span class="sxs-lookup"><span data-stu-id="c4dbb-168"><a name="lng"></a>Local network gateways</span></span>

<span data-ttu-id="c4dbb-169">Při vytváření konfiguraci brány VPN, brána místní sítě hello často představuje vaše místní umístění.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-169">When creating a VPN gateway configuration, hello local network gateway often represents your on-premises location.</span></span> <span data-ttu-id="c4dbb-170">V modelu nasazení classic hello hello brány místní sítě se označují tooas místního webu.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-170">In hello classic deployment model, hello local network gateway was referred tooas a Local Site.</span></span> 

<span data-ttu-id="c4dbb-171">Zadejte název brány místní sítě hello, hello veřejná IP adresa zařízení VPN místní hello a zadejte hello předpony, které se nacházejí na místní umístění hello.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-171">You give hello local network gateway a name, hello public IP address of hello on-premises VPN device, and specify hello address prefixes that are located on hello on-premises location.</span></span> <span data-ttu-id="c4dbb-172">Azure zjistí hello předpony cílových adres pro síťový provoz, zajímají hello konfigurace, který jste zadali pro bránu místní sítě a směruje pakety odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-172">Azure looks at hello destination address prefixes for network traffic, consults hello configuration that you have specified for your local network gateway, and routes packets accordingly.</span></span> <span data-ttu-id="c4dbb-173">Je také zadat brány místní sítě pro sítě VNet-to-VNet konfigurace, které používají připojení k bráně VPN.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-173">You also specify local network gateways for VNet-to-VNet configurations that use a VPN gateway connection.</span></span>

<span data-ttu-id="c4dbb-174">Následující příklad PowerShell Hello vytvoří novou bránu místní sítě:</span><span class="sxs-lookup"><span data-stu-id="c4dbb-174">hello following PowerShell example creates a new local network gateway:</span></span>

```powershell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

<span data-ttu-id="c4dbb-175">Někdy je nutné nastavení brány místní sítě toomodify hello.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-175">Sometimes you need toomodify hello local network gateway settings.</span></span> <span data-ttu-id="c4dbb-176">Například když přidat nebo upravit rozsah adres hello nebo pokud se změní hello IP adresa zařízení VPN hello.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-176">For example, when you add or modify hello address range, or if hello IP address of hello VPN device changes.</span></span> <span data-ttu-id="c4dbb-177">Pro klasické virtuální síti můžete změnit tato nastavení na portálu classic hello na stránce hello místní sítě.</span><span class="sxs-lookup"><span data-stu-id="c4dbb-177">For a classic VNet, you can change these settings in hello classic portal on hello Local Networks page.</span></span> <span data-ttu-id="c4dbb-178">Pro správce prostředků najdete v části [upravit nastavení brány místní sítě pomocí prostředí PowerShell](vpn-gateway-modify-local-network-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="c4dbb-178">For Resource Manager, see [Modify local network gateway settings using PowerShell](vpn-gateway-modify-local-network-gateway.md).</span></span>

## <span data-ttu-id="c4dbb-179"><a name="resources"></a>Rutiny prostředí PowerShell a rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="c4dbb-179"><a name="resources"></a>REST APIs and PowerShell cmdlets</span></span>

<span data-ttu-id="c4dbb-180">Další zdroje technických informací a specifickou syntaxi požadavky při použití rozhraní REST API, rutiny prostředí PowerShell nebo rozhraní příkazového řádku Azure pro konfigurace brány VPN najdete v tématu hello následující stránky:</span><span class="sxs-lookup"><span data-stu-id="c4dbb-180">For additional technical resources and specific syntax requirements when using REST APIs, PowerShell cmdlets, or Azure CLI for VPN Gateway configurations, see hello following pages:</span></span>

| <span data-ttu-id="c4dbb-181">**Classic**</span><span class="sxs-lookup"><span data-stu-id="c4dbb-181">**Classic**</span></span> | <span data-ttu-id="c4dbb-182">**Resource Manager**</span><span class="sxs-lookup"><span data-stu-id="c4dbb-182">**Resource Manager**</span></span> |
| --- | --- |
| [<span data-ttu-id="c4dbb-183">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4dbb-183">PowerShell</span></span>](/powershell/module/azure#networking) |[<span data-ttu-id="c4dbb-184">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4dbb-184">PowerShell</span></span>](/powershell/module/azurerm.network#vpn) |
| [<span data-ttu-id="c4dbb-185">REST API</span><span class="sxs-lookup"><span data-stu-id="c4dbb-185">REST API</span></span>](https://msdn.microsoft.com/library/jj154113) |[<span data-ttu-id="c4dbb-186">REST API</span><span class="sxs-lookup"><span data-stu-id="c4dbb-186">REST API</span></span>](/rest/api/network/virtualnetworkgateways) |
| <span data-ttu-id="c4dbb-187">Nepodporuje se</span><span class="sxs-lookup"><span data-stu-id="c4dbb-187">Not supported</span></span> | [<span data-ttu-id="c4dbb-188">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c4dbb-188">Azure CLI</span></span>](/cli/azure/network/vnet-gateway)|

## <a name="next-steps"></a><span data-ttu-id="c4dbb-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4dbb-189">Next steps</span></span>

<span data-ttu-id="c4dbb-190">Další informace o konfiguracích dostupné připojení najdete v tématu [o službě VPN Gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="c4dbb-190">For more information about available connection configurations, see [About VPN Gateway](vpn-gateway-about-vpngateways.md).</span></span>