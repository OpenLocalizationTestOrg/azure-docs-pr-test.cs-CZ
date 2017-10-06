---
title: "nasazení aaaFlighting (testování verze beta) v Azure App Service"
description: "Zjistěte, jak tooflight nové funkce v aplikaci nebo beta otestovat vaše aktualizace v tomto kurzu začátku do konce. Spojuje služby App Service funkcí, jako je nepřetržitá publikování, sloty, směrování provozu a integrace Application Insights."
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
ms.openlocfilehash: e83477b1fe46be09e5baa7bc2bd239b840b05cf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Flighting nasazení (testování verze beta) v Azure App Service
Tento kurz ukazuje, jak toodo *flighting nasazení* integrací hello různé možnosti [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) a [Azure Application Insights](/services/application-insights/).

*Flighting* je proces nasazení, která ověřuje nové funkce nebo s omezený počet zákazníků skutečné změnit, a hlavní testování v produkční scénář. Je podobná toobeta testování a se někdy označuje jako "řízené testovací letu". Tuto metodu použijte, mnoho velkých podnicích se webová služba získat časná ověření na jejich aktualizace aplikací v jejich postup [agile vývoj](https://en.wikipedia.org/wiki/Agile_software_development). Aplikační služba Azure vám umožní toointegrate testování v produkčním prostředí s publikováním nepřetržité nebo Application Insights tooimplement hello stejné DevOps scénář. Výhody tohoto přístupu patří:

* **Získání zpětné vazby skutečné *před* aktualizace jsou vydané tooproduction** -hello pouze jedinou lepší, než získají zpětnou vazbu při uvolnění je získání zpětné vazby před uvolněním. Aktualizace s přenosy reálný uživatel a chování můžete otestovat časná požadavky hello produktu životního cyklu.
* **Vylepšení [průběžné testy řízený vývoj (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  – díky integraci testování v produkčním prostředí se průběžnou integraci a instrumentace s Application Insights, ověření uživatele se již v rané fázi stane a automaticky v životního cyklu produktu. To pomáhá zkrátit čas investice do spuštění manuálního testu.
* **Optimalizace pracovního postupu testování** -automatizací testování v produkčním prostředí s nepřetržité monitorování instrumentace můžete potenciálně provést hello cílů různé druhy testů v jednom procesu, jako například [integrace](https://en.wikipedia.org/wiki/Integration_testing), [regrese](https://en.wikipedia.org/wiki/Regression_testing), [použitelnost](https://en.wikipedia.org/wiki/Usability_testing), usnadnění, lokalizace, [výkonu](https://en.wikipedia.org/wiki/Software_performance_testing), [zabezpečení](https://en.wikipedia.org/wiki/Security_testing), a [ přijetí](https://en.wikipedia.org/wiki/Acceptance_testing).

Flighting nasazení není jenom o směrování provoz za provozu. Při takovémto nasazení budete chtít toogain přehledy co nejrychleji, ať to neočekávané chyby, snížení výkonu, problémy uživatelského rozhraní. Mějte na paměti, kterou chcete pracovat se skutečnou zákazníků. Ano toodo ho práva, ujistěte se, že jste nastavili tak vaše flighting toogather nasazení všechna data hello budete potřebovat v pořadí toomake informované rozhodnutí pro další krok. Tento kurz ukazuje, jak toocollect dat pomocí Application Insights, ale můžete použít New Relic ani jiné technologie, které vyhovuje vašemu scénáři.

## <a name="what-you-will-do"></a>Co provedete
V tomto kurzu se dozvíte, jak toobring hello následující scénáře společně tootest aplikaci aplikační služby v produkčním prostředí:

* [Směrování provozu produkční](app-service-web-test-in-production-get-start.md) tooyour beta verze aplikace
* [Instrumentace aplikace](../application-insights/app-insights-web-track-usage.md) tooobtain užitečné metriky
* Nepřetržitě nasazení vaší aplikace beta a sledovat metriky za provozu aplikace
* Metriky porovnání mezi hello produkční aplikace a hello beta aplikace toosee jak kód změní převede tooresults

## <a name="what-you-will-need"></a>Co budete potřebovat
* Účet Azure
* A [Githubu](https://github.com/) účtu
* Visual Studio 2015 - si můžete stáhnout hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
* Git prostředí (nainstalované s [GitHub pro Windows](https://windows.github.com/)) – díky tomu můžete toorun obou hello Git a prostředí PowerShell příkazy v hello stejné relace
* Nejnovější [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits
* Základní znalosti o hello následující:
  * [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení šablony (viz [nasazení komplexní aplikace předvídatelné v Azure](app-service-deploy-complex-application-predictably.md))
  * [Git](http://git-scm.com/documentation)
  * [PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> Je třeba účtu Azure toocomplete v tomto kurzu:
>
> * Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/) -získáte kredity, můžete použít tootry na placené služby Azure a i po jejich použití můžete ponechat hello účet a používat bezplatné služby Azure, jako jsou webové aplikace.
> * Můžete [aktivovat výhody předplatitele Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -si Visual Studio předplatné vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.
>
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a name="set-up-your-production-web-app"></a>Nastavení webové aplikace produkční
> [!NOTE]
> skript Hello použili v tomto kurzu bude automaticky nakonfigurovat nepřetržité publikování z úložiště Githubu. To vyžaduje, aby vaše přihlašovací údaje Githubu jsou již uloženy v Azure, jinak hello skripty nasazení selže při pokusu o nastavení ovládacího prvku tooconfigure zdroje pro hello webové aplikace.
>
> toostore vaše Githubu přihlašovací údaje v Azure, vytvořit webovou aplikaci v hello [portálu Azure](https://portal.azure.com/) a [konfigurace nasazení Githubu](app-service-continuous-deployment.md). Potřebujete jenom toodo tomto jednou.
>
>

V případě typické DevOps máte aplikaci, která běží v Azure za provozu a chcete toomake tooit změny prostřednictvím průběžné publikování. V tomto scénáři nasadíte tooproduction šablonu, která mají vývoji a testování.

1. Vytvoření vlastního rozvětvení hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) úložiště. Informace o vytváření vašeho rozvětvení najdete v tématu [rozvětvovat úložišti](https://help.github.com/articles/fork-a-repo/). Po vytvoření vaší rozvětvení, zobrazí se v prohlížeči.

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. Otevřete relaci prostředí Git. Pokud ještě nemáte Git prostředí, nainstalujte [GitHub pro Windows](https://windows.github.com/) nyní.
3. Vytvořte místní vaší rozvětvení spuštěním hello následující příkaz:

     https://github.com/<your_fork>/ToDoApp.git klon Git
4. Až budete mít vaše místní klonování, přejděte příliš*&lt;repository_root >*\ARMTemplates a spusťte hello deploy.ps1 skript s jedinečnou příponu, jak je uvedené níže:

     .\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix >
5. Po zobrazení výzvy zadejte hello požadované uživatelské jméno a heslo pro přístup k databázi. Mějte na paměti, protože je budete potřebovat toospecify přihlašovací údaje databáze je znovu při aktualizaci hello skupinu prostředků.

   Měli byste vidět hello zřizování průběh různých prostředků Azure. Po dokončení nasazení hello skript spuštění aplikace hello v prohlížeči hello a získáte popisný zvukový signál.
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. Zpět v relaci prostředí Git, spusťte tento příkaz:

     . \swap – název ToDoApp < your_suffix >

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. Po dokončení skriptu hello vraťte toobrowse toohello front-end adresa (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) toosee hello aplikace běžící v produkčním prostředí.
8. Přihlaste se k hello [portálu Azure](https://portal.azure.com/) a prohlédněte si co je vytvořen.

   Musí být schopný toosee dva webové aplikace v hello stejnou skupinu prostředků, s hello `Api` přípony v názvu hello. Pokud se podíváte na zobrazení skupiny prostředků hello, zobrazí se také hello SQL Database a serveru, hello plán služby App Service a hello přípravné sloty pro hello webové aplikace. Procházet hello různých prostředků a porovnejte je s  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee, jak jsou nakonfigurované v šabloně hello.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Jste nastavili hello produkční aplikaci.  Nyní Pojďme Představte si, že nedostanete zpětnou vazbu, že je špatná aplikace hello použitelnost. Proto rozhodnete tooinvestigate. Budete tooinstrument toogive vaší aplikaci můžete zpětnou vazbu.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Prozkoumat: Instrumentace vaší klientské aplikace pro monitorování/metriky
1. Otevřete  *&lt;repository_root >*\src\MultiChannelToDo.sln v sadě Visual Studio.
2. Obnovení všech balíčků Nuget kliknutím pravým tlačítkem na řešení > **spravovat balíčky NuGet pro řešení** > **obnovení**.
3. Klikněte pravým tlačítkem na **MultiChannelToDo.Web** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků tooToDoApp*&lt;your_suffix >* > **přidat službu Application Insights tooProject**.
4. V hello portálu Azure, otevřete okno hello hello **MultiChannelToDo.Web** aplikace přehled prostředků. Potom v hello **stavu aplikací** součástí, klikněte na tlačítko **zjistěte, jak toocollect prohlížeče stránku načíst data** > zkopírujte kód.
5. Přidat hello zkopírovat kód instrumentace JS příliš*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml těsně před uzavírací hello `<heading>` značky. Měl by obsahovat klíč instrumentace jedinečný hello prostředku přehled aplikace.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. Odeslat vlastní události tooApplication Insights pro kliknutí myší přidáním následující kód toohello dolní části textu hello:

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   Tento fragment kódu JavaScript odešle vlastní události tooApplication Statistika pokaždé, když uživatel klikne kdekoli v hello webové aplikace.
7. V prostředí Git potvrďte a odešlete rozvětvení tooyour vaše změny v Githubu. Potom počkejte, než klienti toorefresh prohlížeče.

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. Swap hello nasadit tooproduction změny aplikace:

     . \swap – název ToDoApp < your_suffix >
9. Vyhledejte prostředek Application Insights toohello, který jste nakonfigurovali. Klikněte na tlačítko Vlastní události.

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   Pokud nevidíte metriky pro vlastní události, počkejte několik minut a klikněte na tlačítko **aktualizovat**.

Předpokládejme, že se zobrazí graf jako níže:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

A mřížky hello událostí pod ním:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Podle kódu aplikace ToDoApp tooyour, hello **tlačítko** události odpovídá tlačítko Odeslat toohello a hello **vstup** události odpovídá toohello textové pole. Zatím věcí smysl. Ale to vypadá, je dobré množství kliknutí a jen několik kliknutí položkami seznamu úkolů hello (hello **LI** událostí).

Na základě toho je formuláře vaše předpoklad, že někteří uživatelé nerozumíte kterou část hello uživatelského rozhraní je můžete kliknout a je to proto kurzor hello je navržen pro výběr textu ho na hello položky seznamu a jejich ikony vždy, když se.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

To může být contrived příklad. Nicméně jste probíhající toomake zlepšování tooyour aplikace a pak proveďte flighting nasazení tooget použitelnosti odezva od zákazníků za provozu.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Aplikace serveru pro monitorování/metriky instrumentace
To je tangens, protože hello scénář ukázáno v tomto kurzu pouze zabývá hello klientskou aplikaci. Ale pro úplnost nastavíte hello serverové aplikace.

1. Klikněte pravým tlačítkem na **MultiChannelToDo** > **přidat Telemetrii Application Insights** > **konfigurovat nastavení** > změnit skupinu prostředků tooToDoApp*&lt;your_suffix >* > **přidat službu Application Insights tooProject**.
2. V prostředí Git potvrďte a odešlete rozvětvení tooyour vaše změny v Githubu. Potom počkejte, než klienti toorefresh prohlížeče.

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. Swap hello nasadit tooproduction změny aplikace:

     . \swap – název ToDoApp < your_suffix >

A to je vše!

## <a name="investigate-add-slot-specific-tags-tooyour-client-app-metrics"></a>Prozkoumat: Přidáte značky specifické pro slot tooyour klienta aplikace metriky
V této části budete konfigurovat hello jiné nasazení sloty toosend specifické pro slot telemetrie toohello stejný prostředek Application Insights. Tímto způsobem můžete porovnat telemetrická data mezi přenosy dat z různých sloty (nasazení prostředí) tooeasily najdete v části hello účinku změny aplikace. Na hello stejný čas, abyste mohli pokračovat toomonitor produkční aplikace podle potřeby můžete oddělit provoz produkční hello z hello rest.

Vzhledem k tomu, že shromažďování dat na chování klienta bude [přidat telemetrie inicializátoru tooyour kódu jazyka JavaScript](../application-insights/app-insights-api-filtering-sampling.md) v index.cshtml. Pokud chcete výkonu na straně serveru tootest, například můžete provést také podobně jako v kódu serveru (viz [Application Insights API pro vlastní události a metriky](../application-insights/app-insights-api-custom-events-metrics.md).

1. Nejprve přidejte hello kód bewteen hello dva `//` komentáře níže v bloku hello jazyka JavaScript, zda jste přidali toohello `<heading>` dříve značky.

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

    Tento kód inicializátoru způsobí, že hello `appInsights` tooadd objekt hello vlastní vlastnost s názvem `Environment` tooevery část telemetrie odešle.
2. Dál přidejte tuto vlastní vlastnost jako [pozice nastavení](web-sites-staged-publishing.md#AboutConfiguration) pro webovou aplikaci v Azure. toodo se spuštění hello následující příkazy v relaci prostředí Git.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Hello Web.config v projektu již definuje hello `environment` nastavení aplikace. Při tomto nastavení se při testování hello aplikaci místně, vaše metriky budou označené `VS Debugger`. Ale po stisknutí tooAzure vaše změny, Azure bude nalezne a použije hello `environment` aplikace nastavení v konfiguraci webové aplikace hello místo a vaše metriky budou označené `Production`.
3. Potvrzení a váš kód změny tooyour rozvětvení push na Githubu a potom počkejte, než pro uživatele toouse hello novou aplikaci (třeba toorefresh hello prohlížeč). Trvá asi 15 minut pro novou vlastnost tooshow hello v Application Insights `MultiChannelToDo.Web` prostředků.

        git add -A :/
        git commit -m "add environment property tooAI events for client app"
        git push origin master
4. Nyní přejděte toohello **vlastní události** okno znovu a filtr hello metriky na `Environment=Production`. Nyní byste měli mít toosee všechny hello nové události vlastní v produkčním prostředí hello pozici s Tento filtr.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. Klikněte na tlačítko hello **Oblíbené** jako tlačítko toosave hello aktuální Průzkumníku metrik nastavení toosomething **vlastní události: produkční**. Můžete snadno přepnout mezi tato zobrazení a zobrazení slotu nasazení později.

   > [!TIP]
   > Ještě účinnější analýzy, vezměte v úvahu [integraci prostředku Application Insights do Power BI](../application-insights/app-insights-export-power-bi.md).
   >
   >

### <a name="add-slot-specific-tags-tooyour-server-app-metrics"></a>Přidání značky specifické pro slot tooyour serveru aplikace metriky
Znovu pro úplnost nastavíte hello serverové aplikace. Na rozdíl od hello klientskou aplikaci, která je instrumentovány v jazyce JavaScript je značky slotu specifické pro aplikace server hello instrumentována kódem .NET.

1. Otevřete  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Přidejte hello blok kódu níže, těsně před hello zavřením obor názvů složených závorek.

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
2. Opravte chyby překladu názvů hello přidáním hello `using` příkazy níže toohello začátku hello souboru:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. Přidat kód hello pod toohello začátku hello `Application_Start()` metoda:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. Potvrzení a váš kód změny tooyour rozvětvení push na Githubu a potom počkejte, než pro uživatele toouse hello novou aplikaci (třeba toorefresh hello prohlížeč). Trvá asi 15 minut pro novou vlastnost tooshow hello v Application Insights `MultiChannelToDo` prostředků.

        git add -A :/
        git commit -m "add environment property tooAI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Aktualizace: Nastavení větev beta
1. Otevřete  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json a najít hello `appsettings` prostředky (vyhledejte `"name": "appsettings"`). Existují 4 z nich, jeden pro každou slot.
2. Pro každou `appsettings` prostředků, přidejte `"environment": "[parameters('slotName')]"` aplikace nastavení toohello konec hello `properties` pole. Nezapomeňte tooend hello předchozí řádek oddělujte čárkami.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    Jste právě přidali hello `environment` aplikace nastavení tooall hello sloty v šabloně hello.
3. V hello stejného souboru, najde hello `slotconfignames` prostředky (vyhledejte `"name": "slotconfignames"`). Existují 2 z nich, jeden pro každou aplikaci.
4. Pro každou `slotconfignames` prostředků, přidat `"environment"` toohello konec hello `appSettingNames` pole. Nezapomeňte tooend hello předchozí řádek oddělujte čárkami.

    Právě jste provedli hello `environment` slot nasazení příslušné aplikace nastavení Flash disk tooits pro obě aplikace.  
5. V relaci prostředí Git hello spusťte následující příkazy pomocí hello stejnou příponu skupiny prostředků, kterou jste používali před.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. Po zobrazení výzvy zadejte hello stejné přihlašovací údaje databáze SQL jako předtím. Poté, jakmile se zobrazí dotaz tooupdate, hello skupinu prostředků, typ `Y`, pak `ENTER`.

    Po dokončení skriptu hello, všechny prostředky ve skupině prostředků původní hello se zachovají, ale nový slot s názvem "beta" je vytvořen v ní s hello stejné konfigurace jako slotu "Přípravy" hello, který byl vytvořen v začátku hello.

   > [!NOTE]
   > Tato metoda vytvoření různých prostředí se liší od metody hello v [Agile software development službou Azure App Service](app-service-agile-software-development.md). Zde vytvoříte nasazení prostředí s slotů nasazení, kde jako tam vytvořit nasazení prostředí ke skupinám prostředků. Správa prostředí nasazení pomocí skupin prostředků vám umožní off-limits toodevelopers tookeep hello produkční prostředí, ale není snadno toodo testování v produkčním prostředí, které můžete provést snadno sloty.
   >
   >

Pokud chcete, můžete také vytvořit aplikaci alfa spuštěním

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

V tomto kurzu jste se právě zachovat pomocí beta verzi aplikace.

## <a name="update-push-your-updates-toohello-beta-app"></a>Aktualizace: Aplikace aktualizace toohello beta Push
Zpět tooyour aplikace, které chcete tooimprove.

1. Ujistěte se, že nyní jste v větev beta

        git checkout beta
2. V  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, najít hello `<li>` značky a přidejte hello `style="cursor:pointer"` atributu, jak je uvedeno níže.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. Potvrďte a odešlete tooAzure.
4. Ověřte, že hello změny teď se ve slotu beta hello přechodem toohttp://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Pokud nevidíte hello změnit ještě aktualizujte prohlížeč tooget hello nového kódu jazyka javascript.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Teď, když máte změny spuštěna ve slotu hello beta, jste připravené tooperform flighting nasazení.

## <a name="validate-route-traffic-toohello-beta-app"></a>Ověření: Směrování provozu toohello beta verze aplikace
V této části bude směrovat provoz toohello beta verzi aplikace. For sake of přehlednost ukázku budete tooroute podstatnou část tooit provoz hello uživatele. Ve skutečnosti hello objem provozu, který má být, že tooroute bude záviset na konkrétní situaci. Například pokud váš webový server je ve velkém měřítku hello Microsoft.com, pak musíte méně než jeden procent celkového provozu v pořadí toogain užitečné data.

1. V relaci prostředí Git spusťte následující příkazy tooroute hello polovinu hello produkční provoz toohello beta slot:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   Hello `ReroutePercentage=50` vlastnost určuje, že 50 % hello produkční přenosů bude adresa URL aplikace beta směrované toohello (určeného hello `ActionHostName` vlastnost).
2. Nyní přejděte toohttp://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50 % hello přenosů by teď měly být přesměrovaného toohello beta slot.
3. V prostředku Application Insights, filtrovat podle prostředí metriky hello = "beta".

   > [!NOTE]
   > Pokud toto filtrované zobrazení uložit jako oblíbenou jiný, můžete snadno překlopit zobrazení metriky explorer hello mezi produkčního prostředí a zobrazení beta.
   >
   >

Předpokládejme, že ve službě Application Insights se zobrazí něco podobné toohello následující:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Ne jenom se to zobrazuje, že existuje mnoho další kliknutí na hello `<li>` značky, ale nejspíš toobe nárůst kliknutí na `<li>` značky. Můžete pak dokončete, že lidé byly zjištěny hello nové `<li>` značky kliknout a jsou nyní zrušíte všechny dříve dokončit úkoly v aplikaci hello.

Na základě hello dat flighting nasazení, je rozhodnout, že je připravená pro vydání nové uživatelské rozhraní.

## <a name="go-live-move-your-new-code-into-production"></a>Přejděte za provozu: Přesunutí váš nový kód do výroby
Můžete se teď připravena toomove vaše tooproduction aktualizace. Co je skvělým je, že nyní víte, že aktualizace již byl ověřen *před* tooproduction je vložena. Takže teď můžete ho můžou s jistotou nasazovat. Vzhledem k tomu, že jste provedli aktualizaci toohello AngularJS klienta aplikace, pouze ověření kódu na straně klienta hello. Pokud byste byli toomake změny toohello back endové webové rozhraní API aplikace, může ověřit změny, snadno a podobně.

1. V prostředí Git odeberte pravidlo směrování provozu hello spuštěním hello následující příkaz:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. Spusťte příkazy Gitu hello:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. Počkejte několik minut, než hello nové code toohello toobe nasazený přípravné slot a pak spusťte http://ToDoApp*&lt;your_suffix >*-tooverify staging.azurewebsites.net, který hello nové aktualizace je provozní teplotu v pracovní hello slot. Mějte na paměti, že hello vaší rozvětvení hlavní větve je propojená toohello pracovní pozici vaší aplikace.
4. Nyní Prohodit hello pracovní pozici v produkčním prostředí

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Souhrn
Aplikační služba Azure usnadňuje malá toomedium velké firmy tootest jejich zákazníků směřujících aplikací v provozním prostředí, něco byla tradičně udělat ve velkých podnicích. Doufáme, v tomto kurzu jste obdrželi hello znalostní báze, budete potřebovat toobring společně služby App Service a Application Insights toomake možné flighting nasazení a i další testu v produkčních scénářích vaší DevOps světě.

## <a name="more-resources"></a>Další zdroje informací
* [Agile software development službou Azure App Service](app-service-agile-software-development.md)
* [Nastavení přípravných prostředí pro webové aplikace v Azure App Service](web-sites-staged-publishing.md)
* [Nasazení aplikace komplexní předvídatelné v Azure](app-service-deploy-complex-application-predictably.md)
* [Vytváření šablon Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
* [JSONLint - hello validátoru JSON](http://jsonlint.com/)
* [Vytvoření větve Git – základní větvení a slučování](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [Azure PowerShell](/powershell/azure/overview)
* [Wiki Kudu projektu](https://github.com/projectkudu/kudu/wiki)
