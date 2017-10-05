---
title: "Bezpečné připojení k back-endové prostředky ze služby App Service Environment"
description: "Další informace o tom, jak bezpečně připojit back-endové prostředky ze služby App Service Environment."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: f82eb283-a6e7-4923-a00b-4b4ccf7c4b5b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: stefsch
ms.openlocfilehash: 0b6d3a47dc429c469b37c2c74f546cfeca580358
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a><span data-ttu-id="330fd-103">Bezpečné připojení k back-endové prostředky ze služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="330fd-103">Securely Connecting to Backend Resources from an App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="330fd-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="330fd-104">Overview</span></span>
<span data-ttu-id="330fd-105">Vzhledem k tomu, že služby App Service Environment se vždy vytvoří v **buď** virtuální síť Azure Resource Manager **nebo** modelu nasazení classic [virtuální sítě] [ virtualnetwork], odchozí připojení ze služby App Service Environment jiných back-endové prostředky můžete toku výhradně přes virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="330fd-105">Since an App Service Environment is always created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model [virtual network][virtualnetwork], outbound connections from an App Service Environment to other backend resources can flow exclusively over the virtual network.</span></span>  <span data-ttu-id="330fd-106">S poslední změny provedené v červnu 2016 mohou být nasazeny ASEs do virtuální sítě, které používají rozsahů veřejných adres nebo RFC1918 adresní prostory (tj.)</span><span class="sxs-lookup"><span data-stu-id="330fd-106">With a recent change made in June 2016, ASEs can also be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e.</span></span> <span data-ttu-id="330fd-107">privátní adresy).</span><span class="sxs-lookup"><span data-stu-id="330fd-107">private addresses).</span></span>  

<span data-ttu-id="330fd-108">Například může být SQL Server běžící na clusteru s podporou virtuálních počítačů s port 1433 uzamčené.</span><span class="sxs-lookup"><span data-stu-id="330fd-108">For example, there may be a SQL Server running on a cluster of virtual machines with port 1433 locked down.</span></span>  <span data-ttu-id="330fd-109">Koncový bod může být ACLd pouze povolit přístup z jiných zdrojů ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="330fd-109">The endpoint may be ACLd to only allow access from other resources on the same virtual network.</span></span>  

<span data-ttu-id="330fd-110">Další příklad citlivé koncových bodů může spustit místně a připojit k Azure přes buď [Site-to-Site] [ SiteToSite] nebo [Azure ExpressRoute] [ ExpressRoute] připojení.</span><span class="sxs-lookup"><span data-stu-id="330fd-110">As another example, sensitive endpoints may run on-premises and be connected to Azure via either [Site-to-Site][SiteToSite] or [Azure ExpressRoute][ExpressRoute] connections.</span></span>  <span data-ttu-id="330fd-111">V důsledku toho pouze prostředky ve virtuálních sítích, které jsou připojené k Site-to-Site nebo ExpressRoute tunely bude mít přístup k místní koncové body.</span><span class="sxs-lookup"><span data-stu-id="330fd-111">As a result, only resources in virtual networks connected to the Site-to-Site or ExpressRoute tunnels will be able to access on-premises endpoints.</span></span>

<span data-ttu-id="330fd-112">Pro všechny z těchto scénářů bude aplikace běžící na služby App Service Environment navázat zabezpečené připojení k různým serverům a prostředky.</span><span class="sxs-lookup"><span data-stu-id="330fd-112">For all of these scenarios, apps running on an App Service Environment will be able to securely connect to the various servers and resources.</span></span>  <span data-ttu-id="330fd-113">Odchozí provoz z aplikace běžící ve službě App Service Environment privátní koncových bodů ve stejné virtuální síti (nebo připojené ke stejné virtuální síti), bude pouze toku přes virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="330fd-113">Outbound traffic from apps running in an App Service Environment to private endpoints in the same virtual network (or connected to the same virtual network), will only flow over the virtual network.</span></span>  <span data-ttu-id="330fd-114">Odchozí přenosy na privátní koncové body nebude toku prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="330fd-114">Outbound traffic to private endpoints will not flow over the public Internet.</span></span>

