---
title: "Architektura vrstveného zabezpečení pomocí služby App Service Environment"
description: "Implementace architektura vrstveného zabezpečení s prostředí App Service."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 0fb02c13f99a8f4a46e0142c20da3b152c809b6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a><span data-ttu-id="1c636-103">Implementace architektura vrstveného zabezpečení pomocí služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="1c636-103">Implementing a Layered Security Architecture with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="1c636-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1c636-104">Overview</span></span>
<span data-ttu-id="1c636-105">Vzhledem k tomu, že prostředí App Service poskytuje izolovaném prostředí nasadit do virtuální sítě, vývojáři můžou vytvářet architektura vrstveného zabezpečení poskytuje různé úrovně přístupu k síti pro každý fyzický aplikační vrstvě.</span><span class="sxs-lookup"><span data-stu-id="1c636-105">Since App Service Environments provide an isolated runtime environment deployed into a virtual network, developers can create a layered security architecture providing differing levels of network access for each physical application tier.</span></span>

<span data-ttu-id="1c636-106">Společné přání je skrýt rozhraní API back EndY z obecné přístup k Internetu a Povolit jenom rozhraní API, která se má volat nadřazeného webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1c636-106">A common desire is to hide API back-ends from general Internet access, and only allow APIs to be called by upstream web apps.</span></span>  <span data-ttu-id="1c636-107">[Skupin zabezpečení (Nsg) sítě] [ NetworkSecurityGroups] lze použít na podsítě, který obsahuje prostředí App Service omezit veřejný přístup k aplikacím API.</span><span class="sxs-lookup"><span data-stu-id="1c636-107">[Network security groups (NSGs)][NetworkSecurityGroups] can be used on subnets containing App Service Environments to restrict public access to API applications.</span></span>

<span data-ttu-id="1c636-108">Následující diagram ukazuje příklad architekturu a WebAPI na základě aplikaci nasadit na služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="1c636-108">The diagram below shows an example architecture with a WebAPI based app deployed on an App Service Environment.</span></span>  <span data-ttu-id="1c636-109">Tři instance aplikace samostatný webový nasazené na tři samostatné prostředí App Service, volat back-end ke stejné aplikaci WebAPI.</span><span class="sxs-lookup"><span data-stu-id="1c636-109">Three separate web app instances, deployed on three separate App Service Environments, make back-end calls to the same WebAPI app.</span></span>

![Konceptuální architektura][ConceptualArchitecture] 

<span data-ttu-id="1c636-111">Zeleného znaménka plus indikovat, skupiny zabezpečení sítě na podsíť obsahující "apiase" umožňuje příchozí volání z nadřazeného webové aplikace, jako dobře volání ze sebe samotné.</span><span class="sxs-lookup"><span data-stu-id="1c636-111">The green plus signs indicate that the network security group on the subnet containing "apiase" allows inbound calls from the upstream web apps, as well calls from itself.</span></span>  <span data-ttu-id="1c636-112">Ale stejnou skupinu zabezpečení sítě výslovně odepře přístup k obecné příchozí provoz z Internetu.</span><span class="sxs-lookup"><span data-stu-id="1c636-112">However the same network security group explicitly denies access to general inbound traffic from the Internet.</span></span> 

<span data-ttu-id="1c636-113">Zbývající část tohoto tématu vás provede kroky potřebné ke konfiguraci skupinu zabezpečení sítě na podsíť obsahující "apiase".</span><span class="sxs-lookup"><span data-stu-id="1c636-113">The remainder of this topic walks through the steps needed to configure the network security group on the subnet containing "apiase".</span></span>

## <a name="determining-the-network-behavior"></a><span data-ttu-id="1c636-114">Určení chování sítě</span><span class="sxs-lookup"><span data-stu-id="1c636-114">Determining the Network Behavior</span></span>
<span data-ttu-id="1c636-115">Chcete-li vědět, jaká pravidla zabezpečení sítě jsou potřeba, budete muset určit, kteří klienti sítě se bude moct připojit obsahující aplikace API App Service Environment a které klienti budou Blokovaní.</span><span class="sxs-lookup"><span data-stu-id="1c636-115">In order to know what network security rules are needed, you need to determine which network clients will be allowed to reach the App Service Environment containing the API app, and which clients will be blocked.</span></span>

