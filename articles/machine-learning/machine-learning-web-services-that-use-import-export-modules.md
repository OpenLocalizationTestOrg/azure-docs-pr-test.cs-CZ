---
title: "Pomocí importu a exportu dat v Azure Machine Learning webové služby | Microsoft Docs"
description: "Další informace o použití modulů Import Data a Export dat k odesílat a přijímat data z webové služby."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3a7ac351-ebd3-43a1-8c5d-18223903d08e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 123c8c2b1c5bae268b2a61c185743f2c3920175e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a><span data-ttu-id="91ec0-103">Nasazování webových služeb Azure ML používajících moduly Import dat a Export dat</span><span class="sxs-lookup"><span data-stu-id="91ec0-103">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>

<span data-ttu-id="91ec0-104">Když vytvoříte prediktivní experiment, obvykle přidat vstup webové služby a výstup.</span><span class="sxs-lookup"><span data-stu-id="91ec0-104">When you create a predictive experiment, you typically add a web service input and output.</span></span> <span data-ttu-id="91ec0-105">Když nasadíte experiment, příjemci můžete odesílat a přijímat data z webové služby prostřednictvím vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="91ec0-105">When you deploy the experiment, consumers can send and receive data from the web service through the inputs and outputs.</span></span> <span data-ttu-id="91ec0-106">Některé aplikace může být k dispozici z datového kanálu uživatelských dat nebo se již nacházejí v zdroj externích dat, jako je například úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="91ec0-106">For some applications, a consumer's data may be available from a data feed or already reside in an external data source such as Azure Blob storage.</span></span> <span data-ttu-id="91ec0-107">V těchto případech nemusí čtení a zápisu dat pomocí webové služby vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="91ec0-107">In these cases, they do not need read and write data using web service inputs and outputs.</span></span> <span data-ttu-id="91ec0-108">Mohou, místo toho čtení dat ze zdroje dat pomocí modul Import dat pomocí služby Batch provádění (BES) a vyhodnocování výsledky zapsat do různých datových umístění pomocí modul exportovat Data.</span><span class="sxs-lookup"><span data-stu-id="91ec0-108">They can, instead, use the Batch Execution Service (BES) to read data from the data source using an Import Data module and write the scoring results to a different data location using an Export Data module.</span></span>

<span data-ttu-id="91ec0-109">Umožňuje importovat Data a Export dat moduly, můžete číst z a zapisovat do různých dat, které poskytují umístění, jako je například adresa URL webového prostřednictvím protokolu HTTP, dotaz Hive, databázi Azure SQL, Azure Table storage, Azure Blob storage, datového kanálu nebo místní databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="91ec0-109">The Import Data and Export data modules, can read from and write to various data locations such as a Web URL via HTTP, a Hive Query, an Azure SQL database, Azure Table storage, Azure Blob storage, a Data Feed provide, or an on-premises SQL database.</span></span>

<span data-ttu-id="91ec0-110">Toto téma používá "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" ukázkové a předpokládá datovou sadu již byla načtena do tabulky Azure SQL s názvem censusdata.</span><span class="sxs-lookup"><span data-stu-id="91ec0-110">This topic uses the "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample and assumes the dataset has already been loaded into an Azure SQL table named censusdata.</span></span>

## <a name="create-the-training-experiment"></a><span data-ttu-id="91ec0-111">Vytvoření experimentu školení</span><span class="sxs-lookup"><span data-stu-id="91ec0-111">Create the training experiment</span></span>
<span data-ttu-id="91ec0-112">Při otevření "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" Ukázka používá ukázkovou datovou sadu pro dospělé úplné zjišťování příjem binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="91ec0-112">When you open the "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample it uses the sample Adult Census Income Binary Classification dataset.</span></span> <span data-ttu-id="91ec0-113">A na plátno experimentu bude vypadat podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="91ec0-113">And the experiment in the canvas will look similar to the following image:</span></span>

