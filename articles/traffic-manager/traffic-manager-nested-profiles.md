---
title: aaaNested profily Traffic Manageru | Microsoft Docs
description: "Tento článek vysvětluje funkce 'vnořené profily hello z Azure Traffic Manager"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f1b112c4-a3b1-496e-90eb-41e235a49609
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: e476d58a82ed94d12731156280c9665f980de0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="nested-traffic-manager-profiles"></a>Vnořené profily Traffic Manageru

Traffic Manager zahrnuje celou řadu metody směrování provozu, které vám umožňují toocontrol jak zvolí Traffic Manager, kterému koncovému bodu musí přijímat přenosy z jednotlivých koncových uživatelů. Další informace najdete v tématu [metody směrování provozu Traffic Manager](traffic-manager-routing-methods.md).

Každý profil služby Traffic Manager určuje jednu metodu směrování provozu. Existují však scénáře, které vyžadují složitější směrování provozu, než hello směrování poskytuje jeden profil Traffic Manageru. Lze vnořit profily Traffic Manager toocombine hello výhody víc než jednu metodu směrování provozu. Vnořené profily umožňují toooverride hello výchozí Traffic Manager chování toosupport větší a složitější nasazení aplikací.

Hello následující příklady znázorňují, jak toouse vnořené profily Traffic Manageru v různých situacích.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Příklad 1: Kombinování 'Výkonu' a 'Vážená' směrování provozu

Předpokládejme, že jste nasadili aplikace v následujících oblastech Azure hello: západní USA, západní Evropa a ve východní Asii. Používáte Traffic Manager, výkon, směrování provozu metoda toodistribute provoz toohello oblast nejbližší toohello uživatelsky.

![Profil služby Traffic Manager jedním][4]

Nyní předpokládejme, že chcete tooyour služby aktualizace pro tootest před distribucí další široce. Chcete toouse hello 'vyvážené' směrování provozu metoda toodirect malým procentem provoz tooyour testovací nasazení. V oblasti západní Evropa nastavíte hello testovací nasazení společně se hello stávajícímu provoznímu prostředí.

Nelze kombinovat obě vážená a ' výkonu-směrování provozu v jednom profilu. toosupport v tomto scénáři vytvoříte profil Traffic Manageru pomocí západní Evropa hello dva koncové body a metodu směrování provozu "Vážená" hello. Dál přidejte tento profil, podřízené' jako koncový bod toohello "nadřazená" profil. profil nadřazené Hello stále používá hello metodu směrování provozu výkonu a obsahuje hello ostatní globální nasazení jako koncové body.

Hello následující diagram ukazuje tento příklad:

![Vnořené profily Traffic Manageru][2]

V této konfiguraci přenášená prostřednictvím profilu nadřazené hello rozděluje zatížení mezi oblasti normálně. V oblasti západní Evropa hello vnořených profilů distribuuje provoz toohello produkční a testovací koncových bodů podle toohello váhu přiřazen.

Pokud profil nadřazené hello používá metodu směrování provozu "Výkonu" hello, musí být každý koncový bod přiřazen umístění. Hello umístění bude přiřazeno při konfiguraci koncového bodu hello. Zvolte hello oblast Azure nejbližší tooyour nasazení. Hello oblastí Azure jsou hodnoty umístění hello nepodporuje hello Internet latence tabulky. Další informace najdete v tématu [Traffic Manager, výkon, metoda směrování provozu](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Příklad 2: Monitorování koncového bodu ve vnořených profilů

Traffic Manager monitoruje aktivně hello stav každý koncový bod služby. Pokud koncový bod není v pořádku, přesměruje Traffic Manager uživatelé tooalternative koncové body toopreserve hello dostupnost služby. Toto chování monitorování a převzetí služeb při selhání koncový bod se vztahuje tooall metody směrování provozu. Další informace najdete v tématu [Traffic Manager koncového bodu monitorování](traffic-manager-monitoring.md). Monitorování koncového bodu funguje jinak vnořených profilů. S vnořených profilů nemá profil nadřazené hello provádět kontroly stavu na podřízené hello přímo. Místo toho hello stav koncových bodů hello podřízené profilu je použité toocalculate hello celkový stav hello podřízené profilu. Tato informace o stavu šíří hello vnořené profil hierarchie. Hello nadřazené profil používá tento toodetermine agregovaný stav jestli profil podřízené toohello toodirect provozu. V tématu hello [– nejčastější dotazy](traffic-manager-FAQs.md#traffic-manager-nested-profiles) úplné podrobnosti o monitorování stavu serveru vnořených profilů.

Vrácení předchozí příklad toohello, Předpokládejme, že selže hello produkční nasazení v oblasti západní Evropa. Ve výchozím nastavení profil "podřízený" hello směrovat všechny přenosy toohello testovací nasazení. Pokud se nezdaří ani hello testovací nasazení, hello nadřazené profil určuje, že hello podřízené profil by neměla přijímat přenosy vzhledem k tomu, že jsou všechny koncové body podřízené není v pořádku. Potom hello nadřazené profil distribuuje provoz toohello jiných oblastí.

![Vnořené profil převzetí služeb při selhání (výchozí nastavení)][3]

Je možné radostí s toto uspořádání. Nebo může být obavy, že všechny přenosy pro západní Evropa bude toohello testovací nasazení místo omezená podmnožina provoz. Bez ohledu na stav hello hello testovací nasazení, chcete toofail přes toohello jiných oblastí při selhání hello produkční nasazení v oblasti západní Evropa. tooenable toto převzetí služeb při selhání, můžete zadat parametr 'MinChildEndpoints' hello při konfiguraci profilu podřízené hello jako koncový bod v profilu nadřazené hello. Hello parametr určuje minimální počet dostupných koncových bodů v profilu podřízené hello hello. Hello výchozí hodnota je '1'. V tomto scénáři můžete nastavit too2 hodnotu MinChildEndpoints hello. Pod touto prahovou hodnotou hello nadřazené profil zvažuje hello celý podřízené profil toobe není k dispozici a směruje provoz toohello ostatní koncové body.

Hello následující obrázek znázorňuje tuto konfiguraci:

![Vnořené převzetí služeb při selhání profil 'MinChildEndpoints' = 2][4]

> [!NOTE]
> Metoda směrování provozu "Priority" Hello distribuuje všechny přenosy tooa jeden koncový bod. V nastavení MinChildEndpoints než '1' proto není málo účel profil podřízené.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Příklad 3: Oblasti seřazených podle priorit převzetí služeb při selhání v směrování provozu 'výkonu.

výchozí chování Hello hello metodu směrování provozu, výkon, je navrženou tooavoid načítání hello přepsání vedle nejbližší koncový bod a způsobí kaskádových řady chyb. Když se koncový bod nezdaří, bude veškerý provoz, který by byl přesměrován, toothat koncový bod je rovnoměrně přes všechny oblasti distribuované toohello ostatní koncové body.

![Směrování s výchozí převzetí služeb při selhání provozu 'výkonu.][5]

Předpokládejme však raději hello západní Evropa provoz převzetí služeb při selhání tooWest nám a pokud obě koncových bodů nejsou k dispozici jen přímé provoz tooother oblasti. Toto řešení pomocí profilu podřízené hello metodu směrování provozu, Priority, můžete vytvořit.

![Směrování s přednostní převzetí služeb při selhání provozu 'výkonu.][6]

Vzhledem k tomu, že koncový bod západní Evropa hello má vyšší prioritu než koncový bod hello západní USA, všechny přenosy se odesílají koncový bod toohello západní Evropa, oba koncové body jsou online. Pokud se nezdaří západní Evropa, jeho provoz je řízené tooWest USA. S profilem hello vnořené přenosy jsou řízené tooEast Asie jenom v případě selhání západní Evropa a západní USA.

Tento vzor, můžete opakovat pro všechny oblasti. Nahradí všechny tři koncové body v profilu nadřazené hello tři profily podřízené každý poskytování pořadí seřazený podle priority převzetí služeb při selhání.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-hello-same-region"></a>Příklad 4: Kontrole přenosů dat, výkon, směrování mezi několik koncových bodů v hello stejné oblasti

Předpokládejme hello výkonu směrování provozu metoda se používá v profilu, který má více než jeden koncový bod v určité oblasti. Ve výchozím nastavení provoz směrované toothat oblast je rovnoměrně rozdělené mezi všechny koncové body k dispozici v této oblasti.

![Směrování distribuce přenosů v oblasti (výchozí chování) provozu 'výkonu.][7]

Místo přidávání několik koncových bodů v oblasti západní Evropa, jsou v profilu samostatnou podřízenou uzavřené těchto koncových bodů. Hello podřízené profil se přidá toohello nadřazené jako hello pouze koncový bod v oblasti západní Evropa. Hello nastavení v profilu podřízené hello můžete řídit distribuci přenosů hello s západní Evropa povolením směrování na základě priority nebo vyvážené provozu v rámci této oblasti.

![Směrování se distribuce přenosů vlastní v oblasti provozu 'výkonu.][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Příklad 5: Nastavení monitorování pro koncový bod

Předpokládejme, že používáte Traffic Manager toosmoothly migraci provoz z starší verze místní webu tooa nové cloudové verze hostované v Azure. Pro starší verze lokality hello budete chtít toouse hello domovskou stránku URI toomonitor stavu lokality. Ale hello nové cloudové verze, kterou implementujete vlastní monitorování stránku (cesta ' / monitor.aspx') obsahující další kontroly.

![Koncový bod Traffic Manager monitorování (výchozí nastavení)][9]

Hello monitorování nastavení v profilu Správce provozu se týká tooall koncové body v jednom profilu. S vnořených profilů použijte profil jiné podřízené za lokality toodefine různých nastavení monitorování.

![Koncový bod Traffic Manager monitorování pomocí nastavení za koncový bod][10]

## <a name="next-steps"></a>Další kroky

Další informace o [profily Traffic Manageru](traffic-manager-overview.md)

Zjistěte, jak příliš[vytvořit profil správce provozu](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png
