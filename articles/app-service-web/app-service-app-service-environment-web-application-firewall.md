---
title: "aaaConfiguring webové aplikace brány Firewall (firewall webových aplikací) pro App Service Environment"
description: "Zjistěte, jak brány firewall tooconfigure webové aplikace před služby App Service Environment."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 0fcf62aea871751c9d4f294d2d24df2186fc0e7e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a><span data-ttu-id="6511f-103">Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="6511f-103">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="6511f-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="6511f-104">Overview</span></span>
<span data-ttu-id="6511f-105">Webové aplikace brány firewall jako hello [Barracuda firewall webových aplikací pro Azure](https://www.barracuda.com/programs/azure) která je dostupná na hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) pomáhá zabezpečit vaše webové aplikace zkontrolováním příchozí webové přenosy tooblock SQL injekce, skriptování, malwaru nahrávání & aplikace DDoS a jiným útokům.</span><span class="sxs-lookup"><span data-stu-id="6511f-105">Web application firewalls like hello [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) that is available on hello [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helps secure your web applications by inspecting inbound web traffic tooblock SQL injections, Cross-Site Scripting, malware uploads & application DDoS and other attacks.</span></span> <span data-ttu-id="6511f-106">Je také zkontroluje, zda obsahuje hello odpovědí z hello back endové webové servery pro prevenci ztráty dat (DLP).</span><span class="sxs-lookup"><span data-stu-id="6511f-106">It also inspects hello responses from hello back-end web servers for Data Loss Prevention (DLP).</span></span> <span data-ttu-id="6511f-107">V kombinaci s hello izolace a další škálování poskytované prostředí App Service, toto poskytuje ideální prostředí toohost obchodní kritické webové aplikace, které potřebují toowithstand škodlivými požadavky a vyšší objemy přenosů.</span><span class="sxs-lookup"><span data-stu-id="6511f-107">Combined with hello isolation and additional scaling provided by App Service Environments, this provides an ideal environment toohost business critical web applications that need toowithstand malicious requests and high volume traffic.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a><span data-ttu-id="6511f-108">Nastavení</span><span class="sxs-lookup"><span data-stu-id="6511f-108">Setup</span></span>
<span data-ttu-id="6511f-109">K tomuto dokumentu, který bude nakonfigurujeme naše App Service Environment za několik zatížení vyvážit instancí Barracuda firewall webových aplikací tak, aby pouze provoz z firewall webových aplikací hello dosáhnout hello App Service Environment a nebude dostupný z hello hraniční sítě.</span><span class="sxs-lookup"><span data-stu-id="6511f-109">For this document we will configure our App Service Environment behind multiple load balanced instances of Barracuda WAF so that only traffic from hello WAF can reach hello App Service Environment and it will not be accessible from hello DMZ.</span></span> <span data-ttu-id="6511f-110">Azure Traffic Manager jsme bude mít i před naše vyrovnávání tooload instance Barracuda firewall webových aplikací mezi datových center Azure a oblastech.</span><span class="sxs-lookup"><span data-stu-id="6511f-110">We will also have Azure Traffic Manager in front of our Barracuda WAF instances tooload balance across Azure data centers and regions.</span></span> <span data-ttu-id="6511f-111">Nejvyšší úrovni diagram instalačního programu hello bude vypadat co jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="6511f-111">A high level diagram of hello setup would look like what is shown below.</span></span>

![Architektura][Architecture] 

> <span data-ttu-id="6511f-113">Poznámka: S hello zavedení [ILB podporu pro App Service Environment](app-service-environment-with-internal-load-balancer.md), můžete nakonfigurovat toobe hello App Service Environment nejsou přístupné z hello DMZ a pouze být k dispozici toohello privátní sítě.</span><span class="sxs-lookup"><span data-stu-id="6511f-113">Note: With hello introduction of [ILB support for App Service Environment](app-service-environment-with-internal-load-balancer.md), you can configure hello ASE toobe inaccessible from hello DMZ and only be available toohello private network.</span></span> 
> 
> 

## <a name="configuring-your-app-service-environment"></a><span data-ttu-id="6511f-114">Konfigurace prostředí služby App Service</span><span class="sxs-lookup"><span data-stu-id="6511f-114">Configuring your App Service Environment</span></span>
<span data-ttu-id="6511f-115">tooconfigure služby App Service Environment odkazovat příliš[naší dokumentaci](app-service-web-how-to-create-an-app-service-environment.md) na hello subjektu.</span><span class="sxs-lookup"><span data-stu-id="6511f-115">tooconfigure an App Service Environment refer too[our documentation](app-service-web-how-to-create-an-app-service-environment.md) on hello subject.</span></span> <span data-ttu-id="6511f-116">Po vytvoření služby App Service Environment máte, můžete vytvořit [webové aplikace](app-service-web-overview.md), [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) v tomto prostředí, které budou všechny chráněné za firewall webových aplikací hello jsme Nakonfigurujte v další části hello.</span><span class="sxs-lookup"><span data-stu-id="6511f-116">Once you have an App Service Environment created, you can create [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) and [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in this environment that will all be protected behind hello WAF we configure in hello next section.</span></span>

## <a name="configuring-your-barracuda-waf-cloud-service"></a><span data-ttu-id="6511f-117">Konfigurace Barracuda firewall webových aplikací cloudové služby</span><span class="sxs-lookup"><span data-stu-id="6511f-117">Configuring your Barracuda WAF Cloud Service</span></span>
<span data-ttu-id="6511f-118">Barracuda má [podrobné článku](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na nasazení jeho firewall webových aplikací na virtuálním počítači v Azure.</span><span class="sxs-lookup"><span data-stu-id="6511f-118">Barracuda has a [detailed article](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) on deploying its WAF on a virtual machine in Azure.</span></span> <span data-ttu-id="6511f-119">Ale vzhledem k tomu, že nám chcete redundance a není způsobit jediný bod selhání, aby toodeploy minimálně 2 Firewall webových aplikací instance virtuálních počítačů do hello stejné cloudové služby při těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="6511f-119">But because we want redundancy and not introduce a single point of failure, you want toodeploy at least 2 WAF instance VMs into hello same Cloud Service when following these instructions.</span></span>

### <a name="adding-endpoints-toocloud-service"></a><span data-ttu-id="6511f-120">Přidání tooCloud koncové body služby</span><span class="sxs-lookup"><span data-stu-id="6511f-120">Adding Endpoints tooCloud Service</span></span>
<span data-ttu-id="6511f-121">Jakmile máte 2 nebo více virtuálních počítačů firewall webových aplikací instancí v rámci cloudové služby můžete použít hello [portál Azure](https://portal.azure.com/) tooadd HTTP a HTTPS koncových bodů, které se používají v aplikaci, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="6511f-121">Once you have 2 or more WAF VM instances in your Cloud Service you can use hello [Azure portal](https://portal.azure.com/) tooadd HTTP and HTTPS endpoints that are used by your application as shown in hello image below.</span></span>

![Konfigurace koncového bodu][ConfigureEndpoint]

<span data-ttu-id="6511f-123">Pokud vaše aplikace používat ostatní koncové body, ujistěte se, že tooadd také tyto toothis seznamu.</span><span class="sxs-lookup"><span data-stu-id="6511f-123">If your applications use other endpoints, make sure tooadd those toothis list as well.</span></span> 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a><span data-ttu-id="6511f-124">Konfigurace Barracuda firewall webových aplikací prostřednictvím portálu pro správu</span><span class="sxs-lookup"><span data-stu-id="6511f-124">Configuring Barracuda WAF through its Management Portal</span></span>
<span data-ttu-id="6511f-125">Barracuda firewall webových aplikací používá TCP Port 8000 pro konfiguraci prostřednictvím portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="6511f-125">Barracuda WAF uses TCP Port 8000 for configuration through its management portal.</span></span> <span data-ttu-id="6511f-126">Vzhledem k tomu, že máme několik instancí virtuálních počítačů hello firewall webových aplikací je nutné toorepeat hello zde uvedených kroků pro každou instanci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6511f-126">Since we have multiple instances of hello WAF VMs you will need toorepeat hello steps here for each VM instance.</span></span> 

> <span data-ttu-id="6511f-127">Poznámka: Po dokončení konfigurace firewall webových aplikací odebrání koncový bod TCP/8000 hello všechny virtuální počítače firewall webových aplikací tookeep zabezpečené vaší firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6511f-127">Note: Once you are done with WAF configuration, remove hello TCP/8000 endpoint from all your WAF VMs tookeep your WAF secure.</span></span>
> 
> 

<span data-ttu-id="6511f-128">Přidáte koncový bod správy hello jak je znázorněno v následujícím tooconfigure obrázku hello vaší Barracuda firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6511f-128">Add hello management endpoint as shown in hello image below tooconfigure your Barracuda WAF.</span></span>

![Přidání koncového bodu správy][AddManagementEndpoint]

<span data-ttu-id="6511f-130">Pomocí prohlížeče toobrowse toohello správu koncového bodu v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="6511f-130">Use a browser toobrowse toohello management endpoint on your Cloud Service.</span></span> <span data-ttu-id="6511f-131">Pokud cloudové služby je volána test.cloudapp.net, by procházením toohttp://test.cloudapp.net:8000 přístup k tomuto koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="6511f-131">If your Cloud Service is called test.cloudapp.net, you would access this endpoint by browsing toohttp://test.cloudapp.net:8000.</span></span> <span data-ttu-id="6511f-132">Měli byste vidět přihlašovací stránky, jako níže, můžete se přihlásit pomocí přihlašovacích údajů zadaných ve fázi instalace virtuálních počítačů hello firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6511f-132">You should see a login page like below that you can login using credentials you specified in hello WAF VM setup phase.</span></span>

![Správa přihlašovací stránky][ManagementLoginPage]

<span data-ttu-id="6511f-134">Po přihlášení byste měli vidět řídicí panel jako hello jednu v bitové kopii hello nižší, než je nabídne základní statistické údaje o hello ochrany firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="6511f-134">Once you login you should see a dashboard as hello one in hello image below that will present basic statistics about hello WAF protection.</span></span>

![Řídicí panel správy][ManagementDashboard]

<span data-ttu-id="6511f-136">Kliknutím na kartu hello služby vám umožní nakonfigurovat vaše firewall webových aplikací pro služby, které chrání.</span><span class="sxs-lookup"><span data-stu-id="6511f-136">Clicking on hello Services tab will let you configure your WAF for services it is protecting.</span></span> <span data-ttu-id="6511f-137">Další informace o konfiguraci vašeho Barracuda firewall webových aplikací lze najít [jejich dokumentaci](https://techlib.barracuda.com/waf/getstarted1).</span><span class="sxs-lookup"><span data-stu-id="6511f-137">For more details on configuring your Barracuda WAF you can consult [their documentation](https://techlib.barracuda.com/waf/getstarted1).</span></span> <span data-ttu-id="6511f-138">V příkladu hello níže webové aplikace Azure byla nakonfigurována s provozem na protokolu HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6511f-138">In hello example below an Azure Web App serving traffic on HTTP and HTTPS has been configured.</span></span>

![Přidání služeb správy][ManagementAddServices]

> <span data-ttu-id="6511f-140">Poznámka: V závislosti na tom, jak jsou nakonfigurované vašich aplikací a jaké funkce jsou používány ve službě App Service Environment, budete potřebovat tooforward provoz pro porty TCP než 80 a 443, například pokud máte instalaci IP SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6511f-140">Note: Depending on how your applications are configured and what features are being used in your App Service Environment, you will need tooforward traffic for TCP ports other than 80 and 443, e.g. if you have IP SSL setup for a Web App.</span></span> <span data-ttu-id="6511f-141">Seznam síťové porty používané v prostředí App Service naleznete příliš[řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) části síťové porty.</span><span class="sxs-lookup"><span data-stu-id="6511f-141">For a list of network ports used in App Service Environments, please refer too[Control Inbound Traffic documentation's](app-service-app-service-environment-control-inbound-traffic.md) Network Ports section.</span></span>
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a><span data-ttu-id="6511f-142">Konfigurace Microsoft Azure Traffic Manageru (volitelné)</span><span class="sxs-lookup"><span data-stu-id="6511f-142">Configuring Microsoft Azure Traffic Manager (OPTIONAL)</span></span>
<span data-ttu-id="6511f-143">Pokud vaše aplikace je k dispozici v několika oblastech, pak by chcete vyrovnávat tooload je za [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6511f-143">If your application is available in multiple regions, then you would want tooload balance them behind [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span></span> <span data-ttu-id="6511f-144">toodo, můžete přidat koncový bod v hello [portál Azure classic](https://manage.azure.com) pomocí hello název cloudové služby pro vaše firewall webových aplikací v hello profil služby Traffic Manager, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="6511f-144">toodo so you can add an endpoint in hello [Azure classic portal](https://manage.azure.com) using hello Cloud Service name for your WAF in hello Traffic Manager profile as shown in hello image below.</span></span> 

![Koncový bod Traffic Manager][TrafficManagerEndpoint]

<span data-ttu-id="6511f-146">Pokud vaše aplikace vyžaduje ověřování, zajistěte, že abyste měli některé prostředků, která nevyžaduje žádné ověřování pro Traffic Manager tooping hello dostupnosti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6511f-146">If your application requires authentication, ensure you have some resource that doesn't require any authentication for Traffic Manager tooping for hello availability of your application.</span></span> <span data-ttu-id="6511f-147">Hello adresy URL v části hello část konfigurace můžete nakonfigurovat na hello [portál Azure classic](https://manage.azure.com) jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6511f-147">You can configure hello URL under hello Configure section on hello [Azure classic portal](https://manage.azure.com) as shown below.</span></span>

![Konfigurace Traffic Manageru][ConfigureTrafficManager]

<span data-ttu-id="6511f-149">tooforward příkazy ping hello Traffic Manageru z vaší aplikace tooyour firewall webových aplikací, musíte překlady toosetup webu na firewall webových aplikací Barracuda tooforward provoz tooyour aplikace jak je znázorněno v následujícím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="6511f-149">tooforward hello Traffic Manager pings from your WAF tooyour application, you need toosetup Website Translations on your Barracuda WAF tooforward traffic tooyour application as shown in hello example below.</span></span>

![Překlady webu][WebsiteTranslations]

## <a name="securing-traffic-tooapp-service-environment-using-network-security-groups-nsg"></a><span data-ttu-id="6511f-151">Zabezpečení provozu tooApp služby prostředí pomocí zabezpečení sítě skupiny (NSG)</span><span class="sxs-lookup"><span data-stu-id="6511f-151">Securing Traffic tooApp Service Environment Using Network Security Groups (NSG)</span></span>
<span data-ttu-id="6511f-152">Postupujte podle hello [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) pro informace o omezení provozu tooyour App Service Environment z hello firewall webových aplikací pouze pomocí hello VIP adresy cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6511f-152">Follow hello [Control Inbound Traffic documentation](app-service-app-service-environment-control-inbound-traffic.md) for details on restricting traffic tooyour App Service Environment from hello WAF only by using hello VIP address of your Cloud Service.</span></span> <span data-ttu-id="6511f-153">Zde je ukázka příkazu prostředí Powershell pro provádění této úlohy pro TCP port 80.</span><span class="sxs-lookup"><span data-stu-id="6511f-153">Here's a sample Powershell command for performing this task for TCP port 80.</span></span>

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

<span data-ttu-id="6511f-154">Nahraďte hello SourceAddressPrefix hello virtuální IP adresa (VIP) vaší firewall webových aplikací cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6511f-154">Replace hello SourceAddressPrefix with hello Virtual IP Address (VIP) of your WAF's Cloud Service.</span></span>

> <span data-ttu-id="6511f-155">Poznámka: hello VIP cloudové služby se změní, když je odstranit a znovu vytvořit hello cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="6511f-155">Note: hello VIP of your Cloud Service will change when you delete and re-create hello Cloud Service.</span></span> <span data-ttu-id="6511f-156">Ujistěte se, tooupdate hello IP adresu ve skupině prostředků sítě hello až to uděláte tak.</span><span class="sxs-lookup"><span data-stu-id="6511f-156">Make sure tooupdate hello IP address in hello Network Resource group once you do so.</span></span> 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
