---
title: "Nakonfigurovat PHP ve službě Azure App Service Web Apps | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat výchozí instalace PHP nebo přidat vlastní instalace PHP pro webové aplikace v Azure App Service."
services: app-service
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 624dd416f37aacdb3d2f6e59afdc2efe646e610b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a><span data-ttu-id="623e6-103">Nakonfigurovat PHP ve službě Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="623e6-103">Configure PHP in Azure App Service Web Apps</span></span>
## <a name="introduction"></a><span data-ttu-id="623e6-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="623e6-104">Introduction</span></span>
<span data-ttu-id="623e6-105">Tento průvodce vám ukáže, jak konfigurovat předdefinované modulu PHP runtime pro webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)zadejte vlastní modul runtime PHP a povolit rozšíření.</span><span class="sxs-lookup"><span data-stu-id="623e6-105">This guide will show you how to configure the built-in PHP runtime for Web Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), provide a custom PHP runtime, and enable extensions.</span></span> <span data-ttu-id="623e6-106">Pokud chcete používat služby App Service, zaregistrujte si [bezplatnou zkušební verzi].</span><span class="sxs-lookup"><span data-stu-id="623e6-106">To use App Service, sign up for the [free trial].</span></span> <span data-ttu-id="623e6-107">Pokud chcete nejvíce z této příručky, měli nejprve vytvořit webové aplikace PHP ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="623e6-107">To get the most from this guide, you should first create a PHP web app in App Service.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a><span data-ttu-id="623e6-108">Postupy: Změna předdefinované verzi PHP</span><span class="sxs-lookup"><span data-stu-id="623e6-108">How to: Change the built-in PHP version</span></span>
<span data-ttu-id="623e6-109">Ve výchozím nastavení PHP 5.5 je nainstalovaná a okamžitě k dispozici pro použití při vytváření webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="623e6-109">By default, PHP 5.5 is installed and immediately available for use when you create an App Service web app.</span></span> <span data-ttu-id="623e6-110">Nejlepší způsob, jak zobrazit dostupné verze revize, jeho výchozí konfigurace a povolené rozšíření je pro nasazení skript, který volá [phpinfo()] funkce.</span><span class="sxs-lookup"><span data-stu-id="623e6-110">The best way to see the available release revision, its default configuration, and the enabled extensions is to deploy a script that calls the [phpinfo()] function.</span></span>

<span data-ttu-id="623e6-111">Verze PHP 5.6 a PHP 7.0 jsou také k dispozici, ale není povoleno ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="623e6-111">PHP 5.6 and PHP 7.0 versions are also available, but not enabled by default.</span></span> <span data-ttu-id="623e6-112">Pokud chcete aktualizovat verzi PHP, postupujte podle jednu z těchto metod:</span><span class="sxs-lookup"><span data-stu-id="623e6-112">To update the PHP version, follow one of these methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="623e6-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="623e6-113">Azure Portal</span></span>
1. <span data-ttu-id="623e6-114">Přejděte do webové aplikace v [portálu Azure](https://portal.azure.com) a klikněte na **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="623e6-114">Browse to your web app in the [Azure Portal](https://portal.azure.com) and click on the **Settings** button.</span></span>
   
    ![Nastavení webové aplikace][settings-button]
2. <span data-ttu-id="623e6-116">Z **nastavení** okně vyberte **nastavení aplikace** a vyberte novou verzi PHP.</span><span class="sxs-lookup"><span data-stu-id="623e6-116">From the **Settings** blade select **Application Settings** and choose the new PHP version.</span></span>
   
    ![Nastavení aplikace][application-settings]
3. <span data-ttu-id="623e6-118">Klikněte **Uložit** tlačítka v horní části **webové nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="623e6-118">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Uložit nastavení konfigurace][save-button]

