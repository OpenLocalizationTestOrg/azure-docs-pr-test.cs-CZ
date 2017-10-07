---
title: "aaaCreate účtu uživatele Azure AD | Microsoft Docs"
description: "Tento článek popisuje, jak přihlašovací údaje toocreate účtu uživatele Azure AD pro runbooky ve službě Azure Automation tooauthenticate ve službě Azure a classic Azure."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: "uživatel azure active directory, správa služby azure, uživatelský účet azure ad"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 7c6ed4182dbab074d0bc5da7057f74ad321d8884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Ověření runbooků pomocí nasazení Azure Classic a Resource Manager
Tento článek popisuje kroky hello je třeba provést tooconfigure účtu uživatele Azure AD pro runbooky služby automatizace Azure spuštěným pro model nasazení Azure classic nebo prostředky Azure Resource Manager.  I když toto zůstává toobe podporovanou identitou ověřování pro vaše Azure Resource Manager na základě sady runbook, hello doporučoval metoda se pomocí účtu spustit v Azure jako.       

## <a name="create-a-new-azure-active-directory-user"></a>Vytvoření nového uživatele Azure Active Directory
1. Přihlaste se na portálu Azure Classic toohello jako správce služby pro předplatné Azure, které chcete toomanage hello.
2. Vyberte **služby Active Directory**a potom vyberte hello název adresáře své organizace.
3. Vyberte hello **uživatelé** a pak vyberte v oblasti příkazů hello **přidat uživatele**.
4. Na hello **Povězte nám o tohoto uživatele** v části **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.
5. Zadejte jméno uživatele.  
6. Vyberte název adresáře hello, která souvisí s předplatným Azure na stránce služby Active Directory hello.
7. Na hello **profil uživatele** stránky, zadejte první a poslední jméno, uživatelské jméno a uživatele z hello **role** seznamu.  **Nepovolujte službu Multi-Factor Authentication**.
8. Všimněte si, úplný název hello uživatele a dočasné heslo.
9. Vyberte **Nastavení > Správci > Přidat**.
10. Zadejte úplné uživatelské jméno hello hello uživatele, který jste vytvořili.
11. Vyberte předplatné hello, který chcete hello toomanage uživatele.
12. Odhlaste se z Azure a pak se přihlaste zpět pomocí hello účet, který jste právě vytvořili. Bude výzvami toochange hello uživatelského hesla.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Vytvoření účtu Automation na portálu Azure Classic
V této části provedete následující kroky toocreate účet Azure Automation v hello portál Azure pro použití s runbooky správu prostředků v nasazení Azure classic hello.  

> [!NOTE]
> Účty Automation vytvořené pomocí portálu Azure Classic hello je možné spravovat pomocí hello Azure Classic a portálu Azure i buď sadu rutin. Po vytvoření účtu hello nezáleží na tom, jak vytvořit a spravovat prostředky v rámci účtu hello. Pokud plánujete toocontinue toouse hello portálu Azure Classic, měli byste použít ho místo hello Azure toocreate portálu účtů Automation.
> 
> 

1. Přihlaste se na portálu Azure Classic toohello jako správce služby pro předplatné Azure, které chcete toomanage hello.
2. Vyberte **Automation**.
3. Na hello **automatizace** vyberte **vytvořit účet Automation**.
4. V hello **vytvořit účet Automation** pole, zadejte název nového účtu Automation a vyberte **oblast** z rozevíracího seznamu hello.  
5. Klikněte na tlačítko **OK** tooaccept nastavení a vytvořte účet hello.
6. Po vytvoření objeví se na hello **automatizace** stránky.
7. Klikněte na účet hello a zůstalo jste toohello stránka řídicího panelu.  
8. Na stránce řídicí panel automatizace hello vyberte **prostředky**.
9. Na hello **prostředky** vyberte **přidat nastavení** nacházející se v hello dolní části stránky hello.
10. Na hello **přidat nastavení** vyberte **přidat přihlašovací údaje**.
11. Na hello **definování přihlašovacích údajů** vyberte **přihlašovací údaje Windows PowerShell** z hello **typ přihlašovacích údajů** rozevíracího seznamu a zadejte název pro přihlašovací údaje hello.
12. Na následující hello **definování přihlašovacích údajů** stránky zadejte uživatelské jméno hello hello AD uživatelský účet vytvořený dříve v hello **uživatelské jméno** pole a hello heslo v hello **heslo**a **Potvrdit heslo** pole. Klikněte na tlačítko **OK** toosave změny.

