---
title: "Vytvoření Image virtuálních počítačů Azure Linux s balírna | Microsoft Docs"
description: "Další informace o použití balírna k vytvoření bitové kopie virtuálních počítačích s Linuxem v Azure"
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
ms.openlocfilehash: 49a74648bd3953647d581c4e7c548985c5000f17
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-packer-to-create-linux-virtual-machine-images-in-azure"></a><span data-ttu-id="41d25-103">Jak používat balírna k vytvoření bitové kopie virtuálních počítačů Linux v Azure</span><span class="sxs-lookup"><span data-stu-id="41d25-103">How to use Packer to create Linux virtual machine images in Azure</span></span>
<span data-ttu-id="41d25-104">Každý virtuální počítač (VM) v Azure je vytvořený z image, která definuje distribuci systému Linux a verzi operačního systému.</span><span class="sxs-lookup"><span data-stu-id="41d25-104">Each virtual machine (VM) in Azure is created from an image that defines the Linux distribution and OS version.</span></span> <span data-ttu-id="41d25-105">Bitové kopie může zahrnovat předinstalované aplikace a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="41d25-105">Images can include pre-installed applications and configurations.</span></span> <span data-ttu-id="41d25-106">Azure Marketplace poskytuje celou řadu imagí první a třetí strany pro aplikaci v prostředích a nejběžnější distribuce, nebo můžete vytvořit vlastní vlastních bitových kopií přizpůsobit svým potřebám.</span><span class="sxs-lookup"><span data-stu-id="41d25-106">The Azure Marketplace provides many first and third-party images for most common distributions and application environments, or you can create your own custom images tailored to your needs.</span></span> <span data-ttu-id="41d25-107">Tento článek popisuje, jak používat nástroj open source [balírna](https://www.packer.io/) definovat a vytvářet vlastní bitové kopie v Azure.</span><span class="sxs-lookup"><span data-stu-id="41d25-107">This article details how to use the open source tool [Packer](https://www.packer.io/) to define and build custom images in Azure.</span></span>


## <a name="create-azure-resource-group"></a><span data-ttu-id="41d25-108">Vytvoření skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="41d25-108">Create Azure resource group</span></span>
<span data-ttu-id="41d25-109">Během procesu vytváření balírna vytvoří dočasný prostředky Azure, jako sestavuje zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="41d25-109">During the build process, Packer creates temporary Azure resources as it builds the source VM.</span></span> <span data-ttu-id="41d25-110">Když Pokud chcete zachytit tohoto zdrojového virtuálního počítače pro použití jako bitovou kopii, je nutné zadat skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="41d25-110">To capture that source VM for use as an image, you must define a resource group.</span></span> <span data-ttu-id="41d25-111">Výstup z procesu sestavení balírna je uložený v této skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="41d25-111">The output from the Packer build process is stored in this resource group.</span></span>

<span data-ttu-id="41d25-112">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="41d25-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="41d25-113">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="41d25-113">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a><span data-ttu-id="41d25-114">Vytvořit přihlašovací údaje Azure</span><span class="sxs-lookup"><span data-stu-id="41d25-114">Create Azure credentials</span></span>
<span data-ttu-id="41d25-115">Balírna ověřuje s Azure pomocí objektu služby.</span><span class="sxs-lookup"><span data-stu-id="41d25-115">Packer authenticates with Azure using a service principal.</span></span> <span data-ttu-id="41d25-116">Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například balírna.</span><span class="sxs-lookup"><span data-stu-id="41d25-116">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Packer.</span></span> <span data-ttu-id="41d25-117">Můžete řídit a definovat oprávnění, jaké operace objektu služby můžete provádět v Azure.</span><span class="sxs-lookup"><span data-stu-id="41d25-117">You control and define the permissions as to what operations the service principal can perform in Azure.</span></span>

<span data-ttu-id="41d25-118">Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstupní přihlašovací údaje, které potřebuje balírna:</span><span class="sxs-lookup"><span data-stu-id="41d25-118">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output the credentials that Packer needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="41d25-119">Příklad výstupu z předchozích příkazů vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="41d25-119">An example of the output from the preceding commands is as follows:</span></span>

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

<span data-ttu-id="41d25-120">K ověření do Azure, musíte také získat svoje ID předplatného Azure s [az účet zobrazit](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="41d25-120">To authenticate to Azure, you also need to obtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="41d25-121">V dalším kroku použijete výstup z těchto dvou příkazů.</span><span class="sxs-lookup"><span data-stu-id="41d25-121">You use the output from these two commands in the next step.</span></span>


## <a name="define-packer-template"></a><span data-ttu-id="41d25-122">Definovat balírna šablonu</span><span class="sxs-lookup"><span data-stu-id="41d25-122">Define Packer template</span></span>
<span data-ttu-id="41d25-123">K vytvoření bitové kopie, vytvořit šablonu jako soubor ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="41d25-123">To build images, you create a template as a JSON file.</span></span> <span data-ttu-id="41d25-124">V šabloně definujete tvůrce a provisioners, které provádějí procesu skutečné sestavení.</span><span class="sxs-lookup"><span data-stu-id="41d25-124">In the template, you define builders and provisioners that carry out the actual build process.</span></span> <span data-ttu-id="41d25-125">Má balírna [zajištění webu pro Azure](https://www.packer.io/docs/builders/azure.html) , což vám umožní definovat prostředky Azure, jako jsou hlavní přihlašovací údaje služby vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="41d25-125">Packer has a [provisioner for Azure](https://www.packer.io/docs/builders/azure.html) that allows you to define Azure resources, such as the service principal credentials created in the preceding step.</span></span>

<span data-ttu-id="41d25-126">Vytvořte soubor s názvem *ubuntu.json* a vložte následující obsah.</span><span class="sxs-lookup"><span data-stu-id="41d25-126">Create a file named *ubuntu.json* and paste the following content.</span></span> <span data-ttu-id="41d25-127">Zadejte vlastní hodnoty pro následující:</span><span class="sxs-lookup"><span data-stu-id="41d25-127">Enter your own values for the following:</span></span>

| <span data-ttu-id="41d25-128">Parametr</span><span class="sxs-lookup"><span data-stu-id="41d25-128">Parameter</span></span>                           | <span data-ttu-id="41d25-129">Kde můžete získat</span><span class="sxs-lookup"><span data-stu-id="41d25-129">Where to obtain</span></span> |
|-------------------------------------|----------------------------------------------------|
| <span data-ttu-id="41d25-130">*client_id*</span><span class="sxs-lookup"><span data-stu-id="41d25-130">*client_id*</span></span>                         | <span data-ttu-id="41d25-131">První řádek výstupu z `az ad sp` vytvoření příkazu - *appId*</span><span class="sxs-lookup"><span data-stu-id="41d25-131">First line of output from `az ad sp` create command - *appId*</span></span> |
| <span data-ttu-id="41d25-132">*tajný klíč client_secret*</span><span class="sxs-lookup"><span data-stu-id="41d25-132">*client_secret*</span></span>                     | <span data-ttu-id="41d25-133">Druhý řádek výstupu z `az ad sp` vytvoření příkazu - *heslo*</span><span class="sxs-lookup"><span data-stu-id="41d25-133">Second line of output from `az ad sp` create command - *password*</span></span> |
| <span data-ttu-id="41d25-134">*tenant_id*</span><span class="sxs-lookup"><span data-stu-id="41d25-134">*tenant_id*</span></span>                         | <span data-ttu-id="41d25-135">Třetí řádek výstupu z `az ad sp` vytvoření příkazu - *klienta*</span><span class="sxs-lookup"><span data-stu-id="41d25-135">Third line of output from `az ad sp` create command - *tenant*</span></span> |
| <span data-ttu-id="41d25-136">*ID_ODBĚRU*</span><span class="sxs-lookup"><span data-stu-id="41d25-136">*subscription_id*</span></span>                   | <span data-ttu-id="41d25-137">Výstup z `az account show` příkaz</span><span class="sxs-lookup"><span data-stu-id="41d25-137">Output from `az account show` command</span></span> |
| <span data-ttu-id="41d25-138">*managed_image_resource_group_name*</span><span class="sxs-lookup"><span data-stu-id="41d25-138">*managed_image_resource_group_name*</span></span> | <span data-ttu-id="41d25-139">Název skupiny prostředků, kterou jste vytvořili v prvním kroku</span><span class="sxs-lookup"><span data-stu-id="41d25-139">Name of resource group you created in the first step</span></span> |
| <span data-ttu-id="41d25-140">*managed_image_name*</span><span class="sxs-lookup"><span data-stu-id="41d25-140">*managed_image_name*</span></span>                | <span data-ttu-id="41d25-141">Název bitové kopie spravovaného disku, který je vytvořen</span><span class="sxs-lookup"><span data-stu-id="41d25-141">Name for the managed disk image that is created</span></span> |


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

<span data-ttu-id="41d25-142">Tato šablona vytvoří bitovou kopii Ubuntu 16.04 LTS, nainstaluje NGINX a pak deprovisions virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="41d25-142">This template builds an Ubuntu 16.04 LTS image, installs NGINX, then deprovisions the VM.</span></span>

> [!NOTE]
> <span data-ttu-id="41d25-143">Pokud rozbalíte na šabloně této zřízení uživatelských přihlašovacích údajů, upravte příkaz zajištění webu, který deprovisions agent Azure číst `-deprovision` místo `deprovision+user`.</span><span class="sxs-lookup"><span data-stu-id="41d25-143">If you expand on this template to provision user credentials, adjust the provisioner command that deprovisions the Azure agent to read `-deprovision` rather than `deprovision+user`.</span></span>
> <span data-ttu-id="41d25-144">`+user` Příznak odebere všechny uživatelské účty ze zdrojového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="41d25-144">The `+user` flag removes all user accounts from the source VM.</span></span>


## <a name="build-packer-image"></a><span data-ttu-id="41d25-145">Sestavení balírna bitové kopie</span><span class="sxs-lookup"><span data-stu-id="41d25-145">Build Packer image</span></span>
<span data-ttu-id="41d25-146">Pokud ještě nemáte balírna nainstalována na místním počítači, [postupujte podle pokynů pro instalaci balírna](https://www.packer.io/docs/install/index.html).</span><span class="sxs-lookup"><span data-stu-id="41d25-146">If you don't already have Packer installed on your local machine, [follow the Packer installation instructions](https://www.packer.io/docs/install/index.html).</span></span>

<span data-ttu-id="41d25-147">Vytvořit bitovou kopii zadáním vaší balírna soubor šablony následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="41d25-147">Build the image by specifying your Packer template file as follows:</span></span>

```bash
./packer build ubuntu.json
```

<span data-ttu-id="41d25-148">Příklad výstupu z předchozích příkazů vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="41d25-148">An example of the output from the preceding commands is as follows:</span></span>

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
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH to become available...
==> azure-arm: Connected to SSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! The waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able to login as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying the machine’s properties ...
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
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```


## <a name="create-vm-from-azure-image"></a><span data-ttu-id="41d25-149">Vytvoření virtuálního počítače z Azure Image</span><span class="sxs-lookup"><span data-stu-id="41d25-149">Create VM from Azure Image</span></span>
<span data-ttu-id="41d25-150">Nyní můžete vytvořit virtuální počítač z bitové kopie s [vytvořit virtuální počítač az](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="41d25-150">You can now create a VM from your Image with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="41d25-151">Určuje obrázek, který jste vytvořili pomocí `--image` parametr.</span><span class="sxs-lookup"><span data-stu-id="41d25-151">Specify the Image you created with the `--image` parameter.</span></span> <span data-ttu-id="41d25-152">Následující příklad vytvoří virtuální počítač s názvem *Můjvp* z *myPackerImage* a generuje klíče SSH, pokud už neexistují:</span><span class="sxs-lookup"><span data-stu-id="41d25-152">The following example creates a VM named *myVM* from *myPackerImage* and generates SSH keys if they do not already exist:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="41d25-153">Jak dlouho trvá několik minut pro vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="41d25-153">It takes a few minutes to create the VM.</span></span> <span data-ttu-id="41d25-154">Po vytvoření virtuálního počítače, poznamenejte si `publicIpAddress` zobrazí pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="41d25-154">Once the VM has been created, take note of the `publicIpAddress` displayed by the Azure CLI.</span></span> <span data-ttu-id="41d25-155">Tato adresa se používá pro přístup k webu NGINX prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="41d25-155">This address is used to access the NGINX site via a web browser.</span></span>

<span data-ttu-id="41d25-156">Povolit webový provoz připojit virtuální počítač, otevřete port 80 z Internetu s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="41d25-156">To allow web traffic to reach your VM, open port 80 from the Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a><span data-ttu-id="41d25-157">Testovací virtuální počítač a NGINX</span><span class="sxs-lookup"><span data-stu-id="41d25-157">Test VM and NGINX</span></span>
<span data-ttu-id="41d25-158">Nyní můžete otevřít webový prohlížeč a zadejte `http://publicIpAddress` na panelu Adresa.</span><span class="sxs-lookup"><span data-stu-id="41d25-158">Now you can open a web browser and enter `http://publicIpAddress` in the address bar.</span></span> <span data-ttu-id="41d25-159">Zadejte vlastní veřejná IP adresa z virtuálního počítače vytvořit proces.</span><span class="sxs-lookup"><span data-stu-id="41d25-159">Provide your own public IP address from the VM create process.</span></span> <span data-ttu-id="41d25-160">Výchozí NGINX stránky se zobrazí jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="41d25-160">The default NGINX page is displayed as in the following example:</span></span>

![Výchozí web NGINX](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a><span data-ttu-id="41d25-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="41d25-162">Next steps</span></span>
<span data-ttu-id="41d25-163">V tomto příkladu jste použili balírna k vytvoření image virtuálního počítače s NGINX již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="41d25-163">In this example, you used Packer to create a VM image with NGINX already installed.</span></span> <span data-ttu-id="41d25-164">Můžete tuto bitovou kopii virtuálního počítače spolu s existující pracovní postupy nasazení, například k nasazení vaší aplikace na virtuální počítače vytvořené z bitové kopie s Ansible, Chef nebo Puppet.</span><span class="sxs-lookup"><span data-stu-id="41d25-164">You can use this VM image alongside existing deployment workflows, such as to deploy your app to VMs created from the Image with Ansible, Chef, or Puppet.</span></span>

<span data-ttu-id="41d25-165">Další příklad šablony balírna pro ostatní distribucích systému Linux, najdete v části [toto úložiště GitHub](https://github.com/hashicorp/packer/tree/master/examples/azure).</span><span class="sxs-lookup"><span data-stu-id="41d25-165">For additional example Packer templates for other Linux distros, see [this GitHub repo](https://github.com/hashicorp/packer/tree/master/examples/azure).</span></span>