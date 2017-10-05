---
title: "Azure Machine Learning webové služby: Nasazení a používání | Microsoft Docs"
description: "Prostředky pro nasazení a využívají webové služby."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 18edabe267ec06c08074d7a7a6d71435cedc8489
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a><span data-ttu-id="0c1a3-103">Webové služby Azure Machine Learning: Nasazení a využití</span><span class="sxs-lookup"><span data-stu-id="0c1a3-103">Azure Machine Learning Web Services: Deployment and consumption</span></span>
<span data-ttu-id="0c1a3-104">Azure Machine Learning můžete použít k nasazení pracovní postupy a modely jako webové služby machine learning.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-104">You can use Azure Machine Learning to deploy machine-learning workflows and models as web services.</span></span> <span data-ttu-id="0c1a3-105">Tyto webové služby pak lze volat modely machine learningu z aplikací přes Internet udělat předpovědi v reálném čase nebo v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-105">These web services can then be used to call the machine-learning models from applications over the Internet to do predictions in real time or in batch mode.</span></span> <span data-ttu-id="0c1a3-106">Protože webových služeb jsou dosáhl standardu RESTful, můžete je volat z různé programovací jazyky a platformy, jako je například rozhraní .NET a Javu a z aplikace, jako je například aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-106">Because the web services are RESTful, you can call them from various programming languages and platforms, such as .NET and Java, and from applications, such as Excel.</span></span>

<span data-ttu-id="0c1a3-107">Další části obsahují odkazy na postupy, kód a dokumentaci, která vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-107">The next sections provide links to walkthroughs, code, and documentation to help get you started.</span></span>

## <a name="deploy-a-web-service"></a><span data-ttu-id="0c1a3-108">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="0c1a3-108">Deploy a web service</span></span>
### <a name="with-azure-machine-learning-studio"></a><span data-ttu-id="0c1a3-109">S Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="0c1a3-109">With Azure Machine Learning Studio</span></span>
<span data-ttu-id="0c1a3-110">Machine Learning Studio a portálu Microsoft Azure Machine Learning webové služby můžete nasazovat a spravovat bez nutnosti psaní kódu webové služby.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-110">Machine Learning Studio and the Microsoft Azure Machine Learning Web Services portal help you deploy and manage a web service without writing code.</span></span>

<span data-ttu-id="0c1a3-111">Následující odkazy obsahují obecné informace o tom, jak nasadit novou webovou službu:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-111">The following links provide general Information about how to deploy a new web service:</span></span>

