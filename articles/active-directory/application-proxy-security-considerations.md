---
title: "Důležité informace o zabezpečení pro proxy aplikace služby Azure AD | Microsoft Docs"
description: "Popisuje aspekty zabezpečení pomocí proxy aplikace služby Azure AD"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: c6ead651133eb17fd55f7567cdb14dc3bcd64245
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="security-considerations-for-accessing-apps-remotely-with-azure-ad-application-proxy"></a><span data-ttu-id="f0bf2-103">Důležité informace o zabezpečení pro přístup k aplikacím vzdáleně pomocí proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0bf2-103">Security considerations for accessing apps remotely with Azure AD Application Proxy</span></span>

<span data-ttu-id="f0bf2-104">Tento článek vysvětluje, jak Proxy aplikace služby Active Directory Azure poskytuje zabezpečené službu pro publikování a vzdálený přístup k vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-104">This article explains how Azure Active Directory Application Proxy provides a secure service for publishing and accessing your applications remotely.</span></span>

<span data-ttu-id="f0bf2-105">Následující diagram ukazuje, jak Azure AD umožňuje zabezpečený vzdálený přístup k místním aplikacím.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-105">The following diagram shows how Azure AD enables secure remote access to your on-premises applications.</span></span>

 ![Diagram zabezpečený vzdálený přístup prostřednictvím proxy aplikace služby Azure AD](./media/application-proxy-security-considerations/secure-remote-access.png)

## <a name="security-benefits"></a><span data-ttu-id="f0bf2-107">Výhody zabezpečení</span><span class="sxs-lookup"><span data-stu-id="f0bf2-107">Security benefits</span></span>

<span data-ttu-id="f0bf2-108">Azure AD Application Proxy nabízí následující výhody zabezpečení:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-108">Azure AD Application Proxy offers the following security benefits:</span></span>

### <a name="authenticated-access"></a><span data-ttu-id="f0bf2-109">Ověřený přístup</span><span class="sxs-lookup"><span data-stu-id="f0bf2-109">Authenticated access</span></span> 

<span data-ttu-id="f0bf2-110">Pokud se rozhodnete použít předběžné ověřování Azure Active Directory, můžete přístup pouze ověřené připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-110">If you choose to use Azure Active Directory preauthentication, then only authenticated connections can access your network.</span></span>

<span data-ttu-id="f0bf2-111">Azure AD Application Proxy spoléhá na Azure AD služby tokenů zabezpečení (STS) pro všechny ověřování.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-111">Azure AD Application Proxy relies on the Azure AD security token service (STS) for all authentication.</span></span>  <span data-ttu-id="f0bf2-112">Předběžné ověření, ze své podstaty blokuje velký počet anonymní útoky, protože pouze ověřených identit přístup k aplikaci back-end.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-112">Preauthentication, by its very nature, blocks a significant number of anonymous attacks, because only authenticated identities can access the back-end application.</span></span>

<span data-ttu-id="f0bf2-113">Pokud si zvolíte průchozí jako způsob předběžného ověření, neobdržíte této výhody.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-113">If you choose Passthrough as your preauthentication method, you don't get this benefit.</span></span> 

### <a name="conditional-access"></a><span data-ttu-id="f0bf2-114">Podmíněný přístup</span><span class="sxs-lookup"><span data-stu-id="f0bf2-114">Conditional access</span></span>

<span data-ttu-id="f0bf2-115">Použijete širší zásad řízení předtím, než se vytvoří připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-115">Apply richer policy controls before connections to your network are established.</span></span>

<span data-ttu-id="f0bf2-116">S [podmíněného přístupu](active-directory-conditional-access-azuread-connected-apps.md), můžete definovat omezení na jaký provoz je povolený přístup k aplikacím vaší back-end.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-116">With [conditional access](active-directory-conditional-access-azuread-connected-apps.md), you can define restrictions on what traffic is allowed to access your back-end applications.</span></span> <span data-ttu-id="f0bf2-117">Můžete vytvořit zásady, které omezují přihlášení v závislosti na umístění, sílu ověřování a riziko profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-117">You can create policies that restrict sign-ins based on location, strength of authentication, and user risk profile.</span></span>

