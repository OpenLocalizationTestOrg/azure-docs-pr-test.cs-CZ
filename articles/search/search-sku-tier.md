---
title: "aaaChoose SKU nebo cenovou úroveň pro službu Azure Search | Microsoft Docs"
description: "Vyhledávání systému Azure se dá zřídit na tyto identifikátory SKU: volné, Basic a Standard, kde Standard je k dispozici v různých konfigurace prostředků a kapacity úrovně."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.openlocfilehash: eac9a09266db9da4900766808be3bc3b3ff11519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Zvolte SKU nebo cenovou úroveň pro službu Azure Search.
Ve službě Azure Search [je služba zřízena](search-create-service-portal.md) na konkrétní cenová úroveň nebo SKU. Mezi možnosti patří **volné**, **základní**, nebo **standardní**, kde **standardní** je k dispozici v několika konfigurace a kapacity.

účelem Hello tohoto článku je toohelp zvolte úroveň. Pokud kapacitu vrstvy jím toobe příliš nízké, bude potřebovat tooprovision novou službu na vyšší úroveň hello a potom ho znovu načtěte vaší indexy. Neexistuje žádné místní upgrade hello stejné služby z jednoho tooanother SKU.

> [!NOTE]
> Po zvolte úroveň a [zřídit službu vyhledávání](search-create-service-portal.md), můžete zvýšit repliky a počty oddílu v rámci služby hello. Pokyny najdete v tématu [škálovat prostředek úrovně pro dotaz a indexování úlohy](search-capacity-planning.md).
>
>

## <a name="how-tooapproach-a-pricing-tier-decision"></a>Jak tooapproach cenovou úroveň rozhodnutí
Ve službě Azure Search úroveň hello Určuje kapacitu, není dostupnosti funkce. Obecně platí funkce jsou dostupné v každé vrstvě, včetně funkce verze preview. Jedinou výjimkou Hello je žádná podpora pro indexery ve vysokém rozlišení S3.

> [!TIP]
> Doporučujeme vám, že vždy zřizovat **volné** služby (jeden na předplatné s bez časového omezení) tak, aby jeho snadno dostupné pro šedé – projekty. Použití hello **volné** služba pro testování a vyhodnocení; vytvoření druhého fakturovatelný služby v hello **základní** nebo **standardní** vrstva pro produkci nebo větší testovací úlohy.
>
>

Kapacita a nákladů na běžící služba hello přejděte ruční v dolním. Informace v tomto článku vám může pomoct rozhodnout, který SKU dodává hello rovnováhu, ale pro některý z jeho toobe užitečný, potřebujete minimálně hrubý odhady na hello následující:

* Počet a velikost indexů plánujete toocreate
* Počet a velikost tooupload dokumenty
* Přehled o dotazu svazku, z hlediska dotazy na druhý (QPS)

Počet a velikost jsou důležité, protože maximální limitu prostřednictvím pevný limit na hello počet indexů nebo dokumenty ve službě nebo na prostředky (úložiště nebo repliky), který používá služba hello. Hello skutečné limit služby je podle toho, co získá vyčerpáte první: prostředky nebo objekty.

Odhady v dolním by měl hello proces zjednodušit hello následující kroky:

* **Krok 1** zkontrolujte hello SKU popisy níže toolearn o dostupných možností.
* **Krok 2** odpovědi na otázky hello níže tooarrive v předběžné rozhodnutí.
* **Krok 3** dokončit vaše rozhodnutí Kontrola pevných limitů na úložiště a ceny.

## <a name="sku-descriptions"></a>Popisy SKU
Hello následující tabulka obsahuje popis každého vrstvy.