### <a name="azure-powershell-windows"></a><span data-ttu-id="623e6-120">Prostředí Azure PowerShell (Windows)</span><span class="sxs-lookup"><span data-stu-id="623e6-120">Azure PowerShell (Windows)</span></span>
1. <span data-ttu-id="623e6-121">Otevřete prostředí Azure PowerShell a přihlásit k účtu:</span><span class="sxs-lookup"><span data-stu-id="623e6-121">Open Azure PowerShell, and login to your account:</span></span>
   
        PS C:\> Login-AzureRmAccount
2. <span data-ttu-id="623e6-122">Nastavte verzi PHP pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="623e6-122">Set the PHP version for the web app.</span></span>
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. <span data-ttu-id="623e6-123">Verze PHP je teď nastavená.</span><span class="sxs-lookup"><span data-stu-id="623e6-123">The PHP version is now set.</span></span> <span data-ttu-id="623e6-124">Můžete potvrdit, tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="623e6-124">You can confirm these settings:</span></span>
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a><span data-ttu-id="623e6-125">Rozhraní příkazového řádku Azure (Linux, Mac, Windows)</span><span class="sxs-lookup"><span data-stu-id="623e6-125">Azure Command-Line Interface (Linux, Mac, Windows)</span></span>
<span data-ttu-id="623e6-126">Chcete-li používat rozhraní příkazového řádku Azure, musíte mít **Node.js** v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="623e6-126">To use the Azure Command-Line Interface, you must have **Node.js** installed on your computer.</span></span>

1. <span data-ttu-id="623e6-127">Otevřete terminál a přihlášení k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="623e6-127">Open Terminal, and login to your account.</span></span>
   
        azure login
2. <span data-ttu-id="623e6-128">Nastavte verzi PHP pro webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="623e6-128">Set the PHP version for the web app.</span></span>
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. <span data-ttu-id="623e6-129">Verze PHP je teď nastavená.</span><span class="sxs-lookup"><span data-stu-id="623e6-129">The PHP version is now set.</span></span> <span data-ttu-id="623e6-130">Můžete potvrdit, tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="623e6-130">You can confirm these settings:</span></span>
   
        azure site show {app-name}

