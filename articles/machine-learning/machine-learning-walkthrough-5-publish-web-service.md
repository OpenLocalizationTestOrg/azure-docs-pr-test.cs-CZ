---
title: "Krok 5: Nasazení webové službě Machine Learning hello | Microsoft Docs"
description: "Krok 5 hello vývoj prediktivního řešení návod: nasazení prediktivní experiment v nástroji Machine Learning Studio jako webovou službu."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 76391010972ed1450bbda8bfb2352c7b22b51ccc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-5-deploy-hello-azure-machine-learning-web-service"></a><span data-ttu-id="8654d-103">Návod krok 5: Nasazení webové služby Azure Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="8654d-103">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>
<span data-ttu-id="8654d-104">Toto je pátý krok hello hello průvodce [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="8654d-104">This is hello fifth step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="8654d-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="8654d-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="8654d-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="8654d-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="8654d-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="8654d-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="8654d-108">Natrénování a vyhodnocení modelů hello</span><span class="sxs-lookup"><span data-stu-id="8654d-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. <span data-ttu-id="8654d-109">**Nasazení webové služby hello**</span><span class="sxs-lookup"><span data-stu-id="8654d-109">**Deploy hello web service**</span></span>
6. [<span data-ttu-id="8654d-110">Přístup hello webové služby</span><span class="sxs-lookup"><span data-stu-id="8654d-110">Access hello web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="8654d-111">toogive jiné prvního toouse hello prediktivního modelu jsme jste vytvořili v tomto návodu, jsme můžete nasadit jako webovou službu v Azure.</span><span class="sxs-lookup"><span data-stu-id="8654d-111">toogive others a chance toouse hello predictive model we've developed in this walkthrough, we can deploy it as a web service on Azure.</span></span>

<span data-ttu-id="8654d-112">Až toothis bodu jsme jste byla experimentování se cvičení naše modelu.</span><span class="sxs-lookup"><span data-stu-id="8654d-112">Up toothis point we've been experimenting with training our model.</span></span> <span data-ttu-id="8654d-113">Ale hello nasazení služby se už bude toodo školení – přechází nové předpovědi toogenerate ve vyhodnocování vstup hello uživatele na základě naše modelu.</span><span class="sxs-lookup"><span data-stu-id="8654d-113">But hello deployed service is no longer going toodo training - it's going toogenerate new predictions by scoring hello user's input based on our model.</span></span> <span data-ttu-id="8654d-114">Takže vytvoříme toodo některé přípravy tooconvert, to experimentovat z ***školení*** experimentovat tooa ***prediktivní*** experiment.</span><span class="sxs-lookup"><span data-stu-id="8654d-114">So we're going toodo some preparation tooconvert this experiment from a ***training*** experiment tooa ***predictive*** experiment.</span></span> 

<span data-ttu-id="8654d-115">Jedná se o proces třech krocích:</span><span class="sxs-lookup"><span data-stu-id="8654d-115">This is a three-step process:</span></span>  

1. <span data-ttu-id="8654d-116">Odeberte jeden z modelů hello</span><span class="sxs-lookup"><span data-stu-id="8654d-116">Remove one of hello models</span></span>
2. <span data-ttu-id="8654d-117">Převést hello *výukový experiment* vytvořili jsme do *prediktivní experiment*</span><span class="sxs-lookup"><span data-stu-id="8654d-117">Convert hello *training experiment* we've created into a *predictive experiment*</span></span>
3. <span data-ttu-id="8654d-118">Nasadit prediktivní experiment hello jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="8654d-118">Deploy hello predictive experiment as a web service</span></span>

## <a name="remove-one-of-hello-models"></a><span data-ttu-id="8654d-119">Odeberte jeden z modelů hello</span><span class="sxs-lookup"><span data-stu-id="8654d-119">Remove one of hello models</span></span>

<span data-ttu-id="8654d-120">Nejdřív potřebujeme tootrim tohoto experimentu dolů trochu.</span><span class="sxs-lookup"><span data-stu-id="8654d-120">First, we need tootrim this experiment down a little.</span></span> <span data-ttu-id="8654d-121">V současné době máme dva různé modely v experimentu hello, ale chceme jenom jeden model toouse, když jsme nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8654d-121">We currently have two different models in hello experiment, but we only want toouse one model when we deploy this as a web service.</span></span>  

<span data-ttu-id="8654d-122">Řekněme, že jsme jste se rozhodli, že hello boosted stromu model má lepší než hello SVM model.</span><span class="sxs-lookup"><span data-stu-id="8654d-122">Let's say we've decided that hello boosted tree model performed better than hello SVM model.</span></span> <span data-ttu-id="8654d-123">První věc toodo hello je odebrat hello [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu a hello moduly, které se používaly k cvičení ho.</span><span class="sxs-lookup"><span data-stu-id="8654d-123">So hello first thing toodo is remove hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module and hello modules that were used for training it.</span></span> <span data-ttu-id="8654d-124">Může být vhodné toomake kopii hello experimentu nejprve kliknutím **uložit jako** dole hello hello experimentovat plátno.</span><span class="sxs-lookup"><span data-stu-id="8654d-124">You may want toomake a copy of hello experiment first by clicking **Save As** at hello bottom of hello experiment canvas.</span></span>

<span data-ttu-id="8654d-125">Potřebujeme toodelete hello následující moduly:</span><span class="sxs-lookup"><span data-stu-id="8654d-125">We need toodelete hello following modules:</span></span>  

* <span data-ttu-id="8654d-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span><span class="sxs-lookup"><span data-stu-id="8654d-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span></span>
* <span data-ttu-id="8654d-127">[Cvičení modelu] [ train-model] a [Score Model] [ score-model] moduly, které byly připojené tooit</span><span class="sxs-lookup"><span data-stu-id="8654d-127">[Train Model][train-model] and [Score Model][score-model] modules that were connected tooit</span></span>
* <span data-ttu-id="8654d-128">[Normalizuje Data] [ normalize-data] (obou z nich)</span><span class="sxs-lookup"><span data-stu-id="8654d-128">[Normalize Data][normalize-data] (both of them)</span></span>
* <span data-ttu-id="8654d-129">[Vyhodnocení modelu] [ evaluate-model] (protože jsme hotovi, vyhodnocení modelů hello)</span><span class="sxs-lookup"><span data-stu-id="8654d-129">[Evaluate Model][evaluate-model] (because we're finished evaluating hello models)</span></span>

<span data-ttu-id="8654d-130">Vyberte každý modul a stisknout klávesu odstranit hello nebo modul hello klikněte pravým tlačítkem a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="8654d-130">Select each module and press hello Delete key, or right-click hello module and select **Delete**.</span></span> 

![Odebrat hello SVM modelu][3a]

<span data-ttu-id="8654d-132">Naše modelu by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="8654d-132">Our model should now look something like this:</span></span>

![Odebrat hello SVM modelu][3]

<span data-ttu-id="8654d-134">Teď máme připraven toodeploy to modelu pomocí hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span><span class="sxs-lookup"><span data-stu-id="8654d-134">Now we're ready toodeploy this model using hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span></span>

## <a name="convert-hello-training-experiment-tooa-predictive-experiment"></a><span data-ttu-id="8654d-135">Převést hello výukový experiment tooa prediktivní experiment</span><span class="sxs-lookup"><span data-stu-id="8654d-135">Convert hello training experiment tooa predictive experiment</span></span>

<span data-ttu-id="8654d-136">tooget to model připravené pro nasazení, budeme potřebovat tooconvert cvičení prediktivní experiment tooa experimentu.</span><span class="sxs-lookup"><span data-stu-id="8654d-136">tooget this model ready for deployment, we need tooconvert this training experiment tooa predictive experiment.</span></span> <span data-ttu-id="8654d-137">To zahrnuje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="8654d-137">This involves three steps:</span></span>

1. <span data-ttu-id="8654d-138">Uložit model hello jsme natrénovali a potom můžete nahradit naše školení moduly</span><span class="sxs-lookup"><span data-stu-id="8654d-138">Save hello model we've trained and then replace our training modules</span></span>
2. <span data-ttu-id="8654d-139">Trim hello experimentu tooremove moduly, které byly jenom potřebné pro školení</span><span class="sxs-lookup"><span data-stu-id="8654d-139">Trim hello experiment tooremove modules that were only needed for training</span></span>
3. <span data-ttu-id="8654d-140">Zadejte, kde hello webová služba bude akceptovat vstup a kde vygeneruje výstup hello</span><span class="sxs-lookup"><span data-stu-id="8654d-140">Define where hello web service will accept input and where it generates hello output</span></span>

<span data-ttu-id="8654d-141">Jsme může udělat to ručně, ale naštěstí všechny tři kroky, můžete provést kliknutím na tlačítko **nastavit webové služby** dole hello plátno experimentu hello (a výběrem hello **prediktivní webové služby** možnost).</span><span class="sxs-lookup"><span data-stu-id="8654d-141">We could do this manually, but fortunately all three steps can be accomplished by clicking **Set Up Web Service** at hello bottom of hello experiment canvas (and selecting hello **Predictive Web Service** option).</span></span>

> [!TIP]
> <span data-ttu-id="8654d-142">Pokud chcete podrobnosti na co se stane, když je převést školení tooa experiment prediktivní experiment, najdete v části [jak tooprepare vašeho modelu pro nasazení v nástroji Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="8654d-142">If you want more details on what happens when you convert a training experiment tooa predictive experiment, see [How tooprepare your model for deployment in Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span></span>

<span data-ttu-id="8654d-143">Když kliknete na tlačítko **nastavit webové služby**, dojde k několika změnám:</span><span class="sxs-lookup"><span data-stu-id="8654d-143">When you click **Set Up Web Service**, several things happen:</span></span>

* <span data-ttu-id="8654d-144">Hello trained model je převeden tooa jeden **Trained Model** modulu a uložené v hello modulu palety toohello nalevo od hello Experimentujte plátno (najdete ji v části **Trénované modely**)</span><span class="sxs-lookup"><span data-stu-id="8654d-144">hello trained model is converted tooa single **Trained Model** module and stored in hello module palette toohello left of hello experiment canvas (you can find it under **Trained Models**)</span></span>
* <span data-ttu-id="8654d-145">Moduly, které jste použili pro školení jsou odebrány; konkrétně:</span><span class="sxs-lookup"><span data-stu-id="8654d-145">Modules that were used for training are removed; specifically:</span></span>
  * <span data-ttu-id="8654d-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span><span class="sxs-lookup"><span data-stu-id="8654d-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span></span>
  * <span data-ttu-id="8654d-147">[Train Model][train-model]</span><span class="sxs-lookup"><span data-stu-id="8654d-147">[Train Model][train-model]</span></span>
  * <span data-ttu-id="8654d-148">[Rozdělení dat][split]</span><span class="sxs-lookup"><span data-stu-id="8654d-148">[Split Data][split]</span></span>
  * <span data-ttu-id="8654d-149">druhý Hello [spustit skript jazyka R] [ execute-r-script] modul, který byl použit pro testovací data</span><span class="sxs-lookup"><span data-stu-id="8654d-149">hello second [Execute R Script][execute-r-script] module that was used for test data</span></span>
* <span data-ttu-id="8654d-150">Hello uložit trained model je přidat zpět do experimentu hello</span><span class="sxs-lookup"><span data-stu-id="8654d-150">hello saved trained model is added back into hello experiment</span></span>
* <span data-ttu-id="8654d-151">**Webové služby vstup** a **webové služby výstup** moduly jsou přidány (ty identifikují kde hello uživatelská data budou zadejte hello modelu, a jaká data se vrátí, při přístupu k hello webová služba)</span><span class="sxs-lookup"><span data-stu-id="8654d-151">**Web service input** and **Web service output** modules are added (these identify where hello user's data will enter hello model, and what data is returned, when hello web service is accessed)</span></span>

> [!NOTE]
> <span data-ttu-id="8654d-152">Uvidíte, že hello experiment uloží ve dvou částech na kartách, které byly přidány hello horní části plátna experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-152">You can see that hello experiment is saved in two parts under tabs that have been added at hello top of hello experiment canvas.</span></span> <span data-ttu-id="8654d-153">Hello původní výukový experiment je kartě hello **výukový experiment**, a nově vytvořený prediktivní experiment hello je pod **prediktivní experiment**.</span><span class="sxs-lookup"><span data-stu-id="8654d-153">hello original training experiment is under hello tab **Training experiment**, and hello newly created predictive experiment is under **Predictive experiment**.</span></span> <span data-ttu-id="8654d-154">Prediktivní experiment Hello je hello jeden, který jsme budete nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8654d-154">hello predictive experiment is hello one we'll deploy as a web service.</span></span>

<span data-ttu-id="8654d-155">Potřebujeme tootake další krok se tento konkrétní experimentu.</span><span class="sxs-lookup"><span data-stu-id="8654d-155">We need tootake one additional step with this particular experiment.</span></span>
<span data-ttu-id="8654d-156">Jsme přidali dvě [spustit skript jazyka R] [ execute-r-script] tooprovide moduly vyvážení funkce toohello data.</span><span class="sxs-lookup"><span data-stu-id="8654d-156">We added two [Execute R Script][execute-r-script] modules tooprovide a weighting function toohello data.</span></span> <span data-ttu-id="8654d-157">Proto jsme vyjměte tyto moduly v hello konečné modelu, který byl právě efektu potřebnou pro trénování a testování.</span><span class="sxs-lookup"><span data-stu-id="8654d-157">That was just a trick we needed for training and testing, so we can take out those modules in hello final model.</span></span>
<span data-ttu-id="8654d-158">Machine Learning Studio odebrat jeden [spustit skript jazyka R] [ execute-r-script] modulu odebrán hello [rozdělení] [ split] modulu.</span><span class="sxs-lookup"><span data-stu-id="8654d-158">Machine Learning Studio removed one [Execute R Script][execute-r-script] module when it removed hello [Split][split] module.</span></span> <span data-ttu-id="8654d-159">Nyní jsme můžete odebrat hello navzájem a připojení [Metadata Editor] [ metadata-editor] přímo příliš[Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="8654d-159">Now we can remove hello other and connect [Metadata Editor][metadata-editor] directly too[Score Model][score-model].</span></span>    

<span data-ttu-id="8654d-160">Naše experiment by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="8654d-160">Our experiment should now look like this:</span></span>  

![Vyhodnocování hello trained model][4]  

> [!NOTE]
> <span data-ttu-id="8654d-162">Asi vás zajímá, proč jsme left hello UCI němčina platební karty dat datové sady v prediktivní experiment hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-162">You may be wondering why we left hello UCI German Credit Card Data dataset in hello predictive experiment.</span></span> <span data-ttu-id="8654d-163">Hello služby, který se bude tooscore hello uživatelská data, není hello původní datové sady, takže proč nechte hello původní datové sady v modelu hello?</span><span class="sxs-lookup"><span data-stu-id="8654d-163">hello service is going tooscore hello user's data, not hello original dataset, so why leave hello original dataset in hello model?</span></span>
> 
> <span data-ttu-id="8654d-164">Je PRAVDA, že není nutné hello služby hello původní data platební karty.</span><span class="sxs-lookup"><span data-stu-id="8654d-164">It's true that hello service doesn't need hello original credit card data.</span></span> <span data-ttu-id="8654d-165">Ale potřebuje hello schématu pro tato data, což zahrnuje informace, jako je počet sloupců, jsou a sloupce, které jsou číselná.</span><span class="sxs-lookup"><span data-stu-id="8654d-165">But it does need hello schema for that data, which includes information such as how many columns there are and which columns are numeric.</span></span> <span data-ttu-id="8654d-166">Tato informace o schématu je nezbytné toointerpret hello uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="8654d-166">This schema information is necessary toointerpret hello user's data.</span></span> <span data-ttu-id="8654d-167">Jsme ponechat tyto součásti připojení tak, aby hello vyhodnocování modul má schéma datové sady hello když je spuštěna služba hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-167">We leave these components connected so that hello scoring module has hello dataset schema when hello service is running.</span></span> <span data-ttu-id="8654d-168">Hello data se nepoužívá, právě hello schématu.</span><span class="sxs-lookup"><span data-stu-id="8654d-168">hello data isn't used, just hello schema.</span></span>  
> 
> 

<span data-ttu-id="8654d-169">Spusťte experiment hello jednou (klikněte na tlačítko **spustit**.) Pokud chcete přesto funguje tooverify, který hello model, klikněte na tlačítko hello výstup hello [Score Model] [ score-model] modul a vyberte **zobrazení výsledků**.</span><span class="sxs-lookup"><span data-stu-id="8654d-169">Run hello experiment one last time (click **Run**.) If you want tooverify that hello model is still working, click hello output of hello [Score Model][score-model] module and select **View Results**.</span></span> <span data-ttu-id="8654d-170">Uvidíte, že se zobrazí původní data hello, spolu s hello úvěrové riziko hodnotu ("popisky vyhodnocení") a hello vyhodnocování hodnotu pravděpodobnosti ("skóre pro Magnitudu Pravděpodobnostech".)</span><span class="sxs-lookup"><span data-stu-id="8654d-170">You can see that hello original data is displayed, along with hello credit risk value ("Scored Labels") and hello scoring probability value ("Scored Probabilities".)</span></span> 

## <a name="deploy-hello-web-service"></a><span data-ttu-id="8654d-171">Nasazení webové služby hello</span><span class="sxs-lookup"><span data-stu-id="8654d-171">Deploy hello web service</span></span>
<span data-ttu-id="8654d-172">Hello experimentu můžete nasadit jako buď Classic webovou službu nebo jako novou webovou službu, která je založená na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8654d-172">You can deploy hello experiment as either a Classic web service, or as a New web service that's based on Azure Resource Manager.</span></span>

### <a name="deploy-as-a-classic-web-service"></a><span data-ttu-id="8654d-173">Nasadit jako webovou službu Classic</span><span class="sxs-lookup"><span data-stu-id="8654d-173">Deploy as a Classic web service</span></span>
<span data-ttu-id="8654d-174">Klikněte na tlačítko toodeploy klasický webové služby, které jsou odvozené z našich experimentu **nasazení webové služby** níže hello plátno a vyberte **nasazení webové služby [Classic]**.</span><span class="sxs-lookup"><span data-stu-id="8654d-174">toodeploy a Classic web service derived from our experiment, click **Deploy Web Service** below hello canvas and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="8654d-175">Machine Learning Studio nasadí hello experimentu jako webovou službu a přejdete toohello řídicí panel pro tuto webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8654d-175">Machine Learning Studio deploys hello experiment as a web service and takes you toohello dashboard for that web service.</span></span> <span data-ttu-id="8654d-176">Z této stránky můžete se vrátit toohello experimentu (**zobrazení snímku** nebo **zobrazit nejnovější**) a spusťte jednoduchá testovací hello webové služby (najdete v části **testování hello webové služby** níže).</span><span class="sxs-lookup"><span data-stu-id="8654d-176">From this page you can return toohello experiment (**View snapshot** or **View latest**) and run a simple test of hello web service (see **Test hello web service** below).</span></span> <span data-ttu-id="8654d-177">Je zde také informace pro vytváření aplikací, kteří mohou přistupovat k hello webové služby (Další informace o, v dalším kroku hello tohoto názorného postupu).</span><span class="sxs-lookup"><span data-stu-id="8654d-177">There is also information here for creating applications that can access hello web service (more on that in hello next step of this walkthrough).</span></span>

![Řídicí panel webové služby][6]

<span data-ttu-id="8654d-179">Můžete nakonfigurovat službu hello kliknutím hello **konfigurace** kartě. Zde můžete upravit název služby hello (je název experimentu dané hello ve výchozím nastavení) a zadejte jeho popis.</span><span class="sxs-lookup"><span data-stu-id="8654d-179">You can configure hello service by clicking hello **CONFIGURATION** tab. Here you can modify hello service name (it's given hello experiment name by default) and give it a description.</span></span> <span data-ttu-id="8654d-180">Můžete také udělit další popisný popisky pro hello vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="8654d-180">You can also give more friendly labels for hello input and output data.</span></span>  

![Konfigurovat webovou službu hello][5]  

### <a name="deploy-as-a-new-web-service"></a><span data-ttu-id="8654d-182">Nasadit jako novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="8654d-182">Deploy as a New web service</span></span>

> [!NOTE] 
> <span data-ttu-id="8654d-183">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné, které nasazujete hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="8654d-183">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you are deploying hello web service.</span></span> <span data-ttu-id="8654d-184">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="8654d-184">For more information, see [Manage a web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="8654d-185">toodeploy novou webovou službu získané z našich experimentu:</span><span class="sxs-lookup"><span data-stu-id="8654d-185">toodeploy a New web service derived from our experiment:</span></span>

1. <span data-ttu-id="8654d-186">Klikněte na tlačítko **nasazení webové služby** níže hello plátno a vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="8654d-186">Click **Deploy Web Service** below hello canvas and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="8654d-187">Machine Learning Studio přenosy, webové služby Azure Machine Learning toohello **nasazení experimentu** stránky.</span><span class="sxs-lookup"><span data-stu-id="8654d-187">Machine Learning Studio transfers you toohello Azure Machine Learning web services **Deploy Experiment** page.</span></span>

2. <span data-ttu-id="8654d-188">Zadejte název pro hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8654d-188">Enter a name for hello web service.</span></span> 

3. <span data-ttu-id="8654d-189">Pro **cena plán**, můžete vybrat existující cenový plán, nebo vyberte možnost "vytvořit nový" a udělte hello nový plán název a vyberte hello měsíční možnost plánu.</span><span class="sxs-lookup"><span data-stu-id="8654d-189">For **Price Plan**, you can select an existing pricing plan, or select "Create new" and give hello new plan a name and select hello monthly plan option.</span></span> <span data-ttu-id="8654d-190">plán Hello, že vrstvy výchozí toohello plány pro vaši oblast výchozí a webové služby je nasazené toothat oblast.</span><span class="sxs-lookup"><span data-stu-id="8654d-190">hello plan tiers default toohello plans for your default region and your web service is deployed toothat region.</span></span>

4. <span data-ttu-id="8654d-191">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="8654d-191">Click **Deploy**.</span></span>

<span data-ttu-id="8654d-192">Po několika minutách hello **rychlý Start** otevře se stránka pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8654d-192">After a few minutes, hello **Quickstart** page for your web service opens.</span></span>

<span data-ttu-id="8654d-193">Můžete nakonfigurovat službu hello kliknutím hello **konfigurace** kartě. Zde můžete upravit hello služby title a zadejte jeho popis.</span><span class="sxs-lookup"><span data-stu-id="8654d-193">You can configure hello service by clicking hello **Configure** tab. Here you can modify hello service title and give it a description.</span></span> 

<span data-ttu-id="8654d-194">tootest hello webové služby, klikněte na tlačítko hello **Test** karta (najdete v části **testování hello webové služby** níže).</span><span class="sxs-lookup"><span data-stu-id="8654d-194">tootest hello web service, click hello **Test** tab (see **Test hello web service** below).</span></span> <span data-ttu-id="8654d-195">Informace o vytváření aplikací, kteří mohou přistupovat k hello webové služby, klikněte na tlačítko hello **spotřebě** karta (hello další krok v tomto návodu přejde do více podrobností).</span><span class="sxs-lookup"><span data-stu-id="8654d-195">For information on creating applications that can access hello web service, click hello **Consume** tab (hello next step in this walkthrough will go into more detail).</span></span>

> [!TIP]
> <span data-ttu-id="8654d-196">Poté, co nasadíte ji můžete aktualizovat hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="8654d-196">You can update hello web service after you've deployed it.</span></span> <span data-ttu-id="8654d-197">Například pokud chcete toochange modelu, pak můžete upravit hello výukový experiment, upravit parametry modelu hello a klikněte na tlačítko **nasazení webové služby**, vyberete **nasazení webové služby [Classic]** nebo  **Nasazení webové služby [nové]**.</span><span class="sxs-lookup"><span data-stu-id="8654d-197">For example, if you want toochange your model, then you can edit hello training experiment, tweak hello model parameters, and click **Deploy Web Service**, selecting **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span> <span data-ttu-id="8654d-198">Při nasazení hello experiment, nahradí hello webové službě, teď pomocí aktualizovaném modelu.</span><span class="sxs-lookup"><span data-stu-id="8654d-198">When you deploy hello experiment again, it replaces hello web service, now using your updated model.</span></span>  
> 
> 

## <a name="test-hello-web-service"></a><span data-ttu-id="8654d-199">Testování hello webové služby</span><span class="sxs-lookup"><span data-stu-id="8654d-199">Test hello web service</span></span>

<span data-ttu-id="8654d-200">Při přístupu k webové službě hello hello uživatel zadá prostřednictvím hello **webové služby vstup** modulu, kde úspěšně prošel toohello [Score Model] [ score-model] modulu a skóre.</span><span class="sxs-lookup"><span data-stu-id="8654d-200">When hello web service is accessed, hello user's data enters through hello **Web service input** module where it's passed toohello [Score Model][score-model] module and scored.</span></span> <span data-ttu-id="8654d-201">Hello způsob, jakým jsme zřídili hello prediktivní experiment, hello modelu očekává data hello stejný formát jako hello původní úvěrové riziko datové sady.</span><span class="sxs-lookup"><span data-stu-id="8654d-201">hello way we've set up hello predictive experiment, hello model expects data in hello same format as hello original credit risk dataset.</span></span>
<span data-ttu-id="8654d-202">Hello budou vráceny výsledky toohello uživatele z hello webové služby prostřednictvím hello **webové služby výstup** modulu.</span><span class="sxs-lookup"><span data-stu-id="8654d-202">hello results are returned toohello user from hello web service through hello **Web service output** module.</span></span>

> [!TIP]
> <span data-ttu-id="8654d-203">způsob Hello máme hello prediktivní experiment nakonfigurované, hello celý výsledkem hello [Score Model] [ score-model] modulu jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="8654d-203">hello way we have hello predictive experiment configured, hello entire results from hello [Score Model][score-model] module are returned.</span></span> <span data-ttu-id="8654d-204">To zahrnuje všechny vstupní data hello plus hello úvěrové riziko hodnotu a hello vyhodnocování pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="8654d-204">This includes all hello input data plus hello credit risk value and hello scoring probability.</span></span> <span data-ttu-id="8654d-205">Ale něco jiného může vrátit, pokud chcete – například může vrátit právě hello úvěrové riziko hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8654d-205">But you can return something different if you want - for example, you could return just hello credit risk value.</span></span> <span data-ttu-id="8654d-206">toodo tento, insert [sloupce projektu] [ project-columns] modulu mezi [Score Model] [ score-model] a hello **webové služby výstup** tooeliminate sloupce, které nechcete hello tooreturn webové služby.</span><span class="sxs-lookup"><span data-stu-id="8654d-206">toodo this, insert a [Project Columns][project-columns] module between [Score Model][score-model] and hello **Web service output** tooeliminate columns you don't want hello web service tooreturn.</span></span> 
> 
> 

<span data-ttu-id="8654d-207">Můžete otestovat Classic webové služby buď v **Machine Learning Studio** nebo v hello **webové služby Azure Machine Learning** portálu.</span><span class="sxs-lookup"><span data-stu-id="8654d-207">You can test a Classic web service either in **Machine Learning Studio** or in hello **Azure Machine Learning Web Services** portal.</span></span>
<span data-ttu-id="8654d-208">Novou webovou službu můžete otestovat pouze v hello **webové služby Machine Learning** portálu.</span><span class="sxs-lookup"><span data-stu-id="8654d-208">You can test a New web service only in hello **Machine Learning Web Services** portal.</span></span>

> [!TIP]
> <span data-ttu-id="8654d-209">Při testování portálu hello webové služby Azure Machine Learning, může mít portál hello vytvoření ukázkových dat, které můžete použít službu tootest hello požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="8654d-209">When testing in hello Azure Machine Learning Web Services portal, you can have hello portal create sample data that you can use tootest hello Request-Response service.</span></span> <span data-ttu-id="8654d-210">Na hello **konfigurace** vyberte "Ano" pro **ukázkových dat povolené?**.</span><span class="sxs-lookup"><span data-stu-id="8654d-210">On hello **Configure** page, select "Yes" for **Sample Data Enabled?**.</span></span> <span data-ttu-id="8654d-211">Když otevřete kartu hello požadavků a odpovědí na hello **Test** stránce portálu hello vyplní ukázková data přijatá z hello původní úvěrové riziko datové sady.</span><span class="sxs-lookup"><span data-stu-id="8654d-211">When you open hello Request-Response tab on hello **Test** page, hello portal fills in sample data taken from hello original credit risk dataset.</span></span>

### <a name="test-a-classic-web-service"></a><span data-ttu-id="8654d-212">Testování Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="8654d-212">Test a Classic web service</span></span>

<span data-ttu-id="8654d-213">Webová služba Classic můžete otestovat v nástroji Machine Learning Studio nebo hello webové služby Machine Learning portálu.</span><span class="sxs-lookup"><span data-stu-id="8654d-213">You can test a Classic web service in Machine Learning Studio or in hello Machine Learning Web Services portal.</span></span> 

#### <a name="test-in-machine-learning-studio"></a><span data-ttu-id="8654d-214">Testování v nástroji Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="8654d-214">Test in Machine Learning Studio</span></span>

1. <span data-ttu-id="8654d-215">Na hello **řídicí panel** stránky pro webovou službu hello, klikněte na tlačítko hello **Test** tlačítko pod **výchozí koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="8654d-215">On hello **DASHBOARD** page for hello web service, click hello **Test** button under **Default Endpoint**.</span></span> <span data-ttu-id="8654d-216">Zobrazí se dialogové okno se zobrazí a se vás zeptá na hello vstupní data pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-216">A dialog pops up and asks you for hello input data for hello service.</span></span> <span data-ttu-id="8654d-217">Tyto jsou hello stejné sloupce, které se zobrazovaly v hello původní úvěrové riziko datové sady.</span><span class="sxs-lookup"><span data-stu-id="8654d-217">These are hello same columns that appeared in hello original credit risk dataset.</span></span>  

2. <span data-ttu-id="8654d-218">Zadejte sadu dat a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="8654d-218">Enter a set of data and then click **OK**.</span></span> 

#### <a name="test-in-hello-machine-learning-web-services-portal"></a><span data-ttu-id="8654d-219">Test webové služby Machine Learning portálu hello</span><span class="sxs-lookup"><span data-stu-id="8654d-219">Test in hello Machine Learning Web Services portal</span></span>

1. <span data-ttu-id="8654d-220">Na hello **řídicí panel** stránky pro webovou službu hello, klikněte na tlačítko hello **testovací preview** v části **výchozí koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="8654d-220">On hello **DASHBOARD** page for hello web service, click hello **Test preview** link under **Default Endpoint**.</span></span> <span data-ttu-id="8654d-221">Hello zkušební stránku hello webové služby Azure Machine Learning portálu pro koncový bod webové služby hello otevře a zobrazí výzvu k hello vstupní data pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-221">hello test page in hello Azure Machine Learning Web Services portal for hello web service endpoint opens and asks you for hello input data for hello service.</span></span> <span data-ttu-id="8654d-222">Tyto jsou hello stejné sloupce, které se zobrazovaly v hello původní úvěrové riziko datové sady.</span><span class="sxs-lookup"><span data-stu-id="8654d-222">These are hello same columns that appeared in hello original credit risk dataset.</span></span>

2. <span data-ttu-id="8654d-223">Klikněte na tlačítko **testování požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="8654d-223">Click **Test Request-Response**.</span></span> 

### <a name="test-a-new-web-service"></a><span data-ttu-id="8654d-224">Otestovat novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="8654d-224">Test a New web service</span></span>

<span data-ttu-id="8654d-225">Pouze na portál hello webové služby Machine Learning můžete testovat novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="8654d-225">You can test a New web service only in hello Machine Learning Web Services portal.</span></span>

1. <span data-ttu-id="8654d-226">V hello [webové služby Azure Machine Learning](https://services.azureml.net/quickstart) portálu klikněte na **Test** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-226">In hello [Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal, click **Test** at hello top of hello page.</span></span> <span data-ttu-id="8654d-227">Hello **Test** otevře se stránka a můžete vstupní data pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="8654d-227">hello **Test** page opens and you can input data for hello service.</span></span> <span data-ttu-id="8654d-228">vstupní pole Hello zobrazí odpovídají toohello sloupce, které se zobrazovaly v hello původní úvěrové riziko datové sady.</span><span class="sxs-lookup"><span data-stu-id="8654d-228">hello input fields displayed correspond toohello columns that appeared in hello original credit risk dataset.</span></span> 

2. <span data-ttu-id="8654d-229">Zadejte sadu dat a pak klikněte na tlačítko **testu požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="8654d-229">Enter a set of data and then click **Test Request-Response**.</span></span>

<span data-ttu-id="8654d-230">Hello výsledky testu hello zobrazí na pravé straně hello hello stránku hello výstupního sloupce.</span><span class="sxs-lookup"><span data-stu-id="8654d-230">hello results of hello test are displayed on hello right-hand side of hello page in hello output column.</span></span> 


## <a name="manage-hello-web-service"></a><span data-ttu-id="8654d-231">Spravovat hello webové služby</span><span class="sxs-lookup"><span data-stu-id="8654d-231">Manage hello web service</span></span>

### <a name="manage-a-classic-web-service-in-hello-azure-classic-portal"></a><span data-ttu-id="8654d-232">Správa webové služby Classic v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="8654d-232">Manage a Classic web service in hello Azure classic portal</span></span>

<span data-ttu-id="8654d-233">Když máte ukázku nasazenou webovou službu Classic, můžete je spravovat z hello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="8654d-233">Once you've deployed your Classic web service, you can manage it from hello [Azure classic portal](https://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="8654d-234">Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="8654d-234">Sign in toohello [Azure classic portal](https://manage.windowsazure.com)</span></span>
2. <span data-ttu-id="8654d-235">V panelu služby Microsoft Azure hello, klikněte na tlačítko **MACHINE LEARNING**</span><span class="sxs-lookup"><span data-stu-id="8654d-235">In hello Microsoft Azure services panel, click **MACHINE LEARNING**</span></span>
3. <span data-ttu-id="8654d-236">Klikněte na pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="8654d-236">Click your workspace</span></span>
4. <span data-ttu-id="8654d-237">Klikněte na tlačítko hello **webové služby** karta</span><span class="sxs-lookup"><span data-stu-id="8654d-237">Click hello **Web services** tab</span></span>
5. <span data-ttu-id="8654d-238">Klikněte na tlačítko hello webové služby, kterou jsme vytvořili</span><span class="sxs-lookup"><span data-stu-id="8654d-238">Click hello web service we created</span></span>
6. <span data-ttu-id="8654d-239">Klikněte na tlačítko "Výchozí" hello koncový bod</span><span class="sxs-lookup"><span data-stu-id="8654d-239">Click hello "default" endpoint</span></span>

<span data-ttu-id="8654d-240">Zde lze provádět akce, jako jsou monitorování, jak je to hello webové služby a provést vylepšení výkonu změnou, kolik souběžných volání hello služby může zpracovat.</span><span class="sxs-lookup"><span data-stu-id="8654d-240">From here, you can do things like monitor how hello web service is doing and make performance tweaks by changing how many concurrent calls hello service can handle.</span></span>

<span data-ttu-id="8654d-241">Další podrobnosti najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="8654d-241">For more details, see:</span></span>

* [<span data-ttu-id="8654d-242">Vytváření koncových bodů</span><span class="sxs-lookup"><span data-stu-id="8654d-242">Creating Endpoints</span></span>](machine-learning-create-endpoint.md)
* [<span data-ttu-id="8654d-243">Škálování webové služby</span><span class="sxs-lookup"><span data-stu-id="8654d-243">Scaling web service</span></span>](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-hello-azure-machine-learning-web-services-portal"></a><span data-ttu-id="8654d-244">Spravovat na klasickém nebo novou webovou službu portálu hello webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="8654d-244">Manage a Classic or New web service in hello Azure Machine Learning Web Services portal</span></span>

<span data-ttu-id="8654d-245">Po nasazení webové služby, zda Classic nebo nové, můžete je spravovat z hello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portálu.</span><span class="sxs-lookup"><span data-stu-id="8654d-245">Once you've deployed your web service, whether Classic or New, you can manage it from hello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal.</span></span>

<span data-ttu-id="8654d-246">výkon hello toomonitor webové služby:</span><span class="sxs-lookup"><span data-stu-id="8654d-246">toomonitor hello performance of your web service:</span></span>

1. <span data-ttu-id="8654d-247">Přihlaste se toohello [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portálu</span><span class="sxs-lookup"><span data-stu-id="8654d-247">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal</span></span>
2. <span data-ttu-id="8654d-248">Klikněte na tlačítko **webové služby**</span><span class="sxs-lookup"><span data-stu-id="8654d-248">Click **Web services**</span></span>
3. <span data-ttu-id="8654d-249">Klikněte na webovou službu</span><span class="sxs-lookup"><span data-stu-id="8654d-249">Click your web service</span></span>
4. <span data-ttu-id="8654d-250">Klikněte na tlačítko hello **řídicí panel**</span><span class="sxs-lookup"><span data-stu-id="8654d-250">Click hello **Dashboard**</span></span>

- - -
<span data-ttu-id="8654d-251">**Další krok: [přístup k webové službě hello](machine-learning-walkthrough-6-access-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="8654d-251">**Next: [Access hello web service](machine-learning-walkthrough-6-access-web-service.md)**</span></span>

[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
