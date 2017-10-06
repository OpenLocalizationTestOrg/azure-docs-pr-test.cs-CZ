---
title: "aaaAdd tooAzure nového uživatele služby Active Directory | Microsoft Docs"
description: "Vysvětluje, jak tooadd nové uživatele nebo změnit informace o uživateli v Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e3673727-6bec-4fdc-87a4-d65b213c4c3c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 72f67ad41022fd19fd94c8e1301943b0db1e57bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-or-users-with-microsoft-accounts-tooazure-active-directory"></a>Přidání nových uživatelů nebo uživatelé s tooAzure účty Microsoft, Active Directory
Přidejte uživatele toopopulate adresáře. Tento článek vysvětluje, jak tooadd noví uživatelé ve vaší organizaci a jak tooadd uživatelé, kteří mají účty Microsoft. Další informace o přidávání uživatelů z dalších adresářů ve službě Azure Active Directory nebo přidávání uživatelů z partnerských společností najdete v článku o [přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory](active-directory-create-users-external.md). Přidaní uživatelé nemají oprávnění správce ve výchozím nastavení, ale můžete kdykoli přiřadit role toothem.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Pro jak tooadd s uživatelem v Centru pro správu hello Azure AD, najdete v části [přidat nové uživatele tooAzure služby Active Directory](active-directory-users-create-azure-portal.md).

## <a name="add-a-user"></a>Přidání uživatele
1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **služby Active Directory**a potom vyberte hello název adresáře své organizace.
3. Vyberte hello **uživatelé** a pak v řádku nabídek hello vyberte **přidat uživatele**.
4. Na hello **Povězte nám o tohoto uživatele** v části **typ uživatele**, vyberte buď:

   * **Nový uživatel v organizaci** – přidá do vašeho adresáře nový uživatelský účet.
   * **Uživatel s existujícím účtem Microsoft** – přidá existující Microsoft uživatelský účet tooyour adresář (například účet Outlook)
5. V závislosti na tom, jaký **Typ uživatele** jste vybrali, zadejte buď uživatelské jméno (pro nového uživatele), nebo e-mailovou adresu (pro uživatele s účtem Microsoft).
6. Hello uživatele **profil** stránky, zadejte název první a poslední, uživatelské jméno a roli uživatele z hello **role** seznamu. Další informace o rolích uživatelů a správců najdete v článku [Přiřazení rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md). Určit, zda příliš**povolit službu Multi-Factor Authentication** pro uživatele hello.
7. Na hello **získat dočasné heslo** vyberte **vytvořit**.

> [!IMPORTANT]
> Pokud vaše organizace používá více než jednu doménu, měli byste vědět o hello následující problémy při přidání uživatelského účtu:
>
> * hello tooadd uživatelské účty s totožným hlavním názvem uživatele (UPN) napříč doménami, **první** přidat, například geoffgrisso@contoso.onmicrosoft.com, **následuje** geoffgrisso@contoso.com.
> * **Nepřidávejte**geoffgrisso@contoso.com předtím, než přidáte geoffgrisso@contoso.onmicrosoft.com. Správné pořadí je důležité a může být pracné tooundo.
>
>

## <a name="change-user-information"></a>Změna informací o uživateli
Můžete změnit jakýkoli atribut uživatele s výjimkou ID hello objektu.

1. Otevřete svůj adresář.
2. Vyberte hello **uživatelé** kartu a pak vyberte hello zobrazovaný název hello uživatele, bude toochange.
3. Proveďte změny a klikněte na **Uložit**.

Pokud hello uživatele, který chcete změnit synchronizována s místní službou Active Directory, nemůžete změnit informace o uživateli hello pomocí tohoto postupu. toochange hello uživatele, použijte nástroje pro správu vaší místní služby Active Directory.

## <a name="guest-user-management-and-limitations"></a>Správa a omezení uživatele typu host
Účty hostů jsou uživatelé z jiných adresářů, kteří byli pozvané tooyour directory tooaccess SharePoint dokumenty, aplikací nebo jiných prostředků Azure. Účet guest ve vašem adresáři má jeho základní atribut UserType nastavený nastavit příliš "Guest." Běžní uživatelé (konkrétně členové vašeho adresáře) mají atribut UserType hello "(člen).

Hosté mají omezenou sadu oprávnění v adresáři hello. Tato práva omezit možnost hello hosté toodiscover informace o ostatních uživatelích v adresáři hello. Uživatele typu Host může nadále komunikovat s hello uživatelů a skupin přidružených k hello prostředky, které pracují na. Uživatel typu host může:

* Vidět ostatní uživatele a skupiny přidružené toowhich předplatné Azure, který je přiřazený
* V tématu hello členy skupiny toowhich, ke které patří
* Vyhledávat ostatní uživatele v adresáři hello, v případě, že zná hello úplnou e-mailovou adresu uživatele hello
* V tématu omezená sada atributů hello uživatelů, které vyhledává – omezené toodisplay jméno, e-mailovou adresu, hlavní název uživatele (UPN) a miniaturu fotografie
* Získat seznam ověřených domén v adresáři hello
* Tooapplications souhlasu, je udělení hello stejný přístup, který členové mají ve vašem adresáři

## <a name="set-guest-user-access-policies"></a>Nastavení zásad přístupu uživatele typu host
Hello **konfigurace** karta adresáře obsahuje možnosti toocontrol přístupu uživatelů typu Host. Tyto možnosti může změnit pouze globální správce adresáře prostřednictvím portálu Azure Classic. V současné době není k dispozici žádná metoda změny pomocí PowerShellu ani rozhraní API.

tooopen hello **konfigurace** kartě v hello Azure classic portálu, vyberte **služby Active Directory**a pak vyberte název hello hello adresáře.

![Karta Konfigurace ve službě Azure Active Directory][1]

Potom můžete upravit hello možnosti toocontrol přístupu uživatelů typu Host.

![možnosti řízení přístupu pro uživatele typu host][2]

## <a name="whats-next"></a>Kam dál
* [Přidávání uživatelů z dalších adresářů nebo partnerských společností ve službě Azure Active Directory](active-directory-create-users-external.md)
* [Správa služby Azure AD](active-directory-administer.md)
* [Správa hesel ve službě Azure AD](active-directory-manage-passwords.md)
* [Správa skupin ve službě Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
