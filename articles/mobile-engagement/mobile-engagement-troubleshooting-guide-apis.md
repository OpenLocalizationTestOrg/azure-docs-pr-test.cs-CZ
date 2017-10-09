---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží – rozhraní API"
description: "Řešení potíží s příručky pro Azure Mobile Engagement – rozhraní API"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3efc8a52-2b74-4917-b887-815ae8277474
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 10/04/2016
ms.author: piyushjo
ms.openlocfilehash: 5656b6f0f1aaf3e496a168c7cf09b307b9ab2a4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-api-issues"></a>Průvodce řešením potíží pro problémy rozhraní API
Hello následují možných problémů se můžete setkat s jak správci komunikovat s Azure Mobile Engagement přes hello rozhraní API.

## <a name="syntax-issues"></a>Syntaxe problémy
### <a name="issue"></a>Problém
* Chyby syntaxe pomocí rozhraní API hello (nebo neočekávané chování).

### <a name="causes"></a>Způsobí, že
* Syntaxe problémy:
  * Ujistěte se, zda text hello toocheck syntaxe hello konkrétní rozhraní API, kterou používáte tooconfirm, který hello možnost je k dispozici.
  * Běžné problémy s možností využití rozhraní API je tooconfuse hello Reach API a hello Push rozhraní API (většinu úloh by měl provést s hello Reach API místo hello Push rozhraní API). 
  * Další běžné problém s integraci sady SDK a využití rozhraní API je tooconfuse hello klíč SDK a hello klíč rozhraní API.
  * Skripty, které se připojují toohello rozhraní API, potřebujete toosend data minimálně každých 10 minut nebo hello připojení, bude časový limit (zejména v běžné monitorování API skripty naslouchání pro data). vypršení časových limitů tooprevent, mají váš skript odeslání příkazu ping protokolu XMPP každých zachování připojení se serverem hello 10 minut relace hello tookeep.

### <a name="see-also"></a>Viz také
* [Dokumentaci k rozhraní API][Link 4]
* [Informace o protokolu XMPP protokolu](http://xmpp.org/extensions/xep-0199.html)

## <a name="unable-toouse-hello-api-tooperform-hello-same-action-available-in-hello-azure-mobile-engagement-ui"></a>Nelze toouse hello rozhraní API tooperform hello stejnou akci, které jsou k dispozici v hello Azure Mobile Engagement uživatelského rozhraní
### <a name="issue"></a>Problém
* Akce, která funguje z Azure Mobile Engagement uživatelského rozhraní nefunguje z hello hello související rozhraní API služby Azure Mobile Engagement.

### <a name="causes"></a>Způsobí, že
* Potvrzení, že můžete provádět stejné akce z hello Azure Mobile Engagement uživatelského rozhraní ukazuje, že jste správně integrované tuto funkci Azure Mobile Engagement s hello hello SDK.

### <a name="see-also"></a>Viz také
* [Dokumentace uživatelského rozhraní][Link 1]

## <a name="error-messages"></a>Chybové zprávy
### <a name="issue"></a>Problém
* Kódy chyb pomocí rozhraní API hello zobrazí za běhu, nebo v protokolech.

### <a name="causes"></a>Způsobí, že
* Tady je seznam složený běžné čísel kódy stavu rozhraní API pro referenční a předběžné řešení problémů:
  
        200        Success.
        200        Account updated: device registered, associated, updated, or removed from hello current account.
        200        Returns a list of projects as a JSON object or an authentication token generated and returned in hello response’s body.
        201        Account created.
        400        Invalid parameter or validation exception (check payload for details). hello parameters provided toohello API or service are invalid. In this case, hello HTTP response will embed more details. Make sure tootest for hello MIME type of hello response as hello payload can either be plain text or a JSON object.
        401        Authentication error. No user is currently authenticated or connected (check hello AppID and SDK key).
        402        Billing lock. hello application is either off its quotas or is currently in a bad billing state.
        403        hello application is not enabled or hello specific API is disabled for this application.
        403        Unauthorized access toohello project or application, invalid access key (hello key must match hello one provided when created).
        403        Campaign specific errors: campaign must be finished (or has already been activated), hello suspend action can only be performed on an scheduled campaign, cannot finish a campaign that is not currently “in progress”, campaign must be “in progress” and hello campaign’s property named, manual Push must be set tootrue.
        403        hello email address is already associated tooanother account (a super user for instance). No authentication token will be generated.
        404        Application, device, campaign, or project identifier not found.
        404        Query parameter is invalid JSON or has a field with an unexpected value.
        404        hello email address is not associated with an account. Please create or update hello account first.
        405        Invalid HTTP method (GET, POST, etc.) or trying tooedit a read only segment (i.e. add or update or delete a criterion). A segment becomes read only after it has been computed for hello first time.
        409        Name already associated tooa different device ID or campaign.
        413        Too many device identifiers (current limit is 1,000), POST URL encoded entity is over 2MB, or hello period is too large toobe displayed (hello server didn’t retrieve hello analytics because hello user request is for a period that is too large).
        503        Analytics not available yet (hello requested information is not computed yet for an application).
        504        hello server was not able toohandle your request in a reasonable time (if you make multiple calls tooan API very quickly, try toomake one call at a time and spread hello calls out over time).

### <a name="see-also"></a>Viz také
* [Dokumentace k rozhraní API – pro podrobné chyby na každé konkrétní rozhraní API][Link 4]

## <a name="silent-failures"></a>Tiché selhání
### <a name="issue"></a>Problém
* Rozhraní API akce se nezdaří s žádná chybová zpráva se zobrazí v době běhu nebo v protokolech.

### <a name="causes"></a>Způsobí, že
* Mnoho položek bude deaktivováno za hello Azure Mobile Engagement uživatelského rozhraní Pokud nejsou správně integrované, ale bez upozornění selže z hello rozhraní API, takže nezapomeňte tootest hello stejné funkce z uživatelského rozhraní toosee hello, pokud to funguje.
* Azure Mobile Engagement a mnoho pokročilých funkcí Azure Mobile Engagement pokoušíte toouse, třeba toobe jednotlivě integrována do vaší aplikace pomocí hello SDK jako samostatné kroky než budete moci použít.

### <a name="see-also"></a>Viz také
* [Průvodce odstraňováním potíží s - SDK][Link 25]

<!--Link references-->
[Link 1]: mobile-engagement-user-interface-home.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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

