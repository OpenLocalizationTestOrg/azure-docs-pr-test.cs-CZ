---
title: aaaConfigure PHP v Azure App Service Web Apps | Microsoft Docs
description: "Zjistěte, jak tooconfigure hello výchozí instalace PHP a přidat vlastní instalace PHP pro webové aplikace v Azure App Service."
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
ms.openlocfilehash: 2e461e4a269a4ce5614f5f05560f38bc53066251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a><span data-ttu-id="4ab69-103">Nakonfigurovat PHP ve službě Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="4ab69-103">Configure PHP in Azure App Service Web Apps</span></span>
## <a name="introduction"></a><span data-ttu-id="4ab69-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="4ab69-104">Introduction</span></span>
<span data-ttu-id="4ab69-105">Tento průvodce vám ukáže, jak tooconfigure hello předdefinované modulu PHP runtime pro webové aplikace v [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)zadejte vlastní modul runtime PHP a povolit rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4ab69-105">This guide will show you how tooconfigure hello built-in PHP runtime for Web Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), provide a custom PHP runtime, and enable extensions.</span></span> <span data-ttu-id="4ab69-106">toouse App Service, zaregistrujte si hello [bezplatnou zkušební verzi].</span><span class="sxs-lookup"><span data-stu-id="4ab69-106">toouse App Service, sign up for hello [free trial].</span></span> <span data-ttu-id="4ab69-107">tooget hello nejvíce z této příručky, je vhodné nejdříve vytvořit webové aplikace PHP ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="4ab69-107">tooget hello most from this guide, you should first create a PHP web app in App Service.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-hello-built-in-php-version"></a><span data-ttu-id="4ab69-108">Postupy: Změna hello předdefinované verzi PHP</span><span class="sxs-lookup"><span data-stu-id="4ab69-108">How to: Change hello built-in PHP version</span></span>
<span data-ttu-id="4ab69-109">Ve výchozím nastavení PHP 5.5 je nainstalovaná a okamžitě k dispozici pro použití při vytváření webové aplikace služby App Service.</span><span class="sxs-lookup"><span data-stu-id="4ab69-109">By default, PHP 5.5 is installed and immediately available for use when you create an App Service web app.</span></span> <span data-ttu-id="4ab69-110">Hello nejlepší způsob, jak toosee hello k dispozici verze revize jeho výchozí konfigurace a hello povolené rozšíření je toodeploy skript, který volá hello [phpinfo()] funkce.</span><span class="sxs-lookup"><span data-stu-id="4ab69-110">hello best way toosee hello available release revision, its default configuration, and hello enabled extensions is toodeploy a script that calls hello [phpinfo()] function.</span></span>

<span data-ttu-id="4ab69-111">Verze PHP 5.6 a PHP 7.0 jsou také k dispozici, ale není povoleno ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="4ab69-111">PHP 5.6 and PHP 7.0 versions are also available, but not enabled by default.</span></span> <span data-ttu-id="4ab69-112">hello tooupdate verze PHP, postupujte podle jednoho z těchto metod:</span><span class="sxs-lookup"><span data-stu-id="4ab69-112">tooupdate hello PHP version, follow one of these methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="4ab69-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4ab69-113">Azure Portal</span></span>
1. <span data-ttu-id="4ab69-114">Procházet tooyour webové aplikace ve hello [portálu Azure](https://portal.azure.com) a klikněte na hello **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4ab69-114">Browse tooyour web app in hello [Azure Portal](https://portal.azure.com) and click on hello **Settings** button.</span></span>
   
    ![Nastavení webové aplikace][settings-button]
2. <span data-ttu-id="4ab69-116">Z hello **nastavení** okně vyberte **nastavení aplikace** a vyberte novou verzi PHP hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-116">From hello **Settings** blade select **Application Settings** and choose hello new PHP version.</span></span>
   
    ![Nastavení aplikace][application-settings]
3. <span data-ttu-id="4ab69-118">Klikněte na tlačítko hello **Uložit** tlačítko hello horní části hello **webové nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="4ab69-118">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Uložit nastavení konfigurace][save-button]

