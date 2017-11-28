---
title: "aaaUse Docker Compose na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Jak hello toouse Docker a vytvářené na virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 02ab8cf9-318d-4a28-9d0c-4a31dccc2a84
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: cf7254ad4813ccdc641fcacbb06ed1514a93cee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-docker-and-compose-toodefine-and-run-a-multi-container-application-in-azure"></a><span data-ttu-id="e4750-103">Získat začít s Docker a vytvářené toodefine a spuštění aplikace s více kontejnerů v Azure</span><span class="sxs-lookup"><span data-stu-id="e4750-103">Get started with Docker and Compose toodefine and run a multi-container application in Azure</span></span>
<span data-ttu-id="e4750-104">S [vytvářené](http://github.com/docker/compose), používat toodefine jednoduchý text souboru aplikace, který se skládá z několika kontejnerů Docker.</span><span class="sxs-lookup"><span data-stu-id="e4750-104">With [Compose](http://github.com/docker/compose), you use a simple text file toodefine an application consisting of multiple Docker containers.</span></span> <span data-ttu-id="e4750-105">Pak začne pracovat aplikace v jednom příkaz, který nemá všechno, co toodeploy prostředí definované.</span><span class="sxs-lookup"><span data-stu-id="e4750-105">You then spin up your application in a single command that does everything toodeploy your defined environment.</span></span> <span data-ttu-id="e4750-106">Jako příklad, tento článek ukazuje, jak tooquickly nastavit blogu WordPress s back-end MariaDB SQL database na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="e4750-106">As an example, this article shows you how tooquickly set up a WordPress blog with a backend MariaDB SQL database on an Ubuntu VM.</span></span> <span data-ttu-id="e4750-107">Můžete taky vytvářené tooset až složitějších aplikací.</span><span class="sxs-lookup"><span data-stu-id="e4750-107">You can also use Compose tooset up more complex applications.</span></span>


## <a name="set-up-a-linux-vm-as-a-docker-host"></a><span data-ttu-id="e4750-108">Nastavení virtuálního počítače s Linuxem jako hostitele Docker</span><span class="sxs-lookup"><span data-stu-id="e4750-108">Set up a Linux VM as a Docker host</span></span>
<span data-ttu-id="e4750-109">Můžete pomocí různých postupů Azure a dostupných imagí nebo šablony Resource Manageru v Azure Marketplace toocreate hello virtuálního počítače s Linuxem a nastavit jako Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="e4750-109">You can use various Azure procedures and available images or Resource Manager templates in hello Azure Marketplace toocreate a Linux VM and set it up as a Docker host.</span></span> <span data-ttu-id="e4750-110">Například v tématu [pomocí rozšíření virtuálního počítače Docker toodeploy hello prostředí](dockerextension.md) tooquickly vytvoření virtuálního počítače s Ubuntu s hello rozšíření virtuálního počítače Azure Docker pomocí [šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="e4750-110">For example, see [Using hello Docker VM Extension toodeploy your environment](dockerextension.md) tooquickly create an Ubuntu VM with hello Azure Docker VM extension by using a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="e4750-111">Pokud používáte rozšíření virtuálního počítače Docker hello, virtuální počítač je automaticky nastavený jako hostitel Docker a Compose je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="e4750-111">When you use hello Docker VM extension, your VM is automatically set up as a Docker host and Compose is already installed.</span></span>


### <a name="create-docker-host-with-azure-cli-20"></a><span data-ttu-id="e4750-112">Vytvořit hostitele Docker s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e4750-112">Create Docker host with Azure CLI 2.0</span></span>
<span data-ttu-id="e4750-113">Instalace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e4750-113">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="e4750-114">Nejprve vytvořte skupinu prostředků pro vaše prostředí Docker s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e4750-114">First, create a resource group for your Docker environment with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="e4750-115">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění:</span><span class="sxs-lookup"><span data-stu-id="e4750-115">hello following example creates a resource group named *myResourceGroup* in hello *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="e4750-116">V dalším kroku nasaďte virtuální počítač s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) obsahující rozšíření virtuálního počítače Azure Docker hello z [této šablony Azure Resource Manageru na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="e4750-116">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes hello Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="e4750-117">Zadat vlastní hodnoty pro *newStorageAccountName*, *adminUsername*, *adminPassword*, a *dnsNameForPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="e4750-117">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="e4750-118">Jak dlouho trvá několik minut, než toofinish nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="e4750-118">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="e4750-119">Po dokončení nasazení hello [přesunout krok toonext](#verify-that-compose-is-installed) tooSSH tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e4750-119">Once hello deployment is finished, [move toonext step](#verify-that-compose-is-installed) tooSSH tooyour VM.</span></span> 

<span data-ttu-id="e4750-120">Můžete také řádku toohello tooinstead návratový řízení a umožňují hello nasazení pokračovat na pozadí hello přidat hello `--no-wait` příznak toohello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="e4750-120">Optionally, tooinstead return control toohello prompt and let hello deployment continue in hello background, add hello `--no-wait` flag toohello preceding command.</span></span> <span data-ttu-id="e4750-121">Tento proces vám umožní tooperform jinou práci v hello rozhraní příkazového řádku při hello nasazení bude stále několik minut.</span><span class="sxs-lookup"><span data-stu-id="e4750-121">This process allows you tooperform other work in hello CLI while hello deployment continues for a few minutes.</span></span> <span data-ttu-id="e4750-122">Potom můžete zobrazit podrobnosti o stavu hostitele hello Docker s [az virtuálních počítačů zobrazit](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="e4750-122">You can then view details about hello Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="e4750-123">Hello následující příklad ověří hello stav hello virtuálního počítače s názvem *myDockerVM* (hello výchozí název ze šablony hello – neměnit tento název) ve skupině prostředků hello s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="e4750-123">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="e4750-124">Pokud tento příkaz vrátí *úspěšné*, hello nasazení je dokončené, a můžete SSH toohello virtuálních počítačů v hello následující krok.</span><span class="sxs-lookup"><span data-stu-id="e4750-124">When this command returns *Succeeded*, hello deployment has finished and you can SSH toohello VM in hello following step.</span></span>


## <a name="verify-that-compose-is-installed"></a><span data-ttu-id="e4750-125">Ověřte, zda je nainstalován Compose</span><span class="sxs-lookup"><span data-stu-id="e4750-125">Verify that Compose is installed</span></span>
<span data-ttu-id="e4750-126">Po dokončení nasazení hello SSH tooyour nové Docker hostitele pomocí hello název DNS, které jste zadali při nasazení.</span><span class="sxs-lookup"><span data-stu-id="e4750-126">Once hello deployment is finished, SSH tooyour new Docker host using hello DNS name you provided during deployment.</span></span> <span data-ttu-id="e4750-127">Můžete použít `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview podrobnosti virtuálního počítače, včetně názvu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="e4750-127">You can use  `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview details of your VM, including hello DNS name.</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="e4750-128">toocheck, která tvoří nainstalovaný na hello virtuálního počítače, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e4750-128">toocheck that Compose is installed on hello VM, run hello following command:</span></span>

```bash
docker-compose --version
```

<span data-ttu-id="e4750-129">Zobrazí výstup podobný příliš*docker compose 1.6.2, sestavení 4d 72027*.</span><span class="sxs-lookup"><span data-stu-id="e4750-129">You see output similar too*docker-compose 1.6.2, build 4d72027*.</span></span>

> [!TIP]
> <span data-ttu-id="e4750-130">Pokud jste použili jinou metodu toocreate hostitelů Docker a potřebovat tooinstall vytvořit sami, přečtěte si téma hello [tvoří dokumentaci](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="e4750-130">If you used another method toocreate a Docker host and need tooinstall Compose yourself, see hello [Compose documentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span></span>


## <a name="create-a-docker-composeyml-configuration-file"></a><span data-ttu-id="e4750-131">Vytvořte konfigurační soubor docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="e4750-131">Create a docker-compose.yml configuration file</span></span>
<span data-ttu-id="e4750-132">Dále vytvoříte `docker-compose.yml` souboru, který je právě text konfigurační soubor, toodefine hello Docker kontejnery toorun na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e4750-132">Next you create a `docker-compose.yml` file, which is just a text configuration file, toodefine hello Docker containers toorun on hello VM.</span></span> <span data-ttu-id="e4750-133">soubor Hello určuje toorun hello bitové kopie na každý kontejner (nebo to může být sestavení z soubor Docker), nezbytné proměnné prostředí a závislosti, porty a hello propojení mezi kontejnery.</span><span class="sxs-lookup"><span data-stu-id="e4750-133">hello file specifies hello image toorun on each container (or it could be a build from a Dockerfile), necessary environment variables and dependencies, ports, and hello links between containers.</span></span> <span data-ttu-id="e4750-134">Informace o syntaxi souboru yml v [vytvořit odkaz na soubor](https://docs.docker.com/compose/compose-file/).</span><span class="sxs-lookup"><span data-stu-id="e4750-134">For details on yml file syntax, see [Compose file reference](https://docs.docker.com/compose/compose-file/).</span></span>

<span data-ttu-id="e4750-135">Vytvoření hello *docker-compose.yml* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e4750-135">Create hello *docker-compose.yml* file as follows:</span></span>

```bash
touch docker-compose.yml
```

<span data-ttu-id="e4750-136">Použijte tooadd vašem oblíbeném textovém editoru soubor toohello některé data.</span><span class="sxs-lookup"><span data-stu-id="e4750-136">Use your favorite text editor tooadd some data toohello file.</span></span> <span data-ttu-id="e4750-137">Hello následující příklad používá hello *vi* editoru:</span><span class="sxs-lookup"><span data-stu-id="e4750-137">hello following example uses hello *vi* editor:</span></span>

```bash
vi docker-compose.yml
```

<span data-ttu-id="e4750-138">Vložte hello následující ukázka do textového souboru.</span><span class="sxs-lookup"><span data-stu-id="e4750-138">Paste hello following example into your text file.</span></span> <span data-ttu-id="e4750-139">Tato konfigurace používá bitové kopie z hello [DockerHub registru](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello s otevřeným zdrojem blogu a systém správy obsahu) a propojené back-end MariaDB SQL databáze.</span><span class="sxs-lookup"><span data-stu-id="e4750-139">This configuration uses images from hello [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello open source blogging and content management system) and a linked backend MariaDB SQL database.</span></span> <span data-ttu-id="e4750-140">Zadejte vlastní *MYSQL_ROOT_PASSWORD* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e4750-140">Enter your own *MYSQL_ROOT_PASSWORD* as follows:</span></span>

```sh
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="start-hello-containers-with-compose"></a><span data-ttu-id="e4750-141">Začínat hello kontejnery vytvářené</span><span class="sxs-lookup"><span data-stu-id="e4750-141">Start hello containers with Compose</span></span>
<span data-ttu-id="e4750-142">V hello stejný adresář jako vaše *docker-compose.yml* souboru, spusťte následující příkaz hello (v závislosti na vašem prostředí, může být nutné toorun `docker-compose` pomocí `sudo`):</span><span class="sxs-lookup"><span data-stu-id="e4750-142">In hello same directory as your *docker-compose.yml* file, run hello following command (depending on your environment, you might need toorun `docker-compose` using `sudo`):</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="e4750-143">Tento příkaz spustí kontejnery Docker hello zadaný v *docker-compose.yml*.</span><span class="sxs-lookup"><span data-stu-id="e4750-143">This command starts hello Docker containers specified in *docker-compose.yml*.</span></span> <span data-ttu-id="e4750-144">Bude trvat minutu nebo dvě pro tento krok toocomplete.</span><span class="sxs-lookup"><span data-stu-id="e4750-144">It takes a minute or two for this step toocomplete.</span></span> <span data-ttu-id="e4750-145">Zobrazí výstup podobný toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="e4750-145">You see output similar toohello following example:</span></span>

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> <span data-ttu-id="e4750-146">Se, zda text hello toouse **-d** možnost spuštění tak, aby hello kontejnery spouštět nepřetržitě hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="e4750-146">Be sure toouse hello **-d** option on start-up so that hello containers run in hello background continuously.</span></span>


<span data-ttu-id="e4750-147">tooverify, které jsou hello kontejnery, typ `docker-compose ps`.</span><span class="sxs-lookup"><span data-stu-id="e4750-147">tooverify that hello containers are up, type `docker-compose ps`.</span></span> <span data-ttu-id="e4750-148">Měli byste vidět něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="e4750-148">You should see something like:</span></span>

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

<span data-ttu-id="e4750-149">Teď se můžete připojit tooWordPress přímo na hello virtuálních počítačů na portu 80.</span><span class="sxs-lookup"><span data-stu-id="e4750-149">You can now connect tooWordPress directly on hello VM on port 80.</span></span> <span data-ttu-id="e4750-150">Otevřete webový prohlížeč a zadejte název DNS hello vašeho virtuálního počítače (například `http://mypublicdns.westus.cloudapp.azure.com`).</span><span class="sxs-lookup"><span data-stu-id="e4750-150">Open a web browser and enter hello DNS name of your VM (such as `http://mypublicdns.westus.cloudapp.azure.com`).</span></span> <span data-ttu-id="e4750-151">Teď byste měli vidět hello WordPress úvodní obrazovka, kde může dokončení instalace hello a začít pracovat s aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="e4750-151">You should now see hello WordPress start screen, where you can complete hello installation and get started with hello application.</span></span>

![WordPress úvodní obrazovka][wordpress_start]

## <a name="next-steps"></a><span data-ttu-id="e4750-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4750-153">Next steps</span></span>
* <span data-ttu-id="e4750-154">Přejděte toohello [uživatelská příručka k rozšíření virtuálního počítače Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) další možnosti tooconfigure Docker a vytvořit v Docker virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4750-154">Go toohello [Docker VM extension user guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md) for more options tooconfigure Docker and Compose in your Docker VM.</span></span> <span data-ttu-id="e4750-155">Jednou z možností je například tooput hello vytvářené yml souboru (převedený tooJSON) přímo v konfiguraci hello hello rozšíření Docker virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e4750-155">For example, one option is tooput hello Compose yml file (converted tooJSON) directly in hello configuration of hello Docker VM extension.</span></span>
* <span data-ttu-id="e4750-156">Podívejte se na hello [tvoří reference k příkazovému řádku](http://docs.docker.com/compose/reference/) a [uživatelská příručka](http://docs.docker.com/compose/) Další příklady vytváření a nasazování aplikací pro službu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="e4750-156">Check out hello [Compose command-line reference](http://docs.docker.com/compose/reference/) and [user guide](http://docs.docker.com/compose/) for more examples of building and deploying multi-container apps.</span></span>
* <span data-ttu-id="e4750-157">Pomocí šablony Azure Resource Manager, buď vaše vlastní nebo jeden podílí z hello [komunity](https://azure.microsoft.com/documentation/templates/), toodeploy virtuální počítač Azure s Docker a aplikace nastavit nové.</span><span class="sxs-lookup"><span data-stu-id="e4750-157">Use an Azure Resource Manager template, either your own or one contributed from hello [community](https://azure.microsoft.com/documentation/templates/), toodeploy an Azure VM with Docker and an application set up with Compose.</span></span> <span data-ttu-id="e4750-158">Například hello [nasazení blog WordPress pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) šablona používá Docker a tooquickly vytvářené nasazení WordPress s back-end databáze MySQL na virtuálního počítače s Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="e4750-158">For example, hello [Deploy a WordPress blog with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) template uses Docker and Compose tooquickly deploy WordPress with a MySQL backend on an Ubuntu VM.</span></span>
* <span data-ttu-id="e4750-159">Zkuste integraci Docker Compose do clusteru s podporou Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="e4750-159">Try integrating Docker Compose with a Docker Swarm cluster.</span></span> <span data-ttu-id="e4750-160">V tématu [pomocí tvoří s Swarm](https://docs.docker.com/compose/swarm/) pro scénáře.</span><span class="sxs-lookup"><span data-stu-id="e4750-160">See [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) for scenarios.</span></span>

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
