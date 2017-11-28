---
title: "Připojení na základě zásad zařízení VPN serveru Azure VPN Gateway toomultiple místní: Azure Resource Manager: prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede konfigurací Azure založené na trasách zařízení brány VPN toomultiple na základě zásad sítě VPN pomocí Azure Resource Manageru a prostředí PowerShell."
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
ms.openlocfilehash: 866c78d96305207106a66cc3300c355e4b6bfbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-vpn-gateways-toomultiple-on-premises-policy-based-vpn-devices-using-powershell"></a><span data-ttu-id="26849-103">Připojení Azure VPN Gateway toomultiple místně na základě zásad zařízení VPN pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="26849-103">Connect Azure VPN gateways toomultiple on-premises policy-based VPN devices using PowerShell</span></span>

<span data-ttu-id="26849-104">Tento článek vám pomůže nakonfigurovat Azure založené na trasách zařízení brány VPN tooconnect toomultiple místně na základě zásad VPN využívat vlastní zásady protokolu IPsec/IKE na připojení k síti S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="26849-104">This article helps you configure an Azure route-based VPN gateway tooconnect toomultiple on-premises policy-based VPN devices leveraging custom IPsec/IKE policies on S2S VPN connections.</span></span>

## <a name="about-policy-based-and-route-based-vpn-gateways"></a><span data-ttu-id="26849-105">O zásadové a trasové brány VPN</span><span class="sxs-lookup"><span data-stu-id="26849-105">About policy-based and route-based VPN gateways</span></span>

<span data-ttu-id="26849-106">Zásady - *oproti* založené na trasách zařízení VPN se liší v jak selektory provoz protokolu IPsec hello jsou nastaveny na připojení:</span><span class="sxs-lookup"><span data-stu-id="26849-106">Policy- *vs.* route-based VPN devices differ in how hello IPsec traffic selectors are set on a connection:</span></span>

* <span data-ttu-id="26849-107">**Na základě zásad** zařízení VPN použijte hello kombinace předpon z obou sítě toodefine jak provoz je zašifrovaná nebo dešifrovat do tunelových propojení IPsec.</span><span class="sxs-lookup"><span data-stu-id="26849-107">**Policy-based** VPN devices use hello combinations of prefixes from both networks toodefine how traffic is encrypted/decrypted through IPsec tunnels.</span></span> <span data-ttu-id="26849-108">Obvykle je založen na zařízení brány firewall, které provádějí filtrování paketů.</span><span class="sxs-lookup"><span data-stu-id="26849-108">It is typically built on firewall devices that perform packet filtering.</span></span> <span data-ttu-id="26849-109">Protokol IPsec tunel šifrování a dešifrování se přidají filtrování paketů toohello a modul zpracování.</span><span class="sxs-lookup"><span data-stu-id="26849-109">IPsec tunnel encryption and decryption are added toohello packet filtering and processing engine.</span></span>
* <span data-ttu-id="26849-110">**Založené na trasách** zařízení VPN použijte selektory provoz any-to-any (zástupný znak) a umožňují směrování/předávání tabulky tunelových propojení IPsec toodifferent přímé přenosy.</span><span class="sxs-lookup"><span data-stu-id="26849-110">**Route-based** VPN devices use any-to-any (wildcard) traffic selectors, and let routing/forwarding tables direct traffic toodifferent IPsec tunnels.</span></span> <span data-ttu-id="26849-111">Obvykle je založen na platformách směrovače, kde je každý tunelu IPsec modelován jako síťové rozhraní nebo VTI (virtuální tunel rozhraní).</span><span class="sxs-lookup"><span data-stu-id="26849-111">It is typically built on router platforms where each IPsec tunnel is modeled as a network interface or VTI (virtual tunnel interface).</span></span>

<span data-ttu-id="26849-112">Hello následující diagramy zvýrazněte dva modely hello:</span><span class="sxs-lookup"><span data-stu-id="26849-112">hello following diagrams highlight hello two models:</span></span>

