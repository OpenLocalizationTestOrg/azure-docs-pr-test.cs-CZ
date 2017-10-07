---
title: aaaMonitor aplikace v Azure App Service | Microsoft Docs
description: "Zjistěte, jak hello toomonitor aplikace v Azure App Service pomocí portálu Azure."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 80d5a466102a894a49d04ae35aa54cc1d05a58df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Postupy: monitorování aplikací v Azure App Service
[Služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714) poskytuje integrované monitorování funkce v hello [portálu Azure](https://portal.azure.com).
To zahrnuje možnost tooreview hello **kvóty** a **metriky** pro aplikaci, jakož i hello plán služby App Service, nastavení **výstrahy** a i **škálování** automaticky v závislosti na tyto metriky.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Principy kvóty a metriky
### <a name="quotas"></a>Kvóty
Aplikace hostované ve službě App Service jsou subjektu toocertain *omezení* s prostředky, které mohou používat. Hello omezení jsou definovány hello **plán služby App Service** spojená s aplikací hello.

Pokud aplikace hello je hostována v **volné** nebo **sdílené** plánu, pak jsou definovány hello omezení pro aplikace hello může používat prostředky hello **kvóty**.

Pokud aplikace hello je hostována v **základní**, **standardní** nebo **Premium** plánu, pak hello omezení mohou používat prostředky hello se nastavují hello **velikost** (Malé, střední, velký) a **instance počet** (1, 2, 3,...) z hello **plán služby App Service**.

**Kvóty** pro **volné** nebo **sdílené** aplikace jsou:

* **CPU(Short)**
  * Nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci v intervalu 5 minut. Tato kvóta znovu nastaví každých 5 minut.
* **CPU(Day)**
  * Celkový počet nároky na výkon procesoru, které jsou povoleny pro tuto aplikaci za den. Tato kvóta znovu nastaví každých 24 hodin půlnoci času UTC.
* **Paměť**
  * Celková velikost paměti pro tuto aplikaci povolena.
* **Šířka pásma**
  * Celková velikost odchozí šířky pásma povolená pro tuto aplikaci za den.
    Tato kvóta znovu nastaví každých 24 hodin půlnoci času UTC.
* **Systém souborů**
  * Celková velikost úložiště, které jsou povoleny.

Hello jenom kvóta použít tooapps hostované na **základní**, **standardní** a **Premium** plány je **Filesystem**.

Další informace o hello konkrétní kvót, omezení a funkce, které jsou k dispozici pro různé identifikátory SKU služby aplikace hello naleznete zde: [omezení předplatné služby Azure](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Vynucení kvót
Pokud aplikace v jeho využití překročí hello **procesoru (krátký)**, **procesoru (den)**, nebo **šířky pásma** kvóty pak hello aplikace bude zastaven, dokud nebude znovu nastaví kvóty hello. Během této doby bude výsledkem všechny příchozí požadavky **HTTP 403**.
![][http403]

Pokud aplikace hello **paměti** dojde k překročení kvóty a pak aplikace hello se nebude znovu spuštěna.

Pokud hello **Filesystem** dojde k překročení kvóty, pak všechny zápisu se nezdaří, včetně zápis toologs.

Kvóty můžete zvýšit nebo odebrat z vaší aplikace, že si upgradujete plán služby App Service.

### <a name="metrics"></a>Metriky
**Metriky** poskytují informace o aplikaci hello nebo chování plán služby App Service.

Pro **aplikace**, jsou dostupné metriky hello:

* **Průměrná doba odezvy**
  * Hello Průměrná doba hello aplikace tooserve požadavky v ms.
* **Průměrná paměti pracovní sady**
  * Hello Průměrná velikost paměti v MIB používané aplikace hello.
* **Čas procesoru**
  * Hello nároky na výkon procesoru v sekundách spotřebovávají aplikace hello. Další informace o této metriky najdete: [procento vs procesoru čas procesoru](#cpu-time-vs-cpu-percentage)
* **Data v**
  * Hello množství příchozích šířky pásma spotřebovávají aplikace hello v MIB.
* **Data**
  * Hello množství odchozí šířky pásma spotřebovávají aplikace hello v MIB.
* **Http 2xx**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 200 ale < 300.
* **Http 3xx**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 300 ale < 400.
* **HTTP 401**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 401.
* **HTTP 403**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 403.
* **HTTP 404**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 404.
* **HTTP 406**
  * Počet požadavků, které jsou výsledkem stavový kód HTTP 406.
* **Http 4xx**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 400 ale < 500.
* **Chyby protokolu HTTP serveru**
  * Počet požadavků, které jsou výsledkem stavový kód protokolu http > = 500, ale < 600.
* **Paměť pracovní sady**
  * Aktuální velikost paměti, které aplikace hello v MIB.
* **Požadavky**
  * Celkový počet požadavků bez ohledu na jejich výsledné stavový kód HTTP.

Pro **plán služby App Service**, jsou dostupné metriky hello:

> [!NOTE]
> Metriky plánu služby App Service jsou k dispozici pro plány v pouze **základní**, **standardní** a **Premium** SKU.
> 
> 

* **Procento využití procesoru**
  * CPU průměrné Hello použít napříč všemi instancemi plánu hello.
* **Procento paměti**
  * Průměrná paměti Hello používá napříč všemi instancemi plánu hello.
* **Data v**
  * Hello průměrná příchozí šířky pásma použít napříč všemi instancemi plánu hello.
* **Data**
  * průměr Hello odchozí šířky pásma použít napříč všemi instancemi plánu hello.
* **Délka fronty disku**
  * Průměrný počet k Hello číst a zapisovat požadavků, které byly zařazeny do fronty pro úložiště. Délka fronty vysoké disku je údajem o aplikaci, která může být zpomalením z důvodu tooexcessive diskové vstupně-výstupních operací.
* **Délka fronty http**
  * Hello průměrný počet požadavků HTTP, které měl toosit na hello fronty před splnění. Vysoká nebo roste délka fronty HTTP je příznakem plánu v případě velkého zatížení.

### <a name="cpu-time-vs-cpu-percentage"></a>Procentuální hodnota vs procesoru času procesoru
<!-- toodo: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Existují 2 metriky, které odpovídají využití procesoru. **Čas procesoru** a **procento využití procesoru**

**Čas procesoru** je užitečné pro aplikace hostované v **volné** nebo **sdílené** plány, protože jeden z jejich kvóty je definována v minutách procesoru používané aplikace hello.

**Procento využití procesoru** na hello druhé straně je užitečné, pro aplikace hostované v **základní**, **standardní** a **premium** plány vzhledem k tomu může být škálovat na více systémů a tato metrika je dobrá indikace toho Dobrý den celkové využití v rámci všech instancí.

## <a name="metrics-granularity-and-retention-policy"></a>Metriky Členitosti a zásady uchovávání informací
Metriky pro plán služby aplikace a aplikace jsou protokolovány a agregovat služba hello s hello členitostí a zásady uchovávání informací:

* **Minutu** členitosti metriky se zachovají pro **48 hodin**
* **Hodina** členitosti metriky se zachovají pro **30 dnů**
* **Den** členitosti metriky se zachovají pro **90 dnů**

## <a name="monitoring-quotas-and-metrics-in-hello-azure-portal"></a>Monitorování kvóty a metriky v hello portálu Azure.
Můžete zkontrolovat stav hello hello různých **kvóty** a **metriky** ovlivnění aplikace v hello [portálu Azure](https://portal.azure.com).

![][quotas]
**Kvóty** naleznete v části Nastavení >**kvóty**. Hello UX umožňuje zkontrolovat: název kvóty hello (1), (2) jeho interval resetování, (3) jeho aktuální limit a (4) aktuální hodnotu.

![][metrics]
**Metriky** může být přístup přímo z okna prostředků hello. Můžete také upravit graf hello podle: (1) **klikněte na tlačítko** na a vyberte (2) **upravit graf**.
Odsud můžete změnit hello (3) **čas rozsah**, (4) **typ grafu**a (5) **metriky** toodisplay.  

Další informace o metrikách zde: [monitorovat metriky služby](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Výstrahy a škálování
Metriky pro plán aplikace nebo služby App Service může být připojili tooalerts, toolearn více informací o to, najdete v části [přijímat oznámení výstrah](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Aplikace služby App Service hostované v basic, standard nebo premium plány služby App Service podporují **škálování**. To vám umožní tooconfigure pravidla, která monitorování metriky plánu služby App Service a můžete zvýšit nebo snížit počet instancí hello poskytuje další zdroje informací, podle potřeby nebo ukládání peníze přečerpání zřídit po hello aplikace. Další informace o automatické škálování zde: [jak tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) a zde [osvědčené postupy pro automatické škálování Azure monitorování](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
