---
title: "aaaConfigure webové aplikace v Azure App Service"
description: "Jak tooconfigure webové aplikace v Azure App Services"
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 8697ab6f21cfeb470e11f0d82c68692d43142fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a><span data-ttu-id="04214-103">Konfigurace webových aplikací v prostředí Azure App Service</span><span class="sxs-lookup"><span data-stu-id="04214-103">Configure web apps in Azure App Service</span></span>
<span data-ttu-id="04214-104">Toto téma vysvětluje, jak tooconfigure webovou aplikaci pomocí hello [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="04214-104">This topic explains how tooconfigure a web app using hello [Azure Portal].</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a><span data-ttu-id="04214-105">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="04214-105">Application settings</span></span>
1. <span data-ttu-id="04214-106">V hello [portálu Azure], otevřete okno hello hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04214-106">In hello [Azure Portal], open hello blade for hello web app.</span></span>
2. <span data-ttu-id="04214-107">Klikněte na tlačítko **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="04214-107">Click **All Settings**.</span></span>
3. <span data-ttu-id="04214-108">Klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="04214-108">Click **Application Settings**.</span></span>

![Nastavení aplikace][configure01]

<span data-ttu-id="04214-110">Hello **nastavení aplikace** okno obsahuje nastavení, které jsou seskupené v rámci několika kategorií.</span><span class="sxs-lookup"><span data-stu-id="04214-110">hello **Application settings** blade has settings grouped under several categories.</span></span>

### <a name="general-settings"></a><span data-ttu-id="04214-111">Obecná nastavení</span><span class="sxs-lookup"><span data-stu-id="04214-111">General settings</span></span>
<span data-ttu-id="04214-112">**Verze Framework**.</span><span class="sxs-lookup"><span data-stu-id="04214-112">**Framework versions**.</span></span> <span data-ttu-id="04214-113">Pokud vaše aplikace používá tyto architektury, nastavte tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="04214-113">Set these options if your app uses any these frameworks:</span></span> 

* <span data-ttu-id="04214-114">**Rozhraní .NET framework**: Sada hello rozhraní .NET framework verze.</span><span class="sxs-lookup"><span data-stu-id="04214-114">**.NET Framework**: Set hello .NET framework version.</span></span> 
* <span data-ttu-id="04214-115">**PHP**: nastavit hello PHP verzi nebo **OFF** toodisable PHP.</span><span class="sxs-lookup"><span data-stu-id="04214-115">**PHP**: Set hello PHP version, or **OFF** toodisable PHP.</span></span> 
* <span data-ttu-id="04214-116">**Java**: verzi Javy vyberte hello nebo **OFF** toodisable Java.</span><span class="sxs-lookup"><span data-stu-id="04214-116">**Java**: Select hello Java version or **OFF** toodisable Java.</span></span> <span data-ttu-id="04214-117">Použití hello **webový kontejner** možnost toochoose mezi verzí Tomcat a Jetty.</span><span class="sxs-lookup"><span data-stu-id="04214-117">Use hello **Web Container** option toochoose between Tomcat and Jetty versions.</span></span>
* <span data-ttu-id="04214-118">**Python**: hello vyberte verzi jazyka Python, nebo **OFF** toodisable Python.</span><span class="sxs-lookup"><span data-stu-id="04214-118">**Python**: Select hello Python version, or **OFF** toodisable Python.</span></span>

<span data-ttu-id="04214-119">Technické z důvodů povolení pro aplikace Java zakáže možnosti hello .NET, PHP a Python.</span><span class="sxs-lookup"><span data-stu-id="04214-119">For technical reasons, enabling Java for your app disables hello .NET, PHP, and Python options.</span></span>

<span data-ttu-id="04214-120"><a name="platform"></a>
**Platforma**.</span><span class="sxs-lookup"><span data-stu-id="04214-120"><a name="platform"></a>
**Platform**.</span></span> <span data-ttu-id="04214-121">Vybere, zda běží vaše webová aplikace v prostředí 32bitové nebo 64bitové verze.</span><span class="sxs-lookup"><span data-stu-id="04214-121">Selects whether your web app runs in a 32-bit or 64-bit environment.</span></span> <span data-ttu-id="04214-122">Hello 64-bit prostředí vyžaduje režimu Basic nebo Standard.</span><span class="sxs-lookup"><span data-stu-id="04214-122">hello 64-bit environment requires Basic or Standard mode.</span></span> <span data-ttu-id="04214-123">Uvolněte a režimy sdílené vždy spustit v prostředí 32-bit.</span><span class="sxs-lookup"><span data-stu-id="04214-123">Free and Shared modes always run in a 32-bit environment.</span></span>

<span data-ttu-id="04214-124">**Webové sokety**.</span><span class="sxs-lookup"><span data-stu-id="04214-124">**Web Sockets**.</span></span> <span data-ttu-id="04214-125">Nastavit **ON** tooenable hello protokolu WebSocket; například, pokud vaše webová aplikace používá [ASP.NET SignalR] nebo [socket.io].</span><span class="sxs-lookup"><span data-stu-id="04214-125">Set **ON** tooenable hello WebSocket protocol; for example, if your web app uses [ASP.NET SignalR] or [socket.io].</span></span>

<span data-ttu-id="04214-126"><a name="alwayson"></a>
**Always On**.</span><span class="sxs-lookup"><span data-stu-id="04214-126"><a name="alwayson"></a>
**Always On**.</span></span> <span data-ttu-id="04214-127">Ve výchozím nastavení jsou uvolněna webové aplikace, pokud jsou některé dobu nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="04214-127">By default, web apps are unloaded if they are idle for some period of time.</span></span> <span data-ttu-id="04214-128">To umožňuje ušetřit prostředky systému hello.</span><span class="sxs-lookup"><span data-stu-id="04214-128">This lets hello system conserve resources.</span></span> <span data-ttu-id="04214-129">V režimu Basic nebo Standard, povolit **Always On** aplikace hello tookeep načíst všechny hello čas.</span><span class="sxs-lookup"><span data-stu-id="04214-129">In Basic or Standard mode, you can enable **Always On** tookeep hello app loaded all hello time.</span></span> <span data-ttu-id="04214-130">Pokud vaše aplikace běží nepřetržité webové úlohy nebo běží webové úlohy aktivaci pomocí výrazu CRON, měli byste povolit **Always On**, nebo nemusí spolehlivě spuštění hello webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="04214-130">If your app runs continuous WebJobs or runs WebJobs triggered using a CRON expression, you should enable **Always On**, or hello web jobs may not run reliably.</span></span>

<span data-ttu-id="04214-131">**Spravované verze kanálu**.</span><span class="sxs-lookup"><span data-stu-id="04214-131">**Managed Pipeline Version**.</span></span> <span data-ttu-id="04214-132">Nastaví hello IIS [režim kanálů].</span><span class="sxs-lookup"><span data-stu-id="04214-132">Sets hello IIS [pipeline mode].</span></span> <span data-ttu-id="04214-133">Nechte toto nastavení tooIntegrated (výchozí hello) pouze tehdy, je starší verze aplikace, který vyžaduje starší verze služby IIS.</span><span class="sxs-lookup"><span data-stu-id="04214-133">Leave this set tooIntegrated (hello default) unless you have a legacy app that requires an older version of IIS.</span></span>

<span data-ttu-id="04214-134">**Automatické prohození**.</span><span class="sxs-lookup"><span data-stu-id="04214-134">**Auto Swap**.</span></span> <span data-ttu-id="04214-135">Pokud povolíte automatické prohození pro slot nasazení, služby App Service automaticky Prohodit hello webové aplikace do produkčního prostředí, při nabízené aktualizaci toothat slot.</span><span class="sxs-lookup"><span data-stu-id="04214-135">If you enable Auto Swap for a deployment slot, App Service will automatically swap hello web app into production when you push an update toothat slot.</span></span> <span data-ttu-id="04214-136">Další informace najdete v tématu [nasazení toostaging sloty pro webové aplikace v Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="04214-136">For more information, see [Deploy toostaging slots for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span>

### <a name="debugging"></a><span data-ttu-id="04214-137">Ladění</span><span class="sxs-lookup"><span data-stu-id="04214-137">Debugging</span></span>
<span data-ttu-id="04214-138">**Vzdálené ladění**.</span><span class="sxs-lookup"><span data-stu-id="04214-138">**Remote Debugging**.</span></span> <span data-ttu-id="04214-139">Umožňuje vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="04214-139">Enables remote debugging.</span></span> <span data-ttu-id="04214-140">Když je povolené, můžete použít vzdáleného ladicího programu hello v sadě Visual Studio tooconnect přímo tooyour webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04214-140">When enabled, you can use hello remote debugger in Visual Studio tooconnect directly tooyour web app.</span></span> <span data-ttu-id="04214-141">Vzdálené ladění zůstane zapnutá 48 hodin.</span><span class="sxs-lookup"><span data-stu-id="04214-141">Remote debugging will remain enabled for 48 hours.</span></span> 

### <a name="app-settings"></a><span data-ttu-id="04214-142">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="04214-142">App settings</span></span>
<span data-ttu-id="04214-143">Tato část obsahuje dvojice název/hodnota, které vaší webové aplikace načte při spuštění.</span><span class="sxs-lookup"><span data-stu-id="04214-143">This section contains name/value pairs that you web app will load on start up.</span></span> 

* <span data-ttu-id="04214-144">Pro aplikace .NET, tato nastavení jsou vloženy do vaší konfigurace .NET `AppSettings` v době běhu přepsání stávajícího nastavení.</span><span class="sxs-lookup"><span data-stu-id="04214-144">For .NET apps, these settings are injected into your .NET configuration `AppSettings` at runtime, overriding existing settings.</span></span> 
* <span data-ttu-id="04214-145">Aplikace PHP, Python, Java a uzel mají přístup k tato nastavení jako proměnné prostředí za běhu.</span><span class="sxs-lookup"><span data-stu-id="04214-145">PHP, Python, Java and Node applications can access these settings as environment variables at runtime.</span></span> <span data-ttu-id="04214-146">Pro každé nastavení aplikace jsou vytvořeny dvou proměnných prostředí; jednu s názvem hello určeného hello položky nastavení aplikace a druhý s předponou APPSETTING_.</span><span class="sxs-lookup"><span data-stu-id="04214-146">For each app setting, two environment variables are created; one with hello name specified by hello app setting entry, and another with a prefix of APPSETTING_.</span></span> <span data-ttu-id="04214-147">Oba obsahují hello stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="04214-147">Both contain hello same value.</span></span>

### <a name="connection-strings"></a><span data-ttu-id="04214-148">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="04214-148">Connection strings</span></span>
<span data-ttu-id="04214-149">Připojovací řetězce pro odkazované zdroje.</span><span class="sxs-lookup"><span data-stu-id="04214-149">Connection strings for linked resources.</span></span> 

<span data-ttu-id="04214-150">Pro aplikace .NET, tyto připojovací řetězce jsou vloženy do konfiguraci .NET `connectionStrings` nastavení v době běhu přepsání existujících položek, kde klíč hello rovná hello název propojené databáze.</span><span class="sxs-lookup"><span data-stu-id="04214-150">For .NET apps, these connection strings are injected into your .NET configuration `connectionStrings` settings at runtime, overriding existing entries where hello key equals hello linked database name.</span></span> 

<span data-ttu-id="04214-151">Pro aplikace PHP, Python, Java a uzel budou tato nastavení k dispozici jako proměnné prostředí v době běhu předponu hello typ připojení.</span><span class="sxs-lookup"><span data-stu-id="04214-151">For PHP, Python, Java and Node applications, these settings will be available as environment variables at runtime, prefixed with hello connection type.</span></span> <span data-ttu-id="04214-152">předpony proměnné prostředí Hello jsou následující:</span><span class="sxs-lookup"><span data-stu-id="04214-152">hello environment variable prefixes are as follows:</span></span> 

* <span data-ttu-id="04214-153">SQL Server:`SQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="04214-153">SQL Server: `SQLCONNSTR_`</span></span>
* <span data-ttu-id="04214-154">MySQL:`MYSQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="04214-154">MySQL: `MYSQLCONNSTR_`</span></span>
* <span data-ttu-id="04214-155">Databáze SQL:`SQLAZURECONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="04214-155">SQL Database: `SQLAZURECONNSTR_`</span></span>
* <span data-ttu-id="04214-156">Vlastní:`CUSTOMCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="04214-156">Custom: `CUSTOMCONNSTR_`</span></span>

<span data-ttu-id="04214-157">Například, pokud byly s názvem připojovací řetězec databáze MySql `connectionstring1`, by přístupná prostřednictvím proměnné prostředí hello `MYSQLCONNSTR_connectionString1`.</span><span class="sxs-lookup"><span data-stu-id="04214-157">For example, if a MySql connection string were named `connectionstring1`, it would be accessed through hello environment variable `MYSQLCONNSTR_connectionString1`.</span></span>

### <a name="default-documents"></a><span data-ttu-id="04214-158">Výchozí dokumenty</span><span class="sxs-lookup"><span data-stu-id="04214-158">Default documents</span></span>
<span data-ttu-id="04214-159">výchozí dokument Hello je hello webové stránky, která se zobrazí v hello adresy URL kořenového adresáře pro web.</span><span class="sxs-lookup"><span data-stu-id="04214-159">hello default document is hello web page that is displayed at hello root URL for a website.</span></span>  <span data-ttu-id="04214-160">první odpovídající soubor Hello v seznamu hello se používá.</span><span class="sxs-lookup"><span data-stu-id="04214-160">hello first matching file in hello list is used.</span></span> 

<span data-ttu-id="04214-161">Webové aplikace může použít modulů na adresu URL na základě trasy, že místo obsluhující statický obsah, v takovém případě že je jako takový výchozí dokument.</span><span class="sxs-lookup"><span data-stu-id="04214-161">Web apps might use modules that route based on URL, rather than serving static content, in which case there is no default document as such.</span></span>    

### <a name="handler-mappings"></a><span data-ttu-id="04214-162">Mapování obslužných rutin</span><span class="sxs-lookup"><span data-stu-id="04214-162">Handler mappings</span></span>
<span data-ttu-id="04214-163">Pomocí této oblasti tooadd vlastní skript procesory toohandle požadavků pro konkrétní přípony souborů.</span><span class="sxs-lookup"><span data-stu-id="04214-163">Use this area tooadd custom script processors toohandle requests for specific file extensions.</span></span> 

* <span data-ttu-id="04214-164">**Rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="04214-164">**Extension**.</span></span> <span data-ttu-id="04214-165">Hello souboru rozšíření toobe zpracovává například *.php nebo handler.fcgi.</span><span class="sxs-lookup"><span data-stu-id="04214-165">hello file extension toobe handled, such as *.php or handler.fcgi.</span></span> 
* <span data-ttu-id="04214-166">**Skript procesoru cesta**.</span><span class="sxs-lookup"><span data-stu-id="04214-166">**Script Processor Path**.</span></span> <span data-ttu-id="04214-167">absolutní cesta Hello hello skriptu procesoru.</span><span class="sxs-lookup"><span data-stu-id="04214-167">hello absolute path of hello script processor.</span></span> <span data-ttu-id="04214-168">Požadavky toofiles splňujících příponu souboru hello budou zpracovány procesorem hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="04214-168">Requests toofiles that match hello file extension will be processed by hello script processor.</span></span> <span data-ttu-id="04214-169">Použijte hello cestu `D:\home\site\wwwroot` toorefer tooyour aplikace kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="04214-169">Use hello path `D:\home\site\wwwroot` toorefer tooyour app's root directory.</span></span>
* <span data-ttu-id="04214-170">**Další argumenty**.</span><span class="sxs-lookup"><span data-stu-id="04214-170">**Additional Arguments**.</span></span> <span data-ttu-id="04214-171">Volitelné argumenty příkazového řádku pro procesor skriptu hello</span><span class="sxs-lookup"><span data-stu-id="04214-171">Optional command-line arguments for hello script processor</span></span> 

### <a name="virtual-applications-and-directories"></a><span data-ttu-id="04214-172">Virtuální aplikace a adresáře</span><span class="sxs-lookup"><span data-stu-id="04214-172">Virtual applications and directories</span></span>
<span data-ttu-id="04214-173">tooconfigure virtuální aplikace a adresáře, zadejte každý virtuální adresář a kořenové složky jeho odpovídající fyzická cesta relativní toohello webu.</span><span class="sxs-lookup"><span data-stu-id="04214-173">tooconfigure virtual applications and directories, specify each virtual directory and its corresponding physical path relative toohello website root.</span></span> <span data-ttu-id="04214-174">Volitelně můžete vybrat hello **aplikace** políčko toomark virtuální adresář jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="04214-174">Optionally, you can select hello **Application** checkbox toomark a virtual directory as an application.</span></span>

## <a name="enabling-diagnostic-logs"></a><span data-ttu-id="04214-175">Povolení diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="04214-175">Enabling diagnostic logs</span></span>
<span data-ttu-id="04214-176">diagnostické protokoly tooenable:</span><span class="sxs-lookup"><span data-stu-id="04214-176">tooenable diagnostic logs:</span></span>

1. <span data-ttu-id="04214-177">V okně hello pro vaši webovou aplikaci, klikněte na tlačítko **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="04214-177">In hello blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="04214-178">Klikněte na tlačítko **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="04214-178">Click **Diagnostic logs**.</span></span> 

<span data-ttu-id="04214-179">Možnosti pro zápis z webové aplikace, která podporuje protokolování diagnostických protokolů:</span><span class="sxs-lookup"><span data-stu-id="04214-179">Options for writing diagnostic logs from a web application that supports logging:</span></span> 

* <span data-ttu-id="04214-180">**Protokolování aplikací**.</span><span class="sxs-lookup"><span data-stu-id="04214-180">**Application Logging**.</span></span> <span data-ttu-id="04214-181">Zapíše protokoly aplikací toohello systému souborů.</span><span class="sxs-lookup"><span data-stu-id="04214-181">Writes application logs toohello file system.</span></span> <span data-ttu-id="04214-182">Protokolování vydrží po dobu 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="04214-182">Logging lasts for a period of 12 hours.</span></span> 

<span data-ttu-id="04214-183">**Úroveň**.</span><span class="sxs-lookup"><span data-stu-id="04214-183">**Level**.</span></span> <span data-ttu-id="04214-184">Pokud je povoleno protokolování aplikací, tato možnost určuje hello množství informací, které se bude zaznamenaná (chyba, upozornění, informace nebo podrobné).</span><span class="sxs-lookup"><span data-stu-id="04214-184">When application logging is enabled, this option specifies hello amount of information that will be recorded (Error, Warning, Information, or Verbose).</span></span>

<span data-ttu-id="04214-185">**Protokolování webového serveru**.</span><span class="sxs-lookup"><span data-stu-id="04214-185">**Web server logging**.</span></span> <span data-ttu-id="04214-186">Protokoly se ukládají do hello rozšířený formát protokolu W3C souboru.</span><span class="sxs-lookup"><span data-stu-id="04214-186">Logs are saved in hello W3C extended log file format.</span></span> 

<span data-ttu-id="04214-187">**Podrobné chybové zprávy**.</span><span class="sxs-lookup"><span data-stu-id="04214-187">**Detailed error messages**.</span></span> <span data-ttu-id="04214-188">Uloží podrobné chybové zprávy soubory .htm.</span><span class="sxs-lookup"><span data-stu-id="04214-188">Saves detailed error messages .htm files.</span></span> <span data-ttu-id="04214-189">Hello soubory jsou uloženy v části /LogFiles/DetailedErrors.</span><span class="sxs-lookup"><span data-stu-id="04214-189">hello files are saved under /LogFiles/DetailedErrors.</span></span> 

<span data-ttu-id="04214-190">**Trasování neúspěšných žádostí**.</span><span class="sxs-lookup"><span data-stu-id="04214-190">**Failed request tracing**.</span></span> <span data-ttu-id="04214-191">Protokoly se nezdařilo požadavky tooXML soubory.</span><span class="sxs-lookup"><span data-stu-id="04214-191">Logs failed requests tooXML files.</span></span> <span data-ttu-id="04214-192">Hello soubory jsou uloženy v rámci/LogFiles/W3SVC*xxx*, kde xxx je jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="04214-192">hello files are saved under /LogFiles/W3SVC*xxx*, where xxx is a unique identifier.</span></span> <span data-ttu-id="04214-193">Tato složka obsahuje soubor XSL a jeden nebo více souborů XML.</span><span class="sxs-lookup"><span data-stu-id="04214-193">This folder contains an XSL file and one or more XML files.</span></span> <span data-ttu-id="04214-194">Zda toodownload hello soubor XSL proveďte, protože poskytuje funkce pro formátování a filtrování hello obsah souborů XML hello.</span><span class="sxs-lookup"><span data-stu-id="04214-194">Make sure toodownload hello XSL file, because it provides functionality for formatting and filtering hello contents of hello XML files.</span></span>

<span data-ttu-id="04214-195">soubory protokolu hello tooview, musíte vytvořit přihlašovací údaje serveru FTP, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="04214-195">tooview hello log files, you must create FTP credentials, as follows:</span></span>

1. <span data-ttu-id="04214-196">V okně hello pro vaši webovou aplikaci, klikněte na tlačítko **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="04214-196">In hello blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="04214-197">Klikněte na tlačítko **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="04214-197">Click **Deployment credentials**.</span></span>
3. <span data-ttu-id="04214-198">Zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="04214-198">Enter a user name and password.</span></span>
4. <span data-ttu-id="04214-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="04214-199">Click **Save**.</span></span>

![Nastavení přihlašovacích údajů nasazení][configure03]

<span data-ttu-id="04214-201">Hello úplné FTP uživatelské jméno je "app\username", kde *aplikace* je hello název webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="04214-201">hello full FTP user name is “app\username” where *app* is hello name of your web app.</span></span> <span data-ttu-id="04214-202">Hello uživatelské jméno je uvedena v okně webové aplikace hello, v části **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="04214-202">hello username is listed in hello web app blade, under **Essentials**.</span></span>  

![Přihlašovací údaje pro nasazení serveru FTP][configure02]

## <a name="other-configuration-tasks"></a><span data-ttu-id="04214-204">Další konfigurační úlohy</span><span class="sxs-lookup"><span data-stu-id="04214-204">Other configuration tasks</span></span>
### <a name="ssl"></a><span data-ttu-id="04214-205">PROTOKOL SSL</span><span class="sxs-lookup"><span data-stu-id="04214-205">SSL</span></span>
<span data-ttu-id="04214-206">V režimu Basic nebo Standard můžete nahrát certifikáty SSL pro vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="04214-206">In Basic or Standard mode, you can upload SSL certificates for a custom domain.</span></span> <span data-ttu-id="04214-207">Další informace najdete v tématu [Povolit HTTPS pro webovou aplikaci].</span><span class="sxs-lookup"><span data-stu-id="04214-207">For more information, see [Enable HTTPS for a web app].</span></span> 

<span data-ttu-id="04214-208">Klikněte na tlačítko tooview nahrané certifikáty, **všechna nastavení** > **vlastní domény a SSL**.</span><span class="sxs-lookup"><span data-stu-id="04214-208">tooview your uploaded certificates, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="domain-names"></a><span data-ttu-id="04214-209">Názvy domén</span><span class="sxs-lookup"><span data-stu-id="04214-209">Domain names</span></span>
<span data-ttu-id="04214-210">Přidáte vlastní názvy domén pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="04214-210">Add custom domain names for your web app.</span></span> <span data-ttu-id="04214-211">Další informace najdete v tématu [Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="04214-211">For more information, see [Configure a custom domain name for a web app in Azure App Service].</span></span>

<span data-ttu-id="04214-212">Klikněte na názvy domén, tooview **všechna nastavení** > **vlastní domény a SSL**.</span><span class="sxs-lookup"><span data-stu-id="04214-212">tooview your domain names, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="deployments"></a><span data-ttu-id="04214-213">Nasazení</span><span class="sxs-lookup"><span data-stu-id="04214-213">Deployments</span></span>
* <span data-ttu-id="04214-214">Nastavte průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="04214-214">Set up continuous deployment.</span></span> <span data-ttu-id="04214-215">V tématu [toodeploy pomocí Git ve službě Azure App Service Web Apps](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="04214-215">See [Using Git toodeploy Web Apps in Azure App Service](web-sites-deploy.md).</span></span>
* <span data-ttu-id="04214-216">Nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="04214-216">Deployment slots.</span></span> <span data-ttu-id="04214-217">V tématu [nasazení tooStaging prostředí pro webové aplikace v Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="04214-217">See [Deploy tooStaging Environments for Web Apps in Azure App Service].</span></span>

<span data-ttu-id="04214-218">Klikněte na nasazovací sloty, tooview **všechna nastavení** > **nasazovací sloty**.</span><span class="sxs-lookup"><span data-stu-id="04214-218">tooview your deployment slots, click **All Settings** > **Deployment slots**.</span></span>

### <a name="monitoring"></a><span data-ttu-id="04214-219">Monitorování</span><span class="sxs-lookup"><span data-stu-id="04214-219">Monitoring</span></span>
<span data-ttu-id="04214-220">V režimu Basic nebo Standard můžete otestovat hello dostupnosti koncových bodů protokolu HTTP nebo HTTPS, z až toothree zeměpisné polohy umístění.</span><span class="sxs-lookup"><span data-stu-id="04214-220">In Basic or Standard mode, you can  test hello availability of HTTP or HTTPS endpoints, from up toothree geo-distributed locations.</span></span> <span data-ttu-id="04214-221">Monitorování test se nezdaří, pokud hello kódu odpovědi HTTP dojde k chybě (4xx nebo 5xx) nebo hello odpovědi trvá déle než 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="04214-221">A monitoring test fails if hello HTTP response code is an error (4xx or 5xx) or hello response takes more than 30 seconds.</span></span> <span data-ttu-id="04214-222">Koncový bod je považován za dostupný, pokud hello monitorovací testy úspěšné ze všech hello zadané umístění.</span><span class="sxs-lookup"><span data-stu-id="04214-222">An endpoint is considered available if hello monitoring tests succeed from all hello specified locations.</span></span> 

<span data-ttu-id="04214-223">Další informace najdete v tématu [postupy: sledování stavu koncový bod webové].</span><span class="sxs-lookup"><span data-stu-id="04214-223">For more information, see [How to: Monitor web endpoint status].</span></span>

> [!NOTE]
> <span data-ttu-id="04214-224">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service], kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="04214-224">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="04214-225">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="04214-225">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="04214-226">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04214-226">Next steps</span></span>
* <span data-ttu-id="04214-227">[Konfigurace vlastní domény ve službě Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="04214-227">[Configure a custom domain name in Azure App Service]</span></span>
* <span data-ttu-id="04214-228">[Povolit HTTPS pro aplikace v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="04214-228">[Enable HTTPS for an app in Azure App Service]</span></span>
* <span data-ttu-id="04214-229">[Škálování webové aplikace v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="04214-229">[Scale a web app in Azure App Service]</span></span>
* <span data-ttu-id="04214-230">[Základní informace o monitorování pro webové aplikace v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="04214-230">[Monitoring basics for Web Apps in Azure App Service]</span></span>

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[portálu Azure]: https://portal.azure.com/
[Konfigurace vlastní domény ve službě Azure App Service]: ./app-service-web-tutorial-custom-domain.md
[nasazení tooStaging prostředí pro webové aplikace v Azure App Service]: ./web-sites-staged-publishing.md
[Povolit HTTPS pro aplikace v Azure App Service]: ./app-service-web-tutorial-custom-ssl.md
[postupy: sledování stavu koncový bod webové]: http://go.microsoft.com/fwLink/?LinkID=279906
[Základní informace o monitorování pro webové aplikace v Azure App Service]: ./web-sites-monitor.md
[režim kanálů]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Škálování webové aplikace v Azure App Service]: ./web-sites-scale.md
[socket.io]: ./web-sites-nodejs-chat-app-socketio.md
[vyzkoušet službu App Service]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
