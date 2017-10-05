---
title: "Přidání nových uživatelů do služby Azure Active Directory | Dokumentace Microsoftu"
description: "Vysvětluje, jak ve službě Azure Active Directory přidat nové uživatele nebo změnit informace o uživatelích."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: bfe0c556d94d50207a23d2e3984371fb602e9406
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-new-users-to-azure-active-directory"></a>Přidání nových uživatelů do služby Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-users-create-azure-portal.md)
> * [Portál Azure Classic](active-directory-create-users.md)
>
>

Tento článek vysvětluje, jak přidat nové uživatele ve vaší organizaci ve službě Azure Active Directory (Azure AD). 

1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Otevírání uživatelů a skupin](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. Na **uživatelů a skupin** vyberte **všichni uživatelé**a potom vyberte **přidat**.

   ![Použití příkazu Přidat](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. Zadejte podrobnosti pro uživatele, jako například **název** a **uživatelské jméno**. Část názvu domény uživatelské jméno musí být buď počáteční výchozí název domény "foo.onmicrosoft.com" název domény nebo název domény ověřené, nefederovaných například "contoso.com".
5. Zkopírujte nebo jinak Poznámka: hesla generovaného uživatele tak, aby ji mohli nabídnout uživatele po dokončení tohoto procesu.
6. Volitelně můžete otevřít a vyplňte informace v **profil** okně **skupiny** okno, nebo **Directory role** okno pro uživatele. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).
7. Na **uživatele** vyberte **vytvořit**.
8. Bezpečně distribuujte vygenerované heslo pro nového uživatele tak, aby uživatel moct přihlásit.

### <a name="next-steps"></a>Další kroky
* [Přidat externího uživatele](active-directory-users-create-external-azure-portal.md)
* [Resetovat heslo uživatele v nový portál Azure](active-directory-users-reset-password-azure-portal.md)
* [Změnit informace o práci uživatele](active-directory-users-work-info-azure-portal.md)
* [Správa uživatelských profilů](active-directory-users-profile-azure-portal.md)
* [Odstranění uživatele ve službě Azure AD](active-directory-users-delete-user-azure-portal.md)
* [Přiřazení uživatele k roli ve službě Azure AD](active-directory-users-assign-role-azure-portal.md)
