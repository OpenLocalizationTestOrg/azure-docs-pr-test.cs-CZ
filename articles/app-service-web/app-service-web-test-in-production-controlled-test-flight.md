---
title: "Flighting nasazení (testování verze beta) v Azure App Service"
description: "Další informace o letu nové funkce v aplikaci nebo verze beta testování vaše aktualizace v tomto kurzu začátku do konce. Spojuje služby App Service funkcí, jako je nepřetržitá publikování, sloty, směrování provozu a integrace Application Insights."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 17953c51-38f8-442d-bb0b-f69c1542f0e9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cephalin
ms.openlocfilehash: 83e3247310461ac148fff3c4ade3aa7216478537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting nasazení (testování verze beta) v Azure App Service
V tomto kurzu se dozvíte, jak udělat *flighting nasazení* integrací různé možnosti [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure Application Insights](/services/application-insights/).

*Flighting* je proces nasazení, která ověřuje nové funkce nebo s omezený počet zákazníků skutečné změnit, a hlavní testování v produkční scénář. Se podobají testování beta a se někdy označuje jako "řízené testovací letu". Tuto metodu použijte, mnoho velkých podnicích se webová služba získat časná ověření na jejich aktualizace aplikací v jejich postup [agile vývoj](https://en.wikipedia.org/wiki/Agile_software_development). Azure App Service umožňuje integraci se službou Application Insights implementovat stejné scénář DevOps a publikování nepřetržité testování v produkčním prostředí. Výhody tohoto přístupu patří:

* **Získání zpětné vazby skutečné *před* aktualizace vydávají do produkčního prostředí** -lepší, než získají zpětnou vazbu, jakmile uvolnění jediné, co je získání zpětné vazby před uvolněním. Aktualizace s přenosy reálný uživatel a chování můžete otestovat časná si přejete v životním cyklu produktu.
* **Vylepšení [průběžné testy řízený vývoj (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  – díky integraci testování v produkčním prostředí se průběžnou integraci a instrumentace s Application Insights, ověření uživatele se již v rané fázi stane a automaticky v životního cyklu produktu. To pomáhá zkrátit čas investice do spuštění manuálního testu.
* **Optimalizace pracovního postupu testování** -automatizací testování v produkčním prostředí s nepřetržité monitorování instrumentace můžete potenciálně provést cílů různé druhy testů v jednom procesu, jako například [integrace](https://en.wikipedia.org/wiki/Integration_testing), [regrese](https://en.wikipedia.org/wiki/Regression_testing), [použitelnost](https://en.wikipedia.org/wiki/Usability_testing), usnadnění, lokalizace, [výkonu](https://en.wikipedia.org/wiki/Software_performance_testing), [zabezpečení](https://en.wikipedia.org/wiki/Security_testing), a [ přijetí](https://en.wikipedia.org/wiki/Acceptance_testing).

Flighting nasazení není jenom o směrování provoz za provozu. Při takovémto nasazení chcete získat přehled co nejrychleji je použít neočekávané chyby, snížení výkonu, problémy uživatelského rozhraní. Mějte na paměti, kterou chcete pracovat se skutečnou zákazníků. Proto to udělat správné, můžete musí se ujistěte, že jste nastavili flighting nasazení, ke shromažďování všech dat, je nutné, aby bylo možné provést informované rozhodnutí pro další krok. V tomto kurzu se dozvíte, jak ke shromažďování dat pomocí služby Application Insights, ale můžete použít New Relic nebo jiných technologií, které vyhovuje vašemu scénáři.

## <a name="what-you-will-do"></a>Co provedete
V tomto kurzu se dozvíte, jak probouzet následující scénáře společně k testování aplikace služby App Service v produkčním prostředí:

* [Směrování provozu produkční](app-service-web-test-in-production-get-start.md) do vaší aplikace beta
* [Instrumentace aplikace](../application-insights/app-insights-web-track-usage.md) získat užitečné metriky
* Nepřetržitě nasazení vaší aplikace beta a sledovat metriky za provozu aplikace
* Porovnání metriky mezi na produkční aplikaci a beta aplikaci najdete v části jak změny kódu převede výsledky

## <a name="what-you-will-need"></a>Co budete potřebovat
* Účet Azure
* A [Githubu](https://github.com/) účtu
* Visual Studio 2015 - si můžete stáhnout [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
* Git prostředí (nainstalované s [GitHub pro Windows](https://windows.github.com/)) – umožňuje spustit příkazy Git i prostředí PowerShell ve stejné relaci
* Nejnovější [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
* Základní znalosti o následující:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení šablony (viz [nasazení komplexní aplikace předvídatelné v Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> K dokončení tohoto kurzu potřebujete mít účet Azure:
>
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití můžete účet ponechat a používat bezplatné služby Azure, jako jsou webové aplikace.
> * Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -si Visual Studio předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
>
> Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a name="set-up-your-production-web-app"></a>Nastavení webové aplikace produkční
> [!NOTE]
> Skript používá v tomto kurzu bude automaticky nakonfigurovat nepřetržité publikování z úložiště Githubu. To vyžaduje, aby vaše přihlašovací údaje Githubu jsou již uloženy v Azure, jinak skriptované instalace se nezdaří, při pokusu o konfiguraci nastavení správy zdrojů pro webové aplikace.
>
> K ukládání přihlašovacích údajů Githubu v Azure, vytvořit webovou aplikaci v [portálu Azure](https://portal.azure.com/) a [konfigurace nasazení Githubu](app-service-continuous-deployment.md). Stačí k tomu jednou.
>
>

V případě typické DevOps máte aplikaci, která běží v Azure za provozu a chcete provést změny prostřednictvím průběžné publikování. V tomto scénáři se do produkčního prostředí nasadit šablonu, která mají vývoji a testování.

1. Vytvoření vlastního pokračovatelem [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště. Informace o vytváření vašeho rozvětvení najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/). Po vytvoření vaší rozvětvení, zobrazí se v prohlížeči.

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Otevřete relaci prostředí Git. Pokud ještě nemáte Git prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) nyní.
3. Vytvořte místní vaší rozvětvení spuštěním následujícího příkazu:

     https://github.com/<your_fork>/ToDoApp.git klon Git
4. Až budete mít vaše místní klonování, přejděte na  *&lt;repository_root >*\ARMTemplates a spustit deploy.ps1 skript s jedinečnou příponu, jak je uvedeno níže:

     .\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix >
5. Po zobrazení výzvy zadejte požadované uživatelské jméno a heslo pro přístup k databázi. Mějte na paměti svoje přihlašovací údaje databáze, protože budete muset zadávat znovu při aktualizaci skupině prostředků.

   Měli byste vidět zřizování průběh různých prostředků Azure. Po dokončení nasazení, skript bude spuštění aplikace v prohlížeči a získáte popisný zvukový signál.
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. Zpět v relaci prostředí Git, spusťte tento příkaz:

     . \swap – název ToDoApp < your_suffix >

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. Po dokončení skriptu, přejděte zpět a přejděte na adresu front-endu (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) zobrazíte aplikace běžící v produkčním prostředí.
8. Přihlaste se [portálu Azure](https://portal.azure.com/) a prohlédněte si co je vytvořen.

   Byste měli vidět dvě webové aplikace ve stejné skupině prostředků, jeden s `Api` přípony v názvu. Pokud se podíváte na zobrazení skupiny prostředků, zobrazí se také SQL Database a serveru, plán služby App Service a přípravné sloty pro webové aplikace. Procházet různých prostředků a porovnejte je s  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json zobrazíte, jak jsou nakonfigurované v šabloně.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Jste nastavili na produkční aplikaci.  Nyní Pojďme Představte si, že nedostanete zpětnou vazbu, použitelnost je špatná pro aplikaci. Proto se rozhodnete prozkoumat. Budete k instrumentaci aplikace tak, abyste získali zpětnou vazbu.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Prozkoumat: Instrumentace vaší klientské aplikace pro monitorování/metriky
1. Otevřete  *&lt;repository_root >*\src\MultiChannelToDo.sln v sadě Visual Studio.
2. Obnovení všech balíčků Nuget kliknutím pravým tlačítkem na řešení > **spravovat balíčky NuGet pro řešení** > **obnovení**.
3. Klikněte pravým tlačítkem na **MultiChannelToDo.Web** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků. ToDoApp*&lt;your_suffix >* > **přidat službu Application Insights do projektu**.
4. Na portálu Azure otevřete okno **MultiChannelToDo.Web** aplikace přehled prostředků. Potom v **stavu aplikací** součástí, klikněte na tlačítko **zjistěte, jak shromažďovat data zatížení stránky prohlížeče** > zkopírujte kód.
5. Přidejte zkopírované kód instrumentace JS k  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml těsně před uzavírací `<heading>` značky. Měl by obsahovat klíč instrumentace jedinečný prostředek přehled aplikace.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. Odeslat vlastní události Application Insights pro myš kliknutí přidáním následující kód do dolní části textu:

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   Vlastní události Application Insights pokaždé, když uživatel klikne kdekoli ve webové aplikaci odešle tento fragment kódu jazyka JavaScript.
7. V prostředí Git potvrďte a odešlete změny do vaší rozvětvení v Githubu. Potom počkejte klientů aktualizujte prohlížeče.

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. Swap – změny aplikace nasazené do produkčního prostředí:

     . \swap – název ToDoApp < your_suffix >
9. Procházejte do zdroje Application Insights, který jste nakonfigurovali. Klikněte na tlačítko Vlastní události.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   Pokud nevidíte metriky pro vlastní události, počkejte několik minut a klikněte na tlačítko **aktualizovat**.

Předpokládejme, že se zobrazí graf jako níže:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

A pod ním mřížky událostí:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Podle kódu aplikace ToDoApp **tlačítko** události odpovídá tlačítko pro odeslání a **vstup** události odpovídá textové pole. Zatím věcí smysl. Ale to vypadá, je dobré množství kliknutí a jen několik kliknutí položkami seznamu úkolů ( **LI** událostí).

Založené na tomto formuláři je vaše předpoklad, že někteří uživatelé nerozumíte, kterou část uživatelského rozhraní je můžete kliknout a je to proto kurzor je navržen pro výběr textu je vždy na položky seznamu a jejich ikony, když se.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

To může být contrived příklad. Nicméně budete provádět zlepšení do vaší aplikace a pak proveďte flighting nasazení získání použitelnost zpětné vazby od zákazníků za provozu.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Aplikace serveru pro monitorování/metriky instrumentace
To je tangens, protože tento scénář ukázáno v tomto kurzu zabývá pouze klientské aplikace. Ale pro úplnost nastavíte serverové aplikace.

1. Klikněte pravým tlačítkem na **MultiChannelToDo** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků. ToDoApp*&lt;your_suffix >* > **přidat službu Application Insights do projektu**.
2. V prostředí Git potvrďte a odešlete změny do vaší rozvětvení v Githubu. Potom počkejte klientů aktualizujte prohlížeče.

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. Swap – změny aplikace nasazené do produkčního prostředí:

     . \swap – název ToDoApp < your_suffix >

A to je vše!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Prozkoumat: Přidání značek specifické pro slot pro vaše aplikace metriky klienta
V této části můžete nakonfigurovat různé nasazovací sloty k odesílání telemetrie specifické pro slot do stejného zdroje Application Insights. Tímto způsobem snadno projevily provedené změny aplikace můžete porovnat telemetrická data mezi přenosy dat z různých sloty (nasazení prostředí). Ve stejnou dobu můžete oddělit provoz produkční od ostatních, můžete nadále sledovat vaše produkční aplikace podle potřeby.

Vzhledem k tomu, že shromažďování dat na chování klienta bude [přidat inicializátoru telemetrie do kódu jazyka JavaScript](../application-insights/app-insights-api-filtering-sampling.md) v index.cshtml. Pokud chcete k testování výkonu na straně serveru, například můžete provést také podobně jako v kódu serveru (viz [Application Insights API pro vlastní události a metriky](../application-insights/app-insights-api-custom-events-metrics.md).

1. Nejprve přidejte kód bewteen dva `//` blokovat komentáře níže v jazyka JavaScript, který jste přidali do `<heading>` značky dříve.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Tento kód inicializátoru způsobí, že `appInsights` objekt, který chcete přidat vlastní vlastnost s názvem `Environment` k každá část telemetrie odešle.
2. Dál přidejte tuto vlastní vlastnost jako [pozice nastavení](web-sites-staged-publishing.md#AboutConfiguration) pro webovou aplikaci v Azure. Chcete-li to provést, spusťte následující příkazy v relaci prostředí Git.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Soubor Web.config v projektu již definuje `environment` nastavení aplikace. Při tomto nastavení se při testování aplikaci místně, vaše metriky budou označené `VS Debugger`. Ale pokud je odešlete své změny do Azure, Azure bude najít a používat `environment` aplikace radši nastavení v konfiguraci webové aplikace a vaše metriky budou označené `Production`.
3. Potvrďte a odešlete své změny kódu do vaší větve na Githubu a potom počkejte, než pro uživatele, aby používali nové aplikace (třeba aktualizujte stránku prohlížeče). Jak dlouho trvá asi 15 minut pro novou vlastnost objeví ve vaší službě Application Insights `MultiChannelToDo.Web` prostředků.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master
4. Nyní, přejděte na **vlastní události** okno znovu a filtrujte metriky `Environment=Production`. Teď by měla být vidět všechny nové vlastní události v produkčním slotu s Tento filtr.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. Klikněte na tlačítko **Oblíbené** tlačítko Uložit aktuální nastavení Průzkumníku metrik například **vlastní události: produkční**. Můžete snadno přepnout mezi tato zobrazení a zobrazení slotu nasazení později.

   > [!TIP]
   > Ještě účinnější analýzy, vezměte v úvahu [integraci prostředku Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md).
   >
   >

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Přidání značek slotu specifické pro vaše aplikace metriky serveru
Pro úplnost znovu, bude nastavit aplikaci na straně serveru. Na rozdíl od klientské aplikace, které je instrumentovány v jazyce JavaScript je značky slotu specifické pro aplikace serveru instrumentována kódem .NET.

1. Otevřete  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Přidat blok kódu níže, těsně před uzavírací obor názvů složených závorek.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }
2. Opravte chyby překladu názvů přidáním `using` příkazy níže na začátek souboru:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. Kód níže přidejte na začátek `Application_Start()` metoda:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. Potvrďte a odešlete své změny kódu do vaší větve na Githubu a potom počkejte, než pro uživatele, aby používali nové aplikace (třeba aktualizujte stránku prohlížeče). Jak dlouho trvá asi 15 minut pro novou vlastnost objeví ve vaší službě Application Insights `MultiChannelToDo` prostředků.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Aktualizace: Nastavení větev beta
1. Otevřete  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json a najít `appsettings` prostředky (vyhledejte `"name": "appsettings"`). Existují 4 z nich, jeden pro každou slot.
2. Pro každou `appsettings` prostředků, přidejte `"environment": "[parameters('slotName')]"` nastavení aplikace nastavte na konci `properties` pole. Nezapomeňte si ukončení předchozí řádek oddělujte čárkami.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    Jste právě přidali `environment` nastavení aplikace nastavte na všechny sloty v šabloně.
3. Ve stejném souboru najít `slotconfignames` prostředky (vyhledejte `"name": "slotconfignames"`). Existují 2 z nich, jeden pro každou aplikaci.
4. Pro každou `slotconfignames` prostředků, přidat `"environment"` na konec `appSettingNames` pole. Nezapomeňte si ukončení předchozí řádek oddělujte čárkami.

    Právě jste provedli `environment` aplikace nastavení Flash disk do jeho příslušného nasazovací slot pro obě aplikace.  
5. V relaci prostředí Git s příponou stejné skupiny prostředků, které jste používali před spuštěním následujících příkazů.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. Po zobrazení výzvy zadejte stejné SQL databáze přihlašovací údaje jako před. Potom zadejte dotaz, chcete-li aktualizovat skupinu prostředků, `Y`, pak `ENTER`.

    Po dokončení skriptu, se zachovají všechny prostředky v původní skupinu prostředků, ale nový slot s názvem "beta" je vytvořen se stejnou konfiguraci jako slotu "Přípravy", který byl vytvořen v začátku.

   > [!NOTE]
   > Tato metoda vytvoření různých prostředí se liší od metody v [Agile software development službou Azure App Service](app-service-agile-software-development.md). Zde vytvoříte nasazení prostředí s slotů nasazení, kde jako tam vytvořit nasazení prostředí ke skupinám prostředků. Správa prostředí nasazení pomocí skupin prostředků slouží k udržování produkčního prostředí off-limits pro vývojáře, ale není snadné provést testování v produkčním prostředí, které můžete provést snadno sloty.
   >
   >

Pokud chcete, můžete také vytvořit aplikaci alfa spuštěním

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

V tomto kurzu jste se právě zachovat pomocí beta verzi aplikace.

## <a name="update-push-your-updates-to-the-beta-app"></a>Aktualizace: Nabízená vaše aktualizace aplikace beta
Zpět do aplikace, kterou chcete vylepšit.

1. Ujistěte se, že nyní jste v větev beta

        git checkout beta
2. V  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, Najít `<li>` značky a přidat `style="cursor:pointer"` atributu, jak je uvedeno níže.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. potvrzení a push na Azure.
4. Ověřte, že změny teď se ve slotu beta přechodem na http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Pokud nevidíte změnu ještě, aktualizujte webový prohlížeč k získání nového kódu javascript.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Teď, když máte změny spuštěna ve slotu beta, jste připravení na provedení flighting nasazení.

## <a name="validate-route-traffic-to-the-beta-app"></a>Ověření: Směrovat provoz beta verze aplikace
V této části bude směrovat provoz beta verzi aplikace. For sake of přehlednost ukázku budete ho směrovat podstatnou část provozu generovaného uživateli. Ve skutečnosti objem provozu, kterou chcete směrovat bude záviset na konkrétní situaci. Například pokud váš webový server je ve velkém měřítku Microsoft.com, pak musíte méně než jeden procent celkového provozu s cílem získat užitečné data.

1. V relaci prostředí Git spusťte následující příkazy polovinu produkční provoz směrovat na beta slot:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   `ReroutePercentage=50` Vlastnost určuje, že 50 % produkční provozu budou směrované na adresu URL aplikace beta (určená elementem `ActionHostName` vlastnost).
2. Nyní přejděte na http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50 % provozu teď přesměrovat na beta slot.
3. V prostředku Application Insights, filtrovat podle prostředí metriky = "beta".

   > [!NOTE]
   > Pokud toto filtrované zobrazení uložit jako oblíbenou jiný, můžete snadno překlopit zobrazení metriky explorer mezi produkčního prostředí a zobrazení beta.
   >
   >

Předpokládejme, že ve službě Application Insights byste vidět něco podobného jako následující:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Ne jenom se to zobrazuje, že jsou na mnoho další kliknutí `<li>` značky, ale nejspíš nárůst kliknutí na `<li>` značky. Můžete pak dokončete, že lidé mají zjišťuje nové `<li>` značky kliknout a jsou nyní zrušíte všechny dříve dokončit úkoly v aplikaci.

Na základě dat flighting nasazení, je rozhodnout, že je připravená pro vydání nové uživatelské rozhraní.

## <a name="go-live-move-your-new-code-into-production"></a>Přejděte za provozu: Přesunutí váš nový kód do výroby
Nyní jste připravení přejít aktualizace do produkčního prostředí. Co je skvělým je, že nyní víte, že aktualizace již byl ověřen *před* je vložena do produkčního prostředí. Takže teď můžete ho můžou s jistotou nasazovat. Vzhledem k tomu, že jste provedli aktualizaci do klientské aplikace AngularJS, pouze ověřit kód straně klienta. Pokud byste chtěli změnit back-end aplikačním webového rozhraní API, může ověřit změny, snadno a podobně.

1. V prostředí Git odeberte pravidlo směrování provozu tak, že spustíte následující příkaz:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. Spusťte příkazy Git:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. Počkejte několik minut, než nový kód pro nasazení do přípravný slot a pak spusťte http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net k ověření, že je nová aktualizace jestli v přípravný slot. Nezapomeňte, že vaše rozvětvení hlavní větve je propojený s přípravný slot vaší aplikace.
4. Nyní swap přípravný slot na produkční

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Souhrn
Aplikační služba Azure lze snadno pro malé - pro střední firmy něco otestovat jejich zákazníků směřujících aplikací v provozním prostředí, které tradičně provedeno ve velkých podnicích. Doufáme v tomto kurzu jste obdrželi znalosti, že potřebujete shromáždit služba App Service a Application Insights, aby možné flighting nasazení a i další testu v produkčních scénářích vaší DevOps světě.

## <a name="more-resources"></a>Další zdroje informací
* [Agile software development službou Azure App Service](app-service-agile-software-development.md)
* [Nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)
* [Nasazení aplikace komplexní předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - validátor JSON](http://jsonlint.com/)
* [Vytvoření větve Git – základní větvení a slučování](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Azure PowerShell](/powershell/azure/overview)
* [Wiki Kudu projektu](https://github.com/projectkudu/kudu/wiki)
