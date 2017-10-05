---
title: "Informace o zásadách zabezpečení Azure mikroslužeb | Microsoft Docs"
description: "Přehled o tom, jak běžet v rámci systému a účtů místní zabezpečení, včetně SetupEntry bodu, pokud aplikace potřebuje k provedení některých privilegované akce před spuštěním aplikace Service Fabric"
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
ms.openlocfilehash: e673b45a43a06d18040c3437caf8765704d5c36a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-security-policies-for-your-application"></a><span data-ttu-id="592b8-103">Konfigurace zásad zabezpečení pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="592b8-103">Configure security policies for your application</span></span>
<span data-ttu-id="592b8-104">Pomocí Azure Service Fabric můžete zabezpečit aplikace, které jsou spuštěny v clusteru v rámci jiné uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="592b8-104">By using Azure Service Fabric, you can secure applications that are running in the cluster under different user accounts.</span></span> <span data-ttu-id="592b8-105">Service Fabric také pomáhá zabezpečit prostředky, které jsou používány aplikací v době nasazení podle uživatelských účtů – například soubory, adresářů a certifikáty.</span><span class="sxs-lookup"><span data-stu-id="592b8-105">Service Fabric also helps secure the resources that are used by applications at the time of deployment under the user accounts--for example, files, directories, and certificates.</span></span> <span data-ttu-id="592b8-106">Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.</span><span class="sxs-lookup"><span data-stu-id="592b8-106">This makes running applications, even in a shared hosted environment, more secure from one another.</span></span>

<span data-ttu-id="592b8-107">Ve výchozím nastavení se aplikace Service Fabric běžet pod účtem, proces Fabric.exe kompatibilní se.</span><span class="sxs-lookup"><span data-stu-id="592b8-107">By default, Service Fabric applications run under the account that the Fabric.exe process runs under.</span></span> <span data-ttu-id="592b8-108">Service Fabric taky poskytuje možnost spouštět aplikace pod účtem místního uživatelského účtu nebo účtu local system, který je určený v rámci manifest aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-108">Service Fabric also provides the capability to run applications under a local user account or local system account, which is specified within the application manifest.</span></span> <span data-ttu-id="592b8-109">Typy účtů podporovaný místní systém **LocalUser**, **NetworkService**, **LocalService**, a **LocalSystem**.</span><span class="sxs-lookup"><span data-stu-id="592b8-109">Supported local system account types are **LocalUser**, **NetworkService**, **LocalService**, and **LocalSystem**.</span></span>

 <span data-ttu-id="592b8-110">Když spouštíte Service Fabric na Windows serveru ve vašem datovém centru pomocí samostatný instalační program systému, můžete účty domény služby Active Directory, včetně skupinové účty spravované služby.</span><span class="sxs-lookup"><span data-stu-id="592b8-110">When you're running Service Fabric on Windows Server in your datacenter by using the standalone installer, you can use Active Directory domain accounts, including group managed service accounts.</span></span>

<span data-ttu-id="592b8-111">Můžete definovat a vytvořit skupiny uživatelů, aby pro každou skupinu pro správu společně lze přidat jeden nebo více uživatelů.</span><span class="sxs-lookup"><span data-stu-id="592b8-111">You can define and create user groups so that one or more users can be added to each group to be managed together.</span></span> <span data-ttu-id="592b8-112">To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a vyžadují, aby byla určité společné oprávnění, které jsou k dispozici na úrovni skupiny.</span><span class="sxs-lookup"><span data-stu-id="592b8-112">This is useful when there are multiple users for different service entry points and they need to have certain common privileges that are available at the group level.</span></span>

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a><span data-ttu-id="592b8-113">Konfigurace zásad pro bod služby instalační položka</span><span class="sxs-lookup"><span data-stu-id="592b8-113">Configure the policy for a service setup entry point</span></span>
<span data-ttu-id="592b8-114">Jak je popsáno v [aplikačního modelu](service-fabric-application-model.md), instalační program vstupního bodu, **SetupEntryPoint**, je privilegované vstupního bodu, který běží se stejnými pověřeními, jako Service Fabric (obvykle *NetworkService* účtu) před další vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="592b8-114">As described in the [application model](service-fabric-application-model.md), the setup entry point, **SetupEntryPoint**, is a privileged entry point that runs with the same credentials as Service Fabric (typically the *NetworkService* account) before any other entry point.</span></span> <span data-ttu-id="592b8-115">Spustitelný soubor, který je zadán **EntryPoint** je obvykle dlouho běžící hostitele služby.</span><span class="sxs-lookup"><span data-stu-id="592b8-115">The executable that is specified by **EntryPoint** is typically the long-running service host.</span></span> <span data-ttu-id="592b8-116">Proto nutnosti samostatného instalačního vstupního bodu tomu není nutné spustit spustitelný soubor hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="592b8-116">So having a separate setup entry point avoids having to run the service host executable with high privileges for extended periods of time.</span></span> <span data-ttu-id="592b8-117">Spustitelný soubor, **EntryPoint** určuje běží **SetupEntryPoint** ukončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="592b8-117">The executable that **EntryPoint** specifies is run after **SetupEntryPoint** exits successfully.</span></span> <span data-ttu-id="592b8-118">Výsledný proces monitorovat a restartuje a znovu začíná **SetupEntryPoint** Pokud někdy ukončí nebo dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="592b8-118">The resulting process is monitored and restarted, and begins again with **SetupEntryPoint** if it ever terminates or crashes.</span></span>

