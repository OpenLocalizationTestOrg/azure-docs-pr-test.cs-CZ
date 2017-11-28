---
title: "aaaLayered Architektura zabezpečení s prostředí App Service"
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
ms.openlocfilehash: 0627ba6fa849908506fe62c451c888c147cabc03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a><span data-ttu-id="bfae8-103">Implementace architektura vrstveného zabezpečení pomocí služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="bfae8-103">Implementing a Layered Security Architecture with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="bfae8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="bfae8-104">Overview</span></span>
<span data-ttu-id="bfae8-105">Vzhledem k tomu, že prostředí App Service poskytuje izolovaném prostředí nasadit do virtuální sítě, vývojáři můžou vytvářet architektura vrstveného zabezpečení poskytuje různé úrovně přístupu k síti pro každý fyzický aplikační vrstvě.</span><span class="sxs-lookup"><span data-stu-id="bfae8-105">Since App Service Environments provide an isolated runtime environment deployed into a virtual network, developers can create a layered security architecture providing differing levels of network access for each physical application tier.</span></span>

<span data-ttu-id="bfae8-106">Společné přání je toohide rozhraní API back EndY z obecné přístup k Internetu a povolit pouze toobe rozhraní API, které jsou volány nadřazeného webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="bfae8-106">A common desire is toohide API back-ends from general Internet access, and only allow APIs toobe called by upstream web apps.</span></span>  <span data-ttu-id="bfae8-107">[Skupin zabezpečení (Nsg) sítě] [ NetworkSecurityGroups] jde použít na podsítě, který obsahuje prostředí App Service toorestrict veřejný přístup tooAPI aplikace.</span><span class="sxs-lookup"><span data-stu-id="bfae8-107">[Network security groups (NSGs)][NetworkSecurityGroups] can be used on subnets containing App Service Environments toorestrict public access tooAPI applications.</span></span>

<span data-ttu-id="bfae8-108">Hello následující diagram ukazuje příklad architekturu a WebAPI na základě aplikaci nasadit na služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="bfae8-108">hello diagram below shows an example architecture with a WebAPI based app deployed on an App Service Environment.</span></span>  <span data-ttu-id="bfae8-109">Tři instance aplikace samostatný webový nasazené na tři samostatné prostředí App Service, ujistěte se, back-end volání toohello stejné WebAPI aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bfae8-109">Three separate web app instances, deployed on three separate App Service Environments, make back-end calls toohello same WebAPI app.</span></span>

![Konceptuální architektura][ConceptualArchitecture] 

<span data-ttu-id="bfae8-111">Hello zeleného znaménka plus znamenat, že hello skupinu zabezpečení sítě v podsíti hello obsahující "apiase" nadřazeného webové aplikace, umožňuje příchozí volání z hello jako dobře volání ze sebe samotné.</span><span class="sxs-lookup"><span data-stu-id="bfae8-111">hello green plus signs indicate that hello network security group on hello subnet containing "apiase" allows inbound calls from hello upstream web apps, as well calls from itself.</span></span>  <span data-ttu-id="bfae8-112">Ale hello stejnou skupinu zabezpečení sítě výslovně odepře přístup toogeneral příchozí provoz z Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="bfae8-112">However hello same network security group explicitly denies access toogeneral inbound traffic from hello Internet.</span></span> 

<span data-ttu-id="bfae8-113">Hello zbývající část tohoto tématu provede hello kroky potřebné tooconfigure skupinu zabezpečení sítě hello v podsíti hello obsahující "apiase".</span><span class="sxs-lookup"><span data-stu-id="bfae8-113">hello remainder of this topic walks through hello steps needed tooconfigure hello network security group on hello subnet containing "apiase".</span></span>

## <a name="determining-hello-network-behavior"></a><span data-ttu-id="bfae8-114">Určení hello chování sítě</span><span class="sxs-lookup"><span data-stu-id="bfae8-114">Determining hello Network Behavior</span></span>
<span data-ttu-id="bfae8-115">Pořadí tooknow jaké zabezpečení sítě, které jsou potřebné pravidla, je nutné toodetermine, kteří klienti sítě bude možné tooreach hello App Service Environment obsahující hello aplikace API a které klienti budou Blokovaní.</span><span class="sxs-lookup"><span data-stu-id="bfae8-115">In order tooknow what network security rules are needed, you need toodetermine which network clients will be allowed tooreach hello App Service Environment containing hello API app, and which clients will be blocked.</span></span>

