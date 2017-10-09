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
# <a name="common-cloud-service-startup-tasks"></a><span data-ttu-id="c8d17-103">Běžné úlohy spuštění cloudové služby</span><span class="sxs-lookup"><span data-stu-id="c8d17-103">Common Cloud Service startup tasks</span></span>
<span data-ttu-id="c8d17-104">Tento článek obsahuje několik příkladů běžných úloh spuštění může být vhodné tooperform v rámci cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c8d17-104">This article provides some examples of common startup tasks you may want tooperform in your cloud service.</span></span> <span data-ttu-id="c8d17-105">Spuštění úlohy tooperform operace můžete před zahájením roli.</span><span class="sxs-lookup"><span data-stu-id="c8d17-105">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="c8d17-106">Operace může být, že chcete tooperform zahrnovat instalaci komponenty, registraci komponenty modelu COM, nastavení klíče registru nebo dlouhotrvající proces.</span><span class="sxs-lookup"><span data-stu-id="c8d17-106">Operations that you might want tooperform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span> 

<span data-ttu-id="c8d17-107">V tématu [v tomto článku](cloud-services-startup-tasks.md) toounderstand fungování spuštění úlohy a konkrétně jak toocreate hello položky, které definují úloha spuštění.</span><span class="sxs-lookup"><span data-stu-id="c8d17-107">See [this article](cloud-services-startup-tasks.md) toounderstand how startup tasks work, and specifically how toocreate hello entries that define a startup task.</span></span>

> [!NOTE]
> <span data-ttu-id="c8d17-108">Spuštění úlohy nejsou použitelné tooVirtual počítače, jenom tooCloud webové služby a rolí pracovního procesu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-108">Startup tasks are not applicable tooVirtual Machines, only tooCloud Service Web and Worker roles.</span></span>
> 

## <a name="define-environment-variables-before-a-role-starts"></a><span data-ttu-id="c8d17-109">Definování proměnné prostředí před spuštěním roli</span><span class="sxs-lookup"><span data-stu-id="c8d17-109">Define environment variables before a role starts</span></span>
<span data-ttu-id="c8d17-110">Pokud potřebujete proměnné definované pro konkrétní úkol, použijte hello [prostředí] element uvnitř hello [úloh] elementu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-110">If you need environment variables defined for a specific task, use hello [Environment] element inside hello [Task] element.</span></span>

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

<span data-ttu-id="c8d17-111">Můžete také použít proměnné [platnou hodnotu Azure XPath](cloud-services-role-config-xpath.md) tooreference něco o hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="c8d17-111">Variables can also use a [valid Azure XPath value](cloud-services-role-config-xpath.md) tooreference something about hello deployment.</span></span> <span data-ttu-id="c8d17-112">Místo použití hello `value` atribut, definování [RoleInstanceValue] podřízený element.</span><span class="sxs-lookup"><span data-stu-id="c8d17-112">Instead of using hello `value` attribute, define a [RoleInstanceValue] child element.</span></span>

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a><span data-ttu-id="c8d17-113">Konfigurace spuštění služby IIS s AppCmd.exe</span><span class="sxs-lookup"><span data-stu-id="c8d17-113">Configure IIS startup with AppCmd.exe</span></span>
<span data-ttu-id="c8d17-114">Hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) nástroj příkazového řádku lze použít toomanage nastavení služby IIS na spuštění v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8d17-114">hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) command-line tool can be used toomanage IIS settings at startup on Azure.</span></span> <span data-ttu-id="c8d17-115">*AppCmd.exe* poskytuje pohodlné příkazového řádku přístup tooconfiguration nastavení pro použití při spuštění úlohy v Azure.</span><span class="sxs-lookup"><span data-stu-id="c8d17-115">*AppCmd.exe* provides convenient, command-line access tooconfiguration settings for use in startup tasks on Azure.</span></span> <span data-ttu-id="c8d17-116">Pomocí *AppCmd.exe*, nastavení webu můžete přidat, změnit ani odebrat, pro aplikace a weby.</span><span class="sxs-lookup"><span data-stu-id="c8d17-116">Using *AppCmd.exe*, Website settings can be added, modified, or removed for applications and sites.</span></span>

<span data-ttu-id="c8d17-117">Existuje však několik věcí toowatch se pro použití hello *AppCmd.exe* jako úloha spuštění:</span><span class="sxs-lookup"><span data-stu-id="c8d17-117">However, there are a few things toowatch out for in hello use of *AppCmd.exe* as a startup task:</span></span>

* <span data-ttu-id="c8d17-118">Spuštění úlohy mohou být spouštěny více než jednou mezi jednotlivými restartováními.</span><span class="sxs-lookup"><span data-stu-id="c8d17-118">Startup tasks can be run more than once between reboots.</span></span> <span data-ttu-id="c8d17-119">Například když roli recyklování.</span><span class="sxs-lookup"><span data-stu-id="c8d17-119">For instance, when a role recycles.</span></span>
* <span data-ttu-id="c8d17-120">Pokud *AppCmd.exe* akce se provádí více než jednou, to může generovat chybu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-120">If a *AppCmd.exe* action is performed more than once, it may generate an error.</span></span> <span data-ttu-id="c8d17-121">Například pokus tooadd oddíl příliš*Web.config* dvakrát může způsobit chybu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-121">For example, attempting tooadd a section too*Web.config* twice could generate an error.</span></span>
* <span data-ttu-id="c8d17-122">Spuštění úlohy nezdaří, pokud se vrátí nenulový ukončovací kód nebo **errorlevel**.</span><span class="sxs-lookup"><span data-stu-id="c8d17-122">Startup tasks fail if they return a non-zero exit code or **errorlevel**.</span></span> <span data-ttu-id="c8d17-123">Například když *AppCmd.exe* , vygeneruje se chyba.</span><span class="sxs-lookup"><span data-stu-id="c8d17-123">For example, when *AppCmd.exe* generates an error.</span></span>