<span data-ttu-id="592b8-119">Zde je jednoduché služby manifestu příklad, který ukazuje SetupEntryPoint a hlavní vstupní bod pro službu.</span><span class="sxs-lookup"><span data-stu-id="592b8-119">The following is a simple service manifest example that shows the SetupEntryPoint and the main EntryPoint for the service.</span></span>

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

### <a name="configure-the-policy-by-using-a-local-account"></a><span data-ttu-id="592b8-120">Nakonfigurujte zásady pomocí místního účtu</span><span class="sxs-lookup"><span data-stu-id="592b8-120">Configure the policy by using a local account</span></span>
<span data-ttu-id="592b8-121">Po dokončení konfigurace služby tak, aby měl instalační program vstupního bodu, můžete změnit oprávnění zabezpečení, které běží v části v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-121">After you configure the service to have a setup entry point, you can change the security permissions that it runs under in the application manifest.</span></span> <span data-ttu-id="592b8-122">Následující příklad ukazuje, jak nakonfigurovat službu pro spuštění pod oprávnění uživatelského účtu správce.</span><span class="sxs-lookup"><span data-stu-id="592b8-122">The following example shows how to configure the service to run under user administrator account privileges.</span></span>

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

<span data-ttu-id="592b8-123">Nejprve vytvořte **objekty** část s uživatelským jménem, jako je například SetupAdminUser.</span><span class="sxs-lookup"><span data-stu-id="592b8-123">First, create a **Principals** section with a user name, such as SetupAdminUser.</span></span> <span data-ttu-id="592b8-124">To znamená, že uživatel je členem skupiny Administrators systému.</span><span class="sxs-lookup"><span data-stu-id="592b8-124">This indicates that the user is a member of the Administrators system group.</span></span>

<span data-ttu-id="592b8-125">Dále v části **ServiceManifestImport** nakonfigurujte zásadu použít tento objekt zabezpečení na **SetupEntryPoint**.</span><span class="sxs-lookup"><span data-stu-id="592b8-125">Next, under the **ServiceManifestImport** section, configure a policy to apply this principal to **SetupEntryPoint**.</span></span> <span data-ttu-id="592b8-126">Tato hodnota informuje Service Fabric který po **MySetup.bat** spuštění souboru, měla by být `RunAs` s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="592b8-126">This tells Service Fabric that when the **MySetup.bat** file is run, it should be `RunAs` with administrator privileges.</span></span> <span data-ttu-id="592b8-127">Vzhledem k tomu, že máte *není* použít zásadu na hlavní vstupní bod, kód v **MyServiceHost.exe** běží v rámci systému **NetworkService** účtu.</span><span class="sxs-lookup"><span data-stu-id="592b8-127">Given that you have *not* applied a policy to the main entry point, the code in **MyServiceHost.exe** runs under the system **NetworkService** account.</span></span> <span data-ttu-id="592b8-128">Toto je výchozí účet, který všechny vstupní body služby jsou spustit jako.</span><span class="sxs-lookup"><span data-stu-id="592b8-128">This is the default account that all service entry points are run as.</span></span>

<span data-ttu-id="592b8-129">Nyní přidejte umožňuje soubor MySetup.bat do projektu sady Visual Studio k testování s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="592b8-129">Let's now add the file MySetup.bat to the Visual Studio project to test the administrator privileges.</span></span> <span data-ttu-id="592b8-130">V sadě Visual Studio klikněte pravým tlačítkem na projekt služby a přidejte nový soubor s názvem MySetup.bat.</span><span class="sxs-lookup"><span data-stu-id="592b8-130">In Visual Studio, right-click the service project and add a new file called MySetup.bat.</span></span>

<span data-ttu-id="592b8-131">Dále se ujistěte, že soubor MySetup.bat je součástí balíčku služby.</span><span class="sxs-lookup"><span data-stu-id="592b8-131">Next, ensure that the MySetup.bat file is included in the service package.</span></span> <span data-ttu-id="592b8-132">Ve výchozím nastavení není.</span><span class="sxs-lookup"><span data-stu-id="592b8-132">By default, it is not.</span></span> <span data-ttu-id="592b8-133">Vyberte soubor, klikněte pravým tlačítkem na získat v místní nabídce a zvolte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="592b8-133">Select the file, right-click to get the context menu, and choose **Properties**.</span></span> <span data-ttu-id="592b8-134">V dialogovém okně Vlastnosti ověřte, že **kopírovat do výstupního adresáře** je nastaven na **kopírovat, pokud je novější**.</span><span class="sxs-lookup"><span data-stu-id="592b8-134">In the Properties dialog box, ensure that **Copy to Output Directory** is set to **Copy if newer**.</span></span> <span data-ttu-id="592b8-135">Viz následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="592b8-135">See the following screenshot.</span></span>

![Visual Studio CopyToOutput pro SetupEntryPoint dávkového souboru][image1]

