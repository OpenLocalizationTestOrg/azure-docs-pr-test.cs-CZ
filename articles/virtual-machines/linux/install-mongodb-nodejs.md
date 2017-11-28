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
# <a name="how-tooinstall-and-configure-mongodb-on-a-linux-vm-using-hello-azure-cli-10"></a><span data-ttu-id="8e7d2-103">Jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem pomocí hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8e7d2-103">How tooinstall and configure MongoDB on a Linux VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="8e7d2-104">[MongoDB](http://www.mongodb.org) je Oblíbené databáze NoSQL open source a vysoce výkonné.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-104">[MongoDB](http://www.mongodb.org) is a popular open-source, high-performance NoSQL database.</span></span> <span data-ttu-id="8e7d2-105">Tento článek ukazuje, jak tooinstall a konfigurace MongoDB na virtuální počítač s Linuxem v Azure pomocí modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-105">This article shows you how tooinstall and configure MongoDB on a Linux VM in Azure using hello Resource Manager deployment model.</span></span> <span data-ttu-id="8e7d2-106">Příklady jsou uvedeny této podrobnosti o tom, jak na:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-106">Examples are shown that detail how to:</span></span>

* [<span data-ttu-id="8e7d2-107">Ručně nainstalujte a nakonfigurujte základní instance MongoDB</span><span class="sxs-lookup"><span data-stu-id="8e7d2-107">Manually install and configure a basic MongoDB instance</span></span>](#manually-install-and-configure-mongodb-on-a-vm)
* [<span data-ttu-id="8e7d2-108">Vytvořte základní instance MongoDB pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8e7d2-108">Create a basic MongoDB instance using a Resource Manager template</span></span>](#create-basic-mongodb-instance-on-centos-using-a-template)
* [<span data-ttu-id="8e7d2-109">Vytvořit komplexní MongoDB horizontálně dělené clusteru s replikou nastaví pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="8e7d2-109">Create a complex MongoDB sharded cluster with replica sets using a Resource Manager template</span></span>](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="8e7d2-110">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="8e7d2-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="8e7d2-111">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="8e7d2-112">Rozhraní příkazového řádku Azure 1.0 – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="8e7d2-112">Azure CLI 1.0 – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="8e7d2-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="8e7d2-113">[Azure CLI 2.0](create-cli-complete-nodejs.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a><span data-ttu-id="8e7d2-114">Ručně nainstalujte a nakonfigurujte MongoDB na virtuálním počítači</span><span class="sxs-lookup"><span data-stu-id="8e7d2-114">Manually install and configure MongoDB on a VM</span></span>
<span data-ttu-id="8e7d2-115">MongoDB [poskytují pokyny k instalaci](https://docs.mongodb.com/manual/administration/install-on-linux/) pro distribucích systému Linux, včetně Red Hat nebo CentOS, SUSE, Ubuntu a Debian.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-115">MongoDB [provide installation instructions](https://docs.mongodb.com/manual/administration/install-on-linux/) for Linux distros including Red Hat / CentOS, SUSE, Ubuntu, and Debian.</span></span> <span data-ttu-id="8e7d2-116">Hello následující příklad vytvoří *CentOS* virtuálního počítače pomocí klíče SSH uložené v *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-116">hello following example creates a *CentOS* VM using an SSH key stored at *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="8e7d2-117">Odpověď hello vyzve k zadání názvu účtu úložiště, název DNS a přihlašovací údaje správce:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-117">Answer hello prompts for storage account name, DNS name, and admin credentials:</span></span>

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

<span data-ttu-id="8e7d2-118">Přihlaste se toohello virtuálních počítačů pomocí hello veřejnou IP adresu zobrazí na konci hello hello předcházející krok vytvoření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-118">Log on toohello VM using hello public IP address displayed at hello end of hello preceding VM creation step:</span></span>

```bash
ssh azureuser@40.78.23.145
```

<span data-ttu-id="8e7d2-119">Vytvoření zdroje instalace hello tooadd pro MongoDB, **yum** soubor úložiště následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-119">tooadd hello installation sources for MongoDB, create a **yum** repository file as follows:</span></span>

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

<span data-ttu-id="8e7d2-120">Otevřete soubor úložišti hello MongoDB pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-120">Open hello MongoDB repo file for editing.</span></span> <span data-ttu-id="8e7d2-121">Přidejte následující řádky hello:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-121">Add hello following lines:</span></span>

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

<span data-ttu-id="8e7d2-122">Nainstalujte MongoDB pomocí **yum** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-122">Install MongoDB using **yum** as follows:</span></span>

```bash
sudo yum install -y mongodb-org
```

<span data-ttu-id="8e7d2-123">Ve výchozím nastavení se SELinux vynucuje u CentOS bitové kopie, které brání v přístupu k MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-123">By default, SELinux is enforced on CentOS images that prevents you from accessing MongoDB.</span></span> <span data-ttu-id="8e7d2-124">Instalace nástroje pro správu zásad a konfigurace SELinux tooallow MongoDB toooperate na jeho výchozí port TCP 27017 následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-124">Install policy management tools and configure SELinux tooallow MongoDB toooperate on its default TCP port 27017 as follows.</span></span> 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

<span data-ttu-id="8e7d2-125">Spusťte službu MongoDB hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-125">Start hello MongoDB service as follows:</span></span>

```bash
sudo service mongod start
```

<span data-ttu-id="8e7d2-126">Ověření instalace MongoDB hello se připojíte pomocí hello místní `mongo` klienta:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-126">Verify hello MongoDB installation by connecting using hello local `mongo` client:</span></span>

```bash
mongo
```

<span data-ttu-id="8e7d2-127">Nyní testovací hello MongoDB instance přidáním některá data a pak vyhledávání:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-127">Now test hello MongoDB instance by adding some data and then searching:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

<span data-ttu-id="8e7d2-128">V případě potřeby nakonfigurujte MongoDB toostart automaticky během restartu systému:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-128">If desired, configure MongoDB toostart automatically during a system reboot:</span></span>

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a><span data-ttu-id="8e7d2-129">Vytvořte základní instance MongoDB na CentOS pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="8e7d2-129">Create basic MongoDB instance on CentOS using a template</span></span>
<span data-ttu-id="8e7d2-130">Na jednom virtuálním počítači CentOS pomocí hello následujících Azure rychlý start šablony z Githubu, můžete vytvořit základní instance MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-130">You can create a basic MongoDB instance on a single CentOS VM using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="8e7d2-131">Tato šablona používá hello rozšíření vlastních skriptů pro Linux tooadd `yum` úložiště tooyour nově vytvořený CentOS virtuálního počítače a pak nainstalujte MongoDB.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-131">This template uses hello Custom Script extension for Linux tooadd a `yum` repository tooyour newly created CentOS VM and then install MongoDB.</span></span>

* <span data-ttu-id="8e7d2-132">[Základní instance MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="8e7d2-132">[Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json</span></span>

<span data-ttu-id="8e7d2-133">Hello následující příklad vytvoří skupinu prostředků s názvem hello `myResourceGroup` v hello `eastus` oblast.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-133">hello following example creates a resource group with hello name `myResourceGroup` in hello `eastus` region.</span></span> <span data-ttu-id="8e7d2-134">Zadejte vlastní hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-134">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="8e7d2-135">Hello příkazového řádku Azure CLI vrátí jste tooa řádku během několika sekund vytváření hello nasazení, ale hello instalace a konfigurace trvá několik minut toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-135">hello Azure CLI returns you tooa prompt within a few seconds of creating hello deployment, but hello installation and configuration takes a few minutes toocomplete.</span></span> <span data-ttu-id="8e7d2-136">Zkontrolujte stav hello hello nasazení s `azure group deployment show myResourceGroup`, odpovídajícím způsobem zadávání hello název vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-136">Check hello status of hello deployment with `azure group deployment show myResourceGroup`, entering hello name of your resource group accordingly.</span></span> <span data-ttu-id="8e7d2-137">Počkejte, dokud hello **ProvisioningState** ukazuje *úspěšné* před snažíme tooSSH toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-137">Wait until hello **ProvisioningState** shows *Succeeded* before trying tooSSH toohello VM.</span></span>

<span data-ttu-id="8e7d2-138">Po nasazení hello dokončete, SSH toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-138">Once hello deployment is complete, SSH toohello VM.</span></span> <span data-ttu-id="8e7d2-139">Získat IP adresu hello vašeho virtuálního počítače pomocí hello `azure vm show` příkaz jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-139">Obtain hello IP address of your VM using hello `azure vm show` command as in hello following example:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

<span data-ttu-id="8e7d2-140">Téměř hello konec výstup hello se zobrazí hello veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-140">Near hello end of hello output, hello public IP address is displayed.</span></span> <span data-ttu-id="8e7d2-141">SSH tooyour virtuální počítač s IP adresou hello vašeho virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-141">SSH tooyour VM with hello IP address of your VM:</span></span>

```bash
ssh azureuser@138.91.149.74
```

<span data-ttu-id="8e7d2-142">Ověření instalace MongoDB hello se připojíte pomocí hello místní `mongo` klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-142">Verify hello MongoDB installation by connecting using hello local `mongo` client as follows:</span></span>

```bash
mongo
```

<span data-ttu-id="8e7d2-143">Nyní testovací hello instance přidáním některá data a hledání následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-143">Now test hello instance by adding some data and searching as follows:</span></span>

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a><span data-ttu-id="8e7d2-144">Vytvořit Cluster horizontálně dělené komplexní MongoDB na CentOS pomocí šablony</span><span class="sxs-lookup"><span data-stu-id="8e7d2-144">Create a complex MongoDB Sharded Cluster on CentOS using a template</span></span>
<span data-ttu-id="8e7d2-145">Můžete vytvořit komplexní horizontálně dělené clusteru MongoDB pomocí hello následujících Azure rychlý start šablony z Githubu.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-145">You can create a complex MongoDB sharded cluster using hello following Azure quickstart template from GitHub.</span></span> <span data-ttu-id="8e7d2-146">Tato šablona následuje hello [MongoDB horizontálně dělené clusteru osvědčené postupy](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundance a vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-146">This template follows hello [MongoDB sharded cluster best practices](https://docs.mongodb.com/manual/core/sharded-cluster-components/) tooprovide redundancy and high availability.</span></span> <span data-ttu-id="8e7d2-147">Hello šablona vytvoří dvě horizontálních oddílů, s třemi uzly v každé sady replik.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-147">hello template creates two shards, with three nodes in each replica set.</span></span> <span data-ttu-id="8e7d2-148">Jeden konfigurace serveru repliky s tři uzly se také vytvoří plus dva **mongos** směrovač servery tooprovide konzistence tooapplications z napříč hello horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-148">One config server replica set with three nodes is also created, plus two **mongos** router servers tooprovide consistency tooapplications from across hello shards.</span></span>

* <span data-ttu-id="8e7d2-149">[MongoDB horizontálního dělení clusteru na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span><span class="sxs-lookup"><span data-stu-id="8e7d2-149">[MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json</span></span>

> [!WARNING]
> <span data-ttu-id="8e7d2-150">Nasazení tento cluster komplexní MongoDB horizontálně dělené vyžaduje více než 20 jádra, což je obvykle hello výchozí počet jader na oblast pro předplatné.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-150">Deploying this complex MongoDB sharded cluster requires more than 20 cores, which is typically hello default core count per region for a subscription.</span></span> <span data-ttu-id="8e7d2-151">Otevřete podpory Azure žádost tooincrease vaše počet jader.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-151">Open an Azure support request tooincrease your core count.</span></span>

<span data-ttu-id="8e7d2-152">Hello následující příklad vytvoří skupinu prostředků s názvem hello *myResourceGroup* v hello *eastus* oblast.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-152">hello following example creates a resource group with hello name *myResourceGroup* in hello *eastus* region.</span></span> <span data-ttu-id="8e7d2-153">Zadejte vlastní hodnoty následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="8e7d2-153">Enter your own values as follows:</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> <span data-ttu-id="8e7d2-154">Hello příkazového řádku Azure CLI vrátí jste tooa řádku během několika sekund vytváření hello nasazení, ale hello instalace a konfigurace může trvat přes toocomplete hodinu.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-154">hello Azure CLI returns you tooa prompt within a few seconds of creating hello deployment, but hello installation and configuration can take over an hour toocomplete.</span></span> <span data-ttu-id="8e7d2-155">Zkontrolujte stav hello hello nasazení s `azure group deployment show myResourceGroup`, upraví hello název vaší skupiny prostředků odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-155">Check hello status of hello deployment with `azure group deployment show myResourceGroup`, adjusting hello name of your resource group accordingly.</span></span> <span data-ttu-id="8e7d2-156">Počkejte hello **ProvisioningState** ukazuje *úspěšné* před připojením toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-156">Wait until hello **ProvisioningState** shows *Succeeded* before connecting toohello VMs.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8e7d2-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e7d2-157">Next steps</span></span>
<span data-ttu-id="8e7d2-158">V těchto příkladech je připojit toohello MongoDB instance místně z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-158">In these examples, you connect toohello MongoDB instance locally from hello VM.</span></span> <span data-ttu-id="8e7d2-159">Pokud chcete, aby instance MongoDB toohello tooconnect z jiného virtuálního počítače nebo síť, ujistěte se, hello odpovídající [vytvoří skupinu zabezpečení sítě pravidla](nsg-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="8e7d2-159">If you want tooconnect toohello MongoDB instance from another VM or network, ensure hello appropriate [Network Security Group rules are created](nsg-quickstart.md).</span></span>

<span data-ttu-id="8e7d2-160">Další informace o vytváření šablon najdete v tématu hello [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8e7d2-160">For more information about creating using templates, see hello [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="8e7d2-161">šablony Azure Resource Manager Hello použijte toodownload hello rozšíření vlastních skriptů a spouštění skriptů na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="8e7d2-161">hello Azure Resource Manager templates use hello Custom Script Extension toodownload and execute scripts on your VMs.</span></span> <span data-ttu-id="8e7d2-162">Další informace najdete v tématu [hello pomocí rozšíření vlastních skriptů Azure s virtuálním počítačům s Linuxem](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="8e7d2-162">For more information, see [Using hello Azure Custom Script Extension with Linux Virtual Machines](extensions-customscript.md).</span></span>

