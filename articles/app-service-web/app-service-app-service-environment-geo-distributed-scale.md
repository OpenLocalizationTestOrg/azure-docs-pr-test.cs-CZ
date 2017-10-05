---
title: "Geograficky distribuované škálování v prostředí App Service Environments"
description: "Zjistěte, jak vodorovně škálování aplikace pomocí Traffic Manageru a prostředí App Service geo rozdělení."
services: app-service
documentationcenter: 
author: stefsch
manager: erikre
editor: 
ms.assetid: c1b05ca8-3703-4d87-a9ae-819d741787fb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: stefsch
ms.openlocfilehash: 505301b2650c9b8bafdad352055f30e55148ab0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="geo-distributed-scale-with-app-service-environments"></a><span data-ttu-id="8cff5-103">Geograficky distribuované škálování v prostředí App Service Environments</span><span class="sxs-lookup"><span data-stu-id="8cff5-103">Geo Distributed Scale with App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="8cff5-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8cff5-104">Overview</span></span>
<span data-ttu-id="8cff5-105">Scénáře aplikací, které vyžadují velmi vysoký škálování může být vyšší než výpočetní kapacitu prostředků, k dispozici pro jedno nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cff5-105">Application scenarios which require very high scale can exceed the compute resource capacity available to a single deployment of an app.</span></span>  <span data-ttu-id="8cff5-106">Hlasování aplikace, sportovní události a události televizní Zábava jsou všechny příklady scénářů, které vyžadují velmi velkém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-106">Voting applications, sporting events, and televised entertainment events are all examples of scenarios that require extremely high scale.</span></span> <span data-ttu-id="8cff5-107">Vodorovně škálování aplikace s více nasazení aplikací pro zpracování extrémně zatížení požadavky prováděné v jedné oblasti, a také v oblastech, můžete splnit požadavky na velkém rozsahu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-107">High scale requirements can be met by horizontally scaling out apps, with multiple app deployments being made within a single region, as well as across regions, to handle extreme load requirements.</span></span>

<span data-ttu-id="8cff5-108">Služby App Service Environment jsou ideální platformu pro horizontální škálování.</span><span class="sxs-lookup"><span data-stu-id="8cff5-108">App Service Environments are an ideal platform for horizontal scale out.</span></span>  <span data-ttu-id="8cff5-109">Jednou služby App Service Environment configuration se vybral podporující rychlost známé požadavků, vývojáři můžete nasadit další prostředí App Service způsobem "soubor cookie ořezávání" aby bylo možné zatížení ve špičce požadovanou kapacitou.</span><span class="sxs-lookup"><span data-stu-id="8cff5-109">Once an App Service Environment configuration has been selected that can support a known request rate, developers can deploy additional App Service Environments in "cookie cutter" fashion to attain a desired peak load capacity.</span></span>

<span data-ttu-id="8cff5-110">Předpokládejme například, že byl testován aplikace spuštěné v konfiguraci služby App Service Environment, zpracování 20 tisíc požadavků za sekundu (RPS).</span><span class="sxs-lookup"><span data-stu-id="8cff5-110">For example suppose an app running on an App Service Environment configuration has been tested to handle 20K requests per second (RPS).</span></span>  <span data-ttu-id="8cff5-111">Pokud je kapacita zatížení ve špičce požadované 100 tisíc RPS, pak pět (5) prostředí App Service lze vytvořit a nakonfigurovat tak, aby Ujistěte se, že aplikace dokáže zvládat maximální předpokládané zatížení.</span><span class="sxs-lookup"><span data-stu-id="8cff5-111">If the desired peak load capacity is 100K RPS, then five (5) App Service Environments can be created and configured to ensure the application can handle the maximum projected load.</span></span>

