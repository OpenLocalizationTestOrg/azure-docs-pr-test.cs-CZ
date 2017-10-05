---
title: "Konfigurace webových aplikací v prostředí Azure App Service"
description: "Postup konfigurace webové aplikace v Azure App Services"
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
ms.openlocfilehash: cacbcf879555907f81d824dc1069b05579dca010
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a><span data-ttu-id="96081-103">Konfigurace webových aplikací v prostředí Azure App Service</span><span class="sxs-lookup"><span data-stu-id="96081-103">Configure web apps in Azure App Service</span></span>
<span data-ttu-id="96081-104">Toto téma vysvětluje, jak nakonfigurovat webovou aplikaci pomocí [portálu Azure].</span><span class="sxs-lookup"><span data-stu-id="96081-104">This topic explains how to configure a web app using the [Azure Portal].</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a><span data-ttu-id="96081-105">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="96081-105">Application settings</span></span>
1. <span data-ttu-id="96081-106">V [portálu Azure], otevře se okno pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96081-106">In the [Azure Portal], open the blade for the web app.</span></span>
2. <span data-ttu-id="96081-107">Klikněte na tlačítko **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="96081-107">Click **All Settings**.</span></span>
3. <span data-ttu-id="96081-108">Klikněte na tlačítko **nastavení aplikace**.</span><span class="sxs-lookup"><span data-stu-id="96081-108">Click **Application Settings**.</span></span>

![Nastavení aplikace][configure01]

<span data-ttu-id="96081-110">**Nastavení aplikace** okno obsahuje nastavení, které jsou seskupené v rámci několika kategorií.</span><span class="sxs-lookup"><span data-stu-id="96081-110">The **Application settings** blade has settings grouped under several categories.</span></span>

### <a name="general-settings"></a><span data-ttu-id="96081-111">Obecná nastavení</span><span class="sxs-lookup"><span data-stu-id="96081-111">General settings</span></span>
<span data-ttu-id="96081-112">**Verze Framework**.</span><span class="sxs-lookup"><span data-stu-id="96081-112">**Framework versions**.</span></span> <span data-ttu-id="96081-113">Pokud vaše aplikace používá tyto architektury, nastavte tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="96081-113">Set these options if your app uses any these frameworks:</span></span> 

* <span data-ttu-id="96081-114">**Rozhraní .NET framework**: nastavení verze rozhraní .NET framework.</span><span class="sxs-lookup"><span data-stu-id="96081-114">**.NET Framework**: Set the .NET framework version.</span></span> 
* <span data-ttu-id="96081-115">**PHP**: nastavte verzi PHP, nebo **OFF** zakázat PHP.</span><span class="sxs-lookup"><span data-stu-id="96081-115">**PHP**: Set the PHP version, or **OFF** to disable PHP.</span></span> 
* <span data-ttu-id="96081-116">**Java**: Vyberte verzi jazyka Java nebo **OFF** zakázat Java.</span><span class="sxs-lookup"><span data-stu-id="96081-116">**Java**: Select the Java version or **OFF** to disable Java.</span></span> <span data-ttu-id="96081-117">Použití **webový kontejner** možnost si vybrat mezi verzí Tomcat a Jetty.</span><span class="sxs-lookup"><span data-stu-id="96081-117">Use the **Web Container** option to choose between Tomcat and Jetty versions.</span></span>
* <span data-ttu-id="96081-118">**Python**: Vyberte verzi jazyka Python, nebo **OFF** zakázat Python.</span><span class="sxs-lookup"><span data-stu-id="96081-118">**Python**: Select the Python version, or **OFF** to disable Python.</span></span>

<span data-ttu-id="96081-119">Technické z důvodů povolení pro aplikace Java zakáže možnosti .NET, PHP a Python.</span><span class="sxs-lookup"><span data-stu-id="96081-119">For technical reasons, enabling Java for your app disables the .NET, PHP, and Python options.</span></span>

