---
title: "Přehled elastické dotazu SQL Database aaaAzure | Microsoft Docs"
description: "Přehled funkce elastické dotazu hello"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: a8bf0e2c-bc74-44d0-9b1e-bcc9a6aa2e33
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: mlandzic
ms.openlocfilehash: db17f551882cfcae0da67fdda12708baeb6db81c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>Elastické dotazu Přehled služby Azure SQL Database (preview)
Funkce elastické dotazu Hello (ve verzi preview) vám umožní toorun dotazu jazyka Transact-SQL, která přesahuje více databází ve službě Azure SQL Database. Umožní vám tooperform mezidatabázové dotazy tooaccess vzdálených tabulek a tooquery nástroje (aplikace Excel, PowerBI, Tableau atd.) společnosti Microsoft a třetích stran tooconnect napříč datové vrstvy s více databází. Pomocí této funkce můžete škálovat dotazy toolarge datové vrstvy v databázi SQL a vizualizace výsledků hello v sestavách business intelligence (BI).


## <a name="why-use-elastic-queries"></a>Proč používat elastické dotazy?

**Azure SQL Database**

Dotazování mezi databází Azure SQL zcela v T-SQL. To umožňuje jen pro čtení dotazování vzdálené databáze. To poskytuje možnost pro stávající místní aplikace toomigrate zákazníkům SQL serveru pomocí tří a čtyř částečný názvy nebo odkazovaný server tooSQL DB.

**K dispozici na úrovni standard**

Je podporován elastické dotaz na úroveň Standard výkonu hello v přidání toohello Premium výkonu vrstvy. O omezení výkonu pro nižší úrovně výkonu najdete v části hello na omezení verze Preview níže.

**Push tooremote databáze**

Elastické dotazy můžete nyní push vzdálené databáze toohello SQL parametry pro spuštění.

**Spuštění uložené procedury**

Spustit volání vzdálené uložené procedury nebo funkce remote pomocí [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714).

**Flexibilita**

Externí tabulky s elastické dotazu teď najdete tooremote tabulky s jinou schéma nebo název tabulky.

## <a name="elastic-query-scenarios"></a>Scénáře elastické dotazu

cílem Hello je toofacilitate dotazování scénáře, kde více databází přispívat řádků do jednoho celkového výsledku. dotaz Hello můžete buď skládat hello uživatele nebo aplikace přímo nebo nepřímo prostřednictvím nástrojů, které jsou připojené toohello databáze. To je užitečné zejména při vytváření sestav, pomocí komerční integrace nástroje BI nebo data, nebo jakékoli aplikace, kterou nelze změnit. S elastické dotaz se můžete dotazovat napříč několika databází v nástroje, například aplikace Excel, PowerBI, Tableau nebo Cognos hello známou vlastnost připojení k systému SQL Server.
Dotaz elastické umožňuje snadný přístup tooan celou kolekci databáze pomocí dotazů vydaný SQL Server Management Studio nebo Visual Studio a usnadňuje mezidatabázové dotazování z Entity Framework nebo jiných ORM prostředích. Obrázek 1 ukazuje scénář, kde existující cloudové aplikace (hello, který používá [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)) staví na data Škálováním vrstvy a elastické dotazu se používá pro vytváření sestav napříč databáze.

**Obrázek 1** elastické dotaz byl použit na upraveným datové vrstvy

![Elastické dotaz byl použit na upraveným datové vrstvy][1]

Zákazník scénáře pro elastické dotazu jsou charakteristické hello následující topologie:

* **Vertikální dělení - mezidatabázové dotazy** (topologie 1): hello data svisle rozdělena mezi počet databází v datové vrstvě. Různé sady tabulek jsou obvykle umístěné na různých databází. To znamená, že schéma této hello se liší v různých databází. Například všechny tabulky pro inventář je na jednu databázi, zatímco všechny tabulky související s monitorování účtů na druhý databáze. Běžné případy použití s touto topologií vyžadují jednu tooquery napříč nebo toocompile sestavy mezi tabulkami v několika databází.
* **Vodorovné dělení - horizontálního dělení** (topologie 2): Data vodorovně rozdělena toodistribute řádky napříč škálovaný dat vrstvy. S tímto přístupem hello schéma je stejná na všechny zúčastněné databáze. Tento přístup je také označován "horizontálního dělení". Horizontálního dělení může provést a spravovat pomocí knihovny nástroje elastické databáze (1) hello nebo (2) horizontálního samoobslužné dělení. Elastické dotazu je používané sestavy tooquery nebo kompilace napříč mnoha horizontálních oddílů.

> [!NOTE]
> Elastické dotazu je nejvhodnější pro příležitostně scénáře vytváření sestav, kde lze provádět většinu hello zpracování na datové vrstvě hello. Pro scénáře s více složitých dotazů datových skladů nebo náročných úloh vytváření sestav, také zvažte použití [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Vertikální dělení - mezidatabázové dotazy

toobegin kódování, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).

