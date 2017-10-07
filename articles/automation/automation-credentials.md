---
title: "aaaCredential prostředky ve službě Azure Automation | Microsoft Docs"
description: "Prostředků přihlašovacích údajů ve službě Azure Automation obsahovat zabezpečovací pověření, které se dají použít tooauthenticate tooresources přístup hello runbook nebo konfigurace DSC. Tento článek popisuje, jak toocreate assety přihlašovacích údajů a použít na sady runbook nebo konfigurace DSC."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 3209bf73-c208-425e-82b6-df49860546dd
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: bwren
ms.openlocfilehash: 46f23a8f79d5863265af9cf84f6003e30f8e7d39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="credential-assets-in-azure-automation"></a>Prostředků přihlašovacích údajů ve službě Azure Automation
Obsahuje prostředek přihlašovacích údajů automatizace [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objekt, který obsahuje zabezpečovací přihlašovací údaje, jako je například uživatelské jméno a heslo. Konfigurace Runbooků a DSC může použít rutiny přijmout objekt PSCredential pro ověřování, nebo se může extrahuje hello uživatelské jméno a heslo hello PSCredential objekt tooprovide toosome aplikace nebo služby, které vyžadují ověřování. Hello vlastnosti přihlašovacích údajů jsou bezpečně uloženy ve službě Azure Automation a je přístupný v runbooku hello nebo konfigurace DSC s hello [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) aktivity.

> [!NOTE]
> Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené v Azure Automation jednotlivých účtů automation pomocí jedinečný klíč, který se vygeneruje hello. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před uložením o zabezpečený prostředek, hello klíč pro účet automation hello se dešifruje pomocí hlavního certifikátu hello a pak se použije tooencrypt hello asset.  

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell
Hello rutiny v následující tabulce hello jsou použité toocreate a Správa prostředků přihlašovacích údajů automatizace v prostředí Windows PowerShell.  Se dodávají jako součást hello [modul Azure PowerShell](/powershell/azure/overview) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

| Rutiny | Popis |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Načte informace o asset přihlašovacích údajů. Můžete načíst pouze hello přihlašovacích údajů, sám sebe z **Get-AutomationPSCredential** aktivity. |
| [Nové AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Vytvoří nových přihlašovacích údajů automatizace. |
| [Remove - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Odebere pověření automatizace. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Nastaví hello vlastnosti pro existující automatizace pověření. |

## <a name="runbook-activities"></a>Aktivity sady Runbook
Hello aktivity v následující tabulce hello jsou přihlašovací údaje použité tooaccess v sadě runbook a konfigurace DSC.

| Aktivity | Popis |
|:--- |:--- |
| Get-AutomationPSCredential |Získá toouse přihlašovacích údajů v runbooku nebo konfigurace DSC. Vrátí [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objektu. |

> [!NOTE]
> Byste neměli používat proměnné v hello – název parametr Get-AutomationPSCredential, protože to může zkomplikovat zjišťování závislostí mezi runbooky a konfigurace DSC a assety přihlašovacích údajů v době návrhu.
> 
> 

## <a name="creating-a-new-credential-asset"></a>Vytvoření nového prostředku přihlašovacích údajů

### <a name="toocreate-a-new-credential-asset-with-hello-azure-portal"></a>toocreate nový asset přihlašovacích údajů s hello portálu Azure
1. Z vašeho účtu automation, klikněte na tlačítko hello **prostředky** část tooopen hello **prostředky** okno.
2. Klikněte na tlačítko hello **pověření** část tooopen hello **pověření** okno.
3. Klikněte na tlačítko **přidat pověření** hello horní části okna hello.
4. Vyplňte formulář hello a klikněte na tlačítko **vytvořit** toosave hello nových přihlašovacích údajů.

### <a name="toocreate-a-new-credential-asset-with-windows-powershell"></a>toocreate nový asset přihlašovacích údajů v prostředí Windows PowerShell
Hello následující vzorové příkazy ukazují, jak přihlašovací údaje toocreate nové automatizace. Objekt PSCredential prvním vytvoření hello jméno a heslo a pak se použije asset přihlašovacích údajů toocreate hello. Alternativně můžete použít hello **Get-Credential** rutiny toobe výzva tootype jméno a heslo.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

### <a name="toocreate-a-new-credential-asset-with-hello-azure-classic-portal"></a>toocreate nový asset přihlašovacích údajů s hello portál Azure classic
1. Z vašeho účtu automation, klikněte na tlačítko **prostředky** hello horní části okna hello.
2. V dolní části hello hello okna, klikněte na tlačítko **přidat nastavení**.
3. Klikněte na tlačítko **přidat přihlašovací údaje**.
4. V hello **typ přihlašovacích údajů** rozevíracího seznamu vyberte **přihlašovacích údajů prostředí PowerShell**.
5. Dokončete Průvodce hello a klikněte na tlačítko hello políčko toosave hello nových přihlašovacích údajů.

## <a name="using-a-powershell-credential"></a>Pomocí přihlašovacích údajů prostředí PowerShell
Získat prostředek přihlašovacích údajů v runbooku nebo konfigurace DSC s hello **Get-AutomationPSCredential** aktivity. Tento příkaz vrátí [objektu PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) , můžete použít s aktivitu nebo rutinu, která vyžaduje parametr PSCredential. Můžete také načíst vlastnosti hello hello přihlašovací údaje objektu toouse jednotlivě. Hello objekt má vlastnost hello uživatelské jméno a heslo zabezpečené hello nebo můžete použít hello **GetNetworkCredential** metoda tooreturn [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) objekt, který bude poskytovat zabezpečená verze hello heslo.

### <a name="textual-runbook-sample"></a>Ukázkový textový
Hello následující vzorové příkazy ukazují, jak přihlašovací údaje toouse prostředí PowerShell v runbooku. V tomto příkladu jsou načtena hello pověření a toovariables přiřazené jeho uživatelské jméno a heslo.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Ukázka grafický runbook
Přidání **Get-AutomationPSCredential** aktivity tooa grafický runbook kliknutím pravým tlačítkem na hello přihlašovacích údajů v podokně knihovna hello grafický editor hello a výběrem **přidat toocanvas**.

![Přidat toocanvas přihlašovacích údajů](media/automation-credentials/credential-add-canvas.png)

Hello následující obrázek znázorňuje příklad použití pověření v grafický runbook.  V takovém případě ji se použité tooprovide ověřování pro prostředky tooAzure runbook jak je popsáno v [ověření Runbooků pomocí účtu uživatele Azure AD](automation-create-aduser-account.md).  první aktivitu Hello načte hello přihlašovacích údajů, který má přístup toohello předplatného Azure.  Hello **Add-AzureAccount** aktivita pak používá toto pověření tooprovide ověřování pro všechny aktivity, které následují po.  A [propojení kanálu](automation-graphical-authoring-intro.md#links-and-workflow) se zde od **Get-AutomationPSCredential** je očekáván jeden objekt.  

![Přidat toocanvas přihlašovacích údajů](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Pomocí přihlašovacích údajů prostředí PowerShell v DSC
Během konfigurace DSC ve službě Azure Automation, můžete odkazovat pomocí prostředků přihlašovacích údajů **Get-AutomationPSCredential**, prostředků přihlašovacích údajů můžete také být předán prostřednictvím parametrů, v případě potřeby. Další informace najdete v tématu [kompilování konfigurace v Azure Automation DSC](automation-dsc-compile.md#credential-assets).

## <a name="next-steps"></a>Další kroky
* toolearn Další informace o propojení v vytváření grafického obsahu, najdete v části [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow)
* toounderstand hello různé metody ověřování pomocí automatizace, najdete v části [zabezpečení automatizace Azure](automation-security-overview.md)
* tooget kroky s grafickými runbooky najdete v části [Můj první grafický runbook](automation-first-runbook-graphical.md)
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md) 

