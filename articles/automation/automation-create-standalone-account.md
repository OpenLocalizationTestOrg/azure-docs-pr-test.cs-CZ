---
title: "aaaCreate samostatný účet Azure Automation | Microsoft Docs"
description: "Kurz vás provede hello vytváření, testování a ukázkovým použitím ověření objektu zabezpečení ve službě Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 1500d25d9565d4082768933834303a17c5e84234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-azure-automation-account"></a>Vytvoření samostatného účtu Azure Automation
Toto téma ukazuje, jak toocreate účtu Automation na portálu Azure, pokud chcete tooevaluate a další bez včetně řešení pro další správu hello nebo integrace s tooprovide OMS analýzy protokolů Azure Automation hello rozšířené monitorování úlohy sady runbook.  Můžete přidat těchto řešení pro správu nebo integrovat analýzy protokolů v libovolném bodě budoucí hello.  Hello účet Automation jsou sady runbook může tooauthenticate správu prostředků v Azure Resource Manager nebo nasazení Azure classic.

Když vytvoříte účet služby Automation v hello portálu Azure, automaticky vytvoří:

* Účet Spustit jako, který vytvoří nový objekt služby v Azure Active Directory, certifikát, a přiřadí hello řízení přístupu na základě role Přispěvatel (RBAC), který je použit toomanage prostředky Resource Manageru pomocí sad runbook.   
* Classic účet Spustit jako tím, že nahrajete certifikát správy, který je použit toomanage klasické prostředky pomocí runbooků.  

To zjednodušuje proces hello pro vás a pomůže vám rychle začít vytváření a nasazení sad runbook toosupport vaše automation potřebuje.  

## <a name="permissions-required-toocreate-automation-account"></a>Oprávnění požadované toocreate účet Automation.
toocreate nebo aktualizace účtu Automation, musí mít hello následující konkrétní oprávnění a vyžadují oprávnění toocomplete v tomto tématu.   
 
