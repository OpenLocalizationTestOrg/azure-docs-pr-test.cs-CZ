---
title: "Nasazení uzlem 3 Deis clusteru | Microsoft Docs"
description: "Tento článek popisuje, jak vytvořit 3uzel Deis clusteru v Azure pomocí šablony Azure Resource Manager"
services: virtual-machines-linux
documentationcenter: 
author: HaishiBai
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5eb67eb7-95d4-461d-8eac-44925224ba5f
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/24/2015
ms.author: hbai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a0c3dd7562dfb5ce54c2ebfd4665109f59cd8fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a><span data-ttu-id="6961c-103">Nasaďte a nakonfigurujte 3uzlu Deis clusteru v Azure</span><span class="sxs-lookup"><span data-stu-id="6961c-103">Deploy and configure a 3-node Deis cluster in Azure</span></span>
<span data-ttu-id="6961c-104">Tento článek vás provede procesem zřizování [Deis](http://deis.io/) clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="6961c-104">This article walks you through provisioning a [Deis](http://deis.io/) cluster on Azure.</span></span> <span data-ttu-id="6961c-105">Pokrývá všechny kroky ve vytváření potřebné certifikáty pro nasazování a škálování ukázku **přejděte** aplikaci na nově zřízeného clusteru.</span><span class="sxs-lookup"><span data-stu-id="6961c-105">It covers all the steps from creating the necessary certificates to deploying and scaling a sample **Go** application on the newly provisioned cluster.</span></span>

<span data-ttu-id="6961c-106">Následující diagram znázorňuje architekturu nasazený systém.</span><span class="sxs-lookup"><span data-stu-id="6961c-106">The following diagram shows the architecture of the deployed system.</span></span> <span data-ttu-id="6961c-107">Spravuje správce systému na clusteru pomocí nástrojů, jako Deis **deis** a **deisctl**.</span><span class="sxs-lookup"><span data-stu-id="6961c-107">A system administrator manages the cluster using Deis tools such as **deis** and **deisctl**.</span></span> <span data-ttu-id="6961c-108">Připojení se vytvoří pomocí služby Vyrovnávání zatížení Azure, který předává připojení k jednomu členu uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6961c-108">Connections are established through an Azure load balancer, which forwards the connections to one of the member nodes on the cluster.</span></span> <span data-ttu-id="6961c-109">Klienti přístup nasazené aplikace prostřednictvím také Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6961c-109">The clients access deployed applications through the load balancer as well.</span></span> <span data-ttu-id="6961c-110">V takovém případě předá nástroje pro vyrovnávání zatížení provozu, který Deis OK směrovač, který další routs provoz na odpovídající Docker kontejnery hostovaných v daném clusteru.</span><span class="sxs-lookup"><span data-stu-id="6961c-110">In this case, the load balancer forwards the traffic to a Deis router mesh, which further routs traffic to corresponding Docker containers hosted on the cluster.</span></span>

  ![Diagram architektury nasazené Desis clusteru](./media/deis-cluster/architecture-overview.png)

<span data-ttu-id="6961c-112">Aby bylo možné spustit následující kroky projdete, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="6961c-112">In order to run through the following steps, you'll need:</span></span>

* <span data-ttu-id="6961c-113">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="6961c-113">An active Azure subscription.</span></span> <span data-ttu-id="6961c-114">Pokud nemáte, můžete získat volné záznamu [azure.com](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="6961c-114">If you don't have one, you can get a free trail on [azure.com](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="6961c-115">Id pracovní nebo školní použití skupin prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="6961c-115">A work or school id to use Azure resource groups.</span></span> <span data-ttu-id="6961c-116">Pokud máte osobní účet a přihlaste se pomocí id společnosti Microsoft, budete muset [vytvořit id pracovní od vaše osobní](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6961c-116">If you have a personal account and log in with a Microsoft id, you need to [create a work id from your personal one](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="6961c-117">Buď – v závislosti na váš klientský operační systém – [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo [Azure CLI pro Mac, Linux a Windows](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6961c-117">Either -- depending on your client operating system -- the [Azure PowerShell](/powershell/azureps-cmdlets-docs) or the [Azure CLI for Mac, Linux, and Windows](../../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="6961c-118">[OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="6961c-118">[OpenSSL](https://www.openssl.org/).</span></span> <span data-ttu-id="6961c-119">OpenSSL slouží ke generování potřebné certifikáty.</span><span class="sxs-lookup"><span data-stu-id="6961c-119">OpenSSL is used to generate the necessary certificates.</span></span>
* <span data-ttu-id="6961c-120">Git klienta [Git Bash](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="6961c-120">A Git client such as [Git Bash](https://git-scm.com/).</span></span>
* <span data-ttu-id="6961c-121">Chcete-li otestovat vzorovou aplikaci, musíte také DNS server.</span><span class="sxs-lookup"><span data-stu-id="6961c-121">To test the sample application, you'll also need a DNS server.</span></span> <span data-ttu-id="6961c-122">Můžete použít všechny servery DNS nebo služby, které podporují záznamy se zástupným znakem A.</span><span class="sxs-lookup"><span data-stu-id="6961c-122">You can use any DNS servers or services that support wildcard A records.</span></span>
* <span data-ttu-id="6961c-123">Počítače ke spuštění Deis nástrojích klienta.</span><span class="sxs-lookup"><span data-stu-id="6961c-123">A computer to run Deis client tools.</span></span> <span data-ttu-id="6961c-124">Můžete použít místní počítač nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6961c-124">You can use either a local machine or a virtual machine.</span></span> <span data-ttu-id="6961c-125">Tyto nástroje můžete spustit na téměř jakoukoli distribuce systému Linux, ale Ubuntu použijte následující pokyny.</span><span class="sxs-lookup"><span data-stu-id="6961c-125">You can run these tools on almost any Linux distribution, but the following instructions use Ubuntu.</span></span>

## <a name="provision-the-cluster"></a><span data-ttu-id="6961c-126">Zřízení clusteru</span><span class="sxs-lookup"><span data-stu-id="6961c-126">Provision the cluster</span></span>
<span data-ttu-id="6961c-127">V této části budete používat [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) šablony z úložiště s otevřeným zdrojem [šablon azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="6961c-127">In this section, you'll use an [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) template from the open source repository [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="6961c-128">Nejprve budete poznamenejte šablony.</span><span class="sxs-lookup"><span data-stu-id="6961c-128">First, you'll copy down the template.</span></span> <span data-ttu-id="6961c-129">Potom vytvoříte nový pár klíčů SSH pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="6961c-129">Then, you'll create a new SSH key pair for authentication.</span></span> <span data-ttu-id="6961c-130">A pak budete konfigurovat nový identifikátor pro cluster můžete.</span><span class="sxs-lookup"><span data-stu-id="6961c-130">And then, you'll configure a new identifier for you cluster.</span></span> <span data-ttu-id="6961c-131">A nakonec ke zřízení clusteru použijete skript prostředí nebo skript PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6961c-131">And finally, you'll use either the Shell script or the PowerShell script to provision the cluster.</span></span>

1. <span data-ttu-id="6961c-132">Naklonujte úložiště: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="6961c-132">Clone the repository: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. <span data-ttu-id="6961c-133">Přejděte do složky šablony:</span><span class="sxs-lookup"><span data-stu-id="6961c-133">Go to the template folder:</span></span>
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. <span data-ttu-id="6961c-134">Vytvořte nový pár klíčů SSH pomocí ssh-keygen:</span><span class="sxs-lookup"><span data-stu-id="6961c-134">Create a new SSH key pair using ssh-keygen:</span></span>
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. <span data-ttu-id="6961c-135">Vygenerujte certifikát pomocí výše uvedených privátního klíče:</span><span class="sxs-lookup"><span data-stu-id="6961c-135">Generate a certificate using the above private key:</span></span>
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]
5. <span data-ttu-id="6961c-136">Přejděte na [https://discovery.etcd.io/new](https://discovery.etcd.io/new) vygenerovat nový token clusteru, který vypadá zhruba v tomto tvaru:</span><span class="sxs-lookup"><span data-stu-id="6961c-136">Go to [https://discovery.etcd.io/new](https://discovery.etcd.io/new) to generate a new cluster token, which looks something like:</span></span>
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   <span data-ttu-id="6961c-137">Každý cluster CoreOS musí mít jedinečné tokenu z této služby service úrovně free.</span><span class="sxs-lookup"><span data-stu-id="6961c-137">Each CoreOS cluster needs to have a unique token from this free service.</span></span> <span data-ttu-id="6961c-138">Najdete v tématu [CoreOS dokumentace](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="6961c-138">Please see [CoreOS documentation](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) for more details.</span></span>
6. <span data-ttu-id="6961c-139">Změnit **cloudu config.yaml** soubor nahradit stávající **zjišťování** tokenu s nový token:</span><span class="sxs-lookup"><span data-stu-id="6961c-139">Modify the **cloud-config.yaml** file to replace the existing  **discovery** token with the new token:</span></span>
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. <span data-ttu-id="6961c-140">Změnit **azuredeploy-Parameters.JSON tímto kódem** souboru: Otevřete certifikát, který jste vytvořili v kroku 4 v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="6961c-140">Modify the **azuredeploy-parameters.json** file: Open the certificate you created in step 4 in a text editor.</span></span> <span data-ttu-id="6961c-141">Kopírovat veškerý text mezi `----BEGIN CERTIFICATE-----` a `-----END CERTIFICATE-----` do **sshKeyData** parametr (budete muset odebrat všechny znaky nového řádku).</span><span class="sxs-lookup"><span data-stu-id="6961c-141">Copy all text between `----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` into the **sshKeyData** parameter (you'll need to remove all newline characters).</span></span>
8. <span data-ttu-id="6961c-142">Změnit **newStorageAccountName** parametr.</span><span class="sxs-lookup"><span data-stu-id="6961c-142">Modify the **newStorageAccountName** parameter.</span></span> <span data-ttu-id="6961c-143">Toto je účet úložiště pro disky s operačním systémem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6961c-143">This is the storage account for VM OS disks.</span></span> <span data-ttu-id="6961c-144">Tento název účtu musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6961c-144">This account name has to be globally unique.</span></span>
9. <span data-ttu-id="6961c-145">Změnit **publicDomainName** parametr.</span><span class="sxs-lookup"><span data-stu-id="6961c-145">Modify the **publicDomainName** parameter.</span></span> <span data-ttu-id="6961c-146">To se stane součástí DNS název spojený s veřejnou IP nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6961c-146">This will become part of the DNS name associated with the load balancer public IP.</span></span> <span data-ttu-id="6961c-147">Poslední plně kvalifikovaný název domény, bude mít formát *[hodnota tohoto parametru]*. *[Oblast]* . cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="6961c-147">The final FQDN will have the format of *[value of this parameter]*.*[region]*.cloudapp.azure.com.</span></span> <span data-ttu-id="6961c-148">Například pokud zadáte název jako deishbai32 a skupině prostředků se nasadí pro oblast západní USA, potom konečné plně kvalifikovaný název domény pro nástroj pro vyrovnávání zatížení bude deishbai32.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="6961c-148">For example, if you specify the name as deishbai32, and the resource group is deployed to the West US region, then the final FQDN to your load balancer will be deishbai32.westus.cloudapp.azure.com.</span></span>
10. <span data-ttu-id="6961c-149">Uložte soubor parametrů.</span><span class="sxs-lookup"><span data-stu-id="6961c-149">Save the parameter file.</span></span> <span data-ttu-id="6961c-150">A pak můžete zřídit cluster pomocí prostředí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6961c-150">And then you can provision the cluster using Azure PowerShell:</span></span>
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    <span data-ttu-id="6961c-151">nebo rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="6961c-151">or Azure CLI:</span></span>
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. <span data-ttu-id="6961c-152">Po zřízení skupiny prostředků uvidíte na portálu Azure classic všechny prostředky ve skupině.</span><span class="sxs-lookup"><span data-stu-id="6961c-152">Once the resource group is provisioned, you can see all the resources in the group on Azure classic portal.</span></span> <span data-ttu-id="6961c-153">Jak je znázorněno na následujícím snímku obrazovky, skupina prostředků obsahuje virtuální síť s tři virtuální počítače, které jsou připojeny ke stejné sadě dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="6961c-153">As shown in the following screenshot, the resource group contains a virtual network with three VMs, which are joined to the same availability set.</span></span> <span data-ttu-id="6961c-154">Tato skupina obsahuje také Vyrovnávání zatížení, která má přidružené veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="6961c-154">The group also contains a load balancer, which has an associated public IP.</span></span>
    
    ![Skupina zřízené prostředků na portálu Azure classic](./media/deis-cluster/resource-group.png)

## <a name="install-the-client"></a><span data-ttu-id="6961c-156">Instalace klienta</span><span class="sxs-lookup"><span data-stu-id="6961c-156">Install the client</span></span>
<span data-ttu-id="6961c-157">Je třeba **deisctl** na ovládací prvek vaší Deis clusteru.</span><span class="sxs-lookup"><span data-stu-id="6961c-157">You need **deisctl** to control your Deis cluster.</span></span> <span data-ttu-id="6961c-158">I když deisctl je automaticky nainstalován ve všech uzlech clusteru, je vhodné použít deisctl na samostatný počítač pro správu.</span><span class="sxs-lookup"><span data-stu-id="6961c-158">Although deisctl is automatically installed in all the cluster nodes, it's a good practice to use deisctl on a separate administrative machine.</span></span> <span data-ttu-id="6961c-159">Navíc vzhledem k tomu, že všechny uzly jsou nakonfigurované jenom privátních IP adres, musíte použít tunelování prostřednictvím Vyrovnávání zatížení, která má veřejnou IP adresu pro připojení k počítačům uzlu SSH.</span><span class="sxs-lookup"><span data-stu-id="6961c-159">Furthermore, because all nodes are configured with only private IP addresses, you'll need to use SSH tunneling through the load balancer, which has a public IP, to connect to the node machines.</span></span> <span data-ttu-id="6961c-160">Následující kroky nastavení deisctl v samostatné Ubuntu fyzického nebo virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6961c-160">The following are the steps of setting up deisctl on a separate Ubuntu physical or virtual machine.</span></span>

1. <span data-ttu-id="6961c-161">Instalace deisctl:mkdir deis</span><span class="sxs-lookup"><span data-stu-id="6961c-161">Install deisctl:mkdir deis</span></span>
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. <span data-ttu-id="6961c-162">Přidejte svůj privátní klíč pro ssh agenta:</span><span class="sxs-lookup"><span data-stu-id="6961c-162">Add your private key to ssh agent:</span></span>
   
        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]
3. <span data-ttu-id="6961c-163">Nakonfigurujte deisctl:</span><span class="sxs-lookup"><span data-stu-id="6961c-163">Configure deisctl:</span></span>
   
        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

<span data-ttu-id="6961c-164">Šablona definuje příchozích pravidel NAT, které mapují 2223 do instance 1, 2224 instanci 2 a 2225 instanci 3.</span><span class="sxs-lookup"><span data-stu-id="6961c-164">The template defines inbound NAT rules that map 2223 to instance 1, 2224 to instance 2, and 2225 to instance 3.</span></span> <span data-ttu-id="6961c-165">Toto použití nástroje deisctl poskytuje redundanci.</span><span class="sxs-lookup"><span data-stu-id="6961c-165">This provides redundancy for using the deisctl tool.</span></span> <span data-ttu-id="6961c-166">Můžete zkontrolovat na portálu Azure classic tato pravidla:</span><span class="sxs-lookup"><span data-stu-id="6961c-166">You can examine these rules on Azure classic portal:</span></span>

![Pravidla NAT u nástroje pro vyrovnávání zatížení](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> <span data-ttu-id="6961c-168">Šablona aktuálně podporuje jenom 3 uzlu clusterů.</span><span class="sxs-lookup"><span data-stu-id="6961c-168">Currently the template only supports 3-node clusters.</span></span> <span data-ttu-id="6961c-169">To je kvůli omezením šablony Azure Resource Manageru definice pravidla NAT, který nepodporuje syntaxe smyčky.</span><span class="sxs-lookup"><span data-stu-id="6961c-169">This is because of a limitation in Azure Resource Manager template NAT rule definition, which doesn’t support loop syntax.</span></span>
> 
> 

## <a name="install-and-start-the-deis-platform"></a><span data-ttu-id="6961c-170">Instalace a spuštění Deis platformy</span><span class="sxs-lookup"><span data-stu-id="6961c-170">Install and start the Deis platform</span></span>
<span data-ttu-id="6961c-171">Teď můžete použít k instalaci a spuštění deisctl Deis platformy:</span><span class="sxs-lookup"><span data-stu-id="6961c-171">Now you can use deisctl to install and start the Deis platform:</span></span>

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> <span data-ttu-id="6961c-172">Spouštění platformou jeho zpracování chvíli trvá (až 10 minut).</span><span class="sxs-lookup"><span data-stu-id="6961c-172">Starting the platform takes a while (as much as 10 minutes).</span></span> <span data-ttu-id="6961c-173">Spouštění služby Tvůrce zvlášť, může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="6961c-173">Especially, starting the builder service can take a long time.</span></span> <span data-ttu-id="6961c-174">A někdy trvá několik pokusí úspěšné: Pokud operaci zdá se, že zablokuje, zkuste zadat `ctrl+c` rozdělit provedení příkazu, a opakujte.</span><span class="sxs-lookup"><span data-stu-id="6961c-174">And sometimes it takes a few tries to succeed: If the operation seems to hang, try typing `ctrl+c` to break execution of the command and retry.</span></span>
> 
> 

<span data-ttu-id="6961c-175">Můžete použít `deisctl list` ověřit, zda jsou spuštěny všechny služby:</span><span class="sxs-lookup"><span data-stu-id="6961c-175">You can use `deisctl list` to verify if all services are running:</span></span>

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

<span data-ttu-id="6961c-176">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="6961c-176">Congratulations!</span></span> <span data-ttu-id="6961c-177">Nyní máte k dispozici spuštěný Deis clsuter v Azure!</span><span class="sxs-lookup"><span data-stu-id="6961c-177">Now you've got a running Deis clsuter on Azure!</span></span> <span data-ttu-id="6961c-178">Dále umožňuje nasadit ukázku, přejděte aplikace, abyste viděli clusteru v akci.</span><span class="sxs-lookup"><span data-stu-id="6961c-178">Next, let's deploy a sample Go application to see the cluster in action.</span></span>

## <a name="deploy-and-scale-a-hello-world-application"></a><span data-ttu-id="6961c-179">Zavádět a škálovat aplikace Hello World</span><span class="sxs-lookup"><span data-stu-id="6961c-179">Deploy and scale a Hello World application</span></span>
<span data-ttu-id="6961c-180">Následující kroky ukazují, jak nasadit "Hello, World" přejděte aplikací do clusteru.</span><span class="sxs-lookup"><span data-stu-id="6961c-180">The following steps show how to deploy a "Hello World" Go application to the cluster.</span></span> <span data-ttu-id="6961c-181">Kroky jsou založené na [Deis dokumentaci](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span><span class="sxs-lookup"><span data-stu-id="6961c-181">The steps are based on [Deis documentation](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span></span>

1. <span data-ttu-id="6961c-182">Pro směrování síť fungovalo správně budete muset mít zástupný znak A záznam pro vaši doménu odkazující na veřejné IP adresy služby Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="6961c-182">For the routing mesh to work properly, you’ll need to have a wildcard A record for your domain pointing to the public IP of the load balancer.</span></span> <span data-ttu-id="6961c-183">Následující snímek obrazovky ukazuje záznamu A pro ukázkové domény registrace na GoDaddy:</span><span class="sxs-lookup"><span data-stu-id="6961c-183">The following screenshot shows the A record for a sample domain registration on GoDaddy:</span></span>
   
    ![Záznam Godaddy A](./media/deis-cluster/go-daddy.png)
   
   <p />
2. <span data-ttu-id="6961c-185">Instalace deis:</span><span class="sxs-lookup"><span data-stu-id="6961c-185">Install deis:</span></span>
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. <span data-ttu-id="6961c-186">Vytvořte nový klíč SSH a pak přidejte veřejný klíč do GitHub (samozřejmě můžete také znovu použít existující klíče).</span><span class="sxs-lookup"><span data-stu-id="6961c-186">Create a new SSH key, and then add the public key to GitHub (of course, you can also reuse your existing keys).</span></span> <span data-ttu-id="6961c-187">Pokud chcete vytvořit nový pár klíčů SSH, použijte:</span><span class="sxs-lookup"><span data-stu-id="6961c-187">To create a new SSH key pair, use:</span></span>
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)
4. <span data-ttu-id="6961c-188">Přidejte id_rsa.pub nebo veřejný klíč podle svého výběru na Githubu.</span><span class="sxs-lookup"><span data-stu-id="6961c-188">Add id_rsa.pub, or the public key of your choice, to GitHub.</span></span> <span data-ttu-id="6961c-189">Můžete to provést pomocí přidat SSH klíče tlačítko na vaší obrazovce konfigurace klíče SSH:</span><span class="sxs-lookup"><span data-stu-id="6961c-189">You can do this by using the Add SSH key button in your SSH keys configuration screen:</span></span>
   
   ![Klíč Githubu](./media/deis-cluster/github-key.png)
   
   <p />
5. <span data-ttu-id="6961c-191">Registrace nového uživatele:</span><span class="sxs-lookup"><span data-stu-id="6961c-191">Register a new user:</span></span>
   
        deis register http://deis.[your domain]
   <p />
6. <span data-ttu-id="6961c-192">Přidejte klíč SSH:</span><span class="sxs-lookup"><span data-stu-id="6961c-192">Add the SSH key:</span></span>
   
        deis keys:add [path to your SSH public key]
   <p />      
7. <span data-ttu-id="6961c-193">Vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="6961c-193">Create an application.</span></span>
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p /><span data-ttu-id="6961c-194">
8.Git push aktivují imagí Dockeru vytvořená a nasazená, které bude trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="6961c-194">
8. The git push will trigger Docker images to be built and deployed, which will take a few minutes.</span></span> <span data-ttu-id="6961c-195">Z mé prostředí v některých případech krok 10 (Pushing bitové kopie do privátní úložiště) může přestat reagovat.</span><span class="sxs-lookup"><span data-stu-id="6961c-195">From my experience, occasionally, Step 10 (Pushing image to private repository) may hang.</span></span> <span data-ttu-id="6961c-196">V takovém případě můžete zastavit proces, odeberte aplikace pomocí ' deis aplikace: zničit – a <application name> ` to remove the application and try again. You can use `deis apps:list' Chcete-li zjistit název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="6961c-196">When this happens, you can stop the process, remove the application using `deis apps:destroy –a <application name>` to remove the application and try again. You can use `deis apps:list` to find out the name of your application.</span></span> <span data-ttu-id="6961c-197">Pokud všechno funguje, byste měli vidět něco podobného jako následující na konci výstupy příkazů:</span><span class="sxs-lookup"><span data-stu-id="6961c-197">If everything works out, you should see something like the following at the end of command outputs:</span></span>
   
        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. <span data-ttu-id="6961c-198">Ověřte, zda aplikace funguje:</span><span class="sxs-lookup"><span data-stu-id="6961c-198">Verify if the application is working:</span></span>
   
        curl -S http://[your application name].[your domain]
   <span data-ttu-id="6961c-199">Měli byste vidět tohle:</span><span class="sxs-lookup"><span data-stu-id="6961c-199">You should see:</span></span>
   
        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
   <p />
10. <span data-ttu-id="6961c-200">Škálování aplikace na 3 instancí:</span><span class="sxs-lookup"><span data-stu-id="6961c-200">Scale the application to 3 instances:</span></span>
    
        deis scale cmd=3
    <p />
11. <span data-ttu-id="6961c-201">Volitelně můžete použít deis údaje a zkontrolujte podrobnosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6961c-201">Optionally, you can use deis info to examine details of your application.</span></span> <span data-ttu-id="6961c-202">Jsou následující výstupy z nasazení Moje aplikace:</span><span class="sxs-lookup"><span data-stu-id="6961c-202">The following outputs are from my application deployment:</span></span>
    
        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }
    
        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)
    
        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a><span data-ttu-id="6961c-203">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6961c-203">Next Steps</span></span>
<span data-ttu-id="6961c-204">Tento článek vám projít všechny kroky pro zřízení nového Deis clusteru v Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6961c-204">This article walked you through all the steps to provision a new Deis cluster on Azure using an Azure Resource Manager template.</span></span> <span data-ttu-id="6961c-205">Šablona podporuje redundance v tooling připojení a také Vyrovnávání zatížení pro nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="6961c-205">The template supports redundancy in tooling connections as well as load balancing for deployed applications.</span></span> <span data-ttu-id="6961c-206">Šablona taky nevyužívá veřejné IP adresy na uzlech člen, které šetří cenné prostředky veřejné IP adresy a poskytuje prostředí s více zabezpečené hostování aplikací.</span><span class="sxs-lookup"><span data-stu-id="6961c-206">The template also avoids using public IPs on member nodes, which saves precious public IP resources and provides a more secured environment to host applications.</span></span> <span data-ttu-id="6961c-207">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="6961c-207">To learn more, see the following articles:</span></span>

<span data-ttu-id="6961c-208">[Přehled Azure Resource Manageru][resource-group-overview]</span><span class="sxs-lookup"><span data-stu-id="6961c-208">[Azure Resource Manager Overview][resource-group-overview]</span></span>  
<span data-ttu-id="6961c-209">[Jak používat rozhraní příkazového řádku Azure][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="6961c-209">[How to use the Azure CLI][azure-command-line-tools]</span></span>  
<span data-ttu-id="6961c-210">[Použití Azure PowerShell s Azure Resource Manager][powershell-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="6961c-210">[Using Azure PowerShell with Azure Resource Manager][powershell-azure-resource-manager]</span></span>  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
