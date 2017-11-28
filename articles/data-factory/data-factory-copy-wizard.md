---
title: "Kopírování dat snadno pomocí Průvodce kopírováním - Azure | Microsoft Docs"
description: "Další informace o tom, jak pomocí Průvodce kopírováním Data Factory ke zkopírování dat z podporovaných zdrojů dat do jímky."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 282cb4484f8209e6bb36f2a02d7a897f1ba0aa8e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a><span data-ttu-id="1a0e1-103">Zkopírovat nebo přesunout data snadno pomocí Průvodce kopírováním objekt pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="1a0e1-103">Copy or move data easily with Azure Data Factory Copy Wizard</span></span>
<span data-ttu-id="1a0e1-104">Průvodce kopírováním Azure Data Factory je k usnadnění procesu příjem dat, který je obvykle prvním krokem v případě integrace pomocí dat začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-104">The Azure Data Factory Copy Wizard is to ease the process of ingesting data, which is usually a first step in an end-to-end data integration scenario.</span></span> <span data-ttu-id="1a0e1-105">Při průchodu přes Průvodce kopírováním Azure Data Factory, není potřeba pochopit žádné definice JSON propojené služby, datové sady a kanály.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-105">When going through the Azure Data Factory Copy Wizard, you do not need to understand any JSON definitions for linked services, datasets, and pipelines.</span></span> <span data-ttu-id="1a0e1-106">Ale po dokončení všech kroků v průvodci, průvodce automaticky vytvoří kanál ke zkopírování dat z vybraného zdroje dat do vybraného cílového umístění.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-106">However, after you complete all the steps in the wizard, the wizard automatically creates a pipeline to copy data from the selected data source to the selected destination.</span></span> <span data-ttu-id="1a0e1-107">Kromě toho Průvodce kopírováním umožňuje ověřit data se požity v době vytváření obsahu, které ukládá většinu doby, zvláště když je jsou příjem dat první ze zdroje dat.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-107">In addition, the Copy Wizard helps you to validate the data being ingested at the time of authoring, which saves much of your time, especially when you are ingesting data for the first time from the data source.</span></span> <span data-ttu-id="1a0e1-108">Chcete-li spustit Průvodce kopírováním, klikněte na tlačítko **kopírování dat** na domovské stránce objektu pro vytváření dat dlaždici.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-108">To start the Copy Wizard, click the **Copy data** tile on the home page of your data factory.</span></span>

