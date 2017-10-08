---
title: "aaaCreate Docker hostuje v Azure pomocí Docker počítače | Microsoft Docs"
description: "Popisuje využívání hostitelů docker toocreate Docker počítače v Azure."
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
ms.openlocfilehash: fbf67e8189bbf33f874c4a9b619a931f28ccee12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a><span data-ttu-id="40f11-103">Vytvoření hostitelů s Dockerem v Azure pomocí Docker-Machine</span><span class="sxs-lookup"><span data-stu-id="40f11-103">Create Docker Hosts in Azure with Docker-Machine</span></span>
<span data-ttu-id="40f11-104">Spuštění [Docker](https://www.docker.com/) kontejnery vyžaduje hostitele virtuálních počítačů spuštěných hello docker démon.</span><span class="sxs-lookup"><span data-stu-id="40f11-104">Running [Docker](https://www.docker.com/) containers requires a host VM running hello docker daemon.</span></span>
<span data-ttu-id="40f11-105">Toto téma popisuje, jak toouse hello [počítač docker](https://docs.docker.com/machine/) příkaz toocreate nové virtuální počítače s Linuxem, nakonfigurovaný s hello Docker démona, běží v Azure.</span><span class="sxs-lookup"><span data-stu-id="40f11-105">This topic describes how toouse hello [docker-machine](https://docs.docker.com/machine/) command toocreate new Linux VMs, configured with hello Docker daemon, running in Azure.</span></span> 

<span data-ttu-id="40f11-106">**Poznámka:**</span><span class="sxs-lookup"><span data-stu-id="40f11-106">**Note:**</span></span> 

* <span data-ttu-id="40f11-107">*Tento článek závisí na verzi počítač docker 0.9.0-rc2 nebo větší*</span><span class="sxs-lookup"><span data-stu-id="40f11-107">*This article depends on docker-machine version 0.9.0-rc2 or greater*</span></span>
* <span data-ttu-id="40f11-108">*Kontejnery Windows budou podporovány prostřednictvím docker počítače v blízké budoucnosti hello*</span><span class="sxs-lookup"><span data-stu-id="40f11-108">*Windows Containers will be supported through docker-machine in hello near future*</span></span>

## <a name="create-vms-with-docker-machine"></a><span data-ttu-id="40f11-109">Vytvořit virtuální počítače s počítačem Docker</span><span class="sxs-lookup"><span data-stu-id="40f11-109">Create VMs with Docker Machine</span></span>
<span data-ttu-id="40f11-110">Vytvořte docker hostitele virtuálních počítačů v Azure s hello `docker-machine create` příkaz pomocí hello `azure` ovladačů.</span><span class="sxs-lookup"><span data-stu-id="40f11-110">Create docker host VMs in Azure with hello `docker-machine create` command using hello `azure` driver.</span></span> 

<span data-ttu-id="40f11-111">Hello ovladač Azure vyžaduje ID vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="40f11-111">hello Azure driver requires your subscription ID.</span></span> <span data-ttu-id="40f11-112">Můžete použít hello [rozhraní příkazového řádku Azure](cli-install-nodejs.md) nebo hello [portálu Azure](https://portal.azure.com) tooretrieve předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="40f11-112">You can use hello [Azure CLI](cli-install-nodejs.md) or hello [Azure Portal](https://portal.azure.com) tooretrieve your Azure Subscription.</span></span> 

<span data-ttu-id="40f11-113">**Pomocí hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="40f11-113">**Using hello Azure Portal**</span></span>

* <span data-ttu-id="40f11-114">Vyberte **odběry** z hello levé navigační stránku a zkopírujte hello id předplatného.</span><span class="sxs-lookup"><span data-stu-id="40f11-114">Select **Subscriptions** from hello left navigation page and copy hello subscription id.</span></span>

<span data-ttu-id="40f11-115">**Pomocí hello rozhraní příkazového řádku Azure**</span><span class="sxs-lookup"><span data-stu-id="40f11-115">**Using hello Azure CLI**</span></span>

* <span data-ttu-id="40f11-116">Typ ```azure account list``` a id předplatného hello kopírování.</span><span class="sxs-lookup"><span data-stu-id="40f11-116">Type ```azure account list``` and copy hello subscription id.</span></span>

<span data-ttu-id="40f11-117">Typ `docker-machine create --driver azure` toosee hello možnosti a jejich výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="40f11-117">Type `docker-machine create --driver azure` toosee hello options and their default values.</span></span>
<span data-ttu-id="40f11-118">Můžete také zjistit hello [dokumentace Azure ovladač Docker](https://docs.docker.com/machine/drivers/azure/) Další informace.</span><span class="sxs-lookup"><span data-stu-id="40f11-118">You can also see hello [Docker Azure Driver documentation](https://docs.docker.com/machine/drivers/azure/) for more info.</span></span> 

<span data-ttu-id="40f11-119">Hello následující příklad závisí na hello [výchozí hodnoty](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), ale můžete nastavit tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="40f11-119">hello following example relies upon hello [default values](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22), but it does optionally set these values:</span></span> 

* <span data-ttu-id="40f11-120">Azure dns pro název hello spojený s veřejnou IP adresu hello a certifikáty generované.</span><span class="sxs-lookup"><span data-stu-id="40f11-120">azure-dns for hello name associated with hello public IP and certificates generated.</span></span> <span data-ttu-id="40f11-121">Toto je název DNS hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="40f11-121">This is hello DNS name of your virtual machine.</span></span> <span data-ttu-id="40f11-122">Hello virtuálních počítačů můžete pak bezpečně zastavit, vydání hello dynamické IP a poskytnout možnost tooreconnect hello po hello virtuální počítač spustí znovu s novou IP Adresou.</span><span class="sxs-lookup"><span data-stu-id="40f11-122">hello VM can then safely stop, release hello dynamic IP, and provide hello ability tooreconnect after hello vm starts again with a new IP.</span></span> <span data-ttu-id="40f11-123">Předpona názvu Hello musí být jedinečný pro danou oblast UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="40f11-123">hello name prefix must be unique for that region  UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com.</span></span>
* <span data-ttu-id="40f11-124">Otevřete port 80 na hello virtuálních počítačů pro odchozí přístup k Internetu</span><span class="sxs-lookup"><span data-stu-id="40f11-124">open port 80 on hello VM for outbound internet access</span></span>
* <span data-ttu-id="40f11-125">velikost hello úložiště premium rychlejší tooutilize virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="40f11-125">size of hello VM tooutilize faster premium storage</span></span>
* <span data-ttu-id="40f11-126">použít pro disk hello virtuálních počítačů služby storage úrovně Premium</span><span class="sxs-lookup"><span data-stu-id="40f11-126">premium storage used for hello vm disk</span></span>

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a><span data-ttu-id="40f11-127">Zvolit hostitele s docker počítač docker</span><span class="sxs-lookup"><span data-stu-id="40f11-127">Choose a docker host with docker-machine</span></span>
<span data-ttu-id="40f11-128">Až budete mít záznam v docker počítače pro hostitele, můžete nastavit výchozí hostitel hello při spouštění příkazů docker.</span><span class="sxs-lookup"><span data-stu-id="40f11-128">Once you have an entry in docker-machine for your host, you can set hello default host when running docker commands.</span></span>

## <a name="using-powershell"></a><span data-ttu-id="40f11-129">Pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="40f11-129">Using PowerShell</span></span>
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a><span data-ttu-id="40f11-130">Pomocí Bash</span><span class="sxs-lookup"><span data-stu-id="40f11-130">Using Bash</span></span>
```bash
eval $(docker-machine env MyDockerHost)
```

<span data-ttu-id="40f11-131">Teď můžete spustit příkazy docker proti hello zadaného hostitele</span><span class="sxs-lookup"><span data-stu-id="40f11-131">You can now run docker commands against hello specified host</span></span>

```
docker ps
docker info
```

## <a name="run-a-container"></a><span data-ttu-id="40f11-132">Spusťte kontejner</span><span class="sxs-lookup"><span data-stu-id="40f11-132">Run a container</span></span>
<span data-ttu-id="40f11-133">Pomocí konfigurovaného hostitele teď můžete spustit jednoduchého webového serveru tootest zda váš hostitel byla nakonfigurována správně.</span><span class="sxs-lookup"><span data-stu-id="40f11-133">With a host configured, you can now run a simple web server tootest whether your host was configured correctly.</span></span>
<span data-ttu-id="40f11-134">Zde jsme použít bitovou kopii standardní nginx, zadejte, že musí naslouchat na portu 80, že pokud se restartuje hello hostitele virtuálních počítačů, hello kontejneru se restartování a také (`--restart=always`).</span><span class="sxs-lookup"><span data-stu-id="40f11-134">Here we use a standard nginx image, specify that it should listen on port 80, and that if hello host VM restarts, hello container will restart as well (`--restart=always`).</span></span> 

```bash
docker run -d -p 80:80 --restart=always nginx
```

<span data-ttu-id="40f11-135">výstup Hello by měl vypadat podobně jako následující hello:</span><span class="sxs-lookup"><span data-stu-id="40f11-135">hello output should look something like hello following:</span></span>

```
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-hello-container"></a><span data-ttu-id="40f11-136">Kontejner testů hello</span><span class="sxs-lookup"><span data-stu-id="40f11-136">Test hello container</span></span>
<span data-ttu-id="40f11-137">Zkontrolujte spuštěných kontejnerů pomocí `docker ps`:</span><span class="sxs-lookup"><span data-stu-id="40f11-137">Examine running containers using `docker ps`:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

<span data-ttu-id="40f11-138">A toosee hello systémem kontejneru, typu `docker-machine ip <VM name>` toofind hello IP adresu tooenter v prohlížeči hello:</span><span class="sxs-lookup"><span data-stu-id="40f11-138">And, toosee hello running container, type `docker-machine ip <VM name>` toofind hello IP address tooenter in hello browser:</span></span>

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Spuštěné ngnix kontejneru](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a><span data-ttu-id="40f11-140">Souhrn</span><span class="sxs-lookup"><span data-stu-id="40f11-140">Summary</span></span>
<span data-ttu-id="40f11-141">Pomocí docker počítače můžete snadno zřídit hostitelů docker v Azure pro vaše jednotlivé docker hostitele ověření.</span><span class="sxs-lookup"><span data-stu-id="40f11-141">With docker-machine, you can easily provision docker hosts in Azure for your individual docker host validations.</span></span>
<span data-ttu-id="40f11-142">Produkční hostování kontejnerů, najdete v části hello [Azure Container Service](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="40f11-142">For production hosting of containers, see hello [Azure Container Service](http://aka.ms/AzureContainerService)</span></span>

<span data-ttu-id="40f11-143">toodevelop aplikace .NET Core pomocí sady Visual Studio, najdete v části [Docker Tools pro Visual Studio](http://aka.ms/DockerToolsForVS)</span><span class="sxs-lookup"><span data-stu-id="40f11-143">toodevelop .NET Core Applications with Visual Studio, see [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)</span></span>

