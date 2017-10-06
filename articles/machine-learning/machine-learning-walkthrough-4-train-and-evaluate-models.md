---
title: "Krok 4: Natrénování a vyhodnocení hello prediktivní analýzy modely | Microsoft Docs"
description: "Krok 4 hello vývoj prediktivního řešení návod: Train, stanovení skóre a vyhodnotit více modely v Azure Machine Learning Studio."
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
ms.openlocfilehash: d86d7c5ae7524f71fe44d985db67c4618b7965a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-hello-predictive-analytic-models"></a><span data-ttu-id="610e4-103">Návod krok 4: Natrénování a vyhodnocení hello prediktivní analýzy modely</span><span class="sxs-lookup"><span data-stu-id="610e4-103">Walkthrough Step 4: Train and evaluate hello predictive analytic models</span></span>
<span data-ttu-id="610e4-104">Toto téma obsahuje hello čtvrtý krok hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="610e4-104">This topic contains hello fourth step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="610e4-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="610e4-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="610e4-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="610e4-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="610e4-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="610e4-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. <span data-ttu-id="610e4-108">**Natrénování a vyhodnocení modelů hello**</span><span class="sxs-lookup"><span data-stu-id="610e4-108">**Train and evaluate hello models**</span></span>
5. [<span data-ttu-id="610e4-109">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="610e4-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="610e4-110">Přístup k webové službě hello</span><span class="sxs-lookup"><span data-stu-id="610e4-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="610e4-111">Jednou z výhod hello pomocí Azure Machine Learning Studio pro vytváření modelů machine learning je více než jeden typ modelu hello možnost tootry v daný okamžik v jednom experimentu a porovnejte výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-111">One of hello benefits of using Azure Machine Learning Studio for creating machine learning models is hello ability tootry more than one type of model at a time in a single experiment and compare hello results.</span></span> <span data-ttu-id="610e4-112">Tento typ experimentování vám pomůže najít hello nejlepší řešení pro váš problém.</span><span class="sxs-lookup"><span data-stu-id="610e4-112">This type of experimentation helps you find hello best solution for your problem.</span></span>

<span data-ttu-id="610e4-113">V experimentu hello jsme se vývojem v tomto návodu, jsme vytvoříte dva různé typy modely a potom porovnejte jejich vyhodnocování toodecide výsledky který algoritmus chceme toouse v našem konečný experiment.</span><span class="sxs-lookup"><span data-stu-id="610e4-113">In hello experiment we're developing in this walkthrough, we'll create two different types of models and then compare their scoring results toodecide which algorithm we want toouse in our final experiment.</span></span>  

<span data-ttu-id="610e4-114">Existují různé modely, které jsme může vybírat.</span><span class="sxs-lookup"><span data-stu-id="610e4-114">There are various models we could choose from.</span></span> <span data-ttu-id="610e4-115">toosee hello modely k dispozici, rozbalte položku hello **Machine Learning** uzel v hello palety modulů a potom rozbalte **inicializovat Model** a uzly hello pod ní.</span><span class="sxs-lookup"><span data-stu-id="610e4-115">toosee hello models available, expand hello **Machine Learning** node in hello module palette, and then expand **Initialize Model** and hello nodes beneath it.</span></span> <span data-ttu-id="610e4-116">Pro účely hello tohoto experimentu, jsme vyberete hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] (SVM) a hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] moduly.</span><span class="sxs-lookup"><span data-stu-id="610e4-116">For hello purposes of this experiment, we'll select hello [Two-Class Support Vector Machine][two-class-support-vector-machine] (SVM) and hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] modules.</span></span>    

