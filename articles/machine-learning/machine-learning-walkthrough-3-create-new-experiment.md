---
title: "Krok 3: Vytvoření nového experimentu Machine Learning | Microsoft Docs"
description: "Krok 3 vývoj prediktivního řešení návod: vytvoření nového experimentu školení v Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: cd410316910bce76f5c915c06e83b24c034481b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a><span data-ttu-id="2a410-103">Krok 3 průvodce: Vytvoření nového experimentu služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2a410-103">Walkthrough Step 3: Create a new Azure Machine Learning experiment</span></span>
<span data-ttu-id="2a410-104">Toto je třetí krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="2a410-104">This is the third step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="2a410-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2a410-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="2a410-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="2a410-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. <span data-ttu-id="2a410-107">**Vytvoření nového experimentu**</span><span class="sxs-lookup"><span data-stu-id="2a410-107">**Create a new experiment**</span></span>
4. [<span data-ttu-id="2a410-108">Natrénování a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="2a410-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="2a410-109">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="2a410-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="2a410-110">Přístup k webové službě</span><span class="sxs-lookup"><span data-stu-id="2a410-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="2a410-111">Dalším krokem v tomto návodu je vytvoření experimentu v Machine Learning Studio používající datové sady, které jsme nahrát.</span><span class="sxs-lookup"><span data-stu-id="2a410-111">The next step in this walkthrough is to create an experiment in Machine Learning Studio that uses the dataset we uploaded.</span></span>  

1. <span data-ttu-id="2a410-112">V nástroji Studio klikněte na tlačítko **+ nový** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="2a410-112">In Studio, click **+NEW** at the bottom of the window.</span></span>
2. <span data-ttu-id="2a410-113">Vyberte **EXPERIMENTU**a potom vyberte "Prázdný Experiment".</span><span class="sxs-lookup"><span data-stu-id="2a410-113">Select **EXPERIMENT**, and then select "Blank Experiment".</span></span> 

    ![Vytvoření nového experimentu][0]

2. <span data-ttu-id="2a410-115">Vyberte výchozí název v horní části na plátno experimentu a přejmenujte jej na něco smysluplného.</span><span class="sxs-lookup"><span data-stu-id="2a410-115">Select the default experiment name at the top of the canvas and rename it to something meaningful.</span></span>

    ![Přejmenujte experimentu][5]
   
   > [!TIP]
   > <span data-ttu-id="2a410-117">Je dobrým zvykem vyplnit **Souhrn** a **popis** experimentu v **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="2a410-117">It's a good practice to fill in **Summary** and **Description** for the experiment in the **Properties** pane.</span></span> <span data-ttu-id="2a410-118">Tyto vlastnosti získáte možnost dokumentu experimentu tak, aby každý, kdo ho později zabývá pochopili záměry a metody.</span><span class="sxs-lookup"><span data-stu-id="2a410-118">These properties give you the chance to document the experiment so that anyone who looks at it later will understand your goals and methodology.</span></span>
   > 
   > ![Vlastnosti experimentu][6]
   > 
3. <span data-ttu-id="2a410-120">Paletě modulů nalevo od plátna experimentu rozbalte **uložit datové sady**.</span><span class="sxs-lookup"><span data-stu-id="2a410-120">In the module palette to the left of the experiment canvas, expand **Saved Datasets**.</span></span>
4. <span data-ttu-id="2a410-121">Vyhledá se datová jste vytvořili v části **Moje datové sady** a přetáhněte na plátno.</span><span class="sxs-lookup"><span data-stu-id="2a410-121">Find the dataset you created under **My Datasets** and drag it onto the canvas.</span></span> <span data-ttu-id="2a410-122">Datovou sadu můžete také vyhledat zadáním názvu v **vyhledávání** pole výše paletě.</span><span class="sxs-lookup"><span data-stu-id="2a410-122">You can also find the dataset by entering the name in the **Search** box above the palette.</span></span>  

    ![Přidejte datovou sadu experimentu][7]

## <a name="prepare-the-data"></a><span data-ttu-id="2a410-124">Příprava dat</span><span class="sxs-lookup"><span data-stu-id="2a410-124">Prepare the data</span></span>
<span data-ttu-id="2a410-125">Můžete zobrazit prvních 100 řádků dat a některé statistické informace pro celou datovou sadu: klikněte na výstupní port datové sady (přeškrtnutým kroužkem dole) a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="2a410-125">You can view the first 100 rows of the data and some statistical information for the whole dataset: Click the output port of the dataset (the small circle at the bottom) and select **Visualize**.</span></span>  

<span data-ttu-id="2a410-126">Protože datový soubor nebyl dodán záhlaví sloupců, Studio poskytl obecné záhlaví (Sloupec1, Sloupec2, *atd*).</span><span class="sxs-lookup"><span data-stu-id="2a410-126">Because the data file didn't come with column headings, Studio has provided generic headings (Col1, Col2, *etc.*).</span></span> <span data-ttu-id="2a410-127">Dobrý záhlaví nejsou nezbytně nutné, aby vytvoření modelu, ale jejich usnadnili práci s daty v rámci experimentu.</span><span class="sxs-lookup"><span data-stu-id="2a410-127">Good headings aren't essential to creating a model, but they make it easier to work with the data in the experiment.</span></span> <span data-ttu-id="2a410-128">Navíc pokud jsme nakonec publikovat tento model ve webové službě, záhlaví pomoci při identifikaci sloupce, které chcete uživatele služby.</span><span class="sxs-lookup"><span data-stu-id="2a410-128">Also, when we eventually publish this model in a web service, the headings help identify the columns to the user of the service.</span></span>  

<span data-ttu-id="2a410-129">Přidáme záhlaví sloupců pomocí [upravit Metadata] [ edit-metadata] modulu.</span><span class="sxs-lookup"><span data-stu-id="2a410-129">We can add column headings using the [Edit Metadata][edit-metadata] module.</span></span>
<span data-ttu-id="2a410-130">Můžete použít [upravit Metadata] [ edit-metadata] modulu změnit metadata spojená s datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="2a410-130">You use the [Edit Metadata][edit-metadata] module to change metadata associated with a dataset.</span></span> <span data-ttu-id="2a410-131">V takovém případě jsme můžete zadat další popisné názvy záhlaví sloupců.</span><span class="sxs-lookup"><span data-stu-id="2a410-131">In this case, we use it to provide more friendly names for column headings.</span></span> 

<span data-ttu-id="2a410-132">Chcete-li použít [upravit Metadata][edit-metadata], nejprve zadejte sloupce, které chcete upravit (v tomto případě všechny z nich.) Potom zadejte akci prováděnou na tyto sloupce (v tomto případě změna záhlaví sloupců.)</span><span class="sxs-lookup"><span data-stu-id="2a410-132">To use [Edit Metadata][edit-metadata], you first specify which columns to modify (in this case, all of them.) Next, you specify the action to be performed on those columns (in this case, changing column headings.)</span></span>

1. <span data-ttu-id="2a410-133">Paletě modulů, zadejte "metadata" **vyhledávání** pole.</span><span class="sxs-lookup"><span data-stu-id="2a410-133">In the module palette, type "metadata" in the **Search** box.</span></span> <span data-ttu-id="2a410-134">[Upravit Metadata] [ edit-metadata] se zobrazí v seznamu modulů.</span><span class="sxs-lookup"><span data-stu-id="2a410-134">The [Edit Metadata][edit-metadata] appears in the module list.</span></span>

2. <span data-ttu-id="2a410-135">Klikněte na tlačítko a přetáhněte ji [upravit Metadata] [ edit-metadata] modulu na plátno a umístěte jej pod datovou sadu jsme přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="2a410-135">Click and drag the [Edit Metadata][edit-metadata] module onto the canvas and drop it below the dataset we added earlier.</span></span>

3. <span data-ttu-id="2a410-136">Připojit datovou sadu, která [upravit Metadata][edit-metadata]: klikněte na výstupní port datové sady (přeškrtnutým kroužkem v dolní části datové sady), přetáhněte do vstupní port [upravit Metadata] [ edit-metadata] (přeškrtnutým kroužkem v horní části modulu), pak uvolnění tlačítka myši.</span><span class="sxs-lookup"><span data-stu-id="2a410-136">Connect the dataset to the [Edit Metadata][edit-metadata]: click the output port of the dataset (the small circle at the bottom of the dataset), drag to the input port of [Edit Metadata][edit-metadata] (the small circle at the top of the module), then release the mouse button.</span></span> <span data-ttu-id="2a410-137">Datovou sadu a modul zůstanou připojená, i když přesouváte buď na plátně.</span><span class="sxs-lookup"><span data-stu-id="2a410-137">The dataset and module remain connected even if you move either around on the canvas.</span></span>
   
   <span data-ttu-id="2a410-138">Experiment by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="2a410-138">The experiment should now look something like this:</span></span>  
   
   ![Přidání metadat úpravy][1]
   
   <span data-ttu-id="2a410-140">Červený vykřičník označuje, že jsme nebyly dosud nastavit vlastnosti pro tento modul.</span><span class="sxs-lookup"><span data-stu-id="2a410-140">The red exclamation mark indicates that we haven't set the properties for this module yet.</span></span> <span data-ttu-id="2a410-141">Provedeme tento další.</span><span class="sxs-lookup"><span data-stu-id="2a410-141">We'll do that next.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="2a410-142">Kliknutím dvakrát na modul a zadáním textu je možné přidat k modulu komentář.</span><span class="sxs-lookup"><span data-stu-id="2a410-142">You can add a comment to a module by double-clicking the module and entering text.</span></span> <span data-ttu-id="2a410-143">To vám může pomoci rychle poznat, jaký je účel modulu v experimentu.</span><span class="sxs-lookup"><span data-stu-id="2a410-143">This can help you see at a glance what the module is doing in your experiment.</span></span> <span data-ttu-id="2a410-144">V takovém případě dvakrát klikněte [upravit Metadata] [ edit-metadata] modul a zadejte komentář "přidat záhlaví sloupců".</span><span class="sxs-lookup"><span data-stu-id="2a410-144">In this case, double-click the [Edit Metadata][edit-metadata] module and type the comment "Add column headings".</span></span> <span data-ttu-id="2a410-145">Klikněte na tlačítko kdekoliv jinde na plátně zavřete textového pole.</span><span class="sxs-lookup"><span data-stu-id="2a410-145">Click anywhere else on the canvas to close the text box.</span></span> <span data-ttu-id="2a410-146">Pokud chcete zobrazit komentář, klikněte na šipku dolů na modul.</span><span class="sxs-lookup"><span data-stu-id="2a410-146">To display the comment, click the down-arrow on the module.</span></span>
   > 
   > ![Upravit komentář přidat Metadata modulu][8]
   > 
4. <span data-ttu-id="2a410-148">Vyberte [upravit Metadata][edit-metadata]a v **vlastnosti** napravo od plátna, klikněte na tlačítko **spustit selektor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="2a410-148">Select [Edit Metadata][edit-metadata], and in the **Properties** pane to the right of the canvas, click **Launch column selector**.</span></span>

5. <span data-ttu-id="2a410-149">V **vyberte sloupce** dialogovém okně, vyberte všechny řádky v **dostupné sloupce** a klikněte na tlačítko > přesunout jejich **vybrané sloupce**.</span><span class="sxs-lookup"><span data-stu-id="2a410-149">In the **Select columns** dialog, select all the rows in **Available Columns** and click > to move them to **Selected Columns**.</span></span>
   <span data-ttu-id="2a410-150">Dialogové okno by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="2a410-150">The dialog should look like this:</span></span>

   ![Selektor sloupců s vybrané všechny sloupce][2]

6. <span data-ttu-id="2a410-152">Klikněte **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="2a410-152">Click the **OK** check mark.</span></span>

7. <span data-ttu-id="2a410-153">Zpět v **vlastnosti** podokně, podívejte se **nové názvy sloupců** parametr.</span><span class="sxs-lookup"><span data-stu-id="2a410-153">Back in the **Properties** pane, look for the **New column names** parameter.</span></span> <span data-ttu-id="2a410-154">V tomto poli zadejte seznam názvů pro 21 sloupců v datové sadě, oddělených čárkami a pořadí sloupců.</span><span class="sxs-lookup"><span data-stu-id="2a410-154">In this field, enter a list of names for the 21 columns in the dataset, separated by commas and in column order.</span></span> <span data-ttu-id="2a410-155">Názvy sloupců můžete získat z dokumentace datové sady na webu UCI nebo ke zvýšení pohodlí můžete zkopírovat a vložit v následujícím seznamu:</span><span class="sxs-lookup"><span data-stu-id="2a410-155">You can obtain the columns names from the dataset documentation on the UCI website, or for convenience you can copy and paste the following list:</span></span>  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   <span data-ttu-id="2a410-156">V podokně Vlastnosti vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2a410-156">The Properties pane looks like this:</span></span>
   
   ![Vlastnosti pro úpravy metadat][3]

> [!TIP]
> <span data-ttu-id="2a410-158">Pokud chcete ověřit záhlaví sloupců, spustit experiment (klikněte na tlačítko **spustit** níže na plátno experimentu).</span><span class="sxs-lookup"><span data-stu-id="2a410-158">If you want to verify the column headings, run the experiment (click **RUN** below the experiment canvas).</span></span> <span data-ttu-id="2a410-159">Po jeho dokončení spuštění (zelená značka zaškrtnutí se zobrazí na [upravit Metadata][edit-metadata]), klikněte na výstupní port modulu [upravit Metadata] [ edit-metadata] modulu a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="2a410-159">When it finishes running (a green check mark appears on [Edit Metadata][edit-metadata]), click the output port of the [Edit Metadata][edit-metadata] module, and select **Visualize**.</span></span> <span data-ttu-id="2a410-160">Zobrazí se výstup libovolný modul stejným způsobem, chcete-li zobrazit průběh dat přes experimentu.</span><span class="sxs-lookup"><span data-stu-id="2a410-160">You can view the output of any module in the same way to view the progress of the data through the experiment.</span></span>
> 
> 

## <a name="create-training-and-test-datasets"></a><span data-ttu-id="2a410-161">Vytvoření školení a testovací datové sady</span><span class="sxs-lookup"><span data-stu-id="2a410-161">Create training and test datasets</span></span>
<span data-ttu-id="2a410-162">Potřebujeme některá data pro trénování modelu a některé to vyzkoušíte.</span><span class="sxs-lookup"><span data-stu-id="2a410-162">We need some data to train the model and some to test it.</span></span>
<span data-ttu-id="2a410-163">Proto v dalším kroku experiment, jsme datovou sadu rozdělit na dvě samostatné datové sady: jeden pro cvičení našeho modelu a jeden pro testování ho.</span><span class="sxs-lookup"><span data-stu-id="2a410-163">So in the next step of the experiment, we split the dataset into two separate datasets: one for training our model and one for testing it.</span></span>

<span data-ttu-id="2a410-164">K tomuto účelu používáme [rozdělení dat] [ split] modulu.</span><span class="sxs-lookup"><span data-stu-id="2a410-164">To do this, we use the [Split Data][split] module.</span></span>  

1. <span data-ttu-id="2a410-165">Najít [rozdělení dat] [ split] modulu, přetáhněte na plátno a propojte jej s [upravit Metadata] [ edit-metadata] modulu.</span><span class="sxs-lookup"><span data-stu-id="2a410-165">Find the [Split Data][split] module, drag it onto the canvas, and connect it to the [Edit Metadata][edit-metadata] module.</span></span>

2. <span data-ttu-id="2a410-166">Výchozí hodnota je 0,5 poměr rozdělení a **Randomized rozdělení** parametr je nastaven.</span><span class="sxs-lookup"><span data-stu-id="2a410-166">By default, the split ratio is 0.5 and the **Randomized split** parameter is set.</span></span> <span data-ttu-id="2a410-167">Znamená, že náhodných polovina dat je výstup prostřednictvím jednoho portu [rozdělení dat] [ split] modulu a polovina prostřednictvím dalších.</span><span class="sxs-lookup"><span data-stu-id="2a410-167">This means that a random half of the data is output through one port of the [Split Data][split] module, and half through the other.</span></span> <span data-ttu-id="2a410-168">Můžete upravit tyto parametry společně s **náhodná počáteční hodnota** parametr, chcete-li změnit rozdělení mezi trénování a testování data.</span><span class="sxs-lookup"><span data-stu-id="2a410-168">You can adjust these parameters, as well as the **Random seed** parameter, to change the split between training and testing data.</span></span> <span data-ttu-id="2a410-169">V tomto příkladu jsme je jako ponechte-je.</span><span class="sxs-lookup"><span data-stu-id="2a410-169">For this example, we leave them as-is.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="2a410-170">Vlastnost **podíl řádků v první výstupní sadě dat** Určuje, kolik dat je výstup prostřednictvím *levém* výstupní port.</span><span class="sxs-lookup"><span data-stu-id="2a410-170">The property **Fraction of rows in the first output dataset** determines how much of the data is output through the *left* output port.</span></span> <span data-ttu-id="2a410-171">Například pokud nastavíte poměr 0,7, pak 70 % dat je výstup prostřednictvím levý port a z 30 % prostřednictvím pravý port.</span><span class="sxs-lookup"><span data-stu-id="2a410-171">For instance, if you set the ratio to 0.7, then 70% of the data is output through the left port and 30% through the right port.</span></span>  
   > 
   > 

3. <span data-ttu-id="2a410-172">Dvakrát klikněte [rozdělení dat] [ split] modul a zadejte komentář, "školení nebo testování dat rozdělení 50 %".</span><span class="sxs-lookup"><span data-stu-id="2a410-172">Double-click the [Split Data][split] module and enter the comment, "Training/testing data split 50%".</span></span> 

<span data-ttu-id="2a410-173">Můžeme použít výstupů [rozdělení dat] [ split] modulu jsme jako, ale můžeme se rozhodnete použít levý výstupní jako Cvičná data a právo výstupní data jako testování.</span><span class="sxs-lookup"><span data-stu-id="2a410-173">We can use the outputs of the [Split Data][split] module however we like, but let's choose to use the left output as training data and the right output as testing data.</span></span>  

<span data-ttu-id="2a410-174">Jak je uvedeno v [předchozího kroku](machine-learning-walkthrough-2-upload-data.md), je pětkrát vyšší než náklady na misclassifying nízkou úvěrové riziko jako vysoce misclassifying vysoké úvěrové riziko jako nízkými náklady.</span><span class="sxs-lookup"><span data-stu-id="2a410-174">As mentioned in the [previous step](machine-learning-walkthrough-2-upload-data.md), the cost of misclassifying a high credit risk as low is five times higher than the cost of misclassifying a low credit risk as high.</span></span> <span data-ttu-id="2a410-175">Aby se zohlednily to, se vygeneruje nová datová sada, která odráží tuto funkci náklady.</span><span class="sxs-lookup"><span data-stu-id="2a410-175">To account for this, we generate a new dataset that reflects this cost function.</span></span> <span data-ttu-id="2a410-176">V nová datová sada se každý příklad vysoce rizikové replikují pětkrát, zatímco se replikují každý příklad nízké riziko.</span><span class="sxs-lookup"><span data-stu-id="2a410-176">In the new dataset, each high risk example is replicated five times, while each low risk example is not replicated.</span></span>   

<span data-ttu-id="2a410-177">Provedeme tato replikace pomocí kódu jazyka R:</span><span class="sxs-lookup"><span data-stu-id="2a410-177">We can do this replication using R code:</span></span>  

1. <span data-ttu-id="2a410-178">Najděte a přetáhněte [spustit skript jazyka R] [ execute-r-script] modulu na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="2a410-178">Find and drag the [Execute R Script][execute-r-script] module onto the experiment canvas.</span></span> 

2. <span data-ttu-id="2a410-179">Připojit levý výstupní port modulu [rozdělení dat] [ split] modulu první vstupní port ("Dataset1") [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="2a410-179">Connect the left output port of the [Split Data][split] module to the first input port ("Dataset1") of the [Execute R Script][execute-r-script] module.</span></span>

3. <span data-ttu-id="2a410-180">Dvakrát klikněte [spustit skript jazyka R] [ execute-r-script] modul a zadejte komentář, "Sada úprava nákladů".</span><span class="sxs-lookup"><span data-stu-id="2a410-180">Double-click the [Execute R Script][execute-r-script] module and enter the comment, "Set cost adjustment".</span></span>

4. <span data-ttu-id="2a410-181">V **vlastnosti** podokně odstranit výchozí text v **skript jazyka R** parametr a zadejte tento skript:</span><span class="sxs-lookup"><span data-stu-id="2a410-181">In the **Properties** pane, delete the default text in the **R Script** parameter and enter this script:</span></span>
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R skript v modulu spustit skript jazyka R][9]

<span data-ttu-id="2a410-183">Je potřeba udělat stejná operace replikace pro každé výstup [rozdělení dat] [ split] modulu tak, aby data školení a testování stejné úpravu náklady.</span><span class="sxs-lookup"><span data-stu-id="2a410-183">We need to do this same replication operation for each output of the [Split Data][split] module so that the training and testing data have the same cost adjustment.</span></span> <span data-ttu-id="2a410-184">Nejjednodušší způsob je pomocí duplikování [spustit skript jazyka R] [ execute-r-script] modulu jsme provedli a jejím propojením s druhým výstupní port [rozdělení dat] [ split] modulu.</span><span class="sxs-lookup"><span data-stu-id="2a410-184">The easiest way to do this is by duplicating the [Execute R Script][execute-r-script] module we just made and connecting it to the other output port of the [Split Data][split] module.</span></span>

1. <span data-ttu-id="2a410-185">Klikněte pravým tlačítkem myši [spustit skript jazyka R] [ execute-r-script] modul a vyberte **kopie**.</span><span class="sxs-lookup"><span data-stu-id="2a410-185">Right-click the [Execute R Script][execute-r-script] module and select **Copy**.</span></span>

2. <span data-ttu-id="2a410-186">Klikněte pravým tlačítkem na plátno experimentu a vyberte **vložení**.</span><span class="sxs-lookup"><span data-stu-id="2a410-186">Right-click the experiment canvas and select **Paste**.</span></span>

3. <span data-ttu-id="2a410-187">Přetáhněte nového modulu do pozice a potom se připojte správné výstupní port modulu [rozdělení dat] [ split] modulu první vstupní port této nové [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="2a410-187">Drag the new module into position, and then connect the right output port of the [Split Data][split] module to the first input port of this new [Execute R Script][execute-r-script] module.</span></span> 

4. <span data-ttu-id="2a410-188">V dolní části plátna, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="2a410-188">At the bottom of the canvas, click **Run**.</span></span> 

> [!TIP]
> <span data-ttu-id="2a410-189">Kopii modulu spustit skript jazyka R obsahuje stejný skriptu jako původní modul.</span><span class="sxs-lookup"><span data-stu-id="2a410-189">The copy of the Execute R Script module contains the same script as the original module.</span></span> <span data-ttu-id="2a410-190">Když je kopírujete a vkládáte modul na plátně, bude pro kopii uchovává všechny vlastnosti původní.</span><span class="sxs-lookup"><span data-stu-id="2a410-190">When you copy and paste a module on the canvas, the copy retains all the properties of the original.</span></span>  
> 
> 

<span data-ttu-id="2a410-191">Naše experimentu nyní vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="2a410-191">Our experiment now looks something like this:</span></span>

![Přidání modulu rozdělení a skripty R][4]

<span data-ttu-id="2a410-193">Další informace o použití skripty R v experimentů najdete v tématu [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md).</span><span class="sxs-lookup"><span data-stu-id="2a410-193">For more information on using R scripts in your experiments, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md).</span></span>

<span data-ttu-id="2a410-194">**Další krok: [Train a vyhodnocení modelů](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span><span class="sxs-lookup"><span data-stu-id="2a410-194">**Next: [Train and evaluate the models](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span></span>

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
