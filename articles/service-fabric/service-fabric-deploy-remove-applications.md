---
title: "aaaAzure nasazení aplikace Service Fabric | Microsoft Docs"
description: "Jak toodeploy a odebrat aplikace v Service Fabric pomocí prostředí PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b120ffbf-f1e3-4b26-a492-347c29f8f66b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/01/2017
ms.author: ryanwi
ms.openlocfilehash: 3de9c6a937ee7b29bf9ec86d6e9e631487797507
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-remove-applications-using-powershell"></a>Nasazení a odebírat aplikace pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-deploy-remove-applications.md)
> * [Visual Studio](service-fabric-publish-app-remote-cluster.md)
> * [Rozhraní API FabricClient](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Jednou [typu aplikaci vytvořen balíček][10], je připraven k nasazení do clusteru služby Azure Service Fabric. Nasazení zahrnuje hello následující tři kroky:

1. Nahrát úložiště bitových kopií toohello balíčku aplikace hello
2. Registrace typu aplikace hello
3. Vytvoření instance aplikace hello

Jakmile je aplikace nasazená a instance běží v clusteru hello, můžete odstranit hello instanci aplikace a její typ aplikace. toocompletely odebrat aplikaci z clusteru hello zahrnuje hello následující kroky:

1. Odebrat (nebo odstranit) hello spuštěna instance aplikace
2. Zrušení registrace typu aplikace hello, pokud již nepotřebujete
3. Odebrání balíčku aplikace hello z úložiště bitových kopií hello

Pokud používáte [Visual Studio pro nasazování a ladění aplikací](service-fabric-publish-app-remote-cluster.md) na vaše místní vývojový cluster všechny předchozí kroky hello se zpracovávají automaticky pomocí skriptu prostředí PowerShell.  Tento skript se nachází v hello *skripty* složku projekt aplikace hello. Tento článek obsahuje pozadí na co skriptu je to, aby mohli provést hello stejné operace mimo Visual Studio. 
 
## <a name="connect-toohello-cluster"></a>Připojte toohello cluster
Než spustíte všechny příkazy prostředí PowerShell v tomto článku, vždy spustit pomocí [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) cluster Service Fabric toohello tooconnect. tooconnect toohello místního vývojového clusteru, spusťte následující hello:

```powershell
PS C:\>Connect-ServiceFabricCluster
```

Příklady připojující vzdáleného clusteru tooa nebo clusteru zabezpečené pomocí Azure Active Directory, X509 certifikáty nebo služby Windows Active Directory najdete v tématu [clusteru zabezpečené připojení tooa](service-fabric-connect-to-secure-cluster.md).

## <a name="upload-hello-application-package"></a>Nahrání balíčku aplikace hello
Odesílání balíčku aplikace hello vloží ho do umístění, která je přístupná pomocí interní komponenty Service Fabric.
Pokud chcete tooverify hello balíčku aplikace místně, použijte hello [Test ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.

Hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz nahrávání hello úložiště bitových kopií clusteru toohello balíčku aplikace.
Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.  tooimport hello SDK modul, spusťte:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Předpokládejme, že sestavení a balíček aplikace s názvem *Moje_aplikace* ve Visual Studiu 2015. Ve výchozím nastavení je název typu aplikace hello uvedené v hello ApplicationManifest.xml "MyApplicationType".  Hello balíček aplikace, která obsahuje manifest hello nezbytné aplikace, služby manifesty a balíčků kódu/config/data, se nachází v *C:\Users\<uživatelské jméno\>\Documents\Visual Studio 2015\Projects\ MyApplication\MyApplication\pkg\Debug*. 

Hello následující příkaz vypíše hello obsah balíčku aplikace hello:

```powershell
PS C:\> $path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Pokud balíček aplikace hello je velký nebo má mnoho souborů, můžete [je komprimovat](service-fabric-package-apps.md#compress-a-package). Hello komprese snižuje velikost hello a hello počet souborů.
Hello vedlejším účinkem je hello této registrace a zrušení registrace typu aplikace je rychlejší. Čas odeslání může být pomalejší aktuálně, zvláště pokud zahrnete hello čas toocompress hello balíčku. 

hello toocompress balíček, použijte stejný [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz. Komprese lze provést samostatné z nahrávání, pomocí hello `SkipCopy` příznak nebo společně s hello operace odesílání. Použití komprese na zkomprimovaného balíčku je žádná operace.
hello toouncompress zkomprimovaného balíčku, použijte stejný [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) s hello `UncompressPackage` přepínače.

Hello následující rutiny komprimaci hello balíček bez kopírování toohello úložiště bitových kopií. balíček Hello teď obsahuje komprimované soubory pro hello `Code` a `Config` balíčky. hello služby manifestů aplikace Hello a nejsou ZIP, protože jsou potřebné pro mnoho interních operací (např. balíček sdílení extrakci název a verze typu aplikace pro určité ověření). Pomocí formátu ZIP manifesty hello by proveďte tyto operace neefektivní.

```
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
PS C:\> tree /f $path
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

Pro velkých balíčků aplikací komprese hello trvá určitou dobu. Nejlepších výsledků dosáhnete pomocí rychlé jednotky SSD. časy Hello komprese a velikost hello hello zkomprimovaného balíčku se také liší podle hello obsah balíčku.
Zde je například komprese statistiku pro některé balíčky, které ukazují hello počáteční a hello velikost zkomprimovaného balíčku s časem komprese hello.

|Počáteční velikost (MB)|Počet souborů|Komprese čas|Zkomprimovaného balíčku velikost (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2 048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Jakmile je komprimovaný balíček, může být nahrané tooone nebo více Service Fabric clusterů podle potřeby. mechanismus pro nasazení Hello je stejný pro komprimované a nekomprimované balíčky. Pokud balíček hello je komprimován, uloží se jako takový do úložiště bitových kopií clusteru hello a je nekomprimovaným na hello uzlu před spuštěním aplikace hello.


Hello následujícím příkladu se uloží úložiště hello balíček toohello bitové kopie, do složky s názvem "MyApplicationV1":

```powershell
PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
```

Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.  tooimport hello SDK modul, spusťte:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Pokud nezadáte hello *- ApplicationPackagePathInImageStore* parametr, balíček aplikace hello zkopírován do složky "Ladění" hello v úložišti bitové kopie hello.

Hello doby potřebné tooupload balíčku se liší v závislosti na několika faktorech. Některé tyto faktory jsou hello počet souborů v balíčku hello, velikost balíčku hello a velikosti souborů hello. rychlost sítě Hello mezi hello zdrojového počítače a cluster Service Fabric hello se zároveň ovlivňuje doba nahrávání hello. Výchozí hodnota časového limitu pro Hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) je 30 minut.
V závislosti na hello popisuje faktory, může mít tooincrease hello vypršení časového limitu. Pokud jsou komprese hello balíček ve volání hello kopie, musíte tooalso zvažte hello komprese čas.

V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.

## <a name="register-hello-application-package"></a>Registrace balíčku aplikace hello
v manifestu aplikace hello k dispozici pro použití při registraci balíčku aplikace hello deklarována Hello typ a verze aplikace. Hello systému přečte nahraný v předchozím kroku hello hello balíček, ověří hello balíčku, zpracuje hello obsah balíčku a zkopíruje hello zpracovat balíček tooan systému interní umístění.  

Spustit hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) tooregister rutiny hello typu aplikace v clusteru hello a zpřístupní ji pro nasazení:

```powershell
PS C:\> Register-ServiceFabricApplicationType MyApplicationV1
Register application type succeeded
```

"MyApplicationV1" je složka hello v úložišti hello bitové kopie, kde se nachází balíček aplikace hello. Typ aplikace Hello s názvem "MyApplicationType" a verzí "1.0.0" (jak se nacházejí v manifestu aplikace hello) je nyní zaregistrovaná v clusteru hello.

Hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) příkaz vrátí jenom po hello systém obsahuje balíček aplikace hello byl úspěšně zaregistrován. Jak dlouho trvá registrace závisí na velikosti hello a obsah balíčku aplikace hello. V případě potřeby hello **- TimeoutSec** parametr může být použité toosupply delší časový limit (výchozí hodnota časového limitu hello je 60 sekund).