<span data-ttu-id="96081-120"><a name="platform"></a>
**Platforma**.</span><span class="sxs-lookup"><span data-stu-id="96081-120"><a name="platform"></a>
**Platform**.</span></span> <span data-ttu-id="96081-121">Vybere, zda běží vaše webová aplikace v prostředí 32bitové nebo 64bitové verze.</span><span class="sxs-lookup"><span data-stu-id="96081-121">Selects whether your web app runs in a 32-bit or 64-bit environment.</span></span> <span data-ttu-id="96081-122">64bitová verze prostředí vyžaduje režimu Basic nebo Standard.</span><span class="sxs-lookup"><span data-stu-id="96081-122">The 64-bit environment requires Basic or Standard mode.</span></span> <span data-ttu-id="96081-123">Uvolněte a režimy sdílené vždy spustit v prostředí 32-bit.</span><span class="sxs-lookup"><span data-stu-id="96081-123">Free and Shared modes always run in a 32-bit environment.</span></span>

<span data-ttu-id="96081-124">**Webové sokety**.</span><span class="sxs-lookup"><span data-stu-id="96081-124">**Web Sockets**.</span></span> <span data-ttu-id="96081-125">Nastavit **ON** povolit protokol WebSocket; například, pokud vaše webová aplikace používá [ASP.NET SignalR] nebo [socket.io].</span><span class="sxs-lookup"><span data-stu-id="96081-125">Set **ON** to enable the WebSocket protocol; for example, if your web app uses [ASP.NET SignalR] or [socket.io].</span></span>

<span data-ttu-id="96081-126"><a name="alwayson"></a>
**Always On**.</span><span class="sxs-lookup"><span data-stu-id="96081-126"><a name="alwayson"></a>
**Always On**.</span></span> <span data-ttu-id="96081-127">Ve výchozím nastavení jsou uvolněna webové aplikace, pokud jsou některé dobu nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="96081-127">By default, web apps are unloaded if they are idle for some period of time.</span></span> <span data-ttu-id="96081-128">To umožňuje ušetřit prostředky systému.</span><span class="sxs-lookup"><span data-stu-id="96081-128">This lets the system conserve resources.</span></span> <span data-ttu-id="96081-129">V režimu Basic nebo Standard, povolit **Always On** udržovat aplikaci načíst vždy.</span><span class="sxs-lookup"><span data-stu-id="96081-129">In Basic or Standard mode, you can enable **Always On** to keep the app loaded all the time.</span></span> <span data-ttu-id="96081-130">Pokud vaše aplikace běží nepřetržité webové úlohy nebo běží webové úlohy aktivaci pomocí výrazu CRON, měli byste povolit **Always On**, nebo nemusí spolehlivě spuštění webové úlohy.</span><span class="sxs-lookup"><span data-stu-id="96081-130">If your app runs continuous WebJobs or runs WebJobs triggered using a CRON expression, you should enable **Always On**, or the web jobs may not run reliably.</span></span>

<span data-ttu-id="96081-131">**Spravované verze kanálu**.</span><span class="sxs-lookup"><span data-stu-id="96081-131">**Managed Pipeline Version**.</span></span> <span data-ttu-id="96081-132">Nastaví služby IIS [režim kanálů].</span><span class="sxs-lookup"><span data-stu-id="96081-132">Sets the IIS [pipeline mode].</span></span> <span data-ttu-id="96081-133">Pokud nemáte aplikaci ze starší verze, která vyžaduje starší verze služby IIS, ponechte této sady na integrovaný (výchozí).</span><span class="sxs-lookup"><span data-stu-id="96081-133">Leave this set to Integrated (the default) unless you have a legacy app that requires an older version of IIS.</span></span>

