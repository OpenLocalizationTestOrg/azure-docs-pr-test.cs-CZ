---
title: "Služba Fabric a nasazení kontejnerů v systému Linux | Microsoft Docs"
description: "Service Fabric a použití kontejnery Linux k nasazení aplikací mikroslužby. Tento článek popisuje možnosti, které poskytuje služby infrastruktury pro kontejnery a nasazení bitové kopie kontejneru Linux do clusteru"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-linux-container-to-service-fabric"></a>Nasadit kontejner Linux do Service Fabric
> [!div class="op_single_selector"]
> * [Nasazení kontejneru systému Windows](service-fabric-deploy-container.md)
> * [Nasazení kontejneru Linux](service-fabric-deploy-container-linux.md)
>
>

Tento článek vás provede procesem vytváření kontejnerizované služeb v kontejnerech Docker v systému Linux.

Service Fabric má několik kontejneru funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb, která jsou kontejnerizované. Tyto služby se nazývají kontejnerové služby.

Schopnosti zahrnují;

* Nasazení bitové kopie kontejneru a aktivace
* Zásady správného řízení prostředků
* Úložiště ověřování
* Port kontejneru na mapování portů hostitele
* Zjišťování – kontejnery a komunikace
* Možnost konfigurace a nastavení proměnných prostředí

## <a name="packaging-a-docker-container-with-yeoman"></a>Balení kontejner docker s yeoman
Při balení kontejner v systému Linux, můžete buď používat šablony yeoman nebo [ručně vytvořit balíček aplikace](#manually).

Aplikace Service Fabric může obsahovat jeden nebo více kontejnerů, každý s určitou roli při poskytování funkcí aplikace. Sada Service Fabric SDK pro Linux zahrnuje generátor [Yeoman](http://yeoman.io/), který usnadňuje vytvoření aplikace a přidání image kontejneru. Použijme Yeomana k vytvoření aplikace *SimpleContainerApp* s jediným kontejnerem Dockeru. Můžete přidat že další služby později úpravou vygenerovaného manifest soubory.

## <a name="install-docker-on-your-development-box"></a>Nainstalujte na vaše pole vývoj Docker

Spusťte následující příkazy instalace docker na vaše pole vývoj Linux (Pokud používáte vagrant bitové kopie na OSX, docker je již nainstalována):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a>Vytvoření aplikace
1. V terminálu zadejte `yo azuresfcontainer`.
2. Název aplikace - například mycontainerap
3. Zadejte adresu URL pro kontejner bitovou kopii z DockerHub úložišti. Parametr image má podobu [úložišti] / [název image]
4. Pokud bitovou kopii nemá zatížení vstupní bod definované, pak je třeba explicitně zadat vstupní příkazy s oddělovači sadu příkazů pro spuštění uvnitř kontejneru, který uchová kontejneru spuštění po spuštění.

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="deploy-the-application"></a>Nasazení aplikace

### <a name="using-xplat-cli"></a>Použití XPlat CLI
Jakmile je aplikace sestavená, můžete ji nasadit do místního clusteru pomocí rozhraní příkazového řádku Azure.

1. Připojte se k místnímu clusteru služby Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Pomocí instalačního skriptu, který je součástí šablony, zkopírujte balíček aplikace do úložiště imagí clusteru, zaregistrujte typ aplikace a vytvořte její instanci.

    ```bash
    ./install.sh
    ```

3. Otevřete prohlížeč a přejdete k Service Fabric Exploreru na adrese http://localhost:19080/Explorer (pokud používáte Vagrant v Mac OS X, místo localhost použijte privátní IP adresu virtuálního počítače).
4. Rozbalte uzel Aplikace a všimněte si, že už obsahuje položku pro váš typ aplikace a další položku pro první instanci tohoto typu.
5. Odinstalační skript pro zadané v šabloně použijte k odstranění instance aplikace a zrušení registrace typu aplikace.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Použití Azure CLI 2.0

Na správu v tématu referenční dokumentace [životního cyklu aplikace pomocí Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).

Ukázkovou aplikaci [ukázky najdete v článku věnovaném kód kontejneru Service Fabric na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="adding-more-services-to-an-existing-application"></a>Přidání více služeb do stávající aplikace

Pro přidání do aplikace již vytvořené pomocí jiné služby kontejneru `yo`, proveďte následující kroky:

1. Změňte adresář na kořenovou složku stávající aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je aplikace vytvořená pomocí Yeomanu.
2. Spusťte `yo azuresfcontainer:AddService`.

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

Můžete zadat vstupní příkazy zadáním volitelného `Commands` element s oddělovači sadu příkazů pro spuštění uvnitř kontejneru.

> [!NOTE]
> Pokud bitovou kopii nemá zatížení vstupní bod definované, pak je třeba explicitně zadat vstupní příkazy uvnitř `Commands` element s oddělovači sadu příkazů pro spuštění uvnitř kontejneru, který uchová kontejneru spuštění po spuštění.

## <a name="understand-resource-governance"></a>Pochopení zásad správného řízení prostředků
Zásady správného řízení prostředků je funkce kontejneru, který omezuje prostředky, které můžete použít kontejneru na hostiteli. `ResourceGovernancePolicy`, Které je určené v manifestu aplikace se používá k deklaraci limitů prostředků pro balíček kódu služby. Omezení prostředků můžete nastavit pro následující prostředky:

* Memory (Paměť)
* MemorySwap
* CpuShares (Relativní váha procesoru)
* MemoryReservationInMB  
* BlkioWeight (BlockIO relativní váhy).

> [!NOTE]
> V budoucí verzi, podporu pro konkrétní bloku limity vstupně-výstupní operace jako je například IOPs, zadání počtu bitů za Sekundu pro čtení a zápis a ostatní budou zahrnuty.
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

## <a name="configure-container-port-to-host-port-mapping"></a>Konfigurace mapování port kontejneru hostitele a portu
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
Pomocí `PortBinding` zásady, můžete namapovat port kontejneru na `Endpoint` v service manifest. Koncový bod `Endpoint1` můžete zadat pevné číslo portu (například port 80). Ho můžete také zadat žádné port vůbec, v takovém případě je pro vás zvolen náhodných portu z rozsahu portů aplikace clusteru.

Pokud zadáte koncový bod, pomocí `Endpoint` značky v service manifest kontejner hosta, Service Fabric můžete automaticky publikovat tento koncový bod služby pojmenování. Dalším službám, které jsou spuštěny v clusteru může zjišťovat proto tento kontejner pomocí dotazů REST pro řešení.

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

Když si zaregistrujete ve službě pojmenování, snadno to komunikace kontejnery v kódu v rámci vašeho kontejneru pomocí [reverzní proxy](service-fabric-reverseproxy.md). Komunikace se provádí zadáním naslouchající port http reverzní proxy server a název služby, které chcete ke komunikaci s jako proměnné prostředí. Další informace najdete v další části.

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavení proměnných prostředí
Proměnné prostředí lze zadat pro každý balíček kódu v service manifest, jak pro služby, které jsou nasazeny v kontejnerech nebo pro služby, které jsou nasazeny jako spustitelné soubory procesy nebo hosta. Tyto hodnoty proměnné prostředí můžete přepsat konkrétně v manifestu aplikace nebo zadávají během nasazení jako parametry aplikace.

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
* [Komunikace s clustery Service Fabric pomocí rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)
