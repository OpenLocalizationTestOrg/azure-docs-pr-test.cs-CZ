---
title: "aaaIdentify pokročilé analýzy scénáře pro Azure Machine Learning | Microsoft Docs"
description: "Vyberte odpovídající scénáře hello to advanced prediktivní analýzy s hello proces vědecké účely dat Team."
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
ms.openlocfilehash: 52c6bb10d6df4f640a4f66cf17cf4993cc1067b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a><span data-ttu-id="6be9c-103">Scénáře pro pokročilé analýzy ve službě Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6be9c-103">Scenarios for advanced analytics in Azure Machine Learning</span></span>
<span data-ttu-id="6be9c-104">Tento článek popisuje různé hello ukázkových zdrojů dat a cílové scénáře, které mohou být zpracována hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6be9c-104">This article outlines hello variety of sample data sources and target scenarios that can be handled by hello [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span> <span data-ttu-id="6be9c-105">Hello TDSP poskytuje systematicky pro týmy toocollaborate na budování inteligentní aplikace.</span><span class="sxs-lookup"><span data-stu-id="6be9c-105">hello TDSP provides a systematic approach for teams toocollaborate on building intelligent applications.</span></span> <span data-ttu-id="6be9c-106">scénáře Hello uvedeny v tomto tématu ilustrují možnosti dostupné v pracovním postupu hello zpracování dat, které jsou závislé na vlastností hello dat, umístění zdrojových souborů a cílové úložiště v Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-106">hello scenarios presented here illustrate options available in hello data processing workflow that depend on hello data characteristics, source locations, and target repositories in Azure.</span></span>

<span data-ttu-id="6be9c-107">Hello **rozhodovací strom** pro výběr hello ukázkových scénářů, které jsou vhodné pro vaše data a cíl se zobrazí v poslední části hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-107">hello **decision tree** for selecting hello sample scenarios that is appropriate for your data and objective is presented in hello last section.</span></span>

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

<span data-ttu-id="6be9c-108">Každý z následujících částí hello uvede vzorový scénář.</span><span class="sxs-lookup"><span data-stu-id="6be9c-108">Each of hello following sections presents a sample scenario.</span></span> <span data-ttu-id="6be9c-109">Pro každý scénář, vědecké zpracování možných dat nebo pokročilou analýzu jsou uvedeny toku a podpůrné prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-109">For each scenario, a possible data science or advanced analytics flow and supporting Azure resources are listed.</span></span>

> [!NOTE]
> <span data-ttu-id="6be9c-110">**Pro všechny hello následující scénáře budete muset:**
> </span><span class="sxs-lookup"><span data-stu-id="6be9c-110">**For all of hello following scenarios, you need to:**
</span></span><br/>
> 
> * <span data-ttu-id="6be9c-111">[Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md)
>   </span><span class="sxs-lookup"><span data-stu-id="6be9c-111">[Create a storage account](../storage/common/storage-create-storage-account.md)
</span></span><br/>
> * [<span data-ttu-id="6be9c-112">Vytvořit pracovní prostor služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6be9c-112">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
> 
> 

## <span data-ttu-id="6be9c-113"><a name="smalllocal"></a>Scénář \#1: malý toomedium tabulkové datovou sadu v místních souborů</span><span class="sxs-lookup"><span data-stu-id="6be9c-113"><a name="smalllocal"></a>Scenario \#1: Small toomedium tabular dataset in a local files</span></span>
![Malé toomedium místních souborů][1]

#### <a name="additional-azure-resources-none"></a><span data-ttu-id="6be9c-115">Další prostředky Azure: žádné</span><span class="sxs-lookup"><span data-stu-id="6be9c-115">Additional Azure resources: None</span></span>
1. <span data-ttu-id="6be9c-116">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-116">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
2. <span data-ttu-id="6be9c-117">Nahrání datové sady.</span><span class="sxs-lookup"><span data-stu-id="6be9c-117">Upload a dataset.</span></span>
3. <span data-ttu-id="6be9c-118">Vytvoření experimentu tok Azure Machine Learning, počínaje nahrané datových sad.</span><span class="sxs-lookup"><span data-stu-id="6be9c-118">Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).</span></span>

