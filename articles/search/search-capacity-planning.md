---
title: "aaaCapacity plánování pro službu Azure Search | Microsoft Docs"
description: "Upravte oddíl a repliky prostředky počítače ve službě Azure Search, kde je každý prostředek za cenu v jednotkách fakturovatelný vyhledávání."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 1dc16afe-56f9-439d-8874-1733ae1a2b74
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 02/08/2017
ms.author: heidist
ms.openlocfilehash: 4bbbb929a36b932ea7af12e494ca095d98b9005e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-resource-levels-for-query-and-indexing-workloads-in-azure-search"></a>Škálování prostředku úrovně pro dotaz a indexování úlohy ve službě Azure Search
Po jste [zvolte cenovou úroveň](search-sku-tier.md) a [zřídit službu vyhledávání](search-create-service-portal.md), hello dalším krokem je toooptionally zvýšení hello počet replik nebo oddíly, které používá vaše služba. Každá úroveň nabízí pevný počet fakturace jednotky. Tento článek vysvětluje, jak tooallocate tyto jednotky tooachieve optimální konfiguraci, který vyrovnává vaše požadavky na spuštění dotazu, indexování a úložiště.

Konfigurace prostředků je k dispozici při nastavování služby v hello [úroveň Basic](http://aka.ms/azuresearchbasic) nebo jeden z hello [standardní úrovně](search-limits-quotas-capacity.md). Pro fakturovatelný služby na tyto úrovně je kapacity dokupovat v jednotkách po *jednotek vyhledávání* (SUs) kde každý oddíl a repliky počítá jako jedna SU. 

Použití méně služby SUs výsledky v úměrně nižší faktury. Fakturace pro platí, dokud je nastavený hello služby. Pokud nepoužíváte dočasně služby, hello jedině tooavoid fakturace je tak, že odstraněním hello služby a pak vytvoříte ji znovu v případě potřeby.

> [!Note]
> Odstraněním služby odstraníte všechno, co na něm. Neexistuje žádné zařízení v rámci Azure Search pro zálohování a obnovení trvalá data vyhledávání. tooredeploy existujícího indexu na novou službu, doporučujeme spustit program použitý toocreate hello a načíst původně. 

## <a name="terminology-partitions-and-replicas"></a>Terminologie: oddíly a repliky
Oddíly a repliky jsou hello primární prostředky, které zpět vyhledávací službu.

| Prostředek | Definice |
|----------|------------|
|*Oddíly* | Poskytuje index úložiště a vstupně-výstupních operací pro operace čtení a zápisu (například když znovu sestavit nebo aktualizovat index).|
|*Repliky* | Instance služby vyhledávání hello používá primárně tooload operace dotazů vyrovnávání. Každou repliku vždy hostuje jednu kopii indexu. Pokud máte 12 repliky, budete mít 12 kopie každý index načíst hello služby.|

> [!NOTE]
> Neexistuje žádný způsob toodirectly pracovat s nebo spravovat, které indexy spustit v replice. Jednu kopii každého index v každé repliky je součástí architektury služby hello.
>

## <a name="how-tooallocate-partitions-and-replicas"></a>Jak tooallocate oddíly a repliky
Ve službě Azure Search je služba původně přidělena minimální úroveň prostředků, který se skládá z jednoho oddílu a jednu repliku. Pro vrstvy, které ji podporují můžete upravit přírůstkově výpočetní prostředky zvýšením oddílů, pokud potřebujete další úložiště a vstupně-výstupních operací, nebo přidat další repliky větší svazky dotazu nebo lepší výkon. Služba jednotného musí mít dostatek prostředků toohandle všechny úlohy (indexování a dotazy). Nelze rozdělit zatížení mezi více služeb.

tooincrease nebo změňte hello přidělení repliky a oddíly, doporučujeme používat hello portálu Azure. portál Hello vynucuje omezení povolené kombinace, které zůstat nižší než maximální limit:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vyberte službu vyhledávání hello.
2. V **nastavení**, otevřete hello **škálování** okno a použijte hello posuvníky tooincrease nebo snižte počet hello oddíly a repliky.

Pokud budete potřebovat zřizování přístupu založených na skriptech nebo založené na kódu, hello [REST API pro správu](https://msdn.microsoft.com/library/azure/dn832687.aspx) je alternativní toohello portál.

Obecně platí hledání aplikace potřebují další repliky než oddíly, zvláště pokud operací služby hello jsou upřednostněno dotazu úlohy. Hello části na [vysokou dostupnost](#HA) vysvětluje, proč.

> [!NOTE]
> Po zřízení služby nemůže být upgradovaný tooa vyšší skladová položka. Budete potřebovat toocreate vyhledávací službu na novou vrstvu hello a znovu načíst vaše indexy. V tématu [vytvoření služby Azure Search na portálu hello](search-create-service-portal.md) o pomoc se zřizováním služby.
>
>

<a id="HA"></a>

## <a name="high-availability"></a>Vysoká dostupnost
Protože je snadné a relativně rychlé tooscale nahoru, doporučujeme obvykle začínáte s jedním oddílem a jednu nebo dvě repliky a pak rozšiřování škálování využívajících jako dotaz svazky sestavení. Pro mnoho služeb na úrovně Basic nebo S1 hello poskytuje jeden oddíl dostatečné úložiště a vstupně-výstupní operace (na 15 milionů dokumentů na oddíl).

Spuštěné úlohy dotazu především na replikách. Pokud potřebujete další propustnost nebo vysokou dostupnost, bude pravděpodobně vyžadovat další repliky.

Obecná doporučení pro zajištění vysoké dostupnosti jsou:

* Dvě repliky pro vysokou dostupnost úloh jen pro čtení (dotazy)
* Tři nebo více replik pro vysokou dostupnost úloh pro čtení a zápis (dotazy a indexování přidat, aktualizovat ani odstranit jednotlivé dokumenty)

Smlouvy o úrovni služeb (SLA) pro službu Azure Search cílí v operacích dotazu a aktualizace indexu, které se skládají z přidání, aktualizace nebo odstranění dokumentů.

### <a name="index-availability-during-a-rebuild"></a>Index dostupnosti při opětovném sestavení

Vysoké dostupnosti pro službu Azure Search vztahují tooqueries a index aktualizace, které nejsou zahrnují nové sestavení indexu. Pokud odstraníte pole, změnit datový typ nebo název pole, budete potřebovat toorebuild hello index. toorebuild hello index, je nutné odstranit hello index, znovu vytvořte hello index a znovu načtěte hello data.

> [!NOTE]
> Můžete přidat nového indexu Azure Search tooan pole bez znovu sestavit hello index. Hodnota Hello hello nové pole bude mít hodnotu null pro všechny dokumenty již v indexu hello.

dostupnost index toomaintain při opětovném sestavení, musí mít kopii hello index s jiným názvem na hello stejné služby, nebo kopii hello indexu s hello stejný název na jinou službu a pak zadejte přesměrování nebo převzetí služeb při selhání logiku v kódu.

## <a name="disaster-recovery"></a>Zotavení po havárii
V současné době není k dispozici žádné předdefinované mechanismus pro zotavení po havárii. Přidání oddílů nebo repliky by hello nesprávný strategie pro splnění cílů obnovení po havárii. Nejběžnější přístup Hello je tooadd redundance na úrovni služby hello nastavením druhý vyhledávací službu v jiné oblasti. Stejně jako u dostupnosti během opětovné sestavení indexu hello přesměrování nebo logika převzetí služeb při selhání musí pocházet z vašeho kódu.

## <a name="increase-query-performance-with-replicas"></a>Zvýšení výkonu dotazů s replikami
Latence dotazu je indikátorem, jsou potřeba další repliky. Prvním krokem k zlepšení výkonu dotazů, je obecně tooadd další tohoto prostředku. Při přidávání repliky, další kopie hello indexu jsou uvedena do režimu online toosupport větší dotazu úlohy a vyžaduje tooload vyrovnávání hello přes hello víc replik.

Nemůžeme poskytovat pevný odhady na dotazy za sekundu (QPS): dotaz výkon závisí na složitosti hello hello dotazu a konkurenční úlohy. V průměru repliku na základní nebo S1 SKU může obsluhovat o 15 QPS, ale vaše propustnost bude vyšší nebo nižší v závislosti na složitosti dotazu (Fasetové dotazy jsou složitější) a latence sítě. Je také důležité toorecognize, i když přidání repliky výborný přidá rozsah a výkon, hello výsledek není výhradně lineární: Přidání tři repliky nezaručuje Trojitá propustnost.

toolearn o QPS, včetně přístupy k odhadování QPS pro zatížení, najdete v části [Správa služby Search](search-manage.md).

## <a name="increase-indexing-performance-with-partitions"></a>Zvýšení výkonu indexování s oddíly
Vyhledávání aplikací, které vyžadují téměř aktualizace dat v reálném čase, bude nutné úměrně další oddíly než repliky. Přidání oddílů šíří operace čtení a zápisu napříč větší počet výpočetní prostředky. Nabízí také více místa na disku pro uložení další indexy a dokumenty.

Indexy větší trvat déle tooquery. Jako takový je možné, že každý další nárůst oddíly vyžaduje menší, ale přímo úměrná zvýšení replik. složitost Hello dotazy a dotaz svazek bude dvoufaktorového do jak rychle provádění dotazu zapnutý,.

## <a name="basic-tier-partition-and-replica-combinations"></a>Základní úroveň: kombinace parametrů oddílu a repliky
Základní služby můžete mít přesně jeden oddíl a až toothree replik pro maximální limit tři služby SUS. pouze upravit prostředek Hello je repliky. Pro zajištění vysoké dostupnosti u dotazů na potřebujete minimálně dvě repliky.

<a id="chart"></a>

## <a name="standard-tiers-partition-and-replica-combinations"></a>Standardní vrstev: kombinace parametrů oddílu a repliky
Tato tabulka ukazuje hello služby SUs požadované toosupport kombinace repliky a oddíly, limit 36 SU toohello předmětu, pro všechny standardní úrovně.

|   | **oddíl 1** | **2 oddíly** | **3 oddíly** | **4 oddíly** | **6 oddíly** | **12 oddíly** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 repliky** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 repliky** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 repliky** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 repliky** |4 SU |8 SU |12 SU |16 SU |24 SU |Není k dispozici |
| **5 repliky** |5 SU |10 SU |15 SU |20 SU |30 SU |Není k dispozici |
| **6 repliky** |6 SU |12 SU |18 SU |24 SU |36 SU |Není k dispozici |
| **12 repliky** |12 SU |24 SU |36 SU |Není k dispozici |Není dostupné. |Není k dispozici |

Služba SUs, ceny a kapacity jsou podrobně vysvětleny na hello webu Azure. Další informace najdete v tématu [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Hello počet replik a oddíly, které jsou rovnoměrně rozděleny do 12 (konkrétně 1, 2, 3, 4, 6, 12). Je to proto, že každý index Azure Search předem rozdělí na 12 horizontálních oddílů, takže je možné rozdělit do stejné části pro všechny oddíly. Například pokud vaše služba má tři oddíly a vytvoření indexu, každý oddíl bude obsahovat čtyři horizontálních oddílů indexu hello. Jak Azure Search horizontálních oddílů na index je podrobností implementace, předmět toochange v budoucích vezích. I když číslo hello je 12 dnes, není pravděpodobné, že, který číslo tooalways být 12 v hello budoucí.
>
>

## <a name="billing-formula-for-replica-and-partition-resources"></a>Vzorec fakturace pro repliky a oddíl prostředky
Hello vzorec pro výpočet, kolik služby SUs se používají pro konkrétní kombinace je produkt hello repliky a oddíly, nebo (R X P = SU). Například se jako devět služby SUs fakturuje tři repliky násobí hodnotou tři oddíly.

Cena za SU, je dáno hello úroveň, s nižší sazbou za jednotku fakturace pro základní než pro Standard. Vyhodnotí pro každou vrstvu lze najít v [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/search/).
