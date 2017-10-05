---
title: "Databáze úroveň kompatibility 130 – Azure SQL Database | Microsoft Docs"
description: "V tomto článku jsme prozkoumejte výhody spuštění vaší databázi SQL Azure s úrovní kompatibility 130 a využívat výhod nový Optimalizátor dotazů a funkce procesoru dotazů. Také jsme adres nežádoucí vliv na výkon dotazů pro existující aplikace SQL."
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
ms.openlocfilehash: c08c0690df4f389416e4ed2e2df2dbb72d6fd567
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Zvýšení výkonu dotazů s kompatibilitou úrovně 130 ve službě Azure SQL Database
Databáze SQL Azure běží transparentně stovky tisíc databází na mnoha různých kompatibility úrovních, zachování a zajištění zpětné kompatibility na odpovídající verzi systému Microsoft SQL Server pro všechny své zákazníky!

V tomto článku jsme prozkoumejte výhody spuštění vaše připojení databáze SQL Azure s úrovní kompatibility 130 a využívat výhod nový Optimalizátor dotazů a funkce procesoru dotazů. Také jsme adres nežádoucí vliv na výkon dotazů pro existující aplikace SQL.

Připomínáme historie zarovnání verze SQL výchozí úrovně kompatibility jsou následující:

* 100: v systému SQL Server 2008 a Azure SQL Database verze 11.
* 110: v systému SQL Server 2012 a Azure SQL Database verze 11.
* 120: v systému SQL Server 2014 a Azure SQL Database verze 12.
* 130: v systému SQL Server 2016 a Azure SQL databáze verze 12.

> [!IMPORTANT]
> Počínaje **mid června 2016**, v databázi SQL Azure, bude výchozí úroveň kompatibility 130 místo 120 pro **nově vytvořený** databáze.
> 
> Databáze vytvořené před mid června 2016 se *není* mít vliv a bude udržovat své aktuální úroveň kompatibility (100, 110 nebo 120). Databáze, které se migrovat z verzí Azure SQL Database verze 11 na verzi 12, bude mít úrovní kompatibility 110 nebo 100. 
> 

## <a name="about-compatibility-level-130"></a>O úroveň kompatibility 130
První Pokud chcete vědět, aktuální úroveň kompatibility databáze, spusťte následující příkaz Transact-SQL.

```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Před provedením této změny na úroveň 130 pro **nově** vytvořené databáze, můžeme zkontrolujte co tato změna se točí kolem prostřednictvím příklady velmi základního dotazu a v tématu Jak každý, kdo může těžit z něj.

Zpracování dotazu v relační databáze může být velmi složité a může vést k mnoha vědy a matematika pochopit rozhodnutích při návrhu vyplývajících a chování. V tomto dokumentu obsah záměrně zjednodušenou zajistěte, aby každý, kdo má minimální technické základní můžete porozumět dopadu Změna úrovně kompatibility a určit, jak se může hodit aplikace.

Umožňuje rychlý podívejte se na úroveň kompatibility 130 přináší v tabulce.  Můžete najít další podrobnosti o [změnit úroveň kompatibility databáze (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ale zde je uveden krátký:

* Operace Insert příkazu Insert vyberte může být Vícevláknová nebo může mít paralelní plán při před jednovláknové tuto operaci.
* Paměť optimalizovaný tabulka a tabulka proměnné dotazy mohou mít nyní paralelní plány při předtím, než tato operace se také jednovláknové.
* Statistiky pro paměťově optimalizované tabulky se dá teď vzorkovat a jsou automaticky aktualizovány. V tématu [co je nového ve službě databázového stroje: OLTP v paměti](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) další podrobnosti.
* V režimu dávky nebo s režimu Row změny s indexy úložiště sloupců
  * Řazení v tabulce s indexem úložiště sloupce jsou nyní v dávkovém režimu.
  * Agregace oddílová nyní pracovat v dávkovém režimu například TSQL funkce LAG nebo realizace příkazy.
  * Dotazy na úložiště sloupce tabulky s více distinct klauzulí pracovat v dávkovém režimu.
  * Dotazy spuštěnými na úrovni DOP = 1 nebo sériového plán také spustit v dávkovém režimu.
* Poslední vylepšení odhadu kardinality ve skutečnosti přicházejí s úrovní kompatibility 120, ale těch, které je spuštěna na nižší úrovni kompatibility (tj. 100 nebo 110), přesunutí kompatibility úroveň 130 se otevře také tato vylepšení a tyto může taky využívat dotazu výkon aplikací.

## <a name="practicing-compatibility-level-130"></a>Cvičení úroveň kompatibility 130
První Pojďme některé tabulky, indexy a náhodná data vytvořená Pokud chcete provádět některé tyto nové funkce. Příklady skriptů TSQL mohou být provedeny v části SQL Server 2016 nebo v databázi SQL Azure. Ale při vytváření databáze Azure SQL, ujistěte se, že zvolíte na minimální databázi P2 vzhledem k tomu, že budete potřebovat alespoň několik jader, který má povolení více vláken a proto těžit z těchto funkcí.

```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

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


