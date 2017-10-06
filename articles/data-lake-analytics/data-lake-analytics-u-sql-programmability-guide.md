---
title: "Příručka k programovatelnosti aaaU SQL pro Azure Data Lake | Microsoft Docs"
description: "Další informace o hello sadu služeb v Azure Data Lake, které umožňují toocreate velkých objemů dat cloudové platformy."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
ms.assetid: 63be271e-7c44-4d19-9897-c2913ee9599d
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: saveenr
ms.openlocfilehash: cc8f126234c6106a0dc633ce85a1d9ab1e634e30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-programmability-guide"></a>Průvodce programovatelnosti U-SQL

U-SQL je dotazovací jazyk, který je určený pro velkých datových typů úloh. Jednou z funkcí hello jedinečné jazykem U-SQL je kombinací hello hello deklarativní jazyka SQL jako s hello rozšiřitelnosti a programovatelnosti, který je zadán v jazyce C#. V této příručce budeme soustředit na hello rozšiřitelnosti a programovatelnosti hello jazyka U-SQL, který je povolený v jazyce C#.

## <a name="requirements"></a>Požadavky

Stáhněte a nainstalujte [nástrojů Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

## <a name="get-started-with-u-sql"></a>Začínáme s jazykem U-SQL  

Podívejme se na hello následující skript U-SQL:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso",   1500.0, "2017-03-39"),
            ("Woodgrove", 2700.0, "2017-04-10")
        ) AS 
              D( customer, amount );
@results =
    SELECT
        customer,
    amount,
    date
    FROM @a;    
```

Definuje sadu řádků názvem @a a vytvoří sadu řádků názvem @results z @a.

## <a name="c-types-and-expressions-in-u-sql-script"></a>Typy C# a výrazy ve skriptu U-SQL

Výraz U-SQL je výraz jazyka C# v kombinaci s logických operací U-SQL, `AND`, `OR`, a `NOT`. Výrazy U-SQL lze použít s příkazem SELECT, EXTRAHOVÁNÍ, kde s, Seskupit podle a DEKLAROVAT.

Například následující skript hello analyzuje řetězec hodnotu data a času v klauzuli SELECT hello.

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

Hello následující skript analyzuje řetězec hodnotu data a času v příkazu DECLARE.

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a>Použití jazyka C# výrazů pro konverze datových typů
Hello následující příklad ukazuje, jak můžete provést převod dat data a času pomocí výrazy jazyka C#. V tomto scénáři konkrétní data řetězce data a času je převeden toostandard datetime s půlnoc 00:00:00 čas zápis.

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 too@output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a>Použití jazyka C# výrazů pro dnešní datum
toopull dnešní datum, můžeme použít hello následující výraz jazyka C#:

```
DateTime.Now.ToString("M/d/yyyy")
```

Tady je příklad toho, jak toouse tento výraz ve skriptu:

```
@rs1 =
    SELECT
        MAX(guid) AS start_id,
        MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        user,
        des
    FROM @rs0
    GROUP BY user, des;
```



## <a name="using-net-assemblies"></a>Použití sestavení rozhraní .NET
Model rozšiřitelnosti U-SQL založena na hello možnost tooadd vlastní kód. V současné době U-SQL umožňuje snadno způsoby tooadd vlastní Microsoft. Na základě NET kód (zejména, C#). Můžete však také přidat vlastní kód, který je zapsán do jiných jazyků .NET, například VB.NET nebo F #. 

### <a name="register-a-net-assembly"></a>Registrace sestavení rozhraní .NET

Použijte tooplace příkaz CREATE ASSEMBLY hello sestavení .NET do databáze U-SQL. Po sestavení je v databázi, skriptů U-SQL můžete použít tyto sestavení pomocí příkazu referenční sestavení hello. 

Následující kód ukazuje, jak Hello tooregister sestavení:

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

Následující kód ukazuje, jak Hello tooreference sestavení:

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

Poraďte se hello [pokyny pro registraci sestavení](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) , která se vztahuje toto téma podrobněji.


### <a name="use-assembly-versioning"></a>Pomocí správy verzí sestavení
U-SQL v současné době používá hello rozhraní .NET Framework verze 4.5. Proto zajistěte, aby byly kompatibilní s danou verzi modulu runtime hello vlastního sestavení.

Jak už bylo zmíněno dříve, spustí kód U-SQL ve formátu 64bitovou (x 64). Proto se ujistěte, že jste kód napsali kompilované toorun na x64. V opačném případě se zobrazí chyba správný formát hello uvedena výše.

Každý odeslán sestavení knihoven DLL a souboru prostředků, jako je jiný modul runtime, nativní sestavení nebo konfiguračním souboru může být maximálně 400 MB. Celková velikost Hello nasazené prostředky, prostřednictvím nasazení prostředků nebo prostřednictvím odkazů tooassemblies a další soubory, nesmí překročit 3 GB.

Nakonec si všimněte, že každý U-SQL databáze může obsahovat pouze jednu verzi žádné zadaného sestavení. Například pokud budete potřebovat verze 7 a 8 verzi hello knihovně NewtonSoft Json.Net, je třeba tooregister je ve dvou různých databází. Kromě toho každý skript se může vztahovat pouze tooone verzi zadaného sestavení knihovny DLL. V tomto ohledu následuje U-SQL hello C# sestavení správy a správa verzí sémantiku.


## <a name="use-user-defined-functions-udf"></a>Použití uživatelem definované funkce: UDF
Uživatelem definované funkce U-SQL nebo UDF, jsou programovacích rutin, které přijímají parametry, provedení akce (například komplexní výpočet) a hello v důsledku této akce jako hodnotu. Hello vrátit hodnotu UDF může mít pouze jednu skalární hodnota. UDF U-SQL je možné volat v základní skript U-SQL jako všechny ostatní C# skalární funkce.

Doporučujeme, abyste inicializaci U-SQL uživatelsky definované funkce jako **veřejné** a **statické**.

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

První Podíváme se na hello jednoduchý příklad vytvoření uživatelem definovanou FUNKCI.

V tomto scénáři případ použití potřebujeme fiskální období hello toodetermine, včetně hello fiskální čtvrtletí a fiskální měsíc hello první přihlášení pro konkrétního uživatele hello. Hello první měsíc fiskálního roku hello v našem scénáři je června.

fiskální období toocalculate, zavedeme hello následující funkce jazyka C#:

```
public static string GetFiscalPeriod(DateTime dt)
{
    int FiscalMonth=0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter=0;
    if (FiscalMonth >=1 && FiscalMonth<=3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return "Q" + FiscalQuarter.ToString() + ":P" + FiscalMonth.ToString();
}
```

Jednoduše vypočítá fiskální měsíc a čtvrtletí a vrátí hodnotu řetězce. Pro června, hello první měsíc hello první fiskální čtvrtletí, použijeme "Q1:P1". Pro červenec můžeme použít "Q1:P2" a tak dále.

Toto je normální funkce jazyka C# s Snažíme se probíhající toouse v našem projekt U-SQL.

Zde je, jak vypadá hello kódu části v tomto scénáři:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        public static string GetFiscalPeriod(DateTime dt)
        {
            int FiscalMonth=0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter=0;
            if (FiscalMonth >=1 && FiscalMonth<=3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return "Q" + FiscalQuarter.ToString() + ":" + FiscalMonth.ToString();
        }

    }

}
```

Teď se zaměříme toocall tuto funkci z hello základní skript U-SQL. toodo, máme tooprovide plně kvalifikovaný název hello funkce, včetně hello oboru názvů, který je v tomto případě NameSpace.Class.Function(parameter).

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

Toto je základní skript skutečné U-SQL hello:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt DateTime,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

DECLARE @default_dt DateTime = Convert.ToDateTime("06/01/2016");

@rs1 =
    SELECT
        MAX(guid) AS start_id,
    MIN(dt) AS start_time,
        MIN(Convert.ToDateTime(Convert.ToDateTime(dt<@default_dt?@default_dt:dt).ToString("yyyy-MM-dd"))) AS start_zero_time,
        MIN(USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)) AS start_fiscalperiod,
        user,
        des
    FROM @rs0
    GROUP BY user, des;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

