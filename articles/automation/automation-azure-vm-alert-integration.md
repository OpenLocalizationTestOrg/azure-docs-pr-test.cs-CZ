---
title: "AAA\"Opravit výstrahy virtuálního počítače Azure pomocí Runbooků Automation | Microsoft Docs\""
description: "Tento článek ukazuje, jak výstrahy toointegrate virtuální počítač Azure s Azure Automation runbook a automatické odstranění problémů"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 1f7baa7f-7283-4a4f-9385-3f5cd1062c7f
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2016
ms.author: csand;magoedte
ms.openlocfilehash: c226368a5c4c51fbfb331f4b97f7f2f239e701c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---remediate-azure-vm-alerts"></a>Scénář automatizace Azure – opravit virtuální počítač Azure výstrahy
Azure Automation a virtuální počítače Azure vydali novou funkci, umožní vám tooconfigure runbooků služeb automatizace toorun virtuálního počítače (VM) výstrahy. Díky této nové funkci můžete tooautomatically nápravy standardní ve výstrahách tooVM odpovědi, jako je restartování nebo hello zastavení virtuálního počítače.

Dříve, při vytváření virtuálních počítačů pravidlo výstrahy bylo možné příliš[zadejte webhook automatizace](https://azure.microsoft.com/blog/using-azure-automation-to-take-actions-on-azure-alerts/) tooa runbook v pořadí toorun hello runbook vždy, když se aktivuje výstraha hello. Ale to je nutné toodo hello práci při vytváření sady runbook hello, vytvoření webhooku hello hello sady runbook a pak kopírování a vkládání hello webhooku během vytváření pravidlo výstrahy. V této nové verzi není hello proces je mnohem jednodušší, protože sady runbook můžete přímo vybrat ze seznamu během vytváření pravidlo výstrahy, a můžete zvolit účet Automation, který bude spuštění sady runbook hello nebo snadno vytvořit účet.

V tomto článku jsme se ukazují, jak je snadné tooset až výstrahu virtuální počítač Azure a nakonfigurovat toorun automatizace sady runbook vždy, když se aktivuje výstraha hello. Příklad scénáře zahrnují restartování virtuálního počítače, pokud využití paměti hello překročí některé prahovou hodnotu z důvodu tooan aplikace na hello virtuálních počítačů s nevrácenou pamětí nebo zastavení virtuálního počítače, jestliže uživatelského času procesoru hello byl nižší než 1 % pro poslední hodinu a není používán. Dále vysvětlíme, jak hello automatizovat vytvoření objektu služby ve vašem Automation účet zjednodušuje použití hello sad runbook v Azure výstrahy nápravy.

## <a name="create-an-alert-on-a-vm"></a>Vytvořit výstrahu na virtuálním počítači
Proveďte následující kroky tooconfigure výstrahy toolaunch sady runbook při jeho prahová hodnota byla splněna hello.

> [!NOTE]
> V této verzi jsme podporují jenom V2 virtuální počítače a podporu pro classic, který bude brzy přidat virtuální počítače.  
> 
> 

1. Přihlaste se toohello portál Azure a klikněte na tlačítko **virtuální počítače**.  
2. Vyberte jednu z vašich virtuálních počítačů.  Hello okno řídicího panelu virtuální počítač se zobrazí a hello **nastavení** okno tooits vpravo.  
3. Z hello **nastavení** okno, v části hello vyberte části monitorování **výstrah pravidla**.
4. Na hello **výstrah pravidla** okně klikněte na tlačítko **přidat upozornění**.

Otevře se hello **přidání pravidla výstrahy** okno, kde můžete nakonfigurovat podmínky hello hello výstrahy a volit z jednoho nebo všech z těchto možností: Odeslat toosomeone e-mailu, použít systém výstrah tooanother webhooku tooforward hello, nebo Spusťte runbook služby automatizace v odpovědi pokus o tooremediate hello problém.

## <a name="configure-a-runbook"></a>Konfigurace sady runbook
Vyberte tooconfigure runbook toorun při splnění prahové hodnoty pro výstrahu virtuálních počítačů hello **sady Automation Runbook**. V hello **konfigurace runbook** okno, můžete vybrat hello runbook toorun a hello hello runbook toorun účet Automation v.

![Nakonfigurujte runbook služby automatizace a vytvořte nový účet Automation.](media/automation-azure-vm-alert-integration/ConfigureRunbookNewAccount.png)

> [!NOTE]
> Pro tuto verzi můžete zvolit ze tří sady runbook, které nabízí služba hello – restartování virtuálního počítače, zastavte virtuální počítač nebo odeberte virtuální počítač (odstranit).  Hello možnost tooselect jiné sady runbook nebo jeden z vlastní sady runbook bude k dispozici v budoucí verzi.
> 
> 

![Sady Runbook toochoose z](media/automation-azure-vm-alert-integration/RunbooksToChoose.png)

Po výběru hello tři dostupné sady runbook hello **účet Automation** rozevíracím seznamu se zobrazí a vyberete automation bude runbook hello účet Spustit jako. Runbooky potřebují toorun v kontextu hello [účet Automation](automation-security-overview.md) se ve vašem předplatném Azure. Můžete vybrat účet Automation, zda jste již vytvořili, nebo máte vytvořený pro vás nový účet automatizace.

Hello sady runbook, které jsou k dispozici ověřit tooAzure používání objektu služby. Pokud si zvolíte toorun hello runbook v jednom z vaší existující účty Automation, automaticky vytvoříme hello služby hlavní za vás. Pokud si zvolíte toocreate nového účtu Automation, pak automaticky vytvoříme účet hello a hello instanční objekt. V obou případech dva prostředky bude vytvořen také v hello účet Automation – certifikát asset s názvem **AzureRunAsCertificate** a připojení asset s názvem **AzureRunAsConnection**. sady runbook Hello použije **AzureRunAsConnection** tooauthenticate s Azure v pořadí tooperform hello správu akci vůči hello virtuálních počítačů.

> [!NOTE]
> Hello instanční objekt se vytvoří v oboru hello předplatného a je přiřazena role Přispěvatel hello. Tato role je nutná pro hello účet toohave oprávnění toorun automatizace sady runbook toomanage virtuálních počítačích Azure.  Vytvoření Hello účtu Automaton nebo instančního objektu je jednorázové události. Po jejich vytvoření, můžete tento účet toorun runbooky pro jiné výstrahy virtuálního počítače Azure.
> 
> 

Když kliknete na tlačítko **OK** hello výstraha je nakonfigurován, a pokud jste vybrali možnost toocreate hello nového účtu Automation, vytvoří se společně s hello instanční objekt.  Může to trvat několik sekund toocomplete.  

![Konfigurace sady Runbook](media/automation-azure-vm-alert-integration/RunbookBeingConfigured.png)

Po dokončení konfigurace hello uvidíte hello název sady runbook hello se zobrazí v hello **přidání pravidla výstrahy** okno.

![Runbook nakonfigurovat](media/automation-azure-vm-alert-integration/RunbookConfigured.png)

Klikněte na tlačítko **OK** v hello **přidání pravidla výstrahy** okno a hello pravidlo výstrahy se vytvoří a aktivovat, pokud je hello virtuální počítač ve spuštěném stavu.

### <a name="enable-or-disable-a-runbook"></a>Povolit nebo zakázat sady runbook
Pokud je runbook nakonfigurovaný pro výstrahy, ji můžete vypnout bez odebrání konfigurace nástroje runbook hello. To vám umožní tookeep hello výstraha spuštěná a možná testování hello pravidla výstrah a později znovu povolit hello runbook.

## <a name="create-a-runbook-that-works-with-an-azure-alert"></a>Vytvoření sady runbook, který funguje s Azure výstrahy
Když zvolíte sady runbook v rámci Azure pravidla výstrahy, hello runbook musí hello toomanage výstrahy se data, která je předána tooit toohave logiku v ní.  Pokud sada runbook je nakonfigurován v pravidlo výstrahy, webhook, jehož je vytvořena pro sadu runbook hello; Tento webhook je pak použít toostart hello runbook každé čas hello výstrahy aktivační události.  Hello vlastní volání toostart hello runbook je adresu URL webhooku toohello požadavek HTTP POST. Hello text požadavku POST hello obsahuje objekt uvedena ve správném formátu JSON, který obsahuje užitečné vlastnosti toohello související výstrahy.  Jak vidíte níže, dat výstrah hello obsahuje podrobnosti, třeba ID předplatného, název skupiny prostředků, resourceName a typ prostředku.

### <a name="example-of-alert-data"></a>Příklad dat výstrah
```
{
    "WebhookName": "AzureAlertTest",
    "RequestBody": "{
    \"status\":\"Activated\",
    \"context\": {
        \"id\":\"/subscriptions/<subscriptionId>/resourceGroups/MyResourceGroup/providers/microsoft.insights/alertrules/AlertTest\",
        \"name\":\"AlertTest\",
        \"description\":\"\",
        \"condition\": {
            \"metricName\":\"CPU percentage guest OS\",
            \"metricUnit\":\"Percent\",
            \"metricValue\":\"4.26337916666667\",
            \"threshold\":\"1\",
            \"windowSize\":\"60\",
            \"timeAggregation\":\"Average\",
            \"operator\":\"GreaterThan\"},
        \"subscriptionId\":\<subscriptionID> \",
        \"resourceGroupName\":\"TestResourceGroup\",
        \"timestamp\":\"2016-04-24T23:19:50.1440170Z\",
        \"resourceName\":\"TestVM\",
        \"resourceType\":\"microsoft.compute/virtualmachines\",
        \"resourceRegion\":\"westus\",
        \"resourceId\":\"/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\",
        \"portalLink\":\"https://portal.azure.com/#resource/subscriptions/<subscriptionId>/resourceGroups/TestResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM\"
        },
    \"properties\":{}
    }",
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "<webhookURL>"
    }
}
```

Přijetí hello webhook služby automatizace hello HTTP POST extrahuje hello dat výstrah a předává je toohello runbook ve vstupní parametr runbooku WebhookData hello.  Níže je ukázkové sady runbook, který ukazuje, jak toouse hello WebhookData parametr a extrahování dat výstrah hello a použít ho toomanage hello prostředků Azure, který aktivoval výstrahu hello.

### <a name="example-runbook"></a>Ukázkový runbook
```
#  This runbook will restart an ARM (V2) VM in response tooan Azure VM alert.

[OutputType("PSAzureOperationResponse")]

param ( [object] $WebhookData )

if ($WebhookData)
{
    # Get hello data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Assure that hello alert status is 'Activated' (alert condition went from false tootrue)
    # and not 'Resolved' (alert condition went from true toofalse)
    if ($WebhookBody.status -eq "Activated")
    {
        # Get hello info needed tooidentify hello VM
        $AlertContext = [object] $WebhookBody.context
        $ResourceName = $AlertContext.resourceName
        $ResourceType = $AlertContext.resourceType
        $ResourceGroupName = $AlertContext.resourceGroupName
        $SubId = $AlertContext.subscriptionId

        # Assure that this is hello expected resource type
        Write-Verbose "ResourceType: $ResourceType"
        if ($ResourceType -eq "microsoft.compute/virtualmachines")
        {
            # This is an ARM (V2) VM

            # Authenticate tooAzure with service principal and certificate
            $ConnectionAssetName = "AzureRunAsConnection"
            $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null) {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in hello Automation account."
            }
            Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
            Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

            # Restart hello VM
            Restart-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName
        } else {
            Write-Error "$ResourceType is not a supported resource type for this runbook."
        }
    } else {
        # hello alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $WebhookBody.status)
    }
} else {
    Write-Error "This runbook is meant toobe started from an Azure alert only."
}
```

## <a name="summary"></a>Souhrn
Když konfigurujete výstrahu na virtuální počítač Azure, teď máte tooeasily hello možnost konfigurace Automation runbook tooautomatically provést akci nápravy, která při hello výstraha aktivuje. Pro tuto verzi můžete si vybrat z toorestart sady runbook, zastavení nebo odstranit virtuální počítač v závislosti na vašem scénáři výstrahy. Toto je pouze hello začátku povolení scénáře, kde můžete řídit akce hello (oznámení, řešení potíží s nápravy), které se mají provést automaticky, když se aktivuje výstrahu.

## <a name="next-steps"></a>Další kroky
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md)
* toolearn Další informace o typech runbooků, jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md)

