---
title: "Assety přihlašovacích údajů ve službě Azure Automation | Microsoft Docs"
description: "Prostředků přihlašovacích údajů ve službě Azure Automation obsahovat zabezpečovací pověření, která můžete použít k ověřování k prostředkům přístup sada runbook nebo konfigurace DSC. Tento článek popisuje, jak vytvořit prostředků přihlašovacích údajů a použít je v sady runbook nebo konfigurace DSC."
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
ms.openlocfilehash: e2857515f3842a875ef7b5a9327392818931168f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="credential-assets-in-azure-automation"></a>Prostředků přihlašovacích údajů ve službě Azure Automation
Obsahuje prostředek přihlašovacích údajů automatizace [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objekt, který obsahuje zabezpečovací přihlašovací údaje, jako je například uživatelské jméno a heslo. Konfigurace Runbooků a DSC může použít rutiny přijmout objekt PSCredential pro ověřování, nebo se může extrahuje uživatelské jméno a heslo objektu PSCredential zajistit některé aplikace nebo služby, které vyžadují ověřování. Vlastnosti přihlašovacích údajů jsou bezpečně uloženy ve službě Azure Automation a je přístupný v sada runbook nebo konfigurace DSC s [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) aktivity.

> [!NOTE]
> Zabezpečené prostředky ve službě Azure Automation zahrnovat přihlašovací údaje, připojení, certifikátů a zašifrované proměnné. Tyto prostředky jsou zašifrovány a uložené ve službě Azure Automation pomocí jedinečný klíč, který se vygeneruje pro každý účet automation. Tento klíč se šifruje pomocí hlavního certifikátu a uloží ve službě Azure Automation. Před ukládání o zabezpečený prostředek, klíč pro účet služby automation jsou dešifrována pomocí hlavního certifikátu a pak se použije k zašifrování asset.  

## <a name="windows-powershell-cmdlets"></a>Rutiny prostředí Windows PowerShell
Rutiny v následující tabulce se používají k vytváření a správě prostředků přihlašovacích údajů automatizace v prostředí Windows PowerShell.  Se dodávají jako součást [modul Azure PowerShell](/powershell/azure/overview) která je k dispozici pro použití v runbooků služeb automatizace a konfigurace DSC.

| Rutiny | Popis |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Načte informace o asset přihlašovacích údajů. Pouze můžete načíst přihlašovací údaje sám sebe z **Get-AutomationPSCredential** aktivity. |
| [Nové AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Vytvoří nových přihlašovacích údajů automatizace. |
| [Remove - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Odebere pověření automatizace. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Nastaví vlastnosti pro existující automatizace pověření. |

## <a name="runbook-activities"></a>Aktivity sady Runbook
Aktivity v následující tabulce se používají pro přístup k přihlašovací údaje v runbooku a konfigurace DSC.

| Aktivity | Popis |
|:--- |:--- |
| Get-AutomationPSCredential |Získá pověření pro použití v runbooku nebo konfigurace DSC. Vrátí [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objektu. |

> [!NOTE]
> Byste neměli používat proměnné v parametru – Name Get-AutomationPSCredential, protože to může zkomplikovat zjišťování závislostí mezi runbooky a konfigurace DSC a assety přihlašovacích údajů v době návrhu.
> 
> 

## <a name="creating-a-new-credential-asset"></a>Vytvoření nového prostředku přihlašovacích údajů

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Chcete-li vytvořit nový prostředek přihlašovacích údajů pomocí portálu Azure
1. Z vašeho účtu automation, klikněte na tlačítko **prostředky** část otevřete **prostředky** okno.
2. Klikněte na tlačítko **pověření** část otevřete **pověření** okno.
3. Klikněte na tlačítko **přidat pověření** v horní části okna.
4. Vyplňte formulář a klikněte na tlačítko **vytvořit** pro uložení nových přihlašovacích údajů.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Chcete-li vytvořit nový prostředek přihlašovacích údajů v prostředí Windows PowerShell
Následující vzorové příkazy znázorňují postup vytvoření nových přihlašovacích údajů automatizace. Objekt PSCredential, je prvním vytvoření jméno a heslo a pak použít k vytvoření asset přihlašovacích údajů. Alternativně můžete použít **Get-Credential** rutiny se výzva k zadání jména a hesla.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

### <a name="to-create-a-new-credential-asset-with-the-azure-classic-portal"></a>Chcete-li vytvořit nový prostředek přihlašovacích údajů pomocí portálu Azure classic
1. Z vašeho účtu automation, klikněte na tlačítko **prostředky** v horní části okna.
2. V dolní části okna klikněte na tlačítko **přidat nastavení**.
3. Klikněte na tlačítko **přidat přihlašovací údaje**.
4. V **typ přihlašovacích údajů** rozevíracího seznamu vyberte **přihlašovacích údajů prostředí PowerShell**.
5. Dokončete průvodce a klikněte na zaškrtávací políčko pro uložení nových přihlašovacích údajů.

## <a name="using-a-powershell-credential"></a>Pomocí přihlašovacích údajů prostředí PowerShell
Získat prostředek přihlašovacích údajů v runbooku nebo konfigurace DSC s **Get-AutomationPSCredential** aktivity. Tento příkaz vrátí [objektu PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) , můžete použít s aktivitu nebo rutinu, která vyžaduje parametr PSCredential. Můžete také načíst vlastnosti objekt přihlašovacích údajů, který chcete použít jednotlivě. Objekt nemá vlastnost pro uživatelské jméno a heslo zabezpečené, nebo můžete použít **GetNetworkCredential** metoda vrátí [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) objekt, který bude poskytovat zabezpečená verzi heslo.

### <a name="textual-runbook-sample"></a>Ukázkový textový
Následující vzorové příkazy znázorňují postup použití přihlašovacích údajů prostředí PowerShell v runbooku. V tomto příkladu se načte přihlašovací údaje a jeho uživatelské jméno a heslo přiřazené k proměnné.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Ukázka grafický runbook
Přidání **Get-AutomationPSCredential** aktivitu grafický runbook pravým tlačítkem myši na přihlašovací údaje v podokně knihovna grafického editoru a výběrem **přidat na plátno**.

![Přidat přihlašovací údaje na plátno](media/automation-credentials/credential-add-canvas.png)

Následující obrázek ukazuje příklad použití pověření v grafický runbook.  V takovém případě se používá k ověření pro sady runbook a prostředky Azure, jak je popsáno v [ověření Runbooků pomocí účtu uživatele Azure AD](automation-create-aduser-account.md).  První aktivitu načte přihlašovacích údajů, který má přístup k předplatnému Azure.  **Add-AzureAccount** aktivita pak používá toto pověření k ověřování pro všechny aktivity, které následují po.  A [propojení kanálu](automation-graphical-authoring-intro.md#links-and-workflow) se zde od **Get-AutomationPSCredential** je očekáván jeden objekt.  

![Přidat přihlašovací údaje na plátno](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>Pomocí přihlašovacích údajů prostředí PowerShell v DSC
Během konfigurace DSC ve službě Azure Automation, můžete odkazovat pomocí prostředků přihlašovacích údajů **Get-AutomationPSCredential**, prostředků přihlašovacích údajů můžete také být předán prostřednictvím parametrů, v případě potřeby. Další informace najdete v tématu [kompilování konfigurace v Azure Automation DSC](automation-dsc-compile.md#credential-assets).

## <a name="next-steps"></a>Další kroky
* Další informace o propojení v vytváření grafického obsahu najdete v tématu [odkazy v vytváření grafického obsahu](automation-graphical-authoring-intro.md#links-and-workflow)
* Chcete-li pochopit různé metody ověřování pomocí automatizace, přečtěte si téma [zabezpečení automatizace Azure](automation-security-overview.md)
* První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md). 

