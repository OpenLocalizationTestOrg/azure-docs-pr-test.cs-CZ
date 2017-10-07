---
title: aspekty aaaPerformance pro Azure Traffic Manageru | Microsoft Docs
description: "Pochopení výkonu v Traffic Manageru a jak tootest výkon webu při použití Správce provozu"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 3ba5dfa1-2922-43f1-9a23-d06969c4a516
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: fd4e6cb221a2ceee63ec57237ee90fd714e91db8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-considerations-for-traffic-manager"></a>Důležité informace o výkonu nástroje Traffic Manager

Tato stránka vysvětluje aspekty výkonu pomocí Traffic Manager. Vezměte v úvahu hello následující scénář:

Máte instance vašeho webu v hello WestUS a EastAsia oblasti. Jedna z instancí hello selhává Kontrola stavu hello hello provoz manager kontroly. Provoz aplikace je v pořádku oblast řízené toohello. Je očekávána toto převzetí služeb při selhání ale výkon může být problém podle hello latence hello provozu teď cestě tooa vzdáleným oblast.

## <a name="performance-considerations-for-traffic-manager"></a>Důležité informace o výkonu nástroje Traffic Manager

Hello pouze vlivu na výkon s Traffic Manager na vašem webu je hello počáteční vyhledávání DNS. Žádost DNS pro název hello vašeho profilu Traffic Manageru se zpracovává souborem kořenový server hello Microsoft DNS zóny trafficmanager.net hello hostitele. Správce provozu naplní a pravidelně aktualizuje na základě zásadu Traffic Manageru hello a výsledky testu hello hello Microsoft kořenové servery DNS. Proto i během počáteční vyhledávání DNS hello žádné dotazy DNS jsou odesílány tooTraffic správce.

Správce provozu se skládá z několika komponent: název DNS servery, služby rozhraní API, vrstvy úložiště hello a koncový bod služby monitorování. Pokud součást služby Traffic Manager selže, neexistuje žádný vliv na název DNS hello přidružené vašeho profilu Traffic Manageru. Hello záznamy v serverech DNS od Microsoftu hello zůstanou beze změny. Ale monitorování koncového bodu a aktualizace DNS není dojít. Proto Traffic Manager není možné tooupdate DNS toopoint tooyour převzetí služeb při selhání lokality Pokud primární lokalita ocitne mimo provoz.

Překlad názvu DNS je rychlá a výsledky jsou uloženy v mezipaměti. rychlost Hello hello počáteční vyhledávání DNS závisí na hello, které používá klienta hello servery DNS pro rozlišení názvu. Klienta můžete obvykle dokončení vyhledávání DNS v rámci ~ 50 ms. Hello výsledky vyhledávání hello jsou uložená v mezipaměti pro dobu trvání hello hello DNS Time-to-live (TTL). Hello výchozí hodnota TTL pro správce provozu je 300 sekund.

NENÍ přenos prostřednictvím Traffic Manager. Po dokončení vyhledávání DNS hello hello klient má IP adresu pro instanci webového serveru. Klient Hello připojuje přímo toothat adresu a nepředává prostřednictvím Traffic Manager. Hello zásadu Traffic Manageru, který zvolíte nemá vliv na výkon DNS hello. Výkonu směrování metody však může negativně ovlivnit prostředí aplikace hello. Například pokud vaše zásady přesměruje přenosy z instance tooan Severní Ameriku hostované v Asii, hello latence sítě pro tyto relace může být problém s výkonem.

## <a name="measuring-traffic-manager-performance"></a>Měření výkonu Traffic Manageru

Existuje několik weby, které můžete použít toounderstand hello výkonu a chování profil Traffic Manageru. Mnoho z těchto lokalit jsou zdarma, ale mohou mít omezení. Některé servery nabízejí rozšířené monitorování a vytváření sestav pro poplatek.

Hello nástroje v těchto lokalitách měřit DNS latenci a zobrazení hello přeložit IP adresy pro umístění obsahu kolem hello, world. Většina těchto nástrojů Neukládat do mezipaměti hello výsledky DNS. Proto hello nástroje zobrazit úplnou vyhledávání DNS hello při každém spuštění testu. Při testování z vlastního klienta můžete jenom výkon hello úplné DNS vyhledávání jednou během doby trvání TTL hello.

## <a name="sample-tools-toomeasure-dns-performance"></a>Ukázka nástroje toomeasure DNS výkonu

* [SolveDNS](http://www.solvedns.com/dns-comparison/)

    SolveDNS nabízí celou řadu nástrojů výkonu. Nástroj pro porovnání DNS Hello může zobrazit, jak dlouho trvalo tooresolve, název DNS a jak který porovná tooother poskytovatelé služeb DNS.

* [WebSitePulse](http://www.websitepulse.com/help/tools.php)

    Jedním z nejjednodušší nástrojů hello je WebSitePulse. Zadejte dobu překladu názvů DNS toosee URL hello, prvním bajtem, posledního bajtu a další statistiky výkonu. Můžete ze tří různých testovací umístění. V tomto příkladu se zobrazí první spuštění hello ukazuje, že vyhledávání DNS trvá 0.204 sekundu.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse.png)

    Protože hello výsledky jsou uloženy v mezipaměti, hello druhý test pro hello stejné vyhledávání DNS hello Traffic Manager koncového bodu trvá 0,002 sekundu.

    ![pulse2](./media/traffic-manager-performance-considerations/traffic-manager-web-site-pulse2.png)

* [Monitorování aplikace Synthetic certifikační Autority](https://asm.ca.com/en/checkit.php)

    Dříve označované jako nástroj hello Watchmouse kontrola webu, tento web ukazují čas hello překlad DNS z různých geografických oblastech současně. Zadejte dobu překladu názvů DNS toosee URL hello, čas připojení a rychlost z několika zeměpisné umístění. Tento test toosee hostované služby, která je vrácena používejte pro jiné umístění kolem hello, world.

    ![pulse1](./media/traffic-manager-performance-considerations/traffic-manager-web-site-watchmouse.png)

* [Pingdom](http://tools.pingdom.com/)

    Tento nástroj poskytuje statistiku výkonu pro každý prvek webové stránky. Hello analýza stránky karta ukazuje hello procento času stráveného na vyhledávání DNS.

* [Co je můj DNS?](http://www.whatsmydns.net/)

    Tento web nemá vyhledávání DNS z různých míst na 20 a zobrazí výsledky hello na mapě.

* [Prozkoumat webové rozhraní](http://www.digwebinterface.com)

    Tento web obsahuje že podrobnější informace DNS včetně záznamů CNAME a záznamy. Zajistěte, aby zkontrolujte hello výstup Kolorovat' a 'statistiky, v nabídce Možnosti a vyberte všechny' pod Nameservers.

## <a name="next-steps"></a>Další kroky

[O metodách směrování provozu Traffic Manager](traffic-manager-routing-methods.md)

[Test nastavení Traffic Manager](traffic-manager-testing-settings.md)

[Operace v Traffic Manageru (referenční informace k rozhraní API REST)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Rutiny Azure Traffic Manager](http://go.microsoft.com/fwlink/p/?LinkId=400769)