<span data-ttu-id="330fd-115">Jeden přímý přístup paměti platí pro odchozí provoz ze služby App Service Environment do koncových bodů v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="330fd-115">One caveat applies to outbound traffic from an App Service Environment to endpoints within a virtual network.</span></span>  <span data-ttu-id="330fd-116">Prostředí App Service se nemůže připojit koncové body virtuálních počítačů, které jsou umístěné v **stejné** podsíti jako App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="330fd-116">App Service Environments cannot reach endpoints of virtual machines located in the **same** subnet as the App Service Environment.</span></span>  <span data-ttu-id="330fd-117">Za normálních okolností nemělo by se jednat problém, dokud prostředí App Service jsou nasazeny do podsítě vyhrazené pro výhradní použití App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="330fd-117">This normally should not be a problem as long as App Service Environments are deployed into a subnet reserved for exclusive use by only the App Service Environment.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a><span data-ttu-id="330fd-118">Odchozí připojení a požadavky na DNS</span><span class="sxs-lookup"><span data-stu-id="330fd-118">Outbound Connectivity and DNS Requirements</span></span>
<span data-ttu-id="330fd-119">Pro služby App Service Environment ke správnému fungování vyžaduje odchozí přístup k různým koncové body.</span><span class="sxs-lookup"><span data-stu-id="330fd-119">For an App Service Environment to function properly, it requires outbound access to various endpoints.</span></span> <span data-ttu-id="330fd-120">Úplný seznam externí koncové body používané App Service Environment je v části "Vyžaduje připojení k síti" [konfiguraci sítě pro ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) článku.</span><span class="sxs-lookup"><span data-stu-id="330fd-120">A full list of the external endpoints used by an ASE is in the "Required Network Connectivity" section of the [Network Configuration for ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) article.</span></span>

<span data-ttu-id="330fd-121">Prostředí služby App Service vyžaduje platnou konfiguraci pro virtuální síť infrastruktury služby DNS.</span><span class="sxs-lookup"><span data-stu-id="330fd-121">App Service Environments require a valid DNS infrastructure configured for the virtual network.</span></span>  <span data-ttu-id="330fd-122">Pokud z nějakého důvodu dojde ke změně konfigurace DNS po vytvoření služby App Service Environment, vývojáři vynutit služby App Service Environment načíst novou konfiguraci DNS.</span><span class="sxs-lookup"><span data-stu-id="330fd-122">If for any reason the DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment to pick up the new DNS configuration.</span></span>  <span data-ttu-id="330fd-123">Aktivuje restartu postupného prostředí pomocí ikonu "Restartovat" se nachází v horní části okna Správa služby App Service Environment na portálu způsobí, že prostředí tak, aby se vyzvedávat novou konfiguraci DNS.</span><span class="sxs-lookup"><span data-stu-id="330fd-123">Triggering a rolling environment reboot using the "Restart" icon located at the top of the App Service Environment management blade in the portal will cause the environment to pick up the new DNS configuration.</span></span>

<span data-ttu-id="330fd-124">Dále je doporučeno, aby žádné vlastní servery DNS na virtuální sítě se instalační program dopředu před vytvořením služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="330fd-124">It is also recommended that any custom DNS servers on the vnet be setup ahead of time prior to creating an App Service Environment.</span></span>  <span data-ttu-id="330fd-125">Pokud je při vytváření služby App Service Environment se změnila konfigurace DNS virtuální sítě, které způsobí selhání procesu vytvoření služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="330fd-125">If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in the App Service Environment creation process failing.</span></span>  <span data-ttu-id="330fd-126">V souvislosti podobnou Pokud na druhém konci brány VPN existuje vlastního serveru DNS a serveru DNS je nedostupná nebo není k dispozici, procesu vytváření služby App Service Environment se také nezdaří.</span><span class="sxs-lookup"><span data-stu-id="330fd-126">In a similar vein, if a custom DNS server exists on the other end of a VPN gateway, and the DNS server is unreachable or unavailable, the App Service Environment creation process will also fail.</span></span>

## <a name="connecting-to-a-sql-server"></a><span data-ttu-id="330fd-127">Připojení k serveru SQL</span><span class="sxs-lookup"><span data-stu-id="330fd-127">Connecting to a SQL Server</span></span>
<span data-ttu-id="330fd-128">Obvyklé konfigurace systému SQL Server má koncový bod naslouchání na portu 1433:</span><span class="sxs-lookup"><span data-stu-id="330fd-128">A common SQL Server configuration has an endpoint listening on port 1433:</span></span>

![Koncový bod serveru SQL][SqlServerEndpoint]

<span data-ttu-id="330fd-130">Existují dva přístupy pro provoz směřující do tohoto koncového bodu:</span><span class="sxs-lookup"><span data-stu-id="330fd-130">There are two approaches for restricting traffic to this endpoint:</span></span>

* <span data-ttu-id="330fd-131">[Sítě seznamy řízení přístupu] [ NetworkAccessControlLists] (sítě ACL)</span><span class="sxs-lookup"><span data-stu-id="330fd-131">[Network Access Control Lists][NetworkAccessControlLists] (Network ACLs)</span></span>
* <span data-ttu-id="330fd-132">[Skupiny zabezpečení sítě][NetworkSecurityGroups]</span><span class="sxs-lookup"><span data-stu-id="330fd-132">[Network Security Groups][NetworkSecurityGroups]</span></span>

## <a name="restricting-access-with-a-network-acl"></a><span data-ttu-id="330fd-133">Omezení přístupu k síti seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="330fd-133">Restricting Access With a Network ACL</span></span>
<span data-ttu-id="330fd-134">Port 1433 lze zabezpečit pomocí seznamu řízení přístupu síti.</span><span class="sxs-lookup"><span data-stu-id="330fd-134">Port 1433 can be secured using a network access control list.</span></span>  <span data-ttu-id="330fd-135">V příkladu níže povolených programů klienta adresy pocházející z uvnitř virtuální sítě a blokuje přístup pro všechny ostatní klienty.</span><span class="sxs-lookup"><span data-stu-id="330fd-135">The example below whitelists client addresses originating from inside of a virtual network, and blocks access to all other clients.</span></span>

