---
title: "Ověřte nastavení Azure Traffic Manageru | Microsoft Docs"
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
ms.openlocfilehash: aadff1806a7cb22347283143563467366e857569
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="verify-traffic-manager-settings"></a><span data-ttu-id="489c5-103">Ověřte nastavení Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="489c5-103">Verify Traffic Manager settings</span></span>

<span data-ttu-id="489c5-104">Můžete zkontrolovat nastavení Traffic Manager, musíte mít více klientů v různých umístěních, ze kterých můžete spustit testy.</span><span class="sxs-lookup"><span data-stu-id="489c5-104">To test your Traffic Manager settings, you need to have multiple clients, in various locations, from which you can run your tests.</span></span> <span data-ttu-id="489c5-105">Potom přepněte koncových bodů ve vašem profilu Traffic Manageru dolů po jednom.</span><span class="sxs-lookup"><span data-stu-id="489c5-105">Then, bring the endpoints in your Traffic Manager profile down one at a time.</span></span>

* <span data-ttu-id="489c5-106">Nastavte hodnotu DNS TTL nízkou tak, aby změny rozšíří rychle (například 30 sekund).</span><span class="sxs-lookup"><span data-stu-id="489c5-106">Set the DNS TTL value low so that changes propagate quickly (for example, 30 seconds).</span></span>
* <span data-ttu-id="489c5-107">Znáte IP adresy služby Azure cloud services a weby v profilu, který testujete.</span><span class="sxs-lookup"><span data-stu-id="489c5-107">Know the IP addresses of your Azure cloud services and websites in the profile you are testing.</span></span>
* <span data-ttu-id="489c5-108">Pomocí nástrojů, které umožňují přeložit název DNS na IP adresu a zobrazit tuto adresu.</span><span class="sxs-lookup"><span data-stu-id="489c5-108">Use tools that let you resolve a DNS name to an IP address and display that address.</span></span>

<span data-ttu-id="489c5-109">Probíhá kontrola, že přeložit názvy DNS na IP adresy koncových bodů ve vašem profilu.</span><span class="sxs-lookup"><span data-stu-id="489c5-109">You are checking to see that the DNS names resolve to IP addresses of the endpoints in your profile.</span></span> <span data-ttu-id="489c5-110">Musí se překládat názvy v souladu s metodu směrování provozu, který je definován v profilu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="489c5-110">The names should resolve in a manner consistent with the traffic routing method defined in the Traffic Manager profile.</span></span> <span data-ttu-id="489c5-111">Můžete použít nástroje jako **nslookup** nebo **prozkoumat** k překladu názvů DNS.</span><span class="sxs-lookup"><span data-stu-id="489c5-111">You can use the tools like **nslookup** or **dig** to resolve DNS names.</span></span>

<span data-ttu-id="489c5-112">Následující příklady můžete otestovat váš profil Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="489c5-112">The following examples help you test your Traffic Manager profile.</span></span>

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a><span data-ttu-id="489c5-113">Zkontrolujte profil služby Traffic Manager pomocí nástroje nslookup a ipconfig v systému Windows</span><span class="sxs-lookup"><span data-stu-id="489c5-113">Check Traffic Manager profile using nslookup and ipconfig in Windows</span></span>

1. <span data-ttu-id="489c5-114">Otevřete příkaz či řádku prostředí Windows PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="489c5-114">Open a command or Windows PowerShell prompt as an administrator.</span></span>
2. <span data-ttu-id="489c5-115">Typ `ipconfig /flushdns` k vyprázdnit mezipaměť DNS překladač.</span><span class="sxs-lookup"><span data-stu-id="489c5-115">Type `ipconfig /flushdns` to flush the DNS resolver cache.</span></span>
3. <span data-ttu-id="489c5-116">Zadejte `nslookup <your Traffic Manager domain name>`.</span><span class="sxs-lookup"><span data-stu-id="489c5-116">Type `nslookup <your Traffic Manager domain name>`.</span></span> <span data-ttu-id="489c5-117">Například následující příkaz ověří název domény s předponou *myapp.contoso*</span><span class="sxs-lookup"><span data-stu-id="489c5-117">For example, the following command checks the domain name with the prefix *myapp.contoso*</span></span>

        nslookup myapp.contoso.trafficmanager.net

    <span data-ttu-id="489c5-118">Obvykle za následek zobrazí následující informace:</span><span class="sxs-lookup"><span data-stu-id="489c5-118">A typical result shows the following information:</span></span>

    + <span data-ttu-id="489c5-119">Název DNS a IP adresa serveru DNS, přistupuje na tento název domény Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="489c5-119">The DNS name and IP address of the DNS server being accessed to resolve this Traffic Manager domain name.</span></span>
    + <span data-ttu-id="489c5-120">Název domény Traffic Manageru, jste zadali na příkazovém řádku po "nástroje nslookup" a IP adresu, na kterou se přeloží doménu Traffic Manageru.</span><span class="sxs-lookup"><span data-stu-id="489c5-120">The Traffic Manager domain name you typed on the command line after "nslookup" and the IP address to which the Traffic Manager domain resolves.</span></span> <span data-ttu-id="489c5-121">Druhou IP adresu je důležité zkontrolovat.</span><span class="sxs-lookup"><span data-stu-id="489c5-121">The second IP address is the important one to check.</span></span> <span data-ttu-id="489c5-122">Měla by se shodovat veřejné virtuální adresy IP (VIP) pro některý z cloudové služby nebo webů v profil služby Traffic Manager, který testujete.</span><span class="sxs-lookup"><span data-stu-id="489c5-122">It should match a public virtual IP (VIP) address for one of the cloud services or websites in the Traffic Manager profile you are testing.</span></span>

