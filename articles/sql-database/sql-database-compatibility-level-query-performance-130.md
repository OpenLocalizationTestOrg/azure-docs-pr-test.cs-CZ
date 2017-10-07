---
title: "úroveň kompatibility aaaDatabase 130 – Azure SQL Database | Microsoft Docs"
description: "V tomto článku jsme prozkoumat hello výhody spuštění vaší databázi SQL Azure s úrovní kompatibility 130 a využívat výhody hello hello nový Optimalizátor dotazů a dotaz funkce procesoru. Také jsme adres hello nežádoucí vliv na výkon dotazů hello pro existující aplikace SQL hello."
services: sql-database
documentationcenter: 
author: alainlissoir
manager: jhubbard
editor: 
ms.assetid: 8619f90b-7516-46dc-9885-98429add0053
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.topic: article
ms.date: 08/08/2016
ms.author: alainl
ms.openlocfilehash: 25693c5f7b01405b7073fa7d4cc2833fbe125e2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Zvýšení výkonu dotazů s kompatibilitou úrovně 130 ve službě Azure SQL Database
Databáze SQL Azure běží transparentně stovky tisíc databází na mnoha různých kompatibility úrovních, zachování a zaručující hello zpětné kompatibility toohello odpovídající verzi systému Microsoft SQL Server pro všechny své zákazníky!

V tomto článku jsme prozkoumat hello výhody spuštění vaše připojení databáze SQL Azure s úrovní kompatibility 130 a využívat výhody hello hello nový Optimalizátor dotazů a dotaz funkce procesoru. Také jsme adres hello nežádoucí vliv na výkon dotazů hello pro existující aplikace SQL hello.

Připomínáme historie zarovnání hello úrovně kompatibility toodefault verze SQL jsou následující:

* 100: v systému SQL Server 2008 a Azure SQL Database verze 11.
* 110: v systému SQL Server 2012 a Azure SQL Database verze 11.
* 120: v systému SQL Server 2014 a Azure SQL Database verze 12.
* 130: v systému SQL Server 2016 a Azure SQL databáze verze 12.

> [!IMPORTANT]
> Počínaje **mid června 2016**, v databázi SQL Azure, bude úroveň kompatibility výchozí hello 130 místo 120 pro **nově vytvořený** databáze.
> 
> Databáze vytvořené před mid června 2016 se *není* mít vliv a bude udržovat své aktuální úroveň kompatibility (100, 110 nebo 120). Databáze, které se migrovat z verzí Azure SQL Database verze 11 tooV12 bude mít úrovní kompatibility 110 nebo 100. 
> 

## <a name="about-compatibility-level-130"></a>O úroveň kompatibility 130
První Pokud chcete, aby tooknow hello aktuální úroveň kompatibility databáze, spouštění hello následující příkaz jazyka Transact-SQL.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Před provedením této změny se stane toolevel 130 pro **nově** vytvořené databáze, můžeme zkontrolujte co tato změna se točí kolem prostřednictvím příklady velmi základního dotazu a v tématu Jak každý, kdo může těžit z něj.

Zpracování dotazu v relační databáze může být velmi složité a může vést toolots počítače vědy a matematika toounderstand hello vyplývajících volbách návrhu a chování. V tomto dokumentu hello obsah byl záměrně zjednodušené tooensure každý, kdo má minimální technické základní můžete porozumět dopadu hello Změna úrovně kompatibility hello a určit, jak se může hodit aplikace.

