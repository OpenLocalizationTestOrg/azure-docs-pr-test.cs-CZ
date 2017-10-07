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
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension-with-hello-azure-cli-10"></a>Vytvořte prostředí Docker v Azure pomocí rozšíření virtuálního počítače Docker hello hello Azure CLI 1.0
Docker je Oblíbené kontejner správy a vytváření bitové kopie platforma, která vám umožní tooquickly práce s kontejnery na Linuxu (a také Windows). V Azure existují různé způsoby, které můžete nasadit podle potřeby tooyour Docker. Tento článek se zaměřuje na pomocí rozšíření virtuálního počítače Docker hello a šablon Azure Resource Manageru. 

Další informace o hello různých metod nasazení, včetně použití Docker počítače a služby kontejneru Azure najdete v části hello následující články:

* prototyp tooquickly na aplikaci, můžete vytvořit jeden hostitel Docker pomocí [počítač Docker](docker-machine.md).
* Pro větší, stabilnější prostředí, můžete použít rozšíření virtuálního počítače Azure Docker hello, který podporuje i [Docker Compose](https://docs.docker.com/compose/overview/) toogenerate konzistentní kontejner nasazení. Tento článek údaje pomocí rozšíření virtuálního počítače Azure Docker hello.
* toobuild produkční prostředí, škálovatelné prostředí, která poskytují další plánování a nástroje pro správu, můžete nasadit [clusteru Docker Swarm na kontejneru služby Azure](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- [Azure CLI 1.0](#azure-docker-vm-extension-overview) – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](dockerextension.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello 

## <a name="azure-docker-vm-extension-overview"></a>Přehled rozšíření Azure virtuální počítač Docker
Hello rozšíření virtuálního počítače Azure Docker nainstaluje a nakonfiguruje hello Docker démon, klient Docker a Docker Compose ve virtuálním počítači (VM) Linux. Pomocí rozšíření virtuálního počítače Azure Docker hello máte další ovládací prvek a funkce než jednoduše pomocí Docker počítač nebo vytvoření hello Docker hostitele sami. Tyto další funkce, jako například [Docker Compose](https://docs.docker.com/compose/overview/), ujistěte se, rozšíření virtuálního počítače Azure Docker hello vhodné pro robustnější vývojáře nebo v provozním prostředí.

Šablony Azure Resource Manageru definovat strukturu hello celého vašeho prostředí. Šablony umožňují toocreate a nakonfigurujte prostředky, jako je například hello Docker hostitele virtuálních počítačů, úložiště, řízení přístupu na základě Role (RBAC) a diagnostiky. Tyto šablony toocreate další nasazení můžete znovu použít konzistentním způsobem. Další informace o Azure Resource Manager a šablonách najdete v tématu [Přehled služby Správce prostředků](../../azure-resource-manager/resource-group-overview.md). 

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a>Nasazení šablony s hello rozšíření virtuálního počítače Azure Docker
Umožňuje použít existující šablony rychlý start toocreate používající tooinstall rozšíření virtuálního počítače Azure Docker hello virtuálního počítače s Ubuntu a konfigurace hostitele Docker hello. Můžete zobrazit tady šablonu hello: [jednoduché nasazení virtuálního počítače s Ubuntu pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Je třeba hello [nejnovější rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) nainstalován a přihlášení pomocí režimu Resource Manager hello takto:

```azurecli
azure config mode arm
```

Nasazení šablony hello pomocí rozhraní příkazového řádku Azure, zadání hello šablony URI hello. Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění. Použijte vlastní název skupiny prostředků a umístění následujícím způsobem:

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Odpovědět hello výzvy tooname účtu úložiště, zadejte uživatelské jméno a heslo a zadejte název DNS. Hello výstup je podobné toohello následující ukázka:

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

Hello rozhraní příkazového řádku Azure toohello řádku po vrací jenom pár sekund, ale váš hostitel Docker stále probíhá vytváření a konfigurovat rozšíření virtuálního počítače Azure Docker hello. Jak dlouho trvá několik minut, než toofinish nasazení hello. Můžete zobrazit podrobnosti o stavu hostitele Docker hello pomocí hello `azure vm show` příkaz.

Hello následující příklad ověří hello stav hello virtuálního počítače s názvem *myDockerVM* (hello výchozí název ze šablony hello – neměnit tento název) ve skupině prostředků hello s názvem *myResourceGroup*. Zadejte název skupiny prostředků hello, kterou jste vytvořili v předchozím kroku hello hello:

```azurecli
azure vm show --resource-group myResourceGroup --name myDockerVM
```

Hello výstup hello `azure vm show` příkazu je podobné toohello následující ukázka:

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

V horní hello hello výstupu, uvidíte hello **ProvisioningState** z hello virtuálních počítačů. Zobrazí se při *úspěšné*, hello nasazení je dokončené, a můžete SSH toohello virtuálních počítačů.

Hello konci výstup hello *plně kvalifikovaný název domény* zobrazí hello plně kvalifikovaný název domény vaší Docker hostitele. Tento plně kvalifikovaný název domény se používá tooSSH tooyour Docker hostitele v hello zbývající kroky.

## <a name="deploy-your-first-nginx-container"></a>Nasazení vaší první kontejner nginx a sváže
Jednou hello nasazení úspěšně proběhlo, SSH tooyour nového Docker hostitele z místního počítače. Zadejte uživatelské jméno a plně kvalifikovaný název domény následujícím způsobem:

```bash
ssh ops@mypublicdns.westus.cloudapp.azure.com
```

Po přihlášení toohello hostitelů Docker, můžeme spusťte kontejner nginx:

```bash
sudo docker run -d -p 80:80 nginx
```

výstup Hello je podobné toohello následující ukázka, jak je bitová kopie nginx hello se stáhne a kontejner spuštění:

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

Zkontrolujte stav hello hello kontejnerů spuštěné v hostiteli Docker následujícím způsobem:

```bash
sudo docker ps
```

výstup Hello je podobné toohello následující ukázka, zobrazení tento kontejner nginx hello je spuštěná a porty TCP 80 a 443 a předávaná:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Otevřete toosee vašeho kontejneru v akci, až webový prohlížeč a zadejte hello plně kvalifikovaný název domény vaší Docker hostitele:

![Spuštěné ngnix kontejneru](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure odkaz na šablonu rozšíření virtuální počítač Docker
předchozí příklad Hello používá existující šablony rychlý start. Rozšíření virtuálního počítače Azure Docker hello můžete nasadit také pomocí vlastní šablony Resource Manageru. toodo Ano, přidat hello následující šablony Resource Manageru tooyour, definování hello *vmName* vašeho virtuálního počítače správně:

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

Můžete najít podrobnější návod na pomocí šablony Resource Manageru čtení [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Další kroky
Můžete taky příliš[nakonfigurovat port TCP démon Docker hello](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/), nebo nasazení kontejnerů pomocí [Docker Compose](https://docs.docker.com/compose/overview/). Další informace o hello rozšíření Docker virtuálního počítače Azure, samotné, najdete v části hello [Githubu projektu](https://github.com/Azure/azure-docker-extension/).

Přečtěte si další informace o možnostech pro nasazení dalších Docker ze hello v Azure:

* [Použít počítač Docker s hello Azure ovladačů](docker-machine.md)  
* [Začínáme s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure](docker-compose-quickstart.md).
* [Nasazení clusteru Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

