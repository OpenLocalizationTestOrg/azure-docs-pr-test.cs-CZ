---
title: "aaaUsing importu a exportu dat v Azure Machine Learning webové služby | Microsoft Docs"
description: "Zjistěte, jak toouse hello toosend moduly Data importovat a exportovat Data a přijímat data z webové služby."
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
ms.openlocfilehash: 176380259b15cb338ede61c7f28ba2296b35dd52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a><span data-ttu-id="e887d-103">Nasazování webových služeb Azure ML používajících moduly Import dat a Export dat</span><span class="sxs-lookup"><span data-stu-id="e887d-103">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>

<span data-ttu-id="e887d-104">Když vytvoříte prediktivní experiment, obvykle přidat vstup webové služby a výstup.</span><span class="sxs-lookup"><span data-stu-id="e887d-104">When you create a predictive experiment, you typically add a web service input and output.</span></span> <span data-ttu-id="e887d-105">Když nasadíte hello experiment, příjemci můžete odesílat a přijímat data z hello webové služby prostřednictvím hello vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="e887d-105">When you deploy hello experiment, consumers can send and receive data from hello web service through hello inputs and outputs.</span></span> <span data-ttu-id="e887d-106">Některé aplikace může být k dispozici z datového kanálu uživatelských dat nebo se již nacházejí v zdroj externích dat, jako je například úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="e887d-106">For some applications, a consumer's data may be available from a data feed or already reside in an external data source such as Azure Blob storage.</span></span> <span data-ttu-id="e887d-107">V těchto případech nemusí čtení a zápisu dat pomocí webové služby vstupy a výstupy.</span><span class="sxs-lookup"><span data-stu-id="e887d-107">In these cases, they do not need read and write data using web service inputs and outputs.</span></span> <span data-ttu-id="e887d-108">Mohou, místo toho použít hello spuštění služby Batch (BES) tooread data ze zdroje dat hello pomocí modulu importu dat a zápis hello vyhodnocování výsledky tooa různých datových umístění pomocí modul exportovat Data.</span><span class="sxs-lookup"><span data-stu-id="e887d-108">They can, instead, use hello Batch Execution Service (BES) tooread data from hello data source using an Import Data module and write hello scoring results tooa different data location using an Export Data module.</span></span>

<span data-ttu-id="e887d-109">Hello importovat Data a Export dat moduly, může číst a zapisovat data toovarious zadejte umístění, jako je například adresa URL webového prostřednictvím protokolu HTTP, dotaz Hive, databázi Azure SQL, Azure Table storage, Azure Blob storage, datového kanálu nebo místní databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="e887d-109">hello Import Data and Export data modules, can read from and write toovarious data locations such as a Web URL via HTTP, a Hive Query, an Azure SQL database, Azure Table storage, Azure Blob storage, a Data Feed provide, or an on-premises SQL database.</span></span>

<span data-ttu-id="e887d-110">Toto téma používá hello "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" ukázkové a předpokládá datovou sadu hello již byla načtena do tabulky Azure SQL s názvem censusdata.</span><span class="sxs-lookup"><span data-stu-id="e887d-110">This topic uses hello "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample and assumes hello dataset has already been loaded into an Azure SQL table named censusdata.</span></span>

## <a name="create-hello-training-experiment"></a><span data-ttu-id="e887d-111">Vytvoření experimentu školení hello</span><span class="sxs-lookup"><span data-stu-id="e887d-111">Create hello training experiment</span></span>
<span data-ttu-id="e887d-112">Při otevření hello "vzorku 5: Train, testovací, Evaluate pro binární klasifikaci: pro dospělé datovou sadu" ukázkové ji používá hello ukázkovou pro dospělé úplné zjišťování příjem binární klasifikace datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="e887d-112">When you open hello "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample it uses hello sample Adult Census Income Binary Classification dataset.</span></span> <span data-ttu-id="e887d-113">A hello experimentu v hello plátno bude vypadat podobně jako toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="e887d-113">And hello experiment in hello canvas will look similar toohello following image:</span></span>

