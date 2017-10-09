---
title: "Řízení přístupu na základě aaaRole v hello portálu Azure | Microsoft Docs"
description: "Začínáme se správou přístupu pomocí řízení přístupu na základě rolí v hello portálu Azure. Použijte roli přiřazení tooassign oprávnění tooyour prostředky."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b87e00089b0fc93fb212b318330a6f22bfbf59e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-access-tooyour-azure-subscription-resources"></a>Používat řízení přístupu na základě Role toomanage přístup tooyour předplatného Azure prostředky
> [!div class="op_single_selector"]
> * [Správa přístupu podle uživatele nebo skupiny](role-based-access-control-manage-assignments.md)
> * [Správa přístupu podle prostředku](role-based-access-control-configure.md)

Řízení přístupu na základě role v Azure umožňuje přesnou správu přístupu. Pomocí RBAC, můžete udělit pouze hello množství přístup tito uživatelé si musí tooperform svou práci. Tento článek vám pomůže provozu s RBAC v hello portálu Azure. Další informace o tom, jak vám řízení přístupu na základě role v Azure pomůže spravovat přístup uživatelů najdete v článku [Co je řízení přístupu na základě role](role-based-access-control-what-is.md).

V rámci každé předplatné můžete udělit too2000 přiřazení rolí. 

## <a name="view-access"></a>Zobrazení informací o přístupu
Můžete zjistit, kdo má přístup tooa prostředku, skupiny prostředků nebo předplatného v hlavním okně v hello [portál Azure](https://portal.azure.com). Například chceme toosee, který má přístup tooone ze skupin prostředků:

1. Vyberte **skupiny prostředků** hello navigačním panelu na levé straně hello.  
    ![Skupiny prostředků – ikona](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Vyberte hello název skupiny prostředků hello z hello **skupiny prostředků** okno.
3. Vyberte **přístup k ovládacímu prvku (IAM)** hello levé nabídce.  
4. okno pro řízení přístupu Hello obsahuje seznam všech uživatelů, skupin a aplikací, kterým byl udělen přístup skupiny prostředků toohello.  
   
    ![Snímek obrazovky s oknem uživatelé – zděděný a přiřazený přístup](./media/role-based-access-control-configure/view-access.png)

Všimněte si, že některé role jsou vymezeny příliš**tento prostředek** zase **zděděné** z jiného oboru. Přístup je přiřazen konkrétně toohello skupinu prostředků nebo zděděná z nadřazené předplatného toohello přiřazení.

> [!NOTE]
> Classic správci a pomocní správci jsou považovány za vlastníky hello předplatného v novém modelu RBAC hello.

## <a name="add-access"></a>Přidání přístupu
Udělíte přístup z v rámci hello prostředku, skupině prostředků nebo předplatného, které je hello rozsah přiřazení role hello.

1. Vyberte **přidat** v okně řízení přístupu hello.  
2. Vyberte hello role, které chcete tooassign z hello **vyberte roli** okno.
3. Vyberte v adresáři, který chcete toogrant přístup k hello uživatele, skupiny nebo aplikace. Můžete hledat hello adresář se zobrazovaných názvů, e-mailové adresy a identifikátory objektů.  
   
    ![Snímek obrazovky s oknem Přidat uživatele – vyhledávání](./media/role-based-access-control-configure/grant-access2.png)
4. Vyberte **OK** toocreate hello přiřazení. Hello **přidává se uživatel** otevřeném hello průběh.  
    ![Ukazatel průběhu přidávání uživatele – snímek obrazovky](./media/role-based-access-control-configure/addinguser_popup.png)

Po úspěšném přiřazení role, zobrazí se v hello **uživatelé** okno.

## <a name="remove-access"></a>Odebrání přístupu
1. Ukazatele myši hello název hello přiřazení, které chcete tooremove. Zaškrtávací políčko se zobrazí další toohello název.
2. Pomocí zaškrtávacích políček tooselect hello jeden nebo více přiřazení rolí.
2. Vyberte **Odebrat**.  
3. Vyberte **Ano** tooconfirm hello odebrání.

Zděděná přiřazení nelze odebrat. Pokud potřebujete tooremove zděděná přiřazení, je nutné toodo v hello oboru, které bylo vytvořeno hello přiřazení role. V hello **oboru** sloupce, další příliš**zděděné** je odkaz, který přejdete toohello prostředky kde byl přiřazen této role. Přejděte v seznamu uvedeno prostředků toohello přiřazení role tooremove hello.

![Snímek obrazovky s oknem Uživatelé – neaktivní tlačítko odebrání u zděděného přístupu](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-toomanage-access"></a>Jiný přístup toomanage nástroje
Můžete přiřadit role a spravovat přístup pomocí příkazů Azure RBAC v jiných nástrojích než hello portálu Azure.  Postupujte podle hello odkazy toolearn více informací o hello požadavky a začít pracovat s příkazy Azure RBAC hello.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Rozhraní příkazového řádku Azure](role-based-access-control-manage-access-azure-cli.md)
* [REST API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Další kroky
* [Vytvoření sestavy historie změn přístupu](role-based-access-control-access-change-history-report.md)
* V tématu hello [předdefinované role RBAC](role-based-access-built-in-roles.md)
* Definujte své [Vlastní role funkce řízení přístupu na základě role v Azure](role-based-access-control-custom-roles.md).