Pokud máte velké aplikací, balíčků nebo pokud dochází k vypršení časových limitů, použijte hello **- asynchronní** parametr. příkaz Hello vrátí při hello clusteru přijme příkaz hello registrace a zpracování hello pokračuje podle potřeby.
Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace. Tento příkaz toodetermine můžete použít, pokud se provádí registraci hello.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="create-hello-application"></a>Vytvoření aplikace hello
Můžete vytvořit instanci aplikace z libovolné verze typu aplikace, který byl úspěšně zaregistrován pomocí hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) rutiny. Hello názvu každé aplikace musí začínat hello *"fabric:"* scheme a musí být jedinečný pro každou instanci aplikace. Všechny výchozí služby definované v manifestu aplikace hello typu hello cílové aplikace jsou také vytvořit.

```powershell
PS C:\> New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```
Pro danou verzi typu zaregistrovanou aplikaci lze vytvořit více instancí aplikace. Každá instance aplikace spouští v izolaci, s vlastní pracovní adresář a proces.

toosee, které s názvem aplikace a služby běží v clusteru hello, spusťte hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) a [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) rutiny:

```powershell
PS C:\> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}

PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Odebrání aplikace
Instance aplikace už je potřeba, trvale odstraníte ho podle názvu pomocí hello [odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) rutiny. [Odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) automaticky odstraní všechny služby, které patří toohello aplikace také a trvale odebrat všechny služby stavu. 

> [!WARNING]
> Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.

```powershell
PS C:\> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS C:\> Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Typ aplikace se zrušit registraci
Pokud konkrétní verzi typ aplikace se už nepotřebuje, by měl zrušit registraci typu aplikace hello pomocí hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) rutiny. Zrušení registrace nepoužívané aplikace typy verzích prostor úložiště používá úložiště bitových kopií hello. Typ aplikace může být registrace, dokud u ní jsou vytvořeny žádné aplikace a žádné čekající na vyřízení aplikace upgrady na něj odkazují.

