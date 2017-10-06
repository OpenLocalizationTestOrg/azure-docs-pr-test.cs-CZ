---
title: "aaaAdding a horizontálního oddílu pomocí nástroje elastické databáze | Microsoft Docs"
description: "Jak nastavit toouse elastické škálování rozhraní API tooadd nové tooa horizontálních horizontálních oddílů."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f44b59578376d1238b3012a3cb52339978079f0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Přidání horizontálního oddílu pomocí nástroje elastické databáze
## <a name="tooadd-a-shard-for-a-new-range-or-key"></a>tooadd horizontálního oddílu pro nový rozsah nebo klíč
Aplikace často potřebují toosimply přidat nová data toohandle horizontálních oddílů, kterou je očekáván z nových klíčů nebo rozsahy klíčů pro ID horizontálního oddílu mapu, která již existuje. Například aplikace horizontálně dělené podle ID klienta může potřebovat tooprovision nové horizontálního oddílu pro nového klienta, nebo horizontálně dělené měsíční dat může být nutné nové horizontálních zřídit před hello začátek každý nový měsíc. 

Pokud hello nový rozsah hodnot klíče již není součástí existující mapování, je velmi jednoduchý tooadd hello nové horizontálního oddílu a přidružení hello nový klíč nebo rozsah toothat horizontálního oddílu. 

### <a name="example--adding-a-shard-and-its-range-tooan-existing-shard-map"></a>Příklad: Přidání horizontálního oddílu a jeho rozsah tooan existující horizontálního oddílu mapy
Tato ukázka používá hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metody a vytvoří instanci hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) třídy. V ukázce hello níže, databáze s názvem **sample_shard_2** a všechny objekty nezbytné schématu do něj byly vytvořeny toohold rozsah [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create hello mapping and associate it with hello new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Jako alternativu můžete použít Powershell toocreate nového správce mapy horizontálního oddílu. Příkladem je k dispozici [zde](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="tooadd-a-shard-for-an-empty-part-of-an-existing-range"></a>tooadd horizontálního oddílu pro prázdný součástí stávající rozsah
V některých případech je již namapována rozsah tooa horizontálních a částečně vyplněnou data, ale teď chcete nadcházející data toobe směrovanou tooa různých horizontálních. Například můžete horizontálního oddílu den rozsahu a již přiděleno horizontálního oddílu tooa 50 dnů, ale dne 24 chcete tooland budoucí data v různých horizontálního oddílu. elastické databáze Hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) mohou provést tuto operaci, ale pokud přesun dat není potřebné (například data pro hello rozsah dnů [25, 50), tj., včetně too50 den 25 vylučují, ještě neexistuje) můžete provést rozhraní API pro správu mapy horizontálního oddílu pomocí zcela hello přímo.

### <a name="example-splitting-a-range-and-assigning-hello-empty-portion-tooa-newly-added-shard"></a>Příklad: rozdělení rozsah a přiřazení hello prázdný část tooa nově přidané horizontálních
Databáze s názvem "sample_shard_2" a všechny objekty nezbytné schématu do něj byly vytvořeny.  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split hello Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) toodifferent shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline tooa different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Důležité**: použijte tento postup, jen pokud jste některé, které hello rozsah pro mapování hello aktualizovat je prázdný.  metody Hello výše Neprovádět kontrolu pro rozsah hello přesouvání dat, proto je vhodné tooinclude zkontroluje ve vašem kódu.  Pokud řádky existují v rozsahu hello přesouvání, nebude odpovídat hello skutečná data distribuční hello aktualizované horizontálních mapy. Použití hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello operace místo v těchto případech.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