<span data-ttu-id="f0bf2-118">Můžete taky podmíněného přístupu můžete nakonfigurovat zásady služby Multi-Factor Authentication, přidání další úroveň zabezpečení do vaší ověřování uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-118">You can also use conditional access to configure Multi-Factor Authentication policies, adding another layer of security to your user authentications.</span></span> 

### <a name="traffic-termination"></a><span data-ttu-id="f0bf2-119">Ukončení provozu</span><span class="sxs-lookup"><span data-stu-id="f0bf2-119">Traffic termination</span></span>

<span data-ttu-id="f0bf2-120">Veškerý provoz, je ukončen v cloudu.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-120">All traffic is terminated in the cloud.</span></span>

<span data-ttu-id="f0bf2-121">Proxy aplikace služby Azure AD je reverznímu proxy serveru, a proto je veškerý provoz do back-end aplikace ukončena na službu.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-121">Because Azure AD Application Proxy is a reverse-proxy, all traffic to back-end applications is terminated at the service.</span></span> <span data-ttu-id="f0bf2-122">Relaci můžete získat obnovila pouze s back-end serverů, což znamená, že nejsou pro přímý přenos HTTP zveřejněné back-end serverů.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-122">The session can get reestablished only with the back-end server, which means that your back-end servers are not exposed to direct HTTP traffic.</span></span> <span data-ttu-id="f0bf2-123">Tato konfigurace znamená, že je líp chráněný před cílenými útoky.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-123">This configuration means that you are better protected from targeted attacks.</span></span>

### <a name="all-access-is-outbound"></a><span data-ttu-id="f0bf2-124">Je odchozí veškerý přístup.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-124">All access is outbound</span></span> 

<span data-ttu-id="f0bf2-125">Nemusíte otevřete příchozí připojení k podnikové síti.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-125">You don't need to open inbound connections to the corporate network.</span></span>

<span data-ttu-id="f0bf2-126">Konektory Proxy aplikace použít pouze odchozí připojení ke službě Azure AD Application Proxy, což znamená, že je potřeba otevřít porty brány firewall pro příchozí spojení.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-126">Application Proxy connectors only use outbound connections to the Azure AD Application Proxy service, which means that there is no need to open firewall ports for incoming connections.</span></span> <span data-ttu-id="f0bf2-127">Tradiční proxy vyžaduje hraniční síti (označované také jako *DMZ*, *demilitarizovaná zóna*, nebo *monitorována podsíť*) a v hraniční sítě může být přístup k připojení bez ověření.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-127">Traditional proxies required a perimeter network (also known as *DMZ*, *demilitarized zone*, or *screened subnet*) and allowed access to unauthenticated connections at the network edge.</span></span> <span data-ttu-id="f0bf2-128">Tento scénář vyžaduje mnoho dodatečné investice do webové aplikace firewally analyzovat provoz a nabízet přidání ochrany do prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-128">This scenario required many additional investments in web application firewall products to analyze traffic and offer addition protections to the environment.</span></span> <span data-ttu-id="f0bf2-129">Pomocí Proxy aplikace nepotřebujete hraniční síti, protože všechna připojení jsou odchozí a provést přes zabezpečený kanál.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-129">With Application Proxy, you don't need a perimeter network because all connections are outbound and take place over a secure channel.</span></span>

<span data-ttu-id="f0bf2-130">Další informace o konektory najdete v tématu [pochopit Azure AD Application Proxy konektory](application-proxy-understand-connectors.md).</span><span class="sxs-lookup"><span data-stu-id="f0bf2-130">For more information about connectors, see [Understand Azure AD Application Proxy connectors](application-proxy-understand-connectors.md).</span></span>

### <a name="cloud-scale-analytics-and-machine-learning"></a><span data-ttu-id="f0bf2-131">Analýza cloudové škálování a machine learningu</span><span class="sxs-lookup"><span data-stu-id="f0bf2-131">Cloud-scale analytics and machine learning</span></span> 

<span data-ttu-id="f0bf2-132">Získáte ochranu nejmodernější zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-132">Get cutting-edge security protection.</span></span>

