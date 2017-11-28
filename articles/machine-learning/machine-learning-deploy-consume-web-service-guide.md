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
ms.openlocfilehash: 539c2abb053a0f981be0374defe45cf4d96b740b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a><span data-ttu-id="77922-103">Webové služby Azure Machine Learning: Nasazení a využití</span><span class="sxs-lookup"><span data-stu-id="77922-103">Azure Machine Learning Web Services: Deployment and consumption</span></span>
<span data-ttu-id="77922-104">Můžete použít Azure Machine Learning pracovních toodeploy strojové učení a modely jako webové služby.</span><span class="sxs-lookup"><span data-stu-id="77922-104">You can use Azure Machine Learning toodeploy machine-learning workflows and models as web services.</span></span> <span data-ttu-id="77922-105">Tyto webové služby pak lze použít toocall modely machine learningu hello z aplikací přes hello Internet toodo předpovědi v reálném čase nebo v dávkovém režimu.</span><span class="sxs-lookup"><span data-stu-id="77922-105">These web services can then be used toocall hello machine-learning models from applications over hello Internet toodo predictions in real time or in batch mode.</span></span> <span data-ttu-id="77922-106">Protože hello webových služeb jsou dosáhl standardu RESTful, můžete je volat z různé programovací jazyky a platformy, jako je například rozhraní .NET a Javu a z aplikace, jako je například aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="77922-106">Because hello web services are RESTful, you can call them from various programming languages and platforms, such as .NET and Java, and from applications, such as Excel.</span></span>

<span data-ttu-id="77922-107">Další části Hello poskytují toowalkthroughs odkazy, kód a dokumentace toohelp vám pomůžou začít.</span><span class="sxs-lookup"><span data-stu-id="77922-107">hello next sections provide links toowalkthroughs, code, and documentation toohelp get you started.</span></span>

## <a name="deploy-a-web-service"></a><span data-ttu-id="77922-108">Nasazení webové služby</span><span class="sxs-lookup"><span data-stu-id="77922-108">Deploy a web service</span></span>
### <a name="with-azure-machine-learning-studio"></a><span data-ttu-id="77922-109">S Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="77922-109">With Azure Machine Learning Studio</span></span>
<span data-ttu-id="77922-110">Machine Learning Studio a hello portálu Microsoft Azure Machine Learning webové služby můžete nasazovat a spravovat bez nutnosti psaní kódu webové služby.</span><span class="sxs-lookup"><span data-stu-id="77922-110">Machine Learning Studio and hello Microsoft Azure Machine Learning Web Services portal help you deploy and manage a web service without writing code.</span></span>

<span data-ttu-id="77922-111">Hello následující odkazy obsahují obecné informace o tom toodeploy novou webovou službu:</span><span class="sxs-lookup"><span data-stu-id="77922-111">hello following links provide general Information about how toodeploy a new web service:</span></span>

