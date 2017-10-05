---
title: "Řešení potíží s Application Insights ve webovém projektu Java"
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
ms.openlocfilehash: ce46a4f561a273dc340b090a5bf0c8932a308722
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a><span data-ttu-id="ec2d9-103">Řešení potíží a otázky a odpovědi v nástroji Application Insights</span><span class="sxs-lookup"><span data-stu-id="ec2d9-103">Troubleshooting and Q and A for Application Insights for Java</span></span>
<span data-ttu-id="ec2d9-104">Dotazy nebo problémy s [Azure Application Insights v jazyce Java][java]?</span><span class="sxs-lookup"><span data-stu-id="ec2d9-104">Questions or problems with [Azure Application Insights in Java][java]?</span></span> <span data-ttu-id="ec2d9-105">Zde jsou některé tipy.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-105">Here are some tips.</span></span>

## <a name="build-errors"></a><span data-ttu-id="ec2d9-106">Chyby sestavení</span><span class="sxs-lookup"><span data-stu-id="ec2d9-106">Build errors</span></span>
<span data-ttu-id="ec2d9-107">**V prostředí Eclipse, při přidání Application Insights SDK prostřednictvím Maven nebo Gradle zobrazuje se chyby ověření sestavení nebo kontrolního součtu.**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-107">**In Eclipse, when adding the Application Insights SDK via Maven or Gradle, I get build or checksum validation errors.**</span></span>

* <span data-ttu-id="ec2d9-108">Pokud závislost <version> element je pomocí vzoru se zástupnými znaky (například (Maven) `<version>[1.0,)</version>` nebo (Gradle) `version:'1.0.+'`), zkuste místo toho zadat konkrétní verzi jako `1.0.2`.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-108">If the dependency <version> element is using a pattern with wildcard characters (e.g. (Maven) `<version>[1.0,)</version>` or (Gradle) `version:'1.0.+'`), try specifying a specific version instead like `1.0.2`.</span></span> <span data-ttu-id="ec2d9-109">Najdete v článku [poznámky k verzi](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) na nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-109">See the [release notes](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) for the latest version.</span></span>

## <a name="no-data"></a><span data-ttu-id="ec2d9-110">Žádná data</span><span class="sxs-lookup"><span data-stu-id="ec2d9-110">No data</span></span>
<span data-ttu-id="ec2d9-111">**I přidat Application Insights úspěšně a spustil mé aplikace, ale nikdy, uloží se data na portálu.**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-111">**I added Application Insights successfully and ran my app, but I've never seen data in the portal.**</span></span>

