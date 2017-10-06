---
title: "aaaMonitor dostupnosti a odezvy každého webový server | Microsoft Docs"
description: "Nastavení testů webu ve službě Application Insights. Zasílání upozornění, pokud web přestane být k dispozici nebo reaguje pomalu."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: bwren
ms.openlocfilehash: 4c5425c948770cc57a648ca50e217c75ac75fbd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-availability-and-responsiveness-of-any-web-site"></a><span data-ttu-id="9d8fb-104">Sledování dostupnosti a odezvy libovolných webů</span><span class="sxs-lookup"><span data-stu-id="9d8fb-104">Monitor availability and responsiveness of any web site</span></span>
<span data-ttu-id="9d8fb-105">Poté, co nasadíte webovou aplikaci nebo webové stránky tooany serveru, můžete nastavit toomonitor testy dostupnosti a odezvy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-105">After you've deployed your web app or web site tooany server, you can set up tests toomonitor its availability and responsiveness.</span></span> <span data-ttu-id="9d8fb-106">[Azure Application Insights](app-insights-overview.md) odesílá webové žádosti tooyour aplikace v pravidelných intervalech z bodů po hello, world.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-106">[Azure Application Insights](app-insights-overview.md) sends web requests tooyour application at regular intervals from points around hello world.</span></span> <span data-ttu-id="9d8fb-107">Upozorní vás v případě, že vaše aplikace reaguje pomalu nebo nereaguje vůbec.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-107">It alerts you if your application doesn't respond, or responds slowly.</span></span>

<span data-ttu-id="9d8fb-108">Můžete nastavit testy dostupnosti pro jakékoli HTTP nebo HTTPS koncový bod, který je přístupný z hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-108">You can set up availability tests for any HTTP or HTTPS endpoint that is accessible from hello public internet.</span></span> <span data-ttu-id="9d8fb-109">Nemáte tooadd cokoli toohello webové stránky, které testujete.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-109">You don't have tooadd anything toohello web site you're testing.</span></span> <span data-ttu-id="9d8fb-110">I nemá toobe váš web: může testovat službu REST API, na kterém závisí.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-110">It doesn't even have toobe your site: you could test a REST API service on which you depend.</span></span>

<span data-ttu-id="9d8fb-111">Existují dva typy testů dostupnosti:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-111">There are two types of availability tests:</span></span>

