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
# <a name="retrain-a-new-resource-manager-based-web-service-using-hello-machine-learning-management-powershell-cmdlets"></a>Přeučování nového správce prostředků na základě webové službě pomocí rutiny prostředí PowerShell správu Machine Learning hello
Když jste přeučování novou webovou službu, aktualizujete hello prediktivní webové služby definice tooreference hello nový trained model.  

## <a name="prerequisites"></a>Požadavky
Musíte vytvořit výukový experiment a prediktivní experiment, jak je znázorněno v [Machine Learning Přeučování modelů prostřednictvím kódu programu](machine-learning-retrain-models-programmatically.md). 

> [!IMPORTANT]
> Prediktivní experiment Hello musí být nasazený jako počítač Azure Resource Manager (Nový) na základě učení webové služby. toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

Další informace o nasazení webové služby, najdete v části [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

Tento proces vyžaduje, že jste nainstalovali hello rutiny Azure Machine Learning. Informace o instalaci rutin hello Machine Learning, najdete v části hello [rutiny Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt767952.aspx) odkaz na webu MSDN.

Následující informace z retraining výstup hello zkopírovaný hello:

* BaseLocation
* RelativeLocation

Hello postup, kterým jsou:

1. Přihlaste se tooyour účet Azure Resource Manager.
2. Získat definice hello webové služby
3. Exportovat hello definice webové služby jako JSON
4. Aktualizace hello odkaz toohello ilearner objektů blob v hello JSON.
5. Importovat hello JSON do definice webové služby
6. Aktualizovat hello webové služby s novou definice webové služby

## <a name="sign-in-tooyour-azure-resource-manager-account"></a>Přihlaste se tooyour účet Azure Resource Manager
Je nutné se přihlásit v tooyour účet Azure z prostředí PowerShell hello pomocí hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.

## <a name="get-hello-web-service-definition"></a>Získat hello definice webové služby
V dalším kroku získat hello webové služby pomocí volání hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) rutiny. Hello definice webové služby je interní reprezentací hello trained model hello webové služby a není přímo změn. Ujistěte se, že dostáváte hello definice webové služby prediktivní experiment a není experimentu školení.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

toodetermine hello název skupiny prostředků existující webovou službu, spusťte rutinu Get-AzureRmMlWebService hello bez žádné parametry toodisplay hello webové služby v rámci vašeho předplatného. Vyhledejte hello webové služby a podívejte se na jeho identifikátorem webové služby. Hello název skupiny prostředků hello je čtvrtý element hello v hello ID bezprostředně za hello *Skupinyprostředků* element. V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativně toodetermine hello název skupiny prostředků existující webovou službu, protokolu na portálu Microsoft Azure Machine Learning webové služby toohello. Vyberte hello webovou službu. Název skupiny prostředků Hello je pátý element hello adresy URL hello hello webové služby, bezprostředně za hello *Skupinyprostředků* element. V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-as-json"></a>Exportovat hello definice webové služby jako JSON
toomodify hello Definice toohello Trénink modelu toouse hello nově Trained Model, musíte nejprve použít hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) tooexport rutiny ho tooa souboru ve formátu JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob-in-hello-json"></a>Aktualizace hello odkaz toohello ilearner objektů blob v hello JSON.
V hello prostředky, vyhledejte hello [trained model], aktualizace hello *uri* hodnota v hello *locationInfo* uzel s hello identifikátor URI objektu hello ilearner blob. Hello identifikátor URI je generována kombinováním hello *BaseLocation* a hello *RelativeLocation* z hello výstup hello BES retraining volání. Tím se aktualizuje hello cesta tooreference hello v nový trained model.

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

## <a name="import-hello-json-into-a-web-service-definition"></a>Importovat hello JSON do definice webové služby
Je nutné použít hello [Import AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) rutiny tooconvert hello upravit soubor JSON zpět do definice webové služby, které můžete použít tooupdate hello definice webové služby.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service-with-new-web-service-definition"></a>Aktualizovat hello webové služby s novou definice webové služby
Nakonec použijte [aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) rutiny tooupdate hello definice webové služby.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Souhrn
Pomocí rutiny správy Machine Learning PowerShell text hello, můžete aktualizovat hello trained model prediktivní povolení scénáře, jako webové služby:

* Pravidelné model retraining s nová data.
* Distribuce toocustomers modelu s cílem hello aby přeučování hello model pomocí svá vlastní data.

