---
title: "Připojit k více místně na základě zásad zařízení VPN Azure VPN Gateway: Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede konfigurací Azure brány sítě VPN založené na směrování pro více na základě zásad zařízení VPN pomocí Azure Resource Manageru a prostředí PowerShell."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/27/2017
ms.author: yushwang
ms.openlocfilehash: 17211379ec61891982a02efca6730ca0da87c1ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-azure-vpn-gateways-to-multiple-on-premises-policy-based-vpn-devices-using-powershell"></a><span data-ttu-id="6d4b1-103">Připojení brány Azure VPN k více místně na základě zásad zařízení VPN pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d4b1-103">Connect Azure VPN gateways to multiple on-premises policy-based VPN devices using PowerShell</span></span>

<span data-ttu-id="6d4b1-104">Tento článek vám pomůže nakonfigurovat služby Azure na základě trasy VPN gateway se připojit k více místně na základě zásad zařízení VPN využívat vlastní zásady protokolu IPsec/IKE na připojení k síti S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-104">This article helps you configure an Azure route-based VPN gateway to connect to multiple on-premises policy-based VPN devices leveraging custom IPsec/IKE policies on S2S VPN connections.</span></span>

## <a name="about-policy-based-and-route-based-vpn-gateways"></a><span data-ttu-id="6d4b1-105">O zásadové a trasové brány VPN</span><span class="sxs-lookup"><span data-stu-id="6d4b1-105">About policy-based and route-based VPN gateways</span></span>

<span data-ttu-id="6d4b1-106">Zásady - *oproti* zařízení VPN založené na směrování se liší v jak selektory provoz protokolu IPsec jsou nastaveny na připojení:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-106">Policy- *vs.* route-based VPN devices differ in how the IPsec traffic selectors are set on a connection:</span></span>

* <span data-ttu-id="6d4b1-107">**Na základě zásad** zařízení VPN použijte kombinace předpon z obou sítí můžete definovat, jak provoz je zašifrovaná nebo dešifrovat do tunelových propojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-107">**Policy-based** VPN devices use the combinations of prefixes from both networks to define how traffic is encrypted/decrypted through IPsec tunnels.</span></span> <span data-ttu-id="6d4b1-108">Obvykle je založen na zařízení brány firewall, které provádějí filtrování paketů.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-108">It is typically built on firewall devices that perform packet filtering.</span></span> <span data-ttu-id="6d4b1-109">Protokol IPsec tunel šifrování a dešifrování se přidají do paketu filtrování a zpracování modulu.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-109">IPsec tunnel encryption and decryption are added to the packet filtering and processing engine.</span></span>
* <span data-ttu-id="6d4b1-110">**Založené na trasách** zařízení VPN použijte selektory provoz any-to-any (zástupný znak) a umožní směrování/předávání tabulky přímý provoz na jiné tunelových propojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-110">**Route-based** VPN devices use any-to-any (wildcard) traffic selectors, and let routing/forwarding tables direct traffic to different IPsec tunnels.</span></span> <span data-ttu-id="6d4b1-111">Obvykle je založen na platformách směrovače, kde je každý tunelu IPsec modelován jako síťové rozhraní nebo VTI (virtuální tunel rozhraní).</span><span class="sxs-lookup"><span data-stu-id="6d4b1-111">It is typically built on router platforms where each IPsec tunnel is modeled as a network interface or VTI (virtual tunnel interface).</span></span>

<span data-ttu-id="6d4b1-112">Následující diagramy zvýrazněte dva modely:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-112">The following diagrams highlight the two models:</span></span>