* <span data-ttu-id="0c1a3-112">Přehled o tom, jak nasadit novou webovou službu, která je založená na Azure Resource Manager, najdete v tématu [nasadit novou webovou službu](machine-learning-webservice-deploy-a-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-112">For an overview about how to deploy a new web service that's based on Azure Resource Manager, see [Deploy a new web service](machine-learning-webservice-deploy-a-web-service.md).</span></span>
* <span data-ttu-id="0c1a3-113">Návod, o tom, jak nasadit webovou službu, naleznete v [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-113">For a walkthrough about how to deploy a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="0c1a3-114">Úplné návod o tom, jak vytvořit a nasadit webové služby najdete v tématu [návod krok 1: vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-114">For a full walkthrough about how to create and deploy a web service, see [Walkthrough Step 1: Create a Machine Learning workspace](machine-learning-walkthrough-1-create-ml-workspace.md).</span></span>
* <span data-ttu-id="0c1a3-115">Konkrétní příklady, které nasazení webové služby najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-115">For specific examples that deploy a web service, see:</span></span>

  * [<span data-ttu-id="0c1a3-116">Návod krok 5: Nasazení webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0c1a3-116">Walkthrough Step 5: Deploy the Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
  * [<span data-ttu-id="0c1a3-117">Postup nasazení webové služby do několika oblastí</span><span class="sxs-lookup"><span data-stu-id="0c1a3-117">How to deploy a web service to multiple regions</span></span>](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a><span data-ttu-id="0c1a3-118">Pomocí zprostředkovatele prostředků služby webové rozhraní API (rozhraní API Správce Azure Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="0c1a3-118">With web services resource provider APIs (Azure Resource Manager APIs)</span></span>
<span data-ttu-id="0c1a3-119">Zprostředkovatel prostředků Azure Machine Learning pro webové služby umožňuje nasazení a správy webové služby pomocí volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-119">The Azure Machine Learning resource provider for web services enables deployment and management of web services by using REST API calls.</span></span> <span data-ttu-id="0c1a3-120">Další podrobnosti najdete v tématu [Machine Learning webové služby (REST)](/rest/api/machinelearning/index) odkaz.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-120">For additional details, see the [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) reference.</span></span>

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a><span data-ttu-id="0c1a3-121">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c1a3-121">With PowerShell cmdlets</span></span>
<span data-ttu-id="0c1a3-122">Zprostředkovatel prostředků Azure Machine Learning pro webové služby umožňuje nasazení a správy webové služby pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-122">Azure Machine Learning resource provider for web services enables deployment and management of web services by using PowerShell cmdlets.</span></span>

<span data-ttu-id="0c1a3-123">Pokud chcete používat rutiny, musí prvním přihlášení k účtu Azure z prostředí PowerShell pomocí [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-123">To use the cmdlets, you must first sign in to your Azure account from within the PowerShell environment by using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="0c1a3-124">Pokud jste obeznámeni s jak volat příkazy prostředí PowerShell, které jsou založeny na najdete v části správce prostředků, [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-124">If you are unfamiliar with how to call PowerShell commands that are based on Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span></span>

<span data-ttu-id="0c1a3-125">Chcete-li exportovat prediktivní experiment, použijte [ukázkový kód](https://github.com/ritwik20/AzureML-WebServices).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-125">To export your predictive experiment, use [this sample code](https://github.com/ritwik20/AzureML-WebServices).</span></span> <span data-ttu-id="0c1a3-126">Jakmile vytvoříte soubor .exe z kódu, můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-126">After you create the .exe file from the code, you can type:</span></span>

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

<span data-ttu-id="0c1a3-127">Spuštění aplikace vytvoří šablonu JSON webové služby.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-127">Running the application creates a web service JSON template.</span></span> <span data-ttu-id="0c1a3-128">Abyste mohli použít šablonu nasazení webové služby, je nutné přidat následující informace:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-128">To use the template to deploy a web service, you must add the following information:</span></span>

* <span data-ttu-id="0c1a3-129">Název účtu úložiště a klíč</span><span class="sxs-lookup"><span data-stu-id="0c1a3-129">Storage account name and key</span></span>

    <span data-ttu-id="0c1a3-130">Název účtu úložiště a klíč můžete získat z buď [portál Azure](https://portal.azure.com/) nebo [portál Azure classic](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-130">You can get the storage account name and key from either the [Azure portal](https://portal.azure.com/) or the [Azure classic portal](http://manage.windowsazure.com/).</span></span>
* <span data-ttu-id="0c1a3-131">ID plánu závazků</span><span class="sxs-lookup"><span data-stu-id="0c1a3-131">Commitment plan ID</span></span>

    <span data-ttu-id="0c1a3-132">Můžete získat ID plánu z [webové služby Azure Machine Learning](https://services.azureml.net) portálu přihlášení a kliknutím na název plánu.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-132">You can get the plan ID from the [Azure Machine Learning Web Services](https://services.azureml.net) portal by signing in and clicking a plan name.</span></span>

<span data-ttu-id="0c1a3-133">Přidejte je do šablony JSON jako podřízené objekty *vlastnosti* uzel na stejné úrovni jako *MachineLearningWorkspace* uzlu.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-133">Add them to the JSON template as children of the *Properties* node at the same level as the *MachineLearningWorkspace* node.</span></span>

<span data-ttu-id="0c1a3-134">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-134">Here's an example:</span></span>

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

<span data-ttu-id="0c1a3-135">Naleznete v následujících článcích a ukázkový kód pro další podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-135">See the following articles and sample code for additional details:</span></span>

* <span data-ttu-id="0c1a3-136">[Rutiny Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="0c1a3-136">[Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN</span></span>
* <span data-ttu-id="0c1a3-137">Ukázka [návod](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) na Githubu</span><span class="sxs-lookup"><span data-stu-id="0c1a3-137">Sample [walkthrough](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) on GitHub</span></span>

## <a name="consume-the-web-services"></a><span data-ttu-id="0c1a3-138">Využívání webových služeb</span><span class="sxs-lookup"><span data-stu-id="0c1a3-138">Consume the web services</span></span>
### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a><span data-ttu-id="0c1a3-139">Z webové služby Azure Machine Learning uživatelského rozhraní (zkušební režim)</span><span class="sxs-lookup"><span data-stu-id="0c1a3-139">From the Azure Machine Learning Web Services UI (Testing)</span></span>
<span data-ttu-id="0c1a3-140">Můžete otestovat webovou službu na portálu Azure Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-140">You can test your web service from the Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="0c1a3-141">To zahrnuje testování požadavků a odpovědí služby (záznamy RR) a služba Batch Execution (BES) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-141">This includes testing the Request-Response service (RRS) and Batch Execution service (BES) interfaces.</span></span>

* [<span data-ttu-id="0c1a3-142">Nasazení nové webové služby</span><span class="sxs-lookup"><span data-stu-id="0c1a3-142">Deploy a new web service</span></span>](machine-learning-webservice-deploy-a-web-service.md)
* [<span data-ttu-id="0c1a3-143">Nasazení webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0c1a3-143">Deploy an Azure Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
* [<span data-ttu-id="0c1a3-144">Návod krok 5: Nasazení webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0c1a3-144">Walkthrough Step 5: Deploy the Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a><span data-ttu-id="0c1a3-145">Z aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="0c1a3-145">From Excel</span></span>
<span data-ttu-id="0c1a3-146">Si můžete stáhnout šablony aplikace Excel, který využívá webové služby:</span><span class="sxs-lookup"><span data-stu-id="0c1a3-146">You can download an Excel template that consumes the web service:</span></span>

* [<span data-ttu-id="0c1a3-147">Využívají webové služby Azure Machine Learning z Excelu</span><span class="sxs-lookup"><span data-stu-id="0c1a3-147">Consuming an Azure Machine Learning web service from Excel</span></span>](machine-learning-consuming-from-excel.md)
* [<span data-ttu-id="0c1a3-148">Add-in pro webové služby Azure Machine Learning v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="0c1a3-148">Excel add-in for Azure Machine Learning Web Services</span></span>](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a><span data-ttu-id="0c1a3-149">Z klienta na základě REST</span><span class="sxs-lookup"><span data-stu-id="0c1a3-149">From a REST-based client</span></span>
<span data-ttu-id="0c1a3-150">Webové služby sady Azure Machine Learning jsou rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-150">Azure Machine Learning Web Services are RESTful APIs.</span></span> <span data-ttu-id="0c1a3-151">Můžete využívat tato rozhraní API z různých platformách, jako je například .NET, Python, R, Java, atd. **Spotřebě** stránky pro webovou službu na [portálu Microsoft Azure Machine Learning webové služby](https://services.azureml.net) obsahuje ukázkový kód, který můžete začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="0c1a3-151">You can consume these APIs from various platforms, such as .NET, Python, R, Java, etc. The **Consume** page for your web service on the [Microsoft Azure Machine Learning Web Services portal](https://services.azureml.net) has sample code that can help you get started.</span></span> <span data-ttu-id="0c1a3-152">Další informace najdete v tématu o [využívání webové služby Azure Machine Learning](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="0c1a3-152">For more information, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>
