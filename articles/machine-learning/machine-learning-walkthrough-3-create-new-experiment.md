---
title: "Krok 3: Vytvoření nového experimentu Machine Learning | Microsoft Docs"
description: "Krok 3 hello vývoj prediktivního řešení návod: vytvoření nového experimentu školení v Azure Machine Learning Studio."
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
ms.openlocfilehash: 4697d461a205c50c8d2aa6a3bd56697840cb30f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a><span data-ttu-id="89649-103">Krok 3 průvodce: Vytvoření nového experimentu služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="89649-103">Walkthrough Step 3: Create a new Azure Machine Learning experiment</span></span>
<span data-ttu-id="89649-104">Toto je třetí krok hello hello návod [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="89649-104">This is hello third step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="89649-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="89649-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="89649-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="89649-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. <span data-ttu-id="89649-107">**Vytvoření nového experimentu**</span><span class="sxs-lookup"><span data-stu-id="89649-107">**Create a new experiment**</span></span>
4. [<span data-ttu-id="89649-108">Natrénování a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="89649-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="89649-109">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="89649-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="89649-110">Přístup k webové službě hello</span><span class="sxs-lookup"><span data-stu-id="89649-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="89649-111">dalším krokem Hello v tomto návodu je toocreate experimentu v Machine Learning Studio používající hello datovou sadu, které jsme nahrát.</span><span class="sxs-lookup"><span data-stu-id="89649-111">hello next step in this walkthrough is toocreate an experiment in Machine Learning Studio that uses hello dataset we uploaded.</span></span>  

1. <span data-ttu-id="89649-112">V nástroji Studio klikněte na tlačítko **+ nový** v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="89649-112">In Studio, click **+NEW** at hello bottom of hello window.</span></span>
2. <span data-ttu-id="89649-113">Vyberte **EXPERIMENTU**a potom vyberte "Prázdný Experiment".</span><span class="sxs-lookup"><span data-stu-id="89649-113">Select **EXPERIMENT**, and then select "Blank Experiment".</span></span> 

    ![Vytvoření nového experimentu][0]

2. <span data-ttu-id="89649-115">Vyberte hello výchozí experimentovat název hello horní části plátna hello a přejmenujte ji toosomething smysluplný.</span><span class="sxs-lookup"><span data-stu-id="89649-115">Select hello default experiment name at hello top of hello canvas and rename it toosomething meaningful.</span></span>

    ![Přejmenujte experimentu][5]
   
   > [!TIP]
   > <span data-ttu-id="89649-117">Je dobrým zvykem toofill v **Souhrn** a **popis** hello experimentu v hello **vlastnosti** podokně.</span><span class="sxs-lookup"><span data-stu-id="89649-117">It's a good practice toofill in **Summary** and **Description** for hello experiment in hello **Properties** pane.</span></span> <span data-ttu-id="89649-118">Tyto vlastnosti udělení hello prvního toodocument hello experimentu tak, aby každý, kdo ho později zabývá pochopili záměry a metody.</span><span class="sxs-lookup"><span data-stu-id="89649-118">These properties give you hello chance toodocument hello experiment so that anyone who looks at it later will understand your goals and methodology.</span></span>
   > 
   > ![Vlastnosti experimentu][6]
   > 
3. <span data-ttu-id="89649-120">V hello modulu palety toohello nalevo od plátna experimentu hello, rozbalte položku **uložit datové sady**.</span><span class="sxs-lookup"><span data-stu-id="89649-120">In hello module palette toohello left of hello experiment canvas, expand **Saved Datasets**.</span></span>
4. <span data-ttu-id="89649-121">Datová sada hello najít, které jste vytvořili v části **Moje datové sady** a přetáhněte ji na plátno hello.</span><span class="sxs-lookup"><span data-stu-id="89649-121">Find hello dataset you created under **My Datasets** and drag it onto hello canvas.</span></span> <span data-ttu-id="89649-122">Můžete také najít datovou sadu hello tak, že zadáte název hello hello **vyhledávání** pole výše palety hello.</span><span class="sxs-lookup"><span data-stu-id="89649-122">You can also find hello dataset by entering hello name in hello **Search** box above hello palette.</span></span>  

    ![Přidat hello datovou sadu toohello experimentu][7]

## <a name="prepare-hello-data"></a><span data-ttu-id="89649-124">Příprava dat hello</span><span class="sxs-lookup"><span data-stu-id="89649-124">Prepare hello data</span></span>
<span data-ttu-id="89649-125">Můžete zobrazit hello prvních 100 řádků hello dat a některé statistické informace pro celou datovou sadu hello: klikněte na výstupní port hello hello sady dat (přeškrtnutým kroužkem pro hello dolnímu hello) a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="89649-125">You can view hello first 100 rows of hello data and some statistical information for hello whole dataset: Click hello output port of hello dataset (hello small circle at hello bottom) and select **Visualize**.</span></span>  

<span data-ttu-id="89649-126">Protože hello datový soubor nebyl dodán záhlaví sloupců, Studio poskytl obecné záhlaví (Sloupec1, Sloupec2, *atd*).</span><span class="sxs-lookup"><span data-stu-id="89649-126">Because hello data file didn't come with column headings, Studio has provided generic headings (Col1, Col2, *etc.*).</span></span> <span data-ttu-id="89649-127">Základní toocreating model nejsou funkční záhlaví, ale jeho udělají jednodušší toowork daty hello v experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="89649-127">Good headings aren't essential toocreating a model, but they make it easier toowork with hello data in hello experiment.</span></span> <span data-ttu-id="89649-128">Navíc pokud jsme nakonec publikovat tento model ve webové službě, záhlaví hello pomoci při identifikaci hello sloupce toohello uživatel hello služby.</span><span class="sxs-lookup"><span data-stu-id="89649-128">Also, when we eventually publish this model in a web service, hello headings help identify hello columns toohello user of hello service.</span></span>  

<span data-ttu-id="89649-129">Nyní můžete přidat pomocí hello záhlaví sloupců [upravit Metadata] [ edit-metadata] modulu.</span><span class="sxs-lookup"><span data-stu-id="89649-129">We can add column headings using hello [Edit Metadata][edit-metadata] module.</span></span>
<span data-ttu-id="89649-130">Použít hello [upravit Metadata] [ edit-metadata] modulu toochange metadata spojená s datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="89649-130">You use hello [Edit Metadata][edit-metadata] module toochange metadata associated with a dataset.</span></span> <span data-ttu-id="89649-131">V tomto případě používáme ho tooprovide další popisný názvy pro záhlaví sloupců.</span><span class="sxs-lookup"><span data-stu-id="89649-131">In this case, we use it tooprovide more friendly names for column headings.</span></span> 

<span data-ttu-id="89649-132">toouse [upravit Metadata][edit-metadata], nejprve zadat které toomodify sloupce (v tomto případě všechny z nich.) Dále určit toobe hello akce provést na tyto sloupce (v tomto případě změna záhlaví sloupců.)</span><span class="sxs-lookup"><span data-stu-id="89649-132">toouse [Edit Metadata][edit-metadata], you first specify which columns toomodify (in this case, all of them.) Next, you specify hello action toobe performed on those columns (in this case, changing column headings.)</span></span>

1. <span data-ttu-id="89649-133">V modulu palety hello, zadejte "metadata" v hello **vyhledávání** pole.</span><span class="sxs-lookup"><span data-stu-id="89649-133">In hello module palette, type "metadata" in hello **Search** box.</span></span> <span data-ttu-id="89649-134">Hello [upravit Metadata] [ edit-metadata] se zobrazí v seznamu modul hello.</span><span class="sxs-lookup"><span data-stu-id="89649-134">hello [Edit Metadata][edit-metadata] appears in hello module list.</span></span>

2. <span data-ttu-id="89649-135">Klikněte a přetáhněte hello [upravit Metadata] [ edit-metadata] modulu do hello plátno a umístěte jej pod datovou sadu hello jsme přidali dříve.</span><span class="sxs-lookup"><span data-stu-id="89649-135">Click and drag hello [Edit Metadata][edit-metadata] module onto hello canvas and drop it below hello dataset we added earlier.</span></span>

3. <span data-ttu-id="89649-136">Připojit hello datovou sadu toohello [upravit Metadata][edit-metadata]: klikněte na výstupní port hello hello sady dat (přeškrtnutým kroužkem pro hello dole hello datovou sadu hello), přetáhněte toohello vstupní port [upravit Metadata ] [ edit-metadata] (hello přeškrtnutým kroužkem hello horní části modulu hello), pak uvolnění tlačítka myši hello.</span><span class="sxs-lookup"><span data-stu-id="89649-136">Connect hello dataset toohello [Edit Metadata][edit-metadata]: click hello output port of hello dataset (hello small circle at hello bottom of hello dataset), drag toohello input port of [Edit Metadata][edit-metadata] (hello small circle at hello top of hello module), then release hello mouse button.</span></span> <span data-ttu-id="89649-137">datovou sadu Hello a modul zůstanou připojené, i když přesouváte buď na plátně hello.</span><span class="sxs-lookup"><span data-stu-id="89649-137">hello dataset and module remain connected even if you move either around on hello canvas.</span></span>
   
   <span data-ttu-id="89649-138">Hello experiment by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="89649-138">hello experiment should now look something like this:</span></span>  
   
   ![Přidání metadat úpravy][1]
   
   <span data-ttu-id="89649-140">Hello červený vykřičník označuje, že jsme hello vlastnosti pro tento modul nenastavili ještě.</span><span class="sxs-lookup"><span data-stu-id="89649-140">hello red exclamation mark indicates that we haven't set hello properties for this module yet.</span></span> <span data-ttu-id="89649-141">Provedeme tento další.</span><span class="sxs-lookup"><span data-stu-id="89649-141">We'll do that next.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="89649-142">Můžete přidat komentář modul tooa tak, že dvakrát kliknete na panel hello modulu a zadání textu.</span><span class="sxs-lookup"><span data-stu-id="89649-142">You can add a comment tooa module by double-clicking hello module and entering text.</span></span> <span data-ttu-id="89649-143">Díky tomu můžete ihned uvidíte, jaké hello modul provádí v experimentu.</span><span class="sxs-lookup"><span data-stu-id="89649-143">This can help you see at a glance what hello module is doing in your experiment.</span></span> <span data-ttu-id="89649-144">V takovém případě klikněte dvakrát na hello [upravit Metadata] [ edit-metadata] modulu a typu hello komentář "přidat záhlaví sloupců".</span><span class="sxs-lookup"><span data-stu-id="89649-144">In this case, double-click hello [Edit Metadata][edit-metadata] module and type hello comment "Add column headings".</span></span> <span data-ttu-id="89649-145">Kdekoliv jinde, klikněte na hello plátno tooclose hello textové pole.</span><span class="sxs-lookup"><span data-stu-id="89649-145">Click anywhere else on hello canvas tooclose hello text box.</span></span> <span data-ttu-id="89649-146">toodisplay hello komentář, klikněte na šipku dolů hello v modulu hello.</span><span class="sxs-lookup"><span data-stu-id="89649-146">toodisplay hello comment, click hello down-arrow on hello module.</span></span>
   > 
   > ![Upravit komentář přidat Metadata modulu][8]
   > 
4. <span data-ttu-id="89649-148">Vyberte [upravit Metadata][edit-metadata]a v hello **vlastnosti** toohello podokně napravo od plátna hello, klikněte na tlačítko **spustit selektor sloupců**.</span><span class="sxs-lookup"><span data-stu-id="89649-148">Select [Edit Metadata][edit-metadata], and in hello **Properties** pane toohello right of hello canvas, click **Launch column selector**.</span></span>

5. <span data-ttu-id="89649-149">V hello **vyberte sloupce** dialogové okno, vyberte všechny řádky v hello **dostupné sloupce** a klikněte na tlačítko > toomove je příliš**vybrané sloupce**.</span><span class="sxs-lookup"><span data-stu-id="89649-149">In hello **Select columns** dialog, select all hello rows in **Available Columns** and click > toomove them too**Selected Columns**.</span></span>
   <span data-ttu-id="89649-150">Dialogové okno Hello by měl vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="89649-150">hello dialog should look like this:</span></span>

   ![Selektor sloupců s vybrané všechny sloupce][2]

6. <span data-ttu-id="89649-152">Klikněte na tlačítko hello **OK** zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="89649-152">Click hello **OK** check mark.</span></span>

7. <span data-ttu-id="89649-153">Zpět v hello **vlastnosti** podokně, podívejte se na hello **nové názvy sloupců** parametr.</span><span class="sxs-lookup"><span data-stu-id="89649-153">Back in hello **Properties** pane, look for hello **New column names** parameter.</span></span> <span data-ttu-id="89649-154">V tomto poli zadejte seznam názvů pro hello 21 sloupců v datové sadě hello, oddělených čárkami a pořadí sloupců.</span><span class="sxs-lookup"><span data-stu-id="89649-154">In this field, enter a list of names for hello 21 columns in hello dataset, separated by commas and in column order.</span></span> <span data-ttu-id="89649-155">Názvy sloupců hello můžete získat z dokumentace hello datové sady na webu UCI hello nebo ke zvýšení pohodlí můžete zkopírovat a vložit hello následující seznamu:</span><span class="sxs-lookup"><span data-stu-id="89649-155">You can obtain hello columns names from hello dataset documentation on hello UCI website, or for convenience you can copy and paste hello following list:</span></span>  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   <span data-ttu-id="89649-156">Podokno Properties Hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="89649-156">hello Properties pane looks like this:</span></span>
   
   ![Vlastnosti pro úpravy metadat][3]

> [!TIP]
> <span data-ttu-id="89649-158">Pokud chcete záhlaví sloupců hello tooverify, spustit hello experiment (klikněte na tlačítko **spustit** pod plátnem experimentu hello).</span><span class="sxs-lookup"><span data-stu-id="89649-158">If you want tooverify hello column headings, run hello experiment (click **RUN** below hello experiment canvas).</span></span> <span data-ttu-id="89649-159">Po jeho dokončení spuštění (zelená značka zaškrtnutí se zobrazí na [upravit Metadata][edit-metadata]), klikněte na výstupní port hello hello [upravit Metadata] [ edit-metadata] modul a vyberte **vizualizovat**.</span><span class="sxs-lookup"><span data-stu-id="89649-159">When it finishes running (a green check mark appears on [Edit Metadata][edit-metadata]), click hello output port of hello [Edit Metadata][edit-metadata] module, and select **Visualize**.</span></span> <span data-ttu-id="89649-160">Zobrazí se výstup hello libovolný modul v hello tooview způsob stejné hello průběh hello data prostřednictvím hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="89649-160">You can view hello output of any module in hello same way tooview hello progress of hello data through hello experiment.</span></span>
> 
> 

## <a name="create-training-and-test-datasets"></a><span data-ttu-id="89649-161">Vytvoření školení a testovací datové sady</span><span class="sxs-lookup"><span data-stu-id="89649-161">Create training and test datasets</span></span>
<span data-ttu-id="89649-162">Potřebujeme některé datový model hello tootrain a některé tootest ho.</span><span class="sxs-lookup"><span data-stu-id="89649-162">We need some data tootrain hello model and some tootest it.</span></span>
<span data-ttu-id="89649-163">Proto v dalším kroku hello pokusu se hello jsme datovou sadu hello rozdělit na dvě samostatné datové sady: jeden pro cvičení našeho modelu a jeden pro testování ho.</span><span class="sxs-lookup"><span data-stu-id="89649-163">So in hello next step of hello experiment, we split hello dataset into two separate datasets: one for training our model and one for testing it.</span></span>

<span data-ttu-id="89649-164">toodo toho používáme hello [rozdělení dat] [ split] modulu.</span><span class="sxs-lookup"><span data-stu-id="89649-164">toodo this, we use hello [Split Data][split] module.</span></span>  

1. <span data-ttu-id="89649-165">Najde hello [rozdělení dat] [ split] modulu, přetáhněte na plátno hello a připojte ho toohello [upravit Metadata] [ edit-metadata] modulu.</span><span class="sxs-lookup"><span data-stu-id="89649-165">Find hello [Split Data][split] module, drag it onto hello canvas, and connect it toohello [Edit Metadata][edit-metadata] module.</span></span>

2. <span data-ttu-id="89649-166">Ve výchozím nastavení, je poměr rozdělení hello 0,5 a hello **Randomized rozdělení** parametr je nastaven.</span><span class="sxs-lookup"><span data-stu-id="89649-166">By default, hello split ratio is 0.5 and hello **Randomized split** parameter is set.</span></span> <span data-ttu-id="89649-167">Znamená, že půl náhodných hello dat je výstup prostřednictvím jednoho portu hello [rozdělení dat] [ split] modulu a půl až hello jiné.</span><span class="sxs-lookup"><span data-stu-id="89649-167">This means that a random half of hello data is output through one port of hello [Split Data][split] module, and half through hello other.</span></span> <span data-ttu-id="89649-168">Vám může upravit tyto parametry, stejně jako hello **náhodná počáteční hodnota** parametr toochange hello rozdělit mezi trénování a testování data.</span><span class="sxs-lookup"><span data-stu-id="89649-168">You can adjust these parameters, as well as hello **Random seed** parameter, toochange hello split between training and testing data.</span></span> <span data-ttu-id="89649-169">V tomto příkladu jsme je jako ponechte-je.</span><span class="sxs-lookup"><span data-stu-id="89649-169">For this example, we leave them as-is.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="89649-170">Hello vlastnost **podíl řádků v hello nejprve výstupní datovou sadu** Určuje, kolik dat hello je výstup prostřednictvím hello *levém* výstupní port.</span><span class="sxs-lookup"><span data-stu-id="89649-170">hello property **Fraction of rows in hello first output dataset** determines how much of hello data is output through hello *left* output port.</span></span> <span data-ttu-id="89649-171">Například pokud nastavíte too0.7 poměr hello, 70 % hello dat je výstup prostřednictvím hello zbývajících port a z 30 % prostřednictvím hello pravý port.</span><span class="sxs-lookup"><span data-stu-id="89649-171">For instance, if you set hello ratio too0.7, then 70% of hello data is output through hello left port and 30% through hello right port.</span></span>  
   > 
   > 

3. <span data-ttu-id="89649-172">Klikněte dvakrát na hello [rozdělení dat] [ split] modul a zadejte komentář hello, "školení nebo testování dat rozdělení 50 %".</span><span class="sxs-lookup"><span data-stu-id="89649-172">Double-click hello [Split Data][split] module and enter hello comment, "Training/testing data split 50%".</span></span> 

<span data-ttu-id="89649-173">Můžeme použít hello výstupy hello [rozdělení dat] [ split] modulu jsme jako, ale umožňuje vybrat toouse hello levý výstupní jako Cvičná data a hello právo výstupní data jako testování.</span><span class="sxs-lookup"><span data-stu-id="89649-173">We can use hello outputs of hello [Split Data][split] module however we like, but let's choose toouse hello left output as training data and hello right output as testing data.</span></span>  

<span data-ttu-id="89649-174">Jak je uvedeno v hello [předchozího kroku](machine-learning-walkthrough-2-upload-data.md), misclassifying vysokou úvěrové riziko jako nízké náklady hello je pětkrát vyšší než náklady hello misclassifying nízkou úvěrové riziko jako Vysoká.</span><span class="sxs-lookup"><span data-stu-id="89649-174">As mentioned in hello [previous step](machine-learning-walkthrough-2-upload-data.md), hello cost of misclassifying a high credit risk as low is five times higher than hello cost of misclassifying a low credit risk as high.</span></span> <span data-ttu-id="89649-175">tooaccount pro to, se vygeneruje nová datová sada, která odráží tuto funkci náklady.</span><span class="sxs-lookup"><span data-stu-id="89649-175">tooaccount for this, we generate a new dataset that reflects this cost function.</span></span> <span data-ttu-id="89649-176">V hello nová datová sada každý příklad vysoce rizikové replikují pětkrát, zatímco se replikují každý příklad nízké riziko.</span><span class="sxs-lookup"><span data-stu-id="89649-176">In hello new dataset, each high risk example is replicated five times, while each low risk example is not replicated.</span></span>   

<span data-ttu-id="89649-177">Provedeme tato replikace pomocí kódu jazyka R:</span><span class="sxs-lookup"><span data-stu-id="89649-177">We can do this replication using R code:</span></span>  

1. <span data-ttu-id="89649-178">Najděte a přetáhněte hello [spustit skript jazyka R] [ execute-r-script] modulu na plátno experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="89649-178">Find and drag hello [Execute R Script][execute-r-script] module onto hello experiment canvas.</span></span> 

2. <span data-ttu-id="89649-179">Připojte zbývající výstupní port modulu hello hello [rozdělení dat] [ split] modulu toohello první vstup port ("Dataset1") z hello [spustit skript jazyka R] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="89649-179">Connect hello left output port of hello [Split Data][split] module toohello first input port ("Dataset1") of hello [Execute R Script][execute-r-script] module.</span></span>

3. <span data-ttu-id="89649-180">Klikněte dvakrát na hello [spustit skript jazyka R] [ execute-r-script] modul a zadejte komentář hello "Sada úprava nákladů".</span><span class="sxs-lookup"><span data-stu-id="89649-180">Double-click hello [Execute R Script][execute-r-script] module and enter hello comment, "Set cost adjustment".</span></span>

4. <span data-ttu-id="89649-181">V hello **vlastnosti** podokně odstranění hello výchozí text v hello **skript jazyka R** parametr a zadejte tento skript:</span><span class="sxs-lookup"><span data-stu-id="89649-181">In hello **Properties** pane, delete hello default text in hello **R Script** parameter and enter this script:</span></span>
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R skript v modulu hello spustit skript jazyka R][9]

<span data-ttu-id="89649-183">Potřebujeme toodo stejná operace replikace pro každé výstup z hello [rozdělení dat] [ split] modulu tak, že hello trénování a testování dat mají hello stejné úprava nákladů.</span><span class="sxs-lookup"><span data-stu-id="89649-183">We need toodo this same replication operation for each output of hello [Split Data][split] module so that hello training and testing data have hello same cost adjustment.</span></span> <span data-ttu-id="89649-184">Nejjednodušší způsob, jak toodo jedná se o duplikování hello Hello [spustit skript jazyka R] [ execute-r-script] modulu jsme právě provedli a jejím propojením toohello jiné výstupní port hello [rozdělení dat] [ split] modulu.</span><span class="sxs-lookup"><span data-stu-id="89649-184">hello easiest way toodo this is by duplicating hello [Execute R Script][execute-r-script] module we just made and connecting it toohello other output port of hello [Split Data][split] module.</span></span>

1. <span data-ttu-id="89649-185">Klikněte pravým tlačítkem na hello [spustit skript jazyka R] [ execute-r-script] modul a vyberte **kopie**.</span><span class="sxs-lookup"><span data-stu-id="89649-185">Right-click hello [Execute R Script][execute-r-script] module and select **Copy**.</span></span>

2. <span data-ttu-id="89649-186">Klikněte pravým tlačítkem na plátno experimentu hello a vyberte **vložení**.</span><span class="sxs-lookup"><span data-stu-id="89649-186">Right-click hello experiment canvas and select **Paste**.</span></span>

3. <span data-ttu-id="89649-187">Přetáhněte hello nového modulu do pozice a potom se připojte hello správné výstupní port modulu hello [rozdělení dat] [ split] modulu toohello nové první vstup portu tohoto [spustit skript jazyka R] [ execute-r-script] modulu.</span><span class="sxs-lookup"><span data-stu-id="89649-187">Drag hello new module into position, and then connect hello right output port of hello [Split Data][split] module toohello first input port of this new [Execute R Script][execute-r-script] module.</span></span> 

4. <span data-ttu-id="89649-188">Hello dolní části plátna hello, klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="89649-188">At hello bottom of hello canvas, click **Run**.</span></span> 

> [!TIP]
> <span data-ttu-id="89649-189">Hello kopii modulu spustit skript jazyka R hello obsahuje hello stejné jako původní modul hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="89649-189">hello copy of hello Execute R Script module contains hello same script as hello original module.</span></span> <span data-ttu-id="89649-190">Při kopírování a vkládání modul na plátno hello hello kopie uchovává všechny vlastnosti hello hello původní.</span><span class="sxs-lookup"><span data-stu-id="89649-190">When you copy and paste a module on hello canvas, hello copy retains all hello properties of hello original.</span></span>  
> 
> 

<span data-ttu-id="89649-191">Naše experimentu nyní vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="89649-191">Our experiment now looks something like this:</span></span>

![Přidání modulu rozdělení a skripty R][4]

<span data-ttu-id="89649-193">Další informace o použití skripty R v experimentů najdete v tématu [rozšíření experimentů pomocí R](machine-learning-extend-your-experiment-with-r.md).</span><span class="sxs-lookup"><span data-stu-id="89649-193">For more information on using R scripts in your experiments, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md).</span></span>

<span data-ttu-id="89649-194">**Další krok: [Train a vyhodnocení modelů hello](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span><span class="sxs-lookup"><span data-stu-id="89649-194">**Next: [Train and evaluate hello models](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span></span>

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
