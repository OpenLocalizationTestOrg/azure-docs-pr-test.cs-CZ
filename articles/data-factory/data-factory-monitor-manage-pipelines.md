---
title: "aaaMonitor a Správa kanálů pomocí hello portál Azure a prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toouse hello portál Azure a prostředí Azure PowerShell toomonitor a spravovat hello Azure data Factory a kanály, které jste vytvořili."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: spelluru
ms.openlocfilehash: a8d3c7943e79450895ff754f06a37fdad1cbef92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-hello-azure-portal-and-powershell"></a>Monitorování a Správa kanálů služby Azure Data Factory pomocí hello portál Azure a prostředí PowerShell
> [!div class="op_single_selector"]
> * [Použití Azure portal nebo Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Pomocí monitorování a správu aplikací](data-factory-monitor-manage-app.md)


> [!IMPORTANT]
> monitorování a správu aplikace Hello poskytuje lepší podporu pro monitorování a správy datových kanálů a řešení potíží s problémy. Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md). 


Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí portálu Azure a prostředí PowerShell. Hello článku také obsahuje informace o tom, jak toocreate výstrahy a získat oznámení o selhání.

## <a name="understand-pipelines-and-activity-states"></a>Pochopit kanály a aktivity stavy
Pomocí hello portálu Azure, můžete:

* Objekt pro vytváření dat zobrazte jako diagram.
* Zobrazit aktivity v kanálu.
* Zobrazte vstupní a výstupní datové sady.

Tato část také popisuje, jak řez datovou sadu přechází z jednoho stavu tooanother.   

