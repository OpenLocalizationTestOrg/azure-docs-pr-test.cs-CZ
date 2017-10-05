---
title: "Řešení potíží s degradovaný stav na Azure Traffic Manager"
description: "Postup řešení potíží s profily Traffic Manager, když se zobrazí jako degradovaný stav."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
ms.assetid: 8af0433d-e61b-4761-adcc-7bc9b8142fc6
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: kumud
ms.openlocfilehash: b1d00fb84695d2289f37647f55a7c56cf28c8c96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a><span data-ttu-id="fdac9-103">Řešení potíží s degradovaný stav na Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="fdac9-103">Troubleshooting degraded state on Azure Traffic Manager</span></span>

<span data-ttu-id="fdac9-104">Tento článek popisuje postup řešení potíží s profilem Azure Traffic Manager, který se zobrazuje degradovaném stavu.</span><span class="sxs-lookup"><span data-stu-id="fdac9-104">This article describes how to troubleshoot an Azure Traffic Manager profile that is showing a degraded status.</span></span> <span data-ttu-id="fdac9-105">Pro tento scénář zvažte, které jste nakonfigurovali profil Traffic Manageru odkazující na některé z vašich cloudapp.net hostovaných služeb.</span><span class="sxs-lookup"><span data-stu-id="fdac9-105">For this scenario, consider that you have configured a Traffic Manager profile pointing to some of your cloudapp.net hosted services.</span></span> <span data-ttu-id="fdac9-106">Pokud se zobrazí stav z vaší Traffic Manager **snížený** stav a stav jednoho nebo víc koncových bodů může být **snížený**:</span><span class="sxs-lookup"><span data-stu-id="fdac9-106">If the health of your Traffic Manager displays a **Degraded** status, then the status of one or more endpoints may be **Degraded**:</span></span>

