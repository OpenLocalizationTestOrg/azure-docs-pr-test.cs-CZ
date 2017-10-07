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
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a><span data-ttu-id="e1321-103">Řešení potíží a otázky a odpovědi v nástroji Application Insights</span><span class="sxs-lookup"><span data-stu-id="e1321-103">Troubleshooting and Q and A for Application Insights for Java</span></span>
<span data-ttu-id="e1321-104">Dotazy nebo problémy s [Azure Application Insights v jazyce Java][java]?</span><span class="sxs-lookup"><span data-stu-id="e1321-104">Questions or problems with [Azure Application Insights in Java][java]?</span></span> <span data-ttu-id="e1321-105">Zde jsou některé tipy.</span><span class="sxs-lookup"><span data-stu-id="e1321-105">Here are some tips.</span></span>

## <a name="build-errors"></a><span data-ttu-id="e1321-106">Chyby sestavení</span><span class="sxs-lookup"><span data-stu-id="e1321-106">Build errors</span></span>
<span data-ttu-id="e1321-107">**V prostředí Eclipse při přidávání hello Application Insights SDK prostřednictvím Maven nebo Gradle, zobrazuje chyby ověření sestavení nebo kontrolního součtu.**</span><span class="sxs-lookup"><span data-stu-id="e1321-107">**In Eclipse, when adding hello Application Insights SDK via Maven or Gradle, I get build or checksum validation errors.**</span></span>