* <span data-ttu-id="9d8fb-112">[Testování ping adresy URL](#create): jednoduchý test, který můžete vytvořit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-112">[URL ping test](#create): a simple test that you can create in hello Azure portal.</span></span>
* <span data-ttu-id="9d8fb-113">[Vícekrokový test webu](#multi-step-web-tests): které vytvoříte v Visual Studio Enterprise a nahrávání toohello portálu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-113">[Multi-step web test](#multi-step-web-tests): which you create in Visual Studio Enterprise and upload toohello portal.</span></span>

<span data-ttu-id="9d8fb-114">Můžete vytvořit až too25 testy dostupnosti na prostředek aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-114">You can create up too25 availability tests per application resource.</span></span>

## <span data-ttu-id="9d8fb-115"><a name="create"></a>1. Otevření prostředku pro sestavy testů dostupnosti</span><span class="sxs-lookup"><span data-stu-id="9d8fb-115"><a name="create"></a>1. Open a resource for your availability test reports</span></span>

<span data-ttu-id="9d8fb-116">**Pokud jste již nakonfigurovali Application Insights** pro webovou aplikaci, otevřete jeho prostředek Application Insights v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-116">**If you have already configured Application Insights** for your web app, open its Application Insights resource in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="9d8fb-117">**Nebo, pokud chcete toosee si sestavy na nový prostředek,** zaregistrovat příliš[Microsoft Azure](http://azure.com), přejděte toohello [portál Azure](https://portal.azure.com)a vytvořte prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-117">**Or, if you want toosee your reports in a new resource,** sign up too[Microsoft Azure](http://azure.com), go toohello [Azure portal](https://portal.azure.com), and create an Application Insights resource.</span></span>

![Nový > Application Insights](./media/app-insights-monitor-web-app-availability/11-new-app.png)

<span data-ttu-id="9d8fb-119">Klikněte na tlačítko **všechny prostředky** okno Přehled hello tooopen pro nový prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-119">Click **All resources** tooopen hello Overview blade for hello new resource.</span></span>

## <span data-ttu-id="9d8fb-120"><a name="setup"></a>2. Vytvoření testu adresy URL pomocí příkazu Ping</span><span class="sxs-lookup"><span data-stu-id="9d8fb-120"><a name="setup"></a>2. Create a URL ping test</span></span>
<span data-ttu-id="9d8fb-121">Otevřete okno hello dostupnosti a přidat test.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-121">Open hello Availability blade and add a test.</span></span>

![Výplně alespoň hello adresa URL vašeho webu](./media/app-insights-monitor-web-app-availability/13-availability.png)

* <span data-ttu-id="9d8fb-123">**Adresa URL Hello** může být žádné webové stránce chcete tootest, ale musí být viditelná z hello veřejného Internetu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-123">**hello URL** can be any web page you want tootest, but it must be visible from hello public internet.</span></span> <span data-ttu-id="9d8fb-124">Adresa URL Hello může obsahovat řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-124">hello URL can include a query string.</span></span> <span data-ttu-id="9d8fb-125">To znamená, že můžete také trochu vyzkoušet svou databázi.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-125">So, for example, you can exercise your database a little.</span></span> <span data-ttu-id="9d8fb-126">Pokud adresa URL hello přeloží tooa přesměrování, budeme následnou akci too10 přesměrování.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-126">If hello URL resolves tooa redirect, we follow it up too10 redirects.</span></span>
* <span data-ttu-id="9d8fb-127">**Analyzovat závislé požadavky**: Pokud zaškrtnete toto políčko, testovací hello požadavky bitové kopie, skripty, soubory stylu a další soubory, které jsou součástí hello webové stránky v rámci testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-127">**Parse dependent requests**: If this option is checked, hello test requests images, scripts, style files, and other files that are part of hello web page under test.</span></span> <span data-ttu-id="9d8fb-128">Hello doba zaznamenané odezvy zahrnuje hello doba tooget tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-128">hello recorded response time includes hello time taken tooget these files.</span></span> <span data-ttu-id="9d8fb-129">Hello test se nezdaří, pokud tyto prostředky nelze úspěšně stáhnout v časovém limitu hello pro celý test hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-129">hello test fails if all these resources cannot be successfully downloaded within hello timeout for hello whole test.</span></span> 

    <span data-ttu-id="9d8fb-130">Pokud není políčko hello zaškrtnuté, testovací hello pouze požádá o soubor hello na adrese URL hello jste zadali.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-130">If hello option is not checked, hello test only requests hello file at hello URL you specified.</span></span>
* <span data-ttu-id="9d8fb-131">**Povolit opakování**: Pokud zaškrtnete toto políčko, pokud hello test se nezdaří, zopakuje se za krátkou dobu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-131">**Enable retries**:  If this option is checked, when hello test fails, it is retried after a short interval.</span></span> <span data-ttu-id="9d8fb-132">Selhání je nahlášeno pouze v případě tří po sobě jdoucích neúspěšných pokusů.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-132">A failure is reported only if three successive attempts fail.</span></span> <span data-ttu-id="9d8fb-133">Následné testy jsou pak provedeny frekvencí testu obvyklým hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-133">Subsequent tests are then performed at hello usual test frequency.</span></span> <span data-ttu-id="9d8fb-134">Opakování je dočasně pozastaveno do dalšího úspěchu hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-134">Retry is temporarily suspended until hello next success.</span></span> <span data-ttu-id="9d8fb-135">Toto pravidlo platí nezávisle na každém umístění testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-135">This rule is applied independently at each test location.</span></span> <span data-ttu-id="9d8fb-136">Doporučujeme tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-136">We recommend this option.</span></span> <span data-ttu-id="9d8fb-137">V průměru přibližně 80 % selhání při opakování zmizí.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-137">On average, about 80% of failures disappear on retry.</span></span>
* <span data-ttu-id="9d8fb-138">**Frekvence testů**: Nastaví, jak často hello test se spouští z umístění každého testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-138">**Test frequency**: Sets how often hello test is run from each test location.</span></span> <span data-ttu-id="9d8fb-139">S pětiminutovou četností a pěti testovanými místy bude váš web testován v průměru každou minutu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-139">With a frequency of five minutes and five test locations, your site is tested on average every minute.</span></span>
* <span data-ttu-id="9d8fb-140">**Testovací umístění** jsou hello umístí z kterých naše servery odesílají webové požadavky tooyour adresy URL.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-140">**Test locations** are hello places from where our servers send web requests tooyour URL.</span></span> <span data-ttu-id="9d8fb-141">Zvolte více než jeden, aby bylo možné rozlišit problémy ve vašem webu od problémů se sítí.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-141">Choose more than one so that you can distinguish problems in your website from network issues.</span></span> <span data-ttu-id="9d8fb-142">Můžete vybrat až too16 umístění.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-142">You can select up too16 locations.</span></span>
* <span data-ttu-id="9d8fb-143">**Kritéria úspěchu**:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-143">**Success criteria**:</span></span>

    <span data-ttu-id="9d8fb-144">**Časový limit testu**: snížit tuto hodnotu toobe upozornění pomalé odezvy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-144">**Test timeout**: Decrease this value toobe alerted about slow responses.</span></span> <span data-ttu-id="9d8fb-145">Hello test se počítá jako selhání, pokud během tohoto období nebyly přijaty odpovědí hello z webu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-145">hello test is counted as a failure if hello responses from your site have not been received within this period.</span></span> <span data-ttu-id="9d8fb-146">Pokud jste vybrali **analyzovat závislé požadavky**, pak všechny hello bitové kopie, soubory stylů, skripty a další závislé prostředky musí být přijaty během tohoto období.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-146">If you selected **Parse dependent requests**, then all hello images, style files, scripts, and other dependent resources must have been received within this period.</span></span>

    <span data-ttu-id="9d8fb-147">**Odpověď HTTP**: hello vrátil stavový kód, který se počítá jako úspěšný.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-147">**HTTP response**: hello returned status code that is counted as a success.</span></span> <span data-ttu-id="9d8fb-148">200 je hello kód, který označuje, že byla vrácena normální webová stránka.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-148">200 is hello code that indicates that a normal web page has been returned.</span></span>

    <span data-ttu-id="9d8fb-149">**Shoda obsahu**: řetězec, například „Vítejte!“</span><span class="sxs-lookup"><span data-stu-id="9d8fb-149">**Content match**: a string, like "Welcome!"</span></span> <span data-ttu-id="9d8fb-150">U každé odpovědi testujeme výskyt přesné shody (s rozlišováním velkých a malých písmen).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-150">We test that an exact case-sensitive match occurs in every response.</span></span> <span data-ttu-id="9d8fb-151">Musí být prostý řetězec bez zástupných znaků.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-151">It must be a plain string, without wildcards.</span></span> <span data-ttu-id="9d8fb-152">Nezapomeňte, že pokud obsah vaší stránka změní může mít tooupdate ho.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-152">Don't forget that if your page content changes you might have tooupdate it.</span></span>
* <span data-ttu-id="9d8fb-153">**Výstrahy** , ve výchozím nastavení, odešlou tooyou při selhání ve třech umístěních více než pět minut.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-153">**Alerts** are, by default, sent tooyou if there are failures in three locations over five minutes.</span></span> <span data-ttu-id="9d8fb-154">Selhání v jednom umístění je pravděpodobně toobe k potížím se sítí a nejedná se o problém s webem.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-154">A failure in one location is likely toobe a network problem, and not a problem with your site.</span></span> <span data-ttu-id="9d8fb-155">Ale můžete změnit hello prahová hodnota toobe více nebo méně velká a malá písmena a můžete také změnit, kdo hello e-mailů by měly být odeslány na.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-155">But you can change hello threshold toobe more or less sensitive, and you can also change who hello emails should be sent to.</span></span>

    <span data-ttu-id="9d8fb-156">Můžete nastavit [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán, když je vydána výstraha.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-156">You can set up a [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) that is called when an alert is raised.</span></span> <span data-ttu-id="9d8fb-157">(Všimněte si, že v současné době parametry dotazu neprocházejí jako vlastnosti.)</span><span class="sxs-lookup"><span data-stu-id="9d8fb-157">(But note that, at present, query parameters are not passed through as Properties.)</span></span>

### <a name="test-more-urls"></a><span data-ttu-id="9d8fb-158">Testování více adres URL</span><span class="sxs-lookup"><span data-stu-id="9d8fb-158">Test more URLs</span></span>
<span data-ttu-id="9d8fb-159">Přidat další testy</span><span class="sxs-lookup"><span data-stu-id="9d8fb-159">Add more tests.</span></span> <span data-ttu-id="9d8fb-160">Například v přidání tootesting domovské stránky, jste měli jistotu, že vaše databáze se spouští otestováním adresy URL hello hledání.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-160">For example, In addition tootesting your home page, you can make sure your database is running by testing hello URL for a search.</span></span>


## <span data-ttu-id="9d8fb-161"><a name="monitor"></a>3. Zobrazení výsledků testu dostupnosti</span><span class="sxs-lookup"><span data-stu-id="9d8fb-161"><a name="monitor"></a>3. See your availability test results</span></span>

<span data-ttu-id="9d8fb-162">Po několika minutách, klikněte na tlačítko **aktualizovat** toosee výsledky testů.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-162">After a few minutes, click **Refresh** toosee test results.</span></span> 

![Souhrnné výsledky na domácím okně hello](./media/app-insights-monitor-web-app-availability/14-availSummary-3.png)

<span data-ttu-id="9d8fb-164">Hello scatterplot jsou uvedena vzorová hello výsledky testů, které obsahují podrobné diagnostické test krok.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-164">hello scatterplot shows samples of hello test results that have diagnostic test-step detail in them.</span></span> <span data-ttu-id="9d8fb-165">Modul testu Hello ukládá diagnostické podrobnosti pro testy, s chybami.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-165">hello test engine stores diagnostic detail for tests that have failures.</span></span> <span data-ttu-id="9d8fb-166">Pro úspěšné testy diagnostické informace jsou uloženy pro podmnožinu spuštěních hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-166">For successful tests, diagnostic details are stored for a subset of hello executions.</span></span> <span data-ttu-id="9d8fb-167">Podržte ukazatel nad žádné hello zelená nebo červené tečky toosee hello testovací časové razítko, doba trvání testu, umístění a název testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-167">Hover over any of hello green/red dots toosee hello test timestamp, test duration, location, and test name.</span></span> <span data-ttu-id="9d8fb-168">Proklikejte se prostřednictvím jakékoli tečky v hello bodové vykreslení toosee hello podrobnosti výsledků testů hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-168">Click through any dot in hello scatter plot toosee hello details of hello test result.</span></span>  

<span data-ttu-id="9d8fb-169">Vyberte konkrétní test, umístění, nebo snižte dobu hello období toosee více výsledků kolem hello časové období, které vás zajímají.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-169">Select a particular test, location, or reduce hello time period toosee more results around hello time period of interest.</span></span> <span data-ttu-id="9d8fb-170">Použijte Průzkumník služby Search toosee výsledky z všechny spuštěních, nebo použijte vlastní sestavy toorun Analytics dotazy na tato data.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-170">Use Search Explorer toosee results from all executions, or use Analytics queries toorun custom reports on this data.</span></span>

<span data-ttu-id="9d8fb-171">Přidání toohello nezpracovaná výsledků existují dvě dostupnosti metriky v Průzkumníku metrik:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-171">In addition toohello raw results, there are two Availability metrics in Metrics Explorer:</span></span> 

1. <span data-ttu-id="9d8fb-172">Dostupnosti: Procento hello testů, které byly úspěšné, mezi jednotlivými spuštěními všechny testovací.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-172">Availability: Percentage of hello tests that were successful, across all test executions.</span></span> 
2. <span data-ttu-id="9d8fb-173">Doba trvání testu: průměrná doba trvání u všech provedení testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-173">Test Duration: Average test duration across all test executions.</span></span>

<span data-ttu-id="9d8fb-174">Filtry můžete použít na hello testovací název, umístění tooanalyze trendy v konkrétní test nebo umístění.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-174">You can apply filters on hello test name, location tooanalyze trends of a particular test and/or location.</span></span>

## <span data-ttu-id="9d8fb-175"><a name="edit"></a> Kontrola a úprava testů</span><span class="sxs-lookup"><span data-stu-id="9d8fb-175"><a name="edit"></a> Inspect and edit tests</span></span>

<span data-ttu-id="9d8fb-176">Souhrnná stránka hello vyberte konkrétní test.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-176">From hello summary page, select a specific test.</span></span> <span data-ttu-id="9d8fb-177">Zde můžete zobrazit jeho konkrétní výsledky a upravit ho nebo dočasně zakázat.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-177">There, you can see its specific results, and edit or temporarily disable it.</span></span>

![Upravit nebo zakázat webový test](./media/app-insights-monitor-web-app-availability/19-availEdit-3.png)

<span data-ttu-id="9d8fb-179">Můžete chtít testy dostupnosti toodisable nebo hello výstrahy pravidla s nimi spojených během provádění údržby vaší služby.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-179">You might want toodisable availability tests or hello alert rules associated with them while you are performing maintenance on your service.</span></span> 

## <span data-ttu-id="9d8fb-180"><a name="failures"></a>Pokud se zobrazí chyby</span><span class="sxs-lookup"><span data-stu-id="9d8fb-180"><a name="failures"></a>If you see failures</span></span>
<span data-ttu-id="9d8fb-181">Klikněte na červenou tečku.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-181">Click a red dot.</span></span>

![Klikněte na červenou tečku](./media/app-insights-monitor-web-app-availability/open-instance-3.png)


<span data-ttu-id="9d8fb-183">Výsledek testu dostupnosti umožňuje:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-183">From an availability test result, you can:</span></span>

* <span data-ttu-id="9d8fb-184">Zkontrolujte, zda text hello odpověď ze serveru.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-184">Inspect hello response received from your server.</span></span>
* <span data-ttu-id="9d8fb-185">Otevřete hello telemetrické zprávy odesílané aplikace serveru při zpracování instance hello chybných požadavků.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-185">Open hello telemetry sent by your server app while processing hello failed request instance.</span></span>
* <span data-ttu-id="9d8fb-186">Přihlaste se problému nebo pracovních položek v Git nebo služby VSTS tootrack hello problému.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-186">Log an issue or work item in Git or VSTS tootrack hello problem.</span></span> <span data-ttu-id="9d8fb-187">Hello chyb bude obsahovat událost toothis vazba.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-187">hello bug will contain a link toothis event.</span></span>
* <span data-ttu-id="9d8fb-188">Výsledek testu webové hello otevřete v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-188">Open hello web test result in Visual Studio.</span></span>


<span data-ttu-id="9d8fb-189">*Zdá se, že všechno je v pořádku, ale přesto je hlášena chyba.*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-189">*Looks OK but reported as a failure?*</span></span> <span data-ttu-id="9d8fb-190">Zkontrolujte všechny bitové kopie hello, skripty, šablony stylů a všechny další soubory, které jsou načteny stránku hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-190">Check all hello images, scripts, style sheets, and any other files loaded by hello page.</span></span> <span data-ttu-id="9d8fb-191">Pokud některý z nich selže, hello test uvádět jako neúspěšný i v případě hello hlavní html stránka načte bez problémů.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-191">If any of them fails, hello test is reported as failed, even if hello main html page loads OK.</span></span>

<span data-ttu-id="9d8fb-192">*Žádné související položky?*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-192">*No related items?*</span></span> <span data-ttu-id="9d8fb-193">Pokud máte pro aplikaci na straně serveru nastavenou službu Application Insights, může být důvodem to, že právě probíhá [vzorkování](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-193">If you have Application Insights set up for your server-side application, that may be because [sampling](app-insights-sampling.md) is in operation.</span></span> 

## <a name="multi-step-web-tests"></a><span data-ttu-id="9d8fb-194">Vícekrokové webové testy</span><span class="sxs-lookup"><span data-stu-id="9d8fb-194">Multi-step web tests</span></span>
<span data-ttu-id="9d8fb-195">Je možné sledovat scénář, který zahrnuje posloupnost adres URL.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-195">You can monitor a scenario that involves a sequence of URLs.</span></span> <span data-ttu-id="9d8fb-196">Například pokud sledujete Prodejní web, můžete otestovat, přidání položky toohello nákupní košík funguje správně.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-196">For example, if you are monitoring a sales website, you can test that adding items toohello shopping cart works correctly.</span></span>

> [!NOTE] 
> <span data-ttu-id="9d8fb-197">Vícekrokové webové testy jsou zpoplatněné.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-197">There is a charge for multi-step web tests.</span></span> <span data-ttu-id="9d8fb-198">[Cenové schéma](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-198">[Pricing scheme](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>
> 

<span data-ttu-id="9d8fb-199">toocreate vícekrokový test zaznamenání hello scénář pomocí Visual Studio Enterprise a pak nahrajte hello zaznamenávání tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-199">toocreate a multi-step test, you record hello scenario by using Visual Studio Enterprise, and then upload hello recording tooApplication Insights.</span></span> <span data-ttu-id="9d8fb-200">Application Insights opětovná přehrání hello scénář v intervalech a ověřuje hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-200">Application Insights replays hello scenario at intervals and verifies hello responses.</span></span>

> [!NOTE]
> <span data-ttu-id="9d8fb-201">V testech nelze použít programové funkce nebo smyčky.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-201">You can't use coded functions or loops in your tests.</span></span> <span data-ttu-id="9d8fb-202">Hello test musí být zcela obsažené ve skriptu .webtest hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-202">hello test must be contained completely in hello .webtest script.</span></span> <span data-ttu-id="9d8fb-203">Můžete však použít standardní moduly plug-in.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-203">However, you can use standard plugins.</span></span>
>

#### <a name="1-record-a-scenario"></a><span data-ttu-id="9d8fb-204">1. Záznam scénáře</span><span class="sxs-lookup"><span data-stu-id="9d8fb-204">1. Record a scenario</span></span>
<span data-ttu-id="9d8fb-205">Použít Visual Studio Enterprise toorecord a relace webu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-205">Use Visual Studio Enterprise toorecord a web session.</span></span>

1. <span data-ttu-id="9d8fb-206">Vytvořte projekt testu výkonnosti webu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-206">Create a Web performance test project.</span></span>

    ![V aplikaci Visual Studio Enterprise edition vytvořte projekt ze šablony výkonu webu a zátěžového testu hello.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-create.png)

 * <span data-ttu-id="9d8fb-208">*Nevidíte hello výkonu webů a zátěžového testu šablony?*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-208">*Don't see hello Web Performance and Load Test template?*</span></span> <span data-ttu-id="9d8fb-209">– Zavřete sadu Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-209">- Close Visual Studio Enterprise.</span></span> <span data-ttu-id="9d8fb-210">Otevřete **instalační program Visual Studio** toomodify instalaci Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-210">Open **Visual Studio Installer** toomodify your Visual Studio Enterprise installation.</span></span> <span data-ttu-id="9d8fb-211">V části **Jednotlivé komponenty** vyberte **Nástroje pro testování výkonnosti webů a zátěžové testování**.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-211">Under **Individual Components**, select **Web Performance and load testing tools**.</span></span>

2. <span data-ttu-id="9d8fb-212">Otevřete soubor .webtest hello a spusťte záznam.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-212">Open hello .webtest file and start recording.</span></span>

    ![Otevřete soubor .webtest hello a klikněte na tlačítko záznam.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-start.png)
3. <span data-ttu-id="9d8fb-214">Hello akcí uživatele chcete toosimulate v testu: Otevřete webovou stránku, přidat do košíku produkt toohello a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-214">Do hello user actions you want toosimulate in your test: open your website, add a product toohello cart, and so on.</span></span> <span data-ttu-id="9d8fb-215">Pak test zastavte.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-215">Then stop your test.</span></span>

    ![Hello záznamník testování webu běží v aplikaci Internet Explorer.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-record.png)

    <span data-ttu-id="9d8fb-217">Nevytvářejte příliš dlouhý scénář.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-217">Don't make a long scenario.</span></span> <span data-ttu-id="9d8fb-218">Platí limit 100 kroků a 2 minut.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-218">There's a limit of 100 steps and 2 minutes.</span></span>
4. <span data-ttu-id="9d8fb-219">Úpravy hello test na:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-219">Edit hello test to:</span></span>

   * <span data-ttu-id="9d8fb-220">Přidání ověření toocheck hello přijatých textu a kódů odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-220">Add validations toocheck hello received text and response codes.</span></span>
   * <span data-ttu-id="9d8fb-221">Odeberte všechny nadbytečné interakce.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-221">Remove any superfluous interactions.</span></span> <span data-ttu-id="9d8fb-222">Můžete také odebrat závislé požadavky pro obrázky nebo tooad nebo sledování lokalit.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-222">You could also remove dependent requests for pictures or tooad or tracking sites.</span></span>

     <span data-ttu-id="9d8fb-223">Mějte na paměti, že můžete upravit pouze hello zkušební skript – nelze přidat vlastní kód nebo volat jiné webové testy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-223">Remember that you can only edit hello test script - you can't add custom code or call other web tests.</span></span> <span data-ttu-id="9d8fb-224">Nevkládejte smyčky v testu hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-224">Don't insert loops in hello test.</span></span> <span data-ttu-id="9d8fb-225">Můžete použít standardní zásuvné moduly webového testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-225">You can use standard web test plug-ins.</span></span>
5. <span data-ttu-id="9d8fb-226">Spusťte hello test v sadě Visual Studio toomake se, že funguje.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-226">Run hello test in Visual Studio toomake sure it works.</span></span>

    <span data-ttu-id="9d8fb-227">Hello Spouštěč webových testů otevře webový prohlížeč a opakuje hello akce, které jste si poznamenali.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-227">hello web test runner opens a web browser and repeats hello actions you recorded.</span></span> <span data-ttu-id="9d8fb-228">Zkontrolujte, zda vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-228">Make sure it works as you expect.</span></span>

    ![V sadě Visual Studio otevřete soubor .webtest hello a klikněte na tlačítko spustit.](./media/app-insights-monitor-web-app-availability/appinsights-71webtest-multi-vs-run.png)

#### <a name="2-upload-hello-web-test-tooapplication-insights"></a><span data-ttu-id="9d8fb-230">2. Nahrát hello webového testu tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="9d8fb-230">2. Upload hello web test tooApplication Insights</span></span>
1. <span data-ttu-id="9d8fb-231">Hello portálu Application Insights vytvoření testu webu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-231">In hello Application Insights portal, create a web test.</span></span>

    ![V okně pro hello webové testy zvolte možnost přidat.](./media/app-insights-monitor-web-app-availability/16-another-test.png)
2. <span data-ttu-id="9d8fb-233">Vyberte vícekrokový test a nahrajte soubor .webtest hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-233">Select multi-step test, and upload hello .webtest file.</span></span>

    ![Vyberte vícekrokový webový test.](./media/app-insights-monitor-web-app-availability/appinsights-71webtestUpload.png)

    <span data-ttu-id="9d8fb-235">Sada hello testovanými místy, četnost a parametry výstrah v hello stejný způsobem jako u příkazu ping testuje.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-235">Set hello test locations, frequency, and alert parameters in hello same way as for ping tests.</span></span>

#### <a name="3-see-hello-results"></a><span data-ttu-id="9d8fb-236">3. Zobrazte výsledky hello</span><span class="sxs-lookup"><span data-stu-id="9d8fb-236">3. See hello results</span></span>

<span data-ttu-id="9d8fb-237">Zobrazit svůj test výsledky a všechny chyby v hello testy stejným způsobem jako jedné adresy url.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-237">View your test results and any failures in hello same way as single-url tests.</span></span>

<span data-ttu-id="9d8fb-238">Kromě toho si můžete stáhnout tooview výsledky testu hello je v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-238">In addition, you can download hello test results tooview them in Visual Studio.</span></span>

#### <a name="too-many-failures"></a><span data-ttu-id="9d8fb-239">Příliš mnoho selhání?</span><span class="sxs-lookup"><span data-stu-id="9d8fb-239">Too many failures?</span></span>

* <span data-ttu-id="9d8fb-240">Běžným důvodem selhání je, že tento hello test běží příliš dlouho.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-240">A common reason for failure is that hello test runs too long.</span></span> <span data-ttu-id="9d8fb-241">Nesmí se spouštět déle než dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-241">It mustn't run longer than two minutes.</span></span>

* <span data-ttu-id="9d8fb-242">Nezapomeňte, že musí správně načítat všechny prostředky hello stránky pro test toosucceed hello, včetně skripty, šablony stylů, obrázků a tak dále.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-242">Don't forget that all hello resources of a page must load correctly for hello test toosucceed, including scripts, style sheets, images, and so forth.</span></span>

* <span data-ttu-id="9d8fb-243">Hello webový test musí být zcela obsažen ve skriptu hello .webtest: programové funkce nelze použít v testu hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-243">hello web test must be entirely contained in hello .webtest script: you can't use coded functions in hello test.</span></span>

### <a name="plugging-time-and-random-numbers-into-your-multi-step-test"></a><span data-ttu-id="9d8fb-244">Doba zapojení a náhodná čísla do vícekrokového testu</span><span class="sxs-lookup"><span data-stu-id="9d8fb-244">Plugging time and random numbers into your multi-step test</span></span>
<span data-ttu-id="9d8fb-245">Předpokládejme, že testujete nástroj, který získá data závislá na čase, například akcie z externího kanálu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-245">Suppose you're testing a tool that gets time-dependent data such as stocks from an external feed.</span></span> <span data-ttu-id="9d8fb-246">Při záznamu webového testu máte toouse určitých časech, ale můžete nastavit je jako parametry hello otestovat, čas spuštění a čas ukončení.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-246">When you record your web test, you have toouse specific times, but you set them as parameters of hello test, StartTime and EndTime.</span></span>

![Webový test s parametry.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-parameters.png)

<span data-ttu-id="9d8fb-248">Když spustíte hello test, byste chtěli čas ukončení vždy k dispozici toobe hello čas a čas spuštění by měl 15 minut před.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-248">When you run hello test, you'd like EndTime always toobe hello present time, and StartTime should be 15 minutes ago.</span></span>

<span data-ttu-id="9d8fb-249">Test zásuvné moduly webového poskytují způsob hello toodo Parametrizace časy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-249">Web Test Plug-ins provide hello way toodo parameterize times.</span></span>

1. <span data-ttu-id="9d8fb-250">Přidejte zásuvný modul webového testu pro každou hodnotu parametru proměnné, kterou chcete.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-250">Add a web test plug-in for each variable parameter value you want.</span></span> <span data-ttu-id="9d8fb-251">V hello nástrojů webového testu zvolte **přidat modul plug-in pro testování webových**.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-251">In hello web test toolbar, choose **Add Web Test Plugin**.</span></span>

    ![Zvolte možnost přidat zásuvný modul pro testování webu a vyberte typ.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugins.png)

    <span data-ttu-id="9d8fb-253">V tomto příkladu používáme dvě instance hello Plug-in datum čas.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-253">In this example, we use two instances of hello Date Time Plug-in.</span></span> <span data-ttu-id="9d8fb-254">Jedna instance je „před 15 minutami“ a druhá „teď“.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-254">One instance is for "15 minutes ago" and another for "now."</span></span>
2. <span data-ttu-id="9d8fb-255">Otevřete hello vlastnosti každého zásuvného modulu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-255">Open hello properties of each plug-in.</span></span> <span data-ttu-id="9d8fb-256">Pojmenujte ho a nastavte ji toouse hello aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-256">Give it a name and set it toouse hello current time.</span></span> <span data-ttu-id="9d8fb-257">Pro jeden z nich nastavte Přidat minuty = -15.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-257">For one of them, set Add Minutes = -15.</span></span>

    ![Nastavte název, použijte aktuální čas a přidejte minuty.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-parameters.png)
3. <span data-ttu-id="9d8fb-259">V parametrech webového testu hello použijte {{plug-in name}} tooreference na název zásuvného modulu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-259">In hello web test parameters, use {{plug-in name}} tooreference a plug-in name.</span></span>

    ![V parametru hello testu použijte {{plug-in name}}.](./media/app-insights-monitor-web-app-availability/appinsights-72webtest-plugin-name.png)

<span data-ttu-id="9d8fb-261">Teď nahrajte portálu toohello testu.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-261">Now, upload your test toohello portal.</span></span> <span data-ttu-id="9d8fb-262">Při každé spuštění testu hello používá hello dynamické hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-262">It uses hello dynamic values on every run of hello test.</span></span>

## <a name="dealing-with-sign-in"></a><span data-ttu-id="9d8fb-263">Vyřešení přihlášení</span><span class="sxs-lookup"><span data-stu-id="9d8fb-263">Dealing with sign-in</span></span>
<span data-ttu-id="9d8fb-264">Pokud vaši uživatelé přihlásí tooyour aplikace, máte různé možnosti pro simulaci přihlášení, takže můžete testovat stránky za hello přihlásit.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-264">If your users sign in tooyour app, you have various options for simulating sign-in so that you can test pages behind hello sign-in.</span></span> <span data-ttu-id="9d8fb-265">Hello přístupů, které použijete, závisí na hello typu zabezpečení poskytovaném aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-265">hello approach you use depends on hello type of security provided by hello app.</span></span>

<span data-ttu-id="9d8fb-266">Ve všech případech by měl vytvořit účet v aplikaci jenom pro účely hello testování.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-266">In all cases, you should create an account in your application just for hello purpose of testing.</span></span> <span data-ttu-id="9d8fb-267">Pokud je to možné omezte hello oprávnění tohoto účtu testovací tak, že není možné hello webových testů by to ovlivnilo skutečné uživatelé.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-267">If possible, restrict hello permissions of this test account so that there's no possibility of hello web tests affecting real users.</span></span>

### <a name="simple-username-and-password"></a><span data-ttu-id="9d8fb-268">Jednoduché uživatelské jméno a heslo</span><span class="sxs-lookup"><span data-stu-id="9d8fb-268">Simple username and password</span></span>
<span data-ttu-id="9d8fb-269">V hello záznam webového testu obvyklým způsobem.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-269">Record a web test in hello usual way.</span></span> <span data-ttu-id="9d8fb-270">Nejprve odstraňte soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-270">Delete cookies first.</span></span>

### <a name="saml-authentication"></a><span data-ttu-id="9d8fb-271">Ověřování SAML</span><span class="sxs-lookup"><span data-stu-id="9d8fb-271">SAML authentication</span></span>
<span data-ttu-id="9d8fb-272">Použijte zásuvný modul SAML hello, která je k dispozici pro webové testy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-272">Use hello SAML plugin that is available for web tests.</span></span>

### <a name="client-secret"></a><span data-ttu-id="9d8fb-273">Tajný klíč klienta</span><span class="sxs-lookup"><span data-stu-id="9d8fb-273">Client secret</span></span>
<span data-ttu-id="9d8fb-274">Pokud vaše aplikace obsahuje trasu přihlášení, která zahrnuje tajný klíč klienta, použijte ji.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-274">If your app has a sign-in route that involves a client secret, use that route.</span></span> <span data-ttu-id="9d8fb-275">Azure Active Directory (AAD) je příkladem služby, která poskytuje přihlašování pomocí tajného klíče klienta.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-275">Azure Active Directory (AAD) is an example of a service that provides a client secret sign-in.</span></span> <span data-ttu-id="9d8fb-276">V AAD je sdílený tajný klíč klienta hello hello klíč aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-276">In AAD, hello client secret is hello App Key.</span></span>

<span data-ttu-id="9d8fb-277">Tady je ukázkový webový test webové aplikace v Azure pomocí klíče aplikace:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-277">Here's a sample web test of an Azure web app using an app key:</span></span>

![Ukázkový tajný klíč klienta](./media/app-insights-monitor-web-app-availability/110.png)

1. <span data-ttu-id="9d8fb-279">Získejte token ze služby AAD pomocí tajného klíče klienta (AppKey).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-279">Get token from AAD using client secret (AppKey).</span></span>
2. <span data-ttu-id="9d8fb-280">Extrahujte nosný token z odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-280">Extract bearer token from response.</span></span>
3. <span data-ttu-id="9d8fb-281">Volání rozhraní API pomocí tokenu nosiče v hlavičce autorizace hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-281">Call API using bearer token in hello authorization header.</span></span>

<span data-ttu-id="9d8fb-282">Ujistěte se, že webový test hello je skutečný klienta – to znamená, má svou vlastní aplikaci v AAD - a použít jeho clientId + appkey.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-282">Make sure that hello web test is an actual client - that is, it has its own app in AAD - and use its clientId + appkey.</span></span> <span data-ttu-id="9d8fb-283">Služby testovaného také má svou vlastní aplikaci v AAD: hello appID URI této aplikace se odrazí v testu webu hello v poli "prostředek" hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-283">Your service under test also has its own app in AAD: hello appID URI of this app is reflected in hello web test in hello “resource” field.</span></span>

### <a name="open-authentication"></a><span data-ttu-id="9d8fb-284">Otevřené ověřování</span><span class="sxs-lookup"><span data-stu-id="9d8fb-284">Open Authentication</span></span>
<span data-ttu-id="9d8fb-285">Příkladem otevřeného ověřování je přihlašování pomocí účtu Microsoft nebo Google.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-285">An example of open authentication is signing in with your Microsoft or Google account.</span></span> <span data-ttu-id="9d8fb-286">Velký počet aplikací, použijte OAuth poskytují hello alternativní tajný klienta, takže prvním cílem by mělo být tooinvestigate tuto možnost.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-286">Many apps that use OAuth provide hello client secret alternative, so your first tactic should be tooinvestigate that possibility.</span></span>

<span data-ttu-id="9d8fb-287">Pokud váš test musí přihlásit pomocí OAuth, je obecný přístup hello:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-287">If your test must sign in using OAuth, hello general approach is:</span></span>

* <span data-ttu-id="9d8fb-288">Pomocí nástroje, jako je Fiddler tooexamine hello provoz mezi webového prohlížeče, webem ověřování hello a aplikace.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-288">Use a tool such as Fiddler tooexamine hello traffic between your web browser, hello authentication site, and your app.</span></span>
* <span data-ttu-id="9d8fb-289">Proveďte dvě nebo více přihlášení pomocí různých počítačů nebo prohlížečů nebo v dlouhých intervalech (tooexpire tooallow tokeny).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-289">Perform two or more sign-ins using different machines or browsers, or at long intervals (tooallow tokens tooexpire).</span></span>
* <span data-ttu-id="9d8fb-290">Porovnáním různých relací Identifikujte hello token předaný zpět z hello ověřování lokality, který byl následně předán tooyour aplikačního serveru po přihlášení.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-290">By comparing different sessions, identify hello token passed back from hello authenticating site, that is then passed tooyour app server after sign-in.</span></span>
* <span data-ttu-id="9d8fb-291">Uložte webový test pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-291">Record a web test using Visual Studio.</span></span>
* <span data-ttu-id="9d8fb-292">Parametrizace hello tokeny, nastavení hello parametr při vrácení hello tokenu z ověřovatele hello a jeho použití hello dotazu toohello lokality.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-292">Parameterize hello tokens, setting hello parameter when hello token is returned from hello authenticator, and using it in hello query toohello site.</span></span>
  <span data-ttu-id="9d8fb-293">(Visual Studio pokusí tooparameterize hello testu, ale není správně parametrizovat tokeny hello.)</span><span class="sxs-lookup"><span data-stu-id="9d8fb-293">(Visual Studio attempts tooparameterize hello test, but does not correctly parameterize hello tokens.)</span></span>


## <a name="performance-tests"></a><span data-ttu-id="9d8fb-294">Testy výkonnosti</span><span class="sxs-lookup"><span data-stu-id="9d8fb-294">Performance tests</span></span>
<span data-ttu-id="9d8fb-295">Na svém webu můžete spustit zátěžový test.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-295">You can run a load test on your website.</span></span> <span data-ttu-id="9d8fb-296">Jako hello test dostupnosti můžete odeslat jednoduché požadavky nebo požadavky vícekrokový z našich bodů kolem hello, world.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-296">Like hello availability test, you can send either simple requests or multi-step requests from our points around hello world.</span></span> <span data-ttu-id="9d8fb-297">Na rozdíl od testu dostupnosti se odesílá mnoho požadavků, které simulují několik souběžných uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-297">Unlike an availability test, many requests are sent, simulating multiple simultaneous users.</span></span>

<span data-ttu-id="9d8fb-298">V okně Přehled hello, otevřete **nastavení**, **testy výkonnosti**.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-298">From hello Overview blade, open **Settings**, **Performance Tests**.</span></span> <span data-ttu-id="9d8fb-299">Když vytvoříte testu, se zobrazí Pozvánka tooconnect tooor vytvořit účet Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-299">When you create a test, you are invited tooconnect tooor create a Visual Studio Team Services account.</span></span>

<span data-ttu-id="9d8fb-300">Po dokončení testu hello se zobrazí dobu odezvy a úspěšnost.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-300">When hello test is complete, you are shown response times and success rates.</span></span>


![Test výkonnosti](./media/app-insights-monitor-web-app-availability/perf-test.png)

> [!TIP]
> <span data-ttu-id="9d8fb-302">použít tooobserve hello důsledků testu výkonnosti [živý datový proud](app-insights-live-stream.md) a [profileru](app-insights-profiler.md).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-302">tooobserve hello effects of a performance test, use [Live Stream](app-insights-live-stream.md) and [Profiler](app-insights-profiler.md).</span></span>
>

## <a name="automation"></a><span data-ttu-id="9d8fb-303">Automation</span><span class="sxs-lookup"><span data-stu-id="9d8fb-303">Automation</span></span>
* <span data-ttu-id="9d8fb-304">[Použít tooset skripty PowerShell se test dostupnosti](app-insights-powershell.md#add-an-availability-test) automaticky.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-304">[Use PowerShell scripts tooset up an availability test](app-insights-powershell.md#add-an-availability-test) automatically.</span></span>
* <span data-ttu-id="9d8fb-305">Nastavení [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md), který je volán při vydání výstrahy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-305">Set up a [webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) that is called when an alert is raised.</span></span>

## <span data-ttu-id="9d8fb-306"><a name="qna"></a>Máte dotazy?</span><span class="sxs-lookup"><span data-stu-id="9d8fb-306"><a name="qna"></a>Questions?</span></span> <span data-ttu-id="9d8fb-307">Problémy?</span><span class="sxs-lookup"><span data-stu-id="9d8fb-307">Problems?</span></span>
* <span data-ttu-id="9d8fb-308">*Můžu z webového testu volat kód?*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-308">*Can I call code from my web test?*</span></span>

    <span data-ttu-id="9d8fb-309">Ne.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-309">No.</span></span> <span data-ttu-id="9d8fb-310">Hello kroky testu hello musí být v souboru .webtest hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-310">hello steps of hello test must be in hello .webtest file.</span></span> <span data-ttu-id="9d8fb-311">A nemůžete volat jiné webové testy nebo používat smyčky.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-311">And you can't call other web tests or use loops.</span></span> <span data-ttu-id="9d8fb-312">Existují různé zásuvné moduly, které se vám můžou hodit.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-312">But there are several plug-ins that you might find helpful.</span></span>
* <span data-ttu-id="9d8fb-313">*Je podporovaný protokol HTTPS?*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-313">*Is HTTPS supported?*</span></span>

    <span data-ttu-id="9d8fb-314">Podporujeme protokoly TLS 1.1 a TLS 1.2.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-314">We support TLS 1.1 and TLS 1.2.</span></span>
* <span data-ttu-id="9d8fb-315">*Je nějaký rozdíl mezi „webovými testy“ a „testy dostupnosti“?*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-315">*Is there a difference between "web tests" and "availability tests"?*</span></span>

    <span data-ttu-id="9d8fb-316">Hello dvě podmínky může odkazovat zcela zaměnitelným významem.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-316">hello two terms may be referenced interchangeably.</span></span> <span data-ttu-id="9d8fb-317">Testy dostupnosti je více obecný pojem, který zahrnuje jeden ping adresy URL hello testy kromě toohello vícekrokové webové testy.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-317">Availability tests is a more generic term that includes hello single URL ping tests in addition toohello multi-step web tests.</span></span>
* <span data-ttu-id="9d8fb-318">*Nastavit jako toouse testy dostupnosti na našem interním serveru, který se spouští za bránou firewall.*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-318">*I'd like toouse availability tests on our internal server that runs behind a firewall.*</span></span>

    <span data-ttu-id="9d8fb-319">Existují dvě možná řešení:</span><span class="sxs-lookup"><span data-stu-id="9d8fb-319">There are two possible solutions:</span></span>
    
    * <span data-ttu-id="9d8fb-320">Konfigurace vaší brány firewall toopermit příchozí požadavky z hello [IP adresy naše webové testovací agenti](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="9d8fb-320">Configure your firewall toopermit incoming requests from hello [IP addresses of our web test agents](app-insights-ip-addresses.md).</span></span>
    * <span data-ttu-id="9d8fb-321">Zapsat svůj vlastní test tooperiodically kód interního serveru.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-321">Write your own code tooperiodically test your internal server.</span></span> <span data-ttu-id="9d8fb-322">Spusťte hello kód jako proces na pozadí na testovacím serveru za vaší bránou firewall.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-322">Run hello code as a background process on a test server behind your firewall.</span></span> <span data-ttu-id="9d8fb-323">Vaše testovací proces může odesílat své výsledky tooApplication statistika pomocí [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) rozhraní API v balíčku SDK základní hello.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-323">Your test process can send its results tooApplication Insights by using [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API in hello core SDK package.</span></span> <span data-ttu-id="9d8fb-324">To vyžaduje, aby vaše testovací toohave odchozí přístup toohello Application Insights přijímání koncový bod serveru, ale který je mnohem menší riziko zabezpečení než hello alternativní povolit příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-324">This requires your test server toohave outgoing access toohello Application Insights ingestion endpoint, but that is a much smaller security risk than hello alternative of permitting incoming requests.</span></span> <span data-ttu-id="9d8fb-325">výsledky Hello se nebude zobrazovat na hello dostupnost webové testy oken, ale se zobrazí jako dostupnosti výsledky analýzy, vyhledávání a metrika Explorer.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-325">hello results will not appear in hello availability web tests blades, but appears as availability results in Analytics, Search, and Metric Explorer.</span></span>
* <span data-ttu-id="9d8fb-326">*Vícekrokový webový test se nepodařilo nahrát.*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-326">*Uploading a multi-step web test fails*</span></span>

    <span data-ttu-id="9d8fb-327">Maximální velikost je 300 kB.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-327">There's a size limit of 300 K.</span></span>

    <span data-ttu-id="9d8fb-328">Smyčky nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-328">Loops aren't supported.</span></span>

    <span data-ttu-id="9d8fb-329">Odkazy na tooother webové testy nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-329">References tooother web tests aren't supported.</span></span>

    <span data-ttu-id="9d8fb-330">Zdroje dat nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-330">Data sources aren't supported.</span></span>
* <span data-ttu-id="9d8fb-331">*Můj vícekrokový test se nedokončil.*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-331">*My multi-step test doesn't complete*</span></span>

    <span data-ttu-id="9d8fb-332">Existuje limit 100 požadavků na test.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-332">There's a limit of 100 requests per test.</span></span>

    <span data-ttu-id="9d8fb-333">Hello test je zastavena, pokud poběží déle než dvě minuty.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-333">hello test is stopped if it runs longer than two minutes.</span></span>
* <span data-ttu-id="9d8fb-334">*Jak spustím test s klientskými certifikáty?*</span><span class="sxs-lookup"><span data-stu-id="9d8fb-334">*How can I run a test with client certificates?*</span></span>

    <span data-ttu-id="9d8fb-335">Tuto možnost nepodporujeme, je nám líto.</span><span class="sxs-lookup"><span data-stu-id="9d8fb-335">We don't support that, sorry.</span></span>


## <span data-ttu-id="9d8fb-336"><a name="next"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d8fb-336"><a name="next"></a>Next steps</span></span>
<span data-ttu-id="9d8fb-337">[Prohledávání diagnostických protokolů][diagnostic]</span><span class="sxs-lookup"><span data-stu-id="9d8fb-337">[Search diagnostic logs][diagnostic]</span></span>

<span data-ttu-id="9d8fb-338">[Řešení potíží][qna]</span><span class="sxs-lookup"><span data-stu-id="9d8fb-338">[Troubleshooting][qna]</span></span>

[<span data-ttu-id="9d8fb-339">IP adresy webových testovacích agentů</span><span class="sxs-lookup"><span data-stu-id="9d8fb-339">IP addresses of web test agents</span></span>](app-insights-ip-addresses.md)

<!--Link references-->

[azure-availability]: ../insights-create-web-tests.md
[diagnostic]: app-insights-diagnostic-search.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