<span data-ttu-id="592b8-137">Nyní otevřete soubor MySetup.bat a přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="592b8-137">Now open the MySetup.bat file and add the following commands:</span></span>

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

<span data-ttu-id="592b8-138">V dalším kroku sestavení a nasadit řešení místního vývojového clusteru.</span><span class="sxs-lookup"><span data-stu-id="592b8-138">Next, build and deploy the solution to a local development cluster.</span></span> <span data-ttu-id="592b8-139">Po spuštění služby, jak je znázorněno v Service Fabric Exploreru, uvidíte, že MySetup.bat souboru bylo úspěšné dvěma způsoby.</span><span class="sxs-lookup"><span data-stu-id="592b8-139">After the service has started, as shown in Service Fabric Explorer, you can see that the MySetup.bat file was successful in a two ways.</span></span> <span data-ttu-id="592b8-140">Otevřete příkazový řádek prostředí PowerShell a zadejte:</span><span class="sxs-lookup"><span data-stu-id="592b8-140">Open a PowerShell command prompt and type:</span></span>

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

<span data-ttu-id="592b8-141">Poznamenejte si název uzlu, kde služba byla nasazená a spustit v Service Fabric Exploreru – například uzlu 2.</span><span class="sxs-lookup"><span data-stu-id="592b8-141">Then, note the name of the node where the service was deployed and started in Service Fabric Explorer--for example, Node 2.</span></span> <span data-ttu-id="592b8-142">Dále přejděte do složky pracovní instance aplikace out.txt soubor, který se zobrazuje hodnota **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="592b8-142">Next, navigate to the application instance work folder to find the out.txt file that shows the value of **TestVariable**.</span></span> <span data-ttu-id="592b8-143">Například pokud tato služba byla nasazena na uzlu 2, potom můžete přejít na tuto cestu pro **MyApplicationType**:</span><span class="sxs-lookup"><span data-stu-id="592b8-143">For example, if this service was deployed to Node 2, then you can go to this path for the **MyApplicationType**:</span></span>

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-the-policy-by-using-local-system-accounts"></a><span data-ttu-id="592b8-144">Nakonfigurujte zásady pomocí místní systémové účty</span><span class="sxs-lookup"><span data-stu-id="592b8-144">Configure the policy by using local system accounts</span></span>
<span data-ttu-id="592b8-145">Často je vhodnější pro spuštění skriptu spuštění pomocí účtu local system, nikoli účet správce.</span><span class="sxs-lookup"><span data-stu-id="592b8-145">Often, it's preferable to run the startup script by using a local system account rather than an administrator account.</span></span> <span data-ttu-id="592b8-146">Spuštění zásad RunAs jako člen skupiny Administrators obvykle nefunguje správně, protože počítače mají přístup k řízení Uživatelských účtů ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="592b8-146">Running the RunAs policy as a member of the Administrators group typically doesn’t work well because machines have User Access Control (UAC) enabled by default.</span></span> <span data-ttu-id="592b8-147">V takových případech **doporučujeme spustit SetupEntryPoint pod účtem LocalSystem, ne jako místní uživatel přidán do skupiny Administrators**.</span><span class="sxs-lookup"><span data-stu-id="592b8-147">In such cases, **the recommendation is to run the SetupEntryPoint as LocalSystem, instead of as a local user added to Administrators group**.</span></span> <span data-ttu-id="592b8-148">Následující příklad ukazuje nastavení SetupEntryPoint běží pod účtem LocalSystem:</span><span class="sxs-lookup"><span data-stu-id="592b8-148">The following example shows setting the SetupEntryPoint to run as LocalSystem:</span></span>

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

## <a name="start-powershell-commands-from-a-setup-entry-point"></a><span data-ttu-id="592b8-149">Spusťte příkazy prostředí PowerShell ze vstupního bodu instalační program</span><span class="sxs-lookup"><span data-stu-id="592b8-149">Start PowerShell commands from a setup entry point</span></span>
<span data-ttu-id="592b8-150">Spustit prostředí PowerShell z **SetupEntryPoint** bodu, můžete spustit **PowerShell.exe** v dávkovém souboru, který odkazuje na soubor prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="592b8-150">To run PowerShell from the **SetupEntryPoint** point, you can run **PowerShell.exe** in a batch file that points to a PowerShell file.</span></span> <span data-ttu-id="592b8-151">Nejprve přidejte soubor prostředí PowerShell služby projektu – například **MySetup.ps1**.</span><span class="sxs-lookup"><span data-stu-id="592b8-151">First, add a PowerShell file to the service project--for example, **MySetup.ps1**.</span></span> <span data-ttu-id="592b8-152">Nezapomeňte nastavit *kopírovat, pokud je novější* vlastnost tak, aby soubor jsou také obsaženy v balíčku služby.</span><span class="sxs-lookup"><span data-stu-id="592b8-152">Remember to set the *Copy if newer* property so that the file is also included in the service package.</span></span> <span data-ttu-id="592b8-153">Následující příklad ukazuje dávkový soubor začínající soubor prostředí PowerShell s názvem MySetup.ps1, která nastavuje proměnnou prostředí systému s názvem **TestVariable**.</span><span class="sxs-lookup"><span data-stu-id="592b8-153">The following example shows a sample batch file that starts a PowerShell file called MySetup.ps1, which sets a system environment variable called **TestVariable**.</span></span>

