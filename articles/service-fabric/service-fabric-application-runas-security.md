---
title: "aaaLearn o zásady zabezpečení Azure mikroslužeb | Microsoft Docs"
description: "Přehled o tom, jak toorun aplikace Service Fabric v rámci systému a účtů místní zabezpečení, včetně hello SetupEntry bodu, pokud aplikace potřebuje tooperform některé privilegované akce před spuštěním"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: f5afba69e09aa4f3c9efa4d3efc6995c813a1f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-security-policies-for-your-application"></a><span data-ttu-id="6e111-103">Konfigurace zásad zabezpečení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="6e111-103">Configure security policies for your application</span></span>
<span data-ttu-id="6e111-104">Pomocí Azure Service Fabric můžete zabezpečit aplikace, které jsou spuštěny v clusteru hello v rámci jiné uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="6e111-104">By using Azure Service Fabric, you can secure applications that are running in hello cluster under different user accounts.</span></span> <span data-ttu-id="6e111-105">Service Fabric také pomáhá zabezpečené hello prostředky, které jsou používány aplikací v době hello nasazení pod hello uživatelských účtů – například, soubory, adresářů a certifikáty.</span><span class="sxs-lookup"><span data-stu-id="6e111-105">Service Fabric also helps secure hello resources that are used by applications at hello time of deployment under hello user accounts--for example, files, directories, and certificates.</span></span> <span data-ttu-id="6e111-106">Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="6e111-106">This makes running applications, even in a shared hosted environment, more secure from one another.</span></span>

<span data-ttu-id="6e111-107">Ve výchozím nastavení aplikace Service Fabric běžet pod účtem hello, kompatibilní se proces Fabric.exe hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-107">By default, Service Fabric applications run under hello account that hello Fabric.exe process runs under.</span></span> <span data-ttu-id="6e111-108">Service Fabric také poskytuje hello možnost toorun aplikace pod účtem místního uživatelského účtu nebo účtu local system, který je určený v rámci hello manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e111-108">Service Fabric also provides hello capability toorun applications under a local user account or local system account, which is specified within hello application manifest.</span></span> <span data-ttu-id="6e111-109">Typy účtů podporovaný místní systém **LocalUser**, **NetworkService**, **LocalService**, a **LocalSystem**.</span><span class="sxs-lookup"><span data-stu-id="6e111-109">Supported local system account types are **LocalUser**, **NetworkService**, **LocalService**, and **LocalSystem**.</span></span>

 <span data-ttu-id="6e111-110">Když spouštíte Service Fabric na Windows serveru ve vašem datovém centru pomocí hello samostatný instalační program systému, můžete účty domény služby Active Directory, včetně skupinové účty spravované služby.</span><span class="sxs-lookup"><span data-stu-id="6e111-110">When you're running Service Fabric on Windows Server in your datacenter by using hello standalone installer, you can use Active Directory domain accounts, including group managed service accounts.</span></span>

<span data-ttu-id="6e111-111">Můžete definovat a vytvořit skupiny uživatelů, aby toobe tooeach skupiny spravují dohromady lze přidat jeden nebo více uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6e111-111">You can define and create user groups so that one or more users can be added tooeach group toobe managed together.</span></span> <span data-ttu-id="6e111-112">To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a potřebují toohave určité společné oprávnění, které jsou k dispozici na úrovni skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-112">This is useful when there are multiple users for different service entry points and they need toohave certain common privileges that are available at hello group level.</span></span>

## <a name="configure-hello-policy-for-a-service-setup-entry-point"></a><span data-ttu-id="6e111-113">Konfigurace zásad hello pro bod služby instalační položka</span><span class="sxs-lookup"><span data-stu-id="6e111-113">Configure hello policy for a service setup entry point</span></span>
<span data-ttu-id="6e111-114">Jak je popsáno v hello [aplikačního modelu](service-fabric-application-model.md), hello instalace vstupní bod, **SetupEntryPoint**, je privilegované vstupního bodu, který je spuštěn s hello stejné přihlašovací údaje jako Service Fabric (obvykle hello *NetworkService* účtu) před další vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="6e111-114">As described in hello [application model](service-fabric-application-model.md), hello setup entry point, **SetupEntryPoint**, is a privileged entry point that runs with hello same credentials as Service Fabric (typically hello *NetworkService* account) before any other entry point.</span></span> <span data-ttu-id="6e111-115">Hello spustitelný soubor, který je zadán **EntryPoint** je obvykle hostitele služby dlouho běžící hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-115">hello executable that is specified by **EntryPoint** is typically hello long-running service host.</span></span> <span data-ttu-id="6e111-116">Proto nutnosti samostatného instalačního vstupního bodu tomu není nutné hostitele služby toorun hello spustitelný soubor s vysokou úrovní oprávnění pro dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="6e111-116">So having a separate setup entry point avoids having toorun hello service host executable with high privileges for extended periods of time.</span></span> <span data-ttu-id="6e111-117">Hello spustitelný soubor, **EntryPoint** určuje běží **SetupEntryPoint** ukončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="6e111-117">hello executable that **EntryPoint** specifies is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="6e111-118">Hello výsledné proces monitorovat a restartuje a znovu začíná **SetupEntryPoint** Pokud někdy ukončí nebo dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="6e111-118">hello resulting process is monitored and restarted, and begins again with **SetupEntryPoint** if it ever terminates or crashes.</span></span>

