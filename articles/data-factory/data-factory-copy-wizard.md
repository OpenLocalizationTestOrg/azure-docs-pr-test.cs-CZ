---
title: "aaaCopy dat snadno pomocí Průvodce kopírováním - Azure | Microsoft Docs"
description: "Informace o tom, jak toouse hello Průvodce kopírováním služby Data Factory toocopy data z toosinks podporované datové zdroje."
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
ms.openlocfilehash: 99437ec16facf3b94c8be18487ec89e9f13acc9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a><span data-ttu-id="40069-103">Zkopírovat nebo přesunout data snadno pomocí Průvodce kopírováním objekt pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="40069-103">Copy or move data easily with Azure Data Factory Copy Wizard</span></span>
<span data-ttu-id="40069-104">Hello Průvodce kopírováním objekt pro vytváření dat Azure je proces hello tooease příjem dat, který je obvykle prvním krokem v případě integrace pomocí dat začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="40069-104">hello Azure Data Factory Copy Wizard is tooease hello process of ingesting data, which is usually a first step in an end-to-end data integration scenario.</span></span> <span data-ttu-id="40069-105">Při průchodu přes hello Průvodce kopírováním objekt pro vytváření dat Azure, není nutné toounderstand všechny definice JSON propojené služby, datové sady a kanály.</span><span class="sxs-lookup"><span data-stu-id="40069-105">When going through hello Azure Data Factory Copy Wizard, you do not need toounderstand any JSON definitions for linked services, datasets, and pipelines.</span></span> <span data-ttu-id="40069-106">Ale po dokončení všech kroků hello v Průvodci hello hello průvodce automaticky vytvoří toocopy datový kanál z hello vybraná data zdroj toohello vybrané cíl.</span><span class="sxs-lookup"><span data-stu-id="40069-106">However, after you complete all hello steps in hello wizard, hello wizard automatically creates a pipeline toocopy data from hello selected data source toohello selected destination.</span></span> <span data-ttu-id="40069-107">Kromě toho hello Průvodce kopírováním vám pomůže toovalidate hello dat probíhá požity hello během vytváření, které ukládá většinu doby, hlavně když jste jsou příjem dat pro hello poprvé ze zdroje dat hello.</span><span class="sxs-lookup"><span data-stu-id="40069-107">In addition, hello Copy Wizard helps you toovalidate hello data being ingested at hello time of authoring, which saves much of your time, especially when you are ingesting data for hello first time from hello data source.</span></span> <span data-ttu-id="40069-108">toostart hello Průvodce kopírováním, klikněte na tlačítko hello **kopírování dat** dlaždici na domovskou stránku hello svojí datové továrny.</span><span class="sxs-lookup"><span data-stu-id="40069-108">toostart hello Copy Wizard, click hello **Copy data** tile on hello home page of your data factory.</span></span>