![Stav sníženou koncových bodů](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

<span data-ttu-id="fdac9-108">Pokud se zobrazí stav z vaší Traffic Manager **neaktivní** stavu, pak oba koncové body mohou být **zakázané**:</span><span class="sxs-lookup"><span data-stu-id="fdac9-108">If the health of your Traffic Manager displays an **Inactive** status, then both end points may be **Disabled**:</span></span>

![Neaktivní stav Traffic Manager](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a><span data-ttu-id="fdac9-110">Sondy Principy Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="fdac9-110">Understanding Traffic Manager probes</span></span>

* <span data-ttu-id="fdac9-111">Správce provozu zvažuje koncového bodu se ONLINE jenom v případě, že tato kontrola obdrží odpověď HTTP 200 zpět z cesty k testu.</span><span class="sxs-lookup"><span data-stu-id="fdac9-111">Traffic Manager considers an endpoint to be ONLINE only when the probe receives an HTTP 200 response back from the probe path.</span></span> <span data-ttu-id="fdac9-112">Druhá odpověď bez 200 je selhání.</span><span class="sxs-lookup"><span data-stu-id="fdac9-112">Any other non-200 response is a failure.</span></span>
* <span data-ttu-id="fdac9-113">Přesměrování 30 x selže, i když přesměrované adresy URL vrátí 200.</span><span class="sxs-lookup"><span data-stu-id="fdac9-113">A 30x redirect fails, even if the redirected URL returns a 200.</span></span>
* <span data-ttu-id="fdac9-114">Pro sondy HTTPs jsou chyby certifikátu ignorovat.</span><span class="sxs-lookup"><span data-stu-id="fdac9-114">For HTTPs probes, certificate errors are ignored.</span></span>
* <span data-ttu-id="fdac9-115">Skutečný obsah cesty testu není důležité, tak dlouho, dokud se vrátí 200.</span><span class="sxs-lookup"><span data-stu-id="fdac9-115">The actual content of the probe path doesn't matter, as long as a 200 is returned.</span></span> <span data-ttu-id="fdac9-116">Zkušební fáze adresu URL také statický obsah, jako je "/ favicon.ico" je běžné techniky.</span><span class="sxs-lookup"><span data-stu-id="fdac9-116">Probing a URL to some static content like "/favicon.ico" is a common technique.</span></span> <span data-ttu-id="fdac9-117">Dynamický obsah, jako jsou stránky ASP nemusí vždy vrátí 200, i v případě, že aplikace je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="fdac9-117">Dynamic content, like the ASP pages, may not always return 200, even when the application is healthy.</span></span>
* <span data-ttu-id="fdac9-118">Osvědčeným postupem je možné nastavit cestu test na něco, co má dostatek logika pro určení, že lokalita je nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="fdac9-118">A best practice is to set the Probe path to something that has enough logic to determine that the site is up or down.</span></span> <span data-ttu-id="fdac9-119">V předchozím příkladu, nastavením cesta "/ favicon.ico", jsou jenom testování této w3wp.exe reaguje.</span><span class="sxs-lookup"><span data-stu-id="fdac9-119">In the previous example, by setting the path to "/favicon.ico", you are only testing that w3wp.exe is responding.</span></span> <span data-ttu-id="fdac9-120">Tento test nemusí znamenat, že webová aplikace v pořádku.</span><span class="sxs-lookup"><span data-stu-id="fdac9-120">This probe may not indicate that your web application is healthy.</span></span> <span data-ttu-id="fdac9-121">Lepší volbou bude nastavení cesty něco jako například "/ Probe.aspx" s cílem určit stav lokality.</span><span class="sxs-lookup"><span data-stu-id="fdac9-121">A better option would be to set a path to a something such as "/Probe.aspx" that has logic to determine the health of the site.</span></span> <span data-ttu-id="fdac9-122">Například můžete použít čítače výkonu pro využití procesoru nebo určovat počet neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="fdac9-122">For example, you could use performance counters to CPU utilization or measure the number of failed requests.</span></span> <span data-ttu-id="fdac9-123">Nebo může pokusit o přístup k prostředkům databáze nebo stav relace, abyste měli jistotu, že webová aplikace funguje.</span><span class="sxs-lookup"><span data-stu-id="fdac9-123">Or you could attempt to access database resources or session state to make sure that the web application is working.</span></span>
* <span data-ttu-id="fdac9-124">Pokud jsou degradovány všechny koncové body v profilu, pak Traffic Manager zpracovává všechny koncové body v pořádku a směrování provozu pro všechny koncové body.</span><span class="sxs-lookup"><span data-stu-id="fdac9-124">If all endpoints in a profile are degraded, then Traffic Manager treats all endpoints as healthy and routes traffic to all endpoints.</span></span> <span data-ttu-id="fdac9-125">To zaručuje, že problémy s zjišťovací mechanismus nevedou k dokončení výpadku služby.</span><span class="sxs-lookup"><span data-stu-id="fdac9-125">This behavior ensures that problems with the probing mechanism do not result in a complete outage of your service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="fdac9-126">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="fdac9-126">Troubleshooting</span></span>

<span data-ttu-id="fdac9-127">Chcete-li vyřešit selhání testu, musíte nástroj, který ukazuje návratový stavový kód protokolu HTTP z test adresy URL.</span><span class="sxs-lookup"><span data-stu-id="fdac9-127">To troubleshoot a probe failure, you need a tool that shows the HTTP status code return from the probe URL.</span></span> <span data-ttu-id="fdac9-128">Nejsou k dispozici celou řadu nástrojů, které ukazují, nezpracovaná odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="fdac9-128">There are many tools available that show you the raw HTTP response.</span></span>

* [<span data-ttu-id="fdac9-129">Fiddler</span><span class="sxs-lookup"><span data-stu-id="fdac9-129">Fiddler</span></span>](http://www.telerik.com/fiddler)
* [<span data-ttu-id="fdac9-130">curl</span><span class="sxs-lookup"><span data-stu-id="fdac9-130">curl</span></span>](https://curl.haxx.se/)
* [<span data-ttu-id="fdac9-131">wget</span><span class="sxs-lookup"><span data-stu-id="fdac9-131">wget</span></span>](http://gnuwin32.sourceforge.net/packages/wget.htm)

<span data-ttu-id="fdac9-132">Navíc můžete použít síťové kartě Nástroje pro ladění F12 v Internet Exploreru zobrazíte odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="fdac9-132">Also, you can use the Network tab of the F12 Debugging Tools in Internet Explorer to view the HTTP responses.</span></span>

<span data-ttu-id="fdac9-133">V tomto příkladu chceme se zobrazí odpověď z našich test adresy URL: http://watestsdp2008r2.cloudapp.net:80/testu.</span><span class="sxs-lookup"><span data-stu-id="fdac9-133">For this example we want to see the response from our probe URL: http://watestsdp2008r2.cloudapp.net:80/Probe.</span></span> <span data-ttu-id="fdac9-134">Následující příklad PowerShell ukazuje problém.</span><span class="sxs-lookup"><span data-stu-id="fdac9-134">The following PowerShell example illustrates the problem.</span></span>

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

<span data-ttu-id="fdac9-135">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="fdac9-135">Example output:</span></span>

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

<span data-ttu-id="fdac9-136">Všimněte si, že dostali jsme odpověď přesměrování.</span><span class="sxs-lookup"><span data-stu-id="fdac9-136">Notice that we received a redirect response.</span></span> <span data-ttu-id="fdac9-137">Jak uvádí všechny StatusCode dříve, než je 200 považují za chybu.</span><span class="sxs-lookup"><span data-stu-id="fdac9-137">As stated previously, any StatusCode other than 200 is considered a failure.</span></span> <span data-ttu-id="fdac9-138">Traffic Manager změní stav koncového bodu na Offline.</span><span class="sxs-lookup"><span data-stu-id="fdac9-138">Traffic Manager changes the endpoint status to Offline.</span></span> <span data-ttu-id="fdac9-139">Chcete-li problém vyřešit, zkontrolujte konfiguraci webu k zajištění, že může být vráceny správné StatusCode z cesty k testu.</span><span class="sxs-lookup"><span data-stu-id="fdac9-139">To resolve the problem, check the website configuration to ensure that the proper StatusCode can be returned from the probe path.</span></span> <span data-ttu-id="fdac9-140">Překonfigurujte testu Traffic Manager tak, aby odkazoval na cestu, která vrátí hodnotu 200.</span><span class="sxs-lookup"><span data-stu-id="fdac9-140">Reconfigure the Traffic Manager probe to point to a path that returns a 200.</span></span>

<span data-ttu-id="fdac9-141">Pokud se váš test používá protokol HTTPS, musíte zakázat, aby nedocházelo k chybám protokolu SSL/TLS během svůj test ověřování certifikátů.</span><span class="sxs-lookup"><span data-stu-id="fdac9-141">If your probe is using the HTTPS protocol, you may need to disable certificate checking to avoid SSL/TLS errors during your test.</span></span> <span data-ttu-id="fdac9-142">Následující příkazy prostředí PowerShell zakázat ověření certifikátu pro aktuální relace prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="fdac9-142">The following PowerShell statements disable certificate validation for the current PowerShell session:</span></span>

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a><span data-ttu-id="fdac9-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdac9-143">Next Steps</span></span>

[<span data-ttu-id="fdac9-144">O metodách směrování provozu Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="fdac9-144">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)

[<span data-ttu-id="fdac9-145">Co je Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="fdac9-145">What is Traffic Manager</span></span>](traffic-manager-overview.md)

[<span data-ttu-id="fdac9-146">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="fdac9-146">Cloud Services</span></span>](http://go.microsoft.com/fwlink/?LinkId=314074)

[<span data-ttu-id="fdac9-147">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="fdac9-147">Azure Web Apps</span></span>](https://azure.microsoft.com/documentation/services/app-service/web/)

[<span data-ttu-id="fdac9-148">Operace v Traffic Manageru (referenční informace k rozhraní API REST)</span><span class="sxs-lookup"><span data-stu-id="fdac9-148">Operations on Traffic Manager (REST API Reference)</span></span>](http://go.microsoft.com/fwlink/?LinkId=313584)

<span data-ttu-id="fdac9-149">[Rutiny Azure Traffic Manager][1]</span><span class="sxs-lookup"><span data-stu-id="fdac9-149">[Azure Traffic Manager Cmdlets][1]</span></span>

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
