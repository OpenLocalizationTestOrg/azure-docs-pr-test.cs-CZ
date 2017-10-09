---
title: "nastavení Azure Traffic Manager aaaVerify | Microsoft Docs"
description: "Tento článek vám pomůže ověřte nastavení Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 2180b640-596e-4fb2-be59-23a38d606d12
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: c670be6cf55e140c7ab63d5d526de08e14774d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="verify-traffic-manager-settings"></a><span data-ttu-id="98ce3-103">Ověřte nastavení Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="98ce3-103">Verify Traffic Manager settings</span></span>

<span data-ttu-id="98ce3-104">tootest nastavení Traffic Manager, je nutné toohave více klientů v různých umístěních, ze kterých můžete spustit testy.</span><span class="sxs-lookup"><span data-stu-id="98ce3-104">tootest your Traffic Manager settings, you need toohave multiple clients, in various locations, from which you can run your tests.</span></span> <span data-ttu-id="98ce3-105">Přepněte hello koncových bodů ve vašem profilu Traffic Manageru dolů po jednom.</span><span class="sxs-lookup"><span data-stu-id="98ce3-105">Then, bring hello endpoints in your Traffic Manager profile down one at a time.</span></span>

* <span data-ttu-id="98ce3-106">Nastavte hello hodnota DNS TTL nízkou tak, aby změny rozšíří rychle (například 30 sekund).</span><span class="sxs-lookup"><span data-stu-id="98ce3-106">Set hello DNS TTL value low so that changes propagate quickly (for example, 30 seconds).</span></span>
* <span data-ttu-id="98ce3-107">Znáte IP adresy služby Azure cloud services a weby v hello profilu, který testujete hello.</span><span class="sxs-lookup"><span data-stu-id="98ce3-107">Know hello IP addresses of your Azure cloud services and websites in hello profile you are testing.</span></span>
* <span data-ttu-id="98ce3-108">Pomocí nástrojů, které umožňují přeložit název DNS tooan IP adresu a zobrazit tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="98ce3-108">Use tools that let you resolve a DNS name tooan IP address and display that address.</span></span>

<span data-ttu-id="98ce3-109">Při kontrole toosee, názvy DNS hello překládání adres tooIP hello koncových bodů ve vašem profilu.</span><span class="sxs-lookup"><span data-stu-id="98ce3-109">You are checking toosee that hello DNS names resolve tooIP addresses of hello endpoints in your profile.</span></span> <span data-ttu-id="98ce3-110">musí se překládat názvy Hello v souladu s metodu směrování hello provozu definované v hello profil služby Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="98ce3-110">hello names should resolve in a manner consistent with hello traffic routing method defined in hello Traffic Manager profile.</span></span> <span data-ttu-id="98ce3-111">Můžete použít nástroje hello jako **nslookup** nebo **prozkoumat** tooresolve názvy DNS.</span><span class="sxs-lookup"><span data-stu-id="98ce3-111">You can use hello tools like **nslookup** or **dig** tooresolve DNS names.</span></span>

<span data-ttu-id="98ce3-112">Hello následující příklady můžete otestovat váš profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="98ce3-112">hello following examples help you test your Traffic Manager profile.</span></span>

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a><span data-ttu-id="98ce3-113">Zkontrolujte profil služby Traffic Manager pomocí nástroje nslookup a ipconfig v systému Windows</span><span class="sxs-lookup"><span data-stu-id="98ce3-113">Check Traffic Manager profile using nslookup and ipconfig in Windows</span></span>

