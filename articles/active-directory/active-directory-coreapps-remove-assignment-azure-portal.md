---
title: "aaaRemove uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak tooremove hello přistupovat k přiřazení uživatele nebo skupiny z podnikové aplikace v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: c067ecf59b4dedfe8f848357ca8bd545bdc610eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace v Azure Active Directory
Je snadno tooremove uživatele nebo skupiny z přiřazení přístupu tooone podnikových aplikací ve službě Azure Active Directory (Azure AD). Musíte mít hello příslušná oprávnění toomanage hello firemní aplikace a musí být globální správce adresáře hello.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Jak odebrat uživatele nebo přiřazení skupiny?
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.
3. Na hello **Azure Active Directory - *directoryname***  okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. Na hello **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí se seznam hello aplikací, které můžete spravovat.
5. Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.
6. Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **uživatelé a skupiny**.

    ![Výběr uživatelů nebo skupin](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. Na hello ***appname*** **-uživatele & přiřazení skupiny** okně, vyberte jednu z další uživatele nebo skupiny a pak vyberte hello **odebrat** příkaz. Zkontrolujte vaše rozhodnutí hello příkazového řádku.

    ![Vyberte příkaz odebrat hello](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Další kroky
* [Zobrazit všechny moje skupin](active-directory-groups-view-azure-portal.md)
* [Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)
* [Zakázat přihlášení uživatele pro aplikaci, enterprise](active-directory-coreapps-disable-app-azure-portal.md)
* [Změňte název hello nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
