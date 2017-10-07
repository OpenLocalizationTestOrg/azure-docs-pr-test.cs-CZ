---
title: "aaaService prostředků infrastruktury a nasazení kontejnerů | Microsoft Docs"
description: "Service Fabric a hello používat kontejnery toodeploy mikroslužbu aplikací. Tento článek popisuje hello možnosti, které Service Fabric nabízí pro kontejnery a jak toodeploy kontejner Windows image do clusteru."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: 8b6540579641474f21b8712b56049c7d177bec26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-windows-container-tooservice-fabric"></a>Nasazení tooService kontejneru Windows Fabric
> [!div class="op_single_selector"]
> * [Nasazení kontejneru systému Windows](service-fabric-deploy-container.md)
> * [Nasadit kontejner Docker](service-fabric-deploy-container-linux.md)
> 
> 

Tento článek vás provede procesem vytváření kontejnerizované služeb v kontejnerech Windows hello proces.

Service Fabric má několik funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb běžících v rámci kontejnery. 

Hello schopnosti zahrnují:

* Nasazení bitové kopie kontejneru a aktivace
* Zásady správného řízení prostředků
* Úložiště ověřování
* Mapování port kontejneru hostitele a portu
* Zjišťování – kontejnery a komunikace
* Možnost tooconfigure a nastavení proměnných prostředí

Podíváme, jak každý z možností funguje, když jste balení kontejnerizované služby toobe, zahrnout do vaší aplikace.

