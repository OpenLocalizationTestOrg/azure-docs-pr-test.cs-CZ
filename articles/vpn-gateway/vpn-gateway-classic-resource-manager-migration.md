---
title: "Z klasického zprostředkovatele brány sítě VPN na při migraci správce prostředků | Microsoft Docs"
description: "Tato stránka obsahuje přehled portálu VPN Gateway Classic migrace správce prostředků."
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: 1164fc24355657af22b6befaad74685ebbc2b5cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="vpn-gateway-classic-to-resource-manager-migration"></a><span data-ttu-id="de278-103">Brána sítě VPN classic migrace správce prostředků</span><span class="sxs-lookup"><span data-stu-id="de278-103">VPN Gateway classic to Resource Manager migration</span></span>
<span data-ttu-id="de278-104">Brány sítě VPN lze nyní přenést z klasického modelu nasazení Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="de278-104">VPN Gateways can now be migrated from classic to Resource Manager deployment model.</span></span> <span data-ttu-id="de278-105">Další informace o službě Správce prostředků Azure [funkce a výhody](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="de278-105">You can read more about Azure Resource Manager [features and benefits](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="de278-106">V tomto článku jsme podrobnosti migrace z klasického nasazení do novější správce prostředků na základě modelu.</span><span class="sxs-lookup"><span data-stu-id="de278-106">In this article, we  detail how to migrate from classic deployments to newer Resource Manager based model.</span></span> 

<span data-ttu-id="de278-107">Brány sítě VPN se migrují jako součást migrace virtuální sítě z classic do Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="de278-107">VPN Gateways are migrated as part of VNet migration from classic to Resource Manager.</span></span> <span data-ttu-id="de278-108">Tato migrace provádí jednu virtuální síť v čase.</span><span class="sxs-lookup"><span data-stu-id="de278-108">This migration is done one VNet at a time.</span></span> <span data-ttu-id="de278-109">Neexistuje žádný další požadavek z hlediska nástrojů nebo požadavky na migraci.</span><span class="sxs-lookup"><span data-stu-id="de278-109">There is no additional requirement in terms of tools or prerequisites to migration.</span></span> <span data-ttu-id="de278-110">Kroky migrace jsou identické existující virtuální síť migrace a jsou popsané v [stránka migrace prostředky IaaS](../virtual-machines/windows/migration-classic-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="de278-110">Migration steps are identical to existing VNet migration and are documented at [IaaS resources migration page](../virtual-machines/windows/migration-classic-resource-manager-ps.md).</span></span> <span data-ttu-id="de278-111">Neexistuje žádné výpadky cesta dat během migrace a proto by existující úlohy nadále fungovat bez ztráty připojení místní během migrace.</span><span class="sxs-lookup"><span data-stu-id="de278-111">There is no data path downtime during migration and thus existing workloads would continue to function without loss of on-premises connectivity during migration.</span></span> <span data-ttu-id="de278-112">Veřejnou IP adresu, které jsou přidružené k bráně VPN nezmění během procesu migrace.</span><span class="sxs-lookup"><span data-stu-id="de278-112">The public IP address associated with the VPN gateway does not change during the migration process.</span></span> <span data-ttu-id="de278-113">To znamená, že nebudete muset znovu nakonfigurovat místní směrovač po dokončení migrace.</span><span class="sxs-lookup"><span data-stu-id="de278-113">This implies that you will not need to reconfigure your on-premises router once the migration is completed.</span></span>  

<span data-ttu-id="de278-114">V modelu ve službě Správce prostředků se liší od klasického modelu a se skládá z brány virtuální sítě, brány místní sítě a prostředky připojení.</span><span class="sxs-lookup"><span data-stu-id="de278-114">The model in Resource Manager is different from classic model and is composed of virtual network gateways, local network gateways and connection resources.</span></span> <span data-ttu-id="de278-115">Tyto představují brány sítě VPN, local-site představující na místní adresní prostor a připojení mezi těmito dvěma v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="de278-115">These represent the VPN gateway itself, the local-site representing on premises address space and connectivity between the two respectively.</span></span> <span data-ttu-id="de278-116">Po dokončení migrace vašich bran nebude k dispozici v klasického modelu a všechny operace správy v brány virtuální sítě, brány místní sítě a objektů připojení musí být provedeno pomocí modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="de278-116">Once migration is completed your gateways would not be available in classic model and all management operations on virtual network gateways, local network gateways, and connection objects must be performed using Resource Manager model.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="de278-117">Podporované scénáře</span><span class="sxs-lookup"><span data-stu-id="de278-117">Supported scenarios</span></span>
<span data-ttu-id="de278-118">Obvyklé scénáře, připojení VPN jsou předmětem classic do Resource Manager migrace.</span><span class="sxs-lookup"><span data-stu-id="de278-118">Most common VPN connectivity scenarios are covered by classic to Resource Manager migration.</span></span> <span data-ttu-id="de278-119">Mezi podporované scénáře patří-</span><span class="sxs-lookup"><span data-stu-id="de278-119">The supported scenarios include -</span></span>

* <span data-ttu-id="de278-120">Přejděte na připojení k webu</span><span class="sxs-lookup"><span data-stu-id="de278-120">Point to site connectivity</span></span>
* <span data-ttu-id="de278-121">Webový server lokality připojení k bráně VPN připojen k na místní umístění</span><span class="sxs-lookup"><span data-stu-id="de278-121">Site to site connectivity with VPN Gateway connected to on premises location</span></span>
* <span data-ttu-id="de278-122">Virtuální síť VNet připojení mezi oběma virtuálními sítěmi pomocí brány sítě VPN</span><span class="sxs-lookup"><span data-stu-id="de278-122">VNet to VNet connectivity between two VNets using VPN gateways</span></span>
* <span data-ttu-id="de278-123">Více virtuálních sítí připojené k stejné na místní umístění</span><span class="sxs-lookup"><span data-stu-id="de278-123">Multiple VNets connected to same on premises location</span></span>
* <span data-ttu-id="de278-124">Připojení více lokalit</span><span class="sxs-lookup"><span data-stu-id="de278-124">Multi-site connectivity</span></span>
* <span data-ttu-id="de278-125">Povolit vynucené tunelování virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="de278-125">Forced tunneling enabled VNets</span></span>

<span data-ttu-id="de278-126">Scénáře, které nejsou podporovány zahrnují-</span><span class="sxs-lookup"><span data-stu-id="de278-126">Scenarios which are not supported include -</span></span>  

* <span data-ttu-id="de278-127">Virtuální síť, bránu ExpressRoute i bránu VPN není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="de278-127">VNet with both ExpressRoute Gateway and VPN Gateway is not currently supported.</span></span>
* <span data-ttu-id="de278-128">Přenosu scénáře, ve kterých jsou rozšíření virtuálního počítače připojené k místní servery.</span><span class="sxs-lookup"><span data-stu-id="de278-128">Transit scenarios where VM extensions are connected to on-premises servers.</span></span> <span data-ttu-id="de278-129">Omezení připojení VPN přenosu jsou podrobně popsány níže.</span><span class="sxs-lookup"><span data-stu-id="de278-129">Transit VPN connectivity limitations are detailed below.</span></span>

> [!NOTE]
> <span data-ttu-id="de278-130">Ověření CIDR v modelu Resource Manager je striktní více než jeden v klasického modelu.</span><span class="sxs-lookup"><span data-stu-id="de278-130">CIDR validation in Resource Manager model is more strict than the one in classic model.</span></span> <span data-ttu-id="de278-131">Před migrací zabezpečovat, že zadané rozsahy classic adres odpovídají platný formát CIDR před zahájením migrace.</span><span class="sxs-lookup"><span data-stu-id="de278-131">Before migrating ensure that classic address ranges given conform to valid CIDR format before beginning the migration.</span></span> <span data-ttu-id="de278-132">CIDR může být ověřen pomocí všechny běžné validátory CIDR.</span><span class="sxs-lookup"><span data-stu-id="de278-132">CIDR can be validated using any common CIDR validators.</span></span> <span data-ttu-id="de278-133">Virtuální síť nebo místní lokality s neplatnou rozsahy CIDR při migraci by způsobilo stavu selhání.</span><span class="sxs-lookup"><span data-stu-id="de278-133">VNet or local sites with invalid CIDR ranges when migrated would result in failed state.</span></span>
> 
> 

## <a name="vnet-to-vnet-connectivity-migration"></a><span data-ttu-id="de278-134">Virtuální síť VNet připojení migrace</span><span class="sxs-lookup"><span data-stu-id="de278-134">VNet to VNet connectivity migration</span></span>
<span data-ttu-id="de278-135">Virtuální síť připojení virtuální síť v klasickém bylo dosaženo vytvořením reprezentace místní lokality připojené virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="de278-135">VNet to VNet connectivity in classic was achieved by creating a local site representation of the connected VNet.</span></span> <span data-ttu-id="de278-136">Zákazníci bylo potřeba vytvořit dva místní weby, které reprezentované dvě virtuální sítě, které se musel být propojeny.</span><span class="sxs-lookup"><span data-stu-id="de278-136">Customers were required to create two local sites which represented the two VNets which needed to be connected together.</span></span> <span data-ttu-id="de278-137">Potom tyto připojilo k odpovídající virtuální sítě pomocí tunelu IPsec k navázání připojení mezi oběma virtuálními sítěmi.</span><span class="sxs-lookup"><span data-stu-id="de278-137">These were then connected to the corresponding VNets using IPsec tunnel to establish connectivity between the two VNets.</span></span> <span data-ttu-id="de278-138">Tento model má problémy spravovatelnosti, protože změny rozsahu adres ve virtuální síti jeden musí lze udržovat také v odpovídající reprezentace místního webu.</span><span class="sxs-lookup"><span data-stu-id="de278-138">This model has manageability challenges since any address range changes in one VNet must also be maintained in the corresponding local site representation.</span></span> <span data-ttu-id="de278-139">V modelu Resource Manager se už nepotřebuje toto řešení.</span><span class="sxs-lookup"><span data-stu-id="de278-139">In Resource Manager model this workaround is no longer needed.</span></span> <span data-ttu-id="de278-140">Připojení mezi oběma virtuálními sítěmi lze přímo dosáhnout pomocí typ připojení: Vnet2Vnet"v prostředku připojení.</span><span class="sxs-lookup"><span data-stu-id="de278-140">The connection between the two VNets can be directly achieved using 'Vnet2Vnet' connection type in Connection resource.</span></span> 

![Snímek obrazovky z virtuální sítě VNet migrace.](./media/vpn-gateway-migration/migration1.png)

<span data-ttu-id="de278-142">Během migrace virtuální sítě zjistíme, že připojené entity, která má aktuální virtuální síť VPN gateway je jiné virtuální síti a ujistěte se, že po dokončení migrace obě virtuální sítě, by se již nezobrazují dva místní weby představující další virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="de278-142">During VNet migration we detect that the connected entity to current VNet's VPN gateway is another VNet and ensure that once migration of both VNets is completed, you would no longer see two local sites representing the other VNet.</span></span> <span data-ttu-id="de278-143">Klasického modelu dvě brány sítě VPN, dva místní weby a dvě připojení mezi nimi transformována do modelu Resource Manager s dvě brány sítě VPN a dvě připojení typu Vnet2Vnet.</span><span class="sxs-lookup"><span data-stu-id="de278-143">The classic model of two VPN gateways, two local sites and two connections between them is transformed to Resource Manager model with two VPN gateways and two connections of type Vnet2Vnet.</span></span>

## <a name="transit-vpn-connectivity"></a><span data-ttu-id="de278-144">Připojení k síti VPN přenosu</span><span class="sxs-lookup"><span data-stu-id="de278-144">Transit VPN connectivity</span></span>
<span data-ttu-id="de278-145">Brány sítě VPN můžete konfigurovat v topologii tak, aby místní připojení pro virtuální síť se dosahuje připojení k jiné virtuální síti, která je přímo připojena k místní.</span><span class="sxs-lookup"><span data-stu-id="de278-145">You can configure VPN gateways in a topology such that on-premises connectivity for a VNet is achieved by connecting to another VNet that is directly connected to on-premises.</span></span> <span data-ttu-id="de278-146">Toto je přenosu připojení VPN, kde instance v první sítě VNet připojeni k místním prostředkům prostřednictvím přenosu ke službě VPN gateway v připojené virtuální sítě, která je přímo připojena k místní.</span><span class="sxs-lookup"><span data-stu-id="de278-146">This is transit VPN connectivity where instances in first VNet are connected to on-premises resources via transit to the VPN gateway in connected VNet that is directly connected to on-premises.</span></span> <span data-ttu-id="de278-147">K dosažení tohoto konfigurace v modelu nasazení classic, museli byste vytvořit místní lokality, který má agregovat předpony představující obou připojené virtuální sítě a místní adresní prostor.</span><span class="sxs-lookup"><span data-stu-id="de278-147">To achieve this configuration in classic deployment model, you would need to create a local site which has aggregated prefixes representing both the connected VNet and on-premises address space.</span></span> <span data-ttu-id="de278-148">Potom tento representational místní síť připojená k virtuální sítě k dosažení připojení přenosu.</span><span class="sxs-lookup"><span data-stu-id="de278-148">This representational local site is then connected to the VNet to achieve transit connectivity.</span></span> <span data-ttu-id="de278-149">Vzhledem k tomu, že všechny změny v místní rozsah adres musí být zachovaná také na webu místní představující agregace virtuální sítě a místní, tento klasického modelu má také podobné možnosti správy problémů.</span><span class="sxs-lookup"><span data-stu-id="de278-149">This classic model also has similar manageability challenges since any change in on-premises address range must also be maintained on the local site representing the aggregate of VNet and on-premises.</span></span> <span data-ttu-id="de278-150">Zavedení podpory protokolu BGP v Resource Manager podporován brány zjednodušuje možnosti správy, protože připojené brány další směrování z místně bez nutnosti ruční úpravy předpon.</span><span class="sxs-lookup"><span data-stu-id="de278-150">Introduction of BGP support in Resource Manager supported gateways simplifies manageability since the connected gateways can learn routes from on premises without manual modification to prefixes.</span></span>

![Snímek obrazovky scénář směrování přenosu.](./media/vpn-gateway-migration/migration2.png)

<span data-ttu-id="de278-152">Vzhledem k tomu, že jsme transformace virtuální síť připojení virtuální sítě bez nutnosti místní weby, ztratí scénář přenosu pro virtuální síť, která je nepřímo připojena k místní místní připojení.</span><span class="sxs-lookup"><span data-stu-id="de278-152">Since we transform VNet to VNet connectivity without requiring local sites, the transit scenario loses on-premises connectivity for VNet that is indirectly connected to on-premises.</span></span> <span data-ttu-id="de278-153">Ztráty připojení, můžete po dokončení migrace – v následujících dvou způsobů, omezeny.</span><span class="sxs-lookup"><span data-stu-id="de278-153">The loss of connectivity can be mitigated in the following two ways, after migration is completed -</span></span> 

* <span data-ttu-id="de278-154">Povolte protokol BGP na VPN brány, které jsou propojeny společně a místně.</span><span class="sxs-lookup"><span data-stu-id="de278-154">Enable BGP on VPN gateways that are connected together and to on-premises.</span></span> <span data-ttu-id="de278-155">Povolení protokolu BGP obnoví připojení bez další změny konfigurace, protože trasy se naučili a ohlášené mezi brány virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="de278-155">Enabling BGP restores connectivity without any other configuration change since routes are learned and advertised between VNet gateways.</span></span> <span data-ttu-id="de278-156">Všimněte si, že protokol BGP možnost je dostupná jenom na standardní a vyšší SKU.</span><span class="sxs-lookup"><span data-stu-id="de278-156">Note that BGP option is only available on Standard and higher SKUs.</span></span>
* <span data-ttu-id="de278-157">Vytvořit explicitní připojení z ovlivněné virtuální sítě k bráně místní sítě reprezentující místní umístění.</span><span class="sxs-lookup"><span data-stu-id="de278-157">Establish an explicit connection from affected VNet to the local network gateway representing on-premises location.</span></span> <span data-ttu-id="de278-158">To by také vyžadovat změnu konfigurace na místní směrovač vytvořit a nakonfigurovat tunelu IPsec.</span><span class="sxs-lookup"><span data-stu-id="de278-158">This would also require changing configuration on the on-premises router to create and configure the IPsec tunnel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de278-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de278-159">Next steps</span></span>
<span data-ttu-id="de278-160">Po získání informací o podpora migrace brány sítě VPN, přejděte na [platformy podporované migrace z classic do Resource Manager prostředky infrastruktury](../virtual-machines/windows/migration-classic-resource-manager-ps.md) začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="de278-160">After learning about VPN gateway migration support, go to [platform-supported migration of IaaS resources from classic to Resource Manager](../virtual-machines/windows/migration-classic-resource-manager-ps.md) to get started.</span></span>
