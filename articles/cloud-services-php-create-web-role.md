---
title: "Vytvoření Azure webových a pracovních rolí pro jazyk PHP | Microsoft Docs"
description: "Pokyny pro vytváření webových a pracovních rolí PHP v cloudové službě Azure a konfigurace modulu PHP runtime."
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
ms.openlocfilehash: 214fdcfe20f3fa4ebcbe41308404f8b7e7d15310
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-php-web-and-worker-roles"></a><span data-ttu-id="25e74-103">Postup vytvoření PHP webových a pracovních rolí</span><span class="sxs-lookup"><span data-stu-id="25e74-103">How to create PHP web and worker roles</span></span>
## <a name="overview"></a><span data-ttu-id="25e74-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="25e74-104">Overview</span></span>
<span data-ttu-id="25e74-105">Tento průvodce vám ukáže, jak vytvořit role web nebo worker PHP ve vývojovém prostředí Windows, vybrat konkrétní verzi PHP z "integrovaných" verzí, které jsou k dispozici, měnit nastavení PHP, povolit rozšíření a nakonec nasazení do Azure.</span><span class="sxs-lookup"><span data-stu-id="25e74-105">This guide will show you how to create PHP web or worker roles in a Windows development environment, choose a specific version of PHP from the "built-in" versions available, change the PHP configuration, enable extensions, and finally, deploy to Azure.</span></span> <span data-ttu-id="25e74-106">Také popisuje, jak nakonfigurujete roli web nebo worker použít modul runtime PHP (s vlastní konfigurace a rozšíření), který zadáte.</span><span class="sxs-lookup"><span data-stu-id="25e74-106">It also describes how to configure a web or worker role to use a PHP runtime (with custom configuration and extensions) that you provide.</span></span>