<span data-ttu-id="1c636-116">Vzhledem k tomu [skupin zabezpečení (Nsg) sítě] [ NetworkSecurityGroups] se použijí k podsítím a prostředí App Service jsou nasazené do podsítí, platí pravidla obsažené v skupinu NSG k **všechny** aplikace běžící na služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="1c636-116">Since [network security groups (NSGs)][NetworkSecurityGroups] are applied to subnets, and App Service Environments are deployed into subnets, the rules contained in an NSG apply to **all** apps running on an App Service Environment.</span></span>  <span data-ttu-id="1c636-117">Použití ukázkové architektury pro tohoto článku, jakmile skupinu zabezpečení sítě se použije na podsíť obsahující "apiase", budou všechny aplikace běžící na App Service Environment "apiase" chráněné stejná sada pravidel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="1c636-117">Using the sample architecture for this article, once a network security group is applied to the subnet containing "apiase", all apps running on the "apiase" App Service Environment will be protected by the same set of security rules.</span></span> 

* <span data-ttu-id="1c636-118">**Odchozí IP adresu nadřazeného volajícím určit:** co je IP adresa nebo adresy nadřazeného volající?</span><span class="sxs-lookup"><span data-stu-id="1c636-118">**Determine the outbound IP address of upstream callers:**  What is the IP address or addresses of the upstream callers?</span></span>  <span data-ttu-id="1c636-119">Tyto adresy bude muset být explicitně povolené přístup v této skupině.</span><span class="sxs-lookup"><span data-stu-id="1c636-119">These addresses will need to be explicitly allowed access in the NSG.</span></span>  <span data-ttu-id="1c636-120">Vzhledem k tomu, že volání mezi prostředí App Service jsou považovány za "Internet" volání, to znamená, že odchozí IP adresu přiřazenou každému tři nadřazeného prostředí App Service musí mít přístup povolený skupina NSG pro podsíť "apiase".</span><span class="sxs-lookup"><span data-stu-id="1c636-120">Since calls between App Service Environments are considered "Internet" calls, this means the outbound IP address assigned to each of the three upstream App Service Environments needs to be allowed access in the NSG for the "apiase" subnet.</span></span>   <span data-ttu-id="1c636-121">Další informace o určení odchozí IP adresu, najdete v tématu aplikace běžící ve službě App Service Environment [síťovou architekturu] [ NetworkArchitecture] článek s přehledem.</span><span class="sxs-lookup"><span data-stu-id="1c636-121">For more details on determining the outbound IP address for apps running in an App Service Environment see the [Network Architecture][NetworkArchitecture] Overview article.</span></span>
* <span data-ttu-id="1c636-122">**Bude potřeba back-end aplikace API volat sám sebe?**</span><span class="sxs-lookup"><span data-stu-id="1c636-122">**Will the back-end API app need to call itself?**</span></span>  <span data-ttu-id="1c636-123">Někdy přehlíženým a jemně bod je tento scénář, kde back-end aplikace musí volat sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1c636-123">A sometimes overlooked and subtle point is the scenario where the back-end application needs to call itself.</span></span>  <span data-ttu-id="1c636-124">Pokud se aplikace API back-end na služby App Service Environment musí volat sám sebe, je také zpracovaná jako volání na "Internet".</span><span class="sxs-lookup"><span data-stu-id="1c636-124">If a back-end API application on an App Service Environment needs to call itself, this is also treated as an "Internet" call.</span></span>  <span data-ttu-id="1c636-125">Ukázková architektura to vyžaduje povolení přístupu z odchozí adresy IP "apiase" App Service Environment také.</span><span class="sxs-lookup"><span data-stu-id="1c636-125">In the sample architecture this requires allowing access from the outbound IP address of the "apiase" App Service Environment as well.</span></span>

## <a name="setting-up-the-network-security-group"></a><span data-ttu-id="1c636-126">Nastavení skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="1c636-126">Setting up the Network Security Group</span></span>
<span data-ttu-id="1c636-127">Jakmile jsou známé sadu odchozí IP adres, dalším krokem je vytvořit skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1c636-127">Once the set of outbound IP addresses are known, the next step is to construct a network security group.</span></span>  <span data-ttu-id="1c636-128">Skupiny zabezpečení sítě lze vytvořit pro virtuální sítě, jakož i klasické virtuální sítě na základě i Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1c636-128">Network security groups can be created for both Resource Manager based virtual networks, as well as classic virtual networks.</span></span>  <span data-ttu-id="1c636-129">Následující příklady ukazují, vytvoření a konfiguraci skupiny NSG na klasické virtuální sítě pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="1c636-129">The examples below show creating and configuring an NSG on a classic virtual network using Powershell.</span></span>

