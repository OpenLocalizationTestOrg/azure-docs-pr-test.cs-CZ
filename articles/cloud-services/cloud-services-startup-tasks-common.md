---
title: "spuštění úlohy aaaCommon pro cloudové služby | Microsoft Docs"
description: "Poskytuje několik příkladů běžných úloh spuštění může být vhodné tooperform ve vaší cloudové služby webovou roli nebo role pracovního procesu."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: c80fac4079439410dfc3795e4bce0fbc07dbbfab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="common-cloud-service-startup-tasks"></a>Běžné úlohy spuštění cloudové služby
Tento článek obsahuje několik příkladů běžných úloh spuštění může být vhodné tooperform v rámci cloudové služby. Spuštění úlohy tooperform operace můžete před zahájením roli. Operace může být, že chcete tooperform zahrnovat instalaci komponenty, registraci komponenty modelu COM, nastavení klíče registru nebo dlouhotrvající proces. 

V tématu [v tomto článku](cloud-services-startup-tasks.md) toounderstand fungování spuštění úlohy a konkrétně jak toocreate hello položky, které definují úloha spuštění.

> [!NOTE]
> Spuštění úlohy nejsou použitelné tooVirtual počítače, jenom tooCloud webové služby a rolí pracovního procesu.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Definování proměnné prostředí před spuštěním roli
Pokud potřebujete proměnné definované pro konkrétní úkol, použijte hello [prostředí] element uvnitř hello [úloh] elementu.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Můžete také použít proměnné [platnou hodnotu Azure XPath](cloud-services-role-config-xpath.md) tooreference něco o hello nasazení. Místo použití hello `value` atribut, definování [RoleInstanceValue] podřízený element.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>Konfigurace spuštění služby IIS s AppCmd.exe
Hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) nástroj příkazového řádku lze použít toomanage nastavení služby IIS na spuštění v Azure. *AppCmd.exe* poskytuje pohodlné příkazového řádku přístup tooconfiguration nastavení pro použití při spuštění úlohy v Azure. Pomocí *AppCmd.exe*, nastavení webu můžete přidat, změnit ani odebrat, pro aplikace a weby.

Existuje však několik věcí toowatch se pro použití hello *AppCmd.exe* jako úloha spuštění:

* Spuštění úlohy mohou být spouštěny více než jednou mezi jednotlivými restartováními. Například když roli recyklování.
* Pokud *AppCmd.exe* akce se provádí více než jednou, to může generovat chybu. Například pokus tooadd oddíl příliš*Web.config* dvakrát může způsobit chybu.
* Spuštění úlohy nezdaří, pokud se vrátí nenulový ukončovací kód nebo **errorlevel**. Například když *AppCmd.exe* , vygeneruje se chyba.

Je dobrým zvykem toocheck hello **errorlevel** po volání *AppCmd.exe*, která je snadno toodo, pokud příliš zabalení hello volání*AppCmd.exe* s *cmd.*  souboru. Pokud zjistíte známá **errorlevel** odpovědi, můžete ho ignorovat nebo předejte ji zpět.

Hello errorlevel vrácený *AppCmd.exe* jsou uvedeny v souboru winerror.h hello a můžete také zobrazit na [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).

### <a name="example-of-managing-hello-error-level"></a>Příklad správy úroveň chyb hello
Tento příklad přidá části komprese a položku komprese pro JSON toohello *Web.config* soubor s chybou zpracování a protokolování.

Hello odpovídající části hello [ServiceDefinition.csdef] souboru zde se zobrazují, mezi které patří nastavení hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) atribut příliš`elevated` toogive *AppCmd.exe*  nastavení hello toochange dostatečná oprávnění v hello *Web.config* souboru:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Hello *Startup.cmd* batch používá soubor *AppCmd.exe* tooadd části komprese a položku komprese pro JSON toohello *Web.config* souboru. Hello očekává **errorlevel** z 183 nastavena toozero pomocí hello ověřit. EXE příkazového řádku programu. Neočekávané hodnoty parametru ERRORLEVEL při jsou zaznamenané tooStartupErrorLog.txt.

