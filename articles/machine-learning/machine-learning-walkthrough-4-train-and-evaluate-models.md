---
title: "Krok 4: Natrénování a vyhodnocení prediktivní analýzy modely | Microsoft Docs"
description: "Krok 4 vývoj prediktivního řešení návod: Train, stanovení skóre a vyhodnotit více modely v Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 58d46dd1464ec0a3fc9639f78d4429e0e778c2bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a><span data-ttu-id="c963e-103">Krok 4 průvodce: Natrénování a vyhodnocení modelů prediktivní analýzy</span><span class="sxs-lookup"><span data-stu-id="c963e-103">Walkthrough Step 4: Train and evaluate the predictive analytic models</span></span>
<span data-ttu-id="c963e-104">Toto téma obsahuje čtvrtý krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="c963e-104">This topic contains the fourth step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="c963e-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c963e-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="c963e-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="c963e-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="c963e-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="c963e-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. <span data-ttu-id="c963e-108">**Natrénování a vyhodnocení modelů**</span><span class="sxs-lookup"><span data-stu-id="c963e-108">**Train and evaluate the models**</span></span>
5. [<span data-ttu-id="c963e-109">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="c963e-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="c963e-110">Přístup k webové službě</span><span class="sxs-lookup"><span data-stu-id="c963e-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="c963e-111">Jednou z výhod používání Azure Machine Learning Studio pro vytváření modelů machine learning je schopnost zkuste více než jeden typ modelu v daný okamžik v jednom experimentu a porovnejte výsledky.</span><span class="sxs-lookup"><span data-stu-id="c963e-111">One of the benefits of using Azure Machine Learning Studio for creating machine learning models is the ability to try more than one type of model at a time in a single experiment and compare the results.</span></span> <span data-ttu-id="c963e-112">Tento typ experimentování vám pomůže najít nejlepší řešení pro váš problém.</span><span class="sxs-lookup"><span data-stu-id="c963e-112">This type of experimentation helps you find the best solution for your problem.</span></span>

<span data-ttu-id="c963e-113">V experimentu, který jsme se vývojem v tomto návodu jsme vytvoříte dva různé typy modely a potom porovnejte jejich vyhodnocování výsledky se rozhodnout, který algoritmus chceme používat v našem konečný experiment.</span><span class="sxs-lookup"><span data-stu-id="c963e-113">In the experiment we're developing in this walkthrough, we'll create two different types of models and then compare their scoring results to decide which algorithm we want to use in our final experiment.</span></span>  

<span data-ttu-id="c963e-114">Existují různé modely, které jsme může vybírat.</span><span class="sxs-lookup"><span data-stu-id="c963e-114">There are various models we could choose from.</span></span> <span data-ttu-id="c963e-115">Chcete-li zobrazit dostupné modely, rozbalte **Machine Learning** uzlu paletě modulů a potom rozbalte **inicializovat Model** a uzly pod ní.</span><span class="sxs-lookup"><span data-stu-id="c963e-115">To see the models available, expand the **Machine Learning** node in the module palette, and then expand **Initialize Model** and the nodes beneath it.</span></span> <span data-ttu-id="c963e-116">Pro účely tohoto experimentu, jsme vyberete [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) a [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] moduly.</span><span class="sxs-lookup"><span data-stu-id="c963e-116">For the purposes of this experiment, we'll select the [Two-Class Support Vector Machine][two-class-support-vector-machine] (SVM) and the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] modules.</span></span>    

> [!TIP]
> <span data-ttu-id="c963e-117">Potřebujete pomoc při rozhodování o tom, který algoritmus Machine Learning nejlepší vyhovuje konkrétní problém se snažíte vyřešit, najdete v části [jak zvolit algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span><span class="sxs-lookup"><span data-stu-id="c963e-117">To get help deciding which Machine Learning algorithm best suits the particular problem you're trying to solve, see [How to choose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span></span>
> 
> 

## <a name="train-the-models"></a><span data-ttu-id="c963e-118">Cvičení modely</span><span class="sxs-lookup"><span data-stu-id="c963e-118">Train the models</span></span>

<span data-ttu-id="c963e-119">Obě přidáme [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu a [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu v tohoto experimentu.</span><span class="sxs-lookup"><span data-stu-id="c963e-119">We'll add both the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module and [Two-Class Support Vector Machine][two-class-support-vector-machine] module in this experiment.</span></span>

### <a name="two-class-boosted-decision-tree"></a><span data-ttu-id="c963e-120">Two-Class Boosted Decision Tree</span><span class="sxs-lookup"><span data-stu-id="c963e-120">Two-Class Boosted Decision Tree</span></span>

<span data-ttu-id="c963e-121">Nejprve nastavíme modelu vylepšené rozhodovací strom.</span><span class="sxs-lookup"><span data-stu-id="c963e-121">First, let's set up the boosted decision tree model.</span></span>

1. <span data-ttu-id="c963e-122">Najít [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu paletě modulů a přetáhněte na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-122">Find the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module in the module palette and drag it onto the canvas.</span></span>

2. <span data-ttu-id="c963e-123">Najít [Train Model] [ train-model] modulu, přetáhněte na plátno a potom se připojte výstup [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modul vlevo vstupní port [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-123">Find the [Train Model][train-model] module, drag it onto the canvas, and then connect the output of the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module to the left input port of the [Train Model][train-model] module.</span></span>
   
   <span data-ttu-id="c963e-124">[Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu inicializuje obecné modelu a [trénování modelu] [ train-model] používá Cvičná data pro učení model.</span><span class="sxs-lookup"><span data-stu-id="c963e-124">The [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module initializes the generic model, and [Train Model][train-model] uses training data to train the model.</span></span> 

3. <span data-ttu-id="c963e-125">Připojit levém výstup doleva [spustit skript jazyka R] [ execute-r-script] modulu vpravo vstupní port [Train Model] [ train-model] (jsme se rozhodli v modulu [Krok 3](machine-learning-walkthrough-3-create-new-experiment.md) tohoto průvodce používat dat pocházejících z levé strany modulu rozdělení dat pro školení).</span><span class="sxs-lookup"><span data-stu-id="c963e-125">Connect the left output of the left [Execute R Script][execute-r-script] module to the right input port of the [Train Model][train-model] module (we decided in [Step 3](machine-learning-walkthrough-3-create-new-experiment.md) of this walkthrough to use the data coming from the left side of the Split Data module for training).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c963e-126">Společnost Microsoft nepotřebuje dvou vstupních hodnot a jeden z výstupů [spustit skript jazyka R] [ execute-r-script] modul pro tento experiment, takže jsme můžete je nechat odpojit.</span><span class="sxs-lookup"><span data-stu-id="c963e-126">We don't need two of the inputs and one of the outputs of the [Execute R Script][execute-r-script] module for this experiment, so we can leave them unattached.</span></span> 
   > 
   > 

<span data-ttu-id="c963e-127">Tato část experimentu nyní vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="c963e-127">This portion of the experiment now looks something like this:</span></span>  

![Cvičení modelu][1]

<span data-ttu-id="c963e-129">Nyní potřebujeme říct [Train Model] [ train-model] modulu, že má být model předpovědět úvěrové riziko hodnota.</span><span class="sxs-lookup"><span data-stu-id="c963e-129">Now we need to tell the [Train Model][train-model] module that we want the model to predict the Credit Risk value.</span></span>

1. <span data-ttu-id="c963e-130">Vyberte [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-130">Select the [Train Model][train-model] module.</span></span> <span data-ttu-id="c963e-131">V **vlastnosti** podokně klikněte na tlačítko **spustit selektor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="c963e-131">In the **Properties** pane, click **Launch column selector**.</span></span>

2. <span data-ttu-id="c963e-132">V **vyberte jeden sloupec** dialogové okno, zadejte "úvěrového rizika" v poli vyhledávání ve **dostupné sloupce**, vyberte "Úvěrového rizika" níže a klikněte na tlačítko se šipkou vpravo ( **>** ) přesunout "Úvěrové riziko" do **vybrané sloupce**.</span><span class="sxs-lookup"><span data-stu-id="c963e-132">In the **Select a single column** dialog, type "credit risk" in the search field under **Available Columns**, select "Credit risk" below, and click the right arrow button (**>**) to move "Credit risk" to **Selected Columns**.</span></span> 

    ![Vyberte sloupec úvěrové riziko u modulu Train Model][0]

3. <span data-ttu-id="c963e-134">Klikněte **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="c963e-134">Click the **OK** check mark.</span></span>

### <a name="two-class-support-vector-machine"></a><span data-ttu-id="c963e-135">Support Vector Machine (SVM) se dvěma třídami</span><span class="sxs-lookup"><span data-stu-id="c963e-135">Two-Class Support Vector Machine</span></span>

<span data-ttu-id="c963e-136">V dalším kroku nastavíme SVM modelu.</span><span class="sxs-lookup"><span data-stu-id="c963e-136">Next, we set up the SVM model.</span></span>  

<span data-ttu-id="c963e-137">První, trochu vysvětlení SVM.</span><span class="sxs-lookup"><span data-stu-id="c963e-137">First, a little explanation about SVM.</span></span> <span data-ttu-id="c963e-138">Vylepšené rozhodovací stromy fungují dobře u funkce libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="c963e-138">Boosted decision trees work well with features of any type.</span></span> <span data-ttu-id="c963e-139">Ale vzhledem k tomu, že modul SVM generuje lineární třídění, nemá model, který generuje nejlepší Chyba testu, pokud všechny číselné funkce mají stejné měřítko.</span><span class="sxs-lookup"><span data-stu-id="c963e-139">However, since the SVM module generates a linear classifier, the model that it generates has the best test error when all numeric features have the same scale.</span></span> <span data-ttu-id="c963e-140">Všechny číselné funkce převést na stejné měřítko, používáme transformace "Tanh" (s [normalizaci dat] [ normalize-data] modulu).</span><span class="sxs-lookup"><span data-stu-id="c963e-140">To convert all numeric features to the same scale, we use a "Tanh" transformation (with the [Normalize Data][normalize-data] module).</span></span> <span data-ttu-id="c963e-141">To transformuje naše čísla do rozsahu [0, 1].</span><span class="sxs-lookup"><span data-stu-id="c963e-141">This transforms our numbers into the [0,1] range.</span></span> <span data-ttu-id="c963e-142">Modul SVM převede řetězec funkce kategorií funkcí a pak na binární funkce 0 nebo 1, proto jsme nemusíte ručně transformace funkce řetězec.</span><span class="sxs-lookup"><span data-stu-id="c963e-142">The SVM module converts string features to categorical features and then to binary 0/1 features, so we don't need to manually transform string features.</span></span> <span data-ttu-id="c963e-143">Také jsme nechcete transformace sloupci úvěrové riziko (sloupec 21) – je číselné, ale je hodnota jsme se tréninku modelu k předvídání, takže je potřeba ho ponecháte.</span><span class="sxs-lookup"><span data-stu-id="c963e-143">Also, we don't want to transform the Credit Risk column (column 21) - it's numeric, but it's the value we're training the model to predict, so we need to leave it alone.</span></span>  

<span data-ttu-id="c963e-144">Pokud chcete nastavit SVM modelu, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="c963e-144">To set up the SVM model, do the following:</span></span>

1. <span data-ttu-id="c963e-145">Najít [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu paletě modulů a přetáhněte na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-145">Find the [Two-Class Support Vector Machine][two-class-support-vector-machine] module in the module palette and drag it onto the canvas.</span></span>

2. <span data-ttu-id="c963e-146">Klikněte pravým tlačítkem myši [Train Model] [ train-model] modulu, vyberte **kopie**a potom klikněte pravým tlačítkem na plátno a vyberte **vložení**.</span><span class="sxs-lookup"><span data-stu-id="c963e-146">Right-click the [Train Model][train-model] module, select **Copy**, and then right-click the canvas and select **Paste**.</span></span> <span data-ttu-id="c963e-147">Kopii [Train Model] [ train-model] modul má stejné výběr sloupce jako původní.</span><span class="sxs-lookup"><span data-stu-id="c963e-147">The copy of the [Train Model][train-model] module has the same column selection as the original.</span></span>

3. <span data-ttu-id="c963e-148">Připojit výstup [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu vlevo vstupní port druhý [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-148">Connect the output of the [Two-Class Support Vector Machine][two-class-support-vector-machine] module to the left input port of the second [Train Model][train-model] module.</span></span>

4. <span data-ttu-id="c963e-149">Najít [normalizaci dat] [ normalize-data] modulu a přetáhněte na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-149">Find the [Normalize Data][normalize-data] module and drag it onto the canvas.</span></span>

5. <span data-ttu-id="c963e-150">Připojit levém výstup doleva [spustit skript jazyka R] [ execute-r-script] modulu vstup tohoto modulu (Všimněte si, že na výstupní port modulu může být připojen k více než jeden další modul).</span><span class="sxs-lookup"><span data-stu-id="c963e-150">Connect the left output of the left [Execute R Script][execute-r-script] module to the input of this module (notice that the output port of a module may be connected to more than one other module).</span></span>

6. <span data-ttu-id="c963e-151">Připojit levý výstupní port modulu [normalizaci dat] [ normalize-data] modulu vpravo vstupní port druhý [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-151">Connect the left output port of the [Normalize Data][normalize-data] module to the right input port of the second [Train Model][train-model] module.</span></span>

<span data-ttu-id="c963e-152">Tato část naší experiment by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="c963e-152">This portion of our experiment should now look something like this:</span></span>  

![Cvičení druhý modelu][2]  

<span data-ttu-id="c963e-154">Teď nakonfigurovat [normalizaci dat] [ normalize-data] modul:</span><span class="sxs-lookup"><span data-stu-id="c963e-154">Now configure the [Normalize Data][normalize-data] module:</span></span>

1. <span data-ttu-id="c963e-155">Kliknutím vyberte [normalizaci dat] [ normalize-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-155">Click to select the [Normalize Data][normalize-data] module.</span></span> <span data-ttu-id="c963e-156">V **vlastnosti** podokně, vyberte **Tanh** pro **transformace metoda** parametr.</span><span class="sxs-lookup"><span data-stu-id="c963e-156">In the **Properties** pane, select **Tanh** for the **Transformation method** parameter.</span></span>

2. <span data-ttu-id="c963e-157">Klikněte na tlačítko **spustit selektor sloupců**, vyberte možnost "Žádné sloupce" pro **začít s**, vyberte **zahrnout** v první rozevíracího seznamu, vyberte **typ sloupce** v druhé rozevírací seznam a vyberte **číselné** v třetí rozevírací nabídce.</span><span class="sxs-lookup"><span data-stu-id="c963e-157">Click **Launch column selector**, select "No columns" for **Begin With**, select **Include** in the first dropdown, select **column type** in the second dropdown, and select **Numeric** in the third dropdown.</span></span> <span data-ttu-id="c963e-158">Určuje transformaci všechny číselné sloupce (a pouze číslice).</span><span class="sxs-lookup"><span data-stu-id="c963e-158">This specifies that all the numeric columns (and only numeric) are transformed.</span></span>

3. <span data-ttu-id="c963e-159">Kliknutím na symbol plus (+) napravo od tento řádek – tím se vytvoří řádek rozevíracích seznamů.</span><span class="sxs-lookup"><span data-stu-id="c963e-159">Click the plus sign (+) to the right of this row - this creates a row of dropdowns.</span></span> <span data-ttu-id="c963e-160">Vyberte **vyloučit** v první rozevíracího seznamu, vyberte **názvy sloupců** v druhé rozevírací nabídce a zadejte "Úvěrového rizika" v textové pole.</span><span class="sxs-lookup"><span data-stu-id="c963e-160">Select **Exclude** in the first dropdown, select **column names** in the second dropdown, and enter "Credit risk" in the text field.</span></span> <span data-ttu-id="c963e-161">Určuje, že sloupec úvěrové riziko třeba ji ignorovat (je potřeba to udělat, protože tento sloupec je číselná a proto by transformován Pokud jsme ho nebylo vyloučit).</span><span class="sxs-lookup"><span data-stu-id="c963e-161">This specifies that the Credit Risk column should be ignored (we need to do this because this column is numeric and so would be transformed if we didn't exclude it).</span></span>

4. <span data-ttu-id="c963e-162">Klikněte **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="c963e-162">Click the **OK** check mark.</span></span>  

    ![Vyberte sloupce pro modul normalizaci dat][5]

<span data-ttu-id="c963e-164">[Normalizaci dat] [ normalize-data] modulu je teď nastavená na provedení Tanh transformace na všechny číselné sloupce s výjimkou sloupci úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="c963e-164">The [Normalize Data][normalize-data] module is now set to perform a Tanh transformation on all numeric columns except for the Credit Risk column.</span></span>  

## <a name="score-and-evaluate-the-models"></a><span data-ttu-id="c963e-165">Stanovení skóre a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="c963e-165">Score and evaluate the models</span></span>

<span data-ttu-id="c963e-166">Používáme testování data, která byla oddělených se [rozdělení dat] [ split] modul skóre pro naše trénované modely.</span><span class="sxs-lookup"><span data-stu-id="c963e-166">We use the testing data that was separated out by the [Split Data][split] module to score our trained models.</span></span> <span data-ttu-id="c963e-167">Jsme pak můžete porovnat výsledky dva modely zobrazíte, který generuje lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="c963e-167">We can then compare the results of the two models to see which generated better results.</span></span>  

### <a name="add-the-score-model-modules"></a><span data-ttu-id="c963e-168">Přidání modulů Score Model</span><span class="sxs-lookup"><span data-stu-id="c963e-168">Add the Score Model modules</span></span>

1. <span data-ttu-id="c963e-169">Najít [Score Model] [ score-model] modulu a přetáhněte na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-169">Find the [Score Model][score-model] module and drag it onto the canvas.</span></span>

2. <span data-ttu-id="c963e-170">Připojení [Train Model] [ train-model] modul, který je připojen k [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu vlevo vstupní port [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-170">Connect the [Train Model][train-model] module that's connected to the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module to the left input port of the [Score Model][score-model] module.</span></span>

3. <span data-ttu-id="c963e-171">Připojit právo [spustit skript jazyka R] [ execute-r-script] modulu (naše testování data) napravo vstupní port [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-171">Connect the right [Execute R Script][execute-r-script] module (our testing data) to the right input port of the [Score Model][score-model] module.</span></span>

    ![Modul určení skóre modelu připojení][6]
   
   <span data-ttu-id="c963e-173">[Score Model] [ score-model] modulu teď můžete provést platební informace z testovacích dat, spusťte ho pomocí modelu a porovnat předpovědi modelu generuje se sloupci skutečné platební riziko v testování data.</span><span class="sxs-lookup"><span data-stu-id="c963e-173">The [Score Model][score-model] module can now take the credit information from the testing data, run it through the model, and compare the predictions the model generates with the actual credit risk column in the testing data.</span></span>

4. <span data-ttu-id="c963e-174">Zkopírujte a vložte [Score Model] [ score-model] modul k vytváření druhé kopie.</span><span class="sxs-lookup"><span data-stu-id="c963e-174">Copy and paste the [Score Model][score-model] module to create a second copy.</span></span>

5. <span data-ttu-id="c963e-175">Připojit výstup SVM modelu (to znamená, výstupní port modulu [Train Model] [ train-model] modul, který je připojen k [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu) pro vstupní port druhý [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-175">Connect the output of the SVM model (that is, the output port of the [Train Model][train-model] module that's connected to the [Two-Class Support Vector Machine][two-class-support-vector-machine] module) to the input port of the second [Score Model][score-model] module.</span></span>

6. <span data-ttu-id="c963e-176">V případě modelu SVM máme provést stejné transformace na testovací data, jako jsme to udělali pro Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="c963e-176">For the SVM model, we have to do the same transformation to the test data as we did to the training data.</span></span> <span data-ttu-id="c963e-177">Proto zkopírujte a vložte [normalizaci dat] [ normalize-data] modul k vytváření druhé kopie a připojte ho k právo [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-177">So copy and paste the [Normalize Data][normalize-data] module to create a second copy and connect it to the right [Execute R Script][execute-r-script] module.</span></span>

7. <span data-ttu-id="c963e-178">Připojit levý výstupní druhého [normalizaci dat] [ normalize-data] modulu vpravo vstupní port druhý [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-178">Connect the left output of the second [Normalize Data][normalize-data] module to the right input port of the second [Score Model][score-model] module.</span></span>

    ![Obě Score Model moduly připojení][7]

### <a name="add-the-evaluate-model-module"></a><span data-ttu-id="c963e-180">Přidání modulu Evaluate Model</span><span class="sxs-lookup"><span data-stu-id="c963e-180">Add the Evaluate Model module</span></span>

<span data-ttu-id="c963e-181">K vyhodnocení oba vyhodnocování výsledky a porovnat je, používáme [Evaluate Model] [ evaluate-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-181">To evaluate the two scoring results and compare them, we use an [Evaluate Model][evaluate-model] module.</span></span>  

1. <span data-ttu-id="c963e-182">Najít [Evaluate Model] [ evaluate-model] modulu a přetáhněte na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-182">Find the [Evaluate Model][evaluate-model] module and drag it onto the canvas.</span></span>

2. <span data-ttu-id="c963e-183">Připojit výstupní port modulu [Score Model] [ score-model] modulu přidruženého k modelu vylepšené rozhodovací strom pro levý vstupní port [Evaluate Model] [ evaluate-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-183">Connect the output port of the [Score Model][score-model] module associated with the boosted decision tree model to the left input port of the [Evaluate Model][evaluate-model] module.</span></span>

3. <span data-ttu-id="c963e-184">Připojení dalších [Score Model] [ score-model] modulu vpravo vstupní port.</span><span class="sxs-lookup"><span data-stu-id="c963e-184">Connect the other [Score Model][score-model] module to the right input port.</span></span>  

    ![Vyhodnocení modelu modulu připojení][8]

### <a name="run-the-experiment-and-check-the-results"></a><span data-ttu-id="c963e-186">Spusťte experiment a Kontrola výsledků</span><span class="sxs-lookup"><span data-stu-id="c963e-186">Run the experiment and check the results</span></span>

<span data-ttu-id="c963e-187">Chcete-li spustit experiment, klikněte na tlačítko **spustit** tlačítko níže na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-187">To run the experiment, click the **RUN** button below the canvas.</span></span> <span data-ttu-id="c963e-188">To může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="c963e-188">It may take a few minutes.</span></span> <span data-ttu-id="c963e-189">Na roztočený ukazatel na každý modul ukazuje, zda je spuštěná a poté zobrazí zelená značka zaškrtnutí po dokončení modulu.</span><span class="sxs-lookup"><span data-stu-id="c963e-189">A spinning indicator on each module shows that it's running, and then a green check mark shows when the module is finished.</span></span> <span data-ttu-id="c963e-190">Pokud všechny moduly mají zaškrtnuto, experimentu bylo dokončeno.</span><span class="sxs-lookup"><span data-stu-id="c963e-190">When all the modules have a check mark, the experiment has finished running.</span></span>

<span data-ttu-id="c963e-191">Experiment by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="c963e-191">The experiment should now look something like this:</span></span>  

![Vyhodnocení obou modelů][3]

<span data-ttu-id="c963e-193">Pokud chcete zkontrolovat výsledky, klikněte na výstupní port modulu [Evaluate Model] [ evaluate-model] modul a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="c963e-193">To check the results, click the output port of the [Evaluate Model][evaluate-model] module and select **Visualize**.</span></span>  

<span data-ttu-id="c963e-194">[Evaluate Model] [ evaluate-model] modulu vytvoří dvojici křivek a metriky, které vám umožní porovnat výsledky dva scored modely.</span><span class="sxs-lookup"><span data-stu-id="c963e-194">The [Evaluate Model][evaluate-model] module produces a pair of curves and metrics that allow you to compare the results of the two scored models.</span></span> <span data-ttu-id="c963e-195">Můžete zobrazit výsledky jako křivek příjemce operátor vlastnosti (ROC), křivek přesnost nebo odvolání nebo křivek navýšení.</span><span class="sxs-lookup"><span data-stu-id="c963e-195">You can view the results as Receiver Operator Characteristic (ROC) curves, Precision/Recall curves, or Lift curves.</span></span> <span data-ttu-id="c963e-196">Další data zobrazí zahrnuje matice nejasnostem, kumulativní hodnoty pro oblasti pod křivky (AUC) a další metriky.</span><span class="sxs-lookup"><span data-stu-id="c963e-196">Additional data displayed includes a confusion matrix, cumulative values for the area under the curve (AUC), and other metrics.</span></span> <span data-ttu-id="c963e-197">Můžete změnit prahovou hodnotu posunutím jezdce doleva nebo doprava a v tématu Jak ovlivňuje sadu metriky.</span><span class="sxs-lookup"><span data-stu-id="c963e-197">You can change the threshold value by moving the slider left or right and see how it affects the set of metrics.</span></span>  

<span data-ttu-id="c963e-198">Napravo od grafu, klikněte na tlačítko **skóre pro Magnitudu datovou sadu** nebo **skóre pro Magnitudu datovou sadu, která porovnat** zvýrazněte přidružené křivky a zobrazení přidružené metriky níže.</span><span class="sxs-lookup"><span data-stu-id="c963e-198">To the right of the graph, click **Scored dataset** or **Scored dataset to compare** to highlight the associated curve and to display the associated metrics below.</span></span> <span data-ttu-id="c963e-199">V legendě pro křivky "Skóre pro Magnitudu datovou sadu" odpovídá levý vstupní port [Evaluate Model] [ evaluate-model] modul – v našem případě je to modelu vylepšené rozhodovací strom.</span><span class="sxs-lookup"><span data-stu-id="c963e-199">In the legend for the curves, "Scored dataset" corresponds to the left input port of the [Evaluate Model][evaluate-model] module - in our case, this is the boosted decision tree model.</span></span> <span data-ttu-id="c963e-200">"Skóre pro magnitudu datovou sadu, která porovnat" odpovídá pravý vstupní port - SVM modelu v našem případě.</span><span class="sxs-lookup"><span data-stu-id="c963e-200">"Scored dataset to compare" corresponds to the right input port - the SVM model in our case.</span></span> <span data-ttu-id="c963e-201">Když kliknete na jednu z těchto popisky, křivky pro tento model je označený a odpovídající metriky se zobrazí, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="c963e-201">When you click one of these labels, the curve for that model is highlighted and the corresponding metrics are displayed, as shown in the following graphic.</span></span>  

![Křivek ROC pro modely][4]

<span data-ttu-id="c963e-203">Prověřením tyto hodnoty můžete rozhodnout, které model je nejblíže k budete výsledků, které hledáte.</span><span class="sxs-lookup"><span data-stu-id="c963e-203">By examining these values, you can decide which model is closest to giving you the results you're looking for.</span></span> <span data-ttu-id="c963e-204">Můžete se vrátit zpět a iterovat experimentu změnou hodnoty parametrů v různých modelech.</span><span class="sxs-lookup"><span data-stu-id="c963e-204">You can go back and iterate on your experiment by changing parameter values in the different models.</span></span> 

<span data-ttu-id="c963e-205">Vědecké účely a obrázky interpretace těchto výsledků a ladění výkonu modelu je mimo rámec tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="c963e-205">The science and art of interpreting these results and tuning the model performance is outside the scope of this walkthrough.</span></span> <span data-ttu-id="c963e-206">Potřebujete další pomoc můžou číst v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="c963e-206">For additional help, you might read the following articles:</span></span>
- [<span data-ttu-id="c963e-207">Postup vyhodnocení modelu výkon v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c963e-207">How to evaluate model performance in Azure Machine Learning</span></span>](machine-learning-evaluate-model-performance.md)
- [<span data-ttu-id="c963e-208">Vyberte parametry pro Optimalizujte algoritmy v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c963e-208">Choose parameters to optimize your algorithms in Azure Machine Learning</span></span>](machine-learning-algorithm-parameters-optimize.md)
- [<span data-ttu-id="c963e-209">Interpretovat výsledky modelu v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c963e-209">Interpret model results in Azure Machine Learning</span></span>](machine-learning-interpret-model-results.md)

> [!TIP]
> <span data-ttu-id="c963e-210">Při každém spuštění záznam této iterace experimentu je uchovat v historii spustit.</span><span class="sxs-lookup"><span data-stu-id="c963e-210">Each time you run the experiment a record of that iteration is kept in the Run History.</span></span> <span data-ttu-id="c963e-211">Můžete zobrazit tyto iterací a vrátit zpět do některé z nich, klepnutím **zobrazit HISTORII BĚHŮ** níže na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-211">You can view these iterations, and return to any of them, by clicking **VIEW RUN HISTORY** below the canvas.</span></span> <span data-ttu-id="c963e-212">Můžete také kliknout na **předchozí spustit** v **vlastnosti** podokno a vraťte iterace bezprostředně předcházející ten budete mít otevřete.</span><span class="sxs-lookup"><span data-stu-id="c963e-212">You can also click **Prior Run** in the **Properties** pane to return to the iteration immediately preceding the one you have open.</span></span>
> 
> <span data-ttu-id="c963e-213">Kliknutím můžete vytvořit kopii kteroukoli iteraci experimentu **uložit jako** níže na plátno.</span><span class="sxs-lookup"><span data-stu-id="c963e-213">You can make a copy of any iteration of your experiment by clicking **SAVE AS** below the canvas.</span></span> 
> <span data-ttu-id="c963e-214">Použít experiment **Souhrn** a **popis** vlastnosti, které chcete uchovávat informace o jste zkusili ve vašem iterací experimentu.</span><span class="sxs-lookup"><span data-stu-id="c963e-214">Use the experiment's **Summary** and **Description** properties to keep a record of what you've tried in your experiment iterations.</span></span>
> 
> <span data-ttu-id="c963e-215">Další podrobnosti najdete v tématu [Správa iterací experimentu v nástroji Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span><span class="sxs-lookup"><span data-stu-id="c963e-215">For more details, see [Manage experiment iterations in Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span></span>  
> 
> 

- - -
<span data-ttu-id="c963e-216">**Další krok: [nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="c963e-216">**Next: [Deploy the web service](machine-learning-walkthrough-5-publish-web-service.md)**</span></span>

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
