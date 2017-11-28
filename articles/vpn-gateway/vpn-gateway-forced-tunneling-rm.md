---
title: "Nakonfigurujte vynucené tunelování pro připojení Azure Site-to-Site: Resource Manager | Microsoft Docs"
description: "Jak tooredirect nebo \"Vynutit\" všechny internetový provoz back tooyour místní umístění."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cbe58db8-b598-4c9f-ac88-62c865eb8721
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 6bc52c04ab0749a674c9863be5e4f9a9f7c98df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-azure-resource-manager-deployment-model"></a><span data-ttu-id="8648d-103">Nakonfigurujte vynucené tunelování, pomocí modelu nasazení Azure Resource Manager hello</span><span class="sxs-lookup"><span data-stu-id="8648d-103">Configure forced tunneling using hello Azure Resource Manager deployment model</span></span>

<span data-ttu-id="8648d-104">Vynutit tunelové propojení umožňuje přesměrování nebo "Vynutit" všechny internetový provoz back tooyour místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování.</span><span class="sxs-lookup"><span data-stu-id="8648d-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="8648d-105">Požadavek kritické zabezpečení pro většinu organizace IT zásad.</span><span class="sxs-lookup"><span data-stu-id="8648d-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="8648d-106">Bez vynucené tunelování, internetový provoz z virtuálních počítačů v Azure vždy prochází od Azure síťové infrastruktury přímo na Internetu, toohello bez tooallow možnost hello je provoz hello tooinspect nebo kontrola.</span><span class="sxs-lookup"><span data-stu-id="8648d-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure always traverses from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="8648d-107">Neoprávněný přístup k Internetu může potenciálně vést tooinformation zpřístupnění nebo jiné typy narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="8648d-107">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

<span data-ttu-id="8648d-108">Tento článek vás provede procesem konfigurace vynucené tunelování pro virtuální sítě vytvořené pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-108">This article walks you through configuring forced tunneling for virtual networks created using hello Resource Manager deployment model.</span></span> <span data-ttu-id="8648d-109">Vynucené tunelování se dá konfigurovat pomocí prostředí PowerShell, ne přes portál hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-109">Forced tunneling can be configured by using PowerShell, not through hello portal.</span></span> <span data-ttu-id="8648d-110">Pokud chcete tooconfigure vynuceného tunelování pro model nasazení classic hello, vyberte classic článek z hello následující rozevíracího seznamu:</span><span class="sxs-lookup"><span data-stu-id="8648d-110">If you want tooconfigure forced tunneling for hello classic deployment model, select classic article from hello following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8648d-111">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="8648d-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="8648d-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="8648d-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="about-forced-tunneling"></a><span data-ttu-id="8648d-113">O vynuceného tunelování</span><span class="sxs-lookup"><span data-stu-id="8648d-113">About forced tunneling</span></span>

<span data-ttu-id="8648d-114">Hello následující diagram znázorňuje, jak vynucené tunelování funguje.</span><span class="sxs-lookup"><span data-stu-id="8648d-114">hello following diagram illustrates how forced tunneling works.</span></span> 

