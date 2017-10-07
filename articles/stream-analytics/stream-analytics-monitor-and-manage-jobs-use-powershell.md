---
title: "aaaMonitor a spravovat úlohy Stream Analytics v prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak Azure PowerShell a rutiny toomonitor toouse a spravovat úlohy Stream Analytics."
keywords: "prostředí Azure powershell, rutin prostředí azure powershell, příkaz prostředí powershell, skriptů prostředí powershell"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 44abc82f1c44a5ebc1701badd6547b84dac239b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Sledovat a spravovat úlohy Stream Analytics pomocí rutin prostředí Azure PowerShell
Zjistěte, jak toomonitor a spravovat prostředky Stream Analytics s rutin prostředí Azure PowerShell a skriptů prostředí powershell, které jsou spouštěny základní úlohy Stream Analytics.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Požadavky pro spuštění rutiny prostředí Azure PowerShell pro Stream Analytics
* Vytvořte skupinu prostředků Azure v rámci vašeho předplatného. Následující Hello je ukázkový skript prostředí Azure PowerShell. Prostředí Azure PowerShell informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview);  

Azure PowerShell 0.9.8:  

         # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

         # Log in tooyour Azure account
        Login-AzureRmAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Úlohy Stream Analytics vytvořené prostřednictvím kódu programu nemají povoleno ve výchozím nastavení monitorování.  Můžete ručně povolit monitorování v hello portálu Azure tak, že přejdete na stránku toohello úlohy monitorování a kliknutím na tlačítko Povolit hello nebo můžete to provést prostřednictvím kódu programu podle následujících kroků hello nacházející se v [Azure Stream Analytics – monitorování datového proudu Analýza úlohy programově](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Rutiny Azure PowerShell pro Stream Analytics
Hello následující rutiny prostředí Azure PowerShell můžou být použité toomonitor a spravovat úlohy Azure Stream Analytics. Všimněte si, že prostředí Azure PowerShell má různé verze. 
**V příkladech hello, ke kterému se první příkaz uvedené hello prostředí Azure PowerShell 0.9.8 druhý příkaz hello je pro Azure PowerShell 1.0.** příkazy Hello Azure PowerShell 1.0 bude mít vždy "AzureRM" v příkazu hello.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Zobrazí všechny úlohy služby Stream Analytics, které jsou definované v hello předplatného Azure nebo zadaná skupina prostředků, nebo získá úlohy informace o konkrétní úloze ve skupině prostředků.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy služby Stream Analytics hello hello předplatného Azure.

**Příklad 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Tento příkaz prostředí PowerShell vrátí informace o všechny úlohy služby Stream Analytics hello ve skupině prostředků hello StreamAnalytics výchozí střed USA.

**Příklad 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o úloze Stream Analytics hello StreamingJob ve skupině prostředků hello StreamAnalytics výchozí střed USA.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Zobrazí seznam všech hello vstupních hodnot, které jsou definované v zadané úloze Stream Analytics, nebo získá informace o specifický vstup.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o všech vstupů hello definované v úloze hello StreamingJob.

**Příklad 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Tento příkaz prostředí PowerShell vrátí informace o hello vstup s názvem EntryStream definované v úloze hello StreamingJob.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Zobrazí seznam všech hello výstupů, které jsou definované v zadané úloze Stream Analytics, nebo získá informace o konkrétní výstup.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o definované v úloze hello StreamingJob výstupy hello.

**Příklad 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Tento příkaz prostředí PowerShell vrátí informace o výstup hello s názvem definované v úloze hello StreamingJob výstup.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Získá informace o hello kvótu počtu jednotek v zadané oblasti streamování.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Tento příkaz prostředí PowerShell vrátí informace o hello kvóta a využití jednotek streamování v oblasti hello střed USA.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Získá informace o konkrétní transformaci definované v úloze Stream Analytics.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Tento příkaz prostředí PowerShell vrátí informace o transformaci hello názvem StreamingJob v úloze hello StreamingJob.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Nové AzureStreamAnalyticsInput | Nové AzureRMStreamAnalyticsInput
Vytvoří nový vstup v rámci úlohy Stream Analytics nebo aktualizuje existující zadaný vstup.

Hello hello vstupu může být zadán název v souboru .json hello nebo na příkazovém řádku hello. Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.

Pokud zadáte vstup, který již existuje a není zadán hello – Force parametr rutiny hello zobrazí dotaz, zda tooreplace hello stávající vstup.

Pokud zadáte hello – Force parametr a zadejte existující zadejte název, vstup hello se nahradí bez potvrzení.

Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvořit vstup (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] části hello [rozhraní API REST správy Stream Analytics Referenční dokumentace knihovny][stream.analytics.rest.api.reference].

**Příklad 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Tento příkaz prostředí PowerShell vytvoří nový vstup z hello souboru Input.json. Pokud je již definován stávající vstup s názvem hello zadané v souboru definice vstupní hello, hello rutina požádá, jestli tooreplace ho.

**Příklad 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Tento příkaz prostředí PowerShell vytvoří nový vstup v úloze hello názvem EntryStream. Pokud je již definován stávající vstup s tímto názvem, rutina hello požádá, jestli tooreplace ho.

**Příklad 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Tento příkaz prostředí PowerShell nahrazuje hello Definice hello existující vstupní zdroj volána EntryStream s hello definice ze souboru hello.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Nové AzureStreamAnalyticsJob | Nové AzureRMStreamAnalyticsJob
Vytvoří novou úlohu služby Stream Analytics v Microsoft Azure nebo aktualizuje definice hello existující zadanou úlohu.

v souboru .json hello nebo na příkazovém řádku hello může být zadán název Hello hello úlohy. Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.

Pokud zadáte název úlohy, která již existuje a nezadávejte hello – Force parametr hello rutiny zobrazí dotaz, zda tooreplace hello existující úlohy.

Pokud zadáte hello – Force parametr a zadejte název existující úlohy, definice úlohy hello se nahradí bez potvrzení.

Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvořit úlohy Stream Analytics] [ msdn-rest-api-create-stream-analytics-job] části hello [referenční příručka Stream Analytics Management REST API Knihovna][stream.analytics.rest.api.reference].

