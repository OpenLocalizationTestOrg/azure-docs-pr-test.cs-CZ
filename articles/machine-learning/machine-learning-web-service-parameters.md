---
title: "aaaUse parametry webové služby Azure Machine Learning | Microsoft Docs"
description: "Jak toouse parametry webové služby Azure Machine Learning toomodify hello chování modelu při přístupu k hello webové služby."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 214711eb819a6cea34db905abdf015da11e846d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a><span data-ttu-id="36c41-103">Použití parametrů webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="36c41-103">Use Azure Machine Learning Web Service Parameters</span></span>
<span data-ttu-id="36c41-104">Webové služby Azure Machine Learning je vytvořen publikování experimentu, který obsahuje moduly, které se dají konfigurovat parametry.</span><span class="sxs-lookup"><span data-stu-id="36c41-104">An Azure Machine Learning web service is created by publishing an experiment that contains modules with configurable parameters.</span></span> <span data-ttu-id="36c41-105">V některých případech můžete toochange hello modulu chování při spuštění hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-105">In some cases, you may want toochange hello module behavior while hello web service is running.</span></span> <span data-ttu-id="36c41-106">*Parametry webové služby* umožňují toodo této úlohy.</span><span class="sxs-lookup"><span data-stu-id="36c41-106">*Web Service Parameters* allow you toodo this task.</span></span> 

<span data-ttu-id="36c41-107">Běžným příkladem je nastavení hello [importovat Data] [ reader] modulu tak daného uživatele a hello hello publikované webové služby můžete zadat jiný zdroj dat, při přístupu k hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-107">A common example is setting up hello [Import Data][reader] module so that hello user of hello published web service can specify a different data source when hello web service is accessed.</span></span> <span data-ttu-id="36c41-108">Nebo konfigurace hello [Export dat] [ writer] modulu tak, aby lze zadat jiný cíl.</span><span class="sxs-lookup"><span data-stu-id="36c41-108">Or configuring hello [Export Data][writer] module so that a different destination can be specified.</span></span> <span data-ttu-id="36c41-109">Mezi další příklady patří změna hello počet bitů pro hello [Hashování] [ feature-hashing] modulu nebo hello počet požadovaných funkcí pro hello [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu.</span><span class="sxs-lookup"><span data-stu-id="36c41-109">Some other examples include changing hello number of bits for hello [Feature Hashing][feature-hashing] module or hello number of desired features for hello [Filter-Based Feature Selection][filter-based-feature-selection] module.</span></span> 

<span data-ttu-id="36c41-110">Můžete nastavit parametry webové služby a je přidružit jeden nebo více parametrů modulu v experimentu a můžete určit, jestli jsou požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="36c41-110">You can set Web Service Parameters and associate them with one or more module parameters in your experiment, and you can specify whether they are required or optional.</span></span> <span data-ttu-id="36c41-111">uživatel Hello hello webové služby můžete poté zadejte hodnoty pro tyto parametry při volání webové služby hello.</span><span class="sxs-lookup"><span data-stu-id="36c41-111">hello user of hello web service can then provide values for these parameters when they call hello web service.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-tooset-and-use-web-service-parameters"></a><span data-ttu-id="36c41-112">Jak tooset a použijte parametry webové služby</span><span class="sxs-lookup"><span data-stu-id="36c41-112">How tooset and use Web Service Parameters</span></span>
<span data-ttu-id="36c41-113">Parametr webové služby můžete definovat na hello ikonu další toohello parametru pro modul a výběrem "Nastavit jako parametr webové služby".</span><span class="sxs-lookup"><span data-stu-id="36c41-113">You define a Web Service Parameter by clicking hello icon next toohello parameter for a module and selecting "Set as web service parameter".</span></span> <span data-ttu-id="36c41-114">Tím se vytvoří nový parametr webové služby a připojí ho toothat modulu parametr.</span><span class="sxs-lookup"><span data-stu-id="36c41-114">This creates a new Web Service Parameter and connects it toothat module parameter.</span></span> <span data-ttu-id="36c41-115">Potom při přístupu k webové službě hello hello uživatele můžete zadat hodnotu pro hello parametr webové služby a je použité toohello modulu parametr.</span><span class="sxs-lookup"><span data-stu-id="36c41-115">Then, when hello web service is accessed, hello user can specify a value for hello Web Service Parameter and it is applied toohello module parameter.</span></span>

