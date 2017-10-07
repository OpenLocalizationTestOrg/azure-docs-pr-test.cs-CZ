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
# <a name="how-toocreate-php-web-and-worker-roles"></a>Jak toocreate PHP webových a pracovních rolí
## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak vybrat konkrétní verzi PHP z hello "integrovaných" verzí, které jsou k dispozici role web nebo worker PHP toocreate v vývojového prostředí systému Windows, změnit konfiguraci hello PHP, povolit rozšíření a nakonec nasazení tooAzure. Také popisuje, jak tooconfigure web nebo worker role toouse modul runtime PHP (s vlastní konfigurace a rozšíření), který zadáte.

## <a name="what-are-php-web-and-worker-roles"></a>Co jsou webové a pracovní role PHP?
Azure nabízí tři výpočetní modely pro provozování aplikací: Azure App Service, virtuální počítače Azure a cloudových služeb Azure. Všechny tři modely podporují PHP. Cloudové služby, včetně webových a pracovních rolí, poskytuje *platforma jako služba (PaaS)*. V rámci cloudové služby webové role poskytuje vyhrazený webový server Internetové informační služby (IIS) serveru toohost front-endové webové aplikace. Asynchronní, dlouhotrvající nebo trvalé úlohy, které jsou nezávislé na vstupu nebo interakci uživatelů můžete spustit roli pracovního procesu.

Další informace o těchto možnostech najdete v tématu [výpočetní hostování možnosti poskytované Azure](cloud-services/cloud-services-choose-me.md).

## <a name="download-hello-azure-sdk-for-php"></a>Stáhnout hello Azure SDK pro jazyk PHP
Hello [Azure SDK pro jazyk PHP] se skládá z několika součástí. Tento článek se bude používat dvě z nich: prostředí Azure PowerShell a hello emulátorů Azure. Tyto dvě součásti lze nainstalovat pomocí instalačního programu webové platformy Microsoft hello. Další informace najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="create-a-cloud-services-project"></a>Vytvoření projektu cloudové služby
Hello prvním krokem při vytváření role web nebo worker PHP je toocreate projektu služby Azure. projekt služby Azure slouží jako kontejner logické pro webové a pracovní role, která obsahuje hello projektu [služby definice (.csdef)] a [konfigurace služby (.cscfg)] soubory.

toocreate nový projekt služeb Azure, spusťte prostředí Azure PowerShell jako správce a spusťte následující příkaz hello:

    PS C:\>New-AzureServiceProject myProject

Tento příkaz vytvoří nový adresář (`myProject`) toowhich můžete přidat webové a pracovní role.

## <a name="add-php-web-or-worker-roles"></a>Přidání rolí web nebo worker PHP
tooadd projektu tooa PHP webové role, spusťte následující příkaz z v kořenovém adresáři projektu hello hello:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Pro roli pracovního procesu použijte tento příkaz:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> Hello `roleName` parametr je nepovinný. Je-li vynechán, budou automaticky generovány hello název role. Hello první webovou roli vytvořili bude `WebRole1`, hello druhý bude `WebRole2`a tak dále. Hello první vytvoření role pracovního procesu bude `WorkerRole1`, hello druhý bude `WorkerRole2`a tak dále.
>
>

## <a name="specify-hello-built-in-php-version"></a>Zadejte verzi PHP předdefinované hello
PHP web nebo worker role tooa projektu přidáte, konfiguračních souborů projektu hello upravena tak, aby PHP bude nainstalována na každou instanci vaší aplikace web nebo pracovní proces, při nasazení. toosee hello verze PHP, která bude nainstalována ve výchozím nastavení, spusťte následující příkaz hello:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

Hello výstup z výše uvedených hello příkazu bude vypadat podobně jako toowhat jsou uvedeny níže. V tomto příkladu hello `IsDefault` je příznak nastaven příliš`true` pro jazyk PHP 5.3.17, která udává, že bude hello výchozí jazyk PHP verze nainstalované.

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