* <span data-ttu-id="ec2d9-112">Počkejte několik minut a klikněte na tlačítko Aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-112">Wait a minute and click Refresh.</span></span> <span data-ttu-id="ec2d9-113">Pravidelně grafy sami obnovit, ale můžete taky aktualizovat ručně.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-113">The charts refresh themselves periodically, but you can also refresh manually.</span></span> <span data-ttu-id="ec2d9-114">Interval aktualizace závisí na časové rozmezí grafu.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-114">The refresh interval depends on the time range of the chart.</span></span>
* <span data-ttu-id="ec2d9-115">Zkontrolujte, zda máte klíč instrumentace definované v souboru ApplicationInsights.xml (ve složce prostředky ve vašem projektu)</span><span class="sxs-lookup"><span data-stu-id="ec2d9-115">Check that you have an instrumentation key defined in the ApplicationInsights.xml file (in the resources folder in your project)</span></span>
* <span data-ttu-id="ec2d9-116">Ověřte, zda je žádné `<DisableTelemetry>true</DisableTelemetry>` uzlu v souboru xml.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-116">Verify that there is no `<DisableTelemetry>true</DisableTelemetry>` node in the xml file.</span></span>
* <span data-ttu-id="ec2d9-117">V bráně firewall můžete chtít otevřít porty TCP 80 a 443 pro odchozí přenosy na adresu dc.services.visualstudio.com. Najdete v článku [úplný seznam výjimky brány firewall](app-insights-ip-addresses.md)</span><span class="sxs-lookup"><span data-stu-id="ec2d9-117">In your firewall, you might have to open TCP ports 80 and 443 for outgoing traffic to dc.services.visualstudio.com. See the [full list of firewall exceptions](app-insights-ip-addresses.md)</span></span>
* <span data-ttu-id="ec2d9-118">Ve službě Microsoft Azure spustit Tabule, podívejte se na mapě služby stavu.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-118">In the Microsoft Azure start board, look at the service status map.</span></span> <span data-ttu-id="ec2d9-119">Pokud jsou některé výstrahy indikace, počkejte, dokud se změnil na OK a pak zavřete a znovu otevřete okně vaší aplikace Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-119">If there are some alert indications, wait until they have returned to OK and then close and re-open your Application Insights application blade.</span></span>
* <span data-ttu-id="ec2d9-120">Povolení protokolování v okně konzoly IDE přidáním `<SDKLogger />` prvek v rámci kořenového uzlu souboru ApplicationInsights.xml (ve složce prostředky ve vašem projektu) a zkontrolujte položky, kterými [Chyba].</span><span class="sxs-lookup"><span data-stu-id="ec2d9-120">Turn on logging to the IDE console window, by adding an `<SDKLogger />` element under the root node in the ApplicationInsights.xml file (in the resources folder in your project), and check for entries prefaced with [Error].</span></span>
* <span data-ttu-id="ec2d9-121">Ujistěte se, že správný soubor ApplicationInsights.xml byl úspěšně načten Java SDK prohlížením výstup zprávy v konzole pro příkaz "konfigurační soubor byl úspěšně nalezl".</span><span class="sxs-lookup"><span data-stu-id="ec2d9-121">Make sure that the correct ApplicationInsights.xml file has been successfully loaded by the Java SDK, by looking at the console's output messages for a "Configuration file has been successfully found" statement.</span></span>
* <span data-ttu-id="ec2d9-122">Pokud není nalezen konfigurační soubor, zkontrolujte zprávy výstup zobrazíte, kde má být vyhledán do konfiguračního souboru pro a ujistěte se, že soubor ApplicationInsights.xml nachází v jednom z těchto umístění vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-122">If the config file is not found, check the output messages to see where the config file is being searched for, and make sure that the ApplicationInsights.xml is located in one of those search locations.</span></span> <span data-ttu-id="ec2d9-123">Jako existuje pravidlo můžete umístit do konfiguračního souboru téměř Application Insights SDK JARs.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-123">As a rule of thumb, you can place the config file near the Application Insights SDK JARs.</span></span> <span data-ttu-id="ec2d9-124">Příklad: v Tomcat, to znamená složce webové-INF/lib.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-124">For example: in Tomcat, this would mean the WEB-INF/lib folder.</span></span>