<span data-ttu-id="8cff5-112">Vzhledem k tomu, že zákazníci obvykle získat přístup k aplikacím pomocí vlastní (nebo jednoduché) domény, vývojáři potřebovat způsob, jak k distribuci požadavků aplikace ve všech instancí služby App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="8cff5-112">Since customers typically access apps using a custom (or vanity) domain, developers need a way to distribute app requests across all of the App Service Environment instances.</span></span>  <span data-ttu-id="8cff5-113">Skvělý způsob, jak se má vyřešit pomocí vlastní domény [profilu Azure Traffic Manageru][AzureTrafficManagerProfile].</span><span class="sxs-lookup"><span data-stu-id="8cff5-113">A great way to accomplish this is to resolve the custom domain using an [Azure Traffic Manager profile][AzureTrafficManagerProfile].</span></span>  <span data-ttu-id="8cff5-114">Profil služby Traffic Manager lze nakonfigurovat tak, aby odkazoval na všechny jednotlivé prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="8cff5-114">The Traffic Manager profile can be configured to point at all of the individual App Service Environments.</span></span>  <span data-ttu-id="8cff5-115">Traffic Manager automaticky zpracuje distribuce zákazníkům napříč všemi prostředí App Service podle nastavení v profilu Správce provozu služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8cff5-115">Traffic Manager will automatically handle distributing customers across all of the App Service Environments based on the load balancing settings in the Traffic Manager profile.</span></span>  <span data-ttu-id="8cff5-116">Tento postup funguje bez ohledu na to, jestli všechny prostředí App Service jsou nasazené v jedné oblasti Azure, nebo nasazeným po celém světě nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="8cff5-116">This approach works regardless of whether all of the App Service Environments are deployed in a single Azure region, or deployed worldwide across multiple Azure regions.</span></span>

<span data-ttu-id="8cff5-117">Navíc vzhledem k tomu, že zákazníkům přístup k aplikacím prostřednictvím domény jednoduché, zákazníci neberou počet prostředí App Service spuštěna aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cff5-117">Furthermore, since customers access apps through the vanity domain, customers are unaware of the number of App Service Environments running an app.</span></span>  <span data-ttu-id="8cff5-118">V důsledku vývojáři můžete rychle a snadno přidat a odebrat, podle zjištěnou přenosové zatížení prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="8cff5-118">As a result developers can quickly and easily add, and remove, App Service Environments based on observed traffic load.</span></span>

<span data-ttu-id="8cff5-119">Následující koncepční diagram znázorňuje aplikace horizontálně škálovat na více systémů mezi tři prostředí App Service v jedné oblasti.</span><span class="sxs-lookup"><span data-stu-id="8cff5-119">The conceptual diagram below depicts an app horizontally scaled out across three App Service Environments within a single region.</span></span>

![Konceptuální architektura][ConceptualArchitecture] 

<span data-ttu-id="8cff5-121">Zbývající část tohoto tématu vás provede kroky s nastavením Distribuovaná topologie pro ukázkové aplikace pomocí několika prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="8cff5-121">The remainder of this topic walks through the steps involved with setting up a distributed topology for the sample app using multiple App Service Environments.</span></span>

## <a name="planning-the-topology"></a><span data-ttu-id="8cff5-122">Plánování topologie</span><span class="sxs-lookup"><span data-stu-id="8cff5-122">Planning the Topology</span></span>
<span data-ttu-id="8cff5-123">Před vytvořením na nároky distribuované aplikace, pomáhá má několik částí informace předem.</span><span class="sxs-lookup"><span data-stu-id="8cff5-123">Before building out a distributed app footprint, it helps to have a few pieces information ahead of time.</span></span>