**Příklad 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Tento příkaz prostředí PowerShell vytvoří novou úlohu z definice hello v JobDefinition.json. Pokud je již definován existující úlohy s názvem hello zadaný v souboru definice úlohy hello, hello rutina požádá, jestli tooreplace ho.

**Příklad 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Tento příkaz prostředí PowerShell nahrazuje hello definice úlohy pro StreamingJob.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Nové AzureStreamAnalyticsOutput | Nové AzureRMStreamAnalyticsOutput
Vytvoří nový výstupní v rámci úlohy Stream Analytics nebo aktualizuje existující výstup.  

v souboru .json hello nebo na příkazovém řádku hello může být zadán název Hello hello výstupu. Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.

Zadáte-li výstupu, který již existuje a není zadán hello – Force parametr hello rutiny zobrazí dotaz, zda tooreplace hello existující výstup.

Pokud zadáte hello – Force parametr a zadejte název existující výstup, výstup hello se nahradí bez potvrzení.

Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvoření výstupu (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] části hello [rozhraní API REST správy Stream Analytics Referenční dokumentace knihovny][stream.analytics.rest.api.reference].

**Příklad 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Tento příkaz prostředí PowerShell vytvoří nový výstupní názvem "výstupní" v úloze hello StreamingJob. Pokud je již definován existující výstup s tímto názvem, rutina hello požádá, jestli tooreplace ho.

**Příklad 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Tento příkaz prostředí PowerShell nahrazuje definice hello "výstupní" v úloze hello StreamingJob.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Nové AzureStreamAnalyticsTransformation | Nové AzureRMStreamAnalyticsTransformation
Vytvoří nový transformaci v rámci úlohy Stream Analytics nebo aktualizuje existující transformace hello.

