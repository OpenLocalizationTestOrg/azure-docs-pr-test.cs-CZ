---
title: "aaaAzure Mobile Engagement uživatelské rozhraní - dosáhnout obsahu"
description: "Zjistěte, jak kampaně toomanage hello jedinečný obsah hello různé typy nabízených oznámení v Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: add64f06-43c9-475c-8722-51cd00bb844b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: de389eb4368d986ef00135036c26e26a2464663e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-hello-unique-content-of-hello-different-types-of-push-notification-campaigns"></a>Jak toomanage hello jedinečný obsah hello různé typy kampaní nabízených oznámení
Můžete použít oddíl obsahu hello nové reach kampaň toomodify hello obsahu oznámení, hlasování, datová oznámení a dlaždice (pouze Windows Phone). nastavení obsah Hello kampaní nabízených je typ konkrétní toohello kampaně. 

### <a name="content-types"></a>Typy obsahu:
* Oznámení
* Hlasování
* Datová oznámení
* Dlaždice (pouze Windows Phone)

## <a name="content-of-announcements"></a>Obsah oznámení
 ![Reach Content1][30] 

### <a name="choose-hello-type-of-your-announcement"></a>Zvolte typ sdělení hello:
* Pouze oznámení: je jednoduchý standardní oznámení. Znamená, že pokud uživatel klikne na něm, bez dalšího zobrazení se zobrazí, ale pouze hello akce přidružené tooit dojde.
* Text oznámení: je oznámení, že zapojí hello uživatele toohave podívejte se na zobrazení textu.
* Sdělení webovém: je oznámení, že zapojí hello uživatele toohave podívejte se na webové zobrazení.

### <a name="see-also"></a>Viz také
* [Dosažení – jak Tos – oznámení][Link 3] 

### <a name="about-web-view-announcements"></a>O sděleních ve webovém zobrazení:
Výskyty vzoru hello "{deviceid}" hello HTML kód nebo kód jazyka JavaScript, které tady zadáte, se automaticky nahradí identifikátorem hello hello zařízení zobrazení hello oznámení. Jedná se snadný způsob tooretrieve Azure identifikátorů zařízení Mobile Engagement v externí webovou službu hostovanou na vašem interním systému.
Pokud chcete, aby toocreate úplnou obrazovce webové zobrazení (bez hello výchozích tlačítek akce a ukončení poskytujeme) můžete použít následující funkce z Javascriptového kódu vašeho sdělení ve webovém zobrazení hello: 

* provést akci sdělení hello: ReachContent.actionContent()
* Ukončit ze sdělení hello: ReachContent.exitContent()

### <a name="choose-your-action"></a>Vyberte akci:
### <a name="about-action-urls"></a>O adresy URL akce:
Jako adresa URL akce se dá použít libovolná adresa URL, která jde interpretovat operačním systémem zacíleného zařízení.
Jakoukoli vyhrazenou adresu URL, která vaše aplikace může podporu (například toomake uživatelé přejít tooa konkrétní obrazovku) lze také použít jako adresu URL akce.
Každý výskyt vzoru hello {deviceid} se automaticky nahradí identifikátorem hello hello zařízení provedení akce hello. To může být identifikátory zařízení Azure Mobile Engagement použité tooeasily načtení přes externí webovou službu hostovanou na vašem interním systému.

* **Android a iOS akce**
  * Otevřít webovou stránku
  * http://\[domény webové lokality\] 
  * Příklad: http://www.azure.com
  * Odeslat e-mail
  * mailto:\[e-mailu-příjemce\]? subjektu =\[subjektu\]& textu =\[zpráv\] 
  * Example:mailto:foo@example.com? subjektu = pozdrav % 20from % 20Azure % 20Mobile % 20Engagement! & textu = 20stuff dobrý %!
  * Odeslat SMS
  * SMS:\[telefonní číslo\] 
  * Příklad: sms:2125551212
  * Vytočit telefonní číslo
  * Telefon:\[telefonní číslo\] 
  * Příklad: tel:2125551212
* **Android pouze akce**
  * Stáhnout aplikaci v obchodě Play hello
  * Market://details?ID=\[balíček aplikace\] 
  * Příklad: market://details?id=com.microsoft.office.word
  * Spustit hledání se zjištěním polohy
  * GEO:0, 0? q =\[vyhledávací dotaz.\] 
  * Příklad: geo:0, 0? q = starbucks, Paříž