## <span data-ttu-id="6be9c-119"><a name="smalllocalprocess"></a>Scénář \#2: malé toomedium datové sady místních souborů, které vyžadují zpracování</span><span class="sxs-lookup"><span data-stu-id="6be9c-119"><a name="smalllocalprocess"></a>Scenario \#2: Small toomedium dataset of local files that require processing</span></span>
![Malé toomedium místních souborů s zpracování][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="6be9c-121">Další prostředky Azure: virtuální počítač Azure (server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-121">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="6be9c-122">Vytvořte virtuální počítač Azure s IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-122">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="6be9c-123">Nahrajte kontejner dat tooan úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-123">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="6be9c-124">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z kontejneru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-124">Pre-process and clean data in IPython Notebook, accessing data from Azure storage container.</span></span>
4. <span data-ttu-id="6be9c-125">Transformace dat toocleaned, formě tabulky.</span><span class="sxs-lookup"><span data-stu-id="6be9c-125">Transform data toocleaned, tabular form.</span></span>
5. <span data-ttu-id="6be9c-126">Uložte Transformovaná data do objektů BLOB Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-126">Save transformed data in Azure blobs.</span></span>
6. <span data-ttu-id="6be9c-127">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-127">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
7. <span data-ttu-id="6be9c-128">Čtení dat hello z Azure BLOB pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-128">Read hello data from Azure blobs using hello [Import Data][import-data] module.</span></span>
8. <span data-ttu-id="6be9c-129">Vytvoření experimentu tok Azure Machine Learning, počínaje ingestovaný datových sad.</span><span class="sxs-lookup"><span data-stu-id="6be9c-129">Build an Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="6be9c-130"><a name="largelocal"></a>Scénář \#3: velké datové sady místních souborů, cílení objektů BLOB služby Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-130"><a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs</span></span>
![Velké místních souborů][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="6be9c-132">Další prostředky Azure: virtuální počítač Azure (server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-132">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="6be9c-133">Vytvořte virtuální počítač Azure s IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-133">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="6be9c-134">Nahrajte kontejner dat tooan úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-134">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="6be9c-135">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="6be9c-135">Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.</span></span>
4. <span data-ttu-id="6be9c-136">Transformace dat toocleaned, formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-136">Transform data toocleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="6be9c-137">Prozkoumejte data a podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="6be9c-137">Explore data, and create features as needed.</span></span>
6. <span data-ttu-id="6be9c-138">Extrahujte vzorku dat. malé a střední.</span><span class="sxs-lookup"><span data-stu-id="6be9c-138">Extract a small-to-medium data sample.</span></span>
7. <span data-ttu-id="6be9c-139">Uložení hello vzorků dat v Azure BLOB.</span><span class="sxs-lookup"><span data-stu-id="6be9c-139">Save hello sampled data in Azure blobs.</span></span>
8. <span data-ttu-id="6be9c-140">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-140">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="6be9c-141">Čtení dat hello z Azure BLOB pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-141">Read hello data from Azure blobs using hello [Import Data][import-data] module.</span></span>
10. <span data-ttu-id="6be9c-142">Sestavení Azure Machine Learning experimentu toku počínaje ingestovaný datových sad.</span><span class="sxs-lookup"><span data-stu-id="6be9c-142">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="6be9c-143"><a name="smalllocaltodb"></a>Scénář \#4: malá toomedium datové sady místních souborů, cílení na SQL Server virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-143"><a name="smalllocaltodb"></a>Scenario \#4: Small toomedium dataset of local files, targeting SQL Server in an Azure Virtual Machine</span></span>
![Malé toomedium místních souborů tooSQL DB v Azure][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="6be9c-145">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-145">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="6be9c-146">Vytvořte virtuální počítač Azure systémem SQL Server + IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-146">Create an Azure Virtual Machine running SQL Server + IPython Notebook.</span></span>
2. <span data-ttu-id="6be9c-147">Nahrajte kontejner dat tooan úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-147">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="6be9c-148">Předběžně zpracovat a vyčistit data v kontejneru úložiště Azure pomocí IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-148">Pre-process and clean data in Azure storage container using IPython Notebook.</span></span>
4. <span data-ttu-id="6be9c-149">Transformace dat toocleaned, formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-149">Transform data toocleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="6be9c-150">Uložte soubory tooVM místní data (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky získáte tooVM jednotek).</span><span class="sxs-lookup"><span data-stu-id="6be9c-150">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
6. <span data-ttu-id="6be9c-151">Načtěte data tooSQL serveru databázi spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-151">Load data tooSQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="6be9c-152">Možnost \#1: pomocí nástroje SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="6be9c-152">Option \#1: Using SQL Server Management Studio.</span></span>
   
   * <span data-ttu-id="6be9c-153">Přihlášení tooSQL virtuální počítač Server</span><span class="sxs-lookup"><span data-stu-id="6be9c-153">Login tooSQL Server VM</span></span>
   * <span data-ttu-id="6be9c-154">Spusťte SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="6be9c-154">Run SQL Server Management Studio.</span></span>
   * <span data-ttu-id="6be9c-155">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="6be9c-155">Create database and target tables.</span></span>
   * <span data-ttu-id="6be9c-156">Použijte jeden z hello hromadné importovat metody tooload hello data z virtuálního počítače – místní soubory.</span><span class="sxs-lookup"><span data-stu-id="6be9c-156">Use one of hello bulk import methods tooload hello data from VM-local files.</span></span>
   
   <span data-ttu-id="6be9c-157">Možnost \#2: pomocí IPython poznámkového bloku – není vhodné pro střední a větší datové sady</span><span class="sxs-lookup"><span data-stu-id="6be9c-157">Option \#2: Using IPython Notebook – not advisable for medium and larger datasets</span></span>
   
   <!-- -->    
   * <span data-ttu-id="6be9c-158">Pomocí rozhraní ODBC připojovací řetězec tooaccess systému SQL Server na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="6be9c-158">Use ODBC connection string tooaccess SQL Server on VM.</span></span>
   * <span data-ttu-id="6be9c-159">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="6be9c-159">Create database and target tables.</span></span>
   * <span data-ttu-id="6be9c-160">Použijte jeden z hello hromadné importovat metody tooload hello data z virtuálního počítače – místní soubory.</span><span class="sxs-lookup"><span data-stu-id="6be9c-160">Use one of hello bulk import methods tooload hello data from VM-local files.</span></span>
7. <span data-ttu-id="6be9c-161">Prozkoumejte data, podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="6be9c-161">Explore data, create features as needed.</span></span> <span data-ttu-id="6be9c-162">Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-162">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="6be9c-163">Pouze Všimněte si toocreate nezbytné dotazu hello je.</span><span class="sxs-lookup"><span data-stu-id="6be9c-163">Only note hello necessary query toocreate them.</span></span>
8. <span data-ttu-id="6be9c-164">Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-164">Decide on a data sample size, if needed and/or desired.</span></span>
9. <span data-ttu-id="6be9c-165">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-165">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
10. <span data-ttu-id="6be9c-166">Čtení hello data přímo z hello systému SQL Server pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-166">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="6be9c-167">Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-167">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
11. <span data-ttu-id="6be9c-168">Sestavení Azure Machine Learning experimentu toku počínaje ingestovaný datových sad.</span><span class="sxs-lookup"><span data-stu-id="6be9c-168">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="6be9c-169"><a name="largelocaltodb"></a>Scénář \#5: velké datové sady v místních souborů cíle systému SQL Server ve virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-169"><a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM</span></span>
![TooSQL velké místních souborů databáze v Azure][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="6be9c-171">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-171">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="6be9c-172">Vytvořte virtuální počítač Azure systémem SQL Server a server IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-172">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="6be9c-173">Nahrajte kontejner dat tooan úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-173">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="6be9c-174">(Volitelné) Předběžně zpracovat a vyčistit data.</span><span class="sxs-lookup"><span data-stu-id="6be9c-174">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="6be9c-175">a.</span><span class="sxs-lookup"><span data-stu-id="6be9c-175">a.</span></span>  <span data-ttu-id="6be9c-176">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-176">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="6be9c-177">b.</span><span class="sxs-lookup"><span data-stu-id="6be9c-177">b.</span></span>  <span data-ttu-id="6be9c-178">Transformace dat toocleaned, formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-178">Transform data toocleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="6be9c-179">c.</span><span class="sxs-lookup"><span data-stu-id="6be9c-179">c.</span></span>  <span data-ttu-id="6be9c-180">Uložte soubory tooVM místní data (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky získáte tooVM jednotek).</span><span class="sxs-lookup"><span data-stu-id="6be9c-180">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
4. <span data-ttu-id="6be9c-181">Načtěte data tooSQL serveru databázi spuštěna na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-181">Load data tooSQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="6be9c-182">a.</span><span class="sxs-lookup"><span data-stu-id="6be9c-182">a.</span></span>  <span data-ttu-id="6be9c-183">Přihlášení tooSQL virtuální počítač Server.</span><span class="sxs-lookup"><span data-stu-id="6be9c-183">Login tooSQL Server VM.</span></span>
   
   <span data-ttu-id="6be9c-184">b.</span><span class="sxs-lookup"><span data-stu-id="6be9c-184">b.</span></span>  <span data-ttu-id="6be9c-185">Pokud data nebyla uložena již, stahování datových souborů z Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-185">If data not saved already, download data files from Azure</span></span>
   
       storage container toolocal-VM folder.
   
   <span data-ttu-id="6be9c-186">c.</span><span class="sxs-lookup"><span data-stu-id="6be9c-186">c.</span></span>  <span data-ttu-id="6be9c-187">Spusťte SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="6be9c-187">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="6be9c-188">d.</span><span class="sxs-lookup"><span data-stu-id="6be9c-188">d.</span></span>  <span data-ttu-id="6be9c-189">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="6be9c-189">Create database and target tables.</span></span>
   
   <span data-ttu-id="6be9c-190">e.</span><span class="sxs-lookup"><span data-stu-id="6be9c-190">e.</span></span>  <span data-ttu-id="6be9c-191">Použijte jeden z hello hromadně importovat data hello tooload metody.</span><span class="sxs-lookup"><span data-stu-id="6be9c-191">Use one of hello bulk import methods tooload hello data.</span></span>
   
   <span data-ttu-id="6be9c-192">f.</span><span class="sxs-lookup"><span data-stu-id="6be9c-192">f.</span></span>  <span data-ttu-id="6be9c-193">Pokud spoje tabulky platná, vytvořte spojení tooexpedite indexy.</span><span class="sxs-lookup"><span data-stu-id="6be9c-193">If table joins are required, create indexes tooexpedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6be9c-194">Rychlejší načítání velikostí velkých objemů dat, se doporučuje vytvořit dělené tabulky a hromadně importovat data hello paralelně.</span><span class="sxs-lookup"><span data-stu-id="6be9c-194">For faster loading of large data sizes, it is recommended that you create partitioned tables and bulk import hello data in parallel.</span></span> <span data-ttu-id="6be9c-195">Další informace najdete v tématu [Parallel Data importovat tooSQL rozdělena na oddíly tabulky](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="6be9c-195">For more information, see [Parallel Data Import tooSQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="6be9c-196">Prozkoumejte data, podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="6be9c-196">Explore data, create features as needed.</span></span> <span data-ttu-id="6be9c-197">Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-197">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="6be9c-198">Pouze Všimněte si toocreate nezbytné dotazu hello je.</span><span class="sxs-lookup"><span data-stu-id="6be9c-198">Only note hello necessary query toocreate them.</span></span>
6. <span data-ttu-id="6be9c-199">Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-199">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="6be9c-200">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-200">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="6be9c-201">Čtení hello data přímo z hello systému SQL Server pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-201">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="6be9c-202">Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-202">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="6be9c-203">Jednoduchý experiment toku Azure Machine Learning počínaje nahrané datové sady</span><span class="sxs-lookup"><span data-stu-id="6be9c-203">Simple Azure Machine Learning experiment flow starting with uploaded dataset</span></span>

## <span data-ttu-id="6be9c-204"><a name="largedbtodb"></a>Scénář \#6: velké datové sady v SQL Server databáze místní, cílení na SQL Server virtuálním počítači Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-204"><a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine</span></span>
![TooSQL velké místní databáze SQL DB v Azure][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="6be9c-206">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-206">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="6be9c-207">Vytvořte virtuální počítač Azure systémem SQL Server a server IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-207">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="6be9c-208">Použijte jeden z datových hello exportovat metody tooexport hello data z toodump soubory systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6be9c-208">Use one of hello data export methods tooexport hello data from SQL Server toodump files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6be9c-209">Pokud se rozhodnete toomove všechna data z hello místní databázi, alternativní (rychlejší) metoda toomove hello úplné databáze toothe instanci systému SQL Server v Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-209">If you decide toomove all data from hello on-prem database, an alternate (faster) method toomove hello full database toothe SQL Server instance in Azure.</span></span> <span data-ttu-id="6be9c-210">Přeskočit hello kroky tooexport dat, vytvoření databáze a zatížení nebo importovat data toohello cílovou databázi a postupujte podle alternativní metodu hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-210">Skip hello steps tooexport data, create database, and load/import data toohello target database and follow hello alternate method.</span></span>
   > 
   > 
3. <span data-ttu-id="6be9c-211">Nahrajte kontejner úložiště tooAzure odkládacích souborů.</span><span class="sxs-lookup"><span data-stu-id="6be9c-211">Upload dump files tooAzure storage container.</span></span>
4. <span data-ttu-id="6be9c-212">Načíst hello data tooa databáze systému SQL Server spuštěna na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-212">Load hello data tooa SQL Server database running on an Azure Virtual Machine.</span></span>
   
   <span data-ttu-id="6be9c-213">a.</span><span class="sxs-lookup"><span data-stu-id="6be9c-213">a.</span></span>  <span data-ttu-id="6be9c-214">Přihlášení toohello virtuální počítač SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6be9c-214">Login toohello SQL Server VM.</span></span>
   
   <span data-ttu-id="6be9c-215">b.</span><span class="sxs-lookup"><span data-stu-id="6be9c-215">b.</span></span>  <span data-ttu-id="6be9c-216">Stahování datových souborů ze složky místní VM toohello kontejneru úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-216">Download data files from an Azure storage container toohello local-VM folder.</span></span>
   
   <span data-ttu-id="6be9c-217">c.</span><span class="sxs-lookup"><span data-stu-id="6be9c-217">c.</span></span>  <span data-ttu-id="6be9c-218">Spusťte SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="6be9c-218">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="6be9c-219">d.</span><span class="sxs-lookup"><span data-stu-id="6be9c-219">d.</span></span>  <span data-ttu-id="6be9c-220">Vytváření tabulek databáze a cíle.</span><span class="sxs-lookup"><span data-stu-id="6be9c-220">Create database and target tables.</span></span>
   
   <span data-ttu-id="6be9c-221">e.</span><span class="sxs-lookup"><span data-stu-id="6be9c-221">e.</span></span>  <span data-ttu-id="6be9c-222">Použijte jeden z hello hromadně importovat data hello tooload metody.</span><span class="sxs-lookup"><span data-stu-id="6be9c-222">Use one of hello bulk import methods tooload hello data.</span></span>
   
   <span data-ttu-id="6be9c-223">f.</span><span class="sxs-lookup"><span data-stu-id="6be9c-223">f.</span></span>  <span data-ttu-id="6be9c-224">Pokud spoje tabulky platná, vytvořte spojení tooexpedite indexy.</span><span class="sxs-lookup"><span data-stu-id="6be9c-224">If table joins are required, create indexes tooexpedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6be9c-225">Pro rychlejší načítání velikostí velkých objemů dat, vytvoření oddílů tabulky a toobulk importovat hello data paralelně.</span><span class="sxs-lookup"><span data-stu-id="6be9c-225">For faster loading of large data sizes, create partitioned tables and toobulk import hello data in parallel.</span></span> <span data-ttu-id="6be9c-226">Další informace najdete v tématu [Parallel Data importovat tooSQL rozdělena na oddíly tabulky](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="6be9c-226">For more information, see [Parallel Data Import tooSQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="6be9c-227">Prozkoumejte data, podle potřeby vytvářet funkce.</span><span class="sxs-lookup"><span data-stu-id="6be9c-227">Explore data, create features as needed.</span></span> <span data-ttu-id="6be9c-228">Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-228">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="6be9c-229">Pouze Všimněte si toocreate nezbytné dotazu hello je.</span><span class="sxs-lookup"><span data-stu-id="6be9c-229">Only note hello necessary query toocreate them.</span></span>
6. <span data-ttu-id="6be9c-230">Vyberte velikost vzorku dat, pokud potřebné a/nebo potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-230">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="6be9c-231">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-231">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="6be9c-232">Čtení hello data přímo z hello systému SQL Server pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-232">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="6be9c-233">Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-233">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="6be9c-234">Jednoduché Azure Machine Learning experimentu toku počínaje nahrané datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-234">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

### <a name="alternate-method-toocopy-a-full-database-from-an-on-premises--sql-server-tooazure-sql-database"></a><span data-ttu-id="6be9c-235">Alternativní metoda toocopy úplné databáze z SQL serveru tooAzure místní databáze SQL</span><span class="sxs-lookup"><span data-stu-id="6be9c-235">Alternate method toocopy a full database from an on-premises  SQL Server tooAzure SQL Database</span></span>
![Místní databáze odpojit a připojit tooSQL DB v Azure][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="6be9c-237">Další prostředky Azure: virtuální počítač Azure (SQL Server nebo server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-237">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
<span data-ttu-id="6be9c-238">tooreplicate hello celou databázi SQL serveru ve vaší virtuální počítač SQL Server, měli byste zkopírovat databázi z jednoho serveru nebo umístění tooanother, za předpokladu, že hello databázi můžete provedeny dočasně offline.</span><span class="sxs-lookup"><span data-stu-id="6be9c-238">tooreplicate hello entire SQL Server database in your SQL Server VM, you should copy a database from one location/server tooanother, assuming that hello database can be taken temporarily offline.</span></span> <span data-ttu-id="6be9c-239">Můžete to udělat v hello Průzkumník objektů systému SQL Server Management Studio nebo pomocí hello ekvivalentní příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="6be9c-239">You do this in hello SQL Server Management Studio Object Explorer, or using hello equivalent Transact-SQL commands.</span></span>

1. <span data-ttu-id="6be9c-240">Odpojení databáze hello v umístění zdroje hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-240">Detach hello database at hello source location.</span></span> <span data-ttu-id="6be9c-241">Další informace najdete v tématu [odpojení databáze](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="6be9c-241">For more information, see [Detach a database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span></span>
2. <span data-ttu-id="6be9c-242">V okně Průzkumníka Windows nebo příkazového řádku Windows hello kopie odpojit soubor databáze nebo soubory a soubor protokolu nebo soubory toohello cílové umístění na hello virtuální počítač s SQL serverem v Azure.</span><span class="sxs-lookup"><span data-stu-id="6be9c-242">In Windows Explorer or Windows Command Prompt window, copy hello detached database file or files and log file or files toohello target location on hello SQL Server VM in Azure.</span></span>
3. <span data-ttu-id="6be9c-243">Připojte instanci systému SQL Server hello zkopírovat soubory toohello cíl.</span><span class="sxs-lookup"><span data-stu-id="6be9c-243">Attach hello copied files toohello target SQL Server instance.</span></span> <span data-ttu-id="6be9c-244">Další informace najdete v tématu [připojit databázi](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="6be9c-244">For more information, see [Attach a Database](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span></span>

<span data-ttu-id="6be9c-245">[Přesuňte databázi pomocí odpojit a připojit (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span><span class="sxs-lookup"><span data-stu-id="6be9c-245">[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span></span>

## <span data-ttu-id="6be9c-246"><a name="largedbtohive"></a>Scénář \#7: velkých objemů dat v místních souborech cílové databáze Hive v Azure HDInsight Hadoop clusterů</span><span class="sxs-lookup"><span data-stu-id="6be9c-246"><a name="largedbtohive"></a>Scenario \#7: Big data in local files, target Hive database in Azure HDInsight Hadoop clusters</span></span>
![Velké objemy dat v místní cíl Hive][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="6be9c-248">Další prostředky Azure: clusteru Azure HDInsight Hadoop a virtuální počítač Azure (server IPython Poznámkový blok)</span><span class="sxs-lookup"><span data-stu-id="6be9c-248">Additional Azure resources: Azure HDInsight Hadoop Cluster and Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="6be9c-249">Vytvořte virtuální počítač Azure serverem IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="6be9c-249">Create an Azure Virtual Machine running IPython Notebook server.</span></span>
2. <span data-ttu-id="6be9c-250">Vytvoření clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="6be9c-250">Create an Azure HDInsight Hadoop cluster.</span></span>
3. <span data-ttu-id="6be9c-251">(Volitelné) Předběžně zpracovat a vyčistit data.</span><span class="sxs-lookup"><span data-stu-id="6be9c-251">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="6be9c-252">a.</span><span class="sxs-lookup"><span data-stu-id="6be9c-252">a.</span></span>  <span data-ttu-id="6be9c-253">Předběžně zpracovat a vyčistit data v poznámkovém bloku IPython, přístup k datům z Azure</span><span class="sxs-lookup"><span data-stu-id="6be9c-253">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="6be9c-254">b.</span><span class="sxs-lookup"><span data-stu-id="6be9c-254">b.</span></span>  <span data-ttu-id="6be9c-255">Transformace dat toocleaned, formě tabulky, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-255">Transform data toocleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="6be9c-256">c.</span><span class="sxs-lookup"><span data-stu-id="6be9c-256">c.</span></span>  <span data-ttu-id="6be9c-257">Uložte soubory tooVM místní data (IPython poznámkového bloku běží na virtuálním počítači, místní jednotky získáte tooVM jednotek).</span><span class="sxs-lookup"><span data-stu-id="6be9c-257">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
4. <span data-ttu-id="6be9c-258">Nahrání dat toohello výchozí kontejner clusteru Hadoop hello vybrali v kroku hello 2.</span><span class="sxs-lookup"><span data-stu-id="6be9c-258">Upload data toohello default container of hello Hadoop cluster selected in hello step 2.</span></span>
5. <span data-ttu-id="6be9c-259">Načíst data tooHive databázi v clusteru Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="6be9c-259">Load data tooHive database in Azure HDInsight Hadoop cluster.</span></span>
   
   <span data-ttu-id="6be9c-260">a.</span><span class="sxs-lookup"><span data-stu-id="6be9c-260">a.</span></span>  <span data-ttu-id="6be9c-261">Přihlaste se toohello hlavního uzlu v clusteru Hadoop hello</span><span class="sxs-lookup"><span data-stu-id="6be9c-261">Log in toohello head node of hello Hadoop cluster</span></span>
   
   <span data-ttu-id="6be9c-262">b.</span><span class="sxs-lookup"><span data-stu-id="6be9c-262">b.</span></span>  <span data-ttu-id="6be9c-263">Otevřete hello Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6be9c-263">Open hello Hadoop Command Line.</span></span>
   
   <span data-ttu-id="6be9c-264">c.</span><span class="sxs-lookup"><span data-stu-id="6be9c-264">c.</span></span>  <span data-ttu-id="6be9c-265">Zadejte kořenový adresář hello Hive pomocí příkazu `cd %hive_home%\bin` v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6be9c-265">Enter hello Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="6be9c-266">d.</span><span class="sxs-lookup"><span data-stu-id="6be9c-266">d.</span></span>  <span data-ttu-id="6be9c-267">Spusťte hello Hive dotazy toocreate databáze a tabulky a načtení dat z tabulky tooHive úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="6be9c-267">Run hello Hive queries toocreate database and tables, and load data from blob storage tooHive tables.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6be9c-268">Pokud je velkých objemů dat hello, uživatelé mohou vytvářet hello Hive tabulky s oddíly.</span><span class="sxs-lookup"><span data-stu-id="6be9c-268">If hello data is big, users can create hello Hive table with partitions.</span></span> <span data-ttu-id="6be9c-269">Uživatelé pak mohou používat `for` smyčky v hello Hadoop příkazového řádku na hello hlavního uzlu tooload data do tabulky Hive hello rozdělena na oddíly oddílem.</span><span class="sxs-lookup"><span data-stu-id="6be9c-269">Then, users can use a `for` loop in hello Hadoop Command Line on hello head node tooload data into hello Hive table partitioned by partition.</span></span>
   > 
   > 
6. <span data-ttu-id="6be9c-270">Prozkoumejte data a vytvářet funkcí podle potřeby v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6be9c-270">Explore data and create features as needed in Hadoop Command Line.</span></span> <span data-ttu-id="6be9c-271">Všimněte si, že funkce hello nemusí toobe materializována v tabulkách databáze hello.</span><span class="sxs-lookup"><span data-stu-id="6be9c-271">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="6be9c-272">Pouze Všimněte si toocreate nezbytné dotazu hello je.</span><span class="sxs-lookup"><span data-stu-id="6be9c-272">Only note hello necessary query toocreate them.</span></span>
   
   <span data-ttu-id="6be9c-273">a.</span><span class="sxs-lookup"><span data-stu-id="6be9c-273">a.</span></span>  <span data-ttu-id="6be9c-274">Přihlaste se toohello hlavního uzlu v clusteru Hadoop hello</span><span class="sxs-lookup"><span data-stu-id="6be9c-274">Log in toohello head node of hello Hadoop cluster</span></span>
   
   <span data-ttu-id="6be9c-275">b.</span><span class="sxs-lookup"><span data-stu-id="6be9c-275">b.</span></span>  <span data-ttu-id="6be9c-276">Otevřete hello Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6be9c-276">Open hello Hadoop Command Line.</span></span>
   
   <span data-ttu-id="6be9c-277">c.</span><span class="sxs-lookup"><span data-stu-id="6be9c-277">c.</span></span>  <span data-ttu-id="6be9c-278">Zadejte kořenový adresář hello Hive pomocí příkazu `cd %hive_home%\bin` v Hadoop příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="6be9c-278">Enter hello Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="6be9c-279">d.</span><span class="sxs-lookup"><span data-stu-id="6be9c-279">d.</span></span>  <span data-ttu-id="6be9c-280">Spouštění dotazů Hive hello v Hadoop příkazového řádku hello hlavního uzlu hello Hadoop clusteru tooexplore hello dat a vytvořte funkcí podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-280">Run hello Hive queries in Hadoop Command Line on hello head node of hello Hadoop cluster tooexplore hello data and create features as needed.</span></span>
7. <span data-ttu-id="6be9c-281">Pokud potřebné a/nebo potřeby, ukázkové hello toofit dat v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="6be9c-281">If needed and/or desired, sample hello data toofit in Azure Machine Learning Studio.</span></span>
8. <span data-ttu-id="6be9c-282">Přihlaste se toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="6be9c-282">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="6be9c-283">Čtení dat hello přímo z hello `Hive Queries` pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-283">Read hello data directly from hello `Hive Queries` using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="6be9c-284">Hello vložení nezbytné se dotaz, který extrahuje pole, vytvoří funkce a v případě potřeby přímo v hello – ukázky data [importovat Data] [ import-data] dotazu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-284">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
10. <span data-ttu-id="6be9c-285">Jednoduché Azure Machine Learning experimentu toku počínaje nahrané datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="6be9c-285">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

## <span data-ttu-id="6be9c-286"><a name="decisiontree"></a>Rozhodovací strom pro výběr scénář</span><span class="sxs-lookup"><span data-stu-id="6be9c-286"><a name="decisiontree"></a>Decision tree for scenario selection</span></span>
- - -
<span data-ttu-id="6be9c-287">Hello následující diagram shrnuje hello scénáře popsané výše a hello pokročilé analýzy proces a technologie možnosti zvolené vyžadující tooeach hello uvedeno scénáře.</span><span class="sxs-lookup"><span data-stu-id="6be9c-287">hello following diagram summarizes hello scenarios described above and hello Advanced Analytics Process and Technology choices made that take you tooeach of hello itemized scenarios.</span></span> <span data-ttu-id="6be9c-288">Všimněte si, že může trvat zpracování dat, průzkum, funkce inženýrství a vzorkování umístit do jednoho nebo více metoda prostředí – u hello zdroje, zprostředkující, nebo cílové prostředí – a může pokračovat interaktivně, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="6be9c-288">Note that data processing, exploration, feature engineering, and sampling may take place in one or more method/environment -- at hello source, intermediate, and/or target environments – and may proceed iteratively as needed.</span></span> <span data-ttu-id="6be9c-289">Hello diagram pouze slouží jako ilustraci některé možné toků a neposkytuje vyčerpávající výčet.</span><span class="sxs-lookup"><span data-stu-id="6be9c-289">hello diagram only serves as an illustration of some of possible flows and does not provide an exhaustive enumeration.</span></span>

![Vzorové DS proces návod scénáře][8]

### <a name="advanced-analytics-in-action-examples"></a><span data-ttu-id="6be9c-291">Pokročilé analýzy v akci příklady</span><span class="sxs-lookup"><span data-stu-id="6be9c-291">Advanced Analytics in action Examples</span></span>
<span data-ttu-id="6be9c-292">Pro návody začátku do konce Azure Machine Learning, která využívají hello pokročilé analýzy proces a technologie pomocí veřejné datové sady najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="6be9c-292">For end-to-end Azure Machine Learning walkthroughs that employ hello Advanced Analytics Process and Technology using public datasets, see:</span></span>

* <span data-ttu-id="6be9c-293">[Tým proces vědecké účely dat v akci: pomocí SQL serveru](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="6be9c-293">[Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>
* <span data-ttu-id="6be9c-294">[Tým proces vědecké účely dat v akci: pomocí clusterů systému HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="6be9c-294">[Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md).</span></span>

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
