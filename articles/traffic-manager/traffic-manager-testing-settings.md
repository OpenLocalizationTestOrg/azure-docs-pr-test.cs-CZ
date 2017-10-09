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
# <a name="verify-traffic-manager-settings"></a>Ověřte nastavení Traffic Manager

tootest nastavení Traffic Manager, je nutné toohave více klientů v různých umístěních, ze kterých můžete spustit testy. Přepněte hello koncových bodů ve vašem profilu Traffic Manageru dolů po jednom.

* Nastavte hello hodnota DNS TTL nízkou tak, aby změny rozšíří rychle (například 30 sekund).
* Znáte IP adresy služby Azure cloud services a weby v hello profilu, který testujete hello.
* Pomocí nástrojů, které umožňují přeložit název DNS tooan IP adresu a zobrazit tuto adresu.

Při kontrole toosee, názvy DNS hello překládání adres tooIP hello koncových bodů ve vašem profilu. musí se překládat názvy Hello v souladu s metodu směrování hello provozu definované v hello profil služby Traffic Manager. Můžete použít nástroje hello jako **nslookup** nebo **prozkoumat** tooresolve názvy DNS.

Hello následující příklady můžete otestovat váš profil Traffic Manageru.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Zkontrolujte profil služby Traffic Manager pomocí nástroje nslookup a ipconfig v systému Windows

1. Otevřete příkaz či řádku prostředí Windows PowerShell jako správce.
2. Typ `ipconfig /flushdns` tooflush hello mezipaměť Překladač DNS.
3. Zadejte `nslookup <your Traffic Manager domain name>`. Například hello následující příkaz kontroly hello název domény s předponou hello *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Obvykle za následek ukazuje hello následující informace:

    + Hello název DNS a IP adresu se server DNS hello přístup tooresolve tento název domény Traffic Manageru.
    + název domény Traffic Manageru Hello jste zadali na příkazovém řádku hello po nástroje "nslookup" a přeloží hello IP adresu toowhich hello doménu Traffic Manageru. Hello druhou IP adresu je důležité jeden toocheck hello. Měla by se shodovat veřejné virtuální adresy IP (VIP) pro jeden z hello cloudové služby nebo webů v hello profil služby Traffic Manager, které testujete.

## <a name="how-tootest-hello-failover-traffic-routing-method"></a>Jak metoda směrování provozu tootest hello převzetí služeb při selhání

1. Ponechejte si všechny koncové body.
2. Pomocí jednoho klienta, požadavky na překlad DNS pro název domény vaší společnosti, pomocí nástroje nslookup nebo podobného nástroje.
3. Zkontrolujte, zda že tento hello řešena IP adresa odpovídá hello primární koncový bod.
4. Přeneste dolů váš primární koncový bod nebo odeberte hello monitorování souboru tak, aby správce provozu se domnívá, které aplikace hello je vypnutý.
5. Počkejte hello DNS Time-to-Live (TTL) profil služby Traffic Manager hello plus dalších dvou minut. Například pokud vaše TTL DNS je 300 sekund (5 minut), je nutné počkat sedm minut.
6. Vyprázdnění vaší DNS klienta mezipaměti a žádosti o překlad DNS pomocí nástroje nslookup. V systému Windows můžete vyprázdnit mezipaměť DNS pomocí hello ipconfig/flushdns příkazu.
7. Zkontrolujte, zda že tento hello řešena IP adresa odpovídá sekundární koncový bod.
8. Opakujte postup hello, zase ukončování každý koncový bod. Ověřte, že hello DNS vrátí hello IP adresa hello další koncového bodu v seznamu hello. Když jsou všechny koncové body dolů, by měl znovu získat IP adresu hello hello primární koncový bod.

## <a name="how-tootest-hello-weighted-traffic-routing-method"></a>Jak tootest hello vážené metodu směrování provozu

1. Ponechejte si všechny koncové body.
2. Pomocí jednoho klienta, požadavky na překlad DNS pro název domény vaší společnosti, pomocí nástroje nslookup nebo podobného nástroje.
3. Zkontrolujte, zda že tento hello řešena IP adresa odpovídá jednomu z koncových bodů.
4. Vyprázdnění mezipaměti klienta DNS a zopakujte kroky 2 a 3 pro každý koncový bod. Měli byste vidět různé IP adresy vrátí pro každou z koncových bodů.

## <a name="how-tootest-hello-performance-traffic-routing-method"></a>Jak tootest hello výkonu metoda směrování provozu

tooeffectively test metodu směrování provozu výkonu, musí mít klienti nacházející se v různých částech hello, world. Klienty můžete vytvořit v různých oblastech Azure, které se dají použít tootest vašim službám. Pokud máte globální sítě, můžete vzdáleně přihlaste tooclients v dalších částech tohoto hello, world a spouštění testů z ní.

Alternativně existují uvolněte webové vyhledávání DNS a víc služeb, které jsou k dispozici. Některé z těchto nástrojů pro udělení hello překlad názvu DNS možnost toocheck z různých míst kolem hello, world. Proveďte hledání na "Vyhledávání DNS" příklady. Služby třetích stran, jako je Gomez nebo //Build lze použít tooconfirm profilech distribuovanému přenosy podle očekávání.

## <a name="next-steps"></a>Další kroky

* [O metodách směrování provozu Traffic Manager](traffic-manager-routing-methods.md)
* [Důležité informace o výkonu nástroje Traffic Manager](traffic-manager-performance-considerations.md)
* [Řešení potíží při sníženém výkonu Traffic Manageru](traffic-manager-troubleshooting-degraded.md)
