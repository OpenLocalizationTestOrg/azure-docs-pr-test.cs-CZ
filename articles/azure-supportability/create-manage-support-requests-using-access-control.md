---
title: "Řízení přístupu na základě Role (RBAC) toocontrol aaaAzure přístup toocreate práva a spravovat žádosti o podporu | Microsoft Docs"
description: "Azure toocontrol řízení přístupu na základě rolí (RBAC) přístup k toocreate práva a správa žádostí o podporu"
author: ganganarayanan
ms.author: gangan
ms.date: 1/31/2017
ms.topic: article
ms.service: microsoft-docs
ms.assetid: 58a0ca9d-86d2-469a-9714-3b8320c33cf5
ms.openlocfilehash: c68a699ac870fa6bf371deb8ed0424848f39acf0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-based-access-control-rbac-toocontrol-access-rights-toocreate-and-manage-support-requests"></a>Azure toocontrol řízení přístupu na základě rolí (RBAC) přístup k toocreate práva a správa žádostí o podporu

[Řízení přístupu na základě rolí (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) umožňuje vyladění správy přístupu pro Azure.
Při vytváření žádosti o podporu v hello portál Azure, [portal.azure.com](https://portal.azure.com), používá toodefine Azure RBAC modelu, který můžete vytvořit a spravovat žádosti o podporu.
Přístup uděluje přiřazením toousers hello odpovídající RBAC role, skupiny a aplikace v určité obor, což může být předplatné, skupinu prostředků nebo prostředek.

Podívejme příklad: jako vlastník skupiny prostředků s oprávněními ke čtení v oboru hello předplatné, můžete spravovat všechny prostředky hello pod hello skupinu prostředků, jako jsou weby, virtuální počítače a podsítě.
Ale když zkusíte toocreate žádost o podporu pro prostředek virtuálního počítače hello, narazíte hello následující chyba

![Chyba odběru](./media/create-manage-support-requests-using-access-control/subscription-error.png)

Ve správě žádosti o podporu potřebujete oprávnění zapisovat nebo role, kterou má hello podporují akci Microsoft.Support/* v hello předplatné oboru toobe možné toocreate a spravovat žádosti o podporu.

Hello následující článek vysvětluje, jak můžete použít vlastní toocreate řízení přístupu na základě Role (RBAC) Azure a spravovat žádosti o podporu v hello portálu Azure.

## <a name="getting-started"></a>Začínáme

Pomocí hello příkladu výše, by byl schopný toocreate žádosti o podporu pro prostředek Pokud vlastní role RBAC v předplatném hello se přiřadila hello vlastník předplatného.
[Vlastní role RBAC](https://azure.microsoft.com/documentation/articles/role-based-access-control-custom-roles/) lze vytvořit pomocí Azure PowerShell, rozhraní příkazového řádku Azure (CLI) a hello REST API.

Vlastnost akce Hello vlastní role určuje, zda text hello Azure operations toowhich hello role uděluje přístup.
toocreate vlastní role pro správu žádosti o podporu, musí mít role, hello hello akce Microsoft.Support/*

Tady je příklad vlastní roli, můžete použít toocreate a spravovat žádosti o podporu.
Jsme jste s názvem "Přispěvatel žádosti o podporu" této role, kterým je, jak označujeme toohello vlastní role v tomto článku.

``` Json
{
    "Name":  "Support Request Contributor",
    "Id":  "1f2aad59-39b0-41da-b052-2fb070bd7942",
    "IsCustom":  true,
    "Description":  "Lets you create and manage support tickets.",
    "Actions":  [
                    "Microsoft.Support/*"
                ],
    "NotActions":  [
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Postupujte podle kroků hello uvedených v [toto video](https://www.youtube.com/watch?v=-PaBaDmfwKI) toolearn jak toocreate vlastní role pro vaše předplatné.

## <a name="create-and-manage-support-requests-in-hello-azure-portal"></a>Vytvoření a správa žádostí o podporu v hello portálu Azure

Podívejme příklad – jsou hello vlastníka předplatného "Visual Studio předplatné MSDN."
Jan je vaše partnera, který je toosome vlastníka prostředku hello skupin prostředků v tomto předplatném a má předplatné toohello oprávnění ke čtení.
Chcete toogive přístup tooyour rovnocenný, Jan, hello možnost toocreate a spravovat lístky žádostí o podporu pro hello prostředky pod tímto předplatným.

1. prvním krokem Hello je toogo toohello předplatného a v části "Nastavení" zobrazí seznam uživatelů. Klikněte na uživatele hello Jan, kdo má přístup čtečky na hello předplatného a umožňuje přiřadit nové toohim vlastní role.

    ![Přidání role](./media/create-manage-support-requests-using-access-control/add-role.png)

2. V okně "Uživatelé" hello, klikněte na tlačítko "Přidat". Vyberte vlastní roli hello "Přispěvatel žádosti o podporu" hello seznamu rolí

    ![Přidání role Přispěvatel podpory](./media/create-manage-support-requests-using-access-control/add-support-contributor-role.png)

3. Po výběru hello název role, klikněte na tlačítko "Přidat uživatele" a zadejte přihlašovací údaje hello Jan e-mailu. Klikněte na tlačítko "Vyberte"

    ![Přidání uživatelů](./media/create-manage-support-requests-using-access-control/add-users.png)

4. Klikněte na tlačítko "Ok" tooproceed

    ![Přidat přístup](./media/create-manage-support-requests-using-access-control/add-access.png)

5. Nyní se zobrazí hello uživatele s hello nově přidané vlastní role "Podporu požadavků Přispěvatel" v rámci předplatného hello, pro který jste vlastník hello

    ![Přidat uživatele](./media/create-manage-support-requests-using-access-control/user-added.png)

    Jan přihlásí hello portál, zjistí toowhich hello předplatné, které mu byla přidána.

7. Jan klikne na možnost "Nová žádost o podporu" v okně "Nápověda a podpora" hello a můžete vytvořit žádosti o podporu pro "Visual Studio Ultimate s MSDN"

    ![Nová žádost o podporu](./media/create-manage-support-requests-using-access-control/new-support-request.png)

8. Kliknutím na tlačítko "Podporují všechny požadavky" Jan uvidí hello seznam žádostí o podporu pro toto předplatné vytvořit ![případ zobrazení podrobností](./media/create-manage-support-requests-using-access-control/case-details-view.png)

## <a name="remove-support-request-access-in-hello-azure-portal"></a>Odebrat podporu požádat o přístup v hello portálu Azure

Stejně, jako je možné toogrant přístup tooa uživatele toocreate a spravovat žádosti o podporu, je možné tooremove přístupu pro uživatele hello také.
tooremove hello možnost toocreate a spravovat žádosti o podporu, přejděte toohello předplatné, klikněte na tlačítko "Nastavení" a klikněte na hello uživateli (v tomto případě Jan).
Klikněte pravým tlačítkem na název role hello, "Přispěvatel žádosti o podporu" a klikněte na tlačítko "Odebrat"

![Odebrat přístup žádosti o podporu](./media/create-manage-support-requests-using-access-control/remove-support-request-access.png)

Když Jan protokoly portálu toohello a pokusí toocreate žádosti o podporu, mohl zaznamená hello následující chyba

![Předplatné chyba-2](./media/create-manage-support-requests-using-access-control/subscription-error-2.png)

Jan nelze zobrazit žádné podporovat požadavky, pokud klikne "Podporují všechny požadavky"

![zobrazení podrobností případu-2](./media/create-manage-support-requests-using-access-control/case-details-view-2.png)
