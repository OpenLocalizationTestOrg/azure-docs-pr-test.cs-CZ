---
title: "Nasazení na virtuální počítač s Linuxem pomocí Azure CLI 1.0 svítilna | Microsoft Docs"
description: "Informace o instalaci zásobníku svítilna na virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: feba2fb20d1831e92358ff5d1b4c9589d63d28dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-with-the-azure-cli-10"></a><span data-ttu-id="89e3a-103">Nasazení svítilna zásobník 1.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="89e3a-103">Deploy LAMP stack with the Azure CLI 1.0</span></span>
<span data-ttu-id="89e3a-104">Tento článek vás provede procesem nasazení webového serveru Apache, MySQL a PHP (svítilna stack) v Azure.</span><span class="sxs-lookup"><span data-stu-id="89e3a-104">This article walks you through how to deploy an Apache web server, MySQL, and PHP (the LAMP stack) on Azure.</span></span> <span data-ttu-id="89e3a-105">Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [rozhraní příkazového řádku Azure](../../cli-install-nodejs.md) tedy [připojený ke svému účtu Azure](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="89e3a-105">You need an Azure Account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and the [Azure CLI](../../cli-install-nodejs.md) that is [connected to your Azure account](../../xplat-cli-connect.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="89e3a-106">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="89e3a-106">CLI versions to complete the task</span></span>
<span data-ttu-id="89e3a-107">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="89e3a-107">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="89e3a-108">[Azure CLI 1.0] – naše rozhraní příkazového řádku pro classic a resource správu modelech nasazení (v tomto článku)</span><span class="sxs-lookup"><span data-stu-id="89e3a-108">[Azure CLI 1.0] – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="89e3a-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="89e3a-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* <span data-ttu-id="89e3a-110">Nasazení svítilna na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="89e3a-110">Deploy LAMP on existing VM</span></span>

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="89e3a-111">Nasazení svítilna na nový virtuální počítač návod</span><span class="sxs-lookup"><span data-stu-id="89e3a-111">Deploy LAMP on new VM walkthrough</span></span>
<span data-ttu-id="89e3a-112">Můžete spustit tak, že vytvoříte [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md) která bude obsahovat nový virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="89e3a-112">You can start by creating a [resource group](../../azure-resource-manager/resource-group-overview.md) that will contain the new VM:</span></span>

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

<span data-ttu-id="89e3a-113">Pokud chcete vytvořit virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="89e3a-113">To create the VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

<span data-ttu-id="89e3a-114">Byste měli vidět odpověď výzvy některé další vstupy:</span><span class="sxs-lookup"><span data-stu-id="89e3a-114">You should see a response prompting some more inputs:</span></span>

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
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

<span data-ttu-id="89e3a-115">Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="89e3a-115">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="89e3a-116">Pokud chcete, můžete ověřit instalaci přechod dolů na [ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="89e3a-116">If you wish, you can verify the install by jumping down to [Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="89e3a-117">Nasazení svítilna na existující návod virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="89e3a-117">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="89e3a-118">Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [zde se dozvíte, jak vytvořit virtuální počítač s Linuxem](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="89e3a-118">If you need help creating a Linux VM, you can head [here to learn how to create a Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="89e3a-119">Dále je třeba SSH do virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="89e3a-119">Next, you need to SSH into the Linux VM.</span></span> <span data-ttu-id="89e3a-120">Pokud potřebujete pomoc s vytvářením klíč SSH, head [zde se dozvíte, jak vytvořit klíč SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="89e3a-120">If you need help with creating an SSH key, you can head [here to learn how to create an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="89e3a-121">Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh exampleUsername@exampleDNS`.</span><span class="sxs-lookup"><span data-stu-id="89e3a-121">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh exampleUsername@exampleDNS`.</span></span>

<span data-ttu-id="89e3a-122">Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci zásobníku svítilna na základě Debian distribuce.</span><span class="sxs-lookup"><span data-stu-id="89e3a-122">Now that you are working within your Linux VM, we can walk through installing the LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="89e3a-123">Přesný příkazy se mohou lišit pro ostatní distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="89e3a-123">The exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="89e3a-124">Instalace na Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="89e3a-124">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="89e3a-125">Budete potřebovat následující balíčky nainstalované: `apache2`, `mysql-server`, `php5`, a `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="89e3a-125">You need the following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="89e3a-126">Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel.</span><span class="sxs-lookup"><span data-stu-id="89e3a-126">You can install these packages by directly grabbing these packages or using Tasksel.</span></span> <span data-ttu-id="89e3a-127">Pokyny pro obě možnosti jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="89e3a-127">Instructions for both options are listed below.</span></span>
<span data-ttu-id="89e3a-128">Před instalací musíte stáhnout a aktualizovat balíček seznamy.</span><span class="sxs-lookup"><span data-stu-id="89e3a-128">Before installing you need to download and update package lists.</span></span>

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a><span data-ttu-id="89e3a-129">Jednotlivé balíčky</span><span class="sxs-lookup"><span data-stu-id="89e3a-129">Individual packages</span></span>
<span data-ttu-id="89e3a-130">Pomocí výstižný get:</span><span class="sxs-lookup"><span data-stu-id="89e3a-130">Using apt-get:</span></span>

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a><span data-ttu-id="89e3a-131">Pomocí tasksel</span><span class="sxs-lookup"><span data-stu-id="89e3a-131">Using tasksel</span></span>
<span data-ttu-id="89e3a-132">Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="89e3a-132">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

<span data-ttu-id="89e3a-133">Po spuštění některé z předchozích možností, zobrazí se výzva k instalaci těchto balíčků a různé další závislosti.</span><span class="sxs-lookup"><span data-stu-id="89e3a-133">After running either of the previous options, you will be prompted to install these packages and various other dependencies.</span></span> <span data-ttu-id="89e3a-134">Stisknutím tlačítka 'y' a potom "Enter, chcete pokračovat a budete postupovat podle dalších pokynů k nastavení hesla pro správu pro databázi MySQL.</span><span class="sxs-lookup"><span data-stu-id="89e3a-134">Press 'y' and then 'Enter' to continue, and follow any other prompts to set an administrative password for MySQL.</span></span> <span data-ttu-id="89e3a-135">Tím se nainstaluje minimální požadované rozšíření PHP potřeba k použití PHP s MySQL.</span><span class="sxs-lookup"><span data-stu-id="89e3a-135">This installs the minimum required PHP extensions needed to use PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="89e3a-136">Spusťte následující příkaz, který najdete v části Další rozšíření PHP, které jsou k dispozici jako balíčky:</span><span class="sxs-lookup"><span data-stu-id="89e3a-136">Run the following command to see other PHP extensions that are available as packages:</span></span>

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a><span data-ttu-id="89e3a-137">Vytvoření dokumentu info.php</span><span class="sxs-lookup"><span data-stu-id="89e3a-137">Create info.php document</span></span>
<span data-ttu-id="89e3a-138">Nyní byste měli být schopna provést kontrolu, jaká verze Apache, MySQL a PHP, máte pomocí příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.</span><span class="sxs-lookup"><span data-stu-id="89e3a-138">You should now be able to check what version of Apache, MySQL, and PHP you have through the command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="89e3a-139">Pokud chcete otestovat další, můžete vytvořit rychlé PHP stránku s informacemi o zobrazíte v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="89e3a-139">If you would like to test further, you can create a quick PHP info page to view in a browser.</span></span> <span data-ttu-id="89e3a-140">Vytvořte soubor s Nano textový editor se tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="89e3a-140">Create a file with Nano text editor with this command:</span></span>

    user@ubuntu$ sudo nano /var/www/html/info.php

<span data-ttu-id="89e3a-141">V rámci licence GNU Nano textový editor přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="89e3a-141">Within the GNU Nano text editor, add the following lines:</span></span>

    <?php
    phpinfo();
    ?>

<span data-ttu-id="89e3a-142">Potom uložte a ukončete textový editor.</span><span class="sxs-lookup"><span data-stu-id="89e3a-142">Then save and exit the text editor.</span></span>

<span data-ttu-id="89e3a-143">Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="89e3a-143">Restart Apache with this command so all new installs take effect.</span></span>

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="89e3a-144">Ověřte svítilna úspěšně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="89e3a-144">Verify LAMP successfully installed</span></span>
<span data-ttu-id="89e3a-145">Nyní můžete zkontrolovat stránku s informacemi o PHP, kterou jste vytvořili otevřením prohlížeče a přejdete na http://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="89e3a-145">Now you can check the PHP info page you created by opening a browser and going to http://youruniqueDNS/info.php.</span></span> <span data-ttu-id="89e3a-146">By měla vypadat podobně jako tento obrázek.</span><span class="sxs-lookup"><span data-stu-id="89e3a-146">It should look similar to this image.</span></span>

![][2]

<span data-ttu-id="89e3a-147">Apache instalaci můžete zkontrolovat pomocí zobrazení stránky výchozí Ubuntu Apache2 přechodem na http://youruniqueDNS/ můžete.</span><span class="sxs-lookup"><span data-stu-id="89e3a-147">You can check your Apache installation by viewing the Apache2 Ubuntu Default Page by going to you http://youruniqueDNS/.</span></span> <span data-ttu-id="89e3a-148">Měli byste vidět něco podobného jako tuto bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="89e3a-148">You should see something like this image.</span></span>

![][3]

<span data-ttu-id="89e3a-149">Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="89e3a-149">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="89e3a-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89e3a-150">Next steps</span></span>
<span data-ttu-id="89e3a-151">Projděte si dokumentaci Ubuntu v zásobníku svítilna:</span><span class="sxs-lookup"><span data-stu-id="89e3a-151">Check out the Ubuntu documentation on the LAMP stack:</span></span>

* [<span data-ttu-id="89e3a-152">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="89e3a-152">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png