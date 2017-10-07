---
title: "aaaDeploy existující spustitelné tooAzure Service Fabric | Microsoft Docs"
description: "Návod na způsobu nasazení toopackage existující aplikace jako spustitelný hosta, aby ho bylo možné tooa cluster Service Fabric"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: 5599802bdb6bda2407a138d77e12148ccb64f437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-guest-executable-tooservice-fabric"></a>Nasazení spustitelné tooService hosta prostředků infrastruktury
Jakýkoli typ kódu, třeba Node.js, Java nebo C++ v Azure Service Fabric můžete spustit jako služby. Service Fabric označuje toothese typů služeb jako hosta spustitelné soubory.

Spustitelné soubory hosta budou vyhodnocené jako bezstavové služby Service Fabric. V důsledku toho jsou umístěny na uzlech v clusteru, na základě dostupnosti a další metriky. Tento článek popisuje, jak toopackage a nasadit cluster hostů spustitelné tooa Service Fabric pomocí Visual Studio nebo nástroj příkazového řádku.

V tomto článku jsme zahrnují hello kroky toopackage hosta spustitelný soubor a nasaďte ji tooService prostředků infrastruktury.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Výhody spuštění spustitelného souboru v Service Fabric Host
Existuje několik výhod toorunning spustitelný soubor v clusteru Service Fabric Host:

* Vysoká dostupnost. Aplikace, které běží v Service Fabric jsou vysoce dostupné. Service Fabric zajistí, že instance aplikace běží.
* Sledování stavu. Monitorování stavu Service Fabric zjišťuje, zda aplikace běží a poskytuje diagnostické informace, pokud dojde k selhání.   
* Správa životního cyklu aplikací. Kromě toho poskytuje upgrady bez výpadků, Service Fabric nabízí automatického vrácení zpět toohello předchozí verze, pokud existuje událost stavu chybný hlášené během upgradu.    
* Hustotou. Více aplikací můžete spustit v clusteru, která eliminuje potřebu hello toorun každou aplikaci na svůj vlastní hardware.
* Možnosti rozpoznání: Pomocí REST můžete volat hello Service Fabric Naming service toofind další služby v clusteru hello. 