Toto je výstupní soubor hello spuštění skriptu hello:

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

Tento příklad ukazuje jednoduchý využití vložené UDF v U-SQL.

### <a name="keep-state-between-udf-invocations"></a>Zachovat stav mezi UDF volání
U-SQL C# programovatelnosti objekty může být více pokročilé, využitím interaktivity prostřednictvím hello kódu globální proměnné. Podívejme se na následující scénář případu použití obchodní hello.

Ve velkých organizacích mohou uživatelé přepínat mezi typy interních aplikací. Může jít o Microsoft Dynamics CRM, PowerBI a tak dále. Zákazníci může být vhodné tooapply analýzu telemetrii jak uživatelé přepínat mezi různými aplikacemi, jaké hello využití trendů jsou a tak dále. cílem Hello pro firmy hello je toooptimize využití aplikace. Také může chtějí toocombine různé aplikace nebo konkrétní rutiny přihlášení.

tooachieve tento cíl máme toodetermine ID relace a prodleva mezi hello poslední relace, který došlo k chybě.

Budeme potřebovat toofind předchozí přihlášení a pak mu přiřaďte toto přihlášení tooall relací, které probíhá generovaného toohello stejná aplikace. Hello první výzva je, že základní skript U-SQL nepovoluje nám tooapply výpočty přes již počítaného sloupce se funkce LAG. Hello Druhá výzva je, že jsme tookeep hello konkrétní relace pro všechny relace v rámci hello stejné časové období.

toosolve tento problém jsme použít globální proměnné uvnitř části kódu: `static public string globalSession;`.

Tato globální proměnná je použité toohello celý řádků při našich provádění skriptu.

Tady je části kódu hello naše programu U-SQL:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace USQLApplication21
{
    public class UserSession
    {
        static public string globalSession;
        static public string StampUserSession(string eventTime, string PreviousRow, string Session)
        {

            if (!string.IsNullOrEmpty(PreviousRow))
            {
                double timeGap = Convert.ToDateTime(eventTime).Subtract(Convert.ToDateTime(PreviousRow)).TotalMinutes;
                if (timeGap <= 60) {return Session;}
                else {return Guid.NewGuid().ToString();}
            }
            else {return Guid.NewGuid().ToString();}

        }

        static public string getStampUserSession(string Session)
        {
            if (Session != globalSession && !string.IsNullOrEmpty(Session)) { globalSession = Session; }
            return globalSession;
        }

    }
}
```

Tento příklad ukazuje hello – globální proměnná `static public string globalSession;` použít uvnitř hello `getStampUserSession` funkce a získávání inicializace každý čas hello relace parametr je změnit.

Hello základní skript U-SQL je následující:

```
DECLARE @in string = @"\UserSession\test1.tsv";
DECLARE @out1 string = @"\UserSession\Out1.csv";
DECLARE @out2 string = @"\UserSession\Out2.csv";
DECLARE @out3 string = @"\UserSession\Out3.csv";

@records =
    EXTRACT DataId string,
            EventDateTime string,           
            UserName string,
            UserSessionTimestamp string

    FROM @in
    USING Extractors.Tsv();

@rs1 =
    SELECT 
        EventDateTime,
        UserName,
    LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,          
        string.IsNullOrEmpty(LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,           
        USQLApplication21.UserSession.StampUserSession
           (
            EventDateTime,
            LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC),
            LAG(UserSessionTimestamp, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)
           ) AS UserSessionTimestamp
    FROM @records;

@rs2 =
    SELECT 
        EventDateTime,
        UserName,
        LAG(EventDateTime, 1) 
        OVER(PARTITION BY UserName ORDER BY EventDateTime ASC) AS prevDateTime,
        string.IsNullOrEmpty( LAG(EventDateTime, 1) OVER(PARTITION BY UserName ORDER BY EventDateTime ASC)) AS Flag,
        USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp) AS UserSessionTimestamp
    FROM @rs1
    WHERE UserName != "UserName";

OUTPUT @rs2
    too@out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

Funkce `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` je zde volána v průběhu hello druhý paměti řádků výpočtu. Pak předá hello `UserSessionTimestamp` sloupce a vrátí hodnotu, dokud hello `UserSessionTimestamp` došlo ke změně.

výstupní soubor Hello vypadá takto:

