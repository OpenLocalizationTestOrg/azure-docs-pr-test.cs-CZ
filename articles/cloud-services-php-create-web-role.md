---
title: "aaaCreate Azure webových a pracovních rolí pro jazyk PHP | Microsoft Docs"
description: "Průvodce toocreating PHP webových a pracovních rolí do cloudové služby Azure a konfigurace hello modulu PHP runtime."
services: 
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 04a6e8c9c379cb0f854645941b6bc7d614bb91f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-php-web-and-worker-roles"></a><span data-ttu-id="69244-103">Jak toocreate PHP webových a pracovních rolí</span><span class="sxs-lookup"><span data-stu-id="69244-103">How toocreate PHP web and worker roles</span></span>
## <a name="overview"></a><span data-ttu-id="69244-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="69244-104">Overview</span></span>
<span data-ttu-id="69244-105">Tento průvodce vám ukáže, jak vybrat konkrétní verzi PHP z hello "integrovaných" verzí, které jsou k dispozici role web nebo worker PHP toocreate v vývojového prostředí systému Windows, změnit konfiguraci hello PHP, povolit rozšíření a nakonec nasazení tooAzure.</span><span class="sxs-lookup"><span data-stu-id="69244-105">This guide will show you how toocreate PHP web or worker roles in a Windows development environment, choose a specific version of PHP from hello "built-in" versions available, change hello PHP configuration, enable extensions, and finally, deploy tooAzure.</span></span> <span data-ttu-id="69244-106">Také popisuje, jak tooconfigure web nebo worker role toouse modul runtime PHP (s vlastní konfigurace a rozšíření), který zadáte.</span><span class="sxs-lookup"><span data-stu-id="69244-106">It also describes how tooconfigure a web or worker role toouse a PHP runtime (with custom configuration and extensions) that you provide.</span></span>

