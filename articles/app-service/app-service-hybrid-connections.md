---
title: "Aplikační služba Azure hybridních připojení | Microsoft Docs"
description: "Jak vytvořit a používat hybridní připojení pro přístup k prostředkům v sítích různorodých"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a><span data-ttu-id="0bc4b-103">Hybridní připojení služby Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0bc4b-103">Azure App Service Hybrid Connections</span></span> #

## <a name="overview"></a><span data-ttu-id="0bc4b-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="0bc4b-104">Overview</span></span> ##

<span data-ttu-id="0bc4b-105">Hybridní připojení je součástí služby v Azure jak funkce ve službě Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-105">Hybrid Connections is both a service in Azure as well as a feature in the Azure App Service.</span></span>  <span data-ttu-id="0bc4b-106">Jako služba má použití a možnosti nad rámec těch, které jsou využity v Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-106">As a service it has use and capabilities beyond those that are leveraged in the Azure App Service.</span></span>  <span data-ttu-id="0bc4b-107">Další informace o hybridních připojení a jejich využití mimo Azure App Service, můžete spustit zde [Azure předávání hybridní připojení][HCService]</span><span class="sxs-lookup"><span data-stu-id="0bc4b-107">To learn more about Hybrid Connections and their usage outside of the Azure App Service you can start here, [Azure Relay Hybrid Connections][HCService]</span></span>

<span data-ttu-id="0bc4b-108">V rámci Azure App Service hybridní připojení slouží pro přístup k prostředkům aplikací v jiných sítích.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-108">Within the Azure App Service, hybrid connections can be used to access application resources in other networks.</span></span> <span data-ttu-id="0bc4b-109">Poskytuje přístup z vaší aplikace pro koncový bod aplikace.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-109">It provides access FROM your app TO an application endpoint.</span></span>  <span data-ttu-id="0bc4b-110">Alternativní schopnost přístup k aplikaci není povolen.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-110">It does not enable an alternative capability to access your application.</span></span>  <span data-ttu-id="0bc4b-111">Používá v App Service, každý hybridní připojení korelaci na jedné kombinaci hostitele a portu TCP.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-111">As used in the App Service, each hybrid connection correlates to a single TCP host and port combination.</span></span>  <span data-ttu-id="0bc4b-112">To znamená, že koncový bod hybridní připojení může být v jakémkoliv operačním systému a všechny aplikace, pokud že jste nedosáhli naslouchání portu TCP.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-112">This means that the hybrid connection endpoint can be on any operating system and any application provided you are hitting a TCP listening port.</span></span> <span data-ttu-id="0bc4b-113">Hybridní připojení neznáte nebo vědět, co je aplikační protokol nebo co se připojujete.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-113">Hybrid connections does not know or care what the application protocol is or what you are accessing.</span></span>  <span data-ttu-id="0bc4b-114">Jednoduše poskytuje přístup k síti.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-114">It is simply providing network access.</span></span>  


## <a name="how-it-works"></a><span data-ttu-id="0bc4b-115">Jak to funguje</span><span class="sxs-lookup"><span data-stu-id="0bc4b-115">How it works</span></span> ##
<span data-ttu-id="0bc4b-116">Funkce hybridního připojení se skládá z dvě odchozí volání předávání přes Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-116">The hybrid connections feature consists of two outbound calls to Service Bus Relay.</span></span>  <span data-ttu-id="0bc4b-117">Existuje připojení z knihovny na hostiteli, kde vaše aplikace běží ve službě app service a nejsou k dispozici z Manager(HCM) připojení hybridní připojení k předávání přes Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-117">There is a connection from a library on the host where your app is running in the app service and then there is a connection from the Hybrid Connection Manager(HCM) to Service Bus relay.</span></span>  <span data-ttu-id="0bc4b-118">HCM je předávací služba, která můžete nasadit v rámci sítě hostování</span><span class="sxs-lookup"><span data-stu-id="0bc4b-118">The HCM is a relay service that you deploy within the network hosting</span></span> 

