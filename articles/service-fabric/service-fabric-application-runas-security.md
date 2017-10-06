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
# <a name="configure-security-policies-for-your-application"></a>Konfigurace zásad zabezpečení pro aplikaci
Pomocí Azure Service Fabric můžete zabezpečit aplikace, které jsou spuštěny v clusteru hello v rámci jiné uživatelské účty. Service Fabric také pomáhá zabezpečené hello prostředky, které jsou používány aplikací v době hello nasazení pod hello uživatelských účtů – například, soubory, adresářů a certifikáty. Díky spuštěné aplikace, i v prostředí sdílené hostované bezpečnější od sebe navzájem.

Ve výchozím nastavení aplikace Service Fabric běžet pod účtem hello, kompatibilní se proces Fabric.exe hello. Service Fabric také poskytuje hello možnost toorun aplikace pod účtem místního uživatelského účtu nebo účtu local system, který je určený v rámci hello manifest aplikace. Typy účtů podporovaný místní systém **LocalUser**, **NetworkService**, **LocalService**, a **LocalSystem**.

 Když spouštíte Service Fabric na Windows serveru ve vašem datovém centru pomocí hello samostatný instalační program systému, můžete účty domény služby Active Directory, včetně skupinové účty spravované služby.

Můžete definovat a vytvořit skupiny uživatelů, aby toobe tooeach skupiny spravují dohromady lze přidat jeden nebo více uživatelů. To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a potřebují toohave určité společné oprávnění, které jsou k dispozici na úrovni skupiny hello.

## <a name="configure-hello-policy-for-a-service-setup-entry-point"></a>Konfigurace zásad hello pro bod služby instalační položka
Jak je popsáno v hello [aplikačního modelu](service-fabric-application-model.md), hello instalace vstupní bod, **SetupEntryPoint**, je privilegované vstupního bodu, který je spuštěn s hello stejné přihlašovací údaje jako Service Fabric (obvykle hello *NetworkService* účtu) před další vstupní bod. Hello spustitelný soubor, který je zadán **EntryPoint** je obvykle hostitele služby dlouho běžící hello. Proto nutnosti samostatného instalačního vstupního bodu tomu není nutné hostitele služby toorun hello spustitelný soubor s vysokou úrovní oprávnění pro dlouhou dobu. Hello spustitelný soubor, **EntryPoint** určuje běží **SetupEntryPoint** ukončí úspěšně. Hello výsledné proces monitorovat a restartuje a znovu začíná **SetupEntryPoint** Pokud někdy ukončí nebo dojde k chybě.

Následující Hello je příklad manifestu jednoduché služby, ukazuje hello SetupEntryPoint a hello hlavní vstupní bod pro službu hello.

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

### <a name="configure-hello-policy-by-using-a-local-account"></a>Konfigurace zásad hello pomocí místního účtu
Po dokončení konfigurace služby toohave hello vstupní bod instalační program, můžete změnit hello oprávnění zabezpečení, které běží v části v manifestu aplikace hello. Hello následující příklad ukazuje, jak tooconfigure hello toorun služby v části oprávnění uživatelského účtu správce.

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

Nejprve vytvořte **objekty** část s uživatelským jménem, jako je například SetupAdminUser. To znamená, že tento hello uživatel je členem skupiny Administrators systému hello.

Dále v části hello **ServiceManifestImport** nakonfigurujte zásady tooapply tento objekt zabezpečení příliš**SetupEntryPoint**. Tato hodnota informuje Service Fabric, když hello **MySetup.bat** spuštění souboru, měla by být `RunAs` s oprávněními správce. Vzhledem k tomu, že máte *není* použít zásady toohello hlavní vstupní bod, hello kód v **MyServiceHost.exe** běží v rámci systému hello **NetworkService** účtu. Toto je hello výchozí účet, který všechny vstupní body služby jsou spustit jako.

Nyní Pojďme přidat hello souboru MySetup.bat toohello Visual Studio projektu tootest hello oprávnění správce. V sadě Visual Studio klikněte pravým tlačítkem na projekt služby hello a přidejte nový soubor s názvem MySetup.bat.

Dále zkontrolujte, zda že tento soubor MySetup.bat hello je součástí balíčku služby hello. Ve výchozím nastavení není. Vyberte soubor hello, klikněte pravým tlačítkem na tooget hello kontextovou nabídku a vyberte **vlastnosti**. V dialogové okno Vlastnosti hello, ověřte, že **zkopírujte tooOutput Directory** je nastaven příliš**kopírovat, pokud je novější**. V tématu hello následující snímek obrazovky.

