---
title: "aaaFix chyby připojení SQL, přechodné chybě. | Microsoft Docs"
description: "Zjistěte, jak diagnostikovat tootroubleshoot a zabránit chyba připojení SQL nebo přechodné chybě ve službě Azure SQL Database. "
keywords: "připojení SQL, připojovací řetězec, problémy s připojením, přechodná chyba, došlo k chybě připojení"
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: daleche
ms.openlocfilehash: d225e610b9e88170ab53ca16d615bd07220603cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Oprava a diagnostika chyb připojení SQL a přechodných chyb služby SQL Database a jejich předcházení
Tento článek popisuje, jak tooprevent, odstraňování potíží, diagnostikovat a zmírnit chyb připojení a přechodné chyby, které klientské aplikace, zaznamená při komunikuje se službou Azure SQL Database. Zjistěte, jak tooconfigure logika opakovaných pokusů, sestavení hello připojovací řetězec a další nastavení připojení.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Přechodné chyby (přechodné chyby)
Přechodná chyba - navíc přechodná chyba - má základní příčinu, který bude brzy vyřeší. Příležitostně příčinu přechodné chyby je, když hello Azure systému rychle posune hardwarové prostředky toobetter Vyrovnávání zatížení různé úlohy. Většinu těchto událostí Rekonfigurace často dokončit za méně než 60 sekund. Během zadaného období Rekonfigurace může mít připojení k problémy tooAzure databáze SQL. Aplikace připojení tooAzure SQL databáze by měla být vytvářeny tooexpect tyto přechodné chyby popisovač je implementací opakujte logiku v jejich kódu místo zpřístupnění je toousers jako chyby aplikace.