### <a name="azure-powershell-windows"></a><span data-ttu-id="4ab69-120">Prostředí Azure PowerShell (Windows)</span><span class="sxs-lookup"><span data-stu-id="4ab69-120">Azure PowerShell (Windows)</span></span>
1. <span data-ttu-id="4ab69-121">Otevřete prostředí Azure PowerShell a účet přihlášení tooyour:</span><span class="sxs-lookup"><span data-stu-id="4ab69-121">Open Azure PowerShell, and login tooyour account:</span></span>
   
        PS C:\> Login-AzureRmAccount
2. <span data-ttu-id="4ab69-122">Nastavit verzi PHP hello pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4ab69-122">Set hello PHP version for hello web app.</span></span>
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. <span data-ttu-id="4ab69-123">verze PHP Hello je teď nastavená.</span><span class="sxs-lookup"><span data-stu-id="4ab69-123">hello PHP version is now set.</span></span> <span data-ttu-id="4ab69-124">Můžete potvrdit, tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="4ab69-124">You can confirm these settings:</span></span>
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a><span data-ttu-id="4ab69-125">Rozhraní příkazového řádku Azure (Linux, Mac, Windows)</span><span class="sxs-lookup"><span data-stu-id="4ab69-125">Azure Command-Line Interface (Linux, Mac, Windows)</span></span>
<span data-ttu-id="4ab69-126">toouse hello rozhraní příkazového řádku Azure, musíte mít **Node.js** v počítači nainstalována.</span><span class="sxs-lookup"><span data-stu-id="4ab69-126">toouse hello Azure Command-Line Interface, you must have **Node.js** installed on your computer.</span></span>

1. <span data-ttu-id="4ab69-127">Otevřete terminál a tooyour účet pro přihlášení.</span><span class="sxs-lookup"><span data-stu-id="4ab69-127">Open Terminal, and login tooyour account.</span></span>
   
        azure login
2. <span data-ttu-id="4ab69-128">Nastavit verzi PHP hello pro hello webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4ab69-128">Set hello PHP version for hello web app.</span></span>
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. <span data-ttu-id="4ab69-129">verze PHP Hello je teď nastavená.</span><span class="sxs-lookup"><span data-stu-id="4ab69-129">hello PHP version is now set.</span></span> <span data-ttu-id="4ab69-130">Můžete potvrdit, tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="4ab69-130">You can confirm these settings:</span></span>
   
        azure site show {app-name}

> [!NOTE] 
> <span data-ttu-id="4ab69-131">Hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) jsou příkazy, které jsou ekvivalentní toohello výše:</span><span class="sxs-lookup"><span data-stu-id="4ab69-131">hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands that are equivalent toohello above are:</span></span>
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-hello-built-in-php-configurations"></a><span data-ttu-id="4ab69-132">Postupy: Změna hello předdefinovaných konfigurací PHP</span><span class="sxs-lookup"><span data-stu-id="4ab69-132">How to: Change hello built-in PHP configurations</span></span>
<span data-ttu-id="4ab69-133">Pro žádné předdefinované modul runtime PHP můžete změnit některé možnosti konfigurace hello podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-133">For any built-in PHP runtime, you can change any of hello configuration options by following hello steps below.</span></span> <span data-ttu-id="4ab69-134">(Informace o souboru php.ini direktivy najdete v tématu [seznam php.ini direktivy].)</span><span class="sxs-lookup"><span data-stu-id="4ab69-134">(For information about php.ini directives, see [List of php.ini directives].)</span></span>

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a><span data-ttu-id="4ab69-135">Změna PHP\_INI\_uživatele, PHP\_INI\_PERDIR, PHP\_INI\_všechna nastavení konfigurace</span><span class="sxs-lookup"><span data-stu-id="4ab69-135">Changing PHP\_INI\_USER, PHP\_INI\_PERDIR, PHP\_INI\_ALL configuration settings</span></span>
1. <span data-ttu-id="4ab69-136">Přidat [. user.ini] souboru tooyour kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="4ab69-136">Add a [.user.ini] file tooyour root directory.</span></span>
2. <span data-ttu-id="4ab69-137">Přidat konfiguraci nastavení toohello `.user.ini` souboru pomocí hello stejnou syntaxi, kterou použijete v `php.ini` souboru.</span><span class="sxs-lookup"><span data-stu-id="4ab69-137">Add configuration settings toohello `.user.ini` file using hello same syntax you would use in a `php.ini` file.</span></span> <span data-ttu-id="4ab69-138">Například pokud byste chtěli tooturn hello `display_errors` nastavení a nastavte `upload_max_filesize` nastavení too10M, vaše `.user.ini` soubor bude obsahovat tento text:</span><span class="sxs-lookup"><span data-stu-id="4ab69-138">For example, if you wanted tooturn hello `display_errors` setting on and set `upload_max_filesize` setting too10M, your `.user.ini` file would contain this text:</span></span>
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on toowrite errors tood:\home\LogFiles\php_errors.log
        ; log_errors=On
