---
title: aaaDeploy Deis 3uzlu clusteru | Microsoft Docs
description: "Tento článek popisuje, jak toocreate uzlem 3 Deis clusteru v Azure pomocí šablony Azure Resource Manager"
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
ms.openlocfilehash: a4c0fb8cbb849264e64b433540157c9afecd184e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a><span data-ttu-id="ead2f-103">Nasaďte a nakonfigurujte 3uzlu Deis clusteru v Azure</span><span class="sxs-lookup"><span data-stu-id="ead2f-103">Deploy and configure a 3-node Deis cluster in Azure</span></span>
<span data-ttu-id="ead2f-104">Tento článek vás provede procesem zřizování [Deis](http://deis.io/) clusteru v Azure.</span><span class="sxs-lookup"><span data-stu-id="ead2f-104">This article walks you through provisioning a [Deis](http://deis.io/) cluster on Azure.</span></span> <span data-ttu-id="ead2f-105">Pokrývá všechny kroky hello z vytváření hello potřebné certifikáty toodeploying a škálování ukázku **přejděte** aplikaci na nově zřízeného clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ead2f-105">It covers all hello steps from creating hello necessary certificates toodeploying and scaling a sample **Go** application on hello newly provisioned cluster.</span></span>

<span data-ttu-id="ead2f-106">Hello následující diagram ukazuje hello architekturu systému hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="ead2f-106">hello following diagram shows hello architecture of hello deployed system.</span></span> <span data-ttu-id="ead2f-107">Spravuje správce systému hello clusteru pomocí nástrojů, jako Deis **deis** a **deisctl**.</span><span class="sxs-lookup"><span data-stu-id="ead2f-107">A system administrator manages hello cluster using Deis tools such as **deis** and **deisctl**.</span></span> <span data-ttu-id="ead2f-108">Připojení se vytvoří pomocí služby Vyrovnávání zatížení Azure, který předává hello připojení tooone hello člena uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ead2f-108">Connections are established through an Azure load balancer, which forwards hello connections tooone of hello member nodes on hello cluster.</span></span> <span data-ttu-id="ead2f-109">Hello klientům přístup nasazené aplikace prostřednictvím služby Vyrovnávání zatížení hello také.</span><span class="sxs-lookup"><span data-stu-id="ead2f-109">hello clients access deployed applications through hello load balancer as well.</span></span> <span data-ttu-id="ead2f-110">V takovém případě nástroj pro vyrovnávání zatížení hello předává hello provoz tooa Deis OK směrovač, který další routs provoz toocorresponding Docker kontejnery hostovaná v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ead2f-110">In this case, hello load balancer forwards hello traffic tooa Deis router mesh, which further routs traffic toocorresponding Docker containers hosted on hello cluster.</span></span>

  ![Diagram architektury nasazené Desis clusteru](./media/deis-cluster/architecture-overview.png)

<span data-ttu-id="ead2f-112">V pořadí toorun prostřednictvím hello následující kroky budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="ead2f-112">In order toorun through hello following steps, you'll need:</span></span>

* <span data-ttu-id="ead2f-113">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ead2f-113">An active Azure subscription.</span></span> <span data-ttu-id="ead2f-114">Pokud nemáte, můžete získat volné záznamu [azure.com](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="ead2f-114">If you don't have one, you can get a free trail on [azure.com](https://azure.microsoft.com/).</span></span>
* <span data-ttu-id="ead2f-115">Pracovní nebo školní id toouse prostředků Azure skupiny.</span><span class="sxs-lookup"><span data-stu-id="ead2f-115">A work or school id toouse Azure resource groups.</span></span> <span data-ttu-id="ead2f-116">Pokud máte osobní účet a přihlaste se pomocí id společnosti Microsoft, je třeba příliš[vytvořit id pracovní od vaše osobní](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ead2f-116">If you have a personal account and log in with a Microsoft id, you need too[create a work id from your personal one](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="ead2f-117">Buď – v závislosti na váš klientský operační systém – hello [prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs) nebo hello [Azure CLI pro Mac, Linux a Windows](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="ead2f-117">Either -- depending on your client operating system -- hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) or hello [Azure CLI for Mac, Linux, and Windows](../../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="ead2f-118">[OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="ead2f-118">[OpenSSL](https://www.openssl.org/).</span></span> <span data-ttu-id="ead2f-119">OpenSSL je použité toogenerate hello potřebné certifikáty.</span><span class="sxs-lookup"><span data-stu-id="ead2f-119">OpenSSL is used toogenerate hello necessary certificates.</span></span>
* <span data-ttu-id="ead2f-120">Git klienta [Git Bash](https://git-scm.com/).</span><span class="sxs-lookup"><span data-stu-id="ead2f-120">A Git client such as [Git Bash](https://git-scm.com/).</span></span>
* <span data-ttu-id="ead2f-121">tootest hello ukázkovou aplikaci, musíte také DNS server.</span><span class="sxs-lookup"><span data-stu-id="ead2f-121">tootest hello sample application, you'll also need a DNS server.</span></span> <span data-ttu-id="ead2f-122">Můžete použít všechny servery DNS nebo služby, které podporují záznamy se zástupným znakem A.</span><span class="sxs-lookup"><span data-stu-id="ead2f-122">You can use any DNS servers or services that support wildcard A records.</span></span>
* <span data-ttu-id="ead2f-123">Počítač toorun Deis nástrojích klienta.</span><span class="sxs-lookup"><span data-stu-id="ead2f-123">A computer toorun Deis client tools.</span></span> <span data-ttu-id="ead2f-124">Můžete použít místní počítač nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ead2f-124">You can use either a local machine or a virtual machine.</span></span> <span data-ttu-id="ead2f-125">Tyto nástroje můžete spustit na téměř jakoukoli distribuce systému Linux, ale hello následující pokyny používají Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="ead2f-125">You can run these tools on almost any Linux distribution, but hello following instructions use Ubuntu.</span></span>

## <a name="provision-hello-cluster"></a><span data-ttu-id="ead2f-126">Zřízení hello clusteru</span><span class="sxs-lookup"><span data-stu-id="ead2f-126">Provision hello cluster</span></span>
<span data-ttu-id="ead2f-127">V této části budete používat [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) šablony z úložiště s otevřeným zdrojem hello [šablon azure rychlý Start](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="ead2f-127">In this section, you'll use an [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) template from hello open source repository [azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="ead2f-128">Nejprve budete poznamenejte hello šablony.</span><span class="sxs-lookup"><span data-stu-id="ead2f-128">First, you'll copy down hello template.</span></span> <span data-ttu-id="ead2f-129">Potom vytvoříte nový pár klíčů SSH pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="ead2f-129">Then, you'll create a new SSH key pair for authentication.</span></span> <span data-ttu-id="ead2f-130">A pak budete konfigurovat nový identifikátor pro cluster můžete.</span><span class="sxs-lookup"><span data-stu-id="ead2f-130">And then, you'll configure a new identifier for you cluster.</span></span> <span data-ttu-id="ead2f-131">A nakonec použijete skript prostředí hello nebo hello prostředí PowerShell skriptu tooprovision hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ead2f-131">And finally, you'll use either hello Shell script or hello PowerShell script tooprovision hello cluster.</span></span>

1. <span data-ttu-id="ead2f-132">Klonování úložiště hello: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="ead2f-132">Clone hello repository: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).</span></span>
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. <span data-ttu-id="ead2f-133">Složka šablon přejděte toohello:</span><span class="sxs-lookup"><span data-stu-id="ead2f-133">Go toohello template folder:</span></span>
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. <span data-ttu-id="ead2f-134">Vytvořte nový pár klíčů SSH pomocí ssh-keygen:</span><span class="sxs-lookup"><span data-stu-id="ead2f-134">Create a new SSH key pair using ssh-keygen:</span></span>
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. <span data-ttu-id="ead2f-135">Vygenerujte certifikát pomocí hello výše privátního klíče:</span><span class="sxs-lookup"><span data-stu-id="ead2f-135">Generate a certificate using hello above private key:</span></span>
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file toobe generated]
5. <span data-ttu-id="ead2f-136">Přejděte příliš[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate nový token clusteru, který vypadá zhruba v tomto tvaru:</span><span class="sxs-lookup"><span data-stu-id="ead2f-136">Go too[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate a new cluster token, which looks something like:</span></span>
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   <span data-ttu-id="ead2f-137">Každý cluster CoreOS musí toohave jedinečný tokenu z této služby service úrovně free.</span><span class="sxs-lookup"><span data-stu-id="ead2f-137">Each CoreOS cluster needs toohave a unique token from this free service.</span></span> <span data-ttu-id="ead2f-138">Najdete v tématu [CoreOS dokumentace](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="ead2f-138">Please see [CoreOS documentation](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) for more details.</span></span>
6. <span data-ttu-id="ead2f-139">Upravit hello **cloudu config.yaml** souboru existující hello tooreplace **zjišťování** tokenu s hello nový token:</span><span class="sxs-lookup"><span data-stu-id="ead2f-139">Modify hello **cloud-config.yaml** file tooreplace hello existing  **discovery** token with hello new token:</span></span>
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment hello following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. <span data-ttu-id="ead2f-140">Upravit hello **azuredeploy-Parameters.JSON tímto kódem** souboru: Otevřete hello certifikátů, které jste vytvořili v kroku 4 v textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="ead2f-140">Modify hello **azuredeploy-parameters.json** file: Open hello certificate you created in step 4 in a text editor.</span></span> <span data-ttu-id="ead2f-141">Kopírovat veškerý text mezi `----BEGIN CERTIFICATE-----` a `-----END CERTIFICATE-----` do hello **sshKeyData** parametr (budete potřebovat tooremove všechny znaky nového řádku).</span><span class="sxs-lookup"><span data-stu-id="ead2f-141">Copy all text between `----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` into hello **sshKeyData** parameter (you'll need tooremove all newline characters).</span></span>
8. <span data-ttu-id="ead2f-142">Upravit hello **newStorageAccountName** parametr.</span><span class="sxs-lookup"><span data-stu-id="ead2f-142">Modify hello **newStorageAccountName** parameter.</span></span> <span data-ttu-id="ead2f-143">Toto je hello účet úložiště pro disky s operačním systémem virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ead2f-143">This is hello storage account for VM OS disks.</span></span> <span data-ttu-id="ead2f-144">Tento název účtu má toobe globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ead2f-144">This account name has toobe globally unique.</span></span>
9. <span data-ttu-id="ead2f-145">Upravit hello **publicDomainName** parametr.</span><span class="sxs-lookup"><span data-stu-id="ead2f-145">Modify hello **publicDomainName** parameter.</span></span> <span data-ttu-id="ead2f-146">To se stane součástí hello DNS název spojený s veřejnou IP adresu aplikace hello zatížení vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="ead2f-146">This will become part of hello DNS name associated with hello load balancer public IP.</span></span> <span data-ttu-id="ead2f-147">Hello konečné plně kvalifikovaný název domény bude mít formát hello *[hodnota tohoto parametru]*. *[Oblast]* . cloudapp.azure.com. Například pokud zadáte název hello jako deishbai32 a skupina prostředků hello je nasazené toohello oblast západní USA, pak hello konečné bude plně kvalifikovaný název domény pro vyrovnávání zatížení tooyour deishbai32.westus.cloudapp.azure.com.</span><span class="sxs-lookup"><span data-stu-id="ead2f-147">hello final FQDN will have hello format of *[value of this parameter]*.*[region]*.cloudapp.azure.com. For example, if you specify hello name as deishbai32, and hello resource group is deployed toohello West US region, then hello final FQDN tooyour load balancer will be deishbai32.westus.cloudapp.azure.com.</span></span>
10. <span data-ttu-id="ead2f-148">Uložte soubor parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="ead2f-148">Save hello parameter file.</span></span> <span data-ttu-id="ead2f-149">A pak můžete zřídit hello clusteru pomocí Azure PowerShell:</span><span class="sxs-lookup"><span data-stu-id="ead2f-149">And then you can provision hello cluster using Azure PowerShell:</span></span>
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    <span data-ttu-id="ead2f-150">nebo rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="ead2f-150">or Azure CLI:</span></span>
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. <span data-ttu-id="ead2f-151">Po zřízení hello skupiny prostředků uvidíte všechny hello prostředky ve skupině hello na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ead2f-151">Once hello resource group is provisioned, you can see all hello resources in hello group on Azure classic portal.</span></span> <span data-ttu-id="ead2f-152">Jak ukazuje následující snímek obrazovky, hello hello skupina prostředků obsahuje virtuální síť s tři virtuální počítače, které jsou připojené k toohello stejné skupině dostupnosti.</span><span class="sxs-lookup"><span data-stu-id="ead2f-152">As shown in hello following screenshot, hello resource group contains a virtual network with three VMs, which are joined toohello same availability set.</span></span> <span data-ttu-id="ead2f-153">Skupina Hello také obsahuje Vyrovnávání zatížení, která má přidružené veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="ead2f-153">hello group also contains a load balancer, which has an associated public IP.</span></span>
    
    ![Hello zřídit skupinu prostředků na portálu Azure classic](./media/deis-cluster/resource-group.png)

## <a name="install-hello-client"></a><span data-ttu-id="ead2f-155">Instalace klienta na hello</span><span class="sxs-lookup"><span data-stu-id="ead2f-155">Install hello client</span></span>
<span data-ttu-id="ead2f-156">Je třeba **deisctl** toocontrol vaše Deis clusteru.</span><span class="sxs-lookup"><span data-stu-id="ead2f-156">You need **deisctl** toocontrol your Deis cluster.</span></span> <span data-ttu-id="ead2f-157">I když deisctl je automaticky nainstalován ve všech uzlech clusteru hello, je deisctl toouse dobrým zvykem na samostatný počítač pro správu.</span><span class="sxs-lookup"><span data-stu-id="ead2f-157">Although deisctl is automatically installed in all hello cluster nodes, it's a good practice toouse deisctl on a separate administrative machine.</span></span> <span data-ttu-id="ead2f-158">Navíc vzhledem k tomu, že všechny uzly jsou nakonfigurované jenom privátní IP adresy, budete potřebovat toouse tunelování SSH prostřednictvím hello Vyrovnávání zatížení, která má veřejnou IP adresu, tooconnect toohello uzlu počítače.</span><span class="sxs-lookup"><span data-stu-id="ead2f-158">Furthermore, because all nodes are configured with only private IP addresses, you'll need toouse SSH tunneling through hello load balancer, which has a public IP, tooconnect toohello node machines.</span></span> <span data-ttu-id="ead2f-159">Hello následují hello kroky nastavení deisctl na samostatné Ubuntu fyzické nebo virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="ead2f-159">hello following are hello steps of setting up deisctl on a separate Ubuntu physical or virtual machine.</span></span>

1. <span data-ttu-id="ead2f-160">Instalace deisctl:mkdir deis</span><span class="sxs-lookup"><span data-stu-id="ead2f-160">Install deisctl:mkdir deis</span></span>
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. <span data-ttu-id="ead2f-161">Přidáte vaší privátní klíče toossh agenta:</span><span class="sxs-lookup"><span data-stu-id="ead2f-161">Add your private key toossh agent:</span></span>
   
        eval `ssh-agent -s`
        ssh-add [path toohello private key file, see step 1 in hello previous section]
3. <span data-ttu-id="ead2f-162">Nakonfigurujte deisctl:</span><span class="sxs-lookup"><span data-stu-id="ead2f-162">Configure deisctl:</span></span>
   
        export DEISCTL_TUNNEL=[public ip of hello load balancer]:2223

<span data-ttu-id="ead2f-163">Hello Šablona definuje příchozích pravidel NAT, které mapují 2223 tooinstance 1, 2224 tooinstance 2 a 2225 tooinstance 3.</span><span class="sxs-lookup"><span data-stu-id="ead2f-163">hello template defines inbound NAT rules that map 2223 tooinstance 1, 2224 tooinstance 2, and 2225 tooinstance 3.</span></span> <span data-ttu-id="ead2f-164">To poskytuje redundanci pro použití nástroje deisctl hello.</span><span class="sxs-lookup"><span data-stu-id="ead2f-164">This provides redundancy for using hello deisctl tool.</span></span> <span data-ttu-id="ead2f-165">Můžete zkontrolovat na portálu Azure classic tato pravidla:</span><span class="sxs-lookup"><span data-stu-id="ead2f-165">You can examine these rules on Azure classic portal:</span></span>

![Pravidla NAT u nástroje pro vyrovnávání zatížení hello](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> <span data-ttu-id="ead2f-167">Šablona hello aktuálně podporuje jenom 3 uzlu clusterů.</span><span class="sxs-lookup"><span data-stu-id="ead2f-167">Currently hello template only supports 3-node clusters.</span></span> <span data-ttu-id="ead2f-168">To je kvůli omezením šablony Azure Resource Manageru definice pravidla NAT, který nepodporuje syntaxe smyčky.</span><span class="sxs-lookup"><span data-stu-id="ead2f-168">This is because of a limitation in Azure Resource Manager template NAT rule definition, which doesn’t support loop syntax.</span></span>
> 
> 

## <a name="install-and-start-hello-deis-platform"></a><span data-ttu-id="ead2f-169">Instalace a spuštění hello Deis platformy</span><span class="sxs-lookup"><span data-stu-id="ead2f-169">Install and start hello Deis platform</span></span>
<span data-ttu-id="ead2f-170">Teď můžete použít deisctl tooinstall a začít hello Deis platformy:</span><span class="sxs-lookup"><span data-stu-id="ead2f-170">Now you can use deisctl tooinstall and start hello Deis platform:</span></span>

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path toohello private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> <span data-ttu-id="ead2f-171">Počáteční platformy hello jeho zpracování chvíli trvá (až 10 minut).</span><span class="sxs-lookup"><span data-stu-id="ead2f-171">Starting hello platform takes a while (as much as 10 minutes).</span></span> <span data-ttu-id="ead2f-172">Spouštění služby Tvůrce hello zvlášť, může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="ead2f-172">Especially, starting hello builder service can take a long time.</span></span> <span data-ttu-id="ead2f-173">A někdy trvá několik toosucceed pokusů: Pokud operace hello zdá se, že toohang, zkuste zadat `ctrl+c` toobreak provádění příkazu hello a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="ead2f-173">And sometimes it takes a few tries toosucceed: If hello operation seems toohang, try typing `ctrl+c` toobreak execution of hello command and retry.</span></span>
> 
> 

<span data-ttu-id="ead2f-174">Můžete použít `deisctl list` tooverify, pokud jsou spuštěné všechny služby:</span><span class="sxs-lookup"><span data-stu-id="ead2f-174">You can use `deisctl list` tooverify if all services are running:</span></span>

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

<span data-ttu-id="ead2f-175">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="ead2f-175">Congratulations!</span></span> <span data-ttu-id="ead2f-176">Nyní máte k dispozici spuštěný Deis clsuter v Azure!</span><span class="sxs-lookup"><span data-stu-id="ead2f-176">Now you've got a running Deis clsuter on Azure!</span></span> <span data-ttu-id="ead2f-177">Dále umožňuje nasadit ukázku, přejděte aplikace toosee hello clusteru v akci.</span><span class="sxs-lookup"><span data-stu-id="ead2f-177">Next, let's deploy a sample Go application toosee hello cluster in action.</span></span>

## <a name="deploy-and-scale-a-hello-world-application"></a><span data-ttu-id="ead2f-178">Zavádět a škálovat aplikace Hello World</span><span class="sxs-lookup"><span data-stu-id="ead2f-178">Deploy and scale a Hello World application</span></span>
<span data-ttu-id="ead2f-179">Hello následující kroky ukazují, jak toodeploy "Hello World" přejděte aplikace toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="ead2f-179">hello following steps show how toodeploy a "Hello World" Go application toohello cluster.</span></span> <span data-ttu-id="ead2f-180">Hello kroky jsou založeny na [Deis dokumentaci](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span><span class="sxs-lookup"><span data-stu-id="ead2f-180">hello steps are based on [Deis documentation](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).</span></span>

1. <span data-ttu-id="ead2f-181">Pro směrování toowork OK hello správně, budete potřebovat toohave zástupný znak A záznam pro vaši doménu odkazující toohello veřejné IP adresy služby Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="ead2f-181">For hello routing mesh toowork properly, you’ll need toohave a wildcard A record for your domain pointing toohello public IP of hello load balancer.</span></span> <span data-ttu-id="ead2f-182">Hello následující snímek obrazovky ukazuje hello A záznam pro ukázkové domény registrace na GoDaddy:</span><span class="sxs-lookup"><span data-stu-id="ead2f-182">hello following screenshot shows hello A record for a sample domain registration on GoDaddy:</span></span>
   
    ![Záznam Godaddy A](./media/deis-cluster/go-daddy.png)
   
   <p />
2. <span data-ttu-id="ead2f-184">Instalace deis:</span><span class="sxs-lookup"><span data-stu-id="ead2f-184">Install deis:</span></span>
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. <span data-ttu-id="ead2f-185">Vytvořte nový klíč SSH a poté přidejte veřejný klíč tooGitHub hello (samozřejmě můžete také znovu použít existující klíče).</span><span class="sxs-lookup"><span data-stu-id="ead2f-185">Create a new SSH key, and then add hello public key tooGitHub (of course, you can also reuse your existing keys).</span></span> <span data-ttu-id="ead2f-186">toocreate nový pár klíčů SSH, použijte:</span><span class="sxs-lookup"><span data-stu-id="ead2f-186">toocreate a new SSH key pair, use:</span></span>
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s toouse default file names and empty passcode)
4. <span data-ttu-id="ead2f-187">Přidejte id_rsa.pub nebo hello zvoleného tooGitHub veřejný klíč.</span><span class="sxs-lookup"><span data-stu-id="ead2f-187">Add id_rsa.pub, or hello public key of your choice, tooGitHub.</span></span> <span data-ttu-id="ead2f-188">Můžete to provést pomocí hello přidat SSH klíče tlačítko na vaší obrazovce konfigurace klíče SSH:</span><span class="sxs-lookup"><span data-stu-id="ead2f-188">You can do this by using hello Add SSH key button in your SSH keys configuration screen:</span></span>
   
   ![Klíč Githubu](./media/deis-cluster/github-key.png)
   
   <p />
5. <span data-ttu-id="ead2f-190">Registrace nového uživatele:</span><span class="sxs-lookup"><span data-stu-id="ead2f-190">Register a new user:</span></span>
   
        deis register http://deis.[your domain]
   <p />
6. <span data-ttu-id="ead2f-191">Přidejte klíč SSH hello:</span><span class="sxs-lookup"><span data-stu-id="ead2f-191">Add hello SSH key:</span></span>
   
        deis keys:add [path tooyour SSH public key]
   <p />      
7. <span data-ttu-id="ead2f-192">Vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="ead2f-192">Create an application.</span></span>
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p /><span data-ttu-id="ead2f-193">
8.Hello git push aktivují toobe bitové kopie Docker vytvořené a nasazení, který bude trvat několik minut.</span><span class="sxs-lookup"><span data-stu-id="ead2f-193">
8. hello git push will trigger Docker images toobe built and deployed, which will take a few minutes.</span></span> <span data-ttu-id="ead2f-194">Z mé prostředí v některých případech krok 10 (Pushing bitové kopie tooprivate úložiště) může přestat reagovat.</span><span class="sxs-lookup"><span data-stu-id="ead2f-194">From my experience, occasionally, Step 10 (Pushing image tooprivate repository) may hang.</span></span> <span data-ttu-id="ead2f-195">Pokud k tomu dojde, můžete zastavit proces hello odebrat hello aplikace pomocí ' deis aplikace: zrušení – a <application name> ` tooremove hello application and try again. You can use `deis apps:list' toofind out hello název vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ead2f-195">When this happens, you can stop hello process, remove hello application using `deis apps:destroy –a <application name>` tooremove hello application and try again. You can use `deis apps:list` toofind out hello name of your application.</span></span> <span data-ttu-id="ead2f-196">Pokud všechno funguje, byste měli vidět něco podobného jako následující hello na konci hello výstupy příkazů:</span><span class="sxs-lookup"><span data-stu-id="ead2f-196">If everything works out, you should see something like hello following at hello end of command outputs:</span></span>
   
        -----> Launching...
               done, lambda-underdog:v2 deployed tooDeis
               http://lambda-underdog.artitrack.com
               toolearn more, use `deis help` or visit http://deis.io
        toossh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. <span data-ttu-id="ead2f-197">Ověřte, zda aplikace hello funguje:</span><span class="sxs-lookup"><span data-stu-id="ead2f-197">Verify if hello application is working:</span></span>
   
        curl -S http://[your application name].[your domain]
   <span data-ttu-id="ead2f-198">Měli byste vidět tohle:</span><span class="sxs-lookup"><span data-stu-id="ead2f-198">You should see:</span></span>
   
        Welcome tooDeis!
        See hello documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list tooget hello name of your application).
   <p />
10. <span data-ttu-id="ead2f-199">Škálování instancemi too3 aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="ead2f-199">Scale hello application too3 instances:</span></span>
    
        deis scale cmd=3
    <p />
11. <span data-ttu-id="ead2f-200">Volitelně můžete deis podrobnosti tooexamine informace o vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ead2f-200">Optionally, you can use deis info tooexamine details of your application.</span></span> <span data-ttu-id="ead2f-201">Hello následující výstupy jsou z mé nasazení aplikace:</span><span class="sxs-lookup"><span data-stu-id="ead2f-201">hello following outputs are from my application deployment:</span></span>
    
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

## <a name="next-steps"></a><span data-ttu-id="ead2f-202">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ead2f-202">Next Steps</span></span>
<span data-ttu-id="ead2f-203">Tento článek vám projít všechny kroky tooprovision hello Deis nový cluster v Azure pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ead2f-203">This article walked you through all hello steps tooprovision a new Deis cluster on Azure using an Azure Resource Manager template.</span></span> <span data-ttu-id="ead2f-204">Šablona Hello podporuje redundance v tooling připojení a také Vyrovnávání zatížení pro nasazené aplikace.</span><span class="sxs-lookup"><span data-stu-id="ead2f-204">hello template supports redundancy in tooling connections as well as load balancing for deployed applications.</span></span> <span data-ttu-id="ead2f-205">Šablona Hello také nevyužívá veřejné IP adresy na uzlech člen, které šetří cenné prostředky veřejné IP adresy a poskytuje prostředí s více zabezpečené toohost aplikace.</span><span class="sxs-lookup"><span data-stu-id="ead2f-205">hello template also avoids using public IPs on member nodes, which saves precious public IP resources and provides a more secured environment toohost applications.</span></span> <span data-ttu-id="ead2f-206">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="ead2f-206">toolearn more, see hello following articles:</span></span>

<span data-ttu-id="ead2f-207">[Přehled Azure Resource Manageru][resource-group-overview]</span><span class="sxs-lookup"><span data-stu-id="ead2f-207">[Azure Resource Manager Overview][resource-group-overview]</span></span>  
<span data-ttu-id="ead2f-208">[Jak toouse hello rozhraní příkazového řádku Azure][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="ead2f-208">[How toouse hello Azure CLI][azure-command-line-tools]</span></span>  
<span data-ttu-id="ead2f-209">[Použití Azure PowerShell s Azure Resource Manager][powershell-azure-resource-manager]</span><span class="sxs-lookup"><span data-stu-id="ead2f-209">[Using Azure PowerShell with Azure Resource Manager][powershell-azure-resource-manager]</span></span>  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