* <span data-ttu-id="8cff5-124">**Vlastní doména pro aplikaci:** co je název vlastní domény, který zákazníky bude používat pro přístup k aplikaci?</span><span class="sxs-lookup"><span data-stu-id="8cff5-124">**Custom domain for the app:**  What is the custom domain name that customers will use to access the app?</span></span>  <span data-ttu-id="8cff5-125">Pro ukázkovou aplikaci vlastní název domény je *www.scalableasedemo.com*</span><span class="sxs-lookup"><span data-stu-id="8cff5-125">For the sample app the custom domain name is *www.scalableasedemo.com*</span></span>
* <span data-ttu-id="8cff5-126">**Doménu Traffic Manageru:** název domény musí být zvolena při vytváření [profilu Azure Traffic Manageru][AzureTrafficManagerProfile].</span><span class="sxs-lookup"><span data-stu-id="8cff5-126">**Traffic Manager domain:**  A domain name needs to be chosen when creating an [Azure Traffic Manager profile][AzureTrafficManagerProfile].</span></span>  <span data-ttu-id="8cff5-127">Tento název bude sloučen s *trafficmanager.net* příponu zaregistrovat položku domény, který je spravován nástrojem Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="8cff5-127">This name will be combined with the *trafficmanager.net* suffix to register a domain entry that is managed by Traffic Manager.</span></span>  <span data-ttu-id="8cff5-128">Pro ukázkovou aplikaci, je zvolený název *škálovatelné App Service Environment ukázku*.</span><span class="sxs-lookup"><span data-stu-id="8cff5-128">For the sample app, the name chosen is *scalable-ase-demo*.</span></span>  <span data-ttu-id="8cff5-129">Výsledkem je název úplné domény, který je spravován nástrojem Traffic Manager *škálovatelné App Service Environment demo.trafficmanager.net*.</span><span class="sxs-lookup"><span data-stu-id="8cff5-129">As a result the full domain name that is managed by Traffic Manager is *scalable-ase-demo.trafficmanager.net*.</span></span>
* <span data-ttu-id="8cff5-130">**Strategie pro škálování aplikací nároky:** budou nároky na aplikace distribuována mezi více prostředí App Service v jedné oblasti?</span><span class="sxs-lookup"><span data-stu-id="8cff5-130">**Strategy for scaling the app footprint:**  Will the application footprint be distributed across multiple App Service Environments in a single region?</span></span>  <span data-ttu-id="8cff5-131">Více oblastí?</span><span class="sxs-lookup"><span data-stu-id="8cff5-131">Multiple regions?</span></span>  <span data-ttu-id="8cff5-132">Kombinaci a match obou přístupů?</span><span class="sxs-lookup"><span data-stu-id="8cff5-132">A mix-and-match of both approaches?</span></span>  <span data-ttu-id="8cff5-133">Rozhodnutí by měla být založena na očekávání, kde budou pocházet provozu zákazníka a také jak dobře můžete škálovat zbytek aplikace podpůrná infrastruktura back-end.</span><span class="sxs-lookup"><span data-stu-id="8cff5-133">The decision should be based on expectations of where customer traffic will originate, as well as how well the rest of an app's supporting back-end infrastructure can scale.</span></span>  <span data-ttu-id="8cff5-134">Například s 100 % bezstavové aplikace, aplikace je možné massively rozšířit pomocí kombinace více prostředí App Service na oblast Azure, násobenou prostředí App Service nasadit nad několika oblastmi Azure.</span><span class="sxs-lookup"><span data-stu-id="8cff5-134">For example, with a 100% stateless application, an app can be massively scaled using a combination of multiple App Service Environments per Azure region,  multiplied by App Service Environments deployed across multiple Azure regions.</span></span>  <span data-ttu-id="8cff5-135">S 15 + veřejných oblastí Azure můžete vybrat z skutečně zákazníci mohou vytvářet nároky celém světě flexibilně škálovatelné aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cff5-135">With 15+ public Azure regions available to choose from, customers can truly build a world-wide hyper-scale application footprint.</span></span>  <span data-ttu-id="8cff5-136">Ukázková aplikace používá pro tento článek byly vytvořeny tři prostředí App Service v jedné oblasti Azure (Jižní střední USA).</span><span class="sxs-lookup"><span data-stu-id="8cff5-136">For the sample app used for this article, three App Service Environments were created in a single Azure region (South Central US).</span></span>
* <span data-ttu-id="8cff5-137">**Zásady vytváření názvů pro prostředí App Service:** každý App Service Environment vyžaduje jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="8cff5-137">**Naming convention for the App Service Environments:**  Each App Service Environment requires a unique name.</span></span>  <span data-ttu-id="8cff5-138">Nad rámec jednoho nebo dvou prostředí App Service je vhodné mít zásady vytváření názvů k identifikaci jednotlivých App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="8cff5-138">Beyond one or two App Service Environments it is helpful to have a naming convention to help identify each App Service Environment.</span></span>  <span data-ttu-id="8cff5-139">Pro ukázkovou aplikaci byl použit jednoduché zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="8cff5-139">For the sample app a simple naming convention was used.</span></span>  <span data-ttu-id="8cff5-140">Názvy tři prostředí App Service jsou *fe1ase*, *fe2ase*, a *fe3ase*.</span><span class="sxs-lookup"><span data-stu-id="8cff5-140">The names of the three App Service Environments are *fe1ase*, *fe2ase*, and *fe3ase*.</span></span>
* <span data-ttu-id="8cff5-141">**Zásady vytváření názvů pro aplikace:** vzhledem k tomu, že budou nasazeny více instancí aplikace, název se vyžaduje pro každou instanci aplikace nasazené.</span><span class="sxs-lookup"><span data-stu-id="8cff5-141">**Naming convention for the apps:**  Since multiple instances of the app will be deployed, a name is needed for each instance of the deployed app.</span></span>  <span data-ttu-id="8cff5-142">Jeden málo známé, ale je užitečná funkce prostředí App Service je, že stejný název aplikace lze použít v rámci více prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="8cff5-142">One little-known but very convenient feature of App Service Environments is that the same app name can be used across multiple App Service Environments.</span></span>  <span data-ttu-id="8cff5-143">Vzhledem k tomu, že každý App Service Environment má příponu domény jedinečný, vývojáři můžete znovu použít přesný stejný název aplikace v každém prostředí.</span><span class="sxs-lookup"><span data-stu-id="8cff5-143">Since each App Service Environment has a unique domain suffix, developers can choose to re-use the exact same app name in each environment.</span></span>  <span data-ttu-id="8cff5-144">Vývojář může mít třeba aplikace s názvem následujícím způsobem: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*atd.  Pro ukázkovou aplikaci, když každá instance aplikace také musí mít jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="8cff5-144">For example a developer could have apps named as follows:  *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*, etc.  For the sample app though each app instance also has a unique name.</span></span>  <span data-ttu-id="8cff5-145">Názvy instancí aplikace používá jsou *webfrontend1*, *webfrontend2*, a *webfrontend3*.</span><span class="sxs-lookup"><span data-stu-id="8cff5-145">The app instance names used are *webfrontend1*, *webfrontend2*, and *webfrontend3*.</span></span>