<span data-ttu-id="c8d17-124">Je dobrým zvykem toocheck hello **errorlevel** po volání *AppCmd.exe*, která je snadno toodo, pokud příliš zabalení hello volání*AppCmd.exe* s *cmd.*  souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-124">It is a good practice toocheck hello **errorlevel** after calling *AppCmd.exe*, which is easy toodo if you wrap hello call too*AppCmd.exe* with a *.cmd* file.</span></span> <span data-ttu-id="c8d17-125">Pokud zjistíte známá **errorlevel** odpovědi, můžete ho ignorovat nebo předejte ji zpět.</span><span class="sxs-lookup"><span data-stu-id="c8d17-125">If you detect a known **errorlevel** response, you can ignore it, or pass it back.</span></span>

<span data-ttu-id="c8d17-126">Hello errorlevel vrácený *AppCmd.exe* jsou uvedeny v souboru winerror.h hello a můžete také zobrazit na [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).</span><span class="sxs-lookup"><span data-stu-id="c8d17-126">hello errorlevel returned by *AppCmd.exe* are listed in hello winerror.h file, and can also be seen on [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).</span></span>

### <a name="example-of-managing-hello-error-level"></a><span data-ttu-id="c8d17-127">Příklad správy úroveň chyb hello</span><span class="sxs-lookup"><span data-stu-id="c8d17-127">Example of managing hello error level</span></span>
<span data-ttu-id="c8d17-128">Tento příklad přidá části komprese a položku komprese pro JSON toohello *Web.config* soubor s chybou zpracování a protokolování.</span><span class="sxs-lookup"><span data-stu-id="c8d17-128">This example adds a compression section and a compression entry for JSON toohello *Web.config* file, with error handling and logging.</span></span>

<span data-ttu-id="c8d17-129">Hello odpovídající části hello [ServiceDefinition.csdef] souboru zde se zobrazují, mezi které patří nastavení hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) atribut příliš`elevated` toogive *AppCmd.exe*  nastavení hello toochange dostatečná oprávnění v hello *Web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="c8d17-129">hello relevant sections of hello [ServiceDefinition.csdef] file are shown here, which include setting hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) attribute too`elevated` toogive *AppCmd.exe* sufficient permissions toochange hello settings in hello *Web.config* file:</span></span>

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

<span data-ttu-id="c8d17-130">Hello *Startup.cmd* batch používá soubor *AppCmd.exe* tooadd části komprese a položku komprese pro JSON toohello *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-130">hello *Startup.cmd* batch file uses *AppCmd.exe* tooadd a compression section and a compression entry for JSON toohello *Web.config* file.</span></span> <span data-ttu-id="c8d17-131">Hello očekává **errorlevel** z 183 nastavena toozero pomocí hello ověřit. EXE příkazového řádku programu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-131">hello expected **errorlevel** of 183 is set toozero using hello VERIFY.EXE command-line program.</span></span> <span data-ttu-id="c8d17-132">Neočekávané hodnoty parametru ERRORLEVEL při jsou zaznamenané tooStartupErrorLog.txt.</span><span class="sxs-lookup"><span data-stu-id="c8d17-132">Unexpected errorlevels are logged tooStartupErrorLog.txt.</span></span>

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

## <a name="add-firewall-rules"></a><span data-ttu-id="c8d17-133">Přidat pravidla brány firewall</span><span class="sxs-lookup"><span data-stu-id="c8d17-133">Add firewall rules</span></span>
<span data-ttu-id="c8d17-134">V Azure jsou efektivně dvě brány firewall.</span><span class="sxs-lookup"><span data-stu-id="c8d17-134">In Azure, there are effectively two firewalls.</span></span> <span data-ttu-id="c8d17-135">první brána firewall Hello řídí připojení mezi virtuálním počítačem hello a hello mimo world.</span><span class="sxs-lookup"><span data-stu-id="c8d17-135">hello first firewall controls connections between hello virtual machine and hello outside world.</span></span> <span data-ttu-id="c8d17-136">Tato brána firewall řídí hello [koncové body] element v hello [ServiceDefinition.csdef] souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-136">This firewall is controlled by hello [EndPoints] element in hello [ServiceDefinition.csdef] file.</span></span>

<span data-ttu-id="c8d17-137">Druhá brána firewall Hello řídí připojení mezi hello virtuálního počítače a procesy hello v rámci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8d17-137">hello second firewall controls connections between hello virtual machine and hello processes within that virtual machine.</span></span> <span data-ttu-id="c8d17-138">Tato brána firewall může být řízena hello `netsh advfirewall firewall` nástroj příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="c8d17-138">This firewall can be controlled by hello `netsh advfirewall firewall` command-line tool.</span></span>

