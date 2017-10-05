---
title: "Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment"
description: "Informace o konfiguraci brány firewall webových aplikací před služby App Service Environment."
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
ms.openlocfilehash: 3e9e9fa4ddab60a467e8aa793ec0ca269b0bc4e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a><span data-ttu-id="8472b-103">Konfigurace brány Firewall webových aplikací (firewall webových aplikací) pro služby App Service Environment</span><span class="sxs-lookup"><span data-stu-id="8472b-103">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>
## <a name="overview"></a><span data-ttu-id="8472b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="8472b-104">Overview</span></span>
<span data-ttu-id="8472b-105">Brány firewall webových aplikací, jako [Barracuda firewall webových aplikací pro Azure](https://www.barracuda.com/programs/azure) která je dostupná na [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) pomáhá zabezpečit zkontrolováním příchozí webové přenosy blokování vložení SQL, webových aplikací Skriptování, malwaru nahrávání & aplikace DDoS a jiným útokům.</span><span class="sxs-lookup"><span data-stu-id="8472b-105">Web application firewalls like the [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) that is available on the [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) helps secure your web applications by inspecting inbound web traffic to block SQL injections, Cross-Site Scripting, malware uploads & application DDoS and other attacks.</span></span> <span data-ttu-id="8472b-106">Je také zkontroluje, zda obsahuje odpovědi ze serveru back endové webové pro prevenci ztráty dat (DLP).</span><span class="sxs-lookup"><span data-stu-id="8472b-106">It also inspects the responses from the back-end web servers for Data Loss Prevention (DLP).</span></span> <span data-ttu-id="8472b-107">V kombinaci s izolace a další škálování poskytované prostředí App Service, toto poskytuje ideální prostředí k hostiteli obchodní kritické webovým aplikacím, které je potřeba odolat škodlivými požadavky a vyšší objemy přenosů.</span><span class="sxs-lookup"><span data-stu-id="8472b-107">Combined with the isolation and additional scaling provided by App Service Environments, this provides an ideal environment to host business critical web applications that need to withstand malicious requests and high volume traffic.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a><span data-ttu-id="8472b-108">Nastavení</span><span class="sxs-lookup"><span data-stu-id="8472b-108">Setup</span></span>
<span data-ttu-id="8472b-109">Pro tento dokument, který bude nakonfigurujeme naše App Service Environment za několik zatížení vyvážit instancí Barracuda firewall webových aplikací, aby pouze provoz z firewall webových aplikací dosáhnout App Service Environment a nebude dostupný z hraniční síti.</span><span class="sxs-lookup"><span data-stu-id="8472b-109">For this document we will configure our App Service Environment behind multiple load balanced instances of Barracuda WAF so that only traffic from the WAF can reach the App Service Environment and it will not be accessible from the DMZ.</span></span> <span data-ttu-id="8472b-110">Azure Traffic Manager jsme bude mít i před naše instancí Barracuda firewall webových aplikací na Vyrovnávání zatížení v rámci datových center Azure a oblastech.</span><span class="sxs-lookup"><span data-stu-id="8472b-110">We will also have Azure Traffic Manager in front of our Barracuda WAF instances to load balance across Azure data centers and regions.</span></span> <span data-ttu-id="8472b-111">Nejvyšší úrovni diagram instalace bude vypadat co jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="8472b-111">A high level diagram of the setup would look like what is shown below.</span></span>

![Architektura][Architecture] 

> <span data-ttu-id="8472b-113">Poznámka: se zavedením [ILB podporu pro App Service Environment](app-service-environment-with-internal-load-balancer.md), můžete nakonfigurovat App Service Environment pro nepřístupný od DMZ a být k dispozici pro privátní sítě.</span><span class="sxs-lookup"><span data-stu-id="8472b-113">Note: With the introduction of [ILB support for App Service Environment](app-service-environment-with-internal-load-balancer.md), you can configure the ASE to be inaccessible from the DMZ and only be available to the private network.</span></span> 
> 
> 

## <a name="configuring-your-app-service-environment"></a><span data-ttu-id="8472b-114">Konfigurace prostředí služby App Service</span><span class="sxs-lookup"><span data-stu-id="8472b-114">Configuring your App Service Environment</span></span>
<span data-ttu-id="8472b-115">Konfigurace služby App Service Environment najdete v tématu [naší dokumentaci](app-service-web-how-to-create-an-app-service-environment.md) na předmět.</span><span class="sxs-lookup"><span data-stu-id="8472b-115">To configure an App Service Environment refer to [our documentation](app-service-web-how-to-create-an-app-service-environment.md) on the subject.</span></span> <span data-ttu-id="8472b-116">Po vytvoření služby App Service Environment máte, můžete vytvořit [webové aplikace](app-service-web-overview.md), [aplikace API](../app-service-api/app-service-api-apps-why-best-platform.md) a [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) v tomto prostředí, které budou všechny chráněné za firewall webových aplikací jsme Nakonfigurujte v další části.</span><span class="sxs-lookup"><span data-stu-id="8472b-116">Once you have an App Service Environment created, you can create [Web Apps](app-service-web-overview.md), [API Apps](../app-service-api/app-service-api-apps-why-best-platform.md) and [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in this environment that will all be protected behind the WAF we configure in the next section.</span></span>

## <a name="configuring-your-barracuda-waf-cloud-service"></a><span data-ttu-id="8472b-117">Konfigurace Barracuda firewall webových aplikací cloudové služby</span><span class="sxs-lookup"><span data-stu-id="8472b-117">Configuring your Barracuda WAF Cloud Service</span></span>
<span data-ttu-id="8472b-118">Barracuda má [podrobné článku](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na nasazení jeho firewall webových aplikací na virtuálním počítači v Azure.</span><span class="sxs-lookup"><span data-stu-id="8472b-118">Barracuda has a [detailed article](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) on deploying its WAF on a virtual machine in Azure.</span></span> <span data-ttu-id="8472b-119">Ale vzhledem k tomu, že nám chcete redundance a není způsobit jediný bod selhání, které chcete nasadit alespoň 2 virtuální počítače instance firewall webových aplikací do stejné cloudové služby při těchto pokynů.</span><span class="sxs-lookup"><span data-stu-id="8472b-119">But because we want redundancy and not introduce a single point of failure, you want to deploy at least 2 WAF instance VMs into the same Cloud Service when following these instructions.</span></span>

### <a name="adding-endpoints-to-cloud-service"></a><span data-ttu-id="8472b-120">Přidání koncové body pro cloudové služby</span><span class="sxs-lookup"><span data-stu-id="8472b-120">Adding Endpoints to Cloud Service</span></span>
<span data-ttu-id="8472b-121">Jakmile máte 2 nebo více virtuálních počítačů firewall webových aplikací instancí v rámci cloudové služby můžete použít [portál Azure](https://portal.azure.com/) přidat HTTP a HTTPS koncové body, které se používají v aplikaci, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="8472b-121">Once you have 2 or more WAF VM instances in your Cloud Service you can use the [Azure portal](https://portal.azure.com/) to add HTTP and HTTPS endpoints that are used by your application as shown in the image below.</span></span>

![Konfigurace koncového bodu][ConfigureEndpoint]

<span data-ttu-id="8472b-123">Pokud vaše aplikace používat ostatní koncové body, ujistěte se, že jste přidejte je do tohoto seznamu také.</span><span class="sxs-lookup"><span data-stu-id="8472b-123">If your applications use other endpoints, make sure to add those to this list as well.</span></span> 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a><span data-ttu-id="8472b-124">Konfigurace Barracuda firewall webových aplikací prostřednictvím portálu pro správu</span><span class="sxs-lookup"><span data-stu-id="8472b-124">Configuring Barracuda WAF through its Management Portal</span></span>
<span data-ttu-id="8472b-125">Barracuda firewall webových aplikací používá TCP Port 8000 pro konfiguraci prostřednictvím portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="8472b-125">Barracuda WAF uses TCP Port 8000 for configuration through its management portal.</span></span> <span data-ttu-id="8472b-126">Vzhledem k tomu, že máme několik instancí virtuálních počítačů firewall webových aplikací, budete muset sem kroky zopakujte pro každou instanci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8472b-126">Since we have multiple instances of the WAF VMs you will need to repeat the steps here for each VM instance.</span></span> 

> <span data-ttu-id="8472b-127">Poznámka: Po dokončení konfigurace firewall webových aplikací odeberte koncový bod TCP/8000 ze všech firewall webových aplikací virtuálních počítačů k lepšímu zabezpečení vašeho firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="8472b-127">Note: Once you are done with WAF configuration, remove the TCP/8000 endpoint from all your WAF VMs to keep your WAF secure.</span></span>
> 
> 

<span data-ttu-id="8472b-128">Přidáte koncový bod správy, jak je znázorněno na obrázku níže ke konfiguraci vaší Barracuda firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="8472b-128">Add the management endpoint as shown in the image below to configure your Barracuda WAF.</span></span>

![Přidání koncového bodu správy][AddManagementEndpoint]

<span data-ttu-id="8472b-130">Použijte prohlížeč a přejděte ke koncovému bodu správy v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="8472b-130">Use a browser to browse to the management endpoint on your Cloud Service.</span></span> <span data-ttu-id="8472b-131">Pokud cloudové služby je volána test.cloudapp.net, by procházením http://test.cloudapp.net:8000 přístup k tomuto koncovému bodu.</span><span class="sxs-lookup"><span data-stu-id="8472b-131">If your Cloud Service is called test.cloudapp.net, you would access this endpoint by browsing to http://test.cloudapp.net:8000.</span></span> <span data-ttu-id="8472b-132">Měli byste vidět přihlašovací stránky, jako níže, můžete se přihlásit pomocí přihlašovacích údajů zadaných ve fázi instalace virtuálních počítačů firewall webových aplikací.</span><span class="sxs-lookup"><span data-stu-id="8472b-132">You should see a login page like below that you can login using credentials you specified in the WAF VM setup phase.</span></span>

![Správa přihlašovací stránky][ManagementLoginPage]

<span data-ttu-id="8472b-134">Po přihlášení byste měli vidět řídicí panel jako ten, který nabídne základní statistické údaje o ochranu firewall webových aplikací na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="8472b-134">Once you login you should see a dashboard as the one in the image below that will present basic statistics about the WAF protection.</span></span>

![Řídicí panel správy][ManagementDashboard]

<span data-ttu-id="8472b-136">Kliknutím na kartu služby vám umožní nakonfigurovat vaše firewall webových aplikací pro služby, které chrání.</span><span class="sxs-lookup"><span data-stu-id="8472b-136">Clicking on the Services tab will let you configure your WAF for services it is protecting.</span></span> <span data-ttu-id="8472b-137">Další informace o konfiguraci vašeho Barracuda firewall webových aplikací lze najít [jejich dokumentaci](https://techlib.barracuda.com/waf/getstarted1).</span><span class="sxs-lookup"><span data-stu-id="8472b-137">For more details on configuring your Barracuda WAF you can consult [their documentation](https://techlib.barracuda.com/waf/getstarted1).</span></span> <span data-ttu-id="8472b-138">V příkladu níže webové aplikace Azure byla nakonfigurována s provozem na protokolu HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8472b-138">In the example below an Azure Web App serving traffic on HTTP and HTTPS has been configured.</span></span>

![Přidání služeb správy][ManagementAddServices]

> <span data-ttu-id="8472b-140">Poznámka: V závislosti na tom, jak jsou nakonfigurované vašich aplikací a jaké funkce jsou používány ve službě App Service Environment, je nutné pro přenos dat pro TCP jiné porty než 80 a 443, například pokud máte instalaci IP SSL pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8472b-140">Note: Depending on how your applications are configured and what features are being used in your App Service Environment, you will need to forward traffic for TCP ports other than 80 and 443, e.g. if you have IP SSL setup for a Web App.</span></span> <span data-ttu-id="8472b-141">Seznam síťové porty používané v prostředí App Service naleznete v [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) části síťové porty.</span><span class="sxs-lookup"><span data-stu-id="8472b-141">For a list of network ports used in App Service Environments, please refer to [Control Inbound Traffic documentation's](app-service-app-service-environment-control-inbound-traffic.md) Network Ports section.</span></span>
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a><span data-ttu-id="8472b-142">Konfigurace Microsoft Azure Traffic Manageru (volitelné)</span><span class="sxs-lookup"><span data-stu-id="8472b-142">Configuring Microsoft Azure Traffic Manager (OPTIONAL)</span></span>
<span data-ttu-id="8472b-143">Pokud vaše aplikace je k dispozici v několika oblastech, pak budete chtít načíst vyvážit je za [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8472b-143">If your application is available in multiple regions, then you would want to load balance them behind [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md).</span></span> <span data-ttu-id="8472b-144">K tomu můžete přidat koncový bod v [portál Azure classic](https://manage.azure.com) pomocí název cloudové služby pro vaše firewall webových aplikací v profil služby Traffic Manager, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="8472b-144">To do so you can add an endpoint in the [Azure classic portal](https://manage.azure.com) using the Cloud Service name for your WAF in the Traffic Manager profile as shown in the image below.</span></span> 

![Koncový bod Traffic Manager][TrafficManagerEndpoint]

<span data-ttu-id="8472b-146">Pokud vaše aplikace vyžaduje ověřování, zajistěte, že abyste měli některé prostředků, která nevyžaduje žádné ověřování pro správce provozu na příkaz ping dostupnosti vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="8472b-146">If your application requires authentication, ensure you have some resource that doesn't require any authentication for Traffic Manager to ping for the availability of your application.</span></span> <span data-ttu-id="8472b-147">Adresu URL v části konfigurace můžete nakonfigurovat na [portál Azure classic](https://manage.azure.com) jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="8472b-147">You can configure the URL under the Configure section on the [Azure classic portal](https://manage.azure.com) as shown below.</span></span>

![Konfigurace Traffic Manageru][ConfigureTrafficManager]

<span data-ttu-id="8472b-149">Předávání příkazy ping Traffic Manager z vašeho firewall webových aplikací do vaší aplikace, musíte instalační program překlady webu na vaší Barracuda firewall webových aplikací pro přenos dat do vaší aplikace, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8472b-149">To forward the Traffic Manager pings from your WAF to your application, you need to setup Website Translations on your Barracuda WAF to forward traffic to your application as shown in the example below.</span></span>

![Překlady webu][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a><span data-ttu-id="8472b-151">Zabezpečení přenosy do služby App Service Environment pomocí skupin zabezpečení sítě (NSG)</span><span class="sxs-lookup"><span data-stu-id="8472b-151">Securing Traffic to App Service Environment Using Network Security Groups (NSG)</span></span>
<span data-ttu-id="8472b-152">Postupujte podle [řídící příchozí provoz dokumentaci](app-service-app-service-environment-control-inbound-traffic.md) podrobnosti o provoz směřující do služby App Service Environment z firewall webových aplikací pouze pomocí adresy VIP cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="8472b-152">Follow the [Control Inbound Traffic documentation](app-service-app-service-environment-control-inbound-traffic.md) for details on restricting traffic to your App Service Environment from the WAF only by using the VIP address of your Cloud Service.</span></span> <span data-ttu-id="8472b-153">Zde je ukázka příkazu prostředí Powershell pro provádění této úlohy pro TCP port 80.</span><span class="sxs-lookup"><span data-stu-id="8472b-153">Here's a sample Powershell command for performing this task for TCP port 80.</span></span>

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

<span data-ttu-id="8472b-154">Nahraďte virtuální IP adresa (VIP) vaší firewall webových aplikací cloudové služby SourceAddressPrefix.</span><span class="sxs-lookup"><span data-stu-id="8472b-154">Replace the SourceAddressPrefix with the Virtual IP Address (VIP) of your WAF's Cloud Service.</span></span>

> <span data-ttu-id="8472b-155">Poznámka: VIP cloudové služby se změní při odstranit a znovu vytvořit Cloudovou službu.</span><span class="sxs-lookup"><span data-stu-id="8472b-155">Note: The VIP of your Cloud Service will change when you delete and re-create the Cloud Service.</span></span> <span data-ttu-id="8472b-156">Ujistěte se, až to uděláte tak, aktualizujte IP adresu ve skupině prostředků sítě.</span><span class="sxs-lookup"><span data-stu-id="8472b-156">Make sure to update the IP address in the Network Resource group once you do so.</span></span> 
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