## <a name="setting-up-the-traffic-manager-profile"></a><span data-ttu-id="8cff5-146">Nastavení profilu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="8cff5-146">Setting up the Traffic Manager Profile</span></span>
<span data-ttu-id="8cff5-147">Po několik instancí aplikace nasazené na několika prostředí App Service, může být registrováno s nástrojem Traffic Manager instance jednotlivých aplikací.</span><span class="sxs-lookup"><span data-stu-id="8cff5-147">Once multiple instances of an app are deployed on multiple App Service Environments, the  individual app instances can be registered with Traffic Manager.</span></span>  <span data-ttu-id="8cff5-148">Pro ukázkovou aplikaci Traffic Manager profil je potřeba pro *škálovatelné App Service Environment demo.trafficmanager.net* zákazníků, může směrovat do jakéhokoli z následující instance nasazené aplikace:</span><span class="sxs-lookup"><span data-stu-id="8cff5-148">For the sample app a Traffic Manager profile is needed for *scalable-ase-demo.trafficmanager.net* that can route customers to any of the following deployed app instances:</span></span>

* <span data-ttu-id="8cff5-149">**webfrontend1.fe1ase.p.azurewebsites.NET:** instanci ukázková aplikace nasazené na první App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="8cff5-149">**webfrontend1.fe1ase.p.azurewebsites.net:**  An instance of the sample app deployed on the first App Service Environment.</span></span>
* <span data-ttu-id="8cff5-150">**webfrontend2.fe2ase.p.azurewebsites.NET:** instanci ukázková aplikace nasazené na druhý App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="8cff5-150">**webfrontend2.fe2ase.p.azurewebsites.net:**  An instance of the sample app deployed on the second App Service Environment.</span></span>
* <span data-ttu-id="8cff5-151">**webfrontend3.fe3ase.p.azurewebsites.NET:** instanci ukázková aplikace nasazené na třetí App Service Environment.</span><span class="sxs-lookup"><span data-stu-id="8cff5-151">**webfrontend3.fe3ase.p.azurewebsites.net:**  An instance of the sample app deployed on the third App Service Environment.</span></span>

