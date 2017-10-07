---
title: "aaaAzure – nejčastější dotazy DNS | Microsoft Docs"
description: "Časté otázky k Azure DNS"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: jonatul
ms.openlocfilehash: 55006e9d8b311f1e94678eb9d35ce3448b936488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-faq"></a>Nejčastější dotazy k Azure DNS

## <a name="about-azure-dns"></a>O Azure DNS

### <a name="what-is-azure-dns"></a>Co je Azure DNS?

Hello systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) k webu nebo službě název tooits IP adresu. Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure. Hostování domény do Azure, můžete spravovat DNS záznamů pomocí hello stejné přihlašovací údaje, rozhraní API, nástroje a fakturace jako jinými službami Azure.

DNS domén v Azure DNS jsou hostované v Azure globální síti názvových serverů DNS. Používáme všesměrového vysílání sítě tak, aby každý dotaz DNS je zodpovězen pomocí serveru DNS nejbližší dostupné hello. To poskytuje vysoký výkon a vysokou dostupnost vaší domény.

Hello služba Azure DNS je založena na Azure Resource Manager. Jako takový výhody z funkce služby Správce prostředků například řízení přístupu na základě rolí, protokoly auditu a uzamčení prostředků. Doménách a záznamech můžete spravovat prostřednictvím hello portál Azure, rutin prostředí Azure PowerShell a hello rozhraní příkazového řádku Azure napříč platformami. Aplikace, které potřebují Automatická správa DNS může integrovat hello service pomocí hello REST API a sady SDK.

### <a name="how-much-does-azure-dns-cost"></a>Kolik Azure DNS stojí?

model fakturace Hello Azure DNS je na základě hello počtu zónách DNS hostovaných v Azure DNS a hello počet dotazů DNS, který obdrží. Slevy jsou poskytovány na základě využití.