```cmd
REM   *** Add a compression section toohello Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying tooadd a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
REM   task. toohandle this situation, set hello ERRORLEVEL toozero by using hello Verify command. hello Verify
REM   command will safely set hello ERRORLEVEL toozero.
IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL

REM   If hello ERRORLEVEL is not zero at this point, some other error occurred.
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding a compression section toohello Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Add compression for json. ***
%windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
IF %ERRORLEVEL% EQU 183 VERIFY > NUL
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding hello JSON compression type toohello Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Exit batch file. ***
EXIT /b 0

REM   *** Log error and exit ***
:ErrorExit
REM   Report hello date, time, and ERRORLEVEL of hello error.
DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT %ERRORLEVEL%
```

## <a name="add-firewall-rules"></a>Přidat pravidla brány firewall
V Azure jsou efektivně dvě brány firewall. první brána firewall Hello řídí připojení mezi virtuálním počítačem hello a hello mimo world. Tato brána firewall řídí hello [koncové body] element v hello [ServiceDefinition.csdef] souboru.

Druhá brána firewall Hello řídí připojení mezi hello virtuálního počítače a procesy hello v rámci virtuálního počítače. Tato brána firewall může být řízena hello `netsh advfirewall firewall` nástroj příkazového řádku.

Azure vytvoří pravidla brány firewall pro hello procesy spuštěné v rámci role. Například při spuštění služby nebo aplikace, Azure automaticky vytvoří tooallow pravidla brány firewall nezbytné hello této služby toocommunicate s hello Internetu. Ale pokud vytvoříte službu, která se spustí proces mimo vaši roli (např. Windows naplánované úlohy nebo služby COM +), musíte toomanually vytvoření služby toothat přístup tooallow pravidlo brány firewall. Tato pravidla brány firewall můžete vytvořit pomocí úloha spuštění.

Úloha spuštění, který vytvoří pravidlo brány firewall musí mít [executionContext][úloh] z **zvýšenými**. Přidejte následující spuštění úloh toohello hello [ServiceDefinition.csdef] souboru.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

pravidlo brány firewall hello tooadd, je nutné použít hello odpovídající `netsh advfirewall firewall` příkazy do spuštění dávkového souboru. V tomto příkladu hello spuštění úloha vyžaduje zabezpečení a šifrování pro TCP port 80.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Blokovat konkrétní IP adresu
Sadu Azure webové role přístup tooa ze zadaných IP adres můžete omezit změnou vaší služby IIS **web.config** souboru. Musíte taky toouse soubor příkazů, které odemyká hello **ipSecurity** části hello **ApplicationHost.config** souboru.

toodo odemknutí hello **ipSecurity** části hello **ApplicationHost.config** souboru, vytvořte soubor příkaz, který se spustí na spuštění role. Vytvořte složku na kořenové úrovni hello webové role názvem **spuštění** a v této složce vytvořte dávkový soubor s názvem **startup.cmd**. Přidejte tento projekt sady Visual Studio tooyour souborů a nastavte vlastnosti hello příliš**kopie vždy** tooensure, která je součástí vašeho balíčku.

Přidejte následující spuštění úloh toohello hello [ServiceDefinition.csdef] souboru.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Přidejte tento příkaz toohello **startup.cmd** souboru:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Tato úloha způsobí, že hello **startup.cmd** dávky toobe soubor spustit pokaždé, když je inicializovat hello webovou roli, zajistit, že hello požadované **ipSecurity** pořizování.

Nakonec upravte hello [oddíl system.webServer](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) webovou roli **web.config** souboru tooadd seznam IP adres, které mají udělen přístup, jak je znázorněno v hello následující ukázka:

Tato ukázka konfigurace **umožňuje** všechny IP adresy tooaccess hello serveru s výjimkou dva definované hello

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--hello following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Tato ukázka konfigurace **odmítne** všechny IP adresy v přístupu k serveru hello s výjimkou hello dva definované.

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--hello following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>Vytvoření úlohy spuštění prostředí PowerShell
Skripty prostředí Windows PowerShell nelze volat přímo z hello [ServiceDefinition.csdef] souboru, ale můžete vyvolat z v dávkovém souboru spuštění.

