---
title: "aaaInstall MongoDB na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager hello."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 4ce21a2c63da7d00a4422e0a6766e2103e7f12d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm-using-hello-azure-cli-10"></a>Jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem pomocí hello Azure CLI 1.0
[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné. Tento článek ukazuje, jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager hello. Příklady jsou uvedeny této podrobnosti o tom, jak na:

* [Ručně nainstalujte a nakonfigurujte základní instance MongoDB](#manually-install-and-configure-mongodb-on-a-vm)
* [Vytvořte základní instance MongoDB pomocí šablony Resource Manageru](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Vytvořit komplexní MongoDB horizontálně dělené clusteru s replikou nastaví pomocí šablony Resource Manageru](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku
Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

- Rozhraní příkazového řádku Azure 1.0 – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)
- [Azure CLI 2.0](create-cli-complete-nodejs.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Ručně nainstalujte a nakonfigurujte MongoDB na virtuálním počítači
MongoDB [poskytují pokyny k instalaci](https://docs.mongodb.com/manual/administration/install-on-linux/) pro distribucích systému Linux, včetně Red Hat nebo CentOS, SUSE, Ubuntu a Debian. Hello následující příklad vytvoří *CentOS* virtuálního počítače pomocí klíče SSH uložené v *~/.ssh/id_rsa.pub*. Odpověď hello vyzve k zadání názvu účtu úložiště, název DNS a přihlašovací údaje správce:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Přihlaste se toohello virtuálních počítačů pomocí hello veřejnou IP adresu zobrazí na konci hello hello předcházející krok vytvoření virtuálního počítače:

```bash
ssh azureuser@40.78.23.145
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

Ve výchozím nastavení se SELinux vynucuje u CentOS bitové kopie, které brání v přístupu k MongoDB. Instalace nástroje pro správu zásad a konfigurace SELinux tooallow MongoDB toooperate na jeho výchozí port TCP 27017 následujícím způsobem. 

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
Na jednom virtuálním počítači CentOS pomocí hello následujících Azure rychlý start šablony z Githubu, můžete vytvořit základní instance MongoDB. Tato šablona používá hello rozšíření vlastních skriptů pro Linux tooadd `yum` úložiště tooyour nově vytvořený CentOS virtuálního počítače a pak nainstalujte MongoDB.

* [Základní instance MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Hello následující příklad vytvoří skupinu prostředků s názvem hello `myResourceGroup` v hello `eastus` oblast. Zadejte vlastní hodnoty následujícím způsobem:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> Hello příkazového řádku Azure CLI vrátí jste tooa řádku během několika sekund vytváření hello nasazení, ale hello instalace a konfigurace trvá několik minut toocomplete. Zkontrolujte stav hello hello nasazení s `azure group deployment show myResourceGroup`, odpovídajícím způsobem zadávání hello název vaší skupiny prostředků. Počkejte, dokud hello **ProvisioningState** ukazuje *úspěšné* před snažíme tooSSH toohello virtuálních počítačů.

Po nasazení hello dokončete, SSH toohello virtuálních počítačů. Získat IP adresu hello vašeho virtuálního počítače pomocí hello `azure vm show` příkaz jako hello následující ukázka:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Téměř hello konec výstup hello se zobrazí hello veřejnou IP adresu. SSH tooyour virtuální počítač s IP adresou hello vašeho virtuálního počítače:

```bash
ssh azureuser@138.91.149.74
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

Hello následující příklad vytvoří skupinu prostředků s názvem hello *myResourceGroup* v hello *eastus* oblast. Zadejte vlastní hodnoty následujícím způsobem:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> Hello příkazového řádku Azure CLI vrátí jste tooa řádku během několika sekund vytváření hello nasazení, ale hello instalace a konfigurace může trvat přes toocomplete hodinu. Zkontrolujte stav hello hello nasazení s `azure group deployment show myResourceGroup`, upraví hello název vaší skupiny prostředků odpovídajícím způsobem. Počkejte hello **ProvisioningState** ukazuje *úspěšné* před připojením toohello virtuálních počítačů.


## <a name="next-steps"></a>Další kroky
V těchto příkladech je připojit toohello MongoDB instance místně z hello virtuálních počítačů. Pokud chcete, aby instance MongoDB toohello tooconnect z jiného virtuálního počítače nebo síť, ujistěte se, hello odpovídající [vytvoří skupinu zabezpečení sítě pravidla](nsg-quickstart.md).

Další informace o vytváření šablon najdete v tématu hello [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).

šablony Azure Resource Manager Hello použijte toodownload hello rozšíření vlastních skriptů a spouštění skriptů na virtuální počítače. Další informace najdete v tématu [hello pomocí rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem](extensions-customscript.md).