<span data-ttu-id="1c636-130">Pro ukázkové architektury prostředí se nacházíte, Jižní střední USA, aby prázdné skupiny NSG se vytvoří v této oblasti:</span><span class="sxs-lookup"><span data-stu-id="1c636-130">For the sample architecture, the environments are located in South Central US, so an empty NSG is created in that region:</span></span>

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

<span data-ttu-id="1c636-131">Nejprve explicitního Povolit pravidlo je přidáno pro správu Azure infrastrukturu, jak je uvedeno v následujícím článku na [příchozí provoz] [ InboundTraffic] pro prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="1c636-131">First an explicit allow rule is added for the Azure management infrastructure as noted in the article on [inbound traffic][InboundTraffic] for App Service Environments.</span></span>

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

<span data-ttu-id="1c636-132">V dalším kroku dvě pravidla jsou přidány do povolte volání protokolu HTTP a HTTPS z první nadřazeného App Service Environment ("fe1ase").</span><span class="sxs-lookup"><span data-stu-id="1c636-132">Next, two rules are added to allow HTTP and HTTPS calls from the first upstream App Service Environment ("fe1ase").</span></span>

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="1c636-133">Vypláchněte a opakujte pro druhý a třetí nadřazeného prostředí App Service ("fe2ase" a "fe3ase").</span><span class="sxs-lookup"><span data-stu-id="1c636-133">Rinse and repeat for the second and third upstream App Service Environments ("fe2ase"and "fe3ase").</span></span>

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="1c636-134">Nakonec udělte přístup k odchozí IP adresu rozhraní API back-end App Service Environment, aby můžete volat zpět do sebe sama.</span><span class="sxs-lookup"><span data-stu-id="1c636-134">Lastly, grant access to the outbound IP address of the back-end API's App Service Environment so that it can call back into itself.</span></span>

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="1c636-135">Žádná další pravidla zabezpečení sítě je nutné nakonfigurovat, protože každý NSG obsahuje sadu výchozích pravidel, která blokují příchozí přístup z Internetu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1c636-135">No other network security rules need to be configured because every NSG has a set of default rules that block inbound access from the Internet by default.</span></span>

<span data-ttu-id="1c636-136">Níže jsou uvedeny úplný seznam pravidel ve skupině zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="1c636-136">The full list of rules in the network security group are shown below.</span></span>  <span data-ttu-id="1c636-137">Všimněte si, jak poslední pravidlo, které zvýrazní, blokuje příchozí přístup ze všech volající kromě těch, které mají výslovně udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="1c636-137">Note how the last rule, which is highlighted, blocks inbound access from all callers other than those which have been explicitly granted access.</span></span>

![Konfigurace skupiny NSG][NSGConfiguration] 

<span data-ttu-id="1c636-139">Posledním krokem je použít NSG k podsíti, který obsahuje "apiase" App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="1c636-139">The final step is to apply the NSG to the subnet that contains the "apiase" App Service Environment.</span></span>  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

<span data-ttu-id="1c636-140">S NSG použije na podsíť jsou povoleny pouze tři nadřazeného prostředí App Service a služba App Service Environment obsahující rozhraní API back-end, provést volání do prostředí "apiase".</span><span class="sxs-lookup"><span data-stu-id="1c636-140">With the NSG applied to the subnet, only the three upstream App Service Environments, and the App Service Environment containing the API back-end, are allowed to call into the "apiase" environment.</span></span>

## <a name="additional-links-and-information"></a><span data-ttu-id="1c636-141">Další odkazy a informace</span><span class="sxs-lookup"><span data-stu-id="1c636-141">Additional Links and Information</span></span>
<span data-ttu-id="1c636-142">Všechny články a jak – do této pro App Service Environment jsou dostupné v [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="1c636-142">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="1c636-143">Informace o [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="1c636-143">Information about [network security groups](../virtual-network/virtual-networks-nsg.md).</span></span> 

<span data-ttu-id="1c636-144">Principy [odchozí IP adresy] [ NetworkArchitecture] a prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="1c636-144">Understanding [outbound IP addresses][NetworkArchitecture] and App Service Environments.</span></span>

<span data-ttu-id="1c636-145">[Síťové porty] [ InboundTraffic] používá prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="1c636-145">[Network ports][InboundTraffic] used by App Service Environments.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
