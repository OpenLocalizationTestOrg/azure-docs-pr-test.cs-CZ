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
# <a name="how-toouse-packer-toocreate-linux-virtual-machine-images-in-azure"></a>Jak Image toouse balírna toocreate Linux virtuálního počítače v Azure
Každý virtuální počítač (VM) v Azure je vytvořený z image, která definuje hello distribuční Linux a verzi operačního systému. Bitové kopie může zahrnovat předinstalované aplikace a konfigurace. Hello Azure Marketplace poskytuje celou řadu imagí první a třetí strany pro aplikaci v prostředích a nejběžnější distribuce, nebo můžete vytvořit vlastní vlastních bitových kopií přizpůsobit tooyour potřeb. Tento článek popisuje, jak toouse hello otevřete nástroj zdroje [balírna](https://www.packer.io/) toodefine a sestavení vlastní Image ve službě Azure.


## <a name="create-azure-resource-group"></a>Vytvoření skupiny prostředků Azure.
Během procesu sestavení hello balírna vytvoří dočasný prostředky Azure, jako sestavuje hello zdrojového virtuálního počítače. toocapture, který zdroj virtuálního počítače pro použití jako bitovou kopii, je nutné zadat skupinu prostředků. Hello výstup z procesu sestavení balírna hello je uložený v této skupině prostředků.

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Vytvořit přihlašovací údaje Azure
Balírna ověřuje s Azure pomocí objektu služby. Objektu zabezpečení služby Azure je identita zabezpečení, která můžete použít s aplikací, služeb a automatizace nástroje, například balírna. Můžete řídit a definovat hello oprávnění objektu služby hello toowhat operace můžete provádět v rámci Azure.

Vytvoření služby hlavní s [az ad sp vytvořit pro rbac](/cli/azure/ad/sp#create-for-rbac) a výstup hello pověření, které potřebuje balírna:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

Příklad výstupu hello z předchozích příkazů hello vypadá takto:

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

tooauthenticate tooAzure, musíte taky tooobtain ID vašeho předplatného Azure s [az účet zobrazit](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

V dalším kroku hello používáte hello výstup z těchto dvou příkazů.


## <a name="define-packer-template"></a>Definovat balírna šablonu
toobuild Image, vytvořit šablonu jako soubor ve formátu JSON. V šabloně hello můžete definovat počítačů a provisioners, které provádějí hello skutečný proces sestavení. Má balírna [zajištění webu pro Azure](https://www.packer.io/docs/builders/azure.html) , což vám umožní toodefine Azure prostředky, například hello hlavní přihlašovací údaje služby vytvořené v předchozím kroku hello.

Vytvořte soubor s názvem *ubuntu.json* a hello vložte následující obsah. Zadejte vlastní hodnoty pro hello následující:

| Parametr                           | Kde tooobtain |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | První řádek výstupu z `az ad sp` vytvoření příkazu - *appId* |
| *tajný klíč client_secret*                     | Druhý řádek výstupu z `az ad sp` vytvoření příkazu - *heslo* |
| *tenant_id*                         | Třetí řádek výstupu z `az ad sp` vytvoření příkazu - *klienta* |
| *ID_ODBĚRU*                   | Výstup z `az account show` příkaz |
| *managed_image_resource_group_name* | Název skupiny prostředků, kterou jste vytvořili v prvním kroku hello |
| *managed_image_name*                | Název bitové kopie hello spravovaného disku, který je vytvořen |


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

Tato šablona vytvoří bitovou kopii Ubuntu 16.04 LTS, nainstaluje NGINX a pak deprovisions hello virtuálních počítačů.

> [!NOTE]
> Pokud rozbalíte na tuto šablonu tooprovision uživatelských přihlašovacích údajů, upravte příkaz hello zajištění webu, deprovisions hello tooread Azure agent `-deprovision` místo `deprovision+user`.
> Hello `+user` příznak odebere všechny uživatelské účty z hello zdrojového virtuálního počítače.


## <a name="build-packer-image"></a>Sestavení balírna bitové kopie
Pokud ještě nemáte balírna nainstalována na místním počítači, [postupujte podle pokynů pro instalaci balírna hello](https://www.packer.io/docs/install/index.html).

Sestavení hello bitové kopie zadáním vaší balírna soubor šablony následujícím způsobem:

```bash
./packer build ubuntu.json
```

Příklad výstupu hello z předchozích příkazů hello vypadá takto:

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


## <a name="create-vm-from-azure-image"></a>Vytvoření virtuálního počítače z Azure Image
Nyní můžete vytvořit virtuální počítač z bitové kopie s [vytvořit virtuální počítač az](/cli/azure/vm#create). Zadejte hello bitové kopie, které jste vytvořili pomocí hello `--image` parametr. Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* z *myPackerImage* a generuje klíče SSH, pokud už neexistují:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Jak dlouho trvá několik minut toocreate hello virtuálních počítačů. Po vytvoření hello virtuálních počítačů, poznamenejte si hello `publicIpAddress` zobrazí hello rozhraní příkazového řádku Azure. Tato adresa je použité tooaccess hello NGINX web prostřednictvím webového prohlížeče.

tooallow webový provoz tooreach virtuální počítač, otevřete port 80 z hello Internet s [az virtuálních počítačů open-port](/cli/azure/vm#open-port):

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Testovací virtuální počítač a NGINX
Nyní můžete otevřít webový prohlížeč a zadejte `http://publicIpAddress` v panelu Adresa hello. Zadejte vlastní veřejná IP adresa z hello virtuálního počítače vytvořit proces. Hello výchozí NGINX stránky se zobrazí jako hello následující ukázka:

![Výchozí web NGINX](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Další kroky
V tomto příkladu jste použili balírna toocreate image virtuálního počítače s NGINX již nainstalován. Můžete použít tuto bitovou kopii virtuálního počítače spolu s existující pracovní postupy nasazení, například toodeploy tooVMs vaší aplikace vytvořené z bitové kopie s Ansible, Chef nebo Puppet hello.

Další příklad šablony balírna pro ostatní distribucích systému Linux, najdete v části [toto úložiště GitHub](https://github.com/hashicorp/packer/tree/master/examples/azure).