---
title: "aaaImport data do nástroje Machine Learning Studio | Microsoft Docs"
description: "Jak tooimport data do Azure Machine Learning Studio z různých zdrojů dat.. Zjistěte, jaké datové typy a formáty dat jsou podporovány."
keywords: "Importujte dat, formát dat, datové typy, zdroje dat, Cvičná data"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c194ee3b-838c-4efe-bb2a-c1d052326216
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: garye;bradsev
ms.openlocfilehash: 830dcdde9d43809900c520a41d6d94a65731ca3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a><span data-ttu-id="48a87-105">Import cvičných dat do nástroje Azure Machine Learning Studio z různých zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="48a87-105">Import your training data into Azure Machine Learning Studio from various data sources</span></span>
<span data-ttu-id="48a87-106">toouse svoje vlastní data v Machine Learning Studio toodevelop a train řešení prediktivní analýzy, můžete:</span><span class="sxs-lookup"><span data-stu-id="48a87-106">toouse your own data in Machine Learning Studio toodevelop and train a predictive analytics solution, you can:</span></span> 

* <span data-ttu-id="48a87-107">nahrání dat z **místního souboru** dříve času z vašeho pevného disku toocreate datovou sadu modulu v pracovním prostoru</span><span class="sxs-lookup"><span data-stu-id="48a87-107">upload data from a **local file** ahead of time from your hard drive toocreate a dataset module in your workspace</span></span>
* <span data-ttu-id="48a87-108">přístup k datům z jednoho z několika **zdroje dat online** experimentu je spuštěn pomocí hello [importovat Data] [ import-data] modulu</span><span class="sxs-lookup"><span data-stu-id="48a87-108">access data from one of several **online data sources** while your experiment is running using hello [Import Data][import-data] module</span></span> 
* <span data-ttu-id="48a87-109">použít data z jiného Azure Machine learning **experimentovat** uložit jako datové sady</span><span class="sxs-lookup"><span data-stu-id="48a87-109">use data from another Azure Machine learning **experiment** saved as a dataset</span></span>
* <span data-ttu-id="48a87-110">použít data z místního **databáze systému SQL Server**</span><span class="sxs-lookup"><span data-stu-id="48a87-110">use data from an on-premises **SQL Server database**</span></span>

<span data-ttu-id="48a87-111">Každá z těchto možností je popsán v tématech hello v nabídce hello níže.</span><span class="sxs-lookup"><span data-stu-id="48a87-111">Each of these options is described in one of hello topics on hello menu below.</span></span> <span data-ttu-id="48a87-112">Tato témata ukazují, jak tooimport dat od těchto různé zdroje toouse v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="48a87-112">These topics show you how tooimport data from these various data sources toouse in Machine Learning Studio.</span></span> 

[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

> [!NOTE]
> <span data-ttu-id="48a87-113">Nejsou k dispozici v Machine Learning Studio, který můžete použít pro Cvičná data několik ukázkových datových sad.</span><span class="sxs-lookup"><span data-stu-id="48a87-113">There are a number of sample datasets available in Machine Learning Studio that you can use for training data.</span></span> <span data-ttu-id="48a87-114">Informace naleznete v tématu [použít hello ukázkových datových sad v nástroji Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).</span><span class="sxs-lookup"><span data-stu-id="48a87-114">For information on these, see [Use hello sample datasets in Azure Machine Learning Studio](machine-learning-use-sample-datasets.md)).</span></span>
> 
> 

<span data-ttu-id="48a87-115">Toto úvodní téma také popisuje, jak tooget dat připravené pro použití v nástroji Machine Learning Studio a popisuje, jaké formáty dat a datové typy jsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="48a87-115">This introductory topic also discusses how tooget data ready for use in Machine Learning Studio and describes which data formats and data types are supported.</span></span> 

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a><span data-ttu-id="48a87-116">Příprava dat pro použití v nástroji Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="48a87-116">Get data ready for use in Azure Machine Learning Studio</span></span>
<span data-ttu-id="48a87-117">Machine Learning Studio je navrženou toowork obdélníková nebo tabulkovém daty, jako je například textová data, která má oddělený nebo strukturovaná data z databáze, i když se v některých případech může použít obdélníkový data.</span><span class="sxs-lookup"><span data-stu-id="48a87-117">Machine Learning Studio is designed toowork with rectangular or tabular data, such as text data that's delimited or structured data from a database, though in some circumstances non-rectangular data may be used.</span></span>

<span data-ttu-id="48a87-118">Je nejvhodnější Pokud vaše data jsou relativně vyčistit.</span><span class="sxs-lookup"><span data-stu-id="48a87-118">It's best if your data is relatively clean.</span></span> <span data-ttu-id="48a87-119">To znamená budete muset tootake péče o problémy, jako jsou třeba nekotovaných řetězce před nahráním dat hello do experimentu.</span><span class="sxs-lookup"><span data-stu-id="48a87-119">That is, you'll want tootake care of issues such as unquoted strings before you upload hello data into your experiment.</span></span>

