---
title: "Vytvoření aplikace Azure Service Fabric typu kontejner | Dokumentace Microsoftu"
description: "Vytvoříte svou první aplikaci typu kontejner pro Windows na platformě Azure Service Fabric.  Sestavíte image Dockeru s aplikací v Pythonu, nahrajete image do registru kontejneru a sestavíte a nasadíte aplikaci Service Fabric typu kontejner."
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
ms.openlocfilehash: 025bde02b3f342ec3399d51819d1fa8a91f11374
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a><span data-ttu-id="29623-104">Vytvoření první aplikace Service Fabric typu kontejner v systému Windows</span><span class="sxs-lookup"><span data-stu-id="29623-104">Create your first Service Fabric container application on Windows</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29623-105">Windows</span><span class="sxs-lookup"><span data-stu-id="29623-105">Windows</span></span>](service-fabric-get-started-containers.md)
> * [<span data-ttu-id="29623-106">Linux</span><span class="sxs-lookup"><span data-stu-id="29623-106">Linux</span></span>](service-fabric-get-started-containers-linux.md)

<span data-ttu-id="29623-107">Spuštění existující aplikace v kontejneru Windows v clusteru Service Fabric nevyžaduje žádné změny aplikace.</span><span class="sxs-lookup"><span data-stu-id="29623-107">Running an existing application in a Windows container on a Service Fabric cluster doesn't require any changes to your application.</span></span> <span data-ttu-id="29623-108">Tento článek vás provede vytvořením image Dockeru obsahující webovou aplikaci Python [Flask](http://flask.pocoo.org/) a jejím nasazením do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="29623-108">This article walks you through creating a Docker image containing a Python [Flask](http://flask.pocoo.org/) web application and deploying it to a Service Fabric cluster.</span></span>  <span data-ttu-id="29623-109">Kontejnerizovanou aplikaci budete také sdílet prostřednictvím služby [Azure Container Registry](/azure/container-registry/).</span><span class="sxs-lookup"><span data-stu-id="29623-109">You will also share your containerized application through [Azure Container Registry](/azure/container-registry/).</span></span>  <span data-ttu-id="29623-110">Tento článek předpokládá základní znalost Dockeru.</span><span class="sxs-lookup"><span data-stu-id="29623-110">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="29623-111">Informace o Dockeru najdete v článku [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Přehled Dockeru).</span><span class="sxs-lookup"><span data-stu-id="29623-111">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29623-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="29623-112">Prerequisites</span></span>
<span data-ttu-id="29623-113">Vývojový počítač s:</span><span class="sxs-lookup"><span data-stu-id="29623-113">A development computer running:</span></span>
* <span data-ttu-id="29623-114">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="29623-114">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="29623-115">[Sada Service Fabric SDK a nástroje](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="29623-115">[Service Fabric SDK and tools](service-fabric-get-started.md).</span></span>
*  <span data-ttu-id="29623-116">Docker pro Windows.</span><span class="sxs-lookup"><span data-stu-id="29623-116">Docker for Windows.</span></span>  <span data-ttu-id="29623-117">[Získejte Docker CE pro Windows (stabilní verze)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span><span class="sxs-lookup"><span data-stu-id="29623-117">[Get Docker CE for Windows (stable)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description).</span></span> <span data-ttu-id="29623-118">Po nainstalování a spuštění Dockeru klikněte pravým tlačítkem myši na ikonu na hlavním panelu a vyberte **Switch to Windows containers** (Přepnout na kontejnery Windows).</span><span class="sxs-lookup"><span data-stu-id="29623-118">After installing and starting Docker, right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="29623-119">To se vyžaduje pro spuštění imagí Dockeru založených na Windows.</span><span class="sxs-lookup"><span data-stu-id="29623-119">This is required to run Docker images based on Windows.</span></span>

<span data-ttu-id="29623-120">Cluster Windows se třemi nebo více uzly spuštěnými na Windows Serveru 2016 s kontejnery – [Vytvořte cluster](service-fabric-cluster-creation-via-portal.md) nebo [vyzkoušejte Service Fabric zdarma](https://aka.ms/tryservicefabric).</span><span class="sxs-lookup"><span data-stu-id="29623-120">A Windows cluster with three or more nodes running on Windows Server 2016 with Containers- [Create a cluster](service-fabric-cluster-creation-via-portal.md) or [try Service Fabric for free](https://aka.ms/tryservicefabric).</span></span>

<span data-ttu-id="29623-121">Registr ve službě Azure Container Registry – [Vytvořte registr kontejneru](../container-registry/container-registry-get-started-portal.md) ve svém předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="29623-121">A registry in Azure Container Registry - [Create a container registry](../container-registry/container-registry-get-started-portal.md) in your Azure subscription.</span></span>

## <a name="define-the-docker-container"></a><span data-ttu-id="29623-122">Definice kontejneru Dockeru</span><span class="sxs-lookup"><span data-stu-id="29623-122">Define the Docker container</span></span>
<span data-ttu-id="29623-123">Sestavte image založenou na [imagi Pythonu](https://hub.docker.com/_/python/), která se nachází na Docker Hubu.</span><span class="sxs-lookup"><span data-stu-id="29623-123">Build an image based on the [Python image](https://hub.docker.com/_/python/) located on Docker Hub.</span></span>

<span data-ttu-id="29623-124">Definujte kontejner Dockeru v souboru Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="29623-124">Define your Docker container in a Dockerfile.</span></span> <span data-ttu-id="29623-125">Soubor Dockerfile obsahuje pokyny k nastavení prostředí uvnitř kontejneru, načtení aplikace, kterou chcete pustit, a mapování portů.</span><span class="sxs-lookup"><span data-stu-id="29623-125">The Dockerfile contains instructions for setting up the environment inside your container, loading the application you want to run, and mapping ports.</span></span> <span data-ttu-id="29623-126">Soubor Dockerfile je vstupem příkazu `docker build`, který vytvoří image.</span><span class="sxs-lookup"><span data-stu-id="29623-126">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="29623-127">Vytvořte prázdný adresář a soubor *Dockerfile* (bez přípony souboru).</span><span class="sxs-lookup"><span data-stu-id="29623-127">Create an empty directory and create the file *Dockerfile* (with no file extension).</span></span> <span data-ttu-id="29623-128">Do souboru *Dockerfile* přidejte následující a uložte změny:</span><span class="sxs-lookup"><span data-stu-id="29623-128">Add the following to *Dockerfile* and save your changes:</span></span>

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

<span data-ttu-id="29623-129">Další informace najdete v [referenčních informacích k souboru Dockerfile](https://docs.docker.com/engine/reference/builder/).</span><span class="sxs-lookup"><span data-stu-id="29623-129">Read the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/) for more information.</span></span>

## <a name="create-a-simple-web-application"></a><span data-ttu-id="29623-130">Vytvoření jednoduché webové aplikace</span><span class="sxs-lookup"><span data-stu-id="29623-130">Create a simple web application</span></span>
<span data-ttu-id="29623-131">Vytvořte webovou aplikaci Flask, která naslouchá na portu 80 a vrací „Hello World!“.</span><span class="sxs-lookup"><span data-stu-id="29623-131">Create a Flask web application listening on port 80 that returns "Hello World!".</span></span>  <span data-ttu-id="29623-132">Ve stejném adresáři vytvořte soubor *requirements.txt*.</span><span class="sxs-lookup"><span data-stu-id="29623-132">In the same directory, create the file *requirements.txt*.</span></span>  <span data-ttu-id="29623-133">Přidejte do něj následující a uložte změny:</span><span class="sxs-lookup"><span data-stu-id="29623-133">Add the following and save your changes:</span></span>
```
Flask
```

<span data-ttu-id="29623-134">Vytvořte také soubor *app.py* a přidejte do něj následující:</span><span class="sxs-lookup"><span data-stu-id="29623-134">Also create the *app.py* file and add the following:</span></span>

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
## <a name="build-the-image"></a><span data-ttu-id="29623-135">Sestavení image</span><span class="sxs-lookup"><span data-stu-id="29623-135">Build the image</span></span>
<span data-ttu-id="29623-136">Spuštěním příkazu `docker build` vytvořte image spouštějící vaši webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29623-136">Run the `docker build` command to create the image that runs your web application.</span></span> <span data-ttu-id="29623-137">Otevřete okno PowerShellu a přejděte do adresáře, který obsahuje soubor Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="29623-137">Open a PowerShell window and navigate to the directory containing the Dockerfile.</span></span> <span data-ttu-id="29623-138">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="29623-138">Run the following command:</span></span>

```
docker build -t helloworldapp .
```

<span data-ttu-id="29623-139">Tento příkaz sestaví novou image podle pokynů ve vašem souboru Dockerfile a pojmenuje ji (označení -t) „helloworldapp“.</span><span class="sxs-lookup"><span data-stu-id="29623-139">This command builds the new image using the instructions in your Dockerfile, naming (-t tagging) the image "helloworldapp".</span></span> <span data-ttu-id="29623-140">Sestavení image si z Docker Hubu přetáhne základní image a vytvoří novou image, která přidá vaši aplikaci nad základní image.</span><span class="sxs-lookup"><span data-stu-id="29623-140">Building an image pulls the base image down from Docker Hub and creates a new image that adds your application on top of the base image.</span></span>  

<span data-ttu-id="29623-141">Po dokončení příkazu pro sestavení spusťte příkaz `docker images` a zobrazte informace o nové imagi:</span><span class="sxs-lookup"><span data-stu-id="29623-141">Once the build command completes, run the `docker images` command to see information on the new image:</span></span>

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a><span data-ttu-id="29623-142">Místní spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="29623-142">Run the application locally</span></span>
<span data-ttu-id="29623-143">Než image nahrajete do registru kontejneru, ověřte ji místně.</span><span class="sxs-lookup"><span data-stu-id="29623-143">Verify your image locally before pushing it the container registry.</span></span>  

<span data-ttu-id="29623-144">Spusťte aplikaci:</span><span class="sxs-lookup"><span data-stu-id="29623-144">Run the application:</span></span>

```
docker run -d --name my-web-site helloworldapp
```

<span data-ttu-id="29623-145">Parametr *name* udává název spuštěného kontejneru (namísto ID kontejneru).</span><span class="sxs-lookup"><span data-stu-id="29623-145">*name* gives a name to the running container (instead of the container ID).</span></span>

<span data-ttu-id="29623-146">Po spuštění kontejneru vyhledejte jeho IP adresu, abyste se ke spuštěnému kontejneru mohli připojit z prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="29623-146">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

<span data-ttu-id="29623-147">Připojte se ke spuštěnému kontejneru.</span><span class="sxs-lookup"><span data-stu-id="29623-147">Connect to the running container.</span></span>  <span data-ttu-id="29623-148">Otevřete webový prohlížeč a přejděte na vrácenou IP adresu, například http://172.31.194.61.</span><span class="sxs-lookup"><span data-stu-id="29623-148">Open a web browser pointing to the IP address returned, for example "http://172.31.194.61".</span></span> <span data-ttu-id="29623-149">V prohlížeči by se měl zobrazit</span><span class="sxs-lookup"><span data-stu-id="29623-149">You should see the heading "Hello World!"</span></span> <span data-ttu-id="29623-150">nadpis „Hello World!“.</span><span class="sxs-lookup"><span data-stu-id="29623-150">display in the browser.</span></span>

<span data-ttu-id="29623-151">Pokud chcete kontejner zastavit, spusťte:</span><span class="sxs-lookup"><span data-stu-id="29623-151">To stop your container, run:</span></span>

```
docker stop my-web-site
```

<span data-ttu-id="29623-152">Odstraňte kontejner z vývojového počítače:</span><span class="sxs-lookup"><span data-stu-id="29623-152">Delete the container from your development machine:</span></span>

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-the-image-to-the-container-registry"></a><span data-ttu-id="29623-153">Nahrání image do registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="29623-153">Push the image to the container registry</span></span>
<span data-ttu-id="29623-154">Po ověření, že se kontejner spustí na vývojovém počítači, nahrajte image do vašeho registru ve službě Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="29623-154">After you verify that the container runs on your development machine, push the image to your registry in Azure Container Registry.</span></span>

<span data-ttu-id="29623-155">Spusťte příkaz ``docker login`` a přihlaste se ke svému registru kontejnerů pomocí [přihlašovacích údajů registru](../container-registry/container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="29623-155">Run ``docker login`` to log in to your container registry with your [registry credentials](../container-registry/container-registry-authentication.md).</span></span>

<span data-ttu-id="29623-156">Následující příklad předá ID a heslo [instančního objektu](../active-directory/active-directory-application-objects.md) Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="29623-156">The following example passes the ID and password of an Azure Active Directory [service principal](../active-directory/active-directory-application-objects.md).</span></span> <span data-ttu-id="29623-157">Instanční objekt jste k registru mohli přiřadit například pro účely scénáře automatizace.</span><span class="sxs-lookup"><span data-stu-id="29623-157">For example, you might have assigned a service principal to your registry for an automation scenario.</span></span> <span data-ttu-id="29623-158">Nebo se můžete přihlásit pomocí uživatelského jména a hesla registru.</span><span class="sxs-lookup"><span data-stu-id="29623-158">Or, you could login using your registry username and password.</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="29623-159">Následující příkaz vytvoří značku, neboli alias, image s plně kvalifikovanou cestou k vašemu registru.</span><span class="sxs-lookup"><span data-stu-id="29623-159">The following command creates a tag, or alias, of the image, with a fully qualified path to your registry.</span></span> <span data-ttu-id="29623-160">Tento příklad umístí image do oboru názvů ```samples```, aby se zabránilo nepořádku v kořenovém adresáři registru.</span><span class="sxs-lookup"><span data-stu-id="29623-160">This example places the image in the ```samples``` namespace to avoid clutter in the root of the registry.</span></span>

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

<span data-ttu-id="29623-161">Nahrajte image do registru kontejneru:</span><span class="sxs-lookup"><span data-stu-id="29623-161">Push the image to your container registry:</span></span>

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-the-containerized-service-in-visual-studio"></a><span data-ttu-id="29623-162">Vytvoření kontejnerizované služby v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29623-162">Create the containerized service in Visual Studio</span></span>
<span data-ttu-id="29623-163">Sada Service Fabric SDK a nástroje poskytují šablonu služby, která vám pomůže s vytvořením kontejnerizované aplikace.</span><span class="sxs-lookup"><span data-stu-id="29623-163">The Service Fabric SDK and tools provide a service template to help you create a containerized application.</span></span>

1. <span data-ttu-id="29623-164">Spusťte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29623-164">Start Visual Studio.</span></span>  <span data-ttu-id="29623-165">Vyberte **Soubor** > **Nový** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="29623-165">Select **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="29623-166">Vyberte **Aplikace Service Fabric**, pojmenujte ji MyFirstContainer a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29623-166">Select **Service Fabric application**, name it "MyFirstContainer", and click **OK**.</span></span>
3. <span data-ttu-id="29623-167">Ze **seznamu šablon** služeb vyberte **Guest Container**.</span><span class="sxs-lookup"><span data-stu-id="29623-167">Select **Guest Container** from the list of **service templates**.</span></span>
4. <span data-ttu-id="29623-168">Do pole **Název image** zadejte „myregistry.azurecr.io/samples/helloworldapp“, tedy image, kterou jste nahráli do úložiště kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="29623-168">In **Image Name** enter "myregistry.azurecr.io/samples/helloworldapp", the image you pushed to your container repository.</span></span>
5. <span data-ttu-id="29623-169">Zadejte název služby a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="29623-169">Give your service a name, and click **OK**.</span></span>

## <a name="configure-communication"></a><span data-ttu-id="29623-170">Konfigurace komunikace</span><span class="sxs-lookup"><span data-stu-id="29623-170">Configure communication</span></span>
<span data-ttu-id="29623-171">Kontejnerizovaná služba potřebuje koncový bod pro komunikaci.</span><span class="sxs-lookup"><span data-stu-id="29623-171">The containerized service needs an endpoint for communication.</span></span> <span data-ttu-id="29623-172">Do souboru ServiceManifest.xml přidejte element `Endpoint` s protokolem, portem a typem.</span><span class="sxs-lookup"><span data-stu-id="29623-172">Add an `Endpoint` element with the protocol, port, and type to the ServiceManifest.xml file.</span></span> <span data-ttu-id="29623-173">Pro účely tohoto článku kontejnerizovaná služba naslouchá na portu 8081.</span><span class="sxs-lookup"><span data-stu-id="29623-173">For this article, the containerized service listens on port 8081.</span></span>  <span data-ttu-id="29623-174">V tomto příkladu se používá pevný port 8081.</span><span class="sxs-lookup"><span data-stu-id="29623-174">In this example, a fixed port 8081 is used.</span></span>  <span data-ttu-id="29623-175">Pokud port není zadaný, zvolí se v rozsahu portů aplikace náhodně.</span><span class="sxs-lookup"><span data-stu-id="29623-175">If no port is specified, a random port from the application port range is chosen.</span></span>  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

<span data-ttu-id="29623-176">Když Service Fabric definuje koncový bod, publikuje ho ve službě pojmenování.</span><span class="sxs-lookup"><span data-stu-id="29623-176">By defining an endpoint, Service Fabric publishes the endpoint to the Naming service.</span></span>  <span data-ttu-id="29623-177">Ostatní služby spuštěné v clusteru mohou tento kontejner vyhledat.</span><span class="sxs-lookup"><span data-stu-id="29623-177">Other services running in the cluster can resolve this container.</span></span>  <span data-ttu-id="29623-178">Ke komunikaci mezi kontejnery můžete také využít [reverzní proxy server](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="29623-178">You can also perform container-to-container communication using the [reverse proxy](service-fabric-reverseproxy.md).</span></span>  <span data-ttu-id="29623-179">Komunikace se provede tak, že se reverznímu proxy serveru poskytne port pro naslouchání HTTP a název služeb, se kterými chcete komunikovat, jako proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="29623-179">Communication is performed by providing the reverse proxy HTTP listening port and the name of the services that you want to communicate with as environment variables.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="29623-180">Konfigurace a nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="29623-180">Configure and set environment variables</span></span>
<span data-ttu-id="29623-181">Proměnné prostředí je možné zadat pro každý balíček kódu v manifestu služby.</span><span class="sxs-lookup"><span data-stu-id="29623-181">Environment variables can be specified for each code package in the service manifest.</span></span> <span data-ttu-id="29623-182">Tato funkce je dostupná pro všechny služby, bez ohledu na to, jestli jsou nasazené jako kontejnery, procesy nebo spustitelné soubory typu Host.</span><span class="sxs-lookup"><span data-stu-id="29623-182">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="29623-183">Hodnoty proměnných prostředí můžete přepsat v manifestu aplikace, nebo je můžete zadat v průběhu nasazení jako parametry aplikace.</span><span class="sxs-lookup"><span data-stu-id="29623-183">You can override environment variable values in the application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="29623-184">Následující fragment kódu XML manifestu služby ukazuje příklad toho, jak zadat proměnné prostředí pro balíček kódu:</span><span class="sxs-lookup"><span data-stu-id="29623-184">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

<span data-ttu-id="29623-185">Tyto proměnné prostředí je možné přepsat v manifestu aplikace:</span><span class="sxs-lookup"><span data-stu-id="29623-185">These environment variables can be overridden in the application manifest:</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a><span data-ttu-id="29623-186">Konfigurace mapování portu kontejneru na port hostitele a zjišťování mezi kontejnery</span><span class="sxs-lookup"><span data-stu-id="29623-186">Configure container port-to-host port mapping and container-to-container discovery</span></span>
<span data-ttu-id="29623-187">Nakonfigurujte port hostitele používaný ke komunikaci s kontejnerem.</span><span class="sxs-lookup"><span data-stu-id="29623-187">Configure a host port used to communicate  with the container.</span></span> <span data-ttu-id="29623-188">Vazba portu mapuje port, na kterém služba naslouchá uvnitř kontejneru, na port na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="29623-188">The port binding maps the port on which the service is listening inside the container to a port on the host.</span></span> <span data-ttu-id="29623-189">Přidejte element `PortBinding` do `ContainerHostPolicies` v souboru ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="29623-189">Add a `PortBinding` element in `ContainerHostPolicies` element of the ApplicationManifest.xml file.</span></span>  <span data-ttu-id="29623-190">Pro účely tohoto článku je `ContainerPort` nastaven na 80 (kontejner zpřístupňuje port 80, jak je uvedené v souboru Dockerfile) a `EndpointRef` je „Guest1TypeEndpoint“ (koncový bod dříve definovaný v manifestu služby).</span><span class="sxs-lookup"><span data-stu-id="29623-190">For this article, `ContainerPort` is 80 (the container exposes port 80, as specified in the Dockerfile) and `EndpointRef` is "Guest1TypeEndpoint" (the endpoint previously defined in the service manifest).</span></span>  <span data-ttu-id="29623-191">Příchozí požadavky na službu na portu 8081 se mapují na port 80 v kontejneru.</span><span class="sxs-lookup"><span data-stu-id="29623-191">Incoming requests to the service on port 8081 are mapped to port 80 on the container.</span></span>

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a><span data-ttu-id="29623-192">Konfigurace ověřování registru kontejneru</span><span class="sxs-lookup"><span data-stu-id="29623-192">Configure container registry authentication</span></span>
<span data-ttu-id="29623-193">Nakonfigurujte ověřování registru kontejneru přidáním `RepositoryCredentials` do `ContainerHostPolicies` v souboru ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="29623-193">Configure container registry authentication by adding `RepositoryCredentials` to `ContainerHostPolicies` of the ApplicationManifest.xml file.</span></span> <span data-ttu-id="29623-194">Přidejte účet a heslo pro registr kontejneru myregistry.azurecr.io, který službě umožňuje stáhnout image kontejneru z úložiště.</span><span class="sxs-lookup"><span data-stu-id="29623-194">Add the account and password for the myregistry.azurecr.io container registry, which allows the service to download the container image from the repository.</span></span>

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

<span data-ttu-id="29623-195">Doporučujeme šifrovat heslo úložiště pomocí certifikátu šifrování, který je nasazený na všechny uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="29623-195">We recommend that you encrypt the repository password by using an encipherment certificate that's deployed to all nodes of the cluster.</span></span> <span data-ttu-id="29623-196">Když Service Fabric nasadí balíček služby do clusteru, certifikát šifrování se použije k dešifrování šifrovaného textu.</span><span class="sxs-lookup"><span data-stu-id="29623-196">When Service Fabric deploys the service package to the cluster, the encipherment certificate is used to decrypt the cipher text.</span></span>  <span data-ttu-id="29623-197">Rutina Invoke-ServiceFabricEncryptText se používá k vytvoření šifrovaného textu pro heslo, který se přidá do souboru ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="29623-197">The Invoke-ServiceFabricEncryptText cmdlet is used to create the cipher text for the password, which is added to the ApplicationManifest.xml file.</span></span>

<span data-ttu-id="29623-198">Následující skript vytvoří nový certifikát podepsaný svým držitelem a vyexportuje ho do souboru PFX.</span><span class="sxs-lookup"><span data-stu-id="29623-198">The following script creates a new self-signed certificate and exports it to a PFX file.</span></span>  <span data-ttu-id="29623-199">Certifikát se naimportuje do existujícího trezoru klíčů a potom nasadí do clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="29623-199">The certificate is imported into an existing key vault and then deployed to the Service Fabric cluster.</span></span>

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

# Create a self signed cert, export to PFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import the certificate to an existing key vault.  The key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add the certificate to all the VMs in the cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
<span data-ttu-id="29623-200">K zašifrování hesla použijte rutinu [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps).</span><span class="sxs-lookup"><span data-stu-id="29623-200">Encrypt the password using the [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet.</span></span>

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

<span data-ttu-id="29623-201">Heslo nahraďte šifrovaným textem, který vrátila rutina [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps), a `PasswordEncrypted` nastavte na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="29623-201">Replace the password with the cipher text returned by the [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet and set `PasswordEncrypted` to "true".</span></span>

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

## <a name="configure-isolation-mode"></a><span data-ttu-id="29623-202">Konfigurace režimu izolace</span><span class="sxs-lookup"><span data-stu-id="29623-202">Configure isolation mode</span></span>
<span data-ttu-id="29623-203">Systém Windows podporuje pro kontejnery dva režimy izolace: procesy a Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="29623-203">Windows supports two isolation modes for containers: process and Hyper-V.</span></span> <span data-ttu-id="29623-204">V režimu izolace procesů všechny kontejnery spuštěné na stejném hostitelském počítači sdílejí jádro s hostitelem.</span><span class="sxs-lookup"><span data-stu-id="29623-204">With the process isolation mode, all the containers running on the same host machine share the kernel with the host.</span></span> <span data-ttu-id="29623-205">V režimu izolace Hyper-V se jádra pro jednotlivé kontejnery Hyper-V a hostitele kontejneru izolují.</span><span class="sxs-lookup"><span data-stu-id="29623-205">With the Hyper-V isolation mode, the kernels are isolated between each Hyper-V container and the container host.</span></span> <span data-ttu-id="29623-206">Režim izolace určuje element `ContainerHostPolicies` v souboru manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="29623-206">The isolation mode is specified in the `ContainerHostPolicies` element in the application manifest file.</span></span> <span data-ttu-id="29623-207">Je možné zadat tyto režimy izolace: `process`, `hyperv` a `default`.</span><span class="sxs-lookup"><span data-stu-id="29623-207">The isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="29623-208">Výchozím režimem izolace pro hostitele se systémem Windows Server je režim `process`. Výchozím režimem u hostitelů se systémem Windows 10 je `hyperv`.</span><span class="sxs-lookup"><span data-stu-id="29623-208">The default isolation mode defaults to `process` on Windows Server hosts, and defaults to `hyperv` on Windows 10 hosts.</span></span> <span data-ttu-id="29623-209">Následující fragment kódu ukazuje, jakým způsobem je režim izolace určený v souboru manifestu aplikace.</span><span class="sxs-lookup"><span data-stu-id="29623-209">The following snippet shows how the isolation mode is specified in the application manifest file.</span></span>

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a><span data-ttu-id="29623-210">Konfigurace zásad správného řízení prostředků</span><span class="sxs-lookup"><span data-stu-id="29623-210">Configure resource governance</span></span>
<span data-ttu-id="29623-211">[Zásady správného řízení prostředků](service-fabric-resource-governance.md) omezují prostředky, které kontejner může použít na hostiteli.</span><span class="sxs-lookup"><span data-stu-id="29623-211">[Resource governance](service-fabric-resource-governance.md) restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="29623-212">Element `ResourceGovernancePolicy`, který je zadaný v manifestu aplikace, slouží k deklaraci omezení prostředků pro balíček kódu služby.</span><span class="sxs-lookup"><span data-stu-id="29623-212">The `ResourceGovernancePolicy` element, which is specified in the application manifest, is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="29623-213">Omezení prostředků je možné nastavit pro tyto prostředky: Memory, MemorySwap, CpuShares (relativní váha CPU), MemoryReservationInMB, BlkioWeight (relativní váha BlockIO).</span><span class="sxs-lookup"><span data-stu-id="29623-213">Resource limits can be set for the following resources: Memory, MemorySwap, CpuShares (CPU relative weight), MemoryReservationInMB, BlkioWeight (BlockIO relative weight).</span></span>  <span data-ttu-id="29623-214">V tomto příkladu balíček služby Guest1Pkg získá jedno jádro na uzlech clusteru, kde je umístěný.</span><span class="sxs-lookup"><span data-stu-id="29623-214">In this example, service package Guest1Pkg gets one core on the cluster nodes where it is placed.</span></span>  <span data-ttu-id="29623-215">Omezení paměti jsou absolutní, takže balíček kódu je omezený na 1024 MB paměti (a má tuto paměť softwarově vyhrazenou).</span><span class="sxs-lookup"><span data-stu-id="29623-215">Memory limits are absolute, so the code package is limited to 1024 MB of memory (and a soft-guarantee reservation of the same).</span></span> <span data-ttu-id="29623-216">Balíčky kódu (kontejnery a procesy) nejsou schopné přidělit víc paměti, než je toto omezení, a případný pokus o takové přidělení má za následek výjimku z důvodu nedostatku paměti.</span><span class="sxs-lookup"><span data-stu-id="29623-216">Code packages (containers or processes) are not able to allocate more memory than this limit, and attempting to do so results in an out-of-memory exception.</span></span> <span data-ttu-id="29623-217">Aby vynucení omezení prostředků fungovala, musí být omezení paměti zadaná pro všechny balíčky kódu v rámci balíčku služby.</span><span class="sxs-lookup"><span data-stu-id="29623-217">For resource limit enforcement to work, all code packages within a service package should have memory limits specified.</span></span>

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-the-container-application"></a><span data-ttu-id="29623-218">Nasazení aplikace typu kontejner</span><span class="sxs-lookup"><span data-stu-id="29623-218">Deploy the container application</span></span>
<span data-ttu-id="29623-219">Uložte všechny provedené změny a sestavte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="29623-219">Save all your changes and build the application.</span></span> <span data-ttu-id="29623-220">Pokud chcete aplikaci publikovat, klikněte pravým tlačítkem na **MyFirstContainer** v Průzkumníku řešení a vyberte **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="29623-220">To publish your application, right-click on **MyFirstContainer** in Solution Explorer and select **Publish**.</span></span>

<span data-ttu-id="29623-221">Do pole **Koncový bod připojení** zadejte koncový bod správy pro příslušný cluster.</span><span class="sxs-lookup"><span data-stu-id="29623-221">In **Connection Endpoint**, enter the management endpoint for the cluster.</span></span>  <span data-ttu-id="29623-222">Příklad: containercluster.westus2.cloudapp.azure.com:19000.</span><span class="sxs-lookup"><span data-stu-id="29623-222">For example, "containercluster.westus2.cloudapp.azure.com:19000".</span></span> <span data-ttu-id="29623-223">Koncový bod připojení ke klientu najdete v okně Přehled pro váš cluster na webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="29623-223">You can find the client connection endpoint in the Overview blade for your cluster in the [Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="29623-224">Klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="29623-224">Click **Publish**.</span></span>

<span data-ttu-id="29623-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) je webový nástroj pro kontrolu a správu aplikací a uzlů v clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="29623-225">[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) is a web-based tool for inspecting and managing applications and nodes in a Service Fabric cluster.</span></span> <span data-ttu-id="29623-226">Otevřete prohlížeč, přejděte na adresu http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ a sledujte nasazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="29623-226">Open a browser and navigate to http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ and follow the application deployment.</span></span>  <span data-ttu-id="29623-227">Aplikace se nasadí, ale bude v chybovém stavu, dokud se image nestáhne na uzlech clusteru (což v závislosti na velikosti image může nějakou dobu trvat): ![Chyba][1]</span><span class="sxs-lookup"><span data-stu-id="29623-227">The application deploys but is in an error state until the image is downloaded on the cluster nodes (which can take some time, depending on the image size): ![Error][1]</span></span>

<span data-ttu-id="29623-228">Aplikace je připravena, když je ve stavu ```Ready```: ![Připraveno][2]</span><span class="sxs-lookup"><span data-stu-id="29623-228">The application is ready when it's in ```Ready``` state: ![Ready][2]</span></span>

<span data-ttu-id="29623-229">Otevřete prohlížeč a přejděte na adresu http://containercluster.westus2.cloudapp.azure.com:8081.</span><span class="sxs-lookup"><span data-stu-id="29623-229">Open a browser and navigate to http://containercluster.westus2.cloudapp.azure.com:8081.</span></span> <span data-ttu-id="29623-230">V prohlížeči by se měl zobrazit</span><span class="sxs-lookup"><span data-stu-id="29623-230">You should see the heading "Hello World!"</span></span> <span data-ttu-id="29623-231">nadpis „Hello World!“.</span><span class="sxs-lookup"><span data-stu-id="29623-231">display in the browser.</span></span>

## <a name="clean-up"></a><span data-ttu-id="29623-232">Vyčištění</span><span class="sxs-lookup"><span data-stu-id="29623-232">Clean up</span></span>
<span data-ttu-id="29623-233">Za spuštěný cluster se vám stále účtují poplatky, proto zvažte [odstranění clusteru](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span><span class="sxs-lookup"><span data-stu-id="29623-233">You continue to incur charges while the cluster is running, consider [deleting your cluster](service-fabric-get-started-azure-cluster.md#remove-the-cluster).</span></span>  <span data-ttu-id="29623-234">[Party clustery](http://tryazureservicefabric.westus.cloudapp.azure.com/) se automaticky odstraní po několika hodinách.</span><span class="sxs-lookup"><span data-stu-id="29623-234">[Party clusters](http://tryazureservicefabric.westus.cloudapp.azure.com/) are automatically deleted after a few hours.</span></span>

<span data-ttu-id="29623-235">Po nahrání image do registru kontejneru můžete odstranit místní image z vývojového počítače:</span><span class="sxs-lookup"><span data-stu-id="29623-235">After you push the image to the container registry you can delete the local image from your development computer:</span></span>

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a><span data-ttu-id="29623-236">Kompletní příklad manifestů služby a aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="29623-236">Complete example Service Fabric application and service manifests</span></span>
<span data-ttu-id="29623-237">Tady jsou kompletní manifesty aplikace a služby použité v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="29623-237">Here are the complete service and application manifests used in this article.</span></span>

### <a name="servicemanifestxml"></a><span data-ttu-id="29623-238">ServiceManifest.xml</span><span class="sxs-lookup"><span data-stu-id="29623-238">ServiceManifest.xml</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->    
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a><span data-ttu-id="29623-239">ApplicationManifest.xml</span><span class="sxs-lookup"><span data-stu-id="29623-239">ApplicationManifest.xml</span></span>
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
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
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
    <!-- The section below creates instances of service types, when an instance of this
         application type is created. You can also create one or more instances of service type using the
         ServiceFabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="configure-time-interval-before-container-is-force-terminated"></a><span data-ttu-id="29623-240">Konfigurace časového intervalu před vynuceným ukončením kontejneru</span><span class="sxs-lookup"><span data-stu-id="29623-240">Configure time interval before container is force terminated</span></span>

<span data-ttu-id="29623-241">Můžete nakonfigurovat časový interval, který určuje, jak dlouho modul runtime počká před odebráním kontejneru po zahájení odstraňování služby (nebo jejího přesunu do jiného uzlu).</span><span class="sxs-lookup"><span data-stu-id="29623-241">You can configure a time interval for the runtime to wait before the container is removed after the service deletion (or a move to another node) has started.</span></span> <span data-ttu-id="29623-242">Konfigurací časového intervalu se do kontejneru odešle příkaz `docker stop <time in seconds>`.</span><span class="sxs-lookup"><span data-stu-id="29623-242">Configuring the time interval sends the `docker stop <time in seconds>` command to the container.</span></span>   <span data-ttu-id="29623-243">Další podrobnosti najdete v dokumentaci k příkazu [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span><span class="sxs-lookup"><span data-stu-id="29623-243">For more detail, see [docker stop](https://docs.docker.com/engine/reference/commandline/stop/).</span></span> <span data-ttu-id="29623-244">Časový interval pro čekání se zadává v části `Hosting`.</span><span class="sxs-lookup"><span data-stu-id="29623-244">The time interval to wait is specified under the `Hosting` section.</span></span> <span data-ttu-id="29623-245">Následující fragment manifestu clusteru ukazuje nastavení intervalu čekání:</span><span class="sxs-lookup"><span data-stu-id="29623-245">The following cluster manifest snippet shows how to set the wait interval:</span></span>

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
<span data-ttu-id="29623-246">Výchozí časový interval je nastavený na 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="29623-246">The default time interval is set to 10 seconds.</span></span> <span data-ttu-id="29623-247">Vzhledem k tomu, že je tato konfigurace dynamická, časový limit aktualizuje v clusteru upgrade pouze konfigurace.</span><span class="sxs-lookup"><span data-stu-id="29623-247">Since this configuration is dynamic, a config only upgrade on the cluster updates the timeout.</span></span> 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a><span data-ttu-id="29623-248">Konfigurace modulu runtime pro odebrání nepoužívaných imagí kontejneru</span><span class="sxs-lookup"><span data-stu-id="29623-248">Configure the runtime to remove unused container images</span></span>

<span data-ttu-id="29623-249">Cluster Service Fabric můžete nakonfigurovat tak, aby z uzlu odebral nepoužívané image kontejneru.</span><span class="sxs-lookup"><span data-stu-id="29623-249">You can configure the Service Fabric cluster to remove unused container images from the node.</span></span> <span data-ttu-id="29623-250">Tato konfigurace umožňuje znovu získat místo na disku v případě, že je na uzlu příliš mnoho imagí kontejneru.</span><span class="sxs-lookup"><span data-stu-id="29623-250">This configuration allows disk space to be recaptured if too many container images are present on the node.</span></span>  <span data-ttu-id="29623-251">Pokud chcete tuto funkci povolit, aktualizujte část `Hosting` v manifestu clusteru, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="29623-251">To enable this feature, update the `Hosting` section in the cluster manifest as shown in the following snippet:</span></span> 


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

<span data-ttu-id="29623-252">Image, které se nesmí odstranit, můžete zadat v rámci parametru `ContainerImagesToSkip`.</span><span class="sxs-lookup"><span data-stu-id="29623-252">For images that should not be deleted, you can specify them under the `ContainerImagesToSkip` parameter.</span></span> 



## <a name="next-steps"></a><span data-ttu-id="29623-253">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29623-253">Next steps</span></span>
* <span data-ttu-id="29623-254">Další informace o spouštění [kontejnerů v Service Fabric](service-fabric-containers-overview.md).</span><span class="sxs-lookup"><span data-stu-id="29623-254">Learn more about running [containers on Service Fabric](service-fabric-containers-overview.md).</span></span>
* <span data-ttu-id="29623-255">Přečtěte si kurz [Nasazení aplikace .NET v kontejneru](service-fabric-host-app-in-a-container.md).</span><span class="sxs-lookup"><span data-stu-id="29623-255">Read the [Deploy a .NET application in a container](service-fabric-host-app-in-a-container.md) tutorial.</span></span>
* <span data-ttu-id="29623-256">Informace o [životním cyklu aplikace](service-fabric-application-lifecycle.md) Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="29623-256">Learn about the Service Fabric [application life-cycle](service-fabric-application-lifecycle.md).</span></span>
* <span data-ttu-id="29623-257">Prohlédněte si [ukázky kódu kontejneru Service Fabric](https://github.com/Azure-Samples/service-fabric-dotnet-containers) na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="29623-257">Checkout the [Service Fabric container code samples](https://github.com/Azure-Samples/service-fabric-dotnet-containers) on GitHub.</span></span>

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
