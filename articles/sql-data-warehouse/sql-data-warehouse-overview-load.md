---
title: aaaLoad data do Azure SQL Data Warehouse | Microsoft Docs
description: "Další běžné scénáře hello dat načítání do SQL Data Warehouse. Patří sem pomocí PolyBase, úložiště objektů blob v Azure, plochých souborů a přesouvání disku. Můžete také použít nástroje třetích stran."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: d1a5063f484e9bd95f854e27a4baed512148aad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Načtení dat do Azure SQL Data Warehouse
Přehled možností hello scénáře a doporučení pro načítání dat do SQL Data Warehouse.

načítání dat Hello nejtěžší část obvykle připravuje hello data pro zatížení hello. Azure zjednodušuje načítání pomocí úložiště objektů blob v Azure jako běžné úložiště dat pro mnoho hello služeb, a pomocí Azure Data Factory hello tooorchestrate komunikačních a datových přesun mezi službami Azure. Tyto procesy jsou integrované s technologií PolyBase, který používá masivně paralelní zpracování dat (MPP) tooload paralelně z Azure blob storage do SQL Data Warehouse. 

Podrobné pokyny, které načítají ukázkové databáze, najdete v části [načíst ukázkové databáze][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Načtení z Azure blob storage
Hello nejrychlejší způsob, jak tooimport dat do SQL Data Warehouse je toouse PolyBase tooload dat z Azure blob storage. PolyBase používá SQL Data Warehouse na masivně paralelní zpracování (MPP) návrhu tooload dat paralelně z Azure blob storage. toouse PolyBase, můžete použít příkazy T-SQL nebo kanál služby Azure Data Factory.

### <a name="1-use-polybase-and-t-sql"></a>1. Použijte PolyBase a T-SQL
Souhrn procesu načtení:

1. Přesunout úložiště objektů blob tooAzure dat nebo Azure Data Lake Store a uloží jej v textových souborů.
2. Nakonfigurujte externí objekty v SQL Data Warehouse toodefine hello umístění a formát dat hello
3. Paralelní spuštění T-SQL příkazu tooload hello data do nové tabulky databáze.

<!-- 5. Schedule and run a loading job. --> 

Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Použití Azure Data Factory
Pro jednodušší způsob toouse PolyBase můžete vytvořit kanál Azure Data Factory, který používá PolyBase tooload dat z Azure blob storage do SQL Data Warehouse. To je rychlé tooconfigure, protože nepotřebujete toodefine hello T-SQL objekty. Pokud potřebujete tooquery hello externích dat bez provedení importu, použijte T-SQL. 

Souhrn procesu načtení:

1. Přesunout úložiště objektů blob tooAzure dat a uloží jej v textových souborů. Azure Data Factory aktuálně nepodporuje ADLS připojení pomocí funkce PolyBase).
2. Vytvoření Azure Data Factory kanálu tooingest hello data. Použijte možnost PolyBase hello.
4. Naplánování a spuštění kanálu hello.

Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Načtení z SQL serveru
tooload data z tooSQL systému SQL Server datového skladu můžete Integration Services (SSIS), přeneste plochých souborů nebo dodávat tooMicrosoft disky. Číst na toosee souhrn hello různých načítání tootutorials procesy a odkazy.

