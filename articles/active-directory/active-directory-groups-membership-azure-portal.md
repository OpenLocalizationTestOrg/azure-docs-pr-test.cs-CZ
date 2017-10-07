---
title: "skupiny hello aaaManage vaše skupina patří tooin Azure Active Directory | Microsoft Docs"
description: "Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory. Tady je způsob toomanage jejich členství."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0a0a1967084de0968e1e802559f9cdfd7ca6ae4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-toowhich-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Správa skupin toowhich, ke které patří skupiny v klientovi služby Azure Active Directory
Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory. Tady je způsob toomanage jejich členství.

## <a name="how-do-i-find-hello-groups-my-group-is-a-member-of"></a>Jak lze najít hello skupiny, které Moje skupina je členem skupiny?
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. Na hello **uživatelů a skupin** vyberte **všechny skupiny**.

   ![Okno skupiny otevírání hello](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. Na hello **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.
5. Na hello **skupiny - *groupname***  vyberte **členství ve skupinách**.

   ![Okno členství skupiny otevírání hello](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. tooadd skupině jako členem jiné skupiny, na hello **skupiny – členství ve skupinách** okně, vyberte hello **přidat** příkaz.
7. Vyberte skupinu z hello **vybrat skupinu** okna a potom vyberte hello **vyberte** tlačítko v hello dolní části okna hello. Najednou můžete přidat vaší skupiny tooonly jednu skupinu. Hello **uživatele** pole filtry hello zobrazení na základě shody se vaše vstupní tooany součástí názvu uživatele nebo zařízení. V tomto poli, jsou přijaty žádné zástupné znaky.

   ![Přidat členství ve skupině](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. tooremove skupině jako členem jiné skupiny, na hello **skupiny – členství ve skupinách** okně vyberte skupinu.
9. Na hello ***groupname*** okně, vyberte hello **odebrat** příkazů a potvrďte svou volbu příkazového řádku hello.

   ![Odeberte příkaz členství](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Po dokončení změn členství ve skupinách pro skupinu, vyberte **Uložit**.

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidávání členů](active-directory-groups-create-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
