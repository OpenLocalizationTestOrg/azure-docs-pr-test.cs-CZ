---
title: "Service Fabric a nasazení kontejnerů | Microsoft Docs"
description: "Service Fabric a používání kontejnerů k nasazení aplikací mikroslužby. Tento článek popisuje možnosti, které poskytuje služby infrastruktury pro kontejnery a nasazení bitové kopie kontejneru systému Windows do clusteru."
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
ms.openlocfilehash: 25d6b056421e71fa70ed20a39589f77dbbc25c69
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-windows-container-to-service-fabric"></a>Nasazení kontejneru systému Windows pro Service Fabric
> [!div class="op_single_selector"]
> * [Nasazení kontejneru systému Windows](service-fabric-deploy-container.md)
> * [Nasadit kontejner Docker](service-fabric-deploy-container-linux.md)
> 
> 

Tento článek vás provede procesem vytváření kontejnerizované služeb v systému Windows kontejnerech.

Service Fabric má několik funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb běžících v rámci kontejnery. 

Schopnosti zahrnují:

* Nasazení bitové kopie kontejneru a aktivace
* Zásady správného řízení prostředků
* Úložiště ověřování
* Mapování port kontejneru hostitele a portu
* Zjišťování – kontejnery a komunikace
* Možnost konfigurace a nastavení proměnných prostředí

Podíváme, jak každou z možností funguje, když jste balení kontejnerové služby mají být zahrnuty do vaší aplikace.

