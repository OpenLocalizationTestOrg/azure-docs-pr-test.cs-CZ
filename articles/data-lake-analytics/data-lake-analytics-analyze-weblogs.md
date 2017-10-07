---
title: "aaaAnalyze webových protokolů pomocí Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak tooanalyze webových protokolů pomocí Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: d27aaca95ed2b643cfed7a17b0066bf7fa4a1bf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Analýza webových protokolů pomocí Azure Data Lake Analytics
Zjistěte, jak tooanalyze webových protokolů pomocí Data Lake Analytics, hlavně o nalezení které odkazující servery nastaly chyby při, kdy se pokusil toovisit hello webu.

## <a name="prerequisites"></a>Požadavky
* **Visual Studio 2015 nebo Visual Studio 2013**.
* **[Nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs)**.

    Po instalaci nástrojů Data Lake pro Visual Studio se zobrazí **Data Lake** položky v hello **nástroje** nabídky v sadě Visual Studio:

    ![Nabídky U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Základní znalosti o Data Lake Analytics a hello nástrojů Data Lake pro Visual Studio**. tooget začít, najdete v části:

  * [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* **Účet Data Lake Analytics.**  V tématu [vytvoření účtu Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
* **Nahrajte účtu Data Lake Analytics hello ukázková data toohello.** V tématu [toocopy ukázkových datových souborů](data-lake-analytics-get-started-portal.md).

    toorun úlohy Data Lake Analytics, budete potřebovat některá data. I když hello nástrojů Data Lake podporují nahrávání dat, budete používat hello portálu tooupload hello ukázková data toomake tento kurz toofollow jednodušší.

## <a name="connect-tooazure"></a>Připojit tooAzure
Než budete moct sestavení a testování všech skriptů U-SQL, je nutné připojit tooAzure.

**tooconnect tooData Lake Analytics**

1. Otevřete sadu Visual Studio.
2. Klikněte na tlačítko **Data Lake > Možnosti a nastavení**.
3. Klikněte na tlačítko **přihlásit**, nebo **změnit uživatele** Pokud někdo se přihlásil a postupujte podle pokynů hello.
4. Klikněte na tlačítko **OK** tooclose hello možnosti a nastavení dialogové okno.

**toobrowse vaše účty Data Lake Analytics**

1. Ze sady Visual Studio otevřete **Průzkumníka serveru** podle stiskněte **CTRL + ALT + S**.
2. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte položku **Data Lake Analytics**. Zobrazí se seznam účtů Data Lake Analytics, pokud nějaké máte. Nelze vytvořit účty Data Lake Analytics z hello studio. toocreate na účet, najdete v části [Začínáme s Azure Data Lake Analytics pomocí portálu Azure](data-lake-analytics-get-started-portal.md) nebo [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Vývoj aplikací U-SQL
Aplikací U-SQL je většinou skript U-SQL. toolearn Další informace o U-SQL, najdete v části [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md).

Můžete přidat další uživatelem definované operátory toohello aplikaci.  Další informace najdete v tématu [vyvíjet U-SQL uživatelem definované operátory pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**toocreate a odeslání úlohy Data Lake Analytics**

1. Klikněte na tlačítko hello **soubor > Nový > projekt**.
2. Vyberte typ hello projekt U-SQL.

    ![Nový projekt U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klikněte na **OK**. Visual studio vytvoří řešení se souborem Script.usql.
4. Zadejte následující skript do souboru Script.usql hello hello:

        // Create a database for easy reuse, so you don't need tooread from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from hello weblog file with hello correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    hello toounderstand U-SQL, najdete v části [Začínáme s jazykem Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).    
5. Přidat nový projekt tooyour skript U-SQL a zadejte následující hello:

        // Query hello referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        too@"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. Přepnout zpět toohello první skript U-SQL a další toohello **odeslání** tlačítko, zadejte svůj účet Analytics.
7. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**. Zkontrolujte výsledky hello v podokně výstup hello.
8. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**.
9. Ověřte hello **účet Analytics** je hello jeden kam chcete toorun hello úlohy a pak klikněte na tlačítko **odeslání**. Výsledky odeslání a odkaz na úlohu jsou k dispozici v hello nástrojů Data Lake pro Visual Studio výsledky okno po dokončení odesílání hello.
10. Počkejte, dokud je hello úloha úspěšně dokončena.  Pokud hello úlohy se nezdařilo, chybí s největší pravděpodobností hello zdrojový soubor.  Podrobnosti viz hello požadovaných části tohoto kurzu. Další informace o odstraňování potíží naleznete v části [sledování úloh Azure Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Po dokončení úlohy hello, zobrazí se následující obrazovka hello:

    ![data lake analytics analýza weblogů webových protokolů](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Teď zopakujte kroky 7 až 10 pro **Script1.usql**.

**výstup úlohy toosee hello**

1. Z **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **účty úložiště**, klikněte pravým tlačítkem na hello výchozí účet Data Lake Storage a pak klikněte na tlačítko **Explorer**.
2. Klikněte dvakrát na **ukázky** tooopen hello složku a potom dvakrát klikněte na **výstupy**.
3. Klikněte dvakrát na **UnsuccessfulResponsees.log**.
4. Můžete také dvakrát kliknete na výstupní soubor hello uvnitř zobrazení grafu hello hello úlohy v pořadí toonavigate přímo toohello výstup.

## <a name="see-also"></a>Viz také
tooget začít s Data Lake Analytics pomocí různých nástrojů, najdete v části:

* [Začínáme se službou Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md)
* [Začínáme se službou Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-get-started-powershell.md)
* [Začínáme se službou Data Lake Analytics pomocí sady .NET SDK](data-lake-analytics-get-started-net-sdk.md)
