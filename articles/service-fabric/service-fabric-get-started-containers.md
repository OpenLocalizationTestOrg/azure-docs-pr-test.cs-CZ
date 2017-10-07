---
title: aaaCreate kontejneru aplikace Azure Service Fabric | Microsoft Docs
description: "Vytvoříte svou první aplikaci typu kontejner pro Windows na platformě Azure Service Fabric.  Sestavení Docker bitové kopie s aplikací Python, push hello image tooa kontejneru registru, sestavení a nasazení aplikace Service Fabric kontejneru."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/18/2017
ms.author: ryanwi
ms.openlocfilehash: b79d3a41eb2da5f7791266588fe9ea7becb0e58f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Vytvoření první aplikace Service Fabric typu kontejner v systému Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Spuštění existující aplikace v kontejneru systému Windows na cluster Service Fabric nevyžaduje žádné změny tooyour aplikace. Tento článek vás provede procesem vytváření obrazem Docker obsahující Python [Flask](http://flask.pocoo.org/) webové aplikace a jeho nasazení tooa cluster Service Fabric.  Kontejnerizovanou aplikaci budete také sdílet prostřednictvím služby [Azure Container Registry](/azure/container-registry/).  Tento článek předpokládá základní znalost Dockeru. Můžete další informace o Docker ve čtení hello [Docker přehled](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Požadavky
Vývojový počítač s:
* Visual Studio 2015 nebo Visual Studio 2017.
* [Sada Service Fabric SDK a nástroje](service-fabric-get-started.md).
*  Docker pro Windows.  [Získejte Docker CE pro Windows (stabilní verze)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Po instalaci a spuštění Docker, klikněte pravým tlačítkem na ikonu hello na hlavním panelu a vyberte **přepínač kontejnery tooWindows**. Toto je požadované toorun Docker Image založené na systému Windows.

Cluster Windows se třemi nebo více uzly spuštěnými na Windows Serveru 2016 s kontejnery – [Vytvořte cluster](service-fabric-cluster-creation-via-portal.md) nebo [vyzkoušejte Service Fabric zdarma](https://aka.ms/tryservicefabric).

Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure.

## <a name="define-hello-docker-container"></a>Zadejte kontejner Docker hello
Vytvořit bitovou kopii podle hello [Python image](https://hub.docker.com/_/python/) nachází na úložiště Docker Hub.

Definujte kontejner Dockeru v souboru Dockerfile. Hello soubor Docker obsahuje pokyny pro nastavení prostředí hello uvnitř vaší kontejneru, načítání aplikace hello chcete toorun a mapování portů. Hello soubor Docker je vstupní toohello hello `docker build` příkaz, který vytváří hello bitovou kopii.

Vytvoření prázdného adresáře a vytvoření souboru hello *soubor Docker* (se žádné přípony souboru). Přidejte následující hello příliš*soubor Docker* a uložte změny:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set hello working directory too/app
WORKDIR /app

# Copy hello current directory contents into hello container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available toohello world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when hello container launches
CMD ["python", "app.py"]
```

Čtení hello [odkaz na soubor Docker](https://docs.docker.com/engine/reference/builder/) Další informace.

## <a name="create-a-simple-web-application"></a>Vytvoření jednoduché webové aplikace
Vytvořte webovou aplikaci Flask, která naslouchá na portu 80 a vrací „Hello World!“.  V hello stejný adresář, vytvořte soubor hello *requirements.txt*.  Přidejte následující hello a uložit provedené změny:
```
Flask
```

Vytvořit také hello *app.py* souboru a přidejte následující hello:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():

    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

<a id="Build-Containers"></a>
## <a name="build-hello-image"></a>Sestavení hello bitové kopie
Spustit hello `docker build` příkaz toocreate hello image, která spouští webovou aplikaci. Otevřete okno prostředí PowerShell a přejděte toohello adresář obsahující hello soubor Docker. Spusťte následující příkaz hello:

```
docker build -t helloworldapp .
```

Tento příkaz sestavení hello novou bitovou kopii pomocí pokynů hello v váš soubor Docker pojmenování (-t označování) image hello "helloworldapp". Vytváření image vrátí základní image hello z úložiště Docker Hub a vytvoří novou bitovou kopii, která přidává aplikace nad hello základní bitovou kopii.  

Po dokončení příkazu sestavení hello spustit hello `docker images` příkaz toosee informace o novou bitovou kopii hello:

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello
Bitové kopie ověřte místně před odesláním ji hello kontejneru registru.  

Spuštění aplikace hello:

```
docker run -d --name my-web-site helloworldapp
```

*název* poskytuje název toohello, systémem kontejneru (namísto ID kontejneru hello).

Jakmile se spustí hello kontejneru, najděte jeho IP adresu, aby mohl připojit tooyour systémem kontejneru z prohlížeče:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Připojte toohello systémem kontejneru.  Otevřete webový prohlížeč odkazující toohello IP adresu vrácenou, například "http://172.31.194.61". Měli byste vidět hello záhlaví "Hello, World!" Zobrazit v prohlížeči hello.

toostop vašeho kontejneru, spusťte:

```
docker stop my-web-site
```

Odstraňte kontejner hello z vývojovém počítači:

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-hello-image-toohello-container-registry"></a>Push hello image toohello kontejneru registru
Po ověření, že tento kontejner hello běží na počítači pro vývoj, push hello image tooyour registru v registru kontejner Azure.

Spustit ``docker login`` toolog v registru kontejneru tooyour s vaší [registru pověření](../container-registry/container-registry-authentication.md).

Hello následující příklad úspěšně projde hello ID a hesla služby Azure Active Directory [instanční objekt](../active-directory/active-directory-application-objects.md). Například může přiřadili jste hlavní tooyour registru služby automation scénář. Nebo se můžete přihlásit pomocí uživatelského jména a hesla registru.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Hello následující příkaz vytvoří značku nebo alias hello bitové kopie s registru tooyour plně kvalifikovanou cestu. Tento příklad místech hello bitové kopie v hello ```samples``` zbytečné soubory tooavoid oboru názvů v kořenovém hello hello registru.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Push hello image tooyour kontejneru registru:

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-hello-containerized-service-in-visual-studio"></a>Vytvoření služby hello kontejnerizované v sadě Visual Studio
Hello Service Fabric SDK a nástroje poskytují toohelp šablony služby vytvořit kontejnerizované aplikaci.

1. Spusťte Visual Studio.  Vyberte **Soubor** > **Nový** > **Projekt**.
2. Vyberte **Aplikace Service Fabric**, pojmenujte ji MyFirstContainer a klikněte na **OK**.
3. Vyberte **hosta kontejneru** ze seznamu hello **šablony služby**.
4. V **název bitové kopie** zadejte "myregistry.azurecr.io/samples/helloworldapp", bitové kopie hello nabídnutých tooyour kontejner úložiště.
5. Zadejte název služby a klikněte na **OK**.

## <a name="configure-communication"></a>Konfigurace komunikace
Služba Hello kontejnerizované musí koncový bod pro komunikaci. Přidat `Endpoint` element s hello protokol, port a typ toohello ServiceManifest.xml souboru. V tomto článku hello kontejnerové služby naslouchá na portu 8081.  V tomto příkladu se používá pevný port 8081.  Pokud není port určen, je zvolen náhodných portu z rozsahu portů aplikace hello.  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

Definováním koncový bod Service Fabric publikuje hello koncový bod toohello Naming service.  Tento kontejner může vyřešit jiných služeb spuštěných v clusteru hello.  Můžete také provést-kontejnery komunikaci pomocí hello [reverse proxy](service-fabric-reverseproxy.md).  Komunikace se provádí zadáním port pro naslouchání hello reverzní proxy server HTTP a název hello hello služeb, které chcete toocommunicate s jako proměnné prostředí.

## <a name="configure-and-set-environment-variables"></a>Konfigurace a nastavení proměnných prostředí
Pro každý balíček kódu v hello service manifest lze zadat proměnné prostředí. Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host. Proměnné prostředí hodnoty v aplikaci hello manifest nebo je zadat během nasazování jako parametry aplikace můžete přepsat.

Hello následující služby manifestu XML fragment kódu ukazuje příklad toospecify proměnných prostředí pro balíček kódu:
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

Tyto proměnné prostředí může být přepsána nastaveními v manifest aplikace hello:

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a>Konfigurace mapování portu kontejneru na port hostitele a zjišťování mezi kontejnery
Toocommunicate port používaný na hostiteli nakonfigurujte hello kontejneru. vazbou portu Hello mapuje hello port, na které hello služby naslouchá uvnitř port tooa hello kontejneru na hostiteli hello. Přidat `PortBinding` element v `ContainerHostPolicies` prvek hello ApplicationManifest.xml souboru.  V tomto článku `ContainerPort` je 80 (hello kontejneru zpřístupní port 80 a jako zadaný v hello soubor Docker) a `EndpointRef` je "Guest1TypeEndpoint" (hello dříve definované v hello service manifest koncového bodu).  Příchozí žádosti o služby toohello na portu 8081 mapovat tooport 80 kontejneru hello.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a>Konfigurace ověřování registru kontejneru
Konfigurace ověřování kontejneru registru přidáním `RepositoryCredentials` příliš`ContainerHostPolicies` hello ApplicationManifest.xml souboru. Přidejte hello účet a heslo pro hello myregistry.azurecr.io kontejneru registru, který umožňuje bitovou kopii systému hello služby toodownload hello kontejneru z úložiště hello.

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

Doporučujeme, abyste šifrování hesla hello úložiště pomocí certifikátu šifrování, která nasadila tooall uzly clusteru hello. Když Service Fabric nasadí hello služby balíček toohello clusteru, certifikát šifrování hello je použité toodecrypt hello šifrovaný text.  rutina Invoke-ServiceFabricEncryptText Hello je použité toocreate hello šifrovaný text hello hesla, která se přidá soubor ApplicationManifest.xml toohello.

Hello následující skript vytvoří nový certifikát podepsaný svým držitelem a vyexportuje ji tooa souboru PFX.  certifikát Hello je naimportován do existující trezor klíčů a pak nasadí cluster Service Fabric toohello.

```powershell
# Variables.
$certpwd = ConvertTo-SecureString -String "Pa$$word321!" -Force -AsPlainText
$filepath = "C:\MyCertificates\dataenciphermentcert.pfx"
$subjectname = "dataencipherment"
$vaultname = "mykeyvault"
$certificateName = "dataenciphermentcert"
$groupname="myclustergroup"
$clustername = "mycluster"

$subscriptionId = "subscription ID"

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a self signed cert, export tooPFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import hello certificate tooan existing key vault.  hello key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add hello certificate tooall hello VMs in hello cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
Šifrování hesla hello pomocí hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) rutiny.

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

Nahraďte heslo hello hello šifrovaný text vrácený hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) rutiny a nastavte `PasswordEncrypted` příliš "true".

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-isolation-mode"></a>Konfigurace režimu izolace
Systém Windows podporuje pro kontejnery dva režimy izolace: procesy a Hyper-V. V režimu izolace procesu hello hello všechny kontejnery hello systémem stejné hostitele počítače sdílenou složku hello jádra s hostitelem hello. V režimu izolace hello technologie Hyper-V jsou izolovány. mezi každou kontejneru technologie Hyper-V a hostitelem kontejneru hello hello jádra. režimu izolace Hello je uveden v hello `ContainerHostPolicies` element v souboru manifestu aplikace hello. režim Hello izolaci, které lze zadat `process`, `hyperv`, a `default`. režimu izolace výchozí Hello výchozí příliš`process` v systému Windows Server hostuje a použije se výchozí hodnota příliš`hyperv` na hostitelích s Windows 10. Hello následující fragment kódu ukazuje, jak je režimu izolace hello zadaný v souboru manifestu aplikace hello.

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a>Konfigurace zásad správného řízení prostředků
[Zásady správného řízení prostředků](service-fabric-resource-governance.md) omezuje hello prostředky, které hello kontejneru můžete použít na hostiteli hello. Hello `ResourceGovernancePolicy` element, který je uveden v manifestu aplikace hello, je použít toodeclare prostředků limity pro balíček kódu služby. Dají se nastavit limity prostředků pro hello následující prostředky: paměti, MemorySwap, CpuShares (Relativní váha procesoru), MemoryReservationInMB, BlkioWeight (BlockIO relativní váhy).  V tomto příkladu balíček služby Guest1Pkg získá jednoho jádra na uzlech clusteru hello, kde je umístěn.  Omezení paměti jsou absolutní, takže balíček kódu hello je omezená too1024 MB paměti (a konfigurace soft záruku rezervace z stejné hello). Kód balíčky (kontejnerů a procesy) nejsou možné tooallocate, které větší spotřebu paměti než tento limit a pokus toodo tak vede k výjimce z důvodu nedostatku paměti. Pro toowork vynucení omezení prostředků musí mít všechny balíčky kódu v rámci balíčku služby zadaná paměťová omezení.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-hello-container-application"></a>Nasazení aplikace hello kontejneru
Uložte provedené změny a sestavení aplikace hello. toopublish vaší aplikace, klikněte pravým tlačítkem na **MyFirstContainer** v Průzkumníku řešení a vyberte **publikovat**.

V **koncového bodu připojení**, zadejte hello koncový bod správy pro hello cluster.  Příklad: containercluster.westus2.cloudapp.azure.com:19000. Můžete najít připojení klienta hello koncového bodu v okně Přehled hello cluster v hello [portál Azure](https://portal.azure.com).

Klikněte na **Publikovat**.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je webový nástroj pro kontrolu a správu aplikací a uzlů v clusteru Service Fabric. Otevřete prohlížeč a přejděte toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer/a postupujte podle hello nasazení aplikace.  Hello aplikace nasadí, ale je v chybovém stavu, dokud nebudou staženy hello image na uzlech clusteru hello, (které může trvat nějakou dobu, v závislosti na velikosti obrázku hello): ![chyba][1]

aplikace Hello je připravena, pokud je v ```Ready``` stavu: ![připraveno][2]

Otevřete prohlížeč a přejděte toohttp://containercluster.westus2.cloudapp.azure.com:8081. Měli byste vidět hello záhlaví "Hello, World!" Zobrazit v prohlížeči hello.

## <a name="clean-up"></a>Vyčištění
Pokračovat tooincur poplatky, zatímco hello clusteru je spuštěn, zvažte [odstranění clusteru](service-fabric-get-started-azure-cluster.md#remove-the-cluster).  [Party clustery](http://tryazureservicefabric.westus.cloudapp.azure.com/) se automaticky odstraní po několika hodinách.

Po push hello image toohello kontejneru registru můžete odstranit obrázek místní hello z vývojovém počítači:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Kompletní příklad manifestů služby a aplikace Service Fabric
Tady jsou hello dokončení služby a aplikace manifesty používané v tomto článku.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         hello UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers tooService Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables tooyour container: -->    
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is hello contents of hello Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion
       should match hello Name and Version attributes of hello ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentOverrides CodePackageRef="FrontendService.Code">
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentOverrides>
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types, when an instance of this
         application type is created. You can also create one or more instances of service type using the
         ServiceFabric PowerShell module.

         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="configure-time-interval-before-container-is-force-terminated"></a>Konfigurace časového intervalu před vynuceným ukončením kontejneru

Časový interval pro hello runtime toowait můžete nakonfigurovat před hello kontejneru se odebere po hello odstranění služby (nebo přesunutí uzlu tooanother) bylo zahájeno. Konfigurace časového intervalu hello odešle hello `docker stop <time in seconds>` příkaz toohello kontejneru.   Další podrobnosti najdete v dokumentaci k příkazu [docker stop](https://docs.docker.com/engine/reference/commandline/stop/). toowait interval času Hello je zadaný v hello `Hosting` části. Následující fragment kódu manifestu clusteru Hello ukazuje, jak tooset hello interval čekání:

```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "ContainerDeactivationTimeout": "10",
          ...
          }
        ]
}
```
Hello výchozí časový interval je nastaven too10 sekund. Vzhledem k tomu, že tato konfigurace je dynamický, upgradu konfigurace pouze na hello clusteru aktualizace hello vypršení časového limitu. 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a>Konfigurace modulu runtime tooremove hello nepoužívané kontejneru obrázků

Můžete nakonfigurovat tooremove clusteru Service Fabric hello nepoužívané kontejneru bitové kopie z uzlu hello. Tato konfigurace umožňuje toobe místa na disku nové zachytávání Pokud příliš mnoho obrázků kontejneru se nacházejí na uzlu hello.  tooenable tuto funkci, aktualizace hello `Hosting` část v manifestu clusteru hello, jak je znázorněno v následujícím fragmentu kódu hello: 


```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "PruneContainerImages": “True”,
            "ContainerImagesToSkip": "microsoft/windowsservercore|microsoft/nanoserver|…",
          ...
          }
        ]
} 
```

Pro Image, které by neměly být odstraněny, můžete je zadat v rámci hello `ContainerImagesToSkip` parametr. 



## <a name="next-steps"></a>Další kroky
* Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).
* Čtení hello [nasazení aplikace .NET v kontejneru](service-fabric-host-app-in-a-container.md) kurzu.
* Další informace o hello Service Fabric [životního cyklu aplikace](service-fabric-application-lifecycle.md).
* Najdete v článku věnovaném hello [ukázky kódu Service Fabric kontejneru](https://github.com/Azure-Samples/service-fabric-dotnet-containers) na Githubu.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
