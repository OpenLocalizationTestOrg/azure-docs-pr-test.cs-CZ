---
title: "aaaDeploy svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: 42d887bb9f78becc02505e336be25fdaaf78df70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-on-azure"></a><span data-ttu-id="4979e-103">Nasazení svítilna zásobníku v Azure</span><span class="sxs-lookup"><span data-stu-id="4979e-103">Deploy LAMP stack on Azure</span></span>
<span data-ttu-id="4979e-104">Tento článek vás provede jak toodeploy platformě Apache webový server, MySQL a PHP (hello svítilna stack) v Azure.</span><span class="sxs-lookup"><span data-stu-id="4979e-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on Azure.</span></span> <span data-ttu-id="4979e-105">Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="4979e-105">You need an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span> <span data-ttu-id="4979e-106">Můžete také provést tyto kroky hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4979e-106">You can also perform these steps with hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="4979e-107">Rychlý příkaz souhrn</span><span class="sxs-lookup"><span data-stu-id="4979e-107">Quick command summary</span></span>

1. <span data-ttu-id="4979e-108">Ukládání a upravování hello [azuredeploy.parameters.json soubor](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour předvoleb na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4979e-108">Save and edit hello [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour preference on your local machine.</span></span>
2. <span data-ttu-id="4979e-109">Spusťte následující dva příkazy toocreate hello skupinu prostředků a potom nasazení vaší šablony:</span><span class="sxs-lookup"><span data-stu-id="4979e-109">Run hello following two commands toocreate a resource group and then deploy your template:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a><span data-ttu-id="4979e-110">Nasazení svítilna na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4979e-110">Deploy LAMP on existing VM</span></span>
<span data-ttu-id="4979e-111">Hello následující příkazy balíčky aktualizací a poté nainstaluje Apache, MySQL a PHP:</span><span class="sxs-lookup"><span data-stu-id="4979e-111">hello following commands updates packages, then installs Apache, MySQL, and PHP:</span></span>

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="4979e-112">Nasazení svítilna na nový virtuální počítač návod</span><span class="sxs-lookup"><span data-stu-id="4979e-112">Deploy LAMP on new VM walkthrough</span></span>

1. <span data-ttu-id="4979e-113">Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) toocontain hello nového virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="4979e-113">Create a resource group with [az group create](/cli/azure/group#create) toocontain hello new VM:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
```
<span data-ttu-id="4979e-114">toocreate hello virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="4979e-114">toocreate hello VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

2. <span data-ttu-id="4979e-115">Uložit hello [azuredeploy.parameters.json soubor](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="4979e-115">Save hello [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour local machine.</span></span>
3. <span data-ttu-id="4979e-116">Upravit hello **azuredeploy.parameters.json** tooyour souboru preferované vstupy.</span><span class="sxs-lookup"><span data-stu-id="4979e-116">Edit hello **azuredeploy.parameters.json** file tooyour preferred inputs.</span></span>
4. <span data-ttu-id="4979e-117">Nasazení šablony hello s [nasazení skupiny az vytvořit] odkazující na hello stáhli soubor json:</span><span class="sxs-lookup"><span data-stu-id="4979e-117">Deploy hello template with [az group deployment create] referencing hello downloaded json file:</span></span>

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

<span data-ttu-id="4979e-118">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4979e-118">hello output is similar toohello following example:</span></span>

```json
{
"id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Resources/deployments/azuredeploy",
"name": "azuredeploy",
"properties": {
    "correlationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "debugSetting": null,
}
...
"provisioningState": "Succeeded",
"template": null,
"templateLink": {
    "contentVersion": "1.0.0.0",
    "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json"
    },
    "timestamp": "2017-02-22T00:05:51.860411+00:00"
},
"resourceGroup": "myResourceGroup"
}
```

<span data-ttu-id="4979e-119">Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="4979e-119">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="4979e-120">Pokud chcete, můžete ověřit instalaci hello přechod dolů příliš[ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="4979e-120">If you wish, you can verify hello install by jumping down too[Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="4979e-121">Nasazení svítilna na existující návod virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4979e-121">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="4979e-122">Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [sem toolearn jak toocreate virtuálního počítače s Linuxem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span><span class="sxs-lookup"><span data-stu-id="4979e-122">If you need help creating a Linux VM, you can head [here toolearn how toocreate a Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span></span> <span data-ttu-id="4979e-123">Dále musíte tooSSH do hello virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4979e-123">Next, you need tooSSH into hello Linux VM.</span></span> <span data-ttu-id="4979e-124">Pokud potřebujete pomoc s vytvářením klíč SSH, head [sem toolearn jak toocreate klíče SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4979e-124">If you need help with creating an SSH key, you can head [here toolearn how toocreate an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="4979e-125">Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="4979e-125">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="4979e-126">Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci hello svítilna zásobníku na základě Debian distribuce.</span><span class="sxs-lookup"><span data-stu-id="4979e-126">Now that you are working within your Linux VM, we can walk through installing hello LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="4979e-127">přesný příkazy Hello se mohou lišit pro ostatní distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="4979e-127">hello exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="4979e-128">Instalace na Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4979e-128">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="4979e-129">Budete potřebovat následující balíčky nainstalované hello: `apache2`, `mysql-server`, `php5`, a `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="4979e-129">You need hello following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="4979e-130">Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel.</span><span class="sxs-lookup"><span data-stu-id="4979e-130">You can install these packages by directly grabbing these packages or using Tasksel.</span></span>
<span data-ttu-id="4979e-131">Před instalací potřebovat toodownload a aktualizovat balíček seznamy.</span><span class="sxs-lookup"><span data-stu-id="4979e-131">Before installing you need toodownload and update package lists.</span></span>

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a><span data-ttu-id="4979e-132">Jednotlivé balíčky</span><span class="sxs-lookup"><span data-stu-id="4979e-132">Individual packages</span></span>
<span data-ttu-id="4979e-133">Pomocí výstižný get:</span><span class="sxs-lookup"><span data-stu-id="4979e-133">Using apt-get:</span></span>

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a><span data-ttu-id="4979e-134">Pomocí tasksel</span><span class="sxs-lookup"><span data-stu-id="4979e-134">Using tasksel</span></span>
<span data-ttu-id="4979e-135">Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="4979e-135">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

