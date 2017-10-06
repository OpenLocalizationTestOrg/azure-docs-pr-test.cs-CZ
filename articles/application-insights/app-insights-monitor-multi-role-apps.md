---
title: "aaaAzure Application Insights podporu pro více komponent, mikroslužeb a kontejnery | Microsoft Docs"
description: "Monitorování aplikací, které se skládají z několika součástí nebo role pro výkonu a využití."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: bwren
ms.openlocfilehash: 6185eedf32ec450d7541603b94de6c3dcdf64a85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-multi-component-applications-with-application-insights-preview"></a>Monitorování více součásti aplikací s nástrojem Application Insights (preview)

Můžete monitorovat aplikace, které se skládají z více součásti serveru, role nebo služby s [Azure Application Insights](app-insights-overview.md). Stav Hello hello součástí a hello vztahů mezi nimi jsou zobrazeny na jednu mapu aplikace. Mohou sledovat jednotlivých operací prostřednictvím více součástí, které se automatické korelace HTTP. Kontejner diagnostiky můžete integrovat a korelační s telemetrií aplikace. Použijte jeden prostředek Application Insights pro všechny součásti hello vaší aplikace. 

![Mapa více součásti aplikace](./media/app-insights-monitor-multi-role-apps/app-map.png)

Používáme, součásti, zde toomean libovolná funkční součást rozsáhlé aplikace. Například typický obchodní aplikace může obsahovat kód klienta, které jsou spuštěné v prohlížečích webové, rozhovoru tooone nebo další webové aplikace služby, které pak použijte zpět ukončení služby. Součásti serveru může být hostovaná místně na v hello cloudu, nebo může být Azure webové a pracovní role nebo může spustit v kontejnerech například Docker nebo Service Fabric. 

### <a name="sharing-a-single-application-insights-resource"></a>Sdílení jednoho prostředku Application Insights 

Hello klíče technika tady je toosend telemetrie z každé součástí vaší aplikace toohello stejný prostředek Application Insights, ale použití hello `cloud_RoleName` vlastnost toodistinguish součásti, pokud je to nezbytné. Hello Application Insights SDK přidá hello `cloud_RoleName` emitování vlastnost toohello telemetrie součásti. Například hello SDK přidá název webového serveru nebo služby role název toohello `cloud_RoleName` vlastnost. Tato hodnota se telemetryinitializer můžete přepsat. Hello mapy aplikací používá hello `cloud_RoleName` vlastnost tooidentify hello součásti na mapě hello.

