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
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a><span data-ttu-id="e3ffd-103">Analýza webových protokolů pomocí Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="e3ffd-103">Analyze Website logs using Azure Data Lake Analytics</span></span>
<span data-ttu-id="e3ffd-104">Zjistěte, jak tooanalyze webových protokolů pomocí Data Lake Analytics, hlavně o nalezení které odkazující servery nastaly chyby při, kdy se pokusil toovisit hello webu.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-104">Learn how tooanalyze website logs using Data Lake Analytics, especially on finding out which referrers ran into errors when they tried toovisit hello website.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3ffd-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e3ffd-105">Prerequisites</span></span>
* <span data-ttu-id="e3ffd-106">**Visual Studio 2015 nebo Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-106">**Visual Studio 2015 or Visual Studio 2013**.</span></span>
* <span data-ttu-id="e3ffd-107">**[Nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs)**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span></span>

    <span data-ttu-id="e3ffd-108">Po instalaci nástrojů Data Lake pro Visual Studio se zobrazí **Data Lake** položky v hello **nástroje** nabídky v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="e3ffd-108">Once Data Lake Tools for Visual Studio is installed, you will see a **Data Lake** item in hello **Tools** menu in Visual Studio:</span></span>

    ![Nabídky U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* <span data-ttu-id="e3ffd-110">**Základní znalosti o Data Lake Analytics a hello nástrojů Data Lake pro Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-110">**Basic knowledge of Data Lake Analytics and hello Data Lake Tools for Visual Studio**.</span></span> <span data-ttu-id="e3ffd-111">tooget začít, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="e3ffd-111">tooget started, see:</span></span>

  * <span data-ttu-id="e3ffd-112">[Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-112">[Develop U-SQL script using Data Lake tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="e3ffd-113">**Účet Data Lake Analytics.**</span><span class="sxs-lookup"><span data-stu-id="e3ffd-113">**A Data Lake Analytics account.**</span></span>  <span data-ttu-id="e3ffd-114">V tématu [vytvoření účtu Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-114">See [Create an Azure Data Lake Analytics account](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="e3ffd-115">**Nahrajte účtu Data Lake Analytics hello ukázková data toohello.**</span><span class="sxs-lookup"><span data-stu-id="e3ffd-115">**Upload hello sample data toohello Data Lake Analytics account.**</span></span> <span data-ttu-id="e3ffd-116">V tématu [toocopy ukázkových datových souborů](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-116">See [toocopy sample data files](data-lake-analytics-get-started-portal.md).</span></span>

    <span data-ttu-id="e3ffd-117">toorun úlohy Data Lake Analytics, budete potřebovat některá data.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-117">toorun a Data Lake Analytics job, you will need some data.</span></span> <span data-ttu-id="e3ffd-118">I když hello nástrojů Data Lake podporují nahrávání dat, budete používat hello portálu tooupload hello ukázková data toomake tento kurz toofollow jednodušší.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-118">Even though hello Data Lake Tools supports uploading data, you will use hello portal tooupload hello sample data toomake this tutorial easier toofollow.</span></span>

## <a name="connect-tooazure"></a><span data-ttu-id="e3ffd-119">Připojit tooAzure</span><span class="sxs-lookup"><span data-stu-id="e3ffd-119">Connect tooAzure</span></span>
<span data-ttu-id="e3ffd-120">Než budete moct sestavení a testování všech skriptů U-SQL, je nutné připojit tooAzure.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-120">Before you can build and test any U-SQL scripts, you must first connect tooAzure.</span></span>

<span data-ttu-id="e3ffd-121">**tooconnect tooData Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="e3ffd-121">**tooconnect tooData Lake Analytics**</span></span>

1. <span data-ttu-id="e3ffd-122">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-122">Open Visual Studio.</span></span>
2. <span data-ttu-id="e3ffd-123">Klikněte na tlačítko **Data Lake > Možnosti a nastavení**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-123">Click **Data Lake > Options and Settings**.</span></span>
3. <span data-ttu-id="e3ffd-124">Klikněte na tlačítko **přihlásit**, nebo **změnit uživatele** Pokud někdo se přihlásil a postupujte podle pokynů hello.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-124">Click **Sign In**, or **Change User** if someone has signed in, and follow hello instructions.</span></span>
4. <span data-ttu-id="e3ffd-125">Klikněte na tlačítko **OK** tooclose hello možnosti a nastavení dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-125">Click **OK** tooclose hello Options and Settings dialog.</span></span>

<span data-ttu-id="e3ffd-126">**toobrowse vaše účty Data Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="e3ffd-126">**toobrowse your Data Lake Analytics accounts**</span></span>

1. <span data-ttu-id="e3ffd-127">Ze sady Visual Studio otevřete **Průzkumníka serveru** podle stiskněte **CTRL + ALT + S**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-127">From Visual Studio, open **Server Explorer** by press **CTRL+ALT+S**.</span></span>
2. <span data-ttu-id="e3ffd-128">V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte položku **Data Lake Analytics**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-128">From **Server Explorer**, expand **Azure**, and then expand **Data Lake Analytics**.</span></span> <span data-ttu-id="e3ffd-129">Zobrazí se seznam účtů Data Lake Analytics, pokud nějaké máte.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-129">You shall see a list of your Data Lake Analytics accounts if there are any.</span></span> <span data-ttu-id="e3ffd-130">Nelze vytvořit účty Data Lake Analytics z hello studio.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-130">You cannot create Data Lake Analytics accounts from hello studio.</span></span> <span data-ttu-id="e3ffd-131">toocreate na účet, najdete v části [Začínáme s Azure Data Lake Analytics pomocí portálu Azure](data-lake-analytics-get-started-portal.md) nebo [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-131">toocreate an account, see [Get Started with Azure Data Lake Analytics using Azure Portal](data-lake-analytics-get-started-portal.md) or [Get Started with Azure Data Lake Analytics using Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span></span>

## <a name="develop-u-sql-application"></a><span data-ttu-id="e3ffd-132">Vývoj aplikací U-SQL</span><span class="sxs-lookup"><span data-stu-id="e3ffd-132">Develop U-SQL application</span></span>
<span data-ttu-id="e3ffd-133">Aplikací U-SQL je většinou skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-133">A U-SQL application is mostly a U-SQL script.</span></span> <span data-ttu-id="e3ffd-134">toolearn Další informace o U-SQL, najdete v části [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-134">toolearn more about U-SQL, see [Get started with U-SQL](data-lake-analytics-u-sql-get-started.md).</span></span>

<span data-ttu-id="e3ffd-135">Můžete přidat další uživatelem definované operátory toohello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-135">You can add addition user-defined operators toohello application.</span></span>  <span data-ttu-id="e3ffd-136">Další informace najdete v tématu [vyvíjet U-SQL uživatelem definované operátory pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-136">For more information, see [Develop U-SQL user defined operators for Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span></span>

<span data-ttu-id="e3ffd-137">**toocreate a odeslání úlohy Data Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="e3ffd-137">**toocreate and submit a Data Lake Analytics job**</span></span>

1. <span data-ttu-id="e3ffd-138">Klikněte na tlačítko hello **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-138">Click hello **File > New > Project**.</span></span>
2. <span data-ttu-id="e3ffd-139">Vyberte typ hello projekt U-SQL.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-139">Select hello U-SQL Project type.</span></span>

    ![Nový projekt U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. <span data-ttu-id="e3ffd-141">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-141">Click **OK**.</span></span> <span data-ttu-id="e3ffd-142">Visual studio vytvoří řešení se souborem Script.usql.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-142">Visual studio creates a solution with a Script.usql file.</span></span>
4. <span data-ttu-id="e3ffd-143">Zadejte následující skript do souboru Script.usql hello hello:</span><span class="sxs-lookup"><span data-stu-id="e3ffd-143">Enter hello following script into hello Script.usql file:</span></span>

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

    <span data-ttu-id="e3ffd-144">hello toounderstand U-SQL, najdete v části [Začínáme s jazykem Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-144">toounderstand hello U-SQL, see [Get started with Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>    
5. <span data-ttu-id="e3ffd-145">Přidat nový projekt tooyour skript U-SQL a zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="e3ffd-145">Add a new U-SQL script tooyour project and enter hello following:</span></span>

        // Query hello referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        too@"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. <span data-ttu-id="e3ffd-146">Přepnout zpět toohello první skript U-SQL a další toohello **odeslání** tlačítko, zadejte svůj účet Analytics.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-146">Switch back toohello first U-SQL script and next toohello **Submit** button, specify your Analytics account.</span></span>
7. <span data-ttu-id="e3ffd-147">V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-147">From **Solution Explorer**, right click **Script.usql**, and then click **Build Script**.</span></span> <span data-ttu-id="e3ffd-148">Zkontrolujte výsledky hello v podokně výstup hello.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-148">Verify hello results in hello Output pane.</span></span>
8. <span data-ttu-id="e3ffd-149">V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-149">From **Solution Explorer**, right click **Script.usql**, and then click **Submit Script**.</span></span>
9. <span data-ttu-id="e3ffd-150">Ověřte hello **účet Analytics** je hello jeden kam chcete toorun hello úlohy a pak klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-150">Verify hello **Analytics Account** is hello one where you want toorun hello job, and then click **Submit**.</span></span> <span data-ttu-id="e3ffd-151">Výsledky odeslání a odkaz na úlohu jsou k dispozici v hello nástrojů Data Lake pro Visual Studio výsledky okno po dokončení odesílání hello.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-151">Submission results and job link are available in hello Data Lake Tools for Visual Studio Results window when hello submission is completed.</span></span>
10. <span data-ttu-id="e3ffd-152">Počkejte, dokud je hello úloha úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-152">Wait until hello job is completed successfully.</span></span>  <span data-ttu-id="e3ffd-153">Pokud hello úlohy se nezdařilo, chybí s největší pravděpodobností hello zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-153">If hello job failed, it is most likely missing hello source file.</span></span>  <span data-ttu-id="e3ffd-154">Podrobnosti viz hello požadovaných části tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-154">Please see hello Prerequisite section of this tutorial.</span></span> <span data-ttu-id="e3ffd-155">Další informace o odstraňování potíží naleznete v části [sledování úloh Azure Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="e3ffd-155">For additional troubleshooting information, see [Monitor and troubleshoot Azure Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>

    <span data-ttu-id="e3ffd-156">Po dokončení úlohy hello, zobrazí se následující obrazovka hello:</span><span class="sxs-lookup"><span data-stu-id="e3ffd-156">When hello job is completed, you shall see hello following screen:</span></span>

    ![data lake analytics analýza weblogů webových protokolů](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. <span data-ttu-id="e3ffd-158">Teď zopakujte kroky 7 až 10 pro **Script1.usql**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-158">Now repeat steps 7- 10 for **Script1.usql**.</span></span>

<span data-ttu-id="e3ffd-159">**výstup úlohy toosee hello**</span><span class="sxs-lookup"><span data-stu-id="e3ffd-159">**toosee hello job output**</span></span>

1. <span data-ttu-id="e3ffd-160">Z **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **účty úložiště**, klikněte pravým tlačítkem na hello výchozí účet Data Lake Storage a pak klikněte na tlačítko **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-160">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click hello default Data Lake Storage account, and then click **Explorer**.</span></span>
2. <span data-ttu-id="e3ffd-161">Klikněte dvakrát na **ukázky** tooopen hello složku a potom dvakrát klikněte na **výstupy**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-161">Double-click **Samples** tooopen hello folder, and then double-click **Outputs**.</span></span>
3. <span data-ttu-id="e3ffd-162">Klikněte dvakrát na **UnsuccessfulResponsees.log**.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-162">Double-click **UnsuccessfulResponsees.log**.</span></span>
4. <span data-ttu-id="e3ffd-163">Můžete také dvakrát kliknete na výstupní soubor hello uvnitř zobrazení grafu hello hello úlohy v pořadí toonavigate přímo toohello výstup.</span><span class="sxs-lookup"><span data-stu-id="e3ffd-163">You can also double-click hello output file inside hello graph view of hello job in order toonavigate directly toohello output.</span></span>

## <a name="see-also"></a><span data-ttu-id="e3ffd-164">Viz také</span><span class="sxs-lookup"><span data-stu-id="e3ffd-164">See also</span></span>
<span data-ttu-id="e3ffd-165">tooget začít s Data Lake Analytics pomocí různých nástrojů, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="e3ffd-165">tooget started with Data Lake Analytics using different tools, see:</span></span>

* [<span data-ttu-id="e3ffd-166">Začínáme se službou Data Lake Analytics pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e3ffd-166">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="e3ffd-167">Začínáme se službou Data Lake Analytics pomocí Azure PowerShellu</span><span class="sxs-lookup"><span data-stu-id="e3ffd-167">Get started with Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="e3ffd-168">Začínáme se službou Data Lake Analytics pomocí sady .NET SDK</span><span class="sxs-lookup"><span data-stu-id="e3ffd-168">Get started with Data Lake Analytics using .NET SDK</span></span>](data-lake-analytics-get-started-net-sdk.md)