<span data-ttu-id="8cff5-152">Nejjednodušší způsob, jak zaregistrovat několik Azure App Service koncových bodů, ve všech spuštěných **stejné** oblast Azure, je pomocí prostředí Powershell [podpory Azure Resource Manager Traffic Manager] [ ARMTrafficManager].</span><span class="sxs-lookup"><span data-stu-id="8cff5-152">The easiest way to register multiple Azure App Service endpoints, all running in the **same** Azure region, is with the Powershell [Azure Resource Manager Traffic Manager support][ARMTrafficManager].</span></span>  

<span data-ttu-id="8cff5-153">Prvním krokem je vytvoření profilu Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="8cff5-153">The first step is to create an Azure Traffic Manager profile.</span></span>  <span data-ttu-id="8cff5-154">Následující kód ukazuje, jak byl profil vytvořen pro ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8cff5-154">The code below shows how the profile was created for the sample app:</span></span>

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

<span data-ttu-id="8cff5-155">Všimněte si jak *RelativeDnsName* parametr byl nastavený *škálovatelné App Service Environment ukázku*.</span><span class="sxs-lookup"><span data-stu-id="8cff5-155">Notice how the *RelativeDnsName* parameter was set to *scalable-ase-demo*.</span></span>  <span data-ttu-id="8cff5-156">Jedná se jak názvu domény *škálovatelné App Service Environment demo.trafficmanager.net* je vytvořena a přidružena profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8cff5-156">This is how the domain name *scalable-ase-demo.trafficmanager.net* is created and associated with a Traffic Manager profile.</span></span>

<span data-ttu-id="8cff5-157">*TrafficRoutingMethod* parametr definuje zásady Traffic Manager použije k určení způsobu rozloženy zákazníka zatížení k dispozici koncové body pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="8cff5-157">The *TrafficRoutingMethod* parameter defines the load balancing policy Traffic Manager will use to determine how to spread customer load across all of the available endpoints.</span></span>  <span data-ttu-id="8cff5-158">V tomto příkladu *vážená* jste vybrali metodu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-158">In this example the *Weighted* method was chosen.</span></span>  <span data-ttu-id="8cff5-159">Tato akce způsobí žádostem zákazníků se rozloženy koncové body zaregistrovanou aplikaci založené na relativní váhu přidružené každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="8cff5-159">This will result in customer requests being spread across all of the registered application endpoints based on the relative weights associated with each endpoint.</span></span> 

<span data-ttu-id="8cff5-160">S profilem vytvořen každá instance aplikace přidat do profilu jako nativní koncového bodu Azure.</span><span class="sxs-lookup"><span data-stu-id="8cff5-160">With the profile created, each app instance is added to the profile as a native Azure endpoint.</span></span>  <span data-ttu-id="8cff5-161">Následující kód načte odkaz na každý front-endu webové aplikace a pak přidá každou aplikaci jako koncový bod Traffic Manager prostřednictvím *TargetResourceId* parametr.</span><span class="sxs-lookup"><span data-stu-id="8cff5-161">The code below fetches a reference to each front end web app, and then adds each app as a Traffic Manager endpoint by way of the *TargetResourceId* parameter.</span></span>

    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10

    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile

<span data-ttu-id="8cff5-162">Všimněte si, jak je jednoho volání *přidat AzureTrafficManagerEndpointConfig* pro každou instanci jednotlivých aplikací.</span><span class="sxs-lookup"><span data-stu-id="8cff5-162">Notice how there is one call to *Add-AzureTrafficManagerEndpointConfig* for each individual app instance.</span></span>  <span data-ttu-id="8cff5-163">*TargetResourceId* parametr v každém příkazu prostředí Powershell odkazuje jedna z instancí tři nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="8cff5-163">The *TargetResourceId* parameter in each Powershell command references one of the three deployed app instances.</span></span>  <span data-ttu-id="8cff5-164">Profil služby Traffic Manager se zatížení rozloženy všechny tři koncové body, které jsou zaregistrované v profilu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-164">The Traffic Manager profile will spread load across all three endpoints registered in the profile.</span></span>

<span data-ttu-id="8cff5-165">Všechny tři koncových bodů pro používají stejnou hodnotu (10) *váhy* parametr.</span><span class="sxs-lookup"><span data-stu-id="8cff5-165">All of the three endpoints use the same value (10) for the *Weight* parameter.</span></span>  <span data-ttu-id="8cff5-166">Výsledkem rozprostření žádostem zákazníků Traffic Manager napříč všemi instancemi tři aplikace relativně rovnoměrně.</span><span class="sxs-lookup"><span data-stu-id="8cff5-166">This results in Traffic Manager spreading customer requests across all three app instances relatively evenly.</span></span> 

## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a><span data-ttu-id="8cff5-167">Odkazující vlastní domény aplikace na doménu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="8cff5-167">Pointing the App's Custom Domain at the Traffic Manager Domain</span></span>
<span data-ttu-id="8cff5-168">V posledním kroku potřeby je tak, aby odkazovalo vlastní domény aplikace na doménu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8cff5-168">The final step necessary is to point the custom domain of the app at the Traffic Manager domain.</span></span>  <span data-ttu-id="8cff5-169">Ukázková aplikace to znamená, odkazující *www.scalableasedemo.com* v *škálovatelné App Service Environment demo.trafficmanager.net*.</span><span class="sxs-lookup"><span data-stu-id="8cff5-169">For the sample app this means pointing *www.scalableasedemo.com* at *scalable-ase-demo.trafficmanager.net*.</span></span>  <span data-ttu-id="8cff5-170">Tento krok je potřeba dokončit u registrátora domény, který spravuje vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-170">This step needs to be completed with the domain registrar that manages the custom domain.</span></span>  

<span data-ttu-id="8cff5-171">Pomocí nástrojů pro správu vašeho registrátora domény zaznamenává záznam CNAME musí být vytvořen, která odkazuje vlastní domény na doménu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8cff5-171">Using your registrar's domain management tools, a CNAME records needs to be created which points the custom domain at the Traffic Manager domain.</span></span>  <span data-ttu-id="8cff5-172">Následující obrázek ukazuje příklad tuto konfiguraci CNAME, které bude vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="8cff5-172">The picture below shows an example of what this CNAME configuration looks like:</span></span>

![CNAME pro vlastní doménu.][CNAMEforCustomDomain] 

<span data-ttu-id="8cff5-174">I když nejsou zahrnuté v tomto tématu, mějte na paměti, že každá instance jednotlivých aplikací musí mít vlastní doména se také zaregistrována.</span><span class="sxs-lookup"><span data-stu-id="8cff5-174">Although not covered in this topic, remember that each individual app instance needs to have the custom domain registered with it as well.</span></span>  <span data-ttu-id="8cff5-175">Jinak Pokud požadavek je pro instanci aplikace a aplikace nemá vlastní domény zaregistrována aplikace, se požadavek nezdaří.</span><span class="sxs-lookup"><span data-stu-id="8cff5-175">Otherwise if a request makes it to an app instance, and the application does not have the custom domain registered with the app, the request will fail.</span></span>  

<span data-ttu-id="8cff5-176">V tomto příkladu se vlastní doména je *www.scalableasedemo.com*, a každá instance aplikace má vlastní domény s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="8cff5-176">In this example the custom domain is *www.scalableasedemo.com*, and each application instance has the custom domain associated with it.</span></span>

![Vlastní doména][CustomDomain] 

<span data-ttu-id="8cff5-178">Rekapitulace registrace vlastní domény s aplikacemi Azure App Service, najdete v následujícím článku na [registrace vlastní domény][RegisterCustomDomain].</span><span class="sxs-lookup"><span data-stu-id="8cff5-178">For a recap of of registering a custom domain with Azure App Service apps, see the following article on [registering custom domains][RegisterCustomDomain].</span></span>

## <a name="trying-out-the-distributed-topology"></a><span data-ttu-id="8cff5-179">Vyzkoušení topologie distribuované</span><span class="sxs-lookup"><span data-stu-id="8cff5-179">Trying out the Distributed Topology</span></span>
<span data-ttu-id="8cff5-180">Konečný výsledek konfiguraci Traffic Manager a DNS se žádostí o *www.scalableasedemo.com* bude procházet následující sekvenci:</span><span class="sxs-lookup"><span data-stu-id="8cff5-180">The end result of the Traffic Manager and DNS configuration is that requests for *www.scalableasedemo.com* will flow through the following sequence:</span></span>