<span data-ttu-id="bfae8-116">Vzhledem k tomu [skupin zabezpečení (Nsg) sítě] [ NetworkSecurityGroups] jsou použité toosubnets a prostředí App Service jsou nasazeny do podsítí, platí pravidla hello obsažené v skupinu NSG příliš**všechny** aplikace běžící na služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="bfae8-116">Since [network security groups (NSGs)][NetworkSecurityGroups] are applied toosubnets, and App Service Environments are deployed into subnets, hello rules contained in an NSG apply too**all** apps running on an App Service Environment.</span></span>  <span data-ttu-id="bfae8-117">Pomocí hello ukázková architektura v tomto článku, jakmile skupinu zabezpečení sítě je použité toohello podsíť obsahující všechny aplikace běžící na hello "apiase" App Service Environment bude chráněn hello "apiase" stejné sady pravidel zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="bfae8-117">Using hello sample architecture for this article, once a network security group is applied toohello subnet containing "apiase", all apps running on hello "apiase" App Service Environment will be protected by hello same set of security rules.</span></span> 

* <span data-ttu-id="bfae8-118">**Hello odchozí IP adresu z nadřazeného volajícím určit:** co je hello IP adresu nebo adresy nadřazeného volající hello?</span><span class="sxs-lookup"><span data-stu-id="bfae8-118">**Determine hello outbound IP address of upstream callers:**  What is hello IP address or addresses of hello upstream callers?</span></span>  <span data-ttu-id="bfae8-119">Tyto adresy potřebovat toobe explicitně povolen přístup v hello NSG.</span><span class="sxs-lookup"><span data-stu-id="bfae8-119">These addresses will need toobe explicitly allowed access in hello NSG.</span></span>  <span data-ttu-id="bfae8-120">Vzhledem k tomu, že volání mezi prostředí App Service jsou považovány za volání "Internet", znamená to hello odchozí IP adresu přiřazenou tooeach z hello tři nadřazeného prostředí App Service musí toobe přistupovat v hello skupina NSG pro podsíť "apiase" hello.</span><span class="sxs-lookup"><span data-stu-id="bfae8-120">Since calls between App Service Environments are considered "Internet" calls, this means hello outbound IP address assigned tooeach of hello three upstream App Service Environments needs toobe allowed access in hello NSG for hello "apiase" subnet.</span></span>   <span data-ttu-id="bfae8-121">Další informace o určení hello odchozí IP adresu, aplikace běžící ve službě App Service Environment, najdete v tématu hello [síťovou architekturu] [ NetworkArchitecture] článek s přehledem.</span><span class="sxs-lookup"><span data-stu-id="bfae8-121">For more details on determining hello outbound IP address for apps running in an App Service Environment see hello [Network Architecture][NetworkArchitecture] Overview article.</span></span>
* <span data-ttu-id="bfae8-122">**Aplikace API back-end hello potřebovat toocall samotné?**</span><span class="sxs-lookup"><span data-stu-id="bfae8-122">**Will hello back-end API app need toocall itself?**</span></span>  <span data-ttu-id="bfae8-123">Někdy přehlíženým a jemně bod je hello scénář, kde hello back-end aplikace potřebuje toocall sám sebe.</span><span class="sxs-lookup"><span data-stu-id="bfae8-123">A sometimes overlooked and subtle point is hello scenario where hello back-end application needs toocall itself.</span></span>  <span data-ttu-id="bfae8-124">Pokud se aplikace API back-end na služby App Service Environment musí toocall sám sebe, je také zpracovaná jako volání na "Internet".</span><span class="sxs-lookup"><span data-stu-id="bfae8-124">If a back-end API application on an App Service Environment needs toocall itself, this is also treated as an "Internet" call.</span></span>  <span data-ttu-id="bfae8-125">V hello ukázková architektura to vyžaduje povolení přístupu z hello odchozí IP adresy hello "apiase" App Service Environment také.</span><span class="sxs-lookup"><span data-stu-id="bfae8-125">In hello sample architecture this requires allowing access from hello outbound IP address of hello "apiase" App Service Environment as well.</span></span>

## <a name="setting-up-hello-network-security-group"></a><span data-ttu-id="bfae8-126">Nastavení hello skupinu zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="bfae8-126">Setting up hello Network Security Group</span></span>
<span data-ttu-id="bfae8-127">Jakmile sada hello odchozí IP adresy jsou známé, hello dalším krokem je tooconstruct skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="bfae8-127">Once hello set of outbound IP addresses are known, hello next step is tooconstruct a network security group.</span></span>  <span data-ttu-id="bfae8-128">Skupiny zabezpečení sítě lze vytvořit pro virtuální sítě, jakož i klasické virtuální sítě na základě i Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="bfae8-128">Network security groups can be created for both Resource Manager based virtual networks, as well as classic virtual networks.</span></span>  <span data-ttu-id="bfae8-129">Následující příklady Hello ukazují, vytvoření a konfiguraci skupiny NSG na klasické virtuální sítě pomocí prostředí Powershell.</span><span class="sxs-lookup"><span data-stu-id="bfae8-129">hello examples below show creating and configuring an NSG on a classic virtual network using Powershell.</span></span>