v souboru .json hello nebo na příkazovém řádku hello může být zadán název Hello hello transformace. Pokud jsou zadány oba název hello na příkazovém řádku hello musí být hello stejné jako hello, jeden v souboru hello.

Pokud zadáte transformaci, která již existuje a není zadán hello – Force parametr hello rutiny zobrazí dotaz, zda tooreplace hello existující transformace.

Pokud zadáte hello – Force parametr a zadejte název existující transformace, hello transformace se nahradí bez potvrzení.

Podrobné informace o struktuře souboru JSON hello a obsah, najdete v části toohello [vytvořit transformaci (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] části hello [Stream Analytics správy Knihovna referenční dokumentace rozhraní API REST][stream.analytics.rest.api.reference].

**Příklad 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Tento příkaz prostředí PowerShell vytvoří nový transformace názvem StreamingJobTransform v úloze hello StreamingJob. Pokud je již definován transformaci existující s tímto názvem, rutina hello požádá, jestli tooreplace ho.

**Příklad 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Tento příkaz prostředí PowerShell nahrazuje hello Definice StreamingJobTransform v úloze hello StreamingJob.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Odebrat AzureStreamAnalyticsInput | Odebrat AzureRMStreamAnalyticsInput
Asynchronně odstraní specifický vstup z úlohy Stream Analytics v Microsoft Azure.  
Pokud zadáte hello – Force parametr hello vstup bude odstraněn bez potvrzení.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Tento příkaz prostředí PowerShell, že odebere hello vstupní EventStream v úloze hello StreamingJob.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Odebrat AzureStreamAnalyticsJob | Odebrat AzureRMStreamAnalyticsJob
Asynchronně odstraní určité úlohy Stream Analytics v Microsoft Azure.  
Pokud zadáte hello – Force parametr hello úlohy bude odstraněn bez potvrzení.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Tento příkaz prostředí PowerShell odebere úlohy hello StreamingJob.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Odebrat AzureStreamAnalyticsOutput | Odebrat AzureRMStreamAnalyticsOutput
Asynchronně odstraní konkrétní výstup z úlohy Stream Analytics v Microsoft Azure.  
Pokud zadáte hello – Force parametr výstup hello bude odstraněn bez potvrzení.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Toto prostředí PowerShell příkaz odebere hello výstup výstup do úlohy hello StreamingJob.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Spuštění AzureStreamAnalyticsJob | Počáteční AzureRMStreamAnalyticsJob
Asynchronně nasadí a spustí úlohu služby Stream Analytics v Microsoft Azure.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Tento příkaz prostředí PowerShell spustí hello úlohy StreamingJob s časem zahájení vlastní výstup nastavit tooDecember 12, 2012, 12:12:12 UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob
Asynchronně Ukončí úlohu služby Stream Analytics z běžící v Microsoft Azure a zrušte přiděluje prostředky, které byly, který se používá. Hello definice úlohy a metadata bude stále k dispozici v rámci vašeho předplatného prostřednictvím hello portál Azure a rozhraní API pro správu tak, aby hello úlohy lze upravit a restartovat. Vám nebude nic účtováno pro úlohu ve stavu hello byla zastavena.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Tento příkaz prostředí PowerShell zastaví úlohu hello StreamingJob.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test AzureStreamAnalyticsInput | Test AzureRMStreamAnalyticsInput
Schopnost hello testy tooa tooconnect Stream Analytics zadán vstup.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Toto prostředí PowerShell příkaz testy hello stav připojení hello vstupní EntryStream v StreamingJob.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test AzureStreamAnalyticsOutput | Test AzureRMStreamAnalyticsOutput
Schopnost hello testy tooa tooconnect Stream Analytics zadán výstup.

**Příklad 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Toto prostředí PowerShell příkaz testy hello stav připojení hello výstup výstupu v StreamingJob.  

## <a name="get-support"></a>Získat podporu
Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Další kroky
* [Úvod tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Začínáme používat službu Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
* [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

