---
title: "Zakázat přihlášení uživatele pro podnikové aplikace v Azure Active Directory | Microsoft Docs"
description: "Zakázání podniková aplikace tak, aby žádní uživatelé můžou přihlásit k němu v Azure Active Directory"
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
ms.openlocfilehash: 5d27046370eada0c371c94fb573fa1bcf536f7cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Zakázat přihlášení uživatele pro podnikové aplikace v Azure Active Directory
Je snadné zakázat podniková aplikace tak, aby žádní uživatelé můžou přihlásit k němu v Azure Active Directory (Azure AD). Musí mít příslušná oprávnění ke správě firemní aplikace a musí být globální správce adresáře.

## <a name="how-do-i-disable-user-sign-ins"></a>Jakým způsobem vypnout uživatelská přihlášení?
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v textovém poli a potom vyberte **Enter**.
3. Na **Azure Active Directory** -  ***directoryname*** okno (to znamená, Azure AD okna pro adresář spravujete), vyberte **podnikové aplikace, které**.

    ![Otevírání podnikové aplikace](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)
4. Na **podnikové aplikace, které** vyberte **všechny aplikace**. Zobrazí seznam aplikací, které můžete spravovat.
5. Na **podnikové aplikace – všechny aplikace** okně, vyberte aplikaci.
6. Na ***appname*** okno (to znamená, v okně s názvem vybranou aplikaci v názvu), vyberte **vlastnosti**.

    ![Výběr příkaz všechny aplikace](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)
7. Na ***appname*** - **vlastnosti** vyberte **ne** pro **povolit pro uživatele přihlásit?**.
8. Vyberte **Uložit** příkaz.

## <a name="next-steps"></a>Další kroky
* [Zobrazení všech Moje skupin](active-directory-groups-view-azure-portal.md)
* [Přiřazení uživatele nebo skupiny do aplikace enterprise](active-directory-coreapps-assign-user-azure-portal.md)
* [Odebrat uživatele nebo skupinu přiřazení z podnikové aplikace.](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Změna názvu nebo logo aplikace enterprise](active-directory-coreapps-change-app-logo-user-azure-portal.md)
