---
title: "Průvodce návrhem úložiště Table aaaAzure | Microsoft Docs"
description: "Návrh škálovatelné a původce tabulky ve službě Azure Table Storage"
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 02/28/2017
ms.author: mimig
ms.openlocfilehash: 059f05a1d20e4d9537034b7ca133c5334bbefa04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Průvodce návrhem tabulky úložiště Azure: Návrh škálovatelné a původce tabulky
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

toodesign, škálovatelnou a původce tabulky je nutné zvážit několik faktorů, třeba výkon, škálovatelnost a náklady. Pokud dříve navrženy schémata pro relační databáze, bude těchto aspektů známé tooyou, ale existuje některé podobnosti mezi modelu úložiště služby Azure Table hello a relační modely, existují také mnoho důležité rozdíly. Tyto rozdíly obvykle vést toovery různých návrzích, který může vypadat counter-intuitive nebo nesprávný toosomeone obeznámeni s relačních databází, ale které Nedělejte představu, pokud navrhujete pro úložiště dvojic klíč/hodnota NoSQL například hello služby Azure Table. Řadu váš návrh rozdíly bude odrážet hello skutečnost, že služby Table hello je navrženou toosupport škálování cloudové aplikace, které může obsahovat až miliardy entit (řádky v terminologii relační databáze), data nebo datové sady, které musí podporovat velmi vysoká transakce svazky: tedy potřebujete toothink jinak o tom, jak ukládat data a pochopit, jak funguje hello služby Table. Dobře navrženým úložiště dat typu NoSQL můžete povolit vaše řešení tooscale mnohem další (a se to při nižších nákladech) než řešení, které používá relační databáze. Tento průvodce vám pomůže s Tato témata.  

## <a name="about-hello-azure-table-service"></a>O hello služby Azure Table
V této části jsou zdůrazněné některé hello klíčové funkce služby Table hello které jsou obzvláště důležité toodesigning pro výkon a škálovatelnost. Pokud jste tooAzure nové úložiště a hello služby Table, nejdřív přečíst [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md) a [Začínáme s Azure Table Storage pomocí rozhraní .NET](table-storage-how-to-use-dotnet.md) než si přečtete zbytek hello článek. I když je aktivní hello tohoto průvodce hello služby Table, bude obsahovat některé diskuzi o hello fronty Azure a objekt Blob služby a jak je možné použít společně s hello služby Table v řešení.  

Co je služba Table hello? Podle očekávání z názvu hello používá hello služby Table dat toostore tabulkovém formátu. V terminologii standardní hello každý řádek tabulky hello reprezentuje entitu a úložiště sloupců hello hello různé vlastnosti dané entity. Každé entity má pár klíčů toouniquely ho, a sloupec časového razítka, která hello služby Table používá tootrack data poslední aktualizace hello entity (k tomu dojde automaticky a nelze ručně přepsat hello časové razítko s libovolnou hodnotou). Hello služby Table používá tento optimistickou metodu souběžného toomanage poslední úpravy časové razítko (LMT).  

> [!NOTE]
> operace REST API služby Table Hello se taky vrátit **značka ETag** hodnotu, která je odvozena z razítka poslední úpravy hello (LMT). V tomto dokumentu budeme používat hello podmínky ETag a LMT zcela zaměnitelným významem protože odkazují toohello stejné základní data.  
> 
> 

Hello následující příklad ukazuje toostore návrhu jednoduché tabulky zaměstnanců a oddělení entity. Řadu hello příklady uvedené v této příručce jsou založené na tento návrh jednoduché.  

<table>
<tr>
<th>Klíč oddílu</th>
<th>RowKey</th>
<th>časové razítko</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td>Na ochranu</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td>Června</td>
<td>CaO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Oddělení</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Prodej</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Pokud to vypadá velmi podobné tooa tabulky v relační databázi s hlavní rozdíly hello se hello povinné sloupce, a hello možnost toostore více entit typy v hello stejné tabulky. Kromě toho každý hello uživatelem definované vlastnosti, jako například **FirstName** nebo **stáří** má datový typ jako celé číslo nebo řetězec, jenom jako sloupec v relační databázi. I když na rozdíl od v relační databázi, hello bez schématu povaha hello tabulky služby znamená, že vlastnost nemusí mít hello stejný datový typ u každé entity. toostore komplexní datových typů v jedné vlastnosti, musíte použít serializovaných formátu například XML nebo JSON. Další informace o hello tabulky služby, například podporované datové typy, podporované rozsahů, pravidla pojmenování a omezení velikosti najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Jak uvidíte, vaši volbu **PartitionKey** a **RowKey** je základní toogood návrh tabulky. Každou entitu uložené v tabulce musí mít jedinečnou kombinaci **PartitionKey** a **RowKey**. Stejně jako u klíče v relační databázi tabulce hello **PartitionKey** a **RowKey** hodnoty jsou indexované toocreate clusterovaný index, který umožňuje rychlé look-ups, ale hello služby Table nevytvoří žádné sekundární indexy tak, aby byly hello jenom dvou indexované vlastnosti (některé vzory hello popsané dál zobrazit jak můžete obejít toto zřejmá omezení).  

Tabulka se skládá z jednoho nebo více oddílů, a jak uvidíte, řadu hello rozhodnutí, která učiníte bude kolem vyberete vhodný návrh **PartitionKey** a **RowKey** toooptimize řešení. Řešení může obsahovat pouze jednu tabulku, která obsahuje všechny vaše entity, které jsou uspořádány do oddílů, ale obvykle řešení bude mít více tabulek. Tabulky umožňují toologically uspořádání vaší entity, snadněji tak můžete spravovat přístup k toohello dat pomocí seznamy řízení přístupu a mohou vyřadit celé tabulky pomocí operace jedno úložiště.  

