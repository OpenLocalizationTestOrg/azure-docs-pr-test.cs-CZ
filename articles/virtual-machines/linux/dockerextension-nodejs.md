---
title: "Pomocí rozšíření virtuálního počítače Azure Docker 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Další informace o použití rozšíření virtuálního počítače Docker rychle a bezpečně nasadit Docker prostředí v Azure pomocí šablony Resource Manageru."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: a3cbcf63533f4042dcd695e141655c5814bd7068
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension-with-the-azure-cli-10"></a><span data-ttu-id="002ad-103">Vytvořte prostředí Docker v Azure pomocí Azure CLI 1.0 rozšíření virtuálního počítače Docker</span><span class="sxs-lookup"><span data-stu-id="002ad-103">Create a Docker environment in Azure using the Docker VM extension with the Azure CLI 1.0</span></span>
<span data-ttu-id="002ad-104">Docker je Oblíbené kontejner správy a vytváření bitové kopie platforma, která umožňuje rychle pracovat s kontejnery na Linuxu (a také Windows).</span><span class="sxs-lookup"><span data-stu-id="002ad-104">Docker is a popular container management and imaging platform that allows you to quickly work with containers on Linux (and Windows as well).</span></span> <span data-ttu-id="002ad-105">V Azure existují různé způsoby, které můžete nasadit Docker podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="002ad-105">In Azure, there are various ways you can deploy Docker according to your needs.</span></span> <span data-ttu-id="002ad-106">Tento článek se zaměřuje na pomocí rozšíření virtuálního počítače Docker a šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="002ad-106">This article focuses on using the Docker VM extension and Azure Resource Manager templates.</span></span> 

<span data-ttu-id="002ad-107">Další informace o různých metodách nasazení, včetně použití Docker počítače a služby kontejneru Azure najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="002ad-107">For more information about the different deployment methods, including using Docker Machine and Azure Container Services, see the following articles:</span></span>