<span data-ttu-id="592b8-154">MySetup.bat spustit soubor prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="592b8-154">MySetup.bat to start a PowerShell file:</span></span>

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

<span data-ttu-id="592b8-155">V prostředí PowerShell souboru přidejte následující nastavení proměnné prostředí systému:</span><span class="sxs-lookup"><span data-stu-id="592b8-155">In the PowerShell file, add the following to set a system environment variable:</span></span>

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> <span data-ttu-id="592b8-156">Ve výchozím nastavení, když dávkový soubor spouští, vypadá to, v aplikaci složku s názvem **pracovní** pro soubory.</span><span class="sxs-lookup"><span data-stu-id="592b8-156">By default, when the batch file runs, it looks at the application folder called **work** for files.</span></span> <span data-ttu-id="592b8-157">V takovém případě při spuštění MySetup.bat chceme najít soubor MySetup.ps1 ve stejné složce, která je aplikace **balíček kódu** složky.</span><span class="sxs-lookup"><span data-stu-id="592b8-157">In this case, when MySetup.bat runs, we want this to find the MySetup.ps1 file in the same folder, which is the application **code package** folder.</span></span> <span data-ttu-id="592b8-158">Chcete-li změnit tato složka, nastavte pracovní složku:</span><span class="sxs-lookup"><span data-stu-id="592b8-158">To change this folder, set the working folder:</span></span>
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

## <a name="use-console-redirection-for-local-debugging"></a><span data-ttu-id="592b8-159">Použití konzole přesměrování pro místní ladění</span><span class="sxs-lookup"><span data-stu-id="592b8-159">Use console redirection for local debugging</span></span>
<span data-ttu-id="592b8-160">V některých případech je užitečné, pokud chcete zobrazit výstup konzoly z spuštění skriptu pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="592b8-160">Occasionally, it's useful to see the console output from running a script for debugging purposes.</span></span> <span data-ttu-id="592b8-161">K tomuto účelu můžete nastavit zásadu přesměrování konzoly, který zapisuje výstup do souboru.</span><span class="sxs-lookup"><span data-stu-id="592b8-161">To do this, you can set a console redirection policy, which writes the output to a file.</span></span> <span data-ttu-id="592b8-162">Soubor výstup zapsán do aplikace složku s názvem **protokolu** na uzlu, kde je aplikace nasazena a spustit.</span><span class="sxs-lookup"><span data-stu-id="592b8-162">The file output is written to the application folder called **log** on the node where the application is deployed and run.</span></span> <span data-ttu-id="592b8-163">(Viz kde najít to v předchozím příkladu).</span><span class="sxs-lookup"><span data-stu-id="592b8-163">(See where to find this in the preceding example.)</span></span>

> [!WARNING]
> <span data-ttu-id="592b8-164">Nikdy nepoužívejte konzolu Zásady přesměrování v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv převzetí služeb při selhání aplikaci.</span><span class="sxs-lookup"><span data-stu-id="592b8-164">Never use the console redirection policy in an application that is deployed in production because this can affect the application failover.</span></span> <span data-ttu-id="592b8-165">*Pouze* používejte pro místní vývoj a účely ladění.</span><span class="sxs-lookup"><span data-stu-id="592b8-165">*Only* use this for local development and debugging purposes.</span></span>  
> 
> 

<span data-ttu-id="592b8-166">Následující příklad ukazuje nastavení přesměrování konzoly s hodnotou FileRetentionCount:</span><span class="sxs-lookup"><span data-stu-id="592b8-166">The following example shows setting the console redirection with a FileRetentionCount value:</span></span>

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

<span data-ttu-id="592b8-167">Pokud změníte teď MySetup.ps1 souboru pro zápis **Echo** příkaz, to bude zapisovat do výstupního souboru pro účely ladění:</span><span class="sxs-lookup"><span data-stu-id="592b8-167">If you now change the MySetup.ps1 file to write an **Echo** command, this will write to the output file for debugging purposes:</span></span>

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

<span data-ttu-id="592b8-168">**Po ladění skriptu, okamžitě odebrat tuto zásadu přesměrování konzoly**.</span><span class="sxs-lookup"><span data-stu-id="592b8-168">**After you debug your script, immediately remove this console redirection policy**.</span></span>

## <a name="configure-a-policy-for-service-code-packages"></a><span data-ttu-id="592b8-169">Nakonfigurujte zásady pro balíčky služeb kódu</span><span class="sxs-lookup"><span data-stu-id="592b8-169">Configure a policy for service code packages</span></span>
<span data-ttu-id="592b8-170">V předchozích krocích jste viděli, jak použít zásadu RunAs SetupEntryPoint.</span><span class="sxs-lookup"><span data-stu-id="592b8-170">In the preceding steps, you saw how to apply a RunAs policy to SetupEntryPoint.</span></span> <span data-ttu-id="592b8-171">O tom, jak vytvořit různé objekty, které mohou být použity jako zásady služby Podívejme trochu podrobněji.</span><span class="sxs-lookup"><span data-stu-id="592b8-171">Let's look a little deeper into how to create different principals that can be applied as service policies.</span></span>