<span data-ttu-id="f0bf2-133">Protože je součástí služby Azure Active Directory, můžete využít Proxy aplikace [Azure AD Identity Protection](active-directory-identityprotection.md), s machine learning řízené intelligence a data z Microsoft Security Response Center a jejichž šíření tým Dcu.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-133">Because it's part of Azure Active Directory, Application Proxy can leverage [Azure AD Identity Protection](active-directory-identityprotection.md), with machine learning-driven intelligence and data from the Microsoft Security Response Center and Digital Crimes Unit.</span></span> <span data-ttu-id="f0bf2-134">Společně jsme aktivně identifikovat ohroženými účty a nabízejí ochranu v reálném čase z s vysokým rizikem přihlášení. Jsme vzít v úvahu mnoha faktorech, například přístup z nakažených zařízení prostřednictvím zavedení anonymity sítě a netypických a pravděpodobně umístění přístup.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-134">Together we proactively identify compromised accounts and offer real-time protection from high-risk sign-ins. We take into account numerous factors, such as access from infected devices, through anonymizing networks, and from atypical and unlikely locations.</span></span>

<span data-ttu-id="f0bf2-135">Mnoho tyto sestavy a události je již k dispozici prostřednictvím rozhraní API pro integraci s informace o zabezpečení a událostí systémy správy (SIEM).</span><span class="sxs-lookup"><span data-stu-id="f0bf2-135">Many of these reports and events are already available through an API for integration with your security information and event management (SIEM) systems.</span></span>

### <a name="remote-access-as-a-service"></a><span data-ttu-id="f0bf2-136">Vzdálený přístup jako služby</span><span class="sxs-lookup"><span data-stu-id="f0bf2-136">Remote access as a service</span></span>

<span data-ttu-id="f0bf2-137">Nemusíte si dělat starosti o údržbu a opravy na místní servery.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-137">You don’t have to worry about maintaining and patching on-premises servers.</span></span>

<span data-ttu-id="f0bf2-138">Neopravené softwaru stále účty pro velký počet útoků.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-138">Unpatched software still accounts for a large number of attacks.</span></span> <span data-ttu-id="f0bf2-139">Proxy aplikace služby Azure AD je internetových služba, která Microsoft vlastní, takže neustále stahovat nejnovější opravy zabezpečení a upgrady.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-139">Azure AD Application Proxy is an Internet-scale service that Microsoft owns, so you always get the latest security patches and upgrades.</span></span>

<span data-ttu-id="f0bf2-140">Chcete-li zlepšit zabezpečení aplikací publikovaných serverem proxy aplikace služby Azure AD, jsme zablokovat webové prohledávacího modulu robotů ze indexování a archivaci aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-140">To improve the security of applications published by Azure AD Application Proxy, we block web crawler robots from indexing and archiving your applications.</span></span> <span data-ttu-id="f0bf2-141">Pokaždé, když robot webový prohledávací modul, pokusí se načíst nastavení robot pro publikované aplikace Proxy aplikace odpoví odpovědí s robots.txt soubor, který zahrnuje `User-agent: * Disallow: /`.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-141">Each time a web crawler robot tries to retrieve the robot's settings for a published app, Application Proxy replies with a robots.txt file that includes `User-agent: * Disallow: /`.</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="f0bf2-142">Pod pokličkou</span><span class="sxs-lookup"><span data-stu-id="f0bf2-142">Under the hood</span></span>

<span data-ttu-id="f0bf2-143">Proxy aplikace služby Azure AD se skládá ze dvou částí:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-143">Azure AD Application Proxy consists of two parts:</span></span>

* <span data-ttu-id="f0bf2-144">Cloudové služby: Tato služba běží v Azure a je, kde jsou vytvářeny připojení externí klient/uživatel.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-144">The cloud-based service: This service runs in Azure, and is where the external client/user connections are made.</span></span>
* <span data-ttu-id="f0bf2-145">[Místní konektor](application-proxy-understand-connectors.md): komponentu místní konektor naslouchá požadavkům z Azure AD Application Proxy služby a obslužné rutiny připojení k interní aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-145">[The on-premises connector](application-proxy-understand-connectors.md): An on-premises component, the connector listens for requests from the Azure AD Application Proxy service and handles connections to the internal applications.</span></span> 

