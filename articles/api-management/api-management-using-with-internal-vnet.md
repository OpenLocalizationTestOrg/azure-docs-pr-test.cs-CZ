---
title: "aaaHow toouse Azure API Management s interní virtuální sítě | Microsoft Docs"
description: "Zjistěte, jak toosetup a nakonfigurovat Azure API Management v interní virtuální síť."
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
ms.openlocfilehash: 8d25de14e0c0bebe7ba7b47ca432ea4e45dde312
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a><span data-ttu-id="d0572-103">Interní virtuální sítě pomocí služby Azure API Management</span><span class="sxs-lookup"><span data-stu-id="d0572-103">Using Azure API Management service with Internal Virtual Network</span></span>
<span data-ttu-id="d0572-104">Pomocí virtuální sítě Azure (virtuální sítě) můžete spravovat rozhraní API správy rozhraní API není dostupný na Internetu hello.</span><span class="sxs-lookup"><span data-stu-id="d0572-104">With Azure Virtual Networks (VNETs), API Management can manage APIs not accessible on hello Internet.</span></span> <span data-ttu-id="d0572-105">Počet technologie VPN jsou k dispozici toomake hello připojení a API Management se dá nasadit v dva hlavní režimy uvnitř hello virtuální sítě:</span><span class="sxs-lookup"><span data-stu-id="d0572-105">A number of VPN technologies are available toomake hello connection and API Management can be deployed in two main modes inside hello VNET:</span></span>
* <span data-ttu-id="d0572-106">Externí</span><span class="sxs-lookup"><span data-stu-id="d0572-106">External</span></span>
* <span data-ttu-id="d0572-107">Interní</span><span class="sxs-lookup"><span data-stu-id="d0572-107">Internal</span></span>

## <span data-ttu-id="d0572-108"><a name="overview"></a>Přehled</span><span class="sxs-lookup"><span data-stu-id="d0572-108"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="d0572-109">Po nasazení v režimu interní virtuální sítě se API Management jsou viditelné ve virtuální síti, která můžete řídit přístup ke pouze všechny hello koncové body služby (brána, portál pro vývojáře, portál vydavatele, přímou správu a Git).</span><span class="sxs-lookup"><span data-stu-id="d0572-109">When API Management is deployed in an Internal Virtual network mode, all hello service endpoints (gateway, developer portal, publisher portal, direct management and Git) are only visible inside a Virtual network that you control access to.</span></span> <span data-ttu-id="d0572-110">Žádné koncové body služby hello jsou registrované na hello veřejný Server DNS.</span><span class="sxs-lookup"><span data-stu-id="d0572-110">None of hello service endpoints are registered on hello Public DNS Server.</span></span>

<span data-ttu-id="d0572-111">Použití služby API Management v interní režimu, můžete dosáhnout hello následující scénáře</span><span class="sxs-lookup"><span data-stu-id="d0572-111">Using API Management in Internal mode, you can achieve hello following scenarios</span></span>
* <span data-ttu-id="d0572-112">Ujistěte se, bezpečně rozhraní API hostovaných ve vašem privátním datacentru přístupné 3. stranami mimo pomocí připojení Site-to-Site a ExpressRoute VPN.</span><span class="sxs-lookup"><span data-stu-id="d0572-112">Securely make APIs hosted in your private datacenter accessible by 3rd parties outside of it using Site-to-Site or ExpressRoute VPN connections.</span></span>
* <span data-ttu-id="d0572-113">Povolte hybridní cloudové scénáře vystavení rozhraní API pro cloudové a místní rozhraní API prostřednictvím brány běžné.</span><span class="sxs-lookup"><span data-stu-id="d0572-113">Enable hybrid cloud scenarios by exposing your cloud-based APIs and on-prem APIs through a common gateway.</span></span>
* <span data-ttu-id="d0572-114">Spravujte vaše rozhraní API hostovaných ve více zeměpisných oblastí pomocí jeden koncový bod služby Gateway.</span><span class="sxs-lookup"><span data-stu-id="d0572-114">Manage your APIs hosted in multiple geographic locations using a single Gateway endpoint.</span></span> 

