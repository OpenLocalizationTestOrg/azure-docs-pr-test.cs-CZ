---
title: "Azure AD Connect: Bezproblémové jednotné přihlašování – rychlý Start | Microsoft Docs"
description: "Tento článek popisuje, jak tooget pracovat s Azure Active Directory bezproblémové jednotné přihlašování."
services: active-directory
keywords: "Co je Azure AD Connect, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: billmath
ms.openlocfilehash: 97d40ed41b3bfad9ff7e11ddbdb8de594ee85de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-seamless-single-sign-on-quick-start"></a>Azure Active Directory bezproblémové jednotné přihlašování: rychlý start

## <a name="how-toodeploy-seamless-sso"></a>Jak toodeploy bezproblémové jednotného přihlašování

Azure Active Directory bezproblémové jednotné přihlašování (Azure AD bezproblémové jednotné přihlašování) automaticky přihlásí uživatele v když jsou v podnikové síti připojené tooyour firemních počítačů. Poskytuje snadný přístup vašich uživatelů tooyour cloudové aplikace bez nutnosti jakékoli další místní komponenty.

>[!IMPORTANT]
>funkce jednotného přihlašování bezproblémové Hello je aktuálně ve verzi preview.

toodeploy bezproblémové jednotné přihlašování, budete potřebovat toofollow tyto kroky:

## <a name="step-1-check-prerequisites"></a>Krok 1: Kontrola předpokladů

Ujistěte se, že hello následující požadované součásti jsou na místě:

1. Nastavit server Azure AD Connect: Pokud používáte [předávací ověřování](active-directory-aadconnect-pass-through-authentication.md) jako způsob přihlášení, není nutná žádná další akce. Pokud používáte [synchronizaci hodnoty Hash hesla](active-directory-aadconnectsync-implement-password-synchronization.md) jako způsob přihlášení a pokud je brána firewall mezi Azure AD Connect a službou Azure AD, ověřte, že:
- Používáte-li verze 1.1.484.0 nebo novější služby Azure AD Connect.
- Azure AD Connect může komunikovat s `*.msappproxy.net` adresy URL a více port 443. Tento požadavek se vztahuje pouze v případě, že povolíte funkci hello, ne pro skutečné uživatelská přihlášení.
- Azure AD Connect provádět přímé připojení toohello IP [Azure datového centra rozsahy IP adres](https://www.microsoft.com/download/details.aspx?id=41653). Tento požadavek je opět použít, pouze v případě, že povolíte funkci hello.
2. Potřebujete přihlašovací údaje správce domény pro každou doménovou strukturu AD synchronizovat tooAzure AD (přes Azure AD Connect) a pro uživatele, jehož chcete tooenable bezproblémové jednotné přihlašování.

## <a name="step-2-enable-hello-feature"></a>Krok 2: Povolení funkce hello

Bezproblémové jednotné přihlašování se dá nastavit pomocí [Azure AD Connect](active-directory-aadconnect.md).

Pokud se jedná o novou instalaci Azure AD Connect, zvolte hello [vlastní cesta](active-directory-aadconnect-get-started-custom.md). V hello "Přihlášení uživatele" stránka zkontrolujte možnost "Povolit jednotné přihlašování" hello.

![Azure AD Connect – přihlášení uživatele](./media/active-directory-aadconnect-sso/sso8.png)

Pokud již máte instalace služby Azure AD Connect, zvolte "Změna uživatele přihlašovací stránka" na Azure AD Connect a klikněte na tlačítko Další.. Zkontrolujte možnost "Povolit jednotné přihlašování" hello.

![Azure AD Connect – přihlášení uživatele změn](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Pomocí Průvodce hello nadále pokračujte, dokud nezískáte toohello "Povolit jednotné přihlašování" stránky. Zadejte přihlašovací údaje správce domény pro každou doménovou strukturu AD synchronizovat tooAzure AD (přes Azure AD Connect) a pro uživatele, jehož chcete tooenable bezproblémové jednotné přihlašování. 

Po dokončení Průvodce hello je povoleno bezproblémové jednotného přihlašování na klientovi.

>[!NOTE]
> pověření správce domény Hello nejsou uložené ve službě Azure AD Connect nebo ve službě Azure AD, ale jsou pouze funkce použité tooenable hello.

Postupujte podle těchto pokynů tooverify, že je povoleno jednotné přihlašování bezproblémové správně:

1. Přihlaste se toohello [centra pro správu Azure Active Directory](https://aad.portal.azure.com) s přihlašovacími údaji hello globální správce pro vašeho klienta.
2. Vyberte **Azure Active Directory** na levé straně hello.
3. Vyberte **Azure AD Connect**.
4. Ověřte, že hello **bezproblémové jednotné přihlašování** zobrazí jako **povoleno**.

![Portál Azure – okno Azure AD Connect](./media/active-directory-aadconnect-sso/sso10.png)

## <a name="step-3-roll-out-hello-feature"></a>Krok 3: Zavádět hello funkce

tooroll hello funkce tooyour uživatelů, je nutné nastavení zóny intranetu tooadd dva Azure adresy URL AD (https://autologon.microsoftazuread-sso.com a https://aadg.windows.net.nsatc.net) toousers se prostřednictvím zásad skupiny ve službě Active Directory.

>[!NOTE]
> Hello následující pokyny funguje jenom pro Internet Explorer a Google Chrome v systému Windows (pokud ji sdílí sadu adres URL důvěryhodných serverů s aplikací Internet Explorer). Přečtěte si další části hello pokyny tooset Mozilla Firefox a Google Chrome na macu.

### <a name="why-do-you-need-toomodify-users-intranet-zone-settings"></a>Proč potřebujete nastavení zóny intranetu toomodify uživatelů?

Ve výchozím nastavení prohlížeč hello automaticky vypočítá hello správné zóny (Internetu nebo intranetu) z adresy URL. Například http://contoso/ je namapované toohello zóny intranetu, zatímco http://intranet.contoso.com/ je namapované toohello internetové zóny (protože hello adresa URL obsahuje tečku). Prohlížeče Neodesílat Kerberos lístky tooa koncového bodu cloudu - jako hello dvou Azure AD adres URL -, pokud jeho adresa URL je explicitně přidat zóny intranetu prohlížeče toohello.

### <a name="detailed-steps"></a>Podrobné kroky

1. Spusťte nástroj pro správu zásad skupiny hello.
2. Upravte hello zásad skupiny, která je použitá toosome nebo všech uživatelů. V tomto příkladu používáme hello **výchozí zásady domény**.
3. Přejděte příliš**uživatele Konfigurace počítače\Šablony pro správu\Součásti systému Windows\Internet Internetu řízení Panel\Security stránky** a vyberte **lokality tooZone přiřazení seznamu**.
![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso6.png)  
4. Povolit zásady hello a zadejte následující hodnoty (Azure AD adres URL které jsou předávány lístky protokolu Kerberos) hello a data (*1* označuje zóny intranetu) v dialogovém okně hello.

        Value: https://autologon.microsoftazuread-sso.com
        Data: 1
        Value: https://aadg.windows.net.nsatc.net
        Data: 1
>[!NOTE]
> Pokud chcete toodisallow někteří uživatelé pomocí bezproblémové jednotné přihlašování – například pokud tito uživatelé se přihlašujete na sdílené veřejné terminály – nastavte hello předcházející hodnoty příliš*4*. Tato akce přidá hello adresy URL Azure AD toohello zóny s omezeným přístupem a selže bezproblémové SSO celou dobu hello.

5. Klikněte na tlačítko **OK** a **OK** znovu.

![Jednotné přihlašování](./media/active-directory-aadconnect-sso/sso7.png)

### <a name="browser-considerations"></a>Důležité informace o prohlížeči

#### <a name="mozilla-firefox"></a>Mozilla Firefox

Mozilla Firefox automaticky neprovádí ověřování protokolu Kerberos. Každý uživatel má toomanually přidat hello Azure AD adresy URL tootheir Firefox nastavení pomocí hello následující kroky:
1. Spustit Firefox a zadejte `about:config` v panelu Adresa hello. Zavřete všechny oznámení, které vidíte.
2. Vyhledejte hello **identifikátory URI network.negotiate auth.trusted** předvoleb. Tato předvolba obsahuje seznam důvěryhodných serverů na Firefox ověřování protokolem Kerberos.
3. Klikněte pravým tlačítkem a vyberte "Upravit".
4. Zadejte "https://autologon.microsoftazuread-sso.com, https://aadg.windows.net.nsatc.net" v poli hello.
5. Klikněte na tlačítko "OK" a znovu otevřete prohlížeč hello.

#### <a name="safari-on-mac-os"></a>Safari v systému Mac OS

Zkontrolujte, že hello počítač se systémem Mac OS je připojený k tooAD. Zobrazí pokyny o tom, toodo, [zde](http://training.apple.com/pdf/Best_Practices_for_Integrating_OS_X_with_Active_Directory.pdf).

#### <a name="google-chrome-on-mac-os"></a>Google Chrome systému Mac OS

Google Chrome na ostatní platformy jiný systém než Windows a Mac OS, najdete v části příliš[v tomto článku](https://dev.chromium.org/administrators/policy-list-3#AuthServerWhitelist) informace o tom, jak toowhitelist hello adresy URL služby Azure AD pro integrované ověřování.

Pomocí zásad skupiny služby Active Directory třetích stran rozšíření tooroll tooFirefox hello adresy URL Azure AD a Google Chrome na uživatele počítačů Mac je mimo rozsah tohoto článku.

#### <a name="known-limitations"></a>Známá omezení

Bezproblémové SSO nefunguje v privátním režimu procházení na prohlížeče Firefox a okraj. Také nefunguje v Internet Exploreru Pokud hello prohlížeče běží v režimu rozšířené ochrany.

>[!IMPORTANT]
>Jsme nedávno vrácena podporu pro hraniční tooinvestigate zákazníka hlášené problémy.

## <a name="step-4-test-hello-feature"></a>Krok 4: Test hello funkce

Funkce hello tootest pro konkrétního uživatele, ujistěte se, že _všechny_ hello následující podmínky jsou splněné:
  - Hello uživatel přihlašuje na firemní zařízení.
  - Hello zařízení bylo tooyour dříve připojené k doméně Active Directory (AD).
  - Hello zařízení má tooyour přímé připojení k řadiči domény (DC), buď v hello podnikové pevné nebo bezdrátové síti nebo prostřednictvím připojení vzdáleného přístupu, jako je například připojení k síti VPN.
  - Máte [nasazuje hello funkce](##step-3-roll-out-the-feature) toothis uživatele pomocí zásad skupiny.

scénář hello tootest kde hello uživatel zadá pouze hello uživatelské jméno, ale není hello heslo:
- Přihlaste se k *https://myapps.microsoft.com/* v nové relaci privátní prohlížeče.

tootest hello scénář, kde uživatel hello nemá tooenter hello uživatelské jméno nebo hello heslo: 
- Přihlaste se k *https://myapps.microsoft.com/contoso.onmicrosoft.com* v nové relaci privátní prohlížeče. Nahraďte "*contoso*" s název vašeho klienta.
- Nebo se přihlaste *https://myapps.microsoft.com/contoso.com* v nové relaci privátní prohlížeče. Nahraďte "*contoso.com*" s ověřenou doménu (nikoli federované domény) ve vašem klientovi.

## <a name="step-5-roll-over-keys"></a>Krok 5: Vrácení nad klíči

V kroku 2 vytvoří Azure AD Connect účty počítačů (představující Azure AD) ve všech hello AD v doménových strukturách, na kterých jste povolili bezproblémové jednotné přihlašování. Další informace v podrobněji [zde](active-directory-aadconnect-sso-how-it-works.md). Pro lepší zabezpečení se doporučuje, že můžete často mění hello protokolu Kerberos dešifrovací klíče tyto účty počítačů.

>[!IMPORTANT]
>Nepotřebujete toodo tento krok _okamžitě_ po povolení funkce hello. Mění hello protokolu Kerberos dešifrovací klíče minimálně každých 30 dnů.

## <a name="next-steps"></a>Další kroky

- [**Podrobné technické informace** ](active-directory-aadconnect-sso-how-it-works.md) -pochopit, jak tato funkce funguje.
- [**Nejčastější dotazy** ](active-directory-aadconnect-sso-faq.md) -odpovědi toofrequently dotazy.
- [**Řešení potíží s** ](active-directory-aadconnect-troubleshoot-sso.md) – zjistěte, jak tooresolve běžné problémy s funkcí hello.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