<span data-ttu-id="0bc4b-119">Přes dvě připojení připojené k má vaše aplikace na druhé straně HCM tunel ke kombinaci pevné hostitele a portu TCP.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-119">Through the two joined connections your app has a TCP tunnel to a fixed host:port combination on the other side of the HCM.</span></span>  <span data-ttu-id="0bc4b-120">Připojení používá TLS 1.2 pro zabezpečení a klíče SAS pro ověřování nebo autorizace.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-120">The connection uses TLS 1.2 for security and SAS keys for authentication/authorization.</span></span>    

![][1]

<span data-ttu-id="0bc4b-121">Pokud vaše aplikace provede požadavek DNS, který odpovídá koncový bod konfigurovat hybridní připojení, bude odchozí přenosy TCP přesměrován dolů hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-121">When your app makes a DNS request that matches a configure Hybrid Connection endpoint, then the outbound TCP traffic will be redirected down the hybrid connection.</span></span>  

> [!NOTE]
> <span data-ttu-id="0bc4b-122">To znamená, že byste měli zkusit vždy použijte název DNS pro hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-122">This means that you should try to always use a DNS name for your Hybrid Connection.</span></span>  <span data-ttu-id="0bc4b-123">Některé klientský software neprovádí vyhledávání DNS, pokud koncový bod místo toho používá IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-123">Some client software does not do a DNS lookup if the endpoint uses an IP address instead.</span></span>
>
>

<span data-ttu-id="0bc4b-124">Existují dva typy hybridní připojení, nové hybridní připojení, které nabízí jako službu v Azure předávání a starší BizTalk hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-124">There are two types of hybrid connections, the new hybrid connections that are offered as a service under Azure Relay and the older BizTalk Hybrid Connections.</span></span>  <span data-ttu-id="0bc4b-125">Starší BizTalk hybridní připojení jsou označovány jako Classic hybridní připojení na portálu.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-125">The older BizTalk Hybrid Connections are referred to as Classic Hybrid Connections in the portal.</span></span>  <span data-ttu-id="0bc4b-126">Další informace dále v tomto dokumentu o nich není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-126">There is more information later in this document about them.</span></span>

### <a name="app-service-hybrid-connection-benefits"></a><span data-ttu-id="0bc4b-127">Služby App Service hybridní připojení výhody</span><span class="sxs-lookup"><span data-stu-id="0bc4b-127">App Service hybrid connection benefits</span></span> ###

<span data-ttu-id="0bc4b-128">Existuje několik výhod včetně schopnosti hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-128">There are a number of benefits to the hybrid connections capability including</span></span>

- <span data-ttu-id="0bc4b-129">Aplikací mít bezpečný přístup na místní systémy a služby bezpečně</span><span class="sxs-lookup"><span data-stu-id="0bc4b-129">Apps can securely access on premises systems and services securely</span></span>
- <span data-ttu-id="0bc4b-130">Tato funkce nevyžaduje internet dostupný koncový bod</span><span class="sxs-lookup"><span data-stu-id="0bc4b-130">the feature does not require an internet accessible endpoint</span></span>
- <span data-ttu-id="0bc4b-131">je rychlý a snadný nastavit</span><span class="sxs-lookup"><span data-stu-id="0bc4b-131">it is quick and easy to set up</span></span>  
- <span data-ttu-id="0bc4b-132">každé hybridní připojení odpovídá kombinaci jednoho hostitele a portu, který je vynikající zabezpečení aspekt</span><span class="sxs-lookup"><span data-stu-id="0bc4b-132">each hybrid connection matches to a single host:port combination which is an excellent security aspect</span></span>
- <span data-ttu-id="0bc4b-133">za normálních okolností nevyžaduje brány firewall díry přes standardní webu porty jsou všechny odchozí připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-133">it normally does not require firewall holes as the connections are all outbound over standard web ports</span></span>
- <span data-ttu-id="0bc4b-134">protože tato funkce je úroveň sítě, který také znamená, že je lhostejné jazyk používaný aplikace a z technologie používané v koncovém bodě</span><span class="sxs-lookup"><span data-stu-id="0bc4b-134">because the feature is network level that also means that it is agnostic to the language used by your app and the technology used by the endpoint</span></span>
- <span data-ttu-id="0bc4b-135">může sloužit k poskytování přístupu ve více sítí z jedné aplikace</span><span class="sxs-lookup"><span data-stu-id="0bc4b-135">it can be used to provide access in multiple networks from a single app</span></span> 

