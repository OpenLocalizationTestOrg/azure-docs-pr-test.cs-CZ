---
title: "Azure AD Connect: Předávací ověřování – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toofrequently kladené dotazy týkající se Azure Active Directory předávací ověřování."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: billmath
ms.openlocfilehash: 550e2599177682f8ea971a05485dd5ac12b9b072
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory předávací ověřování: Nejčastější dotazy

V tomto článku jsme adres nejčastější dotazy k Azure Active Directory (Azure AD) předávací ověřování. Kontrolovat zpět pro nový obsah.

>[!IMPORTANT]
>Funkce předávací ověřování Hello je aktuálně ve verzi preview.

## <a name="which-of-hello-azure-ad-sign-in-methods---pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs---should-i-choose"></a>Které hello Azure AD přihlášení metody - předávací ověřování, synchronizaci hodnoty Hash hesla a služby Active Directory Federation Services (AD FS) - by měl vybrat?

To závisí na místním prostředí a požadavků organizace. Přečtěte si tento článek [porovnání hello různé Azure AD přihlášení metody](active-directory-aadconnect-user-signin.md).

## <a name="is-pass-through-authentication-a-free-feature"></a>Předávací ověřování je bezplatná funkce?

Předávací ověřování je bezplatné funkce a nepotřebujete žádné placené edice Azure AD toouse ho. Zůstane volné, když funkce hello dosáhne obecné dostupnosti.

## <a name="is-pass-through-authentication-available-in-microsoft-cloud-germanyhttpwwwmicrosoftdecloud-deutschland-and-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Je k dispozici v předávací ověřování [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) a [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/)?

Ne, předávací ověřování je k dispozici pouze v hello world wide instanci Azure AD.

## <a name="does-conditional-accessactive-directory-conditional-accessmd-work-with-pass-through-authentication"></a>Nemá [podmíněného přístupu](../active-directory-conditional-access.md) pracovní pomocí předávacího ověřování?

Ano, všechny funkce podmíněného přístupu, včetně ověřování Azure Multi-Factor Authentication, pracovat s předávací ověřování.

## <a name="does-pass-through-authentication-support-alternate-id-as-hello-username-instead-of-userprincipalname"></a>Podporuje předávací ověřování "Alternativní ID" jako uživatelské jméno hello místo "userPrincipalName"?

Ano. Předávací ověřování podporuje `Alternate ID` jako hello uživatelské jméno při konfiguraci ve službě Azure AD Connect, jak je znázorněno [zde](active-directory-aadconnect-get-started-custom.md). Ne všechny aplikace Office 365 podporují `Alternate ID`. Pro příkaz hello podpory naleznete v dokumentaci toohello konkrétní aplikaci.

## <a name="does-password-hash-synchronization-act-as-a-fallback-toopass-through-authentication"></a>Synchronizaci hodnoty Hash hesla fungovat jako záložní ověřování tooPass prostřednictvím?