![Počáteční konfigurace experimentu.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

<span data-ttu-id="91ec0-115">Přečíst data z tabulky Azure SQL:</span><span class="sxs-lookup"><span data-stu-id="91ec0-115">To read the data from the Azure SQL table:</span></span>

1. <span data-ttu-id="91ec0-116">Odstraňte modul datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-116">Delete the dataset module.</span></span>
2. <span data-ttu-id="91ec0-117">Do pole Hledat součásti typ importu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-117">In the components search box, type import.</span></span>
3. <span data-ttu-id="91ec0-118">V seznamu výsledků přidat *importovat Data* modulu na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-118">From the results list, add an *Import Data* module to the experiment canvas.</span></span>
4. <span data-ttu-id="91ec0-119">Připojit výstup *importovat Data* modulu vstup z *vyčištění chybějících dat* modulu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-119">Connect output of the *Import Data* module the input of the *Clean Missing Data* module.</span></span>
5. <span data-ttu-id="91ec0-120">V podokně vlastnosti, vyberte **Azure SQL Database** v **zdroj dat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-120">In properties pane, select **Azure SQL Database** in the **Data Source** dropdown.</span></span>
6. <span data-ttu-id="91ec0-121">V **název databázového serveru**, **název databáze**, **uživatelské jméno**, a **heslo** pole, zadejte příslušné informace pro vaše databáze.</span><span class="sxs-lookup"><span data-stu-id="91ec0-121">In the **Database server name**, **Database name**, **User name**, and **Password** fields, enter the appropriate information for your database.</span></span>
7. <span data-ttu-id="91ec0-122">V poli dotazu databázi zadejte následující dotaz.</span><span class="sxs-lookup"><span data-stu-id="91ec0-122">In the Database query field, enter the following query.</span></span>
   
     <span data-ttu-id="91ec0-123">Vyberte [stáří]</span><span class="sxs-lookup"><span data-stu-id="91ec0-123">select [age],</span></span>
   
        [workclass],
        [fnlwgt],
        [education],
        [education-num],
        [marital-status],
        [occupation],
        [relationship],
        [race],
        [sex],
        [capital-gain],
        [capital-loss],
        [hours-per-week],
        [native-country],
        [income]
     <span data-ttu-id="91ec0-124">z dbo.censusdata;</span><span class="sxs-lookup"><span data-stu-id="91ec0-124">from dbo.censusdata;</span></span>
8. <span data-ttu-id="91ec0-125">V dolní části plátna experimentu, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-125">At the bottom of the experiment canvas, click **Run**.</span></span>

## <a name="create-the-predictive-experiment"></a><span data-ttu-id="91ec0-126">Vytvořit prediktivní experiment</span><span class="sxs-lookup"><span data-stu-id="91ec0-126">Create the predictive experiment</span></span>
<span data-ttu-id="91ec0-127">Další nastavíte prediktivní experiment, ze kterého nasadíte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-127">Next you set up the predictive experiment from which you deploy your web service.</span></span>

1. <span data-ttu-id="91ec0-128">V dolní části plátna experimentu, klikněte na tlačítko **nastavit webové služby** a vyberte **prediktivní webové služby (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-128">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Predictive Web Service [Recommended]**.</span></span>
2. <span data-ttu-id="91ec0-129">Odeberte *vstup webové služby* a *webové služby výstupní moduly* z prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="91ec0-129">Remove the *Web Service Input* and *Web Service Output modules* from the predictive experiment.</span></span> 
3. <span data-ttu-id="91ec0-130">Do pole Hledat součásti typu export.</span><span class="sxs-lookup"><span data-stu-id="91ec0-130">In the components search box, type export.</span></span>
4. <span data-ttu-id="91ec0-131">V seznamu výsledků přidat *Export dat* modulu na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-131">From the results list, add an *Export Data* module to the experiment canvas.</span></span>
5. <span data-ttu-id="91ec0-132">Připojit výstup *Score Model* modulu vstup z *Export dat* modulu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-132">Connect output of the *Score Model* module the input of the *Export Data* module.</span></span> 
6. <span data-ttu-id="91ec0-133">V podokně vlastnosti, vyberte **Azure SQL Database** v rozevírací nabídce cílový data.</span><span class="sxs-lookup"><span data-stu-id="91ec0-133">In properties pane, select **Azure SQL Database** in the data destination dropdown.</span></span>
7. <span data-ttu-id="91ec0-134">V **název databázového serveru**, **název databáze**, **název uživatelského účtu serveru**, a **heslo uživatelského účtu serveru** pole, zadejte příslušné informace pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="91ec0-134">In the **Database server name**, **Database name**, **Server user account name**, and **Server user account password** fields, enter the appropriate information for your database.</span></span>
8. <span data-ttu-id="91ec0-135">V **čárkami oddělený seznam sloupce, které chcete uložit** zadejte popisky vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="91ec0-135">In the **Comma separated list of columns to be saved** field, type Scored Labels.</span></span>
9. <span data-ttu-id="91ec0-136">V **pole název tabulky dat**, zadejte dbo. ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="91ec0-136">In the **Data table name field**, type dbo.ScoredLabels.</span></span> <span data-ttu-id="91ec0-137">Pokud tabulka neexistuje, vytvoří se při spuštění experimentu nebo se nazývá webovou službu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-137">If the table does not exist, it is created when the experiment is run or the web service is called.</span></span>
10. <span data-ttu-id="91ec0-138">V **čárkami oddělený seznam sloupců datatable** pole, zadejte ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="91ec0-138">In the **Comma separated list of datatable columns** field, type ScoredLabels.</span></span>

<span data-ttu-id="91ec0-139">Při psaní aplikace, která volá konečné webovou službu, můžete zadat různé vstupní dotaz nebo cílové tabulky v době běhu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-139">When you write an application that calls the final web service, you may want to specify a different input query or destination table at run time.</span></span> <span data-ttu-id="91ec0-140">Při konfiguraci těchto vstupy a výstupy, funkce parametry webové služby používá k nastavení *importovat Data* modulu *zdroj dat* vlastnost a *Export dat* data režimu Vlastnost cílové.</span><span class="sxs-lookup"><span data-stu-id="91ec0-140">To configure these inputs and outputs, use the Web Service Parameters feature to set the *Import Data* module *Data source* property and the *Export Data* mode data destination property.</span></span>  <span data-ttu-id="91ec0-141">Další informace o parametry webové služby, najdete v článku [vstupní parametry webové služby AzureML](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) na webu Cortana Intelligence a Machine Learning blogu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-141">For more information on Web Service Parameters, see the [AzureML Web Service Parameters entry](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) on the Cortana Intelligence and Machine Learning Blog.</span></span>

<span data-ttu-id="91ec0-142">Konfigurovat parametry webové služby pro import dotazu a cílové tabulky:</span><span class="sxs-lookup"><span data-stu-id="91ec0-142">To configure the Web Service Parameters for the import query and the destination table:</span></span>

1. <span data-ttu-id="91ec0-143">V podokně vlastností *importovat Data* modulu, klikněte na ikonu v horní části napravo od **databázový dotaz** pole a vyberte **nastavit jako parametr webové služby**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-143">In the properties pane for the *Import Data* module, click the icon at the top right of the **Database query** field and select **Set as web service parameter**.</span></span>
2. <span data-ttu-id="91ec0-144">V podokně vlastností *Export dat* modulu, klikněte na ikonu v horní části napravo od **název tabulky dat** pole a vyberte **nastavit jako parametr webové služby**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-144">In the properties pane for the *Export Data* module, click the icon at the top right of the **Data table name** field and select **Set as web service parameter**.</span></span>
3. <span data-ttu-id="91ec0-145">V dolní části *Export dat* podokno vlastností modulu v **parametry webové služby** části, klikněte na databázový dotaz a přejmenujte ji dotazu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-145">At the bottom of the *Export Data* module properties pane, in the **Web Service Parameters** section, click Database query and rename it Query.</span></span>
4. <span data-ttu-id="91ec0-146">Klikněte na tlačítko **název tabulky dat** a přejmenujte ji **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-146">Click **Data table name** and rename it **Table**.</span></span>

<span data-ttu-id="91ec0-147">Až skončíte, experimentu by měl vypadat podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="91ec0-147">When you are done, your experiment should look similar to the following image:</span></span>

![Poslední vzhled experimentu.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

<span data-ttu-id="91ec0-149">Experiment teď můžete nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-149">Now you can deploy the experiment as a web service.</span></span>

## <a name="deploy-the-web-service"></a><span data-ttu-id="91ec0-150">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="91ec0-150">Deploy the web service</span></span>
<span data-ttu-id="91ec0-151">Můžete nasadit na klasickém nebo novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="91ec0-151">You can deploy to either a Classic or New web service.</span></span>

### <a name="deploy-a-classic-web-service"></a><span data-ttu-id="91ec0-152">Nasazení Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="91ec0-152">Deploy a Classic Web Service</span></span>
<span data-ttu-id="91ec0-153">Nasadit jako webovou službu, Classic a vytvoření aplikace ho zpracovat:</span><span class="sxs-lookup"><span data-stu-id="91ec0-153">To deploy as a Classic Web Service and create an application to consume it:</span></span>

1. <span data-ttu-id="91ec0-154">V dolní části plátna experimentu klikněte na tlačítko spustit.</span><span class="sxs-lookup"><span data-stu-id="91ec0-154">At the bottom of the experiment canvas, click Run.</span></span>
2. <span data-ttu-id="91ec0-155">Po dokončení spuštění, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-155">When the run has completed, click **Deploy Web Service** and select **Deploy Web Service [Classic]**.</span></span>
3. <span data-ttu-id="91ec0-156">Na řídicím panelu webové služby vyhledejte klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="91ec0-156">On the web service dashboard, locate your API key.</span></span> <span data-ttu-id="91ec0-157">Zkopírujte a uložte jej pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="91ec0-157">Copy and save it to use later.</span></span>
4. <span data-ttu-id="91ec0-158">V **výchozí koncový bod** tabulky, klikněte **Batch Execution** odkazu k otevření stránce nápovědy k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="91ec0-158">In the **Default Endpoint** table, click the **Batch Execution** link to open the API Help Page.</span></span>
5. <span data-ttu-id="91ec0-159">Ve Visual Studiu Vytvořte konzolovou aplikaci C#: **nový** > **projektu** > **Visual C#** > **Windows Klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-159">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
6. <span data-ttu-id="91ec0-160">Na stránce nápovědy k rozhraní API najít **ukázkový kód** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="91ec0-160">On the API Help Page, find the **Sample Code** section at the bottom of the page.</span></span>
7. <span data-ttu-id="91ec0-161">Zkopírujte a vložte ukázkový kód C# do souboru Program.cs a odeberte všechny odkazy na úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="91ec0-161">Copy and paste the C# sample code into your Program.cs file, and remove all references to the blob storage.</span></span>
8. <span data-ttu-id="91ec0-162">Aktualizujte hodnotu *apiKey* proměnné s klíčem rozhraní API předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="91ec0-162">Update the value of the *apiKey* variable with the API key saved earlier.</span></span>
9. <span data-ttu-id="91ec0-163">Vyhledejte deklaraci požadavku a aktualizujte hodnoty parametry webové služby, které jsou předávány *importovat Data* a *Export dat* moduly.</span><span class="sxs-lookup"><span data-stu-id="91ec0-163">Locate the request declaration and update the values of Web Service Parameters that are passed to the *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="91ec0-164">V takovém případě použijte původní dotaz ale definovat nový název tabulky.</span><span class="sxs-lookup"><span data-stu-id="91ec0-164">In this case, you use the original query, but define a new table name.</span></span>
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. <span data-ttu-id="91ec0-165">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91ec0-165">Run the application.</span></span> 

<span data-ttu-id="91ec0-166">Při dokončení spuštění, se přidá novou tabulku k databázi obsahující vyhodnocování výsledky.</span><span class="sxs-lookup"><span data-stu-id="91ec0-166">On completion of the run, a new table is added to the database containing the scoring results.</span></span>

### <a name="deploy-a-new-web-service"></a><span data-ttu-id="91ec0-167">Nasadit novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="91ec0-167">Deploy a New Web Service</span></span>

> [!NOTE] 
> <span data-ttu-id="91ec0-168">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které, můžete nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="91ec0-168">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="91ec0-169">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="91ec0-169">For more information, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="91ec0-170">Nasadit jako novou webovou službu a vytvořte aplikaci pro ho využívají:</span><span class="sxs-lookup"><span data-stu-id="91ec0-170">To deploy as a New Web Service and create an application to consume it:</span></span>

1. <span data-ttu-id="91ec0-171">V dolní části plátna experimentu, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-171">At the bottom of the experiment canvas, click **Run**.</span></span>
2. <span data-ttu-id="91ec0-172">Po dokončení spuštění, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-172">When the run has completed, click **Deploy Web Service** and select **Deploy Web Service [New]**.</span></span>
3. <span data-ttu-id="91ec0-173">Na stránce experimentu nasazení zadejte název vaší webové služby a vybrat cenový plán, a pak klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-173">On the Deploy Experiment page, enter a name for your web service, and select a pricing plan, then click **Deploy**.</span></span>
4. <span data-ttu-id="91ec0-174">Na **rychlý Start** klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-174">On the **Quickstart** page, click **Consume**.</span></span>
5. <span data-ttu-id="91ec0-175">V **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-175">In the **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="91ec0-176">Ve Visual Studiu Vytvořte konzolovou aplikaci C#: **nový** > **projektu** > **Visual C#** > **Windows Klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="91ec0-176">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
7. <span data-ttu-id="91ec0-177">Zkopírujte a vložte ukázkový kód C# do souboru Program.cs.</span><span class="sxs-lookup"><span data-stu-id="91ec0-177">Copy and paste the C# sample code into your Program.cs file.</span></span>
8. <span data-ttu-id="91ec0-178">Aktualizujte hodnotu *apiKey* proměnné s **primární klíč** umístěný v **informace o základní spotřeby** části.</span><span class="sxs-lookup"><span data-stu-id="91ec0-178">Update the value of the *apiKey* variable with the **Primary Key** located in the **Basic consumption info** section.</span></span>
9. <span data-ttu-id="91ec0-179">Vyhledejte *scoreRequest* deklarace a aktualizujte hodnoty parametry webové služby, které jsou předávány *importovat Data* a *Export dat* moduly.</span><span class="sxs-lookup"><span data-stu-id="91ec0-179">Locate the *scoreRequest* declaration and update the values of Web Service Parameters that are passed to the *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="91ec0-180">V takovém případě použijte původní dotaz ale definovat nový název tabulky.</span><span class="sxs-lookup"><span data-stu-id="91ec0-180">In this case, you use the original query, but define a new table name.</span></span>
   
        var scoreRequest = new
        {       
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };
10. <span data-ttu-id="91ec0-181">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91ec0-181">Run the application.</span></span> 