1. <span data-ttu-id="98ce3-114">Otevřete příkaz či řádku prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="98ce3-114">Open a command or Windows PowerShell prompt as an administrator.</span></span>
2. <span data-ttu-id="98ce3-115">Typ `ipconfig /flushdns` tooflush hello mezipaměť Překladač DNS.</span><span class="sxs-lookup"><span data-stu-id="98ce3-115">Type `ipconfig /flushdns` tooflush hello DNS resolver cache.</span></span>
3. <span data-ttu-id="98ce3-116">Zadejte `nslookup <your Traffic Manager domain name>`.</span><span class="sxs-lookup"><span data-stu-id="98ce3-116">Type `nslookup <your Traffic Manager domain name>`.</span></span> <span data-ttu-id="98ce3-117">Například hello následující příkaz kontroly hello název domény s předponou hello *myapp.contoso*</span><span class="sxs-lookup"><span data-stu-id="98ce3-117">For example, hello following command checks hello domain name with hello prefix *myapp.contoso*</span></span>

        nslookup myapp.contoso.trafficmanager.net

    <span data-ttu-id="98ce3-118">Obvykle za následek ukazuje hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="98ce3-118">A typical result shows hello following information:</span></span>

    + <span data-ttu-id="98ce3-119">Hello název DNS a IP adresu se server DNS hello přístup tooresolve tento název domény Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="98ce3-119">hello DNS name and IP address of hello DNS server being accessed tooresolve this Traffic Manager domain name.</span></span>
    + <span data-ttu-id="98ce3-120">název domény Traffic Manageru Hello jste zadali na příkazovém řádku hello po nástroje "nslookup" a přeloží hello IP adresu toowhich hello doménu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="98ce3-120">hello Traffic Manager domain name you typed on hello command line after "nslookup" and hello IP address toowhich hello Traffic Manager domain resolves.</span></span> <span data-ttu-id="98ce3-121">Hello druhou IP adresu je důležité jeden toocheck hello.</span><span class="sxs-lookup"><span data-stu-id="98ce3-121">hello second IP address is hello important one toocheck.</span></span> <span data-ttu-id="98ce3-122">Měla by se shodovat veřejné virtuální adresy IP (VIP) pro jeden z hello cloudové služby nebo webů v hello profil služby Traffic Manager, které testujete.</span><span class="sxs-lookup"><span data-stu-id="98ce3-122">It should match a public virtual IP (VIP) address for one of hello cloud services or websites in hello Traffic Manager profile you are testing.</span></span>

## <a name="how-tootest-hello-failover-traffic-routing-method"></a><span data-ttu-id="98ce3-123">Jak metoda směrování provozu tootest hello převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="98ce3-123">How tootest hello failover traffic routing method</span></span>

1. <span data-ttu-id="98ce3-124">Ponechejte si všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="98ce3-124">Leave all endpoints up.</span></span>
2. <span data-ttu-id="98ce3-125">Pomocí jednoho klienta, požadavky na překlad DNS pro název domény vaší společnosti, pomocí nástroje nslookup nebo podobného nástroje.</span><span class="sxs-lookup"><span data-stu-id="98ce3-125">Using a single client, request DNS resolution for your company domain name using nslookup or a similar utility.</span></span>
3. <span data-ttu-id="98ce3-126">Zkontrolujte, zda že tento hello řešena IP adresa odpovídá hello primární koncový bod.</span><span class="sxs-lookup"><span data-stu-id="98ce3-126">Ensure that hello resolved IP address matches hello primary endpoint.</span></span>
4. <span data-ttu-id="98ce3-127">Přeneste dolů váš primární koncový bod nebo odeberte hello monitorování souboru tak, aby správce provozu se domnívá, které aplikace hello je vypnutý.</span><span class="sxs-lookup"><span data-stu-id="98ce3-127">Bring down your primary endpoint or remove hello monitoring file so that Traffic Manager thinks that hello application is down.</span></span>
5. <span data-ttu-id="98ce3-128">Počkejte hello DNS Time-to-Live (TTL) profil služby Traffic Manager hello plus dalších dvou minut.</span><span class="sxs-lookup"><span data-stu-id="98ce3-128">Wait for hello DNS Time-to-Live (TTL) of hello Traffic Manager profile plus an additional two minutes.</span></span> <span data-ttu-id="98ce3-129">Například pokud vaše TTL DNS je 300 sekund (5 minut), je nutné počkat sedm minut.</span><span class="sxs-lookup"><span data-stu-id="98ce3-129">For example, if your DNS TTL is 300 seconds (5 minutes), you must wait for seven minutes.</span></span>
6. <span data-ttu-id="98ce3-130">Vyprázdnění vaší DNS klienta mezipaměti a žádosti o překlad DNS pomocí nástroje nslookup.</span><span class="sxs-lookup"><span data-stu-id="98ce3-130">Flush your DNS client cache and request DNS resolution using nslookup.</span></span> <span data-ttu-id="98ce3-131">V systému Windows můžete vyprázdnit mezipaměť DNS pomocí hello ipconfig/flushdns příkazu.</span><span class="sxs-lookup"><span data-stu-id="98ce3-131">In Windows, you can flush your DNS cache with hello ipconfig /flushdns command.</span></span>
7. <span data-ttu-id="98ce3-132">Zkontrolujte, zda že tento hello řešena IP adresa odpovídá sekundární koncový bod.</span><span class="sxs-lookup"><span data-stu-id="98ce3-132">Ensure that hello resolved IP address matches your secondary endpoint.</span></span>
8. <span data-ttu-id="98ce3-133">Opakujte postup hello, zase ukončování každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="98ce3-133">Repeat hello process, bringing down each endpoint in turn.</span></span> <span data-ttu-id="98ce3-134">Ověřte, že hello DNS vrátí hello IP adresa hello další koncového bodu v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="98ce3-134">Verify that hello DNS returns hello IP address of hello next endpoint in hello list.</span></span> <span data-ttu-id="98ce3-135">Když jsou všechny koncové body dolů, by měl znovu získat IP adresu hello hello primární koncový bod.</span><span class="sxs-lookup"><span data-stu-id="98ce3-135">When all endpoints are down, you should obtain hello IP address of hello primary endpoint again.</span></span>