<span data-ttu-id="36c41-116">Jakmile definujete parametr webové služby, je k dispozici tooany druhý parametr modulu v experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="36c41-116">Once you define a Web Service Parameter, it's available tooany other module parameter in hello experiment.</span></span> <span data-ttu-id="36c41-117">Pokud definujete webové služby parametr spojený s parametrem pro jeden modul, můžete použít tento stejný parametr webové služby pro ostatní moduly, tak dlouho, dokud hello parametr očekává hello stejný typ hodnoty.</span><span class="sxs-lookup"><span data-stu-id="36c41-117">If you define a Web Service Parameter associated with a parameter for one module, you can use that same Web Service Parameter for any other module, as long as hello parameter expects hello same type of value.</span></span> <span data-ttu-id="36c41-118">Například pokud hello parametr webové služby je číselná hodnota, pak lze být použít pouze pro parametry modulu, které očekávají číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="36c41-118">For example, if hello Web Service Parameter is a numeric value, then it can only be used for module parameters that expect a numeric value.</span></span> <span data-ttu-id="36c41-119">Když uživatel hello nastaví hodnotu pro hello parametr webové služby, bude použité tooall přidružené parametry modulu.</span><span class="sxs-lookup"><span data-stu-id="36c41-119">When hello user sets a value for hello Web Service Parameter, it will be applied tooall associated module parameters.</span></span>

<span data-ttu-id="36c41-120">Můžete rozhodnout, zda tooprovide výchozí hodnoty pro hello parametr webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-120">You can decide whether tooprovide a default value for hello Web Service Parameter.</span></span> <span data-ttu-id="36c41-121">Pokud provedete, pak hello je parametr volitelný pro uživatele hello hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-121">If you do, then hello parameter is optional for hello user of hello web service.</span></span> <span data-ttu-id="36c41-122">Pokud nechcete zadat výchozí hodnotu, pak uživatel hello je požadované tooenter hodnotu, při přístupu k hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-122">If you don't provide a default value, then hello user is required tooenter a value when hello web service is accessed.</span></span>

<span data-ttu-id="36c41-123">Hello dokumentaci k rozhraní API pro webovou službu hello obsahuje informace pro uživatele hello webové služby na tom, jak toospecify hello parametr webové služby prostřednictvím kódu programu při přístupu k hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-123">hello API documentation for hello web service includes information for hello web service user on how toospecify hello Web Service Parameter programmatically when accessing hello web service.</span></span>

