---
title: "aaaConnection prostředky ve službě Azure Automation | Microsoft Docs"
description: "Připojení prostředky ve službě Azure Automation obsahovat hello informace požadované tooconnect tooan externí služby nebo aplikace ze sady runbook nebo konfigurace DSC. Tento článek vysvětluje hello podrobnosti o připojení a jak toowork s nimi v textové a grafické vytváření."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: f0239017-5c66-4165-8cca-5dcb249b8091
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/13/2017
ms.author: magoedte; bwren
ms.openlocfilehash: f0f6b9fb960789b34af7b60eb1069313fdcf071c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connection-assets-in-azure-automation"></a>Assety připojení v Azure Automation.

Prostředek připojení automatizace obsahuje hello informace požadované tooconnect tooan externí služby nebo aplikace ze sady runbook nebo konfigurace DSC. To může zahrnovat informace požadované pro ověřování, jako je uživatelské jméno a heslo v přidání tooconnection informace, jako je adresa URL nebo portu. Hodnota Hello připojení všechny hello vlastnosti pro připojení tooa konkrétní aplikace v jeden prostředek jako názvem na rozdíl od toocreating je uchovávání více proměnných. uživatel Hello upravovat hello hodnoty pro připojení na jednom místě a předáte hello název sady runbook tooa připojení nebo konfigurace DSC v jeden parametr. Hello vlastnosti pro připojení je přístupná v runbooku hello nebo konfigurace DSC s hello **Get-AutomationConnection** aktivity.

