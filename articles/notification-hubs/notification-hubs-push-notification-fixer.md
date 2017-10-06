---
title: "aaaAzure centra oznámení – pokyny pro diagnostiku"
description: "Pokyny o tom, jak toodiagnose běžné problémy s Azure Notification Hubs."
services: notification-hubs
documentationcenter: Mobile
author: ysxu
manager: erikre
editor: 
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: e374278f2bfdfad36ba091e8846059cd184c17ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure Notification Hubs – pokyny pro diagnostiku
## <a name="overview"></a>Přehled
Jeden z hello nejčastější dotazy jsme Azure Notification Hubs zákazníci obrátili názvem se toofigure out proč tu nezobrazí oznámení odeslaných z jejich back-end aplikace hello klientského zařízení – kde a proč byly vyřazeny oznámení a jak toofix to. V tomto článku projdeme hello různých důvodů, proč oznámení může získat vyřadit ani nekončí na zařízeních hello. Podíváme se také prostřednictvím způsoby, ve kterém můžete analyzovat a zjistěte hlavní příčinu hello. 

První řadě je důležité toounderstand jak Azure Notification Hubs doručí se oznámení toohello zařízení.
![][0]

V toku typické odesílání oznámení, je odeslána zpráva hello z hello **back-end aplikace** příliš**Azure oznámení centra (NH)** na které se pak některé zpracování na všechny registrace hello s ohledem na hello účet nakonfigurovat značky & toodetermine výrazy značka "cíle", tj. všechny hello registrace, které je třeba tooreceive hello nabízených oznámení. Mezi některé nebo všechny naše podporované platformy – iOS, Google, Windows, Windows Phone, může mít rozsah tyto registrace Kindle a Baidu pro Android v Číně. Po vytvoření hello cíle jsou NH pak nabízených oznámení se oznámení, rozdělit do několika batch registrací, toohello platforma konkrétní **služby nabízených oznámení (PNS)** – např. pro Apple APNS, GCM pro Google atd. NH ověří hello příslušných PNS založené na hello přihlašovací údaje, které jste nastavili v hello portálu Azure Classic na stránce Konfigurace centra oznámení hello. Hello systém PNS poté předává hello oznámení toohello příslušných **klientských zařízení**. Toto je platforma hello doporučené způsob toodeliver nabízená oznámení a Všimněte si, že hello konečné fáze doručení dochází mezi hello platformy systému PNS a hello zařízení. Proto máme čtyři hlavní součásti - *klienta*, *back-end aplikace*, *Azure Notification Hubs (NH)* a *služby nabízených oznámení (PNS)* a některý z těchto může způsobit oznámení získávání vyřadit. Další podrobnosti o této architektury je k dispozici na [přehledu této služby].

Selhání toodeliver, může dojít oznámení, že testovací/pracovní fáze, což může značit problém konfigurace, nebo může dojít v produkčním prostředí, kde všechny nebo některé z hello oznámení během počáteční hello získávání může vyřadit označující některé hlubší aplikace nebo vzor problém pro zasílání zpráv. V části hello níže se podíváme na různé scénáře vynechaných oznámení od běžné druh vzácnějších toohello, některé z nich může být zřejmé a jiná není mnoho. 

## <a name="azure-notifications-hub-mis-configuration"></a>Chybná konfigurace centra oznámení Azure
Centra oznámení Azure potřebuje tooauthenticate sám v kontextu hello nástroje pro vývojáře hello aplikace toobe možné toosuccessfully odesílání oznámení toohello příslušných PNS. To je umožněno díky hello vývojáře vytváření vývojářského účtu s hello příslušné platformy (Google, Apple Windows atd.) a pak registraci své aplikace, kde získat přihlašovací údaje, které vyžadují toobe nakonfigurované hello portálu v části oznámení Oddíl konfigurace rozbočovače. Pokud nejsou žádná oznámení přijímání prostřednictvím prvním krokem by měl být tooensure že hello správné přihlašovací údaje jsou nakonfigurované v hello Centrum oznámení je odpovídající pomocí aplikace hello vytvořil v rámci jejich konkrétní vývojářský účet platformy. Zjistíte, naše [získávání kurzů] užitečné toogo přes tento proces způsobem krok za krokem. Zde jsou některé běžné chybné konfigurace:

1. **Obecné**
   
    (a) ujistěte se, že vaším jménem centra oznámení (bez překlepům) je hello stejné:
   
   * Kde registraci od klienta hello 
   * Kde jsou odesílání oznámení z back-end hello,  
   * Kde jste nakonfigurovali systém PNS hello přihlašovací údaje a 
   * Jehož pověření SAS jste nakonfigurovali na hello klienta a hello back-end. 
     
     b) ujistěte se, že používáte hello správné SAS konfigurace řetězce na klientovi hello a back-end aplikace hello. Jako existuje pravidlo, musí používat hello **DefaultListenSharedAccessSignature** hello klienta a **DefaultFullSharedAccessSignature** na hello aplikace back-end (který poskytuje oprávnění toobe možnost toosend oznámení toohello NH)
2. **Konfigurace Apple Push Notification Service (APNS)**
   
    Musíte udržovat dvě různé centra – jednu pro produkční a druhý pro testování účel. To znamená, odesílání hello certifikátu, že budete toouse v samostatné hub tooa izolovaného prostoru prostředí a hello certifikátu, že budete toouse v produkční tooa samostatné rozbočovače. Nepokoušejte tooupload různé typy certifikátů toohello stejném centru jako ho může způsobit selhání oznámení dolů hello řádku. Pokud se přistihnete na pozici, kde jste omylem odeslali různé typy certifikátů toohello stejné rozbočovače, je doporučeno toodelete hello rozbočovače a spustit novou. Pokud z jakéhokoliv důvodu není možné toodelete hello rozbočovače pak v hello velmi alespoň, je nutné odstranit všechny existující registrace hello z centra hello. 
3. **Konfigurace zasílání zpráv cloudu Google (GCM)** 
   
    (a) ujistěte se, že povolíte "Google Cloud Messaging pro Android" v části projektu cloudu. 
   
    ![][2]
   
    b) zkontrolujte vytvořte klíč"Server" při získání přihlašovacích údajů hello které NH použije tooauthenticate GCM. 
   
    ![][3]
   
    c) ujistěte se, že jste nakonfigurovali "ID projektu" hello klienta, který je zcela číselné entita, která můžete získat z řídicího panelu hello:
   
    ![][1]

## <a name="application-issues"></a>Problémy s aplikací
1) **Značky / značka výrazy**

Pokud používáte značky nebo značky výrazy toosegment cílovou skupinu, vždycky je možné, že při odesílání oznámení hello, že žádný cíl byl nalezen založené na výrazech hello značky nebo značky, který určujete v odesílání volání. Je nejvhodnější tooreview vaše tooensure registrace, která jsou značky které shodu při odesílání oznámení a pak ověřte hello potvrzení o doručení pouze z klientů hello s tyto registrace. Například Pokud všechny vaše registrace s NH měla pracovat vyslovení značka "Politika" a odesílání oznámení pomocí značky "Sports", nebude odeslána tooany zařízení. Komplexní případu může patřit značky výrazy, kde můžete registrovat jenom s "Značky A" nebo "Značky B", ale při odesílání oznámení, kterou cílíte "Značky A a a značky B". V hello samoobslužné diagnostikovat níže uvedené části Tipy, existují způsoby, ve kterém můžete zkontrolovat vaše registrace společně se značkami hello, kterou mají. 

2) **Problémy se šablonou**

Pokud používáte šablony a ujistěte se, že jsou následující hello pokynů popsaných na [šablony pokyny]. 

3) **Neplatný registrace**

Za předpokladu, že hello centra oznámení byla správně nakonfigurována a žádný z výrazů značky nebo značky používaly správně výsledkem hello najít platný cílů toowhich hello oznámení potřebovat toobe odeslána, NH vyvolá několik zpracování dávky paralelně - každé dávky odesílání zpráv tooa sadu registrací. 