> [!TIP]
> <span data-ttu-id="610e4-117">Nápověda tooget rozhodování, který algoritmus Machine Learning nejlépe vyhovuje hello konkrétního problému, které se snažíte toosolve najdete v tématu [jak toochoose algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span><span class="sxs-lookup"><span data-stu-id="610e4-117">tooget help deciding which Machine Learning algorithm best suits hello particular problem you're trying toosolve, see [How toochoose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span></span>
> 
> 

## <a name="train-hello-models"></a><span data-ttu-id="610e4-118">Cvičení hello modely</span><span class="sxs-lookup"><span data-stu-id="610e4-118">Train hello models</span></span>

<span data-ttu-id="610e4-119">Přidáme obou hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu a [Two-Class Support Vector Machine] [ two-class-support-vector-machine] v tomto modulu experimentu.</span><span class="sxs-lookup"><span data-stu-id="610e4-119">We'll add both hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module and [Two-Class Support Vector Machine][two-class-support-vector-machine] module in this experiment.</span></span>

### <a name="two-class-boosted-decision-tree"></a><span data-ttu-id="610e4-120">Two-Class Boosted Decision Tree</span><span class="sxs-lookup"><span data-stu-id="610e4-120">Two-Class Boosted Decision Tree</span></span>

<span data-ttu-id="610e4-121">Nejprve nastavíme hello boosted rozhodovacího stromu modelu.</span><span class="sxs-lookup"><span data-stu-id="610e4-121">First, let's set up hello boosted decision tree model.</span></span>

1. <span data-ttu-id="610e4-122">Najde hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu v palety modulů hello a přetáhněte ji na plátno hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-122">Find hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module in hello module palette and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="610e4-123">Najde hello [Train Model] [ train-model] modulu, přetáhněte na plátno hello a připojte hello výstup hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree]toohello modulu zbývajících vstupní port hello [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-123">Find hello [Train Model][train-model] module, drag it onto hello canvas, and then connect hello output of hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module toohello left input port of hello [Train Model][train-model] module.</span></span>
   
   <span data-ttu-id="610e4-124">Hello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] modulu inicializuje hello obecné modelu a [Train Model] [ train-model] používá Cvičná data tootrain hello model.</span><span class="sxs-lookup"><span data-stu-id="610e4-124">hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module initializes hello generic model, and [Train Model][train-model] uses training data tootrain hello model.</span></span> 

3. <span data-ttu-id="610e4-125">Připojit hello levém výstup hello vlevo [spustit skript jazyka R] [ execute-r-script] modulu toohello právo vstupní port hello [Train Model] [ train-model] modulu (jsme Rozhodli v [krok 3](machine-learning-walkthrough-3-create-new-experiment.md) tento návod toouse hello dat pocházejících z levé straně hello rozdělení dat modul pro školení hello).</span><span class="sxs-lookup"><span data-stu-id="610e4-125">Connect hello left output of hello left [Execute R Script][execute-r-script] module toohello right input port of hello [Train Model][train-model] module (we decided in [Step 3](machine-learning-walkthrough-3-create-new-experiment.md) of this walkthrough toouse hello data coming from hello left side of hello Split Data module for training).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="610e4-126">Společnost Microsoft nepotřebuje dva vstupy hello a jeden z hello výstupy hello [spustit skript jazyka R] [ execute-r-script] modul pro tento experiment, takže jsme můžete je nechat odpojit.</span><span class="sxs-lookup"><span data-stu-id="610e4-126">We don't need two of hello inputs and one of hello outputs of hello [Execute R Script][execute-r-script] module for this experiment, so we can leave them unattached.</span></span> 
   > 
   > 

<span data-ttu-id="610e4-127">Tato část hello experimentu nyní vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="610e4-127">This portion of hello experiment now looks something like this:</span></span>  

![Cvičení modelu][1]

<span data-ttu-id="610e4-129">Nyní potřebujeme tootell hello [Train Model] [ train-model] modulu, že má být hodnota úvěrové riziko hello modelu toopredict hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-129">Now we need tootell hello [Train Model][train-model] module that we want hello model toopredict hello Credit Risk value.</span></span>