### <a name="things-you-cannot-do-with-hybrid-connections"></a><span data-ttu-id="0bc4b-136">Kroky, které nelze provést s hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-136">Things you cannot do with Hybrid Connections</span></span> ###

<span data-ttu-id="0bc4b-137">Existuje několik možností, nemůžete dělat s hybridní připojení a patří mezi ně:</span><span class="sxs-lookup"><span data-stu-id="0bc4b-137">There are a few things you cannot do with hybrid connections and they include:</span></span>

- <span data-ttu-id="0bc4b-138">připojení na jednotku</span><span class="sxs-lookup"><span data-stu-id="0bc4b-138">mounting a drive</span></span>
- <span data-ttu-id="0bc4b-139">pomocí protokolu UDP</span><span class="sxs-lookup"><span data-stu-id="0bc4b-139">using UDP</span></span>
- <span data-ttu-id="0bc4b-140">přístup k protokolu TCP na základě služby, které používají dynamické porty třeba pasivní režim FTP nebo rozšířený pasivní režim</span><span class="sxs-lookup"><span data-stu-id="0bc4b-140">accessing TCP based services that use dynamic ports such as FTP Passive Mode or Extended Passive Mode</span></span>
- <span data-ttu-id="0bc4b-141">Podporu protokolu LDAP, jako je někdy vyžaduje UDP</span><span class="sxs-lookup"><span data-stu-id="0bc4b-141">LDAP support, as it sometimes requires UDP</span></span>
- <span data-ttu-id="0bc4b-142">Podpora služby Active Directory</span><span class="sxs-lookup"><span data-stu-id="0bc4b-142">Active Directory support</span></span>

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a><span data-ttu-id="0bc4b-143">Přidávání a vytváření hybridní připojení v aplikaci</span><span class="sxs-lookup"><span data-stu-id="0bc4b-143">Adding and Creating a Hybrid Connection in your App</span></span> ##

<span data-ttu-id="0bc4b-144">Hybridní připojení lze vytvořit pomocí portálu aplikace nebo na portálu služby hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-144">Hybrid Connections can be created through the app portal or from the Hybrid Connection service portal.</span></span>  <span data-ttu-id="0bc4b-145">Důrazně doporučujeme vytvořit hybridní připojení, které chcete použít s vaší aplikací pomocí aplikace portálu.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-145">It is highly recommended that you use the app portal to create the Hybrid Connections you wish to use with your app.</span></span>  <span data-ttu-id="0bc4b-146">Chcete-li vytvořit hybridní připojení. přejděte na [portál Azure] [ portal] a přejděte do uživatelského rozhraní pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-146">To create a Hybrid Connection go to the [Azure portal][portal] and go into the UI for your app.</span></span>  <span data-ttu-id="0bc4b-147">Vyberte **sítě > Konfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-147">Select **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="0bc4b-148">Zde vidíte hybridní připojení, které jsou nakonfigurované s vaší aplikací</span><span class="sxs-lookup"><span data-stu-id="0bc4b-148">From here you can see the Hybrid Connections that are configured with your app</span></span>  

![][2]

<span data-ttu-id="0bc4b-149">Chcete-li přidat nové hybridní připojení, klikněte na tlačítko Přidat hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-149">To add a new Hybrid Connection, click Add Hybrid Connection.</span></span>  <span data-ttu-id="0bc4b-150">Uživatelské rozhraní, které se otevře seznamy hybridní připojení, který jste už vytvořili.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-150">The UI that opens up lists the Hybrid Connections that you have already created.</span></span>  <span data-ttu-id="0bc4b-151">Chcete-li přidat jeden nebo více těchto do vaší aplikace, klikněte na ty, které chcete a stiskněte tlačítko **přidat vybrané hybridní připojení**.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-151">To add one or more of them to your app, click on the ones you want and hit **Add selected hybrid connection**.</span></span>  

![][3]

<span data-ttu-id="0bc4b-152">Pokud chcete vytvořit nové hybridní připojení, klikněte na tlačítko **vytvořit nové hybridní připojení**.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-152">If you want to create a new Hybrid Connection, click **Create new hybrid connection**.</span></span>  <span data-ttu-id="0bc4b-153">Tady zadáte:</span><span class="sxs-lookup"><span data-stu-id="0bc4b-153">From here you specify the:</span></span> 