```
"2016-02-19T07:32:36.8420000-08:00","User1",,True,"72a0660e-22df-428e-b672-e0977007177f"
"2016-02-17T11:52:43.6350000-08:00","User2",,True,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-17T11:59:08.8320000-08:00","User2","2016-02-17T11:52:43.6350000-08:00",False,"4a0cd19a-6e67-4d95-a119-4eda590226ba"
"2016-02-11T07:04:17.2630000-08:00","User3",,True,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-11T07:10:33.9720000-08:00","User3","2016-02-11T07:04:17.2630000-08:00",False,"51860a7a-1610-4f74-a9ea-69d5eef7cd9c"
"2016-02-15T21:27:41.8210000-08:00","User3","2016-02-11T07:10:33.9720000-08:00",False,"4d2bc48d-bdf3-4591-a9c1-7b15ceb8e074"
"2016-02-16T05:48:49.6360000-08:00","User3","2016-02-15T21:27:41.8210000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-16T06:22:43.6390000-08:00","User3","2016-02-16T05:48:49.6360000-08:00",False,"dd3006d0-2dcd-42d0-b3a2-bc03dd77c8b9"
"2016-02-17T16:29:53.2280000-08:00","User3","2016-02-16T06:22:43.6390000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T16:39:07.2430000-08:00","User3","2016-02-17T16:29:53.2280000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-17T17:20:39.3220000-08:00","User3","2016-02-17T16:39:07.2430000-08:00",False,"2fa899c7-eecf-4b1b-a8cd-30c5357b4f3a"
"2016-02-19T05:23:54.5710000-08:00","User3","2016-02-17T17:20:39.3220000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T05:48:37.7510000-08:00","User3","2016-02-19T05:23:54.5710000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T06:40:27.4830000-08:00","User3","2016-02-19T05:48:37.7510000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T07:27:37.7550000-08:00","User3","2016-02-19T06:40:27.4830000-08:00",False,"6ca7ed80-c149-4c22-b24b-94ff5b0d824d"
"2016-02-19T19:35:40.9450000-08:00","User3","2016-02-19T07:27:37.7550000-08:00",False,"3f385f0b-3e68-4456-ac74-ff6cef093674"
"2016-02-20T00:07:37.8250000-08:00","User3","2016-02-19T19:35:40.9450000-08:00",False,"685f76d5-ca48-4c58-b77d-bd3a9ddb33da"
"2016-02-11T09:01:33.9720000-08:00","User4",,True,"9f0cf696-c8ba-449a-8d5f-1ca6ed8f2ee8"
"2016-02-17T06:30:38.6210000-08:00","User4","2016-02-11T09:01:33.9720000-08:00",False,"8b11fd2a-01bf-4a5e-a9af-3c92c4e4382a"
"2016-02-17T22:15:26.4020000-08:00","User4","2016-02-17T06:30:38.6210000-08:00",False,"4e1cb707-3b5f-49c1-90c7-9b33b86ca1f4"
"2016-02-18T14:37:27.6560000-08:00","User4","2016-02-17T22:15:26.4020000-08:00",False,"f4e44400-e837-40ed-8dfd-2ea264d4e338"
"2016-02-19T01:20:31.4800000-08:00","User4","2016-02-18T14:37:27.6560000-08:00",False,"2136f4cf-7c7d-43c1-8ae2-08f4ad6a6e08"
```

Tento příklad ukazuje složitější scénář případu použití používáme globální proměnné uvnitř oddíl kódu, který je použité toohello celý paměti řádků.

## <a name="use-user-defined-types-udt"></a>Uživatelem definované typy použití: UDT
Uživatelem definované typy nebo UDT, je jiná funkcí programovatelnosti U-SQL. U-SQL UDT funguje jako regulární C# definovaný uživatelem typem. C# je jazyk silného typu, který umožňuje použití hello předdefinované a vlastní uživatelem definované typy.

U-SQL nelze implicitně serializaci nebo libovolný uživatelsky definovaný typ deserializovat, když hello UDT předána mezi vrcholy v sady řádků. To znamená, že tento uživatel hello má tooprovide explicitní formátování s použitím rozhraní IFormatter hello. To poskytuje U-SQL s hello serializovat a deserializovat metody pro hello UDT.

> [!NOTE]
> Předdefinované extraktory U-SQL a outputters aktuálně nelze serializovat nebo deserializovat UDT tooor dat ze souborů i hello IFormatter sady. Takže když píšete UDT data tooa soubor s hello výstup příkazu, nebo jeho čtení s extrakci, máte toopass ji jako řetězec nebo bajtové pole. Potom volání hello serializace a deserializace explicitně kódu (to znamená, metodu ToString() hello UDT). Uživatelem definované extraktory a outputters na hello jiných ruční, lze číst a zapisovat UDT.

Pokud jsme zkuste toouse UDT v EXTRAKTOR nebo OUTPUTTER (mimo předchozí vyberte), jak je vidět tady:

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

Nemůžeme zobrazit hello následující chybě:

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used toooutput column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how tooserialize this type, or call a serialization method on hello type in
hello preceding SELECT. C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

toowork s UDT v outputter buď máme tooserialize ho toostring s hello metodu ToString() nebo vytvořit vlastní outputter.

UDT aktuálně nelze použít v GROUP BY. Pokud UDT se používá v GROUP BY, je vyvolána hello následující chybě:

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want toouse with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

toodefine typ definovaný uživatelem, musíme:

* Přidejte následující obory názvů hello:

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* Přidat `Microsoft.Analytics.Interfaces`, což je vyžadováno pro rozhraní UDT hello. Kromě toho `System.IO` mohou být potřebné toodefine hello IFormatter rozhraní.

* Definování typu používá definované atributem SqlUserDefinedType.

**SqlUserDefinedType** je použité toomark definice typu v sestavení jako uživatelem definovaný typ (UDT) v U-SQL. Vlastnosti Hello na atribut hello odrážet fyzické vlastnosti hello UDT hello. Tato třída nelze dědí.

SqlUserDefinedType je povinný atribut pro UDT definici.

Hello konstruktoru třídy hello:  

* SqlUserDefinedTypeAttribute (formátovací modul typu)

* Formátovací modul typu: vyžaduje parametr toodefine formátování UDT – konkrétně hello typ hello `IFormatter` rozhraní musí být předán sem.

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* Typické UDT taky vyžaduje definice hello IFormatter rozhraní, jak ukazuje následující příklad hello:

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

Hello `IFormatter` rozhraní serializuje a poté serializuje grafu objektu s hello kořenový typ \<typeparamref name = "T" >.

\<typeparam name = "T" > hello kořenový typ pro hello objekt grafu tooserialize a deserializovat.

* **Deserializovat**: zrušte serializuje hello data v datovém proudu hello zadaný a reconstitutes hello grafu objektů.

* **Serializovat**: serializuje objektu, nebo grafu objektů, s hello zadaný kořenový toohello zadaný datový proud.

`MyType`instance: Instance typu hello.  
`IColumnWriter`Zapisovač / `IColumnReader` čtečky: hello základní sloupec datového proudu.  
`ISerializationContext`kontext: výčet, který definuje sadu příznaky, která určuje hello zdrojový nebo cílový kontext pro datový proud hello během serializace.

* **Zprostředkující**: Určuje, že hello zdrojový nebo cílový kontext není trvalého úložiště.

* **Trvalost**: Určuje, že hello zdrojové nebo cílové kontextu je trvalé úložiště.

Jako regulární C# typ definici UDT U-SQL můžete zahrnout přepsání pro operátory, jako +/ == /! =. Může také obsahovat statické metody. Například pokud přidáme toouse tento UDT jako parametr tooa agregační funkci MIN U-SQL, máme toodefine < operátor přepsání.

Dříve v tomto průvodci jsme ukázán příklad pro fiskálním období identifikaci z hello konkrétní datum ve formátu hello Qn:Pn (Q1:P10). Hello následující příklad ukazuje, jak toodefine vlastní typ pro hodnoty fiskálním období.

Tady je příklad oddílu kódu s vlastní UDT a IFormatter rozhraní:

