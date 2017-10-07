---
title: "aaaDisable uživatelská přihlášení pro podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Jak toodisable podniková aplikace tak, aby žádní uživatelé můžou přihlásit tooit v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a27562f9-18dc-42e8-9fee-5419566f8fd7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 4c560b59359d433b0852a7606cc2cc0204866234
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Zakázat přihlášení uživatele pro podnikové aplikace v Azure Active Directory
Je snadno toodisable podniková aplikace tak, aby žádní uživatelé můžou přihlásit tooit v Azure Active Directory (Azure AD). Musíte mít hello příslušná oprávnění toomanage hello firemní aplikace a musí být globální správce adresáře hello.

## <a name="how-do-i-disable-user-sign-ins"></a>Jakým způsobem vypnout uživatelská přihlášení?
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.
3. Na hello **Azure Active Directory** -  ***directoryname*** okno (tedy hello Azure AD okno pro adresář hello spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Na hello **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí seznam hello aplikací, které můžete spravovat.
5. Na hello **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.
6. Na hello ***appname*** okno (tedy hello okno s názvem hello hello vybrané aplikace ve hello title), vyberte **vlastnosti**.

    ![Výběr hello příkaz všechny aplikace](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Na hello ***appname*** - **vlastnosti** vyberte **ne** pro **povolit pro uživatele v toosign?**.
8. Vyberte hello **Uložit** příkaz.

## <a name="next-steps"></a>Další kroky
* [Zobrazení všech Moje skupin](active-directory-groups-view-azure-portal.md)
* [Přiřadit uživatele nebo skupinu tooan firemní aplikace](active-directory-coreapps-assign-user-azure-portal.md)
* [Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Změňte název hello nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
