---
title: "Spravovat členy skupiny ve službě Azure Active Directory | Microsoft Docs"
description: "Postup přidání nebo odebrání uživatelů a zařízení ze skupiny ve službě Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 49c975aa34f28c6c00591435726e538bb79a78cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Správa členství ve skupině pro uživatele v klientovi služby Azure Active Directory
Tento článek vysvětluje, jak spravovat členy skupiny ve službě Azure Active Directory (Azure AD).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Jak najít členy a spravovat?
1. Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.
2. Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.

   ![Správa uživatelů otevírání](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. Na **uživatelů a skupin** vyberte **všechny skupiny**.

   ![Otevření okna skupiny](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. Na **uživatelé a skupiny - všechny skupiny** okně vyberte skupinu.
5. Na **skupiny - *groupname***  vyberte **členy**.

   ![Otevření okna členy](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Chcete-li přidat členy do skupiny, na **skupiny - Členové** vyberte **přidat členy**.

   ![Přidat členy – příkaz](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. Na **členy** okně, vyberte jeden nebo více uživatelů nebo zařízení přidáte do skupiny a vyberte **vyberte** tlačítko v dolní části okna je přidejte do skupiny. **Uživatele** pole filtry zobrazení na základě shody zadání vašeho jakékoliv části názvu uživatele nebo zařízení. V tomto poli, jsou přijaty žádné zástupné znaky.
8. Možnost odebrání členů ze skupiny, na **skupiny - Členové** okně vyberte člena.
9. Na ***membername*** okně, vyberte **odebrat** příkazů a potvrďte svou volbu příkazového řádku.

   ![odebrat členy příkaz](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Po dokončení změn členy do skupiny, vyberte **Uložit**.

## <a name="additional-information"></a>Další informace
Následující články poskytují další informace o službě Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Vytvoření nové skupiny a přidávání členů](active-directory-groups-create-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
