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
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Řešení potíží s degradovaný stav na Azure Traffic Manager

Tento článek popisuje, jak tootroubleshoot profilu Azure Traffic Manageru, se zobrazuje degradovaném stavu. Pro tento scénář zvažte, které jste nakonfigurovali profil Traffic Manageru odkazující toosome cloudapp.net hostovaných služeb. Pokud se zobrazí stav hello z vaší Traffic Manager **snížený** stav a stav hello jeden nebo více koncových bodů může být **snížený**:

![Stav sníženou koncových bodů](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Pokud se zobrazí stav hello z vaší Traffic Manager **neaktivní** stavu, pak oba koncové body mohou být **zakázané**:

![Neaktivní stav Traffic Manager](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Sondy Principy Traffic Manager

* Správce provozu zvažuje koncový bod toobe ONLINE jenom v případě, test hello obdrží odpověď HTTP 200 zpět z cesty testu hello. Druhá odpověď bez 200 je selhání.
* Přesměrování 30 x selže, i když hello přesměrováno vrátí adresu URL, 200.
* Pro sondy HTTPs jsou chyby certifikátu ignorovat.
* skutečný obsah Hello hello testu cesty není důležité, tak dlouho, dokud se vrátí 200. Zjišťování adresa URL toosome statického obsahu jako "/ favicon.ico" je běžné techniky. Dynamický obsah, jako jsou stránky ASP hello, nemusí vracet vždycky 200, i v případě, že aplikace hello je v pořádku.
* Osvědčeným postupem je tooset hello testu toosomething cestu, která má dostatek toodetermine logiku, která hello lokality je nahoru nebo dolů. V hello předchozí příklad, a to nastavením hello cesta too"/favicon.ico", jsou jenom testování této w3wp.exe reaguje. Tento test nemusí znamenat, že webová aplikace v pořádku. Lepší volbou by tooset cesta tooa něco jako například "/ Probe.aspx" má logiku toodetermine hello stavu hello lokality. Můžete například použít využití tooCPU čítače výkonu nebo měr hello počet neúspěšných požadavků. Nebo může pokusit tooaccess databáze prostředků nebo toomake stav relace se, zda text hello webové aplikace funguje.
* Pokud jsou degradovány všechny koncové body v profilu, pak Traffic Manager zpracovává všechny koncové body v pořádku a směrování provozu tooall koncové body. To zaručuje, že problémy s hello zjišťování mechanismus nevedou k dokončení výpadku služby.

## <a name="troubleshooting"></a>Řešení potíží

tootroubleshoot selhání testu, musíte nástroj, který ukazuje návratový kód stavu HTTP hello z hello test adresy URL. Existují celou řadu nástrojů, které jsou k dispozici, zobrazující hello nezpracovaná odpověď HTTP.

* [Fiddler](http://www.telerik.com/fiddler)
* [curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Navíc můžete hello síťové kartě hello nástroje pro ladění F12 v Internet Exploreru tooview hello HTTP odpovědi.

V tomto příkladu chceme toosee hello odpověď z našich test adresy URL: http://watestsdp2008r2.cloudapp.net:80/testu. Následující příklad PowerShell Hello ilustruje problém hello.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Příklad výstupu:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

Všimněte si, že dostali jsme odpověď přesměrování. Jak uvádí všechny StatusCode dříve, než je 200 považují za chybu. Správce provozu změny hello tooOffline stav koncového bodu. tooresolve hello problém, zkontrolujte hello webu konfigurace tooensure, který hello správné StatusCode lze vrátit ze hello testu cestu. Překonfigurujte hello Traffic Manager testu toopoint tooa cestu, která vrátí hodnotu 200.

Pokud se váš test používá hello protokol HTTPS, musíte certifikát toodisable kontrola tooavoid SSL/TLS chyby během svůj test. Hello následující příkazy prostředí PowerShell zakázat ověření certifikátu pro hello aktuální relace prostředí PowerShell:

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

## <a name="next-steps"></a>Další kroky

[O metodách směrování provozu Traffic Manager](traffic-manager-routing-methods.md)

[Co je Traffic Manageru](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Operace v Traffic Manageru (referenční informace k rozhraní API REST)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Rutiny Azure Traffic Manager][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