<span data-ttu-id="c8d17-139">Azure vytvoří pravidla brány firewall pro hello procesy spuštěné v rámci role.</span><span class="sxs-lookup"><span data-stu-id="c8d17-139">Azure creates firewall rules for hello processes started within your roles.</span></span> <span data-ttu-id="c8d17-140">Například při spuštění služby nebo aplikace, Azure automaticky vytvoří tooallow pravidla brány firewall nezbytné hello této služby toocommunicate s hello Internetu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-140">For example, when you start a service or program, Azure automatically creates hello necessary firewall rules tooallow that service toocommunicate with hello Internet.</span></span> <span data-ttu-id="c8d17-141">Ale pokud vytvoříte službu, která se spustí proces mimo vaši roli (např. Windows naplánované úlohy nebo služby COM +), musíte toomanually vytvoření služby toothat přístup tooallow pravidlo brány firewall.</span><span class="sxs-lookup"><span data-stu-id="c8d17-141">However, if you create a service that is started by a process outside your role (like a COM+ service or a Windows Scheduled Task), you need toomanually create a firewall rule tooallow access toothat service.</span></span> <span data-ttu-id="c8d17-142">Tato pravidla brány firewall můžete vytvořit pomocí úloha spuštění.</span><span class="sxs-lookup"><span data-stu-id="c8d17-142">These firewall rules can be created by using a startup task.</span></span>

<span data-ttu-id="c8d17-143">Úloha spuštění, který vytvoří pravidlo brány firewall musí mít [executionContext][úloh] z **zvýšenými**.</span><span class="sxs-lookup"><span data-stu-id="c8d17-143">A startup task that creates a firewall rule must have an [executionContext][Task] of **elevated**.</span></span> <span data-ttu-id="c8d17-144">Přidejte následující spuštění úloh toohello hello [ServiceDefinition.csdef] souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-144">Add hello following startup task toohello [ServiceDefinition.csdef] file.</span></span>

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

<span data-ttu-id="c8d17-145">pravidlo brány firewall hello tooadd, je nutné použít hello odpovídající `netsh advfirewall firewall` příkazy do spuštění dávkového souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-145">tooadd hello firewall rule, you must use hello appropriate `netsh advfirewall firewall` commands in your startup batch file.</span></span> <span data-ttu-id="c8d17-146">V tomto příkladu hello spuštění úloha vyžaduje zabezpečení a šifrování pro TCP port 80.</span><span class="sxs-lookup"><span data-stu-id="c8d17-146">In this example, hello startup task requires security and encryption for TCP port 80.</span></span>

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a><span data-ttu-id="c8d17-147">Blokovat konkrétní IP adresu</span><span class="sxs-lookup"><span data-stu-id="c8d17-147">Block a specific IP address</span></span>
<span data-ttu-id="c8d17-148">Sadu Azure webové role přístup tooa ze zadaných IP adres můžete omezit změnou vaší služby IIS **web.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-148">You can restrict an Azure web role access tooa set of specified IP addresses by modifying your IIS **web.config** file.</span></span> <span data-ttu-id="c8d17-149">Musíte taky toouse soubor příkazů, které odemyká hello **ipSecurity** části hello **ApplicationHost.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-149">You also need toouse a command file which unlocks hello **ipSecurity** section of hello **ApplicationHost.config** file.</span></span>

<span data-ttu-id="c8d17-150">toodo odemknutí hello **ipSecurity** části hello **ApplicationHost.config** souboru, vytvořte soubor příkaz, který se spustí na spuštění role.</span><span class="sxs-lookup"><span data-stu-id="c8d17-150">toodo unlock hello **ipSecurity** section of hello **ApplicationHost.config** file, create a command file that runs at role start.</span></span> <span data-ttu-id="c8d17-151">Vytvořte složku na kořenové úrovni hello webové role názvem **spuštění** a v této složce vytvořte dávkový soubor s názvem **startup.cmd**.</span><span class="sxs-lookup"><span data-stu-id="c8d17-151">Create a folder at hello root level of your web role called **startup** and, within this folder, create a batch file called **startup.cmd**.</span></span> <span data-ttu-id="c8d17-152">Přidejte tento projekt sady Visual Studio tooyour souborů a nastavte vlastnosti hello příliš**kopie vždy** tooensure, která je součástí vašeho balíčku.</span><span class="sxs-lookup"><span data-stu-id="c8d17-152">Add this file tooyour Visual Studio project and set hello properties too**Copy Always** tooensure that it is included in your package.</span></span>

<span data-ttu-id="c8d17-153">Přidejte následující spuštění úloh toohello hello [ServiceDefinition.csdef] souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-153">Add hello following startup task toohello [ServiceDefinition.csdef] file.</span></span>

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

<span data-ttu-id="c8d17-154">Přidejte tento příkaz toohello **startup.cmd** souboru:</span><span class="sxs-lookup"><span data-stu-id="c8d17-154">Add this command toohello **startup.cmd** file:</span></span>

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

<span data-ttu-id="c8d17-155">Tato úloha způsobí, že hello **startup.cmd** dávky toobe soubor spustit pokaždé, když je inicializovat hello webovou roli, zajistit, že hello požadované **ipSecurity** pořizování.</span><span class="sxs-lookup"><span data-stu-id="c8d17-155">This task causes hello **startup.cmd** batch file toobe run every time hello web role is initialized, ensuring that hello required **ipSecurity** section is unlocked.</span></span>

<span data-ttu-id="c8d17-156">Nakonec upravte hello [oddíl system.webServer](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) webovou roli **web.config** souboru tooadd seznam IP adres, které mají udělen přístup, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c8d17-156">Finally, modify hello [system.webServer section](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) your web role’s **web.config** file tooadd a list of IP addresses that are granted access, as shown in hello following example:</span></span>

<span data-ttu-id="c8d17-157">Tato ukázka konfigurace **umožňuje** všechny IP adresy tooaccess hello serveru s výjimkou dva definované hello</span><span class="sxs-lookup"><span data-stu-id="c8d17-157">This sample config **allows** all IPs tooaccess hello server except hello two defined</span></span>

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

<span data-ttu-id="c8d17-158">Tato ukázka konfigurace **odmítne** všechny IP adresy v přístupu k serveru hello s výjimkou hello dva definované.</span><span class="sxs-lookup"><span data-stu-id="c8d17-158">This sample config **denies** all IPs from accessing hello server except for hello two defined.</span></span>

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