![Příklad seznamu řízení přístupu sítě][NetworkAccessControlListExample]

<span data-ttu-id="330fd-137">Všechny aplikace běžící v App Service Environment ve stejné virtuální síti jako SQL Server bude moci připojit k instanci SQL serveru pomocí **interní virtuální síť** IP adresu pro virtuální počítač systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="330fd-137">Any applications running in App Service Environment in the same virtual network as the SQL Server will be able to connect to the SQL Server instance using the **VNet internal** IP address for the SQL Server virtual machine.</span></span>  

<span data-ttu-id="330fd-138">Příklad připojovacího řetězce níže odkazuje na SQL Server pomocí jeho privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="330fd-138">The example connection string below references the SQL Server using its private IP address.</span></span>

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

<span data-ttu-id="330fd-139">I když virtuální počítač obsahuje také veřejný koncový bod, pokusy o připojení pomocí veřejnou IP adresu se odmítne kvůli sítě ACL.</span><span class="sxs-lookup"><span data-stu-id="330fd-139">Although the virtual machine has a public endpoint as well, connection attempts using the public IP address will be rejected because of the network ACL.</span></span> 

## <a name="restricting-access-with-a-network-security-group"></a><span data-ttu-id="330fd-140">Omezení přístupu ke skupině zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="330fd-140">Restricting Access With a Network Security Group</span></span>
<span data-ttu-id="330fd-141">Alternativní způsob pro přístup k zabezpečení je skupina zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="330fd-141">An alternative approach for securing access is with a network security group.</span></span>  <span data-ttu-id="330fd-142">Skupiny zabezpečení sítě můžete použít pro jednotlivé virtuální počítače, nebo podsíť obsahující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="330fd-142">Network security groups can be applied to individual virtual machines, or to a subnet containing virtual machines.</span></span>

<span data-ttu-id="330fd-143">Nejprve je potřeba vytvořit skupinu zabezpečení sítě:</span><span class="sxs-lookup"><span data-stu-id="330fd-143">First a network security group needs to be created:</span></span>

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

<span data-ttu-id="330fd-144">Omezení přístupu k jenom interní provoz VNet je velmi jednoduchý ke skupině zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="330fd-144">Restricting access to only VNet internal traffic is very simple with a network security group.</span></span>  <span data-ttu-id="330fd-145">Výchozí pravidla ve skupině zabezpečení sítě povolí přístup pouze z jiných klientů sítě ve stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="330fd-145">The default rules in a network security group only allow access from other network clients in the same virtual network.</span></span>

<span data-ttu-id="330fd-146">V důsledku zablokujete přístup k systému SQL Server je jednoduché, použití skupinu zabezpečení sítě s jeho výchozí pravidla buď na virtuální počítače s SQL serverem, nebo podsíť obsahující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="330fd-146">As a result locking down access to SQL Server is as simple as applying a network security group with its default rules to either the virtual machines running SQL Server, or the subnet containing the virtual machines.</span></span>

<span data-ttu-id="330fd-147">Následující ukázka platí skupinu zabezpečení sítě pro podsíť obsahující:</span><span class="sxs-lookup"><span data-stu-id="330fd-147">The sample below applies a network security group to the containing subnet:</span></span>

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

<span data-ttu-id="330fd-148">Konečný výsledek je sada pravidel zabezpečení, která blokují externí přístup při povolení přístupu interní virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="330fd-148">The end result is a set of security rules that block external access, while allowing VNet internal access:</span></span>

![Výchozí pravidla zabezpečení sítě][DefaultNetworkSecurityRules]

## <a name="getting-started"></a><span data-ttu-id="330fd-150">Začínáme</span><span class="sxs-lookup"><span data-stu-id="330fd-150">Getting started</span></span>
<span data-ttu-id="330fd-151">Všechny články a jak – do této pro App Service Environment jsou dostupné v [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="330fd-151">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="330fd-152">Chcete-li začít pracovat s prostředí App Service, přečtěte si téma [Úvod do služby App Service Environment][IntroToAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="330fd-152">To get started with App Service Environments, see [Introduction to App Service Environment][IntroToAppServiceEnvironment]</span></span>

<span data-ttu-id="330fd-153">Podrobnosti ohledně ovládání příchozí přenosy do služby App Service Environment najdete v tématu [řízení příchozí přenosy do služby App Service Environment][ControlInboundASE]</span><span class="sxs-lookup"><span data-stu-id="330fd-153">For details around controlling inbound traffic to your App Service Environment, see [Controlling inbound traffic to an App Service Environment][ControlInboundASE]</span></span>

<span data-ttu-id="330fd-154">Další informace o platformě Azure App Service najdete v tématu [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="330fd-154">For more information about the Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
