---
title: "problémy s Azure Data Factory aaaTroubleshoot"
description: "Zjistěte, jak tootroubleshoot problémy s používáním Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: cf65bcf3e1c3f061d3ac1dbf32e99cc7b014f9dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-data-factory-issues"></a>Poradce při potížích se službou Data Factory
Tento článek obsahuje tipy k řešení potíží pro problémy při použití Azure Data Factory. V tomto článku nejsou uvedeny všechny hello možných problémů při používání služby hello, týká se však některé problémy a Obecné tipy k řešení potíží.   

## <a name="troubleshooting-tips"></a>Rady pro řešení potíží
### <a name="error-hello-subscription-is-not-registered-toouse-namespace-microsoftdatafactory"></a>Chyba: hello předplatné není registrované toouse oboru názvů 'Microsoft.DataFactory.
Pokud se zobrazí tato chyba, poskytovatel prostředků Azure Data Factory hello nebyl registrován na váš počítač. Hello následující:

1. Spusťte Azure PowerShell.
2. Přihlaste se tooyour účet Azure pomocí hello následující příkaz.

    ```powershell
    Login-AzureRmAccount
    ```
3. Spusťte následující příkaz tooregister hello Azure Data Factory zprostředkovatele hello.

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a>Problém: Neoprávněným Chyba při spuštění rutiny služby Data Factory
Používáte pravděpodobně není pravé hello účet Azure nebo předplatné s hello prostředí Azure PowerShell. Pomocí následující rutiny tooselect hello vpravo Azure účet a předplatné toouse s hello prostředí Azure PowerShell hello.

1. Login-AzureRmAccount - použití hello správné uživatelské ID a heslo
2. Get-AzureRmSubscription - zobrazení všech hello předplatná pro účet hello.
3. Select-AzureRmSubscription &lt;název odběru&gt; -vyberte správné předplatné hello. Použití hello stejný jako ten, použijte toocreate objekt pro vytváření dat na hello portálu Azure.

### <a name="problem-fail-toolaunch-data-management-gateway-express-setup-from-azure-portal"></a>Problém: Selhání toolaunch Data Management Gateway Express instalace z portálu Azure
Hello Expresní instalace pro hello Brána pro správu dat vyžaduje Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče. Pokud hello Expresní instalace selže toostart, proveďte jednu z následujících hello:

* Použijte Internet Explorer nebo Microsoft ClickOnce kompatibilní webového prohlížeče.

    Pokud používáte Chrome, přejděte toohello [internetového obchodu Chrome](https://chrome.google.com/webstore/), hledání with – klíčové slovo "ClickOnce", vyberte jedno z rozšíření hello ClickOnce a nainstalujte ji.

    Hello stejné pro Firefox (instalace doplňku). Klikněte na tlačítko Otevřít nabídku na panelu nástrojů hello (tři vodorovné čáry v pravém horním rohu hello), klikněte na tlačítko Doplňky, hledání with – klíčové slovo "ClickOnce", vyberte jedno z rozšíření ClickOnce hello a nainstalujte ji.
* Použití hello **ruční instalace** odkazu je zobrazen na hello stejné okno hello portálu. Použijte tento postup toodownload instalační soubor a spustit ručně. Po úspěšné instalaci hello se zobrazí dialogové okno Konfigurace brány pro správu dat hello. Kopírování hello **klíč** z úvodní obrazovka portálu a použít ho v hello configuration manager toomanually registraci brány hello službou hello.  

### <a name="problem-fail-tooconnect-tooon-premises-sql-server"></a>Problém: Selhání tooconnect tooon místní systém SQL Server
Spusťte **Správce konfigurace brány pro správu dat** hello počítač brány a použít hello **Poradce při potížích s** kartě tootest hello připojení tooSQL serveru z počítače brány hello. V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) tipy k řešení potíží s připojení nebo brány související s problémy.   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a>Problém: Vstupní řezy jsou ve stavu čekání pro někdy
Hello řezy může být v **čekání** stavu z důvodu toovarious důvodů. Jednou z běžných příčin hello je tento hello **externí** příliš není nastavena vlastnost**true**. Všechny datovou sadu, která je vytvořené mimo hello oboru objektu pro vytváření dat Azure by měl být označené jako **externí** vlastnost. Tato vlastnost označuje, že hello data je externí a nejsou zálohovány pomocí žádné kanály v rámci objektu pro vytváření dat hello. Hello datové řezy jsou označeny jako **připraven** po hello dat je k dispozici v úložišti příslušných hello.

Viz následující ukázka pro použití hello hello hello **externí** vlastnost. Volitelně můžete zadat **externalData*** Pokud nastavíte externí tootrue.

V tématu [datové sady](data-factory-create-datasets.md) článku Další podrobnosti o této vlastnosti.

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

tooresolve hello chyba, přidejte hello **externí** vlastnost a hello volitelné **externalData** části definici JSON toohello hello vstupní tabulky a znovu vytvořte tabulku hello.

### <a name="problem-hybrid-copy-operation-fails"></a>Problém: Operace kopírování hybridní selže
V tématu [potíží brány](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) pro kroky tootroubleshoot problémy s kopírování do nebo z místních dat úložiště pomocí hello Brána pro správu dat.

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a>Problém: HDInsight na vyžádání zřizování nezdaří
Pokud používáte propojené služby typu HDInsightOnDemand, je nutné toospecify linkedServiceName, který odkazuje tooan Azure Blob Storage. Služba data Factory používá tento protokol toostore úložiště a podpůrné soubory pro váš cluster HDInsight na vyžádání.  Někdy zřizování clusteru HDInsight na vyžádání selže s touto hello následující chybě:

```
Failed toocreate cluster. Exception: Unable toocomplete hello cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

Tato chyba obvykle označuje, že není v hello hello umístění účtu úložiště hello zadaný v hello linkedServiceName stejném datovém centru umístění, kde se děje hello HDInsight zřizování. Příklad: Pokud je objekt pro vytváření dat v západní USA a hello úložiště Azure je v oblasti Východ USA, hello zřizování selže na vyžádání v západní USA.

Navíc existuje ještě druhá vlastnost additionalLinkedServiceNames JSON, která umožňuje zadat další účty úložiště v HDInsightu na vyžádání. Tyto další propojené účty úložiště by měla být v hello stejné umístění jako hello HDInsight cluster, nebo selže s hello stejné chybě.

### <a name="problem-custom-net-activity-fails"></a>Problém: Vlastní selže aktivity rozhraní .NET
V tématu [ladění kanálu s aktivitou vlastní](data-factory-use-custom-activities.md#troubleshoot-failures) podrobné pokyny.

## <a name="use-azure-portal-tootroubleshoot"></a>Použití portálu Azure tootroubleshoot
### <a name="using-portal-blades"></a>Pomocí portálu oken
V tématu [monitorování kanálu](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) kroky.

### <a name="using-monitor-and-manage-app"></a>Pomocí aplikace Monitorování a správa
V tématu [monitorování a Správa kanálů služby data factory pomocí monitorování a Správa aplikací](data-factory-monitor-manage-app.md) podrobnosti.

## <a name="use-azure-powershell-tootroubleshoot"></a>Použití Azure PowerShell tootroubleshoot
### <a name="use-azure-powershell-tootroubleshoot-an-error"></a>Použití Azure PowerShell tootroubleshoot chybu
V tématu [kanálů monitorování pro vytváření dat pomocí Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) podrobnosti.

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