<span data-ttu-id="f0bf2-146">Tok mezi konektor a služba Proxy aplikace je vytvořeno při:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-146">A flow between the connector and the Application Proxy service is established when:</span></span>

* <span data-ttu-id="f0bf2-147">Konektor je nejdřív nastavit.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-147">The connector is first set up.</span></span>
* <span data-ttu-id="f0bf2-148">Konektor vrátí informace o konfiguraci ze služby Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-148">The connector pulls configuration information from the Application Proxy service.</span></span>
* <span data-ttu-id="f0bf2-149">Přístupu uživatele k publikované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-149">A user accesses a published application.</span></span>

>[!NOTE]
><span data-ttu-id="f0bf2-150">Veškerá komunikace probíhá přes protokol SSL a vždy vycházet v konektoru Proxy aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-150">All communications occur over SSL, and they always originate at the connector to the Application Proxy service.</span></span> <span data-ttu-id="f0bf2-151">Služba je pouze odchozí.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-151">The service is outbound only.</span></span>

<span data-ttu-id="f0bf2-152">Tento konektor využívá certifikát klienta k ověření služby Proxy aplikace u skoro všech volání.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-152">The connector uses a client certificate to authenticate to the Application Proxy service for nearly all calls.</span></span> <span data-ttu-id="f0bf2-153">Jedinou výjimkou tohoto procesu je počáteční nastavení krok, kde je navázat certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-153">The only exception to this process is the initial setup step, where the client certificate is established.</span></span>

### <a name="installing-the-connector"></a><span data-ttu-id="f0bf2-154">Instalace konektoru</span><span class="sxs-lookup"><span data-stu-id="f0bf2-154">Installing the connector</span></span>

<span data-ttu-id="f0bf2-155">Pokud konektor je nejdřív nastavený, následující události toku proběhnout:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-155">When the connector is first set up, the following flow events take place:</span></span>

1. <span data-ttu-id="f0bf2-156">Jako součást instalace konektoru se stane registrace konektoru ke službě.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-156">The connector registration to the service happens as part of the installation of the connector.</span></span> <span data-ttu-id="f0bf2-157">Uživatelé se výzva k zadání přihlašovacích údajů správce Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-157">Users are prompted to enter their Azure AD admin credentials.</span></span> <span data-ttu-id="f0bf2-158">Pro službu Azure AD Application Proxy pak zobrazí token získali z tohoto ověřování.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-158">The token acquired from this authentication is then presented to the Azure AD Application Proxy service.</span></span>
2. <span data-ttu-id="f0bf2-159">Služba Proxy aplikace vyhodnotí token.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-159">The Application Proxy service evaluates the token.</span></span> <span data-ttu-id="f0bf2-160">Zajišťuje, že uživatel je správce společnosti v rámci klienta, který byl token vydán pro.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-160">It ensures that the user is a company administrator within the tenant that the token was issued for.</span></span> <span data-ttu-id="f0bf2-161">Pokud uživatel není správcem, ukončení procesu.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-161">If the user is not an administrator, the process is terminated.</span></span>
3. <span data-ttu-id="f0bf2-162">Konektor generuje žádost o certifikát klienta a předává je, společně s token, služba Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-162">The connector generates a client certificate request and passes it, along with the token, to the Application Proxy service.</span></span> <span data-ttu-id="f0bf2-163">Služba zase ověří token a podepíše žádost o certifikát klienta.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-163">The service in turn verifies the token and signs the client certificate request.</span></span>
4. <span data-ttu-id="f0bf2-164">Tento konektor využívá certifikát klienta pro budoucí komunikaci se službou Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-164">The connector uses the client certificate for future communication with the Application Proxy service.</span></span>
5. <span data-ttu-id="f0bf2-165">Konektor provádí počáteční přijetí změn systému konfiguračních dat ze služby pomocí jeho klientský certifikát a je nyní připravena převzít požadavky.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-165">The connector performs an initial pull of the system configuration data from the service using its client certificate, and it is now ready to take requests.</span></span>