Umožňuje rychlý podívejte se na úroveň kompatibility hello 130 přináší v tabulce hello.  Můžete najít další podrobnosti o [změnit úroveň kompatibility databáze (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ale zde je uveden krátký:

* Hello operace Insert příkazu Insert vyberte může být Vícevláknová nebo může mít paralelní plán při před jednovláknové tuto operaci.
* Paměť optimalizovaný tabulka a tabulka proměnné dotazy mohou mít nyní paralelní plány při předtím, než tato operace se také jednovláknové.
* Statistiky pro paměťově optimalizované tabulky se dá teď vzorkovat a jsou automaticky aktualizovány. V tématu [co je nového ve službě databázového stroje: OLTP v paměti](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) další podrobnosti.
* V režimu dávky nebo s režimu Row změny s indexy úložiště sloupců
  * Řazení v tabulce s indexem úložiště sloupce jsou nyní v dávkovém režimu.
  * Agregace oddílová nyní pracovat v dávkovém režimu například TSQL funkce LAG nebo realizace příkazy.
  * Dotazy na úložiště sloupce tabulky s více distinct klauzulí pracovat v dávkovém režimu.
  * Dotazy spuštěnými na úrovni DOP = 1 nebo sériového plán také spustit v dávkovém režimu.
* Poslední vylepšení odhadu kardinality ve skutečnosti přicházejí s úrovní kompatibility 120, ale pro těch, které je spuštěna v kompatibility nižší úrovně (tj. 100 nebo 110), hello přesunutí toocompatibility úroveň 130 se rovněž otevře tato vylepšení a tyto může taky využívat hello dotazu výkon aplikací.

## <a name="practicing-compatibility-level-130"></a>Cvičení úroveň kompatibility 130
První Pojďme některé tabulky, indexy a náhodná data vytvořená toopractice některé z těchto nových funkcí. Příklady skriptů TSQL Hello mohou být provedeny v části SQL Server 2016 nebo v databázi SQL Azure. Při vytváření databáze Azure SQL, ujistěte se, ale v hello minimální P2 databázi, protože je nutné vybrat alespoň několik jader tooallow více vláken a proto těžit z těchto funkcí.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- hello second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nyní budeme mít vzhled toosome funkcí hello zpracování dotazu bude s úrovní kompatibility 130.

## <a name="parallel-insert"></a>Paralelní vložení
Provádění příkazů TSQL hello níže provede hello operace INSERT pod úroveň kompatibility 120 a 130, který v uvedeném pořadí provede hello operace INSERT v jednom zařazování modelu (120) a ve model Vícevláknová (130).

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- hello INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- hello INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Tím, že požádá plán dotazu hello skutečné hello prohlížení jeho grafické vyjádření, její obsah XML, můžete určit, které mohutnost odhad funkce je v play. Prohlížení hello plány-souběžného na obrázku 1, jsme jasně uvidí, že tento hello vložit sloupec úložiště provádění přejde z sériového portu v 120 tooparallel ve 130. Mějte také na paměti tato změna hello hello iterator ikony v hello zobrazující dva paralelní šipky, ilustrující hello skutečnost, že nyní hello iterator spuštění je skutečně paralelní 130 plánu. Pokud máte velké toocomplete operace INSERT, hello paralelní provádění, propojené toohello počet jader, které máte k dispozici pro databázi hello provede lépe; až tooa 100krát rychlejší podle vaší konkrétní situace!

*Obrázek 1: VLOŽTE operace změny z sériové tooparallel s úrovní kompatibility 130.*

![Obrázek 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>SÉRIOVÉ dávkovém režimu
Podobně přesunutí toocompatibility úroveň 130 při zpracování řádky dat umožňuje režimu zpracování dávky. Nejprve dávkových operací režimu jsou dostupná pouze při mají index úložiště sloupců na místě. Druhý dávky obvykle představuje ~ 900 řádky a používá kódu logiku, optimalizované pro vícejádrovými procesoru, vyšší propustnost paměti a přímo využívá hello komprimovaná data hello sloupec úložiště, pokud je to možné. Za těchto podmínek SQL serveru 2016 zpracovávat ~ 900 řádků najednou, namísto 1 řádek během hello, a v důsledku toho hello celkové režijní náklady operace hello je nyní sdílet hello celý batch, snižuje celkové náklady řádek hello. Toto množství sdílených operací v podstatě v kombinaci s hello sloupec úložiště komprese snižuje latence hello zahrnutých v režimu vyberte dávkovou operaci. Můžete najít další podrobnosti o hello sloupec úložiště a dávky režimu v [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Jako viditelná níže sledování hello dotazu plány-souběžného na obrázku 2, jsme lze zjistit, že došlo ke změně režimu zpracování hello s úrovní kompatibility hello a v důsledku toho při provádění dotazů hello v obou úroveň kompatibility zcela, jsme uvidíte, že většinu doby zpracování hello je využita pro práci v režimu (86 %) porovnání toohello batch režimu row (14 %), kde byly zpracovány 2 dávky. Zvýšit hello datovou sadu, hello benefit zvýší.

*Obrázek 2: Vyberte operace změny z sériové toobatch režimu s úrovní kompatibility 130.*

![Obrázek 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>Dávkovém režimu na provedení řazení
Podobně jako toohello výše, ale operace řazení použité tooa, hello přechod z režimu toobatch režimu (úroveň kompatibility 120) řádku (úroveň kompatibility 130) zvyšuje výkon hello hello operace řazení pro hello ze stejných důvodů.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- hello scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- hello scan and aggregate are in batch mode,
-- and force MAXDOP too1 tooshow that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Viditelné-souběžného na obrázku 3, vidíme, že operace řazení hello v režimu row představuje 81 % hello náklady, zatímco hello dávkovém režimu pouze představuje % 19 hello náklady (v uvedeném pořadí 81 % a % 56 na hello řazení samotné).

*Obrázek 3: Operace řazení se změní z řádku toobatch režimu s úrovní kompatibility 130.*

![Obrázek 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Samozřejmě tyto ukázky obsahovat pouze desetitisíce řádků, který není nic při prohlížení hello datech dostupných v většina serverů SQL dní. Právě tyto proti miliony řádků místo projektu a tento fakt může projevit v několika minutách provádění ušetřena každý den čekající na vyřízení hello povaze vašich úloh.

## <a name="cardinality-estimation-ce-improvements"></a>Vylepšení mohutnost odhad (CE)
Zavedena v systému SQL Server 2014, všechny databáze s úrovní kompatibility 120 nebo vyšší budou používat nové funkce odhadu kardinality hello. V podstatě mohutnost odhad je logiku hello používá toodetermine jak systému SQL server provede dotaz založený na jeho odhadované náklady. odhad Hello se počítá pomocí vstup z statistiky přidružené objekty zahrnutých v tomto dotazu. Prakticky, v hlavní funkce odhadu kardinality jsou odhady počet řádek společně s informacemi o hello distribuci hodnot hello počty odlišné hodnoty, a duplicitní počty obsažené v hello tabulky a objekty odkazovaná v dotazu hello. Získávání tyto odhady chyba, může způsobit vstupy/výstupy disků toounnecessary kvůli tooinsufficient paměti uděluje (tj. v databázi TempDB úniky) nebo výběr tooa sériové plán spuštění přes paralelní plán spuštění, tooname pár. Uzavření, může způsobit nesprávné odhady tooan celkové snížení výkonu hello provedení dotazu. Na hello druhé straně lépe odhadne, přesnější odhady, zájemců toobetter dotazu spuštěních!

Jak je uvedeno nahoře, optimalizace dotazu a odhady jsou komplexní záležitosti, ale pokud chcete více informací o plány dotazů a architektury odhadu kardinality toolearn, můžete se podívat toohello dokumentu v [optimalizace vaše plány dotazů s hello SQL Server 2014 Odhadu kardinality](https://msdn.microsoft.com/library/dn673537.aspx) pro podrobnější prohlídku.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Které odhadu kardinality právě používáte?
toodetermine v rámci které mohutnost odhad běží vaše dotazy, budeme právě hello dotazu použijte ukázky níže. Všimněte si, že tento první příklad budou spouštěny pod úrovní kompatibility 110, zdání hello použití hello staré mohutnost odhad funkce.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Po dokončení provádění klikněte na odkaz hello XML a podívejte se na vlastnosti hello hello první iterator, jak je uvedeno níže. Poznámka: název vlastnosti hello názvem CardinalityEstimationModelVersion nastaveno na 70. Neznamená to, že hello úroveň kompatibility databáze je nastavena toohello verze SQL Server 7.0 (je nastavený na 110 jako viditelné v příkazech TSQL hello výše), ale hodnota hello 70 jednoduše představuje hello starší verze mohutnost odhad funkce je k dispozici od SQL Server 7.0, která měla žádné hlavní revize až SQL Server 2014 (který se dodává s úrovní kompatibility 120).

*Obrázek 4: hello CardinalityEstimationModelVersion nastavena too70, při použití úrovní kompatibility 110 nebo níže.*

![Obrázek 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Alternativně můžete změnit too130 úrovně kompatibility hello a zakázání hello použití nové funkce odhadu kardinality hello pomocí hello LEGACY_CARDINALITY_ESTIMATION nastavit tooON s [změnit OBOR konfigurace databáze](https://msdn.microsoft.com/library/mt629158.aspx). To bude mít přesně hello stejné jako pomocí 110 z odhadu kardinality funkce hlediska, při použití hello nejnovější dotaz zpracování úroveň kompatibility. To uděláte, můžete těžit z hello nový dotaz zpracování funkce přicházející s úrovní kompatibility nejnovější hello (tj. v dávkovém režimu), ale stále závisí na hello staré odhadu kardinality funkcích v případě potřeby.

```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Přesunutí jednoduše úroveň kompatibility toohello 120 nebo 130 umožňuje nové funkce odhadu kardinality hello. V takovém případě výchozí hello CardinalityEstimationModelVersion se nastaví odpovídajícím způsobem too120 nebo 130 jako zobrazené níže.

```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Obrázek 5: hello CardinalityEstimationModelVersion nastavena too130, pokud používáte úroveň kompatibility 130.*

![Obrázek 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-hello-cardinality-estimation-differences"></a>Jeho práci rozdíly odhadu kardinality hello
Teď umožňuje spustit trochu složitější dotaz zahrnující vnitřního spojení s klauzulí WHERE se některé predikáty a podíváme se na odhad počtu řádků hello z původního mohutnost odhad funkce hello nejdřív.

```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Efektivní provádění tohoto dotazu vrátí 200,704 řádky, zatímco hello odhad řádek s funkcemi odhadu kardinality staré hello deklarací 194,284 řádků. Samozřejmě jak je uvedená před, tyto řádků počet výsledků bude také záviset jak často jste spustili hello předchozí ukázky, která naplní hello ukázkové tabulky opakovaně v každé spuštění. Predikáty hello v dotazu samozřejmě, bude mít vliv na skutečný odhad hello kromě zajištění dostatečného hello tabulky tvaru, data obsahu a jak tato data ve skutečnosti korelaci mezi sebou.

*Obrázek 6: odhad počtu řádků hello vypnuté 194,284 nebo 6 000 řádků z hello 200,704 řádky očekává.*

![Obrázek 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

V hello stejným způsobem, můžeme provést nyní hello stejný dotaz s novými funkcemi odhadu kardinality hello.

```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Prohlížení hello níže, teď vidíme, že tento řádek odhad hello je 202,877, nebo mnohem blíže a vyšší, než hello staré odhadu kardinality.

*Obrázek 7: odhad počtu řádků hello je nyní 202,877 místo 194,284.*

![Obrázek 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

Ve skutečnosti je sada výsledků dotazu hello 200,704 řádků (ale všechny závisí, jak často se spustit hello dotazy pro hello v předchozí ukázky, ale je důležité, protože hello TSQL používá příkaz RAND() hello, hello skutečných hodnot, vrátí se můžou lišit od jednoho spuštění toohello Další). Proto v tomto konkrétním příkladu hello nové odhadu kardinality nepodporuje lépe daří při odhadování hello počet řádků, protože 202,877 je mnohem blíže too200, 704, než 194,284! Poslední, pokud změníte hello klauzule WHERE predikáty tooequality (místo ">" pro instanci), to se ověřte hello odhady mezi hello staré a nové funkce mohutnost i více různých, v závislosti na tom, kolik odpovídá můžete získat.

Samozřejmě v takovém případě se ~ 6000 řádků vypnout z skutečný počet nepředstavuje velké množství dat v některých situacích. Nyní, transponuje tuto toomillions řádků napříč několika tabulek a složitější dotazy a v některých případech hello odhad lze vypnout podle miliony řádky a proto hello riziko nesprávné, že plán spuštění, nebo pro požadování nedostatek paměti uděluje úvodní výdej up hello tooTempDB úniky a proto více vstupně-výstupních operací, jsou mnohem vyšší.

Pokud máte možnost hello, praxi toto porovnání s nejčastější dotazy a datové sady a zobrazit si sami, jak moc některé hello starý a nový odhady jsou ovlivněni, zatímco některé by se mohly právě stát další vypnout ze skutečnosti hello nebo některé, ostatní stačí jednoduše co nejblíže toohello skutečné řádek počty ve skutečnosti, vrátí se v sadách výsledků hello. Všechny záviset hello tvaru dotazy, hello Azure SQL database charakteristiky, hello povaze a hello velikost datových sad a hello statistiky o je k dispozici. Pokud jste právě vytvořili instanci služby Azure SQL Database, hello dotazu pro optimalizaci bude mít toobuild spustí jeho znalostní báze od začátku místo opětovné použití statistiky, které se skládají z předchozího dotazu hello. Ano jsou odhady hello tooevery velmi kontextové a téměř konkrétní situaci serveru a aplikace. Je důležitým aspektem tookeep pamatovat!

## <a name="some-considerations-tootake-into-account"></a>Některé aspekty tootake do účtu
I když většina úloh by využívat úroveň kompatibility hello 130, před přijetím hello úroveň kompatibility pro produkční prostředí, máte v podstatě 3 možnosti:

1. Přesunout toocompatibility úroveň 130 a v tématu Jak provádět věcí. V případě, že jste si všimli některé regresí, stačí jednoduše nastavit hello kompatibility úrovně back tooits původní úroveň, nebo zachovat 130 a pouze reverse hello odhadu kardinality back toohello režim starší verze (jak je popsáno výše, to samostatně může hello problém vyřešit).
2. Důkladně otestovat existující aplikace podobné produkční zatížení, vyladit a ověření výkonu hello před tooproduction probíhající. V případě problémů stejné jako výše, můžete vždy vrátit původní úroveň kompatibility toohello nebo jednoduše reverse hello odhadu kardinality back toohello režim starší verze.
3. Jako poslední možnost a hello nejnovější tooaddress způsob, jak tyto otázky, je úložiště dotazů tooleverage hello. To je doporučená možnost dnešní! Analýza hello tooassist vašich dotazů v rámci kompatibility na úrovni 120 nebo pod versus 130, nelze doporučujeme dostatek toouse úložiště dotazů. Úložiště dotazů je k dispozici v nejnovější verzi Azure SQL Database V12 hello a je navržena toohelp k řešení potíží s výkonem dotazu. Hello úložiště dotazů si můžete představit jako záznam dat pro vaši databázi shromažďování a nabízí podrobné historické informace o všech dotazů. To výrazně zjednodušuje forenzních výkon snížením hello čas toodiagnose a vyřešte problémy. Další informace najdete [úložiště dotazů: záznam dat pro vaši databázi](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

Na vysoké úrovni, pokud již máte sadu databází s úrovní kompatibility 120 nebo níže a plánování toomove hello některá z nich too130, nebo protože vaše úlohy automaticky zřizovat nové databáze, které budou brzy se ve výchozím nastavení too130 nastavení, zvažte prosím Hello těchto hodnot:

* Před změnou toohello novou úroveň kompatibility v produkčním prostředí, povolte úložiště dotazů. Můžete se podívat příliš[změna hello režim kompatibility databáze a používání hello dotazu úložiště](https://msdn.microsoft.com/library/bb895281.aspx) Další informace.
* V dalším kroku otestujte všechny kritické úlohy pomocí reprezentativní dat a dotazy z prostředí produkčního prostředí a porovnání výkonu hello došlo a jako hlášené pomocí úložiště dotazů. Pokud dojde k některé regresí, můžete identifikovat hello který poklesl dotazy s hello úložiště dotazů a použít plán hello vynucení možnost úložiště dotazů (neboli plán Připnutí). V takovém případě můžete platností zůstat s úrovní kompatibility hello 130 a používat plán hello bývalé dotazu na návrh hello úložiště dotazů.
* Pokud chcete tooleverage nové funkce a možnosti služby Azure SQL Database (který je spuštěn SQL Server 2016), ale jsou citlivé toochanges uvést do režimu podle úrovně kompatibility hello 130, jako poslední možnost, zvažte vynucení hello úroveň kompatibility zpět úroveň toohello, který vyhovuje vaše úlohy pomocí příkazu ALTER DATABASE. Ale nejprve, mějte na paměti, že plán úložiště dotazů hello Připnutí možnost je nejlepší možnost, protože není pomocí 130 je v podstatě zachování na úrovni funkcí hello starší verze systému SQL Server.
* Pokud máte víceklientské aplikace pokrývání uzlů více databází, může být nutné tooupdate hello zřizovat logiku vaší databáze tooensure úroveň kompatibility konzistentní v rámci všech databází; starý a nově zřízeného ty. Výkonu úloh aplikace může být citlivé toohello skutečnost, že některé databáze, které jsou spuštěné v různých kompatibility úrovně, a proto může být požadované kompatibility úrovně konzistence napříč všechny databáze v pořadí tooprovide hello stejné prostředí zákazníků tooyour všechny napříč hello panelu. Všimněte si, že se nejedná o pověření, ve skutečnosti závisí na tom, jak vaše aplikace je ovlivňován hello úroveň kompatibility.
* Poslední, týkající se hello mohutnost odhad a stejně jako změna úrovně kompatibility hello, než budete pokračovat v produkčním prostředí, je doporučené tootest vaše produkční úlohy v rámci nové podmínky toodetermine hello, pokud vaše aplikace výhody z vylepšení odhadu kardinality Hello.

## <a name="conclusion"></a>Závěr
Používání Azure SQL Database může toobenefit ze všech vylepšení SQL Server 2016 jasně zvýšit spuštěních vašeho dotazu. Stejně jako-je! Samozřejmě podobně jako u jakékoli nové funkce, řádné hodnocení je třeba provést toodetermine hello přesné podmínky, za kterých vaše databáze zatížení funguje hello nejlépe. Prostředí uvádí, že většina úloh jsou očekávané tooat alespoň běh transparentně úroveň kompatibility 130, při využití nový dotaz zpracování funkce a novou odhadu kardinality. Který uvedená reálně jsou vždy některé výjimky a provádění správné splatnosti opatrností je toodetermine důležité vyhodnocení, kolik můžete využívat výhod těchto vylepšení. A znovu, můžou být úložiště dotazů hello skvělé pomáhá při provádění této práce!

S rozvojem zpracovaní SQL Azure, můžete očekávat úroveň kompatibility 140 v hello budoucí. Kdy je vhodné čas, spustíme posuzování co se otevře tato úroveň kompatibilitou v budoucnu 140, stejně jako jsme stručně tady popisovaných jakou úroveň kompatibility 130 je přináší dnes.

Teď, můžeme není zapomněli, od června 2016, Azure SQL Database změní úroveň kompatibility výchozí hello ze 120 too130 pro nově vytvořené databáze. Mějte na paměti!

## <a name="references"></a>Odkazy
* [Co je nového v databázový stroj](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blog: Úložiště dotazů: záznam dat pro vaši databázi pomocí Borko Novakovic, 8. června 2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [Úroveň kompatibility databáze ALTER (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [PŘÍKAZ ALTER VYMEZENÁ KONFIGURACE DATABÁZE](https://msdn.microsoft.com/library/mt629158.aspx)
* [Úroveň kompatibility 130 pro Azure SQL Database verze 12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [Optimalizace vaše plány dotazů s hello odhadu kardinality aplikace SQL Server 2014](https://msdn.microsoft.com/library/dn673537.aspx)
* [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx)
* [Blog: Vylepšené dotazu výkonu s úrovní kompatibility 130 služby Azure SQL Database pomocí Alain Lissoir 6 může 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)

<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
