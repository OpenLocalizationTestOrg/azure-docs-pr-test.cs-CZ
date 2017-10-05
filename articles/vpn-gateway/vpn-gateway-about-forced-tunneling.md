---
title: "Nakonfigurujte vynucené tunelování pro připojení Azure Site-to-Site: classic | Microsoft Docs"
description: "Postup přesměrování nebo \"Vynutit\" veškerý provoz vázaný na Internet na vaše místní umístění."
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
ms.openlocfilehash: 79bf6892c823da282c3e763921e830f986419854
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a><span data-ttu-id="3f77c-103">Konfigurace vynuceného tunelování pomocí modelu nasazení Classic</span><span class="sxs-lookup"><span data-stu-id="3f77c-103">Configure forced tunneling using the classic deployment model</span></span>

<span data-ttu-id="3f77c-104">Vynucené tunelování vám umožní přesměrování nebo "Vynutit" veškerý provoz vázaný na Internet zpět na místní umístění prostřednictvím tunelu Site-to-Site VPN pro kontrolu a auditování.</span><span class="sxs-lookup"><span data-stu-id="3f77c-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="3f77c-105">Požadavek kritické zabezpečení pro většinu organizace IT zásad.</span><span class="sxs-lookup"><span data-stu-id="3f77c-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="3f77c-106">Bez vynucené tunelování, se internetový provoz z virtuálních počítačů v Azure procházení od Azure síťové infrastruktury přímo se k Internetu, bez možnosti a umožní vám na svoji provoz vždy.</span><span class="sxs-lookup"><span data-stu-id="3f77c-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="3f77c-107">Neoprávněný přístup k Internetu může potenciálně vést k informacím nebo jiné typy narušení zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="3f77c-107">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="3f77c-108">Tento článek vás provede procesem konfigurace vynucené tunelování pro virtuální sítě vytvořené pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="3f77c-108">This article walks you through configuring forced tunneling for virtual networks created using the classic deployment model.</span></span> <span data-ttu-id="3f77c-109">Vynucené tunelování se dá konfigurovat pomocí prostředí PowerShell, ne prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="3f77c-109">Forced tunneling can be configured by using PowerShell, not through the portal.</span></span> <span data-ttu-id="3f77c-110">Pokud chcete konfigurovat vynucené tunelování pro model nasazení Resource Manager, vyberte z rozevíracího seznamu následující klasické článku:</span><span class="sxs-lookup"><span data-stu-id="3f77c-110">If you want to configure forced tunneling for the Resource Manager deployment model, select classic article from the following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f77c-111">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="3f77c-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="3f77c-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3f77c-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a><span data-ttu-id="3f77c-113">Požadavky a důležité informace</span><span class="sxs-lookup"><span data-stu-id="3f77c-113">Requirements and considerations</span></span>
<span data-ttu-id="3f77c-114">Vynucené tunelování v Azure je nakonfigurován pomocí virtuální síti trasy definované uživatelem (UDR).</span><span class="sxs-lookup"><span data-stu-id="3f77c-114">Forced tunneling in Azure is configured via virtual network user-defined routes (UDR).</span></span> <span data-ttu-id="3f77c-115">Přesměrování přenosů na místní web je vyjádřen jako výchozí směrování k bráně Azure VPN.</span><span class="sxs-lookup"><span data-stu-id="3f77c-115">Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway.</span></span> <span data-ttu-id="3f77c-116">V následující části jsou uvedené aktuální omezení směrovací tabulku a trasy pro virtuální síť Azure:</span><span class="sxs-lookup"><span data-stu-id="3f77c-116">The following section lists the current limitation of the routing table and routes for an Azure Virtual Network:</span></span>