### <a name="table-partitions"></a>Oddíly tabulky
název účtu Hello, název tabulky a **PartitionKey** společně identifikovat hello oddílu v rámci služby hello úložiště, kde služby table hello ukládá hello entity. A také se součástí hello adresování schéma pro entity, oddíly definování oboru pro transakce (v tématu [Entity skupiny transakce](#entity-group-transactions) níže) a jak služby table hello škáluje základ hello formuláře. Další informace o oddílech najdete v části [a cíle výkonnosti služby Azure Storage Scalability](../storage/common/storage-scalability-targets.md).  

V hello služby Table, jednotlivé uzel služeb jeden nebo více dokončení oddíly a hello služby lze rozšířit pomocí dynamické vyrovnávání zatížení oddíly mezi uzly. Pokud uzel je zatížení, můžete služby table hello *rozdělení* hello rozsah oddílů obsluhovány pomocí daného uzlu do jiných uzlů; při provozu subvence, můžete službu hello *sloučení* hello oddílu rozsahy z quiet uzly zpět do jednoho uzlu.  

Další informace o hello interní podrobnosti hello služba Table a na konkrétní způsobu hello služba spravuje oddíly, najdete v dokumentu hello [Microsoft Azure Storage: A vysoce dostupné cloudového úložiště se silnou konzistencí](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Transakce skupiny entity
V hello služby Table jsou Entity skupiny transakce (EGTs) hello pouze integrované mechanismus pro provádění atomic aktualizace napříč více entit. EGTs se také označují tooas *dávky transakce* v některé dokumentaci. EGTs mohou pracovat pouze u entit, které jsou uložené v hello stejného oddílu (sdílené složky hello stejným klíčem oddílu v dané tabulce), takže můžete kdykoli potřebujete atomic transakční chování napříč více entit je třeba tooensure, které jsou tyto entity v hello stejného oddílu. Toto je často důvod pro zachování více typů entit v hello stejné tabulky (a oddílu) a nepoužíváte více tabulek pro typy různých entit. Jeden EGT mohou pracovat na maximálně 100 entit.  Pokud odešlete více souběžných EGTs pro zpracování, je důležité tooensure těchto EGTs na entity, které jsou společné napříč EGTs při zpracování v opačném případě může být zpoždění nefunguje.

EGTs také zavádět potenciální kompromis pro tooevaluate při návrhu: pomocí více oddíly bude zvýšení škálovatelnosti hello vaší aplikace, protože Azure má většího počtu možností pro vyrovnávání zatížení požadavky napříč uzly, ale to může omezit hello schopnost vaší aplikace tooperform jednotlivé transakce a udržovat silnou konzistenci pro vaše data. Kromě toho existují určité škálovatelnost cíle na úrovni hello oddílu, který může omezit hello propustnost transakcí můžete očekávat, že pro jeden uzel: Další informace o hello cíle škálovatelnosti pro účty úložiště Azure a tabulku hello najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](../storage/common/storage-scalability-targets.md). Novější části této příručky popisují různé návrhu strategií, které vám pomohou spravovat kompromis jako je tento a popisují, jak nejlépe toochoose klíč oddílu na základě hello specifických požadavků klientské aplikace.  

### <a name="capacity-considerations"></a>Aspekty kapacity
Hello následující tabulka uvádí některé z hello hodnoty klíče toobe vědět při navrhování řešení pro službu tabulky:  

| Celkové kapacity účtu úložiště Azure | 500 TB |
| --- | --- |
| Počet tabulek v účtu úložiště Azure |Omezeno pouze hello kapacity účtu úložiště hello |
| Počet oddílů v tabulce |Omezeno pouze hello kapacity účtu úložiště hello |
| Počet entit v oddílu |Omezeno pouze hello kapacity účtu úložiště hello |
| Velikost jednotlivých entit |Až too1 MB delší než 255 vlastnosti (včetně hello **PartitionKey**, **RowKey**, a **časové razítko**) |
| Velikost hello **PartitionKey** |Řetězec se velikost too1 KB |
| Velikost hello **RowKey** |Řetězec se velikost too1 KB |
| Velikost skupiny Entity transakce |Transakce může obsahovat maximálně 100 entit a datovou část hello musí být menší než 4 MB. EGT lze aktualizovat pouze entity jednou. |

Další informace najdete v tématu [hello Principy datového modelu služby Table](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Aspekty náklady
Table storage je relativně levný, ale by měla obsahovat odhadované náklady pro obě kapacitu a využití hello množství transakcí v rámci testování jakéhokoli řešení, které používá služby Table hello. V mnoha scénářích ukládání nenormalizované nebo duplicitní data v pořadí tooimprove hello výkonu nebo škálovatelnost řešení je ale tootake platný přístup. Další informace o cenách najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Pokyny pro návrh tabulky
Tyto seznamy shrnují některé hello klíče pokyny, které jste měli vzít v úvahu při navrhování vaší tabulky a tato příručka se jimi bude zabývat v podrobněji později. Tato pravidla se příliš neliší od hello pokyny, které by obvykle postupujte podle návrh relační databáze.  

Navrhování vaší tabulky služby řešení toobe *číst* efektivní:

* ***Návrh pro dotazování v aplikacích číst náročné.*** Při navrhování vaší tabulky, vezměte v úvahu hello dotazy (zejména hello latence citlivé ty), které se provedou předtím, než si myslíte o tom, jak bude aktualizovat váš entity. To obvykle vede efektivní a původce řešení.  
* ***Zadejte klíč oddílu a RowKey v dotazech.*** *Bod dotazy* , jako jsou ty se hello nejúčinnější tabulky služby dotazy.  
* ***Zvažte uložení duplicitní kopie entit.*** Table storage je levných, zvažte uložení hello stejné entity vícekrát (s různými klíči) tooenable efektivnější dotazy.  
* ***Vezměte v úvahu denormalizing vaše data.*** Table storage je levných, vezměte v úvahu denormalizing vaše data. Například uložte souhrn entit, tak, aby dotazy pro agregovaná data, stačí tooaccess jedné entity.  
* ***Použijte hodnoty složeného klíče.*** Hello pouze klíče máte jsou **PartitionKey** a **RowKey**. Například použijte složené klíče hodnoty tooenable alternativní s klíčem přístup cesty tooentities.  
* ***Použijte projekce dotazu.*** Můžete snížit hello množství dat, který přenos přes síť hello pomocí dotazů, které vyberte právě hello pole, které potřebujete.  

Navrhování vaší tabulky služby řešení toobe *zápisu* efektivní:  

* ***Nevytvářejte aktivní oddíly.*** Zvolte klíče, které umožňují toospread své žádosti napříč více oddílů v libovolném bodě času.  
* ***Vyhněte se špičky v provozu.*** Funkce Smooth hello provoz přiměřené období a vyhnout se špičky v provozu.
* ***Nevytvářejte nutně do samostatné tabulky pro každý typ entity.*** Pokud požadujete jednotlivé transakce mezi typy entit, můžete uložit těchto více typů entit ve stejné oddílu v hello hello stejné tabulky.
* ***Vezměte v úvahu hello maximální propustnost, kterou musí dosáhnout.*** Musí mít přehled o hello cíle škálovatelnosti pro hello služby Table a ujistěte se, že váš návrh nezpůsobí jste tooexceed je.  

Při čtení této příručky, zobrazí se příklady, které všechny tyto zásady uvedené do praxe.  

## <a name="design-for-querying"></a>Návrh pro dotazování
Řešení služby TABLE lze číst náročné na prostředky, zápis náročné na prostředky nebo jejich kombinace hello dva. Tato část se zaměřuje na toobear věcí hello nezapomeňte při návrhu vaší toosupport služby tabulky operací čtení efektivně. Návrh podporuje efektivně zapisovacích operací je obvykle také efektivní pro operace zápisu. Existují však toobear další aspekty v úvahu při navrhování toosupport zápisu operace, které jsou popsané v další části hello, [návrhu pro úpravu dat](#design-for-data-modification).

To dobrý výchozí bod pro návrh vaší tabulky služby řešení tooenable tooread data efektivně je tooask "jaké dotazy bude Moje nutné tooexecute tooretrieve hello data aplikací, které je nutné z hello služby Table?"  

> [!NOTE]
> S hello služby Table, je důležité tooget hello návrhu správné předem protože je složité a nákladné toochange později. Například v relační databázi je často problémy s výkonem možné tooaddress jednoduše přidáním indexuje existující databázi tooan: to není možné s hello služby Table.  
> 
> 

Tato část se zaměřuje na hello klíčových otázek, které musíte vyřešit, když tabulek pro zadávání dotazů. témata Hello uvedenými v této části:

* [Jak vaši volbu PartitionKey a RowKey ovlivňuje výkon dotazů](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Výběr vhodné PartitionKey](#choosing-an-appropriate-partitionkey)
* [Optimalizace dotazů pro hello služby Table](#optimizing-queries-for-the-table-service)
* [Řazení dat v hello služby Table](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Jak vaši volbu PartitionKey a RowKey ovlivňuje výkon dotazů
Hello následující příklady předpokládají služby table hello je ukládání entit zaměstnanec s hello strukturu (většina z příkladů hello vynechejte hello **časové razítko** vlastnost pro přehlednost):  

| *Název sloupce* | *Datový typ* |
| --- | --- |
| **PartitionKey** (název oddělení) |Řetězec |
| **RowKey** (Id zaměstnance) |Řetězec |
| **FirstName** |Řetězec |
| **Příjmení** |Řetězec |
| **Stáří** |Integer |
| **EmailAddress** |Řetězec |

Hello výše uvedené části [Přehled služby Azure Table](#overview) popisuje některé z hello klíčové funkce hello služby Azure Table které mají přímý vliv na návrh pro dotaz. To mít za následek hello následující obecné pokyny pro návrh tabulky služby dotazy. Upozorňujeme, že se syntaxí filtru hello použít v následujících příkladech hello se ze služby Table hello rozhraní REST API, další informace najdete v tématu [dotazu entity](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

* A ***bodu dotazu*** je nejúčinnější toouse vyhledávání hello a doporučuje toobe používá pro vyhledávání vysoký počet nebo vyhledávání, které vyžadují nejnižší latenci. Pak můžete takový dotaz velmi efektivně použít hello indexy toolocate jednotlivých entit zadáním obě hello **PartitionKey** a **RowKey** hodnoty. Příklad: $filter = (PartitionKey eq 'Prodej') a (RowKey eq: 2.)  
* Druhý nejlépe je ***dotazu na rozsah*** používající hello **PartitionKey** a filtry pro celou řadu **RowKey** hodnoty tooreturn více než jedna entita. Hello **PartitionKey** hodnota identifikuje konkrétního oddílu a hello **RowKey** hodnoty identifikovat podmnožinu hello entity v oddílu. Příklad: $filter = PartitionKey eq 'prodeje a RowKey ge' a RowKey lt se.  
* Je třetí nejlepší ***oddílu kontrolovat*** používající hello **PartitionKey** a filtry na jiné neklíčovou vlastnost a který může vrátit více než jedna entita. Hello **PartitionKey** hodnota identifikuje na konkrétní oddíl a hodnoty vlastnosti hello, vyberte pro podmnožinu hello entity v oddílu. Příklad: $filter = PartitionKey eq 'prodeje a LastName eq: Váša.  
* A ***tabulky kontrolovat*** nezahrnuje hello **PartitionKey** a je velmi neefektivní, protože všechny hello oddíly, které tvoří tabulku zase pro všechny odpovídající entity vyhledávání. Provede prohledávání tabulky bez ohledu na to, zda filtr používá hello **RowKey**. Příklad: $filter = LastName eq 'Petr.  
* Dotazy, které vracejí víc entit obnoví v nich seřazeny ve **PartitionKey** a **RowKey** pořadí. Zvolte tooavoid opětné řazení hello entity v hello klienta **RowKey** , který definuje hello nejběžnější pořadí řazení.  

Všimněte si, že pomocí "**nebo**" toospecify filtr na základě **RowKey** hodnoty výsledků v oddílu kontroly a nepovažuje se za dotazu na rozsah. Proto byste neměli dotazy, které pomocí filtrů, jako například: $filter = PartitionKey eq 'Prodej' a (RowKey eq '121' nebo RowKey eq "322")  

Příklady kódu na straně klienta, které používají efektivní dotazy tooexecute hello Klientská knihovna pro úložiště najdete v tématu:  

* [Spuštění dotazu na bodu pomocí hello Klientská knihovna pro úložiště](#executing-a-point-query-using-the-storage-client-library)
* [Načítání více entit pomocí LINQ](#retrieving-multiple-entities-using-linq)
* [Projekce na straně serveru](#server-side-projection)  

Příklady kódu na straně klienta, který může zpracovat více entit typy uložené v hello stejné tabulky najdete v tématu:  

* [Práce s typy heterogenní entit](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Výběr vhodné PartitionKey
Vaši volbu **PartitionKey** by měl vyrovnávat hello nutné tooenables hello použití EGTs (tooensure konzistence) proti hello požadavek toodistribute vaší entity ve více oddílů (tooensure škálovatelné řešení).  

V jedné extreme může ukládat všechny entity v jeden oddíl, ale to může omezit škálovatelnost hello vašeho řešení a by bránily je možné tooload vyrovnávat požadavky na služby table hello. V hello jiných extreme může uložit jednu entitu na oddíl, který bude vysoce škálovatelné a která umožňuje hello tabulky služby tooload vyrovnávat požadavky, ale které by zabránit vám v použití transakcí skupiny entity.  

Ideálu **PartitionKey** je ten, který vám umožní efektivní dotazy toouse a má dostatek tooensure oddíly řešení je škálovatelná. Obvykle zjistíte, že vaší entity budou mít vhodný vlastnost, která distribuuje vaší entity v dostatečná oddíly.

> [!NOTE]
> V systému, která uchovává informace o uživatele nebo zaměstnancům, například ID uživatele může být dobrým PartitionKey. Může mít několik entit, které použít jako klíč oddílu hello dané ID uživatele. Každá entita, která ukládá data o uživateli se seskupují do jednoho oddílu, a proto tyto entity jsou přístupné přes transakce skupiny entity, ale stále mít vysoce škálovatelná.
> 
> 

Další rozhodnutí v požadovaném **PartitionKey** které se týkají toohow bude vložit, aktualizovat a odstranit entity: najdete v tématu hello [návrhu pro úpravu dat](#design-for-data-modification) níže.  

### <a name="optimizing-queries-for-hello-table-service"></a>Optimalizace dotazů pro hello služby Table
Hello služby Table automaticky indexuje vaší entity pomocí hello **PartitionKey** a **RowKey** hodnot v jednom clusterovaný index, proto hello důvodu, že bod dotazy jsou hello nejúčinnější toouse . Však nejsou žádné indexy než tu, která na clusterovaný index hello hello **PartitionKey** a **RowKey**.

Řada návrhů musí splňovat požadavky na vyhledávání tooenable entit na základě několika kritérií. Například hledání zaměstnanec entity podle e-mailu, identifikační číslo zaměstnance nebo příjmení. tyto vzory v části hello Hello [vzory návrhu tabulky](#table-design-patterns) adres tyto typy požadavek a popisují způsoby obcházet hello fakt, že služby Table hello nenabízí sekundární indexy:  

* [Vzor sekundární index Intra-partition](#intra-partition-secondary-index-pattern) -uložit více kopií každou entitu s využitím různých **RowKey** hodnoty (v hello stejného oddílu) tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různé **RowKey** hodnoty.  
* [Vzor mezi oddíl sekundární index](#inter-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey v samostatné oddíly nebo v samostatných tabulkách tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různých **RowKey** hodnoty.  
* [Index entity vzor](#index-entities-pattern) -udržovat index entity tooenable efektivní vyhledávání, která vrátí seznam entit.  

### <a name="sorting-data-in-hello-table-service"></a>Řazení dat v hello služby Table
Hello služby Table vrací entity ve vzestupném pořadí podle **PartitionKey** a potom podle **RowKey**. Tyto klíče jsou hodnoty řetězce a tooensure, která číselné hodnoty řadit správně, měli byste je převést tooa pevná délka a jejich odsadí nulami. Například pokud hello hodnota id zaměstnance můžete použít jako hello **RowKey** je celočíselná hodnota, bude třeba převést identifikační číslo zaměstnance **123** příliš**00000123**.  

Mnoho aplikací mít požadavky na toouse data seřazená v různém pořadí: například řazení zaměstnanci podle názvu nebo díky připojení ke službě data. Hello tyto vzory v části hello [vzory návrhu tabulky](#table-design-patterns) adres, jak pořadí řazení tooalternate pro vaše entity:  

* [Vzor sekundární index Intra-partition](#intra-partition-secondary-index-pattern) -uložit více kopií každé entity pomocí různých hodnot RowKey (v hello stejného oddílu) tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí za použití různých hodnot RowKey.  
* [Vzor mezi oddíl sekundární index](#inter-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey v samostatné oddíly v samostatných tabulkách tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různých RowKey hodnoty.
* [Vzor protokolu poškozené databáze](#log-tail-pattern) -načtení hello * n * entity naposledy přidat oddíl tooa pomocí **RowKey** hodnotu, která seřadí zpětné datum a čas pořadí.  

## <a name="design-for-data-modification"></a>Návrh pro úpravu dat
Tato část se zaměřuje na aspekty návrhu hello pro optimalizaci vložení, aktualizace a odstraní. V některých případech je nutné tooevaluate hello kompromis mezi návrhů, které je optimální pro dotazování na návrhů, které je optimální pro úpravu dat stejným způsobem jako v návrhy pro relační databáze (i když hello techniky pro správu hello návrhu kompromis se liší v relační databázi). Hello části [vzory návrhu tabulky](#table-design-patterns) popisuje některé podrobné návrhových schématech hello služby Table a zvýrazňuje některé tyto kompromis. V praxi zjistíte, že řada návrhů optimalizované pro dotazování entity také fungovat i pro úpravy entity.  

### <a name="optimizing-hello-performance-of-insert-update-and-delete-operations"></a>Optimalizace výkonu hello vložit, aktualizovat a operace odstranění
tooupdate nebo odstranění entity, musí být schopný tooidentify ho pomocí hello **PartitionKey** a **RowKey** hodnoty. V tomto ohledu vaši volbu **PartitionKey** a **RowKey** pro úpravy entity byste měli postupovat podle podobné kritéria tooyour volba toosupport bodu dotazů, protože chcete, aby tooidentify entity jako možná nejefektivněji. Nechcete toouse neefektivní oddílu nebo tabulky kontroly toolocate entity v pořadí toodiscover hello **PartitionKey** a **RowKey** hodnoty potřebovat tooupdate nebo ho odstranit.  

tyto vzory v části hello Hello [vzory návrhu tabulky](#table-design-patterns) adres optimalizace výkonu hello nebo vaší insert, update a operace odstranění:  

* [Velkému odstranit vzor](#high-volume-delete-pattern) -Povolit odstranění hello k velkému počtu entity uložením všechny entity hello k odstranění souběžných vlastní samostatné tabulky; odstranit hello entity odstraněním hello tabulky.  
* [Vzorek dat řady](#data-series-pattern) -úložiště dokončení datové řady za jedné entity toominimize hello počet požadavků, které provedete.  
* [Vzor široké entity](#wide-entities-pattern) -použití více fyzických entit toostore logických entit s více než 252 vlastností.  
* [Vzor velké entity](#large-entities-pattern) -použití objektu blob úložiště toostore vlastnost velké hodnoty.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Zajištění konzistence v uložené entity
Hello jiných klíčovým faktorem, který ovlivňuje vaši volbu klíče pro optimalizaci změny dat je jak tooensure konzistence pomocí jednotlivé transakce. Toooperate EGT lze použít pouze u entit, které jsou uložené v hello stejného oddílu.  

tyto vzory v části hello Hello [vzory návrhu tabulky](#table-design-patterns) adresu Správa konzistence:  

* [Vzor sekundární index Intra-partition](#intra-partition-secondary-index-pattern) -uložit více kopií každou entitu s využitím různých **RowKey** hodnoty (v hello stejného oddílu) tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různé **RowKey** hodnoty.  
* [Vzor mezi oddíl sekundární index](#inter-partition-secondary-index-pattern) – ukládání více kopií každé entity pomocí různých hodnot RowKey v samostatné oddíly nebo v samostatných tabulkách tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různých **RowKey** hodnoty.  
* [Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) -povolit nakonec byl konzistentní chování v rámci hranice oddílů nebo hranice systému úložiště pomocí front Azure.
* [Index entity vzor](#index-entities-pattern) -udržovat index entity tooenable efektivní vyhledávání, která vrátí seznam entit.  
* [Vzor denormalization](#denormalization-pattern) -zkombinujte související data společně v jedné entity tooenable tooretrieve všechny hello dat je třeba pomocí dotazu jediný bod.  
* [Vzorek dat řady](#data-series-pattern) -úložiště dokončení datové řady za jedné entity toominimize hello počet požadavků, které provedete.  

Informace o transakcích skupiny entity, najdete v tématu hello [Entity skupiny transakce](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Zajištění návrhu pro efektivní úpravy usnadňuje efektivní dotazy
V mnoha případech by měl návrhu pro efektivní dotazování výsledků v efektivní úpravy, ale vždy vyhodnocení, zda se jedná o případ hello pro konkrétní scénář. Některé vzory hello v části hello [vzory návrhu tabulky](#table-design-patterns) explicitně vyhodnocení kompromis mezi dotazování a úprava entity a vždy byste měli vzít do účtu hello počty jednotlivých typů operace.  

tyto vzory v části hello Hello [vzory návrhu tabulky](#table-design-patterns) adres kompromis mezi návrhu pro efektivní dotazy a návrhu pro úpravu efektivní dat:  

* [Složené klíče vzor](#compound-key-pattern) -použití složené **RowKey** hodnoty tooenable klienta toolookup související data pomocí dotazu jediný bod.  
* [Vzor protokolu poškozené databáze](#log-tail-pattern) -načtení hello * n * entity naposledy přidat oddíl tooa pomocí **RowKey** hodnotu, která seřadí zpětné datum a čas pořadí.  

## <a name="encrypting-table-data"></a>Šifrování dat v tabulce
Hello .NET Klientská knihovna pro úložiště Azure podporuje šifrování vlastnosti entity řetězce pro vložení a nahrazovat operace. Hello šifrované řetězce jsou uložené ve službě hello jako binární vlastnosti a převedení back toostrings po dešifrování.    

Pro tabulky kromě toohello zásady šifrování, musí uživatelé zadat toobe vlastnosti hello zašifrovaná. To lze provést zadáním buď atribut [EncryptProperty] (pro entity objektů POCO, které jsou odvozeny od TableEntity) nebo šifrování překladač v žádosti o možnostech. Překladač šifrování je delegáta, který přebírá klíč oddílu, klíč řádku a název vlastnosti a vrátí logickou hodnotu, která určuje, jestli by se šifrovat tuto vlastnost. Během šifrování se použije hello klientské knihovny tato informace toodecide zda vlastnosti by se měla šifrovat během zápisu toohello přenosu. Delegát Hello také poskytuje možnost hello logiky kolem jak jsou zašifrované vlastnosti. (Například pokud X, potom šifrování vlastnost A; v opačném případě šifrování vlastnosti A a B.) Všimněte si, že IT oddělení není nutné tooprovide tyto informace při čtení nebo dotazování entity.

Všimněte si, že sloučení není aktuálně podporována. Vzhledem k tomu, že podmnožinu vlastností může být šifrována dříve pomocí jiného klíče, jednoduše slučování hello nové vlastnosti a aktualizace hello metadat dojde ke ztrátě dat. Slučování buď vyžaduje provedení další služby volání tooread hello existující entity ze služby hello nebo pomocí nového klíče na vlastnosti, které nejsou vhodné z důvodů výkonu.     

Informace o šifrování dat v tabulce najdete v tématu [šifrování na straně klienta a Azure Key Vault pro Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>Modelování vztahů
Vytváření modelů domény je klíče krok v návrhu hello složitých systémů. Obvykle použijete hello modelování proces tooidentify entity a hello vztahy mezi nimi jako způsob toounderstand hello firmy domény a informujte hello návrh vašeho systému. Tato část se zaměřuje na tom, jak může překládat některé běžné typy vztahů hello najít v doméně toodesigns modely pro hello služby Table. Hello proces mapování z logický datový model tooa fyzické NoSQL na základě datového modelu se příliš neliší od kterého při navrhování relační databáze. Návrh relační databáze obvykle předpokládá proces normalizace dat optimalizované pro minimalizaci redundance – a deklarativní dotazování funkci, která přehledů jak hello implementace fungování hello databáze.  

### <a name="one-to-many-relationships"></a>Vztahy jeden mnoho
Na více vztahů mezi objekty domény obchodní dojde k velmi často: například jeden oddělení má mnoho pracovníků. V hello služby Table každý s výhody a nevýhody, které mohou být relevantní toohello konkrétní scénář je několik způsobů tooimplement na více relací.  

Podívejte se na příklad hello velké korporace více national s desítkami tisíc oddělení a zaměstnanci entity, kde každé oddělení má mnoho zaměstnance a zaměstnance, jak je přidružený jeden konkrétní oddělení. Jeden z přístupů je toostore samostatné oddělení a zaměstnanci entitami, jako je například tyto:  

![][1]

Tento příklad ukazuje implicitní vztah jeden mnoho mezi typy hello podle hello **PartitionKey** hodnotu. Každé oddělení může mít mnoho pracovníků.  

Tento příklad také ukazuje entity oddělení a jeho souvisejících zaměstnanec entity v hello stejného oddílu. Může zvolit toouse různých oddílů, tabulky nebo i účty úložiště pro typy hello různých entit.  

Alternativní způsob je toodenormalize pouze zaměstnanci entity s nenormalizované oddělení dat vaše data a úložiště, jak ukazuje následující příklad hello. V tomto scénáři konkrétní tento nenormalizované přístup nemusí být hello nejlepší Pokud máte požadavek toobe možné toochange hello podrobnosti o oddělení manager, protože toodo to budete potřebovat tooupdate každý zaměstnanec hello oddělení.  

![][2]

Další informace najdete v tématu hello [Denormalization vzor](#denormalization-pattern) dál v této příručce.  

Hello následující tabulka shrnuje hello výhody a nevýhody jednotlivých uvedených výše pro ukládání zaměstnanců a oddělení entity, které mají vztah jeden mnoho přístupů hello. Také byste měli zvážit, jak často předpokládáte tooperform, různé operace: to může být přijatelné toohave návrh, který zahrnuje náročná operace, pokud tuto operaci pouze dochází zřídka.  

<table>
<tr>
<th>Přístup</th>
<th>Odborníci na</th>
<th>Nevýhody</th>
</tr>
<tr>
<td>Jednotlivé typy entit, stejného oddílu, stejné tabulky</td>
<td>
<ul>
<li>Entity oddělení můžete aktualizovat pomocí jedné operace.</li>
<li>Pokud máte požadavek toomodify entity oddělení, můžete použít EGT toomaintain konzistence pokaždé, když jste aktualizace, insert nebo odstranění entity zaměstnanců. Například pokud chcete zachovat počet zaměstnanců oddělení pro každé oddělení.</li>
</ul>
</td>
<td>
<ul>
<li>Může být nutné tooretrieve zaměstnanec i oddělení entity pro některé činnosti klienta.</li>
<li>Operace úložiště dojít v hello stejné oddílu. Na vysoké transakce svazky proto může docházet v aktivní oblast.</li>
<li>Nelze přesunout zaměstnance tooa nové oddělení pomocí EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Typy samostatné entity, různých oddílů nebo tabulek účty úložiště</td>
<td>
<ul>
<li>Oddělení entity nebo zaměstnanec entity můžete aktualizovat pomocí jedné operace.</li>
<li>Na vysoké transakce svazky, může to pomoci šíření hello zatížení mezi více oddílů.</li>
</ul>
</td>
<td>
<ul>
<li>Může být nutné tooretrieve zaměstnanec i oddělení entity pro některé činnosti klienta.</li>
<li>Nelze použít EGTs toomaintain konzistence když jste update, insert nebo odstranění zaměstnanec a aktualizace oddělení. Aktualizuje se například počet zaměstnanců v oddělení entity.</li>
<li>Nelze přesunout zaměstnance tooa nové oddělení pomocí EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Denormalize do jedné entity typu</td>
<td>
<ul>
<li>Můžete načíst všechny hello informace, které je nutné se jeden požadavek.</li>
</ul>
</td>
<td>
<ul>
<li>Pokud potřebujete informace oddělení tooupdate (to by znamenalo jste tooupdate všichni zaměstnanci hello v oddělení) může být nákladné toomaintain konzistence.</li>
</ul>
</td>
</tr>
</table>

Jak zvolíte mezi tyto možnosti a které hello specialisté a cons jsou nejvýznamnější, závisí na konkrétní aplikaci scénářů. Například jak často upravíte entities oddělení; mají všechny dotazy zaměstnanec hello další oddělení informace; jak jsou omezení škálovatelnosti toohello na oddíly nebo účtu úložiště?  

### <a name="one-to-one-relationships"></a>Relace 1: 1
Modely domény může zahrnovat relace 1: 1 mezi entitami. Pokud potřebujete tooimplement relace v hello služby Table, musíte také zvolit, jak toolink hello dvě související entity, pokud je potřebujete tooretrieve obě. Tento odkaz může být implicitní, podle názvů v hodnoty klíče hello nebo explicitní uložením odkaz hello tvar **PartitionKey** a **RowKey** hodnot v jednotlivých entit tooits související entity. Informace o tom, zda by měl uložit hello související entity v hello stejného oddílu, najdete v tématu hello [na více vztahů](#one-to-many-relationships).  

Všimněte si, že existují také aspekty implementace, které můžou způsobit je relace 1: 1 tooimplement v služby Table hello:  

* Zpracování velkých entit (Další informace najdete v tématu [velké vzor entity](#large-entities-pattern)).  
* Implementace řízení přístupu (Další informace najdete v tématu [řízení přístupu s podpisy sdíleného přístupu](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-hello-client"></a>Připojení v klientovi hello
I když existují způsoby toomodel vztahy v hello služby Table, by neměl zapomenete, zda jsou dva prvotní důvody pro používání služby Table hello hello škálovatelnost a výkon. Pokud zjistíte, že jsou modelování více vztahů, které ohrožují hello výkon a škálovatelnost řešení, měli byste požádat sami že pokud je nutné toobuild všechny hello relace mezi daty do návrhu tabulky. Může být schopný toosimplify hello návrhu a zvýšit hello škálovatelnost a výkon vašeho řešení, pokud je necháte klientskou aplikaci, proveďte všechny nezbytné spojení.  

Například pokud máte malé tabulky, které obsahují data, která se nemění příliš často, pak můžete tato data načíst jednou a uložení do mezipaměti na klientovi hello. To se můžete vyhnout opakovaných zbytečné komunikace tooretrieve hello stejná data. V příkladech hello, které jsme jste prohlédli v této příručce hello sadu oddělení v organizaci, malé je pravděpodobně toobe malé a změňte zřídka díky tomu vhodným kandidátem na data, která klientská aplikace můžete stáhnout jednou a mezipaměti jako vyhledávání dat.  

### <a name="inheritance-relationships"></a>Vztahy dědičnosti
Pokud klientské aplikace používá sadu tříd, které jsou součástí dědičnosti vztah toorepresent obchodní entity, můžete snadno zachovat tyto entity v hello služby Table. Například můžete mít následující sadu tříd definovaných v klientské aplikace hello kde **osoba** je abstraktní třída.

![][3]

Můžete zachovat instancí dvě konkrétní třídy v hello služby Table pomocí jedné tabulky osoba pomocí entity v tomto vypadají hello:  

![][4]

Další informace o práci s více typy entit v hello stejné tabulky v kódu klienta najdete v tématu hello [práce s typy entit heterogenní](#working-with-heterogeneous-entity-types) dál v této příručce. To obsahuje příklady jak toorecognize hello typ entity v kódu klienta.  

## <a name="table-design-patterns"></a>Vzory návrhu tabulky
V předchozích částech jste viděli některé podrobné diskusí o tom, jak toooptimize tabulku pro obě načítání dat entity pomocí dotazů a návrhu pro vložení, aktualizaci a odstraňování dat entity. Tato část popisuje některé vzory vhodné používat s řešeními služby Table. Kromě toho se zobrazí, může prakticky adresování některé problémy hello a kompromis vyvolá dříve v této příručce. Hello následující diagram shrnuje hello vztahy mezi hello různé vzorce:  

![][5]

Mapa vzor Hello výše označuje některé vztahy mezi vzory (modré) a proti vzory (oranžová), které jsou popsané v této příručce. Mnoho dalších vzorech, které jsou vhodné vzhledem k tomu, samozřejmě neexistují. Například jeden z hello klíčové scénáře pro služby Table je toouse hello [Materializována vzor zobrazení](https://msdn.microsoft.com/library/azure/dn589782.aspx) z hello [příkaz dotazu odpovědnost oddělení (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) vzor.  

### <a name="intra-partition-secondary-index-pattern"></a>Vzor Intra-partition sekundární index
Uložit více kopií každou entitu s využitím různých **RowKey** hodnoty (v hello stejného oddílu) tooenable rychlé a efektivní vyhledávání a řazení alternativní řadí pomocí různých **RowKey** hodnoty. Aktualizace mezi kopie je možné mít konzistentní pomocí EGT společnosti.  

#### <a name="context-and-problem"></a>Kontext a problém
Hello služby Table automaticky indexuje entity pomocí hello **PartitionKey** a **RowKey** hodnoty. To umožňuje klienta aplikace tooretrieve entity efektivně pomocí těchto hodnot. Například struktura tabulky hello vidíte níže, klientská aplikace pomocí tooretrieve dotaz bod entitu jednotlivých zaměstnanců pomocí názvu hello oddělení a identifikační číslo zaměstnance hello (hello **PartitionKey** a ** RowKey** hodnoty). Klienta můžete také načíst entity seřazené podle id zaměstnance v rámci každé oddělení.

![][6]

Pokud chcete mít toofind toobe entitu zaměstnanců na základě hodnoty hello další vlastnosti, jako je například e-mailovou adresu, musíte použít méně efektivní prohledávání toofind oddílu shody. Je to proto, že služby table hello neposkytuje sekundární indexy. Kromě toho není žádná možnost toorequest seznam seřazen v jiném pořadí než zaměstnanců **RowKey** pořadí.  

#### <a name="solution"></a>Řešení
toowork kolem hello nedostatečná sekundární indexy, můžete uložit více kopií každé entity s každou kopii pomocí jiné **RowKey** hodnotu. Pokud ukládáte entitu s hello struktury vidíte níže, můžete efektivně načíst zaměstnanec entit na základě id e-mailovou adresu nebo zaměstnanců. Hello předpon pro hello **RowKey**, "empid_" a "email_" umožňují tooquery pro jeden zaměstnanců nebo rozsah zaměstnanci pomocí řadu e-mailové adresy nebo ID zaměstnance.  

![][7]

Hello následující dvě kritéria filtru (jeden vyhledávání podle id zaměstnance a jeden vyhledávání pomocí e-mailové adresy) i zadejte bod dotazy:  

* $filter = (PartitionKey eq 'Prodej') a (RowKey eq 'empid_000223')  
* $filter = (PartitionKey eq 'Prodej') a (RowKey eq 'email_jonesj@contoso.com')  

Pokud vyhledat rozsahu entit zaměstnanců, můžete zadat rozsah id řazení zaměstnanců nebo rozsahu seřazeny v e-mailovou adresu pořadí pomocí dotazu pro entity s příslušnou předponu hello hello **RowKey**.  

* použít všechny zaměstnance hello v oddělení prodeje hello se do rozsahu 000100 too000199 hello identifikační číslo zaměstnance toofind: $filter = (PartitionKey eq 'Prodej') a (RowKey ge 'empid_000100') a (RowKey le 'empid_000199')  
* toofind všechny zaměstnance v oddělení prodeje hello hello s e-mailovou adresu, počínaje hello písmeno "a" použití: $filter = (PartitionKey eq 'Prodej') a (RowKey ge 'email_a') a (RowKey lt 'email_b')  
  
  Upozorňujeme, že se syntaxí filtru hello použít ve výše uvedených příkladech hello se ze služby Table hello rozhraní REST API, další informace najdete v tématu [dotazu entity](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Table storage je relativně levný toouse, takže hello náklady režijní náklady na ukládání dat duplicitní by nemělo být závažný problém. Však měli vždy vyhodnoceny hello náklady návrhu na základě požadavků vaší předpokládaného úložiště a přidat pouze duplicitní položky toosupport hello dotazy, které budou spuštěny klientské aplikace.  
* Proto hello sekundární index entity jsou uložené v hello stejné jako původní entity hello oddílu, musíte ověřit nepřekračují hello cíle škálovatelnosti pro jednotlivé oddíl.  
* Vzájemnou konzistenci duplicitní položky můžete ponechat atomicky pomocí EGTs tooupdate hello dvě kopie hello entity. To znamená, že měli uložit všechny kopie entity v hello stejného oddílu. Další informace najdete v tématu hello [pomocí transakce skupiny Entity](#entity-group-transactions).  
* Hello hodnota používaná pro hello **RowKey** musí být jedinečný pro každé entity. Zvažte použití hodnoty složeného klíče.  
* Odsazení číselných hodnot v hello **RowKey** (například id zaměstnance hello 000223), umožňuje opravte řazení a filtrování na základě horní a dolní meze.  
* Není nutné nutně tooduplicate všechny hello vlastnosti vaší entity. Například pokud hello dotazy, které e-mailu pomocí hello vyhledávací hello entity, které se adresu v hello **RowKey** nikdy nepotřebují stáří hello zaměstnance, tyto entit může mít hello strukturu:

![][8]

* Je obvykle lepší toostore duplicitních dat a zkontrolujte, zda lze načíst všechna data hello, které je nutné se jeden dotaz, než jeden dotaz toolocate toouse entity a jiné toolookup hello požadovaná data.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, když klientské aplikace potřebuje tooretrieve entity pomocí různých odlišné klíče, když klient potřebuje tooretrieve entity v jiné pořadí řazení, a tam, kde můžete identifikovat každé entity pomocí různých jedinečné hodnoty. Nicméně byste měli jistotu, že nedošlo k překročení omezení škálovatelnosti oddílu hello při provádění entity vyhledávání pomocí různých hello **RowKey** hodnoty.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Vzor sekundární index mezi oddílů](#inter-partition-secondary-index-pattern)
* [Složené klíče vzor](#compound-key-pattern)
* [Transakce skupiny entity](#entity-group-transactions)
* [Práce s typy heterogenní entit](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Vzor sekundární index mezi oddílů
Uložit více kopií každou entitu s využitím různých **RowKey** hodnoty v samostatné oddíly nebo v samostatných tabulkách tooenable rychlé a efektivní vyhledávání a alternativní pořadí řazení pomocí různých **RowKey**hodnoty.  

#### <a name="context-and-problem"></a>Kontext a problém
Hello služby Table automaticky indexuje entity pomocí hello **PartitionKey** a **RowKey** hodnoty. To umožňuje klienta aplikace tooretrieve entity efektivně pomocí těchto hodnot. Například struktura tabulky hello vidíte níže, klientská aplikace pomocí tooretrieve dotaz bod entitu jednotlivých zaměstnanců pomocí názvu hello oddělení a identifikační číslo zaměstnance hello (hello **PartitionKey** a ** RowKey** hodnoty). Klienta můžete také načíst entity seřazené podle id zaměstnance v rámci každé oddělení.  

![][9]

Pokud chcete mít toofind toobe entitu zaměstnanců na základě hodnoty hello další vlastnosti, jako je například e-mailovou adresu, musíte použít méně efektivní prohledávání toofind oddílu shody. Je to proto, že služby table hello neposkytuje sekundární indexy. Kromě toho není žádná možnost toorequest seznam seřazen v jiném pořadí než zaměstnanců **RowKey** pořadí.  

Jsou očekávání velmi velký objem transakcí proti tyto entity a chcete toominimize hello riziko hello omezení vašeho klienta služby Table.  

#### <a name="solution"></a>Řešení
toowork kolem hello nedostatečná sekundární indexy, můžete uložit více kopií každé entity s každou kopírování pomocí různých **PartitionKey** a **RowKey** hodnoty. Pokud ukládáte entitu s hello struktury vidíte níže, můžete efektivně načíst zaměstnanec entit na základě id e-mailovou adresu nebo zaměstnanců. Hello předpon pro hello **PartitionKey**, "empid_" a "email_" umožňují tooidentify, kterého jste indexu má toouse pro dotaz.  

![][10]

Hello následující dvě kritéria filtru (jeden vyhledávání podle id zaměstnance a jeden vyhledávání pomocí e-mailové adresy) i zadejte bod dotazy:  

* $filter = (PartitionKey eq ' empid_Sales') a (RowKey eq '000223')
* $filter = (PartitionKey eq ' email_Sales') a (RowKey eq 'jonesj@contoso.com')  

Pokud vyhledat rozsahu entit zaměstnanců, můžete zadat rozsah id řazení zaměstnanců nebo rozsahu seřazeny v e-mailovou adresu pořadí pomocí dotazu pro entity s příslušnou předponu hello hello **RowKey**.  

* toofind všechny zaměstnance v hello hello oddělení prodeje s id zaměstnance v rozsahu hello **000100** příliš**000199** seřazeny ve zaměstnanec id pořadí použití: $filter = (PartitionKey eq ' empid_Sales') a (RowKey ge. 000100') a (RowKey le '000199')  
* toofind všechny zaměstnance hello oddělení prodeje hello s e-mailovou adresu, který začíná slovem "a" seřazených podle e-mailová adresa pořadí použití: $filter = (PartitionKey eq ' email_Sales') a (RowKey ge "a") a (RowKey lt "b")  

Upozorňujeme, že se syntaxí filtru hello použít ve výše uvedených příkladech hello se ze služby Table hello rozhraní REST API, další informace najdete v tématu [dotazu entity](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Abyste zajistili, že duplicitní položky nakonec byl konzistentní se navzájem pomocí hello [nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) toomaintain hello primární a sekundární index entity.  
* Table storage je relativně levný toouse, takže hello náklady režijní náklady na ukládání dat duplicitní by nemělo být závažný problém. Však měli vždy vyhodnoceny hello náklady návrhu na základě požadavků vaší předpokládaného úložiště a přidat pouze duplicitní položky toosupport hello dotazy, které budou spuštěny klientské aplikace.  
* Hello hodnota používaná pro hello **RowKey** musí být jedinečný pro každé entity. Zvažte použití hodnoty složeného klíče.  
* Odsazení číselných hodnot v hello **RowKey** (například id zaměstnance hello 000223), umožňuje opravte řazení a filtrování na základě horní a dolní meze.  
* Není nutné nutně tooduplicate všechny hello vlastnosti vaší entity. Například pokud hello dotazy, které e-mailu pomocí hello vyhledávací hello entity, které se adresu v hello **RowKey** nikdy nepotřebují stáří hello zaměstnance, tyto entit může mít hello strukturu:
  
  ![][11]
* Je obvykle lepší toostore duplicitních dat a ujistěte se, že můžete načíst všechna data hello, které je nutné se jeden dotaz než toolocate jeden dotaz toouse entitu s využitím hello sekundární index a jiné toolookup hello požadovaná data v primární index hello.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, když klientské aplikace potřebuje tooretrieve entity pomocí různých odlišné klíče, když klient potřebuje tooretrieve entity v jiné pořadí řazení, a tam, kde můžete identifikovat každé entity pomocí různých jedinečné hodnoty. Pomocí tohoto vzoru můžete tooavoid překročení omezení škálovatelnosti oddílu hello při provádění entity vyhledávání pomocí různých hello **RowKey** hodnoty.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern)  
* [Vzor Intra-partition sekundární index](#intra-partition-secondary-index-pattern)  
* [Složené klíče vzor](#compound-key-pattern)  
* [Transakce skupiny entity](#entity-group-transactions)  
* [Práce s typy heterogenní entit](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Nakonec byl konzistentní transakce vzor
Povolte nakonec byl konzistentní chování mezi hranice oddílů nebo hranice systému úložiště pomocí front Azure.  

#### <a name="context-and-problem"></a>Kontext a problém
EGTs povolit jednotlivé transakce v rámci více entit, které sdílejí hello stejným klíčem oddílu. Pro výkon a škálovatelnost, můžete se rozhodnout toostore entit, které mají požadavky konzistence v samostatné oddíly nebo v samostatné úložiště systému: v takové situaci nelze použít EGTs toomaintain konzistence. Například můžete mít požadavek toomaintain konzistence typu případné mezi:  

* Entity, které jsou uloženy ve dvou různých oddílů v hello stejné tabulky, v různých tabulek ve v jiným účtům úložiště.  
* Entity ukládají v hello služby Table a objekt blob do hello služby objektů Blob.  
* Entity ukládají do služby Table hello a soubor v systému souborů.  
* Entity uložit hello služby Table ještě indexovat pomocí služby Azure Search hello.  

#### <a name="solution"></a>Řešení
Pomocí fronty Azure můžete implementovat řešení, které nabízí konzistence typu případné mezi dva nebo více oddílů nebo úložných systémů.
tooillustrate to postupovat, předpokládá, že máte požadavek toobe možné tooarchive starší zaměstnanec entity. Starší zaměstnanec entity jsou zřídka dotaz a mají být vyloučeny z všechny aktivity, které pracují s aktuální zaměstnanci. tooimplement tento požadavek ukládáte aktivní zaměstnance v hello **aktuální** tabulky a původní zaměstnance v hello **archivu** tabulky. Archivace zaměstnanec vyžaduje toodelete hello entita z hello **aktuální** tabulky a přidejte hello entity toohello **archivu** tabulky, ale nemůžete použít tooperform EGT těchto dvou operací. tooavoid hello riziko, že selhání způsobí, že tooappear entity v obou nebo žádná z tabulek, hello archivu operace musí být nakonec byl konzistentní. Hello následující pořadí diagram popisuje hello kroky v této operaci. Podrobněji se poskytuje pro cesty výjimka v následující text hello.  

![][12]

Klient zahájí operaci archivu hello umístěním zprávu na fronty Azure, v tohoto příkladu tooarchive zaměstnance #456. Role pracovního procesu dotazuje hello fronty pro nové zprávy; v případě, že některou najde, čte uvítací zprávu a zanechá skrytá kopie ve frontě hello. role pracovního procesu Hello vedle načte kopii hello entity z hello **aktuální** tabulky, vloží kopii hello **archivu** tabulky a potom hello odstraní původní z hello **aktuální**tabulky. Nakonec pokud nedošlo k chybám z předchozích kroků hello, role pracovního procesu hello odstraní hello skryté zprávy z fronty hello.  

Krok 4 v tomto příkladu vloží hello zaměstnanec do hello **archivu** tabulky. V systému souborů se může přidat hello zaměstnanec tooa objektů blob v hello služby objektů Blob nebo soubor.  

#### <a name="recovering-from-failures"></a>Zotavení z chyb
Je důležité, hello operace v krocích **4** a **5** musí být *idempotent* v případě, že role pracovního procesu hello musí toorestart hello archivu operaci. Pokud používáte služby Table hello, pro krok **4** byste měli používat operace "vložení nebo nahrazení"; pro krok **5** byste měli používat "odstranit, pokud existuje" operace v hello klientské knihovny, kterou používáte. Pokud používáte jiné úložiště systému, musíte použít příslušné idempotent operace.  

Pokud role pracovního procesu hello nedokončí krok **6**, pak po na hello fronty se znovu zobrazí zprávu hello časový limit připravené pro tooreprocess tootry role pracovního procesu hello ho. role pracovního procesu Hello můžete zkontrolovat, kolikrát byl načten zprávu ve frontě hello a v případě potřeby příznak je zprávu "poškozených" pro zkoumání odesláním tooa oddělit fronty. Další informace o čtení fronty zpráv a kontrolou hello dequeue – počet najdete v tématu [získat zprávy](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Některé chyby ze služby Table a Queue hello jsou přechodné chyby a klientské aplikace by měla obsahovat toohandle logiku vhodný opakovat je.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Toto řešení neposkytuje pro izolace transakce. Například klienta může číst hello **aktuální** a **archivu** tabulky při role pracovního procesu hello mezi kroky **4** a **5**a najdete v části nekonzistentní zobrazení dat hello. Všimněte si, že hello data budou konzistentní nakonec.  
* Je nutné zajistit, že kroky 4 a 5 jsou idempotent v konzistence typu případné tooensure pořadí.  
* Je možné škálovat hello řešení pomocí více front a instance rolí pracovního procesu.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, pokud chcete, aby konzistence typu případné tooguarantee mezi entitami, které existují v různých oddílů nebo tabulky. Tento vzor tooensure konzistence typu případné pro operace můžete rozšířit na služby Table hello a hello služby objektů Blob a další zdroje dat Azure Storage jako systém souborů databáze nebo hello.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Transakce skupiny entity](#entity-group-transactions)  
* [Sloučení nebo nahradit](#merge-or-replace)  

> [!NOTE]
> Pokud izolace transakce je důležité tooyour řešení, měli byste zvážit přepracování vaší tooenable tabulky můžete toouse EGTs.  
> 
> 

### <a name="index-entities-pattern"></a>Vzor entity indexu
Udržujte index entity tooenable efektivní vyhledávání, která vrátí seznam entit.  

#### <a name="context-and-problem"></a>Kontext a problém
Hello služby Table automaticky indexuje entity pomocí hello **PartitionKey** a **RowKey** hodnoty. To umožňuje klienta aplikace tooretrieve entity efektivní použití bodu dotazu. Například pomocí struktura tabulky hello vidíte níže, klientská aplikace můžete efektivně načíst entity jednotlivých zaměstnanců pomocí hello oddělení název a id zaměstnance hello (hello **PartitionKey** a **RowKey **).  

![][13]

Pokud chcete mít tooretrieve toobe seznam zaměstnanec entit na základě hello hodnoty jiný jedinečný vlastnosti, například jejich poslední název, musíte použít méně efektivní oddíl kontroly shody toofind než pomocí toolook indexu je až přímo. Je to proto, že služby table hello neposkytuje sekundární indexy.  

#### <a name="solution"></a>Řešení
tooenable vyhledávání podle příjmení s struktura entity hello uvedené výše, musíte udržovat seznam ID zaměstnance. Pokud tooretrieve hello zaměstnanec entity s konkrétním poslední názvem, jako je například Petr, musíte nejprve vyhledat hello seznam ID zaměstnance pro zaměstnance s Petr jako své příjmení a následně načíst tyto entity zaměstnanců. Existují tři hlavní možnosti pro ukládání hello seznam ID zaměstnanců:  

* Používání úložiště blob.  
* Vytvořte index entity v hello stejné oddílu jako hello zaměstnanec entity.  
* Vytvořte index entity v samostatném oddílu nebo tabulky.  

<u>Možnost #1: Použití objektu blob storage</u>  

Pro první možnost hello vytvoříte objekt blob pro každý jedinečný příjmení a na každý objekt blob úložiště seznam hello **PartitionKey** (oddělení) a **RowKey** hodnoty (zaměstnanec id) pro zaměstnance, které mají to poslední Jméno. Při přidání nebo odstranění zaměstnanec měli byste zajistit, že je hello obsahu objektu blob relevantní hello nakonec byl konzistentní se hello zaměstnanec entity.  

<u>Možnost #2:</u> vytvořit index entity v hello stejného oddílu  

Druhá možnost hello použijte index entity, které ukládají hello následující data:  

![][14]

Hello **EmployeeIDs** vlastnost obsahuje seznam identifikátorů zaměstnanec pro zaměstnance s příjmení hello uložené v hello **RowKey**.  

Hello následující kroky popisují proces hello, které byste měli postupovat při přidávání nového zaměstnance Pokud používáte druhá možnost hello. V tomto příkladu přidáváme zaměstnance s Id 000152 a příjmení Petr v oddělení prodeje hello:  

1. Načtení entity hello index s **PartitionKey** hodnotu "Prodej" a hello **RowKey** hodnotu "Petr." Uložte hello ETag toouse této entity v kroku 2.  
2. Vytvoření skupiny transakci entity (tedy dávkovou operaci), která vloží novou entitu zaměstnanec hello (**PartitionKey** hodnotu "Prodej" a **RowKey** hodnotu "000152"), a aktualizace hello index entity (** PartitionKey** hodnotu "Prodej" a **RowKey** hodnotu "Petr") přidáním hello nové zaměstnance id toohello seznamu v poli EmployeeIDs hello. Další informace o transakcích skupiny entity najdete v tématu [Entity skupiny transakce](#entity-group-transactions).  
3. Pokud se transakce skupiny entity hello nezdaří z důvodu chyby optimistickou metodu souběžného (někdo jiný právě změnil hello index entity), budete potřebovat toostart přes v kroku 1 znovu.  

Podobné toodeleting přístup zaměstnanec můžete použít, pokud používáte hello druhou možnost. Změna příjmení zaměstnance je trochu složitější, protože budete potřebovat tooexecute transakci skupiny entity, která aktualizuje tři entity: hello zaměstnanec entita, hello index entity pro hello staré příjmení a hello index entity pro nové příjmení hello. Před provedením jakýchkoli změn v pořadí tooretrieve hello ETag hodnoty, které pak můžete tooperform hello aktualizací pomocí optimistickou metodu souběžného musí získat každé entity.  

Hello následující kroky popisují proces hello, které byste měli postupovat, když potřebujete toolook až všichni zaměstnanci hello s danou příjmení v oddělení, pokud používáte hello druhou možnost. V tomto příkladu jsme se vyhledá všechny zaměstnance hello se příjmení Petr v oddělení prodeje hello:  

1. Načtení entity hello index s **PartitionKey** hodnotu "Prodej" a hello **RowKey** hodnotu "Petr."  
2. Analyzovat hello seznam ID v poli EmployeeIDs hello zaměstnanců.  
3. Pokud potřebujete další informace o jednotlivých tito zaměstnanci (například jejich e-mailové adresy), načtení jednotlivých entit zaměstnanec hello pomocí **PartitionKey** hodnotu "Prodej" a **RowKey** hodnoty z Hello seznam zaměstnanců, které jste získali v kroku 2.  

<u>Možnost #3:</u> vytvořte index entity v samostatném oddílu nebo tabulky  

Třetí možnost hello použijte index entity, které ukládají hello následující data:  

![][15]

Hello **EmployeeIDs** vlastnost obsahuje seznam identifikátorů zaměstnanec pro zaměstnance s příjmení hello uložené v hello **RowKey**.  

Třetí možnost hello nemůžete použít EGTs toomaintain konzistence, protože hello index entity jsou v samostatném oddílu z hello zaměstnanec entit. Se ujistěte, že hello index entity jsou nakonec byl konzistentní se hello zaměstnanec entity.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Toto řešení vyžaduje alespoň dva dotazy tooretrieve odpovídající entity: jeden tooquery hello index entity tooobtain hello seznam **RowKey** hodnoty a následně dotazuje tooretrieve každé entity v seznamu hello.  
* Oznámeno, že jednotlivé entity má maximální velikost 1 MB, možnost #2 a možnost #3 v hello řešení předpokládá, že hello seznam ID zaměstnance pro danou příjmení se nikdy větší než 1 MB. Pokud hello seznam ID zaměstnance je pravděpodobně toobe větší než velikost 1 MB, použijte možnost #1 a ukládat data indexu hello v úložišti objektů blob.  
* Pokud použijete možnost #2 (s použitím EGTs toohandle přidávání a odstraňování zaměstnanci a změna zaměstnance příjmení), musíte zjistit Pokud hello objem transakcí bude postupovat hello omezení škálovatelnosti v daném oddílu. Pokud se jedná o případ hello, měli byste zvážit nakonec byl konzistentní řešení (možnost #1 nebo #3), které používá fronty toohandle hello aktualizace požadavky a umožňuje vám toostore vaše index entity v samostatném oddílu z hello zaměstnanec entit.  
* Možnost #2 v tomto řešení se předpokládá, má toolook nahoru podle příjmení v rámci oddělení: například chcete tooretrieve seznam zaměstnancům příjmení Petr v oddělení prodeje hello. Pokud chcete mít toolook toobe až všechny zaměstnance hello se příjmení Petr napříč celou organizaci hello, použijte možnost #1 nebo možnost #3.
* Můžete implementovat řešení na základě fronty, který doručí konzistence typu případné (viz hello [nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) podrobnosti).  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, pokud chcete, aby toolookup sady entit, že všechny společné hodnotu vlastnosti, například všechny zaměstnance se hello příjmení Petr sdílet.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Složené klíče vzor](#compound-key-pattern)  
* [Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern)  
* [Transakce skupiny entity](#entity-group-transactions)  
* [Práce s typy heterogenní entit](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Vzor denormalization
Kombinování souvisejících dat společně v jedné entity tooenable tooretrieve všechny hello dat je třeba pomocí dotazu jediný bod.  

#### <a name="context-and-problem"></a>Kontext a problém
V relační databázi obvykle normalizaci odstranění duplicitních dat tooremove výsledkem dotazy, které načtení dat z více tabulek. Pokud jste normalizaci data do tabulek Azure, musíte nastavit více přenosů z hello klienta toohello server tooretrieve související data. Například s struktura tabulky hello zobrazené níže potřebovat dva zaokrouhlit služebních cest tooretrieve hello podrobnosti o oddělení: jeden toofetch hello oddělení entita, která obsahuje id hello manager a pak další požadavek toofetch hello manažera podrobnosti v zaměstnanec entita.  

![][16]

#### <a name="solution"></a>Řešení
Místo ukládání dat hello v dvě samostatné entity, denormalize hello data a ponechat si kopii hello správci v entitě oddělení hello. Například:  

![][17]

S entitami oddělení uložené s těmito vlastnostmi můžete nyní načíst všechny hello podrobnosti, které potřebujete o oddělení použití bodu dotazu.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Není náklady režie spojená s ukládáním některá data dvakrát. Hello výkonu dávky (vyplývající z méně požadavků toohello úložiště služby) většinou převáží hello okrajového vzrůst náklady na úložiště (a náklady na tento je částečně posun omezením v hello počet transakcí vyžadují toofetch hello podrobnosti oddělení).  
* Musíte udržovat hello konzistence hello dvě entity, které obsahují informace o správci. Dokáže zpracovat hello konzistence problém pomocí EGTs tooupdate více entit v rámci jedné transakce atomic: v tomto případě hello oddělení entity a hello zaměstnanec entity pro hello oddělení manager jsou uloženy v hello stejného oddílu.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, pokud potřebujete často toolook si související informace. Tento vzor snižuje hello počet dotazů, které váš klient musí udělat tooretrieve hello daty, která vyžaduje.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Složené klíče vzor](#compound-key-pattern)  
* [Transakce skupiny entity](#entity-group-transactions)  
* [Práce s typy heterogenní entit](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Složené klíče vzor
Použití složené **RowKey** hodnoty tooenable klienta toolookup související data pomocí dotazu jediný bod.  

#### <a name="context-and-problem"></a>Kontext a problém
V relační databázi, je poměrně přirozené toouse spojení v dotazech tooreturn související kusy toohello klientských dat v jednom dotazu. Můžete například použít hello zaměstnanec id toolook seznam entit v relaci, které obsahují výkonu a zkontrolujte data pro zaměstnance.  

Předpokládejme, že zaměstnanec entity jsou ukládána do služby Table hello pomocí hello strukturu:  

![][18]

Musíte taky toostore historická data týkající se tooreviews a výkon pro každý rok hello zaměstnanec fungovala pro vaši organizaci a potřebujete mít tooaccess toobe tyto informace o rok. Jednou z možností je toocreate jiné tabulky, který uchovává entity s hello strukturu:  

![][19]

Všimněte si, že s tímto přístupu můžete rozhodnout tooduplicate některé informace (například křestní jméno a příjmení) v nové entity tooenable hello tooretrieve můžete svá data pomocí jedné žádosti. Nelze však udržovat silnou konzistenci, protože nelze použít dvě entity EGT tooupdate hello atomicky.  

#### <a name="solution"></a>Řešení
Uložte nový typ entity v původní tabulky pomocí entity hello strukturu:  

![][20]

Všimněte si, jak hello **RowKey** je nyní složený klíč tvořená hello identifikační číslo zaměstnance a hello rok hello zkontrolujte data, která vám umožní tooretrieve hello zaměstnance výkon a kontrolovat data pomocí jedné žádosti pro jednu entitu.  

Hello následující příklad popisuje, jak můžete načíst všechna data hello zkontrolujte pro konkrétního zaměstnance (například zaměstnanci 000123 v oddělení prodeje hello):  

$filter = (PartitionKey eq 'Prodej') a (RowKey ge 'empid_000123') a (RowKey lt 'empid_000124') & $select = RowKey, Manager hodnocení, sdílené hodnocení a komentáře  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Měli byste použít vhodný oddělovací znak, který umožňuje snadno tooparse hello **RowKey** hodnota: například **000123_2012**.  
* Také ukládáte tuto entitu v hello stejně jako ostatní entity, které obsahují data týkající se hello oddílu stejného zaměstnance, což znamená, můžete použít silnou konzistenci EGTs toomaintain.
* Měli byste zvážit, jak často bude dotaz hello data toodetermine zda tento vzor je vhodné.  Například pokud bude mít přístup zřídka hello zkontrolujte data a hello hlavní data zaměstnance často byste měli mít jako samostatné entity.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, pokud potřebujete toostore jeden nebo více související entity dotazu často.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Transakce skupiny entity](#entity-group-transactions)  
* [Práce s typy heterogenní entit](#working-with-heterogeneous-entity-types)  
* [Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Vzor protokolu poškozené databáze
Načtení hello * n * entity naposledy přidat oddíl tooa pomocí **RowKey** hodnotu, která seřadí zpětné datum a čas pořadí.  

#### <a name="context-and-problem"></a>Kontext a problém
Běžným požadavkem je, být schopný tooretrieve hello vytvořeno naposled entity, například hello deset nejnovější výdajů deklarace identity odeslaný zaměstnanec. Tabulka dotazuje podporu **$top** dotaz operaci tooreturn hello nejprve * n * entit ze sady: není žádná ekvivalentní dotazu operaci tooreturn hello posledních n entity v sadě.  

#### <a name="solution"></a>Řešení
Entity hello úložiště pomocí **RowKey** že přirozeně řazení obráceným datum a čas pořadí pomocí tak hello poslední položka je vždy hello první z nich v tabulce hello.  

Například může tooretrieve toobe hello deset nejnovější výdajů deklarací odeslaný zaměstnanec, můžete použít hodnotu zpětné značek odvozené od hello aktuální datum a čas. Hello následující ukázka kódu C# ukazuje jeden ze způsobů toocreate vhodný hodnota "obrácený rysky" pro **RowKey** , seřadí z hello nejnovější toohello nejstarší:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

Můžete získat zpět toohello hodnotu data a času pomocí hello následující kód:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

dotaz tabulky Hello vypadá takto:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Hodnota zpětné značek hello s úvodní nuly musí odsadí tooensure hello řetězcovou hodnotu seřadí podle očekávání.  
* Musíte být vědomi hello škálovatelnost cílů na úrovni hello oddílu. Dávejte pozor, vytváření oddílů aktivního bodu.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Použijte tento vzor, když potřebujete tooaccess entity v pořadí zpětné datum a čas, nebo když potřebujete tooaccess hello naposledy přidat entity.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Předřadit / append proti vzor](#prepend-append-anti-pattern)  
* [Načtení entit](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Vzor velkému odstranění
Povolit odstranění hello k velkému počtu entity uložením všechny entity hello k odstranění souběžných vlastní samostatné tabulky; Odstranění entity hello odstraněním hello tabulky.  

#### <a name="context-and-problem"></a>Kontext a problém
Mnoho aplikací odstranit stará data, která už nepotřebuje toobe dostupné tooa klientská aplikace nebo aplikace hello má archivovaných tooanother paměťového média. Obvykle identifikovat taková data data: můžete mít například záznamy a požadavek toodelete všech žádostí o přihlášení, které jsou starší než 60 dní.  

Jeden možné návrhu je toouse hello datum a čas požadavku hello přihlášení v hello **RowKey**:  

![][21]

Tento přístup zabraňuje hotspotům oddílu, protože aplikace hello můžete vložit a odstranit entity přihlášení pro každého uživatele v samostatném oddílu. Však tento přístup může být nákladná a časově náročné Pokud máte velký počet entit, protože je třeba nejprve tooperform tabulku kontroly v pořadí tooidentify všechny entity toodelete hello a pak je nutné odstranit každé staré entity. Všimněte si, že můžete snížit hello počet zpátečních cest toohello server vyžaduje toodelete hello staré entit podle dávkování více žádosti o odstranění do EGTs.  

#### <a name="solution"></a>Řešení
Do samostatné tabulky použijte pro každý den pokusů o přihlášení. Návrh entit hello výše tooavoid hotspotům můžete použít při vkládání entity a odstranění starší entity je nyní prostě odstranění jedna tabulka každý den (jedno úložiště operace) namísto hledání a odstraňování stovky a tisíce jednotlivé přihlášení entity každý den.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Podporuje váš návrh další způsoby, které vaše aplikace bude používat data hello například vyhledávání konkrétních entit, propojení s jinými dat nebo generování agregační informace?  
* Návrhu vyhnout aktivní body při vkládání nové entity  
* Očekávat zpoždění, pokud chcete, aby tooreuse hello stejný název tabulky po jeho odstranění. Je lepší tooalways používat tabulky jedinečné názvy.  
* Očekávají, že některé omezení při prvním použití nové tabulky při hello služby Table zjišťuje hello přístupové vzorce a distribuuje hello oddíly mezi uzly. Měli byste zvážit, jak často potřebujete toocreate nové tabulky.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Použít tento vzor, až budete mít k velkému počtu entit, které je nutné odstranit v hello stejnou dobu.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Transakce skupiny entity](#entity-group-transactions)
* [Úprava entity](#modifying-entities)  

### <a name="data-series-pattern"></a>Řada vzorek dat
Dokončení datové řady úložiště jedna entita toominimize hello počet požadavků, které provedete.  

#### <a name="context-and-problem"></a>Kontext a problém
Obvyklým scénářem je pro aplikací toostore řadu dat, obvykle musí tooretrieve všechny najednou. Vaše aplikace může například záznam kolik zasílání Rychlých zpráv každý zaměstnanec odešle každou hodinu a potom pomocí této informace tooplot, kolik zpráv každý uživatel odešlou přes hello předchozích 24 hodin. Jeden návrhu může být toostore 24 entity pro každý zaměstnanec:  

![][22]

V tomto návrhu můžete snadno vyhledat a aktualizace hello entity tooupdate pro každý zaměstnanec vždy, když aplikace hello je hodnota počtu zpráv tooupdate hello. Ale tooretrieve hello informace tooplot graf hello aktivity pro hello předchozích 24 hodin, je nutné ji načíst 24 entity.  

#### <a name="solution"></a>Řešení
Použijte následující návrh s počtem samostatné vlastnost toostore hello zpráva pro každou hodinu hello:  

![][23]

V tomto návrhu můžete počet zpráv hello tooupdate operaci sloučení pro zaměstnance pro zadané hodiny. Teď můžete načíst všechny hello informace, které budete potřebovat graf hello tooplot pomocí žádosti pro jednu entitu.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Pokud dokončení datové řady nevejde do jedné entity (entita může mít too252 vlastností), použijte alternativní úložišti například objekt blob.  
* Pokud máte víc klientů současně aktualizaci entity, budete potřebovat toouse hello **značka ETag** optimistickou metodu souběžného tooimplement. Pokud máte mnoho klientů, můžete se setkat vysoké kolizí.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, pokud potřebujete tooupdate a načíst data řady přidružené jednotlivých entit.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Vzor velkých entit](#large-entities-pattern)  
* [Sloučení nebo nahradit](#merge-or-replace)  
* [Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) (Pokud ukládáte hello datové řady do objektu BLOB)  

### <a name="wide-entities-pattern"></a>Vzor široké entity
Použití více fyzických entit toostore logických entit s více než 252 vlastností.  

#### <a name="context-and-problem"></a>Kontext a problém
Jednotlivých entit může mít více než 252 vlastností (s výjimkou vlastnosti povinné systému hello) a nelze uložit více než 1 MB dat celkem. V relační databázi by obvykle získáte zaokrouhlí žádné omezení velikosti hello řádku přidáním nové tabulky a vynucování relace 1: 1 mezi nimi.  

#### <a name="solution"></a>Řešení
Pomocí služby Table hello, můžete uložit více entit toorepresent objekt jeden velký podnik s více než 252 vlastností. Například pokud chcete toostore počet hello počet Rychlých zpráv odeslaných každý zaměstnanec pro hello posledních 365 dnů, můžete použít následující návrh, který používá dvě entity s různými schématy hello:  

![][24]

Pokud potřebujete toomake změnu, která vyžaduje aktualizaci obou tookeep entity, je synchronizovány mezi sebou můžete použít EGT. Počet zpráv tooupdate hello jednom sloučení operace, jinak hodnota můžete použít pro určitý den. všechny hello data pro jednotlivé zaměstnance musí načíst obě entity, které můžete provést dva efektivní požadavky, které používají oba tooretrieve **PartitionKey** a **RowKey** hodnotu.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Načítání dokončení logická entita zahrnuje alespoň dva transakce úložiště: jeden tooretrieve fyzická entita.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte v případě potřeby toostore entity, jejichž velikost nebo počet vlastností překračuje hello limity pro jednotlivé entity v hello služba Table.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Transakce skupiny entity](#entity-group-transactions)
* [Sloučení nebo nahradit](#merge-or-replace)

### <a name="large-entities-pattern"></a>Vzor velkých entit
Použijte hodnoty velký vlastností toostore úložiště objektů blob.  

#### <a name="context-and-problem"></a>Kontext a problém
Jednotlivé entity Nejde uložit více než 1 MB dat celkem. Pokud jeden nebo několik vlastnosti ukládá hodnoty, které způsobí hello celková velikost vaší entity tooexceed tuto hodnotu, nemůžete uložit celý entity hello hello služby Table.  

#### <a name="solution"></a>Řešení
Pokud vaší entity překračuje 1 MB velikost, protože jeden nebo více vlastností obsahovat velké množství dat, můžete ukládání dat v hello služby objektů Blob a potom uložte hello adresu hello blob ve vlastnosti v entitě hello. Například můžete ukládat hello fotografie zaměstnanec v úložišti objektů blob a uložit fotografii toohello odkaz hello **fotografií** vlastnosti vaší entity zaměstnanců:  

![][25]

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* konzistence typu případné toomaintain mezi hello entity v hello služby Table a hello data v hello služby objektů Blob, použijte hello [nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern) toomaintain vaší entity.
* Načítání úplnou entitu zahrnuje alespoň dva transakce úložiště: jednu entitu hello tooretrieve a jeden tooretrieve hello data objektů blob.  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Tento vzor použijte, pokud budete potřebovat toostore entity, jehož velikost překračuje hello limity pro jednotlivé entity v služby Table hello.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Nakonec byl konzistentní transakce vzor](#eventually-consistent-transactions-pattern)  
* [Vzor široké entity](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Předřazení připojit proti vzor
Zvýšení škálovatelnosti, když máte k velkému počtu vloží tak, že se vloží hello napříč více oddílů.  

#### <a name="context-and-problem"></a>Kontext a problém
Přidáním nebo připojování entity tooyour uložené entity obvykle výsledkem aplikace hello přidání nové entity toohello první nebo poslední oddílu pořadí oddílů. V takovém případě všechny hello vložení v každém okamžiku jsou probíhající v hello stejného oddílu, vytváření aktivní bod, který brání Vyrovnávání zatížení služby table hello vloží mezi několika uzly a což může způsobit vaše aplikace toohit hello škálovatelnost cíle pro oddíl. Například pokud máte aplikaci, která zaměstnanci, přístup k síťové protokoly a prostředků, pak může mít za následek strukturu entity, jak je uvedeno níže hello stane aktivní oblast, pokud hello objem transakcí dosáhne hello škálovatelnost cíl pro oddíl aktuální hodiny jednotlivé oddílu:  

![][26]

#### <a name="solution"></a>Řešení
Hello následující strukturu alternativní entity zabraňuje v žádné konkrétní oddílu aktivní bod jako hello aplikace protokoly událostí:  

![][27]

Všimněte si tento příklad, jak oba hello **PartitionKey** a **RowKey** jsou složeného klíče. Hello **PartitionKey** používá hello oddělení a zaměstnanci id toodistribute hello protokolování napříč více oddílů.  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Vezměte v úvahu hello následující body při rozhodování o tom, jak tooimplement tento vzor:  

* Nepodporuje hello alternativní klíče struktura, která zabraňuje vytváření aktivní oddíly na vložení efektivně dotazů hello podpory umožňuje klientské aplikace?  
* Znamená předpokládaného svazku transakcí jsou cíle škálovatelnosti hello pro jednotlivé oddíl pravděpodobně tooreach a omezeny službou hello úložiště?  

#### <a name="when-toouse-this-pattern"></a>Když toouse tento vzor
Vzor proti předřazení připojit hello Vyhněte se při svazku transakcí je pravděpodobně tooresult v omezení šířky pásma službou hello úložiště při přístupu k aktivní oddíl.  

#### <a name="related-patterns-and-guidance"></a>Související vzory a pokyny
Hello následující vzory a pokyny může být také důležité při implementaci tohoto vzoru:  

* [Složené klíče vzor](#compound-key-pattern)  
* [Vzor protokolu poškozené databáze](#log-tail-pattern)  
* [Úprava entity](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Proti vzorek dat protokolu
Obvykle použijte hello služby objektů Blob místo hello data protokolu toostore služby Table.  

#### <a name="context-and-problem"></a>Kontext a problém
Případ použití běžné pro data protokolu je tooretrieve výběr položky protokolu pro konkrétní datum a čas rozsah: například chcete toofind všechny hello chyba a kritické zprávy, které vaše aplikace protokolují mezi 15:04 a 15:06 na konkrétní datum. Nechcete toouse hello datum a čas hello protokolu zpráv toodetermine hello oddílu uložit protokolu entity k: aby výsledky do aktivního oddílu vzhledem k tomu, že v každém okamžiku budou sdílet všechny entity protokolu hello hello stejné **PartitionKey** hodnota (části hello [Prepend/připojovat proti vzor](#prepend-append-anti-pattern)). Například hello následující schéma entity pro zprávu protokolu dochází v oddílu aktivní, protože aplikace hello zapíše všechny zprávy protokolu pro hello aktuální datum a hodinu toohello oddíl:  

![][28]

V tomto příkladu hello **RowKey** zahrnuje hello datum a čas hello protokolu zpráv tooensure zprávy protokolu jsou uložená data a času řazení a zahrnuje id zprávy v případě, že více zpráv protokolu sdílet hello stejná data a času.  

Další možností je toouse **PartitionKey** zajistí, že aplikace hello zapíše zprávy napříč celou řadu oddíly. Například pokud hello zdroj zprávy protokolu hello poskytuje způsob toodistribute zprávy napříč mnoha oddílů, můžete použít následující schéma entity hello:  

![][29]

Hello problém s tímto schématem je ale, že tooretrieve všechny hello zprávy protokolu pro konkrétní časové rozpětí musí prohledávat každý oddíl v tabulce hello.

#### <a name="solution"></a>Řešení
Hello předchozí část zvýrazněná hello problém při toouse hello položky protokolu toostore služby tabulky a navrhované dva nevyhovující, návrhy. Jedno řešení vedla tooa aktivní oddíl s hello riziko nízký výkon zápis zpráv protokolu; Hello jiné řešení výsledkem dotaz nízký výkon z důvodu hello požadavek tooscan každého oddílu ve zprávách protokolu tooretrieve hello tabulky pro konkrétní časové období. Úložiště BLOB nabízí lepší řešení pro tento typ scénáře a jedná se o tom, jak Azure Storage Analytics úložiště hello protokolu data shromáždí.  

Tato část popisuje, jak analytika úložiště ukládá data protokolu v úložišti objektů blob jako ilustraci dat toostoring přístup, která obvykle dotazování podle rozsahu.  

Analytika úložiště ukládá zprávy protokolu ve formátu odděleného do více objektů BLOB. Formát odděleného Hello snadno pro klienta aplikace tooparse hello data ve zprávě protokolu hello.  

Analytika úložiště používá vytváření názvů pro objekty BLOB, které umožňuje toolocate hello blob (nebo objekty BLOB) obsahující zprávy protokolu hello, pro které chcete najít. Například objekt blob s názvem "queue/2014/07/31/1800/000001.log" obsahuje zprávy protokolu, které se týkají služby front toohello hello hodinu od 18:00 na 31 červenec 2014. Hello "000001" označuje, že se jedná hello první soubor protokolu pro toto období. Analytika úložiště taky zaznamenává hello časová razítka hello první a poslední protokolování zpráv, které jsou uložené v souboru hello jako součást metadata objektu blob hello. Hello rozhraní API pro objekt blob úložiště umožňuje vyhledat objekty BLOB v kontejneru na základě předpony názvu: toolocate všech objektů BLOB hello, které obsahují fronty protokolovat data hello hodinu od 18:00, můžete použít hello předponu "fronty/2014/07/31/1 800."  

Analytika úložiště interně ukládá zprávy protokolu vyrovnávací paměti a pravidelně aktualizuje příslušné blob hello nebo vytvoří nový s hello nejnovější dávku položky protokolu. Tím se snižuje počet hello zápisů proveďte toohello služby objektů blob.  

Pokud implementujete řešení podobné ve vaší vlastní aplikaci, musíte zvážit, jak toomanage hello kompromis mezi spolehlivost (zápis jako Odehrává se každý záznam tooblob úložiště) a náklady a škálovatelnost (ukládání do vyrovnávací paměti aktualizací ve vaší aplikaci a zápis je tooblob úložiště v dávkách).  

#### <a name="issues-and-considerations"></a>Problémy a důležité informace
Mějte na paměti následující body při rozhodování o tom, jak toostore protokolovat data hello:  

* Pokud vytvoříte návrh tabulky, který zabraňuje potenciální aktivní oddíly, můžete zjistit, data protokolu nelze efektivní přístup.  
* tooprocess protokolovat data, klient často potřebuje tooload mnoho záznamů.  
* I když je často strukturovaná data protokolu, úložiště objektů blob může být lepším řešením.  

### <a name="implementation-considerations"></a>Důležité informace o implementaci
Tato část popisuje některé aspekty toobear hello pamatujte při implementaci hello vzory popsané v předchozích částech hello. Většina Tato část používá příklady napsané v C#, které používají hello Klientská knihovna pro úložiště (verze 4.3.0 v době psaní hello).  

### <a name="retrieving-entities"></a>Načtení entit
Jak je popsáno v části hello [návrhu pro dotazování](#design-for-querying), hello nejúčinnější dotaz je dotaz bodu. Ale v některých scénářích musíte tooretrieve více entit. Tato část popisuje některé běžné entity tooretrieving přístupy pomocí hello Klientská knihovna pro úložiště.  

#### <a name="executing-a-point-query-using-hello-storage-client-library"></a>Spuštění dotazu na bodu pomocí hello Klientská knihovna pro úložiště
Hello nejjednodušší způsob, jak tooexecute bodu dotazu je toouse hello **načíst** tabulky operace, jak je znázorněno v hello následující fragment kódu jazyka C#, který načte entit **PartitionKey** hodnotu "prodej" a ** RowKey** hodnoty "212":  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

Všimněte si, jak tento příklad očekává hello entity ho načte toobe typu **EmployeeEntity**.  

#### <a name="retrieving-multiple-entities-using-linq"></a>Načítání více entit pomocí LINQ
Můžete načíst pomocí LINQ s Klientská knihovna pro úložiště a zadat dotaz s více entit **kde** klauzule. tooavoid prohledávání tabulky, by měla vždycky obsahovat hello **PartitionKey** hodnota v hello kde klauzule a pokud je to možné hello **RowKey** hodnotu tooavoid prohledávání tabulky a oddíl. Hello služby table podporuje omezenou sadu operátory porovnání (větší než, větší než nebo rovna méně než je menší než nebo rovna, stejné a není rovno) toouse v hello kde klauzule. Hello následující fragment kódu jazyka C# vyhledá všechny zaměstnance hello jejichž poslední název začíná na "B" (za předpokladu, že hello **RowKey** úložiště hello příjmení) v oddělení prodeje hello (za předpokladu, že hello **PartitionKey** ukládá název oddělení hello):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

Všimněte si, jak hello dotaz Určuje, jak **RowKey** a **PartitionKey** tooensure lepší výkon.  

Hello následující ukázka kódu zobrazuje ekvivalentní funkce pomocí rozhraní fluent API hello (Další informace o rozhraní fluent API obecně platí, najdete v části [osvědčené postupy pro navrhování rozhraní Fluent API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> Hello ukázka vnořena více **CombineFilters** metody tooinclude hello tři filtr podmínek.  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Načítání velký počet entit z dotazu
Optimální dotaz vrátí jednotlivých entit na základě **PartitionKey** hodnotu a **RowKey** hodnotu. Ale v některých případech může obsahují tooreturn požadavek mnoho entit z hello stejné oddílu, nebo dokonce z mnoha oddílů.  

V takových případech byste měli vždy plně otestovat hello výkon aplikace.  

Dotazy na data služby table hello může vrátit maximálně 1000 entit najednou a může spustit maximálně pět sekund. Pokud hello sadu výsledků dotazu obsahuje více než 1 000 entity, pokud hello dotaz nebyl dokončen v pět sekund, nebo pokud dotaz hello protne hello oddílu hranic, vrátí hello služby Table tooenable token pokračování hello hello toorequest klientské aplikace Další sady entit. Další informace o tom, jak pokračování tokeny pracovní najdete v tématu [časový limit dotazu a Paginování](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Pokud používáte hello Klientská knihovna pro úložiště, může ho automaticky jako vrátí entity z hello služby Table pro vás zpracovávat pokračování tokeny. Hello následující C# ukázka kódu pomocí hello Klientská knihovna pro úložiště automaticky zpracovává pokračování tokeny služby table hello vrátí funkce je v odpovědi:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

Následující kód C# Hello zpracovává pokračování tokeny explicitně:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Pomocí explicitně pokračování tokenů, můžete řídit, kdy aplikace načítá hello další segment dat. Například pokud klientské aplikace umožňuje uživatelům toopage prostřednictvím hello entity uložené v tabulce, uživatel může rozhodnout není toopage prostřednictvím všechny hello entity načíst dotazem hello tak vaše aplikace použijte pouze pokračování tokenu tooretrieve hello další Segment po hello uživatel měl dokončení stránkování prostřednictvím všechny hello entity v aktuálním segmentu hello. Tento přístup má několik výhod:  

* Umožní vám toolimit hello množství dat tooretrieve z hello služby Table a přesunutí přes síť hello.  
* Umožňuje vám tooperform asynchronní vstupně-výstupní operace v rozhraní .NET.  
* Umožní vám tooserialize hello pokračovací token toopersistent úložiště, můžete pokračovat v hello události při selhání aplikace.  

> [!NOTE]
> Token pokračování obvykle vrátí segment obsahující 1000 entity, i když může být méně. Toto je také hello případ, pokud omezíte hello počet položek dotaz vrátí pomocí **trvat** tooreturn hello první n entit, které odpovídají zadaným kritériím vyhledávání: služby table hello může vrátit segment obsahující méně než n entity podél s tooenable tokenu pokračování je tooretrieve hello zbývající entity.  
> 
> 

Hello následující C# – kód ukazuje, jak toomodify hello počet entit, vrátila uvnitř segment:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Projekce na straně serveru
Jedna entita mají too255 vlastností a být až velikost too1 MB. Při dotazování tabulky hello a načtení entit, možná nebudete potřebovat všechny vlastnosti hello a zbytečně přenosu dat se můžete vyhnout (toohelp snížit latenci a náklady). Můžete vytvořit serverovou projekce tootransfer jenom hello vlastnosti, které potřebujete. Hello následující příklad je právě hello načte **e-mailu** vlastnost (spolu s **PartitionKey**, **RowKey**, **časové razítko**a ** Značka ETag**) z entit hello vybrané dotazem hello.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

Všimněte si, jak hello **RowKey** hodnota je k dispozici, přestože nebyl součástí hello seznam tooretrieve vlastnosti.  

### <a name="modifying-entities"></a>Úprava entity
Hello Klientská knihovna pro úložiště umožňuje toomodify vaší entity uloží v služby table hello vkládání, odstraňování a aktualizaci entity. Můžete EGTs toobatch více insert, update a operace odstranění společně tooreduce hello počet zpátečních cest vyžaduje a zlepšit výkon hello vašeho řešení.  

Mějte na paměti, že výjimky vydané když hello Klientská knihovna pro úložiště provede EGT obvykle zahrnout hello index hello entity, která způsobila, že hello batch toofail. To je užitečné při ladění kódu, který používá EGTs.  

Měli byste také zvážit, jak váš návrh ovlivňuje způsob, jakým klientské aplikace zpracovává operace souběžnosti a aktualizace.  

#### <a name="managing-concurrency"></a>Správa souběžnosti
Ve výchozím nastavení, implementuje optimistickou metodu souběžného zpracování kontroluje úrovni hello jednotlivých entit pro služby table hello **vložit**, **sloučení**, a **odstranit** operace, Přestože je možné pro hello tooforce klienta tabulky služby toobypass těchto kontrol. Další informace o správě služby table hello souběžnosti najdete v tématu [Správa souběžnost v Microsoft Azure Storage](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Sloučení nebo nahradit
Hello **nahradit** metoda hello **TableOperation** třída vždycky nahradí hello úplnou entitu v hello služby Table. Pokud nezadáte vlastnost v žádosti o hello tuto vlastnost existuje v entitě hello uložené, žádost hello odebere, vlastnost z hello uložené entity. Pokud chcete, aby tooremove vlastnost explicitně z uložené entity, musí obsahovat všech vlastností v žádosti o hello.  

Můžete použít hello **sloučení** metoda hello **TableOperation** třída tooreduce hello množství dat poslat služby Table toohello když chcete, aby tooupdate entity. Hello **sloučení** metoda nahradí všechny vlastnosti v entitě hello uložené hodnoty vlastností z entity hello součástí hello požadavek, ale nechá beze změn ukládat všechny vlastnosti v hello entita, která nejsou zahrnuta v žádosti o hello. To je užitečné, pokud máte velké entity a stačí tooupdate malý počet vlastností v požadavku.  

> [!NOTE]
> Hello **nahradit** a **sloučení** metody nezdaří, pokud hello entita neexistuje. Jako alternativu, můžete použít hello **InsertOrReplace** a **InsertOrMerge** metody, které vytvářejí nové entity, pokud neexistuje.  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a>Práce s typy heterogenní entit
je Hello služby Table *bez schématu* tabulky úložiště, která znamená, že na jednotlivé tabulky můžete ukládat entity více typů poskytuje flexibilitu při návrhu. Hello následující příklad ilustruje tabulku ukládání zaměstnanců a entity oddělení:  

<table>
<tr>
<th>Klíč oddílu</th>
<th>RowKey</th>
<th>časové razítko</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Všimněte si, že každá entita musí mít dál **PartitionKey**, **RowKey**, a **časové razítko** hodnoty, ale mohou mít všechny sadu vlastností. Kromě toho není nic tooindicate hello typ entity nerozhodnete toostore někde tyto informace. Existují dvě možnosti pro určení typu entity hello:  

* Předřazení hello entity typu toohello **RowKey** (nebo případně hello **PartitionKey**). Například **EMPLOYEE_000123** nebo **DEPARTMENT_SALES** jako **RowKey** hodnoty.  
* Použijte samostatné toorecord hello entity typ vlastnosti jsou uvedené v tabulce hello níže.  

<table>
<tr>
<th>Klíč oddílu</th>
<th>RowKey</th>
<th>časové razítko</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Typ entity</th>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td>Zaměstnanec</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Typ entity</th>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td>Zaměstnanec</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Typ entity</th>
<th>DepartmentName</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Oddělení</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Typ entity</th>
<th>FirstName</th>
<th>Příjmení</th>
<th>Věk</th>
<th>E-mail</th>
</tr>
<tr>
<td>Zaměstnanec</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

první možnost Hello předřazení hello entity typu toohello **RowKey**, je užitečné, pokud je možné, že dvě entity různých typů pravděpodobně hello stejnou hodnotu klíče. Skupiny také entity hello stejný typ společně v oddílu hello.  

Hello technik popsaných v této části jsou obzvláště důležité toohello diskusní [vztahy dědičnosti](#inheritance-relationships) výše v tomto průvodci v části hello [modelování vztahů](#modelling-relationships).  

> [!NOTE]
> Měli byste zvážit číslo verze hello entity hodnota tooenable klienta aplikace tooevolve objektů POCO objekty typu a práci s různými verzemi.  
> 
> 

Hello zbytek Tato část popisuje některé funkce hello v hello Klientská knihovna pro úložiště, které usnadňují práce s více typy entit v hello stejné tabulky.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Načítání typy heterogenní entit
Pokud používáte hello Klientská knihovna pro úložiště, máte tři možnosti pro práci s více typy entit.  

Pokud znáte hello typ entity hello uložené s konkrétním **RowKey** a **PartitionKey** hodnoty, potom můžete zadat typ entity hello při načtení hello entity, jak je znázorněno v hello předchozí dva příklady který načtení entity typu **EmployeeEntity**: [provádění dotazu bodu pomocí hello Klientská knihovna pro úložiště](#executing-a-point-query-using-the-storage-client-library) a [načítání více entit pomocí LINQ](#retrieving-multiple-entities-using-linq).  

Druhá možnost Hello je toouse hello **DynamicTableEntity** typu (kontejner objektů) namísto konkrétní typ entity objektů POCO (Tato možnost může také zvýšit výkon, protože není bez nutnosti tooserialize a příliš deserializovat hello entity. NET typy). načte více entit různých typů z tabulky hello Hello potenciálně následující kód C#, ale vrací všechny entity jako **DynamicTableEntity** instance. Poté použije hello **EntityType** hello typ vlastnosti toodetermine Každá entita:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Všimněte si, že tooretrieve dalších vlastností, je nutné použít hello **TryGetValue** metodu hello **vlastnosti** vlastnost hello **DynamicTableEntity** třídy.  

Třetí možnost je toocombine pomocí hello **DynamicTableEntity** typu a **EntityResolver** instance. To vám umožní typy objektů POCO toomultiple tooresolve v hello stejný dotaz. V tomto příkladu hello **EntityResolver** delegát používá hello **EntityType** toodistinguish vlastnost mezi hello dva typy entit, které hello dotaz vrací. Hello **vyřešit** metoda používá hello **překladač** delegovat tooresolve **DynamicTableEntity** instance příliš**TableEntity** instance.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a>Úprava typy heterogenní entit
Není nutné tooknow typ hello entity toodelete ho a budete vždy vědět hello typ entity, když vložíte ho. Můžete však použít **DynamicTableEntity** zadejte tooupdate entity, aniž by věděly, její typ a bez použití třídu entity objektů POCO. Hello následující ukázka kódu načte jedné entity a kontroluje hello **EmployeeCount** existuje vlastnost před aktualizací.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a>Řízení přístupu s podpisy sdíleného přístupu
Můžete použít sdíleného přístupového podpisu (SAS) tokeny tooenable klienta aplikace toomodify (a dotaz) entity tabulky přímo bez hello nutné tooauthenticate přímo s služby table hello. Obvykle jsou tři hlavní výhody toousing SAS ve vaší aplikaci:  

* Není nutné toodistribute úložiště účtu klíče tooan nezabezpečené platformy (například mobilní zařízení) v pořadí tooallow tooaccess tohoto zařízení a upravit entity v hello služby Table.  
* Část práce hello může převzít tento web a rolí pracovního procesu provádět při správě zařízení entity tooclient například koncového uživatele počítače a mobilní zařízení.  
* Můžete přiřadit omezené a čas omezenou sadu oprávnění tooa klienta (například umožníte přístup jen pro čtení toospecific prostředky).  

Další informace o používání tokeny SAS s hello služby Table najdete v tématu [pomocí sdíleného přístupového podpisy (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Však musí i nadále generovat tokeny hello SAS, která udělují klienta aplikace toohello entity v služby table hello: bude třeba provést v prostředí, které má zabezpečený přístup tooyour klíče účtu úložiště. Obvykle se používá web nebo worker role toogenerate hello SAS tokeny a poskytovat jim toohello klientských aplikací, které potřebují přístup k tooyour entity. Protože je stále režii zahrnutých v generování a doručování tooclients tokeny SAS, měli byste zvážit jak nejlepší tooreduce Tato dodatečná režie, zejména v vysoký počet scénáře.  

Je možné toogenerate token SAS, že uděluje přístup k podmnožině tooa hello entity v tabulce. Ve výchozím nastavení, můžete vytvořit token SAS pro celou tabulku, ale je také možné toospecify této hello SAS token udělení přístupu tooeither a rozsah **PartitionKey** hodnoty nebo celou řadu **PartitionKey** a ** RowKey** hodnoty. Můžete vybrat toogenerate tokeny SAS pro jednotlivé uživatele systému tak, aby každý uživatel tokenu SAS pouze umožňuje jim přístup služba table tootheir vlastní entity v hello.  

### <a name="asynchronous-and-parallel-operations"></a>Operace asynchronní a paralelní
Zadaný své žádosti jsou šíření mezi více oddílů, můžete zlepšit propustnost a klient odezvy pomocí asynchronní nebo paralelní dotazy.
Například můžete mít dvě nebo více pracovních instance rolí přístup k vaší tabulky paralelně. Může mít jednotlivým pracovním role zodpovědná za konkrétní sady oddílů nebo jednoduše mít více instancí role pracovního procesu, každý schopný tooaccess všechny hello oddílů v tabulce.  

V rámci instanci klienta můžete zlepšit propustnost asynchronně provádění operace úložiště. Hello Klientská knihovna pro úložiště umožňuje snadno toowrite asynchronní dotazy a úpravy. Například můžete začít s hello synchronní metoda, která načte všechny hello entit v oddílu, jak je znázorněno v hello následující kód C#:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

Tento kód můžete snadno upravit, spuštění tohoto dotazu hello asynchronně následujícím způsobem:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

V tomto příkladu asynchronní vidíte hello následující změny z hello synchronní verze:  

* Podpis metody Hello nyní zahrnuje hello **asynchronní** modifikátor a vrátí **úloh** instance.  
* Místo volání hello **ExecuteSegmented** metoda tooretrieve výsledky, hello metoda nyní volání hello **ExecuteSegmentedAsync** metoda a používá hello **await** – modifikátor tooretrieve výsledků asynchronně.  

klientská aplikace Hello tuto metodu můžete volat vícekrát (s různými hodnotami hello **oddělení** parametr), a každý dotaz se spustí na samostatné vlákno.  

Všimněte si, že žádné asynchronní verzi hello **Execute** metoda v hello **TableQuery** třídy protože hello **rozhraní IEnumerable** rozhraní nepodporuje asynchronní výčet.  

Můžete také vložit, aktualizovat a odstranit entity asynchronně. Hello následující příklad jazyka C# ukazuje jednoduchý, synchronní metoda tooinsert nebo nahrazení entity zaměstnanců:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

Tento kód můžete snadno upravit tak, aby aktualizace hello asynchronně následujícím způsobem:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

V tomto příkladu asynchronní vidíte hello následující změny z hello synchronní verze:  

* Podpis metody Hello nyní zahrnuje hello **asynchronní** modifikátor a vrátí **úloh** instance.  
* Místo volání hello **Execute** metoda tooupdate hello entity, hello metoda nyní volání hello **ExecuteAsync** metoda a používá hello **await** tooretrieve – modifikátor asynchronně výsledků.  

klientská aplikace Hello můžete volat více asynchronní metody, jako je tato, a každé volání metody se spustí na samostatné vlákno.  

### <a name="credits"></a>Kredity
Rádi bychom znali toothank hello následující členy hello tým Azure pro jejich příspěvky: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Yields, Vamshidhar Kommineni, Vinay Shaha a Serdar Ozler stejně jako tní Hollander z Microsoft DX. 

Rádi bychom znali také toothank hello následující Microsoft MVP pro jejich cenné zpětné vazby během zkontrolujte cykly: Igor Papirov a Edward Bakker.

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