## <a name="package-a-windows-container"></a>Balíček Windows kontejneru
Když vytvoříte balíček kontejner, můžete použít buď šablony sady Visual Studio projektu nebo [ručně vytvořit balíček aplikace](#manually).  Pokud používáte Visual Studio, struktury balíček aplikace a soubory manifestu jsou vytvořeny šablonou nový projekt pro vás.

> [!TIP]
> Nejjednodušší způsob, jak zabalit stávající image kontejneru do služby je pomocí sady Visual Studio.

## <a name="use-visual-studio-to-package-an-existing-container-image"></a>Balíček stávající image kontejneru pomocí sady Visual Studio
Visual Studio poskytuje šablony služby Service Fabric vám pomůžou nasadit kontejner pro cluster Service Fabric.

1. Zvolte **soubor** > **nový projekt**a vytvářet aplikace Service Fabric.
2. Zvolte **hosta kontejneru** jako šablonu služby.
3. Zvolte **název bitové kopie** a zadejte cestu k bitové kopii v úložišti kontejneru. Například `myrepo/myimage:v1` v https://hub.docker.com
4. Zadejte název služby a klikněte na **OK**.
5. Pokud služby kontejnerizované potřebuje koncový bod pro komunikaci, můžete teď přidejte protokol, port a typ souboru ServiceManifest.xml. Například: 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    Tím, že poskytuje `UriScheme`, Service Fabric automaticky zaregistruje koncový bod kontejneru službou pojmenování pro možnosti rozpoznání. Port můžete být fixed (jak je uvedeno v předchozím příkladu) nebo přidělí dynamicky. Pokud port neurčíte, se přidělí dynamicky z rozsahu portů aplikace (protože by se stalo s jakoukoli službu,).
    Musíte také nakonfigurovat kontejner, aby služba mapování portů hostitele zadáním `PortBinding` zásad v manifestu aplikace. Další informace najdete v tématu [kontejneru konfigurace k mapování portů hostitele](#Portsection).
6. Pokud vaše kontejneru musí zásad správného řízení prostředků můžete přidat `ResourceGovernancePolicy`.
8. Pokud se váš kontejner potřebuje ověřovat v privátním úložišti, přidejte `RepositoryCredentials`.
7. Pokud používáte v systému Windows Server 2016 počítač s povolenou podporou kontejneru, můžete použít balíček a publikovat akce k nasazení na místním clusteru. 
8. Až bude připravený, můžete publikovat aplikaci do vzdáleného clusteru nebo řešení správy zdrojového kódu se změnami. 

Příklad najdete v článku věnovaném [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="creating-a-windows-server-2016-cluster"></a>Vytvoření clusteru systému Windows Server 2016
Chcete-li nasadit kontejnerizované aplikace, vytvoření clusteru se systémem Windows Server 2016 s povolenou podporou kontejneru. Cluster může být spuštěn místně nebo nasadit pomocí Azure Resource Manageru v Azure. 

Chcete-li nasadit cluster pomocí Azure Resource Manager, zvolte **systému Windows Server 2016 s kontejnery** bitové kopie možnost v Azure. Najdete v článku [vytvořit cluster Service Fabric pomocí Azure Resource Manager](service-fabric-cluster-creation-via-arm.md). Ujistěte se, že používáte následující nastavení Azure Resource Manager:

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
Můžete také [šablony pět uzlu Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) k vytvoření clusteru. Můžete také načíst komunita [příspěvku na blogu](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) na použití kontejnerů Service Fabric a Windows.

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a>Ručně zabalení a nasazení bitové kopie kontejneru
Proces ručně balení kontejnerové služby podle následujících kroků:

1. Kontejnery publikujte do úložiště.
2. Vytvořte strukturu adresáře balíčku.
3. Upravte soubor manifestu služby.
4. Upravte soubor manifestu aplikace.

## <a name="deploy-and-activate-a-container-image"></a>Nasazení a aktivaci bitovou kopii kontejneru
V Service Fabric [aplikačního modelu](service-fabric-application-model.md), kontejner představuje hostitele aplikace ve více služby, která jsou umístěna repliky. K nasazení a aktivaci kontejner, uveďte název kontejneru bitové kopie do `ContainerHost` element v service manifest.

V manifestu služby, přidat `ContainerHost` pro vstupní bod. Nastavte `ImageName` jako název kontejneru úložiště a bitové kopie. Následující částečné manifest ukazuje příklad nasazení kontejneru názvem `myimage:v1` z úložiště volána `myrepo`:

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

Můžete zadat volitelné příkazy ke spuštění při spuštění kontejneru `Commands` elementu. Pro více příkazů čárkami vymezení je. 

## <a name="understand-resource-governance"></a>Pochopení zásad správného řízení prostředků
Zásady správného řízení prostředků je funkce kontejneru, který omezuje prostředky, které můžete použít kontejneru na hostiteli. `ResourceGovernancePolicy`, Které je určené v manifestu aplikace se používá k deklaraci limitů prostředků pro balíček kódu služby. Omezení prostředků můžete nastavit pro následující prostředky:

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
Pokud chcete stáhnout kontejner, možná muset zadat přihlašovací údaje k úložišti kontejneru. Přihlašovací údaje, zadaný v manifestu aplikace, se používají a zadejte přihlašovací údaje, nebo klíč SSH pro stažení image kontejneru z úložiště imagí. Následující příklad ukazuje účtu nazvaného *TestUser* společně s heslo jako prostý text (*není* doporučená):

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

Doporučujeme, abyste heslo šifrovat pomocí certifikátu, který je nasazen do počítače.

Následující příklad ukazuje účtu nazvaného *TestUser*, kde byla zašifrována heslem pomocí certifikát nazvaný *MyCert*. Můžete použít `Invoke-ServiceFabricEncryptText` příkaz prostředí PowerShell k vytvoření tajný šifrovaného textu pro heslo. Další informace najdete v článku [Správa tajných klíčů v Service Fabric aplikace](service-fabric-application-secret-management.md).

Privátní klíč certifikátu, který se používá k dešifrování hesla musí být nasazený v metodu out-of-band v místním počítači. (V Azure, tato metoda je Azure Resource Manager.) Pak když Service Fabric nasadí balíček služby k počítači, může dešifrovat tajný klíč. Pomocí tajný klíč s názvem účtu může pak ověřit s úložištěm kontejneru.

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

## <a name ="Portsection"></a>Konfigurace kontejner, aby služba mapování portů hostitele
Můžete nakonfigurovat port hostitele používá ke komunikaci s kontejneru zadáním `PortBinding` v manifestu aplikace. Vazbou portu mapuje port, na kterém služba naslouchá uvnitř kontejneru na port na hostiteli.

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
Můžete použít `PortBinding` elementu, který chcete namapovat port kontejneru na koncový bod v service manifest. V následujícím příkladu, koncový bod `Endpoint1` určuje pevný port, 8905. Ho můžete také zadat žádné port vůbec, v takovém případě je pro vás zvolen náhodných portu z rozsahu portů aplikace clusteru.


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
Pokud zadáte koncový bod, pomocí `Endpoint` značky v service manifest kontejner hosta, Service Fabric můžete automaticky publikovat tento koncový bod služby pojmenování. Dalším službám, které jsou spuštěny v clusteru může zjišťovat proto tento kontejner pomocí dotazů REST pro řešení.

Tím, že zaregistrujete ve službě pojmenování, můžete provádět-kontejnery komunikace v rámci vašeho kontejneru pomocí [reverse proxy](service-fabric-reverseproxy.md). Komunikace se provádí zadáním naslouchající port http reverzní proxy server a název služby, které chcete ke komunikaci s jako proměnné prostředí. Další informace najdete v další části. 

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavení proměnných prostředí
Proměnné prostředí je možné zadat pro každý balíček kódu v manifestu služby. Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host. Hodnoty proměnných prostředí můžete přepsat v manifestu aplikace, nebo je můžete zadat v průběhu nasazení jako parametry aplikace.

Následující fragment kódu XML manifestu služby ukazuje příklad toho, jak zadat proměnné prostředí pro balíček kódu:

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

Tyto proměnné prostředí je možné přepsat na úrovni manifestu aplikace:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

V předchozím příkladu jsme zadali explicitní hodnotu pro `HttpGateway` proměnnou prostředí (19000), když jsme nastavit hodnotu pro `BackendServiceName` parametr prostřednictvím `[BackendSvc]` parametr aplikace. Tato nastavení umožňují zadat hodnotu pro `BackendServiceName`hodnota při nasazení aplikace a nemá pevnou hodnotu v manifestu.

## <a name="configure-isolation-mode"></a>Konfigurace režimu izolace

Systém Windows podporuje dva režimy izolace kontejnery - proces a technologie Hyper-V.  V režimu izolace procesů všechny kontejnery spuštěné na stejném hostitelském počítači sdílejí jádro s hostitelem. V režimu izolace Hyper-V se jádra pro jednotlivé kontejnery Hyper-V a hostitele kontejneru izolují. Režimu izolace je uveden v `ContainerHostPolicies` značky v souboru manifestu aplikace.  Je možné zadat tyto režimy izolace: `process`, `hyperv` a `default`. `default` Výchozí nastavení režimu izolace `process` v hostitelích Windows Server a výchozí hodnota je `hyperv` na hostitelích s Windows 10.  Následující fragment kódu ukazuje, jakým způsobem je režim izolace určený v souboru manifestu aplikace.

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

Následuje manifest službu příkladu (zadané v předchozím manifest aplikace):

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
Teď, když jste nasadili kontejnerové služby, zjistěte, jak Správa životního cyklu načtením [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md).

* [Přehled Service Fabric a kontejnery](service-fabric-containers-overview.md)
* Příklad najdete v článku věnovaném [ukázky kódu Service Fabric kontejneru na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)
