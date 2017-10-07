---
title: "aaaUse Docker Compose na virtuální počítač s Linuxem v Azure | Microsoft Docs"
description: "Jak hello toouse Docker a vytvářené na virtuální počítače s Linuxem pomocí rozhraní příkazového řádku Azure"
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
ms.openlocfilehash: cf7254ad4813ccdc641fcacbb06ed1514a93cee5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-docker-and-compose-toodefine-and-run-a-multi-container-application-in-azure"></a>Získat začít s Docker a vytvářené toodefine a spuštění aplikace s více kontejnerů v Azure
S [vytvářené](http://github.com/docker/compose), používat toodefine jednoduchý text souboru aplikace, který se skládá z několika kontejnerů Docker. Pak začne pracovat aplikace v jednom příkaz, který nemá všechno, co toodeploy prostředí definované. Jako příklad, tento článek ukazuje, jak tooquickly nastavit blogu WordPress s back-end MariaDB SQL database na virtuálního počítače s Ubuntu. Můžete taky vytvářené tooset až složitějších aplikací.


## <a name="set-up-a-linux-vm-as-a-docker-host"></a>Nastavení virtuálního počítače s Linuxem jako hostitele Docker
Můžete pomocí různých postupů Azure a dostupných imagí nebo šablony Resource Manageru v Azure Marketplace toocreate hello virtuálního počítače s Linuxem a nastavit jako Docker hostitele. Například v tématu [pomocí rozšíření virtuálního počítače Docker toodeploy hello prostředí](dockerextension.md) tooquickly vytvoření virtuálního počítače s Ubuntu s hello rozšíření virtuálního počítače Azure Docker pomocí [šablony rychlý Start](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Pokud používáte rozšíření virtuálního počítače Docker hello, virtuální počítač je automaticky nastavený jako hostitel Docker a Compose je již nainstalován.


### <a name="create-docker-host-with-azure-cli-20"></a>Vytvořit hostitele Docker s Azure CLI 2.0
Instalace hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) a přihlaste se pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Nejprve vytvořte skupinu prostředků pro vaše prostředí Docker s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *westus* umístění:

```azurecli
az group create --name myResourceGroup --location westus
```

V dalším kroku nasaďte virtuální počítač s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create) obsahující rozšíření virtuálního počítače Azure Docker hello z [této šablony Azure Resource Manageru na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). Zadat vlastní hodnoty pro *newStorageAccountName*, *adminUsername*, *adminPassword*, a *dnsNameForPublicIP*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Jak dlouho trvá několik minut, než toofinish nasazení hello. Po dokončení nasazení hello [přesunout krok toonext](#verify-that-compose-is-installed) tooSSH tooyour virtuálních počítačů. 

Můžete také řádku toohello tooinstead návratový řízení a umožňují hello nasazení pokračovat na pozadí hello přidat hello `--no-wait` příznak toohello předcházející příkaz. Tento proces vám umožní tooperform jinou práci v hello rozhraní příkazového řádku při hello nasazení bude stále několik minut. Potom můžete zobrazit podrobnosti o stavu hostitele hello Docker s [az virtuálních počítačů zobrazit](/cli/azure/vm#show). Hello následující příklad ověří hello stav hello virtuálního počítače s názvem *myDockerVM* (hello výchozí název ze šablony hello – neměnit tento název) ve skupině prostředků hello s názvem *myResourceGroup*:

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name myDockerVM \
    --query [provisioningState] \
    --output tsv
```

Pokud tento příkaz vrátí *úspěšné*, hello nasazení je dokončené, a můžete SSH toohello virtuálních počítačů v hello následující krok.


## <a name="verify-that-compose-is-installed"></a>Ověřte, zda je nainstalován Compose
Po dokončení nasazení hello SSH tooyour nové Docker hostitele pomocí hello název DNS, které jste zadali při nasazení. Můžete použít `az vm show -g myResourceGroup -n myDockerVM -d --query [fqdns] -o tsv` tooview podrobnosti virtuálního počítače, včetně názvu DNS hello.

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

toocheck, která tvoří nainstalovaný na hello virtuálního počítače, spusťte následující příkaz hello:

```bash
docker-compose --version
```

Zobrazí výstup podobný příliš*docker compose 1.6.2, sestavení 4d 72027*.

> [!TIP]
> Pokud jste použili jinou metodu toocreate hostitelů Docker a potřebovat tooinstall vytvořit sami, přečtěte si téma hello [tvoří dokumentaci](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="create-a-docker-composeyml-configuration-file"></a>Vytvořte konfigurační soubor docker-compose.yml
Dále vytvoříte `docker-compose.yml` souboru, který je právě text konfigurační soubor, toodefine hello Docker kontejnery toorun na hello virtuálních počítačů. soubor Hello určuje toorun hello bitové kopie na každý kontejner (nebo to může být sestavení z soubor Docker), nezbytné proměnné prostředí a závislosti, porty a hello propojení mezi kontejnery. Informace o syntaxi souboru yml v [vytvořit odkaz na soubor](https://docs.docker.com/compose/compose-file/).

Vytvoření hello *docker-compose.yml* následujícím způsobem:

```bash
touch docker-compose.yml
```

Použijte tooadd vašem oblíbeném textovém editoru soubor toohello některé data. Hello následující příklad používá hello *vi* editoru:

```bash
vi docker-compose.yml
```

Vložte hello následující ukázka do textového souboru. Tato konfigurace používá bitové kopie z hello [DockerHub registru](https://registry.hub.docker.com/_/wordpress/) tooinstall WordPress (hello s otevřeným zdrojem blogu a systém správy obsahu) a propojené back-end MariaDB SQL databáze. Zadejte vlastní *MYSQL_ROOT_PASSWORD* následujícím způsobem:

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

## <a name="start-hello-containers-with-compose"></a>Začínat hello kontejnery vytvářené
V hello stejný adresář jako vaše *docker-compose.yml* souboru, spusťte následující příkaz hello (v závislosti na vašem prostředí, může být nutné toorun `docker-compose` pomocí `sudo`):

```bash
docker-compose up -d
```

Tento příkaz spustí kontejnery Docker hello zadaný v *docker-compose.yml*. Bude trvat minutu nebo dvě pro tento krok toocomplete. Zobrazí výstup podobný toohello následující ukázka:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

> [!NOTE]
> Se, zda text hello toouse **-d** možnost spuštění tak, aby hello kontejnery spouštět nepřetržitě hello pozadí.


tooverify, které jsou hello kontejnery, typ `docker-compose ps`. Měli byste vidět něco podobného jako:

```bash
        Name                       Command               State         Ports
-----------------------------------------------------------------------------------
azureuser_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp
azureuser_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

Teď se můžete připojit tooWordPress přímo na hello virtuálních počítačů na portu 80. Otevřete webový prohlížeč a zadejte název DNS hello vašeho virtuálního počítače (například `http://mypublicdns.westus.cloudapp.azure.com`). Teď byste měli vidět hello WordPress úvodní obrazovka, kde může dokončení instalace hello a začít pracovat s aplikací hello.

![WordPress úvodní obrazovka][wordpress_start]

## <a name="next-steps"></a>Další kroky
* Přejděte toohello [uživatelská příručka k rozšíření virtuálního počítače Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) další možnosti tooconfigure Docker a vytvořit v Docker virtuálního počítače. Jednou z možností je například tooput hello vytvářené yml souboru (převedený tooJSON) přímo v konfiguraci hello hello rozšíření Docker virtuálního počítače.
* Podívejte se na hello [tvoří reference k příkazovému řádku](http://docs.docker.com/compose/reference/) a [uživatelská příručka](http://docs.docker.com/compose/) Další příklady vytváření a nasazování aplikací pro službu kontejneru.
* Pomocí šablony Azure Resource Manager, buď vaše vlastní nebo jeden podílí z hello [komunity](https://azure.microsoft.com/documentation/templates/), toodeploy virtuální počítač Azure s Docker a aplikace nastavit nové. Například hello [nasazení blog WordPress pomocí Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) šablona používá Docker a tooquickly vytvářené nasazení WordPress s back-end databáze MySQL na virtuálního počítače s Ubuntu.
* Zkuste integraci Docker Compose do clusteru s podporou Docker Swarm. V tématu [pomocí tvoří s Swarm](https://docs.docker.com/compose/swarm/) pro scénáře.

<!--Image references-->

[wordpress_start]: media/docker-compose-quickstart/WordPress.png