* **iOS pouze akce**
  * Stáhnout aplikaci v hello obchodu s aplikacemi
  * http://iTunes.Apple.com/ [Země] /app/ [název aplikace] /id [id aplikace]? mt = 8 
  * Příklad: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
  * Akce Windows
  * Otevřít webovou stránku
  * http://\[domény webové lokality\] 
  * Příklad: http://www.azure.com
  * Odeslat e-mail
  * mailto:\[e-mailu-příjemce\]? subjektu =\[subjektu\]& textu =\[zpráv\] 
  * Example:mailto:foo@example.com? subjektu = pozdrav % 20from % 20Azure % 20Mobile % 20Engagement! & textu = 20stuff dobrý %!
  * Odeslat SMS (vyžaduje se aplikace Skype pro Store)
  * SMS:\[telefonní číslo\] 
  * Příklad: sms:2125551212
  * Vytočit telefonní číslo (vyžaduje se aplikace Skype pro Store)
  * Telefon:\[telefonní číslo\] 
  * Příklad: tel:2125551212
  * Stáhnout aplikaci v obchodě Play hello
  * MS-windows-úložiště: PDP? PFN =\[ID balíčku aplikace\] 
  * Příklad: ms-windows-úložiště: PDP? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1
  * Zahájit hledání v Mapách Bing
  * bingmaps:? q =\[vyhledávací dotaz.\] 
  * Příklad: bingmaps:? q = starbucks, Paříž
  * Použít vlastní schéma
  * \[vlastní schéma\]://\[parametry vlastního schématu\] 
  * Příklad: myCustomProtocol://myCustomParams
  * Použít data balíčku (vyžaduje se aplikace Store pro rozšíření pro čtení)
  * \[Složka\]\[data\].\[ rozšíření\] 
  * Example:myfolderdata.txt

### <a name="build-a-tracking-url"></a>Sestavte adresu URL pro sledování:
* Najdete v části "Nastavení" hello hello <UI Documentation> pro pokyny k vytváření adresu URL pro sledování, které vám umožní uživatelům toodownload jeden z jiné aplikace.

### <a name="define-hello-texts-of-your-announcement"></a>Definujte texty sdělení hello
Zadejte název hello, obsah a tlačítko texty sdělení. Můžete vybrat cílovou skupinu na základě názorů hello reach o tom, jak uživatelé odpověděl toothis kampaň budoucí kampaně. Cílení na publikum může být založen na hello zpětnou vazbu o tom, jestli se tato kampaň právě nabídnutých, zodpovězených, reakcí nebo ukončením.

### <a name="see-also"></a>Viz také
* [Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]

## <a name="content-of-polls"></a>Obsah hlasování
![Reach Content2][31] 

Vyplňte hello název, popis a tlačítko texty sdělení. Pak přidejte otázky a možnosti pro dotazy tooyour hello odpovědi.
Můžete vybrat cílovou skupinu na základě názorů hello reach o tom, jak uživatelé odpověděl toothis kampaň budoucí kampaně. Cílení na publikum může být založené na tom, jestli se tato kampaň právě nabídnutých, zodpovězených, reakcí nebo ukončením. Cílení na publikum může být taky založené na dotazování odpovědí zpětnou vazbu, kde jsou hello otázku a odpověď volba použít jako kritéria.

### <a name="see-also"></a>Viz také
* [Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]

## <a name="content-of-data-pushes"></a>Obsah datová oznámení
![Reach Content3][32] 

### <a name="choose-hello-type-of-your-data"></a>Vyberte typ hello vašich dat:
* Text
* Binární data
* Data formátu Base64.

### <a name="define-hello-content-of-your-data"></a>Definujte obsah dat. hello
* Pokud jste vybrali toopush textová data, zkopírujte a vložte hello text do pole "obsah" hello.
* Pokud jste vybrali toopush binární nebo base64 data, používat tooupload tlačítko "nahrát soubor" hello souboru.
* Můžete vybrat cílovou skupinu na základě názorů hello reach o tom, jak uživatelé odpověděl toothis kampaň budoucí kampaně. Cílení na publikum může být založené na tom, jestli se tato kampaň právě nabídnutých, zodpovězených, reakcí nebo ukončením.

### <a name="see-also"></a>Viz také
* [Nové nabízené kritérium dokumentace - Reach - uživatelského rozhraní][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Obsah dlaždice (pouze Windows Phone)
![Reach Content4][33]

### <a name="define-hello-content-of-your-tile"></a>Definujte obsah dlaždice hello
datová část dlaždice Hello je toobe textu hello zobrazí na dlaždici hello své aplikace na zařízení Windows Phone.
Push dlaždice je verze služby Microsoft nabízených oznámení (MPNS) hello nativního nabízení pro Windows Phone. Typ nabízeného dlaždice Hello je hello pouze nabízené typ, který nemá odpověď a proto nemůže být hello cílové skupiny kampaní budoucí založený na hello výsledků dlaždice nabízené kampaně. 

### <a name="see-also"></a>Viz také
* [Rozhraní API dokumentace - Reach API - nativního nabízení][Link 4]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

