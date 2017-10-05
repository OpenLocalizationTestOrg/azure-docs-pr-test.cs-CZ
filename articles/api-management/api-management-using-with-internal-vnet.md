---
title: "Jak používat Azure API Management s interní virtuální sítě | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat Azure API Management v interní virtuální síť."
services: api-management
documentationcenter: 
author: solankisamir
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 55248387c7e78d05c1cf1afd615b7b921e9669d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a><span data-ttu-id="00890-103">Interní virtuální sítě pomocí služby Azure API Management</span><span class="sxs-lookup"><span data-stu-id="00890-103">Using Azure API Management service with Internal Virtual Network</span></span>
<span data-ttu-id="00890-104">Pomocí virtuální sítě Azure (virtuální sítě) můžete spravovat rozhraní API správy rozhraní API není dostupný na Internetu.</span><span class="sxs-lookup"><span data-stu-id="00890-104">With Azure Virtual Networks (VNETs), API Management can manage APIs not accessible on the Internet.</span></span> <span data-ttu-id="00890-105">Jsou k dispozici pro připojení VPN technologií pro a API Management se dá nasadit v dva hlavní režimy uvnitř virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="00890-105">A number of VPN technologies are available to make the connection and API Management can be deployed in two main modes inside the VNET:</span></span>
* <span data-ttu-id="00890-106">Externí</span><span class="sxs-lookup"><span data-stu-id="00890-106">External</span></span>
* <span data-ttu-id="00890-107">Interní</span><span class="sxs-lookup"><span data-stu-id="00890-107">Internal</span></span>

## <span data-ttu-id="00890-108"><a name="overview"> </a>Přehled</span><span class="sxs-lookup"><span data-stu-id="00890-108"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="00890-109">Po nasazení v režimu interní virtuální sítě se API Management všechny koncové body služby (brána, portál pro vývojáře, portál vydavatele, přímou správu a Git) viditelné pouze uvnitř virtuální sítě, který ovládáte přístup.</span><span class="sxs-lookup"><span data-stu-id="00890-109">When API Management is deployed in an Internal Virtual network mode, all the service endpoints (gateway, developer portal, publisher portal, direct management and Git) are only visible inside a Virtual network that you control access to.</span></span> <span data-ttu-id="00890-110">Žádné koncové body služby jsou registrované na veřejném serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="00890-110">None of the service endpoints are registered on the Public DNS Server.</span></span>

<span data-ttu-id="00890-111">Použití služby API Management v interní režimu, můžete dosáhnout následujících scénářů</span><span class="sxs-lookup"><span data-stu-id="00890-111">Using API Management in Internal mode, you can achieve the following scenarios</span></span>
* <span data-ttu-id="00890-112">Ujistěte se, bezpečně rozhraní API hostovaných ve vašem privátním datacentru přístupné 3. stranami mimo pomocí připojení Site-to-Site a ExpressRoute VPN.</span><span class="sxs-lookup"><span data-stu-id="00890-112">Securely make APIs hosted in your private datacenter accessible by 3rd parties outside of it using Site-to-Site or ExpressRoute VPN connections.</span></span>
* <span data-ttu-id="00890-113">Povolte hybridní cloudové scénáře vystavení rozhraní API pro cloudové a místní rozhraní API prostřednictvím brány běžné.</span><span class="sxs-lookup"><span data-stu-id="00890-113">Enable hybrid cloud scenarios by exposing your cloud-based APIs and on-prem APIs through a common gateway.</span></span>
* <span data-ttu-id="00890-114">Spravujte vaše rozhraní API hostovaných ve více zeměpisných oblastí pomocí jeden koncový bod služby Gateway.</span><span class="sxs-lookup"><span data-stu-id="00890-114">Manage your APIs hosted in multiple geographic locations using a single Gateway endpoint.</span></span> 

## <span data-ttu-id="00890-115"><a name="enable-vpn"></a>Vytváření API Management v interní virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="00890-115"><a name="enable-vpn"> </a>Creating an API Management in Internal Virtual network</span></span>
<span data-ttu-id="00890-116">Služba API Management v interní virtuální sítě je hostován za k interní Balancer(ILB) zatížení.</span><span class="sxs-lookup"><span data-stu-id="00890-116">The API Management service in Internal Virtual network is hosted behind an Internal Load Balancer(ILB).</span></span> <span data-ttu-id="00890-117">IP adresa ILB je v [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) rozsahu.</span><span class="sxs-lookup"><span data-stu-id="00890-117">The IP Address of the ILB is in the [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) range.</span></span>  

