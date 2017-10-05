---
title: "Analýza dat v Data Lake Store pomocí Power BI | Microsoft Docs"
description: "Pomocí Power BI k analýze dat uložených v Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57d19d27-e135-49d9-a7ea-46c48ef4e3bd
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 0cf7e385ef2edd650479e120f52469bc6632f2eb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a><span data-ttu-id="c0250-103">Analýza dat v Data Lake Store pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="c0250-103">Analyze data in Data Lake Store by using Power BI</span></span>
<span data-ttu-id="c0250-104">V tomto článku se dozvíte, jak analyzovat a vizualizovat data uložená v Azure Data Lake Store pomocí Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="c0250-104">In this article you will learn how to use Power BI Desktop to analyze and visualize data stored in Azure Data Lake Store.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0250-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c0250-105">Prerequisites</span></span>
<span data-ttu-id="c0250-106">Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="c0250-106">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="c0250-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="c0250-107">**An Azure subscription**.</span></span> <span data-ttu-id="c0250-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c0250-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c0250-109">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="c0250-109">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="c0250-110">Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c0250-110">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="c0250-111">Tento článek předpokládá, že jste již vytvořili účet Data Lake Store, názvem **mybidatalakestore**a odesláno ukázkový datový soubor (**Drivers.txt**) k němu.</span><span class="sxs-lookup"><span data-stu-id="c0250-111">This article assumes that you have already created a Data Lake Store account, called **mybidatalakestore**, and uploaded a sample data file (**Drivers.txt**) to it.</span></span> <span data-ttu-id="c0250-112">Tento ukázkový soubor je k dispozici ke stažení [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="c0250-112">This sample file is available for download from [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span>
* <span data-ttu-id="c0250-113">**Power BI Desktop**.</span><span class="sxs-lookup"><span data-stu-id="c0250-113">**Power BI Desktop**.</span></span> <span data-ttu-id="c0250-114">Si můžete stáhnout z [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="c0250-114">You can download this from [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=45331).</span></span> 

## <a name="create-a-report-in-power-bi-desktop"></a><span data-ttu-id="c0250-115">Vytvoření sestavy v Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="c0250-115">Create a report in Power BI Desktop</span></span>
1. <span data-ttu-id="c0250-116">Power BI Desktop spustíte ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c0250-116">Launch Power BI Desktop on your computer.</span></span>
2. <span data-ttu-id="c0250-117">Z **Domů** pásu karet, klikněte na tlačítko **načíst Data**a pak klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="c0250-117">From the **Home** ribbon, click **Get Data**, and then click More.</span></span> <span data-ttu-id="c0250-118">V **načíst Data** dialogové okno, klikněte na tlačítko **Azure**, klikněte na tlačítko **Azure Data Lake Store**a potom klikněte na **Connect**.</span><span class="sxs-lookup"><span data-stu-id="c0250-118">In the **Get Data** dialog box, click **Azure**, click **Azure Data Lake Store**, and then click **Connect**.</span></span>
   
    <span data-ttu-id="c0250-119">![Připojení k Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "připojení k Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="c0250-119">![Connect to Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account.png "Connect to Data Lake Store")</span></span>
3. <span data-ttu-id="c0250-120">Pokud se zobrazí dialogové okno o konektoru se ve fázi vývoje, rozhodnout pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c0250-120">If you see a dialog box about the connector being in a development phase, opt to continue.</span></span>
4. <span data-ttu-id="c0250-121">V **Microsoft Azure Data Lake Store** dialogové okno, zadejte adresu URL k vašemu účtu Data Lake Store a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c0250-121">In the **Microsoft Azure Data Lake Store** dialog box, provide the URL to your Data Lake Store account, and then click **OK**.</span></span>
   
    <span data-ttu-id="c0250-122">![Adresa URL pro Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "adresu URL pro Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="c0250-122">![URL for Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL for Data Lake Store")</span></span>
5. <span data-ttu-id="c0250-123">V dialogovém okně Další klikněte na **přihlášení** pro přihlášení do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c0250-123">In the next dialog box, click **Sign in** to sign into Data Lake Store account.</span></span> <span data-ttu-id="c0250-124">Budete přesměrováni na přihlašovací stránku vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="c0250-124">You will be redirected to your organization's sign in page.</span></span> <span data-ttu-id="c0250-125">Postupujte podle výzev a přihlaste se k účtu.</span><span class="sxs-lookup"><span data-stu-id="c0250-125">Follow the prompts to sign into the account.</span></span>
   
    <span data-ttu-id="c0250-126">![Přihlaste se k Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "přihlásit ke službě Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="c0250-126">![Sign into Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Sign into Data Lake Store")</span></span>
6. <span data-ttu-id="c0250-127">Po úspěšném přihlášení, klikněte na tlačítko **Connect**.</span><span class="sxs-lookup"><span data-stu-id="c0250-127">After you have successfully signed in, click **Connect**.</span></span>
   
    <span data-ttu-id="c0250-128">![Připojení k Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "připojení k Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="c0250-128">![Connect to Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Connect to Data Lake Store")</span></span>
7. <span data-ttu-id="c0250-129">Dialogové okno Další ukazuje soubor, který jste nahráli do účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c0250-129">The next dialog box shows the file that you uploaded to your Data Lake Store account.</span></span> <span data-ttu-id="c0250-130">Ověřte informace a pak klikněte na tlačítko **zatížení**.</span><span class="sxs-lookup"><span data-stu-id="c0250-130">Verify the info and then click **Load**.</span></span>
   
    <span data-ttu-id="c0250-131">![Načtení dat z Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "načtení dat z Data Lake Store")</span><span class="sxs-lookup"><span data-stu-id="c0250-131">![Load data from Data Lake Store](./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Load data from Data Lake Store")</span></span>
8. <span data-ttu-id="c0250-132">Po data byla úspěšně načtena do Power BI, zobrazí se následující pole v **pole** kartě.</span><span class="sxs-lookup"><span data-stu-id="c0250-132">After the data has been successfully loaded into Power BI, you will see the following fields in the **Fields** tab.</span></span>
   
    <span data-ttu-id="c0250-133">![Importovat pole](./media/data-lake-store-power-bi/imported-fields.png "importovat pole")</span><span class="sxs-lookup"><span data-stu-id="c0250-133">![Imported fields](./media/data-lake-store-power-bi/imported-fields.png "Imported fields")</span></span>
   
    <span data-ttu-id="c0250-134">Však k vizualizaci a analýzu dat, můžeme dáváte přednost data, která mají být k dispozici pro následující pole</span><span class="sxs-lookup"><span data-stu-id="c0250-134">However, to visualize and analyze the data, we prefer the data to be available per the following fields</span></span>
   
    <span data-ttu-id="c0250-135">![Požadovaného pole](./media/data-lake-store-power-bi/desired-fields.png "požadovaných polí")</span><span class="sxs-lookup"><span data-stu-id="c0250-135">![Desired fields](./media/data-lake-store-power-bi/desired-fields.png "Desired fields")</span></span>
   
    <span data-ttu-id="c0250-136">V dalších krocích budeme aktualizovat dotaz převést importovaných dat v požadovaném formátu.</span><span class="sxs-lookup"><span data-stu-id="c0250-136">In the next steps, we will update the query to convert the imported data in the desired format.</span></span>
9. <span data-ttu-id="c0250-137">Z **Domů** pásu karet, klikněte na tlačítko **upravit dotazy**.</span><span class="sxs-lookup"><span data-stu-id="c0250-137">From the **Home** ribbon, click **Edit Queries**.</span></span>
   
    <span data-ttu-id="c0250-138">![Upravit dotazy](./media/data-lake-store-power-bi/edit-queries.png "úpravy dotazů")</span><span class="sxs-lookup"><span data-stu-id="c0250-138">![Edit queries](./media/data-lake-store-power-bi/edit-queries.png "Edit queries")</span></span>
10. <span data-ttu-id="c0250-139">V editoru dotazů v rámci **obsahu** sloupce, klikněte na tlačítko **binární**.</span><span class="sxs-lookup"><span data-stu-id="c0250-139">In the Query Editor, under the **Content** column, click **Binary**.</span></span>
    
    <span data-ttu-id="c0250-140">![Upravit dotazy](./media/data-lake-store-power-bi/convert-query1.png "úpravy dotazů")</span><span class="sxs-lookup"><span data-stu-id="c0250-140">![Edit queries](./media/data-lake-store-power-bi/convert-query1.png "Edit queries")</span></span>
11. <span data-ttu-id="c0250-141">Zobrazí se ikona souboru, který představuje **Drivers.txt** soubor, který jste nahráli.</span><span class="sxs-lookup"><span data-stu-id="c0250-141">You will see a file icon, that represents the **Drivers.txt** file that you uploaded.</span></span> <span data-ttu-id="c0250-142">Klikněte pravým tlačítkem na soubor a klikněte na tlačítko **CSV**.</span><span class="sxs-lookup"><span data-stu-id="c0250-142">Right-click the file, and click **CSV**.</span></span>    
    
    <span data-ttu-id="c0250-143">![Upravit dotazy](./media/data-lake-store-power-bi/convert-query2.png "úpravy dotazů")</span><span class="sxs-lookup"><span data-stu-id="c0250-143">![Edit queries](./media/data-lake-store-power-bi/convert-query2.png "Edit queries")</span></span>
12. <span data-ttu-id="c0250-144">Měli byste vidět výstup, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c0250-144">You should see an output as shown below.</span></span> <span data-ttu-id="c0250-145">Vaše data jsou nyní k dispozici ve formátu, který slouží k vytváření vizualizací.</span><span class="sxs-lookup"><span data-stu-id="c0250-145">Your data is now available in a format that you can use to create visualizations.</span></span>
    
    <span data-ttu-id="c0250-146">![Upravit dotazy](./media/data-lake-store-power-bi/convert-query3.png "úpravy dotazů")</span><span class="sxs-lookup"><span data-stu-id="c0250-146">![Edit queries](./media/data-lake-store-power-bi/convert-query3.png "Edit queries")</span></span>
13. <span data-ttu-id="c0250-147">Z **Domů** pásu karet, klikněte na tlačítko **zavřete a použít**a potom klikněte na **zavřete a použít**.</span><span class="sxs-lookup"><span data-stu-id="c0250-147">From the **Home** ribbon, click **Close and Apply**, and then click **Close and Apply**.</span></span>
    
    <span data-ttu-id="c0250-148">![Upravit dotazy](./media/data-lake-store-power-bi/load-edited-query.png "úpravy dotazů")</span><span class="sxs-lookup"><span data-stu-id="c0250-148">![Edit queries](./media/data-lake-store-power-bi/load-edited-query.png "Edit queries")</span></span>
14. <span data-ttu-id="c0250-149">Po aktualizaci dotazu **pole** se zobrazí nová pole, které jsou k dispozici pro vizualizaci.</span><span class="sxs-lookup"><span data-stu-id="c0250-149">Once the query is updated, the **Fields** tab will show the new fields available for visualization.</span></span>
    
    <span data-ttu-id="c0250-150">![Aktualizovat pole](./media/data-lake-store-power-bi/updated-query-fields.png "aktualizovat pole")</span><span class="sxs-lookup"><span data-stu-id="c0250-150">![Updated fields](./media/data-lake-store-power-bi/updated-query-fields.png "Updated fields")</span></span>
15. <span data-ttu-id="c0250-151">Dejte nám vytvořte výsečový graf představující ovladače v každé město pro dané země.</span><span class="sxs-lookup"><span data-stu-id="c0250-151">Let us create a pie chart to represent the drivers in each city for a given country.</span></span> <span data-ttu-id="c0250-152">To pokud chcete udělat, vyberte následující možnosti.</span><span class="sxs-lookup"><span data-stu-id="c0250-152">To do so, make the following selections.</span></span>
    
    1. <span data-ttu-id="c0250-153">Na kartě vizualizace kliknutím na symbol pro výsečového grafu.</span><span class="sxs-lookup"><span data-stu-id="c0250-153">From the Visualizations tab, click the symbol for a pie chart.</span></span>
       
        <span data-ttu-id="c0250-154">![Vytvoření výsečového grafu](./media/data-lake-store-power-bi/create-pie-chart.png "vytvořit výsečového grafu")</span><span class="sxs-lookup"><span data-stu-id="c0250-154">![Create pie chart](./media/data-lake-store-power-bi/create-pie-chart.png "Create pie chart")</span></span>
    2. <span data-ttu-id="c0250-155">Sloupce, které jsme se chystáte použít **sloupec 4** (název města) a **7 sloupci** (název země).</span><span class="sxs-lookup"><span data-stu-id="c0250-155">The columns that we are going to use are **Column 4** (name of the city) and **Column 7** (name of the country).</span></span> <span data-ttu-id="c0250-156">Přetáhněte tyto sloupce z **pole** kartu k **vizualizace** kartě, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="c0250-156">Drag these columns from **Fields** tab to **Visualizations** tab as shown below.</span></span>
       
        <span data-ttu-id="c0250-157">![Vytváření vizualizací](./media/data-lake-store-power-bi/create-visualizations.png "vytváření vizualizací")</span><span class="sxs-lookup"><span data-stu-id="c0250-157">![Create visualizations](./media/data-lake-store-power-bi/create-visualizations.png "Create visualizations")</span></span>
    3. <span data-ttu-id="c0250-158">Výsečový graf by měl nyní vypadat podobně jako následující.</span><span class="sxs-lookup"><span data-stu-id="c0250-158">The pie chart should now resemble like the one shown below.</span></span>
       
        <span data-ttu-id="c0250-159">![Výsečový graf](./media/data-lake-store-power-bi/pie-chart.png "vytváření vizualizací")</span><span class="sxs-lookup"><span data-stu-id="c0250-159">![Pie chart](./media/data-lake-store-power-bi/pie-chart.png "Create visualizations")</span></span>
16. <span data-ttu-id="c0250-160">Výběrem konkrétní země ze stránky úrovně filtry, uvidíte nyní počet ovladačů v každé město vybrané země.</span><span class="sxs-lookup"><span data-stu-id="c0250-160">By selecting a specific country from the page level filters, you can now see the number of drivers in each city of the selected country.</span></span> <span data-ttu-id="c0250-161">Například v položce **vizualizace** v části **stránky úrovně filtry**, vyberte **Brazílie**.</span><span class="sxs-lookup"><span data-stu-id="c0250-161">For example, under the **Visualizations** tab, under **Page level filters**, select **Brazil**.</span></span>
    
    <span data-ttu-id="c0250-162">![Vyberte zemi](./media/data-lake-store-power-bi/select-country.png "vyberte zemi")</span><span class="sxs-lookup"><span data-stu-id="c0250-162">![Select a country](./media/data-lake-store-power-bi/select-country.png "Select a country")</span></span>
17. <span data-ttu-id="c0250-163">Výsečový graf se automaticky aktualizuje a zobrazí ovladače ve městech Brazílie.</span><span class="sxs-lookup"><span data-stu-id="c0250-163">The pie chart is automatically updated to display the drivers in the cities of Brazil.</span></span>
    
    <span data-ttu-id="c0250-164">![Ovladače v určité zemi](./media/data-lake-store-power-bi/driver-per-country.png "ovladače podle země")</span><span class="sxs-lookup"><span data-stu-id="c0250-164">![Drivers in a country](./media/data-lake-store-power-bi/driver-per-country.png "Drivers per country")</span></span>
18. <span data-ttu-id="c0250-165">Z **soubor** nabídky, klikněte na tlačítko **Uložit** uložit vizualizaci jako soubor Power BI Desktop.</span><span class="sxs-lookup"><span data-stu-id="c0250-165">From the **File** menu, click **Save** to save the visualization as a Power BI Desktop file.</span></span>

## <a name="publish-report-to-power-bi-service"></a><span data-ttu-id="c0250-166">Publikovat sestavy do služby Power BI</span><span class="sxs-lookup"><span data-stu-id="c0250-166">Publish report to Power BI service</span></span>
<span data-ttu-id="c0250-167">Po vytvoření vizualizacemi v Power BI Desktop, můžete ji sdílet s ostatními ji publikujete do služby Power BI.</span><span class="sxs-lookup"><span data-stu-id="c0250-167">Once you have created the visualizations in Power BI Desktop, you can share it with others by publishing it to the Power BI service.</span></span> <span data-ttu-id="c0250-168">Pokyny o tom, jak to udělat najdete v tématu [publikování z Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).</span><span class="sxs-lookup"><span data-stu-id="c0250-168">For instructions on how to do that, see [Publish from Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).</span></span>

## <a name="see-also"></a><span data-ttu-id="c0250-169">Viz také</span><span class="sxs-lookup"><span data-stu-id="c0250-169">See also</span></span>
* [<span data-ttu-id="c0250-170">Analýza dat v Data Lake Store pomocí Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="c0250-170">Analyze data in Data Lake Store using Data Lake Analytics</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

