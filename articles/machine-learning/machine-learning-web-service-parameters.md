---
title: "Pomocí Azure Machine Learning parametry webové služby | Microsoft Docs"
description: "Jak používat parametry webové služby Azure Machine Learning Pokud chcete změnit chování modelu při přístupu k webové službě."
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
ms.openlocfilehash: 482726c1dae5385964e08b720e529817d5907537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a><span data-ttu-id="3bc3d-103">Použití parametrů webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3bc3d-103">Use Azure Machine Learning Web Service Parameters</span></span>
<span data-ttu-id="3bc3d-104">Webové služby Azure Machine Learning je vytvořen publikování experimentu, který obsahuje moduly, které se dají konfigurovat parametry.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-104">An Azure Machine Learning web service is created by publishing an experiment that contains modules with configurable parameters.</span></span> <span data-ttu-id="3bc3d-105">V některých případech můžete změnit chování modulu je spuštěn webovou službu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-105">In some cases, you may want to change the module behavior while the web service is running.</span></span> <span data-ttu-id="3bc3d-106">*Parametry webové služby* umožňují tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-106">*Web Service Parameters* allow you to do this task.</span></span> 

<span data-ttu-id="3bc3d-107">Běžným příkladem je nastavení [importovat Data] [ reader] modulu tak, aby uživatel publikované webové služby můžete zadat jiný zdroj dat, při přístupu k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-107">A common example is setting up the [Import Data][reader] module so that the user of the published web service can specify a different data source when the web service is accessed.</span></span> <span data-ttu-id="3bc3d-108">Nebo konfigurace [Export dat] [ writer] modulu tak, aby lze zadat jiný cíl.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-108">Or configuring the [Export Data][writer] module so that a different destination can be specified.</span></span> <span data-ttu-id="3bc3d-109">Mezi další příklady patří změna počtu bitů pro [Hashování] [ feature-hashing] modul, nebo počet požadovaných funkcí pro [na základě filtru výběr funkce] [ filter-based-feature-selection] modulu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-109">Some other examples include changing the number of bits for the [Feature Hashing][feature-hashing] module or the number of desired features for the [Filter-Based Feature Selection][filter-based-feature-selection] module.</span></span> 

<span data-ttu-id="3bc3d-110">Můžete nastavit parametry webové služby a je přidružit jeden nebo více parametrů modulu v experimentu a můžete určit, jestli jsou požadované nebo volitelné.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-110">You can set Web Service Parameters and associate them with one or more module parameters in your experiment, and you can specify whether they are required or optional.</span></span> <span data-ttu-id="3bc3d-111">Uživatel webové služby můžete poté zadejte hodnoty pro tyto parametry při volání webové služby.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-111">The user of the web service can then provide values for these parameters when they call the web service.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-to-set-and-use-web-service-parameters"></a><span data-ttu-id="3bc3d-112">Jak nastavit a používat parametry webové služby</span><span class="sxs-lookup"><span data-stu-id="3bc3d-112">How to set and use Web Service Parameters</span></span>
<span data-ttu-id="3bc3d-113">Kliknutím na ikonu vedle parametru pro modul a výběrem "Nastavit jako parametr webové služby" definujete parametr webové služby.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-113">You define a Web Service Parameter by clicking the icon next to the parameter for a module and selecting "Set as web service parameter".</span></span> <span data-ttu-id="3bc3d-114">Tím se vytvoří nový parametr webové služby a připojí k této parametr modulu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-114">This creates a new Web Service Parameter and connects it to that module parameter.</span></span> <span data-ttu-id="3bc3d-115">Potom při přístupu k webové službě, může uživatel zadat hodnotu pro parametr webové služby a použije se parametr modulu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-115">Then, when the web service is accessed, the user can specify a value for the Web Service Parameter and it is applied to the module parameter.</span></span>