Další informace o přepsání hello `cloud_RoleName` vlastnost najdete [přidávat vlastnosti: ITelemetryInitializer](app-insights-api-filtering-sampling.md#add-properties-itelemetryinitializer).  

V některých případech to nemusí být vhodné a dáte možná přednost toouse samostatné prostředky pro různé skupiny součástí. Můžete například potřebovat, aby toouse různých prostředků pro správu nebo pro účely fakturace. Pomocí samostatných prostředků znamená, že nevidíte všechny součásti hello zobrazí na jednu mapu aplikace; a že nemůže zadat dotaz mezi komponentami v [Analytics](app-insights-analytics.md). Máte také tooset systémové prostředky samostatné hello.

S výstrahou budeme předpokládat v hello zbytek tohoto dokumentu má toosend data z několika součástí tooone prostředek Application Insights.

## <a name="configure-multi-component-applications"></a>Konfigurace více součásti aplikací

mapování tooget více součásti aplikace, musíte tooachieve tyto cíle:

* **Nainstalujte nejnovější předběžné verze hello** balíček Application Insights v jednotlivých součástí aplikace hello. 
* **Sdílet jeden prostředek Application Insights** pro všechny součásti aplikace hello.
* **Povolit aplikaci mapování více rolí** v okně hello verze Preview.

Nakonfigurujte jednotlivé komponenty aplikace pomocí odpovídající metody hello u tohoto typu. ([ASP.NET](app-insights-asp-net.md), [Java](app-insights-java-get-started.md), [Node.js](app-insights-nodejs.md), [JavaScript](app-insights-javascript.md).)

### <a name="1-install-hello-latest-pre-release-package"></a>1. Instalovat nejnovější balíček předběžné verze hello

Aktualizace nebo nainstalujte hello Statistika aplikací balíčky v projektu hello pro každou součást serveru. Pokud používáte Visual Studio:

1. Klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**. 
2. Vyberte **zahrnout předběžné verze**.
3. Pokud je balíčky zobrazí v aktualizace, vybrat Application Insights. 

    Jinak vyhledávat a instalovat příslušný balíček hello:
    
    * Microsoft.ApplicationInsights.WindowsServer
    * Microsoft.ApplicationInsights.ServiceFabric - pro součásti spuštěný jako Host spustitelných souborů a Docker kontejnery spuštění aplikace v Service Fabric
    * Microsoft.ApplicationInsights.ServiceFabric.Native - pro spolehlivé služby v aplikacích ServiceFabric
    * Microsoft.ApplicationInsights.Kubernetes pro součásti systémem Kubernetes v Docker

### <a name="2-share-a-single-application-insights-resource"></a>2. Sdílet jeden prostředek Application Insights

* V sadě Visual Studio, klikněte pravým tlačítkem na projekt a vyberte **konfigurovat Application Insights**, nebo **Application Insights > Konfigurovat**. Pro první projekt hello použijte Průvodce toocreate hello prostředek Application Insights. Pro další projekty vyberte hello stejného zdroje.
* Pokud není žádná nabídka Application Insights, nakonfigurujte ručně:

   1. V [portál Azure](https://portal,azure.com), otevřete prostředek Application Insights hello jste již vytvořili pro jiné komponenty.
   2. Okno Přehled hello, karta otevřete hello Essentials rozevíracího seznamu a kopírování hello **klíč instrumentace.**
   3. V projektu otevřete soubor ApplicationInsights.config a vložit:`<InstrumentationKey>your copied key</InstrumentationKey>`

![Zkopírujte soubor .config klíče toohello instrumentace hello](./media/app-insights-monitor-multi-role-apps/copy-instrumentation-key.png)


### <a name="3-enable-multi-role-application-map"></a>3. Povolit aplikaci mapování více rolí

V hello portálu Azure otevřete hello prostředků pro vaši aplikaci. V okně hello verze Preview povolit *mapování aplikace s více rolí*.

### <a name="4-enable-docker-metrics-optional"></a>4. Povolit Docker metriky (volitelné) 

Pokud součást běží v Docker, hostované na virtuálním počítači Windows Azure, můžete shromažďovat další metriky z kontejneru hello. Vložte tuto ve vaší [Azure diagnostics](../monitoring-and-diagnostics/azure-diagnostics.md) konfigurační soubor:

```
"DiagnosticMonitorConfiguration": {
        ...
        "sinks": "applicationInsights",
        "DockerSources": {
                "Stats": {
                    "enabled": true,
                    "sampleRate": "PT1M"
                }
            },
        ...
    }
    ...   
    "SinksConfig": {
        "Sink": [{
            "name": "applicationInsights",
            "ApplicationInsights": "<your instrumentation key here>"
        }]
    }
    ...
}

```

## <a name="use-cloudrolename-tooseparate-components"></a>Použití cloud_RoleName tooseparate komponent

Hello `cloud_RoleName` vlastnost je připojený tooall telemetrie. Identifikuje součást hello - hello roli nebo službě -, která pochází hello telemetrie. (Není stejná hello jako cloud_RoleInstance, který odděluje identické rolí, které běží souběžně na více procesy serveru nebo počítače.)

Hello portálu můžete filtrovat nebo segmentovat telemetrie používání této vlastnosti. V tomto příkladu je okno selhání hello filtrované tooshow jenom informace z hello front-endu webové služby, filtrování chyb z back-end CRM API hello:

![Metriky grafu segmentované podle názvu Role cloudu](./media/app-insights-monitor-multi-role-apps/cloud-role-name.png)

## <a name="trace-operations-between-components"></a>Operace trasování mezi součástmi

Můžete sledovat z jedné součásti tooanother, hello volání provedená při zpracování jednotlivých operací.


![Zobrazit telemetrii pro operaci](./media/app-insights-monitor-multi-role-apps/show-telemetry-for-operation.png)

Klikněte na tlačítko prostřednictvím tooa korelační seznam telemetrická data pro tuto operaci napříč hello klientské části webového serveru a rozhraní API back-end hello:

![Hledání mezi komponentami](./media/app-insights-monitor-multi-role-apps/search-across-components.png)


## <a name="next-steps"></a>Další kroky

* [Samostatné telemetrie z vývoj, testování a provozním](app-insights-separate-resources.md)
