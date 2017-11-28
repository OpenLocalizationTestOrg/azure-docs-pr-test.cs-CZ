---
title: "aaaSecurely připojení tooBackEnd prostředky ze služby App Service Environment"
description: "Informace o tom, jak toosecurely připojení toobackend prostředků ze služby App Service Environment."
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
ms.openlocfilehash: 6311d3fc301512ea3c4ed8f14f268f75755aa415
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="securely-connecting-toobackend-resources-from-an-app-service-environment"></a><span data-ttu-id="50521-103">Bezpečně připojení tooBackend prostředky ze služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="50521-103">Securely Connecting tooBackend Resources from an App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="50521-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="50521-104">Overview</span></span>
<span data-ttu-id="50521-105">Vzhledem k tomu, že služby App Service Environment se vždy vytvoří v **buď** virtuální síť Azure Resource Manager **nebo** modelu nasazení classic [virtuální sítě] [ virtualnetwork], odchozí připojení ze App Service Environment tooother back-endové prostředky můžete toku výhradně prostřednictvím hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="50521-105">Since an App Service Environment is always created in **either** an Azure Resource Manager virtual network, **or** a classic deployment model [virtual network][virtualnetwork], outbound connections from an App Service Environment tooother backend resources can flow exclusively over hello virtual network.</span></span>  <span data-ttu-id="50521-106">Nedávné změny provedené v června 2016 ASEs také lze nasadit do virtuálních sítí, které používají rozsahů veřejných adres nebo RFC1918 adresní prostory (tj. privátní adresy).</span><span class="sxs-lookup"><span data-stu-id="50521-106">With a recent change made in June 2016, ASEs can also be deployed into virtual networks that use either public address ranges, or RFC1918 address spaces (i.e. private addresses).</span></span>  

<span data-ttu-id="50521-107">Například může být SQL Server běžící na clusteru s podporou virtuálních počítačů s port 1433 uzamčené.</span><span class="sxs-lookup"><span data-stu-id="50521-107">For example, there may be a SQL Server running on a cluster of virtual machines with port 1433 locked down.</span></span>  <span data-ttu-id="50521-108">koncový bod Hello může být ACLd tooonly povolit přístup z jiných zdrojů na hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="50521-108">hello endpoint may be ACLd tooonly allow access from other resources on hello same virtual network.</span></span>  

<span data-ttu-id="50521-109">Další příklad citlivé koncových bodů může spustit místně a může být připojené tooAzure prostřednictvím buď [Site-to-Site] [ SiteToSite] nebo [Azure ExpressRoute] [ ExpressRoute] připojení.</span><span class="sxs-lookup"><span data-stu-id="50521-109">As another example, sensitive endpoints may run on-premises and be connected tooAzure via either [Site-to-Site][SiteToSite] or [Azure ExpressRoute][ExpressRoute] connections.</span></span>  <span data-ttu-id="50521-110">V důsledku toho pouze prostředky ve virtuální sítě připojen toohello Site-to-Site nebo ExpressRoute tunely bude možné tooaccess místní koncové body.</span><span class="sxs-lookup"><span data-stu-id="50521-110">As a result, only resources in virtual networks connected toohello Site-to-Site or ExpressRoute tunnels will be able tooaccess on-premises endpoints.</span></span>

<span data-ttu-id="50521-111">Pro všechny z těchto scénářů aplikace běžící na služby App Service Environment být schopný toosecurely připojí toohello různé servery a prostředky.</span><span class="sxs-lookup"><span data-stu-id="50521-111">For all of these scenarios, apps running on an App Service Environment will be able toosecurely connect toohello various servers and resources.</span></span>  <span data-ttu-id="50521-112">Odchozí provoz z aplikace běžící v koncových bodů App Service Environment tooprivate v hello stejné virtuální síti (nebo připojený toohello stejné virtuální síti), bude pouze toku hello virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="50521-112">Outbound traffic from apps running in an App Service Environment tooprivate endpoints in hello same virtual network (or connected toohello same virtual network), will only flow over hello virtual network.</span></span>  <span data-ttu-id="50521-113">Tooprivate odchozí přenosy, které koncové body nebude toku přes hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="50521-113">Outbound traffic tooprivate endpoints will not flow over hello public Internet.</span></span>