### <a name="policy-based-vpn-example"></a><span data-ttu-id="6d4b1-113">Příklad sítě VPN založené na zásadách</span><span class="sxs-lookup"><span data-stu-id="6d4b1-113">Policy-based VPN example</span></span>
![na základě zásad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a><span data-ttu-id="6d4b1-115">Příklad sítě VPN založené na směrování</span><span class="sxs-lookup"><span data-stu-id="6d4b1-115">Route-based VPN example</span></span>
![založené na směrování](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a><span data-ttu-id="6d4b1-117">Podpora Azure pro sítě VPN založené na zásadách</span><span class="sxs-lookup"><span data-stu-id="6d4b1-117">Azure support for policy-based VPN</span></span>
<span data-ttu-id="6d4b1-118">V současné době Azure podporuje oba režimy brány sítě VPN: brány sítě VPN založené na směrování a bran VPN na základě zásad.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-118">Currently, Azure supports both modes of VPN gateways: route-based VPN gateways and policy-based VPN gateways.</span></span> <span data-ttu-id="6d4b1-119">Tyto šablony jsou sestaveny na různých platformách interní, které mít za následek jiné specifikace:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-119">They are built on different internal platforms, which result in different specifications:</span></span>

|                          | <span data-ttu-id="6d4b1-120">**Brána sítě VPN PolicyBased**</span><span class="sxs-lookup"><span data-stu-id="6d4b1-120">**PolicyBased VPN Gateway**</span></span> | <span data-ttu-id="6d4b1-121">**Brána sítě VPN RouteBased**</span><span class="sxs-lookup"><span data-stu-id="6d4b1-121">**RouteBased VPN Gateway**</span></span>               |
| ---                      | ---                         | ---                                      |
| <span data-ttu-id="6d4b1-122">**Služba Azure Gateway SKU**</span><span class="sxs-lookup"><span data-stu-id="6d4b1-122">**Azure Gateway SKU**</span></span>    | <span data-ttu-id="6d4b1-123">Basic</span><span class="sxs-lookup"><span data-stu-id="6d4b1-123">Basic</span></span>                       | <span data-ttu-id="6d4b1-124">Basic, Standard, HighPerformance</span><span class="sxs-lookup"><span data-stu-id="6d4b1-124">Basic, Standard, HighPerformance</span></span>         |
| <span data-ttu-id="6d4b1-125">**Verze IKE**</span><span class="sxs-lookup"><span data-stu-id="6d4b1-125">**IKE version**</span></span>          | <span data-ttu-id="6d4b1-126">IKEv1</span><span class="sxs-lookup"><span data-stu-id="6d4b1-126">IKEv1</span></span>                       | <span data-ttu-id="6d4b1-127">IKEv2</span><span class="sxs-lookup"><span data-stu-id="6d4b1-127">IKEv2</span></span>                                    |
| <span data-ttu-id="6d4b1-128">**Max. Připojení S2S**</span><span class="sxs-lookup"><span data-stu-id="6d4b1-128">**Max. S2S connections**</span></span> | <span data-ttu-id="6d4b1-129">**1**</span><span class="sxs-lookup"><span data-stu-id="6d4b1-129">**1**</span></span>                       | <span data-ttu-id="6d4b1-130">Basic nebo Standard: 10</span><span class="sxs-lookup"><span data-stu-id="6d4b1-130">Basic/Standard: 10</span></span><br> <span data-ttu-id="6d4b1-131">HighPerformance: 30</span><span class="sxs-lookup"><span data-stu-id="6d4b1-131">HighPerformance: 30</span></span> |
|                          |                             |                                          |

<span data-ttu-id="6d4b1-132">Pomocí vlastních zásad protokolu IPsec/IKE, teď můžete konfigurovat Azure brány sítě VPN založené na směrování a použít na základě předpony provoz selektory spolu s možností "**PolicyBasedTrafficSelectors**", pro připojení k místní zařízení VPN na základě zásad.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-132">With the custom IPsec/IKE policy, you can now configure Azure route-based VPN gateways to use prefix-based traffic selectors with option "**PolicyBasedTrafficSelectors**", to connect to on-premises policy-based VPN devices.</span></span> <span data-ttu-id="6d4b1-133">Díky této funkci můžete pro připojení z virtuální sítě Azure a brány VPN k více místně na základě zásad zařízení VPN nebo brány firewall odebrání limit jednoho připojení z aktuální Azure na základě zásad VPN Gateway.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-133">This capability allows you to connect from an Azure virtual network and VPN gateway to multiple on-premises policy-based VPN/firewall devices, removing the single connection limit from the current Azure policy-based VPN gateways.</span></span>

> [!IMPORTANT]
> 1. <span data-ttu-id="6d4b1-134">Pokud chcete povolit toto připojení, musí podporovat zařízení sítě VPN založené na zásadách místní **IKEv2** pro připojení k Azure brány sítě VPN založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-134">To enable this connectivity, your on-premises policy-based VPN devices must support **IKEv2** to connect to the Azure route-based VPN gateways.</span></span> <span data-ttu-id="6d4b1-135">Zkontrolujte dokumentaci sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-135">Check your VPN device specifications.</span></span>
> 2. <span data-ttu-id="6d4b1-136">Místní sítě, připojení prostřednictvím zařízení VPN založené na zásadách s Tento mechanismus lze připojit pouze k virtuální síti Azure; **nelze přenosu do jiných sítích na pracovišti nebo virtuální sítě prostřednictvím tutéž bránu Azure VPN**.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-136">The on-premises networks connecting through policy-based VPN devices with this mechanism can only connect to the Azure virtual network; **they cannot transit to other on-premises networks or virtual networks via the same Azure VPN gateway**.</span></span>
> 3. <span data-ttu-id="6d4b1-137">Možnost konfigurace je součástí vlastní zásady protokolu IPsec/IKE připojení.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-137">The configuration option is part of the custom IPsec/IKE connection policy.</span></span> <span data-ttu-id="6d4b1-138">Pokud povolíte možnost selektoru provozu na základě zásad, musíte zadat dokončení zásady (algoritmy šifrování a integrity protokolu IPsec/IKE, klíče síly a životnost přidružení zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="6d4b1-138">If you enable the policy-based traffic selector option, you must specify the complete policy (IPsec/IKE encryption and integrity algorithms, key strengths, and SA lifetimes).</span></span>

<span data-ttu-id="6d4b1-139">Následující diagram ukazuje proč směrování přenosu prostřednictvím brány Azure VPN nefunguje s možností na základě zásad:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-139">The following diagram shows why transit routing via Azure VPN gateway doesn't work with the policy-based option:</span></span>

![na základě zásad přenosu](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

<span data-ttu-id="6d4b1-141">Jak je vidět v diagramu, má bránu Azure VPN selektory provoz z virtuální sítě pro každé předpony místní sítě, ale není předpony křížové připojení.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-141">As shown in the diagram, the Azure VPN gateway has traffic selectors from the virtual network to each of the on-premises network prefixes, but not the cross-connection prefixes.</span></span> <span data-ttu-id="6d4b1-142">Například místní lokality 2, 3 lokality a lokality 4 můžete každý sdělit VNet1 v uvedeném pořadí, ale nemůže připojit prostřednictvím brány Azure VPN k sobě navzájem.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-142">For example, on-premises site 2, site 3, and site 4 can each communicate to VNet1 respectively, but cannot connect via the Azure VPN gateway to each other.</span></span> <span data-ttu-id="6d4b1-143">Diagram znázorňuje selektory cross-connect přenosy, které nejsou k dispozici ve službě Azure VPN gateway v této konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-143">The diagram shows the cross-connect traffic selectors that are not available in the Azure VPN gateway under this configuration.</span></span>

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="6d4b1-144">Konfigurace založená na zásadách provoz selektory na připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-144">Configure policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="6d4b1-145">Podle pokynů v tomto článku podle stejné příklad, jak je popsáno v [zásady Konfigurace protokolu IPsec/IKE pro připojení S2S nebo VNet-to-VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) k navázání připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-145">The instructions in this article follow the same example as described in [Configure IPsec/IKE policy for S2S or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) to establish a S2S VPN connection.</span></span> <span data-ttu-id="6d4b1-146">To je ukázáno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-146">This is shown in the following diagram:</span></span>

![zásady s2s](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

<span data-ttu-id="6d4b1-148">V pracovním postupu povolte tyto možnosti připojení:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-148">The workflow to enable this connectivity:</span></span>
1. <span data-ttu-id="6d4b1-149">Vytvoření virtuální sítě, brána sítě VPN a bránu místní sítě pro připojení mezi různými místy</span><span class="sxs-lookup"><span data-stu-id="6d4b1-149">Create the virtual network, VPN gateway, and local network gateway for your cross-premises connection</span></span>
2. <span data-ttu-id="6d4b1-150">Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="6d4b1-150">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="6d4b1-151">Použít zásady při vytváření připojení S2S nebo VNet-to-VNet a **povolit provoz na základě zásad selektory** k připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-151">Apply the policy when you create a S2S or VNet-to-VNet connection, and **enable the policy-based traffic selectors** on the connection</span></span>
4. <span data-ttu-id="6d4b1-152">Pokud je již vytvořili připojení, můžete použít nebo aktualizaci zásad na existující připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-152">If the connection is already created, you can apply or update the policy to an existing connection</span></span>

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="6d4b1-153">Povolit provoz na základě zásad selektory na připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-153">Enable policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="6d4b1-154">Ujistěte se, když jste dokončili [část 3 zásady článku Konfigurace protokolu IPsec/IKE](vpn-gateway-ipsecikepolicy-rm-powershell.md) pro tento oddíl.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-154">Make sure you have completed [Part 3 of the Configure IPsec/IKE policy article](vpn-gateway-ipsecikepolicy-rm-powershell.md) for this section.</span></span> <span data-ttu-id="6d4b1-155">Následující příklad používá stejné parametry a kroky:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-155">The following example uses the same parameters and steps:</span></span>

### <a name="step-1---create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="6d4b1-156">Krok 1 – vytvoření virtuální sítě, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="6d4b1-156">Step 1 - Create the virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables--connect-to-your-subscription"></a><span data-ttu-id="6d4b1-157">1. Deklarace proměnných a připojení k vašemu předplatnému</span><span class="sxs-lookup"><span data-stu-id="6d4b1-157">1. Declare your variables & connect to your subscription</span></span>
<span data-ttu-id="6d4b1-158">Pro toto cvičení začneme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-158">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="6d4b1-159">Při konfiguraci pro ostrý provoz nezapomeňte nahradit hodnoty vlastními.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-159">Be sure to replace the values with your own when configuring for production.</span></span>

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```
<span data-ttu-id="6d4b1-160">Pokud chcete používat rutiny Resource Manageru, ujistěte se, že jste přešli do režimu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-160">To use the Resource Manager cmdlets, make sure you switch to PowerShell mode.</span></span> <span data-ttu-id="6d4b1-161">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="6d4b1-161">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="6d4b1-162">Otevřete konzolu prostředí PowerShell a připojte se ke svému účtu.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-162">Open your PowerShell console and connect to your account.</span></span> <span data-ttu-id="6d4b1-163">Připojení vám usnadní následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-163">Use the following sample to help you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-the-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="6d4b1-164">2. Vytvoření virtuální sítě, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="6d4b1-164">2. Create the virtual network, VPN gateway, and local network gateway</span></span>
<span data-ttu-id="6d4b1-165">Následující příklad vytvoří virtuální síť, virtuální sítě TestVNet1 s tři podsítě a bránu VPN.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-165">The following example creates the virtual network, TestVNet1 with three subnets, and the VPN gateway.</span></span> <span data-ttu-id="6d4b1-166">Při nahrazování hodnot je důležité vždy pojmenovat podsítě brány konkrétně GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-166">When substituting values, it's important that you always name your gateway subnet specifically 'GatewaySubnet'.</span></span> <span data-ttu-id="6d4b1-167">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-167">If you name it something else, your gateway creation fails.</span></span>

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a><span data-ttu-id="6d4b1-168">Krok 2 – Vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="6d4b1-168">Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="6d4b1-169">1. Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="6d4b1-169">1. Create an IPsec/IKE policy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d4b1-170">Musíte vytvořit zásady protokolu IPsec/IKE. Chcete-li povolit možnost "UsePolicyBasedTrafficSelectors" k připojení.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-170">You need to create an IPsec/IKE policy in order to enable "UsePolicyBasedTrafficSelectors" option on the connection.</span></span>

<span data-ttu-id="6d4b1-171">Následující příklad vytvoří zásadu protokolu IPsec/IKE se tyto algoritmy a parametry:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-171">The following example creates an IPsec/IKE policy with these algorithms and parameters:</span></span>
* <span data-ttu-id="6d4b1-172">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="6d4b1-172">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="6d4b1-173">Protokol IPsec: AES256, SHA256, PFS24 SA životnost 3600 sekund & 2048KB</span><span class="sxs-lookup"><span data-stu-id="6d4b1-173">IPsec: AES256, SHA256, PFS24, SA Lifetime 3600 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-the-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a><span data-ttu-id="6d4b1-174">2. Vytvoření připojení k síti VPN S2S s selektory provozu na základě zásad a zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="6d4b1-174">2. Create the S2S VPN connection with policy-based traffic selectors and IPsec/IKE policy</span></span>
<span data-ttu-id="6d4b1-175">Vytvoření připojení k síti VPN S2S a použití zásady protokolu IPsec/IKE vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-175">Create an S2S VPN connection and apply the IPsec/IKE policy created in the previous step.</span></span> <span data-ttu-id="6d4b1-176">Mějte na paměti další parametru "-UsePolicyBasedTrafficSelectors $True" což umožňuje na základě zásad provoz selektory pro připojení.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-176">Be aware of the additional parameter "-UsePolicyBasedTrafficSelectors $True"  which enables policy-based traffic selectors on the connection.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="6d4b1-177">Po dokončení těchto kroků, připojení k síti S2S VPN používat definované zásady protokolu IPsec/IKE a povolit provoz na základě zásad selektory pro připojení.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-177">After completing the steps, the S2S VPN connection will use the IPsec/IKE policy defined, and enable policy-based traffic selectors on the connection.</span></span> <span data-ttu-id="6d4b1-178">Stejný postup pro přidání více připojení do dalších místních zařízení VPN založené na zásadách ze tutéž bránu Azure VPN, můžete opakovat.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-178">You can repeat the same steps to add more connections to additional on-premises policy-based VPN devices from the same Azure VPN gateway.</span></span>

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a><span data-ttu-id="6d4b1-179">Aktualizace na základě zásad provoz selektory pro připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-179">Update policy-based traffic selectors for a connection</span></span>
<span data-ttu-id="6d4b1-180">Poslední části se dozvíte, jak aktualizovat možnost selektory provozu na základě zásad pro existující připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-180">The last section shows you how to update the policy-based traffic selectors option for an existing  S2S VPN connection.</span></span>

### <a name="1-get-the-connection"></a><span data-ttu-id="6d4b1-181">1. Získání připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-181">1. Get the connection</span></span>
<span data-ttu-id="6d4b1-182">Získáte prostředek připojení.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-182">Get the connection resource.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-the-policy-based-traffic-selectors-option"></a><span data-ttu-id="6d4b1-183">2. Zaškrtněte možnost selektory provozu na základě zásad</span><span class="sxs-lookup"><span data-stu-id="6d4b1-183">2. Check the policy-based traffic selectors option</span></span>
<span data-ttu-id="6d4b1-184">Následující řádek ukazuje, zda na základě zásad provoz selektory se používají pro připojení:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-184">The following line shows whether the policy-based traffic selectors are used for the connection:</span></span>

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

<span data-ttu-id="6d4b1-185">Pokud vrátí řádek "**True**", pak selektory založené na zásadách provozu se konfigurují na připojení; jinak vrátí hodnotu "**False**."</span><span class="sxs-lookup"><span data-stu-id="6d4b1-185">If the line returns "**True**", then policy-based traffic selectors are configured on the connection; otherwise it returns "**False**."</span></span>

### <a name="3-update-the-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="6d4b1-186">3. Aktualizovat selektory provozu na základě zásad v připojení</span><span class="sxs-lookup"><span data-stu-id="6d4b1-186">3. Update the policy-based traffic selectors on a connection</span></span>
<span data-ttu-id="6d4b1-187">Jakmile je získat prostředek připojení, můžete povolit nebo zakázat možnost.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-187">Once you obtain the connection resource, you can enable or disable the option.</span></span>

#### <a name="disable-usepolicybasedtrafficselectors"></a><span data-ttu-id="6d4b1-188">Zakázat UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="6d4b1-188">Disable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="6d4b1-189">Následující příklad zakáže možnost selektory provozu na základě zásad, ale ponechá zásad protokolu IPsec/IKE beze změny:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-189">The following example disables the policy-based traffic selectors option, but leaves the IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a><span data-ttu-id="6d4b1-190">Povolit UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="6d4b1-190">Enable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="6d4b1-191">Následující příklad umožňuje selektory provozu na základě zásad, ale ponechá zásad protokolu IPsec/IKE beze změny:</span><span class="sxs-lookup"><span data-stu-id="6d4b1-191">The following example enables the policy-based traffic selectors option, but leaves the IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a><span data-ttu-id="6d4b1-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d4b1-192">Next steps</span></span>
<span data-ttu-id="6d4b1-193">Po dokončení připojení můžete do virtuálních sítí přidávat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-193">Once your connection is complete, you can add virtual machines to your virtual networks.</span></span> <span data-ttu-id="6d4b1-194">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6d4b1-194">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>

<span data-ttu-id="6d4b1-195">Také zkontrolujte [zásady Konfigurace protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) Další informace o vlastních zásad protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="6d4b1-195">Also review [Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) for more details on custom IPsec/IKE policies.</span></span>