Nyní Pojďme Podíváme se na některé z funkcí zpracování dotazu bude s úrovní kompatibility 130.

## <a name="parallel-insert"></a>Paralelní vložení
Provádění níže TSQL příkazy provede operaci vložení v rámci úroveň kompatibility 120 a 130, který v uvedeném pořadí provede operaci vložení v jednom zařazování modelu (120) a ve model Vícevláknová (130).

```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Tím, že požádá skutečnou plán dotazu, prohlížení jeho grafické vyjádření, její obsah XML, můžete určit, které mohutnost odhad funkce je v play. Prohlížení plánech-souběžného na obrázku 1, jsme jasně uvidí, že provádění vložit sloupec úložiště přejde z sériového portu v 120 na paralelní ve 130. Všimněte si také, že změna ikony iterátor ve 130 plán zobrazující dva paralelní šipky skutečnost ilustrující, která teď provádění iterator je skutečně paralelní. Pokud máte velké operace INSERT na dokončení, paralelní provádění, spojen s číslem jádra, které máte k dispozici pro databázi, provede lépe; až 100krát rychlejší podle vaší konkrétní situace!

*Obrázek 1: Operace INSERT se mění z sériové paralelní s kompatibilitou úrovně 130.*

![Obrázek 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)

## <a name="serial-batch-mode"></a>SÉRIOVÉ dávkovém režimu
Podobně přesunutí úroveň kompatibility 130 při zpracování řádky dat umožňuje režimu zpracování dávky. Nejprve dávkových operací režimu jsou dostupná pouze při mají index úložiště sloupců na místě. Druhý dávky obvykle představuje ~ 900 řádky a používá kódu logiku, optimalizované pro vícejádrovými procesoru, vyšší propustnost paměti a přímo využívá komprimovaná data sloupce úložiště, pokud je to možné. Za těchto podmínek SQL serveru 2016 zpracovávat ~ 900 řádků najednou, namísto 1 řádek v době, a v důsledku toho celkové režijní náklady operace nyní sdíleny celý batch, snižuje celkové náklady řádek. Toto množství sdílených operací v kombinaci s kompresí úložiště sloupec v podstatě snižuje latenci zahrnutých v režimu vyberte dávkovou operaci. Můžete najít další podrobnosti o úložišti sloupce a dávky režimu v [Průvodce indexy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx).

```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Jako viditelná níže, pomocí sledování dotaz plány vedle sebe na obrázku 2, vidíme, že došlo ke změně režimu zpracování s úrovní kompatibility, a v důsledku toho při provádění dotazů v obou úroveň kompatibility zcela, vidíme, že většina Doba zpracování je stráví v režimu row (86 %) ve srovnání s dávkovém režimu (14 %), které byly zpracovány 2 dávky. Zvýšit datovou sadu, výhodou zvýší.

*Obrázek 2: Vybrat ze sériové operace změny dávkovém režimu s úrovní kompatibility 130.*

![Obrázek 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)

## <a name="batch-mode-on-sort-execution"></a>Dávkovém režimu na provedení řazení
Podobně jako výše, ale použité na operace řazení, přechod z režimu row (úroveň kompatibility 120) dávkovém režimu (úroveň kompatibility 130) zvyšuje výkon operace řazení ze stejných důvodů.