<span data-ttu-id="50521-114">Jeden přímý přístup paměti platí toooutbound provoz ze App Service Environment tooendpoints v rámci virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="50521-114">One caveat applies toooutbound traffic from an App Service Environment tooendpoints within a virtual network.</span></span>  <span data-ttu-id="50521-115">Prostředí App Service se nemůže připojit koncové body virtuálních počítačů, které jsou umístěné v hello **stejné** podsítě jako hello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="50521-115">App Service Environments cannot reach endpoints of virtual machines located in hello **same** subnet as hello App Service Environment.</span></span>  <span data-ttu-id="50521-116">Za normálních okolností nemělo by se jednat problém, dokud prostředí App Service jsou nasazeny do podsítě vyhrazené pro výhradní použití pouze hello App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="50521-116">This normally should not be a problem as long as App Service Environments are deployed into a subnet reserved for exclusive use by only hello App Service Environment.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="outbound-connectivity-and-dns-requirements"></a><span data-ttu-id="50521-117">Odchozí připojení a požadavky na DNS</span><span class="sxs-lookup"><span data-stu-id="50521-117">Outbound Connectivity and DNS Requirements</span></span>
<span data-ttu-id="50521-118">Pro App Service Environment toofunction správně, se vyžaduje odchozí přístup toovarious koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="50521-118">For an App Service Environment toofunction properly, it requires outbound access toovarious endpoints.</span></span> <span data-ttu-id="50521-119">Úplný seznam hello externí koncové body používané App Service Environment je v části "Vyžaduje připojení k síti" hello hello [konfiguraci sítě pro ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) článku.</span><span class="sxs-lookup"><span data-stu-id="50521-119">A full list of hello external endpoints used by an ASE is in hello "Required Network Connectivity" section of hello [Network Configuration for ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) article.</span></span>

<span data-ttu-id="50521-120">Prostředí služby App Service vyžaduje platnou konfiguraci pro virtuální síť hello infrastruktury služby DNS.</span><span class="sxs-lookup"><span data-stu-id="50521-120">App Service Environments require a valid DNS infrastructure configured for hello virtual network.</span></span>  <span data-ttu-id="50521-121">Pokud pro všechny důvod hello dojde ke změně konfigurace DNS po vytvoření služby App Service Environment, vývojáři vynutit App Service Environment toopick až hello novou konfiguraci DNS.</span><span class="sxs-lookup"><span data-stu-id="50521-121">If for any reason hello DNS configuration is changed after an App Service Environment has been created, developers can force an App Service Environment toopick up hello new DNS configuration.</span></span>  <span data-ttu-id="50521-122">Aktivuje restartu postupného prostředí pomocí ikony "Restartovat" hello umístěný na začátku hello hello App Service Environment okna Správa portálu hello způsobí hello prostředí toopick až hello novou konfiguraci DNS.</span><span class="sxs-lookup"><span data-stu-id="50521-122">Triggering a rolling environment reboot using hello "Restart" icon located at hello top of hello App Service Environment management blade in hello portal will cause hello environment toopick up hello new DNS configuration.</span></span>