## <a name="how-tootest-hello-weighted-traffic-routing-method"></a><span data-ttu-id="98ce3-136">Jak tootest hello vážené metodu směrování provozu</span><span class="sxs-lookup"><span data-stu-id="98ce3-136">How tootest hello weighted traffic routing method</span></span>

1. <span data-ttu-id="98ce3-137">Ponechejte si všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="98ce3-137">Leave all endpoints up.</span></span>
2. <span data-ttu-id="98ce3-138">Pomocí jednoho klienta, požadavky na překlad DNS pro název domény vaší společnosti, pomocí nástroje nslookup nebo podobného nástroje.</span><span class="sxs-lookup"><span data-stu-id="98ce3-138">Using a single client, request DNS resolution for your company domain name using nslookup or a similar utility.</span></span>
3. <span data-ttu-id="98ce3-139">Zkontrolujte, zda že tento hello řešena IP adresa odpovídá jednomu z koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="98ce3-139">Ensure that hello resolved IP address matches one of your endpoints.</span></span>
4. <span data-ttu-id="98ce3-140">Vyprázdnění mezipaměti klienta DNS a zopakujte kroky 2 a 3 pro každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="98ce3-140">Flush your DNS client cache and repeat steps 2 and 3 for each endpoint.</span></span> <span data-ttu-id="98ce3-141">Měli byste vidět různé IP adresy vrátí pro každou z koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="98ce3-141">You should see different IP addresses returned for each of your endpoints.</span></span>

## <a name="how-tootest-hello-performance-traffic-routing-method"></a><span data-ttu-id="98ce3-142">Jak tootest hello výkonu metoda směrování provozu</span><span class="sxs-lookup"><span data-stu-id="98ce3-142">How tootest hello performance traffic routing method</span></span>

<span data-ttu-id="98ce3-143">tooeffectively test metodu směrování provozu výkonu, musí mít klienti nacházející se v různých částech hello, world.</span><span class="sxs-lookup"><span data-stu-id="98ce3-143">tooeffectively test a performance traffic routing method, you must have clients located in different parts of hello world.</span></span> <span data-ttu-id="98ce3-144">Klienty můžete vytvořit v různých oblastech Azure, které se dají použít tootest vašim službám.</span><span class="sxs-lookup"><span data-stu-id="98ce3-144">You can create clients in different Azure regions that can be used tootest your services.</span></span> <span data-ttu-id="98ce3-145">Pokud máte globální sítě, můžete vzdáleně přihlaste tooclients v dalších částech tohoto hello, world a spouštění testů z ní.</span><span class="sxs-lookup"><span data-stu-id="98ce3-145">If you have a global network, you can remotely sign in tooclients in other parts of hello world and run your tests from there.</span></span>

<span data-ttu-id="98ce3-146">Alternativně existují uvolněte webové vyhledávání DNS a víc služeb, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="98ce3-146">Alternatively, there are free web-based DNS lookup and dig services available.</span></span> <span data-ttu-id="98ce3-147">Některé z těchto nástrojů pro udělení hello překlad názvu DNS možnost toocheck z různých míst kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="98ce3-147">Some of these tools give you hello ability toocheck DNS name resolution from various locations around hello world.</span></span> <span data-ttu-id="98ce3-148">Proveďte hledání na "Vyhledávání DNS" příklady.</span><span class="sxs-lookup"><span data-stu-id="98ce3-148">Do a search on "DNS lookup" for examples.</span></span> <span data-ttu-id="98ce3-149">Služby třetích stran, jako je Gomez nebo //Build lze použít tooconfirm profilech distribuovanému přenosy podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="98ce3-149">Third-party services like Gomez or Keynote can be used tooconfirm that your profiles are distributing traffic as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98ce3-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98ce3-150">Next steps</span></span>

* [<span data-ttu-id="98ce3-151">O metodách směrování provozu Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="98ce3-151">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="98ce3-152">Důležité informace o výkonu nástroje Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="98ce3-152">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
* [<span data-ttu-id="98ce3-153">Řešení potíží při sníženém výkonu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="98ce3-153">Troubleshooting Traffic Manager degraded state</span></span>](traffic-manager-troubleshooting-degraded.md)