<span data-ttu-id="6e111-119">Následující Hello je příklad manifestu jednoduché služby, ukazuje hello SetupEntryPoint a hello hlavní vstupní bod pro službu hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-119">hello following is a simple service manifest example that shows hello SetupEntryPoint and hello main EntryPoint for hello service.</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-hello-policy-by-using-a-local-account"></a><span data-ttu-id="6e111-120">Konfigurace zásad hello pomocí místního účtu</span><span class="sxs-lookup"><span data-stu-id="6e111-120">Configure hello policy by using a local account</span></span>
<span data-ttu-id="6e111-121">Po dokončení konfigurace služby toohave hello vstupní bod instalační program, můžete změnit hello oprávnění zabezpečení, které běží v části v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-121">After you configure hello service toohave a setup entry point, you can change hello security permissions that it runs under in hello application manifest.</span></span> <span data-ttu-id="6e111-122">Hello následující příklad ukazuje, jak tooconfigure hello toorun služby v části oprávnění uživatelského účtu správce.</span><span class="sxs-lookup"><span data-stu-id="6e111-122">hello following example shows how tooconfigure hello service toorun under user administrator account privileges.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

<span data-ttu-id="6e111-123">Nejprve vytvořte **objekty** část s uživatelským jménem, jako je například SetupAdminUser.</span><span class="sxs-lookup"><span data-stu-id="6e111-123">First, create a **Principals** section with a user name, such as SetupAdminUser.</span></span> <span data-ttu-id="6e111-124">To znamená, že tento hello uživatel je členem skupiny Administrators systému hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-124">This indicates that hello user is a member of hello Administrators system group.</span></span>

<span data-ttu-id="6e111-125">Dále v části hello **ServiceManifestImport** nakonfigurujte zásady tooapply tento objekt zabezpečení příliš**SetupEntryPoint**.</span><span class="sxs-lookup"><span data-stu-id="6e111-125">Next, under hello **ServiceManifestImport** section, configure a policy tooapply this principal too**SetupEntryPoint**.</span></span> <span data-ttu-id="6e111-126">Tato hodnota informuje Service Fabric, když hello **MySetup.bat** spuštění souboru, měla by být `RunAs` s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="6e111-126">This tells Service Fabric that when hello **MySetup.bat** file is run, it should be `RunAs` with administrator privileges.</span></span> <span data-ttu-id="6e111-127">Vzhledem k tomu, že máte *není* použít zásady toohello hlavní vstupní bod, hello kód v **MyServiceHost.exe** běží v rámci systému hello **NetworkService** účtu.</span><span class="sxs-lookup"><span data-stu-id="6e111-127">Given that you have *not* applied a policy toohello main entry point, hello code in **MyServiceHost.exe** runs under hello system **NetworkService** account.</span></span> <span data-ttu-id="6e111-128">Toto je hello výchozí účet, který všechny vstupní body služby jsou spustit jako.</span><span class="sxs-lookup"><span data-stu-id="6e111-128">This is hello default account that all service entry points are run as.</span></span>

<span data-ttu-id="6e111-129">Nyní Pojďme přidat hello souboru MySetup.bat toohello Visual Studio projektu tootest hello oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="6e111-129">Let's now add hello file MySetup.bat toohello Visual Studio project tootest hello administrator privileges.</span></span> <span data-ttu-id="6e111-130">V sadě Visual Studio klikněte pravým tlačítkem na projekt služby hello a přidejte nový soubor s názvem MySetup.bat.</span><span class="sxs-lookup"><span data-stu-id="6e111-130">In Visual Studio, right-click hello service project and add a new file called MySetup.bat.</span></span>

<span data-ttu-id="6e111-131">Dále zkontrolujte, zda že tento soubor MySetup.bat hello je součástí balíčku služby hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-131">Next, ensure that hello MySetup.bat file is included in hello service package.</span></span> <span data-ttu-id="6e111-132">Ve výchozím nastavení není.</span><span class="sxs-lookup"><span data-stu-id="6e111-132">By default, it is not.</span></span> <span data-ttu-id="6e111-133">Vyberte soubor hello, klikněte pravým tlačítkem na tooget hello kontextovou nabídku a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="6e111-133">Select hello file, right-click tooget hello context menu, and choose **Properties**.</span></span> <span data-ttu-id="6e111-134">V dialogové okno Vlastnosti hello, ověřte, že **zkopírujte tooOutput Directory** je nastaven příliš**kopírovat, pokud je novější**.</span><span class="sxs-lookup"><span data-stu-id="6e111-134">In hello Properties dialog box, ensure that **Copy tooOutput Directory** is set too**Copy if newer**.</span></span> <span data-ttu-id="6e111-135">V tématu hello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="6e111-135">See hello following screenshot.</span></span>

![Visual Studio CopyToOutput pro SetupEntryPoint dávkového souboru][image1]

