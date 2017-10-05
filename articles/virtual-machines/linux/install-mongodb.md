---
title: "Nainstalujte MongoDB na virtuální počítač s Linuxem pomocí rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak nainstalovat a nakonfigurovat MongoDB na iusing Linux virtuálního počítače Azure CLI 2.0"
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
ms.openlocfilehash: e19c09558285497f29eb78b4f4ae5b15d7f1a191
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a><span data-ttu-id="8ac00-103">Postup instalace a konfigurace MongoDB na virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="8ac00-103">How to install and configure MongoDB on a Linux VM</span></span>
<span data-ttu-id="8ac00-104">[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="8ac00-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="8ac00-105">Tento článek ukazuje, jak nainstalovat a nakonfigurovat MongoDB na virtuální počítač s Linuxem pomocí Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="8ac00-105">This article shows you how to install and configure MongoDB on a Linux VM with the Azure CLI 2.0.</span></span> <span data-ttu-id="8ac00-106">K provedení těchto kroků můžete také využít [Azure CLI 1.0](install-mongodb-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8ac00-106">You can also perform these steps with the [Azure CLI 1.0](install-mongodb-nodejs.md).</span></span> <span data-ttu-id="8ac00-107">Příklady jsou uvedeny této podrobnosti o tom, jak na:</span><span class="sxs-lookup"><span data-stu-id="8ac00-107">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="8ac00-108">Ručně nainstalujte a nakonfigurujte základní instance MongoDB</span><span class="sxs-lookup"><span data-stu-id="8ac00-108">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="8ac00-109">Vytvořte základní instance MongoDB pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8ac00-109">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="8ac00-110">Vytvořit komplexní MongoDB horizontálně dělené clusteru s replikou nastaví pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8ac00-110">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="8ac00-111">Ručně nainstalujte a nakonfigurujte MongoDB na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="8ac00-111">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="8ac00-112">MongoDB [poskytují pokyny k instalaci](https://docs.mongodb.com/manual/administration/install-on-linux/) pro distribucích systému Linux, včetně Red Hat nebo CentOS, SUSE, Ubuntu a Debian.</span><span class="sxs-lookup"><span data-stu-id="8ac00-112">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="8ac00-113">Následující příklad vytvoří *CentOS* virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8ac00-113">The following example creates a *CentOS* VM.</span></span> <span data-ttu-id="8ac00-114">K vytvoření tohoto prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="8ac00-114">To create this environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="8ac00-115">Vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="8ac00-115">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="8ac00-116">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="8ac00-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="8ac00-117">Vytvořte virtuální počítač pomocí příkazu [az vm create](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="8ac00-117">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="8ac00-118">Následující příklad vytvoří virtuální počítač s názvem *Můjvp* s uživatelem s názvem *azureuser* pomocí ověření veřejného klíče SSH</span><span class="sxs-lookup"><span data-stu-id="8ac00-118">The following example creates a VM named *myVM* with a user named *azureuser* using SSH public key authentication</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="8ac00-119">SSH k virtuálnímu počítači pomocí vlastního uživatelského jména a `publicIpAddress` uvedeného ve výstupu z předchozího kroku:</span><span class="sxs-lookup"><span data-stu-id="8ac00-119">SSH to the VM using your own username and the `publicIpAddress` listed in the output from the previous step:</span></span>

```bash
ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="8ac00-120">Chcete-li přidat zdroje instalace pro MongoDB, vytvořte **yum** soubor úložiště následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8ac00-120">To add the installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="8ac00-121">Otevřete soubor úložišti MongoDB pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="8ac00-121">Open the MongoDB repo file for editing.</span></span> <span data-ttu-id="8ac00-122">Přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="8ac00-122">Add the following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="8ac00-123">Nainstalujte MongoDB pomocí **yum** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8ac00-123">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="8ac00-124">Ve výchozím nastavení se SELinux vynucuje u CentOS bitové kopie, které brání v přístupu k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8ac00-124">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="8ac00-125">Instalace nástroje pro správu zásad a konfigurace SELinux umožňující MongoDB pracovat na jeho výchozí port TCP 27017 následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8ac00-125">Install policy management tools and configure SELinux to allow MongoDB to operate on its default TCP port 27017 as follows:</span></span>

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="8ac00-126">Spusťte službu MongoDB následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8ac00-126">Start the MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="8ac00-127">Ověření instalace MongoDB se připojíte pomocí místní `mongo` klienta:</span><span class="sxs-lookup"><span data-stu-id="8ac00-127">Verify the MongoDB installation by connecting using the local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="8ac00-128">Nyní testovací MongoDB instance přidáním některá data a pak vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="8ac00-128">Now test the MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="8ac00-129">V případě potřeby nakonfigurujte MongoDB spustit automaticky při restartu systému:</span><span class="sxs-lookup"><span data-stu-id="8ac00-129">If desired, configure MongoDB to start automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="8ac00-130">Vytvořte základní instance MongoDB na CentOS pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="8ac00-130">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="8ac00-131">Na jednom virtuálním počítači CentOS pomocí následující šablony Azure rychlý start z Githubu, můžete vytvořit základní instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8ac00-131">You can create a basic MongoDB instance on a single CentOS VM using the following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="8ac00-132">Tato šablona používá k přidání rozšíření vlastních skriptů pro Linux **yum** úložiště pro vaše nově vytvořený CentOS virtuálních počítačů a potom nainstalujte MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8ac00-132">This template uses the Custom Script extension for Linux to add a **yum** repository to your newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="8ac00-133">[Základní instance MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="8ac00-133">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="8ac00-134">K vytvoření tohoto prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="8ac00-134">To create this environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="8ac00-135">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="8ac00-135">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="8ac00-136">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="8ac00-136">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="8ac00-137">V dalším kroku nasaďte šablonu MongoDB s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="8ac00-137">Next, deploy the MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="8ac00-138">Definovat vlastní prostředek názvy a velikosti, kde je potřeba, například jako u *newStorageAccountName*, *virtualNetworkName*, a *vmSize*:</span><span class="sxs-lookup"><span data-stu-id="8ac00-138">Define your own resource names and sizes where needed such as for *newStorageAccountName*, *virtualNetworkName*, and *vmSize*:</span></span>

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

<span data-ttu-id="8ac00-139">Přihlaste se k virtuálnímu počítači pomocí veřejné adresy DNS vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8ac00-139">Log on to the VM using the public DNS address of your VM.</span></span> <span data-ttu-id="8ac00-140">Můžete zobrazit veřejnou adresu DNS s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="8ac00-140">You can view the public DNS address with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

<span data-ttu-id="8ac00-141">SSH k virtuálnímu počítači pomocí vlastního uživatelského jména a veřejnou adresu DNS:</span><span class="sxs-lookup"><span data-stu-id="8ac00-141">SSH to your VM using your own username and public DNS address:</span></span>

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

<span data-ttu-id="8ac00-142">Ověření instalace MongoDB se připojíte pomocí místní `mongo` klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8ac00-142">Verify the MongoDB installation by connecting using the local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="8ac00-143">Nyní testovací instance přidáním některá data a hledání následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8ac00-143">Now test the instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="8ac00-144">Vytvořit Cluster horizontálně dělené komplexní MongoDB na CentOS pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="8ac00-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="8ac00-145">Můžete vytvořit komplexní MongoDB horizontálně dělené clusteru pomocí následující šablony Azure rychlý start z Githubu.</span><span class="sxs-lookup"><span data-stu-id="8ac00-145">You can create a complex MongoDB sharded cluster using the following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="8ac00-146">Tato šablona odpovídá [MongoDB horizontálně dělené clusteru osvědčené postupy](https://docs.mongodb.com/manual/core/sharded-cluster-components/) zajistit redundanci a vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="8ac00-146">This template follows the [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) to provide redundancy and high availability.</span></span> <span data-ttu-id="8ac00-147">Šablona vytvoří dvě horizontálních oddílů, se tři uzly v každé sady replik.</span><span class="sxs-lookup"><span data-stu-id="8ac00-147">The template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="8ac00-148">Jeden konfigurace serveru repliky s tři uzly se také vytvoří plus dva **mongos** směrovač servery k zajištění konzistence k aplikacím z v horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="8ac00-148">One config server replica set with three nodes is also created, plus two **mongos** router servers to provide consistency to applications from across the shards.</span></span>

* <span data-ttu-id="8ac00-149">[MongoDB horizontálního dělení clusteru na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="8ac00-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="8ac00-150">Nasazení tohoto komplexní horizontálně dělené clusteru MongoDB vyžaduje víc než 20 jádra, což obvykle představuje výchozí počet jader na oblast pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="8ac00-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically the default core count per region for a subscription.</span></span> <span data-ttu-id="8ac00-151">Otevřete zvýšení počtu vaše základní požadavek podporu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ac00-151">Open an Azure support request to increase your core count.</span></span>

<span data-ttu-id="8ac00-152">K vytvoření tohoto prostředí, je třeba nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení k účtu Azure pomocí [az přihlášení](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="8ac00-152">To create this environment, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="8ac00-153">Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="8ac00-153">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="8ac00-154">Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroup* v *eastus* umístění:</span><span class="sxs-lookup"><span data-stu-id="8ac00-154">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="8ac00-155">V dalším kroku nasaďte šablonu MongoDB s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="8ac00-155">Next, deploy the MongoDB template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="8ac00-156">Definovat vlastní prostředek názvy a velikosti, kde je potřeba, například jako u *mongoAdminUsername*, *sizeOfDataDiskInGB*, a *configNodeVmSize*:</span><span class="sxs-lookup"><span data-stu-id="8ac00-156">Define your own resource names and sizes where needed such as for *mongoAdminUsername*, *sizeOfDataDiskInGB*, and *configNodeVmSize*:</span></span>

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

<span data-ttu-id="8ac00-157">Toto nasazení může trvat přes hodinu můžete nasadit a nakonfigurovat všechny instance virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8ac00-157">This deployment can take over an hour to deploy and configure all the VM instances.</span></span> <span data-ttu-id="8ac00-158">`--no-wait` Příznak se používá na konci předchozí příkaz k vrácení řízení na příkazovém řádku, jakmile nasazení šablony přijatá platformou Azure.</span><span class="sxs-lookup"><span data-stu-id="8ac00-158">The `--no-wait` flag is used at the end of the preceding command to return control to the command prompt once the template deployment has been accepted by the Azure platform.</span></span> <span data-ttu-id="8ac00-159">Potom můžete zobrazit stav nasazení s [az skupiny nasazení zobrazit](/cli/azure/group/deployment#show).</span><span class="sxs-lookup"><span data-stu-id="8ac00-159">You can then view the deployment status with [az group deployment show](/cli/azure/group/deployment#show).</span></span> <span data-ttu-id="8ac00-160">Následující příklad zobrazení stav *myMongoDBCluster* nasazení v *myResourceGroup* skupiny prostředků:</span><span class="sxs-lookup"><span data-stu-id="8ac00-160">The following example views the status for the *myMongoDBCluster* deployment in the *myResourceGroup* resource group:</span></span>

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a><span data-ttu-id="8ac00-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ac00-161">Next steps</span></span>
<span data-ttu-id="8ac00-162">V těchto příkladech můžete připojit k instanci MongoDB místně z virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8ac00-162">In these examples, you connect to the MongoDB instance locally from the VM.</span></span> <span data-ttu-id="8ac00-163">Pokud se chcete připojit k instanci MongoDB z jiného virtuálního počítače nebo sítě, zkontrolujte příslušné [vytvoří skupinu zabezpečení sítě pravidla](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="8ac00-163">If you want to connect to the MongoDB instance from another VM or network, ensure the appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="8ac00-164">Tyto příklady nasadit prostředí základní MongoDB pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="8ac00-164">These examples deploy the core MongoDB environment for development purposes.</span></span> <span data-ttu-id="8ac00-165">Použít požadované bezpečnostní možnosti konfigurace pro vaše prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ac00-165">Apply the required security configuration options for your environment.</span></span> <span data-ttu-id="8ac00-166">Další informace najdete v tématu [MongoDB zabezpečení dokumentace](https://docs.mongodb.com/manual/security/).</span><span class="sxs-lookup"><span data-stu-id="8ac00-166">For more information, see the [MongoDB security docs](https://docs.mongodb.com/manual/security/).</span></span>

<span data-ttu-id="8ac00-167">Další informace o vytváření šablon najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8ac00-167">For more information about creating using templates, see the [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="8ac00-168">Šablony Azure Resource Manager pomocí rozšíření vlastních skriptů ke stažení a spuštění skriptů na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8ac00-168">The Azure Resource Manager templates use the Custom Script Extension to download and execute scripts on your VMs.</span></span> <span data-ttu-id="8ac00-169">Další informace najdete v tématu [pomocí rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="8ac00-169">For more information, see [Using the Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

