---
title: "aaaIntegrate Azure Automation se správa zdrojového kódu Visual Stuido Team Services | Microsoft Docs"
description: "Scénář vás provede procesem nastavení integrace s účet Azure Automation a zdrojového kódu Visual Stuido Team Services."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: "služby VSTS, Azure powershell zdrojového kódu automatizace"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 8f6faa596a5ad1f8b72e820ca320b3e103d83579
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation scénář – integrace ovládacích prvků zdrojového automatizace s Visual Studio Team Services

V tomto scénáři máte projekt Visual Studio Team Services, že používáte toomanage Azure Automation runbook nebo konfigurace DSC ve správě zdrojového kódu.
Tento článek popisuje, jak toointegrate služby VSTS s vaším prostředím Azure Automation, se stane, že průběžnou integraci pro každý vrácení se změnami.

## <a name="getting-hello-scenario"></a>Získávání scénář hello

Tento scénář se skládá ze dvou Powershellové runbooky, které můžete importovat přímo z hello [Galerie Runbooků](automation-runbook-gallery.md) v hello portál Azure nebo stáhnout z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooky

Runbook | Popis| 
--------|------------|
Služby synchronizace VSTS | Import sady runbook nebo konfigurace z služby VSTS zdrojového kódu, pokud se provádí vrácení se změnami. Je-li spustit ručně, bude import a publikovat všechny sady runbook nebo konfigurace do hello účet Automation.| 
Synchronizace VSTSGit | Import sady runbook nebo konfigurace ze služby VSTS ve správě zdrojového Git Pokud se provádí vrácení se změnami. Je-li spustit ručně, bude import a publikovat všechny sady runbook nebo konfigurace do hello účet Automation.|

### <a name="variables"></a>Proměnné

Proměnná | Popis|
-----------|------------|
VSToken | Zabezpečte variabilní prostředek, které vytvoříte, který obsahuje hello služby VSTS osobní přístupový token. Zjistěte, jak toocreate služby VSTS osobní přístup k tokenu na hello [stránka ověřování služby VSTS](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>Instalace a konfigurace tohoto scénáře

Vytvoření [osobní přístupový token](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) v služby VSTS do vašeho účtu automation bude používat sady runbook hello toosync nebo konfigurace.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Vytvoření [zabezpečené proměnná](automation-variables.md) ve vašem hello toohold účet automation osobní přístupový token, aby se hello runbook ověřit tooVSTS a synchronizace sad runbook hello nebo konfigurace do hello účet Automation. Tato VSToken můžete pojmenovat. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Import hello sady runbook, která se synchronizují konfigurace do účtu automation hello nebo sady runbook. Můžete použít hello [ukázkové sady runbook služby VSTS](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) nebo hello [služby VSTS s Git ukázkové sady runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) z hello PowerShellGallery.com podle toho, pokud používáte služby VSTS Zdroj služby VSTS s Gitem nebo ovládací prvek a nasaďte tooyour účet automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Teď můžete [publikování](automation-creating-importing-runbook.md#publishing-a-runbook) této sady runbook, takže si můžete vytvořit webhooku. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Vytvoření [webhooku](automation-webhooks.md) pro tento runbook služby synchronizace VSTS a vyplňte hello parametry, jak je uvedeno níže. Ujistěte se, že zkopírujete adresa url webhooku hello, jako je nutné pro služby háku v služby VSTS. Hello VSAccessTokenVariableName je název hello (VSToken) hello zabezpečené proměnné, které jste vytvořili starší toohold hello osobní přístupový token. 

Integrace s služby VSTS (synchronizace-VSTS.ps1) bude trvat hello následující parametry.
### <a name="sync-vsts-parameters"></a>Služby synchronizace VSTS parametry

Parametr | Popis| 
--------|------------|
WebhookData | To bude obsahovat hello vrácení se změnami informace odesílané ze háku služby VSTS hello. Tento parametr by měl ponechat prázdné.| 
ResourceGroup | Toto je hello název skupiny prostředků hello, zda má účet automation hello v.|
AutomationAccountName | Název Hello hello účet automation, který bude synchronizovat s služby VSTS.|
VSFolder | Název složky hello v služby VSTS existuje hello sady runbook a konfigurace.|
VSAccount | Název Hello hello účet Visual Studio Team Services.| 
VSAccessTokenVariableName | Název Hello hello zabezpečené proměnné (VSToken) obsahující hello služby VSTS osobní přístupový token.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Pokud používáte služby VSTS s GITEM (synchronizace-VSTSGit.ps1) bude trvat hello následující parametry.

Parametr | Popis|
--------|------------|
WebhookData | To bude obsahovat hello vrácení se změnami informace odesílané ze háku služby VSTS hello. Tento parametr by měl ponechat prázdné.| ResourceGroup | Název skupiny prostředků hello, zda má účet automation hello v této hello.|
AutomationAccountName | Název Hello hello účet automation, který bude synchronizovat s služby VSTS.|
VSAccount | Název Hello hello účet Visual Studio Team Services.|
VSProject | Název Hello hello projekt v služby VSTS existuje hello sady runbook a konfigurace.|
GitRepo | Název Hello hello úložiště Git.|
GitBranch | Název Hello hello větve v úložišti služby VSTS Git.|
Složka | Hello název složky hello v Git služby VSTS větev.|
VSAccessTokenVariableName | Název Hello hello zabezpečené proměnné (VSToken) obsahující hello služby VSTS osobní přístupový token.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Vytvoření služby háku v služby VSTS pro vrácení se změnami toohello složku, která aktivuje tento webhook na změnami kódu. Vyberte nástrojů Web Hooks jako toointegrate hello služby se při vytváření nové předplatné. Další informace o službách v [dokumentace služby VSTS služeb Jenkins](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Musí být schopný toodo všechny vrácení se změnami runbooky a konfiguraci do služby VSTS a automaticky se tyto teď synchronizovat měl do vašeho účtu automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Pokud ručně spuštění této sady runbook bez se aktivuje pomocí služby VSTS parametr webhookdata hello můžete nechat prázdné a provede úplnou synchronizaci ze složky služby VSTS hello zadán.

Pokud chcete toouninstall hello scénář, odebrání služby VSTS hello služby háku, odstranit hello runbook a proměnnou VSToken hello.