<span data-ttu-id="50521-123">Dále je doporučeno, aby žádné vlastní servery DNS v hello virtuální síti vnet se instalační program před čas předchozí toocreating služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="50521-123">It is also recommended that any custom DNS servers on hello vnet be setup ahead of time prior toocreating an App Service Environment.</span></span>  <span data-ttu-id="50521-124">Pokud je při vytváření služby App Service Environment se změnila konfigurace DNS virtuální sítě, které způsobí selhání procesu vytvoření služby App Service Environment hello.</span><span class="sxs-lookup"><span data-stu-id="50521-124">If a virtual network's DNS configuration is changed while an App Service Environment is being created, that will result in hello App Service Environment creation process failing.</span></span>  <span data-ttu-id="50521-125">V souvislosti podobnou Pokud vlastní server DNS existuje na hello druhém konci VPN gateway a hello DNS server je nedostupná nebo není k dispozici, hello selžou i proces vytvoření služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="50521-125">In a similar vein, if a custom DNS server exists on hello other end of a VPN gateway, and hello DNS server is unreachable or unavailable, hello App Service Environment creation process will also fail.</span></span>

## <a name="connecting-tooa-sql-server"></a><span data-ttu-id="50521-126">Připojení tooa systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="50521-126">Connecting tooa SQL Server</span></span>
<span data-ttu-id="50521-127">Obvyklé konfigurace systému SQL Server má koncový bod naslouchání na portu 1433:</span><span class="sxs-lookup"><span data-stu-id="50521-127">A common SQL Server configuration has an endpoint listening on port 1433:</span></span>

![Koncový bod serveru SQL][SqlServerEndpoint]

<span data-ttu-id="50521-129">Existují dva přístupy pro omezení provozu toothis koncový bod:</span><span class="sxs-lookup"><span data-stu-id="50521-129">There are two approaches for restricting traffic toothis endpoint:</span></span>

* <span data-ttu-id="50521-130">[Sítě seznamy řízení přístupu] [ NetworkAccessControlLists] (sítě ACL)</span><span class="sxs-lookup"><span data-stu-id="50521-130">[Network Access Control Lists][NetworkAccessControlLists] (Network ACLs)</span></span>
* <span data-ttu-id="50521-131">[Skupiny zabezpečení sítě][NetworkSecurityGroups]</span><span class="sxs-lookup"><span data-stu-id="50521-131">[Network Security Groups][NetworkSecurityGroups]</span></span>

## <a name="restricting-access-with-a-network-acl"></a><span data-ttu-id="50521-132">Omezení přístupu k síti seznamu ACL</span><span class="sxs-lookup"><span data-stu-id="50521-132">Restricting Access With a Network ACL</span></span>
<span data-ttu-id="50521-133">Port 1433 lze zabezpečit pomocí seznamu řízení přístupu síti.</span><span class="sxs-lookup"><span data-stu-id="50521-133">Port 1433 can be secured using a network access control list.</span></span>  <span data-ttu-id="50521-134">Příklad Hello níže povolených programů klienta adres pocházející z uvnitř virtuální sítě a blokuje přístup k tooall ostatní klienty.</span><span class="sxs-lookup"><span data-stu-id="50521-134">hello example below whitelists client addresses originating from inside of a virtual network, and blocks access tooall other clients.</span></span>

![Příklad seznamu řízení přístupu sítě][NetworkAccessControlListExample]

<span data-ttu-id="50521-136">Všechny aplikace spuštěné v App Service Environment v hello stejné virtuální síti jako hello systému SQL Server bude instance systému SQL Server nemůže tooconnect toohello pomocí hello **interní virtuální síť** IP adresu pro virtuální počítač systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="50521-136">Any applications running in App Service Environment in hello same virtual network as hello SQL Server will be able tooconnect toohello SQL Server instance using hello **VNet internal** IP address for hello SQL Server virtual machine.</span></span>  

<span data-ttu-id="50521-137">Hello příklad připojovacího řetězce následující odkazy hello systému SQL Server pomocí jeho privátní IP adresy.</span><span class="sxs-lookup"><span data-stu-id="50521-137">hello example connection string below references hello SQL Server using its private IP address.</span></span>

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

<span data-ttu-id="50521-138">I když hello virtuální počítač má také veřejný koncový bod, pokusy o připojení pomocí hello veřejnou IP adresu se odmítne kvůli hello sítě ACL.</span><span class="sxs-lookup"><span data-stu-id="50521-138">Although hello virtual machine has a public endpoint as well, connection attempts using hello public IP address will be rejected because of hello network ACL.</span></span> 