| Úroveň | Primární scénáře |
| --- | --- |
| **Volné** |Sdílené služby, bez poplatků, používat pro vyhodnocení, šetření nebo malé úlohy. Vzhledem k tomu, že jsou sdílena s další odběratele, propustnost dotazu a indexování se liší podle uživatele, kteří je pomocí služby hello. Kapacita je malý (50 MB nebo 3 indexy s až 10 000 dokumentů. každý). |
| **Basic** |Malé produkční úlohy na vyhrazeném hardwaru. Vysoce dostupný. Kapacita je too3 repliky a 1 oddílu (2 GB). |
| **S1** |Standardní 1 podporuje flexibilní kombinace oddíly (12) a repliky (12), použít pro střední produkční zatížení na vyhrazeném hardwaru. Přidělením replik v kombinacích nepodporuje maximální počet jednotek 36 fakturovatelný vyhledávání a oddíly. Na této úrovni oddíly jsou 25 GB a QPS je přibližně 15 dotazů za sekundu. |
| **S2** |Spustí standardní 2 větší úlohy v produkčním prostředí pomocí jednotek služby vyhledávání hello stejné 36 jako S1, ale s větší velikostí oddílů a repliky. Na této úrovni oddíly jsou 100 GB a QPS je o 60 dotazů za sekundu. |
| **S3** |Standardní 3 běží úměrně větší úlohy v produkčním prostředí na vyšší end systémy, v konfiguracích too12 oddíly nebo 12 repliky jednotek v části 36 vyhledávání. Na této úrovni oddíly jsou 200 GB a QPS je více než 60 dotazů za sekundu. |
| **S3 HD** |Standardní 3 hustotou je určená pro velký počet menší indexy. Může obsahovat maximálně too3 oddílů, na 200 GB. QPS je více než 60 dotazů za sekundu. |

> [!NOTE]
> Maximální hodnoty repliky a oddíl se účtují se jako vyhledávání jednotek (36 maximální pro službu), které ukládá nižší limit efektivní než jaké hello maximální znamená v hodnotě řez. Například toouse hello maximálně 12 repliky, může mít nejvýše 3 oddíly (12 * 3 = 36 jednotek). Podobně toouse maximální oddílů, snížit too3 repliky. V tématu [škálovat prostředek úrovně pro dotaz a indexování úlohy ve službě Azure Search](search-capacity-planning.md) pro graf na povolené kombinace.
>
>

## <a name="review-limits-per-tier"></a>Zkontrolujte omezení na vrstvě
Hello následující graf je podmnožinou omezení hello z [omezení služby ve službě Azure Search](search-limits-quotas-capacity.md). Zobrazí se seznam hello faktory nejpravděpodobnější tooimpact SKU rozhodnutí. Při revizi níže uvedené otázky hello najdete toothis grafu.

| Prostředek | Free | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Smlouva SLA |Ne <sup>1</sup> |Ano |Ano |Ano |Ano |Ano |
| Omezení indexu |3 |5 |50 |200 |200 |1000 <sup>2</sup> |
| Omezení dokumentů vztahuje |10 000 celkem |1 milion pro službu |15 milionů na oddíly |60 milionů na oddíly |120 milionů na oddíly |1 milion na index |
| Maximální oddíly |Není k dispozici |1 |12 |12 |12 |3 <sup>2</sup> |
| Velikost oddílu |Celkový počet 50 MB |2 GB pro službu |25 GB na oddíly |100 GB na oddíl (až tooa maximální 1.2 TB pro službu) |200 GB na oddíl (až maximální tooa 2,4 TB pro službu) |200 GB (až tooa maximální 600 GB pro službu) |
| Maximální repliky |Není k dispozici |3 |12 |12 |12 |12 |
| Dotazy na za sekundu |Není k dispozici |Přibližně 3 na repliku |Přibližně 15 na repliku |Přibližně 60 na repliku |Více než 60 na repliku |Více než 60 na repliku |

<sup>1</sup> volné vrstvy a preview funkce není součástí smlouvy o úrovni služeb (SLA). Pro všechny fakturovatelné úrovně SLA projeví při zřizování dostatečná redundance pro vaši službu. Dvě nebo více replik jsou požadovány pro SLA dotazu (načíst). Tři nebo více replik jsou požadovány pro dotazy a indexování smlouvy o úrovni služeb (pro čtení a zápis). Hello počet oddílů není k posouzení SLA. 