> [!NOTE] 
> <span data-ttu-id="623e6-131">[Azure CLI 2.0](https://github.com/Azure/azure-cli) jsou příkazy, které jsou ekvivalentem výše:</span><span class="sxs-lookup"><span data-stu-id="623e6-131">The [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands that are equivalent to the above are:</span></span>
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-the-built-in-php-configurations"></a><span data-ttu-id="623e6-132">Postupy: Změna předdefinovaných konfigurací PHP</span><span class="sxs-lookup"><span data-stu-id="623e6-132">How to: Change the built-in PHP configurations</span></span>
<span data-ttu-id="623e6-133">Pro žádné předdefinované modul runtime PHP můžete změnit některé možnosti konfigurace podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="623e6-133">For any built-in PHP runtime, you can change any of the configuration options by following the steps below.</span></span> <span data-ttu-id="623e6-134">(Informace o souboru php.ini direktivy najdete v tématu [seznam php.ini direktivy].)</span><span class="sxs-lookup"><span data-stu-id="623e6-134">(For information about php.ini directives, see [List of php.ini directives].)</span></span>

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a><span data-ttu-id="623e6-135">Změna PHP\_INI\_uživatele, PHP\_INI\_PERDIR, PHP\_INI\_všechna nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="623e6-135">Changing PHP\_INI\_USER, PHP\_INI\_PERDIR, PHP\_INI\_ALL configuration settings</span></span>
1. <span data-ttu-id="623e6-136">Přidat [. user.ini] soubor pro kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="623e6-136">Add a [.user.ini] file to your root directory.</span></span>
2. <span data-ttu-id="623e6-137">Přidat nastavení konfigurace `.user.ini` pomocí stejnou syntaxi, kterou použijete v souboru `php.ini` souboru.</span><span class="sxs-lookup"><span data-stu-id="623e6-137">Add configuration settings to the `.user.ini` file using the same syntax you would use in a `php.ini` file.</span></span> <span data-ttu-id="623e6-138">Například, pokud chcete zapnout `display_errors` nastavení a nastavte `upload_max_filesize` nastavení na 10 MB, vaše `.user.ini` soubor bude obsahovat tento text:</span><span class="sxs-lookup"><span data-stu-id="623e6-138">For example, if you wanted to turn the `display_errors` setting on and set `upload_max_filesize` setting to 10M, your `.user.ini` file would contain this text:</span></span>
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
3. <span data-ttu-id="623e6-139">Nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="623e6-139">Deploy your web app.</span></span>
4. <span data-ttu-id="623e6-140">Restartování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="623e6-140">Restart the web app.</span></span> <span data-ttu-id="623e6-141">(Restartování je nutné, protože frekvenci, se kterou PHP čte `.user.ini` se řídí soubory `user_ini.cache_ttl` nastavení, což je úrovně nastavení systému a ve výchozím nastavení je 300 sekund (5 minut).</span><span class="sxs-lookup"><span data-stu-id="623e6-141">(Restarting is necessary because the frequency with which PHP reads `.user.ini` files is governed by the `user_ini.cache_ttl` setting, which is a system level setting and is 300 seconds (5 minutes) by default.</span></span> <span data-ttu-id="623e6-142">Restartování webové aplikace PHP číst nové nastavení vynutí `.user.ini` souboru.)</span><span class="sxs-lookup"><span data-stu-id="623e6-142">Restarting the web app forces PHP to read the new settings in the `.user.ini` file.)</span></span>

<span data-ttu-id="623e6-143">Jako alternativu k použití `.user.ini` souboru, můžete použít [ini_set()] funkce ve skriptech, chcete-li nastavit možnosti konfigurace, které nejsou direktivy úrovni systému.</span><span class="sxs-lookup"><span data-stu-id="623e6-143">As an alternative to using a `.user.ini` file, you can use the [ini_set()] function in scripts to set configuration options that are not system-level directives.</span></span>

### <a name="changing-phpinisystem-configuration-settings"></a><span data-ttu-id="623e6-144">Změna PHP\_INI\_nastavení konfigurace systému</span><span class="sxs-lookup"><span data-stu-id="623e6-144">Changing PHP\_INI\_SYSTEM configuration settings</span></span>
1. <span data-ttu-id="623e6-145">Přidat nastavení aplikace do webové aplikace s klíčem `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="623e6-145">Add an App Setting to your Web App with the key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
2. <span data-ttu-id="623e6-146">Vytvoření `settings.ini` souboru pomocí konzole Kudu (http://&lt;název lokality&gt;. scm.azurewebsite.net) v `d:\home\site\ini` directory.</span><span class="sxs-lookup"><span data-stu-id="623e6-146">Create an `settings.ini` file using Kudu Console (http://&lt;site-name&gt;.scm.azurewebsite.net) in the `d:\home\site\ini` directory.</span></span>
3. <span data-ttu-id="623e6-147">Přidat nastavení konfigurace `settings.ini` soubor pomocí stejné syntaxe byste použili v souboru php.ini.</span><span class="sxs-lookup"><span data-stu-id="623e6-147">Add configuration settings to the `settings.ini` file using the same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="623e6-148">Například, pokud jste chtěli bodu `curl.cainfo` nastavení `*.crt` souborů a nastavení 'wincache.maxfilesize' na 512 kB, vaše `settings.ini` soubor bude obsahovat tento text:</span><span class="sxs-lookup"><span data-stu-id="623e6-148">For example, if you wanted to point the `curl.cainfo` setting to a `*.crt` file and set 'wincache.maxfilesize' setting to 512K, your `settings.ini` file would contain this text:</span></span>
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. <span data-ttu-id="623e6-149">Restartování webové aplikace načíst změny.</span><span class="sxs-lookup"><span data-stu-id="623e6-149">Restart your Web App to load the changes.</span></span>

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a><span data-ttu-id="623e6-150">Postupy: povolení rozšíření v modulu runtime PHP výchozí</span><span class="sxs-lookup"><span data-stu-id="623e6-150">How to: Enable extensions in the default PHP runtime</span></span>
<span data-ttu-id="623e6-151">Jak je uvedeno v předchozí části, je nejlepší způsob, jak zobrazit výchozí verze PHP, jeho výchozí konfigurace a povolené rozšíření nasazení skriptu, který volá [phpinfo()].</span><span class="sxs-lookup"><span data-stu-id="623e6-151">As noted in the previous section, the best way to see the default PHP version, its default configuration, and the enabled extensions is to deploy a script that calls [phpinfo()].</span></span> <span data-ttu-id="623e6-152">Povolit další rozšíření, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="623e6-152">To enable additional extensions, follow the steps below:</span></span>

### <a name="configure-via-ini-settings"></a><span data-ttu-id="623e6-153">Konfigurovat pomocí nastavení ini</span><span class="sxs-lookup"><span data-stu-id="623e6-153">Configure via ini settings</span></span>
1. <span data-ttu-id="623e6-154">Přidat `ext` do adresáře `d:\home\site` adresáře.</span><span class="sxs-lookup"><span data-stu-id="623e6-154">Add a `ext` directory to the `d:\home\site` directory.</span></span>
2. <span data-ttu-id="623e6-155">Uveďte `.dll` soubory s příponou `ext` directory (například `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="623e6-155">Put `.dll` extension files in the `ext` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="623e6-156">Ujistěte se, že rozšíření jsou kompatibilní se výchozí verze PHP a jsou VC9 a kompatibilní bez bezpečného přístupu (nts).</span><span class="sxs-lookup"><span data-stu-id="623e6-156">Make sure that the extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="623e6-157">Přidat nastavení aplikace do webové aplikace s klíčem `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="623e6-157">Add an App Setting to your Web App with the key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
4. <span data-ttu-id="623e6-158">Vytvoření `ini` souboru v `d:\home\site\ini` názvem `extensions.ini`.</span><span class="sxs-lookup"><span data-stu-id="623e6-158">Create an `ini` file in `d:\home\site\ini` called `extensions.ini`.</span></span>
5. <span data-ttu-id="623e6-159">Přidat nastavení konfigurace `extensions.ini` soubor pomocí stejné syntaxe byste použili v souboru php.ini.</span><span class="sxs-lookup"><span data-stu-id="623e6-159">Add configuration settings to the `extensions.ini` file using the same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="623e6-160">Například, pokud chcete povolit rozšíření MongoDB a nástroje XDebug vaše `extensions.ini` soubor bude obsahovat tento text:</span><span class="sxs-lookup"><span data-stu-id="623e6-160">For example, if you wanted to enable the MongoDB and XDebug extensions, your `extensions.ini` file would contain this text:</span></span>
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. <span data-ttu-id="623e6-161">Restartování webové aplikace načíst změny.</span><span class="sxs-lookup"><span data-stu-id="623e6-161">Restart your Web App to load the changes.</span></span>

### <a name="configure-via-app-setting"></a><span data-ttu-id="623e6-162">Konfigurovat pomocí nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="623e6-162">Configure via App Setting</span></span>
1. <span data-ttu-id="623e6-163">Přidat `bin` adresář na kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="623e6-163">Add a `bin` directory to the root directory.</span></span>
2. <span data-ttu-id="623e6-164">Uveďte `.dll` soubory s příponou `bin` directory (například `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="623e6-164">Put `.dll` extension files in the `bin` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="623e6-165">Ujistěte se, že rozšíření jsou kompatibilní se výchozí verze PHP a jsou VC9 a kompatibilní bez bezpečného přístupu (nts).</span><span class="sxs-lookup"><span data-stu-id="623e6-165">Make sure that the extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="623e6-166">Nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="623e6-166">Deploy your web app.</span></span>
4. <span data-ttu-id="623e6-167">Přejděte na webovou aplikaci na portálu Azure a klikněte na **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="623e6-167">Browse to your web app in the Azure Portal and click on the **Settings** button.</span></span>
   
    ![Nastavení webové aplikace][settings-button]
5. <span data-ttu-id="623e6-169">Z **nastavení** okně vyberte **nastavení aplikace** a přejděte k položce **nastavení aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="623e6-169">From the **Settings** blade select **Application Settings** and scroll to the **App settings** section.</span></span>
6. <span data-ttu-id="623e6-170">V **nastavení aplikace** , vytvořte **PHP_EXTENSIONS** klíč.</span><span class="sxs-lookup"><span data-stu-id="623e6-170">In the **App settings** section, create a **PHP_EXTENSIONS** key.</span></span> <span data-ttu-id="623e6-171">Hodnota tohoto klíče by být cesta relativní vůči kořenovému adresáři webu: **bin\your přípona souboru**.</span><span class="sxs-lookup"><span data-stu-id="623e6-171">The value for this key would be a path relative to website root: **bin\your-ext-file**.</span></span>
   
    ![Povolit rozšíření v nastavení aplikace][php-extensions]
7. <span data-ttu-id="623e6-173">Klikněte **Uložit** tlačítka v horní části **webové nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="623e6-173">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Uložit nastavení konfigurace][save-button]