```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Viditelné-souběžného na obrázku 3, vidíme, že operace řazení v režimu row představuje 81 % nákladů, zatímco dávkovém režimu pouze představuje % 19 náklady (v uvedeném pořadí 81 % a % 56 na řazení samotné).

*Obrázek 3: Operace řazení změny z řádku dávkovém režimu s úrovní kompatibility 130.*

![Obrázek 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)

Samozřejmě tyto ukázky obsahovat pouze desetitisíce řádků, který není nic při vyhledávání v datech dostupných v většina serverů SQL dní. Právě tyto proti miliony řádků místo projektu a tento fakt může projevit v několika minutách provádění ušetřena každý den čekající na povaze vašich úloh.

## <a name="cardinality-estimation-ce-improvements"></a>Vylepšení mohutnost odhad (CE)
Zavedena v systému SQL Server 2014, všechny databáze s úrovní kompatibility 120 nebo vyšší bude využívat nové odhadu kardinality funkce. Mohutnost odhad je v podstatě logikou používanou k určení, jak bude systému SQL server spustit dotaz založený na jeho odhadované náklady. Odhad se počítá pomocí vstup z statistiky přidružené objekty zahrnutých v tomto dotazu. Prakticky, v hlavní funkce odhadu kardinality jsou odhady počet řádek společně s informacemi o distribuci hodnot počty odlišné hodnoty, a duplicitní počty obsažené v tabulky a objekty odkazovaná v dotazu. Získání tyto odhady nesprávný, může způsobit nepotřebné vstupu/výstupu disku z důvodu nedostatku paměti uděluje (tj. v databázi TempDB úniky) nebo výběr sériové plán spuštění přes spouštění paralelní plán, a další. Uzavření, nesprávný odhady může vést k snížení celkového výkonu spuštění dotazu. Na druhé straně lepší odhady přesnější odhady vede k lepší spuštěních dotazu!

Jak je uvedeno nahoře, optimalizace dotazu a odhady jsou komplexní záležitosti, ale pokud chcete další informace o plány dotazů a architektury odhadu kardinality, najdete v dokumentu v [optimalizace vaše plány dotazů mohutností SQL serveru 2014 Odhadu](https://msdn.microsoft.com/library/dn673537.aspx) pro podrobnější prohlídku.

## <a name="which-cardinality-estimation-do-you-currently-use"></a>Které odhadu kardinality právě používáte?
Chcete-li určit, pod které mohutnost odhad své dotazy běží, právě použijeme ukázky dotazů, níže. Všimněte si, že tento první příklad budou spouštěny pod úrovní kompatibility 110, za použití staré mohutnost odhad funkce.

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


Po dokončení provádění klikněte na odkaz XML a podívejte se na vlastnosti první iterator, jak je uvedeno níže. Poznámka: název vlastnosti, která je volána CardinalityEstimationModelVersion nastaveno na 70. Neznamená to, že úroveň kompatibility databáze je nastavena na verzi SQL Server 7.0 (je nastavený na 110 jako viditelné ve výše uvedené příkazy TSQL), ale hodnota 70 jednoduše představuje starší verze funkce mohutnost odhad, která je k dispozici od verze SQL Server 7.0 které měl žádné hlavní revize až SQL Server 2014 (který se dodává s úrovní kompatibility 120).

*Obrázek 4: CardinalityEstimationModelVersion nastavena na hodnotu 70 při použití úrovní kompatibility 110 nebo níže.*

![Obrázek 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)

Alternativně můžete změnit úroveň kompatibility na 130 a zakázat používání nové funkce odhadu kardinality pomocí LEGACY_CARDINALITY_ESTIMATION nastavenou na ON se [ALTER DATABASE OBOR CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx). Bude jím přesně stejný jako pomocí 110 z odhadu kardinality funkce hlediska, při použití nejnovější dotaz zpracování úroveň kompatibility. To uděláte, můžete těžit z nový dotaz zpracování funkce přicházející s nejnovější úroveň kompatibility (tj. v dávkovém režimu), ale stále spoléhají na staré mohutnost odhad funkce v případě potřeby.

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


Přesunutí jednoduše úroveň kompatibility 120 nebo 130 umožňuje nové funkce odhadu kardinality. V takovém případě výchozí CardinalityEstimationModelVersion bude nastavena odpovídajícím způsobem 120 nebo 130 jako zobrazené níže.

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


*Obrázek 5: CardinalityEstimationModelVersion nastavena na 130 při použití 130 úroveň kompatibility.*

![Obrázek 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)

## <a name="witnessing-the-cardinality-estimation-differences"></a>Mohutnost odhad rozdíly při jeho práci
Teď umožňuje spustit trochu složitější dotaz zahrnující vnitřního spojení s klauzulí WHERE se některé predikáty a podíváme se na odhad počtu řádků z původního funkce odhadu kardinality nejdřív.

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


Efektivní provádění tohoto dotazu vrátí 200,704 řádky, zatímco odhad řádek s původním mohutnost odhad funkce deklarací 194,284 řádků. Samozřejmě jak je uvedená před, tyto řádků počet výsledků bude také záviset jak často jste spustili předchozí ukázky, která naplní ukázkové tabulky opakovaně v každé spuštění. Samozřejmě predikáty v dotazu bude také mít vliv na skutečný odhad kromě zajištění dostatečného obrazec tabulky, obsah, dat a jak tato data ve skutečnosti korelaci mezi sebou.

*Obrázek 6: Odhad počtu řádků vypnuté 194,284 nebo 6 000 řádků z 200,704 řádky očekává.*

![Obrázek 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)

Stejným způsobem můžeme teď spustit stejný dotaz s novými funkcemi odhadu kardinality.

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


Prohlížení níže, jsme teď zjistíte, že odhad řádek 202,877, nebo mnohem blíže a vyšší než původní odhadu kardinality.

*Obrázek 7: Odhad počtu řádků je nyní 202,877 místo 194,284.*

![Obrázek 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)

Ve skutečnosti sadu výsledků dotazu se 200,704 řádků (ale všechny závisí, jak často spouštět dotazy předchozí ukázky, ale je důležité, protože TSQL používá příkaz RAND(), skutečné hodnoty, vrátí se můžou lišit od prvního spuštění na další). Proto v tomto konkrétním příkladu nové odhadu kardinality nepodporuje lépe daří v odhadnout počet řádků, protože je mnohem blíže 200,704, než 194,284 202,877! Poslední, pokud změníte klauzule WHERE predikáty k rovnosti (místo ">" pro instanci), může to zkontrolujte odhad mezi starý a můžete získat novou funkci mohutnost i více lišit podle toho, kolik odpovídá.

Samozřejmě v takovém případě se ~ 6000 řádků vypnout z skutečný počet nepředstavuje velké množství dat v některých situacích. Nyní, transpozice na miliony řádků napříč několika tabulek a složitější dotazy a v některých případech odhad může být vypnuto miliony řádky, a proto riziko výdej up plán spuštění nesprávný, nebo pro požadování vedoucí k databázi TempDB uděluje nedostatek paměti úniky a proto více vstupně-výstupních operací, jsou mnohem vyšší.

Pokud máte možnost, praxi toto porovnání s nejčastější dotazy a datové sady a uvidíte sami jak moc některé odhady starý a nový jsou ovlivněni, zatímco některé právě může stát další vypnout z když ve skutečnosti nebo některé jiné stačí jednoduše blíže k počty skutečné řádků ve skutečnosti vrátila v sadách výsledků dotazu. Všechny záviset tvaru své dotazy, vlastnosti databáze Azure SQL, povaze a velikosti vaše datové sady a statistiky o je k dispozici. Pokud jste právě vytvořili instanci databáze SQL Azure, bude mít Optimalizátor dotazů k sestavení své znalosti od začátku namísto opakovaného použití statistiky, které se skládají z předchozích spuštění dotazu. Ano odhad jsou velmi kontextové a téměř specifické pro každé situaci serveru a aplikace. Je důležitým aspektem pamatovat!

## <a name="some-considerations-to-take-into-account"></a>Některé aspekty vzít v úvahu
I když většina úloh by těžit z úrovně kompatibility 130, před přijetím úroveň kompatibility pro produkční prostředí, máte v podstatě 3 možnosti:

1. Přesunout na úroveň kompatibility 130 a v tématu Jak provádět věcí. V případě, že jste si všimli některé regresí, stačí jednoduše nastavit úroveň kompatibility zpět na jeho původní úroveň, nebo se zachovat 130 a pouze zpětné odhadu kardinality zpět do režimu starší verze (jak je popsáno výše, to samostatně může vyřešit problém).
2. Důkladně otestovat existující aplikace podobné produkční zatížení, vyladit a ověření výkonu před přechodem do produkčního prostředí. V případě problémů stejné jako výše, můžete vždy přejděte zpět na původní úroveň kompatibility, nebo jednoduše odhadu kardinality vrátit zpět do režimu starší verze.
3. Jako poslední možnost a nejnovější způsobu, jak vyřešit tyto otázky je využít úložiště dotazů. To je doporučená možnost dnešní! Pomůže analýzy vašich dotazů v rámci kompatibility úrovně 120 nebo pod versus 130, nelze doporučujeme dostatečně používat úložiště dotazů. Úložiště dotazů je k dispozici v nejnovější verzi Azure SQL Database V12 a je určený vám pomůže při řešení potíží s výkonem dotazu. Úložiště dotazů si můžete představit jako záznam dat pro vaši databázi shromažďování a nabízí podrobné historické informace o všech dotazů. To výrazně zjednodušuje forenzních výkon snížením doba diagnostikovat a vyřešit problémy. Další informace najdete [úložiště dotazů: záznam dat pro vaši databázi](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

Na vysoké úrovně, pokud již máte sadu databází s úrovní kompatibility 120 nebo níže a chcete přesunout některé z nich na 130, nebo protože vaše úlohy automaticky zřizovat nové databáze, které budou brzy se ve výchozím nastavení k 130, zvažte prosím Tady:

* Před změnou na novou úroveň kompatibility v produkčním prostředí, povolte úložiště dotazů. Můžete se podívat do [změnit režim kompatibility databáze a používat úložiště dotazů](https://msdn.microsoft.com/library/bb895281.aspx) Další informace.
* V dalším kroku otestujte všechny kritické úlohy pomocí reprezentativní dat a dotazy z prostředí produkčního prostředí a porovnání výkonu zaznamenal a jako hlášené pomocí úložiště dotazů. Pokud dojde k některé regresí, můžete identifikovat regressed dotazy s úložiště dotazů a použít plán vynucení možnost úložiště dotazů (neboli plán Připnutí). V takovém případě můžete platností zůstat s úrovní kompatibility 130 a používat plán bývalé dotazu na návrh úložiště dotazů.
* Pokud chcete využívat nové funkce a možnosti Azure SQL Database (který je spuštěn SQL Server 2016), ale jsou citlivé na změny uvést do režimu podle úrovně kompatibility 130, jako poslední možnost, zvažte vynucení úroveň kompatibility zpět na úroveň který vyhovuje vaše úlohy pomocí příkazu ALTER DATABASE. Ale nejprve, mějte na paměti, úložiště dotazů plánu Připnutí možnost je nejlepší možnost, protože není pomocí 130 je v podstatě zachování na úrovni funkcí starší verze systému SQL Server.
* Pokud máte víceklientské aplikace pokrývání uzlů více databází, bude pravděpodobně nutné aktualizovat zřizování logiku databází zajistit úroveň kompatibility konzistentní napříč všechny databáze; starý a nově zřízeného ty. Výkonu úloh aplikace může být citlivá s ohledem na skutečnost, že některé databáze, které jsou spuštěné v různých kompatibility úrovně a proto může vyžadovat kompatibility úrovně konzistence napříč všechny databáze chcete-li poskytovat stejné prostředí k vašim zákazníkům všechny napříč panelu. Všimněte si, že se nejedná o pověření, ve skutečnosti závisí na tom, jak vaše aplikace je ovlivňován úroveň kompatibility.
* Poslední, pokud jde o odhad mohutnost a stejně jako změna úrovně kompatibility, než budete pokračovat v produkčním prostředí, se doporučuje k testování vaše produkční úlohy v rámci nové podmínky k určení, pokud vaše aplikace výhody z Vylepšení odhadu kardinality.

## <a name="conclusion"></a>Závěr
Používání Azure SQL Database, abyste mohli využívat výhod všechny vylepšení SQL Server 2016 jasně zlepšit spuštěních vašeho dotazu. Stejně jako-je! Samozřejmě podobně jako u jakékoli nové funkce, je třeba provést řádné vyhodnocení určit přesnou podmínky, za kterých vaše databáze zatížení funguje nejvhodnější. Prostředí ukazuje, že většina úloh se očekává alespoň běh transparentně úroveň kompatibility 130, při využití nový dotaz zpracování funkce a novou odhadu kardinality. Který uvedená reálně jsou vždy některé výjimky a provádění správné splatnosti opatrností je důležité určit, kolik můžete využívat výhod těchto vylepšení posouzení. A znovu, můžou být úložiště dotazů skvělé pomáhá při provádění této práce!

S rozvojem zpracovaní SQL Azure, můžete očekávat, že úroveň kompatibility 140 v budoucnu. Kdy je vhodné čas, spustíme posuzování co se otevře tato úroveň kompatibilitou v budoucnu 140, stejně jako jsme stručně tady popisovaných jakou úroveň kompatibility 130 je přináší dnes.

Teď, můžeme není zapomněli, od června 2016, Azure SQL Database se změní výchozí úroveň kompatibility z 120 na 130 pro nově vytvořené databáze. Mějte na paměti!

## <a name="references"></a>Odkazy
* [Co je nového v databázový stroj](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)
* [Blog: Úložiště dotazů: záznam dat pro vaši databázi pomocí Borko Novakovic, 8. června 2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)
* [Úroveň kompatibility databáze ALTER (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)
* [PŘÍKAZ ALTER VYMEZENÁ KONFIGURACE DATABÁZE](https://msdn.microsoft.com/library/mt629158.aspx)
* [Úroveň kompatibility 130 pro Azure SQL Database verze 12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)
* [Optimalizace dotazu plány s SQL serverem 2014 architektury odhadu kardinality](https://msdn.microsoft.com/library/dn673537.aspx)
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
