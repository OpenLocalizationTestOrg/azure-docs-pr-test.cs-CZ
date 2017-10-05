---
title: "Průvodci programovatelnosti U-SQL Azure Data Lake | Microsoft Docs"
description: "Další informace o sadu služeb v Azure Data Lake, které vám umožní vytvořit cloudové velkých objemů dat platformu."
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
ms.openlocfilehash: e4e298475d7be7d51c8bd55be498371ed6ce77a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="u-sql-programmability-guide"></a><span data-ttu-id="9956c-103">Průvodce programovatelnosti U-SQL</span><span class="sxs-lookup"><span data-stu-id="9956c-103">U-SQL programmability guide</span></span>

<span data-ttu-id="9956c-104">U-SQL je dotazovací jazyk, který je určený pro velkých datových typů úloh.</span><span class="sxs-lookup"><span data-stu-id="9956c-104">U-SQL is a query language that's designed for big data-type of workloads.</span></span> <span data-ttu-id="9956c-105">Mezi funkce jedinečné jazykem U-SQL se rozumí kombinace jazyka SQL jako deklarativní s rozšiřitelnosti a programovatelnosti, který je zadán v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="9956c-105">One of the unique features of U-SQL is the combination of the SQL-like declarative language with the extensibility and programmability that's provided by C#.</span></span> <span data-ttu-id="9956c-106">V této příručce budeme soustředit na rozšiřitelnost a programovatelnosti jazyka U-SQL, který je povolený v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="9956c-106">In this guide, we concentrate on the extensibility and programmability of the U-SQL language that's enabled by C#.</span></span>

## <a name="requirements"></a><span data-ttu-id="9956c-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9956c-107">Requirements</span></span>

