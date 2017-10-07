---
title: "přiřazení přístupu prostředků Azure aaaView | Microsoft Docs"
description: "Zobrazit a spravovat všechna přiřazení hello řízení přístupu na základě Role u uživatele nebo skupiny v hello portálu Azure"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: ec96c9d4b9e2cb4925949b8bac78767bd564c8ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-hello-azure-portal"></a>Zobrazení přiřazení přístupu pro uživatele a skupiny v hello portálu Azure
> [!div class="op_single_selector"]
> * [Správa přístupu podle uživatele nebo skupiny](role-based-access-control-manage-assignments.md)
> * [Správa přístupu podle prostředku](role-based-access-control-configure.md)

Pomocí řízení přístupu na základě rolí (RBAC) v hello Azure Active Directory (Azure AD), můžete spravovat přístup tooyour Azure prostředky. 

Přístup přiřazeny RBAC je podrobných, protože můžete omezit oprávnění hello dvěma způsoby:

* **Obor:** přiřazení rolí pro RBAC jsou vymezená tooa konkrétní předplatné, skupinu prostředků nebo prostředek. Uživatel zadaný přístup tooa jediný zdroj nelze přístup k jiným prostředkům v hello stejného předplatného.
* **Role:** v rámci oboru hello hello přiřazení, je přístup zúžit ještě i přiřazení role. Role může být vysoké úrovně, jako je vlastník, nebo konkrétní, jako je virtuální počítač čtečky.

Role můžete přiřadit pouze z v rámci hello předplatné, skupinu prostředků nebo prostředek, který je hello oboru pro přiřazení hello. Ale můžete zobrazit všechna přiřazení hello přístup pro daného uživatele nebo skupinu na jednom místě. Může obsahovat maximálně too2000 přiřazení rolí v každém předplatném. 

Získat další informace o příliš[používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](role-based-access-control-configure.md).

## <a name="view-access-assignments"></a>Přiřazení přístupu zobrazení
toolook až hello přiřazení přístupu pro jednoho uživatele nebo skupinu, spustit v Azure Active Directory v hello [portál Azure](http://portal.azure.com).

1. Vyberte **Azure Active Directory**. Pokud tato možnost není zobrazena v seznamu navigace, vyberte **více služeb** a posuňte se dolů toofind **Azure Active Directory**.
2. Vyberte **uživatelů a skupin**a potom buď **všichni uživatelé** nebo **všechny skupiny**. V tomto příkladu zaměříme na jednotlivé uživatele.
    ![Správa uživatelů a skupin v Azure Active Directory – snímek obrazovky](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Hledat hello uživatelské jméno nebo uživatelské jméno.
4. Vyberte **prostředky Azure** v okně hello uživatele. Zobrazí všechna přiřazení hello přístup pro daného uživatele.

### <a name="read-permissions-tooview-assignments"></a>Oprávnění ke čtení tooview přiřazení
Tato stránka zobrazuje pouze přiřazení přístupu hello, zda máte oprávnění tooread. Například máte toosubscription přístup pro čtení A a přejděte toohello prostředky Azure okno toocheck přiřazení uživatele. Můžete zobrazit jeho přiřazení přístupu pro předplatné A, ale nemůžete zobrazit také, že uživatel je přiřazení přístupu na základě předplatného B.

## <a name="delete-access-assignments"></a>Odstranit přiřazení přístupu
V tomto okně můžete odstranit přiřazení přístupu, které byly přímo přiřazeny tooa uživatele nebo skupinu. Pokud přiřazení přístupu hello byla zděděna od nadřazené skupiny, můžete potřebovat toogo toohello prostředků nebo předplatného a spravovat hello přiřazení došlo.

1. Hello seznamu všech hello přiřazení přístupu pro uživatele nebo skupinu vyberte hello jeden chcete toodelete.
2. Vyberte **odebrat** a potom **Ano** tooconfirm.
    ![Odebrat přiřazení přístupu – snímek obrazovky](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="next-steps"></a>Další kroky

* Začínáme s řízením přístupu na základě rolí příliš[používat roli přiřazení toomanage přístup tooyour předplatného Azure prostředky](role-based-access-control-configure.md)
* V tématu hello [předdefinované role RBAC](role-based-access-built-in-roles.md)

