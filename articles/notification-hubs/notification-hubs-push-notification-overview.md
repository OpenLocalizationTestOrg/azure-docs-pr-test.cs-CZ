---
title: "aaaAzure centra oznámení"
description: "Zjistěte, jak tooadd nabízená oznámení možnosti pomocí Azure Notification Hubs."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: 
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 1/17/2017
ms.author: yuaxu
ms.openlocfilehash: 78ce34b6b094b560c8002ab9652f7ba4563c5c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs"></a>Azure Notification Hubs
## <a name="overview"></a>Přehled
Azure Notification Hubs poskytuje modul snadné použití, více platformami, upraveným push. Pomocí volání API jedné platformě můžete snadno odesílat mobilní platformu tooany cílové a přizpůsobené nabízená oznámení z jakékoli back-end cloudu nebo místně.

Centra oznámení dobře funguje pro scénáře enterprise a příjemce. Zde je několik příkladů, které zákazníci používají Notification Hubs pro:

* Odešlete nejnovější zprávy oznámení toomillions s nízkou latencí.
* Odesílání kupónů toointerested segmenty uživatelů.
* Odešlete oznámení související události toousers nebo skupiny pro média nebo sportu nebo finanční nebo herní aplikace.
* Push propagační obsah tooapps tooengage a trhu toocustomers.
* Upozorní uživatele na firemní události, jako nové zprávy a pracovní položky.
* Odešlete kódy pro službu Multi-Factor authentication.

## <a name="what-are-push-notifications"></a>Co jsou nabízená oznámení?
Nabízená oznámení je formulář aplikace uživatele komunikace, kde jsou uživatelé z mobilních aplikací informováni o některé požadované informace, obvykle v místní nabídce nebo v dialogovém okně. Uživatelé obecně můžete zvolit tooview nebo zrušit uvítací zprávu, a výběr dřívějším hello se otevře hello mobilní aplikaci, která měla oznamovat hello oznámení.

Nabízená oznámení je důležité pro uživatelských aplikací v zvýšení aktivity a využití a pro podnikové aplikace při komunikaci aktuální obchodní údaje. Ho je hello nejlepší aplikace uživatele komunikace, protože je energetickou efektivitu pro mobilní zařízení, flexibilní hello oznámení odesílatelům a k dispozici při odpovídající aplikace nejsou aktivní.

