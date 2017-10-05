---
title: "Vytvoření více modelů z jednoho experimentu | Microsoft Docs"
description: "Pomocí prostředí PowerShell vytvořit více modely Machine Learning a webové koncové body služby se stejným algoritmus, ale jiné školení datové sady."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 21d8c1ee0877df8d317d5a14131dc574fa5303c4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a><span data-ttu-id="c4581-103">Vytvoření mnoha modelů Machine Learning a koncových bodů webové služby z jednoho experimentu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4581-103">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>
<span data-ttu-id="c4581-104">Zde problém je běžný, machine learning: Chcete vytvořit mnoho modely, které mají stejný pracovní postup školení a použít stejný algoritmus, ale mít jiný školení datové sady jako vstup.</span><span class="sxs-lookup"><span data-stu-id="c4581-104">Here's a common machine learning problem: You want to create many models that have the same training workflow and use the same algorithm, but have different training datasets as input.</span></span> <span data-ttu-id="c4581-105">Tento článek ukazuje, jak to udělat ve velkém měřítku v Azure Machine Learning Studio pomocí právě jeden experimentu.</span><span class="sxs-lookup"><span data-stu-id="c4581-105">This article shows you how to do this at scale in Azure Machine Learning Studio using just a single experiment.</span></span>

<span data-ttu-id="c4581-106">Řekněme například, že vlastníte firma franšíza pronájem globální kolo.</span><span class="sxs-lookup"><span data-stu-id="c4581-106">For example, let's say you own a global bike rental franchise business.</span></span> <span data-ttu-id="c4581-107">Budete chtít vytvořit regresní model k vyžádání pronájem na základě historických dat předpovědi.</span><span class="sxs-lookup"><span data-stu-id="c4581-107">You want to build a regression model to predict the rental demand based on historic data.</span></span> <span data-ttu-id="c4581-108">Máte 1000 pronájem umístění po celém světě a jste shromažďují datovou sadu pro každé umístění, která obsahuje důležité funkce, jako je například datum, čas, počasí a provoz, které jsou specifické pro každé umístění.</span><span class="sxs-lookup"><span data-stu-id="c4581-108">You have 1,000 rental locations across the world and you've collected a dataset for each location that includes important features such as date, time, weather, and traffic that are specific to each location.</span></span>

<span data-ttu-id="c4581-109">Může natrénování modelu jednou pomocí sloučené verzi všechny datové sady ve všech umístěních.</span><span class="sxs-lookup"><span data-stu-id="c4581-109">You could train your model once using a merged version of all the datasets across all locations.</span></span> <span data-ttu-id="c4581-110">Ale protože každé z vašich lokalit má jedinečné prostředí, je vhodnější by k natrénování modelu regrese samostatně pomocí datovou sadu pro každé umístění.</span><span class="sxs-lookup"><span data-stu-id="c4581-110">But because each of your locations has a unique environment, a better approach would be to train your regression model separately using the dataset for each location.</span></span> <span data-ttu-id="c4581-111">Tímto způsobem, každý trained model může vzít v úvahu velikosti jiné úložiště, svazku, geography, plnění, kolo friendly provoz prostředí *atd*.</span><span class="sxs-lookup"><span data-stu-id="c4581-111">That way, each trained model could take into account the different store sizes, volume, geography, population, bike-friendly traffic environment, *etc.*.</span></span>

<span data-ttu-id="c4581-112">Který může být nejlepším přístupem, ale nechcete vytvořit 1000 experimenty školení v Azure Machine Learning s každé z nich představující jedinečné umístění.</span><span class="sxs-lookup"><span data-stu-id="c4581-112">That may be the best approach, but you don't want to create 1,000 training experiments in Azure Machine Learning with each one representing a unique location.</span></span> <span data-ttu-id="c4581-113">Kromě toho se čtenáře úloh, je také zdá se, že poměrně neefektivní vzhledem k tomu, že každý experiment by měla mít stejné komponenty s výjimkou školení datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c4581-113">Besides being an overwhelming task, it's also seems pretty inefficient since each experiment would have all the same components except for the training dataset.</span></span>