Elastické dotazu může být použité toomake datům umístěným v SQL databáze k dispozici tooother databáze SQL. To umožňuje dotazy z jednoho tootables toorefer databázi v jiných vzdálené databáze SQL. prvním krokem Hello je toodefine externího zdroje dat pro každou vzdálenou databázi. Hello externí zdroj dat je definován v místní databázi hello, ze kterého mají být tootables přístup toogain nachází na vzdálené databáze hello. Žádné změny jsou nezbytné pro vzdálenou databázi hello. Pro typické vertikální dělení scénáře kde různých databází mají různé schémat elastické dotazy může být použité tooimplement běžné případy použití, jako je přístup k datům tooreference a mezidatabázové dotazování.

> [!IMPORTANT]
> Musíte mít oprávnění ALTER ANY EXTERNAL DATA SOURCE. Toto oprávnění je součástí hello oprávnění ALTER DATABASE. Oprávnění ALTER ANY externí zdroj dat se zdroji dat toohello potřebné toorefer.
>

**Referenční data**: hello topologie se používá pro správu dat na odkaz. Hello obrázek jsou dvě tabulky (T1 a T2) u referenčních dat ukládají na vyhrazené databázi. Pomocí elastické dotazu, můžete nyní přistupovat tabulky T1 a T2 vzdáleně z jiných databází, jak je znázorněno na obrázku hello. Použít topologii 1, pokud referenční tabulky jsou malé nebo vzdálené dotazy na referenční tabulku mít selektivní predikáty.

**Obrázek 2** vertikální dělení – pomocí elastické dotazu tooquery referenční data

![Vertikální dělení – pomocí elastické dotazu tooquery referenční data][3]

**Dotazování mezidatabázové**: Elastická dotazuje povolit případy použití, které vyžadují dotazování napříč několika databází SQL. Obrázek 3 ukazuje čtyř různých databází: CRM, inventáře, HR a produkty. Dotazy v jedné z databází hello provést také potřebovat přístup k tooone nebo všechny hello jiné databáze. Pomocí elastické dotazu, můžete nakonfigurovat databázi pro tento případ spuštěním několik jednoduchých příkazy DDL na každý ze čtyř databází hello. Po této jednorázovou konfiguraci je stejně jednoduché jako odkazující tooa místní tabulky z vašich dotazů T-SQL nebo z vaší nástrojů BI přístup tooa vzdálenou tabulku. Tento postup se doporučuje, když vzdálených dotazů hello nevrátí velké výsledky.

**Obrázek 3** vertikální dělení – pomocí elastické dotazu tooquery napříč různými databázemi

![Vertikální dělení – pomocí elastické dotazu tooquery napříč různými databázemi][4]