### <a name="updating-the-configuration-settings"></a><span data-ttu-id="f0bf2-166">Aktualizuje se nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="f0bf2-166">Updating the configuration settings</span></span>

<span data-ttu-id="f0bf2-167">Vždy, když služba Proxy aplikace aktualizuje nastavení konfigurace, následující události toku proběhnout:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-167">Whenever the Application Proxy service updates the configuration settings, the following flow events take place:</span></span>

1. <span data-ttu-id="f0bf2-168">Konektor připojuje ke koncovému bodu konfigurace v rámci služby Proxy aplikace pomocí jeho klientský certifikát.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-168">The connector connects to the configuration endpoint within the Application Proxy service by using its client certificate.</span></span>
2. <span data-ttu-id="f0bf2-169">Po klientský certifikát byl ověřen, služba Proxy aplikace vrátí konfigurační data do konektor (například konektor skupině, která konektor musí být součástí).</span><span class="sxs-lookup"><span data-stu-id="f0bf2-169">After the client certificate has been validated, the Application Proxy service returns configuration data to the connector (for example, the connector group that the connector should be part of).</span></span>
3. <span data-ttu-id="f0bf2-170">Pokud je aktuální certifikát víc než 180 dní, konektor generuje novou žádost o certifikát, který efektivně aktualizace certifikát klienta za 180 dní.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-170">If the current certificate is more than 180 days old, the connector generates a new certificate request, which effectively updates the client certificate every 180 days.</span></span>

### <a name="accessing-published-applications"></a><span data-ttu-id="f0bf2-171">Přístup k publikovaným aplikacím</span><span class="sxs-lookup"><span data-stu-id="f0bf2-171">Accessing published applications</span></span>

<span data-ttu-id="f0bf2-172">Při přístupu uživatelů k publikované aplikaci, následující události proběhnout mezi Proxy aplikace služby a konektor Proxy aplikace:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-172">When users access a published application, the following events take place between the Application Proxy service and the Application Proxy connector:</span></span>