* <span data-ttu-id="002ad-108">Chcete rychle prototypu na aplikaci, můžete vytvořit pomocí jednoho hostitele Docker [počítač Docker](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="002ad-108">To quickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="002ad-109">Pro větší, stabilnější prostředí, můžete použít rozšíření virtuálního počítače Azure Docker, který podporuje i [Docker Compose](https://docs.docker.com/compose/overview/) ke generování nasazení konzistentní kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="002ad-109">For larger, more stable environments, you can use the Azure Docker VM extension, which also supports [Docker Compose](https://docs.docker.com/compose/overview/) to generate consistent container deployments.</span></span> <span data-ttu-id="002ad-110">Tento článek údaje pomocí rozšíření virtuálního počítače Azure Docker.</span><span class="sxs-lookup"><span data-stu-id="002ad-110">This article details using the Azure Docker VM extension.</span></span>
* <span data-ttu-id="002ad-111">Pokud chcete vytvořit produkční prostředí, škálovatelné prostředí, které poskytují další plánování a nástroje pro správu, můžete nasadit [clusteru Docker Swarm na kontejneru služby Azure](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="002ad-111">To build production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="002ad-112">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="002ad-112">CLI versions to complete the task</span></span>
<span data-ttu-id="002ad-113">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="002ad-113">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="002ad-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="002ad-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="002ad-115">[Azure CLI 2.0](dockerextension.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="002ad-115">[Azure CLI 2.0](dockerextension.md) - our next generation CLI for the resource management deployment model</span></span> 

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="002ad-116">Přehled rozšíření Azure virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="002ad-116">Azure Docker VM extension overview</span></span>
<span data-ttu-id="002ad-117">Rozšíření virtuálního počítače Azure Docker nainstaluje a nakonfiguruje démon Docker, Docker klienta a Docker Compose ve virtuálním počítači (VM) Linux.</span><span class="sxs-lookup"><span data-stu-id="002ad-117">The Azure Docker VM extension installs and configures the Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="002ad-118">Pomocí rozšíření virtuálního počítače Azure Docker máte další ovládací prvek a funkce než jednoduše pomocí Docker počítače nebo vytváření hostitelů Docker sami.</span><span class="sxs-lookup"><span data-stu-id="002ad-118">By using the Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating the Docker host yourself.</span></span> <span data-ttu-id="002ad-119">Tyto další funkce, jako například [Docker Compose](https://docs.docker.com/compose/overview/), ověřte rozšíření virtuálního počítače Azure Docker vhodné pro robustnější vývojáře nebo v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="002ad-119">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make the Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="002ad-120">Šablony Azure Resource Manageru definovat strukturu celého vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="002ad-120">Azure Resource Manager templates define the entire structure of your environment.</span></span> <span data-ttu-id="002ad-121">Šablony umožňují vytvořit a nakonfigurovat prostředkům, například Docker hostitele virtuálních počítačů, úložiště, řízení přístupu na základě rolí (RBAC) a diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="002ad-121">Templates allow you to create and configure resources such as the Docker host VMs, storage, Role-Based Access Controls (RBAC), and diagnostics.</span></span> <span data-ttu-id="002ad-122">Tyto šablony k vytvoření další nasazení konzistentním způsobem můžete znovu použít.</span><span class="sxs-lookup"><span data-stu-id="002ad-122">You can reuse these templates to create additional deployments in a consistent manner.</span></span> <span data-ttu-id="002ad-123">Další informace o Azure Resource Manager a šablonách najdete v tématu [Přehled služby Správce prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="002ad-123">For more information about Azure Resource Manager and templates, see [Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> 

## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a><span data-ttu-id="002ad-124">Nasazení šablony s příponou virtuálního počítače Azure Docker</span><span class="sxs-lookup"><span data-stu-id="002ad-124">Deploy a template with the Azure Docker VM extension</span></span>
<span data-ttu-id="002ad-125">K vytvoření virtuálního počítače s Ubuntu využívající rozšíření virtuálního počítače Azure Docker na instalaci a konfiguraci hostitelů Docker použijeme existující šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="002ad-125">Let's use an existing quickstart template to create an Ubuntu VM that uses the Azure Docker VM extension to install and configure the Docker host.</span></span> <span data-ttu-id="002ad-126">Můžete zobrazit šablonu zde: [jednoduché nasazení virtuálního počítače s Ubuntu pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="002ad-126">You can view the template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="002ad-127">Je nutné [nejnovější rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) nainstalován a přihlášení pomocí režimu Resource Manager takto:</span><span class="sxs-lookup"><span data-stu-id="002ad-127">You need the [latest Azure CLI](../../cli-install-nodejs.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="002ad-128">Šablonu nasaďte pomocí rozhraní příkazového řádku Azure, určení šablony URI.</span><span class="sxs-lookup"><span data-stu-id="002ad-128">Deploy the template using the Azure CLI, specifying the template URI.</span></span> <span data-ttu-id="002ad-129">Následující příklad vytvoří skupinu prostředků *myResourceGroup* v umístění *westus*.</span><span class="sxs-lookup"><span data-stu-id="002ad-129">The following example creates a resource group named *myResourceGroup* in the *westus* location.</span></span> <span data-ttu-id="002ad-130">Použijte vlastní název skupiny prostředků a umístění následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="002ad-130">Use your own resource group name and location as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="002ad-131">Odpovězte výzev a zadejte uživatelské jméno a heslo, název účtu úložiště a zadejte název DNS.</span><span class="sxs-lookup"><span data-stu-id="002ad-131">Answer the prompts to name your storage account, provide a username and password, and provide a DNS name.</span></span> <span data-ttu-id="002ad-132">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="002ad-132">The output is similar to the following example:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: azureuser
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicidns
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

<span data-ttu-id="002ad-133">Vrátí rozhraní příkazového řádku Azure, můžete do příkazového řádku po jenom pár sekund, ale váš hostitel Docker stále probíhá a konfigurují rozšíření virtuálního počítače Azure Docker.</span><span class="sxs-lookup"><span data-stu-id="002ad-133">The Azure CLI returns you to the prompt after only a few seconds, but your Docker host is still being created and configured by the Azure Docker VM extension.</span></span> <span data-ttu-id="002ad-134">Jak dlouho trvá několik minut na dokončení nasazení.</span><span class="sxs-lookup"><span data-stu-id="002ad-134">It takes a few minutes for the deployment to finish.</span></span> <span data-ttu-id="002ad-135">Můžete zobrazit podrobnosti o používání stav hostitelů Docker `azure vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="002ad-135">You can view details about the Docker host status using the `azure vm show` command.</span></span>

<span data-ttu-id="002ad-136">Následující příklad ověří stav virtuálního počítače s názvem *myDockerVM* (výchozí název ze šablony - neměnit tento název) ve skupině prostředků s názvem *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="002ad-136">The following example checks the status of the VM named *myDockerVM* (the default name from the template - don't change this name) in the resource group named *myResourceGroup*.</span></span> <span data-ttu-id="002ad-137">Zadejte název skupiny prostředků, kterou jste vytvořili v předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="002ad-137">Enter the name of the resource group you created in the preceding step:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

<span data-ttu-id="002ad-138">Výstup `azure vm show` příkaz je podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="002ad-138">The output of the `azure vm show` command is similar to the following example:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicdns.westus.cloudapp.azure.com
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

<span data-ttu-id="002ad-139">V horní části výstupu se zobrazí **ProvisioningState** virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="002ad-139">Near the top of the output, you see the **ProvisioningState** of the VM.</span></span> <span data-ttu-id="002ad-140">Zobrazí se při *úspěšné*, nasazení úspěšně proběhlo, a můžete SSH k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="002ad-140">When this displays *Succeeded*, the deployment has finished and you can SSH to the VM.</span></span>

<span data-ttu-id="002ad-141">Na konci výstupu *plně kvalifikovaný název domény* zobrazuje plně kvalifikovaný název vaší Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="002ad-141">Towards the end of the output, *FQDN* displays the fully qualified domain name of your Docker host.</span></span> <span data-ttu-id="002ad-142">Tento plně kvalifikovaný název domény se používá k SSH k hostiteli Docker v zbývající kroky.</span><span class="sxs-lookup"><span data-stu-id="002ad-142">This FQDN is what you use to SSH to your Docker host in the remaining steps.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="002ad-143">Nasazení vaší první kontejner nginx a sváže</span><span class="sxs-lookup"><span data-stu-id="002ad-143">Deploy your first nginx container</span></span>
<span data-ttu-id="002ad-144">Po nasazení úspěšně proběhlo, SSH vašeho nového hostitele Docker z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="002ad-144">Once the deployment has finished, SSH to your new Docker host from your local computer.</span></span> <span data-ttu-id="002ad-145">Zadejte uživatelské jméno a plně kvalifikovaný název domény následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="002ad-145">Enter your own username and FQDN as follows:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="002ad-146">Po přihlášení k hostitelů Docker, můžeme spusťte kontejner nginx:</span><span class="sxs-lookup"><span data-stu-id="002ad-146">Once logged in to the Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="002ad-147">Výstup se podobá v následujícím příkladu jako bitovou kopii nginx se stáhnou a spustí kontejner:</span><span class="sxs-lookup"><span data-stu-id="002ad-147">The output is similar to the following example as the nginx image is downloaded and a container started:</span></span>

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

<span data-ttu-id="002ad-148">Zkontrolujte stav kontejnery spuštěné v hostiteli Docker následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="002ad-148">Check the status of the containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="002ad-149">Výstup se podobá v následujícím příkladu, zobrazující, že kontejner nginx a sváže spuštěná a porty TCP 80 a 443 a předávaná:</span><span class="sxs-lookup"><span data-stu-id="002ad-149">The output is similar to the following example, showing that the nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="002ad-150">Informace o vaší kontejneru v akci, otevře webový prohlížeč a zadejte plně kvalifikovaný název domény název hostitele vašeho Docker:</span><span class="sxs-lookup"><span data-stu-id="002ad-150">To see your container in action, open up a web browser and enter the FQDN name of your Docker host:</span></span>

![Spuštěné ngnix kontejneru](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="002ad-152">Azure odkaz na šablonu rozšíření virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="002ad-152">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="002ad-153">Předchozí příklad používá existující šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="002ad-153">The previous example uses an existing quickstart template.</span></span> <span data-ttu-id="002ad-154">Rozšíření virtuálního počítače Azure Docker můžete nasadit také pomocí vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="002ad-154">You can also deploy the Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="002ad-155">Uděláte to tak, přidejte následující šablony Resource Manageru, definování *vmName* vašeho virtuálního počítače správně:</span><span class="sxs-lookup"><span data-stu-id="002ad-155">To do so, add the following to your Resource Manager templates, defining the *vmName* of your VM appropriately:</span></span>

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
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

<span data-ttu-id="002ad-156">Můžete najít podrobnější návod na pomocí šablony Resource Manageru čtení [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="002ad-156">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="002ad-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="002ad-157">Next steps</span></span>
<span data-ttu-id="002ad-158">Pravděpodobně budete chtít [nakonfigurujte funkce Docker TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/), nebo nasazení kontejnerů pomocí [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="002ad-158">You may wish to [configure the Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="002ad-159">Další informace o rozšíření virtuálního počítače Azure Docker, samotné, najdete v článku [Githubu projektu](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="002ad-159">For more information on the Azure Docker VM Extension itself, see the [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="002ad-160">Přečtěte si další informace o dalších možnostech nasazení Docker v Azure:</span><span class="sxs-lookup"><span data-stu-id="002ad-160">Read more information about the additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="002ad-161">Použít počítač Docker pomocí Azure ovladače</span><span class="sxs-lookup"><span data-stu-id="002ad-161">Use Docker Machine with the Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="002ad-162">[Začínáme s Docker a vytvářené definovat a spusťte aplikaci služby kontejneru na virtuální počítač Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="002ad-162">[Get Started with Docker and Compose to define and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="002ad-163">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="002ad-163">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