## <a name="create-an-automation-account-in-hello-azure-portal"></a>Vytvoření účtu Automation v hello portálu Azure
V této části proveďte následující kroky toocreate účet Azure Automation v hello portál Azure pro použití s runbooky správu prostředků v režimu Azure Resource Manager hello.  

1. Přihlaste se toohello portálu Azure jako správce služeb pro předplatné Azure, které chcete toomanage hello.
2. Vyberte **Účty Automation**.
3. V okně účty Automation hello, klikněte na tlačítko **přidat**.<br><br>![Přidání účtu Automation](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. V hello **přidat účet Automation** okno, v hello **název** zadejte název nového účtu Automation.
5. Pokud máte více než jedno předplatné, zadejte text hello, jednu pro hello nový účet, a také novou nebo stávající **skupiny prostředků** a datovém centru Azure **umístění**.
6. Vyberte hodnotu hello **Ano** pro hello **vytvořit Azure účet Spustit jako** a klikněte na hello **vytvořit** tlačítko.  
   
    > [!NOTE]
    > Pokud se rozhodnete toonot vytvořit účet Spustit jako hello výběrem možnosti hello **ne**, zobrazí se upozornění v hello **přidat účet Automation** okno.  Při hello účet je vytvořen a přiřadit toohello **Přispěvatel** role v hello předplatném, nebude mít účet odpovídající identitu ověřování v rámci adresářové služby a proto není přístup prostředky ve vašem předplatném.  Tím všechny runbooky odkazující na tento účet nebudou moct tooauthenticate a provádět úlohy s prostředky Azure Resource Manager.
    > 
    >

    <br>![Přidání upozornění k účtu Automation](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Zatímco Azure vytváří účet Automation hello, můžete sledovat průběh hello pod **oznámení** nabídce hello.

Po dokončení vytvoření hello hello pověření musíte toocreate hello tooassociate Asset přihlašovacích údajů účtu Automation s hello AD uživatelský účet vytvořený.  Pamatujte si, že jsme vytvořili pouze hello účet Automation a není přidružená k identitě ověřování.  Hello kroků uvedených v hello [assety přihlašovacích údajů ve službě Azure Automation článku](automation-credentials.md#creating-a-new-credential-asset) a zadejte hodnotu hello **uživatelské jméno** ve formátu hello **doména\uživatel**.

## <a name="use-hello-credential-in-a-runbook"></a>Použití přihlašovacích údajů hello v runbooku
Můžete načíst hello přihlašovací údaje v runbooku pomocí hello [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) aktivity a použít je s [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) tooconnect tooyour předplatného Azure. Pokud pověření hello je správcem více předplatných Azure, pak byste měli použít také [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) toospecify hello správná. To je znázorněno v ukázce hello níže prostředí Windows PowerShell, které se obvykle zobrazuje na začátku hello většina runbooků služeb automatizace Azure.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Tyto řádky byste měli v runbooku opakovat po všech [kontrolních bodech](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints). Pokud hello runbook pozastavený a potom obnoví u dalšího pracovního procesu, ho bude nutné znovu tooperform hello ověřování.

## <a name="next-steps"></a>Další kroky
* Zkontrolujte hello různých typech runbooků a kroky pro vytváření vlastních runbooků hello tohoto článku [typy runbooků ve službě Azure Automation](automation-runbook-types.md)