> [!NOTE]
> Vzhledem k tomu, že jsme hello paralelní zpracování, jsme nezaručují hello pořadí, ve které hello bude doručit oznámení. 
> 
> 

Nyní centra oznámení Azure je optimalizovaná pro model doručení zprávy "jednou na většinu". To znamená, jsme pokus deaktivace duplikace tak, aby žádná oznámení se doručují více než jednou tooa zařízení. tooensure jsme projděte hello registrace a ujistěte se, že pouze jednu zprávu je odeslána na identifikátor zařízení před ve skutečnosti odesílání hello zpráva toohello systém PNS. Protože každé dávky je odeslán toohello systém PNS, která pak dále přijímá a ověřování hello registrace, je možné, že hello systém PNS zjistí chybu s jedním nebo více hello registrací v dávce, vrátí k chybě tooAzure NH a ukončí zpracování, a tím vyřadit úplně dávky. To platí hlavně službou APNS, který používá protokol TCP datového proudu. I když jsme jsou optimalizované pro na většinu po doručení, v tomto případě jsme odstranit hello chybující registraci z našich databázi a poté opakujte doručení pro hello zbytek hello zařízení v této dávce.

Můžete získat informace o chybě pro pokus o selhání doručení hello proti registrace pomocí hello API REST centra oznámení Azure: [za zpráva Telemetrie: získání Telemetrických zpráv oznámení](https://msdn.microsoft.com/library/azure/mt608135.aspx) a [zpětnou vazbu systém PNS](https://msdn.microsoft.com/library/azure/mt705560.aspx). V tématu hello [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) například kódu.

## <a name="pns-issues"></a>Systém PNS problémy
Jakmile byla přijata zpráva oznámení hello podle hello příslušných PNS, pak je jeho odpovědnost toodeliver hello oznámení toohello zařízení. Azure Notification Hubs je mimo hello obrázek sem a nemá žádnou kontrolu, když nebo oznámení hello přechází toobe doručit toohello zařízení. Vzhledem k tomu, že jsou poměrně robustní hello platformy oznámení služby, oznámení zpravidla tooreach hello zařízení během několika sekund z hello systém PNS. Pokud hello systém PNS ale je omezování pak Azure Notification Hubs použijte exponenciální regrese strategie a zda hello systém PNS zůstává nedostupný pro 30 min pak je k dispozici zásady v umístěte tooexpire a trvale vyřadit tyto zprávy. 

Pokud systém PNS pokusí toodeliver oznámení, ale hello zařízení je offline, je oznámení hello ukládaná hello systém PNS po omezenou dobu a doručit toohello zařízení, až bude k dispozici. Je uložena pouze jedno poslední oznámení pro konkrétní aplikace. Pokud více oznámení se odesílají, když je zařízení hello offline, každé nové oznámení způsobí, že předchozí oznámení hello toobe zahozeny. Toto chování, aby pouze nejnovější oznámení hello je odkazované tooas slučování oznámení v APNS a sbalení v GCM (který se používá ztenčeného klíč). Pokud zařízení hello zůstane offline po dlouhou dobu, žádné oznámení, které jste právě uložili pro něj se zahodí. Source – [APNS pokyny] & [GCM pokyny]

S Azure Notification Hubs – můžete předat slučování klíč prostřednictvím záhlaví HTTP pomocí hello obecného `SendNotification` rozhraní API (např. v případě sady .NET SDK – `SendNotificationAsync`) což také trvá hlavičky protokolu HTTP, které jsou předány jako je toohello příslušných PNS. 

## <a name="self-diagnose-tips"></a>Samoobslužné diagnostikovat tipy
Zde vyzkoušíme hello různé toodiagnose cesty a kořenové způsobit problémy s Centrum oznámení:

### <a name="verify-credentials"></a>Ověření přihlašovacích údajů
1. **Systém PNS portál pro vývojáře**
   
    Ověření je hello příslušných systém PNS developer portal (GCM, APNS WNS atd) pomocí našich [získávání kurzů].
2. **Portál Azure Classic**
   
    Přejděte toohello konfigurovat karta tooreview a shodovat hello pověřeními získanými z portálu pro vývojáře systému PNS hello. 
   
    ![][4]

### <a name="verify-registrations"></a>Ověření registrace
1. **Visual Studio**
   
    Pokud používáte Visual Studio pro vývoj můžete připojit tooMicrosoft Azure a zobrazovat a spravovat bunch služeb Azure včetně centra oznámení z "Průzkumníka serveru". To je užitečné hlavně pro vaše prostředí pro vývoj/testování. 
   
    ![][9]
   
    Můžete zobrazit a spravovat všechny registrace hello v centru, které jsou klasifikovány vhodně pro platformu, nativní nebo šablony registrace, všechny značky, systém PNS identifikátor, registrace id a hello datum vypršení platnosti. Můžete taky upravit registraci v chodu hello – což je užitečné, například pokud chcete tooedit všechny značky. 
   
    ![][8]
   
   > [!NOTE]
   > Visual Studio funkce tooedit registrace lze používat pouze během vývoje/testování s omezený počet registrací. Pokud nastane toofix potřeby vaší registrace hromadně, zvažte použití hello exportu/importu registrace funkcí popsaných tady - [exportu/importu registrace](https://msdn.microsoft.com/library/dn790624.aspx)
   > 
   > 
2. **Průzkumník služby Service Bus**
   
    Mnoho zákazníků použit sběrnice explorer popsané v tomto - [Explorer sběrnice] pro zobrazování a správu jejich centra oznámení. Je k dispozici z code.microsoft.com - opensourcový projekt [sběrnice Průzkumník kódu]

### <a name="verify-message-notifications"></a>Ověřte oznamování pomocí zpráv
1. **Portál Azure Classic**
   
    Můžete přejít toohello "Debug" karta toosend testovací oznámení tooyour klienty bez nutnosti jakéhokoli back-endu služby nahoru a spuštěna. 
   
    ![][7]
2. **Visual Studio**
   
    Můžete také odeslat zkušební oznámení z hello comforts sady Visual Studio:
   
    ![][10]
   
    Můžete si přečíst další na hello Visual Studio oznámení centra Azure explorer funkce zde- 
   
   * [Přehled Průzkumníka serveru VS]
   * [Příspěvek blogu Průzkumníka serveru VS - 1]
   * [Příspěvek blogu Průzkumníka serveru VS - 2]

### <a name="debug-failed-notifications-review-notification-outcome"></a>Ladění chybných oznámení / zkontrolujte výsledek oznámení
**Vlastnost EnableTestSend**

Při odesílání oznámení prostřednictvím centra oznámení, nejdřív ho jenom získá zařazen do fronty pro zpracování toofigure se všechny jeho cíle toodo NH a potom nakonec NH ji pošle toohello systém PNS. To znamená, že při použití rozhraní REST API nebo některou z hello klienta SDK, hello úspěšné návrat vaše odeslání volání pouze znamená, že hello zpráva má byl úspěšně zařazen do fronty pomocí centra oznámení. Nedává ho lépe pochopit, co se stalo při NH nakonec tu toosend hello zpráva tooPNS. Pokud není oznámení přicházejících u hello klientského zařízení, je možnost, že když NH pokoušeli toodeliver hello zpráva tooPNS, došlo k chybě například velikost datové části hello překročil maximální hello povolenou hello systém PNS nebo jsou nakonfigurované v NH pověření hello Neplatný atd tooget na aspekty hello systém PNS chyby, jsme zavedli vlastnost s názvem [EnableTestSend funkce]. Tato vlastnost je automaticky povolen při odeslání testovacích zpráv ze hello portálu nebo klienta Visual Studia a proto vám umožní toosee podrobné ladicí informace. Můžete použít prostřednictvím rozhraní API trvá hello příklad hello .NET SDK tam, kde je k dispozici nyní a bude přidané tooall klientské sady SDK nakonec. toouse, o volání REST hello, jednoduše připojte querystring parametr s názvem "test" na konci hello volání odesílání například 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Příklad (.NET SDK)*

Předpokládejme, že používáte .NET SDK toosend nativní informační zpráva:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);

`result.State`bude jednoduše stavu `Enqueued` na konci hello hello provádění bez všechny aspekty co se stalo tooyour push. Nyní můžete pomocí hello `EnableTestSend` vlastnost typu boolean při inicializaci hello `NotificationHubClient` a může získat podrobné informace o stavu informace o chybách systému PNS hello došlo při odesílání oznámení hello. Hello odesílání zde volání bude trvat tooreturn další čas, protože pouze vrací poté, co NH předložil hello oznámení tooPNS toodetermine hello výsledek. 

    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Ukázkový výstup*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    hello Token obtained from hello Token Provider is wrong

Tato zpráva znamená buď neplatné přihlašovací údaje jsou nakonfigurované v centru oznámení hello nebo problém s registrací hello ve hello rozbočovače a hello doporučené kurzu by být toodelete tato registrace a nechat hello klienta znovu ji vytvořte před odesláním hello zpráva. 

> [!NOTE]
> Upozorňujeme, že hello použití této vlastnosti je výraznou omezené, a proto musíte použít tento v prostředí pro vývoj/testování s omezenou sadu registrací. Pouze pošleme oznámení ladění too10 zařízení. Máme také limit zpracování ladění zasílá toobe 10 za minutu. 
> 
> 

### <a name="review-telemetry"></a>Zkontrolujte telemetrie
1. **Použijte portál Azure Classic**
   
    portál Hello umožňuje tooget rychlý přehled o všechny aktivity hello na vaše Centrum oznámení. 
   
    (a) z karty "řídicího panelu" hello můžete zobrazit souhrnné zobrazení hello registrace, oznámení, jakož i chyby na každou platformu. 
   
    ![][5]
   
    b) můžete také přidat mnoho dalších platformy určité metriky z hello "Sledování" karta tootake hlubší podívejte se na konkrétní chyby systému PNS vrácené při NH pokusí toosend hello oznámení toohello systém PNS zvlášť. 
   
    ![][6]
   
    c) byste měli začít s Kontrola hello **příchozí zprávy**, **registrace Operations**, **úspěšné oznámení** a potom přejděte tooper platformy karta tooreview hello Systém PNS konkrétní chyby. 
   
    d) Pokud máte zobrazí hello oznámení, že Centrum špatně nakonfigurovaný s nastavením ověřování hello pak můžete systém PNS chyby ověřování. Toto je dobrá indikace toho toocheck hello systém PNS přihlašovací údaje. 