![Vynucené tunelování](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

<span data-ttu-id="8648d-116">V příkladu hello výše hello tunelovým propojením front-endu podsíť není vynutit.</span><span class="sxs-lookup"><span data-stu-id="8648d-116">In hello example above, hello Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="8648d-117">Hello úlohy v podsíť Frontend hello můžete i nadále tooaccept a reagovat toocustomer požadavky z hello Internet přímo.</span><span class="sxs-lookup"><span data-stu-id="8648d-117">hello workloads in hello Frontend subnet can continue tooaccept and respond toocustomer requests from hello Internet directly.</span></span> <span data-ttu-id="8648d-118">Hello střední vrstvě a back-end podsítě, vynuceně přesunuty tunelového propojení.</span><span class="sxs-lookup"><span data-stu-id="8648d-118">hello Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="8648d-119">Všechny odchozí připojení z těchto dvou podsítí toohello Internetu bude tooan vynucené nebo přesměrované zpět na místní lokalitu prostřednictvím jednoho hello tunelovými propojeními S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="8648d-119">Any outbound connections from these two subnets toohello Internet will be forced or redirected back tooan on-premises site via one of hello S2S VPN tunnels.</span></span>

<span data-ttu-id="8648d-120">To vám umožní toorestrict a kontrolovat přístup k Internetu z virtuálních počítačů nebo cloudových služeb v Azure, a přitom dál tooenable vaší architektury víceúrovňová služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="8648d-120">This allows you toorestrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing tooenable your multi-tier service architecture required.</span></span> <span data-ttu-id="8648d-121">Pokud nejsou žádná internetového zatížení ve virtuálních sítích, můžete také použít vynucené tunelování toohello celý virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8648d-121">If there are no Internet-facing workloads in your virtual networks, you also can apply forced tunneling toohello entire virtual networks.</span></span>

## <a name="requirements-and-considerations"></a><span data-ttu-id="8648d-122">Požadavky a důležité informace</span><span class="sxs-lookup"><span data-stu-id="8648d-122">Requirements and considerations</span></span>

<span data-ttu-id="8648d-123">Vynucené tunelování v Azure je nakonfigurován pomocí virtuální síti trasy definované uživatelem.</span><span class="sxs-lookup"><span data-stu-id="8648d-123">Forced tunneling in Azure is configured via virtual network user-defined routes.</span></span> <span data-ttu-id="8648d-124">Přesměrování provozu tooan místní lokality je vyjádřeno jako brána Azure VPN toohello výchozí trasu.</span><span class="sxs-lookup"><span data-stu-id="8648d-124">Redirecting traffic tooan on-premises site is expressed as a Default Route toohello Azure VPN gateway.</span></span> <span data-ttu-id="8648d-125">Další informace o směrování definovaného uživatelem a virtuální sítě najdete v tématu [trasy definované uživatelem a předávání IP](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8648d-125">For more information about user-defined routing and virtual networks, see [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>

* <span data-ttu-id="8648d-126">Každá podsíť virtuální sítě má integrovanou, směrovací tabulky systému.</span><span class="sxs-lookup"><span data-stu-id="8648d-126">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="8648d-127">Hello systémovou tabulku směrování má hello následující tři skupiny tras:</span><span class="sxs-lookup"><span data-stu-id="8648d-127">hello system routing table has hello following three groups of routes:</span></span>
  
  * <span data-ttu-id="8648d-128">**Místní virtuální síť trasy:** přímo toohello cílové virtuální počítače v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="8648d-128">**Local VNet routes:** Directly toohello destination VMs in hello same virtual network.</span></span>
  * <span data-ttu-id="8648d-129">**Místní trasy:** toohello Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="8648d-129">**On-premises routes:** toohello Azure VPN gateway.</span></span>
  * <span data-ttu-id="8648d-130">**Výchozí trasu:** přímo toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="8648d-130">**Default route:** Directly toohello Internet.</span></span> <span data-ttu-id="8648d-131">Jsou pakety určené toohello soukromé IP adresy není pokrytá hello předchozí dva trasy vyřadit.</span><span class="sxs-lookup"><span data-stu-id="8648d-131">Packets destined toohello private IP addresses not covered by hello previous two routes are dropped.</span></span>
* <span data-ttu-id="8648d-132">Tento postup používá trasy definované uživatelem (UDR) toocreate směrovací tabulky tooadd výchozí trasu a pak přidružit hello směrování tabulky tooyour VNet podsítí tooenable vynucené tunelování na těchto podsítí.</span><span class="sxs-lookup"><span data-stu-id="8648d-132">This procedure uses user-defined routes (UDR) toocreate a routing table tooadd a default route, and then associate hello routing table tooyour VNet subnet(s) tooenable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="8648d-133">Vynucené tunelování musí být přidružený virtuální síť, která má brána sítě VPN založené na trasách.</span><span class="sxs-lookup"><span data-stu-id="8648d-133">Forced tunneling must be associated with a VNet that has a route-based VPN gateway.</span></span> <span data-ttu-id="8648d-134">Je nutné tooset "výchozí web" mezi hello mezi různými místy místní lokality připojené toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8648d-134">You need tooset a "default site" among hello cross-premises local sites connected toohello virtual network.</span></span> <span data-ttu-id="8648d-135">Navíc hello místního zařízení VPN musí být nakonfigurovaný pomocí 0.0.0.0/0 jako provoz selektorů.</span><span class="sxs-lookup"><span data-stu-id="8648d-135">Also, hello on-premises VPN device must be configured using 0.0.0.0/0 as traffic selectors.</span></span> 
* <span data-ttu-id="8648d-136">ExpressRoute vynuceného tunelování přes tento mechanismus není nakonfigurovaná, ale místo toho je ve inzeruje výchozí trasu přes hello ExpressRoute BGP povolený pro relace partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="8648d-136">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via hello ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="8648d-137">Další informace najdete v tématu hello [dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).</span><span class="sxs-lookup"><span data-stu-id="8648d-137">For more information, see hello [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/).</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="8648d-138">Přehled konfigurace</span><span class="sxs-lookup"><span data-stu-id="8648d-138">Configuration overview</span></span>

<span data-ttu-id="8648d-139">Hello následující postup vám pomůže vytvořit skupinu prostředků a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8648d-139">hello following procedure helps you create a resource group and a VNet.</span></span> <span data-ttu-id="8648d-140">Potom můžete vytvořit bránu sítě VPN a nakonfigurujte vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="8648d-140">You'll then create a VPN gateway and configure forced tunneling.</span></span> <span data-ttu-id="8648d-141">V tomto postupu hello virtuální sítě "MultiTier-VNet, má tři podsítě:"Frontend","Midtier"a"Backend", s čtyři mezi různými místy připojení: 'DefaultSiteHQ' a tři větve.</span><span class="sxs-lookup"><span data-stu-id="8648d-141">In this procedure, hello virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend', with four cross-premises connections: 'DefaultSiteHQ', and three Branches.</span></span>

<span data-ttu-id="8648d-142">kroky postupu Hello nastavit hello 'DefaultSiteHQ', protože připojení site hello výchozí pro vynuceného tunelování a konfiguraci hello 'Midtier' a 'Back-end' podsítě toouse vynuceného tunelování.</span><span class="sxs-lookup"><span data-stu-id="8648d-142">hello procedure steps set hello 'DefaultSiteHQ' as hello default site connection for forced tunneling, and configure hello 'Midtier' and 'Backend' subnets toouse forced tunneling.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="8648d-143">Než začnete</span><span class="sxs-lookup"><span data-stu-id="8648d-143">Before you begin</span></span>

<span data-ttu-id="8648d-144">Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-144">Install hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="8648d-145">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-145">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

### <a name="toolog-in"></a><span data-ttu-id="8648d-146">toolog v</span><span class="sxs-lookup"><span data-stu-id="8648d-146">toolog in</span></span>

[!INCLUDE [toolog in](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="configure-forced-tunneling"></a><span data-ttu-id="8648d-147">Konfigurace vynuceného tunelování</span><span class="sxs-lookup"><span data-stu-id="8648d-147">Configure forced tunneling</span></span>

1. <span data-ttu-id="8648d-148">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8648d-148">Create a resource group.</span></span>

  ```powershell
  New-AzureRmResourceGroup -Name 'ForcedTunneling' -Location 'North Europe'
  ```
2. <span data-ttu-id="8648d-149">Vytvoření virtuální sítě a určit podsítě.</span><span class="sxs-lookup"><span data-stu-id="8648d-149">Create a virtual network and specify subnets.</span></span>

  ```powershell 
  $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
  $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
  $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
  $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
  $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4
  ```
3. <span data-ttu-id="8648d-150">Vytvořte hello brány místní sítě.</span><span class="sxs-lookup"><span data-stu-id="8648d-150">Create hello local network gateways.</span></span>

  ```powershell
  $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
  $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
  $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
  $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
  ```
4. <span data-ttu-id="8648d-151">Vytvořte pravidlo tabulky a směrování trasy hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-151">Create hello route table and route rule.</span></span>

  ```powershell
  New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
  $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
  Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
  Set-AzureRmRouteTable -RouteTable $rt
  ```
5. <span data-ttu-id="8648d-152">Přidružte hello trasy tabulky toohello Midtier a back-end podsítě.</span><span class="sxs-lookup"><span data-stu-id="8648d-152">Associate hello route table toohello Midtier and Backend subnets.</span></span>

  ```powershell
  $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
  Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```
6. <span data-ttu-id="8648d-153">Vytvořte hello brány s výchozí web.</span><span class="sxs-lookup"><span data-stu-id="8648d-153">Create hello Gateway with a default site.</span></span> <span data-ttu-id="8648d-154">Tento krok trvá některé toocomplete čas, někdy 45 minut nebo déle, protože jsou vytvoření a konfiguraci brány hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-154">This step takes some time toocomplete, sometimes 45 minutes or more, because you are creating and configuring hello gateway.</span></span><br> <span data-ttu-id="8648d-155">Hello **- GatewayDefaultSite** je hello parametr rutiny, která umožňuje hello vynutit směrování toowork konfigurace, takže trvat pozor tooconfigure toto nastavení správně.</span><span class="sxs-lookup"><span data-stu-id="8648d-155">hello **-GatewayDefaultSite** is hello cmdlet parameter that allows hello forced routing configuration toowork, so take care tooconfigure this setting properly.</span></span> <span data-ttu-id="8648d-156">Tento parametr je k dispozici v prostředí PowerShell 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="8648d-156">This parameter is available in PowerShell 1.0 or later.</span></span>

  ```powershell
  $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
  $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
  $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
  New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1 -GatewayDefaultSite $lng1 -EnableBgp $false
  ```
7. <span data-ttu-id="8648d-157">Navázání připojení VPN typu Site-to-Site hello.</span><span class="sxs-lookup"><span data-stu-id="8648d-157">Establish hello Site-to-Site VPN connections.</span></span>

  ```powershell
  $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
  $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
  $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
  $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
  $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 
    
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
  New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"
    
  Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
  ```