![Průvodce kopírováním](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a><span data-ttu-id="1a0e1-110">Intuitivní Průvodce pro kopírování dat</span><span class="sxs-lookup"><span data-stu-id="1a0e1-110">An intuitive wizard for copying data</span></span>
<span data-ttu-id="1a0e1-111">Tento průvodce umožňuje snadno přesunout data z různých zdrojů a cílů v minutách.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-111">This wizard allows you to easily move data from a wide variety of sources to destinations in minutes.</span></span> <span data-ttu-id="1a0e1-112">Poté, co projde průvodce, kanál s aktivitou kopírování se automaticky vytvoří za vás společně s závislé entity služby Data Factory (propojené služby a datové sady).</span><span class="sxs-lookup"><span data-stu-id="1a0e1-112">After going through the wizard, a pipeline with a copy activity is automatically created for you along with dependent Data Factory entities (linked services and datasets).</span></span> <span data-ttu-id="1a0e1-113">Žádné další kroky jsou nutné k vytvoření kanálu.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-113">No additional steps are required to create the pipeline.</span></span>   

![Vyberte zdroj dat](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> <span data-ttu-id="1a0e1-115">V tématu [Průvodce kopírováním kurzu](data-factory-copy-data-wizard-tutorial.md) článku podrobné pokyny k vytvoření ukázkový kanál kopírování dat z Azure blob do tabulky Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-115">See [Copy Wizard tutorial](data-factory-copy-data-wizard-tutorial.md) article for step-by-step instructions to create a sample pipeline to copy data from an Azure blob to an Azure SQL Database table.</span></span> 
> 
> 

<span data-ttu-id="1a0e1-116">Průvodce je určený s velkým objemem dat v paměti od začátku.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-116">The wizard is designed with big data in mind from the start.</span></span> <span data-ttu-id="1a0e1-117">Je jednoduchý a efektivně vytvářet kanály pro vytváření dat, které přesunout stovky složky, soubory nebo pomocí Průvodce kopírování dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-117">It is simple and efficient to author Data Factory pipelines that move hundreds of folders, files, or tables using the Copy Data wizard.</span></span> <span data-ttu-id="1a0e1-118">Průvodce podporuje následujících tří funkcí: náhled dat, schématu zachycení a mapování a filtrování dat.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-118">The wizard supports the following three features: Automatic data preview, schema capture and mapping, and filtering data.</span></span> 

## <a name="automatic-data-preview"></a><span data-ttu-id="1a0e1-119">Náhled automatické dat</span><span class="sxs-lookup"><span data-stu-id="1a0e1-119">Automatic data preview</span></span>
<span data-ttu-id="1a0e1-120">Průvodce kopírováním umožňuje zkontrolovat část dat ze zdroje dat vybraných pro vás k ověření toho, jestli data je správná data, kterou chcete zkopírovat.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-120">The copy wizard allows you to review part of the data from the selected data source for you to validate whether the data it is the right data you want to copy.</span></span> <span data-ttu-id="1a0e1-121">Kromě toho pokud je zdrojová data do textového souboru, Průvodce kopírováním analyzuje textový soubor automaticky další řádků a sloupců oddělovače a schéma.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-121">In addition, if the source data is in a text file, the copy wizard parses the text file to learn row and column delimiters, and schema automatically.</span></span> 

![Nastavení formátu souboru](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a><span data-ttu-id="1a0e1-123">Schéma zachycení a mapování</span><span class="sxs-lookup"><span data-stu-id="1a0e1-123">Schema capture and mapping</span></span>
<span data-ttu-id="1a0e1-124">Schéma vstupní data nemusí odpovídat schématu výstupních dat, v některých případech.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-124">The schema of input data may not match the schema of output data in some cases.</span></span> <span data-ttu-id="1a0e1-125">V tomto scénáři budete muset mapování sloupců ze schématu zdroje na sloupce ze schématu cílové.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-125">In this scenario, you need to map columns from the source schema to columns from the destination schema.</span></span> 

<span data-ttu-id="1a0e1-126">Průvodce kopírováním automaticky mapuje sloupců ve schématu zdroje na sloupce ve schématu cílové.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-126">The copy wizard automatically maps columns in the source schema to columns in the destination schema.</span></span> <span data-ttu-id="1a0e1-127">Můžete přepsat mapování pomocí rozevíracích seznamů (nebo) určete, zda sloupec je nutné při kopírování dat.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-127">You can override the mappings by using the drop-down lists (or) specify whether a column needs to be skipped while copying the data.</span></span>   

![Schéma mapování](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a><span data-ttu-id="1a0e1-129">Filtrování dat</span><span class="sxs-lookup"><span data-stu-id="1a0e1-129">Filtering data</span></span>
<span data-ttu-id="1a0e1-130">Průvodce vám umožní filtrovat zdroje dat a vyberte pouze data, která je možné zkopírovat do úložiště dat cílové nebo podřízený.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-130">The wizard allows you to filter source data to select only the data that needs to be copied to the destination/sink data store.</span></span> <span data-ttu-id="1a0e1-131">Filtrování snižuje objem dat, který se má zkopírovat do úložiště dat podřízený a proto zvyšuje propustnost kopírování.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-131">Filtering reduces the volume of the data to be copied to the sink data store and therefore enhances the throughput of the copy operation.</span></span> <span data-ttu-id="1a0e1-132">Poskytuje flexibilní způsob, jak filtrování dat v relační databázi pomocí SQL dotazu jazyka (nebo) soubory ve složce aplikace objektů blob v Azure pomocí [Data Factory funkce a proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="1a0e1-132">It provides a flexible way to filter data in a relational database by using SQL query language (or) files in an Azure blob folder by using [Data Factory functions and variables](data-factory-functions-variables.md).</span></span>   

### <a name="filtering-of-data-in-a-database"></a><span data-ttu-id="1a0e1-133">Filtrování dat v databázi</span><span class="sxs-lookup"><span data-stu-id="1a0e1-133">Filtering of data in a database</span></span>
<span data-ttu-id="1a0e1-134">V příkladu se používá příkaz jazyka SQL `Text.Format` funkce a `WindowStart` proměnné.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-134">In the example, the SQL query uses the `Text.Format` function and `WindowStart` variable.</span></span> 

![Ověření výrazy](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a><span data-ttu-id="1a0e1-136">Filtrování dat v Azure blob složce</span><span class="sxs-lookup"><span data-stu-id="1a0e1-136">Filtering of data in an Azure blob folder</span></span>
<span data-ttu-id="1a0e1-137">Proměnné v cestě ke složce můžete použít ke zkopírování dat z složku, která je určena za běhu na základě [systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="1a0e1-137">You can use variables in the folder path to copy data from a folder that is determined at runtime based on [system variables](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="1a0e1-138">Podporované proměnné jsou: **{year}**, **{month}**, **{day}**, **{hodinu}**, **{minutu}**, a **{vlastní}**.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-138">The supported variables are: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, and **{custom}**.</span></span> <span data-ttu-id="1a0e1-139">Příklad: inputfolder / {year} / {month} / {day}.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-139">Example: inputfolder/{year}/{month}/{day}.</span></span>

<span data-ttu-id="1a0e1-140">Předpokládejme, že máte vstupní složky v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="1a0e1-140">Suppose that you have input folders in the following format:</span></span>

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

<span data-ttu-id="1a0e1-141">Klikněte na tlačítko **Procházet** tlačítko pro **souboru nebo složky**, přejděte do jednoho z těchto složek (například 2016 -> 03 -> 01 -> 02) a klikněte na tlačítko **zvolte**.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-141">Click the **Browse** button for **File or folder**, browse to one of these folders (for example, 2016->03->01->02), and click **Choose**.</span></span> <span data-ttu-id="1a0e1-142">Měli byste vidět `2016/03/01/02` v textovém poli.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-142">You should see `2016/03/01/02` in the text box.</span></span> <span data-ttu-id="1a0e1-143">Nyní, nahraďte **2016** s **{year}**, **03** s **{month}**, **01** s **{day}**, a **02** s **{hodinu}**, a stiskněte klávesu Tab.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-143">Now, replace **2016** with **{year}**, **03** with **{month}**, **01** with **{day}**, and **02** with **{hour}**, and press Tab.</span></span> <span data-ttu-id="1a0e1-144">Měli byste vidět rozevíracích seznamech vyberte formát pro tyto čtyři proměnné:</span><span class="sxs-lookup"><span data-stu-id="1a0e1-144">You should see drop-down lists to select the format for these four variables:</span></span>

![Pomocí systémové proměnné](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

<span data-ttu-id="1a0e1-146">Jak je znázorněno na následujícím snímku obrazovky, můžete použít také **vlastní** proměnné a všechny [podporované řetězce formátu](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span><span class="sxs-lookup"><span data-stu-id="1a0e1-146">As shown in the following screenshot, you can also use a **custom** variable and any [supported format strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span></span> <span data-ttu-id="1a0e1-147">Chcete-li vybrat jinou složku s této struktury, použijte **Procházet** nejprve tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-147">To select a folder with that structure, use the **Browse** button first.</span></span> <span data-ttu-id="1a0e1-148">Potom můžete nahradit hodnotu s **{vlastní}**, a stiskněte klávesu Tab zobrazíte textového pole, můžete zadat řetězec formátu.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-148">Then replace a value with **{custom}**, and press Tab to see the text box where you can type the format string.</span></span>     

![Použití vlastní proměnné](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a><span data-ttu-id="1a0e1-150">Podpora různých dat a typy objektů</span><span class="sxs-lookup"><span data-stu-id="1a0e1-150">Support for diverse data and object types</span></span>
<span data-ttu-id="1a0e1-151">Pomocí Průvodce kopírováním můžete efektivně přesouvají stovky složky, soubory nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-151">By using the Copy Wizard, you can efficiently move hundreds of folders, files, or tables.</span></span>

![Vyberte tabulky, ze kterého chcete zkopírovat data](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a><span data-ttu-id="1a0e1-153">Možnosti plánování</span><span class="sxs-lookup"><span data-stu-id="1a0e1-153">Scheduling options</span></span>
<span data-ttu-id="1a0e1-154">Operace kopírování můžete spustit jednou nebo podle plánu (každou hodinu, denně, a tak dále).</span><span class="sxs-lookup"><span data-stu-id="1a0e1-154">You can run the copy operation once or on a schedule (hourly, daily, and so on).</span></span> <span data-ttu-id="1a0e1-155">Obě tyto možnosti lze použít pro šířky konektory napříč místními, cloud a místní kopie plochy.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-155">Both of these options can be used for the breadth of the connectors across on-premises, cloud, and local desktop copy.</span></span>

<span data-ttu-id="1a0e1-156">Operace kopírování jednorázové umožňuje přesun dat ze zdroje do cíle pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-156">A one-time copy operation enables data movement from a source to a destination only once.</span></span> <span data-ttu-id="1a0e1-157">Platí pro data libovolné velikosti a jakýkoli podporovaný formát.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-157">It applies to data of any size and any supported format.</span></span> <span data-ttu-id="1a0e1-158">Naplánované kopie umožňuje kopírovat data na předepsané opakování.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-158">The scheduled copy allows you to copy data on a prescribed recurrence.</span></span> <span data-ttu-id="1a0e1-159">Bohaté nastavení (např. Zkuste to znovu, vypršení časového limitu a upozornění) můžete použít ke konfiguraci naplánované kopírování.</span><span class="sxs-lookup"><span data-stu-id="1a0e1-159">You can use rich settings (like retry, timeout, and alerts) to configure the scheduled copy.</span></span>

![Plánování vlastnosti](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a><span data-ttu-id="1a0e1-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a0e1-161">Next steps</span></span>
<span data-ttu-id="1a0e1-162">Rychlé návod k vytvoření kanálu s aktivitou kopírování pomocí Průvodce kopírováním Data Factory najdete v tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1a0e1-162">For a quick walkthrough of using the Data Factory Copy Wizard to create a pipeline with Copy Activity, see [Tutorial: Create a pipeline using the Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

