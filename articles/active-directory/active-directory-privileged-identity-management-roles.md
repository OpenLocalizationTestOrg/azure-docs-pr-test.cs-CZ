---
title: aaaRoles v Azure AD Privileged Identity managementu | Microsoft Docs
description: "Zjistěte, jaké role se používají pro privilegované identity pomocí hello rozšíření Azure Privileged Identity Management."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ac812ccc-cf4e-4ac2-b981-69598056c9ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: dc58d005489e3b51b3b3dbea4bf35bd795dbdfb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="different-administrative-role-in-azure-active-directory-pim"></a>Různé role pro správu v Azure Active Directory PIM
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Můžete přiřadit uživatele ve vaší organizaci toodifferent správní role ve službě Azure AD. Přiřazení rolí řídit, které úkoly, například přidávání nebo odebírání uživatelů nebo změna nastavení služby, hello uživatelé jsou na Azure AD, Office 365 a dalších služeb Microsoft Online Services a propojených aplikací mít tooperform.  

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku.

Můžete aktualizovat globální správce, které jsou uživatelé **trvale** přiřazené tooroles ve službě Azure AD, například pomocí rutin prostředí PowerShell `Add-MsolRoleMember` a `Remove-MsolRoleMember`, nebo prostřednictvím portálu classic hello, jak je popsáno v [ přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD Privileged Identity Management (PIM) spravuje zásady pro privilegovaný přístup pro uživatele ve službě Azure AD. PIM přiřadí uživatelům tooone nebo více rolí ve službě Azure AD, a můžete přiřadit někdo toobe trvale v roli hello nebo způsobilých pro roli hello. Když uživatel trvale přiřazená tooa role nebo aktivuje přiřazení vhodné role, pak můžou spravovat služby Azure Active Directory, Office 365 a dalších aplikací s oprávněními hello přiřazené tootheir role.

Není žádný rozdíl ve hello přístupu zadané toosomeone s trvalé a přiřazení role vhodné. Hello jediným rozdílem je, že někteří uživatelé nepotřebují tento přístup celou dobu hello. Probíhají vhodné pro hello role a můžete zapnout a vypnout vždy, když potřebují.

## <a name="roles-managed-in-pim"></a>Role, které jsou spravovány v PIM
Privileged Identity Management umožňuje přiřadit role správce toocommon uživatelů, včetně:

* **Globální správce** (také označovaný jako správce společnosti) má přístup tooall funkcím pro správu. Můžete mít více než jeden globální správce ve vaší organizaci. Hello osoba zaregistruje toopurchase Office 365 automaticky stane globální správce.
* **Správce privilegovaných rolí** spravuje Azure AD PIM a aktualizuje přiřazení rolí pro ostatní uživatele.  
* **Správce fakturace** může dělat nákupy, spravovat předplatná, spravovat lístky žádostí o podporu a sledovat stav služeb.
* **Heslo správce** může resetovat hesla, spravovat žádosti o služby a sledovat stav služeb. Heslo správce jsou omezené tooresetting hesla pro uživatele.
* **Správce služeb** spravovat žádosti o služby a sledovat stav služeb.
  
  > [!NOTE]
  > Pokud používáte Office 365, pak před přiřazením hello služby role tooa správce, nejprve přiřadíte oprávnění správce uživatele hello tooa služby, jako je Exchange Online.
  > 
  > 
* **Správce správy uživatelů** může resetovat hesla, sledovat stav služeb a spravovat uživatelské účty, skupiny uživatelů a žádosti o služby. Dobrý den, správce správy uživatelů nelze odstranit globální správce, vytvořit další role správce nebo resetování hesla pro fakturace, globálních a Správce služby.
* **Správce serveru Exchange** má přístup pro správu tooExchange Online pomocí centra pro správu Exchange hello (e) a můžete provádět skoro všechny úlohy v systému Exchange Online.
* **Správce služby SharePoint** má přístup pro správu tooSharePoint Online pomocí Centra správy služby SharePoint Online hello a můžete provádět skoro všechny úlohy ve službě SharePoint Online.
* **Skype pro firmy správce** má přístup pro správu tooSkype pro firmy prostřednictvím hello Skype pro firmy centra pro správu a můžete provádět skoro všechny úlohy v Skype for Business Online.

Přečtěte si tyto články další podrobnosti o [přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md) a [přiřazení rolí správců v Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: hello above article may not be hello one we want since PIM gets roles from places other that Office 365**-->


Z PIM, můžete [přiřadit tyto role uživatele tooa](active-directory-privileged-identity-management-how-to-add-role-to-user.md) tak, aby hello uživatel může [aktivovat hello role v případě potřeby](active-directory-privileged-identity-management-how-to-activate-role.md).

Pokud chcete toogive jiné toomanage přístup uživatele v PIM samostatně, hello role, které vyžaduje PIM hello uživatele toohave jsou popsány dále v [přístupu tooPIM toogive](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

<!-- ## hello PIM Security Administrator Role **PLACEHOLDER: Need description of hello Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Role není spravovaná v PIM
Role v systému Exchange Online nebo SharePoint Online, s výjimkou případů uvedených výše, nenachází ve službě Azure AD a proto nejsou viditelné v PIM. Další informace o změně přiřazení jemně odstupňovaných rolí v těchto služeb Office 365 najdete v tématu [oprávnění v Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Skupiny prostředků a předplatná Azure také nenachází ve službě Azure AD. toomanage Azure najdete v části předplatná, [jak tooadd nebo změna role Správce služby Azure](../billing/billing-add-change-azure-subscription-administrator.md) a další informace o Azure RBAC najdete [řízení přístupu](role-based-access-control-configure.md).

<!--**hello above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Uživatelské role a přihlášení
Pro některé služby společnosti Microsoft a aplikace, přiřazení role tooa uživatele nemusí mít dostatečná tooenable toobe tento uživatel správce.

Toohello přístup k portálu Azure classic vyžaduje hello být uživatel správcem služby nebo spolusprávce na předplatné Azure, i v případě, že uživatel hello není nutné toomanage hello předplatných Azure.  Například toomanage nastavení konfigurace pro Azure AD na portálu classic hello, uživatel musí být globálním správcem ve službě Azure AD a spolusprávce předplatného na základě předplatného služby Azure.  toolearn jak zjistit, tooadd uživatelé tooAzure odběry, [jak tooadd nebo změna role Správce služby Azure](../billing/billing-add-change-azure-subscription-administrator.md).

TooMicrosoft přístup k Online službám může vyžadovat hello uživatele také přiřadit licenci před jejich otevřete portál hello služeb nebo provádění úloh správy.

## <a name="assign-a-license-tooa-user-in-azure-ad"></a>Přiřazení licencí tooa uživatele ve službě Azure AD
1. Přihlaste se toohello [portál Azure classic](http://manage.windowsazure.com) s účet globálního správce nebo spolusprávce účtem.
2. Vyberte **všechny položky** z hlavní nabídky hello.
3. Vyberte adresář hello chcete toowork s a že má licence s ním spojená.
4. Vyberte **licence**. Zobrazí se Hello seznam dostupných licencí.
5. Vyberte hello licenční plán, který obsahuje hello licencí, které chcete toodistribute.
6. Vyberte **přiřazení uživatelů**.
7. Vyberte hello uživatele, které chcete tooassign licenci k.
8. Klikněte na tlačítko hello **přiřadit** tlačítko.  uživatel Hello teď může přihlásit tooAzure.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Další kroky
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

