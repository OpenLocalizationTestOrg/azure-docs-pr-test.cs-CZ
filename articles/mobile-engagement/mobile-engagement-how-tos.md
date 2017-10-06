---
title: "aaaAzure Mobile Engagement uživatelské rozhraní – přístup jak pro"
description: "Přehled uživatelského rozhraní pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 30af87e6-c816-4cce-8609-6cbd3e83de14
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4b6dafd09d894214d4c386f5c6f157a77671606f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-using-and-managing-pushes-tooreach-out-tooyour-end-users"></a>Jak tooget spuštění používáním a správou nabízených oznámení tooreach out tooyour koncovým uživatelům
Jakmile hello SDK je plně integrována do vaší aplikace, abyste mohli začít pomocí hello hello Reach části hello uživatelského rozhraní tooPush oznámení toohello uživatele vaší aplikace.  

## <a name="do-your-first-push-notification-campaign"></a>Proveďte své první kampaně nabízených oznámení
* Potvrďte, že vaše kampaně Reach je integrována do vaší aplikace pomocí hello SDK. 
* Vyberte svou aplikaci

![First1][1]

* Přejděte toohello část "Reach" a klikněte na tlačítko "nové oznámení"

![First2][2]

* Vytvořte novou kampaň a pojmenujte ji
  
![First3][3]

* Vyberte, jak by měla doručen hello oznámení, jako v aplikaci jenom

![First4][4]

* Vytvoření uvítací zprávu, že chcete toopush

![First5][5]

* Název může zapisovat na hello oznámení (volitelné).
* Zápis předávaný obsah zprávy.
* Můžete nahrát bitovou kopii. Upozorňujeme, že velikost hello hello souboru nesmí překročit 32 768 bajtů.
* Máte také tooselect možnost hello další možnosti, ale pro hello zaměřit tohoto kurzu, zjistíme, který později.
* Vyberte typ obsahu hello pouze jako oznámení

![First6][6]

* Vytvořte kampaň nabízených a se zobrazí v seznamu kampaně.

