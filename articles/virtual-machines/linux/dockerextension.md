---
title: "Použití rozšíření virtuálního počítače Azure Docker | Microsoft Docs"
description: "Další informace o použití rozšíření virtuálního počítače Docker rychle a bezpečně nasadit Docker prostředí v Azure pomocí šablony Resource Manageru a 2.0 rozhraní příkazového řádku Azure"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 936d67d7-6921-4275-bf11-1e0115e66b7f
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 63d0d80999fd57d014c74d5c6aef3733ec2afe85
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a><span data-ttu-id="9d1ac-103">Vytvořte prostředí Docker v Azure pomocí rozšíření virtuálního počítače Docker</span><span class="sxs-lookup"><span data-stu-id="9d1ac-103">Create a Docker environment in Azure using the Docker VM extension</span></span>
<span data-ttu-id="9d1ac-104">Docker je Oblíbené kontejner správy a vytváření bitové kopie platforma, která umožňuje rychle pracovat s kontejnery v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-104">Docker is a popular container management and imaging platform that allows you to quickly work with containers on Linux.</span></span> <span data-ttu-id="9d1ac-105">V Azure existují různé způsoby, které můžete nasadit Docker podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-105">In Azure, there are various ways you can deploy Docker according to your needs.</span></span> <span data-ttu-id="9d1ac-106">Tento článek se zaměřuje na rozšíření virtuálního počítače Docker a šablon Azure Resource Manageru pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-106">This article focuses on using the Docker VM extension and Azure Resource Manager templates with the Azure CLI 2.0.</span></span> <span data-ttu-id="9d1ac-107">K provedení těchto kroků můžete také využít [Azure CLI 1.0](dockerextension-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-107">You can also perform these steps with the [Azure CLI 1.0](dockerextension-nodejs.md).</span></span>

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="9d1ac-108">Přehled rozšíření Azure virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="9d1ac-108">Azure Docker VM extension overview</span></span>
<span data-ttu-id="9d1ac-109">Rozšíření virtuálního počítače Azure Docker nainstaluje a nakonfiguruje démon Docker, Docker klienta a Docker Compose ve virtuálním počítači (VM) Linux.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-109">The Azure Docker VM extension installs and configures the Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="9d1ac-110">Pomocí rozšíření virtuálního počítače Azure Docker máte další ovládací prvek a funkce než jednoduše pomocí Docker počítače nebo vytváření hostitelů Docker sami.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-110">By using the Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating the Docker host yourself.</span></span> <span data-ttu-id="9d1ac-111">Tyto další funkce, jako například [Docker Compose](https://docs.docker.com/compose/overview/), ověřte rozšíření virtuálního počítače Azure Docker vhodné pro robustnější vývojáře nebo v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-111">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make the Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="9d1ac-112">Další informace o různých metodách nasazení, včetně použití Docker počítače a služby kontejneru Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-112">For more information about the different deployment methods, including using Docker Machine and Azure Container Services, see the following articles:</span></span>

* <span data-ttu-id="9d1ac-113">Chcete rychle prototypu na aplikaci, můžete vytvořit pomocí jednoho hostitele Docker [počítač Docker](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-113">To quickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="9d1ac-114">Pokud chcete vytvořit produkční prostředí, škálovatelné prostředí, které poskytují další plánování a nástroje pro správu, můžete nasadit [clusteru Docker Swarm na kontejneru služby Azure](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-114">To build production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a><span data-ttu-id="9d1ac-115">Nasazení šablony s příponou virtuálního počítače Azure Docker</span><span class="sxs-lookup"><span data-stu-id="9d1ac-115">Deploy a template with the Azure Docker VM extension</span></span>
<span data-ttu-id="9d1ac-116">K vytvoření virtuálního počítače s Ubuntu využívající rozšíření virtuálního počítače Azure Docker na instalaci a konfiguraci hostitelů Docker použijeme existující šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-116">Let's use an existing quickstart template to create an Ubuntu VM that uses the Azure Docker VM extension to install and configure the Docker host.</span></span> <span data-ttu-id="9d1ac-117">Můžete zobrazit šablonu zde: [jednoduché nasazení virtuálního počítače s Ubuntu pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-117">You can view the template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="9d1ac-118">Budete potřebovat nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-118">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="9d1ac-119">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-119">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="9d1ac-120">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *westus* umístění:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-120">The following example creates a resource group named *myResourceGroup* in the *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="9d1ac-121">V dalším kroku nasaďte virtuální počítač s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) obsahující rozšíření virtuálního počítače Azure Docker z [této šablony Azure Resource Manageru na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-121">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes the Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="9d1ac-122">Zadat vlastní hodnoty pro *newStorageAccountName*, *adminUsername*, *adminPassword*, a *dnsNameForPublicIP* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-122">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP* as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="9d1ac-123">Jak dlouho trvá několik minut na dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-123">It takes a few minutes for the deployment to finish.</span></span> <span data-ttu-id="9d1ac-124">Po dokončení nasazení [přesunout k dalšímu kroku](#deploy-your-first-nginx-container) k SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-124">Once the deployment is finished, [move to next step](#deploy-your-first-nginx-container) to SSH to your VM.</span></span> 

<span data-ttu-id="9d1ac-125">Volitelně můžete místo toho vrátí ovládacího prvku do příkazového řádku a umožní nasazení pokračovat na pozadí, přidejte `--no-wait` příznak, který předchozí příkaz.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-125">Optionally, to instead return control to the prompt and let the deployment continue in the background, add the `--no-wait` flag to the preceding command.</span></span> <span data-ttu-id="9d1ac-126">Tento postup můžete provádět jinou práci v rozhraní příkazového řádku při nasazení bude stále několik minut.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-126">This process allows you to perform other work in the CLI while the deployment continues for a few minutes.</span></span> 

<span data-ttu-id="9d1ac-127">Potom můžete zobrazit podrobnosti o stavu hostitele Docker s [az virtuálních počítačů zobrazit](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-127">You can then view details about the Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="9d1ac-128">Následující příklad ověří stav virtuálního počítače s názvem *myDockerVM* (výchozí název ze šablony - neměnit tento název) ve skupině prostředků s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-128">The following example checks the status of the VM named *myDockerVM* (the default name from the template - don't change this name) in the resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="9d1ac-129">Pokud tento příkaz vrátí *úspěšné*, nasazení úspěšně proběhlo, a můžete SSH pro virtuální počítač v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-129">When this command returns *Succeeded*, the deployment has finished and you can SSH to the VM in the following step.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="9d1ac-130">Nasazení vaší první kontejner nginx a sváže</span><span class="sxs-lookup"><span data-stu-id="9d1ac-130">Deploy your first nginx container</span></span>
<span data-ttu-id="9d1ac-131">Chcete-li zobrazit podrobnosti o virtuálního počítače, včetně názvu DNS, použijte `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-131">To view details of your VM, including the DNS name, use `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span></span> <span data-ttu-id="9d1ac-132">SSH do vaší nové Docker hostitele z místního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-132">SSH to your new Docker host from your local computer as follows:</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="9d1ac-133">Po přihlášení k hostitelů Docker, můžeme spusťte kontejner nginx:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-133">Once logged in to the Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="9d1ac-134">Výstup se podobá v následujícím příkladu jako bitovou kopii nginx se stáhnou a spustí kontejner:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-134">The output is similar to the following example as the nginx image is downloaded and a container started:</span></span>

```bash
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

<span data-ttu-id="9d1ac-135">Zkontrolujte stav kontejnery spuštěné v hostiteli Docker následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-135">Check the status of the containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="9d1ac-136">Výstup se podobá v následujícím příkladu, zobrazující, že kontejner nginx a sváže spuštěná a porty TCP 80 a 443 a předávaná:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-136">The output is similar to the following example, showing that the nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="9d1ac-137">Informace o vaší kontejneru v akci, otevře webový prohlížeč a zadejte název DNS vašeho Docker hostitele:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-137">To see your container in action, open up a web browser and enter the DNS name of your Docker host:</span></span>

![Spuštěné ngnix kontejneru](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="9d1ac-139">Azure odkaz na šablonu rozšíření virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="9d1ac-139">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="9d1ac-140">Předchozí příklad používá existující šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-140">The previous example uses an existing quickstart template.</span></span> <span data-ttu-id="9d1ac-141">Rozšíření virtuálního počítače Azure Docker můžete nasadit také pomocí vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="9d1ac-141">You can also deploy the Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="9d1ac-142">Uděláte to tak, přidejte následující šablony Resource Manageru, definování `vmName` vašeho virtuálního počítače správně:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-142">To do so, add the following to your Resource Manager templates, defining the `vmName` of your VM appropriately:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.*",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

<span data-ttu-id="9d1ac-143">Můžete najít podrobnější návod na pomocí šablony Resource Manageru čtení [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-143">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9d1ac-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9d1ac-144">Next steps</span></span>
<span data-ttu-id="9d1ac-145">Pravděpodobně budete chtít [nakonfigurujte funkce Docker TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/), nebo nasazení kontejnerů pomocí [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-145">You may wish to [configure the Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="9d1ac-146">Další informace o rozšíření virtuálního počítače Azure Docker, samotné, najdete v článku [Githubu projektu](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-146">For more information on the Azure Docker VM Extension itself, see the [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="9d1ac-147">Přečtěte si další informace o dalších možnostech nasazení Docker v Azure:</span><span class="sxs-lookup"><span data-stu-id="9d1ac-147">Read more information about the additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="9d1ac-148">Použít počítač Docker pomocí Azure ovladače</span><span class="sxs-lookup"><span data-stu-id="9d1ac-148">Use Docker Machine with the Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="9d1ac-149">[Začínáme s Docker a vytvářené definovat a spusťte aplikaci služby kontejneru na virtuální počítač Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="9d1ac-149">[Get Started with Docker and Compose to define and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="9d1ac-150">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="9d1ac-150">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