```
[SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
public struct FiscalPeriod
{
    public int Quarter { get; private set; }

    public int Month { get; private set; }

    public FiscalPeriod(int quarter, int month):this()
    {
    this.Quarter = quarter;
    this.Month = month;
    }

    public override bool Equals(object obj)
    {
    if (ReferenceEquals(null, obj))
    {
        return false;
    }

    return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
    }

    public bool Equals(FiscalPeriod other)
    {
return this.Quarter.Equals(other.Quarter) && this.Month.Equals(other.Month);
    }

    public bool GreaterThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
    }

    public bool LessThan(FiscalPeriod other)
    {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
    }

    public override int GetHashCode()
    {
    unchecked
    {
        return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
    }
    }

    public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
    {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
    }

    public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.Equals(c2);
    }

    public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
    {
    return !c1.Equals(c2);
    }
    public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.GreaterThan(c2);
    }
    public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
    {
    return c1.LessThan(c2);
    }
    public override string ToString()
    {
    return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
    }

}

public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
{
    public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
    {
    using (var binaryWriter = new BinaryWriter(writer.BaseStream))
    {
        binaryWriter.Write(instance.Quarter);
        binaryWriter.Write(instance.Month);
        binaryWriter.Flush();
    }
    }

    public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
    {
    using (var binaryReader = new BinaryReader(reader.BaseStream))
    {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
        return result;
    }
    }
}
```

Hello definovaný typ. zahrnuje dvě čísla: čtvrtletí a měsíce. Operátory == /! = / > nebo < a statickou metodu ToString() jsou definovány v tomto poli.

Jak už bylo zmíněno dříve, můžete použít ve výrazech vyberte UDT, ale nejde použít v OUTPUTTER/EXTRAKTOR bez vlastní serializace. Buď má toobe serializován jako řetězec s ToString() nebo použít s vlastní OUTPUTTER/EXTRAKTOR.

Nyní probereme využití UDT. V části kódu jsme změnili naše GetFiscalPeriod funkce toohello následující:

```
public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
{
    int FiscalMonth = 0;
    if (dt.Month < 7)
    {
    FiscalMonth = dt.Month + 6;
    }
    else
    {
    FiscalMonth = dt.Month - 6;
    }

    int FiscalQuarter = 0;
    if (FiscalMonth >= 1 && FiscalMonth <= 3)
    {
    FiscalQuarter = 1;
    }
    if (FiscalMonth >= 4 && FiscalMonth <= 6)
    {
    FiscalQuarter = 2;
    }
    if (FiscalMonth >= 7 && FiscalMonth <= 9)
    {
    FiscalQuarter = 3;
    }
    if (FiscalMonth >= 10 && FiscalMonth <= 12)
    {
    FiscalQuarter = 4;
    }

    return new FiscalPeriod(FiscalQuarter, FiscalMonth);
}
```

Jak vidíte, vrátí hodnotu hello naše FiscalPeriod typu.

Zde jsme uveďte příklad jak toouse dále s ním v základní skript U-SQL. Tento příklad ukazuje různé formy UDT volání ze skriptu U-SQL.

```
DECLARE @input_file string = @"c:\work\cosmos\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"c:\work\cosmos\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        guid string,
        dt DateTime,
        user String,
        des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT 
        guid AS start_id,
        dt,
        DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Quarter AS fiscalquarter,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).Month AS fiscalmonth,
        USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt) + new USQL_Programmability.CustomFunctions.FiscalPeriod(1,7) AS fiscalperiod_adjusted,
        user,
        des
    FROM @rs0;

@rs2 =
    SELECT 
           start_id,
           dt,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           fiscalquarter,
           fiscalmonth,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS fiscalperiod,

       // This user-defined type was created in hello prior SELECT.  Passing hello UDT toothis subsequent SELECT would have failed if hello UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    too@output_file 
    USING Outputters.Text();
```

Tady je příklad oddílu úplné kódu:

```
using Microsoft.Analytics.Interfaces;
using Microsoft.Analytics.Types.Sql;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.IO;

namespace USQL_Programmability
{
    public class CustomFunctions
    {
        static public DateTime? ToDateTime(string dt)
        {
            DateTime dtValue;

            if (!DateTime.TryParse(dt, out dtValue))
                return Convert.ToDateTime(dt);
            else
                return null;
        }

        public static FiscalPeriod GetFiscalPeriodWithCustomType(DateTime dt)
        {
            int FiscalMonth = 0;
            if (dt.Month < 7)
            {
                FiscalMonth = dt.Month + 6;
            }
            else
            {
                FiscalMonth = dt.Month - 6;
            }

            int FiscalQuarter = 0;
            if (FiscalMonth >= 1 && FiscalMonth <= 3)
            {
                FiscalQuarter = 1;
            }
            if (FiscalMonth >= 4 && FiscalMonth <= 6)
            {
                FiscalQuarter = 2;
            }
            if (FiscalMonth >= 7 && FiscalMonth <= 9)
            {
                FiscalQuarter = 3;
            }
            if (FiscalMonth >= 10 && FiscalMonth <= 12)
            {
                FiscalQuarter = 4;
            }

            return new FiscalPeriod(FiscalQuarter, FiscalMonth);
        }



        [SqlUserDefinedType(typeof(FiscalPeriodFormatter))]
        public struct FiscalPeriod
        {
            public int Quarter { get; private set; }

            public int Month { get; private set; }

            public FiscalPeriod(int quarter, int month):this()
            {
                this.Quarter = quarter;
                this.Month = month;
            }

            public override bool Equals(object obj)
            {
                if (ReferenceEquals(null, obj))
                {
                    return false;
                }

                return obj is FiscalPeriod && Equals((FiscalPeriod)obj);
            }

            public bool Equals(FiscalPeriod other)
            {
return this.Quarter.Equals(other.Quarter) &&    this.Month.Equals(other.Month);
            }

            public bool GreaterThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) > 0 || this.Month.CompareTo(other.Month) > 0;
            }

            public bool LessThan(FiscalPeriod other)
            {
return this.Quarter.CompareTo(other.Quarter) < 0 || this.Month.CompareTo(other.Month) < 0;
            }

            public override int GetHashCode()
            {
                unchecked
                {
                    return (this.Quarter.GetHashCode() * 397) ^ this.Month.GetHashCode();
                }
            }

            public static FiscalPeriod operator +(FiscalPeriod c1, FiscalPeriod c2)
            {
return new FiscalPeriod((c1.Quarter + c2.Quarter) > 4 ? (c1.Quarter + c2.Quarter)-4 : (c1.Quarter + c2.Quarter), (c1.Month + c2.Month) > 12 ? (c1.Month + c2.Month) - 12 : (c1.Month + c2.Month));
            }

            public static bool operator ==(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.Equals(c2);
            }

            public static bool operator !=(FiscalPeriod c1, FiscalPeriod c2)
            {
                return !c1.Equals(c2);
            }
            public static bool operator >(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.GreaterThan(c2);
            }
            public static bool operator <(FiscalPeriod c1, FiscalPeriod c2)
            {
                return c1.LessThan(c2);
            }
            public override string ToString()
            {
                return (String.Format("Q{0}:P{1}", this.Quarter, this.Month));
            }

        }

        public class FiscalPeriodFormatter : IFormatter<FiscalPeriod>
        {
public void Serialize(FiscalPeriod instance, IColumnWriter writer, ISerializationContext context)
            {
                using (var binaryWriter = new BinaryWriter(writer.BaseStream))
                {
                    binaryWriter.Write(instance.Quarter);
                    binaryWriter.Write(instance.Month);
                    binaryWriter.Flush();
                }
            }

public FiscalPeriod Deserialize(IColumnReader reader, ISerializationContext context)
            {
                using (var binaryReader = new BinaryReader(reader.BaseStream))
                {
var result = new FiscalPeriod(binaryReader.ReadInt16(), binaryReader.ReadInt16());
                    return result;
                }
            }
        }
    }
}
```

