---
title: "aaaDeploy svítilna na virtuální počítač s Linuxem pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Zjistěte, jak tooinstall hello svítilna zásobníku na virtuální počítač s Linuxem v Azure"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jluk
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: NA
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: e78a82d388ce68710933b9b673aa1b2460bdbb14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-with-hello-azure-cli-10"></a><span data-ttu-id="38387-103">Nasazení svítilna zásobník hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="38387-103">Deploy LAMP stack with hello Azure CLI 1.0</span></span>
<span data-ttu-id="38387-104">Tento článek vás provede jak toodeploy platformě Apache webový server, MySQL a PHP (hello svítilna stack) v Azure.</span><span class="sxs-lookup"><span data-stu-id="38387-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on Azure.</span></span> <span data-ttu-id="38387-105">Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a hello [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) tedy [připojené tooyour účet Azure](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="38387-105">You need an Azure Account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and hello [Azure CLI](../../cli-install-nodejs.md) that is [connected tooyour Azure account](../../xplat-cli-connect.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="38387-106">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="38387-106">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="38387-107">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="38387-107">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="38387-108">[Azure CLI 1.0] – naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="38387-108">[Azure CLI 1.0] – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="38387-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello</span><span class="sxs-lookup"><span data-stu-id="38387-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

```
# One command toocreate a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* <span data-ttu-id="38387-110">Nasazení svítilna na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="38387-110">Deploy LAMP on existing VM</span></span>

```
# Two commands: one updates packages, hello other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="38387-111">Nasazení svítilna na nový virtuální počítač návod</span><span class="sxs-lookup"><span data-stu-id="38387-111">Deploy LAMP on new VM walkthrough</span></span>
<span data-ttu-id="38387-112">Můžete spustit tak, že vytvoříte [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) která bude obsahovat hello nového virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="38387-112">You can start by creating a [resource group](../../azure-resource-manager/resource-group-overview.md) that will contain hello new VM:</span></span>

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

<span data-ttu-id="38387-113">toocreate hello virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="38387-113">toocreate hello VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

<span data-ttu-id="38387-114">Byste měli vidět odpověď výzvy některé další vstupy:</span><span class="sxs-lookup"><span data-stu-id="38387-114">You should see a response prompting some more inputs:</span></span>

    info:    Executing command group deployment create
    info:    Supply values for hello following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment toocomplete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

