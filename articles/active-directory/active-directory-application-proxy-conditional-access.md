---
title: "aaaConditional přístup tooon místní aplikace – Azure AD | Microsoft Docs"
description: "Popisuje, jak tooset až podmíněného přístupu pro aplikace publikovat toobe přistupovat vzdáleně pomocí proxy aplikace služby Azure AD."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 7bed25dd4ba17941e77d8c4b2b9ba4edcf0cf597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Práce s podmíněného přístupu v Azure AD Application Proxy

>[!NOTE]
>Tento článek se týká toohello portál Azure classic, které se postupně vyřazuje z provozu. Doporučujeme použít hello [portál Azure](https://portal.azure.com). V hello portálu Azure Proxy aplikací mít aplikace hello stejné funkce podmíněného přístupu jako jakákoli jiná aplikace SaaS. toolearn Další informace o podmíněný přístup, najdete v části [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Přístup můžete nakonfigurovat pravidla toogrant podmíněného přístupu tooapplications publikovaná pomocí Proxy aplikací. To umožňuje:

* Požadovat použití vícefaktorového ověřování na aplikaci.
* Vyžadovat víceúrovňové ověřování jenom v případě, že uživatelé nejsou v práci
* Zabránění uživatelům v přístupu k aplikace hello, pokud nejsou v práci

Tato pravidla může být použité tooall uživatelé a skupiny nebo pouze toospecific uživatelé a skupiny. Ve výchozím nastavení platí pravidlo hello tooall uživatelé, kteří mají přístup toohello aplikace. Pravidlo hello však může být toousers s omezeným přístupem, které jsou členy zadané skupiny zabezpečení.  

Pravidla přístupu se vyhodnocují, když uživatel získá přístup k federované aplikaci, která používá OAuth 2.0, OpenID Connect, SAML nebo WS-Federation. Kromě toho pravidla přístupu k vyhodnocují s OAuth 2.0 a OpenID Connect, pokud token obnovení je použité tooacquire přístupový token.

## <a name="conditional-access-prerequisites"></a>Požadavky podmíněného přístupu
* Předplatné tooAzure Active Directory Premium
* Federovaný nebo spravovaný klienta Azure Active Directory
* Federovaných klientů vyžadovat vícefaktorové ověřování (MFA)  
    ![Konfigurace pravidel přístupu – vyžadovat vícefaktorové ověřování](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Nakonfigurovat každou aplikaci služby Multi-Factor authentication
1. Přihlaste se jako správce v hello portál Azure classic.
2. Přejděte tooActive adresář a vyberte hello adresář, ve kterém chcete tooenable Proxy aplikace.
3. Klikněte na tlačítko **aplikace** a přejděte dolů toohello **pravidla přístupu k** části. část pravidla přístupu Hello se zobrazí pouze pro aplikace publikované pomocí Proxy aplikací, které používají federovaného ověřování.
4. Povolit pravidlo hello výběrem **povolit pravidla přístupu k** příliš**na**.
5. Zadejte hello uživatelů a skupin toowhom hello, které platí pravidla. Použití hello **přidat skupinu** tlačítko tooselect jednu nebo více skupin toowhich hello přístup pravidlo vztahuje. Toto dialogové okno lze také použít tooremove vybrané skupiny.  Vybrané tooapply toogroups po hello pravidla pravidla přístupu k hello se vynucují jenom pro uživatele, kteří patří tooone hello zadané skupiny zabezpečení.  

   * Zkontrolujte skupiny zabezpečení tooexplicitly vyloučit z pravidla hello **s výjimkou** a zadejte jednu nebo více skupin. Uživatelé, kteří jsou členy skupiny v hello kromě seznamu nejsou požadované tooperform služby Multi-Factor authentication.  
   * Pokud uživatel byl nakonfigurován pomocí hello na uživatele služby Multi-Factor authentication funkce, toto nastavení má přednost před hello pravidla aplikace služby Multi-Factor authentication. Uživatel, který byl nakonfigurován pro jednotlivé uživatele služby Multi-Factor authentication je požadovaná tooperform Multi-Factor authentication, i v případě, že máte byly vyloučené z pravidel hello aplikace služby Multi-Factor authentication. Další informace o [vícefaktorového ověřování a uživatelská nastavení](../multi-factor-authentication/multi-factor-authentication.md).
6. Vyberte pravidlo přístupu hello, že chcete tooset:

   * **Vyžadovat vícefaktorové ověřování**: uživatelé použít pravidla přístupu k toowhom jsou požadované toocomplete služby Multi-Factor authentication před přístupem hello aplikace toowhich hello pravidlo vztahuje.
   * **Vyžadovat vícefaktorové ověřování, když není v práci**: uživatelé při aplikace hello tooaccess z důvěryhodné IP adresy nebudou požadované tooperform služby Multi-Factor authentication. Hello důvěryhodné, rozsahy IP adres lze nakonfigurovat na stránce nastavení služby Multi-Factor authentication hello.
   * **Blokování přístupu, když není v práci**: uživatelé při tooaccess aplikace hello mimo podnikovou síť nebudou moct tooaccess hello aplikace.

## <a name="configuring-mfa-for-federation-services"></a>Konfigurace vícefaktorového ověřování pro služby federation
U federovaných klientů, vícefaktorové ověřování (MFA) se dají provést pomocí Azure Active Directory nebo pomocí hello místního serveru služby AD FS. Ve výchozím nastavení provádí MFA na libovolné stránce hostované službou Azure Active Directory. tooconfigure MFA na místě, spusťte prostředí Windows PowerShell a použití hello – SupportsMFA vlastnost tooset hello modulu Azure AD.

Hello následující příklad ukazuje, jak tooenable místní vícefaktorové ověřování s použitím hello [rutiny Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) na hello contoso.com klienta:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

V přidání toosetting tento příznak hello federovaného klienta služby AD FS instance musí být nakonfigurované tooperform služby Multi-Factor authentication. Postupujte podle pokynů hello pro [nasazení Microsoft Azure Multi-Factor authentication na místě](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Viz také
* [Práce s aplikacemi využívajícími deklarace](active-directory-application-proxy-claims-aware-apps.md)
* [Publikování aplikací pomocí Proxy aplikace](active-directory-application-proxy-publish.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Publikování aplikací s použitím vlastního názvu domény](active-directory-application-proxy-custom-domains.md)

Hello nejnovější novinky a aktualizace, najdete na naší hello [blogu Proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)