<span data-ttu-id="4979e-136">Po spuštění, buď z předchozích možností hello, budete mít výzvami tooinstall tyto balíčky a různé další závislosti.</span><span class="sxs-lookup"><span data-stu-id="4979e-136">After running either of hello previous options, you will be prompted tooinstall these packages and various other dependencies.</span></span> <span data-ttu-id="4979e-137">tooset heslo správce pro databázi MySQL, stiskněte "y" a potom toocontinue, zadejte"a budete postupovat podle dalších pokynů.</span><span class="sxs-lookup"><span data-stu-id="4979e-137">tooset an administrative password for MySQL, press 'y' and then 'Enter' toocontinue, and follow any other prompts.</span></span> <span data-ttu-id="4979e-138">Tento proces nainstaluje hello minimální požadované PHP rozšíření potřeby toouse PHP s MySQL.</span><span class="sxs-lookup"><span data-stu-id="4979e-138">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="4979e-139">Spusťte následující příkaz toosee hello další rozšíření PHP, které jsou k dispozici jako balíčky:</span><span class="sxs-lookup"><span data-stu-id="4979e-139">Run hello following command toosee other PHP extensions that are available as packages:</span></span>

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a><span data-ttu-id="4979e-140">Vytvoření dokumentu info.php</span><span class="sxs-lookup"><span data-stu-id="4979e-140">Create info.php document</span></span>
<span data-ttu-id="4979e-141">Teď byste měli být schopný toocheck Apache, MySQL a PHP, jaká verze máte prostřednictvím hello příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.</span><span class="sxs-lookup"><span data-stu-id="4979e-141">You should now be able toocheck what version of Apache, MySQL, and PHP you have through hello command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="4979e-142">Pokud by například tootest další, můžete vytvořit rychlé tooview PHP informace o stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="4979e-142">If you would like tootest further, you can create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="4979e-143">Vytvořte soubor s Nano textový editor se tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="4979e-143">Create a file with Nano text editor with this command:</span></span>

```bash
sudo nano /var/www/html/info.php
```

<span data-ttu-id="4979e-144">V rámci hello GNU Nano textový editor přidejte následující řádky hello:</span><span class="sxs-lookup"><span data-stu-id="4979e-144">Within hello GNU Nano text editor, add hello following lines:</span></span>

```php
<?php
phpinfo();
?>
```

<span data-ttu-id="4979e-145">Potom uložte a zavřete hello textový editor.</span><span class="sxs-lookup"><span data-stu-id="4979e-145">Then save and exit hello text editor.</span></span>

<span data-ttu-id="4979e-146">Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="4979e-146">Restart Apache with this command so all new installs take effect.</span></span>

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="4979e-147">Ověřte svítilna úspěšně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="4979e-147">Verify LAMP successfully installed</span></span>
<span data-ttu-id="4979e-148">Nyní můžete zkontrolovat, stránku s informacemi o PHP hello, kterou jste vytvořili otevřením prohlížeče a budete toohttp://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="4979e-148">Now you can check hello PHP info page you created by opening a browser and going toohttp://youruniqueDNS/info.php.</span></span> <span data-ttu-id="4979e-149">Měl by vypadat podobně jako toothis bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4979e-149">It should look similar toothis image.</span></span>

![][2]

<span data-ttu-id="4979e-150">Apache instalaci můžete zkontrolovat zobrazením hello Apache2 Ubuntu výchozí stránka přechodem tooyou http://youruniqueDNS/.</span><span class="sxs-lookup"><span data-stu-id="4979e-150">You can check your Apache installation by viewing hello Apache2 Ubuntu Default Page by going tooyou http://youruniqueDNS/.</span></span> <span data-ttu-id="4979e-151">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="4979e-151">hello output is similar toohello following example:</span></span>

![][3]

<span data-ttu-id="4979e-152">Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="4979e-152">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="4979e-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4979e-153">Next steps</span></span>
<span data-ttu-id="4979e-154">Projděte si hello Ubuntu dokumentace v zásobníku svítilna hello:</span><span class="sxs-lookup"><span data-stu-id="4979e-154">Check out hello Ubuntu documentation on hello LAMP stack:</span></span>

* [<span data-ttu-id="4979e-155">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="4979e-155">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
