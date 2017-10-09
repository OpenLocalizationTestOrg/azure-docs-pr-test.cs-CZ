---
title: "Příklad MyDriving Azure IoT: sestavení ho | Microsoft Docs"
description: "Vytvořit aplikaci, která je komplexní ukázka jak tooarchitect s systémem IoT pomocí služby Microsoft Azure, včetně služby Stream Analytics, Machine Learning a Event Hubs."
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
ms.openlocfilehash: e78571225697f745fe011c722e57c8600704c392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-hello-mydriving-solution-tooyour-environment"></a>Vytváření a nasazování hello MyDriving řešení tooyour prostředí
MyDriving je řešení Internetu věcí (IoT), které shromáždí data z vaší car, procesy pomocí machine learning a uvede na váš mobilní telefon. back-end Hello se skládá z řady služeb Microsoft Azure poskytuje. Hello klienti mohou být telefony Android, iOS nebo Windows 10.

Jsme vytvořili hello MyDriving řešení toogive je základní informace pro vytváření systému IoT. Z hello [MyDriving úložišti na Githubu](https://github.com/Azure-Samples/MyDriving), můžete získat Azure Resource Manager skripty toodeploy hello back-end architektura do vlastní účet Azure. Od tohoto okamžiku můžete překonfigurovat hello různé služby, upravte hello dotazy toosuit svoje vlastní data a tak dále. Můžete najít tyto skripty – spolu s kód pro mobilní aplikace hello, hello Azure App Service API projektu a další – v úložišti MyDriving hello.

Pokud ještě nebyl proveden o hello aplikaci, podívejte se na hello [Příručka Začínáme Get](iot-solution-get-started.md).

Je podrobný účet architektury hello v hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs). V souhrnu, jsou několik informací, nastavíme toocreate podobné projektu:

* A **klientskou aplikaci** běží na Android, iOS a Windows 10 Phone. Používáme hello Xamarin platformy tooshare velkou část hello kód, který je uložený na Githubu v části `src/MobileApp`. aplikace Hello ve skutečnosti provádí dvě odlišné funkce:
  * Předává telemetrie ze zařízení hello integrovaného diagnostiky (Diagnostického) a z back-endu vlastní umístění služby toohello systému cloudu.
  * Je uživatelské rozhraní, kde můžete dotazovat uživatele o jejich zaznamenaná silniční služebních cest.
* A **Cloudová služba** ingestuje hello cestu data v reálném čase a zpracovává je. Hello hlavní práci při vytváření této služby je toochoose, Parametrizace a propojit se různým službám Azure. Některá z částí hello vyžadovat skripty toofilter a proces hello příchozí data. Všechny části hello používáme tooconfigure šablony Azure Resource Manager.
* A **mobilní služby App Service** je webová služba hello za hello uživatelské rozhraní část aplikace hello zařízení. Hlavní úlohy je tooquery hello databáze uložená, zpracovaná data. Jeho kód je na Githubu v části `src/MobileAppService`.
* **Visual Studio s Xamarinem** je naše vývojové prostředí. Xamarin, která již existuje jako součást sady Visual Studio i jako samostatné integrované vývojové prostředí (IDE), je použít kód toobuild hello napříč platformami zařízení. toobuild hello iOS kód, je nutné toohave instanci Xamarin spuštěná na počítači OS X. V případě potřeby ho můžete spustit jako agenta, spravované ze sady Visual Studio.
* **Testování částí** zařízení hello aplikací se provádí v Xamarin Test Cloud.
* **GitHub** je hello úložiště, kde ukládáme hello kódu, skripty a šablony.
* **Visual Studio Team Services** je Cloudová služba, která se používá toomanage hello průběžné sestavení a testování aplikace hello webové služby a zařízení.
* **HockeyApp** je použité toodistribute verzích hello kód zařízení. Shromažďuje taky selhání a používání sestav a zpětnou vazbu od uživatelů.
* **Visual Studio Application Insights** monitorování hello mobilní webové služby.

Ano Podíváme se, jak jsme nastavit všechny této. 

> [!NOTE] 
> Řadu hello následující kroky jsou volitelné.
>
>