3. <span data-ttu-id="4ab69-139">Nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-139">Deploy your web app.</span></span>
4. <span data-ttu-id="4ab69-140">Restartujte hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-140">Restart hello web app.</span></span> <span data-ttu-id="4ab69-141">(Restartování je nutné, protože hello četnost PHP, který čte `.user.ini` soubory se řídí hello `user_ini.cache_ttl` nastavení, což je úrovně nastavení systému a ve výchozím nastavení je 300 sekund (5 minut).</span><span class="sxs-lookup"><span data-stu-id="4ab69-141">(Restarting is necessary because hello frequency with which PHP reads `.user.ini` files is governed by hello `user_ini.cache_ttl` setting, which is a system level setting and is 300 seconds (5 minutes) by default.</span></span> <span data-ttu-id="4ab69-142">Restartování webové aplikace hello vynutí nové nastavení PHP tooread hello v hello `.user.ini` souboru.)</span><span class="sxs-lookup"><span data-stu-id="4ab69-142">Restarting hello web app forces PHP tooread hello new settings in hello `.user.ini` file.)</span></span>

<span data-ttu-id="4ab69-143">Jako alternativní toousing `.user.ini` souboru, můžete použít hello [ini_set()] funkce v možnosti konfigurace tooset skripty, které nejsou direktivy úrovni systému.</span><span class="sxs-lookup"><span data-stu-id="4ab69-143">As an alternative toousing a `.user.ini` file, you can use hello [ini_set()] function in scripts tooset configuration options that are not system-level directives.</span></span>