<span data-ttu-id="3bc3d-116">Jakmile definujete parametr webové služby, je k dispozici pro všechny ostatní parametry modulu v experimentu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-116">Once you define a Web Service Parameter, it's available to any other module parameter in the experiment.</span></span> <span data-ttu-id="3bc3d-117">Pokud definujete webové služby parametr spojený s parametrem pro jeden modul, můžete použít tento stejný parametr webové služby pro ostatní moduly, tak dlouho, dokud parametr očekává hodnoty stejného typu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-117">If you define a Web Service Parameter associated with a parameter for one module, you can use that same Web Service Parameter for any other module, as long as the parameter expects the same type of value.</span></span> <span data-ttu-id="3bc3d-118">Například pokud parametr webové služby je číselná hodnota, pak lze být použít pouze pro parametry modulu, které očekávají číselná hodnota.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-118">For example, if the Web Service Parameter is a numeric value, then it can only be used for module parameters that expect a numeric value.</span></span> <span data-ttu-id="3bc3d-119">Pokud uživatel nastaví hodnotu pro parametr webové služby, se použijí všechny parametry přidružené modulu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-119">When the user sets a value for the Web Service Parameter, it will be applied to all associated module parameters.</span></span>

<span data-ttu-id="3bc3d-120">Můžete se rozhodnout, zda chcete zadat výchozí hodnotu pro parametr webové služby.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-120">You can decide whether to provide a default value for the Web Service Parameter.</span></span> <span data-ttu-id="3bc3d-121">V takovém případě je parametr volitelný pro uživatele webové služby.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-121">If you do, then the parameter is optional for the user of the web service.</span></span> <span data-ttu-id="3bc3d-122">Pokud nechcete zadat výchozí hodnotu, uživatel je potřeba zadat hodnotu, při přístupu k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-122">If you don't provide a default value, then the user is required to enter a value when the web service is accessed.</span></span>

<span data-ttu-id="3bc3d-123">Dokumentaci rozhraní API pro webovou službu obsahuje informace o uživateli služby webu na tom, jak zadat parametr webové služby prostřednictvím kódu programu při přístupu k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-123">The API documentation for the web service includes information for the web service user on how to specify the Web Service Parameter programmatically when accessing the web service.</span></span>

> [!NOTE]
> <span data-ttu-id="3bc3d-124">Dokumentaci rozhraní API pro classic webové služby je zajišťováno prostřednictvím **stránku nápovědy rozhraní API** odkaz ve webové službě **řídicí panel** v nástroji Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-124">The API documentation for a classic web service is provided through the **API help page** link in the web service **DASHBOARD** in Machine Learning Studio.</span></span> <span data-ttu-id="3bc3d-125">Dokumentaci k rozhraní API pro novou webovou službu je zajišťováno prostřednictvím [webové služby Azure Machine Learning](https://services.azureml.net/Quickstart) portál **spotřebě** a **rozhraní API Swaggeru** stránky pro webovou službu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-125">The API documentation for a new web service is provided through the [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) portal on the **Consume** and **Swagger API** pages for your web service.</span></span>
> 
> 

## <a name="example"></a><span data-ttu-id="3bc3d-126">Příklad</span><span class="sxs-lookup"><span data-stu-id="3bc3d-126">Example</span></span>
<span data-ttu-id="3bc3d-127">Jako příklad předpokládejme máme experimentu s [Export dat] [ writer] modul, který odesílá informace do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-127">As an example, let's assume we have an experiment with an [Export Data][writer] module that sends information to Azure blob storage.</span></span> <span data-ttu-id="3bc3d-128">Budeme definovat parametr webové služby s názvem "Blob cestu", který umožňuje uživateli webové služby změnit cestu k úložišti objektů blob při přístupu k službě.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-128">We'll define a Web Service Parameter named "Blob path" that allows the web service user to change the path to the blob storage when the service is accessed.</span></span>

1. <span data-ttu-id="3bc3d-129">V nástroji Machine Learning Studio, klikněte na tlačítko [Export dat] [ writer] modul a vyberte ji.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-129">In Machine Learning Studio, click the [Export Data][writer] module to select it.</span></span> <span data-ttu-id="3bc3d-130">Jeho vlastnosti jsou uvedené v podokně vlastností napravo od plátna experimentu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-130">Its properties are shown in the Properties pane to the right of the experiment canvas.</span></span>
2. <span data-ttu-id="3bc3d-131">Zadejte typ úložiště:</span><span class="sxs-lookup"><span data-stu-id="3bc3d-131">Specify the storage type:</span></span>
   
   * <span data-ttu-id="3bc3d-132">V části **zadejte cíl dat**, vyberte "Azure Blob Storage".</span><span class="sxs-lookup"><span data-stu-id="3bc3d-132">Under **Please specify data destination**, select "Azure Blob Storage".</span></span>
   * <span data-ttu-id="3bc3d-133">V části **zadejte typ ověřování**, vyberte "Účet".</span><span class="sxs-lookup"><span data-stu-id="3bc3d-133">Under **Please specify authentication type**, select "Account".</span></span>
   * <span data-ttu-id="3bc3d-134">Zadejte informace o účtu pro úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-134">Enter the account information for the Azure blob storage.</span></span> 
     <p /><span data-ttu-id="3bc3d-135">