* <span data-ttu-id="77922-112">Přehled o tom, jak toodeploy novou webovou službu, která je založená na Azure Resource Manager, najdete v části [nasadit novou webovou službu](machine-learning-webservice-deploy-a-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="77922-112">For an overview about how toodeploy a new web service that's based on Azure Resource Manager, see [Deploy a new web service](machine-learning-webservice-deploy-a-web-service.md).</span></span>
* <span data-ttu-id="77922-113">Návod, jak toodeploy webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="77922-113">For a walkthrough about how toodeploy a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="77922-114">Úplné návod, jak toocreate a nasazení webové služby, najdete v části [návod krok 1: vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="77922-114">For a full walkthrough about how toocreate and deploy a web service, see [Walkthrough Step 1: Create a Machine Learning workspace](machine-learning-walkthrough-1-create-ml-workspace.md).</span></span>
* <span data-ttu-id="77922-115">Konkrétní příklady, které nasazení webové služby najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="77922-115">For specific examples that deploy a web service, see:</span></span>

  * [<span data-ttu-id="77922-116">Návod krok 5: Nasazení webové služby Azure Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="77922-116">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
  * [<span data-ttu-id="77922-117">Jak toodeploy webové služby toomultiple oblastí</span><span class="sxs-lookup"><span data-stu-id="77922-117">How toodeploy a web service toomultiple regions</span></span>](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a><span data-ttu-id="77922-118">Pomocí zprostředkovatele prostředků služby webové rozhraní API (rozhraní API Správce Azure Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="77922-118">With web services resource provider APIs (Azure Resource Manager APIs)</span></span>
<span data-ttu-id="77922-119">Poskytovatel prostředků Azure Machine Learning Hello webové služby umožňuje nasazení a správy webové služby pomocí volání rozhraní REST API.</span><span class="sxs-lookup"><span data-stu-id="77922-119">hello Azure Machine Learning resource provider for web services enables deployment and management of web services by using REST API calls.</span></span> <span data-ttu-id="77922-120">Další podrobnosti najdete v tématu [Machine Learning webové služby (REST)](/rest/api/machinelearning/index) odkaz.</span><span class="sxs-lookup"><span data-stu-id="77922-120">For additional details, see the [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) reference.</span></span>

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a><span data-ttu-id="77922-121">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="77922-121">With PowerShell cmdlets</span></span>
<span data-ttu-id="77922-122">Zprostředkovatel prostředků Azure Machine Learning pro webové služby umožňuje nasazení a správy webové služby pomocí rutin prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="77922-122">Azure Machine Learning resource provider for web services enables deployment and management of web services by using PowerShell cmdlets.</span></span>

<span data-ttu-id="77922-123">rutiny hello toouse, je nutné se přihlásit v tooyour účet Azure z prostředí PowerShell hello pomocí hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="77922-123">toouse hello cmdlets, you must first sign in tooyour Azure account from within hello PowerShell environment by using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="77922-124">Pokud jste obeznámeni s jak toocall prostředí PowerShell příkazy, které jsou založené na správci prostředků najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span><span class="sxs-lookup"><span data-stu-id="77922-124">If you are unfamiliar with how toocall PowerShell commands that are based on Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span></span>

<span data-ttu-id="77922-125">tooexport vaše prediktivní experiment, použijte [ukázkový kód](https://github.com/ritwik20/AzureML-WebServices).</span><span class="sxs-lookup"><span data-stu-id="77922-125">tooexport your predictive experiment, use [this sample code](https://github.com/ritwik20/AzureML-WebServices).</span></span> <span data-ttu-id="77922-126">Po vytvoření souboru .exe hello z hello kódu, můžete zadat:</span><span class="sxs-lookup"><span data-stu-id="77922-126">After you create hello .exe file from hello code, you can type:</span></span>

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

<span data-ttu-id="77922-127">Spuštění aplikace hello vytvoří šablonu JSON webové služby.</span><span class="sxs-lookup"><span data-stu-id="77922-127">Running hello application creates a web service JSON template.</span></span> <span data-ttu-id="77922-128">toouse hello šablony toodeploy webové služby, musíte přidat hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="77922-128">toouse hello template toodeploy a web service, you must add hello following information:</span></span>

* <span data-ttu-id="77922-129">Název účtu úložiště a klíč</span><span class="sxs-lookup"><span data-stu-id="77922-129">Storage account name and key</span></span>

    <span data-ttu-id="77922-130">Hello název účtu úložiště a klíč můžete získat z buď hello [portál Azure](https://portal.azure.com/) nebo hello [portál Azure classic](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="77922-130">You can get hello storage account name and key from either hello [Azure portal](https://portal.azure.com/) or hello [Azure classic portal](http://manage.windowsazure.com/).</span></span>
* <span data-ttu-id="77922-131">ID plánu závazků</span><span class="sxs-lookup"><span data-stu-id="77922-131">Commitment plan ID</span></span>

    <span data-ttu-id="77922-132">Můžete získat ID plánu hello hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu přihlášení a kliknutím na název plánu.</span><span class="sxs-lookup"><span data-stu-id="77922-132">You can get hello plan ID from hello [Azure Machine Learning Web Services](https://services.azureml.net) portal by signing in and clicking a plan name.</span></span>

<span data-ttu-id="77922-133">Přidat šablonu JSON toohello jako podřízené objekty hello *vlastnosti* uzel v hello na stejné úrovni jako hello *MachineLearningWorkspace* uzlu.</span><span class="sxs-lookup"><span data-stu-id="77922-133">Add them toohello JSON template as children of hello *Properties* node at hello same level as hello *MachineLearningWorkspace* node.</span></span>

<span data-ttu-id="77922-134">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="77922-134">Here's an example:</span></span>

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

<span data-ttu-id="77922-135">Zobrazit hello následující články a ukázkový kód pro další podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="77922-135">See hello following articles and sample code for additional details:</span></span>

* <span data-ttu-id="77922-136">[Rutiny Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="77922-136">[Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN</span></span>
* <span data-ttu-id="77922-137">Ukázka [návod](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) na Githubu</span><span class="sxs-lookup"><span data-stu-id="77922-137">Sample [walkthrough](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) on GitHub</span></span>

## <a name="consume-hello-web-services"></a><span data-ttu-id="77922-138">Využívání webových služeb hello</span><span class="sxs-lookup"><span data-stu-id="77922-138">Consume hello web services</span></span>
### <a name="from-hello-azure-machine-learning-web-services-ui-testing"></a><span data-ttu-id="77922-139">Z hello Azure Machine Learning webové služby uživatelského rozhraní (testování)</span><span class="sxs-lookup"><span data-stu-id="77922-139">From hello Azure Machine Learning Web Services UI (Testing)</span></span>
<span data-ttu-id="77922-140">Webová služba z portálu hello webové služby Azure Machine Learning můžete otestovat.</span><span class="sxs-lookup"><span data-stu-id="77922-140">You can test your web service from hello Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="77922-141">To zahrnuje testování hello požadavků a odpovědí služby (záznamy RR) a služba Batch Execution (BES) rozhraní.</span><span class="sxs-lookup"><span data-stu-id="77922-141">This includes testing hello Request-Response service (RRS) and Batch Execution service (BES) interfaces.</span></span>

* [<span data-ttu-id="77922-142">Nasazení nové webové služby</span><span class="sxs-lookup"><span data-stu-id="77922-142">Deploy a new web service</span></span>](machine-learning-webservice-deploy-a-web-service.md)
* [<span data-ttu-id="77922-143">Nasazení webové služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="77922-143">Deploy an Azure Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
* [<span data-ttu-id="77922-144">Návod krok 5: Nasazení webové služby Azure Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="77922-144">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a><span data-ttu-id="77922-145">Z aplikace Excel</span><span class="sxs-lookup"><span data-stu-id="77922-145">From Excel</span></span>
<span data-ttu-id="77922-146">Si můžete stáhnout šablony aplikace Excel, který využívá hello webové služby:</span><span class="sxs-lookup"><span data-stu-id="77922-146">You can download an Excel template that consumes hello web service:</span></span>

* [<span data-ttu-id="77922-147">Využívají webové služby Azure Machine Learning z Excelu</span><span class="sxs-lookup"><span data-stu-id="77922-147">Consuming an Azure Machine Learning web service from Excel</span></span>](machine-learning-consuming-from-excel.md)
* [<span data-ttu-id="77922-148">Add-in pro webové služby Azure Machine Learning v aplikaci Excel</span><span class="sxs-lookup"><span data-stu-id="77922-148">Excel add-in for Azure Machine Learning Web Services</span></span>](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a><span data-ttu-id="77922-149">Z klienta na základě REST</span><span class="sxs-lookup"><span data-stu-id="77922-149">From a REST-based client</span></span>
<span data-ttu-id="77922-150">Webové služby sady Azure Machine Learning jsou rozhraní RESTful API.</span><span class="sxs-lookup"><span data-stu-id="77922-150">Azure Machine Learning Web Services are RESTful APIs.</span></span> <span data-ttu-id="77922-151">Můžete využívat tato rozhraní API z různých platformách, jako je například .NET, Python, R, Java, atd. hello **spotřebě** stránky pro webovou službu na hello [portálu Microsoft Azure Machine Learning webové služby](https://services.azureml.net) má ukázka kód, který vám pomůže začít pracovat.</span><span class="sxs-lookup"><span data-stu-id="77922-151">You can consume these APIs from various platforms, such as .NET, Python, R, Java, etc. hello **Consume** page for your web service on hello [Microsoft Azure Machine Learning Web Services portal](https://services.azureml.net) has sample code that can help you get started.</span></span> <span data-ttu-id="77922-152">Další informace najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="77922-152">For more information, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>