1. <span data-ttu-id="8cff5-181">Prohlížeč nebo zařízení bude vyhledávání DNS *www.scalableasedemo.com*</span><span class="sxs-lookup"><span data-stu-id="8cff5-181">A browser or device will make a DNS lookup for *www.scalableasedemo.com*</span></span>
2. <span data-ttu-id="8cff5-182">Záznam CNAME u registrátora domény způsobí, že vyhledávání DNS přesměrovat do Azure Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="8cff5-182">The CNAME entry at the domain registrar causes the DNS lookup to be redirected to Azure Traffic Manager.</span></span>
3. <span data-ttu-id="8cff5-183">Vyhledávání DNS se provádí *škálovatelné App Service Environment demo.trafficmanager.net* na jednom ze serverů Azure DNS Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="8cff5-183">A DNS lookup is made for *scalable-ase-demo.trafficmanager.net* against one of the Azure Traffic Manager DNS servers.</span></span>
4. <span data-ttu-id="8cff5-184">Podle zásad vyrovnávání zatížení ( *TrafficRoutingMethod* parametr dříve použitý při vytváření profil služby Traffic Manager), bude Traffic Manager vyberte jeden z nakonfigurované koncové body a vrátí plně kvalifikovaný název domény tohoto koncového bodu na prohlížeč nebo zařízení.</span><span class="sxs-lookup"><span data-stu-id="8cff5-184">Based on the load balancing policy (the *TrafficRoutingMethod* parameter used earlier when creating the Traffic Manager profile), Traffic Manager will select one of the configured endpoints and return the FQDN of that endpoint to the browser or device.</span></span>
5. <span data-ttu-id="8cff5-185">Vzhledem k tomu, že plně kvalifikovaný název koncového bodu je adresa Url instance aplikace spuštěná na služby App Service Environment, prohlížeč nebo zařízení požádá server Microsoft Azure DNS přeložit plně kvalifikovaný název domény na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-185">Since the FQDN of the endpoint is the Url of an app instance running on an App Service Environment, the browser or device will ask a Microsoft Azure DNS server to resolve the FQDN to an IP address.</span></span> 
6. <span data-ttu-id="8cff5-186">Prohlížeče nebo zařízení, odešle požadavek HTTP/S na IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8cff5-186">The browser or device will send the HTTP/S request to the IP address.</span></span>  
7. <span data-ttu-id="8cff5-187">Žádost bude přicházejí na jednu z instancí aplikace spuštěn v jednom z prostředí App Service.</span><span class="sxs-lookup"><span data-stu-id="8cff5-187">The request will arrive at one of the app instances running on one of the App Service Environments.</span></span>

<span data-ttu-id="8cff5-188">Následující obrázek konzoly ukazuje vyhledávání DNS pro vlastní doménu ukázkové aplikace úspěšně řešení do instance aplikace spuštěná v jednom ze tří prostředí Service ukázkové aplikace (v tomto případě druhý ze tří prostředí App Service):</span><span class="sxs-lookup"><span data-stu-id="8cff5-188">The console picture below shows a DNS lookup for the sample app's custom domain successfully resolving to an app instance running on one of the three sample App Service Environments (in this case the second of the three App Service Environments):</span></span>

![Vyhledávání DNS][DNSLookup] 

## <a name="additional-links-and-information"></a><span data-ttu-id="8cff5-190">Další odkazy a informace</span><span class="sxs-lookup"><span data-stu-id="8cff5-190">Additional Links and Information</span></span>
<span data-ttu-id="8cff5-191">Všechny články a jak – do této pro App Service Environment jsou dostupné v [soubor README pro prostředí aplikačních služeb](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="8cff5-191">All articles and How-To's for App Service Environments are available in the [README for Application Service Environments](../app-service/app-service-app-service-environments-readme.md).</span></span>

<span data-ttu-id="8cff5-192">Dokumentace v prostředí Powershell [podpory Azure Resource Manager Traffic Manager][ARMTrafficManager].</span><span class="sxs-lookup"><span data-stu-id="8cff5-192">Documentation on the Powershell [Azure Resource Manager Traffic Manager support][ARMTrafficManager].</span></span>  

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]: ../traffic-manager/traffic-manager-manage-profiles.md
[ARMTrafficManager]: ../traffic-manager/traffic-manager-powershell-arm.md
[RegisterCustomDomain]: app-service-web-tutorial-custom-domain.md


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 