3.Klikněte na ikonu napravo **cesta k objektu blob počínaje kontejneru parametr**.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-135">
3. Click the icon to the right of the **Path to blob beginning with container parameter**.</span></span> <span data-ttu-id="3bc3d-136">Vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="3bc3d-136">It looks like this:</span></span>
   
   ![Ikona webové služby parametr][icon]
   
   <span data-ttu-id="3bc3d-138">Vyberte "Nastavit jako parametr webové služby".</span><span class="sxs-lookup"><span data-stu-id="3bc3d-138">Select "Set as web service parameter".</span></span>
   
   <span data-ttu-id="3bc3d-139">Položka se přidají pod **parametry webové služby** dole v podokně vlastností s názvem "Cesta k objektu blob počínaje kontejneru".</span><span class="sxs-lookup"><span data-stu-id="3bc3d-139">An entry is added under **Web Service Parameters** at the bottom of the Properties pane with the name "Path to blob beginning with container".</span></span> <span data-ttu-id="3bc3d-140">Toto je parametr webové služby, který je teď přidružený k tomuto [Export dat] [ writer] parametr modulu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-140">This is the Web Service Parameter that is now associated with this [Export Data][writer] module parameter.</span></span>
4. <span data-ttu-id="3bc3d-141">Chcete-li přejmenovat parametr webové služby, klikněte na název, zadejte "Blob cestu" a stiskněte klávesu **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-141">To rename the Web Service Parameter, click the name, enter "Blob path", and press the **Enter** key.</span></span> 
5. <span data-ttu-id="3bc3d-142">Zadejte výchozí hodnotu pro parametr webové služby, klikněte na ikonu vpravo od názvu, vyberte možnost "Zadejte výchozí hodnotu", zadejte hodnotu (například "container1/output1.csv") a stiskněte klávesu **Enter** klíč.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-142">To provide a default value for the Web Service Parameter, click the icon to the right of the name, select "Provide default value", enter a value (for example, "container1/output1.csv"), and press the **Enter** key.</span></span>
   
   ![Parametr webové služby][parameter]
6. <span data-ttu-id="3bc3d-144">Klikněte na **Run** (Spustit).</span><span class="sxs-lookup"><span data-stu-id="3bc3d-144">Click **Run**.</span></span> 
7. <span data-ttu-id="3bc3d-145">Klikněte na tlačítko **nasazení webové služby** a vyberte **nasazení webové služby [Classic]** nebo **nasazení [nové] webové služby** nasadit webovou službu.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-145">Click **Deploy Web Service** and select **Deploy Web Service [Classic]** or **Deploy Web Service [New]** to deploy the web service.</span></span>

> [!NOTE] 
> <span data-ttu-id="3bc3d-146">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které, můžete nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-146">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="3bc3d-147">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="3bc3d-147">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="3bc3d-148">Uživatel webovou službu nyní můžete určit na nové umístění pro [Export dat] [ writer] modulu při přístupu k webové službě.</span><span class="sxs-lookup"><span data-stu-id="3bc3d-148">The user of the web service can now specify a new destination for the [Export Data][writer] module when accessing the web service.</span></span>

## <a name="more-information"></a><span data-ttu-id="3bc3d-149">Další informace</span><span class="sxs-lookup"><span data-stu-id="3bc3d-149">More information</span></span>
<span data-ttu-id="3bc3d-150">Podrobnější příklad najdete v tématu [parametry webové služby](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) položku [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span><span class="sxs-lookup"><span data-stu-id="3bc3d-150">For a more detailed example, see the [Web Service Parameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) entry in the [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span></span>

<span data-ttu-id="3bc3d-151">Další informace o přístupu k webové službě Machine Learning najdete v tématu [využívání Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="3bc3d-151">For more information on accessing a Machine Learning web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

