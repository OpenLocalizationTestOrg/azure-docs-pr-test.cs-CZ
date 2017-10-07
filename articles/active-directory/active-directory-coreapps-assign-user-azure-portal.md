---
title: "aaaAssign aplikace enterprise uživatele nebo skupinu tooan v Azure Active Directory | Microsoft Docs"
description: "Jak tooselect podnikové aplikace tooassign uživatele nebo skupinu tooit v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 86c11f19892b9c947a5331677c17759178ed2806
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-or-group-tooan-enterprise-app-in-azure-active-directory"></a>Přiřaďte aplikaci enterprise uživatele nebo skupinu tooan v Azure Active Directory
Je snadno tooassign uživatele nebo skupiny tooyour podnikové aplikace v Azure Active Directory (Azure AD). Musíte mít hello příslušná oprávnění toomanage hello firemní aplikace a musí být globální správce adresáře hello.

## <a name="how-do-i-assign-user-access-tooan-enterprise-app"></a>Přiřazení uživatelů přístup tooan firemní aplikace
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte do textového pole hello Azure Active Directory a potom vyberte **Enter**.
3. Na hello **Azure Active Directory - *directoryname***  okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. Na hello **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí se seznam hello aplikací, které můžete spravovat.
5. Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.
6. Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **uživatelé a skupiny**.

    ![Výběr hello příkaz všechny aplikace](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. Na hello ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte hello **přidat** příkaz.
8. Na hello **přidat přiřazení** vyberte **uživatelů a skupin**.

    ![Přiřadit uživatele nebo skupiny toohello aplikace](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. Na hello **uživatelů a skupin** okně, vyberte jeden nebo více uživatele nebo skupiny z hello seznamu a pak vyberte hello **vyberte** tlačítko v hello dolní části okna hello.
10. Na hello **přidat přiřazení** vyberte **Role**. Potom na hello **vybrat roli** okně, vyberte toohello tooapply role vybrané uživatele nebo skupiny a potom vyberte hello **OK** tlačítko v hello dolní části okna hello.
11. Na hello **přidat přiřazení** okně, vyberte hello **přiřadit** tlačítko v hello dolní části okna hello. Hello přiřadit uživatele nebo skupiny budou mít oprávnění hello definované hello vybranou roli pro tuto aplikaci enterprise.

## <a name="next-steps"></a>Další kroky
* [Zobrazit všechny moje skupin](active-directory-groups-view-azure-portal.md)
* [Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Zakázat přihlášení uživatele pro aplikaci, enterprise](active-directory-coreapps-disable-app-azure-portal.md)
* [Změňte název hello nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
