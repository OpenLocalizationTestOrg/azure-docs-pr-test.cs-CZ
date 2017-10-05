---
title: "Předplatné služby Azure pomocí klienta služby Office 365 | Microsoft Docs"
description: "Informace o postupu přidání adresář služby Office 365 (klient) k předplatnému Azure."
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
ms.openlocfilehash: 7affb4a83cdb8ababef60e786a3c824e7477ed68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a>Přidružení klienta služby Office 365 k předplatnému Azure
Propojte samostatné předplatných Azure a Office 365, máte přístup klienta Office 365 z vašeho předplatného Azure. Odkaz vašich předplatných, přihlaste se k Azure pomocí účtu správce služby Azure, přidat adresář a přidejte účty organizace Office 365 pro klienta služby Azure Active Directory.

Pokud chcete mít předplatné služeb Office 365 pro uživatele ve vaší instanci Azure Active Directory nebo máte účet Office 365, ale není účet Azure, najdete v části [registraci do Azure pomocí účtu Office 365](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Než začnete
* Musíte mít pověření správce služby předplatného Azure. Účty správce společné nelze provést některé kroky v tomto článku. Chcete-li změnit správce služby, [postup přidání nebo změna role Správce služby Azure](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Musíte mít pověření pro globálního správce klienta Office 365.
* E-mailovou adresu správce služby nesmí být v klientovi Office 365.
* E-mailovou adresu správce služeb nesmí odpovídat všechny globální správce klienta Office 365.
* Pokud používáte e-mailovou adresu, která je účet Microsoft a účet organizace, dočasně změňte na služby správce vašeho předplatného Azure a použít jiný účet Microsoft. Můžete vytvořit účet Microsoft v [stránky registraci účtu Microsoft](https://signup.live.com/).

## <a name="link-office-365-tenant-to-azure-subscription"></a>Klienta Office 365 odkaz pro předplatné Azure
Chcete-li přidružit klienta Office 365 pro předplatné Azure, postupujte takto:

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a>Krok 1: Přidání klienta Office 365 k předplatnému Azure

1. Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) s přihlašovacími údaji správce služby.

    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. V levém podokně vyberte **služby ACTIVE DIRECTORY**. Byste neměli vidět klienta Office 365. Pokud se zobrazí, přejděte k [krok 2: Změňte adresář přidružený k předplatnému Azure](#Step2).
   
   ![Snímek obrazovky Active Directory položka](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. Vyberte **nový** > **DIRECTORY** > **vlastní vytvořit**.
   
    ![Vytvořit vlastní snímek obrazovky nástroje Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. Na **adresáře přidat** v části **DIRECTORY**, vyberte **použít existující adresář**. Potom vyberte **nyní mě můžete odhlásit**a vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Snímek obrazovky "Použít existující adresář"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. Poté, co jste se odhlásili, přihlaste se pomocí přihlašovacích údajů globálního správce vašeho tenanta Office 365.
   
    ![Přihlášení globálního správce Office 365 – snímek obrazovky](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. Vyberte **pokračovat**.
   
    ![Snímek obrazovky ověření](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. Vyberte **Odhlásit**.
   
    ![Snímek obrazovky odhlášení](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) s přihlašovacími údaji správce služby.
   
    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. Měli byste vidět vašeho tenanta Office 365 v řídicím panelu.
   
    ![Snímek obrazovky řídicí panel](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>Krok 2: Změňte adresář přidružený k předplatnému Azure
   
1. Vyberte **nastavení**.
   
    ![Ikona snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. Vyberte předplatné Azure a pak vyberte **upravit adresář**.

    ![Snímek obrazovky Azure předplatné upravit adresář](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. Vyberte **Další** ![další ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Snímek obrazovky "Změna přidružené adresáře"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. Zkontrolujte ovlivněných účty. Všechny spolusprávci a [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) uživatelé s přiřazenou přístupem v existující skupiny prostředků se odeberou. Uvádí, odebrání spolusprávci pouze upozornění, které se zobrazí.
      
    ![Snímek obrazovky, který zobrazuje účty spolusprávcem odeberou.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Snímek obrazovky zobrazující uživatelský účet příklad odeberou.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. Vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a>Krok 3: Přidání vaší organizace účtů Office 365 jako spolusprávce pro klienta služby Azure Active Directory
   
1. Vyberte **správci** a pak vyberte **přidat**.
   
    ![Karta správci snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. Zadejte účet organizace klienta služby Office 365, vyberte předplatné Azure a potom vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Snímek obrazovky Azure spolusprávcem dialogové okno Přidat](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. Přejděte zpět **správci** kartě. Měli byste vidět účet organizace zobrazí jako spolusprávce.
   
    ![Snímek obrazovky s kartou správce](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  Test přístupu k Azure pomocí účtu správce společné.
   
    a. Odhlásit z portálu Azure classic.
   
    b. Otevřete web [Azure Portal](https://portal.azure.com/).
   
    c. Zadejte přihlašovací údaje správce společné a potom vyberte **přihlášení**.
   
    ![Snímek obrazovky Azure přihlašovací stránky](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.


