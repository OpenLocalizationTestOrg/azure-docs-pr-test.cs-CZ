---
title: "Azure Notification Hubs: Nejčastější dotazy (FAQ) | Microsoft Docs"
description: "Nejčastější dotazy na návrh nebo implementaci řešení na centra oznámení"
services: notification-hubs
documentationcenter: mobile
author: ysxu
manager: erikre
keywords: "nabízená oznámení, nabízená oznámení, nabízená oznámení iOS, android nabízená oznámení, nabízené ios, android nabízené"
editor: 
ms.assetid: 7b385713-ef3b-4f01-8b1f-ffe3690bbd40
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2017
ms.author: yuaxu
ms.openlocfilehash: a70efa7fc5954966847d5a173e9b10accf4b737e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="push-notifications-with-azure-notification-hubs-frequently-asked-questions"></a>Nabízená oznámení pomocí Azure Notification Hubs: Nejčastější dotazy
## <a name="general"></a>Obecné
### <a name="what-is-hello-resource-structure-of-notification-hubs"></a>Co je struktura prostředků hello centra oznámení?

Služba Azure Notification Hubs má dvě úrovně prostředků: rozbočovače a obory názvů. Rozbočovač je jeden nabízené prostředek, k němuž mohou být uloženy informace napříč platformami nabízené hello jednu aplikaci. Obor názvů je kolekce centra v jedné oblasti.

Doporučené mapování odpovídá jeden obor názvů s jednu aplikaci. V oboru názvů může mít produkční rozbočovače, který funguje s produkční aplikace, testování rozbočovače, který funguje s testování aplikace a tak dále.

### <a name="what-is-hello-price-model-for-notification-hubs"></a>Co je model hello ceny pro centra oznámení?
Hello nejnovější podrobnosti o cenách najdete na hello [ceny centra oznámení] stránky. Centra oznámení se fakturuje na úrovni oboru názvů hello. (Hello definice oboru názvů, najdete v části "Co je struktura prostředků hello centra oznámení?") Centra oznámení nabízí tři úrovně:

* **Volné**: Tato úroveň je to dobrý výchozí bod pro zkoumání nabízených možností. Nedoporučuje pro produkční aplikace. Získejte 500 zařízení a 1 milion nabízených oznámení zahrnuté na obor názvů za měsíc, s žádná záruka, smlouvu o úrovni (SLA) služby.
* **Základní**: Tato úroveň (nebo standardní úroveň hello) se doporučuje pro menší produkční aplikace. Získejte 200 000 zařízení a 10 milionů nabízených oznámení na obor názvů měsíčně jako základ zahrnut. Růst možnosti kvót, které jsou zahrnuty.
* **Standardní**: Tato úroveň se doporučuje pro střední toolarge produkční aplikace. Získejte 10 milionů zařízení a 10 milionů nabízených oznámení na obor názvů měsíčně jako základ zahrnut. Kvóta zvýšení možnosti a bohaté telemetrie možnosti jsou zahrnuty.

Funkce úrovně Standard:
* **Bohatá telemetrie**: můžete použít centra oznámení za zpráva Telemetrie tootrack žádné push požadavky a zpětné vazby systému oznámení platformy pro ladění.
* **Víceklientská**: pověření systému oznámení platformy můžete pracovat na úrovni oboru názvů. Tato možnost umožňuje tooeasily můžete rozdělit na centra v rámci hello klientům stejný obor názvů.
* **Naplánované nabízené**: můžete naplánovat toobe oznámení odeslaná kdykoli.