<span data-ttu-id="bfae8-130">Pro hello ukázkové architektury hello prostředí jsou umístěny v jihu USA, takže prázdné skupiny NSG se vytvoří v této oblasti:</span><span class="sxs-lookup"><span data-stu-id="bfae8-130">For hello sample architecture, hello environments are located in South Central US, so an empty NSG is created in that region:</span></span>

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

<span data-ttu-id="bfae8-131">Nejprve explicitního Povolit pravidlo se přidá infrastruktury Azure správy hello uvedených v článku hello na [příchozí provoz] [ InboundTraffic] pro prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="bfae8-131">First an explicit allow rule is added for hello Azure management infrastructure as noted in hello article on [inbound traffic][InboundTraffic] for App Service Environments.</span></span>

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

<span data-ttu-id="bfae8-132">V dalším kroku dvě pravidla se přidají tooallow HTTP a HTTPS volání z hello první nadřazeného App Service Environment ("fe1ase").</span><span class="sxs-lookup"><span data-stu-id="bfae8-132">Next, two rules are added tooallow HTTP and HTTPS calls from hello first upstream App Service Environment ("fe1ase").</span></span>

    #Grant access toorequests from hello first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="bfae8-133">Omývá a postup opakujte pro hello druhý a třetí nadřazeného prostředí App Service ("fe2ase" a "fe3ase").</span><span class="sxs-lookup"><span data-stu-id="bfae8-133">Rinse and repeat for hello second and third upstream App Service Environments ("fe2ase"and "fe3ase").</span></span>

    #Grant access toorequests from hello second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access toorequests from hello third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="bfae8-134">Nakonec udělte přístup toohello odchozí IP adresu hello back-end API App Service Environment, aby můžete volat zpět do sebe sama.</span><span class="sxs-lookup"><span data-stu-id="bfae8-134">Lastly, grant access toohello outbound IP address of hello back-end API's App Service Environment so that it can call back into itself.</span></span>

    #Allow apps on hello apiase environment toocall back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

<span data-ttu-id="bfae8-135">Žádná další pravidla zabezpečení sítě je nutné toobe nakonfigurovat, protože každý NSG obsahuje sadu výchozích pravidel, která blokují příchozí přístup z hello Internetu ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="bfae8-135">No other network security rules need toobe configured because every NSG has a set of default rules that block inbound access from hello Internet by default.</span></span>

<span data-ttu-id="bfae8-136">Níže jsou uvedeny Hello úplný seznam pravidel v hello skupinu zabezpečení sítě.</span><span class="sxs-lookup"><span data-stu-id="bfae8-136">hello full list of rules in hello network security group are shown below.</span></span>  <span data-ttu-id="bfae8-137">Všimněte si, jak hello poslední pravidlo, které zvýrazní, blokuje příchozí přístup ze všech volající kromě těch, které mají výslovně udělil přístup.</span><span class="sxs-lookup"><span data-stu-id="bfae8-137">Note how hello last rule, which is highlighted, blocks inbound access from all callers other than those which have been explicitly granted access.</span></span>

![Konfigurace skupiny NSG][NSGConfiguration] 

<span data-ttu-id="bfae8-139">posledním krokem Hello je tooapply hello NSG toohello podsítě, která obsahuje hello "apiase" App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="bfae8-139">hello final step is tooapply hello NSG toohello subnet that contains hello "apiase" App Service Environment.</span></span>  

     #Apply hello NSG toohello backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

<span data-ttu-id="bfae8-140">S podsítí toohello hello skupina NSG se použije hello tři nadřazeného prostředí App Service a hello App Service Environment obsahující hello rozhraní API back-end, jsou povoleny pouze toocall do prostředí "apiase" hello.</span><span class="sxs-lookup"><span data-stu-id="bfae8-140">With hello NSG applied toohello subnet, only hello three upstream App Service Environments, and hello App Service Environment containing hello API back-end, are allowed toocall into hello "apiase" environment.</span></span>

## <a name="additional-links-and-information"></a><span data-ttu-id="bfae8-141">Další odkazy a informace</span><span class="sxs-lookup"><span data-stu-id="bfae8-141">Additional Links and Information</span></span>
<span data-ttu-id="bfae8-142">Všechny články a jak – do této pro App Service Environment jsou dostupné v hello [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="bfae8-142">All articles and How-To's for App Service Environments are available in hello [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="bfae8-143">Informace o [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="bfae8-143">Information about [network security groups](../virtual-network/virtual-networks-nsg.md).</span></span> 

<span data-ttu-id="bfae8-144">Principy [odchozí IP adresy] [ NetworkArchitecture] a prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="bfae8-144">Understanding [outbound IP addresses][NetworkArchitecture] and App Service Environments.</span></span>

<span data-ttu-id="bfae8-145">[Síťové porty] [ InboundTraffic] používá prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="bfae8-145">[Network ports][InboundTraffic] used by App Service Environments.</span></span>

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