### <a name="navigate-tooyour-data-factory"></a>Přejděte tooyour pro vytváření dat
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **datové továrny** hello nabídky na levé straně hello. Pokud ho nevidíte, klikněte na tlačítko **další služby >**a potom klikněte na **datové továrny** pod hello **INTELLIGENCE + analýzy** kategorie.

   ![Procházet všechny > datové továrny](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Na hello **datové továrny** okně, vyberte hello datovou továrnu, která vás zajímá.

    ![Vyberte objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Měli byste vidět hello Domovská stránka objektu pro vytváření dat hello.

   ![Okno objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Zobrazení diagramu svojí datové továrny
Hello **Diagram** zobrazení objektu pro vytváření dat poskytuje jedno podokno pohotovostní toomonitor a spravovat hello data factory a její prostředky. toosee hello **Diagram** zobrazení objektu pro vytváření dat, klikněte na tlačítko **Diagram** na hello domovské stránce objektu pro vytváření dat hello.

![Zobrazení diagramu](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Přiblížení, oddálení, zvětšení toofit, % too100 přiblížení, uzamčení hello rozložení diagramu hello a automatické umísťování kanálů a datové sady. Můžete také zjistit informace o rodokmenu dat hello (tedy zobrazit nadřazené a podřízené položky vybraných položek).

### <a name="activities-inside-a-pipeline"></a>Aktivity v kanálu
1. Klikněte pravým tlačítkem na hello kanál a pak klikněte na **otevřít kanál** toosee všechny aktivity v hello kanálu spolu s vstupní a výstupní datové sady pro hello aktivity. Tato funkce je užitečná, pokud vaše kanál obsahuje více než jednu aktivitu a chcete toounderstand hello provozní rodokmenu jednoho kanálu.

    ![Nabídka Otevřít kanál](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. V následujícím příkladu hello najdete v části aktivity kopírování v kanálu hello s vstup a výstup. 

    ![Aktivity v kanálu](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Můžete přejít zpět toohello Domovská stránka objektu pro vytváření dat hello kliknutím hello **objekt pro vytváření dat** odkaz v hello s popisem cesty v levém horním rohu hello.

    ![Přejděte zpět toodata factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-hello-state-of-each-activity-inside-a-pipeline"></a>Zobrazení stavu hello každé aktivity v kanálu
Můžete zobrazit aktuální stav hello aktivity zobrazením hello stav každého hello datové sady, které vytváří aktivitou hello.

Dvojitým kliknutím na soubor hello **OutputBlobTable** v hello **Diagram**, zobrazí se všechny hello datové řezy, které vznikají pomocí funkcí spustí jinou aktivitu v kanálu. Uvidíte, že aktivity kopírování hello proběhla úspěšně pro hello posledních 8 hodin a vytvořeného hello řezy v hello **připraven** stavu.  

![Stav hello kanálu](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Hello řezy datovou sadu v datové továrně hello může mít jednu z hello následující stavy:

<table>
<tr>
    <th align="left">Stav</th><th align="left">Dílčím stavem</th><th align="left">Popis</th>
</tr>
<tr>
    <td rowspan="8">Čekání</td><td>ScheduleTime</td><td>pro toorun hello řezu ještě nenastal čas Hello.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Hello upstreamové závislosti nejsou připravené.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Hello výpočetní prostředky nejsou k dispozici.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Všechny instance aktivit hello jsou právě zpracovávají jiné řezy.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Hello aktivita je pozastavená a hello řezy nelze spustit, dokud je obnoveno hello aktivity.</td>
</tr>
<tr>
<td>Opakování</td><td>Probíhá pokus o spuštění aktivity je zopakován.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření se ještě nespustilo.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Ověření je čekání toobe opakovat.</td>
</tr>
<tr>
<tr>
<td rowspan="2">InProgress</td><td>Probíhá ověřování</td><td>Probíhá ověřování.</td>
</tr>
<td>-</td>
<td>Hello řez se zpracovává.</td>
</tr>
<tr>
<td rowspan="4">Se nezdařilo</td><td>TimedOut</td><td>provedení aktivity Hello trvalo déle, než je povolené aktivitou hello.</td>
</tr>
<tr>
<td>Zrušeno</td><td>řez Hello zrušil akce uživatele.</td>
</tr>
<tr>
<td>Ověření</td><td>Ověření se nezdařilo.</td>
</tr>
<tr>
<td>-</td><td>řez Hello se nezdařilo toobe vygenerovat nebo ověřit.</td>
</tr>
<td>Připraveno</td><td>-</td><td>Hello řez je připraven ke spotřebování.</td>
</tr>
<tr>
<td>Přeskočena</td><td>Žádný</td><td>Hello řez se zpracovává.</td>
</tr>
<tr>
<td>Žádný</td><td>-</td><td>Řez používá tooexist jiný stav, ale byla obnovena.</td>
</tr>
</table>



Hello podrobnosti o řez lze zobrazit kliknutím na položku řez na hello **nedávno aktualizován řezy** okno.

![Řez podrobnosti](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Pokud hello řez provedl vícekrát, zobrazí více řádků v hello **aktivita spuštěna** seznamu. Můžete zobrazit podrobnosti o aktivitě spustíte kliknutím na položku hello spustit v hello **aktivita spuštěna** seznamu. Hello seznamu jsou uvedeny všechny soubory protokolu hello, spolu s chybovou zprávu, pokud existuje. Tato funkce je užitečná protokoly tooview a ladění bez nutnosti tooleave datovou továrnu.

![Podrobnosti o spuštění aktivit](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Není-li řez hello v hello **připraven** stavu, můžete zobrazit upstreamové datové řezy hello, které nejsou připravené a které blokují hello aktuálního řezu v hello **Upstreamové datové řezy, které nejsou připraveny** seznamu. Tato funkce je užitečná, když vaše řez v **čekání** stavu a chcete, aby hello toounderstand nadřazeného se závislosti, které hello řez čeká na.

![Upstreamové datové řezy, které nejsou připraveny](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Diagram stavu datové sady
Po nasazení služby data factory a kanály hello mají platný aktivní období, datová sada hello řezy přechod z jednoho stavu tooanother. V současné době stav řezu hello dodržuje hello následující diagram stavu:

![Diagram stavu](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Hello tok přechod stavu datovou sadu v datové továrně je hello následující: Čekání na -> průběh/v – probíhající (ověřování) -> Připraveno nebo se nezdařilo.

řez Hello se spustí v **čekání** stavu čekání toobe předběžné podmínky splněny, než se provede. Pak hello aktivity spustí provádění a hello řez přejde do **probíhající** stavu. provedení aktivity Hello může úspěch nebo neúspěch. Hello řez je označena jako **připraven** nebo **se nezdařilo**, podle hello výsledek spuštění hello.

Můžete resetovat hello řez toogo zpět z hello **připraven** nebo **se nezdařilo** stavu toohello **čekání** stavu. Můžete také označit stav řezu hello příliš**přeskočit**, která brání hello aktivity z provádění a není zpracování řezu hello.

## <a name="pause-and-resume-pipelines"></a>Pozastavení a obnovení kanálů
Kanály můžete spravovat pomocí prostředí Azure PowerShell. Například můžete pozastavit a obnovit kanály spuštěním rutin prostředí Azure PowerShell. 

> [!NOTE] 
> zobrazení diagramu Hello nepodporuje pozastavení a obnovení kanály. Pokud chcete toouse uživatelské rozhraní, použijte hello sledování a správu aplikací. Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md) článku. 

Je možné pozastavit nebo pozastavit kanály pomocí hello **Suspend-AzureRmDataFactoryPipeline** rutiny prostředí PowerShell. Tato rutina je užitečná, když nechcete, aby toorun kanály dokud nebude problém vyřešen. 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Například:

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Po napravení problému hello s hello kanálu, můžete obnovit hello pozastaveno kanálu tak, že spustíte následující příkaz prostředí PowerShell hello:

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Například:

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Ladit kanály
Azure Data Factory poskytuje bohaté možnosti pro vás toodebug a řešení potíží kanály pomocí hello portál Azure a prostředí Azure PowerShell.

> [! Poznámka:} je mnohem snazší tootroubleshot, které chyb s použitím hello monitorování aplikace a správu. Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md) článku. 

### <a name="find-errors-in-a-pipeline"></a>Vyhledejte chyby v kanálu
Pokud se nezdaří hello aktivity při spuštění v kanálu, hello datovou sadu, která je vytvořena hello kanálu je v chybovém stavu z důvodu selhání hello. Můžete ladění a odstraňování chyb v Azure Data Factory pomocí hello následující metody.

#### <a name="use-hello-azure-portal-toodebug-an-error"></a>Použít hello Azure portálu toodebug chybu
1. Na hello **tabulky** okně klikněte na tlačítko hello problém řez, který má hello **stav** nastavit příliš**se nezdařilo**.

   ![Okno tabulky s řez problém](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. Na hello **datový řez** okně klikněte na tlačítko hello aktivity při spuštění, který selhal.

   ![Datový řez s chybou](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. Na hello **podrobnosti o spuštění aktivit** okně si můžete stáhnout hello soubory, které jsou spojeny s HDInsight zpracování hello. Klikněte na tlačítko **Stáhnout** pro stav nebo stderr toodownload hello Chyba souboru protokolu, který obsahuje podrobnosti o chybě hello.

   ![Aktivity při spuštění podrobnosti okno s chybou](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-toodebug-an-error"></a>Pomocí prostředí PowerShell toodebug chybu
1. Spusťte **PowerShell**.
2. Spustit hello **Get-AzureRmDataFactorySlice** příkaz toosee hello řezy a jejich stav. Měli byste vidět řez hello stav **se nezdařilo**.        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Například:

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Nahraďte **StartDateTime** se časem spuštění vaší kanálu. 
3. Nyní, spustit hello **Get-AzureRmDataFactoryRun** rutiny tooget podrobnosti o aktivitě hello spustit pro řez hello.

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Například:

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    Hello hodnota StartDateTime je čas spuštění hello hello chyby nebo problému řez, který jste si poznamenali v předchozím kroku hello. Hello datum a čas by měl být uzavřena do uvozovek.
4. Měli byste vidět výstup s podrobnostmi o chybě hello, který je podobný toohello následující:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. Můžete spustit hello **uložit AzureRmDataFactoryLog** rutiny s hello hodnota Id najdete v části z výstupu hello a stažení souborů protokolu hello pomocí hello **- DownloadLogsoption** pro rutinu hello.

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Znovu spustit chyby v kanálu

> [!IMPORTANT]
> Je snazší tootroubleshoot chyby a znovu spusťte selhání řezy pomocí hello monitorování a správu aplikací. Podrobnosti o použití aplikace hello najdete v tématu [sledování a Správa kanálů služby Data Factory pomocí monitorování a správu aplikace hello](data-factory-monitor-manage-app.md). 

### <a name="use-hello-azure-portal"></a>Hello použití portálu Azure
Po řešení potíží a ladění chyby v kanálu se může znovu selhání navigace toohello chyba řez a kliknutím na hello **spustit** tlačítka na panelu příkazů hello.

![Opětovné spuštění neúspěšné řez](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

V případě hello řezu selhalo ověření z důvodu selhání zásad (například pokud data nejsou k dispozici), můžete opravit chyby hello a znovu ověřit kliknutím hello **ověřením** tlačítka na panelu příkazů hello.

![Opravte chyby a ověření](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Použití Azure Powershell
Selhání může znovu pomocí hello **Set-AzureRmDataFactorySliceStatus** rutiny. V tématu hello [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) téma pro syntaxi a další podrobnosti o rutině hello.

**Příklad:**

Hello následující příklad ilustruje hello stav všech řezech pro hello tabulka 'DAWikiAggregatedData' too'Waiting' v Azure data factory hello 'WikiADF'.

Hello 'typ aktualizace, je nastaven too'UpstreamInPipeline ', což znamená, že stavy každý řez hello tabulky a všechny hello závislé (nadřazený) tabulky nastavené too'Waiting'. Hello jiných možná hodnota pro tento parametr je "Individuální".

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="create-alerts"></a>Vytváření upozornění
Azure protokoly událostí uživatele při prostředků Azure (například objekt pro vytváření dat) je vytvořen, aktualizovat ani odstranit. Výstrahy můžete vytvořit na těchto událostech. Můžete použít různé metriky toocapture objekt pro vytváření dat a vytvářet výstrahy o metrikách. Doporučujeme použít události pro monitorování v reálném čase a použít metriky pro účely záznamu historie událostí.

### <a name="alerts-on-events"></a>Výstrahy na události
Azure události poskytují užitečné přehledy co se děje v vašich prostředků Azure. Pokud používáte Azure Data Factory, události se generují při:

* Objekt pro vytváření dat je vytvořen, aktualizovat ani odstranit.
* Zpracování dat (jako "spuštěno") je spuštěna, nebo byla dokončena.
* Vytvoření clusteru HDInsight na vyžádání nebo odebrat.

Můžete vytvářet výstrahy na těchto událostech uživatele a jejich konfigurace toosend e-mailové oznámení toohello správce a coadministrators hello předplatného. Kromě toho můžete zadat další e-mailové adresy uživatelů, kteří potřebují tooreceive e-mailová oznámení, pokud jsou splněny podmínky hello. Tato funkce je užitečná, když chcete tooget upozornění na selhání a nechcete, aby toocontinuously monitorování vaší služby data factory.

> [!NOTE]
> V současné době nepodporuje hello portálu zobrazit výstrahy na události. Použití hello [monitorování a správu aplikace](data-factory-monitor-manage-app.md) toosee všechny výstrahy.


#### <a name="specify-an-alert-definition"></a>Zadejte definici výstrah
toospecify výstrahy definice, vytvořte soubor JSON, který popisuje hello operace, které chcete toobe upozorněni na. V následujícím příkladu hello odešle výstrahu hello e-mailové oznámení pro hello RunFinished operaci. konkrétní toobe, e-mailových oznámení je odeslána při spuštění v datové továrně hello bylo dokončeno a hello spuštění se nezdařilo (stav = FailedExecution).

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of hello data slices for hello Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

Můžete odebrat **subStatus** z hello definici JSON, pokud nechcete, aby toobe upozorněni na konkrétní chyby.

Tento příklad nastaví hello upozornění pro všechny datové továrny v rámci vašeho předplatného. Pokud chcete nastavit pro objekt pro vytváření dat konkrétní výstrahu toobe hello, můžete zadat objekt pro vytváření dat **resourceUri** v hello **dataSource**:

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

Hello následující tabulka obsahuje seznam hello dostupné operace a stavy (a dílčí stavy).

| Název operace | Status | Podřízený stav |
| --- | --- | --- |
| RunStarted |spuštění |Spouštění |
| RunFinished |Nemohl / bylo úspěšné |FailedResourceAllocation<br/><br/>Úspěch<br/><br/>FailedExecution<br/><br/>TimedOut<br/><br/>< zrušena<br/><br/>FailedValidation<br/><br/>opuštění |
| OnDemandClusterCreateStarted |spuštění | |
| OnDemandClusterCreateSuccessful |Úspěch | |
| OnDemandClusterDeleted |Úspěch | |

V tématu [vytvořit pravidlo výstrahy](https://msdn.microsoft.com/library/azure/dn510366.aspx) podrobnosti o hello elementy JSON, které se používají v příkladu hello.

#### <a name="deploy-hello-alert"></a>Nasazení hello výstrahy
toodeploy hello výstrahy, použijte rutinu prostředí Azure PowerShell hello **New-AzureRmResourceGroupDeployment**, jak ukazuje následující příklad hello:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

Po nasazení skupiny prostředků hello byl úspěšně dokončen, zobrazí hello následující zprávy:

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - hello StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts tooremove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> Můžete použít hello [vytvořit pravidlo výstrahy](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST API toocreate pravidlo výstrahy. datová část JSON Hello je podobný příklad toohello JSON.  


#### <a name="retrieve-hello-list-of-azure-resource-group-deployments"></a>Načtení seznamu hello nasazení skupiny prostředků Azure.
seznam hello tooretrieve nasazené nasazení skupiny prostředků Azure, použijte rutinu hello **Get-AzureRmResourceGroupDeployment**, jak ukazuje následující příklad hello:

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a>Řešení potíží s událostmi uživatele
1. Zobrazí všechny události hello, které jsou generovány po kliknutí na hello **metriky a operace** dlaždici.

    ![Dlaždice metriky a operace](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. Klikněte na tlačítko hello **události** dlaždici toosee hello události.

    ![Dlaždice události](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. Na hello **události** okno, můžete zobrazit podrobnosti o události, filtrované události a tak dále.

    ![Okno události](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Klikněte na tlačítko **operace** v seznamu hello operace, která způsobuje chybu.

    ![Vyberte operaci](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. Klikněte na tlačítko **chyba** událostí toosee podrobnosti o chybě hello.

    ![Chyba události](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

V tématu [rutiny Azure přehledy](https://msdn.microsoft.com/library/mt282452.aspx) pro rutiny prostředí PowerShell, které můžete použít tooadd, získat, nebo odeberte výstrahy. Tady je několik příkladů použití hello **Get-AlertRule** rutiny:

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of hello data slices for hello Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

Spusťte následující podrobnosti toosee příkazy get-help a příklady pro rutinu Get-AlertRule hello hello.

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


Pokud se zobrazí události hello generování výstrah v hello okno portálu, ale nebudete dostávat e-mailová oznámení, zkontrolujte, zda text hello e-mailovou adresu, která je zadána je nastaven tooreceive e-mailů externích odesílatelů. výstrahy e-mailů Hello může mít zablokovaný nastavení e-mailu.

### <a name="alerts-on-metrics"></a>Výstrahy na metriky
V objektu pro vytváření dat můžete zachytit různé metriky a vytvořit oznámení o metrikách. Můžete sledovat a vytvářet upozornění na následující metriky hello řezy v datové továrně hello:

* **Spustí se nezdařilo**
* **Úspěšné spuštění**

Tyto metriky jsou užitečné a vám pomohou tooget přehled celkového úspěšné a neúspěšné spustí v datové továrně hello. Metriky jsou vydávány pokaždé, když je spustit řez. Tyto metriky od začátku hello hello hodina, jsou agregovat a nabídnutých tooyour účet úložiště. metriky tooenable nastavit účet úložiště.

#### <a name="enable-metrics"></a>Povolit metriky
tooenable metriky, klikněte na následující hello z hello **Data Factory** okno:

**Monitorování** > **metrika** > **nastavení pro diagnostiku** > **diagnostiky**

![Odkaz diagnostiky](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

Na hello **diagnostiky** okně klikněte na tlačítko **na**, vyberte účet úložiště hello a klikněte na tlačítko **Uložit**.

![Okno diagnostiky](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

To může trvat až hodinu tooone toobe metriky hello viditelný na hello **monitorování** okno protože metriky agregace se stane každou hodinu.

### <a name="set-up-an-alert-on-metrics"></a>Nastavit výstrahy na metriky
Klikněte na tlačítko hello **metriky služby Data Factory** dlaždice:

![Dlaždice metriky objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Na hello **metrika** okně klikněte na tlačítko **+ přidat upozornění** na panelu nástrojů hello.
![Okno metriky objekt pro vytváření dat > Přidat upozornění](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Na hello **přidání pravidla výstrahy** proveďte hello následující kroky a klikněte na tlačítko **OK**.

* Zadejte název pro výstrahu hello (Příklad: "se nezdařilo výstraha").
* Zadejte popis výstrahy hello (Příklad: "Odeslat e-mail, pokud dojde k selhání").
* Vyberte metriku (vs "Spuštění se nezdařilo". "Úspěšně spuštěno").
* Zadejte podmínku a prahová hodnota.   
* Zadejte dobu hello.
* Určete, zda tooowners, přispěvatelé a čtenáři se mají odesílat e-mailu.

![Okno metriky objekt pro vytváření dat > Přidat pravidlo výstrahy](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Po hello pravidlo výstrahy bylo úspěšně přidáno, zavře se okno hello a zobrazí nová výstraha hello na hello **metrika** okno.

![Okno metriky objekt pro vytváření dat > Přidat nové výstrahy](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

Měli byste taky vidět hello počet výstrah v hello **výstrah pravidla** dlaždici. Klikněte na tlačítko hello **výstrah pravidla** dlaždici.

![Data factory metriky okno - pravidla výstrah](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

Na hello **výstrahy pravidla** okně se zobrazí všechny existující výstrahy. tooadd výstraha, klikněte na tlačítko **přidat upozornění** na panelu nástrojů hello.

![Okno pravidla výstrah](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Oznámení o výstrahách
Po pravidlo výstrahy hello odpovídá hello podmínku, měli byste obdržet e-mail s upozorněním, že se aktivuje výstraha hello. Po hello problém se vyřeší a výstražný stav hello neodpovídá už, můžete získat e-mailu s upozorněním, že hello výstraha vyřeší.

Toto chování se liší od událostí, kde jsou oznámení odesílána v každé selhání, který identifikuje pravidlo výstrahy pro.

### <a name="deploy-alerts-by-using-powershell"></a>Výstrahy nasazení pomocí prostředí PowerShell
Výstrahy můžete nasadit pro metriky hello stejné tak, aby se pro události.

**Definice upozornění**

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

Nahraďte *subscriptionId*, *resourceGroupName*, a *dataFactoryName* v ukázce hello s příslušnými hodnotami.

*metricName* aktuálně podporuje dvě hodnoty:

* FailedRuns
* SuccessfulRuns

**Nasazení hello výstrahy**

toodeploy hello výstrahy, použijte rutinu prostředí Azure PowerShell hello **New-AzureRmResourceGroupDeployment**, jak ukazuje následující příklad hello:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

Měli byste vidět následující zprávou po úspěšné nasazení:

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

Můžete taky hello **přidat AlertRule** rutiny toodeploy pravidlo výstrahy. V tématu hello [přidat AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) téma podrobné informace a příklady.  

## <a name="move-a-data-factory-tooa-different-resource-group-or-subscription"></a>Přesunout data factory tooa jiné skupině prostředků nebo předplatného
Můžete přesunout data factory tooa jiné skupině prostředků nebo jiného předplatného pomocí hello **přesunout** příkazu panelu tlačítko na domovskou stránku hello svojí datové továrny.

![Přesunout objekt pro vytváření dat](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Také můžete přesunout všechny související prostředky (například výstrahy, které jsou spojeny s hello data factory), spolu s hello data factory.

![Dialogové okno prostředků](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
