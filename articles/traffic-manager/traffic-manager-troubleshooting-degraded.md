---
title: "aaaTroubleshooting degradovaný stav na Azure Traffic Manager"
description: "Jak tootroubleshoot profily Traffic Manageru, když se zobrazí jako degradovaný stav."
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
ms.openlocfilehash: fd95697781472b52e98d856e66beb7b89dfeaf23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a><span data-ttu-id="53251-103">Řešení potíží s degradovaný stav na Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="53251-103">Troubleshooting degraded state on Azure Traffic Manager</span></span>

<span data-ttu-id="53251-104">Tento článek popisuje, jak tootroubleshoot profilu Azure Traffic Manageru, se zobrazuje degradovaném stavu.</span><span class="sxs-lookup"><span data-stu-id="53251-104">This article describes how tootroubleshoot an Azure Traffic Manager profile that is showing a degraded status.</span></span> <span data-ttu-id="53251-105">Pro tento scénář zvažte, které jste nakonfigurovali profil Traffic Manageru odkazující toosome cloudapp.net hostovaných služeb.</span><span class="sxs-lookup"><span data-stu-id="53251-105">For this scenario, consider that you have configured a Traffic Manager profile pointing toosome of your cloudapp.net hosted services.</span></span> <span data-ttu-id="53251-106">Pokud se zobrazí stav hello z vaší Traffic Manager **snížený** stav a stav hello jeden nebo více koncových bodů může být **snížený**:</span><span class="sxs-lookup"><span data-stu-id="53251-106">If hello health of your Traffic Manager displays a **Degraded** status, then hello status of one or more endpoints may be **Degraded**:</span></span>

