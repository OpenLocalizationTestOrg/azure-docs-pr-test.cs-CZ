---
title: "Pomocí Docker Compose na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Jak používat Docker a vytvářené na virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 541722cb02dd991228726e62a2304b49cdd806f2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-in-azure"></a><span data-ttu-id="ebedb-103">Začínáme s Docker a vytvářené definovat a spuštění aplikace s více kontejnerů v Azure</span><span class="sxs-lookup"><span data-stu-id="ebedb-103">Get started with Docker and Compose to define and run a multi-container application in Azure</span></span>
<span data-ttu-id="ebedb-104">S [vytvářené](http://github.com/docker/compose), používáte k definování aplikace, který se skládá z několika kontejnerů Docker jednoduchý textový soubor.</span><span class="sxs-lookup"><span data-stu-id="ebedb-104">With [Compose](http://github.com/docker/compose), you use a simple text file to define an application consisting of multiple Docker containers.</span></span> <span data-ttu-id="ebedb-105">Pak začne pracovat aplikace v jednom příkaz, který nemá všechno k nasazení prostředí definované.</span><span class="sxs-lookup"><span data-stu-id="ebedb-105">You then spin up your application in a single command that does everything to deploy your defined environment.</span></span> <span data-ttu-id="ebedb-106">Jako příklad Tento článek ukazuje, jak rychle nastavit blog WordPress pomocí MariaDB SQL database na virtuálního počítače s Ubuntu back-end.</span><span class="sxs-lookup"><span data-stu-id="ebedb-106">As an example, this article shows you how to quickly set up a WordPress blog with a backend MariaDB SQL database on an Ubuntu VM.</span></span> <span data-ttu-id="ebedb-107">Můžete taky vytvořit složitější aplikace můžete nastavit.</span><span class="sxs-lookup"><span data-stu-id="ebedb-107">You can also use Compose to set up more complex applications.</span></span>


## <a name="set-up-a-linux-vm-as-a-docker-host"></a><span data-ttu-id="ebedb-108">Nastavení virtuálního počítače s Linuxem jako hostitele Docker</span><span class="sxs-lookup"><span data-stu-id="ebedb-108">Set up a Linux VM as a Docker host</span></span>
<span data-ttu-id="ebedb-109">Vytvoření virtuálního počítače s Linuxem a nastavit jako hostitele Docker, můžete použít různé postupy Azure a dostupných imagí nebo šablony Resource Manageru v Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ebedb-109">You can use various Azure procedures and available images or Resource Manager templates in the Azure Marketplace to create a Linux VM and set it up as a Docker host.</span></span> <span data-ttu-id="ebedb-110">Například v tématu [pomocí Docker rozšíření virtuálního počítače k nasazení prostředí](dockerextension.md) k rychlému vytvoření virtuálního počítače s Ubuntu pomocí rozšíření virtuálního počítače Azure Docker pomocí [šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="ebedb-110">For example, see [Using the Docker VM Extension to deploy your environment](dockerextension.md) to quickly create an Ubuntu VM with the Azure Docker VM extension by using a [quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="ebedb-111">Pokud používáte rozšíření virtuálního počítače Docker, virtuální počítač je automaticky nastavený jako hostitel Docker a Compose je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="ebedb-111">When you use the Docker VM extension, your VM is automatically set up as a Docker host and Compose is already installed.</span></span>


### <a name="create-docker-host-with-azure-cli-20"></a><span data-ttu-id="ebedb-112">Vytvořit hostitele Docker s Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ebedb-112">Create Docker host with Azure CLI 2.0</span></span>
<span data-ttu-id="ebedb-113">Nainstalujte si nejnovější verzi [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se k Azure účet pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ebedb-113">Install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="ebedb-114">Nejprve vytvořte skupinu prostředků pro vaše prostředí Docker s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="ebedb-114">First, create a resource group for your Docker environment with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="ebedb-115">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *westus* umístění:</span><span class="sxs-lookup"><span data-stu-id="ebedb-115">The following example creates a resource group named *myResourceGroup* in the *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="ebedb-116">V dalším kroku nasaďte virtuální počítač s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) obsahující rozšíření virtuálního počítače Azure Docker z [této šablony Azure Resource Manageru na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="ebedb-116">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes the Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="ebedb-117">Zadat vlastní hodnoty pro *newStorageAccountName*, *adminUsername*, *adminPassword*, a *dnsNameForPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="ebedb-117">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="ebedb-118">Jak dlouho trvá několik minut na dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="ebedb-118">It takes a few minutes for the deployment to finish.</span></span> <span data-ttu-id="ebedb-119">Po dokončení nasazení [přesunout k dalšímu kroku](#verify-that-compose-is-installed) k SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="ebedb-119">Once the deployment is finished, [move to next step](#verify-that-compose-is-installed) to SSH to your VM.</span></span> 

<span data-ttu-id="ebedb-120">Volitelně můžete místo toho vrátí ovládacího prvku do příkazového řádku a umožní nasazení pokračovat na pozadí, přidejte `--no-wait` příznak, který předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="ebedb-120">Optionally, to instead return control to the prompt and let the deployment continue in the background, add the `--no-wait` flag to the preceding command.</span></span> <span data-ttu-id="ebedb-121">Tento postup můžete provádět jinou práci v rozhraní příkazového řádku při nasazení bude stále několik minut.</span><span class="sxs-lookup"><span data-stu-id="ebedb-121">This process allows you to perform other work in the CLI while the deployment continues for a few minutes.</span></span> <span data-ttu-id="ebedb-122">Potom můžete zobrazit podrobnosti o stavu hostitele Docker s [az virtuálních počítačů zobrazit](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="ebedb-122">You can then view details about the Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="ebedb-123">Následující příklad ověří stav virtuálního počítače s názvem *myDockerVM* (výchozí název ze šablony - neměnit tento název) ve skupině prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="ebedb-123">The following example checks the status of the VM named *myDockerVM* (the default name from the template - don't change this name) in the resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="ebedb-124">Pokud tento příkaz vrátí *úspěšné*, nasazení úspěšně proběhlo, a můžete SSH pro virtuální počítač v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="ebedb-124">When this command returns *Succeeded*, the deployment has finished and you can SSH to the VM in the following step.</span></span>


## <a name="verify-that-compose-is-installed"></a><span data-ttu-id="ebedb-125">Ověřte, zda je nainstalován Compose</span><span class="sxs-lookup"><span data-stu-id="ebedb-125">Verify that Compose is installed</span></span>
<span data-ttu-id="ebedb-126">Po dokončení nasazení SSH do vaší nové Docker hostitele pomocí DNS název jste zadali během nasazení.</span><span class="sxs-lookup"><span data-stu-id="ebedb-126">Once the deployment is finished, SSH to your new Docker host using the DNS name you provided during deployment.</span></span> <span data-ttu-id="ebedb-127">Můžete použít `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` zobrazíte podrobnosti virtuálního počítače, včetně názvu DNS.</span><span class="sxs-lookup"><span data-stu-id="ebedb-127">You can use  `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` to view details of your VM, including the DNS name.</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="ebedb-128">Pokud chcete zkontrolovat, že Compose je nainstalovaný na Virtuálním počítači, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ebedb-128">To check that Compose is installed on the VM, run the following command:</span></span>

```bash
docker-compose --version
```

<span data-ttu-id="ebedb-129">Zobrazí výstup podobný *docker compose 1.6.2, sestavení 4d 72027*.</span><span class="sxs-lookup"><span data-stu-id="ebedb-129">You see output similar to *docker-compose 1.6.2, build 4d72027*.</span></span>

> [!TIP]
> <span data-ttu-id="ebedb-130">Pokud jste použili jinou metodu a vytvořit hostitele Docker muset nainstalovat vytvořit sami, najdete v článku [tvoří dokumentaci](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="ebedb-130">If you used another method to create a Docker host and need to install Compose yourself, see the [Compose documentation](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).</span></span>


## <a name="create-a-docker-composeyml-configuration-file"></a><span data-ttu-id="ebedb-131">Vytvořte konfigurační soubor docker-compose.yml</span><span class="sxs-lookup"><span data-stu-id="ebedb-131">Create a docker-compose.yml configuration file</span></span>
<span data-ttu-id="ebedb-132">Dále vytvoříte `docker-compose.yml` souboru, který je právě text konfiguračního souboru k definování kontejnery Docker ke spuštění ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="ebedb-132">Next you create a `docker-compose.yml` file, which is just a text configuration file, to define the Docker containers to run on the VM.</span></span> <span data-ttu-id="ebedb-133">Určuje soubor bitové kopie ke spuštění na každý kontejner (nebo to může být sestavení z soubor Docker), nezbytné proměnné prostředí a závislosti, porty a odkazů mezi kontejnery.</span><span class="sxs-lookup"><span data-stu-id="ebedb-133">The file specifies the image to run on each container (or it could be a build from a Dockerfile), necessary environment variables and dependencies, ports, and the links between containers.</span></span> <span data-ttu-id="ebedb-134">Informace o syntaxi souboru yml v [vytvořit odkaz na soubor](https://docs.docker.com/compose/compose-file/).</span><span class="sxs-lookup"><span data-stu-id="ebedb-134">For details on yml file syntax, see [Compose file reference](https://docs.docker.com/compose/compose-file/).</span></span>

<span data-ttu-id="ebedb-135">Vytvořte *docker-compose.yml* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ebedb-135">Create the *docker-compose.yml* file as follows:</span></span>

```bash
touch docker-compose.yml
```

<span data-ttu-id="ebedb-136">Pomocí svém oblíbeném textovém editoru přidejte některá data do souboru.</span><span class="sxs-lookup"><span data-stu-id="ebedb-136">Use your favorite text editor to add some data to the file.</span></span> <span data-ttu-id="ebedb-137">Následující příklad používá *vi* editoru:</span><span class="sxs-lookup"><span data-stu-id="ebedb-137">The following example uses the *vi* editor:</span></span>

```bash
vi docker-compose.yml
```

<span data-ttu-id="ebedb-138">Následující příklad vložte do textového souboru.</span><span class="sxs-lookup"><span data-stu-id="ebedb-138">Paste the following example into your text file.</span></span> <span data-ttu-id="ebedb-139">Tato konfigurace používá bitové kopie z [DockerHub registru](https://registry.hub.docker.com/_/wordpress/) instalace WordPress (s otevřeným zdrojem blogu a systém správy obsahu) a propojené back-end MariaDB SQL database.</span><span class="sxs-lookup"><span data-stu-id="ebedb-139">This configuration uses images from the [DockerHub Registry](https://registry.hub.docker.com/_/wordpress/) to install WordPress (the open source blogging and content management system) and a linked backend MariaDB SQL database.</span></span> <span data-ttu-id="ebedb-140">Zadejte vlastní *MYSQL_ROOT_PASSWORD* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ebedb-140">Enter your own *MYSQL_ROOT_PASSWORD* as follows:</span></span>

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

## <a name="start-the-containers-with-compose"></a><span data-ttu-id="ebedb-141">Kontejnery začínat Compose</span><span class="sxs-lookup"><span data-stu-id="ebedb-141">Start the containers with Compose</span></span>
<span data-ttu-id="ebedb-142">Ve stejném adresáři jako vaše *docker-compose.yml* souboru, spusťte následující příkaz (v závislosti na vašem prostředí, možná budete muset spustit `docker-compose` pomocí `sudo`):</span><span class="sxs-lookup"><span data-stu-id="ebedb-142">In the same directory as your *docker-compose.yml* file, run the following command (depending on your environment, you might need to run `docker-compose` using `sudo`):</span></span>

```bash
docker-compose up -d
```

<span data-ttu-id="ebedb-143">Tento příkaz spustí kontejnery Docker zadaný v *docker-compose.yml*.</span><span class="sxs-lookup"><span data-stu-id="ebedb-143">This command starts the Docker containers specified in *docker-compose.yml*.</span></span> <span data-ttu-id="ebedb-144">Bude trvat minutu nebo dvě pro tento krok dokončit.</span><span class="sxs-lookup"><span data-stu-id="ebedb-144">It takes a minute or two for this step to complete.</span></span> <span data-ttu-id="ebedb-145">Zobrazí výstup, podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="ebedb-145">You see output similar to the following example:</span></span>

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> <span data-ttu-id="ebedb-146">Nezapomeňte použít **-d** možnost spuštění tak, aby kontejnery spouštět nepřetržitě na pozadí.</span><span class="sxs-lookup"><span data-stu-id="ebedb-146">Be sure to use the **-d** option on start-up so that the containers run in the background continuously.</span></span>


<span data-ttu-id="ebedb-147">Chcete-li ověřit, zda jsou kontejnery, zadejte `docker-compose ps`.</span><span class="sxs-lookup"><span data-stu-id="ebedb-147">To verify that the containers are up, type `docker-compose ps`.</span></span> <span data-ttu-id="ebedb-148">Měli byste vidět něco podobného jako:</span><span class="sxs-lookup"><span data-stu-id="ebedb-148">You should see something like:</span></span>

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

<span data-ttu-id="ebedb-149">Nyní můžete připojit k WordPress přímo na virtuálním počítači na portu 80.</span><span class="sxs-lookup"><span data-stu-id="ebedb-149">You can now connect to WordPress directly on the VM on port 80.</span></span> <span data-ttu-id="ebedb-150">Otevřete webový prohlížeč a zadejte název DNS vašeho virtuálního počítače (například `http://mypublicdns.westus.cloudapp.azure.com`).</span><span class="sxs-lookup"><span data-stu-id="ebedb-150">Open a web browser and enter the DNS name of your VM (such as `http://mypublicdns.westus.cloudapp.azure.com`).</span></span> <span data-ttu-id="ebedb-151">Teď byste měli vidět WordPress úvodní obrazovce, kde můžete dokončíte instalaci a začít pracovat s aplikací.</span><span class="sxs-lookup"><span data-stu-id="ebedb-151">You should now see the WordPress start screen, where you can complete the installation and get started with the application.</span></span>

![WordPress úvodní obrazovka][wordpress_start]

## <a name="next-steps"></a><span data-ttu-id="ebedb-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ebedb-153">Next steps</span></span>
* <span data-ttu-id="ebedb-154">Přejděte na [uživatelská příručka k rozšíření virtuálního počítače Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) další možnosti konfigurace Docker a vytvářené v Docker virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ebedb-154">Go to the [Docker VM extension user guide](https://github.com/Azure/azure-docker-extension/blob/master/README.md) for more options to configure Docker and Compose in your Docker VM.</span></span> <span data-ttu-id="ebedb-155">Jednou z možností je například umístění souboru yml vytvářené (převést na JSON) přímo v konfiguraci rozšíření Docker virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ebedb-155">For example, one option is to put the Compose yml file (converted to JSON) directly in the configuration of the Docker VM extension.</span></span>
* <span data-ttu-id="ebedb-156">Podívejte se [tvoří reference k příkazovému řádku](http://docs.docker.com/compose/reference/) a [uživatelská příručka](http://docs.docker.com/compose/) Další příklady vytváření a nasazování aplikací pro službu kontejneru.</span><span class="sxs-lookup"><span data-stu-id="ebedb-156">Check out the [Compose command-line reference](http://docs.docker.com/compose/reference/) and [user guide](http://docs.docker.com/compose/) for more examples of building and deploying multi-container apps.</span></span>
* <span data-ttu-id="ebedb-157">Pomocí šablony Azure Resource Manager, buď vaše vlastní nebo jeden podílí z [komunity](https://azure.microsoft.com/documentation/templates/), k nasazení virtuálního počítače Azure s Docker a nastavit vytvářené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebedb-157">Use an Azure Resource Manager template, either your own or one contributed from the [community](https://azure.microsoft.com/documentation/templates/), to deploy an Azure VM with Docker and an application set up with Compose.</span></span> <span data-ttu-id="ebedb-158">Například [nasazení blog WordPress pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) šablona používá Docker a skládání s back-end databáze MySQL na virtuálního počítače s Ubuntu rychle nasadit WordPress.</span><span class="sxs-lookup"><span data-stu-id="ebedb-158">For example, the [Deploy a WordPress blog with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) template uses Docker and Compose to quickly deploy WordPress with a MySQL backend on an Ubuntu VM.</span></span>
* <span data-ttu-id="ebedb-159">Zkuste integraci Docker Compose do clusteru s podporou Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="ebedb-159">Try integrating Docker Compose with a Docker Swarm cluster.</span></span> <span data-ttu-id="ebedb-160">V tématu [pomocí tvoří s Swarm](https://docs.docker.com/compose/swarm/) pro scénáře.</span><span class="sxs-lookup"><span data-stu-id="ebedb-160">See [Using Compose with Swarm](https://docs.docker.com/compose/swarm/) for scenarios.</span></span>

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