<span data-ttu-id="48a87-120">Však nejsou k dispozici v Machine Learning Studio, které umožňují některé manipulaci s daty v rámci experimentu moduly.</span><span class="sxs-lookup"><span data-stu-id="48a87-120">However, there are modules available in Machine Learning Studio that enable some manipulation of data within your experiment.</span></span> <span data-ttu-id="48a87-121">V závislosti na tom, které budete používat algoritmů strojového učení hello, může být nutné toodecide jak budete zpracovávají data strukturální problémy, například chybějící hodnoty a zhuštěných dat a jsou moduly, které mohou pomoci s které.</span><span class="sxs-lookup"><span data-stu-id="48a87-121">Depending on hello machine learning algorithms you'll be using, you may need toodecide how you'll handle data structural issues such as missing values and sparse data, and there are modules that can help with that.</span></span> <span data-ttu-id="48a87-122">Hledat v hello **transformaci dat** části palety modulů hello pro moduly, které provádějí tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="48a87-122">Look in hello **Data Transformation** section of hello module palette for modules that perform these functions.</span></span>

<span data-ttu-id="48a87-123">V libovolném bodě v experimentu můžete zobrazit nebo stáhnout hello data, která je produkovaný modul kliknutím na výstupní port hello.</span><span class="sxs-lookup"><span data-stu-id="48a87-123">At any point in your experiment you can view or download hello data that's produced by a module by clicking hello output port.</span></span> <span data-ttu-id="48a87-124">V závislosti na modulu hello je možné možnosti různých stahování k dispozici, nebo může být schopný toovisualize hello dat webového prohlížeče v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="48a87-124">Depending on hello module, there may be different download options available, or you may be able toovisualize hello data within your web browser in Machine Learning Studio.</span></span>

## <a name="data-formats-and-data-types-supported"></a><span data-ttu-id="48a87-125">Data formátů a datové typy podporované</span><span class="sxs-lookup"><span data-stu-id="48a87-125">Data formats and data types supported</span></span>
<span data-ttu-id="48a87-126">Můžete importovat do experimentu několik typů dat, v závislosti na tom, jaký mechanismus používáte tooimport dat a odkud pocházejí z:</span><span class="sxs-lookup"><span data-stu-id="48a87-126">You can import a number of data types into your experiment, depending on what mechanism you use tooimport data and where it's coming from:</span></span>

