---
title: "Microsoft Authenticator aplikaci Nápověda a podpora | Microsoft Docs"
description: "Poskytuje seznam Časté otázky a odpovědi týkající se aplikace Microsoft Authentication a ověřování Azure Multi-Factor Authentication."
services: multi-factor-authentication
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: barlan
ms.reviewer: librown
ms.custom: end-user
ms.openlocfilehash: 073b8adfcbe5fdcc2a6d1dba820a1101fac83218
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="microsoft-authenticator-app-faq"></a>Aplikace Microsoft Authenticator – nejčastější dotazy

Tento článek obsahuje odpovědi na běžné otázky, které obdržíme o aplikaci Microsoft Authenticator. Pokud nevidíte odpověď na svoji otázku, přejděte k [fórum aplikace Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp). Máme také jiné – nejčastější dotazy o konkrétní funkci v aplikaci [přihlásit pomocí vašeho telefonu – nejčastější dotazy](microsoft-authenticator-app-phone-signin-faq.md).

Aplikace Microsoft Authenticator nahradit aplikaci Azure Authenticator a je doporučenou aplikaci, když používáte Azure Multi-Factor Authentication. Je k dispozici pro aplikaci Microsoft Authenticator [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), a [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="what-are-the-codes-in-the-app-for-why-does-the-number-keep-counting-down"></a>Jaké jsou kódy v aplikaci pro? Proč čísla zachovat počítání?

Při otevření aplikace Microsoft Authenticator uvidíte účty, které jste přidali a číslem nebo osm šestimístný každou z nich. Může se zobrazit 30 sekundu časovač odpočítávání.

Tyto kódy se používají při přihlášení ke svému účtu. Po zadání uživatelského jména a hesla, může být vyzváni k zadání ověřovací kód. Otevřete aplikaci Microsoft Authenticator a zkopírujte kód, který je aktuálně se zobrazuje. Tento kód zadejte na stránce přihlášení dokončit.

Kódy změnit každých 30 sekund důvodem je, aby bylo nikdy používat stejný kód dvakrát. Není třeba heslo, které se má mějte na paměti. Cílem je, že pouze uživatelé s přístupem na váš telefon zná váš ověřovací kód.

Kódy nevyžadují internet nebo data, takže není nutné si dělat starosti s telefonní služby k přihlášení. Při zavření aplikace nemá dál běžet na pozadí a ho nebude vyprazdňování baterie. Okno můžete zavřít aplikaci a ignorovat až po příštím přihlášení.  

### <a name="i-only-get-notifications-when-i-have-the-app-open-if-the-app-isnt-open-i-dont-get-any-notifications"></a>Pouze při se zobrazuje oznámení je nutné aplikaci otevřené. Pokud aplikace není otevřený, nezobrazí žádná upozornění.

Pokud budete dostávat oznámení, ale jejich nemáte zkontrolujte šumu nebo zavibrovat navzdory vaší vyzvánění probíhá na, nejdřív zkontrolujte nastavení aplikace. Povolte aplikaci používat zvuk nebo zavibrovat s jeho oznámení.

Pokud neobdržíte oznámení vůbec, zkontrolujte v následujících případech:

- Váš telefon je v režimu nebudou narušovat nebo tichý? Tento režim můžete ponechat aplikace z odesílání oznámení.
- Můžete přijímat oznámení z jiných aplikací? Pokud ne, může být problém s připojeních k síti na váš telefon nebo kanál oznámení z Android nebo Apple. První možností můžete vyřešit v nastavení telefonu, ale budete muset obraťte se na svého poskytovatele služeb pro pomoc s druhou možnost.
- Můžete přijímat oznámení pro některé účty v aplikaci, ale jiné ne? Pokud ano, odeberte problematické účet z vaší aplikace a přidejte ji znovu povolte nabízená oznámení.

Pokud se pokusilo tyto návrhy, ale i nadále s problémy, pošlete nám svoje protokoly pro diagnostiku. Přejděte do nastavení aplikace a pak vyberte **Nápověda a zpětnou vazbu** a **odeslání protokolů s**. Potom pokračujte [fórum aplikace Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) a dejte nám vědět, co problém zobrazeny a jak jste se pokusili, pokud.

### <a name="im-already-using-the-microsoft-authenticator-application-for-verification-codes-how-do-i-switch-to-one-click-push-notifications"></a>Aplikace Microsoft Authenticator již používám pro ověřovací kódy. Jak přepnout na jedním kliknutím nabízená oznámení?
Schválení přihlášení prostřednictvím nabízených oznámení je dostupná jenom pro osobní účty Microsoft nebo pracovní a školní účty Microsoft, nikoli pro účty třetích stran jako Google nebo Facebook. Pokud máte pracovní nebo školní účet Microsoft, vaše organizace můžete zrušit zaškrtnutí tohoto políčka.

Pokud používáte účet Microsoft pro svůj osobní účet a chcete přejít k odesílání nabízených oznámení, budete muset znovu přidejte svůj účet. Znovu registrovat zařízení s vaším účtem a nastavit nabízená oznámení.  

Pokud používáte Microsoft Authenticator pro váš pracovní nebo školní účet, pak vaše organizace rozhodne, zda povolit oznámení jedním kliknutím.

### <a name="do-one-click-push-notifications-work-for-non-microsoft-accounts"></a>Fungují jedním kliknutím nabízená oznámení pro účty třetích stran?
Ne, nabízená oznámení pracovat pouze s účty Microsoft a účty služby Azure Active Directory. Pokud vaše práce nebo škola používá účty Azure AD, se může tuto funkci zakázat.  

### <a name="i-restored-my-device-from-a-backup-and-my-account-codes-are-missing-or-not-working-what-happened"></a>Moje zařízení I obnovena ze zálohy a Můj účet kódy jsou chybí nebo nepracuje. Co se přihodilo?
Z bezpečnostních důvodů jsme nemáte účty obnovit ze zálohy aplikace.  Po obnovení aplikace, odstraňte vaše účty a znovu přidejte.

### <a name="i-got-a-new-device-how-do-i-remove-the-microsoft-authenticator-app-from-my-old-device-and-move-to-the-new-one"></a>Zobrazí chybové nového zařízení. Jak odebrat aplikaci Microsoft Authenticator z původního zařízení a přesunout do nového?
Přidávání do nového zařízení aplikaci Microsoft Authenticator neodebere automaticky ji z jiných zařízení. Spravovat zařízení, která jsou nakonfigurované pro váš účet, najdete na stejný web, který můžete použít ke správě dvoustupňové ověřování a odebrat starý aplikace.

Pro osobní účty Microsoft, tento web je vaše [zabezpečení účtů](https://account.microsoft.com/security) stránky. Pro pracovní nebo školní účty Microsoft, tento web může být buď [MyApps](https://myapps.microsoft.com) nebo vlastní portál, který vaše organizace nastavila.

### <a name="how-do-i-remove-an-account-from-the-app"></a>Jak odebrat účet z aplikace?
* iOS: Z hlavní obrazovky prstem na dlaždici účtu vlevo. Vyberte **Odstranit**.
* Windows Phone: Z hlavní obrazovky, vyberte tlačítko nabídky potom **upravit účty**. Klepněte **X** vedle názvu účtu.
* Android: Z hlavní obrazovky, vyberte tlačítko nabídky potom **upravit účty**. Klepněte **X** vedle názvu účtu.

Pokud máte zařízení, které je registrované ve vaší organizaci, musíte provést další krok pro svůj účet odeberte. U těchto zařízení se automaticky registruje aplikaci Microsoft Authenticator jako správce zařízení. Pokud chcete úplně odinstalovat aplikaci, musíte nejprve zrušit registraci aplikace v nastavení aplikace.

### <a name="why-does-the-app-request-so-many-permissions"></a>Proč aplikace požadavku mnoho oprávnění?
Tady je seznam oprávnění, která jsme může požádat o úplnou a jak se používají v aplikaci. Konkrétní oprávnění, která se zobrazí závisí na typu telefon, který máte.

* **Fotoaparát**: jsme použijte fotoaparát k skenovat kódy QR, když přidáte pracovní, školní nebo účet jiných společností než Microsoft.
* **Kontakty a phone**: když se přihlásíte pomocí svého osobního účtu Microsoft, pokusíme se zjednoduší proces podle hledání existujících účtů, které používáte na váš telefon.
* **SMS**: když se přihlásíte pomocí svého osobního účtu Microsoft poprvé, musíme Ujistěte se, zda vaše telefonní číslo odpovídá máme na záznam. Jsme odeslání textové zprávy na telefon kam jste stáhli aplikace. Zpráva obsahuje kód pro ověření číslice 6-8. Místo žádostí o najít tento kód a zadejte ho v aplikaci, jsme vyhledání v textovou zprávu za vás.
* **Zakreslit nad jinými aplikacemi**: Když obdržíte oznámení ověřit vaši identitu, zobrazuje se tohoto oznámení nad jiné aplikace, které můžou běžet.
* **Přijímat data z Internetu**: Toto oprávnění je požadována pro odesílání oznámení.
* **Zabránit phone z režimu spánku**: když si zaregistrujete zařízení ve vaší organizaci, můžou změnit zásady na váš telefon.
* **Řízení vibrace**: můžete zvolit, jestli chcete vibracím při každém přijetí oznámení ověřit vaši identitu.
* **Používat otisk prstu hardware**: Některé pracovní a školní účty vyžadovat další kód PIN, kdykoli ověřit vaši identitu. PRO usnadnění procesu nám umožňují používat otisk prstu místo zadávání kódu PIN.
* **Zobrazení síťových připojení**: Když přidáte účet Microsoft, aplikace vyžaduje připojení k síti nebo Internetu.
* **Přečíst obsah úložiště**: Toto oprávnění je použít pouze v případě hlásíte technický problém prostřednictvím nastavení aplikace. Některé informace z vašeho úložiště se shromažďují a Diagnostikujte problém.
* **Úplný přístup k síti**: Toto oprávnění je požadována pro odesílání oznámení ověřit vaši identitu.
* **Při spuštění**: Pokud restartujete váš telefon, toto oprávnění zajistí dál dostávat oznámení ověřit vaši identitu.

### <a name="why-does-the-microsoft-authenticator-app-allow-you-to-approve-a-request-without-unlocking-the-device"></a>Proč Microsoft ověřovací aplikací, které umožňuje schválit žádost o bez odemknutí zařízení?

Nemáte k odemknutí zařízení ke schválení žádosti o ověření, protože všechny, které potřebujete k prokázání je, že máte telefon s vámi. Dvoustupňové ověřování vyžaduje prokázání dvě věci – věcí, které znáte a věcí, kterou máte. Věcí, které znáte je jím vaše heslo. Věcí, kterou máte je telefonu (nastavit aplikaci Microsoft Authenticator a registrován jako doklad vícefaktorového ověřování.) Proto s telefonu a schválení žádosti splňuje kritéria pro druhý faktor ověřování.

### <a name="what-does-the-lock-icon-in-the-account-list-mean"></a>Co znamená ikonu zámku v seznamu účet?

Na ikonu zámku označuje, že je zařízení zaregistrované ve službě Azure AD a zaregistrované k účtu. Registrace zařízení pro systém iOS probíhá při registraci s Microsoft Intune.

## <a name="next-steps"></a>Další kroky

### <a name="contact-us"></a>Kontaktujte nás
Pokud jste v tomto poli, chceme váš názor. Přejděte na [fórum aplikace Microsoft Authenticator](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) vystavte tady svůj dotaz a získat pomoc od komunity nebo nechte komentář na této stránce.


### <a name="related-topics"></a>Související témata
* [O dvoustupňovém ověřování](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) u účtů Microsoft
* [Došlo k potížím s dvoustupnovym overovanim](multi-factor-authentication-end-user-troubleshoot.md) pro svůj pracovní nebo školní účet?
* [Použít Microsoft Authenticator k přihlášení z telefonu](microsoft-authenticator-app-phone-signin-faq.md)