Spustit [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) typy aplikací hello toosee aktuálně registrované v clusteru hello:

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Spustit [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) toounregister konkrétní typ aplikace:

```powershell
PS C:\> Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="remove-an-application-package-from-hello-image-store"></a>Balíček aplikace odebrat z úložiště bitových kopií hello
Balíček aplikace už je potřeba, odstraňte jej z hello image store toofree až systémové prostředky.

```powershell
PS C:\>Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
```

## <a name="troubleshooting"></a>Řešení potíží
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopírování ServiceFabricApplicationPackage požádá o parametr ImageStoreConnectionString
prostředí Service Fabric SDK Hello byste již měli mít hello správný, nastavit výchozí hodnoty. Ale v případě potřeby pro všechny příkazy hello parametr ImageStoreConnectionString musí odpovídat hello hodnotu této hello používá cluster Service Fabric. Hello parametr ImageStoreConnectionString můžete najít v manifestu clusteru hello načten pomocí hello [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) a Get-ImageStoreConnectionStringFromClusterManifest příkazy:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Hello **Get-ImageStoreConnectionStringFromClusterManifest** rutiny, která je součástí modulu Service Fabric SDK PowerShell text hello, je použít tooget hello image uložit připojovací řetězec.  tooimport hello SDK modul, spusťte:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Parametr ImageStoreConnectionString Hello se nachází v manifestu clusteru hello:

```xml
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

V tématu [pochopit hello image store připojovací řetězec](service-fabric-image-store-connection-string.md) doplňující informace o hello úložiště image store a image uložit připojovací řetězec.

### <a name="deploy-large-application-package"></a>Nasazení balíčku velké aplikace
Problém: [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) časového limitu pro balíček rozsáhlé aplikace (pořadí GB).
Vyzkoušejte:
- Zadat delší časový limit pro [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkazu s `TimeoutSec` parametr. Ve výchozím nastavení je hello časový limit 30 minut.
- Zkontrolujte hello síťové připojení mezi zdrojovým počítačem a clusteru. Pokud jde o pomalé připojení hello, zvažte použití na počítači s lepší síťové připojení.
Pokud hello klientský počítač je v jiné oblasti než hello clusteru, zvažte použití klientský počítač v oblasti blíže nebo stejné jako hello cluster.
- Zkontrolujte, pokud jste nedosáhli externí omezení. Například v případě je úložiště image hello nakonfigurované toouse azure storage, může nahrávání omezeny.

Problém: Nahrávání balíček úspěšně dokončit, ale [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) vyprší časový limit. Vyzkoušejte:
- [Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií.
Hello komprese snižuje velikost hello a musíte provést hello počet souborů, která zase snižuje hello objem provozu a že Service Fabric fungovat. operace nahrávání Hello může být pomalejší (obzvláště pokud zahrnete hello komprese čas), ale registrace a zrušení registrace typu aplikace hello je rychlejší.
- Zadat delší časový limit pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) s `TimeoutSec` parametr.
- Zadejte `Async` přepínač pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). příkaz Hello vrátí, když hello clusteru přijme příkaz hello a hello registrace typu aplikace hello asynchronně. Z tohoto důvodu není bez nutnosti toospecify vyšší vypršení časového limitu v tomto případě. Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace. Tento příkaz toodetermine můžete použít, pokud se provádí registraci hello.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Nasazení balíčku aplikace s mnoho souborů
Problém: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) časového limitu pro balíček aplikace s mnoha souborech (pořadí tisíc).
Vyzkoušejte:
- [Komprimovat hello balíček](service-fabric-package-apps.md#compress-a-package) před kopírováním toohello úložiště bitových kopií. Hello komprese snižuje hello počet souborů.
- Zadat delší časový limit pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) s `TimeoutSec` parametr.
- Zadejte `Async` přepínač pro [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps). příkaz Hello vrátí, když hello clusteru přijme příkaz hello a hello registrace typu aplikace hello asynchronně.
Z tohoto důvodu není bez nutnosti toospecify vyšší vypršení časového limitu v tomto případě. Hello [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) příkaz vypíše všechny verze typů byl úspěšně zaregistrován aplikace a jejich stav registrace. Tento příkaz toodetermine můžete použít, pokud se provádí registraci hello.

```powershell
PS C:\> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Další kroky
[Upgrade aplikace Service Fabric](service-fabric-application-upgrade.md)

[Úvod stavu Service Fabric](service-fabric-health-introduction.md)

[Diagnostika a řešení služby Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Model aplikace v Service Fabric](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