## <a name="use-user-defined-aggregates-udagg"></a>Použití uživatelem definovaných agregacích: UDAGG
Uživatelem definované agregace jsou funkce související s agregace, které jsou součástí není out-of-the-box se U-SQL. Příklad Hello může být agregační tooperform vlastní matematické výpočty, zřetězení řetězců manipulace s řetězci a tak dále.

Definice uživatelem definované agregace základní třídy Hello vypadá takto:

```c#
    [SqlUserDefinedAggregate]
    public abstract class IAggregate<T1, T2, TResult> : IAggregate
    {
        protected IAggregate();

        public abstract void Accumulate(T1 t1, T2 t2);
        public abstract void Init();
        public abstract TResult Terminate();
    }
```

**SqlUserDefinedAggregate** označuje, že hello typ měli být registrován jako uživatelem definovaná agregace. Tato třída nelze dědí.

Atribut SqlUserDefinedType je **volitelné** pro UDAGG definici.


Hello základní třída umožňuje toopass tři abstraktní parametry: dvě jako vstupní parametry a jako výsledek hello. Hello datové typy jsou proměnné a by měl být definován během dědičnosti tříd.

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    { … }

    public override void Accumulate(string guid, string user)
    { … }

    public override string Terminate()
    { … }
}
```

* **Init –** vyvolá jednou pro každou skupinu během výpočtu. Poskytuje rutiny inicializace pro každou skupinu agregace.  
* **Accumulate** se spustí jednou pro každou hodnotu. Hlavní funkce hello poskytuje pro algoritmus agregace hello. Může být hodnoty používané tooaggregate s různými typy dat, které jsou definovány během dědičnosti tříd. Může přijímat dva parametry proměnné datové typy.
* **Ukončit** se spustí jednou na skupinu agregace na konci hello zpracování toooutput hello výsledek pro každou skupinu.

toodeclare správný vstupní a výstupní datové typy, použijte definici třídy hello následujícím způsobem:

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* T1: První parametr tooaccumulate
* T2: První parametr tooaccumulate
* TResult: Návratový typ ukončit

Například:

```
public class GuidAggregate : IAggregate<string, int, int>
```

nebo

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a>Použití UDAGG v U-SQL
toouse UDAGG, nejprve definování v kódu nebo odkazovat z hello svědčící programovatelnosti knihovny DLL, jak je popsáno výše.

Poté hello použijte následující syntaxi:

```
AGG<UDAGG_functionname>(param1,param2)
```

Tady je příklad UDAGG:

```
public class GuidAggregate : IAggregate<string, string, string>
{
    string guid_agg;

    public override void Init()
    {
        guid_agg = "";
    }

    public override void Accumulate(string guid, string user)
    {
        if (user.ToUpper()== "USER1")
        {
        guid_agg += "{" + guid + "}";
        }
    }

    public override string Terminate()
    {
        return guid_agg;
    }

}
```

A základní skript U-SQL:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @" \usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    SELECT
        user,
        AGG<USQL_Programmability.GuidAggregate>(guid,user) AS guid_list
    FROM @rs0
    GROUP BY user;

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

V tomto scénáři případ použití jsme řetězení třída identifikátory GUID pro konkrétní uživatele hello.

## <a name="use-user-defined-objects-udo"></a>Uživatelem definované objekty použít: UDO
U-SQL umožňuje toodefine programovatelnosti vlastní objekty, které se nazývají uživatelem definované objekty nebo UDO.

Hello následuje seznam UDO v U-SQL:

* Uživatelem definované extraktory
    * Extrakce řádek po řádku
    * Použít tooimplement extrakce dat z vlastních strukturovaných souborů

* Uživatelem definované outputters
    * Výstup po řádcích
    * Použít vlastní datové typy toooutput nebo vlastních formátů souborů

* Uživatelem definované procesorů
    * Vezme jeden řádek a vytvoří jeden řádek
    * Použít tooreduce hello počet sloupců nebo vytvořit nové sloupce s hodnotami, které jsou odvozené z existující sady sloupců

* Uživatelem definované appliers
    * Vezme jeden řádek a vytvoří 0 toon řádků
    * Použít s vnější/mezi použít

* Uživatelem definované combiners
    * Kombinuje sady řádků – uživatelem definované spojení

* Uživatelem definované přechodky
    * Vezme n řádků a vytvoří jeden řádek
    * Použít tooreduce hello počet řádků

UDO se obvykle nazývá explicitně ve skriptu U-SQL v rámci hello následující příkazy U-SQL:

* EXTRAKCE
* VÝSTUP
* PROCES
* KOMBINOVÁNÍ
* SNÍŽENÍ

> [!NOTE]  
> UDO na jsou omezené tooconsume 0,5 Gb paměti.  Toto omezení paměti nevztahuje toolocal spuštěních.

## <a name="use-user-defined-extractors"></a>Použití uživatelem definované extraktory
U-SQL umožňuje tooimport externích dat pomocí příkazu EXTRAKCE. Příkaz EXTRAKCE můžete použít předdefinované extraktory UDO:  

* *Extractors.Text()*: poskytuje extrakci z textových souborů s oddělovači různá kódování.

* *Extractors.Csv()*: poskytuje extrakci z hodnot oddělených čárkami (CSV) soubory různá kódování.

* *Extractors.Tsv()*: poskytuje extrakci z hodnoty oddělené tabulátory (TSV) soubory různá kódování.

Může být užitečné toodevelop vlastní Extraktor. To může být užitečné při importu dat pokud chceme, aby toodo žádné hello následující úlohy:

* Upravte vstupní data tak, že rozdělení sloupců a změnou jednotlivé hodnoty. Hello funkcionality procesoru je lepší pro kombinace sloupců.
* Analyzovat Nestrukturovaná data, jako jsou webové stránky a e-mailů nebo polovičním nestrukturovaných dat, jako je například XML nebo JSON.
* Analyzovat data v kódování nepodporovaný.

toodefine Extraktor uživatelem definované nebo OUČIT, potřebujeme toocreate `IExtractor` rozhraní. Všechny vstupní Extraktor toohello parametry, například sloupce či řádku oddělovače a kódování, třeba toobe definované v hello konstruktoru třídy hello. Hello `IExtractor` rozhraní musí také obsahovat definice pro hello `IEnumerable<IRow>` přepsat následujícím způsobem:

```
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