* V pořadí toocreate účet Automation, účtu uživatele AD musí toobe přidané tooa role s roli vlastníka ekvivalentní toohello oprávnění pro Microsoft.Automation prostředky podle pokynů v článku [řízení přístupu na základě Role ve službě Azure Automation ](automation-role-based-access-control.md).  
* Pokud registrace aplikace hello nastavení je nastaven příliš**Ano**, mohou uživatelé bez oprávnění správce v klientovi služby Azure AD [registraci aplikací AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Pokud registrace aplikace hello nastavení je nastaven příliš**ne**, uživatel hello provedení této akce musí být globálním správcem ve službě Azure AD. 

Pokud si nejste členem instanci Active Directory hello předplatné, předtím, než jsou přidány toohello globální správce nebo řidičská-administrator role hello předplatné, se přidají tooActive Directory jako Host. V takovém případě se zobrazí "Nemáte oprávnění toocreate..." upozornění na hello **přidat účet Automation** okno. Uživatelé, kteří byly přidány toohello globální správce nebo řidičská-administrator role nejprve lze odebrat z instance služby Active Directory hello předplatného a přidáno toomake je úplné uživatelské ve službě Active Directory. tooverify této situaci se z hello **Azure Active Directory** podokně hello portál Azure, vyberte **uživatelů a skupin**, vyberte **všichni uživatelé** a po výběru hello konkrétního uživatele, vyberte **profil**. Hello hodnotu hello **typ uživatele** atribut v profilu uživatele hello nesmí rovnat **hosta**.

## <a name="create-a-new-automation-account-from-hello-azure-portal"></a>Vytvořit nový účet Automation z hello portálu Azure
V této části proveďte následující kroky toocreate účet Azure Automation v hello portál Azure hello.    

1. Přihlaste se toohello portálu Azure pomocí účtu, který je členem role Správci předplatného hello a spolusprávce předplatného hello.
2. Klikněte na možnost **Nové**.<br><br> ![Výběr možnosti Nové na webu Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Vyhledejte **automatizace** a pak v hello výsledky hledání vyberte **automatizace a řízení***.<br><br> ![Hledání a výběr Automation na Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
3. V okně účty Automation hello, klikněte na tlačítko **přidat**.<br><br>![Přidání účtu Automation](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
   
   > [!NOTE]
   > Pokud se zobrazí následující upozornění v hello hello **přidat účet Automation** okně je vzhledem k tomu, že váš účet není členem role Správci předplatného hello a spolusprávce předplatného hello.<br><br>![Přidání upozornění do účtu Automation](media/automation-create-standalone-account/create-account-without-perms.png)
   > 
   > 
4. V hello **přidat účet Automation** okno, v hello **název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte jednu pro hello nový účet, a nové nebo existující **skupiny prostředků** a datovém centru Azure **umístění**.
6. Ověření hodnoty hello **Ano** je vybraná hello **vytvořit Azure účet Spustit jako** a klikněte na hello **vytvořit** tlačítko.  
   
   > [!NOTE]
   > Pokud se rozhodnete toonot vytvořit účet Spustit jako hello výběrem možnosti hello **ne**, zobrazí se upozornění v hello **přidat účet Automation** okno.  Při hello účet je vytvořen v hello portálu Azure, nemá odpovídající identitu ověřování v rámci vaší classic nebo Resource Manager předplatné adresářové služby a proto tooresources žádný přístup v rámci vašeho předplatného.  To zabraňuje všechny runbooky odkazující na tento účet nebudou moct tooauthenticate a provádět úlohy s prostředky v těchto modelech nasazení.
   > 
   > ![Přidání upozornění do účtu Automation](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)<br>
   > Pokud není vytvořená hello instanční objekt není přiřazena role Přispěvatel hello.
   > 

7. Zatímco Azure vytváří účet Automation hello, můžete sledovat průběh hello pod **oznámení** nabídce hello.

### <a name="resources-included"></a>Zahrnuté prostředky
Když hello účet Automation je úspěšně vytvořen, jsou automaticky vytvoří několik prostředků.  Hello následující tabulka shrnuje prostředky pro hello účet Spustit jako.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureAutomationTutorial |Ukázkový grafický runbook, který ukazuje, jak pomocí tooauthenticate hello účet Spustit jako a získá všechny prostředky Resource Manager hello. |
| Runbook AzureAutomationTutorialScript |Ukázkový runbook prostředí PowerShell, který ukazuje, jak pomocí tooauthenticate hello účet Spustit jako a získá všechny prostředky Resource Manager hello. |
| AzureRunAsCertificate |Asset certifikátu automaticky vytvořené během vytváření účtu Automation nebo pomocí níže hello Powershellový skript pro existující účet.  Umožní vám tooauthenticate s Azure, aby mohl spravovat prostředky Azure Resource Manager ze sady runbook.  Tento certifikát má životnost jeden rok. |
| AzureRunAsConnection |Asset připojení automaticky vytvořené během vytváření účtu Automation nebo pomocí níže hello Powershellový skript pro existující účet. |

Hello následující tabulka shrnuje prostředky pro hello účet Classic spustit jako.<br>

| Prostředek | Popis |
| --- | --- |
| Runbook AzureClassicAutomationTutorial |Ukázkový grafický runbook, který získá všechny klasické virtuální počítače hello v předplatném pomocí hello Classic účet Spustit jako (certifikátu) a poté uloží hello název virtuálního počítače a stav. |
| Runbook se skriptem AzureClassicAutomationTutorial |Příklad PowerShell runbook služby, který získá všechny klasické virtuální počítače hello v předplatném pomocí hello Classic účet Spustit jako (certifikátu) a poté uloží hello název virtuálního počítače a stav. |
| AzureClassicRunAsCertificate |Asset certifikátu, který je automaticky vytvoří použít tooauthenticate s Azure, abyste mohli spravovat prostředky Azure classic ze sady runbook.  Tento certifikát má životnost jeden rok. |
| AzureClassicRunAsConnection |Asset připojení, který je automaticky vytvoří použít tooauthenticate s Azure, abyste mohli spravovat prostředky Azure classic ze sady runbook. |


## <a name="next-steps"></a>Další kroky
* toolearn Další informace o vytváření grafického obsahu, najdete v části [vytváření grafického obsahu ve službě Azure Automation](automation-graphical-authoring-intro.md).
* tooget kroky s runbooky prostředí PowerShell najdete v části [Můj první Powershellový runbook](automation-first-runbook-textual-powershell.md).
* tooget kroky s runbooky pracovních postupů Powershellu, najdete v části [Můj první runbook pracovního postupu Powershellu](automation-first-runbook-textual.md).
