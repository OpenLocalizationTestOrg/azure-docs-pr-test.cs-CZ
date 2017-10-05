---
title: "Postup přípravy modelu pro nasazení v nástroji Azure Machine Learning Studio | Microsoft Docs"
description: "Postup přípravy trained model nasazení jako webovou službu převedením experimentu Machine Learning Studio školení na prediktivní experiment."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: eb943c45-541a-401d-844a-c3337de82da6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: garye
ms.openlocfilehash: 716a9a9b723df7ff6eb111fa40f2b5941d57d67a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-prepare-your-model-for-deployment-in-azure-machine-learning-studio"></a><span data-ttu-id="dec46-103">Postup přípravy modelu pro nasazení v nástroji Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="dec46-103">How to prepare your model for deployment in Azure Machine Learning Studio</span></span>

<span data-ttu-id="dec46-104">Azure Machine Learning Studio poskytuje nástroje, které potřebujete k vývoji modelu prediktivní analýzy a zprovoznit ho pomocí jeho nasazení jako Azure webové služby.</span><span class="sxs-lookup"><span data-stu-id="dec46-104">Azure Machine Learning Studio gives you the tools you need to develop a predictive analytics model and then operationalize it by deploying it as an Azure web service.</span></span>

<span data-ttu-id="dec46-105">K tomuto účelu použijete k vytvoření experimentu - názvem Studio *výukový experiment* – kde jste trénování, stanovení skóre a upravovat modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-105">To do this, you use Studio to create an experiment - called a *training experiment* - where you train, score, and edit your model.</span></span> <span data-ttu-id="dec46-106">Jakmile budete spokojeni, můžete připravit váš model nasazení na základě převedení experimentu školení pro *prediktivní experiment* nakonfigurovaný skóre uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="dec46-106">Once you're satisfied, you get your model ready to deploy by converting your training experiment to a *predictive experiment* that's configured to score user data.</span></span>