![Průvodce kopírováním](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a><span data-ttu-id="40069-110">Intuitivní Průvodce pro kopírování dat</span><span class="sxs-lookup"><span data-stu-id="40069-110">An intuitive wizard for copying data</span></span>
<span data-ttu-id="40069-111">Tento průvodce vám umožní tooeasily přesun dat z široké škály zdrojů toodestinations v minutách.</span><span class="sxs-lookup"><span data-stu-id="40069-111">This wizard allows you tooeasily move data from a wide variety of sources toodestinations in minutes.</span></span> <span data-ttu-id="40069-112">Poté, co projde hello průvodce, kanál s aktivitou kopírování se automaticky vytvoří za vás společně s závislé entity služby Data Factory (propojené služby a datové sady).</span><span class="sxs-lookup"><span data-stu-id="40069-112">After going through hello wizard, a pipeline with a copy activity is automatically created for you along with dependent Data Factory entities (linked services and datasets).</span></span> <span data-ttu-id="40069-113">Nejsou žádné další kroky požadované toocreate hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="40069-113">No additional steps are required toocreate hello pipeline.</span></span>   

![Vyberte zdroj dat](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> <span data-ttu-id="40069-115">V tématu [Průvodce kopírováním kurzu](data-factory-copy-data-wizard-tutorial.md) článek pro podrobné pokyny toocreate ukázková data toocopy kanálu z tabulky Azure SQL Database tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="40069-115">See [Copy Wizard tutorial](data-factory-copy-data-wizard-tutorial.md) article for step-by-step instructions toocreate a sample pipeline toocopy data from an Azure blob tooan Azure SQL Database table.</span></span> 
> 
> 

<span data-ttu-id="40069-116">Průvodce Hello je určen s velkým objemem dat v paměti od začátku hello.</span><span class="sxs-lookup"><span data-stu-id="40069-116">hello wizard is designed with big data in mind from hello start.</span></span> <span data-ttu-id="40069-117">Je jednoduchý a efektivní tooauthor Data Factory kanály, které přesunout stovky složky, soubory nebo tabulky pomocí Průvodce kopírování dat hello.</span><span class="sxs-lookup"><span data-stu-id="40069-117">It is simple and efficient tooauthor Data Factory pipelines that move hundreds of folders, files, or tables using hello Copy Data wizard.</span></span> <span data-ttu-id="40069-118">Hello Průvodce podporuje následující tři funkce hello: náhled dat, schématu zachycení a mapování a filtrování dat.</span><span class="sxs-lookup"><span data-stu-id="40069-118">hello wizard supports hello following three features: Automatic data preview, schema capture and mapping, and filtering data.</span></span> 

## <a name="automatic-data-preview"></a><span data-ttu-id="40069-119">Náhled automatické dat</span><span class="sxs-lookup"><span data-stu-id="40069-119">Automatic data preview</span></span>
<span data-ttu-id="40069-120">Průvodce kopírováním Hello umožňuje vám tooreview hello data z hello vybrané části zdroj dat pro vás toovalidate zda hello dat je hello pravým data, která chcete toocopy.</span><span class="sxs-lookup"><span data-stu-id="40069-120">hello copy wizard allows you tooreview part of hello data from hello selected data source for you toovalidate whether hello data it is hello right data you want toocopy.</span></span> <span data-ttu-id="40069-121">Kromě toho pokud hello zdrojová data do textového souboru, Průvodce kopírováním hello analyzuje hello text souboru toolearn řádků a sloupců oddělovače a schématu automaticky.</span><span class="sxs-lookup"><span data-stu-id="40069-121">In addition, if hello source data is in a text file, hello copy wizard parses hello text file toolearn row and column delimiters, and schema automatically.</span></span> 

![Nastavení formátu souboru](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a><span data-ttu-id="40069-123">Schéma zachycení a mapování</span><span class="sxs-lookup"><span data-stu-id="40069-123">Schema capture and mapping</span></span>
<span data-ttu-id="40069-124">schéma Hello vstupních dat nemusí odpovídat schématu hello výstupních dat, v některých případech.</span><span class="sxs-lookup"><span data-stu-id="40069-124">hello schema of input data may not match hello schema of output data in some cases.</span></span> <span data-ttu-id="40069-125">V tomto scénáři musíte toomap sloupců z hello zdroj schématu toocolumns od schématu cílové hello.</span><span class="sxs-lookup"><span data-stu-id="40069-125">In this scenario, you need toomap columns from hello source schema toocolumns from hello destination schema.</span></span> 

<span data-ttu-id="40069-126">Průvodce kopírováním Hello automaticky mapuje sloupců v hello zdroj schématu toocolumns ve schématu cílové hello.</span><span class="sxs-lookup"><span data-stu-id="40069-126">hello copy wizard automatically maps columns in hello source schema toocolumns in hello destination schema.</span></span> <span data-ttu-id="40069-127">Můžete přepsat hello mapování pomocí rozevíracích seznamů hello (nebo) určete, zda sloupec musí toobe při kopírování dat hello vynecháno.</span><span class="sxs-lookup"><span data-stu-id="40069-127">You can override hello mappings by using hello drop-down lists (or) specify whether a column needs toobe skipped while copying hello data.</span></span>   

![Schéma mapování](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a><span data-ttu-id="40069-129">Filtrování dat</span><span class="sxs-lookup"><span data-stu-id="40069-129">Filtering data</span></span>
<span data-ttu-id="40069-130">Průvodce Hello vám umožní toofilter zdroje dat tooselect pouze hello data, která potřebují úložiště dat cílové/jímka toohello toobe zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="40069-130">hello wizard allows you toofilter source data tooselect only hello data that needs toobe copied toohello destination/sink data store.</span></span> <span data-ttu-id="40069-131">Filtrování snižuje objem hello hello toobe zkopírovaný toohello jímku dat data ukládat a proto zvyšuje propustnost hello operace kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="40069-131">Filtering reduces hello volume of hello data toobe copied toohello sink data store and therefore enhances hello throughput of hello copy operation.</span></span> <span data-ttu-id="40069-132">Poskytuje flexibilní toofilter dat v relační databázi pomocí SQL dotazu jazyka (nebo) soubory ve složce aplikace objektů blob v Azure pomocí [Data Factory funkce a proměnné](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="40069-132">It provides a flexible way toofilter data in a relational database by using SQL query language (or) files in an Azure blob folder by using [Data Factory functions and variables](data-factory-functions-variables.md).</span></span>   

### <a name="filtering-of-data-in-a-database"></a><span data-ttu-id="40069-133">Filtrování dat v databázi</span><span class="sxs-lookup"><span data-stu-id="40069-133">Filtering of data in a database</span></span>
<span data-ttu-id="40069-134">V příkladu hello dotazu SQL hello používá hello `Text.Format` funkce a `WindowStart` proměnné.</span><span class="sxs-lookup"><span data-stu-id="40069-134">In hello example, hello SQL query uses hello `Text.Format` function and `WindowStart` variable.</span></span> 

![Ověření výrazy](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a><span data-ttu-id="40069-136">Filtrování dat v Azure blob složce</span><span class="sxs-lookup"><span data-stu-id="40069-136">Filtering of data in an Azure blob folder</span></span>
<span data-ttu-id="40069-137">Proměnné můžete použít v hello složky cesta toocopy data ze složky, která je určena za běhu na základě [systémové proměnné](data-factory-functions-variables.md#data-factory-system-variables).</span><span class="sxs-lookup"><span data-stu-id="40069-137">You can use variables in hello folder path toocopy data from a folder that is determined at runtime based on [system variables](data-factory-functions-variables.md#data-factory-system-variables).</span></span> <span data-ttu-id="40069-138">jsou podporovány Hello proměnné: **{year}**, **{month}**, **{day}**, **{hodinu}**, **{minutu}**, a **{vlastní}**.</span><span class="sxs-lookup"><span data-stu-id="40069-138">hello supported variables are: **{year}**, **{month}**, **{day}**, **{hour}**, **{minute}**, and **{custom}**.</span></span> <span data-ttu-id="40069-139">Příklad: inputfolder / {year} / {month} / {day}.</span><span class="sxs-lookup"><span data-stu-id="40069-139">Example: inputfolder/{year}/{month}/{day}.</span></span>

<span data-ttu-id="40069-140">Předpokládejme, že máte vstupní složky ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="40069-140">Suppose that you have input folders in hello following format:</span></span>

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

<span data-ttu-id="40069-141">Klikněte na tlačítko hello **Procházet** tlačítko pro **souboru nebo složky**, procházet tooone tyto složky (například 2016 -> 03 -> 01 -> 02) a klikněte na tlačítko **zvolte**.</span><span class="sxs-lookup"><span data-stu-id="40069-141">Click hello **Browse** button for **File or folder**, browse tooone of these folders (for example, 2016->03->01->02), and click **Choose**.</span></span> <span data-ttu-id="40069-142">Měli byste vidět `2016/03/01/02` hello textového pole.</span><span class="sxs-lookup"><span data-stu-id="40069-142">You should see `2016/03/01/02` in hello text box.</span></span> <span data-ttu-id="40069-143">Nyní, nahraďte **2016** s **{year}**, **03** s **{month}**, **01** s **{day}**, a **02** s **{hodinu}**, a stiskněte klávesu Tab. Měli byste vidět formát hello tooselect rozevírací seznamy pro tyto čtyři proměnné:</span><span class="sxs-lookup"><span data-stu-id="40069-143">Now, replace **2016** with **{year}**, **03** with **{month}**, **01** with **{day}**, and **02** with **{hour}**, and press Tab. You should see drop-down lists tooselect hello format for these four variables:</span></span>

![Pomocí systémové proměnné](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

<span data-ttu-id="40069-145">Jak ukazuje následující snímek obrazovky hello, můžete použít také **vlastní** proměnné a všechny [podporované řetězce formátu](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span><span class="sxs-lookup"><span data-stu-id="40069-145">As shown in hello following screenshot, you can also use a **custom** variable and any [supported format strings](https://msdn.microsoft.com/library/8kb3ddd4.aspx).</span></span> <span data-ttu-id="40069-146">tooselect složka s této struktury použití hello **Procházet** nejprve tlačítko.</span><span class="sxs-lookup"><span data-stu-id="40069-146">tooselect a folder with that structure, use hello **Browse** button first.</span></span> <span data-ttu-id="40069-147">Potom můžete nahradit hodnotu s **{vlastní}**a stiskněte klávesu Tab toosee hello textového pole můžete zadat řetězec formátu hello.</span><span class="sxs-lookup"><span data-stu-id="40069-147">Then replace a value with **{custom}**, and press Tab toosee hello text box where you can type hello format string.</span></span>     

![Použití vlastní proměnné](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a><span data-ttu-id="40069-149">Podpora různých dat a typy objektů</span><span class="sxs-lookup"><span data-stu-id="40069-149">Support for diverse data and object types</span></span>
<span data-ttu-id="40069-150">Pomocí Průvodce kopírováním hello můžete efektivně přesouvají stovky složky, soubory nebo tabulky.</span><span class="sxs-lookup"><span data-stu-id="40069-150">By using hello Copy Wizard, you can efficiently move hundreds of folders, files, or tables.</span></span>

![Vyberte tabulky, z které toocopy dat](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a><span data-ttu-id="40069-152">Možnosti plánování</span><span class="sxs-lookup"><span data-stu-id="40069-152">Scheduling options</span></span>
<span data-ttu-id="40069-153">Operace kopírování hello můžete spustit jednou nebo podle plánu (každou hodinu, denně, a tak dále).</span><span class="sxs-lookup"><span data-stu-id="40069-153">You can run hello copy operation once or on a schedule (hourly, daily, and so on).</span></span> <span data-ttu-id="40069-154">Obě tyto možnosti lze použít pro hello spektra hello konektory napříč místními, cloud a místní kopie plochy.</span><span class="sxs-lookup"><span data-stu-id="40069-154">Both of these options can be used for hello breadth of hello connectors across on-premises, cloud, and local desktop copy.</span></span>

<span data-ttu-id="40069-155">Operace kopírování jednorázové umožňuje přesun dat z zdroj tooa cíl pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="40069-155">A one-time copy operation enables data movement from a source tooa destination only once.</span></span> <span data-ttu-id="40069-156">Bude se vztahovat toodata jakékoli velikosti a jakýkoli podporovaný formát.</span><span class="sxs-lookup"><span data-stu-id="40069-156">It applies toodata of any size and any supported format.</span></span> <span data-ttu-id="40069-157">kopírování Hello naplánované umožňuje toocopy data v předepsaných opakování.</span><span class="sxs-lookup"><span data-stu-id="40069-157">hello scheduled copy allows you toocopy data on a prescribed recurrence.</span></span> <span data-ttu-id="40069-158">Můžete použít bohaté nastavení (např. Zkuste to znovu, vypršení časového limitu a upozornění) tooconfigure hello naplánované kopírování.</span><span class="sxs-lookup"><span data-stu-id="40069-158">You can use rich settings (like retry, timeout, and alerts) tooconfigure hello scheduled copy.</span></span>

![Plánování vlastnosti](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a><span data-ttu-id="40069-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="40069-160">Next steps</span></span>
<span data-ttu-id="40069-161">Rychlé návod k používání hello Průvodce kopírováním služby Data Factory toocreate kanál s aktivitou kopírování, najdete v části [kurz: vytvoření kanálu pomocí Průvodce kopírováním hello](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="40069-161">For a quick walkthrough of using hello Data Factory Copy Wizard toocreate a pipeline with Copy Activity, see [Tutorial: Create a pipeline using hello Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

