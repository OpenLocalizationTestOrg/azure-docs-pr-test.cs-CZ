---
title: postupy aaaStored v SQL Data Warehouse | Microsoft Docs
description: "Tipy pro implementaci uložené procedury v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 416252dd3dea95c66aa5e886860b933b22578002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Uložené procedury v SQL Data Warehouse
SQL Data Warehouse podporuje mnoho funkcí hello Transact-SQL, najít v systému SQL Server. Je důležité nejsou specifické funkce, že chceme tooleverage toomaximize hello výkon vašeho řešení s více instancemi.

Toomaintain hello škálování a výkonu, služby SQL Data Warehouse jsou však také některé funkce a funkce, které mají rozdíly v chování a ostatní uživatele, které nejsou podporovány.

Tento článek vysvětluje, jak tooimplement uložené procedury v rámci SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Představení uložené procedury
Uložené procedury jsou vhodné pro zapouzdření kódu SQL; ukládání dat zavřít tooyour hello datového skladu. Zapouzdřením hello kódu do spravovatelných jednotek uložené procedury pomoci vývojářům rozčlenění moduly svá řešení; usnadnění větší znovuvyužití kódu. Každý uložené procedury může přijmout také parametry toomake je i flexibilnější.

SQL Data Warehouse poskytuje zjednodušenou a efektivní uložené procedury implementaci. Hello největších rozdíl oproti tooSQL, které je Server, hello uložená procedura není předem zkompilovaný kód. V datové sklady jsou obecně menší význam s časem kompilace hello. Je důležité hello uložené procedury kód je při fungování proti velkých objemů dat správně optimalizované. cílem Hello je toosave hodiny, minuty a sekundy není milisekundách. Proto je užitečné toothink uložené procedury jako kontejnery pro logiku SQL.     

Když SQL Data Warehouse provede jsou příkazy SQL hello vaše uložené procedury analyzovat, přeložit a optimalizované v době běhu. Během tohoto procesu je každý příkaz převeden na distribuované dotazy. Hello SQL kód, který je ve skutečnosti spustit pro hello data je jiný toohello dotazu odeslána.

## <a name="nesting-stored-procedures"></a>Vnoření uložené procedury
Když uložené procedury volat jiné uložené procedury nebo spuštění dynamické sql pak hello vnitřní uložené procedury nebo kód volání říká, že je toobe vnořený.

SQL Data Warehouse podporují maximálně 8 vnořených úrovní. Toto je mírně odlišný tooSQL serveru. úroveň vnoření Hello v systému SQL Server je 32.

volání uložené procedury nejvyšší úrovně Hello znamená zároveň toonest úroveň 1

```sql
EXEC prc_nesting
```
Pokud hello uložené procedury navíc využívá další EXEC volání pak to zvýší too2 úrovně vnoření hello

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Pokud se druhý postup hello pak provede některé dynamické sql pak to zvýší too3 úrovně vnoření hello

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Poznámka: SQL Data Warehouse nepodporuje aktuálně@NESTLEVEL. Budete potřebovat tookeep sledovat vaše úrovně vnoření sami. Nepravděpodobné, se setkají limit úrovně vnoření hello 8, ale pokud uděláte budete potřebovat toore pracovní kódu a "zploštění" jej tak, aby odpovídal v rámci tohoto limitu.

## <a name="insertexecute"></a>PŘÍKAZ INSERT... SPUŠTĚNÍ
SQL Data Warehouse vám nepovoluje tooconsume hello sady výsledků dotazu uložené proceduře pomocí příkazu INSERT. Je však alternativní způsob, který můžete použít.

Naleznete v následujícím článku toohello [dočasných tabulek] pro příklad jak toodo to.

## <a name="limitations"></a>Omezení
Existují některé aspekty Transact-SQL uložené procedury, které nejsou implementované v SQL Data Warehouse.

Jsou:

* dočasně uložených procedur
* číslované uložené procedury
* Rozšířené uložené procedury
* CLR uložené procedury
* možnost šifrování
* možnost replikace
* Parametry s hodnotou tabulky
* parametry jen pro čtení
* výchozí parametry
* kontexty provádění
* Return – příkaz

## <a name="next-steps"></a>Další kroky
Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].

<!--Image references-->

<!--Article references-->
[dočasných tabulek]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