<span data-ttu-id="c4581-114">Naštěstí jsme se dá dosáhnout pomocí [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) a automatizaci úloh s [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span><span class="sxs-lookup"><span data-stu-id="c4581-114">Fortunately, we can accomplish this by using the [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) and automating the task with [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span></span>

> [!NOTE]
> <span data-ttu-id="c4581-115">Pokud chcete, aby naše ukázka rychleji probíhají rychleji, jsme budete snížit počet umístění z 1000 až 10.</span><span class="sxs-lookup"><span data-stu-id="c4581-115">To make our sample run faster, we'll reduce the number of locations from 1,000 to 10.</span></span> <span data-ttu-id="c4581-116">Ale stejné zásady a postupy platí pro 1 000 umístění.</span><span class="sxs-lookup"><span data-stu-id="c4581-116">But the same principles and procedures apply to 1,000 locations.</span></span> <span data-ttu-id="c4581-117">Jediným rozdílem je, pokud chcete cvičení z datových sad, 1 000 pravděpodobně chcete zamyslet nad paralelním spuštěním následujících skriptů prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4581-117">The only difference is that if you want to train from 1,000 datasets you probably want to think of running the following PowerShell scripts in parallel.</span></span> <span data-ttu-id="c4581-118">Jak to provést, je nad rámec tohoto článku, ale můžete najít příklady prostředí PowerShell více vláken na Internetu.</span><span class="sxs-lookup"><span data-stu-id="c4581-118">How to do that is beyond the scope of this article, but you can find examples of PowerShell multi-threading on the Internet.</span></span>  
> 
> 

## <a name="set-up-the-training-experiment"></a><span data-ttu-id="c4581-119">Nastavit výukový experiment</span><span class="sxs-lookup"><span data-stu-id="c4581-119">Set up the training experiment</span></span>
<span data-ttu-id="c4581-120">Vytvoříme použijte příklad [výukový experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , jsme již jste vytvořili v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="c4581-120">We're going to use an example [training experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) that we've already created in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="c4581-121">Otevřete tento experiment v vaše [Azure Machine Learning Studio](https://studio.azureml.net) pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="c4581-121">Open this experiment in your [Azure Machine Learning Studio](https://studio.azureml.net) workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="c4581-122">Aby bylo možné postupovat podle spolu se v tomto příkladu, můžete použít standardní pracovní prostor než volného prostoru.</span><span class="sxs-lookup"><span data-stu-id="c4581-122">In order to follow along with this example, you may want to use a standard workspace rather than a free workspace.</span></span> <span data-ttu-id="c4581-123">Jsme budete vytváření jeden koncový bod pro každého zákazníka – celkem 10 koncové body – a vzhledem k tomu, že je omezený na 3 koncové body volného prostoru, bude vyžadovat standardní pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="c4581-123">We'll be creating one endpoint for each customer - for a total of 10 endpoints - and that will require a standard workspace since a free workspace is limited to 3 endpoints.</span></span> <span data-ttu-id="c4581-124">Pokud máte pouze volného prostoru, stačí upravte skripty umožňující jenom 3 umístění níže.</span><span class="sxs-lookup"><span data-stu-id="c4581-124">If you only have a free workspace, just modify the scripts below to allow for only 3 locations.</span></span>
> 
> 

<span data-ttu-id="c4581-125">Experiment používá **Import dat** modulu k importu datovou sadu školení *customer001.csv* z účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c4581-125">The experiment uses an **Import Data** module to import the training dataset *customer001.csv* from an Azure storage account.</span></span> <span data-ttu-id="c4581-126">Předpokládejme, jsme shromážděných z všech kolo pronájem umístění datové sady školení a je uložen ve stejném umístění úložiště objektů blob s názvy souborů od *rentalloc001.csv* k *rentalloc10.csv*.</span><span class="sxs-lookup"><span data-stu-id="c4581-126">Let's assume we have collected training datasets from all bike rental locations and stored them in the same blob storage location with file names ranging from *rentalloc001.csv* to *rentalloc10.csv*.</span></span>

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

<span data-ttu-id="c4581-128">Všimněte si, že **výstup webové služby** modul byl přidán do **Train Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="c4581-128">Note that a **Web Service Output** module has been added to the **Train Model** module.</span></span>
<span data-ttu-id="c4581-129">Při nasazení tohoto experimentu jako webové služby, koncový bod přidružené že výstup vrátí trénovaného modelu ve formátu souboru .ilearner.</span><span class="sxs-lookup"><span data-stu-id="c4581-129">When this experiment is deployed as a web service, the endpoint associated with that output will return the trained model in the format of a .ilearner file.</span></span>

<span data-ttu-id="c4581-130">Také Upozorňujeme, že jsme nastavit parametr webové služby pro adresu URL, **importovat Data** používá modul.</span><span class="sxs-lookup"><span data-stu-id="c4581-130">Also note that we set up a web service parameter for the URL that the **Import Data** module uses.</span></span> <span data-ttu-id="c4581-131">To umožňuje nám jednotlivých školení datové sady pro trénování modelu pro každé umístění zadejte pomocí parametru.</span><span class="sxs-lookup"><span data-stu-id="c4581-131">This allows us to use the parameter to specify individual training datasets to train the model for each location.</span></span>
<span data-ttu-id="c4581-132">Existují jiné způsoby jsme může to provedli, například pomocí příkazu jazyka SQL s parametrem webové služby se získat data z databáze SQL Azure nebo jednoduše pomocí **vstup webové služby** modulu v datové sadě předat webovou službu.</span><span class="sxs-lookup"><span data-stu-id="c4581-132">There are other ways we could have done this, such as using a SQL query with a web service parameter to get data from a SQL Azure database, or simply using a  **Web Service Input** module to pass in a dataset to the web service.</span></span>

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

<span data-ttu-id="c4581-134">Teď umožňuje spustit tento výukový experiment pomocí výchozí hodnota *rental001.csv* jako školení datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c4581-134">Now, let's run this training experiment using the default value *rental001.csv* as the training dataset.</span></span> <span data-ttu-id="c4581-135">Je-li zobrazit výstup **Evaluate** modulu (klikněte na výstup a vyberte **vizualizovat**), najdete v části se nám získat dostatečnou výkon *AUC* = 0.91.</span><span class="sxs-lookup"><span data-stu-id="c4581-135">If you view the output of the **Evaluate** module (click the output and select **Visualize**), you can see we get a decent performance of *AUC* = 0.91.</span></span> <span data-ttu-id="c4581-136">V tomto okamžiku je připraven k nasazení webové služby mimo tento výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="c4581-136">At this point, we're ready to deploy a web service out of this training experiment.</span></span>

## <a name="deploy-the-training-and-scoring-web-services"></a><span data-ttu-id="c4581-137">Nasazení školení a vyhodnocování webové služby</span><span class="sxs-lookup"><span data-stu-id="c4581-137">Deploy the training and scoring web services</span></span>
<span data-ttu-id="c4581-138">Chcete-li nasadit webovou službu školení, klikněte na tlačítko **nastavit webové služby** tlačítko níže na plátno experimentu a vyberte **nasazení webové služby**.</span><span class="sxs-lookup"><span data-stu-id="c4581-138">To deploy the training web service, click the **Set Up Web Service** button below the experiment canvas and select **Deploy Web Service**.</span></span> <span data-ttu-id="c4581-139">Volání této webové služby "" kolo pronájem školení".</span><span class="sxs-lookup"><span data-stu-id="c4581-139">Call this web service ""Bike Rental Training".</span></span>

<span data-ttu-id="c4581-140">Teď musíme nasazení vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="c4581-140">Now we need to deploy the scoring web service.</span></span>
<span data-ttu-id="c4581-141">K tomuto účelu můžete kliknete na **nastavit webové služby** níže na plátno a vyberte **webové služby prediktivní**.</span><span class="sxs-lookup"><span data-stu-id="c4581-141">To do this, we can click **Set Up Web Service** below the canvas and select **Predictive Web Service**.</span></span> <span data-ttu-id="c4581-142">Tím se vytvoří vyhodnocování experimentu.</span><span class="sxs-lookup"><span data-stu-id="c4581-142">This creates a scoring experiment.</span></span>
<span data-ttu-id="c4581-143">Budeme potřebovat provádět několik menších úpravy, aby fungoval jako webovou službu, jako je například odebrání sloupce Popisek "cnt" ze vstupních dat a omezení výstup pouze id instance a odpovídající předpovědět hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c4581-143">We'll need to make a few minor adjustments to make it work as a web service, such as removing the label column "cnt" from the input data and limiting the output to only the instance id and the corresponding predicted value.</span></span>

<span data-ttu-id="c4581-144">Sami práci uložit, můžete jednoduše otevřít [prediktivní experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) v galerii, který je již připravena.</span><span class="sxs-lookup"><span data-stu-id="c4581-144">To save yourself that work, you can simply open the [predictive experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in the Gallery that's already been prepared.</span></span>

<span data-ttu-id="c4581-145">Pokud chcete nasadit webovou službu, spustit prediktivní experiment a pak klikněte na **nasazení webové služby** tlačítko níže na plátno.</span><span class="sxs-lookup"><span data-stu-id="c4581-145">To deploy the web service, run the predictive experiment, then click the **Deploy Web Service** button below the canvas.</span></span> <span data-ttu-id="c4581-146">Název vyhodnocování webové služby "Kolo pronájem vyhodnocování" ".</span><span class="sxs-lookup"><span data-stu-id="c4581-146">Name the scoring web service "Bike Rental Scoring"".</span></span>

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a><span data-ttu-id="c4581-147">Vytvoření 10 koncových bodů identické webové služby pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4581-147">Create 10 identical web service endpoints with PowerShell</span></span>
<span data-ttu-id="c4581-148">Této webové služby se dodává s výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="c4581-148">This web service comes with a default endpoint.</span></span> <span data-ttu-id="c4581-149">Ale ještě nejsme zajímat výchozí koncový bod vzhledem k tomu, že se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="c4581-149">But we're not as interested in the default endpoint since it can't be updated.</span></span> <span data-ttu-id="c4581-150">Co je potřeba udělat je vytvoření 10 další koncové body, jeden pro každé umístění.</span><span class="sxs-lookup"><span data-stu-id="c4581-150">What we need to do is to create 10 additional endpoints, one for each location.</span></span> <span data-ttu-id="c4581-151">Provedeme to pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4581-151">We'll do this with PowerShell.</span></span>

<span data-ttu-id="c4581-152">Nejprve nastavíme naše prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="c4581-152">First, we set up our PowerShell environment:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

<span data-ttu-id="c4581-153">Potom spusťte následující příkaz Powershellu:</span><span class="sxs-lookup"><span data-stu-id="c4581-153">Then, run the following PowerShell command:</span></span>

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

<span data-ttu-id="c4581-154">Nyní vytvořili jsme 10 koncových bodů a všechny obsahují stejné trénovaného modelu trénink na *customer001.csv*.</span><span class="sxs-lookup"><span data-stu-id="c4581-154">Now we've created 10 endpoints and they all contain the same trained model trained on *customer001.csv*.</span></span> <span data-ttu-id="c4581-155">Můžete je zobrazit na portálu správy Azure.</span><span class="sxs-lookup"><span data-stu-id="c4581-155">You can view them in the Azure Management Portal.</span></span>

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a><span data-ttu-id="c4581-157">Aktualizovat koncové body používat samostatný školení datové sady pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4581-157">Update the endpoints to use separate training datasets using PowerShell</span></span>
<span data-ttu-id="c4581-158">Dalším krokem je aktualizovat koncové body s modely, které jednoznačně trénink na jednotlivé data každého zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c4581-158">The next step is to update the endpoints with models uniquely trained on each customer's individual data.</span></span> <span data-ttu-id="c4581-159">Nejprve musíme vytvořit z těchto modelů, ale **kolo pronájem školení** webové služby.</span><span class="sxs-lookup"><span data-stu-id="c4581-159">But first we need to produce these models from the **Bike Rental Training** web service.</span></span> <span data-ttu-id="c4581-160">Přejděte zpět do **kolo pronájem školení** webové služby.</span><span class="sxs-lookup"><span data-stu-id="c4581-160">Let's go back to the **Bike Rental Training** web service.</span></span> <span data-ttu-id="c4581-161">Je potřeba volat svůj koncový bod BES 10krát s datovými sadami 10 různých školení Chcete-li vytvořit 10 odlišnými modely.</span><span class="sxs-lookup"><span data-stu-id="c4581-161">We need to call its BES endpoint 10 times with 10 different training datasets in order to produce 10 different models.</span></span> <span data-ttu-id="c4581-162">Použijeme **InovkeAmlWebServiceBESEndpoint** rutiny prostředí PowerShell k tomu.</span><span class="sxs-lookup"><span data-stu-id="c4581-162">We'll use the **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet to do this.</span></span>

<span data-ttu-id="c4581-163">Budete taky muset zadat přihlašovací údaje pro účet úložiště objektů blob do `$configContent`, a to, v polích `AccountName`, `AccountKey` a `RelativeLocation`.</span><span class="sxs-lookup"><span data-stu-id="c4581-163">You will also need to provide credentials for your blob storage account into `$configContent`, namely, at the fields `AccountName`, `AccountKey` and `RelativeLocation`.</span></span> <span data-ttu-id="c4581-164">`AccountName` Může být jedna z vaší názvy účtů, jak je vidět **portálu pro správu Azure Classic** (*úložiště* karta).</span><span class="sxs-lookup"><span data-stu-id="c4581-164">The `AccountName` can be one of your account names, as seen in the **Classic Azure Management Portal** (*Storage* tab).</span></span> <span data-ttu-id="c4581-165">Když kliknete na účet úložiště jeho `AccountKey` naleznete stisknutím **spravovat přístupové klíče** tlačítko dole a kopírování *primární přístupový klíč*.</span><span class="sxs-lookup"><span data-stu-id="c4581-165">Once you click on a storage account, its `AccountKey` can be found by pressing the **Manage Access Keys** button at the bottom and copying the *Primary Access Key*.</span></span> <span data-ttu-id="c4581-166">`RelativeLocation` Je relativní úložiště cestu, kde bude uložena nový model.</span><span class="sxs-lookup"><span data-stu-id="c4581-166">The `RelativeLocation` is the path relative to your storage where a new model will be stored.</span></span> <span data-ttu-id="c4581-167">Například cesta `hai/retrain/bike_rental/` ve skriptu níže odkazuje na kontejner s názvem `hai`, a `/retrain/bike_rental/` obsahuje podsložky.</span><span class="sxs-lookup"><span data-stu-id="c4581-167">For instance, the path `hai/retrain/bike_rental/` in the script below points to a container named `hai`, and `/retrain/bike_rental/` are subfolders.</span></span> <span data-ttu-id="c4581-168">V současné době nelze vytvořit podsložky prostřednictvím portálu uživatelského rozhraní, ale existují [několik Průzkumníci úložiště Azure](../storage/common/storage-explorers.md) které umožňují učinit.</span><span class="sxs-lookup"><span data-stu-id="c4581-168">Currently, you cannot create subfolders through the portal UI, but there are [several Azure Storage Explorers](../storage/common/storage-explorers.md) that allow you to do so.</span></span> <span data-ttu-id="c4581-169">Doporučuje se vytvořit nový kontejner v úložišti pro uložení nové trénované modely (soubory .ilearner) následujícím způsobem: ze stránky úložiště, klikněte na **přidat** tlačítko dole a pojmenujte ji `retrain`.</span><span class="sxs-lookup"><span data-stu-id="c4581-169">It is recommended that you create a new container in your storage to store the new trained models (.ilearner files) as follows: from your storage page, click on the **Add** button at the bottom and name it `retrain`.</span></span> <span data-ttu-id="c4581-170">Souhrn potřebné změny, které níže uvedeném skriptu týkají `AccountName`, `AccountKey` a `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span><span class="sxs-lookup"><span data-stu-id="c4581-170">In summary, the necassary changes to the script below pertain to `AccountName`, `AccountKey` and `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span></span>

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> <span data-ttu-id="c4581-171">Koncový bod BES je jediný podporovaný režim pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="c4581-171">The BES endpoint is the only supported mode for this operation.</span></span> <span data-ttu-id="c4581-172">Záznamy o prostředku nelze použít pro vytvoření trénované modely.</span><span class="sxs-lookup"><span data-stu-id="c4581-172">RRS cannot be used for producing trained models.</span></span>
> 
> 

<span data-ttu-id="c4581-173">Jak je uvedeno výše, namísto vytváření 10 různých BES úlohy konfigurace soubory json, jsme dynamicky místo toho vytvořte konfigurační řetězec a kanál, aby *jobConfigString* parametr  **InvokeAmlWebServceBESEndpoint** rutiny, protože je ve skutečnosti není potřeba zachovat kopii na disku.</span><span class="sxs-lookup"><span data-stu-id="c4581-173">As you can see above, instead of constructing 10 different BES job configuration json files, we dynamically create the config string instead and feed it to the *jobConfigString* parameter of the **InvokeAmlWebServceBESEndpoint** cmdlet, since there is really no need to keep a copy on disk.</span></span>

<span data-ttu-id="c4581-174">Pokud všechno proběhne správně, po chvíli se měl zobrazit 10 .ilearner souborů z *model001.ilearner* k *model010.ilearner*, v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c4581-174">If everything goes well, after a while you should see 10 .ilearner files, from *model001.ilearner* to *model010.ilearner*, in your Azure storage account.</span></span> <span data-ttu-id="c4581-175">Nyní je vše připraveno k aktualizaci naše 10 vyhodnocování koncových bodů webové služby pomocí těchto modelů pomocí **oprava AmlWebServiceEndpoint** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c4581-175">Now we're ready to update our 10 scoring web service endpoints with these models using the **Patch-AmlWebServiceEndpoint** PowerShell cmdlet.</span></span> <span data-ttu-id="c4581-176">Znovu si pamatujte, že jsme pouze oprava jiné než výchozí koncové body, které jsme prostřednictvím kódu programu vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="c4581-176">Remember again that we can only patch the non-default endpoints we programmatically created earlier.</span></span>

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

<span data-ttu-id="c4581-177">To měly být spuštěny docela rychle.</span><span class="sxs-lookup"><span data-stu-id="c4581-177">This should run fairly quickly.</span></span> <span data-ttu-id="c4581-178">Po dokončení provádění jsme budete úspěšně jste vytvořili 10 prediktivní koncových bodů webové služby, každý obsahující modulu trained model jednoznačně trénink na konkrétní datové sady do umístění pronájem, vše z jedné výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="c4581-178">When the execution finishes, we'll have successfully created 10 predictive web service endpoints, each containing a trained model uniquely trained on the dataset specific to a rental location, all from a single training experiment.</span></span> <span data-ttu-id="c4581-179">Chcete-li to ověřit, můžete zkusit volání tyto koncové body pomocí **InvokeAmlWebServiceRRSEndpoint** rutinu a poskytovat jim se stejným vstupní data a byste měli očekávat zobrazte výsledky různých předpovědi vzhledem k tomu, že probíhá Trénink modely s jinou školení sad.</span><span class="sxs-lookup"><span data-stu-id="c4581-179">To verify this, you can try calling these endpoints using the **InvokeAmlWebServiceRRSEndpoint** cmdlet, providing them with the same input data, and you should expect to see different prediction results since the models are trained with different training sets.</span></span>

## <a name="full-powershell-script"></a><span data-ttu-id="c4581-180">Úplné skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c4581-180">Full PowerShell script</span></span>
<span data-ttu-id="c4581-181">Tady je seznam úplný zdrojový kód:</span><span class="sxs-lookup"><span data-stu-id="c4581-181">Here's the listing of the full source code:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
