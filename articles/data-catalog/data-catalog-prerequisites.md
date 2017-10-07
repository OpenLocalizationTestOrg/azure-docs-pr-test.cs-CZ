---
title: "požadavky aaaAzure katalogu Data Catalog | Microsoft Docs"
description: "Další informace o hello požadavky, které že budete potřebovat tooget začít s Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 0c8e768e5846c61b542b746d7ad80121725a9ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-prerequisites"></a>Požadavky na Azure Data Catalog

Před nastavením Azure Data Catalog musíte tootake péče o pár věcí. Nemusíte si dělat starosti, není tento proces trvá dlouho.

## <a name="azure-subscription"></a>Předplatné Azure
tooset do katalogu Data Catalog, musí být vlastníkem hello nebo spoluvlastník předplatného Azure.

Předplatná Azure můžete uspořádat přístup k prostředkům služby toocloud například katalogu Data Catalog. Odběry také umožňují řídit způsob hlášené, účtují a zaplacení využití prostředků. Každý odběr může mít samostatné fakturace a platebních instalace, tak může mít odběry a plány, které se liší podle oddělení, projektů, místní office a tak dále. Každé cloudové služby patří tooa předplatného a potřebujete toohave předplatné před nastavením Data Catalog. Další, najdete v části toolearn [spravovat účty, odběry a správu role](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
tooset do katalogu Data Catalog můžete musí být podepsané pomocí uživatelského účtu Azure Active Directory (Azure AD).

Azure AD poskytuje snadný způsob pro obchodní toomanage identity a přístup v hello cloudové i místní. Uživatele můžete použít pracovní nebo školní účet pro tooany přihlášení cloud a místní webové aplikace. Data Catalog používá přihlášení tooauthenticate Azure AD. Další, najdete v části toolearn [co je Azure Active Directory?](../active-directory/active-directory-whatis.md).

> [!NOTE]
> Pomocí hello [portál Azure](http://portal.azure.com/), se můžete přihlásit se pomocí osobního účtu Microsoft nebo Azure Active Directory pracovní nebo školní účet. tooset do katalogu Data Catalog pomocí buď hello portálu Azure nebo hello [katalogu Data Catalog portál](http://www.azuredatacatalog.com), musí se přihlásit účtu Azure Active Directory, nikoli osobní účet.
>
>

## <a name="active-directory-policy-configuration"></a>Zásady Konfigurace služby Active Directory
Tam, kde se můžete přihlásit toohello katalogu Data Catalog portálu, ale když se pokusíte toosign v toohello nástroj registrace zdroje dat se může nastat situace, dojde k chybovou zprávu, která brání přihlášení. Toto chování problému může dojít pouze v případě, že jste na podnikové síti hello nebo může dojít, pouze pokud se připojujete ze mimo hello podnikové síti.

Nástroj registrace zdroje dat Hello používá ověřování pomocí formulářů toovalidate pověření uživatele pro službu Active Directory. toohelp úspěšně přihlášení, Správce služby Active Directory musí povolit ověřování pomocí formulářů v hello globální zásady ověřování.

V hello globální zásady ověřování metody ověřování může být povoleno samostatně intranetu a extranetu, jak ukazuje následující snímek obrazovky hello. Pokud je ověřování pomocí formulářů není povoleno pro hello síť, ze kterého se připojujete, může dojít k chybám přihlášení.

 ![Služby Active Directory globální zásady ověřování](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).
