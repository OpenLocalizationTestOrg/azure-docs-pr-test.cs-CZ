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
# <a name="create-your-first-service-fabric-container-application-on-linux"></a><span data-ttu-id="bf77a-104">Vytvoření první aplikace Service Fabric typu kontejner v Linuxu</span><span class="sxs-lookup"><span data-stu-id="bf77a-104">Create your first Service Fabric container application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bf77a-105">Windows</span><span class="sxs-lookup"><span data-stu-id="bf77a-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="bf77a-106">Linux</span><span class="sxs-lookup"><span data-stu-id="bf77a-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="bf77a-107">Spuštění existující aplikace v kontejneru Linux na cluster Service Fabric nevyžaduje žádné změny tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf77a-107">Running an existing application in a Linux container on a Service Fabric cluster doesn't require any changes tooyour application.</span></span> <span data-ttu-id="bf77a-108">Tento článek vás provede procesem vytváření obrazem Docker obsahující Python [Flask](http://flask.pocoo.org/) webové aplikace a jeho nasazení tooa cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bf77a-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it tooa Service Fabric cluster.</span></span>  <span data-ttu-id="bf77a-109">Kontejnerizovanou aplikaci budete také sdílet prostřednictvím služby [Azure Container Registry](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="bf77a-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="bf77a-110">Tento článek předpokládá základní znalost Dockeru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="bf77a-111">Můžete další informace o Docker ve čtení hello [Docker přehled](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="bf77a-111">You can learn about Docker by reading hello [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf77a-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bf77a-112">Prerequisites</span></span>
* <span data-ttu-id="bf77a-113">Vývojový počítač s:</span><span class="sxs-lookup"><span data-stu-id="bf77a-113">A development computer running:</span></span>
  * <span data-ttu-id="bf77a-114">[Sada Service Fabric SDK a nástroje](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="bf77a-114">[Service Fabric SDK and tools](service-fabric-get-started-linux.md).</span></span>
  * <span data-ttu-id="bf77a-115">[Docker CE pro Linux](https://docs.docker.com/engine/installation/#prior-releases).</span><span class="sxs-lookup"><span data-stu-id="bf77a-115">[Docker CE for Linux](https://docs.docker.com/engine/installation/#prior-releases).</span></span> 
  * [<span data-ttu-id="bf77a-116">Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="bf77a-116">Service Fabric CLI</span></span>](service-fabric-cli.md)

* <span data-ttu-id="bf77a-117">Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="bf77a-117">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span> 

## <a name="define-hello-docker-container"></a><span data-ttu-id="bf77a-118">Zadejte kontejner Docker hello</span><span class="sxs-lookup"><span data-stu-id="bf77a-118">Define hello Docker container</span></span>
<span data-ttu-id="bf77a-119">Vytvořit bitovou kopii podle hello [Python image](https://hub.docker.com/_/python/) nachází na úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="bf77a-119">Build an image based on hello [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span> 

<span data-ttu-id="bf77a-120">Definujte kontejner Dockeru v souboru Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="bf77a-120">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="bf77a-121">Hello soubor Docker obsahuje pokyny pro nastavení prostředí hello uvnitř vaší kontejneru, načítání aplikace hello chcete toorun a mapování portů.</span><span class="sxs-lookup"><span data-stu-id="bf77a-121">hello Dockerfile contains instructions for setting up hello environment inside your container, loading hello application you want toorun, and mapping ports.</span></span> <span data-ttu-id="bf77a-122">Hello soubor Docker je vstupní toohello hello `docker build` příkaz, který vytváří hello bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="bf77a-122">hello Dockerfile is hello input toohello `docker build` command, which creates hello image.</span></span> 

<span data-ttu-id="bf77a-123">Vytvoření prázdného adresáře a vytvoření souboru hello *soubor Docker* (se žádné přípony souboru).</span><span class="sxs-lookup"><span data-stu-id="bf77a-123">Create an empty directory and create hello file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="bf77a-124">Přidejte následující hello příliš*soubor Docker* a uložte změny:</span><span class="sxs-lookup"><span data-stu-id="bf77a-124">Add hello following too*Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="bf77a-125">Čtení hello [odkaz na soubor Docker](https://docs.docker.com/engine/reference/builder/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="bf77a-125">Read hello [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="bf77a-126">Vytvoření jednoduché webové aplikace</span><span class="sxs-lookup"><span data-stu-id="bf77a-126">Create a simple web application</span></span>
<span data-ttu-id="bf77a-127">Vytvořte webovou aplikaci Flask, která naslouchá na portu 80 a vrací „Hello World!“.</span><span class="sxs-lookup"><span data-stu-id="bf77a-127">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="bf77a-128">V hello stejný adresář, vytvořte soubor hello *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="bf77a-128">In hello same directory, create hello file *requirements.txt*.</span></span>  <span data-ttu-id="bf77a-129">Přidejte následující hello a uložit provedené změny:</span><span class="sxs-lookup"><span data-stu-id="bf77a-129">Add hello following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="bf77a-130">Vytvořit také hello *app.py* souboru a přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="bf77a-130">Also create hello *app.py* file and add hello following:</span></span>

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    
    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

## <a name="build-hello-image"></a><span data-ttu-id="bf77a-131">Sestavení hello bitové kopie</span><span class="sxs-lookup"><span data-stu-id="bf77a-131">Build hello image</span></span>
<span data-ttu-id="bf77a-132">Spustit hello `docker build` příkaz toocreate hello image, která spouští webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="bf77a-132">Run hello `docker build` command toocreate hello image that runs your web application.</span></span> <span data-ttu-id="bf77a-133">Otevřete okno prostředí PowerShell a přejděte příliš*c:\temp\helloworldapp*.</span><span class="sxs-lookup"><span data-stu-id="bf77a-133">Open a PowerShell window and navigate too*c:\temp\helloworldapp*.</span></span> <span data-ttu-id="bf77a-134">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="bf77a-134">Run hello following command:</span></span>

```bash
docker build -t helloworldapp .
```

<span data-ttu-id="bf77a-135">Tento příkaz sestavení hello novou bitovou kopii pomocí pokynů hello v váš soubor Docker pojmenování (-t označování) image hello "helloworldapp".</span><span class="sxs-lookup"><span data-stu-id="bf77a-135">This command builds hello new image using hello instructions in your Dockerfile, naming (-t tagging) hello image "helloworldapp".</span></span> <span data-ttu-id="bf77a-136">Vytváření image vrátí základní image hello z úložiště Docker Hub a vytvoří novou bitovou kopii, která přidává aplikace nad hello základní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="bf77a-136">Building an image pulls hello base image down from Docker Hub and creates a new image that adds your application on top of hello base image.</span></span>  

<span data-ttu-id="bf77a-137">Po dokončení příkazu sestavení hello spustit hello `docker images` příkaz toosee informace o novou bitovou kopii hello:</span><span class="sxs-lookup"><span data-stu-id="bf77a-137">Once hello build command completes, run hello `docker images` command toosee information on hello new image:</span></span>

```bash
$ docker images
    
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              86838648aab6        2 minutes ago       194 MB
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="bf77a-138">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="bf77a-138">Run hello application locally</span></span>
<span data-ttu-id="bf77a-139">Ověřte, že kontejnerizované aplikace běží místně před odesláním ji hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-139">Verify that your containerized application runs locally before pushing it hello container registry.</span></span>  

<span data-ttu-id="bf77a-140">Spuštění aplikace hello, mapování počítače port 4000 toohello kontejneru zveřejněné port 80:</span><span class="sxs-lookup"><span data-stu-id="bf77a-140">Run hello application, mapping your computer's port 4000 toohello container's exposed port 80:</span></span>

```bash
docker run -d -p 4000:80 --name my-web-site helloworldapp
```

<span data-ttu-id="bf77a-141">*název* poskytuje název toohello, systémem kontejneru (namísto ID kontejneru hello).</span><span class="sxs-lookup"><span data-stu-id="bf77a-141">*name* gives a name toohello running container (instead of hello container ID).</span></span>

<span data-ttu-id="bf77a-142">Připojte toohello systémem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-142">Connect toohello running container.</span></span>  <span data-ttu-id="bf77a-143">Otevřete webový prohlížeč odkazující toohello IP adresu vrácenou na portu 4000, například "http://localhost:4000".</span><span class="sxs-lookup"><span data-stu-id="bf77a-143">Open a web browser pointing toohello IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="bf77a-144">Měli byste vidět hello záhlaví "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="bf77a-144">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="bf77a-145">Zobrazit v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-145">display in hello browser.</span></span>

![Hello World!][hello-world]

<span data-ttu-id="bf77a-147">toostop vašeho kontejneru, spusťte:</span><span class="sxs-lookup"><span data-stu-id="bf77a-147">toostop your container, run:</span></span>

```bash
docker stop my-web-site
```

<span data-ttu-id="bf77a-148">Odstraňte kontejner hello z vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="bf77a-148">Delete hello container from your development machine:</span></span>

```bash
docker rm my-web-site
```

## <a name="push-hello-image-toohello-container-registry"></a><span data-ttu-id="bf77a-149">Push hello image toohello kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="bf77a-149">Push hello image toohello container registry</span></span>
<span data-ttu-id="bf77a-150">Po ověření, že aplikace hello spouští Docker, push hello image tooyour registru v registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="bf77a-150">After you verify that hello application runs in Docker, push hello image tooyour registry in Azure Container Registry.</span></span>

<span data-ttu-id="bf77a-151">Spustit `docker login` toolog v registru kontejneru tooyour s vaší [registru pověření](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="bf77a-151">Run `docker login` toolog in tooyour container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="bf77a-152">Hello následující příklad úspěšně projde hello ID a hesla služby Azure Active Directory [instanční objekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="bf77a-152">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="bf77a-153">Například může přiřadili jste hlavní tooyour registru služby automation scénář.</span><span class="sxs-lookup"><span data-stu-id="bf77a-153">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span>  <span data-ttu-id="bf77a-154">Nebo se můžete přihlásit pomocí uživatelského jména a hesla registru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-154">Or, you could login using your registry username and password.</span></span>

```bash
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="bf77a-155">Hello následující příkaz vytvoří značku nebo alias hello bitové kopie s registru tooyour plně kvalifikovanou cestu.</span><span class="sxs-lookup"><span data-stu-id="bf77a-155">hello following command creates a tag, or alias, of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="bf77a-156">Tento příklad místech hello bitové kopie v hello `samples` zbytečné soubory tooavoid oboru názvů v kořenovém hello hello registru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-156">This example places hello image in hello `samples` namespace tooavoid clutter in hello root of hello registry.</span></span>

```bash
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="bf77a-157">Push hello image tooyour kontejneru registru:</span><span class="sxs-lookup"><span data-stu-id="bf77a-157">Push hello image tooyour container registry:</span></span>

```bash
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="package-hello-docker-image-with-yeoman"></a><span data-ttu-id="bf77a-158">Bitovou kopii balíčku hello Docker s Yeoman</span><span class="sxs-lookup"><span data-stu-id="bf77a-158">Package hello Docker image with Yeoman</span></span>
<span data-ttu-id="bf77a-159">zahrnuje Hello Service Fabric SDK pro Linux [Yeoman](http://yeoman.io/) generátor, který umožňuje snadno toocreate vaší aplikace a přidat bitovou kopii kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-159">hello Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy toocreate your application and add a container image.</span></span> <span data-ttu-id="bf77a-160">Umožňuje použít Yeoman toocreate názvem aplikace s jediný kontejner Docker *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="bf77a-160">Let's use Yeoman toocreate an application with a single Docker container called *SimpleContainerApp*.</span></span>

<span data-ttu-id="bf77a-161">toocreate kontejneru aplikace Service Fabric, otevřete okno terminálu a spusťte `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="bf77a-161">toocreate a Service Fabric container application, open a terminal window and run `yo azuresfcontainer`.</span></span>  

<span data-ttu-id="bf77a-162">Pojmenujte aplikaci (například mycontainer).</span><span class="sxs-lookup"><span data-stu-id="bf77a-162">Name your application (for example, "mycontainer").</span></span> 

<span data-ttu-id="bf77a-163">Zadejte adresu URL hello k bitové kopii kontejneru hello v registru kontejneru (například "").</span><span class="sxs-lookup"><span data-stu-id="bf77a-163">Provide hello URL for hello container image in a container registry (for example, "").</span></span> 

<span data-ttu-id="bf77a-164">Tato bitová kopie je zatížení vstupní bod definován, takže potřebovat tooexplicitly zadat vstupní příkazy (příkazy spustit uvnitř hello kontejneru, který uchová hello kontejneru spuštění po spuštění).</span><span class="sxs-lookup"><span data-stu-id="bf77a-164">This image has a workload entry-point defined, so need tooexplicitly specify input commands (commands run inside hello container, which will keep hello container running after startup).</span></span> 

<span data-ttu-id="bf77a-165">Jako počet instancí zadejte 1.</span><span class="sxs-lookup"><span data-stu-id="bf77a-165">Specify an instance count of "1".</span></span>

![Generátor Service Fabric Yeoman pro kontejnery][sf-yeoman]

## <a name="configure-port-mapping-and-container-repository-authentication"></a><span data-ttu-id="bf77a-167">Konfigurace mapování portů a ověřování úložiště kontejnerů</span><span class="sxs-lookup"><span data-stu-id="bf77a-167">Configure port mapping and container repository authentication</span></span>
<span data-ttu-id="bf77a-168">Vaše kontejnerizovaná služba potřebuje koncový bod pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="bf77a-168">Your containerized service needs an endpoint for communication.</span></span>  <span data-ttu-id="bf77a-169">Nyní přidejte hello protokol, port a typ tooan `Endpoint` v souboru ServiceManifest.xml hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-169">Now add hello protocol, port, and type tooan `Endpoint` in hello ServiceManifest.xml file.</span></span> <span data-ttu-id="bf77a-170">V tomto článku hello kontejnerové služby naslouchá na portu 4000:</span><span class="sxs-lookup"><span data-stu-id="bf77a-170">For this article, hello containerized service listens on port 4000:</span></span> 

```xml
<Endpoint Name="myserviceTypeEndpoint" UriScheme="http" Port="4000" Protocol="http"/>
```
<span data-ttu-id="bf77a-171">Poskytnutí hello `UriScheme` automaticky registruje hello kontejneru koncový bod s hello Service Fabric Naming service pro možnosti rozpoznání.</span><span class="sxs-lookup"><span data-stu-id="bf77a-171">Providing hello `UriScheme` automatically registers hello container endpoint with hello Service Fabric Naming service for discoverability.</span></span> <span data-ttu-id="bf77a-172">Úplný příklad souboru ServiceManifest.xml je k dispozici na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="bf77a-172">A full ServiceManifest.xml example file is provided at hello end of this article.</span></span> 

<span data-ttu-id="bf77a-173">Konfigurace mapování port kontejneru hostitele a portu hello zadáním `PortBinding` zásad v `ContainerHostPolicies` hello ApplicationManifest.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-173">Configure hello container port-to-host port mapping by specifying a `PortBinding` policy in `ContainerHostPolicies` of hello ApplicationManifest.xml file.</span></span>  <span data-ttu-id="bf77a-174">V tomto článku `ContainerPort` je 80 (hello kontejneru zpřístupní port 80 a jako zadaný v hello soubor Docker) a `EndpointRef` je "myserviceTypeEndpoint" (hello koncového bodu definovaného v hello service manifest).</span><span class="sxs-lookup"><span data-stu-id="bf77a-174">For this article, `ContainerPort` is 80 (hello container exposes port 80, as specified in hello Dockerfile) and `EndpointRef` is "myserviceTypeEndpoint" (hello endpoint defined in hello service manifest).</span></span>  <span data-ttu-id="bf77a-175">Příchozí žádosti o služby toohello na portu 4000 mapovat tooport 80 kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-175">Incoming requests toohello service on port 4000 are mapped tooport 80 on hello container.</span></span>  <span data-ttu-id="bf77a-176">Pokud vaše kontejneru potřebuje tooauthenticate s privátní úložiště, přidejte `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="bf77a-176">If your container needs tooauthenticate with a private repository, then add `RepositoryCredentials`.</span></span>  <span data-ttu-id="bf77a-177">V tomto článku přidáte hello název účtu a heslo pro hello myregistry.azurecr.io kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-177">For this article, add hello account name and password for hello myregistry.azurecr.io container registry.</span></span> 

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="myserviceTypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

## <a name="build-and-package-hello-service-fabric-application"></a><span data-ttu-id="bf77a-178">Sestavení a balíček aplikace Service Fabric hello</span><span class="sxs-lookup"><span data-stu-id="bf77a-178">Build and package hello Service Fabric application</span></span>
<span data-ttu-id="bf77a-179">šablony služby Fabric Yeoman Hello zahrnují sestavení skript pro [Gradle](https://gradle.org/), které můžete použít aplikaci hello toobuild z hello terminálu.</span><span class="sxs-lookup"><span data-stu-id="bf77a-179">hello Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use toobuild hello application from hello terminal.</span></span> <span data-ttu-id="bf77a-180">toobuild a balíček hello aplikace, spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="bf77a-180">toobuild and package hello application, run hello following:</span></span>

```bash
cd mycontainer
gradle
```

## <a name="deploy-hello-application"></a><span data-ttu-id="bf77a-181">Nasazení aplikace hello</span><span class="sxs-lookup"><span data-stu-id="bf77a-181">Deploy hello application</span></span>
<span data-ttu-id="bf77a-182">Po hello aplikace, můžete ho nasadit toohello místní cluster pomocí hello Service Fabric rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="bf77a-182">Once hello application is built, you can deploy it toohello local cluster using hello Service Fabric CLI.</span></span>

<span data-ttu-id="bf77a-183">Připojte toohello místní cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="bf77a-183">Connect toohello local Service Fabric cluster.</span></span>

```bash
sfctl cluster select --endpoint http://localhost:19080
```

<span data-ttu-id="bf77a-184">Použití hello nainstalujete skript zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-184">Use hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

```bash
./install.sh
```

<span data-ttu-id="bf77a-185">Otevřete prohlížeč a přejděte tooService Fabric Explorer na http://localhost: 19080/Explorer (nahraďte localhost s privátní IP hello hello virtuálních počítačů, když Vagrant na Mac OS X).</span><span class="sxs-lookup"><span data-stu-id="bf77a-185">Open a browser and navigate tooService Fabric Explorer at http://localhost:19080/Explorer (replace localhost with hello private IP of hello VM if using Vagrant on Mac OS X).</span></span> <span data-ttu-id="bf77a-186">Rozbalte uzel aplikace hello a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.</span><span class="sxs-lookup"><span data-stu-id="bf77a-186">Expand hello Applications node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

<span data-ttu-id="bf77a-187">Připojte toohello systémem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-187">Connect toohello running container.</span></span>  <span data-ttu-id="bf77a-188">Otevřete webový prohlížeč odkazující toohello IP adresu vrácenou na portu 4000, například "http://localhost:4000".</span><span class="sxs-lookup"><span data-stu-id="bf77a-188">Open a web browser pointing toohello IP address returned on port 4000, for example "http://localhost:4000".</span></span> <span data-ttu-id="bf77a-189">Měli byste vidět hello záhlaví "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="bf77a-189">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="bf77a-190">Zobrazit v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-190">display in hello browser.</span></span>

![Hello World!][hello-world]

## <a name="clean-up"></a><span data-ttu-id="bf77a-192">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="bf77a-192">Clean up</span></span>
<span data-ttu-id="bf77a-193">Odinstalační skript pro použití hello k dispozici v instanci aplikace hello šablony toodelete hello z hello místního vývojového clusteru a zrušení registrace typu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-193">Use hello uninstall script provided in hello template toodelete hello application instance from hello local development cluster and unregister hello application type.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="bf77a-194">Po push hello image toohello kontejneru registru můžete odstranit obrázek místní hello z vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="bf77a-194">After you push hello image toohello container registry you can delete hello local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="bf77a-195">Kompletní příklad manifestů služby a aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="bf77a-195">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="bf77a-196">Tady jsou hello dokončení služby a aplikace manifesty používané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="bf77a-196">Here are hello complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="bf77a-197">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="bf77a-197">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="bf77a-198">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="bf77a-198">ApplicationManifest.xml</span></span>
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
## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="bf77a-199">Přidání další služby tooan existující aplikace</span><span class="sxs-lookup"><span data-stu-id="bf77a-199">Adding more services tooan existing application</span></span>

<span data-ttu-id="bf77a-200">provést jiná kontejneru tooan aplikace služby již vytvořený yeoman, tooadd hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="bf77a-200">tooadd another container service tooan application already created using yeoman, perform hello following steps:</span></span>

1. <span data-ttu-id="bf77a-201">Změnit kořenový adresář toohello hello existující aplikace.</span><span class="sxs-lookup"><span data-stu-id="bf77a-201">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="bf77a-202">Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-202">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="bf77a-203">Spusťte `yo azuresfcontainer:AddService`.</span><span class="sxs-lookup"><span data-stu-id="bf77a-203">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>


## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="bf77a-204">Konfigurace časového intervalu před vynuceným ukončením kontejneru</span><span class="sxs-lookup"><span data-stu-id="bf77a-204">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="bf77a-205">Časový interval pro hello runtime toowait můžete nakonfigurovat před hello kontejneru se odebere po hello odstranění služby (nebo přesunutí uzlu tooanother) bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="bf77a-205">You can configure a time interval for hello runtime toowait before hello container is removed after hello service deletion (or a move tooanother node) has started.</span></span> <span data-ttu-id="bf77a-206">Konfigurace časového intervalu hello odešle hello `docker stop <time in seconds>` příkaz toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="bf77a-206">Configuring hello time interval sends hello `docker stop <time in seconds>` command toohello container.</span></span>   <span data-ttu-id="bf77a-207">Další podrobnosti najdete v dokumentaci k příkazu [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="bf77a-207">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="bf77a-208">toowait interval času Hello je zadaný v hello `Hosting` části.</span><span class="sxs-lookup"><span data-stu-id="bf77a-208">hello time interval toowait is specified under hello `Hosting` section.</span></span> <span data-ttu-id="bf77a-209">Následující fragment kódu manifestu clusteru Hello ukazuje, jak tooset hello interval čekání:</span><span class="sxs-lookup"><span data-stu-id="bf77a-209">hello following cluster manifest snippet shows how tooset hello wait interval:</span></span>

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
<span data-ttu-id="bf77a-210">Hello výchozí časový interval je nastaven too10 sekund.</span><span class="sxs-lookup"><span data-stu-id="bf77a-210">hello default time interval is set too10 seconds.</span></span> <span data-ttu-id="bf77a-211">Vzhledem k tomu, že tato konfigurace je dynamický, upgradu konfigurace pouze na hello clusteru aktualizace hello vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="bf77a-211">Since this configuration is dynamic, a config only upgrade on hello cluster updates hello timeout.</span></span> 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a><span data-ttu-id="bf77a-212">Konfigurace modulu runtime tooremove hello nepoužívané kontejneru obrázků</span><span class="sxs-lookup"><span data-stu-id="bf77a-212">Configure hello runtime tooremove unused container images</span></span>

<span data-ttu-id="bf77a-213">Můžete nakonfigurovat tooremove clusteru Service Fabric hello nepoužívané kontejneru bitové kopie z uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-213">You can configure hello Service Fabric cluster tooremove unused container images from hello node.</span></span> <span data-ttu-id="bf77a-214">Tato konfigurace umožňuje toobe místa na disku nové zachytávání Pokud příliš mnoho obrázků kontejneru se nacházejí na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="bf77a-214">This configuration allows disk space toobe recaptured if too many container images are present on hello node.</span></span>  <span data-ttu-id="bf77a-215">tooenable tuto funkci, aktualizace hello `Hosting` část v manifestu clusteru hello, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="bf77a-215">tooenable this feature, update hello `Hosting` section in hello cluster manifest as shown in hello following snippet:</span></span> 


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

<span data-ttu-id="bf77a-216">Pro Image, které by neměly být odstraněny, můžete je zadat v rámci hello `ContainerImagesToSkip` parametr.</span><span class="sxs-lookup"><span data-stu-id="bf77a-216">For images that should not be deleted, you can specify them under hello `ContainerImagesToSkip` parameter.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="bf77a-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bf77a-217">Next steps</span></span>
* <span data-ttu-id="bf77a-218">Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bf77a-218">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="bf77a-219">Čtení hello [nasazení aplikace .NET v kontejneru](service-fabric-host-app-in-a-container.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="bf77a-219">Read hello [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="bf77a-220">Další informace o hello Service Fabric [životního cyklu aplikace](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="bf77a-220">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="bf77a-221">Najdete v článku věnovaném hello [ukázky kódu Service Fabric kontejneru](https://github.com/Azure-Samples/service-fabric-dotnet-containers) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="bf77a-221">Checkout hello [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[hello-world]: ./media/service-fabric-get-started-containers-linux/HelloWorld.png
[sf-yeoman]: ./media/service-fabric-get-started-containers-linux/YoSF.png
