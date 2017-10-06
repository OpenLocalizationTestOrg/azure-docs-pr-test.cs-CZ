---
title: aaaTransactions v SQL Data Warehouse | Microsoft Docs
description: "Tipy pro provádění transakcí v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 7c541648553238443b407666612561918096eb61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-in-sql-data-warehouse"></a>Transakce v SQL Data Warehouse
Jak jste zvyklí, SQL Data Warehouse podporuje transakce v rámci úlohy datového skladu hello. Ale tooensure hello výkon služby SQL Data Warehouse je udržován na úrovni škálování některé funkce jsou omezené při porovnání tooSQL serveru. V tomto článku klade důraz hello rozdíly a seznamy hello ostatní. 

## <a name="transaction-isolation-levels"></a>Úrovně izolace transakce
SQL Data Warehouse implementuje transakce ACID. Hello izolace podpora transakcí hello je však omezená příliš`READ UNCOMMITTED` a toto nastavení nelze změnit. Můžete implementovat řadu kódování metod, které tooprevent nekonzistence čte dat, pokud se jedná o problém pro vás. využít funkce CTAS a přepnutí oddílu tabulky (často označované jako posuvné okno vzor hello) zprostředkovatele Hello nejoblíbenější metody tooprevent uživatele z dotazování na data, která stále připraveném. Zobrazení, které předem filtrování dat hello je také oblíbených přístup.  

## <a name="transaction-size"></a>Velikost transakce
Transakce úpravy jednoho datového je omezen velikostí. Hello limit dnes platí "za distribuční". Proto lze vypočítat celkové přidělení hello vynásobením hello limit počtu distribučních hello. tooapproximate hello maximální počet řádků v transakci hello dělení hello distribuční cap hello celková velikost každý řádek. Pro proměnnou délkou zvažte trvá délka průměrná sloupce, nikoli pomocí hello maximální velikost.

V tabulce hello hello byly provedeny následující předpoklady:

* Rovnoměrné rozdělení dat došlo k chybě. 
* délka řádku průměrná Hello je 250 bajtů

| [DWU][DWU] | Cap za distribuční (GiB) | Počet distribuce | MAXIMÁLNÍ velikost transakce (GiB) | # Řádků na jeden distribuce | Maximální počet řádků na transakci |
| --- | --- | --- | --- | --- | --- |
| OD DW100 |1 |60 |60 |4,000,000 |240,000,000 |
| DW200 |1.5 |60 |90 |6 000 000 |360,000,000 |
| DW300 |2.25 |60 |135 |9,000,000 |540,000,000 |
| DW400 |3 |60 |180 |12,000,000 |720,000,000 |
| DW500 |3.75 |60 |225 |15,000,000 |900,000,000 |
| DW600 |4.5 |60 |270 |18,000,000 |1,080,000,000 |
| DW1000 |7.5 |60 |450 |30,000,000 |1,800,000,000 |
| DW1200 |9 |60 |540 |36,000,000 |2,160,000,000 |
| DW1500 |11.25 |60 |675 |45,000,000 |2,700,000,000 |
| DW2000 |15 |60 |900 |60,000,000 |3,600,000,000 |
| DW3000 |22.5 |60 |1,350 |90,000,000 |5,400,000,000 |
| DW6000 |45 |60 |2,700 |180,000,000 |10,800,000,000 |

omezení velikosti transakce Hello se použije pro transakci nebo operace. Nebude použito v rámci všech souběžných transakcí. Proto každou transakci je povolené toowrite toto množství dat toohello protokolu. 

toooptimize a minimalizovat hello množství dat zapsaných toohello protokolu naleznete toohello [transakce osvědčené postupy] [ Transactions best practices] článku.

