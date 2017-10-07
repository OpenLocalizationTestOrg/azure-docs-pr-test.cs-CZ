---
title: "aaaAdd uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory | Microsoft Docs"
description: "Vysvětluje, jak uživatelé tooadd nebo změnit informace o uživateli v Azure Active Directory, včetně externích uživatelů a hostů."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 564a04ec-53c1-470b-9ab9-f3db57da0a89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 92099e5792365c307b0f3d4f2dff5dd8424aeab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory"></a>Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory

Tento článek vysvětluje, jak tooadd uživatelů z dalších adresářů ve službě Azure Active Directory nebo přidávání uživatelů z partnerských společností. Informace o přidávání nových uživatelů ve vaší organizaci a přidávání uživatelů, kteří mají účty Microsoft najdete v tématu [přidat nové uživatele tooAzure služby Active Directory](active-directory-create-users.md). 

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak tooadd B2B ve službě Azure AD Dobrý den, správce spolupráce uživatele typu Host center, najdete v části [co je spolupráce Azure AD B2B?](active-directory-b2b-what-is-azure-ad-b2b.md)

Přidaní uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete kdykoli přiřadit role toothem.

## <a name="add-a-user"></a>Přidání uživatele
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **Active Directory** a otevřete svůj adresář.
3. Vyberte hello **uživatelé** a pak v řádku nabídek hello vyberte **přidat uživatele**.
4. Na hello **Povězte nám o tohoto uživatele** v části **typ uživatele**, vyberte buď:

   * **Uživatel z jiného adresáře služby Azure AD** – přidá adresář tooyour uživatelského účtu, který pochází z jiného adresáře služby Azure AD. Uživatele z jiného adresáře můžete vybrat pouze v případě, že jste také členem daného adresáře.
   * **Uživatelé z partnerských společností** -tooinvite a autorizovat partnera společnosti uživatelé tooyour adresáře (viz [spolupráce Azure Active Directory s B2B](active-directory-b2b-what-is-azure-ad-b2b.md)). Budete potřebovat příliš[odeslat soubor CSV e-mailovými adresami](active-directory-b2b-references-csv-file-format.md).
5. Hello uživatele **profil** stránky, zadejte název první a poslední, uživatelské jméno a roli uživatele z hello **role** seznamu. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md). Určit, zda příliš**povolit službu Multi-Factor Authentication** pro uživatele hello.
6. Na hello **získat dočasné heslo** vyberte **vytvořit**.

> [!IMPORTANT]
> Pokud vaše organizace používá více než jednu doménu, měli byste vědět o hello následující problémy při přidání uživatelského účtu:
>
> * hello tooadd uživatelské účty s totožným hlavním názvem uživatele (UPN) napříč doménami, **první** přidat, například geoffgrisso@contoso.onmicrosoft.com, **následuje** geoffgrisso@contoso.com.
> * **Nepřidávejte**geoffgrisso@contoso.com předtím, než přidáte geoffgrisso@contoso.onmicrosoft.com.
>

Pokud chcete měnit informace pro uživatele, jehož identita je synchronizována s místní službou Active Directory, nelze změnit informace o uživateli hello v hello portál Azure classic. toochange hello informace o uživateli, použijte nástroje pro správu vaší místní služby Active Directory.

## <a name="add-external-users"></a>Přidání externích uživatelů
Můžete také přidat uživatele z jiného toowhich adresář Azure AD, které patříte, nebo z partnerských společností tím, že nahrajete soubor CSV. tooadd externího uživatele, pro **typ uživatele**, zadejte **uživatel z jiného adresáře Microsoft Azure AD** nebo **uživatelé z partnerských společností**.

Uživatelé těchto typů pocházejí z jiných adresářů a přidají se jako **externí uživatelé**. Externí uživatelé mohou spolupracovat s ostatními uživateli v adresáři, aniž by jakékoli požadavek tooadd nové účty a přihlašovací údaje. Externí uživatelé ověřit pomocí svých domovských adresářů při přihlášení a toto ověření platí pro všechny ostatní toowhich adresáře, které byly přidány.

## <a name="external-user-management-and-limitations"></a>Správa a omezení externích uživatelů
Když přidáte uživatele z jiného adresáře tooyour directory, je tento uživatel externího uživatele ve vašem adresáři. Hello zobrazovaný název a uživatelské jméno jsou zkopírovány ze svého domovského adresáře a použít pro hello externího uživatele ve vašem adresáři. Od toho jsou vlastnosti externího uživatelského účtu hello zcela nezávislé. Pokud vlastnost změny toohello uživatele v jeho domovském adresáři, nerozšíří se tyto změny toohello externího uživatelského účtu ve vašem adresáři.

Hello pouze propojení mezi dvěma účty hello je, že tento hello uživatel vždy ověřuje prostřednictvím svého domovského adresáře nebo účtu Microsoft. Proto nejsou zobrazeny zadat možnost tooreset hello heslo nebo povolení vícefaktorového ověřování pro externí uživatele. V současné době je zásady ověřování hello hello domovského adresáře nebo účtu Microsoft hello pouze jeden, který se vyhodnotí při přihlášení uživatele hello.

> [!NOTE]
> Přesto můžete zakázat hello externí uživatele v adresáři hello, která blokuje přístup k adresáři tooyour.
>
>

Pokud bude odstraněn ze svého domovského adresáře nebo zruší svůj účet Microsoft, externí uživatel hello stále existuje v adresáři. Ale hello uživatel ve vašem adresáři nelze přistupovat k prostředkům vzhledem k tomu, že se nemůže ověřit prostřednictvím domovského adresáře nebo účtu Microsoft.

### <a name="services-that-currently-support-access-by-azure-ad-external-users"></a>Služby, které aktuálně podporují přístup externích uživatelů služby Azure AD
* **Portál Azure classic**: umožňuje externímu uživateli, který je správcem více adresářů toomanage každý z těchto adresářů.
* **SharePoint Online**: Pokud je povoleno externí sdílení, umožňuje externího uživatele tooaccess prostředků na Sharepointu Online oprávnění.
* **Dynamics CRM**: Pokud hello uživatel licencován prostřednictvím rozhraní PowerShell, umožňuje tooaccess externímu uživateli oprávnění prostředky v aplikaci Dynamics CRM.
* **Dynamics AX**: Pokud hello uživatel licencován prostřednictvím rozhraní PowerShell, umožňuje tooaccess externímu uživateli oprávnění prostředky v aplikaci Dynamics AX. Hello omezení pro [externí uživatele Azure AD](#known-limitations-of-azure-ad-external-users) použít tooexternal uživatele v aplikaci Dynamics AX také.

## <a name="next-steps"></a>Další kroky
* [Přidat nové uživatele tooAzure služby Active Directory](active-directory-create-users.md)
* [Správa služby Azure AD](active-directory-administer.md)
* [Správa hesel ve službě Azure AD](active-directory-manage-passwords.md)
* [Správa skupin ve službě Azure AD](active-directory-manage-groups.md)
