---
title: "aaaModeling víceklientskou architekturu, ve službě Azure Search | Microsoft Docs"
description: "Další informace o běžných návrhová schémata pro víceklientské aplikace SaaS při používání služby Azure Search."
services: search
manager: jhubbard
author: ashmaka
documentationcenter: 
ms.assetid: 72e9696a-553b-47dc-9e05-a82db0ebf094
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2016
ms.author: ashmaka
ms.openlocfilehash: dd46cda772d32566b9aaa18d407f12fdf178bd43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Vzory pro víceklientské aplikace SaaS a Azure Search návrhu
Víceklientské aplikace je ten, který poskytuje hello stejný služeb a možnosti tooany počet klientů, kteří se nedají zobrazit ani sdílet hello data žádným jiným klientem. Tento dokument popisuje strategie izolace klienta pro víceklientské aplikace sestavené s Azure Search.

## <a name="azure-search-concepts"></a>Koncepty Azure Search
Jako řešení vyhledávání jako služby Azure Search umožňuje vývojářům tooadd bohaté vyhledávání vyskytne tooapplications bez správou jakékoli infrastruktury nebo stal odborník v hledání. Data je odeslán toohello služby a pak uloženy v cloudu hello. Pomocí jednoduchých požadavků toohello rozhraní API služby Azure Search, hello data pak se dají upravit a vyhledávat. Přehled služby hello lze nalézt v [v tomto článku](http://aka.ms/whatisazsearch). Před hovoříte o vzory návrhu, je důležité toounderstand některé pojmy v Azure Search.

### <a name="search-services-indexes-fields-and-documents"></a>Vyhledávací služby, indexy, pole a dokumentů
Při používání služby Azure Search, jeden odběratel tooa *služby vyhledávání*. Data je nahrané tooAzure vyhledávání, uloží se do *index* v rámci služby vyhledávání hello. Může být počet indexů v rámci jedné služby. Koncepty seznámit toouse hello databází, hello vyhledávací služba může být připodobnit tooa databáze, hello indexy v rámci služby mohou být připodobnit tootables v databázi.

Každý index v rámci služby vyhledávání má svou vlastní schéma, který je definovaný počet přizpůsobitelné *pole*. Přidávání dat indexu Azure Search tooan hello tvar individuální *dokumenty*. Každý dokument musí být nahrané tooa konkrétním indexem a schéma této indexu se musí vejít. Při hledání dat pomocí Azure Search, jsou dotazy fulltextové vyhledávání hello vydaný pro konkrétním indexem.  toocompare tyto koncepty toothose databáze, pole můžou být připodobnit toocolumns v tabulce a dokumenty můžou být připodobnit toorows.

### <a name="scalability"></a>Škálovatelnost
Všechny služby Azure Search v hello Standard [cenová úroveň](https://azure.microsoft.com/pricing/details/search/) můžete škálovat v dvěma rozměry: úložiště a dostupnost.

* *Oddíly* přidáním tooincrease hello úložiště vyhledávací službu.
* *Repliky* přidáním tooa služby tooincrease hello propustnost žádostí, které může zpracovat vyhledávací službu.

Přidávání a odebírání oddílů a repliky ve vám umožní hello kapacitu hello vyhledávání služby toogrow hello množství dat a přenosy dat požadavky aplikace hello. V pořadí pro tooachieve služby hledání čtení [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), vyžaduje dvě repliky. V pořadí pro službu tooachieve pro čtení a zápis [SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), vyžaduje tři repliky.

### <a name="service-and-index-limits-in-azure-search"></a>Limity služby a index ve službě Azure Search
Existuje několik různých [cenové úrovně](https://azure.microsoft.com/pricing/details/search/) ve službě Azure Search, každý z úrovně hello má jiný [omezení a kvóty](search-limits-quotas-capacity.md). Některé z těchto omezení jsou v hello úrovně služeb, některé jsou na úrovni hello index a některé jsou na úrovni oddílu hello.

|  | Basic | Standard1 | Standard2 | Standard3 | Standard3 HD |
| --- | --- | --- | --- | --- | --- |
| Maximální replik pro službu |3 |12 |12 |12 |12 |
| Maximální oddílů pro službu |1 |12 |12 |12 |1 |
| Maximální vyhledávání jednotky (repliky * oddíly) pro službu |3 |36 |36 |36 |36 (max 3 oddíly) |
| Maximální dokumenty pro službu |1 milion |180 milionů |720 milionů |1.4 miliardy |600 milionů |
| Maximální velikost úložiště pro službu |2 GB |300 GB |1,2 TB |2,4 TB |600 GB |
| Maximální dokumenty na oddíly |1 milion |15 milionů |60 milionů |120 milionů |200 milionů |
| Maximální velikost úložiště na jeden oddíl |2 GB |25 GB |100 GB |200 GB |200 GB |
| Maximální indexů pro službu |5 |50 |200 |200 |3000 (max 1000 indexy nebo oddíl) |

#### <a name="s3-high-density"></a>S3 s vysokou hustotou.
Ve službě Azure Search S3 cenovou úroveň je možnost pro režim vysokou hustotou (HD) hello určený speciálně pro víceklientské scénáře. V mnoha případech je nutné toosupport velký počet klientů menší pod hello výhody tooachieve jedné služby efektivitu jednoduchost a náklady.

S3 HD umožňuje hello mnoho malých indexy toobe zabalené pod správu hello služby hledání jednoduchého obchodování tooscale možnost hello se indexy používající oddíly pro hello možnost toohost více indexů v jedné službě.

Služby S3 namítají, může mít mezi 1 a 200 indexy, které společně by mohl být hostitelem až miliardy dokumenty too1.4. S3 HD na hello by umožnilo druhé straně jednotlivé indexy tooonly přejděte too1 milionů dokumentů, ale dokáže zpracovat až too1000 indexů pro oddíl (až too3000 pro službu) se dokument celkový počet 200 milionů na oddíl (až too600 milionů za služby).

## <a name="considerations-for-multitenant-applications"></a>Důležité informace pro víceklientské aplikace
Víceklientské aplikace musí efektivní distribuci prostředků mezi hello klientům při zachování některé úrovně ochrany osobních údajů mezi hello různé klienty. Existuje několik důležité informace při návrhu hello architektura pro takové aplikace:

* *Izolaci klientů:* vývojáři aplikace potřebují tootake příslušná opatření tooensure, že máte žádné klienty Neautorizováno nebo nežádoucí přístup k datům toohello ostatních klientů. Nad rámec hello perspektivy ochrany osobních údajů vyžadují strategie izolace klienta efektivní správu sdílených prostředků a ochranu před aktivní okolí.
* *Náklady na prostředek v cloudu:* jako s žádnou jinou aplikaci, musí zůstat řešení softwaru náklady konkurenční jako součást víceklientské aplikace.
* *Snadné operací:* při vývoji víceklientské architektury, hello dopad na operace aplikace hello a složitost je důležitý faktor. Má služba Azure Search [SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* *Globální nároky:* víceklientské aplikace může být nutné tooeffectively obsluhovat klienty, které jsou rozmístěny v celém světě hello.
* *Škálovatelnost:* vývojáři aplikace potřebují tooconsider, jak se odsouhlasení mezi zachování dostatečně nízkou úroveň aplikace složitost a návrhu tooscale aplikace hello s počtem klientů a hello velikost dat klientů a zatížení.

Služba Azure Search nabízí několik hranice, které mohou být data a úlohy použité tooisolate klientů.

## <a name="modeling-multitenancy-with-azure-search"></a>Modelování víceklientskou architekturu s Azure Search
V případě hello víceklientské scénáře vývojář aplikace hello využívá jednu nebo více služeb vyhledávání a dělit svým klientům mezi služby, indexy nebo obojí. Vyhledávání systému Azure má několik běžných vzorů při modelování víceklientské scénář:

1. *Index každého klienta:* každý klient má vlastní index v rámci služby vyhledávání, která je sdílena s jinými klienty.
2. *Služba každého klienta:* má každý klient svůj vlastní vyhrazený služby Azure Search nabízí nejvyšší úroveň oddělení dat a zatížení.
3. *Kombinace obou:* větší, další aktivní klienty přiřazené vyhrazené služby, zatímco menší klienty přiřazené jednotlivé indexy v rámci sdílených služeb.

## <a name="1-index-per-tenant"></a>1. Index každého klienta
![Portrétu hello index za klienta modelu](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

Ve model index za klienta více klientů zabírají jedné služby Azure Search, kde každý klient má své vlastní index.

Klienti dosáhnout izolace dat, protože všechny vyhledávání, požadavky a operace dokumentu vydávají na úrovni indexu ve službě Azure Search. V hello aplikační vrstvu je sledování nutné hello toodirect hello různé klienty provoz toohello správné indexy při také správu prostředků na úrovni služby hello mezi všechny klienty.

Klíčový atribut hello index za klienta modelu je hello možnost pro vývojáře aplikací hello toooversubscribe hello kapacitu vyhledávací službu mezi klienty aplikace hello. Pokud hello klientům nerovnoměrné distribuci zatížení, hello optimální kombinaci klientů může být distribuovány na službu vyhledávání indexuje tooaccommodate číslo vysoce aktivní, náročná klientů a současně současné obsluhování dlouho konec míň aktivní klienti. kompromis Hello je hello nemohou hello modelu toohandle situacích, kde je každý klient souběžně vysoce aktivní.

model index za klienta Hello poskytuje hello základ pro model náklady na proměnnou, kde je zakoupili počáteční celou službu Azure Search a následně vyplněno s klienty. To umožňuje toobe nevyužité kapacity, které jsou určené pro zkušební verze a bezplatné účty.

Pro aplikace s globální nároky nemusí být hello index za klienta modelu hello maximální efektivitou. Pokud klienti aplikace jsou rozmístěny v celém světě hello, samostatná služba může být nutné pro každou oblast, která může duplikujte náklady na každý z nich.

Služba Azure Search umožňuje škálování hello hello jednotlivé indexy a celkový počet indexů toogrow hello. Pokud příslušné ceny je zvoleno vrstvy, oddíly a repliky můžete přidat službu celý vyhledávání toohello při jednotlivých index v rámci služby hello příliš naroste z hlediska úložiště nebo provoz.

Pokud pro jeden službu příliš naroste hello celkový počet indexů, má jinou službu toobe zřízený tooaccommodate hello nové klienty. Pokud indexy toobe přesouvat mezi vyhledávací služby, když jsou přidávány nové služby, hello data z indexu hello mají toobe ručně zkopírovat z jeden index toohello jiných jako Azure Search nepovoluje indexu toobe přesunout.

## <a name="2-service-per-tenant"></a>2. Služba každého klienta
![Portrétu hello na klienta služby modelu](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

V architektuře služby za klienta má každý klient vlastní službu vyhledávání.

V tomto modelu aplikace hello dosahuje hello maximální úroveň izolace pro své klienty. Každá služba má vyhrazený úložiště a propustnost pro zpracování požadavku hledání a také různé klíče rozhraní API.

Pro aplikace, kde každý klient má velké nároky nebo zatížení hello variabilitu z klienta tootenant, hello na klienta služby model je účinné volba prostředky nejsou sdílená mezi různými klienty úlohy.

Služby za klienta modelu také nabízí hello výhodou model předvídatelný, pevné náklady. Neexistuje žádné počáteční investici v službě celý vyhledávání dokud nebude toofill klienta, ale hello náklady za klienta je vyšší než model index za klienta.

model na klienta služby Hello je pro aplikace s globální nároky na efektivní volbu. S mnoha místech pracujícími klienty, je snadné toohave, služba každého klienta v příslušné oblasti hello.

Hello problémů v škálování tento vzor vynoří během jednotlivých klientů odrůst své služby. Vyhledávání systému Azure aktuálně nepodporuje upgrade hello cenová úroveň služby vyhledávání, takže všechna data by mít toobe ručně zkopírované tooa novou službu.

## <a name="3-mixing-both-models"></a>3. Kombinací obou modelů
Jiné vzor pro modelování víceklientská je kombinací strategie index za klienta a služby za klienta.

Pomocí kombinace hello dva vzory, zabírat největší klienty aplikace vyhrazené služby při hello dlouho značka z méně aktivní, menší klienty zabírat indexy ve sdílených služeb. Tento model zajistí, že klienti největší hello konzistentně vysoký výkon ze služby hello zároveň pomáhá tooprotect hello menší klienty z žádné aktivní okolí.

Implementace této strategie však využívá předvídání předpovídat, které klienti budou vyžadovat vyhrazené service versus indexu sdílené služby. Složitost aplikace se zvyšuje s hello nutné toomanage obou těchto modelů, víceklientské architektury.

## <a name="achieving-even-finer-granularity"></a>Dosažení i jemnějšího členitosti
Hello výše toomodel vzory návrhu ve službě Azure Search víceklientské scénáře. Předpokládejme uniform oboru, kde je každý klient celou instanci aplikace. Aplikace však může zpracovávat někdy mnoho menší obory.

Pokud na klienta služby a index za klienta modely nejsou dostatečně malé obory, je možné toomodel indexu tooachieve i podrobnějšího členitosti.

toohave jeden index chovat jinak pro koncové body jiného klienta, přidané tooan index, který vytváří určitou hodnotu u jednotlivých možných klientů může být pole. Pokaždé, když je klient zavolá Azure Search tooquery nebo upravit index, hello kód z klientské aplikace hello určuje hello odpovídající hodnotu pro toto pole používání služby Azure Search [filtru](https://msdn.microsoft.com/library/azure/dn798921.aspx) schopností v době dotazu.

Tato metoda může být funkce použité tooachieve samostatné uživatelské účty, úrovně samostatné oprávnění a i zcela samostatné aplikace.

> [!NOTE]
> Hello přístup popsané výše tooconfigure jeden index tooserve více klientů ovlivňuje hello relevance výsledků vyhledávání. Skóre vyhledávání významu se vypočítávají v indexu úrovni oboru, není obor úrovni klienta, aby všichni klienti dat je obsažena v hello relevance skóre základní statistické údaje, třeba termín frekvence.
> 
> 

## <a name="next-steps"></a>Další kroky
Služba Azure Search je poutavé volbou pro mnoho aplikací [Další informace o robustní možnosti hello služby](http://aka.ms/whatisazsearch). Při vyhodnocování hello různé vzory pro víceklientské aplikace návrhu, zvažte hello [různých cenových úrovní](https://azure.microsoft.com/pricing/details/search/) a příslušných hello [omezení služby](search-limits-quotas-capacity.md) toobest přizpůsobit toofit Azure Search úlohy aplikací a architektury všech velikostí.

Jakékoli dotazy týkající se Azure Search a víceklientské scénáře může přesměrovat tooazuresearch_contact@microsoft.com.

