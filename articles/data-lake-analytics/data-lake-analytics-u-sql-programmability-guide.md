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
# <a name="u-sql-programmability-guide"></a><span data-ttu-id="c5c05-103">Průvodce programovatelnosti U-SQL</span><span class="sxs-lookup"><span data-stu-id="c5c05-103">U-SQL programmability guide</span></span>

<span data-ttu-id="c5c05-104">U-SQL je dotazovací jazyk, který je určený pro velkých datových typů úloh.</span><span class="sxs-lookup"><span data-stu-id="c5c05-104">U-SQL is a query language that's designed for big data-type of workloads.</span></span> <span data-ttu-id="c5c05-105">Jednou z funkcí hello jedinečné jazykem U-SQL je kombinací hello hello deklarativní jazyka SQL jako s hello rozšiřitelnosti a programovatelnosti, který je zadán v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="c5c05-105">One of hello unique features of U-SQL is hello combination of hello SQL-like declarative language with hello extensibility and programmability that's provided by C#.</span></span> <span data-ttu-id="c5c05-106">V této příručce budeme soustředit na hello rozšiřitelnosti a programovatelnosti hello jazyka U-SQL, který je povolený v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="c5c05-106">In this guide, we concentrate on hello extensibility and programmability of hello U-SQL language that's enabled by C#.</span></span>

## <a name="requirements"></a><span data-ttu-id="c5c05-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c5c05-107">Requirements</span></span>

