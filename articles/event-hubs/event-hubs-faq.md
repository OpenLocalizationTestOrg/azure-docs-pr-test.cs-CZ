---
title: "aaaAzure Event Hubs – nejčastější dotazy | Microsoft Docs"
description: "Nejčastější dotazy (FAQ) Azure Event Hubs"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: bfa10984-eb22-4671-861a-f377a90d9372
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: sethm;shvija
ms.openlocfilehash: cc0844edcc38ad167cde9497d58d44155fc90b7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-frequently-asked-questions"></a>Služba Event Hubs a nejčastější dotazy

## <a name="general"></a>Obecné

### <a name="what-is-hello-difference-between-event-hubs-basic-and-standard-tiers"></a>Co je hello rozdíl mezi základní centra událostí a standardní úrovně?

úroveň Standard Hello služby Azure Event Hubs poskytuje funkce nad rámec k dispozici v základní vrstvě hello. Hello následující funkce jsou standardní součástí:
* Uchování delší událostí
* Další zprostředkovaná připojení s Nadlimitní poplatek za více než číslo hello zahrnuté
* Více než jedné skupiny příjemců
* [Zachycení](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview)

Další informace o cenových úrovní, včetně vyhrazené centra událostí, najdete v části hello [podrobnosti o cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Jaké jsou jednotky propustnosti centra událostí?

Vyberete explicitně jednotky propustnosti centra událostí, buď prostřednictvím hello portál Azure nebo šablony správce prostředků centra událostí. Jednotky propustnosti použijte tooall centra událostí v oboru názvů služby Event Hubs a jednotlivých jednotek propustnosti opravňuje hello obor názvů toohello následující možnosti:

* Až too1 MB za sekundu události příchozího (událostí odeslaných do centra událostí), ale žádné více než 1 000 události příchozího, operace správy nebo ovládacího prvku volání rozhraní API za sekundu.
* Až too2 MB za sekundu odchozí událostí (událostí spotřebované z centra událostí).
* Až too84 GB úložiště událostí (dostačující pro hello výchozí dobu uchování 24 hodin).

Jednotky propustnosti centra událostí se účtují HODINOVĚ, podle hello maximální počet jednotek vybraný během hello zadané hodiny.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Jak se vynucují omezení jednotka propustnosti centra událostí?
Překročí-li propustnost hello celkový příjem příchozích dat nebo hello příchozího celkový počet událostí ve všech centrech událostí v oboru názvů hello agregovanou propustnost jednotky příspěvky, odesílatelé jsou omezené a zobrazí se chyba oznamující, že byla překročena kvóta příjem příchozích dat této hello.

Překročí-li propustnost celkový odchozí hello nebo rychlost odchozí hello celkový počet událostí ve všech centrech událostí v oboru názvů hello agregovanou propustnost jednotky příspěvky, příjemci jsou omezené a zobrazí se chyba oznamující, že tento hello odchozí kvóta byla překročena. Příchozí a odchozí kvóty se vynucují samostatně, tak, aby žádné odesílatele může způsobit událostí spotřeba tooslow dolů, ani příjemce zabránit události odesílány do centra událostí.

### <a name="is-there-a-limit-on-hello-number-of-throughput-units-that-can-be-selected"></a>Existuje nějaké omezení hello počtu jednotek propustnosti, které je možné vybrat?
Je výchozí kvótu 20 jednotek propustnosti na obor názvů. Vyplnění lístek podpory, může vyžádat větší kvótu jednotek propustnosti. Nad rámec hello 20 limit jednotky propustnosti sady jsou k dispozici v jednotky propustnosti 20 a 100. Všimněte si, že pomocí více než 20 jednotky propustnosti odebere hello možnost toochange hello počet jednotek propustnosti bez vyplnění lístek podpory.

### <a name="can-i-use-a-single-amqp-connection-toosend-and-receive-from-multiple-event-hubs"></a>Můžete použít jeden toosend připojení AMQP a přijímat z více event hubs?
Ano, pokud jsou všech centrech událostí hello v hello stejný obor názvů.

### <a name="what-is-hello-maximum-retention-period-for-events"></a>Jaký je interval hello maximální doba uchování pro události?
Maximální doba uchování období 7 dní v současné době podporuje úrovně Standard centra událostí. Všimněte si, že služby event hubs nejsou určeny jako úložiště dat trvalé. Období uchování delší, než 24 hodin, které jsou určené pro scénáře, ve kterých je vhodné tooreplay do datového proudu událost hello stejné systémy; například tootrain nebo ověřte nový model strojového učení na existující data. Pokud třeba zprávy uchování za 7 dní, povolení [zaznamenat centra událostí](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) na událost rozbočovače čte hello data z účtu úložiště toohello centra událostí nebo účet služby Azure Data Lake dle vlastního výběru. Povolení zachycení způsobuje poplatků, na základě vaší zakoupené jednotky propustnosti.

### <a name="where-is-azure-event-hubs-available"></a>Kde je k dispozici Azure Event Hubs?
Azure Event Hubs je k dispozici ve všech oblastech podporovaných Azure. Pro seznam, navštivte hello [oblastí Azure](https://azure.microsoft.com/regions/) stránky.  

## <a name="best-practices"></a>Osvědčené postupy

### <a name="how-many-partitions-do-i-need"></a>Kolik oddíly potřebuji?
Mějte na paměti, která hello počet oddílů v Centru událostí prosím nemůže být upraven po dokončení instalace. Si uvědomit je důležité toothink o tom, kolik oddíly je třeba před Začínáme. 

Event Hubs je navrženou tooallow čtečku jeden oddíl na skupinu uživatelů. Ve většině případů použití stačí výchozí nastavení hello čtyři oddíly. Pokud hledáte tooscale vaše zpracování událostí, měli byste tooconsider přidání další oddíly. Neexistuje žádné omezení konkrétní propustnost pro oddíl, ale hello agregovanou propustnost v oboru názvů je omezena hello počet jednotek propustnosti. Jako hello počet jednotek propustnosti zvýšit v oboru názvů, můžete další oddíly tooallow souběžných čtenářů tooachieve vlastní maximální propustnost.

Ale pokud máte modelu, ve kterém aplikace obsahuje oddíl konkrétní tooa spřažení, zvýšit počet oddílů na hello nemusí být z tooyou žádné výhody. Další informace o tom najdete v tématu [dostupnosti a konzistence](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Ceny

### <a name="where-can-i-find-more-pricing-information"></a>Kde můžete najít další informace o cenách?
Úplné informace o cenách služby Event Hubs najdete v tématu hello [podrobnosti o cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Je k dispozici zdarma pro zachování události Event Hubs pro víc než 24 hodin?
Standardní centra událostí úrovně Hello povolit uchování zpráv období je delší než 24 hodin, maximálně 7 dnů. Překročí-li velikost hello hello celkový počet událostí uložených povoleného užívání úložiště hello hello počet jednotek propustnosti vybrané (84 GB za jednotku propustnosti), je výši hello velikost, která překračuje povoleného užívání hello hello publikovaná míra úložiště objektů Blob v Azure. Hello povoleného užívání úložiště v jednotlivých jednotek propustnosti obsahuje všechny náklady na úložiště pro období 24 hodin (výchozí hello) i v případě, že jednotka propustnosti hello slouží až toohello maximální příchozího povoleného užívání.

### <a name="how-is-hello-event-hubs-storage-size-calculated-and-charged"></a>Jak bude hello velikost úložiště služby Event Hubs a bude účtovat?
Celková velikost Hello všechny uložené události, včetně žádné vnitřní režie pro záhlaví události nebo na struktury úložiště disku ve všech centrech událostí se měří v průběhu dne hello. Na konci hello hello dne je vypočítána velikost úložiště ve špičce hello. denní příspěvek úložiště Hello je vypočítáváno na hello minimální počet jednotek propustnosti, které byly vybrány během dne hello (jednotlivých jednotek propustnosti poskytuje příspěvek ve výši 84 GB). Pokud celková velikost hello překročí hello vypočítat denně povoleného užívání úložiště, hello nadbytečné úložiště se fakturuje pomocí ceníku úložiště objektů Blob v Azure (v hello **místně redundantní úložiště** rychlost).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Jak jsou vypočítávány události příchozího Event Hubs?
Každá událost odeslané centra událostí tooan počítá jako fakturovatelný zprávu. *Příjem příchozích dat událostí* je definován jako jednotka data, která je menší než nebo rovna too64 KB. Událost, která je menší než nebo rovna too64 KB velikostí je považován za toobe jedné fakturovatelný události. Pokud událost hello je větší než 64 KB, hello počet fakturovatelný událostí se počítá podle velikost události toohello, v násobcích 64 KB. Například událost 8 KB odeslané centra událostí toohello se fakturuje jako jednu událost, ale 96 KB zprávy odeslané centra událostí toohello se fakturuje jako dvě události.

Události z centra událostí, a taky operace správy a řízení volání, jako například kontrolní body, se počítají jako fakturovatelný příjem příchozích dat událostí, ale nabíhat až povoleného užívání jednotky propustnosti toohello využívat.

### <a name="do-brokered-connection-charges-apply-tooevent-hubs"></a>Zprostředkované připojení poplatky tooEvent Hubs?
Připojení poplatky jenom v případě, že se používá hello protokolu AMQP. Neexistují žádné poplatky připojení pro odesílání událostí pomocí protokolu HTTP, bez ohledu na počet hello odesílání systémy nebo zařízení. Pokud máte v plánu toouse AMQP (například tooachieve efektivnější událost streamování nebo tooenable obousměrnou komunikaci v příkazy a ovládání scénáře IoT), najdete v části hello [informace o cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) stránku Podrobnosti o počet připojení jsou zahrnuty v jednotlivých úrovních služeb.

### <a name="how-is-event-hubs-capture-billed"></a>Jak se funkce Event Hubs Capture účtuje?
Snímek se povolí, když žádné Centrum událostí v oboru názvů hello má povolenou možnost zachycení hello. Zaznamenat centra událostí se účtují HODINOVĚ za zakoupené jednotky propustnosti. Hello počet jednotek propustnosti zvětšit nebo zmenšit, zachycení událostí centra fakturace odráží tyto změny v přírůstcích po celou hodinu. Další informace o fakturaci zaznamenat centra událostí najdete v tématu [informace o cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-hello-storage-account-i-select-for-event-hubs-capture"></a>Bude možné účtován hello účtu úložiště, který vyberte pro zachycení centra událostí?
Zachycení používá účet úložiště, který zadáte, pokud je povoleno v Centru událostí. Toto je váš účet úložiště, jsou všechny změny pro tuto konfiguraci fakturovaná tooyour předplatného Azure.

## <a name="quotas"></a>Kvóty

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Existují žádné kvóty přidružené služby Event Hubs?
Seznam všech kvót služby Event Hubs naleznete v tématu [kvóty](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Řešení potíží

### <a name="what-are-some-of-hello-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Jaké jsou některé z hello výjimky generované centrům událostí a jejich doporučované akce?
Seznam možných centra událostí výjimek najdete v tématu [výjimky přehled](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Diagnostické protokoly
Služba Event Hubs podporuje dva typy [protokolů diagnostiky](event-hubs-diagnostic-logs.md) -protokoly chyb zachycení provozní protokoly a – z nich jsou reprezentované ve formátu json i lze zapnout prostřednictvím hello portálu Azure.

### <a name="support-and-sla"></a>Podpora a SLA
Technická podpora pro Event Hubs je k dispozici prostřednictvím hello [komunitní fóra](https://social.msdn.microsoft.com/forums/azure/home). Podpora k fakturaci a správě předplatného se poskytuje zadarmo.

toolearn Další informace o našich SLA, najdete v části hello [smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/) stránky.

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
