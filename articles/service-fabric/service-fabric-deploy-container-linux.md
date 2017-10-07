---
title: "aaaService prostředků infrastruktury a nasazení kontejnerů v systému Linux | Microsoft Docs"
description: "Service Fabric a hello pomocí aplikace pro Linux kontejnery toodeploy mikroslužby. Tento článek popisuje hello možnosti, které Service Fabric nabízí pro kontejnery a jak toodeploy kontejner Linux bitové kopie do clusteru"
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
ms.openlocfilehash: e28f99a145b0594d871b0ec0566233a7ad235ce8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-linux-container-tooservice-fabric"></a>Nasazení Linux kontejneru tooService prostředků infrastruktury
> [!div class="op_single_selector"]
> * [Nasazení kontejneru systému Windows](service-fabric-deploy-container.md)
> * [Nasazení kontejneru Linux](service-fabric-deploy-container-linux.md)
>
>

Tento článek vás provede procesem vytváření kontejnerizované služeb v kontejnerech Docker v systému Linux.

Service Fabric má několik kontejneru funkcí, které vám pomůžou s vytváření aplikací, které se skládají z mikroslužeb, která jsou kontejnerizované. Tyto služby se nazývají kontejnerové služby.

Hello schopnosti zahrnují;

* Nasazení bitové kopie kontejneru a aktivace
* Zásady správného řízení prostředků
* Úložiště ověřování
* Mapování portů toohost port kontejneru
* Zjišťování – kontejnery a komunikace
* Možnost tooconfigure a nastavení proměnných prostředí

## <a name="packaging-a-docker-container-with-yeoman"></a>Balení kontejner docker s yeoman
Při balení kontejner v systému Linux, můžete zvolit buď toouse šablonu yeoman nebo [ručně vytvořit balíček aplikace hello](#manually).

Aplikace Service Fabric může obsahovat jeden nebo více kontejnerů, každý s určitou roli při poskytování funkcí aplikace hello. zahrnuje Hello Service Fabric SDK pro Linux [Yeoman](http://yeoman.io/) generátor, který umožňuje snadno toocreate vaší aplikace a přidat bitovou kopii kontejneru. Umožňuje použít Yeoman toocreate názvem aplikace s jediný kontejner Docker *SimpleContainerApp*. Další služby můžete přidat později úpravou hello generované manifestu souborů.

## <a name="install-docker-on-your-development-box"></a>Nainstalujte na vaše pole vývoj Docker

Hello spusťte následující příkazy tooinstall docker na vaše pole vývoj Linux (Pokud používáte hello vagrant image na OSX, docker je již nainstalována):

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-hello-application"></a>Vytvoření aplikace hello
1. V terminálu zadejte `yo azuresfcontainer`.
2. Název aplikace - například mycontainerap
3. Zadejte adresu URL hello hello kontejneru bitové kopie z DockerHub úložišti. Hello image parametr trvá hello formuláře [úložišti] / [název image]
4. Pokud hello obrázek nemá zatížení vstupní bod definované, pak musíte tooexplicitly zadat vstupní příkazy sadu příkazů toorun uvnitř hello kontejneru, který uchová hello kontejneru spuštění po spuštění oddělených čárkou.

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="deploy-hello-application"></a>Nasazení aplikace hello

### <a name="using-xplat-cli"></a>Použití XPlat CLI
Po hello aplikace, můžete ho nasadit toohello místní cluster pomocí hello rozhraní příkazového řádku Azure.

1. Připojte toohello místní cluster Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Použití hello nainstalujete skript zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.

    ```bash
    ./install.sh
    ```

3. Otevřete prohlížeč a přejděte tooService Fabric Explorer na http://localhost: 19080/Explorer (nahraďte localhost s privátní IP hello hello virtuálních počítačů, když Vagrant na Mac OS X).
4. Rozbalte uzel aplikace hello a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.
5. Odinstalační skript pro použití hello k dispozici v instanci aplikace hello šablony toodelete hello a zrušení registrace typu aplikace hello.

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a>Použití Azure CLI 2.0

Na správu najdete v části hello referenční dokumentace [životní cyklus aplikace pomocí Azure CLI 2.0 hello](service-fabric-application-lifecycle-azure-cli-2-0.md).

Ukázkovou aplikaci [ukázky najdete v článku věnovaném hello kód kontejneru Service Fabric na Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="adding-more-services-tooan-existing-application"></a>Přidání další služby tooan existující aplikace

tooadd jiný kontejner služby již vytvořené pomocí aplikace tooan `yo`, proveďte následující kroky hello:

1. Změnit kořenový adresář toohello hello existující aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.
2. Spusťte `yo azuresfcontainer:AddService`.

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

Můžete zadat vstupní příkazy zadáním hello volitelné `Commands` element sadu příkazů toorun uvnitř kontejneru hello oddělených čárkou.

> [!NOTE]
> Pokud hello obrázek nemá zatížení vstupní bod definované, pak musíte tooexplicitly zadat vstupní příkazy uvnitř `Commands` element sadu příkazů toorun uvnitř hello kontejneru, který uchová hello kontejneru spuštění po oddělený čárkami spuštění.

## <a name="understand-resource-governance"></a>Pochopení zásad správného řízení prostředků
Zásady správného řízení prostředků je na hostiteli hello můžete použít funkce hello kontejneru, který omezuje hello prostředky, které hello kontejneru. Hello `ResourceGovernancePolicy`, které je určené v manifestu aplikace hello je použité toodeclare prostředků limity pro balíček kódu služby. Omezení prostředků lze nastavit pro hello následující prostředky:

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

## <a name="configure-container-port-to-host-port-mapping"></a>Konfigurace mapování port kontejneru hostitele a portu
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
Pomocí hello `PortBinding` zásady, můžete namapovat port kontejneru tooan `Endpoint` v hello service manifest. Hello koncový bod `Endpoint1` můžete zadat pevné číslo portu (například port 80). Toho může také specifikovat žádné port vůbec, v takovém případě je zvolen náhodných portu z rozsahu portů aplikace hello clusteru pro vás.

Pokud zadáte koncový bod, pomocí hello `Endpoint` značky v service manifest hello kontejneru hosta, Service Fabric můžete automaticky publikovat tento koncový bod toohello Naming service. Dalším službám, které jsou spuštěny v clusteru hello může zjišťovat proto tento kontejner pomocí hello REST dotazů pro řešení.

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

Po registraci hello Naming service, snadno to komunikace kontejnery v hello kódu v rámci vašeho kontejneru pomocí hello [reverse proxy](service-fabric-reverseproxy.md). Komunikace se provádí zadáním naslouchající port http hello reverzní proxy server a název hello hello služeb, které chcete toocommunicate s jako proměnné prostředí. Další informace najdete v tématu hello další části.

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavení proměnných prostředí
Proměnné prostředí lze zadat pro každý balíček kódu v manifestu hello služby, jak pro služby, které jsou nasazeny v kontejnerech nebo pro služby, které jsou nasazeny jako spustitelné soubory procesy nebo hosta. Tyto hodnoty proměnné prostředí můžete přepsat konkrétně v manifestu aplikace hello nebo zadat během nasazování jako parametry aplikace.

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
* [Interakci s clusterů Service Fabric pomocí hello rozhraní příkazového řádku Azure](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a>Související články

* [Začínáme s platformou Service Fabric a Azure CLI 2.0](service-fabric-azure-cli-2-0.md)
* [Začínáme se Service Fabric XPlat CLI](service-fabric-azure-cli.md)
