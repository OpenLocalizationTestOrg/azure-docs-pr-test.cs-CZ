---
title: "aaaUse hello rozšíření virtuálního počítače Azure Docker s hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak toouse hello tooquickly rozšíření virtuálního počítače Docker a bezpečně nasazovat Docker prostředí v Azure pomocí šablony Resource Manageru."
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
ms.openlocfilehash: 2133cdb1af741fe30093910fae5c3b2c91e8d5fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension-with-hello-azure-cli-10"></a><span data-ttu-id="1289d-103">Vytvořte prostředí Docker v Azure pomocí rozšíření virtuálního počítače Docker hello hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1289d-103">Create a Docker environment in Azure using hello Docker VM extension with hello Azure CLI 1.0</span></span>
<span data-ttu-id="1289d-104">Docker je Oblíbené kontejner správy a vytváření bitové kopie platforma, která vám umožní tooquickly práce s kontejnery na Linuxu (a také Windows).</span><span class="sxs-lookup"><span data-stu-id="1289d-104">Docker is a popular container management and imaging platform that allows you tooquickly work with containers on Linux (and Windows as well).</span></span> <span data-ttu-id="1289d-105">V Azure existují různé způsoby, které můžete nasadit podle potřeby tooyour Docker.</span><span class="sxs-lookup"><span data-stu-id="1289d-105">In Azure, there are various ways you can deploy Docker according tooyour needs.</span></span> <span data-ttu-id="1289d-106">Tento článek se zaměřuje na pomocí rozšíření virtuálního počítače Docker hello a šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="1289d-106">This article focuses on using hello Docker VM extension and Azure Resource Manager templates.</span></span> 

<span data-ttu-id="1289d-107">Další informace o hello různých metod nasazení, včetně použití Docker počítače a služby kontejneru Azure najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="1289d-107">For more information about hello different deployment methods, including using Docker Machine and Azure Container Services, see hello following articles:</span></span>

