---
title: "aaaCollecting data Log Analytics pomocí sady runbook ve službě Azure Automation | Microsoft Docs"
description: "Podrobný kurz, který provede procesem vytvoření sady runbook v Azure Automation toocollect data do úložiště hello OMS pro analýzu, analýzy protokolů."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: e644dc3ef20fb1e930cae02e0fd44ccca31dc13d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Shromáždit data Log Analytics s runbook služby automatizace Azure
Můžete shromáždit významné množství dat v analýzy protokolů z různých zdrojů včetně [zdroje dat](../log-analytics/log-analytics-data-sources.md) na agentech a také [data shromážděná z Azure](../log-analytics/log-analytics-azure-storage.md).  V případě, kdy potřebujete toocollect data, není přístupná prostřednictvím těchto zdrojů je standardní existují scénáře.  V těchto případech můžete použít hello [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md) toowrite data tooLog analýzy libovolného klienta REST API.  Běžné tooperform metoda tento shromažďování dat je pomocí sady runbook ve službě Azure Automation.   

Tento kurz vás provede hello procesu pro vytváření a plánování v sadě runbook v Azure Automation toowrite data tooLog Analytics.


## <a name="prerequisites"></a>Požadavky
Tento scénář vyžaduje hello následující prostředky, které jsou nakonfigurované ve vašem předplatném Azure.  Jak může být bezplatný účet.

- [Přihlaste se pracovní prostor analýzy](../log-analytics/log-analytics-get-started.md).
- [Účet Azure automation](../automation/automation-offering-get-started.md).

## <a name="overview-of-scenario"></a>Přehled scénáře
V tomto kurzu napíšete sadu runbook, která shromažďuje informace o automatizaci úloh.  Runbooky ve službě Azure Automation jsou implementované v prostředí PowerShell, spusťte psaní a testování skript v editoru hello Azure Automation.  Jakmile ověříte, že shromažďujete hello požadované informace, budete zápis tohoto data tooLog analýzy a ověřte vlastní datový typ hello.  Nakonec runbook hello toostart plán vytvoříte v pravidelných intervalech.

> [!NOTE]
> Můžete nakonfigurovat Azure Automation toosend úlohy informace tooLog Analytics bez této sady runbook.  Tento scénář je primárně kurzu hello použité toosupport a se doporučuje poslat hello data tooa testovací prostoru.  