## <a name="create-a-powershell-startup-task"></a><span data-ttu-id="c8d17-159">Vytvoření úlohy spuštění prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8d17-159">Create a PowerShell startup task</span></span>
<span data-ttu-id="c8d17-160">Skripty prostředí Windows PowerShell nelze volat přímo z hello [ServiceDefinition.csdef] souboru, ale můžete vyvolat z v dávkovém souboru spuštění.</span><span class="sxs-lookup"><span data-stu-id="c8d17-160">Windows PowerShell scripts cannot be called directly from hello [ServiceDefinition.csdef] file, but they can be invoked from within a startup batch file.</span></span>

<span data-ttu-id="c8d17-161">Prostředí PowerShell (ve výchozím nastavení) nelze spustit nepodepsané skripty.</span><span class="sxs-lookup"><span data-stu-id="c8d17-161">PowerShell (by default) does not run unsigned scripts.</span></span> <span data-ttu-id="c8d17-162">Pokud váš skript, je nutné tooconfigure prostředí PowerShell toorun nepodepsané skripty.</span><span class="sxs-lookup"><span data-stu-id="c8d17-162">Unless you sign your script, you need tooconfigure PowerShell toorun unsigned scripts.</span></span> <span data-ttu-id="c8d17-163">toorun nepodepsané skripty, hello **ExecutionPolicy** musí být nastaven příliš**bez omezení**.</span><span class="sxs-lookup"><span data-stu-id="c8d17-163">toorun unsigned scripts, hello **ExecutionPolicy** must be set too**Unrestricted**.</span></span> <span data-ttu-id="c8d17-164">Hello **ExecutionPolicy** nastavení použití vychází hello verzi prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c8d17-164">hello **ExecutionPolicy** setting that you use is based on hello version of Windows PowerShell.</span></span>

```cmd
REM   Run an unsigned PowerShell script and log hello output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

<span data-ttu-id="c8d17-165">Pokud používáte hostovaného operačního systému, který je běží PowerShell 2.0 nebo 1.0 můžete vynutit toorun verze 2 a pokud není k dispozici, použijte verze 1.</span><span class="sxs-lookup"><span data-stu-id="c8d17-165">If you're using a Guest OS that is runs PowerShell 2.0 or 1.0 you can force version 2 toorun, and if unavailable, use version 1.</span></span>

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

## <a name="create-files-in-local-storage-from-a-startup-task"></a><span data-ttu-id="c8d17-166">Vytvoření souborů v místním úložišti z spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="c8d17-166">Create files in local storage from a startup task</span></span>
<span data-ttu-id="c8d17-167">Můžete použít toostore prostředků místní úložiště souborů vytvořených vaše úloha spuštění, které je přístupné později vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="c8d17-167">You can use a local storage resource toostore files created by your startup task that is accessed later by your application.</span></span>

<span data-ttu-id="c8d17-168">toocreate hello prostředků místní úložiště, přidejte [LocalResources] části toohello [ServiceDefinition.csdef] souboru a poté přidejte hello [LocalStorage] podřízený element.</span><span class="sxs-lookup"><span data-stu-id="c8d17-168">toocreate hello local storage resource, add a [LocalResources] section toohello [ServiceDefinition.csdef] file and then add hello [LocalStorage] child element.</span></span> <span data-ttu-id="c8d17-169">Poskytují hello místní úložiště prostředků jedinečný název a odpovídající velikost pro spuštění úkolu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-169">Give hello local storage resource a unique name and an appropriate size for your startup task.</span></span>

<span data-ttu-id="c8d17-170">toouse místní úložiště prostředků v vaše úloha spuštění, budete potřebovat toocreate z prostředí proměnné tooreference hello místní úložiště prostředků umístění.</span><span class="sxs-lookup"><span data-stu-id="c8d17-170">toouse a local storage resource in your startup task, you need toocreate an environment variable tooreference hello local storage resource location.</span></span> <span data-ttu-id="c8d17-171">Potom hello spuštění úlohy a aplikace hello jsou možné tooread a zapisovat soubory toohello místní úložiště prostředků.</span><span class="sxs-lookup"><span data-stu-id="c8d17-171">Then hello startup task and hello application are able tooread and write files toohello local storage resource.</span></span>

<span data-ttu-id="c8d17-172">Hello odpovídající části hello **ServiceDefinition.csdef** zde se zobrazují souboru:</span><span class="sxs-lookup"><span data-stu-id="c8d17-172">hello relevant sections of hello **ServiceDefinition.csdef** file are shown here:</span></span>

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

<span data-ttu-id="c8d17-173">Jako příklad to **Startup.cmd** dávkový soubor používá hello **PathToStartupStorage** prostředí proměnné toocreate hello souboru **MyTest.txt** v místním úložišti hello umístění.</span><span class="sxs-lookup"><span data-stu-id="c8d17-173">As an example, this **Startup.cmd** batch file uses hello **PathToStartupStorage** environment variable toocreate hello file **MyTest.txt** on hello local storage location.</span></span>

```cmd
REM   Create a simple text file.

ECHO This text will go into hello MyTest.txt file which will be in hello    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed tooby hello PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO hello contents of hello PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit hello batch file with ERRORLEVEL 0.