* <span data-ttu-id="e1321-108">Pokud hello závislostí <version> element je pomocí vzoru se zástupnými znaky (například (Maven) `<version>[1.0,)</version>` nebo (Gradle) `version:'1.0.+'`), zkuste místo toho zadat konkrétní verzi jako `1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="e1321-108">If hello dependency <version> element is using a pattern with wildcard characters (e.g. (Maven) `<version>[1.0,)</version>` or (Gradle) `version:'1.0.+'`), try specifying a specific version instead like `1.0.2`.</span></span> <span data-ttu-id="e1321-109">V tématu hello [poznámky k verzi](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) hello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="e1321-109">See hello [release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) for hello latest version.</span></span>

## <a name="no-data"></a><span data-ttu-id="e1321-110">Žádná data</span><span class="sxs-lookup"><span data-stu-id="e1321-110">No data</span></span>
<span data-ttu-id="e1321-111">**I přidat Application Insights úspěšně a spustil mé aplikace, ale nikdy, uloží se data hello portálu.**</span><span class="sxs-lookup"><span data-stu-id="e1321-111">**I added Application Insights successfully and ran my app, but I've never seen data in hello portal.**</span></span>

* <span data-ttu-id="e1321-112">Počkejte několik minut a klikněte na tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="e1321-112">Wait a minute and click Refresh.</span></span> <span data-ttu-id="e1321-113">pravidelně Hello grafy sami obnovit, ale můžete taky aktualizovat ručně.</span><span class="sxs-lookup"><span data-stu-id="e1321-113">hello charts refresh themselves periodically, but you can also refresh manually.</span></span> <span data-ttu-id="e1321-114">interval obnovování Hello závisí na hello časové rozmezí hello grafu.</span><span class="sxs-lookup"><span data-stu-id="e1321-114">hello refresh interval depends on hello time range of hello chart.</span></span>
* <span data-ttu-id="e1321-115">Zkontrolujte, že máte klíč instrumentace, který je definován v souboru ApplicationInsights.xml hello (ve složce hello prostředky ve vašem projektu)</span><span class="sxs-lookup"><span data-stu-id="e1321-115">Check that you have an instrumentation key defined in hello ApplicationInsights.xml file (in hello resources folder in your project)</span></span>
* <span data-ttu-id="e1321-116">Ověřte, zda je žádné `<DisableTelemetry>true</DisableTelemetry>` uzlu v souboru xml hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-116">Verify that there is no `<DisableTelemetry>true</DisableTelemetry>` node in hello xml file.</span></span>
* <span data-ttu-id="e1321-117">V bráně firewall můžete mít tooopen porty TCP 80 a 443 pro odchozí provoz toodc.services.visualstudio.com. V tématu hello [úplný seznam výjimky brány firewall](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="e1321-117">In your firewall, you might have tooopen TCP ports 80 and 443 for outgoing traffic toodc.services.visualstudio.com. See hello [full list of firewall exceptions](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="e1321-118">V hello Microsoft Azure spustit Tabule, podívejte se na mapě stav služby hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-118">In hello Microsoft Azure start board, look at hello service status map.</span></span> <span data-ttu-id="e1321-119">Pokud jsou některé výstrahy indikace, počkejte, dokud se vrátili tooOK a pak zavřete a znovu otevřete okně vaší aplikace Application Insights.</span><span class="sxs-lookup"><span data-stu-id="e1321-119">If there are some alert indications, wait until they have returned tooOK and then close and re-open your Application Insights application blade.</span></span>
* <span data-ttu-id="e1321-120">Zapnout protokolování okna konzoly toohello IDE, tak, že přidáte `<SDKLogger />` prvek v rámci hello kořenového uzlu souboru ApplicationInsights.xml hello (ve složce hello prostředky ve vašem projektu) a zkontrolujte položky, kterými [Chyba].</span><span class="sxs-lookup"><span data-stu-id="e1321-120">Turn on logging toohello IDE console window, by adding an `<SDKLogger />` element under hello root node in hello ApplicationInsights.xml file (in hello resources folder in your project), and check for entries prefaced with [Error].</span></span>
* <span data-ttu-id="e1321-121">Ujistěte se, že hello správné souboru ApplicationInsights.xml byl úspěšně načten podle hello sady Java SDK prohlížením zprávy výstup hello konzoly pro příkaz "konfigurační soubor byl úspěšně nalezl".</span><span class="sxs-lookup"><span data-stu-id="e1321-121">Make sure that hello correct ApplicationInsights.xml file has been successfully loaded by hello Java SDK, by looking at hello console's output messages for a "Configuration file has been successfully found" statement.</span></span>
* <span data-ttu-id="e1321-122">Pokud není nalezen hello konfigurační soubor, zkontrolujte toosee zprávy výstup hello, kde má být vyhledán hello konfiguračního souboru pro a ujistěte se, že hello ApplicationInsights.xml se nachází v jedné z těchto umístění vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="e1321-122">If hello config file is not found, check hello output messages toosee where hello config file is being searched for, and make sure that hello ApplicationInsights.xml is located in one of those search locations.</span></span> <span data-ttu-id="e1321-123">Jako existuje pravidlo můžete umístit hello konfiguračního souboru téměř hello Application Insights SDK JARs.</span><span class="sxs-lookup"><span data-stu-id="e1321-123">As a rule of thumb, you can place hello config file near hello Application Insights SDK JARs.</span></span> <span data-ttu-id="e1321-124">Příklad: v Tomcat, to znamená hello složku WEB-INF/lib.</span><span class="sxs-lookup"><span data-stu-id="e1321-124">For example: in Tomcat, this would mean hello WEB-INF/lib folder.</span></span>

#### <a name="i-used-toosee-data-but-it-has-stopped"></a><span data-ttu-id="e1321-125">Mohu použít toosee data, ale byla zastavena</span><span class="sxs-lookup"><span data-stu-id="e1321-125">I used toosee data, but it has stopped</span></span>
* <span data-ttu-id="e1321-126">Zkontrolujte hello [stav blog](http://blogs.msdn.com/b/applicationinsights-status/).</span><span class="sxs-lookup"><span data-stu-id="e1321-126">Check hello [status blog](http://blogs.msdn.com/b/applicationinsights-status/).</span></span>
* <span data-ttu-id="e1321-127">Jste nedosáhli vaše měsíční kvóta datových bodů?</span><span class="sxs-lookup"><span data-stu-id="e1321-127">Have you hit your monthly quota of data points?</span></span> <span data-ttu-id="e1321-128">Otevřete nastavení nebo kvóty a cena toofind limitu. Pokud ano, můžete upgradovat plán nebo platit dodatečnou kapacitu.</span><span class="sxs-lookup"><span data-stu-id="e1321-128">Open Settings/Quota and Pricing toofind out. If so, you can upgrade your plan, or pay for additional capacity.</span></span> <span data-ttu-id="e1321-129">V tématu hello [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="e1321-129">See hello [pricing scheme](https://azure.microsoft.com/pricing/details/application-insights/).</span></span>

#### <a name="i-dont-see-all-hello-data-im-expecting"></a><span data-ttu-id="e1321-130">Nejsou zobrazeny všechny hello data, která se byla očekávána</span><span class="sxs-lookup"><span data-stu-id="e1321-130">I don't see all hello data I'm expecting</span></span>
* <span data-ttu-id="e1321-131">Otevřete hello kvóty a ceny okno a zkontrolujte, zda [vzorkování](app-insights-sampling.md) je v provozu.</span><span class="sxs-lookup"><span data-stu-id="e1321-131">Open hello Quotas and Pricing blade and check whether [sampling](app-insights-sampling.md) is in operation.</span></span> <span data-ttu-id="e1321-132">(přenos 100 % znamená, že vzorkování není v provozu.) Hello služby Application Insights může být sada tooaccept pouze část hello telemetrická data přenášená z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1321-132">(100% transmission means that sampling isn't in operation.) hello Application Insights service can be set tooaccept only a fraction of hello telemetry that arrives from your app.</span></span> <span data-ttu-id="e1321-133">To pomáhá při synchronizaci v rámci vaší měsíční kvóta telemetrie.</span><span class="sxs-lookup"><span data-stu-id="e1321-133">This helps you keep within your monthly quota of telemetry.</span></span> 

## <a name="no-usage-data"></a><span data-ttu-id="e1321-134">Žádná data o využití</span><span class="sxs-lookup"><span data-stu-id="e1321-134">No usage data</span></span>
<span data-ttu-id="e1321-135">**Zobrazuje, data o žádosti a doby odezvy, ale žádné zobrazení stránky, prohlížeč nebo uživatelská data.**</span><span class="sxs-lookup"><span data-stu-id="e1321-135">**I see data about requests and response times, but no page view, browser, or user data.**</span></span>

<span data-ttu-id="e1321-136">Byl úspěšně nainstalován se telemetrie aplikace toosend ze serveru hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-136">You successfully set up your app toosend telemetry from hello server.</span></span> <span data-ttu-id="e1321-137">Teď je dalším krokem je příliš[nastavení webové stránky toosend telemetrii z webového prohlížeče hello][usage].</span><span class="sxs-lookup"><span data-stu-id="e1321-137">Now your next step is too[set up your web pages toosend telemetry from hello web browser][usage].</span></span>

<span data-ttu-id="e1321-138">Případně pokud se váš klient je aplikace v [telefonu nebo jiné zařízení][platforms], mohla odesílat telemetrii z ní.</span><span class="sxs-lookup"><span data-stu-id="e1321-138">Alternatively, if your client is an app in a [phone or other device][platforms], you can send telemetry from there.</span></span> 

<span data-ttu-id="e1321-139">Použití hello stejné instrumentace klíče tooset si klient i server telemetrie.</span><span class="sxs-lookup"><span data-stu-id="e1321-139">Use hello same instrumentation key tooset up both your client and server telemetry.</span></span> <span data-ttu-id="e1321-140">Hello data se zobrazí v hello stejný prostředek Application Insights, a budete moct toocorrelate události z klienta a serveru.</span><span class="sxs-lookup"><span data-stu-id="e1321-140">hello data will appear in hello same Application Insights resource, and you'll be able toocorrelate events from client and server.</span></span>


## <a name="disabling-telemetry"></a><span data-ttu-id="e1321-141">Vypnutí telemetrie</span><span class="sxs-lookup"><span data-stu-id="e1321-141">Disabling telemetry</span></span>
<span data-ttu-id="e1321-142">**Jak můžete zakázat telemetrii kolekce?**</span><span class="sxs-lookup"><span data-stu-id="e1321-142">**How can I disable telemetry collection?**</span></span>

<span data-ttu-id="e1321-143">V kódu:</span><span class="sxs-lookup"><span data-stu-id="e1321-143">In code:</span></span>

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

<span data-ttu-id="e1321-144">**Nebo**</span><span class="sxs-lookup"><span data-stu-id="e1321-144">**Or**</span></span> 

<span data-ttu-id="e1321-145">Aktualizujte soubor ApplicationInsights.xml (ve složce hello prostředky ve vašem projektu).</span><span class="sxs-lookup"><span data-stu-id="e1321-145">Update ApplicationInsights.xml (in hello resources folder in your project).</span></span> <span data-ttu-id="e1321-146">Přidejte následující hello pod hello kořenového uzlu:</span><span class="sxs-lookup"><span data-stu-id="e1321-146">Add hello following under hello root node:</span></span>

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

<span data-ttu-id="e1321-147">Pomocí metody hello XML, když změníte hodnotu hello mít toorestart hello aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1321-147">Using hello XML method, you have toorestart hello application when you change hello value.</span></span>

## <a name="changing-hello-target"></a><span data-ttu-id="e1321-148">Změna cílové hello</span><span class="sxs-lookup"><span data-stu-id="e1321-148">Changing hello target</span></span>
<span data-ttu-id="e1321-149">**Jak můžete změnit který Azure prostředek projektu odešle data?**</span><span class="sxs-lookup"><span data-stu-id="e1321-149">**How can I change which Azure resource my project sends data to?**</span></span>

* <span data-ttu-id="e1321-150">[Získejte klíč instrumentace hello hello nový prostředek.][java]</span><span class="sxs-lookup"><span data-stu-id="e1321-150">[Get hello instrumentation key of hello new resource.][java]</span></span>
* <span data-ttu-id="e1321-151">Pokud jste přidali Application Insights tooyour projekt pomocí hello Azure Toolkit pro Eclipse, klikněte pravým tlačítkem na webový projekt, vyberte **Azure**, **konfigurovat Application Insights**a změňte hello klíč.</span><span class="sxs-lookup"><span data-stu-id="e1321-151">If you added Application Insights tooyour project using hello Azure Toolkit for Eclipse, right click your web project, select **Azure**, **Configure Application Insights**, and change hello key.</span></span>
* <span data-ttu-id="e1321-152">V opačném aktualizujte klíč hello v ApplicationInsights.xml do složky hello prostředky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="e1321-152">Otherwise, update hello key in ApplicationInsights.xml in hello resources folder in your project.</span></span>

## <a name="debug-data-from-hello-sdk"></a><span data-ttu-id="e1321-153">Ladění data z hello SDK</span><span class="sxs-lookup"><span data-stu-id="e1321-153">Debug data from hello SDK</span></span>

<span data-ttu-id="e1321-154">**Jak můžete zjistit, jaké hello provádí SDK?**</span><span class="sxs-lookup"><span data-stu-id="e1321-154">**How can I find out what hello SDK is doing?**</span></span>

<span data-ttu-id="e1321-155">Přidat další informace o co se děje v hello rozhraní API, tooget `<SDKLogger/>` pod hello kořenového uzlu souboru ApplicationInsights.xml konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="e1321-155">tooget more information about what's happening in hello API, add `<SDKLogger/>` under hello root node of hello ApplicationInsights.xml configuration file.</span></span>

<span data-ttu-id="e1321-156">Můžete také určit, aby hello protokolovacího nástroje toooutput tooa souboru:</span><span class="sxs-lookup"><span data-stu-id="e1321-156">You can also instruct hello logger toooutput tooa file:</span></span>

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

<span data-ttu-id="e1321-157">soubory Hello najdete v části `%temp%\javasdklogs` nebo `java.io.tmpdir` v případě Tomcat server.</span><span class="sxs-lookup"><span data-stu-id="e1321-157">hello files can be found under `%temp%\javasdklogs` or `java.io.tmpdir` in case of Tomcat server.</span></span>


## <a name="hello-azure-start-screen"></a><span data-ttu-id="e1321-158">Hello Azure úvodní obrazovce</span><span class="sxs-lookup"><span data-stu-id="e1321-158">hello Azure start screen</span></span>
<span data-ttu-id="e1321-159">**Dívám se na [hello portál Azure](https://portal.azure.com). Hello mapy chci se dozvědět něco o mé aplikaci?**</span><span class="sxs-lookup"><span data-stu-id="e1321-159">**I'm looking at [hello Azure portal](https://portal.azure.com). Does hello map tell me something about my app?**</span></span>

<span data-ttu-id="e1321-160">Ne, zobrazuje stav hello servery Azure kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="e1321-160">No, it shows hello health of Azure servers around hello world.</span></span>

<span data-ttu-id="e1321-161">*Z hello Azure počáteční Tabule (domovskou obrazovku) jak lze najít data o mé aplikaci?*</span><span class="sxs-lookup"><span data-stu-id="e1321-161">*From hello Azure start board (home screen), how do I find data about my app?*</span></span>

<span data-ttu-id="e1321-162">Za předpokladu, že jste [nastavit aplikaci pro službu Application Insights][java], klikněte na tlačítko Procházet, vyberte Application Insights a vyberte prostředek aplikace hello jste vytvořili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e1321-162">Assuming you [set up your app for Application Insights][java], click Browse, select Application Insights, and select hello app resource you created for your app.</span></span> <span data-ttu-id="e1321-163">tooget existuje rychlejší v budoucnosti budete moct připnout Tabule start toohello vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="e1321-163">tooget there faster in future, you can pin your app toohello start board.</span></span>

## <a name="intranet-servers"></a><span data-ttu-id="e1321-164">Intranetové servery</span><span class="sxs-lookup"><span data-stu-id="e1321-164">Intranet servers</span></span>
<span data-ttu-id="e1321-165">**Můžete sledovat server v mém intranetu?**</span><span class="sxs-lookup"><span data-stu-id="e1321-165">**Can I monitor a server on my intranet?**</span></span>

<span data-ttu-id="e1321-166">Ano, pokud váš server může odesílat telemetrii toohello Application Insights portál prostřednictvím hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="e1321-166">Yes, provided your server can send telemetry toohello Application Insights portal through hello public internet.</span></span> 

<span data-ttu-id="e1321-167">V bráně firewall můžete mít tooopen porty TCP 80 a 443 pro odchozí provoz toodc.services.visualstudio.com a f5.services.visualstudio.com.</span><span class="sxs-lookup"><span data-stu-id="e1321-167">In your firewall, you might have tooopen TCP ports 80 and 443 for outgoing traffic toodc.services.visualstudio.com and f5.services.visualstudio.com.</span></span>

## <a name="data-retention"></a><span data-ttu-id="e1321-168">Uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="e1321-168">Data retention</span></span>
<span data-ttu-id="e1321-169">**Jak dlouho se data uchovávají portálu hello? Je bezpečné?**</span><span class="sxs-lookup"><span data-stu-id="e1321-169">**How long is data retained in hello portal? Is it secure?**</span></span>

<span data-ttu-id="e1321-170">V tématu [uchovávání dat a ochrana osobních údajů][data].</span><span class="sxs-lookup"><span data-stu-id="e1321-170">See [Data retention and privacy][data].</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1321-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1321-171">Next steps</span></span>
<span data-ttu-id="e1321-172">**Mám nastavit Application Insights pro aplikace my server Java. Kde můžou dělat?**</span><span class="sxs-lookup"><span data-stu-id="e1321-172">**I set up Application Insights for my Java server app. What else can I do?**</span></span>

* <span data-ttu-id="e1321-173">[Monitorování dostupnosti webových stránek][availability]</span><span class="sxs-lookup"><span data-stu-id="e1321-173">[Monitor availability of your web pages][availability]</span></span>
* <span data-ttu-id="e1321-174">[Sledování využití webové stránky][usage]</span><span class="sxs-lookup"><span data-stu-id="e1321-174">[Monitor web page usage][usage]</span></span>
* <span data-ttu-id="e1321-175">[Sledování využití a diagnostikovat problémy ve svých aplikacích zařízení][platforms]</span><span class="sxs-lookup"><span data-stu-id="e1321-175">[Track usage and diagnose issues in your device apps][platforms]</span></span>
* <span data-ttu-id="e1321-176">[Zápis kódu tootrack využití vaší aplikace][track]</span><span class="sxs-lookup"><span data-stu-id="e1321-176">[Write code tootrack usage of your app][track]</span></span>
* <span data-ttu-id="e1321-177">[Zaznamenat diagnostických protokolů][javalogs]</span><span class="sxs-lookup"><span data-stu-id="e1321-177">[Capture diagnostic logs][javalogs]</span></span>

## <a name="get-help"></a><span data-ttu-id="e1321-178">Podpora</span><span class="sxs-lookup"><span data-stu-id="e1321-178">Get help</span></span>
* [<span data-ttu-id="e1321-179">Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="e1321-179">Stack Overflow</span></span>](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

