---
title: "aaaRetrain existující prediktivní webové služby | Microsoft Docs"
description: "Zjistěte, jak tooretrain modelu a aktualizace hello webové služby toouse hello nově trained model v Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
ms.openlocfilehash: fb0760d0a2adc34fc5f3df1ae41bdac075f91bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a>Přeučování existující prediktivní webovou službu
Tento dokument popisuje hello retraining proces hello následující scénář:

* Máte výukový experiment a prediktivní experiment, který jste nasadili jako operationalized webové služby.
* Máte nová data, které chcete vaší prediktivní webové služby toouse tooperform jeho vyhodnocování.

> [!NOTE] 
> toodeploy novou webovou službu, musíte mít dostatečná oprávnění v toowhich hello předplatné můžete nasazení hello webové služby. Další informace najdete v tématu [spravovat webové služby pomocí portálu webové služby Azure Machine Learning hello](machine-learning-manage-new-webservice.md). 

Počínaje existující webovou službu a experimentů, je třeba toofollow tyto kroky:

1. Aktualizace modelu hello.
   1. Upravte vaše tooallow experimentu školení pro webové služby vstupy a výstupy.
   2. Nasaďte hello výukový experiment jako retraining webovou službu.
   3. Pomocí hello výukový experiment spuštění služby Batch (BES) tooretrain hello modelu.
2. Použijte hello Azure Machine Learning PowerShell rutiny tooupdate hello prediktivní experiment.
   1. Přihlaste se tooyour účet Azure Resource Manager.
   2. Získáte definice hello webové služby.
   3. Exportujte hello definice webové služby jako JSON.
   4. Aktualizace hello odkaz toohello ilearner objektů blob v hello JSON.
   5. Importujte hello JSON do definice webové služby.
   6. Aktualizujte hello webové služby s novou definice webové služby.

## <a name="deploy-hello-training-experiment"></a>Nasazení výukový experiment hello
toodeploy hello výukový experiment jako retraining webové služby, je nutné přidat webovou službu vstupy a výstupy toohello modelu. Připojením *výstup webové služby* experimentu modul toohello * [Train Model] [ train-model] * modulu, povolte hello cvičení experimentu tooproduce nový trained model, který můžete použít v prediktivní experiment. Pokud máte *Evaluate Model* modul, můžete také připojit výsledky hodnocení hello výstup tooget aplikace webové služby jako výstup.

tooupdate výukový experiment:

1. Připojit *webové služby vstupní* vstupních dat tooyour modulu (například *vyčištění chybějících dat* modulu). Obvykle je vhodné tooensure, který se zpracovává vstupní data v hello stejný způsob jako původní data školení.
2. Připojit *výstup webové služby* toohello výstup modulu vaše *Train Model* modulu.
3. Pokud máte *Evaluate Model* modulu a vy chcete výsledky hodnocení hello toooutput, připojit *výstup webové služby* toohello výstup modulu vaše *Evaluate Model* modul.

Spuštění experimentu.

Dále je nutné nasadit hello výukový experiment jako webová služba, která vytváří modulu trained model a výsledky vyhodnocení modelu.  

Hello dolní části plátna experimentu hello, klikněte na **nastavit webové služby**a potom vyberte **nasazení [nové] webové služby**. Hello webové služby Azure Machine Learning portál otevře toohello **nasazení webové služby** stránky. Zadejte název pro webovou službu, zvolte plán platby a pak klikněte na tlačítko **nasadit**. Metoda Batch Execution hello můžete použít pouze pro vytváření trénované modely.

## <a name="retrain-hello-model-with-new-data-by-using-bes"></a>Přeučování model hello se nová data pomocí BES
V tomto příkladu používáme toocreate hello C# retraining aplikace. Python nebo R tooaccomplish ukázkový kód můžete také pomocí této úlohy.

hello toocall retraining API:

1. Vytvořte konzolovou aplikaci C# v sadě Visual Studio: **nový** > **projektu** > **Visual C#** > **Windows Classic Desktop** > **konzolovou aplikaci (rozhraní .NET Framework)**.
2. Přihlaste se toohello portál webové služby Machine Learning.
3. Klikněte na tlačítko hello webová služba, která kterými pracujete.
4. Klikněte na tlačítko **využívat**.
5. Na konci hello hello **spotřebě** stránku hello **ukázkový kód** klikněte na tlačítko **Batch**.
6. Zkopírujte hello ukázkový kód C# pro spuštění dávky a vložte jej do souboru Program.cs hello. Ujistěte se, že tento obor názvů hello zůstává beze změn.