## <span data-ttu-id="d0572-115"><a name="enable-vpn"></a>Vytváření API Management v interní virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d0572-115"><a name="enable-vpn"> </a>Creating an API Management in Internal Virtual network</span></span>
<span data-ttu-id="d0572-116">Hello služba API Management v interní virtuální sítě je hostován za k interní Balancer(ILB) zatížení.</span><span class="sxs-lookup"><span data-stu-id="d0572-116">hello API Management service in Internal Virtual network is hosted behind an Internal Load Balancer(ILB).</span></span> <span data-ttu-id="d0572-117">Hello IP adresu hello ILB je v hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) rozsahu.</span><span class="sxs-lookup"><span data-stu-id="d0572-117">hello IP Address of hello ILB is in hello [RFC1918](http://www.faqs.org/rfcs/rfc1918.html) range.</span></span>  

### <a name="enable-vnet-connection-using-azure-portal"></a><span data-ttu-id="d0572-118">Povolit připojení virtuální sítě pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d0572-118">Enable VNET connection using Azure portal</span></span>
<span data-ttu-id="d0572-119">Služba API Management hello nejdřív vytvořit pomocí následujících kroků hello [služba API Management vytvořit][Create API Management service].</span><span class="sxs-lookup"><span data-stu-id="d0572-119">First create hello API Management service by following hello steps [Create API Management service][Create API Management service].</span></span> <span data-ttu-id="d0572-120">Pak nastavení API Management toobe nasazené uvnitř virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d0572-120">Then set-up API Management toobe deployed inside a Virtual network.</span></span>

![Nabídky pro nastavení APIM v interní virtuální síť][api-management-using-internal-vnet-menu]

<span data-ttu-id="d0572-122">Po úspěšné nasazení hello, měli byste vidět hello interní virtuální IP adresy služby na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="d0572-122">After hello deployment succeeds, you should see hello Internal Virtual IP Address of your service on hello dashboard.</span></span>

![Řídicí panel správy rozhraní API s interní virtuální síť nakonfigurované][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a><span data-ttu-id="d0572-124">Povolit připojení virtuální sítě pomocí rutin prostředí Powershell</span><span class="sxs-lookup"><span data-stu-id="d0572-124">Enable VNET connection using Powershell cmdlets</span></span>
<span data-ttu-id="d0572-125">Můžete také povolit připojení virtuální sítě pomocí rutin prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="d0572-125">You can also enable VNET connectivity using hello PowerShell cmdlets.</span></span>

* <span data-ttu-id="d0572-126">**Vytvoření služby API Management uvnitř virtuální sítě**: použijte rutinu hello [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate službě Azure API Management služby uvnitř virtuální sítě a nakonfigurujte ji toouse hello interní virtuální síť typu.</span><span class="sxs-lookup"><span data-stu-id="d0572-126">**Create an API Management service inside a VNET**: Use hello cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) toocreate an Azure API Management service inside a VNET and configure it toouse hello Internal VNET type.</span></span>

* <span data-ttu-id="d0572-127">**Nasazení služby API Management existující uvnitř virtuální sítě**: použijte rutinu hello [aktualizace AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove existující Azure API Management služby uvnitř virtuální sítě a nakonfigurujte ji toouse Interní typ virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d0572-127">**Deploy an existing API Management service inside a VNET**: Use hello cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) toomove an existing Azure API Management service inside a Virtual network and configure it toouse Internal VNET type.</span></span>

## <span data-ttu-id="d0572-128"><a name="apim-dns-configuration"></a>Konfigurace služby DNS</span><span class="sxs-lookup"><span data-stu-id="d0572-128"><a name="apim-dns-configuration"></a>DNS configuration</span></span>
<span data-ttu-id="d0572-129">Pokud používáte API Management v režimu externí virtuální sítě, DNS spravuje Azure.</span><span class="sxs-lookup"><span data-stu-id="d0572-129">When using API Management in External Virtual network mode, DNS is managed by Azure.</span></span> <span data-ttu-id="d0572-130">Interní virtuální sítě režimu je nutné toomanage vlastní DNS.</span><span class="sxs-lookup"><span data-stu-id="d0572-130">For Internal Virtual network mode, you have toomanage your own DNS.</span></span>

> [!NOTE]
> <span data-ttu-id="d0572-131">Služba API Management nenaslouchá toorequests přicházející na IP adresy.</span><span class="sxs-lookup"><span data-stu-id="d0572-131">API Management service does not listen toorequests coming on IP addresses.</span></span> <span data-ttu-id="d0572-132">Odpovídá pouze toorequests toohello název hostitele nakonfigurovaného na jeho koncové body služby, (která zahrnuje brány, portál pro vývojáře, portál vydavatele, koncový bod přímou správu a git).</span><span class="sxs-lookup"><span data-stu-id="d0572-132">It only responds toorequests toohello Hostname configured on its service endpoints (which includes gateway, developer portal, publisher Portal, direct management endpoint, and git).</span></span>

### <a name="access-on-default-host-names"></a><span data-ttu-id="d0572-133">Přístup na výchozí názvy hostitelů:</span><span class="sxs-lookup"><span data-stu-id="d0572-133">Access on default host names:</span></span>
<span data-ttu-id="d0572-134">Při vytváření služby API Management ve veřejném cloudu Azure, například s názvem "contoso", hello následující koncové body služby jsou nakonfigurované ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d0572-134">When you create an API Management service in public Azure cloud, named "contoso" for example, hello following service endpoints are configured by default.</span></span>

>   <span data-ttu-id="d0572-135">Brána nebo proxy server - contoso.azure api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-135">Gateway / Proxy - contoso.azure-api.net</span></span>

> <span data-ttu-id="d0572-136">Portál vydavatele a portál pro vývojáře - contoso.portal.azure api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-136">Publisher Portal and Developer Portal - contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="d0572-137">Přímá správa koncový bod - contoso.management.azure api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-137">Direct Management Endpoint - contoso.management.azure-api.net</span></span>

>   <span data-ttu-id="d0572-138">GIT - contoso.scm.azure api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-138">GIT - contoso.scm.azure-api.net</span></span>

<span data-ttu-id="d0572-139">tooaccess těchto koncových bodů služby API Management, můžete vytvořit virtuální počítač ve virtuální síti toohello připojené podsítě ve kterém je nasazený API Management.</span><span class="sxs-lookup"><span data-stu-id="d0572-139">tooaccess these API Management service endpoints, you can create a Virtual Machine in a subnet connected toohello Virtual network in which API Management is deployed.</span></span> <span data-ttu-id="d0572-140">Za předpokladu, že je 10.0.0.5 hello interní virtuální IP adresy pro vaši službu, můžete provést hello mapování souborů hostitele (% SystemDrive%\drivers\etc\hosts) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d0572-140">Assuming hello Internal Virtual IP Address for your service is 10.0.0.5, you can do hello hosts file mapping (%SystemDrive%\drivers\etc\hosts) as follows:</span></span>

> <span data-ttu-id="d0572-141">10.0.0.5 contoso.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-141">10.0.0.5    contoso.azure-api.net</span></span>

> <span data-ttu-id="d0572-142">10.0.0.5 contoso.portal.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-142">10.0.0.5    contoso.portal.azure-api.net</span></span>

> <span data-ttu-id="d0572-143">10.0.0.5 contoso.management.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-143">10.0.0.5    contoso.management.azure-api.net</span></span>

> <span data-ttu-id="d0572-144">10.0.0.5 contoso.scm.azure-api.net</span><span class="sxs-lookup"><span data-stu-id="d0572-144">10.0.0.5    contoso.scm.azure-api.net</span></span>

<span data-ttu-id="d0572-145">Všechny koncové body služby hello můžete poté přistoupit z hello virtuálního počítače, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d0572-145">You can then access all hello service endpoints from hello Virtual Machine you created.</span></span> <span data-ttu-id="d0572-146">Pokud používáte server DNS vlastní ve virtuální síti, můžete také vytvořit záznamy A DNS a využít těchto koncových bodů z libovolného místa ve vaší virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="d0572-146">If using a Custom DNS server in a Virtual network, you can also create A DNS records and access these endpoints from anywhere in your Virtual network.</span></span> 

### <a name="access-on-custom-domain-names"></a><span data-ttu-id="d0572-147">Přístup na vlastní názvy domén:</span><span class="sxs-lookup"><span data-stu-id="d0572-147">Access on custom domain names:</span></span>
<span data-ttu-id="d0572-148">Pokud nechcete, aby tooaccess hello služba API Management s názvy hostitelů hello výchozí, můžete nastavit vlastní názvy domén pro všechny vaše koncové body služby jako níže</span><span class="sxs-lookup"><span data-stu-id="d0572-148">If you don’t want tooaccess hello API Management service with hello default host names, you can setup custom domain names for all your service endpoints like below</span></span>

![Nastavení vlastní domény pro rozhraní API Management][api-management-custom-domain-name]

<span data-ttu-id="d0572-150">Potom můžete vytvořit záznamy A v vašeho serveru DNS tooaccess těchto koncových bodů, které jsou přístupné z ve vaší virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="d0572-150">Then you can create A records in your DNS Server tooaccess these endpoints which are only accessible from within your Virtual network.</span></span>

## <span data-ttu-id="d0572-151"><a name="related-content"></a>Související obsah</span><span class="sxs-lookup"><span data-stu-id="d0572-151"><a name="related-content"> </a>Related content</span></span>
* <span data-ttu-id="d0572-152">[Běžné problémy s konfigurací sítě při nastavování APIM ve virtuální síti][Common Network Configuration Issues]</span><span class="sxs-lookup"><span data-stu-id="d0572-152">[Common Network configuration issues while setting up APIM in VNET][Common Network Configuration Issues]</span></span>
* [<span data-ttu-id="d0572-153">Nejčastější dotazy virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d0572-153">Virtual Network faqs</span></span>](../virtual-network/virtual-networks-faq.md)
* [<span data-ttu-id="d0572-154">Vytváření A záznam v DNS</span><span class="sxs-lookup"><span data-stu-id="d0572-154">Creating A record in DNS</span></span>](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues
