---
title: "aaaService sběrnici ceny a fakturace | Microsoft Docs"
description: "Přehled služby Service Bus cenové struktury."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7c45b112-e911-45ab-9203-a2e5abccd6e0
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2017
ms.author: sethm
ms.openlocfilehash: 4d06fe015baba45fef04e198363447c5541d1724
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-pricing-and-billing"></a>Service Bus ceny a fakturace
Service Bus je k dispozici v Basic, Standard a [Premium](service-bus-premium-messaging.md) vrstev. Můžete vybrat vrstvy služby pro každý obor názvů sběrnice služby, který vytvoříte, a tento výběr vrstvy platí mezi všechny entity vytvořené v daném oboru názvů.

> [!NOTE]
> Podrobné informace o cenách aktuální Service Bus, najdete v části hello [Azure Service Bus stránce s cenami](https://azure.microsoft.com/pricing/details/service-bus/)a hello [nejčastější dotazy týkající se Service Bus](service-bus-faq.md#pricing).
>
>

Service Bus používá následující dvě měřidla pro fronty a témata nebo odběry hello:

1. **Zasílání zpráv operace**: definován jako volání rozhraní API proti fronta nebo téma/odběr koncové body služby. Toto měření nahradí zprávy odesílané nebo přijímané jako primární jednotka hello fakturovatelný využití pro fronty a témata nebo předplatných.
2. **Zprostředkované připojení**: definován jako počet ve špičce hello trvalé připojení otevřete před fronty, témata a odběry, během období daného hodinových vzorkování. Toto monitorování bude platit pouze ve standardní vrstvě hello, ve kterém můžete otevřít další připojení (dříve připojení byly omezené too100 podle fronty, tématu nebo předplatného) za úplatu nominální připojení.

Hello **standardní** vrstvy zavádí dělené ceny pro operace provedené pomocí fronty a témata nebo odběry, což vede k slevy na základě svazku z % too80 na nejvyšší úrovni využití hello. Je také úrovně Standard základní poplatek za 10 za měsíc, která umožňuje tooperform too12.5 mil. operací měsíčně bez dalších poplatků.

Hello **Premium** vrstvy nabízí izolaci prostředků v rovině CPU a paměti hello tak, aby každá úloha zákazníka běží izolovaně. Kontejner prostředků se nazývá *jednotka zasílání zpráv*. Každému prémiovému obor názvů se přiřadí aspoň jedna jednotka zasílání zpráv. Pro každý obor názvů Service Bus Premium můžete koupit 1, 2 nebo 4 jednotky zasílání zpráv. Jedna úloha nebo entita může zabírat několik jednotek zasílání zpráv a hello počet jednotek zasílání zpráv můžete změnit podle libosti, ale fakturuje se podle 24hodinoví / denní sazby. Hello výsledkem je předvídatelný a Opakovatelný výkon vašeho řešení postaveného na Service Bus. Vedle toho, že je tento výkon předvídatelnější, je také rychlejší.

Všimněte si, že poplatků základní úroveň Standard hello je účtován pouze jednou za měsíc na předplatné Azure. To znamená, že po vytvoření oboru názvů Service Bus jednu standardní vrstvy, bude možné toocreate základní tolik další standardní obory názvů tak, jak chcete v rámci této stejného předplatného Azure, aniž by docházelo k další poplatky.

Hello [ceny služby Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) tabulka shrnuje hello funkční rozdíly mezi vrstvami hello Basic, Standard a Premium.

## <a name="messaging-operations"></a>Operace zasílání zpráv
Jako součást hello nový cenový model fakturace pro fronty a témata nebo odběry mění. Tyto entity přecházejí z fakturace za toobilling zpráva na operaci. "Operaci" odkazuje volání rozhraní API tooany fronta nebo téma/odběr koncový bod služby. To zahrnuje stav operace správy, odesílání a přijímání a relace.

| Typ operace | Popis |
| --- | --- |
| Správa |Vytváření, čtení, aktualizaci, odstranění (CRUD) před fronty a témata nebo předplatných. |
| Zasílání zpráv |Odesílání a přijímání zpráv pomocí front nebo témata nebo předplatných. |
| Stav relace |Získání nebo nastavení stavu relace na fronta nebo téma/odběr. |

Náklady na podrobnosti najdete v tématu hello ceny uvedené na hello [ceny služby Service Bus](https://azure.microsoft.com/pricing/details/service-bus/) stránky.

## <a name="brokered-connections"></a>Zprostředkovaná připojení
*Zprostředkované připojení* zohlednit vzorce používání zákazníka, které zahrnují velké množství "trvalé připojené" odesílatelé nebo příjemci před fronty, témata a odběry. Trvalé připojené odesílatelé nebo příjemci jsou ty, které se připojují prostřednictvím protokolu AMQP nebo HTTP s nenulovou hodnotou přijímat časový limit (například HTTP dlouhé dotazování). HTTP odesílateli a příjemci s okamžitou časový limit negenerují zprostředkovaná připojení.

Připojení kvóty a omezení jiné služby najdete v tématu hello [Service Bus kvóty](service-bus-quotas.md) článku.

úroveň Standard Hello odebere limitu zprostředkované připojení hello na obor názvů a počty využití agregační zprostředkované připojení mezi hello předplatného Azure. Další informace najdete v tématu hello [zprostředkované připojení](https://azure.microsoft.com/pricing/details/service-bus/) tabulky.

> [!NOTE]
> 1 000 zprostředkované připojení jsou součástí úrovně Standard zasílání zpráv hello (prostřednictvím hello základní zdarma) a mohou být sdíleny všechny fronty, témata a odběry v rámci předplatného Azure přidružené hello.
>
>

<br />

> [!NOTE]
> Fakturace je založený na hello nejvyšší počet souběžných připojení a je účtovány poměrnou částí po hodinách podle 744 hodin měsíčně.
>
>

| Úroveň Premium |
| --- |
| Zprostředkovaná připojení není účtován v úrovni Premium hello. |

Další informace o zprostředkovaná připojení najdete v tématu hello [– nejčastější dotazy](#faq) později v tomto tématu.

## <a name="faq"></a>Nejčastější dotazy

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Jaké jsou zprostředkované připojení a jak mohu získat účtovat poplatek za je?
Zprostředkovaná připojení je definován jako jedna z následujících hello:

1. Připojení k AMQP z fronty Service Bus tooa klienta nebo téma/odběr.
2. HTTP volání tooreceive zprávu z tématu Service Bus nebo fronty, který má hodnotu časového limitu příjmu, která je větší než nula.

Service Bus poplatky za hello nejvyšší počet souběžných zprostředkovaná připojení, které překračují hello zahrnuté množství (1000 ve standardní vrstvě hello). Vrcholů se měří hodinu, účtovány poměrnou částí vydělením 744 hodin za měsíc a přidat přes hello měsíčně fakturační období. Hello zahrnuté množství (1 000 zprostředkované připojení měsíčně) se použijí na hello konci fakturačního období hello proti hello součet hodinové vrcholů hello účtovány poměrnou částí.

Například:

1. Každý z 10 000 zařízení připojí pomocí jednoho připojení AMQP a přijímá příkazy z tématu Service Bus. Hello zařízení odesílat telemetrická data události tooan centra událostí. Pokud se všechna zařízení připojit 12 hodin denně, použít hello následující poplatky připojení (v přidání tooany další poplatky tématu Service Bus): 10 000 připojení * 12 hodin * 31 dnů / 744 = 5 000 zprostředkované připojení. Po hello měsíční příspěvek ve výši 1 000 zprostředkované připojení by se vám účtovat 4000 zprostředkovaná připojení ve hello kurzu 0.03 za zprostředkované připojení celkem $120.
2. 10 000 zařízení příjem zpráv z fronty Service Bus prostřednictvím protokolu HTTP, zadání nenulové vypršení časového limitu. Pokud všechna zařízení připojit 12 hodin denně, zobrazí se následující připojení poplatky hello (v tooany přidání další poplatky za služby Service Bus): 10 000 HTTP přijímat připojení * 12 hodin za den * 31 dnů nebo hodin 744 = 5 000 zprostředkované připojení.

### <a name="do-brokered-connection-charges-apply-tooqueues-and-topicssubscriptions"></a>Zprostředkované připojení poplatky tooqueues a témata nebo předplatné?
Ano. Neexistují žádné poplatky připojení pro odesílání událostí pomocí protokolu HTTP, bez ohledu na počet hello odesílání systémy nebo zařízení. Příjem událostí s protokolem HTTP pomocí vypršení časového limitu větší než nula, které se někdy označuje jako "dlouhé dotazování," generuje náklady zprostředkované připojení. Připojení AMQP generovat náklady zprostředkované připojení bez ohledu na to, jestli hello připojení probíhá použité toosend nebo přijímat. Všimněte si, že jsou povoleny 100 zprostředkovaná připojení zdarma v základní obor názvů. Toto je také hello maximální počet povolených pro hello předplatného Azure zprostředkované připojení. Hello prvních 1000 zprostředkovaná připojení mezi všechny standardní obory názvů v předplatné Azure jsou zahrnuty bez dalších poplatků (kromě hello základní zdarma). Protože jsou tyto příspěvky dostatek toocover mnoho service-to-service scénáře zasílání zpráv, obvykle náklady zprostředkované připojení stane pouze relevantní, pokud máte v plánu toouse AMQP nebo HTTP dlouho dotazování s velkým počtem klientů. například tooachieve efektivnější událost streamování nebo povolit obousměrnou komunikaci s mnoha zařízení nebo instancí aplikace.

## <a name="next-steps"></a>Další kroky
* Úplné podrobnosti o cenách služby Service Bus, najdete v části hello [Service Bus stránce s cenami](https://azure.microsoft.com/pricing/details/service-bus/).
* V tématu hello [nejčastější dotazy týkající se Service Bus](service-bus-faq.md#pricing) pro některé běžné nejčastější dotazy o službě Service bus ceny a fakturace.

[Azure portal]: https://portal.azure.com
