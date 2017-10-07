---
title: "aaaAzure příručky řešení potíží s Mobile Engagement"
description: "Průvodce řešením potíží pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 31134a29-a513-4e5e-b626-f6cf6fe04769
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: dd69bfd7019907c3e1da8df590db3b5f61606173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement---troubleshooting-guide"></a>Azure Mobile Engagement – průvodce odstraňováním potíží
## <a name="introduction"></a>Úvod
Hello následující Průvodci odstraňováním potíží vám pomůže pochopit, že základní příčiny některé běžně zaznamenané problémy a bude umožňují tootroubleshoot sami. 

## <a name="general"></a>Obecné
Obecně platí vždy musíte hello následující:

1. Ujistěte se, že jste prošli všechny hello kroky potřebné k integraci, jak je popsáno v našem [kurzy Začínáme](mobile-engagement-windows-store-dotnet-get-started.md)
2. Používáte nejnovější verzi hello hello platformy sady SDK. 
3. Testování v samotného zařízení a emulátoru, protože jsou pouze konkrétní tooemulator některé problémy. 
4. Nejsou stiskne žádné omezení nebo omezení z Mobile Engagement, které jsou popsané [sem](../azure-subscription-service-limits.md)
5. Pokud nemůžete tooconnect toohello Mobile Engagement služby back-end nebo zobrazuje dat. nebudou načteny nepřetržitě pak zajistěte, aby žádné probíhající služby incidenty kontrolou [sem](https://azure.microsoft.com/status/)

## <a name="monitor-issues"></a>Problémy, monitorování.
### <a name="i-am-not-seeing-my-device-showing-up-on-hello-monitor-tab"></a>Nejsou zobrazeny Moje zařízení zobrazovat na kartě monitorování hello
Monitorování kartě se zobrazují platformy Mobile Engagement připojené tooyour hello zařízení v reálném čase. Pokud ladíte na emulátoru a zařízení, měli byste vidět zde alespoň jednu relaci. Pokud aplikace hello byly distribuovány, uvidíte hello aktivních relací měřidla podle hello zařízení, které jsou připojené toohello platformy v reálném čase. 

Pokud nevidíte zařízení na kartě monitorování hello je pravděpodobně problém integraci sady SDK. Některé běžné tootake tootroubleshoot kroky jsou následující:

1. Zkontrolujte, zda používáte správný připojovací řetězec hello hello mobilní aplikace a je z části klíče hello SDK a není hello část klíče rozhraní API. připojovací řetězec Hello připojí instanci aplikace Mobile Engagement hello, ve kterém se bude zařízení zobrazí na kartě monitorování hello toohello mobilní aplikace. 
2. Pro platformu Windows - Pokud stránka přepíše hello `OnNavigatedTo` metoda, ujistěte se, že toocall `base.OnNavigatedTo(e)`.
3. Pokud jsou integraci Mobile Engagementu do stávající mobilní aplikace, pak můžete také zajistit, že nejsou chybí všechny kroky podle hello advanced kroků integrace [sem](mobile-engagement-windows-store-integrate-engagement.md)
4. Ujistěte se, odesílání alespoň jednu obrazovku nebo aktivity přepsáním hello stránka s EngagementActivity v závislosti na platformě hello pracujete, jak je popsáno v hello [kurzy Začínáme s](mobile-engagement-windows-store-dotnet-get-started.md).

### <a name="i-am-seeing-hello-monitor-tab-showing-a-session-even-when-i-have-disconnected-or-closed-my-app-emulator"></a>Karta sledovat hello zobrazující relaci se zobrazují i když I mít odpojen nebo uzavřeny mé aplikace nebo emulátor.
Pokud v tomto okamžiku jste pouze jednu platformu připojené toohello hello a používáte aplikace hello tooopen emulátoru pak je to pravděpodobně z důvodu tooemulator adaptivní. Obecně platí musíte tooensure, že jste se vrátit toohello domovskou obrazovku na hello emulátor pro toodisconnect relace aplikace hello úspěšně. Kromě toho na platformě Windows při ladění pomocí sady Visual Studio, může být nutné v sadě Visual Studio, přejít toohello tooensure **události životního cyklu** řádku nabídek a klikněte na **pozastavit** tooreally zavřít Hello relace. V tématu [Windows kurzu](mobile-engagement-windows-store-dotnet-get-started.md) podrobnosti. 

## <a name="analytics-issues"></a>Problémy, analýzy.
### <a name="i-am-not-seeing-any-data-refreshed-data-on-analytics-tab"></a>I nejsou zobrazeny žádné data / aktualizovat data na kartě Analytics
Analytická data jsou přepočítána v pravidelných intervalech a může to trvat až 24 hodin pro tuto aktualizaci. Tato data nejsou v reálném čase a zobrazí se, že se že aktualizují v rámci této 24 hodin časové období.
Zkontrolujte prosím, ale o buď přepsání alespoň jednu stránku s jsou odesílání alespoň jednu obrazovku nebo aktivity toohello platformy back-end `EngagementActivity` nebo volání `SendActivity` explcitly. 

### <a name="i-am-seeing-incorrectly-captured-datetime-for-a-device-on-hello-analytics-tab"></a>Nesprávně zaznamenané datum a čas pro zařízení se zobrazují na kartě Analytics hello
Hello časové období pro analýzu vychází hello data z hello uživatelských nastavení zařízení. Proto ujistěte se, že hello zařízení má hello správně nastaveno datum. 

## <a name="segment-issues"></a>Problémy 'segmentu.
### <a name="i-created-a-segment-and-it-is-showing-up-as-greyed-out-or-not-showing-any-data"></a>Po vytvoření segment a je zobrazovat jako šedá nebo nezobrazují se všechna data
Vytvoření segment není momentálně hello v reálném čase. Se počítá na hello, že stejný čas, jako je agregován hello analytická data a může to trvat až 24 hodin. Zkontrolujte znovu později, ale mezitím také se ujistěte, že vaše mobilní aplikace skutečně odesílání hello dat na základě hello, z nichž jsou tvořící hello segmenty. Například Pokud událost vyslovení "foo" se neodesílají všechna mobilní zařízení, pak nebude žádná data segmentu pro segment vytvořen s položkou EventName = foo jako kritérium hello. Také byste měli zkontrolovat vaše tooensure integraci sady SDK mobilní aplikace odesílá hello data správně. 

## <a name="reach-or-push-notifications-issues"></a>Problémy 'dosáhnout, nebo nabízená oznámení
### <a name="my-push-messages-are-not-being-delivered"></a>Moje nabízená oznámení nejsou doručovány.
1. Pokus o odeslání oznámení tooa testovací zařízení první tooensure, zda jsou všechny součásti hello - mobilní aplikace, služba SDK a hello správně připojený a mít toodeliver nabízená oznámení. 
2. Vždy odesílat nejprve prostřednictvím kampaň, která není naplánován hello nejjednodušší "se na aplikace oznámení" a ani má všechny kritéria cílové skupiny, který je zadán. Toto je znovu tooprove, zda připojení oznámení pracuje správně. 
3. Pokud máte problémy s doručováním oznámení v aplikaci, pak je taky dobrou první krok tootry nejprve odesílání upozornění se na aplikace. 
4. Ujistěte se, že hello 'nativní oznámení, je správně nakonfigurováno pro mobilní aplikace. V závislosti na platformě hello je buď bude zahrnovat klíče (Android, Windows) nebo certifikáty (iOS). V tématu [uživatelské rozhraní – nastavení](mobile-engagement-user-interface-settings.md)
5. Mimo aplikaci, kterou oznámení může být také blokována hello uživatele prostřednictvím hello mobilní operační systém Zajistěte proto tento není hello případ. 
6. Ujistěte se, že nejsou nastavení hello *ignorovat cílovou skupinu, nabízení se odešle toousers prostřednictvím hello rozhraní API* možnost v hello **kampaň** části Reach kampaně, protože tím bude zajištěno, že nabízená oznámení může odeslat pouze prostřednictvím rozhraní API. 
7. Ujistěte se, že testujete kampaň nabízených obou zařízení, připojení přes Wi-Fi a phone operátor sítě tooeliminate hello síťové připojení jako zdroj možných problémů s.
8. Ujistěte se, že hello systému data a času na zařízení nebo emulátor je správný, protože všechny synchronizované zařízení bude také narušovat hello službu nabízených oznámení na možnost toodeliver oznámení. 

Další platformy konkrétní řešení potíží s podle následujících pokynů:

1. **iOS** 
   
   * Zajistěte, aby hello certifikáty platné a neprošlé pro nabízená oznámení iOS. 
   * Ujistěte se, že správně nakonfigurujete *produkční* certifikátu v aplikaci Mobile Engagement. 
   * Ujistěte se, že testujete na *skutečné, fyzické zařízení.* simulátoru iOS Hello nemůže zpracovat nabízená oznámení.
   * Ujistěte se, že hello identifikátor svazku je správně nakonfigurován hello mobilní aplikace. Zobrazí pokyny k hello [sem](https://developer.apple.com/library/prerelease/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)
   * Při testování, pomocí ve svém profilu pro zřizování mobilních "Ad Hoc" distribuce. Pokud vaše aplikace je zkompilovat pomocí "Ladění" nebudete moct tooreceive oznámení
2. **Android**
   
   * Ujistěte se, zda jste zadali správné číslo projektu hello v souboru AndroidManifest.xml mobilní aplikace, který následuje znak \n. 
     
           <meta-data android:name="engagement:gcm:sender" android:value="************\n" />
   * Ujistěte se, nejsou chybí nebo nemá nakonfigurovaný všechna oprávnění v souboru Android Manifest hello 
   * Zajistěte, aby číslo projektu hello přidáváte tooyour klientskou aplikaci z hello stejný účet, kde jste získali hello klíč serveru GCM. Jakákoli Neshoda mezi hello dva zabrání ven vaší nabízených oznámení. 
   * Pokud se vám systému oznámení, ale ne v aplikaci zkontrolujte hello [určení ikony pro oznámení oddíl](mobile-engagement-android-get-started.md) jako pravděpodobně nejsou správné ikony hello určení v souboru Android Manifest hello. 
   * Pokud jsou odesílání oznámení BigPicture, ujistěte se, že pokud máte servery externí bitové kopie, pak potřebují mít toosupport toobe HTTP "GET" a "HEAD".
3. **Windows**
   
   * Ujistěte se, že jste přidružili aplikace hello platný aplikace pro Windows Store. V sadě Visual Studio – budete mít tooright klikněte hello projekt a vyberte možnost "Přidružení aplikace pomocí úložiště" a vyberte hello aplikace, které jste vytvořili v hello Windows Store. Tato aplikace pro Windows Store musí být hello stejné jeden ze které jste získali tooconfigure přihlašovací údaje nativního nabízení hello portálu Mobile Engagement hello.
   * Pokud se zobrazují na více systémů aplikaci nabízená oznámení, ale oznámení není v aplikaci s `EngagementOverlay` integrace pak zajistěte kořenového prvku mřížky na stránce. EngagementOverlay používá hello první "" elementů v mřížce, které najde v xaml souboru tooadd dva webové zobrazení na stránku. Pokud chcete toolocate, kde bude nastavena webové zobrazení, můžete definovat mřížky, jako je to s názvem "EngagementGrid", ale máte tooensure není dostatečná výška a šířka pro hello dva další webové zobrazení, které se zobrazí oznámení hello a hello následující oznámení jako oznámení v aplikaci:
     
           <Grid x:Name="EngagementGrid"></Grid>

### <a name="i-created-a-push-notificationannouncement-campaign-and-even-after-it-sent-me-hello-notification-it-is-showing-as-active-what-does-it-mean"></a>Po vytvoření nabízená oznámení nebo oznámení/kampaně a i po jeho odeslání mi oznámení hello se zobrazuje jako "Aktivní". Co to znamená?
Hello **kampaň** jste vytvořili v Mobile Engagementu je volána, protože je dlouhotrvající význam oznámení nabízené při připojování nového zařízení platformy mobile engagementu tooyour, budou automaticky odeslány hello oznámení Zde je nakonfigurovat tak dlouho, dokud splňují kritéria hello, které se nastavují v kampaň hello. Toto není jednomu nastavení snímek jednoho oznámení. Budete mít toomanually klikněte na hello **Dokončit** tlačítko tooterminate hello kampaň tak, aby ji další neodešle oznámení. 

### <a name="i-created-a-push-campaign-and-i-am-receiving-notifications-successfully-however-whenever-i-open-up-hello-app-i-get-hello-same-notification-even-when-i-had-actioned-it-before"></a>Po vytvoření kampaně nabízených a zobrazuje oznámení úspěšně ale, že při každém otevření hello aplikaci se mi hello stejné oznámení i v případě, že došlo k reakcemi ho před?
To je pravděpodobně toohappen během testování a pokud používáte emulátorů nebo některé testovací framework jako TestFlight. Co se děje, zde je v každé aplikace spustí instanci, je získání nového ID zařízení a odesláním tooour back-end, který je příčinou tootreat platformy Mobile Engagement hello jej jako nové zařízení a odesílání oznámení hello. 

## <a name="getting-support"></a>Získání podpory
Pokud se problém hello nelze tooresolve sami pak můžete:

1. Vyhledejte problém v hello existující vláken ve fóru StackOverflow a [fórum MSDN](https://social.msdn.microsoft.com/Forums/windows/en-US/home?forum=azuremobileengagement) a pokud není pak položte otázku existuje. 
2. Pokud zjistíte funkce chybí pak přidejte nebo hlasování pro hello požadavek na našem [fórum UserVoice](https://feedback.azure.com/forums/285737-mobile-engagement/)
3. Pokud máte otevřené podporu společnosti Microsoft na základě incidentu podpory tím, že poskytuje hello následující podrobnosti: 
   * ID předplatného Azure
   * Platforma (například iOS, Android atd.)
   * ID aplikace
   * ID kampaně (pro nabízená oznámení problémů)
   * ID zařízení
   * Mobile Engagement SDK verze (například v2.1.0 sady SDK pro Android)
   * Podrobnosti o chybě s přesnou chybovou zprávu a scénář

