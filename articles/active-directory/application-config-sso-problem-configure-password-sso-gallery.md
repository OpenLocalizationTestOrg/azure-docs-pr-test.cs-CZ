---
title: "aaaProblem konfigurace hesla jednotné přihlašování pro aplikaci Galerie Azure AD | Microsoft Docs"
description: "Pochopení hello běžné problémy uživatelé setkávají při konfiguraci hesla jednotné přihlašování pro aplikace, které jsou již uveden v hello Azure AD Application Gallery"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 78c37c52453c375bf7ccbca6df5c9008be4ce642
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Problém konfigurace hesla jednotné přihlašování pro aplikaci Galerie Azure AD

Tento článek vám pomůže toounderstand hello běžné problémy uživatelé setkávají při konfiguraci **heslo jednotné přihlašování** pomocí aplikace pro galerii Azure AD.

## <a name="credentials-are-filled-in-but-hello-extension-does-not-submit-them"></a>Přihlašovací údaje jsou vyplněna, ale je odeslat není hello rozšíření

To obvykle proběhne, pokud dodavatele aplikace hello došlo ke změně jejich přihlašovací stránka nedávno tooadd pole, změňte základní identifikátor jsme použili toodetect hello uživatelské jméno a heslo pole nebo změnit, jak hello přihlašovací prostředí funguje pro svou aplikaci. Naštěstí v mnoha případech Microsoft můžete pracovat s aplikací dodavatelé toorapidly vyřešit tyto problémy.

Když společnost Microsoft nemá technologie tooautomatically rozpoznat, kdy se tyto integrace přerušení, ale někdy jsme nejsou možné toofind tyto problémy, že tokeny nebo trvat čas toofix. V případě hello při jednu z těchto integrace nebude fungovat správně, uvítáme při otevření případu podpory, aby jsme můžete provést co nejrychleji odstraňovat.