![Visual Studio CopyToOutput pro SetupEntryPoint dávkového souboru][image1]

Nyní otevřete soubor MySetup.bat hello a přidejte hello následující příkazy:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set too> out.txt
echo %TestVariable% >> out.txt

REM toodelete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

V dalším kroku sestavení a nasazení hello řešení tooa místní vývojový cluster. Po spuštění služby hello, jak je znázorněno v Service Fabric Exploreru, uvidíte, že tento soubor MySetup.bat hello proběhla úspěšně dvěma způsoby. Otevřete příkazový řádek prostředí PowerShell a zadejte:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Poznamenejte si název hello hello uzlu, kde služba hello nasazená a spustit v Service Fabric Exploreru – například uzlu 2. Dále přejděte toohello aplikace instance pracovní složky toofind hello out.txt soubor, který zobrazuje hodnotu hello **TestVariable**. Například pokud tato služba byla nasazená tooNode 2, potom můžete přejít toothis cestu pro hello **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-hello-policy-by-using-local-system-accounts"></a>Konfigurace zásad hello pomocí místní systémové účty
Často je vhodnější toorun hello spouštěcí skript pomocí účtu local system, nikoli účet správce. Spuštění zásad RunAs hello jako člen skupiny Administrators obvykle nefunguje správně, protože počítače mají přístup k řízení Uživatelských účtů ve výchozím nastavení povolené hello. V takových případech **hello doporučení je toorun hello SetupEntryPoint pod účtem LocalSystem, místo jako skupinu místního uživatele přidané tooAdministrators**. Hello následující příklad ukazuje nastavení hello SetupEntryPoint toorun pod účtem LocalSystem:

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

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>Spusťte příkazy prostředí PowerShell ze vstupního bodu instalační program
toorun prostředí PowerShell z hello **SetupEntryPoint** bodu, můžete spustit **PowerShell.exe** v dávkovém souboru, který odkazuje tooa prostředí PowerShell souboru. Nejprve přidejte projekt služba prostředí PowerShell souborů toohello – například **MySetup.ps1**. Mějte na paměti, tooset hello *kopírovat, pokud je novější* vlastnost tak, že soubor hello jsou také obsaženy v balíčku služby hello. Hello následující příklad ukazuje dávkový soubor začínající soubor prostředí PowerShell s názvem MySetup.ps1, která nastavuje proměnnou prostředí systému s názvem **TestVariable**.

MySetup.bat toostart soubor prostředí PowerShell:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

V souboru prostředí PowerShell text hello přidejte následující tooset proměnné prostředí systému hello:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Ve výchozím nastavení, když hello dávkový soubor spouští, vypadá na hello aplikace složku s názvem **pracovní** pro soubory. V takovém případě při spuštění MySetup.bat chceme, aby tento toofind hello MySetup.ps1 souboru v hello stejné složky, která je aplikace hello **balíček kódu** složky. toochange tuto složku a sadu hello pracovní složku:
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

## <a name="use-console-redirection-for-local-debugging"></a>Použití konzole přesměrování pro místní ladění
V některých případech je užitečné toosee hello výstup konzoly z spuštění skriptu pro účely ladění. toodo, můžete nastavit zásady přesměrování konzoly, který zapisuje hello výstupní tooa soubor. Hello souboru výstup se nazývá složce aplikace napsané toohello **protokolu** na hello uzlu, kde je aplikace hello nasadit a spustit. (Viz kde toofind v předchozím příkladu hello.)

> [!WARNING]
> Nikdy nepoužívejte zásad přesměrování konzoly hello v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv hello aplikace převzetí služeb při selhání. *Pouze* používejte pro místní vývoj a účely ladění.  
> 
> 

Hello následující příklad ukazuje nastavení přesměrování konzoly hello s hodnotou FileRetentionCount:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Pokud změníte teď hello MySetup.ps1 soubor toowrite **Echo** příkaz, to bude zapisovat toohello výstupní soubor pro účely ladění:

```
Echo "Test console redirection which writes toohello application log folder on hello node that hello application is deployed to"
```

**Po ladění skriptu, okamžitě odebrat tuto zásadu přesměrování konzoly**.

## <a name="configure-a-policy-for-service-code-packages"></a>Nakonfigurujte zásady pro balíčky služeb kódu
V předchozích krocích hello, jste viděli, jak tooapply tooSetupEntryPoint zásad RunAs. Do jak toocreate jiné objekty, které mohou být použity jako služba zásad Podívejme trochu podrobněji.