Můžete nastavit hello PHP runtime verze tooany hello verze PHP, které jsou uvedeny. Například tooset verzi PHP hello (pro role s názvem hello `roleName`) too5.4.0 hello použijte následující příkaz:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> K dispozici verze PHP může změnit v budoucnu hello.
>
>

## <a name="customize-hello-built-in-php-runtime"></a>Přizpůsobení předdefinované runtime PHP hello
Máte plnou kontrolu nad hello konfiguraci hello PHP runtime, který je nainstalován, pokud budete postupovat podle hello výše uvedené kroky, včetně změny `php.ini` nastavení a povolení rozšíření.

toocustomize hello předdefinované runtime PHP, postupujte takto:

1. Přidejte novou složku, s názvem `php`, toohello `bin` adresář webové role. Pro roli pracovního procesu přidejte ji toohello role kořenový adresář.
2. V hello `php` složce vytvořte jinou složku s názvem `ext`. Uveďte všechny `.dll` soubory rozšíření (například `php_mongo.dll`), které chcete tooenable v této složce.
3. Přidat `php.ini` souboru toohello `php` složky. Povolit všechny vlastní rozšíření a nastavte všechny direktivy PHP v tomto souboru. Například pokud byste chtěli tooturn `display_errors` na a povolte hello `php_mongo.dll` rozšíření, hello obsah vaší `php.ini` soubor vypadat takto:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> Všechna nastavení, která v hello nenastavíte explicitně `php.ini` souboru zadejte bude automaticky nastavit tootheir výchozí hodnoty. Ale mějte, můžete přidat úplná `php.ini` souboru.
>
>

## <a name="use-your-own-php-runtime"></a>Použití vlastního modulu PHP runtime
V některých případech, namísto výběru předdefinované modulu PHP runtime a její konfiguraci, jak je popsáno výše můžete tooprovide vlastního modulu PHP runtime. Například můžete použít hello stejné modulu PHP runtime v roli web nebo pracovní proces, který používáte ve vašem vývojovém prostředí. Díky tomu je snazší tooensure, že aplikace hello nedojde ke změně chování v provozním prostředí.

### <a name="configure-a-web-role-toouse-your-own-php-runtime"></a>Konfigurace webové role toouse vlastního modulu PHP runtime
tooconfigure webové role toouse modulu PHP runtime, který zadáte, postupujte takto:

1. Vytvoření projektu Azure Service a přidejte webovou roli PHP, jak je popsáno dříve v tomto tématu.
2. Vytvořit `php` složky v hello `bin` složky, která je v kořenovém adresáři webovou roli a poté přidejte vaše PHP runtime (všechny binární soubory, konfiguračních souborů, podsložky atd.) toohello `php` složky.
3. (VOLITELNÉ) Pokud vaše modulu PHP runtime používá hello [Drivers společnosti Microsoft pro jazyk PHP pro SQL Server][sqlsrv drivers], budete potřebovat tooconfigure vaší webové role tooinstall [SQL Server Native Client 2012] [ sql native client] při je zřízený. toodo, přidejte hello [instalační program sqlncli.msi x64] toohello `bin` složku v kořenovém adresáři webovou roli. Hello spouštěcí skript popsané v dalším kroku hello bezobslužně spustí instalační program hello při zřizování hello role. Pokud vaše modulu PHP runtime nepoužívá Drivers hello společnosti Microsoft pro jazyk PHP pro SQL Server, můžete odebrat hello ze skriptu hello vidět v dalším kroku hello následující řádek:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Úloha spuštění, který konfiguruje definovat [Internetové informační služby (IIS)] [ iis.net] toouse vaše toohandle runtime PHP požadavky pro `.php` stránky. toodo se otevřete hello `setup_web.cmd` souboru (v hello `bin` souboru webovou roli kořenový adresář) v textovém editoru a nahraďte jeho obsah s hello následující skript:

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
5. Přidání kořenového adresáře vaší aplikace soubory tooyour webové role. Bude jím hello webový server kořenový adresář.
6. Publikování aplikace, jak je popsáno v hello [publikování aplikace](#publish-your-application) části níže.

> [!NOTE]
> Hello `download.ps1` skriptu (v hello `bin` složky hello webovou roli kořenový adresář) můžete odstranit po provedení kroků hello popsané výše pro použití vlastního runtime PHP.
>
>

### <a name="configure-a-worker-role-toouse-your-own-php-runtime"></a>Konfigurace pracovní role toouse vlastního modulu PHP runtime
tooconfigure pracovní role toouse modulu PHP runtime, který zadáte, postupujte takto:

1. Vytvoření projektu Azure Service a přidejte roli pracovního procesu PHP, jak je popsáno dříve v tomto tématu.
2. Vytvoření `php` složku v kořenovém adresáři role pracovního procesu hello a poté přidejte vaše PHP runtime (všechny binární soubory, konfiguračních souborů, podsložky atd.) toohello `php` složky.
3. (VOLITELNÉ) Pokud vaše modulu PHP runtime používá [Drivers společnosti Microsoft pro jazyk PHP pro SQL Server][sqlsrv drivers], budete potřebovat tooconfigure vaše tooinstall role pracovního procesu [SQL Server Native Client 2012] [ sql native client] když je zřízený. toodo, přidejte hello [instalační program sqlncli.msi x64] role pracovního procesu toohello kořenový adresář. Hello spouštěcí skript popsané v dalším kroku hello bezobslužně spustí instalační program hello při zřizování hello role. Pokud vaše modulu PHP runtime nepoužívá Drivers hello společnosti Microsoft pro jazyk PHP pro SQL Server, můžete odebrat hello ze skriptu hello vidět v dalším kroku hello následující řádek:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definování úloha spuštění, který přidává vaší `php.exe` proměnné prostředí PATH role pracovního procesu spustitelného souboru toohello při zřizování hello role. toodo se otevřete hello `setup_worker.cmd` (v roli pracovního procesu hello kořenový adresář) soubor v textovém editoru a nahraďte jeho obsah hello následující skript:

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
5. Přidáte své aplikace soubory tooyour role pracovního procesu je kořenový adresář.
6. Publikování aplikace, jak je popsáno v hello [publikování aplikace](#publish-your-application) části níže.

## <a name="run-your-application-in-hello-compute-and-storage-emulators"></a>Spusťte aplikaci v hello emulátorů výpočetního prostředí a úložiště
Hello Azure emulátorů zadejte místní prostředí, ve kterém můžete testovat aplikaci Azure před nasazením toohello cloudu. Existují určité rozdíly mezi hello emulátorů a hello prostředí Azure. toounderstand to lépe, najdete v části [používejte hello emulátoru úložiště Azure pro vývoj a testování](storage/common/storage-use-emulator.md).

Všimněte si, že je nutné mít PHP nainstalovány místně emulátoru služby výpočty toouse hello. emulátoru služby výpočty Hello použije vaše místní instalace toorun PHP vaší aplikace.

spustit projekt v hello emulátorů toorun hello následující příkaz z kořenového adresáře projektu na:

    PS C:\MyProject> Start-AzureEmulator

Zobrazí se výstup podobný toothis:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Uvidí vaši aplikaci běžící v emulátoru hello otevřením webový prohlížeč a procházení toohello místní adresu zobrazí ve výstupu hello (`http://127.0.0.1:81` v hello příklad výstupu výše).

toostop hello emulátorů, spusťte tento příkaz:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Publikování aplikace
toopublish vaší aplikace, musíte importovat toofirst vaše nastavení publikování pomocí hello [Import AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) rutiny. Potom můžete publikovat aplikace pomocí hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) rutiny. Informace o přihlašování najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu hello [středisku pro vývojáře PHP](/develop/php/).

[Azure SDK pro jazyk PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[služby definice (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[konfigurace služby (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[instalační program sqlncli.msi x64]: http://go.microsoft.com/fwlink/?LinkID=239648