## <a name="sign-up-for-accounts"></a>Zaregistrujte si účty
* [Visual Studio Dev Essentials](https://www.visualstudio.com/products/visual-studio-dev-essentials-vs.aspx). Tento program volné poskytuje nástroje pro vývojáře toomany snadný přístup a služby, včetně Visual Studio, Visual Studio Team Services a Azure. Nabízí platební 25/ měsíc na platformě Azure po dobu 12 měsíců. Zahrnuje taky odběry tooPluralsight školení a univerzity Xamarin. Můžete také zaregistrujete odděleně pro bezplatné úrovně [Azure](https://azure.com) a [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx), ale tyto neposkytují kredity Azure.
* [HockeyApp](https://rink.hockeyapp.net/) (volitelné), pro správu testovací distribuci mobilních aplikací a shromažďování telemetrie.
* [Xamarin](https://xamarin.com/) (povinné), pro vytváření mobilních aplikací hello a spuštěné spustí ladění a testy [Xamarin Test Cloud](https://xamarin.com/test-cloud).
* [GitHub](https://github.com/Azure-Samples/MyDriving/) (volitelné), toocreate volné úložiště veřejné pro vlastní kód (privátní úložiště jsou placené). Alternativně můžete vytvořit základní plán hello ve Visual Studio Team Services pro privátní úložiště.
* [Power BI](https://powerbi.microsoft.com/) (volitelné), toocreate bohatých vizualizací služby data mezi hello celý systém.

> [!NOTE]
> Nepotřebujete Githubu, účet tooaccess hello MyDriving kód v [hello úložiště GitHub MyDriving](https://github.com/Azure-Samples/MyDriving).
> 
> 

## <a name="install-development-tools"></a>Instalace nástrojů pro vývoj
Hello následující instalační program pro vývoj řešení úplné hello: iOS, Android a Windows 10 Mobile multiplatformní aplikace pomocí Azure back-end.

Jako alternativu můžete použít Xamarin Studio na Mac nebo Windows toodevelop hello mobilní aplikace, pokud nepracujete v hello ukončení Azure zpět.

Je [delší popis tato instalace](https://msdn.microsoft.com/library/mt613162.aspx).

### <a name="windows-development-machine"></a>Vývoj pro počítač systému Windows
Hello centrální nástroj v systému Windows je Visual Studio pro práci s hello MyDriving aplikace pro Android a Windows hello App Service API projektu a mikroslužbu rozšíření.

Xamarin, Git, emulátorů a další užitečné součásti jsou všechny integrované pomocí sady Visual Studio.

Instalace:

* [Visual Studio s Xamarinem](https://www.visualstudio.com/products/visual-studio-community-vs) (všechny edice – komunity je bezplatná).
* [SQLite pro univerzální platformu Windows](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936). Vyžaduje toobuild hello Windows 10 Mobile kódu.
* [Azure SDK pro Visual Studio](https://www.visualstudio.com/vs/azure-tools/). Poskytuje hello SDK pro aplikace spuštěné v Azure, společně s příkazového řádku nástroje pro správu Azure.
* [Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric). Požadované toobuild hello [mikroslužbu](../service-fabric/service-fabric-get-started.md) rozšíření.

Ujistěte se, že máte správné rozšíření Visual Studia hello. Zkontrolujte, že v části **nástroje**, uvidíte **Android, iOS, Xamarin...** . Pokud ne, otevřete Visual Studio, vyhledejte Xamarin a postupujte podle pokynů tooinstall hello ho. Zkontrolujte také, který **Git pro Windows** je nainstalovaná. Pokud ne, v sadě Visual Studio, vyhledejte je a postupujte podle pokynů tooinstall hello ho. 

### <a name="mac-development-machine"></a>Vývoj pro počítače Mac
Hello Mac (Yosemite nebo novější) je povinný, pokud chcete, aby toodevelop pro iOS. I když jsme použijte sadu Visual Studio s Xamarinem v systému Windows toodevelop a spravovat všechny hello kódu, Xamarin používá agenta nainstalovaného v systému Mac v pořadí toobuild a přihlašovací hello iOS kódu.

![Vývoj v systému Windows a sestavení v systému Mac](./media/iot-solution-build-system/image1.png)

(Alternativně můžete použít Xamarin Studio přímo na aplikací pro různé platformy hello Mac toodevelop.)

Pokud nechcete, aby tooinclude iOS jako cílová platforma nepotřebujete hello Mac.

Instalace:

* [Xamarin Studio pro iOS](https://developer.xamarin.com/guides/ios/getting_started/installation/mac/). Můžete také nastavit Visual Studio a Xamarin, na kterém běží virtuální počítač Windows Macu. V tématu [nastavení, instalaci a ověření pro uživatele Mac](https://msdn.microsoft.com/library/mt488770.aspx) na webu MSDN.
* [Nástroje pro vývoj Azure](https://azure.microsoft.com/downloads/) (volitelné).

Povolit vzdálené přihlášení v hello Mac. Otevřete **předvolbách systému** > **sdílení**a potom vyberte **vzdálené přihlášení**.

Při otevření projektu iOS v sadě Visual Studio v systému Windows, hello Xamarin modulu plug-in zobrazí výzvu k zadání ID hello hello Mac.

## <a name="fetch-hello-github-repository"></a>Načtení úložiště GitHub hello
Načtení místní kopii [hello úložiště GitHub MyDriving](https://github.com/Azure-Samples/MyDriving) pomocí hello **stáhnout ZIP** tlačítka na GitHub, Visual Studio nebo jiného klienta Git.

Rozbalte složku tooa hello soubor s krátký název cesty, jako je například C:\\kódu.

Případně pokud chcete tookeep až toodate s nebo přispívat tooour kódu, klonovat hello úložiště následujícím způsobem:

**https://github.com/Azure-Samples/MyDriving.git klon Git**

## <a name="get-a-bing-maps-api-key"></a>Získat klíč rozhraní API map Bing
[Registrace pro klíč rozhraní API map Bing](https://msdn.microsoft.com/library/ff428642.aspx).

Je třeba tooreplace to v řádku 22 v `src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs`.

## <a name="build-hello-demo-app"></a>Sestavení hello ukázkovou aplikaci
Otevření těchto řešení v sadě Visual Studio:

* src\MobileApps\MyDriving.sln
* src\MobileAppService\MyDrivingService.sln
* src\Extensions\ServiceFabric\VINLookUpApplication\VINLookUpApplication.sln

Získáte pokynů:

* Důvěřujete některé potenciálně nedůvěryhodných projekty. Zvolte tooopen vzbuzené. Pokud chcete, aby toogo dopředu.
* Nastavte režim vývojáře, pokud pracujete na nový počítač s Windows 10.
* Zadejte svoje přihlašovací údaje Xamarin.
* Připojit toohello Xamarin Mac. Pokud nemáte algoritmu Mac, iOS hello klikněte pravým tlačítkem na projekt v sadě Visual Studio a potom vyberte **uvolnit projekt**.

Znovu sestavte řešení hello.

Pokud máte potíže při vytváření, zkuste tooquirks hello řešení, které jsme zjistili:

* *Projekt VINLookupApplication nenačte*: Ujistěte se, že jste nainstalovali hello [Azure SDK pro Visual Studio](https://www.visualstudio.com/vs/azure-tools/).
* *Není sestavení projektu Service Fabric*: sestavení projektů rozhraní hello nejprve a ujistěte se, že jste nainstalovali hello Service Fabric SDK.
* *Aplikace pro Android nemá sestavení*:
  
  * Otevřete **nástroje** > **Android** > **Android SDK Manager**a ujistěte se, že Android 6 (API 23) / SDK platformy je nainstalováno.
  * Odstraňte tento adresář a potom ho sestavte znovu:<br/>
    `%LocalAppData%\Xamarin\zips`

## <a name="get-tooknow-hello-code"></a>Získání tooknow hello kódu
V řešení hello najdete:

* Rozšíření Azure: Service Fabric.
* Azure HDInsight: Skripty pro zpracování dat cesty v Azure.
* Mobilní aplikace: aplikace, hello zařízení.
* MobileAppsService/MyDrivingService: ukončení hello webové zpět.
* Power BI: definice sestavy hello.
* Skripty:
  
  * Správce prostředků: šablony toobuild hello prostředků Azure.
  * Prostředí PowerShell: Skripty toorun hello správce prostředků šablony.
  * Azure SQL Database: Databáze ladění.
* SQL Database: CreateTables: definice schémat.
* Azure Stream Analytics: Dotazuje této transformace hello příchozí datový proud.

## <a name="run-hello-apps-in-development-mode"></a>Spouští aplikace hello v režimu pro vývoj
Proveďte akci toorun aplikace hello, založené na hello zařízení, které používáte:

* Back-end: Sada MyDrivingService jako spouštěný projekt hello a stiskněte klávesu F5 toorun hello back endové webové služby. Otevře se prohlížeč zobrazení seznamu hello rozhraní API.
* Mobilních klientů: hello [jsou mobilní aplikace vyvinuté v Xamarin](https://developer.xamarin.com/guides/cross-platform/deployment,_testing,_and_metrics/debugging_with_xamarin/).
  
  * Android: Podrobnosti najdete v tématu [ladění Android v Xamarin](http://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debugging_with_xamarin_android/).
  * iOS: Podrobnosti najdete v tématu [ladění na iOS](http://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/debugging_in_xamarin_ios/).
  * Windows Phone: Podrobnosti najdete v tématu [Xamarin + Windows Phone](https://developer.xamarin.com/guides/cross-platform/windows/phone/).

## <a name="upload-hello-mobile-app-toohockeyapp"></a>Nahrát tooHockeyApp hello mobilní aplikace
HockeyApp spravuje distribuční hello Android, iOS nebo Windows aplikace tootest uživatelů, upozornění uživatelů nové verze. Shromažďuje taky užitečné havárií sestavy, zpětnou vazbu od uživatelů se snímky obrazovky a metriky využití.

[Začněte tím, že odesílání](http://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app) sestavení aplikace. Potom přihlásit příliš[HockeyApp](https://rink.hockeyapp.net) z vývojovém počítači. Na řídicím panelu hello vývojáře, klikněte na tlačítko **novou aplikaci**a pak přetáhněte hello vytvořené soubory na okno hello. (Později můžete automatizovat vaší služby toodo sestavení to.)

Nyní jste v řídicím panelu aplikace.

![Karta Přehled na řídicím panelu aplikace hello](./media/iot-solution-build-system/image2.png)

Hello postup opakujte pro každou platformu, která vaše aplikace běží na. Potom můžete provést následující hello:

* Použití hello [ID aplikace](http://support.hockeyapp.net/kb/app-management-2/how-to-find-the-app-id) z hello data o chybách toosend řídicí panel a zpětné vazby z vaší aplikace. V MyDriving aktualizujte ID hello v src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs.
* [Testovací uživatele pozvat](http://support.hockeyapp.net/kb/app-management-2/how-to-invite-beta-testers). Získat adresu URL toorecruit testerům, sada uživatelů. Budete se moct toosign pro váš tým, stáhnout aplikaci hello a můžete odeslat zpětnou vazbu.
* Pokud si přejete více otevřete betaverze, nastavte toopublic distribuční hello. Klikněte na tlačítko **spravovat aplikaci** > **distribuční** > **stáhnout = veřejné**. Každý, kdo teď aplikaci stáhněte a můžete odeslat zpětnou vazbu a se zobrazí oznámení, když publikujete novou verzi. Některé sestavy havárií mohou být příliš z nich.
  
   ![Týmy na řídicím panelu hello](./media/iot-solution-build-system/image3.png)
* [Odkaz havárií sestavy tooVisual Studio Team Services](http://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs). Klikněte na tlačítko **spravovat aplikaci** > **Visual Studio Team Services**. HockeyApp můžete automaticky vytvořit pracovní položky v Team Services při hlášení selhání ani při doručení zpětnou vazbu.

Čtení více v hello [HockeyApp lokality](https://hockeyapp.net).

## <a name="test-hello-mobile-app-on-xamarin-test-cloud"></a>Testování mobilní aplikace hello na Xamarin Test Cloud
[Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud/introduction-to-test-cloud/) automatizuje testování uživatelského rozhraní na skutečné zařízení v cloudu hello. Pomocí hello NUnit framework zápisu testy, které spuštění aplikace hello uživatelském rozhraní.

toouse Xamarin, můžete začlenit hello [Xamarin.UITests](https://developer.xamarin.com/guides/testcloud/uitest/intro-to-uitest/) SDK do aplikace, která se dodává jako balíčku NuGet. Najdete v hello ukázkovou aplikaci a má zahrnout, když vytvoříte nové projekty test s hello Xamarin šablony.

![Kde toofind hello napříč platformami SDK na rozhraní hello](./media/iot-solution-build-system/image4.png)

Příklad testovacího projektu je součástí aplikace hello v úložišti hello. V [MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService), podívejte se do části [src](https://github.com/Azure-Samples/MyDriving/tree/master/src)/MobileApps/[MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileApps/MyDriving)/MyDriving.UITests/.

Pokud používáte Visual Studio Team Services sestavení, je snadné toowrite uživatelského rozhraní Xamarin jednotky testy a jejich spuštění v rámci vaší sestavení.

## <a name="deploy-azure-services"></a>Nasazení služby Azure
tooperform automatické nasazení služby Azure a služby sestavení Team Services, naleznete v toohello podrobné pokyny v **scripts/README.md**.

Microsoft Azure poskytuje širokou řadu různých služeb, které můžete použít toobuild cloudové aplikace. Mnoho lze jednotlivě (třeba služby nebo webové aplikace), ale jsou v jejich nejlepší když jste propojeny tooform integrovaný systém jako je například používáme v MyDriving.

Je možné toocreate a ručně propojení služby Azure, ale je mnohem rychlejší a spolehlivější šablon Azure Resource Manageru toouse. [Správce prostředků](../azure-resource-manager/resource-group-overview.md) automatizuje hello nasazení řešení prostředků a provedení hello propojení mezi nimi.

Zjistíte hello šablony pro systém MyDriving hello v úložišti GitHub hello pod [skripty nebo ARM](https://github.com/Azure-Samples/MyDriving/tree/master/scripts/ARM). Poskytuje komplexní a stručným zobrazení způsobu vzájemného propojení hello různé služby v našem architektuře. Všechny tyto podrobně v hello objasníme [MyDriving referenční příručka](http://aka.ms/mydrivingdocs), ale dozvíte mnoho právě načtením prostřednictvím samotné hello šablony.

> [!NOTE]
> Většina Azure služby mají přidružené náklady, v závislosti na hello cenová úroveň. Pokud jste nový tooAzure, můžete [zdarma vyzkoušet](https://azure.microsoft.com/free/). Ale pokud neplánujete toouse určité součásti v hello MyDriving systému, být jisti tooremove je tooavoid přijímají náklady. část "Odhad provozní náklady" Hello později v tomto článku obsahuje souhrn výdaje typické služby.
> 
> 

### <a name="edit-hello-template"></a>Úprava šablony hello
toocustomize vaše nasazení, možná tooremove nepotřebné součásti nebo tooadd ostatní, nejprve vytvořte kopie scénář\_complete.params.json a scénář\_complete.json, ve které toomake změny.

Můžete použít hello\_toooverride souboru complete.params.json různé výchozí hodnoty, jako je například hello služby SKU nebo hello typu replikace úložiště, jak je popsáno v následující tabulce hello. výchozí hodnoty Hello vybrat možnosti hello nejnižší náklady.

| **Parametr** | **Popis** | **Výchozí hodnota** |
| --- | --- | --- |
| Centrum IoT SKU |Vrstva pro službu Azure IoT Hub |F1 |
| Typ účtu úložiště |Typ replikace úložiště |Standardní LRS |
| Cíl služby SQL |Spotřeba slotu souběžnosti |OD DW100 |
| Hostování plán SKU |Plán služby pro službu App Service |F1 |

Ve scénáři\_complete.json:

* Vyhledejte "baseName" a změnit jeho tooa název, který preferujete.
* Vyhledejte "Vytvořit". Každý z těchto částí vytvoří prostředek.
* Nastavte sqlServerAdminLogin a sqlServerAdminPassword toosuitable hodnoty.
* Před odstraněním oddíl, který vytvoří prostředek, zkontrolujte, zda má závislé objekty vyhledáním názvu jinde v souboru hello. Všimněte si, že každý oddíl, který vytvoří služba zahrnuje *dependsOn* oddíl, který uvádí jeho závislosti.

Zde uvádíme nakonfiguruje jakou šablonu hello. Podrobnosti najdete v hello [referenční příručka](http://aka.ms/mydrivingdocs).

| **Služba** | **Popis a podrobnosti** |
| --- | --- |
| Účty úložiště |Hello šablona vytvoří tři účty: |
| -A SQL database, která získává telemetrická agregovaná data ze služby Stream Analytics a slouží jako záložní hello úložiště pro tabulky Azure App Service, které zobrazit tato data prostřednictvím koncových bodů rozhraní API. | |
| – Úložiště objekt blob, které shromažďuje historická data z jiné úlohy Stream Analytics, toobe zpracovává HDInsight. | |
| -A SQL database, která přijímá výsledky zpracovává HDInsight pro použití s Power BI. | |
| Azure IoT Hub |Vytváří obousměrné připojení tooeach připojené zařízení. Hello MyDriving řešení mobilní aplikace hello funguje jako pole brány toosend data tooAzure IoT Hub. Azure IoT Hub poté slouží jako vstupní tooStream Analytics. |
| Azure Event Hubs |Výstup úlohy Stream Analytics, fronty hello tooextensions výstup, které jsou vytvořené pomocí Azure Service Fabric. |
| Azure SQL Data Warehouse | |
| Úlohy Stream Analytics |Připojit vstupy a výstupy s dotazem, což je použité tooaggregate v reálném čase a historická data pro hello rozhraní API App Service, Azure Machine Learning, rozšíření a Power BI. |
| Pracovní prostor Machine Learning |Zahrnuje experimenty, kódu jazyka R a rozhraní API služby. |
| Azure Data Factory |Naplánované retraining Machine Learning. |
| Hostování plánu Service Fabric |Pro rozšíření. |
| Služby App Service (dále jen "mobilní aplikace") |Hostitelé hello mobilní aplikace API projekt, který poskytuje koncové body pro mobilní aplikace hello. Hello kódu rozhraní API musí být nasazené tooApp Service ze sady Visual Studio. |
| Pravidla výstrah |Odešle že e-mailu Pokud odpovědi aplikace hello indikuje selhání. |
| Application Insights |Pro sledování výkonu hello rozhraní API v App Service. Máte tooconfigure hello připojení v sadě Visual Studio. |
| Azure Key Vault |Pro uložení certifikátu hello webové služby clusteru. |

### <a name="run-hello-template"></a>Spusťte šablonu hello
V **scripts/README.md**, existují podrobné pokyny pro spuštěné hello šablony.

tooprovision všechny tyto služby ve vlastní účet Azure pomocí skriptu hello, proveďte jednu z následujících hello:

* Pomocí prostředí PowerShell:
  
  ```
  
  cd scripts/PowerShell;
  deploy.ps1 *location* *resourceGroupName*
  ```
  
  * *umístění* je hello [umístění Azure](https://azure.microsoft.com/regions/), jako například `North Europe` nebo `West US`. Použití `Get-AzureLocation` toofind seznamu dostupných umístění.
  * *Název skupiny prostředků* je hello název, který chcete toogive toohello skupinu, která bude patřit všechny prostředky hello. Až skončíte s hello prostředky, můžete je odstranit všechny společně odstraněním této skupiny.
* Spusťte DeploymentScripts/Bash/deploy.sh s Bash.
* Otevření a sestavení řešení sady Visual Studio hello DeploymentScripts/VS/DeployARM.sln.

Všimněte si, že každé šabloně hello čas spuštění vytvoří novou sadu prostředků pomocí nové názvy. toodelete hello prostředky, přejděte toohello portál a odstraňte skupinu prostředků hello.

Pokud z nějakého důvodu selže hello skriptu, můžete ji znovu spustit.

poskytuje skriptu Hello hello možnost konfigurace průběžnou integraci ve Visual Studio Team Services. Pokud jste nastavili projektu Team Services, budete mít adresu URL: https://yourAccountName.visualstudio.com. Jakmile se zobrazí výzva, zadejte úplnou adresu URL hello. Můžete jí název nového nebo existujícího projektu Team Services.

## <a name="set-up-build-and-test-definitions-in-visual-studio-team-services"></a>Nastavení sestavení a testování definice v sadě Visual Studio Team Services
Můžeme použít Team Services na tomto projektu většinou pro jeho sestavení a otestovat funkce. Ale také poskytuje podporu vynikající spolupráce, jako je například Správa úloh s kanbanové karty, revize kódu integrovaná s úkoly a Správa zdrojového kódu a ověřované vrácení sestavení. Se integruje se službou a pomocí jiných nástrojů, jako je například Githubu, Xamarin, HockeyApp a samozřejmě Visual Studio. Můžete k němu přístup prostřednictvím hello webové rozhraní nebo pomocí sady Visual Studio, podle toho, co je pohodlnější v každém okamžiku.

Hello kroky hello sestavení a definice vydání použít modul plug-in služby, které jsou k dispozici v hello Team Services [Marketplace](https://marketplace.visualstudio.com/VSTS). Přidání toobasic nástroje toorun příkazové řádky nebo kopírovat soubory jsou služby, který vyvolání sestavení Xamarin, Android a jiných dodavatelů a které připojovat tooHockeyApp.

![Možnosti v Team Services sestavení](./media/iot-solution-build-system/image5.png)

### <a name="build-definitions"></a>Definice sestavení
Máme definice sestavení pro každou z hlavních cílů hello. Máme také rozdíly pro funkce a testování regrese. To nám dává:

* MyDriving.Services (hello back endové webové aplikace pro mobilní aplikace hello)
* MyDriving.Xamarin.Android
  
  * MyDriving.Xamarin.Android – funkce
  * Regrese MyDriving.Xamarin.Android
* MyDriving.Xamarin.iOS
  
  * MyDriving.Xamarin.iOS – funkce
  * Regrese MyDriving.Xamarin.iOS
* MyDriving.Xamarin.UWP
  
  * MyDriving.Xamarin.UWP – funkce
  * Regrese MyDriving.Xamarin.UWP

Pokud chcete toosee hello úplné podrobnosti o naše konfigurace, najdete v části 4.7 hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs), "Sestavení a verze konfigurace." Následují hello stejného vzoru Obecné. Hello skriptu:

1. Obnoví hello balíček NuGet. A hello první kroky každé sestavení jsou toorestore hello požadované balíčky NuGet, jsme není v úložišti hello nadále zkompilovaný kód.
2. Aktivuje licenci hello. sestavení Hello se provádí v cloudu hello, tam, kde budeme potřebovat licenci – konkrétně pro Xamarin sestavení služby--hello máme tooactivate naše licencí na počítači aktuální sestavení hello. Potom jsme deaktivovat hned potom tooallow ho toobe použít na jiném počítači.
3. Sestavení s použitím hello příslušnou službu. Používáme sestavení Xamarin pro mobilní aplikace hello a Visual Studio vytvoří hello back endové webové služby.
4. Sestaví testy.
5. Spustí testy. Jsme spuštění testů hello mobilní aplikace Xamarin Test Cloud.
6. Publikuje hello sestavení výsledek toohello rozevírací umístění.

Hello aktivační událost pro hlavní sestavení hello nastavena toocontinuous integrace. To znamená hello sestavení běží pokaždé, když kódu se změnami toohello hlavní větve.

![Rozhraní, kde je aktivační událost hello sadu toocontinuous integrace](./media/iot-solution-build-system/image6.png)

### <a name="release-definitions"></a>Definice vydání
Verze definice je nastavena v mnohem hello stejný způsobem.

Pro webovou službu hello nastavíme nasazení jako webové aplikace Azure:

![Rozhraní pro nastavení nasazení jako webové aplikace Azure](./media/iot-solution-build-system/image7.png)

A nastaví hello verze aktivační událost toocontinuous nasazení. To znamená, každý změnami následuje výsledků úspěšném sestavení ve webové aplikaci toohello aktualizace.

![Rozhraní pro nastavení hello verze aktivační událost toocontinuous nasazení](./media/iot-solution-build-system/image8.png)

Pro mobilní aplikace můžeme nasadit tooHockeyApp:

![Rozhraní pro nasazení tooHockeyApp mobilní aplikace](./media/iot-solution-build-system/image9.png)

## <a name="explore-telemetry-by-using-application-insights"></a>Prozkoumat telemetrie pomocí Application Insights
[Application Insights](../application-insights/app-insights-overview.md) shromažďuje telemetrická data o hello výkonu a využití webových služeb. Hello Application Insights SDK odesílá telemetrii z hello služby toohello prostředek Application Insights v Azure.

Procházejte toohello prostředek Application Insights hello šablona nastavit. Zde můžete prozkoumat grafy hello výkon vaší [projektu služby mobilní aplikace](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService). Ukazují serveru žádostí a časů odezvy, selhání, a počet výjimka. Existují také grafy závislostí odezvy – to znamená, volání toohello databáze a tooREST rozhraní API, jako je například Machine Learning. Pokud nejsou žádné problémy s výkonem, budete moct toosee jaká část systému způsobuje.

![Příklad grafu výkonu](./media/iot-solution-build-system/image11.png)

Pokud máte webové služby, které jste nastavili ručně, snadno tooget hello stejné grafy. V okně pro hello webové služby, klikněte na tlačítko **nástroje** > **rozšíření** > **přidat**. Vyberte **Application Insights**.

![Rozhraní pro výběr Application Insights tooget hello grafy](./media/iot-solution-build-system/image12.png)

Funkce Hello funguje tak, že instrumentaci vaší aplikace pomocí hello Application Insights SDK.

Můžete přidat vlastní telemetrii (nebo nástrojem aplikace, která běží někde mimo Azure) [přidání hello Application Insights SDK](../application-insights/app-insights-asp-net.md) v době vývoje. To je užitečné toolog metriky, které závisí na aplikaci hello, jako je délka průměrná cestě uživatelů nebo celkový vzdálenost. V sadě Visual Studio, klikněte pravým tlačítkem na projekt hello a pak vyberte **přidat službu Application Insights**.

![Rozhraní pro výběr přidat službu Application Insights tooadd vlastní telemetrii](./media/iot-solution-build-system/image10.png)

Application Insights odešle výstrahy e-mailů, pokud je detekována neobvyklou počtu selhání odpovědi. Můžete také nastavit vlastní výstrahy na různé metriky, například dobu odezvy.

Jenom toobe se, že webová služba je vždy nahoru a spuštěn, můžete nastavit [testy dostupnosti](../application-insights/app-insights-monitor-web-app-availability.md). Tyto testy příkazem ping otestovat váš web z různých míst kolem hello, world každých 15 minut. E-mailu, získáte Pokud nejspíš toobe problém.

## <a name="estimate-operational-costs"></a>Odhad provozní náklady
Je pozoruhodně nenákladné toorun aplikace, jako je ten náš v malém měřítku. Mnoho služeb hello žádné volné vstupní úrovně vrstvy, takže vývoj a méně rozsáhlé operace náklady velmi málo. A samozřejmě své vlastní aplikace nemáte toouse všechny funkce hello ukázáno v MyDriving.

Zde je odhad naše náklady v nastavení konfigurace vývoj hello MyDriving. Jsme Všimněte si také některé alternativy, které jsme *není* použít. Tyto informace mohou být užitečné jako odhad vlastní náklady.

Předpokládáme:

* Tým více než pět (plus sledování zúčastněným stranám).
* Spuštěná o měsíc.
* 100 uživatelů s čtyři služebních cest za den.

> [!NOTE]
> Pokud jste nový tooAzure, je [bezplatný účet](https://azure.microsoft.com/free/).
> 
> 

| **Součást/služby** | **Poznámky k** | **Náklady na měsíc** |
| --- | --- | --- |
| [Visual Studio 2015 Community](https://www.visualstudio.com/products/visual-studio-community-vs) s [Xamarin](https://visualstudiogallery.msdn.microsoft.com/dcd5b7bd-48f0-4245-80b6-002d22ea6eee) <br/>Vývoj napříč platformami prostředí |Visual Studio Community. (Třeba [Visual Studio Professional](https://www.visualstudio.com/vs-2015-product-editions) pro [Xamarin.Forms](https://xamarin.com/forms), toodesign napříč platformami od jednotného kódu.) |$0 |
| [Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) <br/>Obousměrný datový toodevices připojení |8000 zprávy + 0,5 KB/zprávy volné. |$0 |
| [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics/)  <br/>   Zpracování datového proudu velkých objemů dat |Zdarma 0,031 za jednotku za hodinu, streaming zapnuto. Zvolte hello počet jednotek streamování, které chcete; Další tooscale nahoru. |$23 |
| [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)<br/> Adaptivní odpovědí |10/stanici/měsíc. <br/>                                                                                                                                                                                 + 3 hodiny experimentu \* 1 USD / experimentovat hodinu. <br/>                                                                                                                                                           + 3.5 hodin API procesoru \* $2 / produkční procesoru hodinu. <br/>                                                                                                                                                          Čas procesoru rozhraní API předpokládá 5 minut a den retraining, i když to vzrostou s více vstupní data.                   <br/>                                                                                                                                                                     + vyhodnocování tooprocess 400 služebních cest a den 2 min a den. |$20 |
| [App Service](https://azure.microsoft.com/pricing/details/app-service/)  <br/> Hostitel pro mobilní back-end |Vrstvy B1 – produkční webové aplikace. |$56 |
| [Visual Studio Team Services](https://azure.microsoft.com/pricing/details/visual-studio-team-services/)  <br/> Vytvoření, testování částí a správa verzí; Úloha správy |Privátní agenti pět uživatelů. |$0 |
| [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) <br/>Monitorování výkonu a využití webových služeb a weby |Úroveň Free. |$0 |
| [HockeyApp](http://hockeyapp.net/pricing/) <br/> Distribuci beta verzi aplikace a kolekce zpětnou vazbu, použití a data o chybách |Dvě bezplatné aplikace pro nové uživatele.<br/> $30/ měsíc po tomto datu. |$0 |
| [Xamarin](https://store.xamarin.com/)<br/> Kód na jednotnou platformu pro více zařízení |Bezplatná zkušební verze. <br/>25/ měsíc po tomto datu. |$0 |
| [Databáze SQL](https://azure.microsoft.com/pricing/details/sql-database/) pro službu Azure App Service |Základní úroveň; model jedné databáze. |$5 |
| [Service Fabric](https://azure.microsoft.com/pricing/details/service-fabric/) (volitelné) |Spusťte místní cluster. |$0 |
| [Power BI](https://powerbi.microsoft.com/pricing/)<br/> Univerzální zobrazí a šetření přenášené datovými proudy a statických dat |Úroveň Free: 1 GB, 10 000 řádků za hodinu, denní aktualizace. <br/> 10/uživatel/měsíc pro [vyšší limity](https://powerbi.microsoft.com/documentation/powerbi-power-bi-pro-content-what-is-it/), další možnosti připojení, spolupráce. |$0 |
| [Úložiště](https://azure.microsoft.com/pricing/details/storage/) |L (místně redundantní) &lt; 100 G $0.024/GB. |$3 |
| [Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) |0,60 na aktivitu \* (FOC 8-5). |$2 |
| [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) <br/>  Cluster na vyžádání pro denní retraining |Tři uzly A3 v $0.32 za hodinu jednu hodinu denně * 31 dní. |$30 |
| [Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) |Základní jednotka propustnosti $11/ měsíc + $0,028 příchozí. |$11 |
| Hardwarový klíč Diagnostického | |$12 |
| **Celkový počet** | |**$157** |

Další informace naleznete v tématu:

* Souhrn [kvóty a omezení služby Azure](../azure-subscription-service-limits.md#iot-hub-limits)
* Azure [cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/)

## <a name="send-us-your-feedback"></a>Sdělte nám svůj názor
Vzhledem k tomu, že jsme vytvořili vlastní systémy IoT MyDriving toohelp základní informace, chceme jistě toohear od vás o tom, jak dobře funguje. Dejte nám vědět, pokud:

* Narazíte na problémy nebo výzvami.
* Není k rozšíření bodu, který by bylo vhodnější tooyour scénář.
* Najít efektivnější způsob tooaccomplish určité požadavky.
* Máte další návrhy na zlepšení MyDriving nebo této dokumentace.

toogive zpětnou vazbu, souborů [problém na Githubu] nebo níže komentář (en-us edition).

Těšíme toohearing od vás!

## <a name="next-steps"></a>Další kroky
Doporučujeme, abyste hello [MyDriving referenční příručka](http://aka.ms/mydrivingdocs), což je komplexní popis návrhu hello hello systému a jeho součástí.

