---
title: "vlastní domény tooa aaaAssign uživatele v Azure Active Directory | Microsoft Docs"
description: "Jak toopopulate vlastní doménu v Azure Active Directory s uživatelskými účty."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 717b5a7c-7bc3-4ab1-98b5-4740b53338fe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 23c338a361a90fddf42d4df90db94c9774305886
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-users-tooa-custom-domain"></a>Přiřazení uživatelů tooa vlastní domény
Po přidání tooAzure vaši vlastní doménu služby Active Directory, je nutné přidat hello uživatelské účty pro tuto doménu tak, abyste ji mohli začít, je ověřování.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak toomanage vaše názvů domén v Centru pro správu hello Azure AD, najdete v části [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Synchronizované z místního adresáře uživatelů
Pokud jste již vytvořili připojení mezi místní služby Active Directory a Azure Active Directory, může synchronizace vyplnit hello účty. Další informace o tom, jak toosynchronize Azure Active Directory s vaší místní služby Active Directory, najdete v části [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-hello-cloud"></a>Uživatelé přidat a spravovat v cloudu hello
doména hello toochange pro existující účet uživatele:

1. Otevřete hello portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.
2. Otevřete svůj adresář.
3. Vyberte hello **uživatelé** kartě.
4. Vyberte hello uživatele ze seznamu hello.
5. Změnit doménu hello hello uživatele a pak vyberte **Uložit**.

To můžete také provést pomocí [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) nebo hello [rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Vyberte vlastní domény při vytváření nového uživatele
1. Otevřete hello portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.
2. Otevřete svůj adresář.
3. Vyberte hello **uživatelé** kartě.
4. V řádku nabídek hello vyberte **přidat**.
5. Když přidáte hello uživatelské jméno, zvolte vlastní domény hello hello domény seznamu.
6. Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
* [Použití vlastní doménu názvy toosimplify hello přihlašování pro vaše uživatele](active-directory-add-domain.md)
* [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
* [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)

