---
title: "aaaCreate více modely z jednoho experimentu | Microsoft Docs"
description: "Pomocí prostředí PowerShell toocreate více modelů Machine Learning a webové služby koncové body pomocí hello stejný algoritmus ale jiné školení datové sady."
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
ms.openlocfilehash: 4a258a8ab26395d4169a058520151c860e16e169
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a><span data-ttu-id="04fa1-103">Vytvoření mnoha modelů Machine Learning a koncových bodů webové služby z jednoho experimentu pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="04fa1-103">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>
<span data-ttu-id="04fa1-104">Zde problém je běžný, machine learning: Chcete toocreate mnoho modely, které mají hello stejný pracovní postup školení a použití hello stejný algoritmus, ale mají různé školení datové sady jako vstup.</span><span class="sxs-lookup"><span data-stu-id="04fa1-104">Here's a common machine learning problem: You want toocreate many models that have hello same training workflow and use hello same algorithm, but have different training datasets as input.</span></span> <span data-ttu-id="04fa1-105">Tento článek ukazuje, jak toodo to škálované v Azure Machine Learning Studio pomocí právě jeden experimentu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-105">This article shows you how toodo this at scale in Azure Machine Learning Studio using just a single experiment.</span></span>

<span data-ttu-id="04fa1-106">Řekněme například, že vlastníte firma franšíza pronájem globální kolo.</span><span class="sxs-lookup"><span data-stu-id="04fa1-106">For example, let's say you own a global bike rental franchise business.</span></span> <span data-ttu-id="04fa1-107">Chcete toobuild regresní model toopredict hello pronájem požadavek na základě historických dat.</span><span class="sxs-lookup"><span data-stu-id="04fa1-107">You want toobuild a regression model toopredict hello rental demand based on historic data.</span></span> <span data-ttu-id="04fa1-108">Máte 1000 pronájem umístění napříč hello, world a jste shromažďují datovou sadu pro každé umístění, která obsahuje důležité funkce, jako je například datum, čas, počasí a provoz, které jsou specifické tooeach umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-108">You have 1,000 rental locations across hello world and you've collected a dataset for each location that includes important features such as date, time, weather, and traffic that are specific tooeach location.</span></span>

<span data-ttu-id="04fa1-109">Může natrénování modelu jednou pomocí sloučené verze všech datových sad hello ve všech umístěních.</span><span class="sxs-lookup"><span data-stu-id="04fa1-109">You could train your model once using a merged version of all hello datasets across all locations.</span></span> <span data-ttu-id="04fa1-110">Ale protože každé z vašich lokalit má jedinečné prostředí, je vhodnější by být tootrain regresní model samostatně pomocí hello datovou sadu pro každé umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-110">But because each of your locations has a unique environment, a better approach would be tootrain your regression model separately using hello dataset for each location.</span></span> <span data-ttu-id="04fa1-111">Tímto způsobem do velikosti jiném úložišti hello účtů, svazku, geography, plnění, kolo friendly provoz prostředí, může trvat každý trained model *atd*.</span><span class="sxs-lookup"><span data-stu-id="04fa1-111">That way, each trained model could take into account hello different store sizes, volume, geography, population, bike-friendly traffic environment, *etc.*.</span></span>

<span data-ttu-id="04fa1-112">Který může být nejlepším postupem hello, ale nechcete, aby toocreate 1000 školení experimenty v Azure Machine Learning s každé z nich představující jedinečné umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-112">That may be hello best approach, but you don't want toocreate 1,000 training experiments in Azure Machine Learning with each one representing a unique location.</span></span> <span data-ttu-id="04fa1-113">Kromě toho se čtenáře úloh, je také zdá se, že poměrně neefektivní vzhledem k tomu, že každý experiment by měla mít všechny hello stejné komponenty s výjimkou hello školení datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-113">Besides being an overwhelming task, it's also seems pretty inefficient since each experiment would have all hello same components except for hello training dataset.</span></span>