> [!WARNING]
> maximální velikost transakce lze dosáhnout pouze pro hodnoty HASH nebo kde hello šířit z tabulky ROUND_ROBIN distribuované hello data Hello je i. Pokud transakce hello je zápis dat zkreslilo způsobem toohello distribuce hello limit je pravděpodobně toobe nedosáhla předchozí toohello transakce maximální velikosti.
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a>Stav transakce
SQL Data Warehouse používá hello XACT_STATE() funkce tooreport selhání transakce pomocí hodnoty hello -2. To znamená, že hello transakce se nezdařilo a je označen pro vrácení zpět pouze

> [!NOTE]
> Hello použít-2 pomocí hello XACT_STATE funkce toodenote selhání transakce představuje různé chování tooSQL serveru. Používá systém SQL Server hello hodnota -1 toorepresent rámci transakce. SQL Server může tolerovat chybami v transakci bez nutnosti toobe označena jako rámci. Například `SELECT 1/0` by způsobit chybu, ale bez vynucení transakce v rámci stavu. Systému SQL Server také umožňuje čtení v rámci transakce hello. Ale SQL Data Warehouse, nebudou vám to. Pokud dojde k chybě v transakci SQL Data Warehouse zadá automaticky stavu hello -2 a nebude ji již možné toomake všechny příkazy další výběr, dokud příkaz hello byla vrácena zpět. Proto je důležité toocheck, že vaše toosee kód aplikace, pokud používá XACT_STATE() podle může být nutné změny kódu toomake.
> 
> 

V systému SQL Server se může zobrazit transakci, která vypadá takto:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Pokud necháte kódu, protože je nad obdržíte hello následující chybová zpráva:

Msg 111233, Level 16, State 1, řádek 1 111233; hello aktuální transakce byla přerušena a všechny neuložené změny byly vráceny zpět. Příčina: Transakce ve stavu jen pro vrácení zpět nebyla vrácena explicitně před DDL, DML nebo příkazu SELECT.

Také nebude získat výstup hello hello ERROR_ * funkcí.

V SQL Data Warehouse potřebuje hello kód toobe mírně změnit:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Hello očekává, že je nyní zjištěnými chování. Hello došlo k chybě v transakci hello je spravovat a hello ERROR_ * funkce zadejte hodnoty, podle očekávání.

Všechno, co došlo ke změně této hello je `ROLLBACK` z hello transakce měl toohappen než hello čtení hello informací o chybách v hello `CATCH` bloku.

## <a name="errorline-function"></a>Error_Line() – funkce
Je také vhodné poznamenat, že SQL Data Warehouse implementovat nebo podporují funkce ERROR_LINE() hello. Pokud máte ve vašem kódu to budete potřebovat tooremove ho toobe kompatibilní s SQL Data Warehouse. Místo toho použijte popisky dotazu v kódu tooimplement ekvivalentní funkce. Podrobnosti najdete toohello [popisek] [ LABEL] další podrobnosti o této funkci najdete v článku.

## <a name="using-throw-and-raiserror"></a>Pomocí THROW a RAISERROR
THROW je hello více moderní implementace pro vyvolávání výjimek v SQL Data Warehouse, ale RAISERROR je také podporována. Existuje několik rozdílů, které jsou vhodné platícího toohowever pozornost.

* Uživatelem definované chybové zprávy, že čísla nemůže být v hello 150 100 000 000 rozsah THROW
* Chybové zprávy RAISERROR stanoví na 50 000
* Použití systémových není podporováno.

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse má několik omezení, které se týkají tootransactions.

Ty jsou následující:

* Žádné distribuovaných transakcí
* Žádné vnořené transakce povolené
* Bez uložení bodů povoleno
* Žádné pojmenované transakce
* Žádné označené transakce
* Žádná podpora pro DDL jako `CREATE TABLE` uvnitř uživatele definované transakce

## <a name="next-steps"></a>Další kroky
toolearn Další informace o optimalizaci transakce, najdete v části [transakce osvědčené postupy][Transactions best practices].  toolearn o ostatní osvědčené postupy SQL Data Warehouse, najdete v části [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse best practices].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