<span data-ttu-id="38387-115">Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="38387-115">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="38387-116">Pokud chcete, můžete ověřit instalaci hello přechod dolů příliš[ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="38387-116">If you wish, you can verify hello install by jumping down too[Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="38387-117">Nasazení svítilna na existující návod virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="38387-117">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="38387-118">Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [sem toolearn jak toocreate virtuálního počítače s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="38387-118">If you need help creating a Linux VM, you can head [here toolearn how toocreate a Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="38387-119">Dále musíte tooSSH do hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="38387-119">Next, you need tooSSH into hello Linux VM.</span></span> <span data-ttu-id="38387-120">Pokud potřebujete pomoc s vytvářením klíč SSH, head [sem toolearn jak toocreate klíče SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="38387-120">If you need help with creating an SSH key, you can head [here toolearn how toocreate an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="38387-121">Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh exampleUsername@exampleDNS`.</span><span class="sxs-lookup"><span data-stu-id="38387-121">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh exampleUsername@exampleDNS`.</span></span>

<span data-ttu-id="38387-122">Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci hello svítilna zásobníku na základě Debian distribuce.</span><span class="sxs-lookup"><span data-stu-id="38387-122">Now that you are working within your Linux VM, we can walk through installing hello LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="38387-123">přesný příkazy Hello se mohou lišit pro ostatní distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="38387-123">hello exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="38387-124">Instalace na Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="38387-124">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="38387-125">Budete potřebovat následující balíčky nainstalované hello: `apache2`, `mysql-server`, `php5`, a `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="38387-125">You need hello following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="38387-126">Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel.</span><span class="sxs-lookup"><span data-stu-id="38387-126">You can install these packages by directly grabbing these packages or using Tasksel.</span></span> <span data-ttu-id="38387-127">Pokyny pro obě možnosti jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="38387-127">Instructions for both options are listed below.</span></span>
<span data-ttu-id="38387-128">Před instalací potřebovat toodownload a aktualizovat balíček seznamy.</span><span class="sxs-lookup"><span data-stu-id="38387-128">Before installing you need toodownload and update package lists.</span></span>

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a><span data-ttu-id="38387-129">Jednotlivé balíčky</span><span class="sxs-lookup"><span data-stu-id="38387-129">Individual packages</span></span>
<span data-ttu-id="38387-130">Pomocí výstižný get:</span><span class="sxs-lookup"><span data-stu-id="38387-130">Using apt-get:</span></span>

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a><span data-ttu-id="38387-131">Pomocí tasksel</span><span class="sxs-lookup"><span data-stu-id="38387-131">Using tasksel</span></span>
<span data-ttu-id="38387-132">Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="38387-132">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

<span data-ttu-id="38387-133">Po spuštění, buď z předchozích možností hello, budete mít výzvami tooinstall tyto balíčky a různé další závislosti.</span><span class="sxs-lookup"><span data-stu-id="38387-133">After running either of hello previous options, you will be prompted tooinstall these packages and various other dependencies.</span></span> <span data-ttu-id="38387-134">Stiskněte "y" a potom toocontinue, zadejte"a podle jakékoli další výzvy tooset hesla pro správu pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="38387-134">Press 'y' and then 'Enter' toocontinue, and follow any other prompts tooset an administrative password for MySQL.</span></span> <span data-ttu-id="38387-135">Tím se nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL.</span><span class="sxs-lookup"><span data-stu-id="38387-135">This installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="38387-136">Spusťte následující příkaz toosee hello další rozšíření PHP, které jsou k dispozici jako balíčky:</span><span class="sxs-lookup"><span data-stu-id="38387-136">Run hello following command toosee other PHP extensions that are available as packages:</span></span>

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a><span data-ttu-id="38387-137">Vytvoření dokumentu info.php</span><span class="sxs-lookup"><span data-stu-id="38387-137">Create info.php document</span></span>
<span data-ttu-id="38387-138">Teď byste měli být schopný toocheck Apache, MySQL a PHP, jaká verze máte prostřednictvím hello příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.</span><span class="sxs-lookup"><span data-stu-id="38387-138">You should now be able toocheck what version of Apache, MySQL, and PHP you have through hello command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="38387-139">Pokud by například tootest další, můžete vytvořit rychlé tooview PHP informace o stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="38387-139">If you would like tootest further, you can create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="38387-140">Vytvořte soubor s Nano textový editor se tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="38387-140">Create a file with Nano text editor with this command:</span></span>

    user@ubuntu$ sudo nano /var/www/html/info.php

<span data-ttu-id="38387-141">V rámci hello GNU Nano textový editor přidejte následující řádky hello:</span><span class="sxs-lookup"><span data-stu-id="38387-141">Within hello GNU Nano text editor, add hello following lines:</span></span>

    <?php
    phpinfo();
    ?>

<span data-ttu-id="38387-142">Potom uložte a zavřete hello textový editor.</span><span class="sxs-lookup"><span data-stu-id="38387-142">Then save and exit hello text editor.</span></span>

<span data-ttu-id="38387-143">Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="38387-143">Restart Apache with this command so all new installs take effect.</span></span>

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="38387-144">Ověřte svítilna úspěšně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="38387-144">Verify LAMP successfully installed</span></span>
<span data-ttu-id="38387-145">Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili otevřením prohlížeče a budete toohttp://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="38387-145">Now you can check hello PHP info page you created by opening a browser and going toohttp://youruniqueDNS/info.php.</span></span> <span data-ttu-id="38387-146">Měl by vypadat podobně jako toothis bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="38387-146">It should look similar toothis image.</span></span>

![][2]

<span data-ttu-id="38387-147">Apache instalaci můžete zkontrolovat zobrazením hello Apache2 Ubuntu výchozí stránka přechodem tooyou http://youruniqueDNS/.</span><span class="sxs-lookup"><span data-stu-id="38387-147">You can check your Apache installation by viewing hello Apache2 Ubuntu Default Page by going tooyou http://youruniqueDNS/.</span></span> <span data-ttu-id="38387-148">Měli byste vidět něco podobného jako tuto bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="38387-148">You should see something like this image.</span></span>

![][3]

<span data-ttu-id="38387-149">Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="38387-149">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="38387-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38387-150">Next steps</span></span>
<span data-ttu-id="38387-151">Projděte si hello Ubuntu dokumentace v zásobníku svítilna hello:</span><span class="sxs-lookup"><span data-stu-id="38387-151">Check out hello Ubuntu documentation on hello LAMP stack:</span></span>

* [<span data-ttu-id="38387-152">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="38387-152">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png