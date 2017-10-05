---
title: Typy IP adres v Azure | Dokumentace Microsoftu
description: "Další informace o veřejných a privátních IP adresách v Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 144f4ea213b8ed0a3530495e185f489155c474c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a><span data-ttu-id="54c8c-103">Typy IP adres a metody přidělování v Azure</span><span class="sxs-lookup"><span data-stu-id="54c8c-103">IP address types and allocation methods in Azure</span></span>
<span data-ttu-id="54c8c-104">Přiřazením IP adres k prostředkům Azure umožníte komunikaci s ostatními prostředky Azure, místní sítí a internetem.</span><span class="sxs-lookup"><span data-stu-id="54c8c-104">You can assign IP addresses to Azure resources to communicate with other Azure resources, your on-premises network, and the Internet.</span></span> <span data-ttu-id="54c8c-105">Existují dva typy IP adres, které můžete v Azure využít:</span><span class="sxs-lookup"><span data-stu-id="54c8c-105">There are two types of IP addresses you can use in Azure:</span></span>

* <span data-ttu-id="54c8c-106">**Veřejné IP adresy**: Slouží ke komunikaci s internetem, včetně veřejně přístupných služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="54c8c-106">**Public IP addresses**: Used for communication with the Internet, including Azure public-facing services</span></span>
* <span data-ttu-id="54c8c-107">**Privátní IP adresy**: Slouží ke komunikaci v rámci virtuální sítě Azure (VNet) a místní sítě, pokud použijete VPN Gateway nebo okruh ExpressRoute pro rozšíření sítě do Azure.</span><span class="sxs-lookup"><span data-stu-id="54c8c-107">**Private IP addresses**: Used for communication within an Azure virtual network (VNet), and your on-premises network when you use a VPN gateway or ExpressRoute circuit to extend your network to Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="54c8c-108">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="54c8c-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="54c8c-109">Tento článek se věnuje modelu nasazení Resource Manager, který Microsoft doporučuje pro většinu nových nasazení namísto [klasického modelu nasazení](virtual-network-ip-addresses-overview-classic.md).</span><span class="sxs-lookup"><span data-stu-id="54c8c-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](virtual-network-ip-addresses-overview-classic.md).</span></span>
> 

<span data-ttu-id="54c8c-110">Pokud už klasický model nasazení znáte, prohlédněte si [rozdíly v IP adresování mezi klasickým nasazením a nasazením Resource Manageru](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).</span><span class="sxs-lookup"><span data-stu-id="54c8c-110">If you are familiar with the classic deployment model, check the [differences in IP addressing between classic and Resource Manager](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).</span></span>

