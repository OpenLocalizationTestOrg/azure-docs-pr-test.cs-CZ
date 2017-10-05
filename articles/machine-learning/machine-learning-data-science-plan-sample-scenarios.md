---
title: "Určit pokročilou analýzu scénáře pro Azure Machine Learning | Microsoft Docs"
description: "Vyberte odpovídající scénáře pro to advanced prediktivní analýzy v procesu Team Data vědecké účely."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fe4f74f2e0602d13eedb6ca186480291a9a5724f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a><span data-ttu-id="58bdf-103">Scénáře pro pokročilé analýzy ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="58bdf-103">Scenarios for advanced analytics in Azure Machine Learning</span></span>
<span data-ttu-id="58bdf-104">Tento článek popisuje řadu ukázkových zdrojů dat a cílové scénáře, které mohou být zpracována [tým datové vědy procesu (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="58bdf-104">This article outlines the variety of sample data sources and target scenarios that can be handled by the [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span> <span data-ttu-id="58bdf-105">TDSP poskytuje systematicky pro týmy spolupracovat na vytváření inteligentní aplikace.</span><span class="sxs-lookup"><span data-stu-id="58bdf-105">The TDSP provides a systematic approach for teams to collaborate on building intelligent applications.</span></span> <span data-ttu-id="58bdf-106">Scénáře, které jsou uvedeny v tomto tématu ilustrují možnosti dostupné v pracovním postupu zpracování dat, které jsou závislé na vlastností dat, umístění zdrojového a cílového úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-106">The scenarios presented here illustrate options available in the data processing workflow that depend on the data characteristics, source locations, and target repositories in Azure.</span></span>

<span data-ttu-id="58bdf-107">**Rozhodovací strom** pro výběr ukázkových scénářů, které jsou vhodné pro vaše data a cíl se zobrazí v poslední části.</span><span class="sxs-lookup"><span data-stu-id="58bdf-107">The **decision tree** for selecting the sample scenarios that is appropriate for your data and objective is presented in the last section.</span></span>

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

<span data-ttu-id="58bdf-108">Každý z těchto částí uvede vzorový scénář.</span><span class="sxs-lookup"><span data-stu-id="58bdf-108">Each of the following sections presents a sample scenario.</span></span> <span data-ttu-id="58bdf-109">Pro každý scénář, vědecké zpracování možných dat nebo pokročilou analýzu jsou uvedeny toku a podpůrné prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-109">For each scenario, a possible data science or advanced analytics flow and supporting Azure resources are listed.</span></span>

> [!NOTE]
> <span data-ttu-id="58bdf-110">**Pro všechny z následujících scénářů budete muset:**
> </span><span class="sxs-lookup"><span data-stu-id="58bdf-110">**For all of the following scenarios, you need to:**
</span></span><br/>
> 
> * <span data-ttu-id="58bdf-111">[Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md)
>   </span><span class="sxs-lookup"><span data-stu-id="58bdf-111">[Create a storage account](../storage/common/storage-create-storage-account.md)
</span></span><br/>
> * [<span data-ttu-id="58bdf-112">Vytvořit pracovní prostor služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="58bdf-112">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
> 
> 

## <span data-ttu-id="58bdf-113"><a name="smalllocal"></a>Scénář \#1: malé a střední tabulkové datovou sadu v místních souborů</span><span class="sxs-lookup"><span data-stu-id="58bdf-113"><a name="smalllocal"></a>Scenario \#1: Small to medium tabular dataset in a local files</span></span>
![Malé a střední místních souborů][1]

#### <a name="additional-azure-resources-none"></a><span data-ttu-id="58bdf-115">Další prostředky Azure: žádné</span><span class="sxs-lookup"><span data-stu-id="58bdf-115">Additional Azure resources: None</span></span>
1. <span data-ttu-id="58bdf-116">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-116">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
2. <span data-ttu-id="58bdf-117">Nahrání datové sady.</span><span class="sxs-lookup"><span data-stu-id="58bdf-117">Upload a dataset.</span></span>
3. <span data-ttu-id="58bdf-118">Vytvoření experimentu tok Azure Machine Learning, počínaje nahrané datových sad.</span><span class="sxs-lookup"><span data-stu-id="58bdf-118">Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).</span></span>

## <span data-ttu-id="58bdf-119"><a name="smalllocalprocess"></a>Scénář \#2: malé a střední datové sady místních souborů, které vyžadují zpracování</span><span class="sxs-lookup"><span data-stu-id="58bdf-119"><a name="smalllocalprocess"></a>Scenario \#2: Small to medium dataset of local files that require processing</span></span>
![Malé a střední místních souborů s zpracování][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="58bdf-121">Další prostředky Azure: virtuální počítač Azure (server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-121">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="58bdf-122">Vytvořte virtuální počítač Azure s IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-122">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="58bdf-123">Nahrání dat do kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-123">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="58bdf-124">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z kontejneru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-124">Pre-process and clean data in IPython Notebook, accessing data from Azure storage container.</span></span>
4. <span data-ttu-id="58bdf-125">Transformace dat do domény, vyčistit formě tabulky.</span><span class="sxs-lookup"><span data-stu-id="58bdf-125">Transform data to cleaned, tabular form.</span></span>
5. <span data-ttu-id="58bdf-126">Uložte Transformovaná data do objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-126">Save transformed data in Azure blobs.</span></span>
6. <span data-ttu-id="58bdf-127">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-127">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
7. <span data-ttu-id="58bdf-128">Čtení dat z Azure BLOB pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-128">Read the data from Azure blobs using the [Import Data][import-data] module.</span></span>
8. <span data-ttu-id="58bdf-129">Vytvoření experimentu tok Azure Machine Learning, počínaje ingestovaný datových sad.</span><span class="sxs-lookup"><span data-stu-id="58bdf-129">Build an Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="58bdf-130"><a name="largelocal"></a>Scénář \#3: velké datové sady místních souborů, cílení objektů BLOB služby Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-130"><a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs</span></span>
![Velké místních souborů][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="58bdf-132">Další prostředky Azure: virtuální počítač Azure (server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-132">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="58bdf-133">Vytvořte virtuální počítač Azure s IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-133">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="58bdf-134">Nahrání dat do kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-134">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="58bdf-135">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="58bdf-135">Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.</span></span>
4. <span data-ttu-id="58bdf-136">Transformace dat do domény, vyčistit formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-136">Transform data to cleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="58bdf-137">Prozkoumejte data a podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="58bdf-137">Explore data, and create features as needed.</span></span>
6. <span data-ttu-id="58bdf-138">Extrahujte vzorku dat. malé a střední.</span><span class="sxs-lookup"><span data-stu-id="58bdf-138">Extract a small-to-medium data sample.</span></span>
7. <span data-ttu-id="58bdf-139">Uložte jen Vzorkovaná data do objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-139">Save the sampled data in Azure blobs.</span></span>
8. <span data-ttu-id="58bdf-140">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-140">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="58bdf-141">Čtení dat z Azure BLOB pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-141">Read the data from Azure blobs using the [Import Data][import-data] module.</span></span>
10. <span data-ttu-id="58bdf-142">Sestavení Azure Machine Learning experimentu toku počínaje ingestovaný datových sad.</span><span class="sxs-lookup"><span data-stu-id="58bdf-142">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="58bdf-143"><a name="smalllocaltodb"></a>Scénář \#4: malé a střední datové sady místních souborů, cílení na SQL Server virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-143"><a name="smalllocaltodb"></a>Scenario \#4: Small to medium dataset of local files, targeting SQL Server in an Azure Virtual Machine</span></span>
![Malé a střední místních souborů k databázi SQL v Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="58bdf-145">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-145">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="58bdf-146">Vytvořte virtuální počítač Azure systémem SQL Server + IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-146">Create an Azure Virtual Machine running SQL Server + IPython Notebook.</span></span>
2. <span data-ttu-id="58bdf-147">Nahrání dat do kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-147">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="58bdf-148">Předběžně zpracovat a vyčistit data v kontejneru úložiště Azure pomocí IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-148">Pre-process and clean data in Azure storage container using IPython Notebook.</span></span>
4. <span data-ttu-id="58bdf-149">Transformace dat do domény, vyčistit formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-149">Transform data to cleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="58bdf-150">Uložení dat do souborů virtuálního počítače – místní (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky odkazovat na disky virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="58bdf-150">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
6. <span data-ttu-id="58bdf-151">Načíst data do databáze systému SQL Server spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-151">Load data to SQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="58bdf-152">Možnost \#1: pomocí nástroje SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="58bdf-152">Option \#1: Using SQL Server Management Studio.</span></span>
   
   * <span data-ttu-id="58bdf-153">Přihlášení k systému SQL Server virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="58bdf-153">Login to SQL Server VM</span></span>
   * <span data-ttu-id="58bdf-154">Spusťte SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="58bdf-154">Run SQL Server Management Studio.</span></span>
   * <span data-ttu-id="58bdf-155">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="58bdf-155">Create database and target tables.</span></span>
   * <span data-ttu-id="58bdf-156">Používají jeden z hromadného importu metody k načítání dat z virtuálního počítače – místní soubory.</span><span class="sxs-lookup"><span data-stu-id="58bdf-156">Use one of the bulk import methods to load the data from VM-local files.</span></span>
   
   <span data-ttu-id="58bdf-157">Možnost \#2: pomocí IPython poznámkového bloku – není vhodné pro střední a větší datové sady</span><span class="sxs-lookup"><span data-stu-id="58bdf-157">Option \#2: Using IPython Notebook – not advisable for medium and larger datasets</span></span>
   
   <!-- -->    
   * <span data-ttu-id="58bdf-158">Použijte připojovací řetězec ODBC pro přístup k systému SQL Server na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="58bdf-158">Use ODBC connection string to access SQL Server on VM.</span></span>
   * <span data-ttu-id="58bdf-159">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="58bdf-159">Create database and target tables.</span></span>
   * <span data-ttu-id="58bdf-160">Používají jeden z hromadného importu metody k načítání dat z virtuálního počítače – místní soubory.</span><span class="sxs-lookup"><span data-stu-id="58bdf-160">Use one of the bulk import methods to load the data from VM-local files.</span></span>
7. <span data-ttu-id="58bdf-161">Prozkoumejte data, podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="58bdf-161">Explore data, create features as needed.</span></span> <span data-ttu-id="58bdf-162">Všimněte si, že funkce nemusí být materializována v tabulkách databáze.</span><span class="sxs-lookup"><span data-stu-id="58bdf-162">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="58bdf-163">Mějte na paměti pouze nezbytné dotaz k jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="58bdf-163">Only note the necessary query to create them.</span></span>
8. <span data-ttu-id="58bdf-164">Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-164">Decide on a data sample size, if needed and/or desired.</span></span>
9. <span data-ttu-id="58bdf-165">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-165">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
10. <span data-ttu-id="58bdf-166">Číst data přímo ze systému SQL Server pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-166">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="58bdf-167">Vložte nezbytné dotaz, který extrahuje pole, vytvoří funkce a ukázky data v případě potřeby přímo v [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-167">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
11. <span data-ttu-id="58bdf-168">Sestavení Azure Machine Learning experimentu toku počínaje ingestovaný datových sad.</span><span class="sxs-lookup"><span data-stu-id="58bdf-168">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="58bdf-169"><a name="largelocaltodb"></a>Scénář \#5: velké datové sady v místních souborů cíle systému SQL Server ve virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-169"><a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM</span></span>
![Velké místních souborů k databázi SQL v Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="58bdf-171">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-171">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="58bdf-172">Vytvořte virtuální počítač Azure systémem SQL Server a server IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-172">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="58bdf-173">Nahrání dat do kontejner úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-173">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="58bdf-174">(Volitelné) Předběžně zpracovat a vyčistit data.</span><span class="sxs-lookup"><span data-stu-id="58bdf-174">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="58bdf-175">a.</span><span class="sxs-lookup"><span data-stu-id="58bdf-175">a.</span></span>  <span data-ttu-id="58bdf-176">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-176">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="58bdf-177">b.</span><span class="sxs-lookup"><span data-stu-id="58bdf-177">b.</span></span>  <span data-ttu-id="58bdf-178">Transformace dat do domény, vyčistit formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-178">Transform data to cleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="58bdf-179">c.</span><span class="sxs-lookup"><span data-stu-id="58bdf-179">c.</span></span>  <span data-ttu-id="58bdf-180">Uložení dat do souborů virtuálního počítače – místní (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky odkazovat na disky virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="58bdf-180">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
4. <span data-ttu-id="58bdf-181">Načíst data do databáze systému SQL Server spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-181">Load data to SQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="58bdf-182">a.</span><span class="sxs-lookup"><span data-stu-id="58bdf-182">a.</span></span>  <span data-ttu-id="58bdf-183">Přihlášení k systému SQL Server virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="58bdf-183">Login to SQL Server VM.</span></span>
   
   <span data-ttu-id="58bdf-184">b.</span><span class="sxs-lookup"><span data-stu-id="58bdf-184">b.</span></span>  <span data-ttu-id="58bdf-185">Pokud data nebyla uložena již, stahování datových souborů z Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-185">If data not saved already, download data files from Azure</span></span>
   
       storage container to local-VM folder.
   
   <span data-ttu-id="58bdf-186">c.</span><span class="sxs-lookup"><span data-stu-id="58bdf-186">c.</span></span>  <span data-ttu-id="58bdf-187">Spusťte SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="58bdf-187">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="58bdf-188">d.</span><span class="sxs-lookup"><span data-stu-id="58bdf-188">d.</span></span>  <span data-ttu-id="58bdf-189">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="58bdf-189">Create database and target tables.</span></span>
   
   <span data-ttu-id="58bdf-190">e.</span><span class="sxs-lookup"><span data-stu-id="58bdf-190">e.</span></span>  <span data-ttu-id="58bdf-191">Některý z hromadného importu metody chcete načíst data.</span><span class="sxs-lookup"><span data-stu-id="58bdf-191">Use one of the bulk import methods to load the data.</span></span>
   
   <span data-ttu-id="58bdf-192">f.</span><span class="sxs-lookup"><span data-stu-id="58bdf-192">f.</span></span>  <span data-ttu-id="58bdf-193">Pokud je potřeba spoje tabulky platná, vytvořte indexy urychlit spojení.</span><span class="sxs-lookup"><span data-stu-id="58bdf-193">If table joins are required, create indexes to expedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="58bdf-194">Rychlejší načítání velikostí velkých objemů dat, se doporučuje, můžete vytvořit dělené tabulky a hromadně importovat data paralelně.</span><span class="sxs-lookup"><span data-stu-id="58bdf-194">For faster loading of large data sizes, it is recommended that you create partitioned tables and bulk import the data in parallel.</span></span> <span data-ttu-id="58bdf-195">Další informace najdete v tématu [paralelní Import dat do tabulek SQL rozdělena na oddíly](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="58bdf-195">For more information, see [Parallel Data Import to SQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="58bdf-196">Prozkoumejte data, podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="58bdf-196">Explore data, create features as needed.</span></span> <span data-ttu-id="58bdf-197">Všimněte si, že funkce nemusí být materializována v tabulkách databáze.</span><span class="sxs-lookup"><span data-stu-id="58bdf-197">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="58bdf-198">Mějte na paměti pouze nezbytné dotaz k jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="58bdf-198">Only note the necessary query to create them.</span></span>
6. <span data-ttu-id="58bdf-199">Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-199">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="58bdf-200">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-200">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="58bdf-201">Číst data přímo ze systému SQL Server pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-201">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="58bdf-202">Vložte nezbytné dotaz, který extrahuje pole, vytvoří funkce a ukázky data v případě potřeby přímo v [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-202">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="58bdf-203">Jednoduchý experiment toku Azure Machine Learning počínaje nahrané datové sady</span><span class="sxs-lookup"><span data-stu-id="58bdf-203">Simple Azure Machine Learning experiment flow starting with uploaded dataset</span></span>

## <span data-ttu-id="58bdf-204"><a name="largedbtodb"></a>Scénář \#6: velké datové sady v SQL Server databáze místní, cílení na SQL Server virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-204"><a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine</span></span>
![Velké SQL DB místní k databázi SQL v Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="58bdf-206">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-206">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="58bdf-207">Vytvořte virtuální počítač Azure systémem SQL Server a server IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-207">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="58bdf-208">Použijte jeden z data exportovat metody pro export dat z SQL serveru do souborů výpisu paměti.</span><span class="sxs-lookup"><span data-stu-id="58bdf-208">Use one of the data export methods to export the data from SQL Server to dump files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="58bdf-209">Pokud se rozhodnete přesouvání všech dat z databáze místní, alternativní metodu (rychlejší) přesunout databázi úplné instanci systému SQL Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-209">If you decide to move all data from the on-prem database, an alternate (faster) method to move the full database to the SQL Server instance in Azure.</span></span> <span data-ttu-id="58bdf-210">Přeskočte kroky pro export dat, vytvoření databáze a zatížení nebo importovat data na cílovou databázi a postupujte podle alternativní metodu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-210">Skip the steps to export data, create database, and load/import data to the target database and follow the alternate method.</span></span>
   > 
   > 
3. <span data-ttu-id="58bdf-211">Nahrání souborů výpisu paměti do kontejneru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-211">Upload dump files to Azure storage container.</span></span>
4. <span data-ttu-id="58bdf-212">Načtěte data do databáze systému SQL Server spuštěna na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-212">Load the data to a SQL Server database running on an Azure Virtual Machine.</span></span>
   
   <span data-ttu-id="58bdf-213">a.</span><span class="sxs-lookup"><span data-stu-id="58bdf-213">a.</span></span>  <span data-ttu-id="58bdf-214">Přihlášení k systému SQL Server virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="58bdf-214">Login to the SQL Server VM.</span></span>
   
   <span data-ttu-id="58bdf-215">b.</span><span class="sxs-lookup"><span data-stu-id="58bdf-215">b.</span></span>  <span data-ttu-id="58bdf-216">Stahování datových souborů z kontejner úložiště Azure do složky, místní počítač.</span><span class="sxs-lookup"><span data-stu-id="58bdf-216">Download data files from an Azure storage container to the local-VM folder.</span></span>
   
   <span data-ttu-id="58bdf-217">c.</span><span class="sxs-lookup"><span data-stu-id="58bdf-217">c.</span></span>  <span data-ttu-id="58bdf-218">Spusťte SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="58bdf-218">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="58bdf-219">d.</span><span class="sxs-lookup"><span data-stu-id="58bdf-219">d.</span></span>  <span data-ttu-id="58bdf-220">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="58bdf-220">Create database and target tables.</span></span>
   
   <span data-ttu-id="58bdf-221">e.</span><span class="sxs-lookup"><span data-stu-id="58bdf-221">e.</span></span>  <span data-ttu-id="58bdf-222">Některý z hromadného importu metody chcete načíst data.</span><span class="sxs-lookup"><span data-stu-id="58bdf-222">Use one of the bulk import methods to load the data.</span></span>
   
   <span data-ttu-id="58bdf-223">f.</span><span class="sxs-lookup"><span data-stu-id="58bdf-223">f.</span></span>  <span data-ttu-id="58bdf-224">Pokud je potřeba spoje tabulky platná, vytvořte indexy urychlit spojení.</span><span class="sxs-lookup"><span data-stu-id="58bdf-224">If table joins are required, create indexes to expedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="58bdf-225">Pro rychlejší načítání velikostí velkých objemů dat, vytváření oddílů tabulek a k hromadnému importu dat paralelně.</span><span class="sxs-lookup"><span data-stu-id="58bdf-225">For faster loading of large data sizes, create partitioned tables and to bulk import the data in parallel.</span></span> <span data-ttu-id="58bdf-226">Další informace najdete v tématu [paralelní Import dat do tabulek SQL rozdělena na oddíly](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="58bdf-226">For more information, see [Parallel Data Import to SQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="58bdf-227">Prozkoumejte data, podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="58bdf-227">Explore data, create features as needed.</span></span> <span data-ttu-id="58bdf-228">Všimněte si, že funkce nemusí být materializována v tabulkách databáze.</span><span class="sxs-lookup"><span data-stu-id="58bdf-228">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="58bdf-229">Mějte na paměti pouze nezbytné dotaz k jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="58bdf-229">Only note the necessary query to create them.</span></span>
6. <span data-ttu-id="58bdf-230">Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-230">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="58bdf-231">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-231">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="58bdf-232">Číst data přímo ze systému SQL Server pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-232">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="58bdf-233">Vložte nezbytné dotaz, který extrahuje pole, vytvoří funkce a ukázky data v případě potřeby přímo v [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-233">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="58bdf-234">Jednoduché Azure Machine Learning experimentu toku počínaje nahrané datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-234">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a><span data-ttu-id="58bdf-235">Alternativní metoda pro kopírování úplné databáze z místního serveru SQL do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="58bdf-235">Alternate method to copy a full database from an on-premises  SQL Server to Azure SQL Database</span></span>
![Místní databáze odpojit a připojit k databázi SQL v Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="58bdf-237">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-237">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
<span data-ttu-id="58bdf-238">Replikovat celou databázi SQL serveru ve vaší virtuální počítač SQL Server, měli byste zkopírovat databázi z jednoho umístění nebo serveru na jiný, za předpokladu, že databáze můžete provedeny dočasně offline.</span><span class="sxs-lookup"><span data-stu-id="58bdf-238">To replicate the entire SQL Server database in your SQL Server VM, you should copy a database from one location/server to another, assuming that the database can be taken temporarily offline.</span></span> <span data-ttu-id="58bdf-239">Můžete to udělat v Průzkumník objektů systému SQL Server Management Studio nebo pomocí ekvivalentní příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="58bdf-239">You do this in the SQL Server Management Studio Object Explorer, or using the equivalent Transact-SQL commands.</span></span>

1. <span data-ttu-id="58bdf-240">Odpojení databáze v umístění zdroje.</span><span class="sxs-lookup"><span data-stu-id="58bdf-240">Detach the database at the source location.</span></span> <span data-ttu-id="58bdf-241">Další informace najdete v tématu [odpojení databáze](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="58bdf-241">For more information, see [Detach a database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span></span>
2. <span data-ttu-id="58bdf-242">V okně Průzkumníka Windows nebo příkazového řádku systému Windows zkopírujte soubor odpojit databáze nebo soubory a soubor protokolu nebo soubory do cílového umístění na virtuální počítač SQL Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="58bdf-242">In Windows Explorer or Windows Command Prompt window, copy the detached database file or files and log file or files to the target location on the SQL Server VM in Azure.</span></span>
3. <span data-ttu-id="58bdf-243">Připojte zkopírované soubory v cílové instanci systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="58bdf-243">Attach the copied files to the target SQL Server instance.</span></span> <span data-ttu-id="58bdf-244">Další informace najdete v tématu [připojit databázi](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="58bdf-244">For more information, see [Attach a Database](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span></span>

<span data-ttu-id="58bdf-245">[Přesuňte databázi pomocí odpojit a připojit (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span><span class="sxs-lookup"><span data-stu-id="58bdf-245">[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span></span>

## <span data-ttu-id="58bdf-246"><a name="largedbtohive"></a>Scénář \#7: velkých objemů dat v místních souborech cílové databáze Hive v Azure HDInsight Hadoop clusterů</span><span class="sxs-lookup"><span data-stu-id="58bdf-246"><a name="largedbtohive"></a>Scenario \#7: Big data in local files, target Hive database in Azure HDInsight Hadoop clusters</span></span>
![Velké objemy dat v místní cíl Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="58bdf-248">Další prostředky Azure: clusteru Azure HDInsight Hadoop a virtuální počítač Azure (server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="58bdf-248">Additional Azure resources: Azure HDInsight Hadoop Cluster and Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="58bdf-249">Vytvořte virtuální počítač Azure serverem IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="58bdf-249">Create an Azure Virtual Machine running IPython Notebook server.</span></span>
2. <span data-ttu-id="58bdf-250">Vytvoření clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="58bdf-250">Create an Azure HDInsight Hadoop cluster.</span></span>
3. <span data-ttu-id="58bdf-251">(Volitelné) Předběžně zpracovat a vyčistit data.</span><span class="sxs-lookup"><span data-stu-id="58bdf-251">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="58bdf-252">a.</span><span class="sxs-lookup"><span data-stu-id="58bdf-252">a.</span></span>  <span data-ttu-id="58bdf-253">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure</span><span class="sxs-lookup"><span data-stu-id="58bdf-253">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="58bdf-254">b.</span><span class="sxs-lookup"><span data-stu-id="58bdf-254">b.</span></span>  <span data-ttu-id="58bdf-255">Transformace dat do domény, vyčistit formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-255">Transform data to cleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="58bdf-256">c.</span><span class="sxs-lookup"><span data-stu-id="58bdf-256">c.</span></span>  <span data-ttu-id="58bdf-257">Uložení dat do souborů virtuálního počítače – místní (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky odkazovat na disky virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="58bdf-257">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
4. <span data-ttu-id="58bdf-258">Nahrání dat do výchozí kontejner clusteru Hadoop vybrali v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="58bdf-258">Upload data to the default container of the Hadoop cluster selected in the step 2.</span></span>
5. <span data-ttu-id="58bdf-259">Načíst data do databáze Hive v clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="58bdf-259">Load data to Hive database in Azure HDInsight Hadoop cluster.</span></span>
   
   <span data-ttu-id="58bdf-260">a.</span><span class="sxs-lookup"><span data-stu-id="58bdf-260">a.</span></span>  <span data-ttu-id="58bdf-261">Přihlaste se k hlavnímu uzlu clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="58bdf-261">Log in to the head node of the Hadoop cluster</span></span>
   
   <span data-ttu-id="58bdf-262">b.</span><span class="sxs-lookup"><span data-stu-id="58bdf-262">b.</span></span>  <span data-ttu-id="58bdf-263">Otevřete příkazový řádek Hadoop.</span><span class="sxs-lookup"><span data-stu-id="58bdf-263">Open the Hadoop Command Line.</span></span>
   
   <span data-ttu-id="58bdf-264">c.</span><span class="sxs-lookup"><span data-stu-id="58bdf-264">c.</span></span>  <span data-ttu-id="58bdf-265">Zadejte kořenový adresář Hive pomocí příkazu `cd %hive_home%\bin` v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="58bdf-265">Enter the Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="58bdf-266">d.</span><span class="sxs-lookup"><span data-stu-id="58bdf-266">d.</span></span>  <span data-ttu-id="58bdf-267">Spouštění dotazů Hive k vytvoření databáze a tabulky a načtení dat z úložiště objektů blob do tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="58bdf-267">Run the Hive queries to create database and tables, and load data from blob storage to Hive tables.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="58bdf-268">Pokud je velkých objemů dat, uživatelé mohou vytvářet tabulku Hive s oddíly.</span><span class="sxs-lookup"><span data-stu-id="58bdf-268">If the data is big, users can create the Hive table with partitions.</span></span> <span data-ttu-id="58bdf-269">Uživatelé pak mohou používat `for` smyčky v Hadoop příkazový řádek z hlavního uzlu pro načtení dat do tabulky Hive rozdělena na oddíly oddílem.</span><span class="sxs-lookup"><span data-stu-id="58bdf-269">Then, users can use a `for` loop in the Hadoop Command Line on the head node to load data into the Hive table partitioned by partition.</span></span>
   > 
   > 
6. <span data-ttu-id="58bdf-270">Prozkoumejte data a vytvářet funkcí podle potřeby v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="58bdf-270">Explore data and create features as needed in Hadoop Command Line.</span></span> <span data-ttu-id="58bdf-271">Všimněte si, že funkce nemusí být materializována v tabulkách databáze.</span><span class="sxs-lookup"><span data-stu-id="58bdf-271">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="58bdf-272">Mějte na paměti pouze nezbytné dotaz k jejich vytvoření.</span><span class="sxs-lookup"><span data-stu-id="58bdf-272">Only note the necessary query to create them.</span></span>
   
   <span data-ttu-id="58bdf-273">a.</span><span class="sxs-lookup"><span data-stu-id="58bdf-273">a.</span></span>  <span data-ttu-id="58bdf-274">Přihlaste se k hlavnímu uzlu clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="58bdf-274">Log in to the head node of the Hadoop cluster</span></span>
   
   <span data-ttu-id="58bdf-275">b.</span><span class="sxs-lookup"><span data-stu-id="58bdf-275">b.</span></span>  <span data-ttu-id="58bdf-276">Otevřete příkazový řádek Hadoop.</span><span class="sxs-lookup"><span data-stu-id="58bdf-276">Open the Hadoop Command Line.</span></span>
   
   <span data-ttu-id="58bdf-277">c.</span><span class="sxs-lookup"><span data-stu-id="58bdf-277">c.</span></span>  <span data-ttu-id="58bdf-278">Zadejte kořenový adresář Hive pomocí příkazu `cd %hive_home%\bin` v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="58bdf-278">Enter the Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="58bdf-279">d.</span><span class="sxs-lookup"><span data-stu-id="58bdf-279">d.</span></span>  <span data-ttu-id="58bdf-280">Spouštění dotazů Hive v Hadoop příkazový řádek z hlavního uzlu clusteru Hadoop data prozkoumat a vytvořit funkcí podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-280">Run the Hive queries in Hadoop Command Line on the head node of the Hadoop cluster to explore the data and create features as needed.</span></span>
7. <span data-ttu-id="58bdf-281">Pokud potřebné a/nebo potřeby, ukázková data, aby se vešla do Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="58bdf-281">If needed and/or desired, sample the data to fit in Azure Machine Learning Studio.</span></span>
8. <span data-ttu-id="58bdf-282">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="58bdf-282">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="58bdf-283">Číst data přímo z `Hive Queries` pomocí [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-283">Read the data directly from the `Hive Queries` using the [Import Data][import-data] module.</span></span> <span data-ttu-id="58bdf-284">Vložte nezbytné dotaz, který extrahuje pole, vytvoří funkce a ukázky data v případě potřeby přímo v [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-284">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
10. <span data-ttu-id="58bdf-285">Jednoduché Azure Machine Learning experimentu toku počínaje nahrané datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="58bdf-285">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

## <span data-ttu-id="58bdf-286"><a name="decisiontree"></a>Rozhodovací strom pro výběr scénář</span><span class="sxs-lookup"><span data-stu-id="58bdf-286"><a name="decisiontree"></a>Decision tree for scenario selection</span></span>
- - -
<span data-ttu-id="58bdf-287">Následující diagram shrnuje výše popsané scénáře a pokročilé analýzy proces a výběr technologie, která vás zavedou na každý z rozepsané scénářů.</span><span class="sxs-lookup"><span data-stu-id="58bdf-287">The following diagram summarizes the scenarios described above and the Advanced Analytics Process and Technology choices made that take you to each of the itemized scenarios.</span></span> <span data-ttu-id="58bdf-288">Všimněte si, že může trvat zpracování dat, průzkum, funkce inženýrství a vzorkování umístit do jednoho nebo více metoda prostředí – ve zdroji zprostředkující, nebo cílové prostředí – a může pokračovat interaktivně, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="58bdf-288">Note that data processing, exploration, feature engineering, and sampling may take place in one or more method/environment -- at the source, intermediate, and/or target environments – and may proceed iteratively as needed.</span></span> <span data-ttu-id="58bdf-289">Diagram pouze slouží jako ilustraci některé možné toků a neposkytuje vyčerpávající výčet.</span><span class="sxs-lookup"><span data-stu-id="58bdf-289">The diagram only serves as an illustration of some of possible flows and does not provide an exhaustive enumeration.</span></span>

![Vzorové DS proces návod scénáře][8]

### <a name="advanced-analytics-in-action-examples"></a><span data-ttu-id="58bdf-291">Pokročilé analýzy v akci příklady</span><span class="sxs-lookup"><span data-stu-id="58bdf-291">Advanced Analytics in action Examples</span></span>
<span data-ttu-id="58bdf-292">Návody začátku do konce Azure Machine Learning, která využívají pokročilé analýzy proces a technologie pomocí veřejné datové sady najdete v části:</span><span class="sxs-lookup"><span data-stu-id="58bdf-292">For end-to-end Azure Machine Learning walkthroughs that employ the Advanced Analytics Process and Technology using public datasets, see:</span></span>

* <span data-ttu-id="58bdf-293">[Tým proces vědecké účely dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="58bdf-293">[Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>
* <span data-ttu-id="58bdf-294">[Tým proces vědecké účely dat v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="58bdf-294">[Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