tooplan úplná data migrace ze systému SQL Server tooSQL datového skladu, najdete v části hello [Přehled migrace][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Použití integrační služby (SSIS)
Pokud už používáte tooload balíčky Integration Services (SSIS) do systému SQL Server, můžete je aktualizovat balíčky toouse systému SQL Server jako zdroj hello a SQL Data Warehouse jako cíl hello. To je rychlý a snadný toodo, a je vhodný, pokud se nepokoušíte toomigrate vaše načítání zpracování dat toouse již v cloudu hello. kompromis Hello je hello zatížení bude nižší než pomocí PolyBase, protože tento SSIS neprovádí hello zatížení paralelně.

Souhrn procesu načtení:

1. Upraveno integrační služby balíček toopoint toohello instanci serveru SQL pro zdroj hello a databázi SQL Data Warehouse hello pro cíl hello.
2. Vaše tooSQL schématu datového skladu, migrujte, pokud ještě není existuje.
3. Mapování hello změn ve vašich balíčcích používají pouze datové typy hello, které jsou podporovány nástrojem SQL Data Warehouse.
4. Naplánování a spuštění balíčku hello.

Podívejte se kurz [načtení dat z tooAzure systému SQL Server datového skladu SSIS (SQL)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Pomocí nástroje AZCopy (doporučeno pro < 10 TB dat)
Pokud má velikost dat < 10 TB, můžete exportovat hello data z tooflat soubory systému SQL Server, zkopírovat úložiště objektů blob tooAzure hello soubory a potom pomocí funkce PolyBase tooload hello dat do SQL Data Warehouse

Souhrn procesu načtení:

1. Použijte data tooexport nástroj příkazového řádku bcp hello z tooflat soubory systému SQL Server.
2. Použijte hello AZCopy nástroj příkazového řádku toocopy dat z plochých souborů tooAzure blob storage.
3. Použijte PolyBase tooload do SQL Data Warehouse.

Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Pomocí bcp
Pokud máte malé množství dat můžete bcp tooload přímo do Azure SQL Data Warehouse.

Souhrn procesu načtení:

1. Použijte data tooexport nástroj příkazového řádku bcp hello z tooflat soubory systému SQL Server.
2. Použijte bcp tooload data od ploché soubory přímo tooSQL datového skladu.

Podívejte se kurz [načtení dat z SQL serveru tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Použití importu a exportu (doporučeno pro > 10 TB dat)
Pokud je vaše velikost dat > 10 TB a chcete toomove ho tooAzure, doporučujeme používat naše disku přesouvání služby [importu a exportu][Import/Export]. 

Souhrn procesu načtení

1. Použijte data tooexport nástroj příkazového řádku bcp hello z tooflat soubory systému SQL Server na přenositelné disky.
2. Dodávat tooMicrosoft disky hello.
3. Microsoft načte hello data do SQL Data Warehouse

## <a name="load-from-hdinsight"></a>Načtení z prostředí HDInsight
SQL Data Warehouse podporuje načítání dat z HDInsight prostřednictvím PolyBase. proces Hello je hello stejné jako načítání dat z Azure Blob Storage – pomocí PolyBase tooconnect tooHDInsight tooload data. 

### <a name="1-use-polybase-and-t-sql"></a>1. Použijte PolyBase a T-SQL
Souhrn procesu načtení:

1. Přesunout tooHDInsight vaše data a ukládá je v textových souborů ORC nebo Parquet formátu.
2. Nakonfigurujte externí objekty v SQL Data Warehouse toodefine hello umístění a formát dat hello.
3. Paralelní spuštění T-SQL příkazu tooload hello data do nové tabulky databáze.

Podívejte se kurz [načtení dat z Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Doporučení
Mnoho našich partnerů má načítání řešení. toofind informace, podívejte se do seznamu naše [partnery poskytujícími řešení][solution partners]. 

Pokud vaše data pochází z nerelační zdroje a chcete tooload do SQL datového skladu můžete potřebovat tootransform do řádků a sloupců jej před načtením. Hello transformovat data nepotřebuje toobe uložená v databázi, mohou být uloženy v textových souborů.

Vytvoření statistiky pro nově načtená data. Azure SQL Data Warehouse zatím nepodporuje automatické vytváření ani automatickou aktualizaci statistik.  V pořadí tooget hello nejlepší výkon ze své dotazy je důležité nejdřív načíst toocreate statistiky pro všechny sloupce všech tabulek po hello nebo dojít k významné změny v datech hello.  Podrobnosti najdete v tématu [statistiky][Statistics].

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části hello [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server tooAzure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server tooAzure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
