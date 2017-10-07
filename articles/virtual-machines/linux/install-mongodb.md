---
title: "aaaInstall MongoDB na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure hello | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace MongoDB na iusing hello Linux virtuálního počítače Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 97a4d9913f0eeaf7b8bf15d7fc81befe538cdc8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm"></a>Jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem
[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné. Tento článek ukazuje, jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure. Můžete také provést tyto kroky hello [Azure CLI 1.0](install-mongodb-nodejs.md). Příklady jsou uvedeny této podrobnosti o tom, jak na:

* [Ručně nainstalujte a nakonfigurujte základní instance MongoDB](#manually-install-and-configure-mongodb-on-a-vm)
* [Vytvořte základní instance MongoDB pomocí šablony Resource Manageru](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Vytvořit komplexní MongoDB horizontálně dělené clusteru s replikou nastaví pomocí šablony Resource Manageru](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Ručně nainstalujte a nakonfigurujte MongoDB na virtuálním počítači
MongoDB [poskytují pokyny k instalaci](https://docs.mongodb.com/manual/administration/install-on-linux/) pro distribucích systému Linux, včetně Red Hat nebo CentOS, SUSE, Ubuntu a Debian. Hello následující příklad vytvoří *CentOS* virtuálních počítačů. toocreate pro toto prostředí musíte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).

Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create). Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* s uživatelem s názvem *azureuser* pomocí ověření veřejného klíče SSH

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH toohello virtuálních počítačů pomocí vlastního uživatelského jména a hello `publicIpAddress` uvedené ve výstupu hello hello v předchozím kroku:

```bash
ssh azureuser@<publicIpAddress>
```

Vytvoření zdroje instalace hello tooadd pro MongoDB, **yum** soubor úložiště následujícím způsobem:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Otevřete soubor úložišti hello MongoDB pro úpravy. Přidejte následující řádky hello:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Nainstalujte MongoDB pomocí **yum** následujícím způsobem:

```bash
sudo yum install -y mongodb-org
```

Ve výchozím nastavení se SELinux vynucuje u CentOS bitové kopie, které brání v přístupu k MongoDB. Instalace nástroje pro správu zásad a konfigurace SELinux tooallow MongoDB toooperate na jeho výchozí port TCP 27017 následujícím způsobem:

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Spusťte službu MongoDB hello následujícím způsobem:

```bash
sudo service mongod start
```

Ověření instalace MongoDB hello se připojíte pomocí hello místní `mongo` klienta:

```bash
mongo
```

Nyní testovací hello MongoDB instance přidáním některá data a pak vyhledávání:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

V případě potřeby nakonfigurujte MongoDB toostart automaticky během restartu systému:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Vytvořte základní instance MongoDB na CentOS pomocí šablony
Na jednom virtuálním počítači CentOS pomocí hello následujících Azure rychlý start šablony z Githubu, můžete vytvořit základní instance MongoDB. Tato šablona používá hello rozšíření vlastních skriptů pro Linux tooadd **yum** tooyour úložiště nově vytvořený CentOS virtuálního počítače a pak nainstalujte MongoDB.

* [Základní instance MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

toocreate pro toto prostředí musíte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

V dalším kroku nasaďte šablonu MongoDB hello s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create). Definovat vlastní prostředek názvy a velikosti, kde je potřeba, například jako u *newStorageAccountName*, *virtualNetworkName*, a *vmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"},
    "virtualNetworkName": {"value": "myVnet"},
    "vmSize": {"value": "Standard_DS2_v2"},
    "vmName": {"value": "myVM"},
    "publicIPAddressName": {"value": "myPublicIP"},
    "nicName": {"value": "myNic"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Přihlaste se toohello virtuálních počítačů pomocí veřejné adresy DNS hello vašeho virtuálního počítače. Můžete zobrazit hello veřejnou adresu DNS s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

SSH tooyour virtuálních počítačů pomocí vlastního uživatelského jména a veřejnou adresu DNS:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Ověření instalace MongoDB hello se připojíte pomocí hello místní `mongo` klienta následujícím způsobem:

```bash
mongo
```

Nyní testovací hello instance přidáním některá data a hledání následujícím způsobem:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Vytvořit Cluster horizontálně dělené komplexní MongoDB na CentOS pomocí šablony
Můžete vytvořit komplexní horizontálně dělené clusteru MongoDB pomocí hello následujících Azure rychlý start šablony z Githubu. Tato šablona následuje hello [MongoDB horizontálně dělené clusteru osvědčené postupy](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundance a vysokou dostupnost. Hello šablona vytvoří dvě horizontálních oddílů, s třemi uzly v každé sady replik. Jeden konfigurace serveru repliky s tři uzly se také vytvoří plus dva **mongos** směrovač servery tooprovide konzistence tooapplications z napříč hello horizontálních oddílů.

* [MongoDB horizontálního dělení clusteru na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Nasazení tento cluster komplexní MongoDB horizontálně dělené vyžaduje více než 20 jádra, což je obvykle hello výchozí počet jader na oblast pro předplatné. Otevřete podpory Azure žádost tooincrease vaše počet jader.

toocreate pro toto prostředí musíte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login). Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:

```azurecli
az group create --name myResourceGroup --location eastus
```

V dalším kroku nasaďte šablonu MongoDB hello s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create). Definovat vlastní prostředek názvy a velikosti, kde je potřeba, například jako u *mongoAdminUsername*, *sizeOfDataDiskInGB*, a *configNodeVmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

Toto nasazení můžete převzít toodeploy hodinu a nakonfigurovat všechny instance virtuálních počítačů hello. Hello `--no-wait` příznak se používá na konci hello hello předcházející příkaz tooreturn řízení toohello příkazového řádku po nasazení šablony hello přijato hello platformy Azure. Potom můžete zobrazit stav nasazení hello s [az skupiny nasazení zobrazit](/cli/azure/group/deployment#show). Hello následující příklad zobrazení hello stav pro hello *myMongoDBCluster* nasazení v hello *myResourceGroup* skupiny prostředků:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Další kroky
V těchto příkladech je připojit toohello MongoDB instance místně z hello virtuálních počítačů. Pokud chcete, aby instance MongoDB toohello tooconnect z jiného virtuálního počítače nebo síť, ujistěte se, hello odpovídající [vytvoří skupinu zabezpečení sítě pravidla](nsg-quickstart.md).

Tyto příklady nasazení hello základní MongoDB prostředí pro účely vývoje. Použít možnosti konfigurace zabezpečení hello požadované pro vaše prostředí. Další informace najdete v tématu hello [MongoDB zabezpečení dokumentace](https://docs.mongodb.com/manual/security/).

Další informace o vytváření šablon najdete v tématu hello [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

šablony Azure Resource Manager Hello použijte toodownload hello rozšíření vlastních skriptů a spouštění skriptů na virtuální počítače. Další informace najdete v tématu [hello pomocí rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem](extensions-customscript.md).