#### <a name="i-used-to-see-data-but-it-has-stopped"></a><span data-ttu-id="ec2d9-125">Mohu použít chcete zobrazit data, ale byla zastavena</span><span class="sxs-lookup"><span data-stu-id="ec2d9-125">I used to see data, but it has stopped</span></span>
* <span data-ttu-id="ec2d9-126">Zkontrolujte [stav blog](http://blogs.msdn.com/b/applicationinsights-status/).</span><span class="sxs-lookup"><span data-stu-id="ec2d9-126">Check the [status blog](http://blogs.msdn.com/b/applicationinsights-status/).</span></span>
* <span data-ttu-id="ec2d9-127">Jste nedosáhli vaše měsíční kvóta datových bodů?</span><span class="sxs-lookup"><span data-stu-id="ec2d9-127">Have you hit your monthly quota of data points?</span></span> <span data-ttu-id="ec2d9-128">Otevřete nastavení nebo kvóty a cena chcete zjistit. Pokud ano, můžete upgradovat plán nebo platit dodatečnou kapacitu.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-128">Open Settings/Quota and Pricing to find out. If so, you can upgrade your plan, or pay for additional capacity.</span></span> <span data-ttu-id="ec2d9-129">Najdete v článku [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="ec2d9-129">See the [pricing scheme](https://azure.microsoft.com/pricing/details/application-insights/).</span></span>

#### <a name="i-dont-see-all-the-data-im-expecting"></a><span data-ttu-id="ec2d9-130">Všechna data, která se byla očekávána se nezobrazí</span><span class="sxs-lookup"><span data-stu-id="ec2d9-130">I don't see all the data I'm expecting</span></span>
* <span data-ttu-id="ec2d9-131">Otevřete kvóty a ceny okno a zkontrolujte, zda [vzorkování](app-insights-sampling.md) je v provozu.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-131">Open the Quotas and Pricing blade and check whether [sampling](app-insights-sampling.md) is in operation.</span></span> <span data-ttu-id="ec2d9-132">(přenos 100 % znamená, že vzorkování není v provozu.) Službu Application Insights můžete nastavit tak, aby přijímal pouze část telemetrická data přenášená z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-132">(100% transmission means that sampling isn't in operation.) The Application Insights service can be set to accept only a fraction of the telemetry that arrives from your app.</span></span> <span data-ttu-id="ec2d9-133">To pomáhá při synchronizaci v rámci vaší měsíční kvóta telemetrie.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-133">This helps you keep within your monthly quota of telemetry.</span></span> 

## <a name="no-usage-data"></a><span data-ttu-id="ec2d9-134">Žádná data o využití</span><span class="sxs-lookup"><span data-stu-id="ec2d9-134">No usage data</span></span>
<span data-ttu-id="ec2d9-135">**Zobrazuje, data o žádosti a doby odezvy, ale žádné zobrazení stránky, prohlížeč nebo uživatelská data.**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-135">**I see data about requests and response times, but no page view, browser, or user data.**</span></span>

<span data-ttu-id="ec2d9-136">Jste úspěšně nastavili aplikace k odesílání telemetrie ze serveru.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-136">You successfully set up your app to send telemetry from the server.</span></span> <span data-ttu-id="ec2d9-137">Teď je dalším krokem je [nastavení webové stránky k odesílání telemetrie z webového prohlížeče][usage].</span><span class="sxs-lookup"><span data-stu-id="ec2d9-137">Now your next step is to [set up your web pages to send telemetry from the web browser][usage].</span></span>

<span data-ttu-id="ec2d9-138">Případně pokud se váš klient je aplikace v [telefonu nebo jiné zařízení][platforms], mohla odesílat telemetrii z ní.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-138">Alternatively, if your client is an app in a [phone or other device][platforms], you can send telemetry from there.</span></span> 

<span data-ttu-id="ec2d9-139">Použijte stejný klíč instrumentace nastavit telemetrie klient i server.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-139">Use the same instrumentation key to set up both your client and server telemetry.</span></span> <span data-ttu-id="ec2d9-140">Data se zobrazí v rámci stejné prostředku Application Insights a budete moct korelovat události z klienta a serveru.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-140">The data will appear in the same Application Insights resource, and you'll be able to correlate events from client and server.</span></span>


## <a name="disabling-telemetry"></a><span data-ttu-id="ec2d9-141">Vypnutí telemetrie</span><span class="sxs-lookup"><span data-stu-id="ec2d9-141">Disabling telemetry</span></span>
<span data-ttu-id="ec2d9-142">**Jak můžete zakázat telemetrii kolekce?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-142">**How can I disable telemetry collection?**</span></span>

<span data-ttu-id="ec2d9-143">V kódu:</span><span class="sxs-lookup"><span data-stu-id="ec2d9-143">In code:</span></span>

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

<span data-ttu-id="ec2d9-144">**Nebo**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-144">**Or**</span></span> 

<span data-ttu-id="ec2d9-145">Aktualizujte soubor ApplicationInsights.xml (ve složce prostředky ve vašem projektu).</span><span class="sxs-lookup"><span data-stu-id="ec2d9-145">Update ApplicationInsights.xml (in the resources folder in your project).</span></span> <span data-ttu-id="ec2d9-146">Přidejte následující do kořenového uzlu:</span><span class="sxs-lookup"><span data-stu-id="ec2d9-146">Add the following under the root node:</span></span>

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

<span data-ttu-id="ec2d9-147">Pomocí metody XML, musíte aplikaci restartovat při změně hodnota.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-147">Using the XML method, you have to restart the application when you change the value.</span></span>

## <a name="changing-the-target"></a><span data-ttu-id="ec2d9-148">Změna cíle</span><span class="sxs-lookup"><span data-stu-id="ec2d9-148">Changing the target</span></span>
<span data-ttu-id="ec2d9-149">**Jak můžete změnit který Azure prostředek projektu odešle data?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-149">**How can I change which Azure resource my project sends data to?**</span></span>

* <span data-ttu-id="ec2d9-150">[Získejte klíč instrumentace nového prostředku.][java]</span><span class="sxs-lookup"><span data-stu-id="ec2d9-150">[Get the instrumentation key of the new resource.][java]</span></span>
* <span data-ttu-id="ec2d9-151">Pokud jste přidali Application Insights do projektu pomocí sady nástrojů pro Azure pro prostředí Eclipse, klikněte pravým tlačítkem na webový projekt, vyberte **Azure**, **konfigurovat Application Insights**a změňte klíč.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-151">If you added Application Insights to your project using the Azure Toolkit for Eclipse, right click your web project, select **Azure**, **Configure Application Insights**, and change the key.</span></span>
* <span data-ttu-id="ec2d9-152">V opačném aktualizujte klíč v ApplicationInsights.xml ve složce prostředky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-152">Otherwise, update the key in ApplicationInsights.xml in the resources folder in your project.</span></span>

## <a name="debug-data-from-the-sdk"></a><span data-ttu-id="ec2d9-153">Ladění data ze sady SDK</span><span class="sxs-lookup"><span data-stu-id="ec2d9-153">Debug data from the SDK</span></span>

<span data-ttu-id="ec2d9-154">**Jak můžete zjistit, co je to sada SDK?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-154">**How can I find out what the SDK is doing?**</span></span>

<span data-ttu-id="ec2d9-155">Chcete-li získat další informace o co se děje v rozhraní API, přidejte `<SDKLogger/>` do kořenového uzlu souboru ApplicationInsights.xml konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-155">To get more information about what's happening in the API, add `<SDKLogger/>` under the root node of the ApplicationInsights.xml configuration file.</span></span>

<span data-ttu-id="ec2d9-156">Můžete také určit, aby protokoly pro výstup do souboru:</span><span class="sxs-lookup"><span data-stu-id="ec2d9-156">You can also instruct the logger to output to a file:</span></span>

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

<span data-ttu-id="ec2d9-157">Soubory lze nalézt v `%temp%\javasdklogs` nebo `java.io.tmpdir` v případě Tomcat server.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-157">The files can be found under `%temp%\javasdklogs` or `java.io.tmpdir` in case of Tomcat server.</span></span>


## <a name="the-azure-start-screen"></a><span data-ttu-id="ec2d9-158">Na obrazovce Azure start</span><span class="sxs-lookup"><span data-stu-id="ec2d9-158">The Azure start screen</span></span>
<span data-ttu-id="ec2d9-159">**Dívám se na [portálu Azure](https://portal.azure.com). Mapy chci se dozvědět něco o mé aplikaci?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-159">**I'm looking at [the Azure portal](https://portal.azure.com). Does the map tell me something about my app?**</span></span>

<span data-ttu-id="ec2d9-160">Ne, zobrazuje stav servery Azure po celém světě.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-160">No, it shows the health of Azure servers around the world.</span></span>

<span data-ttu-id="ec2d9-161">*Z Azure počáteční Tabule (domovskou obrazovku) jak lze najít data o mé aplikaci?*</span><span class="sxs-lookup"><span data-stu-id="ec2d9-161">*From the Azure start board (home screen), how do I find data about my app?*</span></span>

<span data-ttu-id="ec2d9-162">Za předpokladu, že jste [nastavit aplikaci pro službu Application Insights][java], klikněte na tlačítko Procházet, vyberte Application Insights a vyberte prostředek aplikace, který jste vytvořili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-162">Assuming you [set up your app for Application Insights][java], click Browse, select Application Insights, and select the app resource you created for your app.</span></span> <span data-ttu-id="ec2d9-163">Chcete-li získat existuje rychlejší v budoucnosti budete moct připnout aplikaci Tabule start.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-163">To get there faster in future, you can pin your app to the start board.</span></span>

## <a name="intranet-servers"></a><span data-ttu-id="ec2d9-164">Intranetové servery</span><span class="sxs-lookup"><span data-stu-id="ec2d9-164">Intranet servers</span></span>
<span data-ttu-id="ec2d9-165">**Můžete sledovat server v mém intranetu?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-165">**Can I monitor a server on my intranet?**</span></span>

<span data-ttu-id="ec2d9-166">Ano, pokud že váš server mohla odesílat telemetrii do portálu služby Application Insights prostřednictvím veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-166">Yes, provided your server can send telemetry to the Application Insights portal through the public internet.</span></span> 

<span data-ttu-id="ec2d9-167">V bráně firewall můžete chtít otevřít porty TCP 80 a 443 pro odchozí přenosy na adresu dc.services.visualstudio.com a f5.services.visualstudio.com.</span><span class="sxs-lookup"><span data-stu-id="ec2d9-167">In your firewall, you might have to open TCP ports 80 and 443 for outgoing traffic to dc.services.visualstudio.com and f5.services.visualstudio.com.</span></span>

## <a name="data-retention"></a><span data-ttu-id="ec2d9-168">Uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="ec2d9-168">Data retention</span></span>
<span data-ttu-id="ec2d9-169">**Jak dlouho se data uchovávají v portálu? Je bezpečné?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-169">**How long is data retained in the portal? Is it secure?**</span></span>

<span data-ttu-id="ec2d9-170">V tématu [uchovávání dat a ochrana osobních údajů][data].</span><span class="sxs-lookup"><span data-stu-id="ec2d9-170">See [Data retention and privacy][data].</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec2d9-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec2d9-171">Next steps</span></span>
<span data-ttu-id="ec2d9-172">**Mám nastavit Application Insights pro aplikace my server Java. Kde můžou dělat?**</span><span class="sxs-lookup"><span data-stu-id="ec2d9-172">**I set up Application Insights for my Java server app. What else can I do?**</span></span>

* <span data-ttu-id="ec2d9-173">[Monitorování dostupnosti webových stránek][availability]</span><span class="sxs-lookup"><span data-stu-id="ec2d9-173">[Monitor availability of your web pages][availability]</span></span>
* <span data-ttu-id="ec2d9-174">[Sledování využití webové stránky][usage]</span><span class="sxs-lookup"><span data-stu-id="ec2d9-174">[Monitor web page usage][usage]</span></span>
* <span data-ttu-id="ec2d9-175">[Sledování využití a diagnostikovat problémy ve svých aplikacích zařízení][platforms]</span><span class="sxs-lookup"><span data-stu-id="ec2d9-175">[Track usage and diagnose issues in your device apps][platforms]</span></span>
* <span data-ttu-id="ec2d9-176">[Zápis kódu pro sledování využití vaší aplikace][track]</span><span class="sxs-lookup"><span data-stu-id="ec2d9-176">[Write code to track usage of your app][track]</span></span>
* <span data-ttu-id="ec2d9-177">[Zaznamenat diagnostických protokolů][javalogs]</span><span class="sxs-lookup"><span data-stu-id="ec2d9-177">[Capture diagnostic logs][javalogs]</span></span>

## <a name="get-help"></a><span data-ttu-id="ec2d9-178">Podpora</span><span class="sxs-lookup"><span data-stu-id="ec2d9-178">Get help</span></span>
* [<span data-ttu-id="ec2d9-179">Stack Overflow</span><span class="sxs-lookup"><span data-stu-id="ec2d9-179">Stack Overflow</span></span>](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