## <a name="1-install-data-collector-api-module"></a>1. Instalace modulu rozhraní API sady kolekcí dat
Každý [žádost od hello rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md#create-a-request) musí být správně naformátován a obsahovat hlavičku autorizace.  Můžete to provést ve vašem runbooku, ale můžete snížit množství hello pomocí modulu, který zjednodušuje tento proces vyžaduje kód.  Jeden modul, který můžete použít se [OMSIngestionAPI modulu](https://www.powershellgallery.com/packages/OMSIngestionAPI) v hello Galerie prostředí PowerShell.

toouse [modulu](../automation/automation-integration-modules.md) v sadě runbook, musí být nainstalován ve vašem účtu Automation.  Všechny sady runbook v hello pak můžete použít stejný účet hello funkce v modulu hello.  Můžete nainstalovat nový modul výběrem **prostředky** > **moduly** > **přidat modul** ve vašem účtu Automation.  

Hello Galerie prostředí PowerShell, když vám dává možnost rychlé toodeploy modul přímo tooyour automatizace účtem, takže můžete použít tuto možnost pro účely tohoto kurzu.  

![Modul OMSIngestionAPI](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. Přejděte příliš[Galerie prostředí PowerShell](https://www.powershellgallery.com/).
2. Vyhledejte **OMSIngestionAPI**.
3. Klikněte na hello **nasazení tooAzure automatizace** tlačítko.
4. Vyberte svůj účet automation a klikněte na **OK** tooinstall hello modulu.


## <a name="2-create-automation-variables"></a>2. Vytvoření proměnné služeb automatizace
[Proměnné služeb automatizace](..\automation\automation-variables.md) obsahovat hodnoty, které mohou být využívána všem runbookům v účtu Automation.  Provádění sady runbook více flexibilní tak, že umožní toochange tyto hodnoty bez úprav hello skutečné runbook. Každý požadavek hello rozhraní API sady kolekcí dat protokolu HTTP vyžaduje hello ID a klíč pracovního prostoru hello OMS a proměnné prostředky jsou ideální toostore tyto informace.  

![Proměnné](media/operations-management-suite-runbook-datacollect/variables.png)

1. V hello portálu Azure přejděte tooyour účet Automation.
2. Vyberte **proměnné** pod **sdílené prostředky**.
2. Klikněte na tlačítko **přidat proměnnou** a vytvořte dvě proměnné hello v hello následující tabulka.

| Vlastnost | Hodnota ID pracovního prostoru | Hodnota klíče pracovního prostoru |
|:--|:--|:--|
| Name (Název) | ID pracovního prostoru | WorkspaceKey |
| Typ | Řetězec | Řetězec |
| Hodnota | Vložte hello ID pracovního prostoru pracovní prostor analýzy protokolů. | Vkládání pomocí hello primární nebo sekundární klíč pracovního prostoru analýzy protokolů. |
| Šifrované | Ne | Ano |



## <a name="3-create-runbook"></a>3. Vytvoření sady runbook

Automatizace Azure má editoru portálu hello, kde můžete upravit a otestujte svůj runbook.  Máte hello možnost toouse hello skriptu editor toowork s [prostředí PowerShell přímo](../automation/automation-edit-textual-runbook.md) nebo [vytvořit grafický runbook](../automation/automation-graphical-authoring-intro.md).  V tomto kurzu bude fungovat se skript prostředí PowerShell. 

![Úprava runbooku](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. Přejděte tooyour účet Automation.  
2. Klikněte na tlačítko **Runbooky** > **přidat runbook** > **vytvořit nový runbook**.
3. Název sady runbook hello, zadejte **shromažďování. automatizace úloh**.  Typ runbooku hello, vyberte **prostředí PowerShell**.
4. Klikněte na tlačítko **vytvořit** toocreate hello sady runbook a počáteční hello editor.
5. Zkopírujte a vložte následující kód do runbooku hello hello.  Toohello komentáře ve skriptu hello najdete vysvětlení hello kódu.
    
        # Get information required for hello automation account from parameter values when hello runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate toohello Automation account using hello Azure connection created when hello Automation account was created.
        # Code copied from hello runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Add-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set hello $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set hello name of hello record type.
        $logType = "AutomationJob"
        
        # Get hello jobs from hello past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert hello job data toojson
            $body = $jobs | ConvertTo-Json
        
            # Write hello body tooverbose output so we can inspect it if verbose logging is on for hello runbook.
            Write-Verbose $body
        
            # Send hello data tooLog Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a>4. Služba test runbook
Služby Azure Automation zahrnuje prostředí příliš[Otestujte svůj runbook](../automation/automation-testing-runbook.md) před publikováním.  Můžete zkontrolovat hello data shromažďovaná společností hello runbook a ověřte, že se zapíše tooLog Analytics podle očekávání před publikováním tooproduction. 
 
![Služba test runbook](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. Klikněte na tlačítko **Uložit** toosave hello runbook.
1. Klikněte na tlačítko **testovací podokno** tooopen hello runbook v testovacím prostředí hello.
3. Vzhledem k tomu, že vaše sada runbook obsahuje parametry, jste výzvami tooenter hodnoty pro ně.  Zadejte název skupiny prostředků hello hello a automatizace hello účet, který vaše informace o úloze probíhající toocollect z.
4. Klikněte na tlačítko **spustit** toohello spuštění sady runbook hello.
3. Hello runbook se spustí se stavem **zařazeno ve frontě** před probíhá příliš**systémem**.  
3. Hello runbook by měl zobrazit podrobný výstup s úlohami hello shromážděných ve formátu json.  Pokud nejsou uvedeny žádné úlohy, pak může byly žádné úlohy vytvořené v účtu automation hello v hello poslední hodinu.  Pokuste se spustit žádné sady runbook v účtu automation hello a pak znovu proveďte hello test.
4. Ujistěte se, že výstup hello nezobrazí, že všechny chyby hello post příkaz tooLog Analytics.  Měli byste mít podobné toohello následující zpráva.

    ![Výstup POST](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. Ověřit záznamy v analýzy protokolů
Po hello runbook byla dokončena v testu, a jste ověřili, že byl úspěšně přijat výstup hello, můžete ověřit, že hello záznamy byly vytvořené pomocí [hledání protokolů v analýzy protokolů](../log-analytics/log-analytics-log-searches.md).

![Protokolování výstupu](media/operations-management-suite-runbook-datacollect/log-output.png)

1. V hello portálu Azure vyberte pracovní prostor analýzy protokolů.
2. Klikněte na **protokolu vyhledávání**.
3. Typ hello následující příkaz `Type=AutomationJob_CL` a klikněte na tlačítko Hledat hello. Všimněte si, že typ záznamu hello obsahuje _CL, které není zadané ve skriptu hello.  Že přípona je automaticky připojením toohello protokolu typ tooindicate, že se jedná o typ vlastního protokolu.
4. Měli byste vidět jeden nebo více záznamů vrátil oznamující, že dané sady runbook hello funguje podle očekávání.


## <a name="6-publish-hello-runbook"></a>6. Publikovat hello runbook
Jakmile se ujistíte, že hello runbook správně funguje, je nutné toopublish ho, můžete ho spustit v produkčním prostředí.  Můžete pokračovat tooedit a otestování sady runbook hello beze změny hello publikovanou verzi.  

![Publikování sady runbook](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. Vrátí tooyour účet automation.
2. Klikněte na **Runbooky** a vyberte **shromažďování. automatizace úloh**.
3. Klikněte na tlačítko **upravit** a potom **publikování**.
4. Klikněte na tlačítko **Ano** při kladené tooverify, které chcete toooverwrite hello dřív publikovaná verze.

## <a name="7-set-logging-options"></a>7. Nastavení možností protokolování 
Pro test, měla mít tooview [podrobný výstup](../automation/automation-runbook-output-and-messages.md#message-streams) protože nastavit proměnnou hello $VerbosePreference ve skriptu hello.  Pro produkční prostředí je nutné vlastnosti tooset hello protokolování pro sady runbook hello, pokud chcete, aby tooview podrobný výstup.  Pro sadu runbook hello použili v tomto kurzu bude se zobrazovat data json hello odesílány tooLog Analytics.

![Protokolování a trasování](media/operations-management-suite-runbook-datacollect/logging.png)

1. Ve vlastnostech hello vaší sady runbook vyberte **protokolování a trasování** pod **nastavení sady Runbook**.
2. Změna nastavení hello pro **protokolování podrobných záznamů** příliš**na**.
3. Klikněte na **Uložit**.

## <a name="8-schedule-runbook"></a>8. Plánování runbooku
Hello nejběžnější způsob toostart sady runbook, která shromažďuje data monitorování je tooschedule ho toorun automaticky.  To uděláte tak, že vytvoříte [plán ve službě Azure Automation](../automation/automation-schedules.md) a připojíte ho tooyour runbook.

![Plánování runbooku](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. Ve vlastnostech hello vaší sady runbook, vyberte **plány** pod **prostředky**.
2. Klikněte na tlačítko **přidat plán** > **propojit plán tooyour runbook** > **vytvořte nový plán**.
5. Zadejte následující hodnoty pro hello plán a klikněte na tlačítko hello **vytvořit**.

| Vlastnost | Hodnota |
|:--|:--|
| Name (Název) | AutomationJobs-každou hodinu |
| Spustí | Vyberte libovolný čas posledních 5 minut hello aktuální čas. |
| Opakování | Opakování |
| Opakovat každých | 1 hodina |
| Sada vypršení platnosti | Ne |

Po vytvoření plánu hello musíte tooset hello parametr hodnoty, které se použije vždy, když tento plán spuštění sady runbook hello.

6. Klikněte na tlačítko **nakonfigurovat parametry a nastavení spouštění**.
7. Zadejte hodnoty pro vaše **ResourceGroupName** a **AutomationAccountName**.
8. Klikněte na **OK**. 

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Ověřte, sada runbook spustí podle plánu.
Při spuštění sady runbook [se vytvoří úloha](../automation/automation-runbook-execution.md) a jakéhokoli výstupu protokolována.  Ve skutečnosti jedná se o hello je shromažďování stejných úloh, které hello sady runbook.  Můžete ověřit, že dané sady runbook hello spustí podle očekávání kontrolou hello úlohy pro hello runbook po uplynutí hello čas zahájení pro plán hello.

![Úlohy](media/operations-management-suite-runbook-datacollect/jobs.png)

1. Ve vlastnostech hello vaší sady runbook, vyberte **úlohy** pod **prostředky**.
2. Měli byste vidět, že byla spuštěna v seznamu úloh pro každou sadu runbook hello čas.
3. Klikněte na jednu z úloh tooview hello její podrobnosti.
4. Klikněte na **všechny protokoly** tooview hello protokoly a výstup z runbooku hello.
5. Posuňte se toohello dolní toofind položky podobné toohello bitovou kopii níže.<br>![Verbose](media/operations-management-suite-runbook-datacollect/verbose.png)
6. Kliknutím na tuto položku tooview hello podrobná data json, který vám byl zaslán tooLog Analytics.



## <a name="next-steps"></a>Další kroky
- Použití [Návrhář zobrazení](../log-analytics/log-analytics-view-designer.md) toocreate zobrazení zobrazení hello dat, že jste shromážděna toohello úložiště analýzy protokolů.
- Balíček svoji sadu runbook v [řešení pro správu](operations-management-suite-solutions-creating.md) toodistribute toocustomers.
- Další informace o [analýzy protokolů](https://docs.microsoft.com/azure/log-analytics/).
- Další informace o [Azure Automation](https://docs.microsoft.com/azure/automation/).
- Další informace o hello [rozhraní API sady kolekcí dat protokolu HTTP](../log-analytics/log-analytics-data-collector-api.md).