<span data-ttu-id="dec46-107">Můžete zobrazit příklad tohoto procesu v [návod: vývoj řešení prediktivní analýzy pro posuzování úvěrového rizika v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span><span class="sxs-lookup"><span data-stu-id="dec46-107">You can see an example of this process in [Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).</span></span>

<span data-ttu-id="dec46-108">Tento článek přebírá podrobné informace na podrobné informace o tom, jak získá výukový experiment převeden do prediktivní experiment a nasazení této prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="dec46-108">This article takes a deep dive into the details of how a training experiment gets converted into a predictive experiment, and how that predictive experiment is deployed.</span></span> <span data-ttu-id="dec46-109">Pochopením tyto podrobnosti můžete zjistěte, jak nakonfigurovat vaše nasazené model, aby efektivnější.</span><span class="sxs-lookup"><span data-stu-id="dec46-109">By understanding these details, you can learn how to configure your deployed model to make it more effective.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview"></a><span data-ttu-id="dec46-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="dec46-110">Overview</span></span> 

<span data-ttu-id="dec46-111">Proces převod výukový experiment prediktivní experiment zahrnuje tři kroky:</span><span class="sxs-lookup"><span data-stu-id="dec46-111">The process of converting a training experiment to a predictive experiment involves three steps:</span></span>

1. <span data-ttu-id="dec46-112">Nahraďte strojového učení algoritmu modulů s trained model.</span><span class="sxs-lookup"><span data-stu-id="dec46-112">Replace the machine learning algorithm modules with your trained model.</span></span>
2. <span data-ttu-id="dec46-113">Trim experimentu pouze moduly, které jsou potřebné pro vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="dec46-113">Trim the experiment to only those modules that are needed for scoring.</span></span> <span data-ttu-id="dec46-114">Výukový experiment obsahuje několik modulů, které jsou nezbytné, aby školení ale nejsou potřebné, jakmile je cvičení modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-114">A training experiment includes a number of modules that are necessary for training but are not needed once the model is trained.</span></span>
3. <span data-ttu-id="dec46-115">Definujte, jak váš model přijímá data od uživatele webové služby, a jaká data budou vráceny.</span><span class="sxs-lookup"><span data-stu-id="dec46-115">Define how your model will accept data from the web service user, and what data will be returned.</span></span>

> [!TIP]
> <span data-ttu-id="dec46-116">V experimentu školení jste byla nevadí, školení a vyhodnocování model pomocí svoje vlastní data.</span><span class="sxs-lookup"><span data-stu-id="dec46-116">In your training experiment, you've been concerned with training and scoring your model using your own data.</span></span> <span data-ttu-id="dec46-117">Ale po nasazení uživatele bude posílat nová data do modelu a vrátí výsledky předpovědi.</span><span class="sxs-lookup"><span data-stu-id="dec46-117">But once deployed, users will send new data to your model and it will return prediction results.</span></span> <span data-ttu-id="dec46-118">Ano jak převedete výukový experiment prediktivní experiment a připravit ji pro nasazení, mějte na paměti použití modelu třetími stranami.</span><span class="sxs-lookup"><span data-stu-id="dec46-118">So, as you convert your training experiment to a predictive experiment to get it ready for deployment, keep in mind how the model will be used by others.</span></span>
> 
> 

## <a name="set-up-web-service-button"></a><span data-ttu-id="dec46-119">Tlačítko nastavit webové služby</span><span class="sxs-lookup"><span data-stu-id="dec46-119">Set Up Web Service button</span></span>
<span data-ttu-id="dec46-120">Po spuštění experimentu (klikněte na tlačítko **spustit** v dolní části plátna experimentu), klikněte na tlačítko **nastavit webové služby** tlačítko (vyberte **webové služby prediktivní** možnost).</span><span class="sxs-lookup"><span data-stu-id="dec46-120">After you run your experiment (click **RUN** at the bottom of the experiment canvas), click the **Set Up Web Service** button (select the **Predictive Web Service** option).</span></span> <span data-ttu-id="dec46-121">**Nastavení webové služby** pro vás provede převod výukový experiment prediktivní experiment tři kroky:</span><span class="sxs-lookup"><span data-stu-id="dec46-121">**Set Up Web Service** performs for you the three steps of converting your training experiment to a predictive experiment:</span></span>

1. <span data-ttu-id="dec46-122">Ukládá vaše trained model v **Trénované modely** části palety modulů (nalevo od plátna experimentu).</span><span class="sxs-lookup"><span data-stu-id="dec46-122">It saves your trained model in the **Trained Models** section of the module palette (to the left of the experiment canvas).</span></span> <span data-ttu-id="dec46-123">Nahradí algoritmu strojového učení a [Train Model] [ train-model] modulů s uložené naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-123">It then replaces the machine learning algorithm and [Train Model][train-model] modules with the saved trained model.</span></span>
2. <span data-ttu-id="dec46-124">Ho analyzuje experimentu a odebere moduly, které byly jasně používají pouze pro školení a už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="dec46-124">It analyzes your experiment and removes modules that were clearly used only for training and are no longer needed.</span></span>
3. <span data-ttu-id="dec46-125">Vloží _webové služby vstup_ a _výstup_ moduly do výchozího umístění v experimentu (tyto moduly přijmout a vrátit data uživatele).</span><span class="sxs-lookup"><span data-stu-id="dec46-125">It inserts _Web service input_ and _output_ modules into default locations in your experiment (these modules accept and return user data).</span></span>

<span data-ttu-id="dec46-126">Například následující experimentu soupravy two-class boosted decision stromu modelu pomocí ukázkových dat úplné zjišťování:</span><span class="sxs-lookup"><span data-stu-id="dec46-126">For example, the following experiment trains a two-class boosted decision tree model using sample census data:</span></span>

![Výukový experiment][figure1]

<span data-ttu-id="dec46-128">Modulů do tohoto experimentu proveďte v podstatě čtyři různé funkce:</span><span class="sxs-lookup"><span data-stu-id="dec46-128">The modules in this experiment perform basically four different functions:</span></span>

![Funkce modulu][figure2]

<span data-ttu-id="dec46-130">Pokud převedete tento výukový experiment prediktivní experiment, některé z těchto modulů už nejsou potřeba nebo se teď mají různé účely:</span><span class="sxs-lookup"><span data-stu-id="dec46-130">When you convert this training experiment to a predictive experiment, some of these modules are no longer needed, or they now serve a different purpose:</span></span>

* <span data-ttu-id="dec46-131">**Data** -data v této datové sadě ukázka nepoužívá během vyhodnocování – uživatel webové služby se zadat data, která mají být vypočítat skóre.</span><span class="sxs-lookup"><span data-stu-id="dec46-131">**Data** - The data in this sample dataset is not used during scoring - the user of the web service will supply the data to be scored.</span></span> <span data-ttu-id="dec46-132">Metadata z této datové sady, jako je například datové typy, ale používá pro cvičný model.</span><span class="sxs-lookup"><span data-stu-id="dec46-132">However, the metadata from this dataset, such as data types, is used by the trained model.</span></span> <span data-ttu-id="dec46-133">Proto je třeba zachovat datovou sadu v prediktivní experiment tak, že může poskytovat tato metadata.</span><span class="sxs-lookup"><span data-stu-id="dec46-133">So you need to keep the dataset in the predictive experiment so that it can provide this metadata.</span></span>

* <span data-ttu-id="dec46-134">**Příprava** – v závislosti na tom uživatelská data, která bude odeslána pro vyhodnocování, tyto moduly může nebo nemusí být potřeba zpracovat příchozí data.</span><span class="sxs-lookup"><span data-stu-id="dec46-134">**Prep** - Depending on the user data that will be submitted for scoring, these modules may or may not be necessary to process the incoming data.</span></span> <span data-ttu-id="dec46-135">**Nastavit webové služby** tlačítko nemá touch tyto – musíte se rozhodnout, jak chcete mohli je zpracovat.</span><span class="sxs-lookup"><span data-stu-id="dec46-135">The **Set Up Web Service** button doesn't touch these - you need to decide how you want to handle them.</span></span>
  
    <span data-ttu-id="dec46-136">Například v tomto příkladu ukázkovou datovou sadu může mít chybějící hodnoty, takže [vyčištění chybějících dat] [ clean-missing-data] modul byl zahrnut jejich odstranění.</span><span class="sxs-lookup"><span data-stu-id="dec46-136">For instance, in this example the sample dataset may have missing values, so a [Clean Missing Data][clean-missing-data] module was included to deal with them.</span></span> <span data-ttu-id="dec46-137">Také obsahuje ukázkovou datovou sadu sloupců, které nejsou potřebné pro trénování modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-137">Also, the sample dataset includes columns that are not needed to train the model.</span></span> <span data-ttu-id="dec46-138">Proto [výběr sloupců v datové sadě] [ select-columns] modul byl zahrnut vyloučit tyto další sloupce z datového toku.</span><span class="sxs-lookup"><span data-stu-id="dec46-138">So a [Select Columns in Dataset][select-columns] module was included to exclude those extra columns from the data flow.</span></span> <span data-ttu-id="dec46-139">Pokud víte, že data, která bude odeslána pro vyhodnocování prostřednictvím webové služby nebude mít chybějící hodnoty a pak můžete odebrat [vyčištění chybějících dat] [ clean-missing-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="dec46-139">If you know that the data that will be submitted for scoring through the web service will not have missing values, then you can remove the [Clean Missing Data][clean-missing-data] module.</span></span> <span data-ttu-id="dec46-140">Ale protože [výběr sloupců v datové sadě] [ select-columns] modulu pomáhá definovat sloupce dat, které pro cvičný model očekává, že modul musí zůstat.</span><span class="sxs-lookup"><span data-stu-id="dec46-140">However, since the [Select Columns in Dataset][select-columns] module helps define the columns of data that the trained model expects, that module needs to remain.</span></span>

* <span data-ttu-id="dec46-141">**Cvičení** -tyto moduly jsou použity při cvičení modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-141">**Train** - These modules are used to train the model.</span></span> <span data-ttu-id="dec46-142">Když kliknete na tlačítko **nastavit webové služby**, nahradí tyto moduly se jeden modul, který obsahuje můžete cvičení modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-142">When you click **Set Up Web Service**, these modules are replaced with a single module that contains the model you trained.</span></span> <span data-ttu-id="dec46-143">Tento nový modul se uloží do **Trénované modely** části palety modulů.</span><span class="sxs-lookup"><span data-stu-id="dec46-143">This new module is saved in the **Trained Models** section of the module palette.</span></span>

* <span data-ttu-id="dec46-144">**Skóre** – v tomto příkladu [rozdělení dat] [ split] modul se používá k rozdělení datový proud do testovací data a Cvičná data.</span><span class="sxs-lookup"><span data-stu-id="dec46-144">**Score** - In this example, the [Split Data][split] module is used to divide the data stream into test data and training data.</span></span> <span data-ttu-id="dec46-145">V prediktivní experiment jsme nejsou cvičení už nepotřebujeme, takže [rozdělení dat] [ split] lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="dec46-145">In the predictive experiment, we're not training anymore, so [Split Data][split] can be removed.</span></span> <span data-ttu-id="dec46-146">Podobně, druhý [Score Model] [ score-model] modulu a [Evaluate Model] [ evaluate-model] modul se používá k porovnání výsledků v testovacích datech, takže tyto moduly nejsou potřebné v prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="dec46-146">Similarly, the second [Score Model][score-model] module and the [Evaluate Model][evaluate-model] module are used to compare results from the test data, so these modules are not needed in the predictive experiment.</span></span> <span data-ttu-id="dec46-147">Zbývající [Score Model] [ score-model] , ale není potřeba modul vrácení výsledku skóre prostřednictvím webové služby.</span><span class="sxs-lookup"><span data-stu-id="dec46-147">The remaining [Score Model][score-model] module, however, is needed to return a score result through the web service.</span></span>

<span data-ttu-id="dec46-148">Zde je, jak našem příkladu vypadá po kliknutí na **nastavit webové služby**:</span><span class="sxs-lookup"><span data-stu-id="dec46-148">Here is how our example looks after clicking **Set Up Web Service**:</span></span>

![Převést prediktivní experiment][figure3]

<span data-ttu-id="dec46-150">Podle **nastavit webové služby** může být plně dostačující k přípravě experimentu nasadit jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="dec46-150">The work done by **Set Up Web Service** may be sufficient to prepare your experiment to be deployed as a web service.</span></span> <span data-ttu-id="dec46-151">Můžete však provést některé další kroky, které jsou specifické pro experimentu.</span><span class="sxs-lookup"><span data-stu-id="dec46-151">However, you may want to do some additional work specific to your experiment.</span></span>

### <a name="adjust-input-and-output-modules"></a><span data-ttu-id="dec46-152">Upravte vstupní a výstupní moduly</span><span class="sxs-lookup"><span data-stu-id="dec46-152">Adjust input and output modules</span></span>
<span data-ttu-id="dec46-153">V experimentu školení používá sadu Cvičná data a pak se některé zpracování pro získání dat ve formuláři, který algoritmus machine learning potřeby.</span><span class="sxs-lookup"><span data-stu-id="dec46-153">In your training experiment, you used a set of training data and then did some processing to get the data in a form that the machine learning algorithm needed.</span></span> <span data-ttu-id="dec46-154">Pokud toto zpracování nebude nutné data očekáváte získat prostřednictvím webové služby, můžete ho obejít: připojit výstup **vstupní modul webové služby** do jiného modulu v experimentu.</span><span class="sxs-lookup"><span data-stu-id="dec46-154">If the data you expect to receive through the web service will not need this processing, you can bypass it: connect the output of the **Web service input module** to a different module in your experiment.</span></span> <span data-ttu-id="dec46-155">Data uživatele nyní dorazí v modelu v tomto umístění.</span><span class="sxs-lookup"><span data-stu-id="dec46-155">The user's data will now arrive in the model at this location.</span></span>

<span data-ttu-id="dec46-156">Například ve výchozím nastavení **nastavit webové služby** umístí **webové služby vstup** modulu v horní části datový tok, jak je znázorněno na obrázku výše.</span><span class="sxs-lookup"><span data-stu-id="dec46-156">For example, by default **Set Up Web Service** puts the **Web service input** module at the top of your data flow, as shown in the figure above.</span></span> <span data-ttu-id="dec46-157">Ale můžete ručně umístit **webové služby vstup** po zpracování dat moduly:</span><span class="sxs-lookup"><span data-stu-id="dec46-157">But we can manually position the **Web service input** past the data processing modules:</span></span>

![Přesunutí vstup webové služby][figure4]

<span data-ttu-id="dec46-159">Vstupní data poskytnutá prostřednictvím webové služby předá nyní přímo do modul určení skóre modelu bez jakékoli předběžného zpracování.</span><span class="sxs-lookup"><span data-stu-id="dec46-159">The input data provided through the web service will now pass directly into the Score Model module without any preprocessing.</span></span>

<span data-ttu-id="dec46-160">Podobně, ve výchozím nastavení **nastavit webové služby** uloží modul výstupní webové služby v dolní části datový tok.</span><span class="sxs-lookup"><span data-stu-id="dec46-160">Similarly, by default **Set Up Web Service** puts the Web service output module at the bottom of your data flow.</span></span> <span data-ttu-id="dec46-161">V tomto příkladu webovou službu vrátí uživateli výstup [Score Model] [ score-model] modul, který se skládá z vektoru dokončení vstupní data a vyhodnocování výsledky.</span><span class="sxs-lookup"><span data-stu-id="dec46-161">In this example, the web service will return to the user the output of the [Score Model][score-model] module, which includes the complete input data vector plus the scoring results.</span></span>
<span data-ttu-id="dec46-162">Ale pokud chcete vrátit něco jiný, pak můžete přidat další moduly před **webové služby výstup** modulu.</span><span class="sxs-lookup"><span data-stu-id="dec46-162">However, if you would prefer to return something different, then you can add additional modules before the **Web service output** module.</span></span> 

<span data-ttu-id="dec46-163">Například pokud chcete vrátit pouze vyhodnocování výsledky a není celý vektoru vstupních dat, přidejte [výběr sloupců v datové sadě] [ select-columns] modulu vyloučit všechny sloupce kromě vyhodnocování výsledky.</span><span class="sxs-lookup"><span data-stu-id="dec46-163">For example, to return only the scoring results and not the entire vector of input data, add a [Select Columns in Dataset][select-columns] module to exclude all columns except the scoring results.</span></span> <span data-ttu-id="dec46-164">Pak přejděte **webové služby výstup** modulu výstup [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="dec46-164">Then move the **Web service output** module to the output of the [Select Columns in Dataset][select-columns] module.</span></span> <span data-ttu-id="dec46-165">Experiment vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="dec46-165">The experiment looks like this:</span></span>

![Přesunutí výstup webové služby][figure5]

### <a name="add-or-remove-additional-data-processing-modules"></a><span data-ttu-id="dec46-167">Přidat nebo odebrat moduly další zpracování dat</span><span class="sxs-lookup"><span data-stu-id="dec46-167">Add or remove additional data processing modules</span></span>
<span data-ttu-id="dec46-168">Pokud existují další moduly v experimentu víte, že nebude potřeba při vyhodnocování, tyto lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="dec46-168">If there are more modules in your experiment that you know will not be needed during scoring, these can be removed.</span></span> <span data-ttu-id="dec46-169">Například protože jsme přesunout **webové služby vstup** modulu bod po zpracování dat moduly, jsme odebrat [vyčištění chybějících dat] [ clean-missing-data] modulu z prediktivní experiment.</span><span class="sxs-lookup"><span data-stu-id="dec46-169">For example, because we moved the **Web service input** module to a point after the data processing modules, we can remove the [Clean Missing Data][clean-missing-data] module from the predictive experiment.</span></span>

<span data-ttu-id="dec46-170">Naše prediktivní experiment nyní vypadat třeba takto:</span><span class="sxs-lookup"><span data-stu-id="dec46-170">Our predictive experiment now looks like this:</span></span>

![Odebrání dalších modulu][figure6]


### <a name="add-optional-web-service-parameters"></a><span data-ttu-id="dec46-172">Přidejte volitelné parametry webové služby</span><span class="sxs-lookup"><span data-stu-id="dec46-172">Add optional Web Service Parameters</span></span>
<span data-ttu-id="dec46-173">V některých případech můžete povolit uživateli webové služby můžete změnit chování modulů při přístupu k službě.</span><span class="sxs-lookup"><span data-stu-id="dec46-173">In some cases, you may want to allow the user of your web service to change the behavior of modules when the service is accessed.</span></span> <span data-ttu-id="dec46-174">*Parametry webové služby* vám umožní to udělat.</span><span class="sxs-lookup"><span data-stu-id="dec46-174">*Web Service Parameters* allow you to do this.</span></span>

<span data-ttu-id="dec46-175">Běžným příkladem je nastavení [importovat Data] [ import-data] modulu, takže uživatel nasazenou webovou službu, můžete zadat jiný zdroj dat při přístupu k webové službě.</span><span class="sxs-lookup"><span data-stu-id="dec46-175">A common example is setting up an [Import Data][import-data] module so the user of the deployed web service can specify a different data source when the web service is accessed.</span></span> <span data-ttu-id="dec46-176">Nebo konfigurace [Export dat] [ export-data] modulu tak, aby lze zadat jiný cíl.</span><span class="sxs-lookup"><span data-stu-id="dec46-176">Or configuring an [Export Data][export-data] module so that a different destination can be specified.</span></span>

<span data-ttu-id="dec46-177">Můžete definovat parametry webové služby a přidružit jeden nebo více parametrů modulu a můžete určit, jestli jsou požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="dec46-177">You can define Web Service Parameters and associate them with one or more module parameters, and you can specify whether they are required or optional.</span></span> <span data-ttu-id="dec46-178">Při přístupu služby a modul akce jsou upraveny uživatele webové služby obsahuje hodnoty pro tyto parametry.</span><span class="sxs-lookup"><span data-stu-id="dec46-178">The user of the web service provides values for these parameters when the service is accessed, and the module actions are modified accordingly.</span></span>

<span data-ttu-id="dec46-179">Další informace o jaké jsou parametry webové služby a jejich použití naleznete v tématu [pomocí parametry webové služby Azure Machine Learning][webserviceparameters].</span><span class="sxs-lookup"><span data-stu-id="dec46-179">For more information about what Web Service Parameters are and how to use them, see [Using Azure Machine Learning Web Service Parameters][webserviceparameters].</span></span>

[webserviceparameters]: machine-learning-web-service-parameters.md


## <a name="deploy-the-predictive-experiment-as-a-web-service"></a><span data-ttu-id="dec46-180">Nasadit prediktivní experiment jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="dec46-180">Deploy the predictive experiment as a web service</span></span>
<span data-ttu-id="dec46-181">Teď, když dostatečně připravený prediktivní experiment, můžete ho nasadit jako Azure webové služby.</span><span class="sxs-lookup"><span data-stu-id="dec46-181">Now that the predictive experiment has been sufficiently prepared, you can deploy it as an Azure web service.</span></span> <span data-ttu-id="dec46-182">Pomocí webové služby, mohou uživatelé odesílat data do modelu a vrátí její předpovědi modelu.</span><span class="sxs-lookup"><span data-stu-id="dec46-182">Using the web service, users can send data to your model and the model will return its predictions.</span></span>

<span data-ttu-id="dec46-183">Další informace o procesu kompletní nasazení najdete v tématu [nasazení webové služby Azure Machine Learning][deploy]</span><span class="sxs-lookup"><span data-stu-id="dec46-183">For more information on the complete deployment process, see [Deploy an Azure Machine Learning web service][deploy]</span></span>

[deploy]: machine-learning-publish-a-machine-learning-web-service.md


<!-- Images -->
[figure1]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure1.png
[figure2]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure2.png
[figure3]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure3.png
[figure4]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure4.png
[figure5]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure5.png
[figure6]:./media/machine-learning-convert-training-experiment-to-scoring-experiment/figure6.png


<!-- Module References -->
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[export-data]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