![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testování kampaň nabízených oznámení
![test1][8]

* Zaregistrujte zařízení.
* Klikněte na zaškrtávací políčko hello hello zařízení chcete toopush.
* Klikněte na hello "Test" tlačítko toosend hello nabízené toohello zařízení.

![test2][9]

* Aktivovat kampaň hello

![Test3][10]

* Teď, když jste vytvořili kampaň stačí tooactivate pro hello oznámení toobe nabídnutých tooyour uživatele.

## <a name="send-personalized-pushes"></a>Odeslat přizpůsobené nabízených oznámení
* Tento příklad vytvoří push, kde je zadán kód vlastní slevu do hello nabízených oznámení.

![Personalize1][11]

Přizpůsobení funguje tak, že nahradíte značku podle z značku informace o aplikaci tak, budete mít jistotu, že má uživatel hello hello správné app-info definované nejprve toomake. V tento příklad hello cílové uživatele bude mít značku informace o aplikaci s názvem rebate_code definované.
Jak vidíte výše hello nabízená oznámení obsah zahrnuje hello značky ${rebate_code}, které označují, že se jedná o toobe nahrazuje hello skutečný obsah značky hello informace o aplikaci.

> [!WARNING]
> Pokud značka informace o aplikaci hello není definován pro hello uživatele, uživatel hello neobdrží nabízené hello.

* výsledek

![Personalize2][12]

### <a name="you-can-further-personalize-hello-text-your-notification"></a>Můžete upravit hello text oznámení
![Personalize3][13]

* Včetně hello název hello oznámení
* A hello obsah zprávy hello.
* Zvolte typ hello oznámení (textového zobrazení nebo zobrazení webové stránky)

![Personalize4][14]

### <a name="hello-body-of-an-announcement-may-also-be-personalized-with"></a>Hello textu oznámení může také přizpůsobit, pokud:
* Adresa URL akce Hello, budete chtít toocustomize hello úvodní stránka
* Název Hello
* Hello těla zprávy hello.

## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Rozlišení vašeho Push Notification (uvnitř nebo mimo aplikaci)
* Vyberte typ hello oznámení push, vyberte svou aplikaci, přejděte v části "Reach" toohello, vyberte nebo vytvoříte kampaně nabízených a přejděte toohello části "Oznámení".
* Klikněte na hello "doručení režim" Chcete.
* Klikněte na hello k "Omezit aktivity" zaškrtávací políčko, pokud chcete oznámení hello dochází na konkrétní aktivitami (obrazovkami).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Pouze mimo aplikaci" režimu doručení
![Differentiate2][16]

"Pouze mimo aplikaci" způsob dodání poskytuje nabízené oznámení při zavření aplikace hello. Toto je hello standardní nabízených oznámení.
Když vyberete "pouze mimo aplikaci", musí již zadanými hello certifikáty z hello platforma, která vaše aplikace je vychází (APNS nebo GCM).

### <a name="see-also"></a>Viz také
* [Certifikáty Apple Push Notification Service –](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), [Google Cloud Messaging – certifikát](http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"v aplikaci pouze" způsob dodání.
![Differentiate3][17]

Režim doručení "V aplikaci pouze" poskytuje nabízené oznámení hello aplikace běží.
Pro toto oznámení není nutné toogo prostřednictvím hello APNS a GCM systému.
Můžete v aplikaci doručení systému tooreach hello koncovým uživatelům.
Můžete plně přizpůsobit hello oznámení a rozhodnout, jaké aktivity (obrazovky) se zobrazí oznámení hello.

### <a name="anytime-delivery-mode"></a>"Kdykoliv" doručení režimu
Můžete zvolit způsob dodání "Kdykoliv", máte jistotu, že tooreach, zda text hello mohl koncový uživatel aplikace běží, nebo ne.
Když vyberete "Kdykoliv", musí již zadanými hello certifikáty z hello platformy, které vytváří vaše aplikace (APNS nebo GCM). 

## <a name="schedule-a-push-campaign"></a>Plán kampaně nabízených
### <a name="plan-toostart-a-campaign"></a>Plánování tooStart kampaň
![Shedule1][18]

Je hello 21. března a máte toomake oznámení a plánované pro hello 22nd března půlnoci. Nemáte toostay před hello rozhraní toodo push! Můžete naplánovat předem minutu přesný hello, které se budou odesílat oznámení.

* Zrušte zaškrtnutí hello "Žádný" zaškrtávací políčko a vyberte čas spuštění 
* Zvolte hello datum a čas hello chcete toostart hello nabízené kampaně.

### <a name="plan-tooend-a-campaign"></a>Plánování tooend kampaň
![Shedule2][19]

Chcete, aby vaše kampaň toostop na hello 25th března ve 3. hodině, ale vy víte, že nebudete existuje toodo ho.
Nemáte toostay před hello rozhraní toopush! Můžete naplánovat předem minutu přesný text hello, kterou vaše kampaň se zastaví.

* Klikněte na hello "Žádný" zaškrtávací políčko nebo vyberte čas ukončení
* Zvolte hello datum a čas hello chcete toofinish hello nabízené kampaně.

### <a name="end-a-campaign-manually"></a>Ručně ukončete kampaň
![Shedule3][20]

Ve výchozím nastavení hello "Žádný" jsou vybrané zaškrtávací políčka.
To znamená, které hello kampaň se spustí, jakmile ji aktivujete v hello dosáhnout části a dosáhnou konec při zastaví na hello části.

> [!NOTE]
> Kampaně vytvořen bez koncové datum hello nabízené místně uložených na zařízení hello a zobrazit ji hello při příštím otevření aplikace hello i v případě, že ručně hello kampaň skončí.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Vylepšení nabízená oznámení pomocí textového zobrazení
### <a name="what-is-a-text-view"></a>Co je textového zobrazení?
![TextView1][21]

Zobrazení textu je automaticky otevírané okno s textového obsahu. Toto automaticky otevírané okno se zobrazí po hello uživatel klikne na nabízené oznámení hello.
Zobrazení textu umožňuje toopresent více obsahu tooyour koncového uživatele. Toto je také možnost toopresent hello volání tooaction například přechod tooa stránku aplikace, přesměrování tooa úložiště, otevření webové stránky, odesílání e-mailu, spouštění zjištěnou vyhledávání atd...

### <a name="example-text-view"></a>Příklad: Zobrazení textu
* Vytvořte kampaň nabízených oznámení v části "Připojit" hello a zadejte název kampaně

![TextView2][22]

* Zápis uvítací zprávu, která se zobrazí na hello oznámení.
* Vyberte typ obsahu oznámení "text" hello

![TextView3][23]

> [!NOTE]
> Když stisknete textového zobrazení, je vždy se dodává s oznámení nejdřív. 

* Zadejte hello text (po výběru obsah oznámení text hello, hello dílčí části se zobrazí, umožní vám toodefine hello text, který má toobe zobrazit.)

![TextView4][24]

* Zápis hello název, který se zobrazí v horní části hello hello zprávy.
* Zápis hello hlavní obsah zobrazení textu hello.
* Zápis hello obsah, který se zobrazí na hello tlačítko akce (tlačítko akce umožňuje toomake aplikace hello určité akci například otevřete stránku aplikace hello přesměrování tooan App store nebo jakýkoli jiný typu zdroje, které můžete zadat).
* Zápis hello obsah, který se zobrazí na tlačítko Konec hello (kliknutím na tlačítko Konec hello zobrazení textu hello zmizí.)
* Vytvořte kampaň nabízených oznámení a zobrazí se v seznamu kampaň hello.

![TextView5][25]

* Aktivujte uživatelům nabízená oznámení kampaň toosend hello text zobrazení tooyour.

![TextView6][26]

* výsledek

![TextView7][27]

* Hello uživatel obdrží hello oznámení a klepněte na něj.
* zobrazení textu Hello se zobrazí automaticky otevírané okno toointeract uživatele pro povolení hello s ním.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Vylepšení nabízených oznámení s webové zobrazení
### <a name="what-is-a-web-view"></a>Co je webové zobrazení?
![WebView1][28]

Webové zobrazení je automaticky otevírané okno s obsahem serveru. Toto automaticky otevírané okno se zobrazí, když hello uživatel klikne na nabízené oznámení hello.
Webové zobrazení umožňuje toohave větší interakce s koncovým uživatelem hello.
Toto je také možnost toopresent hello volání tooaction například přesměrování tooApp úložiště, otevření webové stránky, odesílání e-mailu, spouštění zjištěnou vyhledávání atd...

### <a name="example-web-view"></a>Příklad: Webové zobrazení
* Vytvořte kampaň nabízených v části "Připojit" hello a zadejte název kampaně.

![WebView2][29]

* Zápis uvítací zprávu, která se zobrazí na hello oznámení.
* Vyberte typ obsahu oznámení hello jako "web"

![WebView3][30]

### <a name="about-announcement-types"></a>O typech oznámení:
* Pouze oznámení: je jednoduchý standardní oznámení. Znamená, že pokud uživatel klikne na něm, bez dalšího zobrazení se zobrazí, ale pouze hello akce přidružené tooit dojde.
* Text oznámení: je oznámení, že zapojí hello uživatele toohave podívejte se na zobrazení textu.
* Sdělení webovém: je oznámení, že zapojí hello uživatele toohave podívejte se na webové zobrazení.
  Vyberte hello "Sdělení webovém" obsah.

> [!NOTE]
> Po stisknutí webové zobrazení, je vždy se dodává s oznámení nejdřív.

* Definování hello webového obsahu (po výběru hello webového obsahu oznámení, část hello se zobrazí, umožní vám toodefine hello webové zobrazení obsah, který chcete zobrazit toobe.)

![WebView4][31]

* Zápis hello název, který se zobrazí v horní části hello hello zprávy (volitelné).
* Zápis váš HTML kód.
* Klikněte na zdroj hello úpravy režimu tlačítko tooswitch edition a v tématu jak to vypadá.
* Zápis hello obsah, který se zobrazí na hello tlačítko akce (tlačítko akce umožňuje toomake aplikace hello určité akci například otevřete stránku aplikace hello přesměrování tooa Store nebo jakýkoli jiný typu zdroje, které můžete zadat).
* Zápis hello obsah, který se zobrazí na tlačítko Konec hello (kliknutím na tlačítko Konec hello hello webové zobrazení zmizí).
* výsledek

![WebView5][32]

* Hello uživatel přijímat oznámení hello a klepněte na ni.
* zobrazení textu Hello se zobrazí automaticky otevírané okno toointeract uživatele pro povolení hello s ním.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