2) **Programový přístup**

Zde – podrobnosti 

* [Telemetrie programový přístup]
* [Telemetrie přístup přes rozhraní API ukázka] 

> [!NOTE]
> Několik telemetrii související funkce, jako jsou **exportu/importu registrace**, **Telemetrie přístup přes rozhraní API** jsou dostupné v úrovni Standard jenom atd. Pokud se pokusíte toouse tyto funkce, pokud jste v volné nebo úroveň Basic pak můžete získají výjimka zpráva toothis vliv při použití hello SDK a protokolu HTTP 403 (zakázáno) je použití přímo z hello rozhraní REST API. Ujistěte se, že mají přesunout nahoru tooStandard vrstvy prostřednictvím portálu Azure Classic.  
> 
> 

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png

<!-- LINKS -->
[přehledu této služby]: notification-hubs-push-notification-overview.md
[získávání kurzů]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[šablony pokyny]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNS pokyny]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM pokyny]: http://developer.android.com/google/gcm/adv.html
[Export/Import Registrations]: http://msdn.microsoft.com/library/dn790624.aspx
[Explorer sběrnice]: http://msdn.microsoft.com/library/dn530751.aspx
[sběrnice Průzkumník kódu]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[Přehled Průzkumníka serveru VS]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[Příspěvek blogu Průzkumníka serveru VS - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[Příspěvek blogu Průzkumníka serveru VS - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend funkce]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Telemetrie programový přístup]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetrie přístup přes rozhraní API ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

