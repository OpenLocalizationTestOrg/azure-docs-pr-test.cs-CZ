---
title: "místní aaaScale U-SQL spustit a otestovat s Azure Data Lake U-SQL SDK | Microsoft Docs"
description: "Zjistěte, jak místní úlohy tooscale U-SQL Azure Data Lake U-SQL SDK toouse spustit a otestovat pomocí příkazového řádku a programovací rozhraní na místní pracovní stanici."
services: data-lake-analytics
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: yanacai
ms.openlocfilehash: 2b0a16229789268e333f723ff6fc2c3efdc29905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-u-sql-local-run-and-test-with-azure-data-lake-u-sql-sdk"></a>Škálování U-SQL místní spuštění a testování pomocí Azure Data Lake U-SQL SDK

Při vývoji skript U-SQL, je běžné toorun a testů U-SQL skriptu místně před odešlete ji toocloud. Azure Data Lake poskytuje balíček Nuget s názvem SDK Azure Data Lake U-SQL pro tento scénář, pomocí kterých lze snadno škálovat U-SQL místní spuštění a testování. Je také možné toointegrate U-SQL testování s CI (nepřetržité integrace) systému tooautomate hello kompilace a testování.

Pokud se zajímáte o jak toomanually místní spuštění a ladění skriptu U-SQL pomocí nástrojů s grafickým uživatelským rozhraním, můžete pomocí nástroje Azure Data Lake pro Visual Studio pro tento. Další informace z [zde](data-lake-analytics-data-lake-tools-local-run.md).

## <a name="install-azure-data-lake-u-sql-sdk"></a>Instalace Azure Data Lake U-SQL SDK

Můžete získat hello SDK Azure Data Lake U-SQL [sem](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) na Nuget.org. A před jeho použitím, je třeba toomake se, že máte následující závislosti.

### <a name="dependencies"></a>Závislosti

Hello Data Lake U-SQL SDK vyžaduje hello následující závislosti:

- [Rozhraní Microsoft .NET Framework 4.6 nebo novější](https://www.microsoft.com/download/details.aspx?id=17851).
- Microsoft Visual C++ 14 a sady Windows SDK 10.0.10240.0 nebo novější (což se označuje jako CppSDK v tomto článku). Existují dva způsoby tooget CppSDK:

    - Nainstalujte [sady Visual Studio Community Edition](https://developer.microsoft.com/downloads/vs-thankyou). Budete mít \Windows Kits\10 složku ve složce Program Files hello – například C:\Program Files (x86) \Windows Kits\10\. Naleznete zde také hello verze Windows 10 SDK v části \Windows Kits\10\Lib. Pokud nevidíte tyto složky, přeinstalujte Visual Studio a že tooselect hello Windows 10 SDK se během instalace hello. Pokud je to nainstalované s Visual Studio, místní kompilátoru hello U-SQL najdete je automaticky.

    ![Nástroje data Lake pro Visual Studio místní spuštění Windows 10 SDK](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-windows-10-sdk.png)

    - Nainstalujte [nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs). Můžete najít, že hello balené C:\Program Files (x86) \Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\ADL Tools\X.X.XXXX.X\CppSDK souborů Visual C++ a sady Windows SDK. Místní kompilátoru hello U-SQL v tomto případě nelze najít hello závislosti automaticky. Musíte pro něj toospecify hello CppSDK cesta. Můžete zkopírovat hello soubory tooanother umístění nebo ho jako je použít.

## <a name="understand-basic-concepts"></a>Pochopit základní koncepty

### <a name="data-root"></a>Kořenové datové

Hello kořenové datové složce je pro účet místního výpočetní hello "místní úložiště". Je ekvivalentní toohello účtu Azure Data Lake Store účtu Data Lake Analytics. Přepínání tooa různé kořenové datové složce je stejně jako přepínání tooa úložiště jiný účet. Pokud budete chtít tooaccess běžně sdílí data pomocí různých dat kořenové složky, je nutné použít absolutní cesty ve skriptech. Nebo vytvořte symbolické odkazy systému souborů (například **mklink** na systém souborů NTFS) v části hello kořenové datové složce toopoint toohello sdílená data.

Hello kořenové datové složce se používá pro:

- Uložení místních metadat, včetně databází, tabulky, funkce vracející tabulku (Tvf) a sestavení.
- Vyhledání hello vstupní a výstupní cesty, které jsou definovány jako relativní cesty v U-SQL. Pomocí relativní cesty umožňuje snazší toodeploy vaše projekty tooAzure U-SQL.

### <a name="file-path-in-u-sql"></a>Cesta k souboru v U-SQL

Můžete je relativní cesta a místní cestou absolutní v skriptů U-SQL. relativní cesta Hello je cesta ke složce relativní toohello zadaný kořenový data. Doporučujeme vám, že můžete použít "/" jako hello cesta oddělovače toomake skripty kompatibilní s hello na straně serveru. Zde jsou některé příklady relativní cesty a jejich ekvivalent absolutní cesty. V těchto příkladech je C:\LocalRunDataRoot hello kořenové datové složce.

|Relativní cesta|Absolutní cesty|
|-------------|-------------|
|/ABC/DEF/Input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/Input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/Input.csv |D:\abc\def\input.csv|

### <a name="working-directory"></a>Pracovní adresář

Při místním spuštění skriptu hello U-SQL, vytvoří se během kompilace v aktuálním adresáři spuštěné pracovní adresář. Kromě toho toohello kompilace výstupy, hello potřebné soubory modulu runtime pro místní spuštění bude stínové zkopírovaný toothis pracovní adresář. Hello práce kořenový adresář se označuje jako "ScopeWorkDir" a hello soubory v pracovním adresáři hello jsou následující:

|Adresář nebo soubor|Adresář nebo soubor|Adresář nebo soubor|Definice|Popis|
|--------------|--------------|--------------|----------|-----------|
|C6A101DDCB470506| | |Řetězec hash verze modulu runtime|Soubory modulu runtime, které jsou potřebné pro provedení místní stínové kopie|
| |Script_66AE4909AA0ED06C| |Název skriptu + hash řetězec cestu ke skriptu|Výstupy kompilace a provádění krok protokolování|
| | |\_skript\_.abr|Výstup kompilátoru|Soubor algebra|
| | |\_ScopeCodeGen\_. *|Výstup kompilátoru|Vygenerovaný spravovaného kódu|
| | |\_ScopeCodeGenEngine\_. *|Výstup kompilátoru|Vygenerovaný nativního kódu|
| | |Odkazovaná sestavení|Odkaz na sestavení|Soubory odkazované sestavení|
| | |deployed_resources|Nasazení prostředků|Soubory nasazení prostředků|
| | |XXXXXXXX.xxx[1..n]\_\*. *|Spuštění protokolu|Protokol provádění kroků|


## <a name="use-hello-sdk-from-hello-command-line"></a>Použití hello SDK z příkazového řádku hello

### <a name="command-line-interface-of-hello-helper-application"></a>Rozhraní příkazového řádku aplikace hello pomocné rutiny

V části SDK directory\build\runtime je LocalRunHelper.exe hello nástroj příkazového řádku se aplikace, která poskytuje rozhraní toomost hello běžně používá místní spuštění funkce. Upozorňujeme, že oba hello příkazu a hello argument přepínače jsou malá a velká písmena. tooinvoke ho:

    LocalRunHelper.exe <command> <Required-Command-Arguments> [Optional-Command-Arguments]

Spustit LocalRunHelper.exe bez argumentů nebo s hello **pomoci** přepínač informace nápovědy tooshow hello:

    > LocalRunHelper.exe help

        Command 'help' :  Show usage information
        Command 'compile' :  Compile hello script
        Required Arguments :
            -Script param
                    Script File Path
        Optional Arguments :
            -Shallow [default value 'False']
                    Shallow compile

V hello pomoci informace:

-  **Příkaz** poskytuje hello název příkazu.  
-  **Vyžaduje Argument** uvádí argumenty, které je nutné zadat.  
-  **Nepovinný Argument** uvádí argumenty, které jsou volitelné, s výchozími hodnotami.  Nepovinné argumenty Boolean nemají parametry a jejich vzhled znamená záporná tootheir výchozí hodnota.

### <a name="return-value-and-logging"></a>Vrátí hodnotu a protokolování

Vrátí Hello podpůrnou aplikací **0** úspěch a **-1** selhání. Ve výchozím nastavení odešle hello pomocná všechny zprávy toohello aktuální konzolu. Ale většina příkazů hello podporují hello **- MessageOut cesta_k_souboru_protokolu** nepovinný argument, který přesměruje hello výstupy tooa souboru protokolu.

### <a name="environment-variable-configuring"></a>Konfigurace proměnné prostředí

U-SQL místní spuštění potřebují kořenové zadaná data jako účet místní úložiště, zadaná cesta CppSDK závislosti. Argument hello obě sady v proměnné prostředí příkazového řádku, nebo je nastavený můžete pro ně.

- Sada hello **SCOPE_CPP_SDK** proměnné prostředí.

    Pokud se zobrazí Microsoft Visual C++ a hello Windows SDK prostřednictvím instalace nástrojů Data Lake pro Visual Studio, ověřte, zda máte hello následující složku:

        C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK

    Zadejte novou proměnnou prostředí s názvem **SCOPE_CPP_SDK** toopoint toothis adresáře. Nebo zkopírujte složku toohello hello jiného umístění a zadejte **SCOPE_CPP_SDK** jako.

    Přidání toosetting hello prostředí proměnné, můžete zadat hello **- CppSDK** argument při použití hello příkazového řádku. Tento argument přepíše vaše proměnná prostředí CppSDK výchozí.

- Sada hello **LOCALRUN_DATAROOT** proměnné prostředí.

    Zadejte novou proměnnou prostředí s názvem **LOCALRUN_DATAROOT** , toohello kořenové datové body.

    Přidání toosetting hello prostředí proměnné, můžete zadat hello **- DataRoot** argument s cestou kořenové datové hello při použití příkazového řádku. Tento argument přepíše vaše proměnná prostředí kořenové datové výchozí. Je nutné tooadd tento argument tooevery příkazového řádku, které používáte, takže můžete přepsat hello výchozí kořenové datové proměnnou prostředí pro všechny operace.

### <a name="sdk-command-line-usage-samples"></a>Ukázky využití příkazový řádek SDK

#### <a name="compile-and-run"></a>Kompilace a spuštění

Hello **spustit** pomocí příkazu toocompile hello skript a potom spusťte kompilované výsledky. Jeho argumenty příkazového řádku jsou kombinaci těchto z **zkompilovat** a **provést**.

    LocalRunHelper run -Script path_to_usql_script.usql [optional_arguments]

Hello následují volitelné argumenty pro **spustit**:


|Argument|Výchozí hodnota|Popis|
|--------|-------------|-----------|
|-CodeBehind|False|skript Hello má .cs kódu na pozadí|
|-CppSDK| |CppSDK adresáře|
|-DataRoot| Proměnné prostředí DataRoot|Výchozí DataRoot pro místní spuštění příliš 'LOCALRUN_DATAROOT' – Proměnná prostředí|
|-MessageOut| |Výpis zprávy v souboru tooa konzoly|
|-Paralelní|1|Spustit hello plán s hello zadaný paralelismus|
|-Odkazy| |Seznam cest tooextra referenční sestavení nebo datové soubory kódu na pozadí, oddělených ';'|
|-UdoRedirect|False|Generovat Udo sestavení přesměrování konfigurace|
|-UseDatabase|master|Databáze toouse pro kód za registraci dočasné sestavení|
|-Verbose|False|Zobrazit podrobné výstupy z modulu runtime|
|-WorkDir|Aktuální adresář|Adresář pro využití kompilátoru a výstupy|
|-RunScopeCEP|0|Toouse ScopeCEP režimu|
|-ScopeCEPTempPath|dočasné|Dočasná cesta toouse pro datový proud|
|-OptFlags| |Seznam oddělený čárkami Optimalizátor příznaky|


Tady je příklad:

    LocalRunHelper run -Script d:\test\test1.usql -WorkDir d:\test\bin -CodeBehind -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB –Parallel 5 -Verbose

Kromě toho kombinování **zkompilovat** a **provést**, můžete zkompilovat a spustit samostatně, spustitelné soubory hello zkompilovat.

#### <a name="compile-a-u-sql-script"></a>Kompilace skript U-SQL

Hello **zkompilovat** příkaz je skript tooexecutables použité toocompile U-SQL.

    LocalRunHelper compile -Script path_to_usql_script.usql [optional_arguments]

Hello následují volitelné argumenty pro **zkompilovat**:


|Argument|Popis|
|--------|-----------|
| -CodeBehind [výchozí hodnotu "False"]|skript Hello má .cs kódu na pozadí|
| -CppSDK [výchozí hodnota:]|CppSDK adresáře|
| -DataRoot [výchozí hodnota "Proměnné prostředí DataRoot"]|Výchozí DataRoot pro místní spuštění příliš 'LOCALRUN_DATAROOT' – Proměnná prostředí|
| -MessageOut [výchozí hodnota:]|Výpis zprávy v souboru tooa konzoly|
| -Odkazuje [výchozí hodnota:]|Seznam cest tooextra referenční sestavení nebo datové soubory kódu na pozadí, oddělených ';'|
| -Malou [výchozí hodnotu "False"]|Bez podstruktury kompilace|
| -UdoRedirect [výchozí hodnotu "False"]|Generovat Udo sestavení přesměrování konfigurace|
| -UseDatabase [výchozí hodnota 'hlavní']|Databáze toouse pro kód za registraci dočasné sestavení|
| -WorkDir [výchozí hodnotu 'aktuální adresář.]|Adresář pro využití kompilátoru a výstupy|
| -RunScopeCEP [výchozí hodnota '0']|Toouse ScopeCEP režimu|
| -ScopeCEPTempPath [výchozí hodnota 'temp']|Dočasná cesta toouse pro datový proud|
| -OptFlags [výchozí hodnota:]|Seznam oddělený čárkami Optimalizátor příznaky|


Tady jsou některé příklady použití.

Kompilace skript U-SQL:

    LocalRunHelper compile -Script d:\test\test1.usql

Kompilace skript U-SQL a nastavte hello kořenové datové složce. Všimněte si, že tato akce přepíše proměnnou prostředí sady hello.

    LocalRunHelper compile -Script d:\test\test1.usql –DataRoot c:\DataRoot

Kompilace skript U-SQL a nastavte pracovní adresář, referenční sestavení a databáze:

    LocalRunHelper compile -Script d:\test\test1.usql -WorkDir d:\test\bin -References "d:\asm\ref1.dll;d:\asm\ref2.dll" -UseDatabase testDB

#### <a name="execute-compiled-results"></a>Spuštění kompilované výsledky

Hello **provést** příkaz je použité tooexecute zkompilovat výsledky.   

    LocalRunHelper execute -Algebra path_to_compiled_algebra_file [optional_arguments]

Hello následují volitelné argumenty pro **provést**:

|Argument|Popis|
|--------|-----------|
|-DataRoot [výchozí hodnota:]|Kořenové datové pro provedení metadat. Výchozí hodnota toohello **LOCALRUN_DATAROOT** proměnné prostředí.|
|-MessageOut [výchozí hodnota:]|Dump zprávy v souboru tooa hello konzoly.|
|-Paralelní [výchozí hodnotu 1.]|Indikátor toorun hello generované místní spuštění kroky s hello zadat úroveň stupně paralelního zpracování.|
|-Verbose [výchozí hodnotu "False"]|Indikátor tooshow podrobné výstupy z modulu runtime.|

Tady je příklad použití:

    LocalRunHelper execute -Algebra d:\test\workdir\C6A101DDCB470506\Script_66AE4909AA0ED06C\__script__.abr –DataRoot c:\DataRoot –Parallel 5


## <a name="use-hello-sdk-with-programming-interfaces"></a>Hello SDK pomocí programovacího rozhraní

programovací rozhraní Hello se nacházejí v hello LocalRunHelper.exe. Můžete používat funkce hello toointegrate hello SDK U-SQL a hello C# test framework tooscale svůj test místní skript U-SQL. V tomto článku použiji hello standardní C# jednotky testovacího projektu tooshow jak toouse těchto rozhraní tootest váš skript U-SQL.

### <a name="step-1-create-c-unit-test-project-and-configuration"></a>Krok 1: Vytvoření projektu testování částí C# a konfigurace

- Vytvoření projektu testování částí C# prostřednictvím soubor > Nový > Projekt > Visual C# > testovací > projektu testování částí.
- Přidejte LocalRunHelper.exe jako odkaz pro projekt hello. Hello LocalRunHelper.exe se nachází v \build\runtime\LocalRunHelper.exe v balíčku Nuget.

    ![Přidat odkaz na sadu SDK Azure Data Lake U-SQL](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-add-reference.png)

- U-SQL SDK **pouze** podporu x64 prostředí, ujistěte se, tooset sestavení Cílová platforma jako x64. Můžete nastavit, prostřednictvím vlastnosti projektu > sestavení > Cílová platforma.

    ![Azure Data Lake U-SQL SDK konfigurace x64 projektu](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-x64.png)

- Ujistěte se, že tooset testovacího prostředí jako x64. V sadě Visual Studio, můžete ho nastavit prostřednictvím testovací > Test nastavení > výchozí architekturu procesoru > x64.

    ![Azure Data Lake U-SQL SDK konfigurace x64 testovacího prostředí](./media/data-lake-analytics-u-sql-sdk/data-lake-analytics-u-sql-sdk-configure-test-x64.png)

- Ujistěte se, že toocopy všechny soubory závislosti v rámci NugetPackage\build\runtime\ tooproject pracovní adresář, který je obvykle pod ProjectFolder\bin\x64\Debug.

### <a name="step-2-create-u-sql-script-test-case"></a>Krok 2: Vytvoření testovacího případu skript U-SQL

Níže je hello ukázkový kód pro test skriptu U-SQL. Pro testování, je nutné tooprepare skripty, soubory očekávaný výstup a vstupní soubory.

    using System;
    using Microsoft.VisualStudio.TestTools.UnitTesting;
    using System.IO;
    using System.Text;
    using System.Security.Cryptography;
    using Microsoft.Analytics.LocalRun;

    namespace UnitTestProject1
    {
        [TestClass]
        public class USQLUnitTest
        {
            [TestMethod]
            public void TestUSQLScript()
            {
                //Specify hello local run message output path
                StreamWriter MessageOutput = new StreamWriter("../../../log.txt");

                LocalRunHelper localrun = new LocalRunHelper(MessageOutput);

                //Configure hello DateRoot path, Script Path and CPPSDK path
                localrun.DataRoot = "../../../";
                localrun.ScriptPath = "../../../Script/Script.usql";
                localrun.CppSdkDir = "../../../CppSDK";

                //Run U-SQL script
                localrun.DoRun();

                //Script output 
                string Result = Path.Combine(localrun.DataRoot, "Output/result.csv");

                //Expected script output
                string ExpectedResult = "../../../ExpectedOutput/result.csv";

                Test.Helpers.FileAssert.AreEqual(Result, ExpectedResult);

                //Don't forget tooclose MessageOutput tooget logs into file
                MessageOutput.Close();
            }
        }
    }

    namespace Test.Helpers
    {
        public static class FileAssert
        {
            static string GetFileHash(string filename)
            {
                Assert.IsTrue(File.Exists(filename));

                using (var hash = new SHA1Managed())
                {
                    var clearBytes = File.ReadAllBytes(filename);
                    var hashedBytes = hash.ComputeHash(clearBytes);
                    return ConvertBytesToHex(hashedBytes);
                }
            }

            static string ConvertBytesToHex(byte[] bytes)
            {
                var sb = new StringBuilder();

                for (var i = 0; i < bytes.Length; i++)
                {
                    sb.Append(bytes[i].ToString("x"));
                }
                return sb.ToString();
            }

            public static void AreEqual(string filename1, string filename2)
            {
                string hash1 = GetFileHash(filename1);
                string hash2 = GetFileHash(filename2);

                Assert.AreEqual(hash1, hash2);
            }
        }
    }


### <a name="programming-interfaces-in-localrunhelperexe"></a>Programování rozhraní LocalRunHelper.exe

LocalRunHelper.exe poskytuje programovací rozhraní pro místní kompilace U-SQL, spusťte hello, jsou uvedeny v následujícím seznamu rozhraní hello atd.

**Konstruktor**

veřejné LocalRunHelper ([System.IO.TextWriter messageOutput = null])

|Parametr|Typ|Popis|
|---------|----|-----------|
|messageOutput|System.IO.TextWriter|pro výstup zprávy nastavte toonull toouse konzoly|

**Vlastnosti**

|Vlastnost|Typ|Popis|
|--------|----|-----------|
|AlgebraPath|Řetězec|Hello cesta tooalgebra soubor (soubor algebra je jedním z výsledky kompilace hello)|
|CodeBehindReferences|Řetězec|Pokud skript hello další kódu na pozadí odkazy, zadejte hello cesty odděleny znakem ";"|
|CppSdkDir|Řetězec|CppSDK adresáře|
|CurrentDir|Řetězec|Aktuální adresář|
|DataRoot|Řetězec|Kořenová cesta dat|
|DebuggerMailPath|Řetězec|zásuvky Hello cesta toodebugger pošty|
|GenerateUdoRedirect|BOOL|Pokud chceme načítání sestavení toogenerate přesměrování přepsat konfigurace|
|HasCodeBehind|BOOL|Pokud má skript hello kódu na pozadí|
|InputDir|Řetězec|Adresář pro vstupní data|
|MessagePath|Řetězec|Cesta k souboru výpisu zpráv|
|OutputDir|Řetězec|Adresář pro výstupní data|
|Paralelismus|celá čísla|Algebra hello toorun paralelismus|
|ParentPid|celá čísla|ID nadřazené hello, na které hello služba monitoruje tooexit, too0 sady nebo záporné tooignore|
|ResultPath|Řetězec|Cesta k souboru výpisu výsledek|
|RuntimeDir|Řetězec|Adresář modulu runtime|
|scriptPath|Řetězec|Kde toofind hello skriptu|
|Bez podstruktury|BOOL|Nedávná kompilace, nebo ne|
|TempDir|Řetězec|Dočasný adresář|
|UseDataBase|Řetězec|Zadejte hello toouse databáze pro kódu na pozadí dočasné sestavení registrace, hlavní ve výchozím nastavení|
|WorkDir|Řetězec|Upřednostňované pracovní adresář|


**– Metoda**

|Metoda|Popis|Vrátí|Parametr|
|------|-----------|------|---------|
|veřejné bool DoCompile()|Kompilace hello U-SQL skriptů|Hodnota TRUE, v případě úspěchu| |
|veřejné bool DoExec()|Hello zkompilovat výsledek spuštění|Hodnota TRUE, v případě úspěchu| |
|veřejné bool DoRun()|Spusťte skript hello U-SQL (kompilace + spouštění)|Hodnota TRUE, v případě úspěchu| |
|veřejné bool IsValidRuntimeDir (cesta řetězec)|Zkontrolujte, jestli hello zadána cesta je cesta platná runtime|Platí pro platný|Cesta Hello adresář modulu runtime|


## <a name="faq-about-common-issue"></a>Nejčastější dotazy o běžné problémy

### <a name="error-1"></a>1. Chyba:
E_CSC_SYSTEM_INTERNAL: Vnitřní chyba! Nepodařilo se načíst soubor nebo sestavení 'ScopeEngineManaged.dll nebo jednu ze závislostí. Hello zadaný modul nebyl nalezen.

Zkontrolujte, zda text hello následující:

- Zajistěte, aby byla x64 prostředí. Cílová platforma Hello sestavení a hello testovací prostředí by měl být x64, najdete v příliš**krok 1: vytvoření C# jednotkové testování projektu a konfigurace** výše.
- Ujistěte se, že jste zkopírovali všechny soubory závislosti v rámci NugetPackage\build\runtime\ tooproject pracovní adresář.


## <a name="next-steps"></a>Další kroky

* toolearn U-SQL, najdete v části [Začínáme s jazykem Azure Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).
* toolog diagnostické informace, najdete v části [přístup k protokolů diagnostiky pro Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md).
* toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Podrobnosti úlohy tooview, najdete v tématu [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).
* zobrazení provádění vrcholů toouse hello, najdete v části [použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