### <a name="create-local-user-groups"></a>Vytvořit místní skupiny uživatelů
Můžete definovat a vytvořit skupiny uživatelů, které umožňují jeden nebo více uživatelů toobe přidat tooa skupiny. To je užitečné, pokud existuje více uživatelů pro různé služby vstupní body a potřebují toohave určité společné oprávnění, které jsou k dispozici na úrovni skupiny hello. Hello následující příklad ukazuje místní skupinu s názvem **LocalAdminGroup** s oprávněními správce. Dva uživatelé, Customer1 a Customer2, stanou členy této místní skupiny.

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

### <a name="create-local-users"></a>Vytvořit místní uživatele
Můžete vytvořit místní uživatele, který lze použít toohelp zabezpečení služby v rámci aplikace hello. Když **LocalUser** typ účtu zadaný v části objekty hello manifest aplikace hello, Service Fabric vytvoří místní uživatelské účty na počítačích, kde je hello aplikace nasazena. Ve výchozím nastavení nemají tyto účty hello stejné názvy jako platformám zadaným v aplikaci hello manifest (například Customer3 v hello následující ukázka). Místo toho se dynamicky generují a mít náhodných hesla.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Pokud aplikace vyžaduje, aby hello uživatelský účet a heslo být stejná ve všech počítačích (například ověřování NTLM tooenable), musíte nastavit manifestu clusteru hello NTLMAuthenticationEnabled tootrue. Hello manifestu clusteru musíte zadat také NTLMAuthenticationPasswordSecret, který se používá toogenerate hello stejné heslo ve všech počítačích.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-toohello-service-code-packages"></a>Přiřaďte zásady toohello balíčky služeb kódu
Hello **RunAsPolicy** část **ServiceManifestImport** určuje hello účet z hello objekty oddílu, který by měl být použité toorun balíček kódu. Také přidruží kód balíčky z hello service manifest s uživatelskými účty v části objekty hello. To můžete zadat pro instalační program hello nebo hlavní vstupních bodů, nebo můžete zadat `All` tooapply ho tooboth. Hello následující příklad ukazuje použití různých zásad:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Pokud **entrypointtype typu** není zadán, hello výchozí nastavení je tooEntryPointType = "Hlavní". Určení **SetupEntryPoint** je obzvláště užitečná, pokud chcete toorun určité operace instalace s vysokou úrovní oprávnění v rámci účtu system. kódu Hello skutečné služby můžete spustit pod účtem nižší oprávnění.

### <a name="apply-a-default-policy-tooall-service-code-packages"></a>Použít kód balíčky výchozí zásady tooall služeb
Použít hello **DefaultRunAsPolicy** části toospecify výchozí uživatelský účet pro všechny balíčky kódu, které nemají konkrétní **RunAsPolicy** definované. Pokud potřebujete toorun pod většinu hello kód balíčky, které jsou určené v manifestu služby hello používá jiná aplikace hello stejného uživatele, hello aplikace můžete definovat jenom výchozí zásady RunAs k tomuto účtu. Hello následující příklad určuje, že pokud balíček kódu neobsahuje **RunAsPolicy** zadán, balíček kódu hello měly být spuštěny pod hello **MyDefaultAccount** uvedený v oddílu hello objekty zabezpečení.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Použít skupiny domény služby Active Directory nebo uživatele
Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí hello samostatný instalační program systému můžete spustit služba hello hello pověření pro účet uživatele nebo skupiny služby Active Directory. Toto je služby Active Directory místně ve vaší doméně a není v Azure Active Directory (Azure AD). Pomocí účtem uživatele domény nebo skupiny můžete pak přístup k jiným prostředkům v doméně hello (například sdílené složky), kterým bylo uděleno oprávnění.

Hello následující příklad ukazuje uživatele služby Active Directory názvem *TestUser* s jejich domény heslo šifrované pomocí certifikátu nazývá *MyCert*. Můžete použít hello `Invoke-ServiceFabricEncryptText` prostředí PowerShell příkaz toocreate hello tajný šifrovaný text. V tématu [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md) podrobnosti.

