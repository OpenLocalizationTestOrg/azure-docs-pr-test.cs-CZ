---
title: "Nakonfigurujte vynucené tunelování pro připojení Azure Site-to-Site: classic | Microsoft Docs"
description: "Jak tooredirect nebo \"Vynutit\" všechny internetový provoz back tooyour místní umístění."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 35b3a9ea370f9f962572629a69cc9aed16a87837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-classic-deployment-model"></a><span data-ttu-id="4b319-103">Nakonfigurujte vynucené tunelování, pomocí modelu nasazení classic hello</span><span class="sxs-lookup"><span data-stu-id="4b319-103">Configure forced tunneling using hello classic deployment model</span></span>

<span data-ttu-id="4b319-104">Vynutit tunelové propojení umožňuje přesměrování nebo "Vynutit" všechny internetový provoz back tooyour místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování.</span><span class="sxs-lookup"><span data-stu-id="4b319-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="4b319-105">Požadavek kritické zabezpečení pro většinu organizace IT zásad.</span><span class="sxs-lookup"><span data-stu-id="4b319-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="4b319-106">Bez vynucené tunelování, internetový provoz z virtuálních počítačů v Azure bude vždy procházení od Azure síťové infrastruktury přímo na Internetu, toohello bez tooallow možnost hello je provoz hello tooinspect nebo kontrola.</span><span class="sxs-lookup"><span data-stu-id="4b319-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="4b319-107">Neoprávněný přístup k Internetu může potenciálně vést tooinformation zpřístupnění nebo jiné typy narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="4b319-107">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="4b319-108">Tento článek vás provede procesem konfigurace vynucené tunelování pro virtuální sítě vytvořené pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-108">This article walks you through configuring forced tunneling for virtual networks created using hello classic deployment model.</span></span> <span data-ttu-id="4b319-109">Vynucené tunelování se dá konfigurovat pomocí prostředí PowerShell, ne přes portál hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-109">Forced tunneling can be configured by using PowerShell, not through hello portal.</span></span> <span data-ttu-id="4b319-110">Pokud chcete tooconfigure vynuceného tunelování pro model nasazení Resource Manager hello, vyberte classic článek z hello následující rozevíracího seznamu:</span><span class="sxs-lookup"><span data-stu-id="4b319-110">If you want tooconfigure forced tunneling for hello Resource Manager deployment model, select classic article from hello following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b319-111">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="4b319-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="4b319-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4b319-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a><span data-ttu-id="4b319-113">Požadavky a důležité informace</span><span class="sxs-lookup"><span data-stu-id="4b319-113">Requirements and considerations</span></span>
<span data-ttu-id="4b319-114">Vynucené tunelování v Azure je nakonfigurován pomocí virtuální síti trasy definované uživatelem (UDR).</span><span class="sxs-lookup"><span data-stu-id="4b319-114">Forced tunneling in Azure is configured via virtual network user-defined routes (UDR).</span></span> <span data-ttu-id="4b319-115">Přesměrování provozu tooan místní lokality je vyjádřeno jako brána Azure VPN toohello výchozí trasu.</span><span class="sxs-lookup"><span data-stu-id="4b319-115">Redirecting traffic tooan on-premises site is expressed as a Default Route toohello Azure VPN gateway.</span></span> <span data-ttu-id="4b319-116">Hello následující části jsou uvedeny hello aktuální omezení trasy a hello směrovací tabulky pro virtuální síť Azure:</span><span class="sxs-lookup"><span data-stu-id="4b319-116">hello following section lists hello current limitation of hello routing table and routes for an Azure Virtual Network:</span></span>

* <span data-ttu-id="4b319-117">Každá podsíť virtuální sítě má integrovanou, směrovací tabulky systému.</span><span class="sxs-lookup"><span data-stu-id="4b319-117">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="4b319-118">Hello systémovou tabulku směrování má hello následující tři skupiny tras:</span><span class="sxs-lookup"><span data-stu-id="4b319-118">hello system routing table has hello following three groups of routes:</span></span>

  * <span data-ttu-id="4b319-119">**Místní virtuální síť trasy:** přímo toohello cílové virtuální počítače v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="4b319-119">**Local VNet routes:** Directly toohello destination VMs in hello same virtual network.</span></span>
  * <span data-ttu-id="4b319-120">**Místní trasy:** toohello Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="4b319-120">**On-premises routes:** toohello Azure VPN gateway.</span></span>
  * <span data-ttu-id="4b319-121">**Výchozí trasu:** přímo toohello Internetu.</span><span class="sxs-lookup"><span data-stu-id="4b319-121">**Default route:** Directly toohello Internet.</span></span> <span data-ttu-id="4b319-122">Pakety určené toohello soukromé IP adresy není pokrytá hello předchozí dva trasy se zahodí.</span><span class="sxs-lookup"><span data-stu-id="4b319-122">Packets destined toohello private IP addresses not covered by hello previous two routes will be dropped.</span></span>
