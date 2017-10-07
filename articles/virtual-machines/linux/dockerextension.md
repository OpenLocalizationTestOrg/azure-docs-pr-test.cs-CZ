---
title: "aaaUse hello rozšíření virtuálního počítače Azure Docker | Microsoft Docs"
description: "Další informace jak toouse hello tooquickly rozšíření virtuálního počítače Docker a bezpečně nasadit Docker prostředí v Azure pomocí šablony Resource Manageru a hello 2.0 rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: 8e43adc594192773466ccd2d3e455105f14c1a61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension"></a><span data-ttu-id="a7092-103">Vytvořte prostředí Docker v Azure pomocí rozšíření virtuálního počítače Docker hello</span><span class="sxs-lookup"><span data-stu-id="a7092-103">Create a Docker environment in Azure using hello Docker VM extension</span></span>
<span data-ttu-id="a7092-104">Docker je Oblíbené kontejner správy a vytváření bitové kopie platforma, která vám umožní tooquickly práce s kontejnery v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="a7092-104">Docker is a popular container management and imaging platform that allows you tooquickly work with containers on Linux.</span></span> <span data-ttu-id="a7092-105">V Azure existují různé způsoby, které můžete nasadit podle potřeby tooyour Docker.</span><span class="sxs-lookup"><span data-stu-id="a7092-105">In Azure, there are various ways you can deploy Docker according tooyour needs.</span></span> <span data-ttu-id="a7092-106">Tento článek se zaměřuje na rozšíření virtuálního počítače Docker hello a šablon Azure Resource Manageru pomocí hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="a7092-106">This article focuses on using hello Docker VM extension and Azure Resource Manager templates with hello Azure CLI 2.0.</span></span> <span data-ttu-id="a7092-107">Můžete také provést tyto kroky hello [Azure CLI 1.0](dockerextension-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a7092-107">You can also perform these steps with hello [Azure CLI 1.0](dockerextension-nodejs.md).</span></span>

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="a7092-108">Přehled rozšíření Azure virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="a7092-108">Azure Docker VM extension overview</span></span>
<span data-ttu-id="a7092-109">Hello rozšíření virtuálního počítače Azure Docker nainstaluje a nakonfiguruje hello Docker démon, klient Docker a Docker Compose ve virtuálním počítači (VM) Linux.</span><span class="sxs-lookup"><span data-stu-id="a7092-109">hello Azure Docker VM extension installs and configures hello Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="a7092-110">Pomocí rozšíření virtuálního počítače Azure Docker hello máte další ovládací prvek a funkce než jednoduše pomocí Docker počítač nebo vytvoření hello Docker hostitele sami.</span><span class="sxs-lookup"><span data-stu-id="a7092-110">By using hello Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating hello Docker host yourself.</span></span> <span data-ttu-id="a7092-111">Tyto další funkce, jako například [Docker Compose](https://docs.docker.com/compose/overview/), ujistěte se, rozšíření virtuálního počítače Azure Docker hello vhodné pro robustnější vývojáře nebo v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a7092-111">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make hello Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="a7092-112">Další informace o hello různých metod nasazení, včetně použití Docker počítače a služby kontejneru Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="a7092-112">For more information about hello different deployment methods, including using Docker Machine and Azure Container Services, see hello following articles:</span></span>

* <span data-ttu-id="a7092-113">prototyp tooquickly na aplikaci, můžete vytvořit jeden hostitel Docker pomocí [počítač Docker](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="a7092-113">tooquickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="a7092-114">toobuild produkční prostředí, škálovatelné prostředí, která poskytují další plánování a nástroje pro správu, můžete nasadit [clusteru Docker Swarm na kontejneru služby Azure](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="a7092-114">toobuild production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a><span data-ttu-id="a7092-115">Nasazení šablony s hello rozšíření virtuálního počítače Azure Docker</span><span class="sxs-lookup"><span data-stu-id="a7092-115">Deploy a template with hello Azure Docker VM extension</span></span>
<span data-ttu-id="a7092-116">Umožňuje použít existující šablony rychlý start toocreate používající tooinstall rozšíření virtuálního počítače Azure Docker hello virtuálního počítače s Ubuntu a konfigurace hostitele Docker hello.</span><span class="sxs-lookup"><span data-stu-id="a7092-116">Let's use an existing quickstart template toocreate an Ubuntu VM that uses hello Azure Docker VM extension tooinstall and configure hello Docker host.</span></span> <span data-ttu-id="a7092-117">Můžete zobrazit tady šablonu hello: [jednoduché nasazení virtuálního počítače s Ubuntu pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="a7092-117">You can view hello template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="a7092-118">Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="a7092-118">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="a7092-119">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="a7092-119">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="a7092-120">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění:</span><span class="sxs-lookup"><span data-stu-id="a7092-120">hello following example creates a resource group named *myResourceGroup* in hello *westus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="a7092-121">V dalším kroku nasaďte virtuální počítač s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) obsahující rozšíření virtuálního počítače Azure Docker hello z [této šablony Azure Resource Manageru na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="a7092-121">Next, deploy a VM with [az group deployment create](/cli/azure/group/deployment#create) that includes hello Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> <span data-ttu-id="a7092-122">Zadat vlastní hodnoty pro *newStorageAccountName*, *adminUsername*, *adminPassword*, a *dnsNameForPublicIP* následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7092-122">Provide your own values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP* as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="a7092-123">Jak dlouho trvá několik minut, než toofinish nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="a7092-123">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="a7092-124">Po dokončení nasazení hello [přesunout krok toonext](#deploy-your-first-nginx-container) tooSSH tooyour virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a7092-124">Once hello deployment is finished, [move toonext step](#deploy-your-first-nginx-container) tooSSH tooyour VM.</span></span> 

<span data-ttu-id="a7092-125">Můžete také řádku toohello tooinstead návratový řízení a umožňují hello nasazení pokračovat na pozadí hello přidat hello `--no-wait` příznak toohello předcházející příkaz.</span><span class="sxs-lookup"><span data-stu-id="a7092-125">Optionally, tooinstead return control toohello prompt and let hello deployment continue in hello background, add hello `--no-wait` flag toohello preceding command.</span></span> <span data-ttu-id="a7092-126">Tento proces vám umožní tooperform jinou práci v hello rozhraní příkazového řádku při hello nasazení bude stále několik minut.</span><span class="sxs-lookup"><span data-stu-id="a7092-126">This process allows you tooperform other work in hello CLI while hello deployment continues for a few minutes.</span></span> 

<span data-ttu-id="a7092-127">Potom můžete zobrazit podrobnosti o stavu hostitele hello Docker s [az virtuálních počítačů zobrazit](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="a7092-127">You can then view details about hello Docker host status with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="a7092-128">Hello následující příklad ověří hello stav hello virtuálního počítače s názvem *myDockerVM* (hello výchozí název ze šablony hello – neměnit tento název) ve skupině prostředků hello s názvem *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="a7092-128">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

<span data-ttu-id="a7092-129">Pokud tento příkaz vrátí *úspěšné*, hello nasazení je dokončené, a můžete SSH toohello virtuálních počítačů v hello následující krok.</span><span class="sxs-lookup"><span data-stu-id="a7092-129">When this command returns *Succeeded*, hello deployment has finished and you can SSH toohello VM in hello following step.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="a7092-130">Nasazení vaší první kontejner nginx a sváže</span><span class="sxs-lookup"><span data-stu-id="a7092-130">Deploy your first nginx container</span></span>
<span data-ttu-id="a7092-131">Podrobnosti o tooview vašeho virtuálního počítače, včetně hello název DNS, použijte `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span><span class="sxs-lookup"><span data-stu-id="a7092-131">tooview details of your VM, including hello DNS name, use `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`.</span></span> <span data-ttu-id="a7092-132">SSH tooyour Docker nového hostitele z místního počítače takto:</span><span class="sxs-lookup"><span data-stu-id="a7092-132">SSH tooyour new Docker host from your local computer as follows:</span></span>

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="a7092-133">Po přihlášení toohello hostitelů Docker, můžeme spusťte kontejner nginx:</span><span class="sxs-lookup"><span data-stu-id="a7092-133">Once logged in toohello Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="a7092-134">výstup Hello je podobné toohello následující ukázka, jak je bitová kopie nginx hello se stáhne a kontejner spuštění:</span><span class="sxs-lookup"><span data-stu-id="a7092-134">hello output is similar toohello following example as hello nginx image is downloaded and a container started:</span></span>

```bash
Unable toofind image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

<span data-ttu-id="a7092-135">Zkontrolujte stav hello hello kontejnerů spuštěné v hostiteli Docker následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="a7092-135">Check hello status of hello containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="a7092-136">výstup Hello je podobné toohello následující ukázka, zobrazení tento kontejner nginx hello je spuštěná a porty TCP 80 a 443 a předávaná:</span><span class="sxs-lookup"><span data-stu-id="a7092-136">hello output is similar toohello following example, showing that hello nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="a7092-137">Otevřete toosee vašeho kontejneru v akci, až webový prohlížeč a zadejte název DNS hello Docker hostiteli:</span><span class="sxs-lookup"><span data-stu-id="a7092-137">toosee your container in action, open up a web browser and enter hello DNS name of your Docker host:</span></span>

![Spuštěné ngnix kontejneru](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="a7092-139">Azure odkaz na šablonu rozšíření virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="a7092-139">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="a7092-140">předchozí příklad Hello používá existující šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="a7092-140">hello previous example uses an existing quickstart template.</span></span> <span data-ttu-id="a7092-141">Rozšíření virtuálního počítače Azure Docker hello můžete nasadit také pomocí vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="a7092-141">You can also deploy hello Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="a7092-142">toodo Ano, přidat hello následující šablony Resource Manageru tooyour, definování hello `vmName` vašeho virtuálního počítače správně:</span><span class="sxs-lookup"><span data-stu-id="a7092-142">toodo so, add hello following tooyour Resource Manager templates, defining hello `vmName` of your VM appropriately:</span></span>

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

<span data-ttu-id="a7092-143">Můžete najít podrobnější návod na pomocí šablony Resource Manageru čtení [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a7092-143">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7092-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7092-144">Next steps</span></span>
<span data-ttu-id="a7092-145">Můžete taky příliš[nakonfigurovat port TCP démon Docker hello](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/), nebo nasazení kontejnerů pomocí [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="a7092-145">You may wish too[configure hello Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="a7092-146">Další informace o hello rozšíření Docker virtuálního počítače Azure, samotné, najdete v části hello [Githubu projektu](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="a7092-146">For more information on hello Azure Docker VM Extension itself, see hello [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="a7092-147">Přečtěte si další informace o možnostech pro nasazení dalších Docker ze hello v Azure:</span><span class="sxs-lookup"><span data-stu-id="a7092-147">Read more information about hello additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="a7092-148">Použít počítač Docker s hello Azure ovladačů</span><span class="sxs-lookup"><span data-stu-id="a7092-148">Use Docker Machine with hello Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="a7092-149">[Začínáme s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="a7092-149">[Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="a7092-150">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="a7092-150">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

