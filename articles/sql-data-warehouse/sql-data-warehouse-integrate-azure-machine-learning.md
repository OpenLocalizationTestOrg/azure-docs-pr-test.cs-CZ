---
title: "SQL Data Warehouse pomocí Azure Machine Learning | Microsoft Docs"
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
ms.openlocfilehash: c19860c6b5b1c15d1e29ddc67f9cf9ad4618725b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a><span data-ttu-id="3e8fe-103">SQL Data Warehouse pomocí Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3e8fe-103">Use Azure Machine Learning with SQL Data Warehouse</span></span>
<span data-ttu-id="3e8fe-104">Azure Machine Learning je plně spravovaná prediktivní analýzy služba, která můžete použít k vytvoření prediktivní modely pro vaše data v SQL Data Warehouse a pak publikovat jako připravené využívají webové služby.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-104">Azure Machine Learning is a fully managed predictive analytics service that you can use to create predictive models against your data in SQL Data Warehouse, and then publish as ready-to-consume web services.</span></span> <span data-ttu-id="3e8fe-105">Můžete seznámíte se základy prediktivní analýzy a strojového učení načtením [Úvod do strojového učení na platformě Azure][Introduction to Machine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="3e8fe-105">You can learn the basics of predictive analytics and machine learning by reading [Introduction to Machine Learning on Azure][Introduction to Machine Learning on Azure].</span></span>  <span data-ttu-id="3e8fe-106">Můžete pak zjistíte, jak k vytváření, trénování, stanovení skóre a otestování machine learning pomocí modelu [vytvořit experimentu kurzu][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="3e8fe-106">You can then learn how to create, train, score and test a machine learning model using the [Create experiment tutorial][Create experiment tutorial].</span></span>