## <a name="how-to-test-the-failover-traffic-routing-method"></a><span data-ttu-id="489c5-123">Postup testování metodu směrování provozu převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="489c5-123">How to test the failover traffic routing method</span></span>

1. <span data-ttu-id="489c5-124">Ponechejte si všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="489c5-124">Leave all endpoints up.</span></span>
2. <span data-ttu-id="489c5-125">Pomocí jednoho klienta, požadavky na překlad DNS pro název domény vaší společnosti, pomocí nástroje nslookup nebo podobného nástroje.</span><span class="sxs-lookup"><span data-stu-id="489c5-125">Using a single client, request DNS resolution for your company domain name using nslookup or a similar utility.</span></span>
3. <span data-ttu-id="489c5-126">Zajistěte, aby odpovídal přeložit IP adresu primární koncový bod.</span><span class="sxs-lookup"><span data-stu-id="489c5-126">Ensure that the resolved IP address matches the primary endpoint.</span></span>
4. <span data-ttu-id="489c5-127">Přeneste dolů váš primární koncový bod nebo odeberte soubor monitorování, aby se tento Traffic Manager se domnívá, že aplikace je mimo provoz.</span><span class="sxs-lookup"><span data-stu-id="489c5-127">Bring down your primary endpoint or remove the monitoring file so that Traffic Manager thinks that the application is down.</span></span>
5. <span data-ttu-id="489c5-128">Počkejte DNS Time-to-Live (TTL) profil služby Traffic Manager plus dalších dvou minut.</span><span class="sxs-lookup"><span data-stu-id="489c5-128">Wait for the DNS Time-to-Live (TTL) of the Traffic Manager profile plus an additional two minutes.</span></span> <span data-ttu-id="489c5-129">Například pokud vaše TTL DNS je 300 sekund (5 minut), je nutné počkat sedm minut.</span><span class="sxs-lookup"><span data-stu-id="489c5-129">For example, if your DNS TTL is 300 seconds (5 minutes), you must wait for seven minutes.</span></span>
6. <span data-ttu-id="489c5-130">Vyprázdnění vaší DNS klienta mezipaměti a žádosti o překlad DNS pomocí nástroje nslookup.</span><span class="sxs-lookup"><span data-stu-id="489c5-130">Flush your DNS client cache and request DNS resolution using nslookup.</span></span> <span data-ttu-id="489c5-131">V systému Windows můžete vyprázdnit mezipaměť DNS pomocí příkazu ipconfig/flushdns.</span><span class="sxs-lookup"><span data-stu-id="489c5-131">In Windows, you can flush your DNS cache with the ipconfig /flushdns command.</span></span>
7. <span data-ttu-id="489c5-132">Zajistěte, aby odpovídal přeložit IP adresu sekundární koncový bod.</span><span class="sxs-lookup"><span data-stu-id="489c5-132">Ensure that the resolved IP address matches your secondary endpoint.</span></span>
8. <span data-ttu-id="489c5-133">Opakujte tento postup, zase ukončování každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="489c5-133">Repeat the process, bringing down each endpoint in turn.</span></span> <span data-ttu-id="489c5-134">Ověřte, že DNS vrátí IP adresu další koncového bodu v seznamu.</span><span class="sxs-lookup"><span data-stu-id="489c5-134">Verify that the DNS returns the IP address of the next endpoint in the list.</span></span> <span data-ttu-id="489c5-135">Když jsou všechny koncové body dolů, musí znovu získat IP adresu primární koncový bod.</span><span class="sxs-lookup"><span data-stu-id="489c5-135">When all endpoints are down, you should obtain the IP address of the primary endpoint again.</span></span>