Je nutné nasadit pomocí metody out-of-band hello privátní klíč hello certifikát toodecrypt hello heslo toohello místního počítače (v Azure, je to prostřednictvím Správce Azure Resource Manager). Když Service Fabric nasadí počítač toohello balíček služby hello, pak je možné toodecrypt hello tajný klíč a (spolu s hello uživatelské jméno) ověření pomocí služby Active Directory toorun pod tyto přihlašovací údaje.

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
### <a name="use-a-group-managed-service-account"></a>Použijte skupinu účet spravované služby.
Pro instance Service Fabric, která byla nainstalována v systému Windows Server pomocí hello samostatný instalační program systému můžete spustit hello služby jako skupinový účet spravované služby (gMSA). Upozorňujeme, že služby Active Directory v místě ve vaší doméně a není v Azure Active Directory (Azure AD). Pomocí gMSA není žádná hesla nebo zašifrované heslo, které jsou uložené v hello `Application Manifest`.

Hello následující příklad ukazuje, jak toocreate účet gMSA názvem *svc Test$*; jak toodeploy, který spravuje služby účet toohello uzly; a jak tooconfigure hello hlavní název uživatele.

##### <a name="prerequisites"></a>Požadavky.
- Hello domény musí kořenový klíč služby KDS.
- Hello domény musí toobe v systému Windows Server 2012 nebo novější úroveň funkčnosti.

##### <a name="example"></a>Příklad
1. Požádejte správce domény služby active directory, vytvoření účtu služby skupina spravované pomocí hello `New-ADServiceAccount` příkaz a ujistěte se, že hello `PrincipalsAllowedToRetrieveManagedPassword` zahrnuje všechny uzly clusteru prostředků infrastruktury služby hello. Všimněte si, že `AccountName`, `DnsHostName`, a `ServicePrincipalName` musí být jedinečný.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. Na každém uzlu clusteru, hello služby fabric (například `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), instalace a testování hello gMSA.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. Nakonfigurovat hlavní název uživatele hello a nakonfigurujte hello RunAsPolicy tooreference hello uživatele.
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

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Přiřadit zásady zabezpečení přístupu pro koncové body HTTP a HTTPS
Pokud použijete službu tooa zásad RunAs a hello service manifest deklaruje koncový bod prostředků s protokolem hello HTTP, je nutné zadat **SecurityAccessPolicy** tooensure, že porty přidělené toothese koncové body jsou správně řízení přístupu uvedené pro hello RunAs uživatelský účet, který spouští služba hello. V opačném **http.sys** není přístup toohello služby a získat selhání pomocí volání od klienta hello. Hello následující příklad se týká hello Customer1 tooan koncový bod účtu názvem **EndpointName**, což dává ho úplná přístupová práva.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Pro koncový bod HTTPS hello máte také název hello tooindicate hello certifikát tooreturn toohello klienta. Můžete to provést pomocí **EndpointBindingPolicy**, certifikátem hello definovaný v oddílu certifikáty v manifestu aplikace hello.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and hello Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if hello EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Upgrade více aplikací s koncovými body https
Je třeba toobe pečlivě není toouse hello **stejný port** pro různé instance hello stejnou aplikaci, když pomocí protokolu http**s**. Hello důvodem je, že Service Fabric nebude možné tooupgrade hello certifikátu pro jeden instancí aplikace hello. Například pokud aplikace 1 nebo aplikace 2 oba mají tooupgrade jejich toocert cert 1, 2. Při upgradu hello se stane, Service Fabric může mít vyčistit hello cert 1 registraci http.sys Přestože hello je stále ji používá jiná aplikace. tooprevent, Service Fabric zjistí, že již existuje jiná instance aplikace zaregistrované na portu hello certifikátem hello (kvůli toohttp.sys) a hello operace selže.

Proto Service Fabric nepodporuje upgrade dvě různé služby pomocí **hello stejný port** v případech jinou aplikaci. Jinými slovy, nemůžete použít stejný certifikát hello na různé služby na hello stejný port. Pokud potřebujete toohave sdílenou certifikátů na hello stejný port, je nutné tooensure této hello služby jsou umístěny na jiné počítače s omezeními umístění. Nebo zvažte, pokud je to možné pomocí Service Fabric dynamické porty pro každou službu v každé instanci aplikace. 

Pokud se zobrazí upgradu služeb při selhání s protokolem https, k chybě upozornění "hello rozhraní API systému Windows HTTP serveru nepodporuje víc certifikátů pro aplikace, které sdílejí port."

## <a name="a-complete-application-manifest-example"></a>V příkladu manifestu aplikace dokončena
Následující manifest aplikace Hello řadu hello ukazuje různých nastavení:

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
## <a name="next-steps"></a>Další kroky
* [Pochopení hello aplikačního modelu](service-fabric-application-model.md)
* [Zadejte prostředky v service manifest](service-fabric-service-manifest-resources.md)
* [Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
