---
title: "aaaReport napříč instancemi cloudu databází (vodorovné rozdělení do oddílů) | Microsoft Docs"
description: "jak toouse mezi databáze databázové dotazy"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: e34f398f8d408cffd91a70fc2cfbda73daec3550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Sestavy napříč instancemi cloudu databází (preview)
Můžete vytvořit sestavy z několika databází Azure SQL z bodu pomocí jednoho připojení [elastické dotazu](sql-database-elastic-query-overview.md). Hello databáze musí mít oddíly vodorovně (také označované jako "horizontálně dělené").

Pokud máte existující databázi, přečtěte si téma [migraci stávající databáze, databáze na více systémů tooscaled](sql-database-elastic-convert-to-use-elastic-tools.md).

objekty SQL hello toounderstand potřeby tooquery najdete v tématu [dotazu mezi databázemi vodorovně oddílů](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Požadavky
Stažení a spuštění hello [Začínáme s ukázkou nástroje elastické databáze](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a>Vytvoření mapy horizontálního oddílu manager pomocí hello ukázkové aplikace
Zde vytvoříte mapu horizontálního oddílu manager spolu s několika horizontálních oddílů, za nímž následuje vložení dat do hello horizontálních oddílů. Pokud jste tooalready nastavili horizontálních oddílů s horizontálně dělená data v nich, můžete přeskočit následující kroky hello a přesunout toohello další části.

1. Sestavení a spuštění hello **Začínáme s nástroje elastické databáze** ukázkové aplikace. Postupujte podle kroků hello až do kroku 7 v části hello [stažení a spuštění ukázkové aplikace hello](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Na konci hello tohoto kroku 7 zobrazí se hello následující příkazový řádek:

    ![příkazový řádek][1]
2. V příkazovém okně hello, zadejte "1" a stiskněte klávesu **Enter**. To vytvoří hello horizontálního oddílu mapa správce a přidá serverové toohello horizontálních oddílů. Potom zadejte "3" a stiskněte klávesu **Enter**; zopakuje akci hello čtyřikrát. Vloží řádky ukázková data ve vašem horizontálních oddílů.
3. Hello [portál Azure](https://portal.azure.com) by měl zobrazit tři nové databáze na serveru:

   ![Visual Studio potvrzení][2]

   Mezidatabázové dotazy v tomto okamžiku jsou podporovány prostřednictvím knihovny klienta hello elastické databáze. Například v příkazovém okně hello použijte možnost 4. Hello výsledků dotazu víc horizontálních jsou vždy **UNION ALL** výsledků hello ze všech horizontálních oddílů.

   V další části hello jsme vytvořit koncový bod ukázkové databáze podporující bohatší dotazování hello dat napříč horizontálních oddílů.

## <a name="create-an-elastic-query-database"></a>Vytvoření dotazu elastické databáze
1. Otevřete hello [portál Azure](https://portal.azure.com) a přihlaste se.
2. Vytvořit novou databázi Azure SQL v hello stejný server jako vašeho nastavení horizontálního oddílu. Název databáze hello "ElasticDBQuery."

    ![Portál Azure a cenovou úroveň][3]

    > [!NOTE]
    > můžete použít existující databázi. Pokud můžete tak učinit, nesmí být jedna z hello horizontálních oddílů, které chcete tooexecute své dotazy na. Tato databáze se použije pro vytvoření hello objekty metadata pro dotaz elastické databáze.
    >

## <a name="create-database-objects"></a>Vytvoření databázových objektů
### <a name="database-scoped-master-key-and-credentials"></a>Hlavní klíč databáze obor a přihlašovací údaje
Toto jsou použité tooconnect toohello horizontálního oddílu mapa správce a hello horizontálních oddílů:

1. Spusťte aplikaci SQL Server Management Studio nebo SQL Server Data Tools v sadě Visual Studio.
2. Připojit databáze tooElasticDBQuery a spusťte následující příkazy T-SQL hello:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "username" a "password" by měl být hello stejné jako informace o přihlášení se používají v kroku 6 v [stažení a spuštění ukázkové aplikace hello](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) v [Začínáme s nástroje elastické databáze](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Externích zdrojů dat.
toocreate externího zdroje dat, spusťte následující příkaz v databázi ElasticDBQuery hello hello:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 "CustomerIDShardMap" je název hello hello horizontálního oddílu mapy, pokud jste vytvořili hello horizontálního oddílu mapy a mapovat horizontálního oddílu manager pomocí Ukázka nástroje elastické databáze hello. Ale pokud jste použili vlastní instalace pro tuto ukázku, pak jej by měl být hello horizontálního oddílu mapy název, které jste zvolili v aplikaci.

### <a name="external-tables"></a>Externí tabulky
Vytvořte externí tabulku, která odpovídá tabulku zákazníků hello na horizontálních oddílů hello tak, že spustíte následující příkaz v databázi ElasticDBQuery hello:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Spuštění ukázkového dotazu T-SQL elastické databáze
Jakmile definujete zdroj externích dat a externí tabulky teď můžete použít úplnou T-SQL na externí tabulky.

Spusťte tento dotaz na databázi ElasticDBQuery hello:

    select count(CustomerId) from [dbo].[Customers]

Si všimnete, že dotaz hello agreguje výsledky ze všech hello horizontálních oddílů a poskytuje následující výstup hello:

![Podrobnosti o výstupu][4]

## <a name="import-elastic-database-query-results-tooexcel"></a>Import tooExcel výsledky dotazu elastické databáze
 Můžete importovat hello výsledky ze souboru aplikace Excel tooan dotazu.

1. Spusťte aplikaci Excel 2013.
2. Přejděte toohello **Data** pásu karet.
3. Klikněte na tlačítko **z jiných zdrojů** a klikněte na tlačítko **z SQL serveru**.

   ![Importu pro aplikaci Excel z jiných zdrojů][5]
4. V hello **Průvodce datovým připojením** zadejte název a přihlašovací údaje serveru hello. Pak klikněte na tlačítko **Další**.
5. V dialogovém okně hello **hello vyberte databázi, která obsahuje hello data, která chcete**, vyberte hello **ElasticDBQuery** databáze.
6. Vyberte hello **zákazníci** tabulky v zobrazení seznamu hello a klikněte na tlačítko **Další**. Pak klikněte na tlačítko **Dokončit**.
7. V hello **importovat Data** formuláři v části **vyberte požadovaný způsob tooview tato data v sešitu**, vyberte **tabulky** a klikněte na tlačítko **OK**.

Všechny řádky z hello **zákazníci** tabulky, uložené v různých horizontálních oddílů naplnit hello Excelovém listu.

Teď můžete použít funkce vizualizace výkonné dat v aplikaci Excel. Můžete použít hello připojovací řetězec s názvem serveru, název databáze a pověření tooconnect vaše data a BI integrace nástrojů toohello elastické dotaz do databáze. Ujistěte se, že systém SQL Server je podporovaný jako zdroj dat pro vaše nástroje. Může odkazovat toohello elastické dotaz do databáze a externí tabulky stejně jako všechny ostatní databáze systému SQL Server a zda byste připojili toowith vaše nástroje tabulek systému SQL Server.

### <a name="cost"></a>Náklady
Není k dispozici pro použití funkce hello elastické databáze dotazu bez dalších poplatků.

Informace o cenách najdete v části [podrobnosti o cenách na SQL databázi](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Další kroky

* Přehled elastické dotazů najdete v tématu [elastické dotazu přehled](sql-database-elastic-query-overview.md).
* Vertikální dělení kurzu, najdete v části [Začínáme s mezidatabázové dotazu (vertikální dělení)](sql-database-elastic-query-getting-started-vertical.md).
* Syntaxe a ukázkové dotazy pro svisle oddílů data, najdete v části [dotazování svisle na oddíly dat)](sql-database-elastic-query-vertical-partitioning.md)
* Syntaxe a ukázkové dotazy pro horizontálně dělenou data, najdete v části [dotazování vodorovně rozdělena na oddíly dat)](sql-database-elastic-query-horizontal-partitioning.md)
* V tématu [sp\_provést \_vzdáleného](https://msdn.microsoft.com/library/mt703714) pro uloženou proceduru, který spouští příkaz jazyka Transact-SQL na jedné vzdálené databáze SQL Azure nebo sadu databází, které slouží jako horizontálních oddílů v vodorovné schéma rozdělení oddílů.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