<span data-ttu-id="c5c05-108">Stáhněte a nainstalujte [nástrojů Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="c5c05-108">Download and install [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="get-started-with-u-sql"></a><span data-ttu-id="c5c05-109">Začínáme s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="c5c05-109">Get started with U-SQL</span></span>  

<span data-ttu-id="c5c05-110">Podívejme se na hello následující skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="c5c05-110">Let’s look at hello following U-SQL script:</span></span>

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

<span data-ttu-id="c5c05-111">Definuje sadu řádků názvem @a a vytvoří sadu řádků názvem @results z @a.</span><span class="sxs-lookup"><span data-stu-id="c5c05-111">It defines a RowSet called @a and creates a RowSet called @results from @a.</span></span>

## <a name="c-types-and-expressions-in-u-sql-script"></a><span data-ttu-id="c5c05-112">Typy C# a výrazy ve skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="c5c05-112">C# types and expressions in U-SQL script</span></span>

<span data-ttu-id="c5c05-113">Výraz U-SQL je výraz jazyka C# v kombinaci s logických operací U-SQL, `AND`, `OR`, a `NOT`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-113">A U-SQL Expression is a C# expression combined with U-SQL logical operations such `AND`, `OR`, and `NOT`.</span></span> <span data-ttu-id="c5c05-114">Výrazy U-SQL lze použít s příkazem SELECT, EXTRAHOVÁNÍ, kde s, Seskupit podle a DEKLAROVAT.</span><span class="sxs-lookup"><span data-stu-id="c5c05-114">U-SQL Expressions can be used with SELECT, EXTRACT, WHERE, HAVING, GROUP BY and DECLARE.</span></span>

<span data-ttu-id="c5c05-115">Například následující skript hello analyzuje řetězec hodnotu data a času v klauzuli SELECT hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-115">For example, hello following script parses a string a DateTime value in hello SELECT clause.</span></span>

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

<span data-ttu-id="c5c05-116">Hello následující skript analyzuje řetězec hodnotu data a času v příkazu DECLARE.</span><span class="sxs-lookup"><span data-stu-id="c5c05-116">hello following script parses a string a DateTime value in a DECLARE statement.</span></span>

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a><span data-ttu-id="c5c05-117">Použití jazyka C# výrazů pro konverze datových typů</span><span class="sxs-lookup"><span data-stu-id="c5c05-117">Use C# expressions for data type conversions</span></span>
<span data-ttu-id="c5c05-118">Hello následující příklad ukazuje, jak můžete provést převod dat data a času pomocí výrazy jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="c5c05-118">hello following example demonstrates how you can do a datetime data conversion by using C# expressions.</span></span> <span data-ttu-id="c5c05-119">V tomto scénáři konkrétní data řetězce data a času je převeden toostandard datetime s půlnoc 00:00:00 čas zápis.</span><span class="sxs-lookup"><span data-stu-id="c5c05-119">In this particular scenario, string datetime data is converted toostandard datetime with midnight 00:00:00 time notation.</span></span>

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 too@output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a><span data-ttu-id="c5c05-120">Použití jazyka C# výrazů pro dnešní datum</span><span class="sxs-lookup"><span data-stu-id="c5c05-120">Use C# expressions for today’s date</span></span>
<span data-ttu-id="c5c05-121">toopull dnešní datum, můžeme použít hello následující výraz jazyka C#:</span><span class="sxs-lookup"><span data-stu-id="c5c05-121">toopull today’s date, we can use hello following C# expression:</span></span>

```
DateTime.Now.ToString("M/d/yyyy")
```

<span data-ttu-id="c5c05-122">Tady je příklad toho, jak toouse tento výraz ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="c5c05-122">Here's an example of how toouse this expression in a script:</span></span>

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



## <a name="using-net-assemblies"></a><span data-ttu-id="c5c05-123">Použití sestavení rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c5c05-123">Using .NET assemblies</span></span>
<span data-ttu-id="c5c05-124">Model rozšiřitelnosti U-SQL založena na hello možnost tooadd vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="c5c05-124">U-SQL’s extensibility model relies heavily on hello ability tooadd custom code.</span></span> <span data-ttu-id="c5c05-125">V současné době U-SQL umožňuje snadno způsoby tooadd vlastní Microsoft. Na základě NET kód (zejména, C#).</span><span class="sxs-lookup"><span data-stu-id="c5c05-125">Currently, U-SQL provides you with easy ways tooadd your own Microsoft .NET-based code (in particular, C#).</span></span> <span data-ttu-id="c5c05-126">Můžete však také přidat vlastní kód, který je zapsán do jiných jazyků .NET, například VB.NET nebo F #.</span><span class="sxs-lookup"><span data-stu-id="c5c05-126">However, you can also add custom code that's written in other .NET languages, such as VB.NET or F#.</span></span> 

### <a name="register-a-net-assembly"></a><span data-ttu-id="c5c05-127">Registrace sestavení rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c5c05-127">Register a .NET assembly</span></span>

<span data-ttu-id="c5c05-128">Použijte tooplace příkaz CREATE ASSEMBLY hello sestavení .NET do databáze U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-128">Use hello CREATE ASSEMBLY statement tooplace a .NET assembly into a U-SQL Database.</span></span> <span data-ttu-id="c5c05-129">Po sestavení je v databázi, skriptů U-SQL můžete použít tyto sestavení pomocí příkazu referenční sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-129">Once an assembly is in a database, U-SQL scripts can use those assemblies by using hello REFERENCE ASSEMBLY statement.</span></span> 

<span data-ttu-id="c5c05-130">Následující kód ukazuje, jak Hello tooregister sestavení:</span><span class="sxs-lookup"><span data-stu-id="c5c05-130">hello following code shows how tooregister an assembly:</span></span>

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

<span data-ttu-id="c5c05-131">Následující kód ukazuje, jak Hello tooreference sestavení:</span><span class="sxs-lookup"><span data-stu-id="c5c05-131">hello following code shows how tooreference an assembly:</span></span>

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

<span data-ttu-id="c5c05-132">Poraďte se hello [pokyny pro registraci sestavení](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) , která se vztahuje toto téma podrobněji.</span><span class="sxs-lookup"><span data-stu-id="c5c05-132">Consult hello [assembly registration instructions](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) that covers this topic in greater detail.</span></span>


### <a name="use-assembly-versioning"></a><span data-ttu-id="c5c05-133">Pomocí správy verzí sestavení</span><span class="sxs-lookup"><span data-stu-id="c5c05-133">Use assembly versioning</span></span>
<span data-ttu-id="c5c05-134">U-SQL v současné době používá hello rozhraní .NET Framework verze 4.5.</span><span class="sxs-lookup"><span data-stu-id="c5c05-134">Currently, U-SQL uses hello .NET Framework version 4.5.</span></span> <span data-ttu-id="c5c05-135">Proto zajistěte, aby byly kompatibilní s danou verzi modulu runtime hello vlastního sestavení.</span><span class="sxs-lookup"><span data-stu-id="c5c05-135">So ensure that your own assemblies are compatible with that version of hello runtime.</span></span>

<span data-ttu-id="c5c05-136">Jak už bylo zmíněno dříve, spustí kód U-SQL ve formátu 64bitovou (x 64).</span><span class="sxs-lookup"><span data-stu-id="c5c05-136">As mentioned earlier, U-SQL runs code in a 64-bit (x64) format.</span></span> <span data-ttu-id="c5c05-137">Proto se ujistěte, že jste kód napsali kompilované toorun na x64.</span><span class="sxs-lookup"><span data-stu-id="c5c05-137">So make sure that your code is compiled toorun on x64.</span></span> <span data-ttu-id="c5c05-138">V opačném případě se zobrazí chyba správný formát hello uvedena výše.</span><span class="sxs-lookup"><span data-stu-id="c5c05-138">Otherwise you get hello incorrect format error shown earlier.</span></span>

<span data-ttu-id="c5c05-139">Každý odeslán sestavení knihoven DLL a souboru prostředků, jako je jiný modul runtime, nativní sestavení nebo konfiguračním souboru může být maximálně 400 MB.</span><span class="sxs-lookup"><span data-stu-id="c5c05-139">Each uploaded assembly DLL and resource file, such as a different runtime, a native assembly, or a config file, can be at most 400 MB.</span></span> <span data-ttu-id="c5c05-140">Celková velikost Hello nasazené prostředky, prostřednictvím nasazení prostředků nebo prostřednictvím odkazů tooassemblies a další soubory, nesmí překročit 3 GB.</span><span class="sxs-lookup"><span data-stu-id="c5c05-140">hello total size of deployed resources, either via DEPLOY RESOURCE or via references tooassemblies and their additional files, cannot exceed 3 GB.</span></span>

<span data-ttu-id="c5c05-141">Nakonec si všimněte, že každý U-SQL databáze může obsahovat pouze jednu verzi žádné zadaného sestavení.</span><span class="sxs-lookup"><span data-stu-id="c5c05-141">Finally, note that each U-SQL database can only contain one version of any given assembly.</span></span> <span data-ttu-id="c5c05-142">Například pokud budete potřebovat verze 7 a 8 verzi hello knihovně NewtonSoft Json.Net, je třeba tooregister je ve dvou různých databází.</span><span class="sxs-lookup"><span data-stu-id="c5c05-142">For example, if you need both version 7 and version 8 of hello NewtonSoft Json.Net library, you need tooregister them in two different databases.</span></span> <span data-ttu-id="c5c05-143">Kromě toho každý skript se může vztahovat pouze tooone verzi zadaného sestavení knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-143">Furthermore, each script can only refer tooone version of a given assembly DLL.</span></span> <span data-ttu-id="c5c05-144">V tomto ohledu následuje U-SQL hello C# sestavení správy a správa verzí sémantiku.</span><span class="sxs-lookup"><span data-stu-id="c5c05-144">In this respect, U-SQL follows hello C# assembly management and versioning semantics.</span></span>


## <a name="use-user-defined-functions-udf"></a><span data-ttu-id="c5c05-145">Použití uživatelem definované funkce: UDF</span><span class="sxs-lookup"><span data-stu-id="c5c05-145">Use user-defined functions: UDF</span></span>
<span data-ttu-id="c5c05-146">Uživatelem definované funkce U-SQL nebo UDF, jsou programovacích rutin, které přijímají parametry, provedení akce (například komplexní výpočet) a hello v důsledku této akce jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-146">U-SQL user-defined functions, or UDF, are programming routines that accept parameters, perform an action (such as a complex calculation), and return hello result of that action as a value.</span></span> <span data-ttu-id="c5c05-147">Hello vrátit hodnotu UDF může mít pouze jednu skalární hodnota.</span><span class="sxs-lookup"><span data-stu-id="c5c05-147">hello return value of UDF can only be a single scalar.</span></span> <span data-ttu-id="c5c05-148">UDF U-SQL je možné volat v základní skript U-SQL jako všechny ostatní C# skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-148">U-SQL UDF can be called in U-SQL base script like any other C# scalar function.</span></span>

<span data-ttu-id="c5c05-149">Doporučujeme, abyste inicializaci U-SQL uživatelsky definované funkce jako **veřejné** a **statické**.</span><span class="sxs-lookup"><span data-stu-id="c5c05-149">We recommend that you initialize U-SQL user-defined functions as **public** and **static**.</span></span>

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

<span data-ttu-id="c5c05-150">První Podíváme se na hello jednoduchý příklad vytvoření uživatelem definovanou FUNKCI.</span><span class="sxs-lookup"><span data-stu-id="c5c05-150">First let’s look at hello simple example of creating a UDF.</span></span>

<span data-ttu-id="c5c05-151">V tomto scénáři případ použití potřebujeme fiskální období hello toodetermine, včetně hello fiskální čtvrtletí a fiskální měsíc hello první přihlášení pro konkrétního uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-151">In this use-case scenario, we need toodetermine hello fiscal period, including hello fiscal quarter and fiscal month of hello first sign-in for hello specific user.</span></span> <span data-ttu-id="c5c05-152">Hello první měsíc fiskálního roku hello v našem scénáři je června.</span><span class="sxs-lookup"><span data-stu-id="c5c05-152">hello first fiscal month of hello year in our scenario is June.</span></span>

<span data-ttu-id="c5c05-153">fiskální období toocalculate, zavedeme hello následující funkce jazyka C#:</span><span class="sxs-lookup"><span data-stu-id="c5c05-153">toocalculate fiscal period, we introduce hello following C# function:</span></span>

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

<span data-ttu-id="c5c05-154">Jednoduše vypočítá fiskální měsíc a čtvrtletí a vrátí hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-154">It simply calculates fiscal month and quarter and returns a string value.</span></span> <span data-ttu-id="c5c05-155">Pro června, hello první měsíc hello první fiskální čtvrtletí, použijeme "Q1:P1".</span><span class="sxs-lookup"><span data-stu-id="c5c05-155">For June, hello first month of hello first fiscal quarter, we use "Q1:P1".</span></span> <span data-ttu-id="c5c05-156">Pro červenec můžeme použít "Q1:P2" a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c5c05-156">For July, we use "Q1:P2", and so on.</span></span>

<span data-ttu-id="c5c05-157">Toto je normální funkce jazyka C# s Snažíme se probíhající toouse v našem projekt U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-157">This is a regular C# function that we are going toouse in our U-SQL project.</span></span>

<span data-ttu-id="c5c05-158">Zde je, jak vypadá hello kódu části v tomto scénáři:</span><span class="sxs-lookup"><span data-stu-id="c5c05-158">Here is how hello code-behind section looks in this scenario:</span></span>

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

<span data-ttu-id="c5c05-159">Teď se zaměříme toocall tuto funkci z hello základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-159">Now we are going toocall this function from hello base U-SQL script.</span></span> <span data-ttu-id="c5c05-160">toodo, máme tooprovide plně kvalifikovaný název hello funkce, včetně hello oboru názvů, který je v tomto případě NameSpace.Class.Function(parameter).</span><span class="sxs-lookup"><span data-stu-id="c5c05-160">toodo this, we have tooprovide a fully qualified name for hello function, including hello namespace, which in this case is NameSpace.Class.Function(parameter).</span></span>

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

<span data-ttu-id="c5c05-161">Toto je základní skript skutečné U-SQL hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-161">Following is hello actual U-SQL base script:</span></span>

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

<span data-ttu-id="c5c05-162">Toto je výstupní soubor hello spuštění skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-162">Following is hello output file of hello script execution:</span></span>

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

<span data-ttu-id="c5c05-163">Tento příklad ukazuje jednoduchý využití vložené UDF v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-163">This example demonstrates a simple usage of inline UDF in U-SQL.</span></span>

### <a name="keep-state-between-udf-invocations"></a><span data-ttu-id="c5c05-164">Zachovat stav mezi UDF volání</span><span class="sxs-lookup"><span data-stu-id="c5c05-164">Keep state between UDF invocations</span></span>
<span data-ttu-id="c5c05-165">U-SQL C# programovatelnosti objekty může být více pokročilé, využitím interaktivity prostřednictvím hello kódu globální proměnné.</span><span class="sxs-lookup"><span data-stu-id="c5c05-165">U-SQL C# programmability objects can be more sophisticated, utilizing interactivity through hello code-behind global variables.</span></span> <span data-ttu-id="c5c05-166">Podívejme se na následující scénář případu použití obchodní hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-166">Let’s look at hello following business use-case scenario.</span></span>

<span data-ttu-id="c5c05-167">Ve velkých organizacích mohou uživatelé přepínat mezi typy interních aplikací.</span><span class="sxs-lookup"><span data-stu-id="c5c05-167">In large organizations, users can switch between varieties of internal applications.</span></span> <span data-ttu-id="c5c05-168">Může jít o Microsoft Dynamics CRM, PowerBI a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c5c05-168">These can include Microsoft Dynamics CRM, PowerBI, and so on.</span></span> <span data-ttu-id="c5c05-169">Zákazníci může být vhodné tooapply analýzu telemetrii jak uživatelé přepínat mezi různými aplikacemi, jaké hello využití trendů jsou a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c5c05-169">Customers might want tooapply a telemetry analysis of how users switch between different applications, what hello usage trends are, and so on.</span></span> <span data-ttu-id="c5c05-170">cílem Hello pro firmy hello je toooptimize využití aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-170">hello goal for hello business is toooptimize application usage.</span></span> <span data-ttu-id="c5c05-171">Také může chtějí toocombine různé aplikace nebo konkrétní rutiny přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c5c05-171">They also might want toocombine different applications or specific sign-on routines.</span></span>

<span data-ttu-id="c5c05-172">tooachieve tento cíl máme toodetermine ID relace a prodleva mezi hello poslední relace, který došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="c5c05-172">tooachieve this goal, we have toodetermine session IDs and lag time between hello last session that occurred.</span></span>

<span data-ttu-id="c5c05-173">Budeme potřebovat toofind předchozí přihlášení a pak mu přiřaďte toto přihlášení tooall relací, které probíhá generovaného toohello stejná aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-173">We need toofind a previous sign-in and then assign this sign-in tooall sessions that are being generated toohello same application.</span></span> <span data-ttu-id="c5c05-174">Hello první výzva je, že základní skript U-SQL nepovoluje nám tooapply výpočty přes již počítaného sloupce se funkce LAG.</span><span class="sxs-lookup"><span data-stu-id="c5c05-174">hello first challenge is that U-SQL base script doesn't allow us tooapply calculations over already-calculated columns with LAG function.</span></span> <span data-ttu-id="c5c05-175">Hello Druhá výzva je, že jsme tookeep hello konkrétní relace pro všechny relace v rámci hello stejné časové období.</span><span class="sxs-lookup"><span data-stu-id="c5c05-175">hello second challenge is that we have tookeep hello specific session for all sessions within hello same time period.</span></span>

<span data-ttu-id="c5c05-176">toosolve tento problém jsme použít globální proměnné uvnitř části kódu: `static public string globalSession;`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-176">toosolve this problem, we use a global variable inside a code-behind section: `static public string globalSession;`.</span></span>

<span data-ttu-id="c5c05-177">Tato globální proměnná je použité toohello celý řádků při našich provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-177">This global variable is applied toohello entire rowset during our script execution.</span></span>

<span data-ttu-id="c5c05-178">Tady je části kódu hello naše programu U-SQL:</span><span class="sxs-lookup"><span data-stu-id="c5c05-178">Here is hello code-behind section of our U-SQL program:</span></span>

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

<span data-ttu-id="c5c05-179">Tento příklad ukazuje hello – globální proměnná `static public string globalSession;` použít uvnitř hello `getStampUserSession` funkce a získávání inicializace každý čas hello relace parametr je změnit.</span><span class="sxs-lookup"><span data-stu-id="c5c05-179">This example shows hello global variable `static public string globalSession;` used inside hello `getStampUserSession` function and getting reinitialized each time hello Session parameter is changed.</span></span>

<span data-ttu-id="c5c05-180">Hello základní skript U-SQL je následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-180">hello U-SQL base script is as follows:</span></span>

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

<span data-ttu-id="c5c05-181">Funkce `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` je zde volána v průběhu hello druhý paměti řádků výpočtu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-181">Function `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` is called here during hello second memory rowset calculation.</span></span> <span data-ttu-id="c5c05-182">Pak předá hello `UserSessionTimestamp` sloupce a vrátí hodnotu, dokud hello `UserSessionTimestamp` došlo ke změně.</span><span class="sxs-lookup"><span data-stu-id="c5c05-182">It passes hello `UserSessionTimestamp` column and returns hello value until `UserSessionTimestamp` has changed.</span></span>

<span data-ttu-id="c5c05-183">výstupní soubor Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c5c05-183">hello output file is as follows:</span></span>

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

<span data-ttu-id="c5c05-184">Tento příklad ukazuje složitější scénář případu použití používáme globální proměnné uvnitř oddíl kódu, který je použité toohello celý paměti řádků.</span><span class="sxs-lookup"><span data-stu-id="c5c05-184">This example demonstrates a more complicated use-case scenario in which we use a global variable inside a code-behind section that's applied toohello entire memory rowset.</span></span>

## <a name="use-user-defined-types-udt"></a><span data-ttu-id="c5c05-185">Uživatelem definované typy použití: UDT</span><span class="sxs-lookup"><span data-stu-id="c5c05-185">Use user-defined types: UDT</span></span>
<span data-ttu-id="c5c05-186">Uživatelem definované typy nebo UDT, je jiná funkcí programovatelnosti U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-186">User-defined types, or UDT, is another programmability feature of U-SQL.</span></span> <span data-ttu-id="c5c05-187">U-SQL UDT funguje jako regulární C# definovaný uživatelem typem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-187">U-SQL UDT acts like a regular C# user-defined type.</span></span> <span data-ttu-id="c5c05-188">C# je jazyk silného typu, který umožňuje použití hello předdefinované a vlastní uživatelem definované typy.</span><span class="sxs-lookup"><span data-stu-id="c5c05-188">C# is a strongly typed language that allows hello use of built-in and custom user-defined types.</span></span>

<span data-ttu-id="c5c05-189">U-SQL nelze implicitně serializaci nebo libovolný uživatelsky definovaný typ deserializovat, když hello UDT předána mezi vrcholy v sady řádků.</span><span class="sxs-lookup"><span data-stu-id="c5c05-189">U-SQL cannot implicitly serialize or de-serialize arbitrary UDTs when hello UDT is passed between vertices in rowsets.</span></span> <span data-ttu-id="c5c05-190">To znamená, že tento uživatel hello má tooprovide explicitní formátování s použitím rozhraní IFormatter hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-190">This means that hello user has tooprovide an explicit formatter by using hello IFormatter interface.</span></span> <span data-ttu-id="c5c05-191">To poskytuje U-SQL s hello serializovat a deserializovat metody pro hello UDT.</span><span class="sxs-lookup"><span data-stu-id="c5c05-191">This provides U-SQL with hello serialize and de-serialize methods for hello UDT.</span></span>

> [!NOTE]
> <span data-ttu-id="c5c05-192">Předdefinované extraktory U-SQL a outputters aktuálně nelze serializovat nebo deserializovat UDT tooor dat ze souborů i hello IFormatter sady.</span><span class="sxs-lookup"><span data-stu-id="c5c05-192">U-SQL’s built-in extractors and outputters currently cannot serialize or de-serialize UDT data tooor from files even with hello IFormatter set.</span></span> <span data-ttu-id="c5c05-193">Takže když píšete UDT data tooa soubor s hello výstup příkazu, nebo jeho čtení s extrakci, máte toopass ji jako řetězec nebo bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="c5c05-193">So when you're writing UDT data tooa file with hello OUTPUT statement, or reading it with an extractor, you have toopass it as a string or byte array.</span></span> <span data-ttu-id="c5c05-194">Potom volání hello serializace a deserializace explicitně kódu (to znamená, metodu ToString() hello UDT).</span><span class="sxs-lookup"><span data-stu-id="c5c05-194">Then you call hello serialization and deserialization code (that is, hello UDT’s ToString() method) explicitly.</span></span> <span data-ttu-id="c5c05-195">Uživatelem definované extraktory a outputters na hello jiných ruční, lze číst a zapisovat UDT.</span><span class="sxs-lookup"><span data-stu-id="c5c05-195">User-defined extractors and outputters, on hello other hand, can read and write UDTs.</span></span>

<span data-ttu-id="c5c05-196">Pokud jsme zkuste toouse UDT v EXTRAKTOR nebo OUTPUTTER (mimo předchozí vyberte), jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="c5c05-196">If we try toouse UDT in EXTRACTOR or OUTPUTTER (out of previous SELECT), as shown here:</span></span>

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    too@output_file 
    USING Outputters.Text();
```

<span data-ttu-id="c5c05-197">Nemůžeme zobrazit hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="c5c05-197">We receive hello following error:</span></span>

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

<span data-ttu-id="c5c05-198">toowork s UDT v outputter buď máme tooserialize ho toostring s hello metodu ToString() nebo vytvořit vlastní outputter.</span><span class="sxs-lookup"><span data-stu-id="c5c05-198">toowork with UDT in outputter, we either have tooserialize it toostring with hello ToString() method or create a custom outputter.</span></span>

<span data-ttu-id="c5c05-199">UDT aktuálně nelze použít v GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="c5c05-199">UDTs currently cannot be used in GROUP BY.</span></span> <span data-ttu-id="c5c05-200">Pokud UDT se používá v GROUP BY, je vyvolána hello následující chybě:</span><span class="sxs-lookup"><span data-stu-id="c5c05-200">If UDT is used in GROUP BY, hello following error is thrown:</span></span>

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

<span data-ttu-id="c5c05-201">toodefine typ definovaný uživatelem, musíme:</span><span class="sxs-lookup"><span data-stu-id="c5c05-201">toodefine a UDT, we have to:</span></span>

* <span data-ttu-id="c5c05-202">Přidejte následující obory názvů hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-202">Add hello following namespaces:</span></span>

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* <span data-ttu-id="c5c05-203">Přidat `Microsoft.Analytics.Interfaces`, což je vyžadováno pro rozhraní UDT hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-203">Add `Microsoft.Analytics.Interfaces`, which is required for hello UDT interfaces.</span></span> <span data-ttu-id="c5c05-204">Kromě toho `System.IO` mohou být potřebné toodefine hello IFormatter rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-204">In addition, `System.IO` might be needed toodefine hello IFormatter interface.</span></span>

* <span data-ttu-id="c5c05-205">Definování typu používá definované atributem SqlUserDefinedType.</span><span class="sxs-lookup"><span data-stu-id="c5c05-205">Define a used-defined type with SqlUserDefinedType attribute.</span></span>

<span data-ttu-id="c5c05-206">**SqlUserDefinedType** je použité toomark definice typu v sestavení jako uživatelem definovaný typ (UDT) v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-206">**SqlUserDefinedType** is used toomark a type definition in an assembly as a user-defined type (UDT) in U-SQL.</span></span> <span data-ttu-id="c5c05-207">Vlastnosti Hello na atribut hello odrážet fyzické vlastnosti hello UDT hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-207">hello properties on hello attribute reflect hello physical characteristics of hello UDT.</span></span> <span data-ttu-id="c5c05-208">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-208">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-209">SqlUserDefinedType je povinný atribut pro UDT definici.</span><span class="sxs-lookup"><span data-stu-id="c5c05-209">SqlUserDefinedType is a required attribute for UDT definition.</span></span>

<span data-ttu-id="c5c05-210">Hello konstruktoru třídy hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-210">hello constructor of hello class:</span></span>  

* <span data-ttu-id="c5c05-211">SqlUserDefinedTypeAttribute (formátovací modul typu)</span><span class="sxs-lookup"><span data-stu-id="c5c05-211">SqlUserDefinedTypeAttribute (type formatter)</span></span>

* <span data-ttu-id="c5c05-212">Formátovací modul typu: vyžaduje parametr toodefine formátování UDT – konkrétně hello typ hello `IFormatter` rozhraní musí být předán sem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-212">Type formatter: Required parameter toodefine an UDT formatter--specifically, hello type of hello `IFormatter` interface must be passed here.</span></span>

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* <span data-ttu-id="c5c05-213">Typické UDT taky vyžaduje definice hello IFormatter rozhraní, jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-213">Typical UDT also requires definition of hello IFormatter interface, as shown in hello following example:</span></span>

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

<span data-ttu-id="c5c05-214">Hello `IFormatter` rozhraní serializuje a poté serializuje grafu objektu s hello kořenový typ \<typeparamref name = "T" >.</span><span class="sxs-lookup"><span data-stu-id="c5c05-214">hello `IFormatter` interface serializes and de-serializes an object graph with hello root type of \<typeparamref name="T">.</span></span>

<span data-ttu-id="c5c05-215">\<typeparam name = "T" > hello kořenový typ pro hello objekt grafu tooserialize a deserializovat.</span><span class="sxs-lookup"><span data-stu-id="c5c05-215">\<typeparam name="T">hello root type for hello object graph tooserialize and de-serialize.</span></span>

* <span data-ttu-id="c5c05-216">**Deserializovat**: zrušte serializuje hello data v datovém proudu hello zadaný a reconstitutes hello grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="c5c05-216">**Deserialize**: De-serializes hello data on hello provided stream and reconstitutes hello graph of objects.</span></span>

* <span data-ttu-id="c5c05-217">**Serializovat**: serializuje objektu, nebo grafu objektů, s hello zadaný kořenový toohello zadaný datový proud.</span><span class="sxs-lookup"><span data-stu-id="c5c05-217">**Serialize**: Serializes an object, or graph of objects, with hello given root toohello provided stream.</span></span>

<span data-ttu-id="c5c05-218">`MyType`instance: Instance typu hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-218">`MyType` instance: Instance of hello type.</span></span>  
<span data-ttu-id="c5c05-219">`IColumnWriter`Zapisovač / `IColumnReader` čtečky: hello základní sloupec datového proudu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-219">`IColumnWriter` writer / `IColumnReader` reader: hello underlying column stream.</span></span>  
<span data-ttu-id="c5c05-220">`ISerializationContext`kontext: výčet, který definuje sadu příznaky, která určuje hello zdrojový nebo cílový kontext pro datový proud hello během serializace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-220">`ISerializationContext` context: Enum that defines a set of flags that specifies hello source or destination context for hello stream during serialization.</span></span>

* <span data-ttu-id="c5c05-221">**Zprostředkující**: Určuje, že hello zdrojový nebo cílový kontext není trvalého úložiště.</span><span class="sxs-lookup"><span data-stu-id="c5c05-221">**Intermediate**: Specifies that hello source or destination context is not a persisted store.</span></span>

* <span data-ttu-id="c5c05-222">**Trvalost**: Určuje, že hello zdrojové nebo cílové kontextu je trvalé úložiště.</span><span class="sxs-lookup"><span data-stu-id="c5c05-222">**Persistence**: Specifies that hello source or destination context is a persisted store.</span></span>

<span data-ttu-id="c5c05-223">Jako regulární C# typ definici UDT U-SQL můžete zahrnout přepsání pro operátory, jako +/ == /! =.</span><span class="sxs-lookup"><span data-stu-id="c5c05-223">As a regular C# type, a U-SQL UDT definition can include overrides for operators such as +/==/!=.</span></span> <span data-ttu-id="c5c05-224">Může také obsahovat statické metody.</span><span class="sxs-lookup"><span data-stu-id="c5c05-224">It can also include static methods.</span></span> <span data-ttu-id="c5c05-225">Například pokud přidáme toouse tento UDT jako parametr tooa agregační funkci MIN U-SQL, máme toodefine < operátor přepsání.</span><span class="sxs-lookup"><span data-stu-id="c5c05-225">For example, if we are going toouse this UDT as a parameter tooa U-SQL MIN aggregate function, we have toodefine < operator override.</span></span>

<span data-ttu-id="c5c05-226">Dříve v tomto průvodci jsme ukázán příklad pro fiskálním období identifikaci z hello konkrétní datum ve formátu hello Qn:Pn (Q1:P10).</span><span class="sxs-lookup"><span data-stu-id="c5c05-226">Earlier in this guide, we demonstrated an example for fiscal period identification from hello specific date in hello format Qn:Pn (Q1:P10).</span></span> <span data-ttu-id="c5c05-227">Hello následující příklad ukazuje, jak toodefine vlastní typ pro hodnoty fiskálním období.</span><span class="sxs-lookup"><span data-stu-id="c5c05-227">hello following example shows how toodefine a custom type for fiscal period values.</span></span>

<span data-ttu-id="c5c05-228">Tady je příklad oddílu kódu s vlastní UDT a IFormatter rozhraní:</span><span class="sxs-lookup"><span data-stu-id="c5c05-228">Following is an example of a code-behind section with custom UDT and IFormatter interface:</span></span>

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

<span data-ttu-id="c5c05-229">Hello definovaný typ. zahrnuje dvě čísla: čtvrtletí a měsíce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-229">hello defined type includes two numbers: quarter and month.</span></span> <span data-ttu-id="c5c05-230">Operátory == /! = / > nebo < a statickou metodu ToString() jsou definovány v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="c5c05-230">Operators ==/!=/>/< and static method ToString() are defined here.</span></span>

<span data-ttu-id="c5c05-231">Jak už bylo zmíněno dříve, můžete použít ve výrazech vyberte UDT, ale nejde použít v OUTPUTTER/EXTRAKTOR bez vlastní serializace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-231">As mentioned earlier, UDT can be used in SELECT expressions, but cannot be used in OUTPUTTER/EXTRACTOR without custom serialization.</span></span> <span data-ttu-id="c5c05-232">Buď má toobe serializován jako řetězec s ToString() nebo použít s vlastní OUTPUTTER/EXTRAKTOR.</span><span class="sxs-lookup"><span data-stu-id="c5c05-232">It either has toobe serialized as a string with ToString() or used with a custom OUTPUTTER/EXTRACTOR.</span></span>

<span data-ttu-id="c5c05-233">Nyní probereme využití UDT.</span><span class="sxs-lookup"><span data-stu-id="c5c05-233">Now let’s discuss usage of UDT.</span></span> <span data-ttu-id="c5c05-234">V části kódu jsme změnili naše GetFiscalPeriod funkce toohello následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-234">In a code-behind section, we changed our GetFiscalPeriod function toohello following:</span></span>

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

<span data-ttu-id="c5c05-235">Jak vidíte, vrátí hodnotu hello naše FiscalPeriod typu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-235">As you can see, it returns hello value of our FiscalPeriod type.</span></span>

<span data-ttu-id="c5c05-236">Zde jsme uveďte příklad jak toouse dále s ním v základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-236">Here we provide an example of how toouse it further in U-SQL base script.</span></span> <span data-ttu-id="c5c05-237">Tento příklad ukazuje různé formy UDT volání ze skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-237">This example demonstrates different forms of UDT invocation from U-SQL script.</span></span>

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

<span data-ttu-id="c5c05-238">Tady je příklad oddílu úplné kódu:</span><span class="sxs-lookup"><span data-stu-id="c5c05-238">Here's an example of a full code-behind section:</span></span>

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

## <a name="use-user-defined-aggregates-udagg"></a><span data-ttu-id="c5c05-239">Použití uživatelem definovaných agregacích: UDAGG</span><span class="sxs-lookup"><span data-stu-id="c5c05-239">Use user-defined aggregates: UDAGG</span></span>
<span data-ttu-id="c5c05-240">Uživatelem definované agregace jsou funkce související s agregace, které jsou součástí není out-of-the-box se U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-240">User-defined aggregates are any aggregation-related functions that are not shipped out-of-the-box with U-SQL.</span></span> <span data-ttu-id="c5c05-241">Příklad Hello může být agregační tooperform vlastní matematické výpočty, zřetězení řetězců manipulace s řetězci a tak dále.</span><span class="sxs-lookup"><span data-stu-id="c5c05-241">hello example can be an aggregate tooperform custom math calculations, string concatenations, manipulations with strings, and so on.</span></span>

<span data-ttu-id="c5c05-242">Definice uživatelem definované agregace základní třídy Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c5c05-242">hello user-defined aggregate base class definition is as follows:</span></span>

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

<span data-ttu-id="c5c05-243">**SqlUserDefinedAggregate** označuje, že hello typ měli být registrován jako uživatelem definovaná agregace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-243">**SqlUserDefinedAggregate** indicates that hello type should be registered as a user-defined aggregate.</span></span> <span data-ttu-id="c5c05-244">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-244">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-245">Atribut SqlUserDefinedType je **volitelné** pro UDAGG definici.</span><span class="sxs-lookup"><span data-stu-id="c5c05-245">SqlUserDefinedType attribute is **optional** for UDAGG definition.</span></span>


<span data-ttu-id="c5c05-246">Hello základní třída umožňuje toopass tři abstraktní parametry: dvě jako vstupní parametry a jako výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-246">hello base class allows you toopass three abstract parameters: two as input parameters and one as hello result.</span></span> <span data-ttu-id="c5c05-247">Hello datové typy jsou proměnné a by měl být definován během dědičnosti tříd.</span><span class="sxs-lookup"><span data-stu-id="c5c05-247">hello data types are variable and should be defined during class inheritance.</span></span>

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

* <span data-ttu-id="c5c05-248">**Init –** vyvolá jednou pro každou skupinu během výpočtu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-248">**Init** invokes once for each group during computation.</span></span> <span data-ttu-id="c5c05-249">Poskytuje rutiny inicializace pro každou skupinu agregace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-249">It provides an initialization routine for each aggregation group.</span></span>  
* <span data-ttu-id="c5c05-250">**Accumulate** se spustí jednou pro každou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-250">**Accumulate** is executed once for each value.</span></span> <span data-ttu-id="c5c05-251">Hlavní funkce hello poskytuje pro algoritmus agregace hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-251">It provides hello main functionality for hello aggregation algorithm.</span></span> <span data-ttu-id="c5c05-252">Může být hodnoty používané tooaggregate s různými typy dat, které jsou definovány během dědičnosti tříd.</span><span class="sxs-lookup"><span data-stu-id="c5c05-252">It can be used tooaggregate values with various data types that are defined during class inheritance.</span></span> <span data-ttu-id="c5c05-253">Může přijímat dva parametry proměnné datové typy.</span><span class="sxs-lookup"><span data-stu-id="c5c05-253">It can accept two parameters of variable data types.</span></span>
* <span data-ttu-id="c5c05-254">**Ukončit** se spustí jednou na skupinu agregace na konci hello zpracování toooutput hello výsledek pro každou skupinu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-254">**Terminate** is executed once per aggregation group at hello end of processing toooutput hello result for each group.</span></span>

<span data-ttu-id="c5c05-255">toodeclare správný vstupní a výstupní datové typy, použijte definici třídy hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c5c05-255">toodeclare correct input and output data types, use hello class definition as follows:</span></span>

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* <span data-ttu-id="c5c05-256">T1: První parametr tooaccumulate</span><span class="sxs-lookup"><span data-stu-id="c5c05-256">T1: First parameter tooaccumulate</span></span>
* <span data-ttu-id="c5c05-257">T2: První parametr tooaccumulate</span><span class="sxs-lookup"><span data-stu-id="c5c05-257">T2: First parameter tooaccumulate</span></span>
* <span data-ttu-id="c5c05-258">TResult: Návratový typ ukončit</span><span class="sxs-lookup"><span data-stu-id="c5c05-258">TResult: Return type of terminate</span></span>

<span data-ttu-id="c5c05-259">Například:</span><span class="sxs-lookup"><span data-stu-id="c5c05-259">For example:</span></span>

```
public class GuidAggregate : IAggregate<string, int, int>
```

<span data-ttu-id="c5c05-260">nebo</span><span class="sxs-lookup"><span data-stu-id="c5c05-260">or</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a><span data-ttu-id="c5c05-261">Použití UDAGG v U-SQL</span><span class="sxs-lookup"><span data-stu-id="c5c05-261">Use UDAGG in U-SQL</span></span>
<span data-ttu-id="c5c05-262">toouse UDAGG, nejprve definování v kódu nebo odkazovat z hello svědčící programovatelnosti knihovny DLL, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="c5c05-262">toouse UDAGG, first define it in code-behind or reference it from hello existent programmability DLL as discussed earlier.</span></span>

<span data-ttu-id="c5c05-263">Poté hello použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="c5c05-263">Then use hello following syntax:</span></span>

```
AGG<UDAGG_functionname>(param1,param2)
```

<span data-ttu-id="c5c05-264">Tady je příklad UDAGG:</span><span class="sxs-lookup"><span data-stu-id="c5c05-264">Here is an example of UDAGG:</span></span>

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

<span data-ttu-id="c5c05-265">A základní skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="c5c05-265">And base U-SQL script:</span></span>

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

<span data-ttu-id="c5c05-266">V tomto scénáři případ použití jsme řetězení třída identifikátory GUID pro konkrétní uživatele hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-266">In this use-case scenario, we concatenate class GUIDs for hello specific users.</span></span>

## <a name="use-user-defined-objects-udo"></a><span data-ttu-id="c5c05-267">Uživatelem definované objekty použít: UDO</span><span class="sxs-lookup"><span data-stu-id="c5c05-267">Use user-defined objects: UDO</span></span>
<span data-ttu-id="c5c05-268">U-SQL umožňuje toodefine programovatelnosti vlastní objekty, které se nazývají uživatelem definované objekty nebo UDO.</span><span class="sxs-lookup"><span data-stu-id="c5c05-268">U-SQL enables you toodefine custom programmability objects, which are called user-defined objects or UDO.</span></span>

<span data-ttu-id="c5c05-269">Hello následuje seznam UDO v U-SQL:</span><span class="sxs-lookup"><span data-stu-id="c5c05-269">hello following is a list of UDO in U-SQL:</span></span>

* <span data-ttu-id="c5c05-270">Uživatelem definované extraktory</span><span class="sxs-lookup"><span data-stu-id="c5c05-270">User-defined extractors</span></span>
    * <span data-ttu-id="c5c05-271">Extrakce řádek po řádku</span><span class="sxs-lookup"><span data-stu-id="c5c05-271">Extract row by row</span></span>
    * <span data-ttu-id="c5c05-272">Použít tooimplement extrakce dat z vlastních strukturovaných souborů</span><span class="sxs-lookup"><span data-stu-id="c5c05-272">Used tooimplement data extraction from custom structured files</span></span>

* <span data-ttu-id="c5c05-273">Uživatelem definované outputters</span><span class="sxs-lookup"><span data-stu-id="c5c05-273">User-defined outputters</span></span>
    * <span data-ttu-id="c5c05-274">Výstup po řádcích</span><span class="sxs-lookup"><span data-stu-id="c5c05-274">Output row by row</span></span>
    * <span data-ttu-id="c5c05-275">Použít vlastní datové typy toooutput nebo vlastních formátů souborů</span><span class="sxs-lookup"><span data-stu-id="c5c05-275">Used toooutput custom data types or custom file formats</span></span>

* <span data-ttu-id="c5c05-276">Uživatelem definované procesorů</span><span class="sxs-lookup"><span data-stu-id="c5c05-276">User-defined processors</span></span>
    * <span data-ttu-id="c5c05-277">Vezme jeden řádek a vytvoří jeden řádek</span><span class="sxs-lookup"><span data-stu-id="c5c05-277">Take one row and produce one row</span></span>
    * <span data-ttu-id="c5c05-278">Použít tooreduce hello počet sloupců nebo vytvořit nové sloupce s hodnotami, které jsou odvozené z existující sady sloupců</span><span class="sxs-lookup"><span data-stu-id="c5c05-278">Used tooreduce hello number of columns or produce new columns with values that are derived from an existing column set</span></span>

* <span data-ttu-id="c5c05-279">Uživatelem definované appliers</span><span class="sxs-lookup"><span data-stu-id="c5c05-279">User-defined appliers</span></span>
    * <span data-ttu-id="c5c05-280">Vezme jeden řádek a vytvoří 0 toon řádků</span><span class="sxs-lookup"><span data-stu-id="c5c05-280">Take one row and produce 0 toon rows</span></span>
    * <span data-ttu-id="c5c05-281">Použít s vnější/mezi použít</span><span class="sxs-lookup"><span data-stu-id="c5c05-281">Used with OUTER/CROSS APPLY</span></span>

* <span data-ttu-id="c5c05-282">Uživatelem definované combiners</span><span class="sxs-lookup"><span data-stu-id="c5c05-282">User-defined combiners</span></span>
    * <span data-ttu-id="c5c05-283">Kombinuje sady řádků – uživatelem definované spojení</span><span class="sxs-lookup"><span data-stu-id="c5c05-283">Combines rowsets--user-defined JOINs</span></span>

* <span data-ttu-id="c5c05-284">Uživatelem definované přechodky</span><span class="sxs-lookup"><span data-stu-id="c5c05-284">User-defined reducers</span></span>
    * <span data-ttu-id="c5c05-285">Vezme n řádků a vytvoří jeden řádek</span><span class="sxs-lookup"><span data-stu-id="c5c05-285">Take n rows and produce one row</span></span>
    * <span data-ttu-id="c5c05-286">Použít tooreduce hello počet řádků</span><span class="sxs-lookup"><span data-stu-id="c5c05-286">Used tooreduce hello number of rows</span></span>

<span data-ttu-id="c5c05-287">UDO se obvykle nazývá explicitně ve skriptu U-SQL v rámci hello následující příkazy U-SQL:</span><span class="sxs-lookup"><span data-stu-id="c5c05-287">UDO is typically called explicitly in U-SQL script as part of hello following U-SQL statements:</span></span>

* <span data-ttu-id="c5c05-288">EXTRAKCE</span><span class="sxs-lookup"><span data-stu-id="c5c05-288">EXTRACT</span></span>
* <span data-ttu-id="c5c05-289">VÝSTUP</span><span class="sxs-lookup"><span data-stu-id="c5c05-289">OUTPUT</span></span>
* <span data-ttu-id="c5c05-290">PROCES</span><span class="sxs-lookup"><span data-stu-id="c5c05-290">PROCESS</span></span>
* <span data-ttu-id="c5c05-291">KOMBINOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="c5c05-291">COMBINE</span></span>
* <span data-ttu-id="c5c05-292">SNÍŽENÍ</span><span class="sxs-lookup"><span data-stu-id="c5c05-292">REDUCE</span></span>

> [!NOTE]  
> <span data-ttu-id="c5c05-293">UDO na jsou omezené tooconsume 0,5 Gb paměti.</span><span class="sxs-lookup"><span data-stu-id="c5c05-293">UDO’s are limited tooconsume 0.5Gb memory.</span></span>  <span data-ttu-id="c5c05-294">Toto omezení paměti nevztahuje toolocal spuštěních.</span><span class="sxs-lookup"><span data-stu-id="c5c05-294">This memory limitation does not apply toolocal executions.</span></span>

## <a name="use-user-defined-extractors"></a><span data-ttu-id="c5c05-295">Použití uživatelem definované extraktory</span><span class="sxs-lookup"><span data-stu-id="c5c05-295">Use user-defined extractors</span></span>
<span data-ttu-id="c5c05-296">U-SQL umožňuje tooimport externích dat pomocí příkazu EXTRAKCE.</span><span class="sxs-lookup"><span data-stu-id="c5c05-296">U-SQL allows you tooimport external data by using an EXTRACT statement.</span></span> <span data-ttu-id="c5c05-297">Příkaz EXTRAKCE můžete použít předdefinované extraktory UDO:</span><span class="sxs-lookup"><span data-stu-id="c5c05-297">An EXTRACT statement can use built-in UDO extractors:</span></span>  

* <span data-ttu-id="c5c05-298">*Extractors.Text()*: poskytuje extrakci z textových souborů s oddělovači různá kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-298">*Extractors.Text()*: Provides extraction from delimited text files of different encodings.</span></span>

* <span data-ttu-id="c5c05-299">*Extractors.Csv()*: poskytuje extrakci z hodnot oddělených čárkami (CSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-299">*Extractors.Csv()*: Provides extraction from comma-separated value (CSV) files of different encodings.</span></span>

* <span data-ttu-id="c5c05-300">*Extractors.Tsv()*: poskytuje extrakci z hodnoty oddělené tabulátory (TSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-300">*Extractors.Tsv()*: Provides extraction from tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="c5c05-301">Může být užitečné toodevelop vlastní Extraktor.</span><span class="sxs-lookup"><span data-stu-id="c5c05-301">It can be useful toodevelop a custom extractor.</span></span> <span data-ttu-id="c5c05-302">To může být užitečné při importu dat pokud chceme, aby toodo žádné hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c5c05-302">This can be helpful during data import if we want toodo any of hello following tasks:</span></span>

* <span data-ttu-id="c5c05-303">Upravte vstupní data tak, že rozdělení sloupců a změnou jednotlivé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c5c05-303">Modify input data by splitting columns and modifying individual values.</span></span> <span data-ttu-id="c5c05-304">Hello funkcionality procesoru je lepší pro kombinace sloupců.</span><span class="sxs-lookup"><span data-stu-id="c5c05-304">hello PROCESSOR functionality is better for combining columns.</span></span>
* <span data-ttu-id="c5c05-305">Analyzovat Nestrukturovaná data, jako jsou webové stránky a e-mailů nebo polovičním nestrukturovaných dat, jako je například XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="c5c05-305">Parse unstructured data such as Web pages and emails, or semi-unstructured data such as XML/JSON.</span></span>
* <span data-ttu-id="c5c05-306">Analyzovat data v kódování nepodporovaný.</span><span class="sxs-lookup"><span data-stu-id="c5c05-306">Parse data in unsupported encoding.</span></span>

<span data-ttu-id="c5c05-307">toodefine Extraktor uživatelem definované nebo OUČIT, potřebujeme toocreate `IExtractor` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-307">toodefine a user-defined extractor, or UDE, we need toocreate an `IExtractor` interface.</span></span> <span data-ttu-id="c5c05-308">Všechny vstupní Extraktor toohello parametry, například sloupce či řádku oddělovače a kódování, třeba toobe definované v hello konstruktoru třídy hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-308">All input parameters toohello extractor, such as column/row delimiters, and encoding, need toobe defined in hello constructor of hello class.</span></span> <span data-ttu-id="c5c05-309">Hello `IExtractor` rozhraní musí také obsahovat definice pro hello `IEnumerable<IRow>` přepsat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c5c05-309">hello `IExtractor`  interface should also contain a definition for hello `IEnumerable<IRow>` override as follows:</span></span>

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

<span data-ttu-id="c5c05-310">Hello **SqlUserDefinedExtractor** atribut uvádí, které hello typ měli být registrován jako Extraktor se definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-310">hello **SqlUserDefinedExtractor** attribute indicates that hello type should be registered as a user-defined extractor.</span></span> <span data-ttu-id="c5c05-311">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-311">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-312">SqlUserDefinedExtractor je volitelný atribut pro OUČIT definici.</span><span class="sxs-lookup"><span data-stu-id="c5c05-312">SqlUserDefinedExtractor is an optional attribute for UDE definition.</span></span> <span data-ttu-id="c5c05-313">Použije toodefine AtomicFileProcessing vlastností pro objekt OUČIT hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-313">It used toodefine AtomicFileProcessing property for hello UDE object.</span></span>

* <span data-ttu-id="c5c05-314">BOOL AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="c5c05-314">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="c5c05-315">**Hodnota TRUE,** = označuje, že tento Extraktor vyžaduje atomic vstupní soubory (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="c5c05-315">**true** = Indicates that this extractor requires atomic input files (JSON, XML, ...)</span></span>
* <span data-ttu-id="c5c05-316">**false** = označuje, že tento Extraktor můžete řešit rozdělení / distribuované soubory (sdíleného svazku clusteru, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="c5c05-316">**false** = Indicates that this extractor can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="c5c05-317">Hello hlavní OUČIT programovatelnosti objekty jsou **vstupní** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="c5c05-317">hello main UDE programmability objects are **input** and **output**.</span></span> <span data-ttu-id="c5c05-318">vstupní objekt Hello je použité tooenumerate vstupní data jako `IUnstructuredReader`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-318">hello input object is used tooenumerate input data as `IUnstructuredReader`.</span></span> <span data-ttu-id="c5c05-319">objekt výstup Hello je použité tooset výstupní data v důsledku hello Extraktor aktivity.</span><span class="sxs-lookup"><span data-stu-id="c5c05-319">hello output object is used tooset output data as a result of hello extractor activity.</span></span>

<span data-ttu-id="c5c05-320">vstupní data Hello je přístupné přes `System.IO.Stream` a `System.IO.StreamReader`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-320">hello input data is accessed through `System.IO.Stream` and `System.IO.StreamReader`.</span></span>

<span data-ttu-id="c5c05-321">Pro vstupní sloupce výčet jsme nejprve rozdělení hello vstupního datového proudu pomocí oddělovač řádků.</span><span class="sxs-lookup"><span data-stu-id="c5c05-321">For input columns enumeration, we first split hello input stream by using a row delimiter.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

<span data-ttu-id="c5c05-322">Další rozdělte pak vstupní řádek na části sloupce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-322">Then, further split input row into column parts.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

<span data-ttu-id="c5c05-323">tooset výstupní data, používáme hello `output.Set` metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c05-323">tooset output data, we use hello `output.Set` method.</span></span>

<span data-ttu-id="c5c05-324">Je důležité, že toounderstand, který hello vlastní Extraktor pouze výstupy sloupců a hodnot, které jsou definovány výstup hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-324">It's important toounderstand that hello custom extractor only outputs columns and values that are defined with hello output.</span></span> <span data-ttu-id="c5c05-325">Nastavit volání metody.</span><span class="sxs-lookup"><span data-stu-id="c5c05-325">Set method call.</span></span>

```
output.Set<string>(count, part);
```

<span data-ttu-id="c5c05-326">skutečné Extraktor výstup Hello se aktivuje při volání metody `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-326">hello actual extractor output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="c5c05-327">Následuje příklad Extraktor hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-327">Following is hello extractor example:</span></span>

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

<span data-ttu-id="c5c05-328">V tomto scénáři případ použití hello Extraktor regeneruje hello identifikátor GUID pro sloupec "guid" a převádí hodnoty hello "user" sloupec tooupper případu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-328">In this use-case scenario, hello extractor regenerates hello GUID for “guid” column and converts hello values of “user” column tooupper case.</span></span> <span data-ttu-id="c5c05-329">Vlastní extraktory může vytvářet složitější výsledky analýzou vstupní data a manipulaci.</span><span class="sxs-lookup"><span data-stu-id="c5c05-329">Custom extractors can produce more complicated results by parsing input data and manipulating it.</span></span>

<span data-ttu-id="c5c05-330">Toto je základní skript U-SQL, která používá vlastní Extraktor:</span><span class="sxs-lookup"><span data-stu-id="c5c05-330">Following is base U-SQL script that uses a custom extractor:</span></span>

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

## <a name="use-user-defined-outputters"></a><span data-ttu-id="c5c05-331">Použít outputters uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="c5c05-331">Use user-defined outputters</span></span>
<span data-ttu-id="c5c05-332">Uživatelem definované outputter je jiný UDO U-SQL, který vám umožní tooextend integrovanou funkci U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-332">User-defined outputter is another U-SQL UDO that allows you tooextend built-in U-SQL functionality.</span></span> <span data-ttu-id="c5c05-333">Podobně jako toohello Extraktor existuje několik předdefinovaných outputters.</span><span class="sxs-lookup"><span data-stu-id="c5c05-333">Similar toohello extractor, there are several built-in outputters.</span></span>

* <span data-ttu-id="c5c05-334">*Outputters.Text()*: zapíše data toodelimited textové soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-334">*Outputters.Text()*: Writes data toodelimited text files of different encodings.</span></span>
* <span data-ttu-id="c5c05-335">*Outputters.Csv()*: zapíše data toocomma s oddělovači (CSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-335">*Outputters.Csv()*: Writes data toocomma-separated value (CSV) files of different encodings.</span></span>
* <span data-ttu-id="c5c05-336">*Outputters.Tsv()*: zapíše data tootab s oddělovači (TSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-336">*Outputters.Tsv()*: Writes data tootab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="c5c05-337">Vlastní outputter umožňuje toowrite data ve vlastním formátu definované.</span><span class="sxs-lookup"><span data-stu-id="c5c05-337">Custom outputter allows you toowrite data in a custom defined format.</span></span> <span data-ttu-id="c5c05-338">To může být užitečné pro hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c5c05-338">This can be useful for hello following tasks:</span></span>

* <span data-ttu-id="c5c05-339">Zápis dat toosemi strukturovaných nebo nestrukturovaných soubory.</span><span class="sxs-lookup"><span data-stu-id="c5c05-339">Writing data toosemi-structured or unstructured files.</span></span>
* <span data-ttu-id="c5c05-340">Zápis dat není podporováno kódování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-340">Writing data not supported encodings.</span></span>
* <span data-ttu-id="c5c05-341">Úprava výstupní data nebo přidání vlastních atributů.</span><span class="sxs-lookup"><span data-stu-id="c5c05-341">Modifying output data or adding custom attributes.</span></span>

<span data-ttu-id="c5c05-342">uživatelem definované outputter toodefine, potřebujeme toocreate hello `IOutputter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-342">toodefine user-defined outputter, we need toocreate hello `IOutputter` interface.</span></span>

<span data-ttu-id="c5c05-343">Toto je základní hello `IOutputter` implementaci třídy:</span><span class="sxs-lookup"><span data-stu-id="c5c05-343">Following is hello base `IOutputter` class implementation:</span></span>

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

<span data-ttu-id="c5c05-344">Všechny vstupní parametry toohello outputter, jako jsou sloupce či řádku oddělovače, kódování a tak dále, musí toobe definované v hello konstruktoru třídy hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-344">All input parameters toohello outputter, such as column/row delimiters, encoding, and so on, need toobe defined in hello constructor of hello class.</span></span> <span data-ttu-id="c5c05-345">Hello `IOutputter` rozhraní musí také obsahovat definice pro `void Output` přepsat.</span><span class="sxs-lookup"><span data-stu-id="c5c05-345">hello `IOutputter` interface should also contain a definition for `void Output` override.</span></span> <span data-ttu-id="c5c05-346">atribut Hello `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` můžete volitelně můžete nastavit pro zpracování atomic souboru.</span><span class="sxs-lookup"><span data-stu-id="c5c05-346">hello attribute `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` can optionally be set for atomic file processing.</span></span> <span data-ttu-id="c5c05-347">Další informace najdete v tématu hello následujících podrobnostech.</span><span class="sxs-lookup"><span data-stu-id="c5c05-347">For more information, see hello following details.</span></span>

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

* <span data-ttu-id="c5c05-348">`Output`je volána pro každý řádek vstupu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-348">`Output` is called for each input row.</span></span> <span data-ttu-id="c5c05-349">Vrátí hello `IUnstructuredWriter output` sady řádků.</span><span class="sxs-lookup"><span data-stu-id="c5c05-349">It returns hello `IUnstructuredWriter output` rowset.</span></span>
* <span data-ttu-id="c5c05-350">Hello konstruktor třída se používá toopass parametry toohello uživatelem definované outputter.</span><span class="sxs-lookup"><span data-stu-id="c5c05-350">hello Constructor class is used toopass parameters toohello user-defined outputter.</span></span>
* <span data-ttu-id="c5c05-351">`Close`se používá toooptionally přepsat toorelease nákladné stavu nebo určit, kdy byla zapsána hello poslední řádek.</span><span class="sxs-lookup"><span data-stu-id="c5c05-351">`Close` is used toooptionally override toorelease expensive state or determine when hello last row was written.</span></span>

<span data-ttu-id="c5c05-352">**SqlUserDefinedOutputter** atribut uvádí, které hello typ měli být registrován jako outputter se definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-352">**SqlUserDefinedOutputter** attribute indicates that hello type should be registered as a user-defined outputter.</span></span> <span data-ttu-id="c5c05-353">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-353">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-354">SqlUserDefinedOutputter je volitelný atribut pro definici outputter definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-354">SqlUserDefinedOutputter is an optional attribute for a user-defined outputter definition.</span></span> <span data-ttu-id="c5c05-355">Použije se toodefine hello AtomicFileProcessing vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c5c05-355">It's used toodefine hello AtomicFileProcessing property.</span></span>

* <span data-ttu-id="c5c05-356">BOOL AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="c5c05-356">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="c5c05-357">**Hodnota TRUE,** = označuje, že tento outputter vyžaduje atomic výstupní soubory (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="c5c05-357">**true** = Indicates that this outputter requires atomic output files (JSON, XML, ...)</span></span>
* <span data-ttu-id="c5c05-358">**false** = označuje, že tento outputter můžete řešit rozdělení / distribuované soubory (sdíleného svazku clusteru, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="c5c05-358">**false** = Indicates that this outputter can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="c5c05-359">Hello hlavního programovatelnosti objekty jsou **řádek** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="c5c05-359">hello main programmability objects are **row** and **output**.</span></span> <span data-ttu-id="c5c05-360">Hello **řádek** objekt je použité tooenumerate výstupní data jako `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-360">hello **row** object is used tooenumerate output data as `IRow` interface.</span></span> <span data-ttu-id="c5c05-361">**Výstup** je použité tooset výstupní data toohello cílový soubor.</span><span class="sxs-lookup"><span data-stu-id="c5c05-361">**Output** is used tooset output data toohello target file.</span></span>

<span data-ttu-id="c5c05-362">Hello výstupní data přistupuje prostřednictvím hello `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-362">hello output data is accessed through hello `IRow` interface.</span></span> <span data-ttu-id="c5c05-363">Výstupní data předávána řádek najednou.</span><span class="sxs-lookup"><span data-stu-id="c5c05-363">Output data is passed a row at a time.</span></span>

<span data-ttu-id="c5c05-364">jednotlivé hodnoty Hello se nachází voláním metody Get hello hello IRow rozhraní:</span><span class="sxs-lookup"><span data-stu-id="c5c05-364">hello individual values are enumerated by calling hello Get method of hello IRow interface:</span></span>

```
row.Get<string>("column_name")
```

<span data-ttu-id="c5c05-365">Názvy jednotlivých sloupců se dá určit pomocí volání `row.Schema`:</span><span class="sxs-lookup"><span data-stu-id="c5c05-365">Individual column names can be determined by calling `row.Schema`:</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="c5c05-366">Tento přístup umožňuje toobuild flexibilní outputter pro žádné schéma metadat.</span><span class="sxs-lookup"><span data-stu-id="c5c05-366">This approach enables you toobuild a flexible outputter for any metadata schema.</span></span>

<span data-ttu-id="c5c05-367">Hello výstupní data jsou zapsána toofile pomocí `System.IO.StreamWriter`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-367">hello output data is written toofile by using `System.IO.StreamWriter`.</span></span> <span data-ttu-id="c5c05-368">parametr datového proudu Hello je nastaven příliš`output.BaseStrea` jako součást `IUnstructuredWriter output`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-368">hello stream parameter is set too`output.BaseStrea` as part of `IUnstructuredWriter output`.</span></span>

<span data-ttu-id="c5c05-369">Všimněte si, že je důležité tooflush hello dat vyrovnávací paměti toohello soubor po každé iteraci řádek.</span><span class="sxs-lookup"><span data-stu-id="c5c05-369">Note that it's important tooflush hello data buffer toohello file after each row iteration.</span></span> <span data-ttu-id="c5c05-370">Kromě toho hello `StreamWriter` objekt musí použít s hello na jedno použití atributu povoleno (výchozí) a hello **pomocí** – klíčové slovo:</span><span class="sxs-lookup"><span data-stu-id="c5c05-370">In addition, hello `StreamWriter` object must be used with hello Disposable attribute enabled (default) and with hello **using** keyword:</span></span>

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

<span data-ttu-id="c5c05-371">Jinak volejte metodu Flush() explicitně po každé iteraci.</span><span class="sxs-lookup"><span data-stu-id="c5c05-371">Otherwise, call Flush() method explicitly after each iteration.</span></span> <span data-ttu-id="c5c05-372">Nemůžeme zobrazit v hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="c5c05-372">We show this in hello following example.</span></span>

### <a name="set-headers-and-footers-for-user-defined-outputter"></a><span data-ttu-id="c5c05-373">Nastavte záhlaví a zápatí pro uživatelem definované outputter</span><span class="sxs-lookup"><span data-stu-id="c5c05-373">Set headers and footers for user-defined outputter</span></span>
<span data-ttu-id="c5c05-374">tooset záhlaví, použijte jednu iterace provádění toku.</span><span class="sxs-lookup"><span data-stu-id="c5c05-374">tooset a header, use single iteration execution flow.</span></span>

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

<span data-ttu-id="c5c05-375">nejprve Hello kód v hello `if (isHeaderRow)` bloku se spustí jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="c5c05-375">hello code in hello first `if (isHeaderRow)` block is executed only once.</span></span>

<span data-ttu-id="c5c05-376">Pro zápatí hello použít instanci toohello odkaz hello `System.IO.Stream` objektu (`output.BaseStream`).</span><span class="sxs-lookup"><span data-stu-id="c5c05-376">For hello footer, use hello reference toohello instance of `System.IO.Stream` object (`output.BaseStream`).</span></span> <span data-ttu-id="c5c05-377">Zapisovat hello zápatí v hello Close() metodu hello `IOutputter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-377">Write hello footer in hello Close() method of hello `IOutputter` interface.</span></span>  <span data-ttu-id="c5c05-378">(Další informace najdete v tématu hello následující ukázka.)</span><span class="sxs-lookup"><span data-stu-id="c5c05-378">(For more information, see hello following example.)</span></span>

<span data-ttu-id="c5c05-379">Následuje příklad uživatelem definované outputter:</span><span class="sxs-lookup"><span data-stu-id="c5c05-379">Following is an example of a user-defined outputter:</span></span>

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

<span data-ttu-id="c5c05-380">A základní skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="c5c05-380">And U-SQL base script:</span></span>

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

<span data-ttu-id="c5c05-381">Toto je outputter HTML, který vytvoří soubor HTML s dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-381">This is an HTML outputter, which creates an HTML file with table data.</span></span>

### <a name="call-outputter-from-u-sql-base-script"></a><span data-ttu-id="c5c05-382">Volání outputter ze základní skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="c5c05-382">Call outputter from U-SQL base script</span></span>
<span data-ttu-id="c5c05-383">toocall vlastní outputter z hello základní skript U-SQL, hello novou instanci objektu outputter hello má toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c5c05-383">toocall a custom outputter from hello base U-SQL script, hello new instance of hello outputter object has toobe created.</span></span>

```sql
OUTPUT @rs0 too@output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="c5c05-384">Vytvoření instance hello tooavoid objekt v základní skriptu, můžeme vytvořit funkce obálku, jak je znázorněno v našem příkladu starší:</span><span class="sxs-lookup"><span data-stu-id="c5c05-384">tooavoid creating an instance of hello object in base script, we can create a function wrapper, as shown in our earlier example:</span></span>

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

<span data-ttu-id="c5c05-385">V takovém případě původní volání hello vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-385">In this case, hello original call looks like hello following:</span></span>

```
OUTPUT @rs0 
too@output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a><span data-ttu-id="c5c05-386">Používají procesory uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="c5c05-386">Use user-defined processors</span></span>
<span data-ttu-id="c5c05-387">Uživatelem definované procesoru nebo UDP, je typ UDO U-SQL, která vám umožní příchozí řádky tooprocess hello použitím funkcí programovatelnosti.</span><span class="sxs-lookup"><span data-stu-id="c5c05-387">User-defined processor, or UDP, is a type of U-SQL UDO that enables you tooprocess hello incoming rows by applying programmability features.</span></span> <span data-ttu-id="c5c05-388">UDP vám umožní toocombine sloupců, upravte hodnoty a v případě potřeby přidejte nové sloupce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-388">UDP enables you toocombine columns, modify values, and add new columns if necessary.</span></span> <span data-ttu-id="c5c05-389">V podstatě pomáhá tooprocess sady řádků tooproduce požadované datové prvky.</span><span class="sxs-lookup"><span data-stu-id="c5c05-389">Basically, it helps tooprocess a rowset tooproduce required data elements.</span></span>

<span data-ttu-id="c5c05-390">toodefine UDP, potřebujeme toocreate `IProcessor` rozhraní s hello `SqlUserDefinedProcessor` atribut, který je pro UDP volitelné.</span><span class="sxs-lookup"><span data-stu-id="c5c05-390">toodefine a UDP, we need toocreate an `IProcessor` interface with hello `SqlUserDefinedProcessor` attribute, which is optional for UDP.</span></span>

<span data-ttu-id="c5c05-391">Toto rozhraní by měl obsahovat definici hello hello `IRow` rozhraní řádků přepsat, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c5c05-391">This interface should contain hello definition for hello `IRow` interface rowset override, as shown in hello following example:</span></span>

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

<span data-ttu-id="c5c05-392">**SqlUserDefinedProcessor** Určuje, které hello typ měli být registrován jako procesor definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-392">**SqlUserDefinedProcessor** indicates that hello type should be registered as a user-defined processor.</span></span> <span data-ttu-id="c5c05-393">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-393">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-394">atribut SqlUserDefinedProcessor Hello je **volitelné** pro definici UDP.</span><span class="sxs-lookup"><span data-stu-id="c5c05-394">hello SqlUserDefinedProcessor attribute is **optional** for UDP definition.</span></span>

<span data-ttu-id="c5c05-395">Hello hlavního programovatelnosti objekty jsou **vstupní** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="c5c05-395">hello main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="c5c05-396">vstupní objekt Hello je vstupní sloupce použité tooenumerate a výstup a výstupní data tooset v důsledku činnosti procesoru hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-396">hello input object is used tooenumerate input columns and output, and tooset output data as a result of hello processor activity.</span></span>

<span data-ttu-id="c5c05-397">Pro vstupní sloupce výčet používáme hello `input.Get` metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c05-397">For input columns enumeration, we use hello `input.Get` method.</span></span>

```
string column_name = input.Get<string>("column_name");
```

<span data-ttu-id="c5c05-398">Hello parametr pro `input.Get` metoda je sloupec, který se předá jako součást hello `PRODUCE` klauzule hello `PROCESS` příkaz skriptu základní hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-398">hello parameter for `input.Get` method is a column that's passed as part of hello `PRODUCE` clause of hello `PROCESS` statement of hello U-SQL base script.</span></span> <span data-ttu-id="c5c05-399">Potřebujeme toouse sem zadejte hello správná data.</span><span class="sxs-lookup"><span data-stu-id="c5c05-399">We need toouse hello correct data type here.</span></span>

<span data-ttu-id="c5c05-400">Pro výstup, použijte hello `output.Set` metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c05-400">For output, use hello `output.Set` method.</span></span>

<span data-ttu-id="c5c05-401">Je důležité toonote tento vlastní producent je výstupy pouze, sloupce a hodnoty, které jsou definovány s hello `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="c5c05-401">It's important toonote that custom producer only outputs columns and values that are defined with hello `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", mycolumn);
```

<span data-ttu-id="c5c05-402">výstup Hello skutečné procesoru se aktivuje při volání metody `return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-402">hello actual processor output is triggered by calling `return output.AsReadOnly();`.</span></span>

<span data-ttu-id="c5c05-403">Následuje příklad procesoru:</span><span class="sxs-lookup"><span data-stu-id="c5c05-403">Following is a processor example:</span></span>

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

<span data-ttu-id="c5c05-404">V tomto scénáři případ použití hello procesoru generuje nový sloupec s názvem "full_description" kombinací hello existující sloupce – v tomto případě "user" velká a "des".</span><span class="sxs-lookup"><span data-stu-id="c5c05-404">In this use-case scenario, hello processor is generating a new column called “full_description” by combining hello existing columns--in this case, “user” in upper case, and “des”.</span></span> <span data-ttu-id="c5c05-405">Také znovu vytvoří identifikátor GUID a vrátí hodnot hello původní a nový identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="c5c05-405">It also regenerates a GUID and returns hello original and new GUID values.</span></span>

<span data-ttu-id="c5c05-406">Jak vidíte z předchozího příkladu hello, můžete volat metody C# během `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="c5c05-406">As you can see from hello previous example, you can call C# methods during `output.Set` method call.</span></span>

<span data-ttu-id="c5c05-407">Tady je příklad základní skriptu U-SQL, který používá vlastního procesoru:</span><span class="sxs-lookup"><span data-stu-id="c5c05-407">Following is an example of base U-SQL script that uses a custom processor:</span></span>

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

## <a name="use-user-defined-appliers"></a><span data-ttu-id="c5c05-408">Použít appliers uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="c5c05-408">Use user-defined appliers</span></span>
<span data-ttu-id="c5c05-409">Uživatelem definované applier U-SQL umožňuje funkce tooinvoke vlastní C# pro každý řádek, který je vrácen hello vnější tabulky výraz dotazu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-409">A U-SQL user-defined applier enables you tooinvoke a custom C# function for each row that's returned by hello outer table expression of a query.</span></span> <span data-ttu-id="c5c05-410">pravé vstup Hello je vyhodnotit pro každý řádek z levého vstup hello a hello řádky, které vytváří spojují pro výsledný výstup hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-410">hello right input is evaluated for each row from hello left input, and hello rows that are produced are combined for hello final output.</span></span> <span data-ttu-id="c5c05-411">Hello seznam sloupců, které jsou vytvářeny v operátor APPLY hello jsou hello kombinací hello sadu sloupců v hello v levém horním a hello právo vstup.</span><span class="sxs-lookup"><span data-stu-id="c5c05-411">hello list of columns that are produced by hello APPLY operator are hello combination of hello set of columns in hello left and hello right input.</span></span>

<span data-ttu-id="c5c05-412">Uživatelem definované applier je volaná jako součást hello USQL vyberte výraz.</span><span class="sxs-lookup"><span data-stu-id="c5c05-412">User-defined applier is being invoked as part of hello USQL SELECT expression.</span></span>

<span data-ttu-id="c5c05-413">Typické Hello volání toohello uživatelem definované applier zdá hello následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-413">hello typical call toohello user-defined applier looks like hello following:</span></span>

```
SELECT …
FROM …
CROSS APPLYis used toopass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

<span data-ttu-id="c5c05-414">Další informace o používání appliers ve výrazu SELECT, najdete v části [U-SQL vyberte výběr z křížové použít a vnější použít](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5c05-414">For more information about using appliers in a SELECT expression, see [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span></span>

<span data-ttu-id="c5c05-415">Hello uživatelem definované applier základní definice třídy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c5c05-415">hello user-defined applier base class definition is as follows:</span></span>

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

<span data-ttu-id="c5c05-416">toodefine applier se definovaný uživatelem, potřebujeme toocreate hello `IApplier` rozhraní s hello [`SqlUserDefinedApplier`] atribut, který je pro uživatelem definované applier definice volitelné.</span><span class="sxs-lookup"><span data-stu-id="c5c05-416">toodefine a user-defined applier, we need toocreate hello `IApplier` interface with hello [`SqlUserDefinedApplier`] attribute, which is optional for a user-defined applier definition.</span></span>

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

* <span data-ttu-id="c5c05-417">Použít je volána pro každý řádek tabulky vnější hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-417">Apply is called for each row of hello outer table.</span></span> <span data-ttu-id="c5c05-418">Vrátí hello `IUpdatableRow` výstup sady řádků.</span><span class="sxs-lookup"><span data-stu-id="c5c05-418">It returns hello `IUpdatableRow` output rowset.</span></span>
* <span data-ttu-id="c5c05-419">Třída konstruktor Hello je použité toopass parametry toohello uživatelem definované applier.</span><span class="sxs-lookup"><span data-stu-id="c5c05-419">hello Constructor class is used toopass parameters toohello user-defined applier.</span></span>

<span data-ttu-id="c5c05-420">**SqlUserDefinedApplier** Určuje, které hello typ měli být registrován jako applier se definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-420">**SqlUserDefinedApplier** indicates that hello type should be registered as a user-defined applier.</span></span> <span data-ttu-id="c5c05-421">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-421">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-422">**SqlUserDefinedApplier** je **volitelné** pro definici applier definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-422">**SqlUserDefinedApplier** is **optional** for a user-defined applier definition.</span></span>


<span data-ttu-id="c5c05-423">Hello hlavního programovatelnosti objekty jsou následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-423">hello main programmability objects are as follows:</span></span>

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

<span data-ttu-id="c5c05-424">Vstupní sady řádků se předávají jako `IRow` vstupní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-424">Input rowsets are passed as `IRow` input.</span></span> <span data-ttu-id="c5c05-425">Hello výstup řádky jsou generovány jako `IUpdatableRow` rozhraní výstup.</span><span class="sxs-lookup"><span data-stu-id="c5c05-425">hello output rows are generated as `IUpdatableRow` output interface.</span></span>

<span data-ttu-id="c5c05-426">Názvy jednotlivých sloupců se dá určit pomocí volání hello `IRow` metoda schématu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-426">Individual column names can be determined by calling hello `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="c5c05-427">tooget hello skutečná data hodnoty z hello příchozí `IRow`, jsme použijte metodu Get() hello `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-427">tooget hello actual data values from hello incoming `IRow`, we use hello Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="c5c05-428">Nebo používáme název sloupce hello schématu:</span><span class="sxs-lookup"><span data-stu-id="c5c05-428">Or we use hello schema column name:</span></span>

```
row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="c5c05-429">Hello výstupní hodnoty musí být nastavena s `IUpdatableRow` výstup:</span><span class="sxs-lookup"><span data-stu-id="c5c05-429">hello output values must be set with `IUpdatableRow` output:</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="c5c05-430">Je důležité toounderstand vlastní appliers pouze výstup sloupců a hodnot, které jsou definovány s `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="c5c05-430">It is important toounderstand that custom appliers only output columns and values that are defined with `output.Set` method call.</span></span>

<span data-ttu-id="c5c05-431">aktuální výstup Hello se aktivuje při volání metody `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-431">hello actual output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="c5c05-432">Hello uživatelem definované applier parametry lze předat toohello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="c5c05-432">hello user-defined applier parameters can be passed toohello constructor.</span></span> <span data-ttu-id="c5c05-433">Applier může vrátit proměnný počet sloupců, které je třeba toobe definované během hello applier volání v základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-433">Applier can return a variable number of columns that need toobe defined during hello applier call in base U-SQL Script.</span></span>

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

<span data-ttu-id="c5c05-434">Tady je příklad applier uživatelem definované hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-434">Here is hello user-defined applier example:</span></span>

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

<span data-ttu-id="c5c05-435">Toto je hello základní skript U-SQL pro tento uživatelem definované applier:</span><span class="sxs-lookup"><span data-stu-id="c5c05-435">Following is hello base U-SQL script for this user-defined applier:</span></span>

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

<span data-ttu-id="c5c05-436">V tento scénář případu využití, uživatelem definované applier jednání jako analyzátor oddělených čárkou pro hello car loďstva vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c5c05-436">In this use case scenario, user-defined applier acts as a comma-delimited value parser for hello car fleet properties.</span></span> <span data-ttu-id="c5c05-437">řádky vstupní soubor Hello vypadat hello následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-437">hello input file rows look like hello following:</span></span>

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

<span data-ttu-id="c5c05-438">Je typické oddělený tabulátory TSV soubor s vlastností sloupec, který obsahuje vlastnosti car například výrobce a model.</span><span class="sxs-lookup"><span data-stu-id="c5c05-438">It is a typical tab-delimited TSV file with a properties column that contains car properties such as make and model.</span></span> <span data-ttu-id="c5c05-439">Tyto vlastnosti musí být Analyzovaná toohello sloupců tabulky.</span><span class="sxs-lookup"><span data-stu-id="c5c05-439">Those properties must be parsed toohello table columns.</span></span> <span data-ttu-id="c5c05-440">Hello applier, který je k dispozici také umožňuje toogenerate dynamické počet vlastností v hello způsobit řádků, na základě hello parametru, který je předán.</span><span class="sxs-lookup"><span data-stu-id="c5c05-440">hello applier that's provided also enables you toogenerate a dynamic number of properties in hello result rowset, based on hello parameter that's passed.</span></span> <span data-ttu-id="c5c05-441">Můžete vygenerovat buď všechny vlastnosti, nebo konkrétní sadu pouze vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c5c05-441">You can generate either all properties or a specific set of properties only.</span></span>

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

<span data-ttu-id="c5c05-442">uživatelem definované applier Hello lze volat jako novou instanci třídy applier objektu:</span><span class="sxs-lookup"><span data-stu-id="c5c05-442">hello user-defined applier can be called as a new instance of applier object:</span></span>

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

<span data-ttu-id="c5c05-443">Nebo pomocí hello volání metody vytváření obálky:</span><span class="sxs-lookup"><span data-stu-id="c5c05-443">Or with hello invocation of a wrapper factory method:</span></span>

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a><span data-ttu-id="c5c05-444">Použít combiners uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="c5c05-444">Use user-defined combiners</span></span>
<span data-ttu-id="c5c05-445">Uživatelem definované kombinační nebo UDC, umožňuje toocombine řádky z levé a pravé sady řádků, na základě vlastní logiky.</span><span class="sxs-lookup"><span data-stu-id="c5c05-445">User-defined combiner, or UDC, enables you toocombine rows from left and right rowsets, based on custom logic.</span></span> <span data-ttu-id="c5c05-446">Uživatelem definované kombinační se používá s KOMBINAČNÍ výraz.</span><span class="sxs-lookup"><span data-stu-id="c5c05-446">User-defined combiner is used with COMBINE expression.</span></span>

<span data-ttu-id="c5c05-447">Výraz hello kombinovaného ÚTVARU, který poskytuje hello nezbytné informace o obou hello vstupní sady řádků se vyvolání kombinační, hello seskupení sloupců, hello očekávaný výsledek schématu a další informace.</span><span class="sxs-lookup"><span data-stu-id="c5c05-447">A combiner is being invoked with hello COMBINE expression that provides hello necessary information about both hello input rowsets, hello grouping columns, hello expected result schema, and additional information.</span></span>

<span data-ttu-id="c5c05-448">toocall kombinační v základní skript U-SQL, můžeme použít hello následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="c5c05-448">toocall a combiner in a base U-SQL script, we use hello following syntax:</span></span>

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

<span data-ttu-id="c5c05-449">Další informace najdete v tématu [KOMBINOVAT výrazu (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span><span class="sxs-lookup"><span data-stu-id="c5c05-449">For more information, see [COMBINE Expression (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span></span>

<span data-ttu-id="c5c05-450">toodefine kombinační se definovaný uživatelem, potřebujeme toocreate hello `ICombiner` rozhraní s hello [`SqlUserDefinedCombiner`] atribut, který je pro definici uživatelem definované kombinační volitelné.</span><span class="sxs-lookup"><span data-stu-id="c5c05-450">toodefine a user-defined combiner, we need toocreate hello `ICombiner` interface with hello [`SqlUserDefinedCombiner`] attribute, which is optional for a user-defined Combiner definition.</span></span>

<span data-ttu-id="c5c05-451">Základní `ICombiner` definici třídy:</span><span class="sxs-lookup"><span data-stu-id="c5c05-451">Base `ICombiner` class definition:</span></span>

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

<span data-ttu-id="c5c05-452">vlastní implementace Hello `ICombiner` rozhraní by měl obsahovat definici hello pro `IEnumerable<IRow>` kombinovat přepsání.</span><span class="sxs-lookup"><span data-stu-id="c5c05-452">hello custom implementation of an `ICombiner` interface should contain hello definition for an `IEnumerable<IRow>` Combine override.</span></span>

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

<span data-ttu-id="c5c05-453">Hello **SqlUserDefinedCombiner** atribut uvádí, které hello typ měli být registrován jako kombinační se definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-453">hello **SqlUserDefinedCombiner** attribute indicates that hello type should be registered as a user-defined combiner.</span></span> <span data-ttu-id="c5c05-454">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-454">This class cannot be inherited.</span></span>

<span data-ttu-id="c5c05-455">**SqlUserDefinedCombiner** je vlastnost režimu kombinační použité toodefine hello.</span><span class="sxs-lookup"><span data-stu-id="c5c05-455">**SqlUserDefinedCombiner** is used toodefine hello Combiner mode property.</span></span> <span data-ttu-id="c5c05-456">Je volitelný atribut pro definici kombinační definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-456">It is an optional attribute for a user-defined combiner definition.</span></span>

<span data-ttu-id="c5c05-457">Režim CombinerMode</span><span class="sxs-lookup"><span data-stu-id="c5c05-457">CombinerMode     Mode</span></span>

<span data-ttu-id="c5c05-458">Výčet CombinerMode může trvat hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="c5c05-458">CombinerMode enum can take hello following values:</span></span>

* <span data-ttu-id="c5c05-459">Úplné (0), každý řádek výstupu potenciálně závisí na všechny hello vstupní řádky z levé a pravé s hello stejnou hodnotu klíče.</span><span class="sxs-lookup"><span data-stu-id="c5c05-459">Full  (0) Every output row potentially depends on all hello input rows from left and right       with hello same key value.</span></span>

* <span data-ttu-id="c5c05-460">Každý řádek výstupu závisí na jednom řádku vstupní zleva hello doleva (1) (a potenciálně všechny řádky z hello přímo s hello stejnou hodnotu klíče).</span><span class="sxs-lookup"><span data-stu-id="c5c05-460">Left  (1) Every output row depends on a single input row from hello left (and potentially all rows       from hello right with hello same key value).</span></span>

* <span data-ttu-id="c5c05-461">Každý řádek výstupu závisí na jeden vstupní řádek z hello správné vpravo (2) (a potenciálně všechny řádky z levé hello s hello stejnou hodnotu klíče).</span><span class="sxs-lookup"><span data-stu-id="c5c05-461">Right (2)     Every output row depends on a single input row from hello right (and potentially all rows       from hello left with hello same key value).</span></span>

* <span data-ttu-id="c5c05-462">Vnitřní (3), každý řádek výstupu závisí na jeden vstup řádek z levé a pravé s hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-462">Inner (3) Every output row depends on a single input row from left and right with hello same value.</span></span>

<span data-ttu-id="c5c05-463">Příklad: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span><span class="sxs-lookup"><span data-stu-id="c5c05-463">Example:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span></span>


<span data-ttu-id="c5c05-464">Hello hlavního programovatelnosti objekty jsou:</span><span class="sxs-lookup"><span data-stu-id="c5c05-464">hello main programmability objects are:</span></span>

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

<span data-ttu-id="c5c05-465">Vstupní sady řádků se předávají jako **levém** a **správné** `IRowset` typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-465">Input rowsets are passed as **left** and **right** `IRowset` type of interface.</span></span> <span data-ttu-id="c5c05-466">Obě sady řádků musí být ve výčtu objevit pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="c5c05-466">Both rowsets must be enumerated for processing.</span></span> <span data-ttu-id="c5c05-467">Každé rozhraní můžete pouze výčet jednou, takže jsme tooenumerate a uložení do mezipaměti v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="c5c05-467">You can only enumerate each interface once, so we have tooenumerate and cache it if necessary.</span></span>

<span data-ttu-id="c5c05-468">Pro ukládání do mezipaměti pro účely, můžeme vytvořit seznam\<T\> typ konstrukce paměti v důsledku LINQ spuštění dotazu, konkrétně seznam <`IRow`>.</span><span class="sxs-lookup"><span data-stu-id="c5c05-468">For caching purposes, we can create a List\<T\> type of memory structure as a result of a LINQ query execution, specifically List<`IRow`>.</span></span> <span data-ttu-id="c5c05-469">Hello anonymní datový typ můžete použít během výčtu také.</span><span class="sxs-lookup"><span data-stu-id="c5c05-469">hello anonymous data type can be used during enumeration as well.</span></span>

<span data-ttu-id="c5c05-470">V tématu [Úvod tooLINQ dotazů (C#)](https://msdn.microsoft.com/library/bb397906.aspx) Další informace o dotazech LINQ a [rozhraní IEnumerable\<T\> rozhraní](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) Další informace o rozhraní IEnumerable\<T\> rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-470">See [Introduction tooLINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) for more information about LINQ queries, and [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) for more information about IEnumerable\<T\> interface.</span></span>

<span data-ttu-id="c5c05-471">tooget hello skutečná data hodnoty z hello příchozí `IRowset`, jsme použijte metodu Get() hello `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c5c05-471">tooget hello actual data values from hello incoming `IRowset`, we use hello Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="c5c05-472">Názvy jednotlivých sloupců se dá určit pomocí volání hello `IRow` metoda schématu.</span><span class="sxs-lookup"><span data-stu-id="c5c05-472">Individual column names can be determined by calling hello `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="c5c05-473">Nebo pomocí názvu sloupce hello schématu:</span><span class="sxs-lookup"><span data-stu-id="c5c05-473">Or by using hello schema column name:</span></span>

```
c# row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="c5c05-474">Obecné – výčet Hello s dotazy LINQ vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="c5c05-474">hello general enumeration with LINQ looks like hello following:</span></span>

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

<span data-ttu-id="c5c05-475">Po vytvoření výčtu obě sady řádků, přidáme tooloop prostřednictvím všechny řádky.</span><span class="sxs-lookup"><span data-stu-id="c5c05-475">After enumerating both rowsets, we are going tooloop through all rows.</span></span> <span data-ttu-id="c5c05-476">Pro každý řádek v levém řádků hello přidáme toofind všechny řádky, které splňují podmínku hello naše kombinační.</span><span class="sxs-lookup"><span data-stu-id="c5c05-476">For each row in hello left rowset, we are going toofind all rows that satisfy hello condition of our combiner.</span></span>

<span data-ttu-id="c5c05-477">Hello výstupní hodnoty musí být nastavena s `IUpdatableRow` výstup.</span><span class="sxs-lookup"><span data-stu-id="c5c05-477">hello output values must be set with `IUpdatableRow` output.</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="c5c05-478">aktuální výstup Hello se aktivuje při volání metody příliš`yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-478">hello actual output is triggered by calling too`yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="c5c05-479">Následuje příklad kombinační:</span><span class="sxs-lookup"><span data-stu-id="c5c05-479">Following is a combiner example:</span></span>

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

<span data-ttu-id="c5c05-480">V tomto scénáři případ použití jsme se vytváření zprávu o analýzy pro hello prodejce.</span><span class="sxs-lookup"><span data-stu-id="c5c05-480">In this use-case scenario, we are building an analytics report for hello retailer.</span></span> <span data-ttu-id="c5c05-481">cílem Hello je toofind všechny produkty, které náklady více než 20 000 $ a že prodeje prostřednictvím webu hello rychlejší než prostřednictvím prodejce regulární hello v určitém časovém rámci.</span><span class="sxs-lookup"><span data-stu-id="c5c05-481">hello goal is toofind all products that cost more than $20,000 and that sell through hello website faster than through hello regular retailer within a certain time frame.</span></span>

<span data-ttu-id="c5c05-482">Zde je hello základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-482">Here is hello base U-SQL script.</span></span> <span data-ttu-id="c5c05-483">Můžete porovnat hello logiku mezi regulárního spojení a kombinační:</span><span class="sxs-lookup"><span data-stu-id="c5c05-483">You can compare hello logic between a regular JOIN and a combiner:</span></span>

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

<span data-ttu-id="c5c05-484">Uživatelem definované kombinační lze volat jako novou instanci objektu applier hello:</span><span class="sxs-lookup"><span data-stu-id="c5c05-484">A user-defined combiner can be called as a new instance of hello applier object:</span></span>

```
USING new MyNameSpace.MyCombiner();
```


<span data-ttu-id="c5c05-485">Nebo pomocí hello volání metody vytváření obálky:</span><span class="sxs-lookup"><span data-stu-id="c5c05-485">Or with hello invocation of a wrapper factory method:</span></span>

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a><span data-ttu-id="c5c05-486">Použít přechodky uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="c5c05-486">Use user-defined reducers</span></span>

<span data-ttu-id="c5c05-487">U-SQL umožňuje toowrite přechodky vlastní řádků v jazyce C# s využitím rozhraní rozšiřitelnosti uživatelem definovaný operátor hello a implementace rozhraní IReducer.</span><span class="sxs-lookup"><span data-stu-id="c5c05-487">U-SQL enables you toowrite custom rowset reducers in C# by using hello user-defined operator extensibility framework and implementing an IReducer interface.</span></span>

<span data-ttu-id="c5c05-488">Uživatelem definované reduktorem nebo UDR, můžou být použité tooeliminate nepotřebné řádků při extrakci dat (Importovat).</span><span class="sxs-lookup"><span data-stu-id="c5c05-488">User-defined reducer, or UDR, can be used tooeliminate unnecessary rows during data extraction (import).</span></span> <span data-ttu-id="c5c05-489">Také můžete být použité toomanipulate a vyhodnotit řádků a sloupců.</span><span class="sxs-lookup"><span data-stu-id="c5c05-489">It also can be used toomanipulate and evaluate rows and columns.</span></span> <span data-ttu-id="c5c05-490">Na základě programovatelnosti logiky, ho můžete také definovat řádky potřebovat toobe extrahovat.</span><span class="sxs-lookup"><span data-stu-id="c5c05-490">Based on programmability logic, it can also define which rows need toobe extracted.</span></span>

<span data-ttu-id="c5c05-491">toodefine třídu UDR, potřebujeme toocreate `IReducer` rozhraní s volitelný `SqlUserDefinedReducer` atribut.</span><span class="sxs-lookup"><span data-stu-id="c5c05-491">toodefine a UDR class, we need toocreate an `IReducer` interface with an optional `SqlUserDefinedReducer` attribute.</span></span>

<span data-ttu-id="c5c05-492">Tato třída rozhraní by měl obsahovat definici hello `IEnumerable` přepsat rozhraní sady řádků.</span><span class="sxs-lookup"><span data-stu-id="c5c05-492">This class interface should contain a definition for hello `IEnumerable` interface rowset override.</span></span>

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

<span data-ttu-id="c5c05-493">Hello **SqlUserDefinedReducer** atribut uvádí, které hello typ měli být registrován jako reduktorem se definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-493">hello **SqlUserDefinedReducer** attribute indicates that hello type should be registered as a user-defined reducer.</span></span> <span data-ttu-id="c5c05-494">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="c5c05-494">This class cannot be inherited.</span></span>
<span data-ttu-id="c5c05-495">**SqlUserDefinedReducer** je volitelný atribut pro definici reduktorem definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="c5c05-495">**SqlUserDefinedReducer** is an optional attribute for a user-defined reducer definition.</span></span> <span data-ttu-id="c5c05-496">Použije se toodefine IsRecursive vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c5c05-496">It's used toodefine IsRecursive property.</span></span>

* <span data-ttu-id="c5c05-497">BOOL IsRecursive</span><span class="sxs-lookup"><span data-stu-id="c5c05-497">bool     IsRecursive</span></span>    
* <span data-ttu-id="c5c05-498">**Hodnota TRUE,** = označuje, zda je tento reduktorem idempotent</span><span class="sxs-lookup"><span data-stu-id="c5c05-498">**true**  = Indicates whether this Reducer is idempotent</span></span>

<span data-ttu-id="c5c05-499">Hello hlavního programovatelnosti objekty jsou **vstupní** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="c5c05-499">hello main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="c5c05-500">vstupní objekt Hello je použité tooenumerate vstupní řádky.</span><span class="sxs-lookup"><span data-stu-id="c5c05-500">hello input object is used tooenumerate input rows.</span></span> <span data-ttu-id="c5c05-501">Výstup je použité tooset výstup řádků v důsledku omezení aktivity.</span><span class="sxs-lookup"><span data-stu-id="c5c05-501">Output is used tooset output rows as a result of reducing activity.</span></span>

<span data-ttu-id="c5c05-502">Pro vstupní řádky výčet používáme hello `Row.Get` metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c05-502">For input rows enumeration, we use hello `Row.Get` method.</span></span>

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

<span data-ttu-id="c5c05-503">parametr pro hello Hello `Row.Get` metoda je sloupec, který se předá jako součást hello `PRODUCE` třídu hello `REDUCE` příkaz skriptu základní hello U-SQL.</span><span class="sxs-lookup"><span data-stu-id="c5c05-503">hello parameter for hello `Row.Get` method is a column that's passed as part of hello `PRODUCE` class of hello `REDUCE` statement of hello U-SQL base script.</span></span> <span data-ttu-id="c5c05-504">Potřebujeme toouse hello správného datového typu zde také.</span><span class="sxs-lookup"><span data-stu-id="c5c05-504">We need toouse hello correct data type here as well.</span></span>

<span data-ttu-id="c5c05-505">Pro výstup, použijte hello `output.Set` metoda.</span><span class="sxs-lookup"><span data-stu-id="c5c05-505">For output, use hello `output.Set` method.</span></span>

<span data-ttu-id="c5c05-506">Je důležité toounderstand, který vlastní reduktorem pouze výstupy hodnoty, které jsou definovány s hello `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="c5c05-506">It is important toounderstand that custom reducer only outputs values that are defined with hello `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", guid);
```

<span data-ttu-id="c5c05-507">skutečné reduktorem výstup Hello se aktivuje při volání metody `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="c5c05-507">hello actual reducer output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="c5c05-508">Následuje příklad reduktorem:</span><span class="sxs-lookup"><span data-stu-id="c5c05-508">Following is a reducer example:</span></span>

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

<span data-ttu-id="c5c05-509">V tomto scénáři případ použití hello reduktorem přeskakuje řádky s prázdné jméno.</span><span class="sxs-lookup"><span data-stu-id="c5c05-509">In this use-case scenario, hello reducer is skipping rows with an empty user name.</span></span> <span data-ttu-id="c5c05-510">Pro každý řádek v sadě řádků přečte každý požadovaný sloupec, následně vyhodnocuje hello délka hello uživatelské jméno.</span><span class="sxs-lookup"><span data-stu-id="c5c05-510">For each row in rowset, it reads each required column, then evaluates hello length of hello user name.</span></span> <span data-ttu-id="c5c05-511">Skutečný řádek hello vyprodukuje pouze v případě, že hodnota délka jména uživatele je větší než 0.</span><span class="sxs-lookup"><span data-stu-id="c5c05-511">It outputs hello actual row only if user name value length is more than 0.</span></span>

<span data-ttu-id="c5c05-512">Toto je základní skript U-SQL, která používá vlastní reduktorem:</span><span class="sxs-lookup"><span data-stu-id="c5c05-512">Following is base U-SQL script that uses a custom reducer:</span></span>

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