Prostředí PowerShell (ve výchozím nastavení) nelze spustit nepodepsané skripty. Pokud váš skript, je nutné tooconfigure prostředí PowerShell toorun nepodepsané skripty. toorun nepodepsané skripty, hello **ExecutionPolicy** musí být nastaven příliš**bez omezení**. Hello **ExecutionPolicy** nastavení použití vychází hello verzi prostředí Windows PowerShell.

```cmd
REM   Run an unsigned PowerShell script and log hello output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

Pokud používáte hostovaného operačního systému, který je běží PowerShell 2.0 nebo 1.0 můžete vynutit toorun verze 2 a pokud není k dispozici, použijte verze 1.

```cmd
REM   Attempt tooset hello execution policy by using PowerShell version 2.0 syntax.
PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If PowerShell version 2.0 isn't available. Set hello execution policy by using hello PowerShell
IF %ERRORLEVEL% EQU -393216 (
   PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
   PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
)

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Vytvoření souborů v místním úložišti z spuštění úlohy
Můžete použít toostore prostředků místní úložiště souborů vytvořených vaše úloha spuštění, které je přístupné později vaše aplikace.

toocreate hello prostředků místní úložiště, přidejte [LocalResources] části toohello [ServiceDefinition.csdef] souboru a poté přidejte hello [LocalStorage] podřízený element. Poskytují hello místní úložiště prostředků jedinečný název a odpovídající velikost pro spuštění úkolu.

toouse místní úložiště prostředků v vaše úloha spuštění, budete potřebovat toocreate z prostředí proměnné tooreference hello místní úložiště prostředků umístění. Potom hello spuštění úlohy a aplikace hello jsou možné tooread a zapisovat soubory toohello místní úložiště prostředků.

Hello odpovídající části hello **ServiceDefinition.csdef** zde se zobrazují souboru:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...

    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Jako příklad to **Startup.cmd** dávkový soubor používá hello **PathToStartupStorage** prostředí proměnné toocreate hello souboru **MyTest.txt** v místním úložišti hello umístění.

```cmd
REM   Create a simple text file.

ECHO This text will go into hello MyTest.txt file which will be in hello    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed tooby hello PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO hello contents of hello PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit hello batch file with ERRORLEVEL 0.

EXIT /b 0
```

Místní úložiště složky můžete přístup z hello Azure SDK pomocí hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metoda.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-hello-emulator-or-cloud"></a>Spustit v emulátoru hello nebo v cloudu
Může mít vaše úloha spuštění provádět různé kroky, když ho pracuje v cloudu porovnání toowhen hello, je v emulátoru služby výpočty hello. Můžete například toouse novou kopii dat SQL jenom v případě, že je spuštěna v emulátoru hello. Nebo můžete chtít toodo některých optimalizací výkonu pro cloud hello, nepotřebujete toodo při spuštění v emulátoru hello.

Tato možnost tooperform různé akce na hello emulátor služby výpočty a hello cloudu, můžete provést vytvoření proměnné prostředí v hello [ServiceDefinition.csdef] souboru. Potom otestovat tuto proměnnou prostředí pro hodnotu ve spuštění úkolu.

proměnné prostředí hello toocreate, přidat hello [proměnná]/[RoleInstanceValue] element a vytvořit má hodnotu XPath `/RoleEnvironment/Deployment/@emulated`. Hello hodnotu hello **ComputeEmulatorRunning %** proměnná prostředí je `true` při spuštění v emulátoru služby výpočty hello, a `false` při spuštění v cloudu hello.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

Hello úloh můžete zkontrolovat nyní hello **ComputeEmulatorRunning %** prostředí proměnné tooperform různé akce založené na tom, jestli se hello role běží v hello cloudu nebo hello emulátor. Zde je skript prostředí .cmd, který kontroluje pro tuto proměnnou prostředí.