1. [<span data-ttu-id="f0bf2-173">Služba ověřuje uživatele pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="f0bf2-173">The service authenticates the user for the app</span></span>](#the-service-checks-the-configuration-settings-for-the-app)
2. [<span data-ttu-id="f0bf2-174">Služba umístí požadavek ve frontě konektoru</span><span class="sxs-lookup"><span data-stu-id="f0bf2-174">The service places a request in the connector queue</span></span>](#The-service-places-a-request-in-the-connector-queue)
3. [<span data-ttu-id="f0bf2-175">Konektor zpracuje požadavek z fronty</span><span class="sxs-lookup"><span data-stu-id="f0bf2-175">A connector processes the request from the queue</span></span>](#the-connector-receives-the-request-from-the-queue)
4. [<span data-ttu-id="f0bf2-176">Konektor čeká na odpověď</span><span class="sxs-lookup"><span data-stu-id="f0bf2-176">The connector waits for a response</span></span>](#the-connector-waits-for-a-response)
5. [<span data-ttu-id="f0bf2-177">Služba datové proudy dat uživatele</span><span class="sxs-lookup"><span data-stu-id="f0bf2-177">The service streams data to the user</span></span>](#the-service-streams-data-to-the-user)

<span data-ttu-id="f0bf2-178">Chcete-li další informace o co probíhá v každé z těchto kroků, uchovávejte čtení.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-178">To learn more about what takes place in each of these steps, keep reading.</span></span>


#### <a name="1-the-service-authenticates-the-user-for-the-app"></a><span data-ttu-id="f0bf2-179">1. Služba ověřuje uživatele pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="f0bf2-179">1. The service authenticates the user for the app</span></span>

<span data-ttu-id="f0bf2-180">Pokud jste nakonfigurovali aplikaci, aby používala průchozí jako jeho metoda předběžného ověření, kroky v této části se přeskočí.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-180">If you configured the app to use Passthrough as its preauthentication method, the steps in this section are skipped.</span></span>

<span data-ttu-id="f0bf2-181">Pokud jste nakonfigurovali aplikaci preauthenticate s Azure AD, uživatelé přesměrovaní na tokenů zabezpečení Azure AD k ověření a provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-181">If you configured the app to preauthenticate with Azure AD, users are redirected to the Azure AD STS to authenticate, and the following steps take place:</span></span>

1. <span data-ttu-id="f0bf2-182">Proxy aplikací kontroluje všechny požadavky zásad podmíněného přístupu pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-182">Application Proxy checks for any conditional access policy requirements for the specific application.</span></span> <span data-ttu-id="f0bf2-183">Tento krok zajistí, že uživatel přiřazený k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-183">This step ensures that the user has been assigned to the application.</span></span> <span data-ttu-id="f0bf2-184">Pokud dvoustupňové ověření je potřeba, pořadí ověřování vyzve uživatele k druhé metody ověřování.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-184">If two-step verification is required, the authentication sequence prompts the user for a second authentication method.</span></span>

2. <span data-ttu-id="f0bf2-185">Po všechny kontroly, služby tokenů zabezpečení služby Azure AD vydá token podepsaný držitelem pro aplikaci a přesměruje uživatele zpět na Proxy aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-185">After all checks have passed, the Azure AD STS issues a signed token for the application and redirects the user back to the Application Proxy service.</span></span>

3. <span data-ttu-id="f0bf2-186">Proxy aplikací ověřuje, zda byl token vydán opravit aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-186">Application Proxy verifies that the token was issued to correct the application.</span></span> <span data-ttu-id="f0bf2-187">K dalším kontrolám vykonává také třeba zajistit, aby byl podepsán token službou Azure AD a že je stále v okně platný.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-187">It performs other checks also, such as ensuring that the token was signed by Azure AD, and that it is still within the valid window.</span></span>

4. <span data-ttu-id="f0bf2-188">Nastaví Proxy aplikace šifrovaný soubor cookie označíte, že ověřování do aplikace došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-188">Application Proxy sets an encrypted authentication cookie to indicate that authentication to the application has occurred.</span></span> <span data-ttu-id="f0bf2-189">Soubor cookie zahrnuje vypršení platnosti časového razítka, která je založená na tokenu z Azure AD a další data, jako je například uživatelské jméno, které je na základě ověření.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-189">The cookie includes an expiration timestamp that's based on the token from Azure AD and other data, such as the user name that the authentication is based on.</span></span> <span data-ttu-id="f0bf2-190">Soubor cookie je šifrován privátní klíč, který zná pouze služba Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-190">The cookie is encrypted with a private key known only to the Application Proxy service.</span></span>

5. <span data-ttu-id="f0bf2-191">Proxy aplikace přesměruje uživatele zpět na původně požadovanou URL adresu.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-191">Application Proxy redirects the user back to the originally requested URL.</span></span>

<span data-ttu-id="f0bf2-192">Pokud se libovolná součást kroky předběžného ověření nezdaří, je odepřena žádost uživatele a uživateli se zobrazí zprávu s upozorněním zdroj problému.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-192">If any part of the preauthentication steps fails, the user’s request is denied, and the user is shown a message indicating the source of the problem.</span></span>


#### <a name="2-the-service-places-a-request-in-the-connector-queue"></a><span data-ttu-id="f0bf2-193">2. Služba umístí požadavek ve frontě konektoru</span><span class="sxs-lookup"><span data-stu-id="f0bf2-193">2. The service places a request in the connector queue</span></span>

<span data-ttu-id="f0bf2-194">Konektory nechte odchozí připojení otevřené Proxy aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-194">Connectors keep an outbound connection open to the Application Proxy service.</span></span> <span data-ttu-id="f0bf2-195">Když přijde žádost, služba fronty požadavek na jedno z připojení k otevřete vyzvedne, až konektor.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-195">When a request comes in, the service queues up the request on one of the open connections for the connector to pick up.</span></span>

<span data-ttu-id="f0bf2-196">Požadavek obsahuje položky z aplikace, třeba hlavičky žádosti, dat z šifrovaného souboru cookie uživatele, který vytvořil požadavek a ID požadavku.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-196">The request includes items from the application, such as the request headers, data from the encrypted cookie, the user making the request, and the request ID.</span></span> <span data-ttu-id="f0bf2-197">I když se pošle dat z šifrovaného souboru cookie s požadavkem samotném souboru cookie ověřování není.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-197">Although data from the encrypted cookie is sent with the request, the authentication cookie itself is not.</span></span>

#### <a name="3-the-connector-processes-the-request-from-the-queue"></a><span data-ttu-id="f0bf2-198">3. Konektor zpracuje požadavek z fronty.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-198">3. The connector processes the request from the queue.</span></span> 

<span data-ttu-id="f0bf2-199">Na základě daného požadavku, provádí aplikace Proxy jednu z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="f0bf2-199">Based on the request, Application Proxy performs one of the following actions:</span></span>

* <span data-ttu-id="f0bf2-200">Pokud požadavek je jednoduchá operace (například nejsou žádná data v textu, jako je se dosáhl standardu RESTful *získat* požadavek), konektor vytvoří připojení k interní cílový prostředek a potom čeká na odpověď.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-200">If the request is a simple operation (for example, there is no data within the body as is with a RESTful *GET* request), the connector makes a connection to the target internal resource and then waits for a response.</span></span>

* <span data-ttu-id="f0bf2-201">Pokud požadavek má data přidružená v těle (například dosáhl standardu RESTful *POST* operaci), konektor vytvoří odchozí připojení pomocí certifikátu klienta do instance Proxy aplikace.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-201">If the request has data associated with it in the body (for example, a RESTful *POST* operation), the connector makes an outbound connection by using the client certificate to the Application Proxy instance.</span></span> <span data-ttu-id="f0bf2-202">Umožňuje toto připojení k vyžádání dat a otevřít připojení k interní prostředek.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-202">It makes this connection to request the data and open a connection to the internal resource.</span></span> <span data-ttu-id="f0bf2-203">Po přijetí žádosti z konektoru nástroje, služba Proxy aplikace začne přijímat obsah od uživatele a předává data ke konektoru.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-203">After it receives the request from the connector, the Application Proxy service begins accepting content from the user and forwards data to the connector.</span></span> <span data-ttu-id="f0bf2-204">Tento konektor, pak předá data do interní prostředku.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-204">The connector, in turn, forwards the data to the internal resource.</span></span>

#### <a name="4-the-connector-waits-for-a-response"></a><span data-ttu-id="f0bf2-205">4. Konektor čeká na odpověď.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-205">4. The connector waits for a response.</span></span>

<span data-ttu-id="f0bf2-206">Po dokončení požadavku a přenosu veškerý obsah na back-end konektor čeká na odpověď.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-206">After the request and transmission of all content to the back end is complete, the connector waits for a response.</span></span>

<span data-ttu-id="f0bf2-207">Po obdržení odpovědi, konektor vytvoří odchozí připojení ke službě Proxy aplikace vrátit podrobnosti záhlaví a začněte streamování vracená data.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-207">After it receives a response, the connector makes an outbound connection to the Application Proxy service, to return the header details and begin streaming the return data.</span></span>

#### <a name="5-the-service-streams-data-to-the-user"></a><span data-ttu-id="f0bf2-208">5. Služba datové proudy dat pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-208">5. The service streams data to the user.</span></span> 

<span data-ttu-id="f0bf2-209">Některé zpracování aplikace může dojít, zde.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-209">Some processing of the application may occur here.</span></span> <span data-ttu-id="f0bf2-210">Pokud jste nakonfigurovali Proxy aplikace přeložit hlavičky nebo adresy URL v aplikaci, se stane toto zpracování podle potřeby v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="f0bf2-210">If you configured Application Proxy to translate headers or URLs in your application, that processing happens as needed during this step.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f0bf2-211">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0bf2-211">Next steps</span></span>

[<span data-ttu-id="f0bf2-212">Aspekty topologie sítě při použití proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0bf2-212">Network topology considerations when using Azure AD Application Proxy</span></span>](application-proxy-network-topology-considerations.md)

[<span data-ttu-id="f0bf2-213">Pochopení konektory proxy aplikace služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0bf2-213">Understand Azure AD Application Proxy connectors</span></span>](application-proxy-understand-connectors.md)