## <a name="public-ip-addresses"></a><span data-ttu-id="54c8c-111">Veřejné IP adresy</span><span class="sxs-lookup"><span data-stu-id="54c8c-111">Public IP addresses</span></span>
<span data-ttu-id="54c8c-112">Veřejné IP adresy umožňují prostředkům Azure komunikovat s internetem a veřejnými službami Azure, jako jsou například [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL Databases](../sql-database/sql-database-technical-overview.md) a [Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="54c8c-112">Public IP addresses allow Azure resources to communicate with Internet and Azure public-facing services such as [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL databases](../sql-database/sql-database-technical-overview.md), and [Azure storage](../storage/common/storage-introduction.md).</span></span>

<span data-ttu-id="54c8c-113">[Veřejná IP](resource-groups-networking.md#public-ip-address) adresa v Azure Resource Manageru je prostředek, který má svoje vlastní vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="54c8c-113">In Azure Resource Manager, a [public IP](resource-groups-networking.md#public-ip-address) address is a resource that has its own properties.</span></span> <span data-ttu-id="54c8c-114">Veřejnou IP adresu můžete přiřadit libovolnému z následujících prostředků:</span><span class="sxs-lookup"><span data-stu-id="54c8c-114">You can associate a public IP address resource with any of the following resources:</span></span>

* <span data-ttu-id="54c8c-115">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="54c8c-115">Virtual machines (VM)</span></span>
* <span data-ttu-id="54c8c-116">Internetové nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="54c8c-116">Internet-facing load balancers</span></span>
* <span data-ttu-id="54c8c-117">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-117">VPN gateways</span></span>
* <span data-ttu-id="54c8c-118">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-118">Application gateways</span></span>

### <a name="allocation-method"></a><span data-ttu-id="54c8c-119">Metoda přidělování</span><span class="sxs-lookup"><span data-stu-id="54c8c-119">Allocation method</span></span>
<span data-ttu-id="54c8c-120">Existují dvě metody přidělení IP adresy *veřejnému* IP prostředku – *dynamická* a *statická*.</span><span class="sxs-lookup"><span data-stu-id="54c8c-120">There are two methods in which an IP address is allocated to a *public* IP resource - *dynamic* or *static*.</span></span> <span data-ttu-id="54c8c-121">Výchozí metoda přidělení je *dynamická*, při které IP adresa **není** přidělená v okamžiku svého vytvoření.</span><span class="sxs-lookup"><span data-stu-id="54c8c-121">The default allocation method is *dynamic*, where an IP address is **not** allocated at the time of its creation.</span></span> <span data-ttu-id="54c8c-122">Místo toho se veřejná IP adresa přidělí, když spustíte (nebo vytvoříte) přidružený prostředek (jako je virtuální počítač nebo nástroj pro vyrovnávání zatížení).</span><span class="sxs-lookup"><span data-stu-id="54c8c-122">Instead, the public IP address is allocated when you start (or create) the associated resource (like a VM or load balancer).</span></span> <span data-ttu-id="54c8c-123">Když tento prostředek zastavíte (nebo odstraníte), IP adresa se uvolní.</span><span class="sxs-lookup"><span data-stu-id="54c8c-123">The IP address is released when you stop (or delete) the resource.</span></span> <span data-ttu-id="54c8c-124">To způsobuje, že se IP adresa při zastavení a spuštění prostředku změní.</span><span class="sxs-lookup"><span data-stu-id="54c8c-124">This causes the IP address to change when you stop and start a resource.</span></span>

<span data-ttu-id="54c8c-125">Pokud chcete zajistit, aby IP adresa přidruženého prostředku zůstala stejná, můžete explicitně nastavit *statickou* metodu přidělování.</span><span class="sxs-lookup"><span data-stu-id="54c8c-125">To ensure the IP address for the associated resource remains the same, you can set the allocation method explicitly to *static*.</span></span> <span data-ttu-id="54c8c-126">V tomto případě se IP adresa přiřadí okamžitě.</span><span class="sxs-lookup"><span data-stu-id="54c8c-126">In this case an IP address is assigned immediately.</span></span> <span data-ttu-id="54c8c-127">Uvolní se, jenom když prostředek odstraníte nebo změníte jeho metodu přidělování na *dynamickou*.</span><span class="sxs-lookup"><span data-stu-id="54c8c-127">It is released only when you delete the resource or change its allocation method to *dynamic*.</span></span>

> [!NOTE]
> <span data-ttu-id="54c8c-128">Ani při nastavení *statické* metody přidělování není možné určit vlastní IP adresu přiřazenou *veřejnému IP prostředku*.</span><span class="sxs-lookup"><span data-stu-id="54c8c-128">Even when you set the allocation method to *static*, you cannot specify the actual IP address assigned to the *public IP resource*.</span></span> <span data-ttu-id="54c8c-129">Tato adresa se vybírá z fondu dostupných IP adres v umístění Azure, ve kterém je prostředek vytvořený.</span><span class="sxs-lookup"><span data-stu-id="54c8c-129">Instead, it gets allocated from a pool of available IP addresses in the Azure location the resource is created in.</span></span>
>

<span data-ttu-id="54c8c-130">Statické veřejné IP adresy se obvykle používají v následujících scénářích:</span><span class="sxs-lookup"><span data-stu-id="54c8c-130">Static public IP addresses are commonly used in the following scenarios:</span></span>

* <span data-ttu-id="54c8c-131">Koncoví uživatelé potřebují aktualizovat pravidla brány firewall pro komunikaci s prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="54c8c-131">End-users need to update firewall rules to communicate with your Azure resources.</span></span>
* <span data-ttu-id="54c8c-132">Překlad názvů DNS, kde by změna IP adresy vyžadovala aktualizace záznamů A.</span><span class="sxs-lookup"><span data-stu-id="54c8c-132">DNS name resolution, where a change in IP address would require updating A records.</span></span>
* <span data-ttu-id="54c8c-133">Vaše prostředky Azure komunikují s ostatními aplikacemi nebo službami, které využívají model zabezpečení založený na IP adresách.</span><span class="sxs-lookup"><span data-stu-id="54c8c-133">Your Azure resources communicate with other apps or services that use an IP address-based security model.</span></span>
* <span data-ttu-id="54c8c-134">Využíváte certifikáty SSL propojené k IP adrese.</span><span class="sxs-lookup"><span data-stu-id="54c8c-134">You use SSL certificates linked to an IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="54c8c-135">Seznam rozsahů IP adres, ze kterých se veřejné IP adresy (dynamické/statické) přidělují prostředkům Azure, je zveřejněný na webu [Rozsahy IP adres datacenter Azure](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="54c8c-135">The list of IP ranges from which public IP addresses (dynamic/static) are allocated to Azure resources is published at [Azure Datacenter IP ranges](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>
>

### <a name="dns-hostname-resolution"></a><span data-ttu-id="54c8c-136">Překlad názvů hostitelů DNS</span><span class="sxs-lookup"><span data-stu-id="54c8c-136">DNS hostname resolution</span></span>
<span data-ttu-id="54c8c-137">Můžete zadat popisek názvu domény DNS pro veřejný IP prostředek. Na serverech DNS spravovaných Azure se vytvoří mapování *popisek_názvu_domény*.*umístění*.cloudapp.azure.com na veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-137">You can specify a DNS domain name label for a public IP resource, which creates a mapping for *domainnamelabel*.*location*.cloudapp.azure.com to the public IP address in the Azure-managed DNS servers.</span></span> <span data-ttu-id="54c8c-138">Pokud například vytvoříte veřejný IP prostředek, který jako *popisek_názvu_domény* má **contoso** a jako *umístění* v Azure používá **Západní USA**, plně kvalifikovaný název domény (FQDN) **contoso.westus.cloudapp.azure.com** se přeloží na veřejnou IP adresu tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="54c8c-138">For instance, if you create a public IP resource with **contoso** as a *domainnamelabel* in the **West US** Azure *location*, the fully-qualified domain name (FQDN) **contoso.westus.cloudapp.azure.com** will resolve to the public IP address of the resource.</span></span> <span data-ttu-id="54c8c-139">Tento plně kvalifikovaný název domény můžete použít k vytvoření vlastního záznamu CNAME domény odkazujícího na veřejnou IP adresu v Azure.</span><span class="sxs-lookup"><span data-stu-id="54c8c-139">You can use this FQDN to create a custom domain CNAME record pointing to the public IP address in Azure.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="54c8c-140">Každý vytvořený popisek názvu domény musí být v rámci příslušného umístění Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="54c8c-140">Each domain name label created must be unique within its Azure location.</span></span>  
>

### <a name="virtual-machines"></a><span data-ttu-id="54c8c-141">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="54c8c-141">Virtual machines</span></span>
<span data-ttu-id="54c8c-142">Veřejnou IP adresu můžete k virtuálnímu počítači s [Windows](../virtual-machines/windows/overview.md) nebo [Linuxem](../virtual-machines/virtual-machines-linux-about.md) přidružit tak, že ji přiřadíte jeho **síťovému rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="54c8c-142">You can associate a public IP address with a [Windows](../virtual-machines/windows/overview.md) or [Linux](../virtual-machines/virtual-machines-linux-about.md) VM by assigning it to its **network interface**.</span></span> <span data-ttu-id="54c8c-143">V případě virtuálních počítačů s více síťovými rozhraními můžete přiřazovat jenom k *primárnímu* síťovému rozhraní.</span><span class="sxs-lookup"><span data-stu-id="54c8c-143">In the case of a VM with multiple network interfaces, you can assign it to the *primary* network interface only.</span></span> <span data-ttu-id="54c8c-144">Virtuálnímu počítači můžete přiřadit dynamickou nebo statickou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-144">You can assign either a dynamic or a static public IP address to a VM.</span></span>

### <a name="internet-facing-load-balancers"></a><span data-ttu-id="54c8c-145">Internetové nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="54c8c-145">Internet-facing load balancers</span></span>
<span data-ttu-id="54c8c-146">Veřejnou IP adresu můžete přiřadit službě [Azure Load Balancer](../load-balancer/load-balancer-overview.md) tak, že ji přiřadíte konfiguraci **front-endu** tohoto nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="54c8c-146">You can associate a public IP address with an [Azure Load Balancer](../load-balancer/load-balancer-overview.md), by assigning it to the load balancer **frontend** configuration.</span></span> <span data-ttu-id="54c8c-147">Tato veřejná IP adresa slouží jako virtuální IP adresa (VIP) s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="54c8c-147">This public IP address serves as a load-balanced virtual IP address (VIP).</span></span> <span data-ttu-id="54c8c-148">Front-endu nástroje pro vyrovnávání zatížení můžete přiřadit dynamickou nebo statickou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-148">You can assign either a dynamic or a static public IP address to a load balancer front-end.</span></span> <span data-ttu-id="54c8c-149">Front-endu nástroje pro vyrovnávání zatížení můžete také přiřadit několik veřejných IP adres, což umožňuje použití scénářů s [několika VIP](../load-balancer/load-balancer-multivip.md), jako je víceklientské prostředí s weby využívajícími SSL.</span><span class="sxs-lookup"><span data-stu-id="54c8c-149">You can also assign multiple public IP addresses to a load balancer front-end, which enables [multi-VIP](../load-balancer/load-balancer-multivip.md) scenarios like a multi-tenant environment with SSL-based websites.</span></span>

### <a name="vpn-gateways"></a><span data-ttu-id="54c8c-150">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-150">VPN gateways</span></span>
<span data-ttu-id="54c8c-151">[Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) se používá k připojení virtuální sítě Azure (VNet) k ostatním virtuálním sítím Azure nebo k místní síti.</span><span class="sxs-lookup"><span data-stu-id="54c8c-151">[Azure VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) is used to connect an Azure virtual network (VNet) to other Azure VNets or to an on-premises network.</span></span> <span data-ttu-id="54c8c-152">Abyste povolili komunikaci se vzdálenou sítí, musíte přiřadit veřejnou IP adresu její **konfiguraci protokolu IP**.</span><span class="sxs-lookup"><span data-stu-id="54c8c-152">You need to assign a public IP address to its **IP configuration** to enable it to communicate with the remote network.</span></span> <span data-ttu-id="54c8c-153">V současné době je možné službě VPN Gateway přiřadit jenom *dynamickou* veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-153">Currently, you can only assign a *dynamic* public IP address to a VPN gateway.</span></span>

### <a name="application-gateways"></a><span data-ttu-id="54c8c-154">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-154">Application gateways</span></span>
<span data-ttu-id="54c8c-155">Veřejnou IP adresu můžete přiřadit službě [Azure Application Gateway](../application-gateway/application-gateway-introduction.md) tak, že ji přiřadíte konfiguraci **front-endu** této brány.</span><span class="sxs-lookup"><span data-stu-id="54c8c-155">You can associate a public IP address with an Azure [Application Gateway](../application-gateway/application-gateway-introduction.md), by assigning it to the gateway's **frontend** configuration.</span></span> <span data-ttu-id="54c8c-156">Tato veřejná IP adresa slouží jako virtuální IP adresa (VIP) s vyrovnáváním zatížení.</span><span class="sxs-lookup"><span data-stu-id="54c8c-156">This public IP address serves as a load-balanced VIP.</span></span> <span data-ttu-id="54c8c-157">V současné době je možné službě Application Gateway přiřadit jenom *dynamickou* veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-157">Currently, you can only assign a *dynamic* public IP address to an application gateway frontend configuration.</span></span>

### <a name="at-a-glance"></a><span data-ttu-id="54c8c-158">Přehledně</span><span class="sxs-lookup"><span data-stu-id="54c8c-158">At-a-glance</span></span>
<span data-ttu-id="54c8c-159">Následující tabulka ukazuje konkrétní vlastnost, jejímž prostřednictvím je možné veřejnou IP adresu přiřadit prostředku nejvyšší úrovně, a metody přidělení (dynamické nebo statické), které je možné použít.</span><span class="sxs-lookup"><span data-stu-id="54c8c-159">The table below shows the specific property through which a public IP address can be associated to a top-level resource, and the possible allocation methods (dynamic or static) that can be used.</span></span>

| <span data-ttu-id="54c8c-160">Prostředek nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="54c8c-160">Top-level resource</span></span> | <span data-ttu-id="54c8c-161">Přidružení IP adresy</span><span class="sxs-lookup"><span data-stu-id="54c8c-161">IP Address association</span></span> | <span data-ttu-id="54c8c-162">Dynamická</span><span class="sxs-lookup"><span data-stu-id="54c8c-162">Dynamic</span></span> | <span data-ttu-id="54c8c-163">Statická</span><span class="sxs-lookup"><span data-stu-id="54c8c-163">Static</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="54c8c-164">Virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="54c8c-164">Virtual machine</span></span> |<span data-ttu-id="54c8c-165">Síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="54c8c-165">Network interface</span></span> |<span data-ttu-id="54c8c-166">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-166">Yes</span></span> |<span data-ttu-id="54c8c-167">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-167">Yes</span></span> |
| <span data-ttu-id="54c8c-168">Nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="54c8c-168">Load balancer</span></span> |<span data-ttu-id="54c8c-169">Konfigurace front-endu</span><span class="sxs-lookup"><span data-stu-id="54c8c-169">Front end configuration</span></span> |<span data-ttu-id="54c8c-170">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-170">Yes</span></span> |<span data-ttu-id="54c8c-171">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-171">Yes</span></span> |
| <span data-ttu-id="54c8c-172">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-172">VPN gateway</span></span> |<span data-ttu-id="54c8c-173">Konfigurace protokolu IP brány</span><span class="sxs-lookup"><span data-stu-id="54c8c-173">Gateway IP configuration</span></span> |<span data-ttu-id="54c8c-174">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-174">Yes</span></span> |<span data-ttu-id="54c8c-175">Ne</span><span class="sxs-lookup"><span data-stu-id="54c8c-175">No</span></span> |
| <span data-ttu-id="54c8c-176">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-176">Application gateway</span></span> |<span data-ttu-id="54c8c-177">Konfigurace front-endu</span><span class="sxs-lookup"><span data-stu-id="54c8c-177">Front end configuration</span></span> |<span data-ttu-id="54c8c-178">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-178">Yes</span></span> |<span data-ttu-id="54c8c-179">Ne</span><span class="sxs-lookup"><span data-stu-id="54c8c-179">No</span></span> |

## <a name="private-ip-addresses"></a><span data-ttu-id="54c8c-180">Privátní IP adresy</span><span class="sxs-lookup"><span data-stu-id="54c8c-180">Private IP addresses</span></span>
<span data-ttu-id="54c8c-181">Privátní IP adresy umožňují prostředkům Azure komunikovat s ostatními prostředky ve [virtuální](virtual-networks-overview.md) nebo místní síti prostřednictvím brány sítě VPN nebo okruhu ExpressRoute, a to bez použití IP adresy dostupné na internetu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-181">Private IP addresses allow Azure resources to communicate with other resources in a [virtual network](virtual-networks-overview.md) or an on-premises network through a VPN gateway or ExpressRoute circuit, without using an Internet-reachable IP address.</span></span>

<span data-ttu-id="54c8c-182">V modelu nasazení Azure Resource Manager se IP adresa přidruží k následujícím typům prostředků Azure:</span><span class="sxs-lookup"><span data-stu-id="54c8c-182">In the Azure Resource Manager deployment model, a private IP address is associated to the following types of Azure resources:</span></span>

* <span data-ttu-id="54c8c-183">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="54c8c-183">VMs</span></span>
* <span data-ttu-id="54c8c-184">Interní nástroje pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="54c8c-184">Internal load balancers (ILBs)</span></span>
* <span data-ttu-id="54c8c-185">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-185">Application gateways</span></span>

### <a name="allocation-method"></a><span data-ttu-id="54c8c-186">Metoda přidělování</span><span class="sxs-lookup"><span data-stu-id="54c8c-186">Allocation method</span></span>
<span data-ttu-id="54c8c-187">Privátní IP adresa se přiděluje z rozsahu adres v podsíti, ke které je prostředek připojen.</span><span class="sxs-lookup"><span data-stu-id="54c8c-187">A private IP address is allocated from the address range of the subnet to which the resource is attached.</span></span> <span data-ttu-id="54c8c-188">Rozsah adres samotné podsítě je součástí rozsahu adres virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="54c8c-188">The address range of the subnet itself is a part of the VNet's address range.</span></span>

<span data-ttu-id="54c8c-189">Existují dvě metody přidělení privátní IP adresy: *dynamická* a *statická*.</span><span class="sxs-lookup"><span data-stu-id="54c8c-189">There are two methods in which a private IP address is allocated: *dynamic* or *static*.</span></span> <span data-ttu-id="54c8c-190">Výchozí metodou je *dynamické* přidělení, kdy se IP adresa automaticky přiděluje z podsítě prostředku (pomocí protokolu DHCP).</span><span class="sxs-lookup"><span data-stu-id="54c8c-190">The default allocation method is *dynamic*, where the IP address is automatically allocated from the resource's subnet (using DHCP).</span></span> <span data-ttu-id="54c8c-191">Tuto IP adresa se může při zastavení a spuštění prostředku změnit.</span><span class="sxs-lookup"><span data-stu-id="54c8c-191">This IP address can change when you stop and start the resource.</span></span>

<span data-ttu-id="54c8c-192">Pokud chcete zajistit, aby IP adresa zůstávala stejná, můžete nastavit *statickou* metodu přidělení.</span><span class="sxs-lookup"><span data-stu-id="54c8c-192">You can set the allocation method to *static* to ensure the IP address remains the same.</span></span> <span data-ttu-id="54c8c-193">V tomto případě musíte také poskytnout platnou IP adresu, která je součástí podsítě prostředku.</span><span class="sxs-lookup"><span data-stu-id="54c8c-193">In this case, you also need to provide a valid IP address that is part of the resource's subnet.</span></span>

<span data-ttu-id="54c8c-194">Statické privátní IP adresy se obvykle používají pro:</span><span class="sxs-lookup"><span data-stu-id="54c8c-194">Static private IP addresses are commonly used for:</span></span>

* <span data-ttu-id="54c8c-195">Virtuální počítače, které slouží jako řadiče domény nebo servery DNS.</span><span class="sxs-lookup"><span data-stu-id="54c8c-195">VMs that act as domain controllers or DNS servers.</span></span>
* <span data-ttu-id="54c8c-196">Prostředky, které vyžadují pravidla brány firewall s využitím IP adres.</span><span class="sxs-lookup"><span data-stu-id="54c8c-196">Resources that require firewall rules using IP addresses.</span></span>
* <span data-ttu-id="54c8c-197">Prostředky, ke kterým se přistupuje z jiných aplikací nebo prostředků prostřednictvím IP adresy.</span><span class="sxs-lookup"><span data-stu-id="54c8c-197">Resources accessed by other apps/resources through an IP address.</span></span>

### <a name="virtual-machines"></a><span data-ttu-id="54c8c-198">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="54c8c-198">Virtual machines</span></span>
<span data-ttu-id="54c8c-199">Privátní IP adresa se přiřazuje **síťovému rozhraní** virtuálního počítače s [Windows](../virtual-machines/windows/overview.md) nebo [Linuxem](../virtual-machines/virtual-machines-linux-about.md).</span><span class="sxs-lookup"><span data-stu-id="54c8c-199">A private IP address is assigned to the **network interface** of a [Windows](../virtual-machines/windows/overview.md) or [Linux](../virtual-machines/virtual-machines-linux-about.md) VM.</span></span> <span data-ttu-id="54c8c-200">V případě virtuálního počítače s několika síťovými rozhraními se privátní IP adresa přiřadí každému z těchto rozhraní.</span><span class="sxs-lookup"><span data-stu-id="54c8c-200">In case of a multi-network interface VM, each interface gets a private IP address assigned.</span></span> <span data-ttu-id="54c8c-201">Pro síťové rozhraní můžete určit dynamickou nebo statickou metodu přidělení.</span><span class="sxs-lookup"><span data-stu-id="54c8c-201">You can specify the allocation method as either dynamic or static for a network interface.</span></span>

#### <a name="internal-dns-hostname-resolution-for-vms"></a><span data-ttu-id="54c8c-202">Překlad názvů hostitelů interních služeb DNS (pro virtuální počítače)</span><span class="sxs-lookup"><span data-stu-id="54c8c-202">Internal DNS hostname resolution (for VMs)</span></span>
<span data-ttu-id="54c8c-203">Všechny virtuální počítače Azure jsou ve výchozím nastavení nakonfigurované se [servery DNS spravovanými Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) (pokud explicitně nenakonfigurujete vlastní servery DNS).</span><span class="sxs-lookup"><span data-stu-id="54c8c-203">All Azure VMs are configured with [Azure-managed DNS servers](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) by default, unless you explicitly configure custom DNS servers.</span></span> <span data-ttu-id="54c8c-204">Tyto servery DNS poskytují interní překlad IP adres pro virtuální počítače umístěné ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="54c8c-204">These DNS servers provide internal name resolution for VMs that reside within the same VNet.</span></span>

<span data-ttu-id="54c8c-205">Když vytvoříte virtuální počítač, do serverů DNS spravovaných Azure se přidá mapování názvu hostitele na jeho IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-205">When you create a VM, a mapping for the hostname to its private IP address is added to the Azure-managed DNS servers.</span></span> <span data-ttu-id="54c8c-206">V případě virtuálního počítače s několika síťovými rozhraními se název hostitele mapuje na privátní IP adresu primárního síťového rozhraní.</span><span class="sxs-lookup"><span data-stu-id="54c8c-206">In case of a multi-network interface VM, the hostname is mapped to the private IP address of the primary network interface.</span></span>

<span data-ttu-id="54c8c-207">Virtuální počítače nakonfigurované servery DNS spravovanými Azure mohou překládat názvy hostitelů všech virtuálních počítačů v rámci virtuální sítě na jejich privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="54c8c-207">VMs configured with Azure-managed DNS servers will be able to resolve the hostnames of all VMs within their VNet to their private IP addresses.</span></span>

### <a name="internal-load-balancers-ilb--application-gateways"></a><span data-ttu-id="54c8c-208">Interní nástroje pro vyrovnávání a Application Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-208">Internal load balancers (ILB) & Application gateways</span></span>
<span data-ttu-id="54c8c-209">Privátní IP adresu můžete přiřadit konfiguraci **front-endu** nástroje [Azure Internal Load Balancer](../load-balancer/load-balancer-internal-overview.md) (ILB) nebo služby [Azure Application Gateway](../application-gateway/application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="54c8c-209">You can assign a private IP address to the **front end** configuration of an [Azure Internal Load Balancer](../load-balancer/load-balancer-internal-overview.md) (ILB) or an [Azure Application Gateway](../application-gateway/application-gateway-introduction.md).</span></span> <span data-ttu-id="54c8c-210">Tato privátní IP adresa slouží jako interní koncový bod, který je přístupný pouze pro prostředky v příslušné virtuální síti (VNet) a ve vzdálených sítích připojených k virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="54c8c-210">This private IP address serves as an internal endpoint, accessible only to the resources within its virtual network (VNet) and the remote networks connected to the VNet.</span></span> <span data-ttu-id="54c8c-211">Konfiguraci front-endu můžete přiřadit dynamickou nebo statickou privátní IP adresu.</span><span class="sxs-lookup"><span data-stu-id="54c8c-211">You can assign either a dynamic or static private IP address to the front end configuration.</span></span>

### <a name="at-a-glance"></a><span data-ttu-id="54c8c-212">Přehledně</span><span class="sxs-lookup"><span data-stu-id="54c8c-212">At-a-glance</span></span>
<span data-ttu-id="54c8c-213">Následující tabulka ukazuje konkrétní vlastnost, jejímž prostřednictvím je možné privátní IP adresu přiřadit prostředku nejvyšší úrovně, a metody přidělení (dynamické nebo statické), které je možné použít.</span><span class="sxs-lookup"><span data-stu-id="54c8c-213">The table below shows the specific property through which a private IP address can be associated to a top-level resource, and the possible allocation methods (dynamic or static) that can be used.</span></span>

| <span data-ttu-id="54c8c-214">Prostředek nejvyšší úrovně</span><span class="sxs-lookup"><span data-stu-id="54c8c-214">Top-level resource</span></span> | <span data-ttu-id="54c8c-215">Přidružení IP adresy</span><span class="sxs-lookup"><span data-stu-id="54c8c-215">IP address association</span></span> | <span data-ttu-id="54c8c-216">Dynamická</span><span class="sxs-lookup"><span data-stu-id="54c8c-216">Dynamic</span></span> | <span data-ttu-id="54c8c-217">Statická</span><span class="sxs-lookup"><span data-stu-id="54c8c-217">Static</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="54c8c-218">Virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="54c8c-218">Virtual machine</span></span> |<span data-ttu-id="54c8c-219">Síťové rozhraní</span><span class="sxs-lookup"><span data-stu-id="54c8c-219">Network interface</span></span> |<span data-ttu-id="54c8c-220">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-220">Yes</span></span> |<span data-ttu-id="54c8c-221">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-221">Yes</span></span> |
| <span data-ttu-id="54c8c-222">Nástroj pro vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="54c8c-222">Load balancer</span></span> |<span data-ttu-id="54c8c-223">Konfigurace front-endu</span><span class="sxs-lookup"><span data-stu-id="54c8c-223">Front end configuration</span></span> |<span data-ttu-id="54c8c-224">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-224">Yes</span></span> |<span data-ttu-id="54c8c-225">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-225">Yes</span></span> |
| <span data-ttu-id="54c8c-226">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="54c8c-226">Application gateway</span></span> |<span data-ttu-id="54c8c-227">Konfigurace front-endu</span><span class="sxs-lookup"><span data-stu-id="54c8c-227">Front end configuration</span></span> |<span data-ttu-id="54c8c-228">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-228">Yes</span></span> |<span data-ttu-id="54c8c-229">Ano</span><span class="sxs-lookup"><span data-stu-id="54c8c-229">Yes</span></span> |

## <a name="limits"></a><span data-ttu-id="54c8c-230">Omezení</span><span class="sxs-lookup"><span data-stu-id="54c8c-230">Limits</span></span>
<span data-ttu-id="54c8c-231">Omezení IP adresování jsou uvedená v kompletní sadě [omezení sítě](../azure-subscription-service-limits.md#networking-limits) v Azure.</span><span class="sxs-lookup"><span data-stu-id="54c8c-231">The limits imposed on IP addressing are indicated in the full set of [limits for networking](../azure-subscription-service-limits.md#networking-limits) in Azure.</span></span> <span data-ttu-id="54c8c-232">Tato omezení platí pro jednotlivé oblasti a jednotlivá předplatná.</span><span class="sxs-lookup"><span data-stu-id="54c8c-232">These limits are per region and per subscription.</span></span> <span data-ttu-id="54c8c-233">Pokud chcete v závislosti na svých obchodních potřebách zvýšit výchozí omezení na povolené maximum, [kontaktujte podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="54c8c-233">You can [contact support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to increase the default limits up to the maximum limits based on your business needs.</span></span>

## <a name="pricing"></a><span data-ttu-id="54c8c-234">Ceny</span><span class="sxs-lookup"><span data-stu-id="54c8c-234">Pricing</span></span>
<span data-ttu-id="54c8c-235">Za veřejné IP adresy se může účtovat nominální poplatek.</span><span class="sxs-lookup"><span data-stu-id="54c8c-235">Public IP addresses may have a nominal charge.</span></span> <span data-ttu-id="54c8c-236">Další informace o cenách IP adres v Azure najdete na stránce [Ceny IP adres](https://azure.microsoft.com/pricing/details/ip-addresses).</span><span class="sxs-lookup"><span data-stu-id="54c8c-236">To learn more about IP address pricing in Azure, review the [IP address pricing](https://azure.microsoft.com/pricing/details/ip-addresses) page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54c8c-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54c8c-237">Next steps</span></span>
* [<span data-ttu-id="54c8c-238">Nasazení virtuálního počítače se statickou veřejnou IP adresou pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="54c8c-238">Deploy a VM with a static public IP using the Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
* [<span data-ttu-id="54c8c-239">Nasazení virtuálního počítače se statickou veřejnou IP adresou pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="54c8c-239">Deploy a VM with a static public IP using a template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
* [<span data-ttu-id="54c8c-240">Nasazení virtuálního počítače se statickou privátní IP adresou pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="54c8c-240">Deploy a VM with a static private IP address using the Azure portal</span></span>](virtual-networks-static-private-ip-arm-pportal.md)