<span data-ttu-id="6e111-137">Nyní otevřete soubor MySetup.bat hello a přidejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="6e111-137">Now open hello MySetup.bat file and add hello following commands:</span></span>

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set too> out.txt
echo %TestVariable% >> out.txt

REM toodelete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

<span data-ttu-id="6e111-138">V dalším kroku sestavení a nasazení hello řešení tooa místní vývojový cluster.</span><span class="sxs-lookup"><span data-stu-id="6e111-138">Next, build and deploy hello solution tooa local development cluster.</span></span> <span data-ttu-id="6e111-139">Po spuštění služby hello, jak je znázorněno v Service Fabric Exploreru, uvidíte, že tento soubor MySetup.bat hello proběhla úspěšně dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="6e111-139">After hello service has started, as shown in Service Fabric Explorer, you can see that hello MySetup.bat file was successful in a two ways.</span></span> <span data-ttu-id="6e111-140">Otevřete příkazový řádek prostředí PowerShell a zadejte:</span><span class="sxs-lookup"><span data-stu-id="6e111-140">Open a PowerShell command prompt and type:</span></span>

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

<span data-ttu-id="6e111-141">Poznamenejte si název hello hello uzlu, kde služba hello nasazená a spustit v Service Fabric Exploreru – například uzlu 2.</span><span class="sxs-lookup"><span data-stu-id="6e111-141">Then, note hello name of hello node where hello service was deployed and started in Service Fabric Explorer--for example, Node 2.</span></span> <span data-ttu-id="6e111-142">Dále přejděte toohello aplikace instance pracovní složky toofind hello out.txt soubor, který zobrazuje hodnotu hello **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="6e111-142">Next, navigate toohello application instance work folder toofind hello out.txt file that shows hello value of **TestVariable**.</span></span> <span data-ttu-id="6e111-143">Například pokud tato služba byla nasazená tooNode 2, potom můžete přejít toothis cestu pro hello **MyApplicationType**:</span><span class="sxs-lookup"><span data-stu-id="6e111-143">For example, if this service was deployed tooNode 2, then you can go toothis path for hello **MyApplicationType**:</span></span>

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-hello-policy-by-using-local-system-accounts"></a><span data-ttu-id="6e111-144">Konfigurace zásad hello pomocí místní systémové účty</span><span class="sxs-lookup"><span data-stu-id="6e111-144">Configure hello policy by using local system accounts</span></span>
<span data-ttu-id="6e111-145">Často je vhodnější toorun hello spouštěcí skript pomocí účtu local system, nikoli účet správce.</span><span class="sxs-lookup"><span data-stu-id="6e111-145">Often, it's preferable toorun hello startup script by using a local system account rather than an administrator account.</span></span> <span data-ttu-id="6e111-146">Spuštění zásad RunAs hello jako člen skupiny Administrators obvykle nefunguje správně, protože počítače mají přístup k řízení Uživatelských účtů ve výchozím nastavení povolené hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-146">Running hello RunAs policy as a member of hello Administrators group typically doesn’t work well because machines have User Access Control (UAC) enabled by default.</span></span> <span data-ttu-id="6e111-147">V takových případech **hello doporučení je toorun hello SetupEntryPoint pod účtem LocalSystem, místo jako skupinu místního uživatele přidané tooAdministrators**.</span><span class="sxs-lookup"><span data-stu-id="6e111-147">In such cases, **hello recommendation is toorun hello SetupEntryPoint as LocalSystem, instead of as a local user added tooAdministrators group**.</span></span> <span data-ttu-id="6e111-148">Hello následující příklad ukazuje nastavení hello SetupEntryPoint toorun pod účtem LocalSystem:</span><span class="sxs-lookup"><span data-stu-id="6e111-148">hello following example shows setting hello SetupEntryPoint toorun as LocalSystem:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

## <a name="start-powershell-commands-from-a-setup-entry-point"></a><span data-ttu-id="6e111-149">Spusťte příkazy prostředí PowerShell ze vstupního bodu instalační program</span><span class="sxs-lookup"><span data-stu-id="6e111-149">Start PowerShell commands from a setup entry point</span></span>
<span data-ttu-id="6e111-150">toorun prostředí PowerShell z hello **SetupEntryPoint** bodu, můžete spustit **PowerShell.exe** v dávkovém souboru, který odkazuje tooa prostředí PowerShell souboru.</span><span class="sxs-lookup"><span data-stu-id="6e111-150">toorun PowerShell from hello **SetupEntryPoint** point, you can run **PowerShell.exe** in a batch file that points tooa PowerShell file.</span></span> <span data-ttu-id="6e111-151">Nejprve přidejte projekt služba prostředí PowerShell souborů toohello – například **MySetup.ps1**.</span><span class="sxs-lookup"><span data-stu-id="6e111-151">First, add a PowerShell file toohello service project--for example, **MySetup.ps1**.</span></span> <span data-ttu-id="6e111-152">Mějte na paměti, tooset hello *kopírovat, pokud je novější* vlastnost tak, že soubor hello jsou také obsaženy v balíčku služby hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-152">Remember tooset hello *Copy if newer* property so that hello file is also included in hello service package.</span></span> <span data-ttu-id="6e111-153">Hello následující příklad ukazuje dávkový soubor začínající soubor prostředí PowerShell s názvem MySetup.ps1, která nastavuje proměnnou prostředí systému s názvem **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="6e111-153">hello following example shows a sample batch file that starts a PowerShell file called MySetup.ps1, which sets a system environment variable called **TestVariable**.</span></span>