```cmd
REM   Check if this task is running on hello compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on hello compute emulator. Perform tasks that must be run only in hello compute emulator.
) ELSE (
    REM   This task is running on hello cloud. Perform tasks that must be run only in hello cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Zjistit, že úlohu již spuštěn
Hello role může recyklujte bez restartování způsobuje vaší toorun spuštění úlohy znovu. Neexistuje žádné tooindicate příznak, který úlohu byl již spuštěn hello hostování virtuálních počítačů. Může mít některé úlohy, kde nezáleží na spuštění více než jednou. Však může spustit do situace, kdy potřebujete tooprevent úlohu ve spuštění více než jednou.

Hello nejjednodušší způsob, jak toodetect, již byla spuštěna úloha je toocreate soubor v hello **% TEMP %** složku Po úspěšné hello úloh a podívejte se na hello spuštění úlohy hello. Tady je ukázkový skript prostředí cmd, který pro sebe.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%RoleRoot%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   hello application installed without error. Create a file tooindicate that hello task
  REM   does not need toobe run again.

  ECHO This line will create a file tooindicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"

) ELSE (
  REM   An error occurred. Log hello error and exit with hello error code.

  DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
  TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
  ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

  EXIT %ERRORLEVEL%
)

:Finish

REM   Exit normally.
EXIT /B 0
```

## <a name="task-best-practices"></a>Osvědčené postupy úloh
Tady jsou některé z osvědčených postupů, které byste měli postupovat při konfiguraci úloh pro váš web nebo worker roli.

### <a name="always-log-startup-activities"></a>Vždy protokolu spuštění aktivit
Visual Studio neposkytuje toostep ladicí program prostřednictvím dávkové soubory, proto je dobré tooget tolik dat na hello operace dávkové soubory míře. Protokolování výstupu hello dávkové soubory i **stdout** a **stderr**, můžete poskytnout důležité informace při pokusu o toodebug a opravte dávkové soubory. toolog obou **stdout** a **stderr** toohello StartupLog.txt souboru v hello directory odkazováno tooby hello **% TEMP %** proměnné prostředí, přidejte hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1`toohello konec konkrétní řádků je vhodné toolog. Například tooexecute setup.exe v hello **PathToApp1Install %** directory:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

toosimplify soubor xml, můžete vytvořit obálku *cmd* soubor, který volá všechny vaše spuštění úloh společně s protokolování a zajistí každé sdílené složky hello podřízené úlohy stejné proměnné prostředí.

Možná bude ale obtěžování toouse `>> "%TEMP%\StartupLog.txt" 2>&1` na konci hello každé spuštění úlohy. Protokolování úloh můžete vynutit tím, že vytvoříte obálku, která zpracovává protokolování pro vás. Tuto obálku volá hello skutečné dávkového souboru chcete toorun. Všechny výstupy z hello cíl dávkový soubor bude přesměrované toohello *Startuplog.txt* souboru.

Následující příklad ukazuje, jak tooredirect všechny výstup z dávkového souboru spuštění Hello. V tomto příkladu vytvoří soubor ServerDefinition.csdef hello spuštění úlohu, která volá *logwrap.cmd*. *logwrap.cmd* volání *Startup2.cmd*, přesměrování veškerý výstup příliš**% TEMP %\\StartupLog.txt**.

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output toohello StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call hello child command batch file, redirecting all output toohello StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log hello completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0

) ELSE (

   REM   Log hello error.
   ECHO [%date% %time%] An error occurred. hello ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%

)
```

**Startup2.cmd:**

