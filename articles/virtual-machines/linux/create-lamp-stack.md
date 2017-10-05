---
title: "Nasazení svítilna na virtuální počítač s Linuxem v Azure | Microsoft Docs"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: ad69876bfbeba5f948a81e5c48c659fdf2265ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-on-azure"></a><span data-ttu-id="be8d7-103">Nasazení svítilna zásobníku v Azure</span><span class="sxs-lookup"><span data-stu-id="be8d7-103">Deploy LAMP stack on Azure</span></span>
<span data-ttu-id="be8d7-104">Tento článek vás provede procesem nasazení webového serveru Apache, MySQL a PHP (svítilna stack) v Azure.</span><span class="sxs-lookup"><span data-stu-id="be8d7-104">This article walks you through how to deploy an Apache web server, MySQL, and PHP (the LAMP stack) on Azure.</span></span> <span data-ttu-id="be8d7-105">Potřebujete účet Azure ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)) a [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="be8d7-105">You need an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and the [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span> <span data-ttu-id="be8d7-106">K provedení těchto kroků můžete také využít [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be8d7-106">You can also perform these steps with the [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="be8d7-107">Rychlý příkaz souhrn</span><span class="sxs-lookup"><span data-stu-id="be8d7-107">Quick command summary</span></span>

1. <span data-ttu-id="be8d7-108">Ukládání a upravování [azuredeploy.parameters.json souboru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) na vaši volbu na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="be8d7-108">Save and edit the [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) to your preference on your local machine.</span></span>
2. <span data-ttu-id="be8d7-109">Spuštěním následujících příkazů pro vytvoření skupiny prostředků a potom nasadíte šablony:</span><span class="sxs-lookup"><span data-stu-id="be8d7-109">Run the following two commands to create a resource group and then deploy your template:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a><span data-ttu-id="be8d7-110">Nasazení svítilna na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="be8d7-110">Deploy LAMP on existing VM</span></span>
<span data-ttu-id="be8d7-111">Následující příkazy balíčky aktualizací a poté nainstaluje Apache, MySQL a PHP:</span><span class="sxs-lookup"><span data-stu-id="be8d7-111">The following commands updates packages, then installs Apache, MySQL, and PHP:</span></span>

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="be8d7-112">Nasazení svítilna na nový virtuální počítač návod</span><span class="sxs-lookup"><span data-stu-id="be8d7-112">Deploy LAMP on new VM walkthrough</span></span>

1. <span data-ttu-id="be8d7-113">Vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create) tak, aby obsahovala nového virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="be8d7-113">Create a resource group with [az group create](/cli/azure/group#create) to contain the new VM:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
```
<span data-ttu-id="be8d7-114">Pokud chcete vytvořit virtuální počítač, můžete použít již napsané šablonu Azure Resource Manager [sem na Githubu](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="be8d7-114">To create the VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

2. <span data-ttu-id="be8d7-115">Uložit [azuredeploy.parameters.json soubor](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="be8d7-115">Save the [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) to your local machine.</span></span>
3. <span data-ttu-id="be8d7-116">Upravit **azuredeploy.parameters.json** soubor upřednostňované vstupy.</span><span class="sxs-lookup"><span data-stu-id="be8d7-116">Edit the **azuredeploy.parameters.json** file to your preferred inputs.</span></span>
4. <span data-ttu-id="be8d7-117">Nasazení šablony s [nasazení skupiny az vytvořit] odkazování na soubor stažený json:</span><span class="sxs-lookup"><span data-stu-id="be8d7-117">Deploy the template with [az group deployment create] referencing the downloaded json file:</span></span>

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

<span data-ttu-id="be8d7-118">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="be8d7-118">The output is similar to the following example:</span></span>

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

<span data-ttu-id="be8d7-119">Nyní jste vytvořili virtuální počítač s Linuxem se SVÍTILNOU již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="be8d7-119">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="be8d7-120">Pokud chcete, můžete ověřit instalaci přechod dolů na [ověřte svítilna úspěšně nainstaloval](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="be8d7-120">If you wish, you can verify the install by jumping down to [Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="be8d7-121">Nasazení svítilna na existující návod virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="be8d7-121">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="be8d7-122">Pokud potřebujete pomoc, vytvoření virtuálního počítače s Linuxem, head [zde se dozvíte, jak vytvořit virtuální počítač s Linuxem](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span><span class="sxs-lookup"><span data-stu-id="be8d7-122">If you need help creating a Linux VM, you can head [here to learn how to create a Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span></span> <span data-ttu-id="be8d7-123">Dále je třeba SSH do virtuálního počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="be8d7-123">Next, you need to SSH into the Linux VM.</span></span> <span data-ttu-id="be8d7-124">Pokud potřebujete pomoc s vytvářením klíč SSH, head [zde se dozvíte, jak vytvořit klíč SSH na Linux nebo Mac.](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be8d7-124">If you need help with creating an SSH key, you can head [here to learn how to create an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="be8d7-125">Pokud již máte klíč SSH, přejděte dopředu a SSH z příkazového řádku do virtuálním počítačům s Linuxem pomocí `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="be8d7-125">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="be8d7-126">Teď, když pracujete v rámci vašeho virtuálního počítače s Linuxem, jsme provede instalaci zásobníku svítilna na základě Debian distribuce.</span><span class="sxs-lookup"><span data-stu-id="be8d7-126">Now that you are working within your Linux VM, we can walk through installing the LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="be8d7-127">Přesný příkazy se mohou lišit pro ostatní distribucích systému Linux.</span><span class="sxs-lookup"><span data-stu-id="be8d7-127">The exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="be8d7-128">Instalace na Debian/Ubuntu</span><span class="sxs-lookup"><span data-stu-id="be8d7-128">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="be8d7-129">Budete potřebovat následující balíčky nainstalované: `apache2`, `mysql-server`, `php5`, a `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="be8d7-129">You need the following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="be8d7-130">Tyto balíčky můžete nainstalovat přímo metodou tyto balíčky nebo pomocí Tasksel.</span><span class="sxs-lookup"><span data-stu-id="be8d7-130">You can install these packages by directly grabbing these packages or using Tasksel.</span></span>
<span data-ttu-id="be8d7-131">Před instalací musíte stáhnout a aktualizovat balíček seznamy.</span><span class="sxs-lookup"><span data-stu-id="be8d7-131">Before installing you need to download and update package lists.</span></span>

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a><span data-ttu-id="be8d7-132">Jednotlivé balíčky</span><span class="sxs-lookup"><span data-stu-id="be8d7-132">Individual packages</span></span>
<span data-ttu-id="be8d7-133">Pomocí výstižný get:</span><span class="sxs-lookup"><span data-stu-id="be8d7-133">Using apt-get:</span></span>

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a><span data-ttu-id="be8d7-134">Pomocí tasksel</span><span class="sxs-lookup"><span data-stu-id="be8d7-134">Using tasksel</span></span>
<span data-ttu-id="be8d7-135">Případně si můžete stáhnout Tasksel, Debian/Ubuntu nástroj, který nainstaluje více souvisejících balíčků jako koordinované "úloha" vašeho systému.</span><span class="sxs-lookup"><span data-stu-id="be8d7-135">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

<span data-ttu-id="be8d7-136">Po spuštění některé z předchozích možností, zobrazí se výzva k instalaci těchto balíčků a různé další závislosti.</span><span class="sxs-lookup"><span data-stu-id="be8d7-136">After running either of the previous options, you will be prompted to install these packages and various other dependencies.</span></span> <span data-ttu-id="be8d7-137">Chcete-li nastavit heslo správce pro databázi MySQL, stiskněte "y" a potom "Enter, chcete pokračovat a budete postupovat podle dalších pokynů.</span><span class="sxs-lookup"><span data-stu-id="be8d7-137">To set an administrative password for MySQL, press 'y' and then 'Enter' to continue, and follow any other prompts.</span></span> <span data-ttu-id="be8d7-138">Tento proces nainstaluje minimální požadované rozšíření PHP potřeba k použití PHP s MySQL.</span><span class="sxs-lookup"><span data-stu-id="be8d7-138">This process installs the minimum required PHP extensions needed to use PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="be8d7-139">Spusťte následující příkaz, který najdete v části Další rozšíření PHP, které jsou k dispozici jako balíčky:</span><span class="sxs-lookup"><span data-stu-id="be8d7-139">Run the following command to see other PHP extensions that are available as packages:</span></span>

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a><span data-ttu-id="be8d7-140">Vytvoření dokumentu info.php</span><span class="sxs-lookup"><span data-stu-id="be8d7-140">Create info.php document</span></span>
<span data-ttu-id="be8d7-141">Nyní byste měli být schopna provést kontrolu, jaká verze Apache, MySQL a PHP, máte pomocí příkazového řádku zadáním `apache2 -v`, `mysql -v`, nebo `php -v`.</span><span class="sxs-lookup"><span data-stu-id="be8d7-141">You should now be able to check what version of Apache, MySQL, and PHP you have through the command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="be8d7-142">Pokud chcete otestovat další, můžete vytvořit rychlé PHP stránku s informacemi o zobrazíte v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="be8d7-142">If you would like to test further, you can create a quick PHP info page to view in a browser.</span></span> <span data-ttu-id="be8d7-143">Vytvořte soubor s Nano textový editor se tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="be8d7-143">Create a file with Nano text editor with this command:</span></span>

```bash
sudo nano /var/www/html/info.php
```

<span data-ttu-id="be8d7-144">V rámci licence GNU Nano textový editor přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="be8d7-144">Within the GNU Nano text editor, add the following lines:</span></span>

```php
<?php
phpinfo();
?>
```

<span data-ttu-id="be8d7-145">Potom uložte a ukončete textový editor.</span><span class="sxs-lookup"><span data-stu-id="be8d7-145">Then save and exit the text editor.</span></span>

<span data-ttu-id="be8d7-146">Restartujte Apache pomocí tohoto příkazu, takže všechny nové instalace vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="be8d7-146">Restart Apache with this command so all new installs take effect.</span></span>

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="be8d7-147">Ověřte svítilna úspěšně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="be8d7-147">Verify LAMP successfully installed</span></span>
<span data-ttu-id="be8d7-148">Nyní můžete zkontrolovat stránku s informacemi o PHP, kterou jste vytvořili otevřením prohlížeče a přejdete na http://youruniqueDNS/info.php.</span><span class="sxs-lookup"><span data-stu-id="be8d7-148">Now you can check the PHP info page you created by opening a browser and going to http://youruniqueDNS/info.php.</span></span> <span data-ttu-id="be8d7-149">By měla vypadat podobně jako tento obrázek.</span><span class="sxs-lookup"><span data-stu-id="be8d7-149">It should look similar to this image.</span></span>

![][2]

<span data-ttu-id="be8d7-150">Apache instalaci můžete zkontrolovat pomocí zobrazení stránky výchozí Ubuntu Apache2 přechodem na http://youruniqueDNS/ můžete.</span><span class="sxs-lookup"><span data-stu-id="be8d7-150">You can check your Apache installation by viewing the Apache2 Ubuntu Default Page by going to you http://youruniqueDNS/.</span></span> <span data-ttu-id="be8d7-151">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="be8d7-151">The output is similar to the following example:</span></span>

![][3]

<span data-ttu-id="be8d7-152">Blahopřejeme, jste nastavili jen svítilna zásobníku na vašem virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="be8d7-152">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="be8d7-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="be8d7-153">Next steps</span></span>
<span data-ttu-id="be8d7-154">Projděte si dokumentaci Ubuntu v zásobníku svítilna:</span><span class="sxs-lookup"><span data-stu-id="be8d7-154">Check out the Ubuntu documentation on the LAMP stack:</span></span>

* [<span data-ttu-id="be8d7-155">https://help.Ubuntu.com/community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="be8d7-155">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
