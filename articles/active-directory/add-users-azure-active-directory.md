---
title: "aaaAdd tooAzure nového uživatele služby Active Directory | Microsoft Docs"
description: "Vysvětluje, jak tooadd nové uživatele ve službě Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 6ca413c84a7a5238a30fd26fc751d687d827b24a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-new-users-tooazure-active-directory"></a>Rychlý úvod: Přidat nové uživatele tooAzure služby Active Directory
Tento článek vysvětluje, jak tooadd noví uživatelé ve vaší organizaci v Azure Active Directory (Azure AD) hello jeden současně pomocí hello portál Azure nebo synchronizaci uživatelů systému Windows Server AD místní účet data. 

## <a name="add-cloud-based-users"></a>Přidání cloudových uživatelů
1. Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **Azure Active Directory** a potom **uživatelů a skupin**.
3. Na hello **uživatelů a skupin** vyberte **všichni uživatelé**a potom vyberte **nového uživatele**.
   ![Výběrem příkazu přidat hello](./media/add-users-azure-active-directory/add-user.png)
4. Zadejte podrobnosti pro hello uživatele, jako například **název** a **uživatelské jméno**. Hello část názvu domény hello uživatelské jméno musí být buď hello počáteční výchozí domény název "[název domény].onmicrosoft.com" nebo ověřené, nefederovaných [vlastní název domény](add-custom-domain.md) například "contoso.com".
5. Kopírování nebo jinak hello Poznámka vygenerované heslo uživatele tak, aby můžete pro jeho toohello uživatele po dokončení tohoto procesu.
6. Volitelně můžete otevřít a vyplňte informace hello v hello **profil** okno, hello **skupiny** okno nebo hello **Directory role** okno pro uživatele hello. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md).
7. Na hello **uživatele** vyberte **vytvořit**.
8. Bezpečně distribuujte hello vygenerované heslo toohello nového uživatele tak, aby hello uživatel může přihlásit.

> [!TIP]
> Můžete také synchronizovat data uživatelských účtů z místního systému Windows Server AD. Řešení společnosti Microsoft identity span místní a cloudové schopnosti, vytváření identitu jednoho uživatele pro ověřování a autorizaci tooall prostředků, bez ohledu na umístění. Říkáme toto hybridní identita. [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) lze použít toointegrate místních adresářů se službou Azure Active Directory pro hybridní identity scénáře. To vám umožní tooprovide společnou identitu pro uživatele pro aplikace Office 365, Azure a SaaS integrované s Azure AD. 

## <a name="delete-users-from-azure-ad"></a>Odstranit uživatele z Azure AD
1. Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **uživatelů a skupin**.
3. Na hello **uživatelů a skupin** okně, vyberte hello toodelete uživatele ze seznamu hello. 
4. V okně hello hello vybraného uživatele, vyberte **přehled**a pak v řádku nabídek hello vyberte **odstranit**.
   ![Výběrem příkazu přidat hello](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Další informace 
* [Přidat externího uživatele](active-directory-users-create-external-azure-portal.md)

* [Přiřadit roli tooa uživatele ve službě Azure AD](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Další kroky
V tento rychlý start, když jste se naučili jak tooadd nové uživatele tooAzure AD Premium. 

Můžete použít hello následujících odkaz toocreate nového uživatele ve službě Azure AD z hello portálu Azure.

> [!div class="nextstepaction"]
> [Přidat uživatele tooAzure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
