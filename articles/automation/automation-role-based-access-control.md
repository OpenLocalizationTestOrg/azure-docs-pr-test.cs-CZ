---
title: "řízení přístupu na základě aaaRole ve službě Azure Automation | Microsoft Docs"
description: "Řízení přístupu na základě role (RBAC) umožňuje správu přístupu k prostředkům Azure. Tento článek popisuje, jak tooset až RBAC ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: "rbac v automation, řízení přístupu na základě rolí, rbac v azure"
ms.assetid: 04b5625e-0ee8-4b5b-85cd-7734c1b3d4a3
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 051438e44d0c5c514d6dbaac5a312344ee311cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-in-azure-automation"></a>Řízení přístupu na základě role ve službě Azure Automation
## <a name="role-based-access-control"></a>Řízení přístupu na základě role
Řízení přístupu na základě role (RBAC) umožňuje správu přístupu k prostředkům Azure. Pomocí [RBAC](../active-directory/role-based-access-control-configure.md), můžete oddělit povinností v rámci týmu a udělit pouze hello množství toousers přístup, skupiny nebo aplikace, které potřebují tooperform svou práci. Toousers pomocí hello portálu Azure, nástroje příkazového řádku Azure nebo rozhraní API pro správu Azure můžete udělit přístup na základě rolí.

## <a name="rbac-in-automation-accounts"></a>Řízení přístupu na základě role v účtech Automation
Ve službě Azure Automation se přístup uděluje přiřazením toousers hello odpovídající RBAC role, skupiny a aplikace na hello rozsahu účtu Automation. Následující jsou hello vestavěné role, které účet Automation podporuje:  

| **Role** | **Popis** |
|:--- |:--- |
| Vlastník |Hello role vlastníka umožňuje přístup k prostředkům tooall a akce v rámci účtu Automation, včetně poskytnutí přístupu tooother uživatele, skupiny a aplikace toomanage hello účet Automation. |
| Přispěvatel |role Přispěvatel Hello vám umožní toomanage všechno kromě úpravy jiný uživatel přístup k účtu Automation tooan oprávnění. |
| Čtenář |role čtenáře Hello umožňuje tooview všechny prostředky hello v Automation účtu však nelze provádět změny. |
| Operátor služby Automation |roli operátor automatizace Hello vám umožní provozní úlohy tooperform jako je například spuštění, zastavení, pozastavení, obnovení a plánování úloh. Tato role je užitečná, pokud chcete tooprotect prostředky na účtu Automation jako assety přihlašovacích údajů a runbooky nebudou zobrazit nebo upravit, ale stále umožňuje členům vaší organizace tooexecute tyto sady runbook. |
| Správce přístupu uživatelů |role správce přístupu uživatelů Hello umožňuje účty Automation tooAzure přístup uživatele toomanage. |

> [!NOTE]
> Nelze udělit přístup práva tooa konkrétní sada runbook nebo sady runbook, pouze toohello prostředkům a akcím v rámci hello účet Automation.  
> 
> 

V tomto článku jsme vás provede procesem jak tooset až RBAC ve službě Azure Automation. Ale první, můžeme provést blíže podívejte se na hello jednotlivých oprávnění udělené toohello přispěvatele, čtečky, operátor automatizace a správce přístupu uživatelů tak, aby jsme získat dobrou znalost před udělením každý, kdo rights toohello účet Automation.  V opačném případě by to mohlo přinést nezamýšlené nebo nežádoucí důsledky.     

## <a name="contributor-role-permissions"></a>Oprávnění role přispěvatele
Hello následující tabulka představuje hello konkrétní akce, které lze provést pomocí role Přispěvatel hello ve službě Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Účet Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset certifikátu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset typu připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset přihlašovacích údajů Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset plánu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Asset proměnné Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Požadovaný stav konfigurace Automation | | | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Typ prostředku procesu Hybrid Runbook Worker |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Úloha Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Datový proud úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Plán úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Modul Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |
| Runbook Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Koncept runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Testovací úloha konceptu runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Webhook služby Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Oprávnění role čtenáře
Hello následující tabulka představuje hello konkrétní akce, které lze provést pomocí role Čtenář hello ve službě Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Klasický správce předplatného |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Zámek pro správu |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Oprávnění |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Operace poskytovatele |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Přiřazení role |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Definice role |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="automation-operator-role-permissions"></a>Oprávnění role operátora služby Automation
Hello následující tabulka představuje hello konkrétní akce, které lze provést pomocí role operátora automatizace hello ve službě Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Účet Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset certifikátu Automation | | | | |
| Asset připojení Automation | | | | |
| Asset typu připojení Automation | | | | |
| Asset přihlašovacích údajů Automation | | | | |
| Asset plánu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | |
| Asset proměnné Automation | | | | |
| Požadovaný stav konfigurace Automation | | | | |
| Typ prostředku procesu Hybrid Runbook Worker | | | | |
| Úloha Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |
| Datový proud úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Plán úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | |
| Modul Automation | | | | |
| Runbook Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Koncept runbooku Automation | | | | |
| Testovací úloha konceptu runbooku Automation | | | | |
| Webhook služby Automation | | | | |

