---
title: "Zobrazení prostředků Azure přístup k přiřazení | Microsoft Docs"
description: "Zobrazit a spravovat všechna přiřazení na základě rolí řízení přístupu pro všechny uživatele nebo skupinu na portálu Azure"
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: 51b158ebfeea52e726213edd042277779f52124b
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal"></a>Zobrazení přiřazení přístupu pro uživatele a skupiny na portálu Azure
> [!div class="op_single_selector"]
> * [Správa přístupu podle uživatele nebo skupiny](role-based-access-control-manage-assignments.md)
> * [Správa přístupu podle prostředku](role-based-access-control-configure.md)

Pomocí řízení přístupu na základě rolí (RBAC) ve službě Azure Active Directory (Azure AD) můžete spravovat přístup k prostředkům Azure. 

Přístup přiřazeny RBAC je podrobných, protože existují dva způsoby, můžete omezit oprávnění:

* **Obor:** přiřazení rolí pro RBAC jsou omezená na konkrétní předplatné, skupinu prostředků nebo prostředek. Uživatel poskytnut přístup k jedinému prostředku nemůže získat přístup k žádné další prostředky ve stejném předplatném.
* **Role:** v rámci oboru přiřazení přístupu zúžit ještě i přiřazení role. Role může být vysoké úrovně, jako je vlastník, nebo konkrétní, jako je virtuální počítač čtečky.

Role můžete přiřadit pouze z v rámci předplatné, skupinu prostředků nebo prostředek, který je v rozsahu přiřazení. Ale můžete zobrazit všechna přiřazení přístupu pro daného uživatele nebo skupinu na jednom místě. Můžete mít až 2000 přiřazení rolí v každém předplatném. 

Další informace o tom, jak [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](role-based-access-control-configure.md).

## <a name="view-access-assignments"></a>Přiřazení přístupu zobrazení
K vyhledání přiřazení přístupu pro jednoho uživatele nebo skupinu, spustit v Azure Active Directory v [portál Azure](http://portal.azure.com).

1. Vyberte **Azure Active Directory**. Pokud tato možnost není zobrazena v seznamu navigace, vyberte **více služeb** a posuňte se dolů najít **Azure Active Directory**.
2. Vyberte **uživatelů a skupin**a potom buď **všichni uživatelé** nebo **všechny skupiny**. V tomto příkladu zaměříme na jednotlivé uživatele.
    ![Správa uživatelů a skupin v Azure Active Directory – snímek obrazovky](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Vyhledat uživatele podle názvu nebo uživatelské jméno.
4. V okně uživatele vyberte **Prostředky Azure**. Zobrazí všechna přiřazení přístupu pro tohoto uživatele.

### <a name="read-permissions-to-view-assignments"></a>Oprávnění ke čtení pro zobrazení přiřazení
Tato stránka se zobrazí pouze přiřazení přístupu, které máte oprávnění ke čtení. Například oprávnění ke čtení pro předplatné A a přejděte do okna prostředků Azure zkontrolujte přiřazení uživatele. Můžete zobrazit jeho přiřazení přístupu pro předplatné A, ale nemůžete zobrazit také, že uživatel je přiřazení přístupu na základě předplatného B.

## <a name="delete-access-assignments"></a>Odstranit přiřazení přístupu
V tomto okně můžete odstranit přiřazení přístupu, které byly přiřazeny přímo na uživatele nebo skupinu. Pokud přístup přiřazení byla zděděna od nadřazené skupiny, budete muset přejít do prostředků nebo předplatného a spravovat přiřazení existuje.

1. Seznam všech přiřazení přístupu pro uživatele nebo skupinu vyberte ten, který chcete odstranit.
2. Vyberte **odebrat** a potom **Ano** k potvrzení.
    ![Odebrat přiřazení přístupu – snímek obrazovky](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="next-steps"></a>Další kroky

* Začínáme s řízením přístupu na základě rolí k [použití přiřazení rolí ke správě přístupu k prostředkům předplatného Azure](role-based-access-control-configure.md)
* Další informace najdete v článku [Vestavěné role řízení přístupu na základě role v Azure](role-based-access-built-in-roles.md).