Pokud váš klientský program nepoužívá ADO.NET, váš program se nedá instrukce o přechodné chybě hello hello throw z **SqlException**. Hello **číslo** vlastnost může být porovná hello seznam přechodných chyb uvedených hello horní části tématu hello: [kódy chyb SQL pro klientské aplikace SQL Database](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Připojení a příkaz
Budete opakujte pokus o připojení SQL hello nebo znovu vytvořit v závislosti na hello následující:

* **Při pokusu o připojení dojde k přechodné chybě**: hello připojení je třeba opakovat po zpozdit pro několik sekund.
* **Příkaz dotazu SQL došlo k přechodné chybě**: příkaz hello nesmí opakovat okamžitě. Místo toho po prodlevě, hello by měl být čerstvě navázat připojení. Potom můžete zkusit znovu příkaz hello.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Logika opakování pro přechodné chyby
Klient programy, které někdy dojde k přechodné chybě jsou robustnější při obsahují logiku opakovaných pokusů.

Pokud váš program komunikuje s Azure SQL Database pomocí 3. stran middleware, Dotázat se s dodavatelem hello zda hello middleware obsahuje logiku opakovaných pokusů pro přechodné chyby.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Zásady pro opakování
* Pokud hello chyba je přechodná je třeba opakovat tooopen pokusu o připojení.
* Příkaz SELECT, který selže s přechodná chyba by neměl přímo opakovat.
  
  * Místo toho udržování připojení a poté opakujte hello vyberte.
* Nepodaří-li prohlášení aktualizace SQL s přechodná chyba transakce, musí před hello, které se pokus o aktualizaci vytvořit novou připojení.
  
  * Logika opakovaných pokusů Hello musíte zajistit, že buď hello celou databázi transakce byla dokončena, nebo že hello celá transakce je vrácena zpět.

#### <a name="other-considerations-for-retry"></a>Další důležité informace pro opakování
* Batch program, který se automaticky spustí po pracovní době, a před ráno, která dokončí si může dovolit toovery pacienta s dlouho časových intervalů mezi jeho počet pokusů o opakování.
* Program uživatelského rozhraní by měl účet pro toogive hello lidského tendence, že se až po příliš dlouho čekání.
  
  * Ale hello řešení nesmí být tooretry každých několik sekund, protože tyto zásady můžete vyplnění hello systému s žádostí.

#### <a name="interval-increase-between-retries"></a>Zvyšte interval mezi opakovanými pokusy
Doporučujeme, abyste zpoždění pro 5 sekund před první opakování. Opakování po prodlevě kratší než 5 sekund rizika čtenáře hello cloudové služby. Pro každý další pokusy hello zpoždění by měl růst exponenciálnímu, až tooa maximálně 60 sekund.

Diskuzi o hello *blokování období* pro klienty, kteří používají ADO.NET je k dispozici v [SQL sdružování připojení serveru (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Můžete také tooset maximální počet opakovaných pokusů před programu hello samoobslužné ukončí.

#### <a name="code-samples-with-retry-logic"></a>Ukázky kódu s logika opakovaných pokusů
Ukázky kódu s logikou opakování v různých programovacích jazyků, jsou k dispozici na:

* [Knihovny připojení k databázi SQL a SQL Server](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testování logika opakovaných pokusů
tootest logika opakovaných pokusů musí simulovat nebo způsobit chybu, než může být vyřešen při běhu programu.

##### <a name="test-by-disconnecting-from-hello-network"></a>Test odpojení od sítě hello
Jedním ze způsobů, které můžete testovat logika opakovaných pokusů je toodisconnect klientský počítač z hello sítě během programu hello. Chyba Hello bude:

* **SqlException.Number** = 11001
* Zpráva: "znám žádný takový hostitel je"

Jako součást hello nejprve opakujte pokus o, můžete vašeho programu opravte hello chybně a pokusíte se tooconnect.

toomake tomto praktické, můžete před zahájením vašeho programu odpojit počítači ze sítě hello. Potom váš program rozpozná běhu parametr, který způsobí, že programu hello:

1. Jako přechodný dočasně přidáte 11001 tooits seznam tooconsider chyby.
2. Pokus první připojení jako obvykle.
3. Po chybě hello se zjistilo, odeberte ze seznamu hello 11001.
4. Zobrazí se zpráva, hello uživatele tooplug hello počítače do sítě hello.
   * Další pozastavení provádění pomocí buď hello **Console.ReadLine** metoda nebo dialogovém okně tlačítko OK. uživatel Hello stisknutím klávesy Enter hello po hello počítače zapojen do sítě hello.
5. Pokus znovu tooconnect, byla očekávána úspěch.

##### <a name="test-by-misspelling-hello-database-name-when-connecting"></a>Název databáze hello chybně test při připojování
Váš program můžete účelově chybně hello uživatelské jméno před hello první pokus o připojení. Chyba Hello bude:

* **SqlException.Number** = 18456
* Zpráva: "přihlášení se nezdařilo pro uživatele 'WRONG_MyUserName'."

Jako součást hello nejprve opakujte pokus o, můžete vašeho programu opravte hello chybně a pokusíte se tooconnect.

toomake tomto praktické, vaše aplikace může rozeznat běhu parametr, který způsobí, že programu hello:

1. Jako přechodný dočasně přidáte 18456 tooits seznam tooconsider chyby.
2. Přidejte účelově 'WRONG_' toohello uživatelské jméno.
3. Po chybě hello se zjistilo, odeberte ze seznamu hello 18456.
4. Odebrání 'WRONG_' hello uživatelské jméno.
5. Pokus znovu tooconnect, byla očekávána úspěch.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Parametry objektu SqlConnection .NET pro opakování připojení
Pokud váš klientský program připojuje tootooAzure SQL Database pomocí třídy rozhraní .NET Framework hello **System.Data.SqlClient.SqlConnection**, měli byste použít .NET 4.6.1 nebo novější (nebo .NET Core), můžete použít funkci opakování jeho připojení. Podrobnosti o hello funkce jsou [zde](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points toodn632678.aspx, which links tooa downloadable .docx related tooSqlClient and SQL Server 2014.
-->


Při vytváření hello [připojovací řetězec](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) pro vaše **SqlConnection** objektu, by měly koordinovat hello hodnoty mezi hello následující parametry:

* ConnectRetryCount &nbsp; &nbsp; *(výchozí hodnota je 1. Rozsah je 0 až 255).*
* ConnectRetryInterval &nbsp; &nbsp; *(výchozí hodnota je 1 sekunda. Rozsah je 1 až 60).*
* Časový limit připojení &nbsp; &nbsp; *(výchozí hodnota je 15 sekund. Rozsah hodnot je 0 až 2147483647)*

Konkrétně vybrané hodnoty měli hello následující rovnosti true:

* Časový limit připojení = ConnectRetryCount * ConnectionRetryInterval

Například pokud hello count = 3 a interval = 10 sekund, vypršení časového limitu z jen 29 sekund nebude poměrně poskytnout hello systému dostatek času pro jeho 3. a finální opakování v připojení: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Připojení a příkaz
Hello **ConnectRetryCount** a **ConnectRetryInterval** parametry umožní vaše **SqlConnection** objekt opakování hello operaci připojení bez informuje nebo bothering vaší program, jako je například vrácení řízení tooyour program. opakování Hello se může objevit v následujících situacích hello:

* volání metody mySqlConnection.Open
* volání metody mySqlConnection.Execute

Není k dispozici subtlety. Pokud dojde k přechodné chybě. při vaší *dotazu* je spouštěna, vaše **SqlConnection** objekt nemá hello opakovat operaci připojení a jeho určitě neopakuje dotazu. Ale **SqlConnection** velmi rychle kontroly hello připojení před odesláním dotazu pro provedení. Pokud hello Rychlá kontrola zjistí problému s připojením **SqlConnection** opakování hello operaci připojení. Pokud hello opakování úspěšné, odešle dotaz je pro provedení.

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Měli ConnectRetryCount nelze kombinovat s logika opakovaných pokusů aplikace?
Předpokládejme, že aplikace má robustní vlastní opakování logiku. Ho mohou zkuste hello operaci připojení. 4 časy. Pokud přidáte **ConnectRetryInterval** a **ConnectRetryCount** = 3 tooyour připojovací řetězec, se zvýší too4 počet opakování hello * 3 = 12 opakování. Hodláte nemusí takové velký počet opakování.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-tooazure-sql-database"></a>TooAzure připojení databáze SQL
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Připojení: Připojovací řetězec
Hello potřebné pro připojení tooAzure připojovací řetězec databáze SQL se mírně liší z hello řetězce pro připojení tooMicrosoft systému SQL Server. Hello připojovací řetězec pro vaši databázi můžete zkopírovat z hello [portál Azure](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Připojení: IP adresa
Je nutné nakonfigurovat hello databáze SQL serveru tooaccept komunikaci z IP adresy hello hello počítače, který je hostitelem váš klientský program. To uděláte tak, že upravíte nastavení brány firewall hello prostřednictvím hello [portál Azure](https://portal.azure.com/).

Pokud zapomenete tooconfigure hello IP adresu, program se nezdaří s užitečný chybovou zprávu, která stavy hello nezbytné IP adresu.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Další informace najdete v tématu: [postupy: Konfigurace nastavení brány firewall pro službu SQL Database](sql-database-configure-firewall-settings.md)

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Připojení: porty
Obvykle potřebujete jenom tooensure, že port 1433 je otevřený pro odchozí komunikaci, hello počítače, který je hostitelem program klienta.

Například pokud je váš klientský program hostované na počítači se systémem Windows, hello brány Windows Firewall na hostiteli hello vám umožní tooopen port 1433:

1. Otevřete ovládací panely hello
2. &gt;Všechny položky Ovládacích panelů
3. &gt;Brána Windows Firewall
4. &gt;Upřesnit nastavení
5. &gt;Odchozí pravidla
6. &gt;Akce
7. &gt;Nové pravidlo

Pokud je váš klientský program hostované na virtuální počítač Azure (VM), měli byste si přečíst:<br/>[Porty nad rámec 1433 pro technologii ADO.NET 4.5 a SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Základní informace o konfigurace portů a IP adresy, najdete v tématu: [brány firewall databáze Azure SQL Database](sql-database-firewall-configure.md)

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Připojení: ADO.NET 4.6.1
Pokud váš program používá třídy ADO.NET jako **System.Data.SqlClient.SqlConnection** tooconnect tooAzure SQL Database, doporučujeme vám použít rozhraní .NET Framework verze 4.6.1 nebo vyšší.

ADO.NET 4.6.1:

* Pro databázi SQL Azure, je lepší spolehlivostí při otevření připojení pomocí hello **SqlConnection.Open** metoda. Hello **otevřete** metoda nyní zahrnuje nejlepší mechanismy úsilí opakování v odpovědi tootransient chyb, některé chyby v době vymezené připojení hello.
* Podporuje sdružování připojení. To zahrnuje efektivní ověření, který hello připojení funguje objekt nabízí vašeho programu.

Použijete-li objekt připojení z fondu připojení, doporučujeme vám, že váš program dočasně uzavřít hello připojení, když není ihned používat. Opakovaném otevření připojení není nákladné je hello způsob vytvoření nového připojení.

Pokud používáte ADO.NET 4.0 nebo starší, doporučujeme upgradu toohello nejnovější ADO.NET.

* Od listopadu 2015, můžete [stáhnout ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostika
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostika: Test, zda může připojit nástroje
Pokud váš program selhává tooconnect tooAzure SQL Database, jeden diagnostiky možnost je tootry tooconnect s programem nástroj. V ideálním případě by nástroj hello připojit pomocí hello stejnou knihovnu, který program používá.

Na libovolném počítači Windows můžete vyzkoušet tyto nástroje:

* SQL Server Management Studio (ssms.exe), který se připojuje pomocí ADO.NET.
* Sqlcmd.exe, který se připojuje pomocí [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).

Po připojení otestujte, zda funguje krátké SQL SELECT dotazu.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-hello-open-ports"></a>Diagnostika: Zkontrolujte otevřené porty hello
Předpokládejme, že máte podezření, že pokusy o připojení se nedaří kvůli tooport problémy. V počítači můžete spustit nástroj, který hlásí hello konfiguraci portů.

V systému Linux hello může být užitečné následující nástroje:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * (Změnit hello příklad hodnota toobe IP adresu.)

Na Windows hello [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) nástroj může být užitečné. Tady je příklad spuštění, dotaz hello situace port na serveru Azure SQL Database a která byla spuštěna na přenosný počítač:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting tooresolve name tooIP address...
Name resolved too23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostiky: Chyby protokolu
Občasný problém je někdy nejlépe zjištěném detekce obecné vzoru přes dny nebo týdny.

Váš klient může být užitečné při diagnostiku protokolováním všechny chyby, který nalezne. Je možné toocorrelate hello položky protokolu s daty chyba, která interně Azure SQL Database samotné protokoly.

Enterprise knihovny 6 (EntLib60) nabízí spravované rozhraní .NET třídy tooassist s protokolováním:

* [5 - jako snadno jako návratem vypnout protokolu: pomocí hello blokem protokolování aplikace](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostika: Zkontrolujte protokoly systému chyby
Tady jsou některé příkazy jazyka Transact-SQL, vyberte tento dotaz protokoly chyby a dalšími informacemi.

| Dotaz protokolu | Popis |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |Hello [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) zobrazení nabízí informace o jednotlivých událostech, včetně některých, která mohou způsobit přechodné chyby nebo chyby připojení.<br/><br/>V ideálním případě mohou korelovat hello **start_time** nebo **end_time** hodnoty s informacemi o když váš klientský program došlo k potížím.<br/><br/>**TIP:** je nutné připojit toohello **hlavní** databáze toorun to. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |Hello [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) zobrazení nabízí souhrnné počty typů událostí pro další diagnostiku.<br/><br/>**TIP:** je nutné připojit toohello **hlavní** databáze toorun to. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-hello-sql-database-log"></a>Diagnostika: Vyhledejte problém události v protokolu databáze SQL hello
Můžete vyhledat záznamy o událostech problém v hello protokolu databáze SQL Azure. Zkuste hello následující příkaz Transact-SQL, vyberte v hello **hlavní** databáze:

```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Několik řádků vrácená z sys.fn_xe_telemetry_blob_target_read_file
Dále je, jak může vypadat vrácený řádek. hodnoty null Hello vidět nejsou často null v dalších řádcích.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Knihovna Enterprise 6
Enterprise knihovny 6 (EntLib60) je architektura tříd rozhraní .NET usnadňující implementovat robustní klienti cloudové služby, z nichž jeden je služba Azure SQL Database hello. Můžete vyhledat témata vyhrazené tooeach oblasti, ve kterém můžete EntLib60 pomáhat při první navštivte stránky:

* [Knihovna Enterprise 6 – duben 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

Logika opakovaných pokusů pro přechodné chyby zpracování je jedné oblasti, ve kterém může být užitečné EntLib60:

* [4 - perseverance, tajný klíč všechny vítězství: pomocí hello přechodné chyby zpracování blokování aplikací](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> Hello zdrojový kód EntLib60 je k dispozici pro veřejné [Stáhnout](http://go.microsoft.com/fwlink/p/?LinkID=290898). Společnost Microsoft nemá žádné plány toomake další funkce aktualizace nebo údržby aktualizací tooEntLib.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Třídy EntLib60 pro přechodné chyby a zkuste to znovu
Následující třídy EntLib60 Hello jsou obzvláště užitečná pro logiku opakovaných pokusů. Všechny tyto v, nebo další pod, hello obor názvů **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*V oboru názvů hello **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

* **RetryPolicy** – třída
  
  * **ExecuteAction** – metoda
* **ExponentialBackoff** – třída
* **SqlDatabaseTransientErrorDetectionStrategy** – třída
* **ReliableSqlConnection** – třída
  
  * **Parametr ExecuteCommand** – metoda

V oboru názvů hello **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** – třída
* **NeverTransientErrorDetectionStrategy** – třída

Tady jsou odkazy tooinformation o EntLib60:

* Volné [stáhnout adresáře: pro vývojáře průvodce tooMicrosoft knihovny Enterprise Edition 2.](http://www.microsoft.com/download/details.aspx?id=41145)
* Osvědčené postupy: [opakujte obecné pokyny](../best-practices-retry-general.md) má vynikající podrobné informace o logika opakovaných pokusů.
* Stahování NuGet [Enterprise Library - přechodných chyb aplikace bloku 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-hello-logging-block"></a>EntLib60: blok protokolování hello
* blok protokolování Hello je velmi flexibilní a konfigurovat řešení, které vám umožní:
  
  * Vytvoření a uložení zprávy protokolu v celé řadě umístění.
  * Kategorizaci a filtrování zprávy.
  * Shromážděte kontextové informace, které jsou užitečné pro ladění a trasování, a také pro auditování a obecné protokolování požadavky.
* hello přehledů bloku protokolování Hello protokolování funkce z cíl protokolu hello tak, aby kód aplikace hello konzistentní bez ohledu na umístění hello a typ hello cíl protokolování úložiště.

Podrobnosti najdete v tématu: [5 – jako snadno jako návratem vypnout protokolu: pomocí hello blokem protokolování aplikace](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient metoda zdrojového kódu
Další z hello **SqlDatabaseTransientErrorDetectionStrategy** třídy, je hello C# zdrojového kódu pro hello **IsTransient** metoda. zdrojový kód Hello vysvětluje, které chyby považovány za toobe přechodná a worthy opakování od dubna 2013.

Řada **//comment** řádky byly odebrány z této kopie tooemphasize čitelnost.

```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in hello exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // hello service is currently busy. Retry hello request after 10 seconds.
            // Code: (reason code toobe decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode hello reason code from hello error message to
            // determine hello grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach hello decoded values as additional attributes to
            // hello original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // hello instance of SQL Server you attempted tooconnect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Další kroky
* Řešení potíží s další běžné problémy s připojením databáze SQL Azure, najdete v článku [připojení řešení problémů tooAzure SQL Database](sql-database-troubleshoot-common-connection-issues.md).
* [Připojení k serveru SQL sdružování (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [*Opakováním* je Apache 2.0 licenci pro obecné účely, opakování knihovny, které jsou napsané v **Python**, toosimplify hello úkolu přidávání toojust chování opakování o něco.](https://pypi.python.org/pypi/retrying)