<span data-ttu-id="6e111-154">MySetup.bat toostart soubor prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6e111-154">MySetup.bat toostart a PowerShell file:</span></span>

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

<span data-ttu-id="6e111-155">V souboru prostředí PowerShell text hello přidejte následující tooset proměnné prostředí systému hello:</span><span class="sxs-lookup"><span data-stu-id="6e111-155">In hello PowerShell file, add hello following tooset a system environment variable:</span></span>

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> <span data-ttu-id="6e111-156">Ve výchozím nastavení, když hello dávkový soubor spouští, vypadá na hello aplikace složku s názvem **pracovní** pro soubory.</span><span class="sxs-lookup"><span data-stu-id="6e111-156">By default, when hello batch file runs, it looks at hello application folder called **work** for files.</span></span> <span data-ttu-id="6e111-157">V takovém případě při spuštění MySetup.bat chceme, aby tento toofind hello MySetup.ps1 souboru v hello stejné složky, která je aplikace hello **balíček kódu** složky.</span><span class="sxs-lookup"><span data-stu-id="6e111-157">In this case, when MySetup.bat runs, we want this toofind hello MySetup.ps1 file in hello same folder, which is hello application **code package** folder.</span></span> <span data-ttu-id="6e111-158">toochange tuto složku a sadu hello pracovní složku:</span><span class="sxs-lookup"><span data-stu-id="6e111-158">toochange this folder, set hello working folder:</span></span>
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a><span data-ttu-id="6e111-159">Použití konzole přesměrování pro místní ladění</span><span class="sxs-lookup"><span data-stu-id="6e111-159">Use console redirection for local debugging</span></span>
<span data-ttu-id="6e111-160">V některých případech je užitečné toosee hello výstup konzoly z spuštění skriptu pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="6e111-160">Occasionally, it's useful toosee hello console output from running a script for debugging purposes.</span></span> <span data-ttu-id="6e111-161">toodo, můžete nastavit zásady přesměrování konzoly, který zapisuje hello výstupní tooa soubor.</span><span class="sxs-lookup"><span data-stu-id="6e111-161">toodo this, you can set a console redirection policy, which writes hello output tooa file.</span></span> <span data-ttu-id="6e111-162">Hello souboru výstup se nazývá složce aplikace napsané toohello **protokolu** na hello uzlu, kde je aplikace hello nasadit a spustit.</span><span class="sxs-lookup"><span data-stu-id="6e111-162">hello file output is written toohello application folder called **log** on hello node where hello application is deployed and run.</span></span> <span data-ttu-id="6e111-163">(Viz kde toofind v předchozím příkladu hello.)</span><span class="sxs-lookup"><span data-stu-id="6e111-163">(See where toofind this in hello preceding example.)</span></span>

> [!WARNING]
> <span data-ttu-id="6e111-164">Nikdy nepoužívejte zásad přesměrování konzoly hello v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv hello aplikace převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="6e111-164">Never use hello console redirection policy in an application that is deployed in production because this can affect hello application failover.</span></span> <span data-ttu-id="6e111-165">*Pouze* používejte pro místní vývoj a účely ladění.</span><span class="sxs-lookup"><span data-stu-id="6e111-165">*Only* use this for local development and debugging purposes.</span></span>  
> 
> 

<span data-ttu-id="6e111-166">Hello následující příklad ukazuje nastavení přesměrování konzoly hello s hodnotou FileRetentionCount:</span><span class="sxs-lookup"><span data-stu-id="6e111-166">hello following example shows setting hello console redirection with a FileRetentionCount value:</span></span>

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

<span data-ttu-id="6e111-167">Pokud změníte teď hello MySetup.ps1 soubor toowrite **Echo** příkaz, to bude zapisovat toohello výstupní soubor pro účely ladění:</span><span class="sxs-lookup"><span data-stu-id="6e111-167">If you now change hello MySetup.ps1 file toowrite an **Echo** command, this will write toohello output file for debugging purposes:</span></span>

```
Echo "Test console redirection which writes toohello application log folder on hello node that hello application is deployed to"
```

<span data-ttu-id="6e111-168">**Po ladění skriptu, okamžitě odebrat tuto zásadu přesměrování konzoly**.</span><span class="sxs-lookup"><span data-stu-id="6e111-168">**After you debug your script, immediately remove this console redirection policy**.</span></span>

## <a name="configure-a-policy-for-service-code-packages"></a><span data-ttu-id="6e111-169">Nakonfigurujte zásady pro balíčky služeb kódu</span><span class="sxs-lookup"><span data-stu-id="6e111-169">Configure a policy for service code packages</span></span>
<span data-ttu-id="6e111-170">V předchozích krocích hello, jste viděli, jak tooapply tooSetupEntryPoint zásad RunAs.</span><span class="sxs-lookup"><span data-stu-id="6e111-170">In hello preceding steps, you saw how tooapply a RunAs policy tooSetupEntryPoint.</span></span> <span data-ttu-id="6e111-171">Do jak toocreate jiné objekty, které mohou být použity jako služba zásad Podívejme trochu podrobněji.</span><span class="sxs-lookup"><span data-stu-id="6e111-171">Let's look a little deeper into how toocreate different principals that can be applied as service policies.</span></span>

