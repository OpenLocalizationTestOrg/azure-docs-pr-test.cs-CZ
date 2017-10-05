---
title: "Správa skupin vaše skupina patří v Azure Active Directory | Microsoft Docs"
description: "Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory. Chcete-li spravovat jejich členství."
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
ms.openlocfilehash: 08e04a6590176c4084ca47b4bd6cbb22500eca2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Spravovat do skupiny, které patří skupiny v klientovi služby Azure Active Directory
Skupiny mohou obsahovat jiné skupiny ve službě Azure Active Directory. Chcete-li spravovat jejich členství.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Jak lze najít Moje skupina je členem skupiny?
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. Na **uživatelů a skupin** vyberte **všechny skupiny**.

   ![Otevření okna skupiny](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. Na **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.
5. Na **skupiny - *groupname***  vyberte **členství ve skupinách**.

   ![Otevření okna členství ve skupině](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. Přidání skupiny jako členem jiné skupiny, na **skupiny – členství ve skupinách** okně, vyberte **přidat** příkaz.
7. Vyberte skupinu z **vybrat skupinu** a pak vyberte **vyberte** tlačítko v dolní části okna. Skupiny můžete přidat pouze do jedné skupiny najednou. **Uživatele** pole filtry zobrazení na základě shody zadání vašeho jakékoliv části názvu uživatele nebo zařízení. V tomto poli, jsou přijaty žádné zástupné znaky.

   ![Přidat členství ve skupině](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Odebrání skupiny jako členem jiné skupiny, na **skupiny – členství ve skupinách** okně vyberte skupinu.
9. Na ***groupname*** okně, vyberte **odebrat** příkazů a potvrďte svou volbu příkazového řádku.

   ![Odeberte příkaz členství](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Po dokončení změn členství ve skupinách pro skupinu, vyberte **Uložit**.

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidávání členů](active-directory-groups-create-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
