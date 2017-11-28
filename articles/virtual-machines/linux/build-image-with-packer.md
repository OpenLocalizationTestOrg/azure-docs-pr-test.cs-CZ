---
title: "aaaHow toocreate Image virtuálních počítačů Azure Linux s balírna | Microsoft Docs"
description: "Zjistěte, jak toouse balírna toocreate bitové kopie virtuálních počítačích s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 5990598859e73efac477884bc8de5fd5138bf6e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-packer-toocreate-linux-virtual-machine-images-in-azure"></a><span data-ttu-id="aae5a-103">Jak Image toouse balírna toocreate Linux virtuálního počítače v Azure</span><span class="sxs-lookup"><span data-stu-id="aae5a-103">How toouse Packer toocreate Linux virtual machine images in Azure</span></span>
<span data-ttu-id="aae5a-104">Každý virtuální počítač (VM) v Azure je vytvořený z image, která definuje hello distribuční Linux a verzi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="aae5a-104">Each virtual machine (VM) in Azure is created from an image that defines hello Linux distribution and OS version.</span></span> <span data-ttu-id="aae5a-105">Bitové kopie může zahrnovat předinstalované aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="aae5a-105">Images can include pre-installed applications and configurations.</span></span> <span data-ttu-id="aae5a-106">Hello Azure Marketplace poskytuje celou řadu imagí první a třetí strany pro aplikaci v prostředích a nejběžnější distribuce, nebo můžete vytvořit vlastní vlastních bitových kopií přizpůsobit tooyour potřeb.</span><span class="sxs-lookup"><span data-stu-id="aae5a-106">hello Azure Marketplace provides many first and third-party images for most common distributions and application environments, or you can create your own custom images tailored tooyour needs.</span></span> <span data-ttu-id="aae5a-107">Tento článek popisuje, jak toouse hello otevřete nástroj zdroje [balírna](https://www.packer.io/) toodefine a sestavení vlastní Image ve službě Azure.</span><span class="sxs-lookup"><span data-stu-id="aae5a-107">This article details how toouse hello open source tool [Packer](https://www.packer.io/) toodefine and build custom images in Azure.</span></span>


## <a name="create-azure-resource-group"></a><span data-ttu-id="aae5a-108">Vytvoření skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="aae5a-108">Create Azure resource group</span></span>
<span data-ttu-id="aae5a-109">Během procesu sestavení hello balírna vytvoří dočasný prostředky Azure, jako sestavuje hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae5a-109">During hello build process, Packer creates temporary Azure resources as it builds hello source VM.</span></span> <span data-ttu-id="aae5a-110">toocapture, který zdroj virtuálního počítače pro použití jako bitovou kopii, je nutné zadat skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="aae5a-110">toocapture that source VM for use as an image, you must define a resource group.</span></span> <span data-ttu-id="aae5a-111">Hello výstup z procesu sestavení balírna hello je uložený v této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="aae5a-111">hello output from hello Packer build process is stored in this resource group.</span></span>

<span data-ttu-id="aae5a-112">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="aae5a-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="aae5a-113">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="aae5a-113">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a><span data-ttu-id="aae5a-114">Vytvořit přihlašovací údaje Azure</span><span class="sxs-lookup"><span data-stu-id="aae5a-114">Create Azure credentials</span></span>
<span data-ttu-id="aae5a-115">Balírna ověřuje s Azure pomocí objektu služby.</span><span class="sxs-lookup"><span data-stu-id="aae5a-115">Packer authenticates with Azure using a service principal.</span></span> <span data-ttu-id="aae5a-116">Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například balírna.</span><span class="sxs-lookup"><span data-stu-id="aae5a-116">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Packer.</span></span> <span data-ttu-id="aae5a-117">Můžete řídit a definovat hello oprávnění objektu služby hello toowhat operace můžete provádět v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="aae5a-117">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span>

<span data-ttu-id="aae5a-118">Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstup hello pověření, které potřebuje balírna:</span><span class="sxs-lookup"><span data-stu-id="aae5a-118">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that Packer needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="aae5a-119">Příklad výstupu hello z předchozích příkazů hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="aae5a-119">An example of hello output from hello preceding commands is as follows:</span></span>

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

<span data-ttu-id="aae5a-120">tooauthenticate tooAzure, musíte taky tooobtain ID vašeho předplatného Azure s [az účet zobrazit](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="aae5a-120">tooauthenticate tooAzure, you also need tooobtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="aae5a-121">V dalším kroku hello používáte hello výstup z těchto dvou příkazů.</span><span class="sxs-lookup"><span data-stu-id="aae5a-121">You use hello output from these two commands in hello next step.</span></span>


## <a name="define-packer-template"></a><span data-ttu-id="aae5a-122">Definovat balírna šablonu</span><span class="sxs-lookup"><span data-stu-id="aae5a-122">Define Packer template</span></span>
<span data-ttu-id="aae5a-123">toobuild Image, vytvořit šablonu jako soubor ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="aae5a-123">toobuild images, you create a template as a JSON file.</span></span> <span data-ttu-id="aae5a-124">V šabloně hello můžete definovat počítačů a provisioners, které provádějí hello skutečný proces sestavení.</span><span class="sxs-lookup"><span data-stu-id="aae5a-124">In hello template, you define builders and provisioners that carry out hello actual build process.</span></span> <span data-ttu-id="aae5a-125">Má balírna [zajištění webu pro Azure](https://www.packer.io/docs/builders/azure.html) , což vám umožní toodefine Azure prostředky, například hello hlavní přihlašovací údaje služby vytvořené v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="aae5a-125">Packer has a [provisioner for Azure](https://www.packer.io/docs/builders/azure.html) that allows you toodefine Azure resources, such as hello service principal credentials created in hello preceding step.</span></span>

<span data-ttu-id="aae5a-126">Vytvořte soubor s názvem *ubuntu.json* a hello vložte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="aae5a-126">Create a file named *ubuntu.json* and paste hello following content.</span></span> <span data-ttu-id="aae5a-127">Zadejte vlastní hodnoty pro hello následující:</span><span class="sxs-lookup"><span data-stu-id="aae5a-127">Enter your own values for hello following:</span></span>

| <span data-ttu-id="aae5a-128">Parametr</span><span class="sxs-lookup"><span data-stu-id="aae5a-128">Parameter</span></span>                           | <span data-ttu-id="aae5a-129">Kde tooobtain</span><span class="sxs-lookup"><span data-stu-id="aae5a-129">Where tooobtain</span></span> |
|-------------------------------------|----------------------------------------------------|
| <span data-ttu-id="aae5a-130">*client_id*</span><span class="sxs-lookup"><span data-stu-id="aae5a-130">*client_id*</span></span>                         | <span data-ttu-id="aae5a-131">První řádek výstupu z `az ad sp` vytvoření příkazu - *appId*</span><span class="sxs-lookup"><span data-stu-id="aae5a-131">First line of output from `az ad sp` create command - *appId*</span></span> |
| <span data-ttu-id="aae5a-132">*tajný klíč client_secret*</span><span class="sxs-lookup"><span data-stu-id="aae5a-132">*client_secret*</span></span>                     | <span data-ttu-id="aae5a-133">Druhý řádek výstupu z `az ad sp` vytvoření příkazu - *heslo*</span><span class="sxs-lookup"><span data-stu-id="aae5a-133">Second line of output from `az ad sp` create command - *password*</span></span> |
| <span data-ttu-id="aae5a-134">*tenant_id*</span><span class="sxs-lookup"><span data-stu-id="aae5a-134">*tenant_id*</span></span>                         | <span data-ttu-id="aae5a-135">Třetí řádek výstupu z `az ad sp` vytvoření příkazu - *klienta*</span><span class="sxs-lookup"><span data-stu-id="aae5a-135">Third line of output from `az ad sp` create command - *tenant*</span></span> |
| <span data-ttu-id="aae5a-136">*ID_ODBĚRU*</span><span class="sxs-lookup"><span data-stu-id="aae5a-136">*subscription_id*</span></span>                   | <span data-ttu-id="aae5a-137">Výstup z `az account show` příkaz</span><span class="sxs-lookup"><span data-stu-id="aae5a-137">Output from `az account show` command</span></span> |
| <span data-ttu-id="aae5a-138">*managed_image_resource_group_name*</span><span class="sxs-lookup"><span data-stu-id="aae5a-138">*managed_image_resource_group_name*</span></span> | <span data-ttu-id="aae5a-139">Název skupiny prostředků, kterou jste vytvořili v prvním kroku hello</span><span class="sxs-lookup"><span data-stu-id="aae5a-139">Name of resource group you created in hello first step</span></span> |
| <span data-ttu-id="aae5a-140">*managed_image_name*</span><span class="sxs-lookup"><span data-stu-id="aae5a-140">*managed_image_name*</span></span>                | <span data-ttu-id="aae5a-141">Název bitové kopie hello spravovaného disku, který je vytvořen</span><span class="sxs-lookup"><span data-stu-id="aae5a-141">Name for hello managed disk image that is created</span></span> |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

<span data-ttu-id="aae5a-142">Tato šablona vytvoří bitovou kopii Ubuntu 16.04 LTS, nainstaluje NGINX a pak deprovisions hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aae5a-142">This template builds an Ubuntu 16.04 LTS image, installs NGINX, then deprovisions hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="aae5a-143">Pokud rozbalíte na tuto šablonu tooprovision uživatelských přihlašovacích údajů, upravte příkaz hello zajištění webu, deprovisions hello tooread Azure agent `-deprovision` místo `deprovision+user`.</span><span class="sxs-lookup"><span data-stu-id="aae5a-143">If you expand on this template tooprovision user credentials, adjust hello provisioner command that deprovisions hello Azure agent tooread `-deprovision` rather than `deprovision+user`.</span></span>
> <span data-ttu-id="aae5a-144">Hello `+user` příznak odebere všechny uživatelské účty z hello zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="aae5a-144">hello `+user` flag removes all user accounts from hello source VM.</span></span>


## <a name="build-packer-image"></a><span data-ttu-id="aae5a-145">Sestavení balírna bitové kopie</span><span class="sxs-lookup"><span data-stu-id="aae5a-145">Build Packer image</span></span>
<span data-ttu-id="aae5a-146">Pokud ještě nemáte balírna nainstalována na místním počítači, [postupujte podle pokynů pro instalaci balírna hello](https://www.packer.io/docs/install/index.html).</span><span class="sxs-lookup"><span data-stu-id="aae5a-146">If you don't already have Packer installed on your local machine, [follow hello Packer installation instructions](https://www.packer.io/docs/install/index.html).</span></span>

<span data-ttu-id="aae5a-147">Sestavení hello bitové kopie zadáním vaší balírna soubor šablony následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="aae5a-147">Build hello image by specifying your Packer template file as follows:</span></span>

```bash
./packer build ubuntu.json
```

<span data-ttu-id="aae5a-148">Příklad výstupu hello z předchozích příkazů hello vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="aae5a-148">An example of hello output from hello preceding commands is as follows:</span></span>

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting hello VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH toobecome available...
==> azure-arm: Connected tooSSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! hello waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able toologin as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying hello machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting hello temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. hello artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```


## <a name="create-vm-from-azure-image"></a><span data-ttu-id="aae5a-149">Vytvoření virtuálního počítače z Azure Image</span><span class="sxs-lookup"><span data-stu-id="aae5a-149">Create VM from Azure Image</span></span>
<span data-ttu-id="aae5a-150">Nyní můžete vytvořit virtuální počítač z bitové kopie s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="aae5a-150">You can now create a VM from your Image with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="aae5a-151">Zadejte hello bitové kopie, které jste vytvořili pomocí hello `--image` parametr.</span><span class="sxs-lookup"><span data-stu-id="aae5a-151">Specify hello Image you created with hello `--image` parameter.</span></span> <span data-ttu-id="aae5a-152">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* z *myPackerImage* a generuje klíče SSH, pokud už neexistují:</span><span class="sxs-lookup"><span data-stu-id="aae5a-152">hello following example creates a VM named *myVM* from *myPackerImage* and generates SSH keys if they do not already exist:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="aae5a-153">Jak dlouho trvá několik minut toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="aae5a-153">It takes a few minutes toocreate hello VM.</span></span> <span data-ttu-id="aae5a-154">Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="aae5a-154">Once hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="aae5a-155">Tato adresa je použité tooaccess hello NGINX web prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="aae5a-155">This address is used tooaccess hello NGINX site via a web browser.</span></span>

<span data-ttu-id="aae5a-156">tooallow webový provoz tooreach virtuální počítač, otevřete port 80 z hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="aae5a-156">tooallow web traffic tooreach your VM, open port 80 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a><span data-ttu-id="aae5a-157">Testovací virtuální počítač a NGINX</span><span class="sxs-lookup"><span data-stu-id="aae5a-157">Test VM and NGINX</span></span>
<span data-ttu-id="aae5a-158">Nyní můžete otevřít webový prohlížeč a zadejte `http://publicIpAddress` v panelu Adresa hello.</span><span class="sxs-lookup"><span data-stu-id="aae5a-158">Now you can open a web browser and enter `http://publicIpAddress` in hello address bar.</span></span> <span data-ttu-id="aae5a-159">Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="aae5a-159">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="aae5a-160">Hello výchozí NGINX stránky se zobrazí jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="aae5a-160">hello default NGINX page is displayed as in hello following example:</span></span>

![Výchozí web NGINX](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a><span data-ttu-id="aae5a-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aae5a-162">Next steps</span></span>
<span data-ttu-id="aae5a-163">V tomto příkladu jste použili balírna toocreate image virtuálního počítače s NGINX již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="aae5a-163">In this example, you used Packer toocreate a VM image with NGINX already installed.</span></span> <span data-ttu-id="aae5a-164">Můžete použít tuto bitovou kopii virtuálního počítače spolu s existující pracovní postupy nasazení, například toodeploy tooVMs vaší aplikace vytvořené z bitové kopie s Ansible, Chef nebo Puppet hello.</span><span class="sxs-lookup"><span data-stu-id="aae5a-164">You can use this VM image alongside existing deployment workflows, such as toodeploy your app tooVMs created from hello Image with Ansible, Chef, or Puppet.</span></span>

<span data-ttu-id="aae5a-165">Další příklad šablony balírna pro ostatní distribucích systému Linux, najdete v části [toto úložiště GitHub](https://github.com/hashicorp/packer/tree/master/examples/azure).</span><span class="sxs-lookup"><span data-stu-id="aae5a-165">For additional example Packer templates for other Linux distros, see [this GitHub repo](https://github.com/hashicorp/packer/tree/master/examples/azure).</span></span>