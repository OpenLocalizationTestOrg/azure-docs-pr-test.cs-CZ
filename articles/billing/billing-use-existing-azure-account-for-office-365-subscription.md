---
title: "aaaSign službu Office 365 pomocí účtu Azure | Microsoft Docs"
description: "Zjistěte, jak předplatné toocreate Office 365 pomocí účtu Azure"
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: cjiang
ms.openlocfilehash: d19e1c1edff0b9658b639e796a72bbf4e87b9c3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Zaregistrujte si předplatné služeb Office 365 s vaším účtem Azure
Pokud jste Azure odběratele, můžete použít váš účet Azure toosign pro předplatné služeb Office 365. Pokud jste součástí v organizaci, která má předplatné Azure, můžete vytvořit předplatná Office 365 pro uživatele ve vaší stávající Azure Active Directory (Azure AD). Zaregistrujte si tooOffice 365 pomocí účtu, který má oprávnění globální správce nebo správce fakturace v klientovi služby Azure Active Directory. Další informace najdete v tématu [zkontrolujte oprávnění pro uživatelský účet ve službě Azure AD](#RoleInAzureAD) a [přiřazení rolí správce v Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

Pokud již máte účet Office 365 a předplatné Azure, můžete [přidružit Office 365 klienta tooan předplatné](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Získat předplatné služeb Office 365 pomocí účtu Azure

1. Přejděte toohello [stránky produktu Office 365](https://products.office.com/business)a vyberte plán.
2. Klikněte na tlačítko **přihlášení** na hello pravém horním rohu stránky hello.

    ![snímek obrazovky stránky zkušební verzi Office 365](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Přihlaste se pomocí přihlašovacích údajů účtu Azure. Pokud vytváříte odběr pro vaši organizaci, použijte účet Azure, který je členem hello globální správce nebo správce fakturace role adresáře v klientovi služby Azure Active Directory.

    ![Snímek obrazovky Office 365 přihlásit](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Klikněte na tlačítko **vyzkoušet**.

    ![Snímek obrazovky, který potvrzuje vaši objednávku pro Office 365.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Na hello stránka potvrzení objednávky, klikněte na tlačítko **pokračovat**.

    ![Snímek obrazovky příjmu objednávky hello Office 365](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Nyní je vše připraveno. Pokud vytvoříte odběr hello Office 365 pro vaši organizaci, použijte hello následující toocheck kroky, které uživatelům Azure AD jsou teď ve službách Office 365.

1. Otevřete Centrum pro správu hello Office 365.
2. Rozbalte položku **uživatelé**a potom klikněte na **aktivní uživatelé**.

    ![Snímek obrazovky center správci hello Office 365](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

Po přihlášení, hello předplatné služeb Office 365 se přidá toohello stejnou instanci služby Azure Active Directory, který je součástí vašeho předplatného Azure. Další informace najdete v tématu [Další informace o předplatných Azure a Office 365](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) a [asociování předplatných Azure se službou Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Zkontrolujte oprávnění pro uživatelský účet ve službě Azure AD
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Klikněte na tlačítko **další služby**a poté vyhledejte **služby Active Directory**.

    ![Snímek obrazovky Active Directory v hello portálu Azure](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Klikněte na tlačítko **uživatelů a skupin** > **všichni uživatelé**.
4. Vyberte hello uživatelské jméno. 

    ![Snímek obrazovky zobrazující hello uživatelů Azure Active Directory](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Klikněte na tlačítko **Directory role**.
  
    ![Snímek obrazovky zobrazující role Azure directory portálu hello](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  Hello role **globálního správce** nebo **správce s omezeními** > **správce fakturace** je požadovaná toocreate předplatné služeb Office 365 pro uživatele v existující Azure Active Directory.

    ![Snímek obrazovky zobrazující role Azure directory portálu správce fakturace](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget rychle vyřešit problém. 