Přidejte hello balíček NuGet Microsoft.AspNet.WebApi.Client, jak je uvedeno v komentářích hello. tooadd hello odkaz tooMicrosoft.WindowsAzure.Storage.dll, bude pravděpodobně nutné nejprve tooinstall hello [Klientská knihovna pro úložiště Azure services](https://www.nuget.org/packages/WindowsAzure.Storage).

Hello následující snímek obrazovky ukazuje hello **spotřebě** stránku hello webové služby Azure Machine Learning portálu.

![Využívat stránky][1]

### <a name="update-hello-apikey-declaration"></a>Aktualizovat hello apikey deklarace
Vyhledejte hello **apikey** deklarace:

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

V hello **informace o základní spotřeby** části hello **spotřebě** stránky, vyhledejte hello primární klíč a zkopírujte jej toohello **apikey** deklarace.

### <a name="update-hello-azure-storage-information"></a>Aktualizace informací o úložišti Azure hello
Hello BES ukázkový kód se uloží soubor z místního disku (například "C:\temp\CensusIpnput.csv") tooAzure úložiště, procesy a zapíše zpět tooAzure výsledky hello úložiště.  

informací o úložišti Azure hello tooupdate, je nutné ji načíst hello účet úložiště, že název, klíč a kontejner informace pro váš účet úložiště z hello portál Azure classic a correspondi hello aktualizace po spuštění experimentu, hello Výsledná pracovní postup by měl vypadat podobně jako následující toohello:

![Po spuštění výsledného pracovního postupu][4]NG hodnoty v kódu hello.

1. Přihlaste se toohello portál Azure classic.
2. V levém navigačním sloupci hello, klikněte na tlačítko **úložiště**.
3. Vyberte jeden toostore hello hello seznamu účtů úložiště retrained modelu.
4. V dolní části hello hello stránky, klikněte na tlačítko **spravovat přístupové klíče**.
5. Zkopírujte a uložte hello **primární přístupový klíč** a dialogové okno zavřít hello.
6. V horní části hello hello stránky, klikněte na tlačítko **kontejnery**.
7. Vyberte existující kontejner, nebo vytvořte novou a uložte hello název.

Vyhledejte hello *StorageAccountName*, *StorageAccountKey*, a *StorageContainerName* deklarace a hodnoty hello aktualizace, které jste uložili z portálu classic hello .

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Je také nutné zajistit, že vstupní soubor hello je k dispozici v umístění hello, který určíte v kódu hello.

### <a name="specify-hello-output-location"></a>Zadejte umístění výstupu hello
Když zadáte umístění výstupu hello v hello datová část požadavku, hello rozšíření hello souboru, který je uveden v *RelativeLocation* musí být zadány jako `ilearner`. Viz následující ukázka hello:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you want toouse for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

Hello následuje příklad výstupu retraining: ![Retraining výstup][6]

## <a name="evaluate-hello-retraining-results"></a>Vyhodnocení hello retraining výsledky
Při spuštění aplikace hello výstup hello zahrnuje hello adresy URL a sdílené přístupové podpisy token, který jsou výsledky hodnocení potřeby tooaccess hello.

Zobrazí výsledky výkonu hello hello retrained modelu kombinací hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello pro *output2* (jak je uvedeno v předchozí retraining image výstup hello) a vkládání hello úplnou adresu URL do panelu Adresa prohlížeče hello.  

Hello výsledky toodetermine zkontrolujte, zda nově trained model hello provede dobře dostatek tooreplace hello existující jeden.

Kopírování hello *BaseLocation*, *RelativeLocation*, a *SasBlobToken* z výsledků výstup hello.

## <a name="retrain-hello-web-service"></a>Přeučování hello webové služby
Když jste přeučování novou webovou službu, aktualizujete hello prediktivní webové služby definice tooreference hello nový trained model. definice Hello webové služby je interní reprezentací hello trained model hello webové služby a není přímo změn. Ujistěte se, že dostáváte hello definice webové služby prediktivní experiment a není experimentu školení.

## <a name="sign-in-tooazure-resource-manager"></a>Přihlaste se tooAzure Resource Manager
Je nutné se přihlásit v tooyour účet Azure z prostředí PowerShell hello pomocí hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.

## <a name="get-hello-web-service-definition-object"></a>Získejte objekt hello definice webové služby
V dalším kroku získat objekt hello definice webové služby podle volání hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) rutiny.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

toodetermine hello název skupiny prostředků existující webovou službu, spusťte rutinu Get-AzureRmMlWebService hello bez žádné parametry toodisplay hello webové služby v rámci vašeho předplatného. Vyhledejte hello webové služby a podívejte se na jeho identifikátorem webové služby. Hello název skupiny prostředků hello je čtvrtý element hello v hello ID bezprostředně za hello *Skupinyprostředků* element. V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternativně toodetermine hello název skupiny prostředků existující webové služby, přihlaste se toohello webové služby Azure Machine Learning portálu. Vyberte hello webovou službu. Název skupiny prostředků Hello je pátý element hello adresy URL hello hello webové služby, bezprostředně za hello *Skupinyprostředků* element. V následujícím příkladu hello název skupiny prostředků hello je výchozí. MachineLearning SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-object-as-json"></a>Export objektu definice webové služby hello jako JSON
definice hello toomodify hello trained model toouse hello nově Trénink modelu, je nutné nejprve použít hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) tooexport rutiny se soubor tooa formátu JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob"></a>Aktualizace hello odkaz toohello ilearner blob
V hello prostředky, vyhledejte hello [trained model], aktualizace hello *uri* hodnota v hello *locationInfo* uzel s hello identifikátor URI objektu hello ilearner blob. Hello identifikátor URI je generována kombinováním hello *BaseLocation* a hello *RelativeLocation* z hello výstup hello BES retraining volání.

     "asset3": {
        "name": "Retrain Sample [trained model]",
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

## <a name="import-hello-json-into-a-web-service-definition-object"></a>Importujte hello JSON do objektu definice webové služby
Je nutné použít hello [Import AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) rutiny tooconvert hello upravit soubor JSON zpět do objektu definice webové služby, které můžete použít tooupdate hello predicative experimentu.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service"></a>Aktualizovat hello webové služby
Nakonec použijte hello [aktualizace AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) tooupdate rutiny hello prediktivní experiment.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