* <span data-ttu-id="3f77c-117">Každá podsíť virtuální sítě má integrovanou, směrovací tabulky systému.</span><span class="sxs-lookup"><span data-stu-id="3f77c-117">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="3f77c-118">Systémovou tabulku směrování má tyto tři skupiny tras:</span><span class="sxs-lookup"><span data-stu-id="3f77c-118">The system routing table has the following three groups of routes:</span></span>

  * <span data-ttu-id="3f77c-119">**Místní virtuální síť trasy:** přímo do cílového umístění virtuálních počítačů ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="3f77c-119">**Local VNet routes:** Directly to the destination VMs in the same virtual network.</span></span>
  * <span data-ttu-id="3f77c-120">**Místní trasy:** k Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="3f77c-120">**On-premises routes:** To the Azure VPN gateway.</span></span>
  * <span data-ttu-id="3f77c-121">**Výchozí trasu:** přímo k Internetu.</span><span class="sxs-lookup"><span data-stu-id="3f77c-121">**Default route:** Directly to the Internet.</span></span> <span data-ttu-id="3f77c-122">Dojde ke ztrátě paketů určené na privátní IP adresy, které nejsou pokryty předchozí dva trasy.</span><span class="sxs-lookup"><span data-stu-id="3f77c-122">Packets destined to the private IP addresses not covered by the previous two routes will be dropped.</span></span>
