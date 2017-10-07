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
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm"></a><span data-ttu-id="3dd79-103">Jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="3dd79-103">How tooinstall and configure MongoDB on a Linux VM</span></span>
<span data-ttu-id="3dd79-104">[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="3dd79-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="3dd79-105">Tento článek ukazuje, jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem pomocí hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="3dd79-105">This article shows you how tooinstall and configure MongoDB on a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="3dd79-106">Můžete také provést tyto kroky hello [Azure CLI 1.0](install-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="3dd79-106">You can also perform these steps with hello [Azure CLI 1.0](install-mongodb-nodejs.md).</span></span> <span data-ttu-id="3dd79-107">Příklady jsou uvedeny této podrobnosti o tom, jak na:</span><span class="sxs-lookup"><span data-stu-id="3dd79-107">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="3dd79-108">Ručně nainstalujte a nakonfigurujte základní instance MongoDB</span><span class="sxs-lookup"><span data-stu-id="3dd79-108">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="3dd79-109">Vytvořte základní instance MongoDB pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="3dd79-109">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="3dd79-110">Vytvořit komplexní MongoDB horizontálně dělené clusteru s replikou nastaví pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="3dd79-110">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="3dd79-111">Ručně nainstalujte a nakonfigurujte MongoDB na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="3dd79-111">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="3dd79-112">MongoDB [poskytují pokyny k instalaci](https://docs.mongodb.com/manual/administration/install-on-linux/) pro distribucích systému Linux, včetně Red Hat nebo CentOS, SUSE, Ubuntu a Debian.</span><span class="sxs-lookup"><span data-stu-id="3dd79-112">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="3dd79-113">Hello následující příklad vytvoří *CentOS* virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dd79-113">hello following example creates a *CentOS* VM.</span></span> <span data-ttu-id="3dd79-114">toocreate pro toto prostředí musíte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3dd79-114">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="3dd79-115">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3dd79-115">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3dd79-116">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="3dd79-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="3dd79-117">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3dd79-117">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="3dd79-118">Hello následující příklad vytvoří virtuální počítač s názvem *Můjvp* s uživatelem s názvem *azureuser* pomocí ověření veřejného klíče SSH</span><span class="sxs-lookup"><span data-stu-id="3dd79-118">hello following example creates a VM named *myVM* with a user named *azureuser* using SSH public key authentication</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="3dd79-119">SSH toohello virtuálních počítačů pomocí vlastního uživatelského jména a hello `publicIpAddress` uvedené ve výstupu hello hello v předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="3dd79-119">SSH toohello VM using your own username and hello `publicIpAddress` listed in hello output from hello previous step:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="3dd79-120">Vytvoření zdroje instalace hello tooadd pro MongoDB, **yum** soubor úložiště následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dd79-120">tooadd hello installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="3dd79-121">Otevřete soubor úložišti hello MongoDB pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="3dd79-121">Open hello MongoDB repo file for editing.</span></span> <span data-ttu-id="3dd79-122">Přidejte následující řádky hello:</span><span class="sxs-lookup"><span data-stu-id="3dd79-122">Add hello following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="3dd79-123">Nainstalujte MongoDB pomocí **yum** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dd79-123">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="3dd79-124">Ve výchozím nastavení se SELinux vynucuje u CentOS bitové kopie, které brání v přístupu k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3dd79-124">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="3dd79-125">Instalace nástroje pro správu zásad a konfigurace SELinux tooallow MongoDB toooperate na jeho výchozí port TCP 27017 následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dd79-125">Install policy management tools and configure SELinux tooallow MongoDB toooperate on its default TCP port 27017 as follows:</span></span>

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="3dd79-126">Spusťte službu MongoDB hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dd79-126">Start hello MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="3dd79-127">Ověření instalace MongoDB hello se připojíte pomocí hello místní `mongo` klienta:</span><span class="sxs-lookup"><span data-stu-id="3dd79-127">Verify hello MongoDB installation by connecting using hello local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="3dd79-128">Nyní testovací hello MongoDB instance přidáním některá data a pak vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="3dd79-128">Now test hello MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="3dd79-129">V případě potřeby nakonfigurujte MongoDB toostart automaticky během restartu systému:</span><span class="sxs-lookup"><span data-stu-id="3dd79-129">If desired, configure MongoDB toostart automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="3dd79-130">Vytvořte základní instance MongoDB na CentOS pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="3dd79-130">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="3dd79-131">Na jednom virtuálním počítači CentOS pomocí hello následujících Azure rychlý start šablony z Githubu, můžete vytvořit základní instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3dd79-131">You can create a basic MongoDB instance on a single CentOS VM using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="3dd79-132">Tato šablona používá hello rozšíření vlastních skriptů pro Linux tooadd **yum** tooyour úložiště nově vytvořený CentOS virtuálního počítače a pak nainstalujte MongoDB.</span><span class="sxs-lookup"><span data-stu-id="3dd79-132">This template uses hello Custom Script extension for Linux tooadd a **yum** repository tooyour newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="3dd79-133">[Základní instance MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="3dd79-133">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="3dd79-134">toocreate pro toto prostředí musíte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3dd79-134">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="3dd79-135">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3dd79-135">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3dd79-136">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="3dd79-136">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="3dd79-137">V dalším kroku nasaďte šablonu MongoDB hello s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="3dd79-137">Next, deploy hello MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="3dd79-138">Definovat vlastní prostředek názvy a velikosti, kde je potřeba, například jako u *newStorageAccountName*, *virtualNetworkName*, a *vmSize*:</span><span class="sxs-lookup"><span data-stu-id="3dd79-138">Define your own resource names and sizes where needed such as for *newStorageAccountName*, *virtualNetworkName*, and *vmSize*:</span></span>

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

<span data-ttu-id="3dd79-139">Přihlaste se toohello virtuálních počítačů pomocí veřejné adresy DNS hello vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3dd79-139">Log on toohello VM using hello public DNS address of your VM.</span></span> <span data-ttu-id="3dd79-140">Můžete zobrazit hello veřejnou adresu DNS s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="3dd79-140">You can view hello public DNS address with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

<span data-ttu-id="3dd79-141">SSH tooyour virtuálních počítačů pomocí vlastního uživatelského jména a veřejnou adresu DNS:</span><span class="sxs-lookup"><span data-stu-id="3dd79-141">SSH tooyour VM using your own username and public DNS address:</span></span>

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="3dd79-142">Ověření instalace MongoDB hello se připojíte pomocí hello místní `mongo` klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dd79-142">Verify hello MongoDB installation by connecting using hello local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="3dd79-143">Nyní testovací hello instance přidáním některá data a hledání následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="3dd79-143">Now test hello instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="3dd79-144">Vytvořit Cluster horizontálně dělené komplexní MongoDB na CentOS pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="3dd79-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="3dd79-145">Můžete vytvořit komplexní horizontálně dělené clusteru MongoDB pomocí hello následujících Azure rychlý start šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="3dd79-145">You can create a complex MongoDB sharded cluster using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="3dd79-146">Tato šablona následuje hello [MongoDB horizontálně dělené clusteru osvědčené postupy](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundance a vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="3dd79-146">This template follows hello [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancy and high availability.</span></span> <span data-ttu-id="3dd79-147">Hello šablona vytvoří dvě horizontálních oddílů, s třemi uzly v každé sady replik.</span><span class="sxs-lookup"><span data-stu-id="3dd79-147">hello template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="3dd79-148">Jeden konfigurace serveru repliky s tři uzly se také vytvoří plus dva **mongos** směrovač servery tooprovide konzistence tooapplications z napříč hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="3dd79-148">One config server replica set with three nodes is also created, plus two **mongos** router servers tooprovide consistency tooapplications from across hello shards.</span></span>

* <span data-ttu-id="3dd79-149">[MongoDB horizontálního dělení clusteru na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="3dd79-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="3dd79-150">Nasazení tento cluster komplexní MongoDB horizontálně dělené vyžaduje více než 20 jádra, což je obvykle hello výchozí počet jader na oblast pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="3dd79-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically hello default core count per region for a subscription.</span></span> <span data-ttu-id="3dd79-151">Otevřete podpory Azure žádost tooincrease vaše počet jader.</span><span class="sxs-lookup"><span data-stu-id="3dd79-151">Open an Azure support request tooincrease your core count.</span></span>

<span data-ttu-id="3dd79-152">toocreate pro toto prostředí musíte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení pomocí účtu Azure tooan [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3dd79-152">toocreate this environment, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="3dd79-153">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3dd79-153">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3dd79-154">Hello následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v hello *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="3dd79-154">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="3dd79-155">V dalším kroku nasaďte šablonu MongoDB hello s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="3dd79-155">Next, deploy hello MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="3dd79-156">Definovat vlastní prostředek názvy a velikosti, kde je potřeba, například jako u *mongoAdminUsername*, *sizeOfDataDiskInGB*, a *configNodeVmSize*:</span><span class="sxs-lookup"><span data-stu-id="3dd79-156">Define your own resource names and sizes where needed such as for *mongoAdminUsername*, *sizeOfDataDiskInGB*, and *configNodeVmSize*:</span></span>

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

<span data-ttu-id="3dd79-157">Toto nasazení můžete převzít toodeploy hodinu a nakonfigurovat všechny instance virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="3dd79-157">This deployment can take over an hour toodeploy and configure all hello VM instances.</span></span> <span data-ttu-id="3dd79-158">Hello `--no-wait` příznak se používá na konci hello hello předcházející příkaz tooreturn řízení toohello příkazového řádku po nasazení šablony hello přijato hello platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="3dd79-158">hello `--no-wait` flag is used at hello end of hello preceding command tooreturn control toohello command prompt once hello template deployment has been accepted by hello Azure platform.</span></span> <span data-ttu-id="3dd79-159">Potom můžete zobrazit stav nasazení hello s [az skupiny nasazení zobrazit](/cli/azure/group/deployment#show).</span><span class="sxs-lookup"><span data-stu-id="3dd79-159">You can then view hello deployment status with [az group deployment show](/cli/azure/group/deployment#show).</span></span> <span data-ttu-id="3dd79-160">Hello následující příklad zobrazení hello stav pro hello *myMongoDBCluster* nasazení v hello *myResourceGroup* skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="3dd79-160">hello following example views hello status for hello *myMongoDBCluster* deployment in hello *myResourceGroup* resource group:</span></span>

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a><span data-ttu-id="3dd79-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3dd79-161">Next steps</span></span>
<span data-ttu-id="3dd79-162">V těchto příkladech je připojit toohello MongoDB instance místně z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3dd79-162">In these examples, you connect toohello MongoDB instance locally from hello VM.</span></span> <span data-ttu-id="3dd79-163">Pokud chcete, aby instance MongoDB toohello tooconnect z jiného virtuálního počítače nebo síť, ujistěte se, hello odpovídající [vytvoří skupinu zabezpečení sítě pravidla](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="3dd79-163">If you want tooconnect toohello MongoDB instance from another VM or network, ensure hello appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="3dd79-164">Tyto příklady nasazení hello základní MongoDB prostředí pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="3dd79-164">These examples deploy hello core MongoDB environment for development purposes.</span></span> <span data-ttu-id="3dd79-165">Použít možnosti konfigurace zabezpečení hello požadované pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="3dd79-165">Apply hello required security configuration options for your environment.</span></span> <span data-ttu-id="3dd79-166">Další informace najdete v tématu hello [MongoDB zabezpečení dokumentace](https://docs.mongodb.com/manual/security/).</span><span class="sxs-lookup"><span data-stu-id="3dd79-166">For more information, see hello [MongoDB security docs](https://docs.mongodb.com/manual/security/).</span></span>

<span data-ttu-id="3dd79-167">Další informace o vytváření šablon najdete v tématu hello [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3dd79-167">For more information about creating using templates, see hello [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="3dd79-168">šablony Azure Resource Manager Hello použijte toodownload hello rozšíření vlastních skriptů a spouštění skriptů na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3dd79-168">hello Azure Resource Manager templates use hello Custom Script Extension toodownload and execute scripts on your VMs.</span></span> <span data-ttu-id="3dd79-169">Další informace najdete v tématu [hello pomocí rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="3dd79-169">For more information, see [Using hello Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

