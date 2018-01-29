---
title: "Vytvoření skupiny pro uživatele v Azure Active Directory | Microsoft Docs"
description: "Postup vytvoření skupiny ve službě Azure Active Directory a přidání členů do skupiny"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1be9947c43c70b7248201b9f470fb3cf5a11519e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Vytvořte skupinu a přidejte členy v Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Tento článek vysvětluje, jak vytvořit a naplnit nové skupiny v Azure Active Directory. Použijte skupinu k provádění úloh správy, jako je například přiřazení licence nebo oprávnění k počtu uživatelů nebo zařízení najednou.

## <a name="how-do-i-create-a-group"></a>Vytvoření skupiny
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatele a skupiny** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Na **uživatelů a skupin** vyberte **všechny skupiny**.

   ![Otevření okna skupiny](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Na **uživatelé a skupiny - všechny skupiny** okně, vyberte **přidat** příkaz.

   ![Použití příkazu Přidat](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Na **skupiny** okně Přidat název a popis skupiny.
6. Vyberte členy, které chcete přidat do skupiny, vyberte **přiřazeno** v **typ členství** a pak vyberte **členy**. Další informace o tom, jak dynamicky spravovat členství ve skupině najdete v tématu [vytváření rozšířených pravidel pro členství ve skupině pomocí atributů](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Výběr členů pro přidání](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Na **členy** okně, vyberte jeden nebo více uživatelů nebo zařízení přidáte do skupiny a vyberte **vyberte** tlačítko v dolní části okna je přidejte do skupiny. **Uživatele** pole filtry zobrazení na základě shody zadání vašeho jakékoliv části názvu uživatele nebo zařízení. V tomto poli, jsou přijaty žádné zástupné znaky.
8. Po dokončení přidávání členů do skupiny, vyberte **vytvořit** na **skupiny** okno.    

   ![Vytvoření skupiny potvrzení](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