## <a name="samples"></a>Ukázky
* [Ukázka pro balení a nasazení spustitelný soubor hosta](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Přehled aplikace a soubory manifestu služby
Jako součást nasazení spustitelný soubor hosta, je užitečné toounderstand hello Service Fabric balení a nasazení modelu jak je popsáno v [aplikačního modelu](service-fabric-application-model.md). model balení Service Fabric Hello spoléhá na dva soubory XML: hello manifestů aplikace a služby. Hello definice schématu pro hello ApplicationManifest.xml a ServiceManifest.xml soubory se instaluje s hello Service Fabric SDK do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Manifest aplikace** manifest aplikace hello je použité toodescribe hello aplikace. Vypíše hello služby, které ji tvoří a dalších parametrů, které jsou používané toodefine musí být nasazené jak jednu nebo více služeb, jako je například hello počet instancí.

  Aplikace v Service Fabric je jednotka nasazení a upgrade. Aplikace lze upgradovat jako na jednu jednotku, kde se spravují potenciální chyby a potenciální odvolání. Service Fabric zaručuje, že proces upgradu hello je buď úspěšné, nebo pokud hello upgrade selže, nenechává aplikace hello v neznámý nebo nestabilním stavu.
* **Manifest služby** hello service manifest popisuje hello součásti služby. Obsahuje data, jako třeba hello název a typ služby a kódu a konfigurace. Hello service manifest také zahrnuje některé další parametry, které se dají použít tooconfigure hello služby po jejím nasazení.

## <a name="application-package-file-structure"></a>Struktura souboru balíčku aplikace
toodeploy aplikaci tooService prostředků infrastruktury, aplikace hello by mělo vycházet předdefinované adresářovou strukturu. Hello následuje příklad této struktury.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

Hello ApplicationPackageRoot obsahuje hello ApplicationManifest.xml soubor, který definuje aplikace hello. Podadresář pro každou službu součástí aplikace hello je použité toocontain všechny hello artefakty, pomocí kterých hello služba vyžaduje. Tyto podadresáře jsou hello ServiceManifest.xml a obvykle hello následující:

* *Kód*. Tento adresář obsahuje kódu služby hello.
* *Konfigurace*. Tento adresář obsahuje soubor souborech Settings.xml (a další soubory v případě potřeby), služba hello můžete získat přístup v modulu runtime tooretrieve specifické nastavení.
* *Data*. Toto je další adresář toostore další místní data, která může být nutné hello služby. Data by měla být pouze dočasné dat použité toostore. Service Fabric nemá zkopírovat nebo replikovat změny toohello datový adresář, pokud služba hello musí toobe přemístění (například během převzetí služeb při selhání).

> [!NOTE]
> Nemáte toocreate hello `config` a `data` adresářů, pokud je nepotřebujete.
>
>

## <a name="package-an-existing-executable"></a>Balíček existující spustitelný soubor
Při balení spustitelný soubor hosta, můžete zvolit buď toouse šablona projektu sady Visual Studio nebo příliš[ručně vytvořit balíček aplikace hello](#manually). Pomocí sady Visual Studio, hello struktura balíček aplikace a soubory manifestu vytvářejí hello nová šablona projektu pro vás.

> [!TIP]
> Nejjednodušší způsob, jak toopackage Hello existující spustitelný soubor do služby systému Windows je toouse Visual Studio a na Linux toouse Yeoman
>

## <a name="use-visual-studio-toopackage-and-deploy-an-existing-executable"></a>Pomocí sady Visual Studio toopackage a nasadit existující spustitelný soubor
Visual Studio poskytuje Service Fabric toohelp šablony služby nasadit cluster hostů spustitelné tooa Service Fabric.

1. Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.
2. Zvolte **spustitelný soubor hosta** jako šablonu služby hello.
3. Klikněte na tlačítko **Procházet** tooselect hello složka se spustitelný soubor a vyplňte hello zbytek hello parametry toocreate hello služby.
   * *Kód balíčku chování*. Může být sada toocopy všechny hello obsah vaší složky toohello projekt sady Visual Studio, což je užitečné, pokud hello spustitelný soubor se nemění. Pokud očekávat toochange hello spustitelný soubor a dynamicky chcete hello možnost toopick si nových sestavení automaticky, můžete toolink toohello složky místo. Propojené složek můžete použít při vytváření projektu aplikace hello v sadě Visual Studio. Tím propojí toohello umístění zdroje z v rámci projektu hello, která umožňuje vám tooupdate hello hosta spustitelný soubor v jeho zdroj cíl. Tyto aktualizace se stanou součástí balíčku aplikace hello na sestavení.
   * *Program* určuje hello spustitelný soubor, který by měl být spuštěn toostart hello služby.
   * *Argumenty* určuje hello argumenty, které mají být předány toohello spustitelný soubor. Může být seznam parametrů s argumenty.
   * *WorkingFolder* určuje hello pracovní adresář pro hello proces, který se bude toobe spuštěna. Můžete určit tří hodnot:
     * `CodeBase`Určuje, že pracovní adresář hello přechází toobe nastavit adresář toohello kódu v balíčku aplikace hello (`Code` directory uvedené v předcházející strukturu souborů hello).
     * `CodePackage`Určuje, že pracovní adresář hello přechází toobe nastavit toohello kořenové balíčku aplikace hello (`GuestService1Pkg` uvedené v předcházející strukturu souborů hello).
     * `Work`Určuje, že hello soubory jsou umístěny v podadresáři volat pracovní.
4. Zadejte název služby a klikněte na **OK**.
5. Pokud vaše služba musí koncový bod pro komunikaci, můžete nyní přidat hello protokol, port a typ toohello ServiceManifest.xml souboru. Například: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Teď můžete použít balíček hello a publikovat akce s místním clusteru tak, že ladění hello řešení v sadě Visual Studio. Až bude připravený, můžete publikovat hello aplikace tooa vzdálený cluster nebo zkontrolujte v ovládacím prvku toosource řešení hello.
7. Jak přejděte toohello konci tohoto článku toosee tooview hosta spustitelný soubor služby spuštěné v Service Fabric Exploreru.

## <a name="use-yoeman-toopackage-and-deploy-an-existing-executable-on-linux"></a>Používat Yoeman toopackage a nasaďte existující spustitelný soubor v systému Linux

Hello postup pro vytvoření a nasazení hosta spustitelného souboru v systému Linux je hello stejné jako nasazení aplikace csharp nebo java.

1. V terminálu zadejte `yo azuresfguest`.
2. Pojmenujte svoji aplikaci.
3. Název služby a zadejte podrobnosti hello včetně cestu hello spustitelný soubor a hello parametry, které musí být volána s.

Yeoman vytvoří balíček aplikace hello příslušné aplikace a soubory manifestu spolu s instalaci a odinstalaci skripty.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Ručně zabalení a nasazení existující spustitelný soubor
proces Hello ručně balení spustitelný soubor hosta je založena na hello následující obecné kroky:

1. Vytvořte strukturu adresáře balíčku hello.
2. Přidejte kód a konfigurační soubory aplikace hello.
3. Upravte soubor manifestu služby hello.
4. Upravte soubor manifestu aplikace hello.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you toocreate hello ApplicationPackage automatically. hello tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-hello-package-directory-structure"></a>Vytvořit strukturu adresáře balíčku hello
Vytvořením hello adresářovou strukturu, můžete spustit jak je popsáno v předcházející části hello "Aplikace balíčku souboru struktura."

### <a name="add-hello-applications-code-and-configuration-files"></a>Přidání kódu a konfigurační soubory aplikace hello
Po vytvoření hello adresářovou strukturu, můžete přidat aplikace hello kódu a konfigurační soubory v hello kódu a konfigurace adresářů. Můžete také vytvořit další adresáře nebo podadresáře v hello kódu nebo konfiguračního adresáře.

Service Fabric nemá `xcopy` hello obsahu z hello kořenového adresáře aplikace, takže není k dispozici žádné předdefinované struktura toouse než vytváření dva hlavní adresáře, kód a nastavení. (Můžete si vybrat odlišné názvy podle potřeby. Další podrobnosti najdete v další části hello.)

> [!NOTE]
> Ujistěte se, jestli jste zahrnuli všechny soubory hello a závislosti, které hello potřebám aplikace. Service Fabric zkopíruje hello obsahu balíčku aplikace hello na všech uzlech v clusteru hello kde hello aplikační služby jsou toobe probíhající nasazení. Hello balíček měl obsahovat všechny hello kódu, že aplikace hello musí toorun. Nepředpokládejte, že hello závislosti jsou již nainstalovány.
>
>

### <a name="edit-hello-service-manifest-file"></a>Upravit soubor manifestu služby hello
dalším krokem Hello je tooedit hello služby souboru manifestu tooinclude hello následující informace:

* Hello název typu služby hello. Toto je ID, že Service Fabric používá tooidentify služby.
* Hello příkaz toouse toolaunch hello aplikace (ExeHost).
* Žádný skript, který potřebuje toobe spustit tooset si aplikace hello (SetupEntrypoint).

Hello tady je příklad `ServiceManifest.xml` souboru:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Následující části Hello projít různé části hello souboru, je nutné, aby tooupdate hello.

#### <a name="update-servicetypes"></a>Aktualizace ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* Můžete vybrat libovolný název, který chcete použít pro `ServiceTypeName`. Hodnota Hello se používá v hello `ApplicationManifest.xml` tooidentify hello služba souborů.
* Zadejte `UseImplicitHost="true"`. Tento atribut informuje Service Fabric, že služba hello je založena na samostatná aplikace, takže všechny Service Fabric musí toodo toolaunch ji jako proces a sledovat jeho stav.

#### <a name="update-codepackage"></a>CodePackage aktualizace
Hello CodePackage element určuje umístění hello (a verzi) kódu služby hello.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Hello `Name` element je použité toospecify hello název adresáře hello v balíčku aplikace hello, který obsahuje kódu služby hello. `CodePackage`má také hello `version` atribut. To může být použité toospecify hello verze hello kódu a taky potenciálně lze používat kódu služby hello tooupgrade pomocí infrastruktury pro správu životního cyklu aplikace hello v Service Fabric.

#### <a name="optional-update-setupentrypoint"></a>Volitelné: Aktualizace SetupEntrypoint
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Hello SetupEntryPoint element je použité toospecify spustitelný soubor nebo dávkového souboru, který se má provést před spuštění kódu služby hello. Je volitelný krok, takže nemusí toobe zahrnuty, pokud neexistuje žádné inicializace požadována. Hello SetupEntryPoint se spustí pokaždé, když hello restartována.

Se jenom jeden SetupEntryPoint, takže skripty instalace potřebovat toobe seskupené do jednoho dávkový soubor, pokud instalace aplikace hello vyžaduje několik skriptů. Hello SetupEntryPoint může spustit libovolný typ souboru: spustitelné soubory, dávkové soubory a rutiny prostředí PowerShell. Další podrobnosti najdete v tématu [konfigurace SetupEntryPoint](service-fabric-application-runas-security.md).

V předchozím příkladu hello, hello SetupEntryPoint běží dávky soubor s názvem `LaunchConfig.cmd` který je umístěn v hello `scripts` podadresáři adresář kódu hello (za předpokladu, že hello WorkingFolder element je nastaven tooCodeBase).

#### <a name="update-entrypoint"></a>EntryPoint aktualizace
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Hello `EntryPoint` element v souboru manifestu služby hello je použité toospecify jak toolaunch hello služby. Hello `ExeHost` element určuje hello spustitelného souboru (a argumenty) který by měl být použit toolaunch hello služby.

* `Program`Určuje název hello hello spustitelný soubor, který by měl spustit službu hello.
* `Arguments`Určuje hello argumenty, které mají být předány toohello spustitelný soubor. Může být seznam parametrů s argumenty.
* `WorkingFolder`Určuje pracovní adresář hello hello procesu, který se bude toobe spuštěna. Můžete určit tří hodnot:
  * `CodeBase`Určuje, že pracovní adresář hello přechází toobe nastavit adresář toohello kódu v balíčku aplikace hello (`Code` adresář v hello předcházející strukturu souborů).
  * `CodePackage`Určuje, že pracovní adresář hello přechází toobe nastavit toohello kořenové balíčku aplikace hello (`GuestService1Pkg` v hello předcházející strukturu souborů).
    * `Work`Určuje, že hello soubory jsou umístěny v podadresáři volat pracovní.

Hello WorkingFolder je užitečné tooset hello správné pracovní adresář tak, aby relativní cesty mohou být využívána buď hello aplikace nebo inicializace skripty.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Aktualizovat koncové body a zaregistrovat u služby DNS pro komunikaci
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
V předchozím příkladu hello, hello `Endpoint` element určuje hello koncových bodů, které může aplikace hello naslouchat. V tomto příkladu aplikace Node.js hello naslouchá na protokolu http na portu 3000.

Kromě toho můžete pokládat Service Fabric toopublish toohello tento koncový bod služby pojmenování tak další služby můžete zjistit hello koncový bod adresy toothis služby. To vám umožní mít toocommunicate toobe mezi službami, které jsou hostované spustitelné soubory.
Hello adresa publikované koncového bodu má hello podobu `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`a `PathSuffix` jsou volitelných atributů. `IPAddressOrFQDN`je hello IP adresu nebo plně kvalifikovaný název domény tohoto spustitelného souboru získá umístěny na uzlu hello a se vypočítává za vás.

V hello následující příklad, jednou hello služby je nasazen, v Service Fabric Explorer zobrazí podobné koncový bod příliš`http://10.1.4.92:3000/myapp/` publikována pro instance služby hello. Nebo pokud se jedná se o místní počítač, zobrazí `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Můžete použít tyto adresy s [reverse proxy](service-fabric-reverseproxy.md) toocommunicate mezi službami.

### <a name="edit-hello-application-manifest-file"></a>Upravte soubor manifestu aplikace hello
Po konfiguraci hello `Servicemanifest.xml` souboru, je nutné toomake některé změny toohello `ApplicationManifest.xml` souboru tooensure, který hello správné, používají typ služby a název.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
V hello `ServiceManifestImport` elementu, můžete zadat jednu nebo více služeb, které chcete tooinclude v aplikaci hello. Služby jsou odkazovány pomocí `ServiceManifestName`, která určuje název hello hello adresáře, kde hello `ServiceManifest.xml` se nachází soubor.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Nastavení protokolování
Pro hosta spustitelné soubory je užitečné toobe možné toosee konzoly protokoly toofind out-li zobrazit všechny chyby hello aplikace a konfigurační skripty.
Přesměrování konzoly se dá nakonfigurovat v hello `ServiceManifest.xml` soubor pomocí hello `ConsoleRedirection` elementu.

> [!WARNING]
> Nikdy nepoužívejte zásad přesměrování konzoly hello v aplikaci, která je nasazena v produkčním prostředí, protože to může mít vliv hello aplikace převzetí služeb při selhání. *Pouze* používejte pro místní vývoj a účely ladění.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection`lze použít tooredirect konzoly výstupního (stdout a stderr) tooa pracovní adresář. To poskytuje tooverify hello možnost, že nejsou žádné chyby během hello instalaci nebo spuštění aplikace hello v clusteru Service Fabric hello.

`FileRetentionCount`Určuje, kolik souborů se ukládají do hello pracovní adresář. Hodnota 5, například znamená, že hello soubory protokolu pro předchozí pět spuštěních hello se ukládají v hello pracovní adresář.

`FileMaxSizeInKb`Určuje maximální velikost hello hello protokolových souborů.

Ukládání souborů protokolu v jednom z hello služby pracovních adresářích. toodetermine, kde jsou umístěny, hello soubory pomocí Service Fabric Explorer toodetermine služby hello uzlu, která běží na a pracovní adresář, kterému je používán. Tento proces je popsaná později v tomto článku.

## <a name="deployment"></a>Nasazení
posledním krokem Hello je příliš[nasazení aplikace](service-fabric-deploy-remove-applications.md). Dobrý den zobrazí skript prostředí PowerShell následující jak toodeploy vaší aplikace toohello místního vývojového clusteru a spusťte novou službu Service Fabric.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitové kopie, pokud balíček hello je velký nebo má mnoho souborů. Další informace [zde](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Služba Service Fabric se dá nasadit v různých "konfigurace". Například se můžete nasadit jako jeden nebo více instancí, nebo se můžete nasadit tak, že existuje jedna instance služby hello v každém uzlu clusteru Service Fabric hello.

Hello `InstanceCount` parametr hello `New-ServiceFabricService` rutina je použité toospecify kolik instancí služby hello musí být spuštěna v clusteru Service Fabric hello. Můžete nastavit hello `InstanceCount` hodnotu, v závislosti na typu hello aplikace, kterou nasazujete. Hello dva nejběžnější scénáře jsou:

* `InstanceCount = "1"`. V takovém případě pouze jednu instanci služby hello je nasazen v clusteru hello. Scheduler Service Fabric určuje služby, která uzlu hello přechází toobe nasazené na.
* `InstanceCount ="-1"`. V takovém případě jednu instanci služby hello je nasazen na každý uzel v clusteru Service Fabric hello. výsledek Hello má jeden (a pouze jeden) instanci hello služby pro každý uzel v clusteru hello.

Toto je užitečné konfiguraci pro front-endu aplikace (například koncový bod REST), protože klientské aplikace potřebují příliš "připojit" tooany hello uzlů v hello koncový bod hello toouse clusteru. Tato konfigurace mohou sloužit také při, například všechny uzly clusteru Service Fabric hello jsou připojené tooa nástroj pro vyrovnávání zatížení. Přenosy klienta lze následně distribuovat mezi hello služba, která běží na všech uzlech v clusteru hello.

## <a name="check-your-running-application"></a>Zkontrolujte spuštěné aplikace
V Service Fabric Exploreru Identifikujte hello uzlu, kde je spuštěna služba hello. V tomto příkladu je spuštěna na Uzel1:

![Uzel, na kterém je spuštěna služba](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Pokud přejdete toohello uzlu a procházet toohello aplikace, zobrazí hello uzlu nezbytné informace, včetně jeho umístění na disku.

![Umístění na disku](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Pokud adresář toohello pomocí Průzkumníka serveru, najdete hello pracovní adresář a složku protokolu hello služby, jak ukazuje následující snímek obrazovky hello: 

![Umístění protokolu](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Další kroky
V tomto článku jste se naučili jak toopackage spustitelný soubor hosta a nasaďte ho tooService prostředků infrastruktury. Viz následující články pro související informace a úlohy hello.

* [Ukázka pro balení a nasazení spustitelný soubor hosta](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), včetně toohello odkaz předběžné verze nástroje balení hello
* [Ukázka dvěma hosta spustitelné soubory (C# a nodejs) komunikaci přes hello pojmenování službu pomocí REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
* [Nasazení několika hostujících spustitelných souborů](service-fabric-deploy-multiple-apps.md)
* [Vytvoření první aplikace Service Fabric pomocí sady Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