### <a name="enable-vnet-connection-using-azure-portal"></a><span data-ttu-id="00890-118">Povolit připojení virtuální sítě pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="00890-118">Enable VNET connection using Azure portal</span></span>
<span data-ttu-id="00890-119">Pomocí následujícího postupu nejdřív vytvořit službu API Management [služba API Management vytvořit][Create API Management service].</span><span class="sxs-lookup"><span data-stu-id="00890-119">First create the API Management service by following the steps [Create API Management service][Create API Management service].</span></span> <span data-ttu-id="00890-120">Pak nastavení API Management k nasazení uvnitř virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="00890-120">Then set-up API Management to be deployed inside a Virtual network.</span></span>

![Nabídky pro nastavení APIM v interní virtuální síť][api-management-using-internal-vnet-menu]

<span data-ttu-id="00890-122">Po úspěšné nasazení, měli byste vidět interní virtuální IP adresu služby na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="00890-122">After the deployment succeeds, you should see the Internal Virtual IP Address of your service on the dashboard.</span></span>

![Řídicí panel správy rozhraní API s interní virtuální síť nakonfigurované][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a><span data-ttu-id="00890-124">Povolit připojení virtuální sítě pomocí rutin prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="00890-124">Enable VNET connection using Powershell cmdlets</span></span>
<span data-ttu-id="00890-125">Můžete také povolit připojení virtuální sítě pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="00890-125">You can also enable VNET connectivity using the PowerShell cmdlets.</span></span>

* <span data-ttu-id="00890-126">**Vytvoření služby API Management uvnitř virtuální sítě**: použijte rutinu [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) k vytvoření služby Azure API Management uvnitř virtuální sítě a nakonfigurujte ji chcete použít typ interní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="00890-126">**Create an API Management service inside a VNET**: Use the cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) to create an Azure API Management service inside a VNET and configure it to use the Internal VNET type.</span></span>

* <span data-ttu-id="00890-127">**Nasazení služby API Management existující uvnitř virtuální sítě**: použijte rutinu [aktualizace AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) přesunout existující službu Azure API Management uvnitř virtuální sítě a nakonfigurovat ji chcete použít typ interní virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="00890-127">**Deploy an existing API Management service inside a VNET**: Use the cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) to move an existing Azure API Management service inside a Virtual network and configure it to use Internal VNET type.</span></span>

## <span data-ttu-id="00890-128"><a name="apim-dns-configuration"></a>Konfigurace služby DNS</span><span class="sxs-lookup"><span data-stu-id="00890-128"><a name="apim-dns-configuration"></a>DNS configuration</span></span>
<span data-ttu-id="00890-129">Pokud používáte API Management v režimu externí virtuální sítě, DNS spravuje Azure.</span><span class="sxs-lookup"><span data-stu-id="00890-129">When using API Management in External Virtual network mode, DNS is managed by Azure.</span></span> <span data-ttu-id="00890-130">Pro režim interní virtuální sítě budete muset spravovat vlastní DNS.</span><span class="sxs-lookup"><span data-stu-id="00890-130">For Internal Virtual network mode, you have to manage your own DNS.</span></span>

> [!NOTE]
> <span data-ttu-id="00890-131">Služba API Management nepřijímá požadavky na požadavky přicházející na IP adresy.</span><span class="sxs-lookup"><span data-stu-id="00890-131">API Management service does not listen to requests coming on IP addresses.</span></span> <span data-ttu-id="00890-132">Pouze reaguje na požadavky na název hostitele nakonfigurovaného na jeho koncové body služby (která zahrnuje brány, portál pro vývojáře, portál vydavatele, koncový bod přímou správu a git).</span><span class="sxs-lookup"><span data-stu-id="00890-132">It only responds to requests to the Hostname configured on its service endpoints (which includes gateway, developer portal, publisher Portal, direct management endpoint, and git).</span></span>

### <a name="access-on-default-host-names"></a><span data-ttu-id="00890-133">Přístup na výchozí názvy hostitelů:</span><span class="sxs-lookup"><span data-stu-id="00890-133">Access on default host names:</span></span>
<span data-ttu-id="00890-134">Při vytváření služby API Management ve veřejném cloudu Azure, například s názvem "contoso", jsou ve výchozím nastavení nakonfigurované následující služby na koncové body.</span><span class="sxs-lookup"><span data-stu-id="00890-134">When you create an API Management service in public Azure cloud, named "contoso" for example, the following service endpoints are configured by default.</span></span>