![Stav sníženou koncových bodů](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

<span data-ttu-id="53251-108">Pokud se zobrazí stav hello z vaší Traffic Manager **neaktivní** stavu, pak oba koncové body mohou být **zakázané**:</span><span class="sxs-lookup"><span data-stu-id="53251-108">If hello health of your Traffic Manager displays an **Inactive** status, then both end points may be **Disabled**:</span></span>

![Neaktivní stav Traffic Manager](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a><span data-ttu-id="53251-110">Sondy Principy Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="53251-110">Understanding Traffic Manager probes</span></span>

* <span data-ttu-id="53251-111">Správce provozu zvažuje koncový bod toobe ONLINE jenom v případě, test hello obdrží odpověď HTTP 200 zpět z cesty testu hello.</span><span class="sxs-lookup"><span data-stu-id="53251-111">Traffic Manager considers an endpoint toobe ONLINE only when hello probe receives an HTTP 200 response back from hello probe path.</span></span> <span data-ttu-id="53251-112">Druhá odpověď bez 200 je selhání.</span><span class="sxs-lookup"><span data-stu-id="53251-112">Any other non-200 response is a failure.</span></span>
* <span data-ttu-id="53251-113">Přesměrování 30 x selže, i když hello přesměrováno vrátí adresu URL, 200.</span><span class="sxs-lookup"><span data-stu-id="53251-113">A 30x redirect fails, even if hello redirected URL returns a 200.</span></span>
* <span data-ttu-id="53251-114">Pro sondy HTTPs jsou chyby certifikátu ignorovat.</span><span class="sxs-lookup"><span data-stu-id="53251-114">For HTTPs probes, certificate errors are ignored.</span></span>
* <span data-ttu-id="53251-115">skutečný obsah Hello hello testu cesty není důležité, tak dlouho, dokud se vrátí 200.</span><span class="sxs-lookup"><span data-stu-id="53251-115">hello actual content of hello probe path doesn't matter, as long as a 200 is returned.</span></span> <span data-ttu-id="53251-116">Zjišťování adresa URL toosome statického obsahu jako "/ favicon.ico" je běžné techniky.</span><span class="sxs-lookup"><span data-stu-id="53251-116">Probing a URL toosome static content like "/favicon.ico" is a common technique.</span></span> <span data-ttu-id="53251-117">Dynamický obsah, jako jsou stránky ASP hello, nemusí vracet vždycky 200, i v případě, že aplikace hello je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="53251-117">Dynamic content, like hello ASP pages, may not always return 200, even when hello application is healthy.</span></span>
* <span data-ttu-id="53251-118">Osvědčeným postupem je tooset hello testu toosomething cestu, která má dostatek toodetermine logiku, která hello lokality je nahoru nebo dolů.</span><span class="sxs-lookup"><span data-stu-id="53251-118">A best practice is tooset hello Probe path toosomething that has enough logic toodetermine that hello site is up or down.</span></span> <span data-ttu-id="53251-119">V hello předchozí příklad, a to nastavením hello cesta too"/favicon.ico", jsou jenom testování této w3wp.exe reaguje.</span><span class="sxs-lookup"><span data-stu-id="53251-119">In hello previous example, by setting hello path too"/favicon.ico", you are only testing that w3wp.exe is responding.</span></span> <span data-ttu-id="53251-120">Tento test nemusí znamenat, že webová aplikace v pořádku.</span><span class="sxs-lookup"><span data-stu-id="53251-120">This probe may not indicate that your web application is healthy.</span></span> <span data-ttu-id="53251-121">Lepší volbou by tooset cesta tooa něco jako například "/ Probe.aspx" má logiku toodetermine hello stavu hello lokality.</span><span class="sxs-lookup"><span data-stu-id="53251-121">A better option would be tooset a path tooa something such as "/Probe.aspx" that has logic toodetermine hello health of hello site.</span></span> <span data-ttu-id="53251-122">Můžete například použít využití tooCPU čítače výkonu nebo měr hello počet neúspěšných požadavků.</span><span class="sxs-lookup"><span data-stu-id="53251-122">For example, you could use performance counters tooCPU utilization or measure hello number of failed requests.</span></span> <span data-ttu-id="53251-123">Nebo může pokusit tooaccess databáze prostředků nebo toomake stav relace se, zda text hello webové aplikace funguje.</span><span class="sxs-lookup"><span data-stu-id="53251-123">Or you could attempt tooaccess database resources or session state toomake sure that hello web application is working.</span></span>
* <span data-ttu-id="53251-124">Pokud jsou degradovány všechny koncové body v profilu, pak Traffic Manager zpracovává všechny koncové body v pořádku a směrování provozu tooall koncové body.</span><span class="sxs-lookup"><span data-stu-id="53251-124">If all endpoints in a profile are degraded, then Traffic Manager treats all endpoints as healthy and routes traffic tooall endpoints.</span></span> <span data-ttu-id="53251-125">To zaručuje, že problémy s hello zjišťování mechanismus nevedou k dokončení výpadku služby.</span><span class="sxs-lookup"><span data-stu-id="53251-125">This behavior ensures that problems with hello probing mechanism do not result in a complete outage of your service.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="53251-126">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="53251-126">Troubleshooting</span></span>

<span data-ttu-id="53251-127">tootroubleshoot selhání testu, musíte nástroj, který ukazuje návratový kód stavu HTTP hello z hello test adresy URL.</span><span class="sxs-lookup"><span data-stu-id="53251-127">tootroubleshoot a probe failure, you need a tool that shows hello HTTP status code return from hello probe URL.</span></span> <span data-ttu-id="53251-128">Existují celou řadu nástrojů, které jsou k dispozici, zobrazující hello nezpracovaná odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="53251-128">There are many tools available that show you hello raw HTTP response.</span></span>

* [<span data-ttu-id="53251-129">Fiddler</span><span class="sxs-lookup"><span data-stu-id="53251-129">Fiddler</span></span>](http://www.telerik.com/fiddler)
* [<span data-ttu-id="53251-130">curl</span><span class="sxs-lookup"><span data-stu-id="53251-130">curl</span></span>](https://curl.haxx.se/)
* [<span data-ttu-id="53251-131">wget</span><span class="sxs-lookup"><span data-stu-id="53251-131">wget</span></span>](http://gnuwin32.sourceforge.net/packages/wget.htm)

<span data-ttu-id="53251-132">Navíc můžete hello síťové kartě hello nástroje pro ladění F12 v Internet Exploreru tooview hello HTTP odpovědi.</span><span class="sxs-lookup"><span data-stu-id="53251-132">Also, you can use hello Network tab of hello F12 Debugging Tools in Internet Explorer tooview hello HTTP responses.</span></span>

<span data-ttu-id="53251-133">V tomto příkladu chceme toosee hello odpověď z našich test adresy URL: http://watestsdp2008r2.cloudapp.net:80/testu.</span><span class="sxs-lookup"><span data-stu-id="53251-133">For this example we want toosee hello response from our probe URL: http://watestsdp2008r2.cloudapp.net:80/Probe.</span></span> <span data-ttu-id="53251-134">Následující příklad PowerShell Hello ilustruje problém hello.</span><span class="sxs-lookup"><span data-stu-id="53251-134">hello following PowerShell example illustrates hello problem.</span></span>

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

<span data-ttu-id="53251-135">Příklad výstupu:</span><span class="sxs-lookup"><span data-stu-id="53251-135">Example output:</span></span>

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

<span data-ttu-id="53251-136">Všimněte si, že dostali jsme odpověď přesměrování.</span><span class="sxs-lookup"><span data-stu-id="53251-136">Notice that we received a redirect response.</span></span> <span data-ttu-id="53251-137">Jak uvádí všechny StatusCode dříve, než je 200 považují za chybu.</span><span class="sxs-lookup"><span data-stu-id="53251-137">As stated previously, any StatusCode other than 200 is considered a failure.</span></span> <span data-ttu-id="53251-138">Správce provozu změny hello tooOffline stav koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="53251-138">Traffic Manager changes hello endpoint status tooOffline.</span></span> <span data-ttu-id="53251-139">tooresolve hello problém, zkontrolujte hello webu konfigurace tooensure, který hello správné StatusCode lze vrátit ze hello testu cestu.</span><span class="sxs-lookup"><span data-stu-id="53251-139">tooresolve hello problem, check hello website configuration tooensure that hello proper StatusCode can be returned from hello probe path.</span></span> <span data-ttu-id="53251-140">Překonfigurujte hello Traffic Manager testu toopoint tooa cestu, která vrátí hodnotu 200.</span><span class="sxs-lookup"><span data-stu-id="53251-140">Reconfigure hello Traffic Manager probe toopoint tooa path that returns a 200.</span></span>

<span data-ttu-id="53251-141">Pokud se váš test používá hello protokol HTTPS, musíte certifikát toodisable kontrola tooavoid SSL/TLS chyby během svůj test.</span><span class="sxs-lookup"><span data-stu-id="53251-141">If your probe is using hello HTTPS protocol, you may need toodisable certificate checking tooavoid SSL/TLS errors during your test.</span></span> <span data-ttu-id="53251-142">Hello následující příkazy prostředí PowerShell zakázat ověření certifikátu pro hello aktuální relace prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="53251-142">hello following PowerShell statements disable certificate validation for hello current PowerShell session:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="53251-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="53251-143">Next Steps</span></span>

[<span data-ttu-id="53251-144">O metodách směrování provozu Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="53251-144">About Traffic Manager traffic routing methods</span></span>](traffic-manager-routing-methods.md)

[<span data-ttu-id="53251-145">Co je Traffic Manageru</span><span class="sxs-lookup"><span data-stu-id="53251-145">What is Traffic Manager</span></span>](traffic-manager-overview.md)

[<span data-ttu-id="53251-146">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="53251-146">Cloud Services</span></span>](http://go.microsoft.com/fwlink/?LinkId=314074)

[<span data-ttu-id="53251-147">Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="53251-147">Azure Web Apps</span></span>](https://azure.microsoft.com/documentation/services/app-service/web/)

[<span data-ttu-id="53251-148">Operace v Traffic Manageru (referenční informace k rozhraní API REST)</span><span class="sxs-lookup"><span data-stu-id="53251-148">Operations on Traffic Manager (REST API Reference)</span></span>](http://go.microsoft.com/fwlink/?LinkId=313584)

<span data-ttu-id="53251-149">[Rutiny Azure Traffic Manager][1]</span><span class="sxs-lookup"><span data-stu-id="53251-149">[Azure Traffic Manager Cmdlets][1]</span></span>

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
