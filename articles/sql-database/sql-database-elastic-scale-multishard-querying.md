---
title: "databáze Azure SQL horizontálně dělené aaaQuery | Microsoft Docs"
description: "Spuštění dotazů napříč horizontálních oddílů pomocí klientské knihovny pro elastické databáze hello."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: a1f0763935a6807b74aa9dec477714e8d117417d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="multi-shard-querying"></a>Víc horizontálních dotazování
## <a name="overview"></a>Přehled
S hello [nástroje elastické databáze](sql-database-elastic-scale-introduction.md), můžete vytvořit řešení horizontálně dělené databáze. **Dotazování víc horizontálních** se používá pro úlohy, jako je kolekce nebo generování sestav dat vyžadující spuštění dotazu roztahovány napříč několika horizontálních oddílů. (To příliš kontrastu[závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md), který provede všechny práci na jednom horizontálního oddílu.) 

1. Získání [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) nebo [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) pomocí hello [ **TryGetRangeShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), nebo hello [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metoda. V tématu [ **vytváření ShardMapManager** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) a [ **získat RangeShardMap nebo ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Vytvoření  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  objektu.
3. Vytvoření  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Sada hello  **[Vlastnost CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  tooa příkazů T-SQL.
5. Spusťte příkaz hello tak, že volání hello  **[ExecuteReader metoda](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Zobrazení výsledků hello pomocí hello  **[MultiShardDataReader třída](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Příklad
Hello následující kód ukazuje použití hello víc horizontálních dotazování pomocí danou **ShardMap** s názvem *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                 } 
           } 
    } 


Klíčovým rozdílem je, hello konstrukce připojení více horizontálního oddílu. Kde **SqlConnection** funguje na jedné databáze, hello **MultiShardConnection** trvá ***kolekce horizontálních oddílů*** jako vstup. Naplnění hello kolekce horizontálních oddílů z mapy horizontálního oddílu. potom se spustí Hello dotaz na kolekce hello horizontálních oddílů pomocí **UNION ALL** sémantiku tooassemble jeden celkový výsledek. Volitelně hello název hello horizontálního oddílu, kde hello řádek pochází z lze přidat toohello výstup pomocí hello **ExecutionOptions** na příkaz Vlastnosti. 

Všimněte si volání hello příliš**myShardMap.GetShards()**. Tato metoda načte všechny horizontálních oddílů z mapování horizontálních hello a poskytuje snadný způsob toorun dotazu ve všech příslušných databází. Hello kolekce horizontálních oddílů pro více horizontálního oddílu dotazu může být přesnějších další provedením LINQ dotazu v kolekci hello vrácená z volání hello příliš**myShardMap.GetShards()**. V kombinaci se zásadou částečné výsledky hello byl hello aktuální funkci víc horizontálních dotazování navrženou toowork dobře u desítkami až toohundreds horizontálních oddílů.

Omezení s více horizontálních dotazování právě hello nedostatečná ověření horizontálních oddílů a shardlets, který je dotazován. Při závislé na data směrování ověřuje, že daný horizontálního oddílu je součástí mapy hello horizontálního oddílu v době hello dotazování, neprovádějte víc horizontálních dotazy této kontroly. To může způsobit toomulti horizontálních dotazy spuštěné v databázích, které byly odebrány z mapování horizontálních hello.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Víc horizontálních dotazy a operace sloučení rozdělení
Dotazy víc horizontálních Neověřovat, zda jsou shardlets na hello dotazované databáze účastní probíhající operace sloučení rozdělení. (Viz [škálování, pomocí nástroje hello elastické databáze rozdělení sloučení](sql-database-elastic-scale-overview-split-and-merge.md).) To může vést tooinconsistencies kde řádky z hello stejné shardlet zobrazit pro více databází v hello stejný dotaz s více horizontálního oddílu. Mějte na paměti tyto omezení a vezměte v úvahu při provádění dotazů víc horizontálních vyprazdňování probíhající rozdělení sloučení operace a změny toohello horizontálního oddílu mapy.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Viz také
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  třídy a metody.

Spravovat pomocí hello horizontálních oddílů [klientské knihovny pro elastické databáze](sql-database-elastic-database-client-library.md). Zahrnuje obor názvů s názvem [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) poskytuje možnost tooquery hello víc horizontálních oddílů pomocí jeden dotaz a výsledek. Dotazování abstrakce zajišťuje v kolekci horizontálních oddílů. Obsahuje také zásady alternativní spouštění, zejména částečné výsledky, toodeal s chybami při dotazování přes mnoho horizontálních oddílů.  