```cmd
@ECHO OFF

REM   This is hello batch file where hello startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in hello directory pointed tooby hello TEMP environment variable.

REM   If an error occurs, hello following command will pass hello ERRORLEVEL back toothe
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

Ukázka výstupu v hello **StartupLog.txt** souboru:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> Hello **StartupLog.txt** soubor je umístěný ve hello *C:\Resources\temp\\{identifikátor role} \RoleTemp* složky.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Nastavit executionContext pro spuštění úlohy
Nastavte oprávnění pro spuštění úloh hello. Spuštění úlohy někdy musí spusťte s vyššími oprávněními, i když hello role běží s normálními oprávněními.

Hello [executionContext][úloh] atribut nastaví úroveň oprávnění hello hello spuštění úlohy. Pomocí `executionContext="limited"` znamená má úloha spuštění hello hello stejnou úroveň oprávnění jako hello role. Pomocí `executionContext="elevated"` znamená úloha spuštění hello má oprávnění správce, který hello spuštění umožňuje úkolů tooperform Správce úloh bez nutnosti poskytnutí oprávnění tooyour role správce.

Úloha spuštění, který používá je například spuštění úlohu, která vyžaduje zvýšená oprávnění **AppCmd.exe** tooconfigure služby IIS. **AppCmd.exe** vyžaduje `executionContext="elevated"`.

### <a name="use-hello-appropriate-tasktype"></a>Použít příslušné taskType hello
Hello [taskType][úloh] atribut určuje hello způsob hello spuštění úlohy je spuštěn. Existují tři hodnoty: **jednoduché**, **pozadí**, a **popředí**. Hello popředí a na pozadí úlohy se spouštějí asynchronně, a potom synchronně spuštěn hello jednoduché úlohy po jednom.

S **jednoduché** spuštění úlohy, můžete nastavit hello pořadí, ve kterém hello úlohy spouštět podle hello pořadí, ve které hello úlohy jsou uvedeny v souboru ServiceDefinition.csdef hello. Pokud **jednoduché** úloh na konci nenulový ukončovací kód a potom hello spuštění procedury zastaví a hello role se nespustí.

Hello rozdíl mezi **pozadí** spuštění úlohy a **popředí** spuštění úlohy je, že **popředí** úlohy zachovat hello role spuštění, dokud hello  **popředí** úkolů elementy end. To také znamená, že pokud hello **popředí** úloh přestane reagovat nebo dojde k chybě, dokud hello neprovede recyklování hello role **popředí** úloh je nucen uzavřen. Z tohoto důvodu **pozadí** úlohy se doporučují pro asynchronní spuštění úloh, pokud je třeba tuto funkci hello **popředí** úloh.

### <a name="end-batch-files-with-exit-b-0"></a>End dávkové soubory s UKONČOVACÍM /B 0
Hello role začne Pokud hello **errorlevel** z každé vaší jednoduché spuštění úloh je nulová. Ne všechny programy nastavit hello **errorlevel** (ukončovací kód) správně, takže hello dávkový soubor musí končit `EXIT /B 0` Pokud se vše správně spustil.

Chybějící `EXIT /B 0` v hello konce souboru spuštění dávky je obvyklou příčinou rolí, které nebudou spuštěny.

> [!NOTE]
> I jste si všimli této vnořené dávkové soubory někdy zablokování, když pomocí hello `/B` parametr. Můžete chtít, aby tento problém zablokování neodehrává volá-li jiný dávkový soubor váš aktuální dávkový soubor, třeba Pokud použijete hello toomake [obálku protokolu](#always-log-startup-activities). Je možné vynechat hello `/B` parametr v tomto případě.
> 
> 

### <a name="expect-startup-tasks-toorun-more-than-once"></a>Spuštění úlohy toorun očekávat více než jednou.
Ne všechny role recykluje zahrnovat restart, ale všechny role recykluje zahrnují spuštění všech spuštění úloh. To znamená, že spuštění úlohy musí být schopný toorun vícekrát mezi jednotlivými restartováními bez problémů. To je podrobněji hello [předcházející části](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-toostore-files-that-must-be-accessed-in-hello-role"></a>Použít místní úložiště toostore souborů, které musí být přístupné v roli hello
Chcete-li toocopy nebo vytvořte soubor během spuštění úkolu, je pak přístupné tooyour role a potom tento soubor musí být umístěn v místním úložišti. V tématu hello [předcházející části](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Další kroky
Zkontrolujte hello cloudu [služby modelu a balíčku](cloud-services-model-and-package.md)

Další informace o [úlohy](cloud-services-startup-tasks.md) fungovat.

[Vytvoření a nasazení](cloud-services-how-to-create-deploy-portal.md) balíček vaše cloudové služby.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[úloh]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[prostředí]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[proměnná]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Koncové body]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
