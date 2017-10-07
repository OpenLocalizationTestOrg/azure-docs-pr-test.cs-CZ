---
title: aaaHow tooactivate nebo deaktivovat roli | Microsoft Docs
description: "Zjistěte, jak tooactivate role pro privilegované identity s aplikací Azure Privileged Identity Management hello."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 8c68ad69e4b006263bbb8a1cfc7ed3dba974e1db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooactivate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Jak tooactivate nebo deaktivace role v Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management zjednodušuje, jak spravovat podniky tooresources privilegovaného přístupu ve službě Azure AD a dalším službám Microsoft online jako je Office 365 nebo Microsoft Intune.  

Pokud jste zadali byl způsobilý pro roli správce, která znamená, že tato role může aktivovat, pokud budete potřebovat tooperform privilegované akce. Například pokud někdy spravujete funkcí Office 365, vaší organizace privilegované role správců nemusí mít je trvalé globální správce, vzhledem k tomu, že tato role má to dopad na jiné služby příliš. Místo toho budou požadovat, abyste vhodné pro Azure AD rolí, například správce serveru Exchange Online. Tooactivate můžete žádost této role, když potřebujete svá oprávnění a potom budete mít kontroly správce po předem určenou dobu.

Tento článek je pro správci, kteří potřebují tooactivate jejich role v Azure AD Privileged Identity Management (PIM). Ho vás provede kroky tooactivate hello roli potřebujete hello oprávnění, a deaktivovat hello role, když jste hotovi. Kromě toho privilegované role správců může vyžadovat schválení tooactivate roli (Preview). Další informace o [PIM schválení pracovních](./privileged-identity-management/azure-ad-pim-approval-workflow.md) sem.

## <a name="add-hello-privileged-identity-management-application"></a>Přidání aplikace Privileged Identity Management hello
Pomocí aplikace hello Azure AD Privileged Identity Management v hello [portál Azure](https://portal.azure.com/) toorequest aktivaci role, i když budete toooperate v jiném portálu nebo prostředí PowerShell. Pokud nemáte hello aplikace Azure AD Privileged Identity Management na portálu Azure, postupujte podle těchto kroků tooget spuštěna.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Vyberte své uživatelské jméno v hello pravém horním rohu hello portál Azure a vyberte hello adresáře, kde vám budou pracovat.
3. Vyberte **další služby** a používat hello filtru textbox toosearch pro **Azure AD Privileged Identity Management**.
4. Zkontrolujte **Pin toodashboard** a pak klikněte na **vytvořit**. Otevře se Hello aplikace Privileged Identity Management.

## <a name="activate-a-role"></a>Aktivovat roli
Pokud budete potřebovat tootake na roli, můžete požádat o aktivaci výběrem hello **Moje role** levé sloupec navigační prvku možnost navigace v aplikaci hello Azure AD Privileged Identity Management.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vyberte hello Azure AD Privileged Identity Management dlaždice.
2. Vyberte **Moje role**. Zobrazí seznam přiřazené role vhodné v hello seskupování hello horní části stránky hello.
3. Vyberte roli tooactivate.
4. Vyberte **aktivovat**. Hello **žádosti o aktivaci role** otevře se okno.
5. Některé role vyžadují služby Multi-Factor Authentication (MFA) před aktivací hello role. Stačí tooauthenticate jednou za relace.
   
    ![Ověřit pomocí vícefaktorového ověřování předtím, než aktivace role – snímek obrazovky][2]
6. Hello textového pole zadejte hello důvod pro žádost o aktivaci hello.  Některé role vyžadují toosupply číslo lístku řešení problémů.
7. Vyberte **OK**.  Pokud hello role nevyžaduje schválení, bude aktivován a hello role se zobrazí v seznamu hello aktivní role (přímo pod hello seznamu přiřazení rolí oprávněné). Pokud hello [role vyžaduje schválení](./privileged-identity-management/azure-ad-pim-approval-workflow.md) tooactivate, informační se krátce zobrazí v hello horním pravém rohu prohlížeče vás informuje o požadavek hello čeká na schválení.

    ![Žádost čeká na oznámení – snímek obrazovky][3]

## <a name="deactivate-a-role"></a>Deaktivovat roli
Po aktivaci role automaticky deaktivuje po dosažení jeho časový limit (vhodné doba trvání).

Pokud již v rané fázi dokončení úlohy správy, můžete také deaktivovat roli ručně v hello aplikace Azure AD Privileged Identity Management.  Vyberte **Moje role**, zvolte roli hello dokončení pomocí hello **přiřazení role služby Active** seskupování a vyberte možnost **deaktivovat**.  

## <a name="cancel-a-pending-request"></a>Zrušení čekající žádosti o
V případě hello nevyžadují aktivaci role, který vyžaduje schválení, můžete kdykoli zrušit čekající žádosti. Jednoduše vyberte hello **Moje role** levé sloupec navigační prvku možnost navigace v aplikaci hello Azure AD Privileged Identity Management.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vyberte hello Azure AD Privileged Identity Management dlaždice.
2. Vyberte **Moje role**. Zobrazí seznam přiřazené role vhodné v hello seskupování hello horní části stránky hello.
3. Vyberte roli.
4. Vyberte hello **Aktivace čeká na schválení** hlavičku v okně Podrobnosti o aktivaci role hello.
5. Vyberte **zrušit** hello horní části hello **čekající na schválení** okno.

   ![Zrušit čekající na vyřízení žádosti – snímek obrazovky][4]

## <a name="next-steps"></a>Další kroky
Pokud byste chtěli dozvědět více o Azure AD Privileged Identity Management, hello následující odkazy obsahovat další informace.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png
[3]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Toast2.png
[4]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_Request_Pending_Banner_Cancel.png