### <a name="create-local-user-groups"></a><span data-ttu-id="6e111-172">Vytvořit místní skupiny uživatelů</span><span class="sxs-lookup"><span data-stu-id="6e111-172">Create local user groups</span></span>
<span data-ttu-id="6e111-173">Můžete definovat a vytvořit skupiny uživatelů, které umožňují jeden nebo více uživatelů toobe přidat tooa skupiny.</span><span class="sxs-lookup"><span data-stu-id="6e111-173">You can define and create user groups that allow one or more users toobe added tooa group.</span></span> <span data-ttu-id="6e111-174">To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a potřebují toohave určité společné oprávnění, které jsou k dispozici na úrovni skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-174">This is useful if there are multiple users for different service entry points and they need toohave certain common privileges that are available at hello group level.</span></span> <span data-ttu-id="6e111-175">Hello následující příklad ukazuje místní skupinu s názvem **LocalAdminGroup** s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="6e111-175">hello following example shows a local group called **LocalAdminGroup** that has administrator privileges.</span></span> <span data-ttu-id="6e111-176">Dva uživatelé, Customer1 a Customer2, stanou členy této místní skupiny.</span><span class="sxs-lookup"><span data-stu-id="6e111-176">Two users, Customer1 and Customer2, are made members of this local group.</span></span>

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a><span data-ttu-id="6e111-177">Vytvořit místní uživatele</span><span class="sxs-lookup"><span data-stu-id="6e111-177">Create local users</span></span>
<span data-ttu-id="6e111-178">Můžete vytvořit místní uživatele, který lze použít toohelp zabezpečení služby v rámci aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-178">You can create a local user that can be used toohelp secure a service within hello application.</span></span> <span data-ttu-id="6e111-179">Když **LocalUser** typ účtu zadaný v části objekty hello manifest aplikace hello, Service Fabric vytvoří místní uživatelské účty na počítačích, kde je hello aplikace nasazena.</span><span class="sxs-lookup"><span data-stu-id="6e111-179">When a **LocalUser** account type is specified in hello principals section of hello application manifest, Service Fabric creates local user accounts on machines where hello application is deployed.</span></span> <span data-ttu-id="6e111-180">Ve výchozím nastavení nemají tyto účty hello stejné názvy jako platformám zadaným v aplikaci hello manifest (například Customer3 v hello následující ukázka).</span><span class="sxs-lookup"><span data-stu-id="6e111-180">By default, these accounts do not have hello same names as those specified in hello application manifest (for example, Customer3 in hello following sample).</span></span> <span data-ttu-id="6e111-181">Místo toho se dynamicky generují a mít náhodných hesla.</span><span class="sxs-lookup"><span data-stu-id="6e111-181">Instead, they are dynamically generated and have random passwords.</span></span>

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

<span data-ttu-id="6e111-182">Pokud aplikace vyžaduje, aby hello uživatelský účet a heslo být stejná ve všech počítačích (například ověřování NTLM tooenable), musíte nastavit manifestu clusteru hello NTLMAuthenticationEnabled tootrue.</span><span class="sxs-lookup"><span data-stu-id="6e111-182">If an application requires that hello user account and password be same on all machines (for example, tooenable NTLM authentication), hello cluster manifest must set NTLMAuthenticationEnabled tootrue.</span></span> <span data-ttu-id="6e111-183">Hello manifestu clusteru musíte zadat také NTLMAuthenticationPasswordSecret, který se používá toogenerate hello stejné heslo ve všech počítačích.</span><span class="sxs-lookup"><span data-stu-id="6e111-183">hello cluster manifest must also specify an NTLMAuthenticationPasswordSecret that is used toogenerate hello same password across all machines.</span></span>

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-toohello-service-code-packages"></a><span data-ttu-id="6e111-184">Přiřaďte zásady toohello balíčky služeb kódu</span><span class="sxs-lookup"><span data-stu-id="6e111-184">Assign policies toohello service code packages</span></span>
<span data-ttu-id="6e111-185">Hello **RunAsPolicy** část **ServiceManifestImport** určuje hello účet z hello objekty oddílu, který by měl být použité toorun balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="6e111-185">hello **RunAsPolicy** section for a **ServiceManifestImport** specifies hello account from hello principals section that should be used toorun a code package.</span></span> <span data-ttu-id="6e111-186">Také přidruží kód balíčky z hello service manifest s uživatelskými účty v části objekty hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-186">It also associates code packages from hello service manifest with user accounts in hello principals section.</span></span> <span data-ttu-id="6e111-187">To můžete zadat pro instalační program hello nebo hlavní vstupních bodů, nebo můžete zadat `All` tooapply ho tooboth.</span><span class="sxs-lookup"><span data-stu-id="6e111-187">You can specify this for hello setup or main entry points, or you can specify `All` tooapply it tooboth.</span></span> <span data-ttu-id="6e111-188">Hello následující příklad ukazuje použití různých zásad:</span><span class="sxs-lookup"><span data-stu-id="6e111-188">hello following example shows different policies being applied:</span></span>

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