1. <span data-ttu-id="610e4-130">Vyberte hello [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-130">Select hello [Train Model][train-model] module.</span></span> <span data-ttu-id="610e4-131">V hello **vlastnosti** podokně klikněte na tlačítko **spustit selektor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="610e4-131">In hello **Properties** pane, click **Launch column selector**.</span></span>

2. <span data-ttu-id="610e4-132">V hello **vyberte jeden sloupec** dialogové okno, zadejte "úvěrového rizika" v poli vyhledávání hello pod **dostupné sloupce**, vyberte "Úvěrového rizika" níže a tlačítko s šipkou vpravo hello ( **>** ) toomove "Úvěrového rizika" příliš**vybrané sloupce**.</span><span class="sxs-lookup"><span data-stu-id="610e4-132">In hello **Select a single column** dialog, type "credit risk" in hello search field under **Available Columns**, select "Credit risk" below, and click hello right arrow button (**>**) toomove "Credit risk" too**Selected Columns**.</span></span> 

    ![Vyberte sloupec hello úvěrové riziko pro modulu Train Model hello][0]

3. <span data-ttu-id="610e4-134">Klikněte na tlačítko hello **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="610e4-134">Click hello **OK** check mark.</span></span>

### <a name="two-class-support-vector-machine"></a><span data-ttu-id="610e4-135">Support Vector Machine (SVM) se dvěma třídami</span><span class="sxs-lookup"><span data-stu-id="610e4-135">Two-Class Support Vector Machine</span></span>

<span data-ttu-id="610e4-136">V dalším kroku nastavíme hello SVM modelu.</span><span class="sxs-lookup"><span data-stu-id="610e4-136">Next, we set up hello SVM model.</span></span>  

<span data-ttu-id="610e4-137">První, trochu vysvětlení SVM.</span><span class="sxs-lookup"><span data-stu-id="610e4-137">First, a little explanation about SVM.</span></span> <span data-ttu-id="610e4-138">Vylepšené rozhodovací stromy fungují dobře u funkce libovolného typu.</span><span class="sxs-lookup"><span data-stu-id="610e4-138">Boosted decision trees work well with features of any type.</span></span> <span data-ttu-id="610e4-139">Vzhledem k tomu, že modul SVM hello generuje lineární třídění, hello model, který generuje má však nejlepší Chyba testu hello Pokud všechny číselné funkce hello stejné měřítko.</span><span class="sxs-lookup"><span data-stu-id="610e4-139">However, since hello SVM module generates a linear classifier, hello model that it generates has hello best test error when all numeric features have hello same scale.</span></span> <span data-ttu-id="610e4-140">tooconvert numerické funkce toohello stejné škálování, používáme transformace "Tanh" (s hello [normalizaci dat] [ normalize-data] modulu).</span><span class="sxs-lookup"><span data-stu-id="610e4-140">tooconvert all numeric features toohello same scale, we use a "Tanh" transformation (with hello [Normalize Data][normalize-data] module).</span></span> <span data-ttu-id="610e4-141">To transformuje naše čísla na rozsah hello [0,1].</span><span class="sxs-lookup"><span data-stu-id="610e4-141">This transforms our numbers into hello [0,1] range.</span></span> <span data-ttu-id="610e4-142">Hello SVM modulu převede řetězec funkce toocategorical funkce a pak funkce toobinary 0 nebo 1, takže nepodporujeme potřebovat toomanually transformace funkce řetězec.</span><span class="sxs-lookup"><span data-stu-id="610e4-142">hello SVM module converts string features toocategorical features and then toobinary 0/1 features, so we don't need toomanually transform string features.</span></span> <span data-ttu-id="610e4-143">Také jsme nechcete, aby tootransform hello úvěrové riziko sloupci (21) – je číselná, ale je hodnota hello jsme se cvičení hello toopredict modelu, takže potřebujeme tooleave ho samostatně.</span><span class="sxs-lookup"><span data-stu-id="610e4-143">Also, we don't want tootransform hello Credit Risk column (column 21) - it's numeric, but it's hello value we're training hello model toopredict, so we need tooleave it alone.</span></span>  

<span data-ttu-id="610e4-144">tooset až hello SVM modelu hello následující:</span><span class="sxs-lookup"><span data-stu-id="610e4-144">tooset up hello SVM model, do hello following:</span></span>

1. <span data-ttu-id="610e4-145">Najde hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu v palety modulů hello a přetáhněte ji na plátno hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-145">Find hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module in hello module palette and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="610e4-146">Klikněte pravým tlačítkem na hello [Train Model] [ train-model] modulu, vyberte **kopie**a potom klikněte pravým tlačítkem na plátno hello a vyberte **vložení**.</span><span class="sxs-lookup"><span data-stu-id="610e4-146">Right-click hello [Train Model][train-model] module, select **Copy**, and then right-click hello canvas and select **Paste**.</span></span> <span data-ttu-id="610e4-147">Hello kopii hello [Train Model] [ train-model] modul má hello stejné výběr sloupce jako původní hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-147">hello copy of hello [Train Model][train-model] module has hello same column selection as hello original.</span></span>

3. <span data-ttu-id="610e4-148">Připojit hello výstup hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] toohello modulu zbývajících vstupní port hello druhý [Train Model] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="610e4-148">Connect hello output of hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module toohello left input port of hello second [Train Model][train-model] module.</span></span>

