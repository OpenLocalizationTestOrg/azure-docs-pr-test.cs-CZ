---
title: "Přiřadit uživatele k vlastní doméně v Azure Active Directory | Microsoft Docs"
description: "Postup naplnění vlastní doménu v Azure Active Directory s uživatelskými účty."
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
ms.openlocfilehash: 39cb54a6637088c35c6aef864a804c24803f48ba
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="assign-users-to-a-custom-domain"></a>Přiřazení uživatelů k vlastní doméně
Po jste přidali vlastní domény do Azure Active Directory, je nutné přidat uživatelské účty pro tuto doménu tak, abyste ji mohli začít, je ověřování.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Jak spravovat názvy domén v Centru správy služby Azure AD, najdete v článku [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="users-synced-from-a-on-premises-directory"></a>Synchronizované z místního adresáře uživatelů
Pokud jste již vytvořili připojení mezi místní služby Active Directory a Azure Active Directory, může synchronizace vyplnit účty. Další informace o tom, jak synchronizovat Azure Active Directory s vaší místní Active Directory najdete v tématu [integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Uživatelé přidat a spravovat v cloudu
Chcete-li změnit doménu pro existující účet uživatele:

1. Otevřete portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.
2. Otevřete svůj adresář.
3. Vyberte kartu **Uživatelé**.
4. Vyberte uživatele ze seznamu.
5. Změnit doménu pro uživatele a pak vyberte **Uložit**.

To můžete také provést pomocí [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) nebo [rozhraní Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Vyberte vlastní domény při vytváření nového uživatele
1. Otevřete portál Azure classic pomocí účtu, který je globální správce nebo správce uživatele.
2. Otevřete svůj adresář.
3. Vyberte kartu **Uživatelé**.
4. Na panelu příkazů vyberte **přidat**.
5. Když přidáte uživatelské jméno, zvolte ze seznamu domény vlastní doménu.
6. Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
* [Pomocí vlastních názvů domén ke zjednodušení přihlašování pro vaše uživatele](active-directory-add-domain.md)
* [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)
* [Další informace o konceptech správy domén ve službě Azure AD](active-directory-add-domain-concepts.md)