Hello následující kroky konfigurace dotazy elastické databáze pro vertikální dělení scénáře, které vyžadují přístup tooa tabulky nachází na vzdálené databáze SQL s hello stejné schéma:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [Příkaz CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource typu **RDBMS**
* [Příkaz CREATE/DROP externí tabulky](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Po spuštění příkazy DDL hello, dostanete Vzdálená tabulka "mytable" hello, jako by šlo místní tabulky. Azure SQL Database automaticky otevře připojení vzdálené databáze toohello, zpracuje požadavek na vzdálené databáze hello a vrátí výsledky hello.

## <a name="horizontal-partitioning---sharding"></a>Vodorovné rozdělení do oddílů - horizontálního dělení
Použití tooperform elastické dotazu úlohy sestav přes horizontálně dělené, tj., vodorovně rozdělena na oddíly, vyžaduje datovou vrstvu [mapy horizontálního oddílu elastické databáze](sql-database-elastic-scale-shard-map-management.md) toorepresent hello databáze hello datové vrstvy. Obvykle se v tomto scénáři se používá jenom jeden horizontálních mapu a vyhrazené databáze s možností elastické dotazu (hlavního uzlu) slouží jako hello vstupní bod pro vytváření sestav dotazy. Pouze tuto databázi vyhrazené musí mapa horizontálního oddílu toohello přístupu. Na obrázku 4 jsou tato topologie a jeho konfigurace s hello dotazu elastické databáze a horizontálního oddílu mapy. Hello databáze v datové vrstvě hello může mít pro všechny databáze SQL Azure verzi nebo edici. Další informace o hello klientské knihovny pro elastické databáze a vytváření mapy horizontálního oddílu najdete v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md).

**Obrázek 4** vodorovné rozdělení do oddílů – pomocí elastické dotazu pro vytváření sestav v horizontálně dělené datové vrstvy

![Vodorovné rozdělení do oddílů – pomocí elastické dotazu pro vytváření sestav v horizontálně dělené datové vrstvy][5]

> [!NOTE]
> Elastické databáze (hlavního uzlu) dotazu může být samostatné databáze, nebo může být hello stejné databáze tohoto hostitele hello horizontálního oddílu mapy. Konfigurace, ať si zvolíte, ujistěte se, že úroveň služby a výkonu této databáze je dostatečně vysoký toohandle hello očekává množství požadavků na přihlášení nebo dotazy.

Hello následující kroky konfigurace dotazy elastické databáze pro vodorovné rozdělení scénáře vyžadující přístup tooa sadu tabulky, které se nacházejí na (obvykle) několik vzdálenou databází SQL:

* [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* Vytvoření [horizontálního oddílu mapy](sql-database-elastic-scale-shard-map-management.md) představující vaší datovou vrstvu pomocí klientské knihovny pro elastické databáze hello.   
* [Příkaz CREATE/DROP EXTERNAL DATA SOURCE](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource typu **SHARD_MAP_MANAGER**
* [Příkaz CREATE/DROP externí tabulky](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Po provedení těchto kroků můžete přistupovat hello horizontálně dělenou tabulku "mytable", jako by šlo místní tabulky. Azure SQL Database automaticky otevře více paralelních připojení toohello vzdálenou databází fyzicky úložiště tabulek hello, zpracovává hello požadavky na vzdálenou databází hello a vrátí výsledky hello.
Další informace o hello kroky potřebné pro vodorovné rozdělení scénář hello lze nalézt v [elastické dotazu pro vodorovné rozdělení do oddílů](sql-database-elastic-query-horizontal-partitioning.md).

toobegin kódování, najdete v části [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>Dotazování T-SQL
Po definování zdrojů externích dat a externí tabulky můžete použít normální systému SQL Server připojovací řetězce tooconnect toohello databáze, kde jste definovali externí tabulky. Potom spuštěním příkazů T-SQL na externí tabulky na toto připojení s omezeními hello uvedených níže. Další informace a příklady dotazů T-SQL můžete najít v tématech hello dokumentace pro [vodorovné rozdělení do oddílů](sql-database-elastic-query-horizontal-partitioning.md) a [vertikální dělení](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Připojení nástroje
Vaše aplikace a BI nebo data toodatabases integrace nástrojů, které mají externí tabulky můžete regulární tooconnect řetězce připojení SQL Server. Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje. Po připojení naleznete stejně, jako by provést v jiné databázi systému SQL Server připojit toowith vaše nástroje toohello dotazu elastické databáze a hello externí tabulky v databázi.

> [!IMPORTANT]
> Ověřování pomocí služby Azure Active Directory s elastické dotazy není aktuálně podporován.
> 
> 

## <a name="cost"></a>Náklady
Elastické dotazu je zahrnut do hello náklady databáze Azure SQL Database. Všimněte si, že jsou podporované topologie, kde jsou vaše vzdálené databáze v různých datových center než hello elastické dotazu koncový bod, ale budou se regular účtovat odchozí data ze vzdálených databází [Azure sazby](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Omezení verze Preview
* Spuštění vaší první elastické dotazu může trvat až tooa několik minut na úroveň Standard výkonu hello. Tato doba je nezbytné tooload hello elastické dotazovacích funkcí; načítání výkonu zlepšuje s vyšší úrovně výkonu.
* Externí zdroje dat nebo externí tabulky z aplikace SSMS nebo rozšíření SSDT skriptu se ještě nepodporuje.
* Import a Export pro SQL DB zatím nepodporuje externí zdroje dat a externí tabulky. Pokud potřebujete toouse importu a exportu, vyřaďte tyto objekty před exportem a potom je znovu vytvořit po importu.
* Elastické dotazu aktuálně podporuje jenom tooexternal tabulky jen pro čtení. Můžete však použít plnou funkčnost T-SQL v databázi hello kterých byla definována hello externí tabulky. To může být užitečné k, například uchování dočasné výsledky pomocí například vyberte < column_list > do < local_table > nebo toodefine uložené procedury na hello dotazu elastické databáze, která odkazují tooexternal tabulky.
* S výjimkou nvarchar(max) typů LOB nepodporují v definicích externí tabulky. Jako alternativní řešení můžete vytvořit zobrazení na hello vzdálené databáze, který vrhá typu LOB hello do nvarchar(max), definovat externí tabulky přes hello zobrazení namísto hello základní tabulky a převést jej zpět do původní typu LOB hello v dotazech.
* Statistiky sloupce na externí tabulky nejsou aktuálně podporovány. Tabulky statistiky jsou podporovány, ale potřebuje toobe ručně vytvořit.

## <a name="feedback"></a>Váš názor
Předejte zpětná vazba týkající se vašich zkušeností s elastické dotazy s námi na Disqus níže, hello fórech MSDN, nebo na Stackoverflow. Snažíme se ve všech druhy zpětnou vazbu týkající se služby hello (závad, případné nedostatky, funkce mezery).

## <a name="next-steps"></a>Další kroky

* Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).
* Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)
* Podívejte se kurz vodorovné rozdělení do oddílů (horizontálního dělení) [Začínáme s elastické dotazu pro vodorovné rozdělení do oddílů (horizontálního dělení)](sql-database-elastic-query-getting-started.md).
* Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)
* V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