4. <span data-ttu-id="610e4-149">Najde hello [normalizaci dat] [ normalize-data] modulu a přetáhněte ji na plátno hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-149">Find hello [Normalize Data][normalize-data] module and drag it onto hello canvas.</span></span>

5. <span data-ttu-id="610e4-150">Připojit hello levém výstup hello vlevo [spustit skript jazyka R] [ execute-r-script] vstup toohello modulu tohoto modulu (Všimněte si, že hello výstupní port modulu může být připojené toomore než jeden další modul).</span><span class="sxs-lookup"><span data-stu-id="610e4-150">Connect hello left output of hello left [Execute R Script][execute-r-script] module toohello input of this module (notice that hello output port of a module may be connected toomore than one other module).</span></span>

6. <span data-ttu-id="610e4-151">Připojit hello zbývajících výstupní port modulu hello [normalizaci dat] [ normalize-data] modulu toohello právo vstupní port hello druhý [Train Model] [ train-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-151">Connect hello left output port of hello [Normalize Data][normalize-data] module toohello right input port of hello second [Train Model][train-model] module.</span></span>

<span data-ttu-id="610e4-152">Tato část naší experiment by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="610e4-152">This portion of our experiment should now look something like this:</span></span>  

![Školení hello druhý modelu][2]  

<span data-ttu-id="610e4-154">Teď nakonfigurovat hello [normalizaci dat] [ normalize-data] modul:</span><span class="sxs-lookup"><span data-stu-id="610e4-154">Now configure hello [Normalize Data][normalize-data] module:</span></span>

1. <span data-ttu-id="610e4-155">Klikněte na tlačítko tooselect hello [normalizaci dat] [ normalize-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-155">Click tooselect hello [Normalize Data][normalize-data] module.</span></span> <span data-ttu-id="610e4-156">V hello **vlastnosti** podokně, vyberte **Tanh** pro hello **transformace metoda** parametr.</span><span class="sxs-lookup"><span data-stu-id="610e4-156">In hello **Properties** pane, select **Tanh** for hello **Transformation method** parameter.</span></span>

2. <span data-ttu-id="610e4-157">Klikněte na tlačítko **spustit selektor sloupců**, vyberte možnost "Žádné sloupce" pro **začít s**, vyberte **zahrnout** v prvním rozevíracím hello vyberte **typ sloupce**v hello druhý rozevírací seznam a vyberte **číselné** v rozevírací nabídce třetí hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-157">Click **Launch column selector**, select "No columns" for **Begin With**, select **Include** in hello first dropdown, select **column type** in hello second dropdown, and select **Numeric** in hello third dropdown.</span></span> <span data-ttu-id="610e4-158">Určuje transformaci všech hello číselné sloupce (a pouze číslice).</span><span class="sxs-lookup"><span data-stu-id="610e4-158">This specifies that all hello numeric columns (and only numeric) are transformed.</span></span>

3. <span data-ttu-id="610e4-159">Klikněte na tlačítko hello toohello znaménko plus (+) napravo od tento řádek – tím se vytvoří řádek rozevíracích seznamů.</span><span class="sxs-lookup"><span data-stu-id="610e4-159">Click hello plus sign (+) toohello right of this row - this creates a row of dropdowns.</span></span> <span data-ttu-id="610e4-160">Vyberte **vyloučit** v prvním rozevíracím hello vyberte **názvy sloupců** v hello druhý rozevíracího seznamu a zadejte "Úvěrového rizika" v hello textové pole.</span><span class="sxs-lookup"><span data-stu-id="610e4-160">Select **Exclude** in hello first dropdown, select **column names** in hello second dropdown, and enter "Credit risk" in hello text field.</span></span> <span data-ttu-id="610e4-161">Určuje sloupce úvěrové riziko hello třeba ji ignorovat (potřebujeme toodo to protože tento sloupec je číselné a Ano by transformován Pokud jsme ho nebylo vyloučit).</span><span class="sxs-lookup"><span data-stu-id="610e4-161">This specifies that hello Credit Risk column should be ignored (we need toodo this because this column is numeric and so would be transformed if we didn't exclude it).</span></span>

4. <span data-ttu-id="610e4-162">Klikněte na tlačítko hello **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="610e4-162">Click hello **OK** check mark.</span></span>  

    ![Vyberte sloupce pro modul normalizaci dat hello][5]

<span data-ttu-id="610e4-164">Hello [normalizaci dat] [ normalize-data] modul je nyní sada tooperform Tanh transformace na všechny číselné sloupce s výjimkou hello úvěrové riziko sloupce.</span><span class="sxs-lookup"><span data-stu-id="610e4-164">hello [Normalize Data][normalize-data] module is now set tooperform a Tanh transformation on all numeric columns except for hello Credit Risk column.</span></span>  

## <a name="score-and-evaluate-hello-models"></a><span data-ttu-id="610e4-165">Stanovení skóre a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="610e4-165">Score and evaluate hello models</span></span>

<span data-ttu-id="610e4-166">Používáme hello testování data, která byla oddělených hello [rozdělení dat] [ split] modulu tooscore naše trénované modely.</span><span class="sxs-lookup"><span data-stu-id="610e4-166">We use hello testing data that was separated out by hello [Split Data][split] module tooscore our trained models.</span></span> <span data-ttu-id="610e4-167">Jsme pak můžete porovnat výsledky hello hello dva modely toosee který vygenerovat lepší výsledky.</span><span class="sxs-lookup"><span data-stu-id="610e4-167">We can then compare hello results of hello two models toosee which generated better results.</span></span>  

### <a name="add-hello-score-model-modules"></a><span data-ttu-id="610e4-168">Přidání modulů Score Model hello</span><span class="sxs-lookup"><span data-stu-id="610e4-168">Add hello Score Model modules</span></span>

1. <span data-ttu-id="610e4-169">Najde hello [Score Model] [ score-model] modulu a přetáhněte ji na plátno hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-169">Find hello [Score Model][score-model] module and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="610e4-170">Připojit hello [Train Model] [ train-model] modul, který byl připojen toohello [Two-Class Boosted Decision Tree] [ two-class-boosted-decision-tree] levé vstup toohello modulu port hello [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-170">Connect hello [Train Model][train-model] module that's connected toohello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module toohello left input port of hello [Score Model][score-model] module.</span></span>

3. <span data-ttu-id="610e4-171">Připojit hello vpravo [spustit skript jazyka R] [ execute-r-script] modulu (naše testování data) toohello právo vstupní port hello [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-171">Connect hello right [Execute R Script][execute-r-script] module (our testing data) toohello right input port of hello [Score Model][score-model] module.</span></span>

    ![Modul určení skóre modelu připojení][6]
   
   <span data-ttu-id="610e4-173">Hello [Score Model] [ score-model] modulu může trvat teď hello platební informace z hello testování dat, spusťte ho pomocí modelu hello a porovnání hello předpovědi modelu hello generuje s hello skutečné úvěrového rizika sloupec v hello testování data.</span><span class="sxs-lookup"><span data-stu-id="610e4-173">hello [Score Model][score-model] module can now take hello credit information from hello testing data, run it through hello model, and compare hello predictions hello model generates with hello actual credit risk column in hello testing data.</span></span>

4. <span data-ttu-id="610e4-174">Zkopírujte a vložte hello [Score Model] [ score-model] modulu toocreate druhé kopie.</span><span class="sxs-lookup"><span data-stu-id="610e4-174">Copy and paste hello [Score Model][score-model] module toocreate a second copy.</span></span>

5. <span data-ttu-id="610e4-175">Připojit výstup hello hello SVM modelu (tedy hello výstupní port hello [Train Model] [ train-model] modul, který byl připojen toohello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu) toohello vstupní port hello druhý [Score Model] [ score-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-175">Connect hello output of hello SVM model (that is, hello output port of hello [Train Model][train-model] module that's connected toohello [Two-Class Support Vector Machine][two-class-support-vector-machine] module) toohello input port of hello second [Score Model][score-model] module.</span></span>

6. <span data-ttu-id="610e4-176">Pro hello SVM model máme toodo hello stejnou transformaci toohello testovací data, jako jsme to udělali toohello Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="610e4-176">For hello SVM model, we have toodo hello same transformation toohello test data as we did toohello training data.</span></span> <span data-ttu-id="610e4-177">Proto zkopírujte a vložte hello [normalizaci dat] [ normalize-data] modulu toocreate druhé kopie a připojte ho toohello vpravo [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-177">So copy and paste hello [Normalize Data][normalize-data] module toocreate a second copy and connect it toohello right [Execute R Script][execute-r-script] module.</span></span>

7. <span data-ttu-id="610e4-178">Druhý připojit hello levém výstup hello [normalizaci dat] [ normalize-data] modulu toohello právo vstupní port hello druhý [Score Model] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="610e4-178">Connect hello left output of hello second [Normalize Data][normalize-data] module toohello right input port of hello second [Score Model][score-model] module.</span></span>

    ![Obě Score Model moduly připojení][7]

### <a name="add-hello-evaluate-model-module"></a><span data-ttu-id="610e4-180">Přidání modulu Evaluate Model hello</span><span class="sxs-lookup"><span data-stu-id="610e4-180">Add hello Evaluate Model module</span></span>

<span data-ttu-id="610e4-181">tooevaluate hello oba vyhodnocování výsledky a porovnat je, používáme [Evaluate Model] [ evaluate-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-181">tooevaluate hello two scoring results and compare them, we use an [Evaluate Model][evaluate-model] module.</span></span>  

1. <span data-ttu-id="610e4-182">Najde hello [Evaluate Model] [ evaluate-model] modulu a přetáhněte ji na plátno hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-182">Find hello [Evaluate Model][evaluate-model] module and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="610e4-183">Připojit hello výstupní port modulu hello [Score Model] [ score-model] modulu přidružené hello boosted rozhodovacího stromu modelu vlevo toohello vstupní port hello [Evaluate Model] [ evaluate-model] modulu.</span><span class="sxs-lookup"><span data-stu-id="610e4-183">Connect hello output port of hello [Score Model][score-model] module associated with hello boosted decision tree model toohello left input port of hello [Evaluate Model][evaluate-model] module.</span></span>

3. <span data-ttu-id="610e4-184">Připojit hello jiných [Score Model] [ score-model] modulu toohello právo vstupní port.</span><span class="sxs-lookup"><span data-stu-id="610e4-184">Connect hello other [Score Model][score-model] module toohello right input port.</span></span>  

    ![Vyhodnocení modelu modulu připojení][8]

### <a name="run-hello-experiment-and-check-hello-results"></a><span data-ttu-id="610e4-186">Spusťte hello experiment a hello výsledky kontroly</span><span class="sxs-lookup"><span data-stu-id="610e4-186">Run hello experiment and check hello results</span></span>

<span data-ttu-id="610e4-187">toorun hello experiment, klikněte na tlačítko hello **spustit** tlačítko pod plátnem hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-187">toorun hello experiment, click hello **RUN** button below hello canvas.</span></span> <span data-ttu-id="610e4-188">To může trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="610e4-188">It may take a few minutes.</span></span> <span data-ttu-id="610e4-189">Na roztočený ukazatel na každý modul ukazuje, zda je spuštěná a poté zobrazí zelená značka zaškrtnutí po dokončení modulu hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-189">A spinning indicator on each module shows that it's running, and then a green check mark shows when hello module is finished.</span></span> <span data-ttu-id="610e4-190">Pokud všechny moduly hello zaškrtnutí, hello experimentu bylo dokončeno.</span><span class="sxs-lookup"><span data-stu-id="610e4-190">When all hello modules have a check mark, hello experiment has finished running.</span></span>

<span data-ttu-id="610e4-191">Hello experiment by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="610e4-191">hello experiment should now look something like this:</span></span>  

![Vyhodnocení obou modelů][3]

<span data-ttu-id="610e4-193">toocheck hello výsledky, klikněte na výstupní port hello hello [Evaluate Model] [ evaluate-model] modul a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="610e4-193">toocheck hello results, click hello output port of hello [Evaluate Model][evaluate-model] module and select **Visualize**.</span></span>  

<span data-ttu-id="610e4-194">Hello [Evaluate Model] [ evaluate-model] modulu vytvoří dvojici křivek a metriky, které umožňují toocompare hello výsledky dva modely scored hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-194">hello [Evaluate Model][evaluate-model] module produces a pair of curves and metrics that allow you toocompare hello results of hello two scored models.</span></span> <span data-ttu-id="610e4-195">Můžete zobrazit výsledky hello jako křivek příjemce operátor vlastnosti (ROC), křivek přesnost nebo odvolání nebo křivek navýšení.</span><span class="sxs-lookup"><span data-stu-id="610e4-195">You can view hello results as Receiver Operator Characteristic (ROC) curves, Precision/Recall curves, or Lift curves.</span></span> <span data-ttu-id="610e4-196">Další data zobrazí zahrnuje matice nejasnostem, kumulativní hodnoty pro oblast hello pod křivkou hello (AUC) a další metriky.</span><span class="sxs-lookup"><span data-stu-id="610e4-196">Additional data displayed includes a confusion matrix, cumulative values for hello area under hello curve (AUC), and other metrics.</span></span> <span data-ttu-id="610e4-197">Můžete změnit tak, že přesunutí hello posuvník doleva nebo doprava hello prahovou hodnotu a zjistit, jak ovlivňuje hello sadu metriky.</span><span class="sxs-lookup"><span data-stu-id="610e4-197">You can change hello threshold value by moving hello slider left or right and see how it affects hello set of metrics.</span></span>  

<span data-ttu-id="610e4-198">Klikněte na tlačítko toohello napravo od grafu hello **skóre pro Magnitudu datovou sadu** nebo **skóre pro Magnitudu toocompare datovou sadu** hello hello toohighlight přidružené křivky a toodisplay přidružené metriky níže.</span><span class="sxs-lookup"><span data-stu-id="610e4-198">toohello right of hello graph, click **Scored dataset** or **Scored dataset toocompare** toohighlight hello associated curve and toodisplay hello associated metrics below.</span></span> <span data-ttu-id="610e4-199">V legendě hello pro hello křivek, "Skóre pro Magnitudu datovou sadu" odpovídá toohello zbývajících vstupní port hello [Evaluate Model] [ evaluate-model] modul – v našem případě je to hello boosted rozhodovacího stromu modelu.</span><span class="sxs-lookup"><span data-stu-id="610e4-199">In hello legend for hello curves, "Scored dataset" corresponds toohello left input port of hello [Evaluate Model][evaluate-model] module - in our case, this is hello boosted decision tree model.</span></span> <span data-ttu-id="610e4-200">"Skóre pro magnitudu toocompare datovou sadu" odpovídá toohello pravý vstupní port - hello SVM modelu v našem případě.</span><span class="sxs-lookup"><span data-stu-id="610e4-200">"Scored dataset toocompare" corresponds toohello right input port - hello SVM model in our case.</span></span> <span data-ttu-id="610e4-201">Když kliknete na jednu z těchto popisky, hello křivky pro tento model je označený a odpovídající metriky hello se zobrazí, jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-201">When you click one of these labels, hello curve for that model is highlighted and hello corresponding metrics are displayed, as shown in hello following graphic.</span></span>  

![Křivek ROC pro modely][4]

<span data-ttu-id="610e4-203">Prověřením tyto hodnoty můžete rozhodnout, které model nejbližší toogiving hello výsledků, které hledáte.</span><span class="sxs-lookup"><span data-stu-id="610e4-203">By examining these values, you can decide which model is closest toogiving you hello results you're looking for.</span></span> <span data-ttu-id="610e4-204">Můžete se vrátit zpět a iterovat experimentu změnou hodnoty parametrů v různých modelech hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-204">You can go back and iterate on your experiment by changing parameter values in hello different models.</span></span> 

<span data-ttu-id="610e4-205">Hello vědy a obrázky interpretace těchto výsledků a ladění výkonu modelu hello je mimo rámec tohoto návodu hello.</span><span class="sxs-lookup"><span data-stu-id="610e4-205">hello science and art of interpreting these results and tuning hello model performance is outside hello scope of this walkthrough.</span></span> <span data-ttu-id="610e4-206">Potřebujete další pomoc můžou číst hello následující články:</span><span class="sxs-lookup"><span data-stu-id="610e4-206">For additional help, you might read hello following articles:</span></span>
- [<span data-ttu-id="610e4-207">Jak tooevaluate modelu výkon v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="610e4-207">How tooevaluate model performance in Azure Machine Learning</span></span>](machine-learning-evaluate-model-performance.md)
- [<span data-ttu-id="610e4-208">Vyberte parametry toooptimize algoritmy v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="610e4-208">Choose parameters toooptimize your algorithms in Azure Machine Learning</span></span>](machine-learning-algorithm-parameters-optimize.md)
- [<span data-ttu-id="610e4-209">Interpretovat výsledky modelu v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="610e4-209">Interpret model results in Azure Machine Learning</span></span>](machine-learning-interpret-model-results.md)

> [!TIP]
> <span data-ttu-id="610e4-210">Pokaždé, když spustíte hello experimentu záznam této iterace je uložen v hello spustit historie.</span><span class="sxs-lookup"><span data-stu-id="610e4-210">Each time you run hello experiment a record of that iteration is kept in hello Run History.</span></span> <span data-ttu-id="610e4-211">Můžete zobrazit tyto iterací a vrátí tooany z nich, kliknutím na **zobrazit HISTORII BĚHŮ** níže hello plátno.</span><span class="sxs-lookup"><span data-stu-id="610e4-211">You can view these iterations, and return tooany of them, by clicking **VIEW RUN HISTORY** below hello canvas.</span></span> <span data-ttu-id="610e4-212">Můžete také kliknout na **předchozí spustit** v hello **vlastnosti** podokně tooreturn toohello iterace bezprostředně předcházející hello jeden máte otevřený.</span><span class="sxs-lookup"><span data-stu-id="610e4-212">You can also click **Prior Run** in hello **Properties** pane tooreturn toohello iteration immediately preceding hello one you have open.</span></span>
> 
> <span data-ttu-id="610e4-213">Kliknutím můžete vytvořit kopii kteroukoli iteraci experimentu **uložit jako** níže hello plátno.</span><span class="sxs-lookup"><span data-stu-id="610e4-213">You can make a copy of any iteration of your experiment by clicking **SAVE AS** below hello canvas.</span></span> 
> <span data-ttu-id="610e4-214">Použít hello experimentu **Souhrn** a **popis** vlastnosti tookeep záznam jste zkusili ve vašem iterací experimentu.</span><span class="sxs-lookup"><span data-stu-id="610e4-214">Use hello experiment's **Summary** and **Description** properties tookeep a record of what you've tried in your experiment iterations.</span></span>
> 
> <span data-ttu-id="610e4-215">Další podrobnosti najdete v tématu [Správa iterací experimentu v nástroji Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span><span class="sxs-lookup"><span data-stu-id="610e4-215">For more details, see [Manage experiment iterations in Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span></span>  
> 
> 

- - -
<span data-ttu-id="610e4-216">**Další krok: [nasazení hello webové služby](machine-learning-walkthrough-5-publish-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="610e4-216">**Next: [Deploy hello web service](machine-learning-walkthrough-5-publish-web-service.md)**</span></span>

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