- <span data-ttu-id="0bc4b-154">název koncového bodu</span><span class="sxs-lookup"><span data-stu-id="0bc4b-154">endpoint name</span></span>
- <span data-ttu-id="0bc4b-155">hostitele koncového bodu</span><span class="sxs-lookup"><span data-stu-id="0bc4b-155">endpoint hostname</span></span>
- <span data-ttu-id="0bc4b-156">koncový port</span><span class="sxs-lookup"><span data-stu-id="0bc4b-156">endpoint port</span></span>
- <span data-ttu-id="0bc4b-157">obor názvů sběrnice, který chcete použít</span><span class="sxs-lookup"><span data-stu-id="0bc4b-157">servicebus namespace you wish to use</span></span>

![][4]

<span data-ttu-id="0bc4b-158">Každý hybridní připojení je vázaný na obor názvů sběrnice a každý obor názvů sběrnice je v oblasti Azure.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-158">Every hybrid connection is tied to a service bus namespace and each service bus namespace is in an Azure region.</span></span>  <span data-ttu-id="0bc4b-159">Je důležité se pokusit použít ve stejné oblasti jako vaše aplikace. aby se zabránilo latence sítě vyvolané obor názvů sběrnice.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-159">It is important to try and use a service bus namespace in the same region as your app so as to avoid network induced latency.</span></span>

<span data-ttu-id="0bc4b-160">Pokud chcete odebrat hybridní připojení z vaší aplikace, klikněte na něj pravým tlačítkem a vyberte **odpojení**.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-160">If you want to remove your hybrid connection from your app, right click on it and select **Disconnect**.</span></span>  

<span data-ttu-id="0bc4b-161">Jakmile hybridní připojení se přidá do webové aplikace, uvidíte na něm podrobnosti jednoduše kliknutím na něm.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-161">Once a hybrid connection is added to your web app, you can see details on it by simply clicking on it.</span></span>  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a><span data-ttu-id="0bc4b-162">Hybridní připojení a plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="0bc4b-162">Hybrid Connections and App Service Plans</span></span> ##

<span data-ttu-id="0bc4b-163">Pouze hybridní připojení, které nyní můžete provést jsou nové hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-163">The only hybrid connections you can now make are the new hybrid connections.</span></span>  <span data-ttu-id="0bc4b-164">Jsou k dispozici v Basic, Standard, Premium a izolovaná ceny SKU.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-164">They are only available in Basic, Standard, Premium and Isolated pricing SKUs.</span></span>  <span data-ttu-id="0bc4b-165">Existují omezení svázané s cenový plán.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-165">There are limits tied to the pricing plan.</span></span>  

| <span data-ttu-id="0bc4b-166">Ceny plán</span><span class="sxs-lookup"><span data-stu-id="0bc4b-166">Pricing Plan</span></span> | <span data-ttu-id="0bc4b-167">Počet hybridní připojení použitelné v plánu</span><span class="sxs-lookup"><span data-stu-id="0bc4b-167">Number of hybrid connections usable in the plan</span></span> |
|----|----|
| <span data-ttu-id="0bc4b-168">Basic</span><span class="sxs-lookup"><span data-stu-id="0bc4b-168">Basic</span></span> | <span data-ttu-id="0bc4b-169">5</span><span class="sxs-lookup"><span data-stu-id="0bc4b-169">5</span></span> |
| <span data-ttu-id="0bc4b-170">Standard</span><span class="sxs-lookup"><span data-stu-id="0bc4b-170">Standard</span></span> | <span data-ttu-id="0bc4b-171">25</span><span class="sxs-lookup"><span data-stu-id="0bc4b-171">25</span></span> |
| <span data-ttu-id="0bc4b-172">Premium</span><span class="sxs-lookup"><span data-stu-id="0bc4b-172">Premium</span></span> | <span data-ttu-id="0bc4b-173">200</span><span class="sxs-lookup"><span data-stu-id="0bc4b-173">200</span></span> |
| <span data-ttu-id="0bc4b-174">Isolated</span><span class="sxs-lookup"><span data-stu-id="0bc4b-174">Isolated</span></span> | <span data-ttu-id="0bc4b-175">200</span><span class="sxs-lookup"><span data-stu-id="0bc4b-175">200</span></span> |

