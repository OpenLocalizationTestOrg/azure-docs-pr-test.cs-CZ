---
title: "schémata aaaUser definované v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro používání schémata Transact-SQL v Azure SQL Data Warehouse pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c411d6fed68e67c444a5871eab06182eaeb6dbf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Uživatelem definované schémata v SQL Data Warehouse
Tradiční datových skladů často použijte samostatné databáze toocreate aplikace hranice podle zatížení, domény nebo zabezpečení. Tradiční datového skladu SQL serveru může například obsahovat pracovní databáze, databáze datového skladu a některé databáze datového tržiště. V této topologii každou databázi funguje jako hranice zabezpečení v architektuře hello a zatížení.

Naopak spouští SQL Data Warehouse hello celého datového skladu zatížení v rámci jedné databáze. Mezi databáze nejsou povoleny spojení. Proto SQL Data Warehouse očekává, že všechny tabulky použité ve toobe skladu hello uložené v rámci jedné databáze hello.

> [!NOTE]
> SQL Data Warehouse nepodporuje křížové databázové dotazy jakéhokoli druhu. V důsledku toho datového skladu implementace, které využívají tento vzor potřebovat toobe revize.
> 
> 

## <a name="recommendations"></a>Doporučení
Toto jsou doporučení pro konsolidaci úlohy, zabezpečení, domény a funkční hranice pomocí uživatelsky definované schémata

1. Použijte jeden toorun databáze SQL Data Warehouse úlohy celého datového skladu
2. Konsolidovat existující databázi datového skladu prostředí toouse jeden datový sklad SQL
3. Využívání **uživatelem definované schémata** hranic hello tooprovide dřív implementovaná pomocí databáze.

Pokud nebyly použity uživatelem definované schémata dříve máte čistou projektem. Jednoduše použijte hello starý název databáze jako hello základ pro váš vlastní schémata v databázi SQL Data Warehouse hello.

Pokud již byl použit schémata máte několik možností:

1. Odeberte hello starší verze schématu názvy a začít pracovat
2. Zachovat starší verze schématu názvy hello název tabulky toohello název starší verze schématu předem čekající hello
3. Zachovat starší verze schématu názvy hello implementací zobrazení hello tabulky v navíc schématu toore-vytvořit původní struktura schématu hello.

> [!NOTE]
> Na první kontroly zdánlivě možnost 3 jako možnost nejvíce přitažlivými hello. Je však hello ďábla podrobně hello. Zobrazení se čtou jenom v SQL Data Warehouse. Všechny změny dat nebo tabulka, potřebovat toobe adresovat hello základní tabulky. Možnost 3 také zavádí vrstvu zobrazení do systému. Můžete chtít toogive tento některé další myšlenku Pokud používáte zobrazení ve vaší architektury již.
> 
> 

### <a name="examples"></a>Příklady:
Implementovat vlastní schémata podle názvy databází

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for hello data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in hello stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in hello edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zachovat starší verze schématu názvy podle předem čekající je toohello název tabulky. Použijte schémata hello zatížení hranice.

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines hello data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend hello old schema name toohello table and create in hello staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend hello old schema name toohello table and create in hello data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Zachovat starší verze schématu názvy pomocí zobrazení

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines hello data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines hello legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create hello base staging tables in hello staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create hello base data warehouse tables in hello data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in hello legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Všechny změny ve schématu strategii potřebuje kontrolu hello model zabezpečení pro databázi hello. V mnoha případech je možné toosimplify model zabezpečení hello přiřazením oprávnění na úrovni schématu hello. Pokud jsou požadována oprávnění podrobnější můžete použít role databáze.
> 
> 

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