>   <span data-ttu-id="00890-135">Brána nebo proxy server - contoso.azure api.net</span><span class="sxs-lookup"><span data-stu-id="00890-135">Gateway / Proxy - contoso.azure-api.net</span></span>

> <span data-ttu-id="00890-136">Portál vydavatele a portál pro vývojáře - contoso.portal.azure api.net</span><span class="sxs-lookup"><span data-stu-id="00890-136">Publisher Portal and Developer Portal - contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="00890-137">Přímá správa koncový bod - contoso.management.azure api.net</span><span class="sxs-lookup"><span data-stu-id="00890-137">Direct Management Endpoint - contoso.management.azure-api.net</span></span>

>   <span data-ttu-id="00890-138">GIT - contoso.scm.azure api.net</span><span class="sxs-lookup"><span data-stu-id="00890-138">GIT - contoso.scm.azure-api.net</span></span>

<span data-ttu-id="00890-139">Pro přístup k tyto koncové body služby API Management, můžete vytvořit virtuální počítač v podsíti, připojený k virtuální síti, ve kterém je nasazený API Management.</span><span class="sxs-lookup"><span data-stu-id="00890-139">To access these API Management service endpoints, you can create a Virtual Machine in a subnet connected to the Virtual network in which API Management is deployed.</span></span> <span data-ttu-id="00890-140">Za předpokladu, že interní virtuální IP adresy pro služby je 10.0.0.5, můžete provést hostitelů souboru mapování (% SystemDrive%\drivers\etc\hosts) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="00890-140">Assuming the Internal Virtual IP Address for your service is 10.0.0.5, you can do the hosts file mapping (%SystemDrive%\drivers\etc\hosts) as follows:</span></span>

> <span data-ttu-id="00890-141">10.0.0.5 contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00890-141">10.0.0.5    contoso.azure-api.net</span></span>

> <span data-ttu-id="00890-142">10.0.0.5 contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00890-142">10.0.0.5    contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="00890-143">10.0.0.5 contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00890-143">10.0.0.5    contoso.management.azure-api.net</span></span>

> <span data-ttu-id="00890-144">10.0.0.5 contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="00890-144">10.0.0.5    contoso.scm.azure-api.net</span></span>

<span data-ttu-id="00890-145">Všechny koncové body služby můžete poté přistoupit z virtuálního počítače, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="00890-145">You can then access all the service endpoints from the Virtual Machine you created.</span></span> <span data-ttu-id="00890-146">Pokud používáte server DNS vlastní ve virtuální síti, můžete také vytvořit záznamy A DNS a využít těchto koncových bodů z libovolného místa ve vaší virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="00890-146">If using a Custom DNS server in a Virtual network, you can also create A DNS records and access these endpoints from anywhere in your Virtual network.</span></span> 

### <a name="access-on-custom-domain-names"></a><span data-ttu-id="00890-147">Přístup na vlastní názvy domén:</span><span class="sxs-lookup"><span data-stu-id="00890-147">Access on custom domain names:</span></span>
<span data-ttu-id="00890-148">Pokud nechcete, aby přístup ke službě API Management s výchozí názvy hostitelů, můžete nastavit vlastní názvy domén pro všechny vaše koncové body služby jako níže</span><span class="sxs-lookup"><span data-stu-id="00890-148">If you don’t want to access the API Management service with the default host names, you can setup custom domain names for all your service endpoints like below</span></span>

![Nastavení vlastní domény pro rozhraní API Management][api-management-custom-domain-name]

<span data-ttu-id="00890-150">Potom můžete vytvořit záznamy v serveru DNS pro přístup k těchto koncových bodů, které jsou přístupné z ve vaší virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="00890-150">Then you can create A records in your DNS Server to access these endpoints which are only accessible from within your Virtual network.</span></span>

## <span data-ttu-id="00890-151"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="00890-151"><a name="related-content"> </a>Related content</span></span>
* <span data-ttu-id="00890-152">[Běžné problémy s konfigurací sítě při nastavování APIM ve virtuální síti][Common Network Configuration Issues]</span><span class="sxs-lookup"><span data-stu-id="00890-152">[Common Network configuration issues while setting up APIM in VNET][Common Network Configuration Issues]</span></span>
* [<span data-ttu-id="00890-153">Nejčastější dotazy virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="00890-153">Virtual Network faqs</span></span>](../virtual-network/virtual-networks-faq.md)
* [<span data-ttu-id="00890-154">Vytváření A záznam v DNS</span><span class="sxs-lookup"><span data-stu-id="00890-154">Creating A record in DNS</span></span>](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
