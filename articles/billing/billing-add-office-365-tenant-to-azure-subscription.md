---
title: "aaaUse Office 365 klienta předplatné | Microsoft Docs"
description: "Zjistěte, jak tooadd Office 365 adresáře (klient) tooan předplatného Azure."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.openlocfilehash: e560370417bd074a7e37ceb7c60da45dbbbcf775
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="associate-an-office-365-tenant-tooan-azure-subscription"></a>Přidružit Office 365 klienta tooan předplatného Azure
Propojte samostatné předplatných Azure a Office 365, máte přístup klienta hello Office 365 z vašeho předplatného Azure. toolink vaše předplatné, přihlaste se tooAzure s hello účet správce služby Azure, přidejte adresáře a přidejte klienta Azure Active Directory toohello účty organizace hello Office 365.

Pokud chcete mít předplatné služeb Office 365 pro uživatele ve vaší instanci Azure Active Directory nebo máte účet Office 365, ale není účet Azure, najdete v části [registraci do Azure pomocí účtu Office 365](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Než začnete
* Musíte mít hello přihlašovací údaje Správce služby hello předplatného Azure. Účty společné správce nemůže provádět některé hello kroky v tomto článku. toochange Správce služby, najdete v části [jak tooadd nebo změna role Správce služby Azure](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Musí mít hello pověření pro globálního správce klienta Office 365 hello.
* Hello e-mailovou adresu správce služeb hello nesmí být v klientovi hello Office 365.
* Hello e-mailovou adresu správce služeb hello nesmí shodovat s globálním administrátorem klienta hello Office 365.
* Pokud používáte e-mailovou adresu, která je účet Microsoft a účet organizace, dočasně změňte hello služby správce vašeho předplatného Azure toouse jiný účet Microsoft. Můžete vytvořit účet Microsoft v hello [stránky registraci účtu Microsoft](https://signup.live.com/).

## <a name="link-office-365-tenant-tooazure-subscription"></a>Odkaz předplatné tooAzure klienta Office 365
tooassociate hello Office 365 klienta toohello předplatné Azure, postupujte takto:

### <a name="step-1-add-office-365-tenant-tooyour-azure-subscription"></a>Krok 1: Přidání tooyour klienta Office 365 předplatného Azure

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) s hello přihlašovací údaje Správce služby.

    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. V levém podokně hello vyberte **služby ACTIVE DIRECTORY**. Byste neměli vidět klienta hello Office 365. Pokud se zobrazí, přeskočte příliš[krok 2: změnit adresář hello hello předplatného Azure přidružené](#Step2).
   
   ![Snímek obrazovky Active Directory položka](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. Vyberte **nový** > **DIRECTORY** > **vlastní vytvořit**.
   
    ![Vytvořit vlastní snímek obrazovky nástroje Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. Na hello **adresáře přidat** v části **DIRECTORY**, vyberte **použít existující adresář**. Potom vyberte **jsem připravené toobe Odhlásit**a vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Snímek obrazovky "Použít existující adresář"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. Poté, co jste se odhlásili, přihlaste se pomocí přihlašovacích údajů hello globálního správce pro vašeho tenanta Office 365.
   
    ![Přihlášení globálního správce Office 365 – snímek obrazovky](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. Vyberte **pokračovat**.
   
    ![Snímek obrazovky ověření](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. Vyberte **Odhlásit**.
   
    ![Snímek obrazovky odhlášení](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) s hello přihlašovací údaje Správce služby.
   
    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. Měli byste vidět vašeho tenanta Office 365 na řídicím panelu hello.
   
    ![Snímek obrazovky řídicí panel](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>Krok 2: Změnit adresář hello přidružené hello předplatného Azure
   
1. Vyberte **nastavení**.
   
    ![Ikona snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. Vyberte předplatné Azure a pak vyberte **upravit adresář**.

    ![Snímek obrazovky Azure předplatné upravit adresář](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. Vyberte **Další** ![další ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Snímek obrazovky "Změna hello přidružené adresáře"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. Zkontrolujte účty hello vliv. Všechny spolusprávci a [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) uživatelé s přiřazenou přístupem v hello existující skupiny prostředků se odeberou. uvádí, hello odebrání spolusprávci pouze Hello upozornění, které se zobrazí.
      
    ![Snímek obrazovky, který zobrazuje hello spolusprávcem účty toobe odebrat.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Snímek obrazovky, který ukazuje příklad uživatelského účtu toobe odebrat.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. Vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-toohello-azure-active-directory-tenant"></a>Krok 3: Přidáte jako klienta Azure Active Directory toohello spolusprávci vaší organizace účtů Office 365
   
1. Vyberte hello **správci** a pak vyberte **přidat**.
   
    ![Karta správci snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. Zadejte účet organizace klienta služby Office 365, vyberte hello předplatného Azure a pak vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Snímek obrazovky Azure spolusprávcem dialogové okno Přidat](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. Přejděte zpět toohello **správci** kartě. Měli byste vidět účet organizace hello zobrazí jako spolusprávce.
   
    ![Snímek obrazovky s kartou správce](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  Test přístupu tooAzure pomocí účtu správce společné hello.
   
    a. Odhlásit z hello portál Azure classic.
   
    b. Otevřete hello [portál Azure](https://portal.azure.com/).
   
    c. Zadejte přihlašovací údaje hello hello společné správce a potom vyberte **přihlášení**.
   
    ![Snímek obrazovky Azure přihlašovací stránky](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém.