<span data-ttu-id="0bc4b-176">Vzhledem k tomu, že existují omezení plán služby App Service je také uživatelského rozhraní v plán služby App Service, ukazuje, kolik hybridní připojení se používají a které aplikace.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-176">Since there are App Service Plan restrictions there is also UI in the App Service Plan that shows you how many hybrid connections are being used and by what apps.</span></span>  

![][6]

<span data-ttu-id="0bc4b-177">Stejně jako s zobrazení aplikace se zobrazí podrobnosti na hybridní připojení kliknutím na.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-177">Just as with the app view, you can see details on your hybrid connection by clicking on it.</span></span>  <span data-ttu-id="0bc4b-178">Ve vlastnostech zobrazeny zde můžete zobrazit všechny informace, které jste měli v zobrazení aplikace ale můžete také zjistit, kolik aplikací v stejný plán služby App Service používáte hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-178">In the properties shown here you can see all the information you had at the app view but can also see how many other apps in the same App Service Plan are using that hybrid connection.</span></span>

![][7]

<span data-ttu-id="0bc4b-179">Zatímco je omezený počet koncové body hybridního připojení, které mohou být používány plánu služby App Service, každý hybridní připojení používají lze napříč libovolný počet aplikací v tomto plánu služby App Service.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-179">While there is a limit on the number of hybrid connection endpoints that can be used in an App Service Plan, each hybrid connection used can be used across any number of apps in that App Service Plan.</span></span>  <span data-ttu-id="0bc4b-180">To je říkají, že pokud došlo k 1 hybridní připojení, který lze použít v 5 samostatných aplikací my plán služby App Service, která je stále právě 1 hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-180">That is to say that if I had 1 hybrid connection that I used in 5 separate apps in my App Service Plan, that is still just 1 hybrid connection.</span></span>

<span data-ttu-id="0bc4b-181">Není dalších nákladů na hybridní připojení nad rámec se dá se použít v Basic, Standard, Premium nebo izolované SKU.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-181">There is an additional cost to hybrid connections beyond being only usable in a Basic, Standard, Premium or Isolated SKU.</span></span>  <span data-ttu-id="0bc4b-182">Podrobnosti o cenách pro hybridní připojení prosím naleznete zde: [ceny služby Service Bus][sbpricing].</span><span class="sxs-lookup"><span data-stu-id="0bc4b-182">For details on hybrid connection pricing please go here: [Service Bus pricing][sbpricing].</span></span>

## <a name="hybrid-connection-manager"></a><span data-ttu-id="0bc4b-183">Správce hybridního připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-183">Hybrid Connection Manager</span></span> ##

<span data-ttu-id="0bc4b-184">V pořadí pro hybridní připojení k pracovním musíte přenosového agenta v síti, která hostí váš koncový bod hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-184">In order for hybrid connections to work you need a relay agent in the network that hosts your hybrid connection endpoint.</span></span>  <span data-ttu-id="0bc4b-185">Tento přenosový agent se nazývá hybridní připojení správce (HCM).</span><span class="sxs-lookup"><span data-stu-id="0bc4b-185">That relay agent is called the Hybrid Connection Manager (HCM).</span></span>  <span data-ttu-id="0bc4b-186">Tento nástroj můžete stáhnout z **sítě > nakonfigurovat koncové body hybridního připojení** dostupné z aplikace v rámci uživatelského rozhraní [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="0bc4b-186">This tool can be downloaded from the **Networking > Configure your hybrid connection endpoints** UI available from your app in the [Azure portal][portal].</span></span>  

<span data-ttu-id="0bc4b-187">Tento nástroj běží na systému Windows server 2008 R2 a novějších verzích systému Windows.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-187">This tool runs on Windows server 2008 R2 and later versions of Windows.</span></span>  <span data-ttu-id="0bc4b-188">Nainstalovaný HCM spuštění jako služba.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-188">Once installed the HCM runs as a service.</span></span>  <span data-ttu-id="0bc4b-189">Tato služba se připojí k předávání přes Azure sběrnice podle nakonfigurované koncové body.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-189">This service connects to Azure servicebus relay based on the configured endpoints.</span></span>  <span data-ttu-id="0bc4b-190">Připojení z HCM jsou odchozí na porty 80 a 443.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-190">The connections from the HCM are outbound to ports 80 and 443.</span></span>    