Další informace o nabízená oznámení pro několik oblíbených platformy:
* [iOS](https://developer.apple.com/notifications/)
* [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
* [Windows](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>Jak nabízená oznámení fungují
Nabízená oznámení se doručují prostřednictvím infrastruktur specifických pro platformy označují jako *systémy oznámení platforem* (PNSes). Nabízejí funkce nabízené barebone toodelivery zpráva tooa zařízení s zadané zpracování a nemá žádné společné rozhraní. toosend oznámení tooall zákazníkům napříč hello iOS, Android a Windows verze aplikace hello vývojáře musí pracovat s APNS (Apple Push Notification Service), FCM (Firebase Cloud Messaging) a WNS (Windows Notification Service), při dávkování odešle Hello.

Na vysoké úrovni zde je, jak funguje nabízené:

1. Hello klientskou aplikaci rozhodne, že se požaduje tooreceive nabízených oznámení proto kontakty hello odpovídající systém PNS tooretrieve svého popisovače nabízené jedinečný a dočasný. Hello typ popisovače závisí na hello systému (například WNS má identifikátory URI, zatímco APNS obsahuje tokenů).
2. Hello klientská aplikace si tento popisovač uloží v back-end aplikace hello nebo zprostředkovatele.
3. toosend nabízených oznámení, hello back-end aplikace kontaktuje systém PNS s použitím konkrétního klienta aplikace hello popisovač tootarget hello.
4. Hello systém PNS předá hello oznámení toohello zařízení určeného popisovačem hello.

![][0]

## <a name="hello-challenges-of-push-notifications"></a>Hello výzvy nabízená oznámení
PNSes jsou efektivní, opustí vývojáři aplikace mnohem pracovní toohello v pořadí tooimplement i běžných scénářů, jako je například vysílání nebo odesílání nabízených oznámení toosegmented uživatele.

Oznámení jsou jedním z hello nejvíce požadované funkce v mobilních cloudové služby, protože jeho pracovní vyžaduje komplexní infrastruktury, které jsou hlavní obchodní logice nesouvisejícími toohello aplikace. Některé problémy hello infrastruktury jsou:

* **Závislost na platformě**: 

  * back-end Hello musí toohave komplexní a pevné udržovat závislé na platformě logiku toosend oznámení toodevices na různých platformách, jako nejsou unified PNSes.
* **Škálování**:

  * Podle pokynů pro systém PNS zařízení tokeny musí aktualizovat při každé spuštění aplikace. To znamená, že back-end hello se zabývají velký objem přenosů a databáze přístup jenom tookeep hello tokeny aktuální. Když hello počet zařízení naroste toohundreds a tisíce miliony, je masivní hello náklady na vytvoření a údržbu této infrastruktury.
  * Většina PNSes nepodporují všesměrového vysílání toomultiple zařízení. To znamená jednoduchého vysílání tooa mil. zařízení výsledkem volání milion toohello PNSes. Toto množství provozu škálování s minimální latence není triviální.
* **Směrování**:
  
  * I když PNSes poskytují způsob toosend zprávy toodevices, většina oznámení aplikace se budou zaměřovat na uživatelům nebo skupinám. To znamená, že back-end hello musí zachovat registru zařízení tooassociate zájmových skupin, vlastností, uživatelů atd. Tato dodatečná režie prodlužuje čas toohello toomarket a údržby náklady aplikace.

## <a name="why-use-notification-hubs"></a>Proč používat Notification Hubs?
Notification Hubs eliminuje složité všechny kroky přidružené k povolení oznámení vlastní. Jeho infrastrukturu pro více platformami a horizontálně škálovanou nabízená oznámení snižuje související nabízené kódy a zjednodušuje váš back-end. S Notification Hubs jenom pro registraci jejich popisovačů systému PNS s rozbočovačem, zatímco back-end hello odesílá zprávy toousers nebo uživatelským skupinám, jak ukazuje následující obrázek hello zodpovídají zařízení:

![][1]

Centra oznámení je modul nabízené připravené k použití s hello následující výhody:

* **Různé platformy**

  * Podpora pro všechny hlavní nabízené platformy včetně iOS, Android, Windows a Kindle a Baidu.
  * Společné rozhraní toopush tooall platformy ve specifické pro platformu nebo nezávislé na platformě formáty se žádný pracovní specifické pro platformu.
  * Zařízení zpracovávat správu na jednom místě.
* **Mezi back-EndY**
  
  * Cloudu nebo místně
  * Rozhraní .NET, Node.js, Java, atd.
* **Bohatá sada schémat doručování**:

  * *Vysílání tooone nebo více platforem*: můžete okamžitě všesměrového vysílání toomillions zařízení napříč platformami pomocí jednoho volání rozhraní API.
  * *Push toodevice*: můžete určit cílovou oznámení tooindividual zařízení.
  * *Push toouser*: funkce značky a šablony můžete dosáhnout všechna zařízení a platformy uživatele.
  * *Push toosegment s dynamické značkami*: funkce značky vám pomůže segmentovat zařízení a push toothem podle potřeby tooyour, zda jsou odesílání tooone segmentu nebo výraz, který segmenty (např. aktivní a život v Praze není nového uživatele). Namísto s omezeným přístupem toopub-sub můžete aktualizovat zařízení značky kdekoliv a kdykoliv.
  * *Lokalizované nabízené*: funkce šablony vám pomůže dosáhnout lokalizace bez ovlivnění back-end kód.
  * *Tichou nabízené*: vzor hello nabízené pro vyžádání obsahu můžete umožňuje odesílání toodevices tichou oznámení a je aktivován toocomplete určité si nebo akce.
  * *Naplánované nabízené*: toosend oznámení můžete naplánovat kdykoli.
  * *Přímé nabízené*: registrace zařízení s naše služby a přímo batch nabízené tooa seznam popisovačů zařízení, můžete přeskočit.
  * *Přizpůsobené nabízené*: zařízení nabízené proměnné pomáhá odeslat konkrétní zařízení přizpůsobené nabízená oznámení pomocí vlastní páry klíč hodnota.
* **Bohatá telemetrie**
  
  * Obecné nabízená oznámení, zařízení, chyby a operaci telemetrie je k dispozici v hello portál Azure a programově.
  * Za zpráva Telemetrie sleduje každý nabízené od vaší služby tooour volání prvotní žádost úspěšně dávkování hello přenáší.
  * Zpětná vazba systému oznámení platformy komunikuje všechny zpětnou vazbu ze systémů oznámení čip tooassist při ladění.
* **Škálovatelnost** 
  
  * Odeslání rychlé zprávy toomillions zařízení bez horizontálního dělení předělávání architektury nebo zařízení.
* **Zabezpečení**

  * Sdílený tajný klíč přístupového (SAS) nebo federovaného ověřování.

## <a name="integration-with-app-service-mobile-apps"></a>Integrace s App Service Mobile Apps
toofacilitate bezproblémové a jednotných prostředí napříč službami Azure, [App Service Mobile Apps] integrovanou podporu pro nabízená oznámení pomocí centra oznámení. [App Service Mobile Apps] nabízí vysoce škálovatelnou a globálně dostupnou mobilních aplikací vývojové platformy pro vývojáře a integrátory přináší bohatou sadu funkcí toomobile vývojáři.

Vývojáři v Mobile Apps můžete Notification Hubs využívat hello následující pracovní postup:

1. Načtení popisovače systému PNS zařízení
2. Registrace zařízení s Notification Hubs prostřednictvím vhodné API registraci Mobile Apps Client SDK
   * Mějte na paměti, že služba Mobile Apps odstraní z bezpečnostních důvodů při registraci všechny značky. Spolupracovat s centra oznámení z backendu přímo tooassociate značky se zařízeními.
3. Odesílání oznámení z back-endu aplikace pomocí Notification Hubs

Zde jsou některé výhody, které začlenění toodevelopers díky této integraci:

* **Mobilní aplikace klientské sady SDK**: tyto multiplatformní sady SDK poskytují jednoduchá rozhraní API pro registraci a komunikovat toohello centra oznámení propojeným s hello mobilní aplikace automaticky. Vývojáři není nutné toodig prostřednictvím přihlašovacích údajů k Notification Hubs a pracovat s další službou.

  * *Push toouser*: hello sady SDK automaticky označí hello zadané zařízení s Mobile Apps ověřeným ID uživatele tooenable nabízené toouser scénář.
  * *Push toodevice*: hello sady SDK automaticky používat hello instalační ID Mobile Apps jako identifikátor GUID tooregister s Notification Hubs, což vývojářům hello potíže Správa více identifikátorů GUID služby.
* **Instalační model**: Mobile Apps pracuje s centra oznámení poslední nabízené modelu toorepresent všechny vlastnosti přidružené k zařízením v instalaci JSON, který zarovnaná s služeb nabízených oznámení a je snadno toouse push.
* **Flexibilita**: vývojáři se vždy mohou rozhodnout toowork přímo s Notification Hubs i s integrací hello na místě.
* **Integrované prostředí na [portál Azure]**: oznámení v Mobile Apps vizuálně reprezentována funkce a vývojáři mohou snadno pracovat s hello přidruženým centrem oznámení přes Mobile Apps.

## <a name="next-steps"></a>Další kroky
Další informace o Notification Hubs naleznete v těchto tématech:

* **[Jak zákazníci používají Notification Hubs]**
* **[Kurzy a příručky k Notification Hubs]**
* **Kurzy Začínáme centra oznámení**: [iOS], [Android], [univerzální pro Windows], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android]

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png
[Jak zákazníci používají Notification Hubs]: http://azure.microsoft.com/services/notification-hubs
[Kurzy a příručky k Notification Hubs]: http://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[univerzální pro Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[portál Azure]: https://portal.azure.com
[tags]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
