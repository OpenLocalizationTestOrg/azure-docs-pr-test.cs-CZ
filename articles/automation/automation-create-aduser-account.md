---
title: "Vytvoření účtu uživatele Azure AD | Dokumentace Microsoftu"
description: "Tento článek popisuje postup vytvoření pověření účtu uživatele Azure AD pro runbooky ve službě Azure Automation k ověřování v Azure."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "uživatel azure active directory, správa služby azure, uživatelský účet azure ad"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: f0a9664898cd27529daf73d130dd25fd296a9b48
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Ověření runbooků pomocí nasazení Azure Classic a Resource Manager
Tento článek popisuje kroky, které musíte provést při konfiguraci uživatelského účtu Azure AD pro runbooky Azure Automation, které běží s modelem nasazení Azure Classic nebo prostředky Azure Resource Manageru.  Když toto zůstává podporovanou identitou ověřování pro runbooky na základě Azure Resource Manager, je použít účet spustit v Azure jako doporučenou metodu.       

## <a name="create-a-new-azure-active-directory-user"></a>Vytvoření nového uživatele Azure Active Directory
1. Přihlaste se k portálu Azure jako správce služby pro předplatné Azure, které chcete spravovat.
2. Vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé** > **nového uživatele**.
3. Zadejte podrobnosti pro uživatele, jako je třeba **název** a **uživatelské jméno**.  
4. Poznamenejte si celé jméno uživatele a dočasné heslo.
5. Vyberte **Directory role**.
6. Přiřazení role globální nebo správce s omezeními.
7. Odhlaste se ze služby Azure a potom se přihlaste zpět pomocí právě vytvořeného účtu. Zobrazí se výzva ke změně uživatelského hesla.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Vytvoření účtu Automation na webu Azure Portal
V této části provedete následující kroky, abyste na webu Azure Portal vytvořili účet Azure Automation, který budete spolu s runbooky používat ke správě prostředků v režimu Azure Resource Manager.  

1. Přihlaste se k portálu Azure jako správce služby pro předplatné Azure, které chcete spravovat.
2. Vyberte **Účty Automation**.
3. Vyberte **Přidat**.<br><br>![Přidání účtu Automation](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. V okně **Přidat účet Automation** do pole **Název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte předplatné pro nový účet a také novou nebo existující **skupinu prostředků** a **umístění** datového centra Azure.
6. U možnosti **Vytvořit účet Spustit v Azure jako** vyberte **Ano** a klikněte na tlačítko **Vytvořit**.  
   
    > [!NOTE]
    > Pokud se rozhodnete nevytvořit účet Spustit v Azure jako tím, že nevyberete možnost **Ne**, zobrazí se v okně **Přidání účtu Automation** zpráva upozornění.  Při vytváření účtu a jeho přiřazování k roli **přispěvatele** v předplatném, nebude mít účet odpovídající identitu ověřování v rámci adresářové služby vašeho předplatného, a proto nebude předplatné obsahovat žádné přístupové prostředky.  Všechny runbooky odkazující na tento účet kvůli tomu nebudou moct ověřit a provádět úlohy s prostředky Azure Resource Manager.
    > 
    >

    <br>![Přidání upozornění k účtu Automation](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Zatímco Azure vytváří účet Automation, můžete průběh sledovat v nabídce v části **Oznámení**.

Po vytvoření přihlašovacích údajů bude nutné, abyste vytvořili asset přihlašovacích údajů, který přidružíte k účtu Automation pomocí dříve vytvořeného účtu uživatele služby AD.  Nezapomeňte, že jsme účet Automation teprve vytvořili a že ještě není přidružený k identitě ověřování.  Proveďte kroky popsané v [článku Assety přihlašovacích údajů v Azure Automation](automation-credentials.md#creating-a-new-credential-asset) a zadejte hodnotu **uživatelského jména** ve formátu **doména\uživatel**.

## <a name="use-the-credential-in-a-runbook"></a>Použití přihlašovacích údajů v runbooku
Přihlašovací údaje v runbooku můžete získat pomocí aktivity [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) a použít je s [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) k připojení k vašemu předplatnému Azure. Pokud přihlašovací údaje patří správci několika předplatných Azure, potom byste měli použít také [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) a určit to správné. Můžete to vidět níže v ukázce prostředí Windows PowerShell, které se obvykle zobrazuje v horní části většiny runbooků služby Azure Automation.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Tyto řádky byste měli v runbooku opakovat po všech [kontrolních bodech](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints). Pokud je runbook pozastavený a potom se obnoví u dalšího pracovního procesu, je potřeba znovu provést ověření.

## <a name="next-steps"></a>Další kroky
* O různých typech runbooků a krocích k vytvoření vlastních runbooků si můžete přečíst v článku [Typy runbooků v Azure Automation](automation-runbook-types.md)