V přidání toothis **Pokud jste v kontaktu s od dodavatele této aplikace,** **jejich odeslání naše způsob** , jsme mohli pracovat s nimi toonatively integraci svých aplikací s Azure Active Directory. Můžete odeslat hello dodavatele toohello [výpis aplikace v galerii aplikací Azure Active Directory hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget je spuštěna.

## <a name="credentials-are-filled-in-and-submitted-but-hello-page-indicates-hello-credentials-are-incorrect"></a>Přihlašovací údaje jsou vyplněny a odeslání, ale stránku hello označuje, že hello přihlašovací údaje jsou nesprávné

tooresolve-li tento problém, první hello následující kontroly:

-   Uživatel hello nejdřív pokusí použít příliš**přihlášení webu toohello aplikace přímo** s hello přihlašovací údaje uložené pro ně.

  * Pokud to funguje, pak mít hello uživatele klikněte na tlačítko hello **aktualizovat přihlašovací údaje** na hello tlačítko **dlaždice aplikace** v hello **aplikace** části hello [aplikace Přístup k panelu](https://myapps.microsoft.com/) tooupdate je toohello nejnovější známé práce uživatelské jméno a heslo.

   * Pokud jste nebo jiné přihlašovací údaje správce přiřazené hello pro tohoto uživatele najít hello uživatele nebo skupiny aplikací přiřazení přechodem toohello **uživatelé a skupiny** karta aplikace hello výběr hello přiřazení a Kliknutím na tlačítko hello **pověření aktualizace** tlačítko.

-   Pokud uživatel hello vlastní pověření, mít hello uživatele **zkontrolujte toobe jistotu, že v aplikaci hello nevypršela platnost hesla pro** a pokud ano, **aktualizovat heslo, jehož platnost vypršela** přihlášením toohello aplikace přímo.

   * Po aktualizaci hello heslo aplikace hello požadavku hello uživatele tooclick hello **aktualizovat přihlašovací údaje** na hello tlačítko **dlaždice aplikace** v hello **aplikace** část hello [Panel přístupu aplikace](https://myapps.microsoft.com/) tooupdate je toohello nejnovější známé práce uživatelské jméno a heslo.

   * Pokud jste nebo jiné přihlašovací údaje správce přiřazené hello pro tohoto uživatele najít hello uživatele nebo skupiny aplikací přiřazení přechodem toohello **uživatelé a skupiny** karta aplikace hello výběr hello přiřazení a Kliknutím na tlačítko hello **pověření aktualizace** tlačítko.

-   Mít rozšíření prohlížeče panely hello hello uživatele aktualizace přístup pomocí následujících kroků hello níže v hello [jak tooinstall hello rozšíření přístup k panelu prohlížeče](#how-to-install-the-access-panel-browser-extension) části.

-   Zkontrolujte, zda rozšíření prohlížeče panely přístup hello běží a je povolený v prohlížeči uživatele.

-   Ujistěte se, že uživatelé nejsou při toosign v aplikaci toohello z hello přístupového panelu při v **privátní, se službou inPrivate nebo incognito režimu**. v těchto režimech nepodporuje rozšíření panely Hello přístup.

V případě, že to nefunguje, může to být hello případě, že došlo ke změně na straně aplikace hello, který je dočasně poškozena hello aplikace integraci s Azure AD. Například tato situace může nastat při dodavatele aplikace hello zavádí skript na jejich stránce, která pracuje odlišně pro ruční vs automatizovaný vstup, který spustí automatizované integraci, jako jsou vlastní, toobreak. Naštěstí v mnoha případech Microsoft můžete pracovat s aplikací dodavatelé toorapidly vyřešit tyto problémy.

Když společnost Microsoft nemá technologie tooautomatically rozpoznat, kdy se tyto integrace přerušení, ale někdy jsme nejsou možné toofind tyto problémy, že tokeny nebo trvat čas toofix. V případě hello při jednu z těchto integrace nebude fungovat správně, uvítáme při otevření případu podpory, aby jsme můžete provést co nejrychleji odstraňovat.

V přidání toothis **Pokud jste v kontaktu s od dodavatele této aplikace,** **jejich odeslání naše způsob** , jsme mohli pracovat s nimi toonatively integraci svých aplikací s Azure Active Directory. Můžete odeslat hello dodavatele toohello [výpis aplikace v galerii aplikací Azure Active Directory hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget je spuštěna.

## <a name="hello-extension-works-in-chrome-and-firefox-but-not-in-internet-explorer"></a>rozšíření Hello funguje v Chrome a Firefox, ale ne v aplikaci Internet Explorer

Existují dvě hlavní příčiny problému toothis:

-   V závislosti na nastavení zabezpečení hello v Internet Exploreru povolená, pokud hello webu není součástí **zóny Důvěryhodné**, někdy se zablokovat naše skript spouští aplikace hello.

  *  tooresolve, vyzvat uživatele hello příliš**přidat web aplikace hello** toohello **Důvěryhodné servery** seznamu v rámci jejich **nastavení zabezpečení aplikace Internet Explorer**. Můžete odeslat vaši uživatelé toohello [jak tooadd toomy lokality důvěryhodných lokalit seznamu](https://answers.microsoft.com/en-us/ie/forum/ie9-windows_7/how-do-i-add-a-site-to-my-trusted-sites-list/98cc77c8-b364-e011-8dfc-68b599b31bf5) podrobné pokyny najdete v článku.

-   Ve výjimečných případech ověření zabezpečení aplikace Internet Explorer můžete někdy způsobit tooload stránku hello pomaleji než hello provedení naše skriptu.

   * Bohužel se této situaci může lišit v závislosti na hello verze prohlížeče, rychlost počítače nebo serveru navštívil. V takovém případě doporučujeme kontaktovat podporu, takže jsme můžete řešit hello integrace pro tuto konkrétní aplikaci.

V přidání toothis **Pokud jste v kontaktu s od dodavatele této aplikace,** **jejich odeslání naše způsob** , jsme mohli pracovat s nimi toonatively integraci svých aplikací s Azure Active Directory. Můžete odeslat hello dodavatele toohello [výpis aplikace v galerii aplikací Azure Active Directory hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget je spuštěna.

## <a name="check-if-hello-applications-login-page-has-changed-recently-or-requires-an-additional-field"></a>Zkontrolujte, zda hello na přihlašovací stránku aplikace se nedávno změnila nebo vyžaduje další pole

Přihlašovací stránku hello aplikace změnil výrazně, někdy způsobí, že naše toobreak integrace. Příkladem je při dodavatele aplikace přidá znaménko v poli, captcha, nebo dojde tootheir služby Multi-Factor authentication. Naštěstí v mnoha případech Microsoft můžete pracovat s aplikací dodavatelé toorapidly vyřešit tyto problémy.

Když společnost Microsoft nemá technologie tooautomatically rozpoznat, kdy se tyto integrace přerušení, ale někdy jsme nejsou možné toofind tyto problémy hned. V opačném případě trvat některé toofix čas. V případě hello při jednu z těchto integrace nebude fungovat správně, uvítáme otevírání případu podpory, aby jsme můžete provést co nejrychleji odstraňovat.

V přidání toothis **Pokud jste v kontaktu s od dodavatele této aplikace,** **jejich odeslání naše způsob** , jsme mohli pracovat s nimi toonatively integraci svých aplikací s Azure Active Directory. Můžete odeslat hello dodavatele toohello [výpis aplikace v galerii aplikací Azure Active Directory hello](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing) tooget je spuštěna.

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Jak tooinstall hello rozšíření přístup k panelu prohlížeče

tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:

1.  Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.

3.  V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení. **Přidat** hello rozšíření tooyour prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** aplikace heslo SSO

Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu Firefox přístup](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)