## <a name="what-are-php-web-and-worker-roles"></a><span data-ttu-id="25e74-107">Co jsou webové a pracovní role PHP?</span><span class="sxs-lookup"><span data-stu-id="25e74-107">What are PHP web and worker roles?</span></span>
<span data-ttu-id="25e74-108">Azure nabízí tři výpočetní modely pro provozování aplikací: Azure App Service, virtuální počítače Azure a cloudových služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="25e74-108">Azure provides three compute models for running applications: Azure App Service, Azure Virtual Machines, and Azure Cloud Services.</span></span> <span data-ttu-id="25e74-109">Všechny tři modely podporují PHP.</span><span class="sxs-lookup"><span data-stu-id="25e74-109">All three models support PHP.</span></span> <span data-ttu-id="25e74-110">Cloudové služby, včetně webových a pracovních rolí, poskytuje *platforma jako služba (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="25e74-110">Cloud Services, which includes web and worker roles, provides *platform as a service (PaaS)*.</span></span> <span data-ttu-id="25e74-111">Webové role v rámci cloudové služby poskytuje vyhrazený webový server Internetové informační služby (IIS) na hostiteli front-endové webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="25e74-111">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server to host front-end web applications.</span></span> <span data-ttu-id="25e74-112">Asynchronní, dlouhotrvající nebo trvalé úlohy, které jsou nezávislé na vstupu nebo interakci uživatelů můžete spustit roli pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="25e74-112">A worker role can run asynchronous, long-running or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="25e74-113">Další informace o těchto možnostech najdete v tématu [výpočetní hostování možnosti poskytované Azure](cloud-services/cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="25e74-113">For more information about these options, see [Compute hosting options provided by Azure](cloud-services/cloud-services-choose-me.md).</span></span>

## <a name="download-the-azure-sdk-for-php"></a><span data-ttu-id="25e74-114">Stažení sady Azure SDK pro PHP</span><span class="sxs-lookup"><span data-stu-id="25e74-114">Download the Azure SDK for PHP</span></span>
<span data-ttu-id="25e74-115">[Azure SDK pro jazyk PHP] se skládá z několika součástí.</span><span class="sxs-lookup"><span data-stu-id="25e74-115">The [Azure SDK for PHP] consists of several components.</span></span> <span data-ttu-id="25e74-116">Tento článek se bude používat dvě z nich: Azure PowerShell a Azure emulátorů.</span><span class="sxs-lookup"><span data-stu-id="25e74-116">This article will use two of them: Azure PowerShell and the Azure emulators.</span></span> <span data-ttu-id="25e74-117">Tyto dvě součásti lze nainstalovat pomocí instalačního programu webové platformy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="25e74-117">These two components can be installed via the Microsoft Web Platform Installer.</span></span> <span data-ttu-id="25e74-118">Další informace najdete v tématu [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="25e74-118">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="create-a-cloud-services-project"></a><span data-ttu-id="25e74-119">Vytvoření projektu cloudové služby</span><span class="sxs-lookup"><span data-stu-id="25e74-119">Create a Cloud Services project</span></span>
<span data-ttu-id="25e74-120">Prvním krokem při vytvoření role web nebo worker PHP je vytvoření projektu Azure Service.</span><span class="sxs-lookup"><span data-stu-id="25e74-120">The first step in creating a PHP web or worker role is to create an Azure Service project.</span></span> <span data-ttu-id="25e74-121">projekt služby Azure slouží jako kontejner logické pro webové a pracovní role, která obsahuje projektu [služby definice (.csdef)] a [konfigurace služby (.cscfg)] soubory.</span><span class="sxs-lookup"><span data-stu-id="25e74-121">an Azure Service project serves as a logical container for web and worker roles, and it contains the project's [service definition (.csdef)] and [service configuration (.cscfg)] files.</span></span>

<span data-ttu-id="25e74-122">Pokud chcete vytvořit nový projekt služeb Azure, spusťte prostředí Azure PowerShell jako správce a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="25e74-122">To create a new Azure Service project, run Azure PowerShell as an administrator, and execute the following command:</span></span>

    PS C:\>New-AzureServiceProject myProject

<span data-ttu-id="25e74-123">Tento příkaz vytvoří nový adresář (`myProject`) do kterého můžete přidat webové a pracovní role.</span><span class="sxs-lookup"><span data-stu-id="25e74-123">This command will create a new directory (`myProject`) to which you can add web and worker roles.</span></span>

## <a name="add-php-web-or-worker-roles"></a><span data-ttu-id="25e74-124">Přidání rolí web nebo worker PHP</span><span class="sxs-lookup"><span data-stu-id="25e74-124">Add PHP web or worker roles</span></span>
<span data-ttu-id="25e74-125">Pokud chcete přidat do projektu webové role PHP, spusťte následující příkaz v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="25e74-125">To add a PHP web role to a project, run the following command from within the project's root directory:</span></span>

    PS C:\myProject> Add-AzurePHPWebRole roleName

<span data-ttu-id="25e74-126">Pro roli pracovního procesu použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="25e74-126">For a worker role, use this command:</span></span>

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> <span data-ttu-id="25e74-127">`roleName` Parametr je nepovinný.</span><span class="sxs-lookup"><span data-stu-id="25e74-127">The `roleName` parameter is optional.</span></span> <span data-ttu-id="25e74-128">Je-li vynechán, budou automaticky generovány název role.</span><span class="sxs-lookup"><span data-stu-id="25e74-128">If it is omitted, the role name will be automatically generated.</span></span> <span data-ttu-id="25e74-129">Bude vytvořen první webovou roli `WebRole1`, druhý bude `WebRole2`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="25e74-129">The first web role created will be `WebRole1`, the second will be `WebRole2`, and so on.</span></span> <span data-ttu-id="25e74-130">První role pracovního procesu vytvoření bude `WorkerRole1`, druhý bude `WorkerRole2`a tak dále.</span><span class="sxs-lookup"><span data-stu-id="25e74-130">The first worker role created will be `WorkerRole1`, the second will be `WorkerRole2`, and so on.</span></span>
>
>

## <a name="specify-the-built-in-php-version"></a><span data-ttu-id="25e74-131">Zadejte předdefinované verzi PHP</span><span class="sxs-lookup"><span data-stu-id="25e74-131">Specify the built-in PHP version</span></span>
<span data-ttu-id="25e74-132">Když přidáte roli web nebo worker PHP do projektu, projektu konfigurační soubory jsou upravit tak, aby PHP bude nainstalována na každou instanci vaší aplikace web nebo pracovní proces, při nasazení.</span><span class="sxs-lookup"><span data-stu-id="25e74-132">When you add a PHP web or worker role to a project, the project's configuration files are modified so that PHP will be installed on each web or worker instance of your application when it is deployed.</span></span> <span data-ttu-id="25e74-133">Pokud chcete zobrazit verzi PHP, která bude nainstalována ve výchozím nastavení, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="25e74-133">To see the version of PHP that will be installed by default, run the following command:</span></span>

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

<span data-ttu-id="25e74-134">Výstup z výše uvedeného příkazu bude vypadat podobně jako co jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="25e74-134">The output from the command above will look similar to what is shown below.</span></span> <span data-ttu-id="25e74-135">V tomto příkladu `IsDefault` je příznak nastaven na `true` pro jazyk PHP 5.3.17, která udává, zda je výchozí verze PHP nainstalován.</span><span class="sxs-lookup"><span data-stu-id="25e74-135">In this example, the `IsDefault` flag is set to `true` for PHP 5.3.17, indicating that it will be the default PHP version installed.</span></span>

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

<span data-ttu-id="25e74-136">Modul runtime PHP můžete nastavit na verze PHP, které jsou uvedené.</span><span class="sxs-lookup"><span data-stu-id="25e74-136">You can set the PHP runtime version to any of the PHP versions that are listed.</span></span> <span data-ttu-id="25e74-137">Chcete-li například nastavit verzi PHP (pro roli s názvem `roleName`) k 5.4.0, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="25e74-137">For example, to set the PHP version (for a role with the name `roleName`) to 5.4.0, use the following command:</span></span>

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> <span data-ttu-id="25e74-138">K dispozici verze PHP může v budoucnu změnit.</span><span class="sxs-lookup"><span data-stu-id="25e74-138">Available PHP versions may change in the future.</span></span>
>
>

## <a name="customize-the-built-in-php-runtime"></a><span data-ttu-id="25e74-139">Přizpůsobení předdefinované modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="25e74-139">Customize the built-in PHP runtime</span></span>
<span data-ttu-id="25e74-140">Máte plnou kontrolu nad konfigurace modulu PHP runtime, který je nainstalován, pokud budete postupovat podle kroků výše, včetně změny `php.ini` nastavení a povolení rozšíření.</span><span class="sxs-lookup"><span data-stu-id="25e74-140">You have complete control over the configuration of the PHP runtime that is installed when you follow the steps above, including modification of `php.ini` settings and enabling of extensions.</span></span>

<span data-ttu-id="25e74-141">Chcete-li přizpůsobit předdefinované runtime PHP, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="25e74-141">To customize the built-in PHP runtime, follow these steps:</span></span>

1. <span data-ttu-id="25e74-142">Přidejte novou složku, s názvem `php`do `bin` adresář webové role.</span><span class="sxs-lookup"><span data-stu-id="25e74-142">Add a new folder, named `php`, to the `bin` directory of your web role.</span></span> <span data-ttu-id="25e74-143">Pro roli pracovního procesu přidejte ji do role kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="25e74-143">For a worker role, add it to the role's root directory.</span></span>
2. <span data-ttu-id="25e74-144">V `php` složce vytvořte jinou složku s názvem `ext`.</span><span class="sxs-lookup"><span data-stu-id="25e74-144">In the `php` folder, create another folder called `ext`.</span></span> <span data-ttu-id="25e74-145">Uveďte všechny `.dll` soubory rozšíření (například `php_mongo.dll`), které chcete povolit v této složce.</span><span class="sxs-lookup"><span data-stu-id="25e74-145">Put any `.dll` extension files (e.g., `php_mongo.dll`) that you want to enable in this folder.</span></span>
3. <span data-ttu-id="25e74-146">Přidat `php.ini` do souboru `php` složky.</span><span class="sxs-lookup"><span data-stu-id="25e74-146">Add a `php.ini` file to the `php` folder.</span></span> <span data-ttu-id="25e74-147">Povolit všechny vlastní rozšíření a nastavte všechny direktivy PHP v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="25e74-147">Enable any custom extensions and set any PHP directives in this file.</span></span> <span data-ttu-id="25e74-148">Například, pokud chcete zapnout `display_errors` na a povolte `php_mongo.dll` rozšíření, obsah vaší `php.ini` soubor vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="25e74-148">For example, if you wanted to turn `display_errors` on and enable the `php_mongo.dll` extension, the contents of your `php.ini` file would be as follows:</span></span>

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> <span data-ttu-id="25e74-149">Všechna nastavení, která v nenastavíte explicitně `php.ini` souboru zadejte bude automaticky nastavit výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="25e74-149">Any settings that you don't explicitly set in the `php.ini` file that you provide will automatically be set to their default values.</span></span> <span data-ttu-id="25e74-150">Ale mějte, můžete přidat úplná `php.ini` souboru.</span><span class="sxs-lookup"><span data-stu-id="25e74-150">However, keep in mind that you can add a complete `php.ini` file.</span></span>
>
>

## <a name="use-your-own-php-runtime"></a><span data-ttu-id="25e74-151">Použití vlastního modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="25e74-151">Use your own PHP runtime</span></span>
<span data-ttu-id="25e74-152">V některých případech, namísto výběru předdefinované modulu PHP runtime a nakonfigurovat jej jak bylo popsáno výše můžete zadat vlastní modul runtime PHP.</span><span class="sxs-lookup"><span data-stu-id="25e74-152">In some cases, instead of selecting a built-in PHP runtime and configuring it as described above, you may want to provide your own PHP runtime.</span></span> <span data-ttu-id="25e74-153">Například můžete použít stejné modulu PHP runtime v roli web nebo pracovní proces, který používáte ve vašem vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="25e74-153">For example, you can use the same PHP runtime in a web or worker role that you use in your development environment.</span></span> <span data-ttu-id="25e74-154">To usnadňuje Ujistěte se, že aplikace nedojde ke změně chování v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="25e74-154">This makes it easier to ensure that the application will not change behavior in your production environment.</span></span>

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a><span data-ttu-id="25e74-155">Konfigurace webové roli, kterou chcete použít vlastní modulu PHP runtime</span><span class="sxs-lookup"><span data-stu-id="25e74-155">Configure a web role to use your own PHP runtime</span></span>
<span data-ttu-id="25e74-156">Pokud chcete konfigurovat webovou roli na-li použít modul runtime PHP, který zadáte, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="25e74-156">To configure a web role to use a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="25e74-157">Vytvoření projektu Azure Service a přidejte webovou roli PHP, jak je popsáno dříve v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="25e74-157">Create an Azure Service project and add a PHP web role as described previously in this topic.</span></span>
2. <span data-ttu-id="25e74-158">Vytvoření `php` složky v `bin` složka, která je v kořenovém adresáři webovou roli a potom přidat do vašeho modulu PHP runtime (všechny binární soubory, konfiguračních souborů, podsložky atd.) `php` složky.</span><span class="sxs-lookup"><span data-stu-id="25e74-158">Create a `php` folder in the `bin` folder that is in your web role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) to the `php` folder.</span></span>
3. <span data-ttu-id="25e74-159">(VOLITELNÉ) Pokud vaše modulu PHP runtime používá [Drivers společnosti Microsoft pro jazyk PHP pro SQL Server][sqlsrv drivers], budete muset nakonfigurovat vaši webovou roli nainstalovat [SQL Server Native Client 2012] [ sql native client] když je zřízený.</span><span class="sxs-lookup"><span data-stu-id="25e74-159">(OPTIONAL) If your PHP runtime uses the [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need to configure your web role to install [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="25e74-160">Chcete-li to provést, přidejte [sqlncli.msi x64 instalační program] k `bin` složku v kořenovém adresáři webovou roli.</span><span class="sxs-lookup"><span data-stu-id="25e74-160">To do this, add the [sqlncli.msi x64 installer] to the `bin` folder in your web role's root directory.</span></span> <span data-ttu-id="25e74-161">Spuštění skriptu popsané v dalším kroku bude bezobslužně spusťte instalační program při zřízení roli.</span><span class="sxs-lookup"><span data-stu-id="25e74-161">The startup script described in the next step will silently run the installer when the role is provisioned.</span></span> <span data-ttu-id="25e74-162">Pokud vaše modulu PHP runtime nepoužívá Drivers společnosti Microsoft pro jazyk PHP pro SQL Server, můžete odebrat ze skriptu vidět v dalším kroku následující řádek:</span><span class="sxs-lookup"><span data-stu-id="25e74-162">If your PHP runtime does not use the Microsoft Drivers for PHP for SQL Server, you can remove the following line from the script shown in the next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="25e74-163">Úloha spuštění, který konfiguruje definovat [Internetové informační služby (IIS)] [ iis.net] používat vaše modulu PHP runtime pro zpracování požadavků pro `.php` stránky.</span><span class="sxs-lookup"><span data-stu-id="25e74-163">Define a startup task that configures [Internet Information Services (IIS)][iis.net] to use your PHP runtime to handle requests for `.php` pages.</span></span> <span data-ttu-id="25e74-164">Chcete-li to provést, otevřete `setup_web.cmd` souboru (v `bin` souboru webovou roli kořenový adresář) v textovém editoru a nahraďte jeho obsah následující skript:</span><span class="sxs-lookup"><span data-stu-id="25e74-164">To do this, open the `setup_web.cmd` file (in the `bin` file of your web role's root directory) in a text editor and replace its contents with the following script:</span></span>

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
5. <span data-ttu-id="25e74-165">Přidání souborů aplikací na vaši webovou roli kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="25e74-165">Add your application files to your web role's root directory.</span></span> <span data-ttu-id="25e74-166">To bude webový server kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="25e74-166">This will be the web server's root directory.</span></span>
6. <span data-ttu-id="25e74-167">Publikování aplikace, jak je popsáno v [publikování aplikace](#publish-your-application) části níže.</span><span class="sxs-lookup"><span data-stu-id="25e74-167">Publish your application as described in the [Publish your application](#publish-your-application) section below.</span></span>

> [!NOTE]
> <span data-ttu-id="25e74-168">`download.ps1` Skriptu (v `bin` složky webovou roli kořenový adresář) můžete odstranit po podle kroků popsaných výše pro použití vlastního runtime PHP.</span><span class="sxs-lookup"><span data-stu-id="25e74-168">The `download.ps1` script (in the `bin` folder of the web role's root directory) can be deleted after you follow the steps described above for using your own PHP runtime.</span></span>
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a><span data-ttu-id="25e74-169">Konfigurace role pracovních procesů na využití vlastního modulu runtime PHP</span><span class="sxs-lookup"><span data-stu-id="25e74-169">Configure a worker role to use your own PHP runtime</span></span>
<span data-ttu-id="25e74-170">Pokud chcete konfigurovat role pracovních procesů na využití modulu runtime PHP, které poskytujete, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="25e74-170">To configure a worker role to use a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="25e74-171">Vytvoření projektu Azure Service a přidejte roli pracovního procesu PHP, jak je popsáno dříve v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="25e74-171">Create an Azure Service project and add a PHP worker role as described previously in this topic.</span></span>
2. <span data-ttu-id="25e74-172">Vytvoření `php` složku v kořenovém adresáři role pracovního procesu a pak přidejte do vašeho modulu PHP runtime (všechny binární soubory, konfiguračních souborů, podsložky atd.) `php` složky.</span><span class="sxs-lookup"><span data-stu-id="25e74-172">Create a `php` folder in the worker role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) to the `php` folder.</span></span>
3. <span data-ttu-id="25e74-173">(VOLITELNÉ) Pokud vaše modulu PHP runtime používá [Drivers společnosti Microsoft pro jazyk PHP pro SQL Server][sqlsrv drivers], budete muset konfigurovat své role pracovního procesu instalace [SQL Server Native Client 2012] [ sql native client] když je zřízený.</span><span class="sxs-lookup"><span data-stu-id="25e74-173">(OPTIONAL) If your PHP runtime uses [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need to configure your worker role to install [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="25e74-174">Chcete-li to provést, přidejte [sqlncli.msi x64 instalační program] pro roli pracovního procesu kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="25e74-174">To do this, add the [sqlncli.msi x64 installer] to the worker role's root directory.</span></span> <span data-ttu-id="25e74-175">Spuštění skriptu popsané v dalším kroku bude bezobslužně spusťte instalační program při zřízení roli.</span><span class="sxs-lookup"><span data-stu-id="25e74-175">The startup script described in the next step will silently run the installer when the role is provisioned.</span></span> <span data-ttu-id="25e74-176">Pokud vaše modulu PHP runtime nepoužívá Drivers společnosti Microsoft pro jazyk PHP pro SQL Server, můžete odebrat ze skriptu vidět v dalším kroku následující řádek:</span><span class="sxs-lookup"><span data-stu-id="25e74-176">If your PHP runtime does not use the Microsoft Drivers for PHP for SQL Server, you can remove the following line from the script shown in the next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="25e74-177">Definování úloha spuštění, který přidává vaší `php.exe` spustitelný soubor do proměnné prostředí PATH role pracovního procesu při zřízení roli.</span><span class="sxs-lookup"><span data-stu-id="25e74-177">Define a startup task that adds your `php.exe` executable to the worker role's PATH environment variable when the role is provisioned.</span></span> <span data-ttu-id="25e74-178">Chcete-li to provést, otevřete `setup_worker.cmd` (v roli pracovního procesu kořenový adresář) soubor v textovém editoru a nahraďte jeho obsah následující skript:</span><span class="sxs-lookup"><span data-stu-id="25e74-178">To do this, open the `setup_worker.cmd` file (in the worker role's root directory) in a text editor and replace its contents with the following script:</span></span>

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service to the web root directory...
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
5. <span data-ttu-id="25e74-179">Přidáte soubory aplikací do kořenového adresáře role pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="25e74-179">Add your application files to your worker role's root directory.</span></span>
6. <span data-ttu-id="25e74-180">Publikování aplikace, jak je popsáno v [publikování aplikace](#publish-your-application) části níže.</span><span class="sxs-lookup"><span data-stu-id="25e74-180">Publish your application as described in the [Publish your application](#publish-your-application) section below.</span></span>

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a><span data-ttu-id="25e74-181">Spusťte aplikaci v emulátorů výpočetního prostředí a úložiště</span><span class="sxs-lookup"><span data-stu-id="25e74-181">Run your application in the compute and storage emulators</span></span>
<span data-ttu-id="25e74-182">Azure emulátorů, zadejte do místního prostředí, ve kterém můžete testovat aplikaci Azure předtím, než můžete nasadit do cloudu.</span><span class="sxs-lookup"><span data-stu-id="25e74-182">The Azure emulators provide a local environment in which you can test your Azure application before you deploy it to the cloud.</span></span> <span data-ttu-id="25e74-183">Existují určité rozdíly mezi emulátorů a prostředí Azure.</span><span class="sxs-lookup"><span data-stu-id="25e74-183">There are some differences between the emulators and the Azure environment.</span></span> <span data-ttu-id="25e74-184">To lépe pochopit, najdete v části [pomocí emulátoru úložiště Azure pro vývoj a testování](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="25e74-184">To understand this better, see [Use the Azure storage emulator for development and testing](storage/common/storage-use-emulator.md).</span></span>

<span data-ttu-id="25e74-185">Všimněte si, že musí mít nainstalované místně pro použití emulátoru služby výpočty v PHP.</span><span class="sxs-lookup"><span data-stu-id="25e74-185">Note that you must have PHP installed locally to use the compute emulator.</span></span> <span data-ttu-id="25e74-186">Emulátoru služby výpočty v použije místní instalace PHP spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="25e74-186">The compute emulator will use your local PHP installation to run your application.</span></span>

<span data-ttu-id="25e74-187">Pokud chcete spustit projekt v emulátorů, spusťte následující příkaz z kořenového adresáře projektu na:</span><span class="sxs-lookup"><span data-stu-id="25e74-187">To run your project in the emulators, execute the following command from your project's root directory:</span></span>

    PS C:\MyProject> Start-AzureEmulator

<span data-ttu-id="25e74-188">Zobrazí se výstup podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="25e74-188">You will see output similar to this:</span></span>

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

<span data-ttu-id="25e74-189">Uvidí vaši aplikaci běžící v emulátoru kliknutím otevřete webový prohlížeč a přejděte na adresu místního zobrazené ve výstupu (`http://127.0.0.1:81` ve výstupu v příkladu výše).</span><span class="sxs-lookup"><span data-stu-id="25e74-189">You can see your application running in the emulator by opening a web browser and browsing to the local address shown in the output (`http://127.0.0.1:81` in the example output above).</span></span>

<span data-ttu-id="25e74-190">Chcete-li zastavit emulátorů, spusťte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="25e74-190">To stop the emulators, execute this command:</span></span>

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a><span data-ttu-id="25e74-191">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="25e74-191">Publish your application</span></span>
<span data-ttu-id="25e74-192">Pro publikování aplikace, musíte nejdříve naimportovat vaše nastavení publikování pomocí [Import AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="25e74-192">To publish your application, you need to first import your publish settings by using the [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet.</span></span> <span data-ttu-id="25e74-193">Potom můžete publikovat aplikace pomocí [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="25e74-193">Then you can publish your application by using the [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet.</span></span> <span data-ttu-id="25e74-194">Informace o přihlašování najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="25e74-194">For information about signing in, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="next-steps"></a><span data-ttu-id="25e74-195">Další kroky</span><span class="sxs-lookup"><span data-stu-id="25e74-195">Next steps</span></span>
<span data-ttu-id="25e74-196">Další informace najdete v tématu [středisku pro vývojáře PHP](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="25e74-196">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

[Azure SDK pro jazyk PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[služby definice (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[konfigurace služby (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 instalační program]: http://go.microsoft.com/fwlink/?LinkID=239648
