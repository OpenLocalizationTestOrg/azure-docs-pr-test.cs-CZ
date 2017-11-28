---
title: "aaaRetrain nové Azure Machine Learning webové služby pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically přeučování modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning pomocí rutin prostředí PowerShell správu Machine Learning hello."
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
ms.openlocfilehash: 5b77fa82cfe17f0b4e90007ef81c506ab712475b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-hello-machine-learning-management-powershell-cmdlets"></a><span data-ttu-id="7c5b8-103">Přeučování nového správce prostředků na základě webové službě pomocí rutiny prostředí PowerShell správu Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="7c5b8-103">Retrain a New Resource Manager based web service using hello Machine Learning Management PowerShell cmdlets</span></span>
<span data-ttu-id="7c5b8-104">Když jste přeučování novou webovou službu, aktualizujete hello prediktivní webové služby definice tooreference hello nový trained model.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-104">When you retrain a New web service, you update hello predictive web service definition tooreference hello new trained model.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="7c5b8-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c5b8-105">Prerequisites</span></span>
<span data-ttu-id="7c5b8-106">Musíte vytvořit výukový experiment a prediktivní experiment, jak je znázorněno v [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="7c5b8-106">You must set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="7c5b8-107">Prediktivní experiment Hello musí být nasazený jako počítač Azure Resource Manager (Nový) na základě učení webové služby.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-107">hello predictive experiment must be deployed as an Azure Resource Manager (New) based machine learning web service.</span></span> <span data-ttu-id="7c5b8-108">toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-108">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="7c5b8-109">Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="7c5b8-109">For more information, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="7c5b8-110">Další informace o nasazení webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="7c5b8-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="7c5b8-111">Tento proces vyžaduje, že jste nainstalovali hello rutiny Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-111">This process requires that you have installed hello Azure Machine Learning Cmdlets.</span></span> <span data-ttu-id="7c5b8-112">Informace o instalaci rutin hello Machine Learning, najdete v části hello [rutiny Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-112">For information installing hello Machine Learning cmdlets, see hello [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN.</span></span>

<span data-ttu-id="7c5b8-113">Následující informace z retraining výstup hello zkopírovaný hello:</span><span class="sxs-lookup"><span data-stu-id="7c5b8-113">Copied hello following information from hello retraining output:</span></span>

* <span data-ttu-id="7c5b8-114">BaseLocation</span><span class="sxs-lookup"><span data-stu-id="7c5b8-114">BaseLocation</span></span>
* <span data-ttu-id="7c5b8-115">RelativeLocation</span><span class="sxs-lookup"><span data-stu-id="7c5b8-115">RelativeLocation</span></span>

<span data-ttu-id="7c5b8-116">Hello postup, kterým jsou:</span><span class="sxs-lookup"><span data-stu-id="7c5b8-116">hello steps you take are:</span></span>

1. <span data-ttu-id="7c5b8-117">Přihlaste se tooyour účet Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-117">Sign in tooyour Azure Resource Manager account.</span></span>
2. <span data-ttu-id="7c5b8-118">Získat definice hello webové služby</span><span class="sxs-lookup"><span data-stu-id="7c5b8-118">Get hello web service definition</span></span>
3. <span data-ttu-id="7c5b8-119">Exportovat hello definice webové služby jako JSON</span><span class="sxs-lookup"><span data-stu-id="7c5b8-119">Export hello Web Service Definition as JSON</span></span>
4. <span data-ttu-id="7c5b8-120">Aktualizace hello odkaz toohello ilearner objektů blob v hello JSON.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-120">Update hello reference toohello ilearner blob in hello JSON.</span></span>
5. <span data-ttu-id="7c5b8-121">Importovat hello JSON do definice webové služby</span><span class="sxs-lookup"><span data-stu-id="7c5b8-121">Import hello JSON into a Web Service Definition</span></span>
6. <span data-ttu-id="7c5b8-122">Aktualizovat hello webové služby s novou definice webové služby</span><span class="sxs-lookup"><span data-stu-id="7c5b8-122">Update hello web service with new Web Service Definition</span></span>

## <a name="sign-in-tooyour-azure-resource-manager-account"></a><span data-ttu-id="7c5b8-123">Přihlaste se tooyour účet Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7c5b8-123">Sign in tooyour Azure Resource Manager account</span></span>
<span data-ttu-id="7c5b8-124">Je nutné se přihlásit v tooyour účet Azure z prostředí PowerShell hello pomocí hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-124">You must first sign in tooyour Azure account from within hello PowerShell environment using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-hello-web-service-definition"></a><span data-ttu-id="7c5b8-125">Získat hello definice webové služby</span><span class="sxs-lookup"><span data-stu-id="7c5b8-125">Get hello Web Service Definition</span></span>
<span data-ttu-id="7c5b8-126">V dalším kroku získat hello webové služby pomocí volání hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-126">Next, get hello Web Service by calling hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="7c5b8-127">Hello definice webové služby je interní reprezentací hello trained model hello webové služby a není přímo změn.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-127">hello Web Service Definition is an internal representation of hello trained model of hello web service and is not directly modifiable.</span></span> <span data-ttu-id="7c5b8-128">Ujistěte se, že dostáváte hello definice webové služby prediktivní experiment a není experimentu školení.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-128">Make sure that you are retrieving hello Web Service Definition for your Predictive experiment and not your training experiment.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="7c5b8-129">toodetermine hello název skupiny prostředků existující webovou službu, spusťte rutinu Get-AzureRmMlWebService hello bez žádné parametry toodisplay hello webové služby v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-129">toodetermine hello resource group name of an existing web service, run hello Get-AzureRmMlWebService cmdlet without any parameters toodisplay hello web services in your subscription.</span></span> <span data-ttu-id="7c5b8-130">Vyhledejte hello webové služby a podívejte se na jeho identifikátorem webové služby.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-130">Locate hello web service, and then look at its web service ID.</span></span> <span data-ttu-id="7c5b8-131">Hello název skupiny prostředků hello je čtvrtý element hello v hello ID bezprostředně za hello *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-131">hello name of hello resource group is hello fourth element in hello ID, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="7c5b8-132">V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-132">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="7c5b8-133">Alternativně toodetermine hello název skupiny prostředků existující webovou službu, protokolu na portálu Microsoft Azure Machine Learning webové služby toohello.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-133">Alternatively, toodetermine hello resource group name of an existing web service, log on toohello Microsoft Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="7c5b8-134">Vyberte hello webovou službu.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-134">Select hello web service.</span></span> <span data-ttu-id="7c5b8-135">Název skupiny prostředků Hello je pátý element hello adresy URL hello hello webové služby, bezprostředně za hello *Skupinyprostředků* element.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-135">hello resource group name is hello fifth element of hello URL of hello web service, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="7c5b8-136">V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-136">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-as-json"></a><span data-ttu-id="7c5b8-137">Exportovat hello definice webové služby jako JSON</span><span class="sxs-lookup"><span data-stu-id="7c5b8-137">Export hello Web Service Definition as JSON</span></span>
<span data-ttu-id="7c5b8-138">toomodify hello Definice toohello Trénink modelu toouse hello nově Trained Model, musíte nejprve použít hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) tooexport rutiny ho tooa souboru ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-138">toomodify hello definition toohello trained model toouse hello newly Trained Model, you must first use hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport it tooa JSON format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob-in-hello-json"></a><span data-ttu-id="7c5b8-139">Aktualizace hello odkaz toohello ilearner objektů blob v hello JSON.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-139">Update hello reference toohello ilearner blob in hello JSON.</span></span>
<span data-ttu-id="7c5b8-140">V hello prostředky, vyhledejte hello [trained model], aktualizace hello *uri* hodnota v hello *locationInfo* uzel s hello identifikátor URI objektu hello ilearner blob.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-140">In hello assets, locate hello [trained model], update hello *uri* value in hello *locationInfo* node with hello URI of hello ilearner blob.</span></span> <span data-ttu-id="7c5b8-141">Hello identifikátor URI je generována kombinováním hello *BaseLocation* a hello *RelativeLocation* z hello výstup hello BES retraining volání.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-141">hello URI is generated by combining hello *BaseLocation* and hello *RelativeLocation* from hello output of hello BES retraining call.</span></span> <span data-ttu-id="7c5b8-142">Tím se aktualizuje hello cesta tooreference hello v nový trained model.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-142">This updates hello path tooreference hello new trained model.</span></span>

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

## <a name="import-hello-json-into-a-web-service-definition"></a><span data-ttu-id="7c5b8-143">Importovat hello JSON do definice webové služby</span><span class="sxs-lookup"><span data-stu-id="7c5b8-143">Import hello JSON into a Web Service Definition</span></span>
<span data-ttu-id="7c5b8-144">Je nutné použít hello [Import AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) rutiny tooconvert hello upravit soubor JSON zpět do definice webové služby, které můžete použít tooupdate hello definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-144">You must use hello [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello modified JSON file back into a Web Service Definition that you can use tooupdate hello Web Service Definition.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service-with-new-web-service-definition"></a><span data-ttu-id="7c5b8-145">Aktualizovat hello webové služby s novou definice webové služby</span><span class="sxs-lookup"><span data-stu-id="7c5b8-145">Update hello web service with new Web Service Definition</span></span>
<span data-ttu-id="7c5b8-146">Nakonec použijte [aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) rutiny tooupdate hello definice webové služby.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-146">Finally, you use [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello Web Service Definition.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a><span data-ttu-id="7c5b8-147">Souhrn</span><span class="sxs-lookup"><span data-stu-id="7c5b8-147">Summary</span></span>
<span data-ttu-id="7c5b8-148">Pomocí rutiny správy Machine Learning PowerShell text hello, můžete aktualizovat hello trained model prediktivní povolení scénáře, jako webové služby:</span><span class="sxs-lookup"><span data-stu-id="7c5b8-148">Using hello Machine Learning PowerShell management cmdlets, you can update hello trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="7c5b8-149">Pravidelné model retraining s nová data.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-149">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="7c5b8-150">Distribuce toocustomers modelu s cílem hello aby přeučování hello model pomocí svá vlastní data.</span><span class="sxs-lookup"><span data-stu-id="7c5b8-150">Distribution of a model toocustomers with hello goal of letting them retrain hello model using their own data.</span></span>

