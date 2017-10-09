---
title: "kódy chyb aaaSQL - Chyba připojení databáze | Microsoft Docs"
description: "Další informace o kódy chyb SQL pro klientské aplikace SQL Database, například běžných chyb připojení databáze, problémů kopie databáze a obecné chyby. "
keywords: "Kód chyby SQL, sql přístup, Chyba připojení databáze, kódy chyb sql"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 2a23e4ca-ea93-4990-855a-1f9f05548202
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 683a7d72c935742f3f9f9a0c683e5d8161167d6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Kódy chyb SQL pro klientské aplikace SQL Database: databáze došlo k chybě připojení a další problémy
<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


V tomto článku jsou uvedeny kódy chyb SQL pro klientské aplikace SQL Database, včetně chyb připojení databáze, přechodné chyby (také nazývané přechodných), prostředků zásad správného řízení chyb, problémů kopie databáze, elastický fond a dalších chyb. Většina kategorie jsou konkrétní tooAzure SQL Database a nevztahují tooMicrosoft systému SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>K chybám připojení databáze, přechodné chyby a dalších dočasné chyb
Hello následující tabulka popisuje hello kódy chyb SQL pro chyby ztrátě připojení a jiné přechodné chyby, které se můžete setkat, když se aplikace pokusí tooaccess databáze SQL. Pro získání Začínáme kurzy o tooconnect tooAzure SQL Database, najdete v části [připojení tooAzure SQL Database](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Většina běžných chyb připojení databáze a přechodná chyba chyb
Hello infrastrukturu Azure má toodynamically hello možnost změnit konfiguraci serverů při náročných úloh jsou vyvolány v hello služba SQL Database.  Toto chování dynamické může způsobit váš klientský program toolose jeho tooSQL připojení databáze. Tento druh chybový stav nazývá *přechodná chyba*.

Důrazně doporučujeme, aby váš klientský program má logika opakovaných pokusů, takže ji může znovu vytvořit připojení po poskytnutí hello přechodná chyba čas toocorrect sám sebe.  Doporučujeme, abyste zpoždění pro 5 sekund před první opakování. Opakování po prodlevě kratší než 5 sekund rizika čtenáře hello cloudové služby. Pro každý další pokusy hello zpoždění by měl růst exponenciálnímu, až tooa maximálně 60 sekund.

Chyby přechodná chyba se obvykle manifest jako jeden z následujících chybových zpráv z klientských programů hello:

* Databáze &lt;%{db_name/&gt; na serveru &lt;Azure_instance&gt; není aktuálně k dispozici. Zkuste to prosím znovu později hello připojení. Pokud hello potíže potrvají, obraťte se na zákaznickou podporu a poskytněte ID trasování relace hello &lt;session_id&gt;
* Databáze &lt;%{db_name/&gt; na serveru &lt;Azure_instance&gt; není aktuálně k dispozici. Zkuste to prosím znovu později hello připojení. Pokud hello potíže potrvají, obraťte se na zákaznickou podporu a poskytněte ID trasování relace hello &lt;session_id&gt;. (Microsoft SQL Server, chyba: 40613)
* Existující připojení se vynuceně ukončeno vzdáleným hostitelem hello.
* System.Data.Entity.Core.EntityCommandExecutionException: Došlo k chybě při provádění definice příkazu hello. Viz vnitřní výjimka hello podrobnosti. ---> System.Data.SqlClient.SqlException: úrovni přenosu došlo k chybě při příjmu výsledků ze serveru hello. (Zprostředkovatel: zprostředkovatele relací, chyba: 19 – fyzické připojení není použitelná)
* Tooa pokus o připojení pro sekundární databázi se nezdařila, protože hello databáze je v hello proces reconfguration a je zaneprázdněný, použití nové stránky při uprostřed hello aktivní transakce na hello primární databáze. 

Příklady kódu logika opakovaných pokusů najdete v části:

* [Knihovny připojení pro SQL Database a SQL Server](sql-database-libraries.md) 
* [Chyby připojení toofix akce a přechodné chyby v databázi SQL](sql-database-connectivity-issues.md)

Diskuzi o hello *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL sdružování připojení serveru (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Kódy chyb přechodná chyba
Hello tyto chyby jsou přechodný a v logiku aplikace je třeba opakovat 

| Kód chyby | Závažnost | Popis |
| ---:| ---:|:--- |
| 4060 |16 |Databázi nelze otevřít "%. & #x2a; ls" požadovaném přihlášením hello. Hello přihlášení se nezdařilo. |
| 40197 |17 |Hello služby došlo k chybě při zpracování vaší žádosti. Zkuste to prosím znovu. Kód chyby: %d.<br/><br/>Zobrazí se tato chyba, a po hello služba není spuštěna z důvodu toosoftware nebo upgradem hardwaru, selhání hardwaru nebo jiných problémů převzetí služeb při selhání. Kód chyby Hello (%d) vloženým do uvítací zprávu chyby 40197 poskytuje další informace o druh hello selhání nebo převzetí služeb při selhání došlo k chybě. Některé příklady hello chyba, kterou kódy jsou vnořené v rámci uvítací zprávu chyby 40197 jsou 40020, 40143, 40166 a 40540.<br/><br/>Opětovně se připojujícího tooyour databáze SQL server se automaticky připojí tooa pořádku kopii databáze. Aplikace musí catch 40197, protokolu hello vložených chyba Kód chyby (%d) v rámci uvítací zprávu pro řešení potíží s a zkuste se znovu připojit tooSQL databázi, dokud hello prostředky jsou k dispozici a je znovu navázat připojení. |
| 40501 |20 |Hello služba je aktuálně zaneprázdněna. Opakujte žádost hello po 10 sekundách. ID incidentu: %ls. Kód: %d.<br/><br/>*Poznámka:* Další informace najdete v tématu:<br/>• [Limitů prostředků azure SQL Database](sql-database-resource-limits.md). |
| 40613 |17 |Databáze '%. & #x2a; ls' na serveru '%. & #x2a; ls' není aktuálně k dispozici. Zkuste to prosím znovu později hello připojení. Pokud hello potíže potrvají, obraťte se na zákaznickou podporu a poskytněte ID trasování relace hello '%. & #x2a; ls'. |
| 49918 |16 |Žádost nelze zpracovat. Žádost o pro tooprocess není dostatek prostředků.<br/><br/>Hello služba je aktuálně zaneprázdněna. Hello požadavek opakujte akci později. |
| 49919 |16 |Proces nelze vytvořit nebo aktualizovat požadavku. Příliš mnoho vytvořit nebo aktualizovat probíhající operace předplatného "% ld".<br/><br/>Služba Hello je zaneprázdněna zpracování více vytvořit nebo aktualizovat požadavky pro vaše předplatné nebo server. Požadavky jsou aktuálně blokovány pro optimalizaci prostředků. Dotaz [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) pro čekajících operací. Počkejte, dokud čeká na vytvoření nebo aktualizace žádosti o dokončení nebo odstraňte jednu z vaší žádosti čekající na vyřízení a opakujte žádost později. |
| 49920 |16 |Žádost nelze zpracovat. Příliš mnoho operací v průběhu pro předplatné "% ld".<br/><br/>Hello služba je zaneprázdněná zpracování více požadavků pro tento odběr. Požadavky jsou aktuálně blokovány pro optimalizaci prostředků. Dotaz [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) pro stav operace. Počkejte, dokud nevyřízené žádosti o dokončení nebo odstraňte jednu z vaší žádosti čekající na vyřízení a opakujte žádost později. |
| 4221 |16 |Tooread sekundární přihlášení se nezdařilo z důvodu toolong čekání na 'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING'. Hello replika není k dispozici pro přihlášení, protože chybí řádek verze pro transakce, které byly během letu, pokud byla replika hello recykluje. Hello problém lze vyřešit vrácení zpět nebo potvrzení hello aktivních transakcí na primární replice hello. Výskyty tuto podmínku můžete minimalizovat zabraňující dlouho zápisu transakce na primární hello. |

## <a name="database-copy-errors"></a>Chyby kopie databáze
Hello následující chyby můžete došlo při kopírování databáze Azure SQL Database. Další informace najdete v tématu [Kopírování databáze služby Azure SQL Database](sql-database-copy.md).

| Kód chyby | Závažnost | Popis |
| ---:| ---:|:--- |
| 40635 |16 |Klient s IP adresou '%. & #x2a; ls' je dočasně zakázána. |
| 40637 |16 |Vytvoření kopie databáze je aktuálně zakázáno. |
| 40561 |16 |Kopírování databáze se nezdařilo. Buď hello zdrojová nebo cílová databáze neexistuje. |
| 40562 |16 |Kopírování databáze se nezdařilo. Hello zdrojová databáze byla vyřazena. |
| 40563 |16 |Kopírování databáze se nezdařilo. Hello cílová databáze byla vyřazena. |
| 40564 |16 |Kopírování databáze se nezdařilo z důvodu vnitřní chyby tooan. Vyřaďte cílovou databázi a akci opakujte. |
| 40565 |16 |Kopírování databáze se nezdařilo. Kopie databáze více než 1 souběžných z hello, které může stejný zdroj. Vyřaďte cílovou databázi a zkuste to znovu později. |
| 40566 |16 |Kopírování databáze se nezdařilo z důvodu vnitřní chyby tooan. Vyřaďte cílovou databázi a akci opakujte. |
| 40567 |16 |Kopírování databáze se nezdařilo z důvodu vnitřní chyby tooan. Vyřaďte cílovou databázi a akci opakujte. |
| 40568 |16 |Kopírování databáze se nezdařilo. Zdrojová databáze již není dostupná. Vyřaďte cílovou databázi a akci opakujte. |
| 40569 |16 |Kopírování databáze se nezdařilo. Cílová databáze již není dostupná. Vyřaďte cílovou databázi a akci opakujte. |
| 40570 |16 |Kopírování databáze se nezdařilo z důvodu vnitřní chyby tooan. Vyřaďte cílovou databázi a zkuste to znovu později. |
| 40571 |16 |Kopírování databáze se nezdařilo z důvodu vnitřní chyby tooan. Vyřaďte cílovou databázi a zkuste to znovu později. |

## <a name="resource-governance-errors"></a>Chyby zásad správného řízení zdrojů
Hello následujícím chybám jsou způsobeny nadměrnému využití prostředků při práci s Azure SQL Database. Například:

* Transakce byla otevřena příliš dlouho.
* Transakce je příliš mnoho zámky.
* Aplikace využívala příliš mnoho paměti.
* Aplikace využívala příliš mnoho `TempDb` místa.

Související témata:

* Podrobnější informace jsou k dispozici zde: [limitů prostředků Azure SQL Database](sql-database-resource-limits.md)

| Kód chyby | Závažnost | Popis |
| ---:| ---:|:--- |
| 10928 |20 |ID zdroje: %d. limit %s Hello hello databáze je %d a byl dosažen. Další informace najdete v tématu [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Hello ID prostředku označuje hello prostředek, který byl dosažen hello limit. Pracovní vlákna text hello ID prostředku = 1. Pro relace, hello ID prostředku = 2.<br/><br/>*Poznámka:* Další informace o této chybě a jak tooresolve, najdete v části:<br/>• [Limitů prostředků azure SQL Database](sql-database-resource-limits.md). |
| 10929 |20 |ID zdroje: %d. minimální záruka Hello %s je %d, maximální limit je %d a hello aktuálního využití pro databázi hello je %d. Hello server je však aktuálně zaneprázdněn toosupport požadavky větší než %d pro tuto databázi. Další informace najdete v tématu [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Jinak zkuste to prosím znovu později.<br/><br/>Hello ID prostředku označuje hello prostředek, který byl dosažen hello limit. Pracovní vlákna text hello ID prostředku = 1. Pro relace, hello ID prostředku = 2.<br/><br/>*Poznámka:* Další informace o této chybě a jak tooresolve, najdete v části:<br/>• [Limitů prostředků azure SQL Database](sql-database-resource-limits.md). |
| 40544 |20 |Hello databáze dosáhla své kvóty velikosti. Data oddílů nebo je odstraňte, případně odstraňte indexy dokumentaci hello možná řešení. |
| 40549 |16 |Relace je ukončena, protože máte dlouhotrvající transakci. Zkuste transakci zkrátit. |
| 40550 |16 |Hello relace byla ukončena, protože získala příliš mnoho zámků. Zkuste čtení nebo upravit menší počet řádků v rámci jedné transakce. |
| 40551 |16 |Hello relace byla ukončena z důvodu nadměrného `TEMPDB` využití. Zkuste upravit vaše využití místa na dotaz tooreduce hello dočasné tabulky.<br/><br/>*Tip:* Pokud používáte dočasné objekty, šetří místo v hello `TEMPDB` databáze umístěním dočasné objekty po hello relace již nepotřebujete. |
| 40552 |16 |Hello relace byla ukončena z důvodu nadměrného transakce využití místa protokolu. Zkuste upravit menší počet řádků v rámci jedné transakce.<br/><br/>*Tip:* Pokud provádíte Hromadná vložení pomocí hello `bcp.exe` nástroj nebo hello `System.Data.SqlClient.SqlBulkCopy` třídy, zkuste použít hello `-b batchsize` nebo `BatchSize` možnosti toolimit hello počet řádků zkopírovali toohello serveru v každou transakci. Pokud jsou nové sestavení indexu s hello `ALTER INDEX` příkaz, zkuste použít hello `REBUILD WITH ONLINE = ON` možnost. |
| 40553 |16 |Hello relace byla ukončena z důvodu nadměrného využití paměti. Zkuste upravit vašeho dotazu tooprocess méně řádků.<br/><br/>*Tip:* snížení počtu hello `ORDER BY` a `GROUP BY` operace v kódu jazyka Transact-SQL snižuje požadavky na paměť hello tohoto dotazu. |

## <a name="elastic-pool-errors"></a>Chyby elastického fondu
Hello následujícím chybám jsou související toocreating a pomocí Elastics fondy.

| Argument číslo chyby | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
|:--- |:--- |:--- |:--- |:--- |:--- |
| 1132 |EX_RESOURCE |elastický fond Hello bylo dosaženo limitu úložiště. Hello využití úložiště pro elastický fond hello nesmí překročit (%d) MB. |Omezení prostoru na elastického fondu v MB. |Pokus toowrite data tooa databáze, pokud bylo dosaženo limitu úložiště hello hello elastického fondu. |Vezměte v úvahu rostoucí Dtu hello hello elastického fondu pokud je to možné v pořadí tooincrease limitu úložiště, snížit hello úložiště používané jednotlivých databází v rámci hello elastického fondu nebo odebrat databáze z hello elastického fondu. |
| 10929 |EX_USER |minimální záruka Hello %s je %d, maximální limit je %d a hello aktuálního využití pro databázi hello je %d. Hello server je však aktuálně zaneprázdněn toosupport požadavky větší než %d pro tuto databázi. V tématu [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) o pomoc. Jinak zkuste to prosím znovu později. |Minimální počet jednotek DTU na databázi; Maximální počet jednotek DTU na databázi |Celkový počet souběžných pracovních procesů (počet požadavků) Hello mezi všechny databáze v limit fondu hello elastického fondu pokus o tooexceed hello. |Zvažte zvýšení hello Dtu hello elastického fondu pokud je to možné v pořadí tooincrease limitu pracovního procesu, nebo odebrat databáze z hello elastického fondu. |
| 40844 |EX_USER |Databáze: %ls"na serveru '%ls' je databáze edice"%ls"v elastickém fondu a nemůže mít relaci průběžná kopie. |Název databáze, edice databáze, název serveru |Příkaz StartDatabaseCopy je vydán pro jiný premium databáze v elastickém fondu. |Již brzy |
| 40857 |EX_USER |Elastický fond pro server nebyl nalezen: '%ls', název elastického fondu: '%ls'. |název serveru. Název elastického fondu |Zadaný elastického fondu v hello zadaný server neexistuje. |Zadejte název platné elastického fondu. |
| 40858 |EX_USER |Elastický fond '%ls' již existuje v serveru: '%ls' |Název elastického fondu, název serveru |Zadaný fond elastické již existuje v zadané logické server hello. |Zadejte nový název elastického fondu. |
| 40859 |EX_USER |Elastický fond nepodporuje úroveň služby "%ls". |úrovně služby elastického fondu |Zadaná služba úroveň se nepodporuje pro zřizování elastického fondu. |Zadejte správné edice hello nebo nechte vrstvy služby vrstvě prázdné toouse hello výchozí služby. |
| 40860 |EX_USER |Kombinace elastického fondu. %ls"a služby cíl '%ls' je neplatný. |Název elastického fondu; Název cílové úrovně služby |Elastický fond a služby cíle lze zadat společně pouze v případě, že je zadaný cíl služby jako 'ElasticPool'. |Zadejte prosím správnou kombinaci elastický fond a cíl služby. |
| 40861 |EX_USER |edice databáze Hello ' %. *ls se nemůže lišit od úrovně služby elastického fondu hello, což je ' %.* ls'. |edice databáze, úrovně služby elastického fondu |edice databáze Hello se liší od úrovně služby elastického fondu hello. |Nezadávejte prosím edici databáze, které se liší od úrovně služby elastického fondu hello.  Všimněte si, že edice databáze hello nemusí toobe zadaný. |
| 40862 |EX_USER |Název elastického fondu musí být zadán, pokud je zadaný cíl služby elastického fondu hello. |Žádný |Cíl služby elastického fondu nepředstavuje jedinečnou identifikaci fondu elastické databáze. |Zadejte název elastického fondu hello, pokud používáte hello cíl služby elastického fondu. |
| 40864 |EX_USER |Hello Dtu pro elastický fond hello musí být minimálně (%d) Dtu, pro úroveň služby "%. * ls'. |Jednotky Dtu pro elastický fond; úrovně služby elastického fondu. |Pokus tooset hello Dtu pro elastický fond hello nižší než minimální limit hello. |Zkuste to prosím znovu nastavení hello Dtu pro elastický fond tooat hello alespoň minimální limit hello. |
| 40865 |EX_USER |Hello Dtu pro elastický fond hello nesmí být delší než (%d) Dtu, pro úroveň služby "%. * ls'. |Jednotky Dtu pro elastický fond; úrovně služby elastického fondu. |Pokus tooset hello Dtu pro elastický fond hello nad maximální limit hello. |Zkuste to prosím znovu, nastavení hello Dtu pro elastický fond toono hello větší než maximální limit hello. |
| 40867 |EX_USER |Hello na databázi maximální počet jednotek DTU, musí být na nejnižší (%d) pro úroveň služby "%. * ls'. |Maximální počet jednotek DTU na databázi; úrovně služby elastického fondu |Limit pokusu o tooset hello na databáze dole hello maximální počet jednotek DTU podporována. |Zvažte použití hello úrovně služby elastického fondu, který podporuje hello požadovaného nastavení. |
| 40868 |EX_USER |Hello na databázi maximální počet jednotek DTU nesmí překročit (%d) pro úroveň služby "%. * ls'. |Maximální počet jednotek DTU na databázi; úrovně služby elastického fondu. |Limit pokusu o tooset hello na databázi nad rámec hello maximální počet jednotek DTU podporována. |Zvažte použití hello úrovně služby elastického fondu, který podporuje hello požadovaného nastavení. |
| 40870 |EX_USER |Hello minimální počet jednotek DTU na databázi nesmí překročit (%d) pro úroveň služby "%. * ls'. |Minimální počet jednotek DTU na databázi; úrovně služby elastického fondu. |Limit pokusu o tooset hello minimální počet jednotek DTU na databázi nad rámec hello podporována. |Zvažte použití hello úrovně služby elastického fondu, který podporuje hello požadovaného nastavení. |
| 40873 |EX_USER |Hello počet databází (%d) a minimální počet jednotek DTU na databázi (%d) nesmí překročit hello Dtu elastického fondu hello (%d). |Počet databází v elastickém fondu; Minimální počet jednotek DTU na databázi; Počet jednotek Dtu elastického fondu. |Probíhá pokus o minimální počet jednotek DTU toospecify u databází v hello elastického fondu, který je delší než hello Dtu elastického fondu hello. |Zvažte zvýšení hello Dtu elastického fondu hello, nebo snížit hello minimální počet jednotek DTU na databázi nebo snížit hello počet databází v elastickém fondu hello. |
| 40877 |EX_USER |Fondu elastické databáze nelze odstranit, pokud neobsahuje žádné databáze. |Žádný |elastický fond Hello obsahuje jednu nebo více databází a proto nelze odstranit. |Prosím odebrat databáze z hello elastického fondu v pořadí toodelete ho. |
| 40881 |EX_USER |Hello elastického fondu. %. * ls' bylo dosaženo limitu počtu databáze.  limit počtu Hello databáze pro elastický fond hello nesmí překročit (%d) pro elastický fond se (%d) Dtu. |Název elastického fondu; limit počtu databázi elastického fondu; e Dtu fondu zdrojů. |Probíhá pokus toocreate nebo přidejte databáze tooelastic fondu, pokud bylo dosaženo limitu pro počet databáze hello hello elastického fondu. |Zvažte zvýšení hello Dtu hello elastického fondu pokud je to možné v pořadí tooincrease limitu databáze, nebo odebrat databáze z hello elastického fondu. |
| 40889 |EX_USER |Hello Dtu nebo limit úložiště pro elastický fond hello ' %. * ls' nelze zmenšit, vzhledem k tomu, které by poskytují dostatek úložného prostoru pro jeho databáze. |Název elastického fondu. |Pokus toodecrease hello limit úložiště elastického fondu hello pod jeho využití úložiště. |Zvažte snížení využití úložiště hello jednotlivých databází v elastickém fondu hello nebo odebrat databáze z fondu hello v pořadí tooreduce jeho Dtu nebo úložiště limit. |
| 40891 |EX_USER |Hello minimální počet jednotek DTU na databázi (%d) nesmí překročit hello maximální počet jednotek DTU na databázi (%d). |Minimální počet jednotek DTU na databázi; Maximální počet jednotek DTU na databázi. |Pokus tooset hello minimální počet jednotek DTU na databázi vyšší než hello na databázi maximální počet jednotek DTU. |Zkontrolujte, zda minimální počet jednotek DTU hello za databáze není delší než hello na databázi maximální počet jednotek DTU. |
| Bude doplněno |EX_USER |velikost úložiště Hello může jedna databáze v elastickém fondu nesmí překročit hello maximální velikost povolenou ' %. * ls' služby elastického fondu vrstvy. |úrovně služby elastického fondu |maximální velikost databáze hello Hello překračuje maximální velikost hello povolenou hello úrovně služby elastického fondu. |Nastavte prosím maximální velikost hello hello databáze v souladu s limity hello hello maximální velikost povolenou hello úrovně služby elastického fondu. |

Související témata:

* [Vytvoření fondu elastické databáze (C#)](sql-database-elastic-pool-manage-csharp.md) 
* [Správa elastického fondu (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Vytvoření fondu elastické databáze (PowerShell)](sql-database-elastic-pool-manage-powershell.md) 
* [Sledování a správě fondu elastické databáze (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Obecné chyby
nevztahuje se na ně Hello následujícím chybám do všech předchozích kategorií.

| Kód chyby | Závažnost | Popis |
| ---:| ---:|:--- |
| 15006 |16 |<AdministratorLogin>není platný název protože obsahuje neplatné znaky. |
| 18452 |14 |Přihlášení se nezdařilo. Hello přihlášení proběhlo z nedůvěryhodné domény a nelze použít s Windows authentication.%. & #x2a; ls (přihlášení systému Windows není podporováno v této verzi systému SQL Server). |
| 18456 |14 |Přihlášení uživatele se nezdařilo. %. & #x2a;ls'.%. & #x2a; ls %. & #x2a; ls (hello přihlášení se nezdařilo pro uživatele "%. & #x2a; ls". Hello Změna hesla se nezdařila. Změna hesla při přihlašování není podporováno v této verzi systému SQL Server.) |
| 18470 |14 |Přihlášení se nezdařilo pro uživatele "%. & #x2a; ls". Důvod: účet hello je disabled.%. & #x2a; ls |
| 40014 |16 |Více databází nelze použít v hello stejné transakci. |
| 40054 |16 |Tabulky bez clusterovaného indexu nejsou podporovány v této verzi systému SQL Server. Vytvořit clusterovaný index a zkuste to znovu. |
| 40133 |15 |Tato operace není podporována v této verzi systému SQL Server. |
| 40506 |16 |Zadaný kód SID je neplatný pro tuto verzi systému SQL Server. |
| 40507 |16 |' %. & #x2a; ls se nedá vyvolat, s parametry v této verzi systému SQL Server. |
| 40508 |16 |Příkaz USE není podporovaný tooswitch mezi databázemi. Použijte nové připojení tooconnect tooa jinou databázi. |
| 40510 |16 |Příkaz '%. & #x2a; ls' není podporován v této verzi systému SQL Server |
| 40511 |16 |Integrovaná funkce '%. & #x2a; ls' není v této verzi systému SQL Server podporována. |
| 40512 |16 |Zastaralé funkce "%ls" v této verzi systému SQL Server nepodporuje. |
| 40513 |16 |Server proměnné '%. & #x2a; ls' není v této verzi systému SQL Server podporována. |
| 40514 |16 |v této verzi systému SQL Server nepodporuje '%ls'. |
| 40515 |16 |Název odkazu toodatabase nebo serveru v '%. & #x2a; ls' není v této verzi systému SQL Server podporována. |
| 40516 |16 |Globální dočasné objekty nejsou v této verzi systému SQL Server podporovány. |
| 40517 |16 |Možnost klíčového slova nebo příkazu '%. & #x2a; ls' není v této verzi systému SQL Server podporována. |
| 40518 |16 |Příkaz DBCC '%. & #x2a; ls' není v této verzi systému SQL Server podporována. |
| 40520 |16 |Zabezpečitelná Třída '% S_MSG' není v této verzi systému SQL Server podporována. |
| 40521 |16 |Zabezpečitelná Třída '% S_MSG' není v oboru serveru hello v této verzi systému SQL Server podporována. |
| 40522 |16 |Typ objektu "%. & #x2a; ls" databáze není podporován v této verzi systému SQL Server. |
| 40523 |16 |Vytvoření implicitního uživatele '%. & #x2a; ls' není podporován v této verzi systému SQL Server. Explicitní vytvoření hello uživatele před jeho použitím. |
| 40524 |16 |Datový typ '%. & #x2a; ls' není v této verzi systému SQL Server podporována. |
| 40525 |16 |S '%.ls' není podporován v této verzi systému SQL Server. |
| 40526 |16 |' %. & #x2a; zprostředkovatele sady řádků ls' v této verzi systému SQL Server není podporována. |
| 40527 |16 |Odkazované servery nejsou v této verzi systému SQL Server podporovány. |
| 40528 |16 |Uživatele nelze mapovat toocertificates, asymetrické klíče ani na přihlášení systému Windows v této verzi systému SQL Server. |
| 40529 |16 |Integrovaná funkce "%. & #x2a; ls" v zosobnění kontextu není podporováno v této verzi systému SQL Server. |
| 40532 |11 |Nelze otevřít serveru "%. & #x2a; ls" požadovaném přihlášením hello. Hello přihlášení se nezdařilo. |
| 40553 |16 |Hello relace byla ukončena z důvodu nadměrného využití paměti. Zkuste upravit vašeho dotazu tooprocess méně řádků.<br/><br/>*Poznámka:* snížení počtu hello `ORDER BY` a `GROUP BY` operace v kódu jazyka Transact-SQL pomáhá snížit požadavky na paměť hello tohoto dotazu. |
| 40604 |16 |Může není CREATE/ALTER DATABASE, protože byste překročili kvótu hello hello serveru. |
| 40606 |16 |V této verzi systému SQL Server nepodporuje připojení databáze. |
| 40607 |16 |Přihlášení systému Windows nejsou v této verzi systému SQL Server podporovány. |
| 40611 |16 |Servery můžete mít definovaný maximálně 128 pravidel brány firewall. |
| 40614 |16 |Počáteční IP adresa pravidla brány firewall nesmí přesahovat koncovou IP adresu. |
| 40615 |16 |Nelze otevřít server: {0} požadovaném přihlášením hello. Klient s IP adresou objekt {1}' tooaccess hello server není povolena.  tooenable přístup, použijte hello portál databázi SQL nebo spusťte příkaz sp_set_firewall_rule v hlavní databázi toocreate hello pravidlo brány firewall pro tuto IP adresu nebo rozsah adres.  To může trvat až minut toofive pro tuto změnu tootake vliv. |
| 40617 |16 |Název pravidla brány firewall Hello, který začíná <rule name> je příliš dlouhý. Maximální délka je 128. |
| 40618 |16 |Název pravidla brány firewall Hello nemůže být prázdný. |
| 40620 |16 |Hello přihlášení se nezdařilo pro uživatele "%. & #x2a; ls". Hello Změna hesla se nezdařila. Změna hesla při přihlašování není podporována v této verzi systému SQL Server. |
| 40627 |20 |Probíhá operace na serveru: {0} a databáze objekt {1}.. Počkejte prosím několik minut, než to zkusíte znovu. |
| 40630 |16 |Ověření hesla se nezdařilo. Hello heslo nesplňuje požadavky zásad, protože je příliš krátké. |
| 40631 |16 |Hello heslo je příliš dlouhý. Hello heslo by měl mít víc než 128 znaků. |
| 40632 |16 |Ověření hesla se nezdařilo. Hello heslo nesplňuje požadavky zásad, protože není dostatečně komplexní. |
| 40636 |16 |Nemůžete použít název vyhrazené databáze "%. & #x2a; ls" v této operaci. |
| 40638 |16 |Neplatné id odběru < id předplatného >. Odběr neexistuje. |
| 40639 |16 |Požadavek neodpovídá tooschema: <schema error>. |
| 40640 |20 |Hello serveru došlo k neočekávané výjimce. |
| 40641 |16 |Hello zadat, že umístění je neplatné. |
| 40642 |17 |Hello server je aktuálně zaneprázdněna. Zkuste to prosím znova později. |
| 40643 |16 |Hello zadána hodnota záhlaví x-ms-version je neplatná. |
| 40644 |14 |Neúspěšné tooauthorize přístup toohello zadaný odběr. |
| 40645 |16 |ServerName <servername> nemůže být prázdný nebo mít hodnotu null. Ho může obsahovat jenom malá písmena 'a'-'z', hello číslice 0-9 a pomlčku hello. nesmí Hello pomlčkou začínat ani končit hello název. |
| 40646 |16 |ID odběru nemůže být prázdný. |
| 40647 |16 |Předplatné < server servername nemá žádné id předplatného. |
| 40648 |17 |Příliš mnoho požadavků prováděly. Zkuste to prosím znovu později. |
| 40649 |16 |Je zadán neplatný typ obsahu. Je podporován pouze application/xml. |
| 40650 |16 |Předplatné < id předplatného > neexistuje nebo není připraven pro operaci hello. |
| 40651 |16 |Toocreate serveru selhalo, protože předplatné hello < id předplatného > je zakázané. |
| 40652 |16 |Nelze přesunout nebo vytvořit server. Předplatné < id předplatného > způsobí překročení kvóty serveru. |
| 40671 |17 |Selhání komunikace mezi hello brány a služba správy hello. Zkuste to prosím znovu později. |
| 40852 |16 |Databázi nelze otevřít ' %. *ls na serveru ' %.* ls' požadovaném přihlášením hello. Databáze Access toohello je povoleno pouze pomocí řetězce připojení s povoleným zabezpečením. tooaccess to databáze, upravte vaše toocontain řetězce připojení zabezpečeného serveru hello plně kvalifikovaný název domény -.database.windows "název serveru".net by měl být název upravené too'server'.database. `secure`. windows.net. |
| 45168 |16 |Hello systému SQL Azure je zatížen a umísťuje horní limit na souběžných operací DB CRUD pro jeden server (například vytvořit databázi). zadaný v hello chybovou zprávu server Hello překročil maximální počet souběžných připojení hello. Zkuste to znovu později. |
| 45169 |16 |Hello systému SQL azure je zatížení a je umístění horní limit počtu hello operace CRUD souběžných serverů pro v rámci jednoho předplatného (například vytvořit serveru). předplatné Hello uveden v chybě hello zpráva byla překročena maximální počet souběžných připojení hello a hello žádost byla zamítnuta. Zkuste to znovu později. |

## <a name="related-links"></a>Související odkazy
* [Funkce databáze Azure SQL](sql-database-features.md)
* [Omezení prostředků Azure SQL Database](sql-database-resource-limits.md)