### <a name="create-local-user-groups"></a><span data-ttu-id="592b8-172">Vytvořit místní skupiny uživatelů</span><span class="sxs-lookup"><span data-stu-id="592b8-172">Create local user groups</span></span>
<span data-ttu-id="592b8-173">Můžete definovat a vytvořit skupiny uživatelů, které umožňují jeden nebo více uživatelů, který se má přidat do skupiny.</span><span class="sxs-lookup"><span data-stu-id="592b8-173">You can define and create user groups that allow one or more users to be added to a group.</span></span> <span data-ttu-id="592b8-174">To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a vyžadují, aby byla určité společné oprávnění, které jsou k dispozici na úrovni skupiny.</span><span class="sxs-lookup"><span data-stu-id="592b8-174">This is useful if there are multiple users for different service entry points and they need to have certain common privileges that are available at the group level.</span></span> <span data-ttu-id="592b8-175">Následující příklad ukazuje místní skupinu s názvem **LocalAdminGroup** s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="592b8-175">The following example shows a local group called **LocalAdminGroup** that has administrator privileges.</span></span> <span data-ttu-id="592b8-176">Dva uživatelé, Customer1 a Customer2, stanou členy této místní skupiny.</span><span class="sxs-lookup"><span data-stu-id="592b8-176">Two users, Customer1 and Customer2, are made members of this local group.</span></span>

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

### <a name="create-local-users"></a><span data-ttu-id="592b8-177">Vytvořit místní uživatele</span><span class="sxs-lookup"><span data-stu-id="592b8-177">Create local users</span></span>
<span data-ttu-id="592b8-178">Můžete vytvořit místní uživatele, který slouží k zajištění služby v rámci aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-178">You can create a local user that can be used to help secure a service within the application.</span></span> <span data-ttu-id="592b8-179">Když **LocalUser** typ účtu je definováno v sekci objekty manifestu aplikace Service Fabric vytvoří místní uživatelské účty na počítačích, kde je aplikace nasazená.</span><span class="sxs-lookup"><span data-stu-id="592b8-179">When a **LocalUser** account type is specified in the principals section of the application manifest, Service Fabric creates local user accounts on machines where the application is deployed.</span></span> <span data-ttu-id="592b8-180">Ve výchozím nastavení tyto účty nemají stejné názvy jako platformám zadaným v manifest aplikace (například Customer3 v následující ukázce).</span><span class="sxs-lookup"><span data-stu-id="592b8-180">By default, these accounts do not have the same names as those specified in the application manifest (for example, Customer3 in the following sample).</span></span> <span data-ttu-id="592b8-181">Místo toho se dynamicky generují a mít náhodných hesla.</span><span class="sxs-lookup"><span data-stu-id="592b8-181">Instead, they are dynamically generated and have random passwords.</span></span>

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

<span data-ttu-id="592b8-182">Pokud aplikace vyžaduje, aby uživatelský účet a heslo být stejná ve všech počítačích (například pro povolení ověřování NTLM), v manifestu clusteru musí nastavit NTLMAuthenticationEnabled na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="592b8-182">If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true.</span></span> <span data-ttu-id="592b8-183">V manifestu clusteru musíte zadat také NTLMAuthenticationPasswordSecret, který se používá ke generování stejné heslo ve všech počítačích.</span><span class="sxs-lookup"><span data-stu-id="592b8-183">The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that is used to generate the same password across all machines.</span></span>

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-to-the-service-code-packages"></a><span data-ttu-id="592b8-184">Přiřadit zásady do balíčků kódu služby</span><span class="sxs-lookup"><span data-stu-id="592b8-184">Assign policies to the service code packages</span></span>
<span data-ttu-id="592b8-185">**RunAsPolicy** část **ServiceManifestImport** Určuje účet z části objekty zabezpečení, který se má použít ke spuštění balíček kódu.</span><span class="sxs-lookup"><span data-stu-id="592b8-185">The **RunAsPolicy** section for a **ServiceManifestImport** specifies the account from the principals section that should be used to run a code package.</span></span> <span data-ttu-id="592b8-186">Také přidruží kód balíčky service manifest s uživatelskými účty v části objekty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="592b8-186">It also associates code packages from the service manifest with user accounts in the principals section.</span></span> <span data-ttu-id="592b8-187">Je to zadané pro instalaci nebo hlavní vstupních bodů, nebo můžete zadat `All` chcete použít pro obojí.</span><span class="sxs-lookup"><span data-stu-id="592b8-187">You can specify this for the setup or main entry points, or you can specify `All` to apply it to both.</span></span> <span data-ttu-id="592b8-188">Následující příklad ukazuje použití různých zásad:</span><span class="sxs-lookup"><span data-stu-id="592b8-188">The following example shows different policies being applied:</span></span>

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

