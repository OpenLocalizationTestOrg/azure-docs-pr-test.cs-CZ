---
title: "aaaCreate kontejneru aplikace Azure Service Fabric v systému Linux | Microsoft Docs"
description: "Vytvoříte svou první aplikaci typu kontejner pro Linux na platformě Azure Service Fabric.  Sestavení Docker bitové kopie s vaší aplikací, push hello image tooa kontejneru registru, sestavení a nasazení aplikace Service Fabric kontejneru."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 348dbcbaa1a534fb2808450e4c2d5f9acc7c7b35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-service-fabric-container-application-on-linux"></a>Vytvoření první aplikace Service Fabric typu kontejner v Linuxu
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Spuštění existující aplikace v kontejneru Linux na cluster Service Fabric nevyžaduje žádné změny tooyour aplikace. Tento článek vás provede procesem vytváření obrazem Docker obsahující Python [Flask](http://flask.pocoo.org/) webové aplikace a jeho nasazení tooa cluster Service Fabric.  Kontejnerizovanou aplikaci budete také sdílet prostřednictvím služby [Azure Container Registry](/azure/container-registry/).  Tento článek předpokládá základní znalost Dockeru. Můžete další informace o Docker ve čtení hello [Docker přehled](https://docs.docker.com/engine/understanding-docker/).

## <a name="prerequisites"></a>Požadavky
* Vývojový počítač s:
  * [Sada Service Fabric SDK a nástroje](service-fabric-get-started-linux.md).
  * [Docker CE pro Linux](https://docs.docker.com/engine/installation/#prior-releases). 
  * [Service Fabric CLI](service-fabric-cli.md)

* Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure. 

## <a name="define-hello-docker-container"></a>Zadejte kontejner Docker hello
Vytvořit bitovou kopii podle hello [Python image](https://hub.docker.com/_/python/) nachází na úložiště Docker Hub. 

Definujte kontejner Dockeru v souboru Dockerfile. Hello soubor Docker obsahuje pokyny pro nastavení prostředí hello uvnitř vaší kontejneru, načítání aplikace hello chcete toorun a mapování portů. Hello soubor Docker je vstupní toohello hello `docker build` příkaz, který vytváří hello bitovou kopii. 

Vytvoření prázdného adresáře a vytvoření souboru hello *soubor Docker* (se žádné přípony souboru). Přidejte následující hello příliš*soubor Docker* a uložte změny:

```
# Use an official Python runtime as a base image
FROM python:2.7-slim

# Set hello working directory too/app
WORKDIR /app

# Copy hello current directory contents into hello container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available outside this container
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

## <a name="build-hello-image"></a>Sestavení hello bitové kopie
Spustit hello `docker build` příkaz toocreate hello image, která spouští webovou aplikaci. Otevřete okno prostředí PowerShell a přejděte příliš*c:\temp\helloworldapp*. Spusťte následující příkaz hello:

```bash
docker build -t helloworldapp .
```

Tento příkaz sestavení hello novou bitovou kopii pomocí pokynů hello v váš soubor Docker pojmenování (-t označování) image hello "helloworldapp". Vytváření image vrátí základní image hello z úložiště Docker Hub a vytvoří novou bitovou kopii, která přidává aplikace nad hello základní bitovou kopii.  

Po dokončení příkazu sestavení hello spustit hello `docker images` příkaz toosee informace o novou bitovou kopii hello:

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-hello-application-locally"></a>Místní spuštění aplikace hello
Ověřte, že kontejnerizované aplikace běží místně před odesláním ji hello kontejneru registru.  

Spuštění aplikace hello, mapování počítače port 4000 toohello kontejneru zveřejněné port 80:

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

*název* poskytuje název toohello, systémem kontejneru (namísto ID kontejneru hello).

Připojte toohello systémem kontejneru.  Otevřete webový prohlížeč odkazující toohello IP adresu vrácenou na portu 4000, například "http://localhost:4000". Měli byste vidět hello záhlaví "Hello, World!" Zobrazit v prohlížeči hello.

![Hello World!][hello-world]

toostop vašeho kontejneru, spusťte:

```bash
docker stop my-web-site
```

Odstraňte kontejner hello z vývojovém počítači:

```bash
docker rm my-web-site
```

## <a name="push-hello-image-toohello-container-registry"></a>Push hello image toohello kontejneru registru
Po ověření, že aplikace hello spouští Docker, push hello image tooyour registru v registru kontejner Azure.

Spustit `docker login` toolog v registru kontejneru tooyour s vaší [registru pověření](../container-registry/container-registry-authentication.md).

Hello následující příklad úspěšně projde hello ID a hesla služby Azure Active Directory [instanční objekt](../active-directory/active-directory-application-objects.md). Například může přiřadili jste hlavní tooyour registru služby automation scénář.  Nebo se můžete přihlásit pomocí uživatelského jména a hesla registru.

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Hello následující příkaz vytvoří značku nebo alias hello bitové kopie s registru tooyour plně kvalifikovanou cestu. Tento příklad místech hello bitové kopie v hello `samples` zbytečné soubory tooavoid oboru názvů v kořenovém hello hello registru.

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Push hello image tooyour kontejneru registru:

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-hello-docker-image-with-yeoman"></a>Bitovou kopii balíčku hello Docker s Yeoman
zahrnuje Hello Service Fabric SDK pro Linux [Yeoman](http://yeoman.io/) generátor, který umožňuje snadno toocreate vaší aplikace a přidat bitovou kopii kontejneru. Umožňuje použít Yeoman toocreate názvem aplikace s jediný kontejner Docker *SimpleContainerApp*.

toocreate kontejneru aplikace Service Fabric, otevřete okno terminálu a spusťte `yo azuresfcontainer`.  

Pojmenujte aplikaci (například mycontainer). 

Zadejte adresu URL hello k bitové kopii kontejneru hello v registru kontejneru (například ""). 

Tato bitová kopie je zatížení vstupní bod definován, takže potřebovat tooexplicitly zadat vstupní příkazy (příkazy spustit uvnitř hello kontejneru, který uchová hello kontejneru spuštění po spuštění). 

Jako počet instancí zadejte 1.

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a>Konfigurace mapování portů a ověřování úložiště kontejnerů
Vaše kontejnerizovaná služba potřebuje koncový bod pro komunikaci.  Nyní přidejte hello protokol, port a typ tooan `Endpoint` v souboru ServiceManifest.xml hello. V tomto článku hello kontejnerové služby naslouchá na portu 4000: 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
Poskytnutí hello `UriScheme` automaticky registruje hello kontejneru koncový bod s hello Service Fabric Naming service pro možnosti rozpoznání. Úplný příklad souboru ServiceManifest.xml je k dispozici na konci hello tohoto článku. 

Konfigurace mapování port kontejneru hostitele a portu hello zadáním `PortBinding` zásad v `ContainerHostPolicies` hello ApplicationManifest.xml souboru.  V tomto článku `ContainerPort` je 80 (hello kontejneru zpřístupní port 80 a jako zadaný v hello soubor Docker) a `EndpointRef` je "myserviceTypeEndpoint" (hello koncového bodu definovaného v hello service manifest).  Příchozí žádosti o služby toohello na portu 4000 mapovat tooport 80 kontejneru hello.  Pokud vaše kontejneru potřebuje tooauthenticate s privátní úložiště, přidejte `RepositoryCredentials`.  V tomto článku přidáte hello název účtu a heslo pro hello myregistry.azurecr.io kontejneru registru. 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-hello-service-fabric-application"></a>Sestavení a balíček aplikace Service Fabric hello
šablony služby Fabric Yeoman Hello zahrnují sestavení skript pro [Gradle](https://gradle.org/), které můžete použít aplikaci hello toobuild z hello terminálu. toobuild a balíček hello aplikace, spusťte následující hello:

```bash
cd mycontainer
gradle
```

## <a name="deploy-hello-application"></a>Nasazení aplikace hello
Po hello aplikace, můžete ho nasadit toohello místní cluster pomocí hello Service Fabric rozhraní příkazového řádku.

Připojte toohello místní cluster Service Fabric.

```bash
sfctl cluster select --endpoint http://localhost:19080
```

Použití hello nainstalujete skript zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.

```bash
./install.sh
```

Otevřete prohlížeč a přejděte tooService Fabric Explorer na http://localhost: 19080/Explorer (nahraďte localhost s privátní IP hello hello virtuálních počítačů, když Vagrant na Mac OS X). Rozbalte uzel aplikace hello a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.

Připojte toohello systémem kontejneru.  Otevřete webový prohlížeč odkazující toohello IP adresu vrácenou na portu 4000, například "http://localhost:4000". Měli byste vidět hello záhlaví "Hello, World!" Zobrazit v prohlížeči hello.

![Hello World!][hello-world]

## <a name="clean-up"></a>Vyčištění
Odinstalační skript pro použití hello k dispozici v instanci aplikace hello šablony toodelete hello z hello místního vývojového clusteru a zrušení registrace typu aplikace hello.

```bash
./uninstall.sh
```

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
<ServiceManifest Name="myservicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is hello name of your ServiceType.
         hello UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="myserviceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers 
      tooService Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
        <Commands></Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables tooyour container: -->
    
    <EnvironmentVariables>
      <!--
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
      -->
    </EnvironmentVariables>
    
  </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="mycontainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- Import hello ServiceManifest from hello ServicePackage. hello ServiceManifestName and ServiceManifestVersion 
       should match hello Name and Version attributes of hello ServiceManifest element defined in hello 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="myservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- hello section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using hello 
         ServiceFabric PowerShell module.
         
         hello attribute ServiceTypeName below must match hello name defined in hello imported ServiceManifest.xml file. -->
    <Service Name="myservice">
      <!-- On a local development cluster, set InstanceCount too1.  On a multi-node production 
      cluster, set InstanceCount too-1 for hello container service toorun on every node in 
      hello cluster.
      -->
      <StatelessService ServiceTypeName="myserviceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="adding-more-services-tooan-existing-application"></a>Přidání další služby tooan existující aplikace

provést jiná kontejneru tooan aplikace služby již vytvořený yeoman, tooadd hello následující kroky:

1. Změnit kořenový adresář toohello hello existující aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.
2. Spusťte `yo azuresfcontainer:AddService`.

<a id="manually"></a>


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

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