## <a name="what-are-php-web-and-worker-roles"></a><span data-ttu-id="69244-107">Co jsou webové a pracovní role PHP?</span><span class="sxs-lookup"><span data-stu-id="69244-107">What are PHP web and worker roles?</span></span>
<span data-ttu-id="69244-108">Azure nabízí tři výpočetní modely pro provozování aplikací: Azure App Service, virtuální počítače Azure a cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="69244-108">Azure provides three compute models for running applications: Azure App Service, Azure Virtual Machines, and Azure Cloud Services.</span></span> <span data-ttu-id="69244-109">Všechny tři modely podporují PHP.</span><span class="sxs-lookup"><span data-stu-id="69244-109">All three models support PHP.</span></span> <span data-ttu-id="69244-110">Cloudové služby, včetně webových a pracovních rolí, poskytuje *platforma jako služba (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="69244-110">Cloud Services, which includes web and worker roles, provides *platform as a service (PaaS)*.</span></span> <span data-ttu-id="69244-111">V rámci cloudové služby webové role poskytuje vyhrazený webový server Internetové informační služby (IIS) serveru toohost front-endové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="69244-111">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server toohost front-end web applications.</span></span> <span data-ttu-id="69244-112">Asynchronní, dlouhotrvající nebo trvalé úlohy, které jsou nezávislé na vstupu nebo interakci uživatelů můžete spustit roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="69244-112">A worker role can run asynchronous, long-running or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="69244-113">Další informace o těchto možnostech najdete v tématu [výpočetní hostování možnosti poskytované Azure](cloud-services/cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="69244-113">For more information about these options, see [Compute hosting options provided by Azure](cloud-services/cloud-services-choose-me.md).</span></span>

## <a name="download-hello-azure-sdk-for-php"></a><span data-ttu-id="69244-114">Stáhnout hello Azure SDK pro jazyk PHP</span><span class="sxs-lookup"><span data-stu-id="69244-114">Download hello Azure SDK for PHP</span></span>
<span data-ttu-id="69244-115">Hello [Azure SDK pro jazyk PHP] se skládá z několika součástí.</span><span class="sxs-lookup"><span data-stu-id="69244-115">hello [Azure SDK for PHP] consists of several components.</span></span> <span data-ttu-id="69244-116">Tento článek se bude používat dvě z nich: prostředí Azure PowerShell a hello emulátorů Azure.</span><span class="sxs-lookup"><span data-stu-id="69244-116">This article will use two of them: Azure PowerShell and hello Azure emulators.</span></span> <span data-ttu-id="69244-117">Tyto dvě součásti lze nainstalovat pomocí instalačního programu webové platformy Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="69244-117">These two components can be installed via hello Microsoft Web Platform Installer.</span></span> <span data-ttu-id="69244-118">Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="69244-118">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="create-a-cloud-services-project"></a><span data-ttu-id="69244-119">Vytvoření projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="69244-119">Create a Cloud Services project</span></span>
<span data-ttu-id="69244-120">Hello prvním krokem při vytváření role web nebo worker PHP je toocreate projektu služby Azure.</span><span class="sxs-lookup"><span data-stu-id="69244-120">hello first step in creating a PHP web or worker role is toocreate an Azure Service project.</span></span> <span data-ttu-id="69244-121">projekt služby Azure slouží jako kontejner logické pro webové a pracovní role, která obsahuje hello projektu [služby definice (.csdef)] a [konfigurace služby (.cscfg)] soubory.</span><span class="sxs-lookup"><span data-stu-id="69244-121">an Azure Service project serves as a logical container for web and worker roles, and it contains hello project's [service definition (.csdef)] and [service configuration (.cscfg)] files.</span></span>

<span data-ttu-id="69244-122">toocreate nový projekt služeb Azure, spusťte prostředí Azure PowerShell jako správce a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="69244-122">toocreate a new Azure Service project, run Azure PowerShell as an administrator, and execute hello following command:</span></span>

    PS C:\>New-AzureServiceProject myProject

<span data-ttu-id="69244-123">Tento příkaz vytvoří nový adresář (`myProject`) toowhich můžete přidat webové a pracovní role.</span><span class="sxs-lookup"><span data-stu-id="69244-123">This command will create a new directory (`myProject`) toowhich you can add web and worker roles.</span></span>

## <a name="add-php-web-or-worker-roles"></a><span data-ttu-id="69244-124">Přidání rolí web nebo worker PHP</span><span class="sxs-lookup"><span data-stu-id="69244-124">Add PHP web or worker roles</span></span>
<span data-ttu-id="69244-125">tooadd projektu tooa PHP webové role, spusťte následující příkaz z v kořenovém adresáři projektu hello hello:</span><span class="sxs-lookup"><span data-stu-id="69244-125">tooadd a PHP web role tooa project, run hello following command from within hello project's root directory:</span></span>

    PS C:\myProject> Add-AzurePHPWebRole roleName

<span data-ttu-id="69244-126">Pro roli pracovního procesu použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="69244-126">For a worker role, use this command:</span></span>

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> <span data-ttu-id="69244-127">Hello `roleName` parametr je nepovinný.</span><span class="sxs-lookup"><span data-stu-id="69244-127">hello `roleName` parameter is optional.</span></span> <span data-ttu-id="69244-128">Je-li vynechán, budou automaticky generovány hello název role.</span><span class="sxs-lookup"><span data-stu-id="69244-128">If it is omitted, hello role name will be automatically generated.</span></span> <span data-ttu-id="69244-129">Hello první webovou roli vytvořili bude `WebRole1`, hello druhý bude `WebRole2`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="69244-129">hello first web role created will be `WebRole1`, hello second will be `WebRole2`, and so on.</span></span> <span data-ttu-id="69244-130">Hello první vytvoření role pracovního procesu bude `WorkerRole1`, hello druhý bude `WorkerRole2`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="69244-130">hello first worker role created will be `WorkerRole1`, hello second will be `WorkerRole2`, and so on.</span></span>
>
>

## <a name="specify-hello-built-in-php-version"></a><span data-ttu-id="69244-131">Zadejte verzi PHP předdefinované hello</span><span class="sxs-lookup"><span data-stu-id="69244-131">Specify hello built-in PHP version</span></span>
<span data-ttu-id="69244-132">PHP web nebo worker role tooa projektu přidáte, konfiguračních souborů projektu hello upravena tak, aby PHP bude nainstalována na každou instanci vaší aplikace web nebo pracovní proces, při nasazení.</span><span class="sxs-lookup"><span data-stu-id="69244-132">When you add a PHP web or worker role tooa project, hello project's configuration files are modified so that PHP will be installed on each web or worker instance of your application when it is deployed.</span></span> <span data-ttu-id="69244-133">toosee hello verze PHP, která bude nainstalována ve výchozím nastavení, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="69244-133">toosee hello version of PHP that will be installed by default, run hello following command:</span></span>

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

<span data-ttu-id="69244-134">Hello výstup z výše uvedených hello příkazu bude vypadat podobně jako toowhat jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="69244-134">hello output from hello command above will look similar toowhat is shown below.</span></span> <span data-ttu-id="69244-135">V tomto příkladu hello `IsDefault` je příznak nastaven příliš`true` pro jazyk PHP 5.3.17, která udává, že bude hello výchozí jazyk PHP verze nainstalované.</span><span class="sxs-lookup"><span data-stu-id="69244-135">In this example, hello `IsDefault` flag is set too`true` for PHP 5.3.17, indicating that it will be hello default PHP version installed.</span></span>

```
Runtime Version     PackageUri                      IsDefault
------- -------     ----------                      ---------
Node 0.6.17         http://nodertncu.blob.core...   False
Node 0.6.20         http://nodertncu.blob.core...   True
Node 0.8.4          http://nodertncu.blob.core...   False
IISNode 0.1.21      http://nodertncu.blob.core...   True
Cache 1.8.0         http://nodertncu.blob.core...   True
PHP 5.3.17          http://nodertncu.blob.core...   True
PHP 5.4.0           http://nodertncu.blob.core...   False
```

<span data-ttu-id="69244-136">Můžete nastavit hello PHP runtime verze tooany hello verze PHP, které jsou uvedeny.</span><span class="sxs-lookup"><span data-stu-id="69244-136">You can set hello PHP runtime version tooany of hello PHP versions that are listed.</span></span> <span data-ttu-id="69244-137">Například tooset verzi PHP hello (pro role s názvem hello `roleName`) too5.4.0 hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="69244-137">For example, tooset hello PHP version (for a role with hello name `roleName`) too5.4.0, use hello following command:</span></span>

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> <span data-ttu-id="69244-138">K dispozici verze PHP může změnit v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="69244-138">Available PHP versions may change in hello future.</span></span>
>
>

## <a name="customize-hello-built-in-php-runtime"></a><span data-ttu-id="69244-139">Přizpůsobení předdefinované runtime PHP hello</span><span class="sxs-lookup"><span data-stu-id="69244-139">Customize hello built-in PHP runtime</span></span>
<span data-ttu-id="69244-140">Máte plnou kontrolu nad hello konfiguraci hello PHP runtime, který je nainstalován, pokud budete postupovat podle hello výše uvedené kroky, včetně změny `php.ini` nastavení a povolení rozšíření.</span><span class="sxs-lookup"><span data-stu-id="69244-140">You have complete control over hello configuration of hello PHP runtime that is installed when you follow hello steps above, including modification of `php.ini` settings and enabling of extensions.</span></span>

<span data-ttu-id="69244-141">toocustomize hello předdefinované runtime PHP, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="69244-141">toocustomize hello built-in PHP runtime, follow these steps:</span></span>

1. <span data-ttu-id="69244-142">Přidejte novou složku, s názvem `php`, toohello `bin` adresář webové role.</span><span class="sxs-lookup"><span data-stu-id="69244-142">Add a new folder, named `php`, toohello `bin` directory of your web role.</span></span> <span data-ttu-id="69244-143">Pro roli pracovního procesu přidejte ji toohello role kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="69244-143">For a worker role, add it toohello role's root directory.</span></span>
2. <span data-ttu-id="69244-144">V hello `php` složce vytvořte jinou složku s názvem `ext`.</span><span class="sxs-lookup"><span data-stu-id="69244-144">In hello `php` folder, create another folder called `ext`.</span></span> <span data-ttu-id="69244-145">Uveďte všechny `.dll` soubory rozšíření (například `php_mongo.dll`), které chcete tooenable v této složce.</span><span class="sxs-lookup"><span data-stu-id="69244-145">Put any `.dll` extension files (e.g., `php_mongo.dll`) that you want tooenable in this folder.</span></span>
3. <span data-ttu-id="69244-146">Přidat `php.ini` souboru toohello `php` složky.</span><span class="sxs-lookup"><span data-stu-id="69244-146">Add a `php.ini` file toohello `php` folder.</span></span> <span data-ttu-id="69244-147">Povolit všechny vlastní rozšíření a nastavte všechny direktivy PHP v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="69244-147">Enable any custom extensions and set any PHP directives in this file.</span></span> <span data-ttu-id="69244-148">Například pokud byste chtěli tooturn `display_errors` na a povolte hello `php_mongo.dll` rozšíření, hello obsah vaší `php.ini` soubor vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="69244-148">For example, if you wanted tooturn `display_errors` on and enable hello `php_mongo.dll` extension, hello contents of your `php.ini` file would be as follows:</span></span>

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> <span data-ttu-id="69244-149">Všechna nastavení, která v hello nenastavíte explicitně `php.ini` souboru zadejte bude automaticky nastavit tootheir výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="69244-149">Any settings that you don't explicitly set in hello `php.ini` file that you provide will automatically be set tootheir default values.</span></span> <span data-ttu-id="69244-150">Ale mějte, můžete přidat úplná `php.ini` souboru.</span><span class="sxs-lookup"><span data-stu-id="69244-150">However, keep in mind that you can add a complete `php.ini` file.</span></span>
>
>

## <a name="use-your-own-php-runtime"></a><span data-ttu-id="69244-151">Použití vlastního modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="69244-151">Use your own PHP runtime</span></span>
<span data-ttu-id="69244-152">V některých případech, namísto výběru předdefinované modulu PHP runtime a její konfiguraci, jak je popsáno výše můžete tooprovide vlastního modulu PHP runtime.</span><span class="sxs-lookup"><span data-stu-id="69244-152">In some cases, instead of selecting a built-in PHP runtime and configuring it as described above, you may want tooprovide your own PHP runtime.</span></span> <span data-ttu-id="69244-153">Například můžete použít hello stejné modulu PHP runtime v roli web nebo pracovní proces, který používáte ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="69244-153">For example, you can use hello same PHP runtime in a web or worker role that you use in your development environment.</span></span> <span data-ttu-id="69244-154">Díky tomu je snazší tooensure, že aplikace hello nedojde ke změně chování v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="69244-154">This makes it easier tooensure that hello application will not change behavior in your production environment.</span></span>

### <a name="configure-a-web-role-toouse-your-own-php-runtime"></a><span data-ttu-id="69244-155">Konfigurace webové role toouse vlastního modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="69244-155">Configure a web role toouse your own PHP runtime</span></span>
<span data-ttu-id="69244-156">tooconfigure webové role toouse modulu PHP runtime, který zadáte, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="69244-156">tooconfigure a web role toouse a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="69244-157">Vytvoření projektu Azure Service a přidejte webovou roli PHP, jak je popsáno dříve v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="69244-157">Create an Azure Service project and add a PHP web role as described previously in this topic.</span></span>
2. <span data-ttu-id="69244-158">Vytvořit `php` složky v hello `bin` složky, která je v kořenovém adresáři webovou roli a poté přidejte vaše PHP runtime (všechny binární soubory, konfiguračních souborů, podsložky atd.) toohello `php` složky.</span><span class="sxs-lookup"><span data-stu-id="69244-158">Create a `php` folder in hello `bin` folder that is in your web role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) toohello `php` folder.</span></span>
3. <span data-ttu-id="69244-159">(VOLITELNÉ) Pokud vaše modulu PHP runtime používá hello [Drivers společnosti Microsoft pro jazyk PHP pro SQL Server][sqlsrv drivers], budete potřebovat tooconfigure vaší webové role tooinstall [SQL Server Native Client 2012] [ sql native client] při je zřízený.</span><span class="sxs-lookup"><span data-stu-id="69244-159">(OPTIONAL) If your PHP runtime uses hello [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need tooconfigure your web role tooinstall [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="69244-160">toodo, přidejte hello [instalační program sqlncli.msi x64] toohello `bin` složku v kořenovém adresáři webovou roli.</span><span class="sxs-lookup"><span data-stu-id="69244-160">toodo this, add hello [sqlncli.msi x64 installer] toohello `bin` folder in your web role's root directory.</span></span> <span data-ttu-id="69244-161">Hello spouštěcí skript popsané v dalším kroku hello bezobslužně spustí instalační program hello při zřizování hello role.</span><span class="sxs-lookup"><span data-stu-id="69244-161">hello startup script described in hello next step will silently run hello installer when hello role is provisioned.</span></span> <span data-ttu-id="69244-162">Pokud vaše modulu PHP runtime nepoužívá Drivers hello společnosti Microsoft pro jazyk PHP pro SQL Server, můžete odebrat hello ze skriptu hello vidět v dalším kroku hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="69244-162">If your PHP runtime does not use hello Microsoft Drivers for PHP for SQL Server, you can remove hello following line from hello script shown in hello next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="69244-163">Úloha spuštění, který konfiguruje definovat [Internetové informační služby (IIS)] [ iis.net] toouse vaše toohandle runtime PHP požadavky pro `.php` stránky.</span><span class="sxs-lookup"><span data-stu-id="69244-163">Define a startup task that configures [Internet Information Services (IIS)][iis.net] toouse your PHP runtime toohandle requests for `.php` pages.</span></span> <span data-ttu-id="69244-164">toodo se otevřete hello `setup_web.cmd` souboru (v hello `bin` souboru webovou roli kořenový adresář) v textovém editoru a nahraďte jeho obsah s hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="69244-164">toodo this, open hello `setup_web.cmd` file (in hello `bin` file of your web role's root directory) in a text editor and replace its contents with hello following script:</span></span>

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. <span data-ttu-id="69244-165">Přidání kořenového adresáře vaší aplikace soubory tooyour webové role.</span><span class="sxs-lookup"><span data-stu-id="69244-165">Add your application files tooyour web role's root directory.</span></span> <span data-ttu-id="69244-166">Bude jím hello webový server kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="69244-166">This will be hello web server's root directory.</span></span>
6. <span data-ttu-id="69244-167">Publikování aplikace, jak je popsáno v hello [publikování aplikace](#publish-your-application) části níže.</span><span class="sxs-lookup"><span data-stu-id="69244-167">Publish your application as described in hello [Publish your application](#publish-your-application) section below.</span></span>

> [!NOTE]
> <span data-ttu-id="69244-168">Hello `download.ps1` skriptu (v hello `bin` složky hello webovou roli kořenový adresář) můžete odstranit po provedení kroků hello popsané výše pro použití vlastního runtime PHP.</span><span class="sxs-lookup"><span data-stu-id="69244-168">hello `download.ps1` script (in hello `bin` folder of hello web role's root directory) can be deleted after you follow hello steps described above for using your own PHP runtime.</span></span>
>
>

### <a name="configure-a-worker-role-toouse-your-own-php-runtime"></a><span data-ttu-id="69244-169">Konfigurace pracovní role toouse vlastního modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="69244-169">Configure a worker role toouse your own PHP runtime</span></span>
<span data-ttu-id="69244-170">tooconfigure pracovní role toouse modulu PHP runtime, který zadáte, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="69244-170">tooconfigure a worker role toouse a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="69244-171">Vytvoření projektu Azure Service a přidejte roli pracovního procesu PHP, jak je popsáno dříve v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="69244-171">Create an Azure Service project and add a PHP worker role as described previously in this topic.</span></span>
2. <span data-ttu-id="69244-172">Vytvoření `php` složku v kořenovém adresáři role pracovního procesu hello a poté přidejte vaše PHP runtime (všechny binární soubory, konfiguračních souborů, podsložky atd.) toohello `php` složky.</span><span class="sxs-lookup"><span data-stu-id="69244-172">Create a `php` folder in hello worker role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) toohello `php` folder.</span></span>
3. <span data-ttu-id="69244-173">(VOLITELNÉ) Pokud vaše modulu PHP runtime používá [Drivers společnosti Microsoft pro jazyk PHP pro SQL Server][sqlsrv drivers], budete potřebovat tooconfigure vaše tooinstall role pracovního procesu [SQL Server Native Client 2012] [ sql native client] když je zřízený.</span><span class="sxs-lookup"><span data-stu-id="69244-173">(OPTIONAL) If your PHP runtime uses [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need tooconfigure your worker role tooinstall [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="69244-174">toodo, přidejte hello [instalační program sqlncli.msi x64] role pracovního procesu toohello kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="69244-174">toodo this, add hello [sqlncli.msi x64 installer] toohello worker role's root directory.</span></span> <span data-ttu-id="69244-175">Hello spouštěcí skript popsané v dalším kroku hello bezobslužně spustí instalační program hello při zřizování hello role.</span><span class="sxs-lookup"><span data-stu-id="69244-175">hello startup script described in hello next step will silently run hello installer when hello role is provisioned.</span></span> <span data-ttu-id="69244-176">Pokud vaše modulu PHP runtime nepoužívá Drivers hello společnosti Microsoft pro jazyk PHP pro SQL Server, můžete odebrat hello ze skriptu hello vidět v dalším kroku hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="69244-176">If your PHP runtime does not use hello Microsoft Drivers for PHP for SQL Server, you can remove hello following line from hello script shown in hello next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="69244-177">Definování úloha spuštění, který přidává vaší `php.exe` proměnné prostředí PATH role pracovního procesu spustitelného souboru toohello při zřizování hello role.</span><span class="sxs-lookup"><span data-stu-id="69244-177">Define a startup task that adds your `php.exe` executable toohello worker role's PATH environment variable when hello role is provisioned.</span></span> <span data-ttu-id="69244-178">toodo se otevřete hello `setup_worker.cmd` (v roli pracovního procesu hello kořenový adresář) soubor v textovém editoru a nahraďte jeho obsah hello následující skript:</span><span class="sxs-lookup"><span data-stu-id="69244-178">toodo this, open hello `setup_worker.cmd` file (in hello worker role's root directory) in a text editor and replace its contents with hello following script:</span></span>

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service toohello web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. <span data-ttu-id="69244-179">Přidáte své aplikace soubory tooyour role pracovního procesu je kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="69244-179">Add your application files tooyour worker role's root directory.</span></span>
6. <span data-ttu-id="69244-180">Publikování aplikace, jak je popsáno v hello [publikování aplikace](#publish-your-application) části níže.</span><span class="sxs-lookup"><span data-stu-id="69244-180">Publish your application as described in hello [Publish your application](#publish-your-application) section below.</span></span>

## <a name="run-your-application-in-hello-compute-and-storage-emulators"></a><span data-ttu-id="69244-181">Spusťte aplikaci v hello emulátorů výpočetního prostředí a úložiště</span><span class="sxs-lookup"><span data-stu-id="69244-181">Run your application in hello compute and storage emulators</span></span>
<span data-ttu-id="69244-182">Hello Azure emulátorů zadejte místní prostředí, ve kterém můžete testovat aplikaci Azure před nasazením toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="69244-182">hello Azure emulators provide a local environment in which you can test your Azure application before you deploy it toohello cloud.</span></span> <span data-ttu-id="69244-183">Existují určité rozdíly mezi hello emulátorů a hello prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="69244-183">There are some differences between hello emulators and hello Azure environment.</span></span> <span data-ttu-id="69244-184">toounderstand to lépe, najdete v části [používejte hello emulátoru úložiště Azure pro vývoj a testování](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="69244-184">toounderstand this better, see [Use hello Azure storage emulator for development and testing](storage/common/storage-use-emulator.md).</span></span>

<span data-ttu-id="69244-185">Všimněte si, že je nutné mít PHP nainstalovány místně emulátoru služby výpočty toouse hello.</span><span class="sxs-lookup"><span data-stu-id="69244-185">Note that you must have PHP installed locally toouse hello compute emulator.</span></span> <span data-ttu-id="69244-186">emulátoru služby výpočty Hello použije vaše místní instalace toorun PHP vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="69244-186">hello compute emulator will use your local PHP installation toorun your application.</span></span>

<span data-ttu-id="69244-187">spustit projekt v hello emulátorů toorun hello následující příkaz z kořenového adresáře projektu na:</span><span class="sxs-lookup"><span data-stu-id="69244-187">toorun your project in hello emulators, execute hello following command from your project's root directory:</span></span>

    PS C:\MyProject> Start-AzureEmulator

<span data-ttu-id="69244-188">Zobrazí se výstup podobný toothis:</span><span class="sxs-lookup"><span data-stu-id="69244-188">You will see output similar toothis:</span></span>

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

<span data-ttu-id="69244-189">Uvidí vaši aplikaci běžící v emulátoru hello otevřením webový prohlížeč a procházení toohello místní adresu zobrazí ve výstupu hello (`http://127.0.0.1:81` v hello příklad výstupu výše).</span><span class="sxs-lookup"><span data-stu-id="69244-189">You can see your application running in hello emulator by opening a web browser and browsing toohello local address shown in hello output (`http://127.0.0.1:81` in hello example output above).</span></span>

<span data-ttu-id="69244-190">toostop hello emulátorů, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="69244-190">toostop hello emulators, execute this command:</span></span>

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a><span data-ttu-id="69244-191">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="69244-191">Publish your application</span></span>
<span data-ttu-id="69244-192">toopublish vaší aplikace, musíte importovat toofirst vaše nastavení publikování pomocí hello [Import AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="69244-192">toopublish your application, you need toofirst import your publish settings by using hello [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet.</span></span> <span data-ttu-id="69244-193">Potom můžete publikovat aplikace pomocí hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="69244-193">Then you can publish your application by using hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet.</span></span> <span data-ttu-id="69244-194">Informace o přihlašování najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="69244-194">For information about signing in, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="next-steps"></a><span data-ttu-id="69244-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="69244-195">Next steps</span></span>
<span data-ttu-id="69244-196">Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="69244-196">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

[Azure SDK pro jazyk PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[služby definice (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[konfigurace služby (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[instalační program sqlncli.msi x64]: http://go.microsoft.com/fwlink/?LinkID=239648