<span data-ttu-id="0bc4b-191">HCM má uživatelské rozhraní k jeho konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-191">The HCM has a UI to configure it.</span></span>  <span data-ttu-id="0bc4b-192">Po instalaci HCM můžete zprovoznit rozhraní spuštěním HybridConnectionManagerUi.exe která se nachází v instalačním adresáři správce hybridního připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-192">After the HCM is installed you can bring up the UI by running the HybridConnectionManagerUi.exe that sits in the Hybrid Connection Manager installation directory.</span></span>  <span data-ttu-id="0bc4b-193">Je také snadno dosáhla ve Windows 10 zadáním *uživatelského rozhraní správce hybridního připojení* vyhledávacího pole.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-193">It is also easily reached on Windows 10 by typing *Hybrid Connection Manager UI* in your search box.</span></span>  

<span data-ttu-id="0bc4b-194">Při spuštění uživatelského rozhraní HCM je první věcí, kterou vidíte tabulku, která obsahuje seznam všech hybridní připojení, které jsou nakonfigurovány k této instanci HCM.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-194">When the HCM UI is started, the first thing you see is a table that lists all of the hybrid connections that are configured with this instance of the HCM.</span></span>  <span data-ttu-id="0bc4b-195">Pokud chcete, aby všechny změny, které potřebujete k ověření pomocí Azure.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-195">If you wish to make any changes you will need to authenticate with Azure.</span></span> 

<span data-ttu-id="0bc4b-196">Přidat jeden nebo více hybridní připojení k vaší HCM:</span><span class="sxs-lookup"><span data-stu-id="0bc4b-196">To add one or more Hybrid Connections to your HCM:</span></span>

1. <span data-ttu-id="0bc4b-197">Spuštění uživatelského rozhraní HCM</span><span class="sxs-lookup"><span data-stu-id="0bc4b-197">Start the HCM UI</span></span>
1. <span data-ttu-id="0bc4b-198">Klikněte na tlačítko Konfigurovat další hybridní připojení![][8]</span><span class="sxs-lookup"><span data-stu-id="0bc4b-198">Click Configure another hybrid connection ![][8]</span></span>

1. <span data-ttu-id="0bc4b-199">Přihlaste se pomocí účtu Azure</span><span class="sxs-lookup"><span data-stu-id="0bc4b-199">Sign in with your Azure account</span></span>
1. <span data-ttu-id="0bc4b-200">Zvolit předplatné</span><span class="sxs-lookup"><span data-stu-id="0bc4b-200">Choose a subscription</span></span>
1. <span data-ttu-id="0bc4b-201">Klikněte na hybridní připojení, které chcete tuto HCM pro předávání![][9]</span><span class="sxs-lookup"><span data-stu-id="0bc4b-201">Click on the hybrid connections you want this HCM to relay ![][9]</span></span>

1. <span data-ttu-id="0bc4b-202">Kliknutí na Uložit</span><span class="sxs-lookup"><span data-stu-id="0bc4b-202">Click Save</span></span>

<span data-ttu-id="0bc4b-203">V tomto okamžiku se zobrazí hybridní připojení, které jste přidali.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-203">At this point you will see the hybrid connections you added.</span></span>  <span data-ttu-id="0bc4b-204">Můžete také kliknutím na nakonfigurované hybridní připojení a zobrazit podrobnosti o připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-204">You can also click on the configured hybrid connection and see details about the connection.</span></span>

![][10]

<span data-ttu-id="0bc4b-205">Pro vaše HCM být schopné podporovat hybridní připojení, který je nakonfigurován s musí:</span><span class="sxs-lookup"><span data-stu-id="0bc4b-205">For your HCM to be able to support the hybrid connections it is configured with, it needs:</span></span>

- <span data-ttu-id="0bc4b-206">TCP přístup k Azure přes porty 80 a 443</span><span class="sxs-lookup"><span data-stu-id="0bc4b-206">TCP access to Azure over ports 80 and 443</span></span>
- <span data-ttu-id="0bc4b-207">TCP přístup ke koncovému bodu hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-207">TCP access to the hybrid connection endpoint</span></span>
- <span data-ttu-id="0bc4b-208">Umožňuje provést DNS ups podívejte se na hostiteli koncový bod a obor názvů sběrnice azure</span><span class="sxs-lookup"><span data-stu-id="0bc4b-208">ability to do DNS look ups on the endpoint host and the azure servicebus namespace</span></span>

