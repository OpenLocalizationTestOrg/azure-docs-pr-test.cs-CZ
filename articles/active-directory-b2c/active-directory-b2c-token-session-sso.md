---
title: "Azure Active Directory B2C: Token, relace a jednotné přihlašování | Microsoft Docs"
description: "Token, relace a konfiguraci jednoho přihlášení v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: e78e6344-0089-49bf-8c7b-5f634326f58c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: swkrish
ms.openlocfilehash: 63325ed97a7363723c97ee3a992046ebb5592662
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token, relace a jednotné přihlašování
Tato funkce poskytuje jemně odstupňovanou kontrolu na [-policy základ](active-directory-b2c-reference-policies.md), z:

1. Životnosti vysílaných Azure Active Directory (Azure AD) B2C tokenů zabezpečení.
2. Životnosti relací webové aplikace spravovat přes Azure AD B2C.
3. Formáty důležité deklarací identity v tokenech zabezpečení hello vygenerované pomocí Azure AD B2C.
4. Jednotné přihlašování (SSO) chování napříč více aplikací a zásad v svého klienta B2C.

Tato funkce v svého klienta B2C můžete takto:

1. Postupujte podle těchto kroků příliš[přejděte okno s funkcemi toohello B2C](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) na hello portálu Azure.
2. Klikněte na tlačítko **přihlášení zásady**. *Poznámka: Můžete použít tuto funkci na libovolného typu zásady, nejen na **přihlášení zásady***.
3. Kliknutím otevřete zásadu. Klikněte například na **B2C_1_SiIn**.
4. Klikněte na tlačítko **upravit** hello horní části okna hello.
5. Klikněte na tlačítko **Token, relace a konfigurace přihlášení**.
6. Proveďte požadované změny. Další informace o dostupných vlastností v následujících částech.
7. Klikněte na **OK**.
8. Klikněte na tlačítko **Uložit** na hello horní části okna hello.

## <a name="token-lifetimes-configuration"></a>Konfigurace životnosti tokenu
Azure AD B2C podporuje hello [protokol autorizace OAuth 2.0](active-directory-b2c-reference-protocols.md) pro povolení zabezpečení přístupu k prostředkům tooprotected. tooimplement tato podpora Azure AD B2C vysílá různé [tokeny zabezpečení](active-directory-b2c-reference-tokens.md). Toto jsou vlastnosti hello toomanage životnosti tokenů zabezpečení vygenerované pomocí Azure AD B2C můžete použít:

* **Přístup k & ID token životnosti (minuty)**: hello životnost hello OAuth 2.0 nosiče tokenu použité toogain přístup tooa chráněný prostředek. Azure AD B2C vydává pouze tokeny typu ID v tuto chvíli. Tato hodnota by použít tooaccess tokeny také, když jsme přidat podporu pro ně.
  * Výchozí = 60 minut.
  * Minimální (včetně) = 5 minut.
  * Maximální počet (včetně) = 1 440 minut.
* **Aktualizujte životnost tokenu (dny)**: hello maximální časové období před kterou token obnovení lze použít tooacquire nové přístupu nebo ID token (a volitelně nový token obnovení, pokud vaše aplikace byl udělen hello `offline_access` oboru).
  * Výchozí = 14 dnů.
  * Minimální (včetně) = 1 den.
  * Maximální počet (včetně) = 90 dní.
* **Aktualizujte životnost tokenu posuvné okno (dny)**: po tohoto uživatele čas období po uplynutí předem hello vynucené toore-ověření, bez ohledu na to, doba platnosti hello hello poslední aktualizace tokenu získali hello aplikací. Ho lze zadat pouze pokud je příliš nastaven přepínač hello**Bounded**. Vyžaduje toobe větší než nebo rovna toohello **životnost tokenu aktualizace (dny)** hodnotu. Pokud je příliš nastaven přepínač hello**nástrojů Unbounded**, nemůžete zadat konkrétní hodnotu.
  * Výchozí = 90 dní.
  * Minimální (včetně) = 1 den.
  * Maximální počet (včetně) = 365 dnů.

Toto je několik případů použití, které můžete povolit pomocí těchto vlastností:

* Povolit uživatele toostay, přihlášeni mobilních aplikací bez omezení, tak dlouho, dokud se na aplikace hello průběžně aktivní. To provedete pomocí nastavení hello **aktualizace posuvné okno dobu životnosti tokenu (dny)** přepínač příliš**nástrojů Unbounded** v zásadách vaše přihlášení.
* Splňovat požadavky na zabezpečení a dodržování předpisů vaší odvětví nastavením životnosti tokenu hello odpovídající přístup.

    > [!NOTE]
    > Tato nastavení nejsou k dispozici pro zásady resetování hesla.
    > 
    > 

## <a name="token-compatibility-settings"></a>Nastavení kompatibility tokenu
Jsme provedli formátování deklarací tooimportant změny v tokenech zabezpečení vygenerované pomocí Azure AD B2C. To se provádí tooimprove naše podpory standardní protokol a pro lepší spolupráci s knihovny identity jiných výrobců. Ale tooavoid narušující existující aplikace, jsme vytvořili hello následující vlastnosti tooallow zákazníkům tooopt – podle potřeby:

* **Deklarace identity vystavitele (iss)**: Toto identifikuje klienta hello Azure AD B2C, která vydala hello token.
  * `https://login.microsoftonline.com/{B2C tenant GUID}/v2.0/`: Toto je výchozí hodnota hello.
  * `https://login.microsoftonline.com/tfp/{B2C tenant GUID}/{Policy ID}/v2.0/`: Tato hodnota zahrnuje ID pro klienta hello B2C i hello zásady používané v žádosti o token hello. Pokud vaše aplikace nebo knihovny musí Azure AD B2C toobe kompatibilní s hello [OpenID Connect 1.0 zjišťování specifikace](http://openid.net/specs/openid-connect-discovery-1_0.html), používejte tuto hodnotu.
* **Deklarace identity subjektu (pod)**: Toto identifikuje hello entity, tedy hello uživatele, pro které hello token vyhodnotí informace.
  * **ObjectID**: Toto je výchozí hodnota hello. Vyplní ID objektu hello hello uživatele v adresáři hello do hello `sub` deklarací identity v tokenu hello.
  * **Nepodporuje**: to je k dispozici pouze z důvodů zpětné kompatibility, a doporučujeme vám, že jste přepnuli příliš**ObjectID** také budete moci.
* **Deklarace identity představující ID zásad**: Toto identifikuje typ deklarace identity hello, do které hello se naplní ID zásady použité v žádosti o token hello.
  * **tfp**: Toto je výchozí hodnota hello.
  * **acr**: to je k dispozici pouze z důvodů zpětné kompatibility, a doporučujeme vám, že jste přepnuli příliš`tfp` také budete moci.

## <a name="session-behavior"></a>Chování relace
Azure AD B2C podporuje hello [ověřovacího protokolu OpenID Connect](active-directory-b2c-reference-oidc.md) pro povolení zabezpečeného přihlášení tooweb aplikací. Toto jsou hello vlastnosti, které můžete použít toomanage webové aplikace relace:

* **Webovou aplikaci dobu platnosti relace (minuty)**: hello životnost uložené na prohlížeče hello uživatele po úspěšném ověření souboru cookie relace Azure AD B2C.
  * Výchozí = 1 440 minut.
  * Minimální (včetně) = 15 minut.
  * Maximální počet (včetně) = 1 440 minut.
* **Časový limit relace webové aplikace**: Pokud je tento přepínač nastavená příliš**absolutní**, uživatel hello je vynucené toore-ověření po hello určeného časového období **webové aplikace dobu platnosti relace (minuty)** uplyne. Pokud je tento přepínač nastavená příliš**kolejová** (hello výchozí nastavení), tak dlouho, dokud uživatel hello je neustále aktivní ve webové aplikaci zůstane přihlášeného uživatele hello.

Toto je několik případů použití, které můžete povolit pomocí těchto vlastností:

* Splňovat požadavky na zabezpečení a dodržování předpisů vaší odvětví nastavením hello příslušné webové aplikaci relace životnosti.
* Vynutí opětovné ověření po nastaveném časovém období, během interakce uživatele s vysokou úrovní zabezpečení součást webové aplikace. 

    > [!NOTE]
    > Tato nastavení nejsou k dispozici pro zásady resetování hesla.
    > 
    > 

## <a name="single-sign-on-sso-configuration"></a>Konfigurace jednotného přihlašování (SSO)
Pokud máte víc aplikací a zásad v svého klienta B2C, můžete spravovat interakcí mezi nimi pomocí hello **konfigurace přihlášení** vlastnost. Můžete nastavit vlastnost tooone hello Dobrý den následující nastavení:

* **Klienta**: Toto je výchozí nastavení hello. Použití tohoto nastavení umožňuje více aplikací a zásad v svého klienta B2C tooshare hello stejné uživatelské relace. Například Jakmile se uživatel přihlásí do aplikace, nákupního Contoso, ale můžete také bezproblémově Přihlaste se k jiné jeden, farmacie Contoso, při k ní přistupují.
* **Aplikace**: To vám umožní toomaintain relaci uživatele výhradně pro aplikaci, nezávisle na jiné aplikace. Například, pokud chcete, aby uživatel toosign hello v tooContoso farmacie (s hello stejné přihlašovací údaje) i v případě, že uživatel je již přihlášen k nakupování Contoso, jiná aplikace na hello stejné klienta B2C. 
* **Zásady**: To vám umožní toomaintain relaci uživatele výhradně pro zásadu nezávislé hello aplikací používat. Například pokud uživatel hello již přihlášení a dokončení krok více Multi-Factor authentication (MFA), potvrdí je možné přidělit přístup toohigher zabezpečení částí více aplikací, dokud nevyprší hello relace vázané toohello zásad.
* **Zakázané**: Vynutí hello toorun uživatele prostřednictvím hello celé uživatelské cesty na každé provedení hello zásad. Například to vám umožní více uživatelů toosign až tooyour aplikace (ve sdílené plochy scénáři), i když celou dobu hello zůstane přihlášeného jednoho uživatele.

    > [!NOTE]
    > Tato nastavení nejsou k dispozici pro zásady resetování hesla.
    > 
    > 