Další podrobnosti, hello [akce operátora automatizace](../active-directory/role-based-access-built-in-roles.md#automation-operator) seznamy hello akcí podporovaných rolí operátora automatizace hello na účtu Automation hello a její prostředky.

## <a name="user-access-administrator-role-permissions"></a>Oprávnění role správce přístupu uživatelů
Hello následující tabulka představuje hello konkrétní akce, které lze provést pomocí hello role správce přístupu uživatelů ve službě Automation.

| **Typ prostředku** | **Čtení** | **Zápis** | **Odstranění** | **Další akce** |
|:--- |:--- |:--- |:--- |:--- |
| Účet Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset certifikátu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset typu připojení Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset přihlašovacích údajů Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset plánu Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Asset proměnné Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Požadovaný stav konfigurace Automation | | | | |
| Typ prostředku procesu Hybrid Runbook Worker |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Úloha Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Datový proud úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Plán úlohy Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Modul Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Runbook Azure Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Koncept runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Testovací úloha konceptu runbooku Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Webhook služby Automation |![Zelený stav](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Konfigurace řízení přístupu na základě role u vašeho účtu Automation pomocí portálu Azure
1. Přihlaste se toohello [portálu Azure](https://portal.azure.com/) a otevřete svůj účet Automation v okně účty Automation hello.  
2. Klikněte na hello **přístup** řízení na hello pravém horním rohu. Tím se otevře hello **uživatelé** okno, kde můžete přidat nové uživatele, skupiny a aplikace toomanage v účtu Automation a zobrazení existující role, které mohou být konfigurovány pro hello účet Automation.  
   
   ![Tlačítko Přístup](media/automation-role-based-access-control/automation-01-access-button.png)  

> [!NOTE]
> **Správci předplatného** již existuje jako hello výchozího uživatele. skupiny služby active directory admins Hello předplatné zahrnuje hello Správce služeb a spolusprávce pro předplatné Azure. Dobrý den, Správce služby je vlastníkem hello vašeho předplatného Azure a jeho prostředků a bude mít roli vlastníka hello zděděná pro účty automation hello příliš. To znamená, že je hello přístup **zděděné** pro **služby správci a pomocní správci** předplatného a jeho **přiřazeno** pro všechny hello jiných uživatelů. Klikněte na tlačítko **správci předplatného** tooview více podrobností o jejich oprávnění.  
> 
> 

### <a name="add-a-new-user-and-assign-a-role"></a>Přidání nového uživatele a přiřazení role
1. V okně uživatelé hello, klikněte na tlačítko **přidat** tooopen hello **přidat přístup okno** kde můžete přidat uživatele, skupiny nebo aplikace a přiřadit role toothem.  
   
   ![Přidání uživatele](media/automation-role-based-access-control/automation-02-add-user.png)  
2. Vyberte roli ze seznamu dostupných rolí hello. Vybereme hello **čtečky** role, ale můžete vybrat všechny hello dostupné předdefinované role, které účet Automation podporuje, nebo jakoukoli vlastní roli, kterou jste definovali.  
   
   ![Výběr role](media/automation-role-based-access-control/automation-03-select-role.png)  
3. Klikněte na **přidat uživatele** tooopen hello **přidat uživatele** okno. Pokud jste přidali všechny uživatele, skupiny nebo aplikace toomanage vaše předplatné pak tito uživatelé jsou uvedena v seznamu a můžete je vybrat tooadd přístup. Pokud nejsou k dispozici všechny uživatele uvedené nebo pokud není uvedené hello uživatele můžete se zajímáte o přidání klikněte **pozvat** tooopen hello **pozvání hosta** okno, kde můžete pozvat uživatele s platným účtem Microsoft e-mailovou adresu, jako jsou Outlook.com, OneDrive nebo Xbox Live ID.. Po zadání e-mailovou adresu hello hello uživatele, klikněte na tlačítko **vyberte** tooadd hello uživatele a pak klikněte na **OK**. 
   
   ![Přidání uživatelů](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Teď byste měli vidět hello uživatele, přidání toohello **uživatelé** okno s hello **čtečky** přiřazenou roli.  
   
   ![Vypsání uživatelů](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   Je také možné přiřadit role uživatele toohello z hello **role** okno. 
4. Klikněte na tlačítko **role** z hello uživatelé okno tooopen hello **okno role**. V tomto okně můžete zobrazit název hello hello role, počet hello uživatelé a skupiny přiřazené toothat role.
   
    ![Přiřazení role v okně Uživatelé](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Řízení přístupu na základě rolí lze nastavit pouze na úrovni účtu Automation hello a ne u jakéhokoliv prostředku níže hello účet Automation.
   > 
   > 
   
    Můžete přiřadit více než jeden uživatel tooa role, skupiny nebo aplikace. Například, pokud přidáme hello **operátor automatizace** role společně s hello **role Čtenář** toohello uživatele a potom se můžete zobrazit všechny prostředky Automation hello, a také spouštět úlohy sady runbook hello. Můžete rozbalit tooview hello rozevírací seznam role přiřazené toohello uživatele.  
   
    ![Zobrazení více rolí](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  

### <a name="remove-a-user"></a>Odebrání uživatele
Můžete odebrat hello přístupová oprávnění pro uživatele nespravuje hello účet Automation nebo už kdo hello organizace. Následující jsou hello kroky tooremove uživatele: 

1. Z hello **uživatelé** okno, že si přejete tooremove přiřazení role vyberte hello.
2. Klikněte na tlačítko hello **odebrat** tlačítka v okně podrobností přiřazení hello.
3. Klikněte na tlačítko **Ano** tooconfirm odebrání. 
   
   ![Odebrání uživatelů](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Uživatel přiřazený k roli
Když uživatel s přiřazenou roli tooa přihlásí tootheir účet Automation, může vidět účet vlastníka hello uvedený v seznamu hello **výchozí adresáře**. V pořadí tooview hello účet Automation, které byly přidány do musí přepnout výchozí adresář hello výchozí adresář toohello vlastníka.  

![Výchozí adresář](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Uživatelské prostředí pro roli operátora služby Automation
Když uživatel, který je přiřazen zobrazení roli operátor automatizace toohello hello účet Automation, které jsou přiřazeny, mohou pouze hello zobrazit seznam sad runbook, úlohy a plány runbooků vytvořené v hello účet Automation, ale nemůže zobrazit jejich definice. Mohou spustit, zastavit, pozastavit, obnovit nebo naplánovat úlohu runbooku hello. Hello uživatel nebude mít přístup k prostředkům Automation tooother například ke konfiguracím, skupinám hybrid worker nebo uzlům DSC.  

![Žádný přístup tooresourcres](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Hello uživatel klikne na hello runbook, hello příkazy tooview hello zdroje nebo upravit hello runbook nejsou k dispozici jako role operátora služby Automation hello neumožňuje toothem přístup.  

![Žádný přístup tooedit runbook](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Hello uživatele budou mít přístup tooview a toocreate plánů, ale nebude mít přístup tooany jiný typ prostředku.  

![Žádný přístup tooassets](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Tento uživatel také nemá přístup k tooview hello webhooky přidružené k runbooku

![Žádný přístup toowebhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Konfigurace řízení přístupu na základě role u vašeho účtu Automation pomocí Azure PowerShellu
Přístup na základě rolí může být také nakonfigurované tooan účtu Automation pomocí následující hello [rutin prostředí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) vypíše všechny role funkce řízení přístupu na základě role, které jsou ve službě Azure Active Directory dostupné. Můžete použít tento příkaz spolu s hello **název** vlastnost toolist všechny hello akce, které může provádět určité role.  
    **Příklad:**  
    ![Získání definice role](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) seznamy přiřazení rolí pro Azure AD RBAC v hello zadaný obor. Pokud nezadáte žádné parametry tento příkaz vrátí všechna přiřazení rolí hello provedené v rámci předplatného hello. Použití hello **ExpandPrincipalGroups** přiřazení přístupu toolist parametr pro hello zadané uživatele, jakož i skupiny hello hello uživatel je členem skupiny.  
    **Příklad:** použití hello následující příkaz toolist všichni uživatelé hello a jejich rolí v rámci účtu automation.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Získat přiřazení role](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) tooassign toousers, skupiny nebo aplikace tooa konkrétní oboru přístupu.  
    **Příklad:** použití hello následující příkaz role "Operátor automatizace" hello tooassign pro uživatele v hello rozsahu účtu Automation.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish toogrant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Přiřazení nové role](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• Použijte [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) tooremove přístup zadaného uživatele, skupiny nebo aplikace v určitém rozsahu.  
    **Příklad:** použití hello následující příkaz tooremove hello uživatele z role "Operátor automatizace" hello v hello rozsahu účtu Automation.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish tooremove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

V hello výše příklady, nahraďte **přihlašovací Id**, **Id předplatného**, **název skupiny prostředků** a **název účtu Automation** s vaší Podrobnosti o účtu. Zvolte **Ano** při zobrazení výzvy tooconfirm před pokračováním tooremove přiřazení role uživatele.   

## <a name="next-steps"></a>Další kroky
* Informace o různých způsobech tooconfigure RBAC pro Azure Automation najdete v části příliš[spravovat RBAC v prostředí Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md).
* Podrobnosti na různé způsoby toostart sady runbook najdete v tématu [spuštění sady runbook](automation-starting-a-runbook.md)
* Informace o různých typech runbooků, najdete v části příliš[typy runbooků ve službě Azure Automation](automation-runbook-types.md)