### <a name="changing-phpinisystem-configuration-settings"></a><span data-ttu-id="4ab69-144">Změna PHP\_INI\_nastavení konfigurace systému</span><span class="sxs-lookup"><span data-stu-id="4ab69-144">Changing PHP\_INI\_SYSTEM configuration settings</span></span>
1. <span data-ttu-id="4ab69-145">Přidat nastavení aplikace tooyour webové aplikace s klíčem hello `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="4ab69-145">Add an App Setting tooyour Web App with hello key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
2. <span data-ttu-id="4ab69-146">Vytvoření `settings.ini` souboru pomocí konzole Kudu (http://&lt;název lokality&gt;. scm.azurewebsite.net) v hello `d:\home\site\ini` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4ab69-146">Create an `settings.ini` file using Kudu Console (http://&lt;site-name&gt;.scm.azurewebsite.net) in hello `d:\home\site\ini` directory.</span></span>
3. <span data-ttu-id="4ab69-147">Přidat konfiguraci nastavení toohello `settings.ini` souboru pomocí hello stejnou syntaxi, kterou použijete v souboru php.ini.</span><span class="sxs-lookup"><span data-stu-id="4ab69-147">Add configuration settings toohello `settings.ini` file using hello same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="4ab69-148">Například pokud byste chtěli toopoint hello `curl.cainfo` nastavení tooa `*.crt` souboru a nastavit wincache.maxfilesize nastavení too512K, vaše `settings.ini` soubor bude obsahovat tento text:</span><span class="sxs-lookup"><span data-stu-id="4ab69-148">For example, if you wanted toopoint hello `curl.cainfo` setting tooa `*.crt` file and set 'wincache.maxfilesize' setting too512K, your `settings.ini` file would contain this text:</span></span>
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. <span data-ttu-id="4ab69-149">Restartujte změny hello tooload webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-149">Restart your Web App tooload hello changes.</span></span>

## <a name="how-to-enable-extensions-in-hello-default-php-runtime"></a><span data-ttu-id="4ab69-150">Postupy: povolení rozšíření v hello výchozí PHP runtime</span><span class="sxs-lookup"><span data-stu-id="4ab69-150">How to: Enable extensions in hello default PHP runtime</span></span>
<span data-ttu-id="4ab69-151">Jak jsme uvedli v předchozí části hello hello nejlepší způsob, jak toosee hello výchozí jazyk PHP verze, jeho výchozí konfigurace a hello povolené rozšíření je toodeploy skript, který volá [phpinfo()].</span><span class="sxs-lookup"><span data-stu-id="4ab69-151">As noted in hello previous section, hello best way toosee hello default PHP version, its default configuration, and hello enabled extensions is toodeploy a script that calls [phpinfo()].</span></span> <span data-ttu-id="4ab69-152">tooenable další rozšíření, postupujte podle následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="4ab69-152">tooenable additional extensions, follow hello steps below:</span></span>

### <a name="configure-via-ini-settings"></a><span data-ttu-id="4ab69-153">Konfigurovat pomocí nastavení ini</span><span class="sxs-lookup"><span data-stu-id="4ab69-153">Configure via ini settings</span></span>
1. <span data-ttu-id="4ab69-154">Přidat `ext` directory toohello `d:\home\site` adresáře.</span><span class="sxs-lookup"><span data-stu-id="4ab69-154">Add a `ext` directory toohello `d:\home\site` directory.</span></span>
2. <span data-ttu-id="4ab69-155">Uveďte `.dll` soubory rozšíření v hello `ext` directory (například `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="4ab69-155">Put `.dll` extension files in hello `ext` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="4ab69-156">Ujistěte se, že jsou kompatibilní se výchozí verze PHP a jsou VC9 a bez bezpečného přístupu (nts) kompatibilní hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4ab69-156">Make sure that hello extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="4ab69-157">Přidat nastavení aplikace tooyour webové aplikace s klíčem hello `PHP_INI_SCAN_DIR` a hodnotu`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="4ab69-157">Add an App Setting tooyour Web App with hello key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
4. <span data-ttu-id="4ab69-158">Vytvoření `ini` souboru v `d:\home\site\ini` názvem `extensions.ini`.</span><span class="sxs-lookup"><span data-stu-id="4ab69-158">Create an `ini` file in `d:\home\site\ini` called `extensions.ini`.</span></span>
5. <span data-ttu-id="4ab69-159">Přidat konfiguraci nastavení toohello `extensions.ini` souboru pomocí hello stejnou syntaxi, kterou použijete v souboru php.ini.</span><span class="sxs-lookup"><span data-stu-id="4ab69-159">Add configuration settings toohello `extensions.ini` file using hello same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="4ab69-160">Například pokud byste chtěli MongoDB a nástroje XDebug rozšíření hello tooenable vaše `extensions.ini` soubor bude obsahovat tento text:</span><span class="sxs-lookup"><span data-stu-id="4ab69-160">For example, if you wanted tooenable hello MongoDB and XDebug extensions, your `extensions.ini` file would contain this text:</span></span>
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. <span data-ttu-id="4ab69-161">Restartujte změny hello tooload webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-161">Restart your Web App tooload hello changes.</span></span>

