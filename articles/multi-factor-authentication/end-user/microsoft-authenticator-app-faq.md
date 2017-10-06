---
title: "aaaMicrosoft ověřovací aplikaci Nápověda a podpora | Microsoft Docs"
description: "Obsahuje seznam často kladené otázky a odpovědi související toohello aplikaci Microsoft Authentication a ověřování Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: afba9b59ccaac60d022e8516fcf573dcfbf68af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-authenticator-app-faq"></a>Aplikace Microsoft Authenticator – nejčastější dotazy

Tento článek obsahuje odpovědi na běžné otázky, které obdržíme o aplikaci Microsoft Authenticator hello. Pokud nevidíte odpovědí tooyour otázku, přejděte toohello [fórum aplikace Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Máme také jiné – nejčastější dotazy o konkrétní funkci v aplikaci hello [přihlásit pomocí vašeho telefonu – nejčastější dotazy](microsoft-authenticator-app-phone-signin-faq.md).

aplikace Microsoft Authenticator Hello nahradit aplikaci Azure Authenticator hello a hello doporučuje aplikace při použití Azure Multi-Factor Authentication. je k dispozici pro aplikaci Microsoft Authenticator Hello [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="what-are-hello-codes-in-hello-app-for-why-does-hello-number-keep-counting-down"></a>Jaké jsou kódy hello v hello aplikaci pro? Proč zachovat hello číslo počítání?

Když otevřete aplikaci Microsoft Authenticator hello, zobrazí hello účty, které jste přidali a číslem nebo osm šestimístný každou z nich. Může se zobrazit 30 sekundu časovač odpočítávání.

Tyto kódy se používají při přihlášení tooyour účtu. Po zadání uživatelského jména a hesla, je výzva tooenter ověřovací kód. Otevřete hello Microsoft Authenticator aplikace a zkopírujte hello kód, který je aktuálně se zobrazuje. Tento kód zadejte v toofinish přihlašovací stránku hello.

Důvod Hello změna každých 30 sekund je tak, aby nikdy používat kódy hello dvakrát hello stejný kód. Není jako heslo, že jste by měl tooremember. Rada Hello je, že pouze uživatelé s phone tooyour přístup zná váš ověřovací kód.

kódy Hello nevyžadují internet nebo data, takže není nutné tooworry o s toosign telefonní služby v. Při zavření aplikace hello není dál běžet v pozadí hello a ho nebude vyprazdňování baterie. Okno můžete zavřít aplikace hello a ignorovat dokud hello při příštím přihlášení.  

### <a name="i-only-get-notifications-when-i-have-hello-app-open-if-hello-app-isnt-open-i-dont-get-any-notifications"></a>Pokud aplikace hello otevřít jenom zobrazí oznámení. Pokud aplikace hello není otevřený, nezobrazí žádná upozornění.

Pokud budete dostávat oznámení, ale jejich nemáte zkontrolujte šumu nebo zavibrovat navzdory vaší vyzvánění probíhá na, nejdřív zkontrolujte nastavení aplikace hello. Povolit hello aplikace toouse zvuk nebo zavibrovat s jeho oznámení.

Pokud neobdržíte oznámení vůbec, zkontrolujte hello následujících případech:

- Váš telefon je v režimu nebudou narušovat nebo tichý? Tento režim můžete ponechat aplikace z odesílání oznámení.
- Můžete přijímat oznámení z jiných aplikací? Pokud ne, může být problém s hello síťových připojení v telefonu, nebo hello kanál oznámení z Android nebo Apple. První možnost hello můžete vyřešit v nastavení telefonu, ale musíte poskytovatele služeb tooyour tootalk pro pomoc s druhou možnost hello.
- Můžete přijímat oznámení pro některé účty v aplikaci hello, ale jiné ne? Pokud ano, odeberte účet problematické hello z vaší aplikace a ji znovu přidejte tooenable nabízená oznámení. 

Pokud se pokusilo tyto návrhy, ale i nadále s problémy, pošlete nám svoje protokoly pro diagnostiku. Přejděte toohello nastavení aplikace a pak vyberte **Nápověda a zpětnou vazbu** a **odeslání protokolů s**. Pak přejděte toohello [fórum aplikace Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) a dejte nám vědět, co problém zobrazeny a jak se zkusili dosavadní práce. 

### <a name="im-already-using-hello-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-tooone-click-push-notifications"></a>Již používám hello aplikace Microsoft Authenticator pro ověřovací kódy. Jak lze přepnout tooone kliknutím nabízená oznámení?
Schválení přihlášení prostřednictvím nabízených oznámení je dostupná jenom pro osobní účty Microsoft nebo pracovní a školní účty Microsoft, nikoli pro účty třetích stran jako Google nebo Facebook. Pokud máte pracovní nebo školní účet Microsoft, vaše organizace může toodisable tuto volbu.

Pokud jste pro svůj osobní účet pomocí účtu Microsoft a chcete tooswitch přes toopush oznámení, musíte tooadd účtu znovu. Znovu registrovat zařízení hello s vaším účtem a nastavit nabízená oznámení.  

Pokud používáte Microsoft Authenticator pro svůj pracovní nebo školní účet, pak vaše organizace rozhodne, zda oznámení tooallow jedním kliknutím.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Fungují jedním kliknutím nabízená oznámení pro účty třetích stran?
Ne, nabízená oznámení pracovat pouze s účty Microsoft a účty služby Azure Active Directory. Pokud vaše práce nebo škola používá účty Azure AD, se může tuto funkci zakázat.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Moje zařízení I obnovena ze zálohy a Můj účet kódy jsou chybí nebo nepracuje. Co se přihodilo?
Z bezpečnostních důvodů jsme nemáte účty obnovit ze zálohy aplikace.  Po obnovení aplikace hello vaše účty odstranit a znovu přidejte.

### <a name="i-got-a-new-device-how-do-i-remove-hello-microsoft-authenticator-app-from-my-old-device-and-move-toohello-new-one"></a>Zobrazí chybové nového zařízení. Jak odebrat aplikaci Microsoft Authenticator hello ze staré zařízení a přesunout toohello novým?
Přidání hello Microsoft Authenticator aplikace tooa nové zařízení neodebere automaticky ji z jiných zařízení. toomanage zařízení, která jsou nakonfigurované pro váš účet, navštivte hello stejné webové stránky použijte toomanage dvoustupňové ověřování a zvolte tooremove staré aplikace.

Pro osobní účty Microsoft, tento web je vaše [zabezpečení účtů](https://account.microsoft.com/security) stránky. Pro pracovní nebo školní účty Microsoft, tento web může být buď [MyApps](https://myapps.microsoft.com) nebo vlastní portál, který vaše organizace nastavila.

### <a name="how-do-i-remove-an-account-from-hello-app"></a>Jak odebrat účet z aplikace hello?
* iOS: Z hlavní obrazovky hello, prstem na dlaždici účtu vlevo. Vyberte **Odstranit**.
* Windows Phone: Úvodní obrazovka hlavní, vyberte tlačítko hello nabídky, pak **upravit účty**. Klepněte na hello **X** další toohello název účtu.
* Android: Úvodní obrazovka hlavní, vyberte tlačítko hello nabídky, pak **upravit účty**. Klepněte na hello **X** další toohello název účtu.

Pokud máte zařízení, které je registrované ve vaší organizaci, musíte toocomplete další krok tooremove váš účet. U těchto zařízení se automaticky registruje aplikaci Microsoft Authenticator hello jako správce zařízení. Pokud chcete, aby aplikace hello toocompletely odinstalovat, je třeba toofirst zrušit registraci aplikace hello v nastavení aplikace hello.

### <a name="why-does-hello-app-request-so-many-permissions"></a>Proč aplikace hello požadavku mnoho oprávnění?
Tady je seznam oprávnění, která jsme může požádat o úplnou a jak se používají v aplikaci hello. Hello konkrétní oprávnění, které vidíte závisí na typu hello telefon, který máte.

* **Fotoaparát**: používáme vaší kódy tooscan QR fotoaparát, když přidáte pracovní, školní nebo jiných společností než Microsoft účtu.
* **Kontakty a phone**: když se přihlásíte pomocí svého osobního účtu Microsoft, můžeme akci toosimplify hello proces hledání existujících účtů, které používáte na váš telefon.
* **SMS**: když se přihlásíte pomocí svého osobního účtu Microsoft pro hello poprvé, máme toomake jistotu, že vaše telefonní číslo odpovídá hello jeden máme na záznam. Odešleme telefon toohello text zprávy, které jste stáhli aplikace hello. Hello zpráva obsahuje kód pro ověření číslice 6-8. Místo s dotazem, toofind tento kód a zadejte ho v aplikaci hello jsme vyhledání v hello textovou zprávu za vás.
* **Zakreslit nad jinými aplikacemi**: Když obdržíte oznámení tooverify vaši identitu, zobrazuje se tohoto oznámení nad jiné aplikace, které můžou běžet.
* **Přijímat data z hello internet**: Toto oprávnění je požadována pro odesílání oznámení.
* **Zabránit phone z režimu spánku**: když si zaregistrujete zařízení ve vaší organizaci, můžou změnit zásady na váš telefon.
* **Řízení vibrace**: můžete zvolit, jestli chcete vibracím při každém přijetí oznámení tooverify vaši identitu.
* **Používat otisk prstu hardware**: Některé pracovní a školní účty vyžadovat další kód PIN, kdykoli ověřit vaši identitu. proces toomake hello jednodušší, nám umožňují toouse hello otisk prstu místo zadávání kódu PIN.
* **Zobrazení síťových připojení**: Když přidáte účet Microsoft, aplikace hello vyžaduje připojení k síti nebo Internetu.
* **Načíst obsah hello úložiště**: Toto oprávnění je použít pouze v případě hlásíte technický problém prostřednictvím nastavení aplikace hello. Některé informace z vašeho úložiště jsou shromažďovány toodiagnose hello problém.
* **Úplný přístup k síti**: Toto oprávnění je požadována pro odesílání oznámení tooverify vaši identitu.
* **Při spuštění**: Pokud restartujete váš telefon, toto oprávnění zajistí pokračovat, dostanete oznámení tooverify vaši identitu.

### <a name="why-does-hello-microsoft-authenticator-app-allow-you-tooapprove-a-request-without-unlocking-hello-device"></a>Proč hello Microsoft ověřovací aplikace umožňuje tooapprove žádost bez odemykání hello zařízení?

Nemáte toounlock, které zařízení tooapprove ověření požadavků, protože potřebujete tooprove je, že máte telefon s vámi. Dvoustupňové ověřování vyžaduje prokázání dvě věci – věcí, které znáte a věcí, kterou máte. Dobrý den, víte, je heslo. Hello věcí, kterou máte je telefonu (nastavit aplikaci Microsoft Authenticator hello a registrován jako doklad vícefaktorového ověřování.) Proto s hello phone a schválení žádosti hello hello splňuje kritéria pro hello druhý faktor ověřování. 

### <a name="what-does-hello-lock-icon-in-hello-account-list-mean"></a>Co znamená ikonou zámku hello v seznamu účtů hello?

Ikona visacího zámku Hello označuje zařízení hello je zaregistrován ve službě Azure AD a zaregistrován toohello účet. Registrace zařízení pro systém iOS probíhá při registraci s Microsoft Intune.

## <a name="next-steps"></a>Další kroky

### <a name="contact-us"></a>Kontaktujte nás
Pokud jste v tomto poli, chceme toohear od vás. Přejděte toohello [fórum aplikace Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) toopost otázku a získání pomoci z komunity hello, nebo ponechte komentář na této stránce.


### <a name="related-topics"></a>Související témata
* [O dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) u účtů Microsoft
* [Došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md) pro svůj pracovní nebo školní účet?
* [Použít Microsoft Authenticator toosign hello v z telefonu](microsoft-authenticator-app-phone-signin-faq.md)

