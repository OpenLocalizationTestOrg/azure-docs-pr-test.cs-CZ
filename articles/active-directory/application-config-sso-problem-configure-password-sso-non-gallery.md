---
title: "Konfigurace hesla jednotné přihlašování pro aplikace bez Galerie aaaProblem | Microsoft Docs"
description: "Pochopení hello běžné problémy uživatelé setkávají při konfiguraci hesla jednotné přihlašování pro vlastní aplikace bez galerie, které nejsou uvedené v hello Azure AD Application Gallery"
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
ms.openlocfilehash: 3aee0a4c525bb3da338da2da0882ec572cf0e5e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-configuring-password-single-sign-on-for-a-non-gallery-application"></a>Problém konfigurace hesla jednotné přihlašování pro aplikace bez Galerie

Tento článek vám pomůže toounderstand hello běžné problémy uživatelé setkávají při konfiguraci **heslo jednotné přihlašování** s aplikací bez galerie.

## <a name="how-toocapture-sign-in-fields-for-an-application"></a>Jak přihlášení toocapture polí pro aplikaci

Zachycení pole přihlášení je podporována pouze pro podporujících formát HTML přihlašovací stránky a je **není podporováno pro nestandardní přihlašovací stránky**, jako jsou ty, které používáte Flash nebo jiné technologie nepodporujícím HTML.

Existují dva způsoby, můžete zaznamenávat pole přihlašování pro vaše vlastní aplikace:

-   Zachycení pole Automatické přihlášení

-   Zachycení pole Ruční přihlášení

**Automatické přihlášení pole zachycení** funguje dobře u většiny podporujících formát HTML přihlašovací stránky, pokud používají **dobře známé identifikátory DIV hello uživatelské jméno a heslo zadejte** pole. Hello tak, jak to funguje je oškrabávání hello HTML na stránce toofind hello DIV ID, které splňují určitá kritéria a potom uložení aby metadata pro tuto aplikaci, tak opětovným přehráním hesla tooit později.

**Ruční pole přihlášení zachycení** lze použít v případě hello, který hello aplikace **dodavatele neoznačí** hello vstupních polí použitých pro přihlášení. Zachycení ruční přihlášení pole lze také v případě hello při hello **dodavatele vykreslí více polí** který nemůže být automaticky nalezeny. Azure AD můžete ukládat data pro jako počet polí na hello přihlašovací stránky, tak dlouho, dokud Řekněte nám kde tato pole jsou na stránce hello.

Obecně platí **Pokud pole Automatické přihlášení zachycení nefunguje, doporučujeme vždy snažíme hello ruční.**

### <a name="how-tooautomatically-capture-sign-in-fields-for-an-application"></a>Jak zachytit tooautomatically pole přihlášení pro aplikaci

tooconfigure **založené na heslech jednotné přihlašování** pro aplikace pomocí **pole Automatické přihlášení zachycení**, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

  * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  Zadejte hello **přihlašovací adresa URL**. Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k. **Ujistěte se, hello přihlášení pole jsou viditelné na adrese URL hello zadáte**.

10. Klikněte na tlačítko hello **Uložit** tlačítko.

11. Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a umožňují toouse Azure AD toosecurely přenášet hesla toothat aplikace pomocí rozšíření prohlížeče panely hello přístup.

## <a name="how-toomanually-capture-sign-in-fields-for-an-application"></a>Jak zachytit toomanually pole přihlášení pro aplikaci