### <a name="configure-via-app-setting"></a><span data-ttu-id="4ab69-162">Konfigurovat pomocí nastavení aplikace</span><span class="sxs-lookup"><span data-stu-id="4ab69-162">Configure via App Setting</span></span>
1. <span data-ttu-id="4ab69-163">Přidat `bin` directory toohello kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="4ab69-163">Add a `bin` directory toohello root directory.</span></span>
2. <span data-ttu-id="4ab69-164">Uveďte `.dll` soubory rozšíření v hello `bin` directory (například `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="4ab69-164">Put `.dll` extension files in hello `bin` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="4ab69-165">Ujistěte se, že jsou kompatibilní se výchozí verze PHP a jsou VC9 a bez bezpečného přístupu (nts) kompatibilní hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4ab69-165">Make sure that hello extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="4ab69-166">Nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-166">Deploy your web app.</span></span>
4. <span data-ttu-id="4ab69-167">Procházet tooyour webové aplikace ve hello portálu Azure a klikněte na hello **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4ab69-167">Browse tooyour web app in hello Azure Portal and click on hello **Settings** button.</span></span>
   
    ![Nastavení webové aplikace][settings-button]
5. <span data-ttu-id="4ab69-169">Z hello **nastavení** okně vyberte **nastavení aplikace** a posuňte se toohello **nastavení aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="4ab69-169">From hello **Settings** blade select **Application Settings** and scroll toohello **App settings** section.</span></span>
6. <span data-ttu-id="4ab69-170">V hello **nastavení aplikace** , vytvořte **PHP_EXTENSIONS** klíč.</span><span class="sxs-lookup"><span data-stu-id="4ab69-170">In hello **App settings** section, create a **PHP_EXTENSIONS** key.</span></span> <span data-ttu-id="4ab69-171">Hello hodnota pro tento klíč bude kořenová cesta relativní toowebsite: **bin\your přípona souboru**.</span><span class="sxs-lookup"><span data-stu-id="4ab69-171">hello value for this key would be a path relative toowebsite root: **bin\your-ext-file**.</span></span>
   
    ![Povolit rozšíření v nastavení aplikace][php-extensions]
7. <span data-ttu-id="4ab69-173">Klikněte na tlačítko hello **Uložit** tlačítko hello horní části hello **webové nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="4ab69-173">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Uložit nastavení konfigurace][save-button]

<span data-ttu-id="4ab69-175">Zend rozšíření jsou podporovány také pomocí **PHP_ZENDEXTENSIONS** klíč.</span><span class="sxs-lookup"><span data-stu-id="4ab69-175">Zend extensions are also supported by using a **PHP_ZENDEXTENSIONS** key.</span></span> <span data-ttu-id="4ab69-176">tooenable více rozšíření, zahrnují obsahuje čárkami oddělený seznam `.dll` soubory pro hodnotu nastavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-176">tooenable multiple extensions, include a comma-separated list of `.dll` files for hello app setting value.</span></span>

## <a name="how-to-use-a-custom-php-runtime"></a><span data-ttu-id="4ab69-177">Postupy: použití vlastního modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="4ab69-177">How to: Use a custom PHP runtime</span></span>
<span data-ttu-id="4ab69-178">App Service Web Apps můžete místo rozšíření modulu PHP runtime výchozí hello použít modulu PHP runtime poskytují tooexecute skripty PHP.</span><span class="sxs-lookup"><span data-stu-id="4ab69-178">Instead of hello default PHP runtime, App Service Web Apps can use a PHP runtime that you provide tooexecute PHP scripts.</span></span> <span data-ttu-id="4ab69-179">dají se nakonfigurovat Hello modul runtime, který zadáte `php.ini` soubor, který je taky zadat.</span><span class="sxs-lookup"><span data-stu-id="4ab69-179">hello runtime that you provide can be configured by a `php.ini` file that you also provide.</span></span> <span data-ttu-id="4ab69-180">toouse vlastní modulu PHP runtime s webovými aplikacemi, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-180">toouse a custom PHP runtime with Web Apps, follow hello steps below.</span></span>

1. <span data-ttu-id="4ab69-181">Získejte není bezpečná pro přístup z více vláken, VC9 nebo VC11 kompatibilní verzi PHP pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="4ab69-181">Obtain a non-thread-safe, VC9 or VC11 compatible version of PHP for Windows.</span></span> <span data-ttu-id="4ab69-182">Poslední verze PHP pro systém Windows naleznete zde: [http://windows.php.net/download/].</span><span class="sxs-lookup"><span data-stu-id="4ab69-182">Recent releases of PHP for Windows can be found here: [http://windows.php.net/download/].</span></span> <span data-ttu-id="4ab69-183">Starší verze najdete v archivu hello zde: [http://windows.php.net/downloads/releases/archives/].</span><span class="sxs-lookup"><span data-stu-id="4ab69-183">Older releases can be found in hello archive here: [http://windows.php.net/downloads/releases/archives/].</span></span>
2. <span data-ttu-id="4ab69-184">Upravit hello `php.ini` soubor pro vaše runtime.</span><span class="sxs-lookup"><span data-stu-id="4ab69-184">Modify hello `php.ini` file for your runtime.</span></span> <span data-ttu-id="4ab69-185">Všimněte si, že všechna nastavení, které jsou systému úroveň jen direktivy budou ignorovány webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-185">Note that any configuration settings that are system-level-only directives will be ignored by Web Apps.</span></span> <span data-ttu-id="4ab69-186">(Informace o systému úroveň jen direktivy najdete v tématu [seznam php.ini direktivy]).</span><span class="sxs-lookup"><span data-stu-id="4ab69-186">(For information about system-level-only directives, see [List of php.ini directives]).</span></span>
3. <span data-ttu-id="4ab69-187">Volitelně můžete přidat rozšíření tooyour PHP runtime a je povolit v hello `php.ini` souboru.</span><span class="sxs-lookup"><span data-stu-id="4ab69-187">Optionally, add extensions tooyour PHP runtime and enable them in hello `php.ini` file.</span></span>
4. <span data-ttu-id="4ab69-188">Přidat `bin` directory tooyour kořenový adresář a put hello adresář, který obsahuje vaše modulu PHP runtime v ní (například `bin\php`).</span><span class="sxs-lookup"><span data-stu-id="4ab69-188">Add a `bin` directory tooyour root directory, and put hello directory that contains your PHP runtime in it (for example, `bin\php`).</span></span>
5. <span data-ttu-id="4ab69-189">Nasazení webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="4ab69-189">Deploy your web app.</span></span>
6. <span data-ttu-id="4ab69-190">Procházet tooyour webové aplikace ve hello portálu Azure a klikněte na hello **nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4ab69-190">Browse tooyour web app in hello Azure Portal and click on hello **Settings** button.</span></span>
   
    ![Nastavení webové aplikace][settings-button]
7. <span data-ttu-id="4ab69-192">Z hello **nastavení** okně vyberte **nastavení aplikace** a posuňte se toohello **mapování obslužných rutin** části.</span><span class="sxs-lookup"><span data-stu-id="4ab69-192">From hello **Settings** blade select **Application Settings** and scroll toohello **Handler mappings** section.</span></span> <span data-ttu-id="4ab69-193">Přidat `*.php` toohello rozšíření pole a přidejte hello cesta toohello `php-cgi.exe` spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="4ab69-193">Add `*.php` toohello Extension field and add hello path toohello `php-cgi.exe` executable.</span></span> <span data-ttu-id="4ab69-194">Když vložíte vaší modulu PHP runtime v hello `bin` directory hello kořenové aplikace, bude cesta hello `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span><span class="sxs-lookup"><span data-stu-id="4ab69-194">If you put your PHP runtime in hello `bin` directory in hello root of you application, hello path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span></span>
   
    ![Zadejte rutinu mapování obslužné rutiny][handler-mappings]
8. <span data-ttu-id="4ab69-196">Klikněte na tlačítko hello **Uložit** tlačítko hello horní části hello **webové nastavení aplikace** okno.</span><span class="sxs-lookup"><span data-stu-id="4ab69-196">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Uložit nastavení konfigurace][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a><span data-ttu-id="4ab69-198">Postupy: povolení autora automation v Azure</span><span class="sxs-lookup"><span data-stu-id="4ab69-198">How to: Enable Composer automation in Azure</span></span>
<span data-ttu-id="4ab69-199">Ve výchozím nastavení služby App Service nic se neděje s composer.json, pokud máte ve vašem projektu PHP.</span><span class="sxs-lookup"><span data-stu-id="4ab69-199">By default, App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="4ab69-200">Pokud používáte [nasazení Git](app-service-deploy-local-git.md), můžete povolit composer.json zpracování během `git push` povolením rozšíření autora hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-200">If you use [Git deployment](app-service-deploy-local-git.md), you can enable composer.json processing during `git push` by enabling hello Composer extension.</span></span>

> [!NOTE]
> <span data-ttu-id="4ab69-201">Můžete [hlas podpora prvotřídní autora ve službě App Service zde](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span><span class="sxs-lookup"><span data-stu-id="4ab69-201">You can [vote for first-class Composer support in App Service here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span></span>
> 
> 

1. <span data-ttu-id="4ab69-202">Ve vašem PHP webové okně aplikace v hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **nástroje** > **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="4ab69-202">In your PHP web app's blade in hello [Azure portal](https://portal.azure.com), click **Tools** > **Extensions**.</span></span>
   
    ![Azure Portal nastavení okno tooenable autora automation v Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. <span data-ttu-id="4ab69-204">Klikněte na tlačítko **přidat**, pak klikněte na tlačítko **autora**.</span><span class="sxs-lookup"><span data-stu-id="4ab69-204">Click **Add**, then click **Composer**.</span></span>
   
    ![Přidat autora rozšíření tooenable autora automation v Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. <span data-ttu-id="4ab69-206">Klikněte na tlačítko **OK** tooaccept právní podmínky.</span><span class="sxs-lookup"><span data-stu-id="4ab69-206">Click **OK** tooaccept legal terms.</span></span> <span data-ttu-id="4ab69-207">Klikněte na tlačítko **OK** znovu tooadd hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4ab69-207">Click **OK** again tooadd hello extension.</span></span>
   
    <span data-ttu-id="4ab69-208">Hello **nainstalovaná rozšíření** okno se nyní zobrazí rozšíření autora hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-208">hello **Installed extensions** blade will now show hello Composer extension.</span></span>  
    <span data-ttu-id="4ab69-209">![Přijměte právní podmínky tooenable autora automation v Azure](./media/web-sites-php-configure/composer-extension-view.png)</span><span class="sxs-lookup"><span data-stu-id="4ab69-209">![Accept legal terms tooenable Composer automation in Azure](./media/web-sites-php-configure/composer-extension-view.png)</span></span>
4. <span data-ttu-id="4ab69-210">Teď, provádět `git add`, `git commit`, a `git push` jako v předchozím oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="4ab69-210">Now, perform `git add`, `git commit`, and `git push` like in hello previous section.</span></span> <span data-ttu-id="4ab69-211">Nyní se zobrazí, autora je instalování závislostí, které jsou definované v composer.json.</span><span class="sxs-lookup"><span data-stu-id="4ab69-211">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Nasazení Git s autora automation v Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a><span data-ttu-id="4ab69-213">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4ab69-213">Next steps</span></span>
<span data-ttu-id="4ab69-214">Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="4ab69-214">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

> [!NOTE]
> <span data-ttu-id="4ab69-215">Pokud chcete, aby tooget začít s Azure App Service před registrací účtu Azure, přejděte příliš[vyzkoušet službu App Service](https://azure.microsoft.com/try/app-service/), kde můžete okamžitě vytvořit krátkodobou úvodní webovou aplikaci ve službě App Service.</span><span class="sxs-lookup"><span data-stu-id="4ab69-215">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4ab69-216">Nevyžaduje se žádná platební karta a nevzniká žádný závazek.</span><span class="sxs-lookup"><span data-stu-id="4ab69-216">No credit cards required; no commitments.</span></span>
> 
> 

[bezplatnou zkušební verzi]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[seznam php.ini direktivy]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

