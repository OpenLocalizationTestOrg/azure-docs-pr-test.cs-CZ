---
title: "protokolování diagnostiky aaaEnable pro webové aplikace v Azure App Service"
description: "Zjistěte, jak protokolování diagnostiky tooenable a přidejte instrumentace aplikací tooyour, a jak tooaccess hello informací zaznamenaných v Azure."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 4b2903ff31cc93180552cf51196c33505ffbaf07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Povolit protokolování diagnostiky pro webové aplikace v Azure App Service
## <a name="overview"></a>Přehled
Azure poskytuje integrované diagnostiky tooassist s laděním [webové aplikace App Service](http://go.microsoft.com/fwlink/?LinkId=529714). V tomto článku se dozvíte jak protokolování diagnostiky tooenable a přidejte instrumentace aplikací tooyour, a jak tooaccess hello informací zaznamenaných v Azure.

Tento článek používá hello [portálu Azure](https://portal.azure.com), Azure PowerShell a rozhraní příkazového řádku Azure (Azure CLI) toowork hello diagnostické protokoly. Informace o práci s diagnostických protokolů pomocí sady Visual Studio najdete v tématu [řešení potíží s Azure v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Webový server diagnostics a application diagnostics
Webové aplikace služby App Service poskytují diagnostické funkce pro protokolování informací z hello webový server a hello webové aplikace. Tyto jsou logicky rozdělené do **webový server diagnostiky** a **rozhraní application diagnostics**.

### <a name="web-server-diagnostics"></a>Diagnostika webového serveru
Můžete povolit nebo zakázat hello následující typy protokolů:

* **Podrobné protokolování chyb** -podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (kód stavu 400 nebo vyšší). To může obsahovat informace, které vám mohou pomoci určit, proč hello server vrátil kód chyby hello.
* **Se nezdařilo, trasování požadavku** -podrobné informace o chybných žádostech, včetně trasování hello IIS komponenty používané tooprocess hello požadavku a hello doba v jednotlivých součástí. To může být užitečné, pokud se pokoušíte výkonu webu tooincrease nebo izolovat, co ho způsobuje konkrétní toobe chyby HTTP vrátil.
* **Webový Server protokolování** -informace o transakcích HTTP pomocí hello [rozšířený formát protokolu W3C souboru](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). To je užitečné, když chcete určit celkový metriky lokality, jako je například hello počet požadavků zpracovaných nebo je počet požadavků z konkrétní IP adresu.

### <a name="application-diagnostics"></a>Rozhraní Application diagnostics
Rozhraní Application diagnostics můžete informace o toocapture vytvořil webovou aplikací. Aplikace ASP.NET můžete použít hello [System.Diagnostics.Trace](http://msdn.microsoft.com/library/36hhw2t6.aspx) rozhraní application diagnostics třída toolog informace toohello protokolu. Například:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

V době běhu může načíst tyto protokoly toohelp při řešení problémů. Další informace najdete v tématu [řešení potíží s Azure webové aplikace v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

Webové aplikace služby App Service také protokolu informace o nasazení, při publikování obsahu tooa webové aplikace. K tomu dojde automaticky a nejsou žádné nastavení konfigurace pro nasazení protokolování. Protokolování nasazení vám umožní toodetermine, proč nasazení se nezdařilo. Například pokud používáte vlastní nasazení skriptu, můžete použít nasazení protokolování toodetermine proč selhává hello skriptu.

## <a name="enablediag"></a>Jak tooenable diagnostiky
Diagnostika tooenable v hello [portálu Azure](https://portal.azure.com), přejděte toohello okno pro vaši webovou aplikaci a klikněte na tlačítko **Nastavení > diagnostické protokoly**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Část protokoly](./media/web-sites-enable-diagnostic-log/logspart.png)

Když povolíte **rozhraní application diagnostics** zvolíte hello **úroveň**. Toto nastavení můžete informace hello toofilter zachytit příliš**informační**, **upozornění** nebo **chyba** informace. Toto nastavení příliš**podrobné** bude protokolovat všechny informace o vytvořil hello aplikací.

> [!NOTE]
> Na rozdíl od změna hello web.config soubor povolení rozhraní Application diagnostics nebo změna úrovně protokolů diagnostiky není recyklujte hello doménu aplikace, která hello aplikace běží v rámci.
>
>

V hello [portálu classic](https://manage.windowsazure.com) webové aplikace **konfigurace** kartě můžete vybrat **úložiště** nebo **systém souborů** pro **webového serveru protokolování**. Výběr **úložiště** vám umožní tooselect účet úložiště a potom kontejner objektů blob, který hello protokoly byly zapsány do. Všechny protokoly pro **lokality diagnostiky** zapisují toohello pouze v systému souborů.

Hello [portálu classic](https://manage.windowsazure.com) webové aplikace **konfigurace** karta má také další nastavení pro rozhraní application diagnostics:

* **Systém souborů** -úložiště hello aplikace diagnostické informace systému souborů toohello webové aplikace. Tyto soubory můžete získat přístup pomocí protokolu FTP, nebo stáhnout jako archivu Zip pomocí hello prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure (Azure CLI).
* **Tabulka úložiště** -úložiště hello aplikace diagnostické informace v hello zadaný účet úložiště Azure a název tabulky.
* **Úložiště objektů blob** -úložiště hello aplikace diagnostické informace v hello zadaný účet úložiště Azure a objektů blob kontejneru.
* **Doba uchování** – ve výchozím nastavení, nejsou automaticky odstraněny protokoly z **úložiště objektů blob**. Vyberte **nastavení ukládání** a zadejte hello počet dní tookeep protokoly, pokud chcete tooautomatically odstranění protokolů.

> [!NOTE]
> Pokud jste [opět vytvořit přístupové klíče účtu úložiště](../storage/common/storage-create-storage-account.md), je nutné obnovit hello příslušných protokolování konfigurace toouse hello aktualizovat klíče. toodo toto:
>
> 1. V hello **konfigurace** nastavte hello příslušných protokolování funkce příliš**vypnout**. Uložte vaše nastavení.
> 2. Povolte protokolování toohello úložiště účet blob nebo table znovu. Uložte vaše nastavení.
>
>

Hello stejný čas a mají úroveň konfigurace jednotlivých protokolových lze povolit libovolnou kombinaci systému souborů, úložiště table nebo úložiště objektů blob. Například můžete toolog chyby a upozornění tooblob úložiště jako dlouhodobé řešení protokolování, při povolování protokolování systému souborů s úrovní verbose.

Zatímco všech tří umístění úložiště poskytují hello stejné základní informace o protokolu událostí, **tabulky úložiště** a **úložiště objektů blob** protokolu Další informace, jako je hello instance ID, ID vlákna a dalších Podrobné časové razítko (formát značek) než protokolování příliš**systém souborů**.

> [!NOTE]
> Informace uložené v **tabulky úložiště** nebo **úložiště objektů blob** lze přistupovat pouze pomocí klienta úložiště nebo aplikaci, která může pracovat přímo s těchto systémů úložiště. Například Visual Studio 2013 obsahuje Průzkumníka úložiště, které můžou být použité tooexplore tabulky nebo objekt blob úložiště a HDInsight můžete přístup k datům uloženým v úložišti objektů blob. Je také možné zapsat aplikaci, která přistupuje k službě Azure Storage pomocí jednoho z hello [sady Azure SDK](/downloads/#).
>
> [!NOTE]
> Diagnostika se dá taky povolit z prostředí Azure PowerShell pomocí hello **Set-AzureWebsite** rutiny. Pokud nebyly Azure PowerShell nainstalovali, nebo ji nenakonfigurovali toouse vašeho předplatného Azure, najdete v části [jak tooUse prostředí Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

## <a name="download"></a>Postupy: stažení protokolů
Systém souborů diagnostické informace uložené toohello webové aplikace lze přistupovat přímo pomocí protokolu FTP. Lze ji také stáhnout jako archivu Zip pomocí prostředí Azure PowerShell nebo hello rozhraní příkazového řádku Azure.

Hello adresářovou strukturu, která hello protokoly jsou uloženy v vypadá takto:

* **Protokoly aplikací** -/LogFiles/aplikace /. Tato složka obsahuje jeden nebo více souborů textu obsahujícího informace o vyprodukované protokolování aplikací.
* **Trasování požadavku se nezdařilo** -/ LogFiles/W3SVC ### /. Tato složka obsahuje soubor XSL a jeden nebo více souborů XML. Ujistěte se, abyste si stáhli soubor XSL hello do stejného adresáře jako hello XML souborům, protože soubor XSL hello poskytuje funkce pro formátování a filtrování hello obsah hello XML souborům v Internet Exploreru hello.
* **Podrobné protokoly chyb** -/LogFiles/DetailedErrors /. Tato složka obsahuje jeden nebo více souborů HTM, které poskytují podrobné informace pro chyby protokolu HTTP, ke kterým došlo.
* **Webový Server protokoly** -/LogFiles/http/RawLogs. Tato složka obsahuje jeden nebo více souborů text formátován pomocí hello [rozšířený formát protokolu W3C souboru](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Protokoly nasazení** -/ LogFiles/Git. Tato složka obsahuje protokoly hello interní nasazení procesy používané modulem webové aplikace Azure, jakož i protokoly pro nasazení Git.

### <a name="ftp"></a>FTP
diagnostické informace tooaccess pomocí protokolu FTP, navštivte hello **řídicí panel** vaší webové aplikace v hello [portálu classic](https://manage.windowsazure.com). V hello **rychlého přehledu** pomocí hello **diagnostické protokoly FTP** odkaz tooaccess hello protokolu souborů pomocí protokolu FTP. Hello **nasazení nebo FTP uživatele** položka uvádí hello uživatelské jméno, které by měl být použité tooaccess hello FTP lokality.

> [!NOTE]
> Pokud hello **nasazení nebo FTP uživatele** položka není nastavena, nebo jste zapomněli heslo hello pro tohoto uživatele, můžete vytvořit nového uživatele a heslo pomocí hello **resetovat přihlašovací údaje nasazení** odkaz v hello **rychlého přehledu** části hello **řídicí panel**.
>
>

### <a name="download-with-azure-powershell"></a>Stáhnout pomocí prostředí Azure PowerShell
soubory protokolu hello toodownload, spustit novou instanci třídy Azure PowerShell a použít hello následující příkaz:

    Save-AzureWebSiteLog -Name webappname

Tato akce uloží hello protokoly pro webovou aplikaci hello určeného hello **-název** parametr tooa soubor s názvem **logs.zip** v aktuálním adresáři hello.

> [!NOTE]
> Pokud nebyly Azure PowerShell nainstalovali, nebo ji nenakonfigurovali toouse vašeho předplatného Azure, najdete v části [jak tooUse prostředí Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="download-with-azure-command-line-interface"></a>Stáhnout pomocí rozhraní příkazového řádku Azure
soubory protokolů hello toodownload pomocí hello rozhraní příkazového řádku Azure, otevřete nový příkazový řádek, prostředí PowerShell, Bash nebo relaci terminálu a zadejte hello následující příkaz:

    azure site log download webappname

Tato akce uloží hello protokoly pro hello webovou aplikaci s názvem 'webappname' tooa soubor s názvem **diagnostics.zip** v aktuálním adresáři hello.

> [!NOTE]
> Pokud jste nenainstalovali hello rozhraní příkazového řádku Azure (Azure CLI), nebo ji nenakonfigurovali toouse vašeho předplatného Azure, najdete v části [jak tooUse rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Postupy: zobrazení protokolů ve službě Application Insights
Visual Studio Application Insights poskytuje nástroje pro filtrování a vyhledávání protokolů a korelace protokolů hello s požadavky a další události.

1. Přidáte hello Application Insights SDK tooyour projekt v sadě Visual Studio.
   * V Průzkumníku řešení klikněte pravým tlačítkem na projekt a zvolte Přidat službu Application Insights. Budete se zobrazil pokyn provede kroky, které zahrnují vytváření prostředek Application Insights. [Další informace](../application-insights/app-insights-asp-net.md)
2. Přidáte hello naslouchací proces trasování balíček tooyour projekt.
   * Klikněte pravým tlačítkem na projekt a zvolte spravovat balíčky NuGet. Vyberte `Microsoft.ApplicationInsights.TraceListener` [Další informace](../application-insights/app-insights-asp-net-trace-logs.md)
3. Odeslání projektu a spusťte ji toogenerate data protokolu.
4. V hello [portálu Azure](https://portal.azure.com/), procházet tooyour nový prostředek Application Insights a otevřete **vyhledávání**. Zobrazí se data protokolu, spolu s požadavku, použití a další telemetrií. Nějaké telemetrie může trvat několik minut tooarrive: klikněte na tlačítko Aktualizovat. [Další informace](../application-insights/app-insights-diagnostic-search.md)

[Další informace o s Application Insights pro sledování výkonu](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a>Postupy: Stream protokoly
Při vývoji aplikace, je často užitečné toosee protokolování informací v skoro v reálném čase. To můžete udělat streamování protokolování informace tooyour vývojového prostředí pomocí prostředí Azure PowerShell nebo hello rozhraní příkazového řádku Azure.

> [!NOTE]
> Některé typy protokolování vyrovnávací paměti zápisu toohello souboru protokolu, což může vést k události mimo pořadí v datovém proudu hello. Například položku protokolu aplikace, která nastane, když uživatel navštíví stránky nemusí být zobrazeny v datovém proudu hello před hello odpovídající HTTP protokolu záznam pro požadavek na stránku hello.
>
> [!NOTE]
> Vysílání datového proudu protokolu bude také stream informace zapsané tooany textový soubor uložený v hello **D:\\domácí\\LogFiles\\**  složky.
>
>

### <a name="streaming-with-azure-powershell"></a>Streamování pomocí prostředí Azure PowerShell
informace o protokolování toostream, spustit novou instanci třídy Azure PowerShell a použijte hello následující příkaz:

    Get-AzureWebSiteLog -Name webappname -Tail

To připojí toohello webové aplikace určeného hello **-název** parametr a začít streamování okno PowerShell toohello informace, jako protokol událostí, ke kterým došlo u hello webové aplikace. Všechny informace zapsané toofiles končící na .txt, .log nebo htm, které jsou uložené v adresáři /LogFiles hello (d: nebo Domovská nebo soubory protokolů) bude Streamovat toohello místní konzoly.

toofilter konkrétními událostmi, například chyby, použijte hello **– zpráva** parametr. Například:

    Get-AzureWebSiteLog -Name webappname -Tail -Message Error

toofilter konkrétní typy, jako je například HTTP, používat hello **-cesta** parametr. Například:

    Get-AzureWebSiteLog -Name webappname -Tail -Path http

toosee seznam cest k dispozici, použijte hello - ListPath parametr.

> [!NOTE]
> Pokud nebyly Azure PowerShell nainstalovali, nebo ji nenakonfigurovali toouse vašeho předplatného Azure, najdete v části [jak tooUse prostředí Azure PowerShell](/develop/nodejs/how-to-guides/powershell-cmdlets/).
>
>

### <a name="streaming-with-azure-command-line-interface"></a>Streamování pomocí rozhraní příkazového řádku Azure
informace o protokolování toostream, otevřete nový příkazový řádek, prostředí PowerShell, Bash nebo relaci Terminálové služby a zadejte hello následující příkaz:

    azure site log tail webappname

To připojí toohello webovou aplikaci s názvem 'webappname' a začít streamování okno toohello informace, jako protokol událostí, ke kterým došlo u hello webové aplikace. Všechny informace zapsané toofiles končící na .txt, .log nebo htm, které jsou uložené v adresáři /LogFiles hello (d: nebo Domovská nebo soubory protokolů) bude Streamovat toohello místní konzoly.

toofilter konkrétními událostmi, například chyby, použijte hello **– filtru** parametr. Například:

    azure site log tail webappname --filter Error

toofilter konkrétní typy, jako je například HTTP, používat hello **– cesta** parametr. Například:

    azure site log tail webappname --path http

> [!NOTE]
> Pokud jste nenainstalovali hello rozhraní příkazového řádku Azure, nebo ji nenakonfigurovali toouse vašeho předplatného Azure, najdete v části [jak tooUse rozhraní příkazového řádku Azure](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Postupy: pochopení protokolů diagnostiky
### <a name="application-diagnostics-logs"></a>Diagnostické protokoly aplikací
Rozhraní Application diagnostics ukládá informace v konkrétním formátu pro aplikace .NET, v závislosti na tom, jestli ukládání systém souborů protokolů toohello, úložiště table nebo úložiště objektů blob. Hello základní sadu uložených dat je stejný hello všech tří typů úložiště – došlo k události hello hello datum a čas, ID procesu hello, který vytváří hello událostí, typ události hello (informace, upozornění, chyby) a zpráva události hello.

**Systém souborů**

Každý řádek protokolu toohello systému souborů nebo pomocí vysílání datového proudu bude mít hello následující formát:

    {Date}  PID[{process id}] {event type/level} {message}

Událost chyby by například zobrazit podobné toohello následující:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on hello page!

Protokolování systému souborů toohello poskytuje hello nejzákladnější informace hello tři dostupné metod, které poskytují jenom hello čas, id procesu, úroveň události a zprávy.

**Table Storage**

Při přihlašování tootable úložiště, jsou další vlastnosti používané toofacilitate hledání hello data uložená v tabulce hello a také podrobnější informace o události hello. Hello (sloupce) se používají následující vlastnosti pro každou entitu (řádek) uložené v tabulce hello.

| Název vlastnosti | Hodnota nebo formátu |
| --- | --- |
| Klíč oddílu |Datum a čas události hello ve formátu yyyyMMddHH |
| RowKey |Hodnota identifikátoru GUID, který jedinečně identifikuje tuto entitu |
| časové razítko |Hello datum a čas, který hello událostí došlo k chybě |
| EventTickCount |Hello datum a čas, který hello událostí došlo k chybě, ve formátu značky (větší přesnost) |
| ApplicationName |Název webové aplikace Hello |
| Úroveň |Úroveň události (například Chyba, upozornění, informace) |
| ID události |ID události Hello této události<p><p>Výchozí nastavení too0, pokud není specifikována. |
| identifikátor instanceId |Došlo k chybě instanci hello webové aplikace, která hello i na |
| PID |ID procesu |
| TID |ID vlákna Hello hello vlákno, které vytváří hello událostí |
| Zpráva |Podrobná zpráva události |

**Blob Storage**

Při přihlašování tooblob úložiště, data se ukládají ve formátu hodnot oddělených čárkami (CSV). Podobně jako tootable úložiště a další pole jsou zaznamenané tooprovide podrobnější informace o události hello. Hello se používají následující vlastnosti pro každý řádek v hello CSV:

| Název vlastnosti | Hodnota nebo formátu |
| --- | --- |
| Datum |Hello datum a čas, který hello událostí došlo k chybě |
| Úroveň |Úroveň události (například Chyba, upozornění, informace) |
| ApplicationName |Název webové aplikace Hello |
| identifikátor instanceId |Instanci hello webové aplikace, která hello událostí došlo k chybě na |
| EventTickCount |Hello datum a čas, který hello událostí došlo k chybě, ve formátu značky (větší přesnost) |
| ID události |ID události Hello této události<p><p>Výchozí nastavení too0, pokud není specifikována. |
| PID |ID procesu |
| TID |ID vlákna Hello hello vlákno, které vytváří hello událostí |
| Zpráva |Podrobná zpráva události |

Hello data uložená v objektu blob vypadat podobně jako toohello následující:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> první řádek Hello hello protokolu bude obsahovat záhlaví sloupců hello reprezentovaný v tomto příkladu.
>
>

### <a name="failed-request-traces"></a>Trasování požadavku se nezdařilo
Trasování chybných požadavků, které jsou uložené v soubory XML s názvem **fr ### .xml**. toomake je snazší tooview hello protokolovat informace, šablony stylů XSL s názvem **freb.xsl** je součástí hello stejný adresář jako hello soubory XML. Otevření jedné ze souborů XML hello v aplikaci Internet Explorer použije hello XSL šablony stylů tooprovide formátovaný zobrazení informací trasování hello. Zobrazí se podobné toohello následující:

![Zobrazit v prohlížeči hello chybných požadavků](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Podrobné protokoly chyb
Podrobné protokoly chyb jsou dokumenty HTML, které poskytují podrobnější informace o chyby protokolu HTTP, které mají došlo k chybě. Vzhledem k tomu, že se jednoduše dokumentů HTML, bylo možné zobrazit pomocí webového prohlížeče.

### <a name="web-server-logs"></a>Protokoly webového serveru
Hello protokolů webového serveru jsou formátovány pomocí hello [rozšířený formát protokolu W3C souboru](http://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Tyto informace lze číst pomocí textového editoru nebo analyzovat pomocí nástrojů, jako třeba [analyzátoru protokolů](http://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Hello protokoly vytvořeného ve službě Azure web apps nepodporují hello **s-computername**, **s-ip**, nebo **cs-version** pole.
>
>

## <a name="nextsteps"></a> Další kroky
* [Jak tooMonitor webové aplikace](http://docs.microsoft.com/en-us/azure/app-service-web/web-sites-monitor)
* [Řešení potíží s aplikací Azure web apps v sadě Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analýza protokolů webové aplikace v prostředí HDInsight](http://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
>
>

## <a name="whats-changed"></a>Co se změnilo
* Průvodce toohello změnu z tooApp weby služby najdete v tématu: [Azure App Service a její vliv na stávající služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
* Průvodce toohello změnu hello starý portál toohello nového portálu najdete v tématu: [odkaz pro navigaci hello portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715)