<span data-ttu-id="623e6-175">Zend rozšíření jsou podporovány také pomocí **PHP_ZENDEXTENSIONS** klíč.</span><span class="sxs-lookup"><span data-stu-id="623e6-175">Zend extensions are also supported by using a **PHP_ZENDEXTENSIONS** key.</span></span> <span data-ttu-id="623e6-176">Chcete-li několik rozšíření, zahrnují obsahuje čárkami oddělený seznam `.dll` soubory pro hodnotu nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="623e6-176">To enable multiple extensions, include a comma-separated list of `.dll` files for the app setting value.</span></span>

## <a name="how-to-use-a-custom-php-runtime"></a><span data-ttu-id="623e6-177">Postupy: použití vlastního modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="623e6-177">How to: Use a custom PHP runtime</span></span>
<span data-ttu-id="623e6-178">App Service Web Apps můžete místo výchozí modul runtime PHP, použít modul runtime PHP, který zadáte ke spuštění skripty PHP.</span><span class="sxs-lookup"><span data-stu-id="623e6-178">Instead of the default PHP runtime, App Service Web Apps can use a PHP runtime that you provide to execute PHP scripts.</span></span> <span data-ttu-id="623e6-179">Dají se nakonfigurovat modul runtime, který zadáte `php.ini` soubor, který je taky zadat.</span><span class="sxs-lookup"><span data-stu-id="623e6-179">The runtime that you provide can be configured by a `php.ini` file that you also provide.</span></span> <span data-ttu-id="623e6-180">Vlastní modul runtime PHP s webovými aplikacemi, postupujte podle následujících kroků.</span><span class="sxs-lookup"><span data-stu-id="623e6-180">To use a custom PHP runtime with Web Apps, follow the steps below.</span></span>

