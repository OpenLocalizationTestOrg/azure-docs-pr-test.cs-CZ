---
title: "aaaMigrate existující databáze na více systémů tooscale | Microsoft Docs"
description: "Převést nástroje elastické databáze toouse horizontálně dělené databáze tak, že vytvoříte manažera horizontálního oddílu mapy"
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: fa2c9e3699f30667cf547d1faadf4504609199be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-existing-databases-tooscale-out"></a>Migrovat existující databáze tooscale na více systémů
Snadno spravovat existující upraveným horizontálně dělené databáze pomocí nástrojů databáze Azure SQL Database (například hello [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md)). Je nutné nejprve převést existující sady databází toouse hello [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Přehled
toomigrate existující horizontálně dělené databázi: 

1. Příprava hello [horizontálního oddílu mapa správce databáze](sql-database-elastic-scale-shard-map-management.md).
2. Vytvoření mapy hello horizontálního oddílu.
3. Připravte hello jednotlivých horizontálních oddílů.  
4. Přidáte mapu horizontálního oddílu toohello mapování.

Tyto postupy můžou být implementovaná pomocí buď hello [Klientská knihovna pro rozhraní .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), nebo skripty prostředí PowerShell hello nalezený na [Azure SQL DB - skripty nástroje elastické databáze](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Zde Hello příklady použití skriptů prostředí PowerShell hello.

Další informace o hello ShardMapManager najdete v tématu [horizontálního oddílu mapy správu](sql-database-elastic-scale-shard-map-management.md). Přehled nástroje elastické databáze hello najdete v tématu [přehled funkcí elastické databáze](sql-database-elastic-scale-introduction.md).

## <a name="prepare-hello-shard-map-manager-database"></a>Příprava hello horizontálního oddílu mapa správce databáze
Hello horizontálního oddílu mapa správce je speciální databázi, která obsahuje hello data toomanage upraveným databáze. Můžete použít existující databázi nebo vytvořte novou databázi. Všimněte si, že databáze, který funguje jako správce mapy horizontálního oddílu by neměl být hello stejné databázi jako horizontálního oddílu. Všimněte si také, že skript prostředí PowerShell hello nevytvoří hello databáze za vás. 

## <a name="step-1-create-a-shard-map-manager"></a>Krok 1: vytvoření horizontálního oddílu správce mapy
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are hello server name and database name 
    # for hello new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="tooretrieve-hello-shard-map-manager"></a>Správce tooretrieve hello horizontálního oddílu mapy
Po vytvoření můžete načíst hello horizontálního oddílu mapy manager pomocí této rutiny. Tento krok je nutný pokaždé, když potřebujete toouse hello ShardMapManager objektu.

    # Try tooget a reference toohello Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-hello-shard-map"></a>Krok 2: vytvoření mapy horizontálního oddílu hello
Je nutné vybrat typ hello toocreate mapy horizontálního oddílu. Volba Hello závisí na architektuře databáze hello: 

1. Jednoho klienta na databázi (podmínky, najdete v části hello [Glosář](sql-database-elastic-scale-glossary.md).) 
2. Několik klientů na databázi (dva typy):
   1. Seznam mapování
   2. Mapování rozsahu

Pro jednoho klienta modelu, vytvořit **seznamu mapování** horizontálního oddílu mapy. model jednoho klienta Hello přiřadí jednu databázi na každého klienta. Jedná se efektivní model pro vývojáře SaaS, protože ho zjednodušuje správu.

![Seznam mapování][1]

Hello víceklientského modelu přiřadí několik klientů tooa jedné databáze (a skupin klientů, které můžete distribuovat mezi více databází). Pomocí tohoto modelu, pokud očekáváte, že je každý klient toohave malá data. V tomto modelu jsme přiřadit rozsah klientů tooa databázi pomocí **rozsah mapování**. 

![Mapování rozsahu][2]

Nebo můžete implementovat s použitím modelu víceklientské databáze *seznamu mapování* tooassign více klientů tooa jedné databáze. Například DB1 je použité toostore informace o id klienta 1 a 5 a DB2 ukládá data pro klienta 7 a klienta 10. 

![Vnořením klientů na jednom DB][3] 

**Podle své volby, vyberte jednu z těchto možností:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Možnost 1: vytvoření mapy horizontálního oddílu pro seznam mapování
Vytvoření mapy horizontálního oddílu pomocí objektu ShardMapManager hello. 

    # $ShardMapManager is hello shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Možnost 2: vytvoření mapy horizontálního oddílu pro mapování rozsahu
Všimněte si, že tooutilize tento vzor mapování, hodnoty id klienta musí toobe průběžné rozsahy a je přijatelné toohave mezera v oblastech hello jednoduše přeskočením hello rozsahu při vytváření databází hello.

    # $ShardMapManager is hello shard map manager object 
    # 'RangeShardMap' is hello unique identifier for hello range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Možnost 3: Seznam mapování na jedné databáze
Nastavení tohoto vzoru také vyžaduje vytvoření seznamu mapování, jak je znázorněno v kroku 2, možnost 1.

## <a name="step-3-prepare-individual-shards"></a>Krok 3: Příprava jednotlivých horizontálních oddílů
Přidejte každou horizontálního oddílu (databáze) toohello horizontálního oddílu mapy správci. To připraví hello jednotlivé databáze pro ukládání informací o mapování. Tato metoda spusťte na každém horizontálního oddílu.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # hello $ShardMap is hello shard map created in step 2.


## <a name="step-4-add-mappings"></a>Krok 4: Přidejte mapování
Přidání Hello mapování závisí na druhu hello mapy horizontálního oddílu, který jste vytvořili. Pokud jste vytvořili seznam mapy, můžete přidat mapování seznamu. Pokud jste vytvořili mapu rozsahu, můžete přidat mapování rozsahu.

### <a name="option-1-map-hello-data-for-a-list-mapping"></a>Možnost 1: mapování hello dat pro mapování seznamu
Mapování dat hello přidáním seznamu mapování pro každého klienta.  

    # Create hello mappings and associate it with hello new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-hello-data-for-a-range-mapping"></a>Možnost 2: mapování hello dat pro mapování rozsahu
Přidejte hello rozsah mapování pro všechny hello klienta rozsah id - přidružení databáze:

    # Create hello mappings and associate it with hello new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-hello-data-for-multiple-tenants-on-a-single-database"></a>Krok 4 možnost 3: mapování hello dat pro více klientů v jedné databáze
Pro každého klienta, spusťte hello přidat ListMapping (možnost 1, výše). 

## <a name="checking-hello-mappings"></a>Kontrola, zda text hello mapování
Informace o hello existující horizontálních oddílů a mapování hello s nimi spojených lze dotazovat pomocí následujících příkazů:  

    # List hello shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Souhrn
Po dokončení instalace hello, můžete začít toouse hello elastické databáze klientské knihovny. Můžete také použít [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md) a [dotazu víc horizontálních](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Další kroky
Získat skriptů prostředí PowerShell hello z [Azure SQL DB Elastická databáze nástroje sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Hello nástroje jsou také na Githubu: [/Elastická db nástroje Azure](https://github.com/Azure/elastic-db-tools).

Použijte hello nástroji pro sloučení rozdělení toomove data tooor z modelu víceklientského modelu tooa jednoho klienta. V tématu [nástroji pro sloučení rozdělení](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Další zdroje
Informace o běžných vzorech architektury dat databázových aplikací softwaru s více tenanty jako služby (SaaS) naleznete v části [Vzory návrhu pro aplikace SaaS s více tenanty s databází Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Otázky a žádosti o funkce
Máte dotazy, kontaktujte toous na hello [SQL Database fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) a pro žádosti o funkce, přidejte je toohello [fóru pro zpětnou vazbu SQL Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