### <a name="policy-based-vpn-example"></a><span data-ttu-id="26849-113">Příklad sítě VPN založené na zásadách</span><span class="sxs-lookup"><span data-stu-id="26849-113">Policy-based VPN example</span></span>
![na základě zásad](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a><span data-ttu-id="26849-115">Příklad sítě VPN založené na směrování</span><span class="sxs-lookup"><span data-stu-id="26849-115">Route-based VPN example</span></span>
![založené na směrování](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a><span data-ttu-id="26849-117">Podpora Azure pro sítě VPN založené na zásadách</span><span class="sxs-lookup"><span data-stu-id="26849-117">Azure support for policy-based VPN</span></span>
<span data-ttu-id="26849-118">V současné době Azure podporuje oba režimy brány sítě VPN: brány sítě VPN založené na směrování a bran VPN na základě zásad.</span><span class="sxs-lookup"><span data-stu-id="26849-118">Currently, Azure supports both modes of VPN gateways: route-based VPN gateways and policy-based VPN gateways.</span></span> <span data-ttu-id="26849-119">Tyto šablony jsou sestaveny na různých platformách interní, které mít za následek jiné specifikace:</span><span class="sxs-lookup"><span data-stu-id="26849-119">They are built on different internal platforms, which result in different specifications:</span></span>

|                          | <span data-ttu-id="26849-120">**Brána sítě VPN PolicyBased**</span><span class="sxs-lookup"><span data-stu-id="26849-120">**PolicyBased VPN Gateway**</span></span> | <span data-ttu-id="26849-121">**Brána sítě VPN RouteBased**</span><span class="sxs-lookup"><span data-stu-id="26849-121">**RouteBased VPN Gateway**</span></span>               |
| ---                      | ---                         | ---                                      |
| <span data-ttu-id="26849-122">**Služba Azure Gateway SKU**</span><span class="sxs-lookup"><span data-stu-id="26849-122">**Azure Gateway SKU**</span></span>    | <span data-ttu-id="26849-123">Basic</span><span class="sxs-lookup"><span data-stu-id="26849-123">Basic</span></span>                       | <span data-ttu-id="26849-124">Basic, Standard, HighPerformance</span><span class="sxs-lookup"><span data-stu-id="26849-124">Basic, Standard, HighPerformance</span></span>         |
| <span data-ttu-id="26849-125">**Verze IKE**</span><span class="sxs-lookup"><span data-stu-id="26849-125">**IKE version**</span></span>          | <span data-ttu-id="26849-126">IKEv1</span><span class="sxs-lookup"><span data-stu-id="26849-126">IKEv1</span></span>                       | <span data-ttu-id="26849-127">IKEv2</span><span class="sxs-lookup"><span data-stu-id="26849-127">IKEv2</span></span>                                    |
| <span data-ttu-id="26849-128">**Max. Připojení S2S**</span><span class="sxs-lookup"><span data-stu-id="26849-128">**Max. S2S connections**</span></span> | <span data-ttu-id="26849-129">**1**</span><span class="sxs-lookup"><span data-stu-id="26849-129">**1**</span></span>                       | <span data-ttu-id="26849-130">Basic nebo Standard: 10</span><span class="sxs-lookup"><span data-stu-id="26849-130">Basic/Standard: 10</span></span><br> <span data-ttu-id="26849-131">HighPerformance: 30</span><span class="sxs-lookup"><span data-stu-id="26849-131">HighPerformance: 30</span></span> |
|                          |                             |                                          |

<span data-ttu-id="26849-132">Pomocí vlastních zásad protokolu IPsec/IKE hello teď můžete konfigurovat Azure založené na směrování VPN Gateway toouse provozu na základě předpony selektory s možností "**PolicyBasedTrafficSelectors**", zařízení sítě VPN založené na zásadách místní tooon tooconnect.</span><span class="sxs-lookup"><span data-stu-id="26849-132">With hello custom IPsec/IKE policy, you can now configure Azure route-based VPN gateways toouse prefix-based traffic selectors with option "**PolicyBasedTrafficSelectors**", tooconnect tooon-premises policy-based VPN devices.</span></span> <span data-ttu-id="26849-133">Díky této funkci můžete tooconnect z virtuální sítě Azure a toomultiple brány VPN místně na základě zásad zařízení VPN nebo brány firewall hello jednoho připojení limit odebráním hello aktuální Azure na základě zásad brány sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="26849-133">This capability allows you tooconnect from an Azure virtual network and VPN gateway toomultiple on-premises policy-based VPN/firewall devices, removing hello single connection limit from hello current Azure policy-based VPN gateways.</span></span>

> [!IMPORTANT]
> 1. <span data-ttu-id="26849-134">tooenable toto připojení, musí podporovat zařízení sítě VPN založené na zásadách místní **IKEv2** tooconnect toohello Azure brány sítě VPN založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="26849-134">tooenable this connectivity, your on-premises policy-based VPN devices must support **IKEv2** tooconnect toohello Azure route-based VPN gateways.</span></span> <span data-ttu-id="26849-135">Zkontrolujte dokumentaci sítě VPN.</span><span class="sxs-lookup"><span data-stu-id="26849-135">Check your VPN device specifications.</span></span>
> 2. <span data-ttu-id="26849-136">připojení prostřednictvím zařízení VPN založené na zásadách s Tento mechanismus Hello místní sítě lze připojit pouze toohello virtuální síť Azure; **jejich nelze přenosu tooother místní sítě nebo virtuální sítě prostřednictvím hello tutéž bránu Azure VPN**.</span><span class="sxs-lookup"><span data-stu-id="26849-136">hello on-premises networks connecting through policy-based VPN devices with this mechanism can only connect toohello Azure virtual network; **they cannot transit tooother on-premises networks or virtual networks via hello same Azure VPN gateway**.</span></span>
> 3. <span data-ttu-id="26849-137">možnost konfigurace Hello je součástí hello vlastní zásady protokolu IPsec/IKE připojení.</span><span class="sxs-lookup"><span data-stu-id="26849-137">hello configuration option is part of hello custom IPsec/IKE connection policy.</span></span> <span data-ttu-id="26849-138">Pokud povolíte možnost selektoru hello provozu na základě zásad, musíte zadat dokončení zásad hello (algoritmy šifrování a integrity protokolu IPsec/IKE, klíče síly a životnost přidružení zabezpečení).</span><span class="sxs-lookup"><span data-stu-id="26849-138">If you enable hello policy-based traffic selector option, you must specify hello complete policy (IPsec/IKE encryption and integrity algorithms, key strengths, and SA lifetimes).</span></span>

<span data-ttu-id="26849-139">Hello následující diagram znázorňuje, proč směrování přenosu prostřednictvím brány Azure VPN nefunguje s možností na základě zásad hello:</span><span class="sxs-lookup"><span data-stu-id="26849-139">hello following diagram shows why transit routing via Azure VPN gateway doesn't work with hello policy-based option:</span></span>

![na základě zásad přenosu](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

<span data-ttu-id="26849-141">Jak je vidět v diagramu hello, má hello Azure VPN gateway selektory provoz z virtuální sítě tooeach hello předpony hello místní sítě, ale není hello křížové připojení předpony.</span><span class="sxs-lookup"><span data-stu-id="26849-141">As shown in hello diagram, hello Azure VPN gateway has traffic selectors from hello virtual network tooeach of hello on-premises network prefixes, but not hello cross-connection prefixes.</span></span> <span data-ttu-id="26849-142">Například na místní lokalitu 2, 3 lokality a lokality 4 může každý komunikovat tooVNet1 v uvedeném pořadí, ale nemůže připojit prostřednictvím hello Azure VPN gateway tooeach jiné.</span><span class="sxs-lookup"><span data-stu-id="26849-142">For example, on-premises site 2, site 3, and site 4 can each communicate tooVNet1 respectively, but cannot connect via hello Azure VPN gateway tooeach other.</span></span> <span data-ttu-id="26849-143">Hello diagram znázorňuje hello provoz selektory, které nejsou k dispozici v bráně Azure VPN hello v této konfiguraci připojení mezi.</span><span class="sxs-lookup"><span data-stu-id="26849-143">hello diagram shows hello cross-connect traffic selectors that are not available in hello Azure VPN gateway under this configuration.</span></span>

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="26849-144">Konfigurace založená na zásadách provoz selektory na připojení</span><span class="sxs-lookup"><span data-stu-id="26849-144">Configure policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="26849-145">Hello pokyny v této postupujte podle článku hello stejné příklad, jak je popsáno v [zásady Konfigurace protokolu IPsec/IKE pro připojení S2S nebo VNet-to-VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="26849-145">hello instructions in this article follow hello same example as described in [Configure IPsec/IKE policy for S2S or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish a S2S VPN connection.</span></span> <span data-ttu-id="26849-146">To je ukázáno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="26849-146">This is shown in hello following diagram:</span></span>

![zásady s2s](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

<span data-ttu-id="26849-148">Hello pracovního postupu tooenable tyto možnosti připojení:</span><span class="sxs-lookup"><span data-stu-id="26849-148">hello workflow tooenable this connectivity:</span></span>
1. <span data-ttu-id="26849-149">Vytvoření virtuální sítě hello, brána sítě VPN a bránu místní sítě pro připojení mezi různými místy</span><span class="sxs-lookup"><span data-stu-id="26849-149">Create hello virtual network, VPN gateway, and local network gateway for your cross-premises connection</span></span>
2. <span data-ttu-id="26849-150">Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="26849-150">Create an IPsec/IKE policy</span></span>
3. <span data-ttu-id="26849-151">Použít zásady hello při vytváření připojení S2S nebo VNet-to-VNet a **povolit hello provozu na základě zásad selektory** hello připojení</span><span class="sxs-lookup"><span data-stu-id="26849-151">Apply hello policy when you create a S2S or VNet-to-VNet connection, and **enable hello policy-based traffic selectors** on hello connection</span></span>
4. <span data-ttu-id="26849-152">Pokud hello připojení je již vytvořený, můžete použít nebo aktualizovat existující připojení tooan hello zásad</span><span class="sxs-lookup"><span data-stu-id="26849-152">If hello connection is already created, you can apply or update hello policy tooan existing connection</span></span>

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="26849-153">Povolit provoz na základě zásad selektory na připojení</span><span class="sxs-lookup"><span data-stu-id="26849-153">Enable policy-based traffic selectors on a connection</span></span>

<span data-ttu-id="26849-154">Ujistěte se, když jste dokončili [část 3 článku zásady Konfigurace protokolu IPsec/IKE hello](vpn-gateway-ipsecikepolicy-rm-powershell.md) pro tento oddíl.</span><span class="sxs-lookup"><span data-stu-id="26849-154">Make sure you have completed [Part 3 of hello Configure IPsec/IKE policy article](vpn-gateway-ipsecikepolicy-rm-powershell.md) for this section.</span></span> <span data-ttu-id="26849-155">Následující příklad používá Hello hello stejné parametry a kroky:</span><span class="sxs-lookup"><span data-stu-id="26849-155">hello following example uses hello same parameters and steps:</span></span>

### <a name="step-1---create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="26849-156">Krok 1 – Vytvoření hello virtuální sítě, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="26849-156">Step 1 - Create hello virtual network, VPN gateway, and local network gateway</span></span>

#### <a name="1-declare-your-variables--connect-tooyour-subscription"></a><span data-ttu-id="26849-157">1. Deklarace proměnných & připojení tooyour odběru</span><span class="sxs-lookup"><span data-stu-id="26849-157">1. Declare your variables & connect tooyour subscription</span></span>
<span data-ttu-id="26849-158">Pro toto cvičení začneme deklarací proměnných.</span><span class="sxs-lookup"><span data-stu-id="26849-158">For this exercise, we start by declaring our variables.</span></span> <span data-ttu-id="26849-159">Zda tooreplace hello hodnoty vlastními být při konfiguraci pro produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="26849-159">Be sure tooreplace hello values with your own when configuring for production.</span></span>

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
<span data-ttu-id="26849-160">toouse hello rutiny Resource Manager, ujistěte se, že jste přešli tooPowerShell režimu.</span><span class="sxs-lookup"><span data-stu-id="26849-160">toouse hello Resource Manager cmdlets, make sure you switch tooPowerShell mode.</span></span> <span data-ttu-id="26849-161">Další informace najdete v tématu [Použití prostředí Windows PowerShell s Resource Managerem](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="26849-161">For more information, see [Using Windows PowerShell with Resource Manager](../powershell-azure-resource-manager.md).</span></span>

<span data-ttu-id="26849-162">Otevřete konzolu prostředí PowerShell a připojte tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="26849-162">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="26849-163">Použijte následující ukázka toohelp, ke kterým se připojujete hello:</span><span class="sxs-lookup"><span data-stu-id="26849-163">Use hello following sample toohelp you connect:</span></span>

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a><span data-ttu-id="26849-164">2. Vytvoření virtuální sítě hello, brána sítě VPN a bránu místní sítě</span><span class="sxs-lookup"><span data-stu-id="26849-164">2. Create hello virtual network, VPN gateway, and local network gateway</span></span>
<span data-ttu-id="26849-165">Hello následující ukázka vytvoří hello virtuální sítě, virtuální sítě TestVNet1 s tři podsítě a brány VPN hello.</span><span class="sxs-lookup"><span data-stu-id="26849-165">hello following example creates hello virtual network, TestVNet1 with three subnets, and hello VPN gateway.</span></span> <span data-ttu-id="26849-166">Při nahrazování hodnot je důležité vždy pojmenovat podsítě brány konkrétně GatewaySubnet.</span><span class="sxs-lookup"><span data-stu-id="26849-166">When substituting values, it's important that you always name your gateway subnet specifically 'GatewaySubnet'.</span></span> <span data-ttu-id="26849-167">Pokud použijete jiný název, vytvoření brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="26849-167">If you name it something else, your gateway creation fails.</span></span>

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

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a><span data-ttu-id="26849-168">Krok 2 – Vytvoření připojení S2S VPN pomocí zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="26849-168">Step 2 - Create a S2S VPN connection with an IPsec/IKE policy</span></span>

#### <a name="1-create-an-ipsecike-policy"></a><span data-ttu-id="26849-169">1. Vytvoření zásady protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="26849-169">1. Create an IPsec/IKE policy</span></span>

> [!IMPORTANT]
> <span data-ttu-id="26849-170">Je nutné toocreate zásady protokolu IPsec/IKE v pořadí tooenable možnost "UsePolicyBasedTrafficSelectors" hello připojení.</span><span class="sxs-lookup"><span data-stu-id="26849-170">You need toocreate an IPsec/IKE policy in order tooenable "UsePolicyBasedTrafficSelectors" option on hello connection.</span></span>

<span data-ttu-id="26849-171">Hello následující příklad vytvoří zásady protokolu IPsec/IKE s tyto algoritmy a parametry:</span><span class="sxs-lookup"><span data-stu-id="26849-171">hello following example creates an IPsec/IKE policy with these algorithms and parameters:</span></span>
* <span data-ttu-id="26849-172">IKEv2: DHGroup24 AES256, SHA384</span><span class="sxs-lookup"><span data-stu-id="26849-172">IKEv2: AES256, SHA384, DHGroup24</span></span>
* <span data-ttu-id="26849-173">Protokol IPsec: AES256, SHA256, PFS24 SA životnost 3600 sekund & 2048KB</span><span class="sxs-lookup"><span data-stu-id="26849-173">IPsec: AES256, SHA256, PFS24, SA Lifetime 3600 seconds & 2048KB</span></span>

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a><span data-ttu-id="26849-174">2. Vytvoření připojení S2S VPN hello s selektory provozu na základě zásad a zásad protokolu IPsec/IKE</span><span class="sxs-lookup"><span data-stu-id="26849-174">2. Create hello S2S VPN connection with policy-based traffic selectors and IPsec/IKE policy</span></span>
<span data-ttu-id="26849-175">Vytvoření připojení k síti VPN S2S a použití zásady protokolu IPsec/IKE hello vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="26849-175">Create an S2S VPN connection and apply hello IPsec/IKE policy created in hello previous step.</span></span> <span data-ttu-id="26849-176">Mějte na paměti další parametru hello "-UsePolicyBasedTrafficSelectors $True" což umožňuje na základě zásad provoz selektory hello připojení.</span><span class="sxs-lookup"><span data-stu-id="26849-176">Be aware of hello additional parameter "-UsePolicyBasedTrafficSelectors $True"  which enables policy-based traffic selectors on hello connection.</span></span>

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

<span data-ttu-id="26849-177">Po dokončení kroků hello hello připojení S2S VPN používat hello definované zásady protokolu IPsec/IKE a povolit provoz na základě zásad selektory hello připojení.</span><span class="sxs-lookup"><span data-stu-id="26849-177">After completing hello steps, hello S2S VPN connection will use hello IPsec/IKE policy defined, and enable policy-based traffic selectors on hello connection.</span></span> <span data-ttu-id="26849-178">Můžete opakovat hello stejné kroky tooadd další připojení tooadditional místně na základě zásad zařízení VPN z hello tutéž bránu Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="26849-178">You can repeat hello same steps tooadd more connections tooadditional on-premises policy-based VPN devices from hello same Azure VPN gateway.</span></span>

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a><span data-ttu-id="26849-179">Aktualizace na základě zásad provoz selektory pro připojení</span><span class="sxs-lookup"><span data-stu-id="26849-179">Update policy-based traffic selectors for a connection</span></span>
<span data-ttu-id="26849-180">poslední část Hello ukazuje, jak tooupdate hello provozu na základě zásad selektory možnost pro existující připojení S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="26849-180">hello last section shows you how tooupdate hello policy-based traffic selectors option for an existing  S2S VPN connection.</span></span>

### <a name="1-get-hello-connection"></a><span data-ttu-id="26849-181">1. Získání připojení hello</span><span class="sxs-lookup"><span data-stu-id="26849-181">1. Get hello connection</span></span>
<span data-ttu-id="26849-182">Získáte prostředek připojení hello.</span><span class="sxs-lookup"><span data-stu-id="26849-182">Get hello connection resource.</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-hello-policy-based-traffic-selectors-option"></a><span data-ttu-id="26849-183">2. Zkontrolujte možnost selektory hello provozu na základě zásad</span><span class="sxs-lookup"><span data-stu-id="26849-183">2. Check hello policy-based traffic selectors option</span></span>
<span data-ttu-id="26849-184">Hello následující řádek ukazuje, zda hello provozu na základě zásad selektory se používají pro připojení hello:</span><span class="sxs-lookup"><span data-stu-id="26849-184">hello following line shows whether hello policy-based traffic selectors are used for hello connection:</span></span>

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

<span data-ttu-id="26849-185">Pokud vrátí řádek hello "**True**", pak selektory založené na zásadách provozu se konfigurují na hello připojení; jinak vrátí hodnotu "**False**."</span><span class="sxs-lookup"><span data-stu-id="26849-185">If hello line returns "**True**", then policy-based traffic selectors are configured on hello connection; otherwise it returns "**False**."</span></span>

### <a name="3-update-hello-policy-based-traffic-selectors-on-a-connection"></a><span data-ttu-id="26849-186">3. Aktualizovat hello selektory provozu na základě zásad v připojení</span><span class="sxs-lookup"><span data-stu-id="26849-186">3. Update hello policy-based traffic selectors on a connection</span></span>
<span data-ttu-id="26849-187">Po získání hello připojení prostředků můžete povolit nebo zakázat možnost hello.</span><span class="sxs-lookup"><span data-stu-id="26849-187">Once you obtain hello connection resource, you can enable or disable hello option.</span></span>

#### <a name="disable-usepolicybasedtrafficselectors"></a><span data-ttu-id="26849-188">Zakázat UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="26849-188">Disable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="26849-189">Hello následující příklad zakazuje možnost selektory hello provozu na základě zásad, ale nechá hello zásad protokolu IPsec/IKE beze změny:</span><span class="sxs-lookup"><span data-stu-id="26849-189">hello following example disables hello policy-based traffic selectors option, but leaves hello IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a><span data-ttu-id="26849-190">Povolit UsePolicyBasedTrafficSelectors</span><span class="sxs-lookup"><span data-stu-id="26849-190">Enable UsePolicyBasedTrafficSelectors</span></span>
<span data-ttu-id="26849-191">Hello následující příklad povolí možnost selektory hello provozu na základě zásad, ale nechá hello zásad protokolu IPsec/IKE beze změny:</span><span class="sxs-lookup"><span data-stu-id="26849-191">hello following example enables hello policy-based traffic selectors option, but leaves hello IPsec/IKE policy unchanged:</span></span>

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a><span data-ttu-id="26849-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26849-192">Next steps</span></span>
<span data-ttu-id="26849-193">Po dokončení připojení můžete přidat virtuální počítače tooyour virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="26849-193">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="26849-194">Kroky jsou uvedeny v tématu [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="26849-194">See [Create a Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) for steps.</span></span>

<span data-ttu-id="26849-195">Také zkontrolujte [zásady Konfigurace protokolu IPsec/IKE pro připojení S2S VPN nebo VNet-to-VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) Další informace o vlastních zásad protokolu IPsec/IKE.</span><span class="sxs-lookup"><span data-stu-id="26849-195">Also review [Configure IPsec/IKE policy for S2S VPN or VNet-to-VNet connections](vpn-gateway-ipsecikepolicy-rm-powershell.md) for more details on custom IPsec/IKE policies.</span></span>