## <a name="how-to-test-the-weighted-traffic-routing-method"></a><span data-ttu-id="489c5-136">Postup testování metodu směrování provozu vyvážené</span><span class="sxs-lookup"><span data-stu-id="489c5-136">How to test the weighted traffic routing method</span></span>

1. <span data-ttu-id="489c5-137">Ponechejte si všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="489c5-137">Leave all endpoints up.</span></span>
2. <span data-ttu-id="489c5-138">Pomocí jednoho klienta, požadavky na překlad DNS pro název domény vaší společnosti, pomocí nástroje nslookup nebo podobného nástroje.</span><span class="sxs-lookup"><span data-stu-id="489c5-138">Using a single client, request DNS resolution for your company domain name using nslookup or a similar utility.</span></span>
3. <span data-ttu-id="489c5-139">Zkontrolujte, zda přeložit IP adresa odpovídá jeden z koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="489c5-139">Ensure that the resolved IP address matches one of your endpoints.</span></span>
4. <span data-ttu-id="489c5-140">Vyprázdnění mezipaměti klienta DNS a zopakujte kroky 2 a 3 pro každý koncový bod.</span><span class="sxs-lookup"><span data-stu-id="489c5-140">Flush your DNS client cache and repeat steps 2 and 3 for each endpoint.</span></span> <span data-ttu-id="489c5-141">Měli byste vidět různé IP adresy vrátí pro každou z koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="489c5-141">You should see different IP addresses returned for each of your endpoints.</span></span>

## <a name="how-to-test-the-performance-traffic-routing-method"></a><span data-ttu-id="489c5-142">Postup testování metodu směrování provozu výkonu</span><span class="sxs-lookup"><span data-stu-id="489c5-142">How to test the performance traffic routing method</span></span>

<span data-ttu-id="489c5-143">K testování efektivně metodu směrování provozu výkonu, musí mít klienti nacházející se v různých částech světa.</span><span class="sxs-lookup"><span data-stu-id="489c5-143">To effectively test a performance traffic routing method, you must have clients located in different parts of the world.</span></span> <span data-ttu-id="489c5-144">Klienty můžete vytvořit v různých oblastech Azure, které lze použít k testování vašich služeb.</span><span class="sxs-lookup"><span data-stu-id="489c5-144">You can create clients in different Azure regions that can be used to test your services.</span></span> <span data-ttu-id="489c5-145">Pokud máte globální sítě, můžete vzdáleně Přihlaste se na klienty v dalších částech světa a spouštění testů z ní.</span><span class="sxs-lookup"><span data-stu-id="489c5-145">If you have a global network, you can remotely sign in to clients in other parts of the world and run your tests from there.</span></span>

<span data-ttu-id="489c5-146">Alternativně existují uvolněte webové vyhledávání DNS a víc služeb, které jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="489c5-146">Alternatively, there are free web-based DNS lookup and dig services available.</span></span> <span data-ttu-id="489c5-147">Některé z těchto nástrojů získáte možnost zkontrolujte překlad názvu DNS z různých míst po celém světě.</span><span class="sxs-lookup"><span data-stu-id="489c5-147">Some of these tools give you the ability to check DNS name resolution from various locations around the world.</span></span> <span data-ttu-id="489c5-148">Proveďte hledání na "Vyhledávání DNS" příklady.</span><span class="sxs-lookup"><span data-stu-id="489c5-148">Do a search on "DNS lookup" for examples.</span></span> <span data-ttu-id="489c5-149">Služby třetích stran, jako je Gomez nebo //Build slouží k potvrzení, že jsou vaše profily distribuci provoz podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="489c5-149">Third-party services like Gomez or Keynote can be used to confirm that your profiles are distributing traffic as expected.</span></span>

## <a name="next-steps"></a><span data-ttu-id="489c5-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="489c5-150">Next steps</span></span>

* [<span data-ttu-id="489c5-151">O metodách směrování provozu Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="489c5-151">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)
* [<span data-ttu-id="489c5-152">Důležité informace o výkonu nástroje Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="489c5-152">Traffic Manager performance considerations</span></span>](traffic-manager-performance-considerations.md)
* [<span data-ttu-id="489c5-153">Řešení potíží při sníženém výkonu Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="489c5-153">Troubleshooting Traffic Manager degraded state</span></span>](traffic-manager-troubleshooting-degraded.md)