* <span data-ttu-id="3f77c-123">S vydáním trasy definované uživatelem můžete vytvořit směrovací tabulku, která chcete přidat výchozí trasu a pak přidružit do směrovací tabulky pro vaši virtuální síť podsítí povolit vynucené tunelování na těchto podsítí.</span><span class="sxs-lookup"><span data-stu-id="3f77c-123">With the release of user-defined routes, you can create a routing table to add a default route, and then associate the routing table to your VNet subnet(s) to enable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="3f77c-124">Budete muset nastavit "výchozí web" mezi místní lokality mezi různými místy připojený k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="3f77c-124">You need to set a "default site" among the cross-premises local sites connected to the virtual network.</span></span>
* <span data-ttu-id="3f77c-125">Vynucené tunelování musí být přidruženy k virtuální síti, která má dynamické směrování brána sítě VPN (není statická brána).</span><span class="sxs-lookup"><span data-stu-id="3f77c-125">Forced tunneling must be associated with a VNet that has a dynamic routing VPN gateway (not a static gateway).</span></span>
* <span data-ttu-id="3f77c-126">ExpressRoute vynuceného tunelování přes tento mechanismus není nakonfigurovaná, ale místo toho je ve inzeruje výchozí trasu prostřednictvím relace partnerského vztahu ExpressRoute BGP povolen.</span><span class="sxs-lookup"><span data-stu-id="3f77c-126">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="3f77c-127">Podrobnosti najdete [dokumentace ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3f77c-127">Please see the [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="3f77c-128">Přehled konfigurace</span><span class="sxs-lookup"><span data-stu-id="3f77c-128">Configuration overview</span></span>
<span data-ttu-id="3f77c-129">V následujícím příkladu tunelovým propojením front-endu podsíť není vynutit.</span><span class="sxs-lookup"><span data-stu-id="3f77c-129">In the following example, the Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="3f77c-130">Úlohy v podsíť Frontend můžete nadále přijímat a reagovat na požadavky zákazníků z Internetu přímo.</span><span class="sxs-lookup"><span data-stu-id="3f77c-130">The workloads in the Frontend subnet can continue to accept and respond to customer requests from the Internet directly.</span></span> <span data-ttu-id="3f77c-131">Střední vrstvě a back-end podsítě, vynuceně přesunuty tunelového propojení.</span><span class="sxs-lookup"><span data-stu-id="3f77c-131">The Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="3f77c-132">Odchozí připojení k Internetu tyto dvě podsítě bude vynutit nebo přesměrován zpět na místní web prostřednictvím jednoho tunelu S2S VPN.</span><span class="sxs-lookup"><span data-stu-id="3f77c-132">Any outbound connections from these two subnets to the Internet will be forced or redirected back to an on-premises site via one of the S2S VPN tunnels.</span></span>

<span data-ttu-id="3f77c-133">To vám umožňuje omezit a kontrolovat přístup k Internetu z virtuálních počítačů nebo cloudových služeb v Azure, můžete nadále povolit vaší architektury víceúrovňová služba vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="3f77c-133">This allows you to restrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing to enable your multi-tier service architecture required.</span></span> <span data-ttu-id="3f77c-134">Také můžete použít vynucené tunelování na celý virtuální sítě Pokud nejsou žádná internetového zatížení ve virtuálních sítích.</span><span class="sxs-lookup"><span data-stu-id="3f77c-134">You also can apply forced tunneling to the entire virtual networks if there are no Internet-facing workloads in your virtual networks.</span></span>

![Vynucené tunelování](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a><span data-ttu-id="3f77c-136">Než začnete</span><span class="sxs-lookup"><span data-stu-id="3f77c-136">Before you begin</span></span>
<span data-ttu-id="3f77c-137">Před zahájením konfigurace ověřte, zda máte následující.</span><span class="sxs-lookup"><span data-stu-id="3f77c-137">Verify that you have the following items before beginning configuration.</span></span>

* <span data-ttu-id="3f77c-138">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="3f77c-138">An Azure subscription.</span></span> <span data-ttu-id="3f77c-139">Pokud ještě nemáte předplatné Azure, můžete si aktivovat [výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f77c-139">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3f77c-140">Nakonfigurované virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3f77c-140">A configured virtual network.</span></span> 
* <span data-ttu-id="3f77c-141">Nejnovější verzi rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3f77c-141">The latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="3f77c-142">Další informace o instalaci rutin prostředí PowerShell najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3f77c-142">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

## <a name="configure-forced-tunneling"></a><span data-ttu-id="3f77c-143">Konfigurace vynuceného tunelování</span><span class="sxs-lookup"><span data-stu-id="3f77c-143">Configure forced tunneling</span></span>
<span data-ttu-id="3f77c-144">Následující postup vám pomůže určit vynucené tunelování pro virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="3f77c-144">The following procedure will help you specify forced tunneling for a virtual network.</span></span> <span data-ttu-id="3f77c-145">Postup konfigurace odpovídají konfiguračního souboru sítě VNet.</span><span class="sxs-lookup"><span data-stu-id="3f77c-145">The configuration steps correspond to the VNet network configuration file.</span></span>

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

<span data-ttu-id="3f77c-146">V tomto příkladu virtuální sítě "MultiTier-VNet, má tři podsítě: 'Front-endu', 'Midtier' a 'Back-end' podsítě s připojeními čtyři mezi více místy: 'DefaultSiteHQ' a tři větve.</span><span class="sxs-lookup"><span data-stu-id="3f77c-146">In this example, the virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend' subnets, with four cross premises connections: 'DefaultSiteHQ', and three Branches.</span></span> 

<span data-ttu-id="3f77c-147">Kroky DefaultSiteHQ nastavit jako výchozí připojení lokality pro vynuceného tunelování a nakonfigurovat Midtier a back-end podsítě používat vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="3f77c-147">The steps will set the 'DefaultSiteHQ' as the default site connection for forced tunneling, and configure the Midtier and Backend subnets to use forced tunneling.</span></span>

1. <span data-ttu-id="3f77c-148">Umožňuje vytvořte směrovací tabulku.</span><span class="sxs-lookup"><span data-stu-id="3f77c-148">Create a routing table.</span></span> <span data-ttu-id="3f77c-149">Chcete-li vytvořit směrovací tabulku použijte následující rutinu.</span><span class="sxs-lookup"><span data-stu-id="3f77c-149">Use the following cmdlet to create your route table.</span></span>

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. <span data-ttu-id="3f77c-150">Přidejte výchozí trasy do směrovací tabulky.</span><span class="sxs-lookup"><span data-stu-id="3f77c-150">Add a default route to the routing table.</span></span> 

  <span data-ttu-id="3f77c-151">Následující příklad přidá výchozí trasy do směrovací tabulky vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="3f77c-151">The following example adds a default route to the routing table created in Step 1.</span></span> <span data-ttu-id="3f77c-152">Všimněte si, že je podporována pouze trasy, která je předpona cílové "0.0.0.0/0" na "Brána VPN" dalšího segmentu.</span><span class="sxs-lookup"><span data-stu-id="3f77c-152">Note that the only route supported is the destination prefix of "0.0.0.0/0" to the "VPNGateway" NextHop.</span></span>

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. <span data-ttu-id="3f77c-153">Přidružte do směrovací tabulky k podsítím.</span><span class="sxs-lookup"><span data-stu-id="3f77c-153">Associate the routing table to the subnets.</span></span> 

  <span data-ttu-id="3f77c-154">Po vytvoření směrovací tabulku a přidat trasu, použijte následující příklad pro přidání nebo přidružení tabulku směrování pro podsíť virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3f77c-154">After a routing table is created and a route added, use the following example to add or associate the route table to a VNet subnet.</span></span> <span data-ttu-id="3f77c-155">V příkladu přidá směrovací tabulka "MyRouteTable" do podsítě virtuální sítě MultiTier-VNet Midtier a back-end.</span><span class="sxs-lookup"><span data-stu-id="3f77c-155">The example adds the route table "MyRouteTable" to the Midtier and Backend subnets of VNet MultiTier-VNet.</span></span>

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. <span data-ttu-id="3f77c-156">Přiřadíte výchozí web pro vynucené tunelování.</span><span class="sxs-lookup"><span data-stu-id="3f77c-156">Assign a default site for forced tunneling.</span></span> 

  <span data-ttu-id="3f77c-157">V předchozím kroku ukázkové skripty rutiny vytvořit směrovací tabulky a související směrovací tabulka ke dvěma z podsítí virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3f77c-157">In the preceding step, the sample cmdlet scripts created the routing table and associated the route table to two of the VNet subnets.</span></span> <span data-ttu-id="3f77c-158">Zbývající krokem je vybrat jako výchozí web nebo tunelové propojení místní lokality mezi více lokalit připojení virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="3f77c-158">The remaining step is to select a local site among the multi-site connections of the virtual network as the default site or tunnel.</span></span>

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a><span data-ttu-id="3f77c-159">Další rutiny prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f77c-159">Additional PowerShell cmdlets</span></span>
### <a name="to-delete-a-route-table"></a><span data-ttu-id="3f77c-160">Chcete-li odstranit tabulku směrování</span><span class="sxs-lookup"><span data-stu-id="3f77c-160">To delete a route table</span></span>

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a><span data-ttu-id="3f77c-161">Do seznamu směrovací tabulku</span><span class="sxs-lookup"><span data-stu-id="3f77c-161">To list a route table</span></span>

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a><span data-ttu-id="3f77c-162">Chcete-li odstranit trasy z tabulky trasy</span><span class="sxs-lookup"><span data-stu-id="3f77c-162">To delete a route from a route table</span></span>

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a><span data-ttu-id="3f77c-163">Chcete-li odebrat trasu z podsítě</span><span class="sxs-lookup"><span data-stu-id="3f77c-163">To remove a route from a subnet</span></span>

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a><span data-ttu-id="3f77c-164">Seznam směrovací tabulka spojené s podsítí</span><span class="sxs-lookup"><span data-stu-id="3f77c-164">To list the route table associated with a subnet</span></span>

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a><span data-ttu-id="3f77c-165">Odebrání výchozí web brány virtuální sítě VPN</span><span class="sxs-lookup"><span data-stu-id="3f77c-165">To remove a default site from a VNet VPN gateway</span></span>

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```