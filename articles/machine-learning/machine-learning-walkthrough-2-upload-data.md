---
title: "Krok 2: Nahrání dat do experimentu Machine Learning | Microsoft Docs"
description: "Krok 2 hello vývoj prediktivního řešení návod: nahrávání veřejná data uložena do Azure Machine Learning Studio."
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
ms.openlocfilehash: 0ea21dcca2d0934ed06508560cf85cf31b48ce6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a><span data-ttu-id="d6628-103">Krok 2 průvodce: Nahrání stávajících dat do experimentu služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d6628-103">Walkthrough Step 2: Upload existing data into an Azure Machine Learning experiment</span></span>
<span data-ttu-id="d6628-104">Toto je druhý krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="d6628-104">This is hello second step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="d6628-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d6628-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. <span data-ttu-id="d6628-106">**Nahrání existujících dat**</span><span class="sxs-lookup"><span data-stu-id="d6628-106">**Upload existing data**</span></span>
3. [<span data-ttu-id="d6628-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="d6628-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="d6628-108">Natrénování a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="d6628-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="d6628-109">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="d6628-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="d6628-110">Přístup k webové službě hello</span><span class="sxs-lookup"><span data-stu-id="d6628-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="d6628-111">toodevelop prediktivního modelu pro úvěrové riziko, potřebujeme data, která jsme použít tootrain a pak hello model testů.</span><span class="sxs-lookup"><span data-stu-id="d6628-111">toodevelop a predictive model for credit risk, we need data that we can use tootrain and then test hello model.</span></span> <span data-ttu-id="d6628-112">V tomto návodu použijeme hello "UCI Statlog (němčině platební Data) Data Set" z úložiště hello UC Irvine Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d6628-112">For this walkthrough, we'll use hello "UCI Statlog (German Credit Data) Data Set" from hello UC Irvine Machine Learning repository.</span></span> <span data-ttu-id="d6628-113">Najdete ho tady:</span><span class="sxs-lookup"><span data-stu-id="d6628-113">You can find it here:</span></span>  
<span data-ttu-id="d6628-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ICS.uci.edu/ml/datasets/Statlog+(German+Credit+data)</a></span><span class="sxs-lookup"><span data-stu-id="d6628-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a></span></span>

<span data-ttu-id="d6628-115">Použijeme hello soubor s názvem **german.data**.</span><span class="sxs-lookup"><span data-stu-id="d6628-115">We'll use hello file named **german.data**.</span></span> <span data-ttu-id="d6628-116">Stáhněte si tento soubor tooyour místní pevný disk.</span><span class="sxs-lookup"><span data-stu-id="d6628-116">Download this file tooyour local hard drive.</span></span>  

<span data-ttu-id="d6628-117">Hello **german.data** datová sada obsahuje řádky 20 proměnných pro 1000 posledních žadatel o platební.</span><span class="sxs-lookup"><span data-stu-id="d6628-117">hello **german.data** dataset contains rows of 20 variables for 1000 past applicants for credit.</span></span> <span data-ttu-id="d6628-118">Tyto proměnné 20 představují hello dataset sadu funkcí (hello *funkce vector*), který nabízí charakteristiky identifikace každý platební žadatel.</span><span class="sxs-lookup"><span data-stu-id="d6628-118">These 20 variables represent hello dataset's set of features (hello *feature vector*), which provides identifying characteristics for each credit applicant.</span></span> <span data-ttu-id="d6628-119">Sloupec v jednotlivých řádcích představuje hello uchazeče počítané úvěrové riziko, s 700 uchazeči označený jako nízkou úvěrové riziko a 300 vysokým rizikem.</span><span class="sxs-lookup"><span data-stu-id="d6628-119">An additional column in each row represents hello applicant's calculated credit risk, with 700 applicants identified as a low credit risk and 300 as a high risk.</span></span>

<span data-ttu-id="d6628-120">Hello UCI web obsahuje popis hello atributů hello vektor funkce pro tato data.</span><span class="sxs-lookup"><span data-stu-id="d6628-120">hello UCI website provides a description of hello attributes of hello feature vector for this data.</span></span> <span data-ttu-id="d6628-121">To zahrnuje finanční informace, platební historii, stav zaměstnání a osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="d6628-121">This includes financial information, credit history, employment status, and personal information.</span></span> <span data-ttu-id="d6628-122">Pro každý žadatel binární hodnocení byl daný, která určuje, jestli jsou na nízkou nebo vysokou úvěrového rizika.</span><span class="sxs-lookup"><span data-stu-id="d6628-122">For each applicant, a binary rating has been given indicating whether they are a low or high credit risk.</span></span> 

<span data-ttu-id="d6628-123">Použijeme tato data tootrain model prediktivní analýzy.</span><span class="sxs-lookup"><span data-stu-id="d6628-123">We'll use this data tootrain a predictive analytics model.</span></span> <span data-ttu-id="d6628-124">Když jsme hotovi, našeho modelu by měl být schopný tooaccept funkce vector pro nové jednotlivé a předvídání, zda je nízkou nebo vysokou úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6628-124">When we're done, our model should be able tooaccept a feature vector for a new individual and predict whether he or she is a low or high credit risk.</span></span>  

<span data-ttu-id="d6628-125">Zde je zajímavé Točitost.</span><span class="sxs-lookup"><span data-stu-id="d6628-125">Here's an interesting twist.</span></span> <span data-ttu-id="d6628-126">Hello popis hello datovou sadu na hello UCI webu zmínkami nákladů Pokud jsme misclassify osoby úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6628-126">hello description of hello dataset on hello UCI website mentions what it costs if we misclassify a person's credit risk.</span></span>
<span data-ttu-id="d6628-127">Pokud hello model předpovídá vysoké úvěrové riziko pro uživatele, který je ve skutečnosti nízkou úvěrové riziko, udělal hello modelu chybnou klasifikaci.</span><span class="sxs-lookup"><span data-stu-id="d6628-127">If hello model predicts a high credit risk for someone who is actually a low credit risk, hello model has made a misclassification.</span></span>
<span data-ttu-id="d6628-128">Ale zpětné chybnou hello je pětkrát dražší toohello finanční instituce: Pokud hello model předpovídá nízkou úvěrové riziko pro uživatele, který je ve skutečnosti vysoké úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6628-128">But hello reverse misclassification is five times more costly toohello financial institution: if hello model predicts a low credit risk for someone who is actually a high credit risk.</span></span>

<span data-ttu-id="d6628-129">Ano chceme tootrain našeho modelu tak, aby hello náklady na tento druhý typ chybnou pětkrát vyšší než misclassifying hello jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="d6628-129">So, we want tootrain our model so that hello cost of this latter type of misclassification is five times higher than misclassifying hello other way.</span></span>
<span data-ttu-id="d6628-130">Jeden toodo jednoduchý způsob, jak to při tréninku modelu hello v našem experimentu je duplikováním (pětkrát) ty položky, které představují někdo s vysokou úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6628-130">One simple way toodo this when training hello model in our experiment is by duplicating (five times) those entries that represent someone with a high credit risk.</span></span> <span data-ttu-id="d6628-131">Poté Pokud hello model misclassifies někdo jako nízkou úvěrové riziko, pokud jsou ve skutečnosti vysoce rizikové, hello model nemá tento stejný chybnou pětkrát, jednou pro každou duplicitní.</span><span class="sxs-lookup"><span data-stu-id="d6628-131">Then, if hello model misclassifies someone as a low credit risk when they're actually a high risk, hello model does that same misclassification five times, once for each duplicate.</span></span> <span data-ttu-id="d6628-132">Tím se zvýší náklady na hello této chyby ve výsledcích školení hello.</span><span class="sxs-lookup"><span data-stu-id="d6628-132">This will increase hello cost of this error in hello training results.</span></span>


## <a name="convert-hello-dataset-format"></a><span data-ttu-id="d6628-133">Převést formát hello datové sady</span><span class="sxs-lookup"><span data-stu-id="d6628-133">Convert hello dataset format</span></span>
<span data-ttu-id="d6628-134">původní datové sady Hello používá formát oddělené prázdné.</span><span class="sxs-lookup"><span data-stu-id="d6628-134">hello original dataset uses a blank-separated format.</span></span> <span data-ttu-id="d6628-135">Machine Learning Studio funguje lépe se soubor s oddělovači (CSV), proto jsme budete převést datovou sadu hello nahrazením mezer čárkami.</span><span class="sxs-lookup"><span data-stu-id="d6628-135">Machine Learning Studio works better with a comma-separated value (CSV) file, so we'll convert hello dataset by replacing spaces with commas.</span></span>  

<span data-ttu-id="d6628-136">Existuje mnoho způsobů tooconvert tato data.</span><span class="sxs-lookup"><span data-stu-id="d6628-136">There are many ways tooconvert this data.</span></span> <span data-ttu-id="d6628-137">Jedním ze způsobů je pomocí hello následující příkaz prostředí Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="d6628-137">One way is by using hello following Windows PowerShell command:</span></span>   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

<span data-ttu-id="d6628-138">Dalším způsobem je pomocí příkazu menšit hello Unix:</span><span class="sxs-lookup"><span data-stu-id="d6628-138">Another way is by using hello Unix sed command:</span></span>  

    sed 's/ /,/g' german.data > german.csv  

<span data-ttu-id="d6628-139">V obou případech jsme vytvořili textový soubor s oddělovači verzi hello data do souboru s názvem **german.csv** , jsme můžete použít v našem experimentu.</span><span class="sxs-lookup"><span data-stu-id="d6628-139">In either case, we have created a comma-separated version of hello data in a file named **german.csv** that we can use in our experiment.</span></span>

## <a name="upload-hello-dataset-toomachine-learning-studio"></a><span data-ttu-id="d6628-140">Nahrát hello datovou sadu tooMachine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="d6628-140">Upload hello dataset tooMachine Learning Studio</span></span>
<span data-ttu-id="d6628-141">Jakmile hello data byla převedená tooCSV formátu, potřebujeme tooupload ji do nástroje Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="d6628-141">Once hello data has been converted tooCSV format, we need tooupload it into Machine Learning Studio.</span></span> 

1. <span data-ttu-id="d6628-142">Domovská stránka Machine Learning Studio otevřete hello ([https://studio.azureml.net](https://studio.azureml.net)).</span><span class="sxs-lookup"><span data-stu-id="d6628-142">Open hello Machine Learning Studio home page ([https://studio.azureml.net](https://studio.azureml.net)).</span></span> 

2. <span data-ttu-id="d6628-143">V nabídce hello ![nabídky][1] v levém horním rohu hello hello okna, klikněte na **Azure Machine Learning**, vyberte **Studio**a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="d6628-143">Click hello menu ![Menu][1] in hello upper-left corner of hello window, click **Azure Machine Learning**, select **Studio**, and sign in.</span></span>

3. <span data-ttu-id="d6628-144">Klikněte na tlačítko **+ nový** v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="d6628-144">Click **+NEW** at hello bottom of hello window.</span></span>

4. <span data-ttu-id="d6628-145">Vyberte **datovou sadu**.</span><span class="sxs-lookup"><span data-stu-id="d6628-145">Select **DATASET**.</span></span>

5. <span data-ttu-id="d6628-146">Vyberte **z místního souboru**.</span><span class="sxs-lookup"><span data-stu-id="d6628-146">Select **FROM LOCAL FILE**.</span></span>

    ![Přidejte datovou sadu, z místního souboru][2]

6. <span data-ttu-id="d6628-148">V hello **nahrát nová datová sada** dialogové okno, klikněte na tlačítko **Procházet** a najde hello **german.csv** souborů, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d6628-148">In hello **Upload a new dataset** dialog, click **Browse** and find hello **german.csv** file you created.</span></span>

7. <span data-ttu-id="d6628-149">Zadejte název pro datovou sadu hello.</span><span class="sxs-lookup"><span data-stu-id="d6628-149">Enter a name for hello dataset.</span></span> <span data-ttu-id="d6628-150">V tomto návodu volání ji "UCI němčina platební karty Data".</span><span class="sxs-lookup"><span data-stu-id="d6628-150">For this walkthrough, call it "UCI German Credit Card Data".</span></span>

8. <span data-ttu-id="d6628-151">Datový typ, vyberte **obecné soubor CSV neobsahuje záhlaví (. nh.csv)**.</span><span class="sxs-lookup"><span data-stu-id="d6628-151">For data type, select **Generic CSV File With no header (.nh.csv)**.</span></span>

9. <span data-ttu-id="d6628-152">Pokud chcete přidáte popis.</span><span class="sxs-lookup"><span data-stu-id="d6628-152">Add a description if you’d like.</span></span>

10. <span data-ttu-id="d6628-153">Klikněte na tlačítko hello **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="d6628-153">Click hello **OK** check mark.</span></span>  

    ![Nahrát hello datové sady][3]

<span data-ttu-id="d6628-155">Tato hello data ukládání do datové sady modul, který používáme v experimentu.</span><span class="sxs-lookup"><span data-stu-id="d6628-155">This uploads hello data into a dataset module that we can use in an experiment.</span></span>

<span data-ttu-id="d6628-156">Můžete spravovat datové sady, že jste odeslali tooStudio kliknutím hello **datové sady** levé kartě toohello okna Studio hello.</span><span class="sxs-lookup"><span data-stu-id="d6628-156">You can manage datasets that you've uploaded tooStudio by clicking hello **DATASETS** tab toohello left of hello Studio window.</span></span>

![Spravovat datové sady][4]

<span data-ttu-id="d6628-158">Další informace o importu dalších typů dat do experimentu najdete v tématu [importu trénovacích dat do Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span><span class="sxs-lookup"><span data-stu-id="d6628-158">For more information about importing other types of data into an experiment, see [Import your training data into Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span></span>

<span data-ttu-id="d6628-159">**Další krok: [vytvoření nového experimentu](machine-learning-walkthrough-3-create-new-experiment.md)**</span><span class="sxs-lookup"><span data-stu-id="d6628-159">**Next: [Create a new experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span></span>

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
