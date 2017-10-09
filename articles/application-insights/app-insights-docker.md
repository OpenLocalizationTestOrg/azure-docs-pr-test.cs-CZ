---
title: "aaaMonitor Docker aplikací ve službě Azure Application Insights | Microsoft Docs"
description: "Čítače výkonu docker, události a výjimek lze zobrazit na Application Insights, společně s hello telemetrie z hello kontejnerové aplikace."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 27a3083d-d67f-4a07-8f3c-4edb65a0a685
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 9aaf1076bae25485a396db1bb3dcd13bccd87c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-docker-applications-in-application-insights"></a>Monitorování Docker aplikací ve službě Application Insights
Události životního cyklu a výkonu čítače z [Docker](https://www.docker.com/) kontejnery může být v grafu zobrazena na Application Insights. Nainstalujte hello [Application Insights](app-insights-overview.md) bitové kopie v kontejneru v hostiteli a zobrazí čítače výkonu pro hello hostitele, jak dobře jako u hello ostatní Image.

Pomocí Docker distribuovat aplikace v lightweight kontejnery kompletní s všechny závislosti. Budete běží na jakýkoli počítač hostitele, který spouští modul Docker.

Když spustíte hello [Application Insights image](https://hub.docker.com/r/microsoft/applicationinsights/) hostiteli Docker získáte tyto výhody:

* Životní cyklus telemetrii týkající se všechny kontejnery hello spuštěná na hostiteli hello - spuštění, zastavení a tak dále.
* Čítače výkonu pro všechny kontejnery hello. Využití procesoru, paměti, využití sítě a další.
* Pokud jste [nainstalován Application Insights SDK pro jazyk Java](app-insights-java-live.md) ve hello aplikace běžící v kontejnerech hello, budou všechny telemetrická hello těchto aplikací mít další vlastnosti, které určíte hello kontejneru a hostitelský počítač. Tak například pokud máte instance aplikace spuštěná ve více než jednomu hostiteli, lze snadno filtrovat telemetrie aplikace hostitele.

![Příklad](./media/app-insights-docker/00.png)

## <a name="set-up-your-application-insights-resource"></a>Nastavit zdroje Application Insights
1. Přihlaste se k [portálu Microsoft Azure](https://azure.com) a otevřete prostředek Application Insights hello aplikace, nebo [vytvořte novou](app-insights-create-new-resource.md). 
   
    *Který prostředek by měl používat?* Pokud někdo vyvinul hello aplikace, které jsou spuštěné v hostiteli, pak musíte příliš[vytvořte nový prostředek Application Insights](app-insights-create-new-resource.md). Toto je, kde můžete zobrazit a analyzovat hello telemetrie. (Vyberte obecné pro typ aplikace hello.)
   
    Ale pokud jste vývojář hello hello aplikace, pak Věříme, že jste [přidat Application Insights SDK](app-insights-java-live.md) tooeach z nich. Pokud jsou všechny skutečně součásti jeden obchodní aplikace, pak můžete nakonfigurovat všechny z nich toosend telemetrie tooone prostředků a budete používat tato prostředků toodisplay hello Docker životního cyklu a výkonu data. 
   
    Třetí scénář je vyvinuté většinu aplikací hello, že používáte samostatné prostředky toodisplay jejich telemetrie. V takovém případě budete pravděpodobně také chcete toocreate samostatné prostředku pro hello Docker data. 
2. Přidat hello Docker dlaždice: Zvolte **přidat dlaždici**, přetáhněte dlaždici Docker hello z Galerie hello a pak klikněte na tlačítko **provádí**. 
   
    ![Příklad](./media/app-insights-docker/03.png)
3. Klikněte na tlačítko hello **Essentials** rozevíracího seznamu a zkopírujte klíč instrumentace hello. Použít tento tootell hello SDK kde toosend jeho telemetrie.

    ![Příklad](./media/app-insights-docker/02-props.png)

Nechte okno prohlížeče užitečný, jak je budete vraťte tooit brzy toolook v telemetrii.

## <a name="run-hello-application-insights-monitor-on-your-host"></a>Spustit monitorování hello Application Insights na hostiteli
Teď, když máte někde toodisplay hello telemetrie, můžete nastavit hello kontejnerizované aplikaci, která bude shromažďovat a odesílat je.

1. Připojte hostitele Docker tooyour. 
2. Upravit klíč instrumentace do tento příkaz a potom ho spusťte:
   
   ```
   
   docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
   ```

Pouze jeden obrázek Application Insights je vyžadován na Docker hostitele. Pokud vaše aplikace je nasazená na několika hostitelích Docker, pak opakujte příkaz hello na každém hostiteli.

## <a name="update-your-app"></a>Aktualizace aplikace
Pokud vaše aplikace není instrumentována s hello [Application Insights SDK pro jazyk Java](app-insights-java-get-started.md), přidejte následující řádek do souboru ApplicationInsights.xml hello ve vašem projektu, v části hello hello `<TelemetryInitializers>` element:

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

Tento postup přidá Docker informace, jako je například kontejneru a hostitele id tooevery telemetrie položek odeslaných z vaší aplikace.

## <a name="view-your-telemetry"></a>Zobrazení telemetrie
Vraťte se zpátky prostředek Application Insights tooyour hello portálu Azure.

Proklikejte se prostřednictvím hello Docker dlaždice.

Krátce zobrazí dat odesílaných z aplikace hello Docker, zvláště pokud máte jiné kontejnery systémem Docker modul.

Tady jsou některé z hello zobrazení, které můžete získat.

### <a name="perf-counters-by-host-activity-by-image"></a>Čítače výkonu hostitele, aktivita podle bitové kopie
![Příklad](./media/app-insights-docker/10.png)

![Příklad](./media/app-insights-docker/11.png)

Klikněte na libovolný název hostitele nebo bitové kopie pro více podrobností.

toocustomize hello zobrazení, klikněte na libovolný graf, tabulka hello záhlaví, nebo pomocí přidat graf. 

[Další informace o Průzkumníku metrik](app-insights-metrics-explorer.md).

### <a name="docker-container-events"></a>Události kontejner docker
![Příklad](./media/app-insights-docker/13.png)

Klikněte na jednotlivé události tooinvestigate, [vyhledávání](app-insights-diagnostic-search.md). Hledat a filtr toofind hello události, které chcete. Další podrobnosti kliknutím na tlačítko tooget žádné události.

### <a name="exceptions-by-container-name"></a>Výjimky podle názvu kontejneru
![Příklad](./media/app-insights-docker/14.png)

### <a name="docker-context-added-tooapp-telemetry"></a>Kontext docker přidat tooapp telemetrie
Žádost o telemetrické zprávy odesílané z aplikace hello instrumentovány s AI SDK a rozšíření s kontextem Docker:

![Příklad](./media/app-insights-docker/16.png)

Čas procesoru a paměti k dispozici čítače výkonu, rozšíření a seskupené podle Docker název kontejneru:

![Příklad](./media/app-insights-docker/15.png)

## <a name="q--a"></a>Dotazy a odpovědi
*Co Application Insights mě, nemůže dostat z Docker?*

* Podrobné rozpis čítačů výkonu tak, že kontejner a bitové kopie.
* Integrace aplikace a kontejneru dat v jednom řídicím.
* [Export telemetrie](app-insights-export-telemetry.md) pro další tooa databáze analýzy, Power BI nebo jiné řídicí panel.

*Jak získat telemetrie z hello aplikace?*

* Nainstalujte hello Application Insights SDK do aplikace hello. Zjistěte, jak pro: [webové aplikace v jazyce Java](app-insights-java-get-started.md), [Windows webové aplikace](app-insights-asp-net.md).

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Další kroky

* [Application Insights pro jazyk Java](app-insights-java-get-started.md)
* [Application Insights pro Node.js](app-insights-nodejs.md)
* [Application Insights pro ASP.NET](app-insights-asp-net.md)