## <a name="package-a-windows-container"></a>Balíček Windows kontejneru
Když vytvoříte balíček kontejner, můžete toouse buď šablony sady Visual Studio projektu nebo [ručně vytvořit balíček aplikace hello](#manually).  Pokud používáte Visual Studio, struktura balíčku aplikace hello a souborů manifestu vytvoří se nový projekt šablonou hello.

> [!TIP]
> Nejjednodušší způsob, jak toopackage Hello stávající image kontejneru do služby je toouse Visual Studio.

## <a name="use-visual-studio-toopackage-an-existing-container-image"></a>Pomocí sady Visual Studio toopackage stávající image kontejneru
Visual Studio poskytuje Service Fabric toohelp šablony služby můžete nasadit cluster Service Fabric tooa kontejneru.

1. Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.
2. Zvolte **hosta kontejneru** jako šablonu služby hello.
3. Zvolte **název bitové kopie** a zadejte hello cesta toohello bitové kopie v kontejneru úložiště. Například `myrepo/myimage:v1` v https://hub.docker.com
4. Zadejte název služby a klikněte na **OK**.
5. Pokud vaše kontejnerizované služba musí koncový bod pro komunikaci, můžete nyní přidat hello protokol, port a typ toohello ServiceManifest.xml souboru. Například: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Tím, že poskytuje hello `UriScheme`, Service Fabric automaticky zaregistruje koncový bod hello kontejneru se hello Naming service pro možnosti rozpoznání. Hello port můžete být fixed (jak je uvedeno v předchozím příkladu hello) nebo přidělí dynamicky. Pokud port neurčíte, se přidělí dynamicky z rozsahu portů aplikace hello (protože by se stalo s jakoukoli službu,).
    Musíte taky mapování portů tooconfigure hello kontejneru toohost zadáním `PortBinding` zásad v manifestu aplikace hello. Další informace najdete v tématu [konfigurace mapování portů toohost kontejneru](#Portsection).
6. Pokud vaše kontejneru musí zásad správného řízení prostředků můžete přidat `ResourceGovernancePolicy`.
8. Pokud vaše kontejneru potřebuje tooauthenticate s privátní úložiště, přidejte `RepositoryCredentials`.
7. Pokud používáte v systému Windows Server 2016 počítač s povolenou podporou kontejneru, můžete použít balíček hello a publikovat akce toodeploy tooyour místní cluster. 
8. Až bude připravený, můžete publikovat hello aplikace tooa vzdálený cluster nebo zkontrolujte v ovládacím prvku toosource řešení hello. 

Příklad najdete v článku věnovaném hello [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="creating-a-windows-server-2016-cluster"></a>Vytvoření clusteru systému Windows Server 2016
toodeploy kontejnerizované aplikace, budete potřebovat toocreate clusteru se systémem Windows Server 2016 s podporou kontejneru povolena. Cluster může být spuštěn místně nebo nasadit pomocí Azure Resource Manageru v Azure. 

toodeploy clusteru pomocí Azure Resource Manager, zvolte hello **systému Windows Server 2016 s kontejnery** bitové kopie možnost v Azure. Najdete v článku hello [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Ujistěte se, že používáte hello následující nastavení Azure Resource Manager:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Můžete taky hello [šablony pět uzlu Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate clusteru. Můžete také načíst komunita [příspěvku na blogu](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) na použití kontejnerů Service Fabric a Windows.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Ručně zabalení a nasazení bitové kopie kontejneru
proces Hello ručně balení kontejnerové služby je založena na hello následující kroky:

1. Publikujte hello kontejnery tooyour úložiště.
2. Vytvořte strukturu adresáře balíčku hello.
3. Upravte soubor manifestu služby hello.
4. Upravte soubor manifestu aplikace hello.

## <a name="deploy-and-activate-a-container-image"></a>Nasazení a aktivaci bitovou kopii kontejneru
V hello Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky. toodeploy a aktivujte kontejner a put hello název obrázku kontejneru hello do `ContainerHost` element v hello service manifest.

V manifestu hello služby, přidat `ContainerHost` pro hello vstupní bod. Potom sadu hello `ImageName` toobe hello název hello kontejneru úložiště a bitové kopie. Hello následující částečné manifest ukazuje příklad jak toodeploy hello kontejner nazývá `myimage:v1` z úložiště volána `myrepo`:

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

Můžete určit volitelné příkazy toorun při spuštění hello kontejneru hello `Commands` elementu. Pro více příkazů čárkami vymezení je. 

## <a name="understand-resource-governance"></a>Pochopení zásad správného řízení prostředků
Zásady správného řízení prostředků je na hostiteli hello můžete použít funkce hello kontejneru, který omezuje hello prostředky, které hello kontejneru. Hello `ResourceGovernancePolicy`, které je určené v manifestu aplikace hello je použité toodeclare prostředků limity pro balíček kódu služby. Omezení prostředků lze nastavit pro hello následující prostředky:

* Memory (Paměť)
* MemorySwap
* CpuShares (Relativní váha procesoru)
* MemoryReservationInMB  
* BlkioWeight (BlockIO relativní váhy).

> [!NOTE]
> Podpora pro zadání konkrétní blok limity vstupně-výstupní operace, jako je IOPs, počtu bitů za Sekundu pro čtení a zápis a dalších plánujeme pro budoucí použití.
> 
> 

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a>Ověření úložiště
toodownload kontejner, můžete mít tooprovide přihlašovací údaje toohello kontejner úložiště. Hello přihlašovací údaje, zadaný v manifestu aplikace hello, jsou použité toospecify hello přihlašovací údaje nebo klíč SSH pro stahování hello kontejneru image z úložiště imagí hello. Hello následující příklad ukazuje účtu nazvaného *TestUser* společně s hello heslo jako prostý text (*není* doporučená):

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

Doporučujeme, abyste šifrování hesla hello pomocí certifikátu, která nasadila toohello počítače.

Hello následující příklad ukazuje účtu nazvaného *TestUser*, kde hello hesla byla zašifrována pomocí certifikát nazvaný *MyCert*. Můžete použít hello `Invoke-ServiceFabricEncryptText` prostředí PowerShell příkaz toocreate hello tajný šifrovaný text hello hesla. Další informace najdete v článku hello [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md).

privátní klíč Hello hello certifikátu, který byl použit toodecrypt hello heslo musí být nasazené toohello v metodu out-of-band místního počítače. (V Azure, tato metoda je Azure Resource Manager.) Pak když Service Fabric nasadí počítač toohello balíček služby hello, ho dešifrovat tajný klíč hello. Pomocí hello tajný klíč společně s názvem účtu hello můžete poté ověřit pomocí hello kontejner úložiště.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name ="Portsection"></a>Konfigurace mapování portů toohost kontejneru
Můžete nakonfigurovat toocommunicate port používaný na hostiteli s hello kontejneru zadáním `PortBinding` v manifestu aplikace hello. Hello port vazby mapy hello port toowhich hello služba naslouchá uvnitř port tooa hello kontejneru na hostiteli hello.

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a>Konfigurace zjišťování kontejnery a komunikace
Můžete použít hello `PortBinding` element toomap tooan koncový bod port kontejneru v hello service manifest. V následujícím příkladu hello, hello koncový bod `Endpoint1` určuje pevný port, 8905. Toho může také specifikovat žádné port vůbec, v takovém případě je zvolen náhodných portu z rozsahu portů aplikace hello clusteru pro vás.


```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```
Pokud zadáte koncový bod, pomocí hello `Endpoint` značky v service manifest hello kontejneru hosta, Service Fabric můžete automaticky publikovat tento koncový bod toohello Naming service. Dalším službám, které jsou spuštěny v clusteru hello může zjišťovat proto tento kontejner pomocí hello REST dotazů pro řešení.

Po registraci hello Naming service, můžete provádět-kontejnery komunikace v rámci vašeho kontejneru pomocí hello [reverse proxy](service-fabric-reverseproxy.md). Komunikace se provádí zadáním naslouchající port http hello reverzní proxy server a název hello hello služeb, které chcete toocommunicate s jako proměnné prostředí. Další informace najdete v tématu hello další části. 

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavení proměnných prostředí
Pro každý balíček kódu v hello service manifest lze zadat proměnné prostředí. Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host. Proměnné prostředí hodnoty v aplikaci hello manifest nebo je zadat během nasazování jako parametry aplikace můžete přepsat.

Hello následující služby manifestu XML fragment kódu ukazuje příklad toospecify proměnných prostředí pro balíček kódu:

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

Tyto proměnné prostředí je možné přepsat na úrovni manifestu aplikace hello:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

V předchozím příkladu hello jsme zadali explicitní hodnotu pro hello `HttpGateway` proměnnou prostředí (19000), když jsme nastavit hodnotu hello `BackendServiceName` parametr prostřednictvím hello `[BackendSvc]` parametr aplikace. Tato nastavení umožňují hodnotu hello toospecify `BackendServiceName`hodnota při nasazení aplikace hello a v manifestu hello nemá pevnou hodnotu.

## <a name="configure-isolation-mode"></a>Konfigurace režimu izolace

Systém Windows podporuje dva režimy izolace kontejnery - proces a technologie Hyper-V.  V režimu izolace procesu hello hello všechny kontejnery hello systémem stejné hostitele počítače sdílenou složku hello jádra s hostitelem hello. V režimu izolace hello technologie Hyper-V jsou izolovány. mezi každou kontejneru technologie Hyper-V a hostitelem kontejneru hello hello jádra. režimu izolace Hello je uveden v hello `ContainerHostPolicies` značky v souboru manifestu aplikace hello.  režim Hello izolaci, které lze zadat `process`, `hyperv`, a `default`. Hello `default` režimu izolace výchozí příliš`process` v systému Windows Server hostuje a použije se výchozí hodnota příliš`hyperv` na hostitelích s Windows 10.  Hello následující fragment kódu ukazuje, jak je režimu izolace hello zadaný v souboru manifestu aplikace hello.

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a>Dokončit příklady pro aplikace a manifest služby

Manifest aplikace například takto:

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

Manifest službu příkladu (zadané v předchozím manifest aplikace hello) zahrnuje:

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a>Další kroky
Teď, když jste nasadili kontejnerové služby, zjistěte, jak toomanage životního cyklu načtením [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).

* [Přehled Service Fabric a kontejnery](service-fabric-containers-overview.md)
* Příklad najdete v článku věnovaném [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