zachycení přihlašovací toomanually pole, je nejdříve nutné rozšíření přístup k panelu prohlížeče hello nainstalovat a **nebyla spuštěna v režimu se službou inPrivate, incognito nebo soukromé.** rozšíření prohlížeče tooinstall hello, postupujte podle kroků hello v hello [jak tooinstall hello rozšíření přístup k panelu prohlížeče](#i-cannot-manually-detect-sign-in-fields-for-my-application) části.

tooconfigure **založené na heslech jednotné přihlašování** pro aplikace pomocí **ruční pole přihlášení zachycení**, postupujte podle následujících kroků hello:

1.  Otevřete hello [ **portálu Azure** ](https://portal.azure.com/) a přihlaste se jako **globálního správce** nebo **ko-správce.**

2.  Otevřete hello **rozšíření Azure Active Directory** kliknutím **další služby** dole hello v navigační nabídce vlevo hlavní hello.

3.  Zadejte **"Azure Active Directory**" hello filtru vyhledávacího pole a vyberte hello **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace, které** z hello Azure Active Directory levém navigační nabídky.

5.  Klikněte na tlačítko **všechny aplikace** tooview seznam všech aplikací.

   * Pokud chcete zobrazit vytvořit tady aplikace hello nevidíte, pomocí hello **filtru** řízení hello horní části hello **seznam všech aplikací** a sadu hello **zobrazit** možnost příliš **Všechny aplikace.**

6.  Vyberte hello aplikaci tooconfigure jednotné přihlašování.

7.  Jakmile aplikace hello načte, klikněte na možnost hello **jednotného přihlašování** z aplikace hello levém navigační nabídky.

8.  Vyberte hello režimu **založené na heslech přihlášení.**

9.  Zadejte hello **přihlašovací adresa URL**. Toto je hello URL, kde uživatelé zadat toosign své uživatelské jméno a heslo v k. **Ujistěte se, hello přihlášení pole jsou viditelné na adrese URL hello zadáte**.

10. Klikněte na tlačítko hello **Uložit** tlačítko.

11. Jakmile to uděláte, jsme budete automaticky scrape tuto adresu URL pro uživatelské jméno a heslo vstupní pole a umožňují toouse Azure AD toosecurely přenášet hesla toothat aplikace pomocí rozšíření prohlížeče panely hello přístup. V případě hello, který tato možnost selže, můžete **změnit hello přihlášení režimu toouse ruční pole přihlášení zachycení** pokračováním toostep 12.

12. Klikněte na tlačítko **konfigurace &lt;appname&gt; nastavení hesla jednotného přihlašování**.

13. Vyberte hello **ručně zjistit přihlášení pole** možnosti konfigurace.

14. Klikněte na tlačítko **OK**.

15. Klikněte na **Uložit**.

16. Postupujte podle hello na obrazovce pokyny toouse hello přístupového panelu.

## <a name="i-see-a-we-couldnt-find-any-sign-in-fields-at-that-url-error"></a>Zobrazuje chybu "Jsme nenašli žádná pole přihlášení na této adrese URL."

Tato chyba zobrazí při automatické zjišťování pole přihlášení selže. Tento problém, zkuste detekce pole Ruční přihlášení pomocí následujících hello kroky hello tooresolve [jak toomanually zaznamenat pole přihlášení pro aplikaci](#how-to-manually-capture-sign-in-fields-for-an-application) části.

## <a name="i-see-an-unable-toosave-single-sign-on-configuration-error"></a>Zobrazuje chybu "nelze toosave jednotné přihlašování konfigurace"

V některých výjimečných případech může selhat aktualizace hello jeden přihlašování konfigurace. tooresolve, vyzkoušejte ukládání hello jeden přihlašování konfiguraci znovu.

Pokud tento postup se opakuje toofail konzistentně, otevřete případu podpory a zadejte hello informace získané v hello [jak toosee hello podrobnosti o portálu oznámení](#i-cannot-manually-detect-sign-in-fields-for-my-application) a [jak pomoci tooget zasláním oznámení podrobnosti tooa pracovník odborné pomoci](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) oddíly.

## <a name="i-cannot-manually-detect-sign-in-fields-for-my-application"></a>I nelze detekovat ručně přihlašovací v polích pro Moje aplikace

Mezi hello chování, mohou být zobrazeny při nepracuje ruční detekce patří:

-   Proces ručního zachycení Hello zobrazovaly toowork, ale pole hello zachytit nebyla správná

-   pole pravé Hello nemáte získat zvýrazněná při provádění proces zachycení hello

-   Proces zachycení Hello trvá mi toohello na přihlašovací stránku aplikace podle očekávání, ale nic nestane

-   Ruční zachycení zobrazí toowork, ale jednotného přihlašování nemá dojít v případě, že moje uživatelé přejdou toohello aplikace hello přístupového panelu.

Zkontrolujte následující hello, pokud dojde k některé z těchto problémů:

-   Zajistěte, abyste měli nejnovější verzi rozšíření prohlížeče panely přístup hello hello **nainstalován** a **povoleno** podle následujících kroků hello v hello [jak tooinstall hello prohlížeče Panel přístupu rozšíření](#how-to-install-the-access-panel-browser-extension) části.

-   Ujistěte se, že proces zachycení hello neprovádíte při prohlížeč v **privátní, se službou inPrivate nebo incognito režimu**. v těchto režimech nepodporuje rozšíření panely Hello přístup.

-   Ujistěte se, že uživatelé nejsou při toosign v aplikaci toohello z hello přístupového panelu při v **privátní, se službou inPrivate nebo incognito režimu**. v těchto režimech nepodporuje rozšíření panely Hello přístup.

-   Proces ručního zachycení hello zkusit znovu, zajistíte hello red značek přes hello správná pole.

-   Pokud proces ruční zachycení hello zdá, že toohang nebo neprovádí hello přihlašovací stránku hello proces ruční zachycení nic (případě 3 výše), opakujte akci. Ale tentokrát po dokončení procesu hello, stiskněte klávesu hello **F12** tlačítko tooopen vývojářské konzole prohlížeče. Jednou existuje, otevřete hello **konzoly** a typ **window.location= "&lt;zadejte přihlašovací hello v adrese url, které jste zadali při konfiguraci aplikace hello&gt;"** a potom stiskněte klávesu  **Zadejte**. Tato síla na stránce přesměrování, který ukončit proces zachycení hello a ukládání hello pole, která byla zachycena.

Pokud žádný z těchto postupů fungovat pro vás, pomůžeme vám. Otevře případu podpory s podrobnostmi hello jste zkusili, jakož i hello informace získané v hello [jak toosee hello podrobnosti o portálu oznámení](#i-cannot-manually-detect-sign-in-fields-for-my-application) a [jak pomoci tooget zasláním oznámení podrobnosti tooa podpory pracovník](#how-to-get-help-by-sending-notification-details-to-a-support-engineer) částech (pokud existuje).

## <a name="how-tooinstall-hello-access-panel-browser-extension"></a>Jak tooinstall hello rozšíření přístup k panelu prohlížeče

tooinstall hello rozšíření prohlížeče panely přístup, postupujte podle kroků hello níže:

1.  Otevřete hello [přístupový Panel](https://myapps.microsoft.com) v jednom z hello podporované prohlížeče a přihlaste se jako **uživatele** ve službě Azure AD.

2.  Klikněte na tlačítko **SSO heslo aplikace** v hello přístupového panelu.

3.  V hello výzva s dotazem tooinstall hello softwaru, vyberte **instalovat nyní**.

4.  Založené na prohlížeči, je možné směrovanou toohello odkaz ke stažení. **Přidat** hello rozšíření tooyour prohlížeče.

5.  Pokud prohlížeč zobrazí dotaz, vyberte tooeither **povolit** nebo **povolit** hello rozšíření.

6.  Po instalaci **restartujte** relaci prohlížeče.

7.  Přihlaste se do hello přístupového panelu a zobrazit, pokud můžete **spusťte** SSO hesla aplikací.

Může také stáhnout hello rozšíření pro Chrome a Firefox z hello přímé odkazy níže:

-   [Rozšíření pro Chrome přístup panely](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Rozšíření panelu Firefox přístup](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-toosee-hello-details-of-a-portal-notification"></a>Jak toosee hello podrobnosti o portálu oznámení

Podle následujících kroků hello, můžete zjistit podrobnosti o hello všechna portálu oznámení:

1.  Klikněte na tlačítko hello **oznámení** ikonu (hello zvonku) v pravé horní hello části hello portálu Azure

2.  Vyberte všechna oznámení v **chyba** stavu (ty s další toothem červený (!)).

  >! Všimněte si] je nelze kliknout oznámení v **úspěšné** nebo **probíhá** stavu.
  >
  >

3.  Tento otevřený hello **podrobnosti oznámení** okno.

4.  Tyto informace použijte sami toounderstand další podrobnosti o problému hello.

5.  Pokud stále potřebujete pomoc, můžete také sdílet tyto informace se na podporu pracovníka nebo hello skupiny tooget Nápověda k produktu s váš problém.

6.  Klikněte na tlačítko hello **kopie** **ikonu** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produktu.

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a>Jak pomoci tooget odesláním pracovníka podpory tooa podrobnosti oznámení

To je velmi důležité, že můžete sdílet **všechny níže uvedené podrobnosti o hello** s pracovníka podpory, pokud potřebujete pomoc, tak, aby se vám může pomoct rychle. Můžete to provést pomocí snadno **uděláte snímek,** nebo kliknutím hello **ikona chyby kopie**, najít toohello napravo od hello **Chyba kopírování** textové pole.

## <a name="notification-details-explained"></a>Vysvětlení podrobnosti oznámení

Hello níže vysvětluje více co každý hello oznámení položek znamená a poskytuje příklady každého z nich.

### <a name="essential-notification-items"></a>Základní oznámení položky

-   **Název** – hello popisný název hello oznámení

    -   Příklad – **nastavení proxy aplikace**

-   **Popis** – hello popis co došlo k chybě v důsledku operace hello

    -   Příklad – **zadaná interní adresa url je již používán jinou aplikací**

-   **Id oznámení** – hello jedinečné id oznámení hello

    -   Příklad – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Id žádosti klienta** – id konkrétního požadavku hello provedené prohlížeč

    -   Příklad – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Čas UTC razítko** – hello časové razítko, během které hello oznámení došlo k chybě, v UTC

    -   Příklad – **2017-03-23T19:50:43.7583681Z**

-   **Interní Id transakce** – hello interní ID můžeme použít toolook hello Chyba v našem systému

    -   Příklad – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **Hlavní název uživatele** – hello uživatele, který provedl operaci hello

    -   Příklad –**tperkins@f128.info**

-   **Id klienta** – hello jedinečné ID klienta hello, který hello uživatele, který provedl operaci hello byl členem

    -   Příklad – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Objekt uživatele Id** – hello jedinečné ID hello uživatele, který provedl operaci hello

    -   Příklad – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Podrobné oznámení položky

-   **Zobrazovaný název** – **(nesmí být prázdné)** podrobnější zobrazovaný název pro chybu hello

    -   Příklad * – **nastavení proxy aplikace**

-   **Stav** – hello konkrétní stav hello oznámení

    -   Příklad * – **se nezdařilo**

-   **Id objektu** – **(nesmí být prázdné)** hello ID objektu, podle které hello byla provedena operace

    -   Příklad – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Podrobnosti o** – hello podrobný popis co došlo k chybě v důsledku operace hello

    -   Příklad – **interní adresa url, http://bing.com/' je neplatná, protože je již používán**

-   **Chyba při kopírování** – klikněte na tlačítko hello **ikona kopírování** toohello napravo od hello **Chyba kopírování** textbox toocopy všechny hello tooshare podrobnosti oznámení s skupiny pracovník podpory nebo produkt

    -   Příklad –```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Další kroky
[Zadejte tooyour přihlašování aplikací pomocí Proxy aplikace](active-directory-application-proxy-sso-using-kcd.md)

