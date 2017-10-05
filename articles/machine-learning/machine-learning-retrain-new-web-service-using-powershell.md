---
title: "Přeučování nové Azure Machine Learning webové služby pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak programově přeučit modelu a aktualizovat webovou službu, která používá nově trénovaného modelu v Azure Machine Learning pomocí rutin prostředí PowerShell správu Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 804dd59e62f38ee1878045d93211ee18e0d5bfce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a><span data-ttu-id="0702d-103">Přeučování nového správce prostředků na základě webové službě pomocí rutin prostředí PowerShell správu Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0702d-103">Retrain a New Resource Manager based web service using the Machine Learning Management PowerShell cmdlets</span></span>
<span data-ttu-id="0702d-104">Pokud jste přeučování novou webovou službu, je aktualizovat definice prediktivní webové služby, chcete-li nový trained model.</span><span class="sxs-lookup"><span data-stu-id="0702d-104">When you retrain a New web service, you update the predictive web service definition to reference the new trained model.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="0702d-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0702d-105">Prerequisites</span></span>
<span data-ttu-id="0702d-106">Musíte vytvořit výukový experiment a prediktivní experiment, jak je znázorněno v [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="0702d-106">You must set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="0702d-107">Prediktivní experiment musí být nasazený jako počítač Azure Resource Manager (Nový) na základě učení webové služby.</span><span class="sxs-lookup"><span data-stu-id="0702d-107">The predictive experiment must be deployed as an Azure Resource Manager (New) based machine learning web service.</span></span> <span data-ttu-id="0702d-108">K nasazení nové webové služby musí mít dostatečná oprávnění v rámci předplatného, do které, můžete nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="0702d-108">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="0702d-109">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="0702d-109">For more information, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="0702d-110">Další informace o nasazení webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="0702d-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="0702d-111">Tento proces vyžaduje, že jste nainstalovali rutiny Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="0702d-111">This process requires that you have installed the Azure Machine Learning Cmdlets.</span></span> <span data-ttu-id="0702d-112">Informace o instalaci rutin Machine Learning, najdete v článku [rutiny Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="0702d-112">For information installing the Machine Learning cmdlets, see the [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN.</span></span>

<span data-ttu-id="0702d-113">Kopírovat z retraining výstupu následující informace:</span><span class="sxs-lookup"><span data-stu-id="0702d-113">Copied the following information from the retraining output:</span></span>

* <span data-ttu-id="0702d-114">BaseLocation</span><span class="sxs-lookup"><span data-stu-id="0702d-114">BaseLocation</span></span>
* <span data-ttu-id="0702d-115">RelativeLocation</span><span class="sxs-lookup"><span data-stu-id="0702d-115">RelativeLocation</span></span>

<span data-ttu-id="0702d-116">Postup, kterým jsou:</span><span class="sxs-lookup"><span data-stu-id="0702d-116">The steps you take are:</span></span>

1. <span data-ttu-id="0702d-117">Přihlaste se ke svému účtu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="0702d-117">Sign in to your Azure Resource Manager account.</span></span>
2. <span data-ttu-id="0702d-118">Získat definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-118">Get the web service definition</span></span>
3. <span data-ttu-id="0702d-119">Exportovat jako JSON definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-119">Export the Web Service Definition as JSON</span></span>
4. <span data-ttu-id="0702d-120">Aktualizujte odkaz na objekt blob ilearner v kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="0702d-120">Update the reference to the ilearner blob in the JSON.</span></span>
5. <span data-ttu-id="0702d-121">Import kódu JSON do definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-121">Import the JSON into a Web Service Definition</span></span>
6. <span data-ttu-id="0702d-122">Aktualizovat webovou službu pomocí nové definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-122">Update the web service with new Web Service Definition</span></span>

## <a name="sign-in-to-your-azure-resource-manager-account"></a><span data-ttu-id="0702d-123">Přihlaste se k účtu Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0702d-123">Sign in to your Azure Resource Manager account</span></span>
<span data-ttu-id="0702d-124">Musíte se nejdřív přihlásit k účtu Azure z v prostředí PowerShell pomocí [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0702d-124">You must first sign in to your Azure account from within the PowerShell environment using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-the-web-service-definition"></a><span data-ttu-id="0702d-125">Získat definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-125">Get the Web Service Definition</span></span>
<span data-ttu-id="0702d-126">V dalším kroku získat voláním webové služby [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="0702d-126">Next, get the Web Service by calling the [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="0702d-127">Definice webové služby je interní reprezentací pro cvičný model webové služby a není přímo změn.</span><span class="sxs-lookup"><span data-stu-id="0702d-127">The Web Service Definition is an internal representation of the trained model of the web service and is not directly modifiable.</span></span> <span data-ttu-id="0702d-128">Ujistěte se, že jsou načítání definice webové služby prediktivní experiment a není experimentu školení.</span><span class="sxs-lookup"><span data-stu-id="0702d-128">Make sure that you are retrieving the Web Service Definition for your Predictive experiment and not your training experiment.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="0702d-129">Chcete-li zjistit název skupiny prostředků existující webovou službu, spusťte rutinu Get-AzureRmMlWebService bez parametrů pro zobrazení webové služby v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="0702d-129">To determine the resource group name of an existing web service, run the Get-AzureRmMlWebService cmdlet without any parameters to display the web services in your subscription.</span></span> <span data-ttu-id="0702d-130">Vyhledejte webovou službu a podívejte se na jeho identifikátorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="0702d-130">Locate the web service, and then look at its web service ID.</span></span> <span data-ttu-id="0702d-131">Název skupiny prostředků je čtvrtý element v ID, bezprostředně za *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="0702d-131">The name of the resource group is the fourth element in the ID, just after the *resourceGroups* element.</span></span> <span data-ttu-id="0702d-132">V následujícím příkladu je název skupiny prostředků výchozí-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="0702d-132">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="0702d-133">Můžete taky určit název skupiny prostředků existující webové služby, přihlaste se k portálu Microsoft Azure Machine Learning webové služby.</span><span class="sxs-lookup"><span data-stu-id="0702d-133">Alternatively, to determine the resource group name of an existing web service, log on to the Microsoft Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="0702d-134">Vyberte webovou službu.</span><span class="sxs-lookup"><span data-stu-id="0702d-134">Select the web service.</span></span> <span data-ttu-id="0702d-135">Název skupiny prostředků je pátý element adresy URL webové služby, bezprostředně za *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="0702d-135">The resource group name is the fifth element of the URL of the web service, just after the *resourceGroups* element.</span></span> <span data-ttu-id="0702d-136">V následujícím příkladu je název skupiny prostředků výchozí-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="0702d-136">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a><span data-ttu-id="0702d-137">Exportovat jako JSON definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-137">Export the Web Service Definition as JSON</span></span>
<span data-ttu-id="0702d-138">Upravit definici, abyste pro cvičný model používat nově Trained Model, musíte nejprve použít [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) rutiny exportovat do souboru ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="0702d-138">To modify the definition to the trained model to use the newly Trained Model, you must first use the [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet to export it to a JSON format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a><span data-ttu-id="0702d-139">Aktualizujte odkaz na objekt blob ilearner v kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="0702d-139">Update the reference to the ilearner blob in the JSON.</span></span>
<span data-ttu-id="0702d-140">V prostředky, vyhledejte [trained model], aktualizujte *identifikátor uri* hodnotu *locationInfo* uzlu s identifikátorem URI objektu ilearner blob.</span><span class="sxs-lookup"><span data-stu-id="0702d-140">In the assets, locate the [trained model], update the *uri* value in the *locationInfo* node with the URI of the ilearner blob.</span></span> <span data-ttu-id="0702d-141">Identifikátor URI je generován kombinování *BaseLocation* a *RelativeLocation* z výstupu BES retraining volání.</span><span class="sxs-lookup"><span data-stu-id="0702d-141">The URI is generated by combining the *BaseLocation* and the *RelativeLocation* from the output of the BES retraining call.</span></span> <span data-ttu-id="0702d-142">Tím se aktualizuje cesta odkazovat na nové naučeného modelu.</span><span class="sxs-lookup"><span data-stu-id="0702d-142">This updates the path to reference the new trained model.</span></span>

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a><span data-ttu-id="0702d-143">Import kódu JSON do definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-143">Import the JSON into a Web Service Definition</span></span>
<span data-ttu-id="0702d-144">Je nutné použít [Import AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) rutiny převést změněný soubor JSON zpět do definice webové služby, který můžete použít k aktualizaci definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="0702d-144">You must use the [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet to convert the modified JSON file back into a Web Service Definition that you can use to update the Web Service Definition.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a><span data-ttu-id="0702d-145">Aktualizovat webovou službu pomocí nové definice webové služby</span><span class="sxs-lookup"><span data-stu-id="0702d-145">Update the web service with new Web Service Definition</span></span>
<span data-ttu-id="0702d-146">Nakonec použijte [aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) rutiny aktualizovat definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="0702d-146">Finally, you use [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet to update the Web Service Definition.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a><span data-ttu-id="0702d-147">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0702d-147">Summary</span></span>
<span data-ttu-id="0702d-148">Pomocí rutin prostředí PowerShell Machine Learning management, můžete aktualizovat pro cvičný model prediktivní povolení scénáře, jako webové služby:</span><span class="sxs-lookup"><span data-stu-id="0702d-148">Using the Machine Learning PowerShell management cmdlets, you can update the trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="0702d-149">Pravidelné model retraining s nová data.</span><span class="sxs-lookup"><span data-stu-id="0702d-149">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="0702d-150">Distribuce modelu pro zákazníky s cílem aby přeučování modelu s použitím svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="0702d-150">Distribution of a model to customers with the goal of letting them retrain the model using their own data.</span></span>

