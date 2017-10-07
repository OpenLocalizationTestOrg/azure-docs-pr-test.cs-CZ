---
title: "skripty aaaDevelop U-SQL pomocí nástrojů Data Lake pro Visual Studio | Microsoft Docs"
description: "Zjistěte, jak nástroje tooinstall Data Lake pro Visual Studio a jak toodevelop a testování skriptů U-SQL."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/28/2017
ms.author: saveenr, yanacai
ms.openlocfilehash: 7a0c02c275b8cefecbe784ba63969cbf24a150d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Zjistěte, jak účtů toouse Visual Studio toocreate Azure Data Lake Analytics, definování úloh v [U-SQL](data-lake-analytics-u-sql-get-started.md)a odeslání úlohy toohello Data Lake Analytics služby. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).


## <a name="prerequisites"></a>Požadavky

* **Visual Studio:** Podporovány jsou všechny edice kromě Express.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* Sada **Microsoft Azure SDK pro .NET** verze 2.7.1 nebo novější.  Nainstalujte ji pomocí hello [instalačního programu webové platformy](http://www.microsoft.com/web/downloads/platform.aspx).
* **Účet Data Lake Analytics**. toocreate na účet, najdete v části [Začínáme s Azure Data Lake Analytics pomocí portálu Azure](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Instalace nástrojů Azure Data Lake pro Visual Studio 

Stáhněte a nainstalujte nástroje Azure Data Lake pro Visual Studio [z hello Download Center](http://aka.ms/adltoolsvs). Po instalaci si všimněte, že:
* Hello **Průzkumníka serveru** > **Azure** uzel obsahuje **Data Lake Analytics** uzlu. 
* Hello **nástroje** má nabídky **Data Lake** položky.

## <a name="connect-tooan-azure-data-lake-analytics-account"></a>Připojit tooan účtu Azure Data Lake Analytics

1. Otevřete sadu Visual Studio.
2. Otevřete Průzkumníka serveru výběrem **Zobrazení** > **Průzkumník serveru**.
3. Klikněte pravým tlačítkem na **Azure**. Potom vyberte **připojit tooMicrosoft předplatné Azure** a postupujte podle pokynů hello.
4. V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**. Zobrazí se seznam vašich účtů Data Lake Analytics.


## <a name="write-your-first-u-sql-script"></a>Napsání prvního skriptu U-SQL

Následující text Hello je jednoduchý skript U-SQL. Definuje, na malou datovou sadu a zápisů, které volá datovou sadu toohello výchozí Data Lake Store jako soubor `/data.csv`.

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

### <a name="submit-a-data-lake-analytics-job"></a>Odeslání úlohy Data Lake Analytics

1. Vyberte **Soubor** > **Nový** > **Projekt**.

2. Vyberte hello **projekt U-SQL** a klepněte na tlačítko **OK**. Visual Studio vytvoří řešení se souborem **Script.usql**.

3. Vložte skript předchozí hello do hello **Script.usql** okno.

4. V levém horním rohu hello hello **Script.usql** okno, zadejte účet Data Lake Analytics hello.

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. V levém horním rohu hello hello **Script.usql** vyberte **odeslání**.
6. Ověřte hello **účet Analytics**a potom vyberte **odeslání**. Po dokončení odesílání hello jsou k dispozici v hello nástroje Data Lake pro Visual Studio výsledky odeslání výsledky.

    ![Odeslání projektu U-SQL sady Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
7. toosee hello nejnovější úlohy stavu a aktualizace úvodní obrazovka, klikněte na tlačítko **aktualizovat**. Pokud je úloha hello úspěšná, zobrazí hello **graf úlohy**, **operací metadat**, **historie stavů**, a **diagnostiky**:

    ![Graf výkonu úlohy U-SQL Visual Studio Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Souhrn úlohy** ukazuje hello Souhrn úlohy hello.   
   * **Podrobnosti úlohy** uvádí podrobnější informace o hello úlohy, včetně hello skriptu, prostředky a vrcholy.
   * **Graf úlohy** vizualizuje hello průběh úlohy hello.
   * **Operace s metadaty** ukazuje všechny hello akce, které byly provedeny v katalogu hello U-SQL.
   * **Data** ukazuje všechny hello vstupy a výstupy.
   * **Diagnostika** poskytuje pokročilé analýzy spouštění úlohy a optimalizace výkonu.

### <a name="toocheck-job-state"></a>Stav úlohy toocheck

1. V Průzkumníku serveru vyberte **Azure** > **Data Lake Analytics**. 
2. Rozbalte název účtu Data Lake Analytics hello.
3. Dvakrát klikněte na **Úlohy**.
4. Vyberte hello úlohu, která již byla odeslána.

### <a name="toosee-hello-output-of-a-job"></a>toosee hello výstup úlohy

1. V Průzkumníku serveru vyhledejte toohello úlohy, které jste odeslali.
2. Klikněte na tlačítko hello **Data** kartě.
3. V hello **výstupy úlohy** karty, vyberte hello `"/data.csv"` souboru.

## <a name="next-steps"></a>Další kroky

* [Spouštění skriptů U-SQL na vlastní pracovní stanici za účelem testování a ladění](data-lake-analytics-data-lake-tools-local-run.md)
* [Ladění kódu C# v úlohách U-SQL](data-lake-analytics-debug-u-sql-jobs.md)
* [Pomocí nástrojů hello Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
