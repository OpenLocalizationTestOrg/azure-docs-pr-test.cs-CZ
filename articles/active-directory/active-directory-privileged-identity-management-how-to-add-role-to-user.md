---
title: "aaaHow tooadd nebo odebrat roli uživatele | Microsoft Docs"
description: "Zjistěte, jak hello tooadd role tooprivileged identity s aplikací Azure Active Directory Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim;oldportal;it-pro;
ms.openlocfilehash: f84639757dd76061ea12ed6ea7ec9e62ad942109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-privileged-identity-management-how-tooadd-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Jak tooadd nebo odebrat roli uživatele
S Azure Active Directory (AD), globální správce (nebo správce společnosti) můžete aktualizovat které uživatelé jsou **trvale** přiřazené tooroles ve službě Azure AD. To se provádí pomocí rutin prostředí PowerShell jako `Add-MsolRoleMember` a `Remove-MsolRoleMember`. Nebo použitím hello portál Azure classic, jak je popsáno v [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).

Hello aplikace Azure AD Privileged Identity Management umožňuje správci privilegované role toomake přiřazení rolí trvalé, také. Kromě toho privilegované role Správci uživatelé **vhodné** pro role správce. Oprávněné správce může aktivovat hello role, když je potřebují, a potom jejich oprávnění vyprší po jejich dokončení akce.

## <a name="manage-roles-with-pim-in-hello-azure-portal"></a>Správa rolí s PIM v hello portálu Azure
Ve vaší organizaci můžete přiřadit uživatele toodifferent správní role ve službě Azure AD, Office 365 a dalších Microsoft služeb a aplikací.  Další informace o dostupných rolí hello najdete na [role v Azure AD PIM](active-directory-privileged-identity-management-roles.md).

tooadd nebo odebrat uživatele v roli pomocí Privileged Identity Management zprovoznit hello PIM řídicího panelu. Pak klikněte buď na hello **uživatelé v rolích správce** tlačítko nebo vyberte určité role (jako je například globální správce) z tabulky role hello.

> [!NOTE]
> Pokud jste nepovolili PIM v hello portál Azure ještě, přejděte příliš[Začínáme s Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) podrobnosti.

Pokud chcete toogive jiné tooPIM přístupu uživatele, samostatně, hello role, které vyžaduje PIM hello uživatele toohave jsou popsány dále v [přístupu tooPIM toogive](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-tooa-role"></a>Přidání tooa role uživatele
1. V hello [portál Azure](https://portal.azure.com/), vyberte hello **Azure AD Privileged Identity Management** dlaždici na řídicím panelu hello.
2. Vyberte **spravovat privilegované role**.
3. V hello **Souhrn rolí** tabulky, vyberte hello role, které chcete toomanage.
4. V okně role hello vyberte **přidat**.
5. Klikněte na tlačítko **vyberte uživatele,** a vyhledejte uživatele hello na hello **vyberte uživatele,** okno.  
6. Vyberte hello uživatele ze seznamu výsledků hledání hello a klikněte na tlačítko **provádí**.
7. Klikněte na tlačítko **OK** toosave váš výběr. Hello uživatele, které jste vybrali, se zobrazí v seznamu hello jako vhodné pro roli hello.

> [!NOTE]
> Vhodné pro hello roli ve výchozím nastavení jsou jenom nových uživatelů v roli. Pokud chcete toomake hello role trvalé, klikněte na tlačítko hello uživatele v seznamu hello. informace o Hello uživateli se zobrazí v novém okně. Vyberte **zkontrolujte oprávnění** v nabídce hello uživatelské informace.  
> Pokud uživatele nelze zaregistrovat pro Azure Multi-Factor Authentication (MFA), nebo se pomocí účtu Microsoft (obvykle @outlook.com), je nutné toomake je trvalé v jejich rolí. Oprávněné správce se zobrazí výzva tooregister pro MFA během aktivace.

Teď, když hello uživatele je vhodné pro roli, dát jim vědět, že se může aktivovat ho podle pokynů toohello v [jak tooactivate nebo deaktivovat roli](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Odebrat uživatele z role
Můžete odebrat uživatele z role vhodné přiřazení, ale ujistěte se, že je vždy alespoň jeden uživatel, který je trvalé globálního správce.

Postupujte podle těchto kroků tooremove konkrétního uživatele z role:

1. Vyberte roli v řídicím panelu Azure AD PIM hello nebo kliknutím na hello přejděte toohello role v seznamu role hello **uživatelé v rolích správce** tlačítko.
2. Klikněte na hello uživatele v seznamu uživatelů hello.
3. Klikněte na tlačítko **odebrat**. Zprávu se zeptá tooconfirm.
4. Klikněte na tlačítko **Ano** tooremove hello role od uživatele hello.

Pokud si nejste jistí, kteří uživatelé stále nutné jejich přiřazení role, pak můžete [spustit kontrola přístupu pro roli hello](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