<span data-ttu-id="6e111-189">Pokud **entrypointtype typu** není zadán, hello výchozí nastavení je tooEntryPointType = "Hlavní".</span><span class="sxs-lookup"><span data-stu-id="6e111-189">If **EntryPointType** is not specified, hello default is set tooEntryPointType=”Main”.</span></span> <span data-ttu-id="6e111-190">Určení **SetupEntryPoint** je obzvláště užitečná, pokud chcete toorun určité operace instalace s vysokou úrovní oprávnění v rámci účtu system.</span><span class="sxs-lookup"><span data-stu-id="6e111-190">Specifying **SetupEntryPoint** is especially useful when you want toorun certain high-privilege setup operations under a system account.</span></span> <span data-ttu-id="6e111-191">kódu Hello skutečné služby můžete spustit pod účtem nižší oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6e111-191">hello actual service code can run under a lower-privilege account.</span></span>

### <a name="apply-a-default-policy-tooall-service-code-packages"></a><span data-ttu-id="6e111-192">Použít kód balíčky výchozí zásady tooall služeb</span><span class="sxs-lookup"><span data-stu-id="6e111-192">Apply a default policy tooall service code packages</span></span>
<span data-ttu-id="6e111-193">Použít hello **DefaultRunAsPolicy** části toospecify výchozí uživatelský účet pro všechny balíčky kódu, které nemají konkrétní **RunAsPolicy** definované.</span><span class="sxs-lookup"><span data-stu-id="6e111-193">You use hello **DefaultRunAsPolicy** section toospecify a default user account for all code packages that don’t have a specific **RunAsPolicy** defined.</span></span> <span data-ttu-id="6e111-194">Pokud potřebujete toorun pod většinu hello kód balíčky, které jsou určené v manifestu služby hello používá jiná aplikace hello stejného uživatele, hello aplikace můžete definovat jenom výchozí zásady RunAs k tomuto účtu.</span><span class="sxs-lookup"><span data-stu-id="6e111-194">If most of hello code packages that are specified in hello service manifest used by an application need toorun under hello same user, hello application can just define a default RunAs policy with that user account.</span></span> <span data-ttu-id="6e111-195">Hello následující příklad určuje, že pokud balíček kódu neobsahuje **RunAsPolicy** zadán, balíček kódu hello měly být spuštěny pod hello **MyDefaultAccount** uvedený v oddílu hello objekty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6e111-195">hello following example specifies that if a code package does not have a **RunAsPolicy** specified, hello code package should run under hello **MyDefaultAccount** specified in hello principals section.</span></span>

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a><span data-ttu-id="6e111-196">Použít skupiny domény služby Active Directory nebo uživatele</span><span class="sxs-lookup"><span data-stu-id="6e111-196">Use an Active Directory domain group or user</span></span>
<span data-ttu-id="6e111-197">Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí hello samostatný instalační program systému můžete spustit služba hello hello pověření pro účet uživatele nebo skupiny služby Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6e111-197">For an instance of Service Fabric that was installed on Windows Server by using hello standalone installer, you can run hello service under hello credentials for an Active Directory user or group account.</span></span> <span data-ttu-id="6e111-198">Toto je služby Active Directory místně ve vaší doméně a není v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e111-198">This is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="6e111-199">Pomocí účtem uživatele domény nebo skupiny můžete pak přístup k jiným prostředkům v doméně hello (například sdílené složky), kterým bylo uděleno oprávnění.</span><span class="sxs-lookup"><span data-stu-id="6e111-199">By using a domain user or group, you can then access other resources in hello domain (for example, file shares) that have been granted permissions.</span></span>