EXIT /b 0
```

<span data-ttu-id="c8d17-174">Místní úložiště složky můžete přístup z hello Azure SDK pomocí hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metoda.</span><span class="sxs-lookup"><span data-stu-id="c8d17-174">You can access local storage folder from hello Azure SDK by using hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-hello-emulator-or-cloud"></a><span data-ttu-id="c8d17-175">Spustit v emulátoru hello nebo v cloudu</span><span class="sxs-lookup"><span data-stu-id="c8d17-175">Run in hello emulator or cloud</span></span>
<span data-ttu-id="c8d17-176">Může mít vaše úloha spuštění provádět různé kroky, když ho pracuje v cloudu porovnání toowhen hello, je v emulátoru služby výpočty hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-176">You can have your startup task perform different steps when it is operating in hello cloud compared toowhen it is in hello compute emulator.</span></span> <span data-ttu-id="c8d17-177">Můžete například toouse novou kopii dat SQL jenom v případě, že je spuštěna v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-177">For example, you may want toouse a fresh copy of your SQL data only when running in hello emulator.</span></span> <span data-ttu-id="c8d17-178">Nebo můžete chtít toodo některých optimalizací výkonu pro cloud hello, nepotřebujete toodo při spuštění v emulátoru hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-178">Or you may want toodo some performance optimizations for hello cloud that you don't need toodo when running in hello emulator.</span></span>

<span data-ttu-id="c8d17-179">Tato možnost tooperform různé akce na hello emulátor služby výpočty a hello cloudu, můžete provést vytvoření proměnné prostředí v hello [ServiceDefinition.csdef] souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-179">This ability tooperform different actions on hello compute emulator and hello cloud can be accomplished by creating an environment variable in hello [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="c8d17-180">Potom otestovat tuto proměnnou prostředí pro hodnotu ve spuštění úkolu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-180">You then test that environment variable for a value in your startup task.</span></span>

<span data-ttu-id="c8d17-181">proměnné prostředí hello toocreate, přidat hello [proměnná]/[RoleInstanceValue] element a vytvořit má hodnotu XPath `/RoleEnvironment/Deployment/@emulated`.</span><span class="sxs-lookup"><span data-stu-id="c8d17-181">toocreate hello environment variable, add hello [Variable]/[RoleInstanceValue] element and create an XPath value of `/RoleEnvironment/Deployment/@emulated`.</span></span> <span data-ttu-id="c8d17-182">Hello hodnotu hello **ComputeEmulatorRunning %** proměnná prostředí je `true` při spuštění v emulátoru služby výpočty hello, a `false` při spuštění v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-182">hello value of hello **%ComputeEmulatorRunning%** environment variable is `true` when running on hello compute emulator, and `false` when running on hello cloud.</span></span>

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

<span data-ttu-id="c8d17-183">Hello úloh můžete zkontrolovat nyní hello **ComputeEmulatorRunning %** prostředí proměnné tooperform různé akce založené na tom, jestli se hello role běží v hello cloudu nebo hello emulátor.</span><span class="sxs-lookup"><span data-stu-id="c8d17-183">hello task can now check hello **%ComputeEmulatorRunning%** environment variable tooperform different actions based on whether hello role is running in hello cloud or hello emulator.</span></span> <span data-ttu-id="c8d17-184">Zde je skript prostředí .cmd, který kontroluje pro tuto proměnnou prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8d17-184">Here is a .cmd shell script that checks for that environment variable.</span></span>

```cmd
REM   Check if this task is running on hello compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on hello compute emulator. Perform tasks that must be run only in hello compute emulator.
) ELSE (
    REM   This task is running on hello cloud. Perform tasks that must be run only in hello cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a><span data-ttu-id="c8d17-185">Zjistit, že úlohu již spuštěn</span><span class="sxs-lookup"><span data-stu-id="c8d17-185">Detect that your task has already run</span></span>
<span data-ttu-id="c8d17-186">Hello role může recyklujte bez restartování způsobuje vaší toorun spuštění úlohy znovu.</span><span class="sxs-lookup"><span data-stu-id="c8d17-186">hello role may recycle without a reboot causing your startup tasks toorun again.</span></span> <span data-ttu-id="c8d17-187">Neexistuje žádné tooindicate příznak, který úlohu byl již spuštěn hello hostování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c8d17-187">There is no flag tooindicate that a task has already run on hello hosting VM.</span></span> <span data-ttu-id="c8d17-188">Může mít některé úlohy, kde nezáleží na spuštění více než jednou.</span><span class="sxs-lookup"><span data-stu-id="c8d17-188">You may have some tasks where it doesn't matter that they run multiple times.</span></span> <span data-ttu-id="c8d17-189">Však může spustit do situace, kdy potřebujete tooprevent úlohu ve spuštění více než jednou.</span><span class="sxs-lookup"><span data-stu-id="c8d17-189">However, you may run into a situation where you need tooprevent a task from running more than once.</span></span>

<span data-ttu-id="c8d17-190">Hello nejjednodušší způsob, jak toodetect, již byla spuštěna úloha je toocreate soubor v hello **% TEMP %** složku Po úspěšné hello úloh a podívejte se na hello spuštění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-190">hello simplest way toodetect that a task has already run is toocreate a file in hello **%TEMP%** folder when hello task is successful and look for it at hello start of hello task.</span></span> <span data-ttu-id="c8d17-191">Tady je ukázkový skript prostředí cmd, který pro sebe.</span><span class="sxs-lookup"><span data-stu-id="c8d17-191">Here is a sample cmd shell script that does that for you.</span></span>

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

## <a name="task-best-practices"></a><span data-ttu-id="c8d17-192">Osvědčené postupy úloh</span><span class="sxs-lookup"><span data-stu-id="c8d17-192">Task best practices</span></span>
<span data-ttu-id="c8d17-193">Tady jsou některé z osvědčených postupů, které byste měli postupovat při konfiguraci úloh pro váš web nebo worker roli.</span><span class="sxs-lookup"><span data-stu-id="c8d17-193">Here are some best practices you should follow when configuring task for your web or worker role.</span></span>

### <a name="always-log-startup-activities"></a><span data-ttu-id="c8d17-194">Vždy protokolu spuštění aktivit</span><span class="sxs-lookup"><span data-stu-id="c8d17-194">Always log startup activities</span></span>
<span data-ttu-id="c8d17-195">Visual Studio neposkytuje toostep ladicí program prostřednictvím dávkové soubory, proto je dobré tooget tolik dat na hello operace dávkové soubory míře.</span><span class="sxs-lookup"><span data-stu-id="c8d17-195">Visual Studio does not provide a debugger toostep through batch files, so it's good tooget as much data on hello operation of batch files as possible.</span></span> <span data-ttu-id="c8d17-196">Protokolování výstupu hello dávkové soubory i **stdout** a **stderr**, můžete poskytnout důležité informace při pokusu o toodebug a opravte dávkové soubory.</span><span class="sxs-lookup"><span data-stu-id="c8d17-196">Logging hello output of batch files, both **stdout** and **stderr**, can give you important information when trying toodebug and fix batch files.</span></span> <span data-ttu-id="c8d17-197">toolog obou **stdout** a **stderr** toohello StartupLog.txt souboru v hello directory odkazováno tooby hello **% TEMP %** proměnné prostředí, přidejte hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1`toohello konec konkrétní řádků je vhodné toolog.</span><span class="sxs-lookup"><span data-stu-id="c8d17-197">toolog both **stdout** and **stderr** toohello StartupLog.txt file in hello directory pointed tooby hello **%TEMP%** environment variable, add hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1` toohello end of specific lines you want toolog.</span></span> <span data-ttu-id="c8d17-198">Například tooexecute setup.exe v hello **PathToApp1Install %** directory:</span><span class="sxs-lookup"><span data-stu-id="c8d17-198">For example, tooexecute setup.exe in hello **%PathToApp1Install%** directory:</span></span>

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

<span data-ttu-id="c8d17-199">toosimplify soubor xml, můžete vytvořit obálku *cmd* soubor, který volá všechny vaše spuštění úloh společně s protokolování a zajistí každé sdílené složky hello podřízené úlohy stejné proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="c8d17-199">toosimplify your xml, you can create a wrapper *cmd* file that calls all of your startup tasks along with logging and ensures each child-task shares hello same environment variables.</span></span>

<span data-ttu-id="c8d17-200">Možná bude ale obtěžování toouse `>> "%TEMP%\StartupLog.txt" 2>&1` na konci hello každé spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="c8d17-200">You may find it annoying though toouse `>> "%TEMP%\StartupLog.txt" 2>&1` on hello end of each startup task.</span></span> <span data-ttu-id="c8d17-201">Protokolování úloh můžete vynutit tím, že vytvoříte obálku, která zpracovává protokolování pro vás.</span><span class="sxs-lookup"><span data-stu-id="c8d17-201">You can enforce task logging by creating a wrapper that handles logging for you.</span></span> <span data-ttu-id="c8d17-202">Tuto obálku volá hello skutečné dávkového souboru chcete toorun.</span><span class="sxs-lookup"><span data-stu-id="c8d17-202">This wrapper calls hello real batch file you want toorun.</span></span> <span data-ttu-id="c8d17-203">Všechny výstupy z hello cíl dávkový soubor bude přesměrované toohello *Startuplog.txt* souboru.</span><span class="sxs-lookup"><span data-stu-id="c8d17-203">Any output from hello target batch file will be redirected toohello *Startuplog.txt* file.</span></span>

<span data-ttu-id="c8d17-204">Následující příklad ukazuje, jak tooredirect všechny výstup z dávkového souboru spuštění Hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-204">hello following example shows how tooredirect all output from a startup batch file.</span></span> <span data-ttu-id="c8d17-205">V tomto příkladu vytvoří soubor ServerDefinition.csdef hello spuštění úlohu, která volá *logwrap.cmd*.</span><span class="sxs-lookup"><span data-stu-id="c8d17-205">In this example, hello ServerDefinition.csdef file creates a startup task that calls *logwrap.cmd*.</span></span> <span data-ttu-id="c8d17-206">*logwrap.cmd* volání *Startup2.cmd*, přesměrování veškerý výstup příliš**% TEMP %\\StartupLog.txt**.</span><span class="sxs-lookup"><span data-stu-id="c8d17-206">*logwrap.cmd* calls *Startup2.cmd*, redirecting all output too**%TEMP%\\StartupLog.txt**.</span></span>

<span data-ttu-id="c8d17-207">ServiceDefinition.cmd:</span><span class="sxs-lookup"><span data-stu-id="c8d17-207">ServiceDefinition.cmd:</span></span>

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

<span data-ttu-id="c8d17-208">**logwrap.cmd:**</span><span class="sxs-lookup"><span data-stu-id="c8d17-208">**logwrap.cmd:**</span></span>

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

<span data-ttu-id="c8d17-209">**Startup2.cmd:**</span><span class="sxs-lookup"><span data-stu-id="c8d17-209">**Startup2.cmd:**</span></span>

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

<span data-ttu-id="c8d17-210">Ukázka výstupu v hello **StartupLog.txt** souboru:</span><span class="sxs-lookup"><span data-stu-id="c8d17-210">Sample output in hello **StartupLog.txt** file:</span></span>

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> <span data-ttu-id="c8d17-211">Hello **StartupLog.txt** soubor je umístěný ve hello *C:\Resources\temp\\{identifikátor role} \RoleTemp* složky.</span><span class="sxs-lookup"><span data-stu-id="c8d17-211">hello **StartupLog.txt** file is located in hello *C:\Resources\temp\\{role identifier}\RoleTemp* folder.</span></span>
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a><span data-ttu-id="c8d17-212">Nastavit executionContext pro spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="c8d17-212">Set executionContext appropriately for startup tasks</span></span>
<span data-ttu-id="c8d17-213">Nastavte oprávnění pro spuštění úloh hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-213">Set privileges appropriately for hello startup task.</span></span> <span data-ttu-id="c8d17-214">Spuštění úlohy někdy musí spusťte s vyššími oprávněními, i když hello role běží s normálními oprávněními.</span><span class="sxs-lookup"><span data-stu-id="c8d17-214">Sometimes startup tasks must run with elevated privileges even though hello role runs with normal privileges.</span></span>

<span data-ttu-id="c8d17-215">Hello [executionContext][úloh] atribut nastaví úroveň oprávnění hello hello spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="c8d17-215">hello [executionContext][Task] attribute sets hello privilege level of hello startup task.</span></span> <span data-ttu-id="c8d17-216">Pomocí `executionContext="limited"` znamená má úloha spuštění hello hello stejnou úroveň oprávnění jako hello role.</span><span class="sxs-lookup"><span data-stu-id="c8d17-216">Using `executionContext="limited"` means hello startup task has hello same privilege level as hello role.</span></span> <span data-ttu-id="c8d17-217">Pomocí `executionContext="elevated"` znamená úloha spuštění hello má oprávnění správce, který hello spuštění umožňuje úkolů tooperform Správce úloh bez nutnosti poskytnutí oprávnění tooyour role správce.</span><span class="sxs-lookup"><span data-stu-id="c8d17-217">Using `executionContext="elevated"` means hello startup task has administrator privileges, which allows hello startup task tooperform administrator tasks without giving administrator privileges tooyour role.</span></span>

<span data-ttu-id="c8d17-218">Úloha spuštění, který používá je například spuštění úlohu, která vyžaduje zvýšená oprávnění **AppCmd.exe** tooconfigure služby IIS.</span><span class="sxs-lookup"><span data-stu-id="c8d17-218">An example of a startup task that requires elevated privileges is a startup task that uses **AppCmd.exe** tooconfigure IIS.</span></span> <span data-ttu-id="c8d17-219">**AppCmd.exe** vyžaduje `executionContext="elevated"`.</span><span class="sxs-lookup"><span data-stu-id="c8d17-219">**AppCmd.exe** requires `executionContext="elevated"`.</span></span>

### <a name="use-hello-appropriate-tasktype"></a><span data-ttu-id="c8d17-220">Použít příslušné taskType hello</span><span class="sxs-lookup"><span data-stu-id="c8d17-220">Use hello appropriate taskType</span></span>
<span data-ttu-id="c8d17-221">Hello [taskType][úloh] atribut určuje hello způsob hello spuštění úlohy je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="c8d17-221">hello [taskType][Task] attribute determines hello way hello startup task is executed.</span></span> <span data-ttu-id="c8d17-222">Existují tři hodnoty: **jednoduché**, **pozadí**, a **popředí**.</span><span class="sxs-lookup"><span data-stu-id="c8d17-222">There are three values: **simple**, **background**, and **foreground**.</span></span> <span data-ttu-id="c8d17-223">Hello popředí a na pozadí úlohy se spouštějí asynchronně, a potom synchronně spuštěn hello jednoduché úlohy po jednom.</span><span class="sxs-lookup"><span data-stu-id="c8d17-223">hello background and foreground tasks are started asynchronously, and then hello simple tasks are executed synchronously one at a time.</span></span>

<span data-ttu-id="c8d17-224">S **jednoduché** spuštění úlohy, můžete nastavit hello pořadí, ve kterém hello úlohy spouštět podle hello pořadí, ve které hello úlohy jsou uvedeny v souboru ServiceDefinition.csdef hello.</span><span class="sxs-lookup"><span data-stu-id="c8d17-224">With **simple** startup tasks, you can set hello order in which hello tasks run by hello order in which hello tasks are listed in hello ServiceDefinition.csdef file.</span></span> <span data-ttu-id="c8d17-225">Pokud **jednoduché** úloh na konci nenulový ukončovací kód a potom hello spuštění procedury zastaví a hello role se nespustí.</span><span class="sxs-lookup"><span data-stu-id="c8d17-225">If a **simple** task ends with a non-zero exit code, then hello startup procedure stops and hello role does not start.</span></span>

<span data-ttu-id="c8d17-226">Hello rozdíl mezi **pozadí** spuštění úlohy a **popředí** spuštění úlohy je, že **popředí** úlohy zachovat hello role spuštění, dokud hello  **popředí** úkolů elementy end.</span><span class="sxs-lookup"><span data-stu-id="c8d17-226">hello difference between **background** startup tasks and **foreground** startup tasks is that **foreground** tasks keep hello role running until hello **foreground** task ends.</span></span> <span data-ttu-id="c8d17-227">To také znamená, že pokud hello **popředí** úloh přestane reagovat nebo dojde k chybě, dokud hello neprovede recyklování hello role **popředí** úloh je nucen uzavřen.</span><span class="sxs-lookup"><span data-stu-id="c8d17-227">This also means that if hello **foreground** task hangs or crashes, hello role will not recycle until hello **foreground** task is forced closed.</span></span> <span data-ttu-id="c8d17-228">Z tohoto důvodu **pozadí** úlohy se doporučují pro asynchronní spuštění úloh, pokud je třeba tuto funkci hello **popředí** úloh.</span><span class="sxs-lookup"><span data-stu-id="c8d17-228">For this reason, **background** tasks are recommended for asynchronous startup tasks unless you need that feature of hello **foreground** task.</span></span>

### <a name="end-batch-files-with-exit-b-0"></a><span data-ttu-id="c8d17-229">End dávkové soubory s UKONČOVACÍM /B 0</span><span class="sxs-lookup"><span data-stu-id="c8d17-229">End batch files with EXIT /B 0</span></span>
<span data-ttu-id="c8d17-230">Hello role začne Pokud hello **errorlevel** z každé vaší jednoduché spuštění úloh je nulová.</span><span class="sxs-lookup"><span data-stu-id="c8d17-230">hello role will only start if hello **errorlevel** from each of your simple startup task is zero.</span></span> <span data-ttu-id="c8d17-231">Ne všechny programy nastavit hello **errorlevel** (ukončovací kód) správně, takže hello dávkový soubor musí končit `EXIT /B 0` Pokud se vše správně spustil.</span><span class="sxs-lookup"><span data-stu-id="c8d17-231">Not all programs set hello **errorlevel** (exit code) correctly, so hello batch file should end with an `EXIT /B 0` if everything ran correctly.</span></span>

<span data-ttu-id="c8d17-232">Chybějící `EXIT /B 0` v hello konce souboru spuštění dávky je obvyklou příčinou rolí, které nebudou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="c8d17-232">A missing `EXIT /B 0` at hello end of a startup batch file is a common cause of roles that do not start.</span></span>

> [!NOTE]
> <span data-ttu-id="c8d17-233">I jste si všimli této vnořené dávkové soubory někdy zablokování, když pomocí hello `/B` parametr.</span><span class="sxs-lookup"><span data-stu-id="c8d17-233">I've noticed that nested batch files sometimes hang when using hello `/B` parameter.</span></span> <span data-ttu-id="c8d17-234">Můžete chtít, aby tento problém zablokování neodehrává volá-li jiný dávkový soubor váš aktuální dávkový soubor, třeba Pokud použijete hello toomake [obálku protokolu](#always-log-startup-activities).</span><span class="sxs-lookup"><span data-stu-id="c8d17-234">You may want toomake sure that this hang problem does not happen if another batch file calls your current batch file, like if you use hello [log wrapper](#always-log-startup-activities).</span></span> <span data-ttu-id="c8d17-235">Je možné vynechat hello `/B` parametr v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="c8d17-235">You can omit hello `/B` parameter in this case.</span></span>
> 
> 

### <a name="expect-startup-tasks-toorun-more-than-once"></a><span data-ttu-id="c8d17-236">Spuštění úlohy toorun očekávat více než jednou.</span><span class="sxs-lookup"><span data-stu-id="c8d17-236">Expect startup tasks toorun more than once</span></span>
<span data-ttu-id="c8d17-237">Ne všechny role recykluje zahrnovat restart, ale všechny role recykluje zahrnují spuštění všech spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="c8d17-237">Not all role recycles include a reboot, but all role recycles include running all startup tasks.</span></span> <span data-ttu-id="c8d17-238">To znamená, že spuštění úlohy musí být schopný toorun vícekrát mezi jednotlivými restartováními bez problémů.</span><span class="sxs-lookup"><span data-stu-id="c8d17-238">This means that startup tasks must be able toorun multiple times between reboots without any problems.</span></span> <span data-ttu-id="c8d17-239">To je podrobněji hello [předcházející části](#detect-that-your-task-has-already-run).</span><span class="sxs-lookup"><span data-stu-id="c8d17-239">This is discussed in hello [preceding section](#detect-that-your-task-has-already-run).</span></span>

### <a name="use-local-storage-toostore-files-that-must-be-accessed-in-hello-role"></a><span data-ttu-id="c8d17-240">Použít místní úložiště toostore souborů, které musí být přístupné v roli hello</span><span class="sxs-lookup"><span data-stu-id="c8d17-240">Use local storage toostore files that must be accessed in hello role</span></span>
<span data-ttu-id="c8d17-241">Chcete-li toocopy nebo vytvořte soubor během spuštění úkolu, je pak přístupné tooyour role a potom tento soubor musí být umístěn v místním úložišti.</span><span class="sxs-lookup"><span data-stu-id="c8d17-241">If you want toocopy or create a file during your startup task that is then accessible tooyour role, then that file must be placed in local storage.</span></span> <span data-ttu-id="c8d17-242">V tématu hello [předcházející části](#create-files-in-local-storage-from-a-startup-task).</span><span class="sxs-lookup"><span data-stu-id="c8d17-242">See hello [preceding section](#create-files-in-local-storage-from-a-startup-task).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8d17-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8d17-243">Next steps</span></span>
<span data-ttu-id="c8d17-244">Zkontrolujte hello cloudu [služby modelu a balíčku](cloud-services-model-and-package.md)</span><span class="sxs-lookup"><span data-stu-id="c8d17-244">Review hello cloud [service model and package](cloud-services-model-and-package.md)</span></span>

<span data-ttu-id="c8d17-245">Další informace o [úlohy](cloud-services-startup-tasks.md) fungovat.</span><span class="sxs-lookup"><span data-stu-id="c8d17-245">Learn more about how [Tasks](cloud-services-startup-tasks.md) work.</span></span>

<span data-ttu-id="c8d17-246">[Vytvoření a nasazení](cloud-services-how-to-create-deploy-portal.md) balíček vaše cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="c8d17-246">[Create and deploy](cloud-services-how-to-create-deploy-portal.md) your cloud service package.</span></span>

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
