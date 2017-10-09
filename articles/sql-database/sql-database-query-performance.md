---
title: "Přehled o výkonu aaaQuery pro Azure SQL Database | Microsoft Docs"
description: "Monitorování výkonu dotazu identifikuje hello většina využívání procesoru dotazy pro Azure SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 01cca26f85193c679365585cd676449c9db00e1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-query-performance-insight"></a>Informace o výkonu dotazů databáze Azure SQL
Správa a ladění výkonu hello relačních databází je náročné úlohu, která vyžaduje značné znalosti a investice čas. Informace o výkonu dotazů můžete toospend méně času řešení potíží s výkonem databáze tím, že poskytuje hello následující:

* Podrobnější přehled o vaší spotřeby prostředků (DTU) databází. 
* Hello nejčastějších dotazů podle počtu procesorů a doba trvání nebo provádění, který lze ladit potenciálně pro zlepšení výkonu.
* Dobrý den možnost toodrill dolů do hello podrobnosti o dotazu, zobrazit historii využití prostředků a text. 
* Poznámky, které se zobrazí akce prováděné při ladění výkonu [Advisor databáze SQL Azure](sql-database-advisor.md)  



## <a name="prerequisites"></a>Požadavky
* Query Performance Insight vyžaduje, aby [úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx) je aktivní databáze. Pokud není spuštěné úložiště dotazů, portál hello vás vyzve k tooturn ho na.

## <a name="permissions"></a>Oprávnění
Následující Hello [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) oprávnění jsou požadované toouse Query Performance Insight: 

* **Čtečka**, **vlastníka**, **Přispěvatel**, **Přispěvatel databází SQL**, nebo **Přispěvatel serveru SQL** jsou oprávnění vyžaduje tooview hello nejvyšší na prostředky dotazy a grafy. 
* **Vlastník**, **Přispěvatel**, **Přispěvatel databází SQL**, nebo **SQL serveru Přispěvatel** oprávnění jsou požadované tooview text dotazu.

## <a name="using-query-performance-insight"></a>Pomocí Query Performance Insight
Informace o výkonu dotazů je snadno toouse:

* Otevřete [portál Azure](https://portal.azure.com/) a najít databáze, které chcete tooexamine. 
  * Z nabídky na levé straně, v rámci podpory a řešení potíží vyberte "Query Performance Insight".
* Na první kartě hello projděte si hello seznam nejčastějších dotazů využívání prostředků.
* Vyberte konkrétní dotaz tooview její podrobnosti.
* Otevřete [Poradce pro funkci SQL Azure Database](sql-database-advisor.md) a zkontrolujte, zda jsou k dispozici žádná doporučení.
* Pomocí posuvníků nebo zvětšení ikony toochange zjištěnými intervalu.
  
    ![řídicí panel výkonu](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Několik hodin dat. musí toobe zachycenou úložiště dotazů pro databázi SQL tooprovide query performance Insight. Pokud nemá žádnou aktivitu hello databáze nebo úložiště dotazů nebyl aktivní během určitého časového období, grafy hello bude prázdný, při zobrazení toto časové období. Pokud není spuštěna, může kdykoli povolit úložiště dotazů.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Zkontrolujte nejvíce využívají dotazy procesor
V hello [portál](http://portal.azure.com) hello následující:

1. Procházet tooa SQL database a klikněte na tlačítko **všechna nastavení** > **podporu + Poradce při potížích s** > **dotaz na informace o výkonu**. 
   
    ![Query Performance Insight][1]
   
    Otevře zobrazení Hello nejčastějších dotazů a jsou uvedeny hello nejvyšší procesoru náročné dotazy.
2. Klikněte na kolem grafu hello podrobnosti.<br>Hello horní řádek zobrazuje celkový počet jednotek DTU % pro databázi hello, zatímco hello řádky zobrazit využití procesoru % spotřebovávají hello vybrané dotazy během intervalu vybrané hello (například pokud **za minulý týden** je vybrána každý řádek představuje jeden den).
   
    ![nejčastějších dotazů][2]
   
    Hello dolní mřížky představuje souhrnné informace pro dotazy viditelné hello.
   
   * ID dotazu – jedinečný identifikátor dotazu v databázi.
   * Procesor na dotaz během pozorovatelné intervalu (záleží na agregační funkce).
   * Doba trvání za dotazu (záleží na agregační funkce).
   * Celkový počet spuštěních pro konkrétní dotaz.
     
     Vyberte nebo zrušte tooinclude jednotlivé dotazy nebo vyloučit z hello grafu pomocí zaškrtávacích políček.
3. Pokud vaše data zastaralá, klikněte na tlačítko hello **aktualizovat** tlačítko.
4. Pomocí posuvníků a přiblížení tlačítka toochange pozorovacím intervalu a prozkoumat špičky: ![nastavení](./media/sql-database-query-performance/zoom.png)
5. Případně, pokud chcete jiné zobrazení, můžete vybrat **vlastní** kartě a nastavte:
   
   * Metrika (procesor, doba trvání, počet provedení)
   * Časový interval (poslední týden minulý měsíc posledních 24 hodin). 
   * Počet dotazů.
   * Agregační funkce.
     
     ![settings](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Zobrazení podrobností o jednotlivých dotazů
Podrobnosti o tooview dotazu:

1. Klikněte na jakýkoli dotaz ve hello seznam nejčastějších dotazů.
   
    ![Podrobnosti](./media/sql-database-query-performance/details.png)
2. Otevře zobrazení podrobností Hello a počet energie a doba trvání nebo provádění procesorů dotazy hello je rozdělena v čase.
3. Klikněte na kolem grafu hello podrobnosti.
   
   * Horní graf znázorňuje řádek s celkovou % DTU databáze a hello řádky jsou využívány službou hello vybraný dotaz % využití procesoru.
   * Druhý graf zobrazuje celkovou dobu trvání podle hello vybraný dotaz.
   * Dolní graf zobrazuje celkový počet spuštěních podle hello vybraný dotaz.
     
     ![Podrobnosti o dotazu][3]
4. V případě potřeby pomocí posuvníků, přiblíží tlačítka nebo klikněte na tlačítko **nastavení** toocustomize zobrazení dotaz na data nebo toopick na jiné časové období.

## <a name="review-top-queries-per-duration"></a>Zkontrolujte nejčastějších dotazů za doba trvání
V poslední aktualizaci hello sady Query Performance Insight, zavedli jsme dvě nové metriky, které pomáhají identifikovat potenciální problémová místa: doba trvání a provádění count.<br>

Dlouho běžící dotazy mají hello největší případným zamykání prostředky delší blokování jiných uživatelů a omezení škálovatelnosti. Jsou i hello nejlepší kandidáty pro optimalizaci.<br>

tooidentify dlouho běžící dotazy:

1. Otevřete **vlastní** kartě v Query Performance Insight pro vybrané databáze
2. Změnit metriky toobe **doba trvání**
3. Vyberte počet dotazů a pozorovacím intervalu.
4. Vyberte funkci agregace
   
   * **Součet** sečte všechny doba provádění dotazu během celého pozorovacím intervalu.
   * **Maximální počet** vyhledá dotazy byl v celé pozorovacím intervalu maximální dobu spuštění.
   * **Průměr** najde Průměrná doba provádění všech dotazování spuštěních a ukazují hello horní mimo tyto průměry. 
     
     ![Doba trvání dotazu][4]

## <a name="review-top-queries-per-execution-count"></a>Zkontrolujte nejčastějších dotazů na počet provedení
Vysoký počet spuštěních nemusí ovlivňovat samotná databáze a může být využití prostředků nízké, ale může získat celkový aplikace pomalé.

V některých případech může vést provádění velmi vysoký počet tooincrease síťových přenosů. Odezev výrazně ovlivnit výkon. Jsou to subjektu toonetwork latenci a dobu toodownstream serveru. 

Například mnoho řízené daty weby výraznou přístup hello databázi pro každý požadavek uživatele. Při připojení sdružování pomůže, hello více síťových přenosů a zatížení na serveru databáze hello může nepříznivě ovlivnit výkon.  Obecné Rady, jak je tookeep zaokrouhlí absolutní minimální tooan služebních cest.

tooidentify provést nejčastější dotazy ("chatty") dotazy:

1. Otevřete **vlastní** kartě v Query Performance Insight pro vybrané databáze
2. Změnit metriky toobe **počet provedení**
3. Vyberte počet dotazů a pozorovacím intervalu.
   
    ![počet provedení dotazu][5]

## <a name="understanding-performance-tuning-annotations"></a>Principy poznámky vyladění výkonu
Při prohlížení zatížení Query Performance Insight, můžete si všimnout ikony s svislá čára nad hello grafu.<br>

Tyto ikony jsou poznámky; představují výkonu by to ovlivnilo provedené akce [Poradce pro funkci SQL Azure Database](sql-database-advisor.md). Ve výběru ukázáním poznámky získáte základní informace o hello akce:

![dotaz – Poznámka][6]

Chcete-li další tooknow nebo použít doporučení služby advisor, klikněte na ikonu hello. Otevře se podrobnosti akce. Pokud je aktivní doporučení můžete provést okamžitou pomocí příkazu.

![Podrobnosti o dotazu – Poznámka][7]

### <a name="multiple-annotations"></a>Více poznámek.
Je možné, že kvůli úroveň přiblížení, bude získat poznámky, které jsou zavřít tooeach jiných sbalené do jednoho. To bude reprezentovat speciální ikonou, kliknutím ji bude otevřete nové okno, kde seznam seskupené poznámky se zobrazí.
Korelace dotazy a ladění akce výkonu může pomoci toobetter pochopit vaše úlohy. 

## <a name="optimizing-hello-query-store-configuration-for-query-performance-insight"></a>Optimalizace hello konfigurace úložiště dotazů pro Query Performance Insight
Během používání Query Performance Insight může dojít k hello následující úložiště dotazů zprávy:

* "Úložiště dotazů není správně nakonfigurována v této databázi. Kliknutím sem toolearn Další."
* "Úložiště dotazů není správně nakonfigurována v této databázi. Kliknutím sem nastavení toochange." 

Tyto zprávy se obvykle zobrazují, když úložiště dotazů není možné toocollect nová data. 

Prvním případě se stane, když úložiště dotazů je ve stavu jen pro čtení a parametry nejsou nastavené optimálně. Můžete opravit zvýšit velikost úložiště dotazů nebo vymazání úložiště dotazů.

![tlačítko qds][8]

Druhém případě se stane, když úložiště dotazů je vypnutý nebo parametry nejsou nastavené optimálně. <br>Hello uchovávání a zachycení zásady a povolit úložiště dotazů, můžete změnit tak, že spustíte následující příkazy nebo přímo z portálu:

![tlačítko qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Doporučené zásady uchovávání informací a zachycení
Existují dva typy zásad uchovávání informací:

* Na základě velikosti – když je dosaženo tooAUTO sadu vyčistí dat automaticky při skoro maximální velikost.
* Čas na základě – ve výchozím nastavení jsme bude nastavena too30 dní, což znamená, pokud úložiště dotazů se dostatek místa, odstraní se dotazovat informací, které jsou starší než 30 dní

Zaznamenat zásad může být nastaven na:

* **Všechny** -zaznamená všechny dotazy.
* **Automatické** -málo časté dotazy a dotazy s zanedbatelný kompilace a provádění trvání jsou ignorovány. Interně jsou určit prahové hodnoty pro počet, kompilace a runtime dobu provádění. Toto je výchozí možnost hello.
* **Žádný** -úložiště dotazů zastaví sběr nové dotazy, ale jsou stále shromážděných statistik modulu runtime pro již zaznamenané dotazy.

Doporučujeme nastavení všechny zásady tooAUTO a vyčištění zásad too30 dnů:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Zvětšete velikost úložiště dotazů. To může mít provádět při připojování tooa databáze a vystavují následující dotaz:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Použití těchto nastavení budou nakonec úložiště dotazů shromažďování nové dotazy, ale pokud nechcete, aby toowait můžete vymazat úložiště dotazů. 

> [!NOTE]
> Provádění následující dotaz odstraní všechny informace aktuální v hello úložiště dotazů. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Souhrn
Informace o výkonu dotazů vám pomůže pochopit hello dopad dotazu úlohy a jak se týká toodatabase spotřeby prostředků. Pomocí této funkce se další informace o hello nejnáročnější dotazy a snadno identifikovat hello ty, které jsou toofix dřív, než narostou problém.

## <a name="next-steps"></a>Další kroky
Další doporučení týkající se vylepšení výkonu hello vaší databáze SQL, klikněte na tlačítko [doporučení](sql-database-advisor.md) na hello **Query Performance Insight** okno.

![Poradce pro výkon](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