* <span data-ttu-id="4b319-123">S vydáním hello trasy definované uživatelem můžete vytvořit směrovací tabulku tooadd výchozí trasu a pak přidružit hello směrovací tabulky tooyour virtuální síť podsítí tooenable vynucené tunelování na těchto podsítí.</span><span class="sxs-lookup"><span data-stu-id="4b319-123">With hello release of user-defined routes, you can create a routing table tooadd a default route, and then associate hello routing table tooyour VNet subnet(s) tooenable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="4b319-124">Je nutné tooset "výchozí web" mezi hello mezi různými místy místní lokality připojené toohello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4b319-124">You need tooset a "default site" among hello cross-premises local sites connected toohello virtual network.</span></span>
* <span data-ttu-id="4b319-125">Vynucené tunelování musí být přidruženy k virtuální síti, která má dynamické směrování brána sítě VPN (není statická brána).</span><span class="sxs-lookup"><span data-stu-id="4b319-125">Forced tunneling must be associated with a VNet that has a dynamic routing VPN gateway (not a static gateway).</span></span>
* <span data-ttu-id="4b319-126">ExpressRoute vynuceného tunelování přes tento mechanismus není nakonfigurovaná, ale místo toho je ve inzeruje výchozí trasu přes hello ExpressRoute BGP povolený pro relace partnerského vztahu.</span><span class="sxs-lookup"><span data-stu-id="4b319-126">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via hello ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="4b319-127">Najdete v tématu hello [dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4b319-127">Please see hello [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="4b319-128">Přehled konfigurace</span><span class="sxs-lookup"><span data-stu-id="4b319-128">Configuration overview</span></span>
<span data-ttu-id="4b319-129">V následujícím příkladu hello tunelovým propojením hello front-endu podsíť není vynutit.</span><span class="sxs-lookup"><span data-stu-id="4b319-129">In hello following example, hello Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="4b319-130">Hello úlohy v podsíť Frontend hello můžete i nadále tooaccept a reagovat toocustomer požadavky z hello Internet přímo.</span><span class="sxs-lookup"><span data-stu-id="4b319-130">hello workloads in hello Frontend subnet can continue tooaccept and respond toocustomer requests from hello Internet directly.</span></span> <span data-ttu-id="4b319-131">Hello střední vrstvě a back-end podsítě, vynuceně přesunuty tunelového propojení.</span><span class="sxs-lookup"><span data-stu-id="4b319-131">hello Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="4b319-132">Všechny odchozí připojení z těchto dvou podsítí toohello Internetu bude tooan vynucené nebo přesměrované zpět na místní lokalitu prostřednictvím jednoho hello tunelovými propojeními S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="4b319-132">Any outbound connections from these two subnets toohello Internet will be forced or redirected back tooan on-premises site via one of hello S2S VPN tunnels.</span></span>

<span data-ttu-id="4b319-133">To vám umožní toorestrict a kontrolovat přístup k Internetu z virtuálních počítačů nebo cloudových služeb v Azure, a přitom dál tooenable vaší architektury víceúrovňová služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="4b319-133">This allows you toorestrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing tooenable your multi-tier service architecture required.</span></span> <span data-ttu-id="4b319-134">Také můžete použít vynucené tunelování toohello celý virtuální sítě Pokud nejsou žádná internetového zatížení ve virtuálních sítích.</span><span class="sxs-lookup"><span data-stu-id="4b319-134">You also can apply forced tunneling toohello entire virtual networks if there are no Internet-facing workloads in your virtual networks.</span></span>

![Vynucené tunelování](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a><span data-ttu-id="4b319-136">Než začnete</span><span class="sxs-lookup"><span data-stu-id="4b319-136">Before you begin</span></span>
<span data-ttu-id="4b319-137">Ověřte, zda máte následující položky před počáteční konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-137">Verify that you have hello following items before beginning configuration.</span></span>

* <span data-ttu-id="4b319-138">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4b319-138">An Azure subscription.</span></span> <span data-ttu-id="4b319-139">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4b319-139">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4b319-140">Nakonfigurované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4b319-140">A configured virtual network.</span></span> 
* <span data-ttu-id="4b319-141">Hello nejnovější verze hello rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4b319-141">hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="4b319-142">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) Další informace o instalaci rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="4b319-142">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

## <a name="configure-forced-tunneling"></a><span data-ttu-id="4b319-143">Konfigurace vynuceného tunelování</span><span class="sxs-lookup"><span data-stu-id="4b319-143">Configure forced tunneling</span></span>
<span data-ttu-id="4b319-144">Hello následující postup vám pomůže určit vynucené tunelování pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="4b319-144">hello following procedure will help you specify forced tunneling for a virtual network.</span></span> <span data-ttu-id="4b319-145">Postup konfigurace Hello odpovídají toohello virtuální síť sítě konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="4b319-145">hello configuration steps correspond toohello VNet network configuration file.</span></span>

```
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

<span data-ttu-id="4b319-146">V tomto příkladu hello virtuální sítě "MultiTier-VNet, má tři podsítě: 'Front-endu', 'Midtier' a 'Back-end' podsítě s připojeními čtyři mezi více místy: 'DefaultSiteHQ' a tři větve.</span><span class="sxs-lookup"><span data-stu-id="4b319-146">In this example, hello virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend' subnets, with four cross premises connections: 'DefaultSiteHQ', and three Branches.</span></span> 

<span data-ttu-id="4b319-147">kroky Hello se nastavit hello 'DefaultSiteHQ' jako hello výchozí lokality připojení pro vynucené tunelování a nakonfigurujte hello Midtier a back-end podsítě toouse vynuceného tunelování.</span><span class="sxs-lookup"><span data-stu-id="4b319-147">hello steps will set hello 'DefaultSiteHQ' as hello default site connection for forced tunneling, and configure hello Midtier and Backend subnets toouse forced tunneling.</span></span>

1. <span data-ttu-id="4b319-148">Umožňuje vytvořte směrovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="4b319-148">Create a routing table.</span></span> <span data-ttu-id="4b319-149">Pomocí následující rutiny toocreate hello směrovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="4b319-149">Use hello following cmdlet toocreate your route table.</span></span>

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. <span data-ttu-id="4b319-150">Přidejte výchozí směrovací toohello směrovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="4b319-150">Add a default route toohello routing table.</span></span> 

  <span data-ttu-id="4b319-151">Hello následující příklad přidá výchozí trasy toohello směrovací tabulku vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="4b319-151">hello following example adds a default route toohello routing table created in Step 1.</span></span> <span data-ttu-id="4b319-152">Všimněte si, že hello pouze trasy podporována je předpona cílové hello "0.0.0.0/0" toohello "Brána VPN" dalšího segmentu.</span><span class="sxs-lookup"><span data-stu-id="4b319-152">Note that hello only route supported is hello destination prefix of "0.0.0.0/0" toohello "VPNGateway" NextHop.</span></span>

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. <span data-ttu-id="4b319-153">Přidružte hello směrovací tabulky toohello podsítě.</span><span class="sxs-lookup"><span data-stu-id="4b319-153">Associate hello routing table toohello subnets.</span></span> 

  <span data-ttu-id="4b319-154">Po vytvoření směrovací tabulku a přidat trasu, použijte následující příklad tooadd hello nebo přidružit hello trasy tabulky tooa VNet podsíť.</span><span class="sxs-lookup"><span data-stu-id="4b319-154">After a routing table is created and a route added, use hello following example tooadd or associate hello route table tooa VNet subnet.</span></span> <span data-ttu-id="4b319-155">Příklad Hello přidá hello trasy tabulky "MyRouteTable" toohello Midtier a back-end podsítě virtuální sítě MultiTier-VNet.</span><span class="sxs-lookup"><span data-stu-id="4b319-155">hello example adds hello route table "MyRouteTable" toohello Midtier and Backend subnets of VNet MultiTier-VNet.</span></span>

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. <span data-ttu-id="4b319-156">Přiřadíte výchozí web pro vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="4b319-156">Assign a default site for forced tunneling.</span></span> 

  <span data-ttu-id="4b319-157">V předchozím kroku hello hello ukázkové rutiny skripty vytvořili hello směrovací tabulky a související hello trasy tabulky tootwo hello podsítí virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4b319-157">In hello preceding step, hello sample cmdlet scripts created hello routing table and associated hello route table tootwo of hello VNet subnets.</span></span> <span data-ttu-id="4b319-158">zbývající krok Hello je tooselect místní sítě mezi připojení více lokalit hello hello jako výchozí web hello nebo tunelové propojení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="4b319-158">hello remaining step is tooselect a local site among hello multi-site connections of hello virtual network as hello default site or tunnel.</span></span>

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a><span data-ttu-id="4b319-159">Další rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4b319-159">Additional PowerShell cmdlets</span></span>
### <a name="toodelete-a-route-table"></a><span data-ttu-id="4b319-160">toodelete směrovací tabulku</span><span class="sxs-lookup"><span data-stu-id="4b319-160">toodelete a route table</span></span>

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="toolist-a-route-table"></a><span data-ttu-id="4b319-161">toolist směrovací tabulku</span><span class="sxs-lookup"><span data-stu-id="4b319-161">toolist a route table</span></span>

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="toodelete-a-route-from-a-route-table"></a><span data-ttu-id="4b319-162">toodelete trasy z tabulky trasy</span><span class="sxs-lookup"><span data-stu-id="4b319-162">toodelete a route from a route table</span></span>

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="tooremove-a-route-from-a-subnet"></a><span data-ttu-id="4b319-163">tooremove trasy z podsítě</span><span class="sxs-lookup"><span data-stu-id="4b319-163">tooremove a route from a subnet</span></span>

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="toolist-hello-route-table-associated-with-a-subnet"></a><span data-ttu-id="4b319-164">Tabulka směrování hello toolist spojené s podsítí</span><span class="sxs-lookup"><span data-stu-id="4b319-164">toolist hello route table associated with a subnet</span></span>

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="tooremove-a-default-site-from-a-vnet-vpn-gateway"></a><span data-ttu-id="4b319-165">tooremove výchozí web z brány virtuální sítě VPN</span><span class="sxs-lookup"><span data-stu-id="4b319-165">tooremove a default site from a VNet VPN gateway</span></span>

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```