<span data-ttu-id="0bc4b-209">HCM podporuje nové hybridní připojení jak starší BizTalk hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-209">The HCM supports both new hybrid connections as well as the older BizTalk hybrid connections.</span></span>

### <a name="redundancy"></a><span data-ttu-id="0bc4b-210">Redundance</span><span class="sxs-lookup"><span data-stu-id="0bc4b-210">Redundancy</span></span> ###

<span data-ttu-id="0bc4b-211">Každý HCM může podporovat víc hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-211">Each HCM can support multiple hybrid connections.</span></span>  <span data-ttu-id="0bc4b-212">Jakékoli dané hybridní připojení může také podporovat více HCMs.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-212">Also, any given hybrid connection can be supported by multiple HCMs.</span></span>  <span data-ttu-id="0bc4b-213">Výchozí chování je kruhové dotazování provoz mezi nakonfigurované HCMs pro libovolný dané koncový bod.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-213">The default behavior is to round robin traffic across the configured HCMs for any given endpoint.</span></span>  <span data-ttu-id="0bc4b-214">Pokud chcete vysoké dostupnosti na hybridní připojení z vaší sítě, jednoduše vytvoří instanci více HCMs na samostatných počítačích.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-214">If you want high availability on your hybrid connections from your network, simply instantiate multiple HCMs on separate machines.</span></span> 

### <a name="manually-adding-a-hybrid-connection"></a><span data-ttu-id="0bc4b-215">Ručně přidejte hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-215">Manually adding a hybrid connection</span></span> ###

<span data-ttu-id="0bc4b-216">Pokud chcete někomu mimo vaše předplatné pro hostování HCM instance pro danou hybridní připojení, můžete sdílet s nimi připojovací řetězec brány pro hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-216">If you wish somebody outside of your subscription to host an HCM instance for a given hybrid connection, you can share with them the gateway connection string for the hybrid connection.</span></span>  <span data-ttu-id="0bc4b-217">Můžete to vidět ve vlastnostech pro hybridní připojení v [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="0bc4b-217">You can see this in the properties for a hybrid connection in the [Azure portal][portal].</span></span> <span data-ttu-id="0bc4b-218">Chcete-li použít tento řetězec, klikněte na tlačítko **nakonfigurovat ručně** tlačítka na HCM a vložte připojovací řetězec brány.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-218">To use that string, click the **Configure manually** button in the HCM and paste in the gateway connection string.</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="0bc4b-219">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="0bc4b-219">Troubleshooting</span></span> ##

<span data-ttu-id="0bc4b-220">Když hybridní připojení nastavená s běžící aplikací a je alespoň jeden HCM, která má toto hybridní připojení nakonfigurovaná a potom se dozvíte stav **připojeno** na portálu.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-220">When your hybrid connection is set up with a running application and there is at least one HCM that has that hybrid connection configured, then the status will say **Connected** in the portal.</span></span>  <span data-ttu-id="0bc4b-221">Pokud není vyslovení **připojeno** pak to znamená, že buď aplikace je mimo provoz nebo vaše HCM se nemůže připojit se k Azure na porty 80 nebo 443.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-221">If it does not say **Connected** then it means that either your app is down or your HCM cannot connect out to Azure on ports 80 or 443.</span></span>  

<span data-ttu-id="0bc4b-222">Hlavním důvodem, který klienti se nemůže připojit k jejich koncový bod je, protože koncový bod byl zadán pomocí IP adresy místo názvu DNS.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-222">The primary reason that clients cannot connect to their endpoint is because the endpoint was specified using an IP address instead of a DNS name.</span></span>  <span data-ttu-id="0bc4b-223">Pokud vaše aplikace nelze dosáhnout požadovaného koncového bodu a použít IP adresu, přejdete k používání název DNS, který je na hostiteli, kde je spuštěna HCM.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-223">If your app cannot reach the desired endpoint and you used an IP address, switch to using a DNS name that is valid on the host where the HCM is running.</span></span>  <span data-ttu-id="0bc4b-224">Další body ke kontrole jsou správně vyřeší názvu DNS na hostitele, na kterém je spuštěný HCM a zda existuje připojení mezi hostiteli se spuštěným HCM ke koncovému bodu hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-224">Other things to check are that the DNS name resolves properly on the host where the HCM is running and that there is connectivity from the host where the HCM is running to the hybrid connection endpoint.</span></span>  

