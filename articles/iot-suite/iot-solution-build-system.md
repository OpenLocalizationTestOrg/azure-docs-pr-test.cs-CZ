---
title: "Příklad MyDriving Azure IoT: sestavení ho | Microsoft Docs"
description: "Vytvořte aplikaci, která je komplexní ukázka postup architektury systému IoT pomocí služby Microsoft Azure, včetně služby Stream Analytics, Machine Learning a Event Hubs."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: c2fcd6ee-3bbe-43d1-a066-dce52cc3a53d
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 06/30/2017
ms.author: harikm
ms.openlocfilehash: c4b19cc76ca11f606ca8af6b0f3277b5aa46ac5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="build-and-deploy-the-mydriving-solution-to-your-environment"></a>Vytváření a nasazování řešení MyDriving pro vaše prostředí
MyDriving je řešení Internetu věcí (IoT), které shromáždí data z vaší car, procesy pomocí machine learning a uvede na váš mobilní telefon. Back-end se skládá z řady služeb Microsoft Azure poskytuje. Klienti mohou být telefony Android, iOS nebo Windows 10.

Jsme vytvořili MyDriving řešení získáte základní informace pro vytváření systému IoT. Z [MyDriving úložišti na Githubu](https://github.com/Azure-Samples/MyDriving), můžete získat skripty Azure Resource Manager k nasazení architektury back-end do vlastní účet Azure. Od tohoto okamžiku můžete znovu nakonfigurovat různé služby, upravovat dotazy a vyhovovala svoje vlastní data a tak dále. Tyto skripty – spolu s kód pro mobilní aplikace, projektu Azure App Service API a další – v úložišti MyDriving můžete najít.

Pokud ještě nebyl proveden o aplikaci, podívejte se na [Příručka Začínáme Get](iot-solution-get-started.md).

Je podrobný účet architektury v [MyDriving referenční příručka](http://aka.ms/mydrivingdocs). V souhrnu existuje několik informací, které jsme nastavení k vytvoření podobných projektu:

* A **klientskou aplikaci** běží na Android, iOS a Windows 10 Phone. Používáme Xamarin platformy sdílet většinu kódu, který je uložený na Githubu v části `src/MobileApp`. Aplikace ve skutečnosti provádí dvě odlišné funkce:
  * Předává telemetrie ze zařízení integrovaného diagnostiky (Diagnostického) a z vlastní umístění služby pro back-endu systému cloudu.
  * Je uživatelské rozhraní, kde můžete dotazovat uživatele o jejich zaznamenaná silniční služebních cest.
* A **Cloudová služba** ingestuje-cestu data v reálném čase a zpracovává je. Hlavní práci při vytváření této služby je zvolte Parametrizace a propojit se různým službám Azure. Některá z částí vyžadovat skripty, které proces příchozích dat a filtru. Nakonfigurujte všechny části používáme šablonu Azure Resource Manager.
* A **mobilní služby App Service** je webová služba za části uživatelské rozhraní aplikace pro zařízení. Hlavní úlohy je dotaz na databázi uložené, zpracovaná data. Jeho kód je na Githubu v části `src/MobileAppService`.
* **Visual Studio s Xamarinem** je naše vývojové prostředí. Xamarin, která již existuje jako součást sady Visual Studio i jako samostatné integrované vývojové prostředí (IDE), slouží k vytvoření kód napříč platformami zařízení. Pokud chcete vytvořit kód iOS, je potřeba mít instanci Xamarin spuštěná na počítači OS X. V případě potřeby ho můžete spustit jako agenta, spravované ze sady Visual Studio.
* **Testování částí** zařízení, aplikace se provádí v Xamarin Test Cloud.
* **GitHub** slouží jako úložiště, kde ukládáme kód, skriptů a šablon.
* **Visual Studio Team Services** je Cloudová služba, která se používá ke správě průběžné sestavení a testování aplikace webové služby a zařízení.
* **HockeyApp** slouží k distribuci verzích kód zařízení. Shromažďuje taky selhání a používání sestav a zpětnou vazbu od uživatelů.
* **Visual Studio Application Insights** monitoruje mobilní webové služby.

Ano Podíváme se, jak jsme nastavit všechny této. 

> [!NOTE] 
> Mnoho z těchto kroků jsou volitelné.
>
>

## <a name="sign-up-for-accounts"></a>Zaregistrujte si účty
* [Visual Studio Dev Essentials](https://www.visualstudio.com/products/visual-studio-dev-essentials-vs.aspx). Tento program volné poskytuje snadný přístup k mnoha nástrojů pro vývojáře a služeb, včetně Visual Studio, Visual Studio Team Services a Azure. Nabízí platební 25/ měsíc na platformě Azure po dobu 12 měsíců. Zahrnuje také odběrů Pluralsight školení a univerzity Xamarin. Můžete také zaregistrujete odděleně pro bezplatné úrovně [Azure](https://azure.com) a [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx), ale tyto neposkytují kredity Azure.
* [HockeyApp](https://rink.hockeyapp.net/) (volitelné), pro správu testovací distribuci mobilních aplikací a shromažďování telemetrie.
* [Xamarin](https://xamarin.com/) (povinné), pro vytváření mobilních aplikací a spouštění spustí ladění a testy na [Xamarin Test Cloud](https://xamarin.com/test-cloud).
* [GitHub](https://github.com/Azure-Samples/MyDriving/) (volitelné), chcete-li vytvořit volné veřejného úložiště pro vlastní kód (privátní úložiště jsou placené). Alternativně můžete vytvořit základní plán ve Visual Studio Team Services pro privátní úložiště.
* [Power BI](https://powerbi.microsoft.com/) (volitelné), chcete-li vytvořit bohatých vizualizací služby data v celém systému.

> [!NOTE]
> Nepotřebujete Githubu účet pro přístup kód MyDriving v [úložišti GitHub MyDriving](https://github.com/Azure-Samples/MyDriving).
> 
> 

## <a name="install-development-tools"></a>Instalace nástrojů pro vývoj
Pro vývoj úplné řešení je následující nastavení: iOS, Android a Windows 10 Mobile multiplatformní aplikace pomocí Azure back-end.

Jako alternativu, můžete použít Xamarin Studio na Mac nebo Windows pro vývoj mobilních aplikací, pokud nepracujete v Azure back-end.

Je [delší popis tato instalace](https://msdn.microsoft.com/library/mt613162.aspx).

### <a name="windows-development-machine"></a>Vývoj pro počítač systému Windows
Centrální nástroj v systému Windows je Visual Studio pro práci s MyDriving aplikace pro Android a Windows, App Service API projektů a rozšíření mikroslužby.

Xamarin, Git, emulátorů a další užitečné součásti jsou všechny integrované pomocí sady Visual Studio.

Instalace:

* [Visual Studio s Xamarinem](https://www.visualstudio.com/products/visual-studio-community-vs) (všechny edice – komunity je bezplatná).
* [SQLite pro univerzální platformu Windows](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936). Abyste mohli sestavit kód Windows 10 Mobile.
* [Azure SDK pro Visual Studio](https://www.visualstudio.com/vs/azure-tools/). Poskytuje sadu SDK pro aplikace spuštěné v Azure, společně s příkazového řádku nástroje pro správu Azure.
* [Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric). Vyžaduje k sestavení [mikroslužbu](../service-fabric/service-fabric-get-started.md) rozšíření.

Ujistěte se, že máte správné rozšíření Visual Studia. Zkontrolujte, že v části **nástroje**, uvidíte **Android, iOS, Xamarin...** . Pokud ne, otevřete Visual Studio, vyhledejte Xamarin a postupujte podle pokynů a nainstalujte ji. Zkontrolujte také, který **Git pro Windows** je nainstalovaná. Pokud ne, v sadě Visual Studio, vyhledejte je a postupujte podle pokynů a nainstalujte ji. 

### <a name="mac-development-machine"></a>Vývoj pro počítače Mac
Mac (Yosemite nebo novější) je povinný, pokud chcete k vývoji pro iOS. Přestože používáme Visual Studio s Xamarinem v systému Windows pro vývoj a správu všech kód, používá Xamarin agenta nainstalovaného v systému Mac pro sestavení a podepsání kódu iOS.

![Vývoj v systému Windows a sestavení v systému Mac](./media/iot-solution-build-system/image1.png)

(Alternativně můžete Xamarin Studio přímo na Mac k vývoji aplikací pro různé platformy.)

Mac není nutný, pokud nechcete, aby se tak rozšiřuje jako cílové platformy.

Instalace:

* [Xamarin Studio pro iOS](https://developer.xamarin.com/guides/ios/getting_started/installation/mac/). Můžete také nastavit Visual Studio a Xamarin, na kterém běží virtuální počítač Windows Macu. V tématu [nastavení, instalaci a ověření pro uživatele Mac](https://msdn.microsoft.com/library/mt488770.aspx) na webu MSDN.
* [Nástroje pro vývoj Azure](https://azure.microsoft.com/downloads/) (volitelné).

Povolit vzdálené přihlášení v Mac. Otevřete **předvolbách systému** > **sdílení**a potom vyberte **vzdálené přihlášení**.

Při otevření projektu iOS v sadě Visual Studio v systému Windows, modul plug-in Xamarin zobrazí výzvu k zadání ID Mac.

## <a name="fetch-the-github-repository"></a>Načtení úložiště GitHub
Načtení místní kopii [úložišti GitHub MyDriving](https://github.com/Azure-Samples/MyDriving) pomocí **stáhnout ZIP** tlačítka na GitHub, Visual Studio nebo jiného klienta Git.

Rozbalte soubor do složky s krátký název cesty, jako je například C:\\kódu.

Případně pokud chcete zachovat aktuální s nebo podílet se na našem kódu, klonovat úložiště následujícím způsobem:

**https://github.com/Azure-Samples/MyDriving.git klon Git**

## <a name="get-a-bing-maps-api-key"></a>Získat klíč rozhraní API map Bing
[Registrace pro klíč rozhraní API map Bing](https://msdn.microsoft.com/library/ff428642.aspx).

Budete muset nahraďte ho v řádku 22 `src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs`.

## <a name="build-the-demo-app"></a>Vytvoření ukázkové aplikace
Otevření těchto řešení v sadě Visual Studio:

* src\MobileApps\MyDriving.sln
* src\MobileAppService\MyDrivingService.sln
* src\Extensions\ServiceFabric\VINLookUpApplication\VINLookUpApplication.sln

Získáte pokynů:

* Důvěřujete některé potenciálně nedůvěryhodných projekty. Vyberte k jejich otevření, pokud chcete přejít k tématu.
* Nastavte režim vývojáře, pokud pracujete na nový počítač s Windows 10.
* Zadejte svoje přihlašovací údaje Xamarin.
* Připojení k Xamarin Mac. Pokud nemáte algoritmu Mac, klikněte pravým tlačítkem na projekt pro iOS v sadě Visual Studio a potom vyberte **uvolnit projekt**.

Znovu sestavte řešení.

Pokud máte potíže při vytváření, zkuste řešení pro adaptivní, které jsme zjistili:

* *Projekt VINLookupApplication nenačte*: Ujistěte se, že jste nainstalovali [Azure SDK pro Visual Studio](https://www.visualstudio.com/vs/azure-tools/).
* *Není sestavení projektu Service Fabric*: sestavení projektů rozhraní nejprve a ujistěte se, že jste nainstalovali Service Fabric SDK.
* *Aplikace pro Android nemá sestavení*:
  
  * Otevřete **nástroje** > **Android** > **Android SDK Manager**a ujistěte se, že Android 6 (API 23) / SDK platformy je nainstalováno.
  * Odstraňte tento adresář a potom ho sestavte znovu:<br/>
    `%LocalAppData%\Xamarin\zips`

## <a name="get-to-know-the-code"></a>Seznámení s kód
V řešení najdete:

* Rozšíření Azure: Service Fabric.
* Azure HDInsight: Skripty pro zpracování dat cesty v Azure.
* Mobilní aplikace: Zařízení aplikací.
* MobileAppsService/MyDrivingService: Ukončení webu zpět.
* Power BI: Definice sestavy.
* Skripty:
  
  * Správce prostředků: šablony k vytváření prostředků Azure.
  * Prostředí PowerShell: Skripty ke spuštění šablony Resource Manageru.
  * Azure SQL Database: Databáze ladění.
* SQL Database: CreateTables: definice schémat.
* Azure Stream Analytics: Dotazy, které transformovat příchozí datový proud.

## <a name="run-the-apps-in-development-mode"></a>Spuštění aplikace v režimu pro vývoj
Proveďte akci pro spuštění aplikace založené na zařízení, které používáte:

* Back-end: Sada MyDrivingService jako spouštěný projekt a stiskněte klávesu F5 ke spouštění back endové webové služby. Otevře se prohlížeč zobrazení seznamu rozhraní API.
* Mobilních klientů: [jsou mobilní aplikace vyvinuté v Xamarin](https://developer.xamarin.com/guides/cross-platform/deployment,_testing,_and_metrics/debugging_with_xamarin/).
  
  * Android: Podrobnosti najdete v tématu [ladění Android v Xamarin](http://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debugging_with_xamarin_android/).
  * iOS: Podrobnosti najdete v tématu [ladění na iOS](http://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/debugging_in_xamarin_ios/).
  * Windows Phone: Podrobnosti najdete v tématu [Xamarin + Windows Phone](https://developer.xamarin.com/guides/cross-platform/windows/phone/).

## <a name="upload-the-mobile-app-to-hockeyapp"></a>Nahrát mobilní aplikaci do HockeyApp
HockeyApp spravuje distribuci Android, iOS nebo Windows aplikaci, kterou chcete testovat uživatele, upozornění uživatelů nové verze. Shromažďuje taky užitečné havárií sestavy, zpětnou vazbu od uživatelů se snímky obrazovky a metriky využití.

[Začněte tím, že odesílání](http://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app) sestavení aplikace. Potom se přihlaste k [HockeyApp](https://rink.hockeyapp.net) z vývojovém počítači. Na řídicím panelu vývojáře, klikněte na tlačítko **novou aplikaci**a pak přetáhněte vytvořené soubory na okno. (Později, je možné automatizovat sestavení služby k tomu.)

Nyní jste v řídicím panelu aplikace.

![Karta Přehled na řídicím panelu aplikací](./media/iot-solution-build-system/image2.png)

Opakujte postup pro každou platformu, která vaše aplikace běží na. Potom můžete provést následující:

* Použití [ID aplikace](http://support.hockeyapp.net/kb/app-management-2/how-to-find-the-app-id) na řídicím panelu odeslat data o chybách a zpětné vazby z vaší aplikace. V MyDriving aktualizujte ID v src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs.
* [Testovací uživatele pozvat](http://support.hockeyapp.net/kb/app-management-2/how-to-invite-beta-testers). Můžete získat adresu URL pro nábor testerům, sada uživatelů. Se budete moct zaregistrovat pro váš tým, stáhněte aplikaci a můžete odeslat zpětnou vazbu.
* Pokud si přejete více otevřete betaverze, nastaven na hodnotu veřejná rozdělení. Klikněte na tlačítko **spravovat aplikaci** > **distribuční** > **stáhnout = veřejné**. Každý, kdo teď aplikaci stáhněte a můžete odeslat zpětnou vazbu a se zobrazí oznámení, když publikujete novou verzi. Některé sestavy havárií mohou být příliš z nich.
  
   ![Týmy na řídicím panelu](./media/iot-solution-build-system/image3.png)
* [Hlášení selhání propojit Visual Studio Team Services](http://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs). Klikněte na tlačítko **spravovat aplikaci** > **Visual Studio Team Services**. HockeyApp můžete automaticky vytvořit pracovní položky v Team Services při hlášení selhání ani při doručení zpětnou vazbu.

Další informace na [HockeyApp lokality](https://hockeyapp.net).

## <a name="test-the-mobile-app-on-xamarin-test-cloud"></a>Testování mobilní aplikace na Xamarin Test Cloud
[Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud/introduction-to-test-cloud/) automatizuje testování uživatelského rozhraní na skutečné zařízení v cloudu. Pomocí rozhraní NUnit zápisu testy, které spuštění aplikace v uživatelském rozhraní.

Pokud chcete používat Xamarin, můžete začlenit [Xamarin.UITests](https://developer.xamarin.com/guides/testcloud/uitest/intro-to-uitest/) SDK do aplikace, která se dodává jako balíčku NuGet. Najdete v ukázkovou aplikaci a má zahrnout, když vytvoříte nové projekty test s Xamarin šablony.

![Kde najít napříč platformami SDK na rozhraní](./media/iot-solution-build-system/image4.png)

Příklad testovacího projektu je součástí aplikace v úložišti. V [MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService), podívejte se do části [src](https://github.com/Azure-Samples/MyDriving/tree/master/src)/MobileApps/[MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileApps/MyDriving)/MyDriving.UITests/.

Pokud používáte Visual Studio Team Services sestavení, je snadné zápis testů částí uživatelského rozhraní Xamarin a jejich spuštění v rámci vaší sestavení.

## <a name="deploy-azure-services"></a>Nasazení služby Azure
Pokud chcete provést automatické nasazení služby Azure a služby sestavení Team Services, najdete podrobné pokyny v **scripts/README.md**.

Microsoft Azure poskytuje širokou řadu různých služeb, které můžete použít k vytvoření cloudové aplikace. Mnoho lze jednotlivě (třeba služby nebo webové aplikace), ale jsou v jejich nejvhodnější když jste připojen do formuláře jako integrovaný systém používáme v MyDriving.

Je možné vytvořit a ručně propojení služby Azure, ale je mnohem rychlejší a spolehlivější používat Azure Resource Manager šablony. [Správce prostředků](../azure-resource-manager/resource-group-overview.md) automatizuje nasazení řešení prostředky a vytváření propojení mezi nimi.

Zjistíte šablonu pro systém MyDriving v úložišti GitHub pod [skripty nebo ARM](https://github.com/Azure-Samples/MyDriving/tree/master/scripts/ARM). Poskytuje komplexní a stručným zobrazení způsobu vzájemného propojení různých služeb v Naše architektura. Všechny tyto podrobně v objasníme [MyDriving referenční příručka](http://aka.ms/mydrivingdocs), ale dozvíte mnoho právě načtením prostřednictvím samotné šablony.

> [!NOTE]
> Většina Azure služby mají přidružené náklady, v závislosti na cenové úrovně. Pokud jste Azure ještě nepoužívali, můžete [zdarma vyzkoušet](https://azure.microsoft.com/free/). Ale pokud nemáte v úmyslu používat určité součásti v systému MyDriving, nezapomeňte odebrat je účtovány poplatky. V části "Odhadnout provozní náklady" později v tomto článku poskytuje souhrn typické služby výdaje.
> 
> 

### <a name="edit-the-template"></a>Úprava šablony
Chcete-li přizpůsobit vaše nasazení, případně odebrání nepotřebných součásti nebo přidat další, nejdřív zkontrolujte kopie scénáře\_complete.params.json a scénář\_complete.json, ve kterém se má provést změny.

Můžete použít tento scénář\_complete.params.json soubor může změnit různé výchozí hodnoty, jako je například služba SKU nebo typ replikace úložiště, jak je popsáno v následující tabulce. Výchozí hodnoty vyberte možnosti nejnižší náklady.

| **Parametr** | **Popis** | **Výchozí hodnota** |
| --- | --- | --- |
| Centrum IoT SKU |Vrstva pro službu Azure IoT Hub |F1 |
| Typ účtu úložiště |Typ replikace úložiště |Standardní LRS |
| Cíl služby SQL |Spotřeba slotu souběžnosti |OD DW100 |
| Hostování plán SKU |Plán služby pro službu App Service |F1 |

Ve scénáři\_complete.json:

* Vyhledejte "baseName" a změňte jej na název, který preferujete.
* Vyhledejte "Vytvořit". Každý z těchto částí vytvoří prostředek.
* Nastavte sqlServerAdminLogin a sqlServerAdminPassword vhodný hodnoty.
* Před odstraněním oddíl, který vytvoří prostředek, zkontrolujte, zda má závislé objekty vyhledáním názvu jinde v souboru. Všimněte si, že každý oddíl, který vytvoří služba zahrnuje *dependsOn* oddíl, který uvádí jeho závislosti.

Zde je co konfiguruje šablonu. Podrobnosti najdete v [referenční příručka](http://aka.ms/mydrivingdocs).

| **Služba** | **Popis a podrobnosti** |
| --- | --- |
| Účty úložiště |Šablona vytvoří tři účty: |
| -A SQL database, která získává telemetrická agregovaná data ze služby Stream Analytics a slouží jako úložiště zálohování pro tabulky Azure App Service, které zobrazit tato data prostřednictvím koncových bodů rozhraní API. | |
| – Úložiště objekt blob, které shromažďuje historická data z jiné úlohy Stream Analytics, mají být zpracovány v prostředí HDInsight. | |
| -A SQL database, která přijímá výsledky zpracovává HDInsight pro použití s Power BI. | |
| Azure IoT Hub |Vytváří obousměrné připojení k každého připojeného zařízení. V řešení MyDriving mobilní aplikace funguje jako brána pole k odesílání dat do Azure IoT Hub. Azure IoT Hub poté slouží jako vstup do služby Stream Analytics. |
| Azure Event Hubs |Výstup úlohy Stream Analytics, která fronty výstup k rozšíření, které jsou vytvořené pomocí Azure Service Fabric. |
| Azure SQL Data Warehouse | |
| Úlohy Stream Analytics |Připojte vstupů a výstupů pomocí dotazu, který se používá k agregaci jak v reálném čase a historických dat pro aplikaci API služby Azure Machine Learning, rozšíření a Power BI. |
| Pracovní prostor Machine Learning |Zahrnuje experimenty, kódu jazyka R a rozhraní API služby. |
| Azure Data Factory |Naplánované retraining Machine Learning. |
| Hostování plánu Service Fabric |Pro rozšíření. |
| Služby App Service (dále jen "mobilní aplikace") |Hostuje mobilní aplikace API projekt, který poskytuje koncové body pro mobilní aplikace. Kódu rozhraní API se musí nasadit do služby App Service ze sady Visual Studio. |
| Pravidla výstrah |Odešle že e-mailu Pokud odpovědi aplikace indikuje selhání. |
| Application Insights |Pro sledování výkonu rozhraní API v App Service. Budete muset nakonfigurovat připojení v sadě Visual Studio. |
| Azure Key Vault |Pro uložení certifikát webové služby clusteru. |

### <a name="run-the-template"></a>Spustit šablonu
V **scripts/README.md**, existují podrobné pokyny pro spuštění šablony.

Ke zřízení těchto služeb svůj vlastní účet Azure pomocí skriptu, proveďte jednu z následujících akcí:

* Pomocí prostředí PowerShell:
  
  ```
  
  cd scripts/PowerShell;
  deploy.ps1 *location* *resourceGroupName*
  ```
  
  * *umístění* je [umístění Azure](https://azure.microsoft.com/regions/), jako například `North Europe` nebo `West US`. Použití `Get-AzureLocation` najít seznam dostupných umístění.
  * *Název skupiny prostředků* je název, který chcete poskytnout ke skupině, budou všechny prostředky, které patří. Až skončíte s prostředky, můžete je odstranit všechny společně odstraněním této skupiny.
* Spusťte DeploymentScripts/Bash/deploy.sh s Bash.
* Otevřete a sestavte řešení sady Visual Studio DeploymentScripts/VS/DeployARM.sln.

Všimněte si, že pokaždé, když běží šablona vytvoří novou sadu prostředků pomocí nové názvy. Chcete-li odstranit prostředky, přejděte na portál a odstraňte skupinu prostředků.

Pokud z nějakého důvodu selže skript, můžete ji znovu spustit.

Skript vám dává možnost konfigurace průběžnou integraci ve Visual Studio Team Services. Pokud jste nastavili projektu Team Services, budete mít adresu URL: https://yourAccountName.visualstudio.com. Jakmile se zobrazí výzva, zadejte úplnou adresu URL. Můžete jí název nového nebo existujícího projektu Team Services.

## <a name="set-up-build-and-test-definitions-in-visual-studio-team-services"></a>Nastavení sestavení a testování definice v sadě Visual Studio Team Services
Můžeme použít Team Services na tomto projektu většinou pro jeho sestavení a otestovat funkce. Ale také poskytuje podporu vynikající spolupráce, jako je například Správa úloh s kanbanové karty, revize kódu integrovaná s úkoly a Správa zdrojového kódu a ověřované vrácení sestavení. Se integruje se službou a pomocí jiných nástrojů, jako je například Githubu, Xamarin, HockeyApp a samozřejmě Visual Studio. Můžete k němu přístup prostřednictvím webového rozhraní nebo pomocí sady Visual Studio, podle toho, co je pohodlnější v každém okamžiku.

Kroky v definicích sestavení a verze použít modul plug-in služby, které jsou k dispozici v Team Services [Marketplace](https://marketplace.visualstudio.com/VSTS). Kromě základní nástroje, které spouštějí příkazové řádky nebo zkopírujte soubory jsou služby, který vyvolání sestavení Xamarin, Android a jiných dodavatelů a které se připojovat k HockeyApp.

![Možnosti v Team Services sestavení](./media/iot-solution-build-system/image5.png)

### <a name="build-definitions"></a>Definice sestavení
Máme definice sestavení pro každou z hlavních cílů. Máme také rozdíly pro funkce a testování regrese. To nám dává:

* MyDriving.Services (back endové webové aplikace pro mobilní aplikace)
* MyDriving.Xamarin.Android
  
  * MyDriving.Xamarin.Android – funkce
  * Regrese MyDriving.Xamarin.Android
* MyDriving.Xamarin.iOS
  
  * MyDriving.Xamarin.iOS – funkce
  * Regrese MyDriving.Xamarin.iOS
* MyDriving.Xamarin.UWP
  
  * MyDriving.Xamarin.UWP – funkce
  * Regrese MyDriving.Xamarin.UWP

Pokud chcete zobrazit podrobnosti o naše konfigurace, najdete v části 4.7 [MyDriving referenční příručka](http://aka.ms/mydrivingdocs), "Sestavení a verze konfigurace." Jejich postupují stejným způsobem Obecné. Tento skript:

1. Obnoví balíček NuGet. Zkompilovaný kód jsme si zachovat v úložišti, tak, aby byly první kroky každé sestavení obnovit požadované balíčky NuGet.
2. Aktivuje licence. Sestavení se provádí v cloudu, kde budeme potřebovat licenci – konkrétně pro službu sestavení Xamarin – máme aktivace naše licence do aktuálního počítače sestavení. Potom jsme deaktivovat hned potom, aby ji mohli použít na jiném počítači.
3. Sestavení s použitím na příslušnou službu. Používáme sestavení Xamarin pro mobilní aplikace a Visual Studio vytvoří pro back endové webové službě.
4. Sestaví testy.
5. Spustí testy. Jsme spuštění testů mobilní aplikace Xamarin Test Cloud.
6. Publikuje výsledek sestavení do místa.

Aktivační událost pro hlavní sestavení nastavená na průběžnou integraci. Sestavení je tedy spustit při každém kódu se změnami do hlavní větve.

![Rozhraní, kde je aktivační událost nastaven na průběžnou integraci](./media/iot-solution-build-system/image6.png)

### <a name="release-definitions"></a>Definice vydání
Definice vydání jsou nastavit prakticky stejně.

Pro webovou službu nastavíme nasazení jako webové aplikace Azure:

![Rozhraní pro nastavení nasazení jako webové aplikace Azure](./media/iot-solution-build-system/image7.png)

A nastaví verze aktivační událost pro průběžné nasazování. To znamená, každý změnami následuje výsledků úspěšném sestavení v aktualizaci do webové aplikace.

![Rozhraní pro nastavení aktivační událost verze na průběžné nasazování.](./media/iot-solution-build-system/image8.png)

Pro mobilní aplikace můžeme nasadit HockeyApp:

![Rozhraní pro nasazení do HockeyApp mobilní aplikace](./media/iot-solution-build-system/image9.png)

## <a name="explore-telemetry-by-using-application-insights"></a>Prozkoumat telemetrie pomocí Application Insights
[Application Insights](../application-insights/app-insights-overview.md) shromažďuje telemetrická data o výkonu a využití webových služeb. Application Insights SDK odesílá telemetrická data ze služby do prostředku Application Insights v Azure.

Procházejte do zdroje Application Insights, který šabloně nastavit. Zde můžete prozkoumat grafy výkonu vaše [projektu služby mobilní aplikace](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService). Ukazují serveru žádostí a časů odezvy, selhání, a počet výjimka. Existují také grafy závislostí doby odezvy – to znamená, volání do databáze a rozhraní REST API, jako je například Machine Learning. Pokud nejsou žádné problémy s výkonem, budete moci zobrazit, jaké část systému způsobuje.

![Příklad grafu výkonu](./media/iot-solution-build-system/image11.png)

Pokud máte webové služby, které jste nastavili ručně, je snadné získat stejné grafy. V okně webové služby, klikněte na **nástroje** > **rozšíření** > **přidat**. Vyberte **Application Insights**.

![Rozhraní pro výběr Application Insights získat grafy](./media/iot-solution-build-system/image12.png)

Tato funkce funguje tak, že instrumentaci vaší aplikace pomocí Application Insights SDK.

Můžete přidat vlastní telemetrii (nebo nástrojem aplikace, která běží někde mimo Azure) [přidání Application Insights SDK](../application-insights/app-insights-asp-net.md) v době vývoje. To je užitečné metriky protokolu, které závisí na aplikaci, například délku průměrná cestě uživatelů nebo celkový vzdálenost. V sadě Visual Studio, klikněte pravým tlačítkem na projekt a potom vyberte **přidat službu Application Insights**.

![Rozhraní pro výběr přidat službu Application Insights přidat vlastní telemetrii](./media/iot-solution-build-system/image10.png)

Application Insights odešle výstrahy e-mailů, pokud je detekována neobvyklou počtu selhání odpovědi. Můžete také nastavit vlastní výstrahy na různé metriky, například dobu odezvy.

Stejně jako se, že webová služba je vždy nahoru a spuštěn, můžete nastavit [testy dostupnosti](../application-insights/app-insights-monitor-web-app-availability.md). Tyto testy příkazem ping otestovat váš web z různých míst po celém světě každých 15 minut. Budete znovu, získat e-mailu, pokud existuje zdá se, že se jednat o problém.

## <a name="estimate-operational-costs"></a>Odhad provozní náklady
Je pozoruhodně levné ke spuštění aplikace podobné následujícímu v malém měřítku. Mnoho služeb žádné volné vstupní úrovně vrstvy, takže vývoj a méně rozsáhlé operace náklady velmi málo. A samozřejmě, není nutné používat všechny funkce ukázáno v MyDriving své vlastní aplikace.

Zde je odhad naše náklady v nastavení konfigurace vývoj MyDriving. Jsme Všimněte si také některé alternativy, které jsme *není* použít. Tyto informace mohou být užitečné jako odhad vlastní náklady.

Předpokládáme:

* Tým více než pět (plus sledování zúčastněným stranám).
* Spuštěná o měsíc.
* 100 uživatelů s čtyři služebních cest za den.

> [!NOTE]
> Pokud jste aplikaci do Azure, je [bezplatný účet](https://azure.microsoft.com/free/).
> 
> 

| **Součást/služby** | **Poznámky k** | **Náklady na měsíc** |
| --- | --- | --- |
| [Visual Studio 2015 Community](https://www.visualstudio.com/products/visual-studio-community-vs) s [Xamarin](https://visualstudiogallery.msdn.microsoft.com/dcd5b7bd-48f0-4245-80b6-002d22ea6eee) <br/>Vývoj napříč platformami prostředí |Visual Studio Community. (Třeba [Visual Studio Professional](https://www.visualstudio.com/vs-2015-product-editions) pro [Xamarin.Forms](https://xamarin.com/forms), návrhu napříč platformami od jednotného kódu.) |$0 |
| [Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) <br/>Obousměrné datové připojení k zařízení |8000 zprávy + 0,5 KB/zprávy volné. |$0 |
| [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics/)  <br/>   Zpracování datového proudu velkých objemů dat |Zdarma 0,031 za jednotku za hodinu, streaming zapnuto. Vyberte počet jednotek streamování, které chcete; Další škálování. |$23 |
| [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)<br/> Adaptivní odpovědí |10/stanici/měsíc. <br/>                                                                                                                                                                                 + 3 hodiny experimentu \* 1 USD / experimentovat hodinu. <br/>                                                                                                                                                           + 3.5 hodin API procesoru \* $2 / produkční procesoru hodinu. <br/>                                                                                                                                                          Čas procesoru rozhraní API předpokládá 5 minut a den retraining, i když to vzrostou s více vstupní data.                   <br/>                                                                                                                                                                     + vyhodnocování zpracovat 400 služebních cest a den 2 min a den. |$20 |
| [App Service](https://azure.microsoft.com/pricing/details/app-service/)  <br/> Hostitel pro mobilní back-end |Vrstvy B1 – produkční webové aplikace. |$56 |
| [Visual Studio Team Services](https://azure.microsoft.com/pricing/details/visual-studio-team-services/)  <br/> Vytvoření, testování částí a správa verzí; Úloha správy |Privátní agenti pět uživatelů. |$0 |
| [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) <br/>Monitorování výkonu a využití webových služeb a weby |Úroveň Free. |$0 |
| [HockeyApp](http://hockeyapp.net/pricing/) <br/> Distribuci beta verzi aplikace a kolekce zpětnou vazbu, použití a data o chybách |Dvě bezplatné aplikace pro nové uživatele.<br/> $30/ měsíc po tomto datu. |$0 |
| [Xamarin](https://store.xamarin.com/)<br/> Kód na jednotnou platformu pro více zařízení |Bezplatná zkušební verze. <br/>25/ měsíc po tomto datu. |$0 |
| [Databáze SQL](https://azure.microsoft.com/pricing/details/sql-database/) pro službu Azure App Service |Základní úroveň; model jedné databáze. |$5 |
| [Service Fabric](https://azure.microsoft.com/pricing/details/service-fabric/) (volitelné) |Spusťte místní cluster. |$0 |
| [Power BI](https://powerbi.microsoft.com/pricing/)<br/> Univerzální zobrazí a šetření přenášené datovými proudy a statických dat |Úroveň Free: 1 GB, 10 000 řádků za hodinu, denní aktualizace. <br/> 10/uživatel/měsíc pro [vyšší limity](https://powerbi.microsoft.com/documentation/powerbi-power-bi-pro-content-what-is-it/), další možnosti připojení, spolupráce. |$0 |
| [Storage](https://azure.microsoft.com/pricing/details/storage/) |L (místně redundantní) &lt; 100 G $0.024/GB. |$3 |
| [Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) |0,60 na aktivitu \* (FOC 8-5). |$2 |
| [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) <br/>  Cluster na vyžádání pro denní retraining |Tři uzly A3 v $0.32 za hodinu jednu hodinu denně * 31 dní. |$30 |
| [Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) |Základní jednotka propustnosti $11/ měsíc + $0,028 příchozí. |$11 |
| Hardwarový klíč Diagnostického | |$12 |
| **Celkový počet** | |**$157** |

Další informace naleznete v tématu:

* Souhrn [kvóty a omezení služby Azure](../azure-subscription-service-limits.md#iot-hub-limits)
* Azure [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/)

## <a name="send-us-your-feedback"></a>Sdělte nám svůj názor
Vzhledem k tomu, že jsme vytvořili vlastní systémy IoT MyDriving pomohou základní informace, jistě chceme slyšet od vás o tom, jak dobře funguje. Dejte nám vědět, pokud:

* Narazíte na problémy nebo výzvami.
* Není k rozšíření bodu, který by bylo vhodnější pro váš scénář.
* Můžete najít efektivnější způsob, jak provést určité požadavky.
* Máte další návrhy na zlepšení MyDriving nebo této dokumentace.

Poskytněte zpětnou vazbu, souborů [problém na Githubu] nebo níže komentář (en-us edition).

Těšíme sluchu od vás!

## <a name="next-steps"></a>Další kroky
Doporučujeme, abyste [MyDriving referenční příručka](http://aka.ms/mydrivingdocs), což je komplexní popis návrhu systému a jeho součástí.

