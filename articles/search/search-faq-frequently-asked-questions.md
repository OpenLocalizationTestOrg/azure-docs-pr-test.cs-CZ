---
title: "aaaFrequently kladené dotazy (FAQ) týkající se Azure Search | Microsoft Docs"
description: "Získejte odpovědi na otázky toocommon o službě Microsoft Azure Search"
services: search
author: HeidiSteen
manager: jhubbard
ms.service: search
ms.technology: search
ms.topic: article
ms.date: 08/03/2017
ms.author: heidist
ms.openlocfilehash: 2c573750600d80585b746bfce20d6c0f8df5a262
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search---frequently-asked-questions-faq"></a>Služba Azure Search – nejčastější dotazy (FAQ)
 
 Najít odpovědi toocommonly kladené dotazy týkající se koncepty, kód a scénáře související s tooAzure vyhledávání.

## <a name="platform"></a>Platforma

### <a name="how-is-azure-search-different-from-full-text-search-in-my-dbms"></a>Jak se liší od fulltextového vyhledávání v mé databázového systému Azure Search?

Služba Azure Search podporuje více zdrojů dat, [lingvistické analýzy pro mnoho jazyků](https://docs.microsoft.com/rest/api/searchservice/language-support), [vlastní analýza dat vstupy zajímavé a neobvyklé](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search), hledání rank ovládacích prvků prostřednictvím [vyhodnocování profily](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)a činnost koncového uživatele funkce jako je například typeahead, počtu zvýraznění a Fasetové navigace. Zahrnuje také další funkce, například synonyma a plnohodnotný dotazovací syntaxe, ale těch, které nejsou obvykle rozlišení funkce.

### <a name="what-is-hello-difference-between-azure-search-and-elasticsearch"></a>Jaký je rozdíl hello Azure Search a Elasticsearch?

Při porovnávání technologie vyhledávání, zákazníci často požádejte pro konkrétní na tom, jak Azure Search porovná s Elasticsearch. Zákazníci, kteří zvolte Azure Search přes Elasticsearch pro jejich vyhledávání projekty aplikací obvykle provést, protože jsme provedli úlohu klíčů snazší nebo potřebují hello integraci s jinými technologiemi společnosti Microsoft:

+ Služba Azure Search je plně spravovaná Cloudová služba se 99,9 % Smlouvy o úrovni služeb (SLA) při zřizování s dostatečnou redundance (2 repliky pro čtení, 3 repliky pro čtení i zápis).
+ Společnosti Microsoft [přirozeného jazyka procesory](https://docs.microsoft.com/rest/api/searchservice/language-support) nabízejí analysis inguistic hrany.  
+ [Azure Search indexery](search-indexer-overview.md) může procházet celou řadu zdrojů dat Azure pro počáteční a přírůstkové indexování.
+ Pokud potřebujete toofluctuations rychlou reakci v dotazu nebo indexování svazky, můžete použít [ovládací prvky posuvníku](search-manage.md#scale-up-or-down) v hello Azure portálu nebo spuštění [skript prostředí PowerShell](search-manage-powershell.md), obcházení horizontálního oddílu správu přímo.  
+ [Vyhodnocování a ladění funkcí](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) zadejte hello znamená, že pro ovlivňující skóre pořadí hledání nad rámec samotné jaké hello vyhledávacího webu můžete zadat. 

### <a name="can-i-pause-azure-search-service-and-stop-billing"></a>Můžete pozastavit službu Azure Search a zastavit fakturace?

Nelze pozastavit službu hello. Úložiště a výpočetní prostředky se přidělují pro výhradní použití při vytváření služby hello. Není možné toorelease a získat tyto prostředky na vyžádání. 

## <a name="indexing-operations"></a>Operace indexování

### <a name="backup-and-restore-or-download-and-move-indexes-or-index-snapshots"></a>Indexy zálohování a obnovení (nebo stahování a přesunutí) nebo snímky index?

Můžete sice [získat definice indexu](https://docs.microsoft.com/rest/api/searchservice/get-index) kdykoli, není žádný index extrakce, snímku nebo funkce zálohování – obnovení pro stahování *naplní* index spuštěné v hello cloudu tooa místní systém, nebo Přesunutí tooanother služby Azure Search. 

Indexy jsou vytvořené a naplněny z kód, který můžete zapsat a spustit pouze v Azure Search v cloudu hello. Obvykle zákazníkům, kteří chtějí toomove služby tooanother index upravit jejich kód toouse nový koncový bod a poté znovu spusťte indexování. Chcete-li možnost tootake hello snímek nebo zálohování index, přetypovat hlas [User Voice](https://feedback.azure.com/forums/263029-azure-search/suggestions/8021610-backup-snapshot-of-index).

### <a name="can-i-index-from-sql-database-replicas-applies-tooazure-sql-database-indexershttpsdocsmicrosoftcomazuresearchsearch-howto-connecting-azure-sql-database-to-azure-search-using-indexers"></a>Mohu indexovat z repliky databáze SQL (platí příliš[Azure SQL Database indexery](https://docs.microsoft.com/azure/search/search-howto-connecting-azure-sql-database-to-azure-search-using-indexers))

 Neexistují žádná omezení na hello použít primární nebo sekundární repliky jako zdroj dat, při vytváření indexu od začátku. Aktualizovat index s přírůstkové aktualizace (podle změněné záznamy) ale vyžaduje hello primární repliky. Tento požadavek pochází z databáze SQL, které záruky změňte sledování na pouze primární repliky. Pokud se pokusíte pomocí sekundární repliky pro úlohu obnovení indexu, není zaručeno, kterou získáte všechna hello data.

## <a name="search-operations"></a>Operace hledání

### <a name="can-i-search-across-multiple-indexes"></a>Můžete najít napříč více indexů?

Ne, tato operace není podporována. Hledání je vždy vymezená tooa jeden index.

### <a name="can-i-restrict-search-corpus-access-by-user-identity"></a>Můžete omezit vyhledávání svátek přístup podle identity uživatele?

Služba Azure Search nemá modelu na základě rolí zabezpečení pro přístup k datům na uživatele. Ověřování je buď úplná práva nebo na úrovni služby hello jen pro čtení. Někteří zákazníci byla implementována řešení založená na [ `$filter` parametr vyhledávání](https://docs.microsoft.com/rest/api/searchservice/search-documents), ale v nejlepší je alternativní řešení. Pokud chcete tuto funkci, přetypovat hlas [User Voice](https://feedback.azure.com/forums/263029-azure-search/category/86074-security).

### <a name="why-are-there-zero-matches-on-terms-i-know-toobe-valid"></a>Proč existují nula odpovídá podle podmínek poznám, že toobe platný?

Nejběžnější případ Hello není zároveň budete vědět, že každý typ dotazu podporuje chování různých vyhledávání a úrovně lingvistické analýzy. Fulltextové vyhledávání, který je hello převládá zatížení, zahrnuje analytické fáze jazyk, který dělí podmínky dolů tooroot formulářů. Tento aspekt analýza dotazu vrhá širší net přes odpovídající položky, protože hello tokenizovaná termín odpovídá větší počet variant.

Pokud vyvolání [jiné typy dotazů](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) (zástupný znak vyhledávání, přibližné vyhledávání, vyhledávání blízkých výrazů, návrhy, mimo jiné), neexistuje žádný lingvistické analýzy. Podmínky se přidají toohello stromu dotazu jako-je. 

### <a name="why-is-hello-search-rank-a-constant-or-equal-score-of-10-for-every-hit"></a>Proč je pořadí ve výsledcích hledání hello konstantní nebo rovna skóre 1.0 pro každý podle?

Ve výchozím nastavení, výsledky hledání jsou vypočítat skóre na základě na hello [statistické vlastnosti odpovídajících podmínky](search-lucene-query-architecture.md#stage-4-scoring)a vysokou toolow seřazené sady výsledků dotazu hello. Ale některé typy (zástupný znak, předpony, regulární výraz) dotazů vždy přispívat konstantní skóre dokumentů toohello celkové skóre. Toto chování je záměrné. Vyhledávání systému Azure vynucuje konstanta stanovení skóre tooallow shodami prostřednictvím toobe rozšíření dotazu součástí hello výsledky, aniž by to ovlivnilo hello hodnocení. 

Předpokládejme například, že vytváří odpovídá na "prohlídka", "tourettes" a "tourmaline" vstup "prohlídka *" v hledání pomocí zástupných znaků. Zadané hello povahu těchto výsledků, neexistuje žádný způsob odvození tooreasonably podmínky, které jsou vyšší hodnotu než jiné. Z tohoto důvodu jsme ignorovat termín frekvence, při vyhodnocování výsledky v dotazech typy zástupný znak, předpony a regulární výraz. Výsledky hledání podle částečné vstup mají konstanta stanovení skóre tooavoid odchylka směrem potenciálně neočekávané odpovídá.

## <a name="design-patterns"></a>Vzory návrhu

### <a name="what-is-hello-best-approach-for-implementing-localized-search"></a>Co je hello nejlepší metodou pro implementaci lokalizované hledání?

Většina zákazníků při přechodu do toosupporting různá národní prostředí (jazyky) v hello zvolte vyhrazené pole v kolekci stejný index. Pole specifické pro národní prostředí díky možné tooassign odpovídající analyzátor. Například přiřazování hello Microsoft francouzština Analyzer tooa pole obsahující francouzštině řetězce. Zjednodušuje také filtrování. Pokud víte, že dotaz se zahájí na stránce fr-fr, omezíte pole toothis výsledky hledání. Nebo vytvořte [vyhodnocování profil](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) toogive hello pole další relativní váhu.

## <a name="next-steps"></a>Další kroky

Je váš dotaz o chybějící funkce nebo funkce? Žádosti o funkce hello na hello [webu User Voice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="see-also"></a>Viz také

 [StackOverflow: Služba Azure Search](https://stackoverflow.com/questions/tagged/azure-search)   
 [Jak úplné textové vyhledávání funguje ve službě Azure Search](search-lucene-query-architecture.md)  
 [Co je Azure Search?](search-what-is-azure-search.md)

 