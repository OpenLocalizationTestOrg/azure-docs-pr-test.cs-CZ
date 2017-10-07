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
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Webové služby Azure Machine Learning: Nasazení a využití
Můžete použít Azure Machine Learning pracovních toodeploy strojové učení a modely jako webové služby. Tyto webové služby pak lze použít toocall modely machine learningu hello z aplikací přes hello Internet toodo předpovědi v reálném čase nebo v dávkovém režimu. Protože hello webových služeb jsou dosáhl standardu RESTful, můžete je volat z různé programovací jazyky a platformy, jako je například rozhraní .NET a Javu a z aplikace, jako je například aplikace Excel.

Další části Hello poskytují toowalkthroughs odkazy, kód a dokumentace toohelp vám pomůžou začít.

## <a name="deploy-a-web-service"></a>Nasazení webové služby
### <a name="with-azure-machine-learning-studio"></a>S Azure Machine Learning Studio
Machine Learning Studio a hello portálu Microsoft Azure Machine Learning webové služby můžete nasazovat a spravovat bez nutnosti psaní kódu webové služby.

Hello následující odkazy obsahují obecné informace o tom toodeploy novou webovou službu:

* Přehled o tom, jak toodeploy novou webovou službu, která je založená na Azure Resource Manager, najdete v části [nasadit novou webovou službu](machine-learning-webservice-deploy-a-web-service.md).
* Návod, jak toodeploy webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).
* Úplné návod, jak toocreate a nasazení webové služby, najdete v části [návod krok 1: vytvoření pracovního prostoru Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).
* Konkrétní příklady, které nasazení webové služby najdete v tématu:

  * [Návod krok 5: Nasazení webové služby Azure Machine Learning hello](machine-learning-walkthrough-5-publish-web-service.md)
  * [Jak toodeploy webové služby toomultiple oblastí](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Pomocí zprostředkovatele prostředků služby webové rozhraní API (rozhraní API Správce Azure Resource Manager)
Poskytovatel prostředků Azure Machine Learning Hello webové služby umožňuje nasazení a správy webové služby pomocí volání rozhraní REST API. Další podrobnosti najdete v tématu [Machine Learning webové služby (REST)](/rest/api/machinelearning/index) odkaz.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a>Pomocí rutin prostředí PowerShell
Zprostředkovatel prostředků Azure Machine Learning pro webové služby umožňuje nasazení a správy webové služby pomocí rutin prostředí PowerShell.

rutiny hello toouse, je nutné se přihlásit v tooyour účet Azure z prostředí PowerShell hello pomocí hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny. Pokud jste obeznámeni s jak toocall prostředí PowerShell příkazy, které jsou založené na správci prostředků najdete v tématu [použití Azure Powershellu s Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).

tooexport vaše prediktivní experiment, použijte [ukázkový kód](https://github.com/ritwik20/AzureML-WebServices). Po vytvoření souboru .exe hello z hello kódu, můžete zadat:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Spuštění aplikace hello vytvoří šablonu JSON webové služby. toouse hello šablony toodeploy webové služby, musíte přidat hello následující informace:

* Název účtu úložiště a klíč

    Hello název účtu úložiště a klíč můžete získat z buď hello [portál Azure](https://portal.azure.com/) nebo hello [portál Azure classic](http://manage.windowsazure.com/).
* ID plánu závazků

    Můžete získat ID plánu hello hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu přihlášení a kliknutím na název plánu.

Přidat šablonu JSON toohello jako podřízené objekty hello *vlastnosti* uzel v hello na stejné úrovni jako hello *MachineLearningWorkspace* uzlu.

Tady je příklad:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Zobrazit hello následující články a ukázkový kód pro další podrobnosti:

* [Rutiny Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN
* Ukázka [návod](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) na Githubu

## <a name="consume-hello-web-services"></a>Využívání webových služeb hello
### <a name="from-hello-azure-machine-learning-web-services-ui-testing"></a>Z hello Azure Machine Learning webové služby uživatelského rozhraní (testování)
Webová služba z portálu hello webové služby Azure Machine Learning můžete otestovat. To zahrnuje testování hello požadavků a odpovědí služby (záznamy RR) a služba Batch Execution (BES) rozhraní.

* [Nasazení nové webové služby](machine-learning-webservice-deploy-a-web-service.md)
* [Nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md)
* [Návod krok 5: Nasazení webové služby Azure Machine Learning hello](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Z aplikace Excel
Si můžete stáhnout šablony aplikace Excel, který využívá hello webové služby:

* [Využívají webové služby Azure Machine Learning z Excelu](machine-learning-consuming-from-excel.md)
* [Add-in pro webové služby Azure Machine Learning v aplikaci Excel](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>Z klienta na základě REST
Webové služby sady Azure Machine Learning jsou rozhraní RESTful API. Můžete využívat tato rozhraní API z různých platformách, jako je například .NET, Python, R, Java, atd. hello **spotřebě** stránky pro webovou službu na hello [portálu Microsoft Azure Machine Learning webové služby](https://services.azureml.net) má ukázka kód, který vám pomůže začít pracovat. Další informace najdete v tématu [jak tooconsume Azure Machine Learning webové služby](machine-learning-consume-web-services.md).