## <a name="restricting-access-with-a-network-security-group"></a><span data-ttu-id="50521-139">Omezení přístupu ke skupině zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="50521-139">Restricting Access With a Network Security Group</span></span>
<span data-ttu-id="50521-140">Alternativní způsob pro přístup k zabezpečení je skupina zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="50521-140">An alternative approach for securing access is with a network security group.</span></span>  <span data-ttu-id="50521-141">Skupiny zabezpečení sítě může být použité tooindividual virtuálních počítačů nebo tooa podsíť obsahující virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="50521-141">Network security groups can be applied tooindividual virtual machines, or tooa subnet containing virtual machines.</span></span>

<span data-ttu-id="50521-142">První skupinu zabezpečení sítě potřebuje toobe vytvořit:</span><span class="sxs-lookup"><span data-stu-id="50521-142">First a network security group needs toobe created:</span></span>

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

<span data-ttu-id="50521-143">Omezení přístupu tooonly interní provoz VNet je velmi jednoduchý ke skupině zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="50521-143">Restricting access tooonly VNet internal traffic is very simple with a network security group.</span></span>  <span data-ttu-id="50521-144">výchozí pravidla Hello ve skupině zabezpečení sítě pouze povolit přístup z jiných klientů sítě v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="50521-144">hello default rules in a network security group only allow access from other network clients in hello same virtual network.</span></span>

<span data-ttu-id="50521-145">V důsledku zamykání přístup tooSQL Server je stejně jednoduché jako použití skupinu zabezpečení sítě s jeho výchozí pravidla tooeither hello virtuální počítače s SQL serverem nebo hello podsíť obsahující hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="50521-145">As a result locking down access tooSQL Server is as simple as applying a network security group with its default rules tooeither hello virtual machines running SQL Server, or hello subnet containing hello virtual machines.</span></span>

<span data-ttu-id="50521-146">Následující ukázka Hello platí zabezpečení skupiny toohello obsahující podsíť sítě:</span><span class="sxs-lookup"><span data-stu-id="50521-146">hello sample below applies a network security group toohello containing subnet:</span></span>

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'

<span data-ttu-id="50521-147">Konečný výsledek Hello je sada pravidel zabezpečení, která blokují externí přístup při povolení přístupu interní virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="50521-147">hello end result is a set of security rules that block external access, while allowing VNet internal access:</span></span>

![Výchozí pravidla zabezpečení sítě][DefaultNetworkSecurityRules]

## <a name="getting-started"></a><span data-ttu-id="50521-149">Začínáme</span><span class="sxs-lookup"><span data-stu-id="50521-149">Getting started</span></span>
<span data-ttu-id="50521-150">Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="50521-150">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="50521-151">tooget začít s prostředí App Service najdete v části [Úvod tooApp Service Environment][IntroToAppServiceEnvironment]</span><span class="sxs-lookup"><span data-stu-id="50521-151">tooget started with App Service Environments, see [Introduction tooApp Service Environment][IntroToAppServiceEnvironment]</span></span>

<span data-ttu-id="50521-152">Podrobnosti ohledně ovládání tooyour příchozí komunikaci služby App Service Environment najdete v tématu [řízení tooan příchozí komunikaci služby App Service Environment][ControlInboundASE]</span><span class="sxs-lookup"><span data-stu-id="50521-152">For details around controlling inbound traffic tooyour App Service Environment, see [Controlling inbound traffic tooan App Service Environment][ControlInboundASE]</span></span>

<span data-ttu-id="50521-153">Další informace o platformě Azure App Service hello najdete v tématu [Azure App Service][AzureAppService].</span><span class="sxs-lookup"><span data-stu-id="50521-153">For more information about hello Azure App Service platform, see [Azure App Service][AzureAppService].</span></span>

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