<span data-ttu-id="6e111-200">Hello následující příklad ukazuje uživatele služby Active Directory názvem *TestUser* s jejich domény heslo šifrované pomocí certifikátu nazývá *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="6e111-200">hello following example shows an Active Directory user called *TestUser* with their domain password encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="6e111-201">Můžete použít hello `Invoke-ServiceFabricEncryptText` prostředí PowerShell příkaz toocreate hello tajný šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="6e111-201">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text.</span></span> <span data-ttu-id="6e111-202">V tématu [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6e111-202">See [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md) for details.</span></span>

<span data-ttu-id="6e111-203">Je nutné nasadit pomocí metody out-of-band hello privátní klíč hello certifikát toodecrypt hello heslo toohello místního počítače (v Azure, je to prostřednictvím Správce Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="6e111-203">You must deploy hello private key of hello certificate toodecrypt hello password toohello local machine by using an out-of-band method (in Azure, this is via Azure Resource Manager).</span></span> <span data-ttu-id="6e111-204">Když Service Fabric nasadí počítač toohello balíček služby hello, pak je možné toodecrypt hello tajný klíč a (spolu s hello uživatelské jméno) ověření pomocí služby Active Directory toorun pod tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="6e111-204">Then, when Service Fabric deploys hello service package toohello machine, it is able toodecrypt hello secret and (along with hello user name) authenticate with Active Directory toorun under those credentials.</span></span>

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a><span data-ttu-id="6e111-205">Použijte skupinu účet spravované služby.</span><span class="sxs-lookup"><span data-stu-id="6e111-205">Use a Group Managed Service Account.</span></span>
<span data-ttu-id="6e111-206">Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí hello samostatný instalační program systému můžete spustit hello služby jako skupinový účet spravované služby (gMSA).</span><span class="sxs-lookup"><span data-stu-id="6e111-206">For an instance of Service Fabric that was installed on Windows Server by using hello standalone installer, you can run hello service as a group Managed Service Account (gMSA).</span></span> <span data-ttu-id="6e111-207">Upozorňujeme, že služby Active Directory v místě ve vaší doméně a není v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e111-207">Note that this is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="6e111-208">Pomocí gMSA není žádná hesla nebo zašifrované heslo, které jsou uložené v hello `Application Manifest`.</span><span class="sxs-lookup"><span data-stu-id="6e111-208">By using a gMSA there is no password or encrypted password stored in hello `Application Manifest`.</span></span>

<span data-ttu-id="6e111-209">Hello následující příklad ukazuje, jak toocreate účet gMSA názvem *svc Test$*; jak toodeploy, který spravuje služby účet toohello uzly; a jak tooconfigure hello hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e111-209">hello following example shows how toocreate a gMSA account called *svc-Test$*; how toodeploy that managed service account toohello cluster nodes; and how tooconfigure hello user principal.</span></span>

##### <a name="prerequisites"></a><span data-ttu-id="6e111-210">Požadavky.</span><span class="sxs-lookup"><span data-stu-id="6e111-210">Prerequisites.</span></span>
- <span data-ttu-id="6e111-211">Hello domény musí kořenový klíč služby KDS.</span><span class="sxs-lookup"><span data-stu-id="6e111-211">hello domain needs a KDS root key.</span></span>
- <span data-ttu-id="6e111-212">Hello domény musí toobe v systému Windows Server 2012 nebo novější úroveň funkčnosti.</span><span class="sxs-lookup"><span data-stu-id="6e111-212">hello domain needs toobe at a Windows Server 2012 or later functional level.</span></span>

##### <a name="example"></a><span data-ttu-id="6e111-213">Příklad</span><span class="sxs-lookup"><span data-stu-id="6e111-213">Example</span></span>
1. <span data-ttu-id="6e111-214">Požádejte správce domény služby active directory, vytvoření účtu služby skupina spravované pomocí hello `New-ADServiceAccount` příkaz a ujistěte se, že hello `PrincipalsAllowedToRetrieveManagedPassword` zahrnuje všechny uzly clusteru prostředků infrastruktury služby hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-214">Have an active directory domain administrator create a group managed service account using hello `New-ADServiceAccount` commandlet and ensure that hello `PrincipalsAllowedToRetrieveManagedPassword` includes all of hello service fabric cluster nodes.</span></span> <span data-ttu-id="6e111-215">Všimněte si, že `AccountName`, `DnsHostName`, a `ServicePrincipalName` musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6e111-215">Note that `AccountName`, `DnsHostName`, and `ServicePrincipalName` must be unique.</span></span>
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. <span data-ttu-id="6e111-216">Na každém uzlu clusteru, hello služby fabric (například `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), instalace a testování hello gMSA.</span><span class="sxs-lookup"><span data-stu-id="6e111-216">On each of hello service fabric cluster nodes (for example, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), install and test hello gMSA.</span></span>
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. <span data-ttu-id="6e111-217">Nakonfigurovat hlavní název uživatele hello a nakonfigurujte hello RunAsPolicy tooreference hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e111-217">Configure hello User principal, and configure hello RunAsPolicy tooreference hello user.</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a><span data-ttu-id="6e111-218">Přiřadit zásady zabezpečení přístupu pro koncové body HTTP a HTTPS</span><span class="sxs-lookup"><span data-stu-id="6e111-218">Assign a security access policy for HTTP and HTTPS endpoints</span></span>
<span data-ttu-id="6e111-219">Pokud použijete službu tooa zásad RunAs a hello service manifest deklaruje koncový bod prostředků s protokolem hello HTTP, je nutné zadat **SecurityAccessPolicy** tooensure, že porty přidělené toothese koncové body jsou správně řízení přístupu uvedené pro hello RunAs uživatelský účet, který spouští služba hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-219">If you apply a RunAs policy tooa service and hello service manifest declares endpoint resources with hello HTTP protocol, you must specify a **SecurityAccessPolicy** tooensure that ports allocated toothese endpoints are correctly access-control listed for hello RunAs user account that hello service runs under.</span></span> <span data-ttu-id="6e111-220">V opačném **http.sys** není přístup toohello služby a získat selhání pomocí volání od klienta hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-220">Otherwise, **http.sys** does not have access toohello service, and you get failures with calls from hello client.</span></span> <span data-ttu-id="6e111-221">Hello následující příklad se týká hello Customer1 tooan koncový bod účtu názvem **EndpointName**, což dává ho úplná přístupová práva.</span><span class="sxs-lookup"><span data-stu-id="6e111-221">hello following example applies hello Customer1 account tooan endpoint called **EndpointName**, which gives it full access rights.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

<span data-ttu-id="6e111-222">Pro koncový bod HTTPS hello máte také název hello tooindicate hello certifikát tooreturn toohello klienta.</span><span class="sxs-lookup"><span data-stu-id="6e111-222">For hello HTTPS endpoint, you also have tooindicate hello name of hello certificate tooreturn toohello client.</span></span> <span data-ttu-id="6e111-223">Můžete to provést pomocí **EndpointBindingPolicy**, certifikátem hello definovaný v oddílu certifikáty v manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-223">You can do this by using **EndpointBindingPolicy**, with hello certificate defined in a certificates section in hello application manifest.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if hello EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a><span data-ttu-id="6e111-224">Upgrade více aplikací s koncovými body https</span><span class="sxs-lookup"><span data-stu-id="6e111-224">Upgrading multiple applications with https endpoints</span></span>
<span data-ttu-id="6e111-225">Je třeba toobe pečlivě není toouse hello **stejný port** pro různé instance hello stejnou aplikaci, když pomocí protokolu http**s**.</span><span class="sxs-lookup"><span data-stu-id="6e111-225">You need toobe careful not toouse hello **same port** for different instances of hello same application when using http**s**.</span></span> <span data-ttu-id="6e111-226">Hello důvodem je, že Service Fabric nebude možné tooupgrade hello certifikátu pro jeden instancí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6e111-226">hello reason is that Service Fabric won't be able tooupgrade hello cert for one of hello application instances.</span></span> <span data-ttu-id="6e111-227">Například pokud aplikace 1 nebo aplikace 2 oba mají tooupgrade jejich toocert cert 1, 2.</span><span class="sxs-lookup"><span data-stu-id="6e111-227">For example, if application 1 or application 2 both want tooupgrade their cert 1 toocert 2.</span></span> <span data-ttu-id="6e111-228">Při upgradu hello se stane, Service Fabric může mít vyčistit hello cert 1 registraci http.sys Přestože hello je stále ji používá jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e111-228">When hello upgrade happens, Service Fabric might have cleaned up hello cert 1 registration with http.sys even though hello other application is still using it.</span></span> <span data-ttu-id="6e111-229">tooprevent, Service Fabric zjistí, že již existuje jiná instance aplikace zaregistrované na portu hello certifikátem hello (kvůli toohttp.sys) a hello operace selže.</span><span class="sxs-lookup"><span data-stu-id="6e111-229">tooprevent this, Service Fabric detects that there is already another application instance registered on hello port with hello certificate (due toohttp.sys) and fails hello operation.</span></span>

<span data-ttu-id="6e111-230">Proto Service Fabric nepodporuje upgrade dvě různé služby pomocí **hello stejný port** v případech jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e111-230">Hence Service Fabric does not support upgrading two different services using **hello same port** in different application instances.</span></span> <span data-ttu-id="6e111-231">Jinými slovy, nemůžete použít stejný certifikát hello na různé služby na hello stejný port.</span><span class="sxs-lookup"><span data-stu-id="6e111-231">In other words, you cannot use hello same certificate on different services on hello same port.</span></span> <span data-ttu-id="6e111-232">Pokud potřebujete toohave sdílenou certifikátů na hello stejný port, je nutné tooensure této hello služby jsou umístěny na jiné počítače s omezeními umístění.</span><span class="sxs-lookup"><span data-stu-id="6e111-232">If you need toohave a shared certificate on hello same port, you need tooensure that hello services are placed on different machines with placement constraints.</span></span> <span data-ttu-id="6e111-233">Nebo zvažte, pokud je to možné pomocí Service Fabric dynamické porty pro každou službu v každé instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e111-233">Or consider using Service Fabric dynamic ports if possible for each service in each application instance.</span></span> 

<span data-ttu-id="6e111-234">Pokud se zobrazí upgradu služeb při selhání s protokolem https, k chybě upozornění "hello rozhraní API systému Windows HTTP serveru nepodporuje víc certifikátů pro aplikace, které sdílejí port."</span><span class="sxs-lookup"><span data-stu-id="6e111-234">If you see an upgrade fail with https, an error warning saying "hello Windows HTTP Server API does not support multiple certificates for applications that share a port.”</span></span>

## <a name="a-complete-application-manifest-example"></a><span data-ttu-id="6e111-235">V příkladu manifestu aplikace dokončena</span><span class="sxs-lookup"><span data-stu-id="6e111-235">A complete application manifest example</span></span>
<span data-ttu-id="6e111-236">Následující manifest aplikace Hello řadu hello ukazuje různých nastavení:</span><span class="sxs-lookup"><span data-stu-id="6e111-236">hello following application manifest shows many of hello different settings:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed hello EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="6e111-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e111-237">Next steps</span></span>
* [<span data-ttu-id="6e111-238">Pochopení hello aplikačního modelu</span><span class="sxs-lookup"><span data-stu-id="6e111-238">Understand hello application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="6e111-239">Zadejte prostředky v service manifest</span><span class="sxs-lookup"><span data-stu-id="6e111-239">Specify resources in a service manifest</span></span>](service-fabric-service-manifest-resources.md)
* [<span data-ttu-id="6e111-240">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="6e111-240">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
