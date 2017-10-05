---
title: "Krok 5: Nasazení webové služby Machine Learning | Microsoft Docs"
description: "Krok 5 vývoj prediktivního řešení návod: nasazení prediktivní experiment v nástroji Machine Learning Studio jako webovou službu."
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
ms.openlocfilehash: cec1bcceea158a20742c7019a266dcefaac4c9cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a><span data-ttu-id="d6218-103">Krok 5 průvodce: Nasazení webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d6218-103">Walkthrough Step 5: Deploy the Azure Machine Learning web service</span></span>
<span data-ttu-id="d6218-104">Toto je pátý krok tohoto průvodce, [vývoj řešení prediktivní analýzy v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="d6218-104">This is the fifth step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="d6218-105">Vytvoření pracovního prostoru Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d6218-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="d6218-106">Nahrání existujících dat</span><span class="sxs-lookup"><span data-stu-id="d6218-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="d6218-107">Vytvoření nového experimentu</span><span class="sxs-lookup"><span data-stu-id="d6218-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="d6218-108">Natrénování a vyhodnocení modelů</span><span class="sxs-lookup"><span data-stu-id="d6218-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. <span data-ttu-id="d6218-109">**Nasazení webové služby**</span><span class="sxs-lookup"><span data-stu-id="d6218-109">**Deploy the web service**</span></span>
6. [<span data-ttu-id="d6218-110">Nastavení přístupu k webové službě</span><span class="sxs-lookup"><span data-stu-id="d6218-110">Access the web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="d6218-111">Ostatním uživatelům možnost použít prediktivní model, který jsme jste vytvořili v tomto návodu, můžeme ho nasadit jako webovou službu v Azure.</span><span class="sxs-lookup"><span data-stu-id="d6218-111">To give others a chance to use the predictive model we've developed in this walkthrough, we can deploy it as a web service on Azure.</span></span>

<span data-ttu-id="d6218-112">V tomto okamžiku jste byla jsme experimentování se cvičení naše modelu.</span><span class="sxs-lookup"><span data-stu-id="d6218-112">Up to this point we've been experimenting with training our model.</span></span> <span data-ttu-id="d6218-113">Ale nasazená služba je již má proveďte školení – má vygenerujte nový předpovědi vyhodnocování vstupu uživatele na základě naše modelu.</span><span class="sxs-lookup"><span data-stu-id="d6218-113">But the deployed service is no longer going to do training - it's going to generate new predictions by scoring the user's input based on our model.</span></span> <span data-ttu-id="d6218-114">Takže vytvoříme provést určitou přípravu pro převod tohoto experimentu z ***školení*** experimentovat k ***prediktivní*** experimentovat.</span><span class="sxs-lookup"><span data-stu-id="d6218-114">So we're going to do some preparation to convert this experiment from a ***training*** experiment to a ***predictive*** experiment.</span></span> 

<span data-ttu-id="d6218-115">Jedná se o proces třech krocích:</span><span class="sxs-lookup"><span data-stu-id="d6218-115">This is a three-step process:</span></span>  

1. <span data-ttu-id="d6218-116">Odeberte jeden z modelů</span><span class="sxs-lookup"><span data-stu-id="d6218-116">Remove one of the models</span></span>
2. <span data-ttu-id="d6218-117">Převést *výukový experiment* vytvořili jsme do *prediktivní experiment*</span><span class="sxs-lookup"><span data-stu-id="d6218-117">Convert the *training experiment* we've created into a *predictive experiment*</span></span>
3. <span data-ttu-id="d6218-118">Nasadit prediktivní experiment jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="d6218-118">Deploy the predictive experiment as a web service</span></span>

## <a name="remove-one-of-the-models"></a><span data-ttu-id="d6218-119">Odeberte jeden z modelů</span><span class="sxs-lookup"><span data-stu-id="d6218-119">Remove one of the models</span></span>

<span data-ttu-id="d6218-120">Nejprve musíme trochu trim tohoto experimentu dolů.</span><span class="sxs-lookup"><span data-stu-id="d6218-120">First, we need to trim this experiment down a little.</span></span> <span data-ttu-id="d6218-121">V současné době máme dva různé modely v experimentu, ale chceme používat jeden model, když jsme nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-121">We currently have two different models in the experiment, but we only want to use one model when we deploy this as a web service.</span></span>  

<span data-ttu-id="d6218-122">Řekněme, že jsme jste se rozhodli lépe než SVM model provést boosted stromu modelu.</span><span class="sxs-lookup"><span data-stu-id="d6218-122">Let's say we've decided that the boosted tree model performed better than the SVM model.</span></span> <span data-ttu-id="d6218-123">Tak, aby první věc, kterou chcete odebrat [Two-Class Support Vector Machine] [ two-class-support-vector-machine] modulu a moduly, které se používaly k cvičení ho.</span><span class="sxs-lookup"><span data-stu-id="d6218-123">So the first thing to do is remove the [Two-Class Support Vector Machine][two-class-support-vector-machine] module and the modules that were used for training it.</span></span> <span data-ttu-id="d6218-124">Můžete chtít vytvořit kopii experimentu nejprve kliknutím **uložit jako** v dolní části na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="d6218-124">You may want to make a copy of the experiment first by clicking **Save As** at the bottom of the experiment canvas.</span></span>

<span data-ttu-id="d6218-125">Je potřeba odstranit následující moduly:</span><span class="sxs-lookup"><span data-stu-id="d6218-125">We need to delete the following modules:</span></span>  

* <span data-ttu-id="d6218-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span><span class="sxs-lookup"><span data-stu-id="d6218-126">[Two-Class Support Vector Machine][two-class-support-vector-machine]</span></span>
* <span data-ttu-id="d6218-127">[Cvičení modelu] [ train-model] a [Score Model] [ score-model] moduly, které byly připojené k němu</span><span class="sxs-lookup"><span data-stu-id="d6218-127">[Train Model][train-model] and [Score Model][score-model] modules that were connected to it</span></span>
* <span data-ttu-id="d6218-128">[Normalizuje Data] [ normalize-data] (obou z nich)</span><span class="sxs-lookup"><span data-stu-id="d6218-128">[Normalize Data][normalize-data] (both of them)</span></span>
* <span data-ttu-id="d6218-129">[Vyhodnocení modelu] [ evaluate-model] (protože jsme hotovi, vyhodnocení modelů)</span><span class="sxs-lookup"><span data-stu-id="d6218-129">[Evaluate Model][evaluate-model] (because we're finished evaluating the models)</span></span>

<span data-ttu-id="d6218-130">Vyberte každý modul a stiskněte klávesu Delete, nebo klikněte pravým tlačítkem na modul a vyberte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="d6218-130">Select each module and press the Delete key, or right-click the module and select **Delete**.</span></span> 

![Odebrat SVM modelu][3a]

<span data-ttu-id="d6218-132">Naše modelu by teď měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="d6218-132">Our model should now look something like this:</span></span>

![Odebrat SVM modelu][3]

<span data-ttu-id="d6218-134">Nyní je vše připraveno k nasazení této konfigurace pomocí modelu [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span><span class="sxs-lookup"><span data-stu-id="d6218-134">Now we're ready to deploy this model using the [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree].</span></span>

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a><span data-ttu-id="d6218-135">Převést výukový experiment prediktivní experiment</span><span class="sxs-lookup"><span data-stu-id="d6218-135">Convert the training experiment to a predictive experiment</span></span>

<span data-ttu-id="d6218-136">Tento model Příprava pro nasazení, je potřeba převést tento výukový experiment prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="d6218-136">To get this model ready for deployment, we need to convert this training experiment to a predictive experiment.</span></span> <span data-ttu-id="d6218-137">To zahrnuje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="d6218-137">This involves three steps:</span></span>

1. <span data-ttu-id="d6218-138">Uložit model jsme natrénovali a potom můžete nahradit naše školení moduly</span><span class="sxs-lookup"><span data-stu-id="d6218-138">Save the model we've trained and then replace our training modules</span></span>
2. <span data-ttu-id="d6218-139">Trim experimentu odebrat moduly, které byly potřeba jenom pro školení</span><span class="sxs-lookup"><span data-stu-id="d6218-139">Trim the experiment to remove modules that were only needed for training</span></span>
3. <span data-ttu-id="d6218-140">Zadejte kde webová služba bude přijímat vstup a kde vygeneruje výstup</span><span class="sxs-lookup"><span data-stu-id="d6218-140">Define where the web service will accept input and where it generates the output</span></span>

<span data-ttu-id="d6218-141">Jsme může udělat to ručně, ale naštěstí všechny tři kroky, můžete provést kliknutím na tlačítko **nastavit webové služby** v dolní části plátna experimentu (a výběrem **webové služby prediktivní** možnost).</span><span class="sxs-lookup"><span data-stu-id="d6218-141">We could do this manually, but fortunately all three steps can be accomplished by clicking **Set Up Web Service** at the bottom of the experiment canvas (and selecting the **Predictive Web Service** option).</span></span>

> [!TIP]
> <span data-ttu-id="d6218-142">Pokud chcete podrobnosti na co se stane, když převedete výukový experiment prediktivní experiment najdete v tématu [postup přípravy modelu pro nasazení v nástroji Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="d6218-142">If you want more details on what happens when you convert a training experiment to a predictive experiment, see [How to prepare your model for deployment in Azure Machine Learning Studio](machine-learning-convert-training-experiment-to-scoring-experiment.md).</span></span>

<span data-ttu-id="d6218-143">Když kliknete na tlačítko **nastavit webové služby**, dojde k několika změnám:</span><span class="sxs-lookup"><span data-stu-id="d6218-143">When you click **Set Up Web Service**, several things happen:</span></span>

* <span data-ttu-id="d6218-144">Pro cvičný model je převést na jednu **Trained Model** modulu a uložené paletě modulů nalevo od plátna experimentu (najdete ji v části **Trénované modely**)</span><span class="sxs-lookup"><span data-stu-id="d6218-144">The trained model is converted to a single **Trained Model** module and stored in the module palette to the left of the experiment canvas (you can find it under **Trained Models**)</span></span>
* <span data-ttu-id="d6218-145">Moduly, které jste použili pro školení jsou odebrány; konkrétně:</span><span class="sxs-lookup"><span data-stu-id="d6218-145">Modules that were used for training are removed; specifically:</span></span>
  * <span data-ttu-id="d6218-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span><span class="sxs-lookup"><span data-stu-id="d6218-146">[Two-Class Boosted Decision Tree][two-class-boosted-decision-tree]</span></span>
  * <span data-ttu-id="d6218-147">[Train Model][train-model]</span><span class="sxs-lookup"><span data-stu-id="d6218-147">[Train Model][train-model]</span></span>
  * <span data-ttu-id="d6218-148">[Rozdělení dat][split]</span><span class="sxs-lookup"><span data-stu-id="d6218-148">[Split Data][split]</span></span>
  * <span data-ttu-id="d6218-149">druhý [spustit skript jazyka R] [ execute-r-script] modul, který byl použit pro testovací data</span><span class="sxs-lookup"><span data-stu-id="d6218-149">the second [Execute R Script][execute-r-script] module that was used for test data</span></span>
* <span data-ttu-id="d6218-150">Uložené trained model je přidat zpět do experimentu</span><span class="sxs-lookup"><span data-stu-id="d6218-150">The saved trained model is added back into the experiment</span></span>
* <span data-ttu-id="d6218-151">**Webové služby vstup** a **webové služby výstup** moduly jsou přidány (ty identifikují kde budou data uživatele zadejte modelu, a jaká data se vrátí, při přístupu k webové službě)</span><span class="sxs-lookup"><span data-stu-id="d6218-151">**Web service input** and **Web service output** modules are added (these identify where the user's data will enter the model, and what data is returned, when the web service is accessed)</span></span>

> [!NOTE]
> <span data-ttu-id="d6218-152">Uvidíte, že experiment uloží ve dvou částech na kartách, které byly přidány v horní části na plátno experimentu.</span><span class="sxs-lookup"><span data-stu-id="d6218-152">You can see that the experiment is saved in two parts under tabs that have been added at the top of the experiment canvas.</span></span> <span data-ttu-id="d6218-153">Původní výukový experiment je na kartě **výukový experiment**, a nově vytvořený prediktivní experiment je pod **prediktivní experiment**.</span><span class="sxs-lookup"><span data-stu-id="d6218-153">The original training experiment is under the tab **Training experiment**, and the newly created predictive experiment is under **Predictive experiment**.</span></span> <span data-ttu-id="d6218-154">Prediktivní experiment je ten, který jsme vám nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-154">The predictive experiment is the one we'll deploy as a web service.</span></span>

<span data-ttu-id="d6218-155">Je potřeba provést další krok se tento konkrétní experimentu.</span><span class="sxs-lookup"><span data-stu-id="d6218-155">We need to take one additional step with this particular experiment.</span></span>
<span data-ttu-id="d6218-156">Jsme přidali dvě [spustit skript jazyka R] [ execute-r-script] moduly, které poskytují funkce vyvážení k datům.</span><span class="sxs-lookup"><span data-stu-id="d6218-156">We added two [Execute R Script][execute-r-script] modules to provide a weighting function to the data.</span></span> <span data-ttu-id="d6218-157">Proto jsme vyjměte tyto moduly v posledním modelu, který byl právě efektu potřebnou pro trénování a testování.</span><span class="sxs-lookup"><span data-stu-id="d6218-157">That was just a trick we needed for training and testing, so we can take out those modules in the final model.</span></span>
<span data-ttu-id="d6218-158">Machine Learning Studio odebrat jeden [spustit skript jazyka R] [ execute-r-script] modulu odebrán [rozdělení] [ split] modulu.</span><span class="sxs-lookup"><span data-stu-id="d6218-158">Machine Learning Studio removed one [Execute R Script][execute-r-script] module when it removed the [Split][split] module.</span></span> <span data-ttu-id="d6218-159">Můžete odebrat dalších a připojit teď [Metadata Editor] [ metadata-editor] přímo na [Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="d6218-159">Now we can remove the other and connect [Metadata Editor][metadata-editor] directly to [Score Model][score-model].</span></span>    

<span data-ttu-id="d6218-160">Naše experiment by měl nyní vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="d6218-160">Our experiment should now look like this:</span></span>  

![Vyhodnocování trénovaného modelu][4]  

> [!NOTE]
> <span data-ttu-id="d6218-162">Asi vás zajímá, proč jsme left UCI němčina platební karty dat datové sady v prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="d6218-162">You may be wondering why we left the UCI German Credit Card Data dataset in the predictive experiment.</span></span> <span data-ttu-id="d6218-163">Služba se bude stanovení skóre data uživatele, není původní datové sady, takže proč nechte původní datové sady v modelu?</span><span class="sxs-lookup"><span data-stu-id="d6218-163">The service is going to score the user's data, not the original dataset, so why leave the original dataset in the model?</span></span>
> 
> <span data-ttu-id="d6218-164">Je PRAVDA, že službu nemusí původní data platební karty.</span><span class="sxs-lookup"><span data-stu-id="d6218-164">It's true that the service doesn't need the original credit card data.</span></span> <span data-ttu-id="d6218-165">Ale potřebuje schéma pro tato data, což zahrnuje informace, jako je počet sloupců, jsou a sloupce, které jsou číselná.</span><span class="sxs-lookup"><span data-stu-id="d6218-165">But it does need the schema for that data, which includes information such as how many columns there are and which columns are numeric.</span></span> <span data-ttu-id="d6218-166">Tato informace o schématu je nutné interpretovat data uživatele.</span><span class="sxs-lookup"><span data-stu-id="d6218-166">This schema information is necessary to interpret the user's data.</span></span> <span data-ttu-id="d6218-167">Jsme ponechat tyto součásti připojení tak, aby modul vyhodnocování nemá schéma datové sady, pokud je služba spuštěná.</span><span class="sxs-lookup"><span data-stu-id="d6218-167">We leave these components connected so that the scoring module has the dataset schema when the service is running.</span></span> <span data-ttu-id="d6218-168">Data se nepoužívá, právě schématu.</span><span class="sxs-lookup"><span data-stu-id="d6218-168">The data isn't used, just the schema.</span></span>  
> 
> 

<span data-ttu-id="d6218-169">Spusťte experiment jednou (klikněte na tlačítko **spustit**.) Pokud chcete ověřit, zda model ještě pracuje, klikněte na výstup [Score Model] [ score-model] modul a vyberte **zobrazení výsledků**.</span><span class="sxs-lookup"><span data-stu-id="d6218-169">Run the experiment one last time (click **Run**.) If you want to verify that the model is still working, click the output of the [Score Model][score-model] module and select **View Results**.</span></span> <span data-ttu-id="d6218-170">Uvidíte, že se zobrazí původní data, společně s platební riziko hodnotu ("popisky vyhodnocení") a vyhodnocování hodnotu pravděpodobnosti ("skóre pro Magnitudu Pravděpodobnostech".)</span><span class="sxs-lookup"><span data-stu-id="d6218-170">You can see that the original data is displayed, along with the credit risk value ("Scored Labels") and the scoring probability value ("Scored Probabilities".)</span></span> 

## <a name="deploy-the-web-service"></a><span data-ttu-id="d6218-171">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="d6218-171">Deploy the web service</span></span>
<span data-ttu-id="d6218-172">Experiment můžete nasadit jako buď Classic webovou službu nebo jako novou webovou službu, která je založená na Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d6218-172">You can deploy the experiment as either a Classic web service, or as a New web service that's based on Azure Resource Manager.</span></span>

### <a name="deploy-as-a-classic-web-service"></a><span data-ttu-id="d6218-173">Nasadit jako webovou službu Classic</span><span class="sxs-lookup"><span data-stu-id="d6218-173">Deploy as a Classic web service</span></span>
<span data-ttu-id="d6218-174">Chcete-li nasadit odvozené z našich experimentu webové služby Classic, klikněte na tlačítko **nasazení webové služby** níže na plátno a vyberte **nasazení webové služby [Classic]**.</span><span class="sxs-lookup"><span data-stu-id="d6218-174">To deploy a Classic web service derived from our experiment, click **Deploy Web Service** below the canvas and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="d6218-175">Machine Learning Studio nasadí experimentu jako webovou službu a přejdete na řídicí panel pro tuto webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-175">Machine Learning Studio deploys the experiment as a web service and takes you to the dashboard for that web service.</span></span> <span data-ttu-id="d6218-176">Z této stránky můžete vrátit do experimentu (**zobrazení snímku** nebo **zobrazit nejnovější**) a spusťte jednoduchý test webové služby (najdete v části **testování webovou službu** níže).</span><span class="sxs-lookup"><span data-stu-id="d6218-176">From this page you can return to the experiment (**View snapshot** or **View latest**) and run a simple test of the web service (see **Test the web service** below).</span></span> <span data-ttu-id="d6218-177">Je zde také informace pro vytváření aplikací, které můžete přístup k webové službě (Další informace o, v dalším kroku tohoto názorného postupu).</span><span class="sxs-lookup"><span data-stu-id="d6218-177">There is also information here for creating applications that can access the web service (more on that in the next step of this walkthrough).</span></span>

![Řídicí panel webové služby][6]

<span data-ttu-id="d6218-179">Můžete nakonfigurovat službu kliknutím **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="d6218-179">You can configure the service by clicking the **CONFIGURATION** tab.</span></span> <span data-ttu-id="d6218-180">Zde můžete upravit název služby (poskytla název experimentu ve výchozím nastavení) a zadejte jeho popis.</span><span class="sxs-lookup"><span data-stu-id="d6218-180">Here you can modify the service name (it's given the experiment name by default) and give it a description.</span></span> <span data-ttu-id="d6218-181">Další popisný popisky můžete udělit taky pro vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="d6218-181">You can also give more friendly labels for the input and output data.</span></span>  

![Konfigurovat webovou službu][5]  

### <a name="deploy-as-a-new-web-service"></a><span data-ttu-id="d6218-183">Nasadit jako novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="d6218-183">Deploy as a New web service</span></span>

> [!NOTE] 
> <span data-ttu-id="d6218-184">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které nasazujete webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-184">To deploy a New web service you must have sufficient permissions in the subscription to which you are deploying the web service.</span></span> <span data-ttu-id="d6218-185">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="d6218-185">For more information, see [Manage a web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="d6218-186">Nasadit novou webovou službu získané z našich experimentu:</span><span class="sxs-lookup"><span data-stu-id="d6218-186">To deploy a New web service derived from our experiment:</span></span>

1. <span data-ttu-id="d6218-187">Klikněte na tlačítko **nasazení webové služby** níže na plátno a vyberte **nasazení [nové] webové služby**.</span><span class="sxs-lookup"><span data-stu-id="d6218-187">Click **Deploy Web Service** below the canvas and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="d6218-188">Machine Learning Studio přenosy, můžete na webové služby Azure Machine Learning **nasazení experimentu** stránky.</span><span class="sxs-lookup"><span data-stu-id="d6218-188">Machine Learning Studio transfers you to the Azure Machine Learning web services **Deploy Experiment** page.</span></span>

2. <span data-ttu-id="d6218-189">Zadejte název pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-189">Enter a name for the web service.</span></span> 

3. <span data-ttu-id="d6218-190">Pro **cena plán**, můžete vybrat existující cenový plán, nebo vyberte "Vytvořit nový" a pojmenujte nový plán a vyberte možnost měsíčního plánu.</span><span class="sxs-lookup"><span data-stu-id="d6218-190">For **Price Plan**, you can select an existing pricing plan, or select "Create new" and give the new plan a name and select the monthly plan option.</span></span> <span data-ttu-id="d6218-191">Výchozí plán vrstvy do plánů pro vaši oblast výchozí a webové služby je nasazený na danou oblast.</span><span class="sxs-lookup"><span data-stu-id="d6218-191">The plan tiers default to the plans for your default region and your web service is deployed to that region.</span></span>

4. <span data-ttu-id="d6218-192">Klikněte na tlačítko **nasazení**.</span><span class="sxs-lookup"><span data-stu-id="d6218-192">Click **Deploy**.</span></span>

<span data-ttu-id="d6218-193">Po několika minutách **rychlý Start** otevře se stránka pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-193">After a few minutes, the **Quickstart** page for your web service opens.</span></span>

<span data-ttu-id="d6218-194">Můžete nakonfigurovat službu kliknutím **konfigurace** kartě.</span><span class="sxs-lookup"><span data-stu-id="d6218-194">You can configure the service by clicking the **Configure** tab.</span></span> <span data-ttu-id="d6218-195">Zde můžete upravit službu title a zadejte jeho popis.</span><span class="sxs-lookup"><span data-stu-id="d6218-195">Here you can modify the service title and give it a description.</span></span> 

<span data-ttu-id="d6218-196">K otestování webové služby, klikněte na tlačítko **testování** karta (najdete v části **testování webovou službu** níže).</span><span class="sxs-lookup"><span data-stu-id="d6218-196">To test the web service, click the **Test** tab (see **Test the web service** below).</span></span> <span data-ttu-id="d6218-197">Informace o vytváření aplikací, které můžete přístup k webové službě, klikněte **spotřebě** karta (na další krok v tomto návodu přejde do více podrobností).</span><span class="sxs-lookup"><span data-stu-id="d6218-197">For information on creating applications that can access the web service, click the **Consume** tab (the next step in this walkthrough will go into more detail).</span></span>

> [!TIP]
> <span data-ttu-id="d6218-198">Poté, co nasadíte ji můžete aktualizovat webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-198">You can update the web service after you've deployed it.</span></span> <span data-ttu-id="d6218-199">Například pokud chcete změnit váš model pak výukový experiment můžete upravit, upravit parametry modelu a klikněte na **nasazení webové služby**, vyberete **nasazení webové služby [Classic]** nebo **Nasazení webové služby [nové]**.</span><span class="sxs-lookup"><span data-stu-id="d6218-199">For example, if you want to change your model, then you can edit the training experiment, tweak the model parameters, and click **Deploy Web Service**, selecting **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span> <span data-ttu-id="d6218-200">Při nasazení experiment, nahradí webové službě, teď pomocí aktualizovaném modelu.</span><span class="sxs-lookup"><span data-stu-id="d6218-200">When you deploy the experiment again, it replaces the web service, now using your updated model.</span></span>  
> 
> 

## <a name="test-the-web-service"></a><span data-ttu-id="d6218-201">Test webové služby</span><span class="sxs-lookup"><span data-stu-id="d6218-201">Test the web service</span></span>

<span data-ttu-id="d6218-202">Při přístupu k webové službě uživatele zadá prostřednictvím **webové služby vstup** modulu, kde je předána [Score Model] [ score-model] modulu a skóre.</span><span class="sxs-lookup"><span data-stu-id="d6218-202">When the web service is accessed, the user's data enters through the **Web service input** module where it's passed to the [Score Model][score-model] module and scored.</span></span> <span data-ttu-id="d6218-203">Způsob, jakým jsme zřídili prediktivní experiment, očekává modelu dat ve stejném formátu jako původní datové sady úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6218-203">The way we've set up the predictive experiment, the model expects data in the same format as the original credit risk dataset.</span></span>
<span data-ttu-id="d6218-204">Jsou vráceny výsledky pro uživatele z webové služby pomocí **webové služby výstup** modulu.</span><span class="sxs-lookup"><span data-stu-id="d6218-204">The results are returned to the user from the web service through the **Web service output** module.</span></span>

> [!TIP]
> <span data-ttu-id="d6218-205">Způsob, jak máme prediktivní experiment nakonfigurované, výsledkem celý [Score Model] [ score-model] modulu jsou vráceny.</span><span class="sxs-lookup"><span data-stu-id="d6218-205">The way we have the predictive experiment configured, the entire results from the [Score Model][score-model] module are returned.</span></span> <span data-ttu-id="d6218-206">To zahrnuje všechny vstupní data a hodnota riziko platební a vyhodnocování pravděpodobnosti.</span><span class="sxs-lookup"><span data-stu-id="d6218-206">This includes all the input data plus the credit risk value and the scoring probability.</span></span> <span data-ttu-id="d6218-207">Ale něco jiného může vrátit, pokud chcete – například může vrátit pouze hodnota platební riziko.</span><span class="sxs-lookup"><span data-stu-id="d6218-207">But you can return something different if you want - for example, you could return just the credit risk value.</span></span> <span data-ttu-id="d6218-208">Chcete-li to provést, vložte [sloupce projektu] [ project-columns] modulu mezi [Score Model] [ score-model] a **webové služby výstup**k vyloučení sloupce nechcete webovou službu vrátit.</span><span class="sxs-lookup"><span data-stu-id="d6218-208">To do this, insert a [Project Columns][project-columns] module between [Score Model][score-model] and the **Web service output** to eliminate columns you don't want the web service to return.</span></span> 
> 
> 

<span data-ttu-id="d6218-209">Můžete otestovat Classic webové služby buď v **Machine Learning Studio** nebo **webové služby Azure Machine Learning** portálu.</span><span class="sxs-lookup"><span data-stu-id="d6218-209">You can test a Classic web service either in **Machine Learning Studio** or in the **Azure Machine Learning Web Services** portal.</span></span>
<span data-ttu-id="d6218-210">Můžete otestovat nové webové služby pouze v **webové služby Machine Learning** portálu.</span><span class="sxs-lookup"><span data-stu-id="d6218-210">You can test a New web service only in the **Machine Learning Web Services** portal.</span></span>

> [!TIP]
> <span data-ttu-id="d6218-211">Při testování na portálu Azure Machine Learning webové služby, můžete mít portálu vytvoření ukázkových dat, který můžete použít k testování služby požadavků a odpovědí.</span><span class="sxs-lookup"><span data-stu-id="d6218-211">When testing in the Azure Machine Learning Web Services portal, you can have the portal create sample data that you can use to test the Request-Response service.</span></span> <span data-ttu-id="d6218-212">Na **konfigurace** vyberte "Ano" pro **ukázkových dat povolené?**.</span><span class="sxs-lookup"><span data-stu-id="d6218-212">On the **Configure** page, select "Yes" for **Sample Data Enabled?**.</span></span> <span data-ttu-id="d6218-213">Když otevřete kartu požadavků a odpovědí na **Test** stránce portálu vyplní ukázková data přijatá z původní datové sady úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6218-213">When you open the Request-Response tab on the **Test** page, the portal fills in sample data taken from the original credit risk dataset.</span></span>

### <a name="test-a-classic-web-service"></a><span data-ttu-id="d6218-214">Testování Classic webové služby</span><span class="sxs-lookup"><span data-stu-id="d6218-214">Test a Classic web service</span></span>

<span data-ttu-id="d6218-215">Webová služba Classic můžete otestovat v nástroji Machine Learning Studio nebo na portálu webové služby Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d6218-215">You can test a Classic web service in Machine Learning Studio or in the Machine Learning Web Services portal.</span></span> 

#### <a name="test-in-machine-learning-studio"></a><span data-ttu-id="d6218-216">Testování v nástroji Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="d6218-216">Test in Machine Learning Studio</span></span>

1. <span data-ttu-id="d6218-217">Na **řídicí panel** stránky pro webovou službu, klikněte na tlačítko **Test** tlačítko pod **výchozí koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="d6218-217">On the **DASHBOARD** page for the web service, click the **Test** button under **Default Endpoint**.</span></span> <span data-ttu-id="d6218-218">Zobrazí se dialogové okno se zobrazí a se vás zeptá na vstupní data pro službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-218">A dialog pops up and asks you for the input data for the service.</span></span> <span data-ttu-id="d6218-219">Jedná se o stejné sloupce, které se zobrazovaly v původní datové sady úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6218-219">These are the same columns that appeared in the original credit risk dataset.</span></span>  

2. <span data-ttu-id="d6218-220">Zadejte sadu dat a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6218-220">Enter a set of data and then click **OK**.</span></span> 

#### <a name="test-in-the-machine-learning-web-services-portal"></a><span data-ttu-id="d6218-221">Testování v portálu webové služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d6218-221">Test in the Machine Learning Web Services portal</span></span>

1. <span data-ttu-id="d6218-222">Na **řídicí panel** stránky pro webovou službu, klikněte na tlačítko **testovací preview** v části **výchozí koncový bod**.</span><span class="sxs-lookup"><span data-stu-id="d6218-222">On the **DASHBOARD** page for the web service, click the **Test preview** link under **Default Endpoint**.</span></span> <span data-ttu-id="d6218-223">Zkušební stránku na portálu Azure Machine Learning webové služby pro koncový bod webové služby se otevře a se vás zeptá na vstupní data pro službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-223">The test page in the Azure Machine Learning Web Services portal for the web service endpoint opens and asks you for the input data for the service.</span></span> <span data-ttu-id="d6218-224">Jedná se o stejné sloupce, které se zobrazovaly v původní datové sady úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6218-224">These are the same columns that appeared in the original credit risk dataset.</span></span>

2. <span data-ttu-id="d6218-225">Klikněte na tlačítko **testování požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="d6218-225">Click **Test Request-Response**.</span></span> 

### <a name="test-a-new-web-service"></a><span data-ttu-id="d6218-226">Otestovat novou webovou službu</span><span class="sxs-lookup"><span data-stu-id="d6218-226">Test a New web service</span></span>

<span data-ttu-id="d6218-227">Pouze na portálu webové služby Machine Learning můžete testovat novou webovou službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-227">You can test a New web service only in the Machine Learning Web Services portal.</span></span>

1. <span data-ttu-id="d6218-228">V [webové služby Azure Machine Learning](https://services.azureml.net/quickstart) portálu klikněte na **Test** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d6218-228">In the [Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal, click **Test** at the top of the page.</span></span> <span data-ttu-id="d6218-229">**Test** otevře se stránka a můžete vstupní data pro službu.</span><span class="sxs-lookup"><span data-stu-id="d6218-229">The **Test** page opens and you can input data for the service.</span></span> <span data-ttu-id="d6218-230">Vstupní pole zobrazí odpovídají na sloupce, které se zobrazovaly v původní datové sady úvěrové riziko.</span><span class="sxs-lookup"><span data-stu-id="d6218-230">The input fields displayed correspond to the columns that appeared in the original credit risk dataset.</span></span> 

2. <span data-ttu-id="d6218-231">Zadejte sadu dat a pak klikněte na tlačítko **testu požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="d6218-231">Enter a set of data and then click **Test Request-Response**.</span></span>

<span data-ttu-id="d6218-232">Výsledky testu se zobrazí na pravé straně stránky v výstupního sloupce.</span><span class="sxs-lookup"><span data-stu-id="d6218-232">The results of the test are displayed on the right-hand side of the page in the output column.</span></span> 


## <a name="manage-the-web-service"></a><span data-ttu-id="d6218-233">Správa webové služby</span><span class="sxs-lookup"><span data-stu-id="d6218-233">Manage the web service</span></span>

### <a name="manage-a-classic-web-service-in-the-azure-classic-portal"></a><span data-ttu-id="d6218-234">Správa webové služby klasického portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="d6218-234">Manage a Classic web service in the Azure classic portal</span></span>

<span data-ttu-id="d6218-235">Když máte ukázku nasazenou webovou službu Classic, můžete spravovat z [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="d6218-235">Once you've deployed your Classic web service, you can manage it from the [Azure classic portal](https://manage.windowsazure.com).</span></span>

1. <span data-ttu-id="d6218-236">Přihlaste se k [portál Azure classic](https://manage.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="d6218-236">Sign in to the [Azure classic portal](https://manage.windowsazure.com)</span></span>
2. <span data-ttu-id="d6218-237">Na panelu služby Microsoft Azure, klikněte na tlačítko **MACHINE LEARNING**</span><span class="sxs-lookup"><span data-stu-id="d6218-237">In the Microsoft Azure services panel, click **MACHINE LEARNING**</span></span>
3. <span data-ttu-id="d6218-238">Klikněte na pracovní prostor</span><span class="sxs-lookup"><span data-stu-id="d6218-238">Click your workspace</span></span>
4. <span data-ttu-id="d6218-239">Klikněte **webové služby** karta</span><span class="sxs-lookup"><span data-stu-id="d6218-239">Click the **Web services** tab</span></span>
5. <span data-ttu-id="d6218-240">Klikněte na webovou službu, kterou jsme vytvořili</span><span class="sxs-lookup"><span data-stu-id="d6218-240">Click the web service we created</span></span>
6. <span data-ttu-id="d6218-241">Klikněte na koncový bod "Výchozí"</span><span class="sxs-lookup"><span data-stu-id="d6218-241">Click the "default" endpoint</span></span>

<span data-ttu-id="d6218-242">Tady můžete provést akce, jako je monitorování, jak je to webovou službu a vylepšení výkonu zkontrolujte změnou, kolik souběžných volání služby může zpracovat.</span><span class="sxs-lookup"><span data-stu-id="d6218-242">From here, you can do things like monitor how the web service is doing and make performance tweaks by changing how many concurrent calls the service can handle.</span></span>

<span data-ttu-id="d6218-243">Další podrobnosti najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d6218-243">For more details, see:</span></span>

* [<span data-ttu-id="d6218-244">Vytváření koncových bodů</span><span class="sxs-lookup"><span data-stu-id="d6218-244">Creating Endpoints</span></span>](machine-learning-create-endpoint.md)
* [<span data-ttu-id="d6218-245">Škálování webové služby</span><span class="sxs-lookup"><span data-stu-id="d6218-245">Scaling web service</span></span>](machine-learning-scaling-webservice.md)

### <a name="manage-a-classic-or-new-web-service-in-the-azure-machine-learning-web-services-portal"></a><span data-ttu-id="d6218-246">Spravovat na klasickém nebo novou webovou službu na portálu Azure Machine Learning webové služby</span><span class="sxs-lookup"><span data-stu-id="d6218-246">Manage a Classic or New web service in the Azure Machine Learning Web Services portal</span></span>

<span data-ttu-id="d6218-247">Po nasazení webové služby, zda Classic nebo nové, můžete spravovat z [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portálu.</span><span class="sxs-lookup"><span data-stu-id="d6218-247">Once you've deployed your web service, whether Classic or New, you can manage it from the [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal.</span></span>

<span data-ttu-id="d6218-248">Sledovat výkon webové služby:</span><span class="sxs-lookup"><span data-stu-id="d6218-248">To monitor the performance of your web service:</span></span>

1. <span data-ttu-id="d6218-249">Přihlaste se k [webové služby aplikace Microsoft Azure Machine Learning](https://services.azureml.net/quickstart) portálu</span><span class="sxs-lookup"><span data-stu-id="d6218-249">Sign in to the [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) portal</span></span>
2. <span data-ttu-id="d6218-250">Klikněte na tlačítko **webové služby**</span><span class="sxs-lookup"><span data-stu-id="d6218-250">Click **Web services**</span></span>
3. <span data-ttu-id="d6218-251">Klikněte na webovou službu</span><span class="sxs-lookup"><span data-stu-id="d6218-251">Click your web service</span></span>
4. <span data-ttu-id="d6218-252">Klikněte **řídicí panel**</span><span class="sxs-lookup"><span data-stu-id="d6218-252">Click the **Dashboard**</span></span>

- - -
<span data-ttu-id="d6218-253">**Další krok: [přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="d6218-253">**Next: [Access the web service](machine-learning-walkthrough-6-access-web-service.md)**</span></span>

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