### <a name="what-is-hello-notification-hubs-sla"></a>Co je hello SLA centra oznámení?
Správně nakonfigurovaných aplikací pro úrovně Basic a Standard centra oznámení můžete odesílat nabízená oznámení nebo provádět operace správy registrace nejméně 99,9 % času hello. Další informace o smlouvě SLA hello, přejděte toohello toolearn [SLA centra oznámení](https://azure.microsoft.com/support/legal/sla/notification-hubs/) stránky.

> [!NOTE]
> Protože nabízená oznámení záviset na jiných výrobců systémy oznámení platforem (například Apple APNS a Google FCM), neexistuje žádná záruka SLA pro doručení hello těchto zpráv. Po Notification Hubs odesílá hello dávek systémy oznámení tooPlatform (SLA zaručit), je to, že hello zodpovědností hello systémy oznámení platforem toodeliver hello nabízených oznámení (bez SLA zaručit).

### <a name="which-customers-are-using-notification-hubs"></a>Které zákazníci používají Notification Hubs?
Mnoho zákazníků používat Notification Hubs. Jsou zde uvedeny některé významné ty, které jsou:

* Sochi 2014: Stovky skupinám, 3 + miliony zařízení a 150 + milionů oznámení odeslaných za dva týdny. [Případová studie: Sochi]
* Skanska: [Případová studie: Skanska]
* Časy Praha: [Případová studie: časy Praha]
* Mural.ly: [Případová studie: Mural.ly]
* 7Digital: [Případová studie: 7Digital]
* Aplikace Bing: Desítkami milionů zařízení odesílat oznámení 3 miliony za den.

### <a name="how-do-i-upgrade-or-downgrade-my-hub-or-namespace-tooa-different-tier"></a>Jak upgradovat nebo starší verzi Moje rozbočovače nebo obor názvů tooa jiné vrstvě?
Přejděte toohello  **[portál Azure]** > **obory názvů centra oznámení** nebo **Notification Hubs**. Vyberte prostředek hello chcete tooupdate a přejděte příliš**cenová úroveň**. Všimněte si hello následující požadavky:

* Hello aktualizované cenové úrovně se vztahuje příliš*všechny* centra v oboru názvů hello kterými pracujete.
* Pokud zařízení počet překračuje limit hello hello vrstvy, které jste Downgrade k, je třeba zařízení toodelete předtím, než jste starší verzi.


## <a name="design-and-development"></a>Návrh a vývoj
### <a name="which-server-side-platforms-do-you-support"></a>Serverové platformy, na kterých podporujete?
Server SDK jsou k dispozici pro rozhraní .NET, Java, Node.js, PHP a Python. Rozhraní API centra oznámení jsou založené na rozhraní REST, abyste mohli pracovat přímo s rozhraní REST API, pokud používáte jiné platformy nebo nechcete, aby další závislosti. Další informace, přejděte toohello [rozhraní API REST centra oznámení] stránky.

### <a name="which-client-platforms-do-you-support"></a>Které klientské platformy podporujete?
Nabízená oznámení jsou podporovány pro [iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [univerzální pro Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Číny (prostřednictvím Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) a [Android](xamarin-notification-hubs-push-notifications-android-gcm.md)), [aplikace pro Chrome](notification-hubs-chrome-push-notifications-get-started.md), a [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari). Další informace, přejděte toohello [kurzy Notification Hubs Začínáme] stránky.

### <a name="do-you-support-text-message-email-or-web-notifications"></a>Podporujete textová zpráva, e-mailu nebo webové oznámení?
Centra oznámení je primárně navrženou toosend oznámení toomobile aplikace. Neposkytuje e-mailu nebo textu zprávy možnosti. Však lze integrovat platformy třetí strany, které nabízí tyto možnosti s Notification Hubs toosend nativní nabízená oznámení pomocí [Mobile Apps].

Centra oznámení také neposkytuje služby v prohlížeči nabízená oznámení doručení předinstalované hello. Zákazníci můžete implementovat tuto funkci pomocí SignalR nad serverové platformy podporované hello. Pokud chcete toosend oznámení toobrowser aplikací v izolovaného prostoru hello Chrome, najdete v části hello [aplikace pro Chrome kurzu].

### <a name="how-are-mobile-apps-and-azure-notification-hubs-related-and-when-do-i-use-them"></a>Jak jsou mobilní aplikace a související s Azure Notification Hubs a kdy je použít?
Pokud máte stávající mobilní aplikace zpět ukončení a chcete tooadd pouze hello schopností toosend nabízená oznámení, můžete používat Azure Notification Hubs. Pokud chcete tooset do mobilní aplikace back-end vašeho od začátku, zvažte použití hello funkce Mobile Apps služby Azure App Service. Mobilní aplikace automaticky zřídí centra oznámení, takže můžete snadno odesílat nabízená oznámení z back-end hello mobilní aplikace. Ceny pro Mobile Apps zahrnuje hello základní poplatky za centra oznámení. Platíte jen při překročení hello zahrnuté nabízených oznámení. Další informace o náklady, přejděte toohello [App Service – ceny] stránky.

### <a name="how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>Kolik zařízení může podporovat Pokud odesílat nabízená oznámení prostřednictvím centra oznámení?
Odkazovat toohello [ceny centra oznámení] stránku Podrobnosti o hello počet podporovaných zařízení.

Pokud potřebujete podporu pro více než 10 milionů registrovaná zařízení, [kontaktujte nás](https://azure.microsoft.com/overview/contact-us/) přímo a pomůžeme vám škálovat vaše řešení.

### <a name="how-many-push-notifications-can-i-send-out"></a>Kolik nabízená oznámení můžete I poslat?
V závislosti na hello vybraná úroveň Azure Notification Hubs automatické škálování podle hello počet oznámení předávaných mezi hello systému.

> [!NOTE]
> Hello celkové náklady na použití může zvýšit podle hello počet nabízených oznámení, které jsou obsluhovány. Ujistěte se, že jste v úvahu limity vrstvy hello uvedených na hello [ceny centra oznámení] stránky.
> 
> 

Naše zákazníky pomocí Notification Hubs toosend milióny nabízených oznámení denně. Nemáte toodo nic speciální tooscale hello dosáhnout nabízených oznámení, dokud používáte Azure Notification Hubs.

### <a name="how-long-does-it-take-for-sent-push-notifications-tooreach-my-device"></a>Jak dlouho dělá proveďte pro odeslat nabízená oznámení tooreach Moje zařízení?
Ve scénáři použití normální, kde hello příchozí zatížení je konzistentní a i, Azure Notification Hubs může zpracovat alespoň *1 milion nabízených oznámení odešle minutu*. Tato míra se můžou lišit v závislosti na hello počet značek, povaha hello příchozí zasílá hello a dalších faktorů externí.

Během hello odhadovaný čas doručení služba hello vypočítá hello ukazatele na každou platformu a směruje zprávy toohello, služby nabízených oznámení (PNS) v závislosti na hello registrované značky nebo značky výrazů. Je zodpovědností hello hello systém PNS toosend oznámení toohello zařízení.

Hello systém PNS nezaručuje všechny smlouvy SLA pro doručování oznámení. Ale většina nabízená oznámení se doručují tootarget zařízení během několika minut (obvykle do 10 minut) z hello, když se odešlou tooNotification rozbočovače. Několik oznámení může trvat delší dobu.

> [!NOTE]
> Služba Azure Notification Hubs má zásady v místě toodrop žádné nabízená oznámení, které nejsou doručeny toohello systém PNS do 30 minut. Toto zpoždění může nastat z mnoha důvodů, ale většina běžně, protože hello systém PNS je omezování vaší aplikace.
> 
> 

### <a name="is-there-any-latency-guarantee"></a>Je k dispozici žádné záruku latence?
Z důvodu hello povaha nabízených oznámení (doručení podle externí, specifické pro platformu PNS) neexistuje žádná záruka latence. Obvykle hello většina nabízená oznámení se doručují během několika minut.

### <a name="what-do-i-need-tooconsider-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>Co dělat potřebuji tooconsider při návrhu řešení pro obory názvů a notification hubs?
#### <a name="mobile-appenvironment"></a>Mobilní aplikace nebo prostředí
* Použijte jeden centra oznámení na mobilní aplikaci na prostředí.
* Ve scénáři víceklientské každého klienta by měl mít samostatné rozbočovače.
* Sdílené složky nikdy hello stejné centra oznámení pro produkční a testovací prostředí. Tento postup může způsobit problémy při odesílání oznámení. (Apple nabízí izolovaného prostoru a produkční Push koncových bodů, každý s samostatné přihlašovací údaje).
* Ve výchozím nastavení můžete odeslat testovací oznámení tooyour registrované zařízení prostřednictvím hello portál Azure nebo hello Azure integrované komponenty v sadě Visual Studio. Prahová hodnota Hello nastavená too10 zařízení, které jsou náhodně vybírány ze hello registrace fondu.

> [!NOTE]
> Pokud vaše Centrum byl původně nakonfigurovaný pomocí certifikátu Apple izolovaného prostoru a pak se změněnou konfigurací toouse produkční certifikát služby Apple, hello původní tokeny zařízení jsou neplatné. Neplatný tokeny způsobit toofail nabízených oznámení. Oddělené produkčním i testovacím prostředí a použití různých centra pro různá prostředí.
> 
> 

#### <a name="pns-credentials"></a>Systém PNS pověření
Pokud mobilní aplikace není zaregistrována portál pro vývojáře na platformě (například Apple nebo Google), budou odeslány tokeny zabezpečení a identifikátor aplikace. back-end Hello aplikaci poskytuje tyto tokeny toohello platformy PNS tak, aby toodevices lze odesílat nabízená oznámení. Tokeny zabezpečení, může být ve tvaru hello certifikáty (například Apple iOS nebo Windows Phone) nebo klíče zabezpečení (například Google Android nebo Windows). Musí být nakonfigurované v centra oznámení. Konfigurace se obvykle provádí na úrovni hello centra oznámení, ale je možné ji provést na úrovni oboru názvů hello ve víceklientském scénáři.

#### <a name="namespaces"></a>obory názvů
Obory názvů lze použít pro nasazení seskupení. Také lze použít toorepresent všechny centra oznámení pro všechny klienty hello stejné aplikace ve víceklientském scénáři.

#### <a name="geo-distribution"></a>GEO rozdělení.
Geograficky distribuční není vždy důležité ve scénářích nabízená oznámení. Různé PNSes (například služby APN nebo GCM), které doručují nabízená oznámení toodevices nejsou rovnoměrně.

Pokud máte aplikaci, která se používá globálně, můžete vytvořit centra v různých oborech názvů pomocí služby Notification Hubs hello v různých oblastech Azure kolem hello, world.

> [!NOTE]
> Toto uspořádání nedoporučujeme, protože jeho hodnota se zvyšuje vaše náklady na správu, zejména pro registrace. By mělo být provedeno pouze v případě, že existuje explicitní potřeba.
> 
> 

### <a name="should-i-do-registrations-from-hello-app-back-end-or-directly-through-client-devices"></a>Je třeba udělat registrace z back-end hello aplikace nebo přímo prostřednictvím klienta zařízení?
Registrace z back-end hello aplikace jsou užitečné, když máte klienty tooauthenticate před vytvořením hello registrace. Jsou užitečná také když máte značky, které musí být vytvořen nebo upraveném hello aplikace back-end na základě logiky aplikace. Další informace, přejděte toohello [back-end registrace pokyny] a [back-end registrace pokyny 2] stránky.

### <a name="what-is-hello-push-notification-delivery-security-model"></a>Co je model zabezpečení pro doručení hello nabízená oznámení?
Používá služba Azure Notification Hubs [sdílený přístupový podpis](../storage/common/storage-dotnet-shared-access-signature-part-1.md)-model zabezpečení založený na. Tokeny podpis hello sdíleného přístupu můžete použít na úrovni oboru názvů hello kořenového adresáře nebo na úrovni centra podrobné oznámení hello. Sdílený přístupový podpis tokenů může být sada toofollow různých autorizační pravidla, například toosend zpráva oprávnění nebo toolisten oznámení oprávnění. Další informace najdete v tématu hello [model zabezpečení Notification Hubs] dokumentu.

### <a name="how-should-i-handle-sensitive-payload-in-push-notifications"></a>Jak by měly zpracovávat citlivé datové části v nabízená oznámení?
Všechna oznámení se doručují tootarget zařízení podle platformy hello PNS. Když jsou oznámení odesílána tooAzure Notification Hubs, je zpracován a předán toohello příslušných PNS.

Všechna připojení z hello odesílatele toohello Azure Notification Hubs toohello PNS, použijte protokol HTTPS.

> [!NOTE]
> Služba Azure Notification Hubs neprotokoluje žádným způsobem hello datovou část zprávy.
> 
> 

toosend citlivé datových částí, doporučujeme pomocí vzoru zabezpečení Push. Odesílatel Hello přináší ping oznámení se zařízením zpráva identifikátor toohello bez hello citlivé datové části. Datová část hello přijetí hello aplikace na zařízení hello hello aplikace volá zabezpečení rozhraní API přímo toofetch hello podrobnosti zprávy. Průvodce na tom, jak se tooimplement tento vzor, přejděte toohello [kurzu centra oznámení zabezpečené Push] stránky.

## <a name="operations"></a>Operace
### <a name="what-support-is-provided-for-disaster-recovery"></a>Jaké podpora je k dispozici pro zotavení po havárii?
Poskytujeme pokrytí obnovení po havárii metadata na naší straně (hello název centra oznámení, hello připojovací řetězec a další důležité informace). Když se aktivuje na scénář zotavení po havárii, registrační data je hello *pouze segmentovat* infrastruktury hello centra oznámení, které se ztratí. Budete potřebovat tooimplement řešení toorepopulate tato data do nové po obnovení centra:

1. Vytvoření centra oznámení sekundární v jiném datovém centru. Doporučujeme vytvořit od začátku tooshield hello vám v události pro zotavení po havárii, které by mohly ovlivnit možnosti správy. Můžete také vytvořit jeden během hello události obnovení po havárii hello.

2. Naplnění hello centra sekundární oznámení s hello registrace z centra primární oznámení. Nemáte doporučujeme při toomaintain registrace na obou rozbočovače a jejich synchronizace jako mají registrace. Tento postup nefunguje správně kvůli hello vyplývajících tendence tooexpire registrace hello systém PNS straně. Centra oznámení vyčistí je jako obdrží systém PNS názor registrace vypršela platnost, nebo neplatný.  

Máme dvě doporučení pro back-EndY aplikace:

* Použijte back end aplikace, která udržuje danou sadu registrací v jeho koncem. Příkaz bulk insert se pak můžete provádět do centra oznámení sekundární hello.

* Použijte aplikaci back-end, který získá výpis regulární registrací z centra primární oznámení hello jako záložní. Příkaz bulk insert se pak můžete provádět do centra oznámení sekundární hello.

> [!NOTE]
> Funkce exportu/importu registrace je k dispozici ve standardní vrstvě hello je popsaná v hello [registrace exportu/importu] dokumentu.
> 
> 

Pokud nemáte back-end, při spuštění aplikace hello na cílová zařízení, fungují v centru oznámení sekundární hello nové registrace. Nakonec hello centra sekundární oznámení budou mít všechny hello aktivních zařízení registrované.

Bude časové období, kdy zařízení s vyberte aplikace nebude přijímat oznámení.

### <a name="is-there-audit-log-capability"></a>Je k dispozici funkce protokolu auditování?
Všechny operace správy Notification Hubs přejděte toooperation protokoly, které jsou zveřejněné v hello [portál Azure classic].

## <a name="monitoring-and-troubleshooting"></a>Monitorování a řešení potíží
### <a name="what-troubleshooting-capabilities-are-available"></a>Jaké možnosti pro odstraňování potíží jsou k dispozici?
Služba Azure Notification Hubs poskytuje několik funkcí pro řešení potíží, zejména pro nejčastější scénáře hello vynechaných oznámení. Podrobnosti najdete v tématu hello [řešení potíží s Notification Hubs] dokumentu white paper.

### <a name="what-telemetry-features-are-available"></a>Jaké funkce telemetrická data jsou k dispozici?
Azure Notification Hubs umožňuje zobrazení telemetrická data v hello [portál Azure classic]. Podrobnosti o hello metriky, které jsou k dispozici na hello [metriky centra oznámení] stránky.

> [!NOTE]
> Úspěšné oznámení jednoduše znamenat, že nabízená oznámení se doručují toohello externí systém PNS (například APNS pro Apple) nebo GCM pro Google. Je zodpovědností hello hello systém PNS toodeliver hello oznámení tootarget zařízení. Obvykle hello systém PNS nevystavuje doručení metriky toothird strany.  
> 
> 

Poskytujeme také hello schopností tooexport hello telemetrická data prostřednictvím kódu programu (ve standardní vrstvě hello). Podrobnosti najdete v tématu hello [metriky centra oznámení ukázka].

[portál Azure classic]: https://manage.windowsazure.com
[ceny centra oznámení]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Notification Hubs SLA]: http://azure.microsoft.com/support/legal/sla/
[Případová studie: Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[Případová studie: Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[Případová studie: Časy Praha]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[Případová studie: Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[Případová studie: 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[rozhraní API REST centra oznámení]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[kurzy Notification Hubs Začínáme]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[aplikace pro Chrome kurzu]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[back-end registrace pokyny]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[back-end registrace pokyny 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[model zabezpečení Notification Hubs]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[kurzu centra oznámení zabezpečené Push]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[řešení potíží s Notification Hubs]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[metriky centra oznámení]: https://msdn.microsoft.com/library/dn458822.aspx
[metriky centra oznámení ukázka]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[registrace exportu/importu]: https://msdn.microsoft.com/library/dn790624.aspx
[portál Azure]: https://portal.azure.com
[complete samples]: https://github.com/Azure/azure-notificationhubs-samples
[Mobile Apps]: https://azure.microsoft.com/services/app-service/mobile/
[App Service – ceny]: https://azure.microsoft.com/pricing/details/app-service/