<span data-ttu-id="3e8fe-107">V tomto článku se dozvíte, jak to provést pomocí následujících [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span><span class="sxs-lookup"><span data-stu-id="3e8fe-107">In this article, you will learn how to do the following using the [Azure Machine Learning Studio][Azure Machine Learning Studio]:</span></span>

* <span data-ttu-id="3e8fe-108">Čtení dat z databáze k vytváření, trénování a stanovení skóre prediktivního modelu</span><span class="sxs-lookup"><span data-stu-id="3e8fe-108">Read data from your database to create, train and score a predictive model</span></span>
* <span data-ttu-id="3e8fe-109">Zapsat data do databáze</span><span class="sxs-lookup"><span data-stu-id="3e8fe-109">Write data to your database</span></span>

## <a name="read-data-from-sql-data-warehouse"></a><span data-ttu-id="3e8fe-110">Čtení dat z SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e8fe-110">Read data from SQL Data Warehouse</span></span>
<span data-ttu-id="3e8fe-111">Jsme bude číst data z produktu tabulky v databázi AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-111">We will read data from Product table in the AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="3e8fe-112">Krok 1</span><span class="sxs-lookup"><span data-stu-id="3e8fe-112">Step 1</span></span>
<span data-ttu-id="3e8fe-113">Začněte nový experiment kliknutím na + nové v dolní části okna Machine Learning Studio vyberte EXPERIMENT a pak vyberte prázdný Experiment.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-113">Start a new experiment by clicking +NEW at the bottom of the Machine Learning Studio window, select EXPERIMENT, and then select Blank Experiment.</span></span> <span data-ttu-id="3e8fe-114">Vyberte výchozí název v horní části na plátno experimentu a přejmenujte jej na něco smysluplného, například předpověď ceny jízdních kol.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-114">Select the default experiment name at the top of the canvas and rename it to something meaningful, for example, Bicycle price prediction.</span></span>

### <a name="step-2"></a><span data-ttu-id="3e8fe-115">Krok 2</span><span class="sxs-lookup"><span data-stu-id="3e8fe-115">Step 2</span></span>
<span data-ttu-id="3e8fe-116">Vyhledejte modul čtečky v paleta datových sad a modulů nalevo od plátna experimentu.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-116">Look for the Reader module in the palette of datasets and modules on the left of the experiment canvas.</span></span> <span data-ttu-id="3e8fe-117">Přetáhněte na plátno experimentu modul.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-117">Drag the module to the experiment canvas.</span></span>
![][drag_reader]

### <a name="step-3"></a><span data-ttu-id="3e8fe-118">Krok 3</span><span class="sxs-lookup"><span data-stu-id="3e8fe-118">Step 3</span></span>
<span data-ttu-id="3e8fe-119">Vyberte modul čtečky a vyplňte v podokně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-119">Select the Reader module and fill out the properties pane.</span></span>

1. <span data-ttu-id="3e8fe-120">Jako zdroj dat, vyberte databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-120">Select Azure SQL Database as the Data Source.</span></span>
2. <span data-ttu-id="3e8fe-121">Název databázového serveru: Zadejte název serveru.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-121">Database server name: Type the server name.</span></span> <span data-ttu-id="3e8fe-122">Můžete použít [portál Azure] [ Azure portal] najít to.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-122">You can use the [Azure portal][Azure portal] to find this.</span></span>

![][server_name]

1. <span data-ttu-id="3e8fe-123">Název databáze: Zadejte název databáze na serveru, který jste právě určili.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-123">Database name: Type the name of a database on the server you just specified.</span></span>
2. <span data-ttu-id="3e8fe-124">Název uživatelského účtu serveru: Zadejte uživatelské jméno účtu, který má přístupová oprávnění k databázi.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-124">Server user account name:  Type the user name of an account that has access permissions for the database.</span></span>
3. <span data-ttu-id="3e8fe-125">Heslo uživatelského účtu serveru: Zadejte heslo pro zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-125">Server user account password: Provide the password for the specified user account.</span></span>
4. <span data-ttu-id="3e8fe-126">Přijměte všechny certifikát serveru: pomocí této možnosti (méně bezpečné), pokud chcete nechat přeskočit kontrola certifikát lokality před číst vaše data.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-126">Accept any server certificate: Use this option (less secure) if you want to skip reviewing the site certificate before you read your data.</span></span>
5. <span data-ttu-id="3e8fe-127">Databázový dotaz: Zadejte příkaz SQL, který popisuje data chcete číst.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-127">Database query: Enter a SQL statement that describes the data you want to read.</span></span> <span data-ttu-id="3e8fe-128">V takovém případě jsme bude číst data z tabulky produktu pomocí následujícího dotazu.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-128">In this case, we will read data from Product table using the following query.</span></span>

```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a><span data-ttu-id="3e8fe-129">Krok 4</span><span class="sxs-lookup"><span data-stu-id="3e8fe-129">Step 4</span></span>
1. <span data-ttu-id="3e8fe-130">Spusťte experiment kliknutím na tlačítko spustit pod plátnem experimentu.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-130">Run the experiment by clicking Run under the experiment canvas.</span></span>
2. <span data-ttu-id="3e8fe-131">Až se experiment dokončí, bude mít modulu Reader zelená značka zaškrtnutí označující, zda byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-131">When the experiment finishes, the Reader module will have a green check mark to indicate that it has completed successfully.</span></span> <span data-ttu-id="3e8fe-132">Všimněte si také dokončeno, stav spuštění v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-132">Notice also the Finished running status in the upper-right corner.</span></span>

![][run]

1. <span data-ttu-id="3e8fe-133">Chcete-li zobrazte naimportovaná data, klikněte na výstupní port v dolní části datové sady automobilů a vyberte vizualizovat.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-133">To see the imported data, click the output port at the bottom of the automobile dataset and select Visualize.</span></span>

## <a name="create-train-and-score-a-model"></a><span data-ttu-id="3e8fe-134">Vytváření, trénování a stanovení skóre modelu</span><span class="sxs-lookup"><span data-stu-id="3e8fe-134">Create, train and score a model</span></span>
<span data-ttu-id="3e8fe-135">Teď můžete použít tuto datovou sadu, která:</span><span class="sxs-lookup"><span data-stu-id="3e8fe-135">Now you can use this dataset to:</span></span>

* <span data-ttu-id="3e8fe-136">Vytvoření modelu: zpracování dat a definice funkcí</span><span class="sxs-lookup"><span data-stu-id="3e8fe-136">Create a Model: Process data and define features</span></span>
* <span data-ttu-id="3e8fe-137">Trénování modelu: volba a použití algoritmu učení</span><span class="sxs-lookup"><span data-stu-id="3e8fe-137">Train the model: Choose and apply a learning algorithm</span></span>
* <span data-ttu-id="3e8fe-138">Stanovení skóre a otestování modelu: předpovědi nová jízdních kol cena</span><span class="sxs-lookup"><span data-stu-id="3e8fe-138">Score and test the model: Predict new bicycle price</span></span>

![][model]

<span data-ttu-id="3e8fe-139">Další informace o tom, jak vytvořit, trénování, stanovení skóre a otestování machine learning použití modelu [vytvořit experimentu kurzu][Create experiment tutorial].</span><span class="sxs-lookup"><span data-stu-id="3e8fe-139">To learn more about how to create, train, score and test a machine learning model use the [Create experiment tutorial][Create experiment tutorial].</span></span>

## <a name="write-data-to-azure-sql-data-warehouse"></a><span data-ttu-id="3e8fe-140">Zapsat data do Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3e8fe-140">Write data to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="3e8fe-141">Zapíše jsme sadu výsledků do ProductPriceForecast tabulky v databázi AdventureWorksDW.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-141">We will write the result set to ProductPriceForecast table in the AdventureWorksDW database.</span></span>

### <a name="step-1"></a><span data-ttu-id="3e8fe-142">Krok 1</span><span class="sxs-lookup"><span data-stu-id="3e8fe-142">Step 1</span></span>
<span data-ttu-id="3e8fe-143">Vyhledejte modul zapisovače v paleta datových sad a modulů nalevo od plátna experimentu.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-143">Look for the Writer module in the palette of datasets and modules on the left of the experiment canvas.</span></span> <span data-ttu-id="3e8fe-144">Přetáhněte na plátno experimentu modul.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-144">Drag the module to the experiment canvas.</span></span>

![][drag_writer]

### <a name="step-2"></a><span data-ttu-id="3e8fe-145">Krok 2</span><span class="sxs-lookup"><span data-stu-id="3e8fe-145">Step 2</span></span>
<span data-ttu-id="3e8fe-146">Vyberte modul, zapisovače a vyplňte v podokně Vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-146">Select the Writer module and fill out the properties pane.</span></span>

1. <span data-ttu-id="3e8fe-147">Vyberte databázi Azure SQL jako cíl Data.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-147">Select Azure SQL Database as the Data Destination.</span></span>
2. <span data-ttu-id="3e8fe-148">Název databázového serveru: Zadejte název serveru.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-148">Database server name: Type the server name.</span></span> <span data-ttu-id="3e8fe-149">Můžete použít [portál Azure] [ Azure portal] najít to.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-149">You can use the [Azure portal][Azure portal] to find this.</span></span>
3. <span data-ttu-id="3e8fe-150">Název databáze: Zadejte název databáze na serveru, který jste právě určili.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-150">Database name: Type the name of a database on the server you just specified.</span></span>
4. <span data-ttu-id="3e8fe-151">Název uživatelského účtu serveru: Zadejte uživatelské jméno účtu, který má oprávnění k zápisu pro databázi.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-151">Server user account name:  Type the user name of an account that has write permissions for the database.</span></span>
5. <span data-ttu-id="3e8fe-152">Heslo uživatelského účtu serveru: Zadejte heslo pro zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-152">Server user account password: Provide the password for the specified user account.</span></span>
6. <span data-ttu-id="3e8fe-153">Přijměte všechny certifikát serveru (nezabezpečené): tuto možnost vyberte, pokud nechcete zobrazit certifikát.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-153">Accept any server certificate (insecure): Select this option if you don’t want to view the certificate.</span></span>
7. <span data-ttu-id="3e8fe-154">Čárkami oddělený seznam sloupce, které chcete uložit: Zadejte seznam datové sady nebo výsledek sloupce, které chcete výstup.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-154">Comma-separated list of columns to be saved: Provide a list of the dataset or result columns that you want to output.</span></span>
8. <span data-ttu-id="3e8fe-155">Název tabulky dat: Zadejte název tabulky data.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-155">Data table name: Specify the name of the data table.</span></span>
9. <span data-ttu-id="3e8fe-156">Seznam oddělený čárkami datatable sloupců: Zadejte názvy sloupců pro použití v nové tabulce.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-156">Comma-separated list of datatable columns:  Specify the column names to use in the new table.</span></span> <span data-ttu-id="3e8fe-157">Názvy sloupců se může lišit od těch v datové sadě zdroje, ale musí seznam stejný počet sloupců sem, kterou definujete pro výstupní tabulku.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-157">The column names can be different from the ones in the source dataset, but you must list the same number of columns here that you define for the output table.</span></span>
10. <span data-ttu-id="3e8fe-158">Počet řádků zapsaných za operace SQL Azure: můžete konfigurovat počet řádků, které jsou zapsány do databáze SQL v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-158">Number of rows written per SQL Azure operation: You can configure the number of rows that are written to a SQL database in one operation.</span></span>

![][writer_properties]

### <a name="step-3"></a><span data-ttu-id="3e8fe-159">Krok 3</span><span class="sxs-lookup"><span data-stu-id="3e8fe-159">Step 3</span></span>
1. <span data-ttu-id="3e8fe-160">Spusťte experiment kliknutím na tlačítko spustit pod plátnem experimentu.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-160">Run the experiment by clicking Run under the experiment canvas.</span></span>
2. <span data-ttu-id="3e8fe-161">Až se experiment dokončí, bude mít všechny moduly zelená značka zaškrtnutí označující, že se úspěšně dokončila.</span><span class="sxs-lookup"><span data-stu-id="3e8fe-161">When the experiment finishes, all modules will have a green check mark to indicate that they completed successfully.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e8fe-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e8fe-162">Next steps</span></span>
<span data-ttu-id="3e8fe-163">Další tipy pro vývoj najdete v části [Přehled vývoje SQL Data Warehouse][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="3e8fe-163">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>

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
[Introduction to machine learning on Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
