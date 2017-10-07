---
title: "aaaAnalyze dat pomocí Azure Machine Learning | Microsoft Docs"
description: "Pomocí Azure Machine Learning toobuild prediktivní strojového učení modelu na základě dat uložené v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 337a2cd77aaad4467683827c56e5015b262b2554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a><span data-ttu-id="ce625-103">Analýza dat pomocí Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ce625-103">Analyze data with Azure Machine Learning</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ce625-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="ce625-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="ce625-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ce625-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="ce625-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce625-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="ce625-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="ce625-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="ce625-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="ce625-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="ce625-109">Tento kurz používá Azure Machine Learning toobuild prediktivní strojového učení modelu na základě dat uložené v Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ce625-109">This tutorial uses Azure Machine Learning toobuild a predictive machine learning model based on data stored in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="ce625-110">Konkrétně to sestavení cílenou marketingovou kampaň společnosti Adventure Works, hello kolo obchod, Odhadnutím toho, jaká Pokud zákazník je pravděpodobně toobuy kolo nebo ne.</span><span class="sxs-lookup"><span data-stu-id="ce625-110">Specifically, this builds a targeted marketing campaign for Adventure Works, hello bike shop, by predicting if a customer is likely toobuy a bike or not.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="ce625-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce625-111">Prerequisites</span></span>
<span data-ttu-id="ce625-112">toostep prostřednictvím tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="ce625-112">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="ce625-113">SQL Data Warehouse s předem načtenými vzorovými daty AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="ce625-113">A SQL Data Warehouse pre-loaded with AdventureWorksDW sample data.</span></span> <span data-ttu-id="ce625-114">tooprovision, najdete v tématu [vytvořit SQL Data Warehouse] [ Create a SQL Data Warehouse] a zvolte tooload hello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="ce625-114">tooprovision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose tooload hello sample data.</span></span> <span data-ttu-id="ce625-115">Pokud už Data Warehouse máte, ale nemáte ukázková data, můžete [ukázková data načíst ručně][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="ce625-115">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-get-hello-data"></a><span data-ttu-id="ce625-116">1. Získat hello data</span><span class="sxs-lookup"><span data-stu-id="ce625-116">1. Get hello data</span></span>
<span data-ttu-id="ce625-117">Hello data jsou v hello zobrazení dbo.vTargetMail v databázi AdventureWorksDW hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-117">hello data is in hello dbo.vTargetMail view in hello AdventureWorksDW database.</span></span> <span data-ttu-id="ce625-118">tooread tato data:</span><span class="sxs-lookup"><span data-stu-id="ce625-118">tooread this data:</span></span>

1. <span data-ttu-id="ce625-119">Přihlaste se k [Azure Machine Learning Studio][Azure Machine Learning studio] a klikněte na Moje experimenty.</span><span class="sxs-lookup"><span data-stu-id="ce625-119">Sign into [Azure Machine Learning studio][Azure Machine Learning studio] and click on my experiments.</span></span>
2. <span data-ttu-id="ce625-120">Klikněte na **+NOVÝ** a vyberte **Prázdný experiment**.</span><span class="sxs-lookup"><span data-stu-id="ce625-120">Click **+NEW** and select **Blank Experiment**.</span></span>
3. <span data-ttu-id="ce625-121">Zadejte název svého experimentu: Cílený marketing.</span><span class="sxs-lookup"><span data-stu-id="ce625-121">Enter a name for your experiment: Targeted Marketing.</span></span>
4. <span data-ttu-id="ce625-122">Přetáhněte hello **čtečky** modul z podokna modulů hello hello plátno.</span><span class="sxs-lookup"><span data-stu-id="ce625-122">Drag hello **Reader** module from hello modules pane into hello canvas.</span></span>
5. <span data-ttu-id="ce625-123">Zadejte podrobnosti hello vaší databáze SQL datového skladu v podokně Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-123">Specify hello details of your SQL Data Warehouse database in hello Properties pane.</span></span>
6. <span data-ttu-id="ce625-124">Zadejte databázi hello **dotazu** tooread hello dat, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="ce625-124">Specify hello database **query** tooread hello data of interest.</span></span>

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

<span data-ttu-id="ce625-125">Spusťte hello experiment kliknutím **spustit** pod plátnem experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-125">Run hello experiment by clicking **Run** under hello experiment canvas.</span></span>
<span data-ttu-id="ce625-126">![Spusťte hello experiment][1]</span><span class="sxs-lookup"><span data-stu-id="ce625-126">![Run hello experiment][1]</span></span>

<span data-ttu-id="ce625-127">Po dokončení experimentu hello spuštěna úspěšně, klikněte na výstupní port hello v hello dolní části modulu Reader hello a vyberte možnost **vizualizovat** toosee hello naimportovalo data.</span><span class="sxs-lookup"><span data-stu-id="ce625-127">After hello experiment finishes running successfully, click hello output port at hello bottom of hello Reader module and select **Visualize** toosee hello imported data.</span></span>
<span data-ttu-id="ce625-128">![Zobrazení naimportovaných dat][3]</span><span class="sxs-lookup"><span data-stu-id="ce625-128">![View imported data][3]</span></span>

## <a name="2-clean-hello-data"></a><span data-ttu-id="ce625-129">2. Vyčištění hello dat</span><span class="sxs-lookup"><span data-stu-id="ce625-129">2. Clean hello data</span></span>
<span data-ttu-id="ce625-130">tooclean hello data, vyřadit některé sloupce, které nejsou důležité pro hello model.</span><span class="sxs-lookup"><span data-stu-id="ce625-130">tooclean hello data, drop some columns that are not relevant for hello model.</span></span> <span data-ttu-id="ce625-131">toodo toto:</span><span class="sxs-lookup"><span data-stu-id="ce625-131">toodo this:</span></span>

1. <span data-ttu-id="ce625-132">Přetáhněte hello **sloupce projektu** hello plátno modul.</span><span class="sxs-lookup"><span data-stu-id="ce625-132">Drag hello **Project Columns** module into hello canvas.</span></span>
2. <span data-ttu-id="ce625-133">Klikněte na tlačítko **spustit selektor sloupců** v hello vlastnosti podokně toospecify sloupce, které chcete toodrop.</span><span class="sxs-lookup"><span data-stu-id="ce625-133">Click **Launch column selector** in hello Properties pane toospecify which columns you wish toodrop.</span></span>
   <span data-ttu-id="ce625-134">![Project Columns][4]</span><span class="sxs-lookup"><span data-stu-id="ce625-134">![Project Columns][4]</span></span>
3. <span data-ttu-id="ce625-135">Vylučte dva sloupce: CustomerAlternateKey a GeographyKey.</span><span class="sxs-lookup"><span data-stu-id="ce625-135">Exclude two columns: CustomerAlternateKey and GeographyKey.</span></span>
   <span data-ttu-id="ce625-136">![Odebrání nepotřebných sloupců][5]</span><span class="sxs-lookup"><span data-stu-id="ce625-136">![Remove unnecessary columns][5]</span></span>

## <a name="3-build-hello-model"></a><span data-ttu-id="ce625-137">3. Vytvoření modelu hello</span><span class="sxs-lookup"><span data-stu-id="ce625-137">3. Build hello model</span></span>
<span data-ttu-id="ce625-138">Rozdělíme hello data 80: 20: 80 % tootrain model machine learning a 20 % tootest hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ce625-138">We will split hello data 80-20: 80% tootrain a machine learning model and 20% tootest hello model.</span></span> <span data-ttu-id="ce625-139">Budeme používat algoritmy hello "Two-Class" pro tento problém binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="ce625-139">We will make use of hello “Two-Class” algorithms for this binary classification problem.</span></span>

1. <span data-ttu-id="ce625-140">Přetáhněte hello **rozdělení** hello plátno modul.</span><span class="sxs-lookup"><span data-stu-id="ce625-140">Drag hello **Split** module into hello canvas.</span></span>
2. <span data-ttu-id="ce625-141">Zadejte 0,8 podíl řádků v první výstupní sadě hello dat v podokně Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-141">Enter 0.8 for Fraction of rows in hello first output dataset in hello Properties pane.</span></span>
   <span data-ttu-id="ce625-142">![Rozdělení dat na sadu učení a testovací sadu][6]</span><span class="sxs-lookup"><span data-stu-id="ce625-142">![Split data into training and test set][6]</span></span>
3. <span data-ttu-id="ce625-143">Přetáhněte hello **Two-Class Boosted Decision Tree** hello plátno modul.</span><span class="sxs-lookup"><span data-stu-id="ce625-143">Drag hello **Two-Class Boosted Decision Tree** module into hello canvas.</span></span>
4. <span data-ttu-id="ce625-144">Přetáhněte hello **Train Model** modul hello plátno a určete vstupy hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-144">Drag hello **Train Model** module into hello canvas and specify hello inputs.</span></span> <span data-ttu-id="ce625-145">Potom klikněte na **spustit selektor sloupců** v podokně Vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-145">Then, click **Launch column selector** in hello Properties pane.</span></span>
   * <span data-ttu-id="ce625-146">První vstup: Algoritmus strojového učení</span><span class="sxs-lookup"><span data-stu-id="ce625-146">First input: ML algorithm.</span></span>
   * <span data-ttu-id="ce625-147">Druhý vstup: algoritmus hello tootrain dat na.</span><span class="sxs-lookup"><span data-stu-id="ce625-147">Second input: Data tootrain hello algorithm on.</span></span>
     <span data-ttu-id="ce625-148">![Připojení modulu Train Model hello][7]</span><span class="sxs-lookup"><span data-stu-id="ce625-148">![Connect hello Train Model module][7]</span></span>
5. <span data-ttu-id="ce625-149">Vyberte hello **BikeBuyer** sloupec jako sloupec toopredict hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-149">Select hello **BikeBuyer** column as hello column toopredict.</span></span>
   <span data-ttu-id="ce625-150">![Vyberte sloupec toopredict][8]</span><span class="sxs-lookup"><span data-stu-id="ce625-150">![Select Column toopredict][8]</span></span>

## <a name="4-score-hello-model"></a><span data-ttu-id="ce625-151">4. Určení skóre modelu hello</span><span class="sxs-lookup"><span data-stu-id="ce625-151">4. Score hello model</span></span>
<span data-ttu-id="ce625-152">Teď otestujeme, jak hello model provádí testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="ce625-152">Now, we will test how hello model performs on test data.</span></span> <span data-ttu-id="ce625-153">Porovnáme námi zvolený s toosee jiný algoritmus, který provádí lépe algoritmus hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-153">We will compare hello algorithm of our choice with a different algorithm toosee which performs better.</span></span>

1. <span data-ttu-id="ce625-154">Přetáhněte **Score Model** hello plátno modul.</span><span class="sxs-lookup"><span data-stu-id="ce625-154">Drag **Score Model** module into hello canvas.</span></span>
    <span data-ttu-id="ce625-155">První vstup: Train model druhý vstup: Test data ![hello skóre][9]</span><span class="sxs-lookup"><span data-stu-id="ce625-155">First input: Trained model Second input: Test data ![Score hello model][9]</span></span>
2. <span data-ttu-id="ce625-156">Přetáhněte hello **Two-Class Bayes Point Machine** hello plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="ce625-156">Drag hello **Two-Class Bayes Point Machine** into hello experiment canvas.</span></span> <span data-ttu-id="ce625-157">Porovnáme, jak tento algoritmus provádí v porovnání toohello Two-Class Boosted Decision Tree.</span><span class="sxs-lookup"><span data-stu-id="ce625-157">We will compare how this algorithm performs in comparison toohello Two-Class Boosted Decision Tree.</span></span>
3. <span data-ttu-id="ce625-158">Kopírování a vložení hello moduly Train Model a Score Model v hello plátno.</span><span class="sxs-lookup"><span data-stu-id="ce625-158">Copy and Paste hello modules Train Model and Score Model in hello canvas.</span></span>
4. <span data-ttu-id="ce625-159">Přetáhněte hello **Evaluate Model** modulu do hello plátno toocompare hello dva algoritmů.</span><span class="sxs-lookup"><span data-stu-id="ce625-159">Drag hello **Evaluate Model** module into hello canvas toocompare hello two algorithms.</span></span>
5. <span data-ttu-id="ce625-160">**Spustit** hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="ce625-160">**Run** hello experiment.</span></span>
   <span data-ttu-id="ce625-161">![Spusťte hello experiment][10]</span><span class="sxs-lookup"><span data-stu-id="ce625-161">![Run hello experiment][10]</span></span>
6. <span data-ttu-id="ce625-162">Klikněte na výstupní port hello v hello dolní části modulu Evaluate Model hello a klikněte na tlačítko vizualizovat.</span><span class="sxs-lookup"><span data-stu-id="ce625-162">Click hello output port at hello bottom of hello Evaluate Model module and click Visualize.</span></span>
   <span data-ttu-id="ce625-163">![Vizualizace výsledků vyhodnocení][11]</span><span class="sxs-lookup"><span data-stu-id="ce625-163">![Visualize evaluation results][11]</span></span>

<span data-ttu-id="ce625-164">poskytuje metriky Hello jsou hello: křivka ROC, diagram přesnosti a křivka navýšení.</span><span class="sxs-lookup"><span data-stu-id="ce625-164">hello metrics provided are hello ROC curve, precision-recall diagram and lift curve.</span></span> <span data-ttu-id="ce625-165">Vyhledávání na tyto metriky podíváme, vidíme této hello první model má lepší výsledky než druhý hello.</span><span class="sxs-lookup"><span data-stu-id="ce625-165">Looking at these metrics, we can see that hello first model performed better than hello second one.</span></span> <span data-ttu-id="ce625-166">toolook v hello co hello předpověděl první model, klikněte na výstupní port modulu Score Model hello a klikněte na vizualizovat.</span><span class="sxs-lookup"><span data-stu-id="ce625-166">toolook at hello what hello first model predicted, click on output port of hello Score Model and click Visualize.</span></span>
<span data-ttu-id="ce625-167">![Vizualizace výsledků skóre][12]</span><span class="sxs-lookup"><span data-stu-id="ce625-167">![Visualize score results][12]</span></span>

<span data-ttu-id="ce625-168">Zobrazí se další dva sloupce přidat tooyour testovací datové sady.</span><span class="sxs-lookup"><span data-stu-id="ce625-168">You will see two more columns added tooyour test dataset.</span></span>

* <span data-ttu-id="ce625-169">Scored Pravděpodobnostech: hello pravděpodobnost, že si zákazník koupí kolo.</span><span class="sxs-lookup"><span data-stu-id="ce625-169">Scored Probabilities: hello likelihood that a customer is a bike buyer.</span></span>
* <span data-ttu-id="ce625-170">Scored popisky: hello klasifikace prováděná modelem hello – kolo (1) nebo ne (0).</span><span class="sxs-lookup"><span data-stu-id="ce625-170">Scored Labels: hello classification done by hello model – bike buyer (1) or not (0).</span></span> <span data-ttu-id="ce625-171">Tato prahová hodnota pravděpodobnosti pro popisky je nastavena too50 % a lze upravit.</span><span class="sxs-lookup"><span data-stu-id="ce625-171">This probability threshold for labeling is set too50% and can be adjusted.</span></span>

<span data-ttu-id="ce625-172">Porovnání hello sloupce BikeBuyer (skutečnost) s hello popisky vyhodnocení (předpověď), můžete zobrazit, jak dobře hello model provedl.</span><span class="sxs-lookup"><span data-stu-id="ce625-172">Comparing hello column BikeBuyer (actual) with hello Scored Labels (prediction), you can see how well hello model has performed.</span></span> <span data-ttu-id="ce625-173">V dalších krocích můžete použít tento model toomake předpovědi nových zákazníků a publikovat tento model jako webovou službu nebo můžete zapsat výsledky zpět tooSQL datového skladu.</span><span class="sxs-lookup"><span data-stu-id="ce625-173">As next steps, you can use this model toomake predictions for new customers and publish this model as a web service or write results back tooSQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce625-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce625-174">Next steps</span></span>
<span data-ttu-id="ce625-175">toolearn Další informace o vytváření prediktivních modelů strojového učení, najdete v příliš[tooMachine Úvod učení na platformě Azure][Introduction tooMachine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="ce625-175">toolearn more about building predictive machine learning models, refer too[Introduction tooMachine Learning on Azure][Introduction tooMachine Learning on Azure].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction tooMachine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