![Počáteční konfigurace hello experimentu.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

<span data-ttu-id="e887d-115">tooread hello data z tabulky Azure SQL hello:</span><span class="sxs-lookup"><span data-stu-id="e887d-115">tooread hello data from hello Azure SQL table:</span></span>

1. <span data-ttu-id="e887d-116">Odstraňte modul hello datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="e887d-116">Delete hello dataset module.</span></span>
2. <span data-ttu-id="e887d-117">Hello součásti vyhledávacího pole zadejte importu.</span><span class="sxs-lookup"><span data-stu-id="e887d-117">In hello components search box, type import.</span></span>
3. <span data-ttu-id="e887d-118">Ze seznamu výsledků hello, přidejte *importovat Data* modulu toohello experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="e887d-118">From hello results list, add an *Import Data* module toohello experiment canvas.</span></span>
4. <span data-ttu-id="e887d-119">Připojit výstup hello *importovat Data* vstup hello modulu Dobrý den *vyčištění chybějících dat* modulu.</span><span class="sxs-lookup"><span data-stu-id="e887d-119">Connect output of hello *Import Data* module hello input of hello *Clean Missing Data* module.</span></span>
5. <span data-ttu-id="e887d-120">V podokně vlastnosti, vyberte **Azure SQL Database** v hello **zdroj dat** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="e887d-120">In properties pane, select **Azure SQL Database** in hello **Data Source** dropdown.</span></span>
6. <span data-ttu-id="e887d-121">V hello **název databázového serveru**, **název databáze**, **uživatelské jméno**, a **heslo** pole, zadejte příslušné informace o hello pro vaše databáze.</span><span class="sxs-lookup"><span data-stu-id="e887d-121">In hello **Database server name**, **Database name**, **User name**, and **Password** fields, enter hello appropriate information for your database.</span></span>
7. <span data-ttu-id="e887d-122">V poli dotazu databáze hello zadejte následující dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="e887d-122">In hello Database query field, enter hello following query.</span></span>
   
     <span data-ttu-id="e887d-123">Vyberte [stáří]</span><span class="sxs-lookup"><span data-stu-id="e887d-123">select [age],</span></span>
   
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
     <span data-ttu-id="e887d-124">z dbo.censusdata;</span><span class="sxs-lookup"><span data-stu-id="e887d-124">from dbo.censusdata;</span></span>
8. <span data-ttu-id="e887d-125">Hello dolní části plátna experimentu hello, klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e887d-125">At hello bottom of hello experiment canvas, click **Run**.</span></span>

## <a name="create-hello-predictive-experiment"></a><span data-ttu-id="e887d-126">Vytvořit prediktivní experiment hello</span><span class="sxs-lookup"><span data-stu-id="e887d-126">Create hello predictive experiment</span></span>
<span data-ttu-id="e887d-127">Další nastavíte hello prediktivní experiment, ze kterého nasadíte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="e887d-127">Next you set up hello predictive experiment from which you deploy your web service.</span></span>

1. <span data-ttu-id="e887d-128">Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby** a vyberte **prediktivní webové služby (doporučeno)**.</span><span class="sxs-lookup"><span data-stu-id="e887d-128">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Predictive Web Service [Recommended]**.</span></span>
2. <span data-ttu-id="e887d-129">Odebrat hello *vstup webové služby* a *webové služby výstupní moduly* z prediktivní experiment hello.</span><span class="sxs-lookup"><span data-stu-id="e887d-129">Remove hello *Web Service Input* and *Web Service Output modules* from hello predictive experiment.</span></span> 
3. <span data-ttu-id="e887d-130">Hello součásti vyhledávacího pole zadejte export.</span><span class="sxs-lookup"><span data-stu-id="e887d-130">In hello components search box, type export.</span></span>
4. <span data-ttu-id="e887d-131">Ze seznamu výsledků hello, přidejte *Export dat* modulu toohello experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="e887d-131">From hello results list, add an *Export Data* module toohello experiment canvas.</span></span>
5. <span data-ttu-id="e887d-132">Připojit výstup hello *Score Model* vstup hello modulu Dobrý den *Export dat* modulu.</span><span class="sxs-lookup"><span data-stu-id="e887d-132">Connect output of hello *Score Model* module hello input of hello *Export Data* module.</span></span> 
6. <span data-ttu-id="e887d-133">V podokně vlastnosti, vyberte **Azure SQL Database** v rozevírací cílové hello data.</span><span class="sxs-lookup"><span data-stu-id="e887d-133">In properties pane, select **Azure SQL Database** in hello data destination dropdown.</span></span>
7. <span data-ttu-id="e887d-134">V hello **název databázového serveru**, **název databáze**, **název uživatelského účtu serveru**, a **heslo uživatelského účtu serveru** pole, zadejte Hello příslušné informace pro vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="e887d-134">In hello **Database server name**, **Database name**, **Server user account name**, and **Server user account password** fields, enter hello appropriate information for your database.</span></span>
8. <span data-ttu-id="e887d-135">V hello **čárkou oddělený seznam sloupců toobe Uložit** zadejte popisky vyhodnocení.</span><span class="sxs-lookup"><span data-stu-id="e887d-135">In hello **Comma separated list of columns toobe saved** field, type Scored Labels.</span></span>
9. <span data-ttu-id="e887d-136">V hello **pole název tabulky dat**, zadejte dbo. ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="e887d-136">In hello **Data table name field**, type dbo.ScoredLabels.</span></span> <span data-ttu-id="e887d-137">Pokud hello tabulka neexistuje, vytvoří se při spuštění experimentu hello nebo se nazývá hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="e887d-137">If hello table does not exist, it is created when hello experiment is run or hello web service is called.</span></span>
10. <span data-ttu-id="e887d-138">V hello **čárkami oddělený seznam sloupců datatable** pole, zadejte ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="e887d-138">In hello **Comma separated list of datatable columns** field, type ScoredLabels.</span></span>

<span data-ttu-id="e887d-139">Při psaní aplikace, volání hello konečné webové služby, můžete chtít toospecify jiné vstupní dotaz nebo cílové tabulky v době běhu.</span><span class="sxs-lookup"><span data-stu-id="e887d-139">When you write an application that calls hello final web service, you may want toospecify a different input query or destination table at run time.</span></span> <span data-ttu-id="e887d-140">tooconfigure tyto vstupy a výstupy, použijte hello parametry webové služby funkce tooset hello *importovat Data* modulu *zdroj dat* vlastnost a hello *Export dat* režimu Vlastnost cílové data.</span><span class="sxs-lookup"><span data-stu-id="e887d-140">tooconfigure these inputs and outputs, use hello Web Service Parameters feature tooset hello *Import Data* module *Data source* property and hello *Export Data* mode data destination property.</span></span>  <span data-ttu-id="e887d-141">Další informace o parametry webové služby, najdete v části hello [vstupní parametry webové služby AzureML](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) na Machine Learning Blog a hello Cortana Intelligence.</span><span class="sxs-lookup"><span data-stu-id="e887d-141">For more information on Web Service Parameters, see hello [AzureML Web Service Parameters entry](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) on hello Cortana Intelligence and Machine Learning Blog.</span></span>

<span data-ttu-id="e887d-142">tooconfigure hello parametry webové služby pro hello cílové tabulky a dotazu import hello:</span><span class="sxs-lookup"><span data-stu-id="e887d-142">tooconfigure hello Web Service Parameters for hello import query and hello destination table:</span></span>

1. <span data-ttu-id="e887d-143">V podokně Vlastnosti hello hello *importovat Data* modulu, klikněte na ikonu hello v hello top napravo od hello **databázový dotaz** pole a vyberte **nastavit jako parametr webové služby**.</span><span class="sxs-lookup"><span data-stu-id="e887d-143">In hello properties pane for hello *Import Data* module, click hello icon at hello top right of hello **Database query** field and select **Set as web service parameter**.</span></span>
2. <span data-ttu-id="e887d-144">V podokně Vlastnosti hello hello *Export dat* modulu, klikněte na ikonu hello v hello top napravo od hello **název tabulky dat** pole a vyberte **nastavit jako parametr webové služby**.</span><span class="sxs-lookup"><span data-stu-id="e887d-144">In hello properties pane for hello *Export Data* module, click hello icon at hello top right of hello **Data table name** field and select **Set as web service parameter**.</span></span>
3. <span data-ttu-id="e887d-145">Na konci hello hello *Export dat* podokno vlastností modulu, v hello **parametry webové služby** části, klikněte na databázový dotaz a přejmenujte ji dotazu.</span><span class="sxs-lookup"><span data-stu-id="e887d-145">At hello bottom of hello *Export Data* module properties pane, in hello **Web Service Parameters** section, click Database query and rename it Query.</span></span>
4. <span data-ttu-id="e887d-146">Klikněte na tlačítko **název tabulky dat** a přejmenujte ji **tabulky**.</span><span class="sxs-lookup"><span data-stu-id="e887d-146">Click **Data table name** and rename it **Table**.</span></span>

<span data-ttu-id="e887d-147">Až skončíte, experimentu by měl vypadat podobně jako toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="e887d-147">When you are done, your experiment should look similar toohello following image:</span></span>

![Poslední vzhled experimentu.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

<span data-ttu-id="e887d-149">Hello experiment teď můžete nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="e887d-149">Now you can deploy hello experiment as a web service.</span></span>

## <a name="deploy-hello-web-service"></a><span data-ttu-id="e887d-150">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="e887d-150">Deploy hello web service</span></span>
<span data-ttu-id="e887d-151">Můžete nasadit tooeither Classic nebo nové webové služby.</span><span class="sxs-lookup"><span data-stu-id="e887d-151">You can deploy tooeither a Classic or New web service.</span></span>

### <a name="deploy-a-classic-web-service"></a><span data-ttu-id="e887d-152">Nasazení Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="e887d-152">Deploy a Classic Web Service</span></span>
<span data-ttu-id="e887d-153">toodeploy jako webovou službu, Classic a vytvoření tooconsume aplikace ho:</span><span class="sxs-lookup"><span data-stu-id="e887d-153">toodeploy as a Classic Web Service and create an application tooconsume it:</span></span>

1. <span data-ttu-id="e887d-154">V hello dolní části plátna experimentu hello klikněte na tlačítko spustit.</span><span class="sxs-lookup"><span data-stu-id="e887d-154">At hello bottom of hello experiment canvas, click Run.</span></span>
2. <span data-ttu-id="e887d-155">Po dokončení hello spustit, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]**.</span><span class="sxs-lookup"><span data-stu-id="e887d-155">When hello run has completed, click **Deploy Web Service** and select **Deploy Web Service [Classic]**.</span></span>
3. <span data-ttu-id="e887d-156">Na panelu webové služby hello vyhledejte klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e887d-156">On hello web service dashboard, locate your API key.</span></span> <span data-ttu-id="e887d-157">Zkopírujte a uložte ho později toouse.</span><span class="sxs-lookup"><span data-stu-id="e887d-157">Copy and save it toouse later.</span></span>
4. <span data-ttu-id="e887d-158">V hello **výchozí koncový bod** tabulky, klikněte na tlačítko hello **Batch Execution** odkaz tooopen hello stránce nápovědy k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e887d-158">In hello **Default Endpoint** table, click hello **Batch Execution** link tooopen hello API Help Page.</span></span>
5. <span data-ttu-id="e887d-159">Ve Visual Studiu Vytvořte konzolovou aplikaci C#: **nový** > **projektu** > **Visual C#** > **Windows Klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="e887d-159">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
6. <span data-ttu-id="e887d-160">Na stránce nápovědy k rozhraní API hello, najít hello **ukázkový kód** oddíl hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="e887d-160">On hello API Help Page, find hello **Sample Code** section at hello bottom of hello page.</span></span>
7. <span data-ttu-id="e887d-161">Zkopírujte a vložte do souboru Program.cs hello ukázkový kód C# a odebrat všechny odkazy na toohello objektu blob úložiště.</span><span class="sxs-lookup"><span data-stu-id="e887d-161">Copy and paste hello C# sample code into your Program.cs file, and remove all references toohello blob storage.</span></span>
8. <span data-ttu-id="e887d-162">Aktualizujte hodnotu hello hello *apiKey* proměnné s klíčem rozhraní API hello předtím uložili.</span><span class="sxs-lookup"><span data-stu-id="e887d-162">Update hello value of hello *apiKey* variable with hello API key saved earlier.</span></span>
9. <span data-ttu-id="e887d-163">Vyhledejte hello žádosti o deklaraci a aktualizace hello hodnoty parametry webové služby, které se předávají toohello *importovat Data* a *Export dat* moduly.</span><span class="sxs-lookup"><span data-stu-id="e887d-163">Locate hello request declaration and update hello values of Web Service Parameters that are passed toohello *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="e887d-164">V takovém případě použijte původní dotaz hello ale definovat nový název tabulky.</span><span class="sxs-lookup"><span data-stu-id="e887d-164">In this case, you use hello original query, but define a new table name.</span></span>
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. <span data-ttu-id="e887d-165">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="e887d-165">Run hello application.</span></span> 

<span data-ttu-id="e887d-166">Při dokončení hello spuštění se přidá novou tabulku toohello databáze obsahující hello vyhodnocování výsledky.</span><span class="sxs-lookup"><span data-stu-id="e887d-166">On completion of hello run, a new table is added toohello database containing hello scoring results.</span></span>

### <a name="deploy-a-new-web-service"></a><span data-ttu-id="e887d-167">Nasadit novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="e887d-167">Deploy a New Web Service</span></span>

> [!NOTE] 
> <span data-ttu-id="e887d-168">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="e887d-168">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="e887d-169">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="e887d-169">For more information, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="e887d-170">toodeploy jako novou webovou službu a vytvořit tooconsume aplikace ho:</span><span class="sxs-lookup"><span data-stu-id="e887d-170">toodeploy as a New Web Service and create an application tooconsume it:</span></span>

1. <span data-ttu-id="e887d-171">Hello dolní části plátna experimentu hello, klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="e887d-171">At hello bottom of hello experiment canvas, click **Run**.</span></span>
2. <span data-ttu-id="e887d-172">Po dokončení hello spustit, klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="e887d-172">When hello run has completed, click **Deploy Web Service** and select **Deploy Web Service [New]**.</span></span>
3. <span data-ttu-id="e887d-173">Na stránce experimentu nasazení hello, zadejte název vaší webové služby a vybrat cenový plán, a pak klikněte na **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="e887d-173">On hello Deploy Experiment page, enter a name for your web service, and select a pricing plan, then click **Deploy**.</span></span>
4. <span data-ttu-id="e887d-174">Na hello **rychlý Start** klikněte na tlačítko **spotřebě**.</span><span class="sxs-lookup"><span data-stu-id="e887d-174">On hello **Quickstart** page, click **Consume**.</span></span>
5. <span data-ttu-id="e887d-175">V hello **ukázkový kód** klikněte na tlačítko **Batch**.</span><span class="sxs-lookup"><span data-stu-id="e887d-175">In hello **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="e887d-176">Ve Visual Studiu Vytvořte konzolovou aplikaci C#: **nový** > **projektu** > **Visual C#** > **Windows Klasický desktopový** > **konzoly aplikace (rozhraní .NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="e887d-176">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
7. <span data-ttu-id="e887d-177">Zkopírujte a vložte hello C# ukázkový kód do souboru Program.cs.</span><span class="sxs-lookup"><span data-stu-id="e887d-177">Copy and paste hello C# sample code into your Program.cs file.</span></span>
8. <span data-ttu-id="e887d-178">Aktualizujte hodnotu hello hello *apiKey* proměnné s hello **primární klíč** umístěný v hello **informace o základní spotřeby** části.</span><span class="sxs-lookup"><span data-stu-id="e887d-178">Update hello value of hello *apiKey* variable with hello **Primary Key** located in hello **Basic consumption info** section.</span></span>
9. <span data-ttu-id="e887d-179">Vyhledejte hello *scoreRequest* deklarace a aktualizace hodnoty hello parametry webové služby, které se předávají toohello *importovat Data* a *Export dat* moduly.</span><span class="sxs-lookup"><span data-stu-id="e887d-179">Locate hello *scoreRequest* declaration and update hello values of Web Service Parameters that are passed toohello *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="e887d-180">V takovém případě použijte původní dotaz hello ale definovat nový název tabulky.</span><span class="sxs-lookup"><span data-stu-id="e887d-180">In this case, you use hello original query, but define a new table name.</span></span>
   
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
10. <span data-ttu-id="e887d-181">Spusťte aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="e887d-181">Run hello application.</span></span> 