* <span data-ttu-id="48a87-127">Prostý text (TXT)</span><span class="sxs-lookup"><span data-stu-id="48a87-127">Plain text (.txt)</span></span>
* <span data-ttu-id="48a87-128">Hodnot oddělených čárkami (CSV) s hlavičkou (CSV) nebo bez (. nh.csv)</span><span class="sxs-lookup"><span data-stu-id="48a87-128">Comma-separated values (CSV) with a header (.csv) or without (.nh.csv)</span></span>
* <span data-ttu-id="48a87-129">Karta s oddělovači (TSV) s hlavičkou (TSV) nebo bez (. nh.tsv)</span><span class="sxs-lookup"><span data-stu-id="48a87-129">Tab-separated values (TSV) with a header (.tsv) or without (.nh.tsv)</span></span>
* <span data-ttu-id="48a87-130">Soubor aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="48a87-130">Excel file</span></span>
* <span data-ttu-id="48a87-131">Tabulky Azure</span><span class="sxs-lookup"><span data-stu-id="48a87-131">Azure table</span></span>
* <span data-ttu-id="48a87-132">Tabulku Hive</span><span class="sxs-lookup"><span data-stu-id="48a87-132">Hive table</span></span>
* <span data-ttu-id="48a87-133">Tabulka databáze SQL</span><span class="sxs-lookup"><span data-stu-id="48a87-133">SQL database table</span></span>
* <span data-ttu-id="48a87-134">Hodnoty OData</span><span class="sxs-lookup"><span data-stu-id="48a87-134">OData values</span></span>
* <span data-ttu-id="48a87-135">Data SVMLight (.svmlight) (viz hello [SVMLight definice](http://svmlight.joachims.org/) formátu informace)</span><span class="sxs-lookup"><span data-stu-id="48a87-135">SVMLight data (.svmlight) (see hello [SVMLight definition](http://svmlight.joachims.org/) for format information)</span></span>
* <span data-ttu-id="48a87-136">Atribut dat vztah soubor formátu (ARFF) (.arff) (viz hello [ARFF definice](http://weka.wikispaces.com/ARFF) formátu informace)</span><span class="sxs-lookup"><span data-stu-id="48a87-136">Attribute Relation File Format (ARFF) data (.arff) (see hello [ARFF definition](http://weka.wikispaces.com/ARFF) for format information)</span></span>
* <span data-ttu-id="48a87-137">Soubor ZIP (.zip)</span><span class="sxs-lookup"><span data-stu-id="48a87-137">Zip file (.zip)</span></span>
* <span data-ttu-id="48a87-138">R objekt nebo prostoru souboru (. RData)</span><span class="sxs-lookup"><span data-stu-id="48a87-138">R object or workspace file (.RData)</span></span>

<span data-ttu-id="48a87-139">Pokud importujete data ve formátu, například ARFF, který obsahuje metadata, Machine Learning Studio používá tento nadpis hello toodefine metadata a datový typ jednotlivých sloupců.</span><span class="sxs-lookup"><span data-stu-id="48a87-139">If you import data in a format such as ARFF that includes metadata, Machine Learning Studio uses this metadata toodefine hello heading and data type of each column.</span></span>

<span data-ttu-id="48a87-140">Pokud importujete data, jako jsou TSV nebo CSV formátu, který neobsahuje tato metadata, Machine Learning Studio odvodí hello datový typ pro každý sloupec vzorkováním hello data.</span><span class="sxs-lookup"><span data-stu-id="48a87-140">If you import data such as TSV or CSV format that doesn't include this metadata, Machine Learning Studio infers hello data type for each column by sampling hello data.</span></span> <span data-ttu-id="48a87-141">Pokud hello data také nemá záhlaví sloupců, Machine Learning Studio obsahuje výchozí názvy.</span><span class="sxs-lookup"><span data-stu-id="48a87-141">If hello data also doesn't have column headings, Machine Learning Studio provides default names.</span></span>

<span data-ttu-id="48a87-142">Můžete explicitně zadat nebo změnit hello záhlaví a datové typy pro sloupce pomocí hello [upravit Metadata][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="48a87-142">You can explicitly specify or change hello headings and data types for columns using hello [Edit Metadata][edit-metadata].</span></span>

<span data-ttu-id="48a87-143">Následující Hello **datové typy** rozpoznává Machine Learning Studio:</span><span class="sxs-lookup"><span data-stu-id="48a87-143">hello following **data types** are recognized by Machine Learning Studio:</span></span>

* <span data-ttu-id="48a87-144">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48a87-144">String</span></span>
* <span data-ttu-id="48a87-145">Integer</span><span class="sxs-lookup"><span data-stu-id="48a87-145">Integer</span></span>
* <span data-ttu-id="48a87-146">Double</span><span class="sxs-lookup"><span data-stu-id="48a87-146">Double</span></span>
* <span data-ttu-id="48a87-147">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="48a87-147">Boolean</span></span>
* <span data-ttu-id="48a87-148">Data a času</span><span class="sxs-lookup"><span data-stu-id="48a87-148">DateTime</span></span>
* <span data-ttu-id="48a87-149">Časový interval</span><span class="sxs-lookup"><span data-stu-id="48a87-149">TimeSpan</span></span>

<span data-ttu-id="48a87-150">Machine Learning Studio používá typ interních datových názvem ***tabulky dat*** toopass dat mezi moduly.</span><span class="sxs-lookup"><span data-stu-id="48a87-150">Machine Learning Studio uses an internal data type called ***Data Table*** toopass data between modules.</span></span> <span data-ttu-id="48a87-151">Data můžete explicitně převést do formátu dat tabulky pomocí hello [převést tooDataset] [ convert-to-dataset] modulu.</span><span class="sxs-lookup"><span data-stu-id="48a87-151">You can explicitly convert your data into Data Table format using hello [Convert tooDataset][convert-to-dataset] module.</span></span>

<span data-ttu-id="48a87-152">Libovolný modul, který přijímá formáty než tabulky datového převede hello data tooData tabulky bez upozornění před jeho odesláním toohello další modul.</span><span class="sxs-lookup"><span data-stu-id="48a87-152">Any module that accepts formats other than Data Table will convert hello data tooData Table silently before passing it toohello next module.</span></span>

<span data-ttu-id="48a87-153">V případě potřeby můžete převést formát Data tabulky zpět do sdíleného svazku clusteru, TSV, ARFF nebo SVMLight formátu pomocí převodu z ostatních modulů.</span><span class="sxs-lookup"><span data-stu-id="48a87-153">If necessary, you can convert Data Table format back into CSV, TSV, ARFF, or SVMLight format using other conversion modules.</span></span>
<span data-ttu-id="48a87-154">Hledat v hello **převody formát dat** části palety modulů hello pro moduly, které provádějí tyto funkce.</span><span class="sxs-lookup"><span data-stu-id="48a87-154">Look in hello **Data Format Conversions** section of hello module palette for modules that perform these functions.</span></span>

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
