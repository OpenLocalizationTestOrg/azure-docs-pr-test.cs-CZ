---
title: "Glosář nástroje databáze aaaElastic | Microsoft Docs"
description: "Vysvětlení termínů používaných pro elastické databáze nástroje"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d6573aad9a097e07135b0a64d1dafec19bb8cc7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-glossary"></a>Glosář nástroje elastické databáze
Hello následující termíny jsou definovány pro hello [nástroje elastické databáze](sql-database-elastic-scale-introduction.md), funkce Azure SQL Database. Hello nástroje jsou použité toomanage [mapuje horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md)a zahrnovat hello [klientské knihovny](sql-database-elastic-database-client-library.md), hello [nástroji pro sloučení rozdělení](sql-database-elastic-scale-overview-split-and-merge.md), [elastické fondy](sql-database-elastic-pool.md), a [dotazy](sql-database-elastic-query-overview.md). 

Tyto podmínky se používají v [přidání horizontálních, pomocí nástroje elastické databáze](sql-database-elastic-scale-add-a-shard.md) a [pomocí hello RecoveryManager třída toofix horizontálního oddílu mapy problémy](sql-database-elastic-database-recovery-manager.md).

![Elastické škálování podmínky][1]

**Databáze**: Azure SQL database. 

**Data závislé směrování**: hello funkci, která umožňuje horizontálních tooa tooconnect aplikace přiřazen klíč konkrétní horizontálního dělení. V tématu [závislé směrování dat](sql-database-elastic-scale-data-dependent-routing.md). Porovnání příliš**[dotazu víc horizontálních](sql-database-elastic-scale-multishard-querying.md)**.

**Globální horizontálních mapy**: hello mapování mezi horizontálního dělení klíčů a jejich odpovídajících horizontálních oddílů v rámci **horizontálního oddílu sadu**. Mapa globální horizontálních Hello je uložen v hello **správce mapy horizontálního oddílu**. Porovnání příliš**místní horizontálních mapy**.

**Mapování horizontálních seznamu**: horizontálního oddílu mapy, ve které horizontálního dělení klíči jsou namapované jednotlivě. Porovnání příliš**rozsah horizontálního oddílu mapy**.   

**Mapa místního horizontálních**: uložené na horizontálního oddílu, hello místní horizontálních mapy obsahuje mapování hello shardlets nacházející se na hello horizontálního oddílu.

**Dotaz s více horizontálních**: hello možnost tooissue dotaz víc horizontálních oddílů; nastaví výsledky se vrátí pomocí sémantiky UNION ALL (také označované jako "fan-out dotaz"). Porovnání příliš**závislé směrování dat**.

**Víceklientské** a **jednoho klienta**: Zobrazí databázi jednoho klienta a víceklientské databáze:

![Databáze jeden a více klientů](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Zde je reprezentace **horizontálně dělené** databáze jednoho a víc klientů. 

![Databáze jeden a více klientů](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Mapování horizontálních rozsah**: horizontálního oddílu mapy, ve které hello horizontálního oddílu distribuční strategie vychází z více oblastí souvislý hodnot. 

**Referenční tabulky**: tabulek, které nejsou horizontálně dělené ale se replikují napříč horizontálních oddílů. Například zip kódy, které mohou být uloženy v referenční tabulce. 

**Horizontálního oddílu**: Azure SQL database, která ukládá data z horizontálně dělené datové sady. 

**Pružnost horizontálního oddílu**: hello možnost tooperform **vodorovné škálování** a **svislé škálování**.

**Horizontálně dělené tabulky**: tabulky, které jsou horizontálně dělené, tj., jejichž data se distribuuje do horizontálních oddílů na základě jejich hodnot klíče horizontálního dělení. 

**Klíč horizontálního dělení**: hodnota sloupce, který určuje, jak se data distribuuje do horizontálních oddílů. Hello typ hodnoty může mít jednu z následujících hello: **int**, **bigint**, **varbinary**, nebo **uniqueidentifier**. 

**Sada horizontálního oddílu**: hello kolekce horizontálních oddílů, které jsou s atributy toohello stejné ID horizontálního oddílu mapy ve hello horizontálního oddílu mapa správce.  

**Shardlet**: všechna data hello přidružené jednu hodnotu klíče horizontálního dělení na horizontálního oddílu. Shardlet je nejmenší jednotka hello přesun dat je možné při Redistribuce horizontálně dělené tabulky. 

**Mapování horizontálních**: hello sada mapování mezi horizontálního dělení klíčů a jejich odpovídajících horizontálních oddílů.

**Správce mapy horizontálního oddílu**: Správa objektů a dat úložiště, které obsahuje hello horizontálního oddílu map(s), horizontálního oddílu umístění a mapování pro jednu nebo více sad horizontálního oddílu.

![Mapování][2]

## <a name="verbs"></a>Příkazy
**Vodorovné škálování**: hello operace škálování out (nebo v) kolekce horizontálních oddílů přidáním nebo odebráním horizontálních oddílů tooa horizontálního oddílu mapu, jak je uvedeno níže.

![Vodorovného a svislého škálování][3]

**Sloučení**: hello operace přesunutí shardlets ze dvou horizontálních oddílů tooone horizontálních a odpovídajícím způsobem aktualizace hello horizontálního oddílu mapy.

**Přesunutí Shardlet**: hello act přesunu jednom shardlet tooa různých horizontálního oddílu. 

**Horizontálního oddílu**: hello act vodorovně oddíly stejně jako strukturovaná data napříč více databází na základě klíče horizontálního dělení.

**Rozdělení**: hello v rámci přechod z jednoho horizontálních tooanother (obvykle novou) horizontálních několik shardlets. Klíč horizontálního dělení je poskytovaná v rámci hello uživatele hello rozdělení bodu.

**Svislé škálování**: hello operace škálování nahoru (nebo dolů) hello úroveň výkonu jednotlivých horizontálního oddílu. Například změna horizontálního oddílu z standardní tooPremium (což vede k více výpočetní prostředky). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

