---
title: aaaUse Azure Machine Learning s SQL Data Warehouse | Microsoft Docs
description: "Kurz pro používání Azure Machine Learning s Azure SQL Data Warehousem pro vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: ac6bc731-6add-47a9-b3fe-68996e656f4d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: fdfe8c936d2bb7a02163a0bbf6435e1ebd518d4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a><span data-ttu-id="645c2-103">SQL Data Warehouse pomocí Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="645c2-103">Use Azure Machine Learning with SQL Data Warehouse</span></span>
<span data-ttu-id="645c2-104">Azure Machine Learning je plně spravovaná prediktivní analýzy služby, můžete použít toocreate prediktivní modely pro vaše data v SQL Data Warehouse a pak publikovat jako připravené využívají webové služby.</span><span class="sxs-lookup"><span data-stu-id="645c2-104">Azure Machine Learning is a fully managed predictive analytics service that you can use toocreate predictive models against your data in SQL Data Warehouse, and then publish as ready-to-consume web services.</span></span> <span data-ttu-id="645c2-105">Můžete další hello základy prediktivní analýzy a strojového učení načtením [tooMachine Úvod učení na platformě Azure][Introduction tooMachine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="645c2-105">You can learn hello basics of predictive analytics and machine learning by reading [Introduction tooMachine Learning on Azure][Introduction tooMachine Learning on Azure].</span></span>  <span data-ttu-id="645c2-106">Potom dozvíte, jak toocreate, trénování, stanovení skóre a otestování modelu strojového učení pomocí hello [vytvořit experimentu kurzu][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="645c2-106">You can then learn how toocreate, train, score and test a machine learning model using hello [Create experiment tutorial][Create experiment tutorial].</span></span>

<span data-ttu-id="645c2-107">V tomto článku se dozvíte, jak toodo hello následující pomocí hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span><span class="sxs-lookup"><span data-stu-id="645c2-107">In this article, you will learn how toodo hello following using hello [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span></span>

* <span data-ttu-id="645c2-108">Čtení dat z vaší databáze toocreate, školení a stanovení skóre prediktivního modelu</span><span class="sxs-lookup"><span data-stu-id="645c2-108">Read data from your database toocreate, train and score a predictive model</span></span>
* <span data-ttu-id="645c2-109">Zápis dat tooyour databáze</span><span class="sxs-lookup"><span data-stu-id="645c2-109">Write data tooyour database</span></span>

## <a name="read-data-from-sql-data-warehouse"></a><span data-ttu-id="645c2-110">Čtení dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="645c2-110">Read data from SQL Data Warehouse</span></span>
<span data-ttu-id="645c2-111">Jsme z produktu tabulky v databázi AdventureWorksDW hello číst data.</span><span class="sxs-lookup"><span data-stu-id="645c2-111">We will read data from Product table in hello AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="645c2-112">Krok 1</span><span class="sxs-lookup"><span data-stu-id="645c2-112">Step 1</span></span>
<span data-ttu-id="645c2-113">Začněte nový experiment kliknutím na + nové dole hello hello okno Machine Learning Studio, vyberte EXPERIMENT a pak vyberte prázdný Experiment.</span><span class="sxs-lookup"><span data-stu-id="645c2-113">Start a new experiment by clicking +NEW at hello bottom of hello Machine Learning Studio window, select EXPERIMENT, and then select Blank Experiment.</span></span> <span data-ttu-id="645c2-114">Vyberte hello výchozí experimentovat název hello horní části plátna hello a přejmenujte ji toosomething smysluplného, například předpověď ceny jízdních kol.</span><span class="sxs-lookup"><span data-stu-id="645c2-114">Select hello default experiment name at hello top of hello canvas and rename it toosomething meaningful, for example, Bicycle price prediction.</span></span>

### <a name="step-2"></a><span data-ttu-id="645c2-115">Krok 2</span><span class="sxs-lookup"><span data-stu-id="645c2-115">Step 2</span></span>
<span data-ttu-id="645c2-116">Vyhledejte modul čtečky hello v hello paleta datových sad a modulů na hello nalevo od plátna experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-116">Look for hello Reader module in hello palette of datasets and modules on hello left of hello experiment canvas.</span></span> <span data-ttu-id="645c2-117">Přetáhněte plátno experimentu toohello modulu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-117">Drag hello module toohello experiment canvas.</span></span>
![][drag_reader]

### <a name="step-3"></a><span data-ttu-id="645c2-118">Krok 3</span><span class="sxs-lookup"><span data-stu-id="645c2-118">Step 3</span></span>
<span data-ttu-id="645c2-119">Vyberte modul čtečky hello a vyplňte hello podokně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="645c2-119">Select hello Reader module and fill out hello properties pane.</span></span>

1. <span data-ttu-id="645c2-120">Vyberte Azure SQL Database jako zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-120">Select Azure SQL Database as hello Data Source.</span></span>
2. <span data-ttu-id="645c2-121">Název databázového serveru: název serveru hello typu.</span><span class="sxs-lookup"><span data-stu-id="645c2-121">Database server name: Type hello server name.</span></span> <span data-ttu-id="645c2-122">Můžete použít hello [portál Azure] [ Azure portal] toofind to.</span><span class="sxs-lookup"><span data-stu-id="645c2-122">You can use hello [Azure portal][Azure portal] toofind this.</span></span>

![][server_name]

1. <span data-ttu-id="645c2-123">Název databáze: název typu hello databáze na serveru hello jste právě určili.</span><span class="sxs-lookup"><span data-stu-id="645c2-123">Database name: Type hello name of a database on hello server you just specified.</span></span>
2. <span data-ttu-id="645c2-124">Název uživatelského účtu serveru: typ hello uživatelské jméno účtu, který má přístupová oprávnění pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-124">Server user account name:  Type hello user name of an account that has access permissions for hello database.</span></span>
3. <span data-ttu-id="645c2-125">Heslo uživatelského účtu serveru: Zadejte heslo hello hello zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="645c2-125">Server user account password: Provide hello password for hello specified user account.</span></span>
4. <span data-ttu-id="645c2-126">Přijměte všechny certifikát serveru: pomocí této možnosti (méně bezpečné), pokud chcete, aby tooskip kontrola certifikát webu hello před číst vaše data.</span><span class="sxs-lookup"><span data-stu-id="645c2-126">Accept any server certificate: Use this option (less secure) if you want tooskip reviewing hello site certificate before you read your data.</span></span>
5. <span data-ttu-id="645c2-127">Databázový dotaz: Zadejte příkaz SQL, který popisuje hello data, která chcete tooread.</span><span class="sxs-lookup"><span data-stu-id="645c2-127">Database query: Enter a SQL statement that describes hello data you want tooread.</span></span> <span data-ttu-id="645c2-128">V takovém případě jsme bude číst data z tabulky produktu pomocí hello následující dotaz.</span><span class="sxs-lookup"><span data-stu-id="645c2-128">In this case, we will read data from Product table using hello following query.</span></span>

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a><span data-ttu-id="645c2-129">Krok 4</span><span class="sxs-lookup"><span data-stu-id="645c2-129">Step 4</span></span>
1. <span data-ttu-id="645c2-130">Spusťte hello experiment kliknutím na tlačítko spustit pod plátnem experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-130">Run hello experiment by clicking Run under hello experiment canvas.</span></span>
2. <span data-ttu-id="645c2-131">Po dokončení hello experimentu modul čtečky hello bude mít tooindicate zelená značka zaškrtnutí, která byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="645c2-131">When hello experiment finishes, hello Reader module will have a green check mark tooindicate that it has completed successfully.</span></span> <span data-ttu-id="645c2-132">Všimněte si také hello dokončeno stav spuštění v pravém horním rohu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-132">Notice also hello Finished running status in hello upper-right corner.</span></span>

![][run]

1. <span data-ttu-id="645c2-133">toosee hello importovaných dat, klikněte na výstupní port hello v hello dolní části datové sady automobilů hello a vyberte vizualizovat.</span><span class="sxs-lookup"><span data-stu-id="645c2-133">toosee hello imported data, click hello output port at hello bottom of hello automobile dataset and select Visualize.</span></span>

## <a name="create-train-and-score-a-model"></a><span data-ttu-id="645c2-134">Vytváření, trénování a stanovení skóre modelu</span><span class="sxs-lookup"><span data-stu-id="645c2-134">Create, train and score a model</span></span>
<span data-ttu-id="645c2-135">Teď můžete použít tuto datovou sadu, která:</span><span class="sxs-lookup"><span data-stu-id="645c2-135">Now you can use this dataset to:</span></span>

* <span data-ttu-id="645c2-136">Vytvoření modelu: zpracování dat a definice funkcí</span><span class="sxs-lookup"><span data-stu-id="645c2-136">Create a Model: Process data and define features</span></span>
* <span data-ttu-id="645c2-137">Train hello model: volba a použití algoritmu učení</span><span class="sxs-lookup"><span data-stu-id="645c2-137">Train hello model: Choose and apply a learning algorithm</span></span>
* <span data-ttu-id="645c2-138">Skóre a testování hello modelu: předpovědi nová jízdních kol cena</span><span class="sxs-lookup"><span data-stu-id="645c2-138">Score and test hello model: Predict new bicycle price</span></span>

![][model]

<span data-ttu-id="645c2-139">Další informace o tom, jak toocreate, trénování, stanovení skóre a otestování machine learning hello použití modelu toolearn [vytvořit experimentu kurzu][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="645c2-139">toolearn more about how toocreate, train, score and test a machine learning model use hello [Create experiment tutorial][Create experiment tutorial].</span></span>

## <a name="write-data-tooazure-sql-data-warehouse"></a><span data-ttu-id="645c2-140">Zápis dat tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="645c2-140">Write data tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="645c2-141">Zapíše jsme hello výsledek nastavit tooProductPriceForecast tabulku v databázi AdventureWorksDW hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-141">We will write hello result set tooProductPriceForecast table in hello AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="645c2-142">Krok 1</span><span class="sxs-lookup"><span data-stu-id="645c2-142">Step 1</span></span>
<span data-ttu-id="645c2-143">Vyhledejte modul zapisovače hello v hello paleta datových sad a modulů na hello nalevo od plátna experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-143">Look for hello Writer module in hello palette of datasets and modules on hello left of hello experiment canvas.</span></span> <span data-ttu-id="645c2-144">Přetáhněte plátno experimentu toohello modulu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-144">Drag hello module toohello experiment canvas.</span></span>

![][drag_writer]

### <a name="step-2"></a><span data-ttu-id="645c2-145">Krok 2</span><span class="sxs-lookup"><span data-stu-id="645c2-145">Step 2</span></span>
<span data-ttu-id="645c2-146">Vyberte modul hello zapisovače a vyplňte podokno properties hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-146">Select hello Writer module and fill out hello properties pane.</span></span>

1. <span data-ttu-id="645c2-147">Vyberte Azure SQL Database jako hello cílové Data.</span><span class="sxs-lookup"><span data-stu-id="645c2-147">Select Azure SQL Database as hello Data Destination.</span></span>
2. <span data-ttu-id="645c2-148">Název databázového serveru: název serveru hello typu.</span><span class="sxs-lookup"><span data-stu-id="645c2-148">Database server name: Type hello server name.</span></span> <span data-ttu-id="645c2-149">Můžete použít hello [portál Azure] [ Azure portal] toofind to.</span><span class="sxs-lookup"><span data-stu-id="645c2-149">You can use hello [Azure portal][Azure portal] toofind this.</span></span>
3. <span data-ttu-id="645c2-150">Název databáze: název typu hello databáze na serveru hello jste právě určili.</span><span class="sxs-lookup"><span data-stu-id="645c2-150">Database name: Type hello name of a database on hello server you just specified.</span></span>
4. <span data-ttu-id="645c2-151">Název uživatelského účtu serveru: typ hello uživatelské jméno účtu, který má oprávnění k zápisu pro databázi hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-151">Server user account name:  Type hello user name of an account that has write permissions for hello database.</span></span>
5. <span data-ttu-id="645c2-152">Heslo uživatelského účtu serveru: Zadejte heslo hello hello zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="645c2-152">Server user account password: Provide hello password for hello specified user account.</span></span>
6. <span data-ttu-id="645c2-153">Přijměte všechny certifikát serveru (nezabezpečené): tuto možnost vyberte, pokud nechcete, aby tooview hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="645c2-153">Accept any server certificate (insecure): Select this option if you don’t want tooview hello certificate.</span></span>
7. <span data-ttu-id="645c2-154">Textový soubor s oddělovači seznam sloupců toobe uložit:, které chcete toooutput, poskytovat seznam hello datové sady nebo výsledek sloupců.</span><span class="sxs-lookup"><span data-stu-id="645c2-154">Comma-separated list of columns toobe saved: Provide a list of hello dataset or result columns that you want toooutput.</span></span>
8. <span data-ttu-id="645c2-155">Název tabulky dat: Zadejte název hello hello dat tabulky.</span><span class="sxs-lookup"><span data-stu-id="645c2-155">Data table name: Specify hello name of hello data table.</span></span>
9. <span data-ttu-id="645c2-156">Seznam oddělený čárkami datatable sloupců: Zadejte toouse názvy sloupců hello v nové tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-156">Comma-separated list of datatable columns:  Specify hello column names toouse in hello new table.</span></span> <span data-ttu-id="645c2-157">Hello názvy sloupců může lišit od hello ty, které jsou v sadě hello zdroje dat, ale musí seznam hello stejný počet sloupců sem, který definujete pro výstupní tabulku hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-157">hello column names can be different from hello ones in hello source dataset, but you must list hello same number of columns here that you define for hello output table.</span></span>
10. <span data-ttu-id="645c2-158">Počet řádků zapsaných za operace SQL Azure: můžete nakonfigurovat hello počet řádků, které jsou zapsány tooa SQL database v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="645c2-158">Number of rows written per SQL Azure operation: You can configure hello number of rows that are written tooa SQL database in one operation.</span></span>

![][writer_properties]

### <a name="step-3"></a><span data-ttu-id="645c2-159">Krok 3</span><span class="sxs-lookup"><span data-stu-id="645c2-159">Step 3</span></span>
1. <span data-ttu-id="645c2-160">Spusťte hello experiment kliknutím na tlačítko spustit pod plátnem experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="645c2-160">Run hello experiment by clicking Run under hello experiment canvas.</span></span>
2. <span data-ttu-id="645c2-161">Po dokončení experimentu hello budou mít všechny moduly tooindicate zelená značka zaškrtnutí, která budou úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="645c2-161">When hello experiment finishes, all modules will have a green check mark tooindicate that they completed successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="645c2-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="645c2-162">Next steps</span></span>
<span data-ttu-id="645c2-163">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="645c2-163">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Create experiment tutorial]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Introduction toomachine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
