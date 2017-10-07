---
title: "aaaTroubleshoot Application Insights ve webovém projektu Java"
description: "Průvodce odstraňováním potíží s – monitorování provozu aplikace v jazyce Java pomocí Application Insights."
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: c274c01b1992971fae194c3e510512ca06ab76b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Řešení potíží a otázky a odpovědi v nástroji Application Insights
Dotazy nebo problémy s [Azure Application Insights v jazyce Java][java]? Zde jsou některé tipy.

## <a name="build-errors"></a>Chyby sestavení
**V prostředí Eclipse při přidávání hello Application Insights SDK prostřednictvím Maven nebo Gradle, zobrazuje chyby ověření sestavení nebo kontrolního součtu.**

* Pokud hello závislostí <version> element je pomocí vzoru se zástupnými znaky (například (Maven) `<version>[1.0,)</version>` nebo (Gradle) `version:'1.0.+'`), zkuste místo toho zadat konkrétní verzi jako `1.0.2`. V tématu hello [poznámky k verzi](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) hello nejnovější verzi.

## <a name="no-data"></a>Žádná data
**I přidat Application Insights úspěšně a spustil mé aplikace, ale nikdy, uloží se data hello portálu.**

* Počkejte několik minut a klikněte na tlačítko Aktualizovat. pravidelně Hello grafy sami obnovit, ale můžete taky aktualizovat ručně. interval obnovování Hello závisí na hello časové rozmezí hello grafu.
* Zkontrolujte, že máte klíč instrumentace, který je definován v souboru ApplicationInsights.xml hello (ve složce hello prostředky ve vašem projektu)
* Ověřte, zda je žádné `<DisableTelemetry>true</DisableTelemetry>` uzlu v souboru xml hello.
* V bráně firewall můžete mít tooopen porty TCP 80 a 443 pro odchozí provoz toodc.services.visualstudio.com. V tématu hello [úplný seznam výjimky brány firewall](app-insights-ip-addresses.md)
* V hello Microsoft Azure spustit Tabule, podívejte se na mapě stav služby hello. Pokud jsou některé výstrahy indikace, počkejte, dokud se vrátili tooOK a pak zavřete a znovu otevřete okně vaší aplikace Application Insights.
* Zapnout protokolování okna konzoly toohello IDE, tak, že přidáte `<SDKLogger />` prvek v rámci hello kořenového uzlu souboru ApplicationInsights.xml hello (ve složce hello prostředky ve vašem projektu) a zkontrolujte položky, kterými [Chyba].
* Ujistěte se, že hello správné souboru ApplicationInsights.xml byl úspěšně načten podle hello sady Java SDK prohlížením zprávy výstup hello konzoly pro příkaz "konfigurační soubor byl úspěšně nalezl".
* Pokud není nalezen hello konfigurační soubor, zkontrolujte toosee zprávy výstup hello, kde má být vyhledán hello konfiguračního souboru pro a ujistěte se, že hello ApplicationInsights.xml se nachází v jedné z těchto umístění vyhledávání. Jako existuje pravidlo můžete umístit hello konfiguračního souboru téměř hello Application Insights SDK JARs. Příklad: v Tomcat, to znamená hello složku WEB-INF/lib.