<span data-ttu-id="96081-134">**Automatické prohození**.</span><span class="sxs-lookup"><span data-stu-id="96081-134">**Auto Swap**.</span></span> <span data-ttu-id="96081-135">Pokud povolíte automatické prohození pro slot nasazení, služby App Service automaticky Prohodit webové aplikace do produkčního prostředí, při nabízené aktualizace pro tento slot.</span><span class="sxs-lookup"><span data-stu-id="96081-135">If you enable Auto Swap for a deployment slot, App Service will automatically swap the web app into production when you push an update to that slot.</span></span> <span data-ttu-id="96081-136">Další informace najdete v tématu [nasadit do přípravné sloty pro webové aplikace v Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="96081-136">For more information, see [Deploy to staging slots for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span>

### <a name="debugging"></a><span data-ttu-id="96081-137">Ladění</span><span class="sxs-lookup"><span data-stu-id="96081-137">Debugging</span></span>
<span data-ttu-id="96081-138">**Vzdálené ladění**.</span><span class="sxs-lookup"><span data-stu-id="96081-138">**Remote Debugging**.</span></span> <span data-ttu-id="96081-139">Umožňuje vzdálené ladění.</span><span class="sxs-lookup"><span data-stu-id="96081-139">Enables remote debugging.</span></span> <span data-ttu-id="96081-140">Když je povolené, můžete v sadě Visual Studio vzdáleného ladicího programu pro připojení přímo k vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="96081-140">When enabled, you can use the remote debugger in Visual Studio to connect directly to your web app.</span></span> <span data-ttu-id="96081-141">Vzdálené ladění zůstane zapnutá 48 hodin.</span><span class="sxs-lookup"><span data-stu-id="96081-141">Remote debugging will remain enabled for 48 hours.</span></span> 

### <a name="app-settings"></a><span data-ttu-id="96081-142">Nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="96081-142">App settings</span></span>
<span data-ttu-id="96081-143">Tato část obsahuje dvojice název/hodnota, které vaší webové aplikace načte při spuštění.</span><span class="sxs-lookup"><span data-stu-id="96081-143">This section contains name/value pairs that you web app will load on start up.</span></span> 

* <span data-ttu-id="96081-144">Pro aplikace .NET, tato nastavení jsou vloženy do vaší konfigurace .NET `AppSettings` v době běhu přepsání stávajícího nastavení.</span><span class="sxs-lookup"><span data-stu-id="96081-144">For .NET apps, these settings are injected into your .NET configuration `AppSettings` at runtime, overriding existing settings.</span></span> 
* <span data-ttu-id="96081-145">Aplikace PHP, Python, Java a uzel mají přístup k tato nastavení jako proměnné prostředí za běhu.</span><span class="sxs-lookup"><span data-stu-id="96081-145">PHP, Python, Java and Node applications can access these settings as environment variables at runtime.</span></span> <span data-ttu-id="96081-146">Pro každé nastavení aplikace jsou vytvořeny dvou proměnných prostředí; jednu s názvem zadaným v položce nastavení aplikace a druhý s předponou APPSETTING_.</span><span class="sxs-lookup"><span data-stu-id="96081-146">For each app setting, two environment variables are created; one with the name specified by the app setting entry, and another with a prefix of APPSETTING_.</span></span> <span data-ttu-id="96081-147">Oba obsahují stejnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96081-147">Both contain the same value.</span></span>

### <a name="connection-strings"></a><span data-ttu-id="96081-148">Připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="96081-148">Connection strings</span></span>
<span data-ttu-id="96081-149">Připojovací řetězce pro odkazované zdroje.</span><span class="sxs-lookup"><span data-stu-id="96081-149">Connection strings for linked resources.</span></span> 

<span data-ttu-id="96081-150">Pro aplikace .NET, tyto připojovací řetězce jsou vloženy do konfiguraci .NET `connectionStrings` nastavení v době běhu přepsání existujících položek, kde klíč rovná název propojené databáze.</span><span class="sxs-lookup"><span data-stu-id="96081-150">For .NET apps, these connection strings are injected into your .NET configuration `connectionStrings` settings at runtime, overriding existing entries where the key equals the linked database name.</span></span> 

<span data-ttu-id="96081-151">Pro aplikace PHP, Python, Java a uzel budou tato nastavení k dispozici jako proměnné prostředí v době běhu předponu typ připojení.</span><span class="sxs-lookup"><span data-stu-id="96081-151">For PHP, Python, Java and Node applications, these settings will be available as environment variables at runtime, prefixed with the connection type.</span></span> <span data-ttu-id="96081-152">Předpony proměnné prostředí jsou následující:</span><span class="sxs-lookup"><span data-stu-id="96081-152">The environment variable prefixes are as follows:</span></span> 

* <span data-ttu-id="96081-153">SQL Server:`SQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="96081-153">SQL Server: `SQLCONNSTR_`</span></span>
* <span data-ttu-id="96081-154">MySQL:`MYSQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="96081-154">MySQL: `MYSQLCONNSTR_`</span></span>
* <span data-ttu-id="96081-155">Databáze SQL:`SQLAZURECONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="96081-155">SQL Database: `SQLAZURECONNSTR_`</span></span>
* <span data-ttu-id="96081-156">Vlastní:`CUSTOMCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="96081-156">Custom: `CUSTOMCONNSTR_`</span></span>

<span data-ttu-id="96081-157">Například, pokud byly s názvem připojovací řetězec databáze MySql `connectionstring1`, by přístupná prostřednictvím proměnné prostředí `MYSQLCONNSTR_connectionString1`.</span><span class="sxs-lookup"><span data-stu-id="96081-157">For example, if a MySql connection string were named `connectionstring1`, it would be accessed through the environment variable `MYSQLCONNSTR_connectionString1`.</span></span>

### <a name="default-documents"></a><span data-ttu-id="96081-158">Výchozí dokumenty</span><span class="sxs-lookup"><span data-stu-id="96081-158">Default documents</span></span>
<span data-ttu-id="96081-159">Výchozí dokument je webové stránky, která se zobrazí na adresy URL kořenového adresáře pro web.</span><span class="sxs-lookup"><span data-stu-id="96081-159">The default document is the web page that is displayed at the root URL for a website.</span></span>  <span data-ttu-id="96081-160">První odpovídající soubor v seznamu se používá.</span><span class="sxs-lookup"><span data-stu-id="96081-160">The first matching file in the list is used.</span></span> 

<span data-ttu-id="96081-161">Webové aplikace může použít modulů na adresu URL na základě trasy, že místo obsluhující statický obsah, v takovém případě že je jako takový výchozí dokument.</span><span class="sxs-lookup"><span data-stu-id="96081-161">Web apps might use modules that route based on URL, rather than serving static content, in which case there is no default document as such.</span></span>    

### <a name="handler-mappings"></a><span data-ttu-id="96081-162">Mapování obslužných rutin</span><span class="sxs-lookup"><span data-stu-id="96081-162">Handler mappings</span></span>
<span data-ttu-id="96081-163">Pomocí této oblasti můžete přidat vlastní skript procesorů pro zpracování požadavků pro konkrétní přípony souborů.</span><span class="sxs-lookup"><span data-stu-id="96081-163">Use this area to add custom script processors to handle requests for specific file extensions.</span></span> 

* <span data-ttu-id="96081-164">**Rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="96081-164">**Extension**.</span></span> <span data-ttu-id="96081-165">Přípona souboru ke zpracování, jako je například *.php nebo handler.fcgi.</span><span class="sxs-lookup"><span data-stu-id="96081-165">The file extension to be handled, such as *.php or handler.fcgi.</span></span> 
* <span data-ttu-id="96081-166">**Skript procesoru cesta**.</span><span class="sxs-lookup"><span data-stu-id="96081-166">**Script Processor Path**.</span></span> <span data-ttu-id="96081-167">Absolutní cesta mapě skriptů.</span><span class="sxs-lookup"><span data-stu-id="96081-167">The absolute path of the script processor.</span></span> <span data-ttu-id="96081-168">Požadavky na soubory, které odpovídají přípona souboru budou zpracovány procesorem skriptu.</span><span class="sxs-lookup"><span data-stu-id="96081-168">Requests to files that match the file extension will be processed by the script processor.</span></span> <span data-ttu-id="96081-169">Použijte cestu `D:\home\site\wwwroot` odkazovat na kořenový adresář vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="96081-169">Use the path `D:\home\site\wwwroot` to refer to your app's root directory.</span></span>
* <span data-ttu-id="96081-170">**Další argumenty**.</span><span class="sxs-lookup"><span data-stu-id="96081-170">**Additional Arguments**.</span></span> <span data-ttu-id="96081-171">Volitelné argumenty příkazového řádku pro mapě skriptů</span><span class="sxs-lookup"><span data-stu-id="96081-171">Optional command-line arguments for the script processor</span></span> 

### <a name="virtual-applications-and-directories"></a><span data-ttu-id="96081-172">Virtuální aplikace a adresáře</span><span class="sxs-lookup"><span data-stu-id="96081-172">Virtual applications and directories</span></span>
<span data-ttu-id="96081-173">Chcete-li nakonfigurovat virtuální aplikace a adresáře, zadejte každý virtuální adresář a jeho odpovídající fyzická cesta relativní vůči kořenovému adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="96081-173">To configure virtual applications and directories, specify each virtual directory and its corresponding physical path relative to the website root.</span></span> <span data-ttu-id="96081-174">Volitelně můžete vybrat **aplikace** zaškrtávací políčko k označení virtuální adresář jako aplikace.</span><span class="sxs-lookup"><span data-stu-id="96081-174">Optionally, you can select the **Application** checkbox to mark a virtual directory as an application.</span></span>

## <a name="enabling-diagnostic-logs"></a><span data-ttu-id="96081-175">Povolení diagnostických protokolů</span><span class="sxs-lookup"><span data-stu-id="96081-175">Enabling diagnostic logs</span></span>
<span data-ttu-id="96081-176">Povolení diagnostických protokolů:</span><span class="sxs-lookup"><span data-stu-id="96081-176">To enable diagnostic logs:</span></span>

1. <span data-ttu-id="96081-177">V okně vaší webové aplikace, klikněte na **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="96081-177">In the blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="96081-178">Klikněte na tlačítko **diagnostické protokoly**.</span><span class="sxs-lookup"><span data-stu-id="96081-178">Click **Diagnostic logs**.</span></span> 

<span data-ttu-id="96081-179">Možnosti pro zápis z webové aplikace, která podporuje protokolování diagnostických protokolů:</span><span class="sxs-lookup"><span data-stu-id="96081-179">Options for writing diagnostic logs from a web application that supports logging:</span></span> 

* <span data-ttu-id="96081-180">**Protokolování aplikací**.</span><span class="sxs-lookup"><span data-stu-id="96081-180">**Application Logging**.</span></span> <span data-ttu-id="96081-181">Zapisuje protokoly aplikace do systému souborů.</span><span class="sxs-lookup"><span data-stu-id="96081-181">Writes application logs to the file system.</span></span> <span data-ttu-id="96081-182">Protokolování vydrží po dobu 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="96081-182">Logging lasts for a period of 12 hours.</span></span> 

<span data-ttu-id="96081-183">**Úroveň**.</span><span class="sxs-lookup"><span data-stu-id="96081-183">**Level**.</span></span> <span data-ttu-id="96081-184">Pokud je povoleno protokolování aplikací, tato možnost určuje, že množství informací, které budou zaznamenány (chyba, upozornění, informace nebo Verbose).</span><span class="sxs-lookup"><span data-stu-id="96081-184">When application logging is enabled, this option specifies the amount of information that will be recorded (Error, Warning, Information, or Verbose).</span></span>

<span data-ttu-id="96081-185">**Protokolování webového serveru**.</span><span class="sxs-lookup"><span data-stu-id="96081-185">**Web server logging**.</span></span> <span data-ttu-id="96081-186">Protokoly se ukládají do formátu souboru rozšířené protokolu W3C.</span><span class="sxs-lookup"><span data-stu-id="96081-186">Logs are saved in the W3C extended log file format.</span></span> 

<span data-ttu-id="96081-187">**Podrobné chybové zprávy**.</span><span class="sxs-lookup"><span data-stu-id="96081-187">**Detailed error messages**.</span></span> <span data-ttu-id="96081-188">Uloží podrobné chybové zprávy soubory .htm.</span><span class="sxs-lookup"><span data-stu-id="96081-188">Saves detailed error messages .htm files.</span></span> <span data-ttu-id="96081-189">Soubory jsou uložena pod /LogFiles/DetailedErrors.</span><span class="sxs-lookup"><span data-stu-id="96081-189">The files are saved under /LogFiles/DetailedErrors.</span></span> 

<span data-ttu-id="96081-190">**Trasování neúspěšných žádostí**.</span><span class="sxs-lookup"><span data-stu-id="96081-190">**Failed request tracing**.</span></span> <span data-ttu-id="96081-191">Protokoly neúspěšné požadavky na soubory XML.</span><span class="sxs-lookup"><span data-stu-id="96081-191">Logs failed requests to XML files.</span></span> <span data-ttu-id="96081-192">Soubory uložené pod/LogFiles/W3SVC*xxx*, kde xxx je jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="96081-192">The files are saved under /LogFiles/W3SVC*xxx*, where xxx is a unique identifier.</span></span> <span data-ttu-id="96081-193">Tato složka obsahuje soubor XSL a jeden nebo více souborů XML.</span><span class="sxs-lookup"><span data-stu-id="96081-193">This folder contains an XSL file and one or more XML files.</span></span> <span data-ttu-id="96081-194">Zajistěte, aby se stáhnout soubor XSL, protože poskytuje funkce pro formátování a filtrování obsah souborů XML.</span><span class="sxs-lookup"><span data-stu-id="96081-194">Make sure to download the XSL file, because it provides functionality for formatting and filtering the contents of the XML files.</span></span>

<span data-ttu-id="96081-195">Chcete-li zobrazit soubory protokolů, musíte vytvořit přihlašovací údaje serveru FTP, následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="96081-195">To view the log files, you must create FTP credentials, as follows:</span></span>

1. <span data-ttu-id="96081-196">V okně vaší webové aplikace, klikněte na **všechna nastavení**.</span><span class="sxs-lookup"><span data-stu-id="96081-196">In the blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="96081-197">Klikněte na tlačítko **přihlašovací údaje nasazení**.</span><span class="sxs-lookup"><span data-stu-id="96081-197">Click **Deployment credentials**.</span></span>
3. <span data-ttu-id="96081-198">Zadejte uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="96081-198">Enter a user name and password.</span></span>
4. <span data-ttu-id="96081-199">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="96081-199">Click **Save**.</span></span>

![Nastavení přihlašovacích údajů nasazení][configure03]

<span data-ttu-id="96081-201">Úplné uživatelské jméno FTP je "app\username", kde *aplikace* je název vaší webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="96081-201">The full FTP user name is “app\username” where *app* is the name of your web app.</span></span> <span data-ttu-id="96081-202">Uživatelské jméno je uvedena v okně webové aplikace v části **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="96081-202">The username is listed in the web app blade, under **Essentials**.</span></span>  

![Přihlašovací údaje pro nasazení serveru FTP][configure02]

## <a name="other-configuration-tasks"></a><span data-ttu-id="96081-204">Další konfigurační úlohy</span><span class="sxs-lookup"><span data-stu-id="96081-204">Other configuration tasks</span></span>
### <a name="ssl"></a><span data-ttu-id="96081-205">PROTOKOL SSL</span><span class="sxs-lookup"><span data-stu-id="96081-205">SSL</span></span>
<span data-ttu-id="96081-206">V režimu Basic nebo Standard můžete nahrát certifikáty SSL pro vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="96081-206">In Basic or Standard mode, you can upload SSL certificates for a custom domain.</span></span> <span data-ttu-id="96081-207">Další informace najdete v tématu [Povolit HTTPS pro webovou aplikaci].</span><span class="sxs-lookup"><span data-stu-id="96081-207">For more information, see [Enable HTTPS for a web app].</span></span> 

<span data-ttu-id="96081-208">Chcete-li zobrazit nahrané certifikáty, klikněte na tlačítko **všechna nastavení** > **vlastní domény a SSL**.</span><span class="sxs-lookup"><span data-stu-id="96081-208">To view your uploaded certificates, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="domain-names"></a><span data-ttu-id="96081-209">Názvy domén</span><span class="sxs-lookup"><span data-stu-id="96081-209">Domain names</span></span>
<span data-ttu-id="96081-210">Přidáte vlastní názvy domén pro vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96081-210">Add custom domain names for your web app.</span></span> <span data-ttu-id="96081-211">Další informace najdete v tématu [Konfigurace vlastního názvu domény pro webovou aplikaci v Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="96081-211">For more information, see [Configure a custom domain name for a web app in Azure App Service].</span></span>

<span data-ttu-id="96081-212">Chcete-li zobrazit názvy domén, klikněte na tlačítko **všechna nastavení** > **vlastní domény a SSL**.</span><span class="sxs-lookup"><span data-stu-id="96081-212">To view your domain names, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="deployments"></a><span data-ttu-id="96081-213">Nasazení</span><span class="sxs-lookup"><span data-stu-id="96081-213">Deployments</span></span>
* <span data-ttu-id="96081-214">Nastavte průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="96081-214">Set up continuous deployment.</span></span> <span data-ttu-id="96081-215">V tématu [pomocí Git, jak nasadit webové aplikace v Azure App Service](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="96081-215">See [Using Git to deploy Web Apps in Azure App Service](web-sites-deploy.md).</span></span>
* <span data-ttu-id="96081-216">Nasazovací sloty.</span><span class="sxs-lookup"><span data-stu-id="96081-216">Deployment slots.</span></span> <span data-ttu-id="96081-217">V tématu [nasazování do pracovní prostředí pro webové aplikace v Azure App Service].</span><span class="sxs-lookup"><span data-stu-id="96081-217">See [Deploy to Staging Environments for Web Apps in Azure App Service].</span></span>

<span data-ttu-id="96081-218">Chcete-li zobrazit nasazovací sloty, klikněte na tlačítko **všechna nastavení** > **nasazovací sloty**.</span><span class="sxs-lookup"><span data-stu-id="96081-218">To view your deployment slots, click **All Settings** > **Deployment slots**.</span></span>

### <a name="monitoring"></a><span data-ttu-id="96081-219">Monitorování</span><span class="sxs-lookup"><span data-stu-id="96081-219">Monitoring</span></span>
<span data-ttu-id="96081-220">V režimu Basic nebo Standard můžete otestovat dostupnost protokolu HTTP nebo HTTPS koncových bodů, z umístění až tři zeměpisné polohy.</span><span class="sxs-lookup"><span data-stu-id="96081-220">In Basic or Standard mode, you can  test the availability of HTTP or HTTPS endpoints, from up to three geo-distributed locations.</span></span> <span data-ttu-id="96081-221">Monitorování test se nezdaří, pokud kód odpovědi HTTP dojde k chybě (4xx nebo 5xx) nebo odpověď trvá déle než 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="96081-221">A monitoring test fails if the HTTP response code is an error (4xx or 5xx) or the response takes more than 30 seconds.</span></span> <span data-ttu-id="96081-222">Koncový bod je považován za dostupný, pokud monitorovací testy úspěšné z určitých umístění.</span><span class="sxs-lookup"><span data-stu-id="96081-222">An endpoint is considered available if the monitoring tests succeed from all the specified locations.</span></span> 

<span data-ttu-id="96081-223">Další informace najdete v tématu [postupy: sledování stavu koncový bod webové].</span><span class="sxs-lookup"><span data-stu-id="96081-223">For more information, see [How to: Monitor web endpoint status].</span></span>

> [!NOTE]
> <span data-ttu-id="96081-224">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service], kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="96081-224">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="96081-225">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="96081-225">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="96081-226">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96081-226">Next steps</span></span>
* <span data-ttu-id="96081-227">[Konfigurace vlastní domény ve službě Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="96081-227">[Configure a custom domain name in Azure App Service]</span></span>
* <span data-ttu-id="96081-228">[Povolit HTTPS pro aplikace v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="96081-228">[Enable HTTPS for an app in Azure App Service]</span></span>
* <span data-ttu-id="96081-229">[Škálování webové aplikace v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="96081-229">[Scale a web app in Azure App Service]</span></span>
* <span data-ttu-id="96081-230">[Základní informace o monitorování pro webové aplikace v Azure App Service]</span><span class="sxs-lookup"><span data-stu-id="96081-230">[Monitoring basics for Web Apps in Azure App Service]</span></span>

<!-- URL List -->

<span data-ttu-id="96081-231">[ASP.NET SignalR]: http://www.asp.net/signalr</span><span class="sxs-lookup"><span data-stu-id="96081-231">[ASP.NET SignalR]: http://www.asp.net/signalr</span></span>
<span data-ttu-id="96081-232">[portálu Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="96081-232">[Azure Portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="96081-233">[Konfigurace vlastní domény ve službě Azure App Service]: ./app-service-web-tutorial-custom-domain.md</span><span class="sxs-lookup"><span data-stu-id="96081-233">[Configure a custom domain name in Azure App Service]: ./app-service-web-tutorial-custom-domain.md</span></span>
<span data-ttu-id="96081-234">[nasazování do pracovní prostředí pro webové aplikace v Azure App Service]: ./web-sites-staged-publishing.md</span><span class="sxs-lookup"><span data-stu-id="96081-234">[Deploy to Staging Environments for Web Apps in Azure App Service]: ./web-sites-staged-publishing.md</span></span>
<span data-ttu-id="96081-235">[Povolit HTTPS pro aplikace v Azure App Service]: ./app-service-web-tutorial-custom-ssl.md</span><span class="sxs-lookup"><span data-stu-id="96081-235">[Enable HTTPS for an app in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md</span></span>
<span data-ttu-id="96081-236">[postupy: sledování stavu koncový bod webové]: http://go.microsoft.com/fwLink/?LinkID=279906</span><span class="sxs-lookup"><span data-stu-id="96081-236">[How to: Monitor web endpoint status]: http://go.microsoft.com/fwLink/?LinkID=279906</span></span>
<span data-ttu-id="96081-237">[Základní informace o monitorování pro webové aplikace v Azure App Service]: ./web-sites-monitor.md</span><span class="sxs-lookup"><span data-stu-id="96081-237">[Monitoring basics for Web Apps in Azure App Service]: ./web-sites-monitor.md</span></span>
<span data-ttu-id="96081-238">[režim kanálů]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application</span><span class="sxs-lookup"><span data-stu-id="96081-238">[pipeline mode]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application</span></span>
<span data-ttu-id="96081-239">[Škálování webové aplikace v Azure App Service]: ./web-sites-scale.md</span><span class="sxs-lookup"><span data-stu-id="96081-239">[Scale a web app in Azure App Service]: ./web-sites-scale.md</span></span>
<span data-ttu-id="96081-240">[socket.io]: ./web-sites-nodejs-chat-app-socketio.md</span><span class="sxs-lookup"><span data-stu-id="96081-240">[socket.io]: ./web-sites-nodejs-chat-app-socketio.md</span></span>
<span data-ttu-id="96081-241">[možnosti vyzkoušet si App Service]: https://azure.microsoft.com/try/app-service/</span><span class="sxs-lookup"><span data-stu-id="96081-241">[Try App Service]: https://azure.microsoft.com/try/app-service/</span></span>

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
