---
title: "aaaTest a ladění úloh U-SQL pomocí místního spuštění a hello Azure Data Lake U-SQL SDK | Microsoft Docs"
description: "Zjistěte, jak úlohy nástroje toouse Azure Data Lake pro Visual Studio a tootest hello SDK Azure Data Lake U-SQL a ladění U-SQL na místní pracovní stanici."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: be04558a504acf6a088e207608ee2d4a011d3ffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-hello-azure-data-lake-u-sql-sdk"></a>Testování a ladění úloh U-SQL pomocí místního spuštění a hello Azure Data Lake U-SQL SDK

Nástroje služby Azure Data Lake pro Visual Studio a úloh U-SQL toorun hello SDK Azure Data Lake U-SQL můžete použít na pracovní stanici, stejně jako ve službě Azure Data Lake hello. Tyto dvě místně spouštěné funkce vám šetří čas při testování a ladění úloh U-SQL.

## <a name="understand-hello-data-root-folder-and-hello-file-path"></a>Pochopení hello kořenové datové složce a cesta k souboru hello

Místní spuštění a hello U-SQL sady SDK vyžadovat kořenové datové složce. Hello kořenové datové složce je pro účet místního výpočetní hello "místní úložiště". Je ekvivalentní toohello účtu Azure Data Lake Store účtu Data Lake Analytics. Přepínání tooa různé kořenové datové složce je stejně jako přepínání tooa úložiště jiný účet. Pokud budete chtít tooaccess běžně sdílí data pomocí různých dat kořenové složky, je nutné použít absolutní cesty ve skriptech. Nebo vytvořte symbolické odkazy systému souborů (například **mklink** na systém souborů NTFS) v části hello kořenové datové složce toopoint toohello sdílená data.

Hello kořenové datové složce se používá pro:

- Ukládání metadat, včetně databází, tabulky, funkce vracející tabulku (Tvf) a sestavení.
- Vyhledání hello vstupní a výstupní cesty, které jsou definovány jako relativní cesty v U-SQL. Pomocí relativní cesty umožňuje snazší toodeploy vaše projekty tooAzure U-SQL.

Můžete je relativní cesta a místní cestou absolutní v skriptů U-SQL. relativní cesta Hello je cesta ke složce relativní toohello zadaný kořenový data. Doporučujeme vám, že můžete použít "/" jako hello cesta oddělovače toomake skripty kompatibilní s hello na straně serveru. Zde jsou některé příklady relativní cesty a jejich ekvivalent absolutní cesty. V těchto příkladech je C:\LocalRunDataRoot hello kořenové datové složce.

|Relativní cesta|Absolutní cesty|
|-------------|-------------|
|/ABC/DEF/Input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/Input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/Input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Použití místního spuštění ze sady Visual Studio

Nástroje data Lake pro Visual Studio poskytuje možnosti místní spuštění U-SQL v sadě Visual Studio. Pomocí této funkce můžete:

- Spusťte skript U-SQL, který je místně, společně s sestavení C#.
- Ladění sestavení C# místně.
- Vytvořit, zobrazit a odstranění katalogů U-SQL (místních databází, sestavení, schémat a tabulek) z Průzkumníka serveru. Můžete také získat místní katalog hello také z Průzkumníka serveru.

    ![Nástroje data Lake pro Visual Studio místní spuštění místního katalogu](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

Instalační program nástroje Data Lake Hello vytvoří toobe složky C:\LocalRunRoot použít jako hello výchozí kořenové datové složce. paralelismus místní spuštění Hello výchozí je 1.

### <a name="tooconfigure-local-run-in-visual-studio"></a>tooconfigure místní spuštění v sadě Visual Studio

1. Otevřete sadu Visual Studio.
2. Otevřete **Průzkumníka serveru**.
3. Rozbalte položku **Azure** > **Data Lake Analytics**.
4. Klikněte na tlačítko hello **Data Lake** nabídce a pak klikněte na tlačítko **možnosti a nastavení**.
5. Rozbalte ve stromu levém hello **Azure Data Lake**a potom rozbalte **Obecné**.

    ![Konfigurace nástrojů data Lake pro Visual Studio spustit místní nastavení](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

Projekt Visual Studio U-SQL je vyžadována pro provádění místní spuštění. Tato část se liší od spuštění skriptů U-SQL z Azure.

### <a name="toorun-a-u-sql-script-locally"></a>skript U-SQL toorun místně
1. Ze sady Visual Studio otevřete projekt U-SQL.   
2. Skript U-SQL v Průzkumníku řešení klikněte pravým tlačítkem myši a pak klikněte na **odeslat skript**.
3. Vyberte **(místní)** jako hello účtu Analytics toorun váš skript místně.
Můžete také kliknout na hello **(místní)** účet na hello horní části okna skript a potom klikněte na **odeslání** (nebo použijte hello Ctrl + F5 klávesové zkratky).

    ![Nástroje data Lake pro Visual Studio spustit místní odeslání úlohy](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Místní ladění skriptů a sestavení C#

Sestavení C# můžete ladit bez odeslali a zaregistrovali tooAzure služba Data Lake Analytics. V obou hello souboru kódu i v odkazovaném projektu C# můžete nastavit zarážky.

#### <a name="toodebug-local-code-in-code-behind-file"></a>toodebug místního kódu v souboru kódu

1. Nastavte zarážky v souboru kódu hello.
2. Stisknutím klávesy F5 toodebug hello skript místně.

> [!NOTE]
   > Následující postup lze použít pouze v sadě Visual Studio 2015 Hello. Ve starší sadě Visual Studio může potřebujete toomanually přidat soubory pdb hello.  
   >
   >

#### <a name="toodebug-local-code-in-a-referenced-c-project"></a>toodebug místního kódu v odkazovaném projektu C#

1. Vytvořte projekt sestavení C# a sestavte jej toogenerate hello výstupní knihovnu dll.
2. Zaregistrujte hello knihovny dll pomocí příkazu U-SQL:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Nastavte zarážky v hello kód C#.
4. Stisknutím klávesy F5 toodebug hello skriptu s odkazujícím hello C# knihovnu dll.

## <a name="use-local-run-from-hello-data-lake-u-sql-sdk"></a>Použití místního spuštění z hello Data Lake U-SQL SDK

Kromě toorunning U-SQL skriptů místně pomocí sady Visual Studio, můžete použít skripty U-SQL toorun SDK Azure Data Lake U-SQL hello místně pomocí příkazového řádku a programovací rozhraní. Pomocí těchto je možné škálovat svůj místní test U-SQL.

Další informace o [SDK Azure Data Lake U-SQL](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Další kroky

* toosee komplexnější dotaz, najdete v části [analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Podrobnosti úlohy tooview, najdete v tématu [použití úlohy prohlížeče a zobrazení úloh pro úlohy Azure Data Lake Analytics](data-lake-analytics-data-lake-tools-view-jobs.md).
* zobrazení provádění vrcholů toouse hello, najdete v části [použití hello nebo zobrazení provádění vrcholů v nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