<span data-ttu-id="592b8-189">Pokud **entrypointtype typu** není zadán, ve výchozím nastavení je entrypointtype typu = "Hlavní".</span><span class="sxs-lookup"><span data-stu-id="592b8-189">If **EntryPointType** is not specified, the default is set to EntryPointType=”Main”.</span></span> <span data-ttu-id="592b8-190">Určení **SetupEntryPoint** je obzvláště užitečné, když chcete spustit určité operace instalace s vysokou úrovní oprávnění v rámci účtu system.</span><span class="sxs-lookup"><span data-stu-id="592b8-190">Specifying **SetupEntryPoint** is especially useful when you want to run certain high-privilege setup operations under a system account.</span></span> <span data-ttu-id="592b8-191">Aktuální služby kód může být spuštěn pod účtem nižší oprávnění.</span><span class="sxs-lookup"><span data-stu-id="592b8-191">The actual service code can run under a lower-privilege account.</span></span>

### <a name="apply-a-default-policy-to-all-service-code-packages"></a><span data-ttu-id="592b8-192">Použít výchozí zásady na všechny balíčky kódu služby</span><span class="sxs-lookup"><span data-stu-id="592b8-192">Apply a default policy to all service code packages</span></span>
<span data-ttu-id="592b8-193">Můžete použít **DefaultRunAsPolicy** části zadejte výchozí účet uživatele pro všechny kód balíčky, které nemají konkrétní **RunAsPolicy** definované.</span><span class="sxs-lookup"><span data-stu-id="592b8-193">You use the **DefaultRunAsPolicy** section to specify a default user account for all code packages that don’t have a specific **RunAsPolicy** defined.</span></span> <span data-ttu-id="592b8-194">Pokud většinu kódu balíčky, které jsou určené v service manifest používaný aplikací k potřebovali pro spuštění pod stejného uživatele, můžete aplikaci právě definovat výchozí zásady RunAs se s uživatelským účtem.</span><span class="sxs-lookup"><span data-stu-id="592b8-194">If most of the code packages that are specified in the service manifest used by an application need to run under the same user, the application can just define a default RunAs policy with that user account.</span></span> <span data-ttu-id="592b8-195">Následující příklad určuje, že pokud balíček kódu neobsahuje **RunAsPolicy** zadán, balíček kódu by měl běžet pod **MyDefaultAccount** zadané v části objekty zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="592b8-195">The following example specifies that if a code package does not have a **RunAsPolicy** specified, the code package should run under the **MyDefaultAccount** specified in the principals section.</span></span>

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a><span data-ttu-id="592b8-196">Použít skupiny domény služby Active Directory nebo uživatele</span><span class="sxs-lookup"><span data-stu-id="592b8-196">Use an Active Directory domain group or user</span></span>
<span data-ttu-id="592b8-197">Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí samostatný instalační program systému můžete spustit službu s pověřeními pro uživatele služby Active Directory nebo účet skupiny.</span><span class="sxs-lookup"><span data-stu-id="592b8-197">For an instance of Service Fabric that was installed on Windows Server by using the standalone installer, you can run the service under the credentials for an Active Directory user or group account.</span></span> <span data-ttu-id="592b8-198">Toto je služby Active Directory místně ve vaší doméně a není v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="592b8-198">This is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="592b8-199">Pomocí účtem uživatele domény nebo skupiny můžete pak přístup k jiným prostředkům v doméně (například sdílené složky), kterým bylo uděleno oprávnění.</span><span class="sxs-lookup"><span data-stu-id="592b8-199">By using a domain user or group, you can then access other resources in the domain (for example, file shares) that have been granted permissions.</span></span>