1. <span data-ttu-id="623e6-181">Získejte není bezpečná pro přístup z více vláken, VC9 nebo VC11 kompatibilní verzi PHP pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="623e6-181">Obtain a non-thread-safe, VC9 or VC11 compatible version of PHP for Windows.</span></span> <span data-ttu-id="623e6-182">Poslední verze PHP pro systém Windows naleznete zde: [http://windows.php.net/download/].</span><span class="sxs-lookup"><span data-stu-id="623e6-182">Recent releases of PHP for Windows can be found here: [http://windows.php.net/download/].</span></span> <span data-ttu-id="623e6-183">Starší verze najdete v archivu zde: [http://windows.php.net/downloads/releases/archives/].</span><span class="sxs-lookup"><span data-stu-id="623e6-183">Older releases can be found in the archive here: [http://windows.php.net/downloads/releases/archives/].</span></span>
2. <span data-ttu-id="623e6-184">Změnit `php.ini` soubor pro vaše runtime.</span><span class="sxs-lookup"><span data-stu-id="623e6-184">Modify the `php.ini` file for your runtime.</span></span> <span data-ttu-id="623e6-185">Všimněte si, že všechna nastavení, které jsou systému úroveň jen direktivy budou ignorovány webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="623e6-185">Note that any configuration settings that are system-level-only directives will be ignored by Web Apps.</span></span> <span data-ttu-id="623e6-186">(Informace o systému úroveň jen direktivy najdete v tématu [seznam php.ini direktivy]).</span><span class="sxs-lookup"><span data-stu-id="623e6-186">(For information about system-level-only directives, see [List of php.ini directives]).</span></span>
3. <span data-ttu-id="623e6-187">Volitelně můžete přidat rozšíření do vaší runtime PHP a povolte je do `php.ini` souboru.</span><span class="sxs-lookup"><span data-stu-id="623e6-187">Optionally, add extensions to your PHP runtime and enable them in the `php.ini` file.</span></span>
4. <span data-ttu-id="623e6-188">Přidat `bin` do kořenového adresáře a put adresář, který obsahuje vaše modulu PHP runtime v ní adresáře (například `bin\php`).</span><span class="sxs-lookup"><span data-stu-id="623e6-188">Add a `bin` directory to your root directory, and put the directory that contains your PHP runtime in it (for example, `bin\php`).</span></span>
5. <span data-ttu-id="623e6-189">Nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="623e6-189">Deploy your web app.</span></span>
6. <span data-ttu-id="623e6-190">Přejděte na webovou aplikaci na portálu Azure a klikněte na **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="623e6-190">Browse to your web app in the Azure Portal and click on the **Settings** button.</span></span>
   
    ![Nastavení webové aplikace][settings-button]
7. <span data-ttu-id="623e6-192">Z **nastavení** okně vyberte **nastavení aplikace** a přejděte k položce **mapování obslužných rutin** části.</span><span class="sxs-lookup"><span data-stu-id="623e6-192">From the **Settings** blade select **Application Settings** and scroll to the **Handler mappings** section.</span></span> <span data-ttu-id="623e6-193">Přidat `*.php` rozšíření pole a přidejte cestu k `php-cgi.exe` spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="623e6-193">Add `*.php` to the Extension field and add the path to the `php-cgi.exe` executable.</span></span> <span data-ttu-id="623e6-194">Když vložíte vaší modulu PHP runtime `bin` adresáře v kořenovém adresáři aplikace, bude cesta `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span><span class="sxs-lookup"><span data-stu-id="623e6-194">If you put your PHP runtime in the `bin` directory in the root of you application, the path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span></span>
   
    ![Zadejte rutinu mapování obslužné rutiny][handler-mappings]
8. <span data-ttu-id="623e6-196">Klikněte **Uložit** tlačítka v horní části **webové nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="623e6-196">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Uložit nastavení konfigurace][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a><span data-ttu-id="623e6-198">Postupy: povolení autora automation v Azure</span><span class="sxs-lookup"><span data-stu-id="623e6-198">How to: Enable Composer automation in Azure</span></span>
<span data-ttu-id="623e6-199">Ve výchozím nastavení služby App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP.</span><span class="sxs-lookup"><span data-stu-id="623e6-199">By default, App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="623e6-200">Pokud používáte [nasazení Git](app-service-deploy-local-git.md), můžete povolit composer.json zpracování během `git push` povolením rozšíření autora.</span><span class="sxs-lookup"><span data-stu-id="623e6-200">If you use [Git deployment](app-service-deploy-local-git.md), you can enable composer.json processing during `git push` by enabling the Composer extension.</span></span>

> [!NOTE]
> <span data-ttu-id="623e6-201">Můžete [hlas podpora prvotřídní autora ve službě App Service zde](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span><span class="sxs-lookup"><span data-stu-id="623e6-201">You can [vote for first-class Composer support in App Service here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span></span>
> 
> 

1. <span data-ttu-id="623e6-202">Ve vašem PHP webové okně aplikace v [portál Azure](https://portal.azure.com), klikněte na tlačítko **nástroje** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="623e6-202">In your PHP web app's blade in the [Azure portal](https://portal.azure.com), click **Tools** > **Extensions**.</span></span>
   
    ![Okno nastavení Azure Portal povolit autora automation v Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. <span data-ttu-id="623e6-204">Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.</span><span class="sxs-lookup"><span data-stu-id="623e6-204">Click **Add**, then click **Composer**.</span></span>
   
    ![Přidání rozšíření autora jak povolit automatizaci autora v Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. <span data-ttu-id="623e6-206">Klikněte na tlačítko **OK** přijměte právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="623e6-206">Click **OK** to accept legal terms.</span></span> <span data-ttu-id="623e6-207">Klikněte na tlačítko **OK** přidat rozšíření.</span><span class="sxs-lookup"><span data-stu-id="623e6-207">Click **OK** again to add the extension.</span></span>
   
    <span data-ttu-id="623e6-208">**Nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora.</span><span class="sxs-lookup"><span data-stu-id="623e6-208">The **Installed extensions** blade will now show the Composer extension.</span></span>  
    <span data-ttu-id="623e6-209">![Přijměte právní podmínky, jak povolit automatizaci autora v Azure](./media/web-sites-php-configure/composer-extension-view.png)</span><span class="sxs-lookup"><span data-stu-id="623e6-209">![Accept legal terms to enable Composer automation in Azure](./media/web-sites-php-configure/composer-extension-view.png)</span></span>
4. <span data-ttu-id="623e6-210">Teď, provádět `git add`, `git commit`, a `git push` jako v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="623e6-210">Now, perform `git add`, `git commit`, and `git push` like in the previous section.</span></span> <span data-ttu-id="623e6-211">Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.</span><span class="sxs-lookup"><span data-stu-id="623e6-211">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Nasazení Git s autora automation v Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a><span data-ttu-id="623e6-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="623e6-213">Next steps</span></span>
<span data-ttu-id="623e6-214">Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="623e6-214">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

> [!NOTE]
> <span data-ttu-id="623e6-215">Pokud chcete začít používat Azure App Service před registrací účtu Azure, přejděte k [možnosti vyzkoušet si App Service](https://azure.microsoft.com/try/app-service/), kde si můžete hned vytvořit krátkodobou úvodní webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="623e6-215">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="623e6-216">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="623e6-216">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="623e6-217">[bezplatnou zkušební verzi]: https://www.windowsazure.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="623e6-217">[free trial]: https://www.windowsazure.com/pricing/free-trial/</span></span>
<span data-ttu-id="623e6-218">[phpinfo()]: http://php.net/manual/en/function.phpinfo.php</span><span class="sxs-lookup"><span data-stu-id="623e6-218">[phpinfo()]: http://php.net/manual/en/function.phpinfo.php</span></span>
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
<span data-ttu-id="623e6-219">[seznam php.ini direktivy]: http://www.php.net/manual/en/ini.list.php</span><span class="sxs-lookup"><span data-stu-id="623e6-219">[List of php.ini directives]: http://www.php.net/manual/en/ini.list.php</span></span>
<span data-ttu-id="623e6-220">[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php</span><span class="sxs-lookup"><span data-stu-id="623e6-220">[.user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php</span></span>
<span data-ttu-id="623e6-221">[ini_set()]: http://www.php.net/manual/en/function.ini-set.php</span><span class="sxs-lookup"><span data-stu-id="623e6-221">[ini_set()]: http://www.php.net/manual/en/function.ini-set.php</span></span>
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
<span data-ttu-id="623e6-222">[http://windows.php.net/download/]: http://windows.php.net/download/</span><span class="sxs-lookup"><span data-stu-id="623e6-222">[http://windows.php.net/download/]: http://windows.php.net/download/</span></span>
<span data-ttu-id="623e6-223">[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/</span><span class="sxs-lookup"><span data-stu-id="623e6-223">[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/</span></span>
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