Hello **SqlUserDefinedExtractor** atribut uvádí, které hello typ měli být registrován jako Extraktor se definovaný uživatelem. Tato třída nelze dědí.

SqlUserDefinedExtractor je volitelný atribut pro OUČIT definici. Použije toodefine AtomicFileProcessing vlastností pro objekt OUČIT hello.

* BOOL AtomicFileProcessing   

* **Hodnota TRUE,** = označuje, že tento Extraktor vyžaduje atomic vstupní soubory (JSON, XML,...)
* **false** = označuje, že tento Extraktor můžete řešit rozdělení / distribuované soubory (sdíleného svazku clusteru, SEQ,...)

Hello hlavní OUČIT programovatelnosti objekty jsou **vstupní** a **výstup**. vstupní objekt Hello je použité tooenumerate vstupní data jako `IUnstructuredReader`. objekt výstup Hello je použité tooset výstupní data v důsledku hello Extraktor aktivity.

vstupní data Hello je přístupné přes `System.IO.Stream` a `System.IO.StreamReader`.

Pro vstupní sloupce výčet jsme nejprve rozdělení hello vstupního datového proudu pomocí oddělovač řádků.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Další rozdělte pak vstupní řádek na části sloupce.

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

tooset výstupní data, používáme hello `output.Set` metoda.

Je důležité, že toounderstand, který hello vlastní Extraktor pouze výstupy sloupců a hodnot, které jsou definovány výstup hello. Nastavit volání metody.

```
output.Set<string>(count, part);
```

skutečné Extraktor výstup Hello se aktivuje při volání metody `yield return output.AsReadOnly();`.

Následuje příklad Extraktor hello:

```
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read hello input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split hello input by hello column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert tooUPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep hello rest of hello columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

V tomto scénáři případ použití hello Extraktor regeneruje hello identifikátor GUID pro sloupec "guid" a převádí hodnoty hello "user" sloupec tooupper případu. Vlastní extraktory může vytvářet složitější výsledky analýzou vstupní data a manipulaci.

Toto je základní skript U-SQL, která používá vlastní Extraktor:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 too@output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a>Použít outputters uživatelem definované
Uživatelem definované outputter je jiný UDO U-SQL, který vám umožní tooextend integrovanou funkci U-SQL. Podobně jako toohello Extraktor existuje několik předdefinovaných outputters.

* *Outputters.Text()*: zapíše data toodelimited textové soubory různá kódování.
* *Outputters.Csv()*: zapíše data toocomma s oddělovači (CSV) soubory různá kódování.
* *Outputters.Tsv()*: zapíše data tootab s oddělovači (TSV) soubory různá kódování.

Vlastní outputter umožňuje toowrite data ve vlastním formátu definované. To může být užitečné pro hello následující úlohy:

* Zápis dat toosemi strukturovaných nebo nestrukturovaných soubory.
* Zápis dat není podporováno kódování.
* Úprava výstupní data nebo přidání vlastních atributů.

uživatelem definované outputter toodefine, potřebujeme toocreate hello `IOutputter` rozhraní.

Toto je základní hello `IOutputter` implementaci třídy:

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Všechny vstupní parametry toohello outputter, jako jsou sloupce či řádku oddělovače, kódování a tak dále, musí toobe definované v hello konstruktoru třídy hello. Hello `IOutputter` rozhraní musí také obsahovat definice pro `void Output` přepsat. atribut Hello `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` můžete volitelně můžete nastavit pro zpracování atomic souboru. Další informace najdete v tématu hello následujících podrobnostech.

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* `Output`je volána pro každý řádek vstupu. Vrátí hello `IUnstructuredWriter output` sady řádků.
* Hello konstruktor třída se používá toopass parametry toohello uživatelem definované outputter.
* `Close`se používá toooptionally přepsat toorelease nákladné stavu nebo určit, kdy byla zapsána hello poslední řádek.

**SqlUserDefinedOutputter** atribut uvádí, které hello typ měli být registrován jako outputter se definovaný uživatelem. Tato třída nelze dědí.

SqlUserDefinedOutputter je volitelný atribut pro definici outputter definovaný uživatelem. Použije se toodefine hello AtomicFileProcessing vlastnost.

* BOOL AtomicFileProcessing   

* **Hodnota TRUE,** = označuje, že tento outputter vyžaduje atomic výstupní soubory (JSON, XML,...)
* **false** = označuje, že tento outputter můžete řešit rozdělení / distribuované soubory (sdíleného svazku clusteru, SEQ,...)

Hello hlavního programovatelnosti objekty jsou **řádek** a **výstup**. Hello **řádek** objekt je použité tooenumerate výstupní data jako `IRow` rozhraní. **Výstup** je použité tooset výstupní data toohello cílový soubor.

Hello výstupní data přistupuje prostřednictvím hello `IRow` rozhraní. Výstupní data předávána řádek najednou.

jednotlivé hodnoty Hello se nachází voláním metody Get hello hello IRow rozhraní:

```
row.Get<string>("column_name")
```

Názvy jednotlivých sloupců se dá určit pomocí volání `row.Schema`:

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Tento přístup umožňuje toobuild flexibilní outputter pro žádné schéma metadat.

Hello výstupní data jsou zapsána toofile pomocí `System.IO.StreamWriter`. parametr datového proudu Hello je nastaven příliš`output.BaseStrea` jako součást `IUnstructuredWriter output`.

Všimněte si, že je důležité tooflush hello dat vyrovnávací paměti toohello soubor po každé iteraci řádek. Kromě toho hello `StreamWriter` objekt musí použít s hello na jedno použití atributu povoleno (výchozí) a hello **pomocí** – klíčové slovo:

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

Jinak volejte metodu Flush() explicitně po každé iteraci. Nemůžeme zobrazit v hello následující ukázka.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Nastavte záhlaví a zápatí pro uživatelem definované outputter
tooset záhlaví, použijte jednu iterace provádění toku.

```
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

nejprve Hello kód v hello `if (isHeaderRow)` bloku se spustí jenom jednou.

Pro zápatí hello použít instanci toohello odkaz hello `System.IO.Stream` objektu (`output.BaseStream`). Zapisovat hello zápatí v hello Close() metodu hello `IOutputter` rozhraní.  (Další informace najdete v tématu hello následující ukázka.)

Následuje příklad uživatelem definované outputter:

```
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // hello Close method is used toowrite hello footer toohello file. It's executed only once, after all rows
    public override void Close().
    {
    //Reference tooIO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization tooenumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required toomatch hello distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference toohello instance of hello IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define hello factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

A základní skript U-SQL:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    too@output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Toto je outputter HTML, který vytvoří soubor HTML s dat v tabulce.

### <a name="call-outputter-from-u-sql-base-script"></a>Volání outputter ze základní skriptu U-SQL
toocall vlastní outputter z hello základní skript U-SQL, hello novou instanci objektu outputter hello má toobe vytvořili.

```sql
OUTPUT @rs0 too@output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Vytvoření instance hello tooavoid objekt v základní skriptu, můžeme vytvořit funkce obálku, jak je znázorněno v našem příkladu starší:

```c#
        // Define hello factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

V takovém případě původní volání hello vypadá hello následující:

```
OUTPUT @rs0 
too@output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a>Používají procesory uživatelem definované
Uživatelem definované procesoru nebo UDP, je typ UDO U-SQL, která vám umožní příchozí řádky tooprocess hello použitím funkcí programovatelnosti. UDP vám umožní toocombine sloupců, upravte hodnoty a v případě potřeby přidejte nové sloupce. V podstatě pomáhá tooprocess sady řádků tooproduce požadované datové prvky.

toodefine UDP, potřebujeme toocreate `IProcessor` rozhraní s hello `SqlUserDefinedProcessor` atribut, který je pro UDP volitelné.

Toto rozhraní by měl obsahovat definici hello hello `IRow` rozhraní řádků přepsat, jak je znázorněno v hello následující ukázka:

```
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**SqlUserDefinedProcessor** Určuje, které hello typ měli být registrován jako procesor definovaný uživatelem. Tato třída nelze dědí.

atribut SqlUserDefinedProcessor Hello je **volitelné** pro definici UDP.

Hello hlavního programovatelnosti objekty jsou **vstupní** a **výstup**. vstupní objekt Hello je vstupní sloupce použité tooenumerate a výstup a výstupní data tooset v důsledku činnosti procesoru hello.

Pro vstupní sloupce výčet používáme hello `input.Get` metoda.

```
string column_name = input.Get<string>("column_name");
```

Hello parametr pro `input.Get` metoda je sloupec, který se předá jako součást hello `PRODUCE` klauzule hello `PROCESS` příkaz skriptu základní hello U-SQL. Potřebujeme toouse sem zadejte hello správná data.

Pro výstup, použijte hello `output.Set` metoda.

Je důležité toonote tento vlastní producent je výstupy pouze, sloupce a hodnoty, které jsou definovány s hello `output.Set` volání metody.

```
output.Set<string>("mycolumn", mycolumn);
```

výstup Hello skutečné procesoru se aktivuje při volání metody `return output.AsReadOnly();`.

Následuje příklad procesoru:

```
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

V tomto scénáři případ použití hello procesoru generuje nový sloupec s názvem "full_description" kombinací hello existující sloupce – v tomto případě "user" velká a "des". Také znovu vytvoří identifikátor GUID a vrátí hodnot hello původní a nový identifikátor GUID.

Jak vidíte z předchozího příkladu hello, můžete volat metody C# během `output.Set` volání metody.

Tady je příklad základní skriptu U-SQL, který používá vlastního procesoru:

```
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a>Použít appliers uživatelem definované
Uživatelem definované applier U-SQL umožňuje funkce tooinvoke vlastní C# pro každý řádek, který je vrácen hello vnější tabulky výraz dotazu. pravé vstup Hello je vyhodnotit pro každý řádek z levého vstup hello a hello řádky, které vytváří spojují pro výsledný výstup hello. Hello seznam sloupců, které jsou vytvářeny v operátor APPLY hello jsou hello kombinací hello sadu sloupců v hello v levém horním a hello právo vstup.

Uživatelem definované applier je volaná jako součást hello USQL vyberte výraz.

Typické Hello volání toohello uživatelem definované applier zdá hello následující:

```
SELECT …
FROM …
CROSS APPLYis used toopass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Další informace o používání appliers ve výrazu SELECT, najdete v části [U-SQL vyberte výběr z křížové použít a vnější použít](https://msdn.microsoft.com/library/azure/mt621307.aspx).

Hello uživatelem definované applier základní definice třídy vypadá takto:

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

toodefine applier se definovaný uživatelem, potřebujeme toocreate hello `IApplier` rozhraní s hello [`SqlUserDefinedApplier`] atribut, který je pro uživatelem definované applier definice volitelné.

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* Použít je volána pro každý řádek tabulky vnější hello. Vrátí hello `IUpdatableRow` výstup sady řádků.
* Třída konstruktor Hello je použité toopass parametry toohello uživatelem definované applier.

**SqlUserDefinedApplier** Určuje, které hello typ měli být registrován jako applier se definovaný uživatelem. Tato třída nelze dědí.

**SqlUserDefinedApplier** je **volitelné** pro definici applier definovaný uživatelem.


Hello hlavního programovatelnosti objekty jsou následující:

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Vstupní sady řádků se předávají jako `IRow` vstupní. Hello výstup řádky jsou generovány jako `IUpdatableRow` rozhraní výstup.

Názvy jednotlivých sloupců se dá určit pomocí volání hello `IRow` metoda schématu.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

tooget hello skutečná data hodnoty z hello příchozí `IRow`, jsme použijte metodu Get() hello `IRow` rozhraní.

```
mycolumn = row.Get<int>("mycolumn")
```

Nebo používáme název sloupce hello schématu:

```
row.Get<int>(row.Schema[0].Name)
```

Hello výstupní hodnoty musí být nastavena s `IUpdatableRow` výstup:

```
output.Set<int>("mycolumn", mycolumn)
```

Je důležité toounderstand vlastní appliers pouze výstup sloupců a hodnot, které jsou definovány s `output.Set` volání metody.

aktuální výstup Hello se aktivuje při volání metody `yield return output.AsReadOnly();`.

Hello uživatelem definované applier parametry lze předat toohello konstruktor. Applier může vrátit proměnný počet sloupců, které je třeba toobe definované během hello applier volání v základní skript U-SQL.

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Tady je příklad applier uživatelem definované hello:

```
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

Toto je hello základní skript U-SQL pro tento uživatelem definované applier:

```
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 too@output_file USING Outputters.Text();
```

V tento scénář případu využití, uživatelem definované applier jednání jako analyzátor oddělených čárkou pro hello car loďstva vlastnosti. řádky vstupní soubor Hello vypadat hello následující:

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Je typické oddělený tabulátory TSV soubor s vlastností sloupec, který obsahuje vlastnosti car například výrobce a model. Tyto vlastnosti musí být Analyzovaná toohello sloupců tabulky. Hello applier, který je k dispozici také umožňuje toogenerate dynamické počet vlastností v hello způsobit řádků, na základě hello parametru, který je předán. Můžete vygenerovat buď všechny vlastnosti, nebo konkrétní sadu pouze vlastnosti.

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

uživatelem definované applier Hello lze volat jako novou instanci třídy applier objektu:

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

Nebo pomocí hello volání metody vytváření obálky:

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a>Použít combiners uživatelem definované
Uživatelem definované kombinační nebo UDC, umožňuje toocombine řádky z levé a pravé sady řádků, na základě vlastní logiky. Uživatelem definované kombinační se používá s KOMBINAČNÍ výraz.

Výraz hello kombinovaného ÚTVARU, který poskytuje hello nezbytné informace o obou hello vstupní sady řádků se vyvolání kombinační, hello seskupení sloupců, hello očekávaný výsledek schématu a další informace.

toocall kombinační v základní skript U-SQL, můžeme použít hello následující syntaxi:

```
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Další informace najdete v tématu [KOMBINOVAT výrazu (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).

toodefine kombinační se definovaný uživatelem, potřebujeme toocreate hello `ICombiner` rozhraní s hello [`SqlUserDefinedCombiner`] atribut, který je pro definici uživatelem definované kombinační volitelné.

Základní `ICombiner` definici třídy:

```
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

vlastní implementace Hello `ICombiner` rozhraní by měl obsahovat definici hello pro `IEnumerable<IRow>` kombinovat přepsání.

```
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

Hello **SqlUserDefinedCombiner** atribut uvádí, které hello typ měli být registrován jako kombinační se definovaný uživatelem. Tato třída nelze dědí.

**SqlUserDefinedCombiner** je vlastnost režimu kombinační použité toodefine hello. Je volitelný atribut pro definici kombinační definovaný uživatelem.

Režim CombinerMode

Výčet CombinerMode může trvat hello následující hodnoty:

* Úplné (0), každý řádek výstupu potenciálně závisí na všechny hello vstupní řádky z levé a pravé s hello stejnou hodnotu klíče.

* Každý řádek výstupu závisí na jednom řádku vstupní zleva hello doleva (1) (a potenciálně všechny řádky z hello přímo s hello stejnou hodnotu klíče).

* Každý řádek výstupu závisí na jeden vstupní řádek z hello správné vpravo (2) (a potenciálně všechny řádky z levé hello s hello stejnou hodnotu klíče).

* Vnitřní (3), každý řádek výstupu závisí na jeden vstup řádek z levé a pravé s hello stejnou hodnotu.

Příklad: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


Hello hlavního programovatelnosti objekty jsou:

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Vstupní sady řádků se předávají jako **levém** a **správné** `IRowset` typu rozhraní. Obě sady řádků musí být ve výčtu objevit pro zpracování. Každé rozhraní můžete pouze výčet jednou, takže jsme tooenumerate a uložení do mezipaměti v případě potřeby.

Pro ukládání do mezipaměti pro účely, můžeme vytvořit seznam\<T\> typ konstrukce paměti v důsledku LINQ spuštění dotazu, konkrétně seznam <`IRow`>. Hello anonymní datový typ můžete použít během výčtu také.

V tématu [Úvod tooLINQ dotazů (C#)](https://msdn.microsoft.com/library/bb397906.aspx) Další informace o dotazech LINQ a [rozhraní IEnumerable\<T\> rozhraní](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) Další informace o rozhraní IEnumerable\<T\> rozhraní.

tooget hello skutečná data hodnoty z hello příchozí `IRowset`, jsme použijte metodu Get() hello `IRow` rozhraní.

```
mycolumn = row.Get<int>("mycolumn")
```

Názvy jednotlivých sloupců se dá určit pomocí volání hello `IRow` metoda schématu.

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Nebo pomocí názvu sloupce hello schématu:

```
c# row.Get<int>(row.Schema[0].Name)
```

Obecné – výčet Hello s dotazy LINQ vypadá hello následující:

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Po vytvoření výčtu obě sady řádků, přidáme tooloop prostřednictvím všechny řádky. Pro každý řádek v levém řádků hello přidáme toofind všechny řádky, které splňují podmínku hello naše kombinační.

Hello výstupní hodnoty musí být nastavena s `IUpdatableRow` výstup.

```
output.Set<int>("mycolumn", mycolumn)
```

aktuální výstup Hello se aktivuje při volání metody příliš`yield return output.AsReadOnly();`.

Následuje příklad kombinační:

```
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

V tomto scénáři případ použití jsme se vytváření zprávu o analýzy pro hello prodejce. cílem Hello je toofind všechny produkty, které náklady více než 20 000 $ a že prodeje prostřednictvím webu hello rychlejší než prostřednictvím prodejce regulární hello v určitém časovém rámci.

Zde je hello základní skript U-SQL. Můžete porovnat hello logiku mezi regulárního spojení a kombinační:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 too@output_file1 USING Outputters.Tsv();
OUTPUT @rs2 too@output_file2 USING Outputters.Tsv();
```

Uživatelem definované kombinační lze volat jako novou instanci objektu applier hello:

```
USING new MyNameSpace.MyCombiner();
```


Nebo pomocí hello volání metody vytváření obálky:

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a>Použít přechodky uživatelem definované

U-SQL umožňuje toowrite přechodky vlastní řádků v jazyce C# s využitím rozhraní rozšiřitelnosti uživatelem definovaný operátor hello a implementace rozhraní IReducer.

Uživatelem definované reduktorem nebo UDR, můžou být použité tooeliminate nepotřebné řádků při extrakci dat (Importovat). Také můžete být použité toomanipulate a vyhodnotit řádků a sloupců. Na základě programovatelnosti logiky, ho můžete také definovat řádky potřebovat toobe extrahovat.

toodefine třídu UDR, potřebujeme toocreate `IReducer` rozhraní s volitelný `SqlUserDefinedReducer` atribut.

Tato třída rozhraní by měl obsahovat definici hello `IEnumerable` přepsat rozhraní sady řádků.

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

Hello **SqlUserDefinedReducer** atribut uvádí, které hello typ měli být registrován jako reduktorem se definovaný uživatelem. Tato třída nelze dědí.
**SqlUserDefinedReducer** je volitelný atribut pro definici reduktorem definovaný uživatelem. Použije se toodefine IsRecursive vlastnost.

* BOOL IsRecursive    
* **Hodnota TRUE,** = označuje, zda je tento reduktorem idempotent

Hello hlavního programovatelnosti objekty jsou **vstupní** a **výstup**. vstupní objekt Hello je použité tooenumerate vstupní řádky. Výstup je použité tooset výstup řádků v důsledku omezení aktivity.

Pro vstupní řádky výčet používáme hello `Row.Get` metoda.

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

parametr pro hello Hello `Row.Get` metoda je sloupec, který se předá jako součást hello `PRODUCE` třídu hello `REDUCE` příkaz skriptu základní hello U-SQL. Potřebujeme toouse hello správného datového typu zde také.

Pro výstup, použijte hello `output.Set` metoda.

Je důležité toounderstand, který vlastní reduktorem pouze výstupy hodnoty, které jsou definovány s hello `output.Set` volání metody.

```
output.Set<string>("mycolumn", guid);
```

skutečné reduktorem výstup Hello se aktivuje při volání metody `yield return output.AsReadOnly();`.

Následuje příklad reduktorem:

```
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

V tomto scénáři případ použití hello reduktorem přeskakuje řádky s prázdné jméno. Pro každý řádek v sadě řádků přečte každý požadovaný sloupec, následně vyhodnocuje hello délka hello uživatelské jméno. Skutečný řádek hello vyprodukuje pouze v případě, že hodnota délka jména uživatele je větší než 0.

Toto je základní skript U-SQL, která používá vlastní reduktorem:

```
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    too@output_file 
    USING Outputters.Text();
```