<span data-ttu-id="592b8-200">Následující příklad ukazuje uživatele služby Active Directory názvem *TestUser* s jejich domény heslo šifrované pomocí certifikátu nazývá *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="592b8-200">The following example shows an Active Directory user called *TestUser* with their domain password encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="592b8-201">Můžete použít `Invoke-ServiceFabricEncryptText` příkaz prostředí PowerShell k vytvoření tajný šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="592b8-201">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text.</span></span> <span data-ttu-id="592b8-202">V tématu [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="592b8-202">See [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md) for details.</span></span>

<span data-ttu-id="592b8-203">Je nutné nasadit privátní klíč certifikátu dešifrovat heslo k místnímu počítači pomocí metody out-of-band (v Azure, je to prostřednictvím Správce Azure Resource Manager).</span><span class="sxs-lookup"><span data-stu-id="592b8-203">You must deploy the private key of the certificate to decrypt the password to the local machine by using an out-of-band method (in Azure, this is via Azure Resource Manager).</span></span> <span data-ttu-id="592b8-204">Potom při Service Fabric nasadí balíček služby k počítači, je možné dešifrovat tajný klíč a (spolu s uživatelské jméno) ověření pomocí služby Active Directory pro spuštění pod tyto přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="592b8-204">Then, when Service Fabric deploys the service package to the machine, it is able to decrypt the secret and (along with the user name) authenticate with Active Directory to run under those credentials.</span></span>

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
### <a name="use-a-group-managed-service-account"></a><span data-ttu-id="592b8-205">Použijte skupinu účet spravované služby.</span><span class="sxs-lookup"><span data-stu-id="592b8-205">Use a Group Managed Service Account.</span></span>
<span data-ttu-id="592b8-206">Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí samostatný instalační program systému můžete spustit službu jako skupinový účet spravované služby (gMSA).</span><span class="sxs-lookup"><span data-stu-id="592b8-206">For an instance of Service Fabric that was installed on Windows Server by using the standalone installer, you can run the service as a group Managed Service Account (gMSA).</span></span> <span data-ttu-id="592b8-207">Upozorňujeme, že služby Active Directory v místě ve vaší doméně a není v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="592b8-207">Note that this is Active Directory on-premises within your domain and is not with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="592b8-208">Pomocí gMSA neexistuje žádné heslo nebo zašifrované heslo uložené v `Application Manifest`.</span><span class="sxs-lookup"><span data-stu-id="592b8-208">By using a gMSA there is no password or encrypted password stored in the `Application Manifest`.</span></span>

<span data-ttu-id="592b8-209">Následující příklad ukazuje, jak vytvořit účet gMSA s názvem *svc Test$*; pro nasazení tohoto účtu spravované služby pro uzly clusteru; a jak nakonfigurovat hlavní název uživatele.</span><span class="sxs-lookup"><span data-stu-id="592b8-209">The following example shows how to create a gMSA account called *svc-Test$*; how to deploy that managed service account to the cluster nodes; and how to configure the user principal.</span></span>

##### <a name="prerequisites"></a><span data-ttu-id="592b8-210">Požadavky.</span><span class="sxs-lookup"><span data-stu-id="592b8-210">Prerequisites.</span></span>
- <span data-ttu-id="592b8-211">Doména musí kořenový klíč služby KDS.</span><span class="sxs-lookup"><span data-stu-id="592b8-211">The domain needs a KDS root key.</span></span>
- <span data-ttu-id="592b8-212">Musí být v systému Windows Server 2012 nebo novější úroveň funkčnosti domény.</span><span class="sxs-lookup"><span data-stu-id="592b8-212">The domain needs to be at a Windows Server 2012 or later functional level.</span></span>

##### <a name="example"></a><span data-ttu-id="592b8-213">Příklad</span><span class="sxs-lookup"><span data-stu-id="592b8-213">Example</span></span>
1. <span data-ttu-id="592b8-214">Požádejte správce domény služby active directory, vytvoření účtu služby skupina spravované pomocí `New-ADServiceAccount` příkaz a ujistěte se, že `PrincipalsAllowedToRetrieveManagedPassword` zahrnuje všechny uzly clusteru service fabric.</span><span class="sxs-lookup"><span data-stu-id="592b8-214">Have an active directory domain administrator create a group managed service account using the `New-ADServiceAccount` commandlet and ensure that the `PrincipalsAllowedToRetrieveManagedPassword` includes all of the service fabric cluster nodes.</span></span> <span data-ttu-id="592b8-215">Všimněte si, že `AccountName`, `DnsHostName`, a `ServicePrincipalName` musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="592b8-215">Note that `AccountName`, `DnsHostName`, and `ServicePrincipalName` must be unique.</span></span>
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. <span data-ttu-id="592b8-216">Na všech uzlech clusteru service fabric (například `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), instalace a testování gMSA.</span><span class="sxs-lookup"><span data-stu-id="592b8-216">On each of the service fabric cluster nodes (for example, `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), install and test the gMSA.</span></span>
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. <span data-ttu-id="592b8-217">Nakonfigurovat hlavní název uživatele a nakonfigurovat RunAsPolicy tak, aby odkazovaly uživatele.</span><span class="sxs-lookup"><span data-stu-id="592b8-217">Configure the User principal, and configure the RunAsPolicy to reference the user.</span></span>
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

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a><span data-ttu-id="592b8-218">Přiřadit zásady zabezpečení přístupu pro koncové body HTTP a HTTPS</span><span class="sxs-lookup"><span data-stu-id="592b8-218">Assign a security access policy for HTTP and HTTPS endpoints</span></span>
<span data-ttu-id="592b8-219">Pokud použijete RunAs zásady pro služby a service manifest deklaruje koncový bod prostředků pomocí protokolu HTTP, je nutné zadat **SecurityAccessPolicy** zajistit, že porty přidělené s těmito koncovými body jsou správně řízení přístupu pro uživatelský účet Spustit jako, který se spouští služba uvedené.</span><span class="sxs-lookup"><span data-stu-id="592b8-219">If you apply a RunAs policy to a service and the service manifest declares endpoint resources with the HTTP protocol, you must specify a **SecurityAccessPolicy** to ensure that ports allocated to these endpoints are correctly access-control listed for the RunAs user account that the service runs under.</span></span> <span data-ttu-id="592b8-220">V opačném **http.sys** nemá přístup ke službě, a získat selhání pomocí volání z klienta.</span><span class="sxs-lookup"><span data-stu-id="592b8-220">Otherwise, **http.sys** does not have access to the service, and you get failures with calls from the client.</span></span> <span data-ttu-id="592b8-221">Následující příklad se týká účet Customer1 koncový bod názvem **EndpointName**, což dává ho úplná přístupová práva.</span><span class="sxs-lookup"><span data-stu-id="592b8-221">The following example applies the Customer1 account to an endpoint called **EndpointName**, which gives it full access rights.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

<span data-ttu-id="592b8-222">Pro koncový bod HTTPS máte také označení názvu certifikátu se vraťte do klienta.</span><span class="sxs-lookup"><span data-stu-id="592b8-222">For the HTTPS endpoint, you also have to indicate the name of the certificate to return to the client.</span></span> <span data-ttu-id="592b8-223">Můžete to provést pomocí **EndpointBindingPolicy**, s certifikátem definovaný v oddílu certifikáty v manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-223">You can do this by using **EndpointBindingPolicy**, with the certificate defined in a certificates section in the application manifest.</span></span>

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a><span data-ttu-id="592b8-224">Upgrade více aplikací s koncovými body https</span><span class="sxs-lookup"><span data-stu-id="592b8-224">Upgrading multiple applications with https endpoints</span></span>
<span data-ttu-id="592b8-225">Budete muset pečlivě nepoužívat **stejný port** pro různé instance stejné aplikace při používání protokolu http**s**.</span><span class="sxs-lookup"><span data-stu-id="592b8-225">You need to be careful not to use the **same port** for different instances of the same application when using http**s**.</span></span> <span data-ttu-id="592b8-226">Důvodem je, že Service Fabric, nebude možné upgradovat certifikátu pro jednu z instancí aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-226">The reason is that Service Fabric won't be able to upgrade the cert for one of the application instances.</span></span> <span data-ttu-id="592b8-227">Například pokud aplikace 1 nebo aplikace 2 obou chcete upgradovat jejich cert 1 cert 2.</span><span class="sxs-lookup"><span data-stu-id="592b8-227">For example, if application 1 or application 2 both want to upgrade their cert 1 to cert 2.</span></span> <span data-ttu-id="592b8-228">Při upgradu se stane, Service Fabric může mít vyčistit cert 1 registrace pomocí ovladače http.sys i když se stále používá jiná aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-228">When the upgrade happens, Service Fabric might have cleaned up the cert 1 registration with http.sys even though the other application is still using it.</span></span> <span data-ttu-id="592b8-229">Chcete-li tomu zabránit, Service Fabric zjistí, že se už používá jiná instance aplikace zaregistrované na portu s certifikátem (z důvodu http.sys) a operaci se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="592b8-229">To prevent this, Service Fabric detects that there is already another application instance registered on the port with the certificate (due to http.sys) and fails the operation.</span></span>

<span data-ttu-id="592b8-230">Proto Service Fabric nepodporuje upgrade dvě různé služby pomocí **stejný port** v případech jinou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="592b8-230">Hence Service Fabric does not support upgrading two different services using **the same port** in different application instances.</span></span> <span data-ttu-id="592b8-231">Jinými slovy nelze použít stejný certifikát na různé služby na stejném portu.</span><span class="sxs-lookup"><span data-stu-id="592b8-231">In other words, you cannot use the same certificate on different services on the same port.</span></span> <span data-ttu-id="592b8-232">Pokud potřebujete mít sdílené certifikát na stejném portu, je třeba zajistit, že služby jsou umístěny na různých počítačích s omezeními umístění.</span><span class="sxs-lookup"><span data-stu-id="592b8-232">If you need to have a shared certificate on the same port, you need to ensure that the services are placed on different machines with placement constraints.</span></span> <span data-ttu-id="592b8-233">Nebo zvažte, pokud je to možné pomocí Service Fabric dynamické porty pro každou službu v každé instanci aplikace.</span><span class="sxs-lookup"><span data-stu-id="592b8-233">Or consider using Service Fabric dynamic ports if possible for each service in each application instance.</span></span> 

<span data-ttu-id="592b8-234">Pokud se zobrazí upgradu služeb při selhání s protokolem https, chybu upozornění oznamující "Rozhraní API systému Windows HTTP serveru nepodporuje víc certifikátů pro aplikace, které sdílejí port."</span><span class="sxs-lookup"><span data-stu-id="592b8-234">If you see an upgrade fail with https, an error warning saying "The Windows HTTP Server API does not support multiple certificates for applications that share a port.”</span></span>

## <a name="a-complete-application-manifest-example"></a><span data-ttu-id="592b8-235">V příkladu manifestu aplikace dokončena</span><span class="sxs-lookup"><span data-stu-id="592b8-235">A complete application manifest example</span></span>
<span data-ttu-id="592b8-236">Následující manifest aplikace se zobrazuje řadu různých nastavení:</span><span class="sxs-lookup"><span data-stu-id="592b8-236">The following application manifest shows many of the different settings:</span></span>

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
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
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


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="592b8-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="592b8-237">Next steps</span></span>
* [<span data-ttu-id="592b8-238">Pochopení aplikačního modelu</span><span class="sxs-lookup"><span data-stu-id="592b8-238">Understand the application model</span></span>](service-fabric-application-model.md)
* [<span data-ttu-id="592b8-239">Zadejte prostředky v service manifest</span><span class="sxs-lookup"><span data-stu-id="592b8-239">Specify resources in a service manifest</span></span>](service-fabric-service-manifest-resources.md)
* [<span data-ttu-id="592b8-240">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="592b8-240">Deploy an application</span></span>](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
