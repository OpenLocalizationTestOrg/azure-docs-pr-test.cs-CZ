---
title: "aaaManage hello členy skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Jak tooadd nebo odebrat uživatele a zařízení ze skupiny ve službě Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4cb16ee63828003da251423a04736f7174dd4896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Správa členství ve skupině pro uživatele v klientovi služby Azure Active Directory
Tento článek vysvětluje, jak toomanage hello členy skupiny ve službě Azure Active Directory (Azure AD).

## <a name="how-do-i-find-hello-members-and-manage-them"></a>Jak najít hello členy a spravovat?
1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Na hello **uživatelů a skupin** vyberte **všechny skupiny**.

   ![Okno skupiny otevírání hello](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Na hello **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.
5. Na hello **skupiny - *groupname***  vyberte **členy**.

   ![Otevírání hello členy okno](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. tooadd členy toohello skupiny, na hello **skupiny - Členové** vyberte **přidat členy**.

   ![Přidat členy – příkaz](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Na hello **členy** okně, vyberte jednu nebo více uživatelů nebo zařízení tooadd toohello skupinu a vyberte hello **vyberte** tlačítko hello dolní části okna tooadd hello je toohello skupiny. Hello **uživatele** pole filtry hello zobrazení na základě shody se vaše vstupní tooany součástí názvu uživatele nebo zařízení. V tomto poli, jsou přijaty žádné zástupné znaky.
8. tooremove členy ze skupiny hello na hello **skupiny - Členové** okně vyberte člena.
9. Na hello ***membername*** okně, vyberte hello **odebrat** příkazů a potvrďte svou volbu příkazového řádku hello.

   ![odebrat členy příkaz](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Po dokončení změn členy hello skupiny, vyberte **Uložit**.

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidávání členů](active-directory-groups-create-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
