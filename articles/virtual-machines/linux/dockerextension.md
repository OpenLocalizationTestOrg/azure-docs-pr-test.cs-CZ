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
# <a name="create-a-docker-environment-in-azure-using-hello-docker-vm-extension"></a>Vytvořte prostředí Docker v Azure pomocí rozšíření virtuálního počítače Docker hello
Docker je Oblíbené kontejner správy a vytváření bitové kopie platforma, která vám umožní tooquickly práce s kontejnery v systému Linux. V Azure existují různé způsoby, které můžete nasadit podle potřeby tooyour Docker. Tento článek se zaměřuje na rozšíření virtuálního počítače Docker hello a šablon Azure Resource Manageru pomocí hello 2.0 rozhraní příkazového řádku Azure. Můžete také provést tyto kroky hello [Azure CLI 1.0](dockerextension-nodejs.md).

## <a name="azure-docker-vm-extension-overview"></a>Přehled rozšíření Azure virtuální počítač Docker
Hello rozšíření virtuálního počítače Azure Docker nainstaluje a nakonfiguruje hello Docker démon, klient Docker a Docker Compose ve virtuálním počítači (VM) Linux. Pomocí rozšíření virtuálního počítače Azure Docker hello máte další ovládací prvek a funkce než jednoduše pomocí Docker počítač nebo vytvoření hello Docker hostitele sami. Tyto další funkce, jako například [Docker Compose](https://docs.docker.com/compose/overview/), ujistěte se, rozšíření virtuálního počítače Azure Docker hello vhodné pro robustnější vývojáře nebo v provozním prostředí.

Další informace o hello různých metod nasazení, včetně použití Docker počítače a služby kontejneru Azure najdete v části hello následující články:

* prototyp tooquickly na aplikaci, můžete vytvořit jeden hostitel Docker pomocí [počítač Docker](docker-machine.md).
* toobuild produkční prostředí, škálovatelné prostředí, která poskytují další plánování a nástroje pro správu, můžete nasadit [clusteru Docker Swarm na kontejneru služby Azure](../../container-service/dcos-swarm/container-service-deployment.md).

## <a name="deploy-a-template-with-hello-azure-docker-vm-extension"></a>Nasazení šablony s hello rozšíření virtuálního počítače Azure Docker
Umožňuje použít existující šablony rychlý start toocreate používající tooinstall rozšíření virtuálního počítače Azure Docker hello virtuálního počítače s Ubuntu a konfigurace hostitele Docker hello. Můžete zobrazit tady šablonu hello: [jednoduché nasazení virtuálního počítače s Ubuntu pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Je třeba hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění:

```azurecli
az group create --name myResourceGroup --location westus
```

V dalším kroku nasaďte virtuální počítač s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) obsahující rozšíření virtuálního počítače Azure Docker hello z [této šablony Azure Resource Manageru na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Zadat vlastní hodnoty pro *newStorageAccountName*, *adminUsername*, *adminPassword*, a *dnsNameForPublicIP* následujícím způsobem:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Jak dlouho trvá několik minut, než toofinish nasazení hello. Po dokončení nasazení hello [přesunout krok toonext](#deploy-your-first-nginx-container) tooSSH tooyour virtuálních počítačů. 

Můžete také řádku toohello tooinstead návratový řízení a umožňují hello nasazení pokračovat na pozadí hello přidat hello `--no-wait` příznak toohello předcházející příkaz. Tento proces vám umožní tooperform jinou práci v hello rozhraní příkazového řádku při hello nasazení bude stále několik minut. 

Potom můžete zobrazit podrobnosti o stavu hostitele hello Docker s [az virtuálních počítačů zobrazit](/cli/azure/vm#show). Hello následující příklad ověří hello stav hello virtuálního počítače s názvem *myDockerVM* (hello výchozí název ze šablony hello – neměnit tento název) ve skupině prostředků hello s názvem *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

Pokud tento příkaz vrátí *úspěšné*, hello nasazení je dokončené, a můžete SSH toohello virtuálních počítačů v hello následující krok.

## <a name="deploy-your-first-nginx-container"></a>Nasazení vaší první kontejner nginx a sváže
Podrobnosti o tooview vašeho virtuálního počítače, včetně hello název DNS, použijte `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv`. SSH tooyour Docker nového hostitele z místního počítače takto:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
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

Otevřete toosee vašeho kontejneru v akci, až webový prohlížeč a zadejte název DNS hello Docker hostiteli:

![Spuštěné ngnix kontejneru](./media/dockerextension/nginxrunning.png)

## <a name="azure-docker-vm-extension-template-reference"></a>Azure odkaz na šablonu rozšíření virtuální počítač Docker
předchozí příklad Hello používá existující šablony rychlý start. Rozšíření virtuálního počítače Azure Docker hello můžete nasadit také pomocí vlastní šablony Resource Manageru. toodo Ano, přidat hello následující šablony Resource Manageru tooyour, definování hello `vmName` vašeho virtuálního počítače správně:

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

Můžete najít podrobnější návod na pomocí šablony Resource Manageru čtení [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

## <a name="next-steps"></a>Další kroky
Můžete taky příliš[nakonfigurovat port TCP démon Docker hello](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), pochopit [Docker zabezpečení](https://docs.docker.com/engine/security/security/), nebo nasazení kontejnerů pomocí [Docker Compose](https://docs.docker.com/compose/overview/). Další informace o hello rozšíření Docker virtuálního počítače Azure, samotné, najdete v části hello [Githubu projektu](https://github.com/Azure/azure-docker-extension/).

Přečtěte si další informace o možnostech pro nasazení dalších Docker ze hello v Azure:

* [Použít počítač Docker s hello Azure ovladačů](docker-machine.md)  
* [Začínáme s Docker a napište toodefine a spusťte aplikaci služby kontejneru na virtuální počítač Azure](docker-compose-quickstart.md).
* [Nasazení clusteru Azure Container Service](../../container-service/dcos-swarm/container-service-deployment.md)