Ne, synchronizaci hodnoty Hash hesla není obecné záložní tooPass prostřednictvím ověřování. Pouze slouží jako záložní pro [scénáře, které předávací ověřování nepodporuje Dnes](active-directory-aadconnect-pass-through-authentication-current-limitations.md#unsupported-scenarios). tooavoid uživatele neúspěšných přihlášení, měli byste nakonfigurovat předávací ověřování pro [vysokou dostupnost](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxyactive-directory-application-proxy-get-startedmd-connector-on-hello-same-server-as-a-pass-through-authentication-agent"></a>Je možné nainstalovat [Azure AD Application Proxy](../active-directory-application-proxy-get-started.md) konektor na hello stejný server jako předávací ověřování agenta?

Ano, tato konfigurace je podporovaná s hello přejmenované verzích hello předávací ověřování agenta (verze 1.5.193.0 nebo novější).

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Jaké verze Azure AD Connect a předávací ověřování agenta potřebujete?

Je třeba verze 1.1.486.0 nebo novější pro Azure AD Connect a 1.5.58.0 nebo novější pro hello předávací ověřování agenta pro tuto funkci toowork. Veškerý software by měly být nainstalovány na serverech se systémem Windows Server 2012 R2 nebo novější.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-toosign-in-using-pass-through-authentication"></a>Co se stane, pokud vypršela platnost hesla pro daného uživatele a zkuste to toosign pomocí předávacího ověřování?

Pokud jste nakonfigurovali [zpětný zápis hesla](../active-directory-passwords-update-your-own-password.md) pro konkrétního uživatele, a pokud hello uživatel se přihlásí pomocí předávacího ověřování, můžou změnit a resetovat jejich hesla. Hello hesel, zapíšou se zpět tooon místní službě Active Directory podle očekávání.

Ale pokud zpětný zápis hesla není nakonfigurován pro konkrétního uživatele nebo pokud hello uživatel nemá platný Azure AD přiřazenou, licenci hello uživatele nelze aktualizovat heslo, v cloudu hello. Jejich nelze aktualizovat heslo, i v případě, že vypršela platnost jejich hesla. Hello uživateli se místo toho zobrazí tato zpráva: "vaše organizace vám neumožňuje tooupdate heslo na tomto webu. Prosím aktualizovat podle toohello metoda doporučovaným vaší organizací, nebo požádejte správce, pokud potřebujete pomoc." Hello uživatel nebo správce hello má tooreset hesla ve vaší místní službě Active Directory.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Jak předávací ověřování ochraně před útoky hrubou silou heslo?

Čtení [v tomto článku](active-directory-aadconnect-pass-through-authentication-smart-lockout.md) Další informace.

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Co předávací ověřování agenti komunikovat přes porty 80 a 443?

- Agenti ověřování Hello provádět požadavky HTTPS přes port 443 pro všechny operace funkce.
- Agenti ověřování Hello navázat požadavky HTTP přes port 80 toodownload SSL seznamy odvolaných certifikátů.

     >[!NOTE]
     >Poslední aktualizace jsme snižuje počet hello porty vyžadované službou hello funkce. Pokud máte starší verze Azure AD Connect nebo hello Agent ověřování, nechte těchto portů také otevřené: 5671, 8080, 9090, 9091, 9350, 9352 a 10100 10120.

## <a name="can-hello-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Hello předávací ověřování agenti komunikovat přes odchozí webového proxy serveru?

Ano. Pokud WPAD (Web Proxy Auto-Discovery) je povoleno v místním prostředí, ověřování agenty automaticky pokusí toolocate a použití webového proxy serveru v síti hello.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-hello-same-server"></a>Je možné nainstalovat dvě nebo více agentů předávací ověřování na hello stejný server?

Ne, můžete nainstalovat pouze jeden Agent předávací ověřování na jednom serveru. Pokud chcete tooconfigure předávací ověřování pro zajištění vysoké dostupnosti, postupujte podle pokynů hello v tomto [článku](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability) místo.

## <a name="i-already-use-active-directory-federation-services-ad-fs-for-azure-ad-sign-in-how-do-i-switch-it-toopass-through-authentication"></a>Používám již Active Directory Federation Services (AD FS) pro přihlášení k Azure AD. Jak lze přepnout ho tooPass prostřednictvím ověřování?

Pokud jste nakonfigurovali službu AD FS jako způsob přihlášení pomocí Průvodce hello Azure AD Connect, změňte metoda přihlašování uživatele hello tooPass prostřednictvím ověřování. Tato změna umožňuje předávací ověřování na hello klienta a převede _všechny_ federovaný domény na spravované domény. Všechny následné přihlášení požadavky ve vašem klientovi jsou zpracovávány předávací ověřování. V současné době není nijak podporované v rámci Azure AD Connect toouse kombinaci služby AD FS a předávací ověřování mezi různými doménami.

Pokud byl služby AD FS nakonfigurovaný jako metoda přihlašování hello _mimo_ Průvodce hello Azure AD Connect, změnit metoda přihlašování uživatele hello tooPass prostřednictvím ověřování (z možnost hello "Nekonfigurujte"). Tato změna umožňuje předávací ověřování na hello klienta. Ale všechny domény federovaný pokračovat toouse služby AD FS pro přihlášení. Pomocí prostředí PowerShell toomanually převést některé nebo všechny tyto federovaný domén tooManaged domény. Potom všechny požadavky přihlášení na spravované domény (_pouze_) jsou zpracovávány předávací ověřování.

>[!IMPORTANT]
>Předávací ověřování nemůže pracovat s přihlášení ke cloudové službě Azure AD users.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-ad-environment"></a>Můžete použít předávací ověřování v prostředí s více doménovými strukturami AD?

Ano. Prostředí s více doménovými strukturami jsou podporovány, pokud existují vztahy důvěryhodnosti doménové struktury mezi doménovými strukturami vaší AD a v případě směrování přípon názvů je správně nakonfigurován.

## <a name="do-pass-through-authentication-agents-provide-load-balancing-capability"></a>Poskytují předávací ověřování agentů funkce Vyrovnávání zatížení?

Ne, instalace více agentů předávací ověřování zajišťuje [vysokou dostupnost](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-5-ensure-high-availability), ale není vyrovnávání zatížení. Jeden nebo dva hello ověřování agentů může stát zpracování hello hromadné hello přihlášení požadavků.

Ověření požadavků heslo, které hello ověřování agentů nutné toohandle jsou jednoduché. Proto se dvě nebo tři agenty ověřování celkem zpracovává snadno ve špičce a průměrnou zátěž pro většinu zákazníků.

Doporučujeme nainstalovat agenty ověřování zavřít tooyour řadiče domény tooimprove přihlášení latence.

## <a name="can-i-install-hello-first-pass-through-authentication-agent-on-a-server-other-than-hello-one-that-runs-azure-ad-connect"></a>Můžete nainstalovat hello prvního předávací ověřování agenta na jiném serveru než hello ten, který spouští Azure AD Connect?

Ne, tento scénář je _není_ podporována.

## <a name="how-many-pass-through-authentication-agents-should-i-install"></a>Kolik předávací ověřování agenty nainstalovat?

Doporučujeme, aby:

- Je celkem nainstalovat dvě nebo tři agenty ověřování. Tato konfigurace je dostatečné pro většinu zákazníků.
- Instalujete agenty ověřování na řadiče domény (nebo jako zavřít toothem nejdříve) tooimprove přihlášení latence.
- Můžete zajistěte, aby servery (kde ověřování jsou agenti nainstalovaní) jsou přidány toohello stejnou AD doménové struktury jako hello uživatele, jejichž hesla se musí ověřit toobe.

>[!NOTE]
>Existuje omezení systému 12 ověřování agentů podle jednotlivých klientů.

## <a name="how-can-i-disable-pass-through-authentication"></a>Jak můžete zakázat předávací ověřování?

Spusťte znovu Průvodce Azure AD Connect hello a změňte metoda přihlašování uživatele hello z metody tooanother předávací ověřování. Tato změna zakáže předávací ověřování na hello klienta a odinstaluje hello ověřování agenta ze serveru hello. Máte toomanually odinstalovat hello ověřování agentů z jiných serverů.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Co se stane, když odinstalovat agenta předávací ověřování?

Odinstalování agenta předávací ověřování ze serveru způsobí, že ho toostop přijímání žádostí o přihlášení. Zajistěte, abyste měli jiné ověřování agenta spuštěného před provedením této operace, tooavoid rozdělení uživatele přihlásit z vašeho klienta.

## <a name="next-steps"></a>Další kroky
- [**Aktuální omezení** ](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – tato funkce je aktuálně ve verzi preview. Zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [**Rychlý Start** ](active-directory-aadconnect-pass-through-authentication-quick-start.md) – zprovoznění a systémem Azure AD předávací ověřování.
- [**Podrobné technické informace** ](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**Azure AD bezproblémové SSO** ](active-directory-aadconnect-sso.md) -Další informace o této funkci vzájemně doplňují.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
