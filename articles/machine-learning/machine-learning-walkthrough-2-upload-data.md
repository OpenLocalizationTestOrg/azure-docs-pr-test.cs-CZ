---
title: "Krok 2: Nahrání dat do experimentu Machine Learning | Microsoft Docs"
description: "Vývoj prediktivního řešení návod krok 2: nahrání veřejná data uložena do Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: c2ab5f5252e1ea1ec51f6c3bd489826c70ff011c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a><span data-ttu-id="381ab-103">Krok 2 průvodce: Nahrání stávajících dat do experimentu služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="381ab-103">Walkthrough Step 2: Upload existing data into an Azure Machine Learning experiment</span></span>
<span data-ttu-id="381ab-104">Toto je druhý krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="381ab-104">This is the second step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="381ab-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="381ab-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. <span data-ttu-id="381ab-106">**Nahrání existujících dat**</span><span class="sxs-lookup"><span data-stu-id="381ab-106">**Upload existing data**</span></span>
3. [<span data-ttu-id="381ab-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="381ab-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="381ab-108">Natrénování a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="381ab-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="381ab-109">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="381ab-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="381ab-110">Přístup k webové službě</span><span class="sxs-lookup"><span data-stu-id="381ab-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="381ab-111">K vývoji prediktivního modelu pro úvěrové riziko, potřebujeme data, která jsme můžete použít k trénování a pak model otestujeme.</span><span class="sxs-lookup"><span data-stu-id="381ab-111">To develop a predictive model for credit risk, we need data that we can use to train and then test the model.</span></span> <span data-ttu-id="381ab-112">V tomto návodu použijeme "UCI Statlog (němčině platební Data) Data Set" z úložiště UC Irvine Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="381ab-112">For this walkthrough, we'll use the "UCI Statlog (German Credit Data) Data Set" from the UC Irvine Machine Learning repository.</span></span> <span data-ttu-id="381ab-113">Najdete ho tady:</span><span class="sxs-lookup"><span data-stu-id="381ab-113">You can find it here:</span></span>  
<span data-ttu-id="381ab-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ICS.uci.edu/ml/datasets/Statlog+(German+Credit+data)</a></span><span class="sxs-lookup"><span data-stu-id="381ab-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a></span></span>

<span data-ttu-id="381ab-115">Použijeme soubor s názvem **german.data**.</span><span class="sxs-lookup"><span data-stu-id="381ab-115">We'll use the file named **german.data**.</span></span> <span data-ttu-id="381ab-116">Stáhněte si tento soubor na místní pevný disk.</span><span class="sxs-lookup"><span data-stu-id="381ab-116">Download this file to your local hard drive.</span></span>  

<span data-ttu-id="381ab-117">**German.data** datová sada obsahuje řádky 20 proměnných pro 1000 posledních žadatel o platební.</span><span class="sxs-lookup"><span data-stu-id="381ab-117">The **german.data** dataset contains rows of 20 variables for 1000 past applicants for credit.</span></span> <span data-ttu-id="381ab-118">Tyto proměnné 20 představují datová sada je sada funkcí ( *funkce vector*), který nabízí charakteristiky identifikace každý platební žadatel.</span><span class="sxs-lookup"><span data-stu-id="381ab-118">These 20 variables represent the dataset's set of features (the *feature vector*), which provides identifying characteristics for each credit applicant.</span></span> <span data-ttu-id="381ab-119">Sloupec v jednotlivých řádcích představuje žadatele počítané úvěrové riziko, s 700 uchazeči označený jako nízkou úvěrové riziko a 300 vysokým rizikem.</span><span class="sxs-lookup"><span data-stu-id="381ab-119">An additional column in each row represents the applicant's calculated credit risk, with 700 applicants identified as a low credit risk and 300 as a high risk.</span></span>

<span data-ttu-id="381ab-120">Web UCI obsahuje popis atributů vektoru funkce pro tato data.</span><span class="sxs-lookup"><span data-stu-id="381ab-120">The UCI website provides a description of the attributes of the feature vector for this data.</span></span> <span data-ttu-id="381ab-121">To zahrnuje finanční informace, platební historii, stav zaměstnání a osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="381ab-121">This includes financial information, credit history, employment status, and personal information.</span></span> <span data-ttu-id="381ab-122">Pro každý žadatel binární hodnocení byl daný, která určuje, jestli jsou na nízkou nebo vysokou úvěrového rizika.</span><span class="sxs-lookup"><span data-stu-id="381ab-122">For each applicant, a binary rating has been given indicating whether they are a low or high credit risk.</span></span> 

<span data-ttu-id="381ab-123">Tato data použijeme pro trénování modelu prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="381ab-123">We'll use this data to train a predictive analytics model.</span></span> <span data-ttu-id="381ab-124">Když jsme hotovi, našeho modelu byste měli mít přijmout funkce vector pro nové jednotlivé a předvídání, zda je nízkou nebo vysokou úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="381ab-124">When we're done, our model should be able to accept a feature vector for a new individual and predict whether he or she is a low or high credit risk.</span></span>  

<span data-ttu-id="381ab-125">Zde je zajímavé Točitost.</span><span class="sxs-lookup"><span data-stu-id="381ab-125">Here's an interesting twist.</span></span> <span data-ttu-id="381ab-126">Popis sady dat na webu UCI uvádí, co ji stojí Pokud jsme misclassify osoby úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="381ab-126">The description of the dataset on the UCI website mentions what it costs if we misclassify a person's credit risk.</span></span>
<span data-ttu-id="381ab-127">Pokud model předpovídá vysoké úvěrové riziko pro uživatele, který je ve skutečnosti nízkou úvěrové riziko, model chybnou udělal.</span><span class="sxs-lookup"><span data-stu-id="381ab-127">If the model predicts a high credit risk for someone who is actually a low credit risk, the model has made a misclassification.</span></span>
<span data-ttu-id="381ab-128">Ale zpětné chybnou klasifikaci je pětkrát nákladnější finanční instituce: Pokud model předpovídá nízkou úvěrové riziko pro uživatele, který je ve skutečnosti vysoké úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="381ab-128">But the reverse misclassification is five times more costly to the financial institution: if the model predicts a low credit risk for someone who is actually a high credit risk.</span></span>

<span data-ttu-id="381ab-129">Ano chceme cvičení našeho modelu tak, aby náklady na tento druhý typ chybnou pětkrát vyšší než misclassifying jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="381ab-129">So, we want to train our model so that the cost of this latter type of misclassification is five times higher than misclassifying the other way.</span></span>
<span data-ttu-id="381ab-130">Jeden způsob, jak to provést při tréninku modelu v našem experimentu je duplikování (pětkrát) ty položky, které představují někdo s vysokou úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="381ab-130">One simple way to do this when training the model in our experiment is by duplicating (five times) those entries that represent someone with a high credit risk.</span></span> <span data-ttu-id="381ab-131">Poté Pokud model misclassifies někdo jako nízkou úvěrové riziko, pokud jsou ve skutečnosti vysoce rizikové, model nemá tento stejný chybnou pětkrát, jednou pro každou duplicitní.</span><span class="sxs-lookup"><span data-stu-id="381ab-131">Then, if the model misclassifies someone as a low credit risk when they're actually a high risk, the model does that same misclassification five times, once for each duplicate.</span></span> <span data-ttu-id="381ab-132">Tím se zvýší náklady na této chyby ve výsledcích školení.</span><span class="sxs-lookup"><span data-stu-id="381ab-132">This will increase the cost of this error in the training results.</span></span>


## <a name="convert-the-dataset-format"></a><span data-ttu-id="381ab-133">Převést formát datové sady</span><span class="sxs-lookup"><span data-stu-id="381ab-133">Convert the dataset format</span></span>
<span data-ttu-id="381ab-134">Původní datové sady používá formát oddělené prázdné.</span><span class="sxs-lookup"><span data-stu-id="381ab-134">The original dataset uses a blank-separated format.</span></span> <span data-ttu-id="381ab-135">Machine Learning Studio funguje lépe se soubor s oddělovači (CSV), proto jsme budete převést datovou sadu nahrazením mezer čárkami.</span><span class="sxs-lookup"><span data-stu-id="381ab-135">Machine Learning Studio works better with a comma-separated value (CSV) file, so we'll convert the dataset by replacing spaces with commas.</span></span>  

<span data-ttu-id="381ab-136">Existuje mnoho způsobů, jak převést tato data.</span><span class="sxs-lookup"><span data-stu-id="381ab-136">There are many ways to convert this data.</span></span> <span data-ttu-id="381ab-137">Jedním ze způsobů je pomocí následujícího příkazu prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="381ab-137">One way is by using the following Windows PowerShell command:</span></span>   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

<span data-ttu-id="381ab-138">Dalším způsobem je pomocí příkazu menšit Unix:</span><span class="sxs-lookup"><span data-stu-id="381ab-138">Another way is by using the Unix sed command:</span></span>  

    sed 's/ /,/g' german.data > german.csv  

<span data-ttu-id="381ab-139">V obou případech jsme vytvořili textový soubor s oddělovači verzi dat do souboru s názvem **german.csv** , jsme můžete použít v našem experimentu.</span><span class="sxs-lookup"><span data-stu-id="381ab-139">In either case, we have created a comma-separated version of the data in a file named **german.csv** that we can use in our experiment.</span></span>

## <a name="upload-the-dataset-to-machine-learning-studio"></a><span data-ttu-id="381ab-140">Nahrání datové sady do nástroje Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="381ab-140">Upload the dataset to Machine Learning Studio</span></span>
<span data-ttu-id="381ab-141">Jakmile data byl převeden do formátu CSV, musíme nahrajte ho do nástroje Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="381ab-141">Once the data has been converted to CSV format, we need to upload it into Machine Learning Studio.</span></span> 

1. <span data-ttu-id="381ab-142">Otevřete domovskou stránku Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)).</span><span class="sxs-lookup"><span data-stu-id="381ab-142">Open the Machine Learning Studio home page ([https://studio.azureml.net](https://studio.azureml.net)).</span></span> 

2. <span data-ttu-id="381ab-143">Klikněte na nabídku ![nabídky][1] v levém horním rohu okna, klikněte na **Azure Machine Learning**, vyberte **Studio**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="381ab-143">Click the menu ![Menu][1] in the upper-left corner of the window, click **Azure Machine Learning**, select **Studio**, and sign in.</span></span>

3. <span data-ttu-id="381ab-144">Klikněte na tlačítko **+ nový** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="381ab-144">Click **+NEW** at the bottom of the window.</span></span>

4. <span data-ttu-id="381ab-145">Vyberte **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="381ab-145">Select **DATASET**.</span></span>

5. <span data-ttu-id="381ab-146">Vyberte **z místního souboru**.</span><span class="sxs-lookup"><span data-stu-id="381ab-146">Select **FROM LOCAL FILE**.</span></span>

    ![Přidejte datovou sadu, z místního souboru][2]

6. <span data-ttu-id="381ab-148">V **nahrát nová datová sada** dialogové okno, klikněte na tlačítko **Procházet** a najděte **german.csv** souborů, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="381ab-148">In the **Upload a new dataset** dialog, click **Browse** and find the **german.csv** file you created.</span></span>

7. <span data-ttu-id="381ab-149">Zadejte název pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="381ab-149">Enter a name for the dataset.</span></span> <span data-ttu-id="381ab-150">V tomto návodu volání ji "UCI němčina platební karty Data".</span><span class="sxs-lookup"><span data-stu-id="381ab-150">For this walkthrough, call it "UCI German Credit Card Data".</span></span>

8. <span data-ttu-id="381ab-151">Datový typ, vyberte **obecné soubor CSV neobsahuje záhlaví (. nh.csv)**.</span><span class="sxs-lookup"><span data-stu-id="381ab-151">For data type, select **Generic CSV File With no header (.nh.csv)**.</span></span>

9. <span data-ttu-id="381ab-152">Pokud chcete přidáte popis.</span><span class="sxs-lookup"><span data-stu-id="381ab-152">Add a description if you’d like.</span></span>

10. <span data-ttu-id="381ab-153">Klikněte **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="381ab-153">Click the **OK** check mark.</span></span>  

    ![Nahrání datové sady][3]

<span data-ttu-id="381ab-155">To nahrává data do datové sady modul, který používáme v experimentu.</span><span class="sxs-lookup"><span data-stu-id="381ab-155">This uploads the data into a dataset module that we can use in an experiment.</span></span>

<span data-ttu-id="381ab-156">Můžete spravovat datové sady, které jste odeslali do nástroje Studio kliknutím **datové sady** karty na levé straně okna Studio.</span><span class="sxs-lookup"><span data-stu-id="381ab-156">You can manage datasets that you've uploaded to Studio by clicking the **DATASETS** tab to the left of the Studio window.</span></span>

![Spravovat datové sady][4]

<span data-ttu-id="381ab-158">Další informace o importu dalších typů dat do experimentu najdete v tématu [importu trénovacích dat do Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span><span class="sxs-lookup"><span data-stu-id="381ab-158">For more information about importing other types of data into an experiment, see [Import your training data into Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span></span>

<span data-ttu-id="381ab-159">**Další krok: [vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)**</span><span class="sxs-lookup"><span data-stu-id="381ab-159">**Next: [Create a new experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span></span>

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