<sup>2</sup> S3 a S3 HD jsou zajišťované identické vysoké kapacity infrastruktury ale každé z nich dosáhne své maximální limit různými způsoby. S3 cílí menší počet velké indexy. Jako takový jeho maximální limit je vázán na prostředků (pro každou službu 2.4 TB). S3 HD cílí velký počet velmi malé indexy. Na 1000 indexy S3 HD dosaženo omezení v podobě hello indexu omezení. Pokud jste zákazník S3 HD, který vyžaduje více než 1 000 indexy, obraťte se na Microsoft Support informace o tom tooproceed.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Odstranění položek SKU, které nesplňují požadavky
Hello následující otázky vám může pomoct přicházejí na hello správné rozhodnutí SKU pro úlohy.

1. Máte **smlouvy o úrovni služeb (SLA)** požadavky? Můžete použít libovolné fakturovatelný úrovně (základní na nahoru), ale je nutné nakonfigurovat služby pro redundanci. Dvě nebo více replik jsou požadovány pro SLA dotazu (načíst). Tři nebo více replik jsou požadovány pro dotazy a indexování smlouvy o úrovni služeb (pro čtení a zápis). Hello počet oddílů není k posouzení SLA.
2. **Kolik indexuje** chcete? Jeden z největších proměnné hello řešení do rozhodnutí SKU je hello počet indexů nepodporuje každý SKU. Podpora index je výrazně liší úrovni v hello nižší cenové úrovně. Požadavky na počet indexů může být primární určující SKU rozhodnutí.
3. **Kolik dokumenty** bude načten do každé indexu? hello počet a velikost dokumenty určí hello případnou velikost hello index. Za předpokladu, že můžete odhadnout hello předpokládané velikost hello index, můžete porovnat počet pro velikost oddílu hello za SKU, prodloužena hello počet požadovaných toostore oddílů indexu této velikosti.
4. **Co je zatížení očekávané dotazu hello**? Jakmile jsou požadavky na úložiště pochopeny, vezměte v úvahu dotazu úlohy. S2 a obě položky S3 nabízejí téměř ekvivalentní propustnost, ale bude požadavků SLA vyloučit všechny preview SKU.
5. Pokud uvažujete o hello S2 nebo S3 vrstvy, zjistěte, zda chcete, aby [indexery](search-indexer-overview.md). Indexery ještě nejsou k dispozici pro vrstvu S3 HD hello. Alternativní způsob je toouse model push aktualizací index, kde můžete psát kód toopush aplikace indexem tooan datové sady.

Většina zákazníků může pravidlo konkrétní SKU příchozí nebo odchozí podle jejich odpovědi toohello výše otázky. Pokud si pořád nejste jistí které SKU toogo s, můžete tooMSDN otázky nebo StackOverflow fóra, nebo požádejte podporu Azure o další pokyny.

## <a name="decision-validation-does-hello-sku-offer-sufficient-storage-and-qps"></a>Decision ověření: hello SKU nabízí systém dostatečné úložiště a QPS?
Jako poslední krok, pokroku hello [stránce s cenami](https://azure.microsoft.com/pricing/details/search/) a hello [částech-service a za index v omezení služby](search-limits-quotas-capacity.md) toodouble kontrola odhady proti limitů předplatného a služby.

Pokud požadavky na úložiště nebo cena buď hello jsou mimo rozsah, můžete chtít toorefactor hello zatížení mezi více menší služeb (například). Na podrobnější úrovni se může změnit návrh indexy toobe menší nebo použít filtry toomake dotazy efektivnější.

> [!NOTE]
> Požadavky na úložiště může být přepsání zvýšeným, pokud dokumenty obsahují nadbytečné data. V ideálním případě dokumenty obsahují pouze s možností vyhledávání dat nebo metadat. Binární data je jiný prohledávat a by měly být uložené samostatně (třeba v tabulce nebo objekt blob úložiště Azure) s pole v indexu toohold hello dat externí adresu URL odkazu toohello. maximální velikost jednotlivých dokumentu Hello je 16 MB (nebo méně Pokud jste hromadné odesílání více dokumentů v jedné žádosti). V tématu [omezení ve službě Azure Search služby](search-limits-quotas-capacity.md) Další informace.
>
>

## <a name="next-step"></a>Další krok
Jakmile již víte, což SKU je hello právo přizpůsobit a pokračujte v těchto krocích:

* [Vytvořte službu vyhledávání v portálu hello](search-create-service-portal.md)
* [Změnit hello přidělení replik a oddíly tooscale služby](search-capacity-planning.md)
