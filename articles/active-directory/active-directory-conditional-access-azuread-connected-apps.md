---
title: "Azure podmíněného přístupu pro aplikace SaaS | Microsoft Docs"
description: "Podmíněný přístup v Azure AD umožňuje konfigurovat pravidla přístupu k jednotlivým aplikacím služby Multi-Factor authentication a možnost blokovat přístup pro uživatele není v důvěryhodné síti. "
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
ms.openlocfilehash: efaa70467346e910a78a63d182041029bb34b1cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Začínáme s Azure Active Directory podmíněného přístupu
Azure Active Directory podmíněný přístup pro [SaaS](https://azure.microsoft.com/overview/what-is-saas/) aplikace a služby Azure AD připojené aplikace umožňuje nakonfigurovat podmíněný přístup na základě skupiny, umístění a aplikace velkých a malých písmen. 

Pomocí podmíněného přístupu podle aplikací velkých a malých písmen můžete nastavit pravidla přístupu k funkci vícefaktorového ověřování (MFA) na aplikaci. MFA na aplikaci poskytuje možnost blokovat přístup pro uživatele, kteří nejsou v důvěryhodné síti. MFA pravidla můžete použít pro všechny uživatele, které jsou přiřazené k aplikaci nebo jenom pro uživatele v rámci určených skupinách zabezpečení.  Uživatelé mohou být vyjmuty z požadavek na vícefaktorové ověřování, pokud uživatelé přistupují aplikace z IP adresy, který je uvnitř sítě organizace.

Tato funkce bude k dispozici pro zákazníky, kteří si zakoupili licenci Azure Active Directory Premium.

## <a name="scenario-prerequisites"></a>Předpoklady pro scénář
* Licence pro Azure Active Directory Premium
* Federovaný nebo spravovaný klienta Azure Active Directory
* Federovaných klientů vyžadují, povolit vícefaktorové ověřování.

## <a name="configure-per-application-access-rules"></a>Konfigurace pravidla přístupu k jednotlivým aplikacím
Tato část popisuje postup konfigurace pravidla přístupu k jednotlivým aplikacím.

1. Přihlaste se k portálu Azure classic pomocí účtu, který je globálním správcem pro Azure AD.
2. V levém podokně vyberte **Active Directory**.
3. Na kartě Adresář vyberte svůj adresář.
4. Vyberte **aplikace** kartě.
5. Vyberte aplikaci, která pravidla bude nastavena pro.
6. Vyberte kartu **Konfigurace**.
7. Přejděte do části pravidla přístupu. Vyberte pravidlo přístupu požadované.
8. Zadejte uživatele, které se použije se pro pravidlo.
9. Povolit zásady výběrem **povoleno na**.

## <a name="understanding-access-rules"></a>Pravidla přístupu k pochopení
Tato část poskytuje podrobný popis pravidel přístupu, které jsou podporovány v podmíněného přístupu aplikace Azure.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Pravidla přístupu k určení uživatelů platí pro
Ve výchozím nastavení se zásady použijí pro všechny uživatele, kteří mají přístup k aplikaci. Zásady ale můžete omezit uživatele, kteří jsou členy určených skupinách zabezpečení. **Přidat skupinu** tlačítko slouží k výběru jedné nebo více skupin z dialogového okna Výběr skupiny, která pravidlo přístupu budou platit pro. Tento dialog můžete použít také k odebrání vybrané skupiny. Pokud jsou pravidla vybraná pro použití na skupiny, bude pravidla přístupu k vynucují jenom pro uživatele, kteří patřit do jedné ze zadaných skupin zabezpečení.

Skupiny zabezpečení můžete také výslovně vyloučit ze zásad výběrem **s výjimkou** možnost a výběrem jednoho nebo více skupin. Uživatelé, kteří jsou členy skupiny v **s výjimkou** seznamu nebude platit požadavku vícefaktorového ověřování, i když jsou členem skupiny, která se vztahuje pravidlo přístupu k.
Následující pravidlo přístupu bude vyžadovat všechny uživatele ve skupině správců k používání služby Multi-Factor authentication při přístupu k aplikaci.

![Nastavení pravidla podmíněného přístupu pomocí vícefaktorového ověřování](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>Pravidla podmíněného přístupu pomocí vícefaktorového ověřování
Pokud uživatel byl nakonfigurovaný pomocí funkce na uživatele služby Multi-Factor authentication, bude toto nastavení na uživatele, kombinovat s pravidla vícefaktorového ověřování aplikace. To znamená, že uživatel, který byl nakonfigurován pro jednotlivé uživatele služby Multi-Factor authentication, bude nutné provést službu Multi-Factor authentication, i když mají byly vyloučené z pravidel aplikace služby Multi-Factor authentication. Další informace o nastavení vícefaktorového ověřování a jednotlivé uživatele.

### <a name="access-rule-options"></a>Možnosti pravidlo přístupu
Podporovány jsou následující možnosti:

* **Vyžadovat vícefaktorové ověřování**: uživatelé, na které se vztahují pravidla přístupu, budou muset dokončení služby Multi-Factor authentication před přístupem k aplikaci, která se zásady vztahují.
* **Vyžadovat vícefaktorové ověřování, když není v práci**: uživatel, který pochází z důvěryhodné adresy IP, nebudou muset provést službu Multi-Factor authentication. Na stránce nastavení služby Multi-Factor authentication lze nakonfigurovat důvěryhodné rozsahy IP adres.
* **Blokování přístupu, když není v práci**: uživatel, který není pocházejících z důvěryhodné IP adresy se zablokuje. Na stránce nastavení služby Multi-Factor authentication lze nakonfigurovat důvěryhodné rozsahy IP adres.

### <a name="setting-rule-status"></a>Stav pravidla nastavení
Stav pravidla přístupu umožňuje zapnutí pravidla nebo vypnutí. Po vypnutí pravidla přístupu k požadavku vícefaktorového ověřování se nevynucuje.

### <a name="access-rule-evaluation"></a>Vyhodnocení pravidla přístupu
Pravidla přístupu se vyhodnocují, když uživatel získá přístup k federované aplikaci, která používá OAuth 2.0, OpenID Connect, SAML nebo WS-Federation. Kromě toho jsou při OpenID Connect a OAuth 2.0 používat token obnovení získat přístupový token vyhodnocovány pravidla přístupu. Pokud hodnocení zásad selže, když se používá token obnovení, chyba **invalid_grant** bude vrácen, znamená to, že uživatel musí znovu provést ověření do klienta.

### <a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Nakonfigurujte federační služby pro zajištění vícefaktorového ověřování
U federovaných klientů, může provést MFA, Azure Active Directory nebo místní server AD FS.

Ve výchozím nastavení vícefaktorového ověřování dojde v stránky hostované službou Azure Active Directory. Konfigurace vícefaktorového ověřování na místních počítačích **– SupportsMFA** musí být nastavena na **true** v Azure Active Directory pomocí modulu Azure AD pro prostředí Windows PowerShell.

Následující příklad ukazuje, jak zapnout MFA místně pomocí [rutiny Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) u contoso.com klienta:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Kromě nastavení tento příznak, je třeba instanci federovaného klienta AD FS provést službu Multi-Factor authentication. Postupujte podle pokynů pro [nasazení Azure Multi-Factor Authentication na místě](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="related-articles"></a>Související články
* [Zabezpečení přístupu k Office 365 a jiným aplikacím připojeným ke službě Azure Active Directory](active-directory-conditional-access.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

