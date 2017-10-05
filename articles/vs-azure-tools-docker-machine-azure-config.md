---
title: "Vytvořte hostitelů Docker v Azure s počítačem Docker | Microsoft Docs"
description: "Popisuje použití Docker počítač pro vytvoření hostitelů docker v Azure."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 766d327a87ed13e04166d71c3d9ae0a1e7a66d19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a><span data-ttu-id="065e1-103">Vytvoření hostitelů s Dockerem v Azure pomocí Docker-Machine</span><span class="sxs-lookup"><span data-stu-id="065e1-103">Create Docker Hosts in Azure with Docker-Machine</span></span>
<span data-ttu-id="065e1-104">Spuštění [Docker](https://www.docker.com/) kontejnery vyžaduje hostitele démona docker virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="065e1-104">Running [Docker](https://www.docker.com/) containers requires a host VM running the docker daemon.</span></span>
<span data-ttu-id="065e1-105">Toto téma popisuje postup použití [počítač docker](https://docs.docker.com/machine/) příkaz pro vytvoření nové virtuální počítače Linux nakonfigurované démona Docker, běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="065e1-105">This topic describes how to use the [docker-machine](https://docs.docker.com/machine/) command to create new Linux VMs, configured with the Docker daemon, running in Azure.</span></span> 

<span data-ttu-id="065e1-106">**Poznámka:**</span><span class="sxs-lookup"><span data-stu-id="065e1-106">**Note:**</span></span> 

* <span data-ttu-id="065e1-107">*Tento článek závisí na verzi počítač docker 0.9.0-rc2 nebo větší*</span><span class="sxs-lookup"><span data-stu-id="065e1-107">*This article depends on docker-machine version 0.9.0-rc2 or greater*</span></span>
* <span data-ttu-id="065e1-108">*Kontejnery Windows budou podporovány prostřednictvím docker počítače v blízké budoucnosti*</span><span class="sxs-lookup"><span data-stu-id="065e1-108">*Windows Containers will be supported through docker-machine in the near future*</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="065e1-109">Vytvořit virtuální počítače s počítačem Docker</span><span class="sxs-lookup"><span data-stu-id="065e1-109">Create VMs with Docker Machine</span></span>
<span data-ttu-id="065e1-110">Vytvoření docker hostitele virtuálních počítačů v Azure pomocí `docker-machine create` příkaz pomocí `azure` ovladačů.</span><span class="sxs-lookup"><span data-stu-id="065e1-110">Create docker host VMs in Azure with the `docker-machine create` command using the `azure` driver.</span></span> 

<span data-ttu-id="065e1-111">Ovladač Azure vyžaduje ID vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="065e1-111">The Azure driver requires your subscription ID.</span></span> <span data-ttu-id="065e1-112">Můžete použít [rozhraní příkazového řádku Azure](cli-install-nodejs.md) nebo [portálu Azure](https://portal.azure.com) načíst vaše předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="065e1-112">You can use the [Azure CLI](cli-install-nodejs.md) or the [Azure Portal](https://portal.azure.com) to retrieve your Azure Subscription.</span></span> 

<span data-ttu-id="065e1-113">**Pomocí portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="065e1-113">**Using the Azure Portal**</span></span>

* <span data-ttu-id="065e1-114">Vyberte **odběry** z levé navigační stránce a zkopírujte id předplatného.</span><span class="sxs-lookup"><span data-stu-id="065e1-114">Select **Subscriptions** from the left navigation page and copy the subscription id.</span></span>

<span data-ttu-id="065e1-115">**Pomocí Azure CLI**</span><span class="sxs-lookup"><span data-stu-id="065e1-115">**Using the Azure CLI**</span></span>

* <span data-ttu-id="065e1-116">Typ ```azure account list``` a zkopírujte id předplatného.</span><span class="sxs-lookup"><span data-stu-id="065e1-116">Type ```azure account list``` and copy the subscription id.</span></span>

<span data-ttu-id="065e1-117">Typ `docker-machine create --driver azure` zobrazíte možnosti a jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="065e1-117">Type `docker-machine create --driver azure` to see the options and their default values.</span></span>
<span data-ttu-id="065e1-118">Můžete také zjistit [dokumentace Azure ovladač Docker](https://docs.docker.com/machine/drivers/azure/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="065e1-118">You can also see the [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/) for more info.</span></span> 

<span data-ttu-id="065e1-119">Následující příklad závisí na [výchozí hodnoty](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), ale můžete nastavit tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="065e1-119">The following example relies upon the [default values](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), but it does optionally set these values:</span></span> 

* <span data-ttu-id="065e1-120">Azure dns pro název spojený s veřejnou IP adresu a certifikáty generované.</span><span class="sxs-lookup"><span data-stu-id="065e1-120">azure-dns for the name associated with the public IP and certificates generated.</span></span> <span data-ttu-id="065e1-121">Toto je název DNS virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="065e1-121">This is the DNS name of your virtual machine.</span></span> <span data-ttu-id="065e1-122">Virtuální počítač může potom bezpečně zastavit, verzi dynamické IP a umožňují znovu připojit po virtuální počítač spustí znovu s novou IP Adresou.</span><span class="sxs-lookup"><span data-stu-id="065e1-122">The VM can then safely stop, release the dynamic IP, and provide the ability to reconnect after the vm starts again with a new IP.</span></span> <span data-ttu-id="065e1-123">Předpona názvu musí být jedinečný pro danou oblast UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="065e1-123">The name prefix must be unique for that region  UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span></span>
* <span data-ttu-id="065e1-124">Otevřete port 80 pro virtuální počítač pro odchozí přístup k Internetu</span><span class="sxs-lookup"><span data-stu-id="065e1-124">open port 80 on the VM for outbound internet access</span></span>
* <span data-ttu-id="065e1-125">velikost virtuálního počítače, který využívají rychlejší storage úrovně premium</span><span class="sxs-lookup"><span data-stu-id="065e1-125">size of the VM to utilize faster premium storage</span></span>
* <span data-ttu-id="065e1-126">použít pro disk virtuálního počítače služby storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="065e1-126">premium storage used for the vm disk</span></span>

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a><span data-ttu-id="065e1-127">Zvolit hostitele s docker počítač docker</span><span class="sxs-lookup"><span data-stu-id="065e1-127">Choose a docker host with docker-machine</span></span>
<span data-ttu-id="065e1-128">Jakmile máte položku v docker počítače pro hostitele, můžete nastavit výchozí hostitel při spuštění docker příkazů.</span><span class="sxs-lookup"><span data-stu-id="065e1-128">Once you have an entry in docker-machine for your host, you can set the default host when running docker commands.</span></span>

## <a name="using-powershell"></a><span data-ttu-id="065e1-129">Pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="065e1-129">Using PowerShell</span></span>
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a><span data-ttu-id="065e1-130">Pomocí Bash</span><span class="sxs-lookup"><span data-stu-id="065e1-130">Using Bash</span></span>
```bash
eval $(docker-machine env MyDockerHost)
```

<span data-ttu-id="065e1-131">Teď můžete spustit příkazy docker proti zadaného hostitele</span><span class="sxs-lookup"><span data-stu-id="065e1-131">You can now run docker commands against the specified host</span></span>

```
docker ps
docker info
```

## <a name="run-a-container"></a><span data-ttu-id="065e1-132">Spusťte kontejner</span><span class="sxs-lookup"><span data-stu-id="065e1-132">Run a container</span></span>
<span data-ttu-id="065e1-133">Pomocí konfigurovaného hostitele teď můžete spustit jednoduchý webový server k ověření, zda váš hostitel byla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="065e1-133">With a host configured, you can now run a simple web server to test whether your host was configured correctly.</span></span>
<span data-ttu-id="065e1-134">Zde jsme použít bitovou kopii standardní nginx, zadejte, že musí naslouchat na portu 80, že pokud se hostitel virtuálních počítačů restartuje, kontejneru se restartování a také (`--restart=always`).</span><span class="sxs-lookup"><span data-stu-id="065e1-134">Here we use a standard nginx image, specify that it should listen on port 80, and that if the host VM restarts, the container will restart as well (`--restart=always`).</span></span> 

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="065e1-135">Výstup by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="065e1-135">The output should look something like the following:</span></span>

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a><span data-ttu-id="065e1-136">Test kontejneru</span><span class="sxs-lookup"><span data-stu-id="065e1-136">Test the container</span></span>
<span data-ttu-id="065e1-137">Zkontrolujte spuštěných kontejnerů pomocí `docker ps`:</span><span class="sxs-lookup"><span data-stu-id="065e1-137">Examine running containers using `docker ps`:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

<span data-ttu-id="065e1-138">A pokud chcete zobrazit spuštěné kontejneru, zadejte `docker-machine ip <VM name>` najít IP adresu, kterou zadat do prohlížeče:</span><span class="sxs-lookup"><span data-stu-id="065e1-138">And, to see the running container, type `docker-machine ip <VM name>` to find the IP address to enter in the browser:</span></span>

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Spuštěné ngnix kontejneru](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a><span data-ttu-id="065e1-140">Souhrn</span><span class="sxs-lookup"><span data-stu-id="065e1-140">Summary</span></span>
<span data-ttu-id="065e1-141">Pomocí docker počítače můžete snadno zřídit hostitelů docker v Azure pro vaše jednotlivé docker hostitele ověření.</span><span class="sxs-lookup"><span data-stu-id="065e1-141">With docker-machine, you can easily provision docker hosts in Azure for your individual docker host validations.</span></span>
<span data-ttu-id="065e1-142">Pro produkční prostředí hostování kontejnerů, najdete v článku [Azure Container Service](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="065e1-142">For production hosting of containers, see the [Azure Container Service](http://aka.ms/AzureContainerService)</span></span>

<span data-ttu-id="065e1-143">K vývoji aplikací .NET Core pomocí sady Visual Studio, najdete v části [Docker Tools pro Visual Studio](http://aka.ms/DockerToolsForVS)</span><span class="sxs-lookup"><span data-stu-id="065e1-143">To develop .NET Core Applications with Visual Studio, see [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span></span>