Při vytváření připojení, je nutné zadat *typ připojení*. Typ připojení Hello je šablonu, která definuje sadu vlastností. připojení Hello definuje hodnoty pro každou vlastnost v jeho typu připojení. Typy připojení jsou přidané tooAzure automatizace v integračních modulech nebo vytvářené pomocí hello [rozhraní API služby Azure Automation](http://msdn.microsoft.com/library/azure/mt163818.aspx) Pokud modul integrace hello obsahuje typ připojení a je importovat do vašeho účtu Automation. Jinak budete potřebovat toocreate metadata souboru toospecify typ připojení Automation.  Další informace týkající se to najdete v tématu [moduly integrace](automation-integration-modules.md).  

>[!NOTE] 
>Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené v Azure Automation jednotlivých účtů automation pomocí jedinečný klíč, který se vygeneruje hello. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před uložením o zabezpečený prostředek, hello klíč pro účet automation hello se dešifruje pomocí hlavního certifikátu hello a pak se použije tooencrypt hello asset.

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell

Hello rutiny v následující tabulce hello jsou použité toocreate a spravovat připojení Automation pomocí prostředí Windows PowerShell. Se dodávají jako součást hello [modul Azure PowerShell](/powershell/azure/overview) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

|Rutina|Popis|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Načte připojení. Zahrnuje zatřiďovací tabulku s hodnotami hello hello připojení polí.|
|[Nové AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Vytvoří nové připojení.|
|[Odebrat AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Odeberte existující připojení.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Nastaví hello hodnotu konkrétního pole pro existující připojení.|

## <a name="activities"></a>Aktivity

Hello aktivity v následující tabulce hello jsou použité tooaccess připojení v runbooku nebo konfigurace DSC.

|Aktivity|Popis|
|---|---|
|[Get-AutomationConnection](/powershell/module/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Získá toouse připojení. Vrátí hodnotu hash tabulku s vlastnostmi hello hello připojení.|

>[!NOTE] 
>Byste neměli používat proměnné s hello – název parametru **Get - AutomationConnection** vzhledem k tomu, že to může zkomplikovat zjišťování závislostí mezi runbooky nebo konfigurace DSC a prostředky připojení v době návrhu.

## <a name="creating-a-new-connection"></a>Vytvoření nového připojení

### <a name="toocreate-a-new-connection-with-hello-azure-portal"></a>toocreate nové připojení s hello portálu Azure

1. Z vašeho účtu automation, klikněte na tlačítko hello **prostředky** část tooopen hello **prostředky** okno.
2. Klikněte na tlačítko hello **připojení** část tooopen hello **připojení** okno.
3. Klikněte na tlačítko **přidat připojení** hello horní části okna hello.
4. V hello **typ** rozevíracího seznamu, vyberte hello typ připojení chcete toocreate. Formulář Hello nabídne hello vlastnosti pro daný typ.
5. Vyplňte formulář hello a klikněte na tlačítko **vytvořit** toosave hello nové připojení.

### <a name="toocreate-a-new-connection-with-hello-azure-classic-portal"></a>toocreate nové připojení s hello portál Azure classic

1. Z vašeho účtu automation, klikněte na tlačítko **prostředky** hello horní části okna hello.
2. V dolní části hello hello okna, klikněte na tlačítko **přidat nastavení**.
3. Klikněte na tlačítko **přidat připojení**.
4. V hello **typ připojení** rozevíracího seznamu, vyberte hello typ připojení chcete toocreate.  Průvodce Hello nabídne hello vlastnosti pro daný typ.
5. Dokončete Průvodce hello a klikněte na tlačítko hello políčko toosave hello nové připojení.

### <a name="toocreate-a-new-connection-with-windows-powershell"></a>toocreate nového připojení pomocí prostředí Windows PowerShell

Vytvoření nového připojení pomocí prostředí Windows PowerShell pomocí hello [New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) rutiny. Tato rutina má parametr s názvem **ConnectionFieldValues** , očekává [zatřiďovací tabulku](http://technet.microsoft.com/library/hh847780.aspx) definování hodnoty pro každé z vlastností hello definované hello typ připojení.

Pokud jste se seznámili s hello automatizace [účet Spustit jako](automation-sec-configure-azure-runas-account.md) tooauthenticate sady runbook pomocí hello instančního objektu, hello skript prostředí PowerShell, zadaný jako hello hello alternativní toocreating účet Spustit jako z portálu hello Vytvoří nový prostředek připojení pomocí následující ukázkové příkazy hello.  

    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues 

Jsou možné toouse hello skriptu toocreate hello prostředek připojení, protože při vytváření účtu Automation, automaticky obsahuje několik globální modulů ve výchozím nastavení společně s typ připojení hello **AzurServicePrincipal**toocreate hello **AzureRunAsConnection** asset připojení.  Je to důležité tookeep na paměti, proto když zkusíte toocreate nové připojení asset tooconnect tooa služby nebo aplikace se shodovat metody ověřování, se nezdaří vzhledem k tomu, že typ připojení hello není již definována v účtu Automation.  Další informace o tom, jak toocreate vlastní připojení typ pro vlastní nebo modul z hello [Galerie prostředí PowerShell](https://www.powershellgallery.com), najdete v části [moduly integrace](automation-integration-modules.md)
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Použití připojení v runbooku nebo konfigurace DSC

Načtení připojení v runbooku nebo konfigurace DSC s hello **Get-AutomationConnection** rutiny.  Nemůžete použít hello [Get-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/resourcemanager/azurerm.automation/v1.0.12/Get-AzureRmAutomationConnection?redirectedfrom=msdn) aktivity.  Tato aktivita načítá hodnoty hello hello různých polí v hello připojení a vrací je jako [zatřiďovací tabulku](http://go.microsoft.com/fwlink/?LinkID=324844) který pak může být použit s příslušnými příkazy hello hello runbook nebo konfigurace DSC.

### <a name="textual-runbook-sample"></a>Ukázkový textový

Hello následující vzorové příkazy ukazují, jak toouse hello účet Spustit jako už zmínili dřív, tooauthenticate s prostředky Azure Resource Manager ve vašem runbooku.  Používá připojení hello asset představující hello účet Spustit jako, který odkazuje na objektu hello služby založené na certifikátech, nejsou přihlašovací údaje.  

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint 

### <a name="graphical-runbook-samples"></a>Ukázky grafický runbook

Přidání **Get-AutomationConnection** aktivity tooa grafický runbook kliknutím pravým tlačítkem na připojení hello v podokně knihovna hello grafický editor hello a výběr **přidat toocanvas**.

![](media/automation-connections/connection-add-canvas.png)

Hello následující obrázek ukazuje příklad použití připojení v grafický runbook.  Toto je hello stejné příkladu výše pro ověřování pomocí účtu spustit jako hello textový.  Tento příklad používá hello **konstantní hodnota** sadu dat pro hello **získat připojení spustit jako** aktivity, která používá objekt připojení pro ověřování.  A [propojení kanálu](automation-graphical-authoring-intro.md#links-and-workflow) zde slouží vzhledem k tomu, že hello ServicePrincipalCertificate sada parametrů je očekáván jeden objekt.

![](media/automation-connections/automation-get-connection-object.png)

## <a name="next-steps"></a>Další kroky

- Zkontrolujte [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow) toounderstand jak hello toodirect a řízení toku logiky ve vašich sadách runbook.  

- toolearn Další informace o použití Azure Automation moduly Powershellu a osvědčené postupy pro vytvoření vlastní toowork moduly prostředí PowerShell jako moduly integrace v rámci Azure Automation najdete v části [moduly integrace](automation-integration-modules.md).  
