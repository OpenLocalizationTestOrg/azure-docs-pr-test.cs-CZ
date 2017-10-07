---
title: "aaaAzure podmíněného přístupu pro aplikace SaaS | Microsoft Docs"
description: "Podmíněného přístupu ve službě Azure AD umožňuje vám tooconfigure jednotlivých aplikací služby Multi-Factor authentication pravidel přístupu a hello možnost tooblock přístupu pro uživatele není v důvěryhodné síti. "
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 69748014c0c8e266ba66562980c784aba4c68d80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Začínáme s Azure Active Directory podmíněného přístupu
Azure Active Directory podmíněný přístup pro [SaaS](https://azure.microsoft.com/overview/what-is-saas/) aplikace a služby Azure AD připojené aplikace umožňuje nakonfigurovat podmíněný přístup na základě skupiny, umístění a aplikace velkých a malých písmen. 

Pomocí podmíněného přístupu podle aplikací velkých a malých písmen můžete nastavit pravidla přístupu k funkci vícefaktorového ověřování (MFA) na aplikaci. MFA na aplikaci poskytuje hello možnost tooblock přístup pro uživatele, kteří nejsou v důvěryhodné síti. Můžete použít MFA pravidla tooall uživatele, kteří jsou přiřazeny toohello aplikace, nebo pouze pro uživatele v rámci zadané skupiny zabezpečení.  Uživatelé mohou být vyjmuty z hello požadavek vícefaktorového ověřování, pokud uživatelé přistupují aplikace hello z IP adresy, která je v síti organizace hello.

Tato funkce bude k dispozici toocustomers, které jste zakoupili licenci Azure Active Directory Premium.

## <a name="scenario-prerequisites"></a>Předpoklady pro scénář
* Licence pro Azure Active Directory Premium
* Federovaný nebo spravovaný klienta Azure Active Directory
* Federovaných klientů vyžadují, povolit vícefaktorové ověřování.

## <a name="configure-per-application-access-rules"></a>Konfigurace pravidla přístupu k jednotlivým aplikacím
Tato část popisuje, jak přistupovat k tooconfigure pro každou aplikaci pravidla.

1. Přihlaste se toohello portál Azure classic pomocí účtu, který je globálním správcem pro Azure AD.
2. V levém podokně hello vyberte **služby Active Directory**.
3. Na kartě hello adresář vyberte svůj adresář.
4. Vyberte hello **aplikace** kartě.
5. Vyberte hello aplikace, která hello pravidlo bude nastavena pro.
6. Vyberte hello **konfigurace** kartě.
7. Posuňte se dolů část pravidla toohello přístup. Vyberte pravidlo přístupu požadované hello.
8. Zadejte hello uživatelů, které hello pravidlo se použije k.
9. Povolit zásady hello výběrem **zapnuta toobe**.

## <a name="understanding-access-rules"></a>Pravidla přístupu k pochopení
Tato část poskytuje podrobný popis pravidla přístupu k hello podporované v hello Azure podmíněný přístup k aplikaci.

### <a name="specifying-hello-users-hello-access-rules-apply-to"></a>Zadání uživatelů hello hello přístupu, které se týkají pravidla
Ve výchozím nastavení hello zásady budou vztahovat tooall uživatele, kteří mají přístup toohello aplikace. Ale můžete taky omezit toousers hello zásady, které jsou členy hello zadané skupiny zabezpečení. Hello **přidat skupinu** tlačítko je použité tooselect jeden nebo víc skupin z dialogového okna Výběr skupiny hello, které hello pravidlo přístupu se použijí na. Toto dialogové okno lze také použít tooremove vybrané skupiny. Vybrané tooapply tooGroups po hello pravidla pravidla přístupu k hello se pouze vynutí pro uživatele, kteří patří tooone hello zadané skupiny zabezpečení.

Skupiny zabezpečení můžete také výslovně vyloučit ze zásad hello výběrem hello **s výjimkou** možnost a výběrem jednoho nebo více skupin. Uživatelé, kteří jsou členy skupiny v hello **s výjimkou** seznamu nebude požadavku vícefaktorového ověřování toohello předmětu, i když jsou členem skupiny daného pravidla přístupu hello platí pro.
následující pravidlo přístupu Hello bude vyžadovat všichni uživatelé v hello správci skupiny toouse služby Multi-Factor authentication při přístupu k aplikaci hello.

![Nastavení pravidla podmíněného přístupu pomocí vícefaktorového ověřování](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Pravidla podmíněného přístupu pomocí vícefaktorového ověřování
Pokud uživatel byl nakonfigurovaný pomocí funkce služby Multi-Factor authentication hello na uživatele, bude toto nastavení uživatele hello kombinovat s hello služby Multi-Factor authentication pravidla aplikace hello. To znamená, že uživatel, který byl nakonfigurován pro jednotlivé uživatele služby Multi-Factor authentication bude požadované tooperform služby Multi-Factor authentication, i v případě, že máte byly vyloučené z hello aplikace služby Multi-Factor authentication pravidel. Další informace o nastavení vícefaktorového ověřování a jednotlivé uživatele.

### <a name="access-rule-options"></a>Možnosti pravidlo přístupu
podporovány jsou následující možnosti Hello:

* **Vyžadovat vícefaktorové ověřování**: budou uživatelé hello pravidla přístupu hello toowhom platit, požadované toocomplete služby Multi-Factor authentication před přístup k aplikaci hello, který hello zásad platí pro.
* **Vyžadovat vícefaktorové ověřování, když není v práci**: uživatel, který pochází z důvěryhodné adresy IP nebudou požadované tooperform služby Multi-Factor authentication. Hello důvěryhodné, rozsahy IP adres lze nakonfigurovat na stránce nastavení služby Multi-Factor authentication hello.
* **Blokování přístupu, když není v práci**: uživatel, který není pocházejících z důvěryhodné IP adresy se zablokuje. Hello důvěryhodné, rozsahy IP adres lze nakonfigurovat na stránce nastavení služby Multi-Factor authentication hello.

### <a name="setting-rule-status"></a>Stav pravidla nastavení
Stav pravidla přístupu umožňuje zapnutí hello pravidla nebo vypnutí. Když hello pravidel přístupu jsou vypnuté, hello požadavku vícefaktorového ověřování se nevynucuje.

### <a name="access-rule-evaluation"></a>Vyhodnocení pravidla přístupu
Pravidla přístupu se vyhodnocují, když uživatel získá přístup k federované aplikaci, která používá OAuth 2.0, OpenID Connect, SAML nebo WS-Federation. Kromě toho jsou při hello OAuth 2.0 a OpenID Connect pomocí tokenu tooacquire aktualizace přístupový token vyhodnocovány pravidla přístupu. Pokud hodnocení zásad selže, pokud se používá token obnovení, hello chyba **invalid_grant** bude vrácen, znamená to, že uživatel hello potřebuje toore-ověření toohello klienta.

### <a name="configure-federation-services-tooprovide-multi-factor-authentication"></a>Nakonfigurujte federační služby tooprovide službu Multi-Factor authentication
U federovaných klientů, může provést MFA, Azure Active Directory nebo hello místního serveru služby AD FS.

Ve výchozím nastavení vícefaktorového ověřování dojde v stránky hostované službou Azure Active Directory. tooconfigure MFA na místních počítačích hello **– SupportsMFA** příliš musí být nastavena vlastnost**true** v Azure Active Directory pomocí modulu hello Azure AD pro prostředí Windows PowerShell.

Hello následující příklad ukazuje, jak tooenable místní vícefaktorové ověřování s použitím hello [rutiny Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) na hello contoso.com klienta:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

V přidání toosetting tento příznak hello federovaného klienta služby AD FS instance musí být nakonfigurované tooperform služby Multi-Factor authentication. Postupujte podle pokynů hello pro [nasazení Azure Multi-Factor Authentication na místě](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Související články
* [Zabezpečení přístupu tooOffice 365 a další aplikace připojeným tooAzure služby Active Directory](active-directory-conditional-access.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