Další informace najdete v tématu hello [Azure DNS stránce s cenami](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-hello-sla-for-azure-dns"></a>Co je hello SLA pro Azure DNS?

Nemůžeme zaručit, že platné požadavky DNS obdrží odpověď z alespoň jeden server název Azure DNS alespoň 99,99 % času hello.

Další informace najdete v tématu hello [stránku smlouvy SLA pro Azure DNS](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-hello-same-as-a-dns-domain"></a>Novinky DNS zóny Je hello stejná jako doména DNS? 

Doména je jedinečný název v hello domain name systemu, například "contoso.com".

Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény. Například hello doménu "contoso.com" může obsahovat několik záznamů DNS, třeba "mail.contoso.com" (pro poštovní server) a "www.contoso.com" (pro webový server). To by byly umístěny v zóně DNS hello "contoso.com".

Název domény je *pouze název*, zatímco zóny DNS je zdroj dat obsahující hello záznamy DNS pro název domény. Azure DNS vám umožní toohost zóny DNS a spravovat hello záznamy DNS pro doménu v Azure. Poskytuje také názvových serverů DNS dotazy DNS tooanswer z hello Internetu.

### <a name="do-i-need-toopurchase-a-dns-domain-name-toouse-azure-dns"></a>Je nutné toopurchase toouse název domény DNS Azure DNS? 

Ne nutně.

Není nutné toopurchase domény toohost zónu DNS v Azure DNS. Můžete vytvořit zónu DNS kdykoli bez vlastnící hello název domény. Dotazy DNS pro tuto zónu pouze vyřeší, pokud jsou směrované názvových serverů Azure DNS toohello přidělena toohello zóny.

Je třeba název domény toopurchase hello, pokud chcete zónu DNS, toolink do hello globální DNS hierarchie – tento umožňuje DNS dotazy z kdekoli v hello world toofind zóny DNS a odpověď, v níž svoje záznamy DNS.

## <a name="azure-dns-features"></a>Funkce Azure DNS

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Podporuje Azure DNS na základě DNS provoz směrování nebo koncový bod převzetí služeb při selhání?

Na základě DNS provoz směrování a koncového bodu převzetí služeb při selhání jsou k dispozici pomocí Azure Traffic Manager. Toto je samostatná služba Azure, který lze použít společně s Azure DNS. Další informace najdete v tématu hello [Traffic Manager Přehled](../traffic-manager/traffic-manager-overview.md).

Azure DNS podporuje pouze hostování "statická" domény DNS, kde každý dotaz DNS pro daný záznam DNS vždycky obdržel hello stejné odpověď DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>Podporuje Azure DNS registrace názvu domény?

Ne. Azure DNS aktuálně nepodporuje nákup názvů domén. Pokud chcete, aby toopurchase domény, je třeba toouse doménového registrátora názvu domény třetí strany. Hello Registrátor obvykle účtuje malý roční poplatek. pro správu záznamů DNS, může být Hello domény pak hostovaný v Azure DNS. V tématu [delegovat tooAzure domény DNS](dns-domain-delegation.md) podrobnosti.

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů příliš[zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Podporuje Azure DNS "privátní" domény?

Ne. Azure DNS aktuálně podporuje jenom internetové domény.

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů příliš[zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/10737696-enable-split-dns-for-providing-both-public-and-int).

Informace o interní možnosti služby DNS v Azure najdete v tématu [překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Podporuje Azure DNS DNSSEC?

Ne. Azure DNS v současné době nepodporuje DNSSEC.

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů příliš[zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Podporuje Azure DNS přenosy zóny (AXFR/IXFR)?

Ne. Azure DNS v současné době nepodporuje přenosy zóny. Zóny DNS může být [importovat do Azure DNS pomocí rozhraní příkazového řádku Azure hello](dns-import-export.md). Záznamy DNS potom je můžete spravovat prostřednictvím hello [portálu pro správu Azure DNS](dns-operations-recordsets-portal.md), naše [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [rutiny prostředí PowerShell](dns-operations-recordsets.md), nebo [ Nástroj příkazového řádku](dns-operations-recordsets-cli.md).

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů příliš[zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Podporuje Azure DNS adresy URL přesměrování?

Ne. Adresa URL přesměrování služby nejsou ve skutečnosti služba DNS – pracují na úrovni hello HTTP, nikoli hello DNS úroveň. Některé toobundle zprostředkovatelé DNS adresy URL přesměrování služby v rámci jejich celkový nabídky. To není podporováno aktuálně Azure DNS.

Tato funkce je sledovaný na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů příliš[zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Pomocí Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Můžu společné hostování domény pomocí Azure DNS a jiného poskytovatele DNS?

Ano. Azure DNS podporuje společné hosting domén s jinými službami DNS.

toodo tedy potřebujete záznamy hello NS toomodify pro doménu hello (který ovládací prvek poskytovatelů, kteří obdrží DNS dotazuje pro doménu hello) toopoint toohello názvové servery obou zprostředkovatelů. Tyto záznamy NS potřebovat toobe upravit v 3 míst: v Azure DNS v hello další zprostředkovatele a v nadřazené zóně hello (obvykle konfiguruje prostřednictvím hello registrátorem názvu domény). Další informace o delegování DNS najdete v tématu [delegování DNS domény](dns-domain-delegation.md).

Kromě toho musíte tooensure, že jsou hello záznamy DNS pro doménu hello synchronizace mezi i poskytovatelé služby DNS. Azure DNS v současné době nepodporuje přenosy zóny DNS. Je třeba toosynchronize záznamů DNS pomocí buď hello [portálu pro správu Azure DNS](dns-operations-recordsets-portal.md), naše [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [rutiny prostředí PowerShell](dns-operations-recordsets.md) , nebo [rozhraní příkazového řádku nástroje](dns-operations-recordsets-cli.md).

### <a name="do-i-have-toodelegate-my-domain-tooall-4-azure-dns-name-servers"></a>Je nutné provést toodelegate názvových serverů Azure DNS tooall 4 Moje doména?

Ano. Azure DNS přiřadí 4 názvové servery zóny DNS tooeach selhání izolace a vyšší odolnosti. tooqualify pro hello smlouvy SLA pro Azure DNS, musíte toodelegate vaší domény tooall 4 názvové servery.

### <a name="what-are-hello-usage-limits-for-azure-dns"></a>Jaké jsou limity hello využití pro Azure DNS?

Hello následující výchozí omezení platí při použití Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Můžete přesunout zóny DNS, mezi skupinami prostředků nebo mezi předplatnými?

Ano. Zóny DNS lze přesouvat mezi skupinami prostředků, nebo mezi předplatnými.

Není žádný vliv na dotazy DNS, při přesouvání zóny DNS. Hello názvové servery přiřazené toohello zóny zůstat stejné a dotazy DNS jsou zpracovávány jako normální v rámci hello.

Další informace a pokyny, jak toomove zóny DNS, najdete v části [přesunout prostředky tooa novou skupinu prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-tootake-effect"></a>Jak dlouho trvá vliv tootake změny DNS?

Nové zóny DNS a záznamy DNS jsou obvykle projeví ve názvových serverů Azure DNS hello rychle během několika sekund.

Záznamy DNS tooexisting změny může trvat delší dobu, ale měli stále projeví na názvových serverů Azure DNS hello během 60 sekund. V takovém případě DNS ukládání do mezipaměti mimo Azure DNS (podle rekurzivní překladače služby DNS a klienty DNS) mohou ovlivnit hello doba změnu toobe DNS efektivní. Tato doba trvání mezipaměti se dá řídit pomocí hello Time To Live (TTL) vlastnost každá sada záznamů.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Jak lze chránit Moje zóny DNS před náhodným odstraněním?

Azure DNS je spravován pomocí Azure Resource Manager a výhody z hello přístup k řízení funkcí správce Azure Resource Manager poskytuje. Řízení přístupu na základě rolí lze použít toocontrol uživatelů, kteří mají pro čtení nebo zápis tooDNS zóny a sady záznamů. Uzamčení prostředků může být použité tooprevent náhodnému úpravy nebo odstranění zóny DNS a sady záznamů.

Další informace najdete v tématu [Protecting zóny DNS a záznamy](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Jak nastavím záznamy SPF v Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Jak lze nastavit až mezinárodní IDN název domény () v Azure DNS?

Mezinárodní názvy domén (IDN) fungovat podle kódování, každý využívající název DNS se[punycode](https://en.wikipedia.org/wiki/Punycode)'. Dotazy DNS jsou vytvářeny pomocí názvy těchto kódováním punycode.

Můžete nakonfigurovat mezinárodní názvy domén (IDN) v Azure DNS první převodu název zóny hello nebo toopunycode název sady záznamů. Azure DNS v současné době nepodporuje integrovanou převod z punycode.

## <a name="next-steps"></a>Další kroky

[Další informace o službě Azure DNS](dns-overview.md)
<br>
[Další informace o zóny DNS a záznamů](dns-zones-records.md)
<br>
[Začínáme s Azure DNS](dns-getstarted-portal.md)

