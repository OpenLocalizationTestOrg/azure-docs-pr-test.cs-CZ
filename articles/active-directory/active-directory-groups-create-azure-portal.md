---
title: "aaaCreate skupinu pro uživatele v Azure Active Directory | Microsoft Docs"
description: "Jak toocreate skupinu v Azure Active Directory a přidání členů skupiny toohello"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
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
ms.openlocfilehash: fc583a7f02ce50e7f3b2c8f97a9c032a3e2dc33a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Vytvořte skupinu a přidejte členy v Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Portál Azure Classic](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Tento článek vysvětluje, jak toocreate a naplnit nové skupiny v Azure Active Directory. Použití úlohy správy skupiny tooperform tak, jako je například přiřazení licence nebo oprávnění tooa počtu uživatelů nebo zařízení najednou.

## <a name="how-do-i-create-a-group"></a>Vytvoření skupiny
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **uživatele a skupiny** v hello textového pole a pak vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Na hello **uživatelů a skupin** vyberte **všechny skupiny**.

   ![Okno skupiny otevírání hello](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Na hello **uživatelé a skupiny - všechny skupiny** okně, vyberte hello **přidat** příkaz.

   ![Výběrem příkazu přidat hello](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Na hello **skupiny** okně Přidat název a popis skupiny hello.
6. tooselect členy tooadd toohello skupiny, vyberte **přiřazeno** v hello **typ členství** a pak vyberte **členy**. Další informace o tom, jak toomanage dynamicky hello členství ve skupině najdete v tématu [pomocí atributů toocreate advanced pravidla pro členství ve skupině](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Výběr členů tooadd](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Na hello **členy** okně, vyberte jednu nebo více uživatelů nebo zařízení tooadd toohello skupinu a vyberte hello **vyberte** tlačítko hello dolní části okna tooadd hello je toohello skupiny. Hello **uživatele** pole filtry hello zobrazení na základě shody se vaše vstupní tooany součástí názvu uživatele nebo zařízení. V tomto poli, jsou přijaty žádné zástupné znaky.
8. Po dokončení přidávání členů toohello skupiny, vyberte **vytvořit** na hello **skupiny** okno.    

   ![Vytvoření skupiny potvrzení](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Další kroky
Následující články poskytují další informace o službě Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
