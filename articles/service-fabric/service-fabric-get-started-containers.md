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
# <a name="create-your-first-service-fabric-container-application-on-windows"></a><span data-ttu-id="67c25-104">Vytvoření první aplikace Service Fabric typu kontejner v systému Windows</span><span class="sxs-lookup"><span data-stu-id="67c25-104">Create your first Service Fabric container application on Windows</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="67c25-105">Windows</span><span class="sxs-lookup"><span data-stu-id="67c25-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="67c25-106">Linux</span><span class="sxs-lookup"><span data-stu-id="67c25-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="67c25-107">Spuštění existující aplikace v kontejneru systému Windows na cluster Service Fabric nevyžaduje žádné změny tooyour aplikace.</span><span class="sxs-lookup"><span data-stu-id="67c25-107">Running an existing application in a Windows container on a Service Fabric cluster doesn't require any changes tooyour application.</span></span> <span data-ttu-id="67c25-108">Tento článek vás provede procesem vytváření obrazem Docker obsahující Python [Flask](http://flask.pocoo.org/) webové aplikace a jeho nasazení tooa cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="67c25-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it tooa Service Fabric cluster.</span></span>  <span data-ttu-id="67c25-109">Kontejnerizovanou aplikaci budete také sdílet prostřednictvím služby [Azure Container Registry](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="67c25-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="67c25-110">Tento článek předpokládá základní znalost Dockeru.</span><span class="sxs-lookup"><span data-stu-id="67c25-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="67c25-111">Můžete další informace o Docker ve čtení hello [Docker přehled](https://docs.docker.com/engine/understanding-docker/).</span><span class="sxs-lookup"><span data-stu-id="67c25-111">You can learn about Docker by reading hello [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67c25-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="67c25-112">Prerequisites</span></span>
<span data-ttu-id="67c25-113">Vývojový počítač s:</span><span class="sxs-lookup"><span data-stu-id="67c25-113">A development computer running:</span></span>
* <span data-ttu-id="67c25-114">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="67c25-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="67c25-115">[Sada Service Fabric SDK a nástroje](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="67c25-115">[Service Fabric SDK and tools](service-fabric-get-started.md).</span></span>
*  <span data-ttu-id="67c25-116">Docker pro Windows.</span><span class="sxs-lookup"><span data-stu-id="67c25-116">Docker for Windows.</span></span>  <span data-ttu-id="67c25-117">[Získejte Docker CE pro Windows (stabilní verze)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span><span class="sxs-lookup"><span data-stu-id="67c25-117">[Get Docker CE for Windows (stable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span></span> <span data-ttu-id="67c25-118">Po instalaci a spuštění Docker, klikněte pravým tlačítkem na ikonu hello na hlavním panelu a vyberte **přepínač kontejnery tooWindows**.</span><span class="sxs-lookup"><span data-stu-id="67c25-118">After installing and starting Docker, right-click on hello tray icon and select **Switch tooWindows containers**.</span></span> <span data-ttu-id="67c25-119">Toto je požadované toorun Docker Image založené na systému Windows.</span><span class="sxs-lookup"><span data-stu-id="67c25-119">This is required toorun Docker images based on Windows.</span></span>

<span data-ttu-id="67c25-120">Cluster Windows se třemi nebo více uzly spuštěnými na Windows Serveru 2016 s kontejnery – [Vytvořte cluster](service-fabric-cluster-creation-via-portal.md) nebo [vyzkoušejte Service Fabric zdarma](https://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="67c25-120">A Windows cluster with three or more nodes running on Windows Server 2016 with Containers- [Create a cluster](service-fabric-cluster-creation-via-portal.md) or [try Service Fabric for free](https://aka.ms/tryservicefabric).</span></span>

<span data-ttu-id="67c25-121">Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="67c25-121">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span>

## <a name="define-hello-docker-container"></a><span data-ttu-id="67c25-122">Zadejte kontejner Docker hello</span><span class="sxs-lookup"><span data-stu-id="67c25-122">Define hello Docker container</span></span>
<span data-ttu-id="67c25-123">Vytvořit bitovou kopii podle hello [Python image](https://hub.docker.com/_/python/) nachází na úložiště Docker Hub.</span><span class="sxs-lookup"><span data-stu-id="67c25-123">Build an image based on hello [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span>

<span data-ttu-id="67c25-124">Definujte kontejner Dockeru v souboru Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="67c25-124">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="67c25-125">Hello soubor Docker obsahuje pokyny pro nastavení prostředí hello uvnitř vaší kontejneru, načítání aplikace hello chcete toorun a mapování portů.</span><span class="sxs-lookup"><span data-stu-id="67c25-125">hello Dockerfile contains instructions for setting up hello environment inside your container, loading hello application you want toorun, and mapping ports.</span></span> <span data-ttu-id="67c25-126">Hello soubor Docker je vstupní toohello hello `docker build` příkaz, který vytváří hello bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="67c25-126">hello Dockerfile is hello input toohello `docker build` command, which creates hello image.</span></span>

<span data-ttu-id="67c25-127">Vytvoření prázdného adresáře a vytvoření souboru hello *soubor Docker* (se žádné přípony souboru).</span><span class="sxs-lookup"><span data-stu-id="67c25-127">Create an empty directory and create hello file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="67c25-128">Přidejte následující hello příliš*soubor Docker* a uložte změny:</span><span class="sxs-lookup"><span data-stu-id="67c25-128">Add hello following too*Dockerfile* and save your changes:</span></span>

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

<span data-ttu-id="67c25-129">Čtení hello [odkaz na soubor Docker](https://docs.docker.com/engine/reference/builder/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="67c25-129">Read hello [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="67c25-130">Vytvoření jednoduché webové aplikace</span><span class="sxs-lookup"><span data-stu-id="67c25-130">Create a simple web application</span></span>
<span data-ttu-id="67c25-131">Vytvořte webovou aplikaci Flask, která naslouchá na portu 80 a vrací „Hello World!“.</span><span class="sxs-lookup"><span data-stu-id="67c25-131">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="67c25-132">V hello stejný adresář, vytvořte soubor hello *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="67c25-132">In hello same directory, create hello file *requirements.txt*.</span></span>  <span data-ttu-id="67c25-133">Přidejte následující hello a uložit provedené změny:</span><span class="sxs-lookup"><span data-stu-id="67c25-133">Add hello following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="67c25-134">Vytvořit také hello *app.py* souboru a přidejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="67c25-134">Also create hello *app.py* file and add hello following:</span></span>

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
## <a name="build-hello-image"></a><span data-ttu-id="67c25-135">Sestavení hello bitové kopie</span><span class="sxs-lookup"><span data-stu-id="67c25-135">Build hello image</span></span>
<span data-ttu-id="67c25-136">Spustit hello `docker build` příkaz toocreate hello image, která spouští webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67c25-136">Run hello `docker build` command toocreate hello image that runs your web application.</span></span> <span data-ttu-id="67c25-137">Otevřete okno prostředí PowerShell a přejděte toohello adresář obsahující hello soubor Docker.</span><span class="sxs-lookup"><span data-stu-id="67c25-137">Open a PowerShell window and navigate toohello directory containing hello Dockerfile.</span></span> <span data-ttu-id="67c25-138">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="67c25-138">Run hello following command:</span></span>

```
docker build -t helloworldapp .
```

<span data-ttu-id="67c25-139">Tento příkaz sestavení hello novou bitovou kopii pomocí pokynů hello v váš soubor Docker pojmenování (-t označování) image hello "helloworldapp".</span><span class="sxs-lookup"><span data-stu-id="67c25-139">This command builds hello new image using hello instructions in your Dockerfile, naming (-t tagging) hello image "helloworldapp".</span></span> <span data-ttu-id="67c25-140">Vytváření image vrátí základní image hello z úložiště Docker Hub a vytvoří novou bitovou kopii, která přidává aplikace nad hello základní bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="67c25-140">Building an image pulls hello base image down from Docker Hub and creates a new image that adds your application on top of hello base image.</span></span>  

<span data-ttu-id="67c25-141">Po dokončení příkazu sestavení hello spustit hello `docker images` příkaz toosee informace o novou bitovou kopii hello:</span><span class="sxs-lookup"><span data-stu-id="67c25-141">Once hello build command completes, run hello `docker images` command toosee information on hello new image:</span></span>

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-hello-application-locally"></a><span data-ttu-id="67c25-142">Místní spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="67c25-142">Run hello application locally</span></span>
<span data-ttu-id="67c25-143">Bitové kopie ověřte místně před odesláním ji hello kontejneru registru.</span><span class="sxs-lookup"><span data-stu-id="67c25-143">Verify your image locally before pushing it hello container registry.</span></span>  

<span data-ttu-id="67c25-144">Spuštění aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="67c25-144">Run hello application:</span></span>

```
docker run -d --name my-web-site helloworldapp
```

<span data-ttu-id="67c25-145">*název* poskytuje název toohello, systémem kontejneru (namísto ID kontejneru hello).</span><span class="sxs-lookup"><span data-stu-id="67c25-145">*name* gives a name toohello running container (instead of hello container ID).</span></span>

<span data-ttu-id="67c25-146">Jakmile se spustí hello kontejneru, najděte jeho IP adresu, aby mohl připojit tooyour systémem kontejneru z prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="67c25-146">Once hello container starts, find its IP address so that you can connect tooyour running container from a browser:</span></span>
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

<span data-ttu-id="67c25-147">Připojte toohello systémem kontejneru.</span><span class="sxs-lookup"><span data-stu-id="67c25-147">Connect toohello running container.</span></span>  <span data-ttu-id="67c25-148">Otevřete webový prohlížeč odkazující toohello IP adresu vrácenou, například "http://172.31.194.61".</span><span class="sxs-lookup"><span data-stu-id="67c25-148">Open a web browser pointing toohello IP address returned, for example "http://172.31.194.61".</span></span> <span data-ttu-id="67c25-149">Měli byste vidět hello záhlaví "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="67c25-149">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="67c25-150">Zobrazit v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-150">display in hello browser.</span></span>

<span data-ttu-id="67c25-151">toostop vašeho kontejneru, spusťte:</span><span class="sxs-lookup"><span data-stu-id="67c25-151">toostop your container, run:</span></span>

```
docker stop my-web-site
```

<span data-ttu-id="67c25-152">Odstraňte kontejner hello z vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="67c25-152">Delete hello container from your development machine:</span></span>

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-hello-image-toohello-container-registry"></a><span data-ttu-id="67c25-153">Push hello image toohello kontejneru registru</span><span class="sxs-lookup"><span data-stu-id="67c25-153">Push hello image toohello container registry</span></span>
<span data-ttu-id="67c25-154">Po ověření, že tento kontejner hello běží na počítači pro vývoj, push hello image tooyour registru v registru kontejner Azure.</span><span class="sxs-lookup"><span data-stu-id="67c25-154">After you verify that hello container runs on your development machine, push hello image tooyour registry in Azure Container Registry.</span></span>

<span data-ttu-id="67c25-155">Spustit ``docker login`` toolog v registru kontejneru tooyour s vaší [registru pověření](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="67c25-155">Run ``docker login`` toolog in tooyour container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="67c25-156">Hello následující příklad úspěšně projde hello ID a hesla služby Azure Active Directory [instanční objekt](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="67c25-156">hello following example passes hello ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="67c25-157">Například může přiřadili jste hlavní tooyour registru služby automation scénář.</span><span class="sxs-lookup"><span data-stu-id="67c25-157">For example, you might have assigned a service principal tooyour registry for an automation scenario.</span></span> <span data-ttu-id="67c25-158">Nebo se můžete přihlásit pomocí uživatelského jména a hesla registru.</span><span class="sxs-lookup"><span data-stu-id="67c25-158">Or, you could login using your registry username and password.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="67c25-159">Hello následující příkaz vytvoří značku nebo alias hello bitové kopie s registru tooyour plně kvalifikovanou cestu.</span><span class="sxs-lookup"><span data-stu-id="67c25-159">hello following command creates a tag, or alias, of hello image, with a fully qualified path tooyour registry.</span></span> <span data-ttu-id="67c25-160">Tento příklad místech hello bitové kopie v hello ```samples``` zbytečné soubory tooavoid oboru názvů v kořenovém hello hello registru.</span><span class="sxs-lookup"><span data-stu-id="67c25-160">This example places hello image in hello ```samples``` namespace tooavoid clutter in hello root of hello registry.</span></span>

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="67c25-161">Push hello image tooyour kontejneru registru:</span><span class="sxs-lookup"><span data-stu-id="67c25-161">Push hello image tooyour container registry:</span></span>

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-hello-containerized-service-in-visual-studio"></a><span data-ttu-id="67c25-162">Vytvoření služby hello kontejnerizované v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="67c25-162">Create hello containerized service in Visual Studio</span></span>
<span data-ttu-id="67c25-163">Hello Service Fabric SDK a nástroje poskytují toohelp šablony služby vytvořit kontejnerizované aplikaci.</span><span class="sxs-lookup"><span data-stu-id="67c25-163">hello Service Fabric SDK and tools provide a service template toohelp you create a containerized application.</span></span>

1. <span data-ttu-id="67c25-164">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67c25-164">Start Visual Studio.</span></span>  <span data-ttu-id="67c25-165">Vyberte **Soubor** > **Nový** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="67c25-165">Select **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="67c25-166">Vyberte **Aplikace Service Fabric**, pojmenujte ji MyFirstContainer a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="67c25-166">Select **Service Fabric application**, name it "MyFirstContainer", and click **OK**.</span></span>
3. <span data-ttu-id="67c25-167">Vyberte **hosta kontejneru** ze seznamu hello **šablony služby**.</span><span class="sxs-lookup"><span data-stu-id="67c25-167">Select **Guest Container** from hello list of **service templates**.</span></span>
4. <span data-ttu-id="67c25-168">V **název bitové kopie** zadejte "myregistry.azurecr.io/samples/helloworldapp", bitové kopie hello nabídnutých tooyour kontejner úložiště.</span><span class="sxs-lookup"><span data-stu-id="67c25-168">In **Image Name** enter "myregistry.azurecr.io/samples/helloworldapp", hello image you pushed tooyour container repository.</span></span>
5. <span data-ttu-id="67c25-169">Zadejte název služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="67c25-169">Give your service a name, and click **OK**.</span></span>

## <a name="configure-communication"></a><span data-ttu-id="67c25-170">Konfigurace komunikace</span><span class="sxs-lookup"><span data-stu-id="67c25-170">Configure communication</span></span>
<span data-ttu-id="67c25-171">Služba Hello kontejnerizované musí koncový bod pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="67c25-171">hello containerized service needs an endpoint for communication.</span></span> <span data-ttu-id="67c25-172">Přidat `Endpoint` element s hello protokol, port a typ toohello ServiceManifest.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="67c25-172">Add an `Endpoint` element with hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="67c25-173">V tomto článku hello kontejnerové služby naslouchá na portu 8081.</span><span class="sxs-lookup"><span data-stu-id="67c25-173">For this article, hello containerized service listens on port 8081.</span></span>  <span data-ttu-id="67c25-174">V tomto příkladu se používá pevný port 8081.</span><span class="sxs-lookup"><span data-stu-id="67c25-174">In this example, a fixed port 8081 is used.</span></span>  <span data-ttu-id="67c25-175">Pokud není port určen, je zvolen náhodných portu z rozsahu portů aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-175">If no port is specified, a random port from hello application port range is chosen.</span></span>  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="67c25-176">Definováním koncový bod Service Fabric publikuje hello koncový bod toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="67c25-176">By defining an endpoint, Service Fabric publishes hello endpoint toohello Naming service.</span></span>  <span data-ttu-id="67c25-177">Tento kontejner může vyřešit jiných služeb spuštěných v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-177">Other services running in hello cluster can resolve this container.</span></span>  <span data-ttu-id="67c25-178">Můžete také provést-kontejnery komunikaci pomocí hello [reverse proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="67c25-178">You can also perform container-to-container communication using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span>  <span data-ttu-id="67c25-179">Komunikace se provádí zadáním port pro naslouchání hello reverzní proxy server HTTP a název hello hello služeb, které chcete toocommunicate s jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="67c25-179">Communication is performed by providing hello reverse proxy HTTP listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="67c25-180">Konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="67c25-180">Configure and set environment variables</span></span>
<span data-ttu-id="67c25-181">Pro každý balíček kódu v hello service manifest lze zadat proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="67c25-181">Environment variables can be specified for each code package in hello service manifest.</span></span> <span data-ttu-id="67c25-182">Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host.</span><span class="sxs-lookup"><span data-stu-id="67c25-182">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="67c25-183">Proměnné prostředí hodnoty v aplikaci hello manifest nebo je zadat během nasazování jako parametry aplikace můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="67c25-183">You can override environment variable values in hello application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="67c25-184">Hello následující služby manifestu XML fragment kódu ukazuje příklad toospecify proměnných prostředí pro balíček kódu:</span><span class="sxs-lookup"><span data-stu-id="67c25-184">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

<span data-ttu-id="67c25-185">Tyto proměnné prostředí může být přepsána nastaveními v manifest aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="67c25-185">These environment variables can be overridden in hello application manifest:</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a><span data-ttu-id="67c25-186">Konfigurace mapování portu kontejneru na port hostitele a zjišťování mezi kontejnery</span><span class="sxs-lookup"><span data-stu-id="67c25-186">Configure container port-to-host port mapping and container-to-container discovery</span></span>
<span data-ttu-id="67c25-187">Toocommunicate port používaný na hostiteli nakonfigurujte hello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="67c25-187">Configure a host port used toocommunicate  with hello container.</span></span> <span data-ttu-id="67c25-188">vazbou portu Hello mapuje hello port, na které hello služby naslouchá uvnitř port tooa hello kontejneru na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-188">hello port binding maps hello port on which hello service is listening inside hello container tooa port on hello host.</span></span> <span data-ttu-id="67c25-189">Přidat `PortBinding` element v `ContainerHostPolicies` prvek hello ApplicationManifest.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="67c25-189">Add a `PortBinding` element in `ContainerHostPolicies` element of hello ApplicationManifest.xml file.</span></span>  <span data-ttu-id="67c25-190">V tomto článku `ContainerPort` je 80 (hello kontejneru zpřístupní port 80 a jako zadaný v hello soubor Docker) a `EndpointRef` je "Guest1TypeEndpoint" (hello dříve definované v hello service manifest koncového bodu).</span><span class="sxs-lookup"><span data-stu-id="67c25-190">For this article, `ContainerPort` is 80 (hello container exposes port 80, as specified in hello Dockerfile) and `EndpointRef` is "Guest1TypeEndpoint" (hello endpoint previously defined in hello service manifest).</span></span>  <span data-ttu-id="67c25-191">Příchozí žádosti o služby toohello na portu 8081 mapovat tooport 80 kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-191">Incoming requests toohello service on port 8081 are mapped tooport 80 on hello container.</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a><span data-ttu-id="67c25-192">Konfigurace ověřování registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="67c25-192">Configure container registry authentication</span></span>
<span data-ttu-id="67c25-193">Konfigurace ověřování kontejneru registru přidáním `RepositoryCredentials` příliš`ContainerHostPolicies` hello ApplicationManifest.xml souboru.</span><span class="sxs-lookup"><span data-stu-id="67c25-193">Configure container registry authentication by adding `RepositoryCredentials` too`ContainerHostPolicies` of hello ApplicationManifest.xml file.</span></span> <span data-ttu-id="67c25-194">Přidejte hello účet a heslo pro hello myregistry.azurecr.io kontejneru registru, který umožňuje bitovou kopii systému hello služby toodownload hello kontejneru z úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-194">Add hello account and password for hello myregistry.azurecr.io container registry, which allows hello service toodownload hello container image from hello repository.</span></span>

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

<span data-ttu-id="67c25-195">Doporučujeme, abyste šifrování hesla hello úložiště pomocí certifikátu šifrování, která nasadila tooall uzly clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-195">We recommend that you encrypt hello repository password by using an encipherment certificate that's deployed tooall nodes of hello cluster.</span></span> <span data-ttu-id="67c25-196">Když Service Fabric nasadí hello služby balíček toohello clusteru, certifikát šifrování hello je použité toodecrypt hello šifrovaný text.</span><span class="sxs-lookup"><span data-stu-id="67c25-196">When Service Fabric deploys hello service package toohello cluster, hello encipherment certificate is used toodecrypt hello cipher text.</span></span>  <span data-ttu-id="67c25-197">rutina Invoke-ServiceFabricEncryptText Hello je použité toocreate hello šifrovaný text hello hesla, která se přidá soubor ApplicationManifest.xml toohello.</span><span class="sxs-lookup"><span data-stu-id="67c25-197">hello Invoke-ServiceFabricEncryptText cmdlet is used toocreate hello cipher text for hello password, which is added toohello ApplicationManifest.xml file.</span></span>

<span data-ttu-id="67c25-198">Hello následující skript vytvoří nový certifikát podepsaný svým držitelem a vyexportuje ji tooa souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="67c25-198">hello following script creates a new self-signed certificate and exports it tooa PFX file.</span></span>  <span data-ttu-id="67c25-199">certifikát Hello je naimportován do existující trezor klíčů a pak nasadí cluster Service Fabric toohello.</span><span class="sxs-lookup"><span data-stu-id="67c25-199">hello certificate is imported into an existing key vault and then deployed toohello Service Fabric cluster.</span></span>

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
<span data-ttu-id="67c25-200">Šifrování hesla hello pomocí hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) rutiny.</span><span class="sxs-lookup"><span data-stu-id="67c25-200">Encrypt hello password using hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.</span></span>

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

<span data-ttu-id="67c25-201">Nahraďte heslo hello hello šifrovaný text vrácený hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) rutiny a nastavte `PasswordEncrypted` příliš "true".</span><span class="sxs-lookup"><span data-stu-id="67c25-201">Replace hello password with hello cipher text returned by hello [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet and set `PasswordEncrypted` too"true".</span></span>

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

## <a name="configure-isolation-mode"></a><span data-ttu-id="67c25-202">Konfigurace režimu izolace</span><span class="sxs-lookup"><span data-stu-id="67c25-202">Configure isolation mode</span></span>
<span data-ttu-id="67c25-203">Systém Windows podporuje pro kontejnery dva režimy izolace: procesy a Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="67c25-203">Windows supports two isolation modes for containers: process and Hyper-V.</span></span> <span data-ttu-id="67c25-204">V režimu izolace procesu hello hello všechny kontejnery hello systémem stejné hostitele počítače sdílenou složku hello jádra s hostitelem hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-204">With hello process isolation mode, all hello containers running on hello same host machine share hello kernel with hello host.</span></span> <span data-ttu-id="67c25-205">V režimu izolace hello technologie Hyper-V jsou izolovány. mezi každou kontejneru technologie Hyper-V a hostitelem kontejneru hello hello jádra.</span><span class="sxs-lookup"><span data-stu-id="67c25-205">With hello Hyper-V isolation mode, hello kernels are isolated between each Hyper-V container and hello container host.</span></span> <span data-ttu-id="67c25-206">režimu izolace Hello je uveden v hello `ContainerHostPolicies` element v souboru manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-206">hello isolation mode is specified in hello `ContainerHostPolicies` element in hello application manifest file.</span></span> <span data-ttu-id="67c25-207">režim Hello izolaci, které lze zadat `process`, `hyperv`, a `default`.</span><span class="sxs-lookup"><span data-stu-id="67c25-207">hello isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="67c25-208">režimu izolace výchozí Hello výchozí příliš`process` v systému Windows Server hostuje a použije se výchozí hodnota příliš`hyperv` na hostitelích s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="67c25-208">hello default isolation mode defaults too`process` on Windows Server hosts, and defaults too`hyperv` on Windows 10 hosts.</span></span> <span data-ttu-id="67c25-209">Hello následující fragment kódu ukazuje, jak je režimu izolace hello zadaný v souboru manifestu aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-209">hello following snippet shows how hello isolation mode is specified in hello application manifest file.</span></span>

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a><span data-ttu-id="67c25-210">Konfigurace zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="67c25-210">Configure resource governance</span></span>
<span data-ttu-id="67c25-211">[Zásady správného řízení prostředků](service-fabric-resource-governance.md) omezuje hello prostředky, které hello kontejneru můžete použít na hostiteli hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-211">[Resource governance](service-fabric-resource-governance.md) restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="67c25-212">Hello `ResourceGovernancePolicy` element, který je uveden v manifestu aplikace hello, je použít toodeclare prostředků limity pro balíček kódu služby.</span><span class="sxs-lookup"><span data-stu-id="67c25-212">hello `ResourceGovernancePolicy` element, which is specified in hello application manifest, is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="67c25-213">Dají se nastavit limity prostředků pro hello následující prostředky: paměti, MemorySwap, CpuShares (Relativní váha procesoru), MemoryReservationInMB, BlkioWeight (BlockIO relativní váhy).</span><span class="sxs-lookup"><span data-stu-id="67c25-213">Resource limits can be set for hello following resources: Memory, MemorySwap, CpuShares (CPU relative weight), MemoryReservationInMB, BlkioWeight (BlockIO relative weight).</span></span>  <span data-ttu-id="67c25-214">V tomto příkladu balíček služby Guest1Pkg získá jednoho jádra na uzlech clusteru hello, kde je umístěn.</span><span class="sxs-lookup"><span data-stu-id="67c25-214">In this example, service package Guest1Pkg gets one core on hello cluster nodes where it is placed.</span></span>  <span data-ttu-id="67c25-215">Omezení paměti jsou absolutní, takže balíček kódu hello je omezená too1024 MB paměti (a konfigurace soft záruku rezervace z stejné hello).</span><span class="sxs-lookup"><span data-stu-id="67c25-215">Memory limits are absolute, so hello code package is limited too1024 MB of memory (and a soft-guarantee reservation of hello same).</span></span> <span data-ttu-id="67c25-216">Kód balíčky (kontejnerů a procesy) nejsou možné tooallocate, které větší spotřebu paměti než tento limit a pokus toodo tak vede k výjimce z důvodu nedostatku paměti.</span><span class="sxs-lookup"><span data-stu-id="67c25-216">Code packages (containers or processes) are not able tooallocate more memory than this limit, and attempting toodo so results in an out-of-memory exception.</span></span> <span data-ttu-id="67c25-217">Pro toowork vynucení omezení prostředků musí mít všechny balíčky kódu v rámci balíčku služby zadaná paměťová omezení.</span><span class="sxs-lookup"><span data-stu-id="67c25-217">For resource limit enforcement toowork, all code packages within a service package should have memory limits specified.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-hello-container-application"></a><span data-ttu-id="67c25-218">Nasazení aplikace hello kontejneru</span><span class="sxs-lookup"><span data-stu-id="67c25-218">Deploy hello container application</span></span>
<span data-ttu-id="67c25-219">Uložte provedené změny a sestavení aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-219">Save all your changes and build hello application.</span></span> <span data-ttu-id="67c25-220">toopublish vaší aplikace, klikněte pravým tlačítkem na **MyFirstContainer** v Průzkumníku řešení a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="67c25-220">toopublish your application, right-click on **MyFirstContainer** in Solution Explorer and select **Publish**.</span></span>

<span data-ttu-id="67c25-221">V **koncového bodu připojení**, zadejte hello koncový bod správy pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="67c25-221">In **Connection Endpoint**, enter hello management endpoint for hello cluster.</span></span>  <span data-ttu-id="67c25-222">Příklad: containercluster.westus2.cloudapp.azure.com:19000.</span><span class="sxs-lookup"><span data-stu-id="67c25-222">For example, "containercluster.westus2.cloudapp.azure.com:19000".</span></span> <span data-ttu-id="67c25-223">Můžete najít připojení klienta hello koncového bodu v okně Přehled hello cluster v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="67c25-223">You can find hello client connection endpoint in hello Overview blade for your cluster in hello [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="67c25-224">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="67c25-224">Click **Publish**.</span></span>

<span data-ttu-id="67c25-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je webový nástroj pro kontrolu a správu aplikací a uzlů v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="67c25-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a web-based tool for inspecting and managing applications and nodes in a Service Fabric cluster.</span></span> <span data-ttu-id="67c25-226">Otevřete prohlížeč a přejděte toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer/a postupujte podle hello nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="67c25-226">Open a browser and navigate toohttp://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ and follow hello application deployment.</span></span>  <span data-ttu-id="67c25-227">Hello aplikace nasadí, ale je v chybovém stavu, dokud nebudou staženy hello image na uzlech clusteru hello, (které může trvat nějakou dobu, v závislosti na velikosti obrázku hello): ![chyba][1]</span><span class="sxs-lookup"><span data-stu-id="67c25-227">hello application deploys but is in an error state until hello image is downloaded on hello cluster nodes (which can take some time, depending on hello image size): ![Error][1]</span></span>

<span data-ttu-id="67c25-228">aplikace Hello je připravena, pokud je v ```Ready``` stavu: ![připraveno][2]</span><span class="sxs-lookup"><span data-stu-id="67c25-228">hello application is ready when it's in ```Ready``` state: ![Ready][2]</span></span>

<span data-ttu-id="67c25-229">Otevřete prohlížeč a přejděte toohttp://containercluster.westus2.cloudapp.azure.com:8081.</span><span class="sxs-lookup"><span data-stu-id="67c25-229">Open a browser and navigate toohttp://containercluster.westus2.cloudapp.azure.com:8081.</span></span> <span data-ttu-id="67c25-230">Měli byste vidět hello záhlaví "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="67c25-230">You should see hello heading "Hello World!"</span></span> <span data-ttu-id="67c25-231">Zobrazit v prohlížeči hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-231">display in hello browser.</span></span>

## <a name="clean-up"></a><span data-ttu-id="67c25-232">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="67c25-232">Clean up</span></span>
<span data-ttu-id="67c25-233">Pokračovat tooincur poplatky, zatímco hello clusteru je spuštěn, zvažte [odstranění clusteru](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="67c25-233">You continue tooincur charges while hello cluster is running, consider [deleting your cluster](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span></span>  <span data-ttu-id="67c25-234">[Party clustery](http://tryazureservicefabric.westus.cloudapp.azure.com/) se automaticky odstraní po několika hodinách.</span><span class="sxs-lookup"><span data-stu-id="67c25-234">[Party clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) are automatically deleted after a few hours.</span></span>

<span data-ttu-id="67c25-235">Po push hello image toohello kontejneru registru můžete odstranit obrázek místní hello z vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="67c25-235">After you push hello image toohello container registry you can delete hello local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="67c25-236">Kompletní příklad manifestů služby a aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="67c25-236">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="67c25-237">Tady jsou hello dokončení služby a aplikace manifesty používané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="67c25-237">Here are hello complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="67c25-238">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="67c25-238">ServiceManifest.xml</span></span>
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
### <a name="applicationmanifestxml"></a><span data-ttu-id="67c25-239">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="67c25-239">ApplicationManifest.xml</span></span>
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

## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="67c25-240">Konfigurace časového intervalu před vynuceným ukončením kontejneru</span><span class="sxs-lookup"><span data-stu-id="67c25-240">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="67c25-241">Časový interval pro hello runtime toowait můžete nakonfigurovat před hello kontejneru se odebere po hello odstranění služby (nebo přesunutí uzlu tooanother) bylo zahájeno.</span><span class="sxs-lookup"><span data-stu-id="67c25-241">You can configure a time interval for hello runtime toowait before hello container is removed after hello service deletion (or a move tooanother node) has started.</span></span> <span data-ttu-id="67c25-242">Konfigurace časového intervalu hello odešle hello `docker stop <time in seconds>` příkaz toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="67c25-242">Configuring hello time interval sends hello `docker stop <time in seconds>` command toohello container.</span></span>   <span data-ttu-id="67c25-243">Další podrobnosti najdete v dokumentaci k příkazu [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="67c25-243">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="67c25-244">toowait interval času Hello je zadaný v hello `Hosting` části.</span><span class="sxs-lookup"><span data-stu-id="67c25-244">hello time interval toowait is specified under hello `Hosting` section.</span></span> <span data-ttu-id="67c25-245">Následující fragment kódu manifestu clusteru Hello ukazuje, jak tooset hello interval čekání:</span><span class="sxs-lookup"><span data-stu-id="67c25-245">hello following cluster manifest snippet shows how tooset hello wait interval:</span></span>

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
<span data-ttu-id="67c25-246">Hello výchozí časový interval je nastaven too10 sekund.</span><span class="sxs-lookup"><span data-stu-id="67c25-246">hello default time interval is set too10 seconds.</span></span> <span data-ttu-id="67c25-247">Vzhledem k tomu, že tato konfigurace je dynamický, upgradu konfigurace pouze na hello clusteru aktualizace hello vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="67c25-247">Since this configuration is dynamic, a config only upgrade on hello cluster updates hello timeout.</span></span> 


## <a name="configure-hello-runtime-tooremove-unused-container-images"></a><span data-ttu-id="67c25-248">Konfigurace modulu runtime tooremove hello nepoužívané kontejneru obrázků</span><span class="sxs-lookup"><span data-stu-id="67c25-248">Configure hello runtime tooremove unused container images</span></span>

<span data-ttu-id="67c25-249">Můžete nakonfigurovat tooremove clusteru Service Fabric hello nepoužívané kontejneru bitové kopie z uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-249">You can configure hello Service Fabric cluster tooremove unused container images from hello node.</span></span> <span data-ttu-id="67c25-250">Tato konfigurace umožňuje toobe místa na disku nové zachytávání Pokud příliš mnoho obrázků kontejneru se nacházejí na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="67c25-250">This configuration allows disk space toobe recaptured if too many container images are present on hello node.</span></span>  <span data-ttu-id="67c25-251">tooenable tuto funkci, aktualizace hello `Hosting` část v manifestu clusteru hello, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="67c25-251">tooenable this feature, update hello `Hosting` section in hello cluster manifest as shown in hello following snippet:</span></span> 


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

<span data-ttu-id="67c25-252">Pro Image, které by neměly být odstraněny, můžete je zadat v rámci hello `ContainerImagesToSkip` parametr.</span><span class="sxs-lookup"><span data-stu-id="67c25-252">For images that should not be deleted, you can specify them under hello `ContainerImagesToSkip` parameter.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="67c25-253">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67c25-253">Next steps</span></span>
* <span data-ttu-id="67c25-254">Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="67c25-254">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="67c25-255">Čtení hello [nasazení aplikace .NET v kontejneru](service-fabric-host-app-in-a-container.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="67c25-255">Read hello [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="67c25-256">Další informace o hello Service Fabric [životního cyklu aplikace](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="67c25-256">Learn about hello Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="67c25-257">Najdete v článku věnovaném hello [ukázky kódu Service Fabric kontejneru](https://github.com/Azure-Samples/service-fabric-dotnet-containers) na Githubu.</span><span class="sxs-lookup"><span data-stu-id="67c25-257">Checkout hello [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