> [!NOTE]
> <span data-ttu-id="36c41-124">Hello dokumentaci k rozhraní API pro classic webové služby je poskytována prostřednictvím hello **stránku nápovědy rozhraní API** odkaz v hello webové služby **řídicí panel** v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="36c41-124">hello API documentation for a classic web service is provided through hello **API help page** link in hello web service **DASHBOARD** in Machine Learning Studio.</span></span> <span data-ttu-id="36c41-125">Hello dokumentaci k rozhraní API pro novou webovou službu je poskytována prostřednictvím hello [webové služby Azure Machine Learning](https://services.azureml.net/Quickstart) portál hello **spotřebě** a **rozhraní API Swaggeru** stránky pro vaše webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-125">hello API documentation for a new web service is provided through hello [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) portal on hello **Consume** and **Swagger API** pages for your web service.</span></span>
> 
> 

## <a name="example"></a><span data-ttu-id="36c41-126">Příklad</span><span class="sxs-lookup"><span data-stu-id="36c41-126">Example</span></span>
<span data-ttu-id="36c41-127">Jako příklad předpokládejme máme experimentu s [Export dat] [ writer] modul, který odešle úložiště objektů blob tooAzure informace.</span><span class="sxs-lookup"><span data-stu-id="36c41-127">As an example, let's assume we have an experiment with an [Export Data][writer] module that sends information tooAzure blob storage.</span></span> <span data-ttu-id="36c41-128">Budeme definovat parametr webové služby s názvem "Blob cestu", který umožňuje hello webové služby uživatele toochange hello cesta toohello úložiště objektů blob při přístupu služby hello.</span><span class="sxs-lookup"><span data-stu-id="36c41-128">We'll define a Web Service Parameter named "Blob path" that allows hello web service user toochange hello path toohello blob storage when hello service is accessed.</span></span>

1. <span data-ttu-id="36c41-129">V nástroji Machine Learning Studio, klikněte na tlačítko hello [Export dat] [ writer] modulu tooselect ho.</span><span class="sxs-lookup"><span data-stu-id="36c41-129">In Machine Learning Studio, click hello [Export Data][writer] module tooselect it.</span></span> <span data-ttu-id="36c41-130">Jeho vlastnosti jsou uvedené v hello vlastnosti podokně toohello napravo od plátna experimentu hello.</span><span class="sxs-lookup"><span data-stu-id="36c41-130">Its properties are shown in hello Properties pane toohello right of hello experiment canvas.</span></span>
2. <span data-ttu-id="36c41-131">Zadejte typ úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="36c41-131">Specify hello storage type:</span></span>
   
   * <span data-ttu-id="36c41-132">V části **zadejte cíl dat**, vyberte "Azure Blob Storage".</span><span class="sxs-lookup"><span data-stu-id="36c41-132">Under **Please specify data destination**, select "Azure Blob Storage".</span></span>
   * <span data-ttu-id="36c41-133">V části **zadejte typ ověřování**, vyberte "Účet".</span><span class="sxs-lookup"><span data-stu-id="36c41-133">Under **Please specify authentication type**, select "Account".</span></span>
   * <span data-ttu-id="36c41-134">Zadejte informace o účtu hello pro hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="36c41-134">Enter hello account information for hello Azure blob storage.</span></span> 
     <p /><span data-ttu-id="36c41-135">
3.Klikněte na ikonu toohello hello napravo od hello **cesta tooblob počínaje kontejneru parametr**.</span><span class="sxs-lookup"><span data-stu-id="36c41-135">
3. Click hello icon toohello right of hello **Path tooblob beginning with container parameter**.</span></span> <span data-ttu-id="36c41-136">Vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="36c41-136">It looks like this:</span></span>
   
   ![Ikona webové služby parametr][icon]
   
   <span data-ttu-id="36c41-138">Vyberte "Nastavit jako parametr webové služby".</span><span class="sxs-lookup"><span data-stu-id="36c41-138">Select "Set as web service parameter".</span></span>
   
   <span data-ttu-id="36c41-139">Položka se přidají pod **parametry webové služby** dole hello v podokně Vlastnosti hello s hello název "cesta tooblob začíná kontejneru".</span><span class="sxs-lookup"><span data-stu-id="36c41-139">An entry is added under **Web Service Parameters** at hello bottom of hello Properties pane with hello name "Path tooblob beginning with container".</span></span> <span data-ttu-id="36c41-140">Toto je hello parametr webové služby, který je teď přidružený k tomuto [Export dat] [ writer] parametr modulu.</span><span class="sxs-lookup"><span data-stu-id="36c41-140">This is hello Web Service Parameter that is now associated with this [Export Data][writer] module parameter.</span></span>
4. <span data-ttu-id="36c41-141">toorename hello parametr webové služby, klikněte na název hello, zadejte "Blob cestu" a stiskněte klávesu hello **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="36c41-141">toorename hello Web Service Parameter, click hello name, enter "Blob path", and press hello **Enter** key.</span></span> 
5. <span data-ttu-id="36c41-142">tooprovide výchozí hodnota pro hello parametr webové služby, klikněte na ikonu toohello hello napravo od hello název, vyberte "Zadejte výchozí hodnotu", zadejte hodnotu (například "container1/output1.csv") a stiskněte klávesu hello **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="36c41-142">tooprovide a default value for hello Web Service Parameter, click hello icon toohello right of hello name, select "Provide default value", enter a value (for example, "container1/output1.csv"), and press hello **Enter** key.</span></span>
   
   ![Parametr webové služby][parameter]
6. <span data-ttu-id="36c41-144">Klikněte na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="36c41-144">Click **Run**.</span></span> 
7. <span data-ttu-id="36c41-145">Klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby** toodeploy hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-145">Click **Deploy Web Service** and select **Deploy Web Service [Classic]** or **Deploy Web Service [New]** toodeploy hello web service.</span></span>

> [!NOTE] 
> <span data-ttu-id="36c41-146">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-146">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="36c41-147">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="36c41-147">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="36c41-148">uživatel Hello hello webové služby teď můžete zadat do nového cíle pro hello [Export dat] [ writer] modulu při přístupu k hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="36c41-148">hello user of hello web service can now specify a new destination for hello [Export Data][writer] module when accessing hello web service.</span></span>

## <a name="more-information"></a><span data-ttu-id="36c41-149">Další informace</span><span class="sxs-lookup"><span data-stu-id="36c41-149">More information</span></span>
<span data-ttu-id="36c41-150">Více podrobný příklad najdete v tématu hello [parametry webové služby](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) položku v hello [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span><span class="sxs-lookup"><span data-stu-id="36c41-150">For a more detailed example, see hello [Web Service Parameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) entry in hello [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span></span>

<span data-ttu-id="36c41-151">Další informace o přístupu k webové službě Machine Learning najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="36c41-151">For more information on accessing a Machine Learning web service, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