<span data-ttu-id="04fa1-114">Naštěstí jsme se dá dosáhnout pomocí hello [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) a automatizace úkolů hello s [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span><span class="sxs-lookup"><span data-stu-id="04fa1-114">Fortunately, we can accomplish this by using hello [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) and automating hello task with [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span></span>

> [!NOTE]
> <span data-ttu-id="04fa1-115">toomake naše ukázka rychleji probíhají rychleji, jsme budete snížit hello počet umístění z 1 000 too10.</span><span class="sxs-lookup"><span data-stu-id="04fa1-115">toomake our sample run faster, we'll reduce hello number of locations from 1,000 too10.</span></span> <span data-ttu-id="04fa1-116">Ale hello stejné zásady a postupy platí too1 000 umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-116">But hello same principles and procedures apply too1,000 locations.</span></span> <span data-ttu-id="04fa1-117">Hello jediným rozdílem je, že pokud chcete, aby tootrain z 1 000 datových sad, budete ho zřejmě chtít toothink spuštěných hello následujících skriptů prostředí PowerShell paralelně.</span><span class="sxs-lookup"><span data-stu-id="04fa1-117">hello only difference is that if you want tootrain from 1,000 datasets you probably want toothink of running hello following PowerShell scripts in parallel.</span></span> <span data-ttu-id="04fa1-118">Jak toodo, který je mimo rozsah hello v tomto článku, ale najdete příklady prostředí PowerShell více vláken na hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-118">How toodo that is beyond hello scope of this article, but you can find examples of PowerShell multi-threading on hello Internet.</span></span>  
> 
> 

## <a name="set-up-hello-training-experiment"></a><span data-ttu-id="04fa1-119">Nastavit výukový experiment hello</span><span class="sxs-lookup"><span data-stu-id="04fa1-119">Set up hello training experiment</span></span>
<span data-ttu-id="04fa1-120">Vytvoříme toouse příklad [výukový experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , jsme již jste vytvořili v hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="04fa1-120">We're going toouse an example [training experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) that we've already created in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="04fa1-121">Otevřete tento experiment v vaše [Azure Machine Learning Studio](https://studio.azureml.net) pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="04fa1-121">Open this experiment in your [Azure Machine Learning Studio](https://studio.azureml.net) workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="04fa1-122">V pořadí toofollow spolu se v tomto příkladu můžete chtít toouse standardní pracovní prostor než volného prostoru.</span><span class="sxs-lookup"><span data-stu-id="04fa1-122">In order toofollow along with this example, you may want toouse a standard workspace rather than a free workspace.</span></span> <span data-ttu-id="04fa1-123">Jsme budete vytváření jeden koncový bod pro každého zákazníka – celkem 10 koncové body – a vzhledem k tomu, že volného prostoru je omezená too3 koncové body, které se vyžadují standardní pracovní prostor.</span><span class="sxs-lookup"><span data-stu-id="04fa1-123">We'll be creating one endpoint for each customer - for a total of 10 endpoints - and that will require a standard workspace since a free workspace is limited too3 endpoints.</span></span> <span data-ttu-id="04fa1-124">Pokud máte pouze volného prostoru, stačí upravte skripty hello níže tooallow pro jenom 3 umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-124">If you only have a free workspace, just modify hello scripts below tooallow for only 3 locations.</span></span>
> 
> 

<span data-ttu-id="04fa1-125">Hello experiment používá **importovat Data** modulu tooimport hello školení datovou sadu *customer001.csv* z účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="04fa1-125">hello experiment uses an **Import Data** module tooimport hello training dataset *customer001.csv* from an Azure storage account.</span></span> <span data-ttu-id="04fa1-126">Předpokládejme, jsme shromážděných z všech kolo pronájem umístění datové sady školení a je uložen v hello stejné umístění úložiště blob s názvy souborů od *rentalloc001.csv* příliš*rentalloc10.csv* .</span><span class="sxs-lookup"><span data-stu-id="04fa1-126">Let's assume we have collected training datasets from all bike rental locations and stored them in hello same blob storage location with file names ranging from *rentalloc001.csv* too*rentalloc10.csv*.</span></span>

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

<span data-ttu-id="04fa1-128">Všimněte si, že **výstup webové služby** modulu přidala toohello **Train Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-128">Note that a **Web Service Output** module has been added toohello **Train Model** module.</span></span>
<span data-ttu-id="04fa1-129">Při nasazení tohoto experimentu jako webové služby, koncový bod hello přidružený tento výstup hello formát souboru .ilearner, vrátí hello trained model.</span><span class="sxs-lookup"><span data-stu-id="04fa1-129">When this experiment is deployed as a web service, hello endpoint associated with that output will return hello trained model in hello format of a .ilearner file.</span></span>

<span data-ttu-id="04fa1-130">Všimněte si, že jsme nastavit parametr webové služby pro adresu URL hello této hello **importovat Data** používá modul.</span><span class="sxs-lookup"><span data-stu-id="04fa1-130">Also note that we set up a web service parameter for hello URL that hello **Import Data** module uses.</span></span> <span data-ttu-id="04fa1-131">To umožňuje nám toouse hello parametr toospecify jednotlivých školení datové sady tootrain hello modelu pro každé umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-131">This allows us toouse hello parameter toospecify individual training datasets tootrain hello model for each location.</span></span>
<span data-ttu-id="04fa1-132">Existují jiné způsoby jsme může to provedli, například pomocí příkazu jazyka SQL webové služby parametr tooget daty z databáze SQL Azure nebo jednoduše pomocí **vstup webové služby** toopass modulu v datové sadě toohello webové služby.</span><span class="sxs-lookup"><span data-stu-id="04fa1-132">There are other ways we could have done this, such as using a SQL query with a web service parameter tooget data from a SQL Azure database, or simply using a  **Web Service Input** module toopass in a dataset toohello web service.</span></span>

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

<span data-ttu-id="04fa1-134">Teď umožňuje spustit tento výukový experiment s výchozí hodnotou hello *rental001.csv* jako hello školení datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-134">Now, let's run this training experiment using hello default value *rental001.csv* as hello training dataset.</span></span> <span data-ttu-id="04fa1-135">Při zobrazení hello výstup hello **Evaluate** modulu (klikněte na výstup hello a vyberte **vizualizovat**), najdete v části se nám získat dostatečnou výkon *AUC* = 0.91.</span><span class="sxs-lookup"><span data-stu-id="04fa1-135">If you view hello output of hello **Evaluate** module (click hello output and select **Visualize**), you can see we get a decent performance of *AUC* = 0.91.</span></span> <span data-ttu-id="04fa1-136">V tomto okamžiku nám připravené toodeploy webová služba mimo tento výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="04fa1-136">At this point, we're ready toodeploy a web service out of this training experiment.</span></span>

## <a name="deploy-hello-training-and-scoring-web-services"></a><span data-ttu-id="04fa1-137">Nasazení hello školení a vyhodnocování webové služby</span><span class="sxs-lookup"><span data-stu-id="04fa1-137">Deploy hello training and scoring web services</span></span>
<span data-ttu-id="04fa1-138">toodeploy hello cvičení webové služby, klikněte na tlačítko hello **nastavit webové služby** tlačítko pod plátnem experimentu hello a vyberte **nasazení webové služby**.</span><span class="sxs-lookup"><span data-stu-id="04fa1-138">toodeploy hello training web service, click hello **Set Up Web Service** button below hello experiment canvas and select **Deploy Web Service**.</span></span> <span data-ttu-id="04fa1-139">Volání této webové služby "" kolo pronájem školení".</span><span class="sxs-lookup"><span data-stu-id="04fa1-139">Call this web service ""Bike Rental Training".</span></span>

<span data-ttu-id="04fa1-140">Nyní potřebujeme toodeploy hello vyhodnocování webové služby.</span><span class="sxs-lookup"><span data-stu-id="04fa1-140">Now we need toodeploy hello scoring web service.</span></span>
<span data-ttu-id="04fa1-141">toodo toho jsme klikněte na tlačítko **nastavit webové služby** níže hello plátno a vyberte **webové služby prediktivní**.</span><span class="sxs-lookup"><span data-stu-id="04fa1-141">toodo this, we can click **Set Up Web Service** below hello canvas and select **Predictive Web Service**.</span></span> <span data-ttu-id="04fa1-142">Tím se vytvoří vyhodnocování experimentu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-142">This creates a scoring experiment.</span></span>
<span data-ttu-id="04fa1-143">Toomake potřebujeme pár menší úpravy toomake fungovat jako webovou službu, například odebráním hello popisek sloupce "cnt" hello vstupní data a omezení id instance hello tooonly výstup hello a odpovídající hello předpovědět hodnotu.</span><span class="sxs-lookup"><span data-stu-id="04fa1-143">We'll need toomake a few minor adjustments toomake it work as a web service, such as removing hello label column "cnt" from hello input data and limiting hello output tooonly hello instance id and hello corresponding predicted value.</span></span>

<span data-ttu-id="04fa1-144">toosave sami, které pracují, můžete jednoduše otevřít hello [prediktivní experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) v hello galerie, která je již připravena.</span><span class="sxs-lookup"><span data-stu-id="04fa1-144">toosave yourself that work, you can simply open hello [predictive experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in hello Gallery that's already been prepared.</span></span>

<span data-ttu-id="04fa1-145">toodeploy hello webové služby, spustit experiment prediktivní hello, pak klikněte na hello **nasazení webové služby** tlačítko pod plátnem hello.</span><span class="sxs-lookup"><span data-stu-id="04fa1-145">toodeploy hello web service, run hello predictive experiment, then click hello **Deploy Web Service** button below hello canvas.</span></span> <span data-ttu-id="04fa1-146">Název hello vyhodnocování webové služby "Kolo pronájem vyhodnocování" ".</span><span class="sxs-lookup"><span data-stu-id="04fa1-146">Name hello scoring web service "Bike Rental Scoring"".</span></span>

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a><span data-ttu-id="04fa1-147">Vytvoření 10 koncových bodů identické webové služby pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="04fa1-147">Create 10 identical web service endpoints with PowerShell</span></span>
<span data-ttu-id="04fa1-148">Této webové služby se dodává s výchozí koncový bod.</span><span class="sxs-lookup"><span data-stu-id="04fa1-148">This web service comes with a default endpoint.</span></span> <span data-ttu-id="04fa1-149">Ale ještě nejsme zajímat hello výchozí koncový bod vzhledem k tomu, že se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="04fa1-149">But we're not as interested in hello default endpoint since it can't be updated.</span></span> <span data-ttu-id="04fa1-150">Co potřebujeme toodo je toocreate 10 další koncové body, jeden pro každé umístění.</span><span class="sxs-lookup"><span data-stu-id="04fa1-150">What we need toodo is toocreate 10 additional endpoints, one for each location.</span></span> <span data-ttu-id="04fa1-151">Provedeme to pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="04fa1-151">We'll do this with PowerShell.</span></span>

<span data-ttu-id="04fa1-152">Nejprve nastavíme naše prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="04fa1-152">First, we set up our PowerShell environment:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and is properly set toopoint toohello valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

<span data-ttu-id="04fa1-153">Potom spusťte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="04fa1-153">Then, run hello following PowerShell command:</span></span>

    # Create 10 endpoints on hello scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

<span data-ttu-id="04fa1-154">Nyní vytvořili jsme 10 koncových bodů a všechny obsahují hello stejný trained model trénink na *customer001.csv*.</span><span class="sxs-lookup"><span data-stu-id="04fa1-154">Now we've created 10 endpoints and they all contain hello same trained model trained on *customer001.csv*.</span></span> <span data-ttu-id="04fa1-155">Můžete je zobrazit v hello portálu pro správu Azure.</span><span class="sxs-lookup"><span data-stu-id="04fa1-155">You can view them in hello Azure Management Portal.</span></span>

![Bitové kopie](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-hello-endpoints-toouse-separate-training-datasets-using-powershell"></a><span data-ttu-id="04fa1-157">Aktualizovat hello koncové body toouse samostatné školení datové sady pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="04fa1-157">Update hello endpoints toouse separate training datasets using PowerShell</span></span>
<span data-ttu-id="04fa1-158">dalším krokem Hello je tooupdate hello koncových bodů s modely, které jednoznačně trénink na jednotlivé data každého zákazníka.</span><span class="sxs-lookup"><span data-stu-id="04fa1-158">hello next step is tooupdate hello endpoints with models uniquely trained on each customer's individual data.</span></span> <span data-ttu-id="04fa1-159">Ale nejdřív potřebujeme tooproduce, tyto modely z hello **kolo pronájem školení** webové služby.</span><span class="sxs-lookup"><span data-stu-id="04fa1-159">But first we need tooproduce these models from hello **Bike Rental Training** web service.</span></span> <span data-ttu-id="04fa1-160">Přejděte zpět toohello **kolo pronájem školení** webové služby.</span><span class="sxs-lookup"><span data-stu-id="04fa1-160">Let's go back toohello **Bike Rental Training** web service.</span></span> <span data-ttu-id="04fa1-161">Potřebujeme toocall svůj koncový bod BES 10krát s 10 různých školení datové sady v různých modelech tooproduce 10 pořadí.</span><span class="sxs-lookup"><span data-stu-id="04fa1-161">We need toocall its BES endpoint 10 times with 10 different training datasets in order tooproduce 10 different models.</span></span> <span data-ttu-id="04fa1-162">Použijeme hello **InovkeAmlWebServiceBESEndpoint** toodo rutiny prostředí PowerShell to.</span><span class="sxs-lookup"><span data-stu-id="04fa1-162">We'll use hello **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet toodo this.</span></span>

<span data-ttu-id="04fa1-163">Budete také potřebovat tooprovide pověření pro účet úložiště objektů blob do `$configContent`, a to, v polích hello `AccountName`, `AccountKey` a `RelativeLocation`.</span><span class="sxs-lookup"><span data-stu-id="04fa1-163">You will also need tooprovide credentials for your blob storage account into `$configContent`, namely, at hello fields `AccountName`, `AccountKey` and `RelativeLocation`.</span></span> <span data-ttu-id="04fa1-164">Hello `AccountName` může být jedna z vaší názvy účtů, jak je vidět v hello **portálu pro správu Azure Classic** (*úložiště* karta).</span><span class="sxs-lookup"><span data-stu-id="04fa1-164">hello `AccountName` can be one of your account names, as seen in hello **Classic Azure Management Portal** (*Storage* tab).</span></span> <span data-ttu-id="04fa1-165">Když kliknete na účet úložiště jeho `AccountKey` získáte stisknutím hello **spravovat přístupové klíče** tlačítko v dolní části hello a kopírování hello *primární přístupový klíč*.</span><span class="sxs-lookup"><span data-stu-id="04fa1-165">Once you click on a storage account, its `AccountKey` can be found by pressing hello **Manage Access Keys** button at hello bottom and copying hello *Primary Access Key*.</span></span> <span data-ttu-id="04fa1-166">Hello `RelativeLocation` je hello cesta relativní tooyour úložiště, kde bude uložena nový model.</span><span class="sxs-lookup"><span data-stu-id="04fa1-166">hello `RelativeLocation` is hello path relative tooyour storage where a new model will be stored.</span></span> <span data-ttu-id="04fa1-167">Například cesta hello `hai/retrain/bike_rental/` ve skriptu hello pod body tooa kontejner s názvem `hai`, a `/retrain/bike_rental/` obsahuje podsložky.</span><span class="sxs-lookup"><span data-stu-id="04fa1-167">For instance, hello path `hai/retrain/bike_rental/` in hello script below points tooa container named `hai`, and `/retrain/bike_rental/` are subfolders.</span></span> <span data-ttu-id="04fa1-168">V současné době nelze vytvořit podsložky prostřednictvím portálu hello uživatelského rozhraní, ale existují [několik Průzkumníci úložiště Azure](../storage/common/storage-explorers.md) , umožňují toodo tak.</span><span class="sxs-lookup"><span data-stu-id="04fa1-168">Currently, you cannot create subfolders through hello portal UI, but there are [several Azure Storage Explorers](../storage/common/storage-explorers.md) that allow you toodo so.</span></span> <span data-ttu-id="04fa1-169">Doporučuje se vytvořit nový kontejner v vašeho úložiště toostore hello nové trénované modely (soubory .ilearner) následujícím způsobem: ze stránky úložiště, klikněte na hello **přidat** tlačítko dole v hello a pojmenujte ji `retrain`.</span><span class="sxs-lookup"><span data-stu-id="04fa1-169">It is recommended that you create a new container in your storage toostore hello new trained models (.ilearner files) as follows: from your storage page, click on hello **Add** button at hello bottom and name it `retrain`.</span></span> <span data-ttu-id="04fa1-170">Souhrnně níže hello potřebné změny toohello skriptu týkají příliš`AccountName`, `AccountKey` a `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span><span class="sxs-lookup"><span data-stu-id="04fa1-170">In summary, hello necassary changes toohello script below pertain too`AccountName`, `AccountKey` and `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span></span>

    # Invoke hello retraining API 10 times
    # This is hello default (and hello only) endpoint on hello training web service
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
> <span data-ttu-id="04fa1-171">Hello koncový bod BES je hello režimu podporovány pouze pro tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="04fa1-171">hello BES endpoint is hello only supported mode for this operation.</span></span> <span data-ttu-id="04fa1-172">Záznamy o prostředku nelze použít pro vytvoření trénované modely.</span><span class="sxs-lookup"><span data-stu-id="04fa1-172">RRS cannot be used for producing trained models.</span></span>
> 
> 

<span data-ttu-id="04fa1-173">Jak je uvedeno výše, namísto vytváření 10 různých BES úlohy konfigurace soubory json, dynamicky místo vytvoření hello konfigurační řetězec a informačního kanálu toohello *jobConfigString* parametr hello  **InvokeAmlWebServceBESEndpoint** rutiny, protože je skutečně bez nutnosti tookeep kopii na disku.</span><span class="sxs-lookup"><span data-stu-id="04fa1-173">As you can see above, instead of constructing 10 different BES job configuration json files, we dynamically create hello config string instead and feed it toohello *jobConfigString* parameter of hello **InvokeAmlWebServceBESEndpoint** cmdlet, since there is really no need tookeep a copy on disk.</span></span>

<span data-ttu-id="04fa1-174">Pokud všechno proběhne správně, po chvíli se měl zobrazit 10 .ilearner souborů z *model001.ilearner* příliš*model010.ilearner*, v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="04fa1-174">If everything goes well, after a while you should see 10 .ilearner files, from *model001.ilearner* too*model010.ilearner*, in your Azure storage account.</span></span> <span data-ttu-id="04fa1-175">Teď máme připraven tooupdate naše 10 vyhodnocování webové koncové body služby pomocí těchto modelů pomocí hello **oprava AmlWebServiceEndpoint** rutiny prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="04fa1-175">Now we're ready tooupdate our 10 scoring web service endpoints with these models using hello **Patch-AmlWebServiceEndpoint** PowerShell cmdlet.</span></span> <span data-ttu-id="04fa1-176">Znovu si pamatujte, že jsme pouze oprava koncové body hello jiné než výchozí, které jsme prostřednictvím kódu programu vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="04fa1-176">Remember again that we can only patch hello non-default endpoints we programmatically created earlier.</span></span>

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

<span data-ttu-id="04fa1-177">To měly být spuštěny docela rychle.</span><span class="sxs-lookup"><span data-stu-id="04fa1-177">This should run fairly quickly.</span></span> <span data-ttu-id="04fa1-178">Po dokončení provádění hello jsme budete úspěšně jste vytvořili 10 koncových bodů prediktivní webové služby, každá obsahuje modulu trained model jednoznačně trénink na hello datovou sadu konkrétní tooa pronájem umístění, z jednoho výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="04fa1-178">When hello execution finishes, we'll have successfully created 10 predictive web service endpoints, each containing a trained model uniquely trained on hello dataset specific tooa rental location, all from a single training experiment.</span></span> <span data-ttu-id="04fa1-179">tooverify, můžete zkusit volání tyto koncové body pomocí hello **InvokeAmlWebServiceRRSEndpoint** rutinu a poskytovat jim hello stejné vstupní data a vzhledem k tomu, že jsou hello modely, které byste měli očekávat toosee různých předpovědi výsledky cvičení s jinou školení sad.</span><span class="sxs-lookup"><span data-stu-id="04fa1-179">tooverify this, you can try calling these endpoints using hello **InvokeAmlWebServiceRRSEndpoint** cmdlet, providing them with hello same input data, and you should expect toosee different prediction results since hello models are trained with different training sets.</span></span>

## <a name="full-powershell-script"></a><span data-ttu-id="04fa1-180">Úplné skript prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="04fa1-180">Full PowerShell script</span></span>
<span data-ttu-id="04fa1-181">Tady je seznam hello hello úplný zdrojový kód:</span><span class="sxs-lookup"><span data-stu-id="04fa1-181">Here's hello listing of hello full source code:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and properly set toopoint toohello valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on hello scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke hello retraining API 10 times tooproduce 10 regression models in .ilearner format
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

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
