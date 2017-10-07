---
title: "aaaHow toogive přístup tooPrivileged Identity Management - Azure | Microsoft Docs"
description: "Zjistěte, jak tooadd role toousers s hello rozšíření Azure Active Directory Privileged Identity Management, tak můžou spravovat PIM."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 5d99589af4af766e430d7cecd743ace752f63768
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="giving-access-toomanage-azure-ad-privileged-identity-management"></a>Udělení přístupu toomanage Azure AD Privileged Identity Management
Hello globální správce, který umožňuje Azure AD Privileged Identity Management (PIM) pro organizaci automaticky získat přiřazení rolí a přístup k tooPIM. Nikdo jiný získá přístup pro zápis ve výchozím nastavení, ale včetně další globální správce. Další globální správci, správce zabezpečení a zabezpečení čtečky mají oprávnění jen pro čtení tooAzure AD PIM. toogive tooPIM přístup, hello prvního uživatele můžete přiřadit jiné toohello **správce privilegovaných rolí** role. Toto přiřazení musí provádět v PIM sám a nelze změnit pomocí prostředí PowerShell nebo dalších portálech.

> [!NOTE]
> Správa Azure AD PIM vyžaduje Azure MFA. Vzhledem k tomu, že účty Microsoft nelze zaregistrovat pro Azure MFA, uživatel, který se přihlásí pomocí účtu Microsoft přístup k Azure AD PIM.
> 
> 

Zkontrolujte, že jsou vždy alespoň dva uživatelé v roli správce privilegovaných rolí v případě, že jeden uživatel je uzamčen nebo jejich účet je odstraněný.

## <a name="give-another-user-access-toomanage-pim"></a>Zadejte jiný uživatel přístup toomanage PIM
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) a vyberte hello **Azure AD Privileged Identity Management** aplikace na řídicím panelu hello.
2. Vyberte **spravovat privilegované role** > **správce privilegovaných rolí** > **přidat**.
   
    ![Přidání privilegované role správců – snímek obrazovky][1]
3. V okně spravované uživatele hello přidat je již krok 1 dokončena. Vyberte krok 2, **vyberte uživatele,** a vyhledávání pro uživatele hello chcete tooadd.
   
    ![Vyberte uživatele – snímek obrazovky][2]
4. Vyberte uživatele hello hello výsledky hledání a klikněte na **provádí**.
5. Klikněte na tlačítko **OK** toosave váš výběr. Hello uživatele, které jste vybrali, se zobrazí v seznamu hello privilegované role správců.
   
   * Vždy, když přiřadíte nové toosomeone role, budou se automaticky nastaví jako oprávněné tooactivate hello role. Pokud chcete, aby toomake je trvalé hello role, klikněte na tlačítko hello uživatele v seznamu hello. Vyberte **zkontrolujte oprávnění** v nabídce hello uživatelské informace.
6. Odeslat hello uživatele odkaz příliš[Začínáme s Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>Odeberte jiné uživatelská přístupová práva pro správu PIM
Před odebráním někdo z role správce privilegovaných rolí hello, vždy zajistěte, aby stále bude přiřazeno tooit dva uživatelů.

1. V hello PIM řídicí panel, klikněte na roli hello **správce privilegovaných rolí**.  Zobrazí se seznam Hello uživatelů aktuálně v dané roli.
2. Klikněte na hello uživatele v seznamu uživatelů hello.
3. Klikněte na **odebrat**.  Se zobrazí potvrzovací zpráva.
4. Klikněte na tlačítko **Ano** tooremove hello uživatele z hello role.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
