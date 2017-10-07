---
title: "aaaGet začít s testování v produkčním prostředí pro webové aplikace"
description: "Další informace o hello Test v provozním (TiP) funkce v Azure App Service Web Apps."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 4623468d-886e-4203-8012-8f86deb2790b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: cephalin
ms.openlocfilehash: 2ddbd532ffe2a4f3e07fd386d9741a3fde3639ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-test-in-production-for-web-apps"></a>Začínáme s testováním v produkčním prostředí pro Web Apps
Testování v produkčním prostředí, nebo za provozu testování vaší webové aplikace pomocí za provozu zákazníka provoz, je test strategie, vývojáři aplikací stále integrovat do svých [agile vývoj](https://en.wikipedia.org/wiki/Agile_software_development) metody. Umožní vám tootest hello kvalitu aplikace s provozu generovaného uživateli za provozu v provozním prostředí, jako názvem na rozdíl od toosynthesized data v testovacím prostředí. Díky zpřístupnění noví uživatelé tooreal aplikace, může být informován o hello skutečné problémy, které vaše aplikace může být vystaven po jejím nasazení. Můžete ověřit funkčnost hello, výkon a hodnota vaše aplikace aktualizace před hello svazku a rychlosti a řadu přenos reálný uživatel, který můžete nikdy Přibližná v testovacím prostředí.

## <a name="traffic-routing-in-app-service-web-apps"></a>Přenosy dat směrování ve službě App Service Web Apps
S hello směrování provozu funkci v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), nasměrujete část tooone provozu za provozu uživatele nebo více [nasazovací sloty](web-sites-staged-publishing.md)a potom analyzovat vaší aplikace pomocí [aplikace Azure Statistika](/services/application-insights/) nebo [Azure HDInsight](/services/hdinsight/), nebo jako nástroj třetí strany [New Relic](/marketplace/partners/newrelic/newrelic/) toovalidate změny. Například můžete implementovat hello následující scénáře službou App Service:

* Zjistit funkční chyby nebo přesně určit kritické body ve vašem nasazení aktualizace předchozí toosite celou
* "Řízené testovací lety" změny provést měření použitelnost metriky pro beta verzi aplikace hello
* Postupně rychle pochopit práci tooa nové aktualizace a řádně zpět dolů toohello aktuální verze, pokud dojde k chybě 
* Optimalizace aplikace obchodní výsledky spuštěním [A / B testy](https://en.wikipedia.org/wiki/A/B_testing) nebo [multivariační testy](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) v počet nasazovacích slotů

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Požadavky pro použití ke směrování provozu ve službě Web Apps
* Webové aplikace, musí běžet v **standardní** nebo **Premium** vrstvy, jako je povinný pro více nasazovací sloty.
* V pořadí toowork správně, směrování provozu vyžaduje toobe soubory cookie v prohlížeči hello uživatelů povolené. Směrování provozu používá soubory cookie toopin slotu nasazení tooa prohlížeč klienta pro relaci klienta hello hello životnosti.
* Směrování provozu podporuje pokročilé scénáře TiP pomocí rutin prostředí Azure PowerShell.

## <a name="route-traffic-segment-tooa-deployment-slot"></a>Směrování provozu segment tooa nasazovací slot.
Na základní úrovni hello v každý scénář TiP směrovat předdefinované procento vaší živé provoz tooa mimo produkční nasazovací slot. toodo tento, postupujte podle kroků hello níže:

> [!NOTE]
> Hello zde uvedených kroků se předpokládá, že už máte [mimo produkční nasazovací slot](web-sites-staged-publishing.md) a že hello požadovaného obsahu webové aplikace je již [nasazené](web-sites-deploy.md) tooit.
> 
> 

1. Přihlaste se k hello [portálu Azure](https://portal.azure.com/).
2. V okně vaší webové aplikace, klikněte na tlačítko **nastavení** > **směrování provozu**.
   ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Vyberte hello slotu, který chcete tooroute provoz tooand hello procento celkového provozu hello požadavky a pak klikněte na **Uložit**.
   
    ![](./media/app-service-web-test-in-production/02-select-slot.png)
4. Okno přejděte toohello nasazovací slot. Teď byste měli vidět za provozu provoz se směruje tooit.
   
    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Po nakonfigurování směrování provozu hello zadat, že procento klientů, bude mít náhodně směrované tooyour mimo produkční slot. Je však důležité toonote, jakmile je klient automaticky směrované tooa konkrétní pozici, bude slotu "definovaného" toothat dobu životnosti hello této relace klienta. To provést pomocí souboru cookie toopin hello uživatelské relace. Je-li si prohlédnout hello HTTP požadavků, najdete `TipMix` souborů cookie v každé další požadavek.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-tooa-specific-slot"></a>Vynutit klienta požadavky tooa konkrétní pozici
App Service v přidání tooautomatic ke směrování provozu, je možné tooroute požadavky tooa konkrétní pozici. To je užitečné, když chcete, aby vaši uživatelé toobe možné tooopt-do nebo výslovný nesouhlas s beta verzi aplikace. toodo, použijte hello `x-ms-routing-name` parametr dotazu.

tooreroute uživatelé tooa konkrétní pozici pomocí `x-ms-routing-name`, ujistěte se, že hello slotu je již přidána toohello směrování provozu seznamu. Vzhledem k tomu, že chcete tooroute tooa slotu explicitně, není důležité hello skutečné směrování procento, které nastavíte. Pokud chcete, můžete vytvořit "beta link", který mohou uživatelé kliknout tooaccess hello beta verzi aplikace.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>OPT uživatelé mimo aplikaci beta
Uživatelé toolet vyjádření výslovného nesouhlasu s beta verze aplikace, například můžete vložit tento odkaz na webové stránce:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back tooproduction app</a>

Hello řetězec `x-ms-routing-name=self` určuje hello produkční slot. Jakmile hello klienta browser link hello přístup a ne jenom ho přesměruje toohello produkční slot, ale každý další požadavek bude obsahovat hello `x-ms-routing-name=self` souboru cookie, který PIN kódy hello relace toohello produkční slot.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-toobeta-app"></a>OPT uživatele v aplikaci toobeta
Uživatelé toolet vyjádřit výslovný souhlas tooyour beta verze aplikace, sada hello stejný dotaz na název parametru toohello hello mimo produkční slot, například:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Další zdroje informací
* [Nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)
* [Nasazení aplikace komplexní předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Agile software development službou Azure App Service](app-service-agile-software-development.md)
* [Efektivně používat DevOps prostředí pro webové aplikace](app-service-web-staged-publishing-realworld-scenarios.md)