<span data-ttu-id="9956c-108">Stáhněte a nainstalujte [nástrojů Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="9956c-108">Download and install [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="get-started-with-u-sql"></a><span data-ttu-id="9956c-109">Začínáme s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="9956c-109">Get started with U-SQL</span></span>  

<span data-ttu-id="9956c-110">Podívejme se na následující skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-110">Let’s look at the following U-SQL script:</span></span>

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

<span data-ttu-id="9956c-111">Definuje sadu řádků názvem @a a vytvoří sadu řádků názvem @results z @a.</span><span class="sxs-lookup"><span data-stu-id="9956c-111">It defines a RowSet called @a and creates a RowSet called @results from @a.</span></span>

## <a name="c-types-and-expressions-in-u-sql-script"></a><span data-ttu-id="9956c-112">Typy C# a výrazy ve skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="9956c-112">C# types and expressions in U-SQL script</span></span>

<span data-ttu-id="9956c-113">Výraz U-SQL je výraz jazyka C# v kombinaci s logických operací U-SQL, `AND`, `OR`, a `NOT`.</span><span class="sxs-lookup"><span data-stu-id="9956c-113">A U-SQL Expression is a C# expression combined with U-SQL logical operations such `AND`, `OR`, and `NOT`.</span></span> <span data-ttu-id="9956c-114">Výrazy U-SQL lze použít s příkazem SELECT, EXTRAHOVÁNÍ, kde s, Seskupit podle a DEKLAROVAT.</span><span class="sxs-lookup"><span data-stu-id="9956c-114">U-SQL Expressions can be used with SELECT, EXTRACT, WHERE, HAVING, GROUP BY and DECLARE.</span></span>

<span data-ttu-id="9956c-115">Například následující skript analyzuje řetězec hodnotu data a času v klauzuli SELECT.</span><span class="sxs-lookup"><span data-stu-id="9956c-115">For example, the following script parses a string a DateTime value in the SELECT clause.</span></span>

```
@results =
    SELECT
        customer,
    amount,
    DateTime.Parse(date) AS date
    FROM @a;    
```

<span data-ttu-id="9956c-116">Následující skript analyzuje řetězec hodnotu data a času v příkazu DECLARE.</span><span class="sxs-lookup"><span data-stu-id="9956c-116">The following script parses a string a DateTime value in a DECLARE statement.</span></span>

```
DECLARE @d DateTime = ToDateTime.Date("2016/01/01");
```

### <a name="use-c-expressions-for-data-type-conversions"></a><span data-ttu-id="9956c-117">Použití jazyka C# výrazů pro konverze datových typů</span><span class="sxs-lookup"><span data-stu-id="9956c-117">Use C# expressions for data type conversions</span></span>
<span data-ttu-id="9956c-118">Následující příklad ukazuje, jak můžete provést převod dat data a času pomocí výrazy jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="9956c-118">The following example demonstrates how you can do a datetime data conversion by using C# expressions.</span></span> <span data-ttu-id="9956c-119">V tomto scénáři konkrétní data řetězce data a času jsou převedeny na standardní datetime s půlnoc 00:00:00 čas zápis.</span><span class="sxs-lookup"><span data-stu-id="9956c-119">In this particular scenario, string datetime data is converted to standard datetime with midnight 00:00:00 time notation.</span></span>

```
DECLARE @dt String = "2016-07-06 10:23:15";

@rs1 =
    SELECT 
        Convert.ToDateTime(Convert.ToDateTime(@dt).ToString("yyyy-MM-dd")) AS dt,
        dt AS olddt
    FROM @rs0;
OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

### <a name="use-c-expressions-for-todays-date"></a><span data-ttu-id="9956c-120">Použití jazyka C# výrazů pro dnešní datum</span><span class="sxs-lookup"><span data-stu-id="9956c-120">Use C# expressions for today’s date</span></span>
<span data-ttu-id="9956c-121">Vyžádání dnešní datum, můžeme použít následující výraz jazyka C#:</span><span class="sxs-lookup"><span data-stu-id="9956c-121">To pull today’s date, we can use the following C# expression:</span></span>

```
DateTime.Now.ToString("M/d/yyyy")
```

<span data-ttu-id="9956c-122">Tady je příklad toho, jak používat tento výraz ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="9956c-122">Here's an example of how to use this expression in a script:</span></span>

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



## <a name="using-net-assemblies"></a><span data-ttu-id="9956c-123">Použití sestavení rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9956c-123">Using .NET assemblies</span></span>
<span data-ttu-id="9956c-124">Model rozšiřitelnosti U-SQL založena na možnost přidat vlastní kód.</span><span class="sxs-lookup"><span data-stu-id="9956c-124">U-SQL’s extensibility model relies heavily on the ability to add custom code.</span></span> <span data-ttu-id="9956c-125">V současné době U-SQL umožňuje snadno způsoby, jak přidat vlastní Microsoft. Na základě NET kód (zejména, C#).</span><span class="sxs-lookup"><span data-stu-id="9956c-125">Currently, U-SQL provides you with easy ways to add your own Microsoft .NET-based code (in particular, C#).</span></span> <span data-ttu-id="9956c-126">Můžete však také přidat vlastní kód, který je zapsán do jiných jazyků .NET, například VB.NET nebo F #.</span><span class="sxs-lookup"><span data-stu-id="9956c-126">However, you can also add custom code that's written in other .NET languages, such as VB.NET or F#.</span></span> 

### <a name="register-a-net-assembly"></a><span data-ttu-id="9956c-127">Registrace sestavení rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="9956c-127">Register a .NET assembly</span></span>

<span data-ttu-id="9956c-128">Použijte příkaz CREATE ASSEMBLY umístit sestavení .NET do databáze U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-128">Use the CREATE ASSEMBLY statement to place a .NET assembly into a U-SQL Database.</span></span> <span data-ttu-id="9956c-129">Po sestavení je v databázi, skriptů U-SQL můžete použít tyto sestavení s použitím příkazu referenční sestavení.</span><span class="sxs-lookup"><span data-stu-id="9956c-129">Once an assembly is in a database, U-SQL scripts can use those assemblies by using the REFERENCE ASSEMBLY statement.</span></span> 

<span data-ttu-id="9956c-130">Následující kód ukazuje, jak registrovat sestavení:</span><span class="sxs-lookup"><span data-stu-id="9956c-130">The following code shows how to register an assembly:</span></span>

```
CREATE ASSEMBLY MyDB.[MyAssembly]
    FROM "/myassembly.dll";
```

<span data-ttu-id="9956c-131">Následující kód ukazuje, jak odkazovat na sestavení:</span><span class="sxs-lookup"><span data-stu-id="9956c-131">The following code shows how to reference an assembly:</span></span>

```
REFERENCE ASSEMBLY MyDB.[MyAssembly];
```

<span data-ttu-id="9956c-132">Obrátit [pokyny pro registraci sestavení](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) , která se vztahuje toto téma podrobněji.</span><span class="sxs-lookup"><span data-stu-id="9956c-132">Consult the [assembly registration instructions](https://blogs.msdn.microsoft.com/azuredatalake/2016/08/26/how-to-register-u-sql-assemblies-in-your-u-sql-catalog/) that covers this topic in greater detail.</span></span>


### <a name="use-assembly-versioning"></a><span data-ttu-id="9956c-133">Pomocí správy verzí sestavení</span><span class="sxs-lookup"><span data-stu-id="9956c-133">Use assembly versioning</span></span>
<span data-ttu-id="9956c-134">U-SQL v současné době používá rozhraní .NET Framework verze 4.5.</span><span class="sxs-lookup"><span data-stu-id="9956c-134">Currently, U-SQL uses the .NET Framework version 4.5.</span></span> <span data-ttu-id="9956c-135">Proto zajistěte, aby byly kompatibilní s danou verzi modulu runtime vlastního sestavení.</span><span class="sxs-lookup"><span data-stu-id="9956c-135">So ensure that your own assemblies are compatible with that version of the runtime.</span></span>

<span data-ttu-id="9956c-136">Jak už bylo zmíněno dříve, spustí kód U-SQL ve formátu 64bitovou (x 64).</span><span class="sxs-lookup"><span data-stu-id="9956c-136">As mentioned earlier, U-SQL runs code in a 64-bit (x64) format.</span></span> <span data-ttu-id="9956c-137">Proto se ujistěte, že je zkompilovaný kód pro spuštění na x64.</span><span class="sxs-lookup"><span data-stu-id="9956c-137">So make sure that your code is compiled to run on x64.</span></span> <span data-ttu-id="9956c-138">V opačném případě se zobrazí správný formát chyba, uvedena výše.</span><span class="sxs-lookup"><span data-stu-id="9956c-138">Otherwise you get the incorrect format error shown earlier.</span></span>

<span data-ttu-id="9956c-139">Každý odeslán sestavení knihoven DLL a souboru prostředků, jako je jiný modul runtime, nativní sestavení nebo konfiguračním souboru může být maximálně 400 MB.</span><span class="sxs-lookup"><span data-stu-id="9956c-139">Each uploaded assembly DLL and resource file, such as a different runtime, a native assembly, or a config file, can be at most 400 MB.</span></span> <span data-ttu-id="9956c-140">Celková velikost nasazené prostředky, prostřednictvím nasazení prostředků nebo prostřednictvím odkazů na sestavení a další soubory, nemůže být delší než 3 GB.</span><span class="sxs-lookup"><span data-stu-id="9956c-140">The total size of deployed resources, either via DEPLOY RESOURCE or via references to assemblies and their additional files, cannot exceed 3 GB.</span></span>

<span data-ttu-id="9956c-141">Nakonec si všimněte, že každý U-SQL databáze může obsahovat pouze jednu verzi žádné zadaného sestavení.</span><span class="sxs-lookup"><span data-stu-id="9956c-141">Finally, note that each U-SQL database can only contain one version of any given assembly.</span></span> <span data-ttu-id="9956c-142">Například pokud budete potřebovat verze 7 a 8 verzi knihovny NewtonSoft Json.Net, budete muset registraci je ve dvou různých databází.</span><span class="sxs-lookup"><span data-stu-id="9956c-142">For example, if you need both version 7 and version 8 of the NewtonSoft Json.Net library, you need to register them in two different databases.</span></span> <span data-ttu-id="9956c-143">Kromě toho každý skript může odkazovat pouze na jednu verzi zadaného sestavení knihovny DLL.</span><span class="sxs-lookup"><span data-stu-id="9956c-143">Furthermore, each script can only refer to one version of a given assembly DLL.</span></span> <span data-ttu-id="9956c-144">V tomto ohledu následuje U-SQL C# sestavení správy a správa verzí sémantiky.</span><span class="sxs-lookup"><span data-stu-id="9956c-144">In this respect, U-SQL follows the C# assembly management and versioning semantics.</span></span>


## <a name="use-user-defined-functions-udf"></a><span data-ttu-id="9956c-145">Použití uživatelem definované funkce: UDF</span><span class="sxs-lookup"><span data-stu-id="9956c-145">Use user-defined functions: UDF</span></span>
<span data-ttu-id="9956c-146">Uživatelem definované funkce U-SQL nebo UDF, jsou programovacích rutin, které přijímají parametry, provedení akce (například komplexní výpočet) a vrátit výsledek této akce jako hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9956c-146">U-SQL user-defined functions, or UDF, are programming routines that accept parameters, perform an action (such as a complex calculation), and return the result of that action as a value.</span></span> <span data-ttu-id="9956c-147">Návratová hodnota UDF lze pouze jednu skalární hodnota.</span><span class="sxs-lookup"><span data-stu-id="9956c-147">The return value of UDF can only be a single scalar.</span></span> <span data-ttu-id="9956c-148">UDF U-SQL je možné volat v základní skript U-SQL jako všechny ostatní C# skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="9956c-148">U-SQL UDF can be called in U-SQL base script like any other C# scalar function.</span></span>

<span data-ttu-id="9956c-149">Doporučujeme, abyste inicializaci U-SQL uživatelsky definované funkce jako **veřejné** a **statické**.</span><span class="sxs-lookup"><span data-stu-id="9956c-149">We recommend that you initialize U-SQL user-defined functions as **public** and **static**.</span></span>

```
public static string MyFunction(string param1)
{
    return "my result";
}
```

<span data-ttu-id="9956c-150">První Podíváme se na jednoduchý příklad vytvoření uživatelem definovanou FUNKCI.</span><span class="sxs-lookup"><span data-stu-id="9956c-150">First let’s look at the simple example of creating a UDF.</span></span>

<span data-ttu-id="9956c-151">V tomto scénáři případ použití musíme určit fiskální období, včetně fiskální čtvrtletí a fiskální měsíc prvního sign-in pro konkrétního uživatele.</span><span class="sxs-lookup"><span data-stu-id="9956c-151">In this use-case scenario, we need to determine the fiscal period, including the fiscal quarter and fiscal month of the first sign-in for the specific user.</span></span> <span data-ttu-id="9956c-152">První měsíc fiskálního roku v našem scénáři je června.</span><span class="sxs-lookup"><span data-stu-id="9956c-152">The first fiscal month of the year in our scenario is June.</span></span>

<span data-ttu-id="9956c-153">K výpočtu fiskální období, zavedeme následující funkce jazyka C#:</span><span class="sxs-lookup"><span data-stu-id="9956c-153">To calculate fiscal period, we introduce the following C# function:</span></span>

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

<span data-ttu-id="9956c-154">Jednoduše vypočítá fiskální měsíc a čtvrtletí a vrátí hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="9956c-154">It simply calculates fiscal month and quarter and returns a string value.</span></span> <span data-ttu-id="9956c-155">Pro červen prvního měsíce od první fiskální čtvrtletí používáme "Q1:P1".</span><span class="sxs-lookup"><span data-stu-id="9956c-155">For June, the first month of the first fiscal quarter, we use "Q1:P1".</span></span> <span data-ttu-id="9956c-156">Pro červenec můžeme použít "Q1:P2" a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9956c-156">For July, we use "Q1:P2", and so on.</span></span>

<span data-ttu-id="9956c-157">To je normální C# funkce budeme používat v našem projekt U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-157">This is a regular C# function that we are going to use in our U-SQL project.</span></span>

<span data-ttu-id="9956c-158">Zde je, jak vypadá v části kódu v tomto scénáři:</span><span class="sxs-lookup"><span data-stu-id="9956c-158">Here is how the code-behind section looks in this scenario:</span></span>

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

<span data-ttu-id="9956c-159">Teď přidáme volání této funkce ze základní skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-159">Now we are going to call this function from the base U-SQL script.</span></span> <span data-ttu-id="9956c-160">K tomuto účelu máme zadejte plně kvalifikovaný název funkce, včetně oboru názvů, který je v tomto případě NameSpace.Class.Function(parameter).</span><span class="sxs-lookup"><span data-stu-id="9956c-160">To do this, we have to provide a fully qualified name for the function, including the namespace, which in this case is NameSpace.Class.Function(parameter).</span></span>

```
USQL_Programmability.CustomFunctions.GetFiscalPeriod(dt)
```

<span data-ttu-id="9956c-161">Toto je skutečný základní skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-161">Following is the actual U-SQL base script:</span></span>

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
    TO @output_file 
    USING Outputters.Text();
```

<span data-ttu-id="9956c-162">Toto je výstupní soubor spuštění skriptu:</span><span class="sxs-lookup"><span data-stu-id="9956c-162">Following is the output file of the script execution:</span></span>

```
0d8b9630-d5ca-11e5-8329-251efa3a2941,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User1",""

20843640-d771-11e5-b87b-8b7265c75a44,2016-02-11T07:04:17.2630000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User2",""

301f23d2-d690-11e5-9a98-4b4f60a1836f,2016-02-11T09:01:33.9720000-08:00,2016-06-01T00:00:00.0000000,"Q3:8","User3",""
```

<span data-ttu-id="9956c-163">Tento příklad ukazuje jednoduchý využití vložené UDF v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-163">This example demonstrates a simple usage of inline UDF in U-SQL.</span></span>

### <a name="keep-state-between-udf-invocations"></a><span data-ttu-id="9956c-164">Zachovat stav mezi UDF volání</span><span class="sxs-lookup"><span data-stu-id="9956c-164">Keep state between UDF invocations</span></span>
<span data-ttu-id="9956c-165">U-SQL C# programovatelnosti objekty může být více pokročilé, využitím interaktivity prostřednictvím globální proměnné kódu.</span><span class="sxs-lookup"><span data-stu-id="9956c-165">U-SQL C# programmability objects can be more sophisticated, utilizing interactivity through the code-behind global variables.</span></span> <span data-ttu-id="9956c-166">Podívejme se na následující scénář případu použití firmy.</span><span class="sxs-lookup"><span data-stu-id="9956c-166">Let’s look at the following business use-case scenario.</span></span>

<span data-ttu-id="9956c-167">Ve velkých organizacích mohou uživatelé přepínat mezi typy interních aplikací.</span><span class="sxs-lookup"><span data-stu-id="9956c-167">In large organizations, users can switch between varieties of internal applications.</span></span> <span data-ttu-id="9956c-168">Může jít o Microsoft Dynamics CRM, PowerBI a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9956c-168">These can include Microsoft Dynamics CRM, PowerBI, and so on.</span></span> <span data-ttu-id="9956c-169">Zákazníci chtít použít analýzu telemetrii jak uživatelé přepínat mezi různými aplikacemi, jaké jsou trendy využití, a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9956c-169">Customers might want to apply a telemetry analysis of how users switch between different applications, what the usage trends are, and so on.</span></span> <span data-ttu-id="9956c-170">Cílem pro firmu je za účelem optimalizace využití aplikace.</span><span class="sxs-lookup"><span data-stu-id="9956c-170">The goal for the business is to optimize application usage.</span></span> <span data-ttu-id="9956c-171">Také může chtějí kombinovat různé aplikace nebo konkrétní rutiny přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9956c-171">They also might want to combine different applications or specific sign-on routines.</span></span>

<span data-ttu-id="9956c-172">K dosažení tohoto cíle, musíme určit ID relace a prodlevy mezi poslední relace, který došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="9956c-172">To achieve this goal, we have to determine session IDs and lag time between the last session that occurred.</span></span>

<span data-ttu-id="9956c-173">Potřebujeme najít předchozí přihlášení a pak mu přiřaďte tento přihlášení do všech relací, ke kterým dochází na stejné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9956c-173">We need to find a previous sign-in and then assign this sign-in to all sessions that are being generated to the same application.</span></span> <span data-ttu-id="9956c-174">První výzva je, že základní skript U-SQL nepodporuje nám umožňují použít výpočty již počítaného sloupce se funkce LAG.</span><span class="sxs-lookup"><span data-stu-id="9956c-174">The first challenge is that U-SQL base script doesn't allow us to apply calculations over already-calculated columns with LAG function.</span></span> <span data-ttu-id="9956c-175">Druhá výzva je, že se musí zachovat konkrétní relace pro všechny relace v rámci stejné časové období.</span><span class="sxs-lookup"><span data-stu-id="9956c-175">The second challenge is that we have to keep the specific session for all sessions within the same time period.</span></span>

<span data-ttu-id="9956c-176">Chcete-li tento problém vyřešit, používáme globální proměnné v části kódu: `static public string globalSession;`.</span><span class="sxs-lookup"><span data-stu-id="9956c-176">To solve this problem, we use a global variable inside a code-behind section: `static public string globalSession;`.</span></span>

<span data-ttu-id="9956c-177">Tato globální proměnná se použije pro celý řádků při našich provádění skriptu.</span><span class="sxs-lookup"><span data-stu-id="9956c-177">This global variable is applied to the entire rowset during our script execution.</span></span>

<span data-ttu-id="9956c-178">Tady je části kódu programu naše U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-178">Here is the code-behind section of our U-SQL program:</span></span>

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

<span data-ttu-id="9956c-179">Tento příklad ukazuje globální proměnná `static public string globalSession;` použít uvnitř `getStampUserSession` funkce a získávání inicializace pokaždé, když se změní parametr relace.</span><span class="sxs-lookup"><span data-stu-id="9956c-179">This example shows the global variable `static public string globalSession;` used inside the `getStampUserSession` function and getting reinitialized each time the Session parameter is changed.</span></span>

<span data-ttu-id="9956c-180">Základní skript U-SQL je následující:</span><span class="sxs-lookup"><span data-stu-id="9956c-180">The U-SQL base script is as follows:</span></span>

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
    TO @out2
    ORDER BY UserName, EventDateTime ASC
    USING Outputters.Csv();
```

<span data-ttu-id="9956c-181">Funkce `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` se nazývá zde během druhé sady řádků výpočtu paměti.</span><span class="sxs-lookup"><span data-stu-id="9956c-181">Function `USQLApplication21.UserSession.getStampUserSession(UserSessionTimestamp)` is called here during the second memory rowset calculation.</span></span> <span data-ttu-id="9956c-182">Pak předá `UserSessionTimestamp` sloupce a vrátí hodnotu, dokud `UserSessionTimestamp` došlo ke změně.</span><span class="sxs-lookup"><span data-stu-id="9956c-182">It passes the `UserSessionTimestamp` column and returns the value until `UserSessionTimestamp` has changed.</span></span>

<span data-ttu-id="9956c-183">Výstupní soubor je následující:</span><span class="sxs-lookup"><span data-stu-id="9956c-183">The output file is as follows:</span></span>

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

<span data-ttu-id="9956c-184">Tento příklad ukazuje složitější scénář případu použití používáme globální proměnné v části kódu, který se použije pro celý paměti řádků.</span><span class="sxs-lookup"><span data-stu-id="9956c-184">This example demonstrates a more complicated use-case scenario in which we use a global variable inside a code-behind section that's applied to the entire memory rowset.</span></span>

## <a name="use-user-defined-types-udt"></a><span data-ttu-id="9956c-185">Uživatelem definované typy použití: UDT</span><span class="sxs-lookup"><span data-stu-id="9956c-185">Use user-defined types: UDT</span></span>
<span data-ttu-id="9956c-186">Uživatelem definované typy nebo UDT, je jiná funkcí programovatelnosti U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-186">User-defined types, or UDT, is another programmability feature of U-SQL.</span></span> <span data-ttu-id="9956c-187">U-SQL UDT funguje jako regulární C# definovaný uživatelem typem.</span><span class="sxs-lookup"><span data-stu-id="9956c-187">U-SQL UDT acts like a regular C# user-defined type.</span></span> <span data-ttu-id="9956c-188">C# je jazyk silného typu, který umožňuje použít předdefinované a vlastní uživatelem definované typy.</span><span class="sxs-lookup"><span data-stu-id="9956c-188">C# is a strongly typed language that allows the use of built-in and custom user-defined types.</span></span>

<span data-ttu-id="9956c-189">U-SQL nelze implicitně serializaci nebo libovolný uživatelsky definovaný typ deserializovat, když UDT předána mezi vrcholy v sady řádků.</span><span class="sxs-lookup"><span data-stu-id="9956c-189">U-SQL cannot implicitly serialize or de-serialize arbitrary UDTs when the UDT is passed between vertices in rowsets.</span></span> <span data-ttu-id="9956c-190">To znamená, že uživatel musí zadat explicitní formátovací modul používá rozhraní IFormatter.</span><span class="sxs-lookup"><span data-stu-id="9956c-190">This means that the user has to provide an explicit formatter by using the IFormatter interface.</span></span> <span data-ttu-id="9956c-191">To poskytuje U-SQL serializace a deserializovat metody pro UDT.</span><span class="sxs-lookup"><span data-stu-id="9956c-191">This provides U-SQL with the serialize and de-serialize methods for the UDT.</span></span>

> [!NOTE]
> <span data-ttu-id="9956c-192">Předdefinované extraktory U-SQL a outputters aktuálně nelze serializovat nebo deserializovat UDT data do nebo ze souborů i sadou IFormatter.</span><span class="sxs-lookup"><span data-stu-id="9956c-192">U-SQL’s built-in extractors and outputters currently cannot serialize or de-serialize UDT data to or from files even with the IFormatter set.</span></span> <span data-ttu-id="9956c-193">Proto při zápisu do souboru s příkazem výstupní UDT data, nebo jeho čtení s extrakci, budete muset předat jako řetězec nebo bajtové pole.</span><span class="sxs-lookup"><span data-stu-id="9956c-193">So when you're writing UDT data to a file with the OUTPUT statement, or reading it with an extractor, you have to pass it as a string or byte array.</span></span> <span data-ttu-id="9956c-194">Potom volání serializace a deserializace explicitně kódu (to znamená, metodu ToString() UDT).</span><span class="sxs-lookup"><span data-stu-id="9956c-194">Then you call the serialization and deserialization code (that is, the UDT’s ToString() method) explicitly.</span></span> <span data-ttu-id="9956c-195">Uživatelem definované extraktory a outputters, na druhé straně může číst a zapisovat UDT.</span><span class="sxs-lookup"><span data-stu-id="9956c-195">User-defined extractors and outputters, on the other hand, can read and write UDTs.</span></span>

<span data-ttu-id="9956c-196">Pokud jsme zkuste použít UDT v EXTRAKTOR nebo OUTPUTTER (mimo předchozí vyberte), jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="9956c-196">If we try to use UDT in EXTRACTOR or OUTPUTTER (out of previous SELECT), as shown here:</span></span>

```
@rs1 =
    SELECT 
        MyNameSpace.Myfunction_Returning_UDT(filed1) AS myfield
    FROM @rs0;

OUTPUT @rs1 
    TO @output_file 
    USING Outputters.Text();
```

<span data-ttu-id="9956c-197">Nemůžeme zobrazit následující chyba:</span><span class="sxs-lookup"><span data-stu-id="9956c-197">We receive the following error:</span></span>

```
Error   1   E_CSC_USER_INVALIDTYPEINOUTPUTTER: Outputters.Text was used to output column myfield of type
MyNameSpace.Myfunction_Returning_UDT.

Description:

Outputters.Text only supports built-in types.

Resolution:

Implement a custom outputter that knows how to serialize this type, or call a serialization method on the type in
the preceding SELECT.   C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\
USQL-Programmability\Types.usql 52  1   USQL-Programmability
```

<span data-ttu-id="9956c-198">Pro práci s UDT v outputter, máme buď ho na řetězec pomocí metody ToString() nebo vytvořit vlastní outputter serializovat.</span><span class="sxs-lookup"><span data-stu-id="9956c-198">To work with UDT in outputter, we either have to serialize it to string with the ToString() method or create a custom outputter.</span></span>

<span data-ttu-id="9956c-199">UDT aktuálně nelze použít v GROUP BY.</span><span class="sxs-lookup"><span data-stu-id="9956c-199">UDTs currently cannot be used in GROUP BY.</span></span> <span data-ttu-id="9956c-200">Pokud UDT se používá v GROUP BY, je vyvolána k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="9956c-200">If UDT is used in GROUP BY, the following error is thrown:</span></span>

```
Error   1   E_CSC_USER_INVALIDTYPEINCLAUSE: GROUP BY doesn't support type MyNameSpace.Myfunction_Returning_UDT
for column myfield

Description:

GROUP BY doesn't support UDT or Complex types.

Resolution:

Add a SELECT statement where you can project a scalar column that you want to use with GROUP BY.
C:\Users\sergeypu\Documents\Visual Studio 2013\Projects\USQL-Programmability\USQL-Programmability\Types.usql
62  5   USQL-Programmability
```

<span data-ttu-id="9956c-201">Pokud chcete definovat typ definovaný uživatelem, musíme:</span><span class="sxs-lookup"><span data-stu-id="9956c-201">To define a UDT, we have to:</span></span>

* <span data-ttu-id="9956c-202">Přidání následujících oborů názvů:</span><span class="sxs-lookup"><span data-stu-id="9956c-202">Add the following namespaces:</span></span>

```
using Microsoft.Analytics.Interfaces
using System.IO;
```

* <span data-ttu-id="9956c-203">Přidat `Microsoft.Analytics.Interfaces`, což je vyžadováno pro UDT rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-203">Add `Microsoft.Analytics.Interfaces`, which is required for the UDT interfaces.</span></span> <span data-ttu-id="9956c-204">Kromě toho `System.IO` mohou být potřebné k definování rozhraní IFormatter.</span><span class="sxs-lookup"><span data-stu-id="9956c-204">In addition, `System.IO` might be needed to define the IFormatter interface.</span></span>

* <span data-ttu-id="9956c-205">Definování typu používá definované atributem SqlUserDefinedType.</span><span class="sxs-lookup"><span data-stu-id="9956c-205">Define a used-defined type with SqlUserDefinedType attribute.</span></span>

<span data-ttu-id="9956c-206">**SqlUserDefinedType** slouží k označení definice typu v sestavení jako uživatelem definovaný typ (UDT) v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-206">**SqlUserDefinedType** is used to mark a type definition in an assembly as a user-defined type (UDT) in U-SQL.</span></span> <span data-ttu-id="9956c-207">Vlastnosti v atributu projeví fyzické charakteristiky UDT.</span><span class="sxs-lookup"><span data-stu-id="9956c-207">The properties on the attribute reflect the physical characteristics of the UDT.</span></span> <span data-ttu-id="9956c-208">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-208">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-209">SqlUserDefinedType je povinný atribut pro UDT definici.</span><span class="sxs-lookup"><span data-stu-id="9956c-209">SqlUserDefinedType is a required attribute for UDT definition.</span></span>

<span data-ttu-id="9956c-210">V konstruktoru třídy:</span><span class="sxs-lookup"><span data-stu-id="9956c-210">The constructor of the class:</span></span>  

* <span data-ttu-id="9956c-211">SqlUserDefinedTypeAttribute (formátovací modul typu)</span><span class="sxs-lookup"><span data-stu-id="9956c-211">SqlUserDefinedTypeAttribute (type formatter)</span></span>

* <span data-ttu-id="9956c-212">Formátovací modul typu: požadovaný parametr k definování formátování UDT – konkrétně typ `IFormatter` rozhraní musí být předán sem.</span><span class="sxs-lookup"><span data-stu-id="9956c-212">Type formatter: Required parameter to define an UDT formatter--specifically, the type of the `IFormatter` interface must be passed here.</span></span>

```
[SqlUserDefinedType(typeof(MyTypeFormatter))]
public class MyType
{ … }
```

* <span data-ttu-id="9956c-213">Typické UDT taky vyžaduje definice IFormatter rozhraní, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9956c-213">Typical UDT also requires definition of the IFormatter interface, as shown in the following example:</span></span>

```
public class MyTypeFormatter : IFormatter<MyType>
{
    public void Serialize(MyType instance, IColumnWriter writer, ISerializationContext context)
    { … }

    public MyType Deserialize(IColumnReader reader, ISerializationContext context)
    { … }
}
```

<span data-ttu-id="9956c-214">`IFormatter` Rozhraní serializuje a poté serializuje grafu objektu s kořenový typ \<typeparamref name = "T" >.</span><span class="sxs-lookup"><span data-stu-id="9956c-214">The `IFormatter` interface serializes and de-serializes an object graph with the root type of \<typeparamref name="T">.</span></span>

<span data-ttu-id="9956c-215">\<typeparam name = "T" > kořenový typ grafu objektů serializovat a deserializovat.</span><span class="sxs-lookup"><span data-stu-id="9956c-215">\<typeparam name="T">The root type for the object graph to serialize and de-serialize.</span></span>

* <span data-ttu-id="9956c-216">**Deserializovat**: zrušte serializuje data na zadaný datový proud a reconstitutes grafu objektů.</span><span class="sxs-lookup"><span data-stu-id="9956c-216">**Deserialize**: De-serializes the data on the provided stream and reconstitutes the graph of objects.</span></span>

* <span data-ttu-id="9956c-217">**Serializovat**: serializuje objektu, nebo grafu objektů s danou kořenovou do zadaného datového proudu.</span><span class="sxs-lookup"><span data-stu-id="9956c-217">**Serialize**: Serializes an object, or graph of objects, with the given root to the provided stream.</span></span>

<span data-ttu-id="9956c-218">`MyType`instance: Instance typu.</span><span class="sxs-lookup"><span data-stu-id="9956c-218">`MyType` instance: Instance of the type.</span></span>  
<span data-ttu-id="9956c-219">`IColumnWriter`Zapisovač / `IColumnReader` čtečky: základního datového proudu sloupce.</span><span class="sxs-lookup"><span data-stu-id="9956c-219">`IColumnWriter` writer / `IColumnReader` reader: The underlying column stream.</span></span>  
<span data-ttu-id="9956c-220">`ISerializationContext`kontext: výčet, který definuje sadu příznaky, která určuje zdrojový nebo cílový kontext pro datový proud během serializace.</span><span class="sxs-lookup"><span data-stu-id="9956c-220">`ISerializationContext` context: Enum that defines a set of flags that specifies the source or destination context for the stream during serialization.</span></span>

* <span data-ttu-id="9956c-221">**Zprostředkující**: Určuje, že kontext zdrojové nebo cílové není trvalého úložiště.</span><span class="sxs-lookup"><span data-stu-id="9956c-221">**Intermediate**: Specifies that the source or destination context is not a persisted store.</span></span>

* <span data-ttu-id="9956c-222">**Trvalost**: Určuje, že zdrojový nebo cílový kontext je trvalé úložiště.</span><span class="sxs-lookup"><span data-stu-id="9956c-222">**Persistence**: Specifies that the source or destination context is a persisted store.</span></span>

<span data-ttu-id="9956c-223">Jako regulární C# typ definici UDT U-SQL můžete zahrnout přepsání pro operátory, jako +/ == /! =.</span><span class="sxs-lookup"><span data-stu-id="9956c-223">As a regular C# type, a U-SQL UDT definition can include overrides for operators such as +/==/!=.</span></span> <span data-ttu-id="9956c-224">Může také obsahovat statické metody.</span><span class="sxs-lookup"><span data-stu-id="9956c-224">It can also include static methods.</span></span> <span data-ttu-id="9956c-225">Například pokud jsme se bude používat toto UDT jako parametr pro agregační funkci MIN U-SQL, se musí definovat < operátor přepsání.</span><span class="sxs-lookup"><span data-stu-id="9956c-225">For example, if we are going to use this UDT as a parameter to a U-SQL MIN aggregate function, we have to define < operator override.</span></span>

<span data-ttu-id="9956c-226">Dříve v tomto průvodci jsme ukázán příklad pro fiskálním období identifikaci od určitého data ve formátu Qn:Pn (Q1:P10).</span><span class="sxs-lookup"><span data-stu-id="9956c-226">Earlier in this guide, we demonstrated an example for fiscal period identification from the specific date in the format Qn:Pn (Q1:P10).</span></span> <span data-ttu-id="9956c-227">Následující příklad ukazuje, jak definovat vlastní typ pro hodnoty fiskálním období.</span><span class="sxs-lookup"><span data-stu-id="9956c-227">The following example shows how to define a custom type for fiscal period values.</span></span>

<span data-ttu-id="9956c-228">Tady je příklad oddílu kódu s vlastní UDT a IFormatter rozhraní:</span><span class="sxs-lookup"><span data-stu-id="9956c-228">Following is an example of a code-behind section with custom UDT and IFormatter interface:</span></span>

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

<span data-ttu-id="9956c-229">Definovaného typu zahrnuje dvě čísla: čtvrtletí a měsíce.</span><span class="sxs-lookup"><span data-stu-id="9956c-229">The defined type includes two numbers: quarter and month.</span></span> <span data-ttu-id="9956c-230">Operátory == /! = / > nebo < a statickou metodu ToString() jsou definovány v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="9956c-230">Operators ==/!=/>/< and static method ToString() are defined here.</span></span>

<span data-ttu-id="9956c-231">Jak už bylo zmíněno dříve, můžete použít ve výrazech vyberte UDT, ale nejde použít v OUTPUTTER/EXTRAKTOR bez vlastní serializace.</span><span class="sxs-lookup"><span data-stu-id="9956c-231">As mentioned earlier, UDT can be used in SELECT expressions, but cannot be used in OUTPUTTER/EXTRACTOR without custom serialization.</span></span> <span data-ttu-id="9956c-232">Je buď musí být serializován jako řetězec s ToString() nebo použít s vlastní OUTPUTTER/EXTRAKTOR.</span><span class="sxs-lookup"><span data-stu-id="9956c-232">It either has to be serialized as a string with ToString() or used with a custom OUTPUTTER/EXTRACTOR.</span></span>

<span data-ttu-id="9956c-233">Nyní probereme využití UDT.</span><span class="sxs-lookup"><span data-stu-id="9956c-233">Now let’s discuss usage of UDT.</span></span> <span data-ttu-id="9956c-234">V části kódu jsme změnili naše funkce GetFiscalPeriod takto:</span><span class="sxs-lookup"><span data-stu-id="9956c-234">In a code-behind section, we changed our GetFiscalPeriod function to the following:</span></span>

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

<span data-ttu-id="9956c-235">Jak vidíte, vrátí hodnotu typu naše FiscalPeriod.</span><span class="sxs-lookup"><span data-stu-id="9956c-235">As you can see, it returns the value of our FiscalPeriod type.</span></span>

<span data-ttu-id="9956c-236">Zde jsme uveďte příklad toho, jak ho další použít základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-236">Here we provide an example of how to use it further in U-SQL base script.</span></span> <span data-ttu-id="9956c-237">Tento příklad ukazuje různé formy UDT volání ze skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-237">This example demonstrates different forms of UDT invocation from U-SQL script.</span></span>

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

       // This user-defined type was created in the prior SELECT.  Passing the UDT to this subsequent SELECT would have failed if the UDT was not annotated with an IFormatter.
           fiscalperiod_adjusted.ToString() AS fiscalperiod_adjusted,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

<span data-ttu-id="9956c-238">Tady je příklad oddílu úplné kódu:</span><span class="sxs-lookup"><span data-stu-id="9956c-238">Here's an example of a full code-behind section:</span></span>

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

## <a name="use-user-defined-aggregates-udagg"></a><span data-ttu-id="9956c-239">Použití uživatelem definovaných agregacích: UDAGG</span><span class="sxs-lookup"><span data-stu-id="9956c-239">Use user-defined aggregates: UDAGG</span></span>
<span data-ttu-id="9956c-240">Uživatelem definované agregace jsou funkce související s agregace, které jsou součástí není out-of-the-box se U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-240">User-defined aggregates are any aggregation-related functions that are not shipped out-of-the-box with U-SQL.</span></span> <span data-ttu-id="9956c-241">V příkladu může být agregace k provedení vlastní matematické výpočty, zřetězení řetězců, manipulace s řetězci a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9956c-241">The example can be an aggregate to perform custom math calculations, string concatenations, manipulations with strings, and so on.</span></span>

<span data-ttu-id="9956c-242">Definice uživatelem definované agregace základní třídy je následující:</span><span class="sxs-lookup"><span data-stu-id="9956c-242">The user-defined aggregate base class definition is as follows:</span></span>

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

<span data-ttu-id="9956c-243">**SqlUserDefinedAggregate** označuje, že typ by měl být registrován jako uživatelem definovaná agregace.</span><span class="sxs-lookup"><span data-stu-id="9956c-243">**SqlUserDefinedAggregate** indicates that the type should be registered as a user-defined aggregate.</span></span> <span data-ttu-id="9956c-244">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-244">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-245">Atribut SqlUserDefinedType je **volitelné** pro UDAGG definici.</span><span class="sxs-lookup"><span data-stu-id="9956c-245">SqlUserDefinedType attribute is **optional** for UDAGG definition.</span></span>


<span data-ttu-id="9956c-246">Základní třída umožňuje předat abstraktní tři parametry: dvě jako vstupní parametry a jako výsledek.</span><span class="sxs-lookup"><span data-stu-id="9956c-246">The base class allows you to pass three abstract parameters: two as input parameters and one as the result.</span></span> <span data-ttu-id="9956c-247">Datové typy jsou proměnné a by měl být definován během dědičnosti tříd.</span><span class="sxs-lookup"><span data-stu-id="9956c-247">The data types are variable and should be defined during class inheritance.</span></span>

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

* <span data-ttu-id="9956c-248">**Init –** vyvolá jednou pro každou skupinu během výpočtu.</span><span class="sxs-lookup"><span data-stu-id="9956c-248">**Init** invokes once for each group during computation.</span></span> <span data-ttu-id="9956c-249">Poskytuje rutiny inicializace pro každou skupinu agregace.</span><span class="sxs-lookup"><span data-stu-id="9956c-249">It provides an initialization routine for each aggregation group.</span></span>  
* <span data-ttu-id="9956c-250">**Accumulate** se spustí jednou pro každou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9956c-250">**Accumulate** is executed once for each value.</span></span> <span data-ttu-id="9956c-251">Poskytuje hlavní funkce algoritmu agregace.</span><span class="sxs-lookup"><span data-stu-id="9956c-251">It provides the main functionality for the aggregation algorithm.</span></span> <span data-ttu-id="9956c-252">Může sloužit k agregované hodnoty s různými typy dat, které jsou definovány během dědičnosti tříd.</span><span class="sxs-lookup"><span data-stu-id="9956c-252">It can be used to aggregate values with various data types that are defined during class inheritance.</span></span> <span data-ttu-id="9956c-253">Může přijímat dva parametry proměnné datové typy.</span><span class="sxs-lookup"><span data-stu-id="9956c-253">It can accept two parameters of variable data types.</span></span>
* <span data-ttu-id="9956c-254">**Ukončit** se spustí jednou na skupinu agregace na konci zpracování k vypsání výsledků pro každou skupinu.</span><span class="sxs-lookup"><span data-stu-id="9956c-254">**Terminate** is executed once per aggregation group at the end of processing to output the result for each group.</span></span>

<span data-ttu-id="9956c-255">Deklarovat správný vstupní a výstupní datové typy, použijte definici třídy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9956c-255">To declare correct input and output data types, use the class definition as follows:</span></span>

```
public abstract class IAggregate<T1, T2, TResult> : IAggregate
```

* <span data-ttu-id="9956c-256">T1: Accumulate první parametr</span><span class="sxs-lookup"><span data-stu-id="9956c-256">T1: First parameter to accumulate</span></span>
* <span data-ttu-id="9956c-257">T2: Accumulate první parametr</span><span class="sxs-lookup"><span data-stu-id="9956c-257">T2: First parameter to accumulate</span></span>
* <span data-ttu-id="9956c-258">TResult: Návratový typ ukončit</span><span class="sxs-lookup"><span data-stu-id="9956c-258">TResult: Return type of terminate</span></span>

<span data-ttu-id="9956c-259">Například:</span><span class="sxs-lookup"><span data-stu-id="9956c-259">For example:</span></span>

```
public class GuidAggregate : IAggregate<string, int, int>
```

<span data-ttu-id="9956c-260">nebo</span><span class="sxs-lookup"><span data-stu-id="9956c-260">or</span></span>

```
public class GuidAggregate : IAggregate<string, string, string>
```

### <a name="use-udagg-in-u-sql"></a><span data-ttu-id="9956c-261">Použití UDAGG v U-SQL</span><span class="sxs-lookup"><span data-stu-id="9956c-261">Use UDAGG in U-SQL</span></span>
<span data-ttu-id="9956c-262">Chcete-li použít UDAGG, nejprve definování v kódu nebo odkazovat z existující programovatelnosti knihovny DLL, jak je popsáno výše.</span><span class="sxs-lookup"><span data-stu-id="9956c-262">To use UDAGG, first define it in code-behind or reference it from the existent programmability DLL as discussed earlier.</span></span>

<span data-ttu-id="9956c-263">Pak použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="9956c-263">Then use the following syntax:</span></span>

```
AGG<UDAGG_functionname>(param1,param2)
```

<span data-ttu-id="9956c-264">Tady je příklad UDAGG:</span><span class="sxs-lookup"><span data-stu-id="9956c-264">Here is an example of UDAGG:</span></span>

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

<span data-ttu-id="9956c-265">A základní skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-265">And base U-SQL script:</span></span>

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

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

<span data-ttu-id="9956c-266">V tomto scénáři případ použití jsme řetězení třída identifikátory GUID pro konkrétní uživatele.</span><span class="sxs-lookup"><span data-stu-id="9956c-266">In this use-case scenario, we concatenate class GUIDs for the specific users.</span></span>

## <a name="use-user-defined-objects-udo"></a><span data-ttu-id="9956c-267">Uživatelem definované objekty použít: UDO</span><span class="sxs-lookup"><span data-stu-id="9956c-267">Use user-defined objects: UDO</span></span>
<span data-ttu-id="9956c-268">U-SQL umožňuje definovat vlastní programovatelnosti objekty, které se nazývají uživatelem definované objekty nebo UDO.</span><span class="sxs-lookup"><span data-stu-id="9956c-268">U-SQL enables you to define custom programmability objects, which are called user-defined objects or UDO.</span></span>

<span data-ttu-id="9956c-269">Následuje seznam UDO v U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-269">The following is a list of UDO in U-SQL:</span></span>

* <span data-ttu-id="9956c-270">Uživatelem definované extraktory</span><span class="sxs-lookup"><span data-stu-id="9956c-270">User-defined extractors</span></span>
    * <span data-ttu-id="9956c-271">Extrakce řádek po řádku</span><span class="sxs-lookup"><span data-stu-id="9956c-271">Extract row by row</span></span>
    * <span data-ttu-id="9956c-272">Použít k implementaci extrakce dat z vlastních strukturovaných souborů</span><span class="sxs-lookup"><span data-stu-id="9956c-272">Used to implement data extraction from custom structured files</span></span>

* <span data-ttu-id="9956c-273">Uživatelem definované outputters</span><span class="sxs-lookup"><span data-stu-id="9956c-273">User-defined outputters</span></span>
    * <span data-ttu-id="9956c-274">Výstup po řádcích</span><span class="sxs-lookup"><span data-stu-id="9956c-274">Output row by row</span></span>
    * <span data-ttu-id="9956c-275">Použít k výstupu vlastní datové typy nebo vlastního souboru formáty</span><span class="sxs-lookup"><span data-stu-id="9956c-275">Used to output custom data types or custom file formats</span></span>

* <span data-ttu-id="9956c-276">Uživatelem definované procesorů</span><span class="sxs-lookup"><span data-stu-id="9956c-276">User-defined processors</span></span>
    * <span data-ttu-id="9956c-277">Vezme jeden řádek a vytvoří jeden řádek</span><span class="sxs-lookup"><span data-stu-id="9956c-277">Take one row and produce one row</span></span>
    * <span data-ttu-id="9956c-278">Umožňuje snížit počet sloupců nebo vytvořit nové sloupce s hodnotami, které jsou odvozené z existující sady sloupců</span><span class="sxs-lookup"><span data-stu-id="9956c-278">Used to reduce the number of columns or produce new columns with values that are derived from an existing column set</span></span>

* <span data-ttu-id="9956c-279">Uživatelem definované appliers</span><span class="sxs-lookup"><span data-stu-id="9956c-279">User-defined appliers</span></span>
    * <span data-ttu-id="9956c-280">Vezme jeden řádek a vytvoří 0 n řádků</span><span class="sxs-lookup"><span data-stu-id="9956c-280">Take one row and produce 0 to n rows</span></span>
    * <span data-ttu-id="9956c-281">Použít s vnější/mezi použít</span><span class="sxs-lookup"><span data-stu-id="9956c-281">Used with OUTER/CROSS APPLY</span></span>

* <span data-ttu-id="9956c-282">Uživatelem definované combiners</span><span class="sxs-lookup"><span data-stu-id="9956c-282">User-defined combiners</span></span>
    * <span data-ttu-id="9956c-283">Kombinuje sady řádků – uživatelem definované spojení</span><span class="sxs-lookup"><span data-stu-id="9956c-283">Combines rowsets--user-defined JOINs</span></span>

* <span data-ttu-id="9956c-284">Uživatelem definované přechodky</span><span class="sxs-lookup"><span data-stu-id="9956c-284">User-defined reducers</span></span>
    * <span data-ttu-id="9956c-285">Vezme n řádků a vytvoří jeden řádek</span><span class="sxs-lookup"><span data-stu-id="9956c-285">Take n rows and produce one row</span></span>
    * <span data-ttu-id="9956c-286">Se používá ke snížení počtu řádků</span><span class="sxs-lookup"><span data-stu-id="9956c-286">Used to reduce the number of rows</span></span>

<span data-ttu-id="9956c-287">UDO se obvykle nazývá explicitně ve skriptu U-SQL v rámci následující příkazy U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-287">UDO is typically called explicitly in U-SQL script as part of the following U-SQL statements:</span></span>

* <span data-ttu-id="9956c-288">EXTRAKCE</span><span class="sxs-lookup"><span data-stu-id="9956c-288">EXTRACT</span></span>
* <span data-ttu-id="9956c-289">VÝSTUP</span><span class="sxs-lookup"><span data-stu-id="9956c-289">OUTPUT</span></span>
* <span data-ttu-id="9956c-290">PROCES</span><span class="sxs-lookup"><span data-stu-id="9956c-290">PROCESS</span></span>
* <span data-ttu-id="9956c-291">KOMBINOVÁNÍ</span><span class="sxs-lookup"><span data-stu-id="9956c-291">COMBINE</span></span>
* <span data-ttu-id="9956c-292">SNÍŽENÍ</span><span class="sxs-lookup"><span data-stu-id="9956c-292">REDUCE</span></span>

> [!NOTE]  
> <span data-ttu-id="9956c-293">UDO na jsou omezené využívat 0,5 Gb paměti.</span><span class="sxs-lookup"><span data-stu-id="9956c-293">UDO’s are limited to consume 0.5Gb memory.</span></span>  <span data-ttu-id="9956c-294">Toto omezení paměti se nevztahuje na místní spuštění.</span><span class="sxs-lookup"><span data-stu-id="9956c-294">This memory limitation does not apply to local executions.</span></span>

## <a name="use-user-defined-extractors"></a><span data-ttu-id="9956c-295">Použití uživatelem definované extraktory</span><span class="sxs-lookup"><span data-stu-id="9956c-295">Use user-defined extractors</span></span>
<span data-ttu-id="9956c-296">U-SQL umožňuje importovat externí data pomocí příkazu EXTRAKCE.</span><span class="sxs-lookup"><span data-stu-id="9956c-296">U-SQL allows you to import external data by using an EXTRACT statement.</span></span> <span data-ttu-id="9956c-297">Příkaz EXTRAKCE můžete použít předdefinované extraktory UDO:</span><span class="sxs-lookup"><span data-stu-id="9956c-297">An EXTRACT statement can use built-in UDO extractors:</span></span>  

* <span data-ttu-id="9956c-298">*Extractors.Text()*: poskytuje extrakci z textových souborů s oddělovači různá kódování.</span><span class="sxs-lookup"><span data-stu-id="9956c-298">*Extractors.Text()*: Provides extraction from delimited text files of different encodings.</span></span>

* <span data-ttu-id="9956c-299">*Extractors.Csv()*: poskytuje extrakci z hodnot oddělených čárkami (CSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="9956c-299">*Extractors.Csv()*: Provides extraction from comma-separated value (CSV) files of different encodings.</span></span>

* <span data-ttu-id="9956c-300">*Extractors.Tsv()*: poskytuje extrakci z hodnoty oddělené tabulátory (TSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="9956c-300">*Extractors.Tsv()*: Provides extraction from tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="9956c-301">Může být užitečné pro vývoj vlastních Extraktor.</span><span class="sxs-lookup"><span data-stu-id="9956c-301">It can be useful to develop a custom extractor.</span></span> <span data-ttu-id="9956c-302">To může být užitečné při importu dat Pokud nám chcete provádět žádnou z následujících úloh:</span><span class="sxs-lookup"><span data-stu-id="9956c-302">This can be helpful during data import if we want to do any of the following tasks:</span></span>

* <span data-ttu-id="9956c-303">Upravte vstupní data tak, že rozdělení sloupců a změnou jednotlivé hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9956c-303">Modify input data by splitting columns and modifying individual values.</span></span> <span data-ttu-id="9956c-304">Funkce procesoru je lepší pro kombinace sloupců.</span><span class="sxs-lookup"><span data-stu-id="9956c-304">The PROCESSOR functionality is better for combining columns.</span></span>
* <span data-ttu-id="9956c-305">Analyzovat Nestrukturovaná data, jako jsou webové stránky a e-mailů nebo polovičním nestrukturovaných dat, jako je například XML nebo JSON.</span><span class="sxs-lookup"><span data-stu-id="9956c-305">Parse unstructured data such as Web pages and emails, or semi-unstructured data such as XML/JSON.</span></span>
* <span data-ttu-id="9956c-306">Analyzovat data v kódování nepodporovaný.</span><span class="sxs-lookup"><span data-stu-id="9956c-306">Parse data in unsupported encoding.</span></span>

<span data-ttu-id="9956c-307">K definování vlastní Extraktor nebo OUČIT, je potřeba vytvořit `IExtractor` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-307">To define a user-defined extractor, or UDE, we need to create an `IExtractor` interface.</span></span> <span data-ttu-id="9956c-308">Všechny vstupní parametry pro extrakci, jako je například sloupce či řádku oddělovače a kódování, musí být definované v konstruktoru třídy.</span><span class="sxs-lookup"><span data-stu-id="9956c-308">All input parameters to the extractor, such as column/row delimiters, and encoding, need to be defined in the constructor of the class.</span></span> <span data-ttu-id="9956c-309">`IExtractor` Rozhraní musí také obsahovat definice pro `IEnumerable<IRow>` přepsat následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9956c-309">The `IExtractor`  interface should also contain a definition for the `IEnumerable<IRow>` override as follows:</span></span>

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

<span data-ttu-id="9956c-310">**SqlUserDefinedExtractor** atribut uvádí, že typ by měl být registrován jako Extraktor se uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-310">The **SqlUserDefinedExtractor** attribute indicates that the type should be registered as a user-defined extractor.</span></span> <span data-ttu-id="9956c-311">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-311">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-312">SqlUserDefinedExtractor je volitelný atribut pro OUČIT definici.</span><span class="sxs-lookup"><span data-stu-id="9956c-312">SqlUserDefinedExtractor is an optional attribute for UDE definition.</span></span> <span data-ttu-id="9956c-313">Ji používá k definování vlastností AtomicFileProcessing OUČIT objektu.</span><span class="sxs-lookup"><span data-stu-id="9956c-313">It used to define AtomicFileProcessing property for the UDE object.</span></span>

* <span data-ttu-id="9956c-314">BOOL AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="9956c-314">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="9956c-315">**Hodnota TRUE,** = označuje, že tento Extraktor vyžaduje atomic vstupní soubory (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="9956c-315">**true** = Indicates that this extractor requires atomic input files (JSON, XML, ...)</span></span>
* <span data-ttu-id="9956c-316">**false** = označuje, že tento Extraktor můžete řešit rozdělení / distribuované soubory (sdíleného svazku clusteru, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="9956c-316">**false** = Indicates that this extractor can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="9956c-317">Hlavní objekty programovatelnosti OUČIT **vstupní** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="9956c-317">The main UDE programmability objects are **input** and **output**.</span></span> <span data-ttu-id="9956c-318">Vstupní objekt se používá k vytvoření výčtu vstupní data jako `IUnstructuredReader`.</span><span class="sxs-lookup"><span data-stu-id="9956c-318">The input object is used to enumerate input data as `IUnstructuredReader`.</span></span> <span data-ttu-id="9956c-319">Objekt výstup se používá nastavit výstupní data v důsledku Extraktor aktivity.</span><span class="sxs-lookup"><span data-stu-id="9956c-319">The output object is used to set output data as a result of the extractor activity.</span></span>

<span data-ttu-id="9956c-320">Vstupní data přistupuje prostřednictvím `System.IO.Stream` a `System.IO.StreamReader`.</span><span class="sxs-lookup"><span data-stu-id="9956c-320">The input data is accessed through `System.IO.Stream` and `System.IO.StreamReader`.</span></span>

<span data-ttu-id="9956c-321">Pro vstupní sloupce výčet nám nejdřív rozdělení vstupního datového proudu pomocí oddělovač řádků.</span><span class="sxs-lookup"><span data-stu-id="9956c-321">For input columns enumeration, we first split the input stream by using a row delimiter.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

<span data-ttu-id="9956c-322">Další rozdělte pak vstupní řádek na části sloupce.</span><span class="sxs-lookup"><span data-stu-id="9956c-322">Then, further split input row into column parts.</span></span>

```
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

<span data-ttu-id="9956c-323">Pokud chcete nastavit výstupní data, použijeme `output.Set` metoda.</span><span class="sxs-lookup"><span data-stu-id="9956c-323">To set output data, we use the `output.Set` method.</span></span>

<span data-ttu-id="9956c-324">Je důležité si uvědomit, že vlastní Extraktor pouze výstupy sloupců a hodnot, které jsou definovány s výstup.</span><span class="sxs-lookup"><span data-stu-id="9956c-324">It's important to understand that the custom extractor only outputs columns and values that are defined with the output.</span></span> <span data-ttu-id="9956c-325">Nastavit volání metody.</span><span class="sxs-lookup"><span data-stu-id="9956c-325">Set method call.</span></span>

```
output.Set<string>(count, part);
```

<span data-ttu-id="9956c-326">Skutečné Extraktor výstup se aktivuje při volání metody `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="9956c-326">The actual extractor output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="9956c-327">Následuje příklad Extraktor:</span><span class="sxs-lookup"><span data-stu-id="9956c-327">Following is the extractor example:</span></span>

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
         //Read the input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split the input by the column delimiter
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
                 // for column “user”, convert to UPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep the rest of the columns as-is
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

<span data-ttu-id="9956c-328">V tomto scénáři případ použití Extraktor regeneruje identifikátor GUID pro sloupec "guid" a převede k hodnotám sloupce "user" na velká písmena.</span><span class="sxs-lookup"><span data-stu-id="9956c-328">In this use-case scenario, the extractor regenerates the GUID for “guid” column and converts the values of “user” column to upper case.</span></span> <span data-ttu-id="9956c-329">Vlastní extraktory může vytvářet složitější výsledky analýzou vstupní data a manipulaci.</span><span class="sxs-lookup"><span data-stu-id="9956c-329">Custom extractors can produce more complicated results by parsing input data and manipulating it.</span></span>

<span data-ttu-id="9956c-330">Toto je základní skript U-SQL, která používá vlastní Extraktor:</span><span class="sxs-lookup"><span data-stu-id="9956c-330">Following is base U-SQL script that uses a custom extractor:</span></span>

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

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-outputters"></a><span data-ttu-id="9956c-331">Použít outputters uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="9956c-331">Use user-defined outputters</span></span>
<span data-ttu-id="9956c-332">Uživatelem definované outputter je jiný UDO U-SQL, který vám umožní rozšiřovat integrovanou funkci U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-332">User-defined outputter is another U-SQL UDO that allows you to extend built-in U-SQL functionality.</span></span> <span data-ttu-id="9956c-333">Podobně jako Extraktor, existuje několik předdefinovaných outputters.</span><span class="sxs-lookup"><span data-stu-id="9956c-333">Similar to the extractor, there are several built-in outputters.</span></span>

* <span data-ttu-id="9956c-334">*Outputters.Text()*: zapisuje data do textových souborů s oddělovači různých kódování.</span><span class="sxs-lookup"><span data-stu-id="9956c-334">*Outputters.Text()*: Writes data to delimited text files of different encodings.</span></span>
* <span data-ttu-id="9956c-335">*Outputters.Csv()*: zapisuje data do různých kódování na soubory textový soubor s oddělovači (CSV).</span><span class="sxs-lookup"><span data-stu-id="9956c-335">*Outputters.Csv()*: Writes data to comma-separated value (CSV) files of different encodings.</span></span>
* <span data-ttu-id="9956c-336">*Outputters.Tsv()*: zapisuje data do hodnoty oddělené tabulátory (TSV) soubory různá kódování.</span><span class="sxs-lookup"><span data-stu-id="9956c-336">*Outputters.Tsv()*: Writes data to tab-separated value (TSV) files of different encodings.</span></span>

<span data-ttu-id="9956c-337">Vlastní outputter umožňuje zapisovat data ve vlastním formátu definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-337">Custom outputter allows you to write data in a custom defined format.</span></span> <span data-ttu-id="9956c-338">To může být užitečné pro následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="9956c-338">This can be useful for the following tasks:</span></span>

* <span data-ttu-id="9956c-339">Zápis dat do částečně strukturovaných nebo nestrukturovaných souborů.</span><span class="sxs-lookup"><span data-stu-id="9956c-339">Writing data to semi-structured or unstructured files.</span></span>
* <span data-ttu-id="9956c-340">Zápis dat není podporováno kódování.</span><span class="sxs-lookup"><span data-stu-id="9956c-340">Writing data not supported encodings.</span></span>
* <span data-ttu-id="9956c-341">Úprava výstupní data nebo přidání vlastních atributů.</span><span class="sxs-lookup"><span data-stu-id="9956c-341">Modifying output data or adding custom attributes.</span></span>

<span data-ttu-id="9956c-342">K definování outputter definovaný uživatelem, je potřeba vytvořit `IOutputter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-342">To define user-defined outputter, we need to create the `IOutputter` interface.</span></span>

<span data-ttu-id="9956c-343">Toto je základní `IOutputter` implementaci třídy:</span><span class="sxs-lookup"><span data-stu-id="9956c-343">Following is the base `IOutputter` class implementation:</span></span>

```
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

<span data-ttu-id="9956c-344">Všechny vstupní parametry outputter, jako jsou sloupce či řádku oddělovače, kódování a tak dále, musí být definované v konstruktoru třídy.</span><span class="sxs-lookup"><span data-stu-id="9956c-344">All input parameters to the outputter, such as column/row delimiters, encoding, and so on, need to be defined in the constructor of the class.</span></span> <span data-ttu-id="9956c-345">`IOutputter` Rozhraní musí také obsahovat definice pro `void Output` přepsat.</span><span class="sxs-lookup"><span data-stu-id="9956c-345">The `IOutputter` interface should also contain a definition for `void Output` override.</span></span> <span data-ttu-id="9956c-346">Atribut `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` můžete volitelně můžete nastavit pro zpracování atomic souboru.</span><span class="sxs-lookup"><span data-stu-id="9956c-346">The attribute `[SqlUserDefinedOutputter(AtomicFileProcessing = true)` can optionally be set for atomic file processing.</span></span> <span data-ttu-id="9956c-347">Další informace najdete v tématu následující podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9956c-347">For more information, see the following details.</span></span>

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

* <span data-ttu-id="9956c-348">`Output`je volána pro každý řádek vstupu.</span><span class="sxs-lookup"><span data-stu-id="9956c-348">`Output` is called for each input row.</span></span> <span data-ttu-id="9956c-349">Vrátí `IUnstructuredWriter output` sady řádků.</span><span class="sxs-lookup"><span data-stu-id="9956c-349">It returns the `IUnstructuredWriter output` rowset.</span></span>
* <span data-ttu-id="9956c-350">Konstruktor třída se používá k předat parametry outputter definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9956c-350">The Constructor class is used to pass parameters to the user-defined outputter.</span></span>
* <span data-ttu-id="9956c-351">`Close`slouží k volitelně přepsat verzi nákladné stavu nebo zjistit, kdy byla zapsána poslední řádek.</span><span class="sxs-lookup"><span data-stu-id="9956c-351">`Close` is used to optionally override to release expensive state or determine when the last row was written.</span></span>

<span data-ttu-id="9956c-352">**SqlUserDefinedOutputter** atribut uvádí, že typ by měl být registrován jako outputter se uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-352">**SqlUserDefinedOutputter** attribute indicates that the type should be registered as a user-defined outputter.</span></span> <span data-ttu-id="9956c-353">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-353">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-354">SqlUserDefinedOutputter je volitelný atribut pro definici outputter definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9956c-354">SqlUserDefinedOutputter is an optional attribute for a user-defined outputter definition.</span></span> <span data-ttu-id="9956c-355">Slouží k definování vlastností AtomicFileProcessing.</span><span class="sxs-lookup"><span data-stu-id="9956c-355">It's used to define the AtomicFileProcessing property.</span></span>

* <span data-ttu-id="9956c-356">BOOL AtomicFileProcessing</span><span class="sxs-lookup"><span data-stu-id="9956c-356">bool     AtomicFileProcessing</span></span>   

* <span data-ttu-id="9956c-357">**Hodnota TRUE,** = označuje, že tento outputter vyžaduje atomic výstupní soubory (JSON, XML,...)</span><span class="sxs-lookup"><span data-stu-id="9956c-357">**true** = Indicates that this outputter requires atomic output files (JSON, XML, ...)</span></span>
* <span data-ttu-id="9956c-358">**false** = označuje, že tento outputter můžete řešit rozdělení / distribuované soubory (sdíleného svazku clusteru, SEQ,...)</span><span class="sxs-lookup"><span data-stu-id="9956c-358">**false** = Indicates that this outputter can deal with split / distributed files (CSV, SEQ, ...)</span></span>

<span data-ttu-id="9956c-359">Objekty hlavního programovatelnosti **řádek** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="9956c-359">The main programmability objects are **row** and **output**.</span></span> <span data-ttu-id="9956c-360">**Řádek** objekt se používá k vytvoření výčtu výstupní data jako `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-360">The **row** object is used to enumerate output data as `IRow` interface.</span></span> <span data-ttu-id="9956c-361">**Výstup** slouží k nastavení výstupní data k cílovému souboru.</span><span class="sxs-lookup"><span data-stu-id="9956c-361">**Output** is used to set output data to the target file.</span></span>

<span data-ttu-id="9956c-362">Výstupní data přistupuje prostřednictvím `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-362">The output data is accessed through the `IRow` interface.</span></span> <span data-ttu-id="9956c-363">Výstupní data předávána řádek najednou.</span><span class="sxs-lookup"><span data-stu-id="9956c-363">Output data is passed a row at a time.</span></span>

<span data-ttu-id="9956c-364">Jednotlivé hodnoty jsou uvedené voláním metody Get IRow rozhraní:</span><span class="sxs-lookup"><span data-stu-id="9956c-364">The individual values are enumerated by calling the Get method of the IRow interface:</span></span>

```
row.Get<string>("column_name")
```

<span data-ttu-id="9956c-365">Názvy jednotlivých sloupců se dá určit pomocí volání `row.Schema`:</span><span class="sxs-lookup"><span data-stu-id="9956c-365">Individual column names can be determined by calling `row.Schema`:</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="9956c-366">Tento přístup umožňuje vytvořit flexibilní outputter pro žádné schéma metadat.</span><span class="sxs-lookup"><span data-stu-id="9956c-366">This approach enables you to build a flexible outputter for any metadata schema.</span></span>

<span data-ttu-id="9956c-367">Výstupní data je zapsán do souboru pomocí `System.IO.StreamWriter`.</span><span class="sxs-lookup"><span data-stu-id="9956c-367">The output data is written to file by using `System.IO.StreamWriter`.</span></span> <span data-ttu-id="9956c-368">Parametr datový proud je nastaven na `output.BaseStrea` jako součást `IUnstructuredWriter output`.</span><span class="sxs-lookup"><span data-stu-id="9956c-368">The stream parameter is set to `output.BaseStrea` as part of `IUnstructuredWriter output`.</span></span>

<span data-ttu-id="9956c-369">Všimněte si, že je důležité, abyste po každé iteraci řádek vyprázdní vyrovnávací paměť dat do souboru.</span><span class="sxs-lookup"><span data-stu-id="9956c-369">Note that it's important to flush the data buffer to the file after each row iteration.</span></span> <span data-ttu-id="9956c-370">Kromě toho `StreamWriter` objekt musí použít s povoleno uvolnitelná atribut (výchozí) a **pomocí** – klíčové slovo:</span><span class="sxs-lookup"><span data-stu-id="9956c-370">In addition, the `StreamWriter` object must be used with the Disposable attribute enabled (default) and with the **using** keyword:</span></span>

```
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

<span data-ttu-id="9956c-371">Jinak volejte metodu Flush() explicitně po každé iteraci.</span><span class="sxs-lookup"><span data-stu-id="9956c-371">Otherwise, call Flush() method explicitly after each iteration.</span></span> <span data-ttu-id="9956c-372">Nemůžeme zobrazit v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="9956c-372">We show this in the following example.</span></span>

### <a name="set-headers-and-footers-for-user-defined-outputter"></a><span data-ttu-id="9956c-373">Nastavte záhlaví a zápatí pro uživatelem definované outputter</span><span class="sxs-lookup"><span data-stu-id="9956c-373">Set headers and footers for user-defined outputter</span></span>
<span data-ttu-id="9956c-374">Pokud chcete nastavit hlavičku, použijte jednu iterace provádění toku.</span><span class="sxs-lookup"><span data-stu-id="9956c-374">To set a header, use single iteration execution flow.</span></span>

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

<span data-ttu-id="9956c-375">Kód v prvním `if (isHeaderRow)` bloku se spustí jenom jednou.</span><span class="sxs-lookup"><span data-stu-id="9956c-375">The code in the first `if (isHeaderRow)` block is executed only once.</span></span>

<span data-ttu-id="9956c-376">Pro zápatí stránky, použijte odkaz na instanci `System.IO.Stream` objektu (`output.BaseStream`).</span><span class="sxs-lookup"><span data-stu-id="9956c-376">For the footer, use the reference to the instance of `System.IO.Stream` object (`output.BaseStream`).</span></span> <span data-ttu-id="9956c-377">Zápis zápatí v metodě Close() `IOutputter` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-377">Write the footer in the Close() method of the `IOutputter` interface.</span></span>  <span data-ttu-id="9956c-378">(Další informace najdete v následujícím příkladu.)</span><span class="sxs-lookup"><span data-stu-id="9956c-378">(For more information, see the following example.)</span></span>

<span data-ttu-id="9956c-379">Následuje příklad uživatelem definované outputter:</span><span class="sxs-lookup"><span data-stu-id="9956c-379">Following is an example of a user-defined outputter:</span></span>

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

    // The Close method is used to write the footer to the file. It's executed only once, after all rows
    public override void Close().
    {
    //Reference to IO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization to enumerate column names
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
        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
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
    // Reference to the instance of the IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define the factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

<span data-ttu-id="9956c-380">A základní skript U-SQL:</span><span class="sxs-lookup"><span data-stu-id="9956c-380">And U-SQL base script:</span></span>

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
    TO @output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="9956c-381">Toto je outputter HTML, který vytvoří soubor HTML s dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="9956c-381">This is an HTML outputter, which creates an HTML file with table data.</span></span>

### <a name="call-outputter-from-u-sql-base-script"></a><span data-ttu-id="9956c-382">Volání outputter ze základní skriptu U-SQL</span><span class="sxs-lookup"><span data-stu-id="9956c-382">Call outputter from U-SQL base script</span></span>
<span data-ttu-id="9956c-383">Vlastní outputter volat z základní skript U-SQL, je potřeba vytvořit novou instanci objektu outputter.</span><span class="sxs-lookup"><span data-stu-id="9956c-383">To call a custom outputter from the base U-SQL script, the new instance of the outputter object has to be created.</span></span>

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

<span data-ttu-id="9956c-384">Abyste se vyhnuli, vytvoření instance objektu v základní skriptu, můžeme vytvořit funkce obálku, jak je znázorněno v našem příkladu starší:</span><span class="sxs-lookup"><span data-stu-id="9956c-384">To avoid creating an instance of the object in base script, we can create a function wrapper, as shown in our earlier example:</span></span>

```c#
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

<span data-ttu-id="9956c-385">V takovém případě původní volání vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9956c-385">In this case, the original call looks like the following:</span></span>

```
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="use-user-defined-processors"></a><span data-ttu-id="9956c-386">Používají procesory uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="9956c-386">Use user-defined processors</span></span>
<span data-ttu-id="9956c-387">Uživatelem definované procesoru nebo UDP, je typ UDO U-SQL, která umožňuje použití funkcí programovatelnosti zpracování příchozí řádky.</span><span class="sxs-lookup"><span data-stu-id="9956c-387">User-defined processor, or UDP, is a type of U-SQL UDO that enables you to process the incoming rows by applying programmability features.</span></span> <span data-ttu-id="9956c-388">UDP umožňuje sloučení sloupců, upravte hodnoty a v případě potřeby přidejte nové sloupce.</span><span class="sxs-lookup"><span data-stu-id="9956c-388">UDP enables you to combine columns, modify values, and add new columns if necessary.</span></span> <span data-ttu-id="9956c-389">V podstatě je dobré se zpracovat sadu řádků k vytvoření požadované datové prvky.</span><span class="sxs-lookup"><span data-stu-id="9956c-389">Basically, it helps to process a rowset to produce required data elements.</span></span>

<span data-ttu-id="9956c-390">K definování UDP, je potřeba vytvořit `IProcessor` rozhraní s `SqlUserDefinedProcessor` atribut, který je pro UDP volitelné.</span><span class="sxs-lookup"><span data-stu-id="9956c-390">To define a UDP, we need to create an `IProcessor` interface with the `SqlUserDefinedProcessor` attribute, which is optional for UDP.</span></span>

<span data-ttu-id="9956c-391">Toto rozhraní by měl obsahovat definici `IRow` rozhraní řádků přepsat, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9956c-391">This interface should contain the definition for the `IRow` interface rowset override, as shown in the following example:</span></span>

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

<span data-ttu-id="9956c-392">**SqlUserDefinedProcessor** označuje, že typ by měl být registrován jako procesor uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-392">**SqlUserDefinedProcessor** indicates that the type should be registered as a user-defined processor.</span></span> <span data-ttu-id="9956c-393">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-393">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-394">Je atribut SqlUserDefinedProcessor **volitelné** pro definici UDP.</span><span class="sxs-lookup"><span data-stu-id="9956c-394">The SqlUserDefinedProcessor attribute is **optional** for UDP definition.</span></span>

<span data-ttu-id="9956c-395">Objekty hlavního programovatelnosti **vstupní** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="9956c-395">The main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="9956c-396">Vstupní objekt se používá výčet vstupní sloupce a výstup a nastavit výstupní data v důsledku činnosti procesoru.</span><span class="sxs-lookup"><span data-stu-id="9956c-396">The input object is used to enumerate input columns and output, and to set output data as a result of the processor activity.</span></span>

<span data-ttu-id="9956c-397">Pro vstupní sloupce výčet používáme `input.Get` metoda.</span><span class="sxs-lookup"><span data-stu-id="9956c-397">For input columns enumeration, we use the `input.Get` method.</span></span>

```
string column_name = input.Get<string>("column_name");
```

<span data-ttu-id="9956c-398">Parametr pro `input.Get` metoda je sloupec, který se předá jako součást `PRODUCE` klauzuli `PROCESS` prohlášení o základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-398">The parameter for `input.Get` method is a column that's passed as part of the `PRODUCE` clause of the `PROCESS` statement of the U-SQL base script.</span></span> <span data-ttu-id="9956c-399">Je potřeba použít na správný typ. zde.</span><span class="sxs-lookup"><span data-stu-id="9956c-399">We need to use the correct data type here.</span></span>

<span data-ttu-id="9956c-400">Pro výstup, použijte `output.Set` metoda.</span><span class="sxs-lookup"><span data-stu-id="9956c-400">For output, use the `output.Set` method.</span></span>

<span data-ttu-id="9956c-401">Je důležité si uvědomit, že vlastní výrobce pouze výstupy sloupců a hodnot, které jsou definovány pomocí `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="9956c-401">It's important to note that custom producer only outputs columns and values that are defined with the `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", mycolumn);
```

<span data-ttu-id="9956c-402">Skutečné procesoru výstup se aktivuje při volání metody `return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="9956c-402">The actual processor output is triggered by calling `return output.AsReadOnly();`.</span></span>

<span data-ttu-id="9956c-403">Následuje příklad procesoru:</span><span class="sxs-lookup"><span data-stu-id="9956c-403">Following is a processor example:</span></span>

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

<span data-ttu-id="9956c-404">V tomto scénáři případ použití procesor generuje nový sloupec s názvem "full_description" kombinací existujícího sloupce – v tomto případě "user" velká a "des".</span><span class="sxs-lookup"><span data-stu-id="9956c-404">In this use-case scenario, the processor is generating a new column called “full_description” by combining the existing columns--in this case, “user” in upper case, and “des”.</span></span> <span data-ttu-id="9956c-405">Také znovu vytvoří identifikátor GUID a vrátí původní a nové hodnoty identifikátor GUID.</span><span class="sxs-lookup"><span data-stu-id="9956c-405">It also regenerates a GUID and returns the original and new GUID values.</span></span>

<span data-ttu-id="9956c-406">Jak vidíte z předchozího příkladu, můžete volat metody C# během `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="9956c-406">As you can see from the previous example, you can call C# methods during `output.Set` method call.</span></span>

<span data-ttu-id="9956c-407">Tady je příklad základní skriptu U-SQL, který používá vlastního procesoru:</span><span class="sxs-lookup"><span data-stu-id="9956c-407">Following is an example of base U-SQL script that uses a custom processor:</span></span>

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

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

## <a name="use-user-defined-appliers"></a><span data-ttu-id="9956c-408">Použít appliers uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="9956c-408">Use user-defined appliers</span></span>
<span data-ttu-id="9956c-409">Uživatelem definované applier U-SQL umožňuje vyvolání vlastní C# funkce pro každý řádek, který je vrácen vnější tabulky výraz dotazu.</span><span class="sxs-lookup"><span data-stu-id="9956c-409">A U-SQL user-defined applier enables you to invoke a custom C# function for each row that's returned by the outer table expression of a query.</span></span> <span data-ttu-id="9956c-410">Správný vstup je vyhodnotit pro každý řádek z levého vstup a řádky, které vytváří spojují pro finální výstup.</span><span class="sxs-lookup"><span data-stu-id="9956c-410">The right input is evaluated for each row from the left input, and the rows that are produced are combined for the final output.</span></span> <span data-ttu-id="9956c-411">Seznam sloupců, které jsou vytvářeny v operátor APPLY jsou kombinace sadu sloupců v vlevo a vpravo vstup.</span><span class="sxs-lookup"><span data-stu-id="9956c-411">The list of columns that are produced by the APPLY operator are the combination of the set of columns in the left and the right input.</span></span>

<span data-ttu-id="9956c-412">Uživatelem definované applier je volaná jako součást výrazu USQL vyberte.</span><span class="sxs-lookup"><span data-stu-id="9956c-412">User-defined applier is being invoked as part of the USQL SELECT expression.</span></span>

<span data-ttu-id="9956c-413">Typické volání applier uživatelem definované vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9956c-413">The typical call to the user-defined applier looks like the following:</span></span>

```
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

<span data-ttu-id="9956c-414">Další informace o používání appliers ve výrazu SELECT, najdete v části [U-SQL vyberte výběr z křížové použít a vnější použít](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span><span class="sxs-lookup"><span data-stu-id="9956c-414">For more information about using appliers in a SELECT expression, see [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](https://msdn.microsoft.com/library/azure/mt621307.aspx).</span></span>

<span data-ttu-id="9956c-415">Definice uživatelem definované applier základní třídy je následující:</span><span class="sxs-lookup"><span data-stu-id="9956c-415">The user-defined applier base class definition is as follows:</span></span>

```
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

<span data-ttu-id="9956c-416">K definování applier se definovaný uživatelem, je potřeba vytvořit `IApplier` rozhraní s [`SqlUserDefinedApplier`] atribut, který je pro uživatelem definované applier definice volitelné.</span><span class="sxs-lookup"><span data-stu-id="9956c-416">To define a user-defined applier, we need to create the `IApplier` interface with the [`SqlUserDefinedApplier`] attribute, which is optional for a user-defined applier definition.</span></span>

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

* <span data-ttu-id="9956c-417">Použít je volána pro každý řádek vnější tabulky.</span><span class="sxs-lookup"><span data-stu-id="9956c-417">Apply is called for each row of the outer table.</span></span> <span data-ttu-id="9956c-418">Vrátí `IUpdatableRow` výstup sady řádků.</span><span class="sxs-lookup"><span data-stu-id="9956c-418">It returns the `IUpdatableRow` output rowset.</span></span>
* <span data-ttu-id="9956c-419">Konstruktor třída se používá k předat parametry applier definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9956c-419">The Constructor class is used to pass parameters to the user-defined applier.</span></span>

<span data-ttu-id="9956c-420">**SqlUserDefinedApplier** označuje, že typ by měl být registrován jako applier se uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-420">**SqlUserDefinedApplier** indicates that the type should be registered as a user-defined applier.</span></span> <span data-ttu-id="9956c-421">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-421">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-422">**SqlUserDefinedApplier** je **volitelné** pro definici applier definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9956c-422">**SqlUserDefinedApplier** is **optional** for a user-defined applier definition.</span></span>


<span data-ttu-id="9956c-423">Objekty hlavního programovatelnosti jsou následující:</span><span class="sxs-lookup"><span data-stu-id="9956c-423">The main programmability objects are as follows:</span></span>

```
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

<span data-ttu-id="9956c-424">Vstupní sady řádků se předávají jako `IRow` vstupní.</span><span class="sxs-lookup"><span data-stu-id="9956c-424">Input rowsets are passed as `IRow` input.</span></span> <span data-ttu-id="9956c-425">Výstup řádky jsou generovány jako `IUpdatableRow` rozhraní výstup.</span><span class="sxs-lookup"><span data-stu-id="9956c-425">The output rows are generated as `IUpdatableRow` output interface.</span></span>

<span data-ttu-id="9956c-426">Názvy jednotlivých sloupců se dá určit pomocí volání `IRow` metoda schématu.</span><span class="sxs-lookup"><span data-stu-id="9956c-426">Individual column names can be determined by calling the `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="9956c-427">Chcete-li získat skutečný datových hodnot z příchozích `IRow`, jsme použijte metodu Get() `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-427">To get the actual data values from the incoming `IRow`, we use the Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="9956c-428">Nebo používáme schématu název sloupce:</span><span class="sxs-lookup"><span data-stu-id="9956c-428">Or we use the schema column name:</span></span>

```
row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="9956c-429">Výstupní hodnoty musí být nastavena s `IUpdatableRow` výstup:</span><span class="sxs-lookup"><span data-stu-id="9956c-429">The output values must be set with `IUpdatableRow` output:</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="9956c-430">Je důležité si uvědomit, vlastní appliers pouze výstup sloupců a hodnot, které jsou definovány s `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="9956c-430">It is important to understand that custom appliers only output columns and values that are defined with `output.Set` method call.</span></span>

<span data-ttu-id="9956c-431">Skutečný výstup se aktivuje při volání metody `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="9956c-431">The actual output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="9956c-432">Uživatelem definované applier parametry mohou být předaný konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="9956c-432">The user-defined applier parameters can be passed to the constructor.</span></span> <span data-ttu-id="9956c-433">Applier může vrátit proměnný počet sloupců, které musí být definované během applier volání v základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-433">Applier can return a variable number of columns that need to be defined during the applier call in base U-SQL Script.</span></span>

```
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

<span data-ttu-id="9956c-434">Tady je příklad applier uživatelem definované:</span><span class="sxs-lookup"><span data-stu-id="9956c-434">Here is the user-defined applier example:</span></span>

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

<span data-ttu-id="9956c-435">Toto je základní skript U-SQL pro tento uživatelem definované applier:</span><span class="sxs-lookup"><span data-stu-id="9956c-435">Following is the base U-SQL script for this user-defined applier:</span></span>

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

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

<span data-ttu-id="9956c-436">V tomto scénáři případu použití uživatelem definované applier funguje jako analyzátor oddělených čárkou pro vlastnosti loďstva Auto.</span><span class="sxs-lookup"><span data-stu-id="9956c-436">In this use case scenario, user-defined applier acts as a comma-delimited value parser for the car fleet properties.</span></span> <span data-ttu-id="9956c-437">Vstupní soubor řádky vypadat následovně:</span><span class="sxs-lookup"><span data-stu-id="9956c-437">The input file rows look like the following:</span></span>

```
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Shevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

<span data-ttu-id="9956c-438">Je typické oddělený tabulátory TSV soubor s vlastností sloupec, který obsahuje vlastnosti car například výrobce a model.</span><span class="sxs-lookup"><span data-stu-id="9956c-438">It is a typical tab-delimited TSV file with a properties column that contains car properties such as make and model.</span></span> <span data-ttu-id="9956c-439">Tyto vlastnosti musí být analyzován na sloupce tabulky.</span><span class="sxs-lookup"><span data-stu-id="9956c-439">Those properties must be parsed to the table columns.</span></span> <span data-ttu-id="9956c-440">Applier, který je k dispozici také umožňuje vygenerovat dynamické počet vlastností v dané sadě řádků výsledek, na základě parametru, který je předán.</span><span class="sxs-lookup"><span data-stu-id="9956c-440">The applier that's provided also enables you to generate a dynamic number of properties in the result rowset, based on the parameter that's passed.</span></span> <span data-ttu-id="9956c-441">Můžete vygenerovat buď všechny vlastnosti, nebo konkrétní sadu pouze vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="9956c-441">You can generate either all properties or a specific set of properties only.</span></span>

    …USQL_Programmability.ParserApplier ("all")
    …USQL_Programmability.ParserApplier ("make")
    …USQL_Programmability.ParserApplier ("make&model")

<span data-ttu-id="9956c-442">Uživatelem definované applier lze volat jako novou instanci třídy applier objektu:</span><span class="sxs-lookup"><span data-stu-id="9956c-442">The user-defined applier can be called as a new instance of applier object:</span></span>

```
CROSS APPLY new MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

<span data-ttu-id="9956c-443">Nebo pomocí volání metody vytváření obálky:</span><span class="sxs-lookup"><span data-stu-id="9956c-443">Or with the invocation of a wrapper factory method:</span></span>

```c#
    CROSS APPLY MyNameSpace.MyApplier (parameter: “value”) AS alias([columns types]…);
```

## <a name="use-user-defined-combiners"></a><span data-ttu-id="9956c-444">Použít combiners uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="9956c-444">Use user-defined combiners</span></span>
<span data-ttu-id="9956c-445">Uživatelem definované kombinační nebo UDC, můžete kombinovat řádky z levé a pravé sady řádků, na základě vlastní logiky.</span><span class="sxs-lookup"><span data-stu-id="9956c-445">User-defined combiner, or UDC, enables you to combine rows from left and right rowsets, based on custom logic.</span></span> <span data-ttu-id="9956c-446">Uživatelem definované kombinační se používá s KOMBINAČNÍ výraz.</span><span class="sxs-lookup"><span data-stu-id="9956c-446">User-defined combiner is used with COMBINE expression.</span></span>

<span data-ttu-id="9956c-447">Kombinační je volaná s KOMBINAČNÍ výraz, který obsahuje potřebné informace o jak vstupní sady řádků, sloupců seskupení, očekávaný výsledek schématu a další informace.</span><span class="sxs-lookup"><span data-stu-id="9956c-447">A combiner is being invoked with the COMBINE expression that provides the necessary information about both the input rowsets, the grouping columns, the expected result schema, and additional information.</span></span>

<span data-ttu-id="9956c-448">Volat kombinační v základní skript U-SQL, můžeme použít následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="9956c-448">To call a combiner in a base U-SQL script, we use the following syntax:</span></span>

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

<span data-ttu-id="9956c-449">Další informace najdete v tématu [KOMBINOVAT výrazu (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span><span class="sxs-lookup"><span data-stu-id="9956c-449">For more information, see [COMBINE Expression (U-SQL)](https://msdn.microsoft.com/library/azure/mt621339.aspx).</span></span>

<span data-ttu-id="9956c-450">K definování kombinační se definovaný uživatelem, je potřeba vytvořit `ICombiner` rozhraní s [`SqlUserDefinedCombiner`] atribut, který je pro definici uživatelem definované kombinační volitelné.</span><span class="sxs-lookup"><span data-stu-id="9956c-450">To define a user-defined combiner, we need to create the `ICombiner` interface with the [`SqlUserDefinedCombiner`] attribute, which is optional for a user-defined Combiner definition.</span></span>

<span data-ttu-id="9956c-451">Základní `ICombiner` definici třídy:</span><span class="sxs-lookup"><span data-stu-id="9956c-451">Base `ICombiner` class definition:</span></span>

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

<span data-ttu-id="9956c-452">Vlastní implementace `ICombiner` rozhraní by měl obsahovat definici `IEnumerable<IRow>` kombinovat přepsání.</span><span class="sxs-lookup"><span data-stu-id="9956c-452">The custom implementation of an `ICombiner` interface should contain the definition for an `IEnumerable<IRow>` Combine override.</span></span>

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

<span data-ttu-id="9956c-453">**SqlUserDefinedCombiner** atribut uvádí, že typ by měl být registrován jako kombinační se uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-453">The **SqlUserDefinedCombiner** attribute indicates that the type should be registered as a user-defined combiner.</span></span> <span data-ttu-id="9956c-454">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-454">This class cannot be inherited.</span></span>

<span data-ttu-id="9956c-455">**SqlUserDefinedCombiner** se používá k definování vlastnost kombinační režimu.</span><span class="sxs-lookup"><span data-stu-id="9956c-455">**SqlUserDefinedCombiner** is used to define the Combiner mode property.</span></span> <span data-ttu-id="9956c-456">Je volitelný atribut pro definici kombinační definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9956c-456">It is an optional attribute for a user-defined combiner definition.</span></span>

<span data-ttu-id="9956c-457">Režim CombinerMode</span><span class="sxs-lookup"><span data-stu-id="9956c-457">CombinerMode     Mode</span></span>

<span data-ttu-id="9956c-458">Výčet CombinerMode provést následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="9956c-458">CombinerMode enum can take the following values:</span></span>

* <span data-ttu-id="9956c-459">Úplné (0), každý řádek výstupu potenciálně závisí na všechny vstupní řádky z levé a pravé se stejnou hodnotou klíče.</span><span class="sxs-lookup"><span data-stu-id="9956c-459">Full  (0) Every output row potentially depends on all the input rows from left and right       with the same key value.</span></span>

* <span data-ttu-id="9956c-460">Doleva (1) každý řádek výstupu závisí na jeden vstupní řádek z doleva (a potenciálně všechny řádky z pravé se stejnou hodnotou klíče).</span><span class="sxs-lookup"><span data-stu-id="9956c-460">Left  (1) Every output row depends on a single input row from the left (and potentially all rows       from the right with the same key value).</span></span>

* <span data-ttu-id="9956c-461">Práva (2) každý řádek výstupu závisí na jeden vstupní řádek z vpravo (a potenciálně všechny řádky z levé straně se stejnou hodnotou klíče).</span><span class="sxs-lookup"><span data-stu-id="9956c-461">Right (2)     Every output row depends on a single input row from the right (and potentially all rows       from the left with the same key value).</span></span>

* <span data-ttu-id="9956c-462">Vnitřní (3), každý řádek výstupu závisí na jeden řádek vstupu z doleva a doprava a mají stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9956c-462">Inner (3) Every output row depends on a single input row from left and right with the same value.</span></span>

<span data-ttu-id="9956c-463">Příklad: [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span><span class="sxs-lookup"><span data-stu-id="9956c-463">Example:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]</span></span>


<span data-ttu-id="9956c-464">Objekty hlavního programovatelnosti jsou:</span><span class="sxs-lookup"><span data-stu-id="9956c-464">The main programmability objects are:</span></span>

```c#
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

<span data-ttu-id="9956c-465">Vstupní sady řádků se předávají jako **levém** a **správné** `IRowset` typu rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-465">Input rowsets are passed as **left** and **right** `IRowset` type of interface.</span></span> <span data-ttu-id="9956c-466">Obě sady řádků musí být ve výčtu objevit pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="9956c-466">Both rowsets must be enumerated for processing.</span></span> <span data-ttu-id="9956c-467">Každé rozhraní můžete pouze výčet jednou, abychom měli vytvořit výčet a uložení do mezipaměti v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9956c-467">You can only enumerate each interface once, so we have to enumerate and cache it if necessary.</span></span>

<span data-ttu-id="9956c-468">Pro ukládání do mezipaměti pro účely, můžeme vytvořit seznam\<T\> typ konstrukce paměti v důsledku LINQ spuštění dotazu, konkrétně seznam <`IRow`>.</span><span class="sxs-lookup"><span data-stu-id="9956c-468">For caching purposes, we can create a List\<T\> type of memory structure as a result of a LINQ query execution, specifically List<`IRow`>.</span></span> <span data-ttu-id="9956c-469">Anonymní datový typ lze během výčtu také.</span><span class="sxs-lookup"><span data-stu-id="9956c-469">The anonymous data type can be used during enumeration as well.</span></span>

<span data-ttu-id="9956c-470">V tématu [Úvod do dotazů LINQ (C#)](https://msdn.microsoft.com/library/bb397906.aspx) Další informace o dotazech LINQ a [rozhraní IEnumerable\<T\> rozhraní](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) Další informace o rozhraní IEnumerable\<T\> rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-470">See [Introduction to LINQ Queries (C#)](https://msdn.microsoft.com/library/bb397906.aspx) for more information about LINQ queries, and [IEnumerable\<T\> Interface](https://msdn.microsoft.com/library/9eekhta0(v=vs.110).aspx) for more information about IEnumerable\<T\> interface.</span></span>

<span data-ttu-id="9956c-471">Chcete-li získat skutečný datových hodnot z příchozích `IRowset`, jsme použijte metodu Get() `IRow` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9956c-471">To get the actual data values from the incoming `IRowset`, we use the Get() method of `IRow` interface.</span></span>

```
mycolumn = row.Get<int>("mycolumn")
```

<span data-ttu-id="9956c-472">Názvy jednotlivých sloupců se dá určit pomocí volání `IRow` metoda schématu.</span><span class="sxs-lookup"><span data-stu-id="9956c-472">Individual column names can be determined by calling the `IRow` Schema method.</span></span>

```
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

<span data-ttu-id="9956c-473">Nebo pomocí názvu sloupce schématu:</span><span class="sxs-lookup"><span data-stu-id="9956c-473">Or by using the schema column name:</span></span>

```
c# row.Get<int>(row.Schema[0].Name)
```

<span data-ttu-id="9956c-474">Obecné výčtu s dotazy LINQ vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="9956c-474">The general enumeration with LINQ looks like the following:</span></span>

```
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

<span data-ttu-id="9956c-475">Po vytvoření výčtu obě sady řádků, přidáme můžete procházet všechny řádky.</span><span class="sxs-lookup"><span data-stu-id="9956c-475">After enumerating both rowsets, we are going to loop through all rows.</span></span> <span data-ttu-id="9956c-476">Pro každý řádek v levém řádků přidáme najít všechny řádky, které splňují podmínku naše kombinační.</span><span class="sxs-lookup"><span data-stu-id="9956c-476">For each row in the left rowset, we are going to find all rows that satisfy the condition of our combiner.</span></span>

<span data-ttu-id="9956c-477">Výstupní hodnoty musí být nastavena s `IUpdatableRow` výstup.</span><span class="sxs-lookup"><span data-stu-id="9956c-477">The output values must be set with `IUpdatableRow` output.</span></span>

```
output.Set<int>("mycolumn", mycolumn)
```

<span data-ttu-id="9956c-478">Skutečný výstup se aktivuje při volání do `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="9956c-478">The actual output is triggered by calling to `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="9956c-479">Následuje příklad kombinační:</span><span class="sxs-lookup"><span data-stu-id="9956c-479">Following is a combiner example:</span></span>

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

<span data-ttu-id="9956c-480">V tomto scénáři případ použití jsme se vytváření zprávu o analýzy, které prodejce.</span><span class="sxs-lookup"><span data-stu-id="9956c-480">In this use-case scenario, we are building an analytics report for the retailer.</span></span> <span data-ttu-id="9956c-481">Cílem je najít všechny produkty, které náklady více než 20 000 $ a který prodeje prostřednictvím webu rychleji než pomocí regulárních prodejce v určitém časovém rámci.</span><span class="sxs-lookup"><span data-stu-id="9956c-481">The goal is to find all products that cost more than $20,000 and that sell through the website faster than through the regular retailer within a certain time frame.</span></span>

<span data-ttu-id="9956c-482">Zde je základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-482">Here is the base U-SQL script.</span></span> <span data-ttu-id="9956c-483">Můžete porovnat logiku mezi regulárního spojení a kombinační:</span><span class="sxs-lookup"><span data-stu-id="9956c-483">You can compare the logic between a regular JOIN and a combiner:</span></span>

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

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

<span data-ttu-id="9956c-484">Uživatelem definované kombinační lze volat jako novou instanci třídy applier objektu:</span><span class="sxs-lookup"><span data-stu-id="9956c-484">A user-defined combiner can be called as a new instance of the applier object:</span></span>

```
USING new MyNameSpace.MyCombiner();
```


<span data-ttu-id="9956c-485">Nebo pomocí volání metody vytváření obálky:</span><span class="sxs-lookup"><span data-stu-id="9956c-485">Or with the invocation of a wrapper factory method:</span></span>

```
USING MyNameSpace.MyCombiner();
```

## <a name="use-user-defined-reducers"></a><span data-ttu-id="9956c-486">Použít přechodky uživatelem definované</span><span class="sxs-lookup"><span data-stu-id="9956c-486">Use user-defined reducers</span></span>

<span data-ttu-id="9956c-487">U-SQL umožňuje psát vlastní řádků přechodky v jazyce C# s využitím rozhraní rozšiřitelnosti uživatelem definovaný operátor a implementace rozhraní IReducer.</span><span class="sxs-lookup"><span data-stu-id="9956c-487">U-SQL enables you to write custom rowset reducers in C# by using the user-defined operator extensibility framework and implementing an IReducer interface.</span></span>

<span data-ttu-id="9956c-488">Uživatelem definované reduktorem nebo UDR, slouží k vyloučení nepotřebných řádků při extrakci dat (Importovat).</span><span class="sxs-lookup"><span data-stu-id="9956c-488">User-defined reducer, or UDR, can be used to eliminate unnecessary rows during data extraction (import).</span></span> <span data-ttu-id="9956c-489">Je také slouží k manipulaci a vyhodnotit, řádků a sloupců.</span><span class="sxs-lookup"><span data-stu-id="9956c-489">It also can be used to manipulate and evaluate rows and columns.</span></span> <span data-ttu-id="9956c-490">Na základě programovatelnosti logiky, ho můžete také definovat řádky musí být rozbalena.</span><span class="sxs-lookup"><span data-stu-id="9956c-490">Based on programmability logic, it can also define which rows need to be extracted.</span></span>

<span data-ttu-id="9956c-491">Můžete definovat třídu UDR, je potřeba vytvořit `IReducer` rozhraní s volitelný `SqlUserDefinedReducer` atribut.</span><span class="sxs-lookup"><span data-stu-id="9956c-491">To define a UDR class, we need to create an `IReducer` interface with an optional `SqlUserDefinedReducer` attribute.</span></span>

<span data-ttu-id="9956c-492">Tato třída rozhraní by měl obsahovat definici `IEnumerable` přepsat rozhraní sady řádků.</span><span class="sxs-lookup"><span data-stu-id="9956c-492">This class interface should contain a definition for the `IEnumerable` interface rowset override.</span></span>

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

<span data-ttu-id="9956c-493">**SqlUserDefinedReducer** atribut uvádí, že typ by měl být registrován jako reduktorem se uživatelem definované.</span><span class="sxs-lookup"><span data-stu-id="9956c-493">The **SqlUserDefinedReducer** attribute indicates that the type should be registered as a user-defined reducer.</span></span> <span data-ttu-id="9956c-494">Tato třída nelze dědí.</span><span class="sxs-lookup"><span data-stu-id="9956c-494">This class cannot be inherited.</span></span>
<span data-ttu-id="9956c-495">**SqlUserDefinedReducer** je volitelný atribut pro definici reduktorem definovaný uživatelem.</span><span class="sxs-lookup"><span data-stu-id="9956c-495">**SqlUserDefinedReducer** is an optional attribute for a user-defined reducer definition.</span></span> <span data-ttu-id="9956c-496">Slouží k definování IsRecursive vlastnost.</span><span class="sxs-lookup"><span data-stu-id="9956c-496">It's used to define IsRecursive property.</span></span>

* <span data-ttu-id="9956c-497">BOOL IsRecursive</span><span class="sxs-lookup"><span data-stu-id="9956c-497">bool     IsRecursive</span></span>    
* <span data-ttu-id="9956c-498">**Hodnota TRUE,** = označuje, zda je tento reduktorem idempotent</span><span class="sxs-lookup"><span data-stu-id="9956c-498">**true**  = Indicates whether this Reducer is idempotent</span></span>

<span data-ttu-id="9956c-499">Objekty hlavního programovatelnosti **vstupní** a **výstup**.</span><span class="sxs-lookup"><span data-stu-id="9956c-499">The main programmability objects are **input** and **output**.</span></span> <span data-ttu-id="9956c-500">Vstupní objekt se používá k vytvoření výčtu vstupní řádky.</span><span class="sxs-lookup"><span data-stu-id="9956c-500">The input object is used to enumerate input rows.</span></span> <span data-ttu-id="9956c-501">Výstup se používá k nastavení výstupní řádků v důsledku omezení aktivity.</span><span class="sxs-lookup"><span data-stu-id="9956c-501">Output is used to set output rows as a result of reducing activity.</span></span>

<span data-ttu-id="9956c-502">Pro vstupní řádky výčet používáme `Row.Get` metoda.</span><span class="sxs-lookup"><span data-stu-id="9956c-502">For input rows enumeration, we use the `Row.Get` method.</span></span>

```
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

<span data-ttu-id="9956c-503">Parametr pro `Row.Get` metoda je sloupec, který se předá jako součást `PRODUCE` třídu `REDUCE` prohlášení o základní skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9956c-503">The parameter for the `Row.Get` method is a column that's passed as part of the `PRODUCE` class of the `REDUCE` statement of the U-SQL base script.</span></span> <span data-ttu-id="9956c-504">Je potřeba použít na správný typ. zde také.</span><span class="sxs-lookup"><span data-stu-id="9956c-504">We need to use the correct data type here as well.</span></span>

<span data-ttu-id="9956c-505">Pro výstup, použijte `output.Set` metoda.</span><span class="sxs-lookup"><span data-stu-id="9956c-505">For output, use the `output.Set` method.</span></span>

<span data-ttu-id="9956c-506">Je důležité si uvědomit, že vlastní reduktorem výstupy pouze hodnoty, které jsou definovány pomocí `output.Set` volání metody.</span><span class="sxs-lookup"><span data-stu-id="9956c-506">It is important to understand that custom reducer only outputs values that are defined with the `output.Set` method call.</span></span>

```
output.Set<string>("mycolumn", guid);
```

<span data-ttu-id="9956c-507">Skutečné reduktorem výstup se aktivuje při volání metody `yield return output.AsReadOnly();`.</span><span class="sxs-lookup"><span data-stu-id="9956c-507">The actual reducer output is triggered by calling `yield return output.AsReadOnly();`.</span></span>

<span data-ttu-id="9956c-508">Následuje příklad reduktorem:</span><span class="sxs-lookup"><span data-stu-id="9956c-508">Following is a reducer example:</span></span>

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

<span data-ttu-id="9956c-509">V tomto scénáři případ použití reduktorem přeskakuje řádky s prázdné jméno.</span><span class="sxs-lookup"><span data-stu-id="9956c-509">In this use-case scenario, the reducer is skipping rows with an empty user name.</span></span> <span data-ttu-id="9956c-510">Pro každý řádek v sadě řádků přečte každý požadovaný sloupec, následně vyhodnocuje délka uživatelského jména.</span><span class="sxs-lookup"><span data-stu-id="9956c-510">For each row in rowset, it reads each required column, then evaluates the length of the user name.</span></span> <span data-ttu-id="9956c-511">Skutečný řádek vyprodukuje pouze v případě, že hodnota délka jména uživatele je větší než 0.</span><span class="sxs-lookup"><span data-stu-id="9956c-511">It outputs the actual row only if user name value length is more than 0.</span></span>

<span data-ttu-id="9956c-512">Toto je základní skript U-SQL, která používá vlastní reduktorem:</span><span class="sxs-lookup"><span data-stu-id="9956c-512">Following is base U-SQL script that uses a custom reducer:</span></span>

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
    TO @output_file 
    USING Outputters.Text();
```