<span data-ttu-id="0bc4b-225">V App Service, která se může vyvolat z konzoly názvem tcpping je nástroj.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-225">There is a tool in the App Service that can be invoked from the console called tcpping.</span></span>  <span data-ttu-id="0bc4b-226">Tento nástroj vám sdělí, pokud máte přístup k koncový bod TCP, ale neobsahuje žádné informace můžete Pokud máte přístup do koncového bodu hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-226">This tool can tell you if you have access to a TCP endpoint but does not tell you if you have access to a hybrid connection endpoint.</span></span>  <span data-ttu-id="0bc4b-227">Pokud se použije v konzole pro koncový bod hybridní připojení, úspěšný příkaz ping pouze zjistíte, že máte hybridní připojení nakonfigurovaná pro vaši aplikaci, která používá tuto kombinaci hostitele a portu.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-227">When used in the console against a hybrid connection endpoint, a successful ping will only tell you that you have a hybrid connection configured for your app that uses that host:port combination.</span></span>  

## <a name="biztalk-hybrid-connections"></a><span data-ttu-id="0bc4b-228">Hybridní připojení BizTalk</span><span class="sxs-lookup"><span data-stu-id="0bc4b-228">BizTalk Hybrid Connections</span></span> ##

<span data-ttu-id="0bc4b-229">Starší schopností BizTalk hybridní připojení bylo ukončeno vypnout k vytváření další BizTalk hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-229">The older BizTalk Hybrid Connections capability has been closed off to further BizTalk Hybrid Connection creations.</span></span>  <span data-ttu-id="0bc4b-230">Můžete pokračovat pomocí dříve existující připojení BizTalk hybridní aplikace, ale by bylo nutné migrovat na novou službu.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-230">You can continue using your preexisting BizTalk Hybrid Connections with your apps but should migrate to the new service.</span></span>  <span data-ttu-id="0bc4b-231">Mezi výhody v rámci nové služby BizTalk verze jsou:</span><span class="sxs-lookup"><span data-stu-id="0bc4b-231">Among the benefits in the new service over the BizTalk version are:</span></span>

- <span data-ttu-id="0bc4b-232">není třeba žádné další účet BizTalk</span><span class="sxs-lookup"><span data-stu-id="0bc4b-232">no additional BizTalk account is required</span></span>
- <span data-ttu-id="0bc4b-233">Je protokol TLS 1.2 místo 1.0 jako BizTalk hybridní připojení</span><span class="sxs-lookup"><span data-stu-id="0bc4b-233">TLS is 1.2 instead of 1.0 as in BizTalk Hybrid Connections</span></span>
- <span data-ttu-id="0bc4b-234">Komunikace je přes porty 80 a 443 pomocí názvu DNS k dosažení Azure místo IP adresy a řadu dalších jiné porty.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-234">Communication is over ports 80 and 443 using a DNS name to reach Azure instead of IP addresses and a range of additional other ports.</span></span>  

<span data-ttu-id="0bc4b-235">Přidat hybridní připojení BizTalk do vaší aplikace, přejděte na aplikace v rámci [portál Azure] [ portal] a klikněte na tlačítko **sítě > nakonfigurovat koncové body hybridního připojení**.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-235">To add a BizTalk hybrid connection to your app, go to your app in the [Azure portal][portal] and click **Networking > Configure your hybrid connection endpoints**.</span></span>  <span data-ttu-id="0bc4b-236">V tabulce Classic hybridní připojení klikněte na tlačítko **přidat classic hybridní připojení**.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-236">In the Classic hybrid connections table click **Add classic hybrid connection**.</span></span>  <span data-ttu-id="0bc4b-237">Zde uvidíte seznam BizTalk hybridní připojení.</span><span class="sxs-lookup"><span data-stu-id="0bc4b-237">From here you will see a list of your BizTalk hybrid connections.</span></span>  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