#### <a name="i-used-toosee-data-but-it-has-stopped"></a>Mohu použít toosee data, ale byla zastavena
* Zkontrolujte hello [stav blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Jste nedosáhli vaše měsíční kvóta datových bodů? Otevřete nastavení nebo kvóty a cena toofind limitu. Pokud ano, můžete upgradovat plán nebo platit dodatečnou kapacitu. V tématu hello [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-hello-data-im-expecting"></a>Nejsou zobrazeny všechny hello data, která se byla očekávána
* Otevřete hello kvóty a ceny okno a zkontrolujte, zda [vzorkování](app-insights-sampling.md) je v provozu. (přenos 100 % znamená, že vzorkování není v provozu.) Hello služby Application Insights může být sada tooaccept pouze část hello telemetrická data přenášená z vaší aplikace. To pomáhá při synchronizaci v rámci vaší měsíční kvóta telemetrie. 

## <a name="no-usage-data"></a>Žádná data o využití
**Zobrazuje, data o žádosti a doby odezvy, ale žádné zobrazení stránky, prohlížeč nebo uživatelská data.**

Byl úspěšně nainstalován se telemetrie aplikace toosend ze serveru hello. Teď je dalším krokem je příliš[nastavení webové stránky toosend telemetrii z webového prohlížeče hello][usage].

Případně pokud se váš klient je aplikace v [telefonu nebo jiné zařízení][platforms], mohla odesílat telemetrii z ní. 

Použití hello stejné instrumentace klíče tooset si klient i server telemetrie. Hello data se zobrazí v hello stejný prostředek Application Insights, a budete moct toocorrelate události z klienta a serveru.


## <a name="disabling-telemetry"></a>Vypnutí telemetrie
**Jak můžete zakázat telemetrii kolekce?**

V kódu:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Nebo** 

Aktualizujte soubor ApplicationInsights.xml (ve složce hello prostředky ve vašem projektu). Přidejte následující hello pod hello kořenového uzlu:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Pomocí metody hello XML, když změníte hodnotu hello mít toorestart hello aplikaci.

## <a name="changing-hello-target"></a>Změna cílové hello
**Jak můžete změnit který Azure prostředek projektu odešle data?**

* [Získejte klíč instrumentace hello hello nový prostředek.][java]
* Pokud jste přidali Application Insights tooyour projekt pomocí hello Azure Toolkit pro Eclipse, klikněte pravým tlačítkem na webový projekt, vyberte **Azure**, **konfigurovat Application Insights**a změňte hello klíč.
* V opačném aktualizujte klíč hello v ApplicationInsights.xml do složky hello prostředky ve vašem projektu.

## <a name="debug-data-from-hello-sdk"></a>Ladění data z hello SDK

**Jak můžete zjistit, jaké hello provádí SDK?**

Přidat další informace o co se děje v hello rozhraní API, tooget `<SDKLogger/>` pod hello kořenového uzlu souboru ApplicationInsights.xml konfigurace hello.

Můžete také určit, aby hello protokolovacího nástroje toooutput tooa souboru:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

soubory Hello najdete v části `%temp%\javasdklogs` nebo `java.io.tmpdir` v případě Tomcat server.


## <a name="hello-azure-start-screen"></a>Hello Azure úvodní obrazovce
**Dívám se na [hello portál Azure](https://portal.azure.com). Hello mapy chci se dozvědět něco o mé aplikaci?**

Ne, zobrazuje stav hello servery Azure kolem hello, world.

*Z hello Azure počáteční Tabule (domovskou obrazovku) jak lze najít data o mé aplikaci?*

Za předpokladu, že jste [nastavit aplikaci pro službu Application Insights][java], klikněte na tlačítko Procházet, vyberte Application Insights a vyberte prostředek aplikace hello jste vytvořili pro vaši aplikaci. tooget existuje rychlejší v budoucnosti budete moct připnout Tabule start toohello vaší aplikace.

## <a name="intranet-servers"></a>Intranetové servery
**Můžete sledovat server v mém intranetu?**

Ano, pokud váš server může odesílat telemetrii toohello Application Insights portál prostřednictvím hello veřejného Internetu. 

V bráně firewall můžete mít tooopen porty TCP 80 a 443 pro odchozí provoz toodc.services.visualstudio.com a f5.services.visualstudio.com.

## <a name="data-retention"></a>Uchovávání dat
**Jak dlouho se data uchovávají portálu hello? Je bezpečné?**

V tématu [uchovávání dat a ochrana osobních údajů][data].

## <a name="next-steps"></a>Další kroky
**Mám nastavit Application Insights pro aplikace my server Java. Kde můžou dělat?**

* [Monitorování dostupnosti webových stránek][availability]
* [Sledování využití webové stránky][usage]
* [Sledování využití a diagnostikovat problémy ve svých aplikacích zařízení][platforms]
* [Zápis kódu tootrack využití vaší aplikace][track]
* [Zaznamenat diagnostických protokolů][javalogs]

## <a name="get-help"></a>Podpora
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