* <span data-ttu-id="1289d-108">prototyp tooquickly na aplikaci, můžete vytvořit jeden hostitel Docker pomocí [počítač Docker](docker-machine.md).</span><span class="sxs-lookup"><span data-stu-id="1289d-108">tooquickly prototype an app, you can create a single Docker host using [Docker Machine](docker-machine.md).</span></span>
* <span data-ttu-id="1289d-109">Pro větší, stabilnější prostředí, můžete použít rozšíření virtuálního počítače Azure Docker hello, který podporuje i [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate konzistentní kontejner nasazení.</span><span class="sxs-lookup"><span data-stu-id="1289d-109">For larger, more stable environments, you can use hello Azure Docker VM extension, which also supports [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate consistent container deployments.</span></span> <span data-ttu-id="1289d-110">Tento článek údaje pomocí rozšíření virtuálního počítače Azure Docker hello.</span><span class="sxs-lookup"><span data-stu-id="1289d-110">This article details using hello Azure Docker VM extension.</span></span>
* <span data-ttu-id="1289d-111">toobuild produkční prostředí, škálovatelné prostředí, která poskytují další plánování a nástroje pro správu, můžete nasadit [clusteru Docker Swarm na kontejneru služby Azure](../../container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="1289d-111">toobuild production-ready, scalable environments that provide additional scheduling and management tools, you can deploy a [Docker Swarm cluster on Azure Container Services](../../container-service/dcos-swarm/container-service-deployment.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="1289d-112">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="1289d-112">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="1289d-113">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="1289d-113">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="1289d-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="1289d-114">[Azure CLI 1.0](#azure-docker-vm-extension-overview) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="1289d-115">[Azure CLI 2.0](dockerextension.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="1289d-115">[Azure CLI 2.0](dockerextension.md) - our next generation CLI for hello resource management deployment model</span></span> 

## <a name="azure-docker-vm-extension-overview"></a><span data-ttu-id="1289d-116">Přehled rozšíření Azure virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="1289d-116">Azure Docker VM extension overview</span></span>
<span data-ttu-id="1289d-117">Hello rozšíření virtuálního počítače Azure Docker nainstaluje a nakonfiguruje hello Docker démon, klient Docker a Docker Compose ve virtuálním počítači (VM) Linux.</span><span class="sxs-lookup"><span data-stu-id="1289d-117">hello Azure Docker VM extension installs and configures hello Docker daemon, Docker client, and Docker Compose in your Linux virtual machine (VM).</span></span> <span data-ttu-id="1289d-118">Pomocí rozšíření virtuálního počítače Azure Docker hello máte další ovládací prvek a funkce než jednoduše pomocí Docker počítač nebo vytvoření hello Docker hostitele sami.</span><span class="sxs-lookup"><span data-stu-id="1289d-118">By using hello Azure Docker VM extension, you have more control and features than simply using Docker Machine or creating hello Docker host yourself.</span></span> <span data-ttu-id="1289d-119">Tyto další funkce, jako například [Docker Compose](https://docs.docker.com/compose/overview/), ujistěte se, rozšíření virtuálního počítače Azure Docker hello vhodné pro robustnější vývojáře nebo v provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1289d-119">These additional features, such as [Docker Compose](https://docs.docker.com/compose/overview/), make hello Azure Docker VM extension suited for more robust developer or production environments.</span></span>

<span data-ttu-id="1289d-120">Šablony Azure Resource Manageru definovat strukturu hello celého vašeho prostředí.</span><span class="sxs-lookup"><span data-stu-id="1289d-120">Azure Resource Manager templates define hello entire structure of your environment.</span></span> <span data-ttu-id="1289d-121">Šablony umožňují toocreate a nakonfigurujte prostředky, jako je například hello Docker hostitele virtuálních počítačů, úložiště, řízení přístupu na základě Role (RBAC) a diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="1289d-121">Templates allow you toocreate and configure resources such as hello Docker host VMs, storage, Role-Based Access Controls (RBAC), and diagnostics.</span></span> <span data-ttu-id="1289d-122">Tyto šablony toocreate další nasazení můžete znovu použít konzistentním způsobem.</span><span class="sxs-lookup"><span data-stu-id="1289d-122">You can reuse these templates toocreate additional deployments in a consistent manner.</span></span> <span data-ttu-id="1289d-123">Další informace o Azure Resource Manager a šablonách najdete v tématu [Přehled služby Správce prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1289d-123">For more information about Azure Resource Manager and templates, see [Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span> 

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a><span data-ttu-id="1289d-124">Nasazení šablony s hello rozšíření virtuálního počítače Azure Docker</span><span class="sxs-lookup"><span data-stu-id="1289d-124">Deploy a template with hello Azure Docker VM extension</span></span>
<span data-ttu-id="1289d-125">Umožňuje použít existující šablony rychlý start toocreate používající tooinstall rozšíření virtuálního počítače Azure Docker hello virtuálního počítače s Ubuntu a konfigurace hostitele Docker hello.</span><span class="sxs-lookup"><span data-stu-id="1289d-125">Let's use an existing quickstart template toocreate an Ubuntu VM that uses hello Azure Docker VM extension tooinstall and configure hello Docker host.</span></span> <span data-ttu-id="1289d-126">Můžete zobrazit tady šablonu hello: [jednoduché nasazení virtuálního počítače s Ubuntu pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span><span class="sxs-lookup"><span data-stu-id="1289d-126">You can view hello template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu).</span></span> 

<span data-ttu-id="1289d-127">Je třeba hello [nejnovější rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) nainstalován a přihlášení pomocí režimu Resource Manager hello takto:</span><span class="sxs-lookup"><span data-stu-id="1289d-127">You need hello [latest Azure CLI](../../cli-install-nodejs.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="1289d-128">Nasazení šablony hello pomocí rozhraní příkazového řádku Azure, zadání hello šablony URI hello.</span><span class="sxs-lookup"><span data-stu-id="1289d-128">Deploy hello template using hello Azure CLI, specifying hello template URI.</span></span> <span data-ttu-id="1289d-129">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění.</span><span class="sxs-lookup"><span data-stu-id="1289d-129">hello following example creates a resource group named *myResourceGroup* in hello *westus* location.</span></span> <span data-ttu-id="1289d-130">Použijte vlastní název skupiny prostředků a umístění následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1289d-130">Use your own resource group name and location as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

<span data-ttu-id="1289d-131">Odpovědět hello výzvy tooname účtu úložiště, zadejte uživatelské jméno a heslo a zadejte název DNS.</span><span class="sxs-lookup"><span data-stu-id="1289d-131">Answer hello prompts tooname your storage account, provide a username and password, and provide a DNS name.</span></span> <span data-ttu-id="1289d-132">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="1289d-132">hello output is similar toohello following example:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for hello following parameters
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

<span data-ttu-id="1289d-133">Hello rozhraní příkazového řádku Azure toohello řádku po vrací jenom pár sekund, ale váš hostitel Docker stále probíhá vytváření a konfigurovat rozšíření virtuálního počítače Azure Docker hello.</span><span class="sxs-lookup"><span data-stu-id="1289d-133">hello Azure CLI returns you toohello prompt after only a few seconds, but your Docker host is still being created and configured by hello Azure Docker VM extension.</span></span> <span data-ttu-id="1289d-134">Jak dlouho trvá několik minut, než toofinish nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="1289d-134">It takes a few minutes for hello deployment toofinish.</span></span> <span data-ttu-id="1289d-135">Můžete zobrazit podrobnosti o stavu hostitele Docker hello pomocí hello `azure vm show` příkaz.</span><span class="sxs-lookup"><span data-stu-id="1289d-135">You can view details about hello Docker host status using hello `azure vm show` command.</span></span>

<span data-ttu-id="1289d-136">Hello následující příklad ověří hello stav hello virtuálního počítače s názvem *myDockerVM* (hello výchozí název ze šablony hello – neměnit tento název) ve skupině prostředků hello s názvem *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="1289d-136">hello following example checks hello status of hello VM named *myDockerVM* (hello default name from hello template - don't change this name) in hello resource group named *myResourceGroup*.</span></span> <span data-ttu-id="1289d-137">Zadejte název skupiny prostředků hello, kterou jste vytvořili v předchozím kroku hello hello:</span><span class="sxs-lookup"><span data-stu-id="1289d-137">Enter hello name of hello resource group you created in hello preceding step:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

<span data-ttu-id="1289d-138">Hello výstup hello `azure vm show` příkazu je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="1289d-138">hello output of hello `azure vm show` command is similar toohello following example:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myDockerVM"
+ Looking up hello NIC "myVMNicD"
+ Looking up hello public ip "myPublicIPD"
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

<span data-ttu-id="1289d-139">V horní hello hello výstupu, uvidíte hello **ProvisioningState** z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1289d-139">Near hello top of hello output, you see hello **ProvisioningState** of hello VM.</span></span> <span data-ttu-id="1289d-140">Zobrazí se při *úspěšné*, hello nasazení je dokončené, a můžete SSH toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1289d-140">When this displays *Succeeded*, hello deployment has finished and you can SSH toohello VM.</span></span>

<span data-ttu-id="1289d-141">Hello konci výstup hello *plně kvalifikovaný název domény* zobrazí hello plně kvalifikovaný název domény vaší Docker hostitele.</span><span class="sxs-lookup"><span data-stu-id="1289d-141">Towards hello end of hello output, *FQDN* displays hello fully qualified domain name of your Docker host.</span></span> <span data-ttu-id="1289d-142">Tento plně kvalifikovaný název domény se používá tooSSH tooyour Docker hostitele v hello zbývající kroky.</span><span class="sxs-lookup"><span data-stu-id="1289d-142">This FQDN is what you use tooSSH tooyour Docker host in hello remaining steps.</span></span>

## <a name="deploy-your-first-nginx-container"></a><span data-ttu-id="1289d-143">Nasazení vaší první kontejner nginx a sváže</span><span class="sxs-lookup"><span data-stu-id="1289d-143">Deploy your first nginx container</span></span>
<span data-ttu-id="1289d-144">Jednou hello nasazení úspěšně proběhlo, SSH tooyour nového Docker hostitele z místního počítače.</span><span class="sxs-lookup"><span data-stu-id="1289d-144">Once hello deployment has finished, SSH tooyour new Docker host from your local computer.</span></span> <span data-ttu-id="1289d-145">Zadejte uživatelské jméno a plně kvalifikovaný název domény následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1289d-145">Enter your own username and FQDN as follows:</span></span>

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

<span data-ttu-id="1289d-146">Po přihlášení toohello hostitelů Docker, můžeme spusťte kontejner nginx:</span><span class="sxs-lookup"><span data-stu-id="1289d-146">Once logged in toohello Docker host, let's run an nginx container:</span></span>

```bash
sudo docker run -d -p 80:80 nginx
```

<span data-ttu-id="1289d-147">výstup Hello je podobné toohello následující ukázka, jak je bitová kopie nginx hello se stáhne a kontejner spuštění:</span><span class="sxs-lookup"><span data-stu-id="1289d-147">hello output is similar toohello following example as hello nginx image is downloaded and a container started:</span></span>

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

<span data-ttu-id="1289d-148">Zkontrolujte stav hello hello kontejnerů spuštěné v hostiteli Docker následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1289d-148">Check hello status of hello containers running on your Docker host as follows:</span></span>

```bash
sudo docker ps
```

<span data-ttu-id="1289d-149">výstup Hello je podobné toohello následující ukázka, zobrazení tento kontejner nginx hello je spuštěná a porty TCP 80 a 443 a předávaná:</span><span class="sxs-lookup"><span data-stu-id="1289d-149">hello output is similar toohello following example, showing that hello nginx container is running and TCP ports 80 and 443 and being forwarded:</span></span>

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

<span data-ttu-id="1289d-150">Otevřete toosee vašeho kontejneru v akci, až webový prohlížeč a zadejte hello plně kvalifikovaný název domény vaší Docker hostitele:</span><span class="sxs-lookup"><span data-stu-id="1289d-150">toosee your container in action, open up a web browser and enter hello FQDN name of your Docker host:</span></span>

![Spuštěné ngnix kontejneru](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a><span data-ttu-id="1289d-152">Azure odkaz na šablonu rozšíření virtuální počítač Docker</span><span class="sxs-lookup"><span data-stu-id="1289d-152">Azure Docker VM extension template reference</span></span>
<span data-ttu-id="1289d-153">předchozí příklad Hello používá existující šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="1289d-153">hello previous example uses an existing quickstart template.</span></span> <span data-ttu-id="1289d-154">Rozšíření virtuálního počítače Azure Docker hello můžete nasadit také pomocí vlastní šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="1289d-154">You can also deploy hello Azure Docker VM extension with your own Resource Manager templates.</span></span> <span data-ttu-id="1289d-155">toodo Ano, přidat hello následující šablony Resource Manageru tooyour, definování hello *vmName* vašeho virtuálního počítače správně:</span><span class="sxs-lookup"><span data-stu-id="1289d-155">toodo so, add hello following tooyour Resource Manager templates, defining hello *vmName* of your VM appropriately:</span></span>

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

<span data-ttu-id="1289d-156">Můžete najít podrobnější návod na pomocí šablony Resource Manageru čtení [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1289d-156">You can find more detailed walkthrough on using Resource Manager templates by reading [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1289d-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1289d-157">Next steps</span></span>
<span data-ttu-id="1289d-158">Můžete taky příliš[nakonfigurovat port TCP démon Docker hello](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/), nebo nasazení kontejnerů pomocí [Docker Compose](https://docs.docker.com/compose/overview/).</span><span class="sxs-lookup"><span data-stu-id="1289d-158">You may wish too[configure hello Docker daemon TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), understand [Docker security](https://docs.docker.com/engine/security/security/), or deploy containers using [Docker Compose](https://docs.docker.com/compose/overview/).</span></span> <span data-ttu-id="1289d-159">Další informace o hello rozšíření Docker virtuálního počítače Azure, samotné, najdete v části hello [Githubu projektu](https://github.com/Azure/azure-docker-extension/).</span><span class="sxs-lookup"><span data-stu-id="1289d-159">For more information on hello Azure Docker VM Extension itself, see hello [GitHub project](https://github.com/Azure/azure-docker-extension/).</span></span>

<span data-ttu-id="1289d-160">Přečtěte si další informace o možnostech pro nasazení dalších Docker ze hello v Azure:</span><span class="sxs-lookup"><span data-stu-id="1289d-160">Read more information about hello additional Docker deployment options in Azure:</span></span>

* [<span data-ttu-id="1289d-161">Použít počítač Docker s hello Azure ovladačů</span><span class="sxs-lookup"><span data-stu-id="1289d-161">Use Docker Machine with hello Azure driver</span></span>](docker-machine.md)  
* <span data-ttu-id="1289d-162">[Začínáme s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure](docker-compose-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="1289d-162">[Get Started with Docker and Compose toodefine and run a multi-container application on an Azure virtual machine](docker-compose-quickstart.md).</span></span>
* [<span data-ttu-id="1289d-163">Nasazení clusteru Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="1289d-163">Deploy an Azure Container Service cluster</span></span>](../../container-service/dcos-swarm/container-service-deployment